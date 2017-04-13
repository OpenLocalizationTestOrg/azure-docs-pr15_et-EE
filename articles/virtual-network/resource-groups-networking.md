<properties
   pageTitle="Võrgu ressursside pakkuja ülevaade | Microsoft Azure'i"
   description="Lisateavet uue ressursi pakkuja Azure'i ressursihaldur"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Ressursi pakkuja
Tänase business edu, vaja toetamine on võimalus luua ja hallata dünaamilised, paindlik, turvaline ja korratavad suuremahuliste võrgu rakendusi. Azure'i ressursi Manager (ARM) võimaldab teil luua selliseid rakendusi ressursirühma ressursside ühe kogumina. Selliste ressursside hallatakse erinevate ressursside pakkujate osa kaudu.

Azure'i ressursihaldur tugineb erinevate ressursside pakkujate juurdepääsu oma ressursse. On kolm peamist ressursi pakkujad: võrk, salvestusruumi ja Arvuta. Selle dokumendi käsitletakse omadusi ja ressursside teenusepakkuja, sh eelised:

- **Metaandmete** – saate lisada teavet ressursside siltide kasutamine. Jälgida ressursi kasutamine ressursi rühmad ja tellimuste saab kasutada järgmisi silte.
- **Suuremat kontrolli võrgu** - ressursid on lõdvalt ühendatud võrku ja te saate määrata need täpsema viisil. See tähendab, et teil on rohkem paindlikkust võrgu ressursside haldamine.
- **Kiirem konfiguratsiooni** - kuna võrgu ressursse lõdvalt on ühendatud, saate luua ja korraldab paralleelselt võrgu ressursse. See on oluliselt vähendada konfigureerimise aeg.
- **Rollipõhine juurdepääsu juhtimine** - RBAC pakub vaikerollid koos turvalisuse ulatus, lisaks, mis võimaldab luua kohandatud rollid turvaline haldus.
- **Lihtsam haldamine ja juurutamine** - oleks lihtsam juurutada ja hallata rakendusi, kuna võite luua ka kogu taotluse virnas ressursirühma ressursside ühe kogumina. Ja juurutada, kuna saate juurutada lihtsalt esitada malli JSON last kiirem.
- **Kiire kohandamine** - deklaratiivseid laadi mallide abil saate lubada juurutuste korratavad ja kiire kohandamine.
- **Korduvate kohandamine** - deklaratiivseid laadi mallide abil saate lubada juurutuste korratavad ja kiire kohandamine.
- **Halduse liidesed** – saate kasutada mis tahes järgmised liidesed hallata oma ressursse:
    - Vastavalt REST API
    - PowerShelli
    - .NET SDK
    - Node.JS SDK
    - Java SDK
    - Azure'i CLI
    - Eelvaate portaal
    - ARM malli keel

## <a name="network-resources"></a>Võrgu ressursid
Võrgu ressursse saate hallata nüüd sõltumatult, neid hallatakse ühe Arvuta ressursi (virtuaalse masina) asemel. See tagab suurema paindlikkuse ja agility sisse koostamise keerukate ja suures ulatuses taristu ressursside rühma.

Kontseptuaalne vaade valimi juurutuse seotud mitmerajalisest rakendus on esitatud allpool. Iga ressursi näete, nt NICs, avaliku IP-aadressid ja VMs saab hallata ükshaaval.

![Võrgu ressursi mudel](./media/resource-groups-networking/Figure2.png)

Iga ressursi sisaldab ühist atribuutide komplekti, ja nende üksikute atribuudi. Ühised atribuudid on:

|Atribuut|Kirjeldus|Näidisväärtuste|
|---|---|---|
|**Nimi**|Ressursi kordumatu nimi. Iga ressursi tüüp on oma nime piirangud.|PIP01, VM01, NIC01|
|**asukoht**|Azure'i piirkond, kus asub ressurss|westus eastus|
|**ID**|Kordumatu vastavalt URI|/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

Saate vaadata omaduste ressursid allpool.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Halduse liidesed
Saate hallata oma Azure võrgu ressursse erinevaid kasutajaliideseid abil. Selles dokumendis me keskenduda kahe nende liideste: REST API-ga ja malle.

### <a name="rest-api"></a>REST API-GA
Nagu varem mainitud, saab hallata võrgu ressursse kaudu erinevaid liideste, sh REST API-ga, .NET SDK, Node.JS SDK, Java SDK, PowerShelli, CLI, Azure portaali ja mallid.

