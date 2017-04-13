<properties
    pageTitle="PowerShelli kasutamine loomine ja konfigureerimine Log Analytics tööruumi | Microsoft Azure'i"
    description="Logige oma kohapealse serveritest Analytics kasutab andmete või pilvepõhises taristu. Azure'i salvestusruumi Azure diagnostika loomisel saate kohapeal andmete kogumine."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Log Analytics PowerShelli kaudu hallata

Saate kasutada Log Analytics PowerShelli cmdletid (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) abil saate kasutada mitmesuguseid funktsioone Log Analytics käsurea või skripti osana.  PowerShelli abil saate teha tööülesanded näited.

+ Tööruumi loomine
+ Lisage või eemaldage lahendus
+ Importimine ja eksportimine salvestatud otsingud
+ Arvuti rühma loomine
+ Kogum arvutitest IIS-i logid Windows agendi installimisel lubamine
+ Jõudluse hinnale kogumine Linuxi ja Windowsi arvutisse
+ Koguda sündmuste logi Linux arvutites 
+ Windowsi sündmuselogide sündmuste kogumine
+ Kohandatud sündmuselogide kogumine
+ Azure'i virtuaalarvuti Logi analytics agent lisamine
+ Log analytics Azure diagnostika abil kogutud andmete indekseerimiseks konfigureerimine


Selles artiklis on toodud kaks koodinäiteid, mis illustreerivad mõned funktsioonid, mida saate teha PowerShelli kaudu.  Viidata saate [Log Analytics PowerShelli cmdleti viide] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) muude funktsioonide jaoks.

> [AZURE.NOTE] Log Analytics kutsuti varem töö ülevaated, mis on, miks see on nimi, mida kasutatakse cmdlet-käsud.

## <a name="prerequisites"></a>Eeltingimused

PowerShelli kasutamine tööruumis Log Analytics, peab teil olema:

+ Azure'i tellimuse ja 
+ Azure'i Log Analytics tööruumi lingitud Azure tellimuse.

Kui olete loonud mõne OMS tööruumi, kuid see ei ole veel lingitud selle Azure märkimiseks, saate luua lingi:

+ Azure'i portaalis
+ OMS portaalis või 
+ Get-AzureRmOperationalInsightsLinkTargets ja uus-AzureRmOperationalInsightsWorkspace cmdlet-käskude abil.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Loomine ja konfigureerimine Log Analytics tööruumi

Skripti järgmises näites on näidatud, kuidas:

1.  Tööruumi loomine
2.  Loendis Saadaolevad lahendused
3.  Lahenduste lisamine tööruumi
4.  Salvestatud impordi otsingud
5.  Salvestatud ekspordi otsingud
6.  Arvuti rühma loomine
7.  Kogum arvutitest IIS-i logid Windows agendi installimisel lubamine
8.  Linux arvutisse loogiline ketas täiuslik hinnale kogumine (% kasutatud Inodes; Tasuta MB; % Kasutatud ruumi; Ketas edastamine sekundis; Kui teil on järgmine sekundis; Ketas kirjutab sekundis)
9.  Sündmuste logi kogumine Linux arvutites
10. Koguda viga ja hoiatus sündmuste kaudu rakenduse sündmuselogi Windowsi arvutitest
11. Mälu Saadaval megabaiti Jõudluseloenduri kogumine Windowsi arvutisse
12. Kohandatud Logi kogumine 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Log Analytics registrisse Azure diagnostika konfigureerimine 

Agentless jälgimise Azure ressursse, ressursside peab olema lubatud ja konfigureeritud kirjutamiseks salvestusruumi konto Azure diagnostika. Log Analytics saab konfigureerida koguda logid salvestusruumi konto. Ressursid, mida peate tegema eelmise konfiguratsiooni sisaldab järgmist:

+ Klassikaline pilveteenustega (web ja töötaja rollid)
+ Teenus struktuuri kogumite
+ Võrgu turberühmad
+ Võti võlvid ja 
+ Rakenduse lüüsid

PowerShelli abil konfigureerimine Log Analytics tööruumi ühe Azure tellimuse koguda logid erinevate Azure'i tellimused.

Järgmises näites tehke järgmist:

1.  Olemasoleva salvestusruumi kontod ja kohtades, mida Log Analytics on indekseerimine andmeid
2.  Konfiguratsiooni lugeda salvestusruumi konto loomine
3.  Täiendavad asukohtadest viiteandmete vastloodud konfiguratsiooni värskendamine
4.  Äsja loodud konfiguratsiooni kustutamine

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Järgmised sammud

- [Läbivaatus Log Analytics PowerShelli cmdlet-käskude](http://msdn.microsoft.com/library/mt188224.aspx) konfigureerimiseks Log Analytics PowerShelli kasutamise kohta lisateabe saamiseks.

