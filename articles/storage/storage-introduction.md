<properties
    pageTitle="Sissejuhatus salvestusruumi | Microsoft Azure'i"
    description="Ülevaade Azure Storage Microsoft Online'i andmete talletamine pilveteenuses. Saate teada, kuidas kasutada saadaval pilveteenuse salvestusruumi parim lahendus rakenduste."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Sissejuhatus Microsoft Azure'i salvestusruum

## <a name="overview"></a>Ülevaade

Azure'i salvestusruumi on pilveteenuse salvestusruumi lahendus tänapäevased rakendused kestvus, kättesaadavus ja nende klientide vajaduste rahuldamiseks skaleeritavus. Selles artiklis arendajate IT-spetsialistid ja business otsust lugedes tegijad saate tutvuda järgmiste teemadega.

- Mis on Azure Storage ja kuidas saate seda kasutama oma pilves mobiilsideseadmete, server ja töölauarakenduste
- Millist liiki andmed salvestada teenustega Azure Storage: Bloobivahemälu (objekt) andmed, NoSQL tabeli andmed, järjekorda sõnumeid ja failiketastele.
- Kuidas hallatakse Azure Storage teie andmetele juurdepääsu
- Kuidas Azure Storage andmete tehakse püsival kaudu koondamise ja kopeerimine
- Kust edasi koostada esimese Azure Storage rakenduse

