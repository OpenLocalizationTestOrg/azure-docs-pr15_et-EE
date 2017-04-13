<properties 
    pageTitle="Teenuse siini AMQP ülevaade Java | Microsoft Azure'i" 
    description="Lugege teemat Java abil on täiustatud sõnumi järjekord protokolli (AMQP) 1.0 Azure." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="amqp-10-support-in-service-bus"></a>Teenuse siini AMQP 1.0 tugi

Azure'i teenus siini pilveteenuses nii kohapeal [Teenuse siini Windows Server (teenuse siini 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) tugi on täiustatud sõnumi Queueing protokolli (AMQP) 1.0. AMQP võimaldab teil koostada mitu platvormi, hübriidjuurutuse rakendustes, mis kasutavad avatud standardne protokoll. Saate luua rakendusi, mis kasutavad komponendid, mis on loodud, kasutades erinevates keeltes ja raamistik ja opsüsteeme, mis töötavad. Kõik need komponendid saate luua ühenduse teenuse siini ja sujuvalt Exchange'i liigendatud business sõnumite tõhus ja täielik kvaliteedikadu.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Sissejuhatus: Mis on AMQP 1.0 ja miks on oluline?

Traditsiooniliselt kasutanud sõnumi rakendusse vahetarkvaratoodete kaitstud Protokollid klientrakendused ja maaklerid vahel. See tähendab, et kui olete valinud konkreetse tootja kiirsõnumside ta, peate kasutama selle tarnija teekide ühenduse soovite, et ta oma klientrakendustes. Tulemuseks astet sõltuvus hankija, kuna teisaldamise taotluse mõne muu toote jaoks on vaja koodi muudatusi ühendatud rakendustes. 

Lisaks ühendamine sõnumside maaklerid eri hankijad on keeruline. Tavaliselt on selleks vaja Rakendusetaseme teisendamine sõnumite teisaldamine ühest süsteemist teise ja nende kaitstud sõnumivormingute vahel tõlkimine. See on levinud nõue; Näiteks kui peate pakkuda uut ühendatud kasutajaliidese vanemaealiste eraldiseisvat süsteemi või integreerida see süsteemide ühinemine.

Tarkvara on kiiresti business; uue programmeerimise keeled ja rakenduse raamistiku tuuakse mõnikord hämmastava tempos. Samuti ajas muutuda nõuded IT-süsteemide ja arendajate soovite platvormi uusimate funktsioonide eeliseid. Siiski mõnikord valitud sõnumside hankija ei toeta need platvormid. Kuna sõnumside Protokollid on kaitstud, ei ole võimalik teiste teekide ette need uue platvormid. Seetõttu tuleb kasutada lähenemisel lüüsid või sillad, mis võimaldavad teil sõnumid toote edasi kasutada.

Arengu, on täiustatud sõnumi järjekord protokolli (AMQP) 1.0 oli tingitud järgmiste probleemide korral. See pärineb JP Morgan Chase, kes on kõige teenuste ettevõtete, nagu on sõnumi rakendusse vahevara juures. Eesmärk on lihtne: Ava-standard sõnumside protokoll, mis võimaldas koostada sõnumi põhiseid rakendusi, mis on ehitatud kasutades erinevates keeltes, raamistik ja operatsioonisüsteemides komponentide abil luua kõik kasutamise head ning oma ala komponentide tarnijate vahemikus.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 tehnilise funktsioonid

AMQP 1.0 on tõhusa, usaldusväärseid, kaabel taseme sõnumside protokoll, mille abil saate koostada töökindlaid, mitu platvormi, sõnumside rakendused. Protokoll on lihtne eesmärk: sõnumite vahel pooled turvalist, usaldusväärseid ja tõhusa edastamise mehaanika määratleda. Teate ise kodeeritud portable andmete esituse, mis võimaldab heterogeensete saatja ja vastuvõtja Exchange liigendatud business sõnumid täielik kvaliteedikadu. Järgmine on kõige olulisemad funktsioonid Kokkuvõte:

