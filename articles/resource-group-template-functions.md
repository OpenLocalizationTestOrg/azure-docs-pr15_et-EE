<properties
   pageTitle="Ressursihaldur malli funktsioonid | Microsoft Azure'i"
   description="Kirjeldatakse funktsioone kasutada Azure ressursihaldur mallis tuua väärtused, töötavad stringide ja numerics ja tuua juurutamine."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-template-functions"></a>Azure'i ressursihaldur malli funktsioonid

Selles teemas kirjeldatakse kõik funktsioonid, mida saate kasutada Azure ressursihaldur mallis.

Malli funktsioonid ja nende parameetrid on väiketähed. Näiteks ressursihaldur lahendab **variables('var1')** ja **VARIABLES('VAR1')** sama nimega. Kui hinnata, kui funktsioon muudab selgesõnaliselt juhul (nt toUpper või toLower), funktsioon säilitab täheregistrit. Teatud ressursi võib-olla juhul nõuded olenemata sellest, kuidas funktsioonid hinnatakse.

## <a name="numeric-functions"></a>Arvuline funktsioonid

Ressursihaldur pakub täisarvude töötamiseks järgmisi funktsioone:

- [lisamine](#add)
- [copyIndex](#copyindex)
- [DIV](#div)
- [int](#int)
- [mod](#mod)
- [Mul](#mul)
- [sub](#sub)


<a id="add" />
### <a name="add"></a>lisamine

**Lisage (operand1 operand2)**

Tagastab kahe esitatud täisarvude summa.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| operand1                           |   Jah    | Esimese täisarv lisada.
| operand2                           |   Jah    | Teine täisarv lisada.

Järgmises näites liidetakse kaks parameetrid.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to add"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to add"
        }
      }
    },
    ...
    "outputs": {
      "addResult": {
        "type": "int",
        "value": "[add(parameters('first'), parameters('second'))]"
      }
    }

<a id="copyindex" />
### <a name="copyindex"></a>copyIndex

**copyIndex(offset)**

Tagastab praeguse indeks on iteratsiooni tsükkel. 

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| OFFSET                           |   Ei    | Summa lisamiseks iteratsiooni praegust väärtust.

See funktsioon on alati kasutada objekti **kopeerimine** . Kohta, kuidas kasutada **copyIndex**täieliku kirjelduse leiate teemast [mitmes eksemplaris ressursside Azure'i ressursihaldur loomine](resource-group-create-multiple.md).

Järgmises näites on kujutatud Kopeeri tsükkel ja registri väärtuse kaasata nimi. 

    "resources": [ 
      { 
        "name": "[concat('examplecopy-', copyIndex())]", 
        "type": "Microsoft.Web/sites", 
        "copy": { 
          "name": "websitescopy", 
          "count": "[parameters('count')]" 
        }, 
        ...
      }
    ]


<a id="div" />
### <a name="div"></a>DIV

**DIV (operand1, operand2)**

Annab vastuseks kahe esitatud täisarvude täisarvu jagamine.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| operand1                           |   Jah    | Täisarv jagatakse.
| operand2                           |   Jah    | Täisarv, mida kasutatakse jagamine. Ei saa olla 0.

Järgmises näites jagab üks parameeter teise parameetriga.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "divResult": {
        "type": "int",
        "value": "[div(parameters('first'), parameters('second'))]"
      }
    }

<a id="int" />
### <a name="int"></a>int

**int(valueToConvert)**

Teisendab määratud väärtus täisarv.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Jah    | Teisendamiseks täisarvulise väärtuse. Väärtuse tüübi saab ainult tekstistring või täisarv.

Järgmises näites teisendab kasutaja esitatud parameetri väärtus täisarv.

    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    }


<a id="mod" />
### <a name="mod"></a>mod

**Mod (operand1, operand2)**

Tagastab jäägi, täisarv osakonna abil kahe esitatud murdosa.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| operand1                           |   Jah    | Täisarv jagatakse.
| operand2                           |   Jah    | Täisarv, mida kasutatakse jagada, peab olema erinevad 0.

Järgmine näide tagastab ülejäänud sisu jagamine teise parameetriga üks parameeter.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "modResult": {
        "type": "int",
        "value": "[mod(parameters('first'), parameters('second'))]"
      }
    }

<a id="mul" />
### <a name="mul"></a>Mul

**Mul (operand1, operand2)**

Annab vastuseks kahe esitatud täisarvude korrutise.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| operand1                           |   Jah    | Esimese täisarv korrutamiseks.
| operand2                           |   Jah    | Teine täisarv korrutamiseks.

Järgmises näites korrutab üks parameeter teise parameetriga.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to multiply"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to multiply"
        }
      }
    },
    ...
    "outputs": {
      "mulResult": {
        "type": "int",
        "value": "[mul(parameters('first'), parameters('second'))]"
      }
    }

