<properties
   pageTitle="Azure Active Directory autentimine teekide | Microsoft Azure'i"
   description="Azure AD autentimiseks teeki (ADAL) võimaldab kliendi rakenduste arendajatele kasutajate Cloud hõlpsalt autentimiseks või kohapealse Active Directory (AD) ja seejärel saada juurdepääsu sõned tagamiseks API kõned."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory autentimine teegid

Azure AD autentimine teek (ADAL) võimaldab kliendi rakenduste arendajatele kasutajate Cloud hõlpsalt autentimiseks või kohapealse Active Directory (AD) ja seejärel saada juurdepääsu sõned tagamiseks API kõned. ADAL on palju funktsioone, et muuta autentimine lihtsam arendajatele, nt asünkroonne tugi konfigureeritav Turbeloa vahemälu, mis talletab Accessi sõned ja Värskenda märkide Turbeloa automaatselt värskendada, kui juurdepääsu sümboolse aegub ja Värskenda märgiks on saadaval, ja palju muud. Teostavaid kõige keerukuse, ADAL aitab arendaja keskenduda oma äriloogika ja hõlpsasti secure ressursid ilma eksperdi turvalisus.

ADAL on saadaval mitmesuguseid platvormid.

|Platvorm|Paketi nimi|Klient-Server|Laadi alla|Lähtekoodi|Dokumendid ja näidised|
|---|---|---|---|---|---|
|.Net-i klient, Windowsi poodi, UWP Xamarin iOS-i ja Androidi jaoks|Active Directory autentimise Raamatukogu (ADAL) .NET v3 jaoks |Kliendi|[Microsoft.IdentityModel.Clients.ActiveDirectory (Nugeti)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL .net-i (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Dokumentatsioon](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.Net-i klient, Windowsi poodi, Windows Phone 8.1 |Active Directory autentimise Raamatukogu (ADAL) .NET v2 |Kliendi|[Microsoft.IdentityModel.Clients.ActiveDirectory (Nugeti)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL .net-i (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Dokumentatsioon](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScripti|Active Directory Authentication Library (ADAL) JavaScript|Kliendi|[ADAL JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Näide: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|IOS-i OS X|Active Directory Authentication Library (ADAL) eesmärk-c|Kliendi|[ADAL eesmärk-c (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL eesmärk-c (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Näide: [NativeClient-iOS-i (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Active Directory Authentication Library (ADAL) Androidi jaoks|Kliendi|[ADAL Android (keskses hoidlas)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Näide: [NativeClient-Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Active Directory Authentication Library (ADAL) Node.js jaoks|Kliendi|[ADAL Node.js (npm) jaoks](https://www.npmjs.com/package/adal-node)|[ADAL Node.js (Github) jaoks](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Näide: [WebAPI-Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Microsoft Azure Active Directory pass autentimise vahevara sõlme|Kliendi|[Azure Active Directory pass Node.js (npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory jaoks Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Active Directory Authentication Library (ADAL) Java|Kliendi|[ADAL Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET-I|Identiteedi protokolli laiendid Microsoft .NET Framework 4.5|Server|[Microsoft.IdentityModel.Protocol.Extensions (Nugeti)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure'i AD identiteedi mudel laiendid .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET-I|JSON Web Turbeloa sündmuseohjuri Microsoft .net Framework 4.5|Server|[System.IdentityModel.Tokens.Jwt (Nugeti)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure'i AD identiteedi mudeli laiendid .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET-I|OWIN vahevara, mis võimaldab rakenduse kasutamiseks Microsofti tehnoloogia autentimist.|Server|[Microsoft.Owin.Security.ActiveDirectory (Nugeti)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET-I|OWIN vahevara, mis võimaldab taotluse kasutada OpenIDConnect autentimiseks.|Server|[Microsoft.Owin.Security.OpenIdConnect (Nugeti)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Näide: [Web Appis-OpenIDConnecty-DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET-I|OWIN vahevara, mis võimaldab rakenduse kasutamiseks oli-Federation autentimist.|Server|[Microsoft.Owin.Security.WsFederation (Nugeti)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Näide: [Web Appis-WSFederation-DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Stsenaariumid

Siin on kolm Levinumad stsenaariumid, kus saab kasutada ADAL autentimist.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Kliendi rakendus Remote ressursi kasutajate autentimine

Selle stsenaariumi korral on arendaja klient, nt WPF rakendus, mida vajab turvatud Azure AD, nt veebi-API remote ressursi. Ta on Azure tellimuse, oskab autonoomsest järgneval veebi-API ja teaksid, mis kasutab veebi-API Azure AD rentniku. Selle tulemusena ta kasutada ADAL hõlbustamiseks Azure AD, kas täielikult delegeerivad ADAL-autentimine kogemusi või konkreetselt töötlemise kasutajatunnust autentimine. ADAL muudab hõlpsaks autentida, saada juurdepääsu luba- ja Värskenda luba Azure AD ja seejärel kasutage juurdepääsu luba, et teha see taotleb veebi-API.

Koodi näide, mis näitab selle stsenaariumi Azure AD autentimist kasutades, leiate teemast [Kohalikke WPF klientrakenduse Web API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Server rakenduse Remote ressursi autentimine

Selle stsenaariumi korral arendaja on rakenduse serveris, mida vajab turvatud Azure AD, nt veebi-API remote ressursi. Ta on Azure tellimuse, oskab Kasuta teenust järgneval ja teab Azure AD rentniku veebi-API kasutab. Selle tulemusena ta kasutada ADAL hõlbustamiseks autentimise Azure AD konkreetselt töötlemise rakenduse mandaat. ADAL teeb lihtsaks laadida märgiks Azure AD Rakenduse kliendi mandaadi abil ja veenduge, et luba abil taotleb veebi-API. ADAL töötleb ka haldamise eluiga juurdepääsu luba see vahemällu talletamine ja uuendada seda vastavalt vajadusele. Proovi kood, mis näitab selle stsenaariumi, leiate teemast [Web API konsooli rakendus](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Remote ressursi juurdepääsu kasutaja nimel serverirakenduse autentimine

Selle stsenaariumi korral arendaja on rakenduse serveris, mida vajab turvatud Azure AD, nt veebi-API remote ressursi. Taotluse peab kasutaja nimel Azure AD teha. Ta on Azure tellimuse, oskab autonoomsest järgneval veebi-API ja teab Azure AD rentniku teenus kasutab. Kui kasutaja on autenditud veebirakendusse, võite rakenduse Kasutaja autoriseerimine näeb välja umbes järgmine: Azure'i AD. Veebirakenduse saate ADAL saada juurdepääsu sümboolse ja värskendamiseks luba ja sellega seotud: Azure'i AD autoriseerimine kood ja kliendi mandaadi abil kasutaja nimel. Kui veebirakenduse on juurdepääsu luba, saate selle helistada veebi-API kuni luba on aegunud. Kui luba on aegunud, saate veebirakenduse ADAL saada uue juurdepääsu luba abil Värskenda Turbeloa, mis varem võttis.


## <a name="see-also"></a>Vt ka

[Azure Active Directory arendaja juhend](active-directory-developers-guide.md)

[Azure Active directory autentimine stsenaariumid](active-directory-authentication-scenarios.md)

[Azure Active Directory koodinäiteid](active-directory-code-samples.md)
