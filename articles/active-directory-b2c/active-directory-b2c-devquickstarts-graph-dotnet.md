<properties
    pageTitle="Azure Active Directory B2C: Kasutada Graph API | Microsoft Azure'i"
    description="Kuidas kõne Graph API B2C rentniku identiteedi rakenduse abil automatiseerida."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure'i AD B2C: Kasutada API graafik

Azure Active Directory (Azure AD) B2C rentnikud tavaliselt väga suurte. See tähendab, et paljusid levinud rentniku toiminguid on vaja teha programmiliselt. Esmane näide on kasutajate haldamine. Peate mõne olemasoleva kasutaja poe migreerimiseks B2C rentniku jaoks. Kui soovite majutada oma lehel kasutajaks registreerumine ja Azure AD taustal kasutajakontode loomine. Järgmist tüüpi ülesanded vajavad võimalust loomine, lugemine, värskendamine ja kasutajate kontod kustutada. Azure'i AD Graph API abil saate teha järgmised toimingud.

B2C rentnikud, on kaks esmane Graph API suhelda.

- Interaktiivne ühekordselt põhiülesandeid, tuleks administraatorikonto B2C rentniku tööülesannete täitmisel tegutseda. Selle režiimi nõuab administraator kirjutama mandaadid enne selle administraator saab teha mis tahes Graph API kõned.
- Automatiseeritud, pidev ülesandeid, kasutage mõnda tüüpi teenusekonto pakkuda haldustoimingute sooritamiseks vajalikke õigusi. Azure AD, saate seda teha rakenduse registreerimisel ja Azure AD autentimine. Seda tehakse, kasutades **Rakenduse ID** , mis kasutab [OAuth 2.0 kliendi mandaadi andmine](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). Sel juhul taotlus toimib endale, mitte kasutajana helistamiseks Graph API.

Selles artiklis selgitame automatiseeritud kasutamine juhul sooritamiseks. Näidata, saame tuleb koostada .NET 4.5 `B2CGraphClient` mis teeb kasutaja loomine, lugemine, värskendamine ja kustutamine (CRUD) toimingud. Kliendi on Windowsi käsurea liides (CLI), mis võimaldab teil kasutada erinevaid viise. Kood on kirjutatud mitteinteraktiivsete, automatiseeritud mood käitumine.

## <a name="get-an-azure-ad-b2c-tenant"></a>Azure'i AD B2C rentnikku hankimine

