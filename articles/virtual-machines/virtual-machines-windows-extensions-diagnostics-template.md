<properties
    pageTitle="Luua Windowsi virtuaalse masina jälgida ja diagnostika Azure'i ressursihaldur malli abil | Microsoft Azure'i"
    description="Azure'i ressursi halduri malli abil saate luua uue virtuaalse masina Windows Azure'i diagnostika laiendiga."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Luua Windowsi virtuaalse masina jälgida ja diagnostika Azure'i ressursihaldur malli abil

Azure'i diagnostika laiend pakub jälgimine ja vastavalt diagnostika võimaluste Windows Azure virtuaalse masina. Saate lubada virtual arvutisse neid võimalusi, sh laiend azure ressursi halduri malli osana. Leiate [Azure'i ressursihaldur mallide VM laiendiga loome](virtual-machines-windows-extensions-authoring-templates.md) , sh kõigile laienditele virtuaalse masina malli osana kohta lisateabe saamiseks. Selles artiklis kirjeldatakse, kuidas saate lisada Azure diagnostika laiend Windowsi virtuaalse masina malli.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Azure'i diagnostika laiend lisamine VM ressursside määratlemine 

Diagnostika laiendamiseks klõpsake Windowsi virtuaalse masina peate lisama laiendamine VM ressurssi ressursi halduri malli.

Lihtne ressursihaldur jaoks vastavalt virtuaalse masina lisamine laiend konfiguratsiooni *ressursid* massiiv virtuaalse masina: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Mõne muu levinud mess on lisada laiend konfiguratsiooni juur ressursse sõlm malli asemel määratledes virtuaalse masina ressursid sõlme all. Seda moodust teil määrata hierarhiliste seoste laiendamine ja virtuaalse masina *nimi* ja *Tüüp* väärtustega. Näiteks: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Laiendamine on alati seostatud virtuaalse masina, võite otse määratlemine virtuaalse masina ressursi sõlme all otse või määratleda base tasemel ja hierarhiliste nimetamistava seostada virtuaalse masina abil.

Virtuaalse masina skaala komplektide laiendid konfiguratsioon on määratud *VirtualMachineProfile* *extensionProfile* atribuuti.
   
*Publisheri* **Microsoft.Azure.Diagnostics** väärtusega atribuut ja atribuudi *Tüüp* väärtus **IaaSDiagnostics** kordumatult Azure'i diagnostika laiendamine.

Atribuudi *nimi* väärtus saab viidata ressursirühma pikendamine. See säte spetsiaalselt **Microsoft.Insights.VMDiagnosticsSettings** võimaldab kerge vaevaga tuvastada on Azure klassikaline portaali portaali tagavad jälgimisega seotud diagrammid Kuva õigesti Azure klassikaline portaalis.

*TypeHandlerVersion* määrab versiooni laiend, mida soovite kasutada. Säte *autoUpgradeMinorVersion* vaheversioon väärtuseks **true** tagab, et teil on mõnevõrra uusima versiooni laiend, mis on saadaval. See on soovitatav alati seatud *autoUpgradeMinorVersion* alati olema **täidetud** , et saate alati kasutada uusima saadaval diagnostika laiend uusi funktsioone ja veaparandused. 

