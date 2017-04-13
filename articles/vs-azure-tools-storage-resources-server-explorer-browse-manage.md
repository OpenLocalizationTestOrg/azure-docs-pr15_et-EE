<properties
   pageTitle="Sirvimine ja salvestusruumi ressursside serveri Exploreriga | Microsoft Azure'i"
   description="Sirvimine ja salvestusruumi ressursside serveri Exploreriga haldamine"
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

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Sirvimine ja salvestusruumi ressursside serveri Exploreriga haldamine

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Ülevaade
Kui olete installinud Microsoft Visual Studio Azure'i tööriistad, saate vaadata bloobimälu, kuhjuda ja tabeliandmete kontodelt salvestusruumi Azure. Sõlme Azure Storage Server Explorer kuvatakse andmed, mis on teie kohalikku emulaator konto ja teie Azure salvestusruumi kontod.

Visual Studio menüü Server Explorer kuvamiseks valige **Kuva** **Server Explorer**. Salvestusruumi sõlm kuvatakse kõigi salvestusruumi kontod, mis on olemas iga Azure tellimuse/serti soovite ühendust luua, klõpsake jaotises. Kui teie konto salvestusruumi ei kuvata, saate selle lisada, järgides juhiseid [käesoleva teema](#add-storage-accounts-by-using-server-explorer).

Alates Azure'i SDK 2.7, saate uute Cloud Explorer vaadata ja hallata oma Azure ressursse. Lisateabe saamiseks vaadake [Haldamise Azure'i ressursid Cloud Exploreriga](./vs-azure-tools-resources-managing-with-cloud-explorer.md) .


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Saate vaadata ja hallata salvestusruumi ressursid Visual Studio

Server Explorer kuvatakse automaatselt teie salvestusruumi emulaator konto plekid, järjekorrad ja tabelite loendi. Salvestusruumi emulaator konto kuvatakse kujul **arengu** sõlm Server Explorer salvestusruumi sõlme all.

Salvestusruumi emulaator konto ressursside vaatamiseks laiendage **areng** . Kui salvestusruumi emulaator pole alustatud kui laiendage **arengu** , see käivitub automaatselt. See võib kuluda mitu minutit. Töötada muude alade Visual Studio samal ajal emulaator salvestusruum hakkab saate jätkata.

Salvestusruumi konto ressursside vaatamiseks laiendage salvestusruumi konto Server Explorer. Kuvatakse järgmised sub sõlmed:

- Plekid

- Järjekorrad

- Tabelid

## <a name="work-with-blob-resources"></a>Töö ressurssidega bloobimälu

Plekid sõlm kuvatakse valitud salvestusruumi konto ümbriste loend. Bloobimälu ümbriste sisaldavad bloobimälu failide ja nende plekid saate korraldada kaustadesse ja alamkaustad. Lisateabe saamiseks vaadake [bloobimälu kaudu .net-i kasutamise kohta](./storage/storage-dotnet-how-to-use-blobs.md) .

### <a name="to-create-a-blob-container"></a>Bloobimälu container loomiseks

1. **Plekid** sõlme kiirmenüü avamine ja valige **Loomine bloobimälu ümbrises**.

1. Sisestage nimi uue container **Loomine bloobimälu Container** dialoogiboksis ja seejärel valige **Ok**

    >[AZURE.NOTE] Container bloobimälu nimi peab algama väiketähtedes täht (a – z) või number (0 – 9).

### <a name="to-delete-a-blob-container"></a>Bloobimälu container kustutamine

- Avage kiirmenüü bloobimälu s.o ümbris, mille soovite eemaldada, ja seejärel valige **Kustuta**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Bloobimälu container leiduvate üksuste loendi kuvamiseks

- Avage kiirmenüü bloobimälu container nime loendis ja seejärel valige **Vaate bloobimälu ümbrises**.

    Kui vaatate bloobimälu container sisu, kuvatakse see tuntud bloobimälu container vaate vahekaardil.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    Ülemises parempoolses nurgas bloobimälu ümbrise vaade nuppude abil saate teha plekid järgmised toimingud:

    - Sisestage väärtus filtri ja rakendada selle

    - Plekid ümbrises loendi värskendamine

    - Faili üleslaadimine

    - Kustutamine on bloobimälu

      >[AZURE.NOTE] Faili kustutamine bloobimälu container ei kustutata aluseks oleva faili; See eemaldab ainult selle bloobimälu ümbrises.

    - Avage soovitud bloobimälu

    - Salvestage soovitud bloobimälu kohalikku arvutisse

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Kausta või alamkausta loomiseks bloobimälu ümbrises

