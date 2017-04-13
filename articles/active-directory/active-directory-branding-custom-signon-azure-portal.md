<properties
pageTitle="Azure Active Directory eelvaates oma sisselogimislehel kohandamine | Microsoft Azure'i"
description="Saate teada, kuidas mõni ettevõte, sümboolika Azure lehe lisamine"
services="active-directory"
documentationCenter=""
authors="curtand"
manager="femila"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Ettevõtte sümboolika Azure Active Directory eelvaate lehele sisselogimiseks lisamine

Vältida segadust, paljud soovite rakendada ühtse ilme ja olemus üle kõik veebilehed ja teenused, mida nad hallata. Azure Active Directory eelvaade pakub seda võimalust, mis võimaldab teil kohandada oma ettevõtte logo ja kohandatud Värviskeemid sisselogimise lehe ilme. [Mis on eelvaates?](active-directory-preview-explainer.md) Sisselogimise leht on leht, mis kuvatakse siis, kui logite teenusekomplekti Office 365 või muu veebipõhised rakendused, mida kasutate oma identiteedipakkuja Azure AD. Suhtlete selle lehe sisestage oma kasutajanimi ja parool.

Kui soovite kuvada oma ettevõtte kaubamärgile, värvide ja muude kohandatavate elementide sellel lehel, lugege teemat järgmistel piltidel on kaks versiooni teineteisest erinevused.

Järgmine pilt kuvatakse ja Office 365 sisselogimise lehele on lauaarvuti **enne** kohandamine näide:

![Office 365 sisselogimislehel enne kohandamine](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Järgmine pilt kuvatakse ja Office 365 sisselogimise lehele on lauaarvuti **pärast** kohandamine näide:

![Office 365 sisselogimislehel pärast kohandamine](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>Sisselogimise lehe kohandamine

Tavaliselt, kui teil on vaja oma pilve rakendused ja teenused, mida teie ettevõte tellib Brauseripõhine juurdepääs, kasutate sisselogimise leht.

Kui olete rakendanud muudatusi teie lehele, võib kuluda kuni tund kuvatakse muudatuste.

Firmastiilis sisselogimislehel kuvatakse ainult teenuse rentniku kohased URL, näiteks https://outlook.com/**contoso**.com või https://mail külastamisel. **contoso**. com.

Külastamisel teenuse koos – rentniku teatud URL-id (nt: https://mail.office365.com), kuvatakse selliseid sisselogimise leht. Sel juhul margikujundus kuvatakse, kui olete sisestanud oma kasutaja ID või valitud kasutaja paan.

> [AZURE.NOTE]
>
- Teie domeeni nimi peab asuma nimega "Aktiivne" **domeenide** osa, kus on konfigureeritud, sümboolika Azure portaali. Lisateavet leiate teemast [lisamine kohandatud domeeninimesid](active-directory-domains-add-azure-portal.md).
- Sisselogimislehel sümboolika ei üle kanda tarbija sisselogimislehel Microsofti. Kui logite sisse Microsofti kontoga, võite näha firmastiilis loendi kasutaja paanid muudab Azure AD, kuid teie ettevõtte sümboolika ei kehti Microsofti konto lehele sisselogimiseks.

Klõpsake lehel sisselogimine ruut **Hoia mind sisse logituna** saab kasutaja väljalogimiseks sulgeda ja uuesti avada oma brauseris. 

   ![Hoia mind sisse logitud](./media/active-directory-branding-custom-signon-azure-portal/01.png)

See ei mõjuta seansi eluiga. Saate peita Azure Active Directory sisselogimise lehel olev ruut.
Kas märkeruut kuvatakse sõltub selle sätte **Hoia mind sisse logituna keelatud**.

   ![Hoia mind sisse logitud](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Märkeruut peitmiseks konfigureerida seda sätet **Jah**. 

> [AZURE.NOTE] Mõned funktsioonid Office 2010 ja SharePoint Online'i sõltuvad kasutajad ei saaks seda märkige see ruut. Kui konfigureerite seda sätet peidetud, kasutajate võidakse kuvada täiendavad ja ootamatud viipasid sisse logima.




**Ettevõtte kataloogi margikujunduse lisamiseks tehke järgmist.**

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajad ja rühmad** ja seejärel valige **Sisesta**.

    ![Avamine kasutajate haldamine](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. Enne **kasutajad ja rühmad** , valige **ettevõtte sümboolika**.

4. Klõpsake soovitud **kasutajad ja rühmad - ettevõtte sümboolika** tera, valige käsk **Redigeeri** .

    ![Kohandatud margikujundus redigeerimine](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Muutke elemendid, mida soovite kohandada. Kõik elemendid on valikuline.

6. Klõpsake nuppu **Salvesta**.

Võib kuluda kuni tund mis tahes muudatusi sümboolika kuvada sisselogimise leht.

## <a name="next-steps"></a>Järgmised sammud

[Lisage Keelekohased ettevõtte sümboolika](active-directory-branding-localize-azure-portal.md)
