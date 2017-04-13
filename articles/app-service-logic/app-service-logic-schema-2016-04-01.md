<properties 
    pageTitle="Uue skeemi versiooni 2016-06-01 | Microsoft Azure'i" 
    description="Siit saate teada, kuidas kirjutada JSON definitsiooni loogika rakenduste uusima versiooni" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Uue skeemi versiooni 2016-06-01

Uue skeemi ja API loogika rakenduste versioon on mitmeid parandusi, mille usaldusväärsuse parandamiseks ja hõlpsamaks kasutamise loogika rakenduste. On 3 Peamised erinevused.

1. Lisaks otsinguulatuste, millised on toimingud, mis sisaldavad toimingute kogum.
1. Tingimused ja silmuseid on esimese klassi toimingud
1. Täitmise tellimise kaudu rohkem Paljusõnaline `runAfter` atribuudi (mis asendab `dependsOn`)

Lisateavet versiooniks 2015-08-01-eelvaade skeemi loogika rakenduste 2016-06-01 skeemiga, [vaadake allpool olevat jaotist versioonitäienduse.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. otsinguulatuste

Üks selle skeemi suurim muudatused on lisaks otsinguulatuste ja võimalus pesastada toimingud üksteise sees.  See on kasulik, kui toimingute kogum rühmitamine või kellel on vaja pesastada tegevuste üksteisest (nt tingimus võib olla mõni muu).  Üksikasjalikumat teavet ulatus süntaks võib olla leitud [siin](app-service-logic-loops-and-scopes.md), kuid lihtsa ulatus näite leiate allpool:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2 tingimused ja tsüklid muudatused

Skeemi varasemates versioonides tingimused ja silmuseid olid seotud ühe toimingu parameetrid.  Seda piirangut on tühistatud selle skeemi ja nüüd tingimused ja silmuseid kuvataks teatud tüüpi toimingut.  Lisateavet leiate [selle artikli](app-service-logic-loops-and-scopes.md)ja tingimus toimingu lihtne näide on allpool näidatud:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. atribuut RunAfter

Uue `runAfter` atribuudi asendab `dependsOn` aidata lubada suurema täpsusega Käivita tellimine.  `dependsOn`on sünonüüm "toimingu käivitasite ja õnnestus," aga mitu korda peate toimingu käivitada, kui eelmise toimingu õnnestus, nurjus või vahele.  `runAfter`selle paindlikkus võimaldab.  See on objekt, mis määrab kõik see käivitub pärast toimingu nimed ja massiivi olek, mis on aktsepteeritav, mis määratleb.  Näiteks kui soovite käivitada pärast õnnestus A ja B on õnnestus või nurjus, te soovite ehitada järgmine `runAfter` atribuut:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>Täiendamine versiooniks 2016-06-01 skeem

Uue 2016-06-01 skeemi versiooniks võtab vaid paar toimingut.  Skeemi kaudu muudatuste kohta leiate [selle artikli](app-service-logic-schema-2016-04-01.md).  Versiooniuuenduse sisaldab versioonitäienduse skripti, loogika uue rakendusena salvestamine ja potentsiaalselt ülekirjutamise vana loogika rakendus vajaduse korral.

1. Avage oma praeguse loogika rakendus.
1. Klõpsake tööriistaribal nuppu **Update skeemi**
   
    ![][1]
   
    Tagastatakse täiendatud määratlus.  Pidite kopeerida ja kleepida selle ressursi määratlus, kui teil on vaja, kuid **soovitame tungivalt** nupu **Salvesta nimega** abil tagada kõigi ühenduse viited kehtivad täiendatud loogika rakenduses.
1. Klõpsake nuppu **Salvesta nimega** , versioonitäienduse tera tööriistariba.
1. Täitke rakenduse nimi ja loogika olek ja klõpsake nuppu **Loo** juurutamiseks versioonitäienduse loogika rakenduse.
1. Kontrollige oma uuele versioonile üle viidud loogika rakendus töötab ootuspäraselt.

    >[AZURE.NOTE] Kui kasutate kasutusjuhendit või taotluse päästik, tagasihelistamise URL on muudetud loogika uus rakendus.  Veenduge, et see töötab lõpuni, ja te saate säilitada eelmise URL-id oma olemasoleva loogika rakenduse kohale klooni uus URL-i abil.

1. *Valikuline.* Tööriistariba (külgnevaid ülaltoodud joonisel ikooni **Värskenda skeemi** ) **klooni** nupu abil kirjutada oma eelmise loogika rakenduse skeemi uue versiooniga.  See on vajalik ainult siis, kui soovite säilitada sama ressursi ID-d või taotleda loogika rakenduse päästik URL-i.

### <a name="upgrade-tool-notes"></a>Versioonitäienduse tööriista märkmed

#### <a name="condition-mapping"></a>Tingimuse kaardistamine

Tööriist teeb parima rühmitamiseks true ja false haru toimingud koos ulatus uuele versioonile üle viidud määratlus.  Konkreetselt kujundaja muster `@equals(actions('a').status, 'Skipped')` nimega ilmub mõni `else` toiming.  Aga kui tööriist tuvastab mustrite seda ei tuvasta potentsiaalselt loob see nii on true ja false haru eraldi tingimused.  Saab uuesti vastendatud postitada vajadusel uuele versioonile.

#### <a name="foreach-with-condition"></a>ForEach tingimusega
  
Uue skeemi toiminguga filter saab paljundada eelmise muster foreach tsükkel tingimusega iga üksuse kohta.  See peaks toimuma automaatselt uuele versioonile.  Seisund muutub filter toimingu enne foreach tsükkel (ainult massiivi üksused, mis vastavad tingimusele tagastamiseks) ja selle massiiv on läinud foreach toiming.  Saate vaadata selle [selle artikli](app-service-logic-loops-and-scopes.md) näide

#### <a name="resource-tags"></a>Ressursi Sildid

Ressursi sildid eemaldatakse uuendada ja määrata need uuesti täiendatud töövoo jaoks on vaja.

## <a name="other-changes"></a>Muud muudatused

### <a name="manual-trigger-renamed-to-request-trigger"></a>Käsitsi päästik uueks nimeks taotluse päästik

Tüüp `manual` on aegunud ja uueks nimeks `request` sellist, `http`.  See on ühtsema mustri päästiku kasutatakse koostamiseks tüüpi.

### <a name="new-filter-action"></a>Uus "filter" toiming

Kui töötate suure massiiv ja et filtreerida üksuste väiksemat allapoole vaja, saate uute "filter" tüüp.  See aktsepteerib massiivi ja tingimus ja hindab iga üksuse tingimus ja üksused, mis vastavad tingimusele massiivi.

### <a name="foreach-and-until-action-restrictions"></a>ForEach ja, kuni toiming piirangud

Funktsiooni foreach ja kuni tsükkel on piiratud ühe toimingu.

### <a name="trackedproperties-on-actions"></a>TrackedProperties toimingute kohta

Toimingud nüüd on teil täiendavad vara (vend, et `runAfter` ja `type`) ehk `trackedProperties`.  See on objekti, mis määrab teatud toimingu sisendeid või väljundeid kaasatavate Azure'i diagnostika telemeetria, mis kiirgab töövoo käigus.  Näiteks:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Järgmised sammud
- [Loogika rakenduse töövoo kasutamine](app-service-logic-author-definitions.md)
- [Loogika rakenduse juurutamine malli loomine](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
