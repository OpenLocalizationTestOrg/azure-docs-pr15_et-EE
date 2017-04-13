<properties
    pageTitle="Võtme edastamiseks sisselogimisel Azure AD | Microsoft Azure'i"
    description="Selles artiklis käsitletakse Azure Active Directory jaoks allkirjastamiseks võtme edastamiseks head tavad"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Sisselogimise klahv edastamiseks Azure Active Directory 's

Selles teemas käsitletakse, mida on vaja teada, kuidas avaliku klahvid, mida kasutatakse Azure Active Directory (Azure AD) sisselogimise turvalisus sõned. See on oluline Pange tähele, et need klahvid edastamiseks perioodiliselt ja hädaolukorras võib kerida kohe. Kõik rakendused, mis kasutavad Azure AD peaks oskama programmiliselt toime võtme edastamiseks protsessi või luua perioodiliste käsitsi edastamiseks protsess. Edasi lugedes mõistmiseks võtmed töötada, kuidas edastamiseks rakenduse mõju ja kuidas värskendada rakenduse või luua perioodiliste käsitsi edastamiseks protsess käsitlema klahv edastamiseks vajaduse korral.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Klahvid sisselogimisel Azure AD ülevaade

Azure AD kasutab avaliku võtme krüptograafia tugineb tööstusstandarditele usalda enda ja rakendusi, mis kasutavad seda vahel. Praktikas see toimib järgmiselt: Azure'i AD kasutab allkirjastamiseks võti, mis koosneb avalike ja privaatvõ võtme paari. Kui kasutaja sisse logib rakendus kasutab Azure AD autentimine, loob Azure AD Turbeloa, mis sisaldab teavet kasutaja kohta. Selle märgiks on Azure AD abil oma privaatvõti enne saatmist tagasi rakendus alla. Veenduge, et luba on lubatud ja tegelikult pärineb: Azure'i AD, rakenduse peate kontrollima selle luba allkirja abil esitatud Azure AD rentniku [OpenID Connect discovery](http://openid.net/specs/openid-connect-discovery-1_0.html) või SAML/oli-Fed [federation metaandmete dokumendi](active-directory-federation-metadata.md)olev avalik võti.

Turvalisuse huvides Azure AD sisselogimise klahv rullides perioodiliselt ja hädaolukorra võib kerida kohe. Mis tahes rakendus, mis ühendab Azure AD peaks olema valmis hakkama võtme edastamiseks sündmuse sõltumata sellest, kui sageli võib juhtuda. See ei, kui teie rakendus proovib aegunud klahvi allkirja valemisilti, taotluse sisselogimine nurjub.

Alati on rohkem kui üks kehtiv võti saadaval OpenID Connect discovery dokumendi ja federation metaandmete dokument. Rakenduse peaks olema valmis kasutama ühte dokumenti määratud võtmed, kuna üks klahv võib kiiresti võetud, teise võib olla selle asendamine ja jne.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Kuidas hindamaks, kui rakenduse mõjutab ja mida teha

Kuidas käsitleb rakenduse tootenumbri edastamiseks sõltub muutujate näiteks või kasutatud mis identiteedi Protocol (protokoll) ja teegi tüüp. Järgmistes jaotistes hinnata, kas levinumat tüüpi rakendusi mõjutab võtme edastamiseks ja antakse juhised värskendamiseks rakendus toetab automaatse või käsitsi värskendada võti.

* [Juurdepääs ressursid kohalikke klientrakendused](#nativeclient)
* [Veebirakenduste / API-de juurdepääs ressursid](#webclient)
* [Veebirakenduste / API-de kaitsmine ressursid ja Azure rakenduse Services abil loodud](#appservices)
* [Veebirakenduste / API-de abil .NET OWIN OpenID Connect oli-Fed või WindowsAzureActiveDirectoryBearerAuthentication vahevara kaitsmine](#owin)
* [Veebirakenduste / API-de abil .NET Core OpenID ühendada või JwtBearerAuthentication vahevara kaitsmine](#owincore)
* [Veebirakenduste / API-de abil Node.js pass-azure-ad moodul kaitsmine](#passport)
* [Veebirakenduste / API-de kaitsmine ressursid ja loodud Visual Studio 2015](#vs2015)
* [Veebirakenduste kaitsmine ressursid ja loodud Visual Studio 2013](#vs2013)
* [Web API-de kaitsmine ressursid ja loodud Visual Studio 2013](#vs2013_webapi)
* [Veebirakenduste kaitsmine ressursid ja loodud Visual Studio 2012](#vs2012)
* [Veebirakenduste kaitsmine ressursid ja Visual Studio 2010 2008 o Windows Identity Foundation abil loodud](#vs2010)
* [Veebirakenduste / API-de ressursid muude teekide abil või käsitsi rakendamiseks mis tahes toetatud Protokollid kaitsmine](#other)

Juhised on **pole** mõeldud:

* Rakenduste lisatud galeriist Azure AD Rakenduse (sh kohandatud) on eraldi juhiseid seoses allkirjastamise võtmed. [Lisateabe saamiseks.](active-directory-sso-certs.md)
* Kohapealse rakenduse puhverserveri kaudu avaldatud ei pea muretsema klahvid sisselogimise kohta.

### <a name="nativeclient"></a>Omakliendi rakenduste juurdepääs ressursid

Rakendusi, mis on juurdepääs ainult ressursid (st Microsoft Graphi, KeyVault, Outlooki API ja muude Microsofti APIs) üldiselt ainult saada märgiks ja andke seda mööda ressursi omanik. Nad ei kaitse ressursse, et kontrollida luba ja seetõttu ei vaja, et tagada, et see on õigesti allkirjastatud.

Omakliendi rakendused, kas arvuti või mobiilsideseadme, sellesse kategooriasse kuuluvad ja seega ei mõjuta selle edastamiseks.

### <a name="webclient"></a>Veebirakenduste / API-de juurdepääs ressursid

Rakendusi, mis on juurdepääs ainult ressursid (st Microsoft Graphi, KeyVault, Outlooki API ja muude Microsofti APIs) üldiselt ainult saada märgiks ja andke seda mööda ressursi omanik. Nad ei kaitse ressursse, et kontrollida luba ja seetõttu ei vaja, et tagada, et see on õigesti allkirjastatud.

Veebirakenduste ja kasutate ainult rakenduse voogu API-de web (klient mandaate / kliendi sert), see kategooriasse kuuluvad ja seega ei mõjuta selle edastamiseks.

### <a name="appservices"></a>Veebirakenduste / API-de kaitsmine ressursid ja Azure rakenduse Services abil loodud

Azure'i rakenduse teenuste autentimine / autoriseerimine (EasyAuth) funktsioonid on juba olemas vajalikud loogika käsitlema võtme edastamiseks automaatselt.

### <a name="owin"></a>Veebirakenduste / API-de abil .NET OWIN OpenID Connect oli-Fed või WindowsAzureActiveDirectoryBearerAuthentication vahevara kaitsmine

Kui teie rakendus kasutab .NET OWIN OpenID ühendamine, oli-Fed või WindowsAzureActiveDirectoryBearerAuthentication vahevara, on juba vajalikud loogika käsitlema võtme edastamiseks automaatselt.

Saate kinnitada, et teie rakendus kasutab mõnda neist ühte järgmistest Koodilõigud rakenduse Startup.cs või Startup.Auth.cs otsides

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Veebirakenduste / API-de abil .NET Core OpenID ühendada või JwtBearerAuthentication vahevara kaitsmine

Kui teie rakendus kasutab .NET Core OWIN OpenID ühendamine või JwtBearerAuthentication vahevara, on juba vajalikke loogika käsitlema klahv edastamiseks automaatselt.

Saate kinnitada, et teie rakendus kasutab mõnda neist ühte järgmistest Koodilõigud rakenduse Startup.cs või Startup.Auth.cs otsides

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Veebirakenduste / API-de abil Node.js pass-azure-ad moodul kaitsmine

Kui teie rakendus kasutab Node.js pass-ad moodul, on juba vajalikud loogika käsitlema võtme edastamiseks automaatselt.

Saate kinnitada, et teie rakenduse pass-ad, kui otsite järgmised koodilõigu sisse oma rakenduse app.js

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Veebirakenduste / API-de kaitsmine ressursid ja loodud Visual Studio 2015

Kui rakenduse ehitatud web rakenduse malli kasutamine Visual Studio 2015 ja teie valitud **Töö ja kooli kontode** menüüst **Muuda autentimist** , on juba vajalikud loogika käsitlema võtme edastamiseks automaatselt. Selle loogika OWIN OpenID Connect vahevara, manustatud laadib ja vahemälu dokumendist OpenID Connect discovery võtmed ja perioodiliselt värskendab neid.

Kui olete lisanud autentimise teie lahendus käsitsi, ei pruugi rakenduse vajalikud võtme edastamiseks loogika. Peate kirjutada seda ise või järgige [veebirakenduste / API-de muude teekide abil või käsitsi rakendamiseks mis tahes toetatud protokolle.](#other).

### <a name="vs2013"></a>Veebirakenduste kaitsmine ressursid ja loodud Visual Studio 2013

Kui rakenduse ehitatud Visual Studio 2013 web rakenduse malli abil ja **Muuda autentimist** menüüs valitud **Ettevõtte kontod** , on juba vajalikud loogika käsitlema võtme edastamiseks automaatselt. See loogika salvestab teie asutuse ainuidentifikaator ja allkirjastamiseks põhiteave projektiga seotud kaks andmebaasi tabelites. Leiate ühendusstringi andmebaasi projekti fail.

Kui olete lisanud autentimise teie lahendus käsitsi, ei pruugi rakenduse vajalikud võtme edastamiseks loogika. Peate kirjutada seda ise või järgige [veebirakenduste / API-de muude teekide abil või käsitsi rakendamiseks mis tahes toetatud protokolle.](#other).

Järgmised juhised aitavad teil kinnitamine loogika töötab korralikult oma rakenduse.

1. Visual Studio 2013, avage lahendus ja seejärel klõpsake vahekaarti **Server Exploreri** akna paremas.
2. Laiendage **andmeühendusi**, **DefaultConnection**ja **tabeleid**. Leidke **IssuingAuthorityKeys** tabeli, paremklõpsake seda ja seejärel klõpsake nuppu **Kuva tabeli andmeid**.
3. Tabelis **IssuingAuthorityKeys** on vähemalt üks rida, mis vastab sõrmejälje võti väärtus. Mis tahes tabeli ridu kustutada.
4. Paremklõpsake **rentnikud** tabelit ja seejärel klõpsake nuppu **Kuva tabeli andmed**.
5. Tabelis **rentnikud** on vähemalt üks rida, mis vastab kordumatu register rentniku identifikaator. Mis tahes tabeli ridu kustutada. Kui te ei kustutaks **rentnikud** tabeli ja **IssuingAuthorityKeys** tabeli read, saate tõrke käitusajal.
6. Koostamine ja käivitage rakendus. Kui olete oma kontosse sisse logitud, saate peatada rakendus.
7. Naaske **Server Explorer** ja vaadake **IssuingAuthorityKeys** ja **rentnikud** tabeli väärtused. Märkate, et need on antud automaatselt uuesti asustada dokumendist federation metaandmete asjakohast teavet.

### <a name="vs2013"></a>Web API-de kaitsmine ressursid ja loodud Visual Studio 2013

Kui olete loonud web API rakenduse Visual Studio 2013 Web API malli abil, ja seejärel **Muuda autentimist** menüüs valitud **Ettevõtte kontod** , on juba vajalikke loogika oma rakenduse.

Kui olete konfigureerinud käsitsi autentimine, järgige alltoodud juhiseid, et saate teada, kuidas konfigureerida oma Web API automaatselt värskendada oma olulist teavet.

Järgmised koodilõigu näitab, kuidas viimased klahvid toomine federation metaandmete dokument ja seejärel kasutage [JWT Turbeloa sündmuseohjuri](https://msdn.microsoft.com/library/dn205065.aspx) valideerimiseks luba. Koodilõigu eeldab, et kasutate oma vahemällu talletamise süsteem püsib võti valideerimiseks tulevaste sõnet Azure AD, olgu see siis andmebaasi, konfiguratsioonifail või mujal.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Veebirakenduste kaitsmine ressursid ja loodud Visual Studio 2012

Kui rakenduse Visual Studio 2012, tõenäoliselt kasutasite identiteedi ja juurdepääsu tööriista rakenduse konfigureerimiseks. Samuti on tõenäoline, et kasutate [Kontrollimine väljaandja nimi registri (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). Funktsiooni VINR eest ja nende väljastatud sõned valideerimiseks võtmed usaldusväärsete identiteedipakkujad (Azure AD) kohta. Funktsiooni VINR on samuti lihtne värskendamiseks automaatselt salvestatud faili Web.config uusima federation metaandmete dokument seostatud kataloogi, laadides olulist teavet konfiguratsiooni kursis uusimate dokumendi kontrollimine ja rakenduse kasutamiseks uue tootenumbri vastavalt vajadusele.

Kui lõite rakenduse kasutamine koodinäiteid või Microsofti kiirtutvustus dokumente, võtme edastamiseks loogika on juba kaasatud projekti. Märkate, et allpool kood on juba olemas projektis. Kui teie rakendus pole veel see loogika, tehke järgmist, et lisada see ning veenduge, et see ei tööta õigesti.

1. **Solution Exploreris** **System.IdentityModel** komplekti sobiv projekti viite lisamiseks.
2. Avage fail **Global.asax.cs** ja lisage järgmine suuniseid abil:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Lisage **Global.asax.cs** faili järgmisel viisil:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Autonoomsest **Application_Start()** meetod **Global.asax.cs** **RefreshValidationSettings()** meetod, nagu on näidatud:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Pärast nende juhiste järgimist värskendatakse teie taotlus Web.config dokumendist federation metaandmete, sh viimased klahvid värskeimaid andmeid. Selle värskenduse ilmneb iga kord, kui teie rakendus rakenduskausta ringlusse IIS-i; Vaikimisi on seatud IIS-i prügikastis rakenduste iga 29 tundi.

Tehke järgmist, et klahv edastamiseks loogika töötamise kontrollimiseks.

1. Pärast, et teie rakendus kasutab ülaltoodud kood, avage **fail** ja liikuge soovitud **<issuerNameRegistry>** blokeerida, spetsiaalselt otsivad järgmisi ridu:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. Klõpsake soovitud **<add thumbprint=””>** säte, asendades mis tahes märgi mõne teise sõrmejälje väärtust muuta. Salvestage **fail** .

3. Rakenduse koostamine ja seejärel käivitage see. Kui saate sisselogimise protsessi lõpule viia, edukalt värskendamine rakenduse allalaadimine vajalik teave oma directory federation metaandmete dokumendi võti. Kui teil on sisselogimisega probleeme, veenduge, et rakenduse muudatused on õige teema [Lisamise sisselogimise Web rakenduse kasutamine Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) lugemise või laadides ja kontrollimise järgmine kood näidis: [Mitme rentniku pilve rakendus Azure Active Directory jaoks](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>Veebirakenduste kaitsmine ressursid ja Visual Studio 2008 või 2010 loodud ja Windows Identity Foundation (WIF) v1.0 .NET 3.5

Kui varem rakenduse WIF v1.0, on automaatselt värskendada oma rakenduse konfiguratsioon uue tootenumbri kasutamiseks ei ole esitatud süsteem.

- *Lihtsaim viis* Kasutage FedUtil instrumentaarium kaasatud WIF Tarkvaraarenduskomplektist, mis võib tuua uusimate metaandmete dokument ja värskendage oma konfiguratsioon.
- Värskendage oma rakenduse .NET 4.5, mis sisaldab WIF asub süsteemi nimeruumi uusim versioon. Seejärel saate teha rakenduse konfiguratsioon automaatne uuendamine [Kontrollimine väljaandja nimi registri (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) .
- Tehke käsitsi edastamiseks juhendi lõpus juhistele.

Juhised selle FedUtil saate värskendada oma konfiguratsioon:

1. Veenduge, et teil on WIF v1.0 SDK Visual Studio 2008 või 2010 arengu teie arvutisse installitud. Saate [alla laadida järgmiselt](https://www.microsoft.com/en-us/download/details.aspx?id=4451) , kui te pole veel installinud seda.
2. Visual Studio, avage lahendus, ja seejärel paremklõpsake rakendatav projekt ja valige **federation metaandmete värskendamine**. Kui see suvand pole saadaval, FedUtil ja/või WIF v1.0 SDK pole installitud.
3. Vastava viiba kuvamisel valige **Update** alustada oma federation metaandmete värskendamine. Kui teil on juurdepääs serveri keskkonnas, kus on majutatud rakenduse, saate soovi korral FedUtil's [metaandmete automaatne värskendamine ajasti](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klõpsake nuppu **valmis** värskenduse lõpuleviimiseks.

### <a name="other"></a>Veebirakenduste / API-de ressursid muude teekide abil või käsitsi rakendamiseks mis tahes toetatud Protokollid kaitsmine

Kui kasutate mõnda muud teegi või käsitsi rakendada mis tahes toetatud protokollid, peate teeki või oma rakendamist tagamaks, et võti tuuakse OpenID Connect discovery dokument või federation metaandmete dokument läbi vaadata. Selle üheks võimaluseks on teha otsingu oma kood või teegi jaoks OpenID discovery dokument või federation metaandmete dokumendi telefoniga.

Kui nad oluline on salvestatud kuskil või kõva oma rakenduse, saate käsitsi tuua võti ja Värskenda see vastavalt sellele, teha käsitsi edastamiseks juhendi lõpus juhistele. Selles artiklis tulevaste häirete vältimiseks **on tungivalt, et te täiustamiseks rakenduse toetamiseks automaatse** kasutamise võimalused liigendamine ja kohal siis, kui Azure AD suurendab see edastamiseks sagedus või on mõni hädaolukorra riba-, edastamiseks.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Kuidas kindlaks teha, kui see mõjutab rakenduse testida

Te saate kontrollida, kas teie rakendus toetab automaatse võtme edastamiseks laadides skripte ja juhiste [selle GitHub hoidla.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Kuidas teha käsitsi edastamiseks, kui saate rakenduse ei toeta automaatse

Kui rakenduse **tugi automaatset pikendamist** , peate luua protsess, mis perioodiliselt jälgib Azure AD allkirjastamiseks võtmed ja sooritab käsitsi edastamiseks vastavalt sellele. [See GitHub hoidla](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) sisaldab skripte ja juhised, kuidas seda teha.
