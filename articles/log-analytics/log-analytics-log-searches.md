<properties
    pageTitle="Logige otsingud Log Analytics | Microsoft Azure'i"
    description="Log otsingud võimaldavad ühendamiseks ja oleksid mis tahes seadme andmeid mitmest allikast teie keskkonnas."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>Log otsinguid Log Analytics

Log Analytics keskmes on log otsingufunktsiooni, mis võimaldab teil ühendamiseks ja oleksid mis tahes seadme andmeid mitmest allikast teie keskkonnas. Lahenduste on ka tootja log otsingu mõõdikute tuua kallutatava ümber kindla probleemi lahendamiseks ala.

Lehel, saate luua päringu ja seejärel kui otsite, saate filtreerida tulemid tahukas juhtelementide abil. Samuti saate luua täpsemaid päringuid transformatsioon, filtri ja aruande tulemused.

Levinud log otsingupäringuid kuvatakse enamik lahenduse lehtedel. Kogu OMS konsooli, saate nuppu paanid või muude üksuste üksuse üksikasjade kuvamiseks log otsingu abil hulgidiagrammis.

Selles õpetuses juhendame näited katmiseks kõik põhitõed, kui kasutate Logi otsing.

Vaatame alustamine lihtne, praktiline näited ja siis koostada neile nii, et saaksite praktilise kasutamise juhtumite kohta, kuidas kasutada järgmist süntaksit ekstrakti andmed muude võimalustega, mida soovite mõistmine.

