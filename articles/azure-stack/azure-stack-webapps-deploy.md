<properties
    pageTitle="Azure'i virnas Web Apps ressursi pakkuja lisamine | Microsoft Azure'i"
    description="Üksikasjalikud juhised veebirakenduste Azure'i virnas"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Azure'i virnas veebirakenduste ressursi pakkuja lisamine

> [AZURE.NOTE] Järgmine teave kehtib ainult Azure virnas TP1 juurutuste.

Web Appsi ressursi pakkuja lisamine Azure'i virnas on seitse juhiseid.

1.  Laadige alla nõutavad komponendid.
2.  Azure'i virnas veebirakendustes kasutatav sertide loomine.
3.  Allalaadimiseks etapi ja installige Azure'i virnas veebirakendustes installiprogrammi abil. 
4.  Kinnitage Web Appsi installi.
5.  DNS-kirjete loomine ees ja Management serveri koormus soolise jaoks.
6.  Äsja juurutatud veebirakenduste ressursi pakkuja registreeruma ARM.
7.  Test Drive Web Appsi ressursi pakkuja.

## <a name="download-required-components"></a>Laadige alla nõutavad komponendid

1.  Laadige alla [Azure'i virnas rakenduse teenus eelvaade installer](http://aka.ms/azasinstaller). 
2.  [Azure'i virnas rakenduse teenuse juurutamise helper skriptide](http://aka.ms/azashelper)allalaadimiseks. 
3.  Failide eraldamiseks helper skriptide zip-fail, on kolm skriptide:
    - Looge AppServiceCerts.ps1
    - Looge AppServiceDnsRecords.ps1
    - Register-AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Azure'i virnas veebirakendustes kasutatav sertide loomine

See esimese skript töötab 3 serdid, mida veebirakenduste vaja luua Azure'i virnas sertimiskeskus. Käivitage skript ClientVM, tagades, et kasutate PowerShelli azurestack\administrator:
1.  Kui **azurestack\administrator**PowerShelli seansi käivitamine **Loomine – AppServiceCerts.ps1** skripti  See loob kolme serdid, skripti, mis on vajalik veebirakenduste samasse kausta.
2.  Sisestage parool secure pfx-failid ja märkige üles selle peate sisestamine Azure'i virnas Web Apps Installer.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Laadige alla ja installige Azure'i virnas veebirakendustes installiprogrammi abil

Appservice.exe Installeri järgmist:
1.  Küsi kasutaja nõustumiseks Microsofti ja muude tootjate kontsessioone.
2.  Azure'i virnas juurutamise teabe kogumine.
3.  Luua bloobimälu container määratud Azure'i virnas salvestusruumi konto.
4.  Laadige failid, mis on vaja installida Azure virnas veebirakenduse ressursside pakkuja.
5.  Ettevalmistused installi juurutamiseks veebirakenduse ressursside pakkuja Azure'i virnas keskkonnas.
6.  Laadige failid üles rakendus teenusekonto salvestusruumi.
7.  Esita võrgukoosolekuga Azure'i ressursihaldur malli vajalikku teavet.

Järgmised toimingud juhatavad teid installimise etapid:

>[AZURE.NOTE] Installiprogrammi käivitamiseks peate kasutama tõstetud konto (kohalik või domeeni administraator). Kui logite sisse nagu azurestack\azurestackuser, küsitakse teilt, kas kõrgemal mandaati. 

1.  Käivitada appservice.exe **azurestack\administrator**. 
2.  Klõpsake käsku **Deploy abil Azure'i ressursihaldur**.

![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 Installer][1]

3.  Vaadake üle aktsepteerida väljalaske-eelse Microsofti tarkvara litsentsitingimused läbi ja seejärel klõpsake nuppu **edasi**.
4.  Vaadake üle kolmas partylicense nõustumine ja seejärel klõpsake nuppu **edasi**.
5.  Rakenduse pilveteenuses konfiguratsiooni teave üle ja klõpsake nuppu **edasi**.

![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 rakenduse teenus Cloud konfigureerimine][2]

6. Klõpsake nuppu **Ühenda** (Azure'i virnas tellimuste kõrval).

![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 rakenduse teenus Cloud konfiguratsiooni ekraani kahe][3]

7.  Sisestage akna Azure'i virnas autentimist **Azure Active Directory teenuse administraator konto** ja **parool**ning seejärel klõpsake nuppu **Logi sisse**.
**Märkus:** See on esitatud Azure virnas juurutamisel Azure Active Directory konto.
    - Klõpsake **Allanoolt** paremas servas **Azure'i virnas tellimuste** kõrval olev ruut ja seejärel valige oma tellimuse.

![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 tellimuse valimine][5]

8.  Klõpsake **Allanoolt** **Azure'i virnas asukohad**ruut paremal.
    - Valige **kohalik**.
9. Sisestage **nimi** oma administraatori poole.
10. Sisestage administraatori **parool** .
11. **SQL serveri andmeid** vaadata ja muuta, kui see on nõutav.
12. **Süsteemiadministraator Login konto** üle vaadata ja muuta, kui see on nõutav.
13. Sisestage **Parool süsteemiadministraator**.
14. Klõpsake nuppu **edasi**.  Selles etapis nüüd installer ühenduse üksikasju kinnitamine SQL serveri jaoks.

![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 tellimuse valimine][4]    

15. Klõpsake nuppu **Sirvi** **Veebi rakenduste vaikimisi SSL-i serdi faili** nime kõrval ja liikuge **veebirakenduste. AzureStack.Local** serdi [varem loodud](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
16. Sisestage **serdi parool** , mida saate määrata, kui olete loonud serdid.
17. Klõpsake nuppu **Sirvi** **Ressursi pakkuja SSL-i serdi faili** nime kõrval ja liikuge **haldus. AzureStack.Local** serdi [varem loodud](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
18. Sisestage **serdi parool** , mida saate määrata, kui olete loonud serdid.
19. Klõpsake nuppu **Sirvi** **Ressursi pakkuja Root serdi faili** nime kõrval ja liikuge **haldus. AzureStack.Local** serdi [varem loodud](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
20. Klõpsake nuppu **Järgmine** Installiprogramm kontrollib esitatud serdi parool.

![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 serdi üksikasjade][6]

Juurutamise võtab umbes 45-60 minutit lõpuleviimiseks.

![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 installimise edenemine][7]

21. Pärast installer on edukalt lõpule jõudnud, klõpsake käsku **välju**.

## <a name="validate-web-apps-installation"></a>Kinnitage Web rakenduste installimine

1.  Avage **Azure'i virnas hosti arvuti** **Hyper-V haldur**.
2.  Otsige üles **CN0-VM** ja **ühenduse** VM.
![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 Hyper-V haldur][8]
3.  See VM töölaual topeltklõps **Web Cloud halduskonsool**.
![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 halduskonsooli][9]
4.  Liikuge **hallatavate serverites**.
5.  Kui kõik masinad on **valmis** jätkake järgmise juhisega. 
![Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 hallatud serverite olek][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>DNS-kirjete loomine Management Server ja esiosa koormus soolise
1.  Avage PowerShelli eksemplari nimega **azurestack\administrator**.
2.  Liikuge alla ja [eelnevalt nõutud samm](#Download-Required-Components)ekstraktimist skriptid.
3.  **Loo-AppServiceDnsRecords.ps1** skripti käivitada see loob DNS-i kirjed lubada portaali ja web rakendusetaotluste marsruutida Front End serverid.  Ressursi pakkuja ja loodi ARM juurutamisel veebirakenduste, kaks tarkvara koormus soolise (SLBs). Need käsk Management serverid ja Front End serverid. Portaali ja ARM põhise taotluste Azure'i virnas rakendust Service ressursi pakkuja minge Management Server.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>Äsja juurutatud Azure'i virnas veebirakenduste pakkuja registreerida ARM
1.  Avage PowerShelli eksemplari nimega **azurestack\administrator**.
2.  Liikuge alla ja [eelnevalt nõutud samm](#Download-Required-Components)ekstraktimist skriptid.
3.  Käivitage **Register-AppServiceResourceProvider.ps1** skript. 

>[AZURE.NOTE] Kui sisestasite selle **Virtual Machine(s) administraator** ja **parool** väljadele installimine või registreerimise ressursi pakkuja ei õnnestu, tippige kasutajanimi ja parool **täpselt (sh ülemise ja alumise puhul)** .

## <a name="test-drive-azure-stack-web-apps"></a>Testi ketas Azure virnas Web Apps

Nüüd, kui teil on juurutatud ja veebirakenduste ressursi pakkuja registreeritud, saate selle veenduge, et rentnikud juurutamine veebirakenduste testida.

1.  Azure'i virnas portaalis, klõpsake nuppu Uus, klõpsake Web + Mobile ja suvandit Web App.
2.  Veebirakenduse tera, tippige nimi väljale Web app.
3.  Ressursirühm, klõpsake jaotises nuppu Uus ja seejärel tippige nimi väljale ressursirühma. 
4.  Klõpsake rakenduse leping/asukoht ja klõpsake nuppu Loo uus.
5.  Rakenduse teenuse leping tera, tippige nimi väljale rakenduse teenuse leping.
6.  Klõpsake hinnakirjad taseme, klõpsake tasuta-ühiskasutuses või ühiskasutuses-ühiskasutuses, nuppu Vali, klõpsake nuppu OK ja seejärel klõpsake nuppu Loo.
7.  Vähem kui minutiga uue veebirakenduse paani ilmub armatuurlaual. Klõpsake paani.
8.  Labale web app nuppu Sirvi, et kuvada Vaikeveebisait selle rakenduse.


**(Valikuline) WordPress, DNN või Django veebisaidi juurutamine**

1. Azure'i virnas portaalis, klõpsake nuppu (+), avage Azure'i turuplatsi, juurutada Django veebisait ja oodake, kuni edukaks. Django web platvormi kasutab faili süsteemi-põhine andmebaas ja ei nõua mis tahes täiendavaid ressursi pakkujad nagu SQL-i või MySQL-i.  

2. Kui olete juurutanud ka MySQL-i ressursikeskuse pakkuja, saate juurutada WordPress veebilehel turuplatsilt. Kui teil palutakse andmebaasi parameetrite, sisestage soovitud kasutaja nimi, nagu *User1@Server1* (koos kasutaja nimi ja serveri nime).

3. Kui olete juurutanud ka SQL serveri ressursi pakkuja, saate juurutada DNN veebisaidi turuplatsilt. Kui teil palutakse andmebaasi parameetrite, valige andmebaasi arvutis töötab SQL serveri, mis on ühendatud ressursi pakkuja.

## <a name="next-steps"></a>Järgmised sammud

Võite proovida ka muud [service (PaaS) teenused platvorm](azure-stack-tools-paas-services.md), nagu [SQL serveri ressursi pakkuja](azure-stack-sql-rp-deploy-short.md) ja [MySQL-i ressursikeskuse pakkuja](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
