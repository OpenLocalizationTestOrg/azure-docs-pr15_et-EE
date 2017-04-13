<properties 
    pageTitle="Kuidas lubada arendaja kontod OAuth 2.0 kasutamine Azure'i API haldus" 
    description="Saate teada, kuidas lubada kasutajate OAuth 2.0 kasutamine API haldus." 
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

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Kuidas lubada arendaja kontod OAuth 2.0 kasutamine Azure'i API haldus

Mitme API-de toetavad [OAuth 2.0](http://oauth.net/2/) secure API ja veenduge, et ainult kehtiv kasutajatel on juurdepääs ja nad pääsevad ainult ressursid on õigus. Kasutamiseks koos sellise API-de Azure API Management interaktiivne arendaja konsooli teenuse võimaldab teil oma eksemplari töötamiseks oma OAuthi 2.0 lubatud API konfigureerimine.

## <a name="prerequisites"> </a>Eeltingimused

Sellest juhendist näidatakse, kuidas konfigureerida oma API halduse eksemplari kasutama OAuth 2.0 autoriseerimine arendaja kontode jaoks, kuid mitte näitab teile, kuidas konfigureerida OAuth 2.0 osutaja. Iga OAuth 2.0 teenusepakkuja konfiguratsioon erineb, kuigi sarnanevad juhiseid ja teavet kasutatakse konfigureerimine OAuth 2.0 API halduse eksemplari nõutav tekstilõigule on samad. Selles teemas kirjeldatakse kasutades Azure Active Directory OAuth 2.0 osutaja näited.

>[AZURE.NOTE] Azure Active Directory abil OAuth 2.0 konfigureerimise kohta leiate lisateavet teemast [Web Appis-GraphAPI-DotNet][] valimi.

## <a name="step1"> </a>OAuth 2.0 loa serveris API halduse konfigureerimine

Alustamiseks klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis. See viib teid Publisheri API haldusportaali.

![Publisheri portaal][api-management-management-console]

>[AZURE.NOTE] Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

Klõpsake vasakul menüüst **API halduse** **Turvalisus** , klõpsake **OAuth 2.0**ja klõpsake **lisada autoriseerimine server**.

![OAuth 2.0][api-management-oauth2]

Pärast nupu **Lisa autoriseerimine server**, kuvatakse uue autoriseerimine server vormi.

![Uus server][api-management-oauth2-server-1]

Sisestage **nimi** ja **Kirjeldus** väljadele nimi ja soovi korral ka kirjeldus. 

>[AZURE.NOTE] Neid välju kasutatakse OAuth 2.0 autoriseerimine server sees API halduse praeguse eksemplari tuvastamiseks ja nende väärtuste ei tule OAuth 2.0 serverist.

Sisestage **kliendi registreerimise lehe URL-i**. Selle lehe on, kus kasutajad saate luua ja hallata oma kontode ja erineb sõltuvalt kasutatud OAuth 2.0 pakkuja. **Kliendi registreerimise lehe URL-i** suunab lehele, millega kasutajad saavad luua ja OAuth 2.0 kasutajahalduse kontod toetavad teenusepakkujad oma kontode konfigureerimine. Mõni ettevõte konfigureerida või isegi siis, kui OAuth 2.0 pakkuja toetab seda funktsiooni kasutada. Kui OAuth 2.0 pakkuja ei ole konfigureeritud kontode kasutajahalduse, sisestage kohatäite URL siin näiteks oma ettevõtte URL või URL-i, näiteks `https://placeholder.contoso.com`.

Järgmises jaotises vormi sisaldab **koodi loa andmine tüübid**, **autoriseerimine lõpp-punkti URL-i**ja **Luba meetodit** sätteid.

![Uus server][api-management-oauth2-server-2]

Määrake **kood anda tüübid** , märkides ruudu soovitud tüüpi. Vaikimisi on määratud **kood** .

Sisestage **URL-i autoriseerimine lõpp-punkti**. Azure Active Directory, see URL on umbes järgmine URL, kus `<client_id>` on asendatud rakenduse OAuth 2.0 server tuvastamaks klienti id-ga.

    https://login.windows.net/<client_id>/oauth2/authorize

**Luba meetodit** saate määrata, kuidas OAuth 2.0 serverisse luba taotluse saata. Vaikimisi on valitud **saada** .

Järgmises jaotises on, kus **on luba lõpp-punkti URL**, **Kliendi autentimismeetodite**, **juurdepääsu luba saatmise meetod**ja **vaikimisi ulatus** on määratud.

![Uus server][api-management-oauth2-server-3]

Azure Active Directory OAuthi 2.0 serveriks **sümboolne lõpp-punkti URL** on järgmises vormingus, kus `<APPID>` vorming on `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

**Kliendi** autentimismeetodite vaikesäte on **lihtne**ja **juurdepääsu luba saatmise meetod** on **autoriseerimine päis**. Selles jaotises, koos **vaikimisi ulatus**on konfigureeritud need väärtused.

**Kliendi identimisteabe** jaotis sisaldab **Kliendi ID** ja **Kliendi salajane**, mis on saadud OAuth 2.0 serveri käigus loomine ja konfigureerimine. Kui **Kliendi ID** ja **Kliendi salajane** on määratud, luuakse **redirect_uri** **kood** . Selle URI kasutatakse teie OAuth 2.0 konfiguratsiooni vastus URL-i konfigureerimine.

![Uus server][api-management-oauth2-server-4]

Kui **kood anda tüübid** on seatud **ressursi omanik parooli**, jaotise **ressursi omanik parooli mandaadi** abil saate määrata mandaadi; muul juhul võite selle tühjaks jätta.

![Uus server][api-management-oauth2-server-5]

Kui vorm on lõpule jõudnud, klõpsake **Salvesta** API halduse OAuth 2.0 autoriseerimine serveri konfiguratsiooni salvestamiseks. Kui serveri konfiguratsioon on salvestatud, saate konfigureerida API-de kasutamine selle konfiguratsiooni, nagu on näidatud järgmises jaotises.

## <a name="step2"> </a>API kasutamine OAuth 2.0 Kasutaja autoriseerimine konfigureerimine

Klõpsake vasakul menüüst **API haldus** **API-d** , klõpsake soovitud API nime, vahekaarti **Turvalisus**ja seejärel märkige ruut **OAuthi**2.0.

![Kasutaja autoriseerimine][api-management-user-authorization]

Valige soovitud **autoriseerimine server** rippmenüü loendist ja klõpsake nuppu **Salvesta**.

![Kasutaja autoriseerimine][api-management-user-authorization-save]

## <a name="step3"> </a>Testida OAuth 2.0 Kasutaja autoriseerimine arendaja portaalis

Kui teil on konfigureeritud serverisse OAuth 2.0 autoriseerimine ja konfigureerinud oma API selles serveris kasutada, saate selle arendaja portaali ja kutsudes API testida.  Klõpsake ülemise parempoolse menüü **arendaja portaalis** .

![Portaali arendaja][api-management-developer-portal-menu]

Klõpsake **API-de** top menüü ja valige **Kaja API -ga**.

![Kaja API][api-management-apis-echo-api]

>[AZURE.NOTE] Kui teil on ainult üks API konfigureeritud või näevad teie konto, siis API-de klõpsamine viib teid otse toimingud, et API.

Valige **saada ressursi** toimingu, klõpsake nuppu **Ava konsool**ja valige ripploendist soovitud **kood** .

![Avatud konsooli][api-management-open-console]

Kui **kood** on valitud, kuvatakse hüpikaknas OAuth 2.0 pakkuja vorm. Selles näites Azure Active Directory pakub vormi Logi sisse.

>[AZURE.NOTE] Kui teil on keelatud hüpikakendega teil palutakse lubada need brauseris. Pärast nende lubamiseks valige **Luba kood** uuesti ja vormi sisselogimist kuvatakse.

![Logi sisse][api-management-oauth2-signin]

Olete sisse loginud, kus täidetakse **taotluse päised** on `Authorization : Bearer` päise, mis lubab kutse.

![Taotluse päise luba][api-management-request-header-token]

Selles etapis saate konfigureerida ülejäänud parameetrite soovitud väärtused ja taotluse esitamine. 

## <a name="next-steps"></a>Järgmised sammud

OAuth 2.0 ja API halduse kasutamise kohta leiate lisateavet teemast järgmisest videost ja kaasas [artikkel](api-management-howto-protect-backend-with-aad.md).

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure'i API kasutamist alustada]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[Web Appis-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

