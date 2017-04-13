<properties
   pageTitle="Mis on SQL Azure'i andmebaas? | Microsoft Azure'i"
   description="Äriklassi jaotatud andmebaasi võimeline töötlemiseks petabyte relatsiooniline ja relatsiooniline andmemahtusid. On tööstusharu esimese pilve andmete ladu kasvata, Kahanda ja viige sekundites."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Mis on SQL Azure'i andmebaas?

SQL Azure'i andmebaas on võimeline töötlemiseks suuri andmemahtusid, relatsiooniline- ja relatsiooniline pilvepõhist, skaala-out andmebaas. Ehitatud meie oluliselt paralleelne töötlemine (MPP) arhitektuuri, SQL-i andmebaas toime oma ettevõtte töökoormus.

SQL-i andmebaas:

- Ühendab SQL serveri relatsiooniandmebaasist Azure pilveteenuse skaala võimalusi. Saate suurendada, vähendada, peatamine või jätkamine Arvuta sekundites. Kulude kokkuhoidmiseks skaleerimist CPU välja, kui neid vajate ja lõigata kasutus-tippväärtus ajal.
- Azure'i platvormi mõjutab. See on lihtne juurutada, sujuvalt säilitada ja täielikult vea salliv automaatne varukoopiate tõttu.
- SQL serveri ökosüsteemi täiendab. Saate arendada tuttavad SQL Server Transact-SQL (T-SQL-i) ja tööriistu.

Selles artiklis kirjeldatakse olulisi funktsioone SQL-i andmebaas.

## <a name="massively-parallel-processing-architecture"></a>Oluliselt paralleelne töötlemine arhitektuur

SQL-i andmebaas on oluliselt paralleelne töötlemine (MPP) jaotatud andmebaasi süsteem. Jagada andmed ja töötlemine võimalus üle mitme sõlmed, SQL-i andmebaas pakkuda suur skaleeritavus - kaugemale mis tahes ühe süsteem.  Varjatult SQL-i andmebaas kahel andmete üle palju salvestusruumi ühiskasutuses midagi ja töötlemine üksused. Andmed on talletatud Premium kohalik liigsete salvestusruumi ja seotud arvutada sõlmed päringu täitmise. See arhitektuur võtab SQL-i andmebaas "Jaga ja valitse" lähenemine laadimise ja keerukate päringute töötab. Taotlusi saadud juhtelemendi sõlm, optimeeritud ja seejärel edasi Arvuta sõlmed paralleelselt oma tööd teha.

SQL-i andmebaas, kombineerides MPP arhitektuur ja Azure storage võimalusi, teha järgmist.

- Kasvata või Kahanda sõltumatu Arvuta salvestusruumi.
- Kasvata või Kahanda Arvuta liikumata andmed.
- Paus Arvuta võimsus hoides andmeid samaks.
- Elulookirjelduse Arvutage läbilaskevõime on hetkega.

Järgmisel joonisel on esitatud arhitektuur üksikasjalikumalt.

![SQL-i andmete ladu arhitektuur][1]


**Juhtelemendi sõlm:** Juhtelemendi sõlm haldab ja optimeerib päringud. See on esiosa, mis suhtleb kõik rakendused ja ühendused. SQL-i andmebaas, sõlme juhtelement on tootja SQL-andmebaasi ja ühenduse see näeb välja ja tunneb sama. Surface, klõpsake jaotises juhtelemendi sõlm koordinaadid kõik andmed liikumine ja arvutus kuvava paralleelselt päringuid oma jaotatud andmete põhjal. Kui saadate T-SQL-i päringu SQL-i andmebaas, juhtelemendi sõlm muudab see eraldi päringud, mis töötab samal ajal iga MSDN.

**Arvutada sõlmed:** Arvuta sõlmed olla power taha SQL-i andmebaas. Need on SQL andmebaase, mis teie andmete talletamiseks ja päringu töötlemine. Kui lisate andmeid, SQL-i andmebaas jaotab oma Arvuta sõlmed read. Arvuta sõlmed on töötajate, mis paralleelselt päringuid sooritada oma andmete põhjal. Pärast töötlemist edastavad tulemused tagasi sõlme juhtelement. Päringu lõpetamiseks sõlme kontrolli tulemuste liitmise ja tagastab lõplikud.

**Salvestusruumi:** Azure'i bloobimälu talletatakse teie andmeid. Kui teie andmeid interaktiivselt kasutada Arvuta sõlmed, nad kirjutada ja lugeda otse ja bloobimälu. Kuna Azure storage laieneb läbipaistev väga, SQL-i andmebaas saab teha sama. Kuna Arvuta ja salvestusruumi sõltumatult, saate SQL-i andmebaas automaatselt mastaapida salvestusruumi eraldi kaudu skaleerimist Arvuta ja vastupidi. Azure'i bloobimälu on ka täielikult vea salliv ja ühtlustab varundus ja taaste protsess.

