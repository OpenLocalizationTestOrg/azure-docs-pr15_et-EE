<properties
    pageTitle="Omakliendi rakenduste puhverserveri rakendustega avaldamise lubamine | Microsoft Azure'i"
    description="Hõlmab omakliendi rakenduste Azure AD Rakenduse puhverserveri konnektor turvalist remote juurdepääsu oma kohapealse rakendusi suhelda lubamise kohta."
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

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Kuidas lubada omakliendi rakendused suhelda puhverserveri rakendused

Azure Active Directory rakenduse puhverserveri levinud avaldada brauseri rakendusi nagu SharePointi, Outlook Web Accessi ja kohandatud ärirakenduste joon. See saab avaldada omakliendi rakendusi, mis erinevad veebirakenduste, kuna need seadmesse installitud. Selleks toetavad Azure AD välja sõned standard lubada HTTP päised saadetud.

![Lõppkasutajad, Azure Active Directory ja avaldatud rakenduste seos](./media/active-directory-application-proxy-native-client/richclientflow.png)

Avaldada selliseid rakendusi soovitatav meetod on kasutada Azure AD Authentication Library, mis hoolitseb autentimise vaeva ja palju muu kliendi keskkonnas toetab. [Algne rakendus Veebiteenuste stsenaarium](active-directory-authentication-scenarios.md#native-application-to-web-api)sobib rakenduse puhverserver. Muul viisil see protsess on järgmine:

## <a name="step-1-publish-your-application"></a>Samm 1: Avaldamine rakenduse

Avaldada oma rakenduse puhverserveri muid tavapärasel viisil, määrata kasutajatele ja anda neile premium või lihtsa litsentsid. Lisateabe saamiseks vaadake teemat [rakenduse puhverserveri avalda rakendusi](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Samm 2: Konfigureerimine rakenduse

Konfigureerige oma kohalikus rakenduses järgmiselt:

1. Azure'i klassikaline portaali sisse logida.
2. Valige vasakpoolsest menüüst Active Directory ikoon ja seejärel valige kataloogi.
3. Klõpsake menüü top **rakendused**. Kui pole rakendused on lisatud kataloogi, kuvatakse selle lehe ainult linki **Lisa rakendus** . Klõpsake linki või teise võimalusena võite klõpsata käsuribal nuppu **Lisa** .
4. Klõpsake lehel **soovitud teha** linki **minu ettevõte on arendamise rakenduse lisamine**.
5. Klõpsake lehel **meile rakenduse kohta** Määrake oma rakenduse nimi ja valige **kohalikke klientrakendusega**. Klõpsake noolt ikooni jätkata.
6. Lehel **teenuserakenduse teabe** **Ümber suunata URI** ette omakliendi rakendus ja seejärel märke lõpuleviimiseks klõpsake nuppu.

Rakenduse on lisatud, ja siis võetakse lehele Kiirkäivituse rakenduse.

## <a name="step-3-grant-access-to-other-applications"></a>Samm 3: Muude rakenduste juurdepääsu andmine

Kohalikus rakenduses puutuda muude rakenduste kataloogi lubamiseks tehke järgmist.

1. Menüü top käsku **rakendused**, valige Uus rakendus ja klõpsake nuppu **Konfigureeri**.
2. Liikuge kerides jaotiseni **muude rakenduste õigused** . Klõpsake nuppu **Lisa rakenduse** valige puhverserveri rakendus, mida soovite anda juurdepääsu kohalikus rakenduses ja seejärel klõpsake märkeruutu alumises paremas nurgas. Valige rippmenüüst **Delegeeritud õiguste** uue õigusetaseme.

![Muude rakenduste kuvatõmmis - õiguste lisamiseks rakenduse](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Samm 4: Redigeerimine Active Directory Authentication Library

Kohalikus rakenduses koodi autentimine kontekstis, on Active Directory autentimise Raamatukogu (ADAL) kaasata järgmise redigeerimiseks tehke järgmist.

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

Muutujate asendatakse järgmiselt:

- **TenantId** leiate kohe pärast "/ Directory /" GUID rakenduse **konfigureerimise** lehe URL.
- **Frontend URL** on ees teie sisestatud rakenduse puhverserveri ja leiate rakenduse puhverserveri **konfigureerimise** lehel.
- **Kliendi Id** kohalikke rakenduse leiate rakendus lehel **konfigureerimine** .
- **Suunake URI kohalikke rakenduse** leiate rakendus lehel **konfigureerimine** .

![Uue kohalikus rakenduses konfigureerimine lehe kuvatõmmis](./media/active-directory-application-proxy-native-client/new_native_app.png)

Kohalikus rakenduses voo kohta leiate lisateavet teemast [Algne rakendus veebi-API](active-directory-authentication-scenarios.md#native-application-to-web-api).


## <a name="see-also"></a>Vt ka

- [Avaldada oma domeeninime kasutavad rakendused](active-directory-application-proxy-custom-domains.md)
- [Tingimusvormingu juurdepääsu lubamine](active-directory-application-proxy-conditional-access.md)
- [Nõuded rakendusi töötamine](active-directory-application-proxy-claims-aware-apps.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)
