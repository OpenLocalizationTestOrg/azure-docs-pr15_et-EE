<properties
    pageTitle="Mis juhtus minu WebJob project (Visual Studio Azure Storage ühendatud teenuse)? | Microsoft Azure'i"
    description="Kirjeldab, mis juhtus Azure'i WebJob projekti pärast salvestusruumi Visual Studio abil kontoga ühenduse ühendatud teenused"
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

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Mis juhtus minu WebJob project (Visual Studio Azure Storage ühendatud teenuse)?

## <a name="references-added"></a>Lisatud viited

Azure'i salvestusruumi Nugeti pakett on lisatud või mida on värskendatud Visual Studio projekti.  
See pakett lisab järgmised .net-i viited:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Ühendusstringi Azure Storage lisatud
Projekti failis App.config värskendati valitud salvestusruumi konto ühendusstringi ja klahvi **AzureWebJobsStorage** ja **AzureWebJobsDashboard** kirjeid.

Lisateavet leiate [Azure'i WebJobs dokumentatsiooni teemadest](http://go.microsoft.com/fwlink/?linkid=390226).
