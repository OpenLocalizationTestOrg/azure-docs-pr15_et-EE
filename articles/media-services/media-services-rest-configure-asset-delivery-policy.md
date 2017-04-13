<properties 
    pageTitle="Varade kohaletoimetamise poliitikate abil Media Services REST API konfigureerimise | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse, kuidas konfigureerida erinevate varade kohaletoimetamise poliitikate Media Services REST API abil." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Varade kohaletoimetamise poliitikate konfigureerimine

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Kui plaanite esitamisel dünaamiliselt krüptitud varad, üks Media Servicesi sisu kohaletoimetamise töövoo juhised konfigureerib kohaletoimetamise poliitika varad. Varade kohaletoimetamise poliitika ütleb Media Servicesi, kuidas soovite oma vara kohaletoimetamiseks: üheks mis streaming protokoll peaks teie vara olema dünaamiliselt pakitud (näiteks MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse või kõik), kas soovite oma varade dünaamiliselt krüptimiseks ja kuidas (ümbriku või levinud krüptimise).

Selles teemas käsitletakse miks ja kuidas luua ja varade kohaletoimetamise poliitikate konfigureerimine.

>[AZURE.NOTE]Saama kasutada dünaamilise pakendit ja dünaamiline krüptimist, tuleb teil teha kindel, et on vähemalt üks skaala ühik (tuntud ka kui streaming üksus). Lisateavet leiate teemast [skaala meediumid teenus](media-services-portal-manage-streaming-endpoints.md).
>
>Lisaks teie vara peab sisaldama kohandatava bitrate MP4s või kohandatava bitrate sujuva voogesituse failide kogum.

Erinevate poliitikate võivad rakendada sama vara. Näiteks võib rakendada sujuva voogesituse ja AES ümbriku krüptimise MPEG KRIIPSJOONE ja HLS PlayReady krüptimine. Mis tahes protokollid, mis on määratletud kohaletoimetamise poliitika (näiteks saate lisada ühe poliitika, mis määrab ainult HLS protokoll) streaming blokeeritud. Erandiks on, kui teil pole üldse määratud varade kohaletoimetamise poliitika. Klõpsake kõigi Protokollid lubatud selge.

Kui soovite pakkuda salvestusruumi krüptitud varade, tuleb konfigureerida vara kohaletoimetamise poliitika. Enne oma varade saate voona, streaming server eemaldab salvestusruumi krüptimise ja andmevoogu abil määratud kohaletoimetamise poliitika sisu. Näiteks teie vara krüptitud Advanced Standard (AES) ümbriku krüptovõtme osutamiseks seatud poliitika tüüp **DynamicEnvelopeEncryption**. Salvestusruumi krüptimise eemaldamine ja voona varade selge, tippige poliitika määramine **NoDynamicEncryption**. Näiteid, kuidas konfigureerida järgmist tüüpi poliitika jälgimine.

Sõltuvalt varade kohaletoimetamise poliitika konfigureerimise teil oleks võimalik dünaamiliselt paketti, dünaamiliselt krüptimiseks ja taustal alla laadida järgmistest streaming protokollid: sujuva voogesituse HLS, MPEG MÕTTEKRIIPSU ja HDS.

Järgmises loendis on näidatud vormingud abil sujuv, HLS, MÕTTEKRIIPSU ja HDS.

Sujuva voogesituse:

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG MÕTTEKRIIPSU

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HDS

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Juhised selle kohta, kuidas avaldada vara ja koostada streaming URL-i, lugege teemat [koostada streaming URL](media-services-deliver-streaming-content.md).


##<a name="considerations"></a>Kaalutlused

- Te ei saa kustutada ka AssetDeliveryPolicy seotud vara ajal kuvatakse nõudmisel (streaming) locator olemas vara. Soovitus on enne kustutamist poliitika vara poliitika eemaldamine.
- Streaming locator ei saa luua salvestusruumi krüptitud varade kui vara kohaletoimetamise poliitika on seatud.  Kui vara pole krüptitud salvestusruumi, süsteem võimaldab teil luua mõne locator ja voona varade selge ilma varade kohaletoimetamise poliitika.
- Teil võib olla mitu varade kohaletoimetamise poliitika, mis on seotud ühe varade, kuid saate määrata ainult üks viis käsitlema antud AssetDeliveryProtocol.  See tähendab, kui proovite kahe kohaletoimetamise poliitikate määratud AssetDeliveryProtocol.SmoothStreaming protokolli, mille tulemuseks on tõrge, kuna süsteemi ei tea, kus üks, milles soovite neid rakendada, kui mõnda muud klienti taotleb sujuva voogesituse link.
- Kui teil on mõne olemasoleva streaming locator vara, ei saa uue poliitika linkimine vara, olemasoleva poliitika kaudu vara linkimise tühistamine või värskendada vara kohaletoimetamise poliitika.  Peate esmalt eemaldamine streaming locator, muuta poliitikaid ja seejärel uuesti luua streaming locator.  Kui streaming locator uuesti luua, kuid veenduge, mis ei põhjustada probleeme klientidele, kuna sisu saab kopeerida päritolu või järgmise etapi CDN-ID, saate kasutada sama locatorId.

>[AZURE.NOTE] Kui töötate Media Services REST API-ga, kehtivad järgmisega.
>
>Üksuste Media teenuste kasutamisel peate seadma kindlate päise välju ja väärtuste HTTP-päringud. Kohta leiate teemast [Media Services REST API arengu häälestus](media-services-rest-how-to-use.md).

