<properties 
    pageTitle="Teenuse siini rakenduste katkestuste eest katastroofide soojustamine | Microsoft Azure'i"
    description="Kirjeldatakse tehnikate abil saate rakenduste võimalikud teenuse siini katkestuste eest kaitsta."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Rakenduste teenuse siini katkestuste eest katastroofide soojustamine head tavad

Olulise rakenduste peab töötama pidevalt, isegi kui on olemas planeerimata katkestuste või katastroofide. Selles teemas kirjeldatakse tehnikate abil saate kaitsta teenuse siini rakenduste võimalikud teenuse katkestuste või tõttu suhtes.

Mõne katkestuste on määratletud Azure'i teenus siini ajutiselt kättesaadavad. Funktsiooni katkestuste võib mõjutada teatud komponendid teenuse siini, nt sõnumside poe või isegi kogu andmekeskuse. Kui probleem on lahendatud, teenuse siini muutub kättesaadavaks uuesti. Tavaliselt on katkestuste ei põhjusta sõnumeid või muid andmeid. Kindla sõnumside poe puudumist on näiteks komponendi tõrge. Kogu andmekeskuse katkestuste näide on power tõrge andmekeskuse või vigase andmekeskuse võrgu aktiveerimine. Mõne katkestuste saate kesta mõne päeva paar minutit.

Katastroof on määratletud jäädavalt kadumist või teenuse siini skaala ühik andmekeskuse. Andmekeskuse võib või ei tohi olla saadaval uuesti. Tavaliselt katastroof põhjustab mõne või kõigi sõnumite või muid andmeid. Katastroofid on fire, üleujutus või maavärin.

## <a name="current-architecture"></a>Praeguse arhitektuur

Teenuse siini kasutab mitme kiirsõnumside poed järjekorrad või teemade saadetavate sõnumite talletamiseks. Liigendatud järjekorda või teema on määratud üks sõnumside poe. Kui see sõnumside poe pole saadaval, ei õnnestu kõik toimingud, et järjekorda või teema.

Kõigi teenuste siini sõnumside üksuste (järjekorrad, teemade, releed) asuda teenuse nimeruumi, mis on sidusettevõte andmekeskuses. Teenuse siini ei luba automaatne geo dispersioonanalüüs andmete ega võimalda nimeruumi jaotamiseks mitmes andmekeskuses.

## <a name="protecting-against-acs-outages"></a>ACS katkestuste eest kaitsmine

Kui kasutate ACS identimisteabe ja ACS pole saadaval, saate kliendid enam hankida sõned. Kliendid, mis on märgiks ACS läheb alla ajal saate jätkata teenuse siini kuni märkide aeguda. Sümboolne eluiga vaikesäte on 3 tundi.