Rest API vastama HTTP 1.1 protokolli määratlus. Allpool on esitatud üldist URI struktuuri API:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

Ja parameetrid looksulgudega tähistada järgmisi elemente:

- **tellimuse id** - oma Azure tellimuse ID-d.
- **ressursi-pakkuja-nimeruum** - nimeruumi jaoks kasutatava pakkuja. Ressursi pakkuja väärtus *Microsoft.Network*.
- **regiooni nimi** - Azure regiooni nimi

HTTP järgmistest on toetatud, kui helistamist REST API:

- **Sellele** - ressursi antud tüüpi loomiseks kasutatud ressursi atribuudi muutmiseks või muuta ressursside vahel.
- **Saada** - ettevalmistatud ressursi teabe toomiseks kasutada.
- **Kustutamine** – kustutada mõne olemasoleva ressursi.

Nii koosolekukutsete ja kutsele vastamise vastama JSON last vorming. Leiate üksikasjalikumat teavet teemast [Azure ressursside halduse API -d](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="arm-template-language"></a>ARM malli keel
Lisaks haldamise ressursid kindlasti (kaudu API-d või SDK), saate kasutada ka deklaratiivseid programmeerimise laadi luua ja hallata võrgu ressursse keele ARM malli abil.

Valimi kujutis malli on esitatud allpool –

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Mall on peamiselt JSON ressursside ja kirjeldust eksemplari süstida kaudu parameetrite väärtused. Järgmises näites saab luua virtuaalse võrgu 2 alamvõrku.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Teil on võimalus näha parameetrite väärtused käsitsi, kui kasutate malli või kasutage parameetrit faili. Järgmises näites on kuvatud võimalik kogum koos malli üle parameetrite väärtused:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Mallide kasutamine peamisi eeliseid on:

- Saate koostada keerukate taristu deklaratiivseid stiil ressursside rühma. Luua ressursid, sealhulgas sõltuvus haldamise korraldamise teostab ARM.
- Infrastruktuuri saab luua korratavad viisil mööda eri piirkondade ja piirkonna lihtsalt muuta parameetrid.
- Laadi deklaratiivseid viib lühem täitmisaeg, malle ja jooksvalt läbi infrastruktuuri.

Näidismallid, leiate [Azure'i Kiirjuhend Mallid](https://github.com/Azure/azure-quickstart-templates).

ARM malli keele kohta leiate lisateavet teemast [Azure ressursihaldur malli keel](../resource-group-authoring-templates.md).

Ülaltoodud näidismall kasutab virtuaalse võrgu ja alamvõrgu ressursid. Muud võrgu ressursid saate kasutada nagu allpool on:

### <a name="using-a-template"></a>Malli abil

Saate juurutada teenused Azure malli põhjal, kasutades PowerShelli AzureCLI, või nuppu, et juurutada GitHub täites. Mallist GitHub teenuste juurutamine, täita järgmised toimingud:

1. Avage fail template3 GitHub. Näiteks, avage [Virtual võrgu ja kahe alamvõrku](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Klõpsake **Deploy Azure**ja seejärel logige sisse Azure portaali mandaadiga.
3. Veenduge, et mall ja klõpsake siis nuppu **Salvesta**.
4. Klõpsake nuppu **Redigeeri parameetrid** ja valige asukoht, nt *Lääne USA*, vnet ja alamvõrku.
5. Vajaduse korral **ADDRESSPREFIX** ja **SUBNETPREFIX** parameetrite muutmine ja seejärel klõpsake nuppu **OK**.
6. Klõpsake nuppu **Vali ressursirühma** ja seejärel klõpsake lisatava vnet ja alamvõrku ressursirühma. Teise võimalusena saate luua uue ressursirühma, klõpsates **või looge uus**.
3. Klõpsake nuppu **Loo**. Teate kuvamise **ettevalmistamise malli juurutamise**paani. Kui juurutamise on lõpule jõudnud, kuvatakse kuva, mis on sarnane ühte allpool.

![Valimi malli juurutamine](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Järgmised sammud

[Azure'i ressursihaldur malli keel](../resource-group-authoring-templates.md)

[Azure'i võrgunduse – levinud Mallid](https://github.com/Azure/azure-quickstart-templates)

[Arvutage ressursi pakkuja](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md)
