<properties
    pageTitle="Azure'i masina õ Web Serivces portaalis veebiteenuse haldamine | Microsoft Azure'i"
    description="Azure'i masina õ tööruumid, juurdepääsu haldamine ja juurutada ja hallata ML API veebiteenused"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Azure'i masina õ veebiteenuste portaalis veebiteenuse haldamine

Saate hallata oma arvuti õ uue ja klassikaline veebiteenuste Microsoft Azure'i masina õ veebiteenuste portaalis. Kuna klassikaline veebiteenustele ja uued veebiteenuste põhinevad erinevaid aluseks tehnoloogiaid, on teil neid veidi erinevaid võimalusi.

Seadme õ veebiteenuste portaalis saate teha järgmist.

- Jälgida, kuidas veebiteenuse kasutatakse.
- Konfigureerimine kirjeldus, värskendada võtmed veebi jaoks teenuse (ainult uus), teie salvestusruumi konto klahv (uus ainult) Luba logimine, värskendada ja lubamine või keelamine näidisandmed.
- Kustutage veebiteenuse.
- Loomine, kustutamine või Värskenda arveldamine lepingute (ainult uus).
- Lisamine ja kustutamine lõpp-punktid (ainult klassikaline)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Uue veebiteenused haldamine

Teie uus veebiteenuste haldamiseks tehke järgmist.