<a id="sub" />
### <a name="sub"></a>sub

**sub (operand1, operand2)**

Annab vastuseks kahe esitatud täisarvude lahutamine.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| operand1                           |   Jah    | Täisarv, mis on lahutatud.
| operand2                           |   Jah    | Täisarv, mis on lahutatud.

Järgmises näites lahutab üks parameeter teise parameeter.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer subtracted from"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer to subtract"
        }
      }
    },
    ...
    "outputs": {
      "subResult": {
        "type": "int",
        "value": "[sub(parameters('first'), parameters('second'))]"
      }
    }

## <a name="string-functions"></a>Stringifunktsioonide

Ressursihaldur pakub stringide töötamiseks järgmisi funktsioone:

- [Base64](#base64)
- [concat](#concat)
- [pikkus](#lengthstring)
- [padLeft](#padleft)
- [asendamine](#replace)
- [Jäta](#skipstring)
- [tükeldatud](#split)
- [string](#string)
- [Alamstringi](#substring)
- [tegemine](#takestring)
- [toLower](#tolower)
- [toUpper](#toupper)
- [Trim](#trim)
- [uniqueString](#uniquestring)
- [URI](#uri)


<a id="base64" />
### <a name="base64"></a>Base64

**Base64 (inputString)**

Tagastab base64 kujutis Sisestuskeel string.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| inputString                        |   Jah    | Kui base64 tagastamiseks stringiväärtus.

Järgmises näites on kujutatud base64 funktsiooni kasutamise kohta.

    "variables": {
      "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
      "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

<a id="concat" />
### <a name="concat---string"></a>concat - string

**concat (string1, string2, string3,...)**

Ühendab mitu tekstistringi väärtust ja tagastab ühendatud string. 

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| string1                        |   Jah    | Concatenate stringiväärtuse.
| täiendavad stringide             |   Ei     | Concatenate string väärtused.

See funktsioon võib kuluda mõni argumentide arv ning saate aktsepteerida stringide või parameetrite massiivi. Näiteks ühendades massiive, lugege teemat [concat - massiiv](#concatarray).

Järgmises näites kujutatakse ühendada mitu stringi väärtuste tagastamiseks ühendatud string.

    "outputs": {
        "siteUri": {
          "type": "string",
          "value": "[concat('http://', reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
        }
    }


<a id="lengthstring" />
### <a name="length---string"></a>pikkus - string

**Length(string)**

Tagastab märkide arvu tekstistringis oleva.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| string                        |   Jah    | Märkide arvu toomiseks kasutada stringiväärtus.

Näiteks massiivi pikkus abil, vt [pikkus - massiiv](#length).

Järgmine näide tagastab märkide arvu tekstistringis oleva. 

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "nameLength": "[length(parameters('appName'))]"
    }
        

<a id="padleft" />
### <a name="padleft"></a>padLeft

**padLeft (valueToPad, totalLength, paddingCharacter)**

Tagastab stringi paremjoondatud, lisades märgid kuni jõuavad määratud kogupikkus vasakule.
  
| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| valueToPad                         |   Jah    | Tekstistring või int, et Joonda paremale.
| totalLength                        |   Jah    | Tagastatud stringi märkide koguarvu.
| paddingCharacter                   |   Ei     | Märgi vasakule täidise kuni kogupikkus jaoks kasutada. Vaikeväärtus on tühik.

Järgmises näites kujutatakse valimisklahvistikku kasutaja esitatud parameetri väärtus on null lisamisega kuni 10 märki stringiga. Kui parameeter algväärtus on pikem kui 10 tähemärki, lisatakse märke.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "paddedAppName": "[padLeft(parameters('appName'),10,'0')]"
    }

<a id="replace" />
### <a name="replace"></a>asendamine

**Asendage (originalString, oldCharacter, newCharacter)**

Tagastab määratud stringi asendatud mõne muu märgi koos ühe märgi kõik eksemplarid uue stringi.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| originalString                     |   Jah    | String, mis on asendatud mõne muu märgi ühe märgi kõik eksemplarid.
| oldCharacter                       |   Jah    | Märgi eemaldatakse algse stringi.
| newCharacter                       |   Jah    | Märgi asemel eemaldatud märgi lisada.

Järgmises näites on kujutatud kõik kriipsud eemaldamine kasutaja esitatud string.

    "parameters": {
        "identifier": { "type": "string" }
    },
    "variables": { 
        "newidentifier": "[replace(parameters('identifier'),'-','')]"
    }

<a id="skipstring" />
### <a name="skip---string"></a>Jäta - string
**Jäta (originalValue, numberToSkip)**

Tagastab arvu määratud stringi pärast stringi kõik märgid.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| originalValue                      |   Jah    | Stringi kasutamiseks vahele.
| numberToSkip                       |   Jah    | Vahele märkide arv. Kui selle väärtus on 0 või, tagastatakse kõik märkide stringi. Kui see on suurem kui stringi pikkus, tagastatakse tühi string. 

Näiteks massiivi jäta abil, vt [vahele jätta – massiiv](#skip).

Järgmises näites ignoreerib määratud arvu märke stringi.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for skipping"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[skip(parameters('first'),parameters('second'))]"
      }
    }


<a id="split" />
### <a name="split"></a>tükeldatud

**tükeldatud (inputString, delimiterString)**

**tükeldatud (inputString, delimiterArray)**

Tagastab massiivi stringid, mis sisaldab kasutatavat Sisestuskeel stringi võrdluslaadi, mis on eraldatud poolt määratud eraldajad.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| inputString                        |   Jah    | Stringi tükeldada.
| eraldaja                          |   Jah    | Eraldaja kasutamiseks saab ühele tekstistringile või stringide massiiv.

Järgmises näites jagab Sisestuskeel stringi, koma koos.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "stringPieces": "[split(parameters('inputString'), ',')]"
    }

Järgmises näites jagab Sisestuskeel stringi, koma või semikooloniga.

    "variables": {
      "stringToSplit": "test1,test2;test3",
      "delimiters": [ ",", ";" ]
    },
    "resources": [ ],
    "outputs": {
      "exampleOutput": {
        "value": "[split(variables('stringToSplit'), variables('delimiters'))]",
        "type": "array"
      }
    }

<a id="string" />
### <a name="string"></a>string

**string(valueToConvert)**

Teisendab tekstistringi määratud väärtuse.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Jah    | Teisendamiseks stringi väärtus. Mis tahes tüüpi väärtus saab ümber, sh objektid ja massiivi.

Järgmises näites teisendab stringide kasutaja esitatud parameetrite väärtused.

    "parameters": {
      "jsonObject": {
        "type": "object",
        "defaultValue": {
          "valueA": 10,
          "valueB": "Example Text"
        }
      },
      "jsonArray": {
        "type": "array",
        "defaultValue": [ "a", "b", "c" ]
      },
      "jsonInt": {
        "type": "int",
        "defaultValue": 5
      }
    },
    "variables": { 
      "objectString": "[string(parameters('jsonObject'))]",
      "arrayString": "[string(parameters('jsonArray'))]",
      "intString": "[string(parameters('jsonInt'))]"
    }

<a id="substring" />
### <a name="substring"></a>Alamstringi

**Alamstringi (stringToParse, startIndex, pikkus)**

Tagastab alamstringi, mis algab määratud märgi paigutus ja see sisaldab määratud arvu märke.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| stringToParse                     |   Jah    | Algse stringi, millest on alamstringi ekstraktimist.
| startIndex                         | Ei      | 0-põhine märgi alguskoha jaoks soovitud alamstringi.
| pikkus                             | Ei      | Funktsiooni alamstringi märkide arvu.

Järgmises näites ekstraktib parameetri kolm esimest märki.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "prefix": "[substring(parameters('inputString'), 0, 3)]"
    }

<a id="takestring" />
### <a name="take---string"></a>Võtke - string
**võtta (originalValue numberToTake)**

Tagastab määratud arvu märke stringi stringi algusest.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| originalValue                      |   Jah    | Stringi teha märke kaudu.
| numberToTake                       |   Jah    | Võtta märkide arv. Kui selle väärtus on 0 või, tagastatakse tühi string. Kui see on suurem kui antud stringi pikkus, tagastatakse kõik märkide stringi.

Näide massiivi võta abil leiate teemast [võtta - massiiv](#take).

Järgmine näide juhatab määratud arvu märke stringi.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for taking"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[take(parameters('first'), parameters('second'))]"
      }
    }

<a id="tolower" />
### <a name="tolower"></a>toLower

**toLower(stringToChange)**

Teisendab määratud stringiga madalama juhtumi.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Jah    | Teisendamiseks alumise juhul string.

Järgmises näites teisendab madalama juhtumi kasutaja esitatud parameetri väärtuse.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "lowerCaseAppName": "[toLower(parameters('appName'))]"
    }

<a id="toupper" />
### <a name="toupper"></a>toUpper

**toUpper(stringToChange)**

Teisendab määratud stringiga läbivate suurtähtedega.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Jah    | Stringi suurtähed teisendada.

Järgmises näites teisendab kasutaja esitatud parameetri väärtuse läbivate suurtähtedega.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "upperCaseAppName": "[toUpper(parameters('appName'))]"
    }

<a id="trim" />
### <a name="trim"></a>Trim

**funktsiooni TRIM (stringToTrim)**

Eemaldab määratud stringiga kõik ees- ja lõpunullid tühimiku märgid.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| stringToTrim                       |   Jah    | Stringi trim.

Järgmises näites trimmib valge tühikuid kasutaja esitatud parameetri väärtuse.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "trimAppName": "[trim(parameters('appName'))]"
    }

<a id="uniquestring" />
### <a name="uniquestring"></a>uniqueString

**uniqueString (baseString,...)**

Loob tarkadeks räsi stringi nagu parameetrite väärtuste põhjal. 

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| baseString      |   Jah    | Funktsiooni räsi saab luua kordumatu stringi string.
| täiendavad parameetrid vastavalt vajadusele    | Ei       | Saate lisada nii palju stringide vastavalt vajadusele väärtus, mis määrab unikaalsuse taseme loomiseks.

See funktsioon on kasulik, kui teil on vaja luua ressursi kordumatu nimi. Esitate parameetrite väärtused, mida tulemi jaoks unikaalsuse ulatust piirata. Saate määrata, kas nimi on kordumatu tellimus, ressursirühm või juurutamise allapoole. 

Tagastatud väärtus pole juhuslik string vaid räsi funktsiooni tulemus. Tagastatud väärtus on 13 tähemärki. See pole globaalselt kordumatu. Kui soovite ühendamiseks väärtus eesliitega kaudu oma nimetamistava loomiseks tähenduslik nimi. Järgmises näites on kujutatud tagastatud väärtus vorming. Muidugi tegelik väärtus sõltub esitatud parameetrid.

    tcvhiyu5h2o5o

Järgmised näited kujutavad uniqueString abil saate luua kordumatu väärtus levinumaid tasemete puhul tehke järgmist.

Tellimuse rakendatud kordumatu

    "[uniqueString(subscription().subscriptionId)]"

Kordumatu, kus otsinguulatuseks on määratud ressursirühm

    "[uniqueString(resourceGroup().id)]"

Kordumatu rakendatud ressursirühma juurutamine

    "[uniqueString(resourceGroup().id, deployment().name)]"
    
Järgmises näites kujutatakse luua salvestusruumi konto, mis põhineb ressursirühma kordumatu nimi (sees selle ressursi rühma nimi pole kordumatud, kui ehitatud samal viisil).

    "resources": [{ 
        "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
        "type": "Microsoft.Storage/storageAccounts", 
        ...



<a id="uri" />
### <a name="uri"></a>URI

**URI (baseUri, relativeUri)**

Loob on absoluutne URI, kombineerides soovitud baseUri ja relativeUri string.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| baseUri                            |   Jah    | Base uri string.
| relativeUri                        |   Jah    | Suhteline uri stringi tekstistringi base uri lisada.

**BaseUri** parameetri väärtus saate lisada mingit kindlat tüüpi failiga, kuid ainult base tee kasutatakse ehitamine URI. Näiteks läbimine **http://contoso.com/resources/azuredeploy.json** , nagu on baseUri parameeter tulemuste alus URI **http://contoso.com/resources/**.

Järgmises näites kujutatakse ehitada pesastatud malli põhjal malli emaüksuse väärtus link.

    "templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"

## <a name="array-functions"></a>Massiivi funktsioonid

Ressursihaldur pakub mitmeid funktsioone töötamiseks massiivi väärtusi.

- [concat](#concatarray)
- [pikkus](#length)
- [Jäta](#skip)
- [tegemine](#take)

Massiivi väärtuse alusel eraldatud stringiväärtust kohta leiate teavet [jagada](#split).

<a id="concatarray" />
### <a name="concat---array"></a>concat - massiiv

**concat (massiiv1, massiiv2; massiiv3;...)**

Ühendab mitu massiivides ja tagastab ühendatud massiiv. 

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| massiiv1                        |   Jah    | Massiivi concatenate.
| täiendavad massiivid             |   Ei     | Massiivid concatenate.

See funktsioon võib kuluda mõni argumentide arv ning saate aktsepteerida stringide või parameetrite massiivi. Näiteks ühendades stringiväärtust, leiate teemast [concat - string](#concat).

Järgmises näites on kujutatud kahe massiivi ühendamiseks tehke järgmist.

    "parameters": {
        "firstarray": {
            type: "array"
        }
        "secondarray": {
            type: "array"
        }
     },
     "variables": {
         "combinedarray": "[concat(parameters('firstarray'), parameters('secondarray'))]
     }
        

<a id="length" />
### <a name="length---array"></a>pikkus - massiiv

**Length(array)**

Annab vastuseks massiivi elementide arv.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| massiiv                        |   Jah    | Massiivi elementide arv toomiseks kasutada.

Array selle funktsiooni abil saate määrata korduste arv ressursid loomisel. Järgmises näites parameeter **siteNames** viitab nimede loomisel veebisaite massiivi.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

Massiivi selle funktsiooni kasutamise kohta leiate lisateavet teemast [mitmes eksemplaris ressursside Azure'i ressursihaldur loomine](resource-group-create-multiple.md). 

Näiteks väärtuse stringi pikkus abil, vt [pikkus - string](#lengthstring).

<a id="skip" />
### <a name="skip---array"></a>vahele jätta – massiiv
**Jäta (originalValue, numberToSkip)**

Tagastab massiivi kõik andmed pärast määratud arv massiivis.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| originalValue                      |   Jah    | Massiivi kasutada vahele.
| numberToSkip                       |   Jah    | Vahele elementide arv. Kui selle väärtus on 0 või, tagastatakse kõik elemendid massiivis. Kui see on suurem kui massiivi pikkus, tagastatakse tühi massiivi. 

Näiteks jäta abil tekstistring, leiate teemast [vahele jätta - string](#skipstring).

Järgmises näites ignoreerib määratud massiivi elementide arv.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for skipping"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[skip(parameters('first'), parameters('second'))]"
      }
    }

<a id="take" />
### <a name="take---array"></a>Võtke - massiiv
**võtta (originalValue numberToTake)**

Tagastab massiivi elementide arv määratud massiiv algust.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| originalValue                      |   Jah    | Massiivi aspekte kaudu.
| numberToTake                       |   Jah    | Võtta elementide arv. Kui selle väärtus on 0 või, tagastatakse tühi massiivi. Kui see on suurem kui antud massiivi pikkus, tagastatakse kõik elemendid massiivis.

Näiteks võta abil tekstistring, leiate teemast [võtta - string](#takestring).

Järgmine näide juhatab elemente massiiv määratud arvu.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for taking"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[take(parameters('first'),parameters('second'))]"
      }
    }

## <a name="deployment-value-functions"></a>Juurutamise väärtus funktsioonid

Ressursihaldur pakub võimaluste väärtused jaotiste Mall ja juurutamise seotud väärtused järgmised funktsioonid:

- [juurutamine](#deployment)
- [parameetrid](#parameters)
- [muutujad](#variables)

Väärtuste ressursid, ressursside rühmade või tellimuste kohta leiate teavet [ressursside funktsioonid](#resource-functions).

<a id="deployment" />
### <a name="deployment"></a>juurutamine

**Deployment()**

Annab vastuseks teabe praeguse juurutamine toiming kohta.

See funktsioon tagastab objekti, mis käigus juurutamist. Tagastatud objekt atribuutide erineb sõltuvalt kas juurutamise objekt on möödas lingina või -rea objektina. 

Kui juurutamise objekt on möödas lineaarse, näiteks **- TemplateFile** parameetri kasutamisel Azure PowerShelli osutamiseks kohalik fail, tagastatud objekt on järgmises vormingus:

    {
        "name": "",
        "properties": {
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [
                ],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Kui objekt on möödunud lingina, nagu remote objekti osutamiseks **- TemplateUri** parameetri kasutamisel tagastatakse objekti järgmises vormingus. 

    {
        "name": "",
        "properties": {
            "templateLink": {
                "uri": ""
            },
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Järgmises näites on kujutatud deployment() linkida teise malli põhjal URI ema malli kasutamise kohta.

    "variables": {  
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
    }  

<a id="parameters" />
### <a name="parameters"></a>parameetrid

**parameetrite (parameterName)**

Tagastab parameetri väärtuse. Parameetrite jaotises malli tuleb määratleda määratud parameetri nime.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| parameterName                      |   Jah    | Tagastamiseks parameetri nimi.

Järgmises näites on kujutatud lihtsustatud kasutage funktsiooni parameetrid.

    "parameters": { 
      "siteName": {
          "type": "string"
      }
    },
    "resources": [
       {
          "apiVersion": "2014-06-01",
          "name": "[parameters('siteName')]",
          "type": "Microsoft.Web/Sites",
          ...
       }
    ]

<a id="variables" />
### <a name="variables"></a>muutujad

**muutujate (variableName)**

Tagastab väärtuse muutujana. Määratud muutuja nimi peab olema määratletud muutujate jaotise malli.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| muutuja nimi                      |   Jah    | Tagastamiseks muutuja nimi.

Järgmises näites kasutatakse muutuja väärtuse.

    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
      }
    ],

## <a name="resource-functions"></a>Ressursi funktsioonid

Ressursihaldur pakub võimaluste ressursi väärtused järgmised funktsioonid:

- [listKeys ja loendi {väärtus}](#listkeys)
- [pakkujad](#providers)
- [viide](#reference)
- [resourceGroup](#resourcegroup)
- [ResourceIdkasutamisel](#resourceid)
- [tellimuse](#subscription)

Väärtuste parameetrid, muutujate või praegune juurutamise kohta leiate teavet [juurutamise väärtus funktsioonid](#deployment-value-functions).

<a id="listkeys" />
<a id="list" />
### <a name="listkeys-and-listvalue"></a>listKeys ja loendi {väärtus}

**listKeys (resourceName või resourceIdentifier, apiVersion)**

**loendi {väärtus} (resourceName või resourceIdentifier, apiVersion)**

Tagastab väärtused, mis tahes ressursi tüüp, mida toetab loendist toimingu. Kõige levinum kasutamine on **listKeys**. 
  
| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| resourceName või resourceIdentifier |   Jah    | Ressursi ainuidentifikaator.
| apiVersion                         |   Jah    | Ressursi käitusaja olekus API versioon.

Kõik toimingud, mis algab **loendi** saab kasutada funktsiooni malli. Saadaval toimingute kaasata, mitte ainult **listKeys**, vaid ka toimingute **loend**, **listAdminKeys**ja **listStatus**. Otsustada, millist tüüpi ressursi on loendis toiming, kasutage järgmist PowerShelli käsu.

    Get-AzureRmProviderOperation -OperationSearchString *  | where {$_.Operation -like "*list*"} | FT Operation

Või hankige Azure'i CLI loendi. Järgmises näites toob kõik need toimingud **apiapps**ja kasutab JSON kasuliku [jq](http://stedolan.github.io/jq/download/) filtreerimiseks ainult loendi toimingud.

    azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"

Funktsiooni ResourceIdkasutamisel saab määrata [ResourceIdkasutamisel funktsioon](#resourceid) või vormingus **{providerNamespace} / {resourceType} / {resourceName}**.

Järgmises näites kujutatakse tulu salvestusruumi konto jaotises väljundeid esmaseid ja teiseseid võtmed.

    "outputs": { 
      "listKeysOutput": { 
        "value": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2016-01-01')]", 
        "type" : "object" 
      } 
    } 

Tagastatud objekti listKeys on järgmises vormingus:

    {
      "keys": [
        {
          "keyName": "key1",
          "permissions": "Full",
          "value": "{value}"
        },
        {
          "keyName": "key2",
          "permissions": "Full",
          "value": "{value}"
        }
      ]
    }

<a id="providers" />
### <a name="providers"></a>pakkujad

**pakkujad (providerNamespace, [resourceType])**

Annab vastuseks teabe saamiseks ressursi pakkuja ja selle ressursi toetatud. Kui te ei esita ressursi tüüp, tagastab funktsioon toetatud failitüübid ressursi pakkuja.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| providerNamespace                  |   Jah    | Namespace pakkuja
| resourceType                       |   Ei     | Ressursi jooksul määratud nimeruumi tüüp.

Iga toetatud tüüp, tagastatakse järgmises vormingus. Massiivi tellimine on tagatud.

    {
        "resourceType": "",
        "locations": [ ],
        "apiVersions": [ ]
    }

Järgmises näites kirjeldatakse, kuidas kasutada funktsiooni pakkuja.

    "outputs": {
        "exampleOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }

<a id="reference" />
### <a name="reference"></a>viide

**viide (resourceName või resourceIdentifier, [apiVersion])**

Tagastab tähistava teise ressursi käitusaja olekus.

| Parameetri                          | Nõutav | Kirjeldus
| :--------------------------------: | :------: | :----------
| resourceName või resourceIdentifier |   Jah    | Nime või ressursi ainuidentifikaator.
| apiVersion                         |   Ei     | Määratud ressursi API versioon. See parameeter kaasata ressursi pole ette valmistatud jooksul sama malli.

Funktsiooni **viide** saab väärtusega käitusaja olekust ja seetõttu ei saa kasutada jaotises muutujaid. Seda saab kasutada malli väljundeid osas.

Funktsiooni viide abil saate peidetult deklareerida sõltub üks ressurss teise ressursi kui viidatud ressursi on ette valmistatud jooksul sama malli. Teil pole vaja kasutada ka atribuudi **ei leia dependsOn** . Funktsioon ei väärtustata kuni viidatud ressurss lõpule viinud juurutamise.

Järgmises näites viitab salvestusruumi konto, sama malli juurutatud.

    "outputs": {
        "NewStorage": {
            "value": "[reference(parameters('storageAccountName'))]",
            "type" : "object"
        }
    }

Järgmises näites viitab salvestusruumi konto, mis on juurutatud selle malli, kuid olemas ressursi rühma kasutusele ressurssidena.

    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }

Saate tuua kindla väärtuse tagastatud objekti, nt bloobimälu lõpp-punkti URI, nagu on näidatud järgmises näites.

    "outputs": {
        "BlobUri": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Järgmises näites viitab salvestusruumi konto erinevate ressursside rühma.

    "outputs": {
        "BlobUri": {
            "value": "[reference(resourceId(parameters('relatedGroup'), 'Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Funktsiooni **viide** tagastatud objekti atribuutide erinevad ressursside tüübi järgi. Atribuutide nimesid ja ressursside tüüp väärtuste kuvamiseks luua lihtne Mall, mis tagastab objekti jaotises **väljundid** . Kui teil on mõne olemasoleva ressursi seda tüüpi, tagastab malli objekti lihtsalt ilma uusi ressursse rakendades. Kui teil on olemasolev ressurss seda tüüpi malli kasutab ainult seda tüüpi ja tagastab objekti. Seejärel lisage muud mallid, mis dünaamiliselt tuua väärtused juurutamise käigus on vaja need atribuudid. 

<a id="resourcegroup" />
### <a name="resourcegroup"></a>resourceGroup

**resourceGroup()**

Tagastab objekti, mis tähistab praegust ressursirühma. 

Tagastatud objekt on järgmises vormingus:

    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
      "name": "{resourceGroupName}",
      "location": "{resourceGroupLocation}",
      "tags": {
      },
      "properties": {
        "provisioningState": "{status}"
      }
    }

Järgmises näites kasutatakse ressursi rühma asukoht veebisaidi asukoha määramiseks.

    "resources": [
       {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          ...
       }
    ]

<a id="resourceid" />
### <a name="resourceid"></a>ResourceIdkasutamisel

**ResourceIdkasutamisel ([subscriptionId], [resourceGroupName] resourceType resourceName1, [resourceName2])**

Tagastab Ressursi ainuidentifikaator. 
      
| Parameetri         | Nõutav | Kirjeldus
| :---------------: | :------: | :----------
| subscriptionId    |   Ei     | Vaikeväärtus on praegune tellimus. Määrake selle väärtuseks peate hankima muu tellimus ressursi.
| resourceGroupName |   Ei     | Vaikeväärtus on praeguse ressursirühma. Määrake selle väärtuseks peate hankima muu ressursirühm ressurssi.
| resourceType      |   Jah    | Ressursside, sh ressursi pakkuja nimeruum tüüp.
| resourceName1     |   Jah    | Ressursi nimi.
| resourceName2     |   Ei     | Järgmise ressursi nimi lõigu kui ressursi on pesastatud.

Selle funktsiooni kasutamiseks, kui ressursi nimi on ebaselgete või mitte ettevalmistatud sama mall. Identifikaator tagastatakse järgmises vormingus:

    /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/{resourceProviderNamespace}/{resourceType}/{resourceName}

Järgmises näites kujutatakse toomiseks ressursi ID veebisaidi ja andmebaas. Veebisait olemas nimega **myWebsitesGroup** ressursirühma ja andmebaasi praeguse ressursirühma selle malli jaoks on olemas.

    [resourceId('myWebsitesGroup', 'Microsoft.Web/sites', parameters('siteName'))]
    [resourceId('Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]
    
Sageli, peate selle funktsiooni kasutamiseks salvestusruumi konto või virtuaalse võrgu kasutamisel alternatiivse ressursside rühma. Salvestusruumi konto või virtuaalse võrgu võib kasutada mitme ressursi rühma; Seetõttu ei soovi kustutamise neid ühe ressursirühma. Järgmises näites on kujutatud ressursi väliste ressursside rühmast hõlpsalt kasutamise kohta:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "virtualNetworkName": {
              "type": "string"
          },
          "virtualNetworkResourceGroup": {
              "type": "string"
          },
          "subnet1Name": {
              "type": "string"
          },
          "nicName": {
              "type": "string"
          }
      },
      "variables": {
          "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
      },
      "resources": [
      {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[parameters('nicName')]",
          "location": "[parameters('location')]",
          "properties": {
              "ipConfigurations": [{
                  "name": "ipconfig1",
                  "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "subnet": {
                          "id": "[variables('subnet1Ref')]"
                      }
                  }
              }]
           }
      }]
    }

<a id="subscription" />
### <a name="subscription"></a>tellimuse

**Subscription()**

Tagastab järgmises vormingus Tellimuse üksikasjad.

    {
        "id": "/subscriptions/#####",
        "subscriptionId": "#####",
        "tenantId": "#####"
    }

Järgmine näide kujutab funktsiooni tellimuse väljundeid jaotises nimega. 

    "outputs": { 
      "exampleOutput": { 
          "value": "[subscription()]", 
          "type" : "object" 
      } 
    } 


## <a name="next-steps"></a>Järgmised sammud
- Jaotised on Azure ressursihaldur malli kirjeldus, leiate [Azure'i ressursihaldur loome Mallid](resource-group-authoring-templates.md)
- Ühendada mitu Mallid, lugege teemat [lingitud mallide Azure'i ressursihaldur kasutamine](resource-group-linked-templates.md)
- Kordamiseks määratud arv kordi tüüpi ressursi loomisel leiate [ressursse Azure'i ressursihaldur mitu eksemplari loomine](resource-group-create-multiple.md)
- Olete loonud malli juurutamise leiate teemast [Deploy rakenduse Azure'i ressursihaldur malli abil](resource-group-template-deploy.md)

