<properties
    pageTitle="Rakendusega Microsoft Authenticator mobiiltelefonid | Microsoft Azure'i"
    description="Saate teada, kuidas võtta kasutusele Azure'i Autentija uusim versioon."
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

# <a name="microsoft-authenticator"></a>Microsoft Authenticator

Rakendusega Microsoft Authenticator pakub väärtpaberi Azure'i konto (nt bsimon@contoso.onmicrosoft.com), oma kohapealse töökonto (näiteks bsimon@contoso.com), või Microsofti kontot (näiteks bsimon@outlook.com).

Rakendus töötab kahel viisil:

- **Teatis**. Rakendus aitab volitamata juurdepääsu vältimiseks kontod ja Lõpeta praktilise vajutamine teate oma nutitelefoni või tahvelarvuti. Lihtsalt teatist vaadata ja kui see on õigustatud, valige **Kinnita**. Muul juhul valige **keeldu**. Keelamisega seotud teatiste kohta leiate teavet teemast Mitmikautentimise Keela ja aruande pettuse funktsioonide kasutamiseks.

- **Parooli kinnituskood**. Rakenduse saab kasutada tarkvara märgiks luua mõne OAuthi kinnituskood. Sisestage tähis, mis on esitatud rakenduse abil sisselogimise ekraani koos kasutajanimi ja parool, kui seda küsitakse. Kinnituskood pakub teise liiki autentimine.

Koos rakendusega Microsoft Authenticator vabastamist, asendatakse vana Azure'i autentimisrakenduse.  Azure'i autentimisrakenduse tööd jätkata, kuid kui otsustate uue rakendusega Microsoft Authenticator liikumiseks, artiklit aitavad teil.  

## <a name="install-the-app"></a>Installige rakendus