>Pärast ühenduse loomist https://media.windows.net, saate määrata mõne muu Media Services URI 301 uuesti. Te peate järgmise kõned uue URI, nagu on kirjeldatud [Media Servicesi kaudu REST API -ga ühenduse loomine](media-services-rest-connect-programmatically.md).


##<a name="clear-asset-delivery-policy"></a>Eemalda varade kohaletoimetamise poliitika

###<a id="create_asset_delivery_policy"></a>Varade kohaletoimetamise poliitika loomine
Järgmised HTTP-päringu loob varade kohaletoimetamise poliitika, mis täpsustab, ei kehti dünaamiline krüptimise ja pakkuda voo mis tahes järgmised protokollid: MPEG MÕTTEKRIIPSU, HLS ja sujuva voogesituse protokollid. 

Teavet Millised väärtused saate määrata ka AssetDeliveryPolicy loomisel, jaotisest [kasutada AssetDeliveryPolicy määratlemisel](#types) .   


Taotlemiseks tehke järgmist.
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Vastus:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Link vara varade kohaletoimetamise poliitika

Järgmised HTTP-päringu lingid määratud varade varade kohaletoimetamise poliitika.

Taotlemiseks tehke järgmist.

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Vastus:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption varade kohaletoimetamise poliitika 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Luua EnvelopeEncryption tüüpi sisu võti ja linkida selle vara

DynamicEnvelopeEncryption kohaletoimetamise poliitika määramisel peate veenduge, et teie vara linkimiseks EnvelopeEncryption tüüpi sisu klahvi. Lisateavet leiate teemast: [loomise sisu klahvi](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Kohaletoimetamise URL-i hankimine

Saate kohaletoimetamise URL-i sisu eelmises etapis loodud võtme määratud kohaletoimetusviis. Klientrakenduse tagastatud URL-i taotleda mõne AES klahv või mõne PlayReady litsentsi, et Taasesita kaitstud sisu.

Määrake URL-i hankimine HTTP-päringu sisu tüübi. Kui teil kaitsta oma sisu PlayReady, taotleda Media Services PlayReady litsentsi omandamise URL, kasutades 1 on keyDeliveryType: {"keyDeliveryType": 1}. Kui teil kaitsta oma sisu ümbriku krüptimise abil, taotleda võtme omandamise URL-i määramisega 2 keyDeliveryType: {"keyDeliveryType": 2}.

Taotlemiseks tehke järgmist.
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Vastus:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Varade kohaletoimetamise poliitika loomine

Järgmised HTTP-päringu loob **AssetDeliveryPolicy** , mis on konfigureeritud dünaamilise ümbriku krüptimise (**DynamicEnvelopeEncryption**) rakendamiseks **HLS** protokolli (selles näites teiste protokollidega blokeeritud streaming). 


Teavet Millised väärtused saate määrata ka AssetDeliveryPolicy loomisel, jaotisest [kasutada AssetDeliveryPolicy määratlemisel](#types) .   

Taotlemiseks tehke järgmist.

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Vastus:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Link vara varade kohaletoimetamise poliitika

Vt [Link varade varade kohaletoimetamise poliitika](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption varade kohaletoimetamise poliitika 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Luua CommonEncryption tüüpi sisu võti ja linkida selle vara

DynamicCommonEncryption kohaletoimetamise poliitika määramisel peate veenduge, et teie vara linkimine CommonEncryption tüüpi sisu klahvi. Lisateavet leiate teemast: [loomise sisu klahvi](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Kohaletoimetamise URL-i hankimine

Hankida PlayReady kohaletoimetusviis sisu eelmises etapis loodud võtme kohaletoimetamise URL. Taotlemiseks PlayReady litsentsi, et kaitstud sisu taasesitus kasutab mõnda muud klienti tagastatud URL. Lisateavet leiate teemast [Kohaletoimetamise URL-i hankimine](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Varade kohaletoimetamise poliitika loomine

Järgmised HTTP-päringu loob **AssetDeliveryPolicy** , mis on konfigureeritud dünaamiline levinud krüptimise (**DynamicCommonEncryption**) rakendamiseks **Sujuva voogesituse** protokolli (selles näites teiste protokollide blokeeritud streaming). 

Teavet Millised väärtused saate määrata ka AssetDeliveryPolicy loomisel, jaotisest [kasutada AssetDeliveryPolicy määratlemisel](#types) .   


Taotlemiseks tehke järgmist.

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Kui soovite oma Widevine DRM abil sisu kaitsta, värskendage AssetDeliveryConfiguration väärtused kasutada WidevineLicenseAcquisitionUrl (mis on 7 väärtus) ja määrake URL-i teenus litsentsi. Järgmised AMS partnerite abil saate aitavad teil esitada Widevine litsentsid: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Näiteks: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Krüptimisel Widevine, saate vaid saaks esitamisel SIDEKRIIPSU abil. Veenduge, et määrata varade kohaletoimetamise protokoll MÕTTEKRIIPSU (2).
  
###<a name="link-asset-with-asset-delivery-policy"></a>Link vara varade kohaletoimetamise poliitika

Vt [Link varade varade kohaletoimetamise poliitika](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Kasutatud AssetDeliveryPolicy määratlemisel tüübid

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
