<properties
    pageTitle="Microsoft Azure'i tellimus ja teenuste piirangud, kvoote ja piiranguid"
    description="Loetletakse levinud Azure tellimuse ja teenuste piirangud, kvootide ja piiranguid. See sisaldab teavet, kuidas suurendada koos maksimumväärtused piirangud."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure'i tellimus ja teenuste piirangud, kvoote ja piiranguid

## <a name="overview"></a>Ülevaade

Seda dokumenti saate määrata mõne enamlevinud Microsoft Azure'i piirangud. See ei hõlma praegu kõik Azure teenused. Aja jooksul need piirangud on laiendatud ja värskendatud platvormi laiendada.

[Azure'i hinnad ülevaade](https://azure.microsoft.com/pricing/) Azure hindade kohta lisateabe saamiseks külastage. Seal saate hinnata teie kulude [Kalkulaator hinnad](https://azure.microsoft.com/pricing/calculator/) kasutamine või külastades hinnakirjad üksikasjade lehel teenuse (nt [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Kui soovite **Vaikimisi limiit**piirmäära, saate [avada mõne Online'i kliendi tugiteenuse taotluse tasuta](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Ei saa tõsta kohal järgmistes tabelites on **Lubatud** väärtus. Kui pole **Lubatud** veerg, siis ei ole määratud ressursi reguleeritava piirangud.

## <a name="limits-and-the-azure-resource-manager"></a>Piirangud ja Azure ressursihaldur

Nüüd on võimalik ühendada mitu Azure ressursside ühe Azure'i ressursirühma. Ressursi rühmade kasutamisel piirangud, mis on üks kord globaalsed hakata haldama Azure'i ressursihaldur piirkondliku tasemel. Azure'i ressursi rühmade kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md).

Allpool seotud piirangud uude tabelisse on lisatud kajastamiseks piirangud erinevused Azure'i ressursihaldur kasutamisel. Näiteks on **Tellimuse piirid - Azure'i ressursihaldur** tabeli ja **Tellimuse piirangud** tabeli. Kui piirang kehtib nii stsenaariumid, siis kuvatakse ainult esimese tabeli. Teisiti piirangud on globaalne kõigis piirkondades.

> [AZURE.NOTE] See on oluline rõhutada, et kvootide ressursid Azure'i ressursi rühmade kohta – piirkond pääseb teie tellimus on, ja ei ole tellimuse eest teenuste haldus kvootide on. Näiteks kasutame core piirmäärasid. Kui soovite taotleda kvoodi suurendamist toega tuuma, peate otsustada, mida soovite kasutada piirkonnad ja seejärel tehke Azure'i ressursirühm core kvootide taotluse summad ja regioonid, mille soovite mitu südamikud. Seetõttu, kui teil on vaja 30 tuuma Lääne Euroopa abil saate käivitada rakenduse; konkreetselt tuleks taotleda 30 poolide Lääne Europe. Aga on teil core kvoodi suurendage teiste piirkonnas - ainult Lääne Euroopa on 30-core kvoodi.
<!-- -->
Seetõttu võib-olla kasulik arvesse võtta, mis oma Azure'i ressursirühm kvootide peavad olema oma töökoormus mis tahes ühe piirkonna jaoks sobivaima ja taotleda iga piirkonna kohta, kuhu te kaalute juurutamise summa. Vaadake teemat abi saamiseks oma praeguse kvootide teatud piirkondadele avastamine [juurutamise probleemide tõrkeotsing](./resource-manager-common-deployment-errors.md) .

## <a name="service-specific-limits"></a>Teenuse kohased piirangud

- [Active Directory](#active-directory-limits)
- [API haldus](#api-management-limits)
- [Rakenduse teenus](#app-service-limits)
- [Rakenduse lüüsi](#application-gateway-limits)
- [Rakenduse ülevaated](#application-insights-limits)
- [Automatiseerimine](#automation-limits)
- [Azure'i Redis vahemälu](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Varundus](#backup-limits)
- [Paketi](#batch-limits)
- [BizTalki teenused](#biztalk-services-limits)
- [CDN-ID](#cdn-limits)
- [Pilveteenused](#cloud-services-limits)
- [Andmete Factory](#data-factory-limits)
- [Andmeanalüüsi Lake](#data-lake-analytics-limits)
- [DNS-I](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Sündmuse jaoturi](#event-hubs-limits)
- [Asjade jaoturi](#iot-hub-limits)
- [Võtme Vault](#key-vault-limits)
- [Media Services](#media-services-limits)
- [Mobiilse kaasamine](#mobile-engagement-limits)
- [Mobiilse teenused](#mobile-services-limits)
- [Jälgimine](#monitoring-limits)
- [Mitmikautentimise](#multi-factor-authentication)
- [Võrgunduse](#networking-limits)
- [Jaoturi teavitusteenuse](#notification-hub-service-limits)
- [Töö ülevaated](#operational-insights-limits)
- [Ressursirühm](#resource-group-limits)
- [Ajasti](#scheduler-limits)
- [Otsing](#search-limits)
- [Teenuse siini](#service-bus-limits)
- [Saidi taastamine](#site-recovery-limits)
- [SQL-andmebaas](#sql-database-limits)
- [Salvestusruumi](#storage-limits)
- [StorSimple süsteemi](#storsimple-system-limits)
- [Voo Analytics](#stream-analytics-limits)
- [Tellimuse](#subscription-limits)
- [Liikluse haldur](#traffic-manager-limits)
- [Virtuaalmasinates](#virtual-machines-limits)
- [Virtuaalse masina skaala komplektid](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Tellimuse piirangud
#### <a name="subscription-limits"></a>Tellimuse piirangud
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Tellimuse piirid - Azure'i ressursihaldur

Järgmised piirangud kehtivad, kasutades Azure ressursihaldur ja Azure ressursi rühmad. Allpool on loetletud piirangud, mis on muutunud Azure'i ressursihaldur. Vaadake eelmisse tabelisse jaoks need piirangud.

Töötlemise piirangud ressursihaldur taotluste kohta leiate teavet teemast [pidurdamise ressursihaldur taotlused](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Ressursirühm piirangud

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Virtuaalmasinates piirangud
#### <a name="virtual-machine-limits"></a>Virtuaalse masina piirangud
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Azure'i ressursihaldur - Virtuaalmasinates limiitide kohta

Järgmised piirangud kehtivad, kasutades Azure ressursihaldur ja Azure ressursi rühmad. Allpool on loetletud piirangud, mis on muutunud Azure'i ressursihaldur. Vaadake eelmisse tabelisse jaoks need piirangud.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Virtuaalse masina skaala piirmäärad

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Võrgunduse piirangud

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Võrgunduse piirangud
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Rakenduse lüüsi piirab

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Liikluse haldur piirangud

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-i piirangud

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Salvestuslimiidid

Üksikasjalikumat salvestuslimiidi konto, leiate [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohad](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Salvestuslimiitide teenus

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Virtuaalne arvuti kettale piirangute

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Lisateabe saamiseks vt [virtuaalse masina suurused](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Kindlad kontod**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Premium salvestusruumi kontod**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Salvestuslimiidi ressursi pakkuja

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Pilveteenuse teenuste piirangud

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Rakenduse teenuse piirangud
Järgmised rakenduse teenuste piirangud kaasata piirangud veebirakenduste, mobiilirakenduste, API rakendused ja loogika rakendused.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Ajasti piirangud

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Paketi piirangud

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>BizTalki teenuste piirangud
Järgmine tabel näitab piiranguid Azure'i BizTalki teenuste jaoks.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>DocumentDB piirangud

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Kvootide loetletud [saab reguleerida Azure klienditoega](./documentdb/documentdb-increase-limits.md)on tärn (*).

### <a name="mobile-engagement-limits"></a>Mobile kaasamine piirangud

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Otsingu piirangud

Hinnakirjad astme kindlaks võimsus ja oma otsingu teenuse piirangud. Järgmised astme.

- *Tasuta* mitme rentniku teenus ühiskasutusse teiste Azure'i tellijad mõeldud ja small projektid.
- *Tavaline* pakub sihtotstarbeline ressursside tootmise töökoormus veebisaidil väiksemas kuni kolm koopiad tugevalt saadaval päringu töökoormus.
- *Standard (S1, S2, S3, S3 suure tihedusega)* on suurem tootmise töökoormus. Mitu taset olemas standard taseme nii, et saate valida jaoks teatud stsenaariumide ressursi konfiguratsiooni.

**Tellimuse kohta piirangud**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Otsinguteenuse kohta piirangud**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Muud piirangud, sh dokumendi suurus päringute arv teise, võtmed, taotlusi ja vastuseid, täpsema teavet leiate [Azure'i otsingu teenuse piirangud](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Servicesi piirangud

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN piirangud

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Mobile teenuste piirangud

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Jälgimisega seotud piirangud

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Teatis jaoturi teenuste piirangud

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Sündmuse jaoturi piirangud

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Teenuse siini piirangud

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Asjade jaoturi piirangud

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Andmete Factory piirangud

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Andmete Lake Analytics piirangud
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Voo Analytics piirangud
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory piirangud

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp piirangud

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple süsteemi piirangud

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Funktsionaalseid ülevaateid piirangud

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Varukoopia piirangud

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Saidi taastamise piirangud

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Rakenduse ülevaateid piirangud

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API haldamise piirangud

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure'i Redis vahemälu piirangud

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Klahv Vault piirangud

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Mitmikautentimise
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automaatika piirangud
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>SQL-andmebaasi piirangud

SQL-andmebaasi piirangud, vt [SQL-i andmebaasi ressursi piirangud](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Vt ka

[Azure'i piirangud ja suurendab mõistmine](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuaalse masina ja pilvepõhise teenuse suurused Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Pilveteenustega suurus](cloud-services/cloud-services-sizes-specs.md)
