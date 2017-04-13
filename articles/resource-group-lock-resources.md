<properties 
    pageTitle="Lukusta ressursse koos ressursihaldur | Microsoft Azure'i" 
    description="Kasutajatel värskendamine ja kustutamine teatud ressursse rakendades piirang kõigi kasutajate ja rollide." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Ressursside Azure'i ressursihaldur lukustamine

Administraatorina, peate tellimuse, ressursirühm või ressursside vältimaks teiste kasutajate kogemata kustutamise või muutmine kriitiliste ressursid ettevõttes lukustada. Lukusta määra **CanNotDelete** või **kirjutuskaitstud**. 

- **CanNotDelete** tähendab, et volitatud kasutajad saavad endiselt lugeda ja ressursi muuta, kuid neid ei saa kustutada. 
- **Kirjutuskaitstud** tähendab, et volitatud kasutajad saavad lugeda ressurss, kuid neid ei saa kustutada või mis tahes toiminguid sooritada. Õiguste ressurss on piiratud **lugeja** roll. 

**Kirjutuskaitstud** rakendamise võib kaasa tuua ootamatuid tagajärgi, kuna mõned toimingud, mis tunduda lugeda toimingute tegelikult nõuda lisatoiminguid. Näiteks pannes **kirjutuskaitstud** Lukusta salvestusruumi kontol takistab kõigi kasutajate loetelu võtmed. Klahvid toiming käsitletakse POST taotlus läbi, kuna tagastatud võtmed on saadaval loendi kirjutada. Näiteks pannes **kirjutuskaitstud** Lukusta mõnda rakendust Service ressursi takistab Visual Studio Server Explorer kuvatakse faili ressursi, kuna selle suhtluse nõuab kirjutamisõigused.

Erinevalt Rollipõhine juurdepääsu reguleerimine, saate rakendada piirangut kõikide kasutajate ja rollide lukkude haldamine. Kasutajate ja rollide õiguste seadmise kohta leiate teemast [Azure Rollipõhine juurdepääsu reguleerimine](./active-directory/role-based-access-control-configure.md).

Kui rakendate lukustus ema ulatuses, pärivad kõik lapse ressursid samale lukustamiseks. Isegi ressursse hiljem lisada lukustus pärivad ema. Kõige piiravad lüüsist päriluse ülimuslik.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kes saab loomine või kustutamine lukud ettevõttes

Saate luua või kustutada halduse lukud, peab olema juurdepääs **Microsoft.Authorization/\* ** või **Microsoft.Authorization/locks/\* ** toimingud. Sisseehitatud rolle, antakse ainult **omanik** ja **Kasutajale Accessi administraator** need toimingud.

## <a name="creating-a-lock-through-the-portal"></a>Portaali kaudu lukustus loomine

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Lukustus malli loomine

Järgmises näites on kujutatud malli, mis loob salvestusruumi kontol. Salvestusruumi konto, millel rakendamiseks lukustus on esitatud parameetrina. Lukusta nimi on loodud, ühendades ressursinimi koos **/Microsoft.Authorization/** ja selle juhul **myLock**Kindlustage nimi.

Ressursi tüüp on esitatud tüüp. Määrake Storage, tippige "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Loomise lukustus REST API-ga

Saate lukustada [REST API halduse lukud](https://msdn.microsoft.com/library/azure/mt204563.aspx)juurutatud ressursse. REST API võimaldab teil luua ja kustutada lukud ja tuua olemasoleva lukud teavet.

Lukustus loomiseks käivitage.

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Ulatus võib olla tellimus, ressursirühm või ressurss. Lukusta – nimi on, mida iganes soovite helistada lukustus. Api-versiooni, kasutage **2015-01-01**.

Taotluse sisaldada JSON-objekti, mis määrab lukus atribuudid.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Näiteid leiate [Halduse lukud REST API -ga](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## <a name="creating-a-lock-with-azure-powershell"></a>Azure'i PowerShelliga lukustus loomine

Juurutatud ressursid Azure PowerShelli abil saate lukustada **New-AzureRmResourceLock** abil, nagu on näidatud järgmises näites.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure'i PowerShelli pakub muude käsku tööta lukud, nt **Set-AzureRmResourceLock** lukustus värskendamiseks ja **Eemalda-AzureRmResourceLock** kustutamiseks on lukustamine.

## <a name="next-steps"></a>Järgmised sammud

- Ressursi lukud töötamise kohta lisateabe saamiseks leiate [Lukusta alla oma Azure'i ressurssidest](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Oma ressursse loogiliselt korraldamise kohta leiate teemast [kasutamine siltide korraldamiseks oma ressursid](resource-group-using-tags.md)
- Mis asub ressursi ressursirühm muutmiseks lugege teemat [uue ressursirühma ressursse liikumine](resource-group-move-resources.md)
- Saate rakendada kohandatud poliitikat tellimus üle piirangud ja põhimõtted. Lisateabe saamiseks vt [Kasutamine poliitika haldamine ressursid ja reguleerida juurdepääsu](resource-manager-policy.md).