**Liikumine andmeteenuse:** Andmete liikumine teenuse Sharepointil viib andmete sõlme vahel. DMS annab Arvuta sõlmed juurdepääsu neile vajalikule ühendused ja liitmised andmed. DMS ei ole Azure'i teenus. See on Windowsi teenus, mis töötab SQL-andmebaasi kõrval sõlme. Kuna DMS töötab taustal, mida ei suhelda selle otse. Juhul, kui vaatate päringu lepingud, te teate need sisaldavad mõned DMS toiminguid, kuna andmete liikumine on vaja iga päringu paralleelselt.


## <a name="optimized-for-data-warehouse-workloads"></a>Optimeeritud andmed ladu töökoormus

MPP lähenemine on abistas arvu andmete hoidmise konkreetset täitmise optimeerimine, sh:

- Jaotatud päringu optimeerija ja keerukate statistikat üle kõik andmed. Teabe kasutamine andmete suurus ja jaotus teenus on võimalik optimeerida päringute hindamise maksumus teatud jaotatud päringu toimingute abil.

- Täiustatud algoritmid ja meetodite abil integreeritud andmete liikumine protsessi arvutuste ressursid päringu vajadusel hulgas andmete tõhus teisaldamine. Nende andmete liikumine toimingute ehitatakse ja kõik andmed liikumine teenusega Optimeerimised tehakse automaatselt.

- Rühmitatud **columnstore** registrite vaikimisi. Veeru-põhise salvestusruumi abil saab SQL-i andmebaas keskmiselt 5 x tihendamise kasum üle traditsiooniline reana salvestusruumi ja kuni 10 x või rohkem päringu jõudluse tõstmiseks. Kasutusanalüüsi päringud, mis on vaja skannida suure hulga ridade töö suur columnstore registrid.


## <a name="predictable-and-scalable-performance"></a>Hõlpsalt prognoosida ja skaleeritav jõudlus

SQL-i andmebaas eraldab salvestusruumi ja Arvuta, mis võimaldab iga mastaapimiseks sõltumatult. SQL-i andmebaas saate kiiresti ja lihtsalt skaala lisada Arvuta täiendavad ressursid on hetkega. Täiendavad see on Azure'i bloobimälu kasutada. Plekid pakuvad ühed, tiražeeritud salvestusruumi, vaid ka infrastruktuuri sundimatu madalad laiendamiseks. Pilveteenuse ulatusega salvestusruumi ja Azure Arvuta kombinatsiooniga kasutamisel SQL-i andmebaas võimaldab päringu jõudluse ja salvestusruumi eest maksta, kui neid vajate. Arvuta hulk muutmine on sama lihtne nagu teisaldada Azure'i portaalis liugurit vasakule või paremale või seda saab ajastada, T-SQL-i ja PowerShelli abil.

Koos võimalus täielikult kontrollida summa Arvuta salvestusruumi sõltumatult, SQL-i andmebaas võimaldab teil oma andmebaas, mis tähendab, et te ei Arvuta eest maksta, kui te ei pea seda täielikult peatamiseks. Hoides salvestusruumi kohas, on kõik Arvuta sattuda peamine kogumi Azure, raha säästa. Kui vaja, lihtsalt jätkata selle Arvuta ja on andmete ja arvutada oma töökoormus jaoks saadaval.

## <a name="data-warehouse-units"></a>Andmete ladu ühikud

Ressursside abil oma SQL-i andmebaas on mõõdetud andmete ladu üksused (DWUs). DWUs on aluseks ressursside CPU, mälu, IOPS, mis on eraldatud oma SQL-i andmebaas nagu mõõt. DWUs arvu suurendamine suurendab ressursid ja jõudlust. Täpsemalt DWUs aitab tagada, et:

- Teil on võimalik mastaapimiseks teie andmebaas hõlpsasti, ilma muretsema aluseks oleva riistvara või tarkvara.

- Enne, kui teie andmebaas suurust muuta, saate prognoosida jõudluse parandamiseks DWU tase.

- Aluseks oleva riist- ja tarkvara oma eksemplari saate muuta või teisaldada töökoormus jõudluse mõjutamata.

- Microsoft saab teha muudatused arhitektuur teenuse mõjutamata teie töökoormus jõudlus.

- Microsoft SQL-i andmebaas, viisil, mis on paindlik ja ühtlaselt efektid süsteemi jõudluse kiiresti parandada.

Andmete ladu üksused pakuvad kolme täpne mõõdikute, mis on tugevalt seotud andmete hoidmise töökoormus jõudluse mõõt. Eesmärk on, et järgmisi olulisi töökoormus mõõdikute tihendab lineaarses DWUs, et olete valinud teie andmebaas.

