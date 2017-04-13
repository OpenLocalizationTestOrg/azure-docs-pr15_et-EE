<properties 
    pageTitle="Õppekeskuse soovitused API dokumentatsiooni masina | Microsoft Azure'i" 
    description="Azure'i masina õ soovitused dokumentatsiooni on soovitused mootor saadaval Microsoft Azure'i turuplatsilt." 
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
    ms.author="LuisCa"/>

#<a name="azure-machine-learning-recommendations-api-documentation"></a>Azure'i masina Õppekeskuse soovitusi API dokumentatsioon

>[AZURE.NOTE]Soovitused API kognitiivse teenuse peaks kasutuselevõtt, selle asemel, et see versioon. Soovitused kognitiivse teenus on asendades selle teenuse ja seal arendatakse uusi funktsioone. See on uue võimalused nagu partiide tugi, parem API Explorer, cleaner API Surface'i, ühtsema registreerimine/arveldamine kasutuskogemuse jne.
> Lisateavet [uue kognitiivse teenuse migreerimine](http://aka.ms/recomigrate)

Selles dokumendis on kujutatud Microsoft Azure'i masina õ soovitused API-d.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. Üldine ülevaade
Selles dokumendis on viide API. Teil peab algama eesliitega "Azure seadme õ soovitus - Kiirkäivituse" dokument.

Azure'i masina õ soovituste API saab jagada järgmised loogiliste rühmadena:

- <ins>Piirangud</ins> - soovitusi API piirangud.
- <ins>Üldist teavet</ins> - autentimine, teenuse URI ja versioonimise kohta.
- <ins>Mudeli põhi</ins> - API-d, mis võimaldavad teil teha mudeli põhitoimingutega (nt luua, värskendada ja kustutada mudeli).
- <ins>Mudeli täpsemalt</ins> - API-d, mis võimaldavad teil täiustatud Andmeanalüüs mudel.
- <ins>Mudeli Business reeglid</ins> - API-d, mis võimaldavad teil mudel soovitus tulemuste business reeglite haldamiseks.
- <ins>Kataloogi</ins> - API-d, mis võimaldavad teil mudel kataloogi peamiste toimingute tegemiseks. Kataloogiga sisaldab metaandmete kasutamine andmete üksused.
- <ins>Funktsioon</ins> - API-d, mis võimaldavad saada ülevaadet üksusel kataloogi ja kuidas selle teabe abil saate koostada parem soovitusi.
- <ins>Kasutusandmete</ins> - API-d, mis võimaldavad teil teha põhilised tegevused mudeli kasutamine andmete põhjal. Read, mis sisaldavad paari & #60; kasutajanimi & #62; koosneb kasutusandmete lihtsa vormi, & #60; itemId ja #62;.
- <ins>Koostage</ins> - API-d, mis võimaldavad teil päästik mudeli koostamine ja tehke põhilised tegevused, mis on seotud selle. Kui teil on väärtuslik kasutusandmete võite käivitada mudeli koostamine.
- <ins>Soovitus</ins> - API-d, mis võimaldavad teil kui mudeli koostamine lõpus peab olema soovitusi.
- <ins>User Data</ins> - API-d, mis võimaldavad teil teabe toomiseks kasutus kasutaja andmeid.
- <ins>Teatiste</ins> - API-d, mis võimaldavad teil saada teatisi oma API toimingutega seotud probleemide kohta. (Näiteks on aruannete kasutamine andmete andmete kogumine ja töötlemine ei suuda sündmuste enamik kaudu. Teatis tõrge tõstetakse.)

##<a name="2-limitations"></a>2. piirangud

- Suurim arv tellimuse kohta mudelite on 10.
- Suurim lubatud arv järgud mudeli kohta on 20.
- Üksused, mille kataloogiga mahub maksimumarv on 100 000.
- Kasutus punkte, mis on maksimaalne arv on ~ 5 000 000. Vanimast kustutatakse uusi faile üles laadida või teatatud.
- Andmed, mida saab saata postitusse (nt kataloogi andmete importimine, andmete importimine kasutus) maksimaalne maht on 200MB.
- Üksused, mille puhul saate küsida, kui te soovitusi maksimumarv on 150.

##<a name="3-apis---general-information"></a>3. API - Üldteave

###<a name="31-authentication"></a>3,1. Autentimine
Järgige Microsoft Azure'i turuplatsi suuniste autentimist. Kuvatakse Marketplace'ist toetab Basic või OAuthi autentimise meetodit.

###<a name="32-service-uri"></a>3,2. Teenuse URI
Teenuse Azure seadme õ soovitused API URI juur on [siin.](https://api.datamarket.azure.com/amla/recommendations/v3/)

Täielik teenuse URI on väljendatud elemendid OData määratlus.  

###<a name="33-api-version"></a>3.3. API versioon
Iga API kõne on lõpus, nimetatakse apiVersion, mis peaks olema seatud 1.0 Päringuparameetri.

###<a name="34-ids-are-case-sensitive"></a>3.4. ID-d on tõstutundlik
ID-d, mis tahes API-d, tagastatud on tõstutundlikud ja tuleks kasutada sellisena, kui nimega parameetrite edaspidised API kõned vastu. Näiteks mudeli ID-d ja kataloogi ID-d on tõstutundlikud.

##<a name="4-recommendations-quality-and-cold-items"></a>4. soovitused kvaliteet ja külma üksused

###<a name="41-recommendation-quality"></a>4.1. Soovitused kvaliteet

Soovitus mudel on tavaliselt piisavalt lubamiseks soovituste andmiseks. Soovitused kvaliteet sõltub siiski kasutus, töödeldav ja kataloogi ulatust. Näiteks kui teil on palju külma esemete (ilma märgatavat kasutus), süsteemi on raskusi esitada soovitus sellise üksuse või soovitatav ühe sellise objekti kasutamine. Selleks, et külm üksuse probleemi lahendamiseks süsteem võimaldab kasutada metaandmete üksuste täiustamiseks soovitusi. Selle metaandmete nimetatakse funktsioone. Tüüpilised funktsioonid on raamatu autori või mõne filminäitleja. Funktsioonid on saadaval /-väärtuse stringide kujul kataloogi kaudu. Kataloogi faili täielik vorming, lugege [importimise kataloogi jaotis](#81-import-catalog-data). 

###<a name="42-rank-build"></a>4.2. Rank koostamine

Funktsioonid võivad parandada soovitus mudeli, kuid selleks on vaja mõtestatud funktsioone kasutada. Selleks võeti kasutusele uue koostamine – rank koostamine. Selle koostamine on järjestada kasulikkus funktsioone. Tähendusega funktsioon on funktsiooni rank Keskmine, 2 ja üles.
Pärast mõistmist, millised funktsioonid on tähenduslik, käivitamine soovitus Koosta mõtestatud funktsioonide loendi (või alamloendi). On võimalik nende funktsioonide kasutamiseks nii soe üksused ja külma üksused. Kasutamiseks soe üksusi, kuvatakse `UseFeatureInModel` koostamine parameeter tuleks luua. Funktsioonide kasutamiseks külma üksuste soovitud `AllowColdItemPlacement` koostamine parameeter peaks olema lubatud.
Märkus: Ei ole võimalik lubada `AllowColdItemPlacement` ilma `UseFeatureInModel`.

###<a name="43-recommendation-reasoning"></a>4.3. Soovitus arutluskäik

Soovitus arutluskäik on teise funktsiooni kasutamine. Tõepoolest Azure seadme õ soovitused mootori abil saate funktsioonid soovitus selgituse (alias menetluse järjepidevuse) viib rohkem confidence soovitatav üksuse soovitus tarbija.
Lubamiseks mõttekäiku on `AllowFeatureCorrelation` ja `ReasoningFeatureList` parameetrid tuleb enne paluda soovitus Koosta häälestus.


##<a name="5-model-basic"></a>5. Basic mudeli

###<a name="51-create-model"></a>5.1. Andmemudeli loomine
Loob "andmemudeli loomine" taotluse.

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

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

###<a name="52-get-model"></a>5.2. Mudeli hankimine
Loob "get mudel" taotluse.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Näide:<br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   ID  |   Kui seate mudeli (tõstutundlik) ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Mudeli andmed leiate järgmisi elemente:

- `feed/entry/content/properties/Id`-Mudeli kordumatu ID-ga.
- `feed/entry/content/properties/Name`-Mudeli nimi.
- `feed/entry/content/properties/Date`-Mudeli loomise kuupäev.
- `feed/entry/content/properties/Status`-Mudeli olek. Ühte järgmistest:
    - Loodud - mudel on loodud ja ei sisalda kataloogi ja kasutamist.
    - ReadyForBuild - mudeli on loodud ja see sisaldab kataloogi ja kasutamist.
- `feed/entry/content/properties/HasActiveBuild`– Näitab, kui mudeli ehitatud edukalt.
- `feed/entry/content/properties/BuildId`-Mudeli aktiivne Koosta ID-ga.
- `feed/entry/content/properties/Mpr`-Modelleerimise keskväärtus protsentiili järjestuse (MPR - lisateabe saamiseks vaadake ModelInsight).
- `feed/entry/content/properties/UserName`-Mudeli ettevõttesisese kasutaja nimi.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

###<a name="53-get-all-models"></a>5.3. Saada kõik mudelid
Toob kõik mudelid praeguse kasutaja.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br>Näide:<br>`<rootURI>/GetAllModels?apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

- `feed/entry/content/properties/Id`-Mudeli kordumatu ID-ga.
- `feed/entry/content/properties/Name`-Mudeli nimi.
- `feed/entry/content/properties/Date`-Mudeli loomise kuupäev.
- `feed/entry/content/properties/Status`-Mudeli olek. Ühte järgmistest:
  - Loodud - mudel on loodud ja ei sisalda kataloogi ja kasutamist.
  - ReadyForBuild - mudeli on loodud ja see sisaldab kataloogi ja kasutamist.
- `feed/entry/content/properties/HasActiveBuild`– Näitab, kui mudeli ehitatud edukalt.
- `feed/entry/content/properties/BuildId`-Mudeli aktiivne Koosta ID-ga.
- `feed/entry/content/properties/Mpr`-Modelleerimise MPR (Lisateavet leiate ModelInsight).
- `feed/entry/content/properties/UserName`-Mudeli ettevõttesisese kasutaja nimi.
- `feed/entry/content/properties/UsageFileNames`-Mudeli kasutamine failide komaga eraldatud loend.
- `feed/entry/content/properties/CatalogId`-Mudeli kataloogi ID-ga.
- `feed/entry/content/properties/Description`-Mudeli kirjeldus.
- `feed/entry/content/properties/CatalogFileName`-Mudeli kataloogi faili nimi.

OData XML-i


    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

###<a name="54-update-model"></a>5.4. Mudeli värskendamine

Saate värskendada mudeli kirjeldus või aktiivne Koosta ID-ga.<br>
<ins>Aktiivse Koosta ID</ins> - iga mudeli iga koostamine on Koosta ID-ga. Aktiivse koostamine ID on iga uue mudeli esimese eduka koostamine. Kui teil on aktiivne Koosta ID-d ja teha täiendavaid koostab sama mudeli, peate konkreetselt määratud see vaikimisi koostamine ID, kui soovite. Kui teil peab olema soovitusi, kui te ei määra koostamine ID, mida soovite kasutada, kasutatakse vaikimisi automaatselt.<br>
See võimaldab teil – kui olete soovitus mudeli valmistamisel - koostada uusi mudeleid ja testimine need enne neid edendada tootmisele.


| HTTP-meetod | URI |
|:--------|:--------|
|SELLELE     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br>Näide:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   ID      | Kui seate mudeli (tõstutundlik) ainuidentifikaator  |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br>Pange tähele, et XML-sildid, kirjelduse ja ActiveBuildId on valikuline. Kui te ei soovi kirjeldus või ActiveBuildId, kogu sildi eemaldada.|

**Vastus**:

HTTP olekukoodi: 200

###<a name="55-delete-model"></a>5,5. Mudeli kustutamine
Kustutab olemasoleva mudeli ID kaudu.

| HTTP-meetod | URI |
|:--------|:--------|
|KUSTUTAMINE     |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Näide:<br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   ID  |   Kui seate mudeli (tõstutundlik) ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

##<a name="6-model-advanced"></a>6. Täpsem mudel

###<a name="61-model-data-insight"></a>6.1. Mudeli andmete ülevaade
Tagastab selle mudeli ehitatud kasutuse andmeid statistilisi andmeid.

Saadaval ainult soovitus koostamine.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Näide:<br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Andmete tagastatakse atribuutide kogum.

- `feed/entry/id/content/properties/key`-Hoiab atribuudi nimi.
- `feed/entry/id/content/properties/value`-Hoiab atribuudi väärtust.

Järgmises tabelis on kujutatud väärtus, mis tähistab iga võti.

|Klahv|Kirjeldus|
|:-----|:----|
| AvgItemLength | Iga üksuse kohta erinevate kasutajate arvu. |
| AvgUserLength | Kasutaja kohta erinevate üksuste arvu. |
| DensificationNumberOfItems | Pärast kärpimine üksused, mida ei saa modelleerida üksuste arv. |
| DensificationNumberOfUsers | Pärast kärpimine kasutajad ja üksused, mida ei saa modelleerida kasutus punktide arv. |
| DensificationNumberOfRecords | Pärast kärpimine kasutajad ja üksused, mida ei saa modelleerida kasutus punktide arv. |
| MaxItemLength | Kõige populaarsemate üksuse erinevate kasutajate arv. |
| MaxUserLength | Erinevate üksuste kasutaja maksimaalne arv. |
| MinItemLength | Erinevate kasutajate üksuse maksimaalne arv. |
| MinUserLength | Erinevate üksuste kasutaja minimaalne arv. |
| RawNumberOfItems | Failide kasutamine üksuste arv. |
| RawNumberOfUsers | Enne mis tahes kärpimine kasutus punktide arv. |
| RawNumberOfRecords | Enne mis tahes kärpimine kasutus punktide arv. |
| SamplingNumberOfItems | N/A |
| SamplingNumberOfRecords | N/A |
| SamplingNumberOfUsers | N/A |

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="62-model-insight"></a>6.2. Mudeli tutvustus
Tagastab modelleerimise ülevaate aktiivse ehitada või (kui antud) klõpsake teatud koostamine.

Saadaval ainult soovitus koostamine.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |Aktiivne koostada ID:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>Järgu ID:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator |
|   buildId |   Valikuline – arv, mis tuvastab eduka koostamine. |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Andmete tagastatakse atribuutide kogum.

- `feed/entry/id/content/properties/key`
- `feed/entry/id/content/properties/value`


Järgmises tabelis on kujutatud väärtus, mis tähistab iga võti.

| Klahv | Kirjeldus |
|:---- |:----|
| CatalogCoverage | Milline osa kataloogi saab modelleerida kasutus mustriga. Ülejäänud üksused on vaja sisu-põhised funktsioonid. |
| MPR | Keskmise protsentiili järjestus mudel. Alumine on parem. |
| NumberOfDimensions | Maatriksi Faktoriseerimine algoritmi kasutatavad mõõdud arv. |


OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="63-get-model-sample"></a>6.3. Mudeli valimi hankimine
Saab valim aia soovitus mudel.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Näide:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>Järgu ID:<br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br>Näide:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

OData XML-i

Vastuse tagastatakse töötlemata teksti vormingus:

<pre>
Level 1 --------------- 655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel 655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel Rating: 0.5215 3f471802-f84f-44a0-99c8-6d2e7418eec1, Black Hawk Down: A Story of Modern War Rating: 0.5151 07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5148 6afc18e4-8c2a-43d1-9021-57543d6b11d8, Imajica Rating: 0.5146 e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.514 56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai 56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai Rating: 0.5218 53156702-cc0c-443d-b718-6fb74b2491d3, Son of \ Rating: 0.5212 fb8cf7a6-8719-46ee-97d4-92f931d77a3a, Smoke and Mirrors: Short Fictions and Illusions Rating : 0.5188 8f5fe006-79e4-4679-816b-950989d1db4b, koht olen kunagi (kaasaegne Ameerika Fiction) Reiting: 0.5156 d8db4583-cc0f-49ce-bc95-b7fa3491623f, õnne: A romaan hindamine: 0.5156 50471eec-9aeb 4900 84 d 7-21567ab18546, kui Buddha kuupäevaga: A kasutusjuhendist otsimine armastus on Usuteemaline tee cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, jumalik saladusi on Ya-Ya Sõsarkond: A romaan hindamine: 0.5266 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, Poisonwood Piibel: A romaan hindamine: 0.5252 973f8cbd-0846-4f6b – 9d 28-4dd0d7dc3a19, siga sisse Heaven Reiting: 0.5244 e2cbf7ad-0636-4117-8b30-298da6df7077, looma unistused hindamine : 0.5227 6c818fd3-5a09-417d-9ab4-7ffe090f0fef, on inetu poolõde ülestunnistused: A romaan hindamine: 0.5222 5e97148f-defb - 4d 74-af2d-80f4763bf531, Deep End selle ookeani (Oprah broneerida klubi) 5e97148f-defb - 4d 74-af2d-80f4763bf531, Deep End ookeani (Oprah broneerida klubi) Reiting: 0.537 5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Island üles: A romaan hindamine: 0.5277 bc5b69db-733b-4346-adde-3927544258f7, Downtown hindamine: 0.5275 31fe5c63-3e5a - 48d 0-802b-d3b0f989a634, on tore päev: A lugu Vererõhumõõtja ja Sweatsocks hindamine: 0.5252 0adf981a-b65b - 4c 11-b36b-78aca2f948a2, täiuslik Storm : On tõene lugu, mehed vastu the Sea hindamine: 0.5238 68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True lugu on suurim elevant, kes kunagi elanud 68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True lugu on suurim Elephant, et kunagi elas hindamine: 0.5379 6724862e-e4e7-4022-9614-1468d8b902ff, väike maja Prairie hindamine: 0.5345 cdedb837-1620-496d - 94c 4-6ccfed888320, väike maja suur Woods hindamine: 0.5325 382164ba-406b-4187-b726-d7a54b9d790d, lõpetamiseks koputage, Puhh hindamine: 0.5309 6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5 , Klõpsake pankadevahelistes ploomi Creek Reiting: 0.5285 37ef8e74-e348-44e5-aabc-1d7f9efcb25b, mehed on Mars naised on Venus: praktiline juhend parandamine suhtluse ja saada mida soovite oma seosed 37ef8e74-e348-44e5-aabc-1d7f9efcb25b, mehed on Marsilt, naised on Venus: praktiline juhend parandamine suhtluse ja saada mida soovite oma seoste hindamine: 0.5397 f2be16d4-5faf - 4d 32-ab83-7ba74d29261e, Politically õige Unejutud : Tänapäevane jutustused meie elu ja kellaajad hindamine: 0.5207 ef732c5c-334b-4d6b-ab82-7255eb7286d0, au vahel varastada hindamine: 0.5195 0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, The annab puu hindamine: 0.5194 883b360f-8b42-407f-b977-2f44ad840877, jube lugusid öelda, tume: kogutud Ameerika Folklore (hirmutav lood) Reiting: 0.5184 ff51b67e-fa8e-4c5e-8f4d-02a928de735d, mehed tööl: käsitöö Baseball d008dae9-c73a-40a1-9a9b-96d5cf546f36, The arhipelaag 1918-1956: An katse kirjandusliku uurimine I – II Reiting: 0.5416 ff51b67e-fa8e-4c5e-8f4d-02a928de735d, mehed tööl : Baseball reiting käsitöö: 0.5403 49dec30e-0adb-411a-b186-48eaabf6f8bc, Isamaa hindamine: 0.5394 cc7964fd-d30f-478e-a425-93ddbdf094ed, maagiline kogumine: Arena mahu 1 hindamine: 0.5379 8a1e9f36-97af-4614-bed9-24e3940a05f3, rohkem Sniglets: iga sõna, et ei kuvata sõnastikus, kuid peaksite hindamine: 0.5377 12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream kotkad 07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon hindamine: 0.5417 e4cc5e69-3567-43ab-b00f-f0d8d0506870, tulemus loendi hindamine: 0.5416 1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, The pere hindamine: 0.5371 56daeffe - 7d 48-43cd-8ef8-7dffd0c103d3, kg klassi hindamine: 0.5366 b2fe511e-5cb9-4a56-b823-2801e63e6a96, tasuta pakkumise hindamine : 0.5366 df87525b e435-4bd6-8701-4e60ad344e28, kala otsimise 56d 33036-dfda-46b9-8e2a-76cb03921bb0, X-Files: nullpunkt hindamine: 0.5417 0780cde8-6529-4e1d-b6c6-082c1b80e596, kaksteist punane heeringas hindamine: 0.5416 df87525b-e435-4bd6-8701-4e60ad344e28, leida kala hindamine: 0.5408 400fe331 - 2c 35-490c-adbc-b28b4b73d56c, peab me öelda juhataja? Reiting: 0.5383 f86ad7d0 - 5c 03-42b3-aebf-13d44aec8b30, tooni ajapikenduse hindamine: 0.5358 de1f62a4-89e6 - 44d 2-aaee-992a4bf093f1, mis muutis maailma kaardi: William soe ja selle sünni kaasaegse geoloogia de1f62a4-89e6 - 44d 2-aaee-992a4bf093f1, muudetud maailma kaardi: William soe ja selle sünni, kaasaegse geoloogia hindamine: 0.5422 b303538f-e2c6-4a2c-b425-8d21e684fc3e, minu onu Oswald hinnang: 0.5385 34b84627-48af-4a4c - 96c 4-b26fb3863f56, keskööst rakenduses Aed on hea ja kurja hindamine: 0.5379 306cbaa7 b1a8-4142 9 d 55-e11b5018a7a8, tänav advokaat hindamine : 0.5376 e53b4baa - 8c 09-45c 4-95c 0-b6a26b98770b, Miss Smillas tunne Snow hindamine: 0.5367

<a name="level-2"></a>Tase 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, The halba siga (Hillerman Tony) 352aaea1-6b12-454d-a3d5-46379d9e4eb2, halba siga (Hillerman Tony) hindamine: 0.5425 74c 49398-bc10-4af5-a658-a996a1201254, Laste tormi (Peters Elizabeth) hindamine: 0.5387 9ba80080-196e-43fd-8025-391d963f77e7, The ujuv tüdruku hindamine: 0.5372 e68f81d5 7745-4cc7-b943-fedb8fcc2ced, tapja naeratus (Scottoline Lisa) Reiting: 0.5353 b2fe511e-5cb9-4a56-b823-2801e63e6a96, tasuta pakkumise hindamine: 0.5332 c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon päeva 0adf981a-b65b - 4c 11-b36b-78aca2f948a2, täiuslik Storm: A True lugu, mehed vastu the Sea hindamine: 0.5433 c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon päeva hindamine : 0.543 a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: kõik sõna, et ei kuvata sõnastiku, kuid peaksite) hindamine: 0.5327 6f6e192e - 0d 64-49ca-9b63-f09413ea1ee6, Politically õige Püha lugude: jaoks an valgustunud jõulude aeg Season (aastaaeg) hindamine: 0.5307 798051a8 - 147d - 4d 46-b0dc-e836325029e6, vanus presumptsiooni (filmi TIE-IN) REITING: 0.5301 73f3e25a-e996-4162-9ed8-ff3d34075650, O pioneerid! (Penguin 20 – Century Classics) cba8163f-6536-436b-8130-47b4a43c827f, usalda keegi (ametlik juhend mahu 2 X-failid) Reiting: 0.5434 5708e4cb-2492-49 c 0-94a8-cc413eec5d89, väike jumalad (Kettamaailma romaanid (Paperback)) hindamine: 0.5406 73f3e25a-e996-4162-9ed8-ff3d34075650, O pioneerid! (Penguin 20 – Century Classics) Reiting: 0.5403 d885b0bd-ae4b-452d-bdf2-faa90197dbc9, värv, maagiline hindamine: 0.539 b133a9c4-4784-4db3-b100-d0d6dffb94d2, etalonina on välja seal (ametlik juhend X-failide mahu 1) Reiting: 0.5367 271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: või ma teada, miks the tiibadega vaal laulab 271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: või ma tean, miks tiivuline vaal laulab Reiting: 0.5445 2de1c354-90ff - 47c 5-a0db-1bad7d88ef94, The Salaryman naine (lastel vägivalla sarja) hindamine: 0.5329 d279416e - 19c 0-43f8-9ec9-a585947879ca, Zen suhtumine hindamine : 0.5316 c8f854d7-3de3-4b23-8217-f4f851670fd4, kätte maksta Cootie tüdrukud: A jaan Hudson salajase (jaan Hudson saladused (Paperback)) Reiting: 0.5305 8ef4751c-7074-409e-a3ac-d49b222fc864, kus the Wild asju on hindamine: 0.5289 9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, nende Eyes olid vaadates Jumal 9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, nende Eyes olid vaadates Jumal hindamine: 0.5446 da45c4d5-aba1-413b-a9bd-50df98b1e1d2, Bean puude hindamine: 0.5389 65ecbdd1 - 131c - 40c 3-a3d6-d86ca281377a, The Jumal, väike asjade hindamine: 0.5387 c78743bf-7947-4a0c-8db7-8a3bfe69ba70, kivi päevikud hindamine: 0.5355 973f8cbd-0846-4f6b – 9d 28-4dd0d7dc3a19, klõpsake Heaven reiting siga : 0.5344 5f17d90a-2604-4fe8-8977-1a280b9098b1, üks raha (Stephanie ploomi romaanid (Paperback)) 5f17d90a-2604-4fe8-8977-1a280b9098b1, üks reiting raha (Stephanie ploomi romaanid (Paperback)): 0.5446 57169b2b-9a8a-486b-9aac-1ed98ce57168, lõplik peale hindamine: 0.5332 efcb1bc4-7278-4a8f-b491 – befde02070d6, tõe hindamine: 0.5329 1efa91a2-993b - 4c 43-9f5c-3454fc12612d, kirjutamine tegur hindamine: 0.5309 24c 59962-458a-4ec8-b95d-d694e861919c, kodus Mitford (Mitford aastate) hindamine: 0.5303 4fd48c46-1a20 - 4c 57-bc7f-a02ef123dc52, nagu laadi teda: The poisi kellel oli astmes kui ka tüdruku 4fd48c46-1a20-4c57-bc7f-a02ef123dc52 , Nagu laadi talle: selle poisi kes oli astmes kui ka tüdruku hindamine: 0.5449 cd5f2c03-20cb-43be-a1fb-3b4233e63222, siga sisse Heaven Reiting: 0.5329 19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, armastus koolera aeg (Penguin väga raamatuid 20) Reiting: 0.5267 15689d 09-c711 4844 84 d 8-130a90237b26 Bel Canto hindamine: 0.5245 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, Poisonwood Piibel: A romaan hindamine: 0.5235 98df28ec-41e7-4fca-b77f-8b0d3109085d, Võitlejaks mälu f874b5a3 - 5d 40-4436-94ff-0fa1c090ddf5, The p ka tõusu (A Scribner klassikaline) hindamine : 0.5451 98df28ec-41e7-4fca-b77f-8b0d3109085d, Võitlejaks mälu hindamine: 0.5442 0ce0014a-9a48-4013-a08a-7f2c11877930, teisiti Nähtamatu Reiting: 0.5421 15316ca6-1e38-425f-893d-691944a47000, rohkem hirmutav lugude soovite teada saada, klõpsake The tume hindamine: 0.5409 329d 5682-3dc3-4206-8aa2-eef4b1032258, tähed maa hindamine: 0,54 5b9445d5-c072 - 419c - 8d 49-6f669bb1b0a9, Tütar, Fortune: A romaan (Oprah broneerida klubi (kõva kaas)) 5b9445d5-c072 - 419c - 8d 49 6f669bb1b0a9, Tütar, Fortune: A romaan (Oprah broneerida klubi (kõva kaas)) Reiting: 0.5462 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, Poisonwood Piibel: A romaan hindamine: 0.5372 604eb3bd-6026-4f51-bffd-9fb54f180400, pere pildid: A romaan hindamine: 0.5341 8d06d01d – 31cd-4678-b6b1-140a67987ce9, laule tavaliste ajakulu (Oprah broneerida klubi (Paperback)) : 0.5334 da45c4d5-aba1-413b-a9bd-50df98b1e1d2, Bean puude hindamine: 0.5319 d5358189-d70f-4e35-8add-34b83b4942b3, rakenduses taevas d5358189-d70f-4e35-8add-34b83b4942b3 siga, siga sisse Heaven Reiting: 0.5491 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, Poisonwood Piibel: A romaan hindamine: 0.5401 c78743bf-7947-4a0c-8db7-8a3bfe69ba70, kivi päevikud hindamine: 0.5393 8d06d01d – 31cd-4678-b6b1-140a67987ce9, tavaline (Oprah broneerida klubi (Paperback)) ajakulu laule: 0.5382 973f8cbd-0846-4f6b – 9d 28-4dd0d7dc3a19, siga sisse Heaven Reiting: 0.5367

</pre>


##<a name="7-model-business-rules"></a>7. mudeli Business reeglid

Need on reeglid, mis on toetatud tüüpi:
- <strong>Blokeeritud saatjate loendisse</strong> - blokeeritud saatjate loendisse võimaldab loetleda üksused, mida te ei soovi soovitus tulemite tagastamiseks. 

- <strong>FeatureBlockList</strong> - funktsiooni blokeeritud saatjate loendisse abil saate blokeerida üksuste funktsioonide väärtuste põhjal.

*Saata rohkem kui 1000 üksust reeglis ühe blokeeritud saatjate loendisse või kõne võib ajalõpp. Kui soovite blokeerida enam kui 1000 üksusi, saate helistada mitmele blokeeritud saatjate loendisse.*

- <strong>Upsale</strong> - Upsale võimaldab Jõusta üksuste soovitus tulemite tagastamiseks.

- <strong>Valge</strong> - valge loendi võimaldab ainult soovitamine üksuste loendist soovitused.

- <strong>FeatureWhiteList</strong> - funktsioonide valge loend, mis võimaldab teil ainult soovitada üksused, mis on teatud funktsioonide väärtused.

- <strong>PerSeedBlockList</strong> - kohta seemne blokeeritute loendis võimaldab teil iga üksuse kohta loetleda üksused, mida ei saa tagastada soovitus tulemitena.




###<a name="71-get-model-rules"></a>7.1. Saada mudeli reeglid

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Näide:<br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

- `feed/entry/content/properties/Id`– See reegel ainuidentifikaator.
- `feed/entry/content/properties/Type`-Reegli tüüp.
- `feed/entry/content/properties/Parameter`-Reegli parameeter.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="72-add-rule"></a>7.2. Reegli lisamine

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/AddRule?apiVersion=%271.0%27`|
|PÄIS   |`"Content-Type", "text/xml"`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | 
<ins>Iga kord, kui esitada business reeglite üksuse ID-d, veenduge, et kasutada välise üksuse (sama Id, mida kasutasite kataloogi faili) ID.</ins><br>
<ins>Blokeeritud saatjate loendisse reegli lisamiseks tehke järgmist.</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><ins>
<ins>FeatureBlockList reegli lisamiseks tehke järgmist.</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><ins>Reegli Upsale lisamiseks tehke järgmist.</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br>
<ins>Valge reegli lisamiseks tehke järgmist.</ins><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><ins>
<ins>FeatureWhiteList reegli lisamiseks tehke järgmist.</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><ins>PerSeedBlockList reegli lisamiseks tehke järgmist.</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|


**Vastus**:

HTTP olekukoodi: 200

API tagastab selle üksikasjadega äsja loodud reegel. Atribuudi reeglid võib laadida järgmine tee:

- `feed/entry/content/properties/Id`– See reegel ainuidentifikaator.
- `feed/entry/content/properties/Type`-Reegli tüüp: blokeeritud saatjate loendisse või Upsale.
- `feed/entry/content/properties/Parameter`-Reegli parameeter.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="73-delete-rule"></a>7.3. Reegli kustutamine

| HTTP-meetod | URI |
|:--------|:--------|
|KUSTUTAMINE     |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator |
|   filterId    |   Filtri ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

###<a name="74-delete-all-rules"></a>7.4. Kustutage kõik reeglid

| HTTP-meetod | URI |
|:--------|:--------|
|KUSTUTAMINE     |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

##<a name="8-catalog"></a>8. kataloogi

###<a name="81-import-catalog-data"></a>8.1. Andmete importimine kataloogi

Kui mitu kataloogi failide üleslaadimine mitu kõned sama mudel, me lisatakse ainult kataloogi uued üksused. Olemasolevate üksuste jäävad algsed väärtused. Kataloogi andmeid ei saa värskendada selle meetodi abil.

Andmete kataloogi peaks järgige järgmises vormingus:

- Funktsioonid - ilma`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Funktsioonidega-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

Märkus: Failide maksimummaht on 200MB.

**Vormingu üksikasjad**

| Nimi | Kohustusliku | Tüüp |  Kirjeldus |
|:---|:---|:---|:---|
| Üksuse Id |Jah | [A – z], [a – z], [0 – 9], [_] & #40; Allkriipsu & #41; [-] & #40; Kriipsjoone & #41;<br> Max pikkus: 50 | Üksuse ainuidentifikaator. |
| Üksuse nimi | Jah | Mis tahes tähtnumbrilised märgid<br> Max pikkus: 255 | Üksuse nimi. | 
| Üksuse kategooria | Jah | Mis tahes tähtnumbrilised märgid <br> Max pikkus: 255 | Kategooria, mis kuulub selle üksuse (nt Cooking raamatud, draama...); võib olla tühi. |
| Kirjeldus | Ei, v.a juhul, kui need funktsioonid on esitamine (kuid võib olla tühi) | Mis tahes tähtnumbrilised märgid <br> Max pikkus: 4000 | Selle üksuse kirjeldus. |
| Funktsioonide loend | Ei | Mis tahes tähtnumbrilised märgid <br> Max pikkus: 4000; Max arv funktsioonid: 20 | Funktsiooni nimi komaga eraldatud loend = funktsiooni väärtuse, mida saate kasutada täiustamiseks mudeli soovitus; vt jaotist [Täpsem teemade](#2-advanced-topics) . |


| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|
|PÄIS   |`"Content-Type", "text/xml"`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator  |
| faili nimi | Teksti identifikaator kataloogi.<br>Ainult tähti (A – Z, a – z) arvud (0 – 9) sidekriipsu (-) ja allkriipsu (_) on lubatud.<br>Max pikkus: 50 |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | Näide (funktsioonidega):<br/>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, raamat, raamat kirjeldus, Autor = Richard Wright, publisher = Harper Flamingo Kanada, aasta = 2001<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, unusta Room: A Fiction (Byzantium vihik), raamat,, Autor = hüüdnimi Bantock, publisher = Harpercollins, aasta = 1997<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, raamat,, Autor = Timothy Findley, publisher = HarperFlamingo Kanada, aasta = 2001<br>552a1940-21e4-4399-82bb-594b46d7ed54, piiravad, loomad, raamat, raamat kirjeldus, Autor = Magnus Mills, publisher = avaldamise Arcade aasta = 1998</pre> |


**Vastus**:

HTTP olekukoodi: 200

API tagastab importimine aruande.
- `feed\entry\content\properties\LineCount`-Aktsepteeritud ridade arv.
- `feed\entry\content\properties\ErrorCount`-Pole tõrke tõttu lisatud ridade arv.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="82-get-catalog"></a>8.2. Kataloogi hankimine
Toob kõik kataloogiüksuste.
Kataloogi saab tuua korraga ühele lehele. Kui soovite saada üksuste teatud indeksi, saate $skip odata parameeter. Näiteks kui soovite saada alates positsioonist 100 üksusi, lisada parameeter $skip = 100 taotlusele.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Vastus sisaldab ühte kirjet kataloogi üksuse kohta. Iga kirje on järgmised andmed.

- `feed/entry/content/properties/ExternalId`-Kataloogi välise üksuse ID ühte kliendi poolt.
- `feed/entry/content/properties/InternalId`-Kataloogi üksuse sisemine ID, mis Azure seadme õ soovitused on loodud.
- `feed/entry/content/properties/Name`– Kataloogi üksuse nimi.
- `feed/entry/content/properties/Category`-Kataloogi üksuse kategooria.
- `feed/entry/content/properties/Description`-Kataloogi üksuse kirjeldus.
- `feed/entry/content/properties/Metadata`-Kataloogi üksuse metaandmete.


OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">The Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="83-get-catalog-items-by-token"></a>8.3. Luba kataloogi üksuste hankimine

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator |
|   luba   |   Turbeloa kataloogi üksuse nime. Peaks sisaldama vähemalt 3 märki. |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Vastus sisaldab ühte kirjet kataloogi üksuse kohta. Iga kirje on järgmised andmed.

- `feed/entry/content/properties/InternalId`-Kataloogi üksuse sisemine ID, mis Azure seadme õ soovitused on loodud.
- `feed/entry/content/properties/Name`– Kataloogi üksuse nimi.
- `feed/entry/content/properties/Rating`-(tulevaste kasutamiseks)
- `feed/entry/content/properties/Reasoning`-(tulevaste kasutamiseks)
- `feed/entry/content/properties/Metadata`-(tulevaste kasutamiseks)
- `feed/entry/content/properties/FormattedRating`-(tulevaste kasutamiseks)

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                </m:properties>
            </content>
        </entry>
    </feed>

##<a name="9-usage-data"></a>9. andmete kasutamine
###<a name="91-import-usage-data"></a>9.1. Andmete importimine kasutus
####<a name="911-uploading-file"></a>9.1.1. Faili üleslaadimine
Selles jaotises kirjeldatakse, kuidas kasutusandmete abil faili üles laadida. Saate helistada selle API mitu korda kasutuse andmetega. Kõik kasutusandmete salvestatakse kõik kõnede jaoks.

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId |   Kui seate mudeli ainuidentifikaator  |
| faili nimi | Teksti identifikaator kataloogi.<br>Ainult tähti (A – Z, a – z) arvud (0 – 9) sidekriipsu (-) ja allkriipsu (_) on lubatud.<br>Max pikkus: 50 |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | Andmete kasutamine. Vorming:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Nimi</th><th>Kohustusliku</th><th>Tüüp</th><th>Kirjeldus</th></tr><tr><td>Kasutaja-Id</td><td>Jah</td><td>[A – z], [a – z], [0 – 9], [_] & #40; Allkriipsu & #41; [-] & #40; Kriipsjoone & #41;<br> Max pikkus: 255 </td><td>Kasutaja ainuidentifikaator.</td></tr><tr><td>Üksuse Id</td><td>Jah</td><td>[A – z], [a – z], [0 – 9], [& #95;] & #40; Allkriipsu & #41; [-] & #40; Kriipsjoone & #41;<br> Max pikkus: 50</td><td>Üksuse ainuidentifikaator.</td></tr><tr><td>Aeg</td><td>Ei</td><td>Kuupäev vormingus: YYYY/MM/DDTHH:MM:SS (nt 06 2013/20T10:00:00)</td><td>Andmete aeg.</td></tr><tr><td>Sündmuse</td><td>Ei; Kui esitatud, siis peate ka sellele kuupäeva</td><td>Ühte järgmistest:<br>• Klõpsake<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Ostmine</td><td></td></tr></table><br>Faili suurim lubatud maht: 200MB<br><br>Näide:<br><pre>149452 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Vastus**:

HTTP olekukoodi: 200

- `Feed\entry\content\properties\LineCount`-Aktsepteeritud ridade arv.
- `Feed\entry\content\properties\ErrorCount`-Pole tõrke tõttu lisatud ridade arv.
- `Feed\entry\content\properties\FileId`-Identifikaator fail.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="912-using-data-acquisition"></a>9.1.2. Abil andmete kogumine
Selles jaotises kirjeldatakse, kuidas saata sündmuste reaalajas Azure seadme õ soovitusi, tavaliselt veebisaidilt.

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27`|
|PÄIS   |`"Content-Type", "text/xml"`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
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
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
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

###<a name="92-list-model-usage-files"></a>9.2. Loendi kasutamine Mudelifaile
Toob kõik mudeli kasutamine failide metaandmed.
Kasutamine faile saab alla laadida korraga ühele lehele. Iga lehe sisaldab 100 üksused. Kui soovite saada üksuste teatud indeksi, saate $skip odata parameeter. Näiteks kui soovite saada alates positsioonist 100 üksusi, lisada parameeter $skip = 100 taotlusele.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   forModelId  |   Kui seate mudeli ainuidentifikaator |
|   apiVersion      | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Vastus sisaldab ühte kirjet kasutus faili kohta. Iga kirje on järgmised andmed.

- `feed\entry\content\properties\Id`-Kasutus ID
- `feed\entry\content\properties\Length`-Kasutus faili pikkus MB.
- `feed\entry\content\properties\DateModified`-Kasutus faili loomise kuupäev.
- `feed\entry\content\properties\UseInModel`– Kas kasutus fail on kasutusel mudel.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
            </m:properties>
        </content>
    </entry>
</feed>

###<a name="93-get-usage-statistics"></a>9.3. Saada kasutuse statistika
Saab kasutusstatistika.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator  |
| Alguskuupäev |   Alguskuupäev. Vorming: yyyy/MM/ddTHH:mm:ss |
| Lõppkuupäev | Lõppkuupäev. Vorming: yyyy/MM/ddTHH:mm:ss |
| eventTypes |  Komaga eraldatud tekstistringi tüüpi sündmusi või null, et saada kõik sündmused  |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Kogum /-väärtuse elemente. Iga üks sisaldab sündmused Rühmitusalus tund kindla sündmuse tüübi summa.

- `feed\entry[i]\content\properties\Key`-Sisaldab (Rühmitusalus tund) aja ja sündmuse tüüp.
- `feed\entry[i]\content\properties\Value`-Kokku sündmuste arv.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="94-get-usage-file-sample"></a>9.4. Saada kasutus faili näidis
Otsib esimese 2KB kasutus faili sisu.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator  |
| fileId |  Mudeli kasutamine faili ainuidentifikaator  |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Vastuse tagastatakse töötlemata teksti vormingus:
<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15 , Tõene, 1 235588, 21BF8088-B6C0-4509-870C-E1C7AC78304A, 2014/11/02T13:40:15, tõene, 1 158254, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, tõene, 1 271195, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, tõene, 1 141157, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, tõene, 1 171118, 3BB5CB44-D143-4BDD-A55C-443964BF4B23, 2014/11/02T13:40:15, tõene, 1 225087, 3BB5CB44-D143-4BDD-A55C-443964BF4B23, 2014/11/02T13:40:15, tõene, 1
</pre>


###<a name="95-get-model-usage-file"></a>9.5. Saada faili mudeli kasutamine
Toob kogu sisu kasutus-faili.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| funktsioon MID | Kui seate mudeli ainuidentifikaator  |
| FID | Mudeli kasutamine faili ainuidentifikaator |
| Laadi alla | 1 |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

Vastuse tagastatakse töötlemata teksti vormingus:
<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15 ,True,1 235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15 ,True,1 260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15 , Tõene, 1 260965, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, tõene, 1 102758, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, tõene, 1 112602, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, tõene, 1 163925, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, tõene, 1 262998, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, tõene, 1 144717, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, tõene, 1
</pre>

###<a name="96-delete-usage-file"></a>9.6. Kasutus-faili kustutamine
Kustutab määratud mudeli kasutamine faili.

| HTTP-meetod | URI |
|:--------|:--------|
|KUSTUTAMINE     |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27`|

| Parameetri nimi    |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator  |
| fileId | Faili kustutada ainuidentifikaator |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200


###<a name="97-delete-all-usage-files"></a>9.7. Kõik kasutamine failide kustutamine
Kustutab kõik mudeli kasutamine failid.

| HTTP-meetod | URI |
|:--------|:--------|
|KUSTUTAMINE     |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27`|

| Parameetri nimi    |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator  |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

##<a name="10-features"></a>10. funktsioonide
Selles jaotises kirjeldatakse lepingute funktsioon teavet, nt imporditud funktsioonid ja nende väärtuste oma asukoht ja selle asukoht on antud tuua. Funktsioonide imporditakse kataloogi andmed ja seejärel oma asukoht on seotud kui rank koostamine on lõpule jõudnud.
Funktsioon rank muutmiseks vastavalt muster kasutusandmete ja tüüpi üksused. Kuid kasutus üksused, ühtsete reiting peaks olema ainult väikeste kõikumistega.
Asukoha funktsioonid on negatiivne arv. Arvu 0 tähendab, et see funktsioon pole oli kohal (juhtub siis, kui kutsute esimese rank koostamine enne selle API). Kuupäev, kus on antud reiting nimetatakse Keskmine värskus.

###<a name="101-get-features-info-for-last-rank-build"></a>10,1. Saamiseks funktsioonid Info (viimase Rank järk)
Otsib funktsioon teabe, sh järjestuse jaoks viimase eduka rank koostamine.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE      |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27`

| Parameetri nimi    |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator  |
|samplingSize| Iga funktsiooni vastavalt kataloogi leiduvate andmete kaasamiseks väärtuste arv. <br/>Võimalikud väärtused on:<br> -1 - kõik näidised. <br>0 - pole valimite. <br>N - tagastada N näidised iga funktsiooni nime.|
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |


**Vastus**:

HTTP olekukoodi: 200

Vastus sisaldab funktsiooni info kirjete loendi. Iga kirje sisaldab:

- `feed/entry/content/m:properties/d:Name`-Funktsiooni nimi.
- `feed/entry/content/m:properties/d:RankUpdateDate`-Kuupäeva, mille reiting on eraldatud see funktsioon nimega Keskmine värskus funktsiooni. Varasemate kuupäevade ('0001-01-01T00:00:00 ") tähendab, et tehtud pole rank koostamine.
- `feed/entry/content/m:properties/d:Rank`-Funktsiooni rank (hõljumine). Hea funktsiooni käsitletakse 2.0 ja üles soovitud asukoht.
- `feed/entry/content/m:properties/d:SampleValues`-Komaga eraldatud väärtuste loend kuni nõutav valimi suurus

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get the features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>


###<a name="102-get-features-info-for-specific-rank-build"></a>10.2. Saamiseks funktsioonid Info (Rank järgu)

Otsib funktsioon teabe, sh teatud rank ehitada järjestuse.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE      |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27`

| Parameetri nimi    |   Kehtivad väärtused    |
|:--------          |:--------          |
| modelId | Kui seate mudeli ainuidentifikaator  |
|samplingSize| Iga funktsiooni vastavalt kataloogi leiduvate andmete kaasamiseks väärtuste arv.<br/> Võimalikud väärtused on:<br> -1 - kõik näidised. <br>0 - pole valimite. <br>N - tagastada N näidised iga funktsiooni nime.|
|rankBuildId| Rank koostamine või viimase rank koostamine jaoks -1 ainuidentifikaator|
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |


**Vastus**:

HTTP olekukoodi: 200

Vastus sisaldab funktsiooni info kirjete loendi. Iga kirje sisaldab:

- `feed/entry/content/m:properties/d:Name`-Funktsiooni nimi.
- `feed/entry/content/m:properties/d:RankUpdateDate`-Kuupäeva, mille reiting on eraldatud see funktsioon nimega Keskmine värskus funktsiooni. Varasemate kuupäevade ('0001-01-01T00:00:00 ") tähendab, et tehtud pole rank koostamine.
- `feed/entry/content/m:properties/d:Rank`-Funktsiooni rank (hõljumine). Hea funktsiooni käsitletakse 2.0 ja üles soovitud asukoht.
- `feed/entry/content/m:properties/d:SampleValues`-Komaga eraldatud väärtuste loend kuni nõutav valimi suurus

OData

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get the features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


##<a name="11-build"></a>11. Koosta

  Selles jaotises selgitatakse erinevate API-de seotud järgud. On 3 tüüpi järgud: soovitus koostamine, rank koostamine ja mõne Koosta FBT (sageli ostnud koos).

Soovitus koostamine eesmärk on kasutatud prognoose soovitus mudeli loomine. Ennustatud väärtuste (seda tüüpi koostamine) jaoks tulevad kahte tüüpi:
* I2i - nimega Üksuse üksus soovitused - edastatud üksuse või selle suvandi prognoositakse tõenäoliselt kõrge tähtsusega üksuste loendi üksuste loendit.
* U2I - nimega Kasutaja üksuse soovitused - kasutaja id (ja soovi korral üksuste loend) see suvand prognoosida loendi üksused, mis võivad kõrge tähtsusega antud kasutaja (ja selle täiendavate üksuste valik). Soovitused U2I põhinevad üksused, mis olid huvi kasutaja kuni mudeli ehitatud ajalugu.

Rank koostamine on tehnilise Koosta, mis võimaldab teil sellega oma funktsioonide kohta. Tavaliselt seotud funktsioonid soovitus mudeli parima tulemuse saamiseks tuleks teha ka järgmist:
- Rank Koosta käivitamine (välja arvatud teie funktsioonid hinde säilib) ja ootama, kuni kuvatakse funktsiooni Keskmine.
- [Saada funktsioonid Info](#101-get-features-info-for-last-rank-build) API helistades tuua asukoha oma funktsioone.
- Konfigureerima soovitus Koosta järgmiste parameetrite abil:
    - `useFeatureInModel`-Väärtuseks True.
    - `ModelingFeatureList`-Määratud vastava hinde, 2.0 või rohkem (vastavalt saate tuua eelmises etapis) komaga eraldatud loend.
    - `AllowColdItemPlacement`-Väärtuseks True.
    - Soovi korral saate määrata `EnableFeatureCorrelation` True ja `ReasoningFeatureList` funktsioonide loend, mida soovite kasutada selgitusi (tavaliselt sama loendi funktsioonidest, mis on kasutusel modelleerimine või alamloendit).
- Soovitus koostamine konfigureeritud parameetritega käivitamine.

Märkus: Kui konfigureerite parameetrid (nt autonoomsest soovitus koostamine ilma parameetrid) või saate keelata konkreetselt funktsioonide kasutamist (nt `UseFeatureInModel` väärtuseks False), süsteemi häälestamine funktsiooni seotud parameetrid selgitatud ülaltoodud väärtused, juhuks, kui rank koostamine on olemas.

Puudub piirang töötab rank koostamine ja soovitus koostamine üheaegselt sama mudeli jaoks. Siiski ei saa käivitada mudelit paralleelselt kahe järgud sama tüüpi.

Mõne FBT (sageli ostnud koos) on veel mõne muu soovitusi algoritmi nimetatakse mõnikord "konservatiivne" soovitaja, mis on hea kataloogid, mis pole ühtlaste laadi (ühtlaste: raamatud, filmid, mõned toit mood; ebaühtlane: arvuti ja seadmed, domeenide, väga erinevad).

Märkus: kui üleslaaditud failid kasutus sisaldavad valikuline väli "sündmuse tüüp" siis jaoks FBT modelleerimine ainult "Osta" sündmuste kasutatakse. Kui pole sündmuse tüüp on esitatud kõikide sündmuste käsitletakse ostmine.


####<a name="111-build-parameters"></a>11.1 koostada parameetrid

Kindlat tüüpi koostamine saab konfigureerida kogumi parameetrite (allpool kujutatud) kaudu. Kui konfigureerite ei parameetrid, automaatselt atribuut süsteemi andmetel ajal saate käivitada ehitada parameetrite väärtused.

#####<a name="1111-usage-condenser"></a>11.1.1. Kasutus kondensaatori
Kasutajate või mõned kasutus punktid üksused võivad sisaldada rohkem müra kui teavet. Süsteemi püüab prognoosida kasutaja/üksuse kasutada mudeli kasutamine punktide minimaalne arv. See number on määratletud ItemCutoffLowerBound ja ItemCutoffUpperBound parameetrid üksuste vahemikust ja UserCutOffLowerBound ja UserCutoffUpperBound parameetrid kasutajate jaoks määratletud vahemikust. Kondensaatori mõju üksused või kasutaja saab vähendada säte vähemalt üks vastava piirid null.

#####<a name="1112-rank-build-parameters"></a>11.1.2. Järjesta koostada parameetrid

Järgmises tabelis on kujutatud Koosta parameetrid rank koostamine.

|Klahv|Kirjeldus|Tüüp|Sobiv väärtus|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Mudeli sooritab iteratsiooni arv kajastub Arvuta aega ja mudeli täpsuse. Mida suurem arv, on parem täpselt saate, kuid Arvuta aega võtab rohkem aega.| Täisarv | 10-50 |
| NumberOfModelDimensions | Dimensioonide arvu, mis on seotud mitmeid funktsioone, mudeli proovib leida oma andmete jooksul. Arvu mõõtmed võimaldab paremini peenhäälestus tulemuste väiksemate kogumite sisse. Siiski liiga palju mõõtmed takistada mudeli leidmist üksuste vahel. | Täisarv | 10-40 |
|ItemCutOffLowerBound| Määratleb jaoks jahuti üksuse alampiir. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|ItemCutOffUpperBound| Määratleb üksuse ülemist kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|UserCutOffLowerBound| Määratleb kasutaja alampiir kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|UserCutOffUpperBound| Määratleb kasutaja ülemist kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |

#####<a name="1113-recommendation-build-parameters"></a>11.1.3. Soovitus koostada parameetrid
Järgmises tabelis on kujutatud Koosta parameetrid soovitus koostamine.

|Klahv|Kirjeldus|Tüüp|Sobiv väärtus|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Mudeli sooritab iteratsiooni arv kajastub Arvuta aega ja mudeli täpsuse. Mida suurem arv, on parem täpselt saate, kuid Arvuta aega võtab rohkem aega.| Täisarv | 10-50 |
| NumberOfModelDimensions | Dimensioonide arvu, mis on seotud mitmeid funktsioone, mudeli proovib leida oma andmete jooksul. Arvu mõõtmed võimaldab paremini peenhäälestus tulemuste väiksemate kogumite sisse. Siiski liiga palju mõõtmed takistada mudeli leidmist üksuste vahel. | Täisarv | 10-40 |
|ItemCutOffLowerBound| Määratleb jaoks jahuti üksuse alampiir. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|ItemCutOffUpperBound| Määratleb üksuse ülemist kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|UserCutOffLowerBound| Määratleb kasutaja alampiir kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|UserCutOffUpperBound| Määratleb kasutaja ülemist kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
| Kirjeldus | Koostada kirjeldus. | String | Teksti, kuni 512 märki |
| EnableModelingInsights | Saate arvutada mõõdikute soovitus mudel. | Kahendmuutuja | Tõene/Väär |
| UseFeaturesInModel | Näitab, kui funktsioone saab kasutada suurendamiseks soovitus mudel. | Kahendmuutuja | Tõene/Väär |
| ModelingFeatureList | Funktsioon nimed soovitus koostamine, kasutada, et suurendada soovitust komaga eraldatud loend. | String | Funktsioonide nimed, kuni 512 märki |
| AllowColdItemPlacement | Näitab, kui soovitust peaks ka push külma üksuste funktsioon sarnased kaudu. | Kahendmuutuja | Tõene/Väär |
| EnableFeatureCorrelation | Näitab, kui funktsioone saab kasutada arutluskäigus. | Kahendmuutuja | Tõene/Väär |
| ReasoningFeatureList | Funktsioon nimede kasutatakse arutluskäiku lausete (nt soovitus selgitused) komaga eraldatud loend.  | String | Funktsioonide nimed, kuni 512 märki |
| EnableU2I | Luba isikupärastatud soovitus nimega U2I (kasutaja üksuse soovitused). | Kahendmuutuja | Tõene/Väär (vaikimisi true) |

#####<a name="1114-fbt-build-parameters"></a>11.1.4. FBT koostada parameetrid
Järgmises tabelis on kujutatud Koosta parameetrid soovitus koostamine.

|Klahv|Kirjeldus|Tüüp|Sobiv väärtus (vaikeväärtus)|
|:-----|:----|:----|:---|
|FbtSupportThreshold | Kuidas konservatiivne mudel on. Tuleb arvestada modelleerimiseks kaasautorluse esinemiskordade arvu.| Täisarv | 3-50 (6) |
|FbtMaxItemSetSize | Piire sagedased kogumis olevate üksuste arvu.| Täisarv | 2-3 (2) |
|FbtMinimalScore | Minimaalne Keskmine sagedased kogum peaksid olema tagastatud tulemite lisada. Mida suurem on parem.| Kahekordne | 0 ja kohal (0) |
|FbtSimilarityFunction | Määratleb koostamine kasutavad funktsiooni sarnased. Lifti eelistab serendipity, soodustab koostööd esinemiskord prognoositavuse ja Jaccard on kaks kena kompromiss. | String | cooccurrence, lifti jaccard (lift) |


###<a name="112-trigger-a-recommendation-build"></a>11.2. Soovitus Koosta käivitamine

  Vaikimisi käivitab selle API soovitus mudeli koostamine. Rank koostamine (et Keskmine funktsioonid), käivitamiseks Koosta API variant Koosta parameetrit tuleks kasutada.


| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|
|PÄIS   |`"Content-Type", "text/xml"`(Kui see saadetakse koosolekukutse kehasse)|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator  |
| userDescription | Teksti identifikaator kataloogi. Pange tähele, et kui kasutate tühikuid saate peab kodeerida selle 20% selle asemel. Ülaltoodud näites näha.<br>Max pikkus: 50 |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | Kui tühjaks siis koostamine viiakse vaikimisi Koosta parameetrid.<br><br>Kui soovite seada Koosta parameetrid, saata parameetrid XML-i kehasse nagu järgmises näites. (Jaotisest "Ehitada parameetrid" parameetrid selgitused.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>` |

**Vastus**:

HTTP olekukoodi: 200

See on asünkroonne API. Saate koostamine ID vastus. Teada, millal koostamine on lõppenud, peaks kõne "Saada koostab olek on ka mudel" API ja otsige üles see koostamine ID vastuse. Arvestage, et ehitada saavad võtta minutit tundi sõltuvalt andmete maht.

Te ei saa kasutada kuni koostamine soovitused lõpeb.

Lubatud Koosta olek:

- Looge – Koosta taotlus on loodud.
- Ootele - Koosta taotleb ja see on ootele.
- Koosteüksuse - koostamine on pooleli.
- Edu - koostamine lõppes edukalt.
- Tõrge - koostamine lõppes tõrke.
- Tühistatud - koostamine on tühistatud.
- Tühistamine - saadeti Loobu taotluse koostamine.


Pange tähele, et koostamine ID leiate jaotises tee järgmist:`Feed\entry\content\properties\Id`

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

###<a name="113-trigger-build-recommendation-rank-or-fbt"></a>11.3. Päästik koostamine (soovitus, asukoha või FBT)

| HTTP-meetod | URI |
|:--------|:--------|
|POSTITUSE     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27`|
|PÄIS   |`"Content-Type", "text/xml"`(Kui see saadetakse koosolekukutse kehasse)|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator  |
| userDescription | Teksti identifikaator kataloogi. Pange tähele, et kui kasutate tühikuid saate peab kodeerida selle 20% selle asemel. Ülaltoodud näites näha.<br>Max pikkus: 50 |
| buildType | Viidata koostamine tüüp: <br/> -"Soovitus" soovitus koostamine <br> -Järjestamine jaoks rank koostamine <br/> -'Fbt' jaoks FBT koostamine
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | Kui tühjaks siis koostamine viiakse vaikimisi Koosta parameetrid.<br><br>Kui soovite seada ehitada parameetrid, saata need XML-i kehasse umbes järgmist valimis. (Jaotisest "Ehitada parameetrid" selgitus ja parameetrite täieliku loendi jaoks.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>` |

**Vastus**:

HTTP olekukoodi: 200

See on asünkroonne API. Saate koostamine ID vastus. Teada, millal koostamine on lõppenud, peaks kõne "Saada koostab olek on ka mudel" API ja otsige üles see koostamine ID vastuse. Arvestage, et ehitada saavad võtta minutit tundi sõltuvalt andmete maht.

Te ei saa kasutada kuni koostamine soovitused lõpeb.

Lubatud Koosta olek:

- Looge - mudel.
- Ootele - mudeli koostamine käivitati ja see on ootele.
- Koosteüksuse - mudeli ehitatakse.
- Edu - koostamine lõppes edukalt.
- Tõrge - koostamine lõppes tõrke.
- Tühistatud - koostamine on tühistatud.
- Tühistamine - Koosta tühistamisest.

Pange tähele, et koostamine ID leiate jaotises tee järgmist:`Feed\entry\content\properties\Id`

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




###<a name="114-get-builds-status-of-a-model"></a>11.4. Hankige järgud mudeli
Toob järgud ja nende olekut määratud mudel.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|


|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   modelId         |   Kui seate mudeli ainuidentifikaator  |
|   onlyLastBuild   |   Näitab, kas Koosta ajaloo mudeli või ainult olekut viimase koostamine  |
|   apiVersion      |   1.0                                 |


**Vastus**:

HTTP olekukoodi: 200

Vastus sisaldab ühe kande koostamine. Iga kirje on järgmised andmed.

- `feed/entry/content/properties/UserName`– Kasutaja nimi.
- `feed/entry/content/properties/ModelName`-Mudeli nimi.
- `feed/entry/content/properties/ModelId`-Mudeli kordumatut tunnust.
- `feed/entry/content/properties/IsDeployed`– Kas (alias juurutatud koostamine aktiivse järk).
- `feed/entry/content/properties/BuildId`-Koostada kordumatut tunnust.
- `feed/entry/content/properties/BuildType`-Tüüpi koostamine.
- `feed/entry/content/properties/Status`-Koostada olek. Võib olla üks järgmistest: tõrke, hoone, ootel, Cancelling, tühistatud, edu.
- `feed/entry/content/properties/StatusMessage`– Üksikasjalik olekuteade (kehtib ainult teatud olekus).
- `feed/entry/content/properties/Progress`-Koostada edenemist (%).
- `feed/entry/content/properties/StartTime`-Koostada algusaeg.
- `feed/entry/content/properties/EndTime`-Koostada lõppkellaaeg.
- `feed/entry/content/properties/ExecutionTime`-Koostada kestus.
- `feed/entry/content/properties/ProgressStep`-Edenemise build hetkeseisu üksikasjad.

Lubatud Koosta olek:
- Loodud - Koosta taotlus kirje on loodud.
- Ootele - käivitati Koosta taotlus ja see on ootele.
- Koosteüksuse - koostamine on pooleli.
- Edu - koostamine lõppes edukalt.
- Tõrge - koostamine lõppes tõrke.
- Tühistatud - koostamine on tühistatud.
- Tühistamine - Koosta tühistamisest.

Sobivad väärtused Koosta tüüp:
- Asukoht – Rank koostada.
- Soovitus - soovitusteenuse koostamine.


OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


###<a name="115-get-builds-status"></a>11.5. Hangi koostab olek
Toob koostada olekud kõikide mudelite kasutaja.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27`|


|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
|   onlyLastBuild   |   Näitab, kas Koosta ajalugu mudeli või ainult viimase koostamine olekut. |
|   apiVersion      |   1.0                                 |


**Vastus**:

HTTP olekukoodi: 200

Vastus sisaldab ühe kande koostamine. Iga kirje on järgmised andmed.

- `feed/entry/content/properties/UserName`– Kasutaja nimi.
- `feed/entry/content/properties/ModelName`-Mudeli nimi.
- `feed/entry/content/properties/ModelId`-Mudeli kordumatut tunnust.
- `feed/entry/content/properties/IsDeployed`– Kas juurutatakse koostamine.
- `feed/entry/content/properties/BuildId`-Koostada kordumatut tunnust.
- `feed/entry/content/properties/BuildType`-Tüüpi koostamine.
- `feed/entry/content/properties/Status`-Koostada olek. Võib olla üks järgmistest: tõrke, hoone, ootel, tühistatud, Cancelling, edu.
- `feed/entry/content/properties/StatusMessage`– Üksikasjalik olekuteade (kehtib ainult teatud olekus).
- `feed/entry/content/properties/Progress`-Koostada edenemist (%).
- `feed/entry/content/properties/StartTime`-Koostada algusaeg.
- `feed/entry/content/properties/EndTime`-Koostada lõppkellaaeg.
- `feed/entry/content/properties/ExecutionTime`-Koostada kestus.
- `feed/entry/content/properties/ProgressStep`-Edenemise build hetkeseisu üksikasjad.

Lubatud Koosta olek:
- Loodud - Koosta taotlus kirje on loodud.
- Ootele - käivitati Koosta taotlus ja see on ootele.
- Koosteüksuse - koostamine on pooleli.
- Edu - koostamine lõppes edukalt.
- Tõrge - koostamine lõppes tõrke.
- Tühistatud - koostamine on tühistatud.
- Tühistamine - Koosta tühistamisest.


Sobivad väärtused Koosta tüüp:
- Asukoht – Rank koostada.
- Soovitus - soovitusteenuse koostamine.


OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


###<a name="116-delete-build"></a>11.6. Koosta kustutamine
Kustutab ehitada.

MÄRKUS: <br>Te ei saa kustutada ka aktiivne koostamine. Mudeli tuleks ajakohastada, et erinevate aktiivse koostamine enne selle kustutamist.<br>Te ei saa kustutada on pooleli koostamine. Koostamine tuleks esmalt tühistada, helistades <strong>Tühistamine koostamine</strong>.

| HTTP-meetod | URI |
|:--------|:--------|
|KUSTUTAMINE     |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| buildId | Ainuidentifikaator koostamine. |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200

###<a name="117-cancel-build"></a>11,7. Loobu koostamine
Tühistab Koosta, mis on koostamise olek.

| HTTP-meetod | URI |
|:--------|:--------|
|SELLELE     |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| buildId | Ainuidentifikaator koostamine. |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200

###<a name="118-get-build-parameters"></a>11,8. Saada ehitada parameetrid
Toob koostada parameetrid.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| buildId | Ainuidentifikaator koostamine. |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200

See API tagastab kogumi elementide /-väärtuse. Iga element tähistab parameeter ja selle väärtus:
- `feed/entry/content/properties/Key`-Koostada parameetri nimi.
- `feed/entry/content/properties/Value`-Koostada parameetri väärtuse.

Järgmises tabelis on kujutatud väärtus, mis tähistab iga võti.

|Klahv|Kirjeldus|Tüüp|Sobiv väärtus|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Mudeli sooritab iteratsiooni arv kajastub Arvuta aega ja mudeli täpsuse. Mida suurem arv, on parem täpselt saate, kuid Arvuta aega võtab rohkem aega.| Täisarv | 10-50 |
| NumberOfModelDimensions | Dimensioonide arvu, mis on seotud mitmeid funktsioone, mudeli proovib leida oma andmete jooksul. Arvu mõõtmed võimaldab paremini peenhäälestus tulemuste väiksemate kogumite sisse. Siiski liiga palju mõõtmed takistada mudeli leidmist üksuste vahel. | Täisarv | 10-40 |
|ItemCutOffLowerBound| Määratleb jaoks jahuti üksuse alampiir. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|ItemCutOffUpperBound| Määratleb üksuse ülemist kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|UserCutOffLowerBound| Määratleb kasutaja alampiir kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
|UserCutOffUpperBound| Määratleb kasutaja ülemist kondensaatori jaoks. Lugege teemat kasutus kondensaatori ülaltoodud. | Täisarv | 2 või rohkem (0 blokeerida jahuti) |
| Kirjeldus | Koostada kirjeldus. | String | Teksti, kuni 512 märki |
| EnableModelingInsights | Saate arvutada mõõdikute soovitus mudel. | Kahendmuutuja | Tõene/Väär |
| UseFeaturesInModel | Näitab, kui funktsioone saab kasutada suurendamiseks soovitus mudel. | Kahendmuutuja | Tõene/Väär |
| ModelingFeatureList | Funktsioon nimed soovitus koostamine, kasutada, et suurendada soovitust komaga eraldatud loend. | String | Funktsioonide nimed, kuni 512 märki |
| AllowColdItemPlacement | Näitab, kui soovitust peaks ka push külma üksuste funktsioon sarnased kaudu. | Kahendmuutuja | Tõene/Väär |
| EnableFeatureCorrelation | Näitab, kui funktsioone saab kasutada arutluskäigus. | Kahendmuutuja | Tõene/Väär |
| ReasoningFeatureList | Funktsioon nimede kasutatakse arutluskäiku lausete (nt soovitus selgitused) komaga eraldatud loend.  | String | Funktsioonide nimed, kuni 512 märki |


OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

##<a name="12-recommendation"></a>12. soovitus
###<a name="121-get-item-recommendations-for-active-build"></a>12.1. Saamiseks üksuse soovitused (aktiivne järk)

Hankida aktiivse koostamine tüüpi soovitusi "Soovitus" või "Fbt" seemned (Sisestuskeel) üksuste loendil põhineva.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| Kas ItemId-d | Üksuste jaoks soovitada komaga eraldatud loend. <br>Kui aktiivne koostamine on tippige FBT saate saata ainult ühe üksuse. <br>Max pikkus: 1024 |
| numberOfResults | Nõutav tulemite arv <br> Max: 150 |
| includeMetatadata | Edaspidiseks kasutamiseks,. alati väärtus false |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200


Vastus sisaldab ühte kirjet soovitatavaid üksuse kohta. Iga kirje on järgmised andmed.
- `Feed\entry\content\properties\Id`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name`– Üksuse nimi.
- `Feed\entry\content\properties\Rating`-Reiting soovitus; suurem arv tähendab, et kõrgema confidence.
- `Feed\entry\content\properties\Reasoning`-Soovitus arutluskäiku (nt soovitus selgitused).

Alltoodud näites vastus sisaldab 10 Soovitatavad üksused.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

###<a name="122-get-item-recommendations-of-a-specific-build"></a>12.2. Hangi üksuse soovitusi (mõne kindla järk).

Saada teatud Koosta tüüpi "Soovitus" või "Fbt" soovitusi.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| Kas ItemId-d | Üksuste jaoks soovitada komaga eraldatud loend. <br>Kui aktiivne koostamine on tippige FBT saate saata ainult ühe üksuse. <br>Max pikkus: 1024 |
| numberOfResults | Nõutav tulemite arv <br> Max: 150  |
| includeMetatadata | Edaspidiseks kasutamiseks,. alati väärtus false
| buildId | kasutada soovitus selle päringu koostamine id |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200


Vastus sisaldab ühte kirjet soovitatav üksuse kohta. Iga kirje on järgmised andmed.
- `Feed\entry\content\properties\Id`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name`– Üksuse nimi.
- `Feed\entry\content\properties\Rating`-Reiting soovitus; suurem arv tähendab, et kõrgema confidence.
- `Feed\entry\content\properties\Reasoning`-Soovitus arutluskäiku (nt soovitus selgitused).

Vt 12.1 mõne vastuse näide

###<a name="123-get-fbt-recommendations-for-active-build"></a>12.3. Saamiseks FBT soovitused (aktiivne järk)

Saada soovitusi aktiivne koostamine "Fbt" põhjal seemne (input) üksuse tüüp.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| itemId | Üksuse jaoks soovitada. <br>Max pikkus: 1024 |
| numberOfResults | Nõutav tulemite arv <br>Max: 150 |
| minimalScore | Minimaalne Keskmine, mis peaksid olema sagedased määramine, et lisada tagastatud tulemite |
| includeMetatadata | Edaspidiseks kasutamiseks,. alati väärtus false |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200


Vastus sisaldab ühe kande soovitatav üksuse määramine (seatud üksused, mis on tavaliselt ostnud koos seemne/sisendit üksus). Iga kirje on järgmised andmed.
- `Feed\entry\content\properties\Id1`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name1`– Üksuse nimi.
- `Feed\entry\content\properties\Id2`2 -. soovitatav üksuse ID (valikuline).
- `Feed\entry\content\properties\Name2`-2 (valikuline) üksuse nimi.
- `Feed\entry\content\properties\Rating`-Reiting soovitus; suurem arv tähendab, et kõrgema confidence.
- `Feed\entry\content\properties\Reasoning`-Soovitus arutluskäiku (nt soovitus selgitused).

Alltoodud näites vastus sisaldab 3 soovitatav üksuse komplekti.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="124-get-fbt-recommendations-of-a-specific-build"></a>12.4. Hangi FBT soovitusi (mõne kindla järk).

Saada teatud Koosta tüüpi "Fbt" soovitusi.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| itemId | Üksuse jaoks soovitada. <br>Max pikkus: 1024 |
| numberOfResults | Nõutav tulemite arv <br>Max: 150 |
| minimalScore | Minimaalne Keskmine, mis peaksid olema sagedased määramine, et lisada tagastatud tulemite |
| includeMetatadata | Edaspidiseks kasutamiseks,. alati väärtus false |
| buildId | kasutada soovitus selle päringu koostamine id |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200


Vastus sisaldab ühe kande soovitatav üksuse määramine (seatud üksused, mis on tavaliselt ostetud seemne/sisendit üksus koos). Iga kirje on järgmised andmed.
- `Feed\entry\content\properties\Id1`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name1`– Üksuse nimi.
- `Feed\entry\content\properties\Id2`2 -. soovitatav üksuse ID (valikuline).
- `Feed\entry\content\properties\Name2`-2 (valikuline) üksuse nimi.
- `Feed\entry\content\properties\Rating`-Reiting soovitus; suurem arv tähendab, et kõrgema confidence.
- `Feed\entry\content\properties\Reasoning`-Soovitus arutluskäiku (nt soovitus selgitused).

Vt 12.3 mõne vastuse näide

###<a name="125-get-user-recommendations-for-active-build"></a>12.5. Hankida kasutaja soovitused (aktiivne järk)

Saada kasutaja soovitusi Koosta tüüpi "Soovitus" märgitud aktiivne koostamine.

API tagastab loendi prognoositud üksuse vastavalt kasutuse ajalugu kasutaja.

Märkmed: 
 1. Ei ole kasutaja soovitust FBT ehitada.
 2. Kui aktiivne koostamine on see meetod on FBT tagastab tõrke.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| Kasutajanimi  | Kasutaja ainuidentifikaator |
| numberOfResults | Nõutav tulemite arv |
| includeMetatadata | Edaspidiseks kasutamiseks,. alati väärtus false |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200


Vastus sisaldab ühte kirjet soovitatavaid üksuse kohta. Iga kirje on järgmised andmed.
- `Feed\entry\content\properties\Id`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name`– Üksuse nimi.
- `Feed\entry\content\properties\Rating`-Reiting soovitus; suurem arv tähendab, et kõrgema confidence.
- `Feed\entry\content\properties\Reasoning`-Soovitus arutluskäiku (nt soovitus selgitused).

Vt 12.1 mõne vastuse näide

###<a name="126-get-user-recommendations-with-item-list-for-active-build"></a>12.6. Kasutaja soovitused võtta loendist üksus (aktiivne järk) jaoks

Saada kasutaja soovitusi Koosta tüüpi "Soovitus" märgitud aktiivne koostamine täiendavate üksuste loendiga

API tulemuseks prognoositud üksuse vastavalt kasutuse ajalugu kasutaja ja täiendavate esitatud üksuste loendit.

Märkmed: 
 1. Ei ole kasutaja soovitust FBT ehitada.
 2. Kui aktiivne koostamine on see meetod on FBT tagastab tõrke.


| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| Kasutajanimi  | Kasutaja ainuidentifikaator |
| itemsIds | Üksuste jaoks soovitada komaga eraldatud loend. Max pikkus: 1024 |
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

Vt 12.1 mõne vastuse näide

###<a name="127-get-user-recommendations--of-a-specific-build"></a>12.7. Saada kasutaja soovitusi (mõne kindla järk).

Saada teatud Koosta tüüpi "Soovitus" kasutaja soovitusi.

API tagastab loendi prognoositud üksuse vastavalt kasutuse ajalugu kasutaja (kasutatakse teatud koostamine).

Märkus: On kasutaja ei saa anda FBT koostamine.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27`|


|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| Kasutajanimi  | Kasutaja ainuidentifikaator |
| numberOfResults | Nõutav tulemite arv |
| includeMetatadata | Edaspidiseks kasutamiseks,. alati väärtus false |
| buildId | kasutada soovitus selle päringu koostamine id |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200


Vastus sisaldab ühte kirjet soovitatav üksuse kohta. Iga kirje on järgmised andmed.
- `Feed\entry\content\properties\Id`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name`– Üksuse nimi.
- `Feed\entry\content\properties\Rating`-Reiting soovitus; suurem arv tähendab, et kõrgema confidence.
- `Feed\entry\content\properties\Reasoning`-Soovitus arutluskäiku (nt soovitus selgitused).

Vt 12.1 mõne vastuse näide


###<a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a>12,8. Kasutaja soovitused võtta üksuse loend (mõne kindla järk)

Saada kasutaja soovitused teatud Koosta tüüpi "Soovitus" ja täiendavate üksuste loendit.

API tulemuseks loendi prognoositud üksuse vastavalt kasutuse ajalugu kasutaja ja täiendavate üksuste loendit.

Märkus: Tthere on kasutaja ei saa anda FBT koostamine.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27`|



|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| Kasutajanimi  | Kasutaja ainuidentifikaator |
| Kas ItemId-d | Üksuste jaoks soovitada komaga eraldatud loend. Max pikkus: 1024 |
| numberOfResults | Nõutav tulemite arv |
| includeMetatadata | Edaspidiseks kasutamiseks,. alati väärtus false |
| buildId | kasutada soovitus selle päringu koostamine id |
| apiVersion | 1.0 |

**Vastus:**

HTTP olekukoodi: 200


Vastus sisaldab ühte kirjet soovitatav üksuse kohta. Iga kirje on järgmised andmed.
- `Feed\entry\content\properties\Id`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name`– Üksuse nimi.
- `Feed\entry\content\properties\Rating`-Reiting soovitus; suurem arv tähendab, et kõrgema confidence.
- `Feed\entry\content\properties\Reasoning`-Soovitus arutluskäiku (nt soovitus selgitused).

Vt 12.1 mõne vastuse näide

##<a name="13-user-usage-history"></a>13. kasutaja kasutus ajalugu
Pärast soovitus mudeli ehitatud süsteem võimaldab kasutaja ajalugu (üksused, mis on seotud mõne kindla kasutaja) toomiseks kasutada koostamine.
See API luba tuua kasutaja ajalugu

Märkus: kasutaja ajaloo on praegu saadaval ainult soovitus järgud.

###<a name="131-retrieve-user-history"></a>13.1 tuua kasutaja ajalugu
Saate tuua loendis üksuse oli aktiivne koostamine või määratud koostamine antud kasutaja ID-d.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     | Saate aktiivse koostamine kasutaja ajalugu.<br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/>Kasutaja ajalugu saamiseks antud koostamine`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`<br/><br/>Näide:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`|


|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator.|
| Kasutajanimi | kasutaja ainuidentifikaator.
| buildId | valikuline parameeter, näitamaks, millised koostamine tuleks kasutaja ajaloo toomise lubamine
| apiVersion | 1.0 |


**Vastus:**

HTTP olekukoodi: 200

Vastus sisaldab ühte kirjet soovitatav üksuse kohta. Iga kirje on järgmised andmed.
- `Feed\entry\content\properties\Id`-Soovitatav üksuse ID-ga.
- `Feed\entry\content\properties\Name`– Üksuse nimi.
- `Feed\entry\content\properties\Rating`-N/A.
- `Feed\entry\content\properties\Reasoning`-N/A.

OData XML-i

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

##<a name="14-notifications"></a>14. teatiste
Azure'i masina õ soovitusi loob teatisi, kui püsivate tõrkeid ilmneda süsteemi. On 3 tüüpi teatised.
1.  Koosta tõrge – see teatis käivitatakse iga Koosta tõrge.
2.  Andmete kogumine töötlemine tõrge – see teatis käivitatakse, kui meil on rohkem kui 100 tõrgete viimase viie minuti töötlemise kasutussündmusi mudeli kohta.
3.  Soovitus tarbimine tõrge – see teatis käivitatakse, kui meil on rohkem kui 100 tõrgete viimase viie minuti töötlemise soovitus taotluste mudeli kohta.


###<a name="141-get-notifications"></a>14.1. Saada teatisi
Toob kõik teatised kõigi või ühe mudel.

| HTTP-meetod | URI |
|:--------|:--------|
|HANKIMINE     |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Saada kõik teatised kõigi:<br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br>Näide konkreetse mudeli teatiste saamise:<br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277`|


|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Valikuline parameeter. Kui välja jäetud, kuvatakse kõik teatised kõigi. <br>Sobiv väärtus: mudeli ainuidentifikaator.|
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus:**

HTTP olekukoodi: 200

OData XML-i

    The response includes one entry per notification. Each entry has the following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

###<a name="142-delete-model-notifications"></a>14.2. Mudeli teatiste kustutamine
Kustutab kõik loetud teatiste mudel.

| HTTP-meetod | URI |
|:--------|:--------|
|KUSTUTAMINE     |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br>Näide:<br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27`|


|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| modelId | Kui seate mudeli ainuidentifikaator |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200

###<a name="143-delete-user-notifications"></a>14.3. Kasutaja teatiste kustutamine
Kustutab kõik teatised kõigi.

| HTTP-meetod | URI |
|:--------|:--------|
|KUSTUTAMINE     |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27`|


|   Parameetri nimi  |   Kehtivad väärtused                        |
|:--------          |:--------                              |
| apiVersion | 1.0 |
|||
| Koosolekukutse kehasse. | ÜKSKI |

**Vastus**:

HTTP olekukoodi: 200




##<a name="15-legal"></a>15. juriidiline
Selles dokumendis on esitatud "nagu-on". Teavet ja arvamusi selles dokumendis, sh URL ja muud veebilehtede viited, võivad olla järgmised.<br><br>
Mõned näited siin kujutatud on mõeldud ainult joonisel ja on väljamõeldud. Pole otse ühendus või ühendus on mõeldud või tohiks seostada.<br><br>
Selle dokumendi ei anna teile mis tahes õigusi seda intellektuaalomandi tootes Microsoft. Võite kopeerida ja kasutada seda dokumenti oma asutuse siseseks teatmematerjalina.<br><br>
© 2015 Microsoft. Kõik õigused.
 
