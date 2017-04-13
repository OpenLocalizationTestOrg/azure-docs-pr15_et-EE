<properties 
    pageTitle="Levinud DocumentDB kasutamine juhtudel | Microsoft Azure'i" 
    description="Teave ülaosas viis kasutada juhtudel DocumentDB: kasutaja loodud sisu, sündmuste logimine, andmete kataloogi, kasutaja eelistused andmed ja Internet, asjade." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Enamikul juhtudel DocumentDB kasutamine
Selles artiklis antakse ülevaade mitme levinud kasutamise juhtudel DocumentDB jaoks.  Selle artikli soovitused olla lähtepunktina arendate rakenduse abil DocumentDB.   

Pärast selle artikli lugemist on teil saama vastavad järgmistele küsimustele. 
 
- Mis on levinud kasutamise juhtudel DocumentDB jaoks?
- Mis on kasutaja DocumentDB kasutamise eeliseid loodud sisu poe?
- Mis on DocumentDB kataloogi andmete kasutamise eeliseid talletada?
- Mis on kasu kasutamine DocumentDB sündmuste logi talletada?
- Mis on kasutaja eelistused andmetena DocumentDB kasutamise eeliseid talletada?
- Mis on kasu kasutamine andmete poe DocumentDB Internet, asjade süsteemidele?

## <a name="common-use-cases-for-documentdb"></a>Levinud kasutamine karpide DocumentDB
Azure'i DocumentDB on üldine otstarve NoSQL andmebaas, mis kasutatakse mitmesuguseid rakenduste ja kasutamise juhtudel. See on hea valik mis tahes rakendus, mis peab madal järjestuses-millisekundilist vastuse korda ja peab olema mastaapimiseks kiiresti. Järgnevalt mõned DocumentDB atribuute, mis muudavad selle sobivad hästi suure jõudlusega rakenduste.

- DocumentDB piirded algupäraselt andmete kõrge-saadavus ja skaleeritavus.
- DocumentDB kasutaja on tagatud SSD salvestusruumi latentsusajaga järjestuses-millisekundilist vastuse korda.
- Vormindusühtsuse tasemed nagu lõpliku, seansi ja mis on piiratud staleness DocumentDB's tugi võimaldab madal maksumus, et täitmise-suhe. 
- DocumentDB on paindlik andmete sõbralik hinnakirjad mudelit, mis meetrit salvestus- ja läbilaskevõime sõltumatult.
- DocumentDB's reserveeritud läbilaskevõime mudel võimaldab mõelda loeb/kirjutab asemel CPU/mälu/IOPs aluseks oleva riistvara arv.
- DocumentDB's kujundus võimaldab muudate suuri taotluse mahud miljardit taotlusi päevas järjekorras.

Neid atribuute on eriti kasulik, kui tegemist on web, mobiil, mängimine ja asjade rakendusi, mis tuleb madal vastuse korda ja tuleb toime suurel hulgal loeb ja kirjutab. 

## <a name="user-generated-content"></a>Kasutaja loodud sisu
Levinud DocumentDB puhul kasutada on talletamiseks ja päringu kasutaja loodud sisu (UGC) veebi ja mobiilirakendusi, eriti sotsiaalmeedia rakendusi.  Mõned näited UGC on vestelda seansside, tweets, ajaveebipostituste, hinnangute ja kommentaarid.  Sageli UGC sotsiaalmeedia rakendustes on tasuta vormi teksti, atribuutide, sildid ja seostest, mis ei suuda jäik struktuur segu.   

Sisu, nt vestlused, kommentaarid ja postituste saate salvestatakse DocumentDB nõudmata teisendused või keerukas objekti relatsiooniline vastenduse kihte.  Andmete atribuudid saate lisada või muuta lihtsalt vastavad nagu arendajate itereerima üle rakenduse koodi, seega edendada kiire areng.  

Rakenduste integreerimine erinevate suhtlusvõrgustike peab reageerima skeemid need võrgu kaudu.  Kui andmed on vaikimisi DocumentDB automaatselt registrisse, andmed on valmis igal ajal esitatakse selle kohta päring.  Seega on need rakendused paindlikkust tuua prognooside kohta nende vastavate vajadustele.       

