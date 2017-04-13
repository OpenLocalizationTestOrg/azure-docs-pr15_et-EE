<properties 
    pageTitle="SQL-süntaks ja SQL-i päringu jaoks DocumentDB | Microsoft Azure'i" 
    description="Lisateavet SQL-süntaks, andmebaasi põhimõtet ja SQL-päringud DocumentDB NoSQL andmebaasi. SQL-i saate kasutada DocumentDB keelena JSON päringu." 
    keywords="SQL-süntaks, sql-päringu, sql-päringud, json päringukeele, andmebaasi põhimõtet ja sql-päringud, kokkuvõttefunktsioone"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL-päringu ja DocumentDB SQL-süntaks
Microsoft Azure'i DocumentDB toetab JSON päringukeele SQL-i (Structured Query Language) kasutamine päringu dokumendid. DocumentDB on tõeliselt skeemi tasuta. Alusel oma kohustused otse andmebaasimootor JSON andmemudeliga, pakub JSON dokumentide automaatse indekseerimise nõudmata konkreetsete skeemi või sekundaarne registrite loomine. 

Päringu keele jaoks DocumentDB kujundamise ajal meil oli kaks eesmärgid meeles:

-   Asemel leiutas uue JSON päringukeele, saame kalendriüksusi SQL-i tugi. SQL-i on üks kõige tuttavad ja populaarsed päringu tekst. DocumentDB SQL-i pakub ametlik programmeerimise mudeli rikkaliku päringute üle JSON dokumendid.
-   JSON dokumendi andmebaasi tagama JavaScripti otse sisse andmebaasimootor, me otsustanud kasutada JavaScripti 's programmeerimise mudeli fondi meie päringukeele. JavaScripti on tippige süsteemi, avaldise hindamisel ja funktsioon kutsumise juured DocumentDB SQL-i. Selle sisse lülitada pakub JSON dokumente, ise ühendused, ruumiline päringud ja kasutaja määratletud funktsioonid (UDF) kirjutatud täiesti JavaScript muude funktsioonide kutsumise loomulikus programmeerimise mudeli relatsiooniline prognooside, hierarhilise navigeerimine. 

Me loomise turvalisuses kindel, et neid võimalusi on rakendus ja andmebaasi vahel vähendada ja on olulised arendaja tööviljakuse tõstmiseks.

Soovitame alustamine järgmisest videost, kus kuvatakse Tavahind Ramachandran DocumentDB's päringu võimalusi, vaadates ja meie [Mänguväljak päringu](http://www.documentdb.com/sql/demo), kus saate proovida DocumentDB ja SQL-päringud vastuolus meie andmekomplekti külastades.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Tagastab funktsioon selle artikli kui Alustuseks kirjeldame SQL-i päring õpetuse, mis juhendab teid mõned lihtsad JSON dokumentide ja SQL-i käskude.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>SQL-i käskude DocumentDB töötamise alustamine
Vaatamiseks tööl DocumentDB SQL-i loome algavad mõne lihtsa JSON dokumendid ja üle mõned lihtsad päringud selle vastu. Kaaluge nende kahe JSON dokumentide kaks peredele kohta. Pange tähele, et DocumentDB, kus me ei selgesõnaliselt mis tahes skeemid või sekundaarne indeksite loomiseks. Lihtsalt tuleb lisada DocumentDB saidikogumi JSON dokumendid ja seejärel päringu. Siin on lihtne JSON dokumendi Andersen pere, vanemad, Laste (ja suurendades), aadress ja registreerimist. Dokument on stringid, numbrite, loogikaväärtusi, massiivi ja pesastatud atribuudid. 

**Dokumendi**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Siin on teine dokumendi üks väike erinevus – `givenName` ja `familyName` asemel kasutatakse `firstName` ja `lastName`.

**Dokumendi**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Nüüd Proovime nüüd paar päringute vastu mõista mõningaid DocumentDB SQL-i võtme aspektide andmed. Näiteks järgmine päring tagastab dokumentide kus välja id vastab `AndersenFamily`. Kuna see on mõne `SELECT *`, päringu väljund on lõpule viidud JSON dokumendi:

**Päringu**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulemused**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Nüüd arvesse võtta juhul, kui peame JSON väljundi muu kujundi vormindada. Selle päringu projektide JSON uuele objektile koos kahe valitud väljadele nimi ja linn, kui selle aadressi linna on sama nimi nagu riik. Sel juhul vastab "NY, NY".

**Päringu**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Tulemused**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Järgmine päring tagastab antud nimed laste pere, kelle id vastet `WakefieldFamily` tellitud elu linna.

**Päringu**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Tulemused**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Soovime tähelepanu mõned märkimisväärsed aspektide DocumentDB päringukeele olen näinud nii palju näiteid kaudu:  
 
