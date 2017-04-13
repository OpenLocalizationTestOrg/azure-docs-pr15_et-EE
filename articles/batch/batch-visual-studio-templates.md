<properties
    pageTitle="Visual Studio malle Azure'i paketi | Microsoft Azure'i"
    description="Siit saate teada, kuidas need Visual Studio projekti mallid aitavad teil rakendada ja käivitada oma Arvuta mahukat töökoormus Azure'i paketi"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio projektimallide Azure'i paketi

**Töö juhataja** ja paketi **tööülesande protsessor Visual Studio Mallid** pakuvad koodi abil saate rakendada ja käivitada oma Arvuta mahukat töökoormus paketi peegeldav vähemalt summa. Selles dokumendis kirjeldatakse neid malle ja annab juhised, kuidas neid kasutada.

>[AZURE.IMPORTANT] Selles artiklis käsitletakse ainult need kaks malle kehtivad teabe ja eeldab, et olete tuttav paketi teenuse ja sellega seotud võtme põhimõtet: kaustu, arvutada sõlmed, töö ja ülesanded, halduri tööülesandeid, keskkonna muutujate ja muu asjakohane teave. Lisateavet leiate [Azure'i paketi põhitõed](batch-technical-overview.md), [paketi funktsioon ülevaade arendajatele](batch-api-basics.md)ja [.net-i Azure'i paketi teegi kasutamise alustamine](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Üksikasjalik ülevaade

Töö juhataja ja tööülesande protsessor malle saab luua kasulikke kahest osast:

* Töö halduri tööülesande, mis rakendab töö tükeldi, mis võivad murda töö mitme tööülesandeid, mida saab käitada sõltumatult, samal ajal sisse.

* Tööülesande protsessor, mida saab teha eeltöötlus ja töötlemise ümber mõne käsurea rakenduse.

Näiteks filmi renderdamise stsenaariumis töö tükeldusriba muutuks ühe filmi töö sadu või tuhandeliste töötleb üksikud raamid eraldi eraldi toiminguid. Vastavalt tööülesande protsessor oleks renderdamise Käivita_rakendus ja kõik sõltuvad protsessid, mida on vaja muuta iga raami, samuti mis tahes täiendavaid toiminguid (nt sulatatud paneeli kopeerimine Salvestuskoht).

>[AZURE.NOTE] Töö juhataja ja tööülesande protsessor Mallid on sõltumatult, seega saate kasutada mõlemat või ainult üks neist, sõltuvalt teie Arvuta töö ja seejärel klõpsake oma eelistusi nõuetele.

Nagu on näidatud alloleval joonisel, Arvuta tööd, mis kasutab neid malle läbida kolmest etapist:

1. Kliendi kood (nt rakenduse, veebiteenuse, jne) edastab töö paketi teenusega Azure, määrata vastavalt oma töö halduri tööülesannete töö manager programm.

2. Paketi teenus töötab halduri tööülesanne Arvuta sõlme ja töö tükeldusriba käivitab nagu paljud arvutada sõlmed vastavalt vajadusele, töö tükeldusriba kood tehnilised andmed ja parameetrid protsessor tööülesanded tööülesande määratud arvu kohta.

3. Tööülesande protsessor tööülesanded käivitage sõltumatult, samal ajal töödelda sisendandmete ja luua väljundi andmed.

![Skeem, mis näitab, kuidas kliendi kood suhtleb paketi teenus][diagram01]

## <a name="prerequisites"></a>Eeltingimused

Paketi malli kasutamiseks on vaja järgmist:

* Arvuti abil Visual Studio 2015 või uuemat versiooni, juba installitud.

* Paketi Mallid, mis on saadaval [Visual Studio galerii] [ vs_gallery] nimega Visual Studio laiendid. On kaks võimalust saada malle.

  * Malle Visual Studio dialoogiboksi **laiendid ja värskenduste** installimine (Lisateavet leiate teemast [leidmine ja kasutamine Visual Studio laiendid][vs_find_use_ext]). Dialoogiboksis **laiendid ja värskenduste** otsimine ja allalaadimine järgmised kaks laiendid.

    * Azure'i paketi töö Manager töö tükeldusriba
    * Azure'i paketi tööülesande protsessor

  * Visual Studio online galeriist soovitud mallide allalaadimine: [Microsoft Azure paketi projekti Mallid][vs_gallery_templates]

* Kui kavatsete kasutada funktsiooni [Rakenduse paketid](batch-application-packages.md) juurutamise töö ülemus ja tööülesande protsessor paketti arvutada sõlmed, peate konto paketi salvestusruumi konto linkimine.

## <a name="preparation"></a>Ettevalmistamine

Soovitame luua lahenduse, mis võib sisaldada teie haldur töö ning tööülesande protsessor, kuna see võib oleks lihtsam jagada oma töö ja tööülesande protsessor programmide vahel kood. See lahendus loomiseks tehke järgmist.

1. Avage Visual Studio 2015 ja valige **fail** > **Uus** > **projekti**.

2. Jaotises **Mallid**laiendamine **Muud projekti tüübid**, klõpsake **Visual Studio lahenduste**ja valige **Tühi lahendus**.

3. Tippige nimi, mis kirjeldab rakenduse ja otstarbe see lahendus (nt "LitwareBatchTaskPrograms").

4. Uue lahenduse loomiseks klõpsake nuppu **OK**.

## <a name="job-manager-template"></a>Töö halduri malli

Töö halduri malli abil saate rakendada halduri tööülesandega, mida saate teha järgmisi toiminguid:

* Tükeldatud töö mitme tööülesanded.
* Esitada nende käitamist paketi tööülesanded.

>[AZURE.NOTE] Halduri tööülesannete kohta leiate lisateavet teemast [paketi funktsioon ülevaade arendajatele](batch-api-basics.md#job-manager-task).

### <a name="create-a-job-manager-using-the-template"></a>Looge töö ülemuse malli abil

Lahenduse, mis on varem loodud töö halduri lisamiseks tehke järgmist.

1. Avage oma olemasoleva lahenduse Visual Studio 2015.

2. Lahenduste Explorer, paremklõpsake lahendus, klõpsake nuppu **Lisa** > **Uue projekti**.

3. Jaotises **Visual C#**, klõpsake **pilve**ja klõpsake **Azure'i paketi töö Manager töö tükeldusriba**.

4. Tippige nimi, mis kirjeldab rakenduse ja selle projekti määratletakse töö manager (nt "LitwareJobManager").

5. Projekti loomiseks klõpsake nuppu **OK**.

6. Lõpuks koostada projekti Visual Studio kõigi viidatud Nugeti pakettide laadimiseks ja veenduge, et projekt on lubatud enne alustamist, muutes jõustamine.

### <a name="job-manager-template-files-and-their-purpose"></a>Töö halduri mallifailid ja nende eesmärk

Töö halduri malli abil projekti loomisel see loob koodi failide kolme rühma:

* Peamised programmifaili (Program.cs). See sisaldab programmi sisendpunkti ja ülataseme erandi töötlemine. Peate ei tohiks tavaliselt seda muuta.

* Kataloogi raames. See sisaldab faile vastutab 'trafarett' töö töö manager programm – lahtipakkimine parameetrid, tööülesannete lisamine pakett jne. Peate ei tohiks tavalisel viisil muuta need failid.

* Töö tükeldusriba faili (JobSplitter.cs). See on, kui lisate oma rakenduse kohased loogika tükeldamine projekti tööülesanded.

Muidugi saate lisada täiendavad failid vastavalt vajadusele tugiteenuste töö tükeldusriba kood, tükeldamise loogika töö keerukusest.

Mall loob ka standardse .NET projekti faile nagu .csproj faili, app.config, packages.config jne.

Ülejäänud selles jaotises kirjeldatakse erinevaid faile ja nende koodi struktuuri ja selgitab, mida iga tunni.

![Visual Studio Solution Exploreris nähtaval töö halduri malli lahendus][solution_explorer01]

**Raamistiku failid**

* `Configuration.cs`: Kapseloi laadimise konfiguratsioon töö andmeid, nt paketi konto üksikasjad, lingitud salvestusruumi konto identimisteave, töö ja ülesannete teave ning töö parameetrid. See sisaldab ka paketi määratletud keskkonna muutujate (vt keskkonna sätteid tööülesannete paketi dokumentatsiooni) Configuration.EnvironmentVariable klassi kaudu juurdepääsu.

* `IConfiguration.cs`: Abstracts rakendamise konfiguratsiooni ainekursust, nii et saate oma töö tükeldi abil petulingid või fiktiivne konfiguratsiooni objekti ühiku test.

* `JobManager.cs`: Orchestrates komponentide töö manager programm. See vastutab lähtestamisel töö tükeldusriba, töö tükeldusriba kasutada, ja tagastab töö tükeldusriba tööülesande esitaja tööülesanded saatmise.

* `JobManagerException.cs`: Tähistab viga, mis nõuab töö halduri lõpetada. JobManagerException kasutatakse mähkimiseks 'oodatud' tõrgete, kus teatud diagnostika teave lõpetamise osana.

* `TaskSubmitter.cs`: Tagastatud töö tükeldusriba pakett-töö ülesannete lisamine vastutab see tund. JobManager klassi agregaadid tööülesannete järjestust sisse pakettidena tõhusa, kuid õigeaegne liitmiseks töö, siis helistab TaskSubmitter.SubmitTasks tausta teemat iga.

**Töö tükeldusriba**

`JobSplitter.cs`: See tund sisaldab rakenduse kohased loogika tükeldamine projekti tööülesanded. Raames käivitab JobSplitter.Split meetodit saada ülesandeid, mis lisab see töö nagu meetodit tagastab nende jada. See on ainekursust, kuhu te sisestab oma töö loogika. Rakendada tükeldatud meetodit CloudTask eksemplari tähistav ülesanded, kuhu soovite sektsiooni töö jada tagastamiseks.

**Standardse .NET käsurea projekti faile**

* `App.config`: Standard .net-i rakenduse konfigureerimise faili.

* `Packages.config`: Standard Nugeti sõltuvus pakettfaili.

* `Program.cs`: Sisaldab programmi sisendpunkti ja ülataseme erandi töötlemine.

### <a name="implementing-the-job-splitter"></a>Töö tükeldusriba rakendamine

Töö halduri malli projekti avamisel projekt on JobSplitter.cs fail avatakse vaikimisi. Allpool Split() meetod Kuva abil saate juurutada tükeldatud loogika toimingute oma töökoormus:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] Selgitustega jaotisele selle `Split()` meetod on ainult osa töö Manager malli kood, mis on mõeldud muuta, lisades loogika jagada oma töö erinevad toimingud. Kui soovite muuta mõne muu jaotise malli, siis veenduge, saate on familiarized koos paketi tööpõhimõte ja proovida vaid mõne [paketi koodinäiteid][github_samples].

Teie Split() rakendamine on juurdepääs.

* Töö parameetrid, kaudu soovitud `_parameters` välja.
* CloudJob objekti kaudu tööd, mis on `_job` välja.
* CloudTask objekti tähistav tööülesanne halduri kaudu soovitud `_jobManagerTask` välja.

Teie `Split()` rakendamist ei pea tööülesannete lisamine projekti otse. Selle asemel oma kood tuleks tagastada CloudTask objektide jada ja need lisatakse töö automaatselt autonoomsest töö tükeldusriba framework kaupa. See on ühine kasutamine C# 's iteraatori (`yield return`) rakendamiseks töö lõhkujad see võimaldab alustamiseks töötab nii kiiresti kui võimalik, mitte oodata kõigi toimingute tegemiseks tuleb arvutada tööülesannete funktsiooni.

**Töö tükeldusriba tõrge**

Kui teie töö tükeldusriba tekib tõrge, tuleks kas:

* Lõpetada järjestust, kasutades C# `yield break` lause, milles käsitletakse juhul töö halduri nii eduka; või

* Põhjustada erandi, millisel juhul töö halduri käsitletakse nurjus ning võib sõltuvalt sellest, kuidas kliendi on konfigureerinud selle proovitakse).

