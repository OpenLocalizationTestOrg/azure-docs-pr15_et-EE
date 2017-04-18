<properties 
    pageTitle="PowerShelli abil saate luua VM Omarežiimis aruandeserveri | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse ja juhendab teid juurutus- ja SQL serveri aruandlusteenuste omarežiimis aruandeserveri on Azure virtuaalse masina konfigureerimine. "
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

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>PowerShelli kasutamine Omarežiimis Report Server Azure'i VM loomiseks

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Selles teemas kirjeldatakse ja juhendab teid juurutus- ja SQL serveri aruandlusteenuste omarežiimis aruandeserveri on Azure virtuaalse masina konfigureerimine. Juhised selle dokumendi abil saate luua virtuaalse masina ja konfigureerimiseks aruandlusteenuste VM Windows PowerShelli skripti kombinatsiooni juhised. Konfigureerimise skripti sisaldab HTTP- või HTTPs tulemüüri portide avamine.

>[AZURE.NOTE] Kui teil pole vaja **HTTPS** aruande serveris, **jätkake 2**.
>
>Pärast loomist juhises 1 VM liikuge jaotisse Kasuta skripti aruandeserveri ja HTTP konfigureerimiseks. Pärast skripti aruande server on kasutamiseks valmis.

## <a name="prerequisites-and-assumptions"></a>Eeltingimused ja oletused

