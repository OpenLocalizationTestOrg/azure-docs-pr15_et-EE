<properties
    pageTitle="Azure'i AD domeeniteenused: Loomine või valige virtuaalse võrgus | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused töötamise alustamine"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Looge või valige virtuaalse võrgu Azure AD domeeni teenuste jaoks

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Juhised, mis on Azure virtuaalse võrgu valimine
> [AZURE.NOTE] **Enne alustamist**: [Networking kaalutluste kohta Azure AD domeeniteenused](active-directory-ds-networking.md)viidata.


## <a name="task-2-create-an-azure-virtual-network"></a>Ülesanne 2: Luua Azure virtuaalse
Järgmise ülesande konfigureerimine on luua mõne Azure virtuaalse võrgu ja on alamvõrgu kogumahtu. Azure'i AD domeeniteenused selle alamvõrgu oma virtuaalse võrgustikus lubamine Kui teil on juba olemasolevat virtuaalse võrku eelistate kasutada, võite selle sammu vahele jätta.

> [AZURE.NOTE] Veenduge, et Azure'i alale, mis toetab Azure AD domeeniteenused kuulumist Azure virtuaalse võrgu saate luua või valida Azure AD domeeni teenustega kasutada. Teemast [Azure teenuste regiooniti](https://azure.microsoft.com/regions/#services/) lehe leida Azure piirkonnad, mille Azure AD domeeniteenused on saadaval.

Pange tähele, virtuaalse võrgu nimi, nii, et valite paremale virtuaalse võrgu kui Azure AD domeeniteenused konfiguratsiooni järgmise juhise lubamine.

Järgmiste toimingute konfiguratsiooni luua Azure virtuaalse, kus soovite lubada Azure AD domeeniteenused.

1. Liikuge **Azure klassikaline portaali** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Valige vasakpoolsel paanil **võrkude** sõlm.

    ![Võrgu sõlm](./media/active-directory-domain-services-getting-started/networks-node.png)

3. Klõpsake paani lehe allosas nuppu **Uus** .

    ![Virtuaalne võrkude sõlm](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. Valige **Võrguteenuste** sõlme **Virtuaalse võrgu**.

5. Klõpsake **Kiiresti luua** virtuaalse võrgu loomiseks.

    ![Virtuaalse võrgu - kiire loomine](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Määrake virtuaalse võrgu **nimi** . Samuti võite selle võrgu jaoks konfigureerida **aadressiruumi** või **VM maksimaalne arv** . Nüüd saate **DNS-i serveri** sätte väärtuseks "pole jätta. Saate värskendada seadmine pärast teie luba Azure AD domeeni teenuste DNS-i serveri.

7. Valige rippmenüüst **asukoht** toetatud Azure piirkond kindlasti. Teemast [Azure teenuste regiooniti](https://azure.microsoft.com/regions/#services/) lehe leida Azure piirkonnad, mille Azure AD domeeniteenused on saadaval.

8. Virtuaalse võrgu loomiseks nuppu **Loo virtuaalse võrgus** .

    ![Luua virtuaalse võrgu Azure AD domeeni teenuste jaoks.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Virtuaalne võrk on loodud, valige virtuaalse võrgu ja klõpsake vahekaarti **KONFIGUREERIMINE** .

    ![Mõne alamvõrgu loomine](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Liikuge jaotisse **virtuaalse võrgu aadress tühikuid** . Klõpsake nuppu **Lisa alamvõrgu** ja määrake soovitud alamvõrgu nimega **AaddsSubnet**. Klõpsake nuppu **Salvesta** alamvõrgu loomiseks.

    ![Luua mõne alamvõrgu Azure AD domeeni teenuste jaoks.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Ülesanne 3 – luba Azure AD domeeniteenused
Järgmise ülesande konfigureerimine on [Azure AD domeeniteenused lubamine](active-directory-ds-getting-started-enableaadds.md).
