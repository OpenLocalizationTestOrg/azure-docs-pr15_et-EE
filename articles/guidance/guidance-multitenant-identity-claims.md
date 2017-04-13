<properties
   pageTitle="Töötamine põhise taotluste identiteedid rentnikuga rakendustes | Microsoft Azure'i"
   description="Kuidas kasutada väidab väljaandja kinnitamine ja luba"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Taotlusepõhise identiteedid rentnikuga rakendustes töötamine

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

See artikkel on [osa sarjast]. Olemas on ka täieliku [valimi rakendus] , mis kaasneb selle sarja.

## <a name="claims-in-azure-ad"></a>Azure AD nõuded

Kui kasutaja sisse logib, suunatakse Azure AD ID luba, mis sisaldab taotlusi kasutaja kohta. Nõue on lihtsalt soovitud teabelõigule, väljendatud hinnaks dollarites paari /-väärtuse. Näiteks `email` = `bob@contoso.com`.  Nõuded on mõni väljaandja &mdash; sel juhul Azure AD &mdash; mis on üksus, mis autendib kasutaja ja loob nõuded. Usaldate nõuete, kuna usaldate väljaandja. (Vastupidi, kui te ei usalda väljaandja, ära usalda nõuete!)

Kõrge:

1.  Kasutaja autendib.
2.  Funktsiooni IDP saadab taotluste kogum.
3.  Rakenduse normaliseerib või suurendab nõuete (valikuline).
4.  Rakendus kasutab nõuete autoriseerimine otsuseid langetada.

OpenID Connect väidab, et teile määratud kontrollib [ulatus parameetri] taotluse autentimist. Siiski Azure AD probleemid piiratud hulk taotluste kaudu OpenID ühenduse loomine; lugege teemat [toetatud Turbeloa ja taotluste tüübid]. Kui soovite lisateavet kasutaja, peate kasutama Azure AD Graph API.

Siin on mõned nõuded: AAD, mis rakendus võib tavaliselt kohta:

Tippige ID luba taotlemine |    Kirjeldus
-----------------------|--------------
AUD | Kes on antud luba jaoks. Sellest saab rakenduse kliendi ID-ga. Üldiselt ei tohiks pea muretsema selle nõude, kuna selle vahevara automaatselt kinnitatakse see. Näide:`"91464657-d17a-4327-91f3-2ed99386406f"`
rühmad   | Loendi AAD rühmade kasutaja kuulub. Näide:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
ISS  | [Väljaandja] OIDC luba. Näide:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
Nimi    | Kasutaja kuvatav nimi. Näide:`"Alice A."`
objekti ID | Objekti identifikaator AAD kasutaja. See väärtus on kasutaja püsiv ja ühekordselt ainuidentifikaator. Kasutage selle väärtuse, mitte e-posti kordumatut tunnust kasutajatele; e-posti aadressid, saate muuta. Kui kasutate Azure AD Graph API rakenduse, objekti ID on seda väärtust kasutatakse päringu profiiliteavet. Näide:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
rollid   | Kasutaja rakenduse rollide loend. Näide:`["SurveyCreator"]`
tid | Rentniku ID-ga. See väärtus on Azure AD rentniku jaoks kordumatut tunnust. Näide:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Kasutaja inimeste loetav kuvatav nimi. Näide:`"alice@contoso.com"`
UPN-i | Kasutaja turvasubjektinimi. Näide:`"alice@contoso.com"`

Selles tabelis on loetletud taotluste tüüpi kuvatavate ID luba. ASP.net-i Core 1.0, OpenID Connect vahevara teisendab teatud tüüpi taotluste kui see kuvab taotluste saidikogumi põhisumma kasutaja jaoks.

-   objekti ID >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   tid >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   UPN-i >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Nõuded teisendused

Ajal autentimist voogu, võiksite muuta taotluste, mille saate ka IDP. ASP.net-i Core 1.0, saate teha taotluste teisendus sees **AuthenticationValidated** sündmuse OpenID Connect vahevara kaudu. (Vt [autentimise sündmused]).

Mis tahes väidab, et lisate ajal **AuthenticationValidated** on talletatud seansi autentimise küpsise. Ta ei saa lükata tagasi Azure AD.

Siin on mõned näited taotluste teisendus:

