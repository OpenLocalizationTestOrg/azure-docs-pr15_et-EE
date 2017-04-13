<properties
    pageTitle="Rühma on Azure Active Directory eelvaates liige rühmade haldamine | Microsoft Azure'i"
    description="Rühmad võivad sisaldada muude rühmade Azure Active Directory. Siin on need liikmestaatuste haldamine."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>Rühma on Azure Active Directory eelvaates liige rühmade haldamine

Rühmad võivad sisaldada muude rühmade Azure Active Directory eelvaade. [Mis on eelvaates?](active-directory-preview-explainer.md) Siin on need liikmestaatuste haldamine.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Kuidas otsida rühmad, mis on minu rühma liige?

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajad ja rühmad** ja seejärel valige **Sisesta**.

  ![Avamine kasutajate haldamine](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Enne **kasutajad ja rühmad** , valige **kõik rühmad**.

  ![Rühmade tera avamine](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Enne **kasutajad ja rühmad - kõik rühmad** , valige rühm.

5. * *Rühma - *groupname* ** tera, valige **rühma liikmestaatusi **.

  ![Rühma liikmestaatusi tera avamine](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Valige oma rühma lisada teise rühma, **rühma - rühmaliikmeid** enne, kui käsk **Lisa** .

7. Valige rühm, **Valige jaotises** keelest ja valige tera allosas nuppu **Valige** . Korraga saate oma rühma lisada ainult üks rühm. **Kasutaja** välja filtreerib põhjal sobitamine teie kirjet selle kasutaja või seadme nime osa. Pole metamärkide aktsepteeritakse see ruut.

  ![Rühma liikmete lisamine](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Teise rühma, **rühma - rühmaliikmeid** enne, kui teie rühma eemaldamiseks valige rühm.

9. Enne ***groupname*** , valige käsk **Eemalda** ja kinnitage tehtud valikust kuvatakse vastav viip.

  ![liikmelisuse käsu eemaldamine](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Kui olete muutnud rühmaliikmeid oma rühma, valige **Salvesta**.


## <a name="additional-information"></a>Lisateave

Azure Active Directory esitada lisateavet järgmistest artiklitest.

* [Vaadake olemasoleva rühmad](active-directory-groups-view-azure-portal.md)
* [Uue rühma loomine ja liikmete lisamine](active-directory-groups-create-azure-portal.md)
* [Rühma sätete haldamine](active-directory-groups-settings-azure-portal.md)
* [Rühma liikmete haldamine](active-directory-groups-members-azure-portal.md)
* [Dünaamiliste reeglite kasutajate rühma haldamine](active-directory-groups-dynamic-membership-azure-portal.md)
