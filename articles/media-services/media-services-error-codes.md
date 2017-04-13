<properties
    pageTitle="Azure Media Servicesi tõrkekoodid | Microsoft Azure'i"
    description="Teema annab ülevaate Azure Media Servicesi tõrkekoodid."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Azure Media Servicesi kuvatavad tõrkekoodid

Kui kasutate Microsoft Azure Media Servicesi, võidakse kuvada HTTP tõrkekoodid teenusest sõltuvalt näiteks autentimine märkide aeguv tegevusi, mis ei toeta Media Services. Järgmises loendis **HTTP tõrkekoodid** , mis võib tagastada Media Servicesi ja võimalike põhjuste neid.  
  
## <a name="400-bad-request"></a>400 vigane päring

Kutse sisaldab sobimatut teavet ja on tagasi lükatud ühel järgmistest põhjustest.

- API toetamata versioon on määratud. Uusimat versiooni teemast [Media Services REST API arengu häälestus](media-services-rest-how-to-use.md).
- Media Services API versioon on määramata. Kuidas määrata API versiooni kohta lisateabe saamiseks leiate teemast [ühenduse loomine Media Servicesi Media Services REST API-ga](media-services-rest-connect-programmatically.md). 
   
    >[AZURE.NOTE] Kui kasutate Media Servicesi ühenduse .NET või Java SDK-d, API versioon on määratud teile iga kord, kui proovite ja teatud toimingu Media Servicesi suhtes.
