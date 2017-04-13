<properties 
   pageTitle="StorSimple 8000 värskendus 0,2 väljalaskemärkmed | Microsoft Azure'i"
   description="Uued funktsioonid ja parandused, avatud probleemid ja saadaval lahendused kirjeldatakse jaanuar 2015 Microsoft Azure'i StorSimple release (Update 0,2)."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 seeria värskendus 0,2 väljalaskemärkmed – jaanuar 2015

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed tuvastada kriitilised avatud probleemid Microsoft Azure'i StorSimple jaanuar 2015 versiooni. Sisaldavad ka see väljaanne sisaldab StorSimple tarkvara ja püsivara värskenduste loendit. See on teine väljaanne pärast StorSimple 8000 sarja väljaande versioon on üldiselt kättesaadavaks juuli 2014.
  
Värskendust ei muuda Update'ilt oktoober seadme tarkvara versiooni. See endiselt versiooni 6.3.9600.17312. Pilti kasutada virtuaalse seadme pilt on muutunud selles versioonis. Seetõttu kõik uued pärast 1-20-2015 loodud virtuaalse seadmed näitavad versioon 6.3.9600.17361.  

Palun vaadake üle jaanuar 2015 värskenduse väljalaskemärkmed järgmine teave.

> [AZURE.IMPORTANT]  
>
>- Selle värskenduse pole saadaval Windows Update'i kaudu ja ei saa installida nagu muud värskendused. Teie seade ei saa selle värskenduse isegi juhul, kui olete rakendanud Värskenduste Azure klassikaline portaali kaudu. Selle värskenduse rakendatakse ainult virtuaalse seadmetes, mis on loodud pärast 20 jaanuar 2015. 
> 
>- StorSimple jaanuar versiooni ei sisalda StorSimple seadme värskendusi. Siiski saate rakendada saadaolevad värskendused Windows virtuaalse seadmes, sh turbe tehtud parandused, kuid ei kuvata muudatuste StorSimple seadme versioon.

## <a name="whats-new-in-the-january-release"></a>Mis on uut versioonis jaanuar

