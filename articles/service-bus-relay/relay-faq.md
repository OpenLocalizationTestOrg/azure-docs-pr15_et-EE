<properties 
    pageTitle="Relee KKK | Microsoft Azure'i"
    description="Vastused mõned korduma kippuvad küsimused Azure'i Relay."
    services="service-bus"
    documentationCenter="na"
    authors="jtaubensee"
    manager=""
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub" />

# <a name="relay-faq"></a>Relay KKK

Sellest artiklist leiate vastused mõned korduma kippuvad küsimused Microsoft Azure'i Relay. Leiate [Azure'i toe KKK](http://go.microsoft.com/fwlink/?LinkID=185083) üldteavet Azure hinnad ja tugi. Järgmistes teemades on kaasatud.

- [Üldised küsimused Azure'i edastamine](#general-questions)
- [Hinnad](#pricing)
- [Kvoote](#quotas)
- [Tellimuse ja nimeruum haldus](#subscription-and-namespace-management)
- [Tõrkeotsing](#troubleshooting)

## <a name="general-questions"></a>Üldised küsimused

### <a name="what-is-azure-relay"></a>Mis on Azure Relay?

[Vahendusteenusesse](relay-what-is-it.md) pakub läbipaistev majutada ja WCF teenused igalt poolt juurde pääsemiseks. Teisisõnu, see võimaldab hübriid rakendusi, mis töötavad nii on Azure andmekeskuse ja kohapealse ettevõtte keskkonnas.

### <a name="what-is-a-relay-namespace"></a>Mis on Relay nimeruum?

[Nimeruumi](Relay-create-namespace-portal.md) pakub ulatuse container Relay ressursse rakenduse tegelemiseks. Üks loomise on vaja kasutada edastamine ja üks esimesed sammud alustamine.

## <a name="pricing"></a>Hinnad

Selles jaotises vastused teatud edastamine, hinnad struktuuri kohta korduma kippuvad küsimused. Leiate [Azure'i tugi KKK](http://go.microsoft.com/fwlink/?LinkID=185083) üldteavet Microsoft Azure'i hinnakirjad. Lisateavet Relay hinnad, lugege teemat [teenuse siini hinnad üksikasjad](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-relay"></a>Kuidas te eest edastamine

Lisateavet Relay hinnad, lugege [teenuse siini hinnad üksikasjad][Pricing overview]. Lisaks märkida hinnad on seotud andmeedastuse jaoks sealt, kus teie rakendus on ette valmistatud andmekeskuse väljaspool eest.

### <a name="what-usage-of-relay-is-subject-to-data-transfer"></a>Millist kasutamist Relay kehtib andmeedastus?

Relay sisaldab 5 GB kohta kuus tellimuse andmed sissepääsu. On täiendavaid Azure sissepääsu/sealt tasuta andmete edastamine kasutavad.

Andmete edastamine tasu on mõeldud ainult saatjatelt sissepääsu, nimega edastamine kuulajatele kanna andmed tasuta. Näiteks kui saadate 1 GB, saate ainult, saadetakse teile arve 1 GB, kuigi on kuulajale ka saanud 1 GB ja võib olla väljaspool Azure andmekeskuste.

### <a name="how-are-relay-hours-calculated"></a>Kuidas arvutatakse Relay tundi?

Aja jooksul iga edastamine on "Ava" käigus antud arveldusperioodi kumulatiivne summa on arve edastamine tundi. Mõne edastamine on peidetult käivitatud ja avada antud nimeruum Relay kuulajale esmalt ühendub see aadress. Funktsiooni edastamine on suletud ainult siis, kui viimase kuulajale katkestatakse ühendus oma meiliaadress. Seetõttu on Relay käsitletakse "Ava" hetkest arvete koostamiseks esimese Relay kuulajale ühendab, viimase Relay kuulajale katkestatakse nimeruumi aeg. Teisisõnu on Relay käsitletakse avatud iga kord, kui üks või rohkem edastamine kuulajatele on ühendatud selle teenuse siini aadress.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-given-relay"></a>Mida teha, kui mul on mitu kuulajale teatud edastamine ühendatud?

Mõnel juhul ühe Relay võib olla mitu ühendatud kuulajatele. Mõne Relay käsitletakse "Ava", kui see on ühendatud vähemalt üks Relay kuulajale. Täiendavad kuulajatele lisamine avatud edastus tulemuseks täiendavad relay tundi. Ühendatud Relay saatjad (kliendid, mida autonoomsest või sõnumeid saata releed) arv on Relay ka Relay tundi arvutus ei mõjuta.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Kuidas arvutatakse sõnumite arvesti WCF-i releed?

**See kehtib ainult WCF-i releed ja ei ole kogukulu hübriid ühendused**

Üldiselt arvutatakse arveldatavate sõnumite releed vahendatud üksuste (järjekorrad, teemasid ja tellimused) eespool kirjeldatud meetodiga. Siiski on mitu poolest:

Saata teate teenuse siini Relay käsitletakse nagu "täielik kaudu" saada Relay kuulajale, mis saab sõnumi, mitte saada teenuse siini Relay järgneb Relay kuulajale tarne. Seetõttu on kutse – vasta laadi teenuse kutsumise (kuni 64 KB) suhtes Relay kuulajale tulemuseks kaks arveldatavate sõnumite: ühe arveldatavate sõnumi taotlus ja ühe arveldatavate sõnumi vastuse (eeldades, et vastus on ka \<= 64 KB). See erineb järjekorda abil olla vahendajaks klient ja teenuse vahel. Viimasel juhul nõuaks sama kutse – vasta muster taotluse järjekorda, järgneb dequeue/kohaletoimetamine kuhjuda kaudu teenusega teise järjekorda, millele järgneb vastuse saatmine ja dequeue/kohaletoimetamine, et järjekorda kliendile saata. Kasutades sama (\<= 64 KB) suurus oletused kogu, vahendusel järjekorda mustri seega tekitaks neli arveldatavate sõnumite kaks korda rakendada sama muster abil Relay arve number. Muidugi on kasu saavutada seda mudelit, nt kestvus ja laadimine ühtlustamise järjekorrad abil. Järgmisi eeliseid võib olla aluseks täiendavad kulud.

Avatud abil netTCPRelay WCF-i sidumine releed kohelge sõnumite üksikuid sõnumeid, vaid läbib süsteemi andmevoo. Teisisõnu, ainult saatja ja kuulajale on nähtavus raamimiseks üksikute sõnumite saadetud ja vastu võetud, kasutades selle sidumine. Seetõttu käsitletakse jaoks releed netTCPRelay sidumine abil, kõik andmed voo arveldatavate sõnumite arvutamiseks. Sel juhul arvutab teenuse siini andmete kogusumma saadetud või vastu võetud rakenduses iga üksiku Relay 5-minutilise alusel ja jagage määramiseks arveldatavate sõnumite jaoks selle aja jooksul kõnealuse Relay 64 KB.

## <a name="quotas"></a>Kvoote

|Kvoodi nimi|Ulatus|Tüüp|Ületanud käitumine|Väärtus|
|---|---|---|---|---|
|Klõpsake soovitud relay samaaegseid kuulajatele|Üksuse|Staatiline|Edaspidised taotlused täiendavate ühenduste on tagasi lükatud ja erandi vastu helistaja koodi.|25|
|Samaaegseid edastamine kuulajatele|Kogu süsteemi|Staatiline|Edaspidised taotlused täiendavate ühenduste on tagasi lükatud ja erandi vastu helistaja koodi.|2000|
|Samaaegseid relay ühendused kõik teenuse nimeruumi relay lõpp-punktid kohta|Kogu süsteemi|Staatiline|-|5000|
|Relay lõpp-punktid teenuse nimeruumi kohta.|Kogu süsteemi|Staatiline|-|10 000|
|Sõnumi maht [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) ja [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) edastab|Kogu süsteemi|Staatiline|Sissetulevate sõnumite, mis ületavad kvootide on tagasi lükatud ja erandi vastu helistaja koodi.|64KB
|Sõnumi maht [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) ja [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) edastab|Kogu süsteemi|Staatiline|-|Piiramatu|

### <a name="does-relay-have-any-usage-quotas"></a>Kas edastamine on mis tahes piirmäärasid?

Vaikimisi määrab mis tahes Cloud teenus Microsoft liitväärtuse kuu kasutuse piirmäära, mis on arvutatud kõigis kliendi tellimused. Kuna me mõista, et peate võib-olla rohkem kui need piirangud, pöörduge klienditeeninduse igal ajal, et saaksime mõista teie vajadustele ja kohandada need piirangud õigesti. Teenuse siini, liitväärtuse kasutus kvootide on järgmised:

- 5 miljardit sõnumid
- 2 miljonit Relay tundi

Ajal meil õigus keelamiseks kliendikonto, mis on ületanud oma piirmäärasid vastava kuu jooksul, me esitada e-posti teate ja võtta ühendust enne mistahes mitme katsete teha. Klientide üle nende kvootide ikkagi mis ületavad kvootide kulude eest vastutav.

#### <a name="naming-restrictions"></a>Nime andmise piirangud

Relay nimeruumi nime saab ainult vahel 6-50 märki.

## <a name="subscription-and-namespace-management"></a>Tellimuse ja nimeruum haldus

### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Kuidas migreerida nimeruumi teise Azure tellimust?

Saate kasutada PowerShelli käske (leida selle artikli [allpool](../service-bus-messaging/service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription)) teisaldamiseks nimeruumi ühe Azure tellimuselt teisele. Selle toimingu täitmiseks nimeruumi juba peab olema aktiivne. Ka käivitamisel käske kasutaja peab olema nii lähte- ja tellimuste administraator.

## <a name="troubleshooting"></a>Tõrkeotsing

### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-their-suggested-actions"></a>Millised on mõned erandid Azure'i Relay API-d ja nende soovitatud toimingud?

[Relay erandid] [ Relay exceptions] artiklis kirjeldatakse soovitatud toimingute mõned erandid.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Mis on jagatud juurdepääsu signatuuri ja milliseid keeli toetab signatuuri luua?

Ühiskasutusega juurdepääsu allkirjad on autentimise süsteem, mis põhineb SHA – 256 secure hashes või URI-d. Lisateavet selle kohta, kuidas luua oma allkirjad sõlm, PHP, Java ja C\#, artiklist [Ühiskasutusse Accessi allkirjad][] .

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Ühiskasutusega juurdepääsu allkirjad]: service-bus-sas-overview.md

## <a name="next-steps"></a>Järgmised toimingud:

- [Nimeruumi loomine](relay-create-namespace-portal.md)
- [.Net-i kasutamise alustamine](relay-hybrid-connections-dotnet-get-started.md)
- [Alustamine sõlm](relay-hybrid-connections-node-get-started.md)