<properties
   pageTitle="Rakenduse Visual Studio silumine | Microsoft Azure'i"
   description="Parandada töökindlust ja jõudlust teenuste arendamise ja silumine need Visual Studio kohaliku arengu klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Silumine teenuse struktuuri rakenduse Visual Studio abil

## <a name="debug-a-local-service-fabric-application"></a>Kohaliku teenuse struktuuri rakenduse silumine

Saate säästa aega ja raha juurutamine ja Azure teenuse struktuuri rakenduse kohaliku arvuti arengu klaster silumine. Visual Studio saate rakenduse kohaliku kobar ja siluri automaatselt ühenduse rakenduse kõik eksemplarid.

1. Kohaliku arengu kobar Alustuseks [oma teenuse struktuuri arenduskeskkond häälestamise](service-fabric-get-started.md)juhised.

2. Vajutage **klahvi F5** või klõpsake **silumine** > **Käivitamine silumine**.

    ![Käivitage rakendus silumine][startdebugging]

3. Murdepunkte kood ja samm rakenduse kaudu, klõpsates menüüs **silumine** käsud.

    > [AZURE.NOTE] Visual Studio manustab rakenduse kõik eksemplarid. Samal ajal, kui te kasutate hetkeks koodi kaudu, võib katkestuspunktid saada tabas mitut protsessi tulemuseks samaaegseid seansid. Keelake katkestuspunktide pärast ta olete tabas, muutes iga katkestuspunkt tingimusvormingu jutulõnga ID või diagnostika sündmuste abil.

4. **Diagnostika sündmuste** akna avab automaatselt nii, et saate vaadata diagnostika sündmuste reaalajas.

    ![Reaalajas vaate diagnostika sündmused][diagnosticevents]

5. Saate avada ka **Diagnostika sündmuste** akna Cloud Explorer.  Jaotises **Teenuse struktuuri**, paremklõpsake mis tahes sõlm ja valige **Vaate Streaming jälgi**.

    ![Diagnostika sündmuste akna avamine][viewdiagnosticevents]

    Kui soovite filtreerida teatud teenuse või rakenduse jälitusandmete, lihtsalt lubada streaming Jälgi seda kindla teenuse või rakenduse.

6. Diagnostika sündmuste näha automaatselt loodud **ServiceEventSource.cs** faili ja nimetatakse rakenduse koodi kaudu.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. **Diagnostika sündmuste** akna toetab filtreerimine, pausi ja sündmuste reaalajas kontrollimise.  Filter on lihtne string otsingu sündmuse sõnumi, sh selle sisu.

    ![Filtreerida, viige ja jätkamiseks ja sündmuste reaalajas uurimine][diagnosticeventsactions]

