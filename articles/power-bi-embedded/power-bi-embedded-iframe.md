<properties
   pageTitle="Kuidas kasutada Power BI manustatud ülejäänud | Microsoft Azure'i"
   description="Siit saate teada, kuidas kasutada Power BI manustatud ülejäänud "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Kuidas kasutada Power BI manustatud ülejäänud


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI manustatud: Mis see on ja mis on
Power BI manustatud ülevaade on kirjeldatud ametlik [Power BI manustatud saidi](https://azure.microsoft.com/services/power-bi-embedded/), kuid vaatame kiirülevaate enne me sattuda funktsiooniga ülejäänud üksikasjad.

See on tegelikult üsna lihtne. Mõne ISV sageli soovib kasutada oma rakenduses [Power](https://powerbi.microsoft.com) bi dünaamiline visualiseeringud UI koosteüksused.

Aga, teate, Power BI aruanded või paanid manustamine oma veebilehele sisse on juba ilma Power BI manustatud Azure'i teenus **Power BI API**abil. Kui soovite oma ettevõtte sama aruannete ühiskasutus, saate manustada aruannete Azure AD autentimist. Kasutaja, kes vaatavad aruannete peab logige sisse oma konto Azure AD. Kui soovite oma aruannete (sh välistele kasutajatele) kõigi kasutajate jaoks ühiskasutusse anda, saate manustada lihtsalt anonüümse juurdepääsuga.

Näete, see lihtne manustamine lahenduse, kuid ei tundu hästi rakenduse ISV vajadustele.
Enamik ISV taotluste vaja pakkuda oma klientidele, ei pruugi oma asutuse kasutajate andmed. Näiteks kui kasutate pakkuda mõni ettevõte A ja B ettevõtte teenuse, kasutajate ettevõttes A ainult, lugege artiklit andmete oma ettevõtte A. Mis on mitme rendiõigus on vaja lugejateni toimetamiseks.

ISV rakendus võib ka pakkuda oma autentimismeetodite näiteks vormide auth, tavaline auth.. Seejärel krae lahendus peab koostööd selle olemasoleva autentimismeetodite turvaline. Samuti on vaja kasutada ISV taotluste ilma eest või hulgilitsentsimise Power BI tellimuse kasutajate jaoks.

 **Power BI manustatud** on mõeldud täpselt selliseid ISV stsenaariumid. Nii nüüd kus oleme ära selle tutvustuse, vaatame tuua mõned üksikasjad

Saate kasutada .NET \(C#) või Node.js Tarkvaraarenduskomplektist, kerge vaevaga luua rakenduse Power BI manustatud abil. Pärast seda, kuid selles artiklis selgitame kohta HTTP meilivoo \(sh AuthN) Power BI ilma SDK-d. See vool mõistmine, saate koostada oma rakenduse **programmeerimise keel, mille**ja saavad aru sügavalt sisuliselt Power BI manustatud.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Saidikogumi loomine Power BI tööruumi ja saada juurdepääsu võti \(Provisioning)
Power BI manustatud on üks Azure teenuseid. Ainult ISV, kes kasutab Azure portaali on ostmisega kasutuse tasud \(tunni kasutaja seansi kohta), ja kasutajale, kes aruande ei lisandu või isegi nõuab Azure tellimust.
Enne alustamist meie rakenduste arendamise, peab me luua **Power BI tööruumi saidikogumi** Azure portaali kaudu.

Iga tööruum on Power BI manustatud on iga kliendi (rentnik) tööruumi ja lisame palju tööruumide iga saidikogumi tööruumi. Sama kiirklahv kasutatakse iga saidikogumi tööruumi. Efekti sisse tööruumi kogumine on turvalisus piiri Power BI manustatud jaoks.

![](media\power-bi-embedded-iframe\create-workspace.png)

Kui soovime saidikogumi tööruumi loomise lõpetanud, kopeerige kiirklahv Azure portaali.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Saame ka saidikogumi tööruumi ettevalmistamine ja saada kaudu REST API võti. Lisateavet leiate teemast [Power BI ressursi pakkuja API -de](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Power BI Desktopi .pbix faili loomine
Järgmiseks peate looma andmeühenduse ja aruannete abil saate manustada.
Selle toimingu jaoks ei ole programmeerimise või kood. Kasutame ainult Power BI Desktopi.
Selles artiklis me ei lähe teavet selle kohta, kuidas kasutada Power BI Desktopi kaudu. Kui vajate abi siin, lugege teemat [Power BI Desktopi töötamise alustamine](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). Selles näites kasutame lihtsalt [Näidis Retail analüüsi](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Power BI tööruumi loomine

Nüüd, kui selle ettevalmistamise on kõik teinud, Alustagem kliendi tööruumi loomine rakenduses tööruumi saidikogumi REST API-de kaudu. Järgmine HTTP POST taotlemine (ülejäänud) on meie olemasoleva tööruumi saidikogumi uue tööruumi loomine. Selles näites on tööruumi saidikogumi nimi **mypbiapp**.
Juurdepääs võtme, mis varem koopia, seadmiseks lihtsalt **AppKey**nimega. See on väga lihtsa autentimise!

**HTTP-päring**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-vastus**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Tagastatud **workspaceId** kasutatakse järgmist API ajakava. Meie rakenduse säilitama selle väärtuse.

## <a name="import-pbix-file-into-the-workspace"></a>Tööruumi .pbix faili importimine
Iga tööruum majutada ühe Power BI Desktopi faili andmekomplekt \(sh andmeallika sätted) ja aruandeid. Me tööruumi, nagu on näidatud allpool kood meie .pbix faili importida. Nagu näete, me kahendarvuks MIME multipart kasutamine http .pbix faili üles laadida.

Uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** on selle workspaceId ja päringu parameetrite **datasetDisplayName** on andmekomplekti nime loomiseks. Loodud andmekomplekti hoiab kõik andmed seotud esemeid .pbix fail, näiteks imporditud andmete kursor andmeallika, jne …

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Selle ülesande importimine võivad töötada samal ajal. Kui olete lõpetanud, meie rakenduse tööülesande oleku impordi id abil esitada. Selles näites on impordi id **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Järgmine küsib olek importimine see id abil:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Kui tööülesanne pole täielik, HTTP vastus võib olla umbes järgmine:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Kui tööülesanne on lõpule jõudnud, HTTP vastus võib olla mitu umbes järgmine:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Andmeallika ühenduvuse \(ja mitme kehtivaid andmeid)
Kuigi peaaegu kõik esemeid .pbix failis imporditakse meie tööruumi, andmeallikate identimisteabe ei ole. Selle tulemusena **DirectQuery režiimi**kasutades manustatud aruannet ei saa õigesti kuvada. Kuid **impordi režiimi**kasutades me saame vaadata aruande olemasoleva imporditud andmete kasutamine. Sellisel juhul peab Määrake mandaat, täitke järgmised juhised ülejäänud kõnede kaudu.

Esmalt saame lüüsi andmeallikas. Me leida andmekomplekti **id** on varem tagastatud ID-d.

**HTTP-päring**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-vastus**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Tagastatud lüüsi id ja andmeallika id \(kuvatakse eelmise **gatewayId** ja tagastatud tulem **id** ), saame muuta selle andmeallika mandaati järgmiselt:

**HTTP-päring**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-vastus**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

Valmistamisel, saame ka määrata eri ühendusstringi iga tööruumi REST API abil. \(st me eraldi andmebaasi iga klientide.)

Andmeallika kaudu ülejäänud ühendusstring on muutunud.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Või rea turvalisuse kasutame rakenduses Power BI manustatud ja me eraldi iga kasutajate ühe aruande andmed. AS a result, saame saate ettevalmistamise iga kliendi aruande sama .pbix \(UI, jne.) ja erinevatest andmeallikatest.

> [AZURE.NOTE] Kui kasutate **impordi režiimi** **DirectQuery režiimi**asemel, on võimalus värskendada API kaudu. Ja kohapealse andmeallikatele Power BI lüüsi kaudu pole veel toetatud Power BI manustatud. Siiski saate tõesti soovite hoida silma peal, mis on uut [Power BI ajaveebi](https://powerbi.microsoft.com/blog/) ja mis peagi tulevikus välja.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Autentimis- ja hosting (krae) aruannete veebis

Eelmise REST API-ga saame kasutada kiirklahv **AppKey** ise kui autoriseerimine päis. Kuna need kõned võib käsitleda kirjutamata serveripoolse, on ohutu.

Kuid kui me manustamine aruande veebis, seda tüüpi turbeteave tuleks käsitleda JavaScripti \(frontend). Klõpsake väärtust luba päise peavad olema kinnitatud. Kui meie kiirklahv on pahatahtlik kasutaja või pahatahtlik kood, nad saavad helistada mis tahes toimingute abil see võti.

Kui me manustamine aruande veebis, peate kasutame arvutatud luba kiirklahv **AppKey**asemel. Meie rakenduse peate looma OAuthi Json Web Turbeloa \(JWT) mis koosneb nõudeid ja arvutatud digitaalallkiri. Nagu on näidatud allpool, on see OAuthi JWT punkti eraldatud encoded stringi sõned.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Esmalt me koostama sisendväärtust, millele on hiljem alla. See väärtus on järgmine json string base64 url-kodeeringuga (rfc4648) ja need on piiratud punkti \(.) märgi. Hiljem selgitame aruande id hankimine.

> [AZURE.NOTE] Kui soovime kasutada Power BI manustatud rea taseme turvalisus (RLS), saame peate määrama ka **kasutajanime** ja **rollide** nõuded.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Järgmiseks loome peab base64-kodeeringuga stringi, HMAC-i \(allkirja) algoritmi SHA256. See kehtib Sisestuskeel väärtus on eelmine string.

Lõpuks me peab kombineerida sisestatav väärtus ja allkirja string abil perioodi \(.) märgi. Lõpetatud string on rakenduse sõnet aruande manustamine. Isegi juhul, kui app luba avastatakse pahatahtlik kasutaja, nad ei saa Accessi tootevõtme. See rakendus luba aegub kiiresti.

Siin on PHP näiteks järgmist:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Lõpuks manustamine veebilehele aruanne

Manustamise meie aruande, tuleb saada manustamine url ja aruande **id** abil järgmine REST API-ga.

**HTTP-päring**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-vastus**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Me saate manustada aruande meie web Appi abil eelmise app luba.
Kui vaatame järgmise proovi kood, endise osa on sama, mis eelmises näites. Viimases osas, kuvatakse see valimi **embedUrl** \(eelmise tulemi vaatamiseks) IFRAME, ja on postitamisel app luba IFRAME'i sisse.

> [AZURE.NOTE] Peate mõne enda aruande id väärtust muuta. Sisuhaldus süsteemi vea tõttu IFRAME'i silt proovi kood on lugege ka sõna-sõnalt. Kui kopeerite ja kleebite selle proovi kood sildi eemaldada ülemmäära tekst.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

Ja siin on meie tulemus:

![](media\power-bi-embedded-iframe\view-report.png)

Sel ajal, Power BI manustatud kuvatakse ainult aruande IFRAME'i. Kuid hoida silma peal [Power BI ajaveeb](). Tulevaste täiustused kasutada uut kliendipoolse API-d, mis aitab andke meile saata teabe importimine IFRAME'i samuti saada teavet välja. Köitva asjade!


## <a name="see-also"></a>Vt ka
- [Autentimist ja lubab rakenduses Power BI manustatud](power-bi-embedded-app-token-flow.md)
