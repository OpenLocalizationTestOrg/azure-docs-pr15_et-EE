<properties
    pageTitle="Azure Active Directory domeeniteenused: Haldamine hallatava domeeni | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused hallatavate domeenide haldamine"
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
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Mõne hallatava Azure Active Directory domeeniteenuste domeeni haldamine
Selles artiklis kirjeldatakse, kuidas hallata on Azure Active Directory (AD) domeeniteenused hallatavate Domeen.


## <a name="before-you-begin"></a>Enne alustamist
Selles artiklis toodud ülesannete täitmiseks peate.

1. Kehtiv **Azure'i tellimus**.

2. Mõne **directory Azure'i AD** - ühte sünkroonida mõne kohapealse kataloogi või vaid kataloogist.

3. Azure AD directory **domeeniteenused Azure AD** peab olema lubatud. Kui te pole seda veel teinud, täitke ülesandeid, mis on kirjeldatud [kasutusjuhend](./active-directory-ds-getting-started.md).

4. **Domeeni ühendatud virtuaalse masina** , kust saate hallata Azure AD domeeniteenused hallatud domeeni. Kui teil pole virtuaalse masina, täitke kõik [liitumine Windows virtuaalse masina hallatava domeeni](./active-directory-ds-admin-guide-join-windows-vm.md)nimega artiklis kirjeldatud toimingud.

5. Peate kataloogi hallata hallatava domeeni identimisteabe **' AAD näiteks Põhiliselt administraatorid ' rühma kuuluvate kasutajakonto** .

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Saate teha hallatavate domeenis haldustoiminguid
'AAD näiteks Põhiliselt administraatorite' rühma liikmed antakse õigused hallatavate domeenis, mis võimaldavad näiteks teha järgmisi toiminguid:

- Liitumine masinad hallatava domeeni.

- Sisseehitatud GPO "AADDC arvutid" ja 'AADDC kasutajad' ümbriste konfigureerida hallatava domeeni.

- Hallatava domeeni DNS-i haldamine.

- Luua ja hallata kohandatud ettevõtte üksused (organisatsiooniüksused) hallatava domeenis.

- Hallatava domeeni ühendatud arvutites juurdepääsu saamiseks haldus.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Teil pole hallatavate domeenis administraatoriõigusi
Domeeni haldab Microsoft, sh lappimine, jälgimine ja läbimiseks varukoopiad. Seetõttu domeen on lukus ja teil on õigused teatud haldustoimingute sooritamiseks domeenis. Allpool on mõned näited toiminguid, mida ei saa teha.

- Te ei anta hallatava domeeni domeeni administraatori või ettevõtte administraatori õigusi.

- Skeemi hallatava domeeni ei saa laiendada.

- Te ei saa ühendust domeenikontrolleritega hallatava domeeni kaugtöölaua abil.

- Ei saa lisada hallatavate domeeni domeenikontrollerid.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Ülesanne 1 - sätte domeeni ühendatud Windows Server virtuaalse masina eemalt hallata hallatava domeeni
Azure'i AD domeeniteenused hallatava domeeni saab hallata tuttavad Active Directory Haldusriistad, nt Active Directory haldus Center (ADAC) või AD PowerShelli abil. Rentniku administraatorite domeenikontrollerid hallatava domeeni kaudu kaugtöölaua ühenduse õigusi pole. Seetõttu 'AAD näiteks Põhiliselt administraatorite' rühma liikmed saab hallata hallatavate domeenide kaugkustutada Windows Server/kliendi arvutisse, mis on ühendatud hallatava domeeni AD Haldustööriistad. Remote Server Administration Tools (RSAT) valikuline funktsioon Windows Server ja ühendatud hallatava domeeni klientarvutite osana installimist AD Haldustööriistad.

Esimese asjana Windows Server virtuaalse masina, mis on ühendatud hallatava domeeni häälestamiseks. Juhiste saamiseks lugege artiklit [liitumine Windows Server virtuaalse masina Azure AD domeeniteenused hallatavate domeeni](active-directory-ds-admin-guide-join-windows-vm.md)nimega.

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Eemalt hallata hallatava domeeni klientarvutist (nt Windows 10)
Selles artiklis kasutamine Windows Server virtuaalse masina juhiseid manustamiseks AAD-DS hallatud domeeni. Samas võite ka Windowsi kliendi (nt Windows 10) virtuaalse masina abil teha.

