<properties
    pageTitle="Kasutajate lisamine muude kataloogid või partneri ettevõtete Azure Active Directory eelvaates | Microsoft Azure'i"
    description="Selgitab, kuidas kasutajaid lisada või muuta kasutajateabe Azure Active Directory, sh külaline ja välised kasutajad."
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

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Kasutajate lisamine muude kataloogide või partnerid Azure Active Directory preview 's

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-users-create-external-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-create-users-external.md)

Selles artiklis selgitatakse, kuidas kasutajaid lisada muid kataloogide Azure Active Directory (Azure AD) eelvaates või partnerid. [Mis on eelvaateversioonis?](active-directory-preview-explainer.md) Uute kasutajate lisamine oma asutuses ja kasutajatele, kellel on Microsofti kontode lisamise kohta leiate teavet teemast [Azure Active Directory uute kasutajate lisamine](active-directory-users-create-azure-portal.md). Lisatud kasutajate pole administraatoriõigused vaikimisi, kuid saate määrata rollid neile igal ajal.

## <a name="add-a-user"></a>Kasutaja lisamine

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajad ja rühmad** ja seejärel valige **Sisesta**.

    ![Avamine kasutajate haldamine](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Enne **kasutajad ja rühmad** , valige **Kasutajad**ja seejärel valige **Lisa**.

    ![Valige käsk Lisa](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Enne **kasutaja** , sisestage kuvatav nimi väljale **nimi** ja kasutaja sisselogimisnimi **kasutaja**nimi.

5. Kopeerige või muul viisil Märkus loodud kasutaja parooli, et saate selle sisestada kasutajale pärast selle toimingu lõpuleviimist.

6. Soovi korral valige **profiili** esmalt lisada nende kasutajate ja perekonnanimi, töö pealkiri ja osakonna nimi.
    
    ![Kasutajaprofiili avamine](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Valige **rühmad** rühmade ühe või mitme kasutaja lisamiseks.

        ![Rühmade kasutaja lisamine](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Valige **ettevõtte rollile** määrata rolli kasutaja **rollid** loendist. Kasutaja ja administraatori rollid kohta leiate lisateavet teemast [administraatorirollide Azure AD](active-directory-assign-admin-roles.md).

        ![Kasutaja lisamine rolli määramine](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Valige **Loo**.

8. Turvaline levitamine loodud parool uus kasutaja, et kasutaja saab sisse logida.

> [AZURE.IMPORTANT] Kui teie asutus kasutab rohkem kui üks domeen, peaks teadma järgmist kasutajakonto lisamisel:
>
> - Lisada kasutajakontosid sama kasutaja turvasubjektinimi (UPN) domeenides, **esimese** lisada, näiteks geoffgrisso@contoso.onmicrosoft.com, **, millele järgneb** geoffgrisso@contoso.com.
> - **Ära** lisa geoffgrisso@contoso.com enne lisamist geoffgrisso@contoso.onmicrosoft.com. Selles järjestuses on oluline ja võib olla tülikas tagasi võtta.

Kui muudate teavet kasutaja nime, kelle identiteeti on sünkroonitud kohapealse Active Directory teenust, ei saa muuta kasutajateabe Azure klassikaline portaalis. Kasutaja teabe muutmiseks kasutage oma kohapealse Active Directory Haldusriistad.


## <a name="whats-next"></a>Mis saab edasi

- [Kasutaja lisamine](active-directory-users-create-azure-portal.md)
- [Uue Azure portaali kasutaja parooli lähtestamine](active-directory-users-reset-password-azure-portal.md)
- [Kasutajale määrata rolli teie Azure AD](active-directory-users-assign-role-azure-portal.md)
- [Kasutaja töö teabe muutmine](active-directory-users-work-info-azure-portal.md)
- [Kasutajaprofiilide haldamine](active-directory-users-profile-azure-portal.md)
- [Oma Azure AD kasutaja kustutamine](active-directory-users-delete-user-azure-portal.md)
