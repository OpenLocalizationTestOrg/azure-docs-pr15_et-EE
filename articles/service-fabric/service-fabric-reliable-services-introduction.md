<properties
   pageTitle="Ülevaade ja struktuuri usaldusväärne teenus | Microsoft Azure'i"
   description="Lisateavet teenuse struktuuri töökindlat programmeerimise mudeli ja alustada kirjalikult oma teenuste."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Usaldusväärne teenuste ülevaade
Azure teenuse struktuuri lihtsustab kirjutamine ja haldamise kodakondsuseta ja stateful usaldusväärsed teenused. Selle dokumendi räägivad:

- Usaldusväärsed teenused programmeerimise mudel kodakondsuseta ja stateful teenuste jaoks.
- Valikud, mida peate tegema kirjutamise ajal töökindlat.
- Mõned stsenaariumid ja millal kasutada usaldusväärsed teenused ja kuidas need on kirjutatud näited.

Usaldusväärsed teenused on saadaval, klõpsake teenuse struktuuri programmeerimise näidiseid. Usaldusväärne osalejate programmeerimise mudeli kohta leiate lisateavet teemast [teenuse struktuuri usaldusväärne osalejate tutvustus](service-fabric-reliable-actors-introduction.md).

Teenuse struktuuri, teenuse koosneb rakenduse koodi, konfigureerimise ja maakond (soovi korral).

Teenuse struktuuri haldab teenused eluiga ettevalmistamise ja juurutamise versioonitäiendus ja kustutamise kaudu [teenuse struktuuri Rakendusehaldus](service-fabric-deploy-remove-applications.md)kaudu.

## <a name="what-are-reliable-services"></a>Mis on usaldusväärsed teenused?
Usaldusväärsed teenused annab teile lihtne, võimas, ülataseme programmeerimise mudeli saate avaldada, mis on olulised rakenduse. Mudeli programmeerimisega usaldusväärne teenustega saate teha järgmist:

- Stateful teenuste, usaldusväärseid teenuseid programmeerimise mudel võimaldab teil süsteemne ja usaldusväärselt talletada oma olekus otse oma teenuse abil usaldusväärne saidikogumid. See on väga saadaval saidikogumi tunnid, et igaüks, kes on kasutanud C# saidikogumid tunneb lihtsate. Traditsiooniliselt teenuseid vaja välised süsteemid usaldusväärne oleku haldamise. Usaldusväärne saidikogumid, kus saate talletada oleku kõrval teie Arvuta sama suur-saadavus ja te olete harjunud väga saadaval välise poed töökindluse ja täiendavad latentsus täiustused, varustavate koostööd asukoha Arvuta ja olek.

- Lihtsa andmemudeli töötab oma kood, mis näeb programmeerimise mudelite abil. Teie kood on täpselt määratletud sisendpunkti ja hõlpsasti hallatava elutsükli.

- Ühendatav side mudel. Kasutage transport teie valitud, nt HTTP [Veebiteenuste](service-fabric-reliable-services-communication-webapi.md), WebSockets, kohandatud TCP protokollid jne. Mõned suured out-of-box suvandid saate kasutada või saate sisestada oma pakuvad usaldusväärsed teenused.

## <a name="what-makes-reliable-services-different"></a>Milline usaldusväärsed teenused erinevad?
Usaldusväärne teenused teenuse struktuuri erineb teenused, mida olete kirjutanud enne. Teenuse struktuuri pakub töökindlust, kättesaadavus, järjepidevus ja skaleeritavus.  

- **Töökindluse**--teenust jäävad üles isegi usaldusväärsed keskkonnas, kus teie masinad võib nurjuda või tabas võrguprobleemide.

- **Kättesaadavus**--teenust on kättesaadav ja reageeri. (See ei tähenda, et see ei saa olla teenused, mida ei saa leitud või jõudis väljaspool.)

- **Skaleeritavus**--teenused on sidumata teatud riistvara ja nad saavad kasvata või Kahanda vastavalt vajadusele lisamise või eemaldamisega riistvara või virtuaalse ressursside kaudu. Teenuste hõlpsalt liigendatud (eriti stateful korral), sõltumatu osade teenuse mastaapimiseks ja vastata tõrkeid sõltumatult. Lõpuks teenuse struktuuri soovitab olla kerge lubades tuhandeliste olema ette valmistatud ühe protsessis teenuste, mitte nõudva või pühendades kogu OS eksemplarid kindla töökoormus ühekordsest services.

- **Vormindusühtsuse**– kõik andmed, salvestatakse see teenus tagatud ühtlustamist (kehtib ainult stateful teenuste - enam seda hiljem)

## <a name="service-lifecycle"></a>Teenuse elutsükkel
Kas teenust on stateful või kodakondsuseta, usaldusväärseid teenuseid pakuvad lihtsa elutsükli, mille abil saate kiiresti ühendage koodi ja saategi alustada.  On ainult ühe või kahe viise, mida soovite rakendada, et saada teenust ja töötab.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** – see on, kus teenuse määratleb side virnas, mida ta soovib kasutada. Side virnas, nt [Veebi-API](service-fabric-reliable-services-communication-webapi.md), on, mis määratleb kuulamise lõpp-punkti või (kuidas kliendid jõuavad selle) teenuse lõpp-punktid. Samuti määratletakse kuidas sattuda teenuse kood ülejäänud suheldes kuvatavate teadete.

