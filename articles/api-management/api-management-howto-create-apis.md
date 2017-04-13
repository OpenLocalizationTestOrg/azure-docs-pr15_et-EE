<properties 
    pageTitle="Kuidas luua API-de Azure'i API haldus" 
    description="Saate teada, kuidas loomine ja konfigureerimine API-de Azure'i API haldus." 
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

# <a name="how-to-create-apis-in-azure-api-management"></a>Kuidas luua API-de Azure'i API haldus

API Management API tähistab toimingute, mida saab kutsuda klientrakendustes. Publisheri portaalis on loodud uue API-de ja seejärel soovitud toimingud on lisatud. Kui need toimingud on lisatud, API lisatakse mõne toote ja saab avaldada. Kui API on avaldatud, saate selle tellinud ja arendajate kasutada.

Sellest juhendist kuvab kõigepealt protsessi: loomine ja konfigureerimine uue API API haldus. Toimingute lisamine ja toote avaldamise kohta leiate lisateavet teemast [Kuidas toimingute API lisada][] ja [Kuidas luua ja avaldada toote][].

## <a name="create-new-api"> </a>Luua uue API

API-d on loodud ja konfigureeritud Publisheri portaalis. Publisheri portaali, klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis.

![Publisheri portaal][api-management-management-console]

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

Klõpsake **API-de** vasakul menüüst **API haldus** ja seejärel klõpsake nuppu **Lisa API**.

![API loomine][api-management-create-api]

**Lisa uus API** akna abil saate konfigureerida uue API.

![Lisage uus API][api-management-add-new-api]

Uue API konfigureerimiseks kasutatakse järgmised väljad.

-   **Veebi-API nimi** pakub API kordumatu ja kirjeldav nimi. Kuvatakse see arendaja ka Publisheri portaalide.
-   **Veebiteenuse URL** viitab HTTP teenuse rakendamise API. API halduse edastab selle aadressi taotluste.
-   **Web API URL-i järelliite** on lisatud API halduse teenuse base URL. Alusega URL on tavaline kõigi API-de korraldaja API halduse teenuse eksemplar. API juhtimine eristab API-de oma järelliite, ja seetõttu järelliide peab olema kordumatu iga API antud Publisheri.
-   **Web API URL-i skeem** määratleb, milliseid protokolle saab kasutada API juurdepääsu. Vaikimisi on määratud HTTPs.
-   Toote võite selle uue API lisamiseks klõpsake ripploendis **tooted (valikuline)** ja valige toote. Seda toimingut saate korrata mitu korda API lisamiseks tooted.

Kui soovitud väärtused on konfigureeritud, klõpsake nuppu **Salvesta**. Kui uus API on loodud, kuvatakse API kokkuvõtte leht Publisheri portaalis.

![API Kokkuvõte][api-management-api-summary]

## <a name="configure-api-settings"> </a>Konfigureerimine API-sätted

Saate kontrollida ja redigeerida konfiguratsiooni API vahekaarti **sätted** . **Veebi-API nimi**, **veebiteenuse URL**ja **Web API URL-i järelliite** algselt määratakse kui API on loodud ja muuta siin. **Kirjeldus** pakub soovi korral ka kirjeldus ja **Web API URL-i skeem** määratleb, milliseid protokolle saab kasutada API juurdepääsu.

![API-sätted][api-management-api-settings]

Lüüsi autentimise rakendamise API taustväärtus teenuse konfigureerimine, valige vahekaart **Turve** . **Mandaadiga** rippmenüüst saab kasutada **HTTP tavaline** või **kliendi sertide** autentimise konfigureerimine. HTTP lihttekstautentimine kasutamiseks sisestage soovitud mandaat. Kliendi serdi autentimist kasutamise kohta leiate teavet teemast [secure tagaandmebaas teenuste kasutamine kliendi serdi autentimist Azure'i API haldamine][].

Konfigureerimiseks **Kasutaja autoriseerimine** OAuthi 2.0 abil saab kasutada ka vahekaarti **Turve** . Lisateabe saamiseks vaadake, [Kuidas lubada arendaja kontod OAuthi 2.0 Azure'i API Management abil][].

![Lihttekstautentimine sätted][api-management-api-settings-credentials]

Klõpsake nuppu **Salvesta** muudatused API sätete salvestamiseks.

## <a name="next-steps"> </a>Järgmised sammud

Kui API on loodud ja konfigureeritud sätete järgmised toimingud on lisamistoimingute API, API lisada mõne toote ja selle avaldada, nii et see oleks saadaval arendajatele. Lisateavet leiate järgmistest artiklitest.

-   [Kuidas lisada toimingute API][]
-   [Kuidas luua ja avaldada toote][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Kuidas lisada toimingute API]: api-management-howto-add-operations.md
[Kuidas luua ja avaldada toote]: api-management-howto-add-products.md

[Azure'i API kasutamist alustada]: api-management-get-started.md
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance
[Kuidas tagada tagaandmebaas services kliendi serdi autentimist Azure'i API haldamine]: api-management-howto-mutual-certificates.md
[Kuidas lubada arendaja kontod OAuth 2.0 kasutamine Azure'i API haldus]: api-management-howto-oauth2.md