<properties 
   pageTitle="StorSimple virtuaalse massiiv värskenduste vabastage märkmete | Microsoft Azure'i"
   description="Kirjeldatakse avatud kriitiliste probleemide ja lahenduste Update'i 0,3 StorSimple virtuaalse massiiv."
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
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple virtuaalse massiiv Update 0.3 väljalaskemärkmed

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed tuvastada, kriitiliste avatud probleemide ja lahendada probleeme Microsoft Azure'i StorSimple virtuaalse massiiv värskendusi.

Funktsiooni väljalaskemärkmed värskendatakse pidevalt ja nagu vaja minna kriitiliste probleemide avastamisel neid lisada. Enne juurutamist oma StorSimple virtuaalse massiiv, hoolikalt üle väljaanne märkmetes sisalduvat teavet.

Värskenduse 0,3 vastab tarkvara versioon **10.0.10288.0**.

> [AZURE.NOTE] Värskendused on häiriva ja taaskäivitage oma seade. Kui I/O on pooleli, seadme andmesidekasutusele tööseisakute.


## <a name="whats-new-in-the-update-03"></a>Mis on uut rakenduses värskenduse 0,3

Eelkõige vigu parandada koostamine on värskendus 0,3. Selles versioonis on adresseeritud mitu varukoopia tõrkeid eelmises versioonis tulemuseks viga.

## <a name="issues-fixed-in-the-update-03"></a>Probleemid 0,3 värskendus

Järgmises tabelis esitatakse Kokkuvõte probleemid fikseeritud selles versioonis.

| Ei.  | Funktsioon                              | Probleem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Varukoopiate                                |Probleem on näha varasem versioon, kus varukoopiaid lõpuleviimiseks failikettale jaoks nurjub. Kui see probleem ilmnes, Varundustöö nurjub ja Kriitiline teade esitati StorSimple halduri teenuse, et kasutajat. See probleem ei mõjuta aktsiate või juurdepääsu andmetele andmeid. Põhjus on tuvastatud ja fikseeritud selles versioonis. <br></br> Lahendus ei kehti tagasiulatuvalt osakud, mis on juba näha selle probleemi. Kliendid, kes ei näe probleemi tuleks esmalt värskenduse 0,3 ja seejärel ühendust Microsofti Support terve süsteemi varundamine probleemi lahendamiseks. Asemel poole pöördumist Microsoft Support, kliendid saate taastada ka uut ühiskasutusega probleemse aktsiad korralik varukoopia põhjal.                                                                                                                                                                                 |
| 2    | iSCSI                         | Varasem versioon, kus mahud kaoks andmete kopeerimisel StorSimple virtuaalse massiivi maht on näha probleemi. See probleem on lahendatud selles versioonis. <br></br> Parandused rakenduvad ainult vastloodud maht. Parandused ei rakendata tagasiulatuvalt mahud, mis on juba näha selle probleemi. Kliendid on soovitatav probleemse mahud võrgus Azure klassikaline portaali kaudu tuua, mahud varukoopia ja seejärel nende mahud taastamiseks uus maht.                                                               |


## <a name="known-issues-in-the-update-03"></a>Värskenduse 0,3 teadaolevad probleemid

Järgmises tabelis esitatakse Kokkuvõte teadaolevad probleemid StorSimple virtuaalse massiivi ja väljaanne märkida varasemate versioonidega probleemid, mis sisaldab. 


