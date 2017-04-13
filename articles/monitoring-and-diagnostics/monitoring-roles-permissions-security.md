<properties
    pageTitle="Alustamine rollid, õiguste ja turbe Azure'i monitoris | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure kuvari sisseehitatud rollide ja õiguste juurdepääsu piiramiseks jälgimise ressursid."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Rollid, õiguste ja turbe Azure'i monitoris kasutamise alustamine

Paljud meeskonnad vaja tingimata reguleerida juurdepääsu andmed ja sätted. Näiteks, kui teil on meeskonna liikmed, kes töötavad ainult jälgimine (devops insenerid toe töötajad) või kui kasutate hallatavate teenuse pakkuja, mida soovite anda neile juurdepääs ainult jälgida andmeid, kuid piirata nende loomiseks, muuta või kustutada ressursid. Selles artiklis kirjeldatakse kiiresti rakendada mõne Sisseehitatud RBAC rolli Azure kasutaja või luua oma kohandatud rolliga kasutaja, kes vajab piiratud jälgimisega seotud õiguste kohta. Seejärel arutatakse oma Azure kuvari seotud ressursid ja kuidas saate piirata juurdepääsu sisalduvate andmete turvakaalutlused.

## <a name="built-in-monitoring-roles"></a>Sisseehitatud jälgimisega seotud rollid

Azure'i kuvari sisseehitatud rollid on loodud ressursside tellimust samas neile, kes vastutab taristu hankimine ja konfigureerimiseks tuleb need andmed võimaldavad juurdepääsu piirata. Azure'i kuvari pakub kahte out-of-box rollid: A jälgimise lugeja ja kaasautor jälgimise.

### <a name="monitoring-reader"></a>Lugeja jälgimine

Isikute jälgimise lugeja rollile määratud saate tellimuse kõik jälgimisega seotud andmete kuvamiseks, kuid ei saa muuta mis tahes ressursi või mis tahes seotud ressursid jälgimiseks sätete redigeerimine. Sellel rollil on sobiv organisatsiooni alluvussuhted, näiteks tugi või toimingute insenerid, kellel on vaja on võimalik, et kasutajad:

