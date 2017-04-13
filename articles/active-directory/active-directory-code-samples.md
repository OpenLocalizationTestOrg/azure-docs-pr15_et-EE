<properties
   pageTitle="Azure Active Directory koodinäiteid | Microsoft Azure'i"
   description="Registri Azure Active Directory koodinäiteid, stsenaariumi järgi."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Azure Active Directory koodinäiteid

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure AD) abil saate lisada autentimise ja luba veebirakenduste ja web API-d. Selles jaotises saate lingid koodinäiteid, mis näitab teile, kuidas seda teha ja Koodilõigud, mille abil saate oma rakendused. Proovi kood lehel leiate üksikasjaliku lugemis-minu teemadele, kus nõuded, installimise ja häälestamise spikker. Ja kood on kommenteerinud, et aidata teil mõista kriitilised jaotised.

Tavaline stsenaarium iga valimi tüübi, vt autentimise stsenaariumid Azure AD.

Kaasa meie näidised github: [Microsoft Azure Active Directory näidiseid ja dokumentatsiooni](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Veebirakenduse veebibrauser

Nende näidet kujutavad veebirakenduse, mis suunab kasutaja brauseris sisse Azure AD logida kirjutamist.



| Keele/platvormi | Näidis | Kirjeldus
| ----------------- | ------ | -----------
| C# / .NET-I | [Web Appis-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Autentida kasutajat on Azure AD rentniku OpenID ühendamine (ASP.net-i OpenID ühenduse OWIN vahetarkvara) abil.
| C# / .NET-I | [Web Appis MultiTenant-OpenIdConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Mitme rentniku .NET MVC kasutava veebirakenduse OpenID ühendamine (ASP.net-i OpenID ühenduse OWIN vahetarkvara) kasutajate autentimiseks mitme Azure AD rentniku kaudu.
| C# / .NET-I | [Web Appis-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Kasutamiseks on Azure AD rentniku kasutajate autentimiseks oli-Federation (ASP.net-i oli-Federation OWIN vahetarkvara).






## <a name="single-page-application-spa"></a>Ühe lehe rakenduse (SPA)

See näide näitab, kuidas kirjutada ühelt leheküljelt taotluse turvatud Azure AD.  

| Keele/platvormi | Näidis | Kirjeldus
| ----------------- | ------ | -----------
| JavaScripti, C# / .net-i | [SinglePageApp-DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | ADAL JavaScript ja Azure AD abil secure AngularJS vastavalt ühele lehele rakenduse ASP.net-i veebi-API tagasi end rakendada.


## <a name="native-application-to-web-api"></a>Algne rakendus veebi-API

Nende koodinäiteid näitab, kuidas luua omakliendi rakendusi, mis nõuavad web API-d, mis on kaitstud Azure AD. Nad kasutavad [Azure AD autentimiseks teeki (ADAL)](active-directory-authentication-libraries.md) ja [Azure AD OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Keele/platvormi | Näidis | Kirjeldus
| ----------------- | ------ | -----------
| JavaScripti | [NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | ADAL lisandmooduli jaoks Apache Cordova abil saate koostada Apache Cordova rakendus, mis helistab veebi-API ja kasutab Azure AD autentimine.
| C# / .NET-I | [NativeClient-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | .NET WPF rakendus, mis nõuab veebi-API, mis on kaitstud, kasutades Azure AD.
| C# / .NET-I | [NativeClient-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Windowsi poe rakendus, mis nõuab veebi-API, mis on kaitstud Azure AD.
| C# / .NET-I | [NativeClient WebAPI-MultiTenant WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Windowsi poe rakendus mitme rentniku web API, mis on turvatud Azure AD helistamiseks.
| C# / .NET-I | [WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Omakliendi rakendus, mis nõuab web API, mis saab sümboolse kasutaja nimel ja seejärel kasutab luba teise veebi-API helistada.
| C# / .NET-I | [NativeClient-WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Windowsi poe rakendus Windows Phone 8.1, mis nõuab veebi-API, mis on tagatud Azure AD.
| ObjC | [NativeClient-iOS](https://github.com/Azure-Samples/active-directory-ios) | IOS-i rakendus, mis nõuab veebi-API, mis nõuab autentimist Azure AD.
| C# / .NET-I | [WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Omakliendi rakendus, mis sisaldab loogika töödelda JWT luba Web API, OWIN vahevara kasutamise asemel.
| C# / Xamarin | [NativeClient-Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Xamarin siduv abil soovitud kohalikke Azure AD autentimiseks teeki (ADAL) Androidi teegi.
| C# / Xamarin | [NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Xamarin siduv abil soovitud kohalikke Azure AD autentimiseks teeki (ADAL) iOS-i.
| C# / Xamarin | [NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Xamarin projekti, et viis platvormid eesmärgid ning helistab veebi-API, mis on tagatud Azure AD.
| C# / .NET-I | [NativeClient Peata DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Kohalikke rakendus, mis teeb-interaktiivne autentimist ja helistab veebi-API, mis on tagatud Azure AD.



## <a name="web-application-to-web-api"></a>Veebirakenduse veebi-API

Nende koodinäiteid, kuidas kasutada Kuva koostamiseks kõne veebirakenduste [OAuth 2.0 Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) web API-d, mis on kaitstud Azure AD.

| Keele/platvormi | Näidis | Kirjeldus
| ----------------- | ------ | -----------
| C# / .NET-I | [Web Appis WebAPI-OpenIDConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Helistage web API sisselogitud kasutaja õigusi.
|  C# / .NET-I | [Web Appis WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Helistage web API rakenduse õigustega.
| C# / .NET-I | [Web Appis WebAPI-OAuth2-UserIdentityle-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Luba [OAuthi](https://msdn.microsoft.com/library/azure/dn645545.aspx) 2.0 Azure AD lisada olemasoleva veebirakenduse nii, et see saate helistada veebi-API.
| JavaScripti | [WebAPI-Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Saate häälestada REST API teenus, mis on integreeritud Azure AD API kaitse. Sisaldab Node.js serveri veebi-API-ga.
| C# / .NET-I | [Web Appis WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Mitme rentniku MVC veebirakendus, mis kasutab mõni Azure AD rentniku kasutajate autentimiseks OpenID ühendamine (ASP.net-i OpenID ühenduse OWIN vahetarkvara). Kasutab autonoomsest Graph API autoriseerimine näeb välja umbes järgmine.

## <a name="server-or-daemon-application-to-web-api"></a>Server või Daemon rakenduse veebi-API

Nende koodinäiteid näitab, kuidas koostada daemon või serveri rakendus, mis toob ressursid veebist API [Azure AD autentimiseks teeki (ADAL)](active-directory-authentication-libraries.md) ja [Azure AD OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)abil.

| Keele/platvormi | Näidis | Kirjeldus
| ----------------- | ------ | -----------
| C# / .NET-I | [Daemon-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Konsooli rakendus kõned veebi-API. Kliendi mandaat on parool.
| C# / .NET-I | [Daemon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Konsooli rakendus, mis nõuab veebi-API. Kliendi mandaat on sert.


## <a name="calling-azure-ad-graph-api"></a>Azure'i AD Graph API helistamine

Need proovi kood näitab, kuidas luua rakendusi, mis on Azure AD Graph API directory andmete lugemine ja kirjutamine.

| Keele/platvormi | Näidis | Kirjeldus
| ----------------- | ------ | -----------
| Java | [Web Appis-GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Graph API juurdepääsu Azure AD directory andmeid kasutava veebirakenduse.
| PHP | [Web Appis-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Graph API juurdepääsu Azure AD directory andmeid kasutava veebirakenduse.
| C# / .NET-I | [Web Appis-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Graph API juurdepääsu Azure AD directory andmeid kasutava veebirakenduse.
| C# / .NET-I | [ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | See rakendus konsooli näitab levinud lugemine ja kirjutamine kõned Graph API ning näitab, kuidas käivitada kasutajale litsentsi määramine ja kasutaja pisipiltide foto ja linkide värskendamiseks.
| C# / .NET-I | [ConsoleApp GraphAPI-DiffQuery DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Konsooli rakendus, mis kasutab erinevat päringu Graph API saada perioodiliste muudab Azure AD rentniku kasutaja objektid.
| C# / .NET-I | [Web Appis GraphAPI-DirectoryExtensions DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Rakenduse MVC kasutab Graph API päringute lihtsa ettevõtte organisatsiooniskeemi loomiseks.
| PHP | [Web Appis-GraphAPI DirectoryExtensions PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | PHP rakendus, mis nõuab API graafik laiend registreerida ja seejärel lugeda, värskendada ja kustutada laiend atribuut väärtused.


## <a name="authorization"></a>Luba

Nende koodinäiteid näitab, kuidas kasutada Azure AD autoriseerimine.

| Keele/platvormi | Näidis | Kirjeldus
| ----------------- | ------ | -----------
| C# / .NET-I | [Web Appis-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Rollipõhise juurdepääsu reguleerimine (RBAC) rakenduses, mis on integreeritud Azure AD Azure Active Directory rühma taotluste abil teha.
| C# / .NET-I | [Web Appis-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Rollipõhise juurdepääsu reguleerimine (RBAC) rakenduses, mis on integreeritud Azure AD Azure Active Directory rakenduse rollide abil teha.


## <a name="legacy-walkthroughs"></a>Pärand juhendavad tutvustused

Neid juhendavad tutvustused kasutada veidi vanem tehnoloogia, kuid ikka võiks huvi.

| Keele/platvormi | Näidis | Kirjeldus
| ----------------- | ------ | -----------
| C# / .NET-I | [Rollipõhine ja ACL-põhine autoriseerimine rakenduses Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) | Tehke rakenduses, mis on integreeritud Azure AD Rollipõhine autoriseerimine (RBAC) ja ACL-põhine loa.
| C# / .NET-I |  [AAL - Windowsi poe rakenduse ülejäänud teenusega - autentimine](http://go.microsoft.com/fwlink/?LinkId=330605) |  [Azure'i AD autentimiseks teeki (ADAL)](active-directory-authentication-libraries.md) (varem AAL) Windowsi poe beeta abil saate lisada Windowsi poe rakenduse kasutaja autentimise võimalusi.
| C# / .NET-I | [ADAL - rakenduste ülejäänud teenusega - autentimine AAD brauseri dialoogiboksi kaudu](http://go.microsoft.com/fwlink/?LinkId=259814) |  [Azure'i AD autentimiseks teeki (ADAL)](active-directory-authentication-libraries.md) abil saate lisada WPF kliendi kasutaja autentimise võimalusi.
| C# / .NET-I | [ADAL - rakenduste ülejäänud teenusega - autentimine ACS brauseri dialoogiboksi kaudu](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  [Azure'i AD autentimiseks teeki (ADAL)](active-directory-authentication-libraries.md) ja [Juurdepääsu juhtimine teenuse 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) abil saate lisada WPF kliendi kasutaja autentimise võimalusi.
| C# / .NET-I | [ADAL - Server Server autentimine](http://go.microsoft.com/fwlink/?LinkId=259816) | Teenuse kõned serveri pool protsess MVC4 Web API ülejäänud teenuse secure [Azure AD autentimise Raamatukogu (ADAL)](active-directory-authentication-libraries.md) abil.
| C# / .NET-I | [Sisselogimine oma veebirakenduse abil Azure AD lisamine](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Konfigureerige .net-i rakenduse web ühekordse sisselogimise suhtes Azure AD ettevõtte kataloogi teha.
| C# / .NET-I | [Mitme rentniku veebirakenduste Azure AD arendamine](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Azure AD abil saate lisada ühekordse sisselogimise ja directory access võimalusi ühe .net-i taotluse üle mitme ettevõtted töötada.
JAVA | [Java valimi rakenduse Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) | Graafik API abil Accessi andmed: Azure'i AD.
PHP | [Azure'i AD Graph API PHP valimi rakendus](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Graph API abil Accessi andmed: Azure'i AD.
| C# / .NET-I | [Azure'i AD Graph API rakenduse näidis](http://go.microsoft.com/fwlink/?LinkID=262648) | Graafik API abil Accessi andmed: Azure'i AD.
| C# / .NET-I | [Valimi rakenduse Azure AD graafiku erinevat päring](http://go.microsoft.com/fwlink/?LinkId=275398) | Vahe päringu abil Graph API saada Azure AD rentniku perioodiliste muudatuste kasutaja objektid.
| C# / .NET-I | [Valimi rakenduse integreerimise mitme rentniku pilve Azure AD taotlus](http://go.microsoft.com/fwlink/?LinkId=275397) | Azure AD rentniku mitme rakenduse integreerida.
| C# / .NET-I | [Windowsi poe rakendus ja REST veebiteenuse abil Azure AD turvamine](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Luua lihtsa veebi-API ressursi ja kasutades Azure AD kliendi Windowsi poe rakendus ja [Azure AD autentimise Raamatukogu (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET-I| [Päringu Azure AD graafik API abil](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Konfigureerige Microsoft .net-i rakenduse kasutamiseks Azure AD Graph API pääsevad andmetele juurde Azure AD rentniku kataloogist.

## <a name="see-also"></a>Vt ka

##### <a name="other-resources"></a>Muud ressursid

[Azure Active Directory arendaja juhend](active-directory-developers-guide.md)

[Azure'i AD Graphi kontseptuaalne API- ja viitamisfunktsioonid](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure'i AD Graph API Helper teek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

