<properties
    pageTitle="Saada alustamine Azure mitmekordne Auth pakkuja | Microsoft Azure'i"
    description="Saate teada, kuidas luua Azure mitmekordne Auth pakkuja."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Alustamine: Azure'i mitmekordne Auth pakkuja
Kaheastmelise kontrollimise on vaikimisi globaalne administraatoritele, kes on Azure Active Directory ja Office 365 kasutajatele saadaval. Aga kui soovite [täpsemate](multi-factor-authentication-whats-next.md) funktsioonide siis peaks osta täisversiooni Azure'i mitme teguriga autentimine (MFA).

> [AZURE.NOTE]  Azure'i mitmekordne Auth pakkuja kasutatakse ära võimalused Azure'i MFA täisversiooni. Kasutajate jaoks on kes **ei saa litsentse Azure'i MFA, Azure AD Premium, või EMS kaudu**.  Azure'i MFA, Azure AD Premium ja EMS kaasata täisversiooni Azure'i MFA vaikimisi.  Kui teil on litsentse, siis pole vaja Azure mitmekordne Auth pakkuja.

Azure'i mitmekordne Auth osutaja on vajalik SDK alla laadida.

> [AZURE.IMPORTANT]  SDK allalaadimiseks luua Azure mitmekordne Auth pakkuja isegi juhul, kui teil on Azure MFA, AAD Premium või EMS litsentsid.  Kui loote Azure mitmekordne Auth pakkuja selleks ja juba litsentse, kindlasti pakkuja loomiseks mudeliga **Lubatud kasutaja kohta** . Seejärel link pakkuja Azure'i MFA, Azure AD Premium või EMS litsentside sisaldava kausta.  See tagab, et teil on ainult arve, kui teil on kordumatu kasutajaid, kui te oma litsentside arv SDK abil.


## <a name="to-create-a-multi-factor-auth-provider"></a>Mitme teguri Auth pakkuja loomiseks

Järgmiste juhiste abil saate luua Azure mitmekordne Auth pakkuja.

1. Logige sisse [Azure klassikaline portaali](https://manage.windowsazure.com) administraatorina.
2. Valige vasakul **Active Directory**.
3. Valige lehel Active Directory kohal **Mitme teguri autentimisteenuse pakkujate**.
![MFA osutaja loomine](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Allosas nuppu **Uus**.
![MFA osutaja loomine](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Valige jaotises rakenduse teenused **Mitme teguri Auth pakkuja**
![MFA osutaja loomine](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Valige **kiire loomine**.
![MFA osutaja loomine](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Täitke järgmised väljad ja valige **Loo**.
    1. **Nimi** – mitme teguri Auth pakkuja nimi.
    2. **Andmemudeli kasutamine** – kas soovite lubada üksikkasutajate või kontrollimise eest maksta. Valige üks kahest suvandist.
        - Kohta autentimine – osta mudel, et maksud autentimise kohta. Tavaliselt kasutatakse stsenaariumi, mis Azure'i Mitmikautentimise tarbija suunatud rakenduse kasutamine.
        - Lubatud kasutaja – osta mudel, mis kulude eest lubatud kasutaja. Tavaliselt kasutatakse töötaja juurdepääsu keelamine Office 365 näiteks. Valige see suvand, kui teil on mõned kasutajad, mis on juba litsentsitud Azure'i MFA jaoks.
    2. **Directory** – The Azure Active Directory rentniku mitmekordne autentimisteenuse seotud. Pidage silmas järgmist.
        - Peate mõne Azure AD directory mitme teguri Auth pakkuja loomiseks. Jätke väli tühjaks, kui te plaanite kasutada Azure mitmekordne autentimine serveri või SDK ainult.
        - Mitme teguri Auth pakkuja peab olema seostatud Directoryga Azure AD ära täiustatud funktsioone.
        - Azure'i AD-ühendus, AAD Sync või DirSync on ainult nõue, kui mõni Azure AD Directory kohapealse keskkonna Active Directory sünkroonimine.  Kui kasutate Azure AD kataloogi, mis on sünkroonitud, ainult siis, kui see on vajalik.
![MFA osutaja loomine](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Kui klõpsate nuppu loomine mitme teguri autentimisteenuse on loodud ja peaksite nägema teadet, mis kinnitab: **loodud mitme teguri autentimisteenuse**. Klõpsake nuppu **Ok**.
![MFA osutaja loomine](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
