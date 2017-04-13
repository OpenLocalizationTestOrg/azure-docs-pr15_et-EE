<properties 
   pageTitle="Seadme StorSimple jälgimine | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse StorSimple halduri teenuse abil saate jälgida jõudluse, võimsuse kasutamine, võrgu läbilaskevõime,- ja seadme jõudlust."
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
   ms.workload="TBD"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>Jälgimine StorSimple seadme StorSimple halduri teenuse abil 

## <a name="overview"></a>Ülevaade

StorSimple halduri teenuse abil saate jälgida kindlate seadmete StorSimple lahendusse sees. Saate luua kohandatud diagrammide jõudluse, võimsuse kasutamine, võrgu läbilaskevõime, ja seadme jõudlust mõõdikute põhjal. 

Azure'i klassikaline portaalis mõne kindla seadme jälgimisega seotud teabe vaatamiseks valige StorSimple halduri teenuse. Klõpsake vahekaardil **kuvar** ja seejärel valige loendist seadmed. Lehe **jälgimine** sisaldab järgmist teavet.

## <a name="io-performance"></a>I/O jõudlus 

**I/O jõudluse** jälitab mõõdikute seotud lugemine arv ja kirjutage toimingute vahel ühte iSCSI algataja liideste hosti server ja seadme või seadmes kui ka pilveteenustes. Konkreetsete maht, köite container või kõik helitugevuse ümbriste saab mõõta selle jõudlust.

Allolevas tabelis I/O algataja oma seadmesse kõik mahud tootmise seadme jaoks. Diagrammile mõõdikud on lugemine ja kirjutamine baiti sekundis, lugeda ja kirjutada IO sekundis, ja lugeda ja kirjutada latentsused.

