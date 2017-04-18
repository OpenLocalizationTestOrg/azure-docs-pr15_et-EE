<properties
    pageTitle="Konfigureerimine alati kättesaadavus rühma Azure VM – klassikaline"
    description="Azure'i Virtuaalmasinates alati kättesaadavus rühma loomine. Selles õpetuses kasutatakse peamiselt kasutajaliidese ja tööriistad asemel skriptimise."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm---classic"></a>Konfigureerimine alati kättesaadavus rühma Azure VM – klassikaline

> [AZURE.SELECTOR]
- [Ressursihaldur: Mall](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Ressursihaldur: käsitsi](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassikaline: kasutajaliides](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassikaline: PowerShelli](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Õppeteema – lõpuni näidatakse, kuidas rakendada kasutamine SQL serveri alati töötavate Azure'i virtuaalmasinates kättesaadavus rühmad.

Õpetuse lõpus oma SQL serveri alati klõpsake lahenduse Azure koosneb järgmisi elemente:

- Mitu alamvõrku, sh on ees- ja tagaandmebaas alamvõrgu sisaldavad virtuaalse võrgus

- Domeenikontrolleri Active Directory (AD) domeeniga

- Kahe SQL serveri VMs tagaandmebaas alamvõrgu juurutatud ja ühendatud AD domeeni

- 3-sõlm WSFC kobar mudeliga sõlm enamik kvoorumi

- Kättesaadavus rühma kaks sünkroonse kinnitamine koopiad andmebaasi kättesaadavus

Alloleval joonisel on graafiline kujutis lahendus.

![AG Azure test Lab arhitektuur](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Pange tähele, et see on üks võimalik konfiguratsioon. Näiteks saate minimeerida arvu kahe-koopia kättesaadavus rühma VMs salvestama Arvuta tundi Azure domeenikontrolleri kvoorumi failide ühiskasutuse haldur 2 sõlme WSFC kobar abil. See meetod vähendab ühte ülaltoodud konfiguratsiooni loendus VM.

Selle õpetuse eeldab järgmist:

- Teil on juba Azure'i konto.

- Teate juba ettevalmistamise klassikaline SQL serveri VM abil GUI virtuaalse masina galeriist.

- Teil on juba ühtlase mõistmine alati klõpsake kättesaadavus rühmad. Lisateavet leiate teemast [Alati klõpsake kättesaadavus rühmade (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Kui olete huvitatud alati klõpsake kättesaadavus rühmade kasutamine koos SharePointi, lugege teemat [Konfigureerimine SQL Server 2012 alati sees kättesaadavus rühmad SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Virtuaalse võrgu ja domeenikontrolleri serverisse loomine

Alustamist Azure uue prooviversiooni kontoga. Kui teie konto häälestamine on lõpule jõudnud, peaksite avakuval Azure klassikaline portaali.

1. Klõpsake lehe vasakus ülanurgas nupp **Uus** , nagu allpool näidatud.

    ![Klõpsake nuppu Uus portaalis](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)

1. Klõpsake valikut **Teenused**, ning **Virtuaalse võrgu,** klõpsake nuppu ja seejärel nuppu **Kohandatud loomine**, nagu allpool näidatud.

    ![Virtuaalse võrgu loomine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)

1. Dialoogiboksis **Loomine VIRTUAALSE võrgu** luua uue virtuaalse võrgu etapid lehtede allpool sätetega. 

  	|Lehe|Sätted|
|---|---|
|Virtuaalse võrgu üksikasjad|**NIMI = ContosoNET**<br/>**PIIRKONNA = Lääne USA.**|
|DNS-serverid ja VPN Ühenduvus|Ükski|
|Virtuaalse võrgu aadress tühikud|Sätted on pildil näidatud: ![Virtuaalse võrgu loomine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png)|

1. Järgmiseks loote VM, kas kasutate selle domeenikontrolleri (näiteks Põhiliselt). Klõpsake nuppu **Uus** uuesti, seejärel **arvutada**, siis **virtuaalse masina**ja **Galeriist**, nagu allpool näidatud.

    ![Luua VM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)

1. Dialoogiboksis **loomine VIRTUAALSE masina** konfigureerida uue VM etapid lehtede allpool sätetega. 

  	|Lehe|Sätted|
|---|---|
|Valige virtuaalse masina operatsioonisüsteem|Windows Server 2012 R2 andmekeskusega|
|Virtuaalse masina konfigureerimine|**Versiooni VÄLJAANDMISAEG** = (uusim)<br/>**VIRTUAALSE masina nimi** = ContosoDC<br/>**Esimese taseme** = STANDARD<br/>**Suurus** = A2 (2 südamikud)<br/>**Uus kasutaja nimi** = AzureAdmin<br/>**Uus parool** = Contoso! 000<br/>**Kinnitage** = Contoso! 000|
|Virtuaalse masina konfigureerimine|**PILVETEENUSES** = Loo uus pilveteenuses<br/>**PILVETEENUSE teenuse DNS-i nimi** = pilve kordumatu teenuse nimi<br/>**DNS-i nimi** = kordumatu nimi (ex: ContosoDC123)<br/>**Piirkonna/osaleja rühma/VIRTUAALSE võrgu** = ContosoNET<br/>**Virtuaalne ALAMVÕRKU** = Back(10.10.2.0/24)<br/>**Salvestusruumi konto** = konto automaatselt loodud mäluruumi kasutamine<br/>**Kättesaadavus** = (pole)|
|Virtuaalse masina suvandid|Kasutage vaikesätteid.|

Kui olete lõpetanud, konfigureerida uue VM, oodake olema provsioned VM. Selleks võib kuluda aega ja Azure klassikaline portaalis menüü **virtuaalse masina** klõpsamisel näete ContosoDC Rattasõit olekus **alustades (Provisioning)** **peatamiseni**, **alustades**, **töötab (Provisioning)**ja lõpuks **töötab**.

Näiteks Põhiliselt server on nüüd ette valmistatud. Järgmiseks tuleb konfigureerida Active Directory domeeni, näiteks Põhiliselt selles serveris.

## <a name="configure-the-domain-controller"></a>Selle domeenikontrolleri konfigureerimine

Järgmistes juhistes, saate seadistada ContosoDC masina domeenikontrolleri Corp.contoso.com.

1. Valige portaalis **ContosoDC** kohapeal. Klõpsake vahekaardil **armatuurlaua** **ühendamine** kaugtöölaua juurdepääs RDP-faili avamiseks.

    ![Ühenduse loomine Vritual arvutisse](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)

1. Logige sisse oma konfigureeritud administraatori konto (**\AzureAdmin**) ja parool (**Contoso! 000**).

1. Vaikimisi **Serveri halduri** armatuurlaual peaks olema kuvatud.

1. Klõpsake armatuurlaual linki **Lisa rollid ja funktsioonid** .

    ![Server Explorer lisada rollid](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)

1. Valige **järgmise** , kuni kuvatakse jaotises **Serveri rollid** .

1. Valige **Active Directory domeeniteenused** ja **DNS-i serveri** rollid. Kui kuvatakse vastav viip, lisage lisafunktsioone, mis on nõutud järgmised rollid.

    >[AZURE.NOTE] Saate valideerimise hoiatus, et on staatiline IP-aadress. Kui teil on katsetamine konfiguratsiooni, klõpsake nuppu Jätka. Jaoks stsenaariumid [PowerShelliga domeeni domeenikontrolleri arvuti staatiline IP-aadressi määramiseks](./virtual-network/virtual-networks-reserved-private-ip.md).

    ![Rollide dialoog lisa](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)

1. Klõpsake nuppu **edasi** , kuni jõuate **kinnitamise** jaotis. Märkige ruut **uuesti sihtkoha server automaatselt, kui nõutav** .

1. Klõpsake nuppu **Installi**.

1. Pärast funktsioonide installimine lõpule jõuaks, naaske armatuurlaua **Server Manager** .

1. Klõpsake vasakpoolsel paanil uue **AD DS** suvandi valimine

1. Klõpsake linki **veel** kollasel ribal hoiatus.

    ![AD DS dialoogiboksi DNS-i serveri VM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)

1. Dialoogiboksi **Kõik serveri tööülesande üksikasjad** veerus **toiming** nuppu **Liigenda vasakule domeenikontrolleri selles serveris**.

1. **Active Directory domeeni teenuste konfiguratsiooniviisardi**kasutada järgmised väärtused:

  	|Lehe|Säte|
|---|---|
|Juurutamise konfigureerimine|**Lisa uus mets** = valitud<br/>**Root domeeninime** = corp.contoso.com|
|Domeeni domeenikontrolleri suvandid|**Parooli** = Contoso! 000<br/>**Kinnitage parool** = Contoso! 000|

1. Klõpsake nuppu **edasi** läbi viisardi teised lehed. Lehel **Eeltingimuste kontrollimine** veenduge, et näete järgmist teadet: **kõik eelnevalt nõutud kontrollid edukalt läbitud**. Pange tähele, et vaatate mis tahes rakendatav hoiatusteated, kuid see on võimalik jätkata installimist.

1. Klõpsake nuppu **Installi**. **ContosoDC** virtuaalse masina kuvatakse automaatselt arvuti taaskäivitada.

## <a name="configure-domain-accounts"></a>Domeeni kontode konfigureerimine

Järgmiste juhiste juurde konfigureerida Active Directory (AD) kontode hilisemaks kasutamiseks.

1. Logige uuesti **ContosoDC** kohale.

1. **Server** Manager valige **Tööriistad** ja klõpsake nuppu **Active Directory haldus keskele**.

    ![Active Directory halduskeskus](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)

1. Klõpsake soovitud **Active Directory haldus Center** valige **corp (kohalik)** vasakpoolsel paanil.

1. Klõpsake parempoolsel paanil **ülesanded** , valige **Uus** ja seejärel käsku **kasutaja**. Saate kasutada järgmisi sätteid:

  	|Säte|Väärtus|
|---|---|
|**Eesnimi**|Installimine|
|**Kasutaja SamAccountName**|Installimine|
|**Parooli**|Contoso! 000|
|**Kinnitage parool**|Contoso! 000|
|**Muud suvandid**|Valitud|
|**Parool ei aegu**|Märgitud|

1. Klõpsake nuppu **OK** , et luua kasutaja **installida** . Selle konto kasutatakse tõrkesiirdeklastri ja kättesaadavus rühma konfigureerida.

1. Luua kaks täiendavate kasutajate samu juhiseid: **CORP\SQLSvc1** ja **CORP\SQLSvc2**. Nende kontodega kasutatakse SQL Serveri eksemplari. Järgmiseks peate **CORP\Install** konfigureerida Windowsi teenuse Tõrkesiirde rühmitamise (WSFC) vajalikke õigusi anda.

1. **Active Directory haldus Center**valige vasakul paanil **corp (kohalik)** . Klõpsake parempoolsel paanil **toimingud** nuppu **Atribuudid**.

    ![CORP kasutaja atribuudid](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)

1. Valige **laiendid**ja seejärel klõpsake vahekaarti **Turvalisus** ja siis nuppu **Täpsemalt** .

1. Dialoogiboksis **Täpsemad turvasätted corp** . Klõpsake nuppu **Lisa**.

1. Klõpsake käsku **Valige subjekt**. Otsige üles **CORP\Install**. Klõpsake nuppu **OK**.

1. Valige **Kõik atribuudid lugeda** ja **luua arvuti objektide** õigused.

    ![Corp kasutajaõigused](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)

1. Klõpsake nuppu **OK**ja seejärel klõpsake uuesti nuppu **OK** . Sulgege aken corp atribuudid.

Nüüd, kui olete lõpetanud Active Directory ja kasutajale objektid, saate luua kolm SQL serveri VMs ja selle domeeni liituma.

## <a name="create-the-sql-server-vms"></a>SQL serveri VMs loomine

Järgmisena Looge kolm VMs, sh kobar WSFC sõlm ja kahe SQL serveri VMs. Luua iga VMs, minge tagasi Azure klassikaline portaali, klõpsake nuppu **Uus**, **arvutada**, **virtuaalse masina**ja seejärel **Galeriist**. Seejärel abil malle järgmise tabeli abil saate luua VMs.

|Lehe|VM1|VM2|VM3|
|---|---|---|---|
|Valige virtuaalse masina operatsioonisüsteem|**Windows Server 2012 R2 andmekeskusega**|**SQL serveri 2014 RTM Enterprise**|**SQL serveri 2014 RTM Enterprise**|
|Virtuaalse masina konfigureerimine|**Versiooni VÄLJAANDMISAEG** = (uusim)<br/>**VIRTUAALSE masina nimi** = ContosoWSFCNode<br/>**Esimese taseme** = STANDARD<br/>**Suurus** = A2 (2 südamikud)<br/>**Uus kasutaja nimi** = AzureAdmin<br/>**Uus parool** = Contoso! 000<br/>**Kinnitage** = Contoso! 000|**Versiooni VÄLJAANDMISAEG** = (uusim)<br/>**VIRTUAALSE masina nimi** = ContosoSQL1<br/>**Esimese taseme** = STANDARD<br/>**Suurus** = A3 (4 tuuma)<br/>**Uus kasutaja nimi** = AzureAdmin<br/>**Uus parool** = Contoso! 000<br/>**Kinnitage** = Contoso! 000|**Versiooni VÄLJAANDMISAEG** = (uusim)<br/>**VIRTUAALSE masina nimi** = ContosoSQL2<br/>**Esimese taseme** = STANDARD<br/>**Suurus** = A3 (4 tuuma)<br/>**Uus kasutaja nimi** = AzureAdmin<br/>**Uus parool** = Contoso! 000<br/>**Kinnitage** = Contoso! 000|
|Virtuaalse masina konfigureerimine|**PILVETEENUSES** = varem loodud kordumatu pilvepõhise teenuse DNS-i nimi (ex: ContosoDC123)<br/>**Piirkonna/osaleja rühma/VIRTUAALSE võrgu** = ContosoNET<br/>**Virtuaalne ALAMVÕRKU** = Back(10.10.2.0/24)<br/>**Salvestusruumi konto** = konto automaatselt loodud mäluruumi kasutamine<br/>**Kättesaadavus** = Loo-saadavus seadmine<br/>**Kättesaadavus nime SEADMISEKS** = SQLHADR|**PILVETEENUSES** = varem loodud kordumatu pilvepõhise teenuse DNS-i nimi (ex: ContosoDC123)<br/>**Piirkonna/osaleja rühma/VIRTUAALSE võrgu** = ContosoNET<br/>**Virtuaalne ALAMVÕRKU** = Back(10.10.2.0/24)<br/>**Salvestusruumi konto** = konto automaatselt loodud mäluruumi kasutamine<br/>**Kättesaadavus** = SQLHADR (samuti saate konfigureerida: määrake pärast seadme on loodud kättesaadavus. Kõik kolm masinad määratakse SQLHADR-saadavus häälestada.)|**PILVETEENUSES** = varem loodud kordumatu pilvepõhise teenuse DNS-i nimi (ex: ContosoDC123)<br/>**Piirkonna/osaleja rühma/VIRTUAALSE võrgu** = ContosoNET<br/>**Virtuaalne ALAMVÕRKU** = Back(10.10.2.0/24)<br/>**Salvestusruumi konto** = konto automaatselt loodud mäluruumi kasutamine<br/>**Kättesaadavus** = SQLHADR (samuti saate konfigureerida: määrake pärast seadme on loodud kättesaadavus. Kõik kolm masinad määratakse SQLHADR-saadavus häälestada.)|
|Virtuaalse masina suvandid|Kasutage vaikesätteid.|Kasutage vaikesätteid.|Kasutage vaikesätteid.|

<br/>

>[AZURE.NOTE] Eelmise konfiguratsiooni soovitab STANDARD taseme virtuaalmasinates, tavaline taseme masinad ei toeta koormusetasakaalustusega lõpp-punktid hiljem luua mõne rühma kättesaadavus kuulajatele nõutav. Ka arvuti suurusi soovitatud siin on mõeldud testimise kättesaadavus rühmad Azure VMs. Klõpsake tootmise töökoormus parima jõudluse tagamiseks teemast soovitused SQL serveri arvuti suurused ja [jõudluse head tavad SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-performance.md)konfigureerimine.

Kui kolm VMs on täielikult ette valmistatud, peate liituma **corp.contoso.com** domeeni ja CORP\Install masinad administraatoriõigusi anda. Selleks, kasutage järgmist iga kolme VMs.

1. Esmalt eelistatud DNS-i serveri aadressi muutmiseks. Alustuseks, valides VM loendis ja klõpsake nuppu **Loo ühendus** kohaliku kataloogi iga VM Remote'i töölaua (RDP) faili allalaadimiseks. VM valimiseks klõpsake esimest lahtrit Real, kuid nagu allpool näidatud.

    ![RDP faili alla laadida](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)

1. Käivitage RDP faili alla laadida ja sisse logida kasutades oma konfigureeritud VM administraatorikonto (**BUILTIN\AzureAdmin**) ja parool (**Contoso! 000**).

1. Kui olete sisse logitud, peaksite nägema armatuurlaua **Server Manager** . Klõpsake vasakpoolsel paanil **Kohalik Server** .

1. Valige link **IPv4 aadress on määratud DHCP, IPv6 lubatud** .

1. Valige aknas **Võrguühenduste** võrgu ikooni.

    ![Muuta VM eelistatud DNS-i Server](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)

1. Käsuriba, klõpsake **selle ühenduse sätteid muuta** (sõltuvalt teie akna suurust, peate kahekordsete paremnoolt selle käsu kuvamiseks klõpsake).

1. Valige **Internet Protocol Version 4 (TCP/IPv4)** ja klõpsake käsku Atribuudid.

1. Valige suvand Kasuta järgmisi DNS-serveri aadresse ja määrake **10.10.2.4** **Eelistatud DNS-**serveris.

1. Aadress **10.10.2.4** on määratud VM 10.10.2.0/24 alamvõrku Azure virtuaalse võrgu aadress ja selle VM on **ContosoDC**. Veendumaks, et **ContosoDC**IP-aadress, kasutage **nslookup contosodc** Käsuviip, nagu allpool näidatud.

    ![IP-aadressi otsimine näiteks Põhiliselt NSLOOKUPI abil](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)

1. Klõpsake O**K** ja seejärel **Sule** muudatuste kinnitamiseks. Teil on nüüd võimalik liituda VM **corp.contoso.com**.

1. Tagasi **Kohaliku serveri** aknas, klõpsake linki **töörühm** .

1. Jaotises **Arvuti nimi** , klõpsake nuppu **Muuda**.

1. **Domeeni** märkige ruut ja tippige tekstiväljale **corp.contoso.com** . Klõpsake nuppu **OK**.

1. Määrake dialoogiboksis **Windowsi Turve** popup mandaat vaikimisi domeeni administraatori konto (**CORP\AzureAdmin**) ja parool (**Contoso! 000**).

1. Kui kuvatakse teade "Tere tulemast corp.contoso.com domeeni", klõpsake nuppu **OK**.

1. Klõpsake nuppu **Sule**ja seejärel klõpsake **Taaskäivita kohe** popup dialoogiboksi.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-vm"></a>Iga VM administraatorina Corp\Install kasutaja lisamiseks tehke järgmist.

1. Oodake, kuni VM taaskäivitamist ja seejärel käivitage uuesti sisse logida **BUILTIN\AzureAdmin** kontot kasutades VM RDP-faili.

1. **Server Manager** valige **Tööriistad**ja seejärel klõpsake nuppu **Arvuti haldamine**.

    ![Arvuti haldamine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)

1. Klõpsake aknas **Arvuti haldamine** laiendage **Kohalikud kasutajad ja rühmad**ja valige **rühmad**.

1. Topeltklõpsake soovitud **administraatorite** rühma.

1. **Administraatorid atribuutide** dialoogiboksis, klõpsake nuppu **Lisa** .

1. Sisestage kasutaja **CORP\Install**ja seejärel klõpsake nuppu **OK**. Mandaadi küsimise korral kasutada **AzureAdmin** konto abil soovitud **Contoso! 000** parool.

1. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Atribuudid administraator** .

### <a name="add-the-failover-clustering-feature-to-each-vm"></a>Iga VM **Tõrkesiirdeklastrite** funktsioon lisada.

1. **Serveri halduri** armatuurlaual nuppu **Lisa rollid ja funktsioonid**.

1. **Rollide ja funktsioonide viisard**ja klõpsake nuppu **Järgmine** kuni kuvatakse lehel **funktsioonid** .

1. Valige **tõrkesiirdeklastrite**. Kui kuvatakse vastav viip, lisage sõltuvad funktsioonid.

    ![Rühmitamise funktsiooni VM Tõrkesiirde lisamine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)

1. Klõpsake nuppu **Järgmine**ja klõpsake **kinnitamise** lehel nuppu **Installi** .

1. Kui funktsioon **Tõrkesiirdeklastrite** installimine on lõpule viidud, klõpsake nuppu **Sule**.

1. Logige välja VM.

1. Korrake kõigi kolme serverid-- **ContosoWSFCNode**, **ContosoSQL1**ja **ContosoSQL2**selles jaotises toodud juhised.

SQL serveri VMs on nüüd ette valmistatud ja töötab, kuid installitakse SQL serveri vaikesuvandite.

## <a name="create-the-wsfc-cluster"></a>Looge WSFC kobar

Selles jaotises saate luua WSFC kobar kättesaadavus rühma loomist hiljem majutavad. Nüüd, peaks olete teinud iga kolme VMs, kui te kasutate WSFC kobar järgmist:

- Täielikult ette valmistatud Azure

- Ühendatud VM domeeni

- Lisatud **CORP\Install** kohalike administraatorite rühma

- Lisatud tõrkesiirdeklastrite funktsioon

Kõik need on iga VM eeltingimused, enne kui saate liitumiseks WSFC klaster.

Samuti võtke arvesse, et Azure virtuaalse võrgu käituda samamoodi nagu ka kohapealse võrgu. Peate looma klaster järgmises järjestuses:

1. Luua ühe sõlme kobar üks sõlmed (**ContosoSQL1**).

1. Muutke kobar IP-aadressi kasutamata IP-aadress (**10.10.2.101**).

1. Kuvage kobar nime võrgus.

1. Lisage muud sõlmed (**ContosoSQL2** ja **ContosoWSFCNode**).

Järgige neid ülesannete täielikult konfigureerib klaster alltoodud juhiseid.

1. Käivitage RDP-faili jaoks **ContosoSQL1** ja logige sisse kontoga domeeni **CORP\Install**.

1. **Serveri halduri** armatuurlaual, valige **Tööriistad**ja klõpsake **Tõrkesiirdeklastri halduri**.

1. Klõpsake vasakpoolsel paanil Paremklõpsake **Tõrkesiirdeklastri halduri**ja seejärel klõpsake nuppu **Loo klaster**, nagu allpool näidatud.

    ![Looge kobar](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)

1. Luua kobar viisardis loomiseks ühe sõlme kobar etapid lehtede allpool sätetega järgmist.

  	|Lehe|Sätted|
|---|---|
|Enne alustamist|Kasutage vaikesätteid.|
|Valige serverid|Tippige **ContosoSQL1** **Sisestage serveri nimi** ja klõpsake nuppu **Lisa**|
|Valideerimine hoiatus|Valige **nr ma ei nõua see Microsofti tugiteenuste ja seetõttu ei soovi valideerimise testi käivitamine. Edasi klõpsamisel loomise klaster jätkamiseks**.|
|Pääsupunktile klaster haldamiseks|Tippige **Cluster1** **kobar** nimi|
|Kinnituse|Kui kasutate salvestusruum, kasutage vaikesätted. Vt märkus pärast selle tabeli.|

    >[AZURE.WARNING] Kui kasutate [Salvestusruum](https://technet.microsoft.com/library/hh831739), mis pakuvad mitut ketast salvestusruumi kaustadesse, tuleb **lisada kõik õigus klaster** **kinnitamise** lehel ruut tühjendage ruut. Kui tühjendate selle suvandi, kuvatakse virtuaalse ketast lahti klastrite käigus. Selle tulemusena ka ei kuvata ketta halduri või Exploreri enne selle salvestusruum eemaldatakse kobar ja uuesti kasutada PowerShelli kaudu.

1. Vasakul paanil laiendage **Tõrkesiirdeklastri halduri**ja seejärel klõpsake nuppu **Cluster1.corp.contoso.com**.

1. Keskmisel paanil, liikuge kerides jaotiseni **Kobar Core ressursid** ja laiendada selle **nimi: Clutser1** üksikasjad. Peaksite nägema nii **nimi** ja **IP-aadressi** ressursside **nurjunud** olekus. IP-aadressi ressursi ei saa esitada online, kuna klaster on määratud enese, kui sama IP-aadress, mis on korduv aadress.

1. Nurjunud **IP-aadressi** ressursi paremklõpsake ja seejärel klõpsake käsku **Atribuudid**.

    ![Kobar atribuudid](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)

1. Valige **Staatiline IP-aadress** ja määrata **10.10.2.101** tekst väljale aadress. Seejärel klõpsake nuppu **OK**.

1. Paremklõpsake jaotises **Kobar Core ressursid** **nimi: Cluster1** ja klõpsake nuppu **Too veebis**. Oodake, kuni nii ressursid on võrgus. Kui online ressursi kobar nimi, värskendatakse näiteks Põhiliselt server AD uus konto. Kättesaadavusteenus rühma kobartulpdiagramm hiljem käivitamiseks kasutatakse AD selle kontoga.

1. Lisaks saate lisada ülejäänud sõlmed klaster. Puus brauseri Paremklõpsake **Cluster1.corp.contoso.com** ja klõpsake nuppu **Lisa sõlm**, nagu allpool näidatud.

    ![Klaster sõlme lisamine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)

1. **Sõlm viisardit**, klõpsake nuppu **edasi**. Klõpsake lehel **Valige serverid** **ContosoSQL2** ja **ContosoWSFCNode** loendisse lisada serveri nimi **Sisestage serveri nimi** ja seejärel klõpsata käsku **Lisa**. Kui olete lõpetanud, klõpsake nuppu **edasi**.

1. **Valideerimine hoiatus** lehel klõpsake nuppu **ei** (tootmise stsenaariumi tuleks teha katsed valideerimine). Klõpsake nuppu **edasi**.

1. **Klõpsake kinnituslehel** nuppu **Järgmine** sõlmed lisada.

    >[AZURE.WARNING] Kui kasutate [Salvestusruum](https://technet.microsoft.com/library/hh831739), mis pakuvad mitut ketast salvestusruumi kaustadesse, tuleb **lisada kõik õigus klaster** ruut tühjendage ruut. Kui tühjendate selle suvandi, kuvatakse virtuaalse ketast lahti klastrite käigus. Selle tulemusena ka ei kuvata ketta halduri või Exploreri enne selle salvestusruum eemaldatakse kobar ja uuesti kasutada PowerShelli kaudu.

1. Kui sõlmed on lisatud klaster, klõpsake nuppu **valmis**. Tõrkesiirdeklastri halduri peaks nüüd näitavad, et klaster on kolm sõlmed ja loendis **sõlmed** ümbrises.

1. Logige välja seansil.

## <a name="prepare-the-sql-server-instances-for-availability-group"></a>Kättesaadavus rühma SQL Serveri eksemplari ettevalmistamine

Selles jaotises tuleb teha nii **ContosoSQL1** ja **contosoSQL2**järgmist.

- **NT AUTHORITY\System** jaoks vajalikud õigused, mis on seatud vaikimisi SQL serveri eksemplar sisselogimist lisamine

- Lisada **CORP\Install** süsteemiadministraator rolli vaikimisi SQL serveri eksemplar

- SQL serveri Kaugpöördusteenuse tulemüüri avamine

- Alati klõpsake kättesaadavus rühmad funktsiooni lubamine

- Vastavalt **CORP\SQLSvc1** ja **CORP\SQLSvc2**, SQL Serveri teenuse konto muutmine

Neid toiminguid saab teha järjekorras. Siiski alltoodud juhiseid juhendab neid järjestuses. Nii **ContosoSQL1** ja **ContosoSQL2**tehke järgmist.

1. Kui te pole VM seansil välja logitud, tehke seda kohe.

1. Käivitage RDP-failid **ContosoSQL1** ja **ContosoSQL2** ja **BUILTIN\AzureAdmin**sisse logima.

1. Esmalt lisage **NT AUTHORITY\System** SQL serveri sisselogimise ja vajalikud õigused. Käivitage **SQL Server Management Studio**.

1. Klõpsake nuppu **Loo ühendus** ühenduse vaikimisi SQL serveri eksemplar.

1. **Objekti Exploreri**laiendage **Turvalisus**ja seejärel laiendage väärtus **logimine**.

1. Paremklõpsake **NT AUTHORITY\System** Logi sisse ja klõpsake käsku **Atribuudid**.

1. Kohaliku serveri **Securables** lehel Valige **Anna** järgmised õigused ja klõpsake nuppu **OK**.

    - Mis tahes kättesaadavus rühma muuta

    - Ühenduse loomine SQL-i

    - Kuva serveri olek

1. Järgmiseks lisada **CORP\Install** **süsteemiadministraator** rolli vaikimisi SQL serveri eksemplar. **Objekti Explorer**, paremklõpsake **sisselogimise** ja klõpsake nuppu **Uus Logi sisse**.

1. Tippige **CORP\Install** **kasutajanimi**.

1. Valige lehel **Serverirollide** **süsteemiadministraator**. Seejärel klõpsake nuppu **OK**. Kui login on loodud, näete seda laiendab Exploreris **Objekt** **logimine** .

1. Järgmiseks peate looma tulemüüri reegli SQL serveri. Käivitage kuval **käivitamine** **Täiustatud turvalisusega Windowsi tulemüür**.

1. Valige vasakul paanil **Sissetulevad reeglid**. Klõpsake parempoolsel paanil nuppu **Uus reegel**.

1. **Reegli tüüp** klõpsake lehel Valige **programm**ja seejärel klõpsake nuppu **edasi**.

1. Lehe **programmi** , valige **see programm tee** ja tippige **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** tekstivälja (kui teil on pärast neid juhiseid, kuid kasutades SQL Server 2012, SQL serveri kataloogi on **MSSQL11. MSSQLSERVER**). Klõpsake nuppu **edasi**.

1. Klõpsake lehel **toimingu** säilitada **ühenduse luba** valitud ja klõpsake nuppu **edasi**.

1. **Profiili** lehel aktsepteerige vaikesätted ja klõpsake nuppu **edasi**.

1. Lehe **nime** , saate määrata reegli nimi, näiteks **SQL serveri (reegli programmi)** väljale **nimi** ja seejärel klõpsake nuppu **valmis**.

1. Järgmiseks **Alati klõpsake kättesaadavus rühmad** funktsiooni lubada. Käivitage kuval **käivitage** **SQL Serveri konfigureerimishaldur**.

1. Puus brauseri nuppu **SQL Serveri teenuseid**, paremklõpsake teenust **SQL Server (MSSQLSERVER)** ja käsku **Atribuudid**.

1. **Alati On kõrge kättesaadavus** vahekaarti, seejärel valige **Luba alati klõpsake kättesaadavus rühmad**, nagu allpool näidatud ja seejärel klõpsake nuppu **Rakenda**. Klõpsake hüpikakna dialoogiboksis nuppu **OK** ja sulgege aken Atribuudid veel. SQL Serveri teenuse taaskäivitamist pärast seda, kui muudate teenuse konto.

    ![Luba alati kättesaadavus rühmad](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)

1. Järgmisena saate muuta SQL Serveri teenuse konto. Klõpsake vahekaarti **Sisselogimine** ja seejärel **CORP\SQLSvc1** (jaoks **ContosoSQL1**) või **CORP\SQLSvc2** (jaoks **ContosoSQL2**) tippige väljale **Konto nimi**ja seejärel täitke ja kinnitage parool ja seejärel klõpsake nuppu **OK**.

1. Hüpikakna, klõpsake nuppu **Jah** SQL Serveri teenuse taaskäivitama. Pärast taaskäivitamist SQL Serveri teenuse aknas atribuudid tehtud muudatused kehtivad.

1. Logige välja VMs.

## <a name="create-the-availability-group"></a>Kättesaadavus rühma loomine

Nüüd olete valmis kättesaadavus rühma konfigureerida. Allpool on ülevaade sellest, mida te teete:

- Luua uue andmebaasi (**MyDB1**) **ContosoSQL1**

- Teha nii täieliku varukoopia ja tehingulogi andmebaasi varukoopiat

- Täielik taastamine ja logige varukoopiate **ContosoSQL2** **NORECOVERY** suvand

- Kättesaadavus rühma (**AG1**) luua sünkroonse kinnitamine, automaatselt Tõrkesiirde ja loetavad teisene koopiad

### <a name="create-the-mydb1-database-on-contososql1"></a>ContosoSQL1 MyDB1 andmebaasi loomine

1. Kui te ei ole juba sisse loginud töölaua kaugseansi välja **ContosoSQL1** ja **ContosoSQL2**, tehke seda kohe.

1. Käivitage RDP-faili jaoks **ContosoSQL1** ja **CORP\Install**sisse logima.

1. Valige **File Exploreris**jaotises * *C:\**, looge kaust nimega * *varukoopia**. Kasutage seda directory abil ja oma andmebaasi taastamine.

1. Paremklõpsake uut kataloogi, valige käsk **Anna ühiskasutusse**ja valige **konkreetsed inimesed**, nagu allpool näidatud.

    ![Varukoopia kausta loomine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)

1. Lisada **CORP\SQLSvc1** ja anda sellele **Lugemis-ja kirjutamisõigusega** õigus, ja seejärel lisada **CORP\SQLSvc2** ja anda sellele **lugemisõigus,** nagu allpool näidatud ja seejärel nuppu **Anna ühiskasutusse**. Kui faili ühiskasutuse protsess on lõpule jõudnud, klõpsake nuppu **valmis**.

    ![Varukausta jaoks õiguste andmine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)

1. Järgmiseks andmebaasi loomist. Menüü **Start** , käivitage **SQL Server Management Studio**ja seejärel klõpsake nuppu **Loo ühendus** ühenduse vaikimisi SQL serveri eksemplar.

1. **Objekti Explorer**, paremklõpsake **andmebaasid** ja klõpsake käsku **Uus andmebaas**.

1. **Andmebaasi nime**, tippige **MyDB1**ja seejärel klõpsake nuppu **OK**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Võtma MyDB1 varukoopia ja taastada ContosoSQL2.

1. Järgmiseks võtate täieliku andmebaasi varukoopia. **Objekti Exploreri**Laienda **andmebaase**, seejärel paremklõpsake **MyDB1**, ja seejärel osutage nupule **Tööülesanded**ja seejärel klõpsake nuppu **Varunda**.

1. Säilita **Allika** jaotises **varundamise tüüp** väärtuseks **täielik**. Klõpsake jaotises **sihtkoht** vaikimisi faili tee varufaili eemaldamiseks **eemaldada** .

1. Klõpsake jaotises **sihtkoht** nuppu **Lisa**.

1. Tippige väljale **faili nimi** ** \\ContosoSQL1\backup\MyDB1.bak**. Seejärel klõpsake nuppu **OK**ja seejärel klõpsake nuppu **OK** uuesti andmebaasi varundada. Kui varukoopia toiming on lõpule jõudnud, klõpsake nuppu **OK** dialoogiboksi sulgemiseks.

1. Järgmiseks salvestatakse tehingu logifaili andmebaasi varukoopia. **Objekti Exploreri**Laienda **andmebaase**, seejärel paremklõpsake **MyDB1**, ja seejärel osutage nupule **Tööülesanded**ja seejärel klõpsake nuppu **Varunda**.

1. **Varundus** tüüp, valige **Tehingulogi**. Säilita **sihtkohta** faili tee määramine määratud varem ja klõpsake nuppu **OK**. Kui varukoopia toiming on lõpule jõudnud, klõpsake uuesti nuppu **OK** .

1. Järgmisena saate taastada **ContosoSQL2**täielik ja tehingu varundamine. Käivitage RDP-faili jaoks **ContosoSQL2** ja **CORP\Install**sisse logima. Jätke seansil **ContosoSQL1** avatud.

1. Menüü **Start** , käivitage **SQL Server Management Studio**ja seejärel klõpsake nuppu **Loo ühendus** ühenduse vaikimisi SQL serveri eksemplar.

1. **Objekti Explorer**, paremklõpsake **andmebaasid** ja klõpsake **Andmebaasi taastamine**.

1. **Andmeallika** jaotises Valige **seade**ja klõpsake **…** nupp.

1. **Valige varukoopia seadmed**, klõpsake nuppu **Lisa**.

1. Varundus faili asukohta, tippige \\ContosoSQL1\backup ja klõpsake siis nuppu Värskenda, seejärel valige MyDB1.bak, ja seejärel klõpsake nuppu OK ja siis uuesti nuppu OK. Nüüd peaks nähtaval olema täielik varukoopia ja varundamine Varunda määrab paani taastamiseks.

1. Minge lehele Suvandid ja seejärel valige taastamine NORECOVERY oleku taastamine ja klõpsake siis nuppu OK andmebaasi taastamine. Kui taastetoimingu lõpule jõudnud, klõpsake nuppu OK.

### <a name="create-the-availability-group"></a>Kättesaadavus rühma loomiseks tehke järgmist.

1. Minge tagasi seansil **ContosoSQL1**. **Objekti Exploreri** SSMS **Alati On kõrge kättesaadavus** paremklõpsake ja klõpsake nuppu **Uus kättesaadavuse nimel viisardi**, nagu allpool näidatud.

    ![Käivitage uus kättesaadavus rühma viisard](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)

1. **Sissejuhatus** lehel klõpsake nuppu **edasi**. Lehel **Määrake kättesaadavus rühma nimi** tippige **AG1** **kättesaadavus rühma nime**ja seejärel nuppu **edasi** .

    ![Uue AG viisardi, määrake AG nimi](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)

1. Klõpsake lehel **Valige andmebaaside** valige **MyDB1** ja klõpsake nuppu **edasi**. Andmebaasi vastab kättesaadavus rühma eeltingimused, kuna olete vähemalt üks täielik varukoopia ettenähtud esmane koopia.

    ![Valige uue AG viisardis andmebaasi](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)

1. Klõpsake lehel **Määrake koopiad** **Lisada koopia**.

    ![Uue AG viisardis määrata koopiad](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)

1. Kuvatakse dialoogiboks **Loo ühendus serveriga** . Tippige **ContosoSQL2** **Serveri nimi**ja seejärel klõpsake nuppu **Ühenda**.

    ![Uue AG viisardis serveriga](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)

1. Uuesti sisse lehel **Määrake koopiad** nüüd peaks nähtaval olema **ContosoSQL2** loendis **Saadaolevad koopiad**. Konfigureerida selle koopiad, nagu allpool näidatud. Kui olete lõpetanud, klõpsake nuppu **edasi**.

    ![Uue AG viisardis määrata koopiad (täielik)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)

1. Klõpsake lehel **Valige algse andmete sünkroonimine** valige **liituda ainult** ja klõpsake nuppu **edasi**. Teil on juba teostatud andmete sünkroonimine käsitsi kui võttis **ContosoSQL1** täielik ja tehingu varukoopia ja taastada nende **ContosoSQL2**kohta. Selle asemel saate teha varukoopia ja toiminguid andmebaasi taastamine ja valige **täielik** uus kättesaadavuse nimel viisardi andmete sünkroonimine saate lasta. Kuid see ei ole soovitatav väga suurte andmebaaside, mis leiduvad mõned suurettevõtete jaoks.

    ![Valige uue AG viisardis algandmed](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)

1. Klõpsake **kinnitamise** lehel nuppu **edasi**. Selle lehe peaks sarnanema järgmisega all. Seal on kuulajale konfiguratsiooni hoiatus, sest mõne kättesaadavus rühma kuulajale pole konfigureeritud. Hoiatus, võite ignoreerida, sest selles õpetuses Konfigureeri on kuulajale. Pärast selle õpetuse kuulajale konfigureerimiseks vaadake [konfigureerimine on ILB kuulajale alati klõpsake kättesaadavus rühmade Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

    ![Uue AG viisardis valideerimine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)

1. **Kokkuvõte** lehel klõpsake nuppu **valmis**ja oodake, kuni viisard konfigureerib kättesaadavus uus rühm. **Edenemise** leht, võite klõpsata üksikasjalik edenemise kuvamiseks **rohkem üksikasju** . Kui viisard on lõpule jõudnud, kontrolli **tulemusi** lehe veenduge, et rühma kättesaadavus on loodud, nagu allpool näidatud ja seejärel klõpsake viisardi sulgemiseks nuppu **Sule** .

    ![Uue AG viisardis tulemused](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)

1. **Objekti Exploreri**, laiendage **Alati On kõrge kättesaadavus**, siis Laienda **Rühmad kättesaadavus**. Nüüd näete selle ümbrises jaotises Uus kättesaadavus. Paremklõpsake **AG1 (primaarne)** ja klõpsake nuppu **Kuva armatuurlaud**.

    ![Kuva AG armatuurlaud](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)

1. **Alati kohta armatuurlaua** peaks välja nägema umbes allpool näidatud. Saate vaadata selle koopiad, Tõrkesiirde režiimi iga koopia ja sünkroonimise olek.

    ![AG armatuurlaud](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)

1. Naaske **Server Manager**, valige **Tööriistad**ja seejärel käivitage **Tõrkesiirdeklastri halduri**.

1. Laiendage **Cluster1.corp.contoso.com**, ja seejärel laiendage **teenused ja rakendused**. Valige **rollid** ja arvestage, et **AG1** kättesaadavus rühma roll on loodud. Pange tähele, et AG1 ei saa mis tahes IP-aadress, mille andmebaasi saate klientrakendustega kättesaadavaks rühma, kuna te ei konfigureerimine on kuulajale. Saate luua ühenduse otse esmane sõlm lugemis-ja kirjutamisõigusega toimingute ja teisene sõlm kirjutuskaitstud päringuid.

    ![AG tõrkesiirdeklastri halduris](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

>[AZURE.WARNING] Proovige üle soovitud tõrkesiirdeklastri halduri nurjumise. Kõik Tõrkesiirde toimingud tuleks teha **Alati kohta armatuurlaua** SSMS sees. Lisateabe saamiseks vt [piirangute kohta abil The WSFC tõrkesiirdeklastri halduri kättesaadavus rühmad](https://msdn.microsoft.com/library/ff929171.aspx).

## <a name="next-steps"></a>Järgmised sammud
Olete nüüd edukalt rakendatud SQL serveri alati klõpsake Azure kättesaadavus rühma loomise abil. Konfigureerimine on kuulajale selle kättesaadavus rühma jaoks, vaadake [konfigureerimine on ILB kuulajale alati klõpsake kättesaadavus rühmade Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Muud Azure SQL serveri kasutamise kohta leiate artiklist [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md).