| Ei. | Funktsioon | Probleem | Lahendus/kommentaarid |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Värskendused | Virtuaalne seadmete loodud preview release ei saa värskendada üldiselt kättesaadav toetatud versiooni. | Need virtuaalse seadmed peab nurjus üle üldiselt kättesaadav vabastamiseks töövoo katastroofi taastamine (DR) abil. |
| **2.** | Andmete ettevalmistatud kettal | Kui teil on ette valmistatud andmed kettale määratud suurusega ja vastava StorSimple virtuaalse seadme loonud, peate ei laiendamine või kahandamine andmed kettale. Üritades Ärge kaotus tulemuste kohaliku astme seadme kõik andmed. |   |
| **3.** | Rühmapoliitika | Kui seade on domeeni ühendatud, rühmapoliitika rakendades kahjustada seadme toimimist. | Veenduge, et teie virtuaalse massiiv on eraldi organisatsiooniüksusega (OU) Active Directory jaoks ja see rakendatakse pole rühmapoliitika objektid (GPO).|
| **4.** | Kohaliku web UI | Kui täiustatud funktsioonid on lubatud Internet Explorer (IE paoklahvi (ESC)), ei pruugi kohaliku UI veebilehtede nagu tõrkeotsingu õigesti töötada. Nupud külastamine ka ei tööta. | Internet Exploreri täiustatud funktsioone välja lülitada.|
| **5.** | Kohaliku web UI | Hyper-V virtual kohapeal, võrgu liidesed 10 kuvatakse kasutajaliides Web Gbit liidesed. | Selline käitumine on Hyper-V peegeldus. Hyper-V näitab alati 10 Gbit virtuaalse võrguadapteri jaoks. |
| **6.** | Astmeline mahud või aktsiad | Bait vahemik lukustamine rakendusi, mis töötavad StorSimple, astmeline maht ei toetata. Kui bait vahemiku lukustamine on lubatud, StorSimple tiering ei tööta. | Soovitatud meetmed sisaldavad järgmist: <br></br>Bait vahemiku lukustamine oma rakenduse loogika välja lülitada.<br></br>Panna kohalikult kinnitatud mahud astmeline mahud asemel selle rakenduse jaoks andmete valimine<br></br>*Hoiatus*: kui kohalik abil kinnitatud mahud ja bait vahemiku lukustamine on lubatud, kohalikult kinnitatud helitugevuse saab võrgus isegi enne taastamine on lõpule viidud. Sellisel juhul, kui taastamise on pooleli, siis peate ootama taastamine lõpuleviimiseks. |
| **7.** | Astmeline aktsiad | Suuremahuliste failide töötamise võib põhjustada aeglane taseme välja. | Kui töötate suuri faile, soovitame, et suurim fail on väiksem kui 3% ühiskasutus suurus. |
| **8.** | Osade võimsus kasutatud | Teile võidakse kuvada tarbimine ühiskasutusse anda, kui andmed on ühiskasutus. See on kasutatud võimsus aktsiate sisaldab metaandmete. |   |
| **9.** | Katastroofiabi | Saate teha ainult avariitaastet faili serveri sama Domeen allikas seadme abil. Sihtrakenduse seadme lisadomeeni avariitaastet selles versioonis ei toetata. | See rakendatakse uuem väljaanne. |
| **10.** | Azure'i PowerShelli | Azure'i PowerShelli selles versioonis ei saa hallatakse StorSimple virtuaalne seadmed. | Kõik virtuaalse seadmete haldamine tuleks teha Azure klassikaline portaali ja kohalik web Kasutajaliidese kaudu. |
| **11.** | Parooli muutmine | Virtuaalne massiiv seadme konsooli aktsepteerib ainult sisend en-US klaviatuuri vormingus. |   |
| **12.** | PEATÜKK | Kui loonud CHAP identimisteave ei saa eemaldada. Lisaks, kui muudate CHAP mandaat, peate mahud võrguühenduseta ja seejärel viia need võrgus muudatuse jõustumiseks. | See probleem on adresseeritud uuem väljaanne. |
| **13.** | iSCSI server  | Funktsiooni "kasutada salvestusruumi" iSCSI maht on kuvatud võivad olla erinevad StorSimple halduri teenuse ja iSCSI host. | ISCSI host on failisüsteemi vaade.<br></br>Seadme näeb plokid eraldatud kui maht oli maksimummaht.|
| **14.** | Faili server  | Kui faili kaustas on mõne alternatiivse andmete voo (reklaamid) seostatud, reklaamid on varundada või taastada kaudu avariitaastet, klooni ja üksuse taseme taastamine.| |


## <a name="next-step"></a>Järgmise juhise juurde

[Installige värskendus 0,3](storsimple-ova-install-update-01.md) oma StorSimple virtuaalse massiiv.

## <a name="references"></a>Viited

Kas otsite mõne vanema release Märkus? Mine: 

- [StorSimple virtuaalse massiiv värskendus 0,1-0,2 väljalaskemärkmed](storsimple-ova-update-01-release-notes.md)

- [StorSimple virtuaalse massiiv üldine kättesaadavus väljalaskemärkmed](storsimple-ova-pp-release-notes.md)
 

