<properties
    pageTitle="Automaatselt skaala arvutada sõlmed on Azure'i paketi pool | Microsoft Azure'i"
    description="Luba automaatne mastaapimine cloud rakenduskausta dünaamiliselt kohandada Arvuta sõlmed kogumi arv."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Automaatselt mastaapida Arvuta sõlmed on Azure'i paketi pargis

Automaatset mastaapimist, kus saate Azure'i paketi teenuse dünaamiliselt lisada või eemaldada Arvuta sõlmed pargis määratlete parameetrite põhjal. Saate potentsiaalselt salvestada aega ja raha reguleerides automaatselt summa, Arvuta power kasutatavaid rakenduse--lisada oma töö ülesanne nõuab suurendamine sõlmed ja eemaldada, kui nad vähendada.

Lubate automaatne mastaapimine kogumi Arvuta sõlmed seostada seda ka *autoscale valemi* , mis määratlete, näiteks koos [PoolOperations.EnableAutoScale] [ net_enableautoscale] meetod [Paketi .NET](batch-dotnet-get-started.md) teegis. Paketi teenuse seejärel kasutab järgmist valemit Arvuta sõlmed, mida on vaja käivitada oma töökoormus arvu. Paketi vastab teenuse mõõdikute perioodiliselt kogutud andmete näidised, ning reguleerib Arvuta sõlmed pool konfigureeritav intervalliga, võttes aluseks valemi arv.

Saate lubada automaatset mastaapimist on loomisel või mõne olemasoleva kausta sisse. Saate muuta mõnda olemasolevat valemit, klõpsake kausta, mis on "autoscale" lubatud. Paketi võimaldab hinnata valemite enne määramisel kaustu, samuti automaatse skaleerimise käivitatakse oleku jälgimine.

## <a name="automatic-scaling-formulas"></a>Automaatse skaleerimise valemid

Mõni automaatse skaleerimise valem on stringi väärtus, et määratleda sisaldab ühe või mitme laused, mis on määratud mõne kausta [autoScaleFormula] [ rest_autoscaleformula] elemendi (paketi REST) või [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] atribuudi (paketi .net-i). Kui määratud on, kasutab paketi service valem määratlemiseks sihtrühma arv Arvuta sõlmed pool järgmise intervalli töötlemise (rohkem intervallide hiljem). Valemi stringi ei tohi ületada 8 KB suurust, võib sisaldada kuni 100 laused, mis on semikoolonitega ja võivad sisaldada reapiirid ja kommentaarid.

