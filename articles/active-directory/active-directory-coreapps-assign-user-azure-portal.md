<properties
    pageTitle="Kasutaja või rühma määramiseks rakenduse enterprise Preview Azure Active Directory | Microsoft Azure'i"
    description="Ettevõtte rakenduse määrata Azure Active Directory selle kasutaja või rühma valimine"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Kasutaja või rühma määramiseks rakenduse enterprise Preview Azure Active Directory 's

See on lihtne kasutaja või rühma määramiseks enterprise rakenduste Azure Active Directory (Azure AD) eelvaates. [Mis on eelvaates?](active-directory-preview-explainer.md) Peab teil olema rakenduse ettevõtte haldamiseks vajalikud õigused. Praeguse Preview, peate olema üldadministraator kataloogi.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Kuidas määrata kasutajate juurdepääsu ettevõtte rakendus?

1. Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2. Valige **rohkem teenuseid**, Sisestage tekstiväljale Azure Active Directory ja seejärel valige **Sisesta**.

3. Funktsiooni * *Azure Active Directory - *directoryname* ** blade (ehk teisisõnu öeldes Azure AD tera haldate kataloogi jaoks), valige **ettevõtte rakenduste **.

    ![Ettevõtte rakenduste avamine](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Enne **ettevõtte rakendused** , valige **Kõik rakendused**. Näete saate hallata rakenduste loendit.

5. Enne **ettevõtte rakendused - kõik rakendused** , valige rakendus.

6. Enne ***rakendusenimi*** (ehk teisisõnu öeldes blade pealkiri valitud rakenduse nimi), valige **kasutajad ja rühmad**.

    ![Valige käsk Kõik rakendused](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Klõpsake ***rakendusenimi*** **– kasutaja ja rühma ülesande** tera, valige käsk **Lisa** .

8. Enne **Ülesande lisada** , valige **kasutajad ja rühmad**.

    ![Rakenduse kasutaja või rühma määramine](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Enne **kasutajad ja rühmad** , valige loendist üks või mitu kasutajate või rühmade ja seejärel valige tera allosas nuppu **Valige** .

10. Enne **Ülesande lisada** , valige **roll**. Seejärel **Valige rolli** enne, valige roll, et rakendada valitud kasutajatele või rühmadele, ja seejärel valige tera allservas nuppu **OK** .

11. Enne **Ülesande lisada** , nuppu **määramine** tera allosas. Määratud kasutajatele või rühmadele on selle ettevõtte rakenduse valitud rolliga määratletud õigused.

## <a name="next-steps"></a>Järgmised sammud

- [Näha kõiki minu rühmad](active-directory-groups-view-azure-portal.md)
- [Ettevõtte rakenduse kasutaja või rühma ülesande eemaldamine](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Kasutaja sisselogimist enterprise rakenduse keelamine](active-directory-coreapps-disable-app-azure-portal.md)
- [Rakenduse ettevõtte logo või nime muutmine](active-directory-coreapps-change-app-logo-user-azure-portal.md)
