<properties 
   pageTitle="StorSimple virtuaalse massiiv väljalaskemärkmed | Microsoft Azure'i"
   description="Kirjeldatakse avatud kriitiliste probleemide ja lahenduste StorSimple virtuaalse massiiv."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>StorSimple virtuaalse massiiv väljalaskemärkmed

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed tuvastada kriitiliste avatud probleemide märts 2016 üldiselt kättesaadav (GA) versiooni Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme või StorSimple virtuaalse seade). See väljaanne vastab tarkvara versiooni 10.0.10271.0.

Funktsiooni väljalaskemärkmed värskendatakse pidevalt ja nagu vaja minna kriitiliste probleemide avastamisel neid lisada. Enne juurutamist StorSimple virtuaalse seadme, hoolikalt üle väljaanne märkmetes sisalduvat teavet. 

Järgmises tabelis antakse ülevaade selle väljaande teadaolevate probleemide.


| Ei. | Funktsioon | Probleem | Lahendus/kommentaarid |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Värskendused | Virtuaalne seadmete loodud preview release ei saa värskendada üldiselt kättesaadav toetatud versiooni. | Need virtuaalse seadmed peab nurjus üle üldiselt kättesaadav vabastamiseks töövoo katastroofi taastamine (DR) abil. |
| **2.** | Andmete ettevalmistatud kettal | Kui teil on ette valmistatud andmed kettale määratud suurusega ja vastava StorSimple virtuaalse seadme loonud, peate ei laiendamine või kahandamine andmed kettale. Proovite seda teha tulemuseks vähenemist kohaliku astme seadme kõik andmed. |   |
| **3.** | Rühmapoliitika | Kui seade on domeeni ühendatud, rühmapoliitika rakendades kahjustada seadme toimimist. | Veenduge, et teie virtuaalse massiiv on eraldi organisatsiooniüksusega (OU) Active Directory jaoks ja see rakendatakse pole rühmapoliitika objektid (GPO).|
| **4.** | Kohaliku web UI | Kui täiustatud funktsioonid on lubatud Internet Explorer (IE paoklahvi (ESC)), ei pruugi kohaliku UI veebilehtede nagu tõrkeotsingu õigesti töötada. Nupud külastamine ka ei tööta. | Internet Exploreri täiustatud funktsioone välja lülitada.|
| **5.** | Kohaliku web UI | Hyper-V virtual kohapeal, võrgu liidesed 10 kuvatakse kasutajaliides Web Gbit liidesed. | Selline käitumine on Hyper-V peegeldus. Hyper-V näitab alati 10 Gbit virtuaalse võrguadapteri jaoks. |
| **6.** | Astmeline mahud või aktsiad | Bait vahemik lukustamine rakendusi, mis töötavad StorSimple, astmeline maht ei toetata. Kui bait vahemiku lukustamine on lubatud, StorSimple tiering ei tööta. | Soovitatud meetmed sisaldavad järgmist: <br></br>Bait vahemiku lukustamine oma rakenduse loogika välja lülitada.<br></br>Panna kohalikult kinnitatud mahud astmeline mahud asemel selle rakenduse jaoks andmete valimine<br></br>*Hoiatus*: kui kohalik abil kinnitatud mahud ja byte vahemiku lukustamine on lubatud, pidage meeles, et kohalikult kinnitatud helitugevuse saab võrgus isegi enne taastamine on lõpule viidud. Sellisel juhul, kui taastamise on pooleli, siis peate ootama taastamine lõpuleviimiseks. |
| **7.** | Astmeline aktsiad | Suuremahuliste failide töötamise võib põhjustada aeglane taseme välja. | Kui töötate suuri faile, soovitame, et suurim fail on väiksem kui 3% ühiskasutus suurus. |
| **8.** | Osade võimsus kasutatud | Teile võidakse kuvada tarbimine puudumisel võrgukettal andmeid jagada. See on kasutatud võimsus aktsiate sisaldab metaandmete. |   |
| **9.** | Katastroofiabi | Saate teha ainult avariitaastet faili serveri sama Domeen allikas seadme abil. Sihtrakenduse seadme lisadomeeni avariitaastet selles versioonis ei toetata. | See rakendatakse uuem väljaanne. |
| **10.** | Azure'i PowerShelli | Azure'i PowerShelli selles versioonis ei saa hallatakse StorSimple virtuaalne seadmed. | Kõik virtuaalse seadmete haldamine tuleks teha Azure klassikaline portaali ja kohalik web Kasutajaliidese kaudu. |
| **11.** | Parooli muutmine | Virtuaalne massiiv seadme konsooli aktsepteerib ainult sisend en-US klaviatuuri vormingus. |   |
| **12.** | PEATÜKK | Kui loonud CHAP identimisteave ei saa eemaldada. Lisaks, kui muudate CHAP mandaat, pead mahud võrguühenduseta ja seejärel viia need võrgus muudatuse jõustumiseks. | Siin käsitletakse uuem väljaanne. |
| **13.** | iSCSI server  | Funktsiooni "kasutada salvestusruumi" iSCSI maht on kuvatud võivad olla erinevad StorSimple halduri teenuse ja iSCSI host. | ISCSI host on failisüsteemi vaade.<br></br>Seadme näeb plokid eraldatud kui maht oli maksimummaht.|