*Sätete* elemendi sisaldab konfiguratsioone atribuutide laiend, mis saab määrata ja lugeda tagasi (mõnikord nimetatakse avaliku konfiguratsiooni) laiendamine. Atribuut *xmlcfg* sisaldab XML-i-põhine konfigureerimine diagnostika logid, jõudluse hinnale diagnostika agent kogunud jne. Vt lisateavet XML-skeemi ise [Diagnostika konfiguratsioon skeemi](https://msdn.microsoft.com/library/azure/dn782207.aspx) . Levinud on tegelik XML-i konfigureerimine kui muutuja Azure'i ressursihaldur malli talletada ja seejärel concatenate ja base64 kodeerida neid seatakse *xmlcfg*väärtus. Vt lisateavet selle kohta, kuidas salvestada XML-i muutujate [diagnostika konfiguratsiooni muutujaid](#diagnostics-configuration-variables) . Atribuut *storageAccount* määrab salvestusruumi konto, millele diagnostika andmed lähevad nimi. 
 
*ProtectedSettings* (mõnikord nimetatakse privaatne konfiguratsiooni) atribuudid saate määrata, kuid ei saa tagasi pärast seatud lugeda. Kirjutage ainult laadi *protectedSettings* muudab abiks talletamise saladusi nagu salvestusruumi konto võti kuhu diagnostika andmed tuleb kirjutada.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Milles diagnostika salvestusruumi konto on parameetrid 

Diagnostika laiend json koodilõigu ülaltoodud eeldab kahe parameetrid *existingdiagnosticsStorageAccountName* ja *existingdiagnosticsStorageResourceGroup* määramiseks diagnostika salvestusruumi konto diagnostika andmete talletamiseks. Täpsustades diagnostika salvestusruumi konto, kui parameeter on lihtne muuta diagnostika salvestusruumi konto üle viibite näiteks võite soovida erinevate diagnostika salvestusruumi konto kasutamiseks testimine ja ühe oma tootmise juurutamiseks.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

See on parim määramiseks diagnostika salvestusruumi konto kui ressursirühm virtuaalse masina jaoks eri ressursirühma. Ressursirühma saab lugeda oma elu jooksul juurutamise üksust, virtuaalse masina saate juurutada ja ümberpaigutatud konfiguratsioone uued värskendused on tehtud selle, kuid soovite jätkata diagnostika andmete säilitamise samasse salvestusruumi konto üle nende virtuaalse masina juurutuste. Salvestusruumi konto on erinevate koodiressursi võimaldab salvestusruumi konto kinnitamiseks andmete erinevad virtuaalse masina juurutuste hõlbus üle erinevate versioonide probleemide tõrkeotsing.

>[AZURE.NOTE] Kui loote windows virtuaalse masina malli Visual Studio võib salvestusruumi vaikekonto seadmine kasutada sama salvestusruumi konto, kus virtuaalse masina VHD on üles laaditud. See on lihtsustada Alghäälestus VM. Uuesti peaks tegur erinevate salvestusruumi konto, mis võib edastada parameetrina kasutada malli. 

## <a name="diagnostics-configuration-variables"></a>Diagnostika konfiguratsiooni muutujad
 
Diagnostika laiend json koodilõigu ülaltoodud määratleb mõne *accountid* muutuja lihtsustamiseks saada diagnostika Storage salvestusruumi konto võti:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Atribuudi *xmlcfg* pikendamise diagnostika on määratletud, kasutades mitut muutujat, mis on liitsõnumeid koos. Selle väärtused on XML-i nii, et nad vajavad tuleb seda vältida õigesti json muutujate määramisel.

Järgnevalt diagnostika konfiguratsiooni xml, mis kogub standard süsteemi jõudluse taseme hinnale koos mõne Windowsi sündmuselogide ja diagnostika taristu logid. See on põgenes ja õigesti vormindatud nii, et konfiguratsiooni saate kleepida otse muutujate jaotise malli. Lugege teemat [Diagnostika konfiguratsioon skeemi](https://msdn.microsoft.com/library/azure/dn782207.aspx) XML-i konfigureerimine rohkem inimeste loetav näiteks.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Mõõdikute määratlus XML-i sõlm ülaltoodud konfiguratsiooni on oluline konfiguratsiooni element, nagu see määratleb, kuidas varem määratletud XML-i *PerformanceCounter* sõlm Jõudluseloendurite kokku liita ja talletatud. 

> [AZURE.IMPORTANT] Need mõõdikud juhtida jälgimisega seotud diagrammid ja teatiste Azure'i portaalis.  **Mõõdikute** sõlm *ResourceIdkasutamisel* ja **MetricAggregation** peab teie VM diagnostika konfiguratsioon lisatud, kui soovite leiate Azure'i portaalis VM andmed. 

Järgmises näites XML mõõdikute määratlusi: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Atribuut *ResourceIdkasutamisel* kordumatult virtuaalse masina teie tellimus. Veenduge, et mall automaatselt värskendatakse neid väärtusi tellimus ja ressursirühm juurutate, et saate kasutada funktsioone subscription() ja resourceGroup().

Kui loote mitme Virtuaalmasinates esitatavaks siis on asustamiseks *ResourceIdkasutamisel* väärtus funktsiooniga copyIndex() eristamiseks õigesti iga üksiku VM. *XmlCfg* väärtus saab värskendada selle toetamiseks järgmiselt:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

*PT1H* ja *PT1M* MetricAggregation väärtus tähistavad liitmise üle minuti ja liitmise üle tunni.

## <a name="wadmetrics-tables-in-storage"></a>Salvestusruumi WADMetrics tabelid

Ülaltoodud mõõdikute konfiguratsiooni loob tabelite diagnostika salvestusruumi konto järgmised nimereeglid:

- **WADMetrics** : standardse eesliitega WADMetrics kõigi tabelite jaoks
- **PT1H** või **PT1M** : tähendab, et tabel sisaldab andmete liitmine ühe tunni jooksul või 15 minutit
- **P10D** : täht tähistab tabeli andmed kuvatakse 10 päeva alates tabeli käivitati andmete kogumine
- **V2S** : stringikonstant
- **AAAAKKPP** : kuupäev, kus tabeli algas andmete kogumine

Näide: *WADMetricsPT1HP10DV2S20151108* sisaldab üle 10 päeva, 11-November-2015 algava tunni kokkuvõtliku mõõdikute andmete    

Iga WADMetrics tabel sisaldab järgmisi veerge:

- **PartitionKey**: selle partitionkey ehitatakse *ResourceIdkasutamisel* väärtuse tuvastamiseks VM ressursi. jaoks näiteks: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : järgib vormingu <Descending time tick>:<Performance Counter Name>. Laskuvas järjestuses aja rist arvutamine on max aeg puugid miinus aega koondamine perioodi alguses. Nt Kui valimi ajavahemikus käivitatud 10 – November-2015 ja 00:00Hrs UTC siis arvutus oleks: DateTime.MaxValue.Ticks - (uus DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Puugid). Jaoks saadaval baiti jõudluse counter rea võti mälu näeb välja nagu: 2519551871999999999__:005CMemory:005CAvailable:0020 baiti
- **CounterName** : on jõudluse näidiku nimi. See vastab *counterSpecifier* määratletud XML-i config.
- **Suurim lubatud** : jõudluse näidiku jooksul koondamine suurima väärtuse.
- **Minimaalne** : jõudluse näidiku koondamine perioodil väikseima väärtuse.
- **Kogusumma** : koondamine perioodil teatatud jõudluse näidiku kõigi väärtuste summa.
- **Count** : jõudluse näidiku teatatud väärtuste arv.
- **Keskmine** : Keskmine (Kokku/arv) väärtus jõudluse näidiku koondamine aja jooksul.


## <a name="next-steps"></a>Järgmised sammud

- Vt Windowsi virtuaalse masina diagnostika laiendiga täieliku valimi malli [201-vm-jälgimine-diagnostika-laiend](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- [Azure'i PowerShelli](virtual-machines-windows-ps-manage.md) või [Azure käsurea](virtual-machines-linux-cli-deploy-templates.md) resource halduri malli juurutamine
- Lisateavet [autorlusega Azure'i ressursihaldur Mallid](../resource-group-authoring-templates.md)







