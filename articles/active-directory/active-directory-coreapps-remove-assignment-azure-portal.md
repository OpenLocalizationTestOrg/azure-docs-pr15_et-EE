<properties
    pageTitle="Kasutaja või rühma ülesande eemaldamiseks rakenduse enterprise Preview Azure Active Directory | Microsoft Azure'i"
    description="Ettevõtte rakenduse Azure Active Directory access ülesande kasutaja või rühma eemaldamine"
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


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Kasutaja või rühma ülesande eemaldamiseks rakenduse enterprise Preview Azure Active Directory 's

See on lihtne eemaldada kasutaja või rühma määratud Accessi üks rakendustest enterprise Preview Azure Active Directory (Azure AD). [Mis on eelvaates?](active-directory-preview-explainer.md) Peab teil olema rakenduse ettevõtte haldamiseks vajalikud õigused. Praeguse eelvaates, peate olema üldadministraator kataloogi.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Kuidas eemaldada kasutaja või rühma ülesande?

1. Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2. Valige **rohkem teenuseid**, Sisestage tekstiväljale **Azure Active Directory** ja seejärel valige **Sisesta**.

3. Funktsiooni * *Azure Active Directory - *directoryname* ** blade (ehk teisisõnu öeldes Azure AD tera haldate kataloogi jaoks), valige **ettevõtte rakenduste **.

    ![Ettevõtte rakenduste avamine](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Enne **ettevõtte rakendused** , valige **Kõik rakendused**. Näete saate hallata rakenduste loendit.

5. Enne **ettevõtte rakendused - kõik rakendused** , valige rakendus.

6. Enne ***rakendusenimi*** (ehk teisisõnu öeldes blade pealkiri valitud rakenduse nimi), valige **kasutajad ja rühmad**.

    ![Kasutajate või rühmade valimine](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Klõpsake ***rakendusenimi*** **– kasutaja ja rühma ülesande** tera, valige üks järgmistest mitu kasutajat või rühma ja seejärel valige käsk **Eemalda** . Kinnitage viibale otsust.

    ![Valige käsk Eemalda](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Järgmised sammud

- [Näha kõiki minu rühmad](active-directory-groups-view-azure-portal.md)
- [Ettevõtte rakenduse kasutaja või rühma määramine](active-directory-coreapps-assign-user-azure-portal.md)
- [Kasutaja sisselogimist enterprise rakenduse keelamine](active-directory-coreapps-disable-app-azure-portal.md)
- [Rakenduse ettevõtte logo või nime muutmine](active-directory-coreapps-change-app-logo-user-azure-portal.md)
