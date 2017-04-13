<properties
    pageTitle="Azure'i salvestusruumi Azure PowerShelli abil | Microsoft Azure'i"
    description="Saate teada, kuidas Azure Storage Azure PowerShelli cmdlet-käskude abil saate luua ja hallata salvestusruumi kontod; töötamine plekid, tabelite, järjekorrad ja faile; konfigureerimine päringu Kasutusanalüüsi salvestusruumi ja ühiskasutusega juurdepääsu signatuuride loomine."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Azure'i salvestusruumi Azure PowerShelli kasutamine

## <a name="overview"></a>Ülevaade

Azure'i PowerShelli on leiate Azure'i kaudu Windows PowerShelli cmdlet-käskude. See on toimingupõhise käsurea kesta ja skriptimiskeele, mis on mõeldud eelkõige süsteemi haldamine. PowerShelli abil, saate juhtida ja automatiseerida oma Azure'i teenuste ja rakenduste haldamine. Näiteks saate kasutada cmdlet-käskude teha sama toiminguid, mida saate teha [Azure portaali](https://portal.azure.com)kaudu.

Sellest juhendist uurime [Azure'i salvestusruumi cmdlet-käskude](https://msdn.microsoft.com/library/azure/mt269418.aspx) kasutamine mitmesuguseid Azure Storage arengu ja haldamise ülesannete täitmiseks.

Sellest juhendist eeldab, et teil on eelnev kogemus [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) ja [Windows PowerShelli](http://technet.microsoft.com/library/bb978526.aspx)abil. Juhend pakub mitmesuguseid näidata kasutamist Azure Storage PowerShelli skriptide. Peate värskendama skripti muutujaid vastavalt kasutatavale konfiguratsioonile enne iga skripti käivitamine.

Esimene jaotis sellest juhendist leiate PowerShelli ja Azure Storage kiirpilgu. Üksikasjalikku teavet ja juhiseid alustada [eeltingimuste Azure Storage Azure PowerShelli kaudu](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>5 minutit PowerShelli ja Azure Storage töötamise alustamine

Selles jaotises kirjeldatakse, kuidas Azure Storage PowerShelli kaudu juurdepääsu 5 minutit.

**Uue Azure:** Microsoft Azure'i tellimust ja selle tellimusega seostatud Microsofti konto hankida. Azure'i ostmise suvandite kohta leiate teavet leiate [Tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/), [Ostke suvandid](https://azure.microsoft.com/pricing/purchase-options/)ja [Liikme pakub](https://azure.microsoft.com/pricing/member-offers/) (MSDN-i, Microsoft Partner Network, ja BizSpark ja muude Microsofti programmide puhul).

Azure'i abonementide kohta leiate lisateavet teemast [administraatorirollide Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Pärast Microsoft Azure'i tellimus ja konto loomiseks:**

1.  Laadige alla ja installige [Azure'i PowerShelli](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Käivitamine Windows PowerShelli integreeritud skriptimine keskkonnas (ISE): Kohaliku arvuti, avage menüü **Start** . Tippige **Haldusriistad** ja klõpsake seda käivitamiseks. Klõpsake aknas **Haldustööriistad** Paremklõpsake **Windows PowerShell ISE**, klõpsake käsku **Käivita administraatorina**.
3.  **Windows PowerShell ISE**, klõpsake **faili** > **Uus** , et luua uue skriptifail.
4.  Nüüd pakume teile lihtsa skripti, mis näitab lihtsa PowerShelli käskude Azure'i salvestusruumi juurde. Skript küsib esmalt oma Azure'i konto identimisteave lisada oma Azure'i konto kohaliku PowerShelli keskkonnas. Seejärel skripti Azure tellimuse vaikesätted ja Azure salvestusruumi uue konto loomine. Järgmiseks skripti uus ümbris salvestusruumi uue konto loomine ja selle container olemasoleva pildifaili (bloobimälu) üles laadida. Pärast skripti loetleb kõik plekid selle ümbrises, see luua uue sihtkausta kohalikus arvutis ja pildi faili alla laadida.
5.  Valige jaotises järgmine kood skripti märkuste **#begin** ja **#end**vahel. Vajutage selle lõikelauale kopeerimiseks klahvikombinatsiooni CTRL + C.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  **Windows PowerShell ISE**, vajutage klahvikombinatsiooni CTRL + V skript kopeerida. Klõpsake menüü **fail** > **salvestada**. Tippige dialoogiboksi **Nimega salvestamine** akna skriptifail, näiteks "mystoragescript" nime. Klõpsake nuppu **Salvesta**.

6.  Nüüd, peate värskendama skripti muutujate põhjal sätete konfigureerimine. **$SubscriptionName** muutuja peate värskendama oma tellimus. Saate säilitada skripti määratletud muutujate või neid värskendada, kui soovite.

    - **$SubscriptionName:** Muutuja peate värskendama oma tellimuse nime. Üks järgmised kolm võimalust oma tellimuse nime leidmiseks tehke järgmist.

        lisamine. **Windows PowerShell ISE**, klõpsake **faili** > **Uus** , et luua uue skriptifail. Kopeerige järgmine skript uue skripti fail ja klõpsake nuppu **silumine** > **käivitada**. Järgmine skript küsib esmalt oma Azure'i konto identimisteave lisada oma Azure'i kohaliku PowerShelli keskkonnas konto ja seejärel kuvatakse kõik tellimused, mis on ühendatud PowerShelli kohalike seansil. Pange kirja tellimus, mida soovite kasutada, järgides selles õpetuses nimi:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Leidke ja kopeerige oma tellimuse nime [Azure portaali](https://portal.azure.com)jaoturi menüü vasakus servas nuppu **tellimused**. Kopeerige sellest juhendist skriptide käitamise ajal soovitud tellimuse nime.

        ![Azure'i portaal][Image2]

        c. Leidke ja kopeerige oma tellimuse nime [Azure klassikaline portaali](https://manage.windowsazure.com/), liikuge kerides allapoole ja nuppu **sätted** portaali vasakus servas. Klõpsake nuppu **tellimused** oma tellimuste loendi kuvamiseks. Kopeeri juhendis antud skriptide käitamise ajal soovitud tellimuse nime.

        ![Azure'i klassikaline portaal][Image1]

    - **$StorageAccountName:** Skript kasutamine antud nimi või sisestage uus nimi salvestusruumi konto jaoks. **Oluline:** Salvestusruumi konto nimi peab olema kordumatu Azure. See peab olema väiketähtedes, liiga!

    - **$Location:** Skript kasutamine antud "Lääne USA" või muud Azure kohtadesse, nt Ida-USA-s, Põhja-Europe, valige jne.

    - **$ContainerName:** Skript kasutamine antud nimi või sisestage uus nimi oma ümbrises.

    - **$ImageToUpload:** Sisestage tee pildi oma kohalikus arvutis, näiteks: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Sisestage tee kohalikku kausta salvestada faile alla laadida Azure Storage, näiteks: "C:\DownloadImages".

7.  Pärast värskendamist skripti muutujate "mystoragescript.ps1" faili, valige **fail** > **salvestada**. Klõpsake **silumine** > **käivitada** või vajutage **klahvi F5** skripti käivitamiseks.  

Pärast skripti, peaks teil olema kohaliku sihtkausta, mis sisaldab allalaaditud faili. Järgmine pilt on kuvatud näide väljund.

![Laadige alla plekid][Image3]


> [AZURE.NOTE] Jaotises "Alustamine PowerShelli ja Azure Storage 5 minutit" esitada tutvustuse Azure Storage Azure PowerShelli kasutamine. Üksikasjalikku teavet ja juhiseid, soovitame teil lugeda järgmistest jaotistest.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Azure Storage Azure PowerShelli kasutamise eeltingimused
Peate leiate Azure'i tellimuse ning konto, nagu on kirjeldatud ülal antud sellest juhendist PowerShelli cmdlet-käskude käivitamiseks.

Azure'i PowerShelli on leiate Azure'i kaudu Windows PowerShelli cmdlet-käskude. Installimine ja Azure PowerShelli häälestamise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Soovitame teil alla laadida ja installida või versioonile uusim Azure PowerShelli mooduli enne selle juhendi abil.

Saate kasutada cmdlet-käsud standard Windows PowerShelli konsooli või selle Windows PowerShell integreeritud skriptimise keskkonnas (ISE). Näiteks **Windows PowerShell ISE**avamiseks minge menüüsse Start, tippige Haldustööriistad ja klõpsake seda käivitamiseks. Klõpsake aknas Haldustööriistad Paremklõpsake Windows PowerShell ISE, klõpsake käsku Käivita administraatorina.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Kuidas hallata Azure salvestusruumi kontod

### <a name="how-to-set-a-default-azure-subscription"></a>Kuidas määrata vaikimisi Azure tellimuse
Azure Storage Azure PowerShelli kaudu haldamise kohta leiate peate autentida kliendi keskkonna Azure Azure Active Directory autentimine või serdi autentimise kaudu. Üksikasjalikku teavet, vaadake [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) . Sellest juhendist kasutab Azure Active Directory autentimine.

1.  Windows PowerShell ISE, tippige järgmine käsk Lisa oma Azure'i konto kohaliku PowerShelli keskkonnas:

    `Add-AzureAccount`

2.  Tippige aknas "Logi sisse teenusekomplekti Microsoft Azure'i" meiliaadress ja parool, mis on teie kontoga seotud. Azure'i autendib ja salvestab mandaadi teavet suletakse aken.

3.  Järgmine, käivitage järgmine käsk kuvada Azure'i kontod teie kohaliku PowerShelli keskkonnas ja veenduge, et teie konto on loetletud:

    `Get-AzureAccount`

4.  Seejärel käivitage järgmine cmdlet tellimused, mis on ühendatud PowerShelli kohalike seansil vaadata, ja veenduge, et teie tellimus on loetletud.

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Käivitage vaikimisi Azure tellimuse seadmiseks valige-AzureSubscription cmdlet-käsk:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Kontrollige vaikimisi tellimuse nime käivitades Get-AzureSubscription cmdlet-käsk:

    `Get-AzureSubscription -Default`

7.  Kõik saadaolevad PowerShelli cmdletid Azure Storage kuvamiseks käivitage:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Kuidas luua uus Azure storage konto
Azure'i salvestusruumi kasutamiseks peate salvestusruumi konto. Saate luua uue Azure storage konto pärast tellimuse ühenduse arvuti on konfigureeritud.

1.  Käivitage saadaval andmekeskuse koha leidmiseks Get-AzureLocation cmdlet-käsk:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Järgmisena käivitage uus-AzureStorageAccount cmdlet-käsk salvestusruumi uue konto loomiseks. Järgmises näites luuakse uus konto salvestusruumi "Lääne USA" andmekeskuses.

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] Teie salvestusruumi konto nimi peab olema kordumatu Azure'is ja peab olema väiketähtedeks. Nimereeglid ja piirangute kohta leiate teemast [Azure salvestusruumi kontod](storage-create-storage-account.md) ja [nimetamise ja viitamine ümbriste, plekid, ja metaandmed](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Kuidas määrata vaikekonto Azure salvestusruum
Teie tellimus võib olla mitu salvestusruumi kontod. Saate valida ühe ja selle seadmine salvestusruumi käsud salvestusruumi vaikekonto sama PowerShelli seanss. See võimaldab teil ilma salvestusruumi konteksti konkreetselt salvestusruumi Azure PowerShelli käskude käivitamiseks.

1.  Tellimuse salvestusruumi vaikekonto seadmiseks käivitada cmdlet Set-AzureSubscription.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Järgmisena käivitage Get-AzureSubscription cmdlet veenduge, et konto salvestusruumi on seostatud teie tellimuse vaikekonto. See käsk tagastab praeguse tellimuse, sh oma praeguse salvestusruumi konto tellimuse atribuudid.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Loetle kõik Azure salvestusruumi kontod tellimuse kohta
Iga Azure'i tellimus võib olla kuni 100 salvestusruumi kontod. Kõige ajakohasemat teavet piirangud, leiate [Azure'i tellimus ja teenuste piirangud, kvootide ja piiranguid](../azure-subscription-service-limits.md).

Käivitage järgmine cmdlet-käsk välja nime ja oleku praeguselt tellimuselt salvestusruumi kontode leidmiseks:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Kuidas luua Azure storage keskkonnas
Azure'i salvestusruumi kontekst on objekti PowerShellis kapseldada salvestusruumi mandaat. Salvestusruumi konteksti abil, mis tahes järgmise cmdleti käitamise ajal võimaldab autentida teie taotlus ilma salvestusruumi konto ja selle kiirklahv otseselt. Saate luua salvestusruumi konteksti mitmel viisil, näiteks kasutades salvestusruumi konto nimi ja Accessi võti, ühiskasutusega juurdepääsu allkirja (SAS) luba, ühendusstringi, või Anonüümsetelt. Lisateavet leiate teemast [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx).  

Kasutage ühte salvestusruumi konteksti loomiseks on järgmised kolm võimalust.

- Käivitage [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) cmdlet-käsk välja selgitada esmased kiirklahv Azure storage konto jaoks. Järgmiseks helistada [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) cmdlet salvestusruumi konteksti loomiseks.

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Luua märgiks ühiskasutusega juurdepääsu allkirja on Azure storage container ja selle abil luua salvestusruumi kontekstis:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Lisateabe saamiseks vt [New-AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) ja [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md).

- Mõnel juhul võite määrata teenuse lõpp-punktid, kui loote uue salvestusruumi kontekstis. See võib olla vajalik, kui olete registreerunud salvestusruumi konto jaoks kohandatud domeeninime bloobimälu teenuse või soovite kasutada ühiskasutusega juurdepääsu signatuuri juurdepääsuks salvestusruumi ressursid. Teenuse lõpp-punktid ühendusstringi seadmine ja selle abil saate luua uue salvestusruumi konteksti, nagu allpool näidatud:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Ühendusstringi salvestusruumi konfigureerimise kohta lisateabe saamiseks vt [Ühendusstringi konfigureerimine](storage-configure-connection-string.md).

Nüüd, kui teil on häälestada oma arvuti ja teada, kuidas hallata tellimused ja salvestusruumi kontod Azure PowerShelli kaudu, avage saate teada, kuidas hallata Azure plekid ja Bloobivahemälu hetktõmmiseid järgmise jaotise juurde.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Kuidas hankida ja taastada Azure storage võtmed

Azure Storage konto on kaks konto võtmed. Järgmine cmdlet-käsk abil saate tuua oma võtmed.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Järgmine cmdlet-käsk abil saate tuua teatud klahvi. Sobivad väärtused on esmane ja teisene.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Kui soovite taastada oma klahvid, kasutage järgmist cmdletti. -KeyType sobivad väärtused on "" ja ""

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Kuidas hallata Azure plekid
Azure'i bloobimälu on teenus salvestamiseks suure hulga struktureerimata andmed, nt teksti või binaarsed andmed, millele pääseb juurde kõikjalt maailmas HTTP- või HTTPS-i kaudu. Selles jaotises eeldab, et teil on juba tuttavad Azure'i bloobimälu salvestusteenus põhimõtet. Üksikasjalikku teavet leiate teemast [Alustamine bloobimälu kasutades .net-i](storage-dotnet-how-to-use-blobs.md) ja [Bloobimälu teenuste kontseptsioonide](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Kuidas luua ümbris
Ümbris peab olema iga laos Azure'i bloobimälu. Privaatne ümbris New-AzureStorageContainer cmdlet-käsu abil saate luua.

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] On kolm taset anonüümse lugemisõigus: **väljalülitamine**, **bloobimälu**ja **ümbrises**. Anonüümse juurdepääsu vältimiseks plekid, seadke parameetri õiguse **off**. Vaikimisi uus ümbris on privaatne ja pääsevad juurde ainult konto omanik. Anonüümse avaliku lugemisõigus bloobimälu ressursse, kuid mitte container metaandmete või loendisse plekid ümbrises lubamiseks parameetri õiguste määramine **bloobimälu**. Täielik avaliku lugemisõigus bloobimälu ressursid lubamiseks container metaandmete ja plekid ümbrises, loendi seatud õiguste parameeter **ümbrises**. Lisateavet leiate teemast [haldamine anonüümse lugemisõigus ümbriste ja plekid](storage-manage-access-to-resources.md).

### <a name="how-to-upload-a-blob-into-a-container"></a>Kuidas üles laadida üheks ümbris on bloobimälu
Azure'i bloobimälu toetab plokk plekid ja lehe plekid. Lisateabe saamiseks vt [mõistmine Blokeeri plekid, plekid lisada, ja lehe plekid](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Üles laadida plekid ümbris, saate kasutada cmdlet-käsu [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) . Vaikimisi lisatud selle käsu kohalike failide blokeerimise bloobimälu. Määrake soovitud bloobimälu jaoks, saate kasutada - BlobType parameeter.

Järgmises näites käivitatakse [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet saada kõik failid määratud kausta ja edastab need müügivõimaluste tehtemärkide abil järgmine cmdlet-käsk. Cmdlet [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) lisatud kohaliku failid oma container:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Kuidas alla laadida plekid pakendist
Järgmises näites näitab, kuidas alla laadida plekid pakendist. Käesolevas näites esmalt loob ühenduse Azure Storage salvestusruumi konto kontekstis, mis sisaldab salvestusruumikonto nimi ja selle esmane kiirklahv abil. Seejärel näites toob bloobimälu viite [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-käsu abil. Järgmiseks näide kasutab [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) cmdlet allalaadimiseks plekid kohaliku sihtkoha kaust.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Ühe salvestusruumi container plekid kopeerimine teise
Saate kopeerida plekid salvestusruumi kontod ja piirkondade vahel asünkroonselt. Järgmises näites näitab, kuidas ühe salvestusruumi container plekid kopeerimiseks teisele üle kahe eri salvestusruumi kontod. Näide esmalt määrab muutujad lähte-kui ka salvestusruumi kontod ja seejärel loob iga konto salvestusruumi kontekstis. Järgmiseks näiteks kopeerib plekid source container sihtkoha ümbris [Algus-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) cmdlet-käsu abil. Näide eeldab, et lähte-kui ka salvestusruumi kontod ja ümbriste juba olemas.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Pange tähele, et selles näites teeb selle asünkroonne koopia. Saate jälgida olekut iga koopia käivitades cmdleti [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) .

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Kuidas kopeerida plekid teisene asukohast
Saate kopeerida plekid RA GRS lubatud konto teisene asukoht.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Kuidas kustutada mõne bloobimälu
Mõne bloobimälu kustutamiseks esmalt saada bloobimälu viide ja seejärel helistamiseks Eemalda-AzureStorageBlob cmdlet-käsk selle. Järgmises näites kustutab kõik plekid antud ümbrises. Käesolevas näites esmalt määrab muutujate salvestusruumi konto ja seejärel loob salvestusruumi kontekstis. Järgmiseks näites toob bloobimälu viite [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-käsu abil ja käivitab [Eemalda-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) cmdlet eemaldada plekid Azure laos pakendist.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Kuidas hallata hetktõmmiseid Azure'i bloobimälu
Azure'i võimaldab teil luua mõne bloobimälu hetktõmmise. Hetktõmmise on kirjutuskaitstud versioon bloobimälu, mis on võetud samal ajal aega. Kui hetktõmmise on loodud, seda saab lugeda, kopeerida, või kustutatud, kuid mitte muuta. Hetktõmmiseid võimalda varundamine on bloobimälu, nagu see kuvatakse ajahetkel. Lisateabe saamiseks lugege teemat [on bloobimälu hetktõmmise loomine](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Kuidas luua hetktõmmise bloobimälu
Snaphot, on bloobimälu loomiseks esmalt saada bloobimälu viide ja seejärel kõne on `ICloudBlob.CreateSnapshot` seda meetodit. Järgmises näites esmalt määrab muutujate salvestusruumi konto ja seejärel loob salvestusruumi kontekstis. Järgmiseks näites toob bloobimälu viite [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-käsu abil ja käivitab [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) meetodit hetktõmmist.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Loendite lisamine bloobimälu hetktõmmiseid
Saate luua nii palju pilte, kui soovite mõne bloobimälu. Saate loetleda pilte, mis on seotud teie bloobimälu praeguse tõmmised jälgimiseks. Järgmine näide kasutab eelmääratletud bloobimälu ja helistab [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet selle bloobimälu hetktõmmiste loendi.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Kuidas kopeerida mõnda bloobimälu hetktõmmise
Saate kopeerida hetktõmmise bloobimälu, taastamine hetktõmmis. Üksikasjaliku teabe ja piirangute kohta leiate teemast [on bloobimälu hetktõmmise loomine](http://msdn.microsoft.com/library/azure/hh488361.aspx). Järgmises näites esmalt määrab muutujate salvestusruumi konto ja seejärel loob salvestusruumi kontekstis. Järgmiseks näide määratleb container ja bloobimälu nimi muutujaid. Näites toob bloobimälu viite [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-käsu abil ja käivitab hetktõmmist [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) meetod. Seejärel käivitatakse näide [Algus-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) cmdlet bloobimälu, mis on ICloudBlob objekti kasutamise allika bloobimälu hetktõmmist kopeerimiseks. Ärge unustage vastavalt kasutatavale konfiguratsioonile enne käivitamist näide muutujaid. Pange tähele, et järgmises näites eeldatakse, et allika ja sihtkoha ümbriste ja allika bloobimälu juba olemas.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Nüüd, kui olete õppinud, kuidas hallata Azure plekid ja Bloobivahemälu hetktõmmiseid Azure PowerShelli abil, minge järgmise jaotise saate teada, kuidas tabelite, järjekorrad ja failide haldamiseks.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Kuidas hallata Azure tabelite ja tabeli üksused
Azure'i tabeli salvestusteenus on NoSQL andmesalve, mille abil saate talletada ja päringu suur liigendatud, mitte relatsiooniliste andmete hulki. Põhilised teenus on tabelite, üksused ja atribuudid. Tabel on üksuste kogum. Üksus on atribuutide kogum. Iga üksuse võib olla kuni 252 atribuudid, mis on kõik nime ja väärtuse paarideks. Selles jaotises eeldab, et teil on juba tuttavad Azure'i tabeli salvestusteenus põhimõtet. Üksikasjalikku teavet leiate teemast [tabeli teenuse andmemudeli mõistmine](http://msdn.microsoft.com/library/azure/dd179338.aspx) ja [Alustamine Azure'i tabelimälu .net-i abil](storage-dotnet-how-to-use-tables.md).

Järgmistes osades, saate teada, kuidas hallata Azure'i tabeli salvestusteenus Azure PowerShelli kaudu. Stsenaariumid, mis hõlmab kaasata **loomine**, **kustutamine**ja **toomine** **tabelid**, kui ka **lisamise**, **päringu**ja **tabeli üksuste kustutamine**.

### <a name="how-to-create-a-table"></a>Kuidas tabeli loomine
Iga tabeli peab asuma Azure storage konto. Järgmises näites näitab, kuidas Azure Storage tabeli loomiseks. Käesolevas näites esmalt loob ühenduse Azure Storage salvestusruumi konto konteksti, mis sisaldab salvestusruumikonto nimi ja selle kiirklahv abil. Järgmiseks kasutab cmdlet-käsu [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) Azure Storage tabeli loomiseks.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Kuidas hankida tabeli
Saate otsida ja tuua ühe või kõigi tabelite salvestusruumi konto. Järgmises näites näitab, kuidas antud tabeli [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) cmdlet-käsu abil saate tuua.

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Kui helistate Get-AzureStorageTable cmdlet ilma parameetrid, saab kõik salvestusruumi tabelid salvestusruumi konto.

### <a name="how-to-delete-a-table"></a>Tabeli kustutamine
Saate kustutada tabeli salvestusruumi konto [Eemaldamine-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) cmdleti abil.  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Tabeli üksuste haldamine
Praegu ei paku Azure PowerShelli cmdlet-käskude tabeli üksuste otse haldamine. Tabeli üksustele toimingute tegemiseks saate kasutada tunnid, mis on antud [Azure'i salvestusruumi kliendi teek .net-i jaoks](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Kuidas lisada tabeli üksused
Üksuse lisamiseks tabeli esmalt luua objekti, mis määratleb oma üksuse atribuudid. Üksus võib olla kuni 255 atribuutide, sh 3 Süsteemiatribuudid: **PartitionKey**, **RowKey**ja **ajatempli**. Teie vastutate lisamise ja värskendamise **PartitionKey** ja **RowKey**väärtusi. Server haldab väärtus **ajatempli**, mida ei saa muuta. Koos **PartitionKey** ja **RowKey** kordumatult iga tabeli üksus.

-   **PartitionKey**: määratleb sektsiooni, mis on talletatud üksus.
-   **RowKey**: See tuvastab kordumatult sektsiooni soovitud üksus.

Saate määrata kuni 252 kohandatud atribuutide üksus. Lisateavet leiate teemast [tabeli teenuse andmemudeli mõistmine](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Järgmises näites näitab, kuidas üksuste lisamine tabelisse. Näites kujutatakse tuua töötajate tabeli ja lisada sinna mitu üksust. Kõigepealt see loob ühenduse Azure Storage salvestusruumi konto kontekstis, mis sisaldab salvestusruumikonto nimi ja selle kiirklahv abil. Järgmiseks toob selle tabeli [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) cmdlet-käsu abil. Kui tabel pole, kasutatakse cmdlet-käsu [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) Azure Storage tabeli loomiseks. Järgmiseks näide määratleb üksuste lisamiseks tabeli iga üksuse sektsiooni ja rea klahvi määrates üksuse lisamine kohandatud funktsiooni. Funktsiooni lisamine üksuse kutsub cmdlet-käsu [New-objekti](http://technet.microsoft.com/library/hh849885.aspx) [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) klassi üksuse objekti loomiseks. Hiljem, näiteks kutsub [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) meetod selle üksuse objekti lisamine tabelisse.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Kuidas päringu tabeli üksused
Päringu tabeli, kasutage [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) klassi. Järgmises näites eeldab, et olete juba käivitada skripti, mis on toodud juhised juhendi üksuste jaotise lisada. Käesolevas näites esmalt loob ühenduse Azure Storage salvestusruumi konteksti, mis sisaldab salvestusruumikonto nimi ja selle kiirklahv abil. Järgmisena proovib varem loodud tabel "Töötajad" [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) cmdlet-käsu abil saate tuua. Cmdlet-käsu [New-objekti](http://technet.microsoft.com/library/hh849885.aspx) , kutsudes Microsoft.WindowsAzure.Storage.Table.TableQuery klassi loob uue päringu objekti. Käesolevas näites otsitakse üksused, mis on 'ID' veerg, mis väärtusega on määratletud filtri stringi 1. Üksikasjalikku teavet leiate teemast [päringute tabelite ja üksused](http://msdn.microsoft.com/library/azure/dd894031.aspx). Selle päringu käivitamisel tagastab see kõik üksused, mis vastavad määratud filtrikriteeriumide.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Tabeli üksuste kustutamine
Saate kustutada selle sektsiooni ja rida klahvide abil üksus. Järgmises näites eeldab, et olete juba käivitada skripti, mis on toodud juhised juhendi üksuste jaotise lisada. Käesolevas näites esmalt loob ühenduse Azure Storage salvestusruumi konteksti, mis sisaldab salvestusruumikonto nimi ja selle kiirklahv abil. Järgmisena proovib varem loodud tabel "Töötajad" [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) cmdlet-käsu abil saate tuua. Kui tabel on olemas, näiteks kõned [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) meetodi abil tuua üksus oma sektsiooni ja rida võtme väärtuste põhjal. Seejärel edastama üksuse [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) meetodi abil kustutada.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Kuidas hallata Azure järjekorrad ja järjekorda sõnumid
Azure'i järjekorda salvestusruumi on teenus hoidke ühiskaustas palju sõnumeid, millele pääseb juurde kõikjalt maailmas autenditud telefonikõned HTTP- või HTTPS-i kaudu. Selles jaotises eeldab, et teil on juba tuttavad Azure'i järjekorda salvestusteenus põhimõtet. Üksikasjalikku teavet leiate teemast [Alustamine Azure'i järjekorda salvestusruumi .net-i abil](storage-dotnet-how-to-use-queues.md).

Selles jaotises näitab teile, kuidas hallata Azure järjekorda salvestusteenus Azure PowerShelli kaudu. Stsenaariumid, mis hõlmab kaasata **lisamise** ja **kustutamise** järjekorda sõnumeid, kui ka **loomine**, **kustutamine**ja **järjekorrad toomine**.

### <a name="how-to-create-a-queue"></a>Kuidas luua järjekord
Järgmises näites esmalt loob ühenduse Azure Storage salvestusruumi konto kontekstis, mis sisaldab salvestusruumikonto nimi ja selle kiirklahv abil. Järgmiseks see nõuab [New-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) cmdlet-käsu luua nimega "queuename" järjekorda.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Azure'i järjekorda teenuse nimereeglid kohta lisateabe saamiseks vt [järjekorrad nime andmise ja metaandmed](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Kuidas hankida järjekord
Saate otsida ja tuua teatud järjekorda või loendi kõigi järjekorda salvestusruumi konto. Järgmises näites näitab, kuidas tuua määratud järjekorda [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet-käsu abil.

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Kui helistate [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet ilma parameetrid, saab selle loendi kõigi järjekorda.

### <a name="how-to-delete-a-queue"></a>Kuidas kustutada järjekord
Järjekorda ja kõik selles sisalduvad sõnumid kustutamiseks helistada Eemalda-AzureStorageQueue cmdlet-käsk. Järgmises näites kirjeldatakse, kuidas kustutada määratud järjekorda Eemalda-AzureStorageQueue cmdlet-käsu abil.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Kuidas lisada sõnumi järjekorda
Sõnumi lisamiseks mõne olemasoleva järjekorra esmalt luua uue eksemplari [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klassi. Järgmiseks helistage [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) meetod. Mõne CloudQueueMessage saab luua string (vormingus UTF-8) või baitide massiivis.

Järgmises näites näitab, kuidas lisada sõnumi järjekorda. Käesolevas näites esmalt loob ühenduse Azure Storage salvestusruumi konto konteksti, mis sisaldab salvestusruumikonto nimi ja selle kiirklahv abil. Järgmiseks toob abil cmdleti [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) määratud järjekorda. Kui järjekorda, cmdlet-käsu [New-objekti](http://technet.microsoft.com/library/hh849885.aspx) saab luua [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klassi eksemplar. Hiljem, näiteks kõned [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) meetodit selle sõnumi objekti lisamiseks järjekorda. Siin on kood, mis toob järjekorda ja lisab sõnum "MessageInfo":

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Järgmise sõnumi tühistage seisma järjekorras kohta
Teie kood tühistage järjekorrad sõnumi järjekorda on kaks toimingut. Kui helistate [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) meetod, kuvatakse järgmine teade järjekorda. Tagastatud **GetMessage** sõnumi muutub nähtamatuid ühegi muu koodi lugemine sõnumeid selles järjekorras. Sõnumi eemaldamine kuhjuda lõpetamiseks helistada ka [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) meetod. Kontrollimiseks, eemaldades sõnumi tagab, et kui teie kood ei töödelda sõnumile või riistvara tõrke tõttu, muus eksemplaris oma koodi saate sama teade ja proovige uuesti. Teie kood kõned **DeleteMessage** paremale sõnum on töödeldud.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Azure'i failikettad ja failide haldamine
Azure'i failisalvestusruumi pakub jagatud salvestusruumi rakenduste standard SMB protokolli abil. Microsoft Azure'i virtuaalmasinates ja pilveteenustega saab jagada faili andmete üle rakenduse komponendid kaudu ühendatud osakud ja kohapealse rakendused pääsevad faili andmete ühiskasutamine faili mälu API või Azure PowerShelli kaudu.

Azure'i failisalvestusruumi kohta leiate lisateavet teemast [alustamine Windows Azure'i failide talletamine](storage-dotnet-how-to-use-files.md) ja [Faili teenuse REST API -ga](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Kuidas määrata ja päringu salvestusruumi analytics
[Azure'i salvestusruumi Analytics](storage-analytics.md) abil saate koguda mõõdikute Azure salvestusruumi kontod ja logiandmed salvestusruumi kontole saadetud kohta. Talletusruumi mõõdikute abil saate jälgida salvestusruumi konto ja salvestusruumi logimine diagnoosimine ja salvestusruumi kontoga seotud probleemide tõrkeotsing.
Vaikimisi on lubatud talletusruumi mõõdikute salvestusruumi teenused. Saate lubada jälgimise Azure portaali või Windows PowerShelli abil või programmiliselt abil salvestusruumi kliendi teek. Salvestusruumi logimine juhtub serveripoolne ja võimaldab kirjete nii õnnestunud ja nurjunud taotlused teie salvestusruumi konto üksikasjad. Need logid võimaldavad näha üksikasju lugeda, kirjutada ja kustutada toimingute eest tabelite, järjekordade ja plekid kui ka nurjunud taotlusi põhjused.

Saate teada, kuidas lubada ja talletusruumi mõõdikute andmeid PowerShelli kaudu vaadata, vaadake, [Kuidas lubada talletusruumi mõõdikute PowerShelli kaudu](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Saate teada, kuidas lubada ja PowerShelli kaudu salvestusruumi logimise andmeid tuua, vt [lubamine salvestusruumi logimine PowerShelli kaudu](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) ja [teie salvestusruumi logimine Logi andmete otsimine](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Üksikasjalikku teavet talletusruumi mõõdikute ja salvestusruumi logimine salvestusruumi probleemide tõrkeotsingu kohta leiate [jälgimis, diagnoosimine, ja tõrkeotsingu Microsoft Azure'i Tabelimäluga](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Kuidas hallata ühiskasutusse antud juurdepääs allkirja (SAS) ja talletatud juurdepääsu poliitika
Ühiskasutusega juurdepääsu allkirjad on oluline osa mudelit, mis tahes rakenduse abil Azure Storage. Need on kasulikud esitada piiratud õiguste, salvestusruumi kontole kliendid, mida ei peaks konto võti. Vaikimisi pääsevad ainult salvestusruumi konto omanik plekid, tabelite ja järjekorrad selle konto. Kui teie teenuse või rakenduse peab kättesaadavaks nende ressursside teiste klientide ilma ühiskasutuse Accessi tootevõti, on teil kolm võimalust:

- Lubada ümbris ja selle plekid anonüümse lugemisõigus on container õiguste määramine. See on lubatud, tabelite või järjekordade jaoks.
- Ühiskasutusega juurdepääsu signatuuri, et toetused piiratud juurdepääsuõigus ümbriste, plekid, järjekorrad ja tabelite teatud ajavahemiku jaoks kasutada.
- Kasutada salvestatud Accessi poliitika on täiendavad taseme juhtida ühiskasutuses Accessi allkirjade ümbris või selle plekid, järjekorda või tabeli saamiseks. Poliitika salvestatud access võimaldab teil algusaeg, aeg või signatuuri õiguste muutmine või tühistab selle pärast see on välja antud.

Ühiskasutusega juurdepääsu signatuuri võib olla üks kahel viisil:

- **Sihtotstarbelise SAS**: kui loote sihtotstarbelise SAS, alguskellaaeg, kehtivuse aeg ja muude õiguste on kõik määratud SAS URI. Seda tüüpi SAS luuakse container, bloobimälu, tabel või kuhjuda ja see on revokable.
- **Salvestatud Accessi poliitika SAS**: Salvestatud Accessi poliitika on määratletud ressursi container bloobimälu container, tabelis või järjekorda - ja abil saate hallata piiranguid ühe või mitme ühiskasutusega juurdepääsu allkirjade. Kui soovitud SAS seostada salvestatud Accessi poliitika muude pärib piiranguid - algusaeg, aeg ja õiguste - salvestatud Accessi poliitika jaoks määratletud. Seda tüüpi SAS on revokable.

Lisateabe saamiseks vt [Abil ühiskasutusse Accessi allkirjad (SAS)](storage-dotnet-shared-access-signature-part-1.md) ja [haldamine anonüümse ümbriste ja plekid lugemisõigus](storage-manage-access-to-resources.md).

Järgmistest jaotistest saate teada, kuidas Azure'i tabelite jaoks ühiskasutusse antud juurdepääs allkirja Turbeloa ja talletatud juurdepääsu poliitika loomiseks. Azure'i PowerShelli pakub sarnaseid cmdlettide ümbriste, plekid ja järjekorrad ka. Selles jaotises skriptide käitamiseks allalaadimine [Azure PowerShelli versioon 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) või uuem versioon.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Kuidas luua poliitika-põhise ühiskasutusse Accessi allkirja luba
Salvestatud Accessi uue poliitika loomiseks saate kasutada cmdlet-käsu New-AzureStorageTableStoredAccessPolicy. Seejärel helistage cmdleti [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) luua uue poliitika põhineva ühiskasutusega juurdepääsu allkirja loa on Azure Storage Tabel.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Kuidas luua sihtotstarbelise (mitte revokable) ühiskasutusse antud juurdepääs allkirja luba
[Uus-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) cmdlet-käsu abil saate luua uue sihtotstarbelise (mitte revokable) ühiskasutusse antud juurdepääs allkirja loa jaoks on Azure Storage Tabel:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Kuidas luua Accessi salvestatud poliitika
Cmdlet-käsu New-AzureStorageTableStoredAccessPolicy abil saate luua uue salvestatud Accessi poliitika on Azure Storage tabeli:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Kuidas värskendada Accessi salvestatud poliitika
Cmdlet Set-AzureStorageTableStoredAccessPolicy abil värskendada Accessi salvestatud olemasoleva poliitika on Azure Storage tabeli:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Kuidas kustutada salvestatud Accessi poliitika
Eemalda-AzureStorageTableStoredAccessPolicy cmdlet-käsu abil saate kustutada salvestatud Accessi poliitika on Azure Storage Tabel.

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Kuidas kasutada Azure Storage USA valitsuse ja Azure Hiina
Azure'i keskkond on mõne sõltumatu kasutuselevõtu Microsoft Azure'i, nt [USA valitsuse Azure'i valitsuse](https://azure.microsoft.com/features/gov/), [AzureCloud globaalne Azure](https://portal.azure.com)ja [AzureChinaCloud Azure, mida käitab 21Vianet Hiinas](http://www.windowsazure.cn/). Saate juurutada uue Azure keskkonnas USA valitsuse ja Azure Hiina.

Azure Storage AzureChinaCloud kasutamiseks peate looma salvestusruumi konteksti, mis on seotud AzureChinaCloud. Järgmiste juhiste abil saate alustada.

1.  Käivitage kuvamiseks saadaval Azure keskkonnas [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) cmdlet-käsk:

    `Get-AzureEnvironment`

2.  Windows PowerShelli Azure'i Hiina konto lisamiseks tehke järgmist.

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Salvestusruumi kontekstis AzureChinaCloud konto loomiseks tehke järgmist.

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Azure Storage [USA Azure'i valitsuse](https://azure.microsoft.com/features/gov/)kasutamiseks tuleks määratlemine uude keskkonda ja seejärel luua uue salvestusruumi kontekstis selles keskkonnas.

1.  Käivitage kuvamiseks saadaval Azure keskkonnas [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) cmdlet-käsk:

    `Get-AzureEnvironment`

2.  Windows PowerShelli Azure'i USA valitsuse konto lisamiseks tehke järgmist.

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Salvestusruumi kontekstis AzureUSGovernment konto loomiseks tehke järgmist.

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Lisateavet leiate teemast:

- [Microsoft Azure'i Government arendaja juhend](../azure-government-developer-guide.md).
- [Erinevused Hiina Service rakenduse loomisel ülevaade](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Järgmised sammud
Sellest juhendist olete õppinud, kuidas hallata Azure Storage Azure PowerShelli abil. Siin on mõned seotud artiklid ja nende kohta rohkem ressursse.

- [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)
- [Azure'i salvestusruumi PowerShelli cmdlet-käsud](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Windows PowerShelli viide](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
