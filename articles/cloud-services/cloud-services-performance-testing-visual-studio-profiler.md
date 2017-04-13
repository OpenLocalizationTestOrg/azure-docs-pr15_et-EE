<properties 
    pageTitle="Profiili pilveteenus kohalikult rakenduses Arvuta emulaator | Microsoft Azure'i" 
    services="cloud-services"
    description="Koos Visual Studio profiler pilveteenustega jõudlusprobleemide uurimine" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Testimise pilveteenus jõudlus kohalikult Azure Arvuta emulaator Profiler Visual Studio abil

Mitmesuguseid tööriistu ja meetodite abil on saadaval katsetamiseks pilveteenuste jõudlust.
Kui avaldate pilveteenus Azure, võib teil profiilide andmete kogumise ja siis analüüsida, nagu kirjeldatud [profiilide Azure'i rakenduse]Visual Studio[1].
Samuti saate diagnostika jälgida erinevaid jõudluse hinnale, nagu on kirjeldatud [kasutamine jõudluse hinnale Azure][2].
Võite ka oma rakenduses kohalikult Arvuta emulaator enne juurutamist selle pilveteenusesse profiil.

Selles artiklis antakse ülevaade CPU valimite meetod profiili, mida saab teha kohalik emulaator. CPU valimite on meetod profiili, mis ei ole väga pealetükkiv. Määratud valimite intervalliga soovitud profiler suunab kõne virnas hetktõmmise. Andmed on aja jooksul kogutud ja näidatud aruande. See meetod profiilide sageli näitavad, kus arvesatud intensiivse rakenduses enamik CPU tööd on tehtud.  See annab teile võimaluse keskenduda "kuum tee", kus teie rakendus on kulutuste kõige rohkem aega.



## <a name="1-configure-visual-studio-for-profiling"></a>1: profiili Visual Studio konfigureerimine

Esmalt on mõned Visual Studio konfiguratsiooni võimalusi, mis võib olla kasulik, kui profiili. Arusaamiseks profiilide aruandeid, peate sümboleid (.pdb failid) rakenduse ja ka sümbolid süsteemi teekide jaoks. Peaksite veenduge, et viidatava serverid saadaval sümbol. Selleks klõpsake Visual Studio menüü **Tööriistad** nuppu **Suvandid**ja siis valige **silumine**, siis **sümboleid**. Veenduge, et Microsoft sümbol serverid on loetletud **sümbol failiasukohtade (.pdb)**.  Saate otsida ka http://referencesource.microsoft.com/symbols, mis võib olla tähis failid.

![Sümboli suvandid][4]

Soovi korral saate lihtsustada aruanded, mis on profiler genereeritud seadmisega ainult minu kood. Ainult minu koodi lubatud, on funktsioon kõne virnadena lihtsustatud nii, et kõned täiesti sisemise teekide ja .NET Framework on peidetud aruanded. Klõpsake menüü **Tööriistad** nuppu **Suvandid**. Seejärel laiendage **Jõudluse tööriistad** ja valige **Üldine**. Märkige ruut **Luba ainult minu koodi profiler aruannete**jaoks.

![Ainult minu koodi suvandid][17]

Olemasoleva projekti või uue projekti, saate neid juhiseid.  Kui loote uue projekti proovige allpool kirjeldatud meetodid, valige C# **Azure'i pilveteenuses** projekti ja valige **Web rolli** ja **Töötaja roll**.

![Azure'i pilveteenuses projekti rollid][5]

Näiteks lisada taustvärvi, mis kulub palju aega ja mõned selge JÕUDLUSPROBLEEM näitab projekti mõned koodi. Töötaja rolli projekti lisada näiteks järgmine kood:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Helistage järgmine kood töötaja rolli RoleEntryPoint saadud tunni RunAsync-meetod. (Ignoreeri hoiatust töötab sünkroonselt meetod).

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Koostamine ja käivitage oma pilveteenuses kohalikult ilma silumine (Ctrl + F5) seatud **väljaanne**lahenduse konfiguratsioon. See tagab, et kõik failid ja kaustad luuakse kohalik töötab rakendus ja tagab kõigi emulators käivitatud. Veenduge, et teie töötaja rolli töötab alustage arvutada emulaator UI tegumiriba kaudu.

## <a name="2-attach-to-a-process"></a>2: protsessi manustamine

Rakenduse profiilide, alates Visual Studio 2010 IDE asemel peab manustada selle profiler töötavad protsessi. 

Valige soovitud profiler manustamiseks protsessi, klõpsake menüüs **analüüsi** **Profiler** ja **Lisa/eemalda**.

![Profiili suvand manustamine][6]

Töötaja roll, otsige üles WaWorkerHost.exe protsess.

![WaWorkerHost protsess][7]

Kui projekti kausta on võrgukettale, on profiler küsib teilt muusse asukohta salvestamine profiilide aruanded.

 Samuti saate manustada web rolli lisades WaIISHost.exe.
Kui teie taotlus on mitme töötaja rolli protsesside, peate kasutama funktsiooni processID eristada. Saate päringu soovitud processID programmiliselt kasutades protsessi objekti. Näiteks kui RoleEntryPoint saadud roll klassi meetodit selle koodi lisamist saate vaadata Logi arvutada emulaator UI teada, millised protsess ühenduse loomiseks.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Vaatamiseks käivitage arvutada emulaator UI.

![Arvuta emulaator UI käivitamine][8]

Avage klõpsates konsooli akna tiitliriba arvutada emulaator UI töötaja rolli Logi konsooli aken. Saate vaadata protsessi ID Logi sisse.

![Kuva protsessi ID][9]

Üks manustamist toimingute rakenduse UI (vajadusel) paljundada seda stsenaariumi.

Kui soovite peatada profiili, valige **Peata profiili** link.

![Profiili suvand peatamine][10]

## <a name="3-view-performance-reports"></a>3: jõudluse aruannete kuvamine

Toimivuse aruanne rakenduse jaoks kuvatakse.

Selles etapis soovitud profiler lõpetab käivitamisel, salvestab andmed .vsp faili ja kuvab aruande, mis kuvatakse andmete analüüs.

![Profiler aruanne][11]


Kui näete String.wstrcpy kuum tee, klõpsake lihtsalt minu koodi muuta Kuva ainult kasutaja kood.  Kui näete String.Concat, proovige nupu Kuva kõik kood.

Peaksite nägema Concatenate meetodi ja String.Concat võtaks täitmisaeg suur osa.

![Analüüsi aruanne][12]

Kui lisasite selle artikli stringi ühendamine koodi, peaksite nägema selle tööülesandeloendi hoiatus. Samuti võite näha on liiga palju Prügikoristus, mis on stringid, mis on loodud ja müüa arvu tõttu hoiatus.

![Jõudluse hoiatused][14]

## <a name="4-make-changes-and-compare-performance"></a>4: muudatused ja võrrelda tulemusi

Samuti saate võrrelda jõudlus enne ja pärast koodi muutmine.  Töötab protsessi lõpetamine ja asendada stringi ühendamine toimingu StringBuilder kasutamine koodi redigeerimine.

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Tehke muu jõudluse Käivita ja seejärel võrrelda. Jõudluse Exploreris, kui jookseb sama seansi jooksul, on teil saate lihtsalt valida nii aruannet, kiirmenüü avamine, ja valige **Võrdlus Jõudlusearuannete**. Kui soovite võrrelda kestab mõne muu jõudluse seansi, saate avada menüü **analüüsi** ja valige **Jõudlusearuannete võrdlus**. Määrake kuvatavas dialoogiboksis mõlemad failid.

![Jõudluse aruannete suvand võrdlus][15]

Aruannete erinevused kahe käivitatakse esile tõsta.

![Aruande võrdlus][16]

Palju õnne! Olete saanud alustada selle profiler.

## <a name="troubleshooting"></a>Tõrkeotsing

- Veenduge, et teil on profiili väljaanne koostamine ja käivitage ilma silumine.

- Kui suvand Lisa/eemalda Profiler menüü pole lubatud, käivitage viisard jõudlust.

- Arvutage emulaator Kasutajaliidese abil saate vaadata oma rakenduste olekut. 

- Kui teil on probleeme alates rakenduste emulaator või manustamise profiler, välja lülitada Arvuta emulaator alla ja käivitage see uuesti. Kui see probleemi ei lahenda, siis taaskäivitage. See probleem võib ilmneda juhul, kui abil saate arvutada emulaator peatada ja käivitatud juurutuste eemaldada.

- Kui kasutasite profiilide käske käsureal, eriti Globaalsätete veenduge, et VSPerfClrEnv /globaloff nimega ja VsPerfMon.exe on suletud.

- Kui kui proovide, kuvatakse teade "PRF0025: andmed on kogutud," märkige ruut, et te lisatud protsessi on CPU tegevus. Rakendusi, mis ei tee arvutuslik töö võib tooda valimite andmed.  Samuti on võimalik, et protsessi väljunud enne tahes. Kontrollige, et rolli, mida teil on profiilide meetodit ei lõpeta.

## <a name="next-steps"></a>Järgmised sammud

Visual Studio profiler instrumenting Azure kahendfaile emulaator ei toetata, kuid kui soovite testida mälu jaotamine, saate valida selle suvandi, kui profiili. Saate valida ka kokkulangevus profiili, mis aitab teil otsustada, kas teemad on aega raisata kiiruisutamises lukud või taseme suhtluse profiili, mis aitab teil välja selgitada jõudlusprobleemide kasutamisel vahel astme rakenduse kõige sagedamini vahel andmed teise töötaja roll.  Saate vaadata oma app loob andmebaasi päringute ja profiilide andmete abil saate parandada oma andmebaasi kasutamine. Esimese taseme suhtluse profiilide kohta leiate teavet teemast ajaveebipostitusest [kiirtutvustus: esimese taseme suhtluse Profiler kasutamine Visual Studio meeskonnatöö süsteemi 2010][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 