<properties
    pageTitle="Alustamine Azure'i voo Kasutusanalüüsi töötlemise asjade seadmetest. | Microsoft Azure'i"
    description="Asjade andur sildid ja andmevoogu voo Kasutusanalüüsi ja reaalajas andmetöötlus"
    keywords="asjade lahenduse asjade kasutamise alustamine"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Alustamine Azure'i voo Kasutusanalüüsi töötlemise asjade seadmetes

Selles õppetükis saate teada, kuidas voo töötlemiseks loogika andmete kogumine Internet, asjade seadmete loomiseks. Tegelike, Internet, asjade kasutamine juhtumi kasutame näitavad, kuidas luua oma lahenduse kiiresti ja majanduslikult.

## <a name="prerequisites"></a>Eeltingimused

-   [Azure'i tellimus](https://azure.microsoft.com/pricing/free-trial/)
-   Päringu ja näidisfailide [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) alla laadida

## <a name="scenario"></a>Stsenaarium

Contoso, mis on mõni ettevõte tööstusliku automatiseerimise ruumi, on täielikult automatiseeritud oma tootmisprotsessi. Masina selles ettevõttes on andurid, mis võivad tekitavate voole andmete reaalajas. Selle stsenaariumi korral on floor Tootmisjuht soovib reaalajas teadmisi andurite andmeid otsida mustrite ja nende toimingute tegemiseks. Kasutame voo Analytics päringu keele (SAQL) üle andurite andmeid huvitav mustrid sissetuleva voo andmete otsimiseks.

Siin on andmed loodud Texases vahendite andur sildi seadme kaudu.

![Texases vahendite andur silt](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Last andmed on JSON-vormingus ja näeb välja umbes järgmine:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

Tegelike stsenaariumi võib teil olla sadu need andurid genereerimine sündmuste voo nimega. Ideaalvariandis mitte oleks lüüsi seadme lükake need sündmused [Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) või [Azure asjade jaoturi](https://azure.microsoft.com/services/iot-hub/)koodi käivitada. Voo Analytics tööpäevad oleks neelata need sündmused sündmus jaoturi kaudu ja reaalajas analytics päringuid käivitada soovitud voogu vastu. Seejärel võib saata tulemused ühele [väljundeid toetatud](stream-analytics-define-outputs.md).

Kasutuslihtsus, saamisega alustamise juhendi pakub valimi andmete faili, mis salvestati reaal andur sildi seadmetest. Saate teha päringuid Näidisandmete ja tulemuste vaatamiseks. Edaspidised õpetustes teada, kuidas luua ühendus oma töö sisendina ja väljundid ja juurutada Azure'i teenus.

## <a name="create-a-stream-analytics-job"></a>Voo Analytics töö loomine

1. [Azure portaali](http://portal.azure.com), klõpsake plussmärki ja seejärel tippige **Voo ANALYTICS** aknas tekst paremale. Seejärel valige **voo Analytics töö** tulemuste loendis.

    ![Looge uus voo Analytics töö](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Sisestage töö kordumatu nimi ja veenduge, et tellimus on õige oma töö. Seejärel saate luua uue ressursirühma või valige olemasoleva tellimuse.

3. Valige oma töö jaoks asukoht. Kiirus töötlemine ja andmete kulude vähendamine on soovitatav edastamine, valides ressursirühm ja ettenähtud salvestusruumi konto samasse asukohta.

    ![Looge uus voo Analytics töö üksikasjad](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] Tuleb teil luua salvestusruumi konto ainult üks kord müüginäitajaid piirkonna kohta. See salvestusruumi jagatakse üle kõik voo Analytics tööd, mis on loodud selle piirkonna.

4. Märkige ruut paigutada oma töö armatuurlauale ja seejärel klõpsake nuppu **Loo**.

    ![töö loomine on pooleli](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Peaksite nägema "Juurutamise alustamine..." brauseriakna paremas ülanurgas kuvatakse. Varsti see muutub lõplikus akna nagu allpool näidatud.

    ![töö loomine on pooleli](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Azure'i voo Analytics päringu koostamine

Pärast loomist oma töö aeg on see avada ja päringu koostamine. Pääsete oma töö hõlpsasti avamiseks klõpsake seda paani.

![Töö paan](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Klõpsake paani **Töö topoloogia** **Päringuvälja Päringuredaktori minna** . **Päringuredaktori** saate sisestada T-SQL-päringu, mis sooritavad üle sissetuleva sündmuse andmete muutmist.

![Päringuväli](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Päringu: Arhiiv teie lähteandmed

Lihtsaim vorm päring on läbiv päring, mis arhiivi kõik sisendandmete määratud arvust. Asukohta oma arvutisse alla laadida [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) näidisfaili andmed. 

1. Kleepige päringu PassThrough.txt failist. 

    ![Testi Sisestuskeel voo](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Märkige ruut **Näidisandmete faili üleslaadimiseks** klõpsake sisendit kõrval kolm punkti.

    ![Testi Sisestuskeel voo](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Paremal avatakse paani tulemusena, see valige HelloWorldASA-InputStream.json andmefail oma allalaaditud asukohast ja klõpsake nuppu **OK** paani allosas.

    ![Testi Sisestuskeel voo](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Klõpsake nuppu **testida** ala akna vasakus ülanurgas hammasratta ja töödelda oma testi valimi andmekomplekti päring. Tulemuste aken avaneb päringu all, kui töötlemine on lõpule viidud.

    ![Testi tulemused](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Päringu: Tingimuse alusel andmete filtreerimine

Proovime nüüd tingimuse alusel tulemite filtreerimiseks. Soovime kuvada tulemusi ainult sündmused, mis on pärit "sensorA." Päring on Filtering.txt faili.

![Andmete voo filtreerimine](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Pange tähele, et tõstutundlik päringu võrdleb stringi väärtus. Klõpsake nuppu **testi** hammasratta uuesti, et päringu. Päring peaks tagastama 389 ridade 1860 sündmustest.

![Teine väljund tulemused päringu testimine](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Päringu: Teatise käivitada äri töövoo

Teeme meie päringu üksikasjalikumat. Iga tüüpi andur soovime jälgida Keskmine temperatuur 30-sekundiline akna kohta ja kuvada ainult siis, kui keskmine temperatuur on üle 100 kraadi tulemid. Me Kirjutage järgmine päring ja seejärel klõpsake nuppu **testi** tulemite kuvamiseks. Päring on ThresholdAlerting.txt faili.

![30-sekundiline filter päring](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Nüüd peaks nähtaval tulemeid, mis sisaldavad ainult 245 read ja andurid nimed, kus mõõdukas on suurem kui 100. Selle päringu rühmitatakse sündmuse voo **dspl**, mis on üle 30 sekundit **Langema akna** andur nime järgi. Ajalise päringute märkima, kuidas soovime edenemise aeg. Klausel **AJATEMPLI,** kasutades me määratud **OUTPUTTIME** veeru kõik ajaline arvutused korda seostada. Üksikasjalikku teavet MSDN-i artiklitest [Ajajuhtimise](https://msdn.microsoft.com/library/azure/mt582045.aspx) ja [akendesüsteemide funktsioonide](https://msdn.microsoft.com/library/azure/dn835019.aspx)kohta.

### <a name="query-detect-absence-of-events"></a>Päringu: Tuvasta puudumisel sündmused

Kuidas saab kirjutada päringu sisend puudumine leida? Vaatame leida viimast, et sensori saadetud andmed ja siis ei saada sündmuste järgmise minuti. Päring on AbsenseOfEvent.txt faili.

![Tuvasta puudumisel sündmused](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Siin kasutame **Vasak** välisühend sama andmevoos (iseendaga ühendamine). **Sisemise** ühenduse, tagastatakse tulemuseks ainult vaste leidmisel.  **Vasakul VÄLISES** ühendamises, kui ühendus vasakus servas ürituse on vasteta, rea, mis on NULL kõigi veergude paremal tagastatakse. See meetod on väga kasulik leida sündmuste puudumine. Vaadake meie MSDN-i dokumentatsiooni Lisateavet [liituda](https://msdn.microsoft.com/library/azure/dn835026.aspx).

## <a name="conclusion"></a>Kokkuvõte

Selle õpetuse eesmärk on näidata, kuidas kirjutada eri voo Analytics päringukeele päringud ja brauseris tulemuste vaatamiseks. Kuid see on te alles alustasite. Saate teha nii palju voo Analytics. Voo Analytics toetab paljusid sisendina ja väljundid ja saate isegi kasutada funktsioone Azure seadme õ, et see tööriist analüüsimiseks andmevoogu. Voo Analytics kohta rohkem uurida meie [Õppekeskuse MAPI](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/)abil saate alustada. Lisateavet selle kohta, kuidas kirjutada päringuid, lugege artiklit [levinud päring mustrite](./stream-analytics-stream-analytics-query-patterns.md)kohta.
