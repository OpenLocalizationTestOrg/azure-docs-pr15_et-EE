<properties
    pageTitle="Klahv Vault häälestamine lõpuni võtme pööramine ja auditeerimine | Microsoft Azure'i"
    description="Häälestamise võtme pööramine ja jälgimise võtme vault logid teile abiks see kuidas abil"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Kuidas seadistada klahv Vault lõpuni võtme pööramine ja auditeerimine

##<a name="introduction"></a>Sissejuhatus

Pärast oma Azure'i klahvi Vault loomist saab alustada kasutamine selle vault oma võtmed ja saladusi talletamiseks. Rakenduste pole enam vaja püsima oma võtmed või saladusi, vaid taotleb neid võtme hoidlast vastavalt vajadusele. See võimaldab teil värskendada võtmed ja saladused mõjutamata käitumise rakenduse, mis avab laius võimalusi ümber tootevõti ja salajane juhtimise käitumist.

Selles artiklis tutvustatakse näide Azure'i klahvi Vault talletamiseks salajase, praegusel juhul Azure Storage konto võti, mille saab avada rakenduse kasutamine. See näidata ka selle salvestusruumi konto võti ajastatud pööre rakendamine. Lõpetuseks, juhendab tutvustava võtme vault auditilogide jälgida ja tõsta teatisi, kui ootamatud taotlused.

