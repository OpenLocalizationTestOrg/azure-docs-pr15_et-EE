<properties 
    pageTitle="Juhtimise API võti mõisted" 
    description="Lisateavet API-d, toodete, rollid, rühmad ja muude halduse API võti mõisted." 
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

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Kuidas importida toimingutega Azure'i API Management API määratlus

API haldus, saab luua uue API-d ja käsitsi lisatud toiminguid või API saab importida koos ühe sammu toimingud.

API-d ja nende toimingute saab importida järgmistes vormingutes abil.

-   WADL
-   Ärplema

Sellest juhendist näitab, kuidas luua uus API ja importida oma tegevust ühe sammu. Käsitsi loomine API ja lisamine toimingute kohta leiate teavet teemast [API-de loomiseks][] ja [Kuidas lisada API toimingud][].

## <a name="import-api"> </a>API importimine

API-d on loodud ja konfigureeritud Publisheri portaalis. Publisheri portaali, klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis. Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

![Publisheri portaal][api-management-management-console]

Klõpsake vasakul menüüst **API haldus** **API-d** , ja klõpsake **importimine API**.

![Impordi API][api-management-import-apis]

**Impordi API** aknas on kolm vahekaardid, mis vastavad esitada API määratlus on kolm võimalust.

-   **Lõikelaualt** saate kleepida määratud tekstivälja API määratlus.
-   **Faili** võimaldab teil leidke ja valige fail, mis sisaldab API määratlus.
-   **URL:** võimaldab URL-i spetsifikatsioonide jaoks API esitama.

![Impordi API vorming][api-management-import-api-clipboard]

Pärast API määratlus, kasutage raadionuppude paremal tähistamiseks määratlus vorming. Toetatud failivormingud.

-   WADL
-   Ärplema

Seejärel sisestage **Web API URL-i järelliite**. See on lisatud base URL-i API halduse teenust. Alus URL on tavaline kõigi API-de majutatud iga eksemplari teenuse API haldus. API juhtimine eristab API-de oma järelliite, ja seetõttu järelliide peab olema kordumatu iga API teatud API halduse teenuse eksemplari.

Kui kõik väärtused on sisestanud, klõpsake nuppu **Salvesta** API ja seotud toimingute loomiseks. 

>[AZURE.NOTE] Õpetuse sellest, kuidas importida tavaline kalkulaator API ärplema vormingus, vaadake [haldamine oma esimese API Azure'i API haldamine](api-management-get-started.md).

## <a name="export-api"></a> API eksportimine

Lisaks saate importida uued API-d, saate eksportida määratlused oma API-de Publisheri portaalist. Selleks klõpsake **Ekspordi API** oma **API** **vahekaarti Kokkuvõte** .

![Ekspordi API][api-management-export-api]

API-d saab eksportida WADL või ärplema abil. Valige soovitud vorming, klõpsake nuppu **Salvesta**ja valige asukoht, kuhu soovite faili salvestada.

![API ekspordivorming][api-management-export-api-format]

## <a name="next-steps"> </a>Järgmised sammud

Kui API on loodud ja toiminguid, mis on imporditud, saate vaadata ja konfigureerida täiendavaid sätteid, API lisada toote ja selle avaldada, nii et see oleks saadaval arendajatele. Lisateavet leiate teemast järgmised juhised.

-   [Kuidas API sätete konfigureerimine][]
-   [Kuidas luua ja avaldada toote][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Azure'i API kasutamist alustada]: api-management-get-started.md
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance

[Kuidas lisada toimingute API]: api-management-howto-add-operations.md
[Kuidas luua ja avaldada toote]: api-management-howto-add-products.md
[Kuidas luua API-d]: api-management-howto-create-apis.md
[Kuidas API sätete konfigureerimine]: api-management-howto-create-apis.md#configure-api-settings
