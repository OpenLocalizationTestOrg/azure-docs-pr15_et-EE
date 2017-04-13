<properties
    pageTitle="Azure Active Directory eelvaates rühma liikmete haldamine | Microsoft Azure'i"
    description="Kuidas kasutajatele ja seadmetele, mis on Azure Active Directory rühma liikmed"
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


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Azure Active Directory eelvaates rühma liikmete haldamine

Selles artiklis selgitatakse, kuidas eelvaates Azure Active Directory (Azure AD) rühma liikmete haldamine. [Mis on eelvaates?](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Kuidas leida liikmeid ja neid hallata?

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajad ja rühmad** ja seejärel valige **Sisesta**.

  ![Avamine kasutajate haldamine](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  Enne **kasutajad ja rühmad** , valige **kõik rühmad**.

  ![Rühmade tera avamine](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. Enne **kasutajad ja rühmad - kõik rühmad** , valige rühm.

5. * *Rühmitamine - *groupname* ** tera, valige **liikmed **.

  ![Liikmete tera avamine](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. Jaotises **rühma - liikmed** enne, liikmete lisamiseks valige **Lisa liikmeid**.

  ![Liikmete käsk Lisa](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. Enne **liikmed** , valige üks või mitu kasutajatele või seadmete rühma lisada, ja valige tera allosas nuppu **Valige** lisamiseks rühma. **Kasutaja** välja filtreerib põhjal sobitamine teie kirjet selle kasutaja või seadme nime osa. Pole metamärkide aktsepteeritakse see ruut.

8. Liikmete eemaldamiseks rühmast enne **rühma - liikmed** , valige liige.

9. Enne ***membername*** , valige käsk **Eemalda** ja kinnitage tehtud valikust kuvatakse vastav viip.

  ![Liikmete käsu eemaldamine](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. Kui olete muutnud rühma liikmed, valige **Salvesta**.


## <a name="additional-information"></a>Lisateave

Azure Active Directory esitada lisateavet järgmistest artiklitest.

* [Vaadake olemasoleva rühmad](active-directory-groups-view-azure-portal.md)
* [Uue rühma loomine ja liikmete lisamine](active-directory-groups-create-azure-portal.md)
* [Rühma sätete haldamine](active-directory-groups-settings-azure-portal.md)
* [Rühma liikmestaatuste haldamine](active-directory-groups-membership-azure-portal.md)
* [Dünaamiliste reeglite kasutajate rühma haldamine](active-directory-groups-dynamic-membership-azure-portal.md)