Selle värskenduse sisaldab seotud ühenduseta režiim virtuaalse seadme mahud Paranda. (Vt [probleemid fikseeritud selles versioonis](#issues-fixed-in-the-january-release)).  

Värskendust ei sisalda uusi funktsioone.  

## <a name="issues-fixed-in-the-january-release"></a>Fikseeritud jaanuar väljaanne probleemid

Järgmises tabelis kirjeldatakse probleemi, mis on määratud see värskendus.

|Ei.|Funktsioon|Probleem|Kehtib seadme|Kehtib virtuaalse seade 
|---|-------|-----|--------------------------|-------------------------
|1|Ühenduseta režiim mahud|Kui kõrge cloud latentsused püsida minuti, avage StorSimple virtuaalse seadme mahud hosts ühenduseta. See parandus suurendab cloud latentsused, vähendades olukordades, mis võib põhjustada mahud vallasrežiimi hosts läve.|Ei|Jah  

## <a name="known-issues-in-the-january-release"></a>Jaanuar väljaande teadaolevate probleemide

Järgmises tabelis antakse ülevaade selle väljaande teadaolevate probleemide.
 
|Ei.|Funktsioon|Probleem|Kommentaaride/lahendus|Kehtib seadme|Kehtib virtuaalse seade 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Factory lähtestamine|  Mõnel juhul, kui lähtestamist factory, StorSimple seade võib kinni ja kuvada teade: **Reset to factory on pooleli (etapp 8).** See juhtub siis, kui vajutate klahvikombinatsiooni CTRL + C cmdlet ajal.| Vajutage klahvikombinatsiooni CTRL + C pärast alustamist factory lähtestamine. Kui olete juba selles olekus, pöörduge Microsoft Support järgmised toimingud.|Jah|Ei|
|2|Ketas kvoorumi| Harvaesinevate eksemplari, kui ketta jaotise EBOD ruumi 8600 seadme enamik ei saa tulemuseks kvoorumit kettale, siis selle talletusmahtu saab ühenduseta. See jääb ühenduseta isegi juhul, kui selle ketast ühendatakse.|Peate taaskäivitage seade. Kui probleem ei lahene, pöörduge Microsoft Support järgmised toimingud.|Jah |Ei
|3|Pilveteenuse hetktõmmise tõrked|Harvaesinevate juhtudel ei pruugi hetktõmmise cloud tõrkega **varukoopia piirmäära jõudnud**. See juhtub, kui te ületada 255 online kloonid samasse seadmesse sama algse mahtu, mis on kustutatud.||Jah|Jah
|4|Vale kontrolleril ID|Kui selle domeenikontrolleri asendamine toimub, domeenikontrolleri 0 võib kuvada selle domeenikontrolleri 1. Ajal kontrolleril väljavahetamine, pildi laadimisel omavahelistes sõlme kontrolleril ID kuvada algselt omavahelistes kontrolleril ID.  Harvaesinevate juhtudel selline käitumine võib näha ka pärast taaskäivitada.|Kasutajatoimingut ei vaja. Olukord lahendab ise, kui selle domeenikontrolleri asendamine on lõpule jõudnud.|Jah|Ei 
|5| Seadme jälgimise diagrammid|StorSimple halduri teenus, seadme jälgimisega seotud diagrammid ei tööta, kui lihtne või puhverserveri server Configuration seadme jaoks on lubatud NTLM autentimine.|Muutke veebi Puhverserveri konfiguratsioon seadme registreeritud teie haldur StorSimple teenusega, nii et autentimine sätteks pole. Selleks käivitada soovitud Windows PowerShelli cmdlet-käsk StorSimple Set-HcsWebProxy.|Jah|Jah
|6| Salvestusruumi kontod|Salvestusruumi teenuse abil konto salvestusruumi kustutada on mõni mittetoetatud stsenaariumi. See viib olukorda, kus kasutaja andmeid ei saa tuua.|| Jah |  Jah
|7|Seadme Tõrkesiirde| Mitme failovers helitugevuse container sama allika seadmest ja muu target seadmed ei toetata.| Helitugevuse ümbriste esimesel üle seadme nurjus ilma andmete omaniku teeb Tõrkesiirde ühe surnud seadmest, mitu seadet. Pärast Tõrkesiirde, nende helitugevuse ümbriste kuvada või toimivad teisiti Azure klassikaline portaali kuvatava.|Jah|Ei
|8| Installimine|Ajal StorSimple adapterit SharePointi installi, tuleb sisestada seadme IP selleks installi lõpule.||Jah|Ei
|9| Veebiteenuse puhverserveri|Kui teie web puhverserveri konfigureerimine on määratud protokoll HTTPS, mõjutab oma seadme-teenus side ja seadme katkestada. Tugiteenuste pakettide luuakse ka protsessi, tarbimine märgatavalt ressursid oma seadmes.|Veenduge, et web puhverserveri URL on määratud protokoll HTTP. Lisateavet leiate teemast [konfigureerimine veebiteenuse puhverserveri teie seadme](storsimple-configure-web-proxy.md)kohta.|Jah |Ei
|10|Veebiteenuse puhverserveri|  Kui konfigureerimine ja lubada veebiteenuse puhverserveri registreeritud seadmes, siis pead taaskäivitage aktiivne kontrolleril seadmes.|| Jah |Ei
|11|Kõrge cloud latentsus ja suur I/O töökoormus|Väga kõrge cloud latentsused (järjestuse sekundites) ja suur I/O töökoormus kombinatsiooni käivitamise StorSimple seadme seadme mahud minema halvenenud olekus ja selle väljundtoimingute võib nurjuda tõrketeatega "seade ei ole valmis" tõrge.|Peate käsitsi taaskäivitage seade kontrollerid või seadme Tõrkesiirde taastamine juhul teha.|Jah|Ei

## <a name="physical-device-updates-in-the-january-release"></a>Seadme värskenduste jaanuar versioon

Värskendust ei sisalda StorSimple seadmele muid muudatusi.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Vabastage järjestikust manustatud SCSI (SAS) domeenikontrolleri ja jaanuar püsivara värskendused

Selles versioonis ei sisalda järjestikust lisatud SCSI (SAS) kontrolleril või püsivara värskendused. Draiveri värskendus oli oktoobris, 2014 väljaanne. 

## <a name="virtual-device-updates-in-the-january-release"></a>Virtuaalne seadme värskenduste jaanuar versioon

See väljaanne sisaldab värskendatud pilti, virtuaalse seadme. Kõik virtuaalse seadmetes, mis on loodud pärast 20 jaanuar 2015 näitavad 6.3.9600.17361 tarkvara versiooni.

 