ACS katkestuste eest kaitsmiseks kasutada ühiskasutusse Accessi allkirja (SAS) märgid. Sel juhul kliendi autendib otse teenuse siini logides omas vermitud märgiks salajane võti. Kõned ACS enam ei vaja. SAS sõned kohta leiate lisateavet teemast [teenuse siini autentimist][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Järjekorrad ja teemade vastu sõnumside poe tõrkeid kaitsmine

Liigendatud järjekorda või teema on määratud üks sõnumside poe. Kui see sõnumside poe pole saadaval, ei õnnestu kõik toimingud, et järjekorda või teema. Sektsioonitud järjekorda, teisalt, koosneb mitmest fragmendid. Iga fragment talletatakse erinevate sõnumside poest. Sõnumi saatmisel sektsioonitud järjekorda või teema määrab teenuse siini sõnumi ühte osadest. Kui vastav sõnumside poe pole saadaval, teenuse siini kirjutab sõnumi erinevate fragmendid, võimaluse korral. Sektsioonitud üksuste kohta leiate lisateavet teemast [tekstsõnumside üksused liigendatud][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Kaitsta andmekeskuse katkestuste või katastroofid

Luba Tõrkesiirde vahel kahes andmekeskuses, saate luua teenuse siini teenuse nimeruumi iga andmekeskuses. Näiteks teenuse siini teenuse nimeruumi **contosoPrimary.servicebus.windows.net** võib asuda piirkonnas Ameerika Ühendriikides Põhja-ja Kesk ja **contosoSecondary.servicebus.windows.net** võib asuda meile Lõuna/keskne piirkond. Kui teenus siini, sõnumside üksus peab jääma puuetega inimestele juurdepääsetavate juuresolekul andmekeskuse katkestuste, saate selle üksuse nii nimeruumid.

Lisateavet leiate jaotisest "Teenuse siini sees on Azure andmekeskuse tõrge" [asünkroonne sõnumside mustrite][]ja kõrge-saadavus.

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Relay lõpp-punktid andmekeskuse katkestuste või katastroofide kaitsmine

Geo-dispersioonanalüüs relay lõpp-punktid võimaldab teenus, mis seab juuresolekul teenuse siini katkestuste kättesaadav, et relay lõpp-punkti. Geo-dispersioonanalüüs saavutamiseks looma teenuse erinevate nimeruumid kaks relay lõpp-punktid. Funktsiooni nimeruumid peab asuma eri andmekeskuste ning kaks lõpp-punktid peavad olema erinevad nimed. Näiteks jõuate esmane lõpp-punkti **contosoPrimary.servicebus.windows.net/myPrimaryService**, klõpsake jaotises ajal teisene töölauafunktsioonid selle saab teile helistada jaotises **contosoSecondary.servicebus.windows.net/mySecondaryService**.

Teenuse kuulab seejärel mõlemad lõpp-punktid ja mõnda muud klienti saate Kasuta teenust ükskõik kumma lõpp-punkti kaudu. Kliendi rakendus CEIP huvitavat ühte soovitud releed esmane lõpp- ja saadab taotluse aktiivse lõpp-punkti. Kui toiming nurjus tõrkekoodiga, see viga näitab relay lõpp-punkti ei ole saadaval. Avab kanali varukoopia lõpp-punkti ja annab taotluse. Sel hetkel aktiivse ja varukoopia lõpp-punktid vahetada rollid: klientrakenduse leiab vanad aktiivse lõpp-punkti varukoopia uue lõpp-punkti ja vana varukoopia lõpp-punkti olema aktiivne uue lõpp-punkti. Kui nii saata toimingute fail, rollid kaks üksust ei muutu ja tagastatakse tõrge.

[Geo kopeerimisest teenuse siini ümber sõnumite][] valimi näitab, kuidas ise releed.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Järjekorrad ja teemade andmekeskuse katkestuste või katastroofide kaitsmine

Saavutamiseks paindlikkuse andmekeskuse katkestuste eest kui abil vahendajaks sõnumside teenuse siini toetab kahel viisil: *aktiivne* ja *passiivne* kopeerimine. Iga lähenemisviisi, kui on antud järjekorda või teema peab jääma puuetega inimestele juurdepääsetavate juuresolekul andmekeskuse elektrikatkestus, saate luua selle sisse nii nimeruumid. Mõlemad üksused võib olla sama nimi. Näiteks jõuate esmane järjekorda **contosoPrimary.servicebus.windows.net/myQueue**, klõpsake jaotises ajal teisene töölauafunktsioonid selle saab teile helistada jaotises **contosoSecondary.servicebus.windows.net/myQueue**.

Kui rakendus ei nõua vahetust saatja vastuvõtja pilti, saate rakenduse rakendada püsival kliendipoolne järjekorda sõnumi kaotsimineku vältimiseks ja siirdamiseks teenuse siini vigu saatja kaitsta.

## <a name="active-replication"></a>Aktiivse kopeerimine

Aktiivse kopeerimine kasutab üksuste nii nimeruumid iga toimingu jaoks. Mis tahes klient, mis saadab sõnumi saadab sama sõnumi koopiat. Esimese koopia saadetakse esmane üksus (näiteks **contosoPrimary.servicebus.windows.net/sales**) ja teise sõnumi koopia saadetakse teisene üksus (näiteks **contosoSecondary.servicebus.windows.net/sales**).

Klient saab sõnumeid nii järjekorrad. Vastuvõtja töötleb esimese sõnumi koopia ja teine koopia pärsitakse. Mis tahes eksemplaris sõnumeid, peab saatja sildistamine iga sõnumi kordumatut tunnust. Mõlemad eksemplarid sõnum peab olema sama identifikaatoriga kodeeritud. Saate sõnumi sildistamiseks [BrokeredMessage.MessageId][] või [BrokeredMessage.Label][] atribuudid või kohandatud atribuut. Vastuvõtja peab nimekirja sõnumid, mis on juba saanud.

[Geo kopeerimisest teenuse siini vahendajaks sõnumite][] valimi näitab aktiivse sõnumside üksuste kopeerimine.

> [AZURE.NOTE] Aktiivse kopeerimine lähenemine kahekordistab toimingute arv, seetõttu seda moodust võib põhjustada kõrgem.

## <a name="passive-replication"></a>Passiivne dispersioonanalüüs

Veatu juhul passiivne dispersioonanalüüs kasutab ainult üks kahest sõnumside üksustest. Aktiivse üksuse klient saadab sõnumi. Aktiivse üksuse toimingu nurjumisel tõrkekoodiga, mis näitab andmekeskuses, mis hostib aktiivse üksuse ei pruugi olla saadaval klient saadab sõnumi koopia varukoopia üksusele. Sel hetkel aktiivse ja varukoopia üksuste aktiveerimine rollid: saatmise kliendi leiab vanad aktiivse üksuse olema varukoopia uue üksuse ja vana varukoopia üksus on aktiivne uus üksus. Kui nii saata toimingute fail, rollid kaks üksust ei muutu ja tagastatakse tõrge.

Klient saab sõnumeid nii järjekorrad. Kuna seal on võimalus, et vastuvõtja saaks sama sõnumi koopiat, peab vastuvõtja tõkestamine eksemplaris sõnumeid. Saate tõkestada duplikaatide samamoodi, nagu on kirjeldatud aktiivse kopeerimine.

Passiivne dispersioonanalüüs on üldiselt väiksema kui aktiivse kopeerimine, kuna enamasti ühe toimingu. Latentsus, läbilaskevõime ja rahalise kulu on identne-kopeeritud stsenaariumi.

Passiivne dispersioonanalüüs kasutamisel järgmistel juhtudel sõnumite saate kadunud või saadud kaks korda:

-   **Sõnumi viivituse või kaotus**: Oletame, et saatjale saadetud sõnumi m1 esmane kuhjuda ja seejärel kuhjuda kättesaamatu enne vastuvõtja saab m1. Saatja saadab järgmise sõnumi m2 teisene järjekorda. Kui esmane järjekorda pole ajutiselt saadaval, saab vastuvõtja m1 pärast kuhjuda muutub uuesti. Puhul katastroof, võidakse vastuvõtja kunagi m1.

-   **Vastuvõtu dubleerimine**: Oletame, et saatja saadab sõnumi m esmane järjekorda. Teenuse siini edukalt töötleb m, kuid ei saada vastuse. Pärast Saatmistoiming ajalõpp, saadab saatja identse koopia m teisene järjekorda. Kui vastuvõtja on saada m esimene koopia enne esmane kuhjuda muutub pole saadaval, saab vastuvõtja nii koopiad m ligikaudu samal ajal. Kui vastuvõtja ei ole võimalik saada m esimene koopia enne esmane kuhjuda kättesaamatu, vastuvõtja algselt saab ainult m teine koopia, kuid saab seejärel koopia m vabanemise esmane järjekorda.

[Geo kopeerimisest teenuse siini vahendajaks sõnumite][] valimi näitab passiivne sõnumside üksuste kopeerimine.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet katastroofiabi kohta leiate järgmistest artiklitest:

- [SQL Azure'i andmebaasi järjepidevuse tagamine][]
- [Azure'i paindlikkust tehnilise abi saada?][]

  [Teenuse siini autentimine]: service-bus-authentication-and-authorization.md
  [Sektsioonitud sõnumside üksused]: service-bus-partitioning.md
  [Asünkroonne sõnumside mustrite ja kõrge-saadavus]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Geo kopeerimisest teenuse siini edastatakse sõnumid]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Geo kopeerimisest teenuse siini vahendajaks sõnumid]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [SQL Azure'i andmebaasi järjepidevuse tagamine]: ../sql-database/sql-database-business-continuity.md
  [Azure'i paindlikkust tehnilise abi saada?]: ../resiliency/resiliency-technical-guidance.md
