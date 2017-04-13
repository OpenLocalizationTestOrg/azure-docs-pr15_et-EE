<properties
   pageTitle="Suhtlevad aruannetega, mis kasutavad JavaScripti API | Microsoft Azure'i"
   description="Power BI manustatud, suhtlevad aruannetega JavaScripti API kasutamine"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Power BI aruannete abil JavaScripti API kasutamine

Power BI JavaScripti API võimaldab teil hõlpsasti embed rakenduste Power BI aruanded. API-ga, saate oma rakenduste programmiliselt suhelda erinevate elementide nagu lehed ja filtrid. Selle interaktiivsuse teeb Power BI aruannete integreeritud rakenduse osa.

Saate manustada aruande Power BI rakenduse abil IFRAME'i, mis on majutatud rakenduse osana. IFRAME'i toimib nagu järgmisel pildil näete rakenduse ja aruande vahel piiri. 

![Power BI manustatud iframe ilma JavaScripti API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

IFRAME'i muudab krae protsessi palju lihtsam, kuid ilma JavaScripti API aruanne ja rakenduse ei saa omavahel suhelda. Suhtluse puudumine saab teha tundub, nagu aruande ei ole eriti rakenduse osa. Aruanne ja rakenduse väga vaja suhelda edasi ja tagasi, nagu järgmisel pildil.

![Power BI manustatud iframe JavaScripti API-ga](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Power BI JavaScripti API võimaldab teil kirjutada koodi, mis võivad turvaliselt läbida IFRAME'i äärist. See võimaldab rakenduse programmiliselt aruande toimingu sooritamine ja toiminguid, mida kasutajad teha aruandes sündmuste kuulata.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Mida saab teha Power BI JavaScripti API-ga?
JavaScripti API-ga saate aruannete haldamine, liikuge aruande lehekülgede, aruande filtreerimine ja käsitleda manustamine sündmused. Järgmisel joonisel on esitatud API struktuuri.

![Power BI JavaScripti API skeem](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Aruannete haldamine
JavaScripti API võimaldab teil hallata käitumine aruanne ja lehe tasemel.

- Manustada teatud Power BI aruande turvaliselt rakenduse - proovige [demo rakenduse manustamine](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Seadmine juurdepääsu luba
- Aruande konfigureerimine
  - Lubamine ja keelamine filtri paani ja navigeerimispaani lehe - proovige [sätted demo rakenduse värskendamine](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Lehtede ja filtrid vaikesätete seadmine - proovige [demo vaikeväärtuste seadmine](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Sisestage ja täisekraanvaate väljumine

[Lisateavet aruande manustamine](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Liikuge aruande lehekülgede
JavaScripti API enbales avastada aruande kõik lehed ja seadmine praeguse lehe. Proovige [navigeerimise demo rakenduse](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Lisateavet lehel liikumine](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Aruande filtreerimine
JavaScripti API pakub põhi- ja filtreerimise võimalusi, manustatud aruannete ja aruande lehed. Proovige [filtreerimine demo rakendus](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)ja vaadake üle mõned sissejuhatavad koodi siin.  


#### <a name="basic-filters"></a>Põhifiltrid
Elementaarne filtreerimine on paigutatud veeru- või hierarhia taset ja kaasamiseks või välistamiseks väärtuste loend sisaldab.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Täpsemad filtrid
Täpsemad filtrid kasutage loogika tehtemärkide ja või või ja nõustuge ühe või mitme tingimuse, oma tehtemärk ja väärtus. Toetatud tingimused on:

- Ükski
- Vähem kui
- LessThanOrEqual
- Ületas
- GreaterThanOrEqual
- Sisaldab
- DoesNotContain
- StartsWith
- DoesNotStartWith
- On
- Ei ole
- IsBlank
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Lisateavet filtreerimise kohta](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Käsitsemise sündmused
Lisaks saatmine teabe importimine IFRAME'i, saate rakenduse ka järgmised sündmused IFRAME'i pärit teave:

- Manustamine
  - laaditud
  - tõrge
- Aruanded
  - pageChanged
  - dataSelected (tulekul)

[Lisateave sündmuste töötlemise](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Järgmised sammud
Power BI JavaScripti API kohta lisateabe saamiseks vaadake järgmisi linke:

- [JavaScripti API viki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Objekti mudeli viide](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Näidised
  - [Nurga](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Reaalajas demo](https://microsoft.github.io/PowerBI-JavaScript/demo/)