Rakendusega Microsoft Authenticator on saadaval [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Androidi](http://go.microsoft.com/fwlink/?Linkid=825072)ja [iOS-i](http://go.microsoft.com/fwlink/?Linkid=825073)jaoks.

## <a name="add-accounts-to-the-app"></a>Konto lisamine rakendusse

Iga konto, mille soovite lisada rakendusse Microsoft Authenticator, kasutage ühte järgmistest toimingutest.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Konto lisamine rakendusse QR koodi skanneri abil

1. Minge turvalisus kinnitamine sätete kuva.  Kuidas see kuvale leiate teemast [oma turvasätete muutmine](multi-factor-authentication-end-user-manage-settings.md).

2. Valige **konfigureerimine**.

    ![Kuval turvalisus kontrollimise sätted nupp konfigureerimine](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Kuvatakse tabeliveergude QR koodi seda ekraani.

    ![Kuva, mis pakub QR kood](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Avage Microsofti autentimisrakenduse. Valige kuval **kontod** **+**, ja seejärel määrake, kas soovite lisada töökoha või kooli kontoga.

    ![Plussmärk kuval kontod](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Kuva määratlemiseks töökoha või kooli kontoga](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Kaamera abil krediitkaardita ja seejärel valige **valmis** QR koodi Kuva sulgemiseks.

    ![Kuva skannimiseks QR kood](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Kui kaamera ei tööta korralikult, saate sisestada QR koodi ja URL-i käsitsi. Lisateabe saamiseks lugege teemat [rakenduse konto käsitsi lisamine](#add-an-account-to-the-app-manually).

5. Oodake, kuni aktiveeritakse konto. Pärast aktiveerimise lõppemist valige **minuga ühendust võtta**.  See saadab teavitus- või kinnituskood telefoni.  Valige **kinnitamine**.

    ![Kuva, kus saate valida kinnitamine sisse logida](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Kui teie ettevõtte vajadustele kinnitamise kontrollimise sisselogimise PIN-koodi, sisestage see.

    ![PIN-koodi sisestamiseks väljale](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. Pärast PIN-kood on lõpule jõudnud, valige **Sule**. Selles etapis tuleb oma kinnitamine eduka.
8. Soovitame, et sisestate oma mobiiltelefoninumber juhuks, kui te kaotate Accessi rakenduse. Määrake oma riigis rippmenüü loendist ja sisestage oma mobiiltelefoninumber riigi nime kõrval olev ruut. Valige **edasi**.
9. Selles etapis on häälestada oma kontakti meetod. Nüüd on aeg luua rakenduse paroolid-brauseri rakendused, nt Outlook 2010 või vanemad. Kui te ei kasuta need rakendused, valige **valmis**. Muul juhul jätkake järgmise juhisega.

    ![Kuva rakenduse parooli loomise kohta](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. -Brauseri rakenduste kasutamisel kopeerida esitatud rakenduse parool ja kleepige oma rakenduste parool. Juhised üksikute rakendused, nt Outlooki ja Lynci kohta leiate artiklist Kuidas muuta rakenduse parool oma meilikonto parool ja parooli oma rakenduse app parooli muutmine.
11. Valige **valmis**.

Nüüd peaks nähtaval uue konto, Valige kuval **kontod** .

![Kuval kontod](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Konto lisamine rakendusse käsitsi

1. Minge turvalisus kinnitamine sätete kuva.  Kuidas see kuvale leiate teemast [oma turvasätete muutmine](multi-factor-authentication-end-user-manage-settings.md).

2. Valige **konfigureerimine**.

    ![Kuval turvalisus kontrollimise sätted nupp konfigureerimine](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Kuvatakse tabeliveergude QR koodi seda ekraani.  Pange tähele, et kood ja URL-i.

    ![Ekraanil, mida pakub QR koodi ja URL-i](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Avage Microsofti autentimisrakenduse. Valige kuval **kontod** **+**, ja seejärel määrake, kas soovite lisada töökoha või kooli kontoga.

    ![Plussmärk kuval kontod](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Kuva määratlemiseks töökoha või kooli kontoga](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Valige skanner, **Sisestage kood käsitsi**.

    ![Kuva skannimiseks QR kood](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Sisestage kood ja URL-i rakenduse vastavad märkeruudud.

    ![Kuva kood ja URL-i sisestamine](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Kuva kood ja URL-i sisestamine](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Oodake, kuni aktiveeritakse konto. Pärast aktiveerimist lõppemist valige **minuga ühendust võtta**. See saadab teavitus- või kinnituskood telefoni. Valige **kinnitamine**.

Nüüd peaks nähtaval uue konto, Valige kuval **kontod** .

![Kuval kontod](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Konto lisamine rakendusse Touch ID abil

IOS rakendusega Microsoft Authenticator toetab Touch ID-ga.  Azure'i Mitmikautentimise võimaldab asutustel vaja PIN-koodi seadmete jaoks. Puutefunktsioonide ID-ga iOS-i kasutajad pole vaja PIN-koodi. Selle asemel saate need skannida oma sõrmejälje ja valige **Kinnita**.

Häälestamise Microsoft Authenticator touchis ID on lihtne. Tavaline kinnitamine ülesanne PIN-koodiga lõpuleviimine Kui teie seade toetab Touch ID, Microsoft Authenticator kuvatakse see automaatselt häälestada konto jaoks.

![Touch ID seadistuse kinnitamine](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Et alates, kui olete peab teie sisselogimine kinnitamine, valige vastuvõetud tõuketeatised teatis ja skannida oma PIN-koodi sisestamise asemel sõrmejälge.

![Tõuketeatised teatis](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>Vana Azure'i autentimise rakendus desinstallida

Kui olete lisanud kõik kontod uue rakenduse, saate desinstallida vana rakendust oma telefoni kaudu.

## <a name="delete-an-account"></a>Konto kustutamine

Rakendusega Microsoft Authenticator konto eemaldamiseks valige konto ja seejärel valige **Kustuta**.

![Nupp Kustuta](./media/multi-factor-authentication-azure-authenticator/remove.png)
