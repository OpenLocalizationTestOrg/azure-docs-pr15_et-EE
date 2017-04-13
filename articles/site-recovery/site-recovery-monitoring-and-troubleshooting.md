<properties
    pageTitle="Jälgimine ja kaitse virtuaalmasinates ja füüsilise servereid tõrkeotsing | Microsofti Auzre" 
    description="Azure'i saidi taastamine koordinaadid dispersioonanalüüs, Tõrkesiirde ja kohapealsed serverid Azure'i või sekundaarne andmekeskuse asuv virtuaalmasinates taastamine. Selles artiklis abil saate jälgida ja VMM või Hyper-V saidi tõrkeotsing." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Jälgimine ja kaitse virtuaalmasinates ja füüsilise servereid tõrkeotsing

Selle jälgimine ja tõrkeotsingu juhend võimaldab teil teavet jälgimise dispersioonanalüüs seisundi ja tõrkeotsingu tehnikate Azure saidi taastamise.

## <a name="understanding-the-components"></a>Mõistmine

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>VMware/füüsilise saidi juurutus kopeerimise kohapealse ja Azure vahel.
Häälestamise DR vahel kohapealse VMware/füüsilise seadme; Konfiguratsiooni serveri, juhtslaidi Target ja protsess peab konfigureeritud. Kaitse lähteserveriga võimaldades installida Azure saidi taastamine mobiilsus teenuse. Kohapealse katkestuste postitada, kui lähteserveriga nurjub üle Azure klientide vajaduste protsessi Server Azure'i häälestamine ja juhtslaidi Target serveri kohapealse andmeallika server taasloodud kohapealse kaitsmiseks. 

