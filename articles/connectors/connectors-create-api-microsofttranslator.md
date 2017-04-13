<properties
    pageTitle="Lisage loogika rakendustes Microsoft Translator | Microsoft Azure'i"
    description="Microsoft Translator konnektor REST API parameetritega ülevaade"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-microsoft-translator-connector"></a>Microsoft Translator konnektor kasutamise alustamine
Ühenduse loomine Microsoft Translator teksti tõlkimine, Tuvasta keel ja palju muud. Microsoft Translator, saate teha järgmist. 

- Koostage oma voog saate Microsoft Translator andmete põhjal. 
- Toimingute abil saate teksti tõlkimine, Tuvasta keel ja palju muud. Neid toiminguid vastuse saamine, ja seejärel tehke väljund muude toimingute jaoks saadaval. Uue faili loomisel Dropboxi, saate tõlkida teksti muus keeles, kasutades Microsoft Translator fail.

Toimingu lisamiseks loogika rakendused, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud
Microsoft Translator hõlmab järgmisi toiminguid. Ei ole päästikute.

Päästikute | Toimingud
--- | ---
Ükski | <ul><li>Keeletuvastus</li><li>Teksti kõneks</li><li>Teksti tõlkimine</li><li>Keelte hankimine</li><li>Kõne keelte hankimine</li></ul>

Kõik konnektorid toetavad andmete JSON ja XML-vormingus.


## <a name="create-a-connection-to-microsoft-translator"></a>Microsoft Translator ühenduse loomine

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Ärplema REST API viide
Kehtib versioon: 1.0.

### <a name="detect-language"></a>Keeletuvastus    
Andmeallika keele tuvastab antud teksti.  
```GET: /Detect```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|päringu|string|Jah|päringu|ükski |Teksti, mille keel ei tuvastata|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="text-to-speech"></a>Teksti kõneks    
Teisendab antud teksti kõneks nimega heli voogu laine vormingus.  
```GET: /Speak```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|päringu|string|Jah|päringu|ükski |Teksti teisendamine|
|keel|string|Jah|päringu|ükski |Keele koodi genereerida kõne (näide: "en-us)|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="translate-text"></a>Teksti tõlkimine    
Kas pakett tõlgib teksti kasutades Microsoft Translator määratud keel.  
```GET: /Translate```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|päringu|string|Jah|päringu|ükski |Teksti tõlkimine|
|languageTo|string|Jah|päringu| ükski|Sihtkeel koodi (näide: "Prantsusmaa")|
|languageFrom|string|Ei|päringu|ükski |Allika keel; Kui ei esitata, proovib Microsoft Translator automaattuvastust. (näide: en)|
|kategooria|string|Ei|päringu|Üldine |Tõlke kategooria (vaikimisi: "Üldine")|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="get-languages"></a>Keelte hankimine    
Kõigi keelte, mis toetab Microsoft Translator toob.  
```GET: /TranslatableLanguages```

On selle kõne parameetreid pole. 

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="get-speech-languages"></a>Kõne keelte hankimine    
Saadaolevate keelte jaoks kõnesünteesi toob.  
```GET: /SpeakLanguages``` 

On selle kõne parameetreid pole.

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|

## <a name="object-definitions"></a>Objekti määratlused

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Keel: keel mudel Microsoft Translator tõlgitavate keelte jaoks

|Atribuudi nimi | Andmetüüp | Nõutav|
|---|---|---|
|Kood|string|Ei|
|Nimi|string|Ei|


## <a name="next-steps"></a>Järgmised sammud

[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

Minge tagasi [API-de loendis](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
