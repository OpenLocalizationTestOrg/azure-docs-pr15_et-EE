<properties 
    pageTitle="Kuidas kodeerida abil Media kodeerija Standard vara | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Media kodeerija Standard kodeerida meediumisisu Media Services. Koodinäiteid kasutada REST API-ga." 
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


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Kuidas kodeerida abil Media kodeerija Standard vara


> [AZURE.SELECTOR]
- [.NET-I](media-services-dotnet-encode-with-media-encoder-standard.md)
- [ÜLEJÄÄNUD](media-services-rest-encode-asset.md)
- [Portaal](media-services-portal-encode.md)

##<a name="overview"></a>Ülevaade
Digitaalse video osutamiseks Interneti kaudu peab on meediumi tihendamine. Digitaalse videofailid on üsna suur ja võib olla liiga suur esitamisel Internetis või õigesti kuvamiseks klientide seadmete jaoks. Kodeerimine on tihendamise video ja heli, et kliendid saaksid vaadata meediumifailide protsess.

Kodeering tööde haldamine on üks kõige levinum töötlemise Media Services. Saate luua kodeering töökohta kodeeringus teise meediumifailide teisendada. Kui te kodeerida, saate kasutada Media Servicesi sisseehitatud kodeerija (Media kodeerija standardne). Saate kasutada ka esitatud partnerilt Media Services kodeerija kolmanda osapoole kodeerimisseadmetest on saadaval Azure'i turuplatsi kaudu. Üksikasju kodeerimine ülesanded, kasutades oma kodeerija jaoks määratletud eelseadistatud stringide või eelseadistatud konfiguratsiooni failid abil saate määrata. Tüüpi valmiskombinatsioonid, mis on saadaval, leiate teemast [Tööülesande valmiskombinatsioonid Media kodeerija Standard jaoks](http://msdn.microsoft.com/library/mt269960).

Iga töö võib olla üks või mitu tööülesannet sõltuvalt töötluse, mida soovite täita. REST API, kaudu saate luua oma seotud ülesannete ja on kaks võimalust:

- Tööülesannete võib olla määratletud Tekstisisese kaudu atribuut tööülesanded liikumine töö üksustele, või
- kaudu OData Pakktöötlus.


Soovitatav on alati kodeerida Standard failide on kohandatava bitrate MP4 seadmine ja seejärel teisendage määramine [Dünaamilise pakendit](media-services-dynamic-packaging-overview.md)abil soovitud vorming. Dünaamiliste pakendit ära, peate esmalt saada vähemalt üks tellitav streaming ühiku streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu. Lisateavet leiate teemast [skaala Media Services](media-services-portal-manage-streaming-endpoints.md).

