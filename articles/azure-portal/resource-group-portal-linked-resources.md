<properties 
    pageTitle="Lingitud ja seotud ressursid galeriis paan" 
    description="Lugege lisateavet seotud ja lingitud ressursside, mis kuvatakse paan Galerii Azure eelvaade portaali." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Lingitud ja seotud ressursid galeriis paan

Paani Galerii võimaldab teil otsida paanid konkreetse ressursi ja lohistage need oma praeguse tera. Paani galerii abil saate luua halduse vaadete ulatuvad ressursid. Mis tahes määratud ressursi sisaldama seotud ressursid kõik jagavad sama ressursirühma nimega ressurss ressursid ja mis tahes ressursid, millest link või sealt ressurss.

## <a name="linked-resources-in-azure-resource-manager"></a>Lingitud ressursid Azure'i ressursihaldur

Linkimine on funktsioon Azure'i ressursihaldur.  See võimaldab teil deklareerida seoseid ressursid isegi juhul, kui nad ei asu ühte ressursirühma. Linkimise ei mõjuta käitusaja oma ressursse, ei mõjuta arveldamine ja Rollipõhine juurdepääsu ei mõjuta.  See on lihtsalt abil saate tähistada seosed, et tööriistad nagu paani Galerii pakub rikkaliku halduse kogemusi süsteem.  Oma tööriistad saate kontrollida lingid linkide API kaudu ja sisestage kohandatud haldus kogemusi ka. 

## <a name="how-do-i-link-my-resources"></a>Kuidas siduda oma ressursse?

Ressursid portaali kaudu või kasutades Azure PowerShelli või Azure CLI malli loomisel lingid luuakse automaatselt mõne sõltuvad ressursid. Ressursid saate linkida ka programmiliselt [Lingitud ressursid REST API](https://msdn.microsoft.com/library/azure/mt238499.aspx) abil või deklareerimise malli seoseid. Täieliku arutelu töötamise lingitud ressursid, vt [Linking ressursid Azure'i ressursihaldur](../resource-group-link-resources.md).

## <a name="next-steps"></a>Järgmised sammud

- Kui teil on vaja Sissejuhatus kirjutamine Azure'i ressursihaldur Mallid, vaadake [funktsiooniga Mallid](../resource-group-authoring-templates.md).
- Alustada üheks lähemalt ressursid vaheliste linkide loomise kohta vt [Linking ressursid Azure'i ressursihaldur](../resource-group-link-resources.md).
- Veel eelvaade portaali kaudu ressursside levirühmadega töötamise kohta vt [Azure'i eelvaade portaalis hallata oma Azure ressursse](resource-group-portal.md).
