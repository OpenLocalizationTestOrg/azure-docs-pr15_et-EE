<properties 
    pageTitle="Failide üleslaadimine Media Servicesi kontole ülejäänud abil | Microsoft Azure'i" 
    description="Saate teada, kuidas tuua meediumisisu Media Services abil luua ja üles laadida varad." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Failide üleslaadimine Media Servicesi kaudu ülejäänud kontole

 > [AZURE.SELECTOR]
 - [.NET-I](media-services-dotnet-upload-files.md)
 - [ÜLEJÄÄNUD](media-services-rest-upload-files.md)
 - [Portaal](media-services-portal-upload-files.md)

Media Services, saate faile üles laadida oma digitaalse arvesse vara. [Varade](https://msdn.microsoft.com/library/azure/hh974277.aspx) üksus võib sisaldada video, heli, pilte, pisipiltide saidikogumid, teksti jälitab ja tiitrite failide (ja nende failide metaandmeid.)  Kui failid on üles laaditud vara sisse, talletatakse sisu turvaliselt edasiseks töötlemiseks ja streaming pilveteenuses. 

>[AZURE.NOTE]Järgmisega kehtivad valimisel kuvatakse varade faili nimi.
>
>- Media Services kasutab IAssetFile.Name atribuudi väärtus, kui hoone URL-ide streaming sisu (nt http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Seetõttu protsentkodeering pole lubatud. **Nime** atribuudi väärtus ei tohi olla järgmised [protsendi-kodeeringus-reserveeritud märgid](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Samuti ei saa ühte "." failinimelaiend jaoks.
>
>- Nime pikkus ei peaks olema suurem kui 260 märki.

Järgmistes jaotistes on jaotatud lihtsa töövoo varad üles:

- Vara loomine
- (Valikuline) vara krüptimine
- Faili üleslaadimine bloobimälu

AMS abil saate üles laadida varad mitmekaupa. [Lisateavet leiate jaotisest.](media-services-rest-upload-files.md#upload_in_bulk)

##<a name="upload-assets"></a>Varad üles laadida

###<a name="create-an-asset"></a>Vara loomine

>[AZURE.NOTE] Töötades Media Services REST API-ga, rakendatakse järgmisega.
>
>Üksuste Media teenuste kasutamisel peate seadma kindlate päise välju ja väärtuste HTTP-päringud. Kohta leiate teemast [Media Services REST API arengu häälestus](media-services-rest-how-to-use.md).

>Pärast ühenduse loomist https://media.windows.net, saate määrata mõne muu Media Services URI 301 uuesti. Te peate järgmise kõned uue URI, nagu on kirjeldatud [Media Servicesi kaudu REST API -ga ühenduse loomine](media-services-rest-connect-programmatically.md). 
 
Vara on mitut tüüpi või komplektid objektide meediumi teenustes video, heli, pilte, pisipiltide saidikogumid, teksti jälitab ja tiitrite failide ümbris. REST API, vara loomine nõuab Media Services postituse taotluse saatnud ja tuleks paigutada atribuudi teavet oma varade koosolekukutse kehasse.

Üks atribuudid, mida saate määrata vara loomisel on **Suvandid**. **Suvandid** on loendamine väärtus, mis kirjeldab krüptimise suvandid, mida saab luua vara. Sobiv väärtus on üks alla, mitte väärtuste kombinatsioon loendis olevad väärtused. 

- **Ükski** = **0**: kasutab krüptimist. See on vaikeväärtus. Pange tähele, et selle suvandi kasutamisel sisu pole kaitstud teel või ülejäänud salvestusruumi.
    Kui plaanite esitamisel on MP4 abil järk-järgult allalaadimine, kasutage seda suvandit. 

- **StorageEncrypted** = **1**: saate määrata, kas soovite oma failid on krüptitud AES 256 bitises krüptimist üles ja.

    Kui teie vara on krüptitud salvestusruumi, tuleb konfigureerida varade kohaletoimetamise poliitika. Lisateabe saamiseks vt [seadistamine varade kohaletoimetamise poliitika](media-services-rest-configure-asset-delivery-policy.md).

- **CommonEncryptionProtected** = **2**: saate määrata, kui levinud krüptimismeetod (nt PlayReady) kaitsega failide üleslaadimiseks. 

- **EnvelopeEncryptionProtected** = **4**: saate määrata, kui HLS krüptitud AES failide üleslaadimiseks. Pange tähele, et failid on kodeeritud ja muuta halduri krüptitud.

>[AZURE.NOTE]Kui teie vara kasutab krüptimist, peate looma ka **ContentKey** ja linkida selle oma varade järgmises teemas kirjeldatud:[Kuidas luua mõne ContentKey](media-services-rest-create-contentkey.md). Pange tähele, et pärast failide üleslaadimist vara sisse, peate värskendama **AssetFile** üksuse atribuutide krüptimise teil **varade** krüptimise ajal väärtustega. Seda **ühendamine** HTTP-päringu abil. 


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
    
    {"Name":"BigBuckBunny.mp4"}
    

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
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Mõne AssetFile loomine

Üksuse [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) tähistab video või heli faili, mis on talletatud bloobimälu ümbrises. Rakenduse varade fail on alati seostatud vara ja vara võib sisaldada ühe või mitu vara failid. Tööülesande Media Services kodeerija nurjub, kui objekti varade fail pole seotud digitaalne faili bloobimälu ümbrises.

Pange tähele, et **AssetFile** eksemplar ja tegeliku media fail on kaks erinevat objekti. AssetFile eksemplari sisaldab metaandmeid meediumifail, samal ajal media fail sisaldab tegelik meediumisisu.

Pärast üheks bloobimälu container digitaalmeediumi faili üleslaadimiseks kasutate **ühendamine** HTTP-päringu värskendamiseks on AssetFile teavet oma media fail (nagu on näidatud allpool olevat teemat) kohta. 

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
       "IsEncrypted":"false",
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
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Funktsiooni AccessPolicy kirjutamisõigust loomine. 

Enne mis tahes failide üleslaadimise üheks bloobimälu, määrake juurdepääs poliitika õiguste kirjutamiseks vara. Selleks, et POSTITADA AccessPolicies üksuse häälestada HTTP-päring. Saate määratleda DurationInMinutes väärtus pärast loomist või saate tagasi vastuseks 500 sisemine serveri tõrketeade. AccessPolicies kohta leiate lisateavet teemast [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Järgmises näites kirjeldatakse, kuidas luua ka AccessPolicy.
        
**HTTP-päring**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-päring**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

###<a name="get-the-upload-url"></a>Üleslaadimise URL-i hankimine

Saate tegelike üleslaadimise URL-i, luua SAS Locator. Lokaatorid määratleda kliendid, mida soovite juurde pääseda failide vara alguskellaaeg ja ühenduse lõpp-punkti tüüp. Saate luua mitme Locator üksuste antud AccessPolicy ja varade paar muu kliendi taotlusi ja vajadustele. Kõik need lokaatorid kasutada StartTime väärtus pluss DurationInMinutes väärtus on AccessPolicy aeg URL-i saab kasutada. Lisateavet leiate teemast [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


SAS URL on järgmises vormingus:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Mõni järgmine kehtida.

- Ei tohi olla kuni viis kordumatu lokaatorid seotud konkreetse vara korraga. Lisateavet leiate teemast Locator.
- Kui vajate kohe oma faile üles laadida, määrake oma StartTime väärtus viis minutit enne praeguse kellaaja. Selle põhjuseks võib olla kell skew oma kliendi seadme ja Media Servicesi vahel. Lisaks teie StartTime väärtus peab olema järgmises vormingus DateTime: YYYY-MM-DDTHH:mm:ssZ (nt "2014-05-23T17:53:50Z").   
- Võib olla 30-40 teise viivitamine on Locator on loodud, kui see on saadaval kasutamiseks. See probleem on olemas nii SAS URL-i ja Origin lokaatorid.

Järgmises näites kujutatakse SAS URL-i Locator, loomiseks tippige atribuudi koosolekukutse kehas ("1" SAS locator) ja "2" on tellitavate origin locator määratletud. Tagastatud **tee** atribuut sisaldab URL-i, mida tuleb kasutada oma faili üles laadida.
    
**HTTP-päring**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-vastus**

Kui õnnestus, tagastatakse järgmised vastus:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Faili üleslaadimine rakendusse bloobimälu salvestusruumi container
    
Kui olete AccessPolicy ja määrake Locator, tegelik fail on üles laaditud Azure'i bloobimälu container, mis Azure'i salvestusruumi REST API-de abil. Saate kas üleslaadimise lehe või ploki plekid. 

>[AZURE.NOTE] Peate lisama faili Locator **tee** väärtus, mis on saanud eelmises jaotises üleslaaditava faili nime. Näiteks https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Azure'i salvestusruumi plekid töötamise kohta leiate lisateavet teemast [Bloobimälu teenuse REST API -ga](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Funktsiooni AssetFile värskendamine 

Nüüd, kui teie fail on üles laaditud, FileAsset suurus (ja muud) teabe värskendamine. Näiteks:
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-vastus**

Kui õnnestub, kuvatakse järgmine tagastatakse: HTTP/1.1 204 sisu puudub

### <a name="delete-the-locator-and-accesspolicy"></a>Üldine ressursiaadress ja AccessPolicy kustutamine 

**HTTP-päring**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**HTTP-vastus**

Edu korral tagastatakse järgmist:

    HTTP/1.1 204 No Content 
    ...

**HTTP-päring**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**HTTP-vastus**

Edu korral tagastatakse järgmist:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Varade mitmekaupa üleslaadimine

###<a name="create-the-ingestmanifest"></a>Funktsiooni IngestManifest loomine

Funktsiooni IngestManifest on ümbris hulk varad ja varade failide statistiline teavet, mida saate kasutada määratlemiseks edenemise hulgi söömisega jaoks.


**HTTP-päring**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Varade loomine

Enne selle IngestManifestAsset loomist peate looma varade, mis lõpeb hulgi söömisega abil. Vara on mitut tüüpi või komplektid objektide meediumi teenustes video, heli, pilte, pisipiltide saidikogumid, teksti jälitab ja tiitrite failide ümbris. REST API, vara loomine nõuab saatmisega HTTP POST Microsoft Azure Media Servicesi ja tuleks paigutada atribuudi teavet oma varade koosolekukutse kehasse. Selles näites luuakse vara StorageEncrption(1) suvandiga kaasas koosolekukutse sisusse.

**HTTP-vastus**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>Funktsiooni IngestManifestAssets loomine

IngestManifestAssets tähistavad kasutatakse hulgi söömisega on IngestManifest vara. Funktsiooni vara põhiliselt linkimine manifest. Azure Media Servicesi jälgimine ettevõttesiseselt põhjal IngestManifestFiles saidikogumi seotud selle IngestManifestAsset faili üles laadida. Kui need failid üles, vara on lõpule viidud. Saate luua uue IngestManifestAsset HTTP POST taotluse. Koosolekukutse kehasse sisaldavad IngestManifest ID-d ja selle IngestManifestAsset peaks link koos hulgi söömisega varade ID-d.

**HTTP-vastus**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>Iga üksuse jaoks soovitud IngestManifestFiles loomine

Mõne IngestManifestFile tähistab tegeliku video või heli bloobimälu objekti, mis laaditakse hulgi söömisega vara osana. Krüptimise seotud atribuudid pole kohustuslikud, kui vara kasutab krüptimist soovitud suvand. Näiteks kasutada selles jaotises näitab, et luua mõne IngestManifestFile, mis kasutab StorageEncryption varem loodud vara.


**HTTP-vastus**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Laadige failid üles bloobimälu

Saate kasutada mis tahes võimeline varade failide üleslaadimine bloobimälu salvestusruumi container Uri esitatud BlobStorageUriForUpload atribuut, on IngestManifest kiire klientrakendusega. Ühe kiire märkimisväärne üleslaadimise teenus on [Aspera On Demand Azure'i rakenduse](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Kuvari hulgi neelata edenemine

Saate hulgi söömisega toimingute jaoks soovitud IngestManifest, küsitlused on IngestManifest atribuudi statistika edenemist jälgida. See atribuut on keerukas tüüp, [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Küsitlus atribuudi statistika, esitage HTTP GET-päring läbides IngestManifest ID-ga.
 

##<a name="create-contentkeys-used-for-encryption"></a>Kasutatakse krüptimise ContentKeys loomine

Kui teie vara kasutab krüptimist, peate looma ContentKey kasutatakse krüptimise enne varade failide loomine. Salvestusruumi krüptimine, tuleks koosolekukutse kehasse järgmised atribuudid.
 
Koosolekukutse kehasse atribuut   | Kirjeldus
---|---
ID | ContentKey Id, mille loome ise järgmises vormingus "nb:kid:UUID:<NEW GUID>".
ContentKeyType | See on klahv sisutüübi selle sisu võtme täisarv. Võtame salvestusruumi krüptimise väärtus 1.
EncryptedContentKey | Loome uue sisu võtmeväärtuse, mis on 256-bitine (32-baidine) väärtus. Võti on krüptitud salvestusruumi krüptimise x.509 vastav sert, mida me tuua Microsoft Azure Media Servicesi täites GetProtectionKeyId ja GetProtectionKey meetodite HTTP GET-päring.
ProtectionKeyId | See on kaitse võtme id salvestusruumi krüptimise x.509 vastav sert, mida kasutati meie sisu klahvi krüptimiseks.
ProtectionKeyType | See on kaitse Key, mida kasutati sisu võtme krüptimiseks krüptimise tüüp. Selles näites on see väärtus StorageEncryption(1).
Kontrollsumma |Funktsiooni MD5 arvutatud algatamiseks sisu võti. See on arvutatud krüptides sisu Id sisu võti. Näide koodi näitab, kuidas arvutada kontrollsumma.


**HTTP-vastus**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Link kuvatakse ContentKey vara

Selle ContentKey on seotud ühe või mitme vara HTTP POST kutse saatmisega. Järgmine kutse on näide näide ContentKey linkimiseks näide varade ID kaudu.

**HTTP-vastus**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**HTTP-vastus**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