*    **Tõhus**: AMQP 1.0 on ühendus rakendusse protokolli, mis kasutab kahendarvu kodeering protokolli juhiseid ja sõnumite äri üle selle üle. See hõlmab keerukaid meilivoo kontrolli süsteemidega maksimeerimiseks võrgu ja ühendatud komponentide kasutamist. See tähendab, et eesmärk on tasakaalustada tõhus, paindlik ja koostalitlusvõime protokolli.
*    **Usaldusväärne**: The AMQP 1.0 protokoll võimaldab sõnumeid vahetada erinevaid töökindluse garantiid, alates fire-ja-unusta usaldusväärne, et täpselt – üks kord tunnistada kohaletoimetamise.
*    **Paindlik**: AMQP 1.0 on paindlik protokoll, mida saate kasutada erinevate topoloogiatest toetamiseks. Suhtlus kliendi klient, kliendi-ta ja ta-ta saab kasutada sama protokolli.
*    **Ta-mudeli sõltumatu**: The AMQP 1.0 spetsifikatsioon ei tee nõuete kasutatavaid on ta sõnumside mudel. See tähendab võimalik hõlpsalt lisamiseks olemasoleva sõnumside maaklerid AMQP 1.0 tugi.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 on Standard (koos mõne muutmine ")

Alates 2008 rohkem kui 20 ettevõtet, core rühm on AMQP 1.0 tehnoloogia tarnijate nii lõppkasutaja ettevõtetele. Selle aja jooksul kasutaja ettevõtete võis oma ettevõtte tegelike nõuetele ja tehnoloogia müüjad muutunud protokolli nendele nõuetele. Kogu protsessi, tarnijatele on osalenud, kus ta koostööd oma rakendusi koostalitlusvõime kinnitamiseks.

Oktoober 2012 ilmus oktoober 2011 arengu töö transitioned tehnikakomitee, liigendatud teavet standardid edutamine (OASIS) ja OASIS AMQP 1.0 Standard ettevõttes. Järgmised ettevõtted osalesid tehnilise teavitada standardit arendamise käigus:

*    **Tehnoloogia müüjad**: Axway tarkvara, Huawei tehnoloogiad, IIT tarkvara, INETCO süsteemid, Kaazing, Microsoft, Mitre Corporation, Primeton tehnoloogiad, poolelioleva tarkvara, punane rolli, SITA, tarkvara AG, Solace süsteemide, VMware, WSO2, Zenika.
*    **Kasutaja ettevõtetele**: Ameerika panga krediitkaardiga Suisse, saksa Boerse, Goldman Sachs, JPMorgan Chase.

Mõned sagedamini viidatud avatud standardid eelised on järgmised:

*    Väiksem võimalus müüja lock-in
*    Koostalitlusvõime
*    Teekide ning tööriistade laialdane kättesaadavus
*    Kaitse aegumine
*    Kogenud töötajate kättesaadavus
*    Alumise ja mõistliku risk

## <a name="amqp-10-and-service-bus"></a>AMQP 1,0 ja teenus siini

Azure'i teenus siini AMQP 1.0 tugi tähendab, et saate kohe kasutada teenuse siini queuing ja avaldamise/tellimise vahendatud sõnumside funktsioonide erinevatest seadmetest kasutab tõhusa kahendarvu protokolli kaudu. Lisaks saate luua rakendusi asuvaid komponentidest kasutavate keelte, raamistiku ja operatsioonisüsteemid.

Järgmisel joonisel illustreerib näide juurutus, mis, kus töötab Linux, kirjutada, kasutades standard Java sõnumi teenuse (JMS) API ja .net-i kliendid töötab Windows, Java kliendid exchange sõnumeid teenuse siini abil AMQP 1.0 kaudu.

![][0]

**Joonis 1: Näide juurutamise stsenaarium näitab, mitu platvormi sõnumside teenuse siini ja AMQP 1.0 abil**

Sel ajal järgmised kliendi teegid teadaolevalt teenuse siini töötamine.

| Keel | Teegis                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Kliendi Apache Qpid Java sõnumi teenuse (JMS)<br/>IIT tarkvara SwiftMQ Java klient |
| C        | Apache Qpid prootonpumba-C                                                          |
| PHP      | Apache Qpid prootonpumba-PHP                                                        |
| Python   | Apache Qpid prootonpumba-Python                                                     |


**Joonis 2: Tabeli AMQP 1.0 kliendi teegid**

Hankimine ja nende teekide abil teenuse siini kohta leiate lisateavet teemast [teenuse siini AMQP arendaja juhend][]. Lisateavet linke vt [järgmised toimingud](service-bus-java-amqp-overview.md#next-steps) .

## <a name="summary"></a>Kokkuvõte

*    AMQP 1.0 on avatud, usaldusväärseid sõnumside protokoll, mille abil saate koostada mitu platvormi, hübriid rakendused. AMQP 1.0 on OASIS standard.
*    AMQP 1.0 tugi on nüüd saadaval Azure'i teenus siini kui ka teenuse siini Windows Server (teenuse siini 1.1). Hinnad on sama mis olemasolevate protokollidega.

## <a name="next-steps"></a>Järgmised sammud

Külastage teenuse siini AMQP toe kohta järgmisi linke.

*    [Kuidas kasutada AMQP 1.0 teenuse siini .net-i API-ga](service-bus-dotnet-advanced-message-queuing.md)
*    [Kuidas kasutada Java sõnumi teenuse (JMS) API teenuse siini & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Teenuse siini AMQP arendaja juhend][]
*    [OASIS täiustatud sõnumi järjekord protokolli (AMQP) versiooni 1.0 määratlus](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)

[0]: ./media/service-bus-java-amqp-overview/Example1.png
[Teenuse siini AMQP arendaja juhend]: service-bus-amqp-dotnet.md

 
