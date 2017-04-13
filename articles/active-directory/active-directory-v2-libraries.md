<properties
   pageTitle="Azure Active Directory v2.0 autentimise teekide | Microsoft Azure'i"
   description="Ühilduvad kliendi teekide ja serveri vahevara teegid ja seotud teegi, allikas ja näidised lingid, Azure Active Directory v2.0 lõpp-punkti."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0 autentimise teegid
Azure Active Directory (Azure AD) v2.0 lõpp-punkti toetab industry-standard OAuth 2.0 ja OpenID ühenduse 1.0 protokolle. Saate mitmesuguste teekide Microsofti ja muude organisatsioonide v2.0 lõpp-punkti.

Kui koostate rakendus kasutab v2.0 lõpp-punkti, soovitame kasutada teegid, mis on kirjutatud protokolli domeeni eksperdid, kes jälgivad turvalisus arengu elutsükli (SDL) meetodid, nagu [see, millele järgneb Microsoft][Microsoft-SDL]. Kui otsustate kood tugi protokolle, soovitame järgige SDL meetodite ja pöörama tähelepanu turvalisuse alused standardid spetsifikatsioonide iga Protocol (protokoll).

## <a name="types-of-libraries"></a>Teekide tüübid

Azure'i AD v2.0 töötab kaks teegitüüpi:

- **Kliendi teegid**. Kohalikke klientide ja serverite kliendi teekide abil saate Accessi sõned helistamine ressursi, nt Microsoft Graphi.
- **Serveri vahevara teegid**. Veebirakenduste kasutamine serveri vahevara teekide jaoks kasutaja sisselogimine. Veebi-API serveri vahevara teekide abil domeenist saadetud kohalikke kliendid või muudes serverites sõned.

## <a name="library-support"></a>Teegi tugi
Kuna v2.0 lõpp-punkti kasutamisel saate valida mis tahes standardses teegis, on oluline teada, kust tugi. Probleemid ja soovitada funktsioone teegi koodi, võtke ühendust teegi omanik. Probleemid ja teenuse-side protokolli rakendamisel soovitada funktsioone, võtke ühendust Microsofti.

Teekide tulevad kaks tugi Kategooriad:

- **Microsofti toetatud**. Microsoft sisaldab parandusi nende teekide ja teinud SDL nõuetele vastavuse kohta need teegid.
- **Ühilduvad**. Microsoft on katsetada nende teekide tavaline stsenaariumid ja kinnitanud töötamine v2.0 lõpp-punkti. Microsoft ei paku nende teekide parandused ja on valmis neid teekide ülevaade. Teegi avatud lähtekoodi projekti teid suunatakse probleemid ja soovitada funktsioone.

Loendi teegid, mis töötavad v2.0 lõpp-punkti, leiate selle artikli jaotistest.

## <a name="microsoft-supported-client-libraries"></a>Microsofti toetatud kliendi teegid
| Platvorm| Teegi nimi| Laadi alla | Lähtekoodi | Näidis |
| :-: | :-: | :-: | :-: | :-: |
| .Net-i, Windowsi poe, Xamarin | Microsoft Authentication Library (MSAL) .net-i jaoks | [Microsoft.Identity.Client (Nugeti)][ClientLib-NET-Lib] | [MSAL .net-i (GitHub)][ClientLib-NET-Repo] | [Windowsi töölaua omakliendi näidis][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory Passport.js lisandmoodul | [Pass-Azure-AD (npm)][ClientLib-Node-Lib] | [Pass-Azure-AD (GitHub)][ClientLib-Node-Repo] | Tulen varsti |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsofti toetatud serveri vahevara teegid
| Platvorm| Teegi nimi| Laadi alla | Lähtekoodi | Näidis |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID ühenduse vahevara ASP.net-i | [Microsoft.Owin.Security.OpenIdConnect (Nugeti)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Web Appi näidis][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | OWIN OAuthi esitaja vahevara ASP.net-i | [Microsoft.Owin.Security.OAuth (Nugeti)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Veebi-API näidis][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET core | OWIN OpenID ühenduse vahetarkvara .NET Core | [Microsoft.AspNetCore.Authentication.OpenIdConnect (Nugeti)][ServerLib-NetCore-Owin-Oidc-Lib] | [ASP.net-i Turve (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Web Appi näidis][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET core | OWIN OAuthi esitaja vahetarkvara .NET Core | [Microsoft.AspNetCore.Authentication.OAuth (Nugeti)][ServerLib-NetCore-Owin-Oauth-Lib] | [ASP.net-i Turve (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Tulen varsti |
| Node.js | Microsoft Azure Active Directory Passport.js lisandmoodul | [Pass-Azure-AD (npm)][ServerLib-Node-Lib] | [Pass-Azure-AD (GitHub)][ServerLib-Node-Repo] | [Web Appi näidis][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Ühilduvad kliendi teegid
| Platvorm| Teegi nimi | Testitud versioon | Lähtekoodi | Näidis |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Rakenduste näidis](active-directory-v2-devquickstarts-android.md) |
| iOS-i | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Rakenduste näidis](active-directory-v2-devquickstarts-ios.md)|
| JavaScripti | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Tulen varsti |
| Python-lisatakse | [Lisatakse-OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Lisatakse-OAuthlib](https://github.com/lepture/flask-oauthlib) | Tulen varsti |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Tulen varsti |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Ühilduvad serveri vahevara teegid
Tulen varsti

## <a name="related-content"></a>Seotud teemad
Azure AD v2.0 lõpp-punkti kohta leiate lisateavet teemast [Azure AD Rakenduse mudeli v2.0 ülevaade][AAD-App-Model-V2-Overview].

Aidake meil viimistlemine ja kujundamine meie sisu, kasutage funktsiooni Disqus kommentaarid käesoleva artikli lõpus tagasiside.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
