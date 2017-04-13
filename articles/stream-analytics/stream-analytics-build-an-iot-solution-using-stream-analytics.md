<properties
    pageTitle="Koostada lahenduse asjade voo Analytics abil | Microsoft Azure'i"
    description="Voo Analytics asjade lahendus Kassa stsenaarium õpetuse töötamise alustamine"
    keywords="asjade lahenduse akna funktsioonid"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Asjade lahenduse koostamine voo Analytics abil

## <a name="introduction"></a>Sissejuhatus

Selles õppetükis saate teada, kuidas kasutada Azure voo Analytics andmete reaalajas ülevaateid toomine. Arendajad saate hõlpsalt kombineerida voogu andmeid, klõpsake nuppu-voogu, logid ja seadme genereeritud sündmustele, nt ajalooliste kirjeid või lähteandmed tuletada äri teadmisi. Täielikult hallatud, reaalajas voo arvutus teenus, mis majutab Microsoft Azure, Azure voo Analytics pakub sisseehitatud paindlikkust, madal latentsus ja skaleeritavus saada olete ja minutiga töötab.

Pärast selle õpetuse, on võimalik:

-   Tutvumine Azure'i voo Analytics portaali.
-   Konfigureerige ja juurutamine streaming tööd.
-   Väljendada tegelike probleemide ja nende lahendamiseks voo Analytics päringukeele abil.
-   Välja streaming lahendusi oma klientidele confidence voo Analytics abil.
-   Probleemide tõrkeotsing jälgimine ja logimine teenuse abil.

## <a name="prerequisites"></a>Eeltingimused

Peate selle õpetuse lõpuleviimiseks järgmine kohustuslik tarkvara:

