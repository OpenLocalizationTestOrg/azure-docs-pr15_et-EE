<properties
    pageTitle="Määrata kasutajale administraatorirollid Azure Active Directory eelvaates | Microsoft Azure'i"
    description="Selgitab, kuidas muuta haldus kasutajateabe Azure Active Directory 's"
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

# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory-preview"></a>Administraatorirollid Azure Active Directory eelvaates kasutaja määramine

Selles artiklis selgitatakse administraatori rolli määramine kasutajale Azure Active Directory (Azure AD) eelvaates. [Mis on eelvaateversioonis?](active-directory-preview-explainer.md) Ettevõtte uute kasutajate lisamise kohta leiate teavet teemast [Azure Active Directory uute kasutajate lisamine](active-directory-users-create-azure-portal.md). Lisatud kasutajate pole administraatoriõigused vaikimisi, kuid saate määrata rollid neile igal ajal.

## <a name="assign-a-role-to-a-user"></a>Kasutajale rolli määramine

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajad ja rühmad** ja seejärel valige **Sisesta**.

    ![Avamine kasutajate haldamine](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  Enne **kasutajad ja rühmad** , valige **Kõik kasutajad**.

    ![Kõigi kasutajate tera avamine](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)

4. Enne **kasutajad ja rühmad – kõik kasutajad** , valige loendist kasutaja.

5. Enne valitud kasutaja, valige **Directory roll**ja seejärel määrata kasutaja rollid **Directory rolli** loendist. Kasutaja ja administraatori rollid kohta leiate lisateavet teemast [administraatorirollide Azure AD](active-directory-assign-admin-roles.md).

      ![Kasutaja lisamine rolli määramine](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. Valige **Salvesta**.


## <a name="whats-next"></a>Mis saab edasi

- [Kasutaja lisamine](active-directory-users-create-azure-portal.md)
- [Uue Azure portaali kasutaja parooli lähtestamine](active-directory-users-reset-password-azure-portal.md)
- [Kasutaja töö teabe muutmine](active-directory-users-work-info-azure-portal.md)
- [Kasutajaprofiilide haldamine](active-directory-users-profile-azure-portal.md)
- [Oma Azure AD kasutaja kustutamine](active-directory-users-delete-user-azure-portal.md)
