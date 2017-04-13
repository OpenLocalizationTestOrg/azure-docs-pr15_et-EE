<properties
    pageTitle="Azure Active Directory domeeniteenused: Haldamine klõpsake hallatavate domeenide DNS-i | Microsoft Azure'i"
    description="Klõpsake Azure Active Directory domeeniteenused hallatavate domeenide DNS-i haldamine"
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
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Mõni Azure AD domeeniteenused hallatava domeeni DNS-i haldamine
Azure Active Directory domeeniteenused sisaldab DNS-i (Domain Name eraldusvõime) server, mis pakub hallatava domeeni DNS-i resolvimise. Aeg-ajalt, peate võib-olla hallatava domeeni DNS-i konfigureerimine. Kui peate masinad, mis on ühendatud domeeni DNS-kirjete loomine, konfigureerida virtuaalse IP-aadresside koormus soolise või häälestamise välise DNS-i edasisuunamislahendusi. Seetõttu antakse kasutajaid, kes kuuluvad 'AAD näiteks Põhiliselt administraatorite' rühma DNS-i halduse õigusi hallatavate domeenis.


## <a name="before-you-begin"></a>Enne alustamist
Selles artiklis toodud ülesannete täitmiseks peate.

1. Kehtiv **Azure'i tellimus**.

2. Mõne **directory Azure'i AD** - ühte sünkroonida mõne kohapealse kataloogi või vaid kataloogist.

3. Azure AD directory **domeeniteenused Azure AD** peab olema lubatud. Kui te pole seda veel teinud, täitke ülesandeid, mis on kirjeldatud [kasutusjuhend](./active-directory-ds-getting-started.md).

4. **Domeeni ühendatud virtuaalse masina** , kust saate hallata Azure AD domeeniteenused hallatud domeeni. Kui teil pole virtuaalse masina, täitke kõik [liitumine Windows virtuaalse masina hallatava domeeni](./active-directory-ds-admin-guide-join-windows-vm.md)nimega artiklis kirjeldatud toimingud.

5. Peate kataloogi hallatava domeeni DNS-i hallata identimisteabe **' AAD näiteks Põhiliselt administraatorid ' rühma kuuluvate kasutajakonto** .

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Ülesanne 1 - eemalt hallata hallatava domeeni DNS-i domeeni ühendatud virtuaalse masina säte
Azure'i AD domeeniteenused hallatavate domeenide saab hallata kaugkustutada tuttavad Active Directory Haldusriistad, nt Active Directory haldus Center (ADAC) või AD PowerShelli. Samuti saate hallatava domeeni DNS-i manustada, kaugkustutada DNS-i serveri Haldustööriistad.

Administraatorid Azure AD kataloogis pole domeenikontrollerid hallatava domeeni kaudu kaugtöölaua ühenduse õigusi. 'AAD näiteks Põhiliselt administraatorite' rühma liikmed saate hallata DNS-i hallatavate domeenide kaugkustutada Windows Server/kliendi arvutisse, mis on ühendatud hallatava domeeni DNS-i serveri tööriistad. DNS-serveri tööriistad installimist Remote Server Administration Tools (RSAT) valikuline funktsioon Windows Server ja ühendatud hallatava domeeni klientarvutite osana.

