<properties
    pageTitle="Azure'i AD domeeniteenused: Värskenda DNS-i sätted Azure virtuaalse võrgu jaoks | Microsoft Azure'i"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Domeeniteenused Azure'i AD - Azure virtuaalse võrgu Värskenda DNS-i sätted

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Ülesanne 4: Värskendada DNS-i sätted Azure virtuaalse võrgu jaoks
Eelnevate konfigureerimistoimingud, on edukalt lubatud Azure AD domeeni teenuste kataloogi. Järgmise ülesande on tagada, et arvutid virtual võrgustikus saate luua ja kasutada nende teenuste. Värskendage DNS-serveri sätete virtuaalse võrgu osutage kaks IP-aadressid, kus Azure AD domeeniteenused on virtuaalse võrgus saadaval.

> [AZURE.NOTE] Azure'i AD domeeni teenuste kuvatud, klõpsake **konfigureerimine** menüü kataloogi, kui teil on lubatud Azure AD domeeni teenuste kataloogi märkida üles IP-aadressid.

Järgmiste toimingute konfiguratsiooni värskendada DNS-i server, kus teil on lubatud Azure AD domeeniteenused virtuaalse võrgu säte.

1. Liikuge **Azure klassikaline portaali** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Valige vasakpoolsel paanil **võrkude** sõlm.

    ![Virtuaalne võrkude sõlm](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. **Virtuaalne võrkude** menüüs valige virtuaalse võrk, mille te lubatud Azure AD domeeniteenused kuvada selle atribuute.

4. Klõpsake vahekaarti **konfigureerimine** .

    ![Virtuaalne võrkude sõlm](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. Sisestage jaotises **DNS-serverid** Azure AD domeeniteenused IP-aadressid.

6. Veenduge, et sisestate nii IP-aadresside kataloogi **konfigureerimine** menüü jaotisest **Domeeniteenused** kuvatav.

7. Selle virtuaalse võrgu jaoks DNS-serveri sätete salvestamiseks klõpsake tööpaani lehe allosas nuppu **Salvesta** .

   ![Värskendage DNS-serveri sätete virtuaalse võrgu jaoks.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Pärast värskendamist virtuaalse võrgu jaoks DNS-i serveri sätted, võib kuluda aega virtuaalmasinates saada värskendatud DNS-i konfigureerimine võrgus. Kui virtuaalse masina ei saa luua ühendust domeeni, saate joondada DNS-i vahemälu (nt. "ipconfig/flushdns") virtual arvutisse. See käsk jõustab Värskenda virtuaalse masina DNS-i sätted.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Tööülesande 5 - Azure AD domeeniteenused luba parooli sünkroonimine
Järgmise ülesande konfigureerimine on [Azure AD domeeniteenused parooli sünkroonimise](active-directory-ds-getting-started-password-sync.md)lubamiseks.
