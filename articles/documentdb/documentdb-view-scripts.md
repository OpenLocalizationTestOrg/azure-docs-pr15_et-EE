<properties
    pageTitle="JavaScripti redaktor DocumentDB skripti Exploreris | Microsoft Azure'i"
    description="Lisateavet Exploreris DocumentDB skripti Azure portaali aitab hallata DocumentDB serveripoolne programmeerimise esemeid, sealhulgas salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid."
    keywords="JavaScripti redaktor"
    services="documentdb"
    authors="kirillg"
    manager="jhubbard"
    editor="monicar"
    documentationCenter=""/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="kirillg"/>

# <a name="create-and-run-stored-procedures-triggers-and-user-defined-functions-using-the-documentdb-script-explorer"></a>Loomine ja käivitamine salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid DocumentDB skripti Explorerit

Selles artiklis antakse ülevaade [Microsoft Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) skripti Exploreris, mis on JavaScripti redaktor Azure'i portaalis, mis võimaldab teil vaadata ja käivitada DocumentDB serveripoolne programmeerimise esemeid, sealhulgas salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid. Lisateavet leiate DocumentDB serveripoolne programmeerimise [Salvestatud toimingute ja andmebaasi päästikute, leiate ülevaate](documentdb-programming.md) artikli kohta.

## <a name="launch-script-explorer"></a>Käivitage skripti Explorer

1. Klõpsake Azure portaali, Jumpbar, **DocumentDB (NoSQL)**. Kui **DocumentDB kontod** ei ole nähtav, klõpsake nuppu **Rohkem teenuseid** ja klõpsake **DocumentDB (NoSQL)**.

2. Klõpsake menüüs ressursid **Skripti Explorer**.

    ![Käsk skripti Exploreri kuvatõmmis](./media/documentdb-view-scripts/scriptexplorercommand.png)
 
    Ripploendi väljad **andmebaasi** ja **saidikogumi** eelasustatud vastavalt kontekstile, kus käivitamisel skripti Explorer.  Näiteks kui käivitate andmebaasi tera kaudu, siis praeguse andmebaasi on juba eelnevalt täidetud.  Kui käivitate saidikogumi tera kaudu, siis praeguse saidikogumi on juba eelnevalt täidetud.

4.  Ripploendi väljad **andmebaasi** ja **saidikogumi** abil saate hõlpsalt muuta, kust on praegu skriptide vaadata ilma sulgeda ja uuesti käivitada skripti Explorer.  

5. Skripti Explorer toetab samuti filtreerimine skriptide praegu laaditud määramine nende id atribuut.  Lihtsalt tippige väljale filtri ja tulemused skripti Exploreri loendis on filtreeritud oma esitatud kriteeriumide alusel.

    ![Exploreri filtreeritud tulemustega skripti kuvatõmmis](./media/documentdb-view-scripts/scriptexplorerfilterresults.png)


    > [AZURE.IMPORTANT] Skripti Exploreri filtreerimine funktsioonid ainult filtrid ***praegu*** laaditakse põhinevad skripte ja automaatne värskendamine praegu valitud saidikogumi.

5. Skriptide skript Exploreriga laadida loendi värskendamiseks klõpsake lihtsalt tera ülaosas käsk **Värskenda** .

    ![Skripti Exploreri kuvatõmmis käsk Värskenda](./media/documentdb-view-scripts/scriptexplorerrefresh.png)


## <a name="create-view-and-edit-stored-procedures-triggers-and-user-defined-functions"></a>Luua, vaadata ja redigeerida salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid

Skripti Explorer võimaldab teil hõlpsasti toiminguid CRUD DocumentDB serveripoolne programmeerimise esemeid.  

- Luua skripti, klõpsa kohaldatavate luua skripti Exploreri käsu, pakuvad id, sisestage skripti sisu ja klõpsake nuppu **Salvesta**.

    ![Skripti Exploreri kuvatõmmis loomine suvand nähtaval JavaScripti redaktor](./media/documentdb-view-scripts/scriptexplorercreatecommand.png)

- Kui loote käivitamiseks, peate määrama toimingu tüüp ja päästik päästik

    ![Skripti Exploreri kuvatõmmis loomine päästik suvand](./media/documentdb-view-scripts/scriptexplorercreatetrigger.png)

- Skripti vaatamiseks klõpsake lihtsalt skripti, mis teile huvi.

    ![Skripti Exploreri kuvatõmmis vaade skript kogemus](./media/documentdb-view-scripts/scriptexplorerviewscript.png)

- Skripti redigeerimiseks lihtsalt editor JavaScript tehke soovitud muudatused ja klõpsake nuppu **Salvesta**.

    ![Skripti Exploreri kuvatõmmis vaade skripti kogemus](./media/documentdb-view-scripts/scriptexplorereditscript.png)

