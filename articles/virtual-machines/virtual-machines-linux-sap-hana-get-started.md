<properties
   pageTitle="Käsitsi installimine HANA SAP-i juhend Kiirjuhend Azure VMs | Microsoft Azure'i"
   description="Käsitsi installimine SAP-i HANA Azure VMs Kiirjuhend juhend"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Käsitsi installimine ühekordse SAP-i HANA kohta Azure VMs Kiirjuhend juhend

## <a name="introduction"></a>Sissejuhatus

Selle juhendi Kiirjuhend abil häälestada ühekordse SAP-i HANA prototüüp/demo süsteemi Azure VMs, käsitsi installimine SAP NetWeaver 7.5 ja SAP-i HANA SP12.
Juhend eeldab, et lugeja tuttavat Azure'i IaaS põhitõdesid nagu juurutamise virtuaalmasinates või virtuaalne võrkude kas kaudu Azure portaali või PowerShelli/CLI võimalust json mallide kasutamise kohta. Lisaks on tõenäoline, lugeja tunneb SAP-i HANA, SAP NetWeaver ja kuidas seda installida kohapealse.

On tõenäoline, mis lugeja andmetel üldine SAP-Azure'i dokumentatsiooni jaotises üldist teavet selle artikli lõpus.

Tõttu mitte tekitamiseks süsteemid piirang on sellest juhendist ei hõlma teemade HA, nt varundamine DR, suure jõudlusega või teisiti turvakaalutlused.

Valimi häälestamine on tehtud abil kahe virtuaalmasinates täita jaotatud SAP NetWeaver installi Azure'i ressursihaldur mudeli kaudu, kui SAP-Linux-Azure'i toetatakse ainult Azure ressursihaldur ja mitte klassikaline mudel. Lingid täiendava teabe Azure'i ressursihaldur kohta leiate käesoleva artikli lõpus jaotises üldist teavet.

Need olid kaks testi VMs valimi installimiseks kasutada:

* Hana-appsrv (tüüp DS3) majutada NW 7.5 ASCS eksemplari + PAS
* Hana-dbsrv (tüüp GS4) Host HANA SP12
* nii VMs kuulus üks Azure virtuaalse võrk (azure-hana-testi-vnet)
* Mõlemal juhul OS oli SLES 12 SP1


Olema teadlik küll, et juuli 2016 seisuga SAP-i HANA ainult täielikult toetatud OLAP-i kohta tootmissüsteemide Azure VM tippige GS5 jaoks. Katsetamiseks, kui see pole ootate ametlik SAP-i tugiteenuste see on hea kasutada midagi väiksem, nagu näiteks GS4.
Mida tuleks alati kasutada SAP-i HANA Azure on Azure premium mälu HANA andmed ja log failide - kohta leiate jaotise "Kettapuhastusriista Setup" veel alla. Seoses täiendavad üksikasjad kohta, millised SAP-i tooted on toetatud Azure märkige jaotises Üldteave käesoleva artikli lõpus.


Juhend kirjeldab käsitsi installida Azure VMs SAP-i HANA kahel viisil:

* SAP-i HANA kaudu SAP-i tarkvara ettevalmistamise Manager (SWPM) jaotatud NetWeaver installi "eksemplari" etapis osana installida
* SAP-i HANA HANA elutsükli Manager tööriista hdblcm abil installida ja seejärel installige NetWeaver hiljem

Samuti on võimalik kasutada SWPM ja installige kõik komponendid (SAP-i HANA SAP-i rakenduste serveri ASCS eksemplari, SAP-i GUI) ühe ühe VM. See suvand pole selles artiklis kirjeldatud, kuid üksused, mis tuleb on samad.

Enne alustamist installi tõlgendatakse vältida mitme lihtsa vigu, mis juhtub, kui kasutate ainult vaikimisi Azure VM konfiguratsiooni pärast selle all tutvumissaitide Azure test VMs häälestamise kohta.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Kontroll-loend SAP-i HANA installi SWPM SAP-i kaudu

See on lihtne kontroll-loendi üksused, mis on seotud käsitsi ühekordse SAP-i HANA installimine demo või prototüüpimine pursposes SAP-i SWPM tehes jaotatud SAP-i NW 7.5 kaudu installida. Üksikute üksuste on selgitatud ja pildid kogu artiklis täpsemalt vormil kuvatud.

