<properties 
    pageTitle="Alustamine videosisu nõudmisel ülejäänud abil | Microsoft Azure'i" 
    description="Selles õpetuses juhendab teid rakendamiseks sees nõudmisel sisu kohaletoimetamise rakenduse abil Azure Media Servicesi kaudu REST API juhiseid." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Videosisu nõudmisel ülejäänud kasutamise alustamine 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](/pricing/free-trial/?WT.mc_id=A261C142F). 


See Kiirjuhend juhendab teid Video-on-Demand (VoD) sisu kohaletoimetamise rakenduse abil Azure Media Services (AMS) REST API-de rakendamise juhiseid. 

Õpetuse tutvustab lihtsa Media Servicesi töövoo ja kõige levinum programmeerimise objektid ja ülesandeid Media Servicesi arengu. Õpetuse lõpus saab voogesituseks või järk-järgult laadida valimi media faili, mis on üles laaditud, kodeeritud ja alla laaditud.  

## <a name="prerequisites"></a>Eeltingimused
Järgmine kohustuslik tarkvara on vaja alustada arendamise meediumi teenustega koos REST API-d.

- Ülevaade sellest, kuidas töötada Media Services REST API-ga. Lisateavet leiate teemast [media services-ülejäänud ülevaade](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Rakenduse teie valitud, mille saate saata HTTP päringuid ja vastuseid. Selle õpetuse kasutab [viiuldaja](http://www.telerik.com/download/fiddler). 

Järgmised toimingud on näidatud selles Kiirjuhend.

1.  Looge konto Media Services (kasutades Azure portaali).
2.  Konfigureerige streaming lõpp-punkti (kasutades Azure portaali).
1.  Ühenduse Media Servicesi konto REST API-ga.
1.  Uue varade luua ja üles laadida videofaili REST API-ga.
1.  Konfigureerige streaming üksuste REST API-ga.
2.  Kodeerida lähtefail failikogumi kohandatava bitrate MP4 REST API-ga.
1.  Avaldamine aktiva ja streaming hankimine ja järk-järgult allalaadimine URL-ide REST API-ga. 
1.  Sisu esitamine 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Azure'i portaalis Azure Media Servicesi konto loomine

Selle jaotise juhised näitab, kuidas luua konto AMS.

1. Logige sisse veebisaidil [Azure portaali](https://portal.azure.com/).
2. Valige **+ Uus** > **Media + CDN** > **Media Services**.

    ![Media Servicesi loomine](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Sisestage **MEDIA SERVICES konto loomine** nõutav väärtused.

    ![Media Servicesi loomine](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Sisestage väljale **Konto nimi**uue AMS konto nimi. Media Servicesi konto nimi on kõik väiketähtedeks või -tähed ilma tühikuteta ja on 3 kuni 24 märki.
    2. Valige tellimus, vahel eri Azure'i tellimused, mida teil on juurdepääs.
    
    2. **Ressursirühm**valige uue või olemasoleva ressurss.  Ressursirühma on ressursid, millest jagada elutsükli, õiguste ja poliitikate kogum. Lisateavet [siin](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Asukoht**, valige geograafiliselt saab talletada meediumid ja metaandmete kirjeid Media Servicesi konto jaoks. See piirkond saab töödelda ja voogesitus. Ainult saadaval Media Servicesi piirkondade kuvatakse väljal ripploendist. 
    
    3. **Salvestusruumi konto**, valige salvestusruumi konto bloobimälu Media Servicesi kontolt meediumi sisu esitada. Saate valida olemasoleva salvestusruumi konto sama geograafilise piirkonna Media Servicesi konto või luua salvestusruumi konto. Piirkonna on loodud uue konto salvestusruumi. Salvestusruumi kontonimed reeglid on samad Media Servicesi kontod.

        Lisateavet mäluruumi [siin](storage-introduction.md).

    4. Valige **Kinnita armatuurlaua** konto juurutamise edenemise kuvamiseks.
    
7. Klõpsake nuppu **Loo** vormi allservas.

    Kui konto on loodud, olek muutub **töötab**. 

    ![Media Servicesi sätted](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS konto haldamiseks (nt videote üleslaadimine, kodeerida varad, töö edenemist jälgida) aknas **sätted** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Azure'i portaalis streaming lõpp-punktid konfigureerimine

Kui te töötate Azure Media Servicesi üks kõige levinumad stsenaariumid on pakkuda oma klientidele video voogesitus kohandatava bitrate kaudu. Media Services toetab järgmisi kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks.

Media Services pakub dünaamiline pakendit, mis võimaldab teil esitada oma kohandatava bitrate MP4 kodeeritud sisu voogesitamine pole vaja talletada eelnevalt pakitud versioonid iga järgmistes vormingutes streaming Media Services (MPEG SIDEKRIIPSU, HLS, sujuva voogesituse, HDS) lihtsalt õigel ajal, poolt toetatavad.

Dünaamiliste pakendit ära, peate tehke järgmist:

- Kodeerida Standard (allikas) faili failikogumi kohandatava bitrate MP4 (kodeering juhiseid on näidatud allpool selle õpetuse).  
- Looge vähemalt üks streaming ühiku *streaming lõpp-punkti* , millest plaanite kohaletoimetamise sisu. Kuidas muuta streaming ühikute alltoodud juhiseid kuvamine

Dünaamiliste pakendit, peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi koostab ja pakutakse sobiv vastus taotluste klientrakenduses.

Luua ja muuta streaming reserveeritud üksuste arvu, tehke järgmist.


1. Klõpsake aknas **sätted** **Streaming lõpp-punktid**. 

2. Klõpsake nuppu Vaikesäte streaming lõpp-punkti. 

    Kuvatakse aken **Vaikimisi STREAMING lõpp-punkti üksikasjad** .

3. Streaming ühikute määramiseks libistage liugurit **Streaming üksused** .

    ![Streaming ühikud](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klõpsake muudatuste salvestamiseks nuppu **Salvesta** .

    >[AZURE.NOTE]Mis tahes uute üksuste eraldatud kuluda kuni 20 minutit.

## <a id="connect"></a>Media Servicesi konto REST API-ga ühenduse loomine

Kaks asja on nõutav Azure Media Servicesi kasutamisel: juurdepääsu sümboolse esitatud Azure Access Control (ACS) ja Media Servicesi URI ise. Saate kasutada mis tahes viisil, mida soovite taotlused loomisel, kui määrate õige päise väärtused ja edastamiseks klõpsake juurdepääsu luba õigesti helistamisel Media Services.

Järgmistes juhistes kirjeldatakse levinumaid töövoo ühendamiseks Media Servicesi Media Services REST API kasutamisel:

1. Saada juurdepääsu sümboolse. 
2. Ühenduse Media Servicesi URI.  

    Pidage meeles, et pärast ühenduse loomist https://media.windows.net, saate määrata mõne muu Media Services URI 301 uuesti. Te peate järgmise kõned uue URI. Võidakse kuvada ka HTTP/1.1 200 vastuse, mis sisaldab ODATA API metaandmete kirjeldus.
3. Edaspidised API kõned uus URL-i sisestamine. 
    
    Näiteks kui pärast ühenduse loomise katsel teil järgmist:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Tuleks postitada edaspidised API kõned https://wamsbayclus001rest-hs.cloudapp.net/api/.

###<a name="getting-an-access-token"></a>Saada juurdepääsu sümboolse

Meediumi teenustele juurdepääsuks otse REST API kaudu tuua juurdepääsu sümboolse ACS ja kasutada seda iga HTTP taotluse teete teenusesse ajal. Teil pole vaja mõnda muud eeltingimused enne otse ühenduse Media Services.

Järgmises näites on kujutatud HTTP taotluse päise ja keha märgiks toomiseks kasutada.

**Päis**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Sisu**:

Teil on vaja tõendada client_id ja client_secret väärtused kehas taotluse; client_id ja client_secret vastavad väärtused account_name ja AccountKey vastavalt. Need väärtused osutavad teile Media Servicesi konto häälestamisel. 

AccountKey Media Servicesi konto jaoks peab olema URL-kodeeringuga Accessi Turbeloa kutse väärtusena client_secret kasutamisel.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Näiteks: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Järgmises näites on kujutatud HTTP vastuse, mis sisaldab juurdepääs Turbeloa vastuse kehasse.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Soovitatav on laiendatud "access_token" ja "expires_in" väärtusi vahemälu. Turbeloa andmeid hiljem olema vormilt talletamist ja Media Services REST API-kõnede uuesti kasutada. See on eriti kasulik stsenaariumid, kus luba saab turvaliselt jagada mitme protsesside või arvutite vahel.

Veenduge, et jälgida juurdepääsu luba väärtus "expires_in" ja värskendage oma REST API kõned koos uute märkide vastavalt vajadusele.

###<a name="connecting-to-the-media-services-uri"></a>Ühenduse Media Servicesi URI

Media Servicesi URI juur on https://media.windows.net/. Loote peaks algselt selle URI ja 301 uuesti tagasi sisse vastuse saamiseks peate järgmise kõned uue URI. Lisaks kasutada mis tahes automaatne-redirect/jälgi loogika oma taotlused. HTTP verbide ja taotluse asutused ei edastatakse uue URI.

Üles- ja allalaadimine varade failide URI juur on https://yourstorageaccount.blob.core.windows.net/, kus on salvestusruumikonto nimi on sama ühe oma Media Servicesi konto häälestamise ajal kasutasite.

Järgmises näites näitab Media Services (https://media.windows.net/) URI juur HTTP-päring. Kutse saab 301 uuesti tagasi vastuse. Järgmise taotluse kasutab uue URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-päring**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-vastus**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP-päring** (abil uue URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-vastus**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Nüüd uue URI kasutatakse selles õpetuses.

## <a id="upload"></a>Looge uus vara ja REST API-ga video faili üleslaadimine

Media Services, saate faile üles laadida oma digitaalse arvesse vara. **Varade** üksus võib sisaldada video, heli, pilte, pisipiltide saidikogumid, teksti jälitab ja tiitrite failide (ja nende failide metaandmeid.)  Kui failid on üles laaditud vara sisse, talletatakse sisu turvaliselt edasiseks töötlemiseks ja streaming pilveteenuses. 

Üks väärtus, mis tuleb sisestada vara loomisel on varade loomise suvandid. Atribuut **Suvandid** on loendamine väärtus, mis kirjeldab krüptimise suvandid, mida saab luua vara. Sobiv väärtus on üks alla, mitte sellest loendist väärtuste kombinatsioon loendis olevad väärtused.

 
- **Ükski** = **0** - krüptimist kasutatakse. Selle suvandi kasutamisel teie sisu on kaitstud teel või ülejäänud salvestusruumi.
    Kui plaanite esitamisel on MP4 abil järk-järgult allalaadimine, kasutage seda suvandit. 
- **StorageEncrypted** = **1** - krüptib kohalikult abil AES 256 bitises krüptimist Eemalda sisu ja seejärel lisatud Azure Storage salvestuskoha krüptitakse ülejäänud. Varade salvestusruumi krüptimise abil kaitstud automaatselt krüptimata ja enne kodeerimine süsteemi krüptitud faili asukoht ja soovi uuesti krüptitud enne üleslaadimist tagasi nimega uue väljundi varade. Salvestusruumi krüptimise esmane kasutamine puhul on, kui soovite secure kvaliteetne Sisestuskeel media failide tugev krüptimist ja ülejäänud kettal.
- **CommonEncryptionProtected** = **2** – kasutage seda suvandit, kui on juba krüptitud ja levinud krüptimise või PlayReady DRM (nt sujuva voogesituse kaitstud PlayReady DRM) kaitsega sisu üleslaadimiseks.
- **EnvelopeEncryptionProtected** = **4** – kasutage seda suvandit HLS krüptitud AES üleslaadimiseks. Failid on kodeeritud ja krüptitud halduri muuta.

### <a name="create-an-asset"></a>Vara loomine

Vara on mitut tüüpi või komplektid objektide meediumi teenustes video, heli, pilte, pisipiltide saidikogumid, teksti jälitab ja tiitrite failide ümbris. REST API, vara loomine nõuab Media Services postituse taotluse saatnud ja tuleks paigutada atribuudi teavet oma varade koosolekukutse kehasse.

Järgmises näites on kujutatud vara loomise kohta.

**HTTP-päring**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

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
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Mõne AssetFile loomine

Üksuse [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) tähistab video või heli faili, mis on talletatud bloobimälu ümbrises. Rakenduse varade fail on alati seostatud vara ja vara võib sisaldada ühe või mitu AssetFiles. Tööülesande Media Services kodeerija nurjub, kui objekti varade fail pole seotud digitaalne faili bloobimälu ümbrises.

Pärast digitaalmeediumi faili üleslaadimiseks üheks bloobimälu container abil saate **ühendada** HTTP-päringu uuendada selle AssetFile teabega meediumi faili (nagu on näidatud allpool olevat teemat) kohta. 

**HTTP-päring**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
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

Enne mis tahes failide üleslaadimise üheks bloobimälu, määrake juurdepääs poliitika õiguste kirjutamiseks vara. Selleks, et POSTITADA AccessPolicies üksuse häälestada HTTP-päring. Saate määratleda DurationInMinutes väärtus pärast loomist või tagasi vastuseks 500 sisemine serveri tõrketeade kuvatakse. AccessPolicies kohta leiate lisateavet teemast [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Järgmises näites kirjeldatakse, kuidas luua ka AccessPolicy.
        
**HTTP-päring**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-vastus**

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
    X-Powered-By: ASP.NET
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

### <a name="get-the-upload-url"></a>Üleslaadimise URL-i hankimine

Saate tegelike üleslaadimise URL-i, luua SAS Locator. Lokaatorid määratleda kliendid, mida soovite juurde pääseda failide vara alguskellaaeg ja ühenduse lõpp-punkti tüüp. Saate luua mitme Locator üksuste antud AccessPolicy ja varade paar muu kliendi taotlusi ja vajadustele. Kõik need lokaatorid kasutab StartTime väärtus pluss DurationInMinutes väärtus on AccessPolicy aeg, mida saab kasutada URL-i määramiseks. Lisateavet leiate teemast [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


SAS URL on järgmises vormingus:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Mõni järgmine kehtida.

- Ei tohi olla kuni viis kordumatu lokaatorid seotud konkreetse vara korraga. Lisateavet leiate teemast Locator.
- Kui vajate kohe oma faile üles laadida, määrake oma StartTime väärtus viis minutit enne praeguse kellaaja. Selle põhjuseks võib olla kell skew oma kliendi seadme ja Media Servicesi vahel. Lisaks teie StartTime väärtus peab olema järgmises vormingus DateTime: YYYY-MM-DDTHH:mm:ssZ (nt "2014-05-23T17:53:50Z").   
- Võib olla 30-40 teise viivitamine on Locator on loodud, kui see on saadaval kasutamiseks. See probleem on olemas nii SAS URL-i ja Origin lokaatorid.

Järgmises näites kujutatakse SAS URL-i Locator, loomiseks tippige atribuudi koosolekukutse kehas ("1" SAS locator) ja "2" on tellitavate origin locator määratletud. Tagastatud **tee** atribuut sisaldab URL-i, mida tuleb kasutada oma faili üles laadida.
    
**HTTP-päring**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
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
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-vastus**

Kui õnnestub, kuvatakse järgmine tagastatakse: HTTP/1.1 204 sisu puudub

## <a name="delete-the-locator-and-accesspolicy"></a>Üldine ressursiaadress ja AccessPolicy kustutamine 

**HTTP-päring**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**HTTP-vastus**

Edu korral tagastatakse järgmist:

    HTTP/1.1 204 No Content 
    ...

**HTTP-päring**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-vastus**

Edu korral tagastatakse järgmist:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Konfigureerimine voogesituse üksuste REST API-ga

Kui te töötate Azure Media Servicesi üks kõige levinumad stsenaariumid on kohandatava bitrate streaming pakkuda oma klientidele. Kohandatava bitrate streaming, saate kliendi aktiveerige suurema või vähema bitrate voo video kuvatav praeguse võrgu läbilaskevõime, Protsessori kasutuse ja ka muud tegurid. Media Services toetab järgmisi kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG KRIIPSJOONE ja HDS (Adobe PrimeTime Access kes ainult) jaoks. 

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma kohandatava bitrate MP4 või sujuva voogesituse kodeeritud sisu streaming Media Services (MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse, HDS) poolt toetatavad ilma pakkige kaasa streaming vormingud. 

Dünaamiliste pakendit ära, peate tehke järgmist:

- saada vähemalt üks streaming ühiku **streaming lõpp-punkti **, millest plaanite oma sisu (selles jaotises kirjeldatud).
- kodeerida või transcode komplekt kohandatava bitrate MP4 või kohandatava bitrate sujuva voogesituse failide (kodeering juhiseid on seda hiljem on selles õpetuses), esitada oma Standard (allikas)  

Dünaamiliste pakendit, peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi koostab ja pakutakse sobiv vastus taotluste klientrakenduses. 


>[AZURE.NOTE] Lisateavet üksikasjad hindade kohta leiate teemast [Media Services hinnad üksikasjad](http://go.microsoft.com/fwlink/?LinkId=275107).

Streaming reserveeritud üksuste arvu muutmiseks tehke järgmist.
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Saada streaming lõpp-punkti, mida soovite värskendada

Näiteks võta esimese streaming lõpp-punkti oma konto (võib olla kuni kahe streaming lõpp-punktid töötab samal ajal olekus.)

**HTTP-päring**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-vastus**
    
Edu korral tagastatakse järgmist:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Skaala streaming lõpp-punkti
 
**HTTP-päring**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**HTTP-vastus**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Klõpsake pikaajaline toiming oleku kontrollimine

Mis tahes uute üksuste eraldatud võtab umbes 20 minutit lõpuleviimiseks. Toiming kasutamise **toimingute** meetodit olekut ja määrake toiming. Selle toimingu Id tagastati **skaala** taotluse vastuseks.

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**HTTP-päring**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**HTTP-vastus**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Kodeerida failikogumi kohandatava bitrate MP4 lähtefail

Pärast söömisega varad media Media teenustesse saate kodeeritud, transmuxed, vesimärkidega, jne, enne see saadetakse kliendid. Need tegevused on ajastatud ja vastuolus mitmes eksemplaris tausta rolli suurt jõudlust ja kättesaadavuse tagamiseks. Need tegevused nimetatakse töö ja iga [töö](http://msdn.microsoft.com/library/azure/hh974289.aspx) koosneb atomic toiminguid teha varade faili tegelik töö. 

Nagu enne mainitud, kui töötamise Azure Media Servicesi üks kõige levinumad stsenaariumid on pakkuda oma klientidele streaming kohandatava bitrate. Media Servicesi dünaamiliselt saate paketti failikogumi kohandatava bitrate MP4 ühte järgmistes vormingutes: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks. 

Dünaamiliste pakendit ära, peate tehke järgmist:

- kodeerida või transcode oma Standard (allikas) faili kohandatava bitrate MP4 või kohandatava bitrate sujuva voogesituse failide komplekt  
- vähemalt üks streaming ühiku saamiseks streaming lõpp-punkti, millest soovite esitada oma sisu. 

Järgmises jaotises kuvatakse töö, mis sisaldab ühe kodeering tööülesande loomise kohta. Tööülesande määrab Transcode Standard faili kohandatava bitrate MP4s abil **Media kodeerija Standard**komplekt. Jaotis kujutatakse ka töö töötlemine edenemise jälgimine. Kui töö on valmis, mida oleks võimalik luua lokaatorid, mida on vaja saada juurdepääsu oma varasid. 

### <a name="get-a-media-processor"></a>Saada meediumi protsessor

Media Servicesi, meedia protsessor on osa, mis tegeleb konkreetse töötlemise ülesannet, näiteks kodeerimine, vormingu teisendamisest, krüptimise või dekrüptimisel meediumi sisu. Näidatud selles õpetuses kodeering ülesande me kasutada Media kodeerija standardne.

Järgmine kood taotleb kodeerija kasutaja ID-d. 

**HTTP-päring**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-vastus**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Töö loomine

Iga töö võib olla üks või mitu tööülesannet sõltuvalt töötluse, mida soovite täita. REST API, kaudu saate luua oma seotud ülesannete ja on kaks võimalust: määratletud Tekstisisese atribuut tööülesanded liikumine töö üksustele või OData Pakktöötlus abil saab tööülesandeid. Media Services SDK kasutab Pakktöötlus. Siiski loetavuse koodi näited selles teemas, ülesanded on määratletud teksti sees. Pakktöötlus kohta leiate teavet teemast [Open Data protokolli (OData) Pakktöötlus](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Järgmises näites näidatakse, kuidas luua ja postita töö ühe tööülesande määramine kodeerida kindla eraldusvõime ja kvaliteediga video. Järgmises jaotises dokumendid sisaldab loendit, kõik [tööülesande valmiskombinatsioonid](http://msdn.microsoft.com/library/mt269960) Media kodeerija Standard protsessor ei toeta.  

**HTTP-päring**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-vastus**

Kui õnnestus, tagastatakse järgmised vastus:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


On mõned olulised asjad, mis tahes töö taotluse märkida.

- TaskBody atribuudid peab sõnasõnaline XML-i abil sisendi arvu määratlemine või väljundi varad, mida kasutatakse tööülesande. Tööülesande teema sisaldab XML-skeemi määratluse XML-i.
- TaskBody määratlus, iga sisemine väärtus <inputAsset> ja <outputAsset> peab olema seatud JobInputAsset(value) või JobOutputAsset(value).
- Tööülesande võib olla mitu väljundi varad. Ühe JobOutputAsset(x) saab kasutada ainult üks kord tööülesande töö väljund.
- Saate määrata ülesande Sisestuskeel aktiva JobInputAsset või JobOutputAsset.
- Tööülesannete peab ei moodusta tsükkel.
- Parameetri väärtus, mida te kaotate JobInputAsset või JobOutputAsset tähistab vara indeksi väärtus. Tegelik varad on määratletud InputMediaAssets ja OutputMediaAssets navigeerimise atribuudid töö üksuse määratlus. 

>[AZURE.NOTE] Kuna Media Servicesi on ehitatud OData v3, InputMediaAssets ja OutputMediaAssets navigeerimine atribuudi saidikogumid üksikute varade viidatakse kaudu on "__metadata: uri" nimi väärtus sidumiseks. 

- Ühe või mitme vara olete loonud Media Servicesi InputMediaAssets kaardid. OutputMediaAssets on loodud süsteem. Need viita olemasoleva vara.
- OutputMediaAssets saab nimeks assetName atribuudi abil. Kui see atribuut pole, siis selle OutputMediaAsset nimi on mis tahes sisemine teksti väärtus on <outputAsset> element on järelliite töö nimi väärtus või töö Id väärtus (juhuks, kui atribuudi nimi pole määratletud). Näiteks kui seate väärtus assetName "Proovi", siis OutputMediaAsset nime atribuudi peaks olema seatud "Valim". Kui te ei määra assetName väärtus, kuid ei seada töö nime "NewJob", siis OutputMediaAsset nimi oleks siiski "JobOutputAsset (väärtus) _NewJob". 

    Järgmises näites kujutatakse assetName atribuutide määramine:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Tööülesande Aheldamise lubamiseks tehke järgmist.

    - Töö peab olema vähemalt kaks tööülesanded
    - Peab olema vähemalt üks tööülesande nime, kelle sisend on ülesandega töö.

Lisateavet leiate teemast [loomise kodeerimine töö Media Services REST API-ga](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Töötlemine edenemise jälgimine

Saate tuua töö oleku olek atribuudi, nagu on näidatud järgmises näites. 

**HTTP-päring**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-vastus**

Kui õnnestus, tagastatakse järgmised vastus:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Katkestamine

Media Servicesi saate tühistada esitatava töö funktsiooni CancelJob kaudu. Selle kõne tagastab 400 tõrkekood, kui proovite kui seisu tühistatakse katkestamine, tühistamine, viga või lõpule jõudnud.

Järgmises näites kujutatakse CancelJob helistamine.


**HTTP-päring**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Kui õnnestus, tagastatakse 204 vastuse koodi pole sõnumi keha.

>[AZURE.NOTE] Peate URL-i kodeerida töö id (tavaliselt nb:jid:UUID: somevalue) läbides see parameetrina CancelJob.


### <a name="get-the-output-asset"></a>Väljundi varade hankimine 

Järgmine kood näitab, kuidas taotleda väljundi varade ID-ga. 


**HTTP-päring**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-vastus**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Avaldamine aktiva ja streaming hankimine ja järk-järgult allalaadimine URL-ide REST API-ga

Voogesituseks või vara alla laadida, peate esmalt "avaldada" see on locator loomisega. Lokaatorid pakuvad juurdepääsu vara failid. Media Services toetab kahte tüüpi lokaatorid: OnDemandOrigin lokaatorid, kasutada meediumide (nt MÕTTEKRIIPSU MPEG, HLS või sujuva voogesituse) ja lokaatorid Accessi allkirja (SAS), kasutatakse meediumide faile alla laadida.

Kui loote selle lokaatorid, saate koostada URL-id, mida kasutatakse voogesituseks või oma faile alla laadida. 


Sujuva voogesituse streaming URL on järgmises vormingus:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Streaming URL HLS on järgmises vormingus:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG MÕTTEKRIIPSU streaming URL on järgmises vormingus:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


SAS URL-i kasutatakse failide allalaadimiseks on järgmises vormingus:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Selles jaotises kirjeldatakse, kuidas teha järgmisi toiminguid vajalikud "avaldate" oma varasid.  

- Lugemisõigus on AccessPolicy loomine 
- Loomise SAS URL-i sisu allalaadimisel 
- Mõne origin URL-i loomise streaming sisu 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Lugemisõigus on AccessPolicy loomine

Enne allalaadimist või mis tahes meediumisisu voogesitus, esmalt määrata mõne AccessPolicy loetuks õigustega ja luua vastav Locator üksus, mis määrab soovite lubada oma klientidele kohaletoimetamise süsteemi tüüp. Saadaval atribuutide kohta leiate lisateavet teemast [AccessPolicy üksuse atribuudid](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

Järgmises näites on kujutatud loetuks õiguste osas antud varade AccessPolicy määramine.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Kui õnnestus, tagastatakse 201 edu koodi, mis kirjeldab AccessPolicy üksus, mille lõite. Seejärel saate AccessPolicy Id koos soovite luua Locator üksuse (nt vara väljundi) esitamisel faili sisaldava varade varade ID-d.

>[AZURE.NOTE]
Selle lihtsa töövoo on sama faili üleslaadimisel, kui söömisega vara (nagu käesolevas teemas). Nt failide üleslaadimine kui (või kliendid) on vaja kohe failidele juurde pääseda, määrata ka teie StartTime väärtus viis minutit enne praeguse kellaaja. See toiming on vaja, kuna võib olla kell skew klientseadmel Media Services. StartTime väärtus peab olema järgmises vormingus DateTime: YYYY-MM-DDTHH:mm:ssZ (nt "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>Loomise SAS URL-i sisu allalaadimisel 

Järgmine kood näitab, kuidas saada URL, mis saab luua ja üles laadida varem media faili alla laadida. Funktsiooni AccessPolicy on õiguste määramine lugemis- ja Locator tee viitab SAS alla URL.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Kui õnnestus, tagastatakse järgmised vastus:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


Atribuut tagastatud **tee** sisaldab SAS URL-i.

>[AZURE.NOTE]
Kui salvestusruumi krüptitud sisu alla laadida, peate käsitsi dekrüptida enne seda, või kasutada salvestusruumi dekrüptimine MediaProcessor töötlemine tööülesande väljundi töödeldud faili on OutputAsset selge ja selle alla laadida. Töötlemise kohta leiate lisateavet teemast Media Services REST API-ga on kodeerimine töökohtade loomine. Ka SAS URL-i lokaatorid ei saa värskendada, kui need on loodud. Näiteks ei saa kasutada sama Locator värskendatud StartTime väärtusega. Selle põhjuseks SAS URL-ide loomise viisi. Kui soovite juurdepääsu allalaadimiseks vara pärast soovitud Locator on aegunud, siis peate looma uue StartTime uue.

###<a name="download-files"></a>Failide allalaadimine

Kui olete AccessPolicy ja määrake Locator, võite alla laadida Azure'i salvestusruumi REST API-de abil.  

>[AZURE.NOTE] Peate lisama faili nimi faili, mida soovite alla laadida Locator **tee** väärtus, mis on saanud eelmises jaotises. Näiteks https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Azure'i salvestusruumi plekid töötamise kohta leiate lisateavet teemast [Bloobimälu teenuse REST API -ga](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Tulemusena kodeering töö tegite varasemates (kodeerimine kohandatava MP4 komplekt), on teil mitu MP4 faili, et järk-järgult saate alla laadida. Näiteks:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Sisu streaming streaming URL-i loomine


Järgmine kood näitab, kuidas luua streaming URL-i Locator:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Kui õnnestus, tagastatakse järgmised vastus:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Sujuva voogesituse origin URL streaming media Playeris voogesituseks tuleb lisada tee atribuudi nimi sujuva voogesituse näidata faili, millele järgneb "/ näidata".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS voogesituseks lisamine (vormingus = m3u8-aapl) pärast selle "/ näidata".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG MÕTTEKRIIPSU voogesituseks lisamine (vormingus = mpd-kellaaja-csf) pärast selle "/ näidata".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Sisu esitamine  

Kasutage voogesituseks saate video [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Järk-järgult allalaadimine testimiseks kleepige URL-i brauseri (nt IE, Chrome, Safari).


##<a name="next-steps-media-services-learning-paths"></a>Järgmised toimingud: Media Servicesi õppeteemad

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Kas otsite midagi muud?

Kui selle teema ei sisalda, mida te ei oodanud, pole midagi või mõnel muul viisil ei vasta teie vajadustele, anna meile tagasisidet, kasutades Disqus teemas allpool.



