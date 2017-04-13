<properties 
   pageTitle="Testimine pilveteenus jõudlus | Microsoft Azure'i"
   description="Testida pilveteenus profiler Visual Studio abil"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>Testimine pilveteenus jõudlus 

##<a name="overview"></a>Ülevaade

Te saate testida pilveteenus järgmisel viisil:

- Kasutage Azure'i diagnostika taotlusi ja ühenduste kohta teavet koguda ja üle vaadata saidi statistika, kus näidatakse, kuidas teenuse täidab kliendi seisukohast. Alustamine, leiate [Azure'i pilveteenused ja Virtuaalmasinates seadistamine diagnostika]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- Visual Studio profiler abil saate sügavuti analüüsida arvutuslik aspektide kuidas töötab teenus. See teema kirjeldab, saate selle profiler jõudluse mõõtmiseks nagu Azure'i töötab teenus. Jõudluse mõõtmiseks nagu Arvuta emulaator kohalikult töötab teenus on profiler kasutamise kohta leiate lisateavet teemast [testimine jõudlus on Azure pilveteenuse teenuse kohalikult arvutada emulaator kasutamisel Visual Studio Profiler](http://go.microsoft.com/fwlink/p/?LinkId=262845).



## <a name="choosing-a-performance-testing-method"></a>Tulemuslikkuse meetodit valimine

###<a name="use-azure-diagnostics-to-collect"></a>Azure'i diagnostika abil koguda.###

- Statistika veebilehtede või teenuseid, näiteks taotlusi ja ühendused.

- Statistika rollid, nt kui tihti rolli taaskäivitamist.

- Üldist teavet mälukasutus, nt kellaaeg, mis viib prahikogur või mälu protsent määrata töötava roll.

###<a name="use-the-visual-studio-profiler-to"></a>Visual Studio profiler kasutamine###

- Kindlaks teha, millised funktsioonid on kõige rohkem aega võtta.

- Mõõtke, kui palju aega võtab iga osa arvesatud intensiivne programm.

- Võrrelge üksikasjalik jõudlusearuannete kaks versiooni teenuse jaoks.

- Analüüsimine mälu eraldamine täpsemalt kui üksikute mälu jaotuse tase.

- Analüüsimine kokkulangevus probleeme multithreaded kood.

Funktsiooni profiler kasutamisel saate koguda andmeid pilveteenus käitamisel kohalikult või Azure.

###<a name="collect-profiling-data-locally-to"></a>Profiili andmed kohalik koguda:###

- Pilveteenuse, nt kindla töötaja rolli, mis ei nõua realistlik jäljendatud laadi täitmise osa testida.

- Testida pilveteenus eraldi kontrollitud.

- Testida pilveteenus, enne juurutamist Azure.

- Testida pilveteenus privaatselt, ilma häirida olemasoleva juurutuste.

- Testida teenuse ilma, et tekiks kulude Azure töötab.

###<a name="collect-profiling-data-in-azure-to"></a>Azure'i profiilide andmete kogumise:###

- Pilveteenus jäljendatud või otse koormuse testida.

- Kasutage instrumentation meetodit profiilide andmete kogumine, nagu selles teemas kirjeldatakse hiljem.

- Testida teenuse nimega kui teenus töötab valmistamisel samas keskkonnas.

Saate simuleerida tavaliselt testi pilveteenustega jaotises tavaline või pingetaluvust koormus.

## <a name="profiling-a-cloud-service-in-azure"></a>Profiili Azure pilveteenus

