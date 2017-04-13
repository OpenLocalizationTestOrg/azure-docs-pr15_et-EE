<properties
    pageTitle="Õpetus: Krüptimiseks ja dekrüptida plekid Microsoft Azure Storage Azure'i klahvi Vault abil | Microsoft Azure'i"
    description="Selle õpetuse tutvustatakse, kuidas krüptida ja dekrüptida bloobimälu, mis on kliendipoolne krüptimine Microsoft Azure'i salvestusruumi Azure'i klahvi Vault."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Õpetus: Krüptimiseks ja dekrüptida plekid Microsoft Azure Storage Azure'i klahvi Vault abil

## <a name="introduction"></a>Sissejuhatus

Selle õpetuse antakse ülevaade teha kliendipoolne salvestusruumi krüptimise Azure'i klahvi Vault koos kasutamine. See tutvustatakse, kuidas krüptida ja nende tehnoloogiate konsooli rakendus on bloobimälu dekrüptida.

**Hinnanguline aega:** 20 minutit

Azure'i klahvi Vault kohta ülevaate leiate teemast [mis on Azure klahvi Vault?](../key-vault/key-vault-whatis.md).

Kliendipoolne krüptimise Azure Storage kohta ülevaate leiate teemast [kliendipoolne krüptimise ja Azure klahvi Vault Microsoft Azure Storage](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse lõpuleviimiseks peab olema järgmised:

- Azure Storage konto
- Visual Studio 2013 või uuem versioon
- Azure'i PowerShelli


## <a name="overview-of-client-side-encryption"></a>Kliendipoolne krüptimise ülevaade

Kliendipoolne krüptimise Azure Storage ülevaate leiate teemast [kliendipoolne krüptimise ja Azure klahvi Vault Microsoft Azure Storage](storage-client-side-encryption.md)

Siin on Lühikirjeldus sellest, kuidas klientrakenduse küljel krüptimise:

1. Azure Storage klient SDK loob sisu krüptovõtme (CEK), mis on üks kasutamiseks sümmeetriline võti.
2. Kliendiandmete on krüptitud selle CEK.
3. Funktsiooni CEK on seejärel pakitud (krüptitud) klahv krüptovõtme (KEK). Selle KEK on tähistatud võtme identifikaator ja saate asümmeetriline võtme jutumärkide paar ilma või sümmeetriline võti ja saate olla hallatavate kohalikult või talletatud Azure'i klahvi Vault. Salvestusruumi kliendi ise kunagi on juurdepääs on KEK. See viitab lihtsalt võtme mähkimine algoritmi, mis on esitatud klahvi Vault. Kohandatud pakkujate kasutamiseks klahvi mähkimine/unwrapping soovi korral saate valida kliendid.
4. Krüptitud andmed on seejärel laadida Azure'i salvestusteenus.


## <a name="set-up-your-azure-key-vault"></a>Oma Azure'i klahvi Vault häälestamine
Selle õpetuse jätkamiseks peate tegema järgmist, mis on esitatud õpetusega [Alustamine Azure'i klahvi Vault](../key-vault/key-vault-get-started.md):

- Saate luua võtme vault.
- Klahv või salajane lisamine võtme hoidlasse.
- Azure Active Directory rakenduse registreerida.
- Kasutage või salajane rakenduse autoriseerida.

Märkige üles ClientID ja Azure Active Directory rakenduse registreerimisel loodud ClientSecret.

Võtme võlvkelder luua nii võtmed. Oleme endale õpetuse ülejäänud, et olete kasutanud järgmised nimed: ContosoKeyVault ja TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Paketid ja AppSettings konsooli rakenduse loomine

Visual Studio, looge uus konsool rakendus.

Lisage vajalikud Nugeti paketid Package Manager konsooli.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


Lisage soovitud App.Config AppSettings.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Lisage järgmine `using` laused ja veenduge, et viide System.Configuration lisamine projekti.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Meetodi saada märgiks konsooli rakenduse lisamine

Klahv Vault klassid, mis on vaja juurdepääsu oma võtme vault autentida kasutab järgmisel viisil.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Salvestusruum ja selle klahvi Vault juurde oma programmis

Funktsiooni põhi, lisage järgmine kood.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Võtme Vault objekti mudelite
>
>See on oluline mõista on tegelikult kaks klahvi Vault objekti mudeleid olema teadlik: üks põhineb REST API (KeyVault nimeruum) ja teine on kliendipoolne krüptimise pikendamine.

> Klahv Vault kliendi suhtleb REST API-ga ja mõistab JSON Web võtmed ja kahte tüüpi toiminguid, mis sisalduvad klahvi Vault saladused.

> Klahv Vault laiendid on tunnid, et tundub kliendipoolne krüptimise Azure Storage jaoks loodud. Need sisaldavad liidest klahvid (IKey) ja tunnid klahvi lahendaja mõiste põhjal. On kaks rakendusi IKey, mida peaksite teadma: RSAKey ja SymmetricKey. Nüüd need juhtuda mis langeb kokku asjad, mis sisalduvad klahvi Vault, kuid sel hetkel nad on sõltumatu Tunnid (nii, et klahvi ja klahvi Vault kliendi toodavate salajane IKey juuruta).


## <a name="encrypt-blob-and-upload"></a>Bloobimälu krüptimiseks ja üles laadida
Lisage järgmine kood on bloobimälu krüptimiseks ja üleslaadimine Azure storage konto. Kasutatav meetod **ResolveKeyAsync** tagastab mõne IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Järgmine on [Azure klassikaline portaali](https://manage.windowsazure.com) jaoks bloobimälu, mis on talletatud klahvi Vault võti kliendipoolne krüptimise abil krüptitud kuvatõmmis. Atribuut **KeyId** on võti hoidla, mis toimib soovitud KEK klahvi URI. Atribuut **EncryptedKey** sisaldab selle CEK krüptitud versiooni.

![Kuvatõmmis bloobimälu metaandmed, mis sisaldab krüptimise metaandmete][1]

> [AZURE.NOTE] Kui vaatate BlobEncryptionPolicy ehitaja, näete, et saate aktsepteerida klahvi ja/või lahendaja. Pange tähele, mis ei toeta praegu ei saa kasutada lahendaja krüptimiseks, sest see pole praegu vaikimisi klahvi.



## <a name="decrypt-blob-and-download"></a>Bloobimälu dekrüptida ja allalaadimine
Dekrüptimine on eriti kui lahendaja tunnid abil teha. Kasutatud krüptimise võti ID-d on seotud bloobimälu oma metaandmete, seega pole ühtegi põhjust, miks teile tuua klahvi ja klahvi ja bloobimälu vahel seose meeles pidada. Teil on lihtsalt veenduge, et klahv võlvkelder jääb võti.   

Klahv Vault, jääb alles mõne RSA võti privaatvõti nii dekrüptimine tekkida, krüptitud võtme bloobimälu metaandmeid, mis sisaldab soovitud CEK on saadetud võti Vault dekrüptimine jaoks.

Lisage äsja üleslaaditud bloobimälu dekrüptida järgmist.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] On paar muud tüüpi selsüünid hõlbustamiseks võtme haldamise, sh: AggregateKeyResolver ja CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Kasutage klahvi Vault saladusi
Võimalus kasutada salajase kliendipoolne krüptimise abil on SymmetricKey klassi kaudu, kuna salajase on põhiosas sümmeetriline võti. Kuid nagu ülal, võti Vault salajase kaarti täpselt on SymmetricKey. On mõned asjad, mida mõista.


- Sisestage soovitud SymmetricKey peab olema fikseeritud pikkus: 128, 192, 256, 384 või 512 bittide.
- Sisestage soovitud SymmetricKey peaks olema Base64 kodeeringuga.
- Klahv Vault salajane, mis on SymmetricKey, kasutatakse peab olema "rakenduse/oktettide-voo" sisutüübi klahvi võlvkelder.

Siin on näide PowerShellis loomiseks salajane võti hoidla, mis on SymmetricKey saab kasutada.
Märkus: Kõva kodeeritud väärtuse $key, on tutvustamise eesmärgil. Oma kood peaksite luua see võti.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

Konsooli rakendus, saate sama kõne nagu enne selle salajase nimega on SymmetricKey toomiseks.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

See on õige. Nautige!

## <a name="next-steps"></a>Järgmised sammud

Microsoft Azure'i Tabelimäluga C# kasutamise kohta leiate lisateavet teemast [Microsoft Azure'i salvestusruumi kliendi teek .net-i jaoks](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Bloobimälu REST API kohta leiate lisateavet teemast [Bloobimälu teenuse REST API -ga](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Microsoft Azure'i Tabelimäluga uusimat teavet, minge [Microsoft Azure'i salvestusruumi meeskonna ajaveebi](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
