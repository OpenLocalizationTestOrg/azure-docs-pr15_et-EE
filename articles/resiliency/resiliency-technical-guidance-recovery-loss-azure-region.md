<properties
   pageTitle="Paindlikkust taastamise kaotsimineku on Azure piirkond tehnilised juhendid | Microsoft Azure'i"
   description="Artiklit mõistmine ja olles, mis on väga saadaval, vea salliv rakenduste kujundamine ning avariitaastet kavandamine"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Azure'i paindlikkust tehnilised juhendid: piirkond kogu teenuse häirete taastamine

Azure'i on jagatud üksused piirkondade füüsilise ja loogiline. Piirkonnas koosneb ühe või mitme andmekeskuste lähedal. Kirjutamise ajal on Azure 24 piirkondade üle maailma.

Harvaesinevate tingimustel, on võimalik, et vahendid kogu piirkonnas saate muutuvad kättesaamatuks, nt võrgu tõrgete tõttu. Või vahendid võib olla näiteks loodusõnnetuse tõttu täiesti, kaotsi. Selles jaotises selgitatakse Azure'i võimaluste loomise rakendusi, mis on jaotatud piirkondade lõikes. Selline jaotus aitab võimalus, et ühe piirkonna tõrge võib mõjutada muude piirkondade minimeerimiseks.

##<a name="cloud-services"></a>Pilveteenused

###<a name="resource-management"></a>Ressursside haldamine

Arvuta eksemplarid piirkondade jaotada loomine eraldi pilveteenuses iga piirkonna sihtkoht ja siis avaldamise juurutamise pakett iga pilveteenusesse. Pange tähele, et levitamise liikluse üle pilveteenustega eri piirkondade rakendatakse rakenduse arendaja või liikluse halduse teenusega.

Kasutamata rolli aknad juurutamiseks eelnevalt avariitaastet arvu määramiseks on oluline osa võimsuse planeerimine. Olles täielik teisene juurutamise tagab maht on juba saadaval vajaduse korral; kuid see tõhusalt kahekordistab maksumus. Levinud mustri on väike, teisene juurutamise lihtsalt piisavalt suur, kriitiliste teenuste käivitamiseks. See väike teisene juurutamise on hea mõte, nii reserveerida võimsus, ja testimiseks teisene keskkonna konfiguratsioon.

>[AZURE.NOTE]Tellimust kvoot ei garanteeri võimsus. Kvoodi on lihtsalt krediitkaardiga piirang. Läbilaskevõime tagamiseks määratletavad teenuse mudeli vajaliku arvu rollid ja rollid tuleb kasutada.

###<a name="load-balancing"></a>Laadi tasakaalustamiseks

