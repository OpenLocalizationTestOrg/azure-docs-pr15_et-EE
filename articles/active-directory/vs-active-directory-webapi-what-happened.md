<properties
    pageTitle="Mis juhtus minu WebApi projekt (Visual Studio Azure Active Directory ühendatud teenuse) | Microsoft Azure'i "
    description="Kirjeldab, mis toimub MVC projekti WebApi loote Azure AD Visual Studio abil"
  services="active-directory"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Mis juhtus minu WebApi projekt (Visual Studio Azure Active Directory ühendatud teenus)

> [AZURE.SELECTOR]
> - [Alustamine](vs-active-directory-webapi-getting-started.md)
> - [Mis juhtus](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Viited on lisatud

###<a name="nuget-package-references"></a>Nugeti pakett viited

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>.Net-i viited

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Koodi muudatusi

###<a name="code-files-were-added-to-your-project"></a>Projekti on lisatud kood failid

Autentimise käivitus ainekursuse, **App_Start/Startup.Auth.cs** lisati projekti sisaldav käivitus loogika Azure AD autentimiseks.

###<a name="startup-code-was-added-to-your-project"></a>Projekti lisati käivitus kood

Kui projekti oli juba käivitus ainekursuse, **konfiguratsiooni** meetod värskendatud kaasata kõne `ConfigureAuth(app)`. Muul juhul lisati käivitus klassi oma projekti.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>App.config või web.config faili on uued väärtused.

Järgmised konfiguratsiooni kirjed on lisatud.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Azure'i AD rakendus on loodud

Azure'i AD-Rakenduse loodi valitud viisardi kataloogis.

[Lisateavet Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Kui ma kontrollida *keelata üksikute kasutajakontode autentimine*, milliseid täiendavaid muudatusi tehtud oma projekti?
Nugeti pakett viited eemaldatud ja failid on eemaldatud ja varundada. Sõltuvalt projekti, peate käsitsi eemaldada täiendavad viited või failide või muutke vastavalt vajadusele koodi.

###<a name="nuget-package-references-removed-for-those-present"></a>Nugeti pakett viited eemaldada (need esitamiseks)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Koodi failide varundatud ja eemaldada (need esitamiseks)

Kõik järgmised failid on varundatud ja eemaldada. Projekti directory keskmes 'Varundamise' kaustas asuvad varukoopia.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Koodi failide varundatud (nende kohal) jaoks

Iga järgmised failid oli varundada enne asendatud. Projekti directory keskmes 'Varundamise' kaustas asuvad varukoopia.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>*Andmete lugemine directory*märkimisel tehtud mis täiendavaid muudatusi oma projekti?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Teie app.config või web.config tehtud täiendavaid muudatusi

Järgmised täiendavad konfiguratsiooni kirjed on lisatud.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Azure Active Directory rakenduse värskendatud
Azure Active Directory rakenduse värskendatud kaasata *andmete lugemine directory* õiguste ja mõne täiendavad võti on loodud, mida kasutati seejärel *Ida: parool* on `web.config` faili.

[Lisateavet Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
