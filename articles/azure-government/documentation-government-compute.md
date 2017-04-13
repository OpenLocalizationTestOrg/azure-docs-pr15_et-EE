<properties
    pageTitle="Azure'i valitsuse dokumentidele | Microsoft Azure'i"
    description="See pakub võrdlus funktsioonid ja juhiseid Azure'i valitsuse rakenduste arendamise kohta"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Azure'i Government Arvuta

##  <a name="virtual-machines"></a>Virtuaalmasinates

Selle teenuse ja kuidas seda kasutada kohta täpsema teabe saamiseks vt [Azure'i Virtuaalmasinates suurused](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Variatsioonid

Järgmised VM SKU-d on üldiselt kättesaadav (GA) Azure'i valitsuse.

VM SKU|USA valitsuse VA|USA-i gov – a|Märkmete
---|---|---|---
A|GA|GA|Ükski
Lisatud Dv1|GA|-|Ükski
DSv1|GA|-|Ükski
Dv2|GA|GA|15 tulekul
F|GA|GA|Ükski
G|Plaanitud|-|Ükski

###  <a name="data-considerations"></a>Andmete kaalutlused

Järgmine teave tuvastab Azure Governmenti äärist Azure'i Virtuaalmasinates:

| Lubatud reguleeritud kontrolli all andmed | Andmete reguleeritud kontrolli all pole lubatud |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Andmed sisestanud, salvestatud ja töödeldakse VM võib sisaldada ekspordi kontrollitud andmed. Binaarfailid töötab Azure'i Virtuaalmasinates sees. Staatilise authenticators, nt paroolide ja kiipkaardi sõrmed juurdepääsuks Azure'i platvormi komponendid. Sertide abil saate hallata Azure'i platvormi komponendid privaatvõtmete. SQL-i ühendusstringi.  Muu teave/saladused, nt serdid, krüptimise võtmed, juhtslaidi võtmed ja salvestusruumi võtmed salvestatakse Azure teenused.  | Metaandmete on ei tohi sisaldada ekspordi kontrollitud andmed. Selle metaandmed sisaldab kogu sisestatud loomisel ja säilitada oma Azure virtuaalse masina konfiguratsiooni andmed.  Sisestage andmete reguleeritud/järgmised väljad: rentniku rolli nimed, ressursside rühmad, juurutamise nimed, ressursside nimed, ressursside Sildid  

## <a name="next-steps"></a>Järgmised sammud

Lisateave ja värskendused, tellimine on <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure'i Government ajaveeb.</a>
