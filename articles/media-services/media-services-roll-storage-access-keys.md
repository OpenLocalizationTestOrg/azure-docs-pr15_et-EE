<properties 
    pageTitle="Pärast jooksvalt salvestusruumi kiirklahvide Media Servicesi värskendada | Microsoft Azure'i" 
    description="See artikkel annab juhiseid värskendamise pärast salvestusruumi kiirklahvide Media Services." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Pärast salvestusruumi kiirklahvide Media Servicesi värskendamine

Kui loote uue Azure Media Servicesi konto, palutakse teil ka valige Azure Storage konto, mida kasutatakse teie meediumi sisu talletamiseks. Pange tähele, et saate [lisada rohkem kui ühe salvestusruumi kontod](meda-services-managing-multiple-storage-accounts.md) Media Servicesi kontole.

Uue salvestusruumi konto loomisel Azure'i loob kaks 512-bitine salvestusruumi kiirklahvide, mida kasutatakse teie salvestusruumi konto juurdepääsu autentimiseks. Hoida oma salvestusruumi ühendused kasutamist turvalisemaks, on soovitatav perioodiliselt taastada ja teie salvestusruumi kiirklahv pöörata. Selleks, et teil säilitada ühenduste kasutamise ajal saate taastada juurdepääsu võti ühe kiirklahv salvestusruumi konto on esitatud kahe kiirklahvide (esmaseid ja teiseseid). Seda toimingut nimetatakse ka "jooksva kiirklahvide".

Media Servicesi sõltub selle esitatud salvestusruumi klahvi. Täpsemalt voona või allalaadimiseks oma varasid kasutatud lokaatorid sõltuvad kiirklahv määratud salvestusruumi. AMS konto loomisel kulub sõltuvus esmased kiirklahv vaikimisi, kuid kasutajana saate värskendada salvestusruumi klahvi, mis on AMS. Te peate kindlasti teada, millised klahvi abil, järgides juhiseid selles teemas kirjeldatud Media Services. Samuti kui libisev salvestusruumi kiirklahvide, peate veenduge, et värskendada oma lokaatorid nii tekib pole katkestamist streaming teenust (seda toimingut ka kirjeldatud teema).

>[AZURE.NOTE]Kui teil on mitu salvestusruumi kontod, soovite selle toimingu iga salvestusruumi kontoga.
>
>Enne tootmise kontol selles teemas kirjeldatud juhised, veenduge, et testida neid tootmiseelse kontol.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Samm 1: Taastada teisene salvestusruumi kiirklahv

Käivitage regenereeruvate teisene salvestusruumi võti. Vaikimisi kasutatakse teisese võtme Media teenused.  Tööks salvestusruumi klahvid kohta teabe saamiseks lugege teemat [kohta: vaade, kopeerimise ja uuesti luua salvestusruumi kiirklahvide](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Samm 2: Update Media Servicesi uue teisene salvestusruumi tootenumbri kasutamine

Värskendage Media Servicesi teisene salvestusruumi kiirklahv kasutama. Saate ühe järgmistest kaks regenereeritud salvestusruumi võti sünkroonimiseks Media Servicesi.

- Kasutada Azure portaali: nime ja klahvi asuvate väärtuste otsimine, Azure'i portaalis ja valige oma konto. Sätete aken paremal. Valige aknas sätted võtmed. Sõltuvalt sellest, mis salvestusruumi võti soovite meediumi teenuste Outlookiga, valige Sünkrooni primaarvõtme või sekundaarne nuppu Sünkrooni. Sel juhul kasutada teisese võtme.

- Kasutage Media Servicesi halduse REST API-ga.

Koodi järgmises näites kujutatakse ehitada selle https://endpoint/***subscriptionId/teenuste/mediaservices/kontod/account_name*/StorageAccounts/*storageAccountName*/Key taotluse selleks, et Media Servicesi määratud salvestusruumi võti sünkroonimine. Sel juhul kasutatakse teisese salvestusruumi võtmeväärtuse. Lisateavet leiate teemast [kohta: kasutamine meediumi teenuste haldamine REST API](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Pärast seda toimingut värskendada olemasoleva lokaatorid (mis on sõltuvus vana salvestusruumi võti), nagu on näidatud järgmises etapis.

>[AZURE.NOTE]Oodake, kuni 30 minutit enne toimingute mis tahes Media teenustega (nt loomise uue lokaatorid) vältimiseks mingit mõju kohta ootel tööd.

##<a name="step-3-update-locators"></a>Samm 3: Update lokaatorid

>[AZURE.NOTE]Kui libisev salvestusruumi kiirklahvide, peate veenduge, et värskendada oma olemasoleva lokaatorid, seega pole katkestamist streaming teenust.

Oodake vähemalt 30 minutit pärast uue salvestusruumi tootenumbri sünkroonimine AMS. Seejärel saate Looge oma nõudmisel lokaatorid nii, et nad võtta sõltuvus määratud salvestusruumi võti ja säilitada olemasoleva URL-i.

Pange tähele, et kui värskendamine (või looge) SAS locator, URL-i alati muuta.

>[AZURE.NOTE] Veenduge, et saate säilitada oma nõudmisel lokaatorid olemasoleva URL-id, peate olemasoleva locator kustutada ja luua uue sama ID-ga.

.Net-i näites kujutatakse uuesti luua ka locator sama ID-ga.

Privaatne staatilise ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / Salvesta olemasoleva locator atribuudid.
var varade = locator. Varade; var accessPolicy = locator. AccessPolicy; var locatorId = locator. ID; var startDate = locator. StartTime; var locatorType = locator. Tippige; var locatorName = locator. Nimi;

Kustutage vana locator.
üldine ressursiaadress. Delete();

Kui (locator. ExpirationDateTime < = DateTime.UtcNow) {põhjustada erandi uue (String.Format ("ei saa taastada locator Id = {0}, sest selle locator aegumise aeg on varem", locator. ID)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Juhis 5: Taastada esmased kiirklahv

Esmased kiirklahv taastada. Tööks salvestusruumi klahvid kohta teabe saamiseks lugege teemat [kohta: vaade, kopeerimise ja uuesti luua salvestusruumi kiirklahvide](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Samm 6: Update Media Servicesi uue esmased tootenumbri kasutamine
    
Kasutage sama toimingut, nagu on kirjeldatud toimingus [2](media-services-roll-storage-access-keys.md#step2) ainult seekord sünkroonida uue esmased kiirklahv Media Servicesi kontoga.

>[AZURE.NOTE]Oodake, kuni 30 minutit enne toimingute mis tahes Media teenustega (nt loomise uue lokaatorid) vältimiseks mingit mõju kohta ootel tööd.

##<a name="step-7-update-locators"></a>Juhis 7: Update lokaatorid  

30 minuti järel saate Looge oma nõudmisel lokaatorid nii, et nad võtta sõltuvus uus esmased võti ja säilitada olemasoleva URL-i.

Kasutage sama toimingut, mis on kirjeldatud juhise [3](media-services-roll-storage-access-keys.md#step-3-update-locators).


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Litsents 

Soovime kinnitage järgmised inimestele, kes selle dokumendi loomiseks kasutatud: Cenk Dingiloglu Milano Gada, Seva Titov.
