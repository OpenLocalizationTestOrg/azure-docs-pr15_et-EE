<properties
    pageTitle="Microsoft Azure Storage kokkulangevus haldamine"
    description="Kuidas hallata kokkulangevus bloobimälu, järjekorda, tabeli või faili teenuste jaoks"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Microsoft Azure Storage kokkulangevus haldamine

## <a name="overview"></a>Ülevaade

Tänapäevane rakendustest on tavaliselt kuvamine ja värskendamine andmete korraga mitu kasutajat. Selleks, et rakendus arendajad põhjalikult läbi mõelda, kuidas anda oma lõppkasutajatele prognoositavad kogemus eriti stsenaariumid, kus mitu kasutajat saab värskendada samad andmed. On kolme peamine andmete kokkulangevus strateegiad arvestab tavaliselt arendajatel.  


1.  Loodetav kokkulangevus – viimast värskendust selle värskenduse osana kindlaks, kui andmed on muutunud rakenduse läbimiseks on rakenduse lugemist andmeid. Näiteks kui kaks kasutajat vaatamine vikilehe update teha sama lehe seejärel viki platvormi tagama teise värskenduse üle kirjutada esimese update- ja et nii kasutajate aru saada, kas nende värskendamine õnnestus või mitte. See strateegia on kõige sagedamini kasutatav veebirakendusi.
2.  Kardetav kokkulangevus – rakenduse vaatab värskendamiseks võtab lukustus objekti, mis takistab teiste kasutajate andmete värskendamine, kuni soovitud vabastatakse. Näiteks kui ainult juhtslaidi täidab värskenduste juhtslaidi/slave andmete dispersioonanalüüs stsenaarium juhteksemplari tavaliselt hoiab eksklusiivset lukustamist pikema perioodi jooksul tagamaks, et keegi teine seda saate värskendada andmeid.
3.  Viimase kirjutaja võitu – lähenemisviisi, mis võimaldab mis tahes värskenduse toimingute alustada ilma kontrollida, kui mõni muu rakendus on värskendatud andmed rakenduse esmalt andmeid lugeda. See strateegia (või puudub ametlik strateegia) on tavaliselt kasutatakse, kui andmed on liigendatud nii, et puudub tõenäosus, et mitme kasutaja on juurdepääs samad andmed. Samuti võib kasu kui lühiajaline andmevoogu töödeldakse.  

Selles artiklis antakse ülevaade sellest, kuidas Azure Storage platform lihtsustab arengu esimese klassi toetavad kõiki kolme need kokkulangevus strateegiad.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure'i salvestusruumi – lihtsustab Cloud arengu
Azure'i salvestusteenus toetab kõiki kolme strateegiad, kuigi see on selles, et täieliku toe pakkumine loodetav ja kardetav kokkulangevus, kuna see on loodud omaks tugev järjepidevuse mudelit, mis tagab salvestusruumi teenuse paneb andmete lisamine või värskendamist kõik täpsemaks pääseb soovite, et andmed kuvatakse värskenduses eristatav. Salvestusruumi platvormide jaoks loodud lõpliku järjepidevuse mudelit, mis kasutavad on viivitusega vahel, kui kirjutada toimub üks kasutaja ja teiste kasutajate, seega keerulisemaks vältimiseks vastuolusid mõjutamata lõppkasutajad kliendi rakenduste arendamise näha värskendatud andmete.  

Lisaks valimise korral kokkulangevus strateegia arendajate tuleks teada, kuidas salvestusruumi platvormi isoleerib muutusi – eriti samale objektile tehingud üle. Azure'i salvestusteenus kasutab hetktõmmise eraldamise toimingud juhtub samaaegselt ühes sektsioonis kirjutada toiminguid. Erinevalt muude eraldamise taset, hetktõmmise eraldamise tagab kõigi loeb vt ühtsete andmete hetktõmmist, isegi siis, kui värskendusi – sisuliselt viimase kinnitatud väärtused ajal värskendust tagastades tehingu töötlemise.  