![IO jõudluse algataja seadmesse](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Sama seadme/v-toimingud on kantakse seadme andmete jaoks soovitud maht ümbriste pilveteenusesse. Selle seadme andmed on ainult lineaarse taseme ja midagi on peale pilveteenusesse. Toiminguid ei tehta lugemis-ja kirjutamisõigusega pilve mobiilsideseadmest jäävate. Seetõttu on peaks diagrammi, mis vastab sageduse, kus on südamelöögi ajal on märgitud seadme ja teenuse viie minuti järel. 

![IO jõudluse Cloud seadmest](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


Samasse seadmesse võeti hetktõmmise cloud helitugevuse andmete alates 2:00 pm. Selle tulemusena andmed liiguvad seadme pilveteenusesse. Loeb-kirjutab olid pakutakse pilveteenusesse selle kestus. Diagrammi IO kuvatakse vastav aega, kui hetktõmmis on erinevad mõõdikud tipp. 

![Cloud cloud hetktõmmise pärast seadet IO jõudlus](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>Võimsuse kasutamine 

**Võimsus kasutamine** jälitab mõõdikute seotud andmete maht, helitugevuse ümbriste või seadme kasutatakse salvestusruumi summa. Saate luua aruandeid, mis on teie peamine salvestusruumile, teie salvestusruumi või seadme salvestusruumi kasutamise põhjal. Konkreetsete maht, köite container või kõik helitugevuse ümbriste saab mõõta võimsuse kasutamine.


Esmane, pilves, ja seadme mälumaht saab kirjeldada järgmiselt:

###<a name="primary-storage-capacity-utilization"></a>Esmased võimsus kasutamine
 
Nende diagrammide kuvamine kirjutatud StorSimple mahud enne, kui andmed on deduplicated ja tihendatud andmed summa. Saate vaadata esmased kasutamine kõigi mahud või ühe maht.

Esmased helitugevuse võimsus kasutamine diagrammide või iga üksiku mahu kõik mahud vaatamisel ja mõlemal juhul peamine andmete Kokkuvõtteks, võib vastuolu kahe arvu vahelises. Peamine andmete kõik draividel ei saa lisada lähteandmeid üksikud mahu kogusumma. See võib olla mõni järgmistest:

- **Kõik mahud lisatud hetktõmmis andmete**: selline käitumine on näha ainult juhul, kui kasutate versiooni varem Update 3. Esmane andmed kuvatakse kõik maht on iga maht lähteandmeid ja andmete hetktõmmis. Esmane andmed kuvatakse antud maht vastab ainult andmeid, mis on eraldatud mahu summa (ja ei sisalda helitugevuse hetktõmmise vastavaid andmeid).

    Seda saab selgitada järgmist võrrandit:

    *Peamine andmete (kõik mahud) = summa (esmane andmete (helitugevuse ma) + hetktõmmise andmete maht (helitugevuse ma))*
    
    *Kui lähteandmeid (helitugevuse ma) = i helitugevuse eraldatav peamine andmete maht*
 
    Kui tõmmised kustutatakse teenuse kaudu, kustutamise tehakse asünkroonselt tausta. See võib võtta aega jaoks andmete maht hetktõmmise kustutamine pärast värskendamist. 

    Kui operatsioonisüsteemi värskendamine 3 või uuem versioon, siis hetktõmmise andmed ei sisalda andmete maht. Ja esmane kasutamine arvutatakse järgmiselt:

    * Lähteandmeid (kõik mahud) = summa (esmane andmete (helitugevuse ma)
    
    *Kui lähteandmeid (helitugevuse ma) = i helitugevuse eraldatav peamine andmete maht*
 
- **Mahud jälgimise korral keelatud kaasata kõik mahud**: kui teil on mahud teie seadmes, mille jälgimise on välja lülitatud, üksikute mahud andmed ei ole saadaval diagrammides. Siiski sisaldab kõik mahud diagrammi jaoks andmete maht, mille jälgimise on välja lülitatud. 
 
- **Kustutatud mahud koos reaalajas seotud varukoopiate kaasata kõik maht**: kui kustutatakse hetktõmmise andmeid sisaldav maht, kuid seotud pilte ikka olemas, siis võidakse kuvada vastuolu.

- **Kustutatud mahu kaasata kõik maht**: mõnel vana maht on olemas isegi juhul, kui need olid kustutatud. Kustutamise mõju ei ole näha ja seade võib kuvada alumise saadaval võimsus. Peate ühendust Microsofti Support eemaldamine nende mahtu.

Järgmisi diagramme kuvada soovitud esmased kasutamise StorSimple seadme enne ja pärast hetktõmmise pilve. See on ainult andmeid, ei tohiks hetktõmmise cloud esmane salvestusruumi muuta. Nagu näete, kuvatakse diagrammi erinevust esmane võimsus kasutamise tõttu hetktõmmise pilve. Pilveteenuse hetktõmmise alustada umbes 2:00 PL seadmes.

![Esmane võimsus kasutamine enne pilve hetktõmmis](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Pärast pilvepõhise hetktõmmise esmane võimsus kasutamine](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Kui teil on värskendada 2 või suurem, võite murda esmased võimsus kasutamine on üksikud helitugevust, kõik mahud, kõik astmeline mahud ja kõik kohalikult kinnitatud mahud nagu allpool näidatud. Katkestamine kõik kohalikult kinnitatud maht, saate kiiresti kindlaks teha palju kohaliku tasandi kasutatakse.

![Esmane võimsus kasutamine kõigi kohalikult kinnitatud mahud](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Pilveteenuse salvestusruumi võimsus kasutamine

Nende diagrammide kuvamine salvestusruumi kasutatud summa. Andmed on deduplicated ja tihendatud. Summa sisaldab pilve pilte, mis võivad sisaldada andmeid, mis ei kajastu mis tahes esmane helitugevust ja pärand- või nõutud säilitamist eesmärgil säilitatakse. Saate võrrelda esmast ja cloud salvestusruumi näitajad saada aimu, milline andmete vähendamise määr, kuigi arv ei ole täpne. Järgmised punktdiagrammil pilveteenuse salvestusruumi võimsus kasutamine StorSimple seadme enne ja pärast hetktõmmise pilve. Pilveteenuse hetktõmmise alustamine veebisaidil umbes 2:00 PL seadmes ja te näete cloud võimsus kasutamine kuvatõmmis samal ajal suureneb 5,73 MB 4,04 GB.

![Pilveteenuse võimsus kasutamine enne pilve hetktõmmis](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Pilveteenuse võimsus kasutamine pärast cloud hetktõmmis](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Seadme salvestusruumi võimsus kasutamine

Nende diagrammide kuvatakse seade, mis on rohkem kui esmased kasutamine kuna see sisaldab SSD lineaarse taseme kokku kasutamist. Selle taseme sisaldab andmeid, mis on olemas ka seadme tasandi. SSD lineaarse astme võimsus on ringlema nii, et uued andmed on saadaval, vana andmed teisaldatakse HDD esimese taseme (mil see on deduplicated ja tihendatud) ja seejärel pilve.

Aja jooksul esmane võimsus kasutamise ja seadme võimsus kasutamine tõenäoliselt suureneb koos kuni hakkab andmed sõltuvad pilveteenusesse. Sel hetkel seadme võimsus kasutamine ilmselt hakkab platoo, kuid esmane võimsus kasutamise korral suureneb rohkem andmeid on kirjutatud.

Järgmisi diagramme kuvada soovitud esmased kasutamise StorSimple seadme enne ja pärast hetktõmmise pilve. Pilveteenuse hetktõmmise alustada 2:00 PL ja seadme võimsus kasutamise alustamine väheneb sel ajal. Seadme salvestusruumi võimsus kasutamine läks alla 11.58 GB 7.48 GB. See näitab, et tõenäoliselt lineaarse SSD taseme tihendamata andmed on deduplicated, tihendatud, ja HDD teise teisaldada. Pange tähele, et juhul, kui seade on juba nii SSD ja HDD astme suurt hulka andmeid, te ei näe seda vähendamine. Selles näites on seadme small suurt hulka andmeid.

![Seadme võimsus kasutamine enne pilve hetktõmmis](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Seadme võimsus kasutamine pärast cloud hetktõmmis](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Võrgu läbilaskevõime

**Võrgu läbilaskevõime** jälitab mõõdikute seotud andmehulga üle iSCSI algataja võrgu liidesed host serveris ja seadme ja seadme ja pilveteenuse vahel. Iga iSCSI võrgu liidesed selle mõõdiku saate jälgida oma seadmes.

Järgmisi diagramme Kuva võrgu läbilaskevõime andmete 0 ja andmete 4, nii 1 New York võrgu liidesed seadmes. Käesoleval juhul andmete 0 on cloud lubatud andmete 4 oli iSCSI lubatud. Saate vaadata selle sissetuleva ja väljamineva liikluse StorSimple seadme. Tasapinnalise diagrammi, alates 3:24 pm rida on tänu sellele, et saaksime koguda andmeid iga 5 minuti ja võib ignoreerida. 

![Võrgu läbilaskevõime Data4 jaoks](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Võrgu läbilaskevõime Data4 jaoks](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Seadme jõudlust 

**Seadme jõudlust** jälitab mõõdikute seotud seadme jõudlust. Järgmine tabel näitab CPU kasutuse statistika seadme valmistamisel.

![CPU kasutuse seade](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [StorSimple halduri teenuse seadme armatuurlaua kasutamise](storsimple-device-dashboard.md)kohta.

- Siit saate teada, [StorSimple halduri teenuse haldamiseks StorSimple seadme kasutamise](storsimple-manager-service-administration.md)kohta.
