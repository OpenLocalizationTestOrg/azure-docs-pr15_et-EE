<properties
    pageTitle="Ühenduse loomine Xamarin.Forms rakenduse Azure'i Tabelimäluga"
    description="Piltide lisamine todo loendi Xamarin.Forms mobiilirakenduses ühenduse Azure'i bloobimälu"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Ühenduse loomine Xamarin.Forms rakenduse Azure'i Tabelimäluga

## <a name="overview"></a>Ülevaade

Azure'i mobiilirakenduste klient ja server SDK toetab ühenduseta sünkroonimine Liigendatud andmete CRUD toimingutega /tables lõpp-punkti suhtes. Üldiselt andmed on salvestatud andmebaasi või sarnane poe ja üldiselt need andmed poed ei saa salvestada suure binaarandmeid tõhus. Samuti on mõned rakendused seotud andmeid, mis on salvestatud mujal (nt bloobimälu, failide aktsiad) ja kasulik on võimalik luua seoseid /tables lõpp-punkti kirjete ja muude andmete.

Selles teemas näidatakse, kuidas lisada pilte tugi mobiilirakenduste todo loendi Kiirjuhend. Esmalt peate teostama õpetuse [Xamarin.Forms rakenduse loomine].

Selles õpetuses salvestusruumi konto loomine ja lisamine oma mobiilirakenduse kirjutamata ühendusstringi. Seejärel lisada uue pärimist mobiilirakenduste uut tüüpi `StorageController<T>` serveri projekti.

>[AZURE.TIP] Selles õpetuses on [companion valimi](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) saadaval, mida saab kasutada Azure'i kontosse. 

## <a name="prerequisites"></a>Eeltingimused

