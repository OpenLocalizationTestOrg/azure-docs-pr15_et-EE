<properties
    pageTitle="Azure'i virnas POC juurutamine | Microsoft Azure'i"
    description="Saate teada, kuidas valmistada Azure'i virnas POC ja käivitage PowerShelli skripti Azure virnas POC juurutamiseks."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Azure'i virnas POC juurutamine
Azure'i virnas POC juurutamiseks peate esmalt [ettevalmistamine juurutamise arvuti](#prepare-the-deployment-machine) ja seejärel [käivitage PowerShelli skripti juurutamise](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Laadige alla ja Microsoft Azure'i virnas POC TP2 eraldamiseks

Enne alustamist veenduge, et teil olema vähemalt 85 GB kõvakettaruumi.

1. Azure'i virnas POC TP2 allalaadimine hulka kuuluvad järgmised 12 failid, summeerimine ~ 20 GB zip-fail:
    - 1 MicrosoftAzureStackPOC.EXE
2. Lugege läbi litsentsileping Kuva ja teavet iseavanevate viisardi ja klõpsake nuppu **edasi**.
3. Vaadake üle ekraan privaatsusavalduse ja teavet iseavanevate viisardi ja klõpsake nuppu **edasi**.
4. Valige failid sihtkoht ekstraktita, klõpsake nuppu **edasi**.
    - Vaikimisi on: <drive letter>:\<praeguse kausta > \Microsoft Azure virnas POC
5. Kontrollige sihtkoha asukoht Kuva ja teavet iseavanevate viisardi ja klõpsake **ekstraktida** CloudBuilder.vhdx (~44.5 GB) ja ThirdPartyLicenses.rtf failide ekstraktimiseks.

> [AZURE.NOTE] Pärast failid ekstraktida, saate kustutada ruumi vabastamiseks seadmesse zip-fail. Või teisaldada zip-fail mõnda muusse asukohta, nii et kui teil on vaja ümberkorraldamine, mida pole vaja uuesti alla laadida zip-failid.

## <a name="prepare-the-deployment-machine"></a>Juurutamise arvuti ettevalmistamine

1. Veenduge, et te füüsilise ühenduse juurutamise arvutisse, või füüsilise konsooli pääsevad (nt KVM). Juurdepääs peate pärast te juhises 9 juurutamise arvuti taaskäivitama.

2. Veenduge, et juurutamise arvuti vastab [miinimumnõuetele](azure-stack-deploy.md). [Juurutamise kontroll Azure virnas Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) abil saate kinnitada, et teie vajadustele.

3. Logige sisse kui kohalik administraator POC teie arvutis.

4. Kopeerige fail CloudBuilder.vhdx C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Kui te ei soovi kasutada soovitatav skript POC hosti arvuti ettevalmistamiseks (juhised 5 – juhis 7), sisestage litsentsi võti aktiveerimise lehele. Prooviversiooni Windows Server 2016 pilt on lisatud ja sisestada võti põhjustab aegumise hoiatusteated.

5. Käivitage arvutis POC järgmist PowerShelli skripti Azure virnas TP2 tugi faile alla laadida.

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    See skript allalaadimist Azure'i virnas TP2 tugi failide $LocalPath parameetriga määratud kausta.
    
6. Avage mõni administraatoriõigustes PowerShelli konsooli ja muuta kataloogi, kuhu kopeerisite failid.
    - 11 MicrosoftAzureStackPOC N.BIN (kus N on 1-11)
7. Paremklõpsake soovitud MicrosoftAzureStackPOC.EXE > Käivita administraatorina.

8. Käivitage PrepareBootFromVHD.ps1 skript. See skript ja järelevalveta failid on saadaval koos selle koostamine esitatud tugi skripte.
    On viis parameetrite seda PowerShelli skripti:
    - CloudBuilderDiskPath (nõutav) – CloudBuilder.vhdx hosti tee.
    - DriverPath (valikuline) – saate lisada täiendavad draiverid selle hosti virtuaalse HD.
    - (Valikuline) – ApplyUnattend määrata automatiseerimiseks operatsioonisüsteemi konfiguratsiooni parameeter vahetamine. Kui määratud, peab kasutaja esitama AdminPassword konfigureerida OS käivitamisel (nõuab antud lisatud faili unattend_NoKVM.xml).
    Kui kasutate selle parameetri, kasutatakse faili üldise unattend.xml ilma täpsemaks kohandamine. Pärast seda, kui ta taaskäivitamisega on vaja KVM täielik kohandamine.
    - (Valikuline) – AdminPassword kasutada ainult siis, kui ApplyUnattend parameeter on seatud, jaoks on vaja vähemalt kuus märki.
    - (Valikuline) VHDLanguage – saate määrata VHD keelt, vaikimisi "en-US".
    Skripti on ja see sisaldab näiteks kasutamine, kuigi kõige levinum kasutamine on:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Kui käivitate selle täpse käsu, peate sisestama AdminPassword kaudu.

9. Kui script on lõpule jõudnud, peate kinnitama ning taaskäivitage arvuti. Kui teised kasutajad sisse logitud, see käsk nurjub. Kui käsk nurjub, käivitage järgmine käsk:`Restart-Computer -force` 

10. Selle HOSTI taaskäivitamisega CloudBuilder.vhdx, kus juurutamise endiselt OS sisse.

> [AZURE.IMPORTANT] Azure'i virnas nõuab Interneti-ühendus, otseselt või läbipaistvaid puhverserveri. Juurutamise TP2 POC toetab täpselt ühe NIC võrgunduse. Kui teil on mitu NICs, veenduge, et on lubatud ainult ühe (ja kõik teised on keelatud) enne järgmise jaotise juurutamise skripti käivitamist.