**Skannimine/koondamine:** Selle töökoormus mõõdiku võtab standard andmete, päringu, mis skannib suure hulga ridu ja seejärel sooritab keerukate koondamine lao. See on ka SV- ja CPU toiming.

**Laadi:** Selle mõõdiku meetmed võimalus neelata andmete teenusesse. Laadimise on lõpetatud PolyBase laadimise tüüpilised andmehulgas Azure'i bloobimälu. Selle mõõdiku on mõeldud stress võrk ja teenuse CPU aspekte.

**Kas tabel valige (CTAS):** CTAS meetmed tabeli kopeerida. See hõlmab andmete lugemine salvestusruumist, selle seadme sõlmed üle levitamise ja kirjalikult uuesti salvestusruumi. See on mõne CPU, IO ja võrgu toiming.

## <a name="pause-and-scale-on-demand"></a>Paus ja mastaabi nõudmisel

Kui teil on vaja kiiremini tulemusi, Suurendage oma DWUs ja suurem jõudluse eest maksta. Kui vajate vähem arvutada power, oma DWUs vähendamiseks ja maksma ainult vajalik. Arvate võib teie DWUs need stsenaariumid muutmise kohta:

- Kui ei pea päringuid käivitada, võib-olla isiklike saitide õhtut või nädalavahetustel jõudeolekusse viima oma päringuid. Seejärel viige vältida DWUs kui te ei pea neid oma Arvuta ressursse.

- Kui teie süsteemi on vähese nõudmisel, kaaluge vähendamise DWU väike. Pääsete endiselt andmed, kuid märgatavat kokkuhoiu.

- Kui toiming on raske andmete laadimise või transformatsioon, soovite mastaapimiseks nii, et teie andmed on saadaval kiiremini.

Et aru saada, mis on teie optimaalne DWU väärtus, proovige skaleerimist üles ja töötab mõni päringute pärast andmete laadimine. Kuna skaleerimist on kiire, võite proovida jõudluse tund või vähem erinevate tasemete arvu.  Pidage meeles, et SQL-i andmebaas on mõeldud suurte andmehulkade töötlemine ja näha oma true võimalused mastaapimist, eriti veebisaidil suuremat skaalasid pakume, peaksite kasutama suure andmehulga, mille lähenedes või ületab 1 TB.


## <a name="built-on-sql-server"></a>Ehitatud SQL Server

SQL-i andmebaas põhineb relatsiooniline andmebaasimootor SQL Server ja sisaldab palju funktsioone, mis on ootuspärane ettevõtte andmebaas. Kui te ei tea juba T-SQL-i, on lihtne oma teadmisi edastada SQL-i andmebaas. Kas teil on täpsemad või te alles alustasite, aitavad kogu dokumentatsiooni näidetes alustada. Üldiselt Mõelge nii, et saaksime olete koostatud keele elementide SQL-i andmebaas järgmiselt:

- SQL-i andmebaas kasutab T-SQL-süntaksis palju tegevust. Samuti toetab suurt hulka traditsiooniline SQL-i importida, nt salvestatud toimingute, kasutaja määratletud funktsioonid, tabeli eraldamine, registrid ja sortimist.

- SQL-i andmebaas sisaldab ka uuem SQL serveri funktsioone, sh: rühmitatud **columnstore** registrid, PolyBase integreerimine ja andmete auditeerimine (täielik ohtude hindamine).

- Ei pruugi teatud T-SQL-i keele elemente, mis on vähem levinud jaoks andmete hoidmise töökoormus või uuemat versiooni SQL serveriga on praegu saadaval. Lisateabe saamiseks vaadake [migreerimise dokumentatsiooni][].

Transact-SQL-i ja funktsioon kriteeriumeid SQL Server, SQL-i andmebaas, SQL-andmebaasi ja Analytics platvormi süsteemi vahel, saate välja töötada lahenduse, mis sobib teie andmete vajadustele. Saate otsustada, kui hoida oma andmete põhjal jõudluse, Turve ja nõuetele skaala ja edastamine andmed vajadusel muu süsteemi vahel.

## <a name="data-protection"></a>Andmekaitse

SQL-i andmebaas talletab kõik andmed Azure Premium kohalikult liigsete salvestusruumi. Mitme sünkroonse eksemplari andmeid hoitakse kohaliku andmekeskuse selleks, et tagada läbipaistev andmekaitse korral lokaliseeritud tõrkeid. Lisaks SQL-i andmebaas automaatselt varundage aktiivne (UN peatatud) andmebaaside kasutamine Azure salvestusruumi hetktõmmiseid intervalliga. Lisateavet selle kohta, kuidas varundus ja taaste töötab leiate teemast [varundus ja taaste ülevaade][].

