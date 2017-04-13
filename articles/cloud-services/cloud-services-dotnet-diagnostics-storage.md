<properties
    pageTitle="Salvestamine ja View Diagnostikaandmete Azure Storage | Microsoft Azure'i"
    description="Azure'i diagnostika andmeid tuua Azure Storage ja seda vaadata"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Salvestamine ja vaate Diagnostikaandmete Azure Storage

Diagnostikaandmete on talletatud juhul, kui teisaldate Microsoft Azure'i salvestusruumi emulaator või Azure storage jäädavalt. Üks kord laos, seda saab vaadata ühe saadaval mitmesuguseid tööriistu.

## <a name="specify-a-storage-account"></a>Määrake salvestusruumi konto

Saate määrata salvestusruumi konto, mida soovite kasutada ServiceConfiguration.cscfg faili. Konto teave on määratletud ühendusstringi konfiguratsioon sätte. Järgmises näites kuvatakse vaikimisi ühendusstringi loonud uue pilveteenuses projekti Visual Studio:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Saate muuta selle konto teavet Azure storage konto ühendusstring.

Sõltuvalt Diagnostikaandmete, mis on kogutud, kasutab diagnostika Azure'i bloobimälu teenuse või tabeli teenuse. Järgmises tabelis on jätkunud andmeallikad ja nende vormi.

|Andmeallikas|Salvestusruumi vorming|
|---|---|
|Azure'i logid|Tabel|
|IIS 7.0 logid|Bloobimälu|
|Azure'i diagnostika taristu logid|Tabel|
|Taotleda Jälita logid nurjus|Bloobimälu|
|Windowsi sündmuste logid|Tabel|
|Jõudluse hinnale|Tabel|
|Puistab ootamatult sulguda|Bloobimälu|
|Kohandatud Tõrkelogide|Bloobimälu|

## <a name="transfer-diagnostic-data"></a>Diagnostikaandmete edastamine

SDK 2,5 ja hiljem taotluse edastamiseks Diagnostikaandmete võivad tekkida konfiguratsiooni faili kaudu. Saate edastada Diagnostikaandmete ajastatud intervalliga konfiguratsioonis määratud viisil.

Eelmise ja SDK 2.4 saate taotleda edastamiseks diagnostikateavet kaudu konfiguratsioonifail samuti nagu programmiliselt. Programmiline lähenemine võimaldab teil teha nõudmisel edastamine.


>[AZURE.IMPORTANT] Kui teisaldate Diagnostikaandmete Azure storage konto, on teil tekkida salvestusruumi ressursse, mis kasutab teie Diagnostikaandmete kulud.

## <a name="store-diagnostic-data"></a>Diagnostikaandmete talletamine

Logi andmed salvestatakse bloobimälu või tabeli salvestusruumi järgmised nimed:

**Tabelid**

- **WadLogsTable** - kood, kasutades sõnumijälgimise kuulajale kirjutatud logid.

- Muudab **WADDiagnosticInfrastructureLogsTable** - diagnostika kuvar ja konfigureerimine.

- **WADDirectoriesTable** – kataloogide, mis jälgib diagnostika kuvar.  See hõlmab IIS-i logid, IIS-i nurjus taotluse logid ja kohandatud kataloogide.  Container väljal määratletud bloobimälu logifaili asukoht ja nimi on bloobimälu on RelativePath välja.  AbsolutePath väljal faili nimi ja asukoht nii nagu see oli Azure virtuaalse masina.

- **WADPerformanceCountersTable** – jõudluse hinnale.

- **WADWindowsEventLogsTable** – Windowsi sündmuste logid.

**Plekid**

- **wad-juhtelemendi-container** – (ainult SDK 2.4 ja eelmise) sisaldab XML-i konfiguratsiooni faile, mis määrab Azure diagnostika.

- **wad-iis-failedreqlogfiles** – sisaldab teavet IIS-i nurjus taotlemine logib.

- **wad-iis-logifailidele** – sisaldab teavet IIS-i logib.

- **"kohandatud"** – kohandatud ümbris konfigureerimise diagnostika kuvari jälgitavate kataloogide põhjal.  See bloobimälu ümbris nime täpsustatakse WADDirectoriesTable.

## <a name="tools-to-view-diagnostic-data"></a>Diagnostikaandmete kuvamiseks tööriistad
Pärast seda kantakse salvestusruumi andmete kuvamiseks saadaval on mitu tööriista. Näiteks:

- Server Explorer Visual Studio – kui olete installinud Microsoft Visual Studio, Azure'i tööriistad saate sõlme Azure Storage Server Explorer kontodelt Azure storage kirjutuskaitstud bloobimälu ja tabeli andmete kuvamiseks. Andmeid saate kuvada kohalikku emulaator kontolt ning samuti salvestusruumi kontode loomist Azure. Lisateavet leiate teemast [sirvimine ja haldamise salvestusruumi ressursid serveri Exploreriga](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Microsoft Azure'i salvestusruumi Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) on autonoomne rakendus, mis võimaldab teil hõlpsasti andmetega töötamiseks Azure Storage Windows, OSX ja Linux.

- [Azure'i Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) sisaldab Azure'i diagnostika Manager, mis võimaldab teil vaadata, alla laadida ja diagnostika töötavad Azure rakendused kogutud andmete haldamine.


## <a name="next-steps"></a>Järgmised sammud

[Jälita voogu pilveteenustega rakenduses Azure'i diagnostika](cloud-services-dotnet-diagnostics-trace-flow.md)
