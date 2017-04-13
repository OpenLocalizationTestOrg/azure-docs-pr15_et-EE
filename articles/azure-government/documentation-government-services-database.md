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
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Valitsuse Azure'i andmebaasid

##  <a name="sql-database"></a>SQL-andmebaas

Vaadake<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft turbekeskus andmebaasimootor SQL-i jaoks</a> ja [SQL Azure'i andmebaasi avaliku dokumentatsiooni](https://azure.microsoft.com/documentation/services/sql-database/) metaandmete nähtavuse konfiguratsiooni ja kaitse head tavad täiendavad juhised.

### <a name="variations"></a>Variatsioonid

SQL-andmebaasi V12 on üldiselt saadaval Azure Government.

SQL Azure'i server Azure'i Government aadress on erinevad:

Teenuse tüüp|Azure'i avaliku|Azure'i Government
---|---|---
SQL-andmebaas|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Kaalutlused

Järgmine teave tuvastab Azure SQL Azure'i Government äärist:

| Lubatud reguleeritud kontrolli all andmed | Andmete reguleeritud kontrolli all pole lubatud |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kõik andmed salvestatakse ja vaadatakse läbi Microsoft Azure SQL-i võivad sisaldada Azure'i valitsuse reguleeritud andmeid. Kasutage andmete edastamiseks Azure'i valitsuse reguleeritud andmete Andmebaasiriistad. | Azure SQL-i metaandmete on ei tohi sisaldada ekspordi kontrollitud andmed. Selle metaandmete sisaldab kõik konfiguratsiooni andmed sisestanud loomisel ja toote salvestusruumi haldamine.  Pole reguleeritud kontrolli all andmete sisestamine järgmised väljad: andmebaasi nimi, tellimuse nime, ressursside rühmad, serveri nimi, serveri administraator sisselogimine, juurutamise nimed, ressursside nimed, ressursside Sildid

## <a name="azure-redis-cache"></a>Azure'i Redis vahemälu

Selle teenuse ja kuidas seda kasutada kohta täpsema teabe saamiseks leiate [Azure'i Redis vahemälu avaliku dokumentatsioonist](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Variatsioonid

Juurdepääs ja haldamise Azure'i Redis vahemälu Azure Governmenti URL-id on erinevad.

Teenuse tüüp|Azure'i avaliku|Azure'i Government
---|---|---
Vahemälu lõpp-punkti|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure'i portaal|https://Portal.Azure.com|https://Portal.Azure.us

>[AZURE.NOTE] Kõik oma skriptide ja koodi peab konto jaoks sobiv lõpp-punktid ja keskkonnas. Lisateabe saamiseks vaadake, [Kuidas ühendada Azure Governmenti pilveteenusesse](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


### <a name="considerations"></a>Kaalutlused

Järgmine teave tuvastab Azure Governmenti äärist Azure'i Redis vahemälu jaoks:

| Lubatud reguleeritud kontrolli all andmed | Andmete reguleeritud kontrolli all pole lubatud |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kõik andmed, talletada ja töödelda Azure'i Redis vahemälu võivad sisaldada Azure'i valitsuse reguleeritud andmeid. | Azure'i Redis vahemälu metaandmed on ei tohi sisaldada ekspordi kontrollitud andmed. Pole reguleeritud kontrolli all andmete sisestamine järgmised väljad: vahemälu nimi, VNET nimi, tellimuse nime, ressursside rühmad, ressursside sildid, Redis atribuudid.  

##  <a name="next-steps"></a>Järgmised sammud

Jaoks täiendav teave ja värskendused tellida soovitud <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure'i Government ajaveeb.</a>
