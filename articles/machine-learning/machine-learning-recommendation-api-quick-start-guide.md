<properties 
    pageTitle="Lühijuhend: seadme õ soovitused API | Microsoft Azure'i" 
    description="Azure'i masina Õppekeskuse soovitused - Lühijuhend" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Seadme õ soovitusi API Lühijuhend

>[AZURE.NOTE]Soovitused API kognitiivse teenuse peaks kasutuselevõtt, selle asemel, et see versioon. Soovitused kognitiivse teenus on asendades selle teenuse ja seal arendatakse uusi funktsioone. See on uue võimalused nagu partiide tugi, parem API Explorer, cleaner API Surface'i, ühtsema registreerimine/arveldamine kogemus jne.
> Lisateavet [uue kognitiivse teenuse migreerimine](http://aka.ms/recomigrate)


Dokumendis kirjeldatakse, kuidas endal oma teenuse või rakendust Microsoft Azure seadme õ soovitusi. Soovitused API rohkem üksikasju leiate [Galerii](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Ülevaade

Kasutage Azure seadme õ soovitusi, peate toimige järgmiselt:

* Andmemudeli loomine – mudeli on teie andmeid, kataloogi andmed ja mudel soovitus.
* Andmete importimine kataloogi - kataloogiga sisaldab metaandmete üksused. 
* Andmete importimine kasutus - kasutusandmete saab ühel viisil 2 (või mõlemad) üles laadida:
    * Kasutus andmeid sisaldava faili üleslaadimisel.
    * Saates andmeid tuua sündmused.
    Tavaliselt saadate kasutus faili, et luua mudelit, mis algse soovitus (bootstrap) ja seda kasutada kuni süsteemi kogutakse piisavalt andmeid tuua andmevorming abil.
* Luua soovitus mudel – see on asünkroonne toiming, kus soovitus süsteemi võtab kasutamine andmete ja loob mudeli soovitus. See toiming võib võtta mitu minutit või mitu tundi suurusest andmed ja konfiguratsiooni parameetrid koostamine. Kui käivitamise koostamine, saate Koosta ID-ga. Selle abil saate kontrollida, kui Koosta protsess on lõppenud enne alustamist kasutamine soovitusi.
* Soovitused - Get kindla üksuse või üksuste loendi jaoks kasutada.

Ülaltoodud juhiseid on tehtud Azure'i masina õ soovituste API kaudu.  Saate alla laadida valimi rakendus, mis rakendab iga need juhised on [kasutada ka Galerii.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Piirangud

* Suurim arv tellimuse kohta mudelite on 10.
* Üksused, mille kataloogiga mahub maksimumarv on 100 000.
* Kasutus punkte, mis on maksimaalne arv on ~ 5 000 000. Vanimast kustutatakse uusi faile üles laadida või teatatud.
* Andmed, mida saab saata postitusse (nt kataloogi andmete importimine, andmete importimine kasutus) maksimaalne maht on 200MB.
* Soovitused mudeli koostamine, mis pole aktiivne sekundis tehingute arv on ~ 2TPS. Soovitused mudeli koostamine, mis on aktiivne mahub-20TPS.

##<a name="integration"></a>Integreerimine

###<a name="authentication"></a>Autentimine
Micosoft Azure'i turuplatsi toetab Basic või OAuthi autentimise meetodit. Saate kerge vaevaga konto klahvid navigeerimise klahvid turuplatsil jaotises [konto sätted](https://datamarket.azure.com/account/keys). 
####<a name="basic-authentication"></a>Lihttekstautentimine
Luba päise lisamiseks tehke järgmist.

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Teisendamine Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Teisendamine Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Teenuse URI
Teenuse Azure seadme õ soovitused API URI juur on [siin.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Täielik teenuse URI on väljendatud elemendid OData määratlus.

###<a name="api-version"></a>API versioon
Iga API kõne on lõpus, nimetatakse apiVersion, mis peaks olema seatud "1.0" Päringuparameetri.

###<a name="ids-are-case-sensitive"></a>ID-d on tõstutundlik
ID-d, mis tahes API-d, tagastatud on tõstutundlikud ja tuleks kasutada sellisena, kui nimega parameetrite edaspidised API kõned vastu. Näiteks mudeli ID-d ja kataloogi ID-d on tõstutundlikud.

###<a name="create-a-model"></a>Andmemudeli loomine
"Andmemudeli loomine" taotluse loomine:

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelName   |   Ainult tähti (A – Z, a – z) arvud (0 – 9) sidekriipsu (-) ja allkriipsu (_) on lubatud.<br>Max pikkus: 20 |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |


**Vastus**:

HTTP olekukoodi: 200

- `feed/entry/content/properties/id`-Sisaldab mudeli ID-ga.
**Märkus**: mudeli ID on tõstutundlikud.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Andmete importimine kataloogi

Kui mitu kataloogi failide üleslaadimine mitu kõned sama mudel, me lisatakse ainult kataloogi uued üksused. Olemasolevate üksuste jäävad algsed väärtused.

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli (tõstutundlik) ainuidentifikaator  |
| faili nimi | Teksti identifikaator kataloogi.<br>Ainult tähti (A – Z, a – z) arvud (0 – 9) sidekriipsu (-) ja allkriipsu (_) on lubatud.<br>Max pikkus: 50 |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | Kataloogi andmed. Vorming:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Nimi</th><th>Kohustusliku</th><th>Tüüp</th><th>Kirjeldus</th></tr><tr><td>Üksuse Id</td><td>Jah</td><td>Tähtedest ja numbritest koosnev, max pikkus 50</td><td>Üksuse ainuidentifikaator</td></tr><tr><td>Üksuse nimi</td><td>Jah</td><td>Tähtedest ja numbritest koosnev, max pikkus 255</td><td>Üksuse nimi</td></tr><tr><td>Üksuse kategooria</td><td>Jah</td><td>Tähtedest ja numbritest koosnev, max pikkus 255</td><td>Kategooriaga seotud selle üksuse (nt Cooking raamatud, draama...)</td></tr><tr><td>Kirjeldus</td><td>Ei</td><td>Tähtedest ja numbritest koosnev, max pikkus 4000</td><td>Selle üksuse kirjeldus</td></tr></table><br>Faili maht on 200MB.<br><br>Näide:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, aadressiraamatust<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, unusta ruumi: fiktsiooni (Byzantium raamat), broneerida<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, aadressiraamatust<br>552a1940-21e4-4399-82bb-594b46d7ed54, loomad aadressiraamatu liikumise piiramine</pre> |


**Vastus**:

HTTP olekukoodi: 200

- `Feed\entry\content\properties\LineCount`-Aktsepteeritud ridade arv.
- `Feed\entry\content\properties\ErrorCount`-Pole tõrke tõttu lisatud ridade arv.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Andmete importimine kasutus

####<a name="uploading-a-file"></a>Faili üleslaadimine
Selles jaotises kirjeldatakse, kuidas kasutusandmete abil faili üles laadida. Saate helistada selle API mitu korda kasutuse andmetega. Kõik kasutusandmete salvestatakse kõik kõnede jaoks.

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli (tõstutundlik) ainuidentifikaator |
| faili nimi | Teksti identifikaator kataloogi.<br>Ainult tähti (A – Z, a – z) arvud (0 – 9) sidekriipsu (-) ja allkriipsu (_) on lubatud.<br>Max pikkus: 50 |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | Andmete kasutamine. Vorming:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Nimi</th><th>Kohustusliku</th><th>Tüüp</th><th>Kirjeldus</th></tr><tr><td>Kasutaja-Id</td><td>Jah</td><td>Tähtedest ja numbritest koosnev</td><td>Kasutaja ainuidentifikaator</td></tr><tr><td>Üksuse Id</td><td>Jah</td><td>Tähtedest ja numbritest koosnev, max pikkus 50</td><td>Üksuse ainuidentifikaator</td></tr><tr><td>Aeg</td><td>Ei</td><td>Kuupäev vormingus: YYYY/MM/DDTHH:MM:SS (nt 06 2013/20T10:00:00)</td><td>Aja andmed</td></tr><tr><td>Sündmuse</td><td>Ei, kui see on esitatud, siis peate ka sellele kuupäeva</td><td>Ühte järgmistest:<br>• Klõpsake<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Ostmine</td><td></td></tr></table><br>Faili maht on 200MB.<br><br>Näide:<br><pre>149452 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Vastus**:

HTTP olekukoodi: 200

- `Feed\entry\content\properties\LineCount`-Aktsepteeritud ridade arv.
- `Feed\entry\content\properties\ErrorCount`-Pole tõrke tõttu lisatud ridade arv.
- `Feed\entry\content\properties\FileId`-Identifikaator fail.


OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Abil andmete kogumine
Selles jaotises kirjeldatakse, kuidas saata sündmuste reaalajas Azure seadme õ soovitusi, tavaliselt veebisaidilt.

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Koosolekukutse kehasse.| Iga sündmuse juhul andmesisestuse, mida soovite saata. Saatke sama brauserit või kasutaja seansi jaoks sama ID SeansiId väli. (Vt valimi sündmuse asutuse allpool.)|


- Sündmuse 'Klõpsa' näide:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Näide sündmuse "RecommendationClick":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Näide sündmuse "AddShopCart":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Näide sündmuse "RemoveShopCart":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Sündmuse 'Ostmine' näide:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Näide 2 sündmusi, klõpsake"ja"AddShopCart"saatmine:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Vastus**: HTTP olekukoodi: 200

###<a name="build-a-recommendation-model"></a>Soovitused mudeli koostamine

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli (tõstutundlik) ainuidentifikaator  |
| userDescription | Teksti identifikaator kataloogi. Pange tähele, et kui kasutate tühikuid saate peab kodeerida selle 20% selle asemel. Ülaltoodud näites näha.<br>Max pikkus: 50 |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

See on asünkroonne API. Saate koostamine ID vastus. Teada, millal koostamine on lõppenud, peaks kõne "Saada koostab olek on ka mudel" API ja otsige üles see koostamine ID vastuse. Arvestage, et ehitada saavad võtta minutit tundi sõltuvalt andmete maht.

Te ei saa kasutada kuni koostamine soovitused lõpeb.

Lubatud Koosta olek:

- Looge – mudel.
- Ootele – käivitati mudeli koostamine ja see on ootele.
- Ehitatakse Building – mudel.
- Edu-koostamine lõppes edukalt.
- Tõrge – koostamine lõppes tõrke.
- Tühistatud – koostamine on tühistatud.
- Tühistamine – Koosta tühistatakse.


Pange tähele, et koostamine ID leiate jaotises tee järgmist:`Feed\entry\content\properties\Id`

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Hankige mudeli koostamine

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId         |   Kui seate mudeli (tõstutundlik) ainuidentifikaator    |
|   onlyLastBuild   |   Näitab, kas Koosta ajalugu mudeli või ainult viimase koostamine olekut. |
|   apiVersion      |   1.0                                 |


**Vastus**:

HTTP olekukoodi: 200

Vastus sisaldab ühe kande koostamine. Iga kirje on järgmised andmed.

- `feed/entry/content/properties/UserName`– Kasutaja nimi.
- `feed/entry/content/properties/ModelName`– Mudeli nimi.
- `feed/entry/content/properties/ModelId`– Mudel kordumatut tunnust.
- `feed/entry/content/properties/IsDeployed`– Kas juurutatakse (alias koostamine aktiivse järk).
- `feed/entry/content/properties/BuildId`– Koostada kordumatut tunnust.
- `feed/entry/content/properties/BuildType`-Tüüpi koostamine.
- `feed/entry/content/properties/Status`– Koostada olek. Võib olla üks järgmistest: tõrke, hoone, ootel, Cancelling, tühistatud, edu
- `feed/entry/content/properties/StatusMessage`– Üksikasjalik olekuteade (kehtib ainult teatud olekus).
- `feed/entry/content/properties/Progress`– Koostada edenemist (%).
- `feed/entry/content/properties/StartTime`– Koostada algusaeg.
- `feed/entry/content/properties/EndTime`– Koostada lõppkellaaeg.
- `feed/entry/content/properties/ExecutionTime`– Koostada kestus.
- `feed/entry/content/properties/ProgressStep`– Praeguse etapi, mis on pooleli ehitada üksikasjad.

Lubatud Koosta olek:
- Loodud – Koosta taotlus kirje on loodud.
- Ootele – käivitati Koosta taotlus ja see on ootele.
- Koosteüksuse – koostamine on pooleli.
- Edu-koostamine lõppes edukalt.
- Tõrge – koostamine lõppes tõrke.
- Tühistatud – koostamine on tühistatud.
- Tühistamine – Koosta tühistatakse.

Sobivad väärtused Koosta tüüp:
- Asukoht – Rank koostada. (Asukoha jaoks koostada üksikasjad, vaadake "Seadme õ soovitus API dokumentatsiooni" dokument)
- Soovitus - soovitusteenuse koostamine.
- FBT - sageli ostnud koos koostamine.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Soovituste hankimine

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli (tõstutundlik) ainuidentifikaator |
| Kas ItemId-d | Üksuste jaoks soovitada komaga eraldatud loend.<br>Max pikkus: 1024 |
| numberOfResults | Nõutav tulemite arv |
| includeMetatadata | Edaspidiseks kasutamiseks,. alati väärtus false |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200

Vastus sisaldab ühte kirjet soovitatav üksuse kohta. Iga kirje on järgmised andmed.

- `Feed\entry\content\properties\Id`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name`– Üksuse nimi.
- `Feed\entry\content\properties\Rating`-Reiting soovitus; suurem arv tähendab, et kõrgema confidence.
- `Feed\entry\content\properties\Reasoning`-Soovitus arutluskäiku (nt soovitus selgitused).

OData XML-i

Alltoodud näites vastus sisaldab 10 Soovitatavad üksused.

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Värskenda mudel
Saate värskendada mudeli kirjeldus või aktiivne Koosta ID-ga.
*Aktiivse Koosta ID* - iga mudeli iga koostamine on Koosta ID-ga. Aktiivse koostamine ID on iga uue mudeli esimese eduka koostamine. Kui teil on aktiivne Koosta ID-d ja teha täiendavaid koostab sama mudeli, peate konkreetselt määratud see vaikimisi koostamine ID, kui soovite. Kui teil peab olema soovitusi, kui te ei määra koostamine ID, mida soovite kasutada, kasutatakse vaikimisi automaatselt.

See võimaldab teil – kui olete soovitus mudeli valmistamisel - koostada uusi mudeleid ja testimine need enne neid edendada tootmisele.

| HTTP-meetod | URI |
|:--------|:--------|
|SELLELE     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| ID | Kui seate mudeli (tõstutundlik) ainuidentifikaator |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Pange tähele, et XML-sildid, kirjelduse ja ActiveBuildId on valikuline. Kui te ei soovi kirjeldus või ActiveBuildId, kogu sildi eemaldada. |

**Vastus**:

HTTP olekukoodi: 200

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Juriidiline
Selles dokumendis on esitatud "nagu-on". Teavet ja arvamusi selles dokumendis, sh URL ja muud Internet veebisaidi viited, võivad olla järgmised. Mõned näited siin kujutatud on mõeldud ainult joonisel ja on väljamõeldud. Pole otse ühendus või ühendus on mõeldud või tohiks seostada. Selle dokumendi ei anna teile mis tahes õigusi seda intellektuaalomandi tootes Microsoft. Võite kopeerida ja kasutada seda dokumenti oma asutuse siseseks teatmematerjalina. © 2014 Microsofti. Kõik õigused. 
 
