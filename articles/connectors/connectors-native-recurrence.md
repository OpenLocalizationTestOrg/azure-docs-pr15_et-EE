<properties
    pageTitle="Lisa Korduvus päästik loogika rakendustes | Microsoft Azure'i"
    description="Ülevaade Korduvus käivitada, ja kuidas seda kasutada koos Azure loogika rakendus."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Korduvus päästik kasutamise alustamine

Korduvus päästik abil saate luua võimsaid töövoogude pilveteenuses.

Näiteks saate teha järgmist.

- Plaanimine töövoo käivitada SQL-i salvestatud protseduuri iga päev.
- E-posti kõik tweets kokkuvõtte kohta teatud #-sildi profiililehe viimase nädala jooksul.

Alustamiseks loogika rakenduse kasutamine Korduvus päästik, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Kasutage Korduvus päästik

Käivitamiseks on sündmus, mida saab kasutada töövoo, mis on määratletud loogika rakenduse käivitamiseks. [Lisateavet päästikute kohta](connectors-overview.md).

Siin on näide jada häälestamise Korduvus päästik loogika rakenduse.

1. Esimese sammuna loogika rakenduse lisamine päästik **Korduvus** .
2. Täitke parameetrid intervall Korduvus.

Loogika rakendus käivitub kohe pärast iga ajavahemik kestab.

![HTTP päästik](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Päästik üksikasjad

Korduvus päästik on järgmised atribuudid, mida saate konfigureerida.

See käivitub loogika rakenduse teatud ajavahemiku järel.
A * tähendab, et see on nõutav väli.

|Kuvatav nimi|Atribuudi nimi|Kirjeldus|
|---|---|---|
|Sagedus *|Frequency|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Intervall *|intervall|Korduvussuvandite antud sageduse intervalli.|
|Ajavöönd|Ajavöönd|Kui algusaeg ilma UTC-ajavahe, kasutatakse selle ajavööndi.|
|Algus|startTime|Algusaja [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)vormingus.|
<br>


## <a name="next-steps"></a>Järgmised sammud

Nüüd, proovige platvormi ja [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md). Muude saadaolevate konnektorid loogika rakendustes saate uurida vaadates meie [API-de loendis](apis-list.md).
