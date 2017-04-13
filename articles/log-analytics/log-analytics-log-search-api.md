<properties
    pageTitle="Log Analytics logige search REST API | Microsoft Azure'i"
    description="Sellest juhendist leiate põhilised õpetuse, mis kirjeldab, kuidas saate kasutada Log Analytics search REST API-ga sisse toimingud halduse komplekti (OMS) ja pakub näiteid, mis näitab, kuidas kasutada käsud."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Log Analytics log search REST API-ga

Sellest juhendist leiate põhilised õpetuse, mis kirjeldab, kuidas saate kasutada Log Analytics Search REST API sisse toimingud halduse komplekti (OMS) ja see antakse ülevaade, mis näitab, kuidas kasutada käsud. Mõned näited selle artikli viide töö ülevaated, mis on eelmise versiooni Log Analytics nimi.

## <a name="overview-of-the-log-search-rest-api"></a>Log Search REST API ülevaade

Log Analytics Search REST API on RESTful ja pääseb Azure'i ressursihaldur API-ga. Selle dokumendi leiate näiteid kus API pääseb juurde selle [ARMClient](https://github.com/projectkudu/ARMClient), avage allikas käsurea tööriista, mis lihtsustab kasutada Azure ressursihaldur API. ARMClient ja PowerShelli kasutamine on üks Log Analytics otsingu API juurdepääsu palju võimalusi. Teine võimalus on kasutada Azure PowerShelli moodul OperationalInsights, mis sisaldab juurdepääsuks otsingu cmdlet-käsud. Nende tööriistade abil saate kasutada rahulik Azure'i ressursihaldur API helistada OMS tööruumide ja käsud neis tegemiseks. API väljund otsingutulemite teile JSON-vormingus, mis võimaldab teil kasutada otsingutulemite programmiliselt mitmel erineval viisil.

Azure'i ressursihaldur saab kasutada [teegi .net-i](https://msdn.microsoft.com/library/azure/dn910477.aspx) kui ka mõni [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx)kaudu. Vaadake lisateavet lingitud veebilehtede.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Tavaline Log Analytics Search REST API õpetus

### <a name="to-use-the-arm-client"></a>ARM klientrakenduse kasutamiseks