Mõelge automaatse skaleerimise valemeid, kasutades paketi autoscale "keel". Valem on tasuta loodud avaldised, saate lisada nii teenuse määratletud muutujate (muutujad määratletud paketi teenus) ja kasutaja määratletud muutujate (muutujad määratleda). Ta saab teha erinevaid toiminguid need väärtused sisseehitatud tüübid, tehtemärke ja funktsioonide abil. Näiteks aruande võib võtta järgmisel kujul:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Valemis on üldiselt mitut laused, mis toiminguid väärtused, mis on saadud varasemate arvamuste. Näiteks esmalt saame väärtus `variable1`, seejärel andke seda funktsiooni asustamiseks `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Valemi need aruanded, teie eesmärgiks on jõudma arvu Arvuta sõlmed, mis peaks pool ülespoole- **sihtotstarbeline sõlmed** **sihtrühma** arv. See arv võib olla suurem, lower ja sama sõlmed kogumi praegune arv. Paketi tulemiks on pool autoscale valemi kindla intervalliga ([skaleerimise tagant](#automatic-scaling-interval) käsitletakse allpool). Seejärel kohandab sõlmed, et arv, mis teie autoscale valem määrab hindamise ajal sihtrühma arv.

Kiirülevaate näide valem üherealise autoscale saate määrata, et sõlmed arvu tuleks kohandada vastavalt aktiivseid tööülesandeid, kuni 10 Arvuta sõlmed arv.

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

Mõne järgmise jaotised selles artiklis käsitletakse mitmesuguste autoscale valemeid, sh muutujate, tehtemärkide, toimingute ja funktsioonide moodustavate üksuste. Saate teada hankimine erinevate Arvuta ressursside ja tööülesannete mõõdikute paketi sees. Saate neid mõõdikute arukalt kohandada oma pool sõlm count ressursi kasutus- ja tööülesande oleku põhjal. Seejärel saate teada, kuidas koostada valemi ja luba automaatne mastaapimine on paketi ülejäänud ja .net-i API-de abil. Meil kuvatakse lõpule jõuab mõni näide valemid.

> [AZURE.IMPORTANT] Iga Azure'i paketi konto on piiratud maksimum tuuma (ja seega Arvuta sõlmed), mida saab kasutada töötlemiseks. Paketi teenus loob sõlmed ainult selle core piires. Seetõttu võib-olla ei jõua sihtrühma arv Arvuta sõlmed, mis on määratud valemi. Leiate teavet [kvootide ja limiidid Azure'i paketi teenuse](batch-quota-limit.md) vaatamiseks ja suurendab oma konto piirmäärasid.

## <a name="variables"></a>Muutujad

Saate kasutada nii **teenuse määratletud** ja **kasutaja määratletud** muutujate autoscale valemeid. Teenuse määratletud muutujaid on sisse ehitatud paketi teenuse--mõned on lugemine kirjutamine ja mõned on kirjutuskaitstud. Kasutaja määratletud muutujad muutujate, *saate* määratleda. Eeltoodud valemis üherealise näide `$TargetDedicated` on teenuse määratletud muutuv ajal `$averageActiveTaskCount` on kasutaja määratletud muutuja.

Järgmistes tabelites on nii lugemis-ja kirjutamisõigusega ja kirjutuskaitstud muutujad, mis on määratletud paketi teenuse kuvamine

Saate **hankida** ja **määrata** need teenuse määratletud haldamiseks Arvuta sõlmed pargis arvu muutujate väärtusi:

| Registri teenuse määratletud muutujad | Kirjeldus |
| --- | --- |
| $TargetDedicated | **Sihtrühma** arv **Arvuta sõlmed mõeldud** pool. See on arv, mis pool peaks mastaabitud, et Arvuta sõlmed. See on "target" arvu, kuna on võimalik, et on pool pole sõlmed target arvu saavutamiseks. See võib juhtuda siis, kui sihtrühma arv sõlmed muudavad uuesti edaspidised autoscale hindamise enne pool on algse jõudnud. See võib juhtuda, kui paketi konto sõlm või core piirmäär on enne sõlmed target arv. |
| $NodeDeallocationOption | Toiming, mis ilmneb siis, kui Arvuta sõlmed eemaldatakse kaustast. Võimalikud väärtused on:<ul><li>**requeue**--lõpeb tööülesannete kohe ja paneb need tagasi tööde järjekorda nii, et need on ajakavas ümber.<li>**lõpetada**--lõpeb kohe tööülesanded ja nende eemaldab tööde järjekorda.<li>**taskcompletion**--ootab praegu töötavate lõpuleviimiseks toimingud ja seejärel eemaldab sõlme kaustast.<li>**retaineddata**--ootab kõik kohaliku tööülesande säilitatakse andmed sõlme enne eemaldamist sõlme pool puhastada.</ul> |

Saate **hankida** nende teenuste määratletud muutujate teha muudatusi, mis põhinevad mõõdikute paketi teenuse väärtus:

| Kirjutuskaitstud teenuse määratletud muutujat | Kirjeldus |
| --- | --- |
| $CPUPercent | Keskmine protsent CPU hõivatus. |
| $WallClockSeconds | Consumed sekundite arv. |
| $MemoryBytes | Keskmine arv kasutatud MB. |
| $DiskBytes | Keskmine arv gigabaiti kasutatud kohaliku kettale. |
| $DiskReadBytes | Lugege baitide arvust. |
| $DiskWriteBytes | Kirjutatud baitide arvu. |
| $DiskReadOps | Lugege ketta toimingute arvu. |
| $DiskWriteOps | Arvu kirjutada ketta toiminguid teha. |
| $NetworkInBytes | Sissetuleva baitide arvul. |
| $NetworkOutBytes | Väljamineva baitide arvul. |
| $SampleNodeCount | Arvuta sõlmed arvu. |
| $ActiveTasks | Tööülesannete aktiivne olekus arv. |
| $RunningTasks | Tööülesannete töötab arv. |
| $PendingTasks | $ActiveTasks ja $RunningTasks summa. |
| $SucceededTasks | Arv, ülesandeid, mis on lõpule viidud. |
| $FailedTasks | Ülesanded, mida ei saanud arv. |
| $CurrentDedicated | Praegune arv sihtotstarbeline arvutada sõlmed. |

> [AZURE.TIP] Kirjutuskaitstud, teenuse määratletud muutujaid, mis on eespool näidatud on *objektid* , mis erinevad meetodid, omavahel seotud andmete avamiseks. Lisateabe saamiseks vaadake [Hangi Näidisandmete](#getsampledata) all.

## <a name="types"></a>Tüübid

Valemis on toetatud nende **tüüpi** .

- kahekordne
- doubleVec
- doubleVecList
- string
- ajatempli--ajatempli on liitindeksi struktuur, mis sisaldab järgmisi liikmeid.

    - aasta
    - kuu (1 – 12)
    - päeva (1 – 31)
    - WEEKDAY (vormingus numbri, nt esmaspäeval 1)
    - tundi (24-tunnise arvuvormingus, näiteks 13, mis tähendab 1 PL)
    - minutid (00 – 59)
    - teine (00 – 59)
- timeinterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Toimingud

Need **toimingud** on lubatud tüübid, mis on loetletud kohal.

| Toiming                             | Toetatud tehtemärgid   | Tulemuse tüüp   |
| ------------------------------------- | --------------------- | ------------- |
| kahekordne kahekordsete *tehtemärk*              | +, -, *, /            | kahekordne            |
| kahekordne *tehtemärk* timeinterval        | *                     | timeinterval      |
| doubleVec *tehtemärk* kahekordsete           | +, -, *, /            | doubleVec         |
| doubleVec *tehtemärk* doubleVec        | +, -, *, /            | doubleVec         |
| timeinterval *tehtemärk* kahekordsete        | *, /                  | timeinterval      |
| timeinterval *tehtemärk* timeinterval  | +, -                  | timeinterval      |
| timeinterval *tehtemärk* ajatempli     | +                     | ajatempli         |
| ajatempli *tehtemärk* timeinterval     | +                     | ajatempli         |
| ajatempli *tehtemärk* ajatempli        | -                     | timeinterval      |
| *tehtemärk*kahekordsete                      | -, !                  | kahekordne            |
| *tehtemärk*timeinterval                | -                     | timeinterval      |
| kahekordne kahekordsete *tehtemärk*              | < < =, ==, > =, >,! =  | kahekordne            |
| stringi *tehtemärk* string              | < < =, ==, > =, >,! =  | kahekordne            |
| ajatempli *tehtemärk* ajatempli        | < < =, ==, > =, >,! =  | kahekordne            |
| timeinterval *tehtemärk* timeinterval  | < < =, ==, > =, >,! =  | kahekordne            |
| kahekordne kahekordsete *tehtemärk*              | & &, & #124; & #124;      | kahekordne            |

Kui katsetamine kahekordne kolmekomponentsete operaatoriga (`double ? statement1 : statement2`), nullist erinev on **true,**ja 0 on **false**.

## <a name="functions"></a>Funktsioonid

Need eelmääratletud **funktsioonid** on saadaval ka automaatse skaleerimise valemi määratlemisel kasutamiseks.

| Funktsioon                          | Tagastuse tüüp   | Kirjeldus
| --------------------------------- | ------------- | --------- |
| AVG(doubleVecList)                | kahekordne        | Tagastab selle doubleVecList kõigi väärtuste keskmise väärtuse.
| Len(doubleVecList)                | kahekordne        | Tagastab vektorkuju, mis on loodud selle doubleVecList pikkus.
| LG(Double)                        | kahekordne        | Tagastab Logi alus 2 / topelt.
| LG(doubleVecList)                 | doubleVec     | Tagastab componentwise Logi selle doubleVecList alus 2. Mõne vec(double) siseneda konkreetselt parameetri. Muul juhul eeldatakse kahekordsete lg(double) versioon.
| ln(Double)                        | kahekordne        | Tagastab loomulikus Logi topelt.
| ln(doubleVecList)                 | doubleVec     | Tagastab componentwise Logi selle doubleVecList alus 2. Mõne vec(double) siseneda konkreetselt parameetri. Muul juhul eeldatakse kahekordsete lg(double) versioon.
| log(Double)                       | kahekordne        | Tagastab Logi alusega 10 topelt.
| log(doubleVecList)                | doubleVec     | Tagastab componentwise Logi selle doubleVecList alusega 10. Mõne vec(double) siseneda konkreetselt ühe kahekordsete parameetri. Muul juhul eeldatakse kahekordsete log(double) versioon.
| Max(doubleVecList)                | kahekordne        | Tagastab suurima väärtuse soovitud doubleVecList.
| min(doubleVecList)                | kahekordne        | Annab vastuseks väikseima väärtuse soovitud doubleVecList.
| norm(doubleVecList)               | kahekordne        | Annab vastuseks kahe norm vektori, mis on loodud selle doubleVecList.
| protsentiili (doubleVec v kahekordsete p) | kahekordne        | Tagastab protsentiili elemendi vektorkuju v.
| rand()                            | kahekordne        | Annab vastuseks juhusliku väärtuse vahemikus 0,0 ja 1.0.
| range(doubleVecList)              | kahekordne        | Tagastab selle doubleVecList vahe min ja max väärtused.
| STD(doubleVecList)                | kahekordne        | Tagastab väärtuste valimi standardhälve on doubleVecList.
| Stop()                            |               | Astmed hindamine autoscaling avaldis.
| SUM(doubleVecList)                | kahekordne        | Tagastab kõik komponendid on doubleVecList summa.
| kellaaeg (string dateTime = "")          | ajatempli     | Tagastab praeguse kellaaja kui parameetreid pole edastatakse ajatempel või kuupäeva ja kellaaja stringi ajatempel, kui see on möödas. Toetatud kuupäeva ja kellaaja vormingud on W3C-DTF ja RFC 1123.
| Val (kahekordne i doubleVec v)        | kahekordne        | Tagastab vektorkuju v käivitamine indeks on null, ma asukohas on elemendi väärtuse.

Funktsioonid, mida on kirjeldatud ülaltoodud tabelis aktsepteerib argumendina loendi. Komaga eraldatud loend on *kahekordne* ja *doubleVec*kombinatsioon. Näiteks:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

*DoubleVecList* väärtus teisendatakse ühe *doubleVec* enne hindamiseks. Näiteks kui `v = [1,2,3]`, seejärel `avg(v)` on võrdne helistaja `avg(1,2,3)`. Helistamiseks `avg(v, 7)` on võrdne helistaja `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Näidisandmete hankimine

