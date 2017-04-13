<properties
    pageTitle="Kõrge-saadavus ja avariitaastet SQL serveri korral | Microsoft Azure'i"
    description="Arutelu eri tüüpi HADR strateegiad SQL Server Azure'i Virtuaalmasinates töötab."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Kõrge kättesaadavus ja Avariijärgne taaste SQL Server Azure'i Virtuaalmasinates

## <a name="overview"></a>Ülevaade

Microsoft Azure'i virtuaalmasinates (VM) SQL serveriga võib aidata vähendada on kõrge kättesaadavus ja avariitaastet (HADR) andmebaasi lahendus. Enamik SQL serveri HADR lahenduste on toetatud Azure'i virtuaalmasinates Azure ainult nii nagu hübriid lahendused. Kogu HADR süsteem töötab ainult Azure lahenduse Azure. Hübriidkonfiguratsiooni, töötab osa lahendusest Azure ja muu osa käivitatakse asutusesisese teie asutuses. Azure keskkonnas paindlikkust võimaldab liikuda osaliselt või täielikult Azure eelarve ja HADR oma SQL serveri andmebaasi süsteemid vastama.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Lahenduse HADR vajadust mõistmine

On teil tagada, et teie andmebaasi süsteemi on HADR võimalusi, mis nõuab teenusetaseme leping (SLA). Azure'i pakub kõrge-saadavus menetlustele, nt teenuse tervendav pilveteenustega ja tõrge taastamise avastamine Virtuaalmasinates, mitte ise taga soovitud SLA vastate. Need kaitsta kõrge kättesaadavus VMs, kuid mitte kõrge kättesaadavus SQL Server töötab VMs sees. On võimalik nurjumise ajal VM on võrgus ja korralik SQL serveri eksemplar. Lisaks isegi kõrge kättesaadavus menetlustele esitatud Azure luba seisakuaja vms tõttu sündmusi, näiteks tarkvara või riistvara tõrkeid ja operatsioonisüsteemi uuendamine.

