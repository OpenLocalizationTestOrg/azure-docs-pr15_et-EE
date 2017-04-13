<properties
    pageTitle="Kohandatud API kõne loogika rakendustes"
    description="Oma kohandatud API majutatud loogika rakendustega rakenduse teenuse abil"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Oma kohandatud API majutatud loogika rakendustega rakenduse teenuse abil

Kuigi loogika rakendused on suurel hulgal erinevaid 40 + ühendused erinevaid teenuseid, mida soovite sissehelistamine oma kohandatud API, mida saab käitada oma kood. Üks on kõige lihtsam ja kõige scalable võimalused majutada oma kohandatud veebi-API's on rakenduse teenust kasutada. Selles artiklis antakse ülevaade sissehelistamine mis tahes veebi-API, mis on majutatud rakenduse teenuse API rakenduse, web appi või mobiilirakenduses.

Koostamise päästik või toimingu loogika rakendustes API-de kohta lisateabe saamiseks vaadake [artiklit](app-service-logic-create-api-app.md).

## <a name="deploy-your-web-app"></a>Kasutada oma Web App

Esmalt peate oma API juurutamine Web App rakenduse teenus. Selles teemas kirjeldatud juhised kataks juurutamist põhilised: [ASP.net-i web rakenduse loomine](../app-service-web/web-sites-dotnet-get-started.md).  Ajal saate mis tahes API sissehelistamine loogika rakenduse kaudu, soovitame teil parima kasutuskogemuse lisada ärplema metaandmete hõlpsasti integreerida loogika rakenduste toimingud.  Üksikasjad leiate [ärplema lisamise](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)kohta.

### <a name="api-settings"></a>API-sätted

Et loogika rakenduste kujundaja oma ärplema sõeluda, on oluline, et lubada CORS ning määrata oma veebirakenduse APIDefinition atribuudid.  See on väga lihtne seadmine Azure'i portaal.  Lihtsalt avage oma veebirakenduse sätete tera ja määrake jaotises API "API mõiste" URL-i swagger.json faili (see on tavaliselt https://{name}.azurewebsites.net/swagger/docs/v1) ja lisage CORS poliitika "*" lubamiseks taotlusi loogika rakenduste Designer.

## <a name="calling-into-the-api"></a>Kahtluse API

Kui loogika rakenduste portaalis sees, kui seate CORS ja API määratlus atribuudid teil peaks oskama hõlpsalt lisada kohandatud API toiminguid teie meilivoo sees.  Kujundajas saate sirvida oma tellimuse veebilehed määratletud ärplema URL-i veebisaitide loendisse.  Saate kasutada ka HTTP + ärplema toimingu Osutage soovitud ärplema ja loendis Saadaolevad toimingud ja sisendeid.  Lisaks saate luua alati HTTP toimingu abil helistamine, ka need, mis ei ole või jätke ärplema dokumendi mis tahes API taotluse.

Kui soovite oma API turvaline, siis on paar võimalust seda teha.

1. Koodi muutmata vaja - Azure Active Directory saab kasutada mis tahes koodi muudatusi või ümberkorraldamiseks nõudmata oma API kaitsmiseks.
1. Jõusta Basic Auth, AAD Auth või serdi Auth oma API kood.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Turvamise kõned oma API ilma koodi muutmine

Selles jaotises saate luua kaks Azure Active Directory rakendused – ühes oma loogika rakenduse ja veebirakenduse jaoks.  Kuvatakse autentida kõned oma veebirakenduse teenuse põhisumma (kliendi id ja salajane) ja sellega seotud AAD oma loogika rakenduse abil. Lõpetuseks tuleb kaasata rakenduse ID definitsiooni loogika rakendus.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Osa 1: Häälestamise oma loogika rakenduse identiteedi rakendus

See on loogika rakendus kasutab active directory autentimiseks. Saate ainult *peate* tegema üks kord kataloogi. Näiteks saate kasutada sama identiteeti loogika rakenduste, kuigi võite luua kordumatu identiteedid loogika rakenduse kohta soovi. Saate seda teha ka UI või PowerShelli kasutamine.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>Azure'i klassikaline portaalis rakenduste ID loomine

1. Liikuge [klassikaline portaalis Azure Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) ja valige kataloogi, mida kasutate veebirakenduse jaoks
2. Klõpsake vahekaarti **rakendused**
3. Klõpsake lehe allservas asuvat käsuriba **lisamine**
4. Pange oma identiteedi kasutamiseks klõpsake noolenuppu järgmine nimi
5. Kordumatu stringi vormindatud domeeni kaks tekstiväljadele panna, ja klõpsake nuppu märke
6. Klõpsake vahekaarti **konfigureerimine** selle rakenduse jaoks
7. Kopeerige **Kliendi ID**, kasutatakse loogika rakenduse
8. **Klahvid** jaotises nuppu **Valige kestus** ja valida, kas 1 aasta või 2 aastat
9. (Võib-olla peate ootama mõne hetke) ekraani allservas nuppu **Salvesta**
10. Nüüd kindlasti väljale Kopeeri võti. Seda kasutatakse ka loogika rakenduse

#### <a name="create-the-application-identity-using-powershell"></a>PowerShelli kasutamine rakenduste ID loomine
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Veenduge, et kopeerida **Rentniku ID**, **Rakenduse ID** ja parooliga, mida kasutasite

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Osa 2: Kaitsta oma identiteedi AAD rakenduse Web App

