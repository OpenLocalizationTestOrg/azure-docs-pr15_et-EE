<properties
    pageTitle="Päringu toimingu lisada loogika rakendustes | Microsoft Azure'i"
    description="Ülevaade päringu toiming nagu filter massiiv toimingute jaoks."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Päringu toimingu alustamine

Päringu toimingu abil saate töötada pakettidena ja massiive tegemise töövood:

- Toimingut kõrge tähtsusega kõiki kirjeid luua andmebaasist.
- Salvestage PDF-i kõik manused on Azure'i bloobimälu kirju.

Päringu toimingu loogika rakenduse kasutamise alustamine, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>Kasutage toimingut päring

Toimingu on toiming, mis teostavad töövoo, mis on määratletud loogika rakendus. [Lisateavet leiate teemast toimingute kohta](connectors-overview.md).  

Päringu toiming on praegu ühe toiminguga, nimetatakse filtri massiiv, mis on esitatud designer. See võimaldab teil päringu massiivi ja filtreerimistulemitesse arvuga.

Siin on, kuidas saate lisada selle loogika rakendus.

1. Valige nupp **Uus toiming** .
2. Valige **Lisa toiming**.
3. Tippige otsinguväljale toimingu loendi **filtreerimine massiiv** toimingu **filter** .

    ![Valige päringu toiming](./media/connectors-native-query/using-action-1.png)

4. Valige massiivi filtreerimiseks. (Järgmine pilt kuvatakse Twitteri otsingu tulemuste massiivi.)
5. Saate luua tingimus hinnata iga üksuse kohta. (Järgmine pilt filtreerib tweets kasutajad, kellel on rohkem kui 100 jälgijad.)

    ![Toimingu päring](./media/connectors-native-query/using-action-2.png)

    Toimingu väljund uus massiiv, mis sisaldab ainult filtri vastavad tulemid.
6. Klõpsake vasakus ülanurgas tööriistariba salvestamiseks ning rakenduse loogika on nii salvestamine ja avaldamine (aktiveerida).

## <a name="query-action"></a>Päringu toiming

Siin on esitatud üksikasjad toimingute jaoks, mille selle konnektori toetab. Konnektor on üks võimalik toiming.

|Toiming|Kirjeldus|
|---|---|
|Filtri massiiv|Hindab massiivi iga üksuse jaoks tingimus ja tagastab väärtuse|

## <a name="action-details"></a>Toimingu üksikasjad

Päringu toiming on üks võimalik toiming. Järgmises tabelis on kirjeldatud kohustuslike ning valikuliste väljad toiming ja vastavate väljundi üksikasjad, mis on seotud toimingu abil.

### <a name="filter-array"></a>Filtri massiiv
Järgnevalt on Sisestuskeel väljad soovitud toimingu, mis teeb väljaminev HTTP-päring.
A * tähendab, et see on nõutav väli.

|Kuvatav nimi|Atribuudi nimi|Kirjeldus|
|---|---|---|
|: *|kaudu|Filtreerimiseks massiiv|
|Tingimuse *|Kui|Tingimuse hinnata iga üksuse jaoks|
<br>

### <a name="output-details"></a>Väljundi üksikasjad

Järgmisena kirjeldame väljundi üksikasjad HTTP vastus.

|Atribuudi nimi|Andmetüüp|Kirjeldus|
|---|---|---|
|Filtreeritud massiiv|massiiv|Massiivi sisaldava objekti iga filtreeritud tulemi jaoks|

## <a name="next-steps"></a>Järgmised sammud

Nüüd, proovige platvormi ja [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md). Muude saadaolevate konnektorite loogika rakendustes saate uurida vaadates meie [API-de loendis](apis-list.md).