![VMware/füüsilise saidi juurutamise kopeerimise kohapealse ja Azure vahel](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>VMM saidi juurutus kopeerimise kohapealse saidi vahel.

Osana häälestamise DR vahel kahe kohapealse asukoht; Azure'i saidi taastamise pakkuja peab installitakse VMM-i serverisse. Pakkuja peab Interneti-ühendus, et tagada kõik toimingud, mis on Azure portaali käivitatakse saab kohapealse toiminguid nagu lubamiseks, sulgumist esmane pool virtuaalmasinates osana failovers jne.

![VMM saidi juurutamise kopeerimise kohapealse saidi vahel](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>VMM saidi juurutus kopeerimise kohapealse ja Azure vahel.

Osana häälestamise DR vahel kohapealse ja Azure; Azure'i saidi taastamise pakkuja peab installitakse koos Azure taastamise Services Agent, mis peab olema installitud iga Hyper-V hosti serveris VMM. Lisateabe saamiseks vaadake [Mõistmine saidi Azure kaitse](./site-recovery-understanding-site-to-azure-protection.md) .

![VMM saidi juurutamise kopeerimise kohapealse ja Azure vahel](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Hyper-V saidi juurutamise kopeerimise kohapealse ja Azure vahel

See on sama, mis VMM juurutamise – ainult vahe on pakkuja ja Agent saab installitud Hyper-V hosti ise. Lisateabe saamiseks vaadake [Mõistmine saidi Azure kaitse](./site-recovery-understanding-site-to-azure-protection.md) .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Konfiguratsiooni, kaitse ja jälgimine

Iga ASR saab auditeerida ja jälitatakse vahekaardil "Töökohtade". Mis tahes konfiguratsiooni, kaitse või taastamise tõrke korral klõpsake vahekaarti liikuge ja kas on mis tahes ebaõnnestumist.

![Konfiguratsiooni, kaitse ja jälgimine](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Kui olete leidnud töö vaate all tõrkeid, valige töö ja klõpsake nuppu tõrke ÜKSIKASJADE selle tööle.

![Konfiguratsiooni, kaitse ja jälgimine](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Tõrke üksikasjade aitavad teil tuvastada võimalikud põhjused ja soovitus probleemi.

![Konfiguratsiooni, kaitse ja jälgimine](media/site-recovery-monitoring-and-troubleshooting/image5.png)

Ülaltoodud juhul näib muu toiming, mis on pooleli, mille tõttu kaitse konfigureerimine nurjub. Veenduge, et probleemi soovitus – seal pärast ühe klõpsake toimingu uuesti käivitamiseks RESART lahendada.

![Konfiguratsiooni, kaitse ja jälgimine](media/site-recovery-monitoring-and-troubleshooting/image6.png)

UUESTI võimalus pole saadaval kõik toimingud – need, mis ei sisalda uuesti tagasi liikuda objekti ja tagasivõetud toimingu uuesti. Iga töö saab tühistada mis tahes hetkel aega, kui pooleli abil nuppu Loobu.

![Konfiguratsiooni, kaitse ja jälgimine](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Virtuaalne masina dispersioonanalüüs seisundi jälgimine

ASR pakkuja Kesk- ja remote Azure portaali kaudu iga kaitstud isikute jälgimine. Liikuge kaitstud seal pärast üksuste valimine VMM PILVED või rühmade. VMM PILVED menüü on ainult VMM vastavalt juurutuste ja kõigi teiste stsenaariumide on kaitstud üksuste rühmade vahekaardil.

![Virtuaalne masina dispersioonanalüüs seisundi jälgimine](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Pärast seal valige vastav pilveteenuses või kaitse rühma kaitstud üksus. Kui valite kaitstud üksus kõik lubatud toimingud on toodud alaosas.

![Virtuaalne masina dispersioonanalüüs seisundi jälgimine](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Nagu eespool näidatud juhul virtuaalse masina seisund on oluline – saate klõpsata nupp tõrke ÜKSIKASJADE kuvamiseks viga allosas. Põhjal "võimalike põhjuste" ja "Soovitus" mainitud probleemi lahendada.

![Virtuaalne masina dispersioonanalüüs seisundi jälgimine](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Virtuaalne masina dispersioonanalüüs seisundi jälgimine](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Märkus: Kui on aktiivne toimingud, mis on pooleli või nurjunud liikuge töö vaate nagu varem mainitud vaatamiseks töö tõrke.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Kohapealse Hyper-V probleemide tõrkeotsing

Ühenduse loomiseks kohapealse Hyper-V halduri konsooli, valige virtuaalse masina ja vaadake dispersioonanalüüs seisund.

![Kohapealse Hyper-V probleemide tõrkeotsing](media/site-recovery-monitoring-and-troubleshooting/image12.png)

Sel juhul *Dispersioonanalüüs seisund* näidatud nimega kriitiline- *Vaade Dispersioonanalüüs seisund* üksikasjade kuvamiseks.

![Kohapealse Hyper-V probleemide tõrkeotsing](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Paremklõpsake juhtudel, kus dispersioonanalüüs on peatatud virtuaalse masina, valige *Dispersioonanalüüs*->*elulookirjelduse dispersioonanalüüs*
![tõrkeotsing kohapealse Hyper-V probleemid](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Juhuks, kui virtuaalse masina migreerib uue Hyper-V host (jooksul klaster või autonoomse kohapeal), mis on konfigureeritud ASR, ei mõjutada dispersioonanalüüs virtuaalse masina jaoks. Veenduge, et uute Hyper-V server vastab kõik kohta – rekvisiitide ja konfigureeritud ASR.

### <a name="event-log"></a>Sündmuste logi

| Sündmuse allikad                | Üksikasjad                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Rakenduste ja teenuste logid/Microsoft/VirtualMachineManager/Server/administraator** (VMM Server)   |  See pakub kasulik logimine paljude erinevate VMM seotud probleemide tõrkeotsinguks. |
| **Rakenduste ja teenuste logid/MicrosoftAzureRecoveryServices/Dispersioonanalüüs** (Hyper-V Host)   | See pakub kasulik paljude Microsoft Azure taastamise Services Agent tõrkeotsingu logimine. <br/> ![Sündmuse allikas Hyper-V Server](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Rakenduste ja teenuste logid/Microsoft/Azure saidi taastamine/pakkuja/töö** (Hyper-V Host)   | See pakub kasulik paljude Microsoft Azure'i saidi taastamise teenust tõrkeotsingu logimine. <br/> ![Sündmuse allikas Hyper-V Server](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Rakenduste ja teenuste logid/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V Host) | See pakub kasulik logimine palju Hyper-V virtuaalse masina halduse seotud probleemide tõrkeotsinguks. <br/> ![Sündmuse allikas Hyper-V Server](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Logimissuvandite Hyper-V kopeerimine

Kõik sündmused, mis on seotud Hyper-V koopia sisse loginud Hyper-V-VMMS\\Admin Logi paiknevad **rakenduste ja teenuste logid\\Microsoft\\Windows**. Lisaks saate ka analüütiline log lubatud Hyper-V-VMMS. See logi lubamiseks veenduge vaadatav vaataja analüütiline ja silumine logid. Avage Sündmusevaatur, klõpsake **menüüd Vaade**, klõpsake **Kuva analüütiline ja silumine logid**.

![Kohapealse Hyper-V probleemide tõrkeotsing](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Mõne analüütiline Logi on nähtav Hyper-V-VMMS

![Kohapealse Hyper-V probleemide tõrkeotsing](media/site-recovery-monitoring-and-troubleshooting/image15.png)

Klõpsake paanil **toimingud** **Lubamine Logi**sisse. Kui lubatud, kuvatakse see **Jõudluse** monitoris nagu mõni sündmus Jälita seansi paiknevad **koguja andmekogumi.**

![Kohapealse Hyper-V probleemide tõrkeotsing](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Kogutud teabe vaatamiseks esmalt Peata jälgimine seanssi keelamisega Logi salvestada Logi ja avage see uuesti Sündmusevaatur või muude tööriistade abil saate teisendada see vastavalt soovile.



## <a name="reaching-out-for-microsoft-support"></a>Microsoft Support jõuda

### <a name="log-collection"></a>Logi saidikogumi

VMM alade kaitse, vaadake kogumiseks vaja logid [ASR Logi saidikogumi tugi diagnostika platvormi (SDP) tööriista abil](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) .

Hyper-V saidi kaitse, laadige alla [tööriist](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) ja käivitada see Hyper-V hosti kogumiseks logid.

VMware/füüsilise stsenaariumid, leiate [Azure'i taastamine Log saidikogumi jaoks VMware ja füüsilise alade kaitse](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) kogumiseks vaja logid.

Tööriista abil kogutud kohalik jaotises CEIP nimega alamkaust jaotises **%LocalAppData%\ElevatedDiagnostics** logid

![Kuulake Hyper-V saidi kaitse kuvatud juhiseid.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>Tugiteenuste Piletite avamine

Tõsta tugi Piletite ASR, jõuda Azure tugiteenuste <http://aka.ms/getazuresupport> URL-i abil

## <a name="kb-articles"></a>KB artiklid

-   [Kuidas säilitada ketas täht kaitstud on nurjunud üle või migreeritud Azure'i virtuaalmasinates](http://support.microsoft.com/kb/3031135)
-   [Kuidas hallata kohapealse Azure'i kaitse võrgu läbilaskevõime kasutuse abil](https://support.microsoft.com/kb/3056159)
-   [ASR: "kobar ressursi ei leitud" tõrge, kui lubamiseks virtuaalse masina kaitse](http://support.microsoft.com/kb/3010979)
-   [Mõista ja tõrkeotsing Hyper-V koopia juhend](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>ASR levinud vigade ja nende lahendused

Allpool on levinud tõrkeid, mis võib tulemus ja nende lahendused. Iga tõrge on eraldi vikilehe dokumenteerida.

### <a name="general"></a>Üldine
-   <span style="color:green;">Uue</span> [Tööde haldamine ei ole viga "toiming on pooleli". Tõrge 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">Uue</span> [Tööd ei ole viga "Server pole Internetiga ühendatud". Tõrge 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Häälestamine
-   [VMM server ei saa registreerida sisemise tõrke tõttu. Vaadake töö vaade saidi taastamise portaalis üksikasjalikumat teavet tõrke. Käivitage uuesti registreerimiseks Serveri install.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Et Hyper-V taastamine halduri vault ei saa ühendust luua. Kontrollige puhverserveri sätteid või proovige hiljem uuesti.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfigureerimine
-   [Ei saa luua kaitse rühma: nimekirja serverid toomisel ilmnes tõrge.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Hyper-V host kobar sisaldab vähemalt ühte staatilise võrguadapteri või pole ühendatud adapterit on konfigureeritud kasutama DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM ei saa toimingu lõpuleviimiseks õigusi](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Ei saa valida konfigureerimisel kaitse tellimuse salvestusruumi konto](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Kaitse
- <span style="color:green;">Uue</span> [Lubamine kaitse vastasel koos tõrge "kaitse ei saanud konfigureerida virtuaalse masina". Tõrge 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">Uue</span> [Lubamine kaitse vastasel koos tõrge "kaitse ei saanud olema lubatud virtuaalse masina." Tõrge 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">Uue</span> [Reaalajas migreerimise tõrge 23848 - virtuaalse masina saab teisaldada Live tüüp. See võiks murda taastamine kaitse oleku virtuaalse masina.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Luba nurjus, kuna majutusseadme pole installitud agenti kaitse](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Ei leia sobivat hosti koopia virtual masina - tõttu madal arvutada ressursid](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Koopia virtual masina sobivat hosti ei leita - loogilise võrku ühendatud tõttu](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Ei saa ühendust luua koopia hosti arvutis - ei saa ühendust luua](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Taastamine
- VMM ei saa lõpule viia host töö-
    -   [Ei õnnestu üle virtuaalse masina jaoks valitud taastamine punkt: üldine juurdepääs keelatud tõrge.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Hyper-V nurjus ei õnnestu üle virtuaalse masina jaoks valitud taastamine punkt: toiming katkestati proovige uuemasse taastamine punkti. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Ei saa serveriga ühenduse loomise (0x00002EFD)
        -   [Hyper-V lubada masina virtual tagant kopeerimine nurjus](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Hyper-V lubada virtuaalse masina virtual masina kopeerimine nurjus](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Kas Kinnita Tõrkesiirde virtuaalse masina jaoks](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Taastamise leping sisaldab virtuaalmasinates, mis ei ole kavandatud Tõrkesiirde](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [Virtuaalse masina ei ole kavandatud Tõrkesiirde valmis](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Virtuaalse masina ei tööta ja pole sisse lülitatud](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Kontorist väljas riba toiming, mis on juhtunud virtuaalse masina ja kinnita Tõrkesiirde nurjus.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Test Tõrkesiirde
    -   [Tõrkesiirde ei saa alustada, kuna test Tõrkesiirde on pooleli.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">Uue</span>  Tõrkesiirde ajalõpp "PreFailoverWorkflow ülesande WaitForScriptExecutionTaskTimeout" tõttu seotud virtuaalse masina või alamvõrgu, millele seade kuulub võrgu turberühma konfiguratsiooni sätted. Täpsemat teavet lugege jaotisest ["PreFailoverWorkflow tööülesande WaitForScriptExecutionTaskTimeout"](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) .


### <a name="configuration-server-process-server-master-target"></a>Konfiguratsiooni Server, protsessi Server juhtslaidi Target (sihtkoht)
Konfiguratsiooniserveri (CS), protsessi Server (PS) juhtslaidi Targer (MT)
-   [ESXi host PS/CS majutatakse, lilla ekraan tingitud nurjub VM.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Kaugtöölaua pärast Tõrkesiirde tõrkeotsing
-   Paljud kliendid on ees üle VM Azure nurjunud ühenduse probleemid. [Kasutage tõrkeotsingu dokumendi RDP VM](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Ressursi halduri virtuaalse masina avaliku IP lisamine
Kui nupp **Ühenda** portaalis on hall ja te ei ole ühendatud Azure teekonna või-saidilt VPN-ühendus, peate luua ja määrata oma VM avaliku IP-aadressi, enne kui saate kasutada RDP/SSH. Järgige alltoodud avaliku IP võrgu liides virtuaalse masina.  

![Lisamine avaliku IP võrgu liides nurjus üle virtuaalse masina](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)