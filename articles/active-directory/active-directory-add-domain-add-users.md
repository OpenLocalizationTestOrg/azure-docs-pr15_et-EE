<properties
    pageTitle="Kohandatud domeeni Azure Active Directory kasutajate määramine | Microsoft Azure'i"
    description="Kuidas asustamiseks Azure Active Directory domeeni kasutaja kontod."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Kohandatud domeeni kasutajate määramine

Kui olete lisanud oma kohandatud domeeni Azure Active Directory, peate lisama selle domeeni Kasutajakontod nii, et need autentimisel alustamist.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Teie ettevõtte võrguga kataloogi sünkroonitud kasutajad

Kui olete juba loonud ühenduse vahel oma kohapealse Active Directory ja Azure Active Directory, saate sünkroonimise asustada kontod. Azure Active Directory sünkroonimine oma kohapealse Active Directory kohta leiate lisateavet teemast [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Kasutajad lisanud ja hallatavate pilveteenuses

Domeeni jaoks olemasolevat kasutajakontot muutmiseks tehke järgmist.

1.  Avage Azure'i klassikaline portaal kontoga, millel on üld- või kasutaja administraatorina.

2.  Avage kataloogi.

3.  Valige vahekaart **Kasutajad** .

4.  Valige loendist soovitud kasutaja.

5.  Kasutaja domeeni muutmine, ja seejärel valige **Salvesta**.

Seda saab teha [Microsoft PowerShelli](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) või [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Valige kohandatud domeeni, kui loote uue kasutaja

1.  Avage Azure'i klassikaline portaal kontoga, millel on üld- või kasutaja administraatorina.

2.  Avage kataloogi.

3.  Valige vahekaart **Kasutajad** .

4.  Valige käsk **Lisa**.

5.  Kui lisate kasutaja nimi, valige loendist domeeni kohandatud domeeni.

6.  Valige **Salvesta**.

## <a name="next-steps"></a>Järgmised sammud

-   [Kohandatud domeeni nime abil lihtsustada kasutajate jaoks soovitud sisselogimine](active-directory-add-domain.md)

-   [Kohandatud domeeninimede haldamine](active-directory-add-manage-domain-names.md)

-   [Lisateavet domeeni haldamise põhimõtet Azure AD](active-directory-add-domain-concepts.md)