## <a name="integrated-with-microsoft-tools"></a>Integreeritud Microsoft tööriistad

SQL-i andmebaas ühendab paljude tööriistade selle SQL serveri kasutajate võib olla tuttav. Järgmised:

**Traditsiooniline SQL serveri tööriistad:** SQL-i andmebaas on SQL Server Analysis Services, Integration Services ja aruandlusteenuste täielikult integreeritud.

**Pilvepõhist Tööriistad:** SQL-i andmebaas saab kasutada mitmesuguseid uusi tööriistu Azure, sh andmete Factory, voo Analytics, seadme õppimine ja Power BI kõrval. Lisateavet täieliku loendi leiate teemast [integreeritud tööriistade ülevaade][].

**Kolmanda osapoole tööriistad:** Suure hulga kolmanda osapoole tööriista pakkujate on serditud integreerimine nende tööriistade abil SQL-i andmebaas. Täieliku loendi leiate teemast [SQL-i andmebaas partnerid][].

## <a name="hybrid-data-sources-scenarios"></a>Hübriidjuurutuse andmeallikad stsenaariumid

SQL-i andmebaas funktsiooniga PolyBase annab kasutajatele enneolematu võimalus andmete teisaldamiseks üle nende ökosüsteemi avamine võimalus täpsemalt hübriid stsenaariumid relatsiooniline häälestamise ja kohapealsete andmeallikate.

Polybase võimaldab teil ära kasutada eri allikatest pärit andmete T-SQL-i tuttavate käskude abil. Polybase võimaldab päringu-relatsiooniline andmetest Azure'i bloobimälu nii, nagu on tavaline tabel. Kasutage Polybase päringu-relatsiooniliste andmete või -relatsiooniliste andmete importimine SQL-i andmebaas.

- PolyBase kasutab välise tabeleid-relatsiooniliste andmete juurdepääsuks. Tabeli määratlused talletatakse SQL-i andmebaas, pääsete neile SQL-i abil ja tööriistad, nagu oleks juurdepääs tavaline relatsiooniandmete.

- Polybase on selle integratsiooni agnostik. See seab sama funktsioonid ja funktsionaalsus allikatega, mis toetab. Mitmesuguseid vorminguid, sh eraldajaga või ORC faile saab lugeda Polybase andmed.

- PolyBase saab juurdepääsu bloobimälu, mis ka kasutatakse salvestusruumi jaoks soovitud Hdinsightiga kobar. See annab teile juurdepääsu samad andmed relatsiooniline ja relatsiooniline tööriistu.

## <a name="sla"></a>SLA

SQL-i andmebaas pakub toodet taseme taseme hooldusleppe (SLA) Microsoft Online Services SLA osana. Lisateabe saamiseks külastage [SLA jaoks SQL-i andmebaas][]. SLA teavet kõigi muude toodete kohta leiate Azure'i [Taseme lepingute] lehel või alla laadida [Hulgilitsentsimise][] lehele. 

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui teate veidi SQL-i andmebaas kohta, lugege kiiresti [luua SQL-i andmebaas][] ja [laadimine näidisandmeid][]. Kui olete kasutanud Azure, leiate [Azure'i sõnastik][] kasulik nagu ilmneda uusi termineid. Või Heitke pilk mõne muu SQL-i andmete ladu ressursid.  

- [Klientide edulood]
- [Ajaveebide]
- [Soovitada funktsioone]
- [Videod]
- [Kliendi nõu meeskonnatöö ajaveebide]
- [Tugiteenuste Piletite loomine]
- [MSDN-i Foorum]
- [Virnastada ületäitumine Foorum]
- [Twitteri]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Tugiteenuste Piletite loomine]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Laadi näidisandmed]: ./sql-data-warehouse-load-sample-databases.md
[Looge SQL-i andmebaas]: ./sql-data-warehouse-get-started-provision.md
[Migreerimise dokumentatsioon]: ./sql-data-warehouse-overview-migrate.md
[SQL-i andmebaas partnerid]: ./sql-data-warehouse-partner-business-intelligence.md
[Integreeritud tööriistade ülevaade]: ./sql-data-warehouse-overview-integrate.md
[Varundus ja taaste ülevaade]: ./sql-data-warehouse-restore-database-overview.md
[Azure'i sõnastik]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Klientide edulood]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Ajaveebide]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Kliendi nõu meeskonnatöö ajaveebide]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Soovitada funktsioone]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-i Foorum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Virnastada ületäitumine Foorum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitteri]: https://twitter.com/hashtag/SQLDW
[Videod]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA jaoks SQL-andmebaas]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Hulgilitsentsimise]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Teenuse teenusetaseme lepinguid]: https://azure.microsoft.com/en-us/support/legal/sla/
