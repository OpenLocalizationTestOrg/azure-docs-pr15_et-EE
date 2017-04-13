<properties
    pageTitle="Kohapealse rakenduse abil bloobimälu (Java) | Microsoft Azure'i"
    description="Saate teada, kuidas luua konsooli rakendus lisatud pildi Azure'i, mis kuvab pildi brauseris. Java koodinäiteid."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Kohapealse rakenduse abil bloobimälu

## <a name="overview"></a>Ülevaade

Järgmises näites kuvatakse kasutamist Azure storage Azure piltide talletamiseks. Selles artiklis kood on konsooli rakendus, mis lisatud pildi Azure ja seejärel loob HTML-fail, mis kuvatakse pildi brauseris.

## <a name="prerequisites"></a>Eeltingimused

- Mõne Java arendaja Kit (JDK), 1,6 või uuem versioon on installitud.
- Azure'i SDK on installitud.
- Azure'i teekide Java JAR ja mis tahes rakendatav sõltuvus purgid, installitakse ja kasutada oma Java koostaja Koosta tee on. Azure'i teekide Java installimise kohta leiate teavet teemast [Azure SDK Java alla laadida](java-download-azure-sdk.md).
- Azure'i salvestusruumi konto häälestamine. Konto nimi ja konto võti salvestusruumi konto kasutab selle artikli kood. Vaadake, [Kuidas Loo konto salvestusruumi](storage-create-storage-account.md#create-a-storage-account) teavet teavet toomine konto võti salvestusruumi konto ja [salvestusruumi vaatamine ja kopeerimine kiirklahvide](storage-create-storage-account.md#view-and-copy-storage-access-keys) loomise kohta.

- Olete loonud kohaliku pildi fail nimega juures tee c:\\myimages\\image1.jpg. Teise võimalusena muuta   **FileInputStream** ehitaja näites kasutada erineva pildiga tee ja faili nimi.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Azure'i bloobimälu abil faili üles laadimine

Siin on esitatud üksikasjalikult. Kui soovite jätkata, kogu kood on esitatud selle artikli.

