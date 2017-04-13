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
    ms.date="09/30/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-data-and-storage"></a>Azure'i Government andmed ja salvestusruum

##  <a name="azure-storage"></a>Azure'i salvestusruum

Selle teenuse ja kuidas seda kasutada kohta täpsema teabe saamiseks leiate [Azure'i salvestusruumi avaliku dokumentatsioonist](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Variatsioonid

URL-ide Azure Governmenti salvestusruumi kontod on erinevad.

Teenuse tüüp|Azure'i avaliku|Azure'i Government
---|---|---
Bloobimälu|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Järjekorra salvestusruum|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Tabelimälu|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Kõik oma skriptide ja koodi peab konto jaoks sobiv lõpp-punktid.  Leiate [Azure'i salvestusruumi ühendusstringi konfigureerimine](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

API-de kohta lisateabe saamiseks vt <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Pilveteenuse salvestusruumi konto ehitaja</a>.

Lõpp-punkti järelliite nende ülekoormuse kasutamine on core.usgovcloudapi.net 

### <a name="considerations"></a>Kaalutlused

Järgmine teave tuvastab Azure Governmenti äärist Azure Storage:

| Lubatud reguleeritud kontrolli all andmed | Andmete reguleeritud kontrolli all pole lubatud |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Andmed sisestanud, salvestatud ja töödeldud on Azure Storage toote võib sisaldada ekspordi kontrollitud andmed. Staatilise authenticators, nt paroolide ja kiipkaardi sõrmed juurdepääsuks Azure'i platvormi komponendid. Sertide abil saate hallata Azure'i platvormi komponendid privaatvõtmete. Muu teave/saladused, nt serdid, krüptimise võtmed, juhtslaidi võtmed ja salvestusruumi võtmed salvestatakse Azure teenused. | Azure'i salvestusruumi metaandmete on ei tohi sisaldada ekspordi kontrollitud andmed. Selle metaandmete sisaldab kõik konfiguratsiooni andmed sisestanud loomisel ja toote salvestusruumi haldamine.  Sisestage andmete reguleeritud/järgmised väljad: ressursi rühmad, juurutamise nimed, ressursside nimed, ressursside Sildid  

##  <a name="premium-storage"></a>Premium salvestusruum

Selle teenuse ja kuidas seda kasutada kohta täpsema teabe saamiseks vt [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Variatsioonid

Premium mälu on üldiselt kättesaadav autoriõiguspõhimõtted Virginia. See hõlmab DS-sarja Virtuaalmasinates. 

### <a name="considerations"></a>Kaalutlused

Sama salvestusruumi andmete järgmine eelnimetatud kehtida premium salvestusruumi kontod. 

##  <a name="sql-database"></a>SQL-andmebaas

Vaadake<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft turbekeskus andmebaasimootor SQL-i jaoks</a> ja [SQL Azure'i andmebaasi avaliku dokumentatsiooni](https://azure.microsoft.com/documentation/services/sql-database/) metaandmete nähtavuse konfiguratsiooni ja kaitse head tavad täiendavad juhised.

### <a name="variations"></a>Variatsioonid

SQL-andmebaasi V12 on üldiselt saadaval Azure Government.

SQL Azure'i server Azure'i Government aadress on erinevad:

Teenuse tüüp|Azure'i avaliku|Azure'i Government
---|---|---
SQL-andmebaas|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Kaalutlused

Järgmine teave tuvastab Azure Governmenti äärist Azure Storage:

| Lubatud reguleeritud kontrolli all andmed | Andmete reguleeritud kontrolli all pole lubatud |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kõik andmed salvestatakse ja vaadatakse läbi Microsoft Azure SQL-i võivad sisaldada Azure'i valitsuse reguleeritud andmeid. Kasutage andmete edastamiseks Azure'i valitsuse reguleeritud andmete Andmebaasiriistad. | Azure SQL-i metaandmete on ei tohi sisaldada ekspordi kontrollitud andmed. Selle metaandmete sisaldab kõik konfiguratsiooni andmed sisestanud loomisel ja toote salvestusruumi haldamine.  Pole reguleeritud kontrolli all andmete sisestamine järgmised väljad: andmebaasi nimi, tellimuse nime, ressursi rühmad, serverinime, serveri administraator sisselogimine, juurutamise nimed, ressursside nimed, ressursside Sildid

##  <a name="next-steps"></a>Järgmised sammud

Jaoks täiendav teave ja värskendused tellida soovitud <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure'i Government ajaveeb.</a>
