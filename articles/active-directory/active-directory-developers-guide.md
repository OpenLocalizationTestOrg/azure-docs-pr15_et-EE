<properties
   pageTitle="Azure Active Directory arendaja juhendi | Microsoft Azure'i"
   description="Sellest artiklist leiate Azure'i Active Directory mahuka arendaja rakendusse ressursid."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Azure Active Directory arendaja juhend

## <a name="overview"></a>Ülevaade
Identiteedi haldamise teenuse (IDMaaS) platvormi Azure Active Directory (AD) pakub arendajate tõhusalt integreerida nende rakenduste identiteetide haldus. Järgmised artiklid pakuvad ülevaated rakendamise ja Azure AD põhijooned. Soovitame lugeda neid tellimuse või kui olete valmis, minna [Alustamine](#getting-started) liikuda.


1. [Integreerimine Azure AD eelised](./develop/active-directory-how-to-integrate.md): Miks integreerimine Azure AD pakub turvalist sisselogimine ja luba parim lahendus leida.

1. [Azure AD autentimise stsenaariumid](active-directory-authentication-scenarios.md): lihtsustatud autentimise Azure AD esitada rakendusele sisselogimise eeliseid.

1. [Integreerimine Azure AD rakenduste](active-directory-integrating-applications.md): saate teada, kuidas lisada, värskendada ja eemaldada rakendusi Azure AD ja tootemargi juhised integreeritud rakenduste kohta.

1. [Azure'i AD Graph API](active-directory-graph-api.md): Azure'i AD Graph API abil programmiliselt juurde Azure AD kaudu REST API lõpp-punktid. Azure'i AD Graph API pääseb ka [Microsoft Graphi](https://graph.microsoft.io/). Microsoft Graphi pakub ühendatud API, mis lubab juurdepääsu mitme Microsofti pilveteenuses API-d, kuni ühe REST API lõpp-punkti ja ühe juurdepääsu luba.

1. [Azure AD autentimise teekide](active-directory-authentication-libraries.md): kasutajate saada juurdepääsu sõned .net-i, JavaScripti, Azure AD autentimise teekide abil hõlpsasti autentimiseks eesmärk-C, Androidi ja muud.


## <a name="getting-started"></a>Alustamine

Need õpetused on kohandatud mitme kaudu ja abil saate kiiresti hakata välja Azure Active Directory. Tingimusena, tuleb teil [saada on Azure Active Directory rentnik](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Mobiiltelefoni ja arvuti rakendus kiire alustada juhikud

|[![iOS-i](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET-I](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Windowsi universaalne](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS-i](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET-I](active-directory-devquickstarts-dotnet.md)|[Windowsi universaalne](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Integreerimine OAuth 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Web rakenduse kiire alustada juhikud

|[![.NET-I](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![JavaScripti](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID ühenduse loomine](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET-I](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScripti](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Integreerimine OpenID ühenduse loomine](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Veebi-API kiire alustada juhikud

|[![.NET-I](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET-I](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Päringute directory Kiirjuhend juhend

| [![.NET-I](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph API](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>Õpetused

Nendest artiklitest on kirjeldatud, kuidas täita teatud ülesandeid Azure Active Directory abil:

- [Mõni Azure AD rentniku hankimine](active-directory-howto-tenant.md)
- [Mis tahes Azure AD kasutaja mustri mitme rentniku rakenduse abil sisselogimine](active-directory-devhowto-multi-tenant-overview.md)
- Rist-rakenduse SSO ADAL, kasutades [Androidi](active-directory-sso-android.md) või [iOS](active-directory-sso-ios.md) -seadmete lubamine
- [Veenduge, et teie taotlus AppSource sertifitseeritud Azure AD](active-directory-devhowto-appsource-certified.md)
- [Rakenduse galeriis Azure AD rakenduste loend](active-directory-app-gallery-listing.md)
- [Esitage veebirakenduste Office 365 müüja armatuurlaud](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Azure Active Directory Rakendusmanifest mõistmine](active-directory-application-manifest.md)
- [Sisselogimise ja rakenduse omandamise nuppe klientrakenduse tootemargi juhised mõistmine](active-directory-branding-guidelines.md)
- [Eelvaade: Kuidas luua rakendusi, mida kasutajad nii isiklik ja töökoha või kooli kontoga sisse logida](active-directory-appmodel-v2-overview.md)
- [Eelvaade: Kuidas luua rakendusi, mis registreeruda ja logige sisse tarbijad](../active-directory-b2c/active-directory-b2c-overview.md)
- [Eelvaade: Turbeloa eluajal konfigureerida Azure AD](active-directory-configurable-token-lifetimes.md) PowerShelli kaudu. Vaadake teemat [toimingutest](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) ja [poliitika juriidilise](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) Lisateavet konfigureerimise kaudu Azure AD Graph API.

## <a name="reference"></a>Viide

Nendest artiklitest foundation viite ette ülejäänud ja autentimine teegi API-d, protokollid, tõrked, koodinäiteid ja lõpp-punktid.  

###  <a name="support"></a>Tugi
- [Küsimused sildiga](http://stackoverflow.com/questions/tagged/azure-active-directory): otsimine Azure Active Directory lahendusi virnas ületäitumise, otsides siltide [Azure'i – active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) ja [adal](http://stackoverflow.com/questions/tagged/adal).
- Teemast [Azure AD arendaja sõnastik](active-directory-dev-glossary.md) mõned sagedamini kasutatavad tingimused, mis on seotud rakenduste arendamise ja integreerimise määratlusi.

### <a name="code"></a>Kood

- [Azure Active Directory avatud lähtekoodi teekide](http://github.com/AzureAD): teegi allika leidmiseks on kõige lihtsam viis on meie [teegi](active-directory-authentication-libraries.md)abil.

- [Azure Active Directory näidised](https://github.com/azure-samples?query=active-directory): liikuda loendi näidised on kõige lihtsam viis on [index, koodinäiteid](active-directory-code-samples.md)abil.

- [Active Directory autentimise Raamatukogu (ADAL) jaoks .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - dokumentides on saadaval nii [põhi uusima versiooni](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) ja [varasemate põhiversioon](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Graph API

- [Graph API viide](https://msdn.microsoft.com/library/azure/hh974476.aspx): Azure Active Directory Graph API ülejäänud viide. [Interaktiivse Graph API viide kogemuse vaadata](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Graph API õiguste otsinguulatuste](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): OAuth 2.0 õiguste otsinguulatuste rentniku andmetega kataloogi sisaldava rakenduse juurdepääsu kontrollimiseks kasutatud.

### <a name="authentication-and-authorization-protocols"></a>Autentimise ja luba Protokollid

- [Sisselogimise klahvi edastamiseks Azure AD](active-directory-signing-key-rollover.md): teave Azure AD allkirjastamise võtme edastamiseks sagedus ja värskendada kõige levinum rakenduse stsenaariumid võti.

- [OAuth 2.0 Protocol (protokoll): kasutamise loa koodi anda](active-directory-protocols-oauth-code.md): OAuth 2.0 protokolli autoriseerimine koodi anda, saate lubada juurdepääsu veebirakenduste ja rentniku oma Azure Active Directory Web API-d.

- [OAuth 2.0 Protocol (protokoll): mõistmine peidetud anda](active-directory-dev-understanding-oauth2-implicit-grant.md): Lisateave peidetud loa andmine, ja kas see on õige rakenduse.

- [OAuth 2.0 Protocol (protokoll): teenuse teenus nõuab kliendi mandaadi](active-directory-protocols-oauth-service-to-service.md): The OAuthi 2.0 kliendi identimisteabe lube veebiteenuse (konfidentsiaalse klient) oma mandaadi abil autentimine teise veebiteenuse, helistades asemel teadasaamiseks kasutaja. Selle stsenaariumi korral kliendi on tavaliselt Kesk-astme veebiteenuse, daemon teenuse või veebisaiti.

- [OpenID ühenduse 1.0 Protocol (protokoll): sisselogimine ja autentimine](active-directory-protocols-openid-connect-code.md): OpenID ühenduse 1.0 protokolli laiendab OAuth 2.0 kasutamiseks on autentimine Protocol (protokoll). Kliendi rakendus saate vastu võtta mõne id_token sisselogimine protsessi haldamiseks või tõsta autoriseerimine kood voogu nii id_token ja luba koodi.

- [SAML 2.0 protokolli viide](active-directory-saml-protocol-reference.md): The SAML 2.0 Protocol (protokoll) võimaldab rakendusi kasutajatele ühekordse sisselogimise teenuse pakkumiseks.

- [Oli-Federation 1.2 Protocol (protokoll)](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory toetab oli-Federation 1.2 ühe Web Services Federation versioon 1.2 määratlus. Federation metaandmete dokumendi kohta leiate lisateavet teemast [Federation metaandmete](active-directory-federation-metadata.md).

- [Toetatud luba ja taotluste tüüpi](active-directory-token-and-claims.md): selle juhendi abil saate mõista ja hindate nõuded SAML 2.0 ja JSON Web sõned (JWT) märgid.

## <a name="videos"></a>Videod

### <a name="build"></a>Koostamine

Nende rakenduste arendamise abil Azure Active Directory ülevaade esitlustega funktsioonide kõlareid, kes töötada otse soovitud meeskond. Esitluste teemasid olulise, sh IDMaaS, autentimine, identiteedi ühendamine ja ühekordse sisselogimise.

- [Microsofti Identity: Olek ja tulevaste suund](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Identiteetide haldus teenust tänapäevased rakendused](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Tänapäevane veebirakenduste Azure Active Directory töötada](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Tänapäevane algsete Azure Active Directory rakenduste arendamise](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure'i Reede
[Azure'i Reede](https://azure.microsoft.com/documentation/videos/azure-friday/) on korduv Reede eesmärk on jätkuvalt tuua teile lühike (10 – 15 minutit) 1:1 videosari rohelise ekspertide erinevaid Azure teemasid.  Klõpsake lehel funktsiooni teenuste Filter kõik Azure Active Directory videote vaatamiseks.

- [Azure'i identiteedi 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure'i identiteedi 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure'i identiteedi 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Social

- [Active Directory meeskonna ajaveeb](http://blogs.technet.com/b/ad/): Azure Active Directory maailma uusima muutumist.

- [Azure Active Directory Graphi meeskonna ajaveeb](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory teave, mis on seotud Graph API.

- [Pilveteenuse identiteedi](http://www.cloudidentity.net): mõtteid identiteetide haldus teenust, alates põhisumma Azure Active Directory PM.  

- [Azure Active Directory Twitter](https://twitter.com/azuread): Azure Active Directory teated 140 märki.

## <a name="windows-server-on-premises-development"></a>Windows Server kohapealse arengu
Windows Server ja Active Directory Federation Services (ADFS) kasutamise kohta vaadake teemast:

- [AD FS -i stsenaariumid arendajatele](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): antakse ülevaade AD FS-i komponendid ja kuidas see toimib, toetatud autentimine/autoriseerimine stsenaariumid üksikasjad.
- [AD FS-i juhendavad tutvustused](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): loendi walk-through artikleid, mis pakuvad üksikasjalike juhiste rakendamine seotud autentimine/autoriseerimine puhul.
