<properties 
    pageTitle="Loogika appi funktsioonide kasutamine | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada täiustatud funktsioone loogika rakendused." 
    authors="stepsic-microsoft-com" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Loogika rakenduste funktsioonide kasutamine

[Eelmise teema](app-service-logic-create-a-logic-app.md)loodud esimese loogika rakenduse. Nüüd näitame teile koostada täielikuma protsessi rakenduse teenuste loogika rakenduste kasutamise kohta. See teema tutvustab järgmised uued loogika rakenduste mõisted:

- Tingimusvormingu loogika, mis käivitab toimingu ainult siis, kui teatud tingimused on täidetud.
- Koodi vaade redigeerida olemasolevaid loogika rakendus.
- Töövoo käivitamine suvandid.

Enne selle teema tegemiseks tuleb täita juhised [uue loogika rakenduse loomine](app-service-logic-create-a-logic-app.md). [Azure'i portaalis], liikuge sirvides oma loogika rakenduse ja **päästikute ja toimingute** Kokkuvõte loogika rakenduse määratluse redigeerimiseks klõpsake nuppu.

## <a name="reference-material"></a>Viide materjali

Kasulik on järgmised dokumendid:

- [Juhtimis- ja käitusaja REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) - sh kuidas rakendada loogika rakendused otse
- [Keele viide](https://msdn.microsoft.com/library/azure/mt643789.aspx) – kõigi toetatud funktsioonide/avaldiste põhjalik loend
- [Päästiku ja toimingu tüüpi](https://msdn.microsoft.com/library/azure/mt643939.aspx) - erinevat tüüpi toiminguid ja need võtta sisendeid
- [Ülevaade rakenduse teenuse](../app-service/app-service-value-prop-what-is.md) – mis osade valida lahenduse koostamine

## <a name="adding-conditional-logic"></a>Tingimusvormingu loogika lisamine

Kuigi algse vool töötab, on mõnes valdkonnas, mis võiks parandada. 


### <a name="conditional"></a>Tingimusvormingu
Loogika rakenduse võidakse teil saada palju kirju. Järgmised toimingud lisada loogika veenduge, et ainult saate meili, kui selle säutsu tulevad jälgijad arvu. 

1. Klõpsake plussmärki Twitteri toimingu *Saada kasutaja* otsimine.

2. Edastada **, Tweeted** väljal päästik Twitteri kasutaja kohta teabe saamiseks.

    ![Kasutaja hankimine](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Klõpsake plussmärki uuesti, kuid seekord valige **Tingimuse lisamine**

4. Klõpsake esimesele väljale **…** all **Saada kasutaja** **jälgijad count** välja leidmiseks.

5. Valige rippmenüüst väärtus **suurem kui**

6. Teisele väljale Tippige jälgijad soovite kasutajatel on arv.

    ![Tingimusvormingu](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Lõpuks-hiirega e-posti välja kasti **Kui jah** . See tähendab, vaid saate e-kirju kui järgija count on täidetud.

## <a name="repeating-over-a-list-with-foreach"></a>Korduv üle forEach loendi

Tsükkel forEach määrab massiivi üle toimingut korrata. Voo nurjub, kui see on massiiv. Näiteks, kui teil on Meede1, mis väljundid massiivi sõnumeid ja te ei soovi iga sõnumi saatmiseks võite kaasata forEach-lause toimingu atribuudid: forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Kasuta redigeerimiseks loogika rakendus

Lisaks designer, saate redigeerida otse kood, mis määratleb loogika rakendus, järgmiselt. 

1. Klõpsake nuppu **Kuva kood** käsuriba. 

    Avaneb täielik redaktor, saate redigeerida ainult määratluse kuvava.

    ![Koodi vaade](./media/app-service-logic-use-logic-app-features/codeview.png)

    Teksti redaktori abil saate kopeerida ja kleepida mis tahes arv toiminguid sama loogika rakendus või rakendused loogika vahel. Saate hõlpsasti lisada või kogu osade eemaldamine määratluse ja Lisaks saate teistega jagada määratlusi.

2. Kui teie muudatused kood vaadata, klõpsake lihtsalt nuppu **Salvesta**. 

### <a name="parameters"></a>Parameetrid
On mõned võimaluste loogika rakendused, mida saab kasutada ainult koodi vaates. Üks näide on parameetrid. Parameetrite oleks hõlpsam kogu rakenduse loogika väärtuste uuesti kasutada. Näiteks, kui teil on meiliaadress, mida soovite kasutada mitme toimingu, peaksite määratlema selle parameetrina.

Järgmine värskendab olemasoleva loogika rakenduse kasutamiseks parameetrid päringu termin.

1. Koodi vaade, otsige üles soovitud `parameters : {}` objekti ja sisestage järgmine teema objekti:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Liikuge kerides soovitud `twitterconnector` toiming leida päringu väärtust ja asendada see `#@{parameters('topic')}`.
    Funktsioon **concat** võib kasutada ka kokkuliitmiseks kahe või enama stringid, näiteks: `@concat('#',parameters('topic'))` on identne eespool. 
 
Parameetrid on hea võimalus väärtused, mis võivad palju muuta välja tõmmata. Need on eriti kasulik, kui teil on vaja alistamiseks parameetrite viibite. Alistada parameetrite põhjal keskkonna kohta lisateabe saamiseks vaadake meie [REST API dokumentatsiooni](https://msdn.microsoft.com/library/mt643787.aspx).

Nüüd, kui klõpsate nuppu **Salvesta**, tunnis saada uus tweets, mis on rohkem kui 5 retweets toimetatakse kaust nimega **tweets** oma Dropboxi.

Loogika rakenduse määratluste kohta leiate lisateavet teemast [Autor loogika rakenduse määratlusi](app-service-logic-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Loogika rakenduse töövoo käivitamine
On määratletud loogika rakenduse töövoo käivitamist mitut võimalust. Alati saab töövoo käivitada nõudmisel [Azure portaali].

### <a name="recurrence-triggers"></a>Päästikute Korduvus
Korduvus päästik töötab teie määratud intervalliga. Kui päästiku on tingimusvormingu loogika, käivitab määrab, kas töövoo käivitamiseks tuleb. Käivitamiseks näitab, et see peaks töötama tagastades on `200` olekukoodi. Kui see pole vaja käivitada, tagastab see on `202` olekukoodi.

### <a name="callback-using-rest-apis"></a>Tagasihelistamise REST API-de kasutamine
Teenuseid saate helistada loogika rakenduse endpoint töövoo käivitamine. Lisateabe saamiseks vaadake [loogika rakendusi nagu sissenõutav lõpp-punktid](app-service-logic-connector-http.md) . Loogika rakenduse tellitavate sellist käivitamiseks nuppu **Käivita kohe** käsuribal. 

<!-- Shared links -->
[Azure'i portaal]: https://portal.azure.com 