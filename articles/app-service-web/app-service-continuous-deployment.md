<properties
    pageTitle="Pidev juurutamise Azure rakenduse teenusega | Microsoft Azure'i"
    description="Saate teada, kuidas lubada Azure'i rakendust Service pidev juurutamine."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Azure'i rakendust Service pidev juurutamine

Selle õpetuse näidatakse, kuidas konfigureerida oma [Azure'i rakendust Service] rakenduse pideva töövoo. Rakenduse teenuste integreerimine BitBucket, GitHub ja Visual Studio meeskonnatöö teenused (VSTS) võimaldab pideva töövoo, kus Azure'i tõmbab uusimad värskendused oma projekti nende teenuste avaldatud. Pideva on hea valik projektide jaoks, kus mitu ja sagedased osakaalu integreeritakse.

## <a name="overview"></a>Pideva lubamine

Pidev juurutuse lubamiseks 

1. Rakenduse sisu avaldada hoidla, mida kasutatakse pidev juurutamiseks.  
    Projekti avaldamine nende teenuste kohta leiate lisateavet teemast [loomine repo (GitHub)], [repo (BitBucket) loomine]ja [VSTS alustamine].

2. Klõpsake oma rakenduse menüü tera [Azure portaali], **RAKENDUSEJUURUTUSE > Juurutussuvandid**. Klõpsake **Andmeallika valimine**ja seejärel valige juurutamise allikas.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Konfigureerimine on VSTS konto rakenduse teenuse juurutamiseks lugege selles [õpetuses](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. Täitke Luba töövoos. 

4. Valige **juurutamise allika** labale haru kaudu juurutada ja projekti. Kui olete lõpetanud, klõpsake nuppu **OK**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Pidev juurutus, nii et GitHub või BitBucket lubamisel kuvatakse avalike ja privaatvõ projektid.

    Rakenduse teenus loob koos valitud hoidla, tõmbab failid määratud kontorist ja säilitab klooni oma hoidla oma rakenduse teenuse rakenduse. Kui konfigureerite VSTS pidev juurutamise Azure'i portaalis, integreerimine kasutab rakenduse teenuse [Kudu juurutamise engine](https://github.com/projectkudu/kudu/wiki), mis juba automatiseerib koostamine ja juurutamise ülesannete iga `git push`. Pole vaja eraldi häälestamine pidev juurutamise VSTS. Pärast selle protsessi lõpule jõudnud, kuvatakse **Juurutussuvandid** rakenduse tera aktiivne juurutus, mis näitab juurutamise õnnestus.

5. Veenduge, et rakendus on edukalt juurutatud, klõpsake **URL-i** rakenduse blade Azure portaali ülaosas. 

6. Veenduge, et pideva esineb hoidla teie valitud, vajutage muudatuste hoidla. Rakenduse värskendama muudatustele vahetult pärast hoidla PTT on lõpule jõudnud. Saate kontrollida, et see on tõmmatakse **Juurutussuvandid** tera rakenduse värskendus.

## <a name="VSsolution"></a>Pidev kasutuselevõtu lahenduse Visual Studio 

Visual Studio lahendus Azure'i rakenduse teenusega on sama lihtne kui lükkamine lihtsa index.html faili. Rakenduse teenuse juurutamise protsessi sujuvamaks kõik üksikasjad, sh Nugeti sõltuvuste taastamine ja koostamise rakenduse kahendfaile. Järgige andmeallika juhtelemendi head tavad säilitada kood ainult teie Git hoidla ja lasta rakenduse teenuse juurutamise ülejäänud eest.

Juhised lükkamine oma Visual Studio lahendus rakenduse teenusega on sama, nagu [eelmises jaotises](#overview), tingimusel, et saate konfigureerida oma lahenduse ja hoidla järgmiselt.

-   Visual Studio andmeallika juhtelemendi suvandi abil saate luua mõne `.gitignore` faili nagu pildil allpool või käsitsi lisamine on `.gitignore` faili sisu sarnaneb selle [.gitignore valimi](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore)hoidla root. 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   Kogu lahenduse kataloogi puu lisada oma andmebaasi hoidla root .sln failiga.

Kui olete oma andmebaasi kirjeldatud häälestamine ja konfigureeritud rakenduse Azure pidev avaldamise ühest online Git hoidlate, saate töötada ASP.net-i rakenduse kohalikult rakenduses Visual Studio ja pidevalt juurutada oma kood lihtsalt vajutades muudatuste teie võrgus Git hoidla.

## <a name="disableCD"></a>Pideva keelamine

Pidev juurutuse keelamine 

1. Klõpsake oma rakenduse menüü tera [Azure portaali], **RAKENDUSEJUURUTUSE > Juurutussuvandid**. Klõpsake nuppu **Katkesta ühendus** **Juurutussuvandid** tera.

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. Pärast kinnituse sõnumile vastates **Jah** , saate oma rakenduse blade naasmiseks ja klõpsake nuppu **RAKENDUSEJUURUTUSE > Juurutussuvandid** kui soovite häälestada avaldamise mõnest muust allikast.

## <a name="additional-resources"></a>Lisaressursid

* [Kuidas uurida pideva seotud levinud probleemid](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure PowerShelli kasutamine]
* [Kuidas kasutada Azure käsurea tööriistad Mac ja Linux]
* [Git dokumentatsioon]
* [Projekti Kudu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

[Azure'i rakendust Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure'i portaal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure PowerShelli kasutamine]: ../articles/powershell-install-configure.md
[Kuidas kasutada Azure käsurea tööriistad Mac ja Linux]: ../articles/xplat-cli-install.md
[Git dokumentatsioon]: http://git-scm.com/documentation

[Repo (GitHub) loomine]: https://help.github.com/articles/create-a-repo
[Repo (BitBucket) loomine]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[VSTS alustamine]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