8. Teenuste silumine sarnaneb silumine muu rakendus. Tavaliselt seate katkestuspunktid Visual Studio kaudu lihtne silumine. Ehkki usaldusväärne saidikogumid korrata üle mitme sõlmed, rakendatakse endiselt IEnumerable. See tähendab, et saate Kuva tulemused Visual Studio ajal silumine kuvamiseks, mis on talletatud. Lihtsalt määrata katkestuspunkti suvalist koodi.

    ![Käivitage rakendus silumine][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Serveri teenuse struktuuri rakenduse silumine

Kui teenuse struktuuri rakenduste töötab teenuse struktuuri kobar Azure, teil on võimalik eemalt silumine neid otse Visual Studio.

> [AZURE.NOTE] Selle funktsiooni jaoks on vaja [teenuse struktuuri SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ja [Azure SDK.NET 2.9](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Remote silumine on mõeldud arendaja katse stsenaariumid, mitte kasutada Tootmiskeskkondades, töötavad rakendused mõju tõttu.

1. Liikuge klaster **Cloud Explorer**, paremklõpsake ja valige **Luba silumine**

    ![Luba Kaug silumine][enableremotedebugging]

    See on avalöök protsessi lubamine Kaug silumine laiend kobar sõlmed, samuti võrguga konfiguratsioone.

2. Paremklõpsake sõlme kobar **Cloud**Explorer ja valige **Siluri manustamine**

    ![Siluri manustamine][attachdebugger]

3. Dialoogiboksi **manustamine töötlemiseks** valige toiming, mida soovite silumine ja klõpsake käsku **Manusta**

    ![Valige protsess][chooseprocess]

    Protsessi, mida soovite manustada, nime võrdub nimi oma teenuse project komplekti nimi.

    Kõik sõlmed protsess on siluri manustada.
    - Juhul, kui teil on silumine kodakondsuseta teenus, on kõik eksemplarid teenuse kõik sõlmed silumine seansi osa.
    - Kui on silumine stateful teenus, ainult peamine koopia mis tahes sektsiooni on aktiivne ja seetõttu püütud siluri järgi. Kui silumine seansi jooksul liigub peamine koopia, selle koopia töötlemine ikkagi osa silumine seansi.
    - Selleks, et ainult püüdma asjakohaste sektsioonid või antud teenuseid, saate tingimusvormingu katkestuspunktid murda ainult teatud sektsiooni või eksemplari.

    ![Tingimusvormingu katkestuspunkt][conditionalbreakpoint]

    > [AZURE.NOTE] Praegu ei toeta silumine teenuse struktuuri kobar koos teenuse käivitatava samanimelist mitmes eksemplaris.

4. Kui olete lõpetanud, silumine rakenduse, saate keelata Kaug silumine laiend, paremklõpsates kobar **Cloud** Exploreris ja valida **Keelamine silumine**

    ![Keela Kaug silumine][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Streaming jälgi remote kobar sõlm

Teil on ka võimalik voo jälgi otse remote kobar sõlm Visual Studio. See funktsioon võimaldab teil voo ETW Jälita sündmuste toodeti teenuse struktuuri kobar sõlme otse rakenduses Visual Studio.

> [AZURE.NOTE] Selle funktsiooni jaoks on vaja [teenuse struktuuri SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ja [Azure SDK.NET 2.9](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Streaming jälgi on mõeldud arendaja katse stsenaariumid, mitte kasutada Tootmiskeskkondades, töötavad rakendused mõju tõttu.
> Tootmise stsenaariumi peaks toetuvad ümbersuunamine sündmuste Azure'i diagnostika abil.

1. Liikuge klaster **Cloud Explorer**, paremklõpsake ja valige **Luba Streaming jälgi**

    ![Remote streaming jälgi lubamine][enablestreamingtraces]

    See on avalöök protsessi lubamine streaming jälgi laiend kobar sõlmed, samuti võrguga konfiguratsioone.

2. Laiendage **sõlmed** elemendi **Cloud**Explorer, Paremklõpsake sõlme soovite voona jälgi ja valige käsk **Kuva Streaming jälgi**

    ![Vaate remote streaming jälgi][viewremotestreamingtraces]

    Korrake juhist 2 nii palju sõlmi, kui soovite näha jälgi. Iga sõlmed voo kuvatakse eraldi aknas.

    Olete nüüd jälgi teenuse struktuuri ja teenuste tekitatud näeksid. Kui soovite kuvada ainult mõne kindla rakenduse sündmuste filtreerida, tippige lihtsalt filtri rakenduse nimi.

    ![Streaming jälgi vaatamine][viewingstreamingtraces]

4. Kui olete teinud klaster kaudu videote voogesitamise jälgi, saate keelata remote streaming jälgi, paremklõpsates kobar **Cloud** Exploreris ja valida **Streaming jälgi keelamine**

    ![Remote streaming jälgi keelamine][disablestreamingtraces]

## <a name="next-steps"></a>Järgmised sammud

- [Teenuse struktuuri teenuse test](service-fabric-testability-overview.md).
- [Visual Studio teenuse struktuuri rakenduste haldamine](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