- Skripti Ootel muudatuste hülgamiseks lihtsalt käsku **Hülga** .

    ![Skripti Exploreri kuvatõmmis hülga muutused kogemus](./media/documentdb-view-scripts/scriptexplorerdiscardchanges.png)

- Skripti Explorer võimaldab teil vaadata Süsteemiatribuudid praegu laaditud skripti, klõpsates käsku **Atribuudid** .

    ![Skripti Exploreri kuvatõmmis skripti atribuutide vaatamine](./media/documentdb-view-scripts/scriptproperties.png)

    > [AZURE.NOTE] Atribuudi ajatempli (_ts) on esindatud ettevõttesiseselt epohhi aeg, kuid skripti Explorer kuvab väärtuse GMT inimeste loetavas vormingus.

- Skripti kustutamiseks valige see Script Exploreris ja klõpsake käsku **Kustuta** .

    ![Käsk Kustuta Script Exploreri kuvatõmmis](./media/documentdb-view-scripts/scriptexplorerdeletescript1.png)

- Kustutustoiming kinnitada, klõpsates nuppu **Jah** või Kustuta toimingu tühistamine, klõpsates nuppu **ei**.

    ![Käsk Kustuta Script Exploreri kuvatõmmis](./media/documentdb-view-scripts/scriptexplorerdeletescript2.png)

## <a name="execute-a-stored-procedure"></a>Käivitada salvestatud protseduur

> [AZURE.WARNING] Salvestatud toimingute käivitamisel skripti Exploreris veel ei toetata serveri pool liigendatud saidikogumid. Lisateabe saamiseks külastage [Eraldatav ja mastaapimine DocumentDB sisse](documentdb-partition-data.md).

Skripti Explorer võimaldab teil käivitada serveripoolne salvestatud toimingute Azure portaalist.

- Uue Loo salvestatud protseduur blade avamisel vaikimisi script (*eesliide*) juba andnud. *ID-d* ja *sisendina*lisamiseks *eesliite* skripti või oma skripti käivitamiseks. Salvestatud toimingute aktsepteerida mitut parameetrite, peab olema kõik sisendina massiivi (nt *["foo", "lint"]*).

    ![Kuvatõmmis, skripti Exploreri salvestatud toimingute blade sisendit lisada ja käivitada salvestatud protseduur](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure-input.png)

- Salvestatud toimingu käivitada, klõpsake käsku **Salvesta ja käivitada** script editor paanil.

    > [AZURE.NOTE] Käsu **Salvesta ja käivitada** salvestab teie salvestatud protseduur enne, mis tähendab, et see kirjutab salvestatud protseduur varem salvestatud versiooni.

- Eduka salvestatud protseduur täitmised on *nüüd salvestatud ja täidetud salvestatud protseduur* olek ja tagastatud tulemite olema täidetud järgmised *tulemuste* paanil.

    ![Kuvatõmmis, skripti Exploreri salvestatud toimingute tera, käivitada salvestatud protseduur](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure.png)

- Kui täitmise tõrge, kuvatakse tõrge kasutajanimedega *tulemuste* paanil.

    ![Skripti Exploreri kuvatõmmis skripti atribuutide vaatamine Salvestatud protseduur vigadega käivitada](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure-error.png)

## <a name="work-with-scripts-outside-the-portal"></a>Skriptide väljaspool portaali töötamine

Skripti Exploreri Azure'i portaalis on ainult üks viis, kuidas salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonide DocumentDB töötamine. Skriptide abil saate töötada ka selle REST API-ga ja [Kliendi SDK-d](documentdb-sdk-dotnet.md). REST API dokumentatsiooni sisaldab näidised [Salvestatud toimingute abil ülejäänud](https://msdn.microsoft.com/library/azure/mt489092.aspx), [kasutaja määratletud funktsioonid, kasutades ülejäänud](https://msdn.microsoft.com/library/azure/dn781481.aspx)ja [päästikute kasutamine ülejäänud](https://msdn.microsoft.com/library/azure/mt489116.aspx)töötamiseks. Näidised on ka saadaval näitab, kuidas [töötada skriptide abil C#](documentdb-dotnet-samples.md#server-side-programming-examples) ja [skriptide abil Node.js töötamine](documentdb-nodejs-samples.md#server-side-programming-examples).

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet DocumentDB serveripoolne programmeerimise [Salvestatud toimingute ja andmebaasi päästikute, leiate ülevaate](documentdb-programming.md) artiklist.

[Õppeteema](https://azure.microsoft.com/documentation/learning-paths/documentdb/) on ka kasulikke ressursi juhendab teid, kui te DocumentDB kohta rohkem teada.  
