<properties
    pageTitle="Kasutamise bloobimälu Xamarin kaudu | Microsoft Azure'i"
    description="Azure'i salvestusruumi kliendi teek Xamarin võimaldab arendajatel luua iOS-i, Androidi ja Windowsi poest rakenduste kohalikke kasutajaliidese abil. Selle õpetuse näitab, kuidas luua rakendus kasutab Azure'i bloobimälu Xamarin abil."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Kuidas kasutada bloobimälu Xamarin kaudu

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Ülevaade

Xamarin võimaldab arendajatel kasutada ühiskasutusega C# codebase kohalikke kasutajaliidese iOS-i, Androidi ja Windowsi poe rakenduste loomiseks. Selle õpetuse näidatakse, kuidas kasutada Azure'i bloobimälu Xamarin rakendusega. Kui soovite saada lisateavet Azure Storage, enne kui kood, lugege [artikleid Sissejuhatus rakendusse Microsoft Azure'i Tabelimäluga](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Uue Xamarin rakenduse loomine

Alustamiseks selle saadame luua rakenduse, et eesmärgid Androidi, iOS-i ja Windowsi. See rakendus lihtsalt loob ümbris ja laadida soovitud bloobimälu selles ümbrises. Me kasutame Visual Studio Windows kasutamise alustamisel, kuid saab rakendada sama õppetunnid Mac OS-is Xamarin Studio abil rakenduse loomisel.

Rakenduse loomiseks tehke järgmist

1. Kui te pole seda veel teinud, laadige alla ja installige [Visual Studio Xamarin](https://www.xamarin.com/download).
2. Avage Visual Studio ja (kohalikke ühiskasutuses) tühja rakenduse loomine: **Fail > uus > projekti > platvormidel > tühi App(Native Shared)**.
3. Paremklõpsake Solution Exploreris paanil teie lahendus ja valige **Halda Nugeti paketid lahendus**. Otsige **WindowsAzure.Storage** ja installige uusim versioon ühed kõiki projekte teie lahendus.
4. Koostamine ja käivitage projekti.

Nüüd saate rakendus, mis võimaldab teil nuppu, mis suurendab vastu.

> [AZURE.NOTE] Azure'i salvestusruumi kliendi teek Xamarin toetab praegu järgmisi: kohalikke ühiskasutusse andnud, Xamarin.Forms ühiskasutusse, Xamarin.Android ja Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Container luua ja üles laadida bloobimälu

Järgmiseks lisate mõne koodi ühiskasutusega klassi `MyClass.cs` mis loob ümbris ja laadib on bloobimälu see ümbris sisse. `MyClass.cs`peaks välja nägema umbes selline:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Veenduge, et "your_account_name_here" ja "your_account_key_here" asendamine oma tegeliku konto nimi ja konto võti. Seejärel saate selle ühiskasutusse antud klassi oma iOS-i, Androidi ja Windows Phone rakendus. Võite lisada `MyClass.createContainerAndUpload()` iga projekti. Näiteks:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Käivitage rakendus

Nüüd saate selle rakenduse Android- või Windows Phone emulaator sisse. Samuti saate käivitada see rakendus iOS-i emulaator, kuid see nõuab Mac-arvutis. Selle kohta teabe saamiseks lugege dokumentatsiooni ühenduse [Visual Studio Mac-arvutisse](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Kui käivitate rakenduse, loob see ümbris `mycontainer` teie salvestusruumi konto. See peaks sisaldama bloobimälu, `myblob`, mis sisaldab teksti, `Hello, world!`. Te saate kontrollida [Microsoft Azure'i salvestusruumi Exploreri](http://storageexplorer.com/)kaudu.

## <a name="next-steps"></a>Järgmised sammud

Alustamine selle soovite õpitut Xamarin, mis kasutab Azure Storage platvormidel rakenduste loomist. See alustamine spetsiaalselt suunatud bloobimälu ühe stsenaariumi. Siiski saate teha veel palju, mitte ainult bloobimälu, vaid ka tabeli, fail ja järjekorda salvestusruumi. Palun vaadake lisateavet järgmistest artiklitest:
- [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md)
- [Azure'i Tabelimälu kasutades .net-i kasutamise alustamine](storage-dotnet-how-to-use-tables.md)
- [Alustamine Azure'i järjekorda salvestusruumi .net-i abil](storage-dotnet-how-to-use-queues.md)
- [Alustamine Windows Azure'i Failisalvestusruumi](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