1.  Microsoft Azure'i konto kaudu [Microsoft Azure'i masina õ veebiteenuste](https://services.azureml.net/quickstart) portaali sisse logida – selle kontoga, mis on Azure tellimusega seostatud.
2.  Klõpsake menüü, **Web Services**.

See kuvab loendi juurutatud veebiteenuste tellimuse. 

Veebiteenuse haldamiseks valige veebiteenused. Veebiteenused lehe kaudu saate teha järgmist.

- Klõpsake nuppu veebiteenus haldamiseks.
- Klõpsake soovitud arveldus kavandamine veebiteenuse värskendage seda.
- Kustutage veebiteenusest.
- Kopeerige veebiteenuse ja selle juurutama teises regioonis.

Veebiteenuse klõpsamisel avatakse veebileht teenuse Kiirjuhend. Teenuse Kiirjuhend veebilehe on kaks menüü Suvandid, mille abil saate hallata oma veebiteenus.

- **ARMATUURLAUA** – võimaldab teil vaadata veebi teenuse kasutamise.
- **KONFIGUREERIMINE** – võimaldab kirjeldava teksti lisamine, värskendamine seotud veebiteenuse salvestusruumi konto võti ja lubamine või keelamine näidisandmeid.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Kuidas veebiteenuse kasutab jälgimine ###

Klõpsake vahekaarti **ARMATUURLAUD** .

Armatuurlaual saate vaadata oma veebiteenuse üldine kasutamist aja jooksul. Saate valida perioodi kuvamiseks perioodi rippmenüü kasutus diagrammide paremas ülanurgas. Armatuurlaua kuvatakse järgmine teave:

- **Taotlused üle aja** kuvab valitud ajavahemiku jooksul samm diagrammi taotluste arv. See aitab tuvastada, kas teil on probleeme diagrammi kasutus.
- **Päringu vastuse taotlusi** kuvab päringu vastuse kõnede teenus on valitud ajavahemiku ja kui palju neid ei saanud koguarv.
- **Keskmine päringu vastuse arvutada kellaaeg** kuvatakse aega, et käivitada saadud taotlused keskmiseks.
- **Paketi taotlused** kuvatakse paketi taotlusi teenus on valitud ajavahemiku ja kui palju neid ei saanud koguarv.
- **Töö keskmise latentsuse** kuvab keskmiselt aega, et käivitada saadud taotlused.
- **Tõrked** kuvatakse ilmnenud tõrgete koguarvu veebiteenuse kõned.
- **Teenuste kulude** kuvab arvelduse plaani teenusega seotud kulude.

### <a name="configuring-the-web-service"></a>Veebiteenuse konfigureerimine ###

Klõpsake nuppu **Konfigureeri** menüüsuvandi.

Saate värskendada järgmised atribuudid:

* **Kirjeldus** , mis võimaldab teil sisestada veebiteenuse kirjelduse.
* **Jaotise** saate sisestada tiitel veebiteenuse
* **Klahvid** saate pöörata esmaseid ja teiseseid API võtmed.
* **Salvestusruumi konto võti** võimaldab teil värskendada seotud Web teenusemuudatusest salvestusruumi konto võti. 
* **Luba Näidisandmete** võimaldab saata päringu vastuse teenuse testimiseks kasutatavad näidisandmed. Kui olete loonud veebiteenuse masina õ Studios, näidisandmete on võetud andmed teie kasutatud koolitada mudelisse. Kui olete loonud teenuse programmiliselt, andmed on võetud JSON paketi osana esitatud näide andmeid.

### <a name="managing-billing-plans"></a>Lepingute arvelduse haldamine ###

Klõpsake teenuste Kiirjuhend veebilehelt menüüsuvandi **lepingud** . Võite klõpsata ka seotud kindla veebiteenuse leping selle lepingu haldamiseks.

* **Uus** võimaldab teil luua uue lepingu.
* **Lisa/eemalda lepingu eksemplari** võimaldab "mastaapimiseks välja" olemasoleva plaani lisada võimsus.
* **Uuele versioonile/Allavahetamise** võimaldab "skaalal" olemasoleva plaani lisada võimsus.
* **Kustutada** saate kustutada leping.

Klõpsake selle armatuurlaua kuvamiseks plaani. Armatuurlaua annab teile hetktõmmise või lepingu kasutamine valitud aja jooksul. Aja jooksul kuvamiseks valimiseks klõpsake **perioodi** ripploend armatuurlaua paremas ülaosas. 

Lepingu armatuurlaud on järgmine teave:

* **Lepingu kirjeldus** kuvatakse teave kulude ja plaaniga seotud võimsus.
* **Lepingu kasutamine** kuvab arvu tehingud ja Arvuta tundi, mis on antud maksta leping.
* **Veebiteenused** kuvatakse arv, mis on selle lepingu kasutamise.
* **Top Web Service, kõnede** kuvab neli veebiteenuseid, mis on helistamist, mis on kavas maksta.
* **Ülemine veebiteenuste arvutada tundi,** kuvab neli veebiteenuseid, mis kasutavad Arvuta ressursse, mis on kavas maksta.

## <a name="manage-classic-web-services"></a>Klassikaline veebiteenuste haldamine

> [AZURE.NOTE] Selles jaotises on oluline haldamise klassikaline veebiteenuste Azure seadme õ veebiteenuste portaali kaudu. Klassikaline veebiteenuste masina õ Studio ja Azure klassikaline portaali kaudu haldamise kohta leiate teavet teemast [mõne Azure seadme õ tööruumi](machine-learning-manage-workspace.md).

Teie klassikaline veebiteenuste haldamiseks tehke järgmist.

1.  Microsoft Azure'i konto kaudu [Microsoft Azure'i masina õ veebiteenuste](https://services.azureml.net/quickstart) portaali sisse logida – selle kontoga, mis on Azure tellimusega seostatud.
2.  Klõpsake menüü **Klassikaline veebiteenused**.

Klassikaline veebiteenuse haldamiseks valige **Klassikaline veebiteenused**. Klassikaline veebiteenuste lehe kaudu saate teha järgmist.

- Klõpsake nuppu veebiteenus vaatamiseks seotud lõpp-punktid.
- Kustutage veebiteenusest.

Kui haldate klassikaline veebiteenuse, saate hallata iga lõpp-punktid eraldi. Lehel Web Services veebiteenuse klõpsamisel avaneb loendi teenusega seotud lõpp-punktid. 

Klõpsake lehel klassikaline veebiteenuse lõpp-punkti saate lisada ja kustutada teenuse lõpp-punktid. Lõpp-punktid lisamise kohta leiate lisateavet teemast [Loomise lõpp-punktid](machine-learning-create-endpoint.md).

Klõpsake teenuse Kiirjuhend veebilehe avamiseks lõpp-punktid. Klõpsake lehel Kiirjuhend on kaks menüü Suvandid, mille abil saate hallata oma veebiteenus.

- **ARMATUURLAUA** – võimaldab teil vaadata veebi teenuse kasutamise.
- **KONFIGUREERIMINE** – võimaldab kirjeldava teksti lisamine, Lülita logimine sisse ja välja, salvestusruumi konto võti seotud veebiteenuse, värskendamine ja lubamine ja keelamine näidisandmeid.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Kuidas kasutatakse veebiteenuse jälgimine ###

Klõpsake vahekaarti **ARMATUURLAUD** .

Armatuurlaual saate vaadata oma veebiteenuse üldine kasutamist aja jooksul. Saate valida perioodi kuvamiseks perioodi rippmenüü kasutus diagrammide paremas ülanurgas. Armatuurlaua kuvatakse järgmine teave:

- **Taotlused üle aja** kuvab valitud ajavahemiku jooksul samm diagrammi taotluste arv. See aitab tuvastada, kas teil on probleeme diagrammi kasutus.
- **Päringu vastuse taotlusi** kuvab päringu vastuse kõnede teenus on valitud ajavahemiku ja kui palju neid ei saanud koguarv.
- **Keskmine päringu vastuse arvutada kellaaeg** kuvatakse aega, et käivitada saadud taotlused keskmiseks.
- **Paketi taotlused** kuvatakse paketi taotlusi teenus on valitud ajavahemiku ja kui palju neid ei saanud koguarv.
- **Töö keskmise latentsuse** kuvab keskmiselt aega, et käivitada saadud taotlused.
- **Tõrked** kuvatakse ilmnenud tõrgete koguarvu veebiteenuse kõned.
- **Teenuste kulude** kuvab arvelduse plaani teenusega seotud kulude.

### <a name="configuring-the-web-service"></a>Veebiteenuse konfigureerimine ###

Klõpsake nuppu **Konfigureeri** menüüsuvandi.

Saate värskendada järgmised atribuudid:

* **Kirjeldus** , mis võimaldab teil sisestada veebiteenuse kirjelduse. Kirjeldus on nõutav väli.
* **Logimise** võimaldab teil lubada või keelata tõrge logimise lõpp-punkti. Logimise kohta leiate lisateavet teemast luba [logimine masina õ veebiteenuste jaoks](machine-learning-web-services-logging.md).
* **Luba Näidisandmete** võimaldab saata päringu vastuse teenuse testimiseks kasutatavad näidisandmed. Kui olete loonud veebiteenuse masina õ Studios, näidisandmete on võetud andmed teie kasutatud koolitada mudelisse. Kui olete loonud teenuse programmiliselt, andmed on võetud JSON paketi osana esitatud näide andmeid.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Anda või peatada portaalis veebiteenuste kasutajate jaoks

Azure'i klassikaline portaali kaudu, saate lubada või keelata juurdepääsu kindlatele kasutajatele.

### <a name="access-for-users-of-new-web-services"></a>Kasutajate uued veebiteenuste jaoks

Teiste kasutajate töötamiseks oma veebiteenuste Azure seadme õ veebiteenuste portaalis lubamiseks peate lisama need co-adminstrators Azure tellimuse.

Microsoft Azure'i konto kaudu [Azure'i klassikaline portaali](https://manage.windowsazure.com/) sisse logida – selle kontoga, mis on Azure tellimusega seostatud.

1. Navigeerimispaanil, klõpsake nuppu **sätted**ja seejärel nuppu **Administraatorid**.
2. Klõpsake akna allosas nuppu **Lisa**. 
3. Tippige dialoogiboksis lisa A koostöö administraator soovite koostöö administraator lisada, ja seejärel valige tellimus, mida soovite koostöö administraator juurdepääsu isiku meiliaadress.
4. Klõpsake nuppu **Salvesta**.

### <a name="access-for-users-of-classic-web-services"></a>Klassikaline veebiteenuste kasutajate jaoks

Tööruumi haldamiseks tehke järgmist.

Microsoft Azure'i konto kaudu [Azure'i klassikaline portaali](https://manage.windowsazure.com/) sisse logida – selle kontoga, mis on Azure tellimusega seostatud.

1. Klõpsake paanil Microsoft Azure'i teenuste **Masina õ**.
1. Klõpsake tööruumi, mida soovite hallata.
1. Klõpsake vahekaarti **KONFIGUREERIMINE** .

Menüü konfiguratsiooni saate peatada arvuti õ tööruumi juurdepääsu, klõpsates nuppu **Keela**. Kasutaja saab enam tööruumi avada seadme õ Studios. Juurdepääsu taastamiseks klõpsake nuppu **Luba**.

Kindlatele kasutajatele:

Hallata täiendavaid kontosid, kellel on juurdepääs arvuti õ Studios tööruumi, klõpsake nuppu **Logi sisse ML Studio** **ARMATUURLAUA** vahekaardil. Selle tegemisel avaneb tööruumi masina õ Studio. Siit, klõpsake vahekaarti **sätted** ja seejärel **Kasutajad**. Võite klõpsata **KUTSU rohkem kasutajaid** portaalisaidile juurdepääsu tööruumi, või valige kasutaja ja klõpsake nuppu **Eemalda**.

> [AZURE.NOTE] **Sisselogimine ML Studio** link avab arvuti õ Studio te praegu sisse logitud Microsofti Account. Microsoft Azure'i klassikaline portaali sisse logida tööruumi loomiseks kasutatud Account ei ole automaatselt selle tööruumi avamiseks luba. Tööruumi avamiseks peate olema sisse logitud Microsofti Account, mis on määratletud tööruumi omanik või peate kutse liituda tööruumi omanik.