Alustage kood, sh Azure core salvestusruumi tunnid, Azure'i bloobimälu kliendi tunnid, Java IO tunnid ja **URISyntaxException** klassi impordi.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Klassi nimega **StorageSample**deklareerida ja Sulu **{**kaasata.

    public class StorageSample {

**StorageSample** klassis deklareerida stringi muutuja, mis sisaldab vaikimisi lõpp-punkti protokolli, oma salvestusruumikonto nimi ja teie konto Azure storage määratletud kiirklahv salvestusruumi. Kohatäite väärtuste asendamine **oma\_konto\_nimi** ja **oma\_konto\_klahvi** oma konto nimi ja konto võti, vastavalt.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Lisada oma **peamist**deklaratsiooni, **proovige** plokk kaasata ja vajalikud avatud sulgudes **{**kaasata.

    public static void main(String[] args)
    {
        try
        {

Deklareerida muutujate (kirjeldused on nende kasutusviisi selle näite puhul) järgmist tüüpi:

-   **CloudStorageAccount**: kasutatud konto objekti oma Azure'i salvestusruumikonto nimi ja klahvi lähtestamine ja bloobimälu kliendi objekti loomiseks.
-   **CloudBlobClient**: bloobimälu teenust kasutada.
-   **CloudBlobContainer**: bloobimälu container loomiseks kasutatud, loendi ümbrises plekid ja kustutada.
-   **CloudBlockBlob**: kohaliku pilt faili üleslaadimine ümbris kasutada.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Saate määrata **konto** muutuja väärtuse.

    account = CloudStorageAccount.parse(storageConnectionString);

Saate määrata **serviceClient** muutuja väärtuse.

    serviceClient = account.createCloudBlobClient();

**Container** muutuja väärtuse määramiseks. Me saame viide nimega **gettingstarted**ümbris.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Looge ümbris. See meetod loob ümbris, kui see pole olemas (ja tagastab **väärtuse true**). Kui ümbris olemas, tulemuseks **false**. **CreateIfNotExists** alternatiiv on **loomine** meetod (mis tagastavad vea ümbris juba olemas).

    container.createIfNotExists();

Anonüümse juurdepääsu määramiseks ümbris.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Saada Blokeeri bloobimälu, mis esindab bloobimälu Azure laos viide.

    blob = container.getBlockBlobReference("image1.jpg");

**Faili** ehitaja abil saada kohalikult loodud fail, mille saate üles laadida. Veenduge, et olete loonud seda faili enne käivitamist kood.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Kohaliku faili **CloudBlockBlob.upload** meetod kõne kaudu üles laadida. Esimese parameetrina **CloudBlockBlob.upload** meetodil on **FileInputStream** objekti, mis tähistab kohalik fail, mis laaditakse Azure salvestusruumi. Teine parameeter on suurus, baitides faili.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Kõne abimees funktsiooni nimega **MakeHTMLPage**, veendumaks, et HTML-i põhilehe, mis sisaldab ka ** &lt;pilt&gt; ** element allikas seatud bloobimälu, mis on nüüd teie Azure storage konto. Selles artiklis käsitletakse **MakeHTMLPage** kood.

    MakeHTMLPage(container);

Olekuteade ja teavet loodud HTML-i lehele printida.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Sulgege **proovige** blokeering, sisestades Sule nurksulgudega: **}**

Toime järgmised erandid:

-   **FileNotFoundException**: saate visanud **FileInputStream** või **FileOutputStream** ehitajatel.
-   **StorageException**: saate visanud Azure kliendi salvestusruumi teek.
-   **URISyntaxException**: saate visanud **ListBlobItem.getUri** meetod.
-   **Erand**: üldise erandi töötlemine.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Sulgege **peamine** , sisestades Sule nurksulgudega: **}**

Luua nimega **MakeHTMLPage** luua põhilehe HTML-i meetod. See meetod on parameetri tüüpi **CloudBlobContainer**, mida kasutatakse kordamiseks kaudu üles laaditud plekid loendit. See meetod on põhjustada erandid tüüpi **FileNotFoundException**, mille saab visati **FileOutputStream** ehitaja ja **URISyntaxException**, mille saab visati **ListBlobItem.getUri** meetodi abil. Vasak nurksulg, **{**kaasata.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Looge kohalik fail nimega **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Kirjutada kohaliku faili, lisades selle ** &lt;HTML-i&gt;**, ** &lt;päise&gt;**, ja ** &lt;keha&gt; ** elemente.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Itereerima kaudu üles laaditud plekid loendit. Iga bloobimälu HTML-i lehel luua mõne ** &lt;img&gt; ** element, mis on selle **src** atribuut saadetakse soovitud bloobimälu URI, nagu need on teie konto Azure salvestusruumi.
Kuigi lisasite ainult ühe pildi selle valimi, kui lisasite rohkem, järgmine kood itereerima need kõik.

Lihtsa, näites eeldatakse, et iga üleslaaditud bloobimälu on pilt. Kui olete värskendanud plekid peale pilte või lehe plekid asemel Blokeeri plekid, muutke vastavalt vajadusele kood.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Sule soovitud ** &lt;keha&gt; ** elemendi ja ** &lt;HTML-i&gt; ** element.

    stream.println("</body>");
    stream.println("</html>");

Sulgege kohalik fail.

    stream.close();

Sulgege **MakeHTMLPage** , sisestades Sule nurksulgudega: **}**

Sulgege **StorageSample** , sisestades Sule nurksulgudega: **}**

Järgmises näites täielik kood. Ärge unustage muuta kohatäite väärtused **oma\_konto\_nimi** ja **oma\_konto\_klahvi** vastavalt teie konto nimi ja konto võti, kasutama.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Lisaks kohaliku pildi faili üleslaadimine Azure storage, näiteks koodi loob namedindex.html kohaliku faili, mille saate avada oma üleslaaditud pildi kuvamiseks brauseris.

Kuna koodi sisaldab oma konto nimi ja konto võti, veenduge, et teie lähtekoodi on turvaline.

## <a name="to-delete-a-container"></a>Ümbris kustutamine

Kuna ostmisega salvestusruumi, mida soovite kustutada **gettingstarted** pärast seda, kui olete lõpetanud testide järgmises näites. Kasutage ümbris kustutamiseks **CloudBlobContainer.delete** meetodit.

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

**CloudBlobContainer.delete** meetodit kutsuda **CloudStorageAccount**, **ClodBlobClient**ja **CloudBlobContainer** objektide lähtestamine protsess on sama, nagu on näidatud **createIfNotExist** meetodi. Järgnevalt on lõpule jõudnud näide, mis kustutab ümbris nimega **gettingstarted**.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Ülevaate teiste bloobimälu salvestusruumi tunnid ja meetodid, vaadake, [Kuidas kasutada bloobimälu Java kaudu](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Järgmised sammud

Järgige neid linke ja Lisateavet mäluruumi keerukamate tööülesannete.

- [Azure'i salvestusruumi SDK Java](https://github.com/azure/azure-storage-java)
- [Azure'i salvestusruumi kliendi SDK viide](http://dl.windowsazure.com/storage/javadoc/)
- [Azure'i salvestusruumi teenuste REST API-ga](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)
