<properties
    pageTitle="Azure Active Directory B2C: Piirangud ja piirangutega | Microsoft Azure'i"
    description="Loendi piirangud ja piirangutega Azure Active Directory B2C abil"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: Piirangud ja piirangutega

On mitmeid funktsioone ja Azure Active Directory (Azure AD) B2C funktsioone, mis pole veel toetatud. Paljude järgmiste teadaolevate probleemide ja piirangud käsitletakse edasi, kuid peaksite olema teadlikud, kui on tarbija suunatud rakenduste abil Azure AD B2C loomine.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Azure'i AD B2C rentnikud loomise ajal probleemid

Kui [Azure AD B2C rentniku loomise](active-directory-b2c-get-started.md)ajal ilmneb probleeme, lugege juhiseid [on Azure AD rentniku või Azure AD B2C rentniku--probleemide ja lahenduste loomine](active-directory-b2c-support-create-directory.md) .

Pange tähele, et on teadaolevad probleemid, kui kustutate mõne olemasoleva B2C rentniku ja uuesti luua selle domeeni nimi. Teil on muu domeeninimega B2C rentniku loomine.

## <a name="note-about-b2c-tenant-quotas"></a>Pange tähele kvootide B2C rentniku kohta

Vaikimisi B2C rentniku kasutajate arv on piiratud 50 000 kasutaja. Kui teil on vaja oma B2C rentniku talletusmahu suurendamiseks, võtke abi.

## <a name="branding-issues-on-verification-email"></a>Klõpsake kinnitusmeil margikujundusprobleemid

Vaikimisi kinnitusmeil sisaldab Microsoft sümboolika. Meil eemaldage see tulevikus. Nüüd, saate selle [ettevõtte tootemargi funktsiooni](../active-directory/active-directory-add-company-branding.md)abil eemaldada.

## <a name="restrictions-on-applications"></a>Rakenduste piirangud

