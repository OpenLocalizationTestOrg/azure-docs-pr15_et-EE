<properties 
    pageTitle="Kasutades REST API Media Services kontoga ühenduse | Microsoft Azure'i" 
    description="See teema näitab, kuidas Media Servicesi žestid REST API ühendamiseks." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Ühenduse Media Services konto Media Services REST API kasutamine

> [AZURE.SELECTOR]
- [.NET-I](media-services-dotnet-connect-programmatically.md)
- [ÜLEJÄÄNUD](media-services-rest-connect-programmatically.md)

Selles teemas kirjeldatakse, kuidas hankida Microsoft Azure Media Servicesi programmiline ühendus on programmeeritud Media Services REST API-ga.

Kaks asja on nõutav Microsoft Azure Media Servicesi kasutamisel: juurdepääsu sümboolse esitatud Azure Access Control (ACS) ja Media Servicesi URI ise. Saate kasutada mis tahes viisil, mida soovite taotlused loomisel, kui määrate õige päise väärtused ja edastamiseks klõpsake juurdepääsu luba õigesti helistamisel Media Services.

Järgmistes juhistes kirjeldatakse levinumaid töövoo ühendamiseks Media Servicesi Media Services REST API kasutamisel:

1. Saada juurdepääsu sümboolse 
2. Ühenduse Media Servicesi URI 

    >[AZURE.NOTE] Pärast ühenduse loomist https://media.windows.net, saate määrata mõne muu Media Services URI 301 uuesti. Te peate järgmise kõned uue URI.
Võidakse kuvada ka HTTP/1.1 200 vastuse, mis sisaldab ODATA API metaandmete kirjeldus.

3. Pärast edaspidised API kõned uus URL. 

    Näiteks kui pärast ühenduse loomise katsel teil järgmist:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Tuleks postitada edaspidised API kõned https://wamsbayclus001rest-hs.cloudapp.net/api/.

##<a name="access-control-address"></a>Accessi juhtelemendi aadress

Media Servicesi juurdepääsu juhtimine aadress on https://wamsprodglobal001acs.accesscontrol.windows.net, välja arvatud Põhja-Hiinas piirkond, kus https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

##<a name="getting-an-access-token"></a>Saada juurdepääsu sümboolse

Meediumi teenustele juurdepääsuks otse REST API kaudu tuua juurdepääsu sümboolse ACS ja kasutada seda iga HTTP taotluse teete teenusesse ajal. See luba on sarnane teiste märkide ACS põhjal juurdepääsu taotluste esitatud päises HTTP-päring ja kasutab OAuthi v2 protokolli järgi. Teil pole vaja mõnda muud eeltingimused enne otse ühenduse Media Services.

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

Peate tõestamaks client_id ja client_secret väärtused kehas taotluse; client_id ja client_secret vastavad väärtused account_name ja AccountKey vastavalt. Need väärtused osutavad teile Media Servicesi konto häälestamisel. 

Pange tähele, et konto Media Servicesi AccountKey peab URL-kodeeringuga (vt [Protsentkodeering](http://tools.ietf.org/html/rfc3986#section-2.1) Accessi Turbeloa kutse väärtusena client_secret kasutamisel.

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Soovitatav on laiendatud "access_token" ja "expires_in" väärtusi vahemälu. Turbeloa andmeid hiljem olema vormilt talletamist ja Media Services REST API-kõnede uuesti kasutada. See on eriti kasulik stsenaariumid, kus luba saab turvaliselt jagada mitme protsesside või arvutite vahel.

Veenduge, et jälgida juurdepääsu luba väärtus "expires_in" ja värskendage oma REST API kõned koos uute märkide vastavalt vajadusele.

###<a name="connecting-to-the-media-services-uri"></a>Ühenduse Media Servicesi URI

Media Servicesi URI juur on https://media.windows.net/. Loote peaks algselt selle URI ja 301 uuesti tagasi sisse vastuse saamiseks peate järgmise kõned uue URI. Lisaks kasutada mis tahes automaatne-redirect/jälgi loogika oma taotlused. HTTP verbide ja taotluse asutused ei edastatakse uue URI.

Pöörake tähelepanu sellele, üles- ja allalaadimine varade failide URI juur https://yourstorageaccount.blob.core.windows.net/ kus salvestusruumikonto nimi on sama ühe oma Media Servicesi konto häälestamise ajal kasutasite.

Järgmises näites näitab Media Services (https://media.windows.net/) URI juur HTTP-päring. Kutse saab 301 uuesti tagasi vastuse. Järgmise taotluse kasutab uue URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-päring**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Kui saate uue URI, mis on URI, mida tuleks kasutada Media Servicesi suhelda. 


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
