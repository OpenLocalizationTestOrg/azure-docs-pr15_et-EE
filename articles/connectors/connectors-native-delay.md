<properties
    pageTitle="Loogika rakenduste lisamine viivituse | Microsoft Azure'i"
    description="Viivitus ja viivitus ülevaade – kuni toimingud ja kuidas neid kasutada Azure loogika rakenduse abil."
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

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Viivitus ja viivitus kasutamise alustamine – kuni toimingud

Viivitus abil ja "viivitus-kuni" toimingud lõpetamist töövoo stsenaariumi.

Näiteks saate teha järgmist.

- Oodake, kuni soovitud nädalapäeva olek värskenduse saatmiseks e-posti kaudu.
- Töövoo edasi lükata, kuni HTTP-kõne on aega enne jätkamist ja tulemi toomine.

Viivitus toimingu loogika rakenduse kasutamise alustamine, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Kasutage viivitus toimingud

Toimingu on toiming, mis teostavad töövoo, mis on määratletud loogika rakendus. [Lisateavet leiate teemast toimingute kohta](connectors-overview.md).

Siin on näide jada viivituse etapi loogika rakenduse kasutamine.

1. Pärast lisamist käivitamiseks, klõpsake nuppu **Uus samm** toimingu lisamiseks.
2. Otsige **viivituse** avab viivituse toimingud. Selles näites me valib **viivitust**.

    ![Viivitus toimingud](./media/connectors-native-delay/using-action-1.png)

3. Täitke viivituse konfigureerimiseks toimingu atribuute.

    ![Viivitus config](./media/connectors-native-delay/using-action-2.png)

4. Klõpsake nuppu **salvestamine** , avaldamine ja loogika rakenduse aktiveerimiseks.


## <a name="action-details"></a>Toimingu üksikasjad

Korduvus päästik on järgmised atribuudid, mida saab konfigureerida.

### <a name="delay-action"></a>Viivitus toiming

See toiming viivitused Käivita esitab teatud ajavahemiku jaoks.
A * tähendab, et see on nõutav väli.

|Kuvatav nimi|Atribuudi nimi|Kirjeldus|
|---|---|---|
|Count *|Count|Arvu ajaühikute edasi lükata.|
|Ühiku *|ühiku|Aja ühiku: `Second`, `Minute`, `Hour`, või`Day`|
<br>

### <a name="delay-until-action"></a>Viivitus-, kuni toiming

See toiming viivitused kuni määratud kuupäeva/kellaaja Käivita.
A * tähendab, et see on nõutav väli.

|Kuvatav nimi|Atribuudi nimi|Kirjeldus|
|---|---|---|
|Aasta *|ajatempli|Aasta viivitada kuni (GMT)|
|Kuu *|ajatempli|Kuu viivitada kuni (GMT)|
|Päeva *|ajatempli|Päeva viivitada kuni (GMT)|
<br>


## <a name="next-steps"></a>Järgmised sammud

Nüüd, proovige platvormi ja [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md). Muude saadaolevate konnektorite loogika rakendustes saate uurida vaadates meie [API-de loendis](apis-list.md).
