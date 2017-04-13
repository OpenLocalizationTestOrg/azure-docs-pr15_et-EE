<properties
    pageTitle="Seadme õ tööruumi haldamine | Microsoft Azure'i"
    description="Azure'i masina õ tööruumid, juurdepääsu haldamine ja juurutada ja hallata ML API veebiteenused"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Mõne Azure seadme õ tööruumi haldamine

>[AZURE.NOTE] Selles artiklis on Azure seadme õ klassikaline veebiteenuste jaoks oluline. Veebiteenuste masina õ veebiteenuste portaali haldamise kohta leiate teavet teemast [Azure seadme õ veebiteenuste portaalis veebiteenusest](machine-learning-manage-new-webservice.md).

Azure'i klassikaline portaali saab hallata oma arvuti õ tööruumid.

- Kuidas tööruum on kasutusel jälgimine
- Tööruumi lubada või keelata juurdepääsu konfigureerimine
- Veebiteenused loodud tööruumis haldamine
- Tööruumi kustutamine

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Lisaks vahekaart armatuurlaud pakub ülevaadet oma tööruumi kasutus ja tööruumi teabe hoomatavad.  

> [AZURE.TIP] Azure'i masina õ Studio menüü **VEEBITEENUSTE** saate lisada, värskendada või kustutada seadme õ veebiteenus.

Tööruumi haldamiseks tehke järgmist.

1.  Microsoft Azure'i konto kaudu [Azure'i klassikaline portaali](https://manage.windowsazure.com/) sisse logida – selle kontoga, mis on Azure tellimusega seostatud.
2.  Klõpsake paanil Microsoft Azure'i teenuste **Masina õ**.
3.  Klõpsake tööruumi, mida soovite hallata.

Tööruumi leheküljel on kolm vahekaarti:

- **ARMATUURLAUA** – võimaldab teil vaade tööruumi kasutus ja teave
- **KONFIGUREERIMINE** – võimaldab tööruumi juurdepääsu haldamine
- **VEEBITEENUSED** – saate hallata, mis on avaldatud see tööruumist

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Jälgida, kuidas tööruum on kasutusel

Klõpsake vahekaarti **ARMATUURLAUD** .

Armatuurlaual saate vaadata oma tööruumi üldine kasutamist ja hoomatavad tööruumi teabe saamiseks.

- **ARVUTAGE** diagrammil kuvatakse Arvuta ressursid, mida kasutavad tööruumi. Saate muuta vaate kuvamiseks suhteline või absoluutväärtuse ja saate muuta diagrammil kuvatavate ajakava.
- **Kasutus ülevaade** kuvab Azure'i salvestusruumi tööruumi, mida kasutavad.
- **Kiire ülevaade** annab ülevaate tööruumi teabe ja kasulikud lingid.

> [AZURE.NOTE] **Sisselogimine ML Studio** link avab arvuti õ Studio te praegu sisse logitud Microsofti Account. Microsoft Azure'i klassikaline portaali sisse logida tööruumi loomiseks kasutatud Account ei ole automaatselt selle tööruumi avamiseks luba. Tööruumi avamiseks peate olema sisse logitud Microsofti Account, mis on määratletud tööruumi omanik või peate kutse liituda tööruumi omanik.


## <a name="to-grant-or-suspend-access-for-users"></a>Anda või peatada kasutajate jaoks ##

Klõpsake vahekaarti **KONFIGUREERIMINE** .

Konfiguratsiooni menüüs saate teha järgmist.

- Seadme õ tööruumi peatada, klõpsates nuppu Keela. Kasutaja saab enam tööruumi avada seadme õ Studios. Juurdepääsu taastamiseks klõpsake nuppu Luba.

Täiendavate kontode, kellel on juurdepääs arvuti õ Studios tööruumi haldamiseks klõpsake **sisselogimine ML Studio** **ARMATUURLAUA** vahekaardil (vt eelmistel Märkus kohta **ML Studio sisselogimine**). Selle tegemisel avaneb tööruumi masina õ Studio. Siit, klõpsake vahekaarti **sätted** ja seejärel **Kasutajad**. Võite klõpsata **KUTSU rohkem kasutajaid** portaalisaidile juurdepääsu tööruumi, või valige kasutaja ja klõpsake nuppu **Eemalda**.


## <a name="to-manage-web-services-in-this-workspace"></a>Selle tööruumi veebiteenuste haldamine

Klõpsake vahekaarti **VEEBITEENUSED** .

Kuvatakse see tööruumist avaldatud veebiteenuste loendit.
Veebiteenuse haldamiseks klõpsake nime loendis teenuse veebilehe avamiseks.

Veebiteenuse võib olla üks või mitu lõpp-punktid määratletud.

- Saate määratleda rohkem lõpp-punkte Lisaks "Vaikimisi" lõpp-punkti. Lõpp-punkti lisamiseks klõpsake nuppu **Halda lõpp-punktid** armatuurlaud allosas Azure seadme õ Web Services portaal.

- Lõpp (ei saa kustutada "Vaikimisi" lõpp-punkti) kustutamiseks klõpsake nuppu lõpp-punkti rea alguses olev ruut ja klõpsake nuppu **Kustuta**. Lõpp-punkti eemaldatakse veebiteenusest.

    > [AZURE.NOTE] Kui rakendus kasutab web teenuse lõpp-punkti lõpp-punkti kustutamisel, kuvatakse rakenduse tõrge järgmine kord, kui püüab teenuse.

Klõpsake soovitud Web teenuse lõpp-punkti avamiseks nime. 

Armatuurlaual saate vaadata oma veebiteenuse üldine kasutamist aja jooksul. Saate valida perioodi kuvamiseks perioodi rippmenüü kasutus diagrammide paremas ülanurgas. Armatuurlaua kuvatakse järgmine teave:

- **Taotlused üle aja** kuvab valitud ajavahemiku jooksul samm diagrammi taotluste arv. See aitab tuvastada, kas teil on probleeme diagrammi kasutus.
- **Päringu vastuse taotlusi** kuvab päringu vastuse kõnede teenus on valitud ajavahemiku ja kui palju neid ei saanud koguarv.
- **Keskmine päringu vastuse arvutada kellaaeg** kuvatakse aega, et käivitada saadud taotlused keskmiseks.
- **Paketi taotlused** kuvatakse paketi taotlusi teenus on valitud ajavahemiku ja kui palju neid ei saanud koguarv.
- **Töö keskmise latentsuse** kuvab keskmiselt aega, et käivitada saadud taotlused.
- **Tõrked** kuvatakse ilmnenud tõrgete koguarvu veebiteenuse kõned.
- **Teenuste kulude** kuvab arvelduse plaani teenusega seotud kulude.

Lehelt konfigureerimine saate värskendada järgmised atribuudid:

* **Kirjeldus** , mis võimaldab teil sisestada veebiteenuse kirjelduse. Kirjeldus on nõutav väli.
* **Logimise** võimaldab teil lubada või keelata tõrge logimise lõpp-punkti. Logimise kohta leiate lisateavet teemast luba [logimine masina õ veebiteenuste jaoks](machine-learning-web-services-logging.md).
* **Luba Näidisandmete** võimaldab saata päringu vastuse teenuse testimiseks kasutatavad näidisandmed. Kui olete loonud veebiteenuse masina õ Studios, näidisandmete on võetud andmed teie kasutatud koolitada mudelisse. Kui olete loonud teenuse programmiliselt, andmed on võetud JSON paketi osana esitatud näide andmeid.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
