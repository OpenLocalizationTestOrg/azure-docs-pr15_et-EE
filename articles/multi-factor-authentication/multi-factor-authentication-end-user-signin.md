<properties
    pageTitle="Azure'i Azure Mitmikautentimise sisselogimiskuva MFA kogemus"
    description="Selle lehe annab teile juhised selle kohta, kust leiate Azure'i MFA saadaval sisselogimine erineval avamiseks."
    keywords="Kasutaja autentimine, sisselogimine, logige sisse mobiiltelefon, Töötelefon sisselogimine"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Azure'i Mitmikautentimise kogemus sisselogimine
> [AZURE.NOTE]  Selle lehel järgmised dokumendid kuvatakse tüüpilised sisselogimine.  Sisselogimisel abi leiate [Azure'i Mitmikautentimise probleeme](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Mida saab teie sisselogimist kogemus
Sõltuvalt sellest, kuidas sisse logida ja kasutada mitmikautentimise, erinevad kasutusvõimalusi.  Selles jaotises anname andmed, mida oodata, kui logite sisse.  Valige üks, mis kirjeldab kõige paremini, mida te teete.


Mis teed?|Kirjeldus
:------------- | :------------- |
[Mobiiltelefon või Office'i sisse logida](#signing-in-with-mobile-or-office-phone) | See on, mida võib oodata mobiiltelefon või Office'i abil sisse logida.
[Rakendusega Microsoft Authenticator teatise abil sisselogimine](#signing-in-with-the-microsoft-authenticator-app-using-notification) | See on, mida võib oodata rakendusega Microsoft Authenticator teatised.
[Rakendusega Microsoft Authenticator kinnituskood abil sisselogimine](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|See on, mida võib oodata kinnituskood Microsoft Authenticator thapp abil.
[Mõnel muul viisil sisse logida](#signing-in-with-an-alternate-method)|See näitab teile mida oodata, kui soovite kasutada muul viisil.

## <a name="signing-in-with-mobile-or-office-phone"></a>Mobiiltelefon või Office'i sisse logida

Pabermärkmikus kasutades mitmikautentimise mobiiltelefon või office kirjeldab järgmine teave.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Logige sisse oma office või mobiiltelefoni kõne abil

- Logige sisse rakenduse või teenuse, nt Office 365, kasutades oma kasutajanime ja parooliga.
- Microsoft helistab teile.

![Microsoft kõned](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Kõnele ja vajutage klahvi #.

![Answer (vastus)](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Mida peaks nüüd olema sisse logitud.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Rakendusega Microsoft Authenticator teatise abil sisselogimine

Järgmine teave kirjeldab kogemusi kasutamisel koos rakendusega Microsoft Authenticator mitmikautentimise, kui teile saadetakse teatis.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Logige sisse teatise saata rakendusega Microsoft Authenticator

- Logige sisse rakenduse või teenuse, nt Office 365, kasutades oma kasutajanime ja parooliga.
- Microsoft saadab isikule teate.

![Microsoft saadab teate](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Kõnele ja sisestusklahvi (kinnitamine).  Kui teie ettevõtte jaoks on vaja PIN-koodi küsitakse seda siin.

![Kontrollige](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Häälestamine](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Mida peaks nüüd olema sisse logitud.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Rakendusega Microsoft Authenticator kinnituskood abil sisselogimine

Järgmine teave kirjeldab kogemusi kasutamisel koos rakendusega Microsoft Authenticator mitmikautentimise, kui kasutate seda kinnituskood.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Kasutamine koos rakendusega Microsoft Authenticator kinnituskood sisse logida

- Logige sisse rakenduse või teenuse, nt Office 365, kasutades oma kasutajanime ja parooliga.
- Microsoft palub teil kinnituskood.

![Sisestage kinnituskood](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Avage telefonis rakendus Microsoft Authenticator ja sisestage väljale, kui logite sisse.

![Koodi](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Mida peaks nüüd olema sisse logitud.


## <a name="signing-in-with-an-alternate-method"></a>Mõnel muul viisil sisse logida


Järgmises jaotises näitab teile, kuidas mõnel muul viisil sisse logida, kui teie peamine meetod ei pruugi olla saadaval.

### <a name="to-sign-in-with-an-alternate-method"></a>Mõnel muul viisil sisse logida

- Logige sisse rakenduse või teenuse, nt Office 365, kasutades oma kasutajanime ja parooliga.
- Valige Kasuta mõni muu kontrollimise võimalus.  Saab valida erinevaid võimalusi Esita. Arv kuvatakse saavad aluseks olla mitu on häälestatud.

![Mõnel muul viisil kasutada](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Valige mõnel muul viisil ja logige sisse.
