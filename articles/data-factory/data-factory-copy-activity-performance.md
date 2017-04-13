<properties
    pageTitle="Kopeerige tegevuse jõudlus ja häälestamise juhend | Microsoft Azure'i"
    description="Lisateavet olulisi tegureid, mis andmete liikumine Azure'i andmed Factory jõudlust mõjutavad Kopeeri tegevuste kasutamisel."
    services="data-factory"
    documentationCenter=""
    authors="linda33wj"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="jingwang"/>


# <a name="copy-activity-performance-and-tuning-guide"></a>Kopeerige tegevuse jõudlus ja häälestamise juhend
Azure'i andmete Factory Kopeeri tegevus pakub esimese klassi turvaline, usaldusväärseid ja suure jõudlusega andmete laadimine lahendus. See võimaldab teil kümneid koopia andmete TB iga päev rikkaliku hulga cloud ja kohapealsete andmete. Väga kiire andmete laadimine jõudlust on oluline, et tagada, et saate keskenduda core "big data" probleem: täiustatud analüüsi lahenduste ja kõik, et andmete hankimine sügav ülevaateid.

Azure'i pakub kogumi ettevõtte tasemel andmete talletamise hulka ja lahendused, ja kopeerige tegevuse on väga optimeeritud andmete laadimine kogemus, mis on lihtne konfigureerimine ja häälestamine. Ainult ühte eksemplari kasutada tegevust, võite saavutada:

- Andmete laadimine **1,2 Gbit** **SQL Azure'i andmebaas**
- Andmete laadimine **1.0 Gbit** **Azure'i bloobimälu**
- **Azure'i andmesalve Lake** veebisaidil **1.0 Gbit** andmete laadimine


Selles artiklis kirjeldatakse:

