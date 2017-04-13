<properties 
   pageTitle="Konfigureerimine ja kasutamine salvestusruumi emulaator Visual Studio | Microsoft Azure'i"
   description="Konfigureerimine ja kasutamine salvestusruumi emulaator Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Konfigureerimine ja kasutamine salvestusruumi emulaator Visual Studio

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Ülevaade
Azure'i SDK arenduskeskkond sisaldab salvestusruumi emulaator, kasuliku, mis jäljendab bloobimälu, järjekord ja tabel salvestusruumi teenuste saadaval Azure kohaliku arengu teie arvutis. Kui olete koostamise pilveteenus, mis töötab Azure storage teenuseid või kirjutamine välise rakenduse, mis nõuab salvestusruumi teenuseid, saate testida oma koodi kohalikult peale salvestusruumi emulaator. Azure'i tööriistad Microsoft Visual Studio integreerida Visual Studio emulaator salvestusruumi haldamine. Azure'i tööriistad lähtestada salvestusruumi emulaator andmebaasi esmakordsel kasutamisel, käivitab emulaator salvestusteenus käivitamisel või silumine oma kood Visual Studio ja pakub kirjutuskaitstud juurdepääsu salvestusruumi emulaator andmetele Azure'i salvestusruumi Exploreri kaudu.

Salvestusruumi emulaator, sh süsteeminõuded ja kohandatud konfigureerimise juhised leiate üksikasjalikku teavet leiate teemast [Azure salvestusruumi emulaator ja testimine](./storage/storage-use-emulator.md).

>[AZURE.NOTE] On mõned funktsioonid salvestusruumi emulaator simulatsioon ja Azure storage teenuste vahelised erinevused. Leiate Azure'i SDK dokumentatsiooni teatud erinevuste kohta lisateavet [erinevused vahel salvestusruumi emulaator ja Azure salvestusruumi teenuste](./storage/storage-use-emulator.md) .

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Salvestusruumi emulaator ühendusstringi konfigureerimine

Juurdepääsu salvestusruumi emulaator koodi rollid, mida soovite ühendusstring, mis osutab salvestusruumi emulaator ja mida saate hiljem muuta osutamiseks Azure storage konto konfigureerimine. Ühendusstringi on Otsingukonfiguratsiooni säte, mis käitusajal salvestusruumi kontoga ühenduse loomiseks saate lugeda oma rolli. Ühendusstringi loomise kohta leiate lisateavet teemast [Azure rakenduse konfigureerimine](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

>[AZURE.NOTE] Atribuudi **DevelopmentStorageAccount** abil saate naasta salvestusruumi emulaator konto viide oma kood. Seda moodust töötab õigesti, kui soovite oma kood emulaator salvestusruumi juurde, kuid kui plaanite rakenduse Azure avaldada, peate juurdepääsu Azure storage konto ja kasutada seda ühendusstring enne selle avaldamist koodi muuta ühendusstringi loomine. Kui lähete salvestusruumi emulaator konto ja Azure storage konto sageli, on ühendusstringi protsessi hõlbustamiseks.

## <a name="initializing-and-running-the-storage-emulator"></a>Käivitamine ja töötab salvestusruumi emulaator

Saate määrata, et käivitamisel või silumine teenust Visual Studios, käivitab Visual Studio automaatselt salvestusruumi emulaator. Solution Exploreris **Azure'i** projekti kiirmenüü avamine ja valige **Atribuudid**. **Arengu** , **Alustage Azure salvestusruumi emulaator** loendis Valige menüü **True** (kui see pole juba määratud väärtusele).

Esmakordsel käivitamisel või silumine teenust Visual Studio, salvestusruumi emulaator käivitab lähtestamisel. Selle protsessi jätab salvestusruumi emulaatori kohaliku pordid ja loob salvestusruumi emulaator andmebaasi. Kui olete lõpetanud, ei pea seda toimingut uuesti käivitada, kui salvestusruumi emulaator andmebaas on kustutatud.

>[AZURE.NOTE] Alates juunist 2012 versiooni Azure'i tööriistad, salvestusruumi emulaator töötab, SQL-i kiire LocalDB vaikimisi. Azure'i tööriistade eelmiste versioonidega, salvestusruumi emulaator töötab vastu vaikimisi näiteks SQL Express 2005 või 2008, mis tuleb teil installida, enne kui saate installida Azure SDK. Saate käivitada ka salvestusruumi emulaator nimega eksemplar SQL Express või nimega või vaikimisi Microsoft SQL serveri eksemplar. Kui teil on vaja konfigureerida salvestusruumi emulaator käivitada vastu eksemplari peale vaikimisi eksemplari, vt [Azure'i salvestusruumi emulaator testimine ja kasutamine](./storage/storage-use-emulator.md).

Salvestusruumi emulaator sisaldab kasutajaliidese kohalikku teenuste oleku vaatamine ja alustada, peatada ja neid lähtestada. Kui emulaator salvestusteenus käivitas kasutajaliides või käivitamine või teenuse peatamine, paremklõpsates olekuala ikoon Microsoft Azure'i emulaatori Windowsi tegumiribal.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Server Explorer salvestusruumi emulaator andmete vaatamine

Sõlme Azure Storage Server Explorer võimaldab teil andmeid vaadata ja salvestusruumi kontod, sh salvestusruumi emulaator bloobimälu ja tabeli andmete sätete muutmine. Lisateabe saamiseks vaadake [sirvimine ja haldamise salvestusruumi ressursid serveri Exploreriga](https://msdn.microsoft.com/library/azure/ff683677.aspx) .
