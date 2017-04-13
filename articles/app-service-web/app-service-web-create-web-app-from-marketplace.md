<properties
    pageTitle="Luua veebirakenduse Azure'i turuplatsilt | Microsoft Azure'i"
    description="Saate teada, kuidas luua uus WordPress veebirakenduse Azure'i turuplatsilt Azure portaali kaudu."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Luua veebirakenduse Azure'i turuplatsilt

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure'i turuplatsi tehakse kättesaadavaks mitmesuguseid populaarsed veebirakenduste töötanud Microsoft, kolmanda osapoole ettevõtetele ja avatud allika tarkvara algatused. WordPress, Umbraco CMS Drupal, näiteks jne. Nende veebirakenduste on ehitatud populaarsed raamistiku, nagu see WordPress näide, [.NET], [Node.js], [Java]ja [Python], nimi mõne [PHP] mitmeid. Loomiseks vajate vaid tarkvara Azure'i turuplatsilt web app on brauseris kasutatav [Azure portaali].

Selles õppetükis saate teada, kuidas:

* Otsige üles ja Azure rakenduse teenus, mis on Azure turuplatsiga mallil põhineva veebirakenduse loomine.
* Azure'i rakendust Service uue veebirakenduse sätete konfigureerimine.
* Käivitada ja hallata oma veebirakenduse.

Selleks, et selles õpetuses juurutamist WordPress ajaveebisaidi Azure'i turuplatsilt. Kui olete lõpetanud juhiseid selles õpetuses, peate oma WordPress saidi üles ja töötab pilveteenuses.

![WordPress wep rakenduse näidisarmatuurlaual][WordPressDashboard1]

WordPress saidile, mis juurutamist selles õpetuses kasutab MySQL-i andmebaasist. Kui soovite selle asemel kasutada andmebaasi SQL-andmebaasiga, lugege [Projekti Nami], mis on saadaval kaudu Azure'i turuplatsi.

> [AZURE.NOTE]
> Selle õpetuse tegemiseks on vaja Microsoft Azure'i kontosse. Kui teil pole kontot, saate [aktiveerida oma Visual Studio abonendi kasu] [ activate] või [tasuta prooviversiooni kasutajaks][free trial].
>
> Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus]. Seal saate kohe luua lühiajaline starter web app rakenduse teenuse – krediitkaarti on vaja ja samuti ei ole.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Otsige üles ja luua veebirakenduse Azure rakenduse teenus

1. [Azure'i portaali]sisse logida.

1. Klõpsake nuppu **Uus**.
    
    ![Looge uus Azure ressurss][MarketplaceStart]
    
1. **WordPress**otsida, ja klõpsake **WordPress**. (Kui soovite kasutada SQL-andmebaasi asemel MySQL-i, otsige **Projekti Nami**.)

    ![WordPress kuvatakse Marketplace'ist otsimine][MarketplaceSearch]
    
1. Pärast lugemist WordPress rakenduse kirjelduse, klõpsake nuppu **Loo**.

    ![WordPress veebirakenduse loomine][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Uue veebirakenduse jaoks Azure'i rakenduse teenuse sätete konfigureerimine

1. Kui olete loonud uue veebirakenduse, tera WordPress sätted kuvatakse, mille abil tehke järgmist:

    ![WordPress web appi sätete konfigureerimine][ConfigStart]

1. Sisestage väljale **veebirakenduse** veebirakenduse nimi.

    See nimi peab olema kordumatu azurewebsites.net domeen, sest web appi URL on *{nimi}*. azurewebsites.net. Kui teie sisestatud nimi pole kordumatud, kuvatakse punane hüüumärk tekstiväljale.

    ![WordPress web appi nime konfigureerimine][ConfigAppName]

1. Kui teil on mitu tellimust, valige see, mida soovite kasutada. 

    ![Tellimuse web appi konfigureerimine][ConfigSubscription]

1. Valige **Ressursirühm** või looge uus.

    Ressursi rühmade kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade][ResourceGroups].

    ![Ressursirühm web appi konfigureerimine][ConfigResourceGroup]

1. Valige soovitud **Rakenduse leping/asukoht** või looge uus.

    Rakenduse teenuse lepingute kohta leiate lisateavet teemast [Azure rakenduse teenuse lepingute ülevaade][AzureAppServicePlans]. 

    ![Teenusepakett web appi konfigureerimine][ConfigServicePlan]

1. Klõpsake **andmebaasi**ja siis **MySQL-andmebaasiga** tera sisestage nõutav väärtused konfigureerida MySQL-andmebaasiga.

    lisamine. Sisestage uus nimi või jätta vaikenime.

    b. Jätke **Andmebaasi tüüp** väärtuseks **ühiskasutuses**.

    c. Valige veebirakenduse jaoks valitud üks samasse asukohta.

    d. Valige hinnakirjad taseme. **Elavhõbeda** - mis on tasuta minimaalsete ühendused ja kettaruumi – sobib selles õpetuses.

    e. Juriidiline nõustuma tera **MySQL-andmebaasiga** , ja seejärel klõpsake nuppu **OK**. 

    ![Andmebaasi veebirakenduse sätete konfigureerimine][ConfigDatabase]

1. **WordPress** labale juriidiline nõustumine ja seejärel klõpsake nuppu **Loo**. 

    ![Lõpetage web app sätted ja klõpsake nuppu OK][ConfigFinished]

    Azure'i rakenduse teenus loob veebirakenduse, tavaliselt vähem kui minutiga. Saate vaadata, klõpsates portaali lehe ülaosas kellaikooni edenemist.

    ![Edenemisnäidik][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Käivitada ja hallata oma WordPress web app
    
1. Web appi loomine lõpetades Azure'i portaalis, mille lõite rakenduse ressursirühma liikumine ja näete web app ja andmebaas.

    Täiendav ressurss lihtversioon pirnid ikoon on [Rakenduse ülevaated][ApplicationInsights], mis osutab jälgimisega seotud veebirakenduse jaoks.

1. Klõpsake **ressursirühm** labale web appi joon.

    ![Valige oma WordPress web app][WordPressSelect]

1. Web Appi tera, klõpsake nuppu **Sirvi**.

    ![Otsige sirvides üles oma WordPress web app][WordPressBrowse]

1. Kui teil palutakse ajaveebi WordPress keel, valige soovitud keel ja seejärel klõpsake nuppu **Jätka**.

    ![Konfigureerige keele WordPress veebirakenduse jaoks][WordPressLanguage]

1. WordPress **Tere tulemast** lehel, sisestage nõutud WordPress konfiguratsiooni teave ja klõpsake **Installida WordPress**.

    ![Teie WordPress web appi sätete konfigureerimine][WordPressConfigure]

1. Logige sisse lehel **Tere tulemast** loodud mandaadi abil.  

1. Oma saidi armatuurlaualehe avada ja kuvada teavet, mis teil.    

    ![WordPress armatuurlaua kuvamine][WordPressDashboard2]

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses olete näha, kuidas luua ja juurutada näide web app Azure'i turuplatsilt.

Rakenduse teenuse Web Apps töötamise kohta leiate lisateavet teemast linke (lai brauseri Windowsi jaoks) lehe vasakus servas või ülaosas, lehe (kitsas brauseri Windowsi jaoks).

WordPress veebirakenduste Azure arendamise kohta leiate lisateavet teemast [Azure rakenduse teenuse arendamise WordPress][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET-I]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Proovige rakenduse teenus]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure'i portaal]: https://portal.azure.com/
[Projekti Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
