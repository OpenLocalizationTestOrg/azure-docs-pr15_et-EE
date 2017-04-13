<properties 
   pageTitle="StorSimple 8000 väljaande versioon väljalaskemärkmed | Microsoft Azure'i"
   description="Kirjeldatakse uusi funktsioone, avatud probleemid ja saadaval lahendused juuli 2014 Microsoft Azure'i StorSimple väljaanne."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>StorSimple 8000 sarja väljaande versioon väljalaskemärkmed – juuli 2014 

## <a name="overview"></a>Ülevaade

Jälgi väljalaskemärkmed kriitiliste avatud probleemide tuvastamine StorSimple 8000 sarja juuli 2014 üldiselt kättesaadav (GA) väljaandes Microsoft Azure'i StorSimple. See väljaanne vastab tarkvara versioon 6.3.9600.17215.  

Teisiti need vabastage märkmete kõik mudelid StorSimple seadme rakendada. Funktsiooni väljalaskemärkmed uuendatakse; kui vaja minna kriitiliste probleemide avastamisel on lisatud. Enne juurutamist oma Microsoft Azure'i StorSimple lahenduse, võtke arvesse järgmist.  

## <a name="known-issues-in-this-release"></a>See väljaanne teadaolevad probleemid
Järgmises tabelis antakse ülevaade selle väljaande teadaolevate probleemide.  
 
| Ei. | Funktsioon | Probleem | Kommentaaride/lahendus | Kehtib seadme | Kehtib virtuaalse seade |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory lähtestamine | Mõnel juhul, kui lähtestamist factory, StorSimple seade võib kinni ja kuvada teade: **Reset to factory on pooleli (etapp 8)**. See juhtub siis, kui vajutate klahvikombinatsiooni CTRL + C cmdlet ajal. | Vajutage klahvikombinatsiooni CTRL + C pärast alustamist factory lähtestamine. Kui olete juba selles olekus, pöörduge Microsoft Support järgmised toimingud. | Jah | Ei |
| 2 | Ketas kvoorumi | Harvaesinevate eksemplari, kui ketta jaotise EBOD ruumi 8600 seadme enamik ei saa tulemuseks kvoorumit kettale, siis selle talletusmahtu saab ühenduseta. See jääb ühenduseta isegi juhul, kui selle ketast ühendatakse. | Peate taaskäivitage seade. Kui probleem ei lahene, pöörduge Microsoft Support järgmised toimingud. | Jah | Ei |
| 3 | Pilveteenuse hetktõmmise tõrked | Harvaesinevate juhtudel ei pruugi hetktõmmise cloud tõrkega **varukoopia piirmäära jõudnud**. See juhtub, kui te ületada 255 online kloonid samasse seadmesse sama algse mahtu, mis on kustutatud. | | Jah | Jah |
| 4 | Vale kontrolleril ID | Kui selle domeenikontrolleri asendamine toimub, domeenikontrolleri 0 võib kuvada selle domeenikontrolleri 1. Ajal kontrolleril väljavahetamine, kui pilt on laaditud omavahelistes sõlme kontrolleril ID kuvada algselt omavahelistes kontrolleril ID Harvaesinevate juhtudel selline käitumine võib näha ka pärast taaskäivitada. | Kasutajatoimingut ei vaja. Olukord lahendab ise, kui selle domeenikontrolleri asendamine on lõpule jõudnud. | Jah | Ei |
| 5 | Seadme jälgimise diagrammid | StorSimple halduri teenus, seadme jälgimisega seotud diagrammid ei tööta, kui lihtne või puhverserveri server Configuration seadme jaoks on lubatud NTLM autentimine. | Muutke veebi Puhverserveri konfiguratsioon seadme registreeritud teie haldur StorSimple teenusega, nii et autentimine sätteks pole. Selleks käivitada soovitud Windows PowerShelli cmdlet-käsk StorSimple Set-HcsWebProxy. | Jah | Jah |
| 6 | Salvestusruumi kontod | Salvestusruumi teenuse abil konto salvestusruumi kustutada on mõni mittetoetatud stsenaariumi. See viib olukorda, kus kasutaja andmeid ei saa tuua. | | Jah | Jah |
| 7 | Failback | Mõne failback avariitaastet (DR) 24 tunni jooksul ei toetata. | | Jah | Ei |
| 8 | Seadme Tõrkesiirde | Mitme failovers helitugevuse paaniümbrist allika samasse seadmesse target erinevate seadmete ei toetata. Helitugevuse ümbriste esimesel üle seadme nurjus ilma andmete omaniku teeb Tõrkesiirde ühe surnud seadmest, mitu seadet. Pärast Tõrkesiirde, nende helitugevuse ümbriste kuvada või toimivad teisiti Azure klassikaline portaali kuvatava. | | Jah | Ei |
| 9 | Installimine | Ajal StorSimple adapterit SharePointi installi, tuleb sisestada seadme IP installimiseks lõpule. | | Jah | Ei |
| 10 | Võrgu liidesed | Võrgu liidesed andmete 2 ja 3 andmete olid vahetasin tarkvara. | Kui teil on vaja konfigureerida liideste, pöörduge Microsoft Support. | Jah | Ei |


 
