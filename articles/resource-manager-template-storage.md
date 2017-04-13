<properties
   pageTitle="Ressursihaldur malli Storage | Microsoft Azure'i"
   description="Näitab ressursihaldur skeemi salvestusruumi kontode malli kasutamise kohta."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Salvestusruumi konto malli skeemifailid

Loob salvestusruumi konto.

## <a name="schema-format"></a>Skeemi vorming

Salvestusruumi konto loomine malli jaotisse ressursid lisada järgmine skeem.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Väärtused

Järgmises tabelis on kirjeldatud peate määrama skeemi väärtused.

| Nimi | Väärtus |
| ---- | ---- |
| tüüp | Loetelu<br />Nõutav<br />**Microsoft.Storage/storageAccounts**<br /><br />Ressursi tüüp loomiseks. |
| apiVersion | Loetelu<br />Nõutav<br />**2015-06-15** või **2015-05-01-eelvaade**<br /><br />Ressursi loomiseks kasutatav API versioon. | 
| Nimi | String<br />Nõutav<br />3 – 24 märki, ainult arve ja väiketähed tähed.<br /><br />Luua salvestusruumi konto nimi. Nimi peab olema kordumatu kõigis Azure. Võtke arvesse, et teie nimetamistava [uniqueString](resource-group-template-functions.md#uniquestring) funktsiooni abil, nagu on näidatud järgmises näites. |
| asukoht | String<br />Nõutav<br />Piirkond, mis toetab salvestusruumi kontod. Lubatud piirkondade määratlemiseks vaadata [toetatud regioonide](resource-manager-supported-services.md#supported-regions).<br /><br />Piirkonna majutada salvestusruumi konto. |
| atribuudid | Objekti<br />Nõutav<br />[objekti atribuudid](#properties)<br /><br />Objekti, mis määrab salvestusruumi konto loomiseks. |

<a id="properties" />
### <a name="properties-object"></a>objekti atribuudid

| Nimi | Väärtus |
| ---- | ---- | 
| accountType | String<br />Nõutav<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**või **Premium_LRS**<br /><br />Salvestusruumi konto tüüp. Lubatud väärtused vastavad Standard kohalikult liigsed, Standard Zone liigsed, Standard Geo-tarbetud, Standard-lugemisõigus Geo-tarbetud ja Premium kohalikult liigsed. Nende kontode kohta leiate lisateavet teemast [Azure Storage kopeerimine](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Näited

Järgmine näide kasutab Standard kohalikult liigsete salvestusruumi konto koos põhjal ressursi rühma id kordumatu nimi.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Kiirjuhend Mallid

On palju Kiirjuhend mudelit, mis sisaldavad salvestusruumi konto. Järgmised Mallid kirjeldavad mõne levinud stsenaariumi.

- [Kindlad konto loomine](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Mõne Windowsi VM lihtsa kasutuselevõtt](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Lihtne on Linux VM kasutuselevõtt](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [CDN profiili, kui origin kontoga salvestusruumi CDN lõpp-punkti loomine](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Luua kõrge Availabilty SharePointi serveripargi 9 VMs abil PowerShelli DSC laiend](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [5 lihtsat juurutamise sõlm secure teenuse struktuuri kobar koos WAD lubatud](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Luua virtuaalse masina Windows pildilt 4 tühja andmete ketast](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Järgmised sammud

- Üldist teavet salvestusruumi kohta leiate teemast [Sissejuhatus Microsoft Azure'i Tabelimäluga](./storage/storage-introduction.md).
- Näiteks malle, mida uue salvestusruumi konto abil virtuaalse masina, lugege [Deploy lihtne Linux VM](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) või [Deploy lihtsa Windows VM](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
