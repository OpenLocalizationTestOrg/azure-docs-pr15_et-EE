<properties
    pageTitle="Mis juhtus minu ASP.net-i 5 projekt (Visual Studio ühendatud teenused) | Microsoft Azure'i salvestusruum"
    description="Kirjeldab, mis juhtub pärast Visual Studio ASP.net-i 5 projekti, kasutades Visual Studio Azure storage konto ühenduse ühendatud teenused"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Mis juhtus minu ASP.net-i 5 projekt (Visual Studio Azure Storage ühendatud teenused)?

## <a name="references-added"></a>Lisatud viited

Azure'i salvestusruumi Nugeti pakett on lisatud Visual Studio projekti.  
See pakett lisab järgmised .net-i viited:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

Samuti on lisatud Nugeti pakett **Microsoft.Framework.Configuration.Json** .

## <a name="connection-string-for-azure-storage-added"></a>Ühendusstringi Azure Storage lisatud
Projekti failis config.json element loodi valitud salvestusruumi konto ühendusstringi ja võti.

Lisateavet leiate teemast [ASP.net-i 5](http://www.asp.net/vnext).
