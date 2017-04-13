<properties 
    pageTitle="Media Services REST API ülevaade | Microsoft Azure'i" 
    description="Media Services REST API ülevaade" 
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
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Media Services REST API ülevaade 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure Media Servicesi on teenus, mis aktsepteerib OData-põhiste HTTP päringuid ja uuesti sisse, kui Paljusõnaline JSON või Atomi + pub saate vastata. Kuna Media Servicesi vastavad juhised Azure kujundus, on nõutav iga kliendi tuleb kasutada Media Servicesi ühendamisel HTTP-päiste kogum, samuti kogumi valikuline päised, mida saab kasutada. Järgmistes jaotistes kirjeldatakse päiste ja HTTP verbide abil saate luua taotlusi ja vastuste saamine Media Services.

##<a name="considerations"></a>Kaalutlused 

Järgmisega rakendada ülejäänud kasutamisel.

- Kui päringu üksused, on piirang 1000 üksuste tagasi korraga, sest avaliku ülejäänud v2 piirab päringutulemite 1000 tulemusi. Peate kasutama **Jäta** ja **võtta** (.NET) / [.net-i näites](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ja [REST API näites](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)kirjeldatud **top** (REST) nimega. 

- Kui kasutades JSON ja täpsustades taotluse **__metadata** märksõna kasutamine (näiteks, et see viitab lingitud objekt) peate määrama **Aktsepteeri** päise [Paljusõnaline JSON](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) -vormingus (vt järgmist näidet). OData aru ei saa atribuudi **__metadata** taotluse, v.a juhul, kui seate selle Paljusõnaline.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standardse HTTP taotluse päised ei toeta Media Services

Iga kõne ajal saate teha Media Services on nõutav päised, peate lisama oma taotluse kogumi ja ka kogum, valikuline päised soovite kaasata. Järgmises tabelis on loetletud nõutav päised:


Päis|Tüüp|Väärtus
---|---|---
Luba|Esitaja|Esitaja on ainus aktsepteeritud autoriseerimine süsteem. Väärtus peab sisaldama ka esitatud ACS juurdepääsu luba.
x-ms-versioon|Decimal|2.11
DataServiceVersion|Decimal|3.0
MaxDataServiceVersion|Decimal|3.0



>[AZURE.NOTE] Kuna Media Services kasutab OData jätke selle aluseks oleva vara metaandmete hoidla REST API-de kaudu, DataServiceVersion ja MaxDataServiceVersion päiste tuleks lisada mis tahes taotluse; juhul, kui pole, siis praegu Media Servicesi eeldab DataServiceVersion väärtuse kasutamine on 3.0.

Kogum, valikuline päised on:

Päis|Tüüp|Väärtus
---|---|---
Kuupäev|RFC 1123 kuupäev|Ajatempli taotluse
Aktsepteerige|Sisutüüp|Nõutud sisutüübi vastus näiteks järgmine:<p> -rakenduse/json; odata = Paljusõnaline<p> -rakenduse/atom + XML-i<p> Vastuste võib olla erinevad sisutüübi, nt bloobimälu toomise, kus eduka vastus sisaldab bloobimälu voo nimega soovitud last.
Aktsepteerige kodeerimine|Gzip, deflate|GZIP ja DEFLATE kodeerimine, võimaluse korral. Märkus: Suurte ressursid, Media Servicesi võib ignoreerib seda päist ja tagastada mittetihendatud andmeid.
Aktsepteerige keel|"en", "d" ja jne.|Saate määrata vastuse eelistatud keel.
Aktsepteerige märgistik|Märgistik tüüp, näiteks "UTF-8"|Vaikimisi on UTF-8.
X-HTTP-meetod|HTTP-meetod|Võimaldab kliendid või tulemüürid, mis ei toeta HTTP meetodid, nt pane või Kustuta nende meetodite, tunneled toomine kõne kaudu.
Sisutüüp|Sisutüüp|Sisutüübi koosolekukutse kehas panna või POSTITUSSE taotleb.
KliendiID taotlus|String|Helistaja määratletud väärtus, mis määrab antud kutse. Kui määratud, kaasatakse see väärtus vastus kirja viis taotluse vastendamiseks. <p><p>**Oluliste**<p>Väärtused tuleks ülempiir 2096b (2k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standardse HTTP vastus päised ei toeta Media Services

Järgmine on päised, mis võib tagastada teile sõltuvalt on teil paluda ressurss ja toimingu sooritamiseks soovitud kogum.


Päis|Tüüp|Väärtus
---|---|---
taotluse-id|String|Praeguse toimingu, teenuse loodud ainuidentifikaator.
KliendiID taotlus|String|Identifikaatori määratud helistaja taotlus, kui see on olemas.
Kuupäev|RFC 1123 kuupäev|Taotluse töötlemise kuupäev.
Sisutüüp|Muutub|Sisutüübi vastuse keha.
Sisu kodeerimine|Muutub|Gzip või deflate vastavalt vajadusele.


## <a name="standard-http-verbs-supported-by-media-services"></a>Standardse HTTP verbide Media Services ei toeta

Järgmine on HTTP verbide, mida saab teha HTTP taotleb täieliku loendi.


Tegusõna|Kirjeldus
---|---
HANKIMINE|Tagastab praeguse väärtuse objekti.
POSTITUSE|Loob objekti, mis on esitatud andmete põhjal või käsk esitab.
SELLELE|Asendab objekti või loob nimega objekti (kui see on rakendatav).
KUSTUTAMINE|Objekti kustutamine
KIRJAKOOSTE|Värskendab olemasoleva objekti nimega atribuutides tehtud muudatuste.
JUHI|Tagastab metaandmete objekti toomine vastuse ootamine.

##<a name="limitation"></a>Piirangud

Kui päringu üksused, on piirang 1000 üksuste tagasi korraga, sest avaliku ülejäänud v2 piirab päringutulemite 1000 tulemusi. Peate kasutama **Jäta** ja **võtta** (.NET) / [.net-i näites](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ja [REST API näites](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)kirjeldatud **top** (REST) nimega. 


## <a name="discovering-media-services-model"></a>Media Servicesi mudeli avastamine

Media Servicesi üksused veel hõlpsamaks, saab kasutada $metadata toiming. See võimaldab teil laadida kõik kehtiv üksuse tüüpi, üksuse atribuutide, ühendused, funktsioonide, toiminguid ja jne. Järgmises näites kujutatakse ehitada URI: https://media.windows.net/API/ $metadata.

Mida tuleks lisada "? api-version=2.x" URI, kui soovite vaadata metaandmete brauseris või ei sisalda päise x-ms-versioon teie taotlus lõpuni.



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 