* Azure virtuaalse võrgu, mis sisaldab kahte testi VMs loomine 
* kahe Azure VMs OS SLES 12 SP1 kaudu Azure'i ressursihaldur mudeli juurutamine 
* lisada kaks kindlad plaati rakenduse server VM (nt 75GB ja 500GB)
* lisada neli plaati HANA DB server VM - 2 standard salvestusruumi ketast, nt rakenduse serveri VM + 2 premium salvestusruumi ketast (nt 2x512GB)
* sõltuvalt sellest, suurus ja/või läbilaskevõime nõuete lisada mitu ketast ja loomine musta Triibutatud mahud kas OS tasemel sees VM lvm või mdadm abil
* luua XFS failisüsteemi manustatud kettale ja loogiline mahud
* Ühendage uue XFS failisüsteemi OS tasemel. Ühe failisüsteemi abil saate hoida SAP-i tarkvara ja teine näiteks sapmnt directory ja võib-olla varukoopiad. SAP-i HANA DB serveri mount on XFS faili süsteemid premium salvestusruumi kettale /hana ja see on kõik vajalikud vältimiseks juurkausta failisüsteemi, mis pole liiga suur Linuxi Azure VMs täitub /usr/sap
* Sisestage/jne/hosts test VMs kohalik ip-aadressid
* Sisestage nofail parameetri /etc/fstab
* seadmine tuum parameetrid HANA-SLES-12 SAP-i Märkus (vt üksikasjad veelgi allapoole tuum paramters jaotis)
* Vaheta ruumi lisamine
* Kui - installida test VMs graafiline töölaud. Muul juhul kasutatakse remote sapinst installimine
* SAP-i teenuse turuplatsilt SAP-i tarkvara allalaadimine
* installige rakenduse server VM SAP-i ASCS eksemplari
* sapmnt kataloogi vahel test VMs NFS kaudu ühiskasutusse andmine (rakenduse serveri VM on NFS-server)
* Installige andmebaasi eksemplar, sh DB serveris VM HANA SWPM kaudu
* installige rakenduse server VM PASI
* Käivitage SAP-i MC ja ühendada, nt SAP-i GUI / HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Kontroll-loend SAP-i HANA installi hdblcm kaudu

See on lihtne kontroll-loendi üksused, mis on seotud käsitsi ühekordse SAP-i HANA installimine demo või prototüüpimine pursposes SAP-i SWPM tehes jaotatud SAP-i NW 7.5 kaudu installida. Üksikute üksuste on selgitatud ja pildid kogu artiklis täpsemalt vormil kuvatud.

* Azure virtuaalse võrgu, mis sisaldab kahte testi VMs loomine 
* kahe Azure VMs OS SLES 12 SP1 kaudu Azure'i ressursihaldur mudeli juurutamine 
* lisada kaks kindlad plaati rakenduse server VM (nt 75GB ja 500GB)
* lisada neli plaati HANA DB server VM - 2 standard salvestusruumi nagu app server VM + 2 premium salvestusruumi ketast (nt 2x512GB)
* sõltuvalt sellest, suurus ja/või läbilaskevõime nõuete lisada mitu ketast ja loomine musta Triibutatud mahud kas OS tasemel sees VM lvm või mdadm abil
* luua XFS failisüsteemi lisatud kettale ja loogiline mahud
* Ühendage uue XFS failisüsteemi OS tasemel. Ühe failisüsteemi abil saate hoida SAP-i tarkvara ja teine näiteks sapmnt directory ja võib-olla varukoopiad. SAP-i HANA DB serveri mount on XFS faili süsteemid premium salvestusruumi kettale /hana ja see on kõik vajalikud vältimiseks juurkausta failisüsteemi, mis pole liiga suur Linuxi Azure VMs täitub /usr/sap
* Sisestage/jne/hosts test VMs kohalik ip-aadressid
* Sisestage nofail parameetri /etc/fstab
* seadmine tuum parameetrid HANA-SLES-12 SAP-i Märkus (vt üksikasjad veelgi allapoole tuum paramters jaotis)
* Vaheta ruumi lisamine
* Kui - installida test VMs graafiline töölaud. Muul juhul kasutatakse remote sapinst installimine
* SAP-i teenuse turuplatsilt SAP-i tarkvara allalaadimine
* luua rühma "sapsys" HANA DB serveri VM rühma id 1001
* SAP-i HANA installimine DB serveri VM hdblcm abil
* installige rakenduse server VM SAP-i ASCS eksemplari
* sapmnt kataloogi vahel test VMs NFS kaudu ühiskasutusse andmine (rakenduse serveri VM on NFS-server)
* Installige andmebaasi eksemplar, sh DB serveris VM HANA SWPM kaudu
* installige rakenduse server VM PASI
* Käivitage SAP-i MC ja ühendada, nt SAP-i GUI / HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Käsitsi installimine SAP-i HANA Azure VMs ettevalmistamine