Kui teie väljundi vara on krüptitud salvestusruumi, tuleb konfigureerida varade kohaletoimetamise poliitika. Lisateabe saamiseks vt [seadistamine varade kohaletoimetamise poliitika](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE]Enne alustamist, et viitamine meediumi protsessorite, veenduge, et on õige meediumi protsessor ID-ga. Lisateabe saamiseks vt [Meediumi protsessorite saada](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Töö kodeering ühe tööülesande loomine

>[AZURE.NOTE] Kui töötate Media Services REST API-ga, kehtivad järgmisega.
>
>Üksuste Media teenuste kasutamisel peate seadma kindlate päise välju ja väärtuste HTTP-päringud. Kohta leiate teemast [Media Services REST API arengu häälestus](media-services-rest-how-to-use.md).

>Pärast ühenduse loomist https://media.windows.net, saate määrata mõne muu Media Services URI 301 uuesti. Te peate järgmise kõned uue URI, nagu on kirjeldatud [Media Servicesi kaudu REST API -ga ühenduse loomine](media-services-rest-connect-programmatically.md).
>
>Kui kasutades JSON ja täpsustades taotluse **__metadata** märksõna kasutamine (näiteks, et see viitab lingitud objekt) peate määrama **Aktsepteeri** päise [Paljusõnaline JSON](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)-vormingus: Aktsepteeri: rakenduse/json; odata = Paljusõnaline.

Järgmises näites näidatakse, kuidas luua ja postita töö ühe tööülesande määramine kodeerida kindla eraldusvõime ja kvaliteediga video. Kui kodeeringus koos Media kodeerija Standard, saate kasutada määratud tööülesande konfiguratsiooni valmiskombinatsioonid [siin](http://msdn.microsoft.com/library/mt269960).

Taotlemiseks tehke järgmist.

POSTITUSE https://media.windows.net/API/Jobs HTTP/1.1 sisutüüp: rakenduse/json; odata = Paljusõnaline Aktsepteeri: rakenduse/json; odata = Paljusõnaline DataServiceVersion: 3.0 MaxDataServiceVersion: 3.0 x-ms-versioon: 2.11 autoriseerimine: esitaja <token value> 
 x-ms-kliendi-taotlus-id: 00000000-0000-0000-0000-000000000000 Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Vastus:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Määrake väljundi vara nimi

Järgmises näites kujutatakse assetName atribuutide määramine:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Kaalutlused

- TaskBody atribuudid peab literaalstringide XML-i abil sisendi arvu määratlemine või väljundi varad, mis kasutab tööülesanne. Tööülesande teema sisaldab XML-skeemi määratluse XML-i.
- TaskBody määratlus, iga sisemine väärtus <inputAsset> ja <outputAsset> peab olema seatud JobInputAsset(value) või JobOutputAsset(value).
- Tööülesande võib olla mitu väljundi varad. Ühe JobOutputAsset(x) saab kasutada ainult üks kord tööülesande töö väljund.
- Saate määrata ülesande Sisestuskeel aktiva JobInputAsset või JobOutputAsset.
- Tööülesannete peab ei moodusta tsükli.
- Parameetri väärtus, mida te kaotate JobInputAsset või JobOutputAsset tähistab vara indeksi väärtus. Tegelik varad on määratletud InputMediaAssets ja OutputMediaAssets navigeerimise atribuudid töö üksuse määratlus. 
- Kuna Media Servicesi on ehitatud OData v3, InputMediaAssets ja OutputMediaAssets navigeerimine atribuudi saidikogumid üksikute varade viidatakse kaudu on "__metadata: uri" nimi väärtus sidumiseks.
- Ühe või mitme vara olete loonud Media Servicesi InputMediaAssets kaardid. OutputMediaAssets on loodud süsteem. Need viita olemasoleva vara.
- OutputMediaAssets saab nimeks assetName atribuudi abil. Kui see atribuut pole, siis on nimi kuvatakse OutputMediaAsset eksklusiivses sisemine teksti väärtus on <outputAsset> element on järelliite töö nimi väärtus või töö Id väärtus (juhuks, kui atribuudi nimi pole määratletud). Näiteks kui seate väärtus assetName "Proovi", siis OutputMediaAsset nime atribuudi peaks olema seatud "Valim". Kui te ei määra assetName väärtus, kuid ei seada töö nime "NewJob", siis OutputMediaAsset nimi oleks siiski "JobOutputAsset (väärtus) _NewJob". 


##<a name="create-a-job-with-chained-tasks"></a>Töö ülesannetega aheldatud loomine

Tööiga rakenduste arendajatele soovite sarja töötlemine tööülesannete loomine. Media Services, saate luua aheldatud tööülesannete sarja. Iga tööülesande sooritab muu töötlemine juhiseid ja saate kasutada eri meediumide protsessorite. Aheldatud tööülesanded saate käsitsi välja vara kaudu ühe tööülesande teise vara lineaarse jada tööülesandeid täita. Projekti ülesannete ei peavad olema järjestuses. Aheldatud tööülesande loomisel luuakse **IJob** üksikobjektiks aheldatud **ITask** objektid.

>[AZURE.NOTE] Praegu on piiratud 30 ülesannete kohta. Kui vajate rohkem kui 30 tööülesannete keti, luua rohkem kui ühe töö sisaldama tööülesanded.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Kaalutlused

Tööülesande Aheldamise lubamiseks tehke järgmist.

- Töö peab olema vähemalt 2 tööülesanded
- Peab olema vähemalt üks tööülesande nime, kelle sisend on ülesandega töö.

## <a name="use-odata-batch-processing"></a>OData Pakktöötlus kasutamine 

Järgmises näites kujutatakse OData Pakktöötlus töö ja ülesannete loomiseks kasutada. Pakktöötlus kohta leiate teavet teemast [Open Data protokolli (OData) Pakktöötlus](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Looge abil soovitud JobTemplate töö


Mitme levinud määratud tööülesannete varad käsitlemisel JobTemplates on kasulikud määrata vaikimisi tööülesande valmiskombinatsioonid, tööülesannete, järjestust ja muudeks tegevusteks.

Järgmises näites kujutatakse TaskTemplate määratletud tekstisisene lisamine JobTemplate loomiseks. Funktsiooni TaskTemplate kasutab Media kodeerija Standardne soovitud MediaProcessor kodeerida varade faili; Siiski saab kasutada ka muid MediaProcessors. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]Erinevalt Media Servicesi muid üksusi, tuleb määratleda uute GUID identifikaator iga TaskTemplate ja paigutamine taskTemplateId ja Id atribuudi koosolekukutse kehas. Sisu tuvastamine värviskeemi järgima kirjeldatud tuvastamine Azure Media Services üksuste kava. Samuti ei saa värskendada JobTemplates. Selle asemel, peate looma uue värskendatud muudatused.
 

Kui õnnestub, tagastatakse järgmised vastus:
    
    HTTP/1.1 201 Created
    
    . . .


Järgmises näites on kujutatud viitamine JobTemplate Id töö loomise kohta:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Kui õnnestus, tagastatakse järgmised vastus:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Järgmised sammud
Nüüd, kui teate, kuidas luua töö, mida soovite kodeerida mõne assset, minge teemas [Kuidas soovite kontrollida töö edenemise Media teenustega](media-services-rest-check-job-progress.md) .


##<a name="see-also"></a>Vt ka

[Saada meediumi protsessorid](media-services-rest-get-media-processor.md)