Tööks Azure Storage kiiresti, leiate [Azure'i salvestusruumi viie minuti alustamine](storage-getting-started-guide.md).

Tööriistad, teegid ja muud ressursid Azure Storage töötamise kohta täpsema teabe saamiseks vt [Järgmised toimingud](#next-steps) .

## <a name="what-is-azure-storage"></a>Mis on Azure Storage?

Pilvepõhine arvutuste lubab uue stsenaariumid rakendustele, mis nõuavad scalable, püsival ja väga kättesaadav salvestusruumi oma andmete – mis on täpselt, miks Microsoft Azure Storage välja töötatud. Lisaks, mis võimaldab arendajatel luua suuremahuliste rakendusi toetamiseks uue stsenaariumid, Azure Storage ka annab salvestusruumi aluse jaoks Azure'i Virtuaalmasinates, oma töökindluse edasise märk.

Azure'i salvestusruumi on oluliselt scalable, nii et saate talletada ja teadus-, analüüsi ja meedia rakendused nõutud TB andmete toetamiseks suur andmete stsenaariumid protsessi sadu. Või saate talletada vähehaaval andmekogumite väikeettevõtete veebisaidi. Kuhu kuuluvad teie vajadustele, maksate ainult on talletatud andmed. Azure'i salvestusruumi praegu salvestab kümneid triljoneid kordumatu kliendi objektid ja miljonid taotlusi sekundis keskmiselt tegeleb.

Azure'i salvestusruumi on elastne, nii et saate globaalne paljude osalejatega rakenduste kujundamine ja mastaapimiseks taotluste vastavalt vajadusele – nii andmehulga talletatud ja talle taotluste arv. Maksate ainult jaoks, mida te kasutate, ja ainult siis, kui te seda kasutada.

Azure'i salvestusruumi kasutab automaatne eraldamine süsteemi, mis automaatselt laadi-saldo oma andmete põhjal liikluse. See tähendab, et kuna nõuab sisse oma rakenduse kasvamine, eraldab Azure'i salvestusruumi automaatselt asjakohased ressursid neid täita.

Azure'i salvestusruumi pääseb juurde kogu maailmas, mis tahes tüüpi rakendus, mis tahes kohas töötab pilves, töölaual või mobiil kohapealse serveris või tahvelarvuti seadme. Saate kasutada Azure Storage mobiilsideseadmete stsenaariumid, kus rakenduse talletab andmete alamhulga seadmes ja sünkroonib kogum pilves talletatud andmed.

Azure'i salvestusruumi toetab kliendid mugav arengu erinevaid komplekti (sh Windows ja Linux) operatsioonisüsteemid ja mitmesuguseid programmeerimise keeled (sh .NET, Java, Node.js, Python, Ruby, PHP ja C++ ja mobiilsideseadmete programmeerimine keeles) abil. Azure'i salvestusruumi seab ka andmete ressursside kaudu lihtne REST API-d, mis on saadaval iga kliendi võimeline HTTP-või HTTPS kaudu andmete saatmine ja vastuvõtmine.

Azure Premium mälu pakub suure jõudlusega, latentsusajaga ketta I/O intensiivse töökoormus töötab Azure'i Virtuaalmasinates tugi. Azure Premium Storage, saate lisada mitu püsivate andmete ketast virtuaalse masina ja konfigureerida need teie jõudluse nõuetele. Iga andmete ketta toetavad SSD ketta maksimaalse I/O jõudluse Azure Premium mälu. Vt [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](storage-premium-storage.md) rohkem üksikasju.

## <a name="introducing-the-azure-storage-services"></a>Azure'i salvestusruumi teenuste tutvustus

Azure'i salvestusruumi pakub järgmist nelja teenuse: Bloobivahemälu salvestusruumi, tabelimälu, järjekorda salvestusruumi ja failisalvestusruumi.

- Bloobimälu salvestab objekti struktureerimata andmed. Mõne bloobimälu võib olla mis tahes tüüpi teksti või kahendandmeid, näiteks dokumendi, media fail või rakenduse installer. Bloobimälu ka edaspidi objekti salvestusruumi.
- Tabelimälu talletab liigendatud andmekomplektide. Tabelimälu on NoSQL klahv-atribuut andmete talletamine, mis võimaldab kiire areng ja kiire juurdepääsu suurt hulka andmeid.
- Järjekorda salvestusruumi pakub usaldusväärne sõnumside töövoo töötlemiseks ja komponentide pilveteenuste vahel.
- Failide talletamine pakub jagatud salvestusruumi pärand rakenduste standard SMB protokolli abil. Azure'i virtuaalmasinates ja pilveteenustega saab jagada faili andmete kogu komponendid kaudu ühendatud osakud ja kohapealse rakendused pääsevad faili andmete ühiskasutamine faili teenuse REST API kaudu.

Azure'i salvestusruumi konto on turvaline konto, mis annab teile juurdepääsu teenustele Azure Storage. Konto salvestusruumi kokku kordumatute nimeruumi teie salvestusruumi ressursid. Alloleval pildil on näha Azure storage ressursside salvestusruumi konto vahelisi seoseid:

![Azure'i salvestusruumi ressursid](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Bloobimälu

Kasutajatele suurte andmehulkade struktureerimata objekti talletamiseks pilves, pakub bloobimälu tulusate ja scalable lahendust. Bloobimälu abil saate talletada sisu, näiteks:

- Dokumentide
- Social andmeid, nt fotosid, videoid, muusikat ja ajaveebide
- Varukoopiate faile, arvutite, andmebaasid ja seadmed
- Pildid ja tekst veebirakenduste
- Otsingukonfiguratsiooni andmete pilve rakendused
- Suur andmed, nt logid ja muude suurte andmekogumite

Iga bloobimälu on korraldatud ümbris. Ümbriste pakuvad ka kasulikul viisil määrata turbepoliitikate objekti rühmi. Salvestusruumi konto võib olla mis tahes arv ümbriste ja ümbris võib olla mõni muu arv plekid salvestusruumi konto 500 TB tööjõudlusega ulatuses.  

Bloobimälu pakub kolme tüüpi plekid, Blokeeri plekid, lisa plekid ja lehe plekid (ketast).

- Blokeeri plekid on optimeeritud streaming ja talletamine pilveteenuses objektid ja on hea valik talletamise dokumente, meediumifailide, varukoopiate jne.
- Lisa plekid sarnanevad Blokeeri plekid, kuid on optimeeritud lisa toimingud. Mõne lisa bloobimälu saab värskendada vaid lisada uus plokk lõppu. Lisa plekid on hea valik stsenaariumid nagu logimine, kus uute andmete peab olema kirjutatud ainult selle bloobimälu lõpuni.
- Lehe plekid on optimeeritud tähistav IaaS ketast ja toetavad juhuslik kirjutab ja võib olla kuni 1 TB suurusega. Muu Azure virtuaalse masina kaudu lisatud IaaS ketas on on salvestatud lehe bloobimälu VHD.

Jaoks väga suurte andmekogumite, kus võrgu piiranguid teha üles või alla andmete bloobimälu üle ebareaalne kaabel, saate saata kõvaketas Microsoft importida või eksportida andmed otse andmekeskuse. Lugege teemat [kasutamine Bloobivahemälu salvestusruumi andmete edastamiseks teenuse Microsoft Azure impordi/ekspordi](storage-import-export-service.md).

## <a name="table-storage"></a>Tabelimälu

Tänapäevane rakenduste nõuda andmete sageli suurem skaleeritavus ja paindlikult kui eelmised vajalik tarkvara. Tabelimälu pakub väga kättesaadav, oluliselt scalable salvestusruumi, nii, et rakenduse saab automaatselt mastaapida kasutaja rahuldamiseks. Tabelimälu on Microsofti NoSQL klahv/atribuut poe – on schemaless kujundus, muutes selle eri traditsiooniline relatsioonandmebaasidest. Schemaless andmesalve on lihtne, kui teie rakendus areneb edasi vajab andmete kohandamiseks. Tabelimälu on lihtne kasutada, et arendajad saate kiiresti luua rakendusi. Juurdepääs andmetele on kiire ja kulutõhus igasuguseid rakendusi.  Tabelimälu on tavaliselt märkimisväärselt madalam kulu traditsiooniline SQL-i jaoks sarnaseid andmemahtusid.

Tabelimälu on võti-atribuut talletamine, mis tähendab, et iga väärtuse tabelis on salvestatud tipitud atribuudi nimi. Filtreerimine ja kriteeriumide valiku saab atribuudi nimi. Atribuudid ja nende väärtuste kogumi koosnevad üksus. Kuna tabelimälu on schemaless, kaks üksust, samas tabelis võib sisaldada erinevaid kogumeid atribuudid ja nende atribuutide võib olla eri tüüpi.

Tabelimälu abil saate talletada paindlik andmekomplektide, nt veebirakenduste, aadressiraamatuid, seadme kohta ja muud tüüpi metaandmed, mis teie teenus nõuab kasutajaandmeid.  Saate salvestada mis tahes üksuste arv tabeli ja salvestusruumi konto võib sisaldada mis tahes arv tabelite salvestusruumi konto tööjõudlusega ulatuses.

Plekid ja järjekorrad, nt arendajate haldamine ja juurdepääs tabeli salvestusruumi kasutamise standard ülejäänud protokollid, aga tabelimälu ka toetab protokolli OData alamhulk lihtsustamine täpsemalt päringute võimaluste ja lubada JSON-i ja Atompubi (XML-i põhiste) vormingus.

Tänase Interneti-põhiste rakenduste NoSQL andmebaasidest tabelimälu pakkuda populaarne alternatiiv traditsiooniline relatsiooniandmebaasid.

## <a name="queue-storage"></a>Järjekorda salvestusruum

Rakenduste skaala väljatöötamisel komponendid on sageli sidumata, nii, et nad saavad mastaapimiseks sõltumatult. Järjekorda salvestusruumi pakub usaldusväärne sõnumside lahendus asünkroonne suhtlemine komponendid, kas need töötavad pilves, töölaual, kohapealse serveris või mobiilsideseadmes. Järjekorda salvestusruumi toetab ka asünkroonne ülesannete haldamiseks ja koostamise töövoogude.

Salvestusruumi konto võib olla mõni muu arv järjekorda. Järjekorda võib sisaldada mis tahes arv sõnumite salvestusruumi konto tööjõudlusega ulatuses. Üksikute sõnumite võib olla kuni 64 KB suurust.

## <a name="file-storage"></a>Failide talletamine

Azure'i failide talletamine pakub pilvepõhist SMB failikettad, nii, et saate migreerida pärand rakendused failikettad Azure kiiresti ja ilma kulukas rewrites. Azure'i failide talletamine, kus helistamine Azure'i virtuaalmasinates või pilveteenustega ja ühendage failikettale pilves, just nagu töölauarakendus kinnitused tüüpiline SMB ühiskasutus. Suvalist arvu komponendid saate siis mount ja juurdepääs salvestusruumi failikettale samaaegse.

Salvestusruumi failikettale on standardne SMB Failide ühiskasutamine, pääsevad Helistamine ja Azure andmete ühiskasutamine faili sytem i/o API-de kaudu. Arendajate seega saate kasutada oma kood ja oskusi migreerida olemasolevaid rakendusi. IT-spetsialistidele saate PowerShelli cmdlet-käskude abil saate luua, mount ja failiketastel salvestusruumi haldamine osana Azure rakenduste haldamine.

Muud Azure storage teenused, nt failisalvestusruum seab REST API ühiskasutamine andmetele juurdepääsuks. Kohapealne rakendusi saate helistada failide talletamine juurdepääsu andmetele Failide ühiskasutamine REST API-ga. Sellisel viisil ettevõte saate migreerida mõned pärand Azure rakendused ja jätkake töötab teistel oma ettevõttes. Pange tähele, et paigaldus faili ühiskasutusse anda ainult on võimalik helistamine ja Azure; faili ühiskasutus REST API kaudu juurdepääs vaid kohapealse rakenduse.

Hajutatud rakendusi saate kasutada ka failide talletamine talletamiseks ja ühiskasutuseks kasulik rakendus andmete ja arendamise ja testimise vahendid. Näiteks rakendus võib konfiguratsiooni faile talletada ja Diagnostikaandmete, nt logid, mõõdikute ja krahhi puistab salvestusruumi ühiskasutusse anda, nii et need on saadaval mitut virtuaalmasinates või rollide failis. Arendajate ja administraatorid saavad talletada Utiliidid, mis tuleb koostada või haldamiseks kõik komponendid, Saadaolev salvestusruum failikettale rakenduse installimine iga virtuaalse masina või rolli eksemplari asemel.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Juurdepääs bloobimälu, tabel, kuhjuda ja faili ressursse

Vaikimisi pääsevad ainult salvestusruumi konto omanik salvestusruumi konto ressursse. Andmete turvalisus, peavad olema kinnitatud iga taotluse vastu teie konto ressursse. Autentimine tugineb mudeli ühiskasutuses võti. Plekid konfigureerida toetamiseks anonüümne autentimine.

Mäluruumi teie konto on määratud kahe privaatne kiirklahvide loomise autentimise jaoks kasutatavad. On kaks võtit tagab taotlus on saadaval, kui te taastada regulaarselt turvalisus võtme haldamise tavaks võtmed.

Kui teil on vaja kasutajatel kontrollitud juurdepääsu oma salvestusruumi ressursse, siis saate luua ühiskasutusega juurdepääsu allkirja. Ühiskasutusega juurdepääsu allkiri (SAS) ei luba, mida saate lisada URL, mis lubab juurdepääsu delegeeritud salvestusruumi ressursi. Igaüks, kellel on luba juurdepääs see osutab õigustega, seda määrab, aeg, et see on lubatud ressurss. Azure Storage alustades versioon 2015-04-05, toetab kahte liiki ühiskasutusega juurdepääsu allkirjad: teenuse SAS ja SAS kontoga.

Teenuse SAS delegaatide juurdepääsu ressursile vaid ühes salvestusruumi teenuste: teenuse bloobimälu, järjekorda, tabelis või fail.

Konto SAS delegaatide ühe või mitme teenuseid salvestusruumi ressurssidele. Saate delegaadi juurdepääsu teenusetaseme toimingute kohta, mida ei ole saadaval teenusega SAS. Samuti saate delegeerida juurdepääsu lugeda, kirjutada ja kustutada bloobimälu ümbriste, tabelite, järjekorrad ja failiketastel, mis ei ole lubatud teenusega SAS.

Lisaks saate määrata ümbris ja selle plekid või teatud bloobimälu, on saadaval avaliku juurdepääsu. Kui näitate container või bloobimälu on avalik, keegi saab lugeda anonüümselt; pole autentimine on nõutav.  Avaliku ümbriste ja plekid on kasulikud asetades meediumi ja dokumente, mis on majutatud veebisaiti.  Kogu maailmas Võrgu latentsuse vähendamiseks saate vahemälu veebisaidid koos CDN Azure'i bloobimälu andmed.

Ühiskasutusega juurdepääsu allkirjade kohta lisateabe saamiseks vt [Abil ühiskasutusse antud juurdepääs allkirjad (SAS)](storage-dotnet-shared-access-signature-part-1.md) . Turvaline juurdepääs teie salvestusruumi konto kohta lisateabe saamiseks vt [haldamine anonüümse lugemisõigus ümbriste ja plekid](storage-manage-access-to-resources.md) ja [autentimise Azure'i salvestusruumi teenuseid](https://msdn.microsoft.com/library/azure/dd179428.aspx) .

## <a name="replication-for-durability-and-high-availability"></a>Kestvus ja kõrge-saadavus dispersioonanalüüs

Microsoft Azure salvestusruumi konto andmed on alati kopeeritud kestvus ja kättesaadavuse tagamiseks. Dispersioonanalüüs kopeerib andmete sama andmekeskuse või teise andmekeskuse, sõltuvalt sellest, millist dispersioonanalüüs suvandi valite. Dispersioonanalüüs kaitseb teie andmeid ja säilitab rakendus üles-aja siirdamiseks riistvara ebaõnnestumise korral. Kui teie andmed on kopeeritud teise andmekeskuses, mis ka kaitseb teie andmeid katastroofiline tõrge esmane asukohta.

Dispersioonanalüüs tagab, et konto salvestusruumi vastab [Teenusetaseme leping (SLA) Storage](https://azure.microsoft.com/support/legal/sla/storage/) isegi ees ebaõnnestumist. Vaadake lisateavet Azure Storage SLA tagab kestvus ja kättesaadavuseks. 

Salvestusruumi konto loomisel valige üks järgmistest Tiražeerimissuvandid:  

- **Kohalik liigsete salvestusruumi (LRS).** Kohalik liigsete salvestusruumi säilitab andmete kolm eksemplari. LRS korratakse kolm korda ühe andmekeskuse ühe piirkonna sees. LRS kaitseb teie andmeid: tavaline riistvara tõrkeid, kuid mitte ühe andmekeskuse tõrge.  

    LRS pakutakse allahindlust. Suurim lubatud kestvus, soovitame kasutada geograafilise liigne salvestusruumi, mis on kirjeldatud allpool.


- **Tsooni liigsete mälu (ZRS).** Tsooni liigsete salvestusruumi säilitab andmete kolm eksemplari. ZRS on kopeeritud kolm korda üle 2-3 vahendid, kas ühest või mitmest mõlema piirkonna, pakkudes suurem kui LRS kestvus. ZRS tagab, et teie andmed on ühest püsival.  

    ZRS pakub kõrgema taseme kestvus kui LRS; siiski maksimaalne kestvus, soovitame kasutada geograafilise liigne salvestusruumi, mis on kirjeldatud allpool.  

    > [AZURE.NOTE] ZRS on praegu saadaval ainult Blokeeri plekid ja on ainult toetatud versioonid 2014-02-14 ja uuemad versioonid.
    >
    > Kui olete loonud salvestusruumi konto ja valitud ZRS, ei saa teisendada selle koopia, mis tahes muud tüüpi abil või vastupidi.

- **Geograafilise liigne salvestusruumi (GRS)**. GRS säilitab andmete kuus koopiad. GRS, andmete korratakse kolm korda esmane piirkonnas ja on ka kopeeritud teisene piirkonna sadu miili eemale esmane piirkond, pakkudes kõrgeima taseme kestvus kolm korda. Korral veebisaidil esmane piirkond, kuvatakse Azure Storage Tõrkesiirde teisene regioon. GRS tagab, et teie andmed on püsival kahe omaette piirkondades.

    Esmaseid ja teiseseid sidumiste regiooniti kohta leiate teemast [Azure regioonid](https://azure.microsoft.com/regions/).

- **Lugemisõigus – geograafilise liigne salvestusruumi (RA GRS)**. Lugemisõigus – geograafilise liigne salvestusruumi tiražeerib andmete teisese geograafilise asukoha ja pakub andmete teisese asukoha lugemisõigus. Lugemisõigus – geograafilise liigne salvestusruumi võimaldab kasutada andmete esmane või teisene asukoht, juhul kui ühes kohas kättesaamatu. Lugemisõigus – geograafilise liigne salvestusruumi on vaikimisi konto salvestusruumi vaikesäte selle loomisel. 

    > [AZURE.IMPORTANT] Saate muuta, kuidas teie andmed on kopeeritud pärast salvestusruumi konto on loodud, kui määrasite ZRS konto loomisel. Pange tähele, et teil võib tekkida täiendavad ühekordse andmeedastus maksab, kui vahetate LRS GRS või RA-GRS.

Salvestusruumi Tiražeerimissuvandid kohta lisateabe saamiseks lugege teemat [Azure Storage dispersioonanalüüs](storage-redundancy.md) .

Salvestusruumi konto dispersioonanalüüs hinnateavet, leiate [Azure'i salvestusruumi hinnad](https://azure.microsoft.com/pricing/details/storage/). Millised teenused on saadaval iga piirkonna kohta lisateavet teemast [Azure piirkondade](https://azure.microsoft.com/regions/#services) .

Arhitektuuriline kestvus Azure Storage kohta lisateavet [SOSP paberi - Azure Storage: A tugevalt saadaval salvestusruumi pilveteenuses tugev järjepidevus](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Andmete edastamiseks ja sealt Azure salvestusruum

Saate kopeerida bloobimälu, failide ja tabeliandmed kontol salvestusruumi ühest või mitmest salvestusruumi kontod AzCopy käsurea kasuliku. Lisateabe saamiseks vaadake [AzCopy käsurea kasuliku andmete edastamiseks](storage-use-azcopy.md) .

AzCopy ehitatud [Azure'i andmed liikumine teek](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), mis on praegu saadaval eelvaade.

Azure'i impordi/ekspordi teenuse võimaldab bloobimälu andmeid importida või bloobimälu andmete eksportimine kõvakettale kettal postitatakse Azure andmekeskuse konto salvestusruumi. Import/eksport teenuse kohta lisateabe saamiseks leiate teemast [Microsoft Azure'i impordi/ekspordi teenuse bloobimälu andmete edastamiseks](storage-import-export-service.md).

## <a name="pricing"></a>Hinnad

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Salvestusruumi API-d, teekide ja tööriistad

Azure'i salvestusruumi ressursid pääseb mis tahes keeleks, mida saab teha HTTP/HTTPS päringuid. Lisaks Azure Storage pakub programmeerimise teekide jaoks mitme levinud keeled. Need teegid lihtsustada paljude aspektide töötamine Azure Storage töötlemise üksikasjad, nt sünkroonse ja asünkroonse kutsumise, partiide toimingud, erand haldus, automaatne korduskatsed, funktsionaalseid käitumine jne. Teekide on praegu saadaval järgmised keeled ja platvormid teistega teel.

### <a name="azure-storage-data-services"></a>Azure'i salvestusruumi Data Services

- [Salvestusruumi teenuste REST API-ga](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Salvestusruumi kliendi teek .net-i, Windows Phone ja Windows kestus](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Salvestusruumi kliendi teek c++](https://github.com/Azure/azure-storage-cpp)
- [Salvestusruumi kliendi teek Java/Androidi jaoks](/develop/java/)
- [Salvestusruumi kliendi teek Node.js jaoks](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Salvestusruumi kliendi teek php](/develop/php/)
- [Salvestusruumi kliendi teek, mis](/develop/ruby/)
- [Salvestusruumi kliendi teek Python jaoks](/develop/python/)
- [Salvestusruumi 1.0 PowerShelli cmdlet-käsud](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure'i salvestusruumi halduse teenused

- [Salvestusruumi ressursi pakkuja REST API viide](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Salvestusruumi ressursi pakkuja kliendi teek .net-i jaoks](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Salvestusruumi ressursi pakkuja cmdlettide PowerShelli 1.0 jaoks](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Salvestusruumi teenuse haldamine REST API (klassikaline)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure'i salvestusruumi andmeteenuste liikumine

- [Salvestusruumi impordi/ekspordi teenuse REST API-ga](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Salvestusruumi andmete liikumine kliendi teek .net-i jaoks](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Tööriistad ja Utiliidid

- [Azure'i salvestusruumi Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure'i salvestusruumi kliendi tööriistad](storage-explorers.md)
- [Azure'i SDK-d ja tööriistad](https://azure.microsoft.com/tools/)
- [Azure'i salvestusruumi emulaator](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure'i PowerShelli](../powershell-install-configure.md)
- [AzCopy käsurea kasuliku](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Järgmised sammud

Lisateavet Azure Storage, saate nende ressursside:

### <a name="documentation"></a>Dokumentatsioon

- [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Administraatorite jaoks

- [Azure'i salvestusruumi Azure PowerShelli kasutamine](storage-powershell-guide-full.md)
- [Azure'i salvestusruumi Azure CLI kasutamine](storage-azure-cli.md)

### <a name="for-net-developers"></a>.NET arendajatele

- [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md)
- [Azure'i tabelimälu .net-i kasutamise alustamine](storage-dotnet-how-to-use-tables.md)
- [Alustamine Azure'i järjekorda salvestusruumi .net-i abil](storage-dotnet-how-to-use-queues.md)
- [Alustamine Windows Azure'i failide talletamine](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Java/Android arendajatele

- [Kuidas kasutada bloobimälu Java kaudu](storage-java-how-to-use-blob-storage.md)
- [Kuidas kasutada tabelimälu Java kaudu](storage-java-how-to-use-table-storage.md)
- [Kuidas kasutada järjekorda salvestusruumi Java kaudu](storage-java-how-to-use-queue-storage.md)
- [Kuidas kasutada Java: failide talletamine](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Node.js arendajatele

- [Kuidas kasutada bloobimälu Node.js kaudu](storage-nodejs-how-to-use-blob-storage.md)
- [Kuidas kasutada tabelimälu Node.js kaudu](storage-nodejs-how-to-use-table-storage.md)
- [Kuidas kasutada järjekorda salvestusruumi Node.js kaudu](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>PHP arendajatele

- [Kuidas kasutada alates PHP bloobimälu](storage-php-how-to-use-blobs.md)
- [Kuidas kasutada alates PHP tabelimälu](storage-php-how-to-use-table-storage.md)
- [Kuidas kasutada järjekorda salvestusruumi alates PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Foneetiline arendajatele

- [Kuidas kasutada bloobimälu Ruby kaudu](storage-ruby-how-to-use-blob-storage.md)
- [Kuidas kasutada tabelimälu Ruby kaudu](storage-ruby-how-to-use-table-storage.md)
- [Kuidas kasutada järjekorda salvestusruumi Ruby kaudu](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python arendajatele

- [Kuidas kasutada bloobimälu Python kaudu](storage-python-how-to-use-blob-storage.md)
- [Kuidas kasutada tabelimälu Python kaudu](storage-python-how-to-use-table-storage.md)
- [Kuidas kasutada järjekorda salvestusruumi Python kaudu](storage-python-how-to-use-queue-storage.md)
- [Kuidas kasutada Python: failide talletamine](storage-python-how-to-use-file-storage.md)
