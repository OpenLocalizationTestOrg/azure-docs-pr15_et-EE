<properties
    pageTitle="Azure'i paketi teenuse põhitõed | Microsoft Azure'i"
    description="Lugege teemat Azure paketi teenuse suuremahuliste samal ajal ja HPC töökoormus"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Azure'i paketi põhialused

Azure'i paketi võimaldab teil käivitada rakendusi suuremahuliste paralleelselt ja suure jõudlusega arvutuste (HPC) tõhus pilveteenuses. See on platform teenus, mis koosolekut kavandava Arvuta mahukat tööd käitamist virtuaalmasinates hallatavate kogumi ja saab automaatselt skaala arvutada ressursse, mis teie töö vajadustele.

Teenusega paketi määratleda Azure Arvuta ressursse, mis teie rakendusi käivitada, samal ajal ja ulatuses. Nõudmisel või ajastatud käivitada töökohtade ning te ei pea käsitsi, konfigureerimiseks ja loomine ja haldamine on HPC kobar, üksikute virtuaalmasinates, virtuaalse võrgu või keerukas töö tööülesande ajastamise taristu.

## <a name="use-cases-for-batch"></a>Paketi karpide kasutamine

Paketi on hallatavate Azure'i teenus, mis on kasutatud *Pakktöötlus* või *paketi arvutuste*--töötab suurel hulgal sarnaseid ülesandeid mõnda soovitud tulemuse saamiseks. Ettevõtted, mis regulaarselt töötlemine, muuta ja analüüsida suuri andmemahtusid kõige sagedamini kasutatavaid paketi arvuti.

Paketi toimib hästi otseselt paralleelselt (tuntud ka kui "piinlikult paralleelselt") rakendusi ja töökoormus. Mitme toiminguid sooritada töö korraga mitmesse arvutisse jagatakse hõlpsalt otseselt paralleelsetes töökoormus.

![Paralleelsetes tööülesanded][1]<br/>

Mõned töökoormus, mis on tavaliselt töödeldud selle meetodi kasutamise näited:

* Rahandus risk modelleerimine
* Kliima ja hüdroloogia Andmeanalüüs
* Pildi renderdamine, analüüsi ja töötlemine
* Meediumide kodeerimise ja transkodeerida
* Geneetilise järjestuse analüüs
* Matemaatika erifunktsioonid stressi analüüs
* Tarkvara testimine

Paketi saate paralleelselt arvutuste vähendamine toiminguga lõpus ja käivitada keerukamaid HPC töökoormus näiteks [Sõnumi läbides kasutajaliidese (MPI)](batch-mpi.md) rakendused.

