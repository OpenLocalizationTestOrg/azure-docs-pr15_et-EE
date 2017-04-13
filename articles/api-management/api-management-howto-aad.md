<properties 
    pageTitle="Kuidas lubada arendaja kontod Azure Active Directory kasutamine Azure'i API haldus" 
    description="Saate teada, kuidas lubada kasutajate API halduse Azure Active Directory abil." 
    services="api-management" 
    documentationCenter="API Management" 
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

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Kuidas lubada arendaja kontod Azure Active Directory kasutamine Azure'i API haldus


## <a name="overview"></a>Ülevaade
Sellest juhendist näitab, kuidas ühe või mitme Azure Active kataloogide kõigi kasutajate arendaja portaali juurdepääsu lubamine. Sellest juhendist ka näitab, kuidas hallata Azure Active Directory kasutajarühmade välise rühmad, mis sisaldavad Azure Active Directory kasutajate lisamise teel.

>Sellest juhendist juhiseid peab olema Azure Active Directory, kus soovite luua rakenduse.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Kuidas lubada Azure Active Directory kontot arendaja

Alustamiseks klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis. See viib teid Publisheri API haldusportaali.

![Publisheri portaal][api-management-management-console]

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

**API halduse** vasakul menüüs **Turvalisus** ja seejärel klõpsake **Välise identiteedid**.

![Välise identiteedid][api-management-security-external-identities]

Klõpsake **Azure Active Directory**. Kirjutage **Ümber suunata URL** ja vahetamise Azure Active Directory Azure'i klassikaline portaalis.

![Välise identiteedid][api-management-security-aad-new]

Klõpsake nuppu **Lisa** uus Azure Active Directory rakenduse loomiseks ja valige **oma ettevõttes areneb rakenduse lisamine**.

![Uue Azure Active Directory rakenduse lisamine][api-management-new-aad-application-menu]

Sisestage rakenduse nimi, valige **veebirakenduse ja/või veebi-API**ja klõpsake nuppu edasi.

![Azure Active Directory uus rakendus][api-management-new-aad-application-1]

**Sisselogimise URL-i**, sisestage oma arendaja portaali sisselogimise URL. Selles näites on **sisselogimise URL-i** `https://aad03.portal.current.int-azure-api.net/signin`. 

Sisestage Azure Active Directory jaoks vaikedomeeni või kohandatud domeeni **URL-i rakenduse ID**ja kordumatu stringi lisada. Selles näites kasutatakse **https://contoso5api.onmicrosoft.com** vaikedomeeni **/api** määratud järelliide.

![Uue Azure Active Directory teenuserakenduse atribuudid][api-management-new-aad-application-2]

Klõpsake nuppu kontrolli salvestamiseks ja uue rakenduse loomine ja **konfigureerimine** menüüd uue rakenduse konfigureerimiseks.

![Azure Active Directory uus rakendus, mis on loodud][api-management-new-aad-app-created]

Kui mitme Azure Active kataloogide on selle rakenduse jaoks kasutada, klõpsake nuppu **Jah** **rakendus on mitme rentniku**jaoks. Vaikimisi on **ei**.

![Rakendus on mitme rentniku][api-management-aad-app-multi-tenant]

Publisheri portaalis vahekaarti **Välise identiteedid** **Azure Active Directory** osas **Ümber suunata URL-i** kopeerida ja kleepida tekstiväljale **Vastus URL-i** . 

![Vastus URL-i][api-management-aad-reply-url]

Kerige konfigureerimine menüü, valige rippmenüüst **Teenuserakenduse õiguste** ja ruut **Loe directory andmed**.

![Teenuserakenduse õiguste][api-management-aad-app-permissions]

Valige **Volitatud esindaja õigused** ripploendist ja märkige ruut **Luba sisselogimise ja lugeda kasutajate profiilid**.

![Delegeeritud õigused][api-management-aad-delegated-permissions]

>Rakendus ja delegeeritud õiguste kohta leiate lisateavet teemast [juurdepääs Graph API][].

**Kliendi Id** kopeerimine lõikelauale.

![Kliendi Id][api-management-aad-app-client-id]

Aktiveerige uuesti Publisheri portaali ja kleepige kopeeritud Azure Active Directory rakenduse konfigureerimise **Kliendi Id** .

![Kliendi Id][api-management-client-id]

Aktiveerige Azure Active Directory konfiguratsiooni, ja klõpsake soovitud **kestus valige** rippmenüüs **klahvid** jaotises ja määrake intervalli. Selles näites kasutatakse **1 aasta** .

![Klahv][api-management-aad-key-before-save]

Klõpsake nuppu **Salvesta** salvestada konfiguratsiooni ja kuvamiseks klahvi. Võti kopeerimine lõikelauale.

>Märkige üles see võti. Kui Azure Active Directory konfiguratsiooni akna sulgemiseks klahvi ei saa kuvada uuesti.

![Klahv][api-management-aad-key-after-save]

Aktiveerige Publisheri portaali ja kleepige võti **Kliendi salajane** tekstivälja.

