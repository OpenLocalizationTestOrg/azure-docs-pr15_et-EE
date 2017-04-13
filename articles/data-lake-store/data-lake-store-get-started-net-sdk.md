<properties
   pageTitle="Rakenduste arendamise andmete Lake poe .NET SDK abil | Microsoft Azure'i"
   description="Rakenduste arendamise Azure'i andmed Lake poe .NET SDK abil"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Azure'i Lake andmesalve kasutades .NET SDK kasutamise alustamine

> [AZURE.SELECTOR]
- [Portaal](data-lake-store-get-started-portal.md)
- [PowerShelli](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-GA](data-lake-store-get-started-rest-api.md)
- [Azure'i CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saate teada, kuidas kasutada [Azure'i andmed Lake poe .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) põhiliste toimingute näiteks luua kaustu, üles- ja allalaadimine andmefailid jne. Andmete Lake kohta leiate lisateavet teemast [Azure andmesalve Lake](data-lake-store-overview.md).

## <a name="prerequisites"></a>Eeltingimused

* **Visual Studio 2013 või 2015**. Järgmised juhised kasutada Visual Studio 2015.

* **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

* **Azure'i andmesalve Lake konto**. Juhised, kuidas luua konto, leiate [Azure'i andmesalve Lake kasutamise alustamine](data-lake-store-get-started-portal.md)

* Saate **luua rakenduse Azure Active Directory**. Saate Azure AD rakendus autentida Lake andmesalve rakenduse Azure AD. On erinevate lähenemisviisi autentimiseks Azure AD, mis on **lõppkasutaja autentimist** või **teenusest autentimist**. Juhised ja autentida Lisateavet leiate teemast [andmete Lake poe autentimise abil Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>.Net-i rakenduse loomine

1. Avage Visual Studio ja looge konsooli rakendus.

2. Klõpsake menüü **fail** nuppu **Uus**ja seejärel klõpsake nuppu **projekti**.

3. **Uue projekti**, tippige või valige järgmised väärtused:

  	| Atribuut | Väärtus                       |
  	|----------|-----------------------------|
  	| Kategooria | Mallid/Visual C# / Windows |
  	| Mall | Konsooli rakendus         |
  	| Nimi     | CreateADLApplication        |

4. Klõpsake nuppu **OK** , et luua projekt.

5. Nugeti pakettide lisamine projekti.

    1. Paremklõpsake Solution Exploreris projekti nime ja klõpsake käsku **Halda NuGet-paketid**.
    2. **Nugeti Package Manager** klõpsake menüüs veenduge, et **paketi source** on määratud **nuget.org** ja selle **kaasata väljalaske-eelne** ruut on märgitud.
    3. Otsida ja installida järgmised Nugeti paketid.

        * `Microsoft.Azure.Management.DataLake.Store`– Selles õpetuses kasutab v0.12.5-eelvaade.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`– Selles õpetuses kasutab v0.10.6-eelvaade.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`– Selles õpetuses kasutab v2.2.8-eelvaade.

        ![Nugeti allika lisamine] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Azure'i andmed Lake uue konto loomine")

    4. Sulgege **Nugeti Package Manager**.

6. Avage **Program.cs**, olemasolevat kustutamine ja lisage lisada viited nimeruumid järgmistest.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Muutujate deklareerimine, nagu allpool näidatud ja sisestage väärtused Lake andmesalve nime ja ressursside rühma nime, mis on juba olemas. Samuti veenduge, et kohaliku tee ja faili nimi siin arvutis olemas. Lisage järgmised koodilõigu pärast nimeruumi andmed.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Artikli ülejäänud lõikude näete saadaval .NET meetodite abil saate toiminguid nagu autentimine, faili üleslaadimisel jne.

## <a name="authentication"></a>Autentimine

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Kui kasutate lõppkasutaja autentimine (soovitatav selles õpetuses)

Kasutage seda koos Azure AD "Omakliendi" taotluse; üks pakutud nime allpool. Selleks et saaksite selles õpetuses kiiremini lõpule viia, soovitame selle võimaluse abil.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Paari asja teada seda ülaltoodud koodilõigu.

* Aitavad teil täita õpetuse kiiremini, kasutab see koodilõigu ja Azure AD domeeni ja kliendi ID, mis on vaikimisi kõik Azure tellimuste puhul saadaval. Nii saate **kasutada seda koodilõigu nimega-on rakenduse**.
* Juhul, kui soovite kasutada oma Azure AD domeeni ja rakenduse kliendi ID, peate Azure AD kohalikke rakenduse loomine ja seejärel kasutada Azure AD domeeni, kliendi ID, ja ümber suunata URI jaoks loodud rakenduse. Juhised leiate teemast [loomine Active Directory rakendus](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) .

>[AZURE.NOTE] Ülaltoodud linkide juhised on mõeldud rakenduse Azure AD web. Kuid juhised kehtivad täpselt sama isegi juhul, kui otsustasite hoopis omakliendi rakenduse loomine. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Kui kasutate teenusest autentimine kliendi salajane 

Järgmised koodilõigu saab kasutada autentida rakenduse mitte-interaktiivseks, kasutades klient salajane / võti põhisummalt rakenduse / teenus. Kasutage seda olemasoleva [Azure AD "Web App" rakendus](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Kui kasutate serdiga teenusest autentimine

Kui ka kolmas valik, järgmised koodilõigu saab autentida rakenduse mitte-interaktiivseks, serdiga põhisummalt rakenduse / teenus. Kasutage seda olemasoleva [Azure AD "Web App" rakendus](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Kliendi objektide loomine

Järgmised koodilõigu loob Lake andmesalve konto ja failisüsteemi kliendi objektid, mida kasutatakse probleemi taotlusi teenusega.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Loetle kõik Lake andmesalve kontod tellimuse sees

Järgmised koodilõigu loetleb kõik Lake andmesalve kontod antud Azure tellimuse sees.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Looge kaust

Järgmised koodilõigu kuvatakse soovitud `CreateDirectory` meetod, mille abil saate luua kataloog Lake andmesalve kontol.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Faili üleslaadimine

Järgmised koodilõigu kuvatakse ka `UploadFile` meetod, mille abil saate faile üles laadida Lake andmesalve kontole.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`toetab Rekursiivsed üles ja alla laadida kohaliku faili tee ja faili tee Lake andmesalve vahel.    

## <a name="get-file-or-directory-info"></a>Faili või kausta teabe hankimine

Järgmised koodilõigu kuvatakse soovitud `GetItemInfo` meetod, mille abil saate hankida teavet andmete Lake poes saadaval kataloogi või faili. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Loendi faili või kataloogide

Järgmised koodilõigu kuvatakse soovitud `ListItem` meetod, mida saate kasutada loendis soovitud faili ja kataloogide Lake andmesalve konto.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>CONCATENATE failid

Järgmised koodilõigu kuvatakse soovitud `ConcatenateFiles` meetod, mida kasutaksite failid. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Faili lisamine

Järgmised koodilõigu kuvatakse soovitud `AppendToFile` Lake andmesalve konto juba salvestatud faili andmeid lisa kasutatav meetod.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Faili allalaadimine

Järgmised koodilõigu kuvatakse soovitud `DownloadFile` faili alla laadida Lake andmesalve konto kasutatav meetod.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Järgmised sammud

- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Lake andmesalve .NET SDK viide](https://msdn.microsoft.com/library/mt581387.aspx)
- [Lake andmesalve ülejäänud viide](https://msdn.microsoft.com/library/mt693424.aspx)
