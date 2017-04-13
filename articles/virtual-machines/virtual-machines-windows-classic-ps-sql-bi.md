<properties
    pageTitle="SQL serveri ärianalüüs | Microsoft Azure'i"
    description="Selles teemas kasutab ressursse, mis on loodud mudeliga klassikaline juurutamise ja kirjeldatakse Business Intelligence (BI) funktsioone SQL Server töötab klõpsake Azure'i Virtuaalmasinates (VMs)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Azure'i Virtuaalmasinates ärianalüüs

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Microsoft Azure virtuaalse masina Galerii sisaldab pilte, mis sisaldavad SQL serveri installi. SQL serveri väljaanded, mis on toetatud galerii pildid on sama installifailid saate installida kohapealse arvutites ja virtuaalmasinates. Selles teemas on toodud SQL Server Business Intelligence (BI) funktsioonid, mis on installitud pildid ja konfiguratsiooni toimingud pärast virtuaalse masina on ette valmistatud. Selles teemas kirjeldatakse ka ärianalüüsifunktsioone ja heade tavade juurutamise toetatud topoloogiatest.

## <a name="license-considerations"></a>Litsentsi kaalutlused

On kaks võimalust litsentsi SQL Server Microsoft Azure'i Virtuaalmasinates.

1. Litsentsi mobiilsuse eelised, mis on osa teie tarkvaratagatise. Lisateabe saamiseks lugege teemat [Litsentsi mobiilsus Software Assurance Azure kaudu](https://azure.microsoft.com/pricing/license-mobility/).

1. Ühekordne tund määr Azure'i Virtuaalmasinates SQL serveri installitud. Jaotisest "SQL Server" [Virtuaalmasinates hinnad](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Hulgilitsentsimise ja praeguse kohta leiate lisateavet teemast [Virtuaalmasinates litsentsimise KKK](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL serveri pildid on saadaval Azure virtuaalse masina galeriis

Microsoft Azure virtuaalse masina Galerii sisaldab mitu pilti, mis sisaldavad Microsoft SQL Server. Tarkvara installitud virtuaalse masina pildid sõltub opsüsteemi versioon ja SQL serveri versioon. Pildid on saadaval Azure virtuaalse masina Galerii muutuste sageli loendit.

![Pildi SQL Azure'i VM Galerii](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![PowerShelli](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Järgmist PowerShelli skripti tagastab loendi Azure pilte, mis sisaldavad "SQL-Server" on ImageName:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Väljaanded ja SQL Server ei toeta funktsioonide kohta lisateabe saamiseks vaadake järgmist:

- [SQL serveri versioonide](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [SQL Server 2016 väljaannetes toetatud funktsioonid](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>Ärianalüüsifunktsioonid installitud SQL serveri virtuaalse masina galerii pildid

Järgmises tabelis on installitud Microsoft Azure virtuaalse masina levinud galerii pildid SQL serveri Äriteabefunktsioonide"

- SQL Server 2016 RC3

- SQL serveri 2014 SP1 Enterprise

- SQL serveri 2014 SP1 Standard

- SQL Server 2012 SP2 Enterprise

- SQL Server 2012 SP2 Standard

|SQL serveri BI funktsioonide|Installitud on galerii|Märkmete|
|---|---|---|
|**Aruandlusteenuste Omarežiimis teenused**|Jah|Installitud, kuid nõuab konfiguratsiooni, sh aruande halduri URL. Jaotisest [Aruandlusteenused konfigureerimine](#configure-reporting-services).|
|**Aruandluse teenuste SharePointi režiimis**|Ei|Ei sisalda Microsoft Azure virtuaalse masina Galerii SharePointi või SharePoint installifailid. <sup>1</sup>|
|**Analüüsiteenuste mitmedimensiooniliste ja andmete kaevandamine (OLAP-i)**|Jah|Installitud ja konfigureeritud vaikimisi analüüsiteenuste eksemplari|
|**Analüüsiteenuste tabelina**|Ei|Toetatud SQL Server 2012, 2014 ja 2016 pilte, kuid see pole installitud vaikimisi. Installige teise analüüsiteenuste eksemplari. Vaadake teemat installida selles teemas muud teenused SQL Server ja funktsioonid.|
|**Analysis Services Power Pivot for SharePoint**|Ei|Ei sisalda Microsoft Azure virtuaalse masina Galerii SharePointi või SharePoint installifailid. <sup>1</sup>|

<sup>1</sup> SharePointi ja Azure'i virtuaalmasinates kohta lisateabe saamiseks vt [Microsoft Azure'i arhitektuur SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) ja [SharePointi juurutusega Microsoft Azure'i Virtuaalmasinates kohta](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShelli](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Käivitage PowerShelli käsk saada installitud teenused, teenuse nimi "SQL" sisaldavate loendi.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Üldised soovitused ja head tavad

- Soovitatav minimaalne suurus virtuaalse masina on **A3** kasutamisel SQL Server Enterprise Edition. SQL serveri BI analüüsiteenuste ja aruandlusteenuste juurutuste on soovitatav **A4** virtuaalse masina suurus.

    Praeguse VM suuruse kohta leiate teemast [Azure virtuaalse masina suurused](virtual-machines-linux-sizes.md).

- Ketas halduse hea tava on salvestada andmeid, log ja varufailide draividel, välja arvatud **C**: ja **D**:. Näiteks luua andmete ketast **E**: ja **F**:.

    - Ketas, vahemällu poliitikas vaikimisi **C**-ketas: ei ole optimaalne andmetega töötamiseks.

    - **D**: ketas on ajutine ketas, mida kasutatakse peamiselt lehe fail. **D**: ketas mitte hoitakse ja salvestatakse bloobimälu. Haldamise toimingud nagu virtuaalse masina suuruse muutmine lähtestamine **D**: ketas. Soovitatav on **mitte** kasutada **D**: andmebaasi faile, sh tempdb ketas.

    Loomise ja ketast manustamise kohta lisateabe saamiseks vaadake, [Kuidas lisada andmed kettale virtuaalse masina](virtual-machines-windows-classic-attach-disk.md).

- Lõpetamine või desinstallida teenused, mida plaanite kasutada. Näiteks kui virtuaalse masina kasutatakse ainult aruandlusteenuste, peatada või desinstallida Analysis Services ja SQL serveri Integreerimisteenused. Järgmisel pildil on kujutatud teenuseid, mis on käivitatud vaikimisi.

    ![SQL serveri teenused](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] SQL serveri andmebaasi mootor on vajalik toetatud BI stsenaariumites. Ühe serveri VM topoloogia on vajalik andmebaasimootor töötab sama VM.

    Lisateabe saamiseks vaadake järgmist: [Aruandlusteenuste desinstallimine](https://msdn.microsoft.com/library/hh479745.aspx) ja [desinstallimise mõne analüüsiteenuste eksemplari](https://msdn.microsoft.com/library/ms143687.aspx).

- Otsige uue olulised värskendused **Windows Update** . Microsoft Azure virtuaalse masina pildid värskendatakse sageli; kuid olulised värskendused muutuda saadaval **Windows Update'i** pärast viimast värskendamist VM pilt.

## <a name="example-deployment-topologies"></a>Näide juurutamise topoloogiatest

Järgmine on näide juurutuste, mis kasutavad Microsoft Azure'i Virtuaalmasinates. Topoloogiatest joonistel on vaid mõned võimalikud topoloogiatest saate kasutada SQL serveri BI funktsioonide ja Microsoft Azure'i Virtuaalmasinates.

### <a name="single-virtual-machine"></a>Ühe virtuaalse masina

Analysis Services, aruandlusteenused, andmebaasimootor SQL Server, ja andmeallikate virtual ühte arvutisse.

![1 virtuaalse masina ärianalüüsi IAS stsenaarium](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Kahe Virtuaalmasinates

- Analüüsiteenuste, aruandlusteenuste ja SQL serveri andmebaasi mootor virtual ühte arvutisse. See juurutus sisaldab aruande serveri andmebaasidega.

- Teine VM andmeallikad. Teise VM sisaldab SQL serveri andmebaasi mootor andmeallikana.

![ärianalüüsi iaas stsenaarium 2 virtuaalmasinates](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Kombineeritud Azure'i – andmete Azure SQL-andmebaas

- Analüüsiteenuste, aruandlusteenuste ja SQL serveri andmebaasi mootor virtual ühte arvutisse. See juurutus sisaldab aruande serveri andmebaasidega.

- Andmeallikas on Azure SQL-andmebaas.

![ärianalüüsi iaas stsenaariumid vm ja AzureSQL andmeallikana](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hübriid – kohapealse andmed

- Selles näites juurutuse analüüsiteenuste aruandlusteenuste ja SQL serveri andmebaasi mootor käivitada virtual ühte arvutisse. Virtuaalse masina majutab aruande serveri andmebaase. Mõne kohapealse domeeni kaudu Azure virtuaalse Networking või mõne muu VPN tunneling lahendus on ühendatud virtuaalse masina.

- Andmeallikas on kohapealse.

![ärianalüüsi iaas stsenaariumid vm ja eeldusel andmeallikad](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Aruandlusteenuste Omarežiimis konfigureerimine

SQL serveri virtuaalse masina Galerii sisaldab aruandluse teenuste omarežiimis installitud, kuid aruande server pole konfigureeritud. Aruandlusteenuste aruande serveri konfigureerida selles jaotises toodud juhised. Üksikasjalikumat teavet aruandluse teenuste omarežiimis konfigureerimise kohta leiate teemast [Installida aruandlusteenused kohalikke režiimi aruande Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

>[AZURE.NOTE] Sarnase sisuga, mis kasutab Windows PowerShelli skriptide konfigureerida aruande, vt [PowerShelli kasutamine luua Azure'i VM koos vaid kohalikke režiimi aruande Server](virtual-machines-windows-classic-ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Ühenduse loomine virtuaalse masina ja alustada aruannete teenuste Configuration Manager

On kaks levinud töövoogude abil on Azure virtuaalse masina-ühenduse.

- Ühenduse loomiseks, valige virtuaalse masina nimi ja seejärel klõpsake nuppu **Ühenda**. Kaugtöölaua ühendus avatakse ja arvuti nimi täidetakse automaatselt.

    ![azure virtuaalse masina ühendamine](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Ühenduse loomine Windows kaugtöölaua ühenduse virtuaalse masina. Kaugtöölaua kasutajaliideses:

    1. Kui arvuti nimi tippige **pilvepõhise teenuse nimi** .

    1. Tippige koolon (:) ning avalike pordi number, mis on konfigureeritud lubama TCP Remote'i töölaua lõpp.

        MyService.cloudapp.net:63133

        Lisateavet leiate teemast [mis on pilv?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Käivitage aruandlusteenuste.**

1. **Windows Server 2012**:

1. Tippige kuval **käivitamine** **Aruandlusteenuste** rakenduste loendi kuvamiseks.

1. Paremklõpsake **Aruandluse teenuste Configuration Manager** ja klõpsake käsku **Käivita administraatorina**.

1. **Windows Server 2008 R2**:

1. Klõpsake nuppu **Start**ja seejärel käsku **Kõik programmid**.

1. Klõpsake käsku **Microsoft SQL Server 2016**.

1. Klõpsake käsku **Konfigureerimisriistad**.

1. Paremklõpsake **Aruandluse teenuste Configuration Manager** ja klõpsake käsku **Käivita administraatorina**.

Või

1. Klõpsake käsku **Käivita**.

1. Tippige dialoogiboksis **programmide ja failide otsing** **aruandlusteenused**. Kui VM töötab Windows Server 2012, tippige kuval käivitamine Windows Server 2012 **aruandlusteenused** .

1. Paremklõpsake **Aruandluse teenuste Configuration Manager** ja klõpsake käsku **Käivita administraatorina**.

    ![Otsige ssrs konfigureerimishaldur](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Aruandlusteenuste konfigureerimine

**Teenusekonto ja veebiteenuse URL.**

1. Veenduge, et **Serveri nimi** on kohaliku serveri nimi ja klõpsake nuppu **Ühenda**.

1. Märkus tühja **Aruande serveri andmebaasi nimi**. Andmebaas on loodud, kui konfiguratsioon on lõpule jõudnud.

1. Veenduge, et **Oleku aruande Server** on **käivitatud**. Kui soovite kontrollida teenuse Windows Server Manager, teenus on **SQL serveri aruandlusteenuste** Windows teenus.

1. Klõpsake nuppu **Konto** ja konto muutmiseks vastavalt vajadusele. Kui virtuaalse masina kasutatakse-domeeni ühendatud keskkonnas, sisseehitatud **aruandeserveri** on piisavalt. Teenuse konto kohta leiate lisateavet teemast [Teenuse konto](https://msdn.microsoft.com/library/ms189964.aspx).

1. Klõpsake vasakpoolsel paanil nuppu **Veebiteenuse URL** .

1. Klõpsake nuppu **Rakenda** vaikeväärtused konfigureerimiseks.

1. Pange tähele **aruande Server Web teenuse URL-id**. Pange tähele, et vaikeport TCP 80 ja on osa URL-i. Toimub hiljem, saate luua Microsoft Azure virtuaalse masina Endpoint pordi.

1. Kontrollige **tulemuste** paanil toimingud lõpule viidud.

**Andmebaas.**

1. Klõpsake vasakpoolsel paanil **andmebaasi** .

1. Klõpsake nuppu **Muuda andmebaasi**.

1. **Loo uus aruanne serveri andmebaas** on valitud ja seejärel klõpsake nuppu edasi.

1. Kontrollige **Serveri nimi** ja klõpsake nuppu **Testi ühendust**.

1. Kui tulem on **testi ühendust õnnestus**, klõpsake nuppu **OK** ja seejärel klõpsake nuppu **edasi**.

1. Pange tähele, et andmebaasi nimi on **aruandeserveri** ja **aruandeserveris mode** on **levinud** ja seejärel klõpsake nuppu **edasi**.

1. Klõpsake **järgmisel** lehel **identimisteave** .

1. Klõpsake nuppu **Järgmine** lehe **Kokkuvõte** .

1. Lehel **edenemise ja valmis** , klõpsake nuppu **edasi** .

**Web portaali URL-i või aruandehalduri URL-i 2012 – 2014.**

1. 2014 ja 2012 vasakpoolsel paanil nuppu **Web portaali URL-i**või **Aruandehalduri URL-i** .

1. Klõpsake nuppu **Rakenda**.

1. Kontrollige **tulemuste** paanil toimingud lõpule viidud.

1. Klõpsake käsku **välju**.

Aruande serveri õiguste kohta leiate teavet teemast [Kohalikke režiimi aruandeserveri õiguste andmine](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Sirvige kohalik aruandehalduri

Kontrollige konfiguratsiooni, sirvige report Manageri VM.

1. VM, käivitage Internet Explorer administraatori õigustega.

1. Liikuge sirvides http://localhost/reports VM.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Ühenduse loomine Remote veebiportaali või 2014 ja 2012 Report Manageri abil

Kui soovite luua ühenduse veebiportaali või aruandehalduri 2014 ja 2012 virtual arvutisse kaugarvutist, luua uue virtuaalse masina TCP lõpp-punkti. Vaikimisi kuulab serveri **pordi 80**HTTP-päringud. Kui konfigureerite aruande serveri URL-ide teise porti kasutama, peate määrama selle pordi number järgmised juhised.

1. Luua virtuaalse masina TCP pordi 80 lõpp. Lisateavet leiate teemast virtuaalse masina lõpp-punktid tulemüüri portide osa [ja](#virtual-machine-endpoints-and-firewall-ports) selles dokumendis.

1. Avage virtuaalse masina tulemüüri portide 80.

1. Liikuge sirvides veebiportaali või aruandehalduri, kasutades Azure virtuaalse masina **Nimi DNS-i** serveri nimeks URL-i. Näiteks:

    **Aruande server**: http://uebi.cloudapp.net/reportserver  **veebiportaali**: http://uebi.cloudapp.net/reports

    [Aruande serveri juurdepääsu tulemüüri konfigureerimine](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Luua ja avaldada Azure virtuaalse masina aruanded

Järgmises tabelis on toodud mõned kohapealse arvutist olemasolevaid aruandeid aruandeserveris majutatud sisse Microsoft Azure virtuaalse masina avaldada valikuid:

- **Report Builder**: virtuaalse masina sisaldab klõpsake – üks kord versiooni Microsoft SQL Server Report Builder SQL 2014 ja 2012. SQL-i 2016 arvutis virtual aruande builder esimest korda alustamiseks tehke järgmist.

    1. Alustage brauseri administraatoriõigused.

    1. Liikuge sirvides veebiportaali virtual arvutisse, ja valige paremas ülanurgas ikooni **alla laadida** .
    
    1. Valige **Report Builder**.

    Lisateavet leiate teemast [Aruandekoosturi käivitamine](https://msdn.microsoft.com/library/ms159221.aspx).

- **SQL Server Data Tools**: VM: SQL Server Data Tools virtual arvutisse on installitud ja seda saab kasutada arvutis virtual **Aruande serveri projektide** ja aruannete loomiseks. SQL Server Data Tools saate avaldada aruannete virtual arvutisse aruandeserveris.

- **SQL Server Data Tools: Remote**: kohalikus arvutis aruandlusteenuste projekti loomine SQL Server Data Tools, mis sisaldab aruandlusteenuste aruandeid. Project web teenuse URL-i ühenduse konfigureerimine.

    ![SSDT projekti atribuutide SSRS projekti](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Looge lisamine. VHD kõvaketta sisu, mis sisaldab aruandeid ja seejärel üles laadida ja lisada ketas.

    1. Looge lisamine. VHD kõvaketas kohalikus arvutis, mis sisaldab teie aruandeid.

    1. Loomine ja haldamine serdi installimine.

    1. Lisa-AzureVHD cmdlet-käsu abil [loomine ja Laadi Windows Server VHD Azure'i](virtual-machines-windows-classic-createupload-vhd.md)Azure VHD faili üleslaadimiseks.

    1. Kinnitage virtuaalse masina ketas.

## <a name="install-other-sql-server-services-and-features"></a>Muude SQL serveri teenuste ja funktsioonide installimine

Analüüsiteenuste tabelina režiimis, nt SQL serveri lisateenuse installimiseks käivitage SQL serveri häälestamise viisard. Setup failid on virtuaalne arvuti kohalikule kettale.

1. Klõpsake nuppu **Start** ja seejärel käsku **Kõik programmid**.

1. Klõpsake käsku **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** või **Microsoft SQL Server 2012** ja seejärel klõpsake käsku **Konfigureerimisriistad**.

1. Klõpsake nuppu **SQL serveri installi Center**.

Või C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe või C:\SQLServer_11.0_full\setup.exe käivitamine

>[AZURE.NOTE] Esimest korda, kui käivitate SQL Serveri install, rohkem häälestamise faile saab alla laadida ja nõuavad virtuaalse masina taaskäivitage ja SQL serveri installiprogrammi taaskäivitamist.
>
>Kui teil on vaja korduvalt kohandamine: Microsoft Azure virtuaalse masina valitud pilti, kaaluge SQL serveri pilt. SQL Server 2012 SP1 CU2 Analysis Services SysPrep funktsiooni on lubatud. Lisateabe saamiseks vt [kaalutlused installimist SQL serveri abil Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) ja [Sysprep serverirollide tugi](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="to-install-analysis-services-tabular-mode"></a>Installimiseks Analysis Services tabelina režiimis

Juhised selle jaotise **summarize** installimise analüüsiteenuste tabelina režiimis. Lisateabe saamiseks vaadake järgmist:

- [Installige analüüsiteenuste tabelina režiimis](https://msdn.microsoft.com/library/hh231722.aspx)

- [Tabeli modelleerimine (Adventure Works õpetuse)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Analysis Services tabelina režiimis installimiseks tehke järgmist.**

1. SQL serveri installiviisardis, klõpsake vasakul paanil **Installi** ja seejärel nuppu **uue SQL serveri eraldiseisev install või funktsioonide lisamine olemasoleva installi**.

    - Kui te ei näe **Kausta sirvimine**, liikuge sirvides c:\SQLServer_13.0_full, c:\SQLServer_12.0_full või c:\SQLServer_11.0_full ja seejärel klõpsake nuppu **Ok**.

1. Klõpsake **järgmisel** lehel toote värskendused.

1. Klõpsake lehel **Installi tüübi** , valige **Käivita SQL serveri uue installi** ja klõpsake nuppu **edasi**.

1. Klõpsake lehel **Häälestus rolli** **SQL serveri funktsioonid install**.

1. Klõpsake lehel **Funktsioonide valik** **Analysis Services**.

1. Tippige lehel **Eksemplari konfiguratsiooni** kirjeldav nimi, nt **tabelina** tekstiväljadele **Eksemplari nimi** ja **Eksemplari ID-d** .

1. Valige lehel **Analysis Services konfiguratsiooni** **Tabel režiimis**. Praeguse kasutaja lisamiseks loendisse administraatoriõigusi.

1. Lõpetamine ja SQL Serveri installimise viisardi sulgemiseks.

## <a name="analysis-services-configuration"></a>Analüüsiteenuste konfigureerimine

### <a name="remote-access-to-analysis-services-server"></a>Kaugpöördusteenuse Analysis Services serveris

Analüüsiteenuste serverisse toetab ainult Windowsi autentimist. Kaugjuurdepääs Analysis Services kliendi rakenduste nagu SQL Server Management Studio või SQL Server Data Tools, virtuaalse masina peab liidetavate kohaliku domeeni, kasutades Azure virtuaalse Networking. Lisateavet leiate teemast [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md).

**Vaikimisi eksemplari** analüüsiteenuste kuulab TCP pordi **2383**. Avage pordi virtuaalmasinates tulemüüri. Rühmitatud nimega analüüsiteenuste eksemplari kuulab ka pordi **2383**.

Soovitud **nime** analüüsiteenuste, peab SQL Serveri brauser teenuse pordi juurdepääsu haldamiseks. SQL Serveri brauser vaikimisi konfiguratsioon on pordi **2382**.

Virtuaalmasinates tulemüüri, avage port **2382** ja staatilise Analysis Services nimega eksemplari pordi loomine.

1. Veendumaks, et pordid, mis on juba kasutusel VM ja millised protsess kasutab pordid, käivitage järgmine käsk administraatoriõigustega:

        netstat /ao

1. Kasutage SQL Server Management Studio staatilise analüüsiteenuste loomiseks nimega eksemplari portide, värskendades 'Port' väärtus tabeli AS näiteks üldised atribuudid. Lisateabe saamiseks vt "fikseeritud port kasutamiseks vaikimisi või nime" [Windowsi tulemüür Luba juurdepääs Analysis Services konfigureerimine](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).

1. Taaskäivitage teenuse analüüsiteenuste tabelina esitatud eksemplari.

Lisateavet leiate teemast virtuaalse masina lõpp-punktid tulemüüri portide osa **ja** selles dokumendis.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Virtuaalse masina lõpp-punktid ja tulemüüri portide

Selles jaotises antakse ülevaade sellest, Microsoft Azure'i virtuaalse masina lõpp-punktid luua ja avada virtuaalse masina tulemüürid pordid. Järgmises tabelis on kokkuvõte **TCP** -pordid lõpp-punktide jaoks luua ja avada virtuaalmasinates tulemüüri portide.

- Kui kasutate ühe VM ja kaks järgmist on tõesed, pole vaja luua VM lõpp-punktid ja Avage pordid VM tulemüüri pole vaja.

    - Teil pole kaugühenduse teel ühenduse SQL serveri funktsioonid VM. Kaugtöölaua VM loomise ja juurdepääs SQL serveri funktsioonid kohapeal VM käsitletakse kaugühenduse SQL serveri funktsioone.

    - Saate ära Liitu VM kohapealse domeeni Azure virtuaalse Networking või mõne muu VPN tunnelite lahenduse kaudu.

- Kui virtual arvuti pole ühendatud domeeni, kuid soovite kaugühenduse teel ühenduse loomine SQL serveri funktsioonid VM:

    - Avage pordid tulemüüri VM.

    - Saate luua virtuaalse masina lõpp-punktid märkida portide (*).

- Kui virtuaalse masina on ühendatud domeeniga VPN tunneliga, nt Azure virtuaalse Networking abil, siis lõpp-punktid ei ole vaja. Avage pordid siiski tulemüüri VM.

  	|Port|Tüüp|Kirjeldus|
|---|---|---|
|**80**|TCP|Aruande serveri Kaugpöördusteenuse (*).|
|**1433**|TCP|SQL Server Management Studio (*).|
|**1434**|UDP|SQL Serveri brauser. See on vajalik, kui VM ühendatud domeeniga.|
|**2382**|TCP|SQL Serveri brauser.|
|**2383**|TCP|SQL Serveri analüüsiteenuste vaikimisi eksemplari ja rühmitatud nimega eksemplarid.|
|**Kasutaja määratletud**|TCP|Looge staatilise Analysis Services nimega eksemplari pordi jaoks pordi number, valige ja seejärel pordinumber tulemüüri blokeeringu.|

Lõpp-punktid loomise kohta lisateabe saamiseks vaadake järgmist:

- Loo lõpp-punktid:[Kuidas luua virtuaalse masina lõpp-punktid](virtual-machines-windows-classic-setup-endpoints.md).

- SQL Server: Jaotisest "Täieliku konfiguratsiooni juhiseid ühenduse, virtuaalse masina kasutamine SQL Server Management Studio" [ettevalmistamise Azure SQL serveri virtuaalse masina](virtual-machines-windows-portal-sql-server-provision.md).

Järgmine diagramm näitab pordid avada VM tulemüüri lubama Kaugpöördusteenuse funktsioonidele ja komponentide VM.

![pordid Azure VMs ärianalüüsi rakenduste avamine](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Ressursid

- Lugege läbi Microsofti tarkvara kasutada Azure virtuaalse masina keskkonnas toe poliitika. Teema Kokkuvõte nt BitLockeri, tõrkesiirdeklastrite ja võrgu Koormusetasakaalustuseks funktsioonide tugi. [Tugiteenuste serveritarkvara Microsoft Azure'i Virtuaalmasinates](http://support.microsoft.com/kb/2721672).

- [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md)

- [Virtuaalmasinates](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [Azure SQL serveri virtuaalse masina ettevalmistamine](virtual-machines-windows-portal-sql-server-provision.md)

- [Kuidas lisada andmed kettale virtuaalse masina](virtual-machines-windows-classic-attach-disk.md)

- [SQL Server Azure'i VM andmebaasi migreerimine](virtual-machines-windows-migrate-sql.md)

- [Selles režiimis Analysis Services eksemplari määratlemine](https://msdn.microsoft.com/library/gg471594.aspx)

- [Mitmedimensiooniliste modelleerimine (Adventure Works õpetuse)](https://technet.microsoft.com/library/ms170208.aspx)

- [Azure'i dokumentatsiooni Center](https://azure.microsoft.com/documentation/)

- [Power BI kasutamine hübriidkeskkonnas](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Tagasiside ja kontaktide teabe esitamiseks Microsoft SQL Serveri ühenduse kaudu](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Kogukonna sisu

- [SQL Azure'i andmebaasi haldamine PowerShelli abil](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)
