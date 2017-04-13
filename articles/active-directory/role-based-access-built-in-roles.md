<properties
    pageTitle="RBAC: Sisseehitatud rollid | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse ehitatud rollid Rollipõhine juurdepääsu reguleerimine (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Sisseehitatud rollid

Azure'i Rollipõhine juurdepääsu juhtimine (RBAC) on kasutajate, rühmade ja teenuste määratud järgmised sisseehitatud rollid. Te ei saa muuta määratlused sisseehitatud rollid. Siiski saate luua [kohandatud rollid Azure'i RBAC](role-based-access-control-custom-roles.md) teatud oma ettevõtte vajadustele sobivaks.

## <a name="roles-in-azure"></a>Azure'i rollid

Järgmises tabelis kirjeldatakse põgusalt sisseehitatud rollid. Klõpsake rolli nime kuvamiseks **toimingute** ja **notactions** rolli üksikasjalik loend. Atribuudi **toiminguid** saate määrata lubatud toimingud Azure ressursid. Toimingu stringide saate kasutada metamärke. Atribuut **notactions** määrab toimingud, mis ei kuulu lubatud toiminguid.

>[AZURE.NOTE] Azure'i rolli määratlusi pidevalt. Selles artiklis on ajakohane nii kui võimalik, kuid leiate alati uusimate rollid määratlused Azure PowerShelli. Cmdlet-käskude kasutamine `(get-azurermroledefinition "<role name>").actions` või `(get-azurermroledefinition "<role name>").notactions` saadaval.