Esimese tööülesande on Windows Server virtuaalse masina, mis on ühendatud hallatava domeeni ettevalmistamine. Juhiste saamiseks lugege artiklit [liitumine Windows Server virtuaalse masina Azure AD domeeniteenused hallatavate domeeni](active-directory-ds-admin-guide-join-windows-vm.md)nimega.


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Ülesanne 2: installi DNS-i Server tools virtual arvutisse
Järgmiste toimingute DNS-i haldamise tööriistad ühendatud domeeni virtual arvutisse installida. [Installimine ja Remote Haldustööriistad kasutamise](https://technet.microsoft.com/library/hh831501.aspx)kohta lisateabe saamiseks leiate TechNeti.

1. Liikuge klassikaline portaalis Azure'i **Virtuaalmasinates** sõlm. Valige virtuaalse masina loodud ülesanne 1 ja käsuribal akna allosas käsku **Ühenda** .

    ![Ühenduse loomine Windows virtuaalse masina](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klassikaline portaali küsib, kas soovite avada või salvestada faili laiendiga "RDP", virtuaalse masina ühenduse loomiseks kasutatava. Kui see allalaadimine on lõppenud, klõpsake nuppu fail.

3. Käsureale login mandaatidega 'AAD näiteks Põhiliselt administraatorid' rühma kuuluvate kasutaja. Näiteks kasutame 'bob@domainservicespreview.onmicrosoft.com' meie puhul.

4. Avage avakuva, **Server Manager**. Klõpsake akna Server Manager keskse paanil **rollide ja funktsioonide lisamine** .

    ![Käivitage Server Manager virtual arvutisse](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. **Enne alustamist** lehe **lisada rollid ja funktsioonid viisardi**, klõpsake nuppu **edasi**.

    ![Enne alustamist leht](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Klõpsake lehel **Installi tüübi** , jätke märgituks **rolli või funktsiooni installimise** ja klõpsake nuppu **edasi**.

    ![Installimislehele tüüp](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Lehel **Server valik** valige praegune virtuaalse masina kaustast server ja klõpsake nuppu **edasi**.

    ![Lehe server valik](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Klõpsake lehel **Serverirollide** nuppu **Järgmine**. Me otse selle lehe, kuna me ei installi rollide, serveris.

9. Lehel **funktsioonid** klõpsake laiendage **Remote Haldustööriistad** ja klõpsake laiendage **Rolli Haldustööriistad** . Valige **DNS-i serveri tööriistad** funktsiooni loendist rolli Haldustööriistad.

    ![Lehe funktsioonid](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Klõpsake lehel **Confirmation** **installida** DNS-serveri tööriistad funktsiooni virtual arvutisse installida. Kui funktsioon installi edukalt lõpule jõudnud, klõpsake nuppu **Sule** **rollide ja funktsioonide lisamine** viisardi sulgemiseks.

    ![Kinnituslehel](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Ülesanne 3 - käivitada halduskonsooli DNS-i DNS-i haldamiseks
Nüüd, kui DNS-i serveri tööriistad funktsiooni on installitud domeeni liitunud virtuaalse masina, kasutame DNS-i tööriistade hallatava domeeni DNS-i hallata.

> [AZURE.NOTE] Peate kuuluma 'AAD näiteks Põhiliselt administraatorid' jaotises hallatava domeeni DNS-i hallata.

1. Klõpsake avakuval **Haldustööriistad**. Peaksite nägema **DNS-i** konsooli virtual arvutisse installitud.

    ![Haldusriistade - DNS-i konsooli](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Klõpsake **DNS-i** DNS-i halduskonsooli käivitamiseks.

3. **DNS-i serveriga** dialoogiboksi pealkirjaga **Järgmine arvuti**soovitud suvandit ja sisestage hallatavate domeeni (nt contoso100.com) DNS-i domeeni nimi.

    ![DNS-i konsooli - ühenduse domeeni](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. Ühendab hallatava domeeni DNS-i konsooli.

    ![DNS-i konsooli - domeeni haldamine](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Nüüd saate DNS-i konsooli arvutites, kus olete lubanud AAD domeeniteenused virtuaalse võrgustikus DNS-i kirjete lisamiseks.

> [AZURE.WARNING] Olge ettevaatlik, kui DNS-i haldamise hallatava domeeni DNS-i halduse tööriista abil. Tagada selle te **ei saa kustutada või muuta sisseehitatud DNS-i kirjeid, mis kasutavad domeeniteenused Domeen**. Sisseehitatud DNS-i kirjed sisaldavad domeeni DNS-i kirjeid, nimeserveri kirjeid ja muude kirjetega, näiteks Põhiliselt asukoha jaoks kasutada. Kui muudate need kirjed, domeeniteenused on katkenud virtuaalse võrgus.


Vt [DNS-i tööriistade TechNeti artikkel](https://technet.microsoft.com/library/cc753579.aspx) Lisateavet DNS-i haldamise kohta.


## <a name="related-content"></a>Seotud teemad

- [Domeeniteenused Azure'i AD - kasutusjuhend](./active-directory-ds-getting-started.md)

- [Liitumine virtuaalse masina Windows Server Azure'i AD domeeniteenused hallatava domeeni](active-directory-ds-admin-guide-join-windows-vm.md)

- [Mõni Azure AD domeeniteenused hallatava domeeni haldamine](active-directory-ds-admin-guide-administer-domain.md)

- [DNS-i haldamise tööriistad](https://technet.microsoft.com/library/cc753579.aspx)