Järgmist tüüpi rakendusi Azure AD B2C pole praegu toetatud. Toetatud tüüpi rakendusi kirjeldus, vaadake [Azure AD B2C: tüüpi rakendusi](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Ühelt leheküljelt rakenduste (JavaScript)

Paljud tänapäevased rakendused on ühe lehe rakenduse (spaa) ees, mis on kirjutatud peamiselt JavaScript ja sageli kasutab mõnda SPA framework nagu AngularJS, Ember.js, Durandal jne. See vool ei ole veel saadaval Azure AD B2C.

### <a name="daemons--server-side-applications"></a>Deemonid / serveripoolne rakendused

Samuti vajate rakendusi, mis sisaldavad pikaajalisi protsesside või mis toimivad ilma kasutaja kohalolek turvalise ressursse, näiteks Web API-d juurdepääsu. Need rakendused saate autentimiseks ja märkide abil rakenduse identiteedi (mitte tarbija delegeeritud identiteedi) [OAuth 2.0 kliendi identimisteabe kulgemist](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow). See vool ei ole veel saadaval Azure AD B2C, nii praegu rakendused saavad sõned alles pärast seda, kui on ilmnenud mõni interaktiivsete tarbija sisselogimise meilivoo.

### <a name="standalone-web-apis"></a>Autonoomse Web API-d

Azure'i AD B2C teil on võimalus [koostada Web API, mis on turvatud OAuth 2.0 sõned abil](active-directory-b2c-apps.md#web-apis). Siiski, selle veebi-API ainult võimalik saada sõned klient, mis jagab sama rakenduse ID Koostamise veebi-API-ga, mis on mitu erinevat kliendid ei toetata.

### <a name="web-api-chains-on-behalf-of"></a>Web API ketid (-nimel-kohta)

Mitme arhitektuurides kaasata veebi-API-ga, mis tuleb kõne teise järgneval Web API, mõlemad tagatud Azure AD B2C. See stsenaarium on levinud kohalikke kliendid, mis on Veebiteenuste tagasi end, mis omakorda nõuab Microsoft Online'i teenuse, nt Azure AD Graph API.

See aheldatud Veebiteenuste stsenaarium saate toetatud OAuthi 2.0 Jwt esitaja mandaati anda, muidu tuntud voo sees-nimel-, kasutades. Siiski voo sees-nimel-, pole praegu rakendatakse Azure AD B2C.

## <a name="restriction-on-libraries-and-sdks"></a>Teekide ja SDK-d

Toetatud Microsoft teegid, mis töötavad Azure AD B2C on väga vähe sel ajal. Meil on tugi .net-i põhiste veebirakenduste ja teenuseid, kui ka NodeJS web rakendused ja teenused.  Meil on eelvaade .net-i kliendi teek tuntud MSAL, mida saab kasutada koos Azure AD B2C Windowsi ja muude rakenduste .net-i.

Meil on praegu teegi toetavad muude keelte või platvormid, sealhulgas iOS ja Android.  Kui soovite luua eri platvormi kui eespool nimetatud, soovitame avatud lähtekoodi Tarkvaraarenduskomplektist, mis meie [OAuth 2.0 ja OpenID ühenduse protokoll viite](active-directory-b2c-reference-protocols.md) vastavalt vajadusele.  Azure'i AD B2C rakendab OAuthi ja OpenID ühendus, mis võimaldab üldise OAuthi või OpenID Connect teegi kasutamiseks integreerimise.

Meie iOS-i ja Androidi Lühijuhend õpetused kasutada Azure AD B2C ühilduvust avatud lähtekoodi teekide puhul, kus oleme testinud.  Kõik meie kiire alustada õpetused on saadaval meie [Alustamine](active-directory-b2c-overview.md#getting-started) jaotises.

## <a name="restriction-on-protocols"></a>Piirangu Protokollid

Azure'i AD B2C toetab OpenID ühendamine ja OAuth 2.0. Kuid mitte kõigis funktsioonide ja võimaluste iga protokolli on rakendatud. Paremini mõista toetatud protokolli funktsioonid Azure AD B2C, lugege läbi meie [OpenID ühendamine ja OAuth 2.0 protokolli viide](active-directory-b2c-reference-protocols.md)ulatust. SAML ja oli-Fed protokolli tugi pole saadaval.

## <a name="restriction-on-tokens"></a>Märkide piirang

Mitme välja Azure AD B2C märkide rakendatakse JSON Web sõned või JWTs. Kõik JWTs (nimetatakse "nõuded") sisalduvat teavet on siiski üsna, nagu see peaks olema või on puudu. Mõned näited "sub" ja "preferred_username" nõuded.  Väärtused, vorming või taotluste muutmine aja jooksul tähenduse märkide jaoks oma olemasoleva poliitikate mõjuta – saate alati kasutada nende väärtuste tootmise rakendustes.  Kui väärtused muutuvad, anname teile võimalust neid muudatusi iga oma poliitikate konfigureerimine.  Paremini mõistmiseks praegu kiiratava Azure AD B2C teenuse märkide, lugege meie [Turbeloa viite](active-directory-b2c-reference-tokens.md)kaudu.

## <a name="restriction-on-nested-groups"></a>Pesastatud rühma piirang

Pesastatud rühmaliikmeid ei toeta Azure AD B2C rentnikke. Me ei plaani seda võimalust lisada.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Azure'i AD Graph API erinevat päringu funktsioon piirang

[Azure'i AD Graph API erinevat päringu funktsioon](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) ei toeta Azure AD B2C rentnikke. See on meie pikaajalise juhised.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Kasutajate haldamine Azure klassikaline portaalis probleemid

B2C funktsioonid on juurdepääsetav Azure portaali. Siiski saate Azure klassikaline portaali juurde pääseda rentniku muud funktsioonid, sh kasutajate haldamine. Praegu on mõned teadaolevad probleemid kasutaja halduse ( **Kasutajad** menüü) Azure klassikaline portaalis.

- Kohalik konto kasutaja (st tarbija, kes registreerub e-posti aadress ja parool, või kasutajanime ja parooli), ei vasta **Kasutajanime** väljale sisselogimise ID (e-posti aadress või kasutajanimi) kasutatud registreerumise käigus. Selle põhjuseks on kuvatud Azure klassikaline portaalis väli on tegelikult kasutaja põhisumma nimi (UPN), mis ei kasutata B2C stsenaariumid. Kohalik konto identifikaator vaatamiseks leiate kasutajaobjektis [Graphi Explorer](https://graphexplorer.cloudapp.net/). Leiate sama probleemi suhtlusvõrgu konto kasutaja (st tarbija, kes registreerub Facebooki, Google + jne), kuid sel juhul on sisselogimise identifikaatoreid rääkida.

    ![Kohaliku konto - UPN-i](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Kohalik konto kasutaja, siis ei saa redigeerida välju ja salvestada muudatused lehe vahekaardi **profiil** .

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Administraatori loodud parool lähtestada Azure klassikaline portaalis probleemid

Kui parooli lähtestamine kohaliku konto põhinev tarbija Azure klassikaline portaalis ( **Lähtesta parool** käsk vahekaardil **Kasutajad** ) tarbija ei saa muuta oma parooli järgmise sisselogimine, kui kasutate märk või poliitika, logige sisse ja välja rakenduste lukustatud. Lahendusena kasutada [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) (ilma parooli aegumise) tarbija parooli lähtestamine või sisselogimine poliitika märk asemel kasutada või poliitika sisse logida.

## <a name="issues-with-creating-a-custom-attribute"></a>Probleemid kohandatud atribuudi loomine

[Kohandatud atribuudi lisanud Azure'i portaalis](active-directory-b2c-reference-custom-attr.md) pole kohe loodud teie B2C rentniku. Peate kasutada kohandatud atribuuti vähemalt üks teie poliitika seda oma B2C rentniku luuakse ja saada Graph API kaudu.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Azure'i klassikaline portaalis domeeni kinnitamisega seotud probleemide

Praegu ei saa kinnitada domeeni edukalt [Azure klassikaline portaali](https://manage.windowsazure.com/)sisse.

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>MFA poliitika Safari brauserites koos sisselogimise probleemid

Taotluste sisselogimise poliitika (MFA sisse lülitatud) fail aeg-ajalt sisse Safari brauserites vigadega HTTP 400 (vigane päring). See on tähtaeg Safari madal küpsise mahupiirangud. On paar lahendused sellele probleemile.

- Kasutage "registreerumise või sisselogimise poliitika" asemel "sisselogimise poliitika".
- **Rakenduse nõuded** on nõutud teie poliitika arvu vähendamiseks.