## <a name="run-the-powershell-deployment-script"></a>Käivitage PowerShelli skripti juurutamine

1. Logige sisse kui kohalik administraator POC teie arvutis. Kasutage eelmistes toimingutes määratud mandaadid.

2. Avage mõni administraatoriõigustes PowerShelli konsooli.

3. Käivitage PowerShelli, see käsk:`cd C:\CloudDeployment\Configuration`

4. Käivitage käsk deploy.`.\InstallAzureStackPOC.ps1`

5. **Sisestage parool** kuvatakse vastav viip, sisestage parool ja seejärel kinnitage seda. See on kõik virtuaalmasinates parool. Kindlasti salvestage see.

6. Sisestage konto Azure Active Directory mandaat. Kasutaja peab olema üldadministraator directory rentnik.

7. Juurutamise protsess võib kuluda mitu tundi, mille jooksul süsteemi automaatselt taaskäivitamisega üks kord.

    > [AZURE.IMPORTANT] Kui soovite juurutamise edenemist jälgida, logige sisse azurestack\AzureStackAdmin. Kui logite sisse kohaliku administraatorina pärast seda, kui seade on ühendatud domeeni, ei kuvata juurutamise edenemist. Käivitage uuesti juurutus, selle asemel logige sisse azurestack\AzureStackAdmin kinnitada, et see töötab.

    Juurutamise õnnestub, kuvatakse PowerShelli konsooli: **lõpuleviimine: toimingu 'Juurutamise'**.

    Kui juurutamise nurjub, proovige [see](azure-stack-rerun-deploy.md)uuesti. Või saate [ümberkorraldamine](azure-stack-redeploy.md) selle algusest peale.

### <a name="deployment-script-examples"></a>Juurutamise skripti näited

Kui teie identiteedi AAD on ainult üks AAD Directory seostatud:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Kui teie identiteedi AAD on seostatud rohkem kui üks AAD kataloog.

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Kui teie keskkond, mis ei ole lubatud DHCP, lisage järgmised täiendavad parameetrid ühele suvandi kohal (antud näites kasutusega):

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>InstallAzureStackPOC.ps1 valikulised parameetrid

| Parameetri | Nõutav/valikuline | Kirjeldus |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Valikuline. | Määrab Azure Active Directory kasutajanimi ja parool. Azure'i identimisteabe võib olla ka ettevõtte ID- või Microsofti Account. Kasutada Microsofti Account identimisteavet, jätke see parameeter cmdlet. See parameeter faktitabeli palub Azure'i autentimise hüpikaknas käigus juurutamist. See loob autentimis- ja Värskenda märkide juurutamisel kasutada. |
| AADDirectoryTenantName | Nõutav | Määrab rentniku kataloogi. Selle parameetri abil määrata teatud kataloogid, kus AAD konto on õigused hallata mitut kausta. Täielik nimi kujul AAD Directory rentniku <directoryName>. onmicrosoft.com. |
| AdminPassword | Nõutav | Komplekti osana POC juurutamise loodud virtuaalmasinates kohaliku administraatorikonto ja kõik kasutajakontod. See parool peab vastama hosti praegune kohaliku administraatori parool. |
| AzureEnvironment | Valikuline. | Valige Azure'i keskkonda, millega soovite selle Azure'i virnas juurutamise registreerida. Suvandite hulka kuuluvad *Avaliku Azure*, *Azure - Hiina*, *Azure - USA valitsuse*. |
| EnvironmentDNS | Valikuline. | DNS-i server on loodud Azure'i virnas juurutamise käigus. Lubamiseks sees lahendus nimede väljaspool templi arvutites, sisestage oma olemasoleva taristu DNS-i serveri. Tempel DNS-i server ettepoole tundmatu nime eraldusvõime nõuab selles serveris. |
| NatIPv4Address | Nõutav DHCP NAT tugiteenuste | Määrab MAS-BGPNAT01 staatiline IP-aadress. Kasutage seda parameetrit ainult kui DHCP ei saa määrata Interneti lubatud IP-aadress. |
| NatIPv4DefaultGateway | Nõutav DHCP NAT tugiteenuste | Määrab vaikimisi lüüsi MAS-BGPNAT01 kasutada staatiline IP-aadressiga. Kasutage seda parameetrit ainult kui DHCP ei saa määrata Interneti lubatud IP-aadress.  |
| NatIPv4Subnet | Nõutav DHCP NAT tugiteenuste | IP alamvõrgu eesliide kasutada DHCP NAT tugi. Kasutage seda parameetrit ainult kui DHCP ei saa määrata Interneti lubatud IP-aadress.  |
| PublicVLan | Valikuline. | Määrab VLAN ID-ga. Kui host ja MAS-BGPNAT01 tuleb konfigureerida VLAN ID-d juurdepääsu füüsilise võrgu (ja Internet) kasutada ainult see parameeter. Näiteks`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Käivita uuesti | Valikuline. | Kasutage selle lipu juurutamise uuesti.  Eelmise panus, mida kasutatakse. Uuesti sisestamisega varem esitatud andmed ei toetata, kuna mitu kordumatuid väärtusi on loodud ja juurutamise jaoks kasutada. |
| Myötäilijä | Valikuline. | Kui teil on vaja määrata mõne kindla kellaaja server, kasutage seda parameetrit. |

## <a name="next-steps"></a>Järgmised sammud

[Ühenduse loomine Azure virnas](azure-stack-connect-azure-stack.md)