> \[AZURE'I. Märkus\] pole selles õpetuses mõeldud selgitatakse üksikasjalikult algse määramine üles oma Azure'i klahvi Vault. Seda teavet leiate teemast [Azure klahvi Vault alustamine](key-vault-get-started.md). Või mitu platvormi käsurea liides juhiste saamiseks vaadake teemat [õppeteema samaväärsed](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>Kuidas häälestada KeyVault

Selleks, et tuua salajase Azure'i klahvi hoidlast rakenduse, peate luua salajane ja laadige see oma vault. Seda saab teha hõlpsalt PowerShelli kaudu nagu allpool näidatud.

Mõne Azure PowerShelli seansi käivitamine ja logige sisse Azure'i kontosse järgmine käsk:

```powershell
Login-AzureRmAccount
```

Hüpikakende brauseriaknas, sisestage oma Azure'i konto kasutajanimi ja parool. Azure'i PowerShelli saavad kõik tellimused, mis on seotud selle kontoga ja vaikimisi, kasutab esimene.

Kui teil on mitu tellimust, peate oma Azure'i klahvi Vault loomiseks kasutatud konkreetse ühe määramiseks. Tippige oma konto tellimused kuvamiseks järgmist.

```powershell
Get-AzureRmSubscription
```

See tellimus, mis on seotud teie olulisi vault teil olla logimine määramiseks tippige:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Selles artiklis näitab salvestusruumi konto võti salajase nimega salvestamine, peate selle salvestusruumi konto võti saada.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Pärast toomine oma salajane, sel juhul salvestusruumi konto võti pead teisendamine turvaline stringi ja seejärel looge salajase oma võtme võlvkelder selle väärtusega.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Kas soovite saada URI jaoks äsja loodud salajane edasi. Seda kasutatakse hiljem helistades tuua oma salajane võti Vault. Käivitage PowerShelli järgmine käsk ja märkige üles 'Id' väärtuse, mis on salajane URI.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Kuidas häälestada rakendus

Nüüd, kui teil on talletatud salajase mida soovite tuua, et salajane ja seda kasutada koodi. On paar toimingut selle ees- ja kõige olulisem, mis on rakenduse registreerimisel Azure Active Directory ja seejärel räägib Azure'i klahvi Vault rakenduse teabe nii, et see võimaldab oma taotlusi saavutamiseks vajalikke.

> \[AZURE'I. Märkus\] rakenduse tuleb luua sama Azure Active Directory rentnikus oma võti Vault. 

Avage esmalt Rakendused vahekaardil Azure Active Directory

![Avatud rakendusi Azure AD](./media/keyvault-keyrotation/AzureAD_Header.png)

Valige käsk Lisa oma Azure AD uue rakenduse lisamine

![Valige rakenduse lisamine](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Lahku tüübi nimega "Web rakenduse ja/või WEB API" ja pange oma rakenduse nimi.

![Rakenduse nimi](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Pange rakenduse "Sisselogimise URL-i" ja sellise 'rakenduse ID URI". Need võivad olla midagi, mida soovite selle demo ja saate hiljem muuta vajaduse korral.

![Sisestage nõutav URI-d](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Kui rakendus on lisatud Azure AD, viiakse rakenduse lehele. Sellega menüü "Konfigureerimine" nuppu ja seejärel leidke ja kopeerige 'Kliendi ID' väärtuse. Märkige üles Kliendiid hiljem juhised.

Kas soovite luua rakenduse saaksid suhelda oma Azure AD võti edasi. Saate luua selle "Konfigureerimine" menüüs jaotises "Klahve". Märkige üles soovitud äsja loodud võtme Azure AD rakenduse kasutamiseks hiljem.

![Azure'i AD Rakenduse kiirklahvid](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Enne telefoniga sisse klahvi Vault oma rakendusest loomist peate klahvi Vault teavitamine rakenduse ja selle "õigused. Järgmine käsk võtab vault nimi ja kliendi ID rakenduste Azure AD ja annab "saada juurdepääsu oma põhilistest vault rakendus.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Selles etapis, kui olete valmis koostamise kõnede rakenduse käivitamiseks. Oma rakenduse peate esmalt installimiseks nõutav Azure klahvi hoidla ja Azure Active Directory suhelda NuGet-paketid. Visual Studio Package Manager konsooli sisestage järgmised käsud. Pöörake tähelepanu sellele veebisaidil käesoleva artikli kirjutamist ActiveDirectory paketi praegune versioon 3.10.305231913, nii, et soovite kinnitada, et uusim versioon ja vastavalt uuendada.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Rakenduse koodis, luua klassi hoida oma Active Directory autentimise meetodit. Selles näites, et klassi nimetatakse "Utils". Seejärel peate lisama, järgmised abil.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Järgmisena lisage Azure AD JWT luba toomiseks järgmisel viisil. Jaoks hooldatavust, mida soovite teisaldada kõva kodeeritud web või rakenduse konfiguratsioonist stringi väärtuse.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Lõpetuseks, saate lisada vajalikku koodi võti Vault helistada ja tuua oma salajane väärtus. Kõigepealt tuleb lisada järgmine lause abil.

```csharp
using Microsoft.Azure.KeyVault;
```

Järgmine lisamist autonoomsest klahvi Vault ja tuua oma salajane meetod kõnesid. Selle meetodi esitate salajane URI, mille salvestasite eelmises juhises. Pange tähele GetToken meetodi Utils klass loodud kohal kasutamine.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Rakenduse käivitamisel peaks nüüd olema Azure Active Directory autentimine ja seejärel toomine teie salajane väärtus Azure'i klahvi hoidlast.

##<a name="key-rotation-using-azure-automation"></a>Klahv pööre abil Azure automatiseerimine

On pööre strateegia pole hoida Azure'i klahvi Vault saladusi väärtuste rakendamiseks erinevaid võimalusi. Saladusi saab käsitsi käigus pöörata, nad võivad pööratud programatically tehtavate API kõned või need võivad pööratud on automatiseerimine skripti kaudu. Jaoks selles artiklis on meil kasutamine Azure PowerShelli koos Azure automatiseerimine muutmiseks Azure Storage konto võti ja seejärel uuendame põhilistest vault salajane selle uue tootenumbri abil. 

Selleks, et Azure automatiseerimine oma põhilistest võlvkelder salajane väärtuste määramiseks peate kliendi ID saamiseks ühendust nimega AzureRunAsConnection, mis loodi teie Azure automatiseerimine eksemplari loomise. Saate selle ID, valides oma Azure automatiseerimine eksemplarist "Vara". Sealt saate valida "Ühendused" ja seejärel valige 'AzureRunAsConnection' teenuse põhimõtet. Mida soovite rakenduse ID arvestama. 

![Azure'i automaatika kliendi ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Kui olete endiselt aknas vara kuvatakse ka soovite valida "Moodulid". Moodulid: valige "Galerii" ja otsige üles ja 'Impordi' värskendatud versiooni iga järgmisi mooduleid.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE'I. Märkus\] käesoleva artikli kirjutamist ainult ülaltoodud märkis moodulid tuleb ajakohastada skripti ühiskasutusse antud allpool. Kui leiate, et teie automatiseerimise töö nurjub, veenduge, et teil on kõik vajalikud moodulid ja nende sõltuvusi imporditud.

Pärast rakenduse ID Azure automatiseerimine-ühenduse, peate oma Azure'i klahvi Vault kindlaks teha, et see rakendus on juurdepääs värskendada oma võlvkelder saladusi. Seda saab teha järgmist PowerShelli käsu abil.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Järgmine 'Tegevusraamatud' ressursi valite jaotises oma Azure automatiseerimine eksemplar ja valige "lisamine on Käitusjuhendi". Valige kiire luua. Oma käitusjuhendi nimi ja valige "PowerShelli" käitusjuhendi tüübiks. Lisage soovi korral kirjeldus. Lõpuks nuppu "Loo".

![Käitusjuhendi loomine](./media/keyvault-keyrotation/Create_Runbook.png)

Oma uue käitusjuhendi editor paanile kleebite järgmist PowerShelli skripti.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Paani editor saate valida "Test paanil" skripti testida. Kui skript töötab vigadeta saate valida suvandi "Avalda" ja rakendage käitusjuhendi tagasi veebisaidil paani käitusjuhendi konfiguratsiooni ajakava.

##<a name="key-vault-auditing-pipeline"></a>Müügivõimaluste klahvi Vault kontrollimine

Kui häälestate on Azure klahvi Vault sisselülitamist koguda logid tehtud klahvi Vault Juurdepääsutaotluste audit. Need logid salvestatakse määratud Azure Storage konto ja saab siis välja tõmmata, jälgida ja analüüsida. Allpool jalutab läbi stsenaarium, mis mõjutab Azure'i funktsioone, Azure'i loogika rakendused ja klahvi Vault audit logid loomiseks meilisõnumit saata, kui selle hoidlast saladused tuuakse rakendus, mis vastavad web app rakenduse ID-d.

Esmalt peate selleks logige sisse oma võti Vault. Seda saab teha järgmist PowerShelli käskude kaudu (üksikasjadega näha [siin](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Kui see on lubatud, siis auditilogid hakkavad kogumise talletusmahu määratud kontole. Need logid sisaldab sündmuste kohta, kuidas ja kui teie võti võlvid vaadatakse ja kes. 

> \[AZURE'I. Märkus\] pääsete juurde oma logiteabe kõige, 10 minutit pärast võtme võlvkelder toiming. Enamikul juhtudel on kiirem kui see.

Järgmiseks on [luua mõne Azure'i teenus siini järjekorda](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Sellest saab, kus võtme vault auditilogid on lükata. Kui järjekorda, loogika rakendus on valida neid ja nende kohta. Teenuse siini loomiseks on üsna otse edasi ja allpool toodud juhiseid kõrge taseme:

1. Looge teenuse siini nimeruum (kui teil juba on üks, mida soovite kasutada seda, siis jätkake juhisega 2).
2. Liikuge sirvides teenuse siini portaali ja valige loodava järjekord nimeruumi.
3. Valige uus ja valige teenus siini -> kuhjuda ja sisestage vajalikud andmed.
4. Ostke teenuse siini ühenduseteavet, valides nimeruumi ja klõpsates _Ühendusteabe_. See teave on vaja järgmist osa.

Järgmiseks siis klahvi Vault logid salvestusruumi kontol küsitlus ja kättesaamine uute sündmuste [Azure'i ülesande loomine](../azure-functions/functions-create-first-azure-function.md) . See on funktsioon, mis käivitab ajakava.

Azure'i ülesande loomine (valige uus -> funktsioon rakenduse portaalis). Loomise ajal saate kasutada olemasoleva majutusteenuse plaani või looge uus. Teil võib valida ka dünaamilise majutusteenuse. Funktsioon majutusteenuse suvandid täpsemalt, leiate [siit](../azure-functions/functions-scale.md).

Funktsiooni Azure'i loomisel liikuge seda ja valige soovitud timer funktsioon ja C\# klõpsake nuppu **Loo** avakuva kaudu.

![Azure'i funktsioonide Start Blade](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Klõpsake menüüs _töötada_ asendage run.csx koodi järgmist:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE'I. Märkus\] veenduge, et asendada muutujate ülaltoodud osutage salvestusruumi konto, kus klahvi Vault logid kirjutada, teenuse siini varem loodud ja teatud tee võtme vault salvestusruumi logid kood.

Funktsioon jätkab uusima logifaili kontolt mäluruumi kui võti Vault logid kirjutada, haarab uusima sündmusi, et faili ja sunnib neid teenuse siini järjekorda. Kuna ühte faili võib olla mitu sündmused, nt üle täielik tunniga alla laadida ja seejärel loome funktsiooni käsitletakse ka viimase juhul, kui valiti üles ajatempel määratlemiseks _sync.txt_ faili. See tagab, et me ei push sama sündmuse mitu korda. _Sync.txt_ fail sisaldab lihtsalt viimase tekkinud sündmuse ajatempel. Logide, laadimisel on sortida ajatempli tagamaks, et nad on tellinud õigesti.

Selle funktsiooni puhul me viide teeke, mis pole saadaval välja kasti Azure'i funktsioonide paar. Need kaasamiseks läheb vaja Azure funktsioonide tõmmata neid Nugeti abil. Valige suvandi _Kuva failid_ 

![Suvandi failide vaatamine](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

ja lisage uus fail nimega _project.json_ sisuga järgmist:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Pärast _salvestamine_ käivitab Azure'i funktsioonide allalaadimiseks vajalik kahendfaile. 

Vahekaardi **integreerida** ja anda timer parameetri kasutamine funktsioonis tähendusrikas nimi. Ülaltoodud kood, eeldab see timer nimetatakse _myTimer_. Määrake soovitud [CRON avaldis](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) järgmiselt: 0 \* \* \* \* \* Timer, mis põhjustavad funktsiooni käivitamiseks üks minut. 

Sama **integreerida** menüü, lisage sisendi, mis tüüpi _Azure'i bloobimälu_. See osutab _sync.txt_ fail, mis sisaldab funktsiooni vaevama viimase sündmuse ajatempli. Seda tehakse kättesaadavaks funktsioonis parameetri nime järgi. Ülaltoodud kood, loodab Azure'i bloobimälu sisendi parameetri nimi olema _inputBlob_. Valige salvestusruumi konto, kus hakkavad asuma _sync.txt_ faili (see võib olla sama või muu salvestusruumi konto) ja sisestage tee väljale tee, kus fail asub vormingus {container-name}/path/to/sync.txt.

Mis tüüpi _Azure'i bloobimälu_ väljundi väljund lisada. See osutab _sync.txt_ faili sisend lihtsalt määratletud. Seda kasutatakse funktsiooni kirjutamiseks ajatempli vaevama viimase sündmus. Ülaltoodud kood eeldab, et seda nimetatakse _outputBlob_paramter.

Selles etapis funktsiooni on valmis. Veenduge, et aktiveerige **töötada** menüü ja _salvestage_ kood. Märkige aknas väljund koostamine vigu ja parandada neid vastavalt sellele. Kui see kogub, siis kood tuleks kohe töötama ja iga minut klahvi Vault logid kontrollib ja lükake mis tahes uute sündmuste peale määratletud teenuse siini järjekorda. Peaksite nägema käivitatakse funktsiooni log aknas iga kord kirjutamise logiteabe.

###<a name="azure-logic-app"></a>Azure'i loogika rakendus

Järgmine läheb vaja luua Azure'i loogika rakenduse, mis kättesaamine sündmusi, mis funktsioon on lükkamine teenuse siini järjekorda, sõelub sisu ja saatke meilisõnum, võttes aluseks tingimus on täidetud.

[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md) minnes New -> loogika rakendus. 

Kui rakendus loogika on loodud, liikuge selle ja valige käsk _Redigeeri_. Loogika rakenduse redaktori, valige _Teenus siini järjekorda_ hallatav api ja sisestage mandaat teenuse siini ühenduse järjekorda.

![Azure'i loogika rakenduse teenuse siini](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Edasi nuppu Lisa _tingimus_. Tingimus, aktiveerige _Täpsem redaktor_ ja sisestage järgmine, asendades selle APP_ID tegelik APP_ID oma veebirakenduse:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

See avaldis põhiosas tagastab **false** kui atribuudi **appid** sissetulevate sündmus (mis on teenus siini sõnumi kehasse) pole **appid** rakenduse. 

Nüüd looge toimingu, _kui ei, siis pole vaja midagi teha …_ valikut.

![Azure'i loogika rakenduse valige toiming](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Valige soovitud toimingu _Office 365 – meilisõnumi saatmine_. Täitke väljad luua e-posti saata, kui määratletud tingimus tagastab väärtuse false. Kui teil on Office 365 võib pilk alternatiivid saavutada sama.

Selles etapis teil lõpuni kohaletoimetamisel, mis üks minut, otsib uue klahvi Vault auditilogid. Mis tahes uue logid see otsib, lükake see neid teenuse siini järjekorda. Loogika rakendus käivitatakse kohe, kui uus sõnum maandub kuhjuda ja kui appid sees sündmus ei vasta rakenduse id helistaja rakenduse siis meilisõnumit saata. 
