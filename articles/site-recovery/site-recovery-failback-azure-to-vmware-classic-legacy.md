<properties 
   pageTitle="Tagasi VMware virtuaalmasinates ja füüsiliste serverite VMware (pärand): Azure'i nurjuda | Microsoft Azure'i" 
   description="Selles artiklis kirjeldatakse, kuidas nurjumise tagasi VMware virtuaalse masina, mis on kopeeritud Azure'i Azure saidi taastamine." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Nurjuda tagasi VMware virtuaalmasinates ja füüsiliste serverite Azure VMware Azure saidi taastamine (pärand)

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-failback-azure-to-vmware.md)
- [Azure'i klassikaline portaal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure'i klassikaline portaali (pärand)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


Azure'i saidi taastamise teenuse aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Masinad saab paljundada Azure'i või sekundaarne asutusesisese andmekeskuse. Kiire ülevaate saamiseks lugege [mis on Azure saidi taastamise?](site-recovery-overview.md)

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas nurjumise tagasi VMware virtuaalmasinates ja Windows/Linux füüsilise serveri Azure'i oma kohapealse saidi, kui olete oma kohapealse saidi kopeeritud Azure.

>[AZURE.NOTE] Selles artiklis kirjeldatakse pärand stsenaariumi. Selles artiklis tuleks kasutada ainult juhiseid, kui te kopeeritud Azure'i [pärand](site-recovery-vmware-to-azure-classic-legacy.md)juhiseid. Kui kopeerimise abil [täiustatud juurutamise](site-recovery-vmware-to-azure-classic-legacy.md) häälestada, siis järgige [selle](site-recovery-failback-azure-to-vmware-classic.md) artikli nurjumise tagasi. 


## <a name="architecture"></a>Arhitektuur

See diagramm näitab Tõrkesiirde ja failback stsenaariumi. Sinine read on kasutatud Tõrkesiirde ajal ühendused. Punased jooned on kasutatud ajal failback ühendused. Nooltega read avage Interneti kaudu.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Enne alustamist 

- Mida peaks ei ole oma VMware VMs või füüsilise serveri ja need peaks töötama Azure.
- Peaks Pange tähele, et saate ainult nurjuda tagasi VMware virtuaalmasinates ja Windows/Linux füüsilise serveri Azure'i virtuaalmasinates VMware kohapealse esmane saidi.  Kui olete puudumisel tagasi füüsilise seadme, Tõrkesiirde Azure teisendage see on Azure VM ja failback, et VMware teisendage see Vmware'i VM.

Kuidas seadistada failback siin.

1. **Häälestamine failback komponendid**: peate vContinuum serveri häälestamine asutusesiseselt, ja osutage konfiguratsiooniserveri VM Azure. Saate ka häälestada protsessi server on Azure VM nimega saatma andmete kohapealse serveri. Protsessi serveri registreerima konfiguratsiooniserveri, mida selle Tõrkesiirde. Saate installida ka kohapealse serveri. Kui teil on vaja mõnda Windows Serveri see on häälestatud automaatselt vContinuum installimisel. Kui teil on vaja Linuxi peate selle häälestada käsitsi eraldi serveris.
2. **Luba kaitse ja failback**: kui olete häälestanud komponendid, vContinuum peate lubamiseks kaitse nurjunud Azure VMs üle. Saate käivitada VMs valmisoleku kontrollimiseks ja Tõrkesiirde pidamine Azure'i kohapealse saidile. Kui failback on lõpule viidud saate reprotect kohapealse masinad nii, et Alustuseks imitatsiooniga Azure'i.



## <a name="step-1-install-vcontinuum-on-premises"></a>Samm 1: Install kohapealse vContinuum

Peate installima vContinuum serveri kohapealne ja osutage konfiguratsiooniserveri.

1.  [VContinuum alla laadida](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Alla laadida [vContinuum uuendatud](http://go.microsoft.com/fwlink/?LinkID=533813) versiooni.
3. Installige uusim versioon vContinuum. Klõpsake lehel **Tere tulemast** nuppu **Järgmine**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  Viisardi esimesel lehel Määrake CX serveri IP-aadress ja portide CX. Valige suvand **Kasuta HTTPS**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  Konfiguratsiooniserveri IP-aadressi leidmine **armatuurlaua** vahekaardi konfiguratsiooniserveri VM Azure.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  Leiate Azure'i konfiguratsiooniserveri HTTPS avaliku pordi konfiguratsiooniserveri VM vahekaardi **lõpp-punktid** .

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. Määrake **CS parool üksikasjade** lehel märgitud alla konfiguratsiooniserveri registreerimisel parool. Kui te ei mäleta selle selle sisse **C:\\Program Files (x86)\\InMage süsteemide\\privaatne\\connection.passphrase** konfigureerimine serveris VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  **Valige sihtkoht** lehel Määrake, kuhu soovite installida vContinuum server ja klõpsake nuppu **Installi**.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Kui installimine on lõpule jõudnud, võite käivitada vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Samm 2: Installida Azure protsessi server 

Peate installima protsessi serveri Azure, et Azure'i VM saate saata andmed tagasi on kohapealse serveri. 

1.  Valige lehel **Konfiguratsiooni Servers** Azure lisada uue protsessi server.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Määrake protsessi serveri nimi ja nimi ja parool ühenduse virtual arvutisse administraatorina. Valige konfiguratsiooniserveri, millele soovite registreerida protsessi server. See peaks olema sama serveri üle oma virtuaalmasinates kaitse ja nurjuda kasutate.
3.  Saate määrata, kus tuleks protsessi serveris juurutatud Azure võrgu. Peaks olema konfiguratsiooniserveri samasse võrku. Määrake kordumatu IP-aadress valitud alamvõrgu ja alustage juurutamise.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  Töö käivitatakse juurutamiseks serveri protsess.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  Pärast serveri protsess on juurutatud Azure'i serveri määratud mandaadi abil saate sisse logida. Registreerida protsessi serveri registreeritud kohapealse protsessi serveri samal viisil. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] Registreeritud ajal failback serverid ei saa saidi taastamise VM atribuudid jaotises nähtav. Need on nähtavad konfiguratsiooni server pole registreeritud vahekaardil **serverid** ainult. See võib võtta umbes 10 – 15 minutit, kuni ta protsessi server kuvatakse menüü.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Samm 3: Installi kohapealse juhtslaidi target serveri

Sõltuvalt teie andmeallika virtuaalmasinates operatsioonisüsteemi, peate installima Linux või kohapealse lisamine Windows Serveri.

### <a name="deploy-a-windows-master-target-server"></a>Mõne Windowsi serveri juurutamine

Windowsi juhtslaidi sihtkoht on juba kombineeritud vContinuum häälestus. Kui installite selle vContinuum, juhtslaidi server on ka samasse arvutisse juurutatud ja konfiguratsiooniserveri registreeritud.

1.  Juurutamise alustamiseks luua tühja masina kohapealse peale, mille soovite taastada VMs Azure'i ESX hostis.

2.  Veenduge, et oleks vähemalt kaks plaati manustatud VM – üks kasutatakse operatsioonisüsteem ja teine kasutatakse säilitus ketas.

3.  Installige operatsioonisüsteem.

4.  Installige soovitud vContinuum serveris. See ka installimine on lõpule jõudnud, serveri.

### <a name="deploy-a-linux-master-target-server"></a>Linux juhtslaidi target serveri juurutamine

1.  Juurutamise alustamiseks luua tühja masina kohapealse peale, mille soovite taastada VMs Azure'i ESX hostis.

2.  Veenduge, et oleks vähemalt kaks plaati manustatud VM – üks kasutatakse operatsioonisüsteem ja teine kasutatakse säilitus ketas.

3.  Installige Linux operatsioonisüsteemi. Juhtslaidi target Linuxi ei tohi kasutada LVM juur- või säilituspoliitika salvestusruum. Linux juhtslaidi target server on konfigureeritud vältimiseks LVM sektsioonid/ketast discovery vaikimisi.
4.  Sektsioonid, mida saate luua:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Viia läbi soovitud allpool postituse enne installimist juhtslaidi target installimisjuhiseid.


#### <a name="post-os-installation-steps"></a>Postituse OS installimisjuhiseid.

Saamiseks iga SCSI Linux virtual arvuti kõvaketta SCSI võib lubada parameetri "kettapuhastusriista. EnableUUID = TRUE "järgmiselt:

1. Sulgege arvuti virtuaalne.
2. Paremklõpsake vasakpoolsel paanil VM > **Muuda sätteid**.
3. Klõpsake vahekaarti **Suvandid** . Valige **Täpsemalt\>üldine üksuse** > **Konfiguratsiooni parameetrid**. **Konfiguratsiooni parameetrid** suvand on saadaval ainult siis, kui seade on välja lülitatud.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Kontrollib, kas rea **abil. EnableUUID** on olemas. Kui see ei ja on seatud väärtusele **väär** Määrake selle väärtuseks **True** (pole tõstutundlik). Kui on olemas ja on seatud tõene, klõpsake nuppu **Loobu** ja testimine SCSI käsk sees Külastajate operatsioonisüsteemi pärast seda, kui see on käivitatud. Kui pole nuppu **Lisa rida**.
5. Lisage ketas. EnableUUID veeru **nimi** . Määrake selle väärtuseks TRUE, kui. Ärge lisage ülaltoodud väärtused koos jutumärke.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Laadige alla ja installige täiendavad paketid

Märkus: Veenduge, et süsteemi on Interneti-ühendus, allalaadimiseks ja installimiseks täiendavad paketid.

\#Yum installi -y xfsprogs perl lsscsi rsync wget kexec-tööriistad

See käsk laadib need 15 paketid CentOS 6.6 hoidla ja installib need.

BC-1.06.95-1.el6.x86\_64. p/min

BusyBox-1.15.1-20.el6.x86\_64. p/min

elfutils kena tegevust jälle-0.158 3.2.el6.x86\_64. p/min

kexec tööriistad-2.0.0 280.el6.x86\_64. p/min

lsscsi-0,23-2.el6.x86\_64. p/min

lzo-2.03-3.1.el6\_5.1.x86\_64. p/min

Perl-5.10.1-136.el6\_6.1.x86\_64. p/min

Perl-mooduli-ühendatav-3,90-136.el6\_6.1.x86\_64. p/min

Perl-Pod-põgenemine-1,04-136.el6\_6.1.x86\_64. p/min
    
Perl-Pod-lihtne-3.13-136.el6\_6.1.x86\_64. p/min

Perl kena tegevust jälle-5.10.1 136.el6\_6.1.x86\_64. p/min

Perl versioon-0,77 136.el6\_6.1.x86\_64. p/min

rsync-3.0.6-12.el6.x86\_64. p/min

käre 1.1.0 1.el6.x86\_64. p/min

wget-1.12-5.el6\_6.1.x86\_64. p/min

Märkus: Kui allika arvuti kasutab Reiser või XFS failisüsteemi root või buutimine seade, siis järgmised paketid peaks olema alla ja installitakse Linux juhtslaidi suunata enne kaitse.

\#CD-le /usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#p/min - ivh kmod-reiserfs-0,0-1.el6.elrepo.x86\_64. pööret minutis reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. p/min

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#p/min - ivh xfsprogs-3.1.1 16.el6.x86\_64. p/min

#### <a name="apply-custom-configuration-changes"></a>Kohandatud konfiguratsioonimuudatuste rakendamine

Enne nende muudatuste rakendamist veenduge, et olete eelmises jaotises, siis tehke järgmist:


1. Kopeerige RHEL 6 – 64 ühendatud Agent kahendarvu vastloodud OS.

2. Selle käsu abil soovitud kahendarvuks ekstrakti: **tar - zxvf \<faili nimi\> **

3. Selle käsu abil anda õigused: \# **chmod 755./ApplyCustomChanges.sh**

4. Skripti: ** \# ./ApplyCustomChanges.sh**. Käivitage skript ainult üks kord serveris. Pärast skripti, uuesti serverisse.



### <a name="install-the-linux-server"></a>Linux serverit installida


1. [Laadige alla](http://go.microsoft.com/fwlink/?LinkID=529757) installifail.
2. Kopeerige fail mõnda sftp kliendi kasuliku teie valitud Linux juhtslaidi Target virtuaalse masina. Vaheldumisi saate logige Linux juhtslaidi target virtuaalse masina ja kasutada wget alla laadida installipaketi esitatud link
3. Logivad Linux juhtslaidi target serveri virtuaalse masina abil on ssh klient teie valik
4. Kui olete ühendatud Azure võrku, mille olete juurutanud oma Linux serveri VPN-ühenduse kaudu kasutada siis server, et leiate virtuaalse masina vahekaart **armatuurlaud** ja pordi 22 ühenduse Linux juhtslaidi target serveri abil Secure Shell sisemine IP-aadress.
5. Kui loote ühenduse Linux serveri avaliku Interneti-ühenduse kaudu kasutada Linux juhtslaidi target serveri avaliku virtuaalse IP-aadress (vahekaardilt virtuaalmasinates **armatuurlaud** ) ja avaliku lõpp-punkti jaoks loonud ssh Linux servder sisse logida.
6. Failide ekstraktimine gzipitud Linux juhtslaidi target serveri Installeri tõrva arhiivi käivitades: *"tõrva – Microsoft – ASR xvzf\_UA\_8.2.0.0\_RHEL6-64\"* kataloogist, mis sisaldab Installeri fail.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Kui ekstraktitud failide kopeerimiseks muuta kataloogi installer mis tar sisu arhiivida olid ekstraktimist. Selle kausta tee käivitada: "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Kui teil palutakse valida valige esmane roll **2 (juhtslaidi Target)**. Jätke muude interaktiivsete installisuvandid vaikeväärtused.
9. Oodata installimise jätkamiseks ja Host Config kasutajaliidese kuvada. Juhtslaidi sarget Linux serveri jaoks kasuliku Hostikonfiguratsioon on käsurea kasuliku. Suurust ei soovitud ssh kliendi kasuliku aken. Kasutage NOOLEKLAHVE **Global** suvand ja vajutage sisestusklahvi (ENTER).

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. Sisestage väljale **IP** (leiate selle **armatuurlaua** vahekaardil konfiguratsiooniserveri VM) konfiguratsiooniserveri sisemise IP-aadress ja vajutage sisestusklahvi ENTER. Sisestage **pordi** **22** ja vajutage sisestusklahvi ENTER. 
11.  Jätke **HTTPS-i kasutamine** nimega **Jah**ja vajutage sisestusklahvi ENTER.
12.  Sisestage parool, mis on loodud konfigureerimine serveris. Kui kasutate kitt kliendi Windowsi seadmes, et ssh Linux virtual arvutis saate Shift + Insert lõikelaua sisu kleepimine. Parool kopeerimiseks klahvikombinatsiooni Ctrl + c. abil kohaliku lõikelauale ja kleepige see Shift + Insert abil. Vajutage klahvi ENTER.
13.  Kasutage paremnoolt liikumiseks sulgemine ja vajutage sisestusklahvi ENTER. Oodake installimise lõpuleviimiseks.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Kui mingil põhjusel te ei saanud registreerida oma Linux serveri konfigureerimine serveris saate teha nii uuesti käivitades selle hosti konfiguratsiooni kasuliku /usr/local/ASR/Vx/bin/hostconfigcli (esmalt peate selle kataloogi juurdepääsu õiguste määramine chmod käivitades mõne administraatori nimega).


#### <a name="validate-master-target-registration"></a>Kinnitage juhtslaidi target registreerimine

Saate kinnitada, et serveri konfiguratsiooni server Azure'i saidi taastamise hoidla edukalt registreerimisel > **Konfiguratsiooniserveri** > **Serveri üksikasjad**.

>[AZURE.NOTE] Pärast registreerumist serveri, kui konfiguratsiooni vead virtuaalse masina on kustutatud, Azure või lõpp-punktid on õigesti konfigureeritud, saate selle põhjuseks kuigi juhtslaidi target konfiguratsiooni tuvastamisel juhtslaidi target juurutamisel Azure'i Azure dndpoints see pole kohapealse juhtslaidi target serveri asutusesisese puhul tõesed. See ei mõjuta failback ja nende vigade võite ignoreerida. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Samm 4: Kaitse virtuaalmasinates, kohapealse saidile

Peate enne, kui teil ei õnnestu tagasi kohapealse saidi VMs kaitsmiseks.

### <a name="before-you-begin"></a>Enne alustamist

Kui VM on Azure üle nurjus, lisab see on lisatööd temp draivi lehe fail. See on eest ketas, mis on tavaliselt ei vaja, oma nurjunud üle VM, kuna see võib juba eesmärk on jätkuvalt lehe faili kettal. Enne alustamist on virtuaalmasinates tagant kaitse, peate tagama, et see ketas on ühenduseta nii, et see ei kaitsta. Seda teha järgmiselt:

1.  Avage arvuti haldamine ja valige mäluhaldus nii, et see on loetletud ketast võrgus ja masina külge.
2.  Valige ajutine ketas masina külge ja valige ühenduseta tuua. 

### <a name="protect-the-vms"></a>Kaitse VMs

1. Azure'i portaalis, märkige ruut tagamaks, et need ei üle virtuaalse masina olekus.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. Käivitage vContinuum teie arvutis.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. Klõpsake nuppu **Uus kaitse** ja valige soovitud toimingu tüüp soovitud 

4.  Uues aknas, et avamiseks valige **OS tüüp** > soovite nurjuda tagasi VMs**Üksikasjade saamiseks** . **Esmane server üksikasjad**, tuvastamine ja valige virtuaalmasinates, mida soovite kaitsta. VMs loendis vCenter hosti nimi, et nad ei enne Tõrkesiirde.
5.  Kui valite virtuaalse masina kaitsmiseks (ja see juba ei ole üle Azure) annab hüpikakna virtuaalse masina kaks kirjet. See on konfiguratsiooniserveri tuvastab kaks eksemplari virtuaalmasinates, selle registreeritud. Peate eemaldada kirje kohapealse VM nii, et saate kaitsta õige VM. Õige Azure VM kirje siin tuvastamiseks saate sisse logida Azure VM ja minge C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc. Faili drscout.conf, tuvastada host ID-ga. Säilita vContinuum dialoogiboksis kirje, mille olete leidnud hosti ID VM. Kõigi muude kirjete kustutamine Valige õige VM võib viidata IP-aadress. IP address vahemiku asutusesisese saab kohapealse VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. Peatage vCenter server virtuaalse masina. Saate kustutada ka VMs asutusesisene.
7. Määrake kohapealse MT serveri soovite kaitsta VMs. Selle tegemiseks serveriga vCenter soovite nurjumise tagasi.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Valige serveri host, millele soovite taastada VM põhjal.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Sisestage iga soovitud virtuaalmasinates suvandi kopeerimine. Selle tegemiseks peate valima mis VMs taastub taastamine pool andmesalve. Tabelis on kokku võetud erinevaid võimalusi, peate esitama iga VM jaoks.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **Suvand** | **Soovitatav väärtus suvand**
    ---|---
    Protsess serveri IP-aadress | Valige protsess server Azure'i juurutatud
    Säilituspoliitika maht MB| 
    Säilituspoliitika väärtus | 1
    Päeva/tunni | Päeva
    Vormindusühtsuse intervall | 1
    Valige target andmesalve | Andmesalve saadaval saidil taastamine. Andmesalve peaks on piisavalt ruumi ja ESX Host, mille soovite taastada virtuaalse masina kättesaadav.


10. Mis virtuaalse masina omandada pärast Tõrkesiirde kohapealse saidi atribuutide konfigureerimine. Järgmises tabelis on kokku võetud atribuudid.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **Atribuut** | **Üksikasjad**
    ---|---
    Võrgu konfigureerimise| Iga tuvastatud võrguadapteri jaoks valige see ja klõpsake nuppu **Muuda** konfigureerida virtuaalse masina failback IP-aadress. 
    Riistvara konfiguratsioon| Määrake CPU ja mälu VM. Sätted saate rakendada kõigi vms, mida soovite kaitsta. CPU ja mälu õigete väärtuste tuvastamiseks saate viidata IAAS VMs rolli suuruse ja arvu määratud mälu ja -vormid.
    Kuvatav nimi | Pärast failback kohapealse, te saate ümber nimetada selle virtuaalmasinates kuvatakse need vCenter laoseisu. Vaikenimi on virtuaalse masina arvuti hosti nimi. VM nime tuvastamiseks saate viidata VM loendisse klõpsake jaotises kaitse.
    NAT konfigureerimine | Allpool käsitletakse

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>NAT sätete konfigureerimine

1. Kaitse on virtuaalmasinates lubamiseks vaja kaks side kanalit luua. Esimene kanal on virtuaalse masina ja protsessi serveri vahel. Selle kanali kogub andmed VM ja saadab selle protsessi server, mis saadab andmed serveri. Kui protsess server ja virtuaalse masina kaitsta on samas Azure virtuaalse võrgus, siis ei peate kasutama NAT sätted. Muul juhul määrata NAT sätteid. Saate vaadata Azure'i protsessi serveri avaliku IP-aadress. 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. Teine kanal on protsess server ja serveri vahel. Võimalus kasutada NAT või mitte sõltub sellest, kas kasutate vastavalt VPN-ühendus või Interneti kaudu suhtlemiseks. Ärge valige NAt, kui kasutate VPN, kuid ainult siis, kui kasutate Interneti-ühendus.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Kui te pole veel kustutatud kohapealse virtuaalmasinates määratletud ja kui te ei suuda tagasi endiselt andmesalve sisaldab vana VMDK siis peate tagamaks, et failback, VM luuakse uus koht. Selleks valige sätted **Täpsemalt** ja määrata mõne alternatiivse kausta **Kausta nimi sätete**taastamiseks. Jätke muude suvandite abil oma vaikesätted. Kausta nimi sätted rakendamine kõikides serverites.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Käivitage valmisoleku kontrollimine tagamiseks on virtuaalmasinates valmis naasta kohapealse kaitsta. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Oodake, kuni see lõpuleviimiseks. Kui see on edukalt kõik VMs saate määrata kaitse lepingu nimi. Klõpsake nuppu **kaitse** alustada.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. Saate jälgida edu vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. **Aktiveerimise kaitse kavandamine** lõppemist etappi pärast saate jälgida VM kaitse saidi taastamine portaalis.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. Saate vaadata, klõpsates VM ja selle edenemist jälgida täpse olek.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Ettevalmistused failback leping

Saate koostada failback leping, kasutades vContinuum nii, et rakendus saate igal ajal tagasi saidile kohapealse nurjus. Need taastamine plaanid on väga sarnane taastamine saidi taastamise lepingud.

1.  Käivitage vContinuum ja valige **Halda lepingute** > **taastada.** Saate vaadata kõik lepingud, millele on kasutatud ei õnnestu üle VMs loetelu. Sama lepingu abil saate taastada.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Valige kaitse leping ja kõik VMs, mida soovite taastada, ei. Kui valite iga VM näete rohkem üksikasju, sh target ESX server ja allikas VM ketas. Klõpsake nuppu **edasi** viisardi taastamine ning valige VMs, mille soovite taastada.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. Saate taastada alusel mitu võimalust, kuid soovitame kasutada **Uusimat silti** ja valige **Rakenda kõik vms** värskeimad andmed arvutist virtual kasutamise tagamiseks.
4. Käivitage selle **valmisoleku kontrollimine.** See kontrollib, kui õige parameetrid on konfigureeritud lubama VM taastamine. Klõpsake nuppu **edasi** , kui kõik kontrollid eduka. Kui ei, märkige ruut Logi ja tõrgete lahendamine.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  **VM** konfiguratsiooni veenduge, et taastamine sätted on õigesti seatud. Kui teil on vaja VM sätete muutmiseks.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Vaadake üle virtuaalmasinates, mis on taastatud ja määrake käsu loendit. Pange tähele, et VMs on loetletud, kasutades arvuti hosti nimi. See võib olla keeruline vastendamine virtuaalse masina arvuti hosti nimi.
Vastendage nimed, minge **armatuurlaua** Azure'i virtuaalmasinates ja kontrollida VM hosti nimi.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Määrake lepingu nimi ja valige **hiljem taastada**. Soovitame hiljem taastada, kuna algne kaitse ei pruugi olla lõpule viidud. 
11. Klõpsake salvestamiseks leping või taastamis käivitamine, kui olete märkinud ruudu taastada nüüd ja mitte hiljem **taastada** . Saate vaadata taastamine olekut ning vaadake, kui leping on salvestatud.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Virtuaalmasinates taastamine

Pärast leping on loodud, saate selle virtuaalmasinates taastada. Enne kontrollige, et selle virtuaalmasinates on sünkroonimise lõpule. Kui kopeerimise olek kuvatakse OK kaitse on lõpule viidud ja RPO lävi on täidetud. Saate kontrollida seisundi VM atribuudid.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Enne, kui annate taastamis Azure'i virtuaalmasinates välja lülitada. See tagab on ei tükeldatud aju ja, et kasutajad juurde ainult ühe taotluse koopia. 


1.  Salvestatud plaani alustada. VContinuum valige **kuvar** lepingud. See on loetletud lepingud, mis on käivitatud.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  Valige leping **taastamine** ja klõpsake nuppu **Käivita**. Saate jälgida taastamine. Pärast VMs lülitanud sisse saate luua ühenduse neile vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Pärast uuesti failback Azure kaitsmine

Pärast lõpetamist failback soovite võib-olla selle virtuaalmasinates uuesti kaitsta. Seda teha järgmiselt:

1.  Veenduge, et töötate virtuaalmasinates asutusesisese ja taotlused, mis on kättesaadav.
2.  Azure'i saidi taastamine portaalis, valige soovitud virtuaalmasinates ja kustutage need. Valige soovitud virtuaalmasinates kaitse keelamiseks. See tagab, et VMs enam kaitstud.
3.  Azure kustutamine nurjunud üle Azure'i virtuaalmasinates
4.  Kustutage vana virtuaalse masina vSpehere kohta. Need on VMs, mida te varem ei saanud üle Azure.
5.  Saidi taastamine portaalis kaitsta VMs, mida viimati ei saanud üle. Pärast seda, kui need on kaitstud saate need lisada taastamis.
 
## <a name="next-steps"></a>Järgmised sammud



- [Lugege](site-recovery-vmware-to-azure-classic.md) imitatsiooniga VMware virtuaalmasinates ja füüsilise serveri Azure täiustatud juurutamise abil.

 
