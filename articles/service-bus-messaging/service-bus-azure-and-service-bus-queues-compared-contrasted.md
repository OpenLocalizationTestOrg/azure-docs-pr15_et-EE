<properties 
    pageTitle="Azure'i järjekorrad ja teenuse siini järjekorrad - võrreldes ja normaalses tööasendis | Microsoft Azure'i"
    description="Analüüsib erinevusi ja sarnasusi kahte tüüpi pakutud Azure'i järjekorda."
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
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure'i järjekorrad ja teenuse siini järjekorrad - võrreldes ja normaalses tööasendis

Selles artiklis analüüsib erinevusi ja sarnasusi järjekorda pakutud Microsoft Azure'i täna kahte tüüpi: Azure'i järjekorrad ja teenuse siini järjekorrad. Selle teabe abil saate võrrelda ja kontrasti vastav tehnoloogiad ja saaksid rohkem teavet otsustada, milline lahendus kohta kõige paremini vastab teie vajadustele.

## <a name="introduction"></a>Sissejuhatus

Microsoft Azure'i toetab kahte tüüpi järjekorda menetlustele: **Azure'i järjekorrad** ja **Teenuse siini järjekorrad**.

**Azure'i järjekorrad**, mis on [Azure storage](https://azure.microsoft.com/services/storage/) infrastruktuuri osa, on lihtne ülejäänud vastavalt Peek/saada/panna liides, esitada usaldusväärne, püsivate sõnumside ja nende teenuste vahel.

