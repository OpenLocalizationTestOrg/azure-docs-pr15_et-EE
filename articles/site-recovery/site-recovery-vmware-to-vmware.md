<properties
    pageTitle="Juhendist kohapealse VMware virtuaalmasinates või saidi füüsilise serverid | Microsoft Azure'i"
    description="Selles artiklis abil paljundada VMware VMs või Windows/Linux füüsilise serveri saidi Azure saidi taastamine."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Paljundada kohapealse VMware virtuaalmasinates või füüsilise serveri saidi


## <a name="overview"></a>Ülevaade

Azure'i saidi taastamise InMage otsima pakub reaalajas dispersioonanalüüs kohapealse VMware saitide vahel. InMage otsima sisaldub Azure saidi taastamise teenuse tellimused.


## <a name="prerequisites"></a>Eeltingimused

**Azure'i konto**: peate [Microsoft Azure'i](https://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.


## <a name="step-1-create-a-vault"></a>Samm 1: Loo võlvkelder
1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Valige uus > haldus > varundamine ja taastamine saidi (OMS). Teise võimalusena võite klõpsata nuppu Sirvi > taastamise teenuste hoidla > Lisa.
3. Määrake **nimi** sõbralik nimi, mis tähistavad vault. Kui teil on mitu tellimust, valige üks neist.
4. **Ressursirühm** luua uue ressursirühma või valige olemasoleva. Määrake Azure piirkond lõpuleviimiseks nõutavad väljad. 
5.  Valige **asukoht**, geograafilised piirkonna vault. Vaadata toetatud regioonide, leiate [Azure'i saidi taastamise hinnad](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Kui soovite kiiresti juurdepääsu vault armatuurlaualt klõpsake käsku Kinnita armatuurlaua ja seejärel klõpsake nuppu Loo.
6. Uue vault kuvatakse armatuurlaual > ressursside ja peamised taastamine Services keldritel tera.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Samm 2: Vault konfigureerimine ja laadige InMage otsima komponendid
7. Valige oma vault taastamise teenused võlvid tera ja klõpsake nuppu sätted.
8. **Sätted** > **Alustamine** nuppu **Saidi taastamine** > samm 1: **Ettevalmistamine taristu** > **kaitse eesmärk**.
9. **Kaitse eesmärk** taastamine saidi valimine ja valige Jah, kus VMware vSphere Hypervisor. Klõpsake nuppu OK.
10. **Otsima häälestus**nuppu lae abil InMage otsima 8.0.1 GA tarkvara ja registreerimise võti. Installifailid kõik nõutavad komponendid on alla ZIP-faili.


## <a name="step-3-install-component-updates"></a>Samm 3: Komponent värskenduste installimine

Lugege uusimad [värskendused](#updates). Värskenduse faile serverites tuleb installida järgmises järjestuses:

1. RX server, kui see on olemas
2. Konfiguratsiooni serverid
3. Protsessi serverid
3. Juhtslaidi target serverid
4. vContinuum serverid
5. Allikas server (Windows ja Linux Server)

Installige värskendused järgmiselt:

1. [Värskendage](https://aka.ms/asr-scout-update4) ZIP faili alla laadida. See ZIP-fail sisaldab järgmised failid:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - UA update4 bittide RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Väljavõte ZIP-failid.<br>
3. **RX jaoks server**: Kopeeri **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** RX server ja väljavõte. Käivitage ekstraktitud **kaustas/Install**.<br>
4. **Konfiguratsiooni serveri/protsessi server**: Kopeeri **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** konfiguratsiooniserveri ja protsessi server. Topeltklõpsake seda käivitamiseks.<br>
5. **Windowsi jaoks mõeldud juhtlehed target server**: ühendatud agent värskendamiseks kopeerimine **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** serveri. Topeltklõpsake seda käivitamiseks. Pange tähele, et ühendatud agent kehtib ka lähteserveriga. Peaksite installima selle andmeallika serveris kui ka hiljem selles loendis nimetatud.<br>
7. **VContinuum server**: vContinuum server **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** kopeerimiseks.  Veenduge, et olete sulgenud vContinuum viisard. Topeltklõpsake faili selle käivitamiseks.<br>
8. **Linuxi jaoks serveri**: ühendatud agent värskendamiseks kopeerida **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** serveri ja ekstrakti. Käivitage ekstraktitud **kaustas/Install**.<br>
9. **Windowsi jaoks mõeldud allikas server**: ühendatud agent värskendamiseks kopeerimine **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** lähteserveriga. Topeltklõpsake seda käivitamiseks.<br>
10. **Linuxi jaoks andmeallika server**: ühendatud agent värskendamiseks kopeerida vastavate UA faili versiooni Linux server ja ekstrakti. Käivitage ekstraktitud **kaustas/Install**.  Näide: RHEL 6,7 64-bitine server, kopeerige **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** server ja eemaldage see. Käivitage ekstraktitud **kaustas/Install**.

## <a name="step-4-set-up-replication"></a>Samm 4: Häälestamine dispersioonanalüüs
1. Dispersioonanalüüs vahel allika häälestamine ja suunata VMware saidid.
2. Juhised leiate abil toote InMage otsima dokumentatsioon, mis on alla laaditud. Teise võimalusena pääsete dokumentatsiooni järgmiselt:

    - [Väljalaskemärkmed](https://aka.ms/asr-scout-release-notes)
    - [Ühilduvus maatriks](https://aka.ms/asr-scout-cm)
    - [Kasutusjuhend](https://aka.ms/asr-scout-user-guide)
    - [RX kasutusjuhend](https://aka.ms/asr-scout-rx-user-guide)
    - [Kiirülevaate installi juhend](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Värskendused

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure'i saidi taastamise otsima 8.0.1 värskendus 4
Otsima värskendus 4 on koondvärskenduse. See sisaldab kõiki parandusi, kuni update3 ja järgmised uue veaparandused ja täiustatud update1.

**Uue platvormi tugi** 

- VCenter/vSphere 6.0, 6.1 ja 6.2 lisati tugi
- Tugi on lisatud Linux järgmised operatsioonisüsteemid
    - Punane rolli Enterprise Linux (RHEL) 7.0, 7.1 ja 7.2 
    - CentOS 7.0, 7.1 ja 7.2
    - Punane rolli Enterprise Linux (RHEL) 6,8
    - CentOS 6,8

>[AZURE.NOTE]
>
> RHEL/CentOS 7 64-bitise **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** on kaasas base otsima GA paketi **InMage_Scout_Standard_8.0.1 GA.zip**. Laadige otsima GA portaalist nimetatud [1. etapp](site-recovery-vmware-to-vmware.md#Step 1: Create a vault).

**Täiustatud ja veaparandusi** 

- Täiustatud sulgumist töötlemise järgmised Linux operatsioonisüsteemide ja kloonid soovimatute uuesti sünkroonida probleemide vältimiseks.
    - Punane rolli Enterprise Linux (RHEL) 6.x
    - Oracle'i Linux (OL) 6.x
- Linuxi, täitke õiguste ühendatud agent installikaust on nüüd piiratud ainult kohaliku kasutaja juurdepääs.
- Opsüsteemis Windows ajastus välja probleemi emissiooni levinud jaotatud järjepidevuse järjehoidja kohta tugevalt ajal laadida hajutatud rakendusi nagu SQL-i ja ühiskasutus punkti kogumite.
- Lisatud Logi seotud fix CX base installer.
- Windowsi juhtslaidi Target base Installeri lisatakse VMware vCLI 6.0 allalaadimise link.
- Lisada rohkem kontrolli ja võrgu konfiguratsioone muudatuste logide ajal Tõrkesiirde ja DR harjutused.
- Millalgi säilituspoliitika teave on teatatud selle CX.  
- Füüsilise kobar, jaoks helitugevuse suurust toiming kaudu vContinuum viisard ebaõnnestub, kui andmeallika helitugevuse Kahanda juhtunud.
- Kobar kaitse nurjus tõrke "Kettapuhastusriista signatuuri otsimiseks nurjus" kui kobar ketas on PRDM ketas.
- cxps transport serveri krahh out range erandi tõttu. 
- Serveri nimi ja IP-veerud on nüüd tõuketeatised installi vContinuum viisardi lehel suurust.
- RX API täiustused
    - Rakendusel viis Viimane saadaval levinud järjepidevuse (ainult tagatud sildid).
    - Kaitstud seadmete pakub võimsus ja vaba ruumi üksikasjad.
    - Andmeallika serveris pakub otsima draiver olekus. 
    
>[AZURE.NOTE] 
>
>- **InMage_Scout_Standard_8.0.1_GA.zip** base pakett on värskendatud CX base Installeri **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** ja Windowsi juhtslaidi Target base Installeri **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Kõigi uute installi kasutada uut CX ja Windowsi juhtslaidi Target GA bittide.
>- Värskenduse 4 saab otse rakendada klõpsake 8.0.1 ga
>- Uuesti pärast seda, kui need on rakendatud süsteemi ei saa pöörata konfiguratsiooniserveri ja RX värskendused.

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure'i saidi taastamise otsima 8.0.1 Update 3
Värskenduse 3 sisaldab järgmisi veaparandused ja täiustused:

- Konfiguratsiooniserveri ja RX nurjuvad registreerimiseks saidi taastamise hoidla, kui need on taha puhverserverist.
- Tundide, mis ei ole täidetud taastamine punkti eesmärk (RPO) saada ei värskendata aruandes seisund.
- Konfiguratsiooniserveri ei sünkroonita RX ja kui mis tahes UTF-8 märke sisaldavad ESX riistvara üksikasjad ja võrgu üksikasjad.
- Windows Server 2008 R2 domeenikontrollerid ei käivitu pärast taastamine.
- Ühenduseta sünkroonimine ei tööta ootuspäraselt.
- Pärast virtuaalse masina (VM) Tõrkesiirde, dispersioonanalüüs paari kustutamist jääb saatmata CX UI pikka aega ja kasutajad ei saa lõpule viia ning failback või seda jätkata toiming.
- Üldine hetktõmmise vähendamiseks rakendus on optimeeritud toiminguid, mis on tehtud töö järjepidevuse katkestab nagu SQL-i kliendid.
- Vähendades mälukasutus, mida on vaja luua hetktõmmiseid opsüsteemis Windows on täiustatud jõudlus järjepidevuse tööriist (VACP.exe).
- PTT installi krahhid, kui parool on suurem kui 16 märki.
- vContinuum on kontrollimine ja küsimine uue vCenter mandaadi identimisteabe muutmisel.
- Linuxi juhtslaidi target vahemälu manager (cachemgr) on allalaadimine failide serverist protsessi, mille tulemuseks on dispersioonanalüüs sidumiseks pidurdamise.
- Kui füüsilise Tõrkesiirde kobar (MSCS) ketta järjestus on sama sõlme, on seatud dispersioonanalüüs mõne kobar mahtu.
<br/>Pange tähele, et klaster peab olema reprotected see parandus ära.  
- SMTP-funktsioonid ei tööta pärast RX on täiendatud otsima 7.1 otsima 8.0.1 ootuspäraselt.
- Veel argument statistikud on lisatud Logi tagasipööramine selle toimingu täitmiseks võetud aeg jälgimiseks.
- Tugi on lisatud Linux operatsioonisüsteemide allikas server:
    - Punane rolli Enterprise Linux (RHEL) 6 update 7
    - CentOS 6 värskendada 7
- CX ja RX UI saate nüüd kuvada teate paari, mis läheb rasterpilt režiimi.
- RX on lisatud turvalisus probleemi lahendamiseks:

**Probleemi kirjeldus**|**Viisid**
---|---
Luba mööduda parameetri võltsimist kaudu|Piiratud juurdepääs jaotiste kasutajad.
Saitidevaheline taotluse võltsimist|Lehe-luba mõiste, mida iga lehe jaoks genereeritud juhusliku ID-ga rakendada. <br/>Seda, kuvatakse. <li> On ainult ühekordse sisselogimise eksemplari sama kasutaja jaoks.</li><li>Lehe värskendamine ei tööta – see suunab armatuurlaud.</li>
Pahatahtlik faili üleslaadimine|Piiratud failide teatud laiendid. Laiendid on lubatud: 7z, aiff, asf, avi, bmp csv, dokumendi, docx, fla, flv-, gif-faili, gz, gzip, jpeg, jpg, log, mid, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, PDF-i, png, ppt, pptx, pxd, qt, RAM-i, rar, rm, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, XML-i, ja zip.
Püsivate saitidevahelise skriptimise | Lisatud Sisestuskeel valideerimised.


>[AZURE.NOTE]
>
>-  Kõik saidi taastamine värskendused on kumulatiivsed. Värskendus 3 on kõik parandused Update 1 ja Update 2. Värskenduse 3 saab otse rakendada klõpsake 8.0.1 ga
>-  Uuesti pärast seda, kui need on rakendatud süsteemi ei saa pöörata konfiguratsiooniserveri ja RX värskendused.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure'i saidi taastamine otsima 8.0.1 Update (Värskenda 03 dets 15) 2

Paranduste Update 2 on järgmised.

- **Konfiguratsiooniserveri**: lahendada probleemi, et vältida 31 päeva tasuta mõõtmine funktsioon töötamist õigesti, kui konfiguratsiooniserveri registreerimisel saidi taastamine.
- **Ühendatud agent**: lahendada probleemile pole installitud serveri kohta, kui see oli täiendatud versioon 8.0 abil 8.0.1 värskenduse tulemuseks värskendamine 1.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure'i saidi taastamise otsima 8.0.1 Update 1

Värskendus 1 sisaldab järgmist veaparandused ja uued funktsioonid:

- 31 päeva tasuta kaitse kohta serveri eksemplar. See võimaldab teil testida funktsionaalsust või mõne mõiste tõendada häälestamiseks.
    - Server, Tõrkesiirde ja failback, sh kõik toimingud on tasuta esimese 31 päeva, alates aega, mida server on kaitstud esmalt saidi taastamise otsima.
    - Aastast 32 päevast, tuleb tasuda iga kaitstud server standard eksemplari määra Azure saidi taastamise kaitse kliendipoolse saidile.
    - Igal ajal kaitstud serverid, mis on praegu lisandu arv on saadaval Azure saidi taastamise hoidla armatuurlaualehe.
- Tugi lisatud vSphere käsurea liides (vCLI) 5,5 Update 2.
- Lisatud allikas server Linux operatsioonisüsteemide tugi:
    - RHEL 6 värskendada 6
    - RHEL 5 värskendada 11
    - CentOS 6 Update 6
    - CentOS 5 värskenduse 11
- Veaparandused käsitlema järgmisi teemasid:
    - Vault registreerimine nurjub konfiguratsiooniserveri või RX server.
    - Kobar maht ei kuvata õigesti, kui rühmitatud virtuaalmasinates on reprotected, kui nad jätkata.
    - Failback nurjub, kui serveri majutab kohapealse tootmise virtuaalmasinates erinevate ESXi serveris.
    - Otsingukonfiguratsiooni faili õigused muudetakse täiendamisel 8.0.1, mis mõjutab kaitse ja toimingud.
    - Resynchronization lävi pole jõustatud oodatud viisil, mis viib vastuolulised kogemused dispersioonanalüüs käitumine.
    - RPO sätted ei kuvata õigesti konfigureerimine serveris kasutajaliidese. Väärtuse tihendamata andmed kuvatakse valesti tihendatud väärtus.
    -  Eemalda toiming ei kustutata vContinuum viisardi ootuspäraselt ja dispersioonanalüüs ei kustutata konfiguratsiooni serveri kasutajaliidese kaudu.
    -  VContinuum viisardis ketas on valimata automaatselt, kui klõpsate **üksikasjade** vaates ketta MSCS virtuaalmasinates kaitse.
    - Füüsilise-virtuaalne (P2V) stsenaariumi ajal vajalikke HP teenuseid, nt CIMnotify ja CqMgHost, ei teisaldata virtuaalse masina taastamise käsitsi. Selle tulemusena täiendavad buutimine aeg.
    - Linux virtuaalse masina kaitse nurjub, kui seal on rohkem kui 26 ketast serveri.

## <a name="next-steps"></a>Järgmised sammud

Post küsimusi, millele teil on [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