-   [Azure'i PowerShelli](../powershell-install-configure.md) uusim versioon
-   Visual Studio 2015 või [Visual Studio ühenduse](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) tasuta
-   On [Azure tellimuse](https://azure.microsoft.com/pricing/free-trial/)
-   Arvutis administraatoriõigusi
-   [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) Microsofti allalaadimiskeskusest alla laadida
-   Valikuline: Lähtekoodi TollApp sündmuse generaator ka [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Stsenaarium Sissejuhatus: "Tere, teemaks!"


Teemaks asub levinud nähtus. Ilmneda neid palju kiirliiklusega, sillad ja tunnelid kogu maailmas. Iga teemaks on mitu teemaks boksid. Veebisaidil käsitsi boksid, lõpetate teemaks maksma saatja. Veebisaidil automatiseeritud boksid, iga boksi peal sensor skannib RFID kaart, mis on kinnitatud auto esiklaas kui te kaotate teemaks boksi. See on lihtne visualiseerida liikumist nende teemaks jaamade nimega voogu sündmus, mille huvitav toiminguid teha.

![Auto juures teemaks bokside pilt](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Sissetuleva andmed

Selle õpetuse töötab kaks andmete voogu. Sisenemis- ja välju teemaks vahel andurid aedvili esimese voo. Teine voo on staatiline otsing andmekomplekti, mis sisaldab registreerimise andmed.

### <a name="entry-data-stream"></a>Kirje andmevoos

Kirje andmevoos sisaldab teavet auto, nagu need sisestage teemaks jaamad.


| TollID | EntryTime               | LicensePlate | Olek | Tegemine   | Mudel   | VehicleType | VehicleWeight | Teemaks | Sildi       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 2014-09-10 12:01:00.000 | JNB 7001     | NY    | Honda  | CRV     | 1           | 0             | 7    |           |
| 1      | 2014-09-10 12:02:00.000 | YXZ 1001     | NY    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 2014-09-10 12:02:00.000 | ABC 1004     | CT    | Ford   | Taurus  | 1           | 0             | 5    | 456789123 |
| 2      | 2014-09-10 12:03:00.000 | XYZ 1003     | CT    | Toyota | Corolla | 1           | 0             | 4    |           |
| 1      | 2014-09-10 12:03:00.000 | BNJ 1007     | NY    | Honda  | CRV     | 1           | 0             | 5    | 789123456 |
| 2      | 2014-09-10 12:05:00.000 | EAK 1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Siin on Lühikirjeldus veerud:

| Veerg | Kirjeldus |
|--------------|--------------------------------------------------------------------|
| TollID       | Teemaks boksi ID, mis tuvastab kordumatult teemaks kiosk            |
| EntryTime    | Kuupäeva ja kellaaja kirje sõiduki teemaks boksi UTC-vormingus |
| LicensePlate | Sõiduki numbrimärgi arv                            |
| Olek        | USA osariik                                           |
| Tegemine         | Auto tootja                                 |
| Mudel        | Mudeli arvu auto.                                 |
| VehicleType  | Kas 1 sõiduautode või kommertsveokite 2       |
| WeightType   | Sõiduki kaal tonnides; sõiduautode 0                   |
| Teemaks         | USA teemaks väärtus                                              |
| Sildi          | E-silti auto, mis automatiseerib makse; tühi, kus on makse tehtud käsitsi |


### <a name="exit-data-stream"></a>Väljumine andmevoos

Väljumine andmevoos sisaldab teavet auto teemaks station lahkumata.


| **TollId** | **ExitTime**                 | **LicensePlate** |
|------------|------------------------------|------------------|
| 1          | 2014-09-10T12:03:00.0000000Z | JNB 7001         |
| 1          | 2014-09-10T12:03:00.0000000Z | YXZ 1001         |
| 3          | 2014-09-10T12:04:00.0000000Z | ABC 1004         |
| 2          | 2014-09-10T12:07:00.0000000Z | XYZ 1003         |
| 1          | 2014-09-10T12:08:00.0000000Z | BNJ 1007         |
| 2          | 2014-09-10T12:07:00.0000000Z | EAK 1007         |

Siin on Lühikirjeldus veerud:


| Veerg | Kirjeldus |
|--------------|-----------------------------------------------------------------|
| TollID       | Teemaks boksi ID, mis tuvastab kordumatult teemaks kiosk         |
| ExitTime     | Kuupäeva ja kellaaja väljumise sõiduki teemaks boksi UTC-vormingus |
| LicensePlate | Sõiduki numbrimärgi arv                         |

### <a name="commercial-vehicle-registration-data"></a>Äriliseks registreerimise andmed

Õpetuse kasutab kaubik registreerimise andmebaasi staatilise hetktõmmise.


| LicensePlate | Atribuutide RegistrationId | Aegunud |
|--------------|----------------|---------|
| SVT 6023     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| BAC 1005     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Siin on Lühikirjeldus veerud:


| Veerg | Kirjeldus |
|--------------|-----------------------------------------------------------------|
| LicensePlate | Sõiduki numbrimärgi arv                         |
| Atribuutide RegistrationId     | Sõiduki registreerimise ID |
| Aegunud | Sõiduki registreerimise olek: 0, kui masina on aktiivne, 1, kui registreerimine on aegunud                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Azure'i voo Analyticsi keskkonna häälestamine

Selle õpetuse tegemiseks on vaja Microsoft Azure'i tellimust. Microsoft pakub tasuta prooviversiooni Microsoft Azure'i teenuste jaoks.

Kui teil pole Azure'i konto, saate [kutse tasuta prooviversioon](http://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Tasuta prooviversiooni kasutajaks, tuleb teil mobiilsideseadmes, millel saab saata tekstsõnumeid ja kehtiv krediitkaart.

Ärge unustage juhised leiate käesoleva artikli lõpus "Puhastamiseks Azure'i konto" jaotis nii, et teete tasuta Azure krediitkaardiga $200 kasutamine.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Juhend on vaja Azure ressursside

Selle õpetuse nõuab kahe sündmuse jaoturi saada *kirje* ja *koosolekult lahkumine* andmevoogu. Azure'i SQL-andmebaasi väljundid voo Analytics töö tulemused. Azure'i salvestusruumi talletab sõidukite registreerimine viide andmeid.

TollApp kausta github Setup.ps1 skripti abil saate luua kõik ressursid. Huvides aeg, soovitame selle käivitada. Kui soovite lisateavet selle kohta, kuidas konfigureerida järgmisi ressursse Azure'i portaalis, vaadake "Konfigureerimine kuueosalisest ressursid Azure'i portaalis" lisa.

Laadige alla ja täiendavad [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) kausta ja failid salvestada.

Avage soovitud **Microsoft Azure'i PowerShelli** aken _administraatorina_. Kui teil pole veel Azure PowerShelli, järgige juhiseid [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installida.

Kuna Windows blokeerib automaatselt .ps1, dll ja .exe faile, peate määrama täitmise poliitika enne skripti käivitamist. Veenduge, et töötab Azure PowerShelli aken _administraatorina_. Käivitage **Set-ExecutionPolicy piiranguteta**. Vastava viiba kuvamisel tippige **Y**.

!["Set-ExecutionPolicy piiramatu" töötavad Azure PowerShelli akna kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Veenduge, et käsk töötas **Get-ExecutionPolicy** käivitada.

!["Get-ExecutionPolicy", mis töötavad Azure PowerShelli akna kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Avage kausta, mis sisaldab skripte ja generaator rakendus.

!["CD-le .\TollApp\TollApp" töötavad Azure PowerShelli akna kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Tippige **.\\ Setup.ps1** Azure'i konto häälestamine loomine ja konfigureerimine kõik vajalikud ressursid ja sündmuste loomiseks alustada. Skripti CEIP jätkab piirkonnas luua oma ressursse. Määrata piirkonnas, võite anda selle **-asukohta** parameeter, nagu järgmises näites:

**. \\Setup.ps1-asukoht "Kesk-USA"**

![Azure'i sisselogimislehe kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Skripti avab Microsoft Azure'i lehe **Logi sisse** . Sisestage oma konto kasutajanimi ja parool.

> [AZURE.NOTE] Kui teie konto on mitu tellimust juurdepääsu, palutakse teil sisestada tellimuse nime, mida soovite kasutada õpetuse.

Skripti võib kuluda mitu minutit käivitamiseks. Kui see on lõpule jõudnud, väljund peaks välja nägema järgmine pilt.

![Väljundi aknas Azure PowerShelli skripti kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Näete ka mõne muu aken, mis on sarnane järgmine pilt. See rakendus on saatmine sündmuste Azure'i sündmuse jaoturi, mida on vaja õpetuse käivitamiseks. Jah, Peata rakendust või akna sulgeda, kuni olete õpetuse.

![Kuvatõmmis "Saatmine sündmuse jaoturi andmed"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Peaks olema näeksid kõik loodud ressursid Azure'i portaalis kohe. Minge <https://manage.windowsazure.com>ja logige sisse oma konto identimisteave.

### <a name="azure-event-hubs"></a>Azure'i sündmuse jaoturi

Klõpsake **Teenuse SIINI** Azure portaali vasakus servas skripti eelmises jaotises loodud sündmuse jaoturi kuvamiseks.

![Teenuse siini](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Teie tellimus kuvatakse kõik saadaolevad nimeruumid. Klõpsake seda, mis algab *tolldata* (siinses näites tolldata4637388511). Klõpsake vahekaarti **Sündmus JAOTURI** .

![Jaoturi vahekaarti sündmus Azure'i portaalis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Kuvatakse kaks sündmuse jaoturi nimega *kirje* ja *sulgege* loodud selle nimeruumi.

![Kuvatõmmis "kirje" ja "välju" sündmuse jaoturi Azure'i portaalis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure'i salvestusruumi container

1. Klõpsake nuppu **TALLETUSMAHU** Azure portaali vasakus servas Azure Storage container, mida kasutatakse õpetuse kuvamiseks.

    ![Salvestusruumi menüükäsk](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Klõpsake seda, mis algavad *tolldata* (siinses näites tolldata4637388511). Klõpsake vahekaarti **ÜMBRISTE** loodud container kuvamiseks.

    ![Azure'i portaalis ümbriste menüü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Klõpsake **tolldata** ümbris üleslaaditud JSON faili, mis sisaldab registreerimise andmed.

    ![Registration.json faili ümbrises kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Azure'i SQL-andmebaas

1. Klõpsake vasakus servas Azure portaali **SQL ANDMEBAASE** SQL-andmebaasi, mida kasutatakse õpetuses kuvamiseks.

    ![SQL-i andmebaasid](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Klõpsake **tolldatadb**.

    ![Kuvatõmmis loodud SQL-andmebaasiga](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. Kopeerige serveri nimi ilma pordi number (*serverinimi*. database.windows.net, näiteks).

## <a name="connect-to-the-database-from-visual-studio"></a>Visual Studio andmebaasiga ühenduse loomine

Visual Studio abil pääsete päringutulemite väljundi andmebaasi.

Visual Studio ühenduse SQL-andmebaasiga (sihtkoht):

1. Visual Studio avada, ja siis käsku **Tööriistad** > **andmebaasiga ühenduse loomine**.

2. Kui teilt küsitakse, siis klõpsake **Microsoft SQL Server** andmeallikana.

    ![Dialoogiboks andmeallika muutmine](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. Kleepige väljale **Serveri nimi** nimi, mille kopeerisite eelmises jaotises Azure'i portaalis (ehk teisisõnu öeldes *serverinimi*. database.windows.net).

4. Klõpsake nuppu **Kasuta SQL serveri autentimine**.

5. Sisestage väljale **kasutajanimi** **tolladmin** ja **123toll!** väljale **parool** .

6. Klõpsake nuppu **Valige või sisestage andmebaasi nimi**ja valige **TollDataDB** andmebaas nimega.

    ![Dialoogiboks ühenduse lisamine](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Klõpsake nuppu **OK**.

8. Avatud Server Explorer.

    ![Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Vt nelja TollDataDB andmebaasi tabelid.

    ![TollDataDB andmebaasi tabelid](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Sündmuse generaator: TollApp proovi projekti

PowerShelli skripti käivitub automaatselt saata sündmuste TollApp valimi rakenduse programmi abil. Te ei pea täiendavate toimingute.

Juhul, kui olete huvitatud rakendamise üksikasjad, leiate TollApp rakenduse lähtekoodi GitHub [Näidised/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Proovi kood kuvatakse Visual Studio kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Voo Analytics töö loomine

1. Azure'i portaalis, avage voo Analytics ja luua uue analytics töö lehe vasakus nurgas nuppu **Uus** .

    ![Nupp uus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Klõpsake **kiire loomine**. Valige sama piirkonna, kus teie muud ressursid on loodud skripti.

3. **Piirkondliku jälgimine salvestusruumi konto** säte, valige **Loo uus salvestusruumi konto** ja anda sellele kordumatu nimi. Azure'i voo Analytics kasutab selle konto salvestada tulevaste projektide jälgimisega seotud teavet.

4. Klõpsake **Loomine voo ANALYTICS töö** lehe allosas.

    ![Voo Analytics töö suvand loomine](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Sisendandmete allikatest määratlemine

1. Klõpsake portaali loodud analytics töö.

    ![Kuvatõmmis analytics töö portaalis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. Klõpsake vahekaarti **SISENDINA** lähteandmete määratleda.

    ![Sisendina menüü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Klõpsake **Lisa sisend**.

    ![Suvandi sisestusmeetodi lisamine](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. Klõpsake esimesel lehel **ANDMEVOOS** .

    ![Suvandi andmevoos](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. Klõpsake viisardi teisel lehel **Sündmuse JAOTURI** .

    ![Sündmuse jaoturi suvand](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. Sisestage **EntryStream** **SISESTUSKEEL alias (PSEUDONÜÜM)**.

7. Klõpsake **Sündmuse jaoturi** rippmenüüd ja valige üks, mis algab selgitava lausega "TollData" (nt TollData9518658221).

8. Valige **kirje** sündmuse jaoturi nime ja **Kõik** sündmuse jaoturi poliitika nimi.

    Näeb teie sätted:

    ![Sündmuse jaoturi sätted](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. Järgmisel lehel Valige **JSON** **Sündmuse SERIALISEERIMISVORMINGUS** ja **UTF8** **KODEERING**.

    ![Sariväljaanne sätted](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Klõpsake nuppu **OK** lehe allosas viisardist.

    ![Kuvatõmmis, EntryStream sisendit Azure'i portaalis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Nüüd, kui olete loonud kirje voo, järgite samu juhiseid välju voo loomiseks. Viisardi lehel kolmanda kindlasti sisestada väärtuste kuvamiskuju järgmine pilt.

    ![Väljumine voo sätted](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    Olete määratlenud kaks Sisestuskeel voole:

    ![Määratletud Sisestuskeel voogu Azure'i portaalis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Järgmiseks tuleb lisada viide andmesisestuse bloobimälu auto registreerimise andmeid sisaldava faili.

11. **SISESTUSMEETODI lisamine**nuppu ja seejärel käsku **Viide andmed**.

    ![Valitud lähteandmed võimalusi "Lisa sisend"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. Järgmisel lehel Valige salvestusruumi konto, mis algab **tolldata**. Container nimed peavad olema **tolldata**ja bloobimälu nimi jaotises **Tee MUSTRI** peaks olema **registration.json**. Sellise nimega fail on tõstutundlik ja tuleks väiketähtedeks.

    ![Ajaveebi salvestusruumi sätted](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Valige soovitud väärtused, nagu on näidatud järgmisel lehel järgmine pilt ja seejärel klõpsake nuppu **OK** , et viisard.

    ![JSON valik "isegi serialiseerimisvormingus" ja UTF8 "Kodeering"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Nüüd kõik sisendeid on määratletud.

![Kuvatõmmis kolme määratletud sisendeid](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Väljundi määratlemine

1. Klõpsake vahekaarti **väljundi** ja klõpsake **Lisada AN väljundi**.

    ![Väljundi menüüd ja suvand "Lisa väljund"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Klõpsake nuppu **SQL-andmebaasi**.

3. Valige serveri nimi, mida kasutati selle artikli jaotises "Ühenduse soovite andmebaasi kaudu Visual Studio". Andmebaasi nimi on **TollDataDB**.

4. Sisestage väljale **kasutajanimi** **tolladmin** **123toll!** väli **parool** ja **TollDataRefJoin** **tabeliväljal** .

    ![SQL-andmebaasi sätted](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure'i voo analytics päring

Menüü **päring** sisaldab SQL-päringu, mis muudab sissetulevad andmed.

![Päringu vahekaarti loendisse päringu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

Selle õpetuse püüab mitu business küsimustele, mis on seotud andmed ja importida voo Analytics suhtes, mida saab kasutada Azure voo Analytics oluline vastuse tasuline.

Enne alustamist esimese voo Analytics tööpäevad, uurime mõne stsenaariumid ja päringusüntaksit.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Azure'i voo Analytics päringukeele tutvustus
-----------------------------------------------------

Oletame, et soovite loendada mis sisestage teemaks boksi. Kuna tegemist on pidev voo sündmuste, peate määratlema "aja jooksul." Vaatame muutmine küsimusele olema "Kuidas mitut sisestage teemaks boksi iga kolme minuti järel?". See on tavaliselt edaspidi langema loendus.

Heitkem pilk Azure'i voo Analytics päring, mis vastab sellele küsimusele:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Nagu näete, kasutab Azure voo Analytics päringu keelt, mida on nagu SQL-i ja lisab mõne aja seotud aspektide päringu määramiseks laiendid.

Lisateabe saamiseks lugege MSDN päringus kasutatav [Ajajuhtimise](https://msdn.microsoft.com/library/azure/mt582045.aspx) ja [akendesüsteemide](https://msdn.microsoft.com/library/azure/dn835019.aspx) importida.

## <a name="testing-azure-stream-analytics-queries"></a>Azure'i voo Analytics päringud testimine

Nüüd, kui olete kirjutanud esimese Azure'i voo Analytics päringu, on aeg test selle abil näidisfailide andmed asuvad TollApp kaustas järgmises asukohas:

**.. \\TollApp\\TollApp\\andmed**

See kaust sisaldab järgmisi faile:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Küsimus 1: Teemaks boksi sõidukite arv

1. Avage Azure'i portaal ja minge oma loodud Azure'i voo Analytics töö. Klõpsake **PÄRINGU** vahekaarti ja kleepige eelmise jaotise päring.

    ![Päringu vahekaarti kleebitud päring](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. See päring Näidisandmete kinnitamiseks klõpsake nuppu **testi** . Avage avanevas dialoogiboksis Entry.json, faili, mis on Näidisandmete **EntryTime** sündmuse voo kaudu.

    ![Kuvatõmmis Entry.json fail](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Veenduge, et päringu väljund on oodatud viisil:

    ![Testi tulemused](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Küsimus 2: Aruande kogu aeg iga auto läbida teemaks boksi

Keskmine kellaaeg, auto läbida teemaks nõutava aitab hinnata protsessi ja klientide programmikasutuskogemuse.

Kogu aeg leidmiseks peate EntryTime voo ExitTime voo abil liituda. Voole TollId ja LicencePlate veergude liituda. **Liitumine** tehtemärk peab teil määrata ajaline tulla kirjeldav ühendatud sündmuste lubatud aja erinevus. Funktsioon **DATEDIFF** abil saab määrata, et peaksid olema üksteisest rohkem kui 15 minutit. Mida rakendatakse funktsioon **DATEDIFF** väljumiseks ja kirje korda arvutada auto veedab teemaks jaamas tegelik aeg. Pöörake tähelepanu **DATEDIFF** kasutamise kasutamisel **Valige** lause, mitte **liitumine** tingimus.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Selle päringu testimiseks värskendage oma tööd menüü **päring** päringu.

    ![Päringu vahekaarti värskendatud päringu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Klõpsake nuppu **testi** ja määrata Sisestuskeel näidisfailide EntryTime ja ExitTime.

    ![Valitud failid kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Märkige ruut päringu testimiseks ja vaadata väljund:

    ![Väljundit test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Küsimus 3: Aruande kõik kommertsveokid aegunud registreerimine

Azure'i voo Analytics saate kasutada staatilisi pilte andmete ajaline andmevoogu liituda. Näitamaks, et seda võimalust, kasutage järgmist valimi küsimus.

Kui hoolduses on registreeritud teemaks ettevõttega, see läbi teemaks boksi ilma kontrollimise lõpetanud. Kasutate äriliseks masina otsingutabel kõik kommertsveokid, mis on aegunud registreerimise tuvastamiseks.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Test päringu abil andmeid, peate määratlema sisendi allikat viide andmete, mis on juba teinud.

1. Selle päringu testimiseks päringu kleepida **PÄRINGU** vahekaarti, käsku, **testimine**ja määrake kaks sisendandmete allikatest:

    ![Valitud failid kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. Päringu väljund vaatamiseks tehke järgmist.

    ![Päringu väljund kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Voo Analytics töö alustamine


Nüüd on aeg konfiguratsiooni lõpetamiseks ja alustada tööd. Salvesta päring küsimus 3, mis toodab väljundi, mis vastab **TollDataRefJoin** väljund tabeli skeemi.

Minge töö **ARMATUURLAUA**ja klõpsake nuppu **Käivita**.

![Nupp Start töö armatuurlaua kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

Avanevas dialoogiboksis **kohandatud**aeg **Alustada väljundi** ajal muuta. Saate muuta tunni üks tund enne praeguse kellaaja. See muudatus tagab kõigi sündmuste sündmuse keskuse kaudu töödeldakse Kuna hakkasite luua sündmuste õpetuse alguses. Nüüd klõpsake märge, et alustada tööd.

![Kohandatud aja valimine](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Töö käivitamist võib kuluda mõni minut. Saate vaadata oleku ülataseme lehel voo Analytics.

![Töö oleku kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>Kontrolli tulemusi Visual Studio

1. Avage Visual Studio Server Explorer ja paremklõpsake **TollDataRefJoin** tabelit.
2. Klõpsake nuppu **Kuva tabeli andmete** kuvamiseks väljund oma töö.

    ![Valiku "Kuva tabeli andmed" Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Azure'i voo Analytics välja mastaapimiseks tööde haldamine

Azure'i voo Analytics on mõeldud elastically mastaapimiseks nii, et seda võib käsitleda palju andmeid. Azure'i voo Analytics päringu abil saate klauslis **PARTITION,** mis see toiming tihendab välja süsteemi kindlaks teha. **PartitionId** on teisiti veerg, mis süsteemi lisatakse vastavad sisendit (sündmuse jaoturi) sektsiooni ID-d.

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Praeguse töö lõpetada, värskendamine päringu **PÄRINGU** vahekaarti ja avage vahekaart **skaala** .

    **Üksuste STREAMING** määratleda Arvuta power, millel saab saata töö summa.

2. Nihutage liugurit, kuni 6.

    ![Kuvatõmmis – 6 streaming üksuste valimine](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Minge menüüsse **VÄLJUNDEID** ja **TollDataTumblingCountPartitioned**SQL tabeli nime muuta.

Kui alustada tööd nüüd Azure'i voo Analytics saate jaotada töö kogu Arvuta ressursside ja saavutada parem läbilaskevõime. Pange tähele, et TollApp rakendus ka saadab liigendatud TollId sündmused.

## <a name="monitor"></a>Jälgimine

Vahekaardil **KUVAR** sisaldab statistika käivitatud töö kohta.

![Kuvatõmmis statistikat tööde](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

Pääsete **Logi** vahekaart **ARMATUURLAUD** .

![Suvand "Logi"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Kuvatõmmis, kus on kuvatud tööde oleku Logi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Teatud sündmuste kohta lisateabe vaatamiseks valige sündmus ja seejärel klõpsake nuppu **üksikasjad** .

![Valitud sündmuse üksikasjad kuvatõmmis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Kokkuvõte

Selle õpetuse kasutusele Azure'i voo Kasutusanalüüsi teenus. See näidanud konfigureerimine sisendi ja väljundi voo Analytics töö. Õpetuse teemaks andmete stsenaariumi kasutamisel koos selgitustega levinumat probleeme tekkida ruumi lihtsa SQL-like päringutega Azure'i voo Analytics andmete liikumistee ja kuidas need lahendada. Õpetuse kirjeldatud SQL-i laiend importida ajaline andmetega töötamiseks. See ilmnes Kuidas liituda andmevoogu, kuidas rikastamine andmevoos staatilise viide andmetega mastaapimiseks välja päringu suurema läbilaskevõimega saavutamiseks tehke järgmist.

Kuigi selles õpetuses pakub hea Sissejuhatus, pole täielik, mis tahes viisil. Mitme päringu mustrite [päringu näited levinud voo Analytics mustreid](stream-analytics-stream-analytics-query-patterns.md)SAQL keele abil saate otsida.
Lugege lisateavet Azure voo Analytics [online dokumentatsiooni](https://azure.microsoft.com/documentation/services/stream-analytics/) .

## <a name="clean-up-your-azure-account"></a>Azure'i kontosse puhastamiseks

1. Lõpetage voo Analytics töö Azure'i portaalis.

    Skripti Setup.ps1 loob kahe sündmuse jaoturi ja SQL-andmebaasi. Järgmised juhised aitavad teil puhastada ressursid õpetuse lõpus.

2. PowerShelli aknas tippige **.\\ Cleanup.ps1** skripti, mis kustutab õpetuse kasutatud ressursside käivitamiseks.

    > [AZURE.NOTE] Ressursid on määratletud nimi. Veenduge, et hoolikalt üle iga üksuse enne kinnitamist eemaldamine.

    ![Kuvatõmmis puhastus protsess](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
