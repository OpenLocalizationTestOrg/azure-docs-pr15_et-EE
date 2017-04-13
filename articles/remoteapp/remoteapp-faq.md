<properties 
    pageTitle="Azure RemoteApp KKK | Microsoft Azure'i" 
    description="Siit saate teada, vastused korduma kippuvad küsimused Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Azure RemoteApp KKK

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Oleme kuulnud Azure RemoteApp järgmised küsimused. Teised on? Külastage [RemoteApp Foorumid](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) ja andke meile teada, mida on vaja teada või rippmenüüst kommentaari all.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Te ei leia, mida otsite? Küsimus me ei vastata?
Kui te ei leia soovitud teavet vajate või teil on veel küsimus, mida me ei kataks siin, minge [Azure RemoteApp Foorum](http://aka.ms/araforum) ja esitage oma küsimus seal. Alati lisame rohkem vastuseid siin.

## <a name="what-is-azure-remoteapp"></a>Mis on Azure RemoteApp? ##


- **Mis on Azure RemoteApp?** RemoteApp on Azure teenuse abil saate pakkuda turvaline, remote Accessi rakenduste paljude erinevate kasutaja seadmetes. Lugege lisateavet [Azure RemoteApp](remoteapp-whatis.md).
- **Mis on juurutamise suvandid?** On kahte tüüpi RemoteApp saidikogumid: pilveteenuste ja hübriid. Millist peate sõltub arvu tegureid, nagu kas vajate domeeni Liitu. Kõik nimetatud räägitakse [siin](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Näpunäiteid kasutades Azure RemoteApp ##
- **Kui kaua, kuni ma olen lahti? Kui kaua olla jõudeolekus enne teie mulle selle buutimine?** 4 tundi. Kui teie või teie kasutajad on jõude 4 tundi, logitakse automaatselt sisse Azure RemoteApp välja. Leiate [Azure'i tellimus ja teenuste piirangud, kvootide](../azure-subscription-service-limits.md)ja piiranguid muude vaikesätted.
- **Võite proovida seda teenust tasuta?** Jah. 30 päeva on saadaval tasuta prooviversioon. Pärast prooviperioodi lõppu saate sujuv (mille abil saate valmistamisel) makstud konto või teenuse kasutamise lõpetamine. Alustuseks tasuta prooviversiooni [portal.azure.com](http://portal.azure.com) – saate luua uue eksemplari RemoteApp. Tasuta prooviversiooni, saate koostada 2 eksemplari RemoteApp 10 kasutajatega eksemplari kohta. Pidage meeles, et see prooviversiooni elu ainult 30 päeva.
## <a name="azure-remoteapp-subscription-details"></a>Azure RemoteApp Tellimuse üksikasjad ##

- **Millised piirangud teenuse?** Saate tutvuda vaikesätted ja teenuste piirangud Azure RemoteApp [Azure'i tellimus ja teenuste piirangud, kvootide](../azure-subscription-service-limits.md)ja piiranguid. Andke meile teada, kui teil on veel küsimusi.
- **Mitu kasutajat mul on?** On 20 kasutajate vähemalt. Andke korrata, et super selge – minimaalne on 20. Teile kuvatakse arveid 20. 
- **Kui palju maksab RemoteApp hind?** Lugege teemat [Azure RemoteApp hinnad üksikasjad ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Saidikogumi ühte tüüpi maksab rohkem kui teise?** Jah, on võimalik, sõltuvalt teie saidikogumi nõuetele. Hübriidjuurutuse saidikogumi jaoks on vaja kohapealse võrgu kaudu Azure RemoteApp ühendus. Kui kasutate olemasolevaid VNET/Express protsesse, on tasuta. Kuid kui kasutate uut Azure'i VNET ja lüüsi või teekonna, siis võetakse [VPN-lüüsi](https://azure.microsoft.com/pricing/details/vpn-gateway) või [Teekonna](https://azure.microsoft.com/pricing/details/expressroute/). Selle maksumus (üksikasjalik linkide) on teie igakuine Azure RemoteApp peal maksumus.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Saidikogumite - mis on toetatud, mida te kasutate, ja teised
- **Kohandatud –-ärivaldkonna (LOB) rakenduste toetab?** Jah. Kohandatud rakenduse kasutamiseks Azure RemoteApp, luua [Kohandatud malli pilti](remoteapp-create-custom-image.md)ja siis laadige see RemoteApp kogum.
- **Minu kohandatud LOB rakendus töötab Azure RemoteApp?** Parim viis sellest proovimislink on aru. Tutvuge [RD ühilduvus keskele](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Millist juurutamise meetodit (pilve või hübriid) on parim oma asutuse jaoks?** Hübriidjuurutuse saidikogumid pakuvad ülevaatlikumaid rakendusi, kui soovite täielik integreerimine ühekordse sisselogimise (SSO) ja turvalise asutusesisese võrguühendus. Pilveteenuse saidikogumid pakuvad on kiire ja lihtne viis mitme autentimismeetodite abil juurutamise eristamiseks. Lugege lisateavet [Juurutussuvandid](remoteapp-whatis.md).
- **Meil on SQL-i või muu andmebaasi kas kohapeal või Azure. Mis tüüpi juurutamise tuleks kasutada?** See sõltub sellest, kus on andmebaasi SQL-i või taustväärtus. Kui andmebaas on privaatvõrk, kasutage hübriid saidikogumi. Kui andmebaas on Interneti ja lubab kliendi ühendusi selle ühendamiseks, saate kasutada cloud saidikogumi.
- **Kuidas draivi vastendamine, USB ja järjestikune portide, lõikelaua ühiskasutus ja printeri ümbersuunamine?** Kõik need funktsioonid on toetatud Azure RemoteApp. Lõikelaua ühiskasutus ja printeri ümbersuunamine on vaikimisi lubatud. Saate lisateavet ümbersuunamise [siin](remoteapp-redirection.md). 


## <a name="template-images"></a>Malli pildid
- **Kasutada pilve või olemasoleva virtuaalse masina Mall minu RemoteApp saidikogumi jaoks?** Jah! Saate luua pildi põhjal on Azure VM, kasutage ühte tellimus sisaldab pilte või luua kohandatud pildi. Tutvuge [RemoteApp pilt suvandid](remoteapp-imageoptions.md).


## <a name="network-options"></a>Võrgu suvandid
- **Hübriidjuurutuse saidikogumi jaoks on vaja mõnda VNET. Saate kasutada meie olemasoleva VNET?** Saate teha, kui olemasolev VNET on mõni Azure VNET. Lugege teemat "samm 1: virtuaalse võrgu häälestamine" Lisateavet [hübriid saidikogumi juhised](remoteapp-create-hybrid-deployment.md) .
- **Saate kasutada mõnda VNET cloud saidikogumi?** Tõepoolest saate. [Pilveteenuse saidikogumi loomine](remoteapp-create-cloud-deployment.md), eriti samm 1, lisateabe saamiseks vaadake.

## <a name="authentication-options"></a>Autentimise suvandid



- **Kuidas on lood autentimist? Millised võimalused on toetatud?** Pilveteenuse saidikogumi toetab Microsofti kontod ja Azure Active Directory kontod, mis on Office 365 kontode ning. Hübriidjuurutuse saidikogumi toetab ainult Azure Active Directory kontod, mis on sünkroonitud (nt [Azure Active Directory sünkroonimise](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)tööriista kasutamine) kaudu Windows Server Active Directory juurutuse; eelkõige sellest, kas sünkroonitud parooli sünkroonimise suvandi või sünkroonitud Active Directory Federation Services (AD FS) liiduserveri konfigureeritud. Samuti saate konfigureerida [Mitme teguriga autentimine (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]Azure Active Directory kasutajad peavad olema rentnikust, mis on teie tellimusega seotud. (Saate vaadata ja muuta oma tellimuse portaalis vahekaardil **sätted** . Lisateavet [muutmine Azure Active Directory rentniku kasutatavaid RemoteApp](remoteapp-changetenant.md) .)

- **Miks ei saa anda oma Azure Active Directory kontot juurdepääs?** Azure Active Directory kasutajad peavad olema kataloogist, mis on teie tellimusega seotud. Saate vaadata või muuta selle kausta portaalis vahekaardil sätted. Lisateabe saamiseks vaadake [muutmine Azure Active Directory rentniku kasutatavaid RemoteApp](remoteapp-changetenant.md) .

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Kliendi - seade, mida saate kasutada juurdepääsuks Azure RemoteApp?
Hea kliendi teavet, sh eri klientidele [juurdepääs Azure RemoteApp rakenduste](remoteapp-clients.md)installimist otsimine

- **Millised seadmed ja operatsioonisüsteemid kliendi rakendused toetab?**
Esimene arvutite ja tahvelarvutites. 
    - Windows 10 (kliendi eelvaade)
    - Windows 8.1 ja Windows 8
    - Windows 7 Service Pack 1
    - Mac OS X
    - Windows RT
    - Androidi tahvelarvutite versioonis
    - iPadis ja telefonid:
    - iPhone
    - Androidi telefon
    - Windows Phone
 
    [Laadige alla](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) RemoteApp klient kohe.
- **Azure RemoteApp ei toeta õhuke kliendid?** Jah, on toetatud järgmised Windows manustatud õhuke kliendid:
    - Windowsi varjatud standardit 7
    - Manustatud Windows 8 Standard
    - Manustatud Windows 8.1 Industry Pro
    - Windows 10 asjade Enterprise

- **Millist Windowsi serveri versioon on toetatud jaoks soovitud kaugtöölaua seansi hosti (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Tugi- ja tagasiside


- **Mis on RemoteApp toe kavandamine?** Arvelduste ja tellimustega halduse tugi on saadaval tasuta. Tehniline tugi on saadaval [Azure teenuse lepingute](https://azure.microsoft.com/support/plans/)kaudu. Samuti saate tasuta ühenduse abi meie [Azure Foorum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)kaudu. 
- **Kuidas saavad tagasiside edastada?** Külastage [tagasiside Foorum](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Kes saab lisateavet Azure RemoteApp rääkida?** Lisaks meie [Foorum arutelu](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), mis on hea koht postitage sinna küsimusi, saate liituda nädala [Ask Veebiseminar asjatundjate](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), kus räägitakse kõike RemoteApp.
- **Kuidas on lood RemoteApp dokumentatsiooni?** Pakume nii teilt. Spikri sisu portaali Lisaks aitab sahtlis (klõpsake lihtsalt **?** mis tahes lehel portaalis), järgmistes artiklites on saadaval, saate õppida kõike RemoteApp:
    - **Alustamine:**
        - [Mis on RemoteApp?](remoteapp-whatis.md)
        - [Mis on RemoteApp malli pilti?](remoteapp-images.md)
        - [Hulgilitsentsimise tööpõhimõtted](remoteapp-licensing.md)
        - [Kuidas RemoteApp ja Office koos töötavad?](remoteapp-o365.md)
        - [Ümbersuunamise tööpõhimõtted RemoteApp](remoteapp-redirection.md)?
    - **Juurutada:**
        - [Luua kohandatud malli pilti](remoteapp-create-custom-image.md)
        - [Hübriidjuurutuse saidikogumi loomine](remoteapp-create-hybrid-deployment.md)
        - [Pilveteenuse saidikogumi loomine](remoteapp-create-cloud-deployment.md)
        - [Azure Active Directory konfigureerimine RemoteApp](remoteapp-ad.md)
        - [Rakenduse RemoteApp avaldamine](remoteapp-publish.md)
    - **Haldamiseks tehke järgmist.**
        - [Kasutajate lisamine](remoteapp-user.md)
        - [Konfigureerimise ja RemoteApp kasutamise head tavad](remoteapp-bestpractices.md)  

    Videod! Meil on ka arvu RemoteApp videod. Mõned annavad Sissejuhatus ([tutvustus Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) ajal teistega sõelub juurutamise ([pilve juurutamise](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) ja [hübriidjuurutuse](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Vaadake neid!

 
### <a name="help-us-help-you"></a>Aidake meil aidata teil 
Kas teadsite, et lisaks selles artiklis hindamine ja muutes kommentaaride alla all, saate muuta ise artiklit? Midagi puudu? Midagi on valesti? Kas midagi, mis on lihtsalt selgem kirjutada? Liikuge kerides üles ja klõpsake nuppu **Redigeeri github** muudatuste tegemiseks – need tulevad meile läbivaatuseks ja siis, kui me logige välja need, näete oma muudatused ja täiustused siinsamas.
