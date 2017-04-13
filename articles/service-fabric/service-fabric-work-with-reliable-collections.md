<properties
    pageTitle="Töötamine usaldusväärne saidikogumid | Microsoft Azure'i"
    description="Lugege heade tavade töötamiseks usaldusväärne saidikogumid."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Usaldusväärne saidikogumid töötamine

Teenuse struktuuri pakub stateful programmeerimise mudel saadaval .NET arendajatele usaldusväärne saidikogumid kaudu. Täpsemalt teenuse struktuuri pakub usaldusväärne sõnastik ja usaldusväärne järjekorda tunnid. Need tunnid kasutamisel on oleku liigendatud (jaoks skaleeritavus), kopeeritud (kättesaadavuse) jaoks ja saidi sektsioonis, (HAPPE semantika) jaoks. Vaatame tüüpilised kasutamist usaldusväärne sõnastiku objekti ja vaadata, millised selle tegelikult seda.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Kõik toimingud (välja arvatud ClearAsync ei saa tagasi võtta), usaldusväärseid sõnastiku objektide jaoks on vaja ITransaction objekti. See objekt on seostatud mõne ja kõik muudatused, mida proovite mis tahes usaldusväärne sõnastikku ja/või usaldusväärne järjekorda ühe sektsioonis objektid. Saate hankida mõne ITransaction objekti, helistades sektsiooni kasutaja StateManager's CreateTransaction meetod.
 
Ülaltoodud kood, edastatakse ITransaction objekti usaldusväärne sõnastiku AddAsync meetod. Sees, sõnastiku viise, mida aktsepteerib klahvi võtta lugeja/kirjutaja lukustamine, mis on seotud võti. Kui meetodit muudab võtme väärtuse, meetodit võtab kirjutamiseks lukustamine võti ja kui meetodit ainult loeb võtme väärtus, siis lugege Lukusta võetakse võti. Kuna AddAsync muudab võtme väärtuseks uus, edastatav väärtus, võetakse võtme kirjutamiseks lukustamine. Jah, kui 2 (või rohkem) Teemad proovivad sama võti samal ajal väärtuste liitmiseks, üks teema omandada kirjutamiseks lukustamine ja muud teemad blokeerib. Vaikimisi võimalust blokeerida kuni 4 sekundit omandada lukustamine; meetodite põhjustada 4 sekundi pärast soovitud TimeoutException. Meetod ülekoormuse olemas, saate läbida konkreetsete ajalõpp väärtuse, kui eelistate.
 
Tavaliselt, saate oma reageerida on TimeoutException see sattuda ja kogu toimingu uuesti proovimist (nagu on näidatud ülaltoodud kood), koodi kirjutamine. Minu lihtsa koodi helistan lihtsalt läbides 100 millisekundit iga kord, kui Task.Delay. Kuid tegelikult võib olla parem kasutada hoopis mingi eksponentsiaalse tagasi välja viivitust.
 
Kui lukustus omandatud, lisab AddAsync sisemise ajutine sõnastikku, mis on seotud ITransaction objekti objektiviidete võti ja väärtus. Seda tehakse lugemis-teie-omanik – kirjutab semantika pakkumiseks. Kui helistate AddAsync, hiljem kõne TryGetValueAsync (sama ITransaction objekti abil) tagastab väärtuse isegi juhul, kui te pole veel kinnitatud tehingu. Järgmiseks AddAsync serializes tootevõti ja byte massiivid vastu väärtuse ja lisab need byte massiivid logifaili kohaliku sõlme. Lõpuks AddAsync saadab byte massiivid teisene koopiad nii, et neil on sama teavet /-väärtuse. Ehkki /-väärtuse teave on kirjutatud logifaili, teave ei loeta sõnastiku osa seni, kuni tema tehing, mida nad on seostatud on kinnitatud. 

Ülaltoodud kood, kõne edastamiseks CommitAsync paneb kõik selle tehingu toiminguid. Täpsemalt lisab Kinnita teabe logifaili kohaliku sõlme ja saadab Kinnita kirje teisene koopiad. Kui otsustusvõimeline (enamik) kujundusmuudatusi ei vastanud, kõik andmed muudatuste püsivaks ja mis tahes lukud seostatud klahvid, mis olid manipuleeritud kaudu ITransaction objekt on välja nii, et muud teemad/tehingud saab töödelda sama võtmed ja nende väärtused.