-   **Nõuded normaliseerimine**või tegemisega taotluste ühtsete kasutajad üle. See on eriti oluline, kui teil saavad taotluste mitme ümberasustatud isikule, mida võib kasutada taotluste eri tüüpi sarnase teabega.
Näiteks Azure AD saadab "upn" taotluste, mis sisaldab kasutaja e-posti. Muud sisepagulaste reintegreerimine võib saada "e" taotlemine. Järgmine kood teisendab taotluste "upn" on "e-posti" taotluste:

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Lisada **vaikimisi taotlemine väärtused** nõuete, mis ei sisalda &mdash; näiteks vaikimisi rolli määramine kasutajale. Mõnel juhul võib see lihtsustada autoriseerimine loogika.
- Saate lisada **kohandatud taotluste tüüpi** teavet rakenduse kasutaja kohta. Näiteks võib andmebaasi talletada teavet kasutaja kohta. Saate lisada kohandatud taotluste selle teabe Piletite autentimist. Nõude talletatakse küpsis, seega peate ainult toomine andmebaasist kord seansi Logi sisse. Teisalt, soovite ka vältida liiga suur küpsised, seega tuleb arvestada vahelist küpsise suurus ja andmebaasi otsingud.   

Pärast autentimist voogu on lõpule jõudnud, et nõuded on saadaval `HttpContext.User`. Sel hetkel tuleks käsitleda neid kirjutuskaitstud kogumina &mdash; nt nende abil autoriseerimine otsuseid langetada.

## <a name="issuer-validation"></a>Väljaandja valideerimine
OpenID Connect tuvastab ID luba väljastanud IDP väljaandja taotluste ("iss"). Osa kulgemist OIDC autentimine on veenduge, et väljaandja taotluste vastab tegeliku väljaandja. OIDC vahevara tegeleb see teie eest.

Azure AD, väljaandja väärtus on kordumatu AD rentniku kohta (`https://sts.windows.net/<tenantID>`). Seetõttu rakendus peaks tegema täiendavad sisse, veendumaks, et väljaandja tähistab rentniku, mis on lubatud rakendusse sisse logida.

Ühe-rentniku rakenduse, saate vaadata vaid, et väljaandja on oma rentniku. Tegelikult OIDC vahevara see automaatselt vaikimisi. Mitme rentniku rakenduses, peate esmalt lubama vastav erinevate rentnikega mitme emitentidele. Siin on üldise lähenemisviisi kasutada:

-   OIDC vahevara suvandid **ValidateIssuer** väärtuseks false. See lülitatakse automaatselt sisse välja.
-   Kui rentnikukonto registreerub, hoida oma kasutaja DB rentniku ja väljaandja.
-   Iga kord, kui kasutaja sisse logib, otsige andmebaasi väljaandja. Kui väljaandja ei leita, tähendab see, et rentnik pole registreerunud. Saate neid ümbersuunamiseks registreeruda lehel.
-  Teil võib ka musta teatud rentnikud; näiteks klientidele, kes ei maksa oma tellimust.

Täpsema teabe, lugege teemat [registreerumise ja rentniku kasutuselevõtt rentnikuga rakenduses][signup].

## <a name="using-claims-for-authorization"></a>Kasutamise loa nõuded

Nõuded, kasutaja identiteet pole enam monoliit üksus. Näiteks kasutaja võib olla meiliaadressi, telefoninumbri, sünnipäev, sugu, jne. Võib-olla kasutaja IDP talletab kõik selle andmed. Kuid kui kasutaja autentimiseks, siis saate tavaliselt neist taotluste alamhulk. Selle mudeli kasutaja identiteet on lihtsalt kogumi taotluste. Kui teete autoriseerimine otsuseid kasutaja kohta, saate otsida taotluste kogumit. Teisisõnu, küsimus "Saate kasutaja X toimingu teostamine Y" lõpuks muutub "Kas kasutaja X on nõuda Y".

Siin on mõned põhilised mustrid taotluste kontrollimiseks.

-  Kontrollimaks, et kasutajal on teatud kindlate taotluste kindla väärtusega:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Järgmine kood kontrollib, kas kasutajal on roll taotluste väärtusega "Admin". Käsitlemisel õigesti juhul, kui kasutaja on pole rolli taotluste või mitme rolli taotluste.

    Klassi **ClaimTypes** määratleb konstantide enim kasutatud taotluste tüüpi. Siiski saate mis tahes stringiväärtuse taotluse tüüp.

-   Ühe väärtuse saamiseks taotluse tüüp, kui eeldate, et kõige ühe väärtuse:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Saada kõik väärtused taotluse tüüp:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Lisateabe saamiseks lugege teemat [rolli ja ressursside autoriseerimine rentnikuga rakendustes][authorization].

## <a name="next-steps"></a>Järgmised sammud

- Järgmise artiklist selle sarja: [registreerumise ja rentniku kasutuselevõtt rentnikuga rakenduses][signup]
- Lugege lisateavet [taotlusepõhise autoriseerimine] ASP.net-i Core 1.0 dokumentides

<!-- Links -->
[Sarja mittekuuluva]: guidance-multitenant-identity.md
[ulatuse parameeter.]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Toetatud Turbeloa ja alused]: ../active-directory/active-directory-token-and-claims.md
[väljaandja]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Autentimise sündmused]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Taotlusepõhise autoriseerimine]: https://docs.asp.net/en/latest/security/authorization/claims.html
[proovi taotluse]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