Autoscale valemid toimivad mõõdikute andmete (proovi), mis on esitatud teenuse paketi. Valemi kasvab või ühel vaja pool suurus, mis selle teenuse hangib väärtuste põhjal. Teenuse määratletud muutujate, mida on kirjeldatud ülal on objektid, mis pakuvad erineval juurdepääsu andmetele, mis on seotud objekti. Näiteks järgmine avaldis esitab taotluse saada viimase viie minuti jooksul CPU hõivatus.

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Meetod | Kirjeldus |
| --- | --- |
| GetSample() | Funktsiooni `GetSample()` meetod tagastab vektorkuju andmete proovide.<br/><br/>Valimi on 30 sekundit väärt mõõdikute andmeid. Teisisõnu, saadakse näidised iga 30 sekundi järel. Kuid nagu allpool, on vahel, kui valim on kogutud, ja kui see on saadaval, kui soovite valemi. Pole kõik näidised antud aja jooksul võib olla saadaval, Valemi väärtustamine.<ul><li>`doubleVec GetSample(double count)`<br/>Saate määrata saada viimase näidised, mis on kogutud proovide arv.<br/><br/>`GetSample(1)`Tagastab viimase saadaval valimi. Mõõdikute nagu `$CPUPercent`, kuid see ei tohi kasutada kuna ei ole võimalik teada, *kui* valim on kogutud. See võib olla tehtud või süsteemi probleemid, kuna see võib olla palju vanem. See on parem sellisel juhul kasutada ajavahemiku, nagu allpool näidatud.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Saate määrata ajavahemik kogumiseks näidisandmeid. Soovi korral võite seda ka määrab protsent näidised, mis peab olema nõutud aja jooksul saadaval.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`tulemiks 20 proovi kui kõik näidised viimase kümme minutit CPUPercent ajalugu. Kui viimase hetke ajalugu ei ole saadaval, siiski ainult 18 näidised tuleks tagastada. Selles näites:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`ei, kuna ainult 90 protsenti näidised on saadaval.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`õnnestub.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Saate määrata ajavahemiku jaoks nii algusaeg ja lõpuaja andmete kogumine.<br/><br/>Nagu eespool, on vahel, kui valim on kogutud ja kui see on saadaval, kui soovite valemi. See pidada, kui kasutate funktsiooni `GetSample` meetod. Vt `GetSamplePercent` all.|
| GetSamplePeriod() | Tagastab näidised, mis võeti perioodi ajalooliste valimi andmehulgas. |
| Count() | Tagastab näidised koguarvu argumendil ajalugu. |
| HistoryBeginTime() | Tagastab ajatempel mõõdiku vanimast olemasolevad andmed proovi. |
| GetSamplePercent() |Tagastab protsent näidised, mis on saadaval teatud ajavahemiku jaoks. Näiteks:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Kuna selle `GetSample` meetod nurjub, kui protsent näidised tagastatud on väiksem kui `samplePercent` määratud, saate kasutada funktsiooni `GetSamplePercent` meetodi abil kontrollida esmalt. Seejärel saate alternatiivse toimingu sooritamine kui piisavalt näidised on olemas, ilma automaatse skaleerimise väärtustamise peatada.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Näidised, valimi protsenti ja *GetSample()* meetod

Autoscale valemist core toiming on tööülesande ja ressursside argumendil andmete hankimine ja muutke pool suurus, et andmeid. On oluline mõistmiseks, kuidas autoscale valemite suhtlevad mõõdikute andmeid, või "proovide."

**Näidised**

Paketi teenuse perioodiliselt *näidised* , tööülesannete ja ressursside mõõdikute ning on saadaval autoscale valemeid. Need näited on registreeritud iga 30 sekundi järel paketi teenuse. Siiski on tavaliselt mõned latentsus, mis põhjustab vahel, kui need näited on salvestatud, ja siis, kui nad on kättesaadav (ja saab lugeda) viivituse autoscale valemeid. Lisaks mitmesuguste tegurite tõttu nagu muude taristu probleeme võrgu- või, näidiseid ei salvestatud teatud aja jooksul. Selle tulemusena "puuduvad" näidised.

**Valimi protsent**

Kui `samplePercent` edastatakse soovitud `GetSample()` meetodit või `GetSamplePercent()` meetodit nimetatakse, võrdlus kokku *võimalike* arv paketi teenuse lindistatud näidiseid ja valimite tegelikult *saadaval* autoscale valem viitab "%".

Heitkem pilk on 10-minutilise kuuline ajavahemik, nagu näiteks. Kuna näidised salvestatakse iga 30 sekundi järel ajakavaga on 10 minuti jooksul oleks maksimaalne arv näidised, mis on salvestatud paketi 20 proovi (2 minutis). Siiski tõttu omast latentsus aruandluse korda või mõni muu probleem, Azure infrastruktuuri, võib olla ainult 15 proovi saadaolevad autoscale valem lugemisel. See tähendab, et 10-minutilise perioodi, ainult **75%** näidised, mis on salvestatud arv on tegelikult teie valem saadaval.

**GetSample() ja valimi vahemikud**

Autoscale valemeid ei kavatse kasvab ja kahanemine oma kaustu--sõlmed lisamise või eemaldamise sõlmed. Kuna sõlmed kulu raha, mida soovite tagada, et valemite kasutama nutikad analüüsi, mis põhineb piisavalt andmeid. Seetõttu soovitame trendid tüüp analüüsi oma valemites kasutamine. Seda tüüpi kasvab ja vähenda oma kaustu proovide *vahemiku* põhjal.

Kasutage selleks `GetSample(interval look-back start, interval look-back end)` tagastamiseks **vektorkuju** näited:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Kui eeltoodud rea hinnatakse paketi järgi, tulemuseks mitmesuguseid näidised, kui väärtuste. Näiteks:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Kui olete kogunud proovide vektorkuju, siis saate kasutada funktsioone nagu `min()`, `max()`, ja `avg()` tuletada kogutud vahemiku mõtestatud väärtusi.

Täiendava turvalisuse huvides saate jõustada Valemi väärtustamine *nurjumise* kui väiksem valimi teatud protsendi on saadaval teatud aja jooksul. Kui saate jõustada Valemi väärtustamine nurjumise, saate paluge Valemi väärtustamine täpsemaks lõpetada, kui määratud protsendini näidised pole saadaval--paketi ja tehakse pool suurus ei muutu. Määramiseks vaja protsendi proovide hindamise õnnestub, määrake see kolmas parameeter `GetSample()`. Siin on määratud 75% proovide nõue:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

See on oluline näidis-saadavus, alati kindlaks määrata ajavahemiku ning ilme-tagasi alguskellaaeg, mis on vanemad kui üks minut eelnevalt mainitud hilinemise tõttu. Selle põhjuseks kulub ligikaudu ühe minuti proovide levitamine süsteemis, seega näidised vahemikus `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` sageli ei ole saadaval. Uuesti, saate kasutada protsendi parameetri `GetSample()` jõustamine valimi kindla protsendi nõue.

> [AZURE.IMPORTANT] Me **soovitame** selle saate * *vältida tuginedes *ainult* `GetSample(1)` oma autoscale valemite **. See on, kuna `GetSample(1)` sisuliselt öeldakse, et teenusesse paketi "mulle on, olenemata sellest, kuidas kaua aega tagasi teil viimase valimi." Kuna see on ainult valim ning see võib olla mõne vanema valimi, seda ei pruugi suuremat pildi tehtud ülesande või ressursi olek. Kui kasutate `GetSample(1)`, veenduge, et see on osa suuremat lause- ja mitte ainult andmepunkti, millele teie valem viitab.

## <a name="metrics"></a>Mõõdikud

Saate kasutada nii **ressursside** ja **tööülesannete** mõõdikute valemi määratlemisel. Saate reguleerida sihtotstarbeline sõlmed kogumi alusel mõõdikute andmed, mida te saada ja hindate sihtrühma arv. Jaotisest [muutujate](#variables) kohal lisateabe saamiseks klõpsake iga meetermõõdustik.

<table>
  <tr>
    <th>Meetermõõdustik</th>
    <th>Kirjeldus</th>
  </tr>
  <tr>
    <td><b>Ressurss</b></td>
    <td><p><b>Ressursi mõõdikute</b> põhinevad mälu CPU ja läbilaskevõime kasutuse Arvuta sõlmed, samuti sõlmed arv.</p>
        <p> Nende teenuste määratletud muutujate on kasulik teha põhinevad sõlm arv.</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Nende teenuste määratletud muutujate on kasulik teha põhinevad sõlm ressursikasutus.</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Ülesanne</b></td>
    <td><p><b>Tööülesande mõõdikute</b> põhinevad ootel, ülesandeid, näiteks aktiivne, oleku ja lõpule viidud. Järgmised teenuse määratletud muutujad on kasulikud pool suurus kohanduste tööülesande mõõdikute põhjal:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Mõne autoscale valemi kirjutamist

Mõne autoscale valemi koostamine moodustab laused, mis ülal komponentide kasutamine, siis need laused täieliku valemisse ühendamine. Selles jaotises loome näide autoscale valem, mida saab teha mõned tegelike skaleerimise otsuseid.

Kõigepealt loome määratlemine meie uut autoscale valemi nõuded. Valem peaks.

1. **Suurendamine** target arvu Arvuta sõlmed pargis kui CPU hõivatus on kõrge.
2. **Vähendamiseks** sihtrühma Arvuta sõlmed pargis kui CPU hõivatus on väike arv.
3. Alati sõlmed **maksimaalne** arv piirata 400.

*Suurendamine* sõlmed ajal kõrge CPU hõivatus arvu, saate määratleda lause, mis kuvab kasutaja määratletud muutuja (`$totalNodes`), mis on 110 protsenti sõlmed, kuid ainult siis, kui praegune target arv minimaalse keskmise CPU hõivatus viimase 10 minuti jooksul on 70% kohal. Muul juhul kasutame sihtotstarbeline praegust väärtust.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

*Vähendamiseks* sõlmed ajal madala CPU arvu, järgmise lause meie valemis määrab sama `$totalNodes` muutuv 90 protsenti praegune target sõlmed kui keskmine CPU hõivatus viimase 60 minutit oli alla 20% on arv. Muul juhul kasutada praegust väärtust `$totalNodes` mida me täidetud ülaltoodud lauses.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Nüüd sihtotstarbeline Arvuta sõlmed on **maksimaalne** 400 target arvu piiramine:

```
$TargetDedicated = min(400, $totalNodes)
```

Siin on lõpule viidud valem:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Kohapeal on autoscale lubatud loomine

Uue kausta loomiseks autoscaling lubatud, saate kasutada üht järgmistest meetoditest:

**Paketi .net-i**

1. Saate luua [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx)pool.
1. Määrake atribuudi [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) `true`.
1. Määrake atribuudi [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) oma autoscale valemiga.
1. (Valikuline) Määrake atribuudi [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) (vaikimisi 15 minutit).
1. Kinnita rakenduskausta [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) või [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx).

**Paketi REST API-ga**

* [Lisa konto lisamine rakenduskausta](https://msdn.microsoft.com/library/azure/dn820174.aspx): Määrake selle `enableAutoScale` ja `autoScaleFormula` elementide REST API kutse konfigureerida automaatset mastaapimist on see loomisel.

Järgmised koodilõigu loob autoscale lubatud kohapeal on [Paketi .net-i] abil[ net_api] teek. Selle kausta autoscale valem määrab target arvu 5 esmaspäevast sõlmed ja 1 päeva nädala kohta. [Automaatse skaleerimise intervall](#automatic-scaling-interval) on seatud 30 minutit. Ja muud C# pikad selle artikli "myBatchClient" on õigesti lähtestatud eksemplari [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Lisaks paketi REST API ja .NET SDK, saate mis tahes muud [Paketi SDK-d](batch-technical-overview.md#batch-development-apis), [paketi PowerShelli cmdlet-käskude](batch-powershell-cmdlets-get-started.md)ja [Paketi CLI](batch-cli-get-started.md) autoscaling töötamiseks.

> [AZURE.IMPORTANT] Kui loote kohapeal on autoscale lubatud, tuleb teil **ei** määrata soovitud `targetDedicated` parameeter. Lisaks, kui soovite käsitsi suuruse muutmine kohapeal on autoscale lubatud (nt koos [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), siis peate esimese **Keela** automaatne mastaapimine pool, siis selle suurust muuta.

### <a name="automatic-scaling-interval"></a>Automaatse skaleerimise intervall

Vaikimisi, reguleerib paketi teenuse lisamine rakenduskausta suurus selle autoscale valemi järgi iga **15 minutit**. Ajavahemik on konfigureeritav, aga abil rakenduskausta järgmised atribuudid:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (pakett .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API)

Minimaalse vahe on viis minutit ja maksimumväärtus on 168 tundi. Kui määratud intervalli väljaspool seda vahemikku, tagastab paketi teenuse vigane päring (400) viga.

> [AZURE.NOTE] Autoscaling on praegu mõeldud reageerida vähem kui minutiga, kuid pigem on mõeldud kohandada oma puhvri järk-järgult, nagu on töökoormus käivitamist suurust.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Mõne olemasoleva kausta sisse autoscaling lubamine

Kui olete juba loonud on määratud arvu Arvuta sõlmed *targetDedicated* parameetrite abil, saate siiski lubada autoscaling kausta sisse. Iga paketi SDK pakub "luba autoscale" toimingut, näiteks:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (pakett .NET)
*  [Luba automaatne mastaapimine on] [ rest_enableautoscale] (REST API)

Kui lubate autoscaling mõne olemasoleva kausta sisse, järgmist:

* Kui automaatne skaleerimist on praegu **keelatud** pool kui te "luba autoscale" taotluse, *peab* teil määrata kehtiv autoscale valemi kui te taotluse. *Soovi korral* saate määrata intervalli autoscale hindamiseks. Kui te ei määra intervalli, kasutatakse vaikeväärtust, milleks on 15 minutit.

* Kui autoscale on praegu pool **lubatud** , saate määrata mõnda autoscale valemit, hindamise intervalli või mõlemad. Ei saa jätke mõlemad atribuudid.

  * Kui te ei määra uue autoscale hindamise intervall, olemasoleva hindamise ajakava on peatatud ja käivitatakse uue ajakava. Uue ajakava algus on aeg, kus "luba autoscale" kutse on välja antud.

  * Kui jätate kas autoscale valemi või hindamise intervall, paketi teenuse jätkuvalt kasutada seda sätet praegust väärtust.

> [AZURE.NOTE] Kui väärtus on määratud *targetDedicated* parameetri pool loomisel, ignoreeritakse kui automaatse skaleerimise valem hinnatakse.

See C# koodilõigu kasutab [Paketi .NET] [ net_api] teegi lubamiseks autoscaling mõne olemasoleva kausta sisse:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Mõnda autoscale valemit värskendamine

Sama "luba autoscale" taotluse *värskendada* valemi kasutamine autoscale lubatud olemasoleva kausta (nt koos [EnableAutoScale] [ net_enableautoscale] paketi .NET-is). Pole ühtegi teisiti "värskendada autoscale" toimingut. Näiteks kui autoscaling on juba aktiveeritud "myexistingpool" kui järgmine kood, selle autoscale valem on asendatud funktsiooniga sisu `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Autoscale intervalli värskendamine

