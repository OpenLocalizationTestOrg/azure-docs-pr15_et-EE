<properties
    pageTitle="Poliitikate abil Azure'i ressursihaldur Virtuaalmasinates | Microsoft Azure'i"
    description="Kuidas rakendada poliitika, mis on Azure ressursihaldur Linux virtuaalse masina"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Poliitikate abil Azure'i ressursihaldur Virtuaalmasinates

Poliitikate abil saate ettevõtte jõustada erinevad ja eeskirjade kogu ettevõtte. Soovitud käitumist täitmine aitab ohtude aidates organisatsiooni edu. Selles artiklis kirjeldatakse, kuidas saate kasutada Azure ressursihaldur poliitikate määratlemiseks Virtuaalmasinates teie asutuse jaoks soovitud käitumist.

Juhised selle saavutamiseks liigendus on all

1. Azure'i ressursihaldur poliitika 101
2. Virtuaalne arvuti poliitika määratlemine
3. Poliitika loomine
4. Poliitika rakendamine

## <a name="azure-resource-manager-policy-101"></a>Azure'i ressursihaldur poliitika 101

Alustamine Azure'i ressursihaldur poliitikad, soovitame artikli alla lugemine ja jätkates selles artiklis toodud juhiseid. Järgmises artiklis kirjeldatakse põhilised määratluse ja struktuuri poliitika, kui poliitika saada hinnatud ja annab mitmesugusest poliitika määratlusi.

* [Poliitika abil saate hallata ressursid ja juurdepääsu reguleerimine](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Virtuaalne arvuti poliitika määratlemine

Ainult nende kasutajatel luua Virtuaalmasinates ühilduma LOB rakenduse testitud operatsioonisüsteemide võib olla üks ettevõte tavastsenaariumid. Azure'i ressursihaldur poliitika abil saab selle tööülesande lõpule paar sammu. Poliitika selles näites me lubada ainult Ubuntu 14.04.2-LTS Virtuaalmasinates luua. Allpool näeb poliitika määratlus

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "Canonical"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "UbuntuServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "14.04.2-LTS"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Ülaltoodud poliitika saab hõlpsalt muuta stsenaariumi, kus võiksite lubada Ubuntu LTS pilt kasutatakse virtuaalse masina juurutus, nii et selle all muutmine

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

#### <a name="virtual-machine-property-fields"></a>Virtuaalse masina atribuudiväljad

Järgmises tabelis kirjeldatakse virtuaalse masina atribuudid, mida saab kasutada väljadena definitsiooni poliitika. Lisateavet rühmapoliitika väljad, lugege artiklit allpool:

* [Väljade ja allikad](../resource-manager-policy.md#fields-and-sources)


| Välja nimi     | Kirjeldus                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Määrab pildi publisher               |
| imageOffer     | Saate määrata valitud pilt Publisheri pakkumine |
| imageSku       | Saate määrata valitud pakkumise jaoks SKU             |
| imageVersion   | Määrab pildi versioon valitud SKU-ga     |

## <a name="create-the-policy"></a>Poliitika loomine

Poliitika saab hõlpsalt luua otse REST API või PowerShelli cmdlet-käskude abil. Poliitika loomiseks leiate artiklist allpool:

* [Poliitika loomine](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Poliitika rakendamine

Poliitika loomise järel peate suhtes määratletud ulatust. Ulatus võib olla tellimuse, ressursirühm või isegi ressurss. Rakendada poliitika, leiate artiklist allpool:

* [Poliitika loomine](../resource-manager-policy.md#applying-a-policy)
