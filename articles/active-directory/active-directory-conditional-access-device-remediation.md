<properties
    pageTitle="Azure Active Directory access probleemide tõrkeotsing | Microsoft Azure'i"
    description="Lugege juhiseid, et teie ettevõtte võrgus ressurssidega juurdepääsu probleemide lahendamiseks saate."
    services="active-directory"
    keywords="seadme põhineva tingimusvormingu juurdepääsu, seadme registreerimine, seadme registreerimine, seadme registreerimine ja MDM-i lubamine"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Azure Active Directory access probleemide tõrkeotsing

Juurdepääs teie asutuse SharePoint Online'i sisevõrgu ja kuvatakse tõrketeade "juurdepääs keelatud". Mis teed?

Selles artiklis antakse ülevaade parandamise juhiseid, mis aitavad teil juurdepääs teie asutuse võrguressursid probleeme lahendada.

Lahendamiseks Azure Active Directory (Azure AD) juurdepääsu probleeme, vaadake jaotist artikli, mis hõlmab oma seadme platvormi:

-   Windowsi seadmes
-   iOS-seadmes (kontrollige olukord kiiresti abi iPhone'is ja iPadis.)
-   Android-seadmes (vaadata tagasi kiiresti abi Androidi telefonide ja tahvelarvutite.)

## <a name="access-from-a-windows-device"></a>Juurdepääs Windows seadme kaudu

Kui teie seade töötab üks järgmistest platvormide, vaadake järgmistest jaotistest juurde pääseda rakenduse või teenuse katsel kuvatakse tõrketeade:

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Seade pole registreeritud

Kui teie seadet pole registreeritud Azure AD ja rakendus on kaitstud seadme põhineva poliitika, võidakse kuvada leht, kus kuvatakse üks järgmistest teadetest tõrge.

!["Te ei saa sinna siit" sõnumite registreerimata seadmete jaoks] (./media/active-directory-conditional-access-device-remediation/01.png "Stsenaarium")

Kui teie seade on domeeni ühendatud Active Directory ettevõttes, proovige järgmist.

1.  Veenduge, et logite Windows, kasutades oma töö konto (konto Active Directory).
2.  Ühenduse, virtuaalse privaatvõrgu (VPN) või DirectAccess kaudu oma ettevõtte võrku.
3.  Kui olete loonud, vajutage Windowsi logoga klahvi + klahvi L lukustamiseks Windowsi seansi.
4.  Sisestage mandaat töö konto oma Windowsi seansi avamiseks.
5.  Oodake, kuni minut ja proovige seejärel uuesti juurde pääseda rakenduse või teenuse.
6.  Kui sama lehe vaatamiseks linki **rohkem üksikasju** ja seejärel võtke ühendust oma administraatoriga üksikasjad.

Kui teie seadet pole domeeni ühendatud ja töötab Windows 10, on teil kaks võimalust:

- Käivitage Azure'i AD-ühendus
- Lisage oma töökoha või kooli kontoga Windowsi

Mille poolest erinevad nende suvandite kohta leiate teavet teemast [kasutamine Windows 10 seadmete oma töökoha](active-directory-azureadjoin-windows10-devices.md).

Azure'i AD liitumine käivitamiseks tehke järgmist platvormi teie seade töötab. (Azure'i AD-ühendus pole saadaval operatsioonisüsteemides Windows Phone'id.)

**Windows 10 tähtpäeva värskendamine**

1.  Avage rakendus **sätted** .
2.  Klõpsake üksust **kontod** > **Accessi tööl või koolis**.
3.  Klõpsake nuppu **Loo ühendus**.
4.  Klõpsake **selle seadme Azure AD liituda**.
5.  Ettevõtte autentida, esitada mitmikautentimise, kui kuvatakse vastav viip, ja seejärel järgige juhiseid, mis kuvatakse.
6.  Logige välja ja seejärel logige sisse oma töö.
7.  Rakenduse avamiseks, proovige uuesti.


**Windows 10 November 2015 värskendus**

1.  Avage rakendus **sätted** .
2.  Valige **süsteem** > **kohta**.
3.  Klõpsake nuppu **Liitu Azure AD**.
4.  Ettevõtte autentida, esitada mitmikautentimise, kui kuvatakse vastav viip, ja seejärel järgige juhiseid, mis kuvatakse.
5.  Logige välja ja seejärel logige sisse oma töö konto (konto Azure AD).
6.  Rakenduse avamiseks, proovige uuesti.

Oma töökoha või kooli konto lisamiseks tehke järgmist.

**Windows 10 tähtpäeva värskendamine**

1.  Avage rakendus **sätted** .
2.  Klõpsake üksust **kontod** > **Accessi tööl või koolis**.
3.  Klõpsake nuppu **Loo ühendus**.
4.  Ettevõtte autentida, esitada mitmikautentimise, kui kuvatakse vastav viip, ja seejärel järgige juhiseid, mis kuvatakse.
5.  Rakenduse avamiseks, proovige uuesti.


**Windows 10 November 2015 värskendus**

1.  Avage rakendus **sätted** .
2.  Klõpsake üksust **kontod** > **kontod**.
3.  Klõpsake nuppu **Lisage töökoha või kooli kontoga**.
4.  Ettevõtte autentida, esitada mitmikautentimise, kui kuvatakse vastav viip, ja seejärel järgige juhiseid, mis kuvatakse.
5.  Rakenduse avamiseks, proovige uuesti.

Kui teie seadet pole domeeni ühendatud ja töötab Windows 8.1 teha töökoha liitmine ja registreeruda Microsoft Intune'i, tehke järgmist.

1.  Avage **arvuti sätteid**.
2.  Klõpsake nuppu **võrk** > **töökoha**.
3.  Klõpsake nuppu **Liitu**.
4.  Ettevõtte autentida, esitada mitmikautentimise, kui kuvatakse vastav viip, ja seejärel järgige juhiseid, mis kuvatakse.
5.  Klõpsake **sisse lülitada**.
6.  Rakenduse avamiseks, proovige uuesti.


### <a name="browser-is-not-supported"></a>Brauser ei toeta

Teil võib keelatud juurdepääsu, kui üritate juurde pääseda rakenduse või teenuse, kasutades ühte järgmistes brauserites:

- Chrome'i, Firefoxi või mõni muu brauser peale Microsoft Edge või Microsoft Internet Exploreri operatsioonisüsteemis Windows 10 või Windows Server 2016
- Firefox Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 või Windows Server 2008 R2

Kuvatakse tõrge leht, mis näeb välja umbes järgmine:

!["Te ei saa sinna siit" sõnum pole toetatud brauserid] (./media/active-directory-conditional-access-device-remediation/02.png "Stsenaarium")

Ainult puhastamiseks on kasutada rakenduse teie seadme platvorm toetavates brauserites.

## <a name="next-steps"></a>Järgmised sammud

[Azure Active Directory juurdepääsu](active-directory-conditional-access.md)