Saate [installida Remote Server Administration Tools (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) Windows klientarvutis virtual TechNeti juhiste järgi.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Ülesanne 2 - installi Active Directory haldamise tööriistad virtual arvutisse
Järgmiste toimingute Active Directory Haldustööriistad ühendatud domeeni virtual arvutisse installida. Leiate TechNeti [teavet installimiseks ja kasutamiseks Remote Haldustööriistad](https://technet.microsoft.com/library/hh831501.aspx).

1. Liikuge klassikaline portaalis Azure'i **Virtuaalmasinates** sõlm. Valige virtuaalse masina loodud ülesanne 1 ja käsuribal akna allosas käsku **Ühenda** .

    ![Ühenduse loomine Windows virtuaalse masina](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klassikaline portaali küsib, kas soovite avada või salvestada faili laiendiga "RDP", virtuaalse masina ühenduse loomiseks kasutatava. Kui see allalaadimine on lõppenud, avage fail, klõpsake nuppu.

3. Käsureale login mandaatidega 'AAD näiteks Põhiliselt administraatorid' rühma kuuluvate kasutaja. Näiteks kasutame 'bob@domainservicespreview.onmicrosoft.com' meie puhul.

4. Avage avakuva, **Server Manager**. Klõpsake akna Server Manager keskse paanil **rollide ja funktsioonide lisamine** .

    ![Käivitage Server Manager virtual arvutisse](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. **Enne alustamist** lehe **lisada rollid ja funktsioonid viisardi**, klõpsake nuppu **edasi**.

    ![Enne alustamist leht](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Klõpsake lehel **Installi tüübi** , jätke märgituks **rolli või funktsiooni installimise** ja klõpsake nuppu **edasi**.

    ![Installimislehele tüüp](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Lehel **Server valik** valige praegune virtuaalse masina kaustast server ja klõpsake nuppu **edasi**.

    ![Lehe server valik](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. **Serverirollid** lehel klõpsake nuppu **edasi**. Me otse selle lehe, kuna me ei installi rollide, serveris.

9. Lehel **funktsioonid** klõpsake laiendage **Remote Haldustööriistad** ja klõpsake laiendage **Rolli Haldustööriistad** . Valige loendist rolli haldustööriistade **AD DS- ja AD LDS** funktsiooni.

    ![Lehe funktsioonid](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Klõpsake lehel **Confirmation** klõpsake installi reklaami ja AD LDS tööriistad funktsiooni virtual arvutisse nuppu **Installi** . Kui funktsioon installi edukalt lõpule jõudnud, klõpsake nuppu **Sule** **rollide ja funktsioonide lisamine** viisardi sulgemiseks.

    ![Kinnituslehel](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Ülesanne 3 - ühenduse loomiseks ja selle hallatava domeeni uurimine
Nüüd, kui on installitud AD Haldustööriistad domeeni liitunud virtuaalse masina, saame nende tööriistade uurimine ja manustada selle hallatava domeeni kasutada.

> [AZURE.NOTE] Peate kuuluma 'AAD näiteks Põhiliselt administraatorid' jaotises hallatava domeeni haldamiseks.

1. Klõpsake avakuval **Haldustööriistad**. Peaksite nägema virtual arvutisse installitud AD Haldustööriistad.

    ![Serverisse installitud Haldusriistad](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klõpsake nuppu **Active Directory halduskeskus**.

    ![Active Directory halduskeskus](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Klõpsake domeeni avastamiseks domeeni nimi (nt contoso100.com) vasakul paanil. Pange tähele kahte ümbriste, mida nimetatakse "AADDC arvutid" ja "AADDC kasutajad".

    ![ADAC - vaade domeeni](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Klõpsake ümbris **AADDC kasutajad** näeksid kõik kasutajad ja rühmad kuuluvad hallatava domeeni nimega. Peaksite nägema Kasutajakontod ja rühmade kaudu oma Azure AD rentniku Kuva selles ümbrises. Selles näites teate, nimega "bob" kasutaja ja rühma nimega 'AAD näiteks Põhiliselt administraatorid' kasutajakonto on saadaval selles ümbrises.

    ![ADAC - domeeni kasutajad](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Klõpsake ümbris **AADDC arvutite** näha arvutites, mis on ühendatud selle hallatava domeeni nimega. Peaksite nägema praeguse virtuaalse masina, mis on ühendatud domeeni kirjet. Arvuti kontod on ühendatud Azure AD domeeniteenused hallatava domeeni kõigi arvutites salvestatakse see "AADDC arvutid" ümbrises.

    ![ADAC - domeeni ühendatud arvutites](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Seotud teemad

- [Domeeniteenused Azure'i AD - kasutusjuhend](./active-directory-ds-getting-started.md)

- [Liitumine virtuaalse masina Windows Server Azure'i AD domeeniteenused hallatava domeeni](active-directory-ds-admin-guide-join-windows-vm.md)

- [Remote Haldustööriistad juurutamine](https://technet.microsoft.com/library/hh831501.aspx)