## <a name="managing-concurrency-in-blob-storage"></a>Haldamise kokkulangevus bloobimälu
Saate valida, kas loodetav või kardetav kokkulangevus mudelite abil hallata juurdepääsu plekid ja ümbriste bloobimälu teenus. Kui te ei määra otseselt strateegia viimase kirjutab võitu on vaikimisi.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Loodetav kokkulangevus plekid ja ümbriste  

Salvestusruumi teenuse määrab identifikaatori iga objekti, mis on talletatud. See identifikaator värskendatakse iga kord, kui mõni värskenduse toimingu objekti. Identifikaator tagastatakse kliendi HTTP GET vastuse abil ETag (üksus sildi) päis, mis on määratletud HTTP-protokolli osana. Kasutaja läbimiseks värskendust sellise objekti saate saata algse ETag koos tingimusvormingu päise tagamaks, et värskendust kuvatakse üksnes juhul, kui teatud tingimused on täidetud – sel juhul tingimus on "If-Match" päis, mis nõuab salvestusteenus ETag, värskendus määratud väärtus on sama, mis on talletatud salvestusruumi teenuse tagamiseks.  

Liigenduse see protsess on järgmine:  

1.  Salvestusruumi teenus on bloobimälu tuua, vastus sisaldab HTTP ETag päise väärtus, mis tuvastab salvestusruumi teenuse objekti praegune versioon.
2.  Funktsiooni bloobimälu värskendamisel kaasata ETag väärtus olete saanud kutse saatmist teenuse **If-Match** tingimusvormingu päises juhises 1.
3.  Teenuse võrdleb taotluse ETag väärtus on bloobimälu praeguse ETag väärtusega.
4.  Kui soovitud bloobimälu ETag praegune väärtus on muu versioon, kui ETag **If-Match** tingimusvormingu päises kutse, tagastab teenuse kliendi 412 tõrge. See näitab kliendile muu protsess värskendamiseni on bloobimälu, kuna kliendi tuua seda.
5.  Kui soovitud bloobimälu ETag praegune väärtus on sama versioon nagu ETag **If-Match** tingimusvormingu päises kutse, teenuse teeb Taotletud toiming ja värskendused on loodud uue versiooni kuvamiseks bloobimälu ETag praegune väärtus.  

Järgmised C# koodilõigu (abil kliendi salvestusruumi teek 4.2.0) kuvatakse lihtne näide sellest, kuidas koostada on **If-Match AccessCondition** põhjal ETag-väärtus, millele pääseb bloobimälu, mis on varem tuua või lisatud atribuudid. Seejärel kasutab **AccessCondition** objekti kui see on bloobimälu värskendamine: **AccessCondition** objekt lisatakse taotluse päis **If-Match** . Kui muu protsess on värskendatud selle bloobimälu, bloobimälu teenus tagastab HTTP 412 (eelduseks nurjus) oleku meilisõnumi. Saate alla laadida täielik valimi siit: [Haldamine kokkulangevus Azure Storage abil](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