Lisaks ei pruugi Geo liigsete salvestusruumi (GRS) Azure, mis on rakendatud funktsiooni geo-dispersioonanalüüs, lahenduse piisav katastroofi taastamine teie andmebaasidest. Kuna geo-dispersioonanalüüs saadab andmeid asünkroonselt, saab viimatiste värskenduste katastroofi korral kaotsi. Geo-dispersioonanalüüs piirangud kohta saate lisateavet asuvad jaotises [Geo-dispersioonanalüüs andmed ja log failide eraldi kettale ei toetata](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>HADR juurutamise arhitektuur

Mis on toetatud Azure SQL serveri HADR tehnoloogiad on järgmised:

- [Alati kättesaadavus rühmade kohta](https://technet.microsoft.com/library/hh510230.aspx)
- [Andmebaasi peegeldus](https://technet.microsoft.com/library/ms189852.aspx)
- [Logi saatmine](https://technet.microsoft.com/library/ms187103.aspx)
- [Varundus ja taaste koos Azure'i bloobimälu salvestusteenus](https://msdn.microsoft.com/library/jj919148.aspx)
- [Alati tõrkesiirdeklastrite eksemplari kohta](https://technet.microsoft.com/library/ms189134.aspx)

On võimalik kombineerida tehnoloogiad koos kasutusele SQL serveri lahenduse, mis sisaldab nii suur-saadavus ja funktsioonid. Sõltuvalt tehnoloogia, mida kasutada, võib hübriidjuurutuse nõuda VPN tunneliga Azure virtuaalse võrguga. Allpool näete mõne näite juurutamise arhitektuur.

## <a name="azure-only-high-availability-solutions"></a>Azure'i ainult: kõrge-saadavus lahendused

Saate on kõrge-saadavus lahenduse oma SQL serveri andmebaasi Azure alati klõpsake kättesaadavus rühmade kasutamine või andmebaasi peegeldus.

| Tehnoloogia                               | Näide arhitektuur                    |
| ---------------------------------------- | ---------------------------------------- |
| **Alati kättesaadavus rühmade kohta**        | Kõigi kättesaadavus koopiate töötab Azure VMs kõrge-saadavus sama piirkonna jaoks. Peate konfigureerima domeenikontrolleri VM, kuna Windows Server Tõrkesiirde rühmitamise (WSFC) nõuab Active Directory domeenis.<br/> ![Alati kättesaadavus rühmade kohta](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Lisateavet leiate teemast [Konfigureerimine alati klõpsake kättesaadavus rühmade Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Alati tõrkesiirdeklastrite eksemplari kohta** | Tõrkesiirde kobar eksemplarid (FCI), mis nõuavad ühine, saab luua 2 erineval viisil.<br/><br/>1. klõpsake kaks sõlme WSFC, mis töötab koos salvestusruumi kolmanda osapoole klastrite lahendus ei toeta Azure VMs FCI. Teatud näide, mis kasutab SIOS DataKeeper, leiate teemast [kõrge kättesaadavus failikettale WSFC ja 3 tarkvara SIOS Datakeeper abil](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. klõpsake kaks sõlme WSFC, töötavad Azure VMs remote iSCSI Target ühiskasutuses Blokeeri salvestusruumi kaudu ExpressRoute FCI. Näiteks NetApp privaatne salvestusruumi (NPS) seab sihtseadmega ExpressRoute koos Equinix Azure vms kaudu.<br/><br/>Kolmanda osapoole jagatud salvestusruumi ja andmete kopeerimine lahenduste, peate pöörduma tarnija jaoks Tõrkesiirde andmetele juurdepääs seotud probleemidest.<br/><br/>Pange tähele, et abil FCI peal [Azure'i failide talletamine](https://azure.microsoft.com/services/storage/files/) ei toetata, kuna see lahendus ei kasuta Premium mälu. Töötame selle kiiresti toetamiseks. |

## <a name="azure-only-disaster-recovery-solutions"></a>Azure'i ainult: katastroofi taastamine lahendused

Saate on katastroofi taastamise lahendus teie alati klõpsake kättesaadavus rühmade kasutamine Azure SQL serveri andmebaasidega, andmebaasi peegeldus või varundamine ja taastamine salvestusruumi plekid.

| Tehnoloogia                               | Näide arhitektuur                    |
| ---------------------------------------- | ---------------------------------------- |
| **Alati kättesaadavus rühmade kohta**        | Töötab üle mitme Azure VMs tõrkejärgseks andmekeskuses kättesaadavus koopiad. See lahendus rist-piirkond kaitseb terviklik katkestuste. <br/> ![Alati kättesaadavus rühmade kohta](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Piirkonna, kõigi koopiate peaks olema sama pilveteenus ja sama VNet. Kuna iga piirkonna on eraldi VNet, lahendused nõuavad VNet VNet Ühenduvus. Lisateavet leiate teemast [konfigureerimine Azure klassikaline portaalis VPN saidilt](../vpn-gateway/vpn-gateway-site-to-site-create.md). |
| **Andmebaasi peegeldus**                   | Põhisummat ja Peegelveerised ja serverites töötab muus andmekeskuste tõrkejärgseks. Juurutamist peab serveri sertide kasutamine, kuna active directory domeenis ei saa ühendada mitu andmekeskuste.<br/>![Andmebaasi peegeldus](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Varundus ja taaste koos Azure'i bloobimälu salvestusteenus** | Tootmise andmebaasi varundada otse bloobimälu erinevate andmekeskuses tõrkejärgseks.<br/>![Varundus ja taaste](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Lisateavet leiate teemast [varundus ja taaste SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hübriidjuurutuse IT: Katastroofi taastamine lahendused

Saate oma SQL serveri andmebaasi alati klõpsake kättesaadavus rühmad, andmebaasi peegeldus, Logi saatmine ja varundamise abil hübriid-IT-keskkonnas katastroofi taastamise lahendus on ja Azure ajaveebi salvestusruumi taastada.

| Tehnoloogia                               | Näide arhitektuur                    |
| ---------------------------------------- | ---------------------------------------- |
| **Alati kättesaadavus rühmade kohta**        | Mõned kättesaadavus koopiad töötab Azure VMs ja muude töötab kohapealse saitidevaheline tõrkejärgseks koopiatega. Tootmiskohast võib olla kas kohapeal või mõne Azure andmekeskuses.<br/>![Alati kättesaadavus rühmade kohta](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Kuna kõigi kättesaadavus koopiate peab olema sama WSFC kobar, peab WSFC kobar ühendada nii võrkude (mitme alamvõrgu WSFC klaster). Selle konfiguratsiooni nõuab Azure ja kohapealse võrgu vahel VPN-ühendus.<br/><br/>Eduka tõrkejärgseks andmebaasi peaksite installima koopia domeenikontrolleri katastroofi taastamine saidil.<br/><br/>On võimalik lisada koopia viisardi kasutamine SSMS Azure'i koopia lisamiseks olemasolevasse alati klõpsake kättesaadavus rühma. Lisateabe saamiseks vaadake õpetus: Azure'i alati klõpsake kättesaadavus rühma laiendamine. |
| **Andmebaasi peegeldus**                   | Ühe partneri töötab ka Azure VM ja muude töötab kohapealse saitidevaheline tõrkejärgseks serveri sertide kasutamine. Partnerite ei pea olema sama Active Directory domeeni ja VPN-ühendus on nõutav.<br/>![Andmebaasi peegeldus](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Mõnda muusse andmebaasi peegeldus stsenaarium hõlmab üks partner on Azure VM ja muude töötava asutusesisese sama Active Directory domeeni saitidevaheline tõrkejärgseks töötab. [Azure virtuaalse võrgu ja kohapealse võrgu vahel VPN-ühendus](../vpn-gateway/vpn-gateway-site-to-site-create.md) on nõutav.<br/><br/>Eduka tõrkejärgseks andmebaasi peaksite installima koopia domeenikontrolleri katastroofi taastamine saidil. |
| **Logi saatmine**                         | Ühe on Azure VM ja muude käivitatud asutusesisene saitidevaheline tõrkejärgseks töötava serveri. Logi saatmine sõltub Windowsi failide ühiskasutus, seega Azure virtuaalse võrgu ja kohapealse võrgu vahel VPN-ühendus vajalik.<br/>![Logi saatmine](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Eduka tõrkejärgseks andmebaasi peaksite installima koopia domeenikontrolleri katastroofi taastamine saidil. |
| **Varundus ja taaste koos Azure'i bloobimälu salvestusteenus** | Kohapealse tootmise andmebaasi varundada otse Azure'i bloobimälu tõrkejärgseks.<br/>![Varundus ja taaste](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Lisateavet leiate teemast [varundus ja taaste SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>SQL serveri HADR Azure olulised kaalutlused

Azure'i VMs, salvestusruumi ja võrk on erinevad funktsionaalseid omadused kui asutusesisese, mitte virtualiseeritud IT taristu. Eduka rakendamise Azure lahenduse HADR SQL Server nõuab, et teil mõista nende erinevusi ja kujundamine mahutamiseks need teie lahendus.

### <a name="high-availability-nodes-in-an-availability-set"></a>Kõrge-saadavus sõlmed saadavus seadmine

Azure kättesaadavus komplektid võimaldavad teha kõrge-saadavus sõlmed eraldi viga Domains (FDs) ja Värskenda Domains (UDs). Azure'i vms paigutada samale kättesaadavus määramine, peate need sama pilveteenuses juurutamine. Ainult sõlmed sama pilveteenuses saavad osaleda samu kättesaadavus. Lisateavet leiate teemast [Virtuaalmasinates kättesaadavus](virtual-machines-windows-manage-availability.md).

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>Azure'i võrgunduse WSFC kobar käitumist

Azure RFC-mittevastavate DHCP teenus, võivad põhjustada teatud WSFC kobar konfiguratsioone nurjumise tõttu kobar võrgu nimi on määratud korduv IP-aadress, nt sama IP-aadressi üks kobar sõlmed loomine. See on probleeme, kui rakendate alati klõpsake kättesaadavus rühmad, mis sõltub WSFC funktsiooni.

Kujutage ette stsenaariumi, kui kaks sõlme kobar on loodud ja veebis.

1. Klaster võrguühenduse ja seejärel NODE1 taotleb kobar võrgu nimi dünaamiliselt IP-aadress.

2. Peale NODE1 oma IP-aadress pole IP-aadress on antud DHCP teenus, kuna DHCP teenus tuvastab, et taotluse pärineb NODE1 ise.

3. Windows tuvastab, et korduv aadress on määratud NODE1 ja kobar võrgu nimi, ja kobar vaikerühma nurjub veebis.

4. Kobar vaikerühma viib NODE2, mis käsitleb NODE1 tema IP-aadress kobar IP-aadress ja kobar vaikerühma veebis toob.

5. Kui NODE2 proovib luua Ühenduvus NODE1, jätta paketid suunatud NODE1 NODE2 kunagi, kuna see lahendab ise NODE1 tema IP-aadress. NODE2 ei saa luua NODE1 Ühenduvus, siis lähevad kaotsi kvoorumi lõpetatakse ja klaster.

6. Vahepeal NODE1 saab saata pakette NODE2, kuid NODE2 ei vasta. NODE1 klaster lõpetatakse ja kvoorumi lähevad kaotsi.

Sel juhul saate vältida kasutamata staatiline IP-aadress, link-local IP-aadress 169.254.1.1, nagu näiteks määramisel kobar võrgu nimi, et tuua kobar võrgu nimi võrgus. Protsessi hõlbustamiseks leiate [Konfigureerimise tõrkesiirdeklastrite Windows Azure alati klõpsake kättesaadavus rühmade jaoks](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Lisateavet leiate teemast [Konfigureerimine alati klõpsake kättesaadavus rühmade Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Kättesaadavus rühma kuulajale tugi

Kättesaadavus rühma kuulajatele on toetatud Azure VMs, kus töötab Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ja Windows Server 2016. See tugi on võimalik koormusetasakaalustusega lõpp-punktid lubatud Azure VMs, mis on kättesaadavus rühma sõlmed abil. Te peate järgima teisiti konfigureerimistoimingute töötada nii klientrakendustes, mis töötavad Azure kui ka need töötavad kohapealse kuulajatele.

On kaks peamist võimalust häälestamiseks oma kuulajale: väline (avaliku) või väljas. Välise (avaliku) kuulajale kasutab vastastikuste laadi koormusetasakaalustusteenuse Interneti ja seotud koos avaliku virtuaalse IP (VIP) mis Internetis juurde pääseb. On sisemine kuulajale kasutab ka sisemise koormuse koormusetasakaalustusteenuse ja ainult toetada sama virtuaalse võrgustikus kliente. Kas koormusetasakaalustusteenuse tüüpi laadida, tuleb lubada otsest Server tagasi. 

Kui rühma kättesaadavus ulatub mitu Azure alamvõrku (nt juurutus, mis ületab Azure piirkondade), kliendi ühendusstringi peab sisaldama "**MultisubnetFailover = True**". Tulemuseks paralleelselt ühenduse katsete koopiad teise alamvõrku. On kuulajale häälestamise juhised leiate teemast

- [Mõne ILB kuulajale alati klõpsake kättesaadavus rühmade Azure konfigureerimine](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Mõne välise kuulajale alati klõpsake kättesaadavus rühmade Azure konfigureerimine](virtual-machines-windows-classic-ps-sql-ext-listener.md).

Saate endiselt ühendada iga kättesaadavus koopia eraldi ühendatud eksemplari. Ka, kuna alati klõpsake kättesaadavus rühmad on tagasiühilduvad andmebaasi peegeldus kliendid, saate luua ühenduse kättesaadavus koopiad, nt andmebaasi peegeldus partnerid, kui selle koopiad on konfigureeritud sarnast andmebaasi peegeldus:

- Ühe peamine koopia ja ühe teise koopia

- Teise koopia on konfigureeritud-loetav (**Loetavad teisene** valik määratud **ei**)

Allpool on näide kliendi ühenduse string, mis vastab selle andmebaasi peegeldus meeldib konfiguratsiooni kasutades ADO.net-i või SQL serveri Omakliendi.

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Klientarvuti Ühenduvus kohta leiate lisateavet teemast:

- [SQL serveri Omakliendi ühenduse stringi märksõnade abil](https://msdn.microsoft.com/library/ms130822.aspx)
- [Klientide andmebaasiga ühenduse loomiseks peegeldus seanss (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Ühenduse hübriid kättesaadavus rühma kuulajale](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Kättesaadavus rühma kuulajatele, klientrakenduse Ühenduvus ja rakenduse Tõrkesiirde (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Andmebaasi peegeldus ühendusstringi abil kättesaadavus rühmad](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Võrgu latentsuse hübriid IT

Juurutamist peaks teie HADR lahendus eeldusel, et võib aega, kõrge Võrgu latentsuse kohapealse võrgu ja Azure. Azure'i koopiad juurutamisel tuleks kasutada asünkroonne Kinnita asemel sünkroonse Kinnita sünkroonimise režiimi. Kui andmebaasi peegeldus serverid juurutamine nii kohapealse ja Azure, suure jõudlusega režiimi asemel kasutada kõrge turvalisuse režiimi.

### <a name="geo-replication-support"></a>Geo-dispersioonanalüüs tugi

Geo-dispersioonanalüüs Azure ketta ei toeta andmefaili ja sama andmebaasi talletamise eraldi kettale logifail. GRS tiražeerib muudatuste kohta iseseisvalt ja asünkroonselt. See tagab kirjutamine tellimuse jooksul ühe ketta geo kopeeritud koopia, kuid mitte üle mitme ketast geo kopeeritud koopiad. Kui konfigureerite andmebaasi salvestada selle faili ja selle logifaili eraldi kettale, taastatud ketast pärast katastroofi võib sisaldada andmefaili uuemast koopia kui logifaili kirjutamine võrra edasi log in SQL serveri ja tehingud ACID omadused katkestades. Kui teil võimalus keelata geo-dispersioonanalüüs salvestusruumi kontol alles jätta kõik andmed ja logifailide failid andmebaasi samale kettale. Kui kasutate rohkem kui üks ketas tõttu andmebaasi mahtu, peate Juurutage üks ülaltoodud andmete koondamine tagamiseks katastroofi taastamine lahendusi.

## <a name="next-steps"></a>Järgmised sammud

Kui teil on vaja luua SQL Server Azure'i virtuaalarvuti, lugege teemat [Azure SQL serveri virtuaalse masina ettevalmistamise](virtual-machines-windows-portal-sql-server-provision.md).

SQL Server töötab ka Azure VM parimaid tulemusi saada, lugege teemat [Jõudluse head tavad SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-performance.md)juhiseid.

Muud teemad, mis töötab SQL Server Azure'i VMs seotud, vt [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Muud ressursid

- [Uue Active Directory kogumis installida Azure](../active-directory/active-directory-new-forest-virtual-machine.md)
- [Luua WSFC kobar jaoks alati Azure'i VM rühmade kättesaadavus](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
