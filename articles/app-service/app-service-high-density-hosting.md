<properties
    pageTitle="Suure tihedusega majutusteenuse teenuses Azure rakendus | Microsoft Azure'i"
    description="Suure tihedusega majutusteenuse teenuses Azure rakendus"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>Suure tihedusega majutusteenuse teenuses Azure rakendus

Kui kasutate rakendust Service, on rakenduse sidumata mõistetest eraldatud võimsus:

- **Rakendus:** Tähistab rakendus ja käitusaja konfiguratsioon. Näiteks see sisaldab versiooni .NET, mis peaks laadimise runtime, rakenduse sätete jne.

- **Rakenduse teenusleping:** Määratleb omaduste võimsus, Saadaval funktsioonikomplekt ja rakenduse asukoht. Näiteks omadused võib olla suur (neli protsessorituuma) arvutisse, nelja, lisafunktsioonidele Ida-USA-s.

Rakendus on alati lingitud mõne rakenduse teenusleping, kuid mõni rakendus teenusleping saate sisestada ühe või mitme võimet.

Selle tulemusena platvorm pakub eristada ühest rakendusest või on mitu rakendused ühiskasutada ressursid on rakenduse teenusleping.

Juhul, kui mitu rakenduste ühiskasutus on rakenduse teenusleping, rakenduse eksemplari töötab iga eksemplari selle rakenduse teenusleping.

## <a name="per-app-scaling"></a>Rakenduse skaleerimist kohta.
*Rakenduse skaleerimist kohta* on funktsioon, mis saab lubatud rakenduse teenindustaseme leping ja seejärel kasutada taotluse kohta.

Rakenduse kohta skaleerimist skaala rakendusele sõltumatult majutava rakenduse teenusleping. Sellisel viisil on rakenduse teenusleping saab konfigureerida 10 eksemplari esitada, kuid rakenduse saate määrata skaala neist ainult 5.

Järgmised Azure'i ressursihaldur mall loob rakenduse teenuse lepingut, mis sobitatakse välja 10 eksemplari ja rakendus, mis on konfigureeritud kasutada rakenduse skaleerimist ja mastaapimiseks ainult 5 eksemplaridesse.

Rakenduse teenusepakett on seada **veebilehte skaleerimist** atribuudi väärtuseks true ( `"perSiteScaling": true`). Rakendus on **töötajate arvu** 5 abil säte (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Soovitatav konfiguratsioon suure tihedusega majutusteenuse

Rakenduse skaleerimist kohta on funktsioon, mis on lubatud nii avaliku Azure piirkondade ja rakenduse teenuse keskkonnas. Soovitatud on siiski kasutada rakenduse teenuse keskkonnas ära oma täiustatud funktsioone ja läbilaskevõime suuremat kaustu.  

Suure tihedusega majutusteenuse rakenduste konfigureerimiseks tehke järgmist

1. Konfigureerida rakendus teenuse keskkonna ja valige töötaja kogumi eesmärk on jätkuvalt suure tihedusega hosting stsenaariumi.

1. Ühe rakenduse teenusleping loomine ja selle kasutamiseks saadaval läbilaskevõime töötaja pool skaala.

1. Klõpsake rakenduse teenusleping seatud väärtusele tõene veebilehte skaleerimise lipp.

1. Uute saitide on loodud ja määratud teenusleping selle rakenduse abil **numberOfWorkers** atribuudi väärtuseks **1**. Selle konfiguratsiooni kasutamine annab kõrgeim tihedusfunktsiooni võimalike selles töötaja kaustas sisse.

1. Töötajaid saab konfigureerida sõltumatult ühe koha andmiseks lisaressursid vastavalt vajadusele. Näiteks võib suure kasutada saidi seatud **numberOfWorkers** **3** on veel töötlemise võimsus selle rakenduse ajal madal kasutamiseks saitide oleks seatud **numberOfWorkers** **1**.
