<properties
    pageTitle="Sisu sünkroonimine pilveteenuse kaustast Azure'i rakendust Service"
    description="Saate teada, kuidas Juurutage rakendus Azure'i rakendust Service kaudu sisu sünkroonimine pilveteenuse kaustast."
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
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Sisu sünkroonimine pilveteenuse kaustast Azure'i rakendust Service

Selle õpetuse näitab, kuidas võtta kasutusele [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714) , nagu Dropbox ja OneDrive populaarsed pilveteenused oma sisu sünkroonimine. 

## <a name="overview"></a>Sisu sünkroonimine juurutamise ülevaade

Juurutamise nõudmisel sisu sünkroonimine on tootja [Kudu juurutamise engine](https://github.com/projectkudu/kudu/wiki) integreeritud rakenduse teenus. [Azure portaali](https://portal.azure.com), saate määrata oma salvestusruumi kausta, rakenduse koodi ja selle kausta sisu töötamine ja rakenduse teenusega sünkroonimiseks klõpsake nupu. Sisu sünkroonimine kasutab koostamine ja juurutamise Kudu protsess. 
    
## <a name="contentsync"></a>Sisu sünkroonimine juurutamise lubamine
Sisu sünkroonimine [Azure portaali](https://portal.azure.com)lubamiseks tehke järgmist.

1. Oma rakenduse tera Azure'i portaalis, klõpsake nuppu **sätted** > **Juurutamise allikas**. Klõpsake **Andmeallika valimine**ja valige **OneDrive'i** või **Dropboxi** allikana juurutamiseks. 

    ![Sisu sünkroonimine](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Kuna aluseks olevat API-d, **OneDrive for Business** ei toeta praegu. 

2. Täitke Luba töövoos rakenduse võimaldavad juurdepääsu teatud eelmääratletud määratud tee OneDrive'i või Dropboxi, kus kogu rakenduse teenuse sisu talletatakse.  
    Pärast autoriseerimine rakenduse teenuse platvormi teile soovitud suvandit jaotises määratud sisu tee sisu kausta loomiseks või valida olemasoleva kausta sisu jaotises selle määratud sisu tee. Määratud sisu teed jaotises pilveteenuse salvestusruumi kontod kasutatakse rakendust Service sünkroonimine on järgmised.  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropboxi**:`Dropbox\Apps\Azure`

3. Pärast algse sisu sünkroonimise saate sisu sünkroonimine algatatud nõudmisel Azure portaalist. Juurutamise ajalugu on saadaval **juurutuste** tera.

    ![Juurutamise ajalugu](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Lisateavet Dropboxi juurutamiseks on saadaval jaotises [Deploy Dropboxi kaudu](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