1. Valige bloobimälu container Server Explorer. Container aknas nuppu **Bloobimälu üles laadida** .

    ![Faili üleslaadimine bloobimälu kausta](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. Dialoogiboksis **Uue faili üleslaadimine** valimine nuppu **Sirvi** , et määrata fail, mille soovite üles laadida, ja seejärel sisestage kausta nimi väljale **kausta (valikuline)** .

    Sama toimingut järgides saate lisada container kaustade alamkaustad. Kui te ei määra kausta nime, laaditakse fail bloobimälu container kõrgeima taseme. Fail kuvatakse ümbrises määratud kausta.

    ![Lisatud bloobimälu container kausta](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Topeltklõpsake kausta või vajutage klahvi ENTER, et näha kausta sisu. Kui olete kõigepealt ümbrist kaustas, saate liikuda tagasi ümbris juurkaustas valides nupu **Ava vanema Directory** (ülesnool).

### <a name="to-delete-a-container-folder"></a>Container kausta kustutamine

 - Kustutage kõik failid kaustast

    >[AZURE.NOTE] Kuna kaustade bloobimälu ümbriste on Virtuaalkaustad, ei saa luua tühja kausta ega saate kustutada selle faili sisu kausta kustutamine. Teil on kaust, kuhu soovite kausta kustutada kogu sisu kustutamine.

### <a name="to-filter-blobs-in-a-container"></a>Filtreerimiseks on ümbrises plekid

Saate filtreerida plekid, mis on kuvatud, määrates levinud eesliite.

Näiteks kui sisestate eesliite `hello` filter tekst väljale ja seejärel valige **käivitamine** (****!) nupp, kuvatakse ainult plekid, mis algavad sõnadega "Tere".

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] Välja filter on tõstutundlikud ja ei toeta filtreerimise metamärkidega. Plekid filtreerida ainult eesliite. Eesliite võivad sisaldada eraldaja, kui kasutate eraldaja korraldamiseks plekid virtuaalse hierarhias. Näiteks eesliite HelloFabric filtreerimine / tagastab kõik selle stringi alustades plekid.

### <a name="to-download-blob-data"></a>Kui soovite bloobimälu andmete allalaadimine

- **Server Explorer**ühe või mitme plekid kiirmenüü avamine ja seejärel **avada**, valige või bloobimälu nimi ja seejärel klõpsake nuppu **Ava** või topeltklõpsake bloobimälu nime.

    Bloobimälu allalaadimise edenemist kuvatakse akna **Azure'i tegevuste Logi** .

    Funktsiooni bloobimälu avaneb vaikimisi redaktoris selle faili jaoks. Kui operatsioonisüsteem tuvastab failitüüp, fail avatakse kohalikult rakenduse; muul juhul teil palutakse valida rakendus, mis sobib faili tüüp soovitud bloobimälu. Kohalik fail, mis on loodud mõne bloobimälu allalaadimisel on märgitud kirjutuskaitstuks.

    Bloobimälu andmete vahemällu talletatud kohalik ja kontrollida selle bloobimälu viimase muutmise aja bloobimälu teenus. Kui soovitud bloobimälu on pärast viimast allalaadimist värskendatud, tuleb see laaditakse uuesti; muul juhul laaditud soovitud bloobimälu kohalikule kettale. Vaikimisi on bloobimälu laaditakse ajutist kataloogi. Teatud kataloogi plekid allalaadimiseks valitud bloobimälu nimede kiirmenüü avamine ja klõpsake nuppu **Salvesta nimega**. Sel viisil on bloobimälu salvestamisel bloobimälu fail on avatud ja kohalik fail on loodud lugemis-ja kirjutamisõigusega atribuutidega.

### <a name="to-upload-blobs"></a>Üles laadida plekid

- Klõpsake nuppu **Laadida bloobimälu** kui ümbris on avatud bloobimälu container vaate kuvamine.

    Saate valida ühe või mitme faili üles laadida, ja te saate mis tahes tüüpi faile üles laadida. **Azure'i tegevuse Log** kuvatakse edenemise üles. Bloobimälu andmetega töötamise kohta lisateabe saamiseks vaadake, [Kuidas kasutada .NET Azure'i bloobimälu salvestusteenus](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Logide plekid üle vaadata

- Kui kasutate Azure diagnostika logige oma Azure rakendusest andmete ning olete logid salvestusruumi kontole, näete ümbriste, mis on loodud Azure'i need logid. Vaatamise need logid Server Explorer on lihtne viis oma rakenduse probleemide tuvastamiseks, eriti siis, kui see on juurutatud Azure. Azure'i diagnostika kohta leiate lisateavet teemast [Logimine andmete kogumise Azure'i diagnostika abil](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>URL-i hankimiseks on bloobimälu

- Avage soovitud bloobimälu kiirmenüü ja valige **Kopeeri URL-i**.

### <a name="to-edit-a-blob"></a>Redigeerimiseks on bloobimälu

- Valige soovitud bloobimälu ja seejärel klõpsake nuppu **Ava bloobimälu** .

    Fail on alla laaditud ajutine asukohta ja kohalikus arvutis avada. Pärast muudatuste, peab üles soovitud bloobimälu.

## <a name="work-with-queue-resources"></a>Töö ressurssidega järjekord

Salvestusruumi teenuste järjekorrad majutatakse Azure storage konto ja saate neid kasutada lubama oma pilvepõhise teenuse rollid suhelda omavahel ja muude teenustega sõnumile, läbides süsteem. Pääsete kuhjuda programmiliselt kaudu pilveteenus ja üle veebiteenuse välistele klientidele. Otse Visual Studio serveri Exploreri abil pääsete juurde ka järjekorda.

Kui teil tekib pilveteenuses, mis kasutab järjekorrad, mida soovite Visual Studio abil saate luua järjekorrad ja nendega töötada interaktiivseks samal ajal, kui teil tekib ja testige oma kood.

Server Explorer, saate vaadata järjekorrad salvestusruumi konto, luua ja kustutada järjekorrad, järjekorda oma sõnumite kuvamiseks avage, ja sõnumite järjekorda lisada. Kui avate järjekorda vaatamine, saate vaadata üksikuid sõnumeid ja saate teha järgmisi toiminguid järjekorda, kasutades nuppe ülemises vasakus nurgas:

- Värskendada vaadet järjekorra

- Sõnumi järjekorda lisamine

- Dequeue pealmine sõnum.

- Tühjendage kogu järjekord

Järgmisel pildil on kujutatud järjekorras, mis sisaldab kahte sõnumeid.

![Järjekorda vaatamine](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Lisateavet mäluruumi teenuste järjekorrad, lugege teemat [kohta: kasutada järjekorda salvestusteenus](http://go.microsoft.com/fwlink/?LinkID=264702). Veebiteenuse järjekorrad salvestusruumi teenuste kohta leiate teavet teemast [Järjekorda teenuse põhimõtet](http://go.microsoft.com/fwlink/?LinkId=264788). Lisateavet sõnumeid saata salvestusruumi teenuste järjekorda Visual Studio abil leiate teemast [Salvestusruumi teenuste järjekorda sõnumeid saata](https://msdn.microsoft.com/library/azure/jj649344.aspx).

>[AZURE.NOTE] Salvestusruumi teenuste järjekorrad erinevad teenuse siini järjekorrad. Teenuse siini järjekorrad kohta leiate lisateavet teemast teenuse siini järjekorrad, teemasid ja tellimused.

## <a name="work-with-table-resources"></a>Töö ressurssidega tabel

Azure'i tabeli salvestusteenus talletab suurte andmehulkade liigendatud. Teenus on NoSQL andmesalve, mida aktsepteerib autenditud kõnede ja sellest väljaspool Azure pilve. Azure'i tabelid on optimaalne struktureeritud, mitte relatsiooniliste andmete talletamiseks.

### <a name="to-create-a-table"></a>Tabeli loomiseks

1. Server Explorer, valige konto salvestusruumi **tabelite** sõlm ja valige **Tabeli loomine**.

1. Sisestage dialoogiboksi **Tabeli loomine** tabeli nimi.

### <a name="to-view-table-data"></a>Tabeli andmete kuvamiseks

1. Server Explorer, avage **Azure'i** sõlm ja avage **salvestusruumi** sõlm.

1. Avage salvestusruumi konto sõlm, mida olete huvitatud, ja seejärel **tabelite** sõlm salvestusruumi konto tabelite loendi kuvamiseks.

1. Tabeli kiirmenüü avamine ja seejärel valige **Kuva tabel**.

    ![Saate Azure tabelis Solution Exploreris](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Tabel on korraldatud üksuste (näidatud read) ja atribuudid (näidatud veergude). Näiteks järgmisel joonisel on loetletud **Tabeli kujundaja**:

### <a name="to-edit-table-data"></a>Tabeli andmete redigeerimiseks

1. **Tabeli Designer**üksus (üks rida) või atribuut (ühest lahtrist) kiirmenüü avamine ja seejärel valige **Redigeeri**.

    ![Lisamine või redigeerimine tabelis üksus](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Üksuste üheks tabeliks ei pea olema sama valiku atribuute (veerge). Pidage meeles on järgmised piirangud tabeli andmete vaatamiseks ja redigeerimiseks.
    - Te ei saa vaadata või redigeerida binaarandmeid (byte[]) tüüp, kuid te saate salvestada selle tabeli.

    - **PartitionKey** või **RowKey** väärtused, ei saa redigeerida, kuna Azure tabelimäluga ei toeta seda toimingut.

    - Te ei saa luua atribuudi ajatempli, Azure Storage teenuseid kasutada atribuudi nime.

    - Kui sisestate väärtus DateTime, peaksite järgima vormingut, mida on vaja oma arvuti piirkond ja keel sätete (nt KK/PP/AAAA HH:MM:SS [AM | PL] USA inglise keeles).

### <a name="to-add-entities"></a>Üksuste lisamine

1. **Tabeli Designer**, klõpsake nuppu **Lisa üksus** , asuvad paremas ülanurgas tabeli vaate.

    ![Üksuse lisamine](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. Sisestage dialoogiboksis **Lisa üksus** **PartitionKey** ja **RowKey** atribuute.

    ![Dialoogiboks üksuse lisamine](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Sisestage soovitud väärtused hoolikalt, kuna neid ei saa muuta, kui dialoogiboksi sulgemiseks, v.a juhul, kui kustutate üksus ja lisage see uuesti.

### <a name="to-filter-entities"></a>Üksuste filtreerimiseks

Saate kohandada üksused, mis kuvatakse tabeli Päringukoosturi kasutamisel määramine.

1. Päringukoosturi avamiseks avage tabeli kuvamine.

1. Klõpsake nuppu parempoolseimad tööriistaribal tabeli kuvamine.

    Kuvatakse dialoogiboks **Päringukoostur** . Järgmisel pildil kuvatakse päring, mis on ehitatud Päringukoostur.

    ![Päringukoostur](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Kui olete lõpetanud, päringu koostamise sulgege dialoogiboks. Päringu teksti vormi kuvatakse tekstivälja WCF Data Services filtrina.

1. Päringu käivitamiseks valige roheline kolmnurk ikoon.

    Saate filtreerida ka olemi andmeid, mis kuvatakse **Tabeli Designer** , kui sisestate WCF Data Services filter string väljale filter. Seda tüüpi string on sarnane on SQL-i WHERE-klausel, kuid ei saadeta serveri HTTP-päring. Ehitada stringide filtri kohta leiate artiklist [Ehitamine Filter stringide tabeli Designer](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    Järgmisel joonisel on kujutatud sobiv filter stringi:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Salvestusruumi andmete värskendamine

Kui Server Explorer loob ühenduse või hangib salvestusruumi konto andmeid, võib kuluda kuni minutiks toimingu lõpuleviimiseks. Kui seda ei saa ühendust luua, võite selle toimingu ajalõpuni. Kui andmed laaditud, saate jätkata Visual Studio mujal. Toimingu tühistamine, kui see võtab liiga kaua aega, valige serveri Exploreri tööriistaribal nuppu **Lõpeta Värskenda** .

#### <a name="to-refresh-blob-container-data"></a>Bloobimälu container andmete värskendamine

- Valige **plekid** sõlm all **salvestusruumi** ja serveri Exploreri tööriistaribal nuppu **Värskenda** .

- Värskendamine plekid loend, mis kuvatakse, klõpsake nuppu **Käivita** .

#### <a name="to-refresh-table-data"></a>Tabeli andmete värskendamine

- Valige **tabelid** sõlm all **salvestusruumi** ja seejärel nuppu **Värskenda** .

- Üksuste loend, mis kuvatakse **Tabeli Designer**värskendamiseks nuppu **Käivita** **Tabeli kujundaja**kohta.

#### <a name="to-refresh-queue-data"></a>Järjekorda andmete värskendamine

- Valige **järjekorrad** sõlm ja seejärel klõpsake nuppu **Värskenda** .

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Kõigi üksuste salvestusruumi konto värskendamine

- Valige konto nimi ja valige serveri Exploreri tööriistaribal nuppu **Värskenda** .

### <a name="add-storage-accounts-by-using-server-explorer"></a>Salvestusruumi kontosid lisada serveri Exploreri kaudu

On kaks võimalust lisada salvestusruumi kontod serveri Exploreri kaudu. Saate luua uue konto salvestusruumi Azure tellimuse või olemasoleva salvestusruumi konto saate manustada.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Mäluruumi uue konto loomiseks serveri Exploreri kaudu

1. Serveri Exploreris salvestusruumi sõlme kiirmenüü avamine ja valige salvestusruumi konto luua.

    ![Azure'i salvestusruumi uue konto loomine](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Valige või sisestage järgmine teave salvestusruumi uue konto **Loomine salvestusruumi konto** dialoogiboksis.

    - Azure'i tellimus, millele soovite lisada salvestusruumi konto.

    - Uue salvestusruumi konto jaoks soovitud nimi.

    - Piirkonna või osaleja rühma (nt Lääne USA või Ida-Aasia).

    - Tüüp, mida soovite kasutada konto salvestusruumi, nt geograafilise liigne dispersioonanalüüs.

1. Valige **Loo**.

    Uue salvestusruumi konto kuvatakse loendis **salvestusruumi** Solution Exploreris.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Olemasoleva salvestusruumi konto lisada serveri Exploreri kaudu

1. Server Explorer, Azure storage sõlme kiirmenüü avamine ja valige **Manusta välise salvestusruumi**.

    ![Olemasoleva salvestusruumi konto lisamine](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Valige või sisestage järgmine teave salvestusruumi uue konto **Loomine salvestusruumi konto** dialoogiboksis.

    - Olemasoleva salvestusruumi konto nimi, mida soovite manustada. Saate sisestada nimi või valige see loendist.

    - Valitud salvestusruumi konto võti. See väärtus on tavaliselt antakse teile salvestusruumi konto valimisel. Kui soovite Visual Studio salvestusruumi konto võti meeles pidada, ruut Jäta konto võti.

    - Protokoll salvestusruumi konto, nt HTTP, HTTPS või kohandatud lõpp-punkti loomiseks kasutada. Vaadake, [Kuidas konfigureerida ühendusstringi](https://msdn.microsoft.com/library/azure/ee758697.aspx) Lisateavet kohandatud lõpp-punktid.

### <a name="to-view-the-secondary-endpoints"></a>Sekundaarse kuvamiseks

- Kui olete loonud salvestusruumi konto, **Lugemisõigus – Geo liigsete** dispersioonanalüüs suvandi abil, saate vaadata oma teisene lõpp-punktid. Avage otseteemenüü konto nime ja seejärel valige **Atribuudid**.

    ![Sekundaarse salvestusruum](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Server Explorer salvestusruumi konto eemaldamine

- Server Explorer, avage otseteemenüü konto nime ja valige **Kustuta**. Kui kustutate salvestusruumi konto, eemaldatakse ka kõik salvestatud põhiteave konto jaoks.

    >[AZURE.NOTE] Kui kustutate salvestusruumi konto Server Explorer, see ei mõjuta teie salvestusruumi konto või andmeid, mida see sisaldab; See eemaldab lihtsalt viide Server Explorer. Salvestusruumi konto jäädavalt kustutamiseks kasutada [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).

## <a name="next-steps"></a>Järgmised sammud

Lisateavet selle kohta, kuidas kasutada Azure storage teenuseid leiate teemast [Azure salvestusruumi teenuste kasutamiseks](https://msdn.microsoft.com/library/azure/ee405490.aspx).