Kui CommitAsync nimetatakse (tavaliselt on erand on) tõttu, siis ITransaction objekti saab kõrvaldada. Müügi sisseviimata ITransaction objekti, teenuse struktuuri lisab katkestamist teavet kohaliku sõlme logifailist ja midagi on vaja saata ühte teisene koopiad. Ja seejärel klõpsake mis tahes lukud seostatud klahvid, mis olid manipuleeritud kaudu tehingu lastakse.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Levinud vigu ja kuidas neid vältida
Nüüd, kui teil mõista, kuidas usaldusväärset saidikogumid töötavad ettevõttesiseselt, Vaatame mõned levinud prognoositavast ülevaade. Vaadake alltoodud kood:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Tavaline .NET sõnastiku töötades saate võtme/väärtuse lisamiseks sõnastikku ja muutke atribuut (nt LastLogin) väärtus. Siiski järgmine kood ei tööta õigesti usaldusväärne sõnastikku. Pidage meeles, et varasemates, kõne AddAsync serializes /-väärtuse objekte byte massiivid ja kohalik fail salvestab massiivides ja ka saadab teisene koopiad. Kui muudate hiljem atribuut, see muudab selle atribuudi väärtust mällu ainult; See ei mõjuta kohaliku faili või selle koopiad saadetud andmeid. Kui protsess jookseb, mis on mällu Expression.Error kaugusel. Uue käivitamisel või kui teine koopia muutub esmane, siis vana atribuudi väärtus on, mis on saadaval. 

Ma ei saa rõhutada piisavalt kui lihtne on teha sellist viga eespool näidatud. Ja ainult tutvustame teile viga kui toimub. Õige koodi kirjutamiseks on lihtsalt tagasi kaks rida:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Järgmisena veel üks näide, kus on kuvatud levinud viga:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Uuesti välistamissõnastike tavaline .net-i kood eespool toimib hästi ja on levinud muster: arendaja kasutab klahvi väärtuse otsimiseks. Kui väärtus on olemas, muudab arendaja on atribuudi väärtust. Usaldusväärseid saidikogumid, järgmine kood eksponeerib sama probleem, nagu juba arutada: __ei tohi muuta objekti, kui olete andnud selle usaldusväärse saidikogumi.__
 
Õige väärtuse, usaldusväärseid kogumi värskendamiseks on olemasolev väärtus saada ja kaaluge selle püsiv kaudselt viidatud objekti. Looge uus objekti, mis on täpne koopia Algne objekt. Nüüd saate muuta selle uuele objektile ja kirjutage uuele objektile kollektsiooni nii, et see saab seeriasertide byte massiivid, et lisatud kohaliku faili ja saata selle koopiad. Pärast soovitud muutused toime, on mälu-objektid, kohaliku faili ja kõigi koopiate täpse muutmata. Kõik on hea!

Alljärgnev kood kuvatakse õige väärtuse usaldusväärne saidikogumi värskendamine:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Määratlege püsiv andmetüübid vältimiseks programmeerija tõrge

Ideaalvariandis mitte soovime koostaja aruande vigu kogemata tootes muteerub objekti, mis teil peaks kaaluma püsiv riigi kood. Kuid C# koostaja ei ole võimalus tehke järgmist. Nii, et võimalikud programmeerija vigade vältimiseks soovitame määratleda failitüübid kasutamisel püsiv tüüpi olevat usaldusväärne saidikogumid. Täpsemalt, see tähendab, et teil jääda core väärtus tüüpi (nt arvude [Int32, UInt64 jne], kuupäev ja kellaaeg, Guid, kuuline ajavahemik ja jms). Ja muidugi, saate kasutada ka String. See on parim vältida Saidikogumi atribuudid nimega jaotamine ja deserializing neid saab sageli saate haiget jõudlust. Juhul, kui soovite kasutada saidikogumi atribuute, soovitame kasutada. NET püsiv saidikogumid Raamatukogu (System.Collections.Immutable). See teek on http://nuget.org allalaadimiseks saadaval. Soovitame tihendus oma tunde ja teha väljad kirjutuskaitstud ainult juhul, kui vähegi võimalik.

