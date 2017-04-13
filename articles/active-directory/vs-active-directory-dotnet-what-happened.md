<properties
    pageTitle="Mis juhtus minu MVC projekt (Visual Studio Azure Active Directory ühendatud teenuse) | Microsoft Azure'i "
    description="Kirjeldab, mis toimub MVC projekti ühenduse loomisel Azure AD Visual Studio ühendatud teenuste abil"
    services="active-directory"
    documentationCenter="na"
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

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Mis juhtus minu MVC projekt (Visual Studio Azure Active Directory ühendatud teenuse)?

> [AZURE.SELECTOR]
> - [Alustamine](vs-active-directory-dotnet-getting-started.md)
> - [Mis juhtus](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Viited on lisatud

### <a name="nuget-package-references"></a>Nugeti pakett viited

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **OWIN**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.Net-i viited

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **OWIN**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Koodi lisamist

### <a name="code-files-were-added-to-your-project"></a>Projekti on lisatud kood failid

Autentimise käivitus ainekursuse, **App_Start/Startup.Auth.cs** lisati projekti sisaldav käivitus loogika Azure AD autentimiseks. Selle domeenikontrolleri ainekursuse, Controllers/AccountController.cs lisati ka, mis sisaldab **SignIn()** ja **SignOut()** . Lõpuks osaline vaade **Views/Shared/_LoginPartial.cshtml** on lisatud, mis sisaldab linki toimingu jaoks SignIn/SignOut.

### <a name="startup-code-was-added-to-your-project"></a>Projekti lisati käivitus kood

Kui projekti oli juba käivitus ainekursuse, värskendatud kaasata **ConfigureAuth(app)**kõne **konfiguratsiooni** meetod. Muul juhul lisati käivitus klassi oma projekti.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Teie app.config või web.config on uute väärtuste määramine

Järgmised konfiguratsiooni kirjed on lisatud.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Azure Active Directory (AD) rakendus on loodud
Azure'i AD-Rakenduse loodi valitud viisardi kataloogis.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Kui ma kontrollida *keelata üksikute kasutajakontode autentimine*, milliseid täiendavaid muudatusi tehtud oma projekti?
Nugeti pakett viited eemaldatud ja failid on eemaldatud ja varundada. Sõltuvalt projekti, peate käsitsi eemaldada täiendavad viited või failide või muutke vastavalt vajadusele koodi.

### <a name="nuget-package-references-removed-for-those-present"></a>Nugeti pakett viited eemaldada (need esitamiseks)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Koodi failide varundatud ja eemaldada (need esitamiseks)

Kõik järgmised failid on varundatud ja eemaldada. Projekti directory keskmes 'Varundamise' kaustas asuvad varukoopia.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Koodi failide varundatud (nende kohal) jaoks

Iga järgmised failid oli varundada enne asendatud. Projekti directory keskmes 'Varundamise' kaustas asuvad varukoopia.

- **Startup.cs**
- **App_Start\Startup.Auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>*Andmete lugemine directory*märkimisel tehtud mis täiendavaid muudatusi oma projekti?

Täiendavad viited on lisatud.

###<a name="additional-nuget-package-references"></a>Täiendavad Nugeti pakett viited

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Täiendavad .net-i viited

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Lisafailid kood on lisatud projekti

Kaks failid on lisatud tugiteenuste Turbeloa vahemällu: **Models\ADALTokenCache.cs** ja **Models\ApplicationDbContext.cs**.  Vaade ja täiendavad kontrolleril lisati illustreerimiseks juurdepääsul kasutajaprofiili teabe Azure Graphi API-de kasutamine.  Need failid on **Controllers\UserProfileController.cs** ja **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Projekti on lisatud täiendavaid käivitus kood

Failis **startup.auth.cs** lisati uue **OpenIdConnectAuthenticationNotifications** objekti **OpenIdConnectAuthenticationOptions** **teatised** liige.  See on lubada OAuthi koodi ja juurdepääsu sümboolse vahetada.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Teie app.config või web.config tehtud täiendavaid muudatusi

Järgmised täiendavad konfiguratsiooni kirjed on lisatud.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

On lisatud konfiguratsiooni järgmistes jaotistes ja ühendusstring.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Azure Active Directory rakenduse värskendatud
Azure Active Directory rakenduse värskendatud kaasata *andmete lugemine directory* õiguste ja mõne täiendavad võti on loodud, siis kasutatud *ida: ClientSecret* rakenduses **fail** nimega.

[Lisateavet Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
