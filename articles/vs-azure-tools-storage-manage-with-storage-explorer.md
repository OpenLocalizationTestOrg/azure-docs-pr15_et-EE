<properties
    pageTitle="Alustamine: salvestusruumi Explorer (eelvaade) | Microsoft Azure'i"
    description="Hallata Azure storage ressursid salvestusruumi Exploreriga (eelvaade)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Alustamine: salvestusruumi Explorer (eelvaade)

## <a name="overview"></a>Ülevaade 

Microsoft Azure'i salvestusruumi Explorer (eelvaade) on autonoomne rakendus, mis võimaldab teil hõlpsasti andmetega töötamiseks Azure Storage Windows, OS X ja Linux. Selles artiklis õpite mitmesuguseid viise ning ühenduse loomise ja haldamise Azure'i salvestusruumi kontod.

![Microsoft Azure'i salvestusruumi Explorer (eelvaade)][15]

## <a name="prerequisites"></a>Eeltingimused

- [Laadige alla ja installige salvestusruumi Explorer (eelvaade)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Salvestusruumi konto või teenuse ühenduse loomine

Salvestusruumi Explorer (eelvaade) pakub hulgaliselt viisid salvestusruumi kontodega ühendus luua. See sisaldab ühenduse salvestusruumi kontode ja teenuste ühiskasutuses muude Azure'i tellimused oma Azure tellimustega seotud salvestusruumi kontodega ühenduse loomise ja isegi ühenduse ja kasutades Azure salvestusruumi emulaator kohaliku salvestusruumi haldamine.

- [Ühenduse loomine Azure tellimuse](#connect-to-an-azure-subscription) - kuuluvad Azure tellimuse salvestusruumi vahendid haldamine.
- [Töötamine kohaliku arengu salvestusruumi](#work-with-local-development-storage) - abil Azure'i salvestusruumi emulaator kohaliku salvestusruumi haldamine. 
- [Manusta välisesse salvestusruumi](#attach-or-detach-an-external-storage-account) - Halda salvestusruumi ressursse, mis kuuluvad mõnele muule Azure tellimusele mäluruumi konto konto nimi ja klahvi abil.
- [Manusta salvestusruumi konto abil SAS](#attach-storage-account-using-sas) - Halda salvestusruumi ressursse, mis kuuluvad teise Azure tellimuse abil soovitud SAS.
- [Manusta teenuse abil SAS](#attach-service-using-sas) - haldamine teatud salvestusteenus (bloobimälu container, järjekorda või tabel) kuuluvad teise Azure'i tellimus on SAS abil.

## <a name="connect-to-an-azure-subscription"></a>Ühenduse loomine Azure tellimuse

> [AZURE.NOTE] Kui teil pole Azure'i konto, saate [tasuta prooviversiooni kasutajaks](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) või [aktiveerida oma Visual Studio abonendi eelised](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Valige salvestusruumi Exploreris (eelvaade) **Azure'i konto sätted**. 

    ![Azure'i konto sätted][0]

1. Klõpsake vasakpoolsel paanil kuvatakse nüüd kõik te olete sisse logitud Microsofti kontod. Ühenduse loomine mõne muu kontoga, valige **konto lisamine**ja järgige dialoogid logima sisse Microsofti kontoga, mis on seotud vähemalt üks aktiivne Azure'i tellimus.

    ![Konto lisamine][1]

1. Kui edukalt logite sisse Microsofti kontoga, vasakul paanil asustada selle kontoga seostatud Azure'i tellimused. Valige Azure'i tellimused, millega soovite töötada, ja seejärel valige **Rakenda**. (Valides **kõik tellimused** tavalise kirja vahel valimine kõik või mitte ühtegi loetletud Azure'i tellimused).

    ![Valige Azure'i tellimused][3]

1. Klõpsake vasakpoolsel paanil kuvatakse nüüd salvestusruumi kontod, mis on seotud valitud Azure'i tellimused.

    ![Valitud Azure'i tellimused][4]

## <a name="work-with-local-development-storage"></a>Kohaliku arengu salvestusruumi töötamine

Salvestusruumi Explorer (eelvaade) võimaldab töötada kohaliku salvestusruumi kasutamise Azure'i salvestusruumi emulaator. See võimaldab teil esitada koodi kirjutamine ja testimine salvestusruumi ilma tingimata salvestusruumi konto, juurutada Azure (kuna salvestusruumi konto on emuleeritakse Azure'i salvestusruumi emulaator).

>[AZURE.NOTE] Azure'i salvestusruumi emulaator ei toeta praegu ainult Windows. 

1. Laiendage vasakpoolsel paanil salvestusruumi Explorer (eelvaade), on **(Local ja lisatud** > **Salvestusruumi kontod** > sõlm**(arendamine)** .

    ![Kohaliku arengu sõlm][21]

1. Kui te pole veel installinud Azure'i salvestusruumi emulaator, palutakse teil teha läbi kuvatakse teaberibal. Kui kuvatakse teaberibal, valige **uusim versioon alla laadida**ja installida emulaator. 

    ![Azure'i salvestusruumi emulaator viip allalaadimine][22]

1. Kui emulaator on installitud, siis on teil võimalus loomine ja nendega töötamine kohaliku plekid, järjekorrad ja tabeleid. Saate teada, kuidas töötada iga salvestusruumi konto tüüp, valige vastavat linki.

    - [Azure'i bloobimälu salvestusruumi ressursside haldamine](./vs-azure-tools-storage-explorer-blobs.md)
    - Azure'i faili ühiskasutusse andmine salvestusruumi ressursid – *Varsti tulekul* haldamine
    - Azure'i järjekorda salvestusruumi ressursid – *Varsti tulekul* haldamine
    - Azure'i tabeli salvestusruumi ressursid – *Varsti tulekul* haldamine

## <a name="attach-or-detach-an-external-storage-account"></a>Manustamine ja välise salvestusruumi konto eemaldamine

Võimalus lisada välise salvestusruumi kontod, et salvestusruumi kontod saab hõlpsasti jagada pakub salvestusruumi Explorer (eelvaade). Selles jaotises selgitatakse, kuidas lisada (ja eemaldada) välise salvestusruumi kontod.

### <a name="get-the-storage-account-credentials"></a>Salvestusruumi konto mandaadi

Konto väliselt ühiskasutusse andmiseks peate selle konto omanik kõigepealt mandaadi - konto nimi ja klahvi - konto ja seejärel selle teabe jagamine isik, kes soovivad manustamiseks (väline) konto. Salvestusruumi konto mandaadi saamiseks saab teha Azure portaali kaudu järgmist: 

1.  [Azure'i portaali](https://portal.azure.com)sisse logida.
1.  Valige **Sirvi**.
1.  Valige **salvestusruumi kontod**.
1.  **Salvestusruumi kontod** tera, valige soovitud salvestusruumi konto.
1.  Valige valitud salvestusruumi konto **sätted** labale **kiirklahvide**.

    ![Accessi klahvid suvand][5]
    
1.  Kopeerige labale **kiirklahvide** kasutamiseks **SALVESTUSRUUMIKONTO nimi** ja **KEY 1** väärtused manustamisel salvestusruumi kontole. 

    ![Kiirklahvid][6]

### <a name="attach-to-an-external-storage-account"></a>Välise salvestusruumi konto manustamine
Välise salvestusruumi konto lisamiseks peate konto nimi ja võti. *Salvestusruumi konto mandaadi* jaotis selgitab hankimine need väärtused Azure portaalist. Pange tähele, et portaalis konto võti nimetatakse "võti 1" nii kui on konto võti palub salvestusruumi Explorer (eelvaade), kuvatakse sisestate (või kleepige) väärtus "võti 1". 
 
1.  Valige salvestusruumi Exploreris (eelvaade) **ühenduse Azure'i salvestusruumi**.

    ![Ühenduse loomine Azure storage võimalus][23]

1.  Klõpsake dialoogiboksis **ühenduse Azure Storage** määrata konto võti (Azure'i portaalis väärtuse "võti 1") ja valige **edasi**.

    ![Azure'i salvestusruumi dialoogiboks ühenduse loomine][24] 

1.  Dialoogiboksi **Manustamine välise salvestusruumi** Sisestage salvestusruumi konto nimi väljale **konto nimi** Määrake muud soovitud sätted ja valige **edasi** kui valmis. 

    ![Välise salvestusruumi dialoogiboksi manustamine][8]

1.  Kontrollige dialoogiboksis **Ühenduse Kokkuvõte** teavet. Kui soovite midagi muuta, klõpsake nuppu **tagasi** ja sisestage uuesti soovitud sätted. Kui olete lõpetanud, valige **Ühenda**.

1.  Kui ühendus on loodud, kuvatakse väliste salvestusruumi konto **(väline)** salvestusruumi konto nimi, lisatud tekstiga. 

    ![Tulemi ühenduse välise salvestusruumi konto.][9]

### <a name="detach-from-an-external-storage-account"></a>Välise salvestusruumi konto eemaldamine

1.  Paremklõpsake välise salvestusruumi kontot, mida soovite eemaldada, ja - valige kontekstimenüüst - **lahti**.

    ![Salvestusruumi suvand eemaldada][10]

1.  Kui kuvatakse teateboks kinnituse, valige **Jah** välise salvestusruumi konto lahtiütlemisele kinnitamiseks.

## <a name="attach-storage-account-using-sas"></a>Salvestusruumi konto abil SAS manustamine

Mõne [SAS (ühiskasutusse Accessi allkiri)](storage/storage-dotnet-shared-access-signature-part-1.md) annab admin Azure tellimuse võimalus anda juurdepääsu salvestusruumi konto ajutiselt ilma vajaduseta volituste Azure'i tellimus. 

Kujutamiseks, Oletagem UserA on Azure tellimuse administraator ja UserA soovib UserB piiratud õigustega teatud aja jooksul salvestusruumi konto juurdepääsu lubamiseks:

1. UserA loob mõne SAS (koosneb ühendusstringi salvestusruumi konto) ja soovitud õigused teatud aja jooksul.
1. UserA jagab muude isik, kes soovivad juurdepääsu salvestusruumi konto - UserB, siinses näites.  
1. UserB kasutab manustamiseks kuuluvate UserA abil esitatud muude konto salvestusruumi Explorer (eelvaade). 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Hankida soovitud SAS konto, mille soovite ühiskasutusse anda

1.  Salvestusruumi Exploreris (eelvaade), paremklõpsake salvestusruumi konto, mida soovite jagada ja - valige kontekstimenüüst - **Saada ühiskasutusse Accessi allkirja**.

    ![SAS kontekstimenüü suvandit saada][13]

1. Klõpsake dialoogiboksis **Ühiskasutuses Accessi signatuuri** määrata ajavahemik ja õigused, mille soovite konto ja valige **Loo**.

    ![Saada SAS dialoogiboks][14]
 
1. Teise **Ühiskasutusse antud juurdepääs allkirja** dialoogiboks kuvatakse muude kuvamise. Valige **Kopeeri** kõrval **Ühendusstringi** kopeerimine lõikelauale. Valige **Sule** hülgamiseks dialoogiboks.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Ühiskasutusega konto abil muude manustamine

1.  Valige salvestusruumi Exploreris (eelvaade) **ühenduse Azure'i salvestusruumi**.

    ![Ühenduse loomine Azure storage võimalus][23]

1.  Klõpsake dialoogiboksis **ühenduse Azure Storage** määrata ühendusstring ja valige **edasi**.

    ![Azure'i salvestusruumi dialoogiboks ühenduse loomine][24] 

1.  Kontrollige dialoogiboksis **Ühenduse Kokkuvõte** teavet. Kui soovite midagi muuta, klõpsake nuppu **tagasi** ja sisestage uuesti soovitud sätted. Kui olete lõpetanud, valige **Loo ühendus**.

1.  Kui lisatud, kuvatakse tekstiga (SAS), mis on lisatud pakutud konto nimi konto salvestusruumi.

    ![Ühendatud konto abil SAS tulemus][17]

## <a name="attach-service-using-sas"></a>Manusta teenuse abil SAS

Jaotise [manustamine salvestusruumi konto abil SAS](#attach-storage-account-using-sas) näitab, kuidas Azure tellimuse administraator saab anda ajutist juurdepääsu salvestusruumi konto luues (ja jagamine) SAS, salvestusruumi konto. Samuti saab luua mõne SAS kindla teenuse (bloobimälu container, järjekorda või tabel) salvestusruumi kontol.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Luua SAS, teenusele, mille soovite ühiskasutusse anda

Siinkohal saab teenuse bloobimälu container, järjekorda või tabel. Järgmistes jaotistes selgitatakse, kuidas luua muude loetletud teenuse kohta:

- [Hankida muude bloobimälu container](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Faili ühiskasutus – *Varsti tulekul* muude hankimine
- Hankida muude järjekorda – *Varsti tulekul*
- Hankida muude tabeli – *Varsti tulekul*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Ühiskasutusega konto teenuse abil muude manustamine

1.  Valige salvestusruumi Exploreris (eelvaade) **ühenduse Azure'i salvestusruumi**.

    ![Ühenduse loomine Azure storage võimalus][23]

1.  Klõpsake dialoogiboksis **ühenduse Azure Storage** määrata SAS URI ja valige **edasi**.

    ![Azure'i salvestusruumi dialoogiboks ühenduse loomine][24] 

1.  Kontrollige dialoogiboksis **Ühenduse Kokkuvõte** teavet. Kui soovite midagi muuta, klõpsake nuppu **tagasi** ja sisestage uuesti soovitud sätted. Kui olete lõpetanud, valige **Ühenda**.

1.  Pärast ühendatud, kuvatakse äsja lisatud teenuse **(Teenuse SAS)** sõlme all.

    ![Tulemi manustamise ühiskasutusega teenuse abil SAS.][20]

## <a name="search-for-storage-accounts"></a>Otsige salvestusruumi kontod

Kui teil on pikk loendi salvestusruumi kontod, on kiiresti leida kindla salvestusruumi konto kasutamiseks vasakpoolse paani ülaosas välja Otsing. 

Otsinguväljale tippimisel kuvatakse vasakul paanil ainult selle salvestusruumi kontod, mis vastavad otsing väärtus sisestatud kuni viitavate. Näide, kus olete otsida kõik salvestusruumi kontod, kus on salvestusruumikonto nimi sisaldab teksti "tarcher" on näidatud järgmisel kuvatõmmisel.

![Salvestusruumi konto otsing][11]
    
Otsingu eemaldamiseks valige otsinguvälja nuppu **x** .

## <a name="next-steps"></a>Järgmised sammud
- [Azure'i bloobimälu salvestusruumi ressursid salvestusruumi Exploreriga (eelvaade) haldamine](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
