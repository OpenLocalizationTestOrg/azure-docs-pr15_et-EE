<properties
    pageTitle="Keela kasutaja sisselogimist enterprise rakenduse Azure Active Directory eelvaates | Microsoft Azure'i"
    description="Ettevõtte rakenduse keelamine nii, et pole kasutajad võivad sisse logida selle Azure Active Directory 's"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Kasutaja sisselogimist Azure Active Directory eelvaates enterprise rakenduse keelamine

On lihtne keelata rakenduse enterprise nii, et pole kasutajad võivad sisse logida selle Azure Active Directory (Azure AD) eelvaates. [Mis on eelvaates?](active-directory-preview-explainer.md) Peab teil olema rakenduse ettevõtte haldamiseks vajalikud õigused. Praeguse eelvaates, peate olema üldadministraator kataloogi.

## <a name="how-do-i-disable-user-sign-ins"></a>Kuidas keelata kasutaja sisselogimist?

1. Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2. Valige **rohkem teenuseid**, Sisestage tekstiväljale **Azure Active Directory** ja seejärel valige **Sisesta**.

3. Funktsiooni * *Azure Active Directory - *directoryname* ** blade (ehk teisisõnu öeldes Azure AD tera haldate kataloogi jaoks), valige **ettevõtte rakenduste **.

    ![Ettevõtte rakenduste avamine](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Enne **ettevõtte rakendused** , valige **Kõik rakendused**. Näete saate hallata rakenduste loendit.

5. Enne **ettevõtte rakendused - kõik rakendused** , valige rakendus.

6. Enne ***rakendusenimi*** (ehk teisisõnu öeldes blade pealkiri valitud rakenduse nimi), valige **Atribuudid**.

    ![Valige käsk Kõik rakendused](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Klõpsake ***rakendusenimi*** **-Atribuudid** tera, valige **pole** jaoks **sisse logima kasutajatele lubatud?**.

8. Valige käsk **Salvesta** .

## <a name="next-steps"></a>Järgmised sammud

- [Vaadake kõiki minu rühmad](active-directory-groups-view-azure-portal.md)
- [Ettevõtte rakenduse kasutaja või rühma määramine](active-directory-coreapps-assign-user-azure-portal.md)
- [Ettevõtte rakenduse kasutaja või rühma ülesande eemaldamine](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Rakenduse ettevõtte logo või nime muutmine](active-directory-coreapps-change-app-logo-user-azure-portal.md)
