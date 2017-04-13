<properties 
    pageTitle=".NET ContentKeys loomine" 
    description="Saate teada, kuidas luua sisu võtmed, et tagada turvaline juurdepääs vara." 
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
    ms.author="juliako"/>


#<a name="create-contentkeys-with-net"></a>.NET ContentKeys loomine

> [AZURE.SELECTOR]
- [ÜLEJÄÄNUD](media-services-rest-create-contentkey.md)
- [.NET-I](media-services-dotnet-create-contentkey.md)

Media Services võimaldab teil luua ja pakkuda krüptitud varad. Mõne **ContentKey** pakub oma **varade**s turvaline juurdepääs. 

Kui loote uue vara (nt enne saate [faile üles laadida](media-services-dotnet-upload-files.md)), saate määrata krüptimise järgmised suvandid: **StorageEncrypted**, **CommonEncryptionProtected**või **EnvelopeEncryptionProtected**. 

Varade esitamisel oma klientidele, saate ühe järgmised kaks encryptions [konfigureerimine varad dünaamiliselt krüptimist](media-services-dotnet-configure-asset-delivery-policy.md) : **DynamicEnvelopeEncryption** või **DynamicCommonEncryption**.

Krüptitud varad peavad olema seostatud **ContentKey**s. Selles artiklis kirjeldatakse, kuidas luua sisu klahvi.

>[AZURE.NOTE] Kui loote uue **StorageEncrypted** varade Media Services .NET SDK abil, on **ContentKey** automaatselt loodud ja seotud vara.

##<a name="contentkeytype"></a>ContentKeyType

Üks väärtus, et peate määrama, millal luua sisu võti on klahv sisutüüp. Valige üks järgmistest väärtustest. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Ümbriku tüüp ContentKey loomine

Järgmised koodilõigu loob ümbriku krüptimise tüüpi sisu klahvi. See seostab seejärel klahvi määratud vara.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

Helistamine

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Levinum ContentKey loomine    

Järgmised koodilõigu loob sisu klahvi levinud krüptimise tüübiga. See seostab seejärel klahvi määratud vara.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
Helistamine

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