Kui avaldate oma pilveteenuses Visual Studio, saate teenuse profiil ja määrata profiilide sätteid, mis annab teile soovitud teavet. Profiilide seansi käivitatakse iga eksemplari rollid. Lisateavet selle kohta, kuidas avaldada oma teenuse Visual Studio leiate [Azure'i pilveteenusesse, Visual Studio avaldamise](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Lisateavet jõudluse profiilide Visual Studios, vt [Algajatele juhend jõudluse profiili](https://msdn.microsoft.com/library/azure/ms182372.aspx) ja [Analüüsimine rakenduse jõudlust profiilide tööriistade abil](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] Saate lubada IntelliTrace või profiilide kui avaldate oma pilveteenuses. Te ei saa kasutada mõlemat.

###<a name="profiler-collection-methods"></a>Profiler saidikogumi meetodid

Saate saidikogumi erinevad meetodid profiili, oma jõudlusega seotud probleemide põhjal:

- **CPU valimite** – seda meetodit kogub rakenduse statistika, mis on abiks esialgne analüüs CPU kasutamine probleemid. CPU valimite on soovitatud meetod, alustades kõige jõudluse juurdluste jaoks. Rakendus, mis on profiili, kui CPU valimite jõudlusandmeid koguda on vähese mõju.

- **Haldusteenuse** – seda meetodit kogub üksikasjalikku ajakava andmed, mis on kasulik aktiivse analüüsi ja sisend jõudlusega seotud probleemide analüüsimine. Haldusteenuse meetodit kirjete iga kirje, välju ja funktsioonide funktsioonikutse mooduli ajal profiilide käivitada. See meetod on kasulik kohta oma koodi osa ajastuse üksikasjaliku teabe kogumine ja väljaloendisse rakenduse tööle sisend- ja toimingud. See meetod on keelatud arvutis, kus töötab operatsioonisüsteem on 32-bitine versioon. See suvand on saadaval ainult siis, kui käivitate pilveteenusesse Azure, mitte kohalik Arvuta emulaator.

- **.Net-i mälu jaotamine** – seda meetodit kogub proovide profiilide meetodit kasutades .NET Frameworki mälu eraldatud andmeid. Kogutud andmete sisaldab arvu ja eraldatud objektide suurust.

- **Kokkulangevus** – seda meetodit kogub ressursi väide andmed ja protsess ja teemat täitmise andmeid, mis on kasulik analüüsimine mitmetoimeline ja mitme protsessi rakendused. Kokkulangevus meetodit kogub andmeid iga sündmuse, et teie koodi, näiteks kui teemat ootab plokid käivitamise lukustatud Accessi rakenduste ressursside vabastada. See meetod on kasulik mitmetoimeline rakenduste analüüsimiseks.

- Saate lubada **Taseme suhtlust profiili**, mis annab Lisateavet sünkroonse ADO.NET teostamise aja kutsub suhelda ühe või mitme andmebaaside mitmerajalisest rakenduste funktsioonid. Esimese taseme suhtluse andmete kogute profiilide meetodite abil. Esimese taseme suhtluse profiilide kohta lisateabe saamiseks vaadake [taseme kasutusviisid](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profiili sätete konfigureerimine

Järgmisel joonisel on Azure teenuserakenduse avaldamine dialoogiboksis profiili sätete konfigureerimine.

![Profiili sätete konfigureerimine](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] **Profiili lubamiseks** märkige ruut Luba, peab teil olema profiler, kasutate avaldada oma pilveteenuses kohalikku arvutisse installitud. Vaikimisi on profiler on installitud Visual Studio installimisel.

### <a name="to-configure-profiling-settings"></a>Kui soovite profiilide sätete konfigureerimine

1. Solution Exploreris Azure'i projekti kiirmenüü avamine ja klõpsake nuppu **Avalda**. Üksikasjalikud juhised selle kohta, kuidas avaldada pilveteenus, lugege teemat [avaldamise pilv teenuse Azure tööriistade abil](http://go.microsoft.com/fwlink/p?LinkId=623012).

1. **Azure'i teenuserakenduse avaldamine** dialoogiboksis valitud vahekaardil **Täpsemad sätted** .

1. Profiili lubamiseks märkige ruut **Luba profiili** .

1. Valige oma profiili sätete konfigureerimiseks **sätted** hüperlink. Kuvatakse dialoogiboks profiilide sätted.

1. Valige raadionupud **millist meetodit profiili soovite kasutada** tüüpi profiili, et teil on vaja.

1. Taseme suhtluse profiilide teabe kogumiseks, märkige ruut **Luba taseme suhtluse profiili** .

1. Sätete salvestamiseks klõpsake nuppu **OK** .

    Kui avaldate selle rakenduse, kasutatakse neid sätteid iga rolli profiilide seanssi luua.

## <a name="viewing-profiling-reports"></a>Profiili aruannete kuvamine

Profiilide seansi luuakse iga eksemplari rolli teie pilveteenuses. Iga seansi Visual Studio profiilide aruannete vaatamiseks saate vaadata serveri Exploreri aknas ja valige Azure Arvuta sõlme rolli eksemplari valimiseks. Saate vaadata profiilide aruande nagu on näidatud järgmisel joonisel.

![Azure profiilide aruande kuvamine](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profiilide aruandeid

1. Visual Studio serveri Exploreri aknas vaatamiseks menüü menüü Vaade käsku Server Explorer.

1. Valige sõlm Azure'i arvutada, ja seejärel valige sõlme Azure juurutamise jaoks valitud profiilile avaldamisel Visual Studio pilveteenuses.

1. Vaadata eksemplari profiilide aruandeid, valige roll teenus, eksemplariga kiirmenüü avamine ja seejärel valige **Kuva profiilide aruanne**.

    Azure'i alla laaditakse kohe aruande .vsp faili, ja allalaadimise olek kuvatakse Azure'i tegevuste Logi. Kui allalaadimine on lõpule jõudnud, profiilide aruanne kuvatakse vahekaardil redaktoris Visual Studio nimega <Role name> _<Instance Number>_ <identifier>.vsp. Kokkuvõtte aruande andmed kuvatakse.

1. Erinevate vaadete aruande kuvamiseks klõpsake loendis Praegune vaade soovitud tüübi valimine. Lisateavet [Profiilide tööriistad aruande vaated](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Järgmised sammud

[Pilveteenustega silumine](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Avaldamine on Azure pilveteenuses Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

