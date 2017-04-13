<properties 
    pageTitle="Salvestusruumi krüptimise abil AMS REST API abil sisu krüptimine" 
    description="Saate teada, kuidas krüptida sisu salvestusruumi krüptimise abil AMS REST API-de abil." 
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


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Salvestusruumi krüptimise abil AMS REST API abil sisu krüptimine

See on soovitatav krüptimiseks kohalikult AES 256 bitise krüptimise abil sisu ja laadige Azure Storage kuhu on talletatud krüptitud ülejäänud veebisaidil.

Selles artiklis annab ülevaate AMS salvestusruumi krüptimise ja näidatakse, kuidas üles laadida salvestusruumi krüptitud sisu.

- Saate luua sisu klahvi.
- Saate luua vara. Funktsiooni AssetCreationOption määramine StorageEncryption vara loomisel.

     Krüptitud varad peavad olema seostatud sisu võtmed.
- Link vara sisu võti.  
- Määrake krüptimise seotud parameetrite AssetFile üksused.
 
>[AZURE.NOTE]Kui soovite pakkuda salvestusruumi krüptitud varade, tuleb konfigureerida vara kohaletoimetamise poliitika. Enne oma varade saate voona, streaming server eemaldab salvestusruumi krüptimise ja andmevoogu abil määratud kohaletoimetamise poliitika sisu. Lisateavet leiate teemast [Varade kohaletoimetamise poliitikate konfigureerimine](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Töötades Media Services REST API-ga, rakendatakse järgmisega.
>
>Üksuste Media teenuste kasutamisel peate seadma kindlate päise välju ja väärtuste HTTP-päringud. Kohta leiate teemast [Media Services REST API arengu häälestus](media-services-rest-how-to-use.md).

>Pärast ühenduse loomist https://media.windows.net, saate määrata mõne muu Media Services URI 301 uuesti. Te peate järgmise kõned uue URI, nagu on kirjeldatud [Media Servicesi kaudu REST API -ga ühenduse loomine](media-services-rest-connect-programmatically.md). 

##<a name="storage-encryption-overview"></a>Salvestusruumi krüptimise ülevaade 

AMS salvestusruumi krüptimise kehtib **KMS-AES** režiimi krüptimise kogu faili.  AES-KMS režiim on blokeerimine salakiri, mida saab krüptida suvalise pikkus andmete ilma vajaduseta täidise. See toimib, krüptides counter plokk AES algoritmi ja seejärel XOR-se AES väljund andmetega krüptida või dekrüptida.  Kasutatud counter plokk ehitatakse kopeerides väärtus on InitializationVector selle väärtuse 0 – 7 baiti ja selle väärtuse 8-15 baiti on määratud null. 16 baiti counter blokeerida, baiti 8 kuni 15 (st vähemalt märgatavat baiti) kasutatakse lihtsa 64-bitise allkirjastamata täisarv, mis on ühe sammu võrra iga järgmise ploki andmete töödeldav ja säilitatakse võrgu bait järjestuses. Pange tähele, et kui seda täisarvu suurima väärtuse (0xFFFFFFFFFFFFFFFF), siis seda suurendades lähtestatakse Blokeeri näidiku (baiti 8-15) 64 bittide mõjutamata näidiku (st baiti 0 – 7).   Turvalisus AES-KMS režiimi krüptimise säilitamiseks antud klahvi tunnuse sisu klahvi InitializationVector väärtus peab olema kordumatu iga faili ja failide peab olema väiksem kui 2 ^ 64 plokki pikkus.  See on tagada, et mõne väärtuse antud võti kunagi uuesti kasutada. KMS režiimi kohta lisateabe saamiseks vt [selle vikilehe](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (viki artiklist kasutab Termini "Nonss" asemel "InitializationVector").

**Salvestusruumi** krüptimise kohalikult abil AES 256 bitises krüptimist Eemalda sisu krüptimiseks ja laadige Azure Storage salvestuskoha krüptitakse ülejäänud. Varade salvestusruumi krüptimise abil kaitstud automaatselt krüptimata ja enne kodeerimine süsteemi krüptitud faili asukoht ja soovi uuesti krüptitud enne üleslaadimist tagasi nimega uue väljundi varade. Salvestusruumi krüptimise esmane kasutamine puhul on, kui soovite oma kõrge kvaliteediga Sisestuskeel meediumifaile tugev krüptimist ja ülejäänud vaba.

Salvestusruumi krüptitud varade osutamiseks tuleb konfigureerida vara kohaletoimetamise poliitika, et Media Servicesi teaksid, kuidas soovite esitada oma sisu. Enne oma varade saate voona, streaming server eemaldab salvestusruumi krüptimise ja andmevoogu sisu abil määratud kohaletoimetamise poliitika (nt AES, levinud krüptimise või krüptimine pole).

##<a name="create-contentkeys-used-for-encryption"></a>Kasutatakse krüptimise ContentKeys loomine

Krüptitud varad peavad olema seostatud salvestusruumi krüptovõtme. Peate looma sisu võti kasutada krüptimiseks enne varade failide loomine. Selles jaotises kirjeldatakse, kuidas luua sisu klahvi.

Järgmised üldised juhised loomiseks sisu tutvustatakse, mida saab seostada varad, mida soovite krüptida. 

1. Salvestusruumi krüptimine, luua CEIP 32-baidine AES võti. 

    Sellest saab teie vara, mis tähendab, et kõik failid, mis on seotud vara tuleb kasutada sama sisu võti dekrüptimine sisu võti. 
2.  Helistage [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) ja [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) meetodite saada õige x.509 vastav sert, mida tuleb kasutada krüptimiseks sisu tootevõtit.
3.  Krüptige sisu tootevõti avalik võti x.509 vastav sert. 

    Media Services .NET SDK kasutab RSA OAEP tehes krüptimine.  Saate vaadata .net-i näites [EncryptSymmetricKeyData funktsioon](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Looge kontrollsumma väärtus arvutatakse võtme identifikaator ja sisu võti. .NET järgmises näites arvutab kontrollsumma GUID osa võtme identifikaator ja tühjendage ruut sisu klahvi abil.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. Luua sisu võti **EncryptedContentKey** (teisendatakse base64-kodeeringuga stringi), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**ja **kontrollsumma** väärtused olete saanud eelmiste juhiste järgi.

    
    Salvestusruumi krüptimine, tuleks koosolekukutse kehasse järgmised atribuudid.
     
    Koosolekukutse kehasse atribuut   | Kirjeldus
    ---|---
    ID | ContentKey Id, mille loome ise järgmises vormingus "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | See on klahv sisutüübi selle sisu võtme täisarv. Võtame salvestusruumi krüptimise väärtus 1.
    EncryptedContentKey | Loome uue sisu võtmeväärtuse, mis on 256-bitine (32-baidine) väärtus. Võti on krüptitud salvestusruumi krüptimise x.509 vastav sert, mida me tuua Microsoft Azure Media Servicesi täites GetProtectionKeyId ja GetProtectionKey meetodite HTTP GET-päring. Näiteks vt .NET järgmine kood: **EncryptSymmetricKeyData** meetod, mis on määratletud [siin](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | See on kaitse võtme id salvestusruumi krüptimise x.509 vastav sert, mida kasutati meie sisu klahvi krüptimiseks.
    ProtectionKeyType | See on kaitse Key, mida kasutati sisu võtme krüptimiseks krüptimise tüüp. Selles näites on see väärtus StorageEncryption(1).
    Kontrollsumma |Funktsiooni MD5 arvutatud algatamiseks sisu võti. See on arvutatud krüptides sisu Id sisu võti. Näide koodi näitab, kuidas arvutada kontrollsumma.
    

###<a name="retrieve-the-protectionkeyid"></a>Funktsiooni ProtectionKeyId tuua 
 

Järgmises näites kujutatakse tuua ProtectionKeyId, serdi sõrmejälje, peate kasutama sisu tootevõtit krüptimise serti. Selle juhise veenduge, et teil juba on asjakohane tunnistus teie arvutis.


Taotlemiseks tehke järgmist.
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Vastus:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>Toomine selle ProtectionKey soovitud ProtectionKeyId

Järgmises näites kujutatakse x.509 vastav sert abil soovitud ProtectionKeyId saite eelmises etapis toomiseks.

Taotlemiseks tehke järgmist.
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Vastus:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>Looge sisu võti 

Olete tuua x.509 vastav sert ja sisu võtme krüptimiseks kasutatavat oma avalik võti, luua **ContentKey** üksus ja seadke selle atribuudi väärtuste vastavalt sellele.

Üks väärtus, et peate määrama, millal luua sisu võti on tüüp. Salvestusruumi krüptimist, kui väärtus on "1". 

Järgmises näites kujutatakse loomiseks on **ContentKey** **ContentKeyType** komplekt salvestusruumi krüptimiseks ("1") ja **ProtectionKeyType** seatud näitamaks, et kaitse võti Id on x.509 vastav sert sõrmejälje "0".  


Taotlemine

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Vastus:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Vara loomine

Järgmises näites on kujutatud vara loomise kohta.

**HTTP-päring**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny" "Options":1}


**HTTP-vastus**

Edu korral tagastatakse järgmist:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>Funktsiooni ContentKey seostada vara

Pärast selle ContentKey loomist seostada teie vara $links toiminguga, nagu on näidatud järgmises näites:
    
Taotlemiseks tehke järgmist.
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Vastus:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Mõne AssetFile loomine

Üksuse [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) tähistab video või heli faili, mis on talletatud bloobimälu ümbrises. Rakenduse varade fail on alati seostatud vara ja vara võib sisaldada ühe või mitu vara failid. Tööülesande Media Services kodeerija nurjub, kui objekti varade fail pole seotud digitaalne faili bloobimälu ümbrises.

Pange tähele, et **AssetFile** eksemplar ja tegeliku media fail on kaks erinevat objekti. AssetFile eksemplari sisaldab metaandmeid meediumifail, samal ajal media fail sisaldab tegelik meediumisisu.

Pärast üheks bloobimälu container digitaalmeediumi faili üleslaadimiseks kasutate **ühendamine** HTTP-päringu värskendamiseks on AssetFile teavet oma media fail (ei kuvata selles teemas) kohta. 

**HTTP-päring**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

**HTTP-vastus**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