Mõlemal juhul on juba töö tükeldusriba tagastatud ja lisatakse pakett-töö ülesannete täitmiseks õigus käivitada. Kui te ei soovi, et see juhtub, siis võib:

* Töö ennetähtaegselt naasnud töö tükeldusriba

* Tööülesanne terve saidikogumi enne tagastades koostada (, tagastavad mõne `ICollection<CloudTask>` või `IList<CloudTask>` asemel rakendamine oma töö tükeldi abil iteraatori C#)

* Kõik toimingud sõltuvad edukaks halduri töö tegemiseks kasutada ülesandesõltuvused

**Töö halduri korduskatsed**

Töö halduri nurjumisel võivad seda uuesti paketi teenuse olenevalt kliendi proovi uuesti sätted. Üldiselt, see on ohutu, kuna kui raames lisab tööülesannete töö, ignoreerib funktsioon neid kõik toimingud, mis on juba olemas. Juhul, kui arvutamise tööülesanded on kallis, soovi korral võite ei tekivad kulud ümberarvutamine toimingud, mis on juba lisatud töö; vastupidi, kui uuesti käivitada on tagatud sama ülesande ID-sid luua seejärel 'Ignoreeri duplikaatide' käitumine ei algavad. Sellisel juhul peaksite kujundama oma töö tükeldusriba töö, mis on juba tehtud, ja korrake, näiteks enne yield tööülesannete lisamine CloudJob.ListTasks täites tuvastamiseks.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Väljuge koodid ja erandid töö halduri malli

Välju koodid ja erandid süsteem selgitamiseks programmi, ning need aitavad probleemid programmi täitmist. Töö halduri malli rakendab välju koodid ja selles jaotises kirjeldatud erandid.

Halduri tööülesandega, mis on rakendatud töö haldur malliga saate tagastavad kolm võimalikku välju koodi.

| Kood | Kirjeldus |
|------|-------------|
| 0    | Töö haldur lõpule viidud. Oma töö tükeldusriba kood käivitasite valmimiseni ja kõik tööülesanded on lisatud töö. |
| 1    | Tööülesanne halduri nurjus erandi 'oodatud' osa programmi. Erandiks on tõlgitud on JobManagerException koos diagnostikateave ja võimaluse korral selle tõrke lahendamise soovitusi. |
| 2    | Tööülesanne halduri nurjus "ootamatut" erandi. Erandiks on logitud väljundit, kuid töö haldur ei saa lisada mis tahes diagnostika või parandamise lisateavet. |

Töö halduri ülesanne ei ole puhul mõningaid ülesandeid endiselt on lisatud teenusega enne viga. Järgmised toimingud käivituvad nagu tavaliselt. Vt "Töö tükeldusriba tõrge" seda koodi tee arutamiseks.

Kogu teabe tagastatud erandid on kirjutatud stdout.txt ja stderr.txt failid. Lisateavet leiate teemast [Tõrge töötlemise](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Kliendi kaalutlused

Selles jaotises kirjeldatakse mõned kliendi täita rakendatakse töö ülemuse selle malli põhjal. Vaadake, [Kuidas parameetrite ja keskkonna muutujate kliendi koodi](#pass-environment-settings) täpsemat parameetrite ja keskkonna sätteid.

**Kohustusliku identimisteave**

Ülesannete lisamiseks Azure'i pakett halduri tööülesanne nõuab teie Azure'i paketi konto URL-i ja võti. Peate läbima need keskkonna muutujad nimega YOUR_BATCH_URL ja YOUR_BATCH_KEY. Saate määrata need töö halduris tööülesande keskkonna sätteid. Näiteks klõpsake C# kliendi:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Salvestusruumi identimisteave**

Tavaliselt klient ei pea pakkuda lingitud salvestusruumi konto identimisteave töö halduri ülesanne, sest a kõige töö haldurid pole vaja konkreetselt kontole juurde pääseda lingitud salvestusruumi ja kõik toimingud (b) lingitud salvestusruumi konto antakse sageli levinud keskkonna sätteks töö. Kui esitate lingitud salvestusruumi konto kaudu levinud keskkonna sätteid ja töö ülemus nõuab juurdepääsu lingitud salvestusruumi, siis saate peaksid esitama lingitud salvestusruumi identimisteabe järgmiselt:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Töö halduri tööülesande sätted**

Kliendi peaks Määrake töö halduri *killJobOnCompletion* lipp väärtuseks **väär**.

See on tavaliselt ohutu kliendi *runExclusive* väärtuseks **false**.

Kliendi tuleks kasutada *resourceFiles* või *applicationPackageReferences* saidikogumi töö Arvuta sõlme juurutatud halduri käivitatava (ja selle nõutav dll).

Vaikimisi pole proovitakse töö ülemus, kui see ei õnnestu. Töö teie haldur loogika olenevalt kliendi soovite lubada korduskatsed kaudu *piiranguid*/*maxTaskRetryCount*.

**Töö sätted**

Kui töö tükeldusriba eraldab tööülesannete sõltuvusi, kliendi väärtuseks true projekti usesTaskDependencies.

Mudelis töö tükeldusriba on ebatavalised klientidele soovida lisada ülesandeid töö ületab, mis loob tükeldi töö. Kliendi peaks seetõttu tavaliselt seatud projekti *onAllTasksComplete* **terminatejob**.

## <a name="task-processor-template"></a>Tööülesande protsessor Mall

Tööülesande protsessor malli abil saate rakendada tööülesande protsessor, mida saate teha järgmisi toiminguid:

* Häälestage iga tööülesande paketi käivitamiseks nõutav teave.
* Kõik toimingud, mis on nõutav iga tööülesande paketi käivitamine
* Tööülesande väljundeid salvestamiseks püsivate salvestusruumi.

Kuigi tööülesande protsessor ei pea käivitada töölehe ülesanded, võtme ära tööülesande protsessorit on see pakub ümbrisesse rakendada kõigi ülesandetoimingud täitmise ühes kohas. Näiteks kui peate käivitama iga tööülesande kontekstis mitu rakendust või kui teil on vaja Kopeerige andmed pärast lõpetamist püsiv hoidla iga tööülesande.

Tööülesande protsessor soovitud toimingud võib olla lihtne või keerukas, ja nii palju või vähe, nagu on nõutud oma töökoormus. Lisaks kõik ülesandetoimingud sisse ühe tööülesande protsessor rakendades saate hõlpsasti värskendada ja toimingute alusel rakenduste või töökoormus nõuded muudatused lisada. Siiski mõnel juhul tööülesande protsessor ei pruugi teie rakendamiseks optimaalne lahendus nagu seda saab lisada tarbetu keerukus, näiteks siis, kui töötab tööd, mis saab kiiresti käivitada lihtsa käsureal.

### <a name="create-a-task-processor-using-the-template"></a>Luua tööülesande protsessor malli abil

Lahendus varem loodud tööülesanne protsessor lisamiseks tehke järgmist.

1. Avage oma olemasoleva lahenduse Visual Studio 2015.

2. Solution Exploreris paremklõpsake lahendus, klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **Uus projekt**.

3. Jaotises **Visual C#**, klõpsake **pilve**ja klõpsake **Azure'i paketi tööülesande protsessor**.

4. Tippige nimi, mis kirjeldab rakenduse ja selle projekti määratletakse tööülesande protsessor (nt "LitwareTaskProcessor").

5. Projekti loomiseks klõpsake nuppu **OK**.

6. Lõpuks koostada projekti Visual Studio kõigi viidatud Nugeti pakettide laadimiseks ja kinnitamaks, et projekt on lubatud enne alustamist, muutes jõustamine.

### <a name="task-processor-template-files-and-their-purpose"></a>Tööülesande protsessor mallifailid ja nende eesmärk

Projekti tööülesannete protsessor malli abil loomisel see loob koodi failide kolme rühma:

* Peamised programmifaili (Program.cs). See sisaldab programmi sisendpunkti ja ülataseme erandi töötlemine. Peate ei tohiks tavaliselt seda muuta.

* Kataloogi raames. See sisaldab faile vastutab 'trafarett' töö töö manager programm – lahtipakkimine parameetrid, tööülesannete lisamine pakett jne. Peate ei tohiks tavalisel viisil muuta need failid.

* Tööülesande protsessor faili (TaskProcessor.cs). See on, kui lisate oma rakenduse kohased loogika tööülesande teostamiseks (tavaliselt helistades olemasoleva täitmisfaili). Enne ja pärast töötlemist kood, nt täiendavate andmete allalaadimine failide või üleslaadimine tulemi ka siia.

Muidugi saate lisada täiendavad failid vastavalt vajadusele tugiteenuste töö protsessor kood, tükeldamise loogika töö keerukusest.

Mall loob ka standardse .NET projekti faile nagu .csproj faili, app.config, packages.config jne.

Ülejäänud selles jaotises kirjeldatakse erinevaid faile ja nende koodi struktuuri ja selgitab, mida iga tunni.

![Visual Studio Solution Exploreris nähtaval tööülesande protsessor malli lahendus][solution_explorer02]

**Raamistiku failid**

* `Configuration.cs`: Kapseloi laadimise konfiguratsioon töö andmeid, nt paketi konto üksikasjad, lingitud salvestusruumi konto identimisteave, töö ja ülesannete teave ning töö parameetrid. See sisaldab ka paketi määratletud keskkonna muutujate (vt keskkonna sätteid tööülesannete paketi dokumentatsiooni) Configuration.EnvironmentVariable klassi kaudu juurdepääsu.

* `IConfiguration.cs`: Abstracts rakendamise konfiguratsiooni ainekursust, nii et saate oma töö tükeldi abil petulingid või fiktiivne konfiguratsiooni objekti ühiku test.

* `TaskProcessorException.cs`: Tähistab viga, mis nõuab töö halduri lõpetada. TaskProcessorException kasutatakse mähkimiseks 'oodatud' tõrgete, kus teatud diagnostika teave lõpetamise osana.

**Tööülesande protsessor**

* `TaskProcessor.cs`: Töötab ülesanne. Raames käivitab TaskProcessor.Run meetod. See on ainekursuse, kuhu te sisestab rakendusele vastav loogika tööülesande. Rakendada Käivita meetodi abil:
  * Sõeluda ja kinnitage tööülesande parameetrid
  * Mis tahes välise programmi, mida soovite kasutada käsurea koostamine
  * Logige mis tahes diagnostikateave nõuate silumine eesmärgil
  * Käivitage see käsurea kaudu protsess
  * Oodake väljumiseks protsess
  * Jäädvustada välju kood protsessi kindlaks teha, kui see õnnestus või mitte.
  * Salvestage failid väljundi soovite alles jätta, püsivate salvestusruum

**Standardse .NET käsurea projekti faile**

* `App.config`: Standard .net-i rakenduse konfigureerimise faili.
* `Packages.config`: Standard Nugeti sõltuvus pakettfaili.
* `Program.cs`: Sisaldab programmi sisendpunkti ja ülataseme erandi töötlemine.

## <a name="implementing-the-task-processor"></a>Tööülesande protsessor rakendamine

Projekti tööülesannete protsessor malli avamisel projekt on TaskProcessor.cs fail avatakse vaikimisi. Allpool Run() meetodi abil saate juurutada Käivita loogika tööülesanded teie töökoormus:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] Selgitustega jaotisele Run() meetod on tööülesande protsessor malli kood, mis on mõeldud saate muuta oma töökoormus tööülesanded Käivita loogika lisamine ainult osa. Kui soovite muuta mõne muu jaotise malli, lugege esmalt tutvuda paketi dokumentide läbivaatamine ja proovida paketi koodinäiteid mõne paketi tööpõhimõte.

Run() meetodit vastutab käsurea, alustades ühe või mitme protsessi, ootamine kogu protsessi lõpuleviimiseks, käivitades tulemuste salvestamine ja lõpuks esitus koos tähisega välju. Run() meetod on, kus saate rakendada töötlemise loogika tööülesannete. Tööülesande protsessor raames käivitab Run() meetod teile; teil pole vaja helistada ise.

Teie Run() rakendamine on juurdepääs.

* Tööülesande parameetrid, kaudu soovitud `_parameters` välja.
* Töö ja ülesande ID-d, kaudu soovitud `_jobId` ning `_taskId` väljad.
* Tööülesande konfigureerimise kaudu soovitud `_configuration` välja.

**Tööülesande tõrge**

Ajalootabelis, Run() meetodit väljumiseks saate erandi ilmnemist, kuid see jätab ülemise taseme erandi sündmuseohjuri juhtimise tööülesande välju kood. Kui teil on vaja määrata välju kood nii, et saate eristada erinevat tüüpi tõrge, näiteks ajutised või kuna mõned tõrgete tüübid tuleks töö lõpetada ja teiste ei tohiks, tuleks Run() meetodit null välju koodi tagastades väljumine. See muutub tööülesande välju kood.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Väljuge koodid ja erandid malli tööülesande protsessor

Välju koodid ja erandid süsteem selgitamiseks programmi, ning need aitavad probleemid programmi täitmist. Tööülesande protsessor malli rakendatakse välju koodid ja selles jaotises kirjeldatud erandid.

Tööülesande protsessor tööülesande, mida rakendatakse tööülesande protsessor malli abil saate tagastada kolm võimalikku välju koodi:

| Kood | Kirjeldus |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | Tööülesande protsessor parandusfunktsiooni valmimiseni. Pange tähele, et see ei tähenda, et saate kasutada programmi õnnestus – ainult, et tööülesanne protsessor kasutada seda edukalt ja tehtud pärast töötlemine ilma erandid. Välju koodi tähendust sõltub käivitatud programm – tavaliselt välju koodi 0 – programmi õnnestus ja välju kood tähendab, et programm nurjus. |
| 1    | Tööülesande protsessor nurjus erandi 'oodatud' osa programmi. Erandiks on tõlgitud on `TaskProcessorException` koos diagnostikateave ja võimaluse korral selle tõrke lahendamise soovitusi. |
| 2    | Tööülesande protsessor nurjus "ootamatut" erandi. Erandiks on logitud väljundit, kuid tööülesande protsessor ei saa lisada mis tahes diagnostika või parandamise lisateavet. |

>[AZURE.NOTE] Kui kutsute programm kasutab teatud tõrgete laad välju koodide 1 ja 2, siis koode välju 1 ja 2 tööülesande protsessor tõrkeid on ebaselgete. Saate muuta nende tööülesande protsessor tõrkekoodid omanäolise välju koodide, redigeerides erand juhtudel Program.cs faili.

Kogu teabe tagastatud erandid on kirjutatud stdout.txt ja stderr.txt failid. Lisateabe saamiseks vt töötlemise tõrge, paketi dokumentatsiooni kohta.

### <a name="client-considerations"></a>Kliendi kaalutlused

**Salvestusruumi identimisteave**

Kui tööülesande protsessor kasutab Azure'i bloobimälu püsima väljundeid, kasutades näiteks faili põhimõtted helper teeki, siis on vaja juurdepääsu *kas* pilve salvestusruumi konto identimisteave *või* bloobimälu container URL, mis sisaldab ühiskasutusse antud juurdepääs signatuuri (SAS). Mall sisaldab tugi mandaatide levinud keskkonna muutujate kaudu. Teie klient ei liigu salvestusruumi identimisteabe järgmiselt:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Salvestusruumi konto muutub saadavaks TaskProcessor klassi kaudu soovitud `_configuration.StorageAccount` atribuut.

Kui eelistate kasutada SASga container URL-i, võite seda anda ka töö levinud keskkonna sätte kaudu, kuid tööülesande protsessor malli ei sisalda praegu sisseehitatud tugi.

**Salvestusruumi häälestamine**

Soovitatav on, et kliendi või töö halduri tööülesande loomine mis tahes ümbriste nõutud enne tööülesannete lisamine projekti tööülesanded. See on kohustuslikud container URL-i kasutamisel SASga sellisena ei sisalda URL-i ümbris loomise õigus. Isegi juhul, kui olete oma ostuõiguse salvestusruumi konto identimisteave, on soovitatav seda salvestab iga tööülesande, millel helistada CloudBlobContainer.CreateIfNotExistsAsync ümbris.

## <a name="pass-parameters-and-environment-variables"></a>Parameetrite ja keskkonna muutujad

### <a name="pass-environment-settings"></a>Liigu keskkonna sätteid

Kliendi saab edastada teabe halduri tööülesanne kujul keskkonna sätteid. Seda teavet saab kasutada siis halduri tööülesanne tööülesande protsessor tööülesanded tööülesannete Arvuta töötavad loomisel. Teavet, mida te ei liigu keskkonna sätteid nagu näited:

* Salvestusruumi konto nimi ja võtmed
* Paketi konto URL
* Paketi konto võti

Paketi teenus on lihtne süsteem edastamiseks töö halduri tööülesande keskkonna sätteid, kasutades funktsiooni `EnvironmentSettings` atribuudi [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Näiteks, et saada soovitud `BatchClient` eksemplari paketi konto ei liigu nagu keskkonna muutujate kliendi kood URL-i ja ühiskasutuses võtme identimisteabe paketi konto. Samuti kontole juurde pääseda salvestusruumi paketi kontoga on seotud, saab edastada salvestusruumikonto nimi ja salvestusruumi konto võti nimega keskkonna muutujate.

### <a name="pass-parameters-to-the-job-manager-template"></a>Parameetrite edastamine töö halduri Mall

Paljudel juhtudel on kasulik edasi töö kohta parameetrid töö halduri tööülesande, protsessi tükeldamise töö juhtimiseks või projekti tööülesanded konfigureerimiseks. Selleks parameters.json nimeks ressursifaili töö halduri tööülesande JSON faili üleslaadimisel. Seejärel saate parameetrid muutuvad kättesaadavaks selle `JobSplitter._parameters` välja töö halduri malli.

>[AZURE.NOTE] Sisseehitatud parameetri sündmuseohjuri toetab ainult stringi string sõnastikud. Kui soovite läbida keerukate JSON väärtused nagu parameetrite väärtused, peate liigu need stringidena ja sõeluda neid töö tükeldusriba või muuta selle raamistiku `Configuration.GetJobParameters` meetod.

### <a name="pass-parameters-to-the-task-processor-template"></a>Parameetrite edastamine malli tööülesande protsessor

Üksikute tööülesannete rakendatud tööülesande protsessor malli abil saate ka parameetrite edastamine. Samamoodi nagu malliga töö halduri tööülesande protsessor malli otsitakse nimega ressursifaili

parameters.JSON ja kui leitakse see laadib selle nimega parameetrite sõnastik. On paar võimalust, kuidas edasi parameetrid protsessor tööülesanded tööülesande jaoks.

* Töö parameetrid JSON taaskasutada. See töötab ka siis, kui ainult parameetrid on töö kogu need (nt, Renderda kõrgus ja laius) jaoks. Selleks, töö tükeldusriba soovitud CloudTask loomisel lisada viite parameters.json ressursi faili objekti töö halduri tööülesande ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) on CloudTask ResourceFiles kogumi.

* Luua ja ülesande konkreetse parameters.json dokumendi üleslaadimiseks töö tükeldusriba täitmise osana ja selle ülesande ressursi failide kogumise bloobimälu viidata. See on vajalik, kui erinevad toimingud on erinevad parameetrid. Näiteks võib 3D renderdamise stsenaarium, kus paneeli registri edastatakse parameetrina tööülesanne.

>[AZURE.NOTE] Sisseehitatud parameetri sündmuseohjuri toetab ainult stringi string sõnastikud. Kui soovite läbida keerukate JSON väärtused nagu parameetrite väärtused, peate liigu need stringidena ja sõeluda neid tööülesande protsessor või muuta raamistiku `Configuration.GetTaskParameters` meetod.

## <a name="next-steps"></a>Järgmised sammud

### <a name="persist-job-and-task-output-to-azure-storage"></a>Töö ja tööülesannete väljund Azure Storage jää püsima

Teine kasulik tööriist paketi lahenduse arendatakse on [Azure'i paketi faili põhimõtted][nuget_package]. Kasutage seda .net-i klassi teeki (praegu jaotises Eelvaade) teie paketi .NET rakendustes saate hõlpsalt talletada ja tuua tööülesande väljundeid ja sealt Azure Storage. [Jää püsima Azure'i pakett ja tööülesande väljund](batch-task-output.md) sisaldab teegi ja selle kasutamine täieliku arutelu.

### <a name="batch-forum"></a>Paketi Foorum

[Azure'i paketi Foorum] [ forum] MSDN-is on hea koht arutada paketi ja teenuse kohta küsimusi. Pea kohta üle kasulikku "sticky" postitused, ning postitada oma küsimusi, kui need tekivad ajal saate luua oma paketi lahendusi.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
