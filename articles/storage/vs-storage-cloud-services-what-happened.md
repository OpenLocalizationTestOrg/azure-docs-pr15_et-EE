<properties
    pageTitle="Mis juhtus minu pilvepõhise teenuse project? | Microsoft Azure'i | Visual Studio ühendatud teenused"
    description="Kirjeldab, mis juhtub pärast Visual Studio abil Azure storage konto ühenduse ühendatud teenuste cloud services projekti"
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

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Mis juhtus minu cloud services project (Visual Studio Azure Storage ühendatud teenuse)?

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

## <a name="connection-string-for-azure-storage-added"></a>Ühendusstringi Azure Storage lisatud
Elementide loodi valitud salvestusruumi konto ühendusstringi ja võti. Muudatused on tehtud failide järgmisega:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
