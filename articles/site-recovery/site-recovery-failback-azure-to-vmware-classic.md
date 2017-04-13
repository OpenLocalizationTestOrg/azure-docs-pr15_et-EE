<properties 
   pageTitle="Nurjuda tagasi VMware virtuaalmasinates ja kohapealse saidi füüsiliste serverite | Microsoft Azure'i"
   description="Teavet ei kohapealse saidi pärast Tõrkesiirde VMware vms ja Azure füüsilise serveri." 
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
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Tagasi VMware virtuaalmasinates ja kohapealse saidi füüsiliste serverite nurjuda

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-failback-azure-to-vmware.md)
- [Azure'i klassikaline portaal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure'i klassikaline portaali (pärand)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



Selles artiklis kirjeldatakse, kuidas tagasi Azure'i virtuaalmasinates Azure kohapealse saidi nurjumise. Järgige selle artikli kui olete valmis oma VMware virtuaalmasinates tagasi nurjumise või Windows/Linux füüsilise serveri pärast seda, kui need on üle nurjus asutusesisese saidi Azure selle [õpetuse](site-recovery-vmware-to-azure-classic.md)abil.



## <a name="overview"></a>Ülevaade

Diagramm kuvab failback arhitektuur selle stsenaariumi.

Kui server protsess on asutusesiseselt ja kasutate mõnda ExpressRoute, kasutage selle arhitektuur.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Kui protsess server on Azure ja teil on ExpressRoute ühendus või VPN, kasutage selle arhitektuur.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Pordid ja selle failback täieliku loendi kuvamiseks architechture skeemi viidata alloleval

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Kuidas failback toimib järgmiselt

- Kui olete Azure'i üle ei, te ei tagasi oma kohapealse saidi mõne järk-järgult:
    - **Etapp 1**: saate reprotect Azure VMs nii, et Alustuseks imitatsiooniga tagasi VMware VMs oma kohapealse saidi töötab. Lubada reprotection liigub VM failback kaitse rühma, mis on loodud automaatselt, kui olete loonud algse Tõrkesiirde kaitse rühma. Kui lisasite oma Tõrkesiirde kaitse rühma taastamine lepingu seejärel failback kaitse rühma ka automaatselt lisatud leping.  Saate määrata, kuidas kavatsete seda ei reprotect ajal tagasi.
    - **Etapp 2**: pärast oma Azure VMs imitatsiooniga oma kohapealse saidi, käivitate ei suuda tagasi: Azure'i nurjumise.
    - **Etapp 3**: kui teie andmed ei ole tagasi, reprotect kohapealse VMs, mida te ei saanud uuesti, nii, et Alustuseks imitatsiooniga Azure'i.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Failback algse või alternatiivse kohta

Kui te ei saanud üle võib nurjuda sama päritolu VM ka siis, kui see on endiselt olemas kohapealse VMware VM. Selle stsenaariumi kuvatakse ainult delta muudatused tagasi nurjus. Pange tähele järgmist.

- Kui te ei saanud üle füüsilise serveri seejärel failback on alati uus Vmware'i VM.
    - Enne puudumisel tagasi füüsilise seadme pidage meeles järgmist.
    - Kaitstud füüsilise seadme tulevad tagasi virtuaalse masina kui VMware tagasi: Azure'i üle nurjus
    - Veenduge, et leida atleast ühte juhtslaidi Target katkestab vajalikud ESX/ESXi hosts, millele soovite failback koos.
- Kui te ei suuda tagasi algse VM on nõutud.
    - Kui VM haldab vCenter server siis juhtslaidi Target ESX host peaks olema juurdepääs VMs andmesalve.
    - Kui VM on mõni ESX hosti, kuid haldab vCenter siis kõvaketta VM peab olema selle MT host juurdepääsetavad andmesalve.
    - Kui teie VM on mõni ESX hosti ja ei kasuta vCenter siis tuleb täita discovery ESX hosti MT enne saate reprotect. See kehtib juhul, kui olete ei ole tagasi füüsilise serveri liiga.
    - Teine võimalus (kui on olemas kohapealse VM) on enne tegemist on failback selle kustutada. Seejärel failback loob uue VM juhtslaidi target ESX hostiks sama Host.
    
- Kui nõutakse failback alternatiivse asukoha andmed sama andmesalve ja sama ESX host kui kohapealse serveri. 


## <a name="prerequisites"></a>Eeltingimused

- Peate VMware keskkond vastama nurjuda VMware VMs ja füüsilise serveri tagasi. Tagasi ei füüsiline server ei toetata.
- Tagasi nurjuda, et teil oleks luua mõne Azure võrgu kaitse häälestamise. Failback peab Azure võrgust, kus asuvad Azure VMs kohapealse saidi VPN- või ExpressRoute loomine.
- Kui VMs, mida soovite ei õnnestu tagasi haldab vCenter server, peate veenduge, et teil on discovery vms vCenter serverites vajalikud õigused. [Lisateavet vt](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Kui hetktõmmiseid esinevad VM nurjub reprotection. Saate kustutada tõmmised või selle ketast. 
- Enne, kui teil ei õnnestu tagasi peate luua komponentide arvu.
    - **Loo protsess server Azure'i**. See on Azure VM, mille peate loomine ja säilitamine käitamise ajal failback. Saate kustutada seadme kui failback on lõpule jõudnud.
    - **Loo juhtslaidi target server**: serveri saadab ja võtab vastu failback andmed. Loodud kohapealse management server on installitud vaikimisi juhtslaidi target serveri. Siiski, olenevalt sellest nurjunud tagasi liiklus peate failback server eraldi juhtslaidi sihtrakenduse loomiseks.
    - Kui soovite luua täiendava juhtslaidi target server töötab Linux, peate häälestamine Linuxi enne installimist serveri, nagu allpool kirjeldatud.
- Konfiguratsiooniserveri on nõutav kohapealse, kui tegemist on failback. Ajal failback, virtuaalse masina kuuluma puudumisel, millised failback ei saa eduka konfiguratsiooni serveri andmebaasi. Seega veenduge, et võtta regulaarselt ajastatud varundus serveri. Puhul katastroof, peate selle taastamine sama IP-aadressiga, et failback ei tööta.

## <a name="set-up-the-process-server-in-azure"></a>Protsessi server Azure'i häälestamine

Peate installima protsessi server Azure'i, et Azure VMs saab saata andmed tagasi kohapealse serveri. 

1.  Saidi taastamine portaalis > **Konfiguratsiooni serverid** valige lisada uue protsessi server.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Määrake protsessi serveri nimi ja sisestage nimi ja parool, mida kasutate ühendamiseks Azure VM administraatorina. Valige kohapealse management serveri **Konfiguratsiooniserveri** ja määrata, kus tuleks protsessi serveris juurutatud Azure võrgu. See peaks olema võrgu Azure VMs asuvad. Määrake kordumatu IP-aadress, valige alamvõrgu ja alustage juurutamise.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    Käivitatakse töö, mida soovite protsessi serveri juurutamine

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Pärast protsessi serveri juurutamist Azure saab sisse logida kasutades saate määratud mandaadid. Esimest korda, kui logite dialoogis protsessi server töötab. Tippige oma parool ja kohapealse management serveri IP-aadress. Jätke pordi 443 vaikesäte. Võite ka jätta vaikimisi 9443 port andmete kopeerimise kui te konkreetselt selle sätte kohapealse management serveri häälestamisel. 

    >[AZURE.NOTE] Server ei ole nähtav, klõpsake jaotises **VM atribuudid**. See on saadaval ainult vahekaardil **serverid** haldamine serveris, millele on registreeritud. See võib võtta umbes 10 – 15 minutit protsessi serveri kuvada.


## <a name="set-up-the-master-target-server-on-premises"></a>Juhtslaidi target serveri asutusesisese häälestamine

Serveri saab failback andmeid. Juhtslaidi target serveri installitakse automaatselt kohapealse management server, kuid kui olete puudumisel tagasi palju andmeid võib juhtuda täiendava juhtslaidi target serveri seadistamine. Seda teha järgmiselt:

>[AZURE.NOTE] Kui soovite installida mõne serveri Linux, järgige järgmise toimingu.

1. Kui installite Windows Serveri, avage leht Kiirkäivituse VM, kuhu olete installimise serveri, ja laadige installifail Azure saidi taastamise ühendatud Setup viisardit.
2. Installiprogramm ja valige **Lisa täiendavad protsessi serverid mastaapimiseks juurutamise välja** **enne alustamist** .
3. Viisardi lõpuleviimine samamoodi kui tegite [management serveri häälestamine](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Määrake lehel **Konfiguratsiooni serveri üksikasjad** selle serveri ja parooli juurdepääs VM IP-aadress.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Kui serveri Linux VM häälestamine
Häälestada töötava serveri nimega Linux VM soovitud protsenti installimiseks peate management serveri) operatsioonisüsteem, S 6.6 minimaalsete tuua iga SCSI kõvaketta SCSI ID-d, installige mõned täiendavad paketid ja kohandatud muudatusi rakendada.

#### <a name="install-centos-66"></a>Installige CentOS 6.6

1.  Installige CentOS 6.6 minimaalsete operatsioonisüsteem management server VM. Hoidke ISO DVD ketas ja käivitada süsteem. Vahele jätta meediumi testimine, valige soovitud keel inglise keeles, valige **Tavaline salvestusseadmed**kontrollida, kas kõvakettale ei on kõik olulised andmed ja klõpsake nuppu **Jah**, andmed, mida. Valige võrguadapteri server management serveri hosti nimi.  **Redigeerimise süsteemi** dialoogiboksis valige** ühendust automaatselt** ja lisage staatiline IP-aadress, võrgu ja DNS-i sätted. Määrake ajavööndi ja root parool juurde pääsemiseks management server. 
2.  Kui teil paluda tüüpi installi soovite märkige **Loomine kohandatud paigutuse** sektsioon. Pärast nupu valige **tasuta** ja klõpsake nuppu Loo **järgmise** . Looge **/**, **/var/crash** ja **/ home sektsioonid** **FS tüüp:** **ext4**. Vaheta partion nimega loomine **FS tüüp: Vaheta**.
3.  Kui olemasolev seadmed on leitud, kuvatakse hoiatusteade. Klõpsake nuppu **Vorming** seade partition sätete abil vormindada. Klõpsake **kirjutada ketta muudatuse** partition muudatuste rakendamiseks.
4.  Valige **Installi buutimine laadur** > **järgmise** installida buutimine laadur juurkausta partition.
5.  Kui installimine on lõpule viidud klõpsake **arvuti taaskäivitada**.


#### <a name="retrieve-the-scsi-ids"></a>Saate tuua SCSI ID-d

1. Pärast installimist saate tuua VM iga SCSI kõvaketta SCSI ID-d. See management serveri VM Sule tegemiseks VM atribuutide VMware paremklõpsake VM kirje > **Muuda sätteid** > **Suvandid**.
2. Valige **Täpsemalt** > **Üldine üksus** ja klõpsake **Konfiguratsiooni parameetrid**. See suvand on de-active siis, kui seade töötab. Selle aktiveerimiseks peate sulgema seade.
3. Kui rida **ketas. EnableUUID** olemas veenduge, et väärtuseks on seatud **tõene** (tõstutundlik). Kui see on juba saate tühistada ja testida SCSI käsk sees Külastajate operatsioonisüsteemi pärast seda, kui see on käivitatud. 
4.  Kui rida ei olemasoleva klõpsake nuppu **Lisa rida** – ja lisage see **tegelik** väärtus. Ärge kasutage jutumärke.

#### <a name="install-additional-packages"></a>Installige pakette

Peate alla laadima ja installima mõned täiendavad paketid. 

1.  Veenduge, et serveri on Internetiga ühendatud.
2.  Selle käsu alla laadima ja installima 15 pakettide CentOS hoidla: **# yum installimine – y xfsprogs perl lsscsi rsync wget kexec-tööriistad**.
3.  Kui allika masinad olete kaitsmine töötavad Linux wit Reiser või XFS failisüsteemi root või buutimine seade, siis peaks alla laadida ja installida pakette järgmiselt:

    - # <a name="cd-usrlocal"></a>CD-le /usr/local
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>p/min – ivh kmod reiserfs-0,0 1.el6.elrepo.x86_64.rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>p/min – ivh xfsprogs-3.1.1 16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Kohandatud muudatused rakendada

Tehke järgmist kohandatud muudatused rakendada, kui olete juhiste pärast installi ja installitud paketid:

1.  Kopeerige RHEL 6 – 64 ühendatud Agent kahendarvu VM. Selle käsu abil soovitud kahendarvuks ekstrakti: **tar – zxvf <file name> **
2.  Selle käsu abil anda õigused: **# chmod 755./ApplyCustomChanges.sh**
3.  Skripti: **#./ApplyCustomChanges.sh**. Skripti peaks töötama ainult üks kord. Taaskäivitage server pärast skripti edukalt.


## <a name="run-the-failback"></a>Funktsiooni failback käivitamine

### <a name="reprotect-the-azure-vms"></a>Azure'i VMs reprotect

1.  Saidi taastamine portaalis > **masinad** menüü valige VM, mis on antud nurjus üle ja seejärel klõpsake käsku **kaitse uuesti**.
2.  Tekstivormingus **Juhtslaidi Target serveri** ja **Protsessi** valige kohapealse serveri ja Azure VM protsessi server.
3.  Valige konto, saate konfigureerida ühenduse VM.
4.  Valige jaotises kaitse failback versioon. Näiteks kui VM on kaitstud, siis PG1 teil vaja PG1_Failback valimiseks.
5.  Kui soovite taastada alternatiivse asukoha määramine, valige säilituspoliitika ketas ja andmesalve serveri jaoks konfigureeritud. Kui teil ei õnnestu tagasi kohapealse saidi VMware VMs failback kaitse leping kasutab sama andmesalve serveri. Kui soovite taastada koopia Azure VM sama kohapealse VM siis kohapealse VM olema sama nimega serveri andmesalve. Kui ei ole VM kohapealse luuakse uus reprotection ajal.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Pärast nupu **OK** reprotection töö alustamiseks hakkab kohapealse saidi Azure VM korrata. **Klõpsake vahekaarti** saate edenemist jälgida.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Käivitage Tõrkesiirde kohapealse saidile

Pärast reprotection VM teisaldatakse failback versiooni selle kaitse rühma ja lisatakse automaatselt taastamine leping, mida kasutasite Tõrkesiirde Azure, kui see on olemas.

1.  Lehe **Taastamine lepingute** valige sisaldav failback rühma taastamine leping ja klõpsake nuppu **Tõrkesiirde** > **Planeerimata Tõrkesiirde**.
2.  **Kinnitage Tõrkesiirde** kinnitage Tõrkesiirde suunda (Azure'i) ja valige taastamine punkti, mida soovite kasutada Tõrkesiirde (uusim). Kui olete lubanud **Mitme-VM** konfigureerimisel dispersioonanalüüs atribuudid saate taastada rakenduse uusim versioon või krahh ühtsete taastamine punkti. Klõpsake märkeruutu selle Tõrkesiirde käivitamiseks.
3.  Azure VMs sulgub Tõrkesiirde ajal saidi taastamine. Kui märgite ruudu selle failback on lõppenud, saate ootuspäraselt saate saate kontrollida, et Azure VMs on sulgeda ootuspäraselt.

### <a name="reprotect-the--on-premises-site"></a>Kohapealse saidi reprotect

Pärast failback lõpulejõudmist andmete kohapealse kohapeal uuesti, kuid ei saa kaitstud. Azure'i imitatsiooniga alustamiseks uuesti teha järgmist:

1.  Saidi taastamine portaalis > **masinad** menüü Vali VMs, mis ei ole tagasi ja seejärel klõpsake käsku **kaitse uuesti**. 
2.  Pärast selle kopeerimine, et kontrollida Azure'i ei tööta oodatud viisil, saate kustutada Azure'i VMs (praegu ei tööta) Azure'i, mis olid tagasi nurjus.


### <a name="common-issues-in-failback"></a>Levinud probleemid failback

1. Kui soovite teha kirjutuskaitstud vCenter avastamine kaitse virtuaalmasinates, see ei õnnestu ja Tõrkesiirde töötab. Reprotect ajal ei õnnestu, kuna selle datastores ei saa avastanud. Kui sümptom ei kuvata datastores, mis on loetletud samas uuesti kaitsta. Probleemi lahendamiseks saate värskendada vCenter mandaati vastav kontoga, mis on õigused ja uuesti tööle. [Lisateave](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Kui failback Linux VM ja käivitage see kohapeal, näete, et võrgu Manager pakett arvutist desinstallida. See on VM on taastatud Azure, eemaldatakse võrgu Manager pakett.
3. Kui VM on konfigureeritud staatiline IP-aadress ja üle ebaõnnestus Azure, omandatud DHCP IP-aadress. Kui teil ei õnnestu üle tagasi kohapeal, VM jätkuvalt DHCP abil saate hankida IP-aadress. Peate käsitsi login arvuti ja IP-aadressi staatilise aadressi vajadusel uuesti määramine.
4. Kui kasutate kas ESXi 5,5 tasuta edition või vSphere 6 Hypervisor tasuta väljaande, Tõrkesiirde õnnestub, kuid failback ei õnnestu. Saate küll ned kummagi hindamise litsentsi lubamiseks failback uuendada.

## <a name="failing-back-with-expressroute"></a>Ta ei ole tagasi ExpressRoute

Teil võib nurjuda tagasi üle VPN-ühendus või Azure'i ExpressRoute. Kui soovite kasutada ExpressRoute Märkus järgmist:

- Millised allikale masinad fail üle ja millised Azure VMs asuvad ilmnemisel on tõrkesiire Azure virtuaalse võrgus tuleks luua ExpressRoute.
- Andmed on kopeeritud Azure storage konto avaliku lõpp-punkti. Seadistage peaks avaliku silmitsemine ExpressRoute target data Centeri kaudu saidi taastamine kopeerimise ExpressRoute kasutama.