Kui värskendamise mõnda autoscale valemit, saate kasutada sama [EnableAutoScale] [ net_enableautoscale] meetod olemasoleva autoscale lubatud puhvri autoscale hindamise intervalli muutmiseks. Näiteks, et seada autoscale hindamise intervall 60 minutit kausta, mis on juba autoscale lubatud:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Mõne autoscale Valemi väärtustamine

Enne rakendamist on saate valemi väärtustada. Nii saate teha on "testi run" näha, kuidas oma laused hinnata enne sellele valem tootmisse valemi.

Mõne autoscale Valemi väärtustamine peate esimese **lubamine autoscaling** rakenduskausta **kehtiv valemi**abil. Kui soovite valemi klõpsake kausta, mis pole veel lubatud autoscaling testida, saate ühele reale valem `$TargetDedicated = 0` kui lubate esmalt autoscaling. Seejärel kasutage ühte järgmistest hindamaks, kui valem, mida soovite testida:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) või [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Nende meetodite paketi .net-i jaoks on vaja ID mõne olemasoleva kausta ja string, mis sisaldab valemit autoscale hinnata. Hindamise tulemused sisalduvad tagastatud [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) eksemplari.

* [Mõni automaatse skaleerimise Valemi väärtustamine](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    Määrake REST API taotlus, rakenduskausta ID on URI ja autoscale valem *autoScaleFormula* elemendi koosolekukutse sisusse. Selle toimingu vastuse sisaldab tõrgete teave, mis võib olla seotud valem.

Klõpsake selle [Paketi .NET] [ net_api] koodilõigu, saame hinnata valemi enne rakendamine [CloudPool][net_cloudpool]. Kui kausta pole lubatud autoscaling, saame lubada esmalt.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Eduka hindamisest valemit selles koodilõigu põhjustab väljund sarnaneb järgmisega:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Autoscale käivitatakse kohta teabe saamine

Teie valem toimib ootuspäraselt tagamiseks soovitame perioodiliselt autoscaling "käivitatakse" paketi sooritamine oma rakenduskausta tulemused. Nii, et saada (või värskendamine) viide pool, ning uurida atribuutide selle viimase autoscale käivitada.

Paketi .net-i [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) atribuut sisaldab teavet uusimate automaatse skaleerimist käitamist toimingud paketi teenus võttis pool mitu atribuudid.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

REST API-ga [on kohta teabe saamiseks](https://msdn.microsoft.com/library/dn820165.aspx) taotluse tagastab kausta, mis sisaldab uusim automaatset mastaapimist käivitamiseks teabe [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun)teavet.

Järgmised C# koodilõigu kasutab paketi .NET teegi prinditakse viimase autoscaling, käivitada kausta "myPool" teave:

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Eelmise koodilõigu väljundi näidis:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Autoscale näidisvalemid

Vaatame pilk mõne valemeid kuvada erineval viisil, et reguleerida klahvivajutuse Arvuta ressursside pool.

### <a name="example-1-time-based-adjustment"></a>Näide 1: Ajapõhiste kohandamine

Võib-olla soovite kausta suuruse soovitud nädalapäeva ja kellaajal suurendamiseks või vähendamiseks sõlmed kogumi arvu vastavalt sellele.

Seda valemit saab esmalt praeguse kellaaja. Kui see on mõne weekday (1-5) ja tööaeg (8 18: 00), seatakse 20 sõlmed pool sihtformaat. Muul juhul väärtuseks 10 sõlmed.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Näide 2: Toimingupõhise kohandamine

Selles näites on kohandatud toimingud järjekorras kuhjuda mitmeid pool suurus. Pange tähele, et nii kommentaarid kui ka reapiirid on lubatud valemi stringide.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Näide 3: Raamatupidamine paralleelselt toimingute tegemiseks.

See on veel üks näide, mis mõõtmeid kausta Tööülesanded arvu põhjal. Järgmine valem võtab arvesse ka [MaxTasksPerComputeNode] [ net_maxtasks] väärtus, mis on mõeldud pool. See on eriti kasulik olukordades, kus [paralleelselt ülesande täitmine](batch-parallel-node-tasks.md) on lubatud teie pool.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Näide 4: Säte on algse pool suurus

Selles näites on kujutatud C# koodilõik autoscale valemiga, mis määrab pool suurus sõlmed arvu esialgse aja aja jooksul. Seejärel seda reguleerib pool suurus töötab arvu põhjal ja aktiivne toimingud esialgne ajaperioodi möödumist.

Koodilõigu järgmist valemit:

- Määrab neli sõlmed algse pool suurus.
- Muutke pool suurus soovitud rakenduskausta elutsükli esimese 10 minuti jooksul.
- Pärast 10 minutit, hangib maksimaalne arv väärtus töötama ning aktiivsed tööülesanded viimase 60 minutit.
  - Kui mõlemad väärtused on 0 (, mis näitab, et ükski toiming oli töötava või aktiivne viimase 60 minutit), pool suurus väärtuseks 0.
  - Kui kumbki väärtus on suurem kui null, ei muutu on muudetud.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Järgmised sammud

* [Azure'i paketi maksimeerimine arvutada ressursikasutus samaaegseid sõlm ülesannetega](batch-parallel-node-tasks.md) sisaldab kohta, kuidas saate käivitada mitme tööülesannete korraga Arvuta sõlmed olevate. Lisaks autoscaling, funktsioon võib aidata vähendada töö kestus mõned töökoormus, raha säästa.

* Teise tõhusust korduva, veenduge, et paketi rakenduse päringute paketi teenuse kõige optimaalse. Klõpsake [päringu Azure'i paketi teenuse tõhus](batch-efficient-list-queries.md), saate teada, kuidas piirata andmehulga, mis ületab kaabel, kui teete päringu potentsiaalselt tuhandetele Arvuta sõlmed või tööülesannete olekut.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