- Toetatud andmeallikate ja valamu andmete [jõudluse viide arve](#performance-reference) talletab aitavad kavandada projekti;
- Funktsioonid, mis toetab Kopeeri läbilaskevõime erinevates stsenaariumides, sh [paralleelselt Kopeeri](#parallel-copy), [cloud andmete liikumine üksused](#cloud-data-movement-units)ja [etapiviisilise Kopeeri](#staged-copy);
- Kuidas häälestada jõudlus ja olulisi tegureid, mis võivad mõjutada jõudlust Kopeeri [jõudluse häälestamise juhiseid](#performance-tuning-steps) .

> [AZURE.NOTE] Kui te pole üldiselt Kopeeri tegevuse juba tuttav, vaadake enne selle artikli lugemist [andmete kopeerimine tegevuse abil](data-factory-data-movement-activities.md) .

## <a name="performance-reference"></a>Jõudluse viide

![Jõudluse maatriks](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

> [AZURE.NOTE] Võite saavutada suurema läbilaskevõimega tehtavate rohkem andmeid liikumine ühikut (diiselrongide) kui vaikimisi lubatud diiselrongid, mis on pilv pilve Kopeeri tegevuste, käivitage 8. Näiteks 100 diiselrongide, kus saate kopeerida andmete Azure'i bloobimälu Azure'i andmed Lake Store moodustab 1 gigabait sekundis. Lisateavet selle funktsiooni jaotisest [Cloud andmete liikumine üksused](#cloud-data-movement-units) . Pöörduge [Azure'i toetavad](https://azure.microsoft.com/support/) rohkem diiselrongide taotleda.

Pange tähele, et punkte.

- Läbilaskevõime arvutatakse, kasutades järgmist valemit: [andmete maht Loe allikast] ja [Kopeeri tegevuste käivitamine kestus].
- Jõudluse viide arvud tabelis olid mõõta abil ühte eksemplari kasutada tegevust, käivitage [TPC-H](http://www.tpc.org/tpch/) andmehulgas.
- Pilveteenuse andmete poed vahel kopeerida, seada võrdlus **cloudDataMovementUnits** 1 ja 4 (või 8). **parallelCopies** pole määratud. Lisateavet nende funktsioonide kohta jaotisest [paralleelselt Kopeeri](#parallel-copy) .
- Azure'i andmed salvestab, on lähte-kui ka valamu Azure piirkonna.
- Hübriid (kohapealse pilveteenuses või pilveteenuse kohapealse) jaoks andmete liikumine, lüüsi ühekordsest töötas olnud poest kohapealse andmete eraldi arvutisse. Järgmises tabelis on loetletud konfiguratsiooni. Kui üks tegevus oli töötab lüüsi, kopeerimine tarbitud ainult osa test arvuti CPU, mälu või võrgu läbilaskevõime.
    <table>
    <tr>
        <td>CPU</td>
        <td>32 südamikud 2.20 GHz Inteli Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>Mälu</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Võrgu</td>
        <td>Interneti kasutajaliidese: 10 Gbit; kasutajaliidese sisevõrk: 40 Gbit</td>
    </tr>
    </table>

## <a name="parallel-copy"></a>Paralleelne kopeerimine
Saate andmeid lugeda lähtekoha või kirjutage andmed soovitud sihtkohta **paralleelselt sees Kopeeri tegevust, käivitage**. See funktsioon täiustab koopia toiming jõudlus ja vähendab andmete teisaldamiseks kuluv aeg.

See säte erineb atribuudi **kokkulangevus** tegevuste määratlus. Atribuudi **kokkulangevus** määratleb arvu **käivitab samaaegset Kopeeri tegevuse** töödelda muu tegevuse Windows (1 EL-2 AM, 2 AM 3 AM, 4 EL 3 EL ja jne). See funktsioon on kasulik, kui teete ajalooliste laadi. Paralleelne Kopeeri võimalus kehtib **ühe tegevuste käivitamine**.

Vaatame valimi stsenaariumi. Järgmises näites mitme sektorid minevikus vaja töödelda. Andmete Factory töötab iga sektorit Kopeeri tegevuse (tegevuste käivitamine) eksemplari:

- Andmete sektorit esimese tegevuse aknast (1 AM 2 EL) == > tegevuste käivitamine 1
- Andmete sektorit teise tegevuse aknast (2 EL 3 EL) == > tegevuste käivitamine 2
- Andmete sektorit teise tegevuse aknast (3: 00 4 EL) == > tegevuste käivitamine 3

Jne.

Selles näites kui **kokkulangevus** väärtuseks on seatud 2, **tegevuste käivitamine 1** ja **tegevuste käivitamine 2** andmeid kopeerida kaks tegevuse windows **samaaegselt** andmete liikumine jõudluse parandamiseks. Juhul, kui tegevuste käivitamine 1 on seotud mitu faili, teenuse andmete kopeerib failide lähtekoha sihtfail ühe korraga.

### <a name="parallelcopies"></a>parallelCopies
Saate soovitud Kopeeri tegevuse kasutada paralleelsus näitamaks **parallelCopies** atribuut. Mõelge selle atribuudi nimega Teemad sees Kopeeri tegevust, mis võib teie andmeallikast lugeda või kirjutada oma valamu andmeid talletab paralleelselt maksimaalne arv.

Iga koopia tegevuste käivitamine, määratleb andmete Factory kasutamine andmete kopeerimiseks allika andmete talletamiseks ja sihtkoha andmete talletamiseks paralleelselt eksemplaride arv. Paralleelne eksemplarid, mis kasutab vaikearvu sõltub tüüpi lähte- ja valamu, mida te kasutate.  

Lähte- ja valamu |   Vaikimisi paralleelsetes Kopeeri arv määratud teenus
------------- | -------------------------------------------------
Kopeerige andmed vahel failipõhiste poed (bloobimälu; Lake andmesalve; Amazon S3; kohapealse failisüsteemi; mõne kohapealse HDFS) | Vahemikus 1 kuni 32. Sõltub suurus ja faile pilveteenuses andmete liikumine ühikute (diiselrongide) kasutatakse kopeerida andmeid kahe pilve andmete poed või füüsilise konfiguratsioon lüüsi masina kasutatakse koopia hübriid (Kopeerige andmed või kohapealse andmete poest).
Kopeerige andmed **salvestada mis tahes lähteandmete Azure'i tabelimälu** | 4
Kõik muud lähte- ja valamu paari | 1

Tavaliselt on vaikekäitumine peaks teile parimad läbilaskevõime. Siiski koormus masinad, et majutada oma andmeid kontrollida talletab või Kopeeri jõudluse häälestamine võite vaikeväärtus ja määramiseks väärtuse **parallelCopies** atribuudi. Väärtus peab olema vahemikus 1 kuni 32 (nii kaasa arvatud). Käivitamise ajal kasutab Kopeeri tegevuse parima jõudluse tagamiseks väärtus, mis on väiksem või võrdne teie määratud väärtuse.

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "parallelCopies": 8
            }
        }
    ]

Pange tähele, et punkte.

- Kui kopeerite andmeid failipõhiste poed vahel, paralleelsus juhtub faili tasemel. On ühe failis pole murenemist. Tegelik eksemplaride arv, paralleelselt teenuse andmete kasutab käitusajal toimingu Kopeeri pole rohkem kui teil on failide arvu. Kui Kopeeri käitumine on **mergeFile**, ei saa Kopeeri tegevuse ära paralleelsuse.
- Kui määrate atribuudi **parallelCopies** väärtus, kaaluge laadi kasvavad lähte- ja valamu andmete poed ja lüüsi, kui see on hübriid koopia. See juhtub, eriti kui teil on mitu tegevuste või samaaegseid jookseb sama tegevusi, mis on vastuolus sama andmesalve. Kui märkate, et andmesalve või lüüsi ülekoormatud koormusega, vähendage **parallelCopies** väärtust laadi leevendada.
- Andmete kopeerimisel salvestab, mis pole poed, mis on failipõhiste failipõhiste teenuse andmete ignoreerib **parallelCopies** atribuuti. Ka siis, kui paralleelsus pole määratud, see on rakendatud sel juhul.

> [AZURE.NOTE] Kasutage **parallelCopies** funktsiooni hübriid koopia, mida teha, kui peate kasutama Andmehalduslüüsi 1.11 või uuem versioon.

### <a name="cloud-data-movement-units"></a>Pilveteenuse andmete liikumine ühikud
**Pilveteenuse andmekogumiks liikumine (DMU)** on näitaja, mis tähistab power (kombinatsiooni CPU, mälu ja võrgu ressursi eraldus) andmete Factory ühe üksuse. Mõne DMU kasutamiseks pilv pilve koopia toiming, kuid mitte hübriid koopia.

Vaikimisi kasutab andmete Factory ühe cloud DMU sooritada ühe Kopeeri tegevuste käivitamine. See vaikimisi alistamiseks määrake atribuudi **cloudDataMovementUnits** väärtus järgmiselt. Jõudluse kohta rohkem mõõtühikud teatud Kopeeri lähte- ja valamu konfigureerimisel võite saada lisateavet [jõudluse viide](#performance-reference).

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "cloudDataMovementUnits": 4
            }
        }
    ]

On **lubatud väärtused** **cloudDataMovementUnits** atribuut on 1 (vaikeväärtus), 2, 4 ja 8. **Tegeliku arvu cloud diiselrongide** käitusajal kasutava kopeerimine on võrdne või väiksem kui konfigureeritud väärtus, sõltuvalt teie andmete muster. 

> [AZURE.NOTE] Kui teil on vaja rohkem pilve diiselrongide suurema läbilaskevõimega, võtke ühendust [Azure tugi](https://azure.microsoft.com/support/). Seadmine 8 ja selle kohal toimib praegu ainult siis, kui kopeerite mitu faili bloobimälu bloobimälu, Lake andmesalve või Azure'i SQL-andmebaasi ja faili maht on suurem kui või võrdne 16 MB ükshaaval.

Paremini kasutada kaks atribuutidest ja täiustamiseks oma andmete liikumine läbilaskevõime, lugege teemat [valimi kasutamine juhtudel](#case-study-use-parallel-copy). Te ei pea **parallelCopies** ära vaikekäitumine konfigureerimine. Kui konfigureerite ja **parallelCopies** on liiga väike, mitme cloud diiselrongide võib pole täielikult kasutada.  

See on **oluline** meeles pidada, et teil on maksta põhjal kopeerimine koguaeg. Kui koopia töökoht kasutada tund ühe üksuse ja kohe kulub 15 minutit nelja cloud üksustega, jääb üldine arve peaaegu sama. Näiteks saate kasutada nelja cloud üksused. Pilveteenuse esimese üksuse veedab 10 minuti järel teine, 10 minuti, kolmas üks, 5 minutit, ja neljas üks, 5 minutit, kõik ühe Kopeeri tegevuse käivitada. Teilt aega kokku Kopeeri (andmete liikumine), mis on 10 + 10 + 5 + 5 = 30 minutit. Kasutades **parallelCopies** ei mõjuta arveldamine.

## <a name="staged-copy"></a>Etapiviisilise kopeerimine
Kui kopeerite andmeid allika andmete poest valamu andmed salvestada, võite kasutada bloobimälu ajutised lavastus poest. Lavastus on eriti kasulik järgmistel juhtudel:

1.  **Soovite andmete erinevad andmed salvestab SQL-i andmebaas PolyBase kaudu sisse neelata**. SQL-i andmebaas kasutab PolyBase suure läbilaskevõimega süsteem SQL-i andmebaas laadimiseks suurt hulka andmeid. Siiski lähteandmete peab olema bloobimälu ja täiendavate kriteeriumide täitma. Kui andmete laadimine peale bloobimälu andmete poest, saate aktiveerida andmete kopeerimine ajutised lavastus bloobimälu kaudu. Sel juhul andmete Factory teostab vajalikud andmed teisendused tagamaks, et see vastab PolyBase. Seejärel kasutab PolyBase andmete laadimiseks SQL-i andmebaas. Lisateavet leiate teemast [Kasutamine PolyBase andmeid SQL Azure'i andmebaas laadimiseks](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
2.  **Mõnikord see võtab aega, et teha hübriid andmete liikumine (st kopeerimiseks mõnda kohapealse andmete vahel talletamine ja pilveteenuse andmed salvestada) aeglase võrguühenduse kaudu**. Jõudluse parandamiseks saate tihendada andmete asutusesisene nii, et andmete teisaldamiseks pilveteenuses lavastus andmesalve vähem aega. Seejärel saate andmed lavastus poes lahti enne laadimist andmesalve sihtkoht.
3.  **Te ei soovi avatud pordid peale pordi 80 ja pordi 443 tulemüüri tõttu ettevõttepoliitikaid IT**. Näiteks kui kopeerite andmeid kohapealse andmete poest soovitud valamu Azure'i SQL-andmebaasi või mõne SQL Azure'i andmebaas valamu, peate aktiveerima väljaminev TCP teatis nii Windowsi tulemüür ja teie ettevõtte tulemüüri portide 1433. Selle stsenaariumi korral ära esimese andmete kopeerimine lüüs bloobimälu salvestusruumi lavastus eksemplari üle HTTP- või HTTPS pordi 443. Seejärel laadida andmeid SQL-andmebaasi või SQL-i andmebaas bloobimälu salvestusruumi lavastus kaudu. See vool ei vaja port 1433 lubamiseks.

### <a name="how-staged-copy-works"></a>Kuidas etapiviisilise Kopeeri töötab
Lavastus aktiveerige esmalt andmed kopeeritakse poest allika andmete lavastus andmesalve (tuua kui oma). Järgmiseks andmed kopeeritakse lavastus andmesalve valamu andmesalve. Andmete Factory haldab automaatselt kahe etapi meilivoo jaoks. Andmete Factory ka puhastada ajutiste andmete lavastus salvestusruumi pärast andmete liikumine on lõpule viidud.

Lüüsi ei kasutata cloud Kopeeri stsenaariumi (nii lähte-kui ka valamu andmed poed on pilves). Andmete Factory teenuse tehete Kopeeri.

![Etapiviisilise koopia: pilveteenuste stsenaarium](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Hübriidjuurutuse Kopeeri stsenaariumi (allikas on kohapealse ja valamu on pilves), lüüsi teisaldab andmed allikas andmesalve lavastus andmesalve. Andmete Factory teenuse teisaldab andmed lavastus andmesalve valamu andmesalve. Pilveteenuse andmete poe andmete kopeerimine kohapealse andmesalve, kaudu lavastus ka on toetatud pööratud voogu.

![Etapiviisilise koopia: hübriidjuurutuse stsenaarium](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Andmete liikumine aktiveerimisel lavastus poe abil saate määrata, kas soovite andmed enne andmesalve allika andmete teisaldamine ajutised või lavastus andmesalve tihendada, ja seejärel lahti enne andmete teisaldamise vahepeal või lavastus andmesalve valamu andmete pood.

Praegu ei saa kopeerida andmete vahel kahe kohapealse andmete poed lavastus poe kaudu. Loodame, et see suvand, et varsti saadaval.

### <a name="configuration"></a>Konfigureerimine
Seadistada **enableStaging** seadmine Kopeeri tegevuse abil määrata, kas soovite andmed etapiviisilise bloobimälu sisse, enne selle laadimine sihtkohta andmed säilitada. Järgmises tabelis on loetletud täiendavate atribuutide määramiseks **enableStaging** määramisel väärtuseks TRUE. Kui teil pole ühte, samuti peate looma ka Azure Storage või salvestusruumi ühiskasutusse antud juurdepääs allkirja lingitud teenuse lavastus.

Atribuut | Kirjeldus | Vaikeväärtus | Nõutav
--------- | ----------- | ------------ | --------
**enableStaging** | Määrake, kas soovite kopeerida andmeid vahepeal lavastus poe kaudu. | FALSE | Ei
**linkedServiceName** | Määrake [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) või [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) lingitud teenus, mis viitab salvestusruumi eksemplari, mida te kasutada mõne faili ajutiselt lavastus poe nimi. <br/><br/> Andmete laadimiseks SQL-i andmebaas PolyBase kaudu ei saa kasutada ühiskasutatava Accessi allkirjastatud salvestusruumi. Kõigi teiste stsenaariumide saate seda kasutada. | N/A | Jah, kui **enableStaging** on seatud väärtusele tõene
**tee** | Määrake bloobimälu salvestusruumi tee, mida soovite etapiviisilise andmed sisaldavad. Kui esitate tee teenus loob container, ajutised andmete talletamiseks. <br/><br/> Määrake tee ainult juhul, kui kasutate salvestusruumi ühiskasutatava Accessi allkirjastatud või vajate ajutised andmed olema kindlas asukohas. | N/A | Ei
**enableCompression** | Saate määrata, kas andmed on tihendatud enne sihtkohta kopeerida. Selle sätte vähendab üle andmete maht. | FALSE | Ei

Siin on valimi määratlus Kopeeri tegevuse atribuudid, mida on kirjeldatud eelmises tabelis.

    "activities":[  
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [{ "name": "OnpremisesSQLServerInput" }],
        "outputs": [{ "name": "AzureSQLDBOutput" }],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "MyStagingBlob",
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
    ]


### <a name="billing-impact"></a>Arveldamine mõju
Teilt põhjal kaks toimingut: kopeerimine kestus ja kopeerige tüüp. 

- Kui kasutate lavastus ajal (kopeerimine andmete pilve andmete poest teise pilve andmesalve) cloud koopia teilt tasu [summa Kopeeri kestus 1 ja 2] [cloud Kopeeri ühiku hind] x.
- Kasutamisel lavastus ajal (andmete kopeerimise kohapealse andmete poest cloud andmed salvestada) hübriid koopia ostmisega [hübriid Kopeeri kestus] x [hübriid Kopeeri ühiku hind] + [cloud Kopeeri kestus] x [cloud Kopeeri ühiku hind].


## <a name="performance-tuning-steps"></a>Jõudluse häälestamise juhised
Me soovitame, et võtta neid juhiseid oma andmete Factory teenuse Kopeeri tegevuse jõudluse häälestamine:

1.  **Looma võrdlusalus**. Ajal, Testige oma müügivõimaluste Kopeeri tegevuse abil andmeid valimi suhtes. Andmete Factory [viilutamine mudeli](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) abil saate piirata andmehulga, saate töötada.

    Täitmisaeg ja parameetritele, kasutades **jälgimine ja haldamine App**koguda. Valige avalehel andmete Factory **jälgimine ja haldamine** . Valige puuvaates **väljundi andmekomplekti**. Valige loendis **Tegevuse Windows** Kopeeri tegevuse käivitada. **Tegevuste Windows** on loetletud Kopeeri tegevuse kestuse ja kopeeritud andmete suurust. Funktsiooni läbilaskevõime on loetletud **tegevuste aknas**Exploreris. Rakenduse kohta leiate lisateavet teemast [jälgimine ja haldamine Azure'i andmed Factory torujuhtmete jälgimine ja haldamine rakenduse abil](data-factory-monitor-manage-app.md).

    ![Tegevuste käivitamine üksikasjad](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

    Selle artikli saate võrrelda jõudlus ja konfigureerimine stsenaariumist Kopeeri tegevuse [jõudluse viide](#performance-reference) meie katsete.
2. **Diagnoosimine ja optimeerida jõudlust**. Kui märkate jõudlus ei vasta teie vajadustele, peate jõudluse põhimõtteid. Seejärel Optimeeri jõudlust eemaldada või vähendada kitsaskohad. Jõudluse diagnostika täielikku kirjeldust on selles artiklis ei käsitleta, kuid siin on mõned levinud kaalutlused.
    - Jõudluse funktsioonid.
        - [Paralleelne kopeerimine](#parallel-copy)
        - [Pilveteenuse andmete liikumine ühikud](#cloud-data-movement-units)
        - [Etapiviisilise kopeerimine](#staged-copy)   
    - [Allikas](#considerations-for-the-source)
    - [Valamu](#considerations-for-the-sink)
    - [Sariväljaanne ja vahemäluasukohaga](#considerations-for-serialization-and-deserialization)
    - [Tihendamine](#considerations-for-compression)
    - [Veeru kaardistamine](#considerations-for-column-mapping)
    - [Andmehalduslüüs](#considerations-for-data-management-gateway)
    - [Muud kaalutlused](#other-considerations)

3. **Laienda konfiguratsioon, teie kogu andmekogumit**. Kui olete rahul täitmise tulemuste ja, saate laiendada määratlemise ja müügivõimaluste aktiivse perioodi kataks kogu teie andmekogumis.

## <a name="considerations-for-the-source"></a>Allika kohta
### <a name="general"></a>Üldine
Pidage meeles, et aluseks andmesalve on ei otsingutulemusi on liiga palju muud töökoormus, mis töötavad sisse- või selle vastu. 

Vt Microsofti andmete poed, [jälgimise ja häälestamise teemasid](#performance-reference) mis on seotud andmete, ja aidata teil mõista andmete talletamine parameetritele, vastuse korda minimeerimine ja maksimeerimine läbilaskevõime.

Kui kopeerite andmeid bloobimälu SQL-i andmebaas, kaaluge **PolyBase** parandada jõudlust. Üksikasjad leiate [Kasutamine PolyBase laadimiseks andmeid SQL Azure'i andmebaas](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) .


### <a name="file-based-data-stores"></a>Andmete failipõhiste
*(Sh bloobimälu salvestusruumi, Lake andmesalve, Amazon S3, kohapealse failisüsteemi ja kohapealse HDFS)*

- **Keskmise failimahu ja faili count**: Kopeeri tegevuse üle ühe andmefaili korraga. Andmete teisaldamiseks sama palju üldine jõudlus on väiksem kui andmete koosneb väike palju faile, mitte mõne suuri faile tõttu bootstrap etapp iga faili. Seetõttu võimalusel väike faili ühendada saada suurema läbilaskevõimega suuremaid faile.
- **Failivorming ja detailsuse**: jõudluse parandamiseks rohkem võimalusi, leiate teemast [kaalutluste kohta sariväljaanne ja vahemäluasukohaga](#considerations-for-serialization-and-deserialization) ja [kaalutluste kohta tihendamise](#considerations-for-compression) jaotised.
- **Kohapealse failisüsteemi** stsenaariumi puhul, kus on nõutav, **Andmehalduslüüsi** jaotisest [kaalutluste kohta Andmehalduslüüsi](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Relatsiooniliste andmete poed
*(Sh SQL-andmebaasi; SQL-i andmebaas; Amazon punanihe; SQL serveri andmebaasi; ja Oracle, MySQL-i, DB2, Teradata, Sybase ja PostgreSQL-i andmebaasi jne)*

- **Andmete mustri**: teie tabeli skeemi mõjutab Kopeeri läbilaskevõime. Suure rea suuruse annab teile töökindluse kui väike rea suuruse, kopeerida sama suurt hulka andmeid. Põhjus on selles, et andmebaasi tõhusamalt saate tuua vähem pakettidena andmeid, mis sisaldavad vähem read.
- **Päringu või salvestatud protseduur**: optimeerimine loogika päringu või määrate Kopeeri tegevuse allika andmete toomiseks tõhusamalt salvestatud protseduur.
- **Kohapealse relatsiooniandmebaasid**, nt SQL serveri ja Oracle, mis nõuavad **Andmehalduslüüsi**, vaadake jaotist [kaalutluste kohta Andmehalduslüüsi](#considerations-on-data-management-gateway) .

## <a name="considerations-for-the-sink"></a>Kaalutlused valamu

### <a name="general"></a>Üldine
Pidage meeles, et aluseks andmesalve on ei otsingutulemusi on liiga palju muud töökoormus, mis töötavad sisse- või selle vastu. 

Microsoft andmed salvestab, vaadake [jälgimise ja häälestamise teemasid](#performance-reference) mis on seotud andmete. Järgmisi teemasid aitavad teil mõista andmete poe parameetritele ja kuidas vähendada vastuse korda ja läbilaskevõime maksimeerimine.

Kui kopeerite andmeid **bloobimälu** : **SQL-i andmebaas**, kaaluge **PolyBase** parandada jõudlust. Üksikasjad leiate [Kasutamine PolyBase andmeid SQL Azure'i andmebaas laadimiseks](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) .


### <a name="file-based-data-stores"></a>Andmete failipõhiste
*(Sh bloobimälu salvestusruumi, Lake andmesalve, Amazon S3, kohapealse failisüsteemi ja kohapealse HDFS)*

- **Kopeeri käitumine**: andmete kopeerimisel poest erinevad faili-i põhiste andmete kopeerimine tegevuse on kolm võimalust atribuudi **copyBehavior** kaudu. See säilitab hierarhia, lömastab hierarhia või ühendab failid. Kas säilitada või lamedamad hierarhia sisaldab või ei pea jõudluse kohal, kuid failide ühendamise sunnib jõudluse elemente, et suurendada.
- **Failivorming ja detailsuse**: teemades [kaalutluste kohta sariväljaanne ja vahemäluasukohaga](#considerations-for-serialization-and-deserialization) ja [kaalutluste kohta tihendamise](#considerations-for-compression) rohkem võimalusi parandada jõudlust.
- **Bloobimälu**: praegu bloobimälu salvestusruumi toetab ainult blokeerida plekid jaoks optimeeritud andmeedastus ja läbilaskevõime.
- **Kohapealse failisüsteemi** stsenaariumi, mis nõuavad **Andmehalduslüüsi**, jaotisest [kaalutluste kohta Andmehalduslüüsi](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Relatsiooniliste andmete poed
*(Sh SQL-andmebaasiga, SQL-i andmebaas, SQL serveri andmebaasi ja Oracle'i andmebaasid)*

- **Kopeerige käitumine**: sõltuvalt teie määratud jaoks **sqlSink**atribuudid Kopeeri tegevuse kirjutab andmed sihtandmebaas erineval viisil.
    - Vaikimisi andmete liikumine teenuse kasutab hulgi Kopeeri API andmete lisamiseks lisa režiimi, mis annab parimaid tulemusi.
    - Kui salvestatud toiming valamu konfigureerimiseks kehtib andmebaasi andmete ühe rea haaval asemel hulgi laadi nimega. Jõudluse langeb märkimisväärselt. Kui teie andmekogumis on suur, kui korral võiksite aktiveerida **sqlWriterCleanupScript** atribuudi abil.
    - Kui atribuudi **sqlWriterCleanupScript** konfigureerimine iga koopia tegevuste käivitamine teenuse käivitab skripti ja seejärel kasutage hulgi Kopeeri API andmete lisamiseks. Näiteks ülekirjutamiseks kogu tabel uusimaid andmeid, saate määrata skripti enne hulgi-laadimise uute andmete lähtekoha kustutama kõik kirjed.
- **Andmete mustri ja paketi suurus**:
    - Oma tabeli skeemi mõjutab Kopeeri läbilaskevõime. Sama suurt hulka andmeid kopeerimiseks suurte rea suuruse annab teile töökindluse kui väike rea suuruse Kuna andmebaasi saab tõhusamalt Kinnita vähem pakettidena andmete.
    - Kopeeri tegevuse sisestab andmeid pakettidena sarja. Atribuudi **writeBatchSize** abil saate määrata partii ridade arv. Kui teil on small read, saate alumise paketi pea kohal ja suurema läbilaskevõimega kõrgema väärtusega atribuut **writeBatchSize** . Kui rea andmete maht on suur, olge ettevaatlik, kui teil suurendada **writeBatchSize**. Kõrge väärtus võib põhjustada Kopeeri tõrke põhjuseks ülekoormuse andmebaasi.
- **Kohapealse relatsiooniandmebaasid** nagu SQL serveri ja Oracle, mis nõuavad **Andmehalduslüüsi**, jaotisest [kaalutluste kohta Andmehalduslüüsi](#considerations-for-data-management-gateway) .


### <a name="nosql-stores"></a>NoSQL poed
*(Sh tabelimälu ja Azure DocumentDB).*

- **Tabelimälu**:
    - **Sektsiooni**: andmete oluliselt üksteise sektsioonid kirjutamise laguneb jõudlust. Allika andmete sortimine partition vajutamisega nii, et andmed on sisestatud tõhus ühe sektsiooni teise järel või reguleerida loogika ühe sektsiooni andmete kirjutamiseks.
- Jaoks **DocumentDB**:
    - **Paketi suurus**: atribuudi **writeBatchSize** määrab paralleelselt taotluste arv DocumentDB teenuse dokumentide loomiseks. Parema jõudluse võib juhtuda, kui te Suurenda **writeBatchSize** , kuna rohkem paralleelselt taotlused saadetakse DocumentDB. Olge siiski pidurdamise DocumentDB kirjutamisel (tõrketeade on "Päring on suur"). Tegurid, võivad põhjustada pidurdamise dokumendimaht, sh dokumendid ja target saidikogumi indekseerimise poliitika arvu. Suurema Kopeeri läbilaskevõimega saavutamiseks kaaluge parem kogumi, näiteks S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Kaalutlused sariväljaanne ja vahemäluasukohaga
Sariväljaanne ja vahemäluasukohaga võib tekkida siis, kui teie Sisestuskeel andmehulga või väljundi andmehulga on faili. Kopeeri tegevuse toetab praegu, Avro ja teksti (nt CSV ja TSV) andmevormingute.

**Kopeerige käitumine**.

-   Failide vahel failipõhiste andmete kopeerimine:
    - Kui sisend- ja andmekogumi mõlemad on sama või pole faili vorming sätteid, teenuse andmete aktiveeritakse ilma sariväljaanne või vahemäluasukohaga kahendarvu koopia. Näete suurema läbilaskevõimega, võrrelduna stsenaariumi puhul, kus on erinevad lähte- ja valamu faili vorming sätteid.
    - Kui sisestada ja väljundi andmekogumi nii on ainult kodeering ja teksti vormingu tüüp on erinev, teenuse andmed ainult ei kodeering teisendamine. See ei mis tahes sariväljaanne ja vahemäluasukohaga, mis põhjustab mõne jõudluse kohal võrreldes kahendarvu koopia.
    - Kui sisend- ja andmekogumi mõlemad on failivormingutes või muu konfiguratsioone, nt eraldajaid, teenuse andmete deserializes lähteandmete voona, muuta ja seejärel serialiseerida selle üheks määrasite vorming. Selle toimingu tulem kohal võrreldes teiste stsenaariumide tähtsamad jõudlust.
- Kui kopeerite failide andmeid hoida, mis ei ole failipõhiste (nt poest failipõhiste relatsiooniliste poest), serialiseerimine või vahemäluasukohaga samm on vajalik. Selle toimingu tulem märkimisväärset jõudluse pea kohal.

**Failivorming**: valige failivorming võib mõjutada kopeerimine jõudlust. Näiteks Avro on tihendatud kahendvormingus, mis talletab metaandmed andmetega. See toetab lai Hadoopi ökosüsteemis töötlemiseks ja päringud. Avro aga kallim sariväljaanne ja vahemäluasukohaga, mille tulemuseks on alumise Kopeeri läbilaskevõime võrreldes tekstivormingus. Tehke valik failivormingu kogu töötlemine voogu terviklikult. Alustada vormi andmed on salvestatud, allika andmete poed või ekstraktimist välised süsteemid; parim vorming säilitamiseks, analytical töötlemiseks ja päringute; ja millises vormingus peaksid andmed eksportida andmete marts aruandlus-ja visualiseeringu jaoks sisse. Mõnikord failivormingus, mis on mõeldud lugemine ja kirjutamine, jõudlus võib hea valik, kui teie arvates analüüsi protsess.

## <a name="considerations-for-compression"></a>Kaalutlused tihendamine
Kui teie andmekogumis sisestuskeel või väljund on faili, saate seada Kopeeri tegevuse tihendamise või rõhu sooritamiseks, nagu see kirjutab andmed soovitud sihtkohta. Kui valite tihendamise, teete on Miinuseks sisend/väljund (I/O) vahel ja CPU. Tihendamise Arvuta ressursid andmete lisatasu. Kuid, see vähendab võrgu I/O ja salvestusruumi. Olenevalt teie andmeid, võidakse kuvada rakenduses üldine Kopeeri jõudlus suurendada.

**Kodek**: Kopeeri tegevuse toetab gzip, bzip2 ja Deflate tihendamise tüübid. Azure Hdinsightiga saab kasutada kõigi kolme tüüpi töötlemiseks. Iga tihendamise kodekiga on järgmised eelised. Näiteks bzip2 on kõige väiksemad Kopeeri läbilaskevõime, kuid saate parima taru päringu jõudluse bzip2, kuna saate tükeldada töötlemiseks. Gzip on kõige kuitasakaalustatudsuvand ja seda kasutatakse enamasti. Valige kodekiga, mis sobib kõige paremini stsenaariumist lõpuni.

**Tase**: saate valida iga tihendamise kodekiga kahel viisil: kiiremini tihendatud ja optimaalselt tihendatud. Kiireim võimalus tihendatud tihendab andmed nii kiiresti kui võimalik, isegi juhul, kui fail on tihendatud optimaalselt. Suvandi optimaalselt tihendatud veedab rohkem aega tihendamise ja annab minimaalsete suurt hulka andmeid. Mõlema variandi, et näha, mis annab parema üldise jõudluse teie puhul saate testida.

**A tasu**: suurt hulka andmeid poest kohapealse ja pilveteenuse vahel kopeerida, kaaluge ajutised bloobimälu tihendamise abil. Ajutiste mäluruumi kasutamine on kasulik, kui Azure'i teenuste ja oma ettevõtte võrku läbilaskevõime on piiramise ning soovite Sisestuskeel andmehulga ja väljundi andmehulga nii tihendamata kujul. Täpsemalt saate ühte eksemplari kasutada tegevuse murda kaks koopia tegevusi. Kopeerige esimese tegevusega kopeerib lähtekoha on ajutised või lavastus bloobimälu tihendatud kujul. Teine koopia tegevuse tihendatud andmed kopeeritakse lavastus ja seejärel decompresses ajal öeldakse valamu.

## <a name="considerations-for-column-mapping"></a>Veeru vastendamise kohta
Saate atribuudi **columnMappings** Kopeeri tegevuse kaardi kõik või Sisestuskeel veerud väljundi veergudele alamhulga. Pärast teenuse andmete loeb andmete lähtekoha, tuleb sooritada veeru vastenduse andmed enne selle kirjutab andmed valamu. Selle eest töötlemine vähendab Kopeeri läbilaskevõime.

Kui teie lähteandmed talletada on päringuvõimalusega, näiteks, kui see on relatsiooniline poe nagu SQL-andmebaasi või SQL serveri või kui NoSQL poe tabelimälu või DocumentDB, nagu on soovitatav lükkamine veeru filtreerimine ja ümberjärjestamisega loogika **päringu** atribuudi asemel veeru vastendamise. Sellisel viisil projektsiooni esineb teenuse andmete loeb andmete allikas andmesalve, kus on palju tõhusamaks.

## <a name="considerations-for-data-management-gateway"></a>Andmehalduslüüsi kaalutlused
Lüüsi häälestamise soovitusi, leiate teemast [teave kasutamise Andmehalduslüüsi](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway).

**Lüüsi seadme keskkonnas**: soovitame kasutada seadet, Host Andmehalduslüüsi. Kasutatavad tööriistad (nt Perfmoni uurida ajal koopia toiming teie arvutis lüüsi CPU, mälu ja läbilaskevõime kasutamine). Kui CPU, mälu või võrgu läbilaskevõime muutub kitsaskoht võimsam arvuti halduskeskuses.

**Käivitab samaaegset Kopeeri tegevuse**: ühekordsest Andmehalduslüüsi võib olla mitu eksemplari tegevuse jookseb samal ajal või samaaegselt. Samaaegseid töökohtade maksimaalne arv arvutatakse lüüsi arvuti riistvara konfiguratsiooni alusel. Täiendavad Kopeeri töö järjekorda seni, kuni on valitud fotodest, lüüsi või mõne muu töö ajalõpp. Ressursi väide lüüsiseadmega vältimiseks saate etapp ajakava kopeerimine tegevuse Kopeeri töökohtade järjekorda arvu vähendamiseks korraga või kaaluge tükeldamise laadimise mitme lüüsi masinad peale.


## <a name="other-considerations"></a>Muud kaalutlused
Kui andmeid, mida soovite kopeerida on suur, saate reguleerida oma äriloogika edasise kasutamine andmete Factory viilutamine süsteemi andmed. Seejärel ajastada Kopeeri tegevuste käivitamiseks rohkem sageli andmete mahu vähendamiseks iga koopia tegevuste käivitamine.

Olge ettevaatlik andmekogumi ja Kopeeri tegevuste nõua andmete Factory konnektori sama andmesalve samal ajal arv. Palju samaaegseid Kopeeri töö võib throttle andmete pood ja jõudluse langemist Kopeeri töö sisemise korduskatsed ja mõnel juhul täitmise tõrkeid.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Näidis stsenaarium: asutusesisese SQL Serverist koopia bloobimälu
**Stsenaarium**: müügivõimaluste on ehitatud asutusesisese SQL Serverist andmete kopeerimine bloobimälu CSV-vormingus. Kopeeri töö kiiremaks tegemiseks peaksite CSV-failid tihendatud bzip2 vormingusse.

**Testi ja analüüsi**: Kopeeri tegevuse läbilaskevõime on väiksem kui 2 MBps, mis on palju aeglasem kui jõudluse võrdlusalus.

**Jõudluse analüüs ja häälestamine**: jõudluse probleemi tõrkeotsinguks vaatame kuidas andmeid töödeldakse ja teisaldada.

1.  **Andmete lugemine**: lüüsi avatakse ühenduse SQL serveri ja saadab päringu. SQL Server vastab, saates andmevoos lüüsi sisevõrgu kaudu.
2.  **Andmete Serialize ja Tihenda**: lüüsi serializes andmevoos CSV-vormingus ja tihendab bzip2 voo andmed.
3.  **Andmete kirjutamiseks**: lüüs laadib bzip2 voo bloobimälu Interneti kaudu.

Nagu näete, andmed on töödeldav ja teisaldatud algses streaming: SQL Server > LAN > lüüsi > WAN > Bloobivahemälu salvestusruumi. **Minimaalne läbilaskevõime üle tulemas on mis üldise jõudluse**.

![Andmevoo](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Üks või mitu järgmistest teguritest võib põhjustada jõudluse kitsaskoht:

-   **Allikas**: SQL Server ise on väikese läbilaskevõimega raske laadimise tõttu.
-   **Andmehalduslüüsi**:
    -   **LAN**: lüüsi on eemal suurematest SQL serveri arvuti ja sellel on kitsaribaline ühendus.
    -   **Lüüsi**: lüüsi jõudnud oma laadi piirangud teha järgmisi toiminguid:
        -   **Sariväljaanne**: jaotamine andmevoos CSV-vormingus on aeglane jõudlus.
        -   **Tihendamine**: teie valitud on aeglane tihendamise kodek (nt bzip2, mis on 2,8 MBps Core i7).
    -   **WAN**: läbilaskevõime ettevõtte võrgus ja Azure teenuste vahel on väike (nt T1 = 1 544 kbps; T2 = 6,312 kbps).
-   **Valamu**: bloobimälu on väikese läbilaskevõimega. (Sel juhul on tõenäoline, kuna selle SLA garantiid vähemalt 60 MBps.)

Sel juhul võib bzip2 andmete pakkimine aeglustavad kogu kohaletoimetamisel. Üleminek gzip tihendamise kodek võib see kitsaskoht leevendada.


## <a name="sample-scenarios-use-parallel-copy"></a>Kuulake stsenaariumid: paralleelselt kopeerimise  

**Stsenaarium I:** Kopeerige 1000 1 MB failide bloobimälu kohapealse failisüsteemis.

**Analüüsi ja jõudluse häälestamine**: näiteks kui olete installinud Standard core arvutis lüüsi andmete Factory kasutab 16 paralleelselt eksemplaride teisaldada faile teenusest failisüsteemi bloobimälu samaaegselt. See paralleelne peaks tulemuseks kõrge läbilaskevõime. Samuti saate otseselt määrata paralleelselt eksemplaride arv. Kui kopeerite small palju faile, abi paralleelselt eksemplaride läbilaskevõime oluliselt ressursid tõhusamalt abil.

![Stsenaarium 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Stsenaarium II**: 20 plekid 500 MB bloobimälu kopeerimiseks Lake poe andmeanalüüsi ja seejärel jõudluse häälestamine.

**Analüüsi ja jõudluse häälestamine**: selle stsenaariumi korral andmete Factory kopeerib andmed bloobimälu Lake andmesalve ühe copy (**parallelCopies** väärtuseks 1) ja ühekordne-cloud andmete liikumine üksuste abil. Läbilaskevõime, märkate lähedane kirjeldatakse [jõudluse viide osas](#performance-reference).   

![Stsenaarium 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Stsenaarium III**: üksikute faili maht on suurem kui kümneid MB ja maht on suur.

**Analüüsi ja lülitada jõudlus**: suurendab **parallelCopies** ei tähenda Kopeeri töökindluse tõttu ühe-cloud DMU ressursi piirangud. Selle asemel määrate rohkem pilve diiselrongide saada rohkem ressursse teha andmete liikumine. Määrake atribuudi **parallelCopies** väärtus. Andmete Factory tegeleb paralleelsus on teie jaoks. Sel juhul kui seate **cloudDataMovementUnits** 4, mille läbilaskevõime on umbes neli korda ilmneb.

![Stsenaarium 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Viide
Siin on jõudluse jälgimise ja häälestamise viited osa toetatud andmete jaoks.

- Azure'i salvestusruumi (sh bloobimälu ja tabelimälu): [Azure'i salvestusruumi skaleeritavus sihtkohtade](../storage/storage-scalability-targets.md) ja [Azure Storage jõudlus ja skaleeritavus kontroll-loend](../storage//storage-performance-checklist.md)
- Azure'i SQL-andmebaasi: Saate [jälgida jõudlus](../sql-database/sql-database-service-tiers.md#monitoring-performance) ja märkige ruut protsenti andmebaasi tehingu üksus (DTU)
- SQL Azure'i andmebaas: Selle võimalus mõõta andmete ladu ühikutes (DWUs); vt [haldamine arvutada power (ülevaade) SQL Azure'i andmebaas](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
- Azure'i DocumentDB: [jõudluse tasemete DocumentDB](../documentdb/documentdb-performance-levels.md)
- Asutusesisese SQL Server: [jälgimine ja jõudluse tune](https://msdn.microsoft.com/library/ms189081.aspx)
- Kohapealse faili server: [jõudluse parandamine fail serverite jaoks](https://msdn.microsoft.com/library/dn567661.aspx)