* Täitke õpetuse [Xamarin.Forms rakenduse loomine] , kus on loetletud muud eeltingimused. Selles artiklis kasutab lõplikus rakendus selle õpetuse kaudu.

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](https://tryappservice.azure.com/?appServiceName=mobile). Olemas, saate kohe luua lühiajaline starter mobiilirakenduse rakendust Service – pole vaja krediitkaarti ja kohustusi.

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

1. [Looge konto Azure salvestusruumi]õpetuse järgides salvestusruumi konto loomine. 

2. Azure'i portaalis, avage äsja loodud salvestusruumi konto ja klõpsake ikooni **võtmed** . **Esmane ühendusstringi**kopeerimine

3. Liikuge oma mobiilirakenduse taustväärtus. Klõpsake jaotises **Kõik sätted** -> **Rakenduse sätted** -> **Ühendusstringi**, Loo uus võti nimega `MS_AzureStorageAccountConnectionString` kopeeritud konto salvestusruumi väärtusega. Kasutage **kohandatud** võtme tüüp.

## <a name="add-a-storage-controller-to-the-server"></a>Lisada salvestusruumi kontrolleril server

Peate lisama uue kontrolleril oma serveri projekti, mis kuvatakse vastata märgiks SAS Azure Storage ning naaske vastavate failide loendi kirje:

- [Salvestusruumi kontrolleril lisamine projekti server](#add-controller-code)
- [Marsruudib registreeritud salvestusruumi domeenikontrolleri](#routes-registered)
- [Kliendi- ja suhtlus](#client-communication)

###<a name="add-controller-code"></a>Salvestusruumi kontrolleril lisamine projekti server

1. Avage Visual Studio .net-i serveri projekti. Lisage Nugeti pakett [Microsoft.Azure.Mobile.Server.Files]. Valige **kaasa väljalaske-eelne**kindlasti.

2. Avage Visual Studio .net-i serveri projekti. Paremklõpsake **kontrollerid** kausta ja valige käsk **Lisa** -> **kontrolleril** -> **Web API 2 kontrolleril - tühi**. Selle domeenikontrolleri nimi `TodoItemStorageController`.

3. Lisage järgmine laused abil:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Muuta base klassi `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Saate lisada klassi järgmistest viisidest:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Värskendage Web API konfiguratsiooni häälestamiseks atribuut marsruutimist. **Startup.MobileApp.cs**, lisage järgmine rida on `ConfigureMobileApp()` meetod pärast määratlus on `config` muutuja:

        config.MapHttpAttributeRoutes();

7. Serveri projekti avaldada oma mobiilirakenduse taustväärtus.

###<a name="routes-registered"></a>Marsruudib registreeritud salvestusruumi domeenikontrolleri

Uue `TodoItemStorageController` seab kaks sub ressursside hallatavate kirje:

- StorageToken

    + HTTP POST: Loob märgiks salvestusruum
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Toob failide loendi seotud kirje
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + HTTP Kustuta: Kustutab faili ressursiidentifikaator määratud fail
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Kliendi- ja suhtlus

Pange tähele, et `TodoItemStorageController` ei *ole* on marsruudi jaoks üles või alla soovitud bloobimälu. On põhjus mobiiliklienti suhtleb bloobimälu salvestusruumi *otse* täitmiseks need toimingud pärast esimese saada märgiks SAS (ühiskasutusse Accessi allkiri) saate turvaliseks juurdepääsuks kindla bloobimälu või ümbrises. See on oluline arhitektuuri, muidu juurdepääsu salvestusruumi oleks piiratud skaleeritavus ning kättesaadavust Mobile'i taustväärtus. Selle asemel ühendatud otse Azure Storage mobile kliendi saate ära selle funktsioone nagu automaatne eraldamine ja geo-jaotuse.

Ühiskasutusega juurdepääsu signatuuri pakub delegeeritud juurdepääsu teie salvestusruumi konto ressursid. See tähendab, et saate anda mõnda muud klienti piiratud õiguste objektide konto salvestusruumi määratud perioodi ja määratud kogumi õigused ilma jagada oma konto kiirklahvide. Lisateabe saamiseks lugege teemat [Mõistmine ühiskasutusse Accessi allkirjad].

Alloleval joonisel kuvatakse kliendi ja serveri sinna. Enne faili üleslaadimisel taotleb kliendi teenuse SAS luba. Teenuse kasutab salvestusruumi ühendusstring luua uue SAS, mis tagastab kliendile. MUUDE on kiire ja piirab ainult teatud faili või container õigused. Mobile kliendi seejärel kasutab seda SAS ja Azure Storage kliendi SDK bloobimälu laadige fail üles.

![Paluda SAS luba](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Värskendage oma kliendi rakendus lisada pildi tugi

Avage Visual Studio või Xamarin Studio Xamarin.Forms Kiirjuhend projekt. Te installida Nugeti pakettide ja värskendada Kaasaskantav Raamatukogu projekti ja iOS-i, Androidi ja Windowsi Klient projektid:

- [Lisage Nugeti paketid](#add-nuget)
- [IPlatform kasutajaliidese lisamine](#add-iplatform)
- [Lisage FileHelper klassi](#add-filehelper)
- [Faili sünkroonimine sündmuseohjuri lisamine](#file-sync-handler)
- [TodoItemManager värskendamine](#update-todoitemmanager)
- [Lisage üksikasjade kuvamine](#add-details-view)
- [Põhikuval värskendamine](#update-main-view)
- [Värskendage Androidi projekt](#update-android), [projekti iOS-i](#update-ios), [Windowsi projekti](#update-windows)

>[AZURE.NOTE] Õppeteema sisaldab ainult Androidi, iOS-i ja Windowsi poe platvormid, mitte Windows Phone juhised.

###<a name="add-nuget"></a>Lisage Nugeti paketid

Paremklõpsake lahendus ja valige **Halda Nugeti paketid lahenduse**. Lisage järgmised Nugeti paketid **kõiki** projekte lahendus. Märkige ruut **kaasa väljalaske-eelne**kindlasti.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

See näide kasutab [PCLStorage] teegi mugavuse huvides, kuid ei ole vaja Azure mobiilirakenduste kliendi SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>IPlatform kasutajaliidese lisamine

Saate luua uue kasutajaliidese `IPlatform` peamised Kaasaskantav Raamatukogu projekti. See järgib [Xamarin.Forms DependencyService] mustri paremale platvormi kohased klassi käitusajal laadimiseks. Hiljem lisate platvormi kohased rakendusi iga kliendi projektid.

1. Lisage järgmine laused abil:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. Asendage rakendamist järgmist:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>Lisage FileHelper klassi

1. Saate luua uue klassi `FileHelper` peamised Kaasaskantav Raamatukogu projekti. Lisage järgmine laused abil:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Klassi määratluse lisamiseks tehke järgmist.

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Faili sünkroonimine sündmuseohjuri lisamine

Saate luua uue klassi `TodoItemFileSyncHandler` peamised Kaasaskantav Raamatukogu projekti. See tund sisaldab kontekstiatribuuti Azure'i SDK kood teavitada, kui faili on lisada või eemaldada.

Azure'i Mobile kliendi SDK ega talleta mis tahes faili andmed: kliendi SDK tugineb oma rakendamine `IFileSyncHandler` mis omakorda määratleb, kas ja kuidas failid on talletatud kohalik seadmes.

1. Lisage järgmine laused abil:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Asendage klassi määratluse järgmist: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>TodoItemManager värskendamine

1. Kommenteerige **TodoItemManager.cs**, klõpsake välja joone `#define OFFLINE_SYNC_ENABLED`.

2. **TodoItemManager.cs**, lisage järgmine laused abil:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. Ehitaja, `TodoItemManager`, lisage järgmine pärast kõne `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. Asendage ehitaja, kõne edastamiseks `InitializeAsync` järgmisega. See tagab, et on kontekstiatribuuti kirjete muutmisel kohalikku salve. Faili sünkroonimisfunktsiooni kasutab neid kontekstiatribuuti oma faili sünkroonimine sündmuseohjuri käivitamiseks.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. Klõpsake `SyncAsync()`, lisage järgmine pärast kõne `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Järgmistest meetoditest, et lisada `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Lisage üksikasjade kuvamine

Selles jaotises lisamist todo üksuse üksikasjad uue vaate. Vaade on loodud, kui kasutaja valib todo üksus ja see võimaldab uue üksuse lisada pilte.

1. Kaasaskantav Raamatukogu projekti alustamine pärast rakendamist uutel **TodoItemImage** lisamiseks tehke järgmist.

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. Redigeerige **App.cs**. Asendage lähtestamine `MainPage` koos järgmist:
    
        MainPage = new NavigationPage(new TodoList());

3. **App.cs**, lisage järgmised atribuut:

        public static object UIContext { get; set; }

4. Paremklõpsake Kaasaskantav Raamatukogu projekti ja valige käsk **Lisa** -> **Uue üksuse** -> **platvormidel** -> **Vormide XAML-i lehe**. Vaate nimi `TodoItemDetailsView`.

5. Avage **TodoItemDetailsView.xaml** ja asendada selle ContentPage keha järgmist:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. **TodoItemDetailsView.xaml.cs** redigeerida ja lisada järgmised laused abil:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Asendage rakendamise `TodoItemDetailsView` koos järgmist:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>Põhikuval värskendamine 

Värskendage põhikuval avamiseks Kuva üksikasjad todo üksuse valimisel.

**TodoList.xaml.cs**, asendage rakendamise `OnSelected` koos järgmist:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Androidi projekti värskendamine

Androidi projekti, sealhulgas kood faile alla laadida ja kaamera abil saate jäädvustada uue pildi platvormi kohased koodi lisamine. 

Järgmine kood kasutab Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) paremale platvormi kohased klassi käitusajal laadimiseks.

1. Lisage komponent **Xamarin.Mobile** Androidi projekt.

2. Uue klassi `DroidPlatform` järgmised rakendamist. Asendage "YourNamespace" peamisi nimeruumi projekti.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Redigeerige **MainActivity.cs**. Klõpsake `OnCreate`, lisage järgmine enne kõne `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>Projekti iOS-i värskendamine

IOS-i projekti platvormi kohased koodi lisamine.

1. IOS-i projekti komponent **Xamarin.Mobile** lisada.

2. Uue klassi `TouchPlatform` järgmised rakendamist. Asendage "YourNamespace" peamisi nimeruumi projekti.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. **AppDelegate.cs** redigeerimine ja eemaldamine kõne `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Projekti Windowsi värskendamine

1. Installige Visual Studio laiend [SQLite Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Lisateabe saamiseks vt [rakenduse Windowsi ühenduseta sünkroonimise lubamine](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)õpetuse. 

2. Redigeerige **Package.appxmanifest** ja **veebikaamera** videovõimaluste.

3. Uue klassi `WindowsStorePlatform` järgmised rakendamist. Asendage "YourNamespace" peamisi nimeruumi projekti.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Kokkuvõte

Selles artiklis kirjeldatud, kuidas kasutada uut faili tugi Azure Mobile kliendi ja serveri SDK Azure Storage töötamiseks. 

- Salvestusruumi konto loomine ja lisamine oma mobiilirakenduse kirjutamata ühendusstring. Ainult selle kirjutamata on Azure Storage võti: mobile kliendi taotleb märgiks SAS (ühiskasutusse Accessi allkiri) iga kord, kui see vajab Azure Storage. SAS sõned Azure Storage kohta leiate lisateavet teemast [Mõistmine ühiskasutusse Accessi allkirjad].

- Luua mõne selle domeenikontrolleri selle alaliikide `StorageController` käsitlemiseks SAS Turbeloa taotlusi ja saada failid, mis on seotud kirje. Vaikimisi failid on seostatud kirje, kasutades kirje ID osana container nimi; käitumise saab kohandada, määrates rakendamise `IContainerNameResolver`. Samuti saab kohandada SAS Turbeloa poliitika.

- Azure'i Mobile kliendi SDK talletamise tegelikult Salvesta faili andmeid. Pigem kliendi SDK tugineb oma `IFileSyncHandler`, mis seejärel otsustab kuidas (ja kui) failid on talletatud kohalik seadmes. Sündmuseohjuri sünkroonimine on registreeritud järgmiselt:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`nimetatakse kui Azure Mobile kliendi SDK vajab faili andmeid (nt osana üleslaadimine). See annab teile võimaluse haldamine kuidas (ja kui) failid on talletatud kohalik seadmes ja tagastada seda teavet vastavalt vajadusele.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`kutsumisel faili sünkroonimise voo osana. Faili viite ja FileSynchronizationAction väärtuse loendamine on olemas, et saaksite otsustada, kuidas rakenduse peaks hakkama sündmust (nt automaatne allalaadimine faili, kui see on loodud või värskendatud, kui fail on server kustutatud faili kustutamine kohaliku seadmega).

- A `MobileServiceFile` saab kasutada kas ühendusega või ühenduseta režiimis, kasutades mõnda `IMobileServiceTable` või `IMobileServiceSyncTable`vastavalt. Ühenduseta stsenaariumi üles juhtub rakenduse helistab `PushFileChangesAsync`. See põhjustab ühenduseta toiming kuhjuda töödeldakse; faili iga toimingu Azure Mobile kliendi SDK käivitub selle `GetDataSource` meetod on `IFileSyncHandler` eksemplar alla laadida faili sisu üles laadida.

- Mõni üksus failide toomiseks kõne on "GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` eksemplari. See meetod tagastab esitatud andmed üksusega seotud failide loend. (Märkus: see on *kohaliku* ja tulemuseks failid, mis põhineb objekti olek, kui seda viimati sünkrooniti. Server on värskendatud failide loendi saamiseks peaks annate sünkroonimine toiming esmalt.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- Faili sünkroonimisfunktsiooni kasutab kirje muudatuste teatisi kirjeid, mida kliendi saanud tõuketeatised või pull toiming osana toomiseks kohalikku salve. See on saavutada sisselülitamine kohalik ja serveri teatised sünkroonimine kontekstis kasutamise soovitud `StoreTrackingOptions` parameeter. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Muud poe jälitus suvandid on saadaval, nt ainult kohaliku või ainult server teatised. Saate lisada või oma kohandatud tagasihelistamise abil soovitud `EventManager` atribuut `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- On võimalik lisada või eemaldada failide kirjest bloobimälu otse, muutes seost saavutamist nimereeglistik kaudu. Käesoleval juhul peaksite aga alati **kirje ajatempli seotud plekid muutmisel värskendada**. Azure'i Mobile kliendi SDK värskendatakse alati kirje, kui lisada või eemaldada faili. 

    Selle nõude põhjus on selles, et mõned mobiilikliendid on juba kirje kohalikku. Kui need kliendid teha ka suureneva pull, seda kirjet ei tagastata ja klient ei päringu uute seotud failide. Selle probleemi vältimiseks on soovitatav bloobimälu salvestusruumi muutmine, mis ei kasuta Azure Mobile kliendi SDK täites värskendada kirjete ajatemplit.

- Kliendi projekti kasutab [Xamarin.Forms DependencyService] mustri laadimiseks paremale platvormi kohased klassi käitusajal. Selles näites me määratletud liidest `IPlatform` kus rakendused iga platvormi kohased projektid.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Xamarin.Forms rakenduse loomine]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Mõistmine ühiskasutusse Accessi allkirjad]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Azure'i salvestusruumi konto loomine]:  ../storage/storage-create-storage-account.md#create-a-storage-account