Kui olete tuttav otsingu võtteid, saate vaadata [Log Analytics logige otsing ja teatmematerjalid](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Kasutage Põhifiltrid

Esmalt teadma on esimene osa otsingu päringu enne "|" vertikaalne püstkriips, on alati *filter*. Mõelge selle WHERE-klausli TSQL--määrab *mis* tõmmake OMS andmesalve andmete alamhulga. Otsida andmesalve on suuresti kohta andmed, mida soovite eraldada, nii et see on loomulikus, et päring oleks alustada WHERE-klausli omadusi.

Saate kasutada kõige Põhifiltrid on *märksõnad*, näiteks "tõrge" või 'ajalõpp' arvuti nimi. Järgmist tüüpi lihtpäringuid tagastada üldiselt mitmesuguse kujundite andmete sama tulemite hulk. Selle põhjuseks on Log Analytics on eri *tüüpi* süsteem.


### <a name="to-conduct-a-simple-search"></a>Lihtsa otsingu abil
1. OMS portaalis, klõpsake nuppu **Log Otsi**.  
    ![Otsingu paan](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Tippige väljale päringu `error` ja seejärel klõpsake nuppu **Otsi**.  
    ![Otsingu tõrge](./media/log-analytics-log-searches/oms-search-error.png)  
    Näiteks päring `error` järgmisel pildil tagastatud 100000 **sündmuse** kirjete (kogutud haldamine), 18 **ConfigurationAlert** (loodud konfiguratsiooni Assessment) ja 12 **ConfigurationChange** kirjete (jäädvustatud muutuste jälitamise).   
    ![Otsingutulemid](./media/log-analytics-log-searches/oms-search-results01.png)  

Neid filtreid ei ole eriti objekti tüübid/tunnid. *Tüüp* on lihtsalt sildi või atribuut või string/nimi/kategooria, mis on seotud andmete tükk. Mõned dokumendid süsteemi on sildistatud nimega **Tüüp: ConfigurationAlert** ja mõned on sildistatud **Tüüp: täiuslik**, või **Tippige: sündmuse**jne. Iga otsingutulemi, dokumendi, kirje või kirje kuvab töötlemata atribuutide ja nende väärtuste iga nende andmeühikuga ja abil saate välja nimed määrata filtri, kui soovite alla laadida ainult need kirjed, mille välja on esitatud väärtuse.

*Tüüp* on väga lihtsalt välja, mis on kõik kirjed, mitte erineb mõne muu välja. See loodi põhjal välja tüüp väärtus. Seda kirjet on erinevate kujul või. Lisaks **Tüüp = täiuslik**, või **Tüüp = sündmuse** on ka süntaks, peate saate teada, kuidas päringu tulemustega seotud andmete või sündmuste jaoks.

Pärast välja nimi ja enne väärtust saate kasutada koolon (:) või võrdusmärk (=). **Tüüp: sündmus** ja **Tüüp = sündmuse** on samaväärne tähendust, saate valida soovitud laadi, saate soovi korral.

Jah, kui tüüp = täiuslik kirjed on väljal nimega "CounterName", siis päring, mis sarnaneb kirjutamise `Type=Perf CounterName="% Processor Time"`.

See annab teile tulemustega seotud andmete kus jõudluse counter nimi on "% protsessori aja".

### <a name="to-search-for-processor-time-performance-data"></a>Protsessor aja jõudluse andmete otsimiseks
- Tippige otsinguväljale päring`Type=Perf CounterName="% Processor Time"`

Saate täpsemalt ja kasutada **InstanceName = _ "Kokku"** päringu, mis on vastuolus Windowsi jõudlus. Võite valida ka mõne tahukas ja teine **väärtuseks:**. Filtri lisatakse automaatselt filtrisse päringu ribal. Järgmisel pildil saavad seda vaadata. See näitab, kuhu lisamiseks klõpsake **InstanceName: '_Total'** päringule midagi tippimata.

![Otsingu tahukas](./media/log-analytics-log-searches/oms-search-facet.png)

Nüüd saab päringu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

Selles näites, pole teil määrata **Tüüp = täiuslik** saada järgmise tulemi. Kuna CounterName ja InstanceName väljad on olemas ainult kirjete tüüp = täiuslik, päring, mis on seotud sama tulemuse kui kauem, eelmise:
```
CounterName="% Processor Time" InstanceName="_Total"
```

Seda sellepärast, et kõik filtrid päring väärtustata olevaks *ja* omavahel. Tõhus, veel välju lisada kriteeriumid, kuvatakse väiksem, teatud rafineeritud tulemused.

Näiteks päringu `Type=Event EventLog="Windows PowerShell"` on identne `Type=Event AND EventLog="Windows PowerShell"`. Tagastab kõik sündmused, mis on sisse logitud ja kogutud sündmuselogi Windows PowerShelli kaudu. Kui lisate filtri mitu korda, valides korduvalt sama tahukas, siis probleem on vaid kosmeetilised – see võib tarbetu otsinguriba, kuid endiselt tagastab sama tulemuse, kuna peidetud tehtemärki on alati olemas.

Saate hõlpsasti tühistada peidetud tehtemärki ei tehtemärk konkreetselt abil. Näiteks:

`Type:Event NOT(EventLog:"Windows PowerShell")`või selle kestvus `Type=Event EventLog!="Windows PowerShell"` tulu kõik logid, mis pole Windows PowerShelli Logi kõik sündmused.

Või kasutage teiste kahendmuutujaga tehtemärk nagu "OR". Järgmine päring tagastab kirjed, mille on EventLog on kas rakenduse või süsteem.

```
EventLog=Application OR EventLog=System
```

Ülaloleval päringul kasutamisel saate kirjeid nii logide sama tulemuse määramine.

Juhul, kui eemaldate või jättes selle peidetud ja kohas, seejärel järgmine päring ei tooda üheski tulemuses kuna seal pole nii logid kuuluva kirjet. Sündmuste logi iga kirje on ainult üks kahest logid kirjutatud.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Täiendavate filtrite kasutamine

Järgmine päring tagastab kirjed 2 sündmuselogide kõik arvutid, mis on saadetud andmed.

```
EventLog=Application OR EventLog=System
```

![Otsingutulemid](./media/log-analytics-log-searches/oms-search-results03.png)

Valige üks väljad või filtrite ahendab kindla arvutiga, välja arvatud muid lisandmooduleid ise päring. Tulemiks oleva päringu oleks näeb välja järgmine.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Mis on võrdne järgmist peidetud ja tõttu

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Iga päringu hinnatakse konkreetsete järgmises järjestuses. Pange tähele sulg.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Sündmuste logi välja nagu saate tuua andmeid ainult teatud arvutite, lisades või. Näiteks:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Samuti see järgmine päring tagastab **% CPU aega** ainult valitud kaks arvutit.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Toetatud brauserid
Kuupäeva ja kellaaja ja arvulised väljad, saate otsida väärtused, mis on *suurem kui*, kasutades *vähem kui*, ja *väiksem või võrdne*. Saate kasutada näiteks lihtsa tehtemärke >, <>; =, < =,! = päringu otsinguriba.


Teatud sündmuste logi saab päringud teatud aja jooksul. Näiteks on viimase 24 tunni väljendatud koos Mnemoonika järgmine avaldis.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Kahendmuutuja abil otsimine
- Tippige otsinguväljale päring`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![kahendmuutuja otsing](./media/log-analytics-log-searches/oms-search-boolean.png)

Kuigi saate määrata ajavahemik graafiliselt ja kõige korda võite seda teha, on eeliseid, sh aeg filtri otse päringu. Näiteks see parim lahendus armatuurlaudade, kus saate alistada iga paani sõltumata *globaalne* kellaaeg valija armatuurlaualehe aeg. Lisateavet leiate teemast [Ajal küsimustega armatuurlaua](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Kellaaja alusel filtreerimine, pidage meeles tulemuste saamiseks *ühisosa* kahe kellaajaperioodi: määratud OMS portaalis (S1) ja määratud päringu (S2).

![ühisosa](./media/log-analytics-log-searches/oms-search-intersection.png)

See tähendab, et kui ajad ei kattuks, näiteks kui valite **sel nädalal** ja päringu määratleda **Eelmine nädal**, siis on pole ühisosa ning te ei saa üheski tulemuses OMS portaalis.

Võrdluse tehtemärgid TimeGenerated välja jaoks kasutatud on ka muudel juhtudel kasulik. Näiteks koos arvulised väljad.

Näiteks valemi, et konfiguratsiooni Assessment teatiste on raskusaste järgmised väärtused:

- 0 = teave
- 1 = hoiatus
- 2 = kriitiline

Saate päringu hoiatus- ja kriitilised teatiste ja jätta ka informatiivsed need järgmine päring.

```
Type=ConfigurationAlert  Severity>=1
```


Samuti saate vahemiku päringud. See tähendab, et saate sisestada järjestikuseks väärtuste vahemiku algus ja lõpp. Näiteks, kui soovite Toiminguhalduri sündmuselogi, kus on EventID on suurem kui või võrdne 2100, kuid pole suurem kui 2199 sündmusi, järgmine päring oleks tagastab need.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] Kasutage vahemiku süntaks on koolon (:) väärtuseks: eraldaja ja *pole* võrdusmärk (=). Ümbritsege vahemiku alumise ja ülemise lõpu nurksulgudega ja eraldamiseks kahe perioodi (.).

## <a name="manipulate-search-results"></a>Otsingutulemite töödelda

Kui otsite andmed, peaksite täpsustada päringu ja on hea kontrolli tulemusi. Kui tulemused alla laaditakse, saate rakendada käsud neid muuta.

Käsud Log Analytics otsingud *peab* tehke pärast vertikaalne püstkriips (|). Filtri tuleb alati päringustringi esimene osa. See määratleb andmehulga, millega töötate ja seejärel "torud" nende tulemite käsu sisse. Seejärel saate toru lisada täiendavaid käske. See on sarnane lõdvalt Windows PowerShelli kohaletoimetamisel.

Üldiselt Log Analytics otsingu keele proovib PowerShelli laad ja juhised teha sarnane IT-spetsialistid ja hõlbustada õ kõvera jälgimine.

Käsud on verbide nimed nii, et teil hõlpsasti kindlaks teha, mida nad teevad.  

### <a name="sort"></a>Sortimine

Käsk sordi võimaldab teil määrata ühe või mitme välja alusel sortimisjärjestuse. Isegi juhul, kui te ei kasuta seda vaikimisi, jõustatakse kellaaja laskuvas järjestuses. Viimati tehtud tulemid on alati otsingutulemite ülaosas. See tähendab, et kui käivitate otsingu, `Type=Event EventID=1234` mis tegelikult täidetakse teie jaoks on:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Mis on, sest see on tüüp, on teile tuttavad logid kogemus. Näiteks rakenduses Windowsi Sündmusevaatur.

Saate muuta nii tulemused tagastatakse sortimine. Järgmised näited kujutavad seda, kuidas see toimib.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Lihtne ülaltoodud näidetes käsud tööpõhimõtted--need saadud tulemused vastavad filtri tagastatud kuju muuta.

### <a name="limit-and-top"></a>Piiratud ja algusse
Mõne muu vähem tuntud käsk on piiratud. Tegusõna PowerShelli nagu lubatud. Lubatud funktsionaalselt identne ülalt käsu. Järgmised päringud tagastavad sama tulemuse.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Kasutades algusse
- Tippige otsinguväljale päring`Type=Event EventID=600 | Top 1`   
    ![Otsingu algusse](./media/log-analytics-log-searches/oms-search-top.png)

Ülaltoodud pildil on 358 tuhat kirjetega EventID = 600. Väljad, omadustega ja filtrid vasakul kuvatakse alati teave tulemuste tagastatud *filtri osa,* päringu, mis on osa enne mis tahes püstkriips. **Otsingutulemite** paan tagastab ainult viimase 1 tulemi, kuna näiteks käsu kujuga ja ümber tulemused.

### <a name="select"></a>Valige

Valige käsk toimib nagu PowerShelli objekti valimine. Tagastab see filtreerimistulemitesse, mis ei ole kõigi nende algse atribuudid. Selle asemel valib ainult teie määratud atribuudid.

#### <a name="to-run-a-search-using-the-select-command"></a>Valige käsk otsingu käivitamiseks

1. Tippige otsinguväljale `Type=Event` ja seejärel klõpsake nuppu **Otsi**.
2. Klõpsake nuppu **+ Kuva veel** üks tulemid on atribuutide vaatamiseks.
3. Valige need selgesõnaliselt ja päringu muutub `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![valige Otsing](./media/log-analytics-log-searches/oms-search-select.png)

See on käsk, mis on eriti kasulik, kui soovite määrata otsingu väljundi ja valige ainult osade tegelikult küsimus uurimiseks, mis pole sageli täielik kirje andmeid. See on kasulik, kui eri tüüpi kirjed on *mõned* ühised atribuudid, kuid mitte *kõigi* nende atribuutide on levinud. Funktsiooni, mis näeb välja rohkem loomulikult tabeli loomiseks või saate töötada ka siis, kui eksporditud CSV-faili ja seejärel masseerida Excelis.



## <a name="use-the-measure-command"></a>Käsuga mõõt

MÕÕT on üks Log Analytics otsinguid mitmekülgseimat käsud. See võimaldab teil rakendada statistilised *funktsioonid* teie andmeid ja antud välja alusel rühmitatud liitväärtuse tulemused. On mitu statistilised funktsioonid, mis toetab mõõt.

### <a name="measure-count"></a>Mõõtke count()

Statistilised funktsiooni töötamine ja üks lihtsaim mõista on funktsiooni *count()* .

Nagu mis tahes päringu tulemused `Type=Event`, nimetatakse ka omadustega otsingutulemite vasakus servas filtrite kuvamine. Täidetud otsingu filtrid jaotumine väärtused antud väli tulemuste kuvamine

![Otsingu meetme loendamine](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Näiteks ülaltoodud pildil näete **arvuti** välja ja see näitab, et peaaegu 739 tuhat sündmuste tulemuste sees pole 68 kordumatu ja erinevate väärtuste **arvuti** välja vastavad kirjed. Kuvatakse paan näitab ainult ülemise 5, millised on kõige levinum 5 väärtused, mis on kirjutatud **arvuti** väljad), sorditud dokumendid, mis sisaldavad selle välja kindla väärtuse arvuga. Pildil näete, et – need peaaegu 369 tuhat sündmuste – vahel 90 tuhat OpsInsights04.contoso.com arvutis, 83 tuhat DB03.contoso.com arvutist pärit jne.


Mida teha, kui soovite näha kõiki väärtusi, kuna paani kuvatakse ainult Ülemiste 5?

Mis on käsk mõõt saate teha funktsiooni count(). See funktsioon ei kasuta parameetrid. Teie määratud lihtsalt välja, mille alusel soovite rühmitada sel juhul – **arvuti** välja alusel:

`Type=Event | Measure count() by Computer`

![Otsingu meetme loendamine](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Kuid **arvuti** on vaid mõne väli, mida kasutatakse *rakenduses* andmete – iga tekstilõigu ei ole relatsiooniandmebaasid ja seal on pole eraldi **arvuti** objekti, mis tahes. Lihtsalt soovitud väärtusi *sisse* andmeid saab kirjeldada, mis üksus loodud neile ja mitmesuguseid muid omadusi ja aspektide andmed – seega Termini *tahukatega*. Siiski saate rühmitada sama hästi muude väljade. Kuna algse tulemuste peaaegu 739 tuhat sündmused, mis on veetorustiku mõõt käsk on ka **EventID**välja, saate rakendada sama võtet välja alusel rühmitamine ja saada arv Võistlusalad vastavalt EventID:

```
Type=Event | Measure count() by EventID
```

Kui olete huvitatud tegelik kirjete arvu, mis sisaldavad kindla väärtuse, kuid selle asemel saate kui soovite ainult väärtused ise loendit, *Valige* käsk selle lõpus lisada ja valige lihtsalt esimene veerg.

```
Type=Event | Measure count() by EventID | Select EventID
```

Seejärel saate hankida rohkem keerulisi ja Sortige eelnevalt päringu tulemused või võite klõpsata koordinaatvõrgu veerud, liiga.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Kasutades mõõt loendamine

- Tippige otsinguväljale päring`Type=Event | Measure count() by EventID`
- Lisa `| Select EventID` päringu lõppu.
- Lõpuks lisa `| Sort EventID asc` päringu lõppu.


On paar teade ja rõhutada olulisi punkte.

Esmalt, näete tulemusi ei ole selle algse tulemused enam. Nad saavad liidetud tulemused – sisuliselt tulemuste rühmad. See ei probleem, kuid peaksite aru saama, et olete suheldes andmeid, mis erineb oluliselt erinev kujundi algse töötlemata kujundi pealt koondamine/statistika funktsiooni tulemusena loodud saab.

Teiseks **mõõt count** praegu tagastab ainult 100 ülemist erinevate tulemused. See piirang ei kehti statistilised funktsioonid. Jah, tavaliselt peate kasutama täpsema filtri kõigepealt enne rakendamist mõõt count() kindlate üksuste otsimine.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Kasutada funktsioone min ja max mõõt käsk

On erinevad stsenaariumid, kus **Mõõt Max()** ja **Mõõt Min()** on kasulik. Siiski iga funktsiooni on vastupidist üksteise, meil kuvatakse kirjeldavad Max() ja võite proovida Min() ise.

Kui saate päringu turvalisus sündmuste jaoks, on neil **taseme** atribuut, mis võivad erineda. Näiteks:

```
Type=SecurityEvent
```

![Otsingu meetme count algus](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Kui soovite vaadata suurim väärtus kõik turvalisus sündmused, mis on antud levinud arvuti, Rühmita välja, saate kasutada

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![Otsingu meetme max arvuti](./media/log-analytics-log-searches/oms-search-measure-max02.png)

See Kuva, et oli **taseme** kirjete arvutites enamikel on vähemalt tasemel 8, paljud oli 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Otsingu meetme max aja loodud arvuti](./media/log-analytics-log-searches/oms-search-measure-max03.png)

See funktsioon töötab ka arve, kuid see töötab ka kuupäeva ja kellaaja väljad. On kasulik, et otsida mis tahes andmeüksus indekseeritud iga arvutis viimase või viimase ajatemplit. Näide: kui viimase turvalisus juhtumist teatati iga seadme jaoks?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Funktsioon avg mõõt käsu kasutamine

Kasutatud mõõduga Avg() statistika funktsioon võimaldab teil mõnes valdkonnas ja rühma tulemused sama või välja keskmise väärtuse arvutamiseks. See on kasulik juhul, nt jõudlusandmeid erinevaid.

Alustuseks kirjeldame jõudlusandmeid. Pange tähele, et OMS praegu kogub jõudluse hinnale nii Windows ja Linux masinad.

*Kõigi* tulemustega seotud andmete otsimiseks kõige olulisemaid päring on:

```
Type=Perf
```

![avg Käivita otsing](./media/log-analytics-log-searches/oms-search-avg01.png)

Märkate esmalt on Log Analytics näitab teile, kolm perspektiivid: loend, mis näitab teile, mis kuvatakse tegelike kirjete taha diagrammid; Tabel, mis kuvab tabeli vaate jõudlus andmete; ja mõõdikute, mis näitab diagrammide jaoks jõudluse hinnale.

Ülaltoodud pildil on kahte tüüpi märgitud väljad, mis näitavad järgmist:

- Esimese komplekti tuvastab Windowsi jõudlus Counter nimi, objekti nimi ja eksemplari nimi päringu filter. Need on ilmselt kõige sagedamini kasutate omadustega/filtritena väljad
- **Põhimõttelinegi** on näidiku tegelik väärtus. Selles näites on väärtus *75*.
- **TimeGenerated** on 12:51, 24-tunnise kellaajavormingu.

Siit leiate ülevaate mõõdikute graafiku.

![avg Käivita otsing](./media/log-analytics-log-searches/oms-search-avg02.png)

Pärast lugedes täiuslik kirje kujundit ja muude otsingu tehnikate kohta lugenud, saate seda tüüpi arvandmeid liitmine mõõt Avg().

Siin on lihtne näide:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![avg samplevalue otsimine](./media/log-analytics-log-searches/oms-search-avg03.png)

Selle näite puhul valige CPU koguaeg jõudluse vastuolus ja Keskmine arvuti. Kui soovite oma ainult viimase 6 tunni tulemuste piiritlemiseks, saate aja filtri juhtelemendi kasutamist või määrata päringu järgmiselt:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Funktsioon avg kasutades mõõt käsk
- Tippige väljale Otsi päringu `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


Saate liitmine ja oleksid andmed *üle* arvutites. Oletagem näiteks, hosts serveripargi, kus iga sõlm on mis tahes muud üks ja need lihtsalt kõik sama tüüpi tööd ja koormusetasakaalustusega olema umbes mingi kogumi olemasolu. Võite oma hinnale kõik ühes minge järgmise päringu ja keskmiste saamiseks kogu serveripargis. Saate alustada, valides arvuti järgmises näites:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Nüüd, kui teil on arvutid, soovite ka ainult valida kahe juhtimismõõdikud (KPI-d): CPU hõivatus – % vaba kettaruumi. Jah, see osa päringu muutub:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Nüüd saate lisada arvutite ja hinnale koos järgmises näites:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Kuna teil on väga konkreetse valiku, **mõõt Avg()** käsk võib tagastada keskmise mitte arvutis, vaid kogu serveripargis, lihtsalt järgi rühmitamine CounterName. Näiteks:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

See annab teile kasulik kompaktne vaade paar teie keskkonnas KPI-d.

![Otsingu avg rühmitamine](./media/log-analytics-log-searches/oms-search-avg04.png)


Saate hõlpsasti otsingupäringu armatuurlaua. Näiteks võib otsingu päringu salvestamiseks ja loomine armatuurlaua selle nimega *Web serveripargi KPI-d*. Armatuurlaudade kasutamise kohta leiate lisateavet teemast [kohandatud armatuurlaua Log Analytics loomine](log-analytics-dashboards.md).

![Otsingu avg armatuurlaud](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Funktsiooni sum kasutamine käsuga mõõt

Funktsiooni sum on sarnane käsk mõõt funktsioone. Saate vaadata näide selle kohta, kuidas kasutada funktsiooni sum [W3C IIS-i logid otsingu Microsoft Azure'i töö ülevaated](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Saate arve, kuupäeva korda ja tekstistringid Max() ja Min(). Tekstistringid, nad sorditakse tähestikulises järjestuses ja saate esimene ja viimane.

Siiski ei saa kasutada usesthe SUM midagi muud kui arvulised väljad. See kehtib ka Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Funktsioon percentile mõõt käsu kasutamine

Funktsioon percentile on sarnane Avg() ja usesthe SUM, et saate seda kasutada ainult arvulised väljade jaoks. Saate kasutada mis tahes protsentiili 1 – 99 arvväärtusega välja vahel. Samuti saate nii **protsentiili** ja **pct** käsud. Siin on mõned näited:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Kasutage where käsk

Funktsiooni kui käsk toimib nagu filtri, kuid seda saab kasutada müügivõimaluste põhjalikumaks filtreerimiseks liidetud tulemid, mille on koostanud mõõt käsu – mis on erinevalt tulemused päringu alguses filtreeritud.

Näiteks:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Saate lisada mõne muu Triibu "|" märgi ning kus käsk saada ainult arvutites, mille keskmine CPU on suurem kui 80%, järgmises näites:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Kui olete tuttav Microsoft System Center - Toiminguhalduri, võite funktsiooni, kus käsk juhtimise pack osas. Kui näiteks reegli, esimene osa päring oleks andmeallika ja kus käsk oleks tuvastamise tingimus.

Saate kasutada päringu paanina **Minu armatuurlaud**, kuvarit sorditakse, kui arvuti protsessorid on kasutatud liiga. Armatuurlaudade kohta leiate lisateavet teemast [kohandatud armatuurlaua Log Analytics loomine](log-analytics-dashboards.md). Saate luua ja kasutada armatuurlaudu mobiilirakenduse kaudu. Lisateavet leiate teemast [OMS Mobile'i rakendus ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). Järgmisel pildil kaks paanid, saate vaadata jälgida kuvatakse loendi ja arvuna. Sisuliselt soovite alati arv on null ja tühi loend. Muul juhul see tähistab tingimuses teatis. Kui vaja, saate seda vaadata kell masinad, mis on rõhu all.

![mobiilse armatuurlaud](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Tehtemärk in kasutamine

*Klõpsake* tehtemärk, *Mitte* koos võimaldab kasutada subsearches, mis on otsinguid, mis sisaldavad uue otsingu argumendina. Need sisalduvad looksulgudega uue * *põhi* - või* otsingu sees. Tulemus subsearch, sageli loendi erinevate tulemuste saamiseks kasutatakse argumendina esmane vasteid.

Saate subsearches sobitada, et te ei saa kirjeldada otse otsingu avaldis, kuid mis saab luua otsingu andmete kohta. Näiteks kui olete huvitatud ühe otsingu abil leida kõikide sündmuste arvutitest *puuduvad turbevärskendusi*, siis peate kujundamise subsearch, tuvastamaks esmalt *arvutisse puuduvad turbevärskendusi* enne, kui see leiab sündmused, mis kuuluvad nende hosts.

Võib kiire *arvutisse praegu puuduvad nõutavad turbevärskendusi* koos järgmist päringut:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![Otsingu NÄITES](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Kui teil on loend, abil saate otsingu nimega on sisemine otsida arvutite loendi siseneda väline (primaarne) otsing, mis otsib sündmuste jaoks nendest arvutitest. Saate seda teha, lisades sisemine otsida looksulgudega ja söötmine tulemuste nimega/filtrivälja väline Otsing abil tehtemärk IN võimalikud väärtused. Päringu sarnaneb:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![Otsingu NÄITES](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Teatis ka aeg filtri kasutatakse sisemine otsida, sest süsteemi värskenduse hindamine võtab kõik arvutid hetktõmmise kord 24 tunni jooksul. Saate sisemise päringu veel kerge ja täpne otsides ainult päeva. Väline otsingu asemel kasutab aeg valiku kasutajaliides, toomine sündmuste viimase 7 päeva. Kohta leiate teavet [toetatud brauserid](#boolean-operators) rohkem aega tehtemärke.

Kuna te tõesti ainult kasutamine filtrina sisemine Otsingu tulemused väärtus väline üks, saate ikka rakendada väline otsing käske. Näiteks saate endiselt rühmitada ülaltoodud sündmuste teise mõõt käsuga:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![Otsingu NÄITES](./media/log-analytics-log-searches/oms-search-in03-revised.png)


Üldiselt soovite sisemise päringu kiiresti käivitada, kuna Log Analytics on teenuse-side ajalõpud seda ja ka väike tulemite tagastamiseks. Kui sisemine päring tagastab Täpsemate tulemuste, tulemiloendi saab kärbitakse, mis võivad põhjustada vale tulemeid väline otsing.

Teise reegli on, et sisemine otsida praegu anda *liidetud* tulemusi. Teisisõnu, peab sisaldama käsk *mõõt* ; praegu ei saa siseneda on väline Otsingu tulemused.

Samuti ei saa olla ainult üks IN tehtemärk ja peab olema viimase filter päringu. Mitme IN tehtemärgid ei saa või sooviksite – see põhiosas takistab töötab mitu subsearches: oluline on, et ainult üks sub/sisemine otsing on võimalik, et iga väline otsing.

Isegi kui tasute need piirangud IN võimaldab mitut tüüpi seotud otsingute ja võimaldab määratleda midagi sarnast nagu arvutid, rühmad kasutajate või failide – mis tahes andmete väljad sisaldavad. Veel näited:

**Kõik värskendused puuduvad arvutitest, kus automaatvärskenduste säte on keelatud.**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Kõigi tõrge sündmuste arvutitest, mis töötab SQL serveri (=, kus SQL-i hindamise käivitamist)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Kõigi turvalisuse sündmuste arvutitest, kus on domeenikontrollerid (=, kus AD Assessment on käivitamine)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Mis muudelt kontodelt on sisse logitud sama arvutites, kus on sisse logitud konto BACONLAND\jochan?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Erinevate käsuga

Juba nime, leiate selle käsu erinevate väärtuste loendi välja jaoks. See on üllatavalt lihtne, kuid väga kasulik. Sama saavutada mõõt count() käsuga samuti, nagu allpool näidatud.

```
Type=Event | Measure count() by Computer
```

![ERINEVATE käsk näide](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Kui kõik, mida olete huvitatud aga lihtsalt loendi erinevate väärtuste ja pole dokumente, millel on väärtused ja seejärel DISTINCT saate sisestada arvu puhtam ja lihtsam lugeda väljundi ja lühemaks süntaks, nagu allpool näidatud.

```
Type=Event | Distinct Computer
```
![ERINEVATE käsk näide](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Funktsiooni countdistinct mõõt käsu kasutamine
Countdistinct funktsioon loendab igas rühmas erinevate väärtuste arv. Näiteks võib kasutada loendada iga aruandlus arvuteid:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Käsuga mõõt intervall
Lähedal reaalajas jõudluse andmete kogumine, saate kogumine ja visualiseerimine mis tahes Jõudluseloenduri Log Analytics. Päringu **Tüüp: täiuslik** , sisestades tagastada tuhandeliste argumendil graafikute põhjal hinnale ja Log Analytics keskkonna serverite arv. Nõudmisel argumendil koondamine, kus saate vaadata üldine mõõdikute kõrge ja sügav sukelduda täpsema andmed, kui teil on vaja teie keskkonnas.

Oletame, et soovite teada, mis on Keskmine CPU kõikides arvutites. Keskmise CPU iga arvuti vaadates ei pruugi olla kasulik kuna tulemuste saamiseks silutud. Vaadata rohkem üksikasju, te saate koondatud tulemi väiksem ajakulu, akna tükkideks ja vaadake aja sarja üle erinevate mõõtmed. Näiteks saate teha kord tunnis keskmise CPU hõivatus kõikides arvutites järgmiselt:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Keskmine intervall mõõt](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Vaikimisi kuvatakse tulemused mitme sarja interaktiivsed joondiagramm.  See diagramm toetab sarja lülitamise (koos y-telje ümber skaleerimine), suumimise ja libistamisel.  Tabeli kuvamine suvand on endiselt saadaval vaatamise töötlemata andmete vajaduse korral.

Saate ka muude väljade rühmitada. Selles näites otsin % hinnale ühe kindla arvutis ja soovin teada, mis on iga Counter tunni 70 protsentiil.

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Pange tähele, et te102827792ei, et need päringud ei ole piiratud jõudluse hinnale. Saate rakendada neid mis tahes meetermõõdustik. Selles näites olen vaadates W3C IIS-i logid. Soovin teada, mis on maksimaalne aega kulub üle 5-minutilise salvestusintervalli iga päringut:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Mitme agregaadid ühe päringu kasutamine
Saate määrata mitu liitväärtuse klauslitest mõõt käsk.  Igaüks saab neile tuleks määrata pseudonüümid sõltumatult.  Kui see on antud pseudonüümi saab tulemiks oleva välja nimi kokkuvõttefunktsioon, mis on kasutada (nt "avg(CounterValue)" avg(CounterValue)) jaoks.

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Siin on teine näide:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Järgmised sammud

Log otsingute kohta lisateabe saamiseks vaadake:

- [Kohandatud väljad Log Analytics](log-analytics-custom-fields.md) abil saate laiendada log otsinguid.
- Vaadake funktsiooni [Log Analytics logige otsingu viide](log-analytics-search-reference.md) kuvada kõik otsinguväljad ja omadustega Log Analytics saadaval.