![Kliendi salajane][api-management-client-secret]

**Lubatud rentnikud** saate määrata, millised kataloogid on juurdepääs API-de API halduse teenuse eksemplari. Määrake Azure Active Directory eksemplarid, millele soovite anda juurdepääsu domeenid. Mitu domeeni eraldamiseks reavahetusi, tühikute või komadega eraldatud.

![Lubatud rentnikega][api-management-client-allowed-tenants]

Jaotises **Lubatud rentnikud** saab määrata mitu domeeni. Enne, kui mõni kasutaja ei saa sisse logida domeenil kui algse domeeni, kus on registreeritud rakenduse, muu domeeni üldadministraator loa rakenduse access directory andmetega. Õiguste andmiseks üldadministraator logige sisse rakendusse ja klõpsake nuppu **Aktsepteeri**. Järgmises näites `miaoaad.onmicrosoft.com` **Rentnikud lubatud** ja globaalse on lisatud domeeni administraator on esimest korda sisselogimine.

![Õigused][api-management-permissions-form]

>Kui-globaalne administraator proovib sisse logida, enne kui globaalne administraator on andnud õigused, sisselogimine ebaõnnestub ja viga ekraanil kuvatakse.

Kui soovitud konfiguratsioon on määratud, klõpsake nuppu **Salvesta**.

![Salvestamine][api-management-client-allowed-tenants-save]

Pärast muudatuste salvestamist, saate kasutajad määratud Azure Active Directory arendaja portaali logida [Azure Active Directory kontot kasutades arendaja portaali sisse logida][]juhiseid järgides.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Kuidas lisada ka välise Azure Active Directory rühma

Pärast Azure Active Directory kasutajate juurdepääsu lubamine, saate lisada API Management hõlpsam hallata jaotises arendajad seos soovitud tooted Azure Active Directory rühmad.

> Välise Azure Active Directory rühmale konfigureerimiseks Azure Active Directory tuleb esmalt konfigureerida klõpsake vahekaarti identiteedid eelmise jaotise juhiseid järgides. 

Välise Azure Active Directory rühmad lisatakse **nähtavuse** menüü toote, mille soovite anda juurdepääsu rühma. Klõpsake **tooted**ja seejärel klõpsake soovitud toote nime.

![Toote konfigureerimine][api-management-configure-product]

**Nähtavuse** menüüd ja käsku **Lisa rühmade Azure Active Directory**.

![Rühmade lisamine][api-management-add-groups]

**Azure Active Directory rentniku** rippmenüü loendist ja seejärel tippige soovitud rühma nimi **rühmad** , et lisada tekstivälja.

![Valige rühm][api-management-select-group]

Selle rühma nime leidub **rühmaloendis** Azure Active Directory jaoks, nagu on näidatud järgmises näites.

![Azure Active Directory Rühmaloendis][api-management-aad-groups-list]

Klõpsake käsku **Lisa** kinnitage rühma nime ja rühma lisada. Selles näites **Contoso 5 arendajad** on lisatud väline rühma. 

![Rühma lisatud][api-management-aad-group-added]

Klõpsake nuppu **Salvesta** uue rühma valiku salvestamiseks.

Kui üks toode on konfigureeritud ka Azure'i Azure Active Directory rühma, on saadaval muude toodete API halduse teenuse eksemplari menüü **nähtavuse** kontrollima.

Läbi ja välise rühmade atribuudid konfigureerida, kui olete lisanud, klõpsake jaotises **rühmad** menüü nimi.

![Rühmade haldamine][api-management-groups]

Siit saate redigeerida **nimi** ja **Kirjeldus** rühma.

![Rühma redigeerimine][api-management-edit-group]

Konfigureeritud Azure Active Directory kasutajad saavad sisse logida arendaja portaali ja vaade ja tellimine kõik rühmad, mille nad on nähtavuse kohta järgmisest jaotisest juhiste järgi.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Kuidas sisse logida arendaja portaali konto Azure Active Directory abil

Eelmiste jaotiste konfigureeritud Azure Active Directory kontot kasutades arendaja portaali sisse logida avamine uues brauseriaknas **sisselogimise URL-i** kaudu Active Directory rakenduse konfigureerimise abil ja klõpsake **Azure Active Directory**.

![Arendaja portaal][api-management-dev-portal-signin]

Sisestage oma Azure Active Directory mandaat ühe kasutaja ja klõpsake nuppu **Logi sisse**.

![Logi sisse][api-management-aad-signin]

Võidakse teilt koos registreerimisvorm kui on vaja täiendavat teavet. Täitke registreerimisvorm ja klõpsake nuppu **Logi sisse**.

![Registreerimine][api-management-complete-registration]

Nüüd on teie kasutaja sisse loginud arendaja portaali API halduse teenuse eksemplari puhul.

![Registreerimine lõpetatud][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

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
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Juurdepääs API graafik]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Azure Active Directory kontot kasutades arendaja portaali sisse logida]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