**Teenuse siini järjekorrad** on osa laiemast [Azure'i sõnumside](https://azure.microsoft.com/services/service-bus/) infrastruktuuri, mis toetab samuti avaldamise/tellimise, Web teenuse remoting ja integreerimise mustrite queuing. Teenuse siini järjekorrad, teemade/tellimused ja releed kohta leiate lisateavet teemast [teenuse siini sõnumside tutvustus](service-bus-messaging-overview.md).

Kuigi mõlemad andmebaasitõrge tehnoloogiad olemas üheaegselt, Azure'i järjekorrad võeti kasutusele esmalt sihtotstarbeline järjekorda salvestusruumi süsteem ehitatud Azure storage teenused. Teenuse siini järjekorrad ehitatakse peal loodud rakenduste või mitme protokollide, andmete lepingute ulatuvad komponendid laiem "vahendajaks sõnumside" taristu usaldada domeenid ja/või võrgu keskkonnas.

Selles artiklis võrreldakse kaks järjekorda tehnoloogiad pakutud Azure'i käsitletakse käitumine ja funktsioonid, mida iga rakendamine erinevused. Selle artikli juhised valida, millised funktsioonid võivad vastavalt oma rakenduste arendamise vajadustele.

## <a name="technology-selection-considerations"></a>Tehnoloogia valiku kaalutlused

Azure'i järjekorrad nii teenuse siini järjekorrad on sõnumi, mis on järjekorda paneku teenus, mida praegu pakutakse Microsoft Azure'i rakendusi. Iga on veidi teistsugused funktsioonikomplekti, mis tähendab, et saate valida ühe või teise või mõlemad, sõltuvalt teie kindla lahenduse või business tehniline lahendamine teil on kasutada.

Määratlemine, mille andmebaasitõrge tehnoloogia sobib selleks antud lahenduse, arendajad ja lahenduse arhitektid tuleks arvesse võtta järgmisi soovitusi. Lisateavet leiate järgmisest jaotisest.

Lahendus arhitekt/arendaja, **võiksite kasutada Azure järjekorrad** kui:

- Rakenduse peab üle 80 GB sõnumite talletamiseks järjekorda, kuhu sõnumid on lühem üle 7-päevaste kasutusaeg.

- Rakenduse soovib jälgimiseks töötlemiseks sõnumi sees järjekorda. See on kasulik, kui sõnumi töötlemine töötaja jookseb. Järgmise töötaja saate seda teavet, kui eelneva töötaja pooleli jäi jätkata.

- Teil on vaja logid küljel kõigi oma järjekorrad suhtes tehtud tehingute.

Lahendus arhitekt/arendaja, **võiksite kasutada teenuse siini järjekorrad** kui:

- Teie lahendus peate saama sõnumeid, ilma vajaduseta küsitlus järjekorda. Teenuse siini, kus seda saavutada abil pikk-küsitlused TCP-põhiste teenuste siini toetab Protokollid abil vastu võtta.

- Teie lahendus nõuab kuhjuda esitada on tagatud esimene-esimese-väljaregistreerimine (FIFO) tellitud kohaletoimetamise.

- Soovite Azure'i ja Windows Server (privaatne cloud) sümmeetriline kogemusel. Lisateavet leiate teemast [Teenuse siini Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).

- Teie lahendus peate saama toetavad automaattuvastus dubleeritud.

- Soovite rakenduse protsessi sõnumeid nimega paralleelselt pikaajalisi voole (sõnumid on seotud [SeansiId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) atribuudi kasutamine sõnumi voo). Selle mudeli iga sõlme nõudvate rakenduse konkureerib voogu asemel sõnumeid. Voo manustamisel nõudvate sõlm sõlme saate tutvuda rakenduse voo riigi tehingud abil.

- Teie lahendus on vaja selgituseks käitumine ja atomicity edastamisel mitme meilisõnumi kaudu järjekorda.

- Aja live (TTL) omadus rakendusele vastav töökoormus ei ületa 7 päeva jooksul.

- Rakenduse tegeleb sõnumid, mida saab üle 64 KB, kuid tõenäoliselt ei lähenemine on 256 KB piirmäära.

- Toime tulla Rollipõhine pääsu mudeli järjekorrad ja eri õiguste/õiguste andmiseks saatja ja vastuvõtja nõue.

- Oma järjekorra suurus kasvab pole suurem kui 80 GB.

- Mida soovite kasutada AMQP 1.0 standarditele vastavalt sõnumside protokoll. AMQP kohta leiate lisateavet teemast [Teenuse siini AMQP ülevaade](./service-bus-amqp-overview.md).

- Saate näevad ette kasutamine lõpliku migreerimiseks järjekorda vastavalt kakspunkt side sõnumi Exchange'i mustri, mis võimaldab sujuv integratsioon täiendavad vastuvõtjad (Tellijad), millest igaüks saab sõltumatu kas osaliselt või täielikult järjekorda saadetud sõnumite koopiad. Viimane viitab algupäraselt pakutav teenus siini avalda/tellimise võimalus.

- Teie tekstsõnumside lahendus peate saama toetavad "Enamik-korraga" kohaletoimetamise tagatis ilma vajaduseta saate luua täiendavaid taristu komponendid.

- Soovite saama avaldamine ja tarbimine pakettidena sõnumeid.

- Teil on vaja Windows Communication Foundation (WCF) side virnas .NET Frameworki täielik integreerimine.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Azure'i järjekorrad ja teenuse siini järjekorrad võrdlus

Järgmistes jaotistes tabelite pakkuda loogiline rühmitamine järjekorda funktsioonide ja võimaldavad teil võrrelda ülevaade, funktsioonid saadaval Azure järjekorrad nii teenuse siini järjekorrad.

## <a name="foundational-capabilities"></a>Põhilisi võimalused

Selles jaotises võrdleb mõne olulise andmebaasitõrge võimaluste järjekorrad Azure'i järjekorrad ja teenuse siini esitatud.

|Võrdluskriteeriumid|Azure'i järjekorrad|Teenus siini järjekorrad|
|---|---|---|
|Garantii tellimine|**Ei** <br/><br>Lisateavet leiate teemast esimene märge jaotisest "Lisateave".</br>|**Jah – esimene-sisse-esimese välja (FIFO)**<br/><br>(abil sõnumside seansid)|
|Kohaletoimetamise tagatis|**Vähim-korraga**|**Vähim-korraga**<br/><br/>**Enamik – korraga**|
|Atomic toiming tugi|**Ei**|**Jah**<br/><br/>|
|Saada käitumine|**Mitte-blokeerimine**<br/><br/>(lõpetab kohe, kui uus sõnum pole)|**Ajalõpp ja ilma blokeerimine**<br/><br/>(pakutakse pika küsitlused, või ["Comet tehnika"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Mitte-blokeerimine**<br/><br/>(.net-i abil hallatav API ainult)|
|Tõuketeatised laadis API|**Ei**|**Jah**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) ja **OnMessage** seansid .net-i API-ga.|
|Saada režiim|**Peek & üürilepingu**|**Peek & lukustamine**<br/><br/>**Vastu võtta ja kustutamine**|
|Välja arvatud režiim|**Üürilepingu vastavalt**|**Lukusta vastavalt**|
|Üürilepingu/Lock kestus|**30 sekundit (vaikeväärtus)**<br/><br/>**7 päeva (lubatud)** (Saate pikendada või vabastage sõnumi rendilepingu [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API abil.)|**60 sekundi järel (vaikeväärtus)**<br/><br/>Sõnumi lukustamine, [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API abil saate pikendada.|
|Üürilepingu/Lock täpsus|**Sõnumi tase**<br/><br/>(iga sõnumi võib olla erinevad ajalõpp väärtus, mis töötlemise sõnumis [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API abil saate siis vastavalt vajadusele värskendada)|**Järjekorda tase**<br/><br/>(iga järjekorra on Lukusta täpsus rakendatakse kõigile oma sõnumid, kuid saate pikendada lukustamine, kasutades [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API.)|
|Batched vastuvõtmine|**Jah**<br/><br/>(otseselt määrata sõnumi count sõnumeid kuni 32 sõnumite allalaadimisel)|**Jah**<br/><br/>(peidetult lubamine eelse toomise atribuut või konkreetselt abil tehingud)|
|Batched saatmine|**Ei**|**Jah**<br/><br/>(tehingud või kliendipoolne partiide) abil|

### <a name="additional-information"></a>Lisateave

- Azure'i järjekordades sõnumid on tavaliselt esimene-esimese-väljaregistreerimine, kuid mõnikord võivad need olla vales järjestuses; Näiteks kui sõnumi nähtavuse ajalõpp kestus lõpeb (näiteks tulemusena kliendi rakendus krahh töötlemise ajal). Nähtavuse ajalõpp aegumisel sõnumi taustal uuesti sisse teise töötaja dequeue selle järjekorras. Sel hetkel võidakse järjekorda (dequeued uuesti) pärast sõnumi, mis on algselt enqueued selle järele äsja nähtav sõnum.

- Kui kasutate Azure salvestusruumi plekid või tabelid ja kasutuselevõtt järjekorrad, siis on tagatud 99,9%-saadavus. Kui kasutate teenuse siini järjekorrad plekid või tabelid, on teil alumise kättesaadavus.

- Tagatud teenuse siini järjekordades FIFO muster tuleb kasutada sõnumside seansid. Rakendus jookseb **Peek & Lock** režiimi saadud sõnumi töötlemise ajal, kui järgmine kord, kui järjekorda vastuvõtja aktsepteerib sõnumside seansi selle alustada nurjunud sõnumi pärast selle aja live (TTL) lõpeb.

- Azure'i järjekorrad on mõeldud toetavad standard andmebaasitõrge stsenaariumid, nagu seose komponendid skaleeritavus ja tõrkeid, hälbe suurendamiseks ühtlustamise ja koostamise töövoogude.

- Teenuse siini järjekorrad toetavad *Vähima-korraga* kohaletoimetamise tagatise. Lisaks saate *Kõige-korraga* semantilise toetatud, seansi olek abil rakenduse laoseisu ja kasutate tehingud atomically sõnumeid ja värskendamine seansi olek.

- Azure'i järjekorrad pakuvad ühtse ja järjepideva programmeerimise mudeli järjekorrad, tabelite ja plekid – arendajad ja meeskondade toimingud.

- Teenuse siini järjekorrad toetada kohaliku tehingud kontekstis ühe järjekorda.

- Teenuse siini ei toeta **vastu ja kustutada** režiimi pakub võimalust vähendada sõnumside toiming count (ja seotud kulu) alandas teenuse tagamine eest.

- Azure'i järjekorrad annab liisingute võimalus sõnumite liisingute laiendamine. See võimaldab töötajate säilitamiseks lühike liisingute sõnumitele. Seega, kui töötaja jookseb, sõnumi saab kiiresti töödelda uuesti teise töötaja. Lisaks saate töötaja laiendamine sõnumi liisingueset kui seda on vaja töödelda pikem kui praegune üürilepingu aeg.

- Azure'i järjekorrad pakkuda nähtavuse ajalõpp, mida saate seada korral on enqueueing või dequeuing sõnumi. Lisaks saate värskendada sõnumi erinevate rendi väärtustega käitusaja ja erinevate väärtuste rakenduda sõnumite samas järjekorras. Teenuse siini Lukusta ajalõpud on määratletud järjekorda metaandmete; Siiski saate pikendada lukustus, helistades [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) meetod.

- Maksimaalne aegumine blokeerida teenuse siini järjekordades vastu võtta toiming on 24 päeva. ÜLEJÄÄNUD vastavalt ajalõpud siiski 55 sekundit suurima väärtuse.

- Kliendipoolne partiide teenuse siini esitatud võimaldab järjekorda kliendil partii mitme sõnumite saatmine ühe toimingu. Partiide on saadaval ainult asünkroonne saata tegevuste jaoks.

- Funktsioonid nagu 200 TB ceiling Azure'i järjekorrad (rohkem kui te Virtualisoinnilla kontod) ja piiramatu järjekorrad teha ka optimaalne platvormi SaaS pakkujad.

- Azure'i järjekorrad pakkuda paindlik ja kiire delegeeritud juurdepääsu juhtimine süsteem.

## <a name="advanced-capabilities"></a>Täpsemad funktsioonid

Selles jaotises võrdleb järjekorrad Azure'i järjekorrad ja teenuse siini esitatud täiustatud võimalused.

|Võrdluskriteeriumid|Azure'i järjekorrad|Teenus siini järjekorrad|
|---|---|---|
|Ajastatud kohaletoimetamise|**Jah**|**Jah**|
|Automaatse surnud kiri|**Ei**|**Jah**|
|Suureneva järjekord aja live väärtus|**Jah**<br/><br/>(kaudu nähtavuse ajalõpp, installige värskendus)|**Jah**<br/><br/>(osutatakse sihtotstarbeline API funktsiooni)|
|Poison sõnumi tugi|**Jah**|**Jah**|
|Installige värskendus|**Jah**|**Jah**|
|Serveripoolne tehingulogi|**Jah**|**Ei**|
|Salvestusruumi mõõdikud|**Jah**<br/><br/>**Minutid mõõdikute**: pakub reaalajas mõõdikute kättesaadavus TPS, API kõne loendab, tõrge loendab ja palju muud reaalajas (kokkuvõtliku minutis ja mis juhtus valmistamisel mõne minuti jooksul pärast. Lisateabe saamiseks vt [Talletusruumi Analytics mõõdikute](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Jah**<br/><br/>(hulgi päringud, helistades [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Maakond haldus|**Ei**|**Jah**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Sõnumi automaatne edasisaatmine|**Ei**|**Jah**|
|Likvideerite järjekorda funktsioon|**Jah**|**Ei**|
|Sõnumi rühmad|**Ei**|**Jah**<br/><br/>(abil sõnumside seansid)|
|Rakenduse oleku teade rühma kohta|**Ei**|**Jah**|
|Duplikaatide tuvastamine|**Ei**|**Jah**<br/><br/>(konfigureeritav saatja poolel)|
|WCF-i integreerimine|**Ei**|**Jah**<br/><br/>(pakub out-of-box WCF-i sidumiste)|
|WF integreerimine|**Kohandatud**<br/><br/>(nõuab koostamise kohandatud WF tegevuse)|**Kohalikke**<br/><br/>(pakub out-of-box WF tegevused)|
|Sõnumi rühmade sirvimine|**Ei**|**Jah**|
|Sõnumi seansid tõmbamine ID|**Ei**|**Jah**|

### <a name="additional-information"></a>Lisateave

- Nii andmebaasitõrge tehnoloogiad lubada sõnumi kohaletoimetamise ajastada, hiljem.

- Järjekorda automaatne edasisaatmine võimaldab tuhandeliste järjekorda, et automaatselt edasi saadetud nende sõnumeid ühe järjekorda, millest vastuvõtmise rakendus töötab sõnum. See abil saavutada turvalisus, kontrollivad ja teostavad mäluruumi iga sõnumi Publisheri vahel.

- Azure'i järjekorrad toe pakkumine värskendamine sõnumi sisu. Saate kasutada seda funktsiooni püsib olekuteabe ja suureneva edenemist sõnumisse, et seda saab töödelda viimase teadaolevad kontrollpunkt, mitte nullist kaudu. Teenuse siini järjekorrad, saate lubada abil sõnumi seansid sama stsenaariumi. Seansid võimaldavad salvestada ja tuua rakenduse töötlemise olek ( [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) ja [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)abil).

- [Surnud kiri](service-bus-dead-letter-queues.md), mis toetab ainult teenuse siini järjekorrad, võib olla kasulik eraldamiseks sõnumid, mis ei saa töötlemine õnnestus vastuvõtmise rakendus või sõnumeid ei jõua sihtkohta on aegunud aja live (TTL) atribuudi tõttu. TTL-i väärtust saate määrata, kui kaua sõnum jääb järjekorda. Koos teenuse siini, teisaldatakse sõnum nimega $DeadLetterQueue kui TTL lõpeb teisiti järjekorda.

- "Mürki" sõnumite otsimine Azure'i järjekordades, kui sõnumi dequeuing rakenduse uurib atribuudi **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** sõnumi. Kui **DequeueCount** on antud kõrgemal, rakenduse teisaldab sõnumid on rakenduse määratletud "dead kiri" järjekorda.

- Azure'i järjekorrad võimaldavad teil saada kõik vastu järjekorda, kui ka nii liidetud mõõdikute tehtud tehingute kohta üksikasjalikku Logi. Nende suvandite on kasulikud silumine ja mõista, kuidas teie rakendus kasutab Azure järjekorrad. Need on kasulik rakenduse jõudluse häälestamine ja kasutamine järjekorrad vähendamine.

- "Sõnumi seansid" teenuse siini ei toeta mõiste võimaldab sõnumid, mis kuuluvad teatud loogilise rühma seotud antud vastuvõtja, mis omakorda loob seansi nagu osaleja sõnumeid ja nende vastavate vastuvõtjad vahel. Saate lubada selle teenuse siini keerukamaid funktsioone, seades atribuuti [SeansiId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) sõnumi. Seejärel saate vastuvõtjad kuulata teatud seansi ID ja sõnumeid, mis ühiskasutus määratud seansi identifikaator.

- Kordamise tuvastamise funktsiooni teenuse siini järjekorrad automaatselt ei toeta eemaldab dubleeritud järjekorda või teema alusel [MessageID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) atribuudi väärtust saadetud sõnumid.

## <a name="capacity-and-quotas"></a>Ametinimetus ja kvoote

Selles jaotises võrdleb Azure'i järjekorrad ja teenuse siini järjekorrad seisukohast [võimsus ja kvootide](service-bus-quotas.md) võidakse rakendada.

|Võrdluskriteeriumid|Azure'i järjekorrad|Teenus siini järjekorrad|
|---|---|---|
|Suurim lubatud järjekorra suurus|**200 TB**<br/><br/>(ainult ühe konto mälumaht)|**1 GB 80 GB**<br/><br/>(pärast loomist järjekord ja [lubada eraldamine](service-bus-partitioning.md) – määratletud jaotisest "Lisateave")|
|Sõnumi maksimaalse mahu|**64 KB**<br/><br/>(48 KB kasutamisel **Base64** kodeeringus)<br/><br/>Azure'i toetab suur sõnumid, kombineerides järjekorrad ja plekid – sel hetkel saate nimekirja lisamine kuni 200GB ühe üksuse.|**256 KB** või **1 MB**<br/><br/>(sh nii päise ja keha, kuni päise suurus: 64 KB).<br/><br/>Sõltub [teenuse kiht](service-bus-premium-messaging.md).|
|Sõnumi maksimaalse TTL|**7 päeva**|**`TimeSpan.Max`**|
|Suurim lubatud arv järjekorda|**Piiramatu**|**10 000**<br/><br/>(kohta teenuse nimeruumi, tõsta)|
|Samaaegseid klientide arvu ülempiir|**Piiramatu**|**Piiramatu**<br/><br/>(100 samaaegseid ühenduse piirang kehtib ainult TCP protokolli alusel side)|

### <a name="additional-information"></a>Lisateave

- Teenuse siini jõustab järjekorda mahupiirangud. Suurim lubatud järjekorra suurus on määratud pärast loomist järjekorra ja võib olla väärtus vahemikus 1 kuni 80 GB. Kui on järjekord suurus väärtus loomine järjekorda seadmine, täiendavad sissetulevad sõnumid on tagasi lükatud ja erandi vastu helistaja koodi. Teenuse siini kvootide kohta leiate lisateavet teemast [Teenuse siini piirmäärasid](service-bus-quotas.md).

- Saate luua teenuse siini järjekorrad 1, 2, 3, 4 ja 5 GB suuruses (vaikimisi 1 GB). Koos eraldamine lubatud (vaikesäte), teenuse siini loob 16 sektsioonid iga määratud GB. Kui loote järjekorda, mis on 5 GB suuruseid, 16 sektsioonid maksimaalne järjekorra suurus muutub (5 * 16) = 80 GB. Saate vaadata oma sektsioonitud järjekorda või teema maksimummaht vaadates selle kirje [Azure portaali][].

- Koos Azure järjekorrad, kui sõnumi sisu pole XML-i ohutu, siis peab olema **Base64** kodeeringuga. Kui te **Base64**-kodeerida sõnumi, last kasutaja saab kuni 48 KB asemel 64 KB.

- Teenuse siini järjekorrad, kus iga sõnumi järjekorda talletatud koosneb kahest osast: päise ja keha. Sõnumi kogumaht ei tohi ületada sõnumi maksimaalse mahu teenuse kiht ei toeta.

- Kui kliendid suhelda teenuse siini järjekorrad TCP-protokolli, samaaegseid ühendusi ühe teenuse siini järjekorda maksimaalne arv on piiratud 100. See arv jagatud saatja ja vastuvõtja. Kui see täis, edaspidised taotlused täiendavate ühenduste on tagasi lükatud ja erandi vastu helistaja koodi. See piirang on pandud ei kliendid kohale ülejäänud vastavalt API abil.

- Kui vajate rohkem kui 10 000 järjekorrad ühe teenuse siini nimeruumi, saate Azure tugimeeskonna poole ja taotleda suurendada. Üle 10 000 järjekorrad koos teenuse siini mastaapida, saate luua ka täiendavaid nimeruumid [Azure portaali][].

## <a name="management-and-operations"></a>Haldus ja toimingud

Selles jaotises võrdleb Azure'i järjekorrad ja teenuse siini järjekorrad haldamise võimalused.

|Võrdluskriteeriumid|Azure'i järjekorrad|Teenus siini järjekorrad|
|---|---|---|
|Protokoll|**Viige kursor HTTP-või HTTPS**|**Viige kursor HTTPS**|
|Käitusaja Protocol (protokoll)|**Viige kursor HTTP-või HTTPS**|**Viige kursor HTTPS**<br/><br/>**AMQP 1.0 Standardne (TCP TSL)**|
|.Net-i hallatav API|**Jah**<br/><br/>(.NET hallatav API salvestusruumi kliendi)|**Jah**<br/><br/>(.Net-i hallatavate vahendajaks sõnumside API)|
|Kohalikke c++:|**Jah**|**Ei**|
|Java API|**Jah**|**Jah**|
|PHP API|**Jah**|**Jah**|
|Node.js API|**Jah**|**Jah**|
|Suvalise metaandmete tugi|**Jah**|**Ei**|
|Järjekorda nime reeglid|**Kuni 63 märki**<br/><br/>(Tähed järjekorra nimi peab olema väiketähed).|**Kuni 260 märki**<br/><br/>(Järjekorra teed ja nimed on väiketähed).|
|Järjekorra pikkus funktsioon hankimine|**Jah**<br/><br/>(Ligikaudne väärtus, kui Sõnumid aeguvad ilma kustutamise Lisaks TTL.)|**Jah**<br/><br/>(Täpne, punkti /-kellaaja väärtuse.)|
|Funktsioon Peek|**Jah**|**Jah**|

### <a name="additional-information"></a>Lisateave

- Azure'i järjekorrad toetada suvalise atribuute, mida saab rakendada järjekorda kirjeldus, nimi ja väärtuse paarideks kujul.

- Nii järjekorda tehnoloogiad pakkuda võimalus peek sõnumi ilma lukustada, mis võib olla kasulik, kui rakendamise järjekorda explorer/brauseri tööriista.

- Teenuse siini .NET vahendajaks sõnumside API-de võimendada täis-dupleks TCP ühendused paranenud jõudlus võrreldes ülejäänud http ja need toetavad AMQP 1.0 standardne protokoll.

- Nimede Azure järjekorda võib olla 3 – 63 tähemärki pikk, võib sisaldada väiketähed, numbreid ja sidekriipse. Lisateabe saamiseks vt [järjekorrad nime andmise ja metaandmed](https://msdn.microsoft.com/library/azure/dd179349.aspx).

- Teenuse siini järjekorda nimesid saab kuni 260 märki ja on vähem piiravad nime reegleid. Teenuse siini järjekorda nimed võivad sisaldada tähtede, numbrite, perioodid, sidekriipse ja allakriipsutatud märkideks.

## <a name="authentication-and-authorization"></a>Autentimise ja luba

Selles jaotises kirjeldatakse autentimise ja luba funktsioonid, mis ei toeta Azure järjekorrad ja teenuse siini järjekorrad.

|Võrdluskriteeriumid|Azure'i järjekorrad|Teenus siini järjekorrad|
|---|---|---|
|Autentimine|**Sümmeetriline võti**|**Sümmeetriline võti**|
|Turvalisuse mudel|Delegeeritud juurdepääsu SAS sõned kaudu.|SAS|
|Identiteedi pakkuja ühendamine|**Ei**|**Jah**|

### <a name="additional-information"></a>Lisateave

- Iga kutse kas andmebaasitõrge tehnoloogiad peavad olema kinnitatud. Anonüümse juurdepääsuga avaliku järjekorrad ei toetata. [SAS](service-bus-sas-overview.md)kasutamisel võite käsitleda seda stsenaariumi kirjutage ainult SAS, kirjutuskaitstud SAS või isegi täis-access SAS avaldamise kaudu.

- Autentimise skeem, mis on esitatud Azure järjekorrad hõlmab sümmeetriline võti, mis on on räsi vastavalt sõnumi autentimist koodi (HMAC-i), arvutatud algoritmi SHA 256 ja stringina **Base64** kodeeritud kasutamist. Vastav protokoll kohta leiate lisateavet teemast [Azure salvestusruumi teenuste autentimise](https://msdn.microsoft.com/library/azure/dd179428.aspx). Teenuse siini järjekorrad toeta sarnase mudeli sümmeetriline klahvide abil. Lisateabe saamiseks vt [Ühiskasutusse Accessi allkirja autentimine teenuse siini](service-bus-shared-access-signature-authentication.md).

## <a name="cost"></a>Maksumus

Selles jaotises võrdleb Azure'i järjekorrad ja teenuse siini järjekorrad maksumus seisukohast.

|Võrdluskriteeriumid|Azure'i järjekorrad|Teenus siini järjekorrad|
|---|---|---|
|Järjekorda tehingu maksumus|**$0.0036**<br/><br/>(kohta 100 000 tehingud)|**Tavaline taseme**: **$0.05**<br/><br/>(kohta miljonit toimingud)|
|Arveldatavate toimingud|**Kõik**|**Saatmise/vastuvõtu**<br/><br/>(tasuta muude toimingute jaoks)|
|Jõude tehinguid|**Arveldatavate**<br/><br/>(Päringute mõnda tühja järjekorda loendatakse arveldatavate tehingu.)|**Arveldatavate**<br/><br/>(On vastuvõtu vastu mõnda tühja järjekorda peetakse arveldatavate sõnumit.)|
|Salvestusruumi maksumus|**$0,07**<br/><br/>(kuus GB /)|**0,00 €**|
|Andmesidetasude väljaminev liiklus andmete|**$0,12 - $0,19**<br/><br/>(Olenevalt geograafia.)|**$0,12 - $0,19**<br/><br/>(Olenevalt geograafia.)|

### <a name="additional-information"></a>Lisateave

- Andmeedastuse tuleb maksta kogusumma jättes Azure andmekeskuste Interneti kaudu antud arveldusperioodi andmete põhjal.

- Andmete ülekannetes paikneva piirkonna Azure teenused on tasuta.

- Kirjutamise, kõigi sissetulevate andmeedastuse on tasuta.

- Antud toetus pikk küsitlused, kasutades teenuse siini järjekorrad saab tõhus olukordades, kus latentsusajaga kohaletoimetamise on nõutav.

>[AZURE.NOTE] Kõik kulud, mis võivad muutuda. Selles tabelis kajastab praeguse hinnad ja ei sisalda reklaami pakub, mis võivad olla praegu saadaval. [Azure'i hinnad](https://azure.microsoft.com/pricing/) lehelt leiate Azure'i hinnad ajakohasemat teavet. Teenuse siini hindade kohta leiate lisateavet teemast [teenuse siini hinnad](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="conclusion"></a>Kokkuvõte

Saamisega süvitsi mõistmine kahe tehnoloogiaid, siis saab teeb rohkem teavet kasutama, millised järjekorda tehnoloogia ja millal. Otsustamine, millal kasutada Azure järjekorrad või teenuse siini järjekorrad selge sõltub mitmest tegurist. Teguritest sõltub tugevalt rakenduse ja selle arhitektuur vajadustele. Kui teie rakendus juba kasutab Microsoft Azure'i core võimaluste, eelistate valida Azure'i järjekorrad, eriti siis, kui vajate põhiliste teatis ja sõnumside teenuseid või vajate järjekorrad, mis võib olla suurem kui 80 GB suuruseid vahel.

Kuna teenuse siini järjekorrad ette mitmeid funktsioone, näiteks seansid, tehingud, duplikaadi tuvastamine, automaatne võimaluste ja püsival dead-kiri avaldamise/tellimise, nad võivad olla eelistatud valik, kui olete hoone hübriid rakenduse või muul viisil rakenduse jaoks on vaja need funktsioonid.

## <a name="next-steps"></a>Järgmised sammud

Järgmised artiklid aitavad juhised ja Azure järjekorrad või teenuse siini järjekorrad kasutamise kohta leiate.

- [Kuidas kasutada teenuse siini järjekorrad](service-bus-dotnet-get-started-with-queues.md)
- [Kuidas kasutada järjekorda salvestusteenus](../storage/storage-dotnet-how-to-use-queues.md)
- [Jõudlustäiustused teenuse siini kasutamise head tavad vahendajaks sõnumside](service-bus-performance-improvements.md)
- [Järjekorrad ja Azure teenuse siini teemade tutvustus](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [Teenuse siini arendaja juhend](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Azure'i andmebaasitõrge teenuse kasutamine](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Azure'i salvestusruumi arveldus-läbilaskevõime, tehingud ja võimsus mõistmine](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure'i portaal]: https://portal.azure.com
 
