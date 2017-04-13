<properties
    pageTitle="Azure'i API halduse teenuse eksemplari juurutamise mitme Azure'i piirkonna"
    description="Saate teada, kuidas mitme Azure'i piirkonna Azure'i API halduse teenuse eksemplari juurutamine." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Azure'i API halduse teenuse eksemplari juurutamise mitme Azure'i piirkonna

API halduse toetab mitme piirkonna juurutus, mis võimaldab API tootjad üles mõni muu arv soovitud Azure piirkondade üle ühe API halduse teenuse. See aitab vähendada taotluse latentsus tajuvad geograafiliselt jaotatud API tarbijad ja parandab teenuse kättesaadavus ka siis, kui ühe piirkonna läheb ühenduseta. 

Loomisel API halduse teenuse algselt sisaldab ainult ühe [üksuse][] ja ühe Azure piirkond, mis on määratud esmane piirkond asub. Azure'i klassikaline portaali kaudu saate hõlpsasti lisada täiendavad piirkonnad. API Management gateway server on juurutatud iga piirkonna ja liikluse kõne suunatakse lähima lüüsi. Kui piirkonnas läheb ühenduseta, on automaatselt ümber suunata järgmise lähima lüüsi liiklust. 

> [AZURE.IMPORTANT] Mitme piirkonna juurutamise kättesaadav ainult **[Premium][]** astme.

## <a name="add-region"> </a>Juurutada API halduse teenuse eksemplari uue piirkond

Alustamiseks klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis. See viib teid Publisheri API haldusportaali.

![Publisheri portaal][api-management-management-console]

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

Liikuge **skaala** sakki Azure'i klassikaline portaalis oma API halduse eksemplari. 

![Skaala menüü][api-management-scale-service]

Võtta kasutusele uue piirkond, klõpsake ripploendis all esmane piirkond ja valige loendist piirkonnas.

![Piirkonna lisamine][api-management-add-region]

Kui piirkonnas on valitud, valige ühikute üksuse rippmenüü loendist uus regiooni jaoks.

![Üksuste määramine][api-management-select-units]

Kui soovitud piirkondade ja üksused on konfigureeritud, klõpsake nuppu **Salvesta**.

## <a name="remove-region"> </a>API halduse teenuse eksemplari kustutada piirkonnas

Liikuge piirkonnas API halduse teenuse eksemplari eemaldamiseks **skaala** sakki Azure'i klassikaline portaalis oma API halduse eksemplari. 

![Skaala menüü][api-management-scale-service]

Klõpsake nuppu **X** soovitud regioonis õigus eemaldada.  

![Piirkonna eemaldamine][api-management-remove-region]

Kui soovitud piirkonnad on eemaldatud, klõpsake nuppu **Salvesta**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance
[Azure'i API kasutamist alustada]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[ühiku]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