1. Installige [Chocolatey](https://chocolatey.org/), mis on avatud allika paketi Manager for Windows. Avage käsuviiba aken administraatorina ja seejärel käivitage järgmine käsk:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Installige soovitud ARMClient, käivitage järgmine käsk:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Lihtsa otsingu abil soovitud ARMClient tegemiseks

1. Logi sisse oma Microsofti või OrgID kontole:

    ```
    armclient login
    ```

    Edukat sisselogimist loetleb kõik antud kontoga seotud tellimuste.

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Kuvada tööruumid Operations Management Suite.

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Eduka toomine kõne oleks väljasta kõik tööruumid tellimusega seotud:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Looge oma otsingu muutujana.

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Abil oma uue otsingu muutujana otsimiseks tehke järgmist.

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Logige Analytics Search REST API viide näited
Järgmised näited kujutavad, kuidas saate kasutada otsingu API.

### <a name="search---actionread"></a>Otsingu - toimingu/lugemine

**Valimi Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Taotlemiseks tehke järgmist.**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Järgmises tabelis kirjeldatakse atribuute, mis on saadaval.

|**Atribuut**|**Kirjeldus**|
|---|---|
|algusse|Suurim lubatud arv tulemite tagastamiseks.|
|Tõstke esile|Enne ja pärast parameetrite, tavaliselt kasutatakse kattuvad väljad esiletõst sisaldab|
|enne|Eesliidete antud stringi oma vastendatud väljad.|
|postituse|Lisab teie vastendatud väljade antud string.|
|päringu|Otsingupäringu kasutatud koguda ja tulemeid.|
|Alustamine|Soovite leida tulemuste aknas alguses.|
|Lõpeta|Soovitud tulemuste leida aega akna lõppu.|


**Vastus:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Otsige / {ID} - toimingu/lugemine

**Taotlege salvestatud otsingu sisu.**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Kui otsing tagastab 'Ootel' olekuga, siis küsitlused värskendatud tulemused saab teha seda API kaudu. Pärast 6 min, Otsingu tulem kõrvaldatakse vahemälu ja tagastatakse HTTP läinud. Kui algset Otsingutaotluse tagastatud 'Eduka' olek kohe, ei lisata seda vahemällu põhjustab selle API HTTP läinud tagastamiseks juhul, kui esitatakse selle kohta päring. Sisu on HTTP 200 tulem on algse Otsingutaotluse lihtsalt väärtustega värskendatud sama vormindamine.

### <a name="saved-searches---rest-only"></a>Salvestatud otsingud - ülejäänud ainult

**Taotlege salvestatud otsingute loendit.**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Toetatud meetodite: GET sellele kustutamine

Toetatud saidikogumi võimalust: hankimine

Järgmises tabelis kirjeldatakse atribuute, mis on saadaval.

|Atribuut|Kirjeldus|
|---|---|
|ID|Ainuidentifikaator.|
|Etag|**Paikade jaoks nõutav**. Serveri värskendada iga kirjutada. Väärtus peab olema talletatud praeguse väärtuse või "*" värskendada. vanade/kehtetu väärtuste puhul tagastatakse 409.|
|Properties.Query|**Nõutav**. Otsingupäringu.|
|properties.displayName|**Nõutav**. Kasutaja määratletud kuvatav nimi päringu. Kui modelleerida, nagu on Azure ressurss, et see oleks sildi.|
|Properties.Category|**Nõutav**. Kasutaja määratletud kategooria päring. Kui modelleerida nagu on Azure ressurss oleks sildi.|

>[AZURE.NOTE] Log Analytics otsingu API praegu tagastab kasutaja loodud salvestatud otsingud küsitluste salvestatud otsingute tööruumis. API ei tagasta esitatud lahenduste praegu salvestatud otsinguid. See funktsioon lisatakse hiljem.

### <a name="create-saved-searches"></a>Salvestatud otsingud loomine

**Taotlemiseks tehke järgmist.**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Kustuta salvestatud otsingud

**Taotlemiseks tehke järgmist.**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Värskenduse, mis on salvestatud otsingud

 **Taotlemiseks tehke järgmist.**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metaandmete - JSON ainult

Siin on võimalus kuvada väljad Logi kõigi oma tööruumis kogutud andmete jaoks. Näiteks, kui soovite, et te ei tea, kas sündmuse tüüp on väli nimega arvuti, siis see on üks võimalus otsida ja kinnitage.

**Kutse väljad:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Vastus:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Järgmises tabelis kirjeldatakse atribuute, mis on saadaval.

|**Atribuut**|**Kirjeldus**|
|---|---|
|Nimi|Välja nimi.|
|displayName|Välja kuvatav nimi.|
|tüüp|Välja väärtus tüüp.|
|facetable|Kombinatsiooni praeguse "indekseeritud", "salvestatud" ja "tahukas" atribuudid.|
|kuvamine|Praeguse "Kuva atribuut. True, kui väli on nähtav väljale Otsi.|
|ownerType|Vähendada ainult tüüpi kuuluvate onboarded IP-aadressi.|


## <a name="optional-parameters"></a>Valikulised parameetrid
Järgmine teave kirjeldatakse valikuliste parameetrite saadaval.

### <a name="highlighting"></a>Esiletõstmine

"Tõsta" parameeter on valikuline parameeter, saate taotleda search alamsüsteemi kogumi markerid vastuses.

Need markerid näitavad käivitamine ja lõppkuupäeva esiletõstetud tekst, mis vastab teie otsingupäringu esitatud tingimustele.
Võite määrata algus- ja tähistega, mis kasutab otsing mähkimiseks esiletõstetud termin.

**Otsingupäringu näide**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Proovi tulemus:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Arvestage, et tulemus ülaltoodud sisaldab kuvatakse tõrketeade, mis on eelkinnitatud ja lisatud.

## <a name="computer-groups"></a>Arvuti rühmad

Arvuti rühmad on eriline salvestatud otsinguid, mis tagastavad arvutite.  Saate arvuti rühma teiste päringute arvutisse jaotises tulemeid piiritleda.  Arvuti rühm on salvestatud otsingu väärtus on arvuti sildiga rühma rakendada.

Järgmine on näide arvuti rühma.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Arvuti rühmad toomine

Arvuti rühma toomiseks kasutada Get meetodit rühma ID-ga.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Luua või rühma arvuti värskendamine

Abil saate panna meetodit kordumatu salvestatud otsingu ID arvuti uue rühma loomine. Kui kasutate arvuti olemasoleva rühma ID, siis üks on muudetud. Arvuti rühma loomisel OMS konsooli seejärel ID luuakse rühma-ja nimi.

Rühma määratlemiseks kasutatud päringu peab andma arvutite rühma õigesti.  Soovitatav on oma päringu *lõpetamist | Erinevate arvuti* õigete andmete tagastamise kindlustamiseks.

Salvestatud otsingu määratlus peab sisaldama sildi väärtus on arvuti otsingu liigitada arvuti jaotises rühma nimega.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Arvuti rühmad kustutamine

Kasutage Kustuta meetodit rühma ID-ga arvutis rühma kustutamiseks.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [log otsinguid](log-analytics-log-searches.md) koostamiseks päringute kohandatud väljade jaoks kriteeriumide abil.