Paljud social rakendusi käivitada maailmas ja võib kaasa tuua ettearvamatuid mustreid.  Paindlikkust skaleerimist andmesalve on oluline rakenduse kiht skaala kasutus nõudmisel vastavaks.  Te saate skaala, lisades täiendavate andmete sektsioonid DocumentDB kasutajakontot.  Lisaks saate luua ka DocumentDB täiendavate kontode mitme piirkondade lõikes. DocumentDB teenuse saadaval, leiate [Azure'i regioonid](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Kataloogi andmed
Kataloogi andmete kasutamise stsenaariumid kaasata talletamine ja päringute üksused, nt inimesed, kohad ja toodete atribuutide kogum.  Mõned näited kataloogi andmed on kasutajakontod, toote kataloogid, seadme registrite asjade ja arve, mida kasutati materjalid.  Fondiatribuudid andmed võivad erineda ja aja jooksul rakenduse nõuded mahutamiseks muuta.  

Kaaluge tootekataloogi näide on auto osade tarnija. Iga osa võib-olla Lisaks levinud atribuute, mida kõik osad jagada oma atribuute.  Lisaks fondiatribuudid teatud osade muutmiseks kui uue mudeli välja järgmine aasta.  Vastavalt JSON dokumendi talletamine, DocumentDB toetab paindlik skeemid võimaldab teil pesastatud omadustega andmete esitamiseks ja seega on sobivad hästi toote kataloogi andmete talletamiseks.       

## <a name="logging-and-time-series-data"></a>Logimine ja kellaaja andmesarja
Rakenduse logimine on sageli väljapaisatud suuri andmemahtusid ja võib juurutatud rakenduse versioon või komponent logimine sündmuste vastavalt erineva atribuute.  Logiandmed ei ole piiratud keerukate seoste või jäik struktuurid. Järjest, logiandmed on jätkunud JSON-vormingus JSON on kerge ja lihtne inimestele lugeda.
   
Tavaliselt on kaks põhilist kasutamise juhtudel seotud sündmuselogi andmeid.  Esimese kasutamine puhul on ajutine päringuid teha tõrkeotsingu andmete alamhulga.  Ajal tõrkeotsing, andmete alamhulga esmalt tuuakse logid, tavaliselt sarja kellaaja järgi.  Seejärel drill-down toimub filtreerimine andmekomplekti tõrge taset või tõrketeated. See on koht, kus talletamine sündmuselogide DocumentDB on mõistlik. Vaikimisi automaatselt registrisse talletatud DocumentDB logiandmed ja seega on valmis igal ajal esitatakse selle kohta päring. Logiandmed sunnita Lisaks andmete sektsioonid kogu aeg sarjana. Vanema logid saab aidata külmladustamine oma säilituspoliitika kohta.          

Teine kasutamine tegemist kaua töötavad toimub ühenduseta kohal logiandmed suurel hulgal analytics töö.  Sel juhul kasutada näiteks serveri saadavuse analüüs, rakenduse veaväärtuse analysis ja Click Stream Andmeanalüüs.  Tavaliselt kasutatakse Hadoopi teha järgmist tüüpi analüüse.  Hadoopi konnektor DocumentDB jaoks, kus DocumentDB andmebaaside toimida andmeallikad ja sidujatena siga, taru ja kaardi/vähenda tööde haldamine. Hadoopi konnektor jaoks DocumentDB kohta täpsema teabe saamiseks vt [Hadoopi töö DocumentDB ja Hdinsightiga](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Mängimine
Andmebaasi taseme on oluline osa mängimine rakendused. Tänapäevane visualiseerimine sooritada graafiline töötlemine mobile/konsooli kliendid, kuid toetuvad pilveteenuses kohandatud ja isikupärastatud sisu nagu-mängu statistika, sotsiaalmeedia integreerimise ja kõrge-keskmine leaderboards esitamisel. Visualiseerimine väga väike latentsused nõue loeb kirjutab pakkuda huvitavat-mängu kogemusi ja andmebaasi taseme peab käsitlema käekäigu ja madalad taotluse määrade ajal uusi mängu käivitab ja funktsioonide värskendustega.

DocumentDB kasutatakse poolt suuri skaala nagu [The elavad surnud: ei mees maa](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) [Järgmise visualiseerimine](http://www.nextgames.com/), ja [Halo 5: hooldajate kohta käiva](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Nii kasutamine juhtudel, DocumentDB põhilisi eeliseid on järgmine:

- DocumentDB võimaldab tuleb venitada üles või alla elastically. See võimaldab visualiseerimine käsitlema värskendamist profiili ja statistika kaudu kümneid miljoneid samaaegne mängijad, tehes ühte API kõne.
- DocumentDB toetab millisekundilist loeb ja kirjutab vältida jääb mängu ajal.
- DocumentDB's automaatse indekseerimise võimaldab esitada mitut eri atribuutide reaalajas filtreerimiseks, nt otsige üles oma sisemise mängija ID-d või nende GameCenter, Facebooki, Google ID-d või päringu põhjal Playeri kuuluvus gild mängijad. See on võimalik keerukas indekseerimine või sharding taristu ilma.
- Suhtlusfunktsioonide, sh-mängu vestluse sõnumid, mängija gild liikmelisused, probleemid, mis on lõpetatud, kõrge-keskmine leaderboards ja social graafikud on lihtsam paindlik skeemi rakendada.
- DocumentDB hallatavate platvormi-kui-a-teenust (PaaS) nõutav minimaalne häälestamine ja haldamine töö kiire iteratsiooni lubamiseks ja vähendada aega Marketist.


## <a name="user-preferences-data"></a>Kasutaja eelistused andmed
Tänapäeval kõige veebi- ja mobiilirakendused on keerukas vaadete ja kogemusi. Need vaated ja kogemused on tavaliselt dünaamiline toitlustus kasutajaeelistuste või meeleolu ja sümboolika vajadustele.  Seega peavad saama tuua isikupärastatud sätted tõhusalt, et muuta Kasutajaliidese elemendid ja funktsioonid kiirelt rakendused. 

JSON on UI paigutus andmete esitamiseks, kui see pole ainult kerge, kuid samuti saate hõlpsasti tõlgendada JavaScripti efektiivse vorming.  DocumentDB pakub Timmitavad järjepidevuse taset, mis võimaldavad kiiresti loeb madal latentsus kirjutab abil. Seega, sh isikupärastatud sätted JSON dokumentidena DocumentDB UI paigutus andmete salvestamiseks on tõhus vahend andmed arusaadavaks kaabel.

## <a name="iot-and-device-sensor-data"></a>Asjade ja seadme andurite andmeid
Asjade kasutamise juhtudel tavaliselt jagada mõne mustrite ja kuidas neid neelata, töötlemine ja andmete talletamiseks.  Esmalt tarvilikud luba andmete tarbimine, mis võivad neelata puruneb seadme andurid eri asukohtades andmeid.  Järgmiseks tarvilikud töötlemine ja reaalajas ülevaateid tuletada streaming andmete analüüsiks. Ja lõpuks, kõige rohkem kui mitte kõik andmed lõpuks maa adhoc päringu ja ühenduseta Analyticsi andmed salvestada.    

Microsoft Azure'i pakub rikkaliku teenused, mida saate kasutada asjade jaoks kasutada juhtudel.  Azure'i asjade teenustele kehtivad teenuste, sh Azure sündmuse jaoturi, Azure'i DocumentDB, Azure voo Analytics, Azure'i teate jaoturi, Azure seadme õ, Windows Azure Hdinsightiga ja PowerBI kogum. 

Andmete puruneb võib märkimisväärselt Azure'i sündmuse jaoturi, mis annab kõrge läbilaskevõime andmete manustamisest koos madal latentsus. Märkimisväärselt andmeid, mida tuleb töödelda reaalajas ülevaate saate funneled Azure'i voo Analytics abil reaalajas Analyticsi. Andmeid saab laadida DocumentDB adhoc päringute. Kui andmed on laaditud DocumentDB, andmed on valmis esitada.  Andmete DocumentDB saate kasutada viide andmete reaalajas analytics osana. Lisaks andmete saab edasi rafineeritud ja töödelda DocumentDB andmete ühendamine Hdinsightiga siga, taru või kaardi/vähenda töökohta.  Rafineeritud andmed on laaditud seejärel naasmiseks DocumentDB aruannete jaoks.   

Valimi asjade lahenduse DocumentDB, EventHubs ja torm abil, vt [hdinsightiga-storm-näited hoidla github](https://github.com/hdinsight/hdinsight-storm-examples/).

Azure'i pakkumisi jaoks asjade kohta leiate lisateavet teemast [teie asjade Interneti loomine](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Järgmised sammud
 
Alustada DocumentDB, saate luua [konto](https://azure.microsoft.com/pricing/free-trial/) ja seejärel järgige meie [õppeteema](https://azure.microsoft.com/documentation/learning-paths/documentdb/) teada DocumentDB ja vajaliku teabe. 

Või kui soovite lugeda Lisateavet kohta klientidele, kes kasutavad DocumentDB, on saadaval järgmised kliendi lood:

- [Järgmise visualiseerimine](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Walking Dead: eikellegimaal mängu soars # 1 Azure'i DocumentDB ei toeta.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Halo 5 rakenduspõhimõtteid social gameplay Azure'i DocumentDB abil.
- [Cortana Analytics Galerii](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Galerii - ehitatud Azure'i DocumentDB scalable kogukonnafoorumi saidi.
- [Tuul](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Viib integraator annab kohalikke ettevõtteid globaalne ülevaate paindlik Cloud tehnoloogiatega minutites.
- [Uudised Vabariik](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Teavet eesmärgiga tööle ettevõtetele uudiste ärianalüüsi lisamine. 
- [SGS rahvusvaheline](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Ühtse värvi ja kogu maailmas suurte kaubamärkide pöörduda SGS. Ja SGS lülitab Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Globaalne juhataja Telenor kasutab liikumiseks käivitamisel järgima pilve. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Poe tulevikus töötab Kiire otsingu ja andmete lihtne liikumist.