- **RunAsync** – see on, kus töötab teenust oma äriloogika. Loobumise luba, mis on esitatud on signaal Millal peaks peatama töötavad. Näiteks kui teil on teenus, mis tuleb pidevalt sõnumite usaldusväärne järjekorda välja tõmmata ja töödelda, on see, kus töötavad võib juhtuda.

### <a name="service-startup"></a>Teenuse käivitamine

Peamised sündmused töökindlat elutsükli on:

1. Ehitatakse teenuse objekti (asi, mis tuleneb kodakondsuseta teenuse või stateful teenus).

2. Funktsiooni `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` meetodit nimetatakse, andes teenuse võimaluse tagastavad ühe või mitme side kuulajatele enda valitud.
  - Pange tähele, et see pole kohustuslik, kuigi enamik teenuste seada teatud lõpp-punkti otse.

3. Kui side kuulajatele on loodud, on avatud.
  - Suhtlus kuulajatele on meetodit nimetatakse `OpenAsync()`, mida nimetatakse sel hetkel ja mis annab teenuse kuulamise aadressi. Kui usaldusväärne teenust kasutab mõnda sisseehitatud ICommunicationListeners, käsitletakse seda teie eest.

4. Kui side kuulajale on avatud, kuvatakse `RunAsync()` peamised teenuse meetodit nimetatakse.
  - Pange tähele, et `RunAsync()` pole kohustuslik. Kui teenus on kõik selle töö otse kasutaja vastuseks ainult kõned, ei ole vaja seda rakendada `RunAsync()`.

### <a name="service-shutdown"></a>Teenuse sulgemine

Kui teenus suletakse (välja jätta, uuele versioonile üle viidud, või on teisaldatud) peegelpildis kõne järjestuses: esmalt loobumise luba hoiab `RunAsync()` tühistatakse; seejärel `CloseAsync()` side kuulajatele kutsutud.

On mõned olulised asjad märkida sulgumist stateful teenuste kohta.

- Teenuse struktuuri ei või reklaamida oma teenuse esmane oleku määramine, kuni teine koopia `CloseAsync` ja `RunAsync` on tagasi. Kui kasutate sisseehitatud side kuulajale, on `CloseAsync` meetod käsitletakse teie eest.

- Kuigi ei ole tähtaja naasnud järgmised võimalused, kohe kaotada võimalus kirjutamise usaldusväärne saidikogumite ja seetõttu ei saa lõpule viia, mis tahes tegelik töö. Soovitatav on nii kiiresti kui võimalik tühistamise taotluse taastada.

## <a name="example-services"></a>Näide teenused
See programmeerimise mudel teab, vaatame pilgu näha, kuidas need andmeühikuga sobivad koos kahe erinevad teenused.

### <a name="stateless-reliable-services"></a>Kodakondsuseta usaldusväärsed teenused
Kodakondsuseta teenus on üks koht, kus on sõna-sõnalt pole säilitada teenuse olek või olekusse, mis on täiesti ühekordselt ja ei nõua sünkroonimise, kopeerimine, püsimine või kõrge kättesaadavus.

Näiteks kaaluge kalkulaator, mis pole mälu ja võtab vastu kõik tingimuste ja toimingute sooritamiseks korraga.

Sel juhul teenuse RunAsync() võib olla tühi, sest puudub tausta-töötlemine mis teenuse tuleb teha. Kui kalkulaator teenus on loodud, see on ICommunicationListener (nt [Veebi-API](service-fabric-reliable-services-communication-webapi.md)) kuulamise lõpp-punkti mõned Port avanenud tagasi. Selle kuulamise lõpp-punkti saab ühenduse luua mitmel viisil (näide: "Lisa (n1, n2)") mis määratlevad selle kalkulaator avaliku API.

Kui on esitatud klient, sobiva meetodi kutsumisel ja teenuse kalkulaator tehete sisalduvate andmete ja tagastab tulemi. See ei säilita riik.

Selles näites kalkulaator ei talletamise mis tahes sisemise oleku teeb väga lihtne. Kuid enamiku teenuste pole tõeliselt kodakondsuseta. Selle asemel ükskikule nad oma oleku mõni muu pood. (Nt mis tahes web appi, mis sõltub säilitamise seansi olek varundamise poe või vahemälu pole täielikult kodakondsuseta.)

