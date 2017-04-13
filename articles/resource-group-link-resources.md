<properties 
    pageTitle="Linkimise ressursid Azure'i ressursihaldur | Microsoft Azure'i" 
    description="Linkimine erinevate ressursside rühmade Azure'i ressursihaldur seotud ressursid." 
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
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Linkimise Azure'i ressursihaldur ressursse

Juurutamise, saate märkida ressursi sõltuv teise ressursi, kuid selle elutsükli lõpeb juurutamise. Pärast juurutuse puudub tuvastatud seos sõltuv ressursside vahel. Ressursihaldur sisaldab funktsiooni nimega linkimine püsivate suhete ressursid ressurss.

Ressursi lingite, kus saate dokumendi ressursirühma ulatuvad seosed. Näiteks on levinud olema andmebaasi oma elutsükli elavad ühte ressursirühma ja rakenduse erinevate elutsükli asu erinevate ressursirühma. Rakendus loob andmebaasi nii, et soovite märkida rakendus ja andmebaasi vaheline link. 

Kõigi lingitud ressursid peab kuuluma sama tellimus. Iga ressursi saab linkida 50 muud ressursid. Ainus võimalus päringu seotud ressursid on REST API kaudu. Kui mõni lingitud ressursid on kustutatud või teisaldatud, link omanik puhastamiseks ülejäänud link. Olete **ei** hoiatada, kui kustutamine ressurss, mis on seotud muud ressursid.

## <a name="linking-in-templates"></a>Korduvkasutatava linkimine

Määratleda lingi malli, saate kaasata ressursi tüüp, mis ühendab ressursi pakkuja nimeruum ja **/providers/links**ressursi andmeallika tüüp. Nimi peab sisaldama ressursi allika nimi. Esitate target ressursi ressursi ID-d. Järgmises näites luuakse link veebisaidile ja salvestusruumi konto vahel.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Malli vormingu täieliku kirjelduse leiate teemast [ressursside lingid - malli skeemi](resource-manager-template-links.md).

## <a name="linking-with-rest-api"></a>Linkimise REST API-ga

Määratleda juurutatud ressursid seos, käivitage:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

{Tellimuse id} asendada oma tellimuse ID-d. Asendamine {ressursirühma}, {pakkuja-nimeruumi, {ressursi-tüüpi}, {ressursi nimi} esimene ressurss link tuvastavat väärtused. {Link nimi} asendada luua lingi nime. Kasutage 2015-01-01 api-versiooni.

Taotluse, on järgmised objekti, mis määratleb teise ressursi link.

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Elemendi atribuudid sisaldab teise ressursi identifikaator.

Teie tellimus abil saate teha päringuid lingid:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Rohkem näiteid, sh tuua teabe linkide kohta leiate teemast [Lingitud ressursid](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## <a name="next-steps"></a>Järgmised sammud

- Samuti saate oma ressursse siltidega. Suhtlussiltide ressursside kohta lisateabe saamiseks vaadake teemat [kasutamine siltide korraldada oma ressursse](resource-group-using-tags.md).
- Mallide loomise ja ressursside kasutamist määratlemine kirjelduse leiate teemast [kasutamisel mallide](resource-group-authoring-templates.md).