- **Azure'i tellimus**: valdkond saadaval Azure tellimuse arvu. Kui loote soovitatav VM suurus **A3**, peate **4** saadaval tuumad. Kui kasutate VM suurus **a2**, peate **2** saadaval tuumad.
    
    - Tellimuse Azure klassikaline portaalis core limiit kinnitamiseks nuppu sätted vasakul paanil ja seejärel klõpsake kasutamine ülemises menüüs.
    
    - Core talletusmahu suurendamiseks tugiteenuste [Azure](https://azure.microsoft.com/support/options/). VM suurus leiate teemast [Azure virtuaalse masina suurused](virtual-machines-linux-sizes.md).

- **Windows PowerShelli skripti**: teema eeldab, et teil on Windows PowerShelli põhiteadmisi töötamine. Windows PowerShelli kasutamise kohta lisateabe saamiseks vaadake järgmist:

    - [Windows PowerShelli käivitamine Windows serveris](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Windows PowerShelli töötamise alustamine](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Samm 1: Ettevalmistamise Azure'i virtuaalarvuti

1. Liikuge sirvides Azure klassikaline portaali.

1. Klõpsake vasakpoolsel paanil nuppu **Virtuaalmasinates** .

    ![Microsoft Azure'i virtuaalmasinates](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Klõpsake nuppu **Uus**.

    ![nupp uus](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Klõpsake **galeriis**.

    ![uue vm galeriist](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Klõpsake noolt jätkamiseks klõpsake **SQL Server 2014 RTM Standard – Windows Server 2012 R2** .

    ![järgmise](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Kui teil on vaja juhitud tellimuste funktsiooni aruandlusteenuste andmed, valige **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. SQL serveri versioonide ja funktsioonide toe kohta leiate lisateavet teemast [SQL Server 2012 väljaannetes toetatud funktsioonid](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).

1. Lehel **virtuaalse masina konfiguratsioon** redigeerimine järgmised väljad:
                                    
    - Kui seal on rohkem kui üks **versioon VÄLJAANDMISAEG**, valige kõige uuem versioon.
    
    - **Virtuaalse masina nimi**: masina nimi kasutatakse ka konfiguratsiooni järgmisel lehel vaikimisi pilvepõhise teenuse DNS-i nimi. DNS-i nimi peab olema kordumatu Azure'i teenus. Kaaluge seadistamine VM arvuti nimi, mis kirjeldab VM kasutada. Näiteks ssrsnativecloud.
    
    - **Esimese taseme**: Standard
    
    - **Suurus: A3** on soovitatav VM suurus SQL serveri töökoormus. Kui VM kasutatakse ainult aruandeserveri, VM suurus a2 on piisavalt juhul, kui aruande server kogemusi suur töökoormus. VM hinnad teavet, leiate teemast [Virtuaalmasinates hinnad](https://azure.microsoft.com/pricing/details/virtual-machines/).
    
    - **Uus kasutaja nimi**: esitate nimi on loodud VM administraatorina.
    
    - **Uus parool** ja **Kinnita**. Uue administraatorikonto kasutatakse seda parooli ja soovitatav on kasutada keeruka parooli.
    
    - Klõpsake nuppu **edasi**. ![järgmise](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Järgmisel lehel Redigeeri järgmised väljad:

    - **Pilveteenuses**: valige **Loo uus pilveteenuses**.
    
    - **Pilveteenuse teenuse DNS-i nimi**: see on pilveteenuses, mis on seotud VM avaliku DNS-i nimi. Vaikenimi on sisestatud VM nimi nimi. Kui hiljem juhiseid teema, usaldusväärse SSL-i serdi loomine ja seejärel DNS-i nimi kasutatakse funktsiooni "**abil välja antud**" sertifikaadi väärtus.
    
    - **Piirkonna/osaleja rühma/Virtual Network**: saate valida oma lõppkasutajad kõige lähemal regiooni.
    
    - **Salvestusruumi konto**: kasutage automaatselt loodud salvestusruumi kontot.
    
    - **Kättesaadavus**: pole.
    
    - **Lõpp-punktid** Säilita **Kaugtöölaua** ja **PowerShelli** lõpp-punktid ja seejärel lisage HTTP või HTTPS-i lõpp-punkti, olenevalt teie keskkonnast.

        - **HTTP**: vaikimisi avalike ja privaatvõ pordid **80**. Pange tähele, et muuta privaatseks pordi 80, v.a kui **$HTTPport = 80** http skripti.

        - **HTTPS**: avalike ja privaatvõ vaikeport on **443**. Levinud on muuta privaatseks pordi ja teie tulemüür ja kasutada privaatne pordi serveri konfigureerimine. Lõpp-punktid kohta leiate lisateavet teemast [teatise üles virtuaalse masina abil](virtual-machines-windows-classic-setup-endpoints.md). Pange tähele, et muuta pordi 443, v.a kui parameetri **$HTTPsport = 443** HTTPS skripti.
    
    - Klõpsake nuppu edasi. ![järgmise](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Viisardi viimasel lehel võtke **installige VM agent** valitud vaikimisi. Selles artiklis kirjeldatud juhiseid kasutada VM agent, kuid kui kavatsete seda VM hoida, VM agent ja laiendid võimaldab teil parandada ta CM.  VM agent kohta leiate lisateavet teemast [VM Agent ja laiendid – 1 osa](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Üks vaikimisi laiendid installitud ad töötab "BGINFO" täiendus, mis kuvab VM töölaualt, nagu sisemine IP ja vaba kettaruumi Süsteemiteave.

1. Klõpsake nuppu valmis. ![Ok](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. VM **olek** kuvatakse **alustades (Provisioning)** käigus sätte ja kuvab nimega **töötab** VM korral ettevalmistatud ja kasutamiseks valmis.

## <a name="step-2-create-a-server-certificate"></a>Samm 2: Serveri serdi loomine

>[AZURE.NOTE] Kui teil pole vaja HTTPS aruande serveris, saate, **jätkake 2,** ja minge jaotisse **Kasuta skripti aruandeserveri ja HTTP konfigureerimiseks**. HTTP skripti abil saate kiiresti konfigureerida aruande ja aruande server on kasutamiseks valmis.

VM HTTPS kasutamiseks peab olema usaldusväärne SSL-sert. Vastavalt stsenaariumile, saate kasutada ühte järgmistest kaks võimalust:

- Lubatud SSL-serdi väljaandja järgi soovitud sertimiskeskuse (CA) ja Microsoft usaldada. CA juursertide vajalike levitatakse Microsoft Root Certificate programmi kaudu. Programmi kohta leiate lisateavet teemast [Windows ja Windows Phone 8 SSL-i Root Certificate Program (liikme CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) ja [The Microsofti juursertide programmis tutvustus](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Iseallkirjastatud sert. Iseallkirjastatud serdid pole soovitatav tootmise keskkonnas.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Kasutada, usaldusväärsete sertimiskeskuse (CA) väljastatud serti loodud sert

1. **Taotleda Serveri sert mõnelt sertimisorganilt veebisaidi jaoks**. 

    Saate veebi serveri serdi viisardi luua sertifikaadi taotluse faili (Certreq.txt) sertimiskeskus saadetavate või luua taotluse vastav online sertimiskeskus. Näiteks Microsoft serdi Services Windows Server 2012. Sõltuvalt ID assurance pakutud oma serveri sert, on mitu päeva mitu kuud oma taotluse kinnitamine ja saata teile serdi faili sertimiskeskus. 

    Paluda serveri sertide kohta lisateabe saamiseks vaadake järgmist: 
    
    - Kasutage [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
    
    - Turvalisus tööriistad haldamine Windows Server 2012.

    [Turvalisus tööriistad haldamiseks Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Usaldusväärse SSL-serdi välja **antud** peaks olema sama, mis oli uue VM **Pilvepõhise teenuse DNS-i nimi** .

1. **Serveri serdi veebiserverisse installida**. Veebiserver on sel juhul majutava serveri VM ja veebisaidi loomist hiljem juhiseid aruandlusteenuste konfigureerimisel. Serdi MMC lisandmooduli abil veebiserver serveri serdi installimise kohta leiate lisateavet teemast [installida serveri sert](https://technet.microsoft.com/library/cc740068).
    
    Kui soovite kasutada kaasas selles teemas skripti, konfigureerida aruande serdid **sõrmejälje** väärtus parameetrina skripti nõutav. Vaadake, kuidas saamiseks serdi sõrmejälje üksikasjad järgmisest jaotisest.

1. Määrata serdi serveri aruandeserveris. Ülesande täidetakse järgmises jaotises serveri konfigureerimisel.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Funktsiooni Virtuaalmasinates kasutada iseallkirjastatud sert

Iseallkirjastatud serdi loodi VM kui VM on ette valmistatud. Serdi on sama nimi nagu VM DNS-i nimi. Serdi tõrgete vältimiseks on nõutav, et sert on usaldusväärne VM ise ja ka kõik saidi kasutajad.

1. Serdi kohaliku VM juurkaustas CA usaldamise lisada **Trusted Root sertimiskeskuste**sert. Järgmises kokkuvõtte etapid. Üksikasjalikud juhised selle kohta, kuidas usaldada CA leiate teemast [serveri serdi installimine](https://technet.microsoft.com/library/cc740068).

    1. Azure'i klassikaline portaalis valige VM ja klõpsake nuppu Ühenda. Olenevalt teie brauseri konfiguratsioon, võidakse teilt ühenduse VM RDP-faili salvestada.
    
        ![azure virtuaalse masina ühendamine](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kasutaja VM nimi, kasutajanimi ja parool, mis on konfigureeritud VM loomisel kasutada. 
    
        Näiteks järgmisel pildil VM nimi on **ssrsnativecloud** ja kasutajanimi on **testuser**.
        
        ![Kasutajanimi sisaldab vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Käivitage mmc.exe. Lisateavet leiate teemast [kohta: sertide kuvamine abil Halduskonsooli lisandmoodul](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. Konsooli rakenduse menüü **fail** , lisandmooduli **serdid** lisamine, valige vastava viiba **Arvuti konto** ja seejärel klõpsake nuppu **edasi**.
    
    1. Valige **Kohaliku arvuti** haldamine ja seejärel klõpsake nuppu **valmis**.
    
    1. Klõpsake nuppu **Ok** ja seejärel laiendage soovitud **sertifikaatide - isikliku** sõlmed ja seejärel klõpsake nuppu **serdid**. Serdi pärast DNS-i nime VM nimega ja lõpeb **cloudapp.net**. Paremklõpsake serdi nimi ja seejärel klõpsake käsku **Kopeeri**.
    
    1. Laiendage **Trusted Root sertimiskeskuste** ja paremklõpsake **serdid** ja seejärel klõpsake käsku **Kleebi**.
    
    1. Kinnitage, topeltklõpsake jaotises **Trusted Root sertimiskeskuste** serdi nimi ja veenduge, et vigu pole ja kuvatakse teie sert. Kui soovite kasutada HTTPS skripti selles teemas kaasas, konfigureerida aruande serdid **sõrmejälje** väärtus parameetrina skripti nõutav. **Sõrmejälje väärtuse saamiseks**tehke järgmist. Olemas on ka PowerShelli valimi toomiseks sõrmejälje jaotises [Kasuta skripti aruandeserveri ja HTTPS-i konfigureerimine](#use-script-to-configure-the-report-server-and-HTTPS).
        
        1. Topeltklõpsake sert, näiteks ssrsnativecloud.cloudapp.net nime.
        
        1. Klõpsake vahekaarti **üksikasjad** .
        
        1. Klõpsake **sõrmejälje**. Funktsiooni sõrmejälje väärtus kuvatakse üksikasjad väljal, näiteks a6 08 3c df f9 0b f7 e3 7c 25 de a4 de 7e ac 91 9c 2c fb 2f.
        
        1. Kopeerige soovitud sõrmejälje ja väärtuse salvestamine hilisemaks või skripti nüüd redigeerida.
        
        1. (*) Enne skripti eemaldada vahepeal väärtuste paare tühikuid. Näiteks sõrmejälje, märkida enne oleks nüüd a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
        
        1. Määrata serdi serveri aruandeserveris. Ülesande täidetakse järgmises jaotises serveri konfigureerimisel.

Kui kasutate iseallkirjastatud SSL-sert, klõpsake serdi nimi vastab juba hostname VM. Seetõttu DNS-i masina globaalselt juba registreeritud ja saab kasutada mis tahes klient.

## <a name="step-3-configure-the-report-server"></a>Samm 3: Konfigureerimine Report Server

Selles jaotises juhendab teid konfigureerida nii aruandlusteenuste omarežiimis aruande server VM. Üht järgmistest meetoditest abil saate konfigureerida aruande:

- Skripti abil saate konfigureerida aruanne

- Konfigureerida Report Server Configuration Manager abil.

Üksikasjalikud juhised leiate jaotisest [ühenduse virtuaalse masina ja alustada aruannete Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Autentimise Märkus:** Windowsi autentimine on soovitatav autentimise meetodit ja see on vaikimisi aruandlusteenuste autentimist. Ainult need kasutajad, kes on konfigureeritud VM pääseb aruandlusteenuste ja aruandlusteenuste rollidele määratud.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Skripti abil saate konfigureerida aruandeserveri ja http kaudu

Windows PowerShelli skripti abil saate konfigureerida aruande, tehke järgmist. Konfiguratsiooni sisaldab HTTP, mitte HTTPS:

1. Azure'i klassikaline portaalis valige VM ja klõpsake nuppu Ühenda. Olenevalt teie brauseri konfiguratsioon, võidakse teilt ühenduse VM RDP-faili salvestada.

    ![azure virtuaalse masina ühendamine](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kasutaja VM nimi, kasutajanimi ja parool, mis on konfigureeritud VM loomisel kasutada. 

    Näiteks järgmisel pildil VM nimi on **ssrsnativecloud** ja kasutajanimi on **testuser**.
    
    ![Kasutajanimi sisaldab vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Avage **Windows PowerShell ISE** VM, administraatoriõigustega. Windows Server 2012 vaikimisi installitakse PowerShell ISE. Soovitatav on kasutada ISE asemel standard Windows PowerShelli aknas, et saate ISE skripti kleepimine, muuta skripti ja seejärel käivitage skript.

1. Windows PowerShell ISE, klõpsake menüü **Vaade** ja seejärel klõpsake nuppu **Kuva skripti paani**.

1. Kopeerige järgmine skript ja skripti kleepimine skripti paani Windows PowerShell ISE.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Kui olete loonud VM HTTP pordiga peale 80, muuta parameetri $HTTPport = 80.

1. Aruandlusteenuste skripti praegu konfigureeritud. Kui soovite skripti käivitamiseks aruandlusteenuste jaoks, muutke tee "v11" Get-WmiObject aruande nimeruumi versioon osa.

1. Käivitage skript.

**Valideerimine**: Veenduge, et põhiaruande serveri funktsioonid töötavad, vt [kinnitamine konfiguratsiooni](#verify-the-configuration) käesoleva teema.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Skripti abil saate konfigureerida aruandeserveri ja HTTPS

Windows PowerShelli abil saate konfigureerida aruande, tehke järgmist. Konfiguratsiooni sisaldab HTTPS, mitte HTTP.

1. Azure'i klassikaline portaalis valige VM ja klõpsake nuppu Ühenda. Olenevalt teie brauseri konfiguratsioon, võidakse teilt ühenduse VM RDP-faili salvestada.

    ![azure virtuaalse masina ühendamine](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kasutaja VM nimi, kasutajanimi ja parool, mis on konfigureeritud VM loomisel kasutada. 

    Näiteks järgmisel pildil VM nimi on **ssrsnativecloud** ja kasutajanimi on **testuser**.

    ![Kasutajanimi sisaldab vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Avage **Windows PowerShell ISE** VM, administraatoriõigustega. Windows Server 2012 vaikimisi installitakse PowerShell ISE. Soovitatav on kasutada ISE asemel standard Windows PowerShelli aknas, et saate ISE skripti kleepimine, muuta skripti ja seejärel käivitage skript.

1. Töötava skriptide lubamiseks käivitage järgmine Windows PowerShelli käsk:

        Set-ExecutionPolicy RemoteSigned

    Seejärel saate käivitada kinnitamiseks poliitika järgmist:

        Get-ExecutionPolicy

1. **Windows PowerShell ISE**, klõpsake menüü **Vaade** ja seejärel klõpsake nuppu **Kuva skripti paani**.

1. Järgmise skripti kopeerimine ja kleepimine skripti paani Windows PowerShell ISE.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Skripti parameetri **$certificatehash** muutmiseks tehke järgmist.

    - See on **nõutav** parameeter. Kui te ei salvestanud serdi väärtust kaudu eelmisi juhiseid, kasutage üht järgmistest meetoditest kopeerimiseks serdi räsi väärtust serdid sõrmejälje.:

        VM, avage Windows PowerShell ISE ja käivitage järgmine käsk:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        Väljund sarnanema järgmisega. Kui skript tagastab tühja rea, VM on konfigureeritud näiteks sert, vaadake jaotist [Virtuaalmasinates Self-signed serti kasutada](#to-use-the-virtual-machines-self-signed-certificate).
    
    VÕI
    
    - Käivitage VM mmc.exe ja seejärel lisage lisandmooduli **serdid** .
    
    - Topeltklõpsake jaotises **Trusted Root sertimiskeskuste** sõlm teie serdi nimi. Kui kasutate iseallkirjastatud sert VM, sertifikaadi VM DNS-i nime järele nimega ja lõpeb **cloudapp.net**.
    
    - Klõpsake vahekaarti **üksikasjad** .
    
    - Klõpsake **sõrmejälje**. Funktsiooni sõrmejälje väärtus kuvatakse üksikasjad väljal, näiteks af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48
    
    - **Enne skripti**, eemaldage tühikud vahepeal väärtuste paare. Näiteks af1160b64b288d890a8212ff6ba9c3664f319048

1. **$httpsport** parameetri muutmiseks tehke järgmist. 

    - Kui kasutasite HTTPS-i lõpp-punkti pordi 443, siis pole vaja värskendada skripti parameeter. Muul juhul kasutatakse pordi väärtus valitud HTTPS privaatne lõpp-punkti konfigureerimisel VM.

1. **$DNSName** parameetri muutmiseks tehke järgmist. 

    - Skripti on konfigureeritud lubama looduses kaardi serdi $DNSName = "+". Kui te ei tee pole soovite konfigureerimine metamärkide serdi sidumine, kommenteerida välja $DNSName ="+"ja lubada järgmine rida, täielik $DNSNAme viide, ## $DNSName="$server.cloudapp.net".

        $DNSName väärtust muuta, kui te ei soovi kasutada aruandlusteenuste virtuaalse masina DNS-i nimi. Parameetri kasutamisel serdi tuleb kasutada ka selle nimi ja registreerimist globaalselt sisse DNS-serveri nime.

1. Aruandlusteenuste skripti praegu konfigureeritud. Kui soovite skripti käivitamiseks aruandlusteenuste jaoks, muutke tee "v11" Get-WmiObject aruande nimeruumi versioon osa.

1. Käivitage skript.

**Valideerimine**: Veenduge, et põhiaruande serveri funktsioonid töötavad, vt [kinnitamine konfiguratsiooni](#verify-the-connection) käesoleva teema. Serdi kinnitamiseks sidumine avage käsuviip administraatoriõigustega ja käivitage järgmine käsk:

    netsh http show sslcert

Tulem on järgmised:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Konfigureerimine Report Server Configuration Manager abil

Kui te ei soovi konfigureerida aruande PowerShelli skripti käivitamiseks, järgige selles jaotises konfigureerimine serveri aruandlusteenuste omarežiimis konfigureerimishalduri abil.

1. Azure'i klassikaline portaalis valige VM ja klõpsake nuppu Ühenda. Kasutajanimi ja parool, mis on konfigureeritud VM loomisel kasutada.

    ![azure virtuaalse masina ühendamine](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Käivitage Windows update ja installige värskendused VM. Kui nõutakse uuesti VM, taaskäivitage VM ja taastada VM Azure klassikaline portaalist.

1. VM menüü Start kaudu tippige **Aruandlusteenuste** ja avage **Teenuste konfigureerimishaldur teatamine**.

1. Jätke vaikeväärtused **Serveri nimi** ja **Aruande serveri eksemplar**. Klõpsake nuppu **Loo ühendus**.

1. Klõpsake vasakul paanil **Veebiteenuse URL**.

1. Vaikimisi RS on konfigureeritud lubama IP "Kõik määratud" HTTP port 80. HTTPS lisamiseks tehke järgmist.

    1. **SSL-**serdi: valige sert, mida soovite kasutada, nt [VM nimi]. cloudapp.net. Kui puuduvad serdid on loetletud, vaadake jaotist **Samm 2: serveri serdi loomine** Lisateavet selle kohta, kuidas installida ja usalda serdi VM.
    
    1. **SSL-i pordi**alusel: valige 443. Kui olete konfigureerinud HTTPS privaatne lõpp-punkti VM erinevate privaatne Port, kasutage selle väärtuse siin.
    
    1. Klõpsake nuppu **Rakenda** ja oodake, kuni toiming lõpule jõuab.

1. Klõpsake vasakpoolsel paanil suvandit **andmebaas**.

    1. Klõpsake nuppu **Muuda Databas**e.
    
    1. Klõpsake nuppu **Loo uus aruanne serveri andmebaas** ja seejärel klõpsake nuppu **edasi**.
    
    1. Jätke vaikimisi **Serveri nimi**: nimega VM nime ja jätke vaikesätted **Autentimistüüp** **Praeguse kasutaja** – **Integreeritud turvalisus**. Klõpsake nuppu **edasi**.
    
    1. Jätke vaikimisi **aruandeserveri** **Andmebaasi nimi** ja klõpsake nuppu **edasi**.
    
    1. Jätke vaikimisi **Mandaati** **Autentimistüüp** ja klõpsake nuppu **edasi**.
    
    1. Klõpsake nuppu **Järgmine** lehe **Kokkuvõte** .
    
    1. Kui konfiguratsioon on lõpule jõudnud, klõpsake nuppu **valmis**.

1. Klõpsake vasakul paanil **Aruandehalduri URL-i**. Jätke vaikimisi **aruannete** **Virtuaalne kaust** ja klõpsake nuppu **Rakenda**.

1. Klõpsake käsku **välju** aruandlus Services Configuration Manager sulgemiseks.

## <a name="step-4-open-windows-firewall-port"></a>Samm 4: Ava Windowsi tulemüüri portide

>[AZURE.NOTE] Kui kasutasite stsenaariumi aruande konfigureerida, võite selle jaotise vahele jätta. Skripti lisada etapi tulemüüri portide avamiseks. Vaikimisi on portide 80 http ja HTTPS-pordi 443.

Ühenduse kaugühenduse teel aruandehalduri või serveri virtual arvutisse, TCP lõpp on vaja VM. See on vajalik sama pordi avamine soovitud VM tulemüüri. Lõpp-punkti loodi, kui VM on ette valmistatud.

Selles jaotises antakse põhiteabe tulemüüri portide avamise kohta. Lisateavet leiate teemast [konfigureerimine aruande serveri juurdepääsu tulemüüri](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] Kui kasutasite skripti aruande konfigureerida, võite selle jaotise vahele jätta. Skripti lisada etapi tulemüüri portide avamiseks.

Kui olete konfigureerinud privaatne pordi HTTPS peale 443, muuta järgmist skripti õigesti. Avage Windowsi tulemüür pordi **443** , tehke järgmist:

1. Avage Windows PowerShelli aken administraatoriõigustega.

1. Kui kasutasite pordi 443 peale HTTPS-i lõpp-punkti konfigureerimisel VM, värskendada pordi järgmine käsk ja seejärel käivitage käsk:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Kui käsk on lõpule jõudnud, **Ok** , kuvatakse Käsuviip.

Veenduge, et port on avatud, avage Windows PowerShelli aken ja käivitage järgmine käsk:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Kontrollige konfiguratsiooni

Põhiaruande serveri funktsiooni nüüd töötamise kontrollimiseks avada brauseri administraatoriõigustega ja liikuge sirvides järgmised server ad aruanne aruandehalduri URL-id.

- VM, liikuge aruande serveri URL:

        http://localhost/reportserver

- VM, liikuge aruande sõim URL:

        http://localhost/Reports

- Liikuge kohalikust arvutist **remote** aruande halduri VM. Värskendage DNS-i nimi järgmises näites vastavalt vajadusele. Parooli küsimise korral kasutada administraatori identimisteave, mis on loodud, kui VM on ette valmistatud. Kasutaja nimi on selle domeeni\[kasutaja nimi] vorming, kui domeen on VM arvuti nimi, näiteks ssrsnativecloud\testuser. Kui te ei kasuta HTTP-**S**, eemaldage **s** URL-i. Lugege järgmise jaotise teavet loomise kohta täiendavate kasutajate VM.

        https://ssrsnativecloud.cloudapp.net/Reports

- Liikuge kohalikust arvutist remote aruande serveri URL. Värskendage DNS-i nimi järgmises näites vastavalt vajadusele. Kui te ei kasuta HTTPS, eemaldage s URL-i.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Kasutajate loomine ja rollide määramine

Pärast konfigureerimise ja kinnitatava serveri, levinud haldus ülesanne on luua ühe või mitme kasutaja ja määrata kasutajatele aruandlusteenuste rollid. Lisateabe saamiseks vaadake järgmist:

- [Uue kasutajakonto loomine](https://technet.microsoft.com/library/cc770642.aspx)

- [Anda kasutaja juurdepääs aruandeserveri (aruandehalduri)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Saate luua ja hallata rollimääranguid](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Luua ja avaldada Azure virtuaalse masina aruanded

Järgmises tabelis on toodud mõned kohapealse arvutist olemasolevaid aruandeid aruandeserveris majutatud sisse Microsoft Azure virtuaalse masina avaldada valikuid:

- **RS.exe skripti**: kasutamine RS.exe skripti aruande üksuste ja olemasoleva aruande serveri abil oma Microsoft Azure'i virtuaalse masina kopeerida. Lisateabe saamiseks vaadake jaotise "Omarežiimis Omarežiimis – Microsoft Azure virtuaalse masina" [valimi aruandlusteenuste rs.exe skripti migreerimine sisu aruande meiliserverite vahel](https://msdn.microsoft.com/library/dn531017.aspx).

- **Report Builder**: virtuaalse masina sisaldab klõpsake – üks kord versiooni Microsoft SQL Server Report Builder. Virtuaalne arvutisse aruande builder esimest korda alustamiseks tehke järgmist.

    1. Alustage brauseri administraatoriõigused.
    
    1. Liikuge sirvides arvutis virtual aruandehalduri ja **Report Builder** lindil nuppu.

    Lisateabe saamiseks vt [installimine ja eemaldate, Report Builder toetavad](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: VM**: kui olete loonud VM koos SQL Server 2012, siis SQL Server Data Tools virtual arvutisse on installitud ja saab luua **Aruande serveri projekte** ja aruandeid virtual arvutisse. SQL Server Data Tools saate avaldada aruannete virtual arvutisse aruandeserveris.
    
    Kui olete loonud VM SQL serveri 2014, saate installida SQL serveri andmete tööriistad - BI visual Studio. Lisateabe saamiseks vaadake järgmist:

    - [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [SQL Server Data Tools ja SQL Server Business Intelligence (SSDT-BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: Remote**: kohalikus arvutis aruandlusteenuste projekti loomine SQL Server Data Tools, mis sisaldab aruandlusteenuste aruandeid. Project web teenuse URL-i ühenduse konfigureerimine.

    ![SSDT projekti atribuutide SSRS projekti](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Kasuta skripti**: aruande serveri sisu kopeerimine skripti abil. Lisateavet leiate teemast [valimi aruandlusteenuste rs.exe skripti migreerimine sisu aruande meiliserverite vahel](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Kui te ei kasuta VM, minimeerida kulu

>[AZURE.NOTE] Kulude jaoks oma Azure'i Virtuaalmasinates kui ei ole kasutusel minimeerimiseks sulgeda VM Azure klassikaline portaalist. Kui kasutate Windows power suvandid sees VM VM sulgeda, on endiselt sama palju VM eest. Kulude vähendamiseks peate sulgeda VM Azure klassikaline portaalis. Kui teil pole enam vaja VM, ärge unustage kustutamine VM ja seotud .vhd failide salvestusruumi kulude vältimiseks. Lisateabe saamiseks lugege teemat veebisaidil [Virtuaalmasinates hinnad üksikasjad](https://azure.microsoft.com/pricing/details/virtual-machines/)jaotisest KKK.

## <a name="more-information"></a>Lisateave

### <a name="resources"></a>Ressursid

- Sarnase sisuga, mis on seotud ühe serveri juurutamine SharePoint 2013 ja SQL Server Business Intelligence, teemast [Windows PowerShelli abil luua Azure'i VM koos SQL serveri BI ja SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).

- Sarnase sisuga, mis on seotud mitme serveri juurutuse SharePoint 2013 ja SQL Server Business Intelligence, leiate teemast [Juurutamine SQL Server Business Intelligence Azure'i Virtuaalmasinates](https://msdn.microsoft.com/library/dn321998.aspx).

- Üldteavet SQL Server Business Intelligence Azure'i Virtuaalmasinates juurutustes, vt [SQL Server Business Intelligence Azure'i Virtuaalmasinates](virtual-machines-windows-classic-ps-sql-bi.md).

- Lisateavet Azure maksumus arvutada kulude leiate teemast [Azure hinnakirjad kalkulaator](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines)Virtuaalmasinates vahekaarti.

### <a name="community-content"></a>Kogukonna sisu

- Samm-sammult juhised, kuidas luua aruandluse teenuste kohalikke režiimi aruandeserveri skripti kasutamata, vt [Majutusteenuse SQL-i aruandlusteenuste teenuse Azure virtuaalse masina](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Muud ressursid SQL Server Azure'i VMs lingid

[SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md)