See peatükk kohta Azure VMs ettevalmistamine käsitsi installimine SAP-i HANA koosneb viiest osast, mis hõlmab järgmisi teemasid:

* Ketas häälestamine
* Tuum parameetrid
* Failisüsteemi
* / jne/hosts
* / jne/fstab


### <a name="disk-setup"></a>Ketas häälestamine

Juurkausta failisüsteemi Linux VM Azure on piiratud. Seetõttu tuleb lisada kettaruumi VM töötab SAP-i. Puhul VM kasutatud puhas prototüüp/demo keskkonnas SAP-i rakenduse server pole midagi Azure standard salvestusruumi ketast kasutada. SAP-i HANA DB andmete ja logide failide - Azure Premium salvestusruumi ketast tuleks kasutada ka mitte tekitamiseks horisontaalpaigutus.

Kuidas manustada ketast Linux VM mõned üksikasjade kuvamiseks [siin](virtual-machines-linux-add-disk.md)

Kui tegemist on Azure ketta vahemällu – üks kasutada "Pole" jaoks ketast, mida kasutatakse HANA tehingu Logide salvestamine. See on ok kasutada andmefailid lugege HANA vahemällu. HANA on-mälu andmebaasi, mis see sõltub üldist kasutus mudelit palju Loe vahemälu Azure'i ketas tasemel parandab jõudlust (nt alates HANA ja andmete lugemine kettalt mällu).

Azure Premium Storage üksikasjade kuvamiseks [siin](../storage/storage-premium-storage.md)

