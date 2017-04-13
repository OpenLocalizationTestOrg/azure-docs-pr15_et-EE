## <a name="specifying-formats"></a>Määrab vormingud

Azure'i andmed Factory toetab järgmist tüüpi vorming. 

- [Teksti vormindamine](#specifying-textformat)
- [JSON-vormingus](#specifying-jsonformat)
- [Avro vorming](#specifying-avroformat)
- [ORC vorming](#specifying-orcformat)
- [Parkett vorming](#specifying-parquetformat)

### <a name="specifying-textformat"></a>Määrab tekstivorming

Kui vorming on seatud **tekstivorming**, saate määrata järgmised **Valikuline** atribuudid jaotises **Vorming** .

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Faili veergude eraldamiseks kasutatav märk. | Ainult üks märk on lubatud. Vaikeväärtus on komaga (","). <br/><br/>Kui soovite kasutada mõnda Unicode'i märgi, viidata [Unicode'i märkide](https://en.wikipedia.org/wiki/List_of_Unicode_characters) vastava koodi saamiseks. Näiteks võite kaaluda harvaesinevate printimatu char, mis pole tõenäoliselt andmete kasutamiseks: Määrake "\u0001", mis tähistab käivitamine on pealkiri (SOH). | Ei |
| rowDelimiter | Faili ridade eraldamiseks kasutatav märk. | Ainult üks märk on lubatud. Vaikeväärtus on mõni järgmistest väärtustest kohta vt: ["\r\n", "\r", "\n"] ja "\r\n" kirjutada. | Ei |
| escapeChar | Veeru eraldaja Sisestuskeel faili sisust pääseda kasutatud erimärk. <br/><br/>Te ei saa määrata nii escapeChar quoteChar tabeli. | Ainult üks märk on lubatud. Vaikeväärtus. <br/><br/>Näide: kui teil on koma (",") veeru eraldaja, kuid soovite olema koma tekst (näide: "Tere, maailm!"), saate määratleda '$' Paomärgi ja kasutada string "$Tere, maailm!" allikas. | Ei | 
| quoteChar | Märgi hinnapakkumise stringiväärtus. Veeru- ja eraldajad märke hinnapakkumise sees töödeldakse stringi väärtuse osana. Atribuut on mõeldud ning nii sisend andmekomplektide.<br/><br/>Te ei saa määrata nii escapeChar quoteChar tabeli. | Ainult üks märk on lubatud. Vaikeväärtus. <br/><br/>Näiteks, kui teil on koma (",") veeru eraldaja, kuid soovite olema koma tekst (näide: < Tere, maailm! >), saate määratleda "(jutumärke) hinnapakkumise märgi ja kasutage string"Tere, maailm!"allikas. | Ei |
| nullValue | Ühte või mitut märki, mis tähistavad tühiväärtus. | Üks või mitu märki. Vaikeväärtused on "\N" ja "NULL" lugemine ja "\N" kirjutada. | Ei |
| encodingName | Määrake kodeering nimi. | Sobiv kodeering nimi. lugege teemat [Encoding.EncodingName atribuut](https://msdn.microsoft.com/library/system.text.encoding.aspx). Näide: windows 1250 või shift_jis. Vaikeväärtus on UTF-8. | Ei | 
| firstRowAsHeader | Määrab, kas esimese rea päisena silmas pidada. Andmete Factory loeb päisena on Sisestuskeel andmekomplekti esimene rida. Mõne väljundi andmekogumi andmete Factory kirjutab esimese rea päisena. <br/><br/>Vaadake [ **firstRowAsHeader** ja **skipLineCount** kasutamise stsenaariumid](#scenarios-for-using-firstrowasheader-and-skiplinecount) valimi stsenaariumid. | True<br/>FALSE (vaikeväärtus) | Ei |
| skipLineCount | Näitab vahele jätta, kui andmete lugemine Sisestuskeel failidest ridade arv. Kui määratud on nii skipLineCount ja firstRowAsHeader, ridade vahele esmalt ja siis päise teabe Sisestuskeel faili lugeda. <br/><br/>Vaadake [firstRowAsHeader ja skipLineCount kasutamise stsenaariumid](#scenarios-for-using-firstrowasheader-and-skiplinecount) valimi stsenaariumid. | Täisarv | Ei | 
| treatEmptyAsNull | Määrab, kas null või tühi käsitleda tühiväärtus Sisestuskeel failist andmete lugemise ajal. | True (vaikimisi)<br/>FALSE | Ei |  

#### <a name="textformat-example"></a>Tekstivorming näide
Järgmises näites kuvatakse mõned vorming atribuudid tekstivorming.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

QuoteChar asemel kasutada mõnda escapeChar, asendage rea quoteChar koos järgmised escapeChar:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>FirstRowAsHeader ja skipLineCount kasutamise stsenaariumid

- Te kopeerite-faili allika tekstifaili ja soovite lisada päise rida sisaldava skeemi metaandmeid (nt: SQL-i skeemi). Saate määrata **firstRowAsHeader** true väljundi andmekogumis selle stsenaariumi. 
- Kopeerite päise rea-faili valamu sisaldav tekstifail ja soovite selle kirjuta. Määrake **firstRowAsHeader** Sisestuskeel andmekogumis tõene.
- Kopeerite tekstifaili ja mõned read alguses päise andmed või andmed sisaldavad vahelejätmise. Määrake **skipLineCount** tähistamiseks vahele jätta ridade arv. Kui ülejäänud fail sisaldab rea päise, saate määrata ka **firstRowAsHeader**. Kui määratud on nii **skipLineCount** ja **firstRowAsHeader** , ridade vahele esmalt ja siis veerupäise teabe lugeda Sisestuskeel failist

### <a name="specifying-avroformat"></a>AvroFormat määramine
Kui vorming on seatud AvroFormat, peate määrata mis tahes atribuudid jaotises Vorming jaotise typeProperties. Näide:

    "format":
    {
        "type": "AvroFormat",
    }

Kasutamiseks taru tabeli Avro vorming võib viidata [Apache taru õpetuse](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Võtke arvesse järgmist.  

- [Keerukate andmetüüpidega](http://avro.apache.org/docs/current/spec.html#schema_complex) ei toetata (kirjete variandid, massiive, kaardid, ametiühingud ja fikseeritud)

### <a name="specifying-jsonformat"></a>JsonFormat määramine

Kui vorming on seatud **JsonFormat**, saate määrata järgmised **Valikuline** atribuudid jaotises **Vorming** .

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| filePattern | Saate määrata, muster andmed salvestatakse iga JSON-faili. Väärtused on lubatud: **setOfObjects** ja **arrayOfObjects**. **Vaikeväärtus** on: **setOfObjects**. Vt allpool nende mustrite üksikasjad.| Ei |
| encodingName | Määrake kodeering nimi. Sobiv kodeering nimede loendi leiate teemast: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) atribuut. Näide: windows 1250 või shift_jis. **Vaikeväärtus** on: **UTF-8**. | Ei | 
| nestingSeparator | Pesastustasemete eraldamiseks kasutatav märk. Vaikeväärtus on "." (punkt). | Ei | 


#### <a name="setofobjects-file-pattern"></a>setOfObjects mustriga

Iga fail sisaldab ühe objekti või mitme rea-eraldatud/liitsõnumeid objektid. Kui see suvand on valitud soovitud väljundi andmekomplekti, Kopeeri tegevuse annab tulemiks ühe JSON fail on iga objekti (rea eraldatud) rea kohta.

**ühele objektile** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**rea eraldatud JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**ühendatud JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>arrayOfObjects mustriga. 

Iga fail sisaldab massiivi objektid. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>JsonFormat näide

Kui teil on JSON faili sisu on järgmine:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

ja te soovite kopeerida Azure SQL-i tabelisse järgmises vormingus: 

ID  | Name.First | Name.Middle | Name.Last | Sildid
--- | ---------- | ----------- | --------- | ----
1 | John | null | Mägi | ["Andmete Factory", "Azure"]

Sisestuskeel andmekomplekti JsonFormat tüüp on määratletud järgmiselt: (osaline määratluse ainult asjakohastele osadele)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Kui on määratletud struktuuri Kopeeri tegevuse lömastab vaikimisi struktuuri ja iga asja kopeerida. 

#### <a name="supported-json-structure"></a>Toetatud JSON struktuur
Võtke arvesse järgmist. 

- Iga objekti nimi ja väärtuse paarideks koguda on vastendatud ühe rea andmed tabelina. Objektide võib olla ka pesastatud ja saate määrata, kuidas Ühenda andmekomplekt struktuur vaikimisi koos pesastamise eraldaja (.). Vt [JsonFormat näide](#jsonformat-example) eelneva sektsiooni näide.  
- Kui andmete Factory andmekomplekti pole määratletud struktuuri, kopeerige tegevuse tuvastab skeemi: esimest objekti ja Ühenda kogu objekti. 
- Kui JSON sisend on massiiv, teisendab Kopeeri tegevuse kogu massiivi väärtuse string. Kui soovite, et selle vahele jätate [veeru vastendamine ja filtreerimise](#column-mapping-with-translator-rules)abil.
- Kui samal tasemel on dubleeritud nimed, kopeerige tegevuse huvitavat Viimane.
- Atribuut nimed on tõstutundlikud. Kaks atribuutide sama nime, kuid erinevad korpused käsitletakse kahte eraldi atribuudid. 

### <a name="specifying-orcformat"></a>OrcFormat määramine
Kui vorming on seatud OrcFormat, peate määrata mis tahes atribuudid jaotises Vorming jaotise typeProperties. Näide:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Kui kopeerite ORC failide **nimega-on** kohapealse ja pilveteenuse vahel andmete salvestab, peate installima teie arvutis lüüsi JRE 8 (Java Runtime keskkond). 64-bitine lüüsi nõuab JRE 64-bitist ja 32-bitine lüüsi nõuab 32-bitine JRE. Saate otsida mõlemad versioonid [siia](http://go.microsoft.com/fwlink/?LinkId=808605). Valige sobiv.

Võtke arvesse järgmist.

-   Keerukate andmete tüüpi pole toetatud (KIRJEL, kaart, loendi, Liidu)
-   ORC failil on kolm [tihendamise seotud valikud](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): pole ZLIB, KÄRE. Andmete Factory toetab järgmisi tihendatud vormingutes ORC failist andmete lugemine. Kasutab tihendus kodek metaandmete andmeid lugeda. Siiski ORC faili kirjutamisel andmete Factory valib ZLIB, mis on vaikimisi ORC jaoks. Praegu on võimalus alistada seda käitumist. 

### <a name="specifying-parquetformat"></a>ParquetFormat määramine
Kui vorming on seatud ParquetFormat, peate määrata mis tahes atribuudid jaotises typeProperties jaotises Vorming. Näide:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Kui kopeerite parkett failide **nimega-on** kohapealse ja pilveteenuse vahel andmete salvestab, peate installima teie arvutis lüüsi JRE 8 (Java Runtime keskkond). 64-bitine lüüsi nõuab JRE 64-bitist ja 32-bitine lüüsi nõuab 32-bitine JRE. Saate otsida mõlemad versioonid [siia](http://go.microsoft.com/fwlink/?LinkId=808605). Valige sobiv.

Võtke arvesse järgmist.

-   Keerukate andmete tüüpi pole toetatud (kaarti loend)
-   Parkett failil on tihendamise seotud järgmised suvandid: pole, KÄRE, GZIP ja LZO. Andmete Factory toetab järgmisi tihendatud vormingutes ORC failist andmete lugemine. Metaandmete tihendamise kodekiga kasutab andmeid lugeda. Siiski parkett faili kirjutamisel andmete Factory valib KÄRE, mis on vaikimisi parkett vormingud. Praegu on võimalus alistada seda käitumist. 
