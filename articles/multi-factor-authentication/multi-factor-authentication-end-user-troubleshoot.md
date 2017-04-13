<properties
    pageTitle="Kaheastmelise kontrollimise tõrkeotsing | Microsoft Azure'i"
    description="Selle dokumendi teavet kasutajate kohta, mida teha, kui nad tekib probleeme Azure'i Mitmikautentimise."
    services="multi-factor-authentication"
    keywords = "multifactor autentimise klient, autentimine probleem, korrelatsiooni ID"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="having-trouble-with-two-step-verification"></a>Kas teil on probleeme kaheastmelise kontrollimise

Selles artiklis käsitletakse mõned probleemid, mis võivad ilmneda kaheastmelise kontrollimise. Kui teil on probleem ei kuulu, esitage üksikasjalik tagasiside jaotisse kommentaarid nii, et saaksime.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Kaotsi läinud telefoni või varguse

On kaks võimalust naasmiseks oma konto. Esimene on alternatiivse autentimise telefoninumbri abil sisse logida, kui olete loonud ühte. Paluge oma administraatoril tühjendage sätete on teine.

Kui teie telefoni kadunud või varastatud, soovitame ka, et teil on rakenduse paroole lähtestada administraator ja tühjendage ruut mis tahes meeles pidada seadmed. Kui teie administraator pole kindel, kuidas täita seda, osutage neid käesolevas artiklis: [haldamine kasutajatele ja seadmetele](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Alternatiivne telefoninumber kasutamine

Kui olete häälestanud mitu kinnitamise suvandid, sh teisene telefoninumber või klõpsake mõne muu seadme autentikaatorirakenduse kaudu saate ühe neist sisse logida.

Alternatiivne telefoninumber abil sisse logida, tehke järgmist.

1. Logige sisse nagu tavaliselt.
2. Kui teil palutakse täpsemaks oma konto kinnitada, valige **Kasuta mõni muu kontrollimise võimalus**.

    ![Erinevate kontrollimine](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Valige soovitud telefoninumber, millele teil on juurdepääs.

    ![Alternatiivne telefoninumber](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Kui olete uuesti sisse oma konto, [saate hallata oma sätteid](multi-factor-authentication-end-user-manage-settings.md) muuta autentimise telefoninumbri.

>[AZURE.IMPORTANT]
>See on oluline telefoninumbri teisene autentimise konfigureerimine. Kui teie peamine telefoninumber ja oma mobiilirakenduse on sama telefoni, teil on vaja ka kolmas valik telefoni on varguse või.

### <a name="clear-your-settings"></a>Tühjendage sätete

Kui teil on konfigureeritud teisene autentimise telefoninumbri, siis on teil pöörduge abi saamiseks oma administraatori poole. Lasta tühjendage oma sätteid nii, et järgmine kord, kui logite, teil palutakse luua [oma konto](multi-factor-authentication-end-user-first-time.md) uuesti.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Ma olen saanud teksti või kõne telefonis

On mitu põhjust, miks proovite sisse logida, kuid ei saa teksti või telefonikõne ajal. Kui olete edukalt saanud tekstide või telefonikõnede oma telefoni varem, siis seda tõenäoliselt probleemi telefoni pakkujaga pole teie kontoga. Veenduge, et teil on hea lahtri signaal, ja kui soovite saada tekstsõnumeid, veenduge, et teie telefoni ja teenuse leping toetavad tekstsõnumeid.

Kui olete oodanud minuti teksti või kõne, on kõige kiiremini tuua oma kontole proovida muu valik.

1. Valige suvand **Kasuta mõni muu kontrollimise võimalus** lehel, mis ootab oma kinnitamine.

    ![Erinevate kontrollimine](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Valige telefoni arv või kohaletoimetamise meetod, mida soovite kasutada.

    Kui olete saanud mitu kinnitamine koodi, toimib ainult üks uusim.

Kui teil pole konfigureeritud mõne muu meetodi, pöörduge oma administraatori poole ja paluge tal tühjendage sätete. Järgmine kord, kui logite, küsitakse [mitmikautentimise](multi-factor-authentication-end-user-first-time.md) häälestada uuesti.


Kui teil on sageli halb lahtri signaal tingitud viivitused, soovitame [Microsoft autentimisrakenduse](multi-factor-authentication-microsoft-authenticator.md) kasutada oma nutitelefoni. Rakenduse saate luua juhuslik turbekoodi, mis te saate sisse logida ja koodid ei nõua lahtri signaal või Interneti-ühendus.


## <a name="app-passwords-are-not-working"></a>Rakenduse paroolid ei tööta

Esmalt veenduge, kas sisestasite rakenduse parooli õigesti.  Kui see on endiselt ei õnnestu, proovige sisse logida ja [luua uue rakenduse parooli](multi-factor-authentication-end-user-app-passwords.md).  Kui see ei tööta, pöörduge oma administraatori poole ja neid [kustutada oma rakenduse parooliga](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) ja seejärel saate luua uue.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ma ei leia vastust minu probleem.

Kui olete proovinud tõrkeotsingujuhiseid, kuid on endiselt probleeme, pöörduge oma administraatori poole või isik, kes teile mitmikautentimise häälestamine. Peaks olema aidata teil.

Lisaks saate postitada [Azure AD Foorumid](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) või [tugiteenuste](https://support.microsoft.com/contactus) küsimus ja me teile vastata teie probleemi, niipea, kui võimalik.

Kui tugiteenuste, sisaldavad järgmist teavet:

- **Kasutaja ID** -mis on e-posti aadress proovisite sisse logida?
- **Üldise kirjelduse viga** – mis täpne tõrketeade, kas teie näha?  Kui tõrketeadet ei kuvata, on kirjeldatud käitu, märkasite üksikasjad.
- **Lehe** -lehe, mis olid teil kui teile kuvati see tõrge (lisada URL-i)?
- **Tõrkekood** - on saabunud konkreetse tõrkekoodi.
- **SeansiId** - teatud seansi id on saabunud.
- **Korrelatsiooni ID** – mis on loodud, kui kasutaja kuvati see tõrge korrelatsiooni id koodi.
- **Ajatempli** – mis on täpsed kuupäeva ja kellaaja, mis teile kuvati see tõrge (sisaldada soovitud ajavöönd)?

Suur osa selle teabe leiate oma lehele. Kui ei kontrollida, kas teie sisselogimine ajal, valige **Kuva üksikasjad**.

![Tõrke üksikasjade sisselogimine](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Need andmed aitab teie probleemi lahendamiseks nii kiiresti kui võimalik.

## <a name="related-topics"></a>Seotud teemad
- [Kaheastmelise kontrollimise sätete haldamine](multi-factor-authentication-end-user-manage-settings.md)  
- [Microsoft Authenticator rakenduse FAQ](multi-factor-authentication-app-faq.md)