Kui teie Web app on juba juurutanud saate lihtsalt lubada portaali. Muul juhul saate teha lubamine autoriseerimine osa juurutamise Azure'i ressursi haldur.

#### <a name="enable-authorization-in-the-azure-portal"></a>Luba Azure'i portaalis lubamine

1. Liikuge Web app ja klõpsake nuppu **sätted** käsuriba.
2. Klõpsake nuppu **Luba/autentimist**.
3. Lülitage **sisse**.

Selles etapis rakendus on teie jaoks automaatselt loodud. Peate selle rakenduse kliendi ID osa 3, seega peate:

1. Minge [klassikaline portaalis Azure Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) ja valige kataloogi.
2. Rakenduse otsinguväljale otsimine
3. Klõpsake seda loendis
4. Klõpsake vahekaarti **konfigureerimine**
5. Peaksite nägema **Kliendi ID**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>Juurutamine oma veebirakenduse on ARM malli abil

Esmalt peate looma oma veebirakenduse taotlus. See peaks olema muu rakendus, mida kasutatakse teie loogika rakenduse kaudu. Alustada pärast ülaltoodud juhiseid 1 osa, kuid nüüd **kodulehele** ja **IdentifierUris** kasutamise oma veebirakenduse tegelik https://**URL-i** .

>[AZURE.NOTE]Veebirakenduse jaoks rakenduse loomisel tuleb kasutada [Azure klassikaline portaali lähenemine](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory)nagu PowerShell abil pole häälestanud veebisaidi kasutajate sisselogimiseks vajalikke õigusi.

Kui olete kliendi ID ja rentniku, kaasake juurutamise malli sub ressursi veebirakenduse järgmist:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Automaatselt, mis kasutab tühja Web app ja loogika rakenduse koos AAD kasutavate juurutamine käivitamiseks nuppu:

[![Azure'i juurutamine](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Täieliku Mall, vaadake [loogika rakenduse kõne kohandatud API majutatud rakenduse teenuse ja AAD kaitstud](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Osa 3: Asustada loogika rakenduse jaotises luba

**Luba** jaotis **HTTP** toimingu.`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Elemendi | Kirjeldus |
|---------|-------------|
| tüüp | Autentimise tüüp. ActiveDirectoryOAuth autentimise väärtus ActiveDirectoryOAuth. |
| rentniku | Rentniku ID, mis tähistavad AD rentniku. |
| sihtrühma | Nõutav. Loote ressursi. |
| clientID | Azure AD Rakenduse kliendi ID. |
| salajane | Nõutav. Salajane kliendi, mis taotleb luba. |

Ülaltoodud mall on juba selle häälestada, kuid kui loote loogika rakenduse otse, peate täielik luba jaotise lisada.

## <a name="securing-your-api-in-code"></a>Oma API-koodi turvamine

### <a name="certificate-auth"></a>Serdi auth

Saate kliendi sertide valideerimiseks sissetulevad taotlused veebirakendusse. Lugege teemat [Kuidas soovite konfigureerida TLS vastastikune autentimine Web Appi](../app-service-web/app-service-web-configure-tls-mutual-auth.md) kohta, kuidas häälestada oma kood.

Klõpsake jaotises *Luba* peaks andma: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Elemendi | Kirjeldus |
|---------|-------------|
| tüüp | Nõutav. Autentimise tüüp. SSL-sertide klient, peab olema ClientCertificate. |
| pfx | Nõutav. Base64-kodeeringuga PFX-faili sisu. |
| parooli | Nõutav. Parooli PFX-fail. |

### <a name="basic-auth"></a>Tavaline auth

Saate kinnitada sissetulevad taotlused elementaarautentimine (nt kasutajanimi ja parool). Tavaline auth on levinud mustri ja saate seda teha mis tahes keeleks, saate luua rakenduse sisse.

Peate esitama jaotises *Luba* : `{"type": "basic","username": "test","password": "test"}`.

| Elemendi | Kirjeldus |
|---------|-------------|
| tüüp | Nõutav. Autentimise tüüp. Väärtus peab olema elementaarautentimine, Basic. |
| kasutajanimi | Nõutav. Kasutajanimi autentida. |
| parooli | Nõutav. Parooli kinnitamiseks. |

### <a name="handle-aad-auth-in-code"></a>Täitepide AAD auth kood

Vaikimisi Azure Active Directory autentimine, et lubada portaali ei ole kohandatud autoriseerimine. Näiteks seda lukustada, oma API teatud kasutaja või rakenduse, kuid ainult teatud rentniku jaoks.

Kui soovite piirata vaid loogika rakendus, näiteks API-koodi, saate ekstraktida päis, mis sisaldab JWT ja vaadake, kes helistaja on, lükake need tagasi taotlused, mis ei vasta.

Edasi, kui soovite rakendada selle täielikult oma kood ja mitte kasutada funktsiooni portaali, lugege artiklit: [Kasutamine Active Directory autentimise teenuses Azure rakendus](../app-service-web/web-sites-authentication-authorization.md).

Peate endiselt Järgige ülaltoodud juhiseid oma loogika rakenduse identiteedi rakenduse loomine ja mille abil saate helistada API.