- Saate vaadata jälgimisega seotud armatuurlaudade portaalis ja oma jälgimisega seotud era armatuurlaudade loomine.
- [Azure'i kuvari REST API -ga](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShelli cmdlet-käskude](insights-powershell-samples.md)või [mitu platvormi CLI](insights-cli-samples.md)mõõdikute päring.
- Päringu tegevuste Logi portaali, Azure'i kuvari REST API-ga, PowerShelli cmdlet-käskude või mitu platvormi CLI abil.
- Ressursi [diagnostikasätete](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) vaadata.
- [Logi profiili](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) tellimuse vaadata.
- Vaate autoscale sätted.
- Kuva teatis tegevuste ja sätted.
- Juurdepääs rakenduse ülevaated andmed ja AI Analytics andmete kuvamine.
- Log Analytics (OMS) tööruumi andmeid, sh kasutusandmete tööruumi otsida.
- Rühmade kuvamine Log Analytics (OMS) haldus.
- Saate tuua otsinguskeemi Log Analytics (OMS).
- Loendi Log Analytics (OMS) Ajateabe pakendit.
- Alla laadida ja käivitada Log Analytics (OMS) salvestatud otsingud.
- Saate tuua Log Analytics (OMS) salvestusruumi konfigureerimine.

> [AZURE.NOTE] Selle rolli ei anna voona sündmuse jaoturiga või talletatud salvestusruumi konto Logi andmete lugemisõigus. Juurdepääsu nende ressursside konfigureerimise kohta lisateabe saamiseks [vt allpool](#security-considerations-for-monitoring-data) .

### <a name="monitoring-contributor"></a>Kaasautor jälgimise

Inimesed, kellele määratakse kaasautor jälgimise roll saate vaadata kõiki andmeid tellimuse ja luua või muuta jälgimise sätted, kuid ei saa muuta muud ressursid. Sellel rollil on jälgimise lugeja roll versioonist ja vastav ettevõtte jälgimisega seotud meeskonnatöö või hallatavate teenuse pakkujad, kes lisaks eeltoodud õigused ka peavad saama.

- Ühiskasutusega armatuurlaua jälgimisega seotud armatuurlaudade avaldada.
- Määrake soovitud resource.* [diagnostikasätete](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)
- [Logi profiili](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) lisamine subscription.* seadmine
- Saate määrata teatiste tegevuste ja sätted.
- Saate luua rakenduse ülevaated web kontrollib ja komponendid.
- Tööruumi loendi Log Analytics (OMS) jagatud võtmed.
- Lubada või keelata Logi Analytics (OMS) ärianalüüsi paketid.
- Loomine ja kustutamine ja käivitada Log Analytics (OMS) salvestatud otsingud.
- Loomine ja kustutamine Log Analytics (OMS) salvestusruumi konfigureerimine.

* kasutaja peab ka eraldi juurdepääsuõigusi ListKeys target ressurss (salvestusruumi konto või sündmuse jaoturi nimeruum) määrata või Logi profiili diagnostika seade.

> [AZURE.NOTE] Selle rolli ei anna voona sündmuse jaoturiga või talletatud salvestusruumi konto Logi andmete lugemisõigus. Juurdepääsu nende ressursside konfigureerimise kohta lisateabe saamiseks [vt allpool](#security-considerations-for-monitoring-data) .

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Õigused ja kohandatud RBAC rollid
Kui eeltoodud sisseehitatud rollid ei vasta teie meeskond täpse vajadustele, saate [luua kohandatud RBAC rolli](../active-directory/role-based-access-control-custom-roles.md) täpsema õigustega. Allpool leiate Azure'i kuvari RBAC levinud toiminguid ja nende kirjeldused.

| Toiming                                                   | Kirjeldus                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read kirjutamine, kustutada]         | Lugemine ja kirjutamine/Kustuta teatis reeglid.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Teatiste reeglite loendi juhtumite (on käivitatud reegli ajalugu). See kehtib ainult portaali.                                              |
| Microsoft.Insights/AutoscaleSettings/[Read kirjutamine, kustutada]  | Kustuta/lugemine ja kirjutamine autoscale sätted.                                                                                                                     |
| Microsoft.Insights/DiagnosticSettings/[Read kirjutamine, kustutada] | Kustuta/lugemine ja kirjutamine diagnostikasätete.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Kasutajad, kellel on vaja logid portaali kaudu juurdepääsu tuleb see õigus.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Loetle tellimuse tegevuste Logi sündmuste (halduse sündmused). See õigus kehtib nii programmilisi ja portaali juurdepääsu tegevuste Logi. |
| Microsoft.Insights/LogDefinitions/Read                      | Kasutajad, kellel on vaja logid portaali kaudu juurdepääsu tuleb see õigus.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Lugege argumendil määratlused (saadaval argumendil tüübid ressursi loend).                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Lugege mõõdikute ressursi.                                                                                                                              |

> [AZURE.NOTE] Juurdepääs teatised ja diagnostikasätete mõõdikute ressursi nõuab, et kasutajal on lugemisõigus ressursi tüüp ja selle ressursi ulatust. Loomine ("kirjutamise") diagnostika säte või Logi profiili, mis Arhiiv salvestusruumi konto või voole, et sündmuse jaoturi nõuab kasutajal on ka ListKeys õigus target ressurss.

Näiteks, kasutades ülaltoodud tabeli, saate luua kohandatud RBAC rolli jaoks mõne "tegevuste Logi lugeja" umbes järgmine:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Andmete jälgimine turvakaalutlused
Andmete jälgimine – eriti logifailid – võib sisaldada tundlikku teavet nagu IP-aadresside või kasutajanimed. Andmete Azure sisaldab kolmes.
1. Activity Log, mis kirjeldab Azure tellimuse juhtelemendi-plane kõik toimingud.
2. Diagnostikalogide, mis on ressursi kiiratava logid.
3. Mõõdikute ressursid põhjustel.

Kõigi kolme tüüpi andmed saate salvestusruumi konto talletatud või voona sündmuse jaoturi, mis on üldotstarbeline Azure ressursse. Kuna need on üldotstarbeline ressursid, loomine, kustutamine ja neile juurdepääs on tavaliselt reserveeritud administraatori õigustega toiming. Soovitame kasutada järgmiste täitmisel jälgimisega seotud ressursid vältida väärkasutamine:

- Ühe, pühendunud salvestusruumi konto abil jälgida andmeid. Kui teil on vaja andmeid eraldi mitme salvestusruumi kontod, Jaga salvestusruumi konto vahel kasutamist ja -jälgimise andmed, kuna see võib tahtmatult anda neile, kes vajavad ainult juurdepääsu jälgida andmeid (nt. kolmanda osapoole SIEM) juurdepääsu jälgimise andmed.
- Kasutage ühe, pühendunud teenuse siini või sündmuse jaoturi nimeruum üle kõik diagnostikasätete samal põhjusel nagu eespool kirjeldatud.
- Piirata juurdepääsu jälgimisega seotud salvestusruumi kontod või sündmuse jaoturi, hoides neid eraldi ressursirühm ja [kasutada ulatuse](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) oma jälgimisega seotud rollid ainult selle ressursirühma juurdepääsu piirata.
- Kui ainult peab kasutaja juurdepääsu jälgida andmeid saate kunagi anda ListKeys õiguse salvestusruumi kontod või sündmuse jaoturi tellimuse ulatuses. Selle asemel anda need õigused ressursi või ressursirühma kasutajale (kui teil on spetsiaalne jälgimisega seotud ressursirühm) ulatust.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Salvestusruumi jälgimisega seotud kontod juurdepääsu piiramine
Kui kasutaja või rakendus vajab juurdepääsu jälgida salvestusruumi konto andmeid, tuleks [luua mõne konto SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx) teenusetaseme kirjutuskaitstud juurdepääsu bloobimälu jälgimisega seotud andmeid sisaldava konto salvestusruumi. PowerShelli, see võib välja näha:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Seejärel saate anda luba üksus, et vajab lugeda selle salvestusruumi konto, ja saate loendi ja lugeda kõik plekid salvestusruumi konto.

Teine võimalus, kui teil on vaja see õigus RBAC abil määrata, saate anda selle üksuse Microsoft.Storage/storageAccounts/listkeys/action luba sellel kontol kindla salvestusruumi. See on vajalik kasutajatele, kes peavad saama diagnostika sätte määramine või arhiivida profiili logida salvestusruumi konto. Näiteks võite luua kohandatud järgmist RBAC rolli kasutaja või rakendus, mis tuleb ainult üks salvestusruumi konto lugemise:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] ListKeys õigus võimaldab kasutajal loendis esmaseid ja teiseseid salvestusruumi konto võtmed. Need võtmed andke kasutajale kõik allkirjastatud õigused (lugeda, kirjutada, plekid loomine, kustutamine plekid jne) kõigis allkirjastatud salvestusruumi konto teenused (bloobimälu, järjekorda, tabel, fail). Soovitame kasutada konto SAS, mis eespool kirjeldatud, kui võimalik.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Sündmuse jälgimisega seotud jaoturi juurdepääsu piiramine
Sündmuse jaoturi abil saate järgida sarnane, kuid esmalt peate sihtotstarbeline kuulata autoriseerimine reegli loomiseks. Kui soovite anda juurdepääsu rakendus, mis ainult vajab jälgimisega seotud sündmuse jaoturi kuulata, tehke järgmist.

1. Luua ühiskasutusega juurdepääsu poliitika sündmuse hub(s) loodud streaming ainult kuulata nõuete jälgimisega seotud andmeid. Seda saab teha portaalis. Näiteks võite kutsuvad seda "monitoringReadOnly." Kui võimalik, mida soovite anda selle klahvi otse tarbija ja järgmise juhise vahele jätta.
2. Kui tarbija peab saama põhilistest erakorralist, andke kasutajale ListKeys toimingu event jaoturi. See on vajalik kasutajatele, kes peavad saama diagnostika sätte määramine või logige profiili voo sündmuse jaoturi abil. Näiteks võite luua reegli RBAC:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Järgmised sammud
- [Lugege RBAC ja ressursside halduri õigused](../active-directory/role-based-access-control-what-is.md)
- [Lugege ülevaade Azure jälgimine](monitoring-overview.md)