Paketi ja muud HPC lahenduse suvandid Azure võrdlus, leiate teemast [paketi ja HPC lahendusi](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Pakett-arendamise

Paralleelsetes töökoormus partii töötlemine on tavaliselt teinud programmiliselt, kasutades ühte [Paketi API -d](#batch-development-apis). Paketi API-de abil saate luua ja hallata kaustu, Arvuta sõlmed (virtuaalmasinates) ja tööde ja ülesannete need sõlmed käivitamiseks. Klientrakenduse või teenus, mis teil autor kasutab suhelda paketi teenuse paketi API-d.

Tõhus saate töötlemine suuremahuliste töökoormus ettevõtte või teenuse vastendamisel anda oma klientidele, et ta saab käitada ülesannete--ja nõudmisel või ajakava--üks, sadu või isegi tuhandete sõlmed. Samuti saate paketi suuremat töövoo, hallatavate selliste tööriistadega nagu [Azure andmete Factory](../data-factory/data-factory-data-processing-using-batch.md)osana.

> [AZURE.TIP] Kui olete valmis minna paketi API põhjalikumalt mõistmiseks pakub funktsioone, lugege teemat [paketi funktsioon ülevaade arendajatele](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Azure'i peate kontod

Kui teil tekib paketi lahendusi, saate kasutada järgmisi kontosid Microsoft Azure.

- **Azure'i konto ja tellimuse** – kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu][msdn_benefits], või registreeruda [tasuta Azure'i konto][free_account]. Konto loomisel luuakse vaikimisi tellimuse jaoks.

- **Paketi konto** - kui rakenduste suhelda paketi teenuse konto nimi konto ja kiirklahvide URL-i kasutatakse mandaati. Kõik teie paketi ressursid kaustu, nt arvutada sõlmed, töö, ja ülesanded oleksid paketi kontoga seostatud. Saate [luua paketi konto](batch-account-create-portal.md) Azure'i portaalis.

- **Salvestusruumi konto** - pakett sisaldab tugi [Azure]Storage failidega töötamine[azure_storage]. Peaaegu iga paketi stsenaarium kasutab Azure Storage--jaoks tööülesannete töötavad programmid ja töötlemist andmeid ja väljundi andmeid, mida nad luua hoidmiseks. Salvestusruumi konto loomiseks vaadake teemat [Azure kohta salvestusruumi kontod](./../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>Paketi arengu API-d

Teie rakenduste ja teenuste kohta saate probleemi otsese REST API kõned, üks või mitu järgmist kliendi teegid või mõlemaid haldamiseks kasutada arvutada materjale ja Käivita paralleelselt töökoormus paketi teenuse kasutamise tasandil.

| API-GA    | API viide | Laadi alla | Koodinäiteid |
| ----------------- | ------------- | -------- | ------------ |
| **Paketi ülejäänud** | [MSDN-I][batch_rest] | N/A | [MSDN-I][batch_rest] |
| **Paketi .net-i**    | [MSDN-I][api_net] | [Nugeti][api_net_nuget] | [GitHub][api_sample_net] |
| **Paketi Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Paketi Node.js** | [github.IO][api_nodejs] | [NPM][api_nodejs_npm] | - |
| **Paketi Java** (eelvaade) | [github.IO][api_java] | [Maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Paketi ressursside haldamine

Lisaks kliendi API-d, saate kasutada ka järgmist haldamiseks ressursid paketi kontol.

- [PowerShelli cmdlet-käskude partii][batch_ps]: [Azure'i PowerShelli](../powershell-install-configure.md) mooduli The Azure'i paketi cmdlet-käskude võimaldavad paketi ressursid PowerShelliga haldamine.

- [Azure'i CLI](../xplat-cli-install.md): Azure'i käsurea liides (Azure'i CLI) on mitu platvormi tööriistakomplekt, shell käsud leiate palju Azure teenuseid, sh paketi suhtlemiseks.

- [Paketi halduse .net-i](batch-management-dotnet.md) kliendi teek: saadaval ka kaudu [Nugeti][api_net_mgmt_nuget], paketi halduse .net-i kliendi teek abil saate hallata programmiliselt paketi kontod, kvootide ja rakenduse paketid. Viide juhtimine teek on [MSDN]-is[api_net_mgmt].

### <a name="batch-tools"></a>Paketi tööriistad

Pole vajalik luua lahendusi, kasutades paketi, siin on mõned väärtuslik vahendid ajal koostamise ja silumine teie paketi rakenduste ja teenuste kasutamiseks.

 - [Azure'i portaalis][portal]: saate luua, jälgida ja kustutada paketi kaustu, töö ja Azure portaali paketi labad tööülesanded. Saate vaadata neid oleku teave ja muud ressursid ning samal ajal käivitada oma tööd, isegi failide allalaadimiseks oma kaustadesse Arvuta sõlmed (allalaadimine nurjunud tööülesande `stderr.txt` ajal tõrkeotsing, näiteks). Samuti saate alla laadida kaugtöölaua (RDP) faile, mille abil saate sisse logida arvutada sõlmed.

 - [Azure'i paketi Exploreri][batch_explorer]: paketi Explorer pakub sarnase paketi ressursside halduse funktsiooni Azure portaali, kuid eraldi Windows Presentation Foundationi (WPF) klientrakendusega. Üks pakett .NET valimi rakendusi saadaval [github][github_samples], saate luua selle kohal või Visual Studio 2015 ja seda vaadata ja hallata teie paketi konto ressursside ning samal ajal töötada silumine teie paketi lahendusi kasutada. Kuva töö, pool ja tööülesande üksikasjad, Arvuta sõlmed failide allalaadimiseks ja ühendage sõlmed kaugühenduse teel, kasutades kaugtöölaua (RDP) faile alla laadida paketi Exploreriga.

 - [Microsoft Azure'i salvestusruumi Exploreri][storage_explorer]: ajal mitte tingimata Azure'i paketi tööriista salvestusruumi Explorer on mõne muu väärtuslik vahend olema ajal programmeerimise ja silumine teie paketi lahendusi.

## <a name="scenario-scale-out-a-parallel-workload"></a>Stsenaarium: Skaala läbi paralleelselt töökoormus

Levinud lahenduse, mis kasutab paketi API suhelda paketi teenuse hõlmab skaleerimist otseselt paralleelselt tööd – näiteks renderdamine pildid 3D stseene--Arvuta sõlmed puhul. Selle kogumi Arvuta sõlmed võib olla teie "Renderda serveripargis", mis pakub kümneid, sadu või isegi tuhandeliste valdkond oma renderdamise töö, näiteks.

Järgmisel joonisel on levinud paketi töövoo klientrakendusega või majutatud teenuse abil paketi käivitamiseks paralleelselt töökoormus.

![Paketi lahenduse töövoo][2]

Levinud stsenaariumi töötleb oma rakenduse või teenuse arvutuslik töökoormus Azure'i partii tehes järgmist:

1. Laadige **failid** ja **rakendus** , mis töötleb need failid Azure Storage kontole. Sisestuskeel faile saab andmeid, mida rakenduse töötleb, nt finantsmudel andmete või videofaile olema videovormingust. Rakenduse failid võivad olla mis tahes rakendus, mis on kasutada andmed, nt 3D muudab taotluse või media transcoder töötlemiseks.

2. Luua paketi **rakenduskausta** Arvuta sõlmed teie paketi konto--sõlmed on virtuaalmasinates, mis käivitatakse tööülesanded. Saate määrata atribuudid, nt [sõlm suurus](./../cloud-services/cloud-services-sizes-specs.md), nende operatsioonisüsteem ja asukoha Azure Storage rakenduse installimiseks kui sõlmed liituda rakenduskausta (rakendus üleslaaditud samm #1). Samuti saate konfigureerida [automaatselt mastaapida](batch-automatic-scaling.md)--rakenduskausta dünaamiliselt kohandada Arvuta sõlmed kogumi--vastuseks tööülesannete andvate töökoormus.

3. Looge paketi **töö** käitamist Arvuta sõlmed kogumi töökoormus. Kui loote tööd, saate seostada paketi kausta.

4. Projekti **tööülesannete** lisada. Kui tööülesannete lisamine projekti paketi teenuse automaatselt koosolekut kavandava ülesannete täitmiseks Arvuta sõlmed kogumi. Iga tööülesande kasutab rakendus, mida te töötlemine Sisestuskeel faile üles laadida.

    - 4a. Enne tööülesande aktiveeritakse, seda saab alla laadida MSDN, see on määratud protsess on andmeid (Sisestuskeel failid). Kui rakendus pole juba installitud sõlme (vt samm #2), seda saab alla siin hoopis. Kui allalaadimine on lõpule viidud, käivitada oma määratud sõlmed tööülesanded.

5. Kui tööülesanded käivitada, saate teha päringuid paketi töö ja ülesannete täitmise edenemist jälgida. Oma kliendi rakenduse või teenuse suhtleb paketi teenuse https, ja Kuna te võib jälgi tuhandeliste tööülesannete töötavate tuhandete Arvuta sõlmed, olla kindel, et [päringu paketi teenuse tõhus](batch-efficient-list-queries.md).

6. Tööülesannetena, mis on lõpule jõudnud, saate üles laadida oma tulemi andmed Azure Storage. Faile saate alla laadida ka otse Arvuta sõlmed.

7. Kui teie jälgimine tuvastab, et tööülesanded oma töö lõpetanud, allalaadimise oma kliendi rakenduse või teenuse väljundi andmete edasiseks töötlemiseks või hindamiseks.

Pidage meeles, see on ainult üks võimalus kasutada paketi ja selle stsenaariumi kirjeldab vaid mõned selle saadaolevate funktsioonide. Näiteks [samaaegselt mitu ülesannet](batch-parallel-node-tasks.md) täidate iga Arvuta sõlme ja [tööülesannete ettevalmistamine ja lõpetamise](batch-job-prep-release.md) abil saate oma töö sõlmed ettevalmistamine ja seejärel puhastamiseks hiljem.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui teil on üksikasjalik ülevaade paketi teenus, on aeg tõrkeid süvitsi saate teada, kuidas kasutada seda töödelda oma Arvuta mahukat paralleelselt töökoormus.

- Lugege [paketi funktsioon ülevaade arendajatele](batch-api-basics.md), oluline teave kõigile, kes hakkab kasutama paketi. See artikkel sisaldab täpsemat teavet paketi teenuste ressursid nagu kaustu, sõlmed, töö, ja ülesanded ja palju API funktsioone, mida saate kasutada rakenduse paketi koostamise ajal.

- Saate teada, kuidas kasutada C# ja paketi .net-i teek on lihtne töökoormus kasutamine levinud paketi töövoo käivitada [Alustamine Azure'i paketi teegi .net-i jaoks](batch-dotnet-get-started.md) . Selles artiklis peaks olema üks teie esimene peatub õppimisel paketi teenuse kasutamise kohta. On ka õpetuse [Python versiooni](batch-python-tutorial.md) .

- Laadige alla [koodinäiteid github] [ github_samples] näha, kuidas nii C# ja Python võib liides paketi abil ajakava ja protsess valimi töökoormus.

- Tutvuge [Paketi Õppeteema] [ learning_path] saada aimu, milline ressursse, mis on saadaval teile, nagu te saate teada, kuidas töötada paketi.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
