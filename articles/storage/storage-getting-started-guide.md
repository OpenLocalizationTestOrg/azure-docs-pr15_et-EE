<properties
    pageTitle="Alustamine Azure Storage viie minuti | Microsoft Azure'i"
    description="Microsoft Azure'i plekid, tabeli ja järjekorrad abil Azure'i salvestusruumi kiirülevaate käivitab Visual Studio ja Azure storage emulaator ramp kiiresti üles. Käivitage esimene Azure Storage rakenduse viis minutit."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Viie minuti Azure Storage alustamine

## <a name="overview"></a>Ülevaade

See on lihtne arendamine Azure Storage. Selle õpetuse näidatakse, kuidas häälestada rakenduse Azure Storage ja töötab kiiresti. Saate kasutada Kiirkäivituse malle kaasas Azure'i SDK .net-i jaoks. Need kiiresti käivitab sisaldavad valmis-to-run kood, mis näitab Azure Storage mõne lihtsa programmeerimise stsenaariumi.

Lisateavet Azure Storage enne kui kood, vt [Järgmised toimingud](#next-steps).

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist peate järgmine kohustuslik tarkvara.

1. Koostada ja rakenduse, peate versiooni [Visual Studio](https://www.visualstudio.com/) teie arvutisse installitud.

2. Installige uusim versioon [Azure SDK .net-i jaoks](https://azure.microsoft.com/downloads/). SDK sisaldab Azure'i Kiirjuhend valimi projektide, Azure storage emulaator ja [Azure salvestusruumi kliendi teek .net-i jaoks](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Veenduge, et teil on [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) teie arvutisse installitud, nagu see on nõutav Azure Kiirjuhend valimi projektid, mida me kasutame selles õpetuses.

    Kui te pole kindel, millist versiooni .NET Framework on arvutisse installitud, lugege teemat [Kuidas: .NET Frameworki versioon on installitud](https://msdn.microsoft.com/vstudio/hh925568.aspx). Või vajutage nuppu **Start** või Windowsi klahvi, tippige **Juhtpaneel**. Klõpsake **programmide** > **programmid ja funktsioonid**, ja kontrollige, kas .NET Framework 4.5 on loetletud installitud programmide vahel.

4. Peate tellimuse Azure ja Azure storage konto.

    - Azure'i tellimuse saamiseks leiate [Tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/), [Ostu suvandid](https://azure.microsoft.com/pricing/purchase-options/)ja [Liikme pakub](https://azure.microsoft.com/pricing/member-offers/) (MSDN-i, Microsoft Partner Network, ja BizSpark ja muude Microsofti programmide puhul).
    - Azure storage konto loomiseks vaadake, [Kuidas luua salvestusruumi konto](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Käivitage esimene Azure Storage rakenduse vastu Azure Storage pilveteenuses

Kui olete konto, saate luua lihtsa Azure Storage rakenduse ühe Azure'i kiirülevaate käivitab valimi projekti Visual Studio abil. Selle õpetuse keskendub valimi projektide Azure Storage: **Azure Storage: plekid**, **Azure Storage: failide**, **Azure Storage: järjekorrad**, ja **Azure Storage: tabelite**:

1. Käivitage Visual Studio.
2. Klõpsake menüü **fail** käsku **Uus projekt**.
3. Klõpsake dialoogiboksis **Uus projekt** **installitud** > **mallide** > **Visual C#** > **pilve** > **QuickStarts** > **Data Services**.
    lisamine. Valige üks järgmistest mall: **Azure Storage: plekid**, **Azure Storage: failide**, **Azure Storage: järjekorrad**, või **Azure Storage: tabelite**.
    b. Veenduge, et **.NET Framework 4.5** on valitud eesmärgi raames.
    - 3.c. Määrake projekti nimi ja luua uue lahenduse Visual Studios, nagu on näidatud:

    ![Azure'i kiire käivitamise][Image1]

Soovite läbi vaadata lähtekoodi enne rakendus töötab. Vaadake üle koodi, valige **Solution Exploreris** Visual Studio menüü **Vaade** . Seejärel topeltklõpsake faili Program.cs.

Käivitage rakendus valimi järgmine:

1.  Visual Studio, valige menüü **Vaade** **Solution Exploreris** . Avage App.config faili ja kommentaari välja ühendusstringi Azure storage emulaatori:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Kommenteerige välja ühendusstringi teenuse Azure salvestusruumi ja salvestusruumi konto nimi ja Accessi võtit App.config faili.`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Salvestusruumi konto kiirklahv toomiseks vaadata [oma kiirklahvide salvestusruumi haldamine](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Pärast esitate salvestusruumikonto nimi ja kiirklahv App.config faili, klõpsake menüüs **fail** käsku **Salvesta kõik** projekti failid salvestada.
4.  Klõpsake menüü **koostada** **Lahenduse luua**.
5.  Menüü **silumine** vajutage **klahvi F11** käivitada lahenduse samm-sammult või vajutage klahvi **F5** lahendust käivitada.


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Käivitage esimene Azure Storage rakenduse kohalik vastu Azure'i salvestusruumi emulaator

[Azure'i salvestusruumi emulaator](storage-use-emulator.md) pakub kohaliku keskkond, mis jäljendab Azure'i bloobimälu, järjekord ja tabel teenuste arendamiseks. Saate salvestusruumi emulaator testimiseks salvestusruumi rakenduse kohalik, ilma Azure tellimuse või salvestusruumi konto loomiseks ja kulud, et tekiks.

Proovige, loome lihtsa Azure Storage rakenduse kasutamise ühe Azure'i kiirülevaate käivitab valimi projekti Visual Studios. Selle õpetuse keskendub **Azure'i bloobimälu**, **Azure'i Tabelimälu**ja **Azure järjekorda salvestusruumi** valimi projektide:

1. Käivitage Visual Studio.
2. Klõpsake menüü **fail** käsku **Uus projekt**.
3. Klõpsake dialoogiboksis **Uus projekt** **installitud** > **mallide** > **Visual C#** > **pilve** > **QuickStarts** > **Data Services**.
    lisamine. Valige üks järgmistest mall: **Azure Storage: plekid**, **Azure Storage: failide**, **Azure Storage: järjekorrad**, või **Azure Storage: tabelite**.
    b. Veenduge, et **.NET Framework 4.5** on valitud eesmärgi raames.
    c. Määrake projekti nimi ja luua uue lahenduse Visual Studios, nagu on näidatud:

    ![Azure'i kiire käivitamise][Image1]

4.  Visual Studio, valige menüü **Vaade** **Solution Exploreris** . Avage App.config faili ja kommentaari ühendusstringi Azure storage konto jaoks välja, kui olete juba lisanud ühte. Klõpsake kommentaari ühendusstringi Azure storage emulaatori:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Soovite läbi vaadata lähtekoodi enne rakendus töötab. Vaadake üle koodi, valige **Solution Exploreris** Visual Studio menüü **Vaade** . Seejärel topeltklõpsake faili Program.cs.

Järgmine näidis rakenduse käivitada Azure'i salvestusruumi emulaator:

1.  Vajutage nuppu **Start** või Windowsi klahv, *Microsoft Azure'i Tabelimäluga emulaator*, otsimine ja käivitage rakendus. Emulaator käivitamisel kuvatakse ikoon ja Windowsi Ülesandevaade alal teatis.
2.  Visual Studio, klõpsake menüü **koostada** **Lahenduse luua** .
3.  Menüü **silumine** vajutage **klahvi F11** lahenduse samm-sammult käivitamiseks või vajutage klahvi **F5** lahendus käivitamine algusest lõpuni.

## <a name="next-steps"></a>Järgmised sammud

Vaadake lisateavet Azure Storage neid materjale:

* [Microsoft Azure'i salvestusruumi tutvustus](storage-introduction.md)
* [Azure'i salvestusruumi Exploreri kasutamise alustamine](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md)
* [Azure'i tabelimälu kasutades .net-i kasutamise alustamine](storage-dotnet-how-to-use-tables.md)
* [Alustamine Azure'i järjekorda salvestusruumi .net-i abil](storage-dotnet-how-to-use-queues.md)
* [Alustamine Windows Azure'i failide talletamine](storage-dotnet-how-to-use-files.md)
* [AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)
* [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure'i salvestusruumi kliendi teek .net-i jaoks](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure'i salvestusruumi teenuste REST API-ga](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