Enne kui saate luua või või Azure AD üldse suhelda, peate mõne Azure AD B2C rentniku ja üldadministraatori kontot rentniku. Kui teil pole rentniku juba, [Azure AD B2C alustamine](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Otsinguteenuse rakenduse oma rentniku registreerimine

Kui olete B2C rentniku, peate looma oma Teenuserakenduse Azure AD PowerShelli cmdlet-käskude abil.
Esmalt alla ja installige soovitud [Microsoft Online Services sisselogimise Assistant](http://go.microsoft.com/fwlink/?LinkID=286152). Alla laadida ja installida [64-bitine Azure Active Directory moodul Windows PowerShelli jaoks](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Teie B2C rentniku Graph API kasutamiseks peate registreerida spetsiaalset PowerShelli abil. Järgige selle artikli seda teha. Ei saa olemasolevaid B2C rakendusi, mida te registreeritud Azure'i portaalis korduvalt kasutada.

PowerShelli mooduli installimist avage PowerShelli ja ühendus oma B2C rentnikuga. Pärast käivitamist `Get-Credential`, palutakse teil on kasutajanimi ja parool, sisestage kasutajanimi ja B2C rentniku administraatorikonto parooli.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Enne rakenduse loomist peate luua uue **klientrakenduse salajane**.  Rakenduse kasutatakse kliendi salajane Azure AD autentimiseks ja omandada Accessi sõned. Saate luua kehtiv salajane PowerShellis:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

Lõplik käsk tuleks printida oma uue klientrakenduse salajane. Kopeerige see turvalises kohas. Peate hiljem. Nüüd saate luua rakenduse rakenduse nimega mandaati uue klientrakenduse salajane pakkudes:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Kui loote edukalt rakendus, tuleks selle rakenduse, nagu need eespool atribuutide välja printida. Peate nii `ObjectId` ja `AppPrincipalId`, seega liiga kopeerige need väärtused.

Kui olete loonud rakenduse teie B2C rentnikus, peate selle kasutaja CRUD-toiminguid läheb vaja õiguste määramine. Rakenduse kolme rolli määramine: directory lugejatel (vt kasutajad), directory poolt (saate luua ja värskendada kasutajad), ja kasutajale konto administraatori (kustutatava kasutajad). Järgmised rollid on tuntud identifikaatorite, nii et saate asendada selle `-RoleMemberObjectId` parameetri `ObjectId` ülevalt ja käivitage järgmised käsud. Kõik directory rollid, proovige töötab loendi vaatamiseks `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Nüüd on rakendus, mis on õigus loomine, lugemine, värskendamine ja kustutamine kasutajad oma B2C rentniku.

## <a name="download-configure-and-build-the-sample-code"></a>Laadige alla, konfigureerimine ja koostada proovi kood

Esmalt Laadige alla proovi kood ja saada see töötab. Seejärel võtame seda lähemalt.  Saate [alla laadida proovi kood ZIP-faili](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Samuti saate selle klooni kataloogi tehtud valikust:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Avage soovitud `B2CGraphClient\B2CGraphClient.sln` Visual Studio lahenduse Visual Studios. Klõpsake soovitud `B2CGraphClient` projekti, avage fail `App.config`. Oma väärtuste kolme rakenduse sätete asendamiseks tehke järgmist.

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Järgmine, paremklõpsake soovitud `B2CGraphClient` lahenduse ja taastada proovi. Kui te peaks nüüd on teil on `B2C.exe` asub täitmisfail `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Luua kasutaja CRUD toimingud Graph API abil

Funktsiooni B2CGraphClient kasutamiseks avage on `cmd` Windowsi Käsuviip ja muuta oma kataloogi, on `Debug` kataloogi. Käivitage selle `B2C Help` käsk.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

See kuvab iga käsu lühikirjeldus. Iga kord, kui kutsute mõnda need käsud `B2CGraphClient` esitab taotluse Azure AD Graph API.

### <a name="get-an-access-token"></a>Saada juurdepääsu sümboolse

Mis tahes taotluse Graph API nõuab autentimist juurdepääsu sümboolse. `B2CGraphClient`kasutab avatud lähtekoodi Active Directory autentimise Raamatukogu (ADAL) hankida access sõned. ADAL lihtsustab lihtsa API ja hoolitseb olulisest teabest, nt vahemällu Accessi sõned Turbeloa tuua. Teil pole saada märkide küll ADAL abil. Samuti saate sõned, käsitöö HTTP päringuid.

> [AZURE.NOTE]
    Seda proovi kood kasutab ADAL v2 Graph API suhelda.  ADAL v2 või v3 tuleb kasutada selleks, et saada juurdepääsu märgid, mida saab kasutada Azure AD Graph API-ga.

Kui `B2CGraphClient` töötab, loob see eksemplar on `B2CGraphClient` klassi. See tund konstruktorit häälestab on ADAL-autentimine tellingud:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Kasutame funktsiooni `B2C Get-User` käsu näitena. Kui `B2C Get-User` järgida ilma mis tahes täiendavaid sisendeid CLI kõned on `B2CGraphClient.GetAllUsers(...)` meetod. See nõuab `B2CGraphClient.SendGraphGetRequest(...)`, mis esitab HTTP GET-päring Graph API. Enne `B2CGraphClient.SendGraphGetRequest(...)` saadab koosolekukutse toomine, see esmalt saab Accessi Turbeloa ADAL abil:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Te võite Accessi Turbeloa Graph API helistades on ADAL `AuthenticationContext.AcquireToken(...)` meetod. ADAL tagastab mõne `access_token` mis tähistab rakenduse identiteedi.

### <a name="read-users"></a>Kasutajate lugemine

Kui soovite saada kasutajate loendit või teatud kasutaja toomine Graph API, saate saata HTTP `GET` taotlus on `/users` lõpp-punkti. Kõigi rentniku kasutajate taotluse näeb välja umbes järgmine:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Käivitage selle päringu kuvamiseks:

 ```
 > B2C Get-User
 ```

Pange tähele, et kahte olulist asja on:

- ADAL kaudu juurdepääsu luba lisatakse selle `Authorization` päis, kasutades funktsiooni `Bearer` värviskeemi.
- B2C rentnikud, peate kasutama päringu parameeter `api-version=1.6`.

Nii need andmed käsitletakse selle `B2CGraphClient.SendGraphGetRequest(...)` meetod:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Tarbija kasutajakontode loomine

Kui loote teie B2C rentnikus Kasutajakontod, saate saata HTTP `POST` taotlus on `/users` lõpp-punkt:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Enamik atribuutidest taotlus on nõutav tarbija kasutajate loomiseks. Lisateabe saamiseks klõpsake [siin](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Pange tähele, et selle `//` illustratsioon on lisatud kommentaarid. Ärge lisage need tegeliku taotluse.

Taotluse kuvamiseks käivitage järgmised käsud:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

Funktsiooni `Create-User` käsk võtab mõne sisendparameetrile .json faili. See sisaldab JSON kujutis kasutaja objekti. Proovi kood on kaks näidisfailide .json: `usertemplate-email.json` ja `usertemplate-username.json`. Saate muuta nende failide vastavalt oma vajadustele. Lisaks ülaltoodud nõutavad väljad, kaasatakse mitu valikuline väljad, mille abil saate need failid. Valikuliste väljade üksikasjad leiate [Azure'i AD Graph API üksuse viide](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Saate vaadata, kuidas postituse kutse on valmistatud `B2CGraphClient.SendGraphPostRequest(...)`.

- Ta peab juurdepääsu luba selle `Authorization` taotluse päis.
- Seda määrab `api-version=1.6`.
- See sisaldab JSON kasutaja objekti taotluse kehas.

> [AZURE.NOTE]
Kui kontod, mida soovite migreerida poest olemasoleva kasutaja on väiksem parooli tugevus kui [keeruka parooli tugevus jõustatud Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), saate keelata keeruka parooli nõue abil soovitud `DisableStrongPassword` väärtus on `passwordPolicies` atribuut. Näiteks saate muuta Loo kasutaja taotluse esitatud kohal järgmiselt: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Värskendage tarbija Kasutajakontod

Kasutaja objektid värskendamisel protsess on sarnane abil saate luua kasutaja objektid. Kuid see protsess kasutab HTTP `PATCH` meetod:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Proovige värskendada kasutaja, värskendades JSON failide uute andmetega. Seejärel saate `B2CGraphClient` käivitamiseks ühte need käsud:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Kontrolli selle `B2CGraphClient.SendGraphPatchRequest(...)` meetod täpsemat teavet selle kutse saatmiseks.

### <a name="search-users"></a>Otsingu kasutajad

Kasutajate jaoks saate otsida oma B2C rentniku mitmel viisil. Ühe kasutaja abil objekti ID-d või kaks, kasutades kasutaja sisselogimise identifer (st on `signInNames` atribuudi).

Käivitage ühte järgmistest käskudest otsida mõne kindla kasutaja.

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Siin on mõned näited.

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Kasutaja kustutamine

Kasutaja kustutamise protsessi on väga lihtne. Kasutage HTTP `DELETE` meetod ja ehitada URL on õige objekti ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Näide, sisestage see käsk ja Kustuta kutse, mis prinditakse konsooli vaadata:

```
> B2C Delete-User <object-id-of-user>
```

Kontrolli selle `B2CGraphClient.SendGraphDeleteRequest(...)` meetod täpsemat teavet selle kutse saatmiseks.

Paljud muude toimingute Azure AD Graph API Lisaks kasutajate haldamine, saate seda teha. [Azure'i AD Graph API viide](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) sisaldab üksikasju iga toimingu koos valimi taotlused.

## <a name="use-custom-attributes"></a>Kasutage kohandatud atribuudid

Enamik tarbija rakenduste teatud tüüpi kohandatud kasutajaprofiili teabe talletamiseks vaja läheb. Üks viis selleks on määratleda kohandatud atribuudi oma B2C rentnik. Seejärel saate kohelge atribuudi samamoodi valite kasutaja objekti muud atribuudid. Atribuut värskendada, atribuudi kustutamine, päringu atribuudi järgi, atribuut saatmine nõude sisselogimise sõned ja palju muud.

Määratleda kohandatud atribuudi teie B2C rentnikus, lugege teemat [B2C kohandatud atribuudi viide](active-directory-b2c-reference-custom-attr.md).

Kohandatud atribuute, mis on määratletud teie B2C rentnikus abil saate vaadata `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Väljund nende funktsioonide ilmneb iga kohandatud atribuudi üksikasju, näiteks:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Saate kasutada täielik nimi, näiteks `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, teie kasutaja objektide atribuudi.  Uue atribuudi ja väärtuse atribuudi .json faili värskendada, ja seejärel käivitage:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Kasutades `B2CGraphClient`, peate teenuserakenduse, mida saate hallata kasutajaid B2C rentniku programmiliselt. `B2CGraphClient`kasutab oma rakenduse identiteedi Azure AD Graph API autentimiseks. See saab ka sõned kliendi salajane abil. See funktsioon lisada rakenduse, pidage meeles mõned põhipunktid B2C rakendused:

- Peate anda rakenduse rentniku asjakohased õigused.
- Nüüd, peate kasutama ADAL v2 saada juurdepääsu sõned. (Lisaks saate saata sõnumeid Protocol (protokoll) otse teegi kasutamata.)
- Kui helistate Graph API, kasutage `api-version=1.6`.
- Kui loote ja värskendada tarbija kasutajad, mõne atribuudid on nõutav, ülalpool kirjeldatud.

Kui teil on küsimusi või soovite oma B2C rentnikus Graph API abil saate teha toiminguid taotlused, kommentaar käesoleva artikli või faili probleemi GitHub kood valimi hoidlas.
