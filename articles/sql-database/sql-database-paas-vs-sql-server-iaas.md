<properties
    pageTitle="SQL-i (PaaS) andmebaasi vs SQL serveri pilve VMs (IaaS) | Microsoft Azure'i"
    description="Siit saate teada, millised cloud SQL Server suvand sobib teie taotlus: (PaaS) Azure SQL-i andmebaasi või SQL Server Azure'i Virtuaalmasinates pilveteenuses."
    services="sql-database, virtual-machines"
    keywords="SQL serveri pilves, SQL serveri pilves, PaaS andmebaasi cloud SQL serveri DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Valige SQL Server suvand pilv: (PaaS) Azure SQL-i andmebaasi või SQL Server Azure'i VMs (IaaS)

Azure'i on kaks võimalust hosting Microsoft Azure SQL serveri töökoormus.

* [Azure'i SQL-andmebaasi](https://azure.microsoft.com/services/sql-database/): A SQL-andmebaasi kohalikke pilveteenusesse, nimetatakse ka platvormi service (PaaS) andmebaasi või teenuste (DBaaS), mis on optimeeritud tarkvara-kui-a-service (SaaS) rakenduste arendamiseni andmebaasi. See pakub ühilduvuse SQL serveri enamik funktsioone. PaaS kohta leiate lisateavet teemast [mis on PaaS](https://azure.microsoft.com/overview/what-is-paas/).
* [SQL Server Azure'i Virtuaalmasinates](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL serveri installitud ja majutatud pilveteenuses kohta Windows Server Virtuaalmasinates (VM) töötavate Azure, tuntud ka kui infrastruktuuri teenus (IaaS).
SQL Server Azure'i virtuaalmasinates on optimeeritud migreerimine olemasoleva SQL serveri rakendused. Kõik versioonid ja SQL serveri on saadaval. See pakub 100% ühilduvuse SQL Server, mis võimaldab teil majutada nii palju andmebaasid on vaja ja täidesaatev rist-andmebaasitoiminguid. See pakub SQL Server ja Windows täielike.

Siit saate teada, kuidas Microsoft andmete platvormi sobib kõigi suvandite ja spikri õige valik, mis teie ettevõtte vajadustele vastavaid. Kas te tähtsuse kokkuhoiu või minimaalsete Administreerimine veel ees, see artikkel aitab teil otsustada, millist lähenemisviisi pakub tähtsate business nõuete suhtes.


## <a name="microsofts-data-platform"></a>Microsofti andmete platvormi

Üks esimese asju aru saada, Azure asutusesisese SQL serveri andmebaasi võrreldes arutelu on, et saate seda kõik. Microsofti andmete platvormi mõjutab SQL serveri tehnoloogia ja teeb selle kättesaadavaks kogu füüsilise kohapealse masinad, Privaatne cloud keskkonnas, kolmanda osapoole majutatud privaatne cloud keskkonnas ja avaliku pilve. SQL Server Azure'i virtuaalse mchines võimaldab teil läbi kohapealse ja pilveteenuse majutatud juurutuste kombinatsiooni kasutades sama valiku serveritoodete, arengu tööriistad ja teadmiste üle nendes keskkondades kordumatu ja mitmesuguse business vajadustele.

   ![Pilvepõhine SQL serveri suvandid: SQL server IaaS või SaaS SQL-i andmebaasi pilveteenuses.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Nagu joonisel näha, saate mõlema pakkumise iseloomustatud teil on üle infrastruktuur (X-teljel) korralduse tase ja astet tasuvust andmebaasi taseme konsolideerimise ja automatiseerimise (Y-teljel).

Rakenduse kujundamisel on saadaval hosting rakenduse SQL serveri osa neli võimalust:

- SQL serveri-virtualiseeritud füüsilise arvutites
- SQL serveri kohapealsetes virtualiseeritud masinad (privaatne cloud)
- SQL Server Azure'i virtuaalse masina (Microsofti pilveteenuse avaliku)
- Azure'i SQL-andmebaasi (Microsoft avaliku pilveteenus)

Järgmistest jaotistest saate teada, SQL Server Microsoft avaliku pilveteenuses kohta: Azure'i SQL-andmebaasi ja SQL Server Azure'i VMs. Lisaks, tutvuge levinud business motivaatorid määramiseks, millise variandi toimib kõige paremini rakenduse.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Azure'i SQL-andmebaasi ja SQL Server Azure'i VMs lähemalt

**Azure'i SQL-andmebaasi** on relatsiooniline andmebaasi-kui-a-teenus (DBaaS) majutatud Azure'i pilves, mis langeb valdkonna kategooriate *Tarkvara-kui-a-Service (SaaS)* ja *Platvorm-kui-a-Service (PaaS)*. [SQL-andmebaasi](sql-database-technical-overview.md) põhineb standardne riist- ja tarkvara, mis on omanik, majutatud ja mida haldab Microsoft. SQL-andmebaasiga, saate arendada otse teenuses abil sisseehitatud ja funktsioonid. Kui kasutate SQL-andmebaasiga, saate pensionitingimustega skaala üles- või suurem power ilma katkestusteta võimalusi.

**SQL serveri kohta Azure'i Virtuaalmasinates (VM)** kuulub kategooriasse valdkonna *Taristu-kui-a-Service (IaaS)* ja võimaldab käivitada SQL serveri sees virtuaalse masina pilveteenuses. Sarnaselt SQL-andmebaasiga, see on ehitatud standardne riistvara, mis on omanik, majutatud ja mida haldab Microsoft. SQL Serveri kasutamisel VM saate maksta-peab teil kiirversiooniga SQL serveri litsentsi juba lisatud pildi SQL serveri või hõlpsalt kasutada mõne olemasoleva litsentsi. Saate teha ka lihtsalt skaala-üles/alla ja peata/Jätka VM vastavalt vajadusele.

Üldiselt need kaks SQL-i suvandid on optimeeritud mitmel otstarbel:

- **SQL-andmebaasi** on optimeeritud vähendada üldine minimaalne ettevalmistamise ja palju andmebaaside haldamine. See vähendab poolelioleva halduskulud, kuna teil pole haldamiseks, mis tahes virtuaalmasinates, operatsioonisüsteemi või andmebaasi tarkvara. Teil pole haldamine täienduste, kõrge-saadavus või [varukoopiaid](sql-database-automated-backups.md). Üldine, Azure SQL-andmebaasi saab oluliselt andmebaaside arvu suurendamiseks haldab ühe IT- või ressurss.
- **SQL Server töötab Azure VMs** on optimeeritud migreerimine olemasolevate rakenduste Azure või laiendamine olemasolevate kohapealse rakenduste hübriidjuurutustes pilveteenusesse. Lisaks saate SQL serveri virtual kohapeal ja katsetamiseks traditsiooniline SQL serveri rakendused. SQL Server Azure'i VMs, kus teil on täis administraatoriõigusi üle sihtotstarbeline SQL serveri eksemplar ja pilvepõhise VM. See on parim valik, kui ettevõtte juba on see saadaval, kui soovite aga säilitada selle virtuaalmasinates ressursid. Neid võimalusi võimaldab teil luua väga kohandatud süsteemi rakenduse konkreetset jõudlus ja kättesaadavus nõuetele.

Järgmises tabelis on kokkuvõte SQL-andmebaas ja SQL Server Azure'i VMs peamised omadused:

|       | SQL-andmebaas | SQL Server Azure'i virtuaalarvuti|
| -------------- | ------------ | ---------------------- |
| **Jaoks kõige paremini:** | Uue cloud loodud rakendused on aeg piiranguid ja turundus. |Olemasoleva rakendusi, mis nõuavad kiiresti migreerimise minimaalsete muudatustega pilveteenusesse. Kiire Arendus ja testimine stsenaariumid, kui te ei soovi kohapealse mitte tekitamiseks SQL serveri riistvara osta. |
|| Meeskonnad, mida vajate andmebaasi sisseehitatud kõrge-saadavus, katastroofiabi ja versioonitäiendus. |Meeskonnad, mida saate konfigureerida ja hallata kõrge-saadavus, katastroofiabi ja SQL serveri lappimine. Mõned esitatud automaatspikri funktsioonidest oluliselt lihtsustada see. |
||Meeskonnad, mida soovite hallata aluseks operatsioonisüsteemi ja sätete konfigureerimine.| Kui teil on vaja kohandatud keskkonna täis administraatoriõigusi.|
||Andmebaaside kuni 1 TB või suurem andmebaasidele, mis võib olla [horisontaalselt või vertikaalselt liigendatud](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) skaala-out mustri abil.|SQL Serveri eksemplari kuni 64 TB salvestusruumi. Näiteks toetab nii palju andmebaaside vastavalt vajadusele. |
||[Koosteüksuse tarkvara-kui-a-Service (SaaS) rakenduste](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Migreerimine ja enterprise ja hübriid rakenduste loomine.|
|||||
|**Ressursid:**|Te ei soovi kasutada ressursid konfigureerimise ja haldamise aluseks infrastruktuuri, kuid soovite rakenduse kiht keskenduda.|Teil on mõned IT ressursid, konfigureerimise ja haldamise kohta. Mõned esitatud automaatspikri funktsioonidest oluliselt lihtsustada see.|
|**Omamise kogukulu:**|Kõrvaldab riistvara kulud ja halduskulud vähendab.|Kõrvaldab riistvara kulud.|
|**Süsteemi järjepidevuse:**|Lisaks sisseehitatud viga hälbe taristu võimalusi, pakub Azure'i SQL-andmebaasi funktsioone, näiteks [automatiseeritud varukoopiate](sql-database-automated-backups.md)punkti /-kellaaja, [Geo-taastamine](sql-database-recovery-using-backups.md#geo-restore)ja [Taastada](sql-database-recovery-using-backups.md#point-in-time-restore) [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md) järjepidevuse suurendamiseks. Lisateavet leiate teemast [SQL-andmebaasi business järjepidevuse ülevaade](sql-database-business-continuity.md).|SQL Server Azure'i VMs saate häälestada on kõrge kättesaadavus ja avariitaastet lahendus oma andmebaasi vajadustele. Seetõttu võib olla süsteemi, mis on väga optimeeritud rakenduse. Saate testimine ja käivitage failovers ise vastavalt vajadusele. Lisateabe saamiseks vt [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Hübriid pilve:**|Kohapealse rakenduse pääsevad Azure'i SQL-andmebaasi andmeid.|SQL Server Azure'i VMs, mis võib olla osaliselt pilves rakenduste ja osaliselt kohapealse. Näiteks saate pikendada oma kohapealse võrgu ja Active Directory domeeni pilve [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md)kaudu. Lisaks saate talletada Azure Storage [Azure SQL serveri andmefailid](http://msdn.microsoft.com/library/dn385720.aspx)abil kohapealse andmefailid. Lisateavet leiate teemast [Sissejuhatus SQL serveri 2014 hübriid pilve](http://msdn.microsoft.com/library/dn606154.aspx).|
||[SQL serveri tiražeerimise](https://msdn.microsoft.com/library/mt589530.aspx) toetab Tellija andmed kopeerida.|Toetab täielikult [SQL serveri tiražeerimise](https://msdn.microsoft.com/library/mt589530.aspx), [Kättesaadavusrühmad](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md), Integration Services ja Logi saatmine andmed kopeerida. Ka SQL serveri traditsiooniline varukoopiate täielikult toetatud|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Business põhjuste Azure'i SQL-andmebaasi või SQL Server Azure'i VMs

### <a name="cost"></a>Maksumus

Kas olete käivitus, mis on rihmaga raha või meeskond loodud ettevõte, mis toimib jaotises range eelarve piiranguid, piiratud vahendeid on sageli esmase juhi kui otsustada, kuidas majutada oma andmebaasi. Selles jaotises saate teada, arveldamine ja põhitõed Azure seoses kahe relatsiooniandmebaasist suvanditest litsentsimise kohta: SQL-andmebaas ja SQL Server Azure'i VMs. Saate teada ka arvutamise maksumus kokku rakenduste kohta.

#### <a name="billing-and-licensing-basics"></a>Arveldus- ja litsentsimise põhialused

**SQL-andmebaasi** müüakse klientidele teenus, mitte litsentsiga.  [SQL Server Azure'i VMs](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) müüakse kaasatud litsents, mida maksate minutis. Kui teil on mõne olemasoleva litsentsi, saate ka seda.  

Praegu on saadaval mitu teenuse astme, mis on arve tunni põhjal teenuse kiht fikseeritud ja valite jõudlusega **SQL-andmebaasi** . Lisaks on arve väljaminevate Interneti-liikluse [edastada andmeid](https://azure.microsoft.com/pricing/details/data-transfers/)korrapärane veebisaidil. Astme Basic, Standard, ja teenus on mõeldud hõlpsalt prognoosida saavutused mitme tulemuslikkuse tasemeid rakenduse tippväärtus nõuetele vastavus. Saate muuta teenuse astme jõudluse tasemed rakenduse erinevad läbilaskevõime vajadustele vastavaks vahel. Kui andmebaas on selgituseks suuremahulise ja toetama samaaegne paljud kasutajad, soovitame Premium teenuse kiht. Praeguse toetatud astme uusimat teavet teemast [Azure SQL-i andmebaasi teenuse astme](sql-database-service-tiers.md). Saate luua ka [elastne andmebaasi kaustu](sql-database-elastic-pool.md) vahel andmebaasi eksemplarid jõudluse ressursside jagamiseks.

**SQL-andmebaasi**andmebaasi tarkvara on automaatselt konfigureeritud, paigatud ja Microsoft, mis vähendab oma halduskulud versioonile. Peale selle [sisseehitatud varundamise](sql-database-automated-backups.md) võimalused aitavad teil saavutada märgatavat kokkuhoiu, eriti siis, kui teil on suur hulk andmebaasid.

**SQL Server Azure'i VMs**, saate kasutada antud platform SQL serveri pildid (mis sisaldab litsentsi) või tuua oma SQL serveri litsentsi. Toetatud SQL serveri versiooni (2008R2, 2012, 2014, 2016) ja väljaanded (arendaja, Express, Web Standard, Enterprise) on saadaval. Lisaks Too-oma-enda-litsents (BYOL) pildid on saadaval. Kui soovitud Azure'i abil pilte, funktsionaalseid hind sõltub VM suurus ja valite SQL serveri versioon. Sõltumata VM suurus või SQL serveri versioon, maksate minutis hulgilitsentsimise maksumus SQL serveri ja Windows Server, on Azure Storage hind VM ketast koos. Suvandi minutis arvelduse võimaldab SQL serveri jaoks kasutada, kui peate SQL serveri ilma osta litsentse. Kui olete tuua Azure SQL serveri litsentsi, on eest Windows Server ja salvestusruumi maksab ainult. Tuua-oma-enda litsentsimise kohta lisateabe saamiseks lugege teemat [Litsentsi mobiilsus Software Assurance Azure kaudu](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Arvutamise kokku rakenduse kulu

Kui pilve platvormi kasutuselevõtt teie rakendus sisse arengu ja haldamise kulud, millele on avalik pilv platvormi teenuse kulud.

Siin on üksikasjalikke arvutamise rakenduse Azure VMs SQL-andmebaas ja SQL Server töötab.

**Kui kasutate Azure'i SQL-andmebaasi:**

*Rakenduse kogumaksumus = tugevalt minimeeritud halduskulud + tarkvara arendamise kulud SQL-andmebaasi kulud*

**Kui kasutate SQL Server Azure'i VMs:**

*Kogumaksumus rakenduse väga minimeeritud tarkvara arendamise kulud + SQL Server ja Windows Server hulgilitsentsimise kulud + halduskulud Azure Storage kulud =*

Vaadake hinnad kohta leiate lisateavet järgmistest teemadest:

- [SQL-andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-database/)
- [Virtuaalse masina hinnad](https://azure.microsoft.com/pricing/details/virtual-machines/) [SQL-i](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) ja [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure'i hinnad kalkulaator](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] On väikese osa funktsioone SQL serveris, mis pole rakendatav või pole saadaval, kus SQL-andmebaasi. Lisateavet leiate [juhised ja SQL-i andmebaasi üldine piirangud](sql-database-general-limitations.md) ja [SQL-i andmebaasi Transact-SQL-i teave](sql-database-transact-sql-information.md) . Kui olemasoleva SQL serveri lahenduse on pilveteenusesse teisaldada, leiate [Azure'i SQL-andmebaasiga SQL serveri andmebaasi migreerimine](sql-database-cloud-migrate.md). Kui migreerite asutusesisese SQL serveri taotluse SQL-andmebaasiga, kaaluge rakenduse, pilve teenuste pakkumise võimaluste ärakasutamiseks. Näiteks võite kaaluda [Azure'i veebiteenuse rakenduse](https://azure.microsoft.com/services/app-service/web/) või [Azure'i pilveteenuste](https://azure.microsoft.com/services/cloud-services/) abil majutada oma kihid suurendamiseks kuludele.

### <a name="administration"></a>Haldus

Paljude ettevõtete jaoks, ülemineku pilveteenusesse otsus on sama palju umbes mahalaadimine keerukuse, nagu see on hind. **SQL-andmebaasi**haldab Microsoft aluseks oleva riistvara. Microsoft automaatselt tiražeerib pakkuda kõrge kättesaadavus kõik andmed, konfigureerib uuendab andmebaasi tarkvara, haldab koormusetasakaalustuseks ja läbipaistev Tõrkesiirde ei, kui server tõrke. Saate hallata oma andmebaasi jätkata, kuid te enam ei vaja andmebaasimootor, operatsioonisüsteemi või riistvara haldamiseks.  Üksused, saate jätkata haldamine näiteks andmebaasid ja sisselogimise, register ja päringu häälestamine, ja auditeerimine ja turvalisus.

**SQL Server Azure'i VMs**, peate operatsioonisüsteemi ja SQL Serveri eksemplari konfiguratsiooni üle täielik kontroll. VM, kus see on kuni teil otsustada, kui värskenduse/uuendada operatsioonisüsteemi ja andmebaasi tarkvara ja kui installida, nt viirusetõrje täiendavat tarkvara. Oluliselt lihtsustada lappimine, varukoopia ja kõrge-saadavus on esitatud mõned automatiseeritud funktsioonid. Lisaks saate määrata VM, ketast arv ja nende storage konfiguratsiooni suurust. Azure'i võimaldab VM suuruse muutmine vastavalt vajadusele. Lisateavet leiate teemast [virtuaalse masina ja pilvepõhise teenuse suurused Azure](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Teenuseleping (SLA)

Paljud IT-osakonna üles-kellaaeg kohustuste täitmise, on teenindustaseme leping (SLA) on prioriteet. Selles jaotises vaatame mis SLA kehtib iga andmebaasi majutusteenuse suvand.

**SQL-andmebaasi** Basic, Standard, ja teenuse astme pakub Microsoft saadavus SLA 99,99%. Värskeima teabe saamiseks vt [Teenuselepingu](https://azure.microsoft.com/support/legal/sla/sql-database/). Vaadake uusimat teavet SQL-andmebaasi teenuse astme ja toetatud järjepidevuse ettevõttelepingud, [Teenuse astme](sql-database-service-tiers.md).

**SQL Server töötab Azure VMs**, pakub Microsoft SLA 99,95%, mis hõlmab lihtsalt virtuaalse masina olemasolu. See SLA ei hõlma VM töötavate protsesside (nt SQL serveri korral) ja nõuab, et majutate vähemalt kaks VM juhtudel on kättesaadavus komplekti. Värskeima teabe saamiseks vt [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Andmebaasi kõrge saadavalolekut (HA) VMs sees, tuleks üks järgmistest toetatud kõrge kättesaadavus SQL Server, nt [Kättesaadavusrühmad](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx)konfigureerimine. Toetatud kõrge-saadavus suvandi abil ei paku täiendavaid SLA, kuid võimaldab teil saavutada > 99,99% andmebaasi kättesaadavus.

### <a name="market"></a>Market aeg

**SQL-andmebaasi** on õige lahendus cloud loodud rakenduste, kui arendaja tootlikkuse ja kiiresti aja market kriitiline. Funktsioonidega programmiline DBA nagu see sobib arendajad ja pilve arhitektide nagu see vähendab vajadust aluseks operatsioonisüsteem ja andmebaasi haldamine. Näiteks saate [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) ja [PowerShelli cmdlet-käskude](http://msdn.microsoft.com/library/azure/dn546726.aspx) automatiseerimiseks ja haldustoimingud tuhandete andmebaaside haldamine. Funktsioonid nagu [Elastne andmebaasi kaustu](sql-database-elastic-pool.md) võimaldavad keskenduda rakenduse kiht ja teie lahendus toomiseks turu kiiremini.

**SQL Server töötab Azure VMs** on täiuslik, kui teie olemasolevad või uued rakenduste kasutamiseks on vaja suuri andmebaase, omavahel seotud andmebaase või kõik funktsioonid SQL serveri või Windowsi juurdepääsu. Samuti on hea sobib, kui soovite migreerida olemasoleva kohapealse rakenduste ja andmebaaside Azure nimega-on. Kuna teil pole vaja muuta esitluse, rakenduse ja andmete kihid, saate säästa aega ja eelarve rearchitecting oma olemasolevat lahendust. Selle asemel saate keskenduda migreerimine kõik teie lahenduste Azure ja teha mõned täitmise optimeerimine, mis võivad nõuda Azure'i platvormi. Lisateavet leiate teemast [Jõudluse head tavad SQL Server Azure'i Virtuaalmasinates](../virtual-machines/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Kokkuvõte

Selles artiklis uurida SQL-andmebaas ja SQL serveri kohta Azure'i Virtuaalmasinates (VMs) ja mida käsitletakse levinud business motivaatorid, mis võivad mõjutada teie otsus. Järgmises soovitused teil silmas pidada Kokkuvõte:

Valige **Azure'i SQL-andmebaasi** , kui:

- On uus pilvepõhist rakenduste ära kokkuhoiu loomine ja jõudluse optimeerimine, mis pilveteenused pakuvad. Seda moodust pakub täielikult hallatud pilveteenuses eelised, aitab alumise algse aeg-to-market ja saate sisestada pikaajalise kulu optimeerimine.

- Soovite Microsoft toiminguid levinud halduse oma andmebaasid ja nõuavad tugevam kättesaadavus SLAs andmebaaside jaoks.



Valige **SQL Server Azure'i VMs** järgmistel juhtudel:

- Teil on olemasolevaid kohapealse rakendusi, mida soovite migreerida või hõlmavad pilves, või kui soovite luua enterprise rakendusi, mis on suurem kui 1 TB. Seda moodust võimaldab 100% SQL-i ühilduvust, suure andmebaasi maht, SQL Server ja Windows täielike ja secure tunneling kohapealse. Seda moodust minimeeritakse arendamise ja olemasolevate rakenduste muudatused.

- Teil on olemasolevaid IT ressursid ja lõpuks saate oma lappimine, varukoopiate ja andmebaasi kõrge kättesaadavus. Pange tähele, et mõned automatiseeritud funktsioonid oluliselt lihtsustada neid toiminguid. 


## <a name="next-steps"></a>Järgmised sammud
- Vt [SQL-andmebaasi õpetus: Azure'i portaalis minutites SQL-i andmebaasi loomine](sql-database-get-started.md) alustada SQL-andmebaasi.
- Vt [SQL-andmebaasi hinnad] (https://azure.microsoft.com/pricing/details/sql-database/).
- Teemast [Azure SQL serveri virtuaalse masina sätte](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) alustada SQL Server Azure'i VMs.
- Vt [SQL Server Azure'i Virtual arvutis: Õppeteema](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
