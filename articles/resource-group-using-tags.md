<properties
    pageTitle="Siltide abil saate korraldada oma Azure ressursse | Microsoft Azure'i"
    description="Näitab, kuidas siltide ressursse, arveldus-ja haldamise korraldamiseks."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Siltide abil saate korraldada oma Azure ressursid

Ressursihaldur võimaldab loogiliselt korraldamine ressursse rakendades sildid. Sildid koosnevad /-väärtuse paarideks tuvastavat ressursid koos määratlete atribuudid. Ressursside kategooriasse kuuluvad märkimiseks rakendada sama silti need ressursid.

Ressursside kindla sildiga kuvatava näete oma ressursi rühmad ressursid. Te ei ole piiratud ainult ühte ressursirühma, mis võimaldab teil oma ressursse viisil, mis ei sõltu juurutamise seosed korraldamine ressurssidele. Siltide võib olla kasulik, kui soovite korraldada ressursse, arveldus-või haldamiseks.

Tellimuse kogu taksonoomia lisatakse automaatselt iga sildi lisamist ressursi või ressursirühma. Samuti saate taksonoomia eeltäita ka silt nimed ning väärtused, mida soovite kasutada, ressursid on sildistatud tulevikus oma tellimusele.

Iga ressursi või ressursirühma võib olla kuni 15 silte. Sildi nimi on piiratud 512 märki ja sildi väärtus on piiratud maksimaalselt 256 märki.

> [AZURE.NOTE] Silte saate rakendada ainult ressursse, mis toetavad ressursihaldur toimingud. Kui olete loonud virtuaalse masina, virtuaalse võrgu või salvestusruumi mudeli klassikaline juurutamise kaudu (nagu klassikaline portaali kaudu), ei saa te selle ressursi sildi rakendada. Toetavad sildistamine, Juurutage uuesti järgmiste ressursside kaudu ressursihaldur. Kõik muud ressursid toetavad sildistamine.

## <a name="templates"></a>Mallid

Ressursi sildistamiseks juurutamisel, lihtsalt **siltide** elemendi lisada juurutate ressurss ja sildi nime ja väärtuse. Sildi nimi ja väärtus pole vaja eelnevalt olemas teie tellimus. Saate sisestada kuni 15 siltide iga ressurss.

Järgmises näites on kujutatud salvestusruumi konto sildiga.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Ressursihaldur ei toeta praegu objekti sildi nimede ja väärtusi töötlemise. Selle asemel liigu objekti sildi väärtuste puhul, kuid peate siiski määrama sildi nime, nagu on näidatud järgmises näites.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portaal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>PowerShelli

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure'i CLI

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST API-GA

Portaali ja PowerShelli kasutamine mõlemad [Ressursihaldur REST API](https://msdn.microsoft.com/library/azure/dn848368.aspx) taustal. Kui soovite integreerida sildistamine teise keskkonda, saate sildid võtta saamiseks ressursi ID ja värskendada siltide kogumi paik kõne.


## <a name="tags-and-billing"></a>Sildid ja arveldamine

Toetatud teenuste, saate sildid arvelduse andmete rühmitamiseks. Näiteks [Virtuaalmasinates integreeritud Azure ressursihaldur](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) võimaldavad teil määratlemine ja siltide korraldamiseks arvelduse kasutus virtuaalmasinates jaoks. Kui teil on mitu VMs erinevate ettevõtete jaoks, saate rühma kasutus sildid maksumus Center.  
Saate kasutada ka siltide liigitamiseks kulud käitusaja keskkonnast; näiteks arvelduse kasutus vms tootmiskeskkonnas töötab.

Saate tuua kaudu [Azure'i ressursikasutus ja RateCard API -d](billing-usage-rate-card-overview.md) või kasutus komaga eraldatud väärtuste (CSV) faili siltide teavet. Kasutus faili laadida [Azure kontod portaali](https://account.windowsazure.com/) või [EA portaali](https://ea.azure.com)kaudu. Programmeerimisjuurdepääs arveldusteabe kohta leiate lisateavet teemast [oma Microsoft Azure'i ressursside tarbimine ülevaate saada](billing-usage-rate-card-overview.md). REST API-analüüsitoiminguid, leiate [Azure'i arveldus REST API viide](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Kui laadite siltide arveldamisega toetavad teenuste kasutamine CSV, Sildid kuvatakse veerus **Sildid** . Lisateavet leiate teemast [Microsoft Azure arve mõistmine](billing/billing-understand-your-bill.md).

![Teemast arveldamine Sildid](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Järgmised sammud

- Saate rakendada kohandatud poliitikat tellimus üle piirangud ja põhimõtted. Poliitika määratlete võivad nõuda kõik ressursse teatud sildi väärtus. Lisateabe saamiseks vt [Kasutamine poliitika haldamine ressursid ja reguleerida juurdepääsu](resource-manager-policy.md).
- Sissejuhatus Azure'i PowerShelli kaudu, kui ressursse rakendades teemast [Azure ressursihaldur Azure PowerShelli kasutamine](./powershell-azure-resource-manager.md).
- Sissejuhatus abil Azure'i CLI juurutamisel ressursid teemast [Azure CLI Mac, Linux, ja Windows Azure'i ressursside haldamise abil](./xplat-cli-azure-resource-manager.md).
- Sissejuhatus portaalis leiate [Azure'i portaalis hallata oma Azure ressursid](./azure-portal/resource-group-portal.md)  
