<properties
pageTitle="Lisada Keelekohased ettevõtte sümboolika Azure Active Directory eelvaate lehele sisselogimiseks | Microsoft Azure'i"
description="Saate teada, kuidas lisada keele konkreetse ettevõtte sümboolika pildid ja tekst Azure sisselogimise leht"
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
ms.date="09/12/2016"
ms.author="curtand"/>

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Lisage Keelekohased ettevõtte sümboolika sisselogimise leht Azure Active Directory preview 's

Vältida segadust, paljud soovite rakendada ühtse ilme ja olemus üle kõik veebilehed ja teenused, mida nad hallata. Azure Active Directory eelvaade pakub seda võimalust, mis võimaldab teil kohandada oma ettevõtte logo ja kohandatud Värviskeemid sisselogimise lehe ilme. [Mis on eelvaates?](active-directory-preview-explainer.md) Sisselogimise leht on leht, mis kuvatakse siis, kui logite teenusekomplekti Office 365 või muu veebipõhised rakendused, mida kasutate oma identiteedipakkuja Azure AD. Suhtlete selle lehe sisestage oma kasutajanimi ja parool.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Mõne muu keele sisselogimise lehe kohandamine

Keelekohased elemendid saate lisada oma kohandatud sisselogimisleht, ainult juhul, kui olete juba loonud kohandatud sisselogimislehel on kirjeldatud [ettevõtte sümboolika oma lehele lisada](active-directory-branding-custom-signon-azure-portal.md). Saate konfigureerida sisselogimise iga lehekülg directory vaikimisi määratud kohandatav elemente. Kui olete konfigureerinud leheelementide vaikimisi komplekt, saate konfigureerida rohkem versioonid eri asukohtades. Saate segada ja vastavad mitmesuguseid elemente. Näiteks võib teil:

- Saate luua vaikimisi **sisselogimise lehe pilt** , mis sobib kõik kultuuri, siis teatud versioonide jaoks inglise ja Prantsuse loomine. Kui seate oma brauserid ühe keele, Keelekohased pilt kuvatakse, kui vaikimisi pildil kuvatakse kõigi muude keelte jaoks.

- Konfigureerige erinevad logod ettevõtte (nt Jaapani või heebrea versioon).

Soovitame teil keele variatsioonide arv madal hooldus ja jõudluse tagamiseks.

**Ettevõtte kataloogi margikujunduse lisamiseks tehke järgmist.**

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajad ja rühmad** ja seejärel valige **Sisesta**.

    ![Avamine kasutajate haldamine](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. Enne **kasutajad ja rühmad** , valige **ettevõtte sümboolika**.

4. Klõpsake soovitud **kasutajad ja rühmad - ettevõtte sümboolika** tera, valige käsk **Lisa keel** .

    ![Lisage Keelekohased tootemargi elemendid](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Muutke elemendid, mida soovite kohandada. Kõik elemendid on valikuline.

6. Klõpsake nuppu **Salvesta**.

Võib kuluda kuni tund mis tahes muudatusi sümboolika kuvada sisselogimise leht.

## <a name="next-steps"></a>Järgmised sammud

[Ettevõtte sümboolika oma lehe lisamine](active-directory-branding-custom-signon-azure-portal.md)
