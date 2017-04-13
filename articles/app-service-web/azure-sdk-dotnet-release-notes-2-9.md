<properties 
   pageTitle="Azure'i SDK .net-i 2.9 väljalaskemärkmed" 
   description="Azure'i SDK .net-i 2.9 väljalaskemärkmed" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-29-release-notes"></a>Azure'i SDK .net-i 2.9 väljalaskemärkmed

##<a name="overview"></a>Ülevaade

See dokument sisaldab selle väljalaskemärkmed Azure'i SDK .NET 2.9 väljaande. 

Üksikasjalikku teavet uuendused selles versioonis, leiate [Azure'i SDK 2.9 teadaanne postitada](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure'i SDK 2.9 Visual Studio 2015 Update 2 ja Visual Studio "15" eelvaade
 
Selle värskenduse sisaldab järgmist veaparandused.

- Probleem on seotud viige API kliendi loomine rakenduses, kus soovite string "Tundmatu tüüp" kuvatakse kood kindralleitnant kausta nime ja/või nimeruumi nimi loobumist sellest genereeritud koodi.
- Ajastatud WebJobs, kus oli autentimise teabe puudumisel ettevalmistamise protsess ajasti edastatavate seotud probleemi.

Selle värskenduse sisaldab järgmised uued funktsioonid:

- Teisene rakenduse teenuste "Teenused" menüüs rakenduse teenuse ettevalmistamise dialoogiboksi tugi. 

##<a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure'i andmed Lake Tools for Visual Studio 2015 värskenduse 2
 
Selle värskendused sisaldab järgmist:

- Visual Studio **Azure'i Andmeriistad Lake** on nüüd ühendatud Azure'i SDK .NET väljaande. Tööriist installitakse automaatselt Azure'i SDK installimisel. 

    Tööriista värskendatakse sageli, valige [siin](http://aka.ms/datalaketool) saada värskendusi.

- **Server Explorer** nüüd võimaldab teil vaadata kõiki ja mõned U-SQL-i metaandmete üksuste loomine. [Lisateabe saamiseks vt.](https://azure.microsoft.com/documentation/services/data-lake-analytics/)


##<a name="hdinsight-tools"></a>Hdinsightiga tööriistad 

Visual Studio **Hdinsightiga tööriistad** toetab nüüd Hdinsightiga versioon 3.3, sh nähtaval Tez diagrammid ja muu keele parandused.


##<a name="azure-resource-manager"></a>Azure'i ressursihaldur 

See väljaanne lisab [KeyVault](../resource-manager-keyvault-parameter.md) toe ARM mallid.

##<a name="see-also"></a>Vt ka

[Azure'i SDK 2.9 teadaanne postituse](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)