Allpool UserInfo tüüp näitab, kuidas määrata mõne eelnimetatud soovitused ära püsiv tüüp.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

ItemId tüüp on püsiv tüüpi, nagu järgmisel joonisel:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Skeemi versioonimist (uuendamine)

Sees, usaldusväärne saidikogumid serialiseerida oma objektide abil. NET DataContractSerializer. Sarjadesse jaotatud objektid on jätkunud peamine koopia kohalikule kettale ja edastatakse samuti teisene koopiad. Kui teie teenuse tagasimakse tähtaeg, tõenäoliselt soovite muuta, millised andmed (skeem) teie teenuse jaoks on vaja. Peate pöörduma versioonimise andmete hoolikalt. Kõigepealt peate olema alati võimalus andmeatribuutide vanu andmeid. Täpsemalt, see tähendab, et teie vahemäluasukohaga tähis peab olema lõputult tagasiühilduvad: versiooni 333 oma teenuse kood peate saama toimivad andmed paigutatakse usaldusväärne saidikogumi oma teenuse kood version 1 5 aastat tagasi.

Lisaks on teenuse kood täiendatud üks versiooniuuenduse domeeni korraga. Nii, versiooniuuenduse on kaks erinevate versioonide oma teenuse kood töötab samal ajal. Teil tuleb vältida uue versiooni oma teenuse kood ei pruugi teie teenuse kood vanemad versioonid käsitlema uue skeemi uue skeemi kasutada. Kui võimalik, peaksite kujundama oma teenuse Esita ühilduvad 1 versioon, et iga versioon. Täpsemalt, see tähendab, et V1 oma teenuse kood, peaks saama lihtsalt ignoreerida kõik konkreetselt ei oska skeemi elemente. Siiski tuleb salvestada see konkreetselt ei tea andmed ja lihtsalt täitmine selle tagasi välja värskendamisel sõnastiku võtme või väärtuse. 

>[AZURE.WARNING] Ajal klahvi skeemi muutmiseks peab tagate oma key kood ja võrdub algoritmide kohta on ühed. Kui soovite muuta, kuidas ühte nende algoritmide toimida, ei saa otsida sees, usaldusväärseid sõnastiku võti kunagi.
  
Teise võimalusena saate teha, mis on tavaliselt edaspidi 2-etapp versioonitäiendus. 2-etapp uuele versioonile, kus täiendate oma teenuse V1 V2: V2 sisaldab koodi mis oskab uue skeemi muutmine tegelema, kuid see kood ei käivitada. Kui V2 koodi loeb V1 andmeid, see toimib ja kirjutab V1 andmeid. Seejärel pärast uuele versioonile üle kõik täiendamine domeenide te saate kuidagi märku töötava V2 eksemplarides versioonitäienduse lõpulejõudmist. (Üks võimalus signaal see on hakkama konfigureerimine versiooniuuenduse; see on, mis teeb selle etapi 2 täiendamine.) V2 eksemplari saate nüüd V1 andmeid lugeda, teisendamine V2 andmed, töötada ja kirjutage välja V2 andmetena. Kui muudel juhtudel V2 andmeid lugeda, ei pea seda teisendada, need lihtsalt töötada ja kirjutage V2 andmed.

## <a name="next-steps"></a>Järgmised sammud
Ühilduvad andmete lepingute loomise kohta leiate lisateavet teemast [Edasiühilduvad andmete lepingud](https://msdn.microsoft.com/library/ms731083.aspx).

Head tavad versioonimise andmete lepingute kohta leiate teemast [Andmete lepingu Versioonimine](https://msdn.microsoft.com/library/ms731138.aspx). 

Rakendada versioon salliv andmete lepingute kohta leiate teavet teemast [Versioon salliv sariväljaanne kontekstiatribuuti](https://msdn.microsoft.com/library/ms733734.aspx). 

Andmete struktuur, mis võib olla nendega üle mitme versiooni kohta leiate teavet teemast [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
