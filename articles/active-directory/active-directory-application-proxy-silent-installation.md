<properties
    pageTitle="Kuidas vaikselt installida Azure AD Rakenduse puhverserveri konnektor | Microsoft Azure'i"
    description="Hõlmab Vaikne install Azure AD Rakenduse puhverserveri Connectori turvalist remote juurdepääsu oma kohapealse rakendusi."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Kuidas vaikselt installida Azure AD Rakenduse puhverserveri konnektor

Mida soovite saata mõne installi skripti mitme Windows serveriga või Windowsi serverid, mis pole lubatud kasutajaliides. See teema selgitab, kuidas luua Windows PowerShelli skripti, mis võimaldab installida ja registreerida oma Azure AD Rakenduse puhverserveri konnektor järelevalveta installi.

## <a name="enabling-access"></a>Juurdepääsu lubamine
Rakenduse puhverserveri töötab, installides õhuke Windows Server teenust nimega konnektor võrgu sees. Töötada rakenduse puhverserveri konnektor on Azure AD kataloogi üldadministraator ja parooli abil registreerida. Tavaliselt on see sisestatud Connectori installimisel klõpsake dialoogiboksis häälestamine pop. Teise võimalusena Windows PowerShelli abil saate luua mandaadi objekti sisestamiseks teie registreerimisteave või saate luua oma luba ja selle abil saate sisestada oma andmed.

## <a name="step-1--install-the-connector-without-registration"></a>Samm 1: Installida ilma registreerimise konnektor


Installige konnektor MSIs registreerimata konnektor järgmiselt:


1. Avage käsuviip.
2. Käivitage järgmine käsk, mille q tähendab vaiksesse installi - installimine ei palub teil nõustuge litsentsilepinguga lõppkasutaja.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Samm 2: Registreerida Azure Active Directory konnektor
Seda saab teha, kasutades ühte järgmistest:


- Windows PowerShelli mandaadi objekti abil konnektor registreerimine
- Registreerida abil loodud ühenduseta märgiks konnektor

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Windows PowerShelli mandaadi objekti abil konnektor registreerimine


1. Loomine Windows PowerShelli mandaadi objekti käitades järgmine, kus "<username>"ja"<password>" asendatakse kasutajanime ja parooli kataloogi:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Avage **C:\Program Files\Microsoft AAD rakenduse puhverserveri konnektor** ja käivitage PowerShelli mandaadi objekti lõite, kus $cred on loodud PowerShelli identimisteabe objektile nime abil skript:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Registreerida abil loodud ühenduseta märgiks konnektor

1. Mõne ühenduseta sõnet abil AuthenticationContext klassi väärtuste kasutamine koodilõigu loomiseks tehke järgmist.


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Kui teil on SecureString luba abil luua luba. <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Käivitage järgmine käsk Windows PowerShelli, kus SecureToken on loodud kohal luba nimi ja tenantID on teie rentnikukontoga GUID: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Vt ka

- [Azure Active Directory puhverserverit lubamine](active-directory-application-proxy-enable.md)
- [Avaldada oma domeeninime kasutavad rakendused](active-directory-application-proxy-custom-domains.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)
- [Teil on rakenduse puhverserveri probleemide tõrkeotsing](active-directory-application-proxy-troubleshoot.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)