-   Kuna DocumentDB SQL-i töötab JSON väärtused, käsitletakse puu kujuga üksuste asemel ridu ja veerge. Seetõttu keele võimaldab viidata sõlmed puu mis tahes suvalise sügavust, nt `Node1.Node2.Node3…..Nodem`, sarnaselt relatsiooniline SQL-i viidates kaks osa viide `<table>.<column>`.   
-   Struktureeritud päringukeele töötab koos skeemi-vähem andmeid. Seetõttu süsteemi tüüp peab olema seotud dünaamiliselt. Sama avaldise võib anda erinevaid eri dokumentide osas. Päringu tulem on kehtiv väärtus, JSON, kuid ei saa tagada fikseeritud skeemi.  
-   DocumentDB toetab ainult range JSON dokumendid. See tähendab, et süsteemi tüüp ja avaldised on piiratud tegelemiseks JSON andmetüüpidega. Lugege rohkem üksikasju [JSON määratlus](http://www.json.org/) .  
-   DocumentDB saidikogumi on skeemi tasuta JSON dokumentide ümbris. Suhete andmete üksustes sees ja kogu kogumi dokumendid on peidetult jäädvustatud abil, mitte primaarvõtme ja võtme rahvusvaheliste suhete. See on vaja rõhutada, arvestades sisene-dokumendi ühendused, mida selles artiklis käsitletakse hiljem tähtis.

## <a name="documentdb-indexing"></a>DocumentDB indekseerimine

Enne kui me sattuda DocumentDB SQL-süntaks, tasub uurimine DocumentDB indekseerimise kujunduses. 

Andmebaasi registrid eesmärk olla päringud saavad erinevate vormide ja kujundite (nt CPU ja sisend) minimaalne ressursside tarbimine tagades hea jõudlus ja madal latentsus. Sageli õige registri andmebaasi päringute valik nõuab palju kavandamise ja katsetamine. Seda moodust kujutab skeemi-vähem andmebaase, kui andmed ei vasta range skeemi ja areneb kiiresti probleem. 

Seetõttu, luues DocumentDB, indekseerimine alasüsteemi, me määrata järgmised eesmärgid.

-   Dokumentide indekseerida nõudmata skeemi: indekseerimise alasüsteemi vajate skeemi teavet või muuta mis tahes oletused skeemi kohta dokumentide. 

-   Tõhusa, rikas hierarhilise ja seoselisi päringute tugi: registri toetab DocumentDB päringukeele tõhus, sh hierarhilise ja seoselisi prognooside tugi.

-   Ühtse päringute Face püsiv helitugevuse kirjutab tugi: kõrge kirjutada läbilaskevõime töökoormus ühtsete päringutega, registri värskendatakse sammhaaval, tõhus ja võrgus püsiv helitugevuse kirjutab ees. Ühtse registri värskendamine on oluline Teeni päringud, kus kasutaja konfigureeritud dokumendi teenuse järjepidevuse tasemel.

-   Mitme rendiõigus tugi: antud juhtimise ressursside broneerimiseks vastavalt mudeli rentnikud, index uuendused tehakse süsteemiressursside (CPU, mälu ja sisend toimingute sekundis) eraldatud koopia eelarve piires. 

-   Tõhusama salvestamise: maksumus tõhustada, kettal salvestusruumi pea kohal, indeks on piiratud ja hõlpsalt prognoosida. See on oluline, kuna DocumentDB võimaldab teha alusel kompromissidega vahel index kohal seoses päringu jõudluse arendaja.  

Vaadake [DocumentDB näidised](https://github.com/Azure/azure-documentdb-net) MSDN-is näidised, mis näitab, kuidas konfigureerida indekseerimise poliitika kogumi jaoks. Vaatame nüüd sattuda DocumentDB SQL-süntaksis üksikasju.


## <a name="basics-of-a-documentdb-sql-query"></a>Päringu DocumentDB SQL-i põhitõed
Iga päringu koosneb SELECT-klauslis ja valikuline saatja ja kus klauslitest ANSI SQL standardid kohta. Tavaliselt iga päringu allika FROM-klauslis on loetletud. Seejärel rakendatakse filtrit WHERE-klausli toomiseks JSON dokumentide alamhulga allikas. Lõpetuseks, kasutatakse SELECT-klauslis Project valige loendist soovitud JSON väärtused.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>FROM-klausel
Funktsiooni `FROM <from_specification>` klausel pole kohustuslik, kui allikas on filtreeritud või prognoositud hiljem päring. See klausel eesmärk määrata andmeallika, mille päringu tegutsema. Tavaliselt on terve kogumi allikas, kuid üks saate määrata alamhulk kollektsiooni asemel. 

Päringu nagu `SELECT * FROM Families` näitab, et kogu peredele saidikogumi mis loendada allikas. Teisiti identifikaator ROOT saab tähistada saidikogumi asemel saidikogumi nimi. Järgmine loend sisaldab reeglid, mis on päringu kohta:

- Selle saidikogumi võib olla näiteks neile tuleks määrata pseudonüümid, `SELECT f.id FROM Families AS f` või lihtsalt `SELECT f.id FROM Families f`. Siin `f` võrdub `Families`. `AS`alias on valikuline võtmesõna on identifikaator.

-   Teate, et kui neile tuleks määrata pseudonüümid, algallikas ei saa siduda. Näiteks `SELECT Families.id FROM Families f` on süntaks on vale, kuna identifikaator "Peredele" ei saa enam lahendada.

-   Kõik atribuudid, mida on vaja viidatakse peab olema täielikult kvalifitseeritud. Range skeemi järgimine puudumisel see rakendatakse mis tahes ebaselgete sidumiste vältimiseks. Seetõttu `SELECT id FROM Families f` on süntaks on vale, kuna see atribuut `id` on seotud.
    
### <a name="sub-documents"></a>Alamtäpi dokumendid
Allika saate vähendada ka väiksema alamhulk. Näiteks nummerdamine sub puu igas dokumendis, et sub juurkausta seejärel muutuda allikas, nagu on näidatud järgmises näites.

**Päringu**

    SELECT * 
    FROM Families.children

**Tulemused**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Kuigi eeltoodud näites kasutatakse massiivi allikana, objekti võib kasutada ka allikas, mis on, mis on näidatud järgmises näites. Mis tahes sobiv JSON väärtus (mitte määratlemata) leiate allika käsitletakse päringu tulemi kandmiseks. Kui mõni peredele pole mõne `address.state` väärtus, ei päringu tulem.

**Päringu**

    SELECT * 
    FROM Families.address.state

**Tulemused**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>WHERE-klausel
WHERE-klausli (**`WHERE <filter_condition>`**) olemasolu kohustuslik. Saate määrata, mis JSON dokumentide esitatud allika filtriavaldised peavad vastama, et kaasata tulemuse. Mis tahes JSON dokumendi peavad olema väärtustatavad "TRUE" käsitletakse tulemi jaoks määratud tingimustele. WHERE-klausli kasutatakse index kiht absoluutne väikseim alamhulk allika dokumente, mis võib olla osa määramiseks. 

Järgmine päring taotleb dokumendid, mis sisaldavad nime atribuut, mille väärtus on `AndersenFamily`. Muu dokumendi, mida ei saa atribuudi nimi, või kui väärtus ei vasta `AndersenFamily` on välja jäetud. 

**Päringu**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulemused**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Eelmise näite ilmnes lihtsa võrdse päringu. DocumentDB SQL-i toetab samuti mitmesuguseid skalaarse avaldised. Kõige sagedamini kasutatav on kahendarvuks ja unaarne avaldised. Atribuut lähteobjekt JSON viited on ka kehtiv avaldised. 

Binaarsed järgmisi tehtemärke on praegu toetatud ja seda saab kasutada päringute nagu on näidatud järgmistes näidetes:  
<table>
<tr>
<td>Aritmeetiline</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bititaseme</td>    
<td>|, &, ^, <<, >>, >>> (null-täitke õige shift) </td>
</tr>
<tr>
<td>Loogiline</td>
<td>JA, VÕI EI</td>
</tr>
<tr>
<td>Võrdlus</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>String</td> 
<td>|| (concatenate)</td>
</tr>
</table>  

Vaatame pilk mõned päringud kahendarvu tehtemärkide abil.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Unaarne tehtemärke +,-, ~ pole on samuti toetatud ja saab kasutada päringute sees, nagu on näidatud järgmises näites:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Lisaks kahendarvuks ja unaarne tehtemärgid, ka lubatud atribuut viited. Näiteks `SELECT * FROM Families f WHERE f.isRegistered` tagastab JSON dokumendi, mis sisaldab atribuut `isRegistered` kui atribuudi väärtus on võrdne selle JSON `true` väärtus. Kõik muud väärtused (false, null, määratlemata, `<number>`, `<string>`, `<object>`, `<array>`, jne), jääks tulemus lähtedokumendis viib. 

### <a name="equality-and-comparison-operators"></a>Võrdne ja võrdluse tehtemärgid
Järgmine tabel näitab tulemus võrdse võrdlemine DocumentDB SQL-i mis tahes kahe JSON vahel.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Määratlemata</strong>
         </td>
         <td valign="top">
            <strong>Null (Tühimärk)</strong>
         </td>
         <td valign="top">
            <strong>Kahendmuutuja</strong>
         </td>
         <td valign="top">
            <strong>Arv</strong>
         </td>
         <td valign="top">
            <strong>String</strong>
         </td>
         <td valign="top">
            <strong>Objekti</strong>
         </td>
         <td valign="top">
            <strong>Massiiv</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Määratlemata<strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null (Tühimärk)<strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Kahendmuutuja<strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Arv<strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>String<strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objekti<strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Määratlemata </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Massiiv<strong>
         </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
Määratlemata </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
      </tr>
   </tbody>
</table>

Nagu muude võrdlusmärgid jaoks >, > =,! =, < ja < =, järgmiste reeglite rakendamine:   

-   Võrdlust tüüpi tulemuseks määratlemata.
-   Võrdlus kahe objekti või kahe massiivi määratlemata tulemusi.   

Kui filter skalaarse Avaldise tulemus on määramata, vastava dokumendi ei sisaldaks tulem, kuna määratlemata ei võrdu loogiliselt "true".

### <a name="between-keyword"></a>Märksõna vahel
BETWEEN märksõna abil kiire päringute vastu Väärtustevahemikud nagu ANSI SQL-is. VAHEL võib kasutada stringide või arve.

Näiteks see päring tagastab kõik pere dokumendid, kus on esimene laps hinde vahemikus 1 – 5 (nii kaasa arvatud). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Erinevalt ANSI SQL, samuti saate BETWEEN klausel FROM klauslis nagu järgmises näites.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Kiirem päringu täitmise korda, pidage meeles indekseerimise poliitika, mis kasutab vahemiku registri tüüp, mis tahes arvuline atribuudid/teed, mis on filtreeritud BETWEEN-klauslis suhtes. 

Peamine erinevus BETWEEN DocumentDB ja ANSI SQL-i abil on mõned dokumendid ja stringid teistele ("grade4"), et saate kiire vahemiku päringute vastu kombineeritud tüüpi atribuudid – näiteks võib teil olla "klassi" arv (5). Sellisel juhul nt JavaScript, kaks erinevat tüüpi tulemuste "määratlemata" ja dokumendi võrdlus jäetakse vahele.

### <a name="logical-and-or-and-not-operators"></a>Loogika (ja, või mitte) tehtemärgid
Loogika tehtemärgid töötada kahendmuutujaga väärtused. Loogika etalonina tabelite jaoks järgmisi tehtemärke on näidatud järgmises tabelis.

VÕI|True|FALSE|Määratlemata
---|---|---|---
True|True|True|True
FALSE|True|FALSE|Määratlemata
Määratlemata|True|Määratlemata|Määratlemata

JA|True|FALSE|Määratlemata
---|---|---|---
True|True|FALSE|Määratlemata
FALSE|FALSE|FALSE|FALSE
Määratlemata|Määratlemata|FALSE|Määratlemata

POLE|  |
---|---
True|FALSE
FALSE|True
Määratlemata|Määratlemata

### <a name="in-keyword"></a>MÄRKSÕNA
IN märksõna saab kontrollida, kas määratud väärtuse vastab mis tahes loendist väärtus. Näiteks see päring tagastab kõik pere dokumendid kus id on üks "WakefieldFamily" või "AndersenFamily". 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

Selles näites tagastatakse kõik dokumendid, kui olek on mis tahes määratud väärtused.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Kolmekomponentsete (?) ja tehtemärke liitumisega (?)
Kolmekomponentsete ja liitumisega tehtemärgid saab koostada tingimusavaldistega, sarnaselt populaarsed programmeerimise keeled nagu C# ja JavaScripti. 

Tehtemärk kolmekomponentsete (?) võib olla väga kasulik, kui ehitamine uue JSON atribuudid pealt. Näiteks nüüd saate kirjutada päringute liigitada klassi tasemed üheks inimeste elektroonilisel kujul, nagu algaja/Vahelink/täpsemalt, nagu allpool näidatud.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Samuti saate pesastada kõnede tehtemärk like allpool päringu.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Nimega koos muude päringu tehtemärke, kui viidatud atribuutide tingimusvormingu avaldises on puudu, mis tahes dokumendis või erinevad tüübid ei võrreldes, siis need dokumendid jäetakse päringutulemites.

Atribuut olemasolu kontrollida tõhus (nimega saab kasutada liitumisega (?) tehtemärk on määratletud) dokumendi. See on kasulik, kui päringu vastu poolstruktuur või kombineeritud tüüpi andmeid. Näiteks see päring tagastab "perekonnanimi" kui see on olemas, või "perekonnanimi", kui see pole Esita.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Pakutud atribuudi juurdepääsumeetodi
Võite kasutada ka atribuudid abil pakutud atribuudi tehtemärk `[]`. Näiteks `SELECT c.grade` ja `SELECT c["grade"]` vastavad. Süntaksit on kasulik, kui teil on vaja atribuut, mis sisaldab tühikuid, erimärke, või jagamiseks sama nimi SQL-i märksõna või reserveeritud sõna juhtub Paoklahv (ESC).

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>SELECT-klauslis
SELECT-klauslis (**`SELECT <select_list>`**) on kohustuslikud ja määrab väärtused on toodud päringu, just nagu ANSI SQL. Alamhulk, mis on filtreeritud peal andmeallika dokumendid edastatakse projektsiooni etapp, kus laaditakse määratud JSON väärtused ja uue JSON objekti ehitatakse, iga sisendi ülekandmist selle peale. 

Järgmises näites on kujutatud tüüpiline valikpäringu. 

**Päringu**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulemused**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Pesastatud atribuudid
Järgmises näites oleme on kaks pesastatud atribuudid projitseerite `f.address.state` ja `f.address.city`.

**Päringu**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulemused**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projektsiooni toetab JSON avaldiste samuti, nagu on näidatud järgmises näites.

**Päringu**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulemused**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Heitkem pilk roll `$1` siin. Funktsiooni `SELECT` klauslis peab JSON objekti loomine ja Kuna te, kasutame peidetud argumendi nimed alustades `$1`. Näiteks see päring tagastab kahe peidetud argumendi muutuja, nimega `$1` ja `$2`.

**Päringu**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulemused**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Aliasing
Nüüd vaatame Ülaltoodud näites konkreetsete vähemtähtsad väärtuste laiendamine. KUI see on kasutatud aliasing märksõna. Pange tähele, et see on valikuline, nagu on näidatud ajal projitseerite teise väärtuse, mis `NameInfo`. 

Juhul, kui päring on kaks sama nimega atribuudid, tuleb kasutada aliasing ümber nimetada ühte või mõlemat atribuutide nii, et need on disambiguated plaanitud tulemis.

**Päringu**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulemused**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalaarse avaldised
Atribuut viited, lisaks SELECT-klauslis toetab skalaarse avaldiste nagu konstantide, aritmeetilisi avaldisi, loogilised avaldised jne. Siin on näiteks lihtsa "Tere, maailm" päringu.

**Päringu**

    SELECT "Hello World"

**Tulemused**

    [{
      "$1": "Hello World"
    }]


Siin on keerulisem näide, mis kasutab skalaaravaldis.

**Päringu**

    SELECT ((2 + 11 % 7)-2)/3   

**Tulemused**

    [{
      "$1": 1.33333
    }]


Järgmises näites on skalaarse expression tulemus on kahendväärtus.

**Päringu**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Tulemused**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Objekti ja Massiivivalemi loomine
DocumentDB SQL-i võtme funktsioonist on massiiv/objekti loomine. Eelmises näites, Pange tähele, et lõime uue JSON objekti. Samuti ühte saate ka koostada massiive, nagu on näidatud järgmistes näidetes.

**Päringu**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Tulemused**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>VÄÄRTUSE märksõna
**Väärtuse** märksõna võimaldab JSON-väärtuse. Näiteks alltoodud päring tagastab selle scalar `"Hello World"` asemel `{$1: "Hello World"}`.

**Päringu**

    SELECT VALUE "Hello World"

**Tulemused**

    [
      "Hello World"
    ]


Järgmine päring tagastab JSON väärtust ilma selle `"address"` tulemuste silt.

**Päringu**

    SELECT VALUE f.address
    FROM Families f 

**Tulemused**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Järgmises näites laiendab see näitab, kuidas JSON lihtsad väärtuste (JSON puu lehtede tase) tagastamiseks. 

**Päringu**

    SELECT VALUE f.address.state
    FROM Families f 

**Tulemused**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Tehtemärk
Teisiti tehtemärk (*) on toetatud dokumendi Project-on. Kui seda kasutatakse, peab olema ainult plaanitud välja. Näiteks päringu andmisel `SELECT * FROM Families f` on lubatud, `SELECT VALUE * FROM Families f ` ja `SELECT *, f.id FROM Families f ` ei sobi.

**Päringu**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulemused**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>ÜLEMISE tehtemärk
TOP märksõna saab päringu väärtuste arv piirata. ÜLEMINE kasutamisel koos klausel ORDER BY tulem on piiratud esimese N arv järjestatud väärtused; Vastasel korral tagastab selle esimese N tulemite arv määratlemata järku. Hea tava, SELECT-lauses Kasuta alati klausel ORDER BY ülemise klausliga. See on ainus võimalus ootuspäraselt näitavad, millised read mõjutab ALGUSSE. 


**Päringu**

    SELECT TOP 1 * 
    FROM Families f 

**Tulemused**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

ÜLEMINE saab konstant (nagu eespool näidatud) või muutuja väärtuse parameetritega päringute abil. Lisateabe saamiseks lugege parameetritega päringute alloleval.

## <a name="order-by-clause"></a>Klausel ORDER BY
Nagu ANSI SQL, saate lisada valikuline klausel Order By, samal ajal päringud. Klausel võib sisaldada valikuline ASC/LASKUV argument täpsustamiseks, kus tulemid tuuakse. Täpsemat teavet veebisaidil Order By, vaadake [DocumentDB järjestuses kiirtutvustus](documentdb-orderby.md).

Siin on näiteks päring, mis toob peredele elavate linnanime järjekorras.

**Päringu**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Tulemused**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

Ja siin on päring, mis toob peredele loomise kuupäev, mis on salvestatud epohhist tähistav arv järjekorras aega, st kulunud aeg alates 1 jaan 1970 rakenduses sekundit.

**Päringu**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Tulemused**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Täiustatud andmebaasi mõisted ja SQL-päringud
### <a name="iteration"></a>Iteratsiooni
Uue ehitada lisati **IN** märksõna toetada iterating üle JSON massiivi DocumentDB SQL-i kaudu. SAATJA allika toetab iteratsiooni. Alustame järgmises näites:

**Päringu**

    SELECT * 
    FROM Families.children

**Tulemused**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Nüüd vaatame teise päringu, mis sooritavad iteratsiooni laste kogumist. Pöörake tähelepanu soovitud massiivis. Selles näites jagab `children` ja lömastab sisse ühe massiivi tulemusi.  

**Päringu**

    SELECT * 
    FROM c IN Families.children

**Tulemused**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

See saab täpsemaks filtreerimiseks massiivi iga üksiku kirje, nagu on näidatud järgmises näites.

**Päringu**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Tulemused**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Ühendab
Relatsiooniandmebaasist, vajadust Liitu kogu tabelites on väga oluline. See on tuleb kujundamise normaliseeritud skeemid loogilise tasakaalustamiseks. Vastuolus DocumentDB tegeleb Denormaliseeritud andmemudeli skeemi tasuta dokumendid. See on loogiline võrdub a "iseendaga ühendamine".

Süntaks, mis toetab keele on < from_source1 > Liitu < from_source2 > Liitu... < From_sourceN > Liitu. Kokkuvõttes see tagastab kogumi **N**- kordseid (korteeži **N** väärtustega). Iga kordse on toodetud iterating kõigi saidikogumi aliases üle nende vastavate komplekti väärtused. Teisisõnu, see on täielik ristkorrutis komplekti osalemine ühendust.

Järgmised näited kujutavad JOIN-klausel toimimise. Järgmises näites tulem on tühi, alates ristkorrutis iga dokumendi andmeallikast ja on tühi hulk on tühi.

**Päringu**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Tulemused**  

    [{
    }]


Järgmises näites on ühendus dokumendi juurkausta ja `children` sub juurkausta. See on ristkorrutis kaks JSON objektide vahel. Asjaolu, et laste massiivi pole tõhus ühendus Kuna tegemist on ühe juur, mis on laste massiiv. Seega tulem sisaldab vaid kahe tulemuse, kuna ristkorrutis massiivi iga dokumendi annab täpselt ainult ühe dokumendi.

**Päringu**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Tulemused**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Järgmises näites on kujutatud rohkem tavalise Liitu:

**Päringu**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Tulemused**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Esmalt märkida, on see, et selle `from_source` **liitumine** klausel on iteraatori. Jah, voo sel juhul on järgmine:  

-   Laiendage iga lapse elemendi **c** massiivis.
-   Dokumendi **f** iga lapse elemendi **c** mis oli eristamine sõltub esmalt root ristkorrutis rakendada.
-   Lõpuks projekti juurkausta objekti **f** nime atribuudi eraldi. 

Esimese dokumendi (`AndersenFamily`) sisaldab ainult ühte tütarelement, nii tulemite hulk sisaldab ainult ühele objektile vastav selles dokumendis. Teine dokument (`WakefieldFamily`) sisaldab kahe alla. Jah, on ristkorrutis toodab eraldi objekti iga lapse, seega tulemuseks kaks objekti, üks iga lapse vastav selle dokumendi. Pange tähele, et mõlema dokumendi juurkausta väljad on sama, nagu ristkorrutis ootate.

Reaal kasulike ühendus on vormi kordseid ristkorrutis kujundi, mis on keeruline projekti. Lisaks nagu näeme järgmises näites, võib filtreerida korteeži laseb kasutaja valitud soovitud kordseid üldiselt vastama tingimus kombinatsiooni.

**Päringu**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Tulemused**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



Selles näites on eelmises näites loomulikus pikendamine ja sooritab kahekordsete Liitu. Jah, rist toote saab vaadata nimega pseudo järgmine kood.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`on üks laps, kellel on üks lemmikloom. Jah, on ristkorrutis annab ühe rea (1*1*1): see pere. WakefieldFamily, kuid on kaks last, kuid ainult ühe lapse "Jesse" on lemmikloomad. Jesse on küll 2 lemmikloomad. Seega on ristkorrutis liitmine annab väärtuse 1, 2 = 2*1*ridade selle pere.

Järgmine näide on ka täiendavad filter sisse `pet`. See välistab kõik kordseid, kus pet nimi on "Vari". Märkate, et saaksime on võimalik luua kordseid massiive, mis tahes elementide korteeži, filtreerimine ja project suvalist kombinatsiooni elementide. 

**Päringu**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Tulemused**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>JavaScripti integreerimine
DocumentDB pakub programmeerimise mudeli teostamiseks vastavalt JavaScripti rakenduse loogika otse sisse saidikogumid salvestatud toimingute ja käivitab. See võimaldab nii:

-   Võimalus teha suure jõudlusega selgituseks CRUD-toiminguid ja päringute vastu dokumentide kogumi alusel JavaScripti käitusaja otse andmebaasimootor sügav integreerimine. 
-   Loomulikus modelleerimine control flow, muutuv ulatuse määramine, ja ülesande ja integreerimine töötlemise primitiivid koos andmebaasitoiminguid erand. DocumentDB tugi JavaScripti integreerimise kohta lisateabe saamiseks vaadake JavaScripti serveri pool programmeeritavus dokumentatsiooni.

###<a name="user-defined-functions-udfs"></a>Kasutaja määratletud funktsioonid (UDF)
Käesolevas artiklis juba määratletud tüüpi koos DocumentDB SQL-i toetab jaoks kasutaja määratletud funktsioonid (UDF). Eelkõige skalaarse UDF-ID on toetatud, kus arendajad edastada null või mitu argumenti ja tagastab ühe argumendi tulem tagasi. Nende argumentide kontrollitakse juriidiline JSON väärtusi.  

DocumentDB SQL-süntaks on laiendatud toetamiseks kohandatud rakenduse loogika nende kasutaja määratletud funktsioonide abil. Siit leiate ülevaate DocumentDB registreeritakse ja seejärel sakki SQL-päringu osana. Tegelikult on UDF peenelt loodud päringud kasutada. Täiendab see valik, UDF-ID pole juurdepääsu kontekstis objekti, mis muud JavaScripti tüüpi (salvestatud toimingute ja käivitab). Kuna päringute käivitada kirjutuskaitstuna, ta saab käitada esmane või teisene koopiad. Seetõttu UDF-ID on mõeldud töötama teisene koopiad erinevalt muudest JavaScript.

Allpool on näide sellest, kuidas saab soovitud UDF registreerida DocumentDB andmebaas, Täpsemalt jaotises dokumendi saidikogumi.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
Eelmises näites loob UDF-i, mille nimi on `REGEX_MATCH`. See aktsepteerib JSON kaks stringiväärtust `input` ja `pattern` ja kontrollid, kui esimene vasteid muster määratud teine funktsiooniga JavaScripti 's string.match().


Nüüd saame kasutada selle UDF on projektsiooni päringu. Siit leiate ülevaate on kvalifitseeritud tõstutundlik eesliitega "udf." Kui kutsuda kaudu päringud. 

>[AZURE.NOTE] Enne 3/17/2015 DocumentDB toetatud UDF kõned ilma "udf". nt valige REGEX_MATCH() eesliide. See helistaja muster on taunitud.  

**Päringu**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Tulemused**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

UDF ka saab filter sees, nagu on näidatud järgmises näites, ka kvalifitseeritud "udf". eesliide:

**Päringu**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Tulemused**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


Sisuliselt UDF-ID on kehtiv skalaarse avaldiste ja saab kasutada nii prognooside ja filtreid. 

Laiendamiseks UDF power Heitkem pilk teine näide tingimusvormingu loogika:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Allpool on näide, mis kasutab UDF.

**Päringu**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Tulemused**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Nagu eelmistes näidetes tõstaks esile, integreerida UDF JavaScripti keele power DocumentDB SQL esitada rikkaliku programmeeritavat kasutajaliidese keerukate üksikasjalik, tingimusvormingu loogika sisseehitatud JavaScripti käitusaja võimaluste abil teha.

DocumentDB SQL-i pakub soovitud UDF argumendid praeguses (WHERE-klausel või SELECT-klauslis) töötlemise UDF iga dokumendi allikas. Tulem on lisatud üldine täitmise müügivõimaluste sujuvalt. Kui atribuutide alusel UDF ei ole parameetreid JSON-väärtuse, parameetri peetakse määratlemata ja seega UDF kutsumise on täiesti vahele. Samamoodi kui UDF on määramata, see pole kaasatud tulemi. 

Kokkuvõttes UDF-ID on suurepärane tööriistad teha keerukate äriloogika päringu osana.

### <a name="operator-evaluation"></a>Tehtemärk hindamine
DocumentDB, kaudu on JSON andmebaasi, kõrvutab JavaScripti tehtemärgid ja selle hindamise semantika. Rakendust DocumentDB püütakse säilitada JavaScripti semantika osas JSON tugi, erineb toiming väärtustamise mõnel juhul.

DocumentDB SQL, erinevalt traditsiooniline SQL, tüüpi väärtused on sageli teadmata kuni väärtused on tegelikult andmebaasist. Tõhus käivitada päringuid, et enamik ettevõtjaid on range tüüp nõuetele. 

DocumentDB SQL-i ei teha peidetud dokumenditeisenduste, JavaScripti erinevalt. Näiteks päringu nagu `SELECT * FROM Person p WHERE p.Age = 21` vastab dokumendid, mis sisaldavad ka vanus atribuut, mille väärtus on 21. Muu dokumendi nime, kelle vanus atribuudi vastab string "21" või muu potentsiaalselt lõpmatu variatsioonid, näiteks "021", "21.0", "0021", "00021", mitte sobitatakse jne. See on aga JavaScripti, kui stringi väärtused on peidetult valatud arvude (põhjal tehtemärk, ex: ==). See valik on oluline tõhusa index sobitamine DocumentDB SQL. 

## <a name="parameterized-sql-queries"></a>Parameetritega SQL-päringud
DocumentDB toetab päringute väljendatud tuttav koos parameetriga @ märke. Parameetritega SQL pakub töökindlaid töötlemise ja pääseks kasutaja sisend, vältimine silma SQL süst kaudu. 

Näiteks saate kirjutada päring, mis võtab perekonnanimi ja aadress olekus parameetrite ja sooritage erinevate väärtuste põhjal kasutaja sisendist perekonnanimi ja aadress.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Taotluse seejärel saatmist DocumentDB nimega JSON parameeterpäring nagu allpool näidatud.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Argument üles, saate määrata parameetritega päringute, nagu allpool näidatud.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parameetrite väärtused võib olla mis tahes kehtiv JSON (stringid, arve, loogikaväärtusi null, isegi massiivi või pesastatud JSON). Ka DocumentDB on skeemi-vähem, parameetrid ei ole kontrollitud suvalise.

##<a name="built-in-functions"></a>Sisseehitatud funktsioonid
DocumentDB toetab ka sisseehitatud funktsioonide arv tavalised toimingud, mida saab kasutada päringute nagu kasutaja määratletud funktsioonid (UDF).

<table>
<tr>
<td>Matemaatiliste ülesannete</td> 
<td>ABS CEILING EXP, FLOOR, LOG, LOG10, POWER, ROUND, märk, SQRT, ruutu, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, kraadi, pii RADIAANI, SIN ja TAN</td>
</tr>
<tr>
<td>Tippige funktsioonide kontrollimine</td>    
<td>IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED ja IS_PRIMITIVE</td>
</tr>
<tr>
<td>Stringifunktsioonide</td>   
<td>CONCAT, sisaldab, ENDSWITH, INDEX_OF, vasakul, pikkus, LOWER, funktsioonid LTRIM, Asenda, juhendist, TAGURPIDI, paremale, RTRIM, STARTSWITH, ALAMSTRINGI ja ülemise</td>
</tr>
<tr>
<td>Massiivi funktsioonid</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH ja ARRAY_SLICE</td>
</tr>
<tr>
<td>Ruumilise funktsioonid</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID ja ST_ISVALIDDETAILED</td>
</tr>
</table>  

Kui kasutate praegu kasutaja määratletud funktsioon (UDF), mille sisseehitatud on nüüd saadaval, kasutage funktsiooni vastavate sisseehitatud nagu see saab käivitada kiirem ja tõhusam. 

### <a name="mathematical-functions"></a>Matemaatiliste ülesannete
Matemaatiliste ülesannete teha arvutusi, tavaliselt sisendväärtuste, mis on esitatud argumentidena ja tagastada arvulise väärtuse alusel. Siin on toetatud matemaatilise valmisfunktsioonide tabelisse.

<table>
<tr>
<td><strong>Kasutus</strong></td>
<td><strong>Kirjeldus</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>Tagastab määratud arvuline avaldis absoluutne (positiivne) väärtus.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">CEILING (num_expr)</a></td> 
<td>Tagastab vähima väärtuse täisarv suurem või võrdne määratud arvuline avaldis.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">FLOOR (num_expr)</a></td> 
<td>Suurim täisarv väiksem või võrdne määratud arvuline avaldis tagastab.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>Tagastab määratud arvuline avaldis aste.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOG (num_expr [, alus])</a></td> 
<td>Annab vastuseks gammafunktsiooni naturaallogaritmi määratud arvuline avaldis või logaritmi määratud alusega abil</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">Log10 (num_expr)</a></td> 
<td>Tagastab määratud arvuline avaldis logaritmiline väärtuse alusel 10.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">Arvu ÜMARDAMINE (num_expr)</a></td> 
<td>Tagastab arvulise väärtuse, ümardatakse väärtus lähima täisarvuni.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">TRUNC (num_expr)</a></td> 
<td>Tagastab arvulise väärtuse, kärbitakse väärtus lähima täisarvuni.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">SQRT (num_expr)</a></td>   
<td>Annab vastuseks ruutjuure korrutisest määratud arvuline avaldis.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">RUUTU (num_expr)</a></td>   
<td>Annab vastuseks määratud arvuline avaldis.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">POWER (num_expr, num_expr)</a></td>   
<td>Tagastab määratud arvuline avaldis power määratud väärtuse.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">MÄRK (num_expr)</a></td>   
<td>Tagastab määratud arvuline avaldis Logi väärtuse (-1, 0, 1).</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ACOS (num_expr)</a></td>   
<td>Annab vastuseks nurga, radiaanides, mille koosinus on määratud arvuline avaldis; nimetatakse ka arkuskoosinuse.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ASIN (num_expr)</a></td>   
<td>Annab vastuseks nurga, radiaanides, mille siinus on määratud arvuline avaldis. Seda nimetatakse ka arkussiinuse.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ATAN (num_expr)</a></td>   
<td>Annab vastuseks nurga, radiaanides, mille tangens on määratud arvuline avaldis. Seda nimetatakse ka arkustangensi.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>Tagastab nurk, radiaanides, positiivne x-telje ja kiir kaudu päritolu punkti (y, x), kus x ja y on kaks määratud hõljumine avaldiste väärtused.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>Annab vastuseks radiaanides määratud avaldise Trigonomeetrilised määratud nurga koosinuse.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>Annab vastuseks radiaanides määratud arvuline avaldis Trigonomeetrilised määratud nurga kootangensi.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">KRAADI (num_expr)</a></td> 
<td>Annab vastuseks radiaanides määratud nurga kraadi vastavate nurk.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI)</a></td>   
<td>Tagastab pii selle järele konstandi väärtus.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RADIANS (num_expr)</a></td> 
<td>Tagastab radians arvuline avaldis, kraadides, sisestamise korral.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>Annab vastuseks radiaanides määratud avaldise Trigonomeetrilised määratud nurga siinuse.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (num_expr)</a></td> 
<td>Tagastab määratud avaldise Sisestuskeel avaldise tangensi.</td>
</tr>

</table> 

Näiteks saate nüüd käivitada päringute umbes selline:

**Päringu**

    SELECT VALUE ABS(-4)

**Tulemused**

    [4]

Peamine erinevus võrreldes ANSI SQL-i DocumentDB's funktsioonid on, et need on loodud töötama ka skeemi-vähem- ja kombineeritud skeemi andmetega. Näiteks kui teil on dokumendi kui atribuudi maht on puudu või on mittenumbriline väärtus, näiteks "tundmatu", siis dokument on vahele tõrke sisaldamise asemel.

### <a name="type-checking-functions"></a>Tippige funktsioonide kontrollimine
Tippige veakontrolli funktsioonid võimaldavad teil vaadata avaldise sees SQL-päringud tüüp. Tippige veakontrolli funktsioone saab kasutada jooksul dokumente tüüpi kindlaks teha, kui see on muutuv või tundmatu. Siin on esitatud funktsioonid kontrollimine toetatud sisseehitatud tüüpi.

<table>
<tr>
  <td><strong>Kasutus</strong></td>
  <td><strong>Kirjeldus</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (avaldis)</a></td>
  <td>Annab vastuseks kahendväärtus, mis näitab, kui väärtus on massiiv.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (avaldis)</a></td>
  <td>Tagastab on kahendväärtus, mis näitab, kui väärtus tüüp on kahendväärtus.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (avaldis)</a></td>
  <td>Annab vastuseks kahendväärtus, mis näitab, kui väärtus on null.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (avaldis)</a></td>
  <td>Annab vastuseks kahendväärtus, mis näitab, kui väärtus on arv.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (avaldis)</a></td>
  <td>Annab vastuseks kahendväärtus, mis näitab, kui väärtus on JSON objekt.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (avaldis)</a></td>
  <td>Annab vastuseks kahendväärtus, mis näitab, kui väärtus on string.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (avaldis)</a></td>
  <td>Annab vastuseks kahendväärtus, mis näitab, kui atribuut on määratud väärtuse.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (avaldis)</a></td>
  <td>Annab vastuseks kahendväärtus, mis näitab, kui väärtus on string, arv, kahendväärtus või null.</td>
</tr>

</table>

Nende funktsioonide abil saate nüüd käivitada päringute umbes selline:

**Päringu**

    SELECT VALUE IS_NUMBER(-4)

**Tulemused**

    [true]

### <a name="string-functions"></a>Stringifunktsioonide
Järgmised skalaarfunktsioonid Sisestuskeel stringiväärtus toimingu ja tagastab stringi, kahendväärtus või arvulise väärtuse. Siin sisseehitatud stringifunktsioonide tabelisse.

Kasutus|Kirjeldus
---|---
[PIKKUS (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Tagastab määratud stringi avaldise märkide arv
[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Tagastab stringi, mis on põhjustanud ühendades kahe või enama stringi väärtuse.
[ALAMSTRINGI (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Tagastab stringi avaldise osa.
[STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Annab vastuseks on kahendväärtus, mis näitab, kas esimese tekstistringi avaldis teine lõpeb
[ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Annab vastuseks on kahendväärtus, mis näitab, kas esimese tekstistringi avaldis teine lõpeb
[SISALDAB (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Annab vastuseks on kahendväärtus, mis näitab, kas esimese tekstistringi avaldis sisaldab teine.
[INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Tagastab esimese esinemise teine alguskoha tekstistringi avaldis esimese määratud stringavaldis või -1, kui stringi ei leita.
[VASAKULE (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Tagastab määratud arvu märke stringi vasak osa.
[PAREMALE (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Tagastab määratud arvu märke stringi õige osa.
[Funktsioonid LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Tagastab stringi avaldis pärast see eemaldab joondada.
[RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Tagastab stringi avaldis pärast kõigi lõputühikud Kärbi.
[LOWER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Tagastab stringi avaldis pärast suurtähtedeks märgi andmete teisendamine väiketähtedeks.
[ÜLEMISE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Tagastab stringi avaldis pärast väiketähtedeks märgi andmete teisendamine suurtäheliseks.
[ASENDAGE (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Asendab määratud stringi väärtuse kõigi esinemiskordade teise stringi väärtus.
[JUHENDIST (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Kordab stringiväärtus määratud arv kordi.
[TAGURPIDI (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|Tagastab stringi väärtuse vastupidises järjekorras.

Nende funktsioonide abil saate käivitada nüüd päringute umbes järgmist. Näiteks saate naasta perekonnanime suurtähtedes järgmiselt:

**Päringu**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Tulemused**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Või concatenate stringid, nagu järgmises näites:

**Päringu**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Tulemused**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Stringifunktsioonide saate kasutada ka WHERE-klausli filter tulemusi, nagu järgmises näites:

**Päringu**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Tulemused**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Massiivi funktsioonid
Järgmised skalaarfunktsioonid toimingu massiivi väärtuse ja arvuliste tagasi, kahendväärtus või massiivi väärtuse. Siin on esitatud sisseehitatud massiivi funktsioone:

Kasutus|Kirjeldus
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Tagastab määratud massiivi avaldise elementide arv.
[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Tagastab massiivi, mis on põhjustanud ühendades kahe või enama massiivi väärtusi.
[ARRAY_CONTAINS (arr_expr; avaldis)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Annab vastuseks kahendväärtus, mis näitab, kas massiiv sisaldab määratud väärtuse.
[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Tagastab massiivi avaldise osa.

Massiivi funktsioone saab kasutada massiivi sees JSON käsitsemiseks. Siin on näiteks päring, mis tagastab kõik dokumendid, kus on üks vanematest "Robin Wakefield". 

**Päringu**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Tulemused**

    [{
      "id": "WakefieldFamily"
    }]

Siin on teine näide, mis kasutab ARRAY_LENGTH saada pere kohta laste arvuga.

**Päringu**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Tulemused**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Ruumilise funktsioonid

DocumentDB toetab ruumilisel päringute avatud ruumilisel Consortium (OGC) sisseehitatud järgmisi funktsioone. Üksikasjalikumat teavet ruumilisel DocumentDB tuge, leiate [Azure'i DocumentDB ruumilisel andmete töötamine](documentdb-geospatial.md). 

<table>
<tr>
  <td><strong>Kasutus</strong></td>
  <td><strong>Kirjeldus</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Tagastab kahe GeoJSON punkti avaldiste vahel kaugus.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Tagastab loogikaavaldis, mis näitab, kas esimese argumendi määratud punkti GeoJSON jääb GeoJSON Hulknurk teises argumendis.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Tagastab kahendväärtuse, mis näitab, kas määratud GeoJSON punkt või Hulknurk avaldis on lubatud.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Tagastab sisaldava on kahendväärtus JSON väärtus väärtus, kui määratud GeoJSON punkt või Hulknurk avaldis on lubatud ja kui see on kehtetu, lisaks põhjus stringi väärtus.</td>
</tr>
</table>

Ruumilise funktsioone saab kasutada lähedus querries ruumilise andmetega täita. Siin on näiteks päring, mis tagastab kõik pere dokumendid, mis on määratud asukohta, kasutades funktsiooni ST_DISTANCE sisseehitatud 30 km piiridesse. 

**Päringu**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Tulemused**

    [{
      "id": "WakefieldFamily"
    }]

Kui te oma indekseerimise poliitika ruumilise indekseerimine, siis "kauguse päringute" pakutakse tõhus registri kaudu. Ruumilise indekseerimine rohkem üksikasju, leiate allpool olevat jaotist. Kui teil pole ruumilise indeks jaoks määratud teed, saate siiski teha ruumilise päringud, määrates `x-ms-documentdb-query-enable-scan` taotluse päis on seatud väärtus "true". .Net-i, saate seda teha saates **FeedOptions** valikulist argumenti päringute [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) on seatud tõene. 

ST_WITHIN saab kontrollida, kui punkti asub hulknurga. Hulknurga on levinud tähistada piirmäärad nagu sihtnumbrite, state piirmäärad või loomulikus koosseise. Uuesti kui kaasate ruumilise indekseerimine oma indekseerimise poliitika, siis "sees" päringute pakutakse tõhus registri kaudu. 

Hulknurga ST_WITHIN argumendid võivad sisaldada ainult ühe ring, st ei tohi sisaldada soovitud hulknurga auke neid. Märkige ruut [DocumentDB piirangud](documentdb-limits.md) lubatud hulknurga ST_WITHIN päringu punktide arvu ülempiir.

**Päringu**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Tulemused**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Sarnane kuidas sobimatud andmetüübid töötab DocumentDB päringus, kui väärtus asukoht määratud üks argument on vigane või ei sobi ja seejärel selle hindab **määratlemata** ja hinnatud dokumendi põhjal päringutulemite vahele jätta. Kui teie päring tagastab tulemeid, käivitage ST_ISVALIDDETAILED abil silumine miks spatail tüüp on kehtetu.     

ST_ISVALID ja ST_ISVALIDDETAILED saab kontrollida, kui ruumiline objekt on lubatud. Näiteks järgmine päring kontrollib kehtivust punkti koos kontorist väljas vahemiku laiuskraad väärtus (-132.8). ST_ISVALID tagastab ainult kahendväärtuse ja ST_ISVALIDDETAILED tagastab selle kahendmuutuja ja string, mis sisaldab põhjus, miks peetakse sobimatu.

**Päringu**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Tulemused**

    [{
      "$1": false
    }]

Neid funktsioone saab kasutada ka hulknurga kinnitamiseks. Näiteks siin kasutame ST_ISVALIDDETAILED valideerimiseks hulknurga, mis on suletud. 

**Päringu**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Tulemused**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Mis murtakse ruumilise funktsioonide ja SQL-süntaks DocumentDB. Nüüd vaatame vaadata, kuidas LINQ päringute töötab, ja kuidas seda suhtleb süntaks oleme näinud siiani.

## <a name="linq-to-documentdb-sql"></a>LINQ DocumentDB SQL-i
LINQ on .NET programmeerimise mudelit, mis väljendab arvutus nimega voole objektide päringud. DocumentDB annab kliendile küljel teegi liides LINQ teisendus objektide JSON ja .net-i ja mille LINQ vastenduse hõlbustades päringute DocumentDB päringutele. 

Alloleval pildil on toetavad LINQ päringute abil DocumentDB arhitektuur.  Kasutades DocumentDB klient, arendajad saate luua otse päringute DocumentDB päringu pakkuja, mis seejärel tõlgib LINQ päringu DocumentDB päringu **IQueryable** objekti. Päringu edastatakse seejärel DocumentDB serveri toomiseks kogumi tulemuste JSON-vormingus. Tagastatud tulemuste on deserialized voo, .NET objektid kliendis.

![Arhitektuur toetavad LINQ päringute abil DocumentDB - SQL-süntaks, JSON päringukeel, andmebaasi mõisted ja SQL-päringud][1]
 


### <a name="net-and-json-mapping"></a>.Net-i ja JSON kaardistamine
.Net-i objektide ja JSON dokumentide kaardistamine loomulikus - iga andmete väli on vastendatud JSON objekt, kus välja nimi on vastendatud objekti osa "võti" ja "value" osa on rekursiivselt vastendatud objekti väärtuse osa. Näide. Pere objekti loodud on vastendatud JSON dokument nagu allpool näidatud. Ja vastupidi, JSON dokument on vastendatud tagasi .NET objekti.

**C# klassi**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>SQL-i tõlge LINQ
DocumentDB päringu pakkuja teostab parim peegeldav vastenduse LINQ päringu üheks DocumentDB SQL-päring. Järgmine kirjeldus, saame endale lugeja on lihtne tuttava LINQ.

Esmalt süsteemi tüüp, toetame kõik JSON lihtsad tüüpide – numbriline tüübid, kahendväärtus, stringi ja null. Toetatakse ainult JSON järgmist tüüpi. Skalaarse järgmised avaldised on toetatud.

-   Konstandi väärtused – need sisaldab konstandi lihtsad andmetüübiga väärtusi päringu hinnatakse ajal.

-   Atribuut/massiiv index avaldiste – need avaldised viidata objekti või massiivi element.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Aritmeetilised avaldiste - nendeks levinud aritmeetilisi avaldisi numbriliste ja kahendmuutujaga väärtuste. Täieliku loendi leiate SQL-i määratlus.

        2 * family.children[0].grade;
        x + y;

-   Stringavaldis võrdlus - nendeks võrdlus stringiväärtuse, mõned järele konstandi väärtus.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Objekti/massiivi loomine avaldise – need avaldised saatja objekti tüüp väärtus ühendi või anonüümse või selliste objektide massiivi. Need väärtused saate pesastada.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Loendi toetatud LINQ tehtemärgid
Siit leiate loendi toetatud LINQ tehtemärgid kaasas DocumentDB .NET SDK LINQ pakkuja.

-   **Valige**: prognooside tõlkimine SQL-i valimiseks, sealhulgas objekti ehitamine
-   **Kus**: filtrite tõlkida SQL-i WHERE ja toetavad vahel, & &, || ja! Kui soovite SQL-i tehtemärgid
-   **SelectMany**: võimaldab ühtki massiivi SQL-i liitumine punktile. Saab kasutada kettäärisega/pesa avaldiste filtreerimiseks massiivi elementide kohta
-   **OrderBy ja OrderByDescending**: vaste JÄRJESTUSALUS tõusvas/laskuvas järjestuses:
-   **CompareTo**: vaste vahemiku võrdlemine. Tavaliselt kasutatakse stringid, kuna need on pole võrreldav .net-i
-   **Võtta**: vaste SQL-i üles piiramiseks päringu tulemused
-   **Matemaatikafunktsioonide**: toetab tõlge. NET Abs Acos, Asin, Atan, Ceiling Cos, Exp, Floor, Log, Log10, Pow, Round, märk, Sin, Sqrt, Tan, Truncate võrdväärse SQL-i sisseehitatud funktsioonide.
-   **Stringifunktsioonide**: toetab tõlge. NET Concat, sisaldab, EndsWith, IndexOf, arv, ToLower, TrimStart, Asenda, vastupidi, TrimEnd, StartsWith, alamstringi, ToUpper samaväärne SQL-i sisseehitatud funktsioonide.
-   **Massiivi funktsioonid**: toetab tõlge. NET Concat, sisaldab ja arv, et samaväärne SQL-i sisseehitatud funktsioonid.
-   **Ruumilisel laiend funktsioonid**:-võrdväärse SQL-i sisseehitatud funktsioonid konts meetodite kauguse sees IsValid ja IsValidDetailed tõlke toetab.
-   **Kasutaja määratletud funktsioon laiend funktsioon**: toetab tõlge konts meetodit UserDefinedFunctionProvider.Invoke vastava kasutaja määratletud funktsioon.
-   **Muud**: toetab liitumisega ja tingimusvormingu tehtemärgid tõlkimine. Saate tõlkida sisaldab String sisaldab, ARRAY_CONTAINS või SQL-i tolli sõltuvalt kontekstis.

### <a name="sql-query-operators"></a>SQL-päringu tehtemärgid
Siin on mõned näited, mis illustreerivad, kuidas teatud standardse LINQ päringu tehtemärgid tõlgitakse allapoole DocumentDB päringud.

#### <a name="select-operator"></a>Valige operaator
Süntaks on `input.Select(x => f(x))`, kus `f` on skalaaravaldis.

**LINQ lambda avaldis**

    input.Select(family => family.parents[0].familyName);

**SQL-I** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda avaldis**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL-I** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda avaldis**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL-I** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany tehtemärk
Süntaks on `input.SelectMany(x => f(x))`, kus `f` on skalaaravaldis, mis tagastab saidikogumi tüüp.

**LINQ lambda avaldis**

    input.SelectMany(family => family.children);

**SQL-I** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Kui tehtemärk
Süntaks on `input.Where(x => f(x))`, kus `f` on skalaaravaldis, mis Tagastab kahendväärtuse.

**LINQ lambda avaldis**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL-I** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda avaldis**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL-I** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Kombineeritud SQL-päringud
Ülaltoodud tehtemärgid võivad koosneda vormi võimsam päringud. Kuna DocumentDB toetab pesastatud saidikogumid, koosseis saate liitsõnumeid või pesastatud.

#### <a name="concatenation"></a>Ühendamine 

Süntaks on `input(.|.SelectMany())(.Select()|.Where())*`. Ühendatud päringu saate alustada vabatahtlik `SelectMany` päringu, millele järgneb mitu `Select` või `Where` tehtemärke.


**LINQ lambda avaldis**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL-I**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda avaldis**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL-I** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda avaldis**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL-I** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda avaldis**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL-I** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Pesastamine

Süntaks on `input.SelectMany(x=>x.Q())` kus k on mõne `Select`, `SelectMany`, või `Where` tehtemärk.

Pesastatud päringus sisemiste päringu rakendatakse väline saidikogumi iga element. Üks pole, et sisemine päring võib viidata väljad väline saidikogumi nagu elementide iseliitmised.

**LINQ lambda avaldis**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL-I** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda avaldis**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL-I** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda avaldis**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL-I** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>SQL-päringute täitmine
DocumentDB seab REST API-ga, saab helistada võimeline tegema HTTP-või HTTPS taotlusi keelega abil. Lisaks DocumentDB pakub programmeerimise teekide populaarsed mitut keelt, nt .net-i, Node.js, JavaScript ja Python. REST API-ga ja erinevate teekide toetavad päringu SQL-i kaudu. .NET SDK toetab LINQ päringute Lisaks SQL-i.

Järgmistes näidetes päringu loomine ja esitab DocumentDB andmebaasi konto suhtes.

### <a name="rest-api"></a>REST API-GA
DocumentDB pakub avatud rahulik programmeerimise mudeli http kaudu. Andmebaasi kontod saab ette valmistatud Azure tellimuse abil. DocumentDB ressursi mudel sisaldab komplekti ressursside andmebaasi kasutajakontot, millest igaüks on adresseeritavad loogilise ja ühed URI abil. Ressursside hulga edaspidi kanalisse selles dokumendis. Andmebaasi konto koosneb andmebaase, iga mitme saidikogumid, mis sisaldab iga mis sisse lülitada sisaldavad dokumendid, UDF-ID ja muud tüüpi ressursi.

Lihtsa suhtluse mudeli need ressursid on verbide HTTP kaudu toomine, panna, postitus ja Kustuta standard tõlgendamise. POSTITUSE tegusõna kasutatakse luua uue ressursi, salvestatud protseduuri käitamisel või emissiooni DocumentDB päringu. Päringute on alati lugeda ainult toimingud ilma küljel – mõjusid.

Järgmised näited kujutavad POSTITUSES DocumentDB päringu kogumik, mis sisaldab kahe valimi dokumentide vastu oleme siiani läbi vaadanud. Päring on lihtne filter JSON nime atribuudi alusel. Pange tähele kasutamine on `x-ms-documentdb-isquery` ja sisutüüp: `application/query+json` päised, mis tähistab, et toiming on päringu.


**Taotlemine**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Tulemused**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Teine näide keerukamaid päring, mis tagastab mitu tulemit ühendust.

**Taotlemine**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Tulemused**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Kui mõne päringu tulemused ei mahu tulemuste ühe lehe maksimaalselt, siis REST API tagastab jätkamiseks luba kaudu soovitud `x-ms-continuation-token` vastuse päis. Kliendid saavad paginate tulemused, sh päis edasiste tulemuste. Lehel tulemite arv saab kontrollida ka selle `x-ms-max-item-count` arv päis.

Andmete järjepidevuse poliitika päringute haldamine, kasutage funktsiooni `x-ms-consistency-level` päise nagu kõik REST API kutsed. Seansi järjepidevuse, on vaja ka kaja Viimane `x-ms-session-token` päringu taotluse küpsise päis. Pange tähele, et päringu saidikogumi indekseerimise poliitika võib mõjutada ka järjepidevuse päringutulemites. Poliitikasätted indekseerimine vaikimisi saidikogumid indeks on alati praeguse dokumendi sisu ja päringu tulemused vastavad järjepidevuse valitud andmete jaoks. Kui indekseerimise poliitika on leevendada, et laisk, siis päringud saate tagastada aegunud tulemusi. Lisateabe saamiseks vaadake [DocumentDB järjepidevuse tasemed] [consistency-levels].

Kui konfigureeritud indekseerimise poliitika kogumise ei toeta määratud päringut, tagastab funktsioon DocumentDB serveri 400 "vigane päring". See tagastatakse vahemiku päringuid vastu konfigureeritud räsi (võrdne) otsinguid ja töötavaid indekseerimise teed teed. Funktsiooni `x-ms-documentdb-query-enable-scan` päise võib määrata, et lubada päring sooritate kui registri pole saadaval.

### <a name="c-net-sdk"></a>C# (.NET) SDK
.NET SDK toetab nii LINQ ja SQL-i päringud. Järgmises näites kujutatakse Lihtfilter päringu selle dokumendi sisse.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Selle valimi võrdleb kahe raames iga dokumendi atribuudid ja kasutab anonüümse prognooside. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Järgmine näidis kuvatakse ühendused, väljendub LINQ SelectMany.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



.Net-i kliendi automaatselt itereerib läbi päringutulemites foreach Blocks nagu eespool näidatud lehtedelt. Päringu kasutusele REST API jaotises Suvandid on saadaval .NET SDK abil ka selle `FeedOptions` ja `FeedResponse` tunde CreateDocumentQuery meetod. Lehekülgede arv saab kontrollida, kasutades funktsiooni `MaxItemCount` säte. 

Saate määrata ka otseselt lehtede nummerdamine, luues `IDocumentQueryable` abil soovitud `IQueryable` objekti, seejärel lugedes on` ResponseContinuationToken` väärtused ja saata need tagasi nimega `RequestContinuationToken` sisse `FeedOptions`. `EnableScanInQuery`Saate määrata lubamiseks skannib, kui päring ei toeta, konfigureeritud indekseerimise poliitika. Sektsioonitud saidikogumid, saate kasutada `PartitionKey` vastu ühe päringu käivitamiseks partition (kuigi DocumentDB saate automaatselt ekstraktida selle päringu teksti), ja `EnableCrossPartitionQuery` päringud, mis võib tekkida vajadus käivitada vastu mitu sektsiooni käivitamiseks. 

Vaadake veel näidiseid päringute [DocumentDB .NET näidised](https://github.com/Azure/azure-documentdb-net) . 

### <a name="javascript-server-side-api"></a>JavaScripti serveripoolne API 
DocumentDB pakub programmeerimise mudeli teostamiseks vastavalt JavaScripti rakenduse loogika otse saidikogumid, Salvestatud toimingute ja käivitab kasutamise kohta. JavaScripti loogika registreeritud saidikogumi tasemel, saate siis välja andmebaasi toimingud antud saidikogumi dokumendid toimingutest. Need toimingud on pakitud ümbritseva HAPPE tehingud.

Järgmises näites näitab, kuidas kasutada funktsiooni queryDocuments server JavaScripti API päringutele sees salvestatud toimingute ja käivitab.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Kokkuvõttefunktsioonid

Kokkuvõttefunktsioone kohalikke tugi on töötab, kuid kui teil on vaja funktsiooni count või summa seni, võite saavutada sama tulemuse eri viisidel.  

Lugege tee:

- Andmete toomine ja tehes sõnaarvestuse kohalikult, saate teha kokkuvõttefunktsioone. Soovitatav on kasutada odavad päringu projektsiooni nagu `SELECT VALUE 1` asemel täielik dokumendi `SELECT * FROM c`. See aitab maksimeerimine iga lehel tulemused, vältida täiendavate vormimallikujundaja teenusega vajaduse korral töödeldud dokumentide arv.
- Salvestatud toimingu abil minimeerida Võrgu latentsuse korduvate edasi-tagasi. Valimi salvestatud protseduur, mis arvutab arvu antud filtri päring, vt [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js). Salvestatud protseduur saate lubada kasutajatel teha liitmised tõhusalt koos rikkaliku äriloogika ühendamiseks.

Kirjutage tee:

- Mõne muu levinud muster on eelnevalt liita "kirjutamise" tee tulemusi. See on eriti atraktiivseks, kui "lugeda" taotluste maht on suurem kui "kirjutada" taotlused. Üks kord eelnevalt liidetud, tulemus on Lugemistaotluse ühes kohas koos.  Parim viis liita eelnevalt DocumentDB on päästik, mida on vaja järgida iga "kirjutamise" õigustega häälestamine ja metaandmete dokumenti, mille viimased tulemused päringu, mis on on juhtumiga värskendada. Näiteks palun vaadake [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) proovi, mis värskendab minSize, maxSize ja totalSize metaandmete dokumendi kogumi. Valimi pikendatav värskendamiseks counter, summa jne.

##<a name="references"></a>Viited
1.  [Azure'i DocumentDB tutvustus][introduction]
2.  [DocumentDB SQL-i määratlus](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [DocumentDB .net-i näited](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB järjepidevuse tasemed][consistency-levels]
5.  ANSI SQL-i 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON [http://json.org/](http://json.org/)
7.  JavaScripti spetsifikatsioon [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Päringu hindamise võtteid suurte andmebaaside [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Päringutöötluse paralleelselt Relatsiooniandmebaasist süsteemide, IEEE arvuti ühiskonna vajutage, 1994
11. Lu, oOMa, Tan, Päringutöötluse paralleelselt Relatsiooniandmebaasist Systems IEEE arvuti ühiskonna vajutage, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar Andrew Tomkins: siga Ladina: ei ole nii, et võõras keeles SIGMOD 2008 andmete töötlemiseks.
13.     G. Graefe. Päringu optimeerimine Cascades raames. IEEE andmete Eng. Bull., 18 3: 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