[Siit](https://github.com/Azure/azure-quickstart-templates) leiate json Näidismallid VMs loomiseks.
"101-vm-lihtne-linux" näitab, kuidas näeb põhimalli sh salvestusruumi jaotis, mis lisab 100GB andmed kettale.

[See artikkel](virtual-machines-linux-sap-on-suse-quickstart.md) sisaldab teavet selle kohta, kuidas leida SUSE pildi PowerShelli või CLI kaudu ja tähtsuse manustamiseks kettal UUID kaudu.


Sõltuvalt süsteemi ja läbilaskevõime nõuete suurust osutuda vajalikuks lisada mitu ketast ühe asemel ja hiljem luua triip üle nende ketast OS tasemel seadmine. Need on kaks aspekte, miks ühte looks triip üle mitme Azure'i ketast määramine:

* läbilaskevõime suurendamine
* ühe failisüsteemi > 1TB on vaja praegust suuruspiirangut Azure'i ketas on 1TB (alates juuli 2016)


Kaks peamist tööriistad striping konfigureerimiseks kohta saate lisateavet leiate siit:

[Artiklit konfigureerida Linux tarkvara ratsiasta on Azure VM mdadm kasutamise kohta](virtual-machines-linux-configure-raid.md)

[Artiklit Linux Azure'i VM loogilise helitugevuse halduri konfigureerimise kohta](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

Testi keskkonnas kaks Azure standard salvestusruumi plaati olid lisatud SAP-i app server VM. Üks kasutatud salvestada SAP-i tarkvara installimiseks (nt NetWeaver 7.5, SAP-i GUI, SAP HANA... ) ja teine on piisavalt ruumi, mis võivad olla vajalikud (nt varukoopia, katse) ning sapmnt kataloogi (nt SAP-i profiil) vahel kõik VMs, mis kuuluvad sama SAP-i horisontaalpaigutus.

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

Vastuolus app server VM nelja ketast on lisatud HANA SAP-i server VM. Nagu enne kaks plaati kasutati SAP-i tarkvara (üks ka jagada SAP-i tarkvara ketas NFS kaudu) ja teil on piisavalt ruumi, nt varukoopia. Täiendavad kaks plaati olid Azure Premium salvestusruumi ketast hoida SAP-i HANA andmed ja logi faile kui ka /usr/sap kataloogi.


### <a name="kernel-parameters"></a>Tuum parameetrid


SAP-i HANA nõuab teatud Linuxi tuum sätted, mis ei kuulu standard Azure galerii pildid ja tuleb käsitsi määramine. On teatud SAP-i märkme, mis kirjeldab sätteid. 


SAP-i Märkus SAP-i HANA DB: OS Soovitatavad sätted SLES 12 / SLES SAP-i rakenduste 12: [SAP-i Märkus 2205917](https://launchpad.support.sap.com/#/notes/2205917)

Üks täiendav teema reagrding lehe-vahemälu seotud töötab SAP-i HANA SLES kohta leiate [siit](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) peatüki 6.1 tuum: lehekülje vahemälu limiit

Olemas on ka lehekülje vahemälu limiit [SAP-i Märkus 1557506](https://service.sap.com/sap/support/notes/1557506) SAP-i märge

SLES 12 on uue tööriista, mis asendab vana sapconf kasuliku. "Häälestatud adm" ja SAP-i HANA teisiti profiili on saadaval. Üks leiate täpsemat teavet selles tööriistas pärast kaks saamiseks allpool olevaid linke.

SLES dokumentatsiooni häälestatud adm profiili sap-hana kohta leiate [siit](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

SLES dokumentatsiooni häälestatud-ettevõttest profiil sap-hana - Peatükk 6.2 häälestamine süsteemide SAP-i töökoormus koos häälestatud adm - leiate [siit](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Siin saate vaadata, kuidas "häälestatud adm" muudetud on transparent_hugepage kui ka numa_balancing väärtuste kohaselt nõutav HANA SAP-i sätted.


SAP-i HANA tuum sätted püsivalt teha tuleb kasutada grub2 SLES 12. Lisateavet grub2 leiate [siit](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Sellel kuvatõmmisel on kuvatud, kuidas tuum sätted on muutunud config faili ja seejärel "koostatakse" grub2-mkconfig kaudu


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Teine võimalus oleks sätete kaudu Yast ja buutimine laadur tuum parameetri sätteid muuta.


### <a name="filesystems"></a>Failisüsteemi 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Siin näete ühte süsteemide kaks faili, mis on loodud SAP-i rakenduse serveris VM peal kaks manustatud Azure standard salvestusruumi plaati. Mõlema failisüsteemi on tippige XFS ja paigaldatud /sapdata ja /sapsoftware.

See pole kohustuslik seda nii. Seal on erinevaid võimalusi sturcture kettaruumi tehke järgmist.
Kõige olulisem on vältida, et root failisüsteemi otsa ruumi. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

SAP-i HANA DB VM kohta on oluline teada, et andmebaasi installimisel sapinst (swpm) kaudu ja lihtsalt kasutate lihtsa "tüüpilised" installisuvandit see installida asjade vaikimisi jaotises /hana ja /usr/sap. SAP-i HANA varukoopia vaikesäte on näiteks /usr/sap.
Nagu enne kui võti vältimiseks, mis on root failisüsteemi otsa ruumi. Seetõttu üks peaks veenduge, et on piisavalt vaba ruumi /hana ja /usr/sap enne installimist SAP-i HANA swpm kaudu.

SAP-i [selles artiklis](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) kirjeldatakse standard failisüsteemi paigutuse SAP-i HANA 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

SAP NetWeaver standard SLES 12 Azure'i Galerii installimisel tekib teate, et ei ole Vaheta ruumi. See teade vabaneda ühte võib nt käsitsi lisada Vaheta faili, nagu on kirjeldatud selles dokumendis dd, mkswap ja swapon kaudu. Otsi "Lisada vaid Vaheta faili käsitsi" [selle](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html) artikli teemad

Teine võimalus on konfigureerimine Vaheta ruumi Linux VM esindaja kaudu. Lisateavet leiate [siit](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>/ jne/hosts

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Enne alustamist SAP-i installimiseks teise olulisem on lisada hostinimed ja IP-aadressid SAP-i vms muutsite faili. Üks peaks SAP-i VMs ühe Azure virtuaalse võrgustikus juurutada ja seejärel kasutage sisemise IP-aadressid.

### <a name="etcfstab"></a>/ jne/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Testimise ajal ilmnes olema on hea mõte kasutada fstab nofail parameetri lisamine. Kui soovitud ketast oleks VM endiselt tulla ja buutimine käigus ei hanguda. Kuid olge nagu käesoleval juhul täiendavat kettaruumi ei pruugi olla saadaval ja protsesside võib täitke juurkausta failisüsteemi. Juhuks, kui /hana oleks puuduvad SAP-i HANA ei alga, siis üldse.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>Graafilise Gnome töölaua installimine SLES 12

See peatükk koosneb kahest secitions, mis hõlmab järgmisi teemasid:

* Installi Gnome töölaua- ja xrdp SLES 12
* Töötab Java-põhine SAP-i MC SLES 12 Firefoxi kasutamine

Võib kasutada ka alternatiive, nt Xterminal, VNC, kuid selles artiklis kirjeldatakse okt 2016 seisuga ainult xrdp.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Installi Gnome töölaua- ja xrdp SLES 12

Nendele, kes on Microsoft Windows tausta ja soovite kasutada otse SAP-i Linux VMs graafiline töölaua käivitamiseks Firefox, Sapinst, SAP-i GUI, SAP-i MC või HANA Studio ja võib-olla ühenduse kaudu RDP VM Microsoft Windowsi arvuti kaudu on lihtne viis saavutada. Kuigi see võib olla asjakohane, nt tootmise andmebaasi server on puhas prototüüp/demo keskkonnas ok. Siit leiate Azure'i SLES 12 VM Gnome töölaua installimise juhiseid:

installida gnome töölaua (nt aknas kitt) järgmine käsk:

   Zypper -t muster gnome-basic

seejärel installige xrdp VM kaudu RDP ühenduse lubamine.

   Zypper on xrdp

Redigeeri /etc/sysconfig/windowmanager ja määrata vaikimisi windows manager GNOME.

   DEFAULT_WM = "gnome"

Käivitage chkconfig veenduge, et xrdp käivitub automaatselt pärast uuesti. 

  chkconfig-tase 3 xrdp kohta

juhuks, kui on probleeme RDP-ühendus, proovige uuesti (võib-olla välja kitt aken).

  /etc/xrdp/xrdp.sh uuesti

juhuks, kui xrdp uuesti nagu eespool ei tööta sisse, kui seal on .pid faili ja selle eemaldada:

  Kontrollige /var/run ja otsige xrdp.pid   
  eemaldage see ja proovige seejärel uuesti



### <a name="sap-mc"></a>SAP-I MC


Graafilise Java-põhine välja Firefox töötab Azure SLES 12 VM pärast installimist Gnome SAP-i MC alustamiseks saavad dekstop ühte eelmises jaotises kirjeldatud tõrke tõttu puuduvad Java-brauseri lisandmoodul.

SAP-i MC alustamiseks URL on <server>: 5 < instance_number > 13

Lisateavet leiate [siit](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Pildil saate vaadata, kuidas kuvatakse tõrketeade näeb välja kui Java-brauseri lisandmoodul on puudu. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Probleemi lahendamiseks üheks võimaluseks on lihtsalt puuduvate lisandmoodulite Yast kaudu installida.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

Korduv SAP-i MC URL-i avab vähe dialoogiboksi esimene küsitakse selle lisandmooduli aktiveerimiseks.


Ühe täiendava probleemi, mis võib hüpikaknas on kadunud faili kohta kuvatakse tõrketeade: see on väga tõenäoliselt Java 1.8, mis on nõutav SAP-i GUI 7,4 installimisega seotud javafx.properties

Näha läbi Yast IBM Java versioon ei sisalda seda faili. Lahendus on Java laadida Oracle.
Artikkel, mis räägib kindla probleemi kohta leiate [siit](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>Käsitsi SAP-i HANA installimine kaudu SWPM NetWeaver 7.5 installimise käigus


Pildid järgmises loendis on esitatud olulisi juhiseid installimise SAP NetWeaver 7.5 ja SAP-i HANA SP12 SWPM (sapinst) kaudu. NW 7.5 osana installi SWPM on ka installima HANA andmebaasi ühekordsest võimaluste.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

Valimi test keskkonnas ainult ühe ABAP appi server on installitud. Suvand "Jaotatud System" kasutati installima ASCS eksemplar ja esmane rakenduse serveri eksemplari ühes Azure VM ja SAP-i HANA andmebaasi süsteemi mõne muu Azure VM.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Kui ASCS eksemplar on installitud rakendus serveris VM on määratud "roheline" sisse SAP-i MC sapmnt kataloogi, mis sisaldab nt SAP-i profiili kataloogi on SAP-i HANA DB serveriga VM jagatud.
DB installi toimingut tuleb kasutada seda teavet. Parim viis on kasutada NFS, mida on võimalik konfigureeritud Yast.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

Rakenduse peaks serveri VM sapmnt kataloogi NFS kaudu ühiskasutusse antud, kasutades suvandid "rw" ning "no_root_squash". Vaikimisi on "ro" ja "root_squash", mis võivad põhjustada probleeme installimisel andmebaasi eksemplari.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

SAP-i HANA DB serveris VM soovitud sapmnt ühiskasutusse andmine rakenduse serverist VM peab olema konfigureeritud "NFS klient" kaudu (nt abiga Yast)


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Seejärel üks on SAP-i HANA DB VM serverisse sisse logida ja asuge järgmise toimingu jaotatud NW 7.5 installi - "Eksemplari" SWPM.


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Seotud SAP-i HANA install ei ole tegelikult liiga palju sisestamiseks valige "tavaline" install. Lisaks on üks installatiom meediumi tee DB SID, hosti nimi, näiteks arv ja DB Sys administraatori parooli sisestamiseks.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Järgmise sammuna tuleb DBACOCKPIT skeemi sisestage parool.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Siis tuleb SAPABAP1 skeemi parooli küsimus.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Lõpus peaks olema ainult roheline valik tähistatud ette DB installitoimingut iga etapi ja vähe teateboks, kus tuleks öelda "täitmise... Andmebaasi eksemplar on lõpule viidud".

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

Pärast edukat installi ka näitama SAP-i MC DB eksemplari nimega "roheline" ja SAP-i HANA protsesside (nt hdbindexserver, hdbcompileserver) täielik loend


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Sellel kuvatõmmisel on kuvatud jaotises /hana/shared, mis SWPM loodud HANA installimise ajal faili struktuuri osa. Esines puudub ka võimalus sisestage teine tee. Seetõttu on nii oluline kettaruumi jaotises /hana enne installimist SAP-i HANA SWPM vältimiseks, et ruumi otsa root failisüsteemi kaudu ühendada.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Siin võib näha sama, nagu on kirjeldatud enne /usr/sap kataloog.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

Viimases etapis jaotatud ABAP installimine on "Peamise rakenduse serveri eksemplari"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Kui PAS kui ka SAP-i GUI kas teil on installitud vaadata ühte tehingu "dbacockpit", mis kõik tundub paremale installiga HANA SAP-i kaudu.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Viimase järgu võib SAP-i HANA Studio installida rakendus SAP-i serveri VM ja ühenduse SAP-i HANA eksemplari serveris DB VM.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Käsitsi SAP-i HANA installimine HANA elutsükli manager tööriista hdblcm kaudu


Lisaks installimise SAP-i HANA jaotatud installimise kaudu SWPM käigus on ka tuleb kasutada arvutusmeetodit esmalt installima HANA autonoomse hdblcm abil ja seejärel installige nt SAP NetWeaver 7.5 hiljem. Kuvatõmmised allpool loend näitab, kuidas see toimib.

Siin on kolm allikad HANA hdblcm tööriista kohta käiva teabe.

[Õige SAP HANA HDBLCM tööülesande valimine](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[SAP-i HANA elutsükli Haldusriistad](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[SAP-i HANA Serveri install ja Värskenda juhend](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Et vältida probleeme hiljem jaoks rühma id vaikesäte on \<HANA SID\>adm kasutaja (loodud tööriista hdblcm) üks tuleks määratleda rühma nimega "sapsys" rühma ID 1001 enne installimist SAP-i HANA hdblcm abil.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Alates hdblcm esimest korda, on lihtne Avamenüü, kui tegemist on üksuse valimine, 1 "Install uue süsteemi"



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Pildil näete üks klahv Suvandid, mis suunati enne. Tähtis - kataloogide HANA Logi ja andmete maht, samuti Installitee (/ hana/jagatud selle valimi) nimetati ja/usr/SAP-i ei tohiks juurkausta failisüsteemi osa, kuid kuuluvad Azure'i andmed ketast, mis on seotud VM, nagu on kirjeldatud jaotises Azure VM häälestus. See aitab vältida ohtu, et root failisüsteemi võib otsa ruumi.
Üks näete ka, et HANA administraator kasutaja on kasutaja id 1005 ja on osa sapsys rühma (id 1001), mis on määratletud enne installimist.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Üks saate kontrollida selle HANA \<HANA SID\>/jne/parool adm (selles näites azdadm) kasutaja üksikasjad



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

Pärast installimist SAP-i HANA hdblcm abil on näha SAP-i HANA Studios. SAPABAP1 skeem, mis sisaldab näiteks kõik SAP NetWeaver tabelid pole veel saadaval.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Pärast installimist SAP-i HANA ühte saate installida SAP NetWeaver peal. Selles näites seda tehti kasutamine kaudu, nagu on kirjeldatud vastavate ülaltoodud SWPM "jaotatud installimise".
Kui installimisel andmebaasi eksemplari SWPM ühte kaudu lihtsalt on samad andmed, mis enne hdblcm (nt hostname, HANA SID, näiteks arv) ja sisestage. SWPM seejärel olemasoleva HANA installi kasutamine ja lisage veel skeeme.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

See on SWPM installi samm pilt, kui tegemist on sisestage andmed DBACOCKPIT skeemi.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Seejärel SWPM eeldab, et sisestada andmed SAPABAP1 skeemi kohta.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Kui SWPM andmebaasi eksemplari installimine on lõpule jõudnud, võib näha SAPABAP1 skeemi HANA Studios.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

Ja lõpuks pärast installimist SAP-i rakenduse server ja SAP-i GUI üks peaks oskama kinnitamiseks tehingu "dbacockpit" HANA DB eksemplariga.




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Üldist teavet seotud SAP-i Azure kinnitamine, SAP HANA töötavate Azure ja SAP-i tarkvara allalaadimine

* Üldine SAP-i Azure doku kohta, kuidas SAP-i OS Windows Azure klassikaline režiim: [Abil SAP-i virtuaalmasinates Windows Azure kohta](virtual-machines-windows-classic-sap-get-started.md)

* teavet olemasolevate SAP-i mallide kasutamine kliendid: [Azure'i Kiirjuhend malle SAP-i](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* Üldised SAP-i Azure doku kohta, kuidas SAP-i Azure Linux OS Azure'i ressursihaldur mudeli: [Abil SAP-i kohta Linux virtuaalmasinates (VM)](virtual-machines-linux-sap-get-started.md)

* sertifitseeritud SAP-i HANA riistvara kataloog, mis on loetletud, milliste Azure VM on toetatud tootmiseks: [Sertifitseeritud SAP-i HANA® riistvara kataloog](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* teavet virtuaalse masina suurused eriti Linux teenustest: [Azure'i virtuaalmasinates suurus](virtual-machines-linux-sizes.md)

* SAP-i märkme, mis sisaldab loendit kõigist toetatud SAP-i toodete Azure ja Azure VM tüüpi toetatud SAP-i jaoks: [SAP-i Märkus 1928533](https://launchpad.support.sap.com/#/notes/1928533/E)

* SAP-i SAP-i teade "täiustatud jälgimine" Linux VMs Azure: [SAP-i Märkus 2191498](https://launchpad.support.sap.com/#/notes/2191498/E)

* SAP-i HANA pakub Azure "Suur juhtudel". On oluline, et aru saada, et see ei ole SAP-i HANA töötavate Azure VMs, kuid hübriid tühjal metallist serverites töötab keskkonnas, kus SAP-i serverid rakenduse käivitamiseks Azure VMs, kuid SAP-i HANA: [SAP-i Märkus 2316233](https://launchpad.support.sap.com/#/notes/2316233/E)

* SAP-i Märkus SAPOSCOL kohta lisateabe saamiseks Linux: [SAP-i Märkus 1102124](https://launchpad.support.sap.com/#/notes/1102124/E)

* Jälgimine mõõdikute Microsoft Azure SAP-i võti: [SAP-i Märkus 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Teavet Azure'i ressursihaldur: [Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md)

* Teabe juurutamine Linux VMs kaudu Mallid: [Deploy ja Azure ressursihaldur Mallid ja CLI Azure'i virtuaalmasinates haldamine](virtual-machines-linux-cli-deploy-templates.md)

* Juurutamise võrdlus mudelid Azure'i ressursihaldur ja klassikaline vahel: [Azure ressursihaldur vs klassikaline juurutus: juurutuse mudelite ja oma seisundi mõistmine](../resource-manager-deployment-model.md)

* Laadige alla Linux/HANA turuplatsilt SAP-i teenuse NetWeaver 7.5:![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* SAP-i teenuse turuplatsilt HANA SP12 platvormi väljaande allalaadimiseks tehke järgmist.![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

