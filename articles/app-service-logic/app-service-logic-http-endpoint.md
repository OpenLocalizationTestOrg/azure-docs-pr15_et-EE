<properties
   pageTitle="Loogika rakendusi nagu sissenõutav lõpp-punktid"
   description="Loomine ja konfigureerimine päästik lõpp-punktid ja loogika rakenduse teenuses Azure rakenduse kasutamine"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>


# <a name="logic-apps-as-callable-endpoints"></a>Loogika rakendusi nagu sissenõutav lõpp-punktid

Loogika Apps algupäraselt saate seada sünkroonse HTTP lõpp-punkti käivitamiseks.  Muster sissenõutav lõpp-punktid abil autonoomsest loogika rakendusi nagu pesastatud töövoo "töövoo" toimingu loogika rakenduse kaudu.

On 3 tüüpi päästikute, millel saab saata taotlused.

* Taotlemine
* ApiConnectionWebhook
* HttpWebhook

Artikli ülejäänud, kasutame **taotluse** näitega, kuid kõik põhimõtted kehtivad ühtemoodi 2 tüüpide käivitab.

## <a name="adding-a-trigger-to-your-definition"></a>Definitsiooni käivitamiseks lisamine
Esimene samm on lisada käivitamiseks loogika rakenduse definitsiooni saab, sissetulevad taotlused.  Saate otsida Designeris jaoks "HTTP päring" päästik kaardi lisamiseks. Saate määratleda taotluse keha JSON skeemi ja designer loob märgid, mis aitavad teil sõeluda ja edastada andmete kaudu töövoo käsitsi käivitada.  Ma soovitame kasutada tööriista nagu [jsonschema.net](http://jsonschema.net) loomiseks JSON skeemi näidis keha last kaudu.

![Päringu päästik kaart][2]

Pärast salvestamist definitsiooni loogika rakendus, luuakse tagasihelistamise URL-i umbes selline:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Seda URL-i sisaldab SAS klahvi päringu parameetrite kasutada autentimiseks.

Samuti saate selle lõpp-punkti Azure'i portaalis:

![][1]

Või helistades:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Turvalisus päästik URL-i

Loogika rakenduse tagasihelistamise URL-id on loodud, kasutades turvaline juurdepääs ühiskasutusse antud signatuuri.  Allkirja päringu parameetrina läbib ning tuleb kinnitada tule loogika rakendus.  See on loodud kordumatu kombinatsiooni salajane võti loogika rakendus, päästik nime ja tehtud toimingu abil.  Kui keegi on salajane loogika rakenduse võti juurdepääsu, nad ei oleks lubatud signatuuri luua.

## <a name="calling-the-logic-app-triggers-endpoint"></a>Loogika rakenduse päästik lõpp-punkti helistamine

Kui olete loonud lõpp-punkti jaoks oma päästik, võite selle kaudu käivitada soovitud `POST` täielik URL. Täiendavate päiste ja mis tahes sisu, saate kaasata kehasse.

Kui soovitud sisutüüp on `application/json` siis, kui teil on võimalik viide atribuudid taotluse sees. Teisiti, käsitletakse seda binary ühe üksuse, mis saab edasi muude API-d, kuid ei saa viidata sees töövoo ilma teisendamise sisu.  Näiteks kui olete oma ostuõiguse `application/xml` sisu võite kasutada `@xpath()` teha ka xpath eraldamine või `@json()` JSON XML-ist teisendamiseks.  Sisu töötamise kohta lisateavet tipib [võib leida siin](app-service-logic-content-type.md)

Lisaks saate määrata JSON skeemi määratlus. See põhjustab designer loomiseks märgid, mida te saate siis lähevad juhiseid.  Näiteks järgmine teeb on `title` ja `name` luba kujundajas saadaval:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Sissetuleva taotluse sisu viitamine

Funktsiooni `@triggerOutputs()` funktsioon väljund sissetuleva taotluse sisu. Näiteks see näeks:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Saate kasutada funktsiooni `@triggerBody()` juurdepääsuks otsetee soovitud `body` atribuudi spetsiaalselt. 

## <a name="responding-to-the-request"></a>Funktsiooni kutsele vastamine

Mõned nõuab, et alustada loogika rakendus, võite funktsiooni helistaja teatud sisuga vastata. On uus toimingu tüüp, mida nimetatakse **vastus** , mille saab koostada olekukoodi keha ja vastus päised. Teate, et kui pole **vastuse** kujund, loogika rakenduse lõpp-punkti on *kohe* vastata **202 aktsepteeritud**.

![HTTP vastuse toiming][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Vastused on järgmine:

| Atribuut | Kirjeldus |
| -------- | ----------- |
| statusCode | HTTP olekukoodi sissetuleva taotluse vastata. See võib olla mis tahes kehtiv olekukoodi, mis algab 2xx, 4xx või 5xx. 3xx olekukoodi ei ole lubatud. | 
| sisu | Keha objekt, mida saab stringi, JSON objekt või isegi binary sisu viidata eelmist toimingut. | 
| päised | Saate määratleda mõni muu arv päiste kaasatavate vastus | 

Kõiki juhiseid rakenduses loogika vastuse jaoks vajalike *60 sekundi järel* algse taotluse vastu vastuse **juhul, kui töövoog on kutsutud pesastatud loogika rakenduse**jooksul lõpule viima. Kui vastus midagi jõutakse sees 60 sekundi järel klõpsake sissetuleva taotluse ajalõpuni ning saate **408 kliendi ajalõpp** HTTP vastuse.  Pesastatud loogika rakenduste loogika rakendus kasutab ka edaspidi ootama kuni lõpetatud, vastuse ema sõltumata sellest, kui palju aega kulub.

## <a name="advanced-endpoint-configuration"></a>Täiustatud lõpp-punkti konfigureerimine

Loogika rakendused on sisse ehitatud tugi otsest juurdepääsu lõpp-punkti ja kasutage soovitud `POST` meetod Käivita loogika rakenduse käivitamiseks. **HTTP kuulajale** API rakenduse toetatud varem ka, URL-i segmente ja HTTP meetod. API rakenduse host (Web app, mis on majutatud API rakenduse) lisamisega võib isegi määrata täiendavaid turvalisuse või kohandatud domeeni. 

See funktsioon on saadaval **API haldamise**kaudu:
* [Muuta taotluse](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [URL-i segmente taotluse muutmine](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Häälestada domeeni API halduse klassikaline Azure'i portaalis menüü **konfigureerimine**
* Lihttekstautentimine (**link vajalik**) otsimiseks poliitika häälestamine

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Kokkuvõte 2014-12-01-preview üleminekut

|  2014-12-01-eelvaade | 2016-06-01 |
|---------------------|--------------------|
| Klõpsake nuppu **HTTP kuulajale** API rakenduse kohta | Klõpsake **käsitsi päästik** (API rakendus pole nõutav) |
| HTTP kuulajale säte "*saadab automaatselt*" | Kas kaasata **vastuse** toimingu või mitte töövoo määratlus |
| Basic või OAuthi autentimise konfigureerimine | API halduse kaudu |
| HTTP meetod konfigureerimine | API halduse kaudu |
| Suhteline tee konfigureerimine | API halduse kaudu |
| Sissetuleva keha kaudu viitamine.`@triggerOutputs().body.Content` | Viite kaudu`@triggerOutputs().body` |
| **Vastuse saatmiseks HTTP** HTTP kuulajale tegevus | Klõpsake **vastata HTTP** (API rakendus pole nõutav)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
