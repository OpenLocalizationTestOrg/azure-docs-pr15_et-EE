<properties
    pageTitle="End-to-End tõrkeotsingu Azure'i talletusruumi mõõdikute ja logimine, AzCopy ja sõnumi Analyzer abil | Microsoft Azure'i"
    description="Näitab lõpuni tõrkeotsingu Azure'i salvestusruumi Analytics, AzCopy ja Microsoft sõnumi Analyzer õpetus"
    services="storage"
    documentationCenter="dotnet"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Azure'i talletusruumi mõõdikute ja logimine, AzCopy ja sõnumi Analyzer abil lõpuni – tõrkeotsing

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Ülevaade

Diagnoosimise ja tõrkeotsing on oluline oskus koostamise ja toetavad Microsoft Azure Storage klientrakendustes. Tõttu jaotatud laadi Azure rakendus, võib diagnoosimise ja tõrgete ja jõudlusega seotud probleemide tõrkeotsingu märksa keerukam kui traditsiooniline keskkonnas.

Selles õpetuses demonstreerime tuvastamise kliendi vigu, mis võivad mõjutada jõudlust ja nende tõrkeotsing: lõpuni tööriistadega Microsoft ja Azure Storage, et optimeerida klientrakendust.

Selle õpetuse pakub praktilise uurimine on lõpuni tõrkeotsingu stsenaariumi. Põhjalikumat kontseptuaalne juhendi tõrkeotsinguga Azure salvestamise rakendusi leiate artiklist [kuvaris, diagnoosimine, ja Microsoft Azure'i Tabelimäluga tõrkeotsing](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Tõrkeotsingu Azure Storage rakenduste tööriistad

Kasutades Microsoft Azure'i Tabelimäluga klientrakendustes tõrkeotsinguks saate kindlaks teha, kui mõni probleem ilmnes ja probleemi põhjuseks võib olla kombinatsiooni abil. Nende tööriistade järgmised.

- **Azure'i salvestusruumi Analytics**. [Azure'i salvestusruumi Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) pakub mõõdikute ja Azure Storage logimine.
    - **Talletusruumi mõõdikute** saab tehingu mõõdikute ja võimsus mõõdikute salvestusruumi konto jaoks. Mõõdikute kasutamisel saate määrata, kuidas teie taotlus on läbimiseks vastavalt mitmesuguseid eri mõõdud. Leiate lisateavet mõõdikute jälitatud salvestusruumi Analytics tüüpi [Salvestusruumi Analytics mõõdikute tabeli skeemi](http://msdn.microsoft.com/library/azure/hh343264.aspx) .

    - **Salvestusruumi logimine** logib iga taotluse Azure Storage teenuste serveripoolne Logi. Logi jälitab iga taotluse, sh sooritatud toimingu, toiming ja latentsuse teave oleku üksikasjalikud andmed. Koosolekukutsete ja kutsele vastamise andmeid, mis on kirjutatud logid salvestusruumi Analytics kohta lisateavet teemast [Salvestusruumi Analytics Log vormindamine](http://msdn.microsoft.com/library/azure/hh343259.aspx) .

> [AZURE.NOTE] Salvestusruumi kontod dispersioonanalüüs tüüpi Zone liigsete mälu (ZRS) pole mõõdikute või logimise võimalus praegu lubatud. 

- **Azure'i portaalis**. Saate konfigureerida [Azure portaali](https://portal.azure.com)konto salvestusruumi mõõdikute ja logimine. Saate vaadata diagrammid ja graafikud, mis näitavad, kuidas teie taotlus on läbimiseks aja jooksul, ja konfigureerida teatiste teavitavad teid, kui teie rakendus teeb määratud mõõdiku oodatust.

    Kohta leiate teavet [kuvari salvestusruumi konto Azure'i portaalis](storage-monitor-storage-account.md) konfigureerimise Azure'i portaalis jälgimine.

- **AzCopy**. Azure Storage logid salvestatakse plekid, abil saate AzCopy kopeerimiseks log plekid kohalikku kausta analüüsimiseks, Microsofti sõnumi Analyzer abil. AzCopy kohta lisateabe saamiseks vaadake [AzCopy käsurea kasuliku andmete edastamiseks](storage-use-azcopy.md) .

- **Microsoft sõnumi analüsaator**. Sõnumi Analyzer on tööriista, mis tarbib logifailid ja kuvab logiandmed visuaalse vormingus, mis lihtsustab rühma Logi andmete filtreerimine, otsimine ja kasulik komplekti, mille abil saate analüüsida tõrgete ja jõudlusega seotud probleemide sisse. Leiate lisateavet sõnumi analüsaator [Microsoft sõnumi analüsaator opsüsteem juhendist](http://technet.microsoft.com/library/jj649776.aspx) .

## <a name="about-the-sample-scenario"></a>Valimi kohta

Selles õpetuses mõeldud uurime stsenaarium, kus Azure Storage mõõdikute näitab madal protsendimäärana edukust Azure storage rakendus. Madal protsendimäärana edu määr meetermõõdustik (näidatakse **PercentSuccess** [Azure portaali](https://portal.azure.com) ja mõõdikute tabelite) jälitab toimingud, mida õnnestub, kuid mis tagastavad HTTP olekukoodi, mis on suurem kui 299. Serveripoolne salvestusruumi logifailide, on need toimingud salvestatud **ClientOtherErrors**kande olek. Madal protsendimäärana edu meetermõõdustik kohta leiate lisateavet teemast [mõõdikute kuvamine madal PercentSuccess või analytics Logi kirjed on kande olek ClientOtherErrors toiminguid](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure'i salvestusruumi toimingute võib tagastada HTTP olekukoodi 299 suurem, tavaline funktsioonide osana. Kuid mõnel juhul nende vigade näitab, et teil on võimalik, et optimeerida klientrakenduse jaoks paranenud jõudlus.

Selle stsenaariumi korral käsitleme madal protsendimäärana edukust kõike alla 100%. Saate valida argumendil erineval tasemel, kuid vastavalt oma vajadustele. Soovitame rakenduse testimisel luua võrdlusalus hälve oma tootluse mõõdikute jaoks. Näiteks võib kindlaks teha, alusel katsetamine, et teie rakendus peaks olema ühtse protsendimäärana edukust 90% või 85%. Kui andmete mõõdikute näitab, et rakendus on mõõtmetest numbrit, siis saate uurida, mis võivad põhjustada kasv.

Valimi stsenaariumi pärast oleme tuvastanud, et protsendimäärana õnnestumise määr meetermõõdustik on väiksem kui 100%, uurime logid tõrkeid, et mõõdikud oleksid ja nende abil selgitada, mis põhjustab alumise protsendimäärana edukust leidmiseks. Vaatame konkreetselt tõrgete 400 vahemikus. Seejärel me lähemalt uurida 404 (ei leitud) vead.

### <a name="some-causes-of-400-range-errors"></a>Mõned põhjused 400-vahemiku tõrked

Alltoodud näiteid kuvatakse asutuste vigu 400-vahemiku taotluste Azure'i bloobimälu ja nende võimalikud põhjused. Mis tahes nende vigade, samuti tõrgete 300 vahemik ja 500 vahemiku saate kaasa madal protsendimäärana edukust.

Pange tähele, et loendid, mis on allpool ei ole lõpule viidud. [Oleku ja tõrkekoodide](http://msdn.microsoft.com/library/azure/dd179382.aspx) MSDN-is üksikasjad leiate Azure'i salvestusruumi üldised tõrked ja kõigi teenuste salvestusruumi kindlate tõrgete kohta.

**Oleku koodi 404 (ei leitud) näited**

Juhul, kui Lugemistoiming container või bloobimälu nurjub, kuna bloobimälu või container ei leitud.

- Juhul, kui teine klient enne selle taotlus on kustutatud container või bloobimälu.
- Juhul, kui kasutate API kõne, mis loob container või bloobimälu pärast kontrollimine, kas see on olemas. CreateIfNotExists API-de helistamine pea esmalt, et otsida container või bloobimälu; kui see pole, tagastatakse tõrge nr 404 ja seejärel teise pane kõne tehakse kirjutada container või bloobimälu.

**Oleku koodi 409 (konflikt) näited**

- Juhul, kui loomine API abil saate luua uue container või bloobimälu, ilma esmalt olemasolu kontrollimine ja container või bloobimälu nimega on juba olemas.
- Juhul, kui ümbris kustutatakse ja proovite luua uus ümbris sama nimega, enne kui kustutamine on lõpule viidud.
- Juhul, kui määrate rendilepingu container või bloobimälu ja seal on juba ühendussündmus Esita.

**Olekukoodi 412 (eelduseks nurjus) näited**

- Juhul, kui tingimusvormingu päise määratud tingimus ei ole täidetud.
- Juhul, kui määratud üürilepingu ID-d ei vasta üürilepingu ID container või bloobimälu.

## <a name="generate-log-files-for-analysis"></a>Luua logifailide analüüsimiseks

Selles õppetükis kasutame sõnumi analüüsiriista töötamiseks kolme eri tüüpi logifailid, kuigi võite määrata, et töötada ühte järgmistest:

- **Serveri logifailide**, mis on loodud, kui Azure Storage logimise lubamine. Serveri logi sisaldab andmeid iga toimingu Azure Storage teenuse - bloobimälu, järjekorda, tabeli või faili vastu. Serveri logi näitab, milline toiming kutsuti ja millist olekukoodi oli koosolekukutsete ja kutsele vastamise tagastatud, samuti muud üksikasjad.
- **.Net-i kliendi log**, mis on loodud, kui lubate kliendipoolne logimine kaudu oma .net-i rakendusest. Kliendi Logi sisaldab üksikasjalikku teavet kuidas kliendi valmistab taotlus ja võtab vastu ja töötleb vastus.
- **HTTP võrgu Jälita log**, mis kogub andmeid HTTP-või HTTPS koosolekukutsete ja kutsele vastamise andmed, sh Azure Storage vastu. Selles õpetuses loome võrgujälituse sõnumi Analyzer kaudu.

### <a name="configure-server-side-logging-and-metrics"></a>Serveripoolne logimine ja mõõdikute konfigureerimine

Esmalt vajame Azure Storage logimine ja mõõdikute, konfigureerida nii, et andmeid analüüsida klientrakenduse meil. Saate konfigureerida logimine ja mõõdikute mitmel viisil – [Azure portaali](https://portal.azure.com)kaudu, kasutades PowerShelli, või programmiliselt. [Lubamine talletusruumi mõõdikute ja mõõdikute andmete vaatamine](http://msdn.microsoft.com/library/azure/dn782843.aspx) ja [salvestusruumi logimise lubamine ja juurdepääs logiandmed](http://msdn.microsoft.com/library/azure/dn782840.aspx) MSDN-is leiate üksikasjalikku logimine ja mõõdikute konfigureerimise kohta.

**Azure'i portaali kaudu**

Logimine ja [Azure portaali](https://portal.azure.com)konto salvestusruumi mõõdikute konfigureerimiseks järgige veebisaidil [kuvari salvestusruumi konto Azure'i portaalis](storage-monitor-storage-account.md).

> [AZURE.NOTE] Ei saa määrata minute mõõdikute Azure'i portaalis. Soovitame siiski teie määratud selles õpetuses eesmärgil ja uurimiseks oma rakendusega jõudlusprobleeme. Saate seada minute mõõdikute PowerShelli kaudu, nagu allpool näidatud või programmiliselt abil salvestusruumi kliendi teek.
>
> Pange tähele, et Azure'i portaal ei saa kuvada minute mõõdikute ainult kord tunnis mõõdikute.

**PowerShelli kaudu**

Azure PowerShelliga alustamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

1. Cmdlet-käsu [Lisa-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) Azure kasutaja konto lisamiseks kasutage PowerShelli aknas:

    ```
    Add-AzureAccount
    ```

2. **Logige sisse Microsoft Azure'i** aknas tippige meiliaadress ja parool, mis on teie kontoga seotud. Azure'i autendib ja salvestab mandaadi teavet suletakse aken.
3. Määrata vaikekonto salvestusruumi te kasutate õpetuse täites need käsud PowerShelli aknas salvestusruumi konto.

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Bloobimälu teenuse jaoks salvestusruumi logimise lubamiseks tehke järgmist.

    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```
5. Luba talletusruumi mõõdikute bloobimälu teenuse, alustades seda kindlasti seatud **- MetricsType** `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>Konfigureerimine .NET kliendipoolne logimine

Kliendipoolne logimine .net-i rakenduse konfigureerimiseks lubada .NET diagnostika rakenduse konfigureerimise faili (web.config või app.config). [Kliendipoolne logimine teegiga .NET salvestusruumi klient](http://msdn.microsoft.com/library/azure/dn782839.aspx) ja [kliendipoolne logimine Microsoft Azure'i salvestusruumi SDK Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) MSDN-is leiate lisateavet.

Kliendipoolne logi sisaldab üksikasjalikku teavet kuidas kliendi valmistab taotlus ja võtab vastu ja töötleb vastus.

Salvestusruumi kliendi teek talletab kliendipoolne logiandmed rakenduse konfiguratsioonifailis (web.config või app.config) määratud asukohta.

### <a name="collect-a-network-trace"></a>Võrgujälituse kogumine

Sõnumi Analyzer abil saate koguda mõne HTTP-või HTTPS võrgujälituse klientrakenduse töötamise ajal. Sõnumi analüsaator kasutab [viiuldaja](http://www.telerik.com/fiddler) tagasi lõpuks. Enne selle võrgujälituse koguda, soovitame konfigureerida viiuldaja salvestada krüptimata HTTPS liiklust:

1. Installige [viiuldaja](http://www.telerik.com/download/fiddler).
2. Käivitage viiuldaja.
2. Valige **Tööriistad | Viiuldaja suvandid**.
3. Dialoogiboksis Suvandid, veenduge, et **Jäädvustada HTTPS ühendab** **Dekrüptida HTTPS-liikluse** valitud on nii, nagu allpool näidatud.

![Konfigureerige viiuldaja suvandid](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Juhend on koguda ja salvestada võrgujälituse esmalt sõnumi analüsaator, siis on analüüs seansiga analüüsimiseks jälitus ja logid loomine. Klõpsake sõnumi analüsaator võrgujälituse kogumiseks:

1. Valige sõnumi Analyzer, **faili | Kiirülevaate Jälita | Krüptimata HTTPS**.
2. Jälitus algab kohe. Valige **Peata** jälitus lõpetada, et saate configure Jälita salvestusruumi liiklus ainult.
3. Valige **Redigeeri** redigeerimiseks jälgimine seanssi.
4. Valige **Konfigureeri** link **Microsoft-Pef-WebProxy** ETW pakkuja paremal.
5. Klõpsake dialoogiboksis **Täpsemad sätted** vahekaarti **pakkuja** .
6. **Hostname (hostinimi)** filtrivälja Määrake oma salvestusruumi lõpp-punktid, eraldades need tühikud. Näiteks saate määrata oma lõpp-punktid järgmiselt: muuta `storagesample` salvestusruumi konto nimi:

    ```
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Sulgege dialoogiboks ja klõpsake **uuesti** alustamiseks kogumiseks jälitus hostname (hostinimi) filtri kohas, nii, et ainult Azure Storage võrguliiklust on kaasatud jälitus.

>[AZURE.NOTE] Kui olete lõpetanud oma võrgujälituse kogumiseks, soovitame, et te taastada sätteid, mida olete muutnud viiuldaja dekrüptimine HTTPS liiklust. Tühjendage dialoogiboksis Suvandid viiuldaja ruudud **HTTPS ühendab jäädvustada** ja **Dekrüptida HTTPS-liikluse** .

[Võrgu jälgimise funktsioonide abil](http://technet.microsoft.com/library/jj674819.aspx) TechNeti artiklist üksikasjalikumat teavet.

## <a name="review-metrics-data-in-the-azure-portal"></a>Azure'i portaalis mõõdikute andmete läbivaatamine

Kui rakenduse juba mõnda aega, saate vaadata mõõdikute diagrammide [Azure portaali](https://portal.azure.com) jälgida, kuidas teie teenus on läbimiseks kuvatavate. Esmalt liikuge Azure portaali konto salvestusruumi ja lisage diagrammi jaoks **Edu protsendi** mõõt.

Azure'i portaalis kuvatakse nüüd **Õnnestumise protsent** diagrammil jälgimisega seotud koos muude mõõdikute, mis on lisatud. Klõpsake soovitud stsenaarium kuvatakse uurime edasi logiandmete oleva sõnumi Analyzer, protsendimäärana edukust on väiksem kui 100%.

Leht jälgimis mõõdikute lisamise kohta leiate lisateavet teemast [kohta: mõõdikute mõõdikute tabelisse lisada](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] See võib võtta aega kuvada Azure'i portaalis, kui olete lubanud talletusruumi mõõdikute mõõdikute andmete jaoks. See on kord tunnis mõõdikute eelmise tunni ei kuvata Azure portaali kuni käesoleva tunni möödumist. Lisaks minute mõõdikute praegu ei kuvata Azure'i portaalis. Nii sõltuvalt sellest, kui lubate mõõdikute, võib kuluda kuni kaks tundi mõõdikute andmete kuvamiseks.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Kopeerige logid kohalikku kausta AzCopy abil

Azure'i salvestusruumi kirjutab server log andmete plekid ajal mõõdikute kirjutada tabelitele. Log plekid on saadaval tuntud `$logs` container salvestusruumi konto jaoks. Log plekid nimega aasta, kuu, päev ja tund, hierarhias, et leiaksite hõlpsalt hulga aega, mida soovite uurida. Näiteks kuvatakse `storagesample` konto container log plekid jaoks 01/02/2015: 8 – 9 am, on `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Üksikute plekid selles ümbrises nimega järjest, alustades `000000.log`.

AzCopy käsurea tööriista abil saate alla laadida neid serveripoolne logifailid soovitud kohta oma kohalikus arvutis. Näiteks saate kasutada järgmine käsk logifailide bloobimälu toiminguid, mis toimus 2 jaanuar 2015 kausta alla laadida `C:\Temp\Logs\Server`; Asendage `<storageaccountname>` salvestusruumi konto nimi ja `<storageaccountkey>` teie konto juurdepääsu võti:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy on [Azure](https://azure.microsoft.com/downloads/) allalaadimislehelt allalaadimiseks saadaval. Lisateavet AzCopy kasutamise kohta leiate teemast [AzCopy käsurea kasuliku andmete edastamiseks](storage-use-azcopy.md).

Allalaadimine serveripoolne logid kohta lisateabe saamiseks vt [allalaadimine salvestusruumi logimine logiandmed](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Kasutage Microsoft sõnumi Analyzer Logi andmete analüüsimiseks

Microsoft sõnumi Analyzer on tööriista hõivamine, kuvamise ja analüüsimine sõnumside liikluse, sündmused ja muud tõrkeotsingu ja diagnostika stsenaariumid või rakendus sõnumite protokolli. Sõnumi analüsaator ka võimaldab teil laadida, liitmine ja analüüsimiseks log ja jälgi faili salvestada. Sõnumi analüsaator kohta leiate lisateavet teemast [Microsoft sõnumi analüsaator opsüsteem juhend](http://technet.microsoft.com/library/jj649776.aspx).

Sõnumi analüsaator sisaldab varad Azure Storage, mis aitavad teil server, klient ja võrgu logid analüüsimiseks. Selles jaotises selgitame nende tööriistade kasutamine madal protsendimäärana edu salvestusruumi logid probleemi lahendamiseks.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Laadige alla ja installige sõnumi analüsaator ja Azure salvestusruumi varad

1. [Sõnumi Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) saate alla laadida Microsoft Download Center ja käivitage installer.
2. Käivitage sõnumi Analyzer.
3. Valige menüü **Tööriistad** **Varade haldur**. Dialoogiboksis **Manageri varade** valige **allalaaditavate failide**ja seejärel **Azure Storage**filtreerida. Kuvatakse Azure'i salvestusruumi varad, nagu on näidatud järgmisel pildil.
4. Klõpsake nuppu **Sünkrooni kõik kuvatavate üksuste** installida Azure salvestusruumi varad. Saadaval varad on:
    - **Azure Storage värvi reegleid:** Azure'i salvestusruumi värvi reeglid võimaldavad määratleda filtrid, mis värvi, teksti ja fondi laadide abil esile tõsta sõnumid, mis sisaldavad kindla teabe jälitus.
    - **Azure Storage diagrammid:** Azure'i salvestusruumi on eelmääratletud diagrammid, mis põhinema serveri logifailide andmed. Pange tähele, et kasutada Azure Storage diagrammide sel ajal, võite ainult alla laadida serveri logifailide analüüsi võrku.
    - **Azure Storage parserite:** Azure Storage parserite sõeluda Azure Storage klient, server ja HTTP logid selleks, et analüüsi koordinaatvõrgu kuvamine.
    - **Azure Storage filtrid:** Azure'i salvestusruumi filtrid on eelmääratletud kriteeriumid, mida saate päringu andmete analüüsi ruudustikku.
    - **Azure Storage vaade paigutused:** Azure'i salvestusruumi vaade paigutused on eelmääratletud veeru paigutust ja rühmitustega analüüsi ruudustiku.
4. Kui olete installinud vara, taaskäivitage sõnumi analüsaator.

![Sõnumi analüüsiriista varade haldur](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [AZURE.NOTE] Installige kõik näidatud selles õpetuses eesmärgil Azure Storage varad.

### <a name="import-your-log-files-into-message-analyzer"></a>Sõnumi Analyzer log failide importimine

Saate importida kõik teie salvestatud logifailid (serveripoolne kliendipoolne ja võrgu) ühe seansi rakenduses Microsoft sõnumi Analyzer analüüsi jaoks.

1. Microsoft sõnumi Analyzer menüü **fail** nuppu **Uus seanss**ja klõpsake **Tühja seanss**. Sisestage dialoogiboksis **Uus seanss** analüüsi seansi nimi. **Seansi üksikasjade** paanil, klõpsake nuppu **failid** .
1. Sõnumi analüsaator loodud võrgu Jälita andmete laadimiseks **Failide lisamiseks**klõpsake nuppu, liikuge asukohta, kuhu salvestasite faili .matp seansi web jälgimine, valige .matp fail ja klõpsake nuppu **Ava**.
1. Serveripoolne logiandmed laadimiseks **Failide lisamiseks**klõpsake nuppu, liikuge asukohta, kus laadisite oma serveripoolne logid, valige ajavahemiku, mida soovite analüüsida logifaile ja klõpsake nuppu **Ava**. Seejärel **Seansi üksikasjade** paanil määramine **Teksti Log konfigureerimine** iga serveripoolne logifaili ripploendi **AzureStorageLog** tagada Microsoft sõnumi Analyzer saab sõeluda logifaili õigesti.
1. Kliendipoolne logiandmed laadimiseks klõpsake **Failide lisamine**, liikuge asukohta, kuhu salvestasite oma kliendipoolseid logisid, valige Logi failid, mida soovite analüüsida ja klõpsake nuppu **Ava**. Seejärel **Seansi üksikasjade** paanil määramine **Teksti Log konfigureerimine** iga kliendipoolne logifaili ripploendi **AzureStorageClientDotNetV4** tagada Microsoft sõnumi Analyzer saab sõeluda logifaili õigesti.
1. **Uue seansi** dialoog laadimine ja sõeluda Logi andmed nuppu **Alustamine** . Logi andmed kuvatakse sõnumi analüsaator analüüsi ruudustikku.

Alloleval pildil on näide seansi server, klient ja võrgu jälgi logifailide konfigureeritud.

![Sõnumi analüsaator seansi konfigureerimine](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Pange tähele, et sõnum analüüsiriista laaditakse memory logifailid. Kui teil on suur hulk log andmeid, mida soovite filtreerides parima jõudluse toomine sõnumi analüsaator.

Esmalt määrata ajavahemiku, mis teile huvi üle vaadata ja selle aja jooksul võimalikult väike hoida. Paljudel juhtudel soovite üle vaadata minutit või kõige tunni jooksul. Saate importida väikseim kogum logid, mida saate vastavalt oma vajadustele.

Kui teil on endiselt logiandmed suurt hulka, siis võite määrata seansi filtri logiandmed enne laadimist. Valige väljal **Seansi Filter** **teegis** nuppu, et valida eelmääratletud filtri; Näiteks valige **globaalne aja filtreerimine ma** Azure Storage filtreerida, et filtreerida ajaintervalli. Seejärel saate redigeerida filtrikriteeriumi määrata algus- ja lõppkuupäeva ajatempli intervalli, mille soovite kuvada. Samuti saate filtreerida teatud olekukoodi; Näiteks soovite, et laadida ainult Logi kirjed, kus olek kood on 404.

Microsoft sõnumi Analyzer Logi andmete importimise kohta leiate lisateavet teemast [Sõnumi andmete toomine](http://technet.microsoft.com/library/dn772437.aspx) TechNeti.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Taotluse Kliendiid oleksid logiandmed faili abil

Azure'i salvestusruumi kliendi teek loob automaatselt kordumatu kliendi taotluse ID iga taotlus. See väärtus kirjutatakse kliendi log, serveri logifailide ja võrgujälituse, abil saate vastavusse viia andmed üle kõigi kolme logid sõnumi analüsaator sees. Vt [taotluse Kliendiid](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) kliendi taotluse kohta lisateabe ID-d.

Järgmistes jaotistes kirjeldatakse, kuidas oleksid paigutus eelnevalt konfigureeritud ja kohandatud vaadete abil ja rühma andmete põhjal taotluse kliendi ID-ga.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Vaate kuvamiseks paanil analüüsi paigutuse valimine

Sõnumi analüüsiriista salvestusruumi vara kaasata Azure'i salvestusruumi vaade paigutused, mis on eelkonfigureeritud vaated, mille abil saate kuvada oma andmeid kasulik rühmitustega ja veergudega eri stsenaariumid. Saate luua kohandatud vaate skeeme ja salvestada taaskasutuse jaoks.

Alloleval pildil on **Paigutuse** menüü, valides **Paigutuse** tööriistariba lindil saadaval. Vaate paigutused Azure Storage on rühmitatud **Azure Storage** sõlme menüü all. Saate otsida `Azure Storage` filtreerimiseks Azure Storage otsinguväljale vaadata ainult paigutused. Samuti võite märkida ruudu kõrval paigutuse lemmikuks muutmine ja selle menüü ülaosas kuvamiseks täht.

![Menüü Vaade paigutus](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Alustuseks valige **rühmitatud ClientRequestID ja mooduli**. See paigutus rühmade kuvamine logige kõigi kolme logide andmed esmalt taotluse Kliendiid, siis allika logifaili (või sõnumi analüüsiriista **mooduli** ). Selles vaates saate teatud Kliendiid taotluse süvitsiminek, ja leiate teemast andmete kõigi kolme logifailid taotluse kliendi ID.

Pildil kuvatakse see küljendivaade rakendatud valimi Logi andmete alamhulga veergu kuvada. Saate vaadata, et konkreetse kliendi taotluse ID, kuvatakse analüüsi ruudustiku kliendi log, serveri logifailide ja võrgujälituse andmed.

![Azure'i salvestusruumi paigutuse](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Erinevate logifailid on erinevate veergude, et andmeid mitmest logifailist kuvamisel analüüsi koordinaatvõrgu veerge ei tohi sisaldada mis tahes antud rea andmed. Näiteks ülaltoodud joonisel kliendi log read ei andmeid kuvada, mis tahes **ajatempli**, **TimeElapsed**, **Allika**ja **sihtkoha** veerud, kuna need veerud pole olemas kliendi Logi, kuid olemas on võrgujälituse. Samuti **ajatempli** veerus kuvatakse ajatempli andmed serveri logi, kuid andmed kuvatakse **TimeElapsed**, **lähte**- ja **sihtkoha** veerud, mis ei kuulu serveri logi.

Lisaks Azure Storage vaade paigutuste abil saate määratleda ja salvestada vaade paigutust. Saate valida muu soovitud väljad andmete rühmitamine ja salvestada rühmitamist osana ka oma kohandatud paigutus.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Analüüsi koordinaatvõrgu värvi reeglite rakendamine

Salvestusruumi varad ka värvi reeglid, mis pakuvad visuaalne tähendab erinevat tüüpi vigade analüüsi ruudustiku tuvastamiseks. Eelmääratletud värvi reeglite rakendamine HTTP tõrgete, nii, et need kuvataks ainult jaoks serveri logifailide ja võrgu jälgimine.

Reeglite värvi rakendamiseks valige tööriistaribal lindi **Värvi reeglid** . Kuvatakse menüü Azure Storage värvi reegleid. Õpetusega, valige **Kliendi tõrked (StatusCode 400 ja 499)**, nagu on näidatud järgmisel pildil.

![Azure'i salvestusruumi paigutuse](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Lisaks Azure Storage värvi õigekirjareeglite abil saate määratleda ja salvestada oma värvi reeglid.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Andmete rühmitamine ja filtri log 400-vahemiku tõrgete leidmiseks

Järgmiseks me rühma ja kõigi vigade leidmiseks 400 vahemikus Logi andmete filtreerimine.

1. Leidke **StatusCode** veeru analüüsi ruudustiku, Paremklõpsake veerupäist ja valige **rühm**.
2. Järgmise, rühma **ClientRequestId** veergu. Kuvatakse andmete analüüsi ruudustiku nüüd korraldatud oleku koodi ja kliendi taotluse ID-ga.
1. Vaatefiltri tööriista akna kuvamine, kui see pole juba kuvatud. Valige lindil tööriistariba **Tööriista Windows**, siis **Vaatefiltri**.
2. Ainult 400-vahemiku vigade kuvamiseks Logi andmete filtreerimiseks lisada järgmised filtrikriteeriumi aknas **Filter** ja klõpsake nuppu **Rakenda**.

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

Alloleval pildil on see rühmitamise ja filter. Näiteks laiendamine **ClientRequestID** välja all olek koodi 409 rühmitamise, kuvatakse tulemuseks selle olekukoodi toimingut.

![Azure'i salvestusruumi paigutuse](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Pärast selle filtri rakendamist näete, et kliendi Logi read on välja jäetud, klient ei sisalda Logi **StatusCode** veerg. Alustuseks vaatame, serveri ja võrgu Jälita logid 404 vigade leidmiseks ja seejärel saate naasta kliendi log kliendi toimingute kohta, mida need viis tutvuda.

>[AZURE.NOTE] Saate filtreerida **StatusCode** veergu ja endiselt kuvatakse kõigi kolme logid, sh kliendi log, kui lisate avaldise filter, mis sisaldab Logi kirjed, kus olek kood on tühi. See filtriavaldise koostada, kasutada.
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> See filter tagastab kõik read kliendi log ja ainult selle serveri registri ja HTTP read kui oleku kood on suurem kui 400. Kui rakendate selle paigutuse Kliendiid taotlus ja mooduli kaupa rühmitatud, otsige või liikuge kerides log kirjete otsimiseks need, kus on esindatud kõigi kolme logid.   

### <a name="filter-log-data-to-find-404-errors"></a>Andmete filtreerimine log 404 tõrgete leidmiseks

Salvestusruumi vara hulka eelmääratletud filtritega, mille abil saate piiritlemiseks logiandmed vigu või trendide otsitava leidmiseks. Järgmiseks meil kuvatakse kaks eelmääratletud filtrite rakendamine: serveri ja võrgu Jälita logid 404 vead filtreeriv ja mis filtreerib määratud ajavahemiku andmeid.

1. Vaatefiltri tööriista akna kuvamine, kui see pole juba kuvatud. Valige lindil tööriistariba **Tööriista Windows**, siis **Vaatefiltri**.
2. Vaatefiltri aknas valige **Teek**ja otsida `Azure Storage` leidmiseks Azure Storage filtrid. Valige filter **kõik logid 404 (ei leitud) sõnumite**jaoks.
3. **Teegi** menüü kuvamiseks klõpsake uuesti ja otsige ja valige **Globaalne kellaaja Filter**.
4. Redigeerige ajatemplid, näidatud filtri vahemikule, mida soovite vaadata. See aitab kitsas vahemikus andmete analüüsimiseks.
5. Filtrisse peaks kuvatama sarnane järgmises näites. Klõpsake nuppu **Rakenda** filtri rakendamiseks analüüsi ruudustikku.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Azure'i salvestusruumi paigutuse](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Logi andmete analüüsimine

Nüüd, kui teil on rühmitatud ja filtreeritud andmete, saate uurida üksikute taotlusi, mis luuakse 404 tõrked üksikasju. Praeguse vaate paigutuse andmed on rühmitatud kliendi taotluse ID, siis Logi allikas. Kuna me on filtreerimine taotluste kohta, kus StatusCode väli sisaldab 404, vaatame ainult server ja ei Logi kliendiandmete Jälita võrguandmete.

Alloleval pildil on konkreetse kus saada bloobimälu toiming andnud 404, kuna selle bloobimälu ei ole. Pange tähele, et mõned veerud on eemaldatud tavavaade selleks, et kuvada andmeid.

![Filtreeritud serveri ja võrgu Jälita logid](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Järgmiseks meil kuvatakse oleksid selle taotluse Kliendiid log kliendiandmete milliseid toiminguid kliendi võtab kui tõrke on juhtunud kuvamiseks. Saate kuvada uue analüüsi ruudustiku vaate kuvamiseks kliendi log andmeid, mis avab teise vahekaardil selle seansi jaoks:

1. Esmalt kopeerimine lõikelauale **ClientRequestId** välja väärtus. Saate seda teha, valides üks rida, asukoha **ClientRequestId** välja, paremklõpsates väärtuse, ja seejärel **Kopeeri "ClientRequestId"**.
1. Klõpsake tööriistaribal lindil **Uue vaatur**valige **Analüüsi ruudustiku** avamiseks uuel vahekaardil. Uue vahekaardi kõik andmed kuvatakse teie logifailide ilma rühmitamist, filtreerimine ja värvi reeglid.
2. Klõpsake tööriistaribal lindil **Paigutuse**valige **Kõik .net-i kliendi veerud** jaotises **Azure Storage** . Selle paigutuse kuvatakse andmed kliendi log kui ka serveri ja võrgu Jälita logid. Vaikimisi on see sorditud **MessageNumber** veergu.
3. Järgmiseks otsida kliendi Logi taotlus kliendi ID-ga. Klõpsake tööriistaribal lindil valige **Sõnumite otsimine**ja seejärel määrata kohandatud filtri taotluse Kliendiid väljale **otsimine** . Kasutage filter, täpsustades oma taotluse Kliendiid järgmist süntaksit:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Sõnumi analüsaator otsib ja valib esimese Logi kirje kui otsingukriteeriumid vastab taotlus kliendi ID-ga. Kliendi Logi on mitu iga kliendi taotluse ID kirjed nii, et soovite rühmitada **ClientRequestId** välja, et oleks lihtsam näha neid kõik koos. Alloleval pildil on kõik sõnumid kliendi Logi määratud kliendi jaoks taotluse ID-ga.

![Kliendi log kuvav 404 vead](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Nende kahe vahekaartide kuvamine paigutused kuvatavad andmed saate analüüsida taotluse andmed määramiseks, mis võivad põhjustada tõrke. Saate vaadata ka taotlusi, mille ees on see üks kuvamiseks, kui eelmise sündmuse on viinud tõrge nr 404. Näiteks saate vaadata enne selle kliendi taotluse ID, et määratleda, kas selle bloobimälu kustutanud või kui tõrge tõttu helistamiseks CreateIfNotExists API container või bloobimälu klientrakenduse kliendi Logi kirjed. Kliendi Logi, leiate selle bloobimälu aadress väljale **Kirjeldus** ; server ja võrgu Jälita logid see teave kuvatakse väljal **Kokkuvõte** .

Kui teate, mis andis tõrge nr 404 bloobimälu aadressi, saate uurida edasi. Kui otsite muid sõnumeid, mis on seotud toiminguid sama bloobimälu log kirjeid, saate kontrollida, kas kliendi varem kustutatud üksus.

## <a name="analyze-other-types-of-storage-errors"></a>Muud tüüpi salvestusruumi vigade analüüs

Nüüd, kui olete tuttav sõnumi Analyzer abil Logi andmete analüüsimine, saate analüüsida muud tüüpi vigade vaate kasutamine paigutused, värvi reeglid ja otsingu ja filtreerimise. Järgmistes tabelites on loetletud mõned probleemid, mis võivad ilmneda ja filtrikriteeriumi, saate nende otsimiseks. Ehitamine filtrid ja sõnumi analüsaator keele filtreerimise kohta leiate lisateavet teemast [Sõnumi andmete filtreerimine](http://technet.microsoft.com/library/jj819365.aspx).

|    Kui soovite uurida...                                                                                               |    Filtri avaldisel kasutamine...                                                                                                                                                                                                                                        |    Avaldis, mis kehtib Log (klient, Server, võrk, kõik)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Ootamatud viivitused sõnumi kohaletoimetamise järjekord                                                              |    AzureStorageClientDotNetV4.Description sisaldab "Retrying nurjus toimingu."                                                                                                                                                                                |    Kliendi                                                        |
|    HTTP PercentThrottlingError suurendamine                                                                       |    HTTP. Response.StatusCode == 500 & #124; & #124; HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Võrgu                                                       |
|    PercentTimeoutError suurendamine                                                                               |    HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Võrgu                                                       |
|    Suurendamine PercentTimeoutError (kõik)                                                                         |    * StatusCode == 500                                                                                                                                                                                                                                          |    Kõik                                                           |
|    PercentNetworkError suurendamine                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Kliendi                                                        |
|    HTTP 403 (keelatud) sõnumid                                                                                 |    HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Võrgu                                                       |
|    HTTP 404 (ei leitud) sõnumid                                                                                 |    HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Võrgu                                                       |
|    404 (kõik)                                                                                                     |    * StatusCode == 404                                                                                                                                                                                                                                          |    Kõik                                                           |
|    Ühiskasutusse antud juurdepääs allkirja (SAS) autoriseerimine probleemi                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Võrgu                                                       |
|    HTTP 409 (konflikt) sõnumid                                                                                  |    HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Võrgu                                                       |
|    409 (kõik)                                                                                                     |    * StatusCode == 409                                                                                                                                                                                                                                          |    Kõik                                                           |
|    Madal PercentSuccess või analytics Logi kirjed on toimingute olek ClientOtherErrors.    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Server                                                        |
|    Nagle hoiatus                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1,5)) ja (AzureStorageLog.RequestPacketSize < 1460) ja (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Server                                                        |
|    Lahtrivahemik, kellaaja-serveri ja võrgu logid                                                                    |    #Ajatempli > = 2014-10-20T16:36:38 ja #Timestamp < = 2014-10-20T16:36:39                                                                                                                                                                                     |    Server Network                                               |
|    Vahemiku aega logid                                                                                |    AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 ja AzureStorageLog.Timestamp < = 2014-10-20T16:36:39                                                                                                                                                     |    Server                                                        |


## <a name="next-steps"></a>Järgmised sammud

Tõrkeotsingu lõpuni stsenaariumid Azure Storage kohta lisateabe saamiseks vaadake neid materjale:

- [Jälgimine, diagnoosimine ja tõrkeotsing Microsoft Azure'i Tabelimäluga](storage-monitoring-diagnosing-troubleshooting.md)
- [Salvestusruumi Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [Salvestusruumi konto Azure'i portaalis jälgimine](storage-monitor-storage-account.md)
- [AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)
- [Microsoft sõnumi analüsaator opsüsteem juhend](http://technet.microsoft.com/library/jj649776.aspx)
