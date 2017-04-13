<properties 
    pageTitle="Mis on Azure RemoteApp? | Microsoft Azure'i" 
    description="Saate teada, kuidas rakendusi ja mis tahes seadme kaudu Azure RemoteApp ressursse ühiskasutusse andmine." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Mis on Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp toob kohapealse Microsoft RemoteApp programm, Azure Kaugtöölauateenuseid, mida toetab funktsioone. Azure RemoteApp aitab teil pakkuda turvaline, remote Accessi rakenduste paljude erinevate kasutaja seadmetes. Azure RemoteApp põhiliselt majutab püsiv Terminal Server seansid pilves, ja saate neid kasutada ja jagada neid kasutajaid.

Azure'i RemoteAppi saate jagada rakendused ja ressursside peaaegu igast seadmest kasutajatega. Me majutada oma rakenduste pilves, tähendab, et saaksime eest riistvara ja kasutajale nõuete täitmiseks mastaapimist. Kõik, mida teha, on üles rakendused, mille soovite ühiskasutusse anda ja siis nende rakenduste kasutajatele. [Kasutajad saavad hoida oma seadmetes](remoteapp-clients.md), kui haldate kõik Azure portaali kaudu. Isegi teil oma ettevõtte mandaadi kasutamise võimalus teavitava rakenduste ja andmete turvalisuse tagamiseks.

Lisateavet Azure RemoteApp, lugege edasi või, kui me juba veendunud saate [proovida kohe](https://azure.microsoft.com/services/remoteapp/).

On Azure RemoteApp küsimusi? Vaadake meie [FAQ](remoteapp-faq.md).

[Microsoft Virtual Desktop infrastruktuuri](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx)Azure RemoteApp kuulub.

**Uus!** Kas soovite lisateavet Azure RemoteApp? Või tasandil Azure RemoteApp kinnitamiseks valmis? Liitumine meie nädala [Küsi eksperdid Veebiseminar](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp saidikogumid
On kahte tüüpi [Azure RemoteApp saidikogumid](remoteapp-collections.md).


- **Pilveteenuse saidikogumi** on majutatud ja salvestab andmed programmide pilve. Kasutajad pääsevad rakendused, logige sisse oma Microsofti konto või ettevõtte mandaadi sünkroonitud või Azure Active Directory ühendatud.

    Pilveteenuse saidikogumi kui valige rakendus, mille soovite ühiskasutusse anda ei nõua ühendus mõne oma ettevõtte privaatvõrgu (nt seadme VPN) kaudu. Kui rakendus kasutab ressursid Internetis, OneDrive või Azure, cloud saidikogumi töötab teie jaoks. See on kiireim loomiseks.

- **Hübriidjuurutuse saidikogumi** on majutatud ja salvestab andmed Azure'i pilves, kuid ka võimaldab kasutajatel Accessi andmed ja ressursse, mis on talletatud teie kohalikus võrgus. Kasutajad pääsevad rakendused, logige sisse oma ettevõtte mandaatidega sünkroonitud või Azure Active Directory ühendatud.

    Kui vajate oma ettevõtte privaatvõrgu ressursse ühenduse hübriid saidikogumi valimine Oletagem näiteks, et rakendus peab juurdepääsu ühte järgmistest:

    - Sisevõrgus asuva faili serverid
    - Kiirendamiseks
    - Andmebaaside tulemüüriga

    See on üldiselt rohkem kasu suurettevõtete paljude ressursid oma võrgud, mida ei saa teisaldada pilve.

Erinevate saidikogumid on erinevaid võimalusi, sealhulgas võrkude, et aru saada, [millised saidikogumi](remoteapp-collections.md) sobib kõige paremini. 


### <a name="updating-your-collection"></a>Teie saidikogumi värskendamine
Üks hübriid ja pilveteenuse saidikogumid Peamised erinevused on, kuidas käsitletakse tarkvaravärskendusi. Koos cloud kogum, mis kasutab eelinstallitud Office 365 ProPlus või Office 2013 pilti, ei pea muretsema värskendusi. Teenuse säilitab ise ja koondab välja värskenduste jooksvalt nii rakenduste ja operatsioonisüsteem.

Hübriidjuurutuse saidikogumid, kui ka pilveteenuses saidikogumid, mis kasutavad kohandatud malli pilti, olete eest pilt ja rakendused. Domeeni ühendatud pilte, saate määrata tööriistadega nagu Windows Update, rühmapoliitika või System Center värskendused.

Pärast värskendamist pilt kohandatud malli, saate uue pildi üleslaadimiseks Azure pilveteenuse ja seejärel värskendage saidikogumi kasutamiseks uus pilt. (Selleks Azure RemoteApp **Kiirkäivituse** lehelt või armatuurlaua.)

Lisateabe saamiseks vaadake [oma saidikogumi värskendamine](remoteapp-update.md) .

## <a name="supported-remoteapp-clients"></a>Toetatud RemoteApp klientrakendused
Azure RemoteApp on toetatud RemoteApp kliendi rakendused Windows ja Windows RT kui ka Microsoft kaugtöölaua apps for Mac, iOS ja Android. Teie kasutajad saavad kasutada nende rakenduste oma mobiiltelefoni või arvutada seadmetest juurde pääseda uue Azure RemoteAppi programmid.

Klientide kohta lisateabe saamiseks vaadake [oma minirakenduste Azure RemoteApp juurdepääs](remoteapp-clients.md) .

## <a name="next-steps"></a>Järgmised sammud
Avage! Proovige järele! Järgmised artiklid aitavad teil alustada Azure'i RemoteAppi:

- [Millist liiki saidikogumi jaoks Azure RemoteApp vajate?](remoteapp-collections.md)
- [Luua Azure RemoteApp pilt](remoteapp-imageoptions.md)
- [Kuidas luua pilve saidikogumi Azure RemoteApp](remoteapp-create-cloud-deployment.md)
- [Kuidas luua hübriid saidikogumi Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Hulgilitsentsimise tööpõhimõtted Azure RemoteApp?](remoteapp-licensing.md)
- [Azure RemoteApp kasutamise head tavad](remoteapp-bestpractices.md)
- [Azure RemoteApp KKK](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Aidake meil aidata teil 
Kas teadsite, et lisaks selles artiklis hindamine ja muutes kommentaaride alla all, saate muuta ise artiklit? Midagi puudu? Midagi on valesti? Kas midagi, mis on lihtsalt selgem kirjutada? Liikuge kerides üles ja klõpsake nuppu **github redigeerida** või **muuta** muudatusi teha - need tulevad meile läbivaatamiseks ja siis, kui me logige välja need, näete oma muudatused ja täiustused siinsamas.