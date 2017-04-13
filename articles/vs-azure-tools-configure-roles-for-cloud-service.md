<properties
   pageTitle="Rollid konfigureerimine on Azure pilveteenuses Visual Studio abil | Microsoft Azure'i"
   description="Siit saate teada, kuidas häälestada ja konfigureerida Azure pilveteenustega rollid Visual Studio abil."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Visual Studio teenuse Azure pilveteenuse rollid konfigureerimine

Teenuse Azure pilveteenuse võib olla ühe või mitme töötaja või web rollid. Iga rolli peate määratlemine rolli häälestusest ja konfigureerida ka rolli töö. Pilveteenustega rollide kohta leiate lisateavet teemast [Sissejuhatus Azure'i pilveteenustega](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services)video. Järgmised failid on talletatud teie pilveteenuses teave:

- **ServiceDefinition.csdef**

    Teenuse definitsioonifail määratleb käitusaja sätete oma pilveteenuses, sh rollid, mis on nõutavad, lõpp-punktid ja virtuaalse masina suurust. Sellesse faili salvestatud andmeid saab muuta oma rolli töötamisel.

- **ServiceConfiguration.cscfg**

    Teenuse konfiguratsioonifail konfigureerib käivitatakse mitu eksemplari rollid ja sätete rollile määratud väärtused. Andmed salvestatakse see fail saab muuta oma rolli töötamise ajal.

Saama salvestada erinevaid väärtusi nende sätete jaoks oma rolli töö, võib olla mitu teenuse konfiguratsioone. Saate iga juurutamise keskkonna muu teenuse konfigureerimine. Näiteks saate määrata oma salvestusruumi konto ühendusstringi kohaliku Azure storage emulaator kohalikust kataloogiteenusest konfiguratsiooni ja loomine mõne muu teenuse konfigureerimine kasutama Azure'i salvestusruumi pilveteenuses.

Visual Studio pilveteenuses Azure loomisel luuakse vaikimisi kahe teenuse konfiguratsioone. Neid konfiguratsioone lisatakse Azure'i projekti. Kuvatakse on nimega:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Azure'i pilvepõhise teenuse konfigureerimine

Saate konfigureerida Azure pilveteenuse teenuse kaudu Solution Exploreris Visual Studios, nagu on näidatud järgmisel joonisel.

![Pilveteenuses konfigureerimine](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Azure'i pilvepõhise teenuse konfigureerimine

1. Iga rolli konfigureerimiseks **Solution Exploreris**: Azure'i projektis Azure'i projekti roll kiirmenüü avamine ja seejärel valige **Atribuudid**.

    Lehe roll nimi kuvatakse Visual Studio redaktoris. Kuvatakse menüü **konfiguratsiooni** väljad.

1. Valige loendist **Teenuse** nime teenuse konfiguratsiooni, mida soovite redigeerida.

    Kui soovite muuta kõik selle rolli jaoks teenuse konfiguratsiooni, saate valida **Kõik konfiguratsioone**.

    >[AZURE.IMPORTANT] Kui valite kindla teenuse konfiguratsiooni, osa atribuute on keelatud, kuna need saab panna ainult kõikide jaoks. Nende atribuutide redigeerimine, peate valima kõikide jaoks.

    Nüüd saate värskendada iga vaate lubatud atribuutide vahekaardi.

## <a name="change-the-number-of-role-instances"></a>Roll eksemplaride arvu muutmine

Oma pilveteenuses jõudluse parandamiseks saate muuta töötavad kasutajate või oodata kindla rolliga laadi arvu põhjal rolli eksemplaride arv. Eraldi virtuaalse masina luuakse iga eksemplari rolli Azure'i töötab pilveteenusesse. See mõjutab see pilveteenuses kasutuselevõtuks arveldamine. Arveldamine kohta leiate lisateavet teemast [Microsoft Azure arve mõistmine](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Muuta rolli eksemplaride arv

1. Valige vahekaart **konfigureerimine** .

1. Valige loendist **Teenuse** teenuse konfiguratsiooni, mida soovite värskendada.

    >[AZURE.NOTE] Saate seada eksemplari arv teatud teenuse konfiguratsiooni või kõikide teenuste jaoks.

1. Sisestage tekstiväljale **arvu** soovite alustada selle rolli eksemplaride arv.

    >[AZURE.NOTE] Kui avaldate oma pilveteenuses Azure'i käivitatakse iga eksemplari eraldi virtuaalse masina.

1. Klõpsake nuppu **Salvesta** tööriistaribal teenuse konfiguratsiooni faili muudatuste salvestamiseks.

## <a name="manage-connection-strings-for-storage-accounts"></a>Ühendusstringi salvestusruumi kontode haldamine

Saate lisada, eemaldada või muuta ühendusstringi teie teenuse jaoks. Võite erinevate ühendusstringi muu teenuse konfiguratsioone. Näiteks võite soovida kohaliku ühendusstringi kohalikust kataloogiteenusest konfiguratsiooni, mis on väärtus `UseDevelopmentStorage=true`. Võite ka pilveteenuses konfiguratsiooni, mis kasutab Azure storage konto konfigureerimine.

>[AZURE.WARNING] Azure storage konto olulist teavet salvestusruumi konto ühendusstringi sisestamisel see teave on salvestatud kohalikult teenuse konfiguratsioonifail. Kuid see teave on praegu ei tekstina talletatud krüptitud.

Muu väärtuse abil iga teenuse konfiguratsiooni on kasutada erinevaid ühendusstringi oma pilveteenuses või koodi muuta oma pilveteenuses avaldamisel Azure. Saate kasutada sama nime ühendusstringi koodi ja väärtus on erinevad, valite kui koostate oma pilveteenuses või kui avaldate selle teenuse konfiguratsiooni põhjal.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Ühendusstringi salvestusruumi kontode haldamine

1. Valige vahekaardil **sätted** .

1. Valige loendist **Teenuse** teenuse konfiguratsiooni, mida soovite värskendada.

    >[AZURE.NOTE] Saate värskendada ühendusstringi kindla teenuse konfiguratsiooni, kuid kui teil on vaja lisada või kustutada ühendusstringi valige kõik konfiguratsioone.

1. Ühendusstringi lisamiseks nuppu **Lisa säte** . Uue kirje lisatakse loendisse.

1. Tippige väljale **nimi** nimi, mida soovite kasutada ühendusstring.

1. Valige ripploendist **Tüüp** **Ühendusstring**.

1. Ühendusstringi väärtus muutmiseks klõpsake nuppu kolmikpunkti (…). Kuvatakse dialoogiboks **Talletusmahu ühendusstringi loomine** .

1. Kasuta kohalikku konto emulaator, valige **Microsoft Azure'i salvestusruumi emulaator** nupp ja seejärel klõpsake nuppu **OK** .

1. Azure storage konto kasutamiseks valige nupp **tellimuse** ja valige soovitud salvestusruumi konto.

1. Kohandatud mandaatide kasutamiseks klõpsake nuppu **käsitsi sisestatud identimisteabe** suvandid. Sisestage salvestusruumikonto nimi ja esmane või teise klahvi. Salvestusruumi konto ja kuidas sisestage üksikasjad salvestusruumi konto dialoogiboksis **Salvestusruumi ühendusstringi loomine** leiate teavet selle kohta, kuidas luua [ettevalmistamine avaldamine või kasutada Azure rakenduse Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Ühendusstringi kustutamiseks valige ühendusstringi ja seejärel klõpsake nuppu **Eemalda säte** .

1. Valige tööriistaribal teenuse konfiguratsiooni faili muudatuste salvestamiseks **Salvesta** ikoon.

1. Ühendusstringi teenuse konfiguratsioonifailis juurdepääsemiseks peavad teil olema konfiguratsioon sätte väärtus. Järgmine kood kuvatakse näite, kus luuakse bloobimälu ja ühendusstringi abil andmeid `MyConnectionString` kui kasutaja valib **Button1** web rolli jaoks teenuse Azure pilveteenuse lehel Default.aspx failist teenuse konfigureerimine. Lisage järgmine lauseid Default.aspx.cs abil:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Avage kujundusvaates Default.aspx.cs ja nupu lisamine tööriistakastist. Lisage järgmine kood on `Button1_Click` meetod. Järgmine kood kasutab `GetConfigurationSettingValue` saada väärtus teenuse konfiguratsioonifail ühendusstring. Seejärel soovitud bloobimälu luuakse salvestusruumi konto ühendusstringi viidatud `MyConnectionString` ja lõpuks programmi lisab teksti soovitud bloobimälu.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Kasutada oma Azure pilveteenuses kohandatud sätete lisamine

Kohandatud sätete konfiguratsioonifailis teenuse abil saate lisada nimi ja kindla teenuse konfiguratsiooni stringi väärtuse. Võite selle sätte abil saate konfigureerida funktsiooni oma pilveteenuses selle sätte väärtuse lugemine ja kasutamine selle väärtuse kontrollimiseks loogika koodi. Saate muuta nende teenuste konfigureerimine väärtuste ilma taastada oma pakett või kui teie pilveteenuses töötab. Oma koodi saate otsida teatisi, kui sätete muudatused. Vaata [Sündmuse RoleEnvironment.Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Saate lisada, eemaldada või muuta oma teenuse jaoks kohandatud sätteid. Võite erinevate väärtuste stringide muu teenuse jaoks.

Muu väärtuse abil iga teenuse konfiguratsiooni on erinevad stringid kasutada oma pilveteenuses või koodi muuta oma pilveteenuses avaldamisel Azure. Saate kasutada sama nime stringi koodi ja väärtus on erinevad, valite kui koostate oma pilveteenuses või kui avaldate selle teenuse konfiguratsiooni põhjal.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Lisada kohandatud sätteid, et kasutada oma Azure pilveteenuses

1. Valige vahekaardil **sätted** .

1. Valige loendist **Teenuse** teenuse konfiguratsiooni, mida soovite värskendada.

    >[AZURE.NOTE] Saate värskendada stringide kindla teenuse konfiguratsiooni, kuid kui teil on vaja lisada või kustutada stringi, valige **Kõik konfiguratsioone**.

1. Stringi lisamiseks nuppu **Lisa säte** . Uue kirje lisatakse loendisse.

1. Tippige väljale **nimi** nimi, mida soovite kasutada stringi.

1. Valige ripploendist **Tüüp** **String**.

1. Lisamiseks või muutmiseks stringi väärtus, tippige väljale **väärtus** tekst on uus väärtus.

1. Stringi kustutamiseks valige stringi ja seejärel klõpsake nuppu **Eemalda säte** .

1. Klõpsake nuppu **Salvesta** tööriistaribal teenuse konfiguratsiooni faili muudatuste salvestamiseks.

1. Stringi teenuse konfiguratsioonifailis juurdepääsemiseks peavad teil olema konfiguratsioon sätte väärtus.

    Veenduge, et peate järgmised abil laused juba lisanud Default.aspx.cs nagu eelmise jaotise.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Lisage järgmine kood on `Button1_Click` juurde pääsemiseks teil on juurdepääs ühendusstringi samal viisil stringile meetodit. Oma koodi saate teha mõne kindla koodi teenuse konfigureerimise faili, mida kasutatakse sätted stringi väärtuse.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Iga rolli eksemplari kohaliku salvestusruumi haldamine

Saate lisada kohalik fail süsteemi mäluruumi iga eksemplari rollid. Saate salvestada siin kohalikud andmed, mis ei pruugi olla kättesaadav teiste rollide omast. Siin saate andmeid, mida teil pole vaja salvestada tabeli, bloobimälu või SQL-andmebaasi salvestusruumi salvestatakse. Näiteks võite kasutada seda kohaliku salvestusruumi vahemälu andmeid, mis võib olla vaja uuesti kasutada. Muude versioonide rolli ei pääse talletatud andmed. 

Kohaliku sätted rakenduvad kõikide teenuste jaoks. Ainult saate lisada, eemaldada või muuta kohalik salvestusruum kõikide teenuste jaoks.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Iga rolli eksemplari kohaliku salvestusruumi haldamine

1. Valige vahekaart **Kohalikku** .

1. Valige loendist **Teenuse** **Kõikide jaoks**.

1. Kohalikku kirje lisamiseks klõpsake nuppu **Kohaliku salvestusruumi lisada** . Uue kirje lisatakse loendisse.

1. Tippige väljale **nimi** nimi, mida soovite kasutada seda kohaliku Storage.

1. Tippige väljale **suurus** teksti suurus, peate selle kohaliku Storage MB.

1. Kui virtuaalse masina selle rolli suunatakse eemaldada selle kohaliku salvestusruumi andmed, märkige ruut **Clean rollile prügikasti** .

1. Kohaliku olemasoleva kirje redigeerimiseks valige rida, mille peate värskendama. Seejärel saate redigeerida väljad, nagu on kirjeldatud eelmisi juhiseid.

1. Kohaliku kirje kustutamiseks valige salvestusruumi kirje loendist ja seejärel klõpsake nuppu **Eemalda kohaliku salvestusruumi** .

1. Teenuse failid nende muudatuste salvestamiseks valige **Salvesta** tööriistaribal ikoon.

1. Konfiguratsioonifailis teenuse lisatud kohalik salvestusruum juurdepääsemiseks peavad teil olema kohaliku ressursi konfiguratsioon sätte väärtus. Kasutage juurdepääsu seda väärtust ja looge fail nimega **MyStorageTest.txt** ja kirjutage joonele katse selle faili järgmised koodiread. Saate lisada selle koodi selle `Button_Click` meetod, mida kasutasite eelmises toiminguid:

1. Veenduge, et peate järgmised abil laused lisatakse Default.aspx.cs:

    ```
    using System.IO;
    using System.Text;
    ```

1. Lisage järgmine kood on `Button1_Click` meetod. See loob faili kohaliku salvestusruumi ja kirjutab katse seda faili.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Valikuline) Kui käivitate oma pilveteenuses kohalikult loodud faili vaatamiseks tehke järgmist:

  1. Käivitage web roll ja valige **Button1** tagada, et koodi sisse `Button1_Click` saab nimega.

  1. Olekuala Azure ikooni kiirmenüü avamine ja valige käsk **Kuva arvutada emulaator UI**. Kuvatakse dialoogiboks **Azure arvutada emulaator** .

  1. Valige web roll.

  1. Valige menüüribal **Tööriistad**, **avatud kohalikku salve**. Windows Exploreri aken.

  1. Klõpsake menüüribal, sisestage **otsinguväljale tekst** **MyStorageTest.txt** ja seejärel valige **Enter** otsingu alustamiseks.

    Otsingutulemites kuvatakse faili.

  1. Faili sisu vaatamiseks faili kiirmenüü avamine ja valige **Ava**.

## <a name="collect-cloud-service-diagnostics"></a>Pilveteenuse teenuse diagnostika kogumine

Kogute diagnostika andmeid oma Azure pilveteenuses. Andmed lisatakse salvestusruumi konto. Võite erinevate ühendusstringi muu teenuse konfiguratsioone. Näiteks võite soovida kohalikku konto kohalikust kataloogiteenusest konfiguratsiooni, mis on väärtus UseDevelopmentStorage = true. Võite ka pilveteenuses konfiguratsiooni, mis kasutab Azure storage konto konfigureerimine. Azure'i diagnostika kohta leiate lisateavet teemast logimine andmete kogumise Azure'i diagnostika abil.

>[AZURE.NOTE] Kohalik teenuse konfigureerimine on juba konfigureeritud kasutama kohalikku ressursid. Kui kasutate pilvepõhise teenuse konfigureerimine avaldada oma Azure pilveteenuses, kui avaldate määratud ühendusstring kasutatakse ka diagnostika ühendusstringi juhul, kui olete määranud ühendusstringi. Kui te oma pilveteenuses, Visual Studio abil, teenuse konfiguratsiooni Ühendusstring ei muudeta.

### <a name="to-collect-cloud-service-diagnostics"></a>Pilveteenuse teenuse diagnostika kogumiseks

1. Valige vahekaart **konfigureerimine** .

1. Valige loendist **Teenuse** teenuse konfiguratsiooni, mida soovite värskendada või valige **Kõik konfiguratsioone**.

    >[AZURE.NOTE] Saate värskendada salvestusruumi konto jaoks teatud teenuse konfiguratsiooni, kuid kui soovite lubada või keelata diagnostika peate valima kõikide jaoks.

1. Diagnostika lubamiseks märkige ruut **Luba diagnostika** .

1. Salvestusruumi konto väärtuse muutmiseks klõpsake nuppu kolmikpunkti (…).

    Kuvatakse dialoogiboks **Talletusmahu ühendusstringi loomine** .

1. Kohaliku ühendusstringi kasutamiseks Azure storage emulaator valik, ja seejärel klõpsake nuppu **OK** .

1. Kasutama seostatud Azure tellimuse salvestusruumi kontot, valige **oma** tellimust.

1. Salvestusruumi konto kasutamiseks kohaliku ühendusstring, valige suvand **käsitsi sisestada mandaat** .

    Salvestusruumi konto loomine ja sisestage üksikasjad **Salvestusruumi ühendusstringi loomine** dialoogiboksis konto salvestusruumi kohta leiate lisateavet artiklist [avaldamine või juurutamine Visual Studio Azure'i rakendust](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Valige salvestusruumi konto, mida soovite kasutada **konto**nimi.

    Kui olete käsitsi sisestamise salvestusruumi konto mandaat, kopeerige või tippige oma primaarvõtme **konto võti**. Selle klahvi saate kopeerida [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885). Kopeerida see võti, **Salvestusruumi kontod** vaate [klassikaline Azure portaali](http://go.microsoft.com/fwlink/?LinkID=213885)kaudu järgmist:
    
  1. Valige salvestusruumi konto, mida soovite kasutada oma pilveteenuses.

  1. Klõpsake nuppu **Halda kiirklahvide** ekraani allosas asub. Kuvatakse dialoogiboks **Kiirklahvide haldamine** .

  1. Kiirklahv kopeerimiseks klõpsake nuppu **Kopeeri lõikelauale** . Nüüd saate selle klahvi kleepige väljale **konto võti** .

1. Salvestusruumi kontoga, mida saate sisestada, kui ühendusstring diagnostika (ja vahemällu talletamise) avaldamisel oma pilveteenuses Azure, märkige ruut **Värskenda arengu salvestusruumi ühendusstringi diagnostika-ja vahemälu Azure Storage konto identimisteave avaldamisel Azure** .

1. Klõpsake nuppu **Salvesta** tööriistaribal teenuse konfiguratsiooni faili muudatuste salvestamiseks.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>Kasutatakse iga rolli virtuaalse masina suuruse muutmine

Saate määrata iga rolli virtuaalse masina suurus. Saate määrata ainult selle suurust kõikide teenuste jaoks. Kui valite kohapeal väiksem, siis vähem CPU südamikud, mälu ja kohalikule kettale salvestusruumi on eraldatud. Eraldatud läbilaskevõime on väiksem. Nende suurused ja eraldatud kohta leiate lisateavet teemast [suurused pilveteenustega](cloud-services/cloud-services-sizes-specs.md).

Iga virtuaalse masina Azure'i jaoks nõutavad ressursid mõjutab kulud oma pilveteenuses Azure. Azure'i arveldus kohta leiate lisateavet teemast [Microsoft Azure arve mõistmine](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>Virtuaalse masina suuruse muutmine

1. Valige vahekaart **konfigureerimine** .

1. Valige loendist **Teenuse** **Kõikide jaoks**.

1. Saate valida virtuaalse masina selle rolli jaoks, valige loendist **VM suurus** sobiv suurus.

1. Klõpsake nuppu **Salvesta** tööriistaribal teenuse konfiguratsiooni faili muudatuste salvestamiseks.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Lõpp-punktid ja serdid oma rollide haldamine

Saate konfigureerida võrgu lõpp-punktid, määrates Protocol (protokoll) ka pordinumber ja HTTPS SSL-i serdi teavet. Enne juuni 2012 toetavad TCP HTTP ja HTTPS. Juuni 2012 väljaanne toetab need protokollid ja UDP. Te ei saa kasutada UDP Arvuta emulaator Sisestuskeel lõpp-punktid. Saate kasutada ainult sisemine lõpp-punktide jaoks protokolli.

Oma Azure pilveteenuses parandada, saate luua lõpp-punktid, mis kasutavad HTTPS-protokolli. Näiteks, kui teil on pilveteenuses, mida kasutatakse klientide tellimuste ostmine, mida soovite veenduge, et nende teave on turvalist SSL-i abil.

Saate lisada ka lõpp-punktid, mida saab kasutada ettevõttesiseselt või väliselt. Väliste lõpp-punktide nimetatakse Sisestuskeel lõpp-punktid. Sisestuskeel lõpp võimaldab mõne muu kasutajad oma pilveteenusesse. Kui teil on WCF-teenus, mida soovite seada sisemise lõpp web rolli selle teenuse abil.

>[AZURE.IMPORTANT] Saate värskendada ainult jaoks kõik teenuse lõpp-punktid.

Kui lisate HTTPS-i lõpp-punktid, peate kasutama SSL-sert. Selleks saate seostada oma rolli kõikide teenuste jaoks serdid ja kasutada neid lõpp-punktide jaoks.

>[AZURE.IMPORTANT] Need serdid on pakitud pole teie teenusega. Teil tuleb üles laadida sertide eraldi Azure'i [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885)kaudu.

Mis tahes teie teenuse konfiguratsioone seostada halduse serdid rakendada ainult siis, kui teie pilveteenuses töötab Azure. Kui teie pilveteenuses töötab kohaliku arenduskeskkond, kasutatakse standardne sert, mida haldab Azure Arvuta emulaator.

### <a name="to-add-a-certificate-to-a-role"></a>Serdi lisamiseks rollid

1. Klõpsake vahekaarti **serdid** .

1. Valige loendist **Teenuse** **Kõikide jaoks**.

    >[AZURE.NOTE] Lisamiseks või eemaldamiseks serdid, valige kõik konfiguratsioone. Vajadusel saate värskendada nime ja sõrmejälje kindla teenuse konfigureerimine.

1. Selle rolli serdi lisamiseks klõpsake nuppu **Serdi lisamine** . Uue kirje lisatakse loendisse.

1. Sisestage väljale **nimi** nimi serti.

1. Valige loendis **Poe asukoht** asukoht sert, mida soovite lisada.

1. Valige loendis **Talletada nimi** poe, mida soovite kasutada serdi valimiseks.

1. Serdi lisamiseks nuppu kolmikpunkti (…). Kuvatakse dialoogiboks **Windowsi turvalisus** .

1. Valige sert, mida soovite kasutada loendist ja seejärel klõpsake nuppu **OK** .

    >[AZURE.NOTE] Kui lisate poest serdi sert, mis tahes vahe serdid lisatakse automaatselt te sätete konfigureerimine. Nende vahe serdid tuleb ka üles laadida Azure'i selleks, et teie teenuse jaoks SSL-i õigesti konfigureerimine.

1. Serdi kustutamiseks valige sert ja seejärel klõpsake nuppu **Eemalda sert** .

1. Valige tööriistaribal teenuse failid nende muudatuste salvestamiseks **Salvesta** ikoon.

### <a name="to-manage-endpoints-for-a-role"></a>Lõpp-punktid rolli haldamine

1. Valige vahekaart **lõpp-punktid** .

1. Valige loendist **Teenuse** **Kõikide jaoks**.

1. Lõpp-punkti lisamiseks klõpsake nuppu **Lisa lõpp-punkti** . Uue kirje lisatakse loendisse.

1. Tippige väljale **nimi** nimi, mida soovite kasutada seda lõpp-punkti.

1. Valige loendist **Tüüp** soovitud lõpp-punkti tüüp.

1. Valige loendist **protokolli** protokoll lõpp-punkti, mida vajate.

1. Kui see on Sisestuskeel lõpp **Avaliku pordi** tekstiväljale, sisestage avaliku pordi kasutamiseks.

1. Tippige tekstiväljale **Era Port** privaatne pordi kasutamiseks.

1. Kui lõpp-punkti jaoks on vaja https-protokolli, **SSL-i serdi nime** loendis valige sert kasutada.

    >[AZURE.NOTE] Selles loendis kuvatakse selle rolli **sertide** sakk lisatud serdid.

1. Klõpsake nuppu **Salvesta** tööriistaribal teenuse failid salvestada need muudatused.

## <a name="next-steps"></a>Järgmised sammud
Lisateavet Azure projektide Visual Studio abil lugemise [konfigureerimine on Azure projekt](vs-azure-tools-configuring-an-azure-project.md). Lisateave pilvepõhise teenuse skeemi abil lugemise [Skeemi viide](https://msdn.microsoft.com/library/azure/dd179398).
