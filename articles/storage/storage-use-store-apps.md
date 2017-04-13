<properties
    pageTitle="Windowsi poe rakendustes kasutada Azure storage | Microsoft Azure'i"
    description="Saate teada, kuidas luua Windowsi poe rakendus, mis kasutab Azure'i bloobimälu, järjekorda, tabelis või faili salvestusruumi."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Kuidas kasutada Azure Storage Windowsi poe rakenduses

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas alustada arendamise Windowsi poe rakendus, mis kasutab Azure Storage.

## <a name="download-required-tools"></a>Laadige alla nõutavad tööriistad

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) lihtne koostada, silumine, localize, paketti, ja juurutamine Windowsi poe rakendused. Visual Studio 2012 või uuem versioon on nõutav.
- [Azure'i salvestusruumi kliendi teek](https://www.nuget.org/packages/WindowsAzure.Storage) pakub Windows käitusaja klassi teegi Azure Storage töötamiseks.
- [WCF-i andmete teenuste tööriistade jaoks Windowsi poe rakenduste](http://www.microsoft.com/download/details.aspx?id=30714) laiendab kliendipoolne OData tugi Windowsi poe rakenduste Visual Studio lisada teenuse viide kogemus.

## <a name="develop-apps"></a>Rakenduste arendamise

### <a name="getting-ready"></a>Kuidas valmis

Looge uus Windowsi poe rakenduse project Visual Studio 2012 või uuem versioon.

![poest rakenduste-salvestusruumi-vs-projekti][store-apps-storage-vs-project]

Järgmisena lisage viide Azure'i salvestusruumi kliendi teek, paremklõpsates **Viited**, klõpsates nuppu **Lisa viide**ja seejärel sirvides salvestusruumi kliendi teegi jaoks Windows käitusaja allalaaditud:

![poe-rakenduste-salvestusruumi – valige-teeki][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Teegi kasutamine teenustega bloobimälu ja järjekord

Selles etapis rakenduse on valmis kõne Azure'i bloobimälu ja järjekorda lisada teenuseid. Azure Storage tüüpi saab viidata otse **abil** järgmistest lisamine

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Nupu lisamine edasi, oma konto lehele. Järgmine kood lisamiseks **klõpsake** sündmuse ja muuta oma sündmuse sündmuseohjuri meetod [asünkroonse märksõna](http://msdn.microsoft.com/library/vstudio/hh156513.aspx)abil.

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Järgmine kood eeldab, et teil on kaks stringi muutujate, *account_name* ja *accountKey*. Moodustavad need teie salvestusruumi konto ja konto võti, mis on seotud konto nimi.

Koostamine ja käivitage rakendus. Klõpsake nuppu kas nimega *container1* ümbris olemas teie konto ja seejärel luua, kui ei.

### <a name="using-the-library-with-the-table-service"></a>Teegi tabeli teenuse abil

Tüübid, mida kasutatakse suhelda Azure'i tabeli teenuse sõltuvad WCF Data Services Windowsi poe rakenduse teegi. Järgmisena lisage nõutavad WCF-i teekide viide Package Manager konsooli abil:

![poe-rakenduste-salvestushalduri-paketi][store-apps-storage-package-manager]

Abil asukoht teie arvutis punkti Package Manager järgmine käsk:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

See käsk lisab automaatselt kõik nõutavad viited projekti. Kui te ei soovi kasutada Package Manager konsooli, saate lisada WCF Data Services Nugeti kausta teie kohalikus arvutis paketi andmeallikate loendi ja seejärel lisage UI, kuni viide, nagu on kirjeldatud [Haldamise Nugeti pakettide abil dialoogiboksi](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Kui olete viidatud WCF Data Services Nugeti pakett, muuta oma nuppu **klõpsake** sündmuse koodi:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Järgmine kood kontrollib, kas teie konto on olemas ja seejärel loob, kui ei tabeli nimega *Tabel1* .

Võite sisestada ka viide Microsoft.WindowsAzure.Storage.Table.dll, mis on saadaval samas pakendis alla laaditud. See teek sisaldab lisafunktsioone, nt peegeldus-põhiste sariväljaanne ja üldise päringud. Pange tähele, et see teek ei toeta JavaScripti.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
