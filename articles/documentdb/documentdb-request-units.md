<properties 
    pageTitle="Üksuste DocumentDB taotlemine | Microsoft Azure'i" 
    description="Lisateavet mõista, määrake ja prognoosimiseks DocumentDB taotluse ühiku nõuetele." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Üksuste DocumentDB taotlemine
Nüüd saadaval: DocumentDB [taotluse ühiku kalkulaator](https://www.documentdb.com/capacityplanner). Lisateavet [oma läbilaskevõime peab Estimating](documentdb-request-units.md#estimating-throughput-needs).

![Läbilaskevõime kalkulaator][5]

##<a name="introduction"></a>Sissejuhatus
Selles artiklis antakse ülevaade [Microsoft Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/)taotluse üksused. 

Pärast selle artikli lugemist on teil saama vastavad järgmistele küsimustele.  

-   Mis on koosolekukutse üksused ja taotleda kulude?
-   Kuidas määrata, taotluse seadme suurus kogumi jaoks?
-   Kuidas põhjal prognoosida, minu rakenduste taotluse üksus?
-   Mis juhtub, kui ma või ületada taotluse ühiku kogumi?


##<a name="request-units-and-request-charges"></a>Koosolekukutse üksused ja kulud taotlemine
DocumentDB pakub kiire ja hõlpsalt prognoosida jõudlust *broneerimist* ressursse rakenduse läbilaskevõime vajaduste järgi.  Kuna rakendus laadimine ja juurdepääs mustrite muutmine aja jooksul, DocumentDB võimaldab teil hõlpsasti suurendada või vähendada reserveeritud läbilaskevõime saadaval rakenduse.

DocumentDB, kus on määratud reserveeritud läbilaskevõime taotluse ühikute sekundis töötlemine.  Mõelge taotluse üksuste läbilaskevõime valuutana, mille abil saate *reserveerida* summa, tagatud taotluse üksused saadaval rakenduse kohta alusel.  Iga DocumentDB - dokumendi kirjutamine, läbimiseks päringu, värskendamine dokumendi - toiming tarbib CPU, mälu ja IOPS.  Mis on iga toimingu andmesidekasutusele *taotluse eest*, mis on *koosolekukutse*ühikutes.  Mõistmine tegureid, mis mõjutavad taotluse ühiku kulude koos teie taotlus läbilaskevõime nõuete, võimaldab täpselt maksumus rakenduse käivitada. 

##<a name="specifying-request-unit-capacity"></a>Määrab taotluse seadme suurus
DocumentDB saidikogumi loomisel saate määrata taotluse ühikute soovite selle saidikogumi jaoks sekundis (RUs).  Kui selle saidikogumi on loodud, on määratud RUs täielik jaotamine reserveeritud selle saidikogumi jaoks.  Iga saidikogumi on tagatud on spetsiaalne ja eraldatud läbilaskevõime omadused.  

See on oluline Pange tähele, et DocumentDB toimib broneerimiseks mudel; Seega on arve summa läbilaskevõime *reserveeritud* kogumiseks, olenemata sellest, kui palju see läbilaskevõime on aktiivselt *kasutada*.  Pidage meeles, et rakenduse laadi, andmed ja kasutus mustrite muutmine muudate saate hõlpsasti üles ja alla summa reserveeritud RUs DocumentDB SDK-d või [Azure portaali](https://portal.azure.com)kaudu.  Lisateavet kellelegi skaala läbilaskevõime üles ja alla [DocumentDB jõudluse tasemed](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Ühiku kaalutlused taotlemine
Taotluse ühikute reserveerida oma DocumentDB kogumi hindamisel on oluline arvesse võtta järgmisi muutujaid:

- **Dokumendi suurus**. Kui dokumendi suuruses lugeda või kirjutada andmed suureneb ka tarbitud ühikute suurendamine
- **Dokumendi atribuut loendus**. Eeldades vaikimisi indekseerimise kõik atribuudid, kirjutamiseks dokumendi tarbitud ühikute suureneb kui atribuudi arv suureneb.
- **Andmete järjepidevuse**. Andmete järjepidevus tasemed tugev või piiratud Staleness kasutamisel kuvatakse täiendavate üksuste tarbitud dokumente lugeda.
- **Indekseeritud atribuudid**. Registri poliitika kohta iga saidikogumi määratleb, mis on vaikimisi indekseeritud. Saate vähendada teie taotlus ühiku tarbimine indekseeritud atribuutide arvu piiramine või lubamine rongile indekseerimine.
- **Dokumendi indekseerimine**. Vaikimisi iga dokumendi automaatselt registrisse saate kasutada vähem taotluse üksuste kui te ei soovi indekseerida mõned dokumendid.
- **Päringu mustrite**. Päringu keerukusest mõjutab toimingu tarbitud mitu taotlemine ühikut. Arvu predicates, on predicates, prognooside, arv UDF-ID ja kõik andmeallika andmehulga mahu mõjutada kulu päringu toiminguid.
- **Skript kasutamine**.  Sarnaselt päringute, Salvestatud toimingute ja käivitab tarbimine taotluse üksuste tehtud toimingute keerukusest. Kui teil tekib teie rakendus, kontrolli taotluse tasuta päise paremini mõista, kuidas iga toimingu tarbib taotluse seadme suurus.

##<a name="estimating-throughput-needs"></a>Läbilaskevõime vajadustele hindamine
Taotluse üksus on koosolekukutse töötlemine maksumus normaliseeritud mõõt. Ühe päringu ühiku tähistab töötlemine läbilaskevõimest lugeda (kaudu ise link või id) ühe 1KB JSON dokument, mis koosneb 10 atribuudi Kordumatud väärtused (välja arvatud Süsteemiatribuudid). Taotluse loomine (lisa), asendamine või kustutada samas dokumendis tarbib rohkem töötlemine teenusest ning seeläbi rohkem taotluse ühikut.   

> [AZURE.NOTE] 1 taotluse üksuse 1KB dokumendi võrdlusalus vastab lihtsa saamiseks ise link või dokumendi id.

###<a name="use-the-request-unit-calculator"></a>Kasutage taotluse ühiku kalkulaator
Aidata kliendid peene häälestada oma läbilaskevõime hinnangud, aidata hinnata tavalised toimingud, sh nõuded taotluse ühiku on veebipõhine [taotluse ühiku kalkulaator](https://www.documentdb.com/capacityplanner) :

- Dokumendi loob (kirjutab)
- Dokumendi sisu
- Dokumendi kustutamine
- Dokumendi värskendused

Tööriist hõlmab ka tugi hindamine andmete salvestusruumi vajadustele esitate dokumentide valimi põhjal.

Tööriista abil on lihtne:

1. Ühe või mitme esindaja JSON dokumentide üleslaadimine.

    ![Dokumentide üleslaadimine taotluse ühiku kalkulaator][2]

2. Sisestage andmed mäluruumi prognoosimiseks, eeldate talletamiseks dokumentide arv.

3. Sisestage arv dokumendi loomine, lugemine, värskendamine ja kustutamine toimingud, mida vajate (sekundis alusel). Laadige prognoosimiseks taotluse ühiku kulude dokumendi update toimingute, samm 1 ülal, mis sisaldab tüüpilised välja valimi dokumendi koopia.  Näiteks kui dokumendi värskendused muuta tavaliselt kaks atribuudid nimega lastLogin ja userVisits, siis lihtsalt kopeerige valimi dokumendi, värskendage need kaks atribuudid väärtused ja kopeeritud dokumendi üleslaadimine.

    ![Sisestage läbilaskevõime nõuete taotluse ühiku kalkulaator][3]

4. Klõpsake nuppu arvutamine ja tulemusi.

    ![Ühiku kalkulaatori tulemuste taotlemine][4]

>[AZURE.NOTE]Kui teil on dokumenditüüpe, mis erineb oluliselt suurus ja atribuudid indekseeritud arv, siis üles iga *tüüpi* tüüpilised valimil tööriist ja seejärel soovitud tulemi arvutamine.

###<a name="use-the-documentdb-request-charge-response-header"></a>Kasutage DocumentDB taotluse tasuta vastuse päis
Iga teenuse DocumentDB vastuse sisaldab kohandatud päise (x-ms-kutse – tasuta), mis sisaldab taotluse tarbitud taotluse. See päis pääseb ka DocumentDB SDK-d. .NET SDK, on RequestCharge atribuudi ResourceResponse objekti.  Päringute DocumentDB päringu Exploreri Azure'i portaalis pakub taotlus teavet sooritatud päringuid.

![RU kulude Exploreris päringu uurimine][1]

Seda meelt, üks meetod hindamine salvestada taotluse ühiku tasuta seotud tavalisi toiminguid teie rakendus kasutab esindaja dokumendi suhtes on nõutud rakenduse reserveeritud läbilaskevõime ja seejärel hindamine arv, eeldate läbimiseks iga teise.  Kindlasti mõõta ja tüüpilised päringud ja DocumentDB skript kasutamine ka.

>[AZURE.NOTE]Kui teil on dokumenditüüpe, mis erineb oluliselt suurus ja atribuudid indekseeritud arv, siis kirje rakendatav toiming taotluse ühiku tasu iga *tüüpi* tüüpilised seotud.

Näiteks:

1. Kirje taotluse ühiku tasuta loomise (lisamise) tüüpilised dokumendi. 
2. Kirje taotluse ühiku tasuta tüüpilised dokumendi lugemine.
3. Kirje värskendamine tüüpilised dokumendi taotluse ühiku eest.
3. Kirje taotluse ühiku tasuta tüüpilised, levinud dokumendi päringuid.
4. Kirje taotluse ühiku eest kohandatud skriptid (salvestatud toimingute, päästikute, kasutaja määratletud funktsioonid) kasutada rakenduse järgi
5. Arvutab antud eeldate iga teise käivitamiseks hinnanguline arv vajalik ühikute.

##<a name="a-request-unit-estimation-example"></a>Taotluse üksuse hindamine näide
Võtke arvesse järgmist ~ 1KB dokumendi.

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Dokumendid on minified DocumentDB, et süsteemi arvutatud ülaltoodud dokumentide maht on veidi väiksem kui 1KB.


Järgmises tabelis on ligikaudse taotluse tavalisi toiminguid (ligikaudse taotluse ühiku tasuta eeldab, et konto järjepidevuse tase on määratud "Seanss" ja, et kõik dokumendid on automaatselt indekseeritud) selle dokumendiga tasu:

Toiming|Ühiku tasuta taotlemine 
---|---
Dokumendi loomine|~ 15 RU 
Dokumendi lugemine|~ 1 RU
Päringu dokumendi ID|~2.5 RU

Lisaks selles tabelis on kuvatud ligikaudse taotluse tüüpilised rakenduse kasutatud päringuid tasu:

Päringu|Ühiku tasuta taotlemine|# Tagastatud dokumendid
---|---|--- 
Valige toiduga ID|~2.5 RU|1 
Valige toiduainete tootja|~ 7 RU|7
Valige toidu rühma ja tellimuse kaalust|~ 70 RU|100
Valige ülemine 10 toiduainete toidu rühma|~ 10 RU|10

>[AZURE.NOTE]RU kulude sõltuvad tagastatud dokumentide arv.

Selle teabe me põhjal prognoosida selle rakenduse antud toimingute ja me oodata sekundis päringute arv RU nõuded:

Toiming päring|Hinnanguline arv sekundis|Nõutav RUs 
---|---|--- 
Dokumendi loomine|10|150 
Dokumendi lugemine|100|100 
Valige toiduainete tootja|25|175 
Valige toidu rühma alusel|10|700 
Valige ülemine 10|15|150 kokku|155|1275

Sel juhul loodame 1 275 RU/s on Keskmine läbilaskevõime nõue.  Ümardamine ülespoole lähima 100, me oleks säte 1 300 RU s selle rakenduse saidikogumi jaoks.

##<a id="RequestRateTooLarge"></a>Reserveeritud läbilaskevõime ületamise
Võta tagasi, et taotluse ühiku tarbimine hinnatakse on sekundis. Rakendusi, mis ületavad ettevalmistatud taotlus ühiku määr kogumi, kuvatakse selle saidikogumi taotluste rakendus kuni määr langeb alla reserveeritud tase. Mõne ahendamise ilmnemisel server sisaldab ennatlikult lõpetamiseks taotluse koos RequestRateTooLargeException (HTTP olekukoodi 429) ja x-ms-proovi uuesti-pärast – ms päis, mis näitab, aeg millisekundites, mida kasutaja peab oodake enne reattempting kutse tagasi.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Kui kasutate .net-i kliendi SDK ja LINQ päringute, siis te kunagi ei pea seda erandit tegelema, nagu praegune versioon .net-i kliendi SDK peidetult püüab selle vastus enamasti, peab kinni serveri määratud proovi uuesti pärast päise ja uued katsed taotluse. Kui teie konto on sellele samaaegselt juurdepääsu mitme kliendi, õnnestub järgmise proovi uuesti.

Kui teil on rohkem kui ühe kliendi kumulatiivselt üle taotluse määr, proovi uuesti vaikekäitumine võib-olla ei piisa ja kliendi kuvatakse põhjustada on DocumentClientException oleku koodiga 429 rakendusse. Juhtu, võite kaaluda uuesti käitumine ja loogika rakenduse tõrge töötlemise moodulid või suurendada reserveeritud läbilaskevõime kogumi.

##<a name="next-steps"></a>Järgmised sammud

Lisateavet reserveeritud läbilaskevõime Azure'i DocumentDB andmebaasidega, saate nende ressursside:
 
- [DocumentDB hinnad](https://azure.microsoft.com/pricing/details/documentdb/)
- [DocumentDB võimsus haldamine](documentdb-manage.md) 
- [Andmete modelleerimine DocumentDB](documentdb-modeling-data.md)
- [DocumentDB jõudluse tasemed](documentdb-partition-data.md)

Lisateavet DocumentDB leiate Azure'i DocumentDB [dokumentatsiooni](https://azure.microsoft.com/documentation/services/documentdb/). 

Alustada skaala ja jõudluse testimine DocumentDB teemast [jõudlus ja katsetada Azure'i DocumentDB abil](documentdb-performance-testing.md).


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
