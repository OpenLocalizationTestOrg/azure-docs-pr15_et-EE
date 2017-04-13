<properties
    pageTitle="Laadi testida rakenduse Visual Studio Team Services abil | Microsoft Azure'i"
    description="Saate teada, kuidas rõhutada testi Azure teenuse struktuuri rakenduste Visual Studio Team Services abil."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Laadi testida rakenduse Visual Studio Team Services abil

Selles artiklis kirjeldatakse, kuidas kasutada Microsoft Visual Studio laadi funktsioonide katsetamiseks rõhutada testi rakenduse. See kasutab Azure teenuse struktuuri stateful teenuse tagasi end ja kodakondsuseta teenuse web esiosa. Näide rakendus, mida kasutatakse siin on mõni lennukikujuline asukoht simulaatorit. Esitate lennukikujuline ID, väljumis- ja sihtkohta. Rakenduse tagasi end töötleb taotluste ja esiosa kuvatakse kaardil lennukikujuline, mis vastab kriteeriumidele.

Järgmine diagramm näitab teenuse struktuuri rakendus, mis teil tuleb testimine.

![Näide lennukikujuline asukoht rakenduse skeem][0]

## <a name="prerequisites"></a>Eeltingimused
Enne alustamist, peate tegema järgmist:

- Visual Studio Team Services konto. Saate ükshaaval tasuta [Visual Studio Team Services](https://www.visualstudio.com).
- Saada ja installige Visual Studio 2013 või Visual Studio 2015. Selles artiklis kasutab Visual Studio 2015 Enterprise edition, kuid Visual Studio 2013 ja teised versioonid peaks toimima samamoodi.
- Juurutada rakendust lavastus keskkonnas. Selle kohta lisateabe saamiseks vaadake, [Kuidas juurutada rakendused remote arvutikobaras Visual Studio abil](service-fabric-publish-app-remote-cluster.md) .
- Rakenduse kasutamine mudelit mõista. Seda teavet kasutatakse laadi mustri simuleerida.
- Laadi kontrollimise eesmärk mõista. See aitab teil tõlgendada ja analüüsida laadi testi tulemused.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Loomine ja käivitamine project Web jõudlus ja testi laadimine

### <a name="create-a-web-performance-and-load-test-project"></a>Web jõudlus ja laadi testi projekti loomine

1. Avage Visual Studio 2015. Valige **fail** > **Uus** > **projekti** menüü **Uue projekti** dialoogiboksi avamiseks.

2. Laiendage **Visual C#** ja valige **testi** > **Web jõudlus ja laadi Test projekti**. Anda projekti nime ja seejärel klõpsake nuppu **OK** .

    ![Kuvatõmmis dialoogiboksi Uus projekt][1]

    Peaksite nägema Solution Exploreris Web jõudlus ja laadi testi uue projekti.

    ![Kuvatõmmis Solution Exploreris, kus on kuvatud uue projekti][2]

### <a name="record-a-web-performance-test"></a>Kirje web jõudluse test

1. Avage projekt .webtest.

2. Valige **Lisada salvestamise** ikooni, et teie brauseris salvestamise seansi käivitamine.

    ![Brauseris ikooni lisamine salvestamise kuvatõmmis][3]

    ![Kuvatõmmis nupu Salvesta brauseris][4]

3. Liikuge sirvides teenuse struktuuri rakendus. Lindistuse paani peaks kuva web nõuab.

    ![Web nõuab paanil salvestamise kuvatõmmis][5]

4. Tehke toimingud, mida eeldate kasutajad teha jada. Neid toiminguid kasutatakse mustri loomiseks laadi.

5. Kui olete lõpetanud, klõpsake nuppu **Peata** salvestamise lõpetamiseks.

    ![Nupu Lõpeta kuvatõmmis][6]

    Visual Studio .webtest projekt on jäädvustata taotlusi sarjaga. Dünaamilised parameetrid asendatakse automaatselt. Selles etapis saate kustutada eest, korduvate sõltuvus taotlused, mis ei kuulu teie testi stsenaariumi.

6. Projekti salvestada ja seejärel valige **Testimiseks käivitage** käsk Käivita web katse kohalikult ja veenduge, et kõik töötab õigesti.

    ![Kuvatõmmis käsust käivitage Test][7]

### <a name="parameterize-the-web-performance-test"></a>Parameterize web katse

Web katse saate parameterize teisendamine kodeeritud web jõudluse test ja seejärel redigeerige kood. Teise võimalusena võite siduda web katse andmete loend, et test seni, kuni andmed. Vt täpsemat teavet web katse teisendamiseks kodeeritud katse [Genereeri ja Käivita kodeeritud web jõudluse test](https://msdn.microsoft.com/library/ms182552.aspx) . Leiate teavet selle kohta, kuidas andmeid sidumiseks web jõudluse test [Lisa andmeallika web jõudlust testida](https://msdn.microsoft.com/library/ms243142.aspx) .

Selle näite puhul teisendame web katse kodeeritud katsetamiseks nii, et saate asendada lennukikujuline ID loodud GUID ja lisada rohkem taotlusi saata lendude asukohtadesse.

### <a name="create-a-load-test-project"></a>Laadi testi projekti loomine

Laadi testi projekti koosneb web katse ja ühiku test lisakoormust määratud testi sätted koos üks või mitu võimalust. Järgmised toimingud Kuva laadimise testi projekti loomise kohta:

1. Kiirmenüü Web jõudlus ja laadi Test projekti, valige **Lisa** > **Laadi Test**. **Laadi testida** viisardis nuppu **järgmise** testi sätteid konfigureerida.

2. **Laadi mustri** jaotises Valige, kas soovite konstandi kasutaja laadi või samm laadi, mis algab mõni kasutaja ja kasutajate suureneb aja jooksul.

    Kui teil on hea hinnanguline summa kasutaja laadimine ja soovite näha, kuidas olemasolevat sooritab, valige **Konstandi laadimine**. Kui teie eesmärgiks on siit saate teada, kas süsteem täidab pidevalt erinevate laadimise, valige **Samm laadi**.

3. **Testige Mix** jaotises Valige nupp **Lisa** ja valige Laadi test kustutatava test. **Jaotuse** veeru abil saate määrata iga katse kokku testide protsenti.

4. Määrake jaotises **Sätted käivitada** laadi katse kestus.

    >[AZURE.NOTE] **Testige iteratsiooni** suvand on saadaval, ainult siis, kui käivitate koormuse testi kohalikult Visual Studio abil.

5. **Käivitage sätted**jaotises **asukoht** , määrake asukoht, kus luuakse laadi test nõuab. Viisard võib teilt meeskonnatöö teenuste kontole sisse logida. Logige sisse ja seejärel valige geograafiline asukoht. Kui olete lõpetanud, klõpsake nuppu **valmis** .

6. Pärast testi laadimine on loodud, avage projekt .loadtest ja valige praegune säte, nt **Käivitada**käivitamine > **Settings1 käivitada [aktiivne]**. Selle tegemisel avaneb Käivita sätete aken **Atribuudid** .

7. **Käivitage sätted** aken Atribuudid jaotises **tulemite** **Ajastuse üksikasjad salvestusruumi** säte peaks olema **pole** vaikeväärtus. Sisestage väärtuseks **Kõik üksikasjalikud andmed** saada lisateavet laadi testi tulemused. Visual Studio meeskonnatöö teenustega ühendust ja käivitage testi laadimine kohta lisateabe saamiseks vaadake [Laadimine testimine](https://www.visualstudio.com/load-testing.aspx) .

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Käivitada koormus katse Visual Studio Team Services abil

**Laadi testimiseks käivitage** alustamiseks käivitage test käsu valimine.

![Kuvatõmmis käsk Käivita testi laadimine][8]

## <a name="view-and-analyze-the-load-test-results"></a>Laadi testi tulemuste vaatamine ja analüüsimine

Laadi nimega testi edenedes, graafikule jõudluse teave. Peaksite nägema midagi sarnast järgmine joonis.

![Laadi kontrolltöö tulemuste jõudluse graafik kuvatõmmis][9]

1. Valige lehe ülaosas linki **aruande alla laadida** . Pärast aruanne on alla laaditud, klõpsake nuppu **Kuva aruanne** .

    Vahekaardil **Graafik** saate vaadata graafikud erinevate jõudluse hinnale. Üldine testi tulemused kuvatakse vahekaardil **Kokkuvõte** . Vahekaardil **tabelid** kuvatakse edastatud ja nurjunud laadi katsete arv.

2. Valige **testi**arv lingid > **nurjunud** ja **vigade** > tõrke üksikasjade kuvamiseks veergude**arv** .

    Vahekaart **üksikasjad** kuvatakse virtuaalse kasutaja ja testi stsenaarium teave nurjunud taotlusi. Andmed on kasulik, kui koormus katse sisaldab mitut stsenaariumid.

[Analüüsimine laadimine testida tulemuste](https://www.visualstudio.com/load-testing.aspx) vaatamiseks laadimine testida Analyzer vaates graafikud laadi kontrolltöö tulemuste vaatamise kohta lisateabe saamiseks.

## <a name="automate-your-load-test"></a>Automatiseerida oma testi laadimine

Visual Studio meeskonnatöö teenuste laadimine testi pakub API-de aitavad hallata laadi testide ja analüüsida meeskonnatöö teenusekonto tulemusi. Lisateabe saamiseks vaadake [Cloud laadi testimine Rest API -d](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) .

## <a name="next-steps"></a>Järgmised sammud
- [Jälgimine ja diagnoosimise kohalikus arvutis arengu häälestamise teenused](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