- On määramata atribuut pole määratud. Atribuudi nimi on kuvatakse tõrketeade. Saate määrata ainult atribuudid, mis on antud üksuse liikmed. Teemast [Azure Media Services REST API viide](http://msdn.microsoft.com/library/azure/hh973617.aspx) loendi üksuste ja nende atribuudid.
- On sobimatu atribuut väärtus on määratud. Atribuudi nimi on kuvatakse tõrketeade. Eelmise link lubatud atribuut tüüpide ja nende väärtuste nähtav.
- Atribuudi väärtus on puudu ja on nõutav.
- Määratud URL-i osa sisaldab vigane väärtus.
- Tehti katse WriteOnce atribuudi värskendamiseks.
- Katse on töö, mis on esmane AssetFile, mis on määramata või ei saa määrata Sisestuskeel vara loomiseks.
- Tehti katse SAS Locator värskendada. SAS lokaatorid saab loodud või kustutatud. Streaming lokaatorid saab värskendada. Lisateavet leiate teemast [lokaatorid](http://msdn.microsoft.com/library/azure/hh974308.aspx).
- Toetuseta toimingut või päringu edastati. 

## <a name="401-unauthorized"></a>401 puuduvad volitused

Kutse ei saa autentida (enne seda saab lubada) ühel järgmistest põhjustest:

- Puuduvate autentimise päis.
- Halb autentimise päise väärtus.
    - Luba on aegunud. Kui REST API-de otse Kasuta, lugege siit saate teada, kuidas luua uus autentimise luba [Media Servicesi Media Services REST API-ga ühenduse loomine](media-services-rest-connect_programmatically.md) . Kui kasutate .NET või Java SDK-d, luua CloudMediaContext või MediaContract objekti loomiseks luba. Lisateavet selle kohta, kuidas seda teha, leiate teemast [Media Services Media Services SDK .net-i ühenduse loomine](media-services-dotnet-connect-programmatically.md).
    - Luba sisaldab kehtetu allkiri.</li></ul></li></ul>

## <a name="403-forbidden"></a>403 keelatud

Taotluse pole lubatud ühel järgmistest põhjustest:

- Media Servicesi kontot ei leitud või on kustutatud.
- Media Servicesi konto on keelatud ja taotluse tüüp pole HTTP GET. Toimingud tulemuseks 403 vastuse ka.
- Autentimise luba ei sisalda kasutaja mandaat teave: account_name ja/või SubscriptionId. Media Services UI laiend selle teabe leiate haldusportaal Azure Media Servicesi konto.
- Ressursi ei pääse juurde.
    - Tehti katse kasutada MediaProcessor, mis pole teie Media Servicesi konto jaoks saadaval.
    - Tehti katse JobTemplate, mis on määratletud Media Servicesi värskendada.
    - Tehti katse mõne muu Media Servicesi konto Locator kirjutada.
    - Tehti katse mõne muu Media Servicesi konto ContentKey kirjutada.

- Ressurssi ei saa luua teenuse piirmäära, mis on vanusepiiri Media Servicesi konto tõttu. Teenuse kvootide kohta leiate lisateavet teemast [kvootide ja -piirangud](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 ei leitud

Taotluse pole lubatud ressurss ühel järgmistest põhjustest:

- Tehti katse värskendada üksus, mida pole olemas.
- Tehti katse Kustuta üksus, mida pole olemas.
- Tehti katse luua lingitud üksus, mida pole olemas.
- Tehti katse saada üksus, mida pole olemas.
- Tehti katse salvestusruumi konto, mis pole Media Servicesi kontoga seostatud määramiseks.  

## <a name="409-conflict"></a>409 konflikti

Taotluse pole lubatud ühel järgmistest põhjustest:

- Rohkem kui üks AssetFile on määratud nimi vara sees.
- Tehti katse luua teine esmane AssetFile vara sees.
- Katse on juba kasutusel määratud Id-ga on ContentKey loomiseks.
- Katse on juba kasutusel määratud Id-ga on Locator loomiseks.
- Rohkem kui üks IngestManifestFile on määratud nimi on IngestManifest sees.
- Tehti katse salvestusruumi krüptitud varade teise salvestusruumi krüptimise ContentKey linkida.
- Katse on sama ContentKey vara linkida.
- Tehti katse on locator vara kelle salvestusruumi container on puudu või ei ole enam seostatud vara loomiseks.
- Tehti katse on locator, mis on juba kasutusel 5 lokaatorid varade loomiseks. (Azure'i salvestusruumi jõustab viis ühiskasutusega juurdepääsu poliitika ühe salvestusruumi container limiit).
- Vara salvestusruumi konto linkimine mõne IngestManifestAsset pole sama, mis kasutavad ema IngestManifest salvestusruumi konto.  

## <a name="500-internal-server-error"></a>500 Sisemine serveritõrge

Taotluse töötlemise ajal tekib Media Servicesi viga, mis takistab töötlemise jätkamist. Põhjuseks võib olla üks järgmistest põhjustest:

- Vara või töö loomine nurjub, kuna Media Servicesi konto teenuseteabe piirmäär pole ajutiselt saadaval.
- Mõne vara või IngestManifest bloobimälu salvestusruumi container loomine nurjub, kuna selle konto salvestusruumi kontoteave pole ajutiselt saadaval.
- Muud ootamatu tõrge. 

## <a name="503-service-unavailable"></a>503 teenus pole saadaval

Server ei saa praegu saada kutsed. See tõrge võib olla põhjustatud teenust liigne taotluste. Media Servicesi pidurdamise süsteem piirab ressursikasutuse rakenduste teenust liigne taotlus tehtud.

>[AZURE.NOTE]Tõrketeade ja tõrge koodi stringi saada täpsemat teavet põhjus, miks teil tõrke 503 kontrollida. See tõrge tähendab alati pidurdamise.

On võimalik olek kirjeldused.

- "Server on hõivatud. Seda tüüpi taotluse eelmise käivitatakse võttis rohkem kui {0} sekundit."
- "Server on hõivatud. Rohkem kui {0} taotlusi sekundis võib olla rakendus."
- "Server on hõivatud. Rohkem kui {0} taotlused {1} sekunditega saate rakendus."

Toime selle vea, soovitame kasutada eksponentsiaalse tagasi välja uuesti loogika. See tähendab, et järk-järgult enam järjestikust tõrge vastuseid korduste vahel ootab abil.  Lisateabe saamiseks vt [Siirdamiseks viga töötlemise rakenduse blokeerimine](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE]Kui kasutate [Azure Media Services SDK .net-i](https://github.com/Azure/azure-sdk-for-media-services/tree/master), on rakendatud SDK uuesti loogika 503 tõrke.  
  
## <a name="see-also"></a>Vt ka  

[Teenuste haldus kuvatavad tõrkekoodid](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