Salvestusruumi teenus sisaldab tugi täiendavad tingimusvormingu päised näiteks **If-muudetud – alates**, **If-mille pole muudetud – alates** **If-pole-Match** ning nende kombinatsioonid. Lisateavet MSDN-is leiate [Täpsustades tingimusvormingu päised bloobimälu toimingud](http://msdn.microsoft.com/library/azure/dd179371.aspx) .  

Järgmises tabelis on kokkuvõte container toiminguid, mis Aktsepteeri tingimusvormingu päiste nt **If-Match** taotluse ja mis tagastab vastuse ETag-väärtus.  

| Toiming                | Annab vastuseks Container ETag väärtuse. | Aktsepteerib tingimusvormingu päised |
|:-------------------------|:-----------------------------|:----------------------------|
| Container loomine         | Jah                          | Ei                          |
| Container atribuudid hankimine | Jah                          | Ei                          |
| Container metaandmete hankimine   | Jah                          | Ei                          |
| Container Sea metaandmete   | Jah                          | Jah                         |
| Container ACL hankimine        | Jah                          | Ei                          |
| Container ACL seadmine        | Jah                          | Jah (*)                     |
| Container kustutamine         | Ei                           | Jah                         |
| Üürilepingu Container          | Jah                          | Jah                         |
| Loendi plekid               | Ei                           | Ei                          |

(*) Vahemällu salvestamise SetContainerACL määratletud õigused ja nende õiguste kuluda 30 sekundit levitamine, mille jooksul on tagatud värskenduste ühtlustamist.  

Järgmises tabelis on kokkuvõte bloobimälu toiminguid, mis Aktsepteeri tingimusvormingu päiste nt **If-Match** taotluse ja mis tagastab vastuse ETag-väärtus.

| Toiming           | Annab vastuseks ETag väärtuse. | Aktsepteerib tingimusvormingu päised           |
|:--------------------|:-------------------|:--------------------------------------|
| Sellele bloobimälu            | Jah                | Jah                                   |
| Saada bloobimälu            | Jah                | Jah                                   |
| Saada bloobimälu atribuudid | Jah                | Jah                                   |
| Atribuutide seadmine bloobimälu | Jah                | Jah                                   |
| Saada bloobimälu metaandmed   | Jah                | Jah                                   |
| Metaandmete määramine bloobimälu   | Jah                | Jah                                   |
| Üürilepingu bloobimälu (*)      | Jah                | Jah                                   |
| Hetktõmmis bloobimälu       | Jah                | Jah                                   |
| Kopeerige bloobimälu           | Jah                | Jah (lähte-kui ka bloobimälu) |
| Kas soovite katkestada Kopeeri bloobimälu     | Ei                 | Ei                                    |
| Kustutage bloobimälu         | Ei                 | Jah                                   |
| Sellele blokeerimine           | Ei                 | Ei                                    |
| Sellele plokkloend      | Jah                | Jah                                   |
| Saada plokkloend      | Jah                | Ei                                    |
| Sellele lehele            | Jah                | Jah                                   |
| Hangi vahemikud     | Jah                | Jah                                   |


(*) Üürilepingu bloobimälu ei muuda ETag on bloobimälu kohta.  

### <a name="pessimistic-concurrency-for-blobs"></a>Kardetav kokkulangevus plekid
Lukustamiseks bloobimälu, välja arvatud kasutamiseks, saate hankida [üürilepingu](http://msdn.microsoft.com/library/azure/ee691972.aspx) seda. Viimistlemine rendilepingu, teil määrata, kui kaua peate rendilepingu: see võib olla 15-60 sekundi vahel või piiramatu, mis ulatub eksklusiivset lukustamist. Saate pikendada piiratud rendilepingu pikendada ja kõigi rendi saate vabastada, kui olete lõpetanud nendega. Teenuse bloobimälu välja piiratud liisingute automaatselt, kui need aeguvad.  

Liisingute lubamine erinevate sünkroonimine strateegiad toetatud, sh eksklusiivse kirjutamine ja ühiskasutuses loetuks, välja arvatud kirjutamine / v.a. lugemine ja kirjutamine ühiskasutuses / v.a. read. Kui on olemas üürilepingu salvestusruumi teenuse jõustab v.a. kirjutab (panna, määramine ja kustutada toimingud) aga ainuõigused loetuks toimingute tagamiseks nõuab tagamaks, et kõik klientrakendustes kasutada üürilepingu ID "ja" ainult üks kliendi korraga arendaja on kehtiv üürileping ID-ga. Read toimingud, mis ei sisalda üürilepingu ID tulemi ühiskasutusse antud sisu.  

Järgmised C# koodilõigu kujutab näidet hankimisega mõne välja arvatud üürilepingu 30 sekundit on bloobimälu, värskendamine on bloobimälu sisu ja seejärel vabastatakse üürileping. Kui on juba kehtiv üürileping on bloobimälu kui proovite omandada uue rendilepingu, bloobimälu teenus tagastab on "HTTP (409) konflikt" olek tulemi. Allpool koodilõigu kasutab **AccessCondition** objekti kapseldada üürilepingu teabe kui seda taotleb värskendamiseks bloobimälu salvestusruumi teenus.  Saate alla laadida täielik valimi siit: [Haldamine kokkulangevus Azure Storage abil](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Kui proovite kirjutamine toiming klõpsake üüritud bloobimälu ilma üürilepingu ID, nurjub taotluse 412 tõrge. Pange tähele, et kui üürileping lõpeb enne **UploadText** meetodi, kuid saate siiski anda üürilepingu ID, taotluse ka nurjub tõrkega **412** . Üürilepingu aegumise korda ja üürilepingu ID haldamise kohta lisateabe saamiseks leiate [Üürilepingu bloobimälu](http://msdn.microsoft.com/library/azure/ee691972.aspx) ülejäänud dokumentatsioonist.  

Bloobimälu järgmiste toimingute abil saate liisingute kardetav kokkulangevus haldamine:  


-   Sellele bloobimälu
-   Saada bloobimälu
-   Saada bloobimälu atribuudid
-   Atribuutide seadmine bloobimälu
-   Saada bloobimälu metaandmed
-   Metaandmete määramine bloobimälu
-   Kustutage bloobimälu
-   Sellele blokeerimine
-   Sellele plokkloend
-   Saada plokkloend
-   Sellele lehele
-   Hangi vahemikud
-   Hetktõmmis bloobimälu - üürilepingu ID valikuline rendilepingu korral
-   Kopeerige bloobimälu - ID on nõutav, kui sihtkoha bloobimälu on olemas üürilepingu üürilepingu
-   Kopeeri bloobimälu - ID on nõutav, kui sihtkoht bloobimälu on olemas lõputu üürilepingu üürilepingu katkestada.
-   Üürilepingu bloobimälu  

### <a name="pessimistic-concurrency-for-containers"></a>Kardetav kokkulangevus ümbriste jaoks
Üürimisega ümbriste lubamise kohta plekid toetatavad sama sünkroonimise strateegiad (exclusive kirjutada / lugeda, välja arvatud kirjutamine ühiskasutuses / v.a. lugemine ja kirjutamine ühiskasutuses / v.a. loe) aga erinevalt plekid salvestusruumi teenuse ainult jõustab ainuõigused kustutada toimingute kohta. Mille maht on aktiivne üürilepingu kustutamiseks mõnda muud klienti peab sisaldama aktiivne üürilepingu ID taotlusega Kustuta. Kõik muud container toimingud õnnestub üüritud container kohta ilma üürilepingu ID millisel juhul ühiskasutuses toimingud. Kui ainuõigused update (pane või Sea) või toimingud on vajalik siis arendajad veenduge kõik kliendid kasutada üürilepingu ID ning et ainult üks klient korraga on kehtiv üürileping ID-ga.  

Container järgmiste toimingute abil saate liisingute kardetav kokkulangevus haldamine:  

-   Container kustutamine
-   Container atribuudid hankimine
-   Container metaandmete hankimine
-   Container Sea metaandmete
-   Container ACL hankimine
-   Container ACL seadmine
-   Üürilepingu Container  

Lisateavet leiate järgmistest teemadest.  

- [Tingimusvormingu päised bloobimälu toimingud määramine](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Üürilepingu Container](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Üürilepingu bloobimälu](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Tabeli teenus kokkulangevus haldamine
Tabeli teenus kasutab loodetav kokkulangevus kontrollib on vaikekäitumine, kui töötate üksustega erinevalt bloobimälu service, kus peate installisuvandite loodetav kokkulangevus kontrolle teha. Muude teenuste tabelist ja bloobimälu vahe on võimalus ainult hallata kokkulangevus toimimist üksused tuleks bloobimälu teenuse abil saate hallata nii ümbriste ja plekid kokkulangevus.  

Loodetav kokkulangevus kasutama ja kontrollida, kui muu protsess muudetud üksuse, kuna teil laadida salvestusteenus tabeli, saate kasutada ETag väärtus kuvatakse tabeli teenus tagastab üksus. Liigenduse see protsess on järgmine:  

1.  Üksuse toomine tabeli salvestusteenus, vastus sisaldab ETag-väärtus, mis tuvastab salvestusruumi teenus selle üksusega seotud emaidentifikaatoreid.
2.  Üksuse värskendamisel kaasata ETag väärtus olete saanud kutse saatmist teenuse kohustuslik **If-Match** päises juhises 1.
3.  Teenuse võrdleb ETag väärtus taotluse praeguse üksuse ETag väärtusega.
4.  Kui üksuse praegust väärtust ETag ETag kohustuslik **If-Match** päises kutse, tagastab teenuse kliendi 412 tõrge. See näitab kliendile muu protsess värskendamiseni üksus, kuna kliendi tuua seda.
5.  Kui praeguse üksuse ETag väärtus on sama ETag kohustuslik **If-Match** taotluse päises või **If-Match** päis sisaldab metamärki (*), teenuse teeb Taotletud toiming ja värskendab ETag praegune väärtus üksuse kuvamiseks, et see on värskendatud.  

Pange tähele, et erinevalt bloobimälu service, tabeli teenuse kliendi värskendus taotlused on **If-Match** päise kaasata. Siiski, on võimalik jõustada tingimusteta värskendamine (viimase kirjutaja võitu strateegia) ja mööda kokkulangevus kontrolli kui kliendi määrab **If-Match** päise metamärki (*) taotlusega.  

Järgmised C# koodilõigu kuvatakse kliendi ettevõte, mis on kas varem loodud või alla laadida, võttes oma meiliaadressi värskendada. Algse lisamine või ETag väärtuse kliendi objekti toiming salvestab toomiseks ja kuna valimi kasutab sama objekti eksemplari, kui see käivitab asenduse, selle automaatselt saatmine ETag väärtus tabeli teenus, teenuse kontrollimiseks kokkulangevus rikkumised lubamine. Kui muu protsess on värskendatud tabelimälu selle üksuse, tagastab teenuse olek meilisõnumi HTTP 412 (tingimuseks nurjus).  Saate alla laadida täielik valimi siit: [Haldamine kokkulangevus Azure Storage abil](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Konkreetselt keelata kokkulangevus sisse, määrake atribuudi **ETag** **töötaja** objekti "*" enne täidate asenduse.  

    customer.ETag = "*";  

Järgmises tabelis on toodud tabeli üksuse toimingute kasutamise ETag väärtused:

| Toiming                | Annab vastuseks ETag väärtuse. | If-Match taotluse päise nõuab |
|:-------------------------|:-------------------|:---------------------------------|
| Päringu üksused           | Jah                | Ei                               |
| Üksuse lisamine            | Jah                | Ei                               |
| Üksuse värskendamine            | Jah                | Jah                              |
| Üksuse ühendamine             | Jah                | Jah                              |
| Üksuse kustutamine            | Ei                 | Jah                              |
| Lisamine või asendamine üksus | Jah                | Ei                               |
| Ühenda üksus või lisamine   | Jah                | Ei                               |

Pange tähele, et **lisamine või asendamine juriidilise** ja **Lisa või ühendamine juriidilise** toiminguid teha *pole* kokkulangevus kontrolle teostama, kuna need saata ETag-väärtus tabeli teenusega.  

Üldiselt arendajate tabelite kasutamine peaks toetuvad loodetav kokkulangevus scalable rakenduste. Vajadusel kardetav lukustamine on üks võimalus arendajate võib kuluda kui juurdepääs tabelid on määratud bloobimälu iga tabeli jaoks määrata ja proovige võtta enne tabeli opsüsteem on bloobimälu rendilepingu. Seda moodust ei nõua rakenduse tagavad kõik andmed Accessi teed üürileping enne alustamist tabeli. Peaks samuti võtke arvesse, mis kohustuse aeg on 15 minutit, mis nõuab tähelepanu jaoks skaleeritavus.  

Lisateavet leiate järgmistest teemadest.  

- [Toimingute üksused](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Kokkulangevus järjekorda teenus haldamine
Üks stsenaarium, mis kokkulangevus on probleemiks queueing teenus on, kus mitme kliendi on sõnumite allalaadimise järjekorda. Kui sõnumi tuuakse järjekorda, vastus sisaldab sõnumi ja pop vastuvõtuteatist väärtus, mida on vaja sõnumi kustutamine. Sõnumit ei kustutata automaatselt järjekorda, kuid pärast seda, kui see on laaditud, ei ole nähtav teiste klientide visibilitytimeout parameetriga määratud ajavahemik. Kliendi, mis toob sõnumi eeldatakse, et pärast töötluse ja enne selle TimeNextVisible määratud aja element, mis on arvutatud tagasiside põhjal visibilitytimeout parameeter sõnumi kustutamine. Aeg, kus välja sõnumi TimeNextVisible väärtuse määramiseks on lisatud visibilitytimeout väärtus.  

Teenuse järjekorda ei saa loodetav või kardetav kokkulangevus tugi ja selle põhjus kliendid vormilt järjekorda sõnumite töötlemist peaks tagada sõnumite töötlemist idempotent viisil. Viimase kirjutaja võitu strateegia kasutatakse update toiminguid nagu SetQueueServiceProperties, SetQueueMetaData, SetQueueACL ja UpdateMessage.  

Lisateavet leiate järgmistest teemadest.  

- [Järjekorda teenuse REST API-ga](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Sõnumite toomine](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Faili teenus kokkulangevus haldamine
Faili teenuse pääseb kaks erinevat protokolli lõpp – SMB ja ülejäänud abil. ÜLEJÄÄNUD teenus ei saa tugi loodetav lukustamine või kardetav lukustamine ja kõik värskendused jälgib viimase kirjutaja võitu strateegia. SMB Kliendid, mida mount failikettad saate kasutada faili süsteemi locking menetlustele ühisfaile – sealhulgas võimalus teha kardetav lukustamine juurdepääsu haldamiseks. Faili avamisel kuvatakse SMB klient seda määrab failidele juurde nii ühiskasutus režiim. Faili Access suvandi "Kirjutada" või "Lugemis-ja kirjutamisõigusega" koos failikettal režiimi "Pole" annab tulemuseks faili lukustatavad on SMB klient, kuni fail on suletud. Kui ülejäänud toiming proovitakse kohale toimetada faili kui ka SMB klient on lukus faili ülejäänud teenuse tagastab olekukoodi 409 (konflikt) tõrkekoodiga SharingViolation.  

Kustuta faili avamisel kuvatakse SMB klient märgib faili ootel Kustuta kõik SMB klient, kuni selle faili avamine pidemete on suletud. Kui fail on märgitud, nagu on ootel Kustuta, tehte ülejäänud faili tagastab olekukoodi 409 (konflikt) tõrkekoodiga SMBDeletePending. Kuna on võimalik, et eemaldada kustutamise ootel lipu enne faili sulgemist SMB klient ei tagastata olekukoodi 404 (ei leitud). Teisisõnu, olekukoodi 404 (ei leitud) on ainult oodata, kui fail on eemaldatud. Pange tähele, et fail on SMB Kustuta ootele, seda ei kaasata tulemuste loendis failid. Samuti võtke arvesse, et ülejäänud kustutamine faili- ja ülejäänud kustutamiseks Directory toimingute on kinnitatud atomically ja tulemuseks Kustuta ootele.  

Lisateavet leiate järgmistest teemadest.  

- [Lukud faili haldamine](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Kokkuvõte ja järgmised toimingud
Microsoft Azure'i salvestusteenus on loodud vajadustele kõige keerukate online rakendused liigset arendajatel kahjustada või mõtlema olulisemad oletused näiteks kokkulangevus ja andmete järjepidevuse, et nad on tulnud võtta antud.  

Täieliku valimi rakenduse viidatud selles blogis:  

- [Haldamise kokkulangevus Azure Storage - valimi rakenduse kasutamine](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Lisateavet leiate Azure'i salvestusruumi:  

- [Microsoft Azure'i salvestusruumi koduleht](https://azure.microsoft.com/services/storage/)
- [Azure Storage Sissejuhatus](storage-introduction.md)
- Salvestusruumi [bloobimälu](storage-dotnet-how-to-use-blobs.md), [Tabel](storage-dotnet-how-to-use-tables.md), [järjekorrad](storage-dotnet-how-to-use-queues.md)ja [failide](storage-dotnet-how-to-use-files.md) kasutamise alustamine
- Salvestusruumi arhitektuur – [Azure Storage: tugevalt saadaval pilvepõhine salvestusteenus tugev järjepidevus](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