| Rolli nimi | Kirjeldus |
| --------- | ----------- |
| [API halduse teenuse kaasautor](#api-management-service-contributor) | Saate hallata API halduse teenused |
| [Rakenduse ülevaateid komponent kaasautor](#application-insights-component-contributor) | Saate hallata rakenduse ülevaated komponendid |
| [Automaatika tehtemärk](#automation-operator) | Käivitada, peatada, peatada ja jätkata tööd |
| [BizTalki kaasautor](#biztalk-contributor) | Saate hallata BizTalki teenused |
| [DB ClearDB MySQL-i kaasautor](#cleardb-mysql-db-contributor) | Saate ClearDB MySQL-i andmebaaside haldamine |
| [Kaasautor](#contributor) | Kõik, välja arvatud juurdepääsu saate hallata. |
| [Andmete Factory kaasautor](#data-factory-contributor) | Saate luua ja hallata andmeid tehased ja neis lapse ressursid. |
| [DevTest Labs kasutaja](#devtest-labs-user) | Saate kuvada kõik ja ühenduse, virtuaalse masinad start, uuesti ja sulgemine |
| [DNS-i tsooni kaasautor](#dns-zone-contributor) | DNS-i tsoonid ja kirjete haldamiseks |
| [DocumentDB konto kaasautor](#documentdb-account-contributor) | Saate hallata DocumentDB kontod |
| [Nutikad süsteemid konto kaasautor](#intelligent-systems-account-contributor) | Saate hallata Nutikad süsteemid kontod |
| [Võrgu kaasautor](#network-contributor) | Saate hallata kõigi võrgu ressursid |
| [Uue reliikvia APM konto kaasautor](#new-relic-apm-account-contributor) | Saate uue reliikvia Rakendusehaldus jõudluse kontod ja rakenduste haldamine |
| [Omanik](#owner) | Saate kõik, sh juurdepääsu haldamine |
| [Lugeja](#reader) | Saate vaadata kõike, kuid ei saa muuta |
| [Redis vahemälu kaasautor](#redis-cache-contributor) | Saate hallata Redis vahemälu |
| [Ajasti töö saidikogumid kaasautor](#scheduler-job-collections-contributor) | Saate hallata ajasti töö saidikogumid |
| [Otsingu teenuse kaasautor](#search-service-contributor) | Saate otsingu teenuste haldamine |
| [Turvalisus haldur](#security-manager) | Saate hallata turvalisus komponendid, turbepoliitikate ja virtuaalmasinates |
| [SQL-i DB kaasautor](#sql-db-contributor) | Saate hallata SQL andmebaase, kuid mitte turvalisusega seotud poliitika |
| [SQL-i turvalisus haldur](#sql-security-manager) | Saate hallata SQL-i serverite ja andmebaaside turvalisusega seotud poliitikad |
| [SQL serveriga kaasautor](#sql-server-contributor) | Saate hallata SQL-i serverite ja andmebaaside, kuid mitte turvalisusega seotud poliitika |
| [Klassikaline salvestusruumi konto kaasautor](#classic-storage-account-contributor) | Saate hallata klassikaline salvestusruumi kontod |
| [Salvestusruumi konto kaasautor](#storage-account-contributor) | Saate hallata salvestusruumi kontod |
| [Kasutaja juurdepääs administraator](#user-access-administrator) | Saate hallata kasutajate juurdepääsu Azure ressursid |
| [Klassikaline virtuaalse masina kaasautor](#classic-virtual-machine-contributor) | Saate hallata klassikaline virtuaalmasinates, kuid mitte virtuaalse võrgu või salvestusruumi konto on ühendatud |
| [Virtuaalse masina kaasautor](#virtual-machine-contributor) | Saate hallata virtuaalmasinates, kuid mitte virtuaalse võrgu või salvestusruumi konto on ühendatud |
| [Klassikaline võrgu kaasautor](#classic-network-contributor) | Saate hallata klassikaline virtuaalne võrkude ja reserveeritud IP-d |
| [Web leping kaasautor](#web-plan-contributor) | Saate hallata web lepingud |
| [Veebisaidi kaasautor](#website-contributor) | Saate hallata veebisaite, kuid mitte web lepingud on ühendatud |

## <a name="role-permissions"></a>Rolli õigused
Järgmises tabelis on kirjeldatud teatud antud iga rolli õigusi. Piira neid **toiminguid**, mida anda, ja **NotActions**, võite siia.

### <a name="api-management-service-contributor"></a>API halduse teenuse kaasautor
Saate hallata API halduse teenused

| **Toimingud** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Saate luua ja hallata API halduse teenused |
| Microsoft.Authorization/*/read | Luba lugemine |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lugege rollid ja rollimääranguid |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="application-insights-component-contributor"></a>Rakenduse ülevaateid komponent kaasautor
Saate hallata rakenduse ülevaated komponendid

| **Toimingud** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja rollimääranguid |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.Insights/components/* | Saate luua ja hallata ülevaateid komponendid |
| Microsoft.Insights/webtests/* | Saate luua ja hallata web testide |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="automation-operator"></a>Automaatika tehtemärk
Käivitada, peatada, peatada ja jätkata tööd

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja rollimääranguid |
| Microsoft.Automation/automationAccounts/jobs/read | Lugege automatiseerimise konto tööde haldamine |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Mõne automatiseerimise konto töö jätkamine |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Mõne automatiseerimise konto töö peatamine |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Lugege automatiseerimise konto töö voogu |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Peatada mõne automatiseerimise konto töö |
| Microsoft.Automation/automationAccounts/jobs/write | Kirjutage automatiseerimise konto tööde haldamine |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Lugege automatiseerimise konto töögraafik |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Lugege automatiseerimise konto töögraafik |
| Microsoft.Automation/automationAccounts/read | Lugege automatiseerimise kontod |
| Microsoft.Automation/automationAccounts/runbooks/read | Automaatika tegevusraamatud lugemine |
| Microsoft.Automation/automationAccounts/schedules/read | Lugege automatiseerimise konto ajakava |
| Microsoft.Automation/automationAccounts/schedules/write | Kirjutage automatiseerimise konto ajakava |
| Microsoft.Insights/components/* | Saate luua ja hallata ülevaateid komponendid |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="biztalk-contributor"></a>BizTalki kaasautor
Saate hallata BizTalki teenused

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja rollimääranguid |
| Microsoft.BizTalkServices/BizTalk/* | Saate luua ja hallata BizTalki teenused |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="cleardb-mysql-db-contributor"></a>DB ClearDB MySQL-i kaasautor
Saate ClearDB MySQL-i andmebaaside haldamine

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja rollimääranguid |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |
| successbricks.cleardb/Databases/* | Saate luua ja hallata ClearDB MySQL-i andmebaasid |

### <a name="contributor"></a>Kaasautor
Saate kõik, välja arvatud juurdepääsu haldamine

| **Toimingud** ||
| ------- | ------ |
| * | Saate luua ja hallata ressursid kõik tüübid |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Ei saa kustutada rollid ja rollimääranguid |  
| Microsoft.Authorization/*/Write | Ei saa luua rollid ja rollimääranguid |

### <a name="data-factory-contributor"></a>Andmete Factory kaasautor
Luua ja hallata andmeid tehased ja neis lapse ressursid.

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.DataFactory/dataFactories/* | Luua ja hallata andmeid tehased ja neis lapse ressursid. |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="devtest-labs-user"></a>DevTest Labs kasutaja
Saate kuvada kõik ja ühenduse, virtuaalse masinad start, uuesti ja sulgemine

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.Compute/availabilitySets/read | Kättesaadavus komplekti atribuutide lugemine |
| Microsoft.Compute/virtualMachines/*/read | Lugege atribuutide virtuaalse masina (VM suurused, käitusaja olek, VM laiendid jne) |
| Microsoft.Compute/virtualMachines/deallocate/action | Deallocate virtuaalmasinates |
| Microsoft.Compute/virtualMachines/read | Virtuaalse masina atribuutide lugemine |
| Microsoft.Compute/virtualMachines/restart/action | Taaskäivitage virtuaalmasinates |
| Microsoft.Compute/virtualMachines/start/action | Käivitage virtuaalmasinates |
| Microsoft.DevTestLab/*/read | Lab atribuutide lugemine |
| Microsoft.DevTestLab/labs/createEnvironment/action | Laborikeskkond loomine |
| Microsoft.DevTestLab/labs/formulas/delete | Valemite kustutamiseks |
| Microsoft.DevTestLab/labs/formulas/read | Lugege valemid |
| Microsoft.DevTestLab/labs/formulas/write | Lisage või muutke valemid |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Lab poliitikate hindamine |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Laadi koormusetasakaalustusteenuse kirjutamata aadress rakenduskausta liitumine |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Laadi koormusetasakaalustusteenuse liitumine sissetulev NAT reegel |
| Microsoft.Network/networkInterfaces/*/read | Võrgu liidese (nt kõik selle koormus soolise võrgu liidese on osa) atribuutide lugemine |
| Microsoft.Network/networkInterfaces/join/action | Võrgu liides virtuaalse masina liitumine |
| Microsoft.Network/networkInterfaces/read | Lugege võrgu liidesed |
| Microsoft.Network/networkInterfaces/write | Kirjutage võrgu liidesed |
| Microsoft.Network/publicIPAddresses/*/read | Avaliku IP-aadressi atribuutide lugemine |
| Microsoft.Network/publicIPAddresses/join/action | Liitumine avaliku IP-aadress |
| Microsoft.Network/publicIPAddresses/read | Lugege võrgu avaliku IP-aadressid |
| Microsoft.Network/virtualNetworks/subnets/join/action | Virtuaalne võrgustikuga |
| Microsoft.Resources/deployments/operations/read | Read juurutamise toimingud |
| Microsoft.Resources/deployments/read | Lugege juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Storage/storageAccounts/listKeys/action | Loendi salvestusruumi konto võtmed |

### <a name="dns-zone-contributor"></a>DNS-i tsooni kaasautor
DNS-i tsoonid ja kirjete haldamiseks.

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/\*/read | Lugege rollid ja rollimääranguid |
| Microsoft.Insights/alertRules/\* | Luua ja hallata |
| Microsoft.Network/dnsZones/\* | DNS-i tsoonid ja kirjete loomine ning haldamine |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/\* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/\* | Loomine ja tugiteenusepiletite haldamine |


### <a name="documentdb-account-contributor"></a>DocumentDB konto kaasautor
Saate hallata DocumentDB kontod

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.DocumentDb/databaseAccounts/* | Saate luua ja hallata DocumentDB kontod |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="intelligent-systems-account-contributor"></a>Nutikad süsteemid konto kaasautor
Saate hallata Nutikad süsteemid kontod

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.IntelligentSystems/accounts/* | Nutikad süsteemid kontode loomiseks ja haldamiseks |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="network-contributor"></a>Võrgu kaasautor
Saate hallata kõigi võrgu ressursid

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.Network/* | Saate luua ja hallata võrke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="new-relic-apm-account-contributor"></a>Uue reliikvia APM konto kaasautor
Saate uue reliikvia Rakendusehaldus jõudluse kontod ja rakenduste haldamine

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |
| NewRelic.APM/accounts/* | Saate luua ja hallata uue reliikvia rakenduse jõudluse haldus kontod |

### <a name="owner"></a>Omanik
Saate kõik, sh juurdepääsu haldamine

| **Toimingud** ||
| ------- | ------ |
| * | Saate luua ja hallata ressursid kõik tüübid |

### <a name="reader"></a>Lugeja
Saate vaadata kõike, kuid ei saa muuta

| **Toimingud** ||
| ------- | ------ |
| * / read | Lugege ressursid kõik tüübid, välja arvatud saladusi. |

### <a name="redis-cache-contributor"></a>Redis vahemälu kaasautor
Saate hallata Redis vahemälu

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.Cache/redis/* | Saate luua ja hallata Redis vahemälu |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="scheduler-job-collections-contributor"></a>Ajasti töö saidikogumid kaasautor
Saate hallata ajasti töö saidikogumid

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Scheduler/jobcollections/* | Loomine ja töö saidikogumite haldamine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine  |

### <a name="search-service-contributor"></a>Otsingu teenuse kaasautor
Saate otsingu teenuste haldamine

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Search/searchServices/* | Saate luua ja hallata otsing |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine  |

### <a name="security-manager"></a>Turvalisus haldur
Saate hallata turvalisus komponendid, turbepoliitikate ja virtuaalmasinates

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.ClassicCompute/*/read | Klassikaline arvutada virtuaalmasinates konfiguratsiooniteavet lugemine |
| Microsoft.ClassicCompute/virtualMachines/*/write | Kirjutage virtuaalmasinates konfigureerimine |
| Microsoft.ClassicNetwork/*/read | Klassikaline võrgu konfigureerimise lugege  |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Security/* | Saate luua ja hallata turvalisus komponendid ja -poliitikad |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine  |

### <a name="sql-db-contributor"></a>SQL-i DB kaasautor
Saate hallata SQL andmebaase, kuid mitte turvalisusega seotud poliitika

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege rollid ja ülesanded roll |
| Microsoft.Insights/alertRules/* | Luua ja hallata |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Sql/servers/databases/* | Saate luua ja hallata SQL andmebaase |
| Microsoft.Sql/servers/read | SQL-i serverite lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Auditilogi poliitikate ei saa redigeerida |
| Microsoft.Sql/servers/databases/auditingSettings/* | Ei saa redigeerida auditeerimissätted |
| Microsoft.Sql/servers/databases/auditRecords/read  | Ei saa lugeda auditilogi kirjed |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Ei saa redigeerida ühenduse poliitika |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Ei saa redigeerida masking poliitikate andmed |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Ei saa redigeerida turbepoliitikate teatis |
| Microsoft.Sql/servers/databases/securityMetrics/* | Ei saa redigeerida turvalisus mõõdikud |

### <a name="sql-security-manager"></a>SQL-i turvalisus haldur
Saate hallata SQL-i serverite ja andmebaaside turvalisusega seotud poliitikad

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lugege Microsoft autoriseerimine |
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Sql/servers/auditingPolicies/* | Loomine ja SQL serveri Auditeerimispoliitika poliitikate haldamine |
| Microsoft.Sql/servers/auditingSettings/* | Saate luua ja hallata SQL serveri Auditeerimispoliitika säte |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Loomine ja SQL serveri andmebaasi Auditeerimispoliitika poliitikate haldamine |
| Microsoft.Sql/servers/databases/auditingSettings/* | Saate luua ja hallata SQL serveri andmebaasi auditeerimissätete |
| Microsoft.Sql/servers/databases/auditRecords/read | Lugege auditilogi kirjed |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Loomine ja SQL Serveri andmebaasiga ühenduse poliitikate haldamine |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Loomine ja SQL serveri andmebaasi andmete masking poliitikate haldamine |
| Microsoft.Sql/servers/databases/read | Vt SQL andmebaase |
| Microsoft.Sql/servers/databases/schemas/read | Vt SQL serveri andmebaasi skeemid |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Vt SQL serveri andmebaasi tabeli veerud |
| Microsoft.Sql/servers/databases/schemas/tables/read | Vt SQL serveri andmebaasi tabelid |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Loomine ja SQL serveri andmebaasi teatiste turbepoliitikate haldus |
| Microsoft.Sql/servers/databases/securityMetrics/* | Saate luua ja hallata SQL serveri andmebaasi turvalisuse mõõdikute |
| Microsoft.Sql/servers/read | SQL-i serverite lugemine |
| Microsoft.Sql/servers/securityAlertPolicies/* | Loomine ja SQL serveri turbepoliitikate teatiste haldamine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="sql-server-contributor"></a>SQL serveriga kaasautor
Saate hallata SQL-i serverite ja andmebaaside, kuid mitte turvalisusega seotud poliitika

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luba lugemine|
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Sql/servers/* | Saate luua ja hallata SQL-i serverid |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Ei saa redigeerida SQL serveri Auditeerimispoliitika poliitika |
| Microsoft.Sql/servers/auditingSettings/* | Ei saa SQL server Auditeerimispoliitika sätete redigeerimine |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Ei saa redigeerida SQL serveri andmebaasi Auditeerimispoliitika poliitika |
| Microsoft.Sql/servers/databases/auditingSettings/* | SQL serveri andmebaasi auditeerimissätete ei saa redigeerida |
| Microsoft.Sql/servers/databases/auditRecords/read  | Ei saa lugeda auditilogi kirjed |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Ei saa redigeerida SQL Serveri andmebaasiga ühenduse poliitika |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Ei saa redigeerida SQL serveri andmebaasi andmete masking poliitika |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL serveri andmebaasi teatiste turbepoliitikate ei saa redigeerida |
| Microsoft.Sql/servers/databases/securityMetrics/* | Ei saa redigeerida SQL serveri andmebaasi turvalisuse mõõdikud |
| Microsoft.Sql/servers/securityAlertPolicies/* | Ei saa redigeerida SQL serveri turbepoliitikate teatis |

### <a name="classic-storage-account-contributor"></a>Klassikaline salvestusruumi konto kaasautor
Saate hallata klassikaline salvestusruumi kontod

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luba lugemine |
| Microsoft.ClassicStorage/storageAccounts/* | Saate luua ja hallata salvestusruumi kontod |
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="storage-account-contributor"></a>Salvestusruumi konto kaasautor
Saate hallata salvestusruumi kontod, kuid ei pääse neile.

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luba kõik lugemine |
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.Insights/diagnosticSettings/* | Diagnostika sätete haldamine |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Storage/storageAccounts/* | Saate luua ja hallata salvestusruumi kontod |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="user-access-administrator"></a>Kasutaja juurdepääs administraator
Saate hallata kasutajate juurdepääsu Azure ressursid

| **Toimingud** ||
| ------- | ------ |
| * / read | Lugege ressursid kõik tüübid, välja arvatud saladusi. |
| Microsoft.Authorization/* | Luba haldamine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="classic-virtual-machine-contributor"></a>Klassikaline virtuaalse masina kaasautor
Saate hallata klassikaline virtuaalmasinates, kuid mitte virtuaalse võrgu või salvestusruumi konto on ühendatud

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luba lugemine |
| Microsoft.ClassicCompute/domainNames/* | Saate luua ja hallata klassikaline Arvuta domeeninimed |
| Microsoft.ClassicCompute/virtualMachines/* | Saate luua ja hallata virtuaalmasinates |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Võrgu turberühmad liitumine |
| Microsoft.ClassicNetwork/reservedIps/link/action | Link: reserveeritud IP-d |
| Microsoft.ClassicNetwork/reservedIps/read | Lugege reserveeritud IP-aadressid |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Virtuaalne võrkude liitumine |
| Microsoft.ClassicNetwork/virtualNetworks/read | Virtuaalne võrkude lugemine |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Salvestusruumi konto ketast lugemine |
| Microsoft.ClassicStorage/storageAccounts/images/read | Lugege salvestusruumi konto pildid |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Loendi salvestusruumi konto võtmed |
| Microsoft.ClassicStorage/storageAccounts/read | Lugege klassikaline salvestusruumi kontod |
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="virtual-machine-contributor"></a>Virtuaalse masina kaasautor
Saate hallata virtuaalmasinates, kuid mitte virtuaalse võrgu või salvestusruumi konto on ühendatud

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luba lugemine |
| Microsoft.Compute/availabilitySets/* | Saate luua ja hallata Arvuta kättesaadavus komplektid |
| Microsoft.Compute/locations/* | Saate luua ja hallata Arvuta asukohad |
| Microsoft.Compute/virtualMachines/* | Saate luua ja hallata virtuaalmasinates |
| Microsoft.Compute/virtualMachineScaleSets/* | Saate luua ja hallata virtuaalse masina skaala komplektid |
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Võrgus taotluse lüüsi kirjutamata aadress kaustu liitumine |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Laadi koormusetasakaalustusteenuse kirjutamata aadress kaustu liitumine |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Laadi koormusetasakaalustusteenuse liitumine sissetulev NAT kaustu |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Laadi koormusetasakaalustusteenuse liitumine sissetulev NAT reeglid |
| Microsoft.Network/loadBalancers/read | Lugege koormus soolise |
| Microsoft.Network/locations/* | Saate luua ja hallata võrgukohtades |
| Microsoft.Network/networkInterfaces/* | Saate luua ja hallata võrgu liidesed |
| Microsoft.Network/networkSecurityGroups/join/action | Võrgu turberühmad liitumine |
| Microsoft.Network/networkSecurityGroups/read | Võrgu turberühmad lugemine |
| Microsoft.Network/publicIPAddresses/join/action | Liitumine võrgu avaliku IP-aadressid |
| Microsoft.Network/publicIPAddresses/read | Lugege võrgu avaliku IP-aadressid |
| Microsoft.Network/virtualNetworks/read | Virtuaalne võrkude lugemine |
| Microsoft.Network/virtualNetworks/subnets/join/action | Virtuaalne alamvõrku liitumine |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Storage/storageAccounts/listKeys/action | Loendi salvestusruumi konto võtmed |
| Microsoft.Storage/storageAccounts/read | Lugege salvestusruumi kontod |
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="classic-network-contributor"></a>Klassikaline võrgu kaasautor
Saate hallata klassikaline virtuaalne võrkude ja reserveeritud IP-d

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luba lugemine |
| Microsoft.ClassicNetwork/* | Saate luua ja hallata klassikaline võrkude |
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |

### <a name="web-plan-contributor"></a>Web leping kaasautor
Saate hallata web lepingud

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luba lugemine |
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |
| Microsoft.Web/serverFarms/* | Saate luua ja hallata serveri parkides |

### <a name="website-contributor"></a>Veebisaidi kaasautor
Saate hallata veebisaite, kuid mitte web lepingud on ühendatud

| **Toimingud** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luba lugemine |
| Microsoft.Insights/alertRules/* | Luua ja hallata ülevaateid |
| Microsoft.Insights/components/* | Saate luua ja hallata ülevaateid komponendid |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ressursside seisundi lugemine |
| Microsoft.Resources/deployments/* | Saate luua ja hallata ressursside rühma juurutuste |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressursi rühmade lugemine |  
| Microsoft.Support/* | Loomine ja tugiteenusepiletite haldamine |
| Microsoft.Web/certificates/* | Loomine ja haldamine veebisaidil serdid |
| Microsoft.Web/listSitesAssignedToHostName/read | Hosti nimi määratud saitide lugemisõigus |
| Microsoft.Web/serverFarms/join/action | Serveri parkides liitumine |
| Microsoft.Web/serverFarms/read | Serveri parkides lugemine |
| Microsoft.Web/sites/* | Saate luua ja hallata veebisaidid |

## <a name="see-also"></a>Vt ka
- [Rollipõhine juurdepääsu reguleerimine](role-based-access-control-configure.md): alustamine RBAC Azure'i portaalis.
- [Azure'i RBAC rollide kohandatud](role-based-access-control-custom-roles.md): saate teada, kuidas luua kohandatud rollid vastavalt vajadusele juurdepääsu.
- [Loo Accessi muuta ajalugu aruande](role-based-access-control-access-change-history-report.md): silma peal muutmisega rollimääranguid RBAC sisse.
- [Rollipõhine juurdepääsu reguleerimine tõrkeotsing](role-based-access-control-troubleshooting.md): leida levinud probleemidele soovitused.
