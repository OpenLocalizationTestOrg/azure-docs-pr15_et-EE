<properties
    pageTitle="Mis juhtus minu ASP.net-i projekti? | Microsoft Azure'i | Visual Studio ühendatud teenused"
    description="Kirjeldab, mis juhtub pärast Azure salvestusruumi lisamine ASP.net-i projekti Visual Studio abil ühendatud teenused"
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

# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Mis juhtus minu ASP.net-i projekti (Visual Studio Azure Storage ühendatud teenuse)?

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

##<a name="connection-string-for-azure-storage-added"></a>Ühendusstringi Azure Storage lisatud
Projekti faili web.config element loodi valitud salvestusruumi konto ühendusstringi ja võti.

Lisateavet leiate teemast [ASP.net-i](http://www.asp.net).