Näide kodakondsuseta teenuste kasutamisest rakenduses teenuse struktuuri on ees, mis seab avaliku API veebirakenduse. Seejärel räägib ees teenuse stateful teenuste kasutaja taotluse lõpuleviimiseks. Sel juhul klientide kõned suunatakse teadaolevad port, nt 80, kus kodakondsuseta teenus on kuulamine. See kodakondsuseta teenus saab kõne ja määrab, kas kõne on usaldusväärne tootja, kui ka nimega mis teenuse need on määratud.  Seejärel kodakondsuseta teenuse edastab kõne õige sektsiooni stateful teenuse ja ootab vastust. Kui kodakondsuseta teenuse saab vastust, see vastuste algse kliendile.

### <a name="stateful-reliable-services"></a>Stateful usaldusväärsed teenused
Stateful teenus on üks, mis peab olema mingi osa hoida funktsiooni järjepidevaid ja kohal teenuse olek. Kaaluge võimalust teenus, mis arvutab pidevalt jooksva keskmise mõne väärtuse alusel värskendusi saab. Selle tegemiseks peab olema praeguse komplekti, sissetulevad taotlused tuleb protsessi ning Keskmine. Teenus, mis toob, töötleb ja salvestab teabe (nt Azure'i bloobimälu või tabeli poest täna) välise poes on stateful. Lihtsalt hoiab seisu poes välise olekus.

Enamiku teenuste täna laoseisu oma väliselt, kuna välise pood on, mis pakub töökindlust, kättesaadavus, skaleeritavus ja järjepidevuse riigi. Teenuse struktuuri, stateful teenuste pole vaja oma laoseisu väliselt; Teenuse struktuuri hoolitseb järgmisi nõudeid nii teenuse kood ja teenuse olek.

Oletame, et soovime teenus, mis suunab taotlused sarja teisendused, mida tuleb teha pilt ning pilt, mida tuleb ümber kirjutada.  Selle teenuse see return side kuulajale (Oletame, et veebi-API) avab side pordi ja võimaldab edastuste kaudu API nagu `ConvertImage(Image i, IList<Conversion> conversions)`. See API teenuse võib teabe talletada taotluse järjekorda usaldusväärsest ja seejärel tagastab mõned luba kliendile nii, et see võib silma peal taotluse (Kuna taotlused võib võtta aega).

See teenus võiks keerukamaid RunAsync. Teenus võiks olla esitatavaks sees oma RunAsync, mis tõmbab taotlusi IReliableQueue välja, teeb teisendused, mis on loetletud ja talletab tulemused IReliableDictionary, nii, et kui kliendi tagasi, nad saavad oma teisendatud pilte. Tagada, et ka siis, kui midagi ebaõnnestub pilt pole kadunud, see usaldusväärne teenus oleks tõmmake järjekorda, on teisendada ja salvestada toimingu. Sel juhul sõnum tegelikult eemaldatakse ainult kuhjuda ja tulemid on talletatud tulemi sõnastiku, kui selle dokumenditeisenduste on lõpule viidud. Kui midagi ei keskel (nt seade töötab eksemplari kood), jääb taotluse järjekorda uuesti töötlemise ootel.

Selle teenuse kohta märkida te102827792ei kõlab tavaline .net-i teenus. Ainult erinevus on, et kasutusel kasutatavad andmestruktuurid (IReliableQueue ja IReliableDictionary) on esitatud teenuse struktuuri ja seega on tehtud väga usaldusväärne, saadaval ja ühtsete.

## <a name="when-to-use-reliable-services-apis"></a>Millal kasutada usaldusväärne teenuste API-d
Kui ühte järgmistest vabadusastmeid oma rakenduse vajadustele, siis kaaluge usaldusväärne teenuste API:

- Tuleb sisestada rakenduse käitumine üle mitu üksuste riigi (nt tellimused ja tellimuse kirjeid).

- Rakenduse olekus võib loomulikult modelleerida nagu usaldusväärne sõnastikud ja järjekorrad.

- Oma riigi peab olema väga kättesaadav madal latentsus juurdepääsu.

- Rakenduse peab kontrollima kokkulangevus või granulaarsus tehingu toimingute üle ühe või mitme usaldusväärse saidikogumid.

- Soovite hallata teatisi või määrata eraldamine kava teie teenuse jaoks.

- Teie kood peab käitusaja-keermestatud keskkonnas.

- Rakenduse peab dünaamiliselt luua või hävitada usaldusväärne sõnastikud või järjekordade käitusajal.

- Peate programmiliselt kontrollida esitatud teenuse struktuuri varundus ja taastamine teie teenuse olek * funktsioone.

- Rakenduse peab oma osakud olekus * muutuste ajalugu säilitada.

- Soovite töötada või tarbimine kolmandate osapoolte väljatöötatud ja kohandatud olekus pakkujate *.

> [AZURE.NOTE] * Funktsioonid, mis on saadaval veebisaidil SDK üldiselt kättesaadav.


## <a name="next-steps"></a>Järgmised sammud
+ [Usaldusväärne teenuste Lühijuhend](service-fabric-reliable-services-quick-start.md)
+ [Täpsemad kasutus usaldusväärsed teenused](service-fabric-reliable-services-advanced-usage.md)
+ [Usaldusväärne osalejate programmeerimise mudel](service-fabric-reliable-actors-introduction.md)
