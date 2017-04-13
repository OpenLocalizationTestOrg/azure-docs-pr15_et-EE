<properties
    pageTitle="Kaheastmelise kontrollimise oma töökoha või kooli konto häälestamine"
    description="Kui teie ettevõte konfigureerib Azure'i Mitmikautentimise, palutakse teil kaheastmelise kontrollimise kasutajaks. Saate teada, kuidas saate selle häälestada. "
    services="multi-factor-authentication"
    keywords="Kuidas kasutada kataloogi azure active directory pilves, active directory õpetus"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>Minu konto kaheastmelise kontrollimise häälestamine

Kaheastmelise kontrollimise on suurema turvalisuse tagamiseks toimingut, mis aitab kaitsta teie kontot, raskem teistele murda. Kui selle artikli lugemist teil ilmselt e-posti kaudu oma töökoha või kooli administraatori Mitmikautentimise kohta. Või äkki proovisin sisse logida ja sain sõnumi palub teil luua täiendavaid turvalisus kinnitamine. Kui see on juhul **ei saa sisse logida, kuni olete kõik automaatne-registreerimise käigus**.

See artikkel aitab teil häälestada oma **töö- või koolikontoga**. Kui soovite lubada kaheastmelise kontrollimise jaoks oma isikliku Microsofti kontoga, lugege teemat [kaheastmelise kontrollimise kohta](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Määrata, kuidas te kasutate mitmikautentimise

Kaheastmelise kontrollimise töötab küsides kahe andmeühikuga ID, kui logite sisse. Esmalt palume oma kasutajanime ja parooli nagu tavaliselt. Klõpsake kontakti telefoni, et saaksime leida kuulub, ja te kinnitame, et sisselogimise katse oli õigustatud.  

Alustamine seadistamisel, proovige oma kontosse sisse logida, nagu tavaliselt. Kui teie administraator on teie konto kaheastmelise kontrollimise jaoks konfigureeritud, palutakse teil alustamiseks automaatseks registreerimiseks. Alustamiseks klõpsake **nüüd häälestamine.**

![Häälestamine](./media/multi-factor-authentication-end-user-first-time/first.png)

Registreerimise käigus esimene küsimus on, kuidas soovite meil teiega ühendust võtta. Heitke pilk suvandi tabeli ja minge häälestamise juhiseid iga meetodi linkide abil.

| Kontaktmeetod | Kirjeldus |
| --- | --- |
[Mobiilirakenduse kaudu](#use-a-mobile-app-as-the-contact-method) | - **Teatiste kinnitamiseks.** See suvand sunnib teatise Autentija rakendus oma nutitelefoni või tahvelarvuti. Teate kuvamine ja kui see on õigustatud, valige **autentimise** rakenduses. Oma töö või kooli nõuda enne autenditakse te sisestage PIN-koodi.<br>- **Kasutage kinnituskood.** Selles režiimis loob soovitud autentimisrakenduse kinnituskood, mis värskendab iga 30 sekundi järel. Sisestage sisselogimise kasutajaliidese uusimad kinnituskood.<br>Rakendusega Microsoft Authenticator on saadaval [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Androidi](http://go.microsoft.com/fwlink/?Linkid=825072)ja [iOS-i](http://go.microsoft.com/fwlink/?Linkid=825073)jaoks. |
[Mobiiltelefoni kõne või teksti](#use-your-mobile-phone-as-the-contact-method) | - **Telefonikõne** paigutab automatiseeritud tavakõne teie telefoninumber. Vastake kõnele ja # vajutamine klahvistiku autentida.<br>- **Tekstsõnumi** lõpeb kinnituskoodi sisaldava teksti sõnumi. Pärast teksti kuvatakse vastav viip, tekstsõnumiga vastamine või sisestada kinnituskood ette kasutajaliidese Logi sisse. |  
[Office'i telefonikõne](#use-your-office-phone-as-the-contact-method) | Paigutab automatiseeritud tavakõne teie telefoninumber. Vastake kõnele ja ajal # nii nagu klahvistiku autentida. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Kontakti meetodit kasutada mobiilirakenduse kaudu

Selle meetodi kasutamise jaoks on vaja installida autentikaatorirakenduse kaudu oma telefonis või tahvelarvutis. Selles artiklis toodud juhiseid põhinevad rakendusega Microsoft Authenticator, mis on saadaval [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Androidi](http://go.microsoft.com/fwlink/?Linkid=825072)ja [iOS-i](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Valige ripploendist soovitud **mobiilirakenduse kaudu** .
2. Valige **kinnitamiseks teatiste** või **Kasuta kinnituskood**ja valige **häälestamine**.

    ![Täiendavad turvalisus kontrollimine](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Telefonis või tahvelarvutis, avage rakendus ja valige **+** konto lisamine. (Android-seadmetes, valige kolm punkti, seejärel **Lisa konto**.)
4. Määrake, kas soovite lisada töökoha või kooli kontoga. Avaneb QR koodi skanneri telefonis. Kui kaamera ei tööta korralikult, saate valida oma ettevõtte andmete käsitsi sisestamine. Lisateavet leiate teemast [konto käsitsi lisamine](#add-an-account-manually).  
5. Skannige QR koodi pilt, mis kuvati ekraanil konfigureerida mobiilirakenduse kaudu.  Valige **valmis** QR koodi Kuva sulgemiseks.  

    ![Kuva QR kood](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. Aktiveerimine telefoni teel lõpulejõudmisel valige **minuga ühendust võtta**.  Selles etapis tuleb telefoni saadab teavitus- või kinnituskood. Valige **kinnitamine**.  
7. Kui teie ettevõtte jaoks on vaja kinnitamise sisselogimise kinnitamine PIN-koodi, sisestage see.

    ![PIN-koodi sisestamiseks väljale](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Pärast PIN-kood on lõpule jõudnud, valige **Sule**. Selles etapis oma kinnitamine peaks olema eduka.
9. Soovitame, et sisestate oma mobiiltelefoninumber juhuks, kui kaotate juurdepääsu oma mobiilirakenduse kaudu. Määrake oma riigis rippmenüü loendist ja sisestage oma mobiiltelefoninumber riigi nime kõrval olev ruut. Valige **edasi**.
10. Selles etapis küsitakse rakenduse paroolid-brauseri rakendused, nt Outlook 2010 või vanemad või Apple'i seadmete rakenduse algset e-posti häälestamine. Seda sellepärast, et mõned rakendused ei toeta kaheastmelise kontrollimise. Kui te ei kasuta need rakendused, klõpsake nuppu **valmis** ja jätke järgmised juhised.
11. Nende rakenduste kasutamisel kopeerimine rakenduse parool esitatud ja kleepige see oma rakenduse tavalise parooli asemel. Saate mitu rakendused rakendus sama parool. Lisateabe saamiseks, [abi rakenduse paroolid].
12. Klõpsake nuppu **valmis**.


### <a name="add-an-account-manually"></a>Konto lisamine käsitsi
Kui soovite konto lisamine rakendusse käsitsi asemel QR lugeja, tehke järgmist.

1. Klõpsake nuppu **konto käsitsi sisestada** .  
2. Sisestage kood ja URL, mis on esitatud samal lehel, kus näidatakse vöötkoodi. See teave väljadele **kood** ja **URL-i** läheb mobiilirakenduse kaudu.

    ![Häälestamine](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Kui aktiveerimine on lõpule jõudnud, valige **minuga ühendust võtta**. Selles etapis tuleb telefoni saadab teavitus- või kinnituskood. Valige **kinnitamine**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Helistage oma mobiiltelefonilt kontakti meetod

1. Valige loendist drop-down **Autentimist telefon** .  

    ![Häälestamine](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. Valige oma riik rippmenüü loendist ja sisestage oma mobiiltelefoninumber.
3. Valige eelistate kasutada teie mobiiltelefoni - teksti või kõne meetod.
4. Valige **minuga ühendust võtta** , et kontrollida oma telefoni number. Sõltuvalt teie valitud, saate saata või helistada. Järgige ekraanil esitatud juhiseid ja seejärel valige **Kinnita**.
5. Selles etapis küsitakse rakenduse paroolid-brauseri rakendused, nt Outlook 2010 või vanemad või Apple'i seadmete rakenduse algset e-posti häälestamine. Seda sellepärast, et mõned rakendused ei toeta kaheastmelise kontrollimise. Kui te ei kasuta need rakendused, klõpsake nuppu **valmis** ja jätke järgmised juhised.
6. Nende rakenduste kasutamisel kopeerimine rakenduse parool esitatud ja kleepige see oma rakenduse tavalise parooli asemel. Saate mitu rakendused rakendus sama parool. Lisateabe saamiseks, [abi rakenduse paroolid].
7. Klõpsake nuppu **valmis**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Kasutage telefonis office kontakti meetod

1. Valige ripploendist soovitud **Töötelefoninumber**  

    ![Häälestamine](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. Telefoni number väljale sisestatakse automaatselt teie ettevõtte kontaktteave. Kui arv on vale või puudub, paluge oma administraatoril muudatusi teha.
4. Valige **minuga** kinnitamaks, et teie telefoninumber ja me kutsume oma number. Järgige ekraanil esitatud juhiseid ja seejärel valige **Kinnita**.
5. Selles etapis küsitakse rakenduse paroolid-brauseri rakendused, nt Outlook 2010 või vanemad või Apple'i seadmete rakenduse algset e-posti häälestamine. Seda sellepärast, et mõned rakendused ei toeta kaheastmelise kontrollimise. Kui te ei kasuta need rakendused, klõpsake nuppu **valmis** ja jätke järgmised juhised.
6. Nende rakenduste kasutamisel kopeerimine rakenduse parool esitatud ja kleepige see oma rakenduse tavalise parooli asemel. Saate mitu rakendused rakendus sama parool. Lisateavet leiate teemast [mis on rakenduse paroolid](multi-factor-authentication-end-user-app-passwords.md).
7. Klõpsake nuppu **valmis**.

## <a name="next-steps"></a>Järgmised sammud

- Saate muuta oma eelistatud suvandid ja [kaheastmelise kontrollimise sätete haldamine](multi-factor-authentication-end-user-manage-settings.md)
- Saate häälestada [Rakenduse paroolid](multi-factor-authentication-end-user-app-passwords.md) kohalikke seadme rakendused, mis ei toeta kaheastmelise kontrollimise jaoks.
- Tutvuge [Microsoft autentimisrakenduse](multi-factor-authentication-microsoft-authenticator.md) kiire ja turvalise autentimise isegi siis, kui teil pole teenuse lahtrisse.
