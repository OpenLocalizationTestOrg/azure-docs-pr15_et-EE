<properties
    pageTitle="Uue rühma loomine Azure Active Directory eelvaates | Microsoft Azure'i"
    description="Kuidas luua rühma Azure Active Directory ja (liikmed) kasutajate lisamine rühma"
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


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Uue rühma loomine Azure Active Directory preview 's

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-groups-create-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-accessmanagement-manage-groups.md)
- [PowerShelli](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Selles artiklis selgitatakse, kuidas luua ja määrata uue rühma eelvaates Azure Active Directory (Azure AD). [Mis on eelvaates?](active-directory-preview-explainer.md) Rühma abil saate teha haldustoiminguid, näiteks määramine litsentse või õiguste korraga mitme kasutaja või seadmed.

## <a name="how-do-i-create-a-group"></a>Kuidas luua rühma?

1. Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2. Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajate ja rühmade** ja seejärel valige **Sisesta**.

  ![Avamine kasutajate haldamine](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Enne **kasutajad ja rühmad** , valige **kõik rühmad**.

  ![Rühmade tera avamine](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Enne **kasutajad ja rühmad - kõik rühmad** , valige käsk **Lisa** .

  ![Valige käsk Lisa](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Enne **rühma** , lisage nimi ja kirjeldus rühma.

6. Liikmete lisamiseks rühma valimiseks valige **määratud** väljal **liikmelisuse tüüp** ja valige **liikmed**. Rühma liikmete dünaamiliselt haldamise kohta leiate lisateavet teemast [kasutamine atribuute rühma liikmeks keerukamate reeglite loomiseks](active-directory-groups-dynamic-membership-azure-portal.md).

  ![Liikmete lisamiseks valimine](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Enne **liikmed** , valige üks või mitu kasutajatele või seadmete rühma lisada, ja valige tera allosas nuppu **Valige** lisamiseks rühma. **Kasutaja** välja filtreerib põhjal sobitamine teie kirjet selle kasutaja või seadme nime osa. Pole metamärkide aktsepteeritakse see ruut.

6. Kui olete rühma liikmete lisamise lõpetanud, valige enne **rühma** **loomine** .    

  ![Kinnituse rühma loomine](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Lisateave

Azure Active Directory esitada lisateavet järgmistest artiklitest.

* [Vaadake olemasoleva rühmad](active-directory-groups-view-azure-portal.md)
* [Rühma sätete haldamine](active-directory-groups-settings-azure-portal.md)
* [Rühma liikmete haldamine](active-directory-groups-members-azure-portal.md)
* [Rühma liikmestaatuste haldamine](active-directory-groups-membership-azure-portal.md)
* [Dünaamiliste reeglite kasutajate rühma haldamine](active-directory-groups-dynamic-membership-azure-portal.md)