Saldo laadimiseks liikluse piirkondade nõuab liikluse haldamise lahendus. Azure'i leiate [Azure'i liikluse haldur](https://azure.microsoft.com/services/traffic-manager/). Saate kasutada muude tootjate teenuseid, mis pakuvad sarnaseid liikluse halduse võimaluste.

###<a name="strategies"></a>Strateegiad

Paljud alternatiivne strateegiad on jaotatud Arvuta rakendamiseks piirkondades saadaval. Need tuleb kohandada teatud business nõuded ja asjaolud rakendus. Kõrge, saate meetodite jagatud järgmised Kategooriad:

  * __Juurutage uuesti katastroof__: selle lähenemisviisi rakendus on asjaomases nullist katastroofi ajal. See on sobiv mittekriitilise rakendusi, mis ei nõua tagatud taastamise aeg.

  * __Soe vaba (aktiivne/passiivne)__: teisene majutatud teenus on loodud alternatiivse piirkonnas ja rollide juurutatud selleks, et tagada minimaalne võimsus; siiski rollid ei saanud tootmise liikluse. See lähenemine on kasulik rakendustes, mille jagada liiklust piirkondade lõikes.

  * __Kuum vaba (aktiivne/aktiivne)__: rakendus on mõeldud tootmise laadimise mitme piirkondades. Suurem võimsus nõutust katastroofi taastamise eesmärgil pilveteenustega iga piirkonna võib konfigureerida. Teise võimalusena võite mastaapimiseks pilveteenuseid viia vastavalt vajadusele katastroofi ja Tõrkesiirde ajal. Seda moodust nõuab oluline investeering rakenduse kujundus, kuid see on märkimisväärsed eelised. Nendeks on pidev taastamine asukohad ja tõhusa kasutamist võimsus madal ja tagatud taastamise aeg.

Täieliku arutelu jaotatud kujundus on selle dokumendi väljapoole. Lisateabe saamiseks vt [ja kõrge-saadavus Azure'i rakenduste](https://aka.ms/drtechguide).

##<a name="virtual-machines"></a>Virtuaalmasinates

Infrastruktuuri teenus (IaaS) virtuaalmasinates (VM) on platvormi sarnaneb nagu teenuse (PaaS) arvutada paljuski taastamine. On olulised erinevused, siiski, et mõne IaaS VM koosneb nii VM ja VM ketas.

  * __Kasutage Azure varukoopia loomiseks rist piirkond varukoopiate kohase, mis on__.
  [Azure'i varundus](https://azure.microsoft.com/services/backup/) võimaldab klientidel varundamiseks rakenduse ühtsete üle mitme VM ketast ja toetavad dispersioonanalüüs varukoopiaid piirkondade lõikes. Saate seda teha, valides geo-juhendist varukoopiate hoidla loomise ajal. Pange tähele selle kopeerimine varukoopia vault peab olema konfigureeritud loomise ajal. See ei saa määrata hiljem. Kui piirkonnas on kadunud, teeb Microsoft varukoopiaid klientidele saadaval. Klientide saab taastamiseks ühte konfigureeritud taastamine punkte.

  * __Eraldi andmed kettale kettalt operatsioonisüsteem__. IaaS vms oluline argument on, et ei saa muuta operatsioonisüsteemi ketas ilma uuesti luua VM. See ei ole probleem, kui taastamise strateegia kavandamine pärast katastroofi Juurutage uuesti. Siiski võib olla probleem kui kasutate soe vaba lähenemine reserveerida võimsus. Rakendada õigesti, peab olema õige operatsioonisüsteemi ketas juurutatud esmaseid ja teiseseid asukohad ja rakenduse andmeid hoitakse eraldi kettadraivi. Võimaluse korral kasutada standard operatsioonisüsteemi konfiguratsiooni, mis on saadaval nii asukohad. Pärast Tõrkesiirde, peab siis lisage andmete ketas oma olemasoleva IaaS VMs teisene näiteks Põhiliselt. Kasutage AzCopy ketta andmete hetktõmmiste kopeerimiseks serveri saidi.

  * __Olema teadlik võimalikud järjepidevuse probleemid pärast geo Tõrkesiirde mitme VM ketast__. VM ketast rakendatakse nimega Azure Storage plekid ja on sama geo-dispersioonanalüüs omadus. Juhul, kui kasutatakse [Azure varukoopia](https://azure.microsoft.com/services/backup/) , pole garantiid järjepidevuse üle ketast, kuna geo-dispersioonanalüüs on asünkroonne ja tiražeerib sõltumatult. Üksikute VM ketast on tagatud olema ühtsete krahhi pärast geo Tõrkesiirde maakond, kuid mitte järjepidevad ketast üle. See võib põhjustada probleeme mõnel juhul (näiteks puhul ketta striping).

##<a name="storage"></a>Salvestusruumi

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Taastamine geograafilise liigne salvestusruumi bloobimälu, tabel, kuhjuda ja VM kettaruumi abil

Azure'i, plekid, tabelite, järjekorrad ja VM ketast on vaikimisi kõik geo kopeeritud. See on viidatakse nimega geograafilise liigne salvestusruumi (GRS). GRS tiražeerib andmed, mille andmepunktipaaride andmekeskuse sadu miili kindla geograafilise piirkonna lahku. GRS on mõeldud täiendavad kestvus juhuks, kui seal on suur andmekeskuse katastroof. Microsoft juhtelementide riketest ja Tõrkesiirde on piiratud katastroofide, mida algse esmane asukoht peetakse parandamatu on mõistlik aeg. Klõpsake jaotises mõnel juhul see võib olla mitu päeva. Tavaliselt kopeeritud andmete paari minutiga, kuigi sünkroonimise intervall ei hõlma veel mõne teenuseleping.

Geo Tõrkesiirde korral on ei muutu kuidas pääseb konto (URL-i ja konto võti ei muutu). Salvestusruumi konto kuvatakse, siiski teises regioonis pärast Tõrkesiirde. See võib mõjutada rakendusi, mis nõuavad piirkondliku osaleja salvestusruumi oma kontoga. Isegi jaoks teenused ja rakendused, mis ei nõua salvestusruumi konto sama andmekeskuses, rist-andmekeskuse latentsus ja läbilaskevõime kulude võib olla silmapaistvate põhjust liikluse ajutiselt Tõrkesiirde alale liikumine. See võiks teguri üldise katastroofi taastamise strateegia.

Lisaks automaatselt Tõrkesiirde esitatud GRS, lisanud Azure'i teenus, mis annab teile lugemisõigus andmete talletuskoht teisene koopia. Seda nimetatakse lugemisõigus – geograafilise liigne salvestusruumi (RA GRS).

GRS nii RA-GRS salvestusruumi kohta leiate lisateavet teemast [Azure Storage kopeerimine](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>Geo-Dispersioonanalüüs piirkond vastendused:

See on oluline teada, kui teie andmed on geo kopeeritud, et teada saada, kus juurutada muude versioonide piirkondliku osaleja koos salvestusruumi nõudvate andmete. Järgmine tabel sisaldab sidumiste esmaseid ja teiseseid asukoht.

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Geo-Dispersioonanalüüs hinnad:

Praeguse hinnad Azure Storage sisaldub Geo-dispersioonanalüüs. Seda nimetatakse geograafilise liigne salvestusruumi (GRS). Kui te ei soovi oma andmete geo kopeeritud saate keelata geo-dispersioonanalüüs teie konto jaoks. Seda nimetatakse kohalikult liigsete salvestusruumi ja see on lisandu võrreldes GRS hinnaga.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Kui geo Tõrkesiirde ilmnes määratlemine

Geo Tõrkesiirde juhul see postitatud [Azure'i teenuste seisundi armatuurlaud](https://azure.microsoft.com/status/). Rakenduste saate kasutada automatiseeritud abinõusid, tuvastamine, aga geo piirkonna oma konto salvestusruumi jälgides rakendada. Seda saab kasutada muude taastamine toiminguid, nagu aktiveerimise Arvuta ressursside, kuhu teisaldada oma salvestusruumi geo piirkonnas käivitamiseks. Saate teha päringu nii: Teenusehaldus API, kasutades [Saada salvestusruumi konto atribuudid](https://msdn.microsoft.com/library/ee460802.aspx). Oluline atribuudid on:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>VM ketast ja geo-Tõrkesiirde

Nagu on kirjeldatud jaotises VM kettale, pole garantiid andmete järjepidevuse üle VM ketast pärast Tõrkesiirde. Varukoopiate õigsuse tagamiseks tuleks kasutada näiteks andmete kaitse Manager varukoopia toote ja rakenduse andmete taastamine.

##<a name="database"></a>Andmebaasi

###<a name="sql-database"></a>SQL-andmebaas

SQL Azure'i andmebaas pakub kahte tüüpi taastamine: Geo-taastamine ja aktiivse Geo-kopeerimine.

####<a name="geo-restore"></a>Geo-taastamine

Andmebaasidega, Basic, Standard, ja pakutakse [Geo-Taasta](../sql-database/sql-database-recovery-using-backups.md#geo-restore) . Kui andmebaas on saadaval juhtum ala, kus teie andmebaas on majutatud pakub taastamine vaikesuvand. Sarnaselt punkti õigel ajal taastada, Geo-taastamine tugineb andmebaasi varundamine geograafilise liigne Azure laos. See taastab geo kopeeritud varukoopia ja seetõttu on olles salvestusruumi katkestuste esmane piirkonna. Rohkem üksikasju, leiate [Azure'i SQL-andmebaasi- või teisese Tõrkesiirde taastamine](../sql-database/sql-database-disaster-recovery.md).

####<a name="active-geo-replication"></a>Aktiivse Geo-kopeerimine

[Aktiivse Geo-kopeerimine](../sql-database/sql-database-geo-replication-overview.md) on saadaval kõigi andmebaasi astme. See on mõeldud rakendusi, mis on tõhusamaks taastamise nõuded kui Geo-taastamine saate pakkuda. Aktiivse Geo-kopeerimine abil saate luua kuni nelja loetav sekundaaride erinevate piirkondade serverites. Saate algatada Tõrkesiirde kõiki selle sekundaaride. Lisaks saab aktiivse Geo-kopeerimine toetavad rakenduse täiendamine või ümberpaigutamise stsenaariumid, samuti laadimine jaoks kirjutuskaitstud töökoormus. Lisateavet leiate teemast [konfigureerimine Geo-kopeerimise](../sql-database/sql-database-geo-replication-portal.md) ja nurjumise [üle teise andmebaasi](../sql-database/sql-database-geo-replication-failover-portal.md). Vaadake [kujundus rakenduse puhul pilveteenuste avariitaastet kasutades aktiivse Geo kopeerimine SQL-andmebaasis](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) ja [pilveteenuse rakenduste haldamine jooksva uuendamine kasutades SQL-i andmebaasi aktiivse Geo kopeerimine](../sql-database/sql-database-manage-application-rolling-upgrade.md) täpsemat teavet kujundus ja rakendada rakendused ja rakenduste versioonitäienduse ilma tööseisakute.

###<a name="sql-server-on-virtual-machines"></a>SQL Server Virtuaalmasinates

Erinevaid võimalusi on saadaval ja SQL Server 2012 (ja uuemad versioonid) töötab Azure'i Virtuaalmasinates kõrge kättesaadavus. Lisateabe saamiseks vt [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

##<a name="other-azure-platform-services"></a>Azure'i platvormi muude teenustega

Kui proovite käivitada oma pilveteenuses mitme Azure'i piirkondades, peate arvesse võtma mõju iga teie sõltuvused. Järgmistes jaotistes teenuse juhiseid eeldab, et peate kasutama sama Azure'i teenus mõne alternatiivse Azure andmekeskuse. See hõlmab nii konfigureerimine ja andmete-dispersioonanalüüs tööülesanded.

>[AZURE.NOTE]Mõnel juhul aitab leevendada teenuse kohased katkestuste, mitte kogu andmekeskuse sündmuse järgmist. Rakenduse seisukohalt teenuse kohased katkestuste võib olla täpselt nagu piirata ja nõuaks ajutiselt migreerimine teenuse alternatiivse Azure alale.

###<a name="service-bus"></a>Teenuse siini

Azure'i teenus siini kasutab kordumatut nimeruumi ulatuvad Azure regioonid. Nii esimene tingimus on vaja teenuse siini nimeruumid alternatiivse piirkonna häälestamine. Kuid on ka kaalutluste kohta selle kestvus järjekorras olevad sõnumid. On mitu strateegiad imitatsiooniga sõnumite Azure piirkondade lõikes. Lisateavet nende dispersioonanalüüs strateegiad ja muud katastroofi taastamine strateegiad leiate teemast [head tavad isoleeriva rakenduste teenuse siini katkestuste eest katastroofid](../service-bus-messaging/service-bus-outages-disasters.md). Muude kättesaadavus kaalutluste kohta leiate teemast [Teenuse siini (kättesaadavuse)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>Rakenduse teenus

Migreerimine Azure'i rakenduse teenuse rakendus, näiteks veebirakenduste või mobiilirakenduste, teisene Azure alale, peab teil olema saadaval avaldamine veebisaidil varukoopia. Kui soovitud katkestuste ei hõlma kogu Azure andmekeskuses, võib olla võimalik kasutada FTP varukoopiat saidi sisu alla laadida. Seejärel luua uue rakenduse alternatiivse piirkonna juhul, kui olete eelnevalt reserveerida võimsus seda teinud. Saidi avaldamine uues ja tehke konfiguratsiooni vajalikud muudatused. Need muudatused võivad sisaldada andmebaasi ühendusstringi või muud piirkonnakohase sätted. Vajaduse korral saidi SSL-serdi lisamine ja muutmine DNS-i CNAME-kirje, et kohandatud domeeni nimi, mis osutab laiendatud Azure Web App URL.

###<a name="hdinsight"></a>Hdinsightiga

Azure'i bloobimälu Vaikimisi talletatakse Hdinsightiga seotud andmed. Hdinsightiga nõuab, et Hadoopi kobar, MapReduce töid asuma samas piirkonnas salvestusruumi konto, mis sisaldab andmeid, mis on analüüsitud koostööd. Kui saadaval Azure Storage geo-dispersioonanalüüs funktsiooni kasutamiseks pääsete andmete teisene ala, kus andmed on kopeeritud, kui mingil põhjusel esmane piirkond pole enam saadaval. Saate luua uue Hadoopi kobar ala, kus andmed on kopeeritud ja jätkake selle töötlemiseks. Muude kättesaadavus kaalutluste kohta leiate teemast [Hdinsightiga (kättesaadavuse)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>SQL-i teatamine

Sel ajal, taastamisel, mis on Azure piirkond kaotsimineku nõuab mitu SQL-i aruandlusteenuste eksemplari Azure eri piirkondadest. SQL-i aruandlusteenuste juhul tuleks kasutada samad andmed ja andmeid peaks olema oma taastamine õnnetuste kavandamine. Saate säilitada välise varukoopiaid iga aruande RDL-faili.

###<a name="media-services"></a>Media Services

Azure Media Servicesi on erinevate taastamise lähenemine kodeerimise ja streaming. Tavaliselt streaming on veel kriitiline piirkondliku katkestuste ajal. Ettevalmistamiseks see peaks olema Media Servicesi konto kahe eri Azure piirkondades. Mõlemad piirkonnad peaks asuma encoded sisu. Ajal, saate suunata alternatiivse piirkond streaming liikluse. Mis tahes Azure'i piirkonnas saab teha kodeerimine. Kui kodeering on kiirete, nt live sündmus töötlemise ajal, peate olema valmis mõne alternatiivse andmekeskuse tööde edastamiseks kasutamisel.

###<a name="virtual-network"></a>Virtuaalse võrgu

Failid pakuvad kiireim viis virtuaalse võrgu alternatiivse Azure piirkonnas. Pärast virtuaalse võrgu konfigureerimist esmane Azure piirkonna praeguse võrgu network configuration faili [eksportida virtuaalse võrgu sätteid](../virtual-network/virtual-networks-create-vnet-classic-portal.md) . Korral on katkestuste esmane piirkonna, [virtuaalse võrgu taastamine](../virtual-network/virtual-networks-create-vnet-classic-portal.md) salvestatud konfigureerimise faili. Seejärel konfigureerimine muude pilveteenustega, virtuaalmasinates või asutusesiseses töötamine uue virtuaalse võrgu sätteid.

##<a name="checklists-for-disaster-recovery"></a>Avariitaastet kontroll-loendid

##<a name="cloud-services-checklist"></a>Cloud Services kontroll-loend

  1. Vaadake üle selle dokumendi jaotise pilveteenustega.
  2. Rist-piirkond katastroofi taastamise strateegia loomine.
  3. Mõista kompromisse rakenduses broneerimist võimsus alternatiivse piirkondades.
  4. Kasutage liikluse marsruutimise tööriistad, nt Azure liikluse haldur.

##<a name="virtual-machines-checklist"></a>Virtuaalmasinates kontroll-loend

  1. Vaadake üle Virtuaalmasinates jaotises selle dokumendi.
  2. [Azure varukoopia](https://azure.microsoft.com/services/backup/) abil saate luua rakenduse ühtsete varukoopiate piirkondade lõikes.

##<a name="storage-checklist"></a>Salvestusruumi kontroll-loend

  1. Vaadake üle selle dokumendi jaotise salvestusruumi.
  2. Keelake geo-dispersioonanalüüs salvestusruumi ressursid.
  3. Alternatiivsete piirkond geo-kopeerimise mõista Tõrkesiirde korral.
  4. Saate luua kohandatud varukoopia strateegiad kasutaja kontrollitud Tõrkesiirde strateegiad.

##<a name="sql-database-checklist"></a>SQL-andmebaasi kontroll-loend

  1. Vaadake üle selle dokumendi jaotist SQL-i andmebaasi.
  2. Kasutage [Geo-taastada](../sql-database/sql-database-recovery-using-backups.md#geo-restore) või [Geo-Dispersioonanalüüs](../sql-database/sql-database-geo-replication-overview.md) .

##<a name="sql-server-on-virtual-machines-checklist"></a>SQL Server Virtuaalmasinates kontroll-loend

  1. Vaadake üle selle dokumendi jaotist Virtuaalmasinates SQL Server.
  2. Kasutage rist-piirkond Kättesaadavusrühmad või andmebaasi peegeldus.
  3. Vaheldumisi kasutada varundamine ja taastamine Bloobivahemälu salvestusruumi.

##<a name="service-bus-checklist"></a>Teenuse siini kontroll-loend

  1. Vaadake üle teenuse siini jaotises selle dokumendi.
  2. Alternatiivsete piirkonnas konfigureerida teenuse siini nimeruum.
  3. Kaaluge kohandatud dispersioonanalüüs strateegiad sõnumite piirkondade lõikes.

##<a name="app-service-checklist"></a>Rakenduse teenuse kontroll-loend

  1. Vaadake üle selle dokumendi jaotist rakenduse teenuse.
  2. Säilitada saidi varukoopiate väljaspool esmane piirkond.
  3. Osalise katkestuste korral üritavad praeguse saidi FTP toomiseks.
  4. Lepingu juurutada saidil uuel või olemasoleval saidil alternatiivne piirkonnas.
  5. Leping konfiguratsioonimuudatuste rakenduse-ja DNS-i CNAME-kirje.

##<a name="hdinsight-checklist"></a>Hdinsightiga kontroll-loend

  1. Vaadake üle Hdinsightiga jaotises selle dokumendi.
  2. Luua uus Hadoopi klaster piirkonna kopeeritud andmed.

##<a name="sql-reporting-checklist"></a>SQL-i aruandluse kontroll-loend

  1. Vaadake üle selle dokumendi jaotist SQL-i teatamine.
  2. Alternatiivsete SQL-i aruandlusteenuste eksemplari teises regioonis säilitada.
  3. Säilitada eraldi plaani kopeerida sihtkohas selle alale.

##<a name="media-services-checklist"></a>Media Servicesi kontroll-loend

  1. Vaadake üle Media Services jaotises selle dokumendi.
  2. Looge konto Media Servicesi alternatiivse piirkonnas.
  3. Kodeerida mõlemad piirkonnad streaming Tõrkesiirde toetada sama sisu.
  4. Edastab kodeering tööd alternatiivse piirkonnale teenuse häirete korral.

##<a name="virtual-network-checklist"></a>Virtuaalse võrgu kontroll-loend

  1. Vaadake üle virtuaalse võrgu jaotises selle dokumendi.
  2. Kasutage eksporditud virtuaalse võrgu sätteid uuestiloomiseks teise piirkond.

##<a name="next-steps"></a>Järgmised sammud

See artikkel on osa sarjast värskendustest [Azure paindlikkust tehnilised juhendid](./resiliency-technical-guidance.md). Selle sarja järgmise artikkel keskendub [on kohapealse andmekeskuse Azure taastamine](./resiliency-technical-guidance-recovery-on-premises-azure.md).
