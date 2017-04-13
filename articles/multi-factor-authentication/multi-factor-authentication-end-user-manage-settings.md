<properties
    pageTitle="Kaheastmelise kontrollimise sätete haldamine | Microsoft Azure'i"
    description="Hallata, sh oma kontaktteabe muutmine või konfigureerida seadmetes Azure'i Mitmikautentimise kasutamise."
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

# <a name="manage-your-settings-for-two-step-verification"></a>Kaheastmelise kontrollimise sätete haldamine

Sellest artiklist leiate vastused küsimustele selle kohta, kuidas värskendada sätete kontrollimine või mitme teguri kaheastmelise autentimise. Kui teil on probleeme oma kontosse sisse logida, vaadake [kas teil on probleeme kaheastmelise kontrollimise](multi-factor-authentication-end-user-troubleshoot.md) tõrkeotsingu kohta abi.


## <a name="where-to-find-the-settings-page"></a>Kust leida lehel sätted
Sõltuvalt sellest, kuidas häälestada oma ettevõtte Azure'i Mitmikautentimise, on mõned kohad, kus saate muuta oma sätteid, nt teie telefoninumber.

Kui teie IT-administraator saadetud teatud URL-i või juhiseid haldamine kaheastmelise kontrollimise, tehke järgmist. Muul juhul teha kõik teised järgmised juhised. Kui järgige neid juhiseid, kuid ei näe samad suvandid, tähendab see, et teie töö või kooli kohandada oma portaalis. Paluge oma administraatoril lingi oma Azure'i Mitmikautentimise portaali.


1. Logige sisse [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Valige kohal **profiil**.  
3. Valige **täiendavad turvalisus kinnitamine**.  

    ![Minu rakendused](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Suurema turvalisuse tagamiseks kontrollimise lehe laadimise teie sätted.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Soovin muuta oma telefoni number või sekundaarne numbri lisamine

See on oluline telefoninumbri teisene autentimise konfigureerimine.  Kuna esmane telefoninumbri ja oma mobiilirakenduse on tõenäoliselt sama telefoni, teisene telefoninumber on ainus viis on võimalik saada tagasi oma kontole telefoni on varguse või.

> [AZURE.NOTE]
> Kui ei on juurdepääs teie peamine telefoninumber ja vajate abi saada sisse oma konto, lugege meie spikriteemadest [probleeme kaheastmelise kontrollimise](multi-factor-authentication-end-user-troubleshoot.md).

**Esmane telefoninumbri muutmiseks tehke järgmist.**  

1. Täiendava kinnitamise lehel, valige tekstiväljal praeguse telefoninumbri ja uue telefoninumbri seda redigeerida.  
2. Valige **Salvesta**.  
3. Kui see on arv, mida kasutate oma eelistatud kontrollimise valik, tuleb kontrollida uus number, enne kui saate selle salvestada.  


**Teisene telefoninumbri lisamiseks tehke järgmist.**  

1. Täiendava kinnitamise lehel, märkige ruut kõrval **alternatiivse autentimise telefon.**  
2. Sisestage tekstiväljale teisene telefoni number.  
3. Valige **Salvesta** ja teie muudatused on lõpule jõudnud.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Kuidas puhastada Microsoft Authenticator vana seadmest ja teisaldamine teise?
Kui rakendus desinstallida teie seadmest või Lähtestage seade, eemaldage aktiveerimise tagasi lõpuks. Kasutage [teisaldamise uude seadmesse](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app)kirjeldatud juhised.

## <a name="next-steps"></a>Järgmised sammud
- Tõrkeotsingu näpunäited ja aidake [probleeme kaheastmelise kontrollimise](multi-factor-authentication-end-user-troubleshoot.md) kohta
- Saate häälestada [Rakenduse paroolid](multi-factor-authentication-end-user-app-passwords.md) rakendused, mis ei toeta kaheastmelise kontrollimise jaoks.
