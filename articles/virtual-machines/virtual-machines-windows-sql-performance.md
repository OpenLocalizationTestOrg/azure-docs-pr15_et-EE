<properties
    pageTitle="Jõudluse head tavad SQL serveri | Microsoft Azure'i"
    description="SQL Server Microsoft Azure'i VMs jõudluse optimeerimine pakub parimaid tavasid."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Jõudluse SQL Server Azure'i Virtuaalmasinates head tavad

## <a name="overview"></a>Ülevaade

Selles teemas antakse head tavad optimeerimine SQL serveri jõudluse Microsoft Azure virtuaalse masina. Kui töötab SQL Server Azure'i Virtuaalmasinates, soovitame jätkamist kasutades sama andmebaasi jõudluse häälestamine suvandid, mis on rakendatavad SQL serveri kohapealse serveri keskkonnas. Relatsiooniandmebaasist avaliku pilveteenuses jõudlus sõltub siiski palju tegureid, nt virtuaalse masina suurust ja andmete ketast konfiguratsiooni.

Kui loote SQL serveri pilte, [kaaluge ettevalmistamise oma VMs Azure'i portaalis](virtual-machines-windows-portal-sql-server-provision.md). SQL serveri VMs ette valmistatud portaalis koos ressursihaldur rakendada kõigi järgmistest headest tavadest, sh salvestusruumi konfigureerimine.

See artikkel keskendub Azure VMs SQL serveri *parimaid* tulemusi saada. Kui teie töökoormus lihtsamaks, teil võib nõuda iga optimeerimine, mis on loetletud allpool. Võtke arvesse oma vajadustele jõudlus ja töökoormus mustrite soovituste hinnata.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Kiire kontroll-loend

Kiire kontroll-loend, SQL Server Azure'i Virtuaalmasinates optimaalse jõudluse tagamiseks on:

|Ala|Optimeerimine|
|---|---|
|[VM suurus](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) või uuemas versioonis SQL-ettevõtte väljaanne.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) või uuemas versioonis SQL Standard- ja Web versioonid.|
|[Salvestusruumi](#storage-guidance)|Kasutage [Premium mälu](../storage/storage-premium-storage.md). Kindlad soovitatakse ainult katsetamiseks arendaja.<br/><br/>Hoidke piirkonna [salvestusruumi konto](../storage/storage-create-storage-account.md) ja SQL serveri VM.<br/><br/>Azure'i keelamine [geograafilise liigne salvestusruumi](../storage/storage-redundancy.md) (geo dispersioonanalüüs) salvestusruumi kontol.|
|[Ketast](#disks-guidance)|Kasutage minimaalselt 2 [P30 ketast](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (logifailide 1; 1 ja TempDB PST-failid).<br/><br/>Vältige operatsioonisüsteemi või ajutiste ketast andmebaasi salvestusruumi või logimine.<br/><br/>Luba lugeda majutusteenuse andmefailid ja TempDB ketta vahemällu.<br/><br/>Luba majutusteenuse logifaili kontrollimiseks klõpsake vahemällu.<br/><br/>Tähtis: SQL Serveri teenuse peatamine on Azure VM ketta vahemälu sätete muutmisel.<br/><br/>Mitme Azure'i andmed ketast saada suurema IO läbilaskevõime triip.<br/><br/>Vormingu dokumenteerida eraldatud suurused.|
|[I/O](#io-guidance)|Andmebaasi lehe tihendamise lubada.<br/><br/>Lubada kiirotsingu faili lähtestamine andmefailide.<br/><br/>Piirata või keelata autogrow andmebaasi.<br/><br/>Keelake autoshrink andmebaasi.<br/><br/>Kõik andmebaasid liikuda andmete ketast, sh süsteemi andmebaasid.<br/><br/>SQL Serveri tõrge Logi- ja faili kataloogide andmete ketast liikumine<br/><br/>Häälestamise varunduse ja andmebaasi failide asukohad.<br/><br/>Luba lehtede lukustatud.<br/><br/>SQL serveri jõudluse parandused rakendada.|
|[Teatud funktsioonide](#feature-specific-guidance)|Varundage otse bloobimälu.|

Lisateavet selle kohta, *Kuidas* ja *miks* neid Optimeerimised teha läbi üksikasjad ja järgmistes jaotistes toodud juhised.

## <a name="vm-size-guidance"></a>VM suurus juhised

Jõudluse tundliku rakenduste, on soovitatav kasutada [virtuaalmasinates suurused](virtual-machines-windows-sizes.md)järgmised:

- **SQL Server Enterprise Edition**: DS3 või uuem versioon

- **SQL serveri Standard- ja Web väljaanded**: DS2 või uuem versioon


## <a name="storage-guidance"></a>Salvestusruumi juhised

DS-sarja (koos DSv2-sari ja GS-sarja) VMs tugi [Premium mälu](../storage/storage-premium-storage.md). Premium mälu on soovitatav kõik tootmise töökoormus.

> [AZURE.WARNING] Kindlad on erineva latentsused ja läbilaskevõime ja soovitatakse ainult katsetamiseks arendaja töökoormus. Tootmise töökoormus tuleks kasutada Premium mälu.

Lisaks soovitame oma Azure storage konto sama nimega SQL serveri virtuaalmasinates edastamine viivituste vähendamiseks andmekeskuse loomine. Salvestusruumi konto loomisel keelamine geo-dispersioonanalüüs ühtsete kirjutamine tellimus üle mitme ketast pole tagatud. Selle asemel, võtke arvesse, et konfigureerida SQL serveri katastroofi taastamise tehnoloogia kaks Azure andmekeskuste vahel. Lisateabe saamiseks vt [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Ketast juhised

On kolm peamist kettale on Azure VM:

- **OS ketas**: on Azure virtuaalse masina loomisel platvormi manustada vähemalt üks ketas (märgistatud **C** -ketas) oma operatsioonisüsteemi ketta VM. See ketas on VHD, mis on salvestatud lehe bloobimälu laos.
- **Ajutiste ketas**: Azure'i Virtuaalmasinates sisaldavad nimetatakse ajutine ketta teisele kettale (märgistatud **D**: ketas). See on kettal sõlme, mida saate kasutada nullist ruumi.
- **Andmete ketast**: saate ka manustada täiendavate ketast arvuti virtuaalne nimega andmed ketast ja need siis salvestatakse salvestusruumi lehe plekid nimega.

Järgmistes jaotistes kirjeldatakse soovitused erinevat ketast abil.

### <a name="operating-system-disk"></a>Operatsioonisüsteemi ketta

Mõne operatsioonisüsteemi ketas on VHD, saate käivitada ja mount töötava versiooni operatsioonisüsteem on ja sildistatud kui **C** -ketas.

Vaikimisi vahemällu poliitika kettal operatsioonisüsteem on **Lugemis-ja kirjutamisõigusega**. Jõudluse tundliku rakendused, soovitame kasutada andmete ketast asemel operatsioonisüsteemi ketas. Jaotisest andmete kettale allpool.

### <a name="temporary-disk"></a>Ajutist

Ajutiste salvestusruumi draiv, **D**: drive, mitte hoitakse Azure'i bloobimälu. Hoida oma andmebaasi failid või kasutaja tehingulogi failide **D**: ketas.

Sarja-D, Dv2-sari ja G-seeria VMs, on need VMs ajutised seadmesse SSD-põhine. Kui teie töökoormus teeb raske kasutada TempDB (näiteks ajutiste objektide või keerukas ühenduste) talletamise TempDB kettadraivi **D** võib põhjustada suurema TempDB läbilaskevõimega ja alumise TempDB latentsus.

VMs, mis toetavad Premium salvestusruumi (DS-sarja, DSv2-sari ja GS-sarja), soovitame talletada TempDB ja/või puhvri Pool laiendid ketas, mis toetab Premium salvestusruumi loetuks vahemälu lubatud. Erandiks selle soovituse; Kui TempDB kasutuse vaatamiseks on kirjutamine mahukat, võite saavutada suurema jõudluse talletades TempDB **D** kohalikule kettale, mis on ka SSD põhjal need seadme suurused.

### <a name="data-disks"></a>Andmete ketast

- **Kasutage andmete ketast andmed ja logifailide jaoks**: vähemalt, kasutada 2 Premium salvestusruumi [P30 ketast](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) kui üks ketas sisaldab logifailid ja teine sisaldab andmefailide ja TempDB.

- **Ketas Striping**: rohkem läbilaskevõime, saate lisada täiendavad andmed ketast ja kasutada kettapuhastusriista Striping. Andmete ketast arvu määramiseks peate analüüsida IOPS saadaval oma andmete ja logide kontrollimiseks arv. Seda teavet vaadata tabelite IOPS ühe [VM suurus](virtual-machines-windows-sizes.md) ja ketta suurus järgmises artiklis: [Abil Premium salvestusruumi ketast](../storage/storage-premium-storage.md). Kasutage järgmist neid juhiseid:

    - Windows 8/Windows Server 2012 või uuem versioon, kasutage [Salvestusruum](https://technet.microsoft.com/library/hh831739.aspx). Määrata triip suurus 64 KB OLTP töökoormus ja 256 KB andmete tolliladustamise töökoormus, et vältida jõudluse mõju partition vastuolud tõttu. Lisaks määrata veergude arv = füüsiliste arv. Rohkem kui 8 ketast salvestusruumi konfigureerimiseks peab PowerShelli (mitte Server Manager UI) abil konkreetselt määratud arvu ketast veergude arv. [Salvestusruum](https://technet.microsoft.com/library/hh831739.aspx)konfigureerimise kohta leiate lisateavet teemast [Windows PowerShelli salvestusruumi tühikuid cmdlet-käskude](https://technet.microsoft.com/library/jj851254.aspx)

    - Windows 2008 R2 või PowerPointi varasemas versioonis, saate kasutada dünaamilise ketast (OS triibuline mahud) ja triip maht on alati 64 KB. Pange tähele, et see suvand on aegunud seisuga Windows 8/Windows Server 2012. Lisateavet leiate teemast tugi lause veebisaidil [virtuaalse ketta teenus on üleminekul Windows salvestusruumi haldamine API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

    - Kui teie töökoormus pole log intensiivse ja ei ole vaja pühendunud IOPs, saate konfigureerida ainult ühe talletusmahtu. Muul juhul luua kaks salvestusruumi, üks logifailid ja muu talletusmahtu andmefailide ja TempDB. Määratlege ketast iga talletusmahtu vastavalt oma vajadustele laadi seostatud arvu. Pidage meeles, et lubada VM erineva suurusega manustatud andmete ketast erinevat arvu. Lisateavet leiate teemast [Virtuaalmasinates suurused](virtual-machines-windows-sizes.md).

    - Kui te ei kasuta Premium salvestusruumi (arendaja katse stsenaariumid), on soovitus lisada maksimumarv andmete ketast ei toeta teie [VM suurus](virtual-machines-windows-sizes.md) ja kasutada kettapuhastusriista Striping.

- **Vahemällu poliitika**: Premium mälu andmete ketast, lubada andmete kettale, majutada oma andmefailid ja TempDB ainult lugege vahemällu. Kui te ei kasuta Premium salvestusruumi, ei luba kõik andmed kettale vahemällu. Juhised ketta vahemällu, konfigureerimise kohta vt järgmisi teemasid: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) ja [Set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] SQL Serveri teenuse peatamine vahemälu säte Azure VM ketast, mis tahes andmebaas on rikutud vältimiseks muutmisel.

- **NTFS jaotuse ühiku maht**: vormindamisel andmed kettale, on soovitatav kasutada 64 KB eraldatud üksuse suurus andmed ja logifailide kui ka TempDB.

- **Ketas haldamise head tavad**: kui andmed kettale eemaldada või muuta selle vahemälu tüüp, SQL Serveri teenuse peatamine ajal muutmine. Vahemällu talletamise sätetest muutmisel kettal OS Azure'i VM lõpetab, muutub vahemälu tüüp ja taaskäivitamist VM. Vahemälusätted ketta andmete muutmisel VM ei ole peatatud, kuid andmed kettale on eemaldatav kaudu VM ajal Muuda ja seejärel uuesti kasutada.

    >[AZURE.WARNING] SQL Serveri teenuse peatada nende toimingute ajal tõrke võib põhjustada andmebaas on rikutud.

## <a name="io-guidance"></a>I/O juhised

- Parima tulemuse Premium mälu on saavutada, kui te parallelize oma rakenduse ja kutsed. Premium mälu on mõeldud stsenaariumid, kus IO järjekorda sügavus on suurem kui 1, nii, et näete vähe või jõudluse tõstmiseks ühelõimelised järjestikune taotlusi (isegi juhul, kui need on salvestusruumi intensiivse). Näiteks see võivad mõjutada jõudlust tööriistad, nt SQLIO ühelõimelised testi tulemused.

- Kaaluge [andmebaasi lehe tihendamise](https://msdn.microsoft.com/library/cc280449.aspx) see aitab parandada jõudlust I/O intensiivse töökoormus. Siiski võib andmete pakkimine suurendada CPU andmebaasiserveri.

- Kaaluge võimalust lubada kiirotsingu faili lähtestamine vähendada aega, mida on vaja algse faili eraldatud. Kiirsõnumite faili lähtestamine ära, anda SQL serveri (MSSQLSERVER) teenusekonto abil SE_MANAGE_VOLUME_NAME ja lisada **Helitugevuse teha hooldustoiminguid** turbepoliitika. Kui kasutate Azure SQL serveri platvormi pilt, ei lisata vaikimisi teenuse konto (NT Service\MSSQLSERVER) **Helitugevuse teha hooldustoiminguid** turbepoliitika. Teisisõnu, kiirsõnumite faili lähtestamine SQL Server Azure'i platvormi pildil pole lubatud. Pärast SQL Serveri teenuse konto lisamist **Helitugevuse teha hooldustoiminguid** turbepoliitika, taaskäivitage SQL Serveri teenuse. Põhjusi võib olla turvakaalutlused selle funktsiooni kasutamise kohta. Lisateavet leiate teemast [Andmebaasi faili lähtestamine](https://msdn.microsoft.com/library/ms175935.aspx).

- **autogrow** loetakse üksnes juhtumiga ootamatud kasvu jaoks. Hallata oma andmete ja logide kasvu igapäevaselt autogrow. Kui kasutatakse autogrow, kasvata eelnevalt faili, kasutades parameetrit suurus.

- Veenduge, et **autoshrink** on keelatud, et vältida mittevajalike pea kohal, mis võivad negatiivselt mõjutada jõudlust.

- Kõik andmebaasid liikuda andmete ketast, sh süsteemi andmebaasid. Lisateavet leiate teemast [Teisaldamine süsteemi andmebaasid](https://msdn.microsoft.com/library/ms345408.aspx).

- SQL Serveri tõrge Logi- ja faili kataloogide andmete ketast liikumine Seda saab teha rakenduses SQL Server Configuration Manager, paremklõpsates SQL serveri eksemplar ja valige Atribuudid. Tõrge Logi- ja faili sätteid saab muuta **Käivitus** parameetrid. Dump Directory määratud vahekaarti **Täpsemalt** . Järgmine pilt kuvatakse kui tõrge Logi startup parameetrit otsida.

    ![SQL-i ErrorLog kuvatõmmis](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Häälestamise varunduse ja andmebaasi failide asukohad. Kasutage soovitusi selles teemas ja tehke soovitud muudatused aknas serveri atribuudid. Juhised leiate teemast [vaatamiseks või muutmiseks vaikimisi andmed ja logifailide (SQL Server Management Studio) asukohad](https://msdn.microsoft.com/library/dd206993.aspx). Järgmine pilt näitab, kus neid muudatusi teha.

    ![SQL-i andmete Logi-ja varundamine](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Luba lukustatud lehtede IO ja Piipar tegevuse vähendamiseks. Lisateabe saamiseks lugege teemat [Lukusta leheküljed mälu (Windows) valik Luba](https://msdn.microsoft.com/library/ms190730.aspx).

- Kui teil on SQL Server 2012, installige Service Pack 1 koondvärskenduse 10. Selle värskenduse sisaldab fix jõudlus i/o kui täidate valimine ajutise tabeli lause SQL Server 2012. Lisateavet leiate teemast selles [teabebaasi artikkel](http://support.microsoft.com/kb/2958012).

- Kaaluge tihendamise üleandmise ja väljaregistreerimise Azure andmefaile.

## <a name="feature-specific-guidance"></a>Funktsioon juhised

Mõned juurutuste võib saavutada täiendavad jõudluse eeliste konfiguratsiooni tehnika kasutamine. Järgmises loendis olulisemad toimingud SQL serveri funktsioonid, mis aitavad teil parema jõudluse saavutamiseks.

- **Varundus Azure storage**: SQL Azure'i virtuaalmasinates töötava serveri varukoopiate täitmisel saate kasutada [SQL serveri varukoopia URL](https://msdn.microsoft.com/library/dn435916.aspx). See funktsioon on saadaval, alustades SQL Server 2012 SP1 CU2 ja varukoopiat ketast manustatud andmete jaoks soovitatava. Kui te varundamine/taastamine ja neid sealt Azure salvestusruumi, soovituste, pakutakse [SQL serveri varukoopia URL-i põhitõed ja tõrkeotsing ja Azure Storage talletatud varukoopiate põhjal taastamine](https://msdn.microsoft.com/library/jj919149.aspx). Saate neid varukoopiate [Automaatse varundamise SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-classic-sql-automated-backup.md)abil automatiseerida.

    Saate enne SQL Server 2012, [SQL serveri varukoopia Azure'i tööriist](https://www.microsoft.com/download/details.aspx?id=40740). See tööriist aitab varukoopia abil mitme varukoopia triip sihtkohtade läbilaskevõime suurendamiseks.

- **SQL serveri andmefailid Azure**: see uus funktsioon [Azure SQL serveri andmefailid](https://msdn.microsoft.com/library/dn385720.aspx), on saadaval alates SQL Server 2014. Töötab SQL serveri andmefailid Azure näitab võrdlev parameetritele Azure'i andmed ketast, kasutades.

## <a name="next-steps"></a>Järgmised sammud

Kui olete huvitatud põhjalikumalt SQL serveri ja Premium salvestusruumi avastamine, leiate artiklist [Kasutamine Premium salvestusruumi Azure'i Virtuaalmasinates SQL Server](virtual-machines-windows-classic-sql-server-premium-storage.md).

Parimad, vt [SQL Server Azure'i Virtuaalmasinates turvakaalutlused](virtual-machines-windows-sql-security.md).

Vaadake üle SQL serveri virtuaalse masina muud teemad, mis [SQL](virtual-machines-windows-sql-server-iaas-overview.md)server Azure'i Virtuaalmasinates ülevaade.
