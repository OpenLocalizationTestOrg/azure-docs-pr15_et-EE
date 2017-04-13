<properties
   pageTitle="SQL Azure'i andmebaasi Azure juhtumianalüüsi - Umbraco | Microsoft Azure'i"
   description="Siit saate teada, kuidas Umbraco kasutab SQL-andmebaasi kiiresti ette valmistada ja skaala teenused tuhandete rentnikud pilveteenuses"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco kasutab Azure'i SQL-andmebaasi, et kiiresti näha ja skaala teenuste tuhandete rentnikud pilveteenuses

![Umbraco Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco on populaarsed avatud allika-sisuhalduse süsteem (cm), mis saab käitada midagi small turunduskampaania või voldiku saidilt keerukate rakendustele Fortune 500 ettevõtete ja globaalse media veebilehed. 

> "Meil on üsna suur ühenduse arendajad, kes kasutavad süsteemi üle 100 000 arendajatele meie Foorumid ja rohkem kui 350.000 saitidel, mis on reaalajas, kus töötab Umbraco."

> – Morten Christensen tehnilise juhi, Umbraco

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Haldamise hõlbustamiseks klient, Umbraco lisatud Umbraco-kui-a-Service (UaaS): tarkvara-kui-a-service (SaaS) pakkumise, mis välistab kohapealse juurutuse, pakub sisseehitatud skaleerimist ja eemaldab kohal haldus võimaldab arendajatel keskenduda toote uuenduste, mitte lahenduste haldus. Umbraco suudab pakkuda tuginedes paindlik platvorm-kui-a-service (PaaS) mudeli pakutud Microsoft Azure'i kõik need eelised.

UaaS võimaldab kasutada Umbraco CMS funktsioonid, mis olid varem nende varjatud SaaS kliendid. Need kliendid on ette valmistatud koos töökeskkonna CMS, mis sisaldab tootmisandmebaasile. Kliente saate lisada kuni kahe uute andmebaaside ja lavastus keskkonnas, vastavalt oma vajadustele. Uude keskkonda nõudmisel automaatse protsessi määrab kliendi andmebaasi automaatselt. Uue andmebaasi on valmis, sekundites, kuna andmebaas on juba eelnevalt ette valmistatud, Umbraco: Azure'i elastne kogumi saadaval andmebaasid (vt joonis 1).


![Umbraco ettevalmistamise elutsükkel](./media/sql-database-implementation-umbraco/figure1.png)

Joonis 1. Kui teenus (UaaS) Umbraco elutsükli ettevalmistamine
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure'i elastne kaustu ja automatiseerimise haldamise hõlbustamiseks

Azure'i SQL-andmebaasi ja muude Azure teenustega, Umbraco kliendid saavad osutamise oma keskkonnas ja Umbraco saate hõlpsalt jälgida ja andmebaaside haldamine intuitiivne töövoo käigus.

1.  Säte

    Umbraco säilitab võimsus 200 saadaval eelnevalt ettevalmistatud andmebaaside elastne kaustu. Kui uus klient registreerub UaaS, annab Umbraco määrates neile andmebaasi kättesaadavus rakenduskausta CMS uude keskkonda peaaegu reaalajas kliendile.

    Selle jõudmisel kohapeal on kättesaadavus on loodud uue elastne rakenduskausta ja uute andmebaaside on eelnevalt ettevalmistatud, mis määratakse klientide vastavalt vajadusele.

    Rakendamine on täielikult automatiseeritud C# haldamine teekide ja Azure teenuse siini järjekorrad.

2.  Kasutada

    Klientide üks kuni kolm keskkonnas (tootmise, lavastus ja/või arengu), iga abil oma andmebaasi. Klientide andmebaasid on elastne andmebaasi kaustadesse, mis võimaldab Umbraco tõhusa skaleerimist ilma ettevalmistamise üle anda.

    ![Umbraco projekti ülevaade](./media/sql-database-implementation-umbraco/figure2.png)

    ![Umbraco projekti üksikasjad](./media/sql-database-implementation-umbraco/figure3.png)

    Joonis 2. Umbraco-kui-a-Service (UaaS) kliendi veebisait, näitab projekti ülevaade ja üksikasjad

    Azure'i SQL-andmebaasi kasutab vaja tegelike andmebaasitoiminguid suhteline power esindavad andmebaasi tehingu üksused (DTUs). UaaS klientide puhul andmebaaside tavaliselt töötamist umbes 10 DTUs, kuid iga on elastsust mastaapimiseks nõudmisel. Mida tähendab, et UaaS saate tagada, et kliendid on alati vajalikud ressursid, isegi tippväärtus ajal. Näiteks Viimatised pühapäev Sport sündmuse ajal ühe UaaS kliendi kogenud andmebaasi peaks kuni 100 DTUs mängu kestel. Azure'i elastne kaustu võimalikuks Umbraco toetama selle kõrge nõudmisel ilma jõudluse vähenemine.

3.  Jälgimine

    Umbraco kuvari andmebaasi tegevuse armatuurlaudade Azure portaali, koos kohandatud meiliteatiste kasutamine.

4.  Katastroofiabi

    Azure'i pakub avariitaastet (DR) kaks võimalust: aktiivse Geo-kopeerimine ja Geo-taastamine. DR suvand, mis tuleb valida mõni ettevõte sõltub selle [järjepidevuse eesmärke](sql-database-business-continuity.md).

    Aktiivse Geo-kopeerimine pakub kiireima vastuse tase tööseisakute korral. Aktiivse Geo-kopeerimine abil saate luua kuni nelja loetavad sekundaaride serverites eri regioonide ja seejärel saate algatada Tõrkesiirde kõiki selle sekundaaride tõrke korral.

    Umbraco ei nõua Geo-Dispersioonanalüüs, kuid see ära Azure'i Geo-taastamine tagada minimaalne tööseisakute korral on katkestuste. Andmebaasi varundamine geograafilise liigne Azure laos tugineb Geo-taastamine. Mis võimaldab kasutajatel taastamine varukoopia põhjal, kui on mõni katkestuste esmane piirkonna.

5.  De säte

    Projekti keskkonnas kustutamisel eemaldatakse kõik seotud andmebaasid (lavastus või reaalajas arengu) Azure'i teenus siini järjekorda hüljata. See automatiseeritud protsessi taastab kasutamata andmebaaside Umbraco's elastne andmebaasi-saadavus rakenduskausta, muutes need kättesaadavaks tulevaste ettevalmistamise säilitades maksimaalset kasutamist.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Elastne kaustu luba UaaS lihtsalt skaala

Ära Azure elastne andmebaasi kaustu, Umbraco saab optimeerida jõudlust oma klientidele ilma üle või alla säte. Umbraco on praegu ligi 3000 andmebaaside üle 19 elastne andmebaasi kaustu, võimalus hõlpsasti mastaapimiseks vajadusel majutada oma olemasolevad 325,000 kliendid või uutele klientidele, kes on valmis kasutama CMS pilveteenuses.

Tegelikult vastavalt Morten Christensen, tehniline juhtimine veebisaidil Umbraco, "UaaS on nüüd probleeme kasvu umbes 30 uutele klientidele päevas. Kliendid on rahul, on võimalik ettevalmistamise uute projektide sekundites, Kiire avaldamine värskendused saavad reaalajas saitide arenduskeskkond, mis abil 'ühe klõpsuga juurutamise' mugavuse ja muuta nii kiiresti kui need leiate vigu."

Kui kliendi ei vaja enam teise ja/või kolmanda keskkonnas, saate selle eemaldada lihtsalt nendes keskkondades. Mis vabastada ressursse, mida saate kasutada teiste klientide Umbraco elastne andmebaasi-saadavus rakenduskausta osana.

![Umbraco juurutamise arhitektuur](./media/sql-database-implementation-umbraco/figure4.png)

Joonis 3. UaaS juurutamise arhitektuur Microsoft Azure

##<a name="the-path-from-datacenter-to-cloud"></a>Andmekeskuse Cloud teed

Kui Umbraco arendajad esialgu otsust liikumiseks SaaS mudeli, teadsid, et need oleks vaja tulusate ja scalable võimalus rajamise teenust.

> "Elastne andmebaasi kaustu on täiuslik sobivad meie SaaS pakkumine, kuna me saate helistada võimsus üles vastavalt vajadusele. Ettevalmistamise on lihtne ja häälestamist, soovime säilitada kasutamine maksimaalne."

> – Morten Christensen tehnilise juhi, Umbraco

"Me kalendriüksusi kulutada aega meie klientide probleemide lahendamiseks, mitte taristu haldamise. Me kalendriüksusi hõlpsalt klientide saada enamik väärtuse"on kirjas Niels Hartvig asutaja Umbraco. "Me algselt lugeda majutusteenuse end serverid, kuid võimsuse planeerimine oleks õudusunenägu." Juhuslikult, Umbraco ei kasutata andmebaasi kõik administraatorid, mis rõhutab võtme kuvandi UaaS kasutamise kohta.

Üks oluline eesmärk Umbraco arendajatele oli võimalda UaaS klientide ettevalmistamise keskkonnas kiiresti ja ilma mahupiirangud. Kuid sihtotstarbeline majutatud teenuse Umbraco andmekeskuste oleks tulnud palju liigse võimet toime puruneb töötlemine. Millele oleks tulnud lisamise märkimisväärne Arvuta taristu, mis peaks olema regulaarselt vähe.

Lisaks Umbraco arendusmeeskonnale soovinud, et neid uuesti kasutada nii palju oma olemasoleva koodi, kui võimalik lahendus. Kui Umbraco arendaja Mikkel Madsen olekud, "Meil oli rahul Microsoft arengu tööriistad, mida me oli juba tuttav, näiteks Microsoft SQL Server, Microsoft Azure'i SQL-andmebaasi, ASP.net-i ja teenusekomplekti Internet Information Services (IIS). Enne mõne IaaS või mõne PaaS investeerida cloud lahenduse me kalendriüksusi veenduge, et see toetab meie Microsoft ja platvormid, nii, et me ei oleks suuri muudatusi teha meie kood alus. "

Vasta selle kriteeriumid, et otsite Umbraco pilvepartner, kellega järgmist:

-   Piisav ja töökindluse
-   Nii, et Umbraco insenerid ei kasutajanimeks oma arenduskeskkond täielikult leiutada oleks Microsoft arengu tööriistad, kasutajatugi
-   Kohaloleku kõikide geograafilised riikides, kus UaaS konkureerib (ettevõtetele vajadust tagada, et nad pääsevad oma andmetele kiiresti ja nende talletamise kohta, kuhu vastab nende piirkondliku regulatiivsetele nõuetele)

##<a name="why-umbraco-chose-azure-for-uaas"></a>Miks Umbraco valitud Azure'i UaaS

Vastavalt Morten Christensen "kõiki meie võimalusi arvestades me valitud Azure, kuna see täidetud kõik meie kriteeriumid hallatavust ja skaleeritavus tuttava ja tasuvust. Meil loodud keskkonnas sisse Azure VMs ja iga keskkond on oma Azure'i SQL-andmebaasi eksemplar, kõik eksemplarid elastne andmebaasi kaustadesse. Pakume andmebaaside vahel arengu, lavastus ja reaalajas keskkonnas, eraldades, saate oma klientidele, töökindlate jõudluse eraldamise sobitada skaala – suured võit. "

Morten ei lahene, "enne oli ettevalmistamise serverid veebiandmebaaside käsitsi. Nüüd me ei pea mõelda. Kõik on automatiseeritud – kaudu ettevalmistamine Kettapuhastus. "

Morten on ka rahul skaleerimise võimaluste Azure'i järgi. "Elastne andmebaasi kaustu on täiuslik sobivad meie SaaS pakkumine, kuna me saate helistada võimsus üles vastavalt vajadusele. Ettevalmistamise on lihtne ja häälestamist, soovime säilitada kasutamine maksimaalne." Morten olekud, "lihtsad elastne kaustu, koos teenuse taseme vastavalt DTUs, tagamine annab meile uue ressursi kaustu nõudmisel ette valmistada. Hiljuti ühte suuremat klientide haripunkti 100 DTUs reaalajas keskkonnas. Azure'i kasutamisel meie elastne kaustu esitada ressursse, mis neid vaja reaalajas ilma prognoosida DTU nõuded kliendi andmebaasid. Lihtsalt panna meie kliendid saavad pöörde aega, mida nad ootavad ja saame vastata meie jõudluse teenusetaseme lepinguid."

Mikkel Madsen summeerib: "Olen edestas võimsaid Azure algoritmi, mis ühendab SaaS stsenaarium (kasutuselevõtt uutele klientidele reaalajas tasandil) meie rakenduse tüübile (eelnevalt ettevalmistamise andmebaaside nii arendamise ja live) peal tehnoloogial, (kasutades Azure teenuse siini järjekorrad SQL Azure'i andmebaas koos)."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Azure'i, mille UaaS on üle klientide ootustele

Valides Azure kui selle pilvepartner, Umbraco alates anda UaaS kliendid optimeeritud sisuhalduse jõudlust ilma IT-ressursside investeering ise hostitud lahenduse nõutav. Morten ütleb, "Soovime kuulda arendaja mugavuse ja skaleeritavus, mis Azure'i annab meile ja kliendid on põnevil funktsioone ja töökindluse. Üldiselt on hea võit meile!"
 
## <a name="more-information"></a>Lisateave

- Azure'i elastne andmebaasi kaustu kohta leiate lisateavet teemast [elastne andmebaasi kaustu](sql-database-elastic-pool.md).

- Azure'i teenus siini kohta leiate lisateavet teemast [Azure teenuse siini](https://azure.microsoft.com/services/service-bus/).

- Web rollid ja töötaja rolli kohta leiate lisateavet teemast [töötaja rollid](../fundamentals-introduction-to-azure.md#compute). 

- Virtuaalne võrgunduse kohta leiate lisateavet teemast [virtuaalse võrgunduse](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Varundamise ja taastamise kohta leiate lisateavet teemast [järjepidevuse](sql-database-business-continuity.md).  

- Ppols jälgimise kohta leiate lisateavet teemast [jälgimise kaustu](sql-database-elastic-pool-manage-portal.md). 

- Kui teenus Umbraco kohta leiate lisateavet teemast [Umbraco](https://umbraco.com/cloud).

