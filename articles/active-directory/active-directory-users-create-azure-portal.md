<properties
    pageTitle="Uute kasutajate lisamine Azure Active Directory eelvaade | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse uute kasutajate lisamiseks ja Azure Active Directory kasutajateabe muutmise."
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


# <a name="add-new-users-to-azure-active-directory-preview"></a>Uute kasutajate lisamine Azure Active Directory eelvaade

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-users-create-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-create-users.md)

Selles artiklis selgitatakse, kuidas lisada uusi kasutajaid ettevõtte eelvaates Azure Active Direstory (Azure AD). [Mis on eelvaateversioonis?](active-directory-preview-explainer.md)

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajad ja rühmad** ja seejärel valige **Sisesta**.

    ![Avamine kasutajate haldamine](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  Enne **kasutajad ja rühmad** , valige **Kõik kasutajad**ja ja seejärel valige **Lisa**.

    ![Valige käsk Lisa](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  Sisestage üksikasjad kasutajat, nt **nimi** ja **kasutajale nimi**. Kasutajanimi osa domeeni nimi peab olema kas algse domeeni nimi "foo.onmicrosoft.com" vaikedomeeninime või kinnitatud, ühendamata domeeni nimi, näiteks "contoso.com."

5. Kopeerige või muul viisil Märkus loodud kasutaja parooli, et saate selle sisestada kasutajale pärast selle toimingu lõpuleviimist.

6. Soovi korral saate avada ja sisestage teave **profiili** tera, **rühmad** tera või **Directory rolli** tera kasutaja jaoks. Kasutaja ja administraatori rollid kohta leiate lisateavet teemast [administraatorirollide Azure AD](active-directory-assign-admin-roles.md).

7.  Enne **kasutaja** , valige **Loo**.

8. Turvaline levitamine loodud parool uus kasutaja, et kasutaja saab sisse logida.

## <a name="whats-next"></a>Mis saab edasi

- [Väline kasutaja lisamine](active-directory-users-create-external-azure-portal.md)
- [Uue Azure portaali kasutaja parooli lähtestamine](active-directory-users-reset-password-azure-portal.md)
- [Kasutaja töö teabe muutmine](active-directory-users-work-info-azure-portal.md)
- [Kasutajaprofiilide haldamine](active-directory-users-profile-azure-portal.md)
- [Oma Azure AD kasutaja kustutamine](active-directory-users-delete-user-azure-portal.md)
- [Kasutajale määrata rolli teie Azure AD](active-directory-users-assign-role-azure-portal.md)
