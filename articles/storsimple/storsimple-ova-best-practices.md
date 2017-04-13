<properties 
   pageTitle="Head tavad StorSimple virtuaalse massiiv | Microsoft Azure'i"
   description="Kirjeldatakse parimaid tavasid juurutamine ja haldamise StorSimple virtuaalse massiiv."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>StorSimple virtuaalse massiiv head tavad

## <a name="overview"></a>Ülevaade

Microsoft Azure'i StorSimple virtuaalse massiiv on integreeritud salvestusruumi lahenduse, mis haldab salvestusruumi tasks kohapealse virtuaalse seade töötab hypervisor ja Microsoft Azure'i salvestusruumi. StorSimple virtuaalse massiiv 8000 sarja füüsilise massiiv tõhusa, kulutõhus alternatiiv. Virtuaalne massiiv hypervisor infrastruktuuri käivitada, toetab nii iSCSI SMB protokollid ja sobib hästi office remote/haru office stsenaariumid. StorSimple lahenduste kohta lisateabe saamiseks minge [Microsoft Azure'i StorSimple ülevaade](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Selles artiklis antakse ülevaade jooksul Alghäälestus, juurutamine ja haldus StorSimple virtuaalse massiivi rakendatud parimaid tavasid. Järgmistest headest tavadest kinnitatud juhised ette seadistamine ja haldamine oma virtuaalse massiiv. Selles artiklis on suunatud IT-administraatorid, kes juurutada ja hallata oma andmekeskuste virtuaalse massiivides.

Soovitame selle korrapärane tagada seade on endiselt vastavuse muudatused meiliteatiste häälestamine või toiming kulgemist paremini. Peaks tekkida probleeme rakendamise järgmistest headest tavadest oma virtuaalse massiivis, abi saamiseks [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) .

## <a name="configuration-best-practices"></a>Konfiguratsiooni head tavad 

Järgmistest headest tavadest katta juhised, mis tuleb järgida Alghäälestus ja virtuaalse massiivi juurutamise käigus. Järgmistest headest tavadest hulka kuuluvad seotud ettevalmistamise virtuaalse masina, rühmapoliitika sätted, sortimine, häälestamiseks on võrgunduse, salvestusruumi kontode konfigureerimine ja osakud ja mahud virtuaalse massiivi loomine. 

### <a name="provisioning"></a>Ettevalmistamise 

StorSimple virtuaalse massiiv on ette valmistatud hypervisor (Hyper-V või VMware) klõpsake virtuaalse masina (VM) hosti serveri. Kui ettevalmistamise virtuaalse masina, veenduge, et teie host on võimalik, et suunata piisavalt ressursse. Lisateabe saamiseks minge [ressursikeskuse minimaalset nõuded](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) ettevalmistamise massiivi. 

Järgmiste juhiste täitmisel rakendada, kui ettevalmistamise virtuaalse massiiv:


|                        | Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Virtuaalne seadme tüüp**   | **Loomine 2** VM kasutamiseks või uuema versiooniga Windows Server 2012 ja *.vhdx* pilt. <br></br> **Genereerimine 1** VM kasutamiseks või uuema versiooniga Windows Server 2008 ja *.vhd* pilt.                                                                                                              | Kasutage virtuaalse masina versiooni 8-11 kasutamisel *.vmdk* pilt.                                                                      |
| **Mälu tüüp**            | **Staatiline mälu**konfigureerida. <br></br> Kasutage suvandi **dünaamilise mälu** .            |                                                    |
| **Ketas andmetüüp**         | Säte nimega **dünaamiliselt laiendamine**.<br></br> **Kindlaksmääratud suurusega** võtab kaua aega. <br></br> Kasutage suvandi **differencing** .                                                                                                                   | Kasutage **õhuke säte** suvandit.                                                                                      |
| **Ketas andmemuudatused** | Laiendamise või kahanemine pole lubatud. Selleks katse, on tulemuseks kõigi kohaliku andmete seadmes.                       | Laiendamise või kahanemine pole lubatud. Selleks katse, on tulemuseks kõigi kohaliku andmete seadmes. |

### <a name="sizing"></a>Suuruse muutmine

Teie StorSimple virtuaalse massiiv sortimisel võtke arvesse järgmisi tegureid.

- Mahud või aktsiad kohalike broneerimiseks. Ligikaudu 12% ruumi on reserveeritud kohaliku taseme iga ettevalmistatud astmeline helitugevust ja ühiskasutus. Umbes 10% ruumi ka reserveeritud failisüsteemi kohalikult kinnitatud maht.
- Hetktõmmis kohal. Umbes 15% ruumi kohaliku taseme on mõeldud pilte.
- Taastab vaja. Kui tehes taastamine uue toiminguga, suurus peaks konto taastamine jaoks vajalik ruum. Taastamine on valmis võrguketta või helitugevuse sama suur või suurem.
- Mõned puhvri eraldada mis tahes ootamatu kasvu jaoks.

Vastavalt eelmise tegureid, sizing nõuded saate esindab järgmist võrrandit:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


Järgmised näited illustreerivad, kuidas suuruse muutmiseks virtuaalse massiiv, oma vajaduste järgi.

#### <a name="example-1"></a>Näide 1:
Oma virtuaalse massiiv, mida soovite saama 

- 2 TB ettevalmistamise astmeline helitugevuse ja ühiskasutusse anda.
- 1 TB ettevalmistamise astmeline helitugevust ja ühiskasutusse anda.
- ettevalmistamise 300 GB kohalikult kinnitatud mahu või ühiskasutusse anda.


Eelneva mahud või osakud, andke meile arvutada kohaliku taseme ruumi nõuetele. 

Esmalt oleks iga astmeline helitugevuse/ühiskasutus kohaliku broneerimiseks võrdne 12% maht/ühiskasutus suurus. Kohalik kinnitatud helitugevuse/ühiskasutus kohaliku broneerimiseks on 10% kohalikult kinnitatud helitugevuse/ühiskasutus suurus (lisaks ettevalmistatud suurus). Selles näites, peate

- 240 GB kohaliku broneerimiseks (2 TB mitmetasandilise helitugevuse/ühiskasutus)
- 120 GB kohaliku broneerimiseks (1 TB mitmetasandilise helitugevuse/ühiskasutus)
- 330 GB kohalikult kinnitatud ruumala või ühiskasutus (10% kohaliku broneerimiseks lisamine 300 GB ette valmistatud suurus)

On nõutavad kohaliku taseme siiani kokku ruumi: 240 GB + 120 GB + 330 GB = 690 GB.

Teiseks läheb vaja vähemalt palju ruumi kohaliku taseme suurim ühe broneerimiseks. Kui teil on vaja taastamine hetktõmmise cloud kasutatakse selle eest summat. Selles näites on suurim kohaliku broneerimiseks 330 GB (sh broneerimiseks failisüsteemi), nii et soovitud 690 GB: 690 GB + 330 GB = 1020 GB lisate.
Kui me järgmise täiendavad taastab, saame alati vaba ruumi eelmise taastetoimingu kaugusel.

Kolmas, läheb vaja 15% siiani talletamiseks kohalikku pilte, teie kokku kohaliku ruumi nii, et ainult 85% on saadaval. Selles näites oleks ümber 1020 GB = 0,85&ast;ettevalmistatud andmete kettale TB. Nii, et oleks andmete ettevalmistatud kettal (1020&ast;(1/0,85)) = 1 200 GB = 1.20 TB ~ 1,25 TB (ümardamine lähima kvartiili)

Faktooring ootamatud growth ja uue taastab, tuleks ettevalmistamise kohalikule kettale, ümber 1,25-1,5 TB.

> [AZURE.NOTE] Soovitame, et kohalikule kettale õhukesed ette valmistatud. See soovitus on Kuna taastamine ruumi on vaja vaid siis, kui soovite taastada andmeid, mis on vanemad kui 5 päeva. Üksusetaseme taastamine saate taastada andmeid ei ole vaja täiendavat ruumi taastamiseks viimase viie päeva jooksul.

#### <a name="example-2"></a>Näide 2: 
Oma virtuaalse massiiv, mida soovite saama 

- 2 TB ettevalmistamise mitmetasandilise maht
- Kohalik kinnitatud 300 GB mahuga ettevalmistamine

Põhineb 12% kohaliku ruumi broneerimiseks astmeline mahud/aktsiate ja 10% kohalikult kinnitatud mahud/aktsiate, läheb vaja

- 240 GB kohaliku broneerimiseks (2 TB mitmetasandilise helitugevuse/ühiskasutus)
- 330 GB kohalikult kinnitatud helitugevuse või ühiskasutus (10% kohaliku broneerimiseks lisamine 300 GB ette valmistatud tühik)

Nõutavad kohaliku taseme kokku kettaruumi: 240 GB + 330 GB = 570 GB

Minimaalse kohaliku ruumi vaja taastamine on 330 GB. 

15% kokku kõvakettal saab talletada pilte, nii et ainult 0,85 on saadaval. Jah, ketta maht on (900&ast;(1/0,85)) = 1.06 TB ~ 1,25 TB (ümardamine lähima quartile) 

Faktooring ootamatut kasvu, saate ettevalmistamise 1,25-1,5 TB kohalikule kettale.


### <a name="group-policy"></a>Rühmapoliitika

Rühmapoliitika on taristu, mis võimaldab teil rakendada teatud konfiguratsioone kasutajatele ja arvutites. Rühmapoliitika sätted sisalduvad rühmapoliitika objektid (GPO), mis on seotud järgmised Active Directory domeeniteenused (AD DS) ümbriste: saidid, domeenid või ettevõtte üksused (organisatsiooniüksused). 

Kui teie virtuaalse massiiv domeeni ühendatud, saab GPO rakendada selle. Nende GPO saate installida rakendusi nagu viirusetõrjetarkvara, saate negatiivset mõju StorSimple virtuaalse massiiv toimimist.

Seetõttu soovitame teil:

-   Veenduge, et teie virtuaalse massiiv on Active Directory jaoks eraldi organisatsiooniüksusega (Browse). 

-   Veenduge, et pole rühmapoliitika objektid (GPO) on rakendatud oma virtuaalse massiiv. Saate blokeerida pärimise tagamaks, et virtuaalse massiiv (lapse sõlme) automaatselt päri mis tahes GPO ema. Lisateabe saamiseks minge [Blokeeri pärimise](https://technet.microsoft.com/library/cc731076.aspx).


### <a name="networking"></a>Võrgunduse

Võrgu konfigureerimise oma virtuaalse array toimub kohalik web Kasutajaliidese kaudu. Virtuaalse võrgu kasutajaliidese kaudu hypervisor, virtuaalse massiiv on ette valmistatud on lubatud. [Võrgu sätete](storsimple-ova-deploy3-fs-setup.md) lehe abil saate konfigureerida virtuaalse võrgu liidese IP-aadress, alamvõrgu ja lüüsi.  Samuti saate konfigureerida esmaseid ja teiseseid DNS-i server, kellaajasätete ja valikuline puhvrisätete seadme. Enamik võrgukonfiguratsioon on ühekordse häälestus. Vaadake läbi [StorSimple võrgu nõuded](storsimple-ova-system-requirements.md#networking-requirements) enne juurutamine virtuaalse massiiv.

Oma virtuaalse massiiv juurutamisel soovitame, et järgite järgmisi head tavad:

-   Veenduge, et võrgu alati juurutatud virtuaalse massiiv paigaldatud võimet suunata 5 Mbps Interneti-läbilaskevõime (või rohkem). 

    -   Interneti läbilaskevõime vajate erineb sõltuvalt teie töökoormus omadusi ja määra andmete muutmine.

    -   Andmete muutmine, mida võib käsitleda on otse teie Interneti-läbilaskevõime. Näiteks kui varukoopia tegemine, 5 Mbps läbilaskevõime majutada andmete muudatuste 18 GB umbes 1 tund. Veel neli korda läbilaskevõime (20 MB), võite hakkama neli korda rohkem andmeid muutmine (72 GB). 

-   Veenduge, et Interneti-ühendus on alati saadaval. Juhuslik või usaldusväärne Interneti-ühendused seadmeid võib kaasa tuua vähenemist juurdepääsu andmetele pilves ja võib kaasa tuua konfiguratsiooni ei toetata.

-   Kui soovite oma seadme nimega serveriks iSCSI juurutada: 
    -   Soovitame keelamise suvand **saada IP-aadress automaatselt** (DHCP). 
    -   Konfigureerige staatiline IP-aadressid. Konfigureerige esmane ja teisene DNS server.

    -   Kui mitme võrgu liidesed määratlemise kohta oma virtuaalne massiiv, ainult esimese võrgu liidese (vaikimisi selle kasutajaliidese on **Ethernet**) jõuavad pilve. Määrata aegu, saate luua mitme virtuaalse võrgu liidesed oma virtuaalse massiivi (iSCSI server on konfigureeritud) ja nende liideste ühendamine teise alamvõrku.

-   Pilveteenuse läbilaskevõime ainult gaasipedaali (kasutada virtuaalse massiiv), pidurdamise ruuteri või tulemüüri konfigureerimine. Kui määrate, et teie hypervisor ahendamine, kuvatakse see throttle kõik protokollid, sh iSCSI ja SMB asemel ainult cloud läbilaskevõime. 

-   Tagada selle aja sünkroonimise hypervisors on lubatud. Kui Hyper-V halduri abil Hyper-V, valige oma virtuaalse massiiv, minge **sätted &gt; Integration Services**, ja veenduge, et märgitud on **sünkroonimise ajal** .

### <a name="storage-accounts"></a>Salvestusruumi kontod

StorSimple virtuaalse massiiv võib olla üks salvestusruumi kontoga seostatud. Selle salvestusruumi konto võib olla automaatselt loodud salvestusruumi konto teenuse, kui ühe tellimuse konto või salvestusruumi kontoga seotud mõnele muule tellimusele. Lisateabe saamiseks lugege teemat Kuidas [hallata oma virtuaalse massiiv salvestusruumi kontod](storsimple-ova-manage-storage-accounts.md).

Kasutada järgmistele soovitustele salvestusruumi kontod seostatud teie virtuaalse massiiv.

-   Kui lingite mitme virtuaalse massiivi ühe salvestusruumi kontoga, tegur maksimaalne maht (64 TB) virtuaalse massiiv ja salvestusruumi konto maksimummaht (500 TB). Seda täissuuruses virtuaalse massiive, mis võivad olla umbes 7 salvestusruumi kontoga seotud arv on piiratud.

-   Kui salvestusruumi uue konto loomine
    -   Soovitame, et luua selle piirkonna kõige lähemal kui teie StorSimple virtuaalse massiiv on juurutatud latentsused minimeerimiseks office remote office/kontorist.

    -   Pidage meeles, et ei saa teisaldada salvestusruumi konto erinevate piirkondade lõikes. Samuti ei saa teisaldada teenuse tellimustes.

    -   Kasutage salvestusruumi konto, mis rakendab koondamise andmekeskuses vahel. Geo-tarbetud salvestusruumi (GRS), Zone liigsete mälu (ZRS) ja kohalikult liigsete salvestusruumi (LRS) on toetatud oma virtuaalse massiivi jaoks. Erinevat tüüpi salvestusruumi kontode kohta lisateabe saamiseks minge [Azure'i salvestusruumi kopeerimine](../storage/storage-redundancy.md).


### <a name="shares-and-volumes"></a>Osakud ja mahud

StorSimple virtuaalse array saate ühtlasi säte, kui see on konfigureeritud failiserverisse ja mahud kui iSCSI server on konfigureeritud. Head tavad loomise osakud ja mahud on seotud suurus ja tüüp, mis on konfigureeritud.

#### <a name="volumeshare-size"></a>Helitugevuse/ühiskasutus suurus

Oma virtuaalse massiivi saate ühtlasi säte, kui see on konfigureeritud failiserverisse ja mahud kui iSCSI server on konfigureeritud. Head tavad loomise osakud ja mahud seotud suurus ja tüüp, mis on konfigureeritud. 

Pidage meeles järgmiste juhiste täitmisel kui ettevalmistamise aktsiate või mahud virtuaalse seadmes.

-   Astmeline ühiskasutus ettevalmistatud mahu faili suhtelisena võib mõjutada tiering jõudlus. Suuremahuliste failide töötamise võib põhjustada aeglane taseme välja. Kui töötate suuri faile, soovitame, et suurim fail on väiksem kui 3% ühiskasutus suurus.

-   Kuni 16 mahud/aktsiate saab luua virtuaalse massiiv. Kui kohalik kinnitatud, saab mahud/aktsiate vahemikus 50 GB kuni 2 TB. Kui mitmetasandilise, mahud/aktsiate peab olema 500 GB kuni 20 TB. 

-   Kui loote maht, oodatud andmete tarbimine kui ka edaspidi kasvab tegurit. Kuigi maht ei saa hiljem laiendada, saate alati taastada suurema mahuga.

-   Kui maht on loodud, ei saa te Kahanda helitugevuse StorSimple suurust.
   
-   Kui kirjalikult astmeline helitugevuse StorSimple, kui andmete maht jõuab teatud lävi (suhtes kohaliku alale maht reserveeritud), IO on rakendus. See maht kirjutamise jätkuva muutub IO märkimisväärselt. Kuigi saate kirjutada astmeline helitugevuse (me aktiivselt lõpetage kirjutamine üksnes ettevalmistatud kasutaja) ettevalmistatud mahust kaugemale, näete teatis teatis selle kohta, mis on teie arvates. Kui näete teadet, on oluline, et võtta parandusmeetmeid kustutada andmete maht või suurema mahuga helitugevuse taastamiseks (helitugevuse laiendamine pole praegu toetatud).

-   Katastroofi taastamine kasutamise juhtudel, nagu on lubatud aktsiate/maht on 16 ja ühtlasi/maht, mida saab töödelda paralleelselt maksimumarv on ka 16, ühtlasi/väljendatud ei ole oma RPO ja RTOs mõjutavad. 

#### <a name="volumeshare-type"></a>Helitugevuse/ühiskasutus tüüp

StorSimple toetab kahte helitugevuse/ühiskasutus tüüpi põhjal kasutamine: kohalikult kinnitatud ja mitmetasandilise. Kohalik kinnitatud mahud/ühtlasi on ette valmistatud paksult tuleks astmeline mahud/aktsiate õhukesed on ette valmistatud. 

Soovitame järgmiste juhiste täitmisel juurutada StorSimple mahud/aktsiate konfigureerimisel:

-   Määratlege helitugevuse tüüp põhineb töökoormus, mida kavatsete juurutada, enne kui saate luua maht. Kasutage töökoormus, mis nõuavad kohaliku garantiid andmete (isegi ajal cloud elektrikatkestus) ja madala cloud latentsused, mis nõuavad kohalikult kinnitatud maht. Kui loote oma virtuaalse massiivi draivi, ei saa muuta draivi tüüp: Kohalik kinnitatud juurde astmeline või *vastupidi*. Näiteks luua kohalikult kinnitatud mahud juurutamisel SQL-i töökoormus või töökoormus majutusteenuse virtuaalmasinates (VM); faili ühiskasutus töökoormus kasutada astmeline maht.

-   Kui tegemist on suur failimaht, märkige ruut suvandi vähem sageli kasutatavate arhiivimine andmeid. Korduste eemaldamise kogumi suuremaks 512 k kasutatakse, kui see on lubatud kiirendamiseks andmeedastus pilveteenusesse.

#### <a name="volume-format"></a>Helitugevuse vorming

Kui olete loonud StorSimple mahud iSCSI serverisse, peate lähtestada, mount ja mahud vormindada. Selle toimingu hosti StorSimple seadmega ühendatud. Järgmiste juhiste täitmisel on soovitatav, kui paigaldus ja mahud hostis StorSimple vormingut.

-   Kiirvormindus sooritada kõik StorSimple maht.

-   StorSimple helitugevuse vormindamisel kasutada määramata ühiku maht (AUS) 64 KB (vaikimisi 4 KB). 64 KB AUS põhineb testimine toimub ettevõttesiseselt levinud StorSimple töökoormus ja muude töökoormus.

-   StorSimple virtuaalse massiivi iSCSI server on konfigureeritud kasutamisel ei kasuta jaotatud mahud või dünaamiline ketast need mahud või ketast ei toeta StorSimple.

#### <a name="share-access"></a>Anda ühiskasutusse

Ühtlasi serverisse virtuaalse massiiv faili loomisel järgige järgmisi juhiseid.

-   Ühiskasutusega loomisel määrata ühe kasutaja asemel administraatorina ühiskasutus kasutaja rühma.

-   Ühiskasutus on loodud, redigeerides aktsiate Windows Exploreri kaudu saate hallata NTFS õigused.

#### <a name="volume-access"></a>Helitugevuse juurdepääs

Teie StorSimple virtuaalse massiivi iSCSI mahud konfigureerimisel on oluline vajaduse korral juurdepääsu kontrollimiseks. Millised host määratlemiseks serverid pääsete juurde maht, loomine ja juurdepääsu juhtimine kirjete (ACRs) seostada StorSimple maht.

Järgmiste juhiste täitmisel StorSimple mahud ACRs konfigureerimisel kasutamine

-   Alati vähemalt üks ACR seostada maht.

-   Mitme ACRs määratleda ainult rühmitatud keskkonnas.

-   Määramisel rohkem kui üks ACR maht, veenduge, et helitugevus on avatud viisil, kus see samaaegselt pääseb rohkem kui üks-rühmitatud host. Kui olete määranud mitme ACRs maht, kuvatakse hoiatusteade hüpikteatis, kuidas teie konfiguratsiooni.

### <a name="data-security-and-encryption"></a>Andmete turvalisus ja krüptimine

Teie StorSimple virtuaalse massiiv on andmete turvalisus ja krüptimine funktsioonid, mis tagavad konfidentsiaalsus ja terviklus andmete. Nende funktsioonide kasutamisel on soovitatav, et järgite järgmisi head tavad: 

-   Määratleda pilveteenuse salvestusruumi krüptovõtme luua AES 256 krüptimist enne, kui andmed on saatnud teie virtuaalse massiiv pilveteenusesse. See võti ei ole vajalik, kui teie andmed on krüptitud alustada. Võti saate loodud ja hoida turvaliste võtme haldamise süsteemi, nt [Azure võtme vault](../key-vault/key-vault-whatis.md)abil.

-   Salvestusruumi konto kaudu StorSimple halduri teenuse konfigureerimisel veenduge, et teil lubada SSL režiimi luua turvalist kanalit võrguühenduse StorSimple seadme ja pilveteenuse vahel kohta.

-   Taastada kontode salvestusruumi klahvid (kasutades Azure salvestusteenus) perioodiliselt kontole juurdepääsemiseks muutuste põhjal administraatorite muudetud loend.

-   Oma virtuaalse massiivi andmed pakitakse kokku ja deduplicated enne saatmist Azure. Me ei soovita andmete korduste eemaldamise rolli teenuse abil oma Windows Server hosti.


## <a name="operational-best-practices"></a>Funktsionaalseid head tavad

Funktsionaalseid põhitõed on juhised, mis tuleb järgida igapäevase või toiming virtuaalse massiivi ajal. Nende toimingute katta teatud haldamise toiminguid, näiteks varukoopiate, taastamine varukoopia põhjal kogum, läbimiseks Tõrkesiirde, desaktiveerimine ja kustutamine massiiv, jälgimine süsteemi kasutus- ja seisundiandmete ja töötab viirusetõrje skannib oma virtuaalse massiivi.

### <a name="backups"></a>Varukoopiate

Andmete oma virtuaalse massiivi varundamise pilveteenusesse kahel viisil, vaikimisi automaatse päev varukoopia kogu seade alates 22:30 või kaudu vajadusel käsitsi varundamise. Vaikimisi seade loob automaatselt iga päev cloud hetktõmmiste kõik andmed elavate seda. Lisateabe saamiseks minge [oma StorSimple virtuaalse massiiv varundada](storsimple-ova-backup.md).

Sagedus ja seostatud vaikimisi varukoopiate säilitamine ei saa muuta, kuid saate konfigureerida aeg, kus iga päev varukoopiate algatatakse iga päev. Algusaja automatiseeritud varufailide konfigureerimisel soovitame:

-   Varukoopiate plaanimine aegsasti. Algusaja varukoopia tuleks ei ühti mitmeid host IO.

-   Kontaktiga nõudmisel käsitsi varundamise seadme Tõrkesiirde või enne hooldustööd akna kaitsta oma virtuaalse massiiv andmeid kavandamisel.

### <a name="restore"></a>Taastamine

Saate taastada varukoopia seadmine kaks võimalust: taastada teise helitugevuse ja ühiskasutusse anda või teha mõne üksuse tasandi taastamine (saadaval ainult virtuaalse massiiv, mis on konfigureeritud failiserverisse). Üksusetaseme taastamine võimaldab teil teha Varundustöö taastamine failide ja kaustade kõigi aktsiate StorSimple seadme pilve varukoopia põhjal. Lisateabe saamiseks minge [varukoopia põhjal taastamine](storsimple-ova-restore.md).

Kui taastamise, pidage meeles järgmisi juhiseid:

-   Ei toeta teie StorSimple virtuaalse massiiv kohapealne taastamine. See aga võib hõlpsasti saavutada kaheosaline toiming: Veenduge, et ruumi virtuaalne massiiv ja seejärel taastada teise helitugevuse/ühiskasutus.

-   Kui taastamine kohaliku helitugevust, pidage meeles, paigutub taastamine toiming. Kuigi maht võib kiiresti veebis, andmeid jätkuvalt olema hüdreeritud taustal.

-   Helitugevuse tüüp jääb samaks taastamine käigus. Astmeline helitugevuse taastatakse teise astmeline helitugevust ja kohalikult kinnitatud helitugevuse teise kohalikult kinnitatud helitugevust.

-   Kui proovite draivi või osa varukoopia põhjal taastamine seadmine taastamine töökohtade nurjumisel target maht või ühiskasutus endiselt luuakse portaalis. On oluline, et kustutada see kasutamata target maht või ühiskasutusse anda portaalis minimeerimiseks tulevaste – see element tekkivaid probleeme.

### <a name="failover-and-disaster-recovery"></a>Tõrkesiirde ja Avariijärgne taaste

Seadme Tõrkesiirde võimaldab teil oma andmete *Allikas* seadme andmekeskuses siirdamiseks mõnda muud seadet *target* , asuvad samas või geograafiline asukoht on muutunud. Seadme Tõrkesiirde on kogu seade. Ajal Tõrkesiirde cloud andmete allikas seadme muutub omandiõiguse target seadme.

StorSimple virtuaalse array saate ainult ei pruugi üle teise virtuaalse massiivi sama StorSimple halduri teenust hallata. Tõrkesiirde 8000 sarja seadmes või massiivi haldab eri StorSimple halduri teenuse (kui see seadme allikas) pole lubatud. Tõrkesiirde kaalutluste kohta lisateabe saamiseks avage [seadme Tõrkesiirde eeltingimuste](storsimple-ova-failover-dr.md).

Kui täita ei suuda üle oma virtuaalse massiiv, pidage meeles järgmist:

-   Kavandatud Tõrkesiirde, on soovitatav heade tavade kõik mahud/aktsiad ühenduseta enne selle Tõrkesiirde tegemiseks. Järgige operatsioonisüsteemi kohased võtta mahud/aktsiate ühenduseta hosti esimese ja seejärel võtta neid ühenduseta virtuaalse seadmes.

-   Faili serveri Õnnetusejärgne taastamine (DR), soovitame liituda target seadme allikana samasse domeeni, et ühiskasutusõigused lahendada. Selles väljaandes on toetatud Tõrkesiirde target seadme sama domeeni.

-   Kui DR on edukalt lõpule viidud, kustutatakse automaatselt allika seade. Kuigi seade pole enam saadaval, virtuaalse masina, mida te ette valmistatud host süsteemi endiselt tarbib ressursse. Soovitame selle virtuaalse masina kustutamine teie hosti süsteemi, mis pärinevad mis tahes kulude vältimiseks.

-   Pange tähele, et selle Tõrkesiirde ei õnnestu, isegi juhul, kui **andmed on alati turvaliste pilveteenuses**. Võtke arvesse järgmist kolme stsenaariumi puhul, kus on tõrkesiire ei viida edukalt lõpuni.

    -   Algne järk-järgult Tõrkesiirde näiteks kui DR enne kontrolli teostamise ilmnes tõrge. Sellisel juhul seadme sihtkoht on veel kasutuses. Te saate uuesti Tõrkesiirde target samasse seadmesse.

    -   Tegelik Tõrkesiirde käigus ilmnes tõrge. Sel juhul target seade on märgitud kasutuskõlbmatuks. Peate ettevalmistamine ja teise target virtuaalse massiivi konfigureerimine ja kasutada Tõrkesiirde.

    -   Selle Tõrkesiirde on lõpule jõudnud, pärast mida allika seade on kustutatud, kuid target seade on probleeme ja te ei pääse andmeid. Andmed on endiselt turvalised pilves ja saab hõlpsalt laadida teise virtuaalse massiivi loomine ja selle siis kasutamine target seadme jaoks DR.

### <a name="deactivate"></a>Desaktiveerimine

Väljalülitamiseks StorSimple virtuaalse massiiv, siis katkestab seade ja vastava StorSimple halduri teenuse vahelise ühenduse. Desaktiveerimine on **püsivate** toiming ja ei saa tagasi võtta. Desaktiveeritud seade ei saa registreeritud StorSimple halduri teenuse uuesti. Lisateabe saamiseks minge [desaktiveerimine ja kustutamine oma StorSimple virtuaalse massiiv](storsimple-deactivate-and-delete-device.md).

Järgmiste juhiste täitmisel pidage meeles oma virtuaalse massiiv desaktiveerimisel:

-   Kõik andmed enne desaktiveerides virtuaalne seadme cloud hetktõmmise tegemiseks. Kui virtuaalse massiiv desaktiveerimist kohaliku seadmega kõik andmed on kadunud. Pilveteenuse hetktõmmise tegemisel võimaldab teil andmeid hiljem taastada.

-   Enne StorSimple virtuaalse massiiv desaktiveerimist veenduge, et peatada või kustutada kliendid ja hakkab majutama, mis sõltub selle seadme.

-   Kui kasutate enam, et see ei koguda kulude desaktiveeritakse seadme kustutamine

### <a name="monitoring"></a>Jälgimine

Veenduge, et teie StorSimple virtuaalse massiiv on pidev terve olekus, peate jälgida massiiv ja veenduge, et saada teavet süsteemist, sh teatised. Üldine seisundi virtuaalse massiivi jälgimiseks rakendada järgmised head tavad:

- Konfigureerige jälgida ketta kasutamine oma virtuaalse massiivi andmed kettale, samuti OS ketas jälgimine. Kui Hyper-V, saate jälgida oma virtualization hosts süsteemi Center virtuaalse masina Manager (SCVMM) ja System Center toimingute Manager (SCOM) kombinatsiooni.   

- Saate konfigureerida oma virtuaalse massiiv teatud kasutus tasanditel teatiste saatmine meiliteatised.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Registri otsingu- ja viirusetõrje skannimine rakendused

StorSimple virtuaalse massiivi saab automaatselt esimese kohaliku taseme andmeid Microsoft Azure'i pilve. Rakendus nagu index otsing või viiruste skannimine kasutamisel skannimiseks StorSimple talletatud andmed peate, et pilveteenuses andmeid ei saada juurde ning tõmmatakse tagasi kohaliku taseme.

Soovitame järgmiste juhiste täitmisel juurutada registri otsingu või viiruste skannimine konfigureerimisel oma virtuaalse massiiv:

-   Keelake automaatselt konfigureeritud täielik skannimine.

-   Astmeline maht, konfigureerige registri otsingu või viiruste skannimine rakenduse on suureneva skannimine sooritamiseks. See oleks skannida ainult selle uute andmete tõenäoliselt elavate kohaliku taseme kohta. Andmed, mida on mitmetasandilise pilveteenusesse on ei avata suureneva toimingu käigus.

-   Tagada õige otsingu filtrid ja sätted on konfigureeritud nii, et saada skannitud ainult ettenähtud tüüpi faile. Näiteks pilt (JPEG-, GIF ja TIFF) ja tehnika joonistused peaks ei kontrollita ajal suureneva või täielik register taastada.

Kui Windowsi indekseerimine protsessi, järgige neid juhiseid.

-   Kasutage Windows indekseerija astmeline mahud nagu see meenutab suurte andmehulkade (TBs) pilvest, kui peab indeks luuakse uuesti korduma. Registri taastamine oleks failitüüpide registrisse nende sisu alla laadida.

-   Kasutage indekseerimine protsessi kohalikult kinnitatud maht, nagu see oleks juurde ainult kohaliku astme koostamiseks (pilve andmed kuvatakse ei avata) registri andmete aknad.

### <a name="byte-range-locking"></a>Bait vahemiku lukustamine

Rakendusi saate faile maksimaalselt lukustada vahemikuga baiti. Kui bait vahemiku lukustamine on lubatud rakendused, mis on teie StorSimple kirjutamine, siis tiering ei tööta teie virtuaalse massiiv. Töötada tiering kõigi alade failide juurde peaks olema lukustamata. Astmeline draividel oma virtuaalse massiiv ei toeta byte vahemiku lukustamine.

Soovitatav mõõdud leevendada bait vahemiku lukustamine on järgmised.

-   Bait vahemiku lukustamine oma rakenduse loogika välja lülitada.

-   Kasutage kohalikult kinnitatud mahud (mitte mitmetasandilise) funktsiooni andmete jaoks. Kohalik kinnitatud maht ei esimese pilve.

-   Kui bait vahemiku lukustamine lubatud maht kohalikult abil kinnitatud, helitugevuse saate veebis enne taastamine on lõpule viidud. Sellisel juhul peate ootama täielikuks taastamine.

## <a name="multiple-arrays"></a>Mitme massiivi

Mitme virtuaalse massiivi võib tekkida vajadus kasvab töökomplekt andmeid, mis võivad kõrvalmõju peale pilves, seega seadme jõudlust mõjutavad konto kasutusele võtta. Sellisel juhul on parim mastaap seadmete töökomplekt kasvab. Selleks on vaja ühe või mitme seadme asutusesisese andmekeskuse lisada. Kui lisate seadmete, võite:

-   Praeguse andmehulga tükeldada.
-   Uue töökoormus juurutada uus seade/seadmed.
-   Kui mitme virtuaalse massiivi võtavad, soovitame kaudu koormust tasakaalustavad perspektiivi, jaotada massiiv kogu muu hypervisor hosts.

-  Jagatud faili süsteemi Namespace saab kasutada mitut virtuaalse massiivi (kui see on konfigureeritud failiserverisse või iSCSI serverisse). Üksikasjalikud juhised, minge [Jaotatud faili süsteemi Namespace lahendus hübriid pilve salvestusruumi juurutusjuhend](https://www.microsoft.com/download/details.aspx?id=45507). Jaotatud failide süsteemi kopeerimine virtuaalse massiiv jaoks praegu pole soovitatav. 


## <a name="see-also"></a>Vt ka
Siit saate teada, [hallata oma StorSimple virtuaalse massiiv](storsimple-ova-manager-service-administration.md) StorSimple halduri teenuse kaudu.
