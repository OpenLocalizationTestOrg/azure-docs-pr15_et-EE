<properties 
   pageTitle="StorSimple virtuaalse massiiv värskenduste vabastage märkmete | Microsoft Azure'i"
   description="Kirjeldatakse avatud kriitiliste probleemide ja lahenduste StorSimple virtuaalse massiiv Update'i 0,2 ja 0,1."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple Virtual massiiv värskendus 0,2 ja 0,1 väljalaskemärkmed

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed tuvastada, kriitiliste avatud probleemide ja lahendada probleeme Microsoft Azure'i StorSimple virtuaalse massiiv värskendusi. (Microsoft Azure'i StorSimple virtuaalse massiiv on ka StorSimple kohapealse virtuaalse seadme või StorSimple virtuaalse seadme.) 

Funktsiooni väljalaskemärkmed värskendatakse pidevalt ja nagu vaja minna kriitiliste probleemide avastamisel neid lisada. Enne juurutamist StorSimple virtuaalse seadme, hoolikalt üle väljaanne märkmetes sisalduvat teavet.

Värskendus 0,2 vastab tarkvara versiooni **10.0.10280.0**; Värskendus 0,1 on versioon **10.0.10279.0**. Järgmistes jaotistes loendis iga värskenduse muudatused. 

> [AZURE.NOTE] Värskendused on häiriva ja kas taaskäivitage oma seade. Kui I/O on pooleli, kehtib seadme tööseisakute.

## <a name="issues-fixed-in-the-update-02"></a>Probleemid 0,2 värskendus
Värskendus 0,2 sisaldab värskendus 0,1 Lisaks järgmises tabelis kirjeldatud lahendus kõik muudatused.

Funktsioon                              | Probleem                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Värskendused                                 | Viimase väljaande värskendused ei tuvastata automaatselt Azure klassikaline portaalis nii, et teil oli kohaliku Web UI värskenduste installimiseks kasutada. See probleem on lahendatud selles versioonis. Pärast installimist värskendus 0,2, saate installida Azure klassikaline portaalis tulevikus värskendusi.                       

## <a name="whats-new-in-the-update-01"></a>Mis on uut rakenduses värskendus 0,1

Värskendus 0,1 sisaldab järgmisi veaparandused ja täiustused. 

- **Täiustatud paindlikkust puhul pilveteenuste katkestuste**: see väljaanne sisaldab erinevaid veaparandusi ümber avariitaastet, varundamine, taastamine ja tiering cloud Ühenduvus katkemise korral. 

- **Täiustatud taastamine jõudluse**: see väljaanne sisaldab veaparandusi, mida on oluliselt kärpima lõpetamise aega töö taastamine.

- **Automaatse ruumi taastamise optimeerimine**: andmete kustutamisel õhukesed ettevalmistatud draividel plokid kasutamata salvestusruumi vaja taastatud. See väljaanne sisaldab täiustatud ruumi taastamise protsessi pilvest, mille tulemusena muutuvad kättesaadavaks kiiremini võrreldes varasemate versioonidega kasutamata ruumi.

- **Uue virtuaalse ketta pildid**: uus VHD, VHDX ja VMDK on nüüd saadaval Azure klassikaline portaali kaudu. Saate neid pilte ettevalmistamise uute värskendus 0,1 seadmete alla laadida.

- **Parandamine töö oleku portaalis täpsuse**: tarkvara varasemas versioonis ei Varundustöö töö oleku portaalis teatamine. Selle probleemi lahendamiseks selles versioonis.

- **Domeeni Liitu versioon**: domeeni liitumine ja seadme ümbernimetamine seotud veaparandused.


## <a name="issues-fixed-in-the-update-01"></a>Probleemid 0,1 värskendus

Järgmises tabelis esitatakse Kokkuvõte probleemid fikseeritud selles versioonis.

| Ei.  | Funktsioon                              | Probleem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | Mõned VMware versioonides OS ketas oli näha vähe põhjustada teatiste ja katkestades toiminguid. See on fikseeritud selles versioonis.                                                                                                                                                                                    |
| 2    | iSCSI server                         | Viimase väljaande vajalikud kasutajale määrata iga lubatud võrgu liidese StorSimple virtuaalse seadme lüüsi. Selline käitumine muudetakse see väljaanne nii, et kasutajal on vähemalt üks lüüsi lubatud võrgu liideste konfigureerimiseks.                                                                              |
| 3    | Tugiteenuste pakett                      | Tarkvara varasemas versioonis toetavad paketi saidikogumi nurjus paketi suurused on suurem kui 1 GB. See probleem on lahendatud selles versioonis.                                                                                                                                                                               |
| 4    | Pilvepõhise juurdepääsu                         |  Viimase väljaande StorSimple virtuaalse massiiv ning uuesti, ei ole võrguühendus kohaliku UI oleks ühenduvusprobleemide. See probleem on lahendatud selles versioonis.                                                                                                                            |
| 5    | Diagrammide jälgimine                    | Pärast seadet Tõrkesiirde, eelmist versiooni cloud võimsus kasutamine diagrammide kuvatakse valede väärtuste Azure klassikaline portaalis. See on fikseeritud praeguses versioonis.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>0,1 värskendus teadaolevad probleemid

Järgmises tabelis esitatakse Kokkuvõte teadaolevad probleemid StorSimple virtuaalse massiivi ja väljaanne märkida varasemate versioonidega probleemid, mis sisaldab. **Probleemid vabastamist märkida selles versioonis on tähistatud tärniga. Peaaegu kõik siin loetletud probleemid on GA viimise StorSimple virtuaalse massiivi üle kanda.**


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
| **14.** | Faili server *  | Kui faili kaustas on mõne alternatiivse andmete voo (reklaamid) seostatud, reklaamid on varundada või taastada kaudu avariitaastet, klooni ja üksuse taseme taastamine.| |


## <a name="next-step"></a>Järgmise juhise juurde

Teie StorSimple virtuaalse massiiv [Installi värskendused](storsimple-ova-install-update-01.md) .
