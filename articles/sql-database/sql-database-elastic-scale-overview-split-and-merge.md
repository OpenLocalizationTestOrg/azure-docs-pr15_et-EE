<properties 
    pageTitle="Andmete teisaldamine mastaabitud kontorist pilve andmebaasi vaheliste | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse töödelda shards ja ise hostitud vahendusel elastne andmebaasi API-de kasutamine andmete teisaldamiseks." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Mastaabitud kontorist pilve andmebaaside andmete liikumine

Kui teil on teenuse arendajate tarkvara ja äkki rakenduse läbib suurt nõudmisel, peate kasvuga mahutamiseks. Nii saate lisada rohkem andmebaasid (shards). Kuidas levitada andmed uute andmebaaside ilma katkestades andmete terviklust? Teisaldage andmed piiratud andmebaaside uute andmebaaside **tükeldatud ühendamine tööriista** abil.  

Tükeldatud ühendamine tööriist käivitatakse Azure web teenust. Administraator või arendaja kasutab tööriista shardlets (andmed on Kildu) andmebaaside (shards) vahel liikumiseks. Tööriist kasutab Kildu kaardi halduse teenuse metaandmete andmebaasi, ja veenduge, et ühtsete vastendused.

![Ülevaade][1]

## <a name="download"></a>Laadi alla
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Dokumentatsioon
1. [Elastne andmebaasi tükeldamine ühendamine tööriista õpetus](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Tükeldatud ühendamine turvalisus konfigureerimine](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Tükeldatud ühendamine turvakaalutlused](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Kildu kaardi haldus](sql-database-elastic-scale-shard-map-management.md)
* [Olemasolevaid andmebaase skaala-out migreerimine](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Elastne Andmebaasiriistad](sql-database-elastic-scale-introduction.md)
* [Elastne andmebaasi tööriistad sõnastik](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Miks kasutada tükeldatud ühendamine tööriista?

**Paindlikkus**

Rakenduste ei pea venitada paindlikult üle ühe DB Azure SQL-i andmebaasi. Andmete teisaldamine vastavalt vajadusele uute andmebaaside usaldusväärsuse säilitades tööriista abil.

**Tükeldatud kasvab** 

Peate plahvatusliku kasvu käsitlema üldine suurendama. Selleks täiendavate võimaluste loomine sharding andmed ja levitamise üle sammhaaval rohkem andmebaase, kuni võimsus vajadused on täidetud. See on näide funktsiooni "tükeldada. 

**Kahanda ühendamine**

Ettevõtte hooajaline laadist vajab Kahanda võimsus. Tööriist võimaldab teil vähem skaala kestusühikud kui business aeglustab mahtu. Funktsiooni "Ühenda" elastne skaala tükeldatud ühendamine teenus hõlmab see nõue. 

**Pääsupunktidega, teisaldades shardlets haldamine**

Mitme rentniku andmebaasi kohta, kus soovite shards shardlets jaotamine võib kaasa tuua võimsus kitsaskohtade mõned shards. Selleks on vaja uuesti eraldamiseks shardlets või hõivatud shardlets teisaldamine uude või vähem kasutatud shards. 

## <a name="concepts--key-features"></a>Mõisted ja peamised omadused

**Kliendi majutatud teenused**

Tükeldatud ühendamine on esitatud kliendi majutatud teenust. Peate juurutada ja teenust majutada oma Microsoft Azure'i tellimus. Saate alla laadida Nugeti pakett sisaldab konfiguratsiooni malli lõpetamiseks ja teatud juurutamise kohta. [Tükeldatud ühendamine õpetuse](sql-database-elastic-scale-configure-deploy-split-and-merge.md) üksikasju vt. Kuna Azure tellimuse töötab teenus, saate juhtida ja konfigureerida teenuse enamikule turvalisus. Vaikemalli sisaldab suvandeid konfigureerida SSL-i, sert-klient autentimise, krüptimise talletatud mandaatide, DoS Valve ja IP-piirangud. Pärast dokumendi [ühendamine tükeldatud turvalisuse konfiguratsiooni](sql-database-elastic-scale-split-merge-security-configuration.md)leiate turvalisuse kohta lisateavet.

Vaikimisi juurutatud teenuse käivitatakse ühe töötaja ja ühe web roll. Iga kasutab A1 VM suurus Azure'i pilveteenustega. Ajal paketi juurutamisel ei saa te neid sätteid muuta, on võimalik muuta need pärast eduka juurutamise töötava pilveteenuses (Azure portaali) kaudu. Pange tähele, et tehnilise põhjustel ei tohi konfigureerida rohkem kui ühe eksemplari töötaja roll. 

**Kildu kaardi integreerimine**

Tükeldatud ühendamine teenuse suhtleb Kildu kaardi taotluse. Tükeldamine või vahemike või shardlets shards vahel liikumiseks tükeldatud ühendamine teenuste kasutamisel teenus hoiab automaatselt Kildu kaardi kursis. Selleks teenuse Kildu kaardi Manageri andmebaasiga rakenduse loob ja säilitab vahemiku ja vastenduste nagu teisaldamine/tükeldatud/Ühenda taotlusi edu. See tagab, et Kildu kaardi annab alati on ajakohane ülevaate, kui tükeldatud ühendamine toimingud on käimas. Tükeldamine, Ühenda ja shardlet liikumine tegevust rakendatakse teisaldades shardlets partii allika Kildu target Kildu. Funktsiooni shardlet ajal liikumine toiming shardlets kehtivad praeguse paketi ühenduseta Kildu kaardil on märgitud ja pole saadaval andmed sõltuvad marsruudi ühenduste abil **OpenConnectionForKey** API. 

**Ühtse shardlet ühendused**

Andmete liikumine käivitamisel uue töölehe shardlets, mis tahes Kildu-kaardi esitatud andmed sõltuvad marsruutimise ühendused on Kildu salvestamise funktsiooni shardlet on tapetud ja järgmiste ühendused Kildu kaardi API-de nende shardlets on blokeeritud töötamise andmete liikumine vastuolude vältimiseks. Ühenduste muude shardlets sisse sama Kildu ka tapeti, kuid õnnestub uuesti kohe uuesti sisse. Kui paketi teisaldatakse, märgitakse selle shardlets online uuesti target Kildu ja lähteandmete eemaldatakse Kildu allikas. Teenuse läbib neid juhiseid iga töölehe seni, kuni kõik shardlets on teisaldatud. Selle tulemusena mitme ühenduse tappa toimingu käigus tükeldatud/Ühenda/Teisalda toimingu lõpuleviimiseks.  

**Shardlet-saadavus haldamine**

Piirata shardlets nagu eespool avatud paketti tappes ühenduse piirab saada üks pakett shardlets, kui soovite korraga. See on eelistatud üle lähenemisviisi kui täieliku Kildu jääks tükeldatud või kirjakooste toiming käigus ühenduseta kõik selle shardlets jaoks. Paketi, määratletud erinevate shardlets teisaldamiseks korraga arvu suurus on konfiguratsiooni parameeter. Saate määrata iga tükeldatud ja Ühenda toimingu sõltuvalt rakenduse-saadavus ja jõudluse vajadustele. Pange tähele, et vahemiku, mida on lukustatavad Kildu kaardil võib olla suurem kui paketi suurus. See on teenuse huvitavat vahemiku suurust nii, et sharding väärtused andmeid tegelik arv vastab ligikaudu paketi suurus. See on oluline meeles pidada, eriti hajaasustusega sharding võtmed. 

**Metaandmete salve**

Tükeldatud ühendamine teenuse kasutab andmebaasi säilitada tema oleku ja hoida logid taotluse töötlemise ajal. Kasutaja loob see andmebaas oma tellimust ja antakse ühendusstringi selle teenuse juurutamiseks konfiguratsioonifailis. Administraatorid kasutaja ettevõttest saate luua ühenduse ka see andmebaas üle vaadata taotlus ja uurida üksikasjalikku teavet võimalike tõrgete kohta.

**Sharding – teadlikkuse**

Tükeldatud ühendamine teenuse eristab (1) sharded tabelite, (2) tabelid ja (3) tavaline tabelid. Tükeldatud/Ühenda/Teisalda toimingu semantika sõltuvad kasutatava tabeli tüüp ja on määratletud järgmiselt: 

* **Sharded tabelite**: tükeldatud, Ühenda ja move toiminguid teisaldamine shardlets allika target Kildu. Pärast eduka üldine kutse, ei ole enam neid shardlets allika Esita. Pange tähele, et target tabelid olemas target Kildu tuleb enne selle toimingu töötlemist target vahemiku andmed ei tohi sisaldada. 

* **Tabelid**: tabelid, tükeldamine, ühendamine ja teisaldada toimingute Kopeerige andmed lähtekoha target Kildu. Pange tähele, et muutusteta ilmneb target Kildu antud tabeli kui ükskõik millisele reale on juba olemas, selle tabeli sihtkohas. Tabelis on tühi, mis tahes viide tabeli koopia toiming saada töödeldud jaoks.

* **Muud tabelid**: teiste tabelitega võib esineda allika või tükeldatud ja Ühenda toiming sihtkohas. Tükeldatud ühendamine teenuse tähelepanuta need tabelid, mis tahes andmete liikumise või Kopeeri toimingute jaoks. Pange tähele, võivad häirida nende toimingute puhul piiranguid.

Funktsiooni viide vs sharded tabelite kohta teavet **SchemaInfo** API-de Kildu kaart. Nende API-de kohta antud Kildu kaardi halduri objekti smm on näidatud järgmises näites: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Tabelid "piirkond" ja "rahvus" on määratletud tabelid ja kopeeritakse teisaldamine/tükeldatud/Ühenda toimingutega. "kliendi" ja "tellimused" omakorda on määratletud sharded tabelid. C_CUSTKEY ja O_CUSTKEY olla sharding võti. 

**Viitamistervikluse jõustamine**

Tükeldatud ühendamine teenuse analüüsib sõltuvused tabelite vahel ja kasutab võõrkeelsed primaarvõtme olulised seosed etapp liikumiseks tabelid ja shardlets toimingud. Üldiselt tabelid kopeeritakse esmalt sõltuvus järjestuses, siis kopeeritakse shardlets järjekorras nende sõltuvusi iga paketi sees. See on vajalik, et FK-PK target Kildu piirangud on au uute andmete saabub. 

**Kildu kaardi järjepidevuse ja lõpuks lõpuleviimine**

Tõrkeid, tükeldatud ühendamine teenuse kasutataks toimingud pärast mis tahes katkestuste ja eesmärk on mis tahes edenemise taotlusi lõpuleviimiseks. Siiski võib esineda parandamatu olukordades, nt target Kildu on kadunud või ei saanud taastada enam turvaline. Neil tingimustel mõned shardlets, mis ei peaks seda teisaldada jätkuvalt asuvad allika Kildu. Teenuse tagab, et shardlet vastendused värskendatakse ainult pärast vajalikud andmed on edukalt kopeeritud soovitud sihtkohta. Shardlets kustutatakse ainult allikas, kui kõik oma andmed kopeeriti sihtrühma ja vastavate vastendused on värskendatud. Kustutamine toimub taustal ajal vahemik on juba online target Kildu. Tükeldatud ühendamine teenuse tagab alati õigsuse talletatud Kildu kaardi vastendused.


## <a name="the-split-merge-user-interface"></a>Tükeldatud ühendamine kasutajaliides

Tükeldatud ühendamine pakett sisaldab töötaja roll ja web roll. Web roll saab interaktiivselt tükeldatud ühendamine taotlusi esitada. Kasutajaliidese põhilised on järgmised:

-    Toimingu tüüp: Toimingu tüüp on raadionupp, mis määrab selle päringu teenuse sooritatud toimingu tüüpi. Saate valida tükeldamine, ühendamine ja stsenaariumid teisaldada. Samuti saate tühistada varem esitatud toiming. Saate kasutada tükeldatud, ühendamine ja teisaldada vahemiku Kildu kaartide taotlused. Loendi Kildu kaardid ainult tugi move toiminguid.

-    Kildu kaart: Järgmise jaotise taotluse parameetrite katta teavet Kildu kaardi ja majutusteenuse Kildu kaardi andmebaasi. Peate Azure'i SQL-andmebaasi server ja andmebaas majutusteenuse shardmap, identimisteavet ühenduse loomiseks Kildu kaartide andmebaas ja lõpuks Kildu kaardi nimi nimi. Praegu toiming on võimalik ainult ühe mandaadikomplekt. Neid mandaate vaja teha muudatusi Kildu kaardi esmakordseks kasutaja andmeid shards piisavaid õigusi.

-    Lähtevahemikus (tükeldamine ja ühendamine): tükeldatud ja Ühenda toiming töötleb vahemiku abil oma kõrge ja madala võti. Määramaks toimingu ääretu kõrge võtmeväärtuse, märkige ruut "kõrge on maksimaalne" ja kõrge võtme välja tühjaks jätta. Vahemiku väärtus teie määratud pole vaja täpselt vastama vastenduse ja nende piirid Kildu kaart. Kui te ei määra mis tahes vahemiku piirmäärad üldse teenuse tuletab ise lähima vahemiku teie jaoks automaatselt. Saate tuua praeguse vastendused antud Kildu kaardil GetMappings.ps1 PowerShelli skripti.

-    Tükeldatud allika käitumine (tükeldatud): tükeldatud toimingute puhul määratleda tükeldamiseks andmeallika vahemiku punkti. Tehke seda esitada, kuhu soovite tükeldatud tekkida sharding võti. Kasuta raadionuppu saate määrata, kas soovite alumine osa vahemikus (välja arvatud tükeldatud võti) liikumiseks, või kas soovite ülemises osas (sh tükeldatud võti) liikumiseks.

-    Andmeallika Shardlet (liikumine): teisaldamine toimingud erinevad tükeldatud või Ühenda, nagu nad ei nõua vahemiku allika kirjeldamiseks. Allika liikuda lihtsalt tuvastatakse sharding võtmeväärtuse, mida soovite teisaldada.

-    Suunata Kildu (tükeldatud): kui olete andnud oma tükeldatud toiming allika teave, peate määratleda, kuhu soovite andmed kopeerida sisestada sihtkohas Azure SQL Db server ja andmebaas nime.

-    Sihtväärtust (ühendamine): Ühenda toimingute shardlets liikuda mõne olemasoleva Kildu. Saate määrata olemasoleva Kildu esitada vahemiku piirid olemasoleva vahemiku, mida soovite ühendada.

-    Paketi suurus: Määrab paketi suurus arv, mis läheb ühenduseta andmete liikumise ajal shardlets. See on täisarv, kus saate kasutada väiksemad väärtused, kui olete tundlik pikemaks ajaks, tööseisakute shardlets jaoks. Suuremate väärtuste korral suureneb aeg, mis on antud shardlet ühenduseta, kuid võib parandada jõudlust.

-    Toimingu ID-d (Tühista): Kui teil on käimas toiming, mis pole enam vaja, saate tühistada toimingu pakkudes selle toimingu ID-d selle välja. Saate tuua tabelist taotluse oleku toimingu ID-d (vt jaotist 8.1) või väljund veebibrauseris, mille saatsite kutse.


## <a name="requirements-and-limitations"></a>Nõuded ja piirangud 

Praeguse rakendamise tükeldatud ühendamine teenus on järgmisi nõudeid ja piirangud. 

* Shards peate olemas ja registreerida Kildu kaardi enne tükeldatud ühendamine toiming, klõpsake nende shards saab teha. 

* Teenust ei loo tabelite või muude andmebaasiobjektidega oma tegevuse käigus automaatselt. See tähendab, et kõik sharded ja viide tabelite skeemi peavad target Kildu enne mis tahes tükeldatud/Ühenda/teisaldamiseks on olemas. Eelkõige peavad sharded tabelite tühi uue shardlets lisada tükeldatud/Ühenda/Teisalda toimingu paiknemise vahemikus. Muul juhul toiming ei õnnestu target Kildu algse vormindusühtsuse kontroll. Samuti võtke arvesse viidet andmed kopeeritakse ainult siis, kui viite tabel on tühi ja et pole järjepidevuse garantiid seoses muude paralleelsete kirjutada viide tabelid. Soovitame seda: kui töötab tükeldatud/ühendamine toiminguid, ei kirjutada toiminguid muuta viide tabelid.

* Teenuse tugineb rea identiteedi loodud kordumatu indeks või võti, mis sisaldab sharding võti ja töökindluse jaoks suure shardlets parandamiseks. See võimaldab liikuda andmed on isegi olevaid kui lihtsalt sharding võtmeväärtuse teenus. See aitab vähendada maksimaalne Logi ruumi ja -lukkude, mida on vaja selle toimingu käigus. Kaaluge võimalust luua kordumatud või kui soovite kasutada tabeli teisaldamine/tükeldatud/Ühenda taotlusi, sh sharding võti antud tabeli primaarvõti. Jõudluse huvides sharding võti peaks olema võti või registri ees veeru.

* Koosolekukutse töötlemine käigus mõned andmed shardlet võib olla nii allika target Kildu. See on vajalik kaitsta tõrkeid shardlet liikumise ajal. Tükeldatud ühendamine Kildu kaart integreerimine tagab ühendused kaudu sõltuvad marsruutimine API-de **OpenConnectionForKey** meetodil kaardil Kildu andmeid ei kuvata kõik vastuolulised kogemused vahe olekus. Ühendamisel allika või target shards ilma **OpenConnectionForKey** meetodil, tabelites sisalduvas vahe olekus võib siiski nähtav kui teisaldamine/tükeldatud/Ühenda taotlused lähevad. Neid seoseid võib kuvada osaline või dubleeritud tulemeid sõltuvalt ajastuse või Kildu, aluseks ühendus. Seda piirangut sisaldab praegu ühendusi, mis on tehtud elastne skaala mitme-Shard-päringuid.

* Ei saa teenuse tükeldatud ühendamine metaandmete andmebaasi ühiskasutusse anda vahel erinevad rollid. Näiteks töötab lavastus tükeldatud ühendamine teenuse osa peab osutage eri metaandmete andmebaasi kui tootmise roll.
 

## <a name="billing"></a>Arveldamine 

Tükeldatud ühendamine teenuse töötab pilveteenuses teie Microsoft Azure'i tellimus. Seetõttu kulude puhul pilveteenuste oma teenuse eksemplarile rakendada. Kui teete sageli teisaldamine/tükeldatud/Ühenda toiminguid, soovitame kustutate oma tükeldatud ühendamine pilveteenuses. Mis salvestab kulud töötab või kasutusele cloud teenuse eksemplari. Saate uuesti juurutada ja alustada oma kergesti runnable konfigureerimine iga kord, kui peate tükeldatud või Ühenda toimingute tegemiseks. 
 
## <a name="monitoring"></a>Jälgimine 
### <a name="status-tables"></a>Oleku tabelid 

Tükeldatud ühendamine teenus pakub **RequestStatus** tabeli metaandmeid talletada andmebaasi lõpule viidud ja kestvat taotluste jälgimiseks. Tabelis on loetletud üht rida iga tükeldatud ühendamine taotlus, mis on esitatud teenuse tükeldatud ühendamine eksemplari. Pakub iga taotlus järgmine teave:

* **Ajatempli**: taotluse käivitamise kuupäeva ja kellaaja.

* **OperationId**: GUID, mis tuvastab kordumatult kutse. Taotluse saate kasutada ka toiming tühistada, kui see on veel pooleli.

* **Olek**: taotluse praegune olek. Poolelioleva taotluste ka loetletud kutse on praeguse etapi.

* **CancelRequest**: lipuga, mis näitab, kas kutse on tühistatud.

* **Edenemise**: protsent hinnangu lõpetamise toiming. 50 väärtuseks näitab, et toiming on 50% valmis.

* **Üksikasjad**: XML-väärtus, mis pakub üksikasjalikuma aruande. Aru värskendatakse regulaarselt target allikas kopeeritakse ridade kogumit. Tõrkeid või erandid korral see veerg sisaldab üksikasjalikumat teavet tõrke kohta.


### <a name="azure-diagnostics"></a>Azure'i diagnostika

Tükeldatud ühendamine teenus kasutab Azure diagnostika diagnostika-ja Azure SDK 2,5 põhjal. Saate määrata diagnostika konfiguratsiooni, nagu on kirjeldatud siin: [Azure'i pilveteenustega ja Virtuaalmasinates diagnostika lubamine](../cloud-services/cloud-services-dotnet-diagnostics.md). Allalaadimine pakett sisaldab kahte diagnostika versiooni – üks web roll ja üks töötaja rolli. Neid diagnostika konfiguratsioone teenuse järgige juhiseid [Teenuse pilveteenuse põhikomponentide, Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). See sisaldab logige jõudluse hinnale, IIS-i logid Windows sündmuselogide ja tükeldatud ühendamine rakenduse sündmuselogide määratlusi. 

## <a name="deploy-diagnostics"></a>Diagnostika juurutamine 

Diagnostika konfiguratsiooni kasutamise web ja töötaja rollid, mis on esitatud Nugeti pakett diagnostika- ja käivitage järgmised käsud Azure PowerShelli kaudu: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Leiate lisateavet ja juurutamine siin diagnostika sätete konfigureerimiseks: [Azure'i pilveteenustega ja Virtuaalmasinates diagnostika lubamine](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Saate tuua diagnostika 

Pääsete hõlpsalt oma diagnostika Visual Studio Server Explorer Server Explorer puu Azure osa. Visual Studio eksemplari ja menüüribal nuppu Vaade ja Server Explorer. Klõpsake ühenduse loomiseks Azure tellimuse Azure ikooni. Liikuge Azure -> salvestusruumi -> <your storage account> -> tabelid -> WADLogsTable. Lisateabe saamiseks vt [Sirvimise salvestusruumi ressursid Server Explorer](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

WADLogsTable, mis on esile tõstetud ülaltoodud joonisel sisaldab üksikasjalikku üritused tükeldatud ühendamine teenuse rakenduse Logi. Pange tähele, et vaikimisi konfiguratsiooni allalaaditud pakett on suunatud tootmise juurutamine. Seetõttu intervall, mille logid ja hinnale on tõmmatakse teenuse eksemplaris on suur (5 minutit). Testi ja arengu, vähendage intervalli, reguleerides diagnostika sätete veebi-või töötaja rolli teie vajadustele. Paremklõpsake roll Visual Studio Server Explorer (vt eespool) ja seejärel kohandada dialoogiboksis diagnostika konfiguratsioon sätte edastamine perioodi: 

![Konfigureerimine][3]


## <a name="performance"></a>Jõudluse

Üldiselt on parema jõudluse oodata suurem, rohkem kiire teenuse astme Azure SQL-andmebaasis. Suurema IO, CPU ja mälu jaotuse kõrgema teenuse astme jaoks kasulik Kopeeri hulgilisamine ja kustutage toimingute kohta, mida kasutab tükeldatud ühendamine service. Seetõttu suurendada teenuse kiht ainult nende andmebaaside jaoks määratletud, piiratud aja jooksul.

Teenuse teostab ka valideerimine päringute oma tavaline toimingud. Need valideerimine päringud ootamatud target vahemiku andmete olemasolu kontrollida ja veenduge, et kõik tükeldatud/Ühenda/teisaldamiseks algab ühtsete olekus. Need kõik päringud töötavad üle sharding võtme vahemike määratletud toiming ulatuse ja paketi suuruse taotluse määratluse osana. Need päringud teha kõige paremini, kui indeks on esitus, mis sisaldab sharding võti ees veeruna. 

Lisaks võimaldab unikaalsuse atribuudi sharding võti ees veeruna teenust kasutada optimeeritud lähenemisviisi, mis piirab ressursside tarbimine Logi ruumi ja mälu. Atribuut unikaalsuse on vaja teisaldada suured andmete suurused (tavaliselt üle 1GB). 

## <a name="how-to-upgrade"></a>Üleminek

1. Järgige [Deploy tükeldatud ühendamine teenus](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Saate muuta oma pilvepõhise teenuse konfiguratsioonifail kajastamiseks uue konfiguratsiooni parameetrid tükeldatud ühendamine juurutamiseks. Uus parameeter on teavet krüptimiseks kasutatavat serti. Lihtne viis selleks on võrrelda uue konfiguratsiooni malli faili allalaadimine oma olemasoleva konfiguratsiooni vastu. Veenduge, et lisate sätete "DataEncryptionPrimaryCertificateThumbprint" ja "DataEncryptionPrimary" veebi-ja töötaja roll.
3. Enne juurutamist Azure värskenduse veenduge, et kõik töötab praegu tükeldatud Ühenda toimingud on lõpule jõudnud. Te saate hõlpsalt selleks päringute tükeldatud ühendamine metaandmete andmebaasi poolelioleva taotluste RequestStatus ja PendingWorkflows tabelid.
4. Värskendage oma olemasoleva pilvepõhise teenuse juurutuse tükeldatud ühendamine teie Azure'i tellimus uue paketi ja värskendatud teenuse konfiguratsiooni fail.

Pole vaja ettevalmistamise metaandmete uue andmebaasi jaoks tükeldatud ühendamine versioonile. Uus versioon on automaatselt uue versiooni olemasoleva metaandmete andmebaasi uuendamine. 

## <a name="best-practices--troubleshooting"></a>Head tavad ja tõrkeotsing
 
-    Määratlege testi rentniku ja kasutada oma kõige olulisemad teisaldamine/tükeldatud/Ühenda toimingute testi rentniku üle mitme shards. Veenduge, et kõik metaandmete õigesti määratletud Kildu kaardi ja, et need toimingud minna vastuollu piiranguid või võõrvõtmed.
-    Säilita testi rentniku andmete maht teie suurim rentniku tagada ilmneb andmete maht ei mahu kuni andmete kohal seotud probleemid. See aitab teil hinnata ülemist aega kulub liigutamiseks ühe rentniku kohta. 
-    Veenduge, et oma skeemi võimaldab kustutamised. Tükeldatud ühendamine teenus nõuab võimalus andmete eemaldamine allika Kildu, kui andmed on edukalt kopeeritud soovitud sihtkohta. Näiteks **kustutamine päästikute** saate takistada teenuse andmete allikas kustutamine ja võib põhjustada toimingute nurjumise.
-    Sharding võti peaks olema ees veeru primaarvõti või kordumatud määratlus. Mis tagab parima jõudluse tükeldatud või Ühenda valideerimine päringuid ja tegelike andmete liikumine ja kustutamise toiminguid, mis toimivad alati sharding võtme vahemike jaoks.
-    Kollokeerida tükeldatud ühendamine teenust piirkonna- ja andmete keskuses, kus asuvad teie andmebaasidest. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 
