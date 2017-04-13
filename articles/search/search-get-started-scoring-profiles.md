<properties 
    pageTitle="Kasutamise hinded profiilid Azure'i otsingus | Microsoft Azure'i | Majutatud pilveteenuses otsing" 
    description="Otsingu kaudu hinded profiilid majutatud cloud otsinguteenuse Microsoft Azure Azure'i otsingus järjestamine häälestada." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Azure'i Otsi profiilid hinded kasutamine

Funktsioon Microsoft Azure'i otsing, mis kohandamine arvutamine otsingu hinded, mõjutades seda, kuidas üksused on järjestatud otsingutulemite loendis on hinded. Mõelge hinded profiilid mudeli asjakohasuse, et abil suurendada eelmääratletud kriteeriumidele vastavad üksused. Oletame näiteks, et teie taotlus on veebis teha broneerimiseks sait. Suurendada, on `location` välja, otsinguid, mis sisaldavad termin nagu Seattle tulemuseks kõrgemad hinded üksuste jaoks, mis on Seattle on `location` välja. Kui vaikimisi hinded piisab rakenduse, Pange tähele, et saaksite hinded rohkem kui üks profiil või pole üldse.

Selleks et saaksite hinded profiilid katsetada, saate alla laadida valimi rakendus, mida kasutab hinded profiilid otsingutulemite rank järjestuse muutmine. Valim on konsooli rakendus – ei pruugi väga realistlik tegelike rakenduste arendamise – kuid kasulik siiski õ tööriista. 

Proovi taotluse näitab hinded käitumist kasutada väljamõeldud andmeid, mida nimetatakse funktsiooni `musicstoreindex`. Lihtsad valimi rakendus on lihtne muuta hinded profiilid ja päringute ja vaadake vahetu mõju rank järjestuses, kui programm.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Eeltingimused

Proovi taotluse kirjutatakse C# Visual Studio 2013. Kui teil pole veel Visual Studio koopia, proovige tasuta [Visual Studio 2013 Express edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) .

Peate tellimuse Azure ja õpetuse lõpuleviimiseks Azure'i otsinguteenuse. Leiate abi teenuse häälestamise [otsinguteenuse portaali loomine](search-create-service-portal.md) .

[AZURE'I. KAASA [peate Azure'i konto lõpuleviimiseks õppeteema:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>Laadige rakendus näidis

Avage [Azure'i otsingu hinded profiilid Demo](https://azuresearchscoringprofiles.codeplex.com/) codeplexis kirjeldatud selles õpetuses valimi rakenduse allalaadimiseks.

Klõpsake menüü lähtekoodi, **Laadige alla** zip-fail lahenduse saamiseks. 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>App.config redigeerimine

1. Pärast failid ekstraktida, avage lahenduse Visual Studios konfiguratsiooni faili redigeerimiseks.
1. Topeltklõpsake Solution Exploreris **app.config**. Saate määrata selle faili teenuse lõpp-punkti ja `api-key` kasutatud taotluse kinnitamiseks. Need väärtused saate hankida klassikaline portaalist.
1. [Azure'i portaali](https://portal.azure.com)sisse logima.
1. Avage teenus armatuurlaua Azure'i otsingu jaoks.
1. Klõpsake paani **Atribuudid** teenuse URL-i kopeerimine
1. Klõpsake paani kopeerimiseks **klahve** soovitud `api-key`.

Kui olete lõpetanud, lisades URL-i ja `api-key` app.config, et rakenduse sätted peaks välja nägema umbes järgmine:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Tutvumine rakenduse

Saate luua ja käivitage rakendus peaaegu märkimist, kuid enne, kui te ei tee, tutvuge JSON faile, mida kasutatakse luua ja määrata registri.

**Schema.JSON** määratleb registri, sh selle demo hinded profiilid, mis on rõhutada. Teate, et skeem, mis määratleb kõik väljad, mida kasutatakse registri, sealhulgas otsitavate väljad, nagu `margin`, mille abil saate hinded profiili. Hinded profiili süntaks on dokumenteerida [lisamine Azure'i otsingu registri hinded profiili](http://msdn.microsoft.com/library/azure/dn798928.aspx).

**Data1-3.json** sisaldab andmeid, 246 albumit žanrid üksikud üle. Andmed on tegelik albumi ja artistide teave, näiteks väljamõeldud väljadega kombinatsiooni `price` ja `margin` otsing toimingute abil. Andmefailid ja vastaksid registri laaditakse teie Azure'i otsinguteenuse. Kui andmed on üles laaditud ja indekseeritud, saate väljastada päringute vastu.

**Program.cs** teostab järgmisi toiminguid:

- Avab akna konsooli.

- Loob ühenduse teenuse URL-i Azure otsingu ja `api-key`.

- Kustutab selle `musicstoreindex` kui see on olemas.

- Loob uue `musicstoreindex` schema.json-faili abil.

- Kuvab registri abil andmefailid.

- Päringute abil neli päringute registri. Pange tähele, et hinded profiilid on määratud Päringuparameetri. Kõigi päringute otsida sama termin "parem". Esimene päring näitab vaikimisi hinded. Ülejäänud kolme päringute kasutamine hinded profiili.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Koostamine ja käivitage rakendus

Ühenduvus või komplekti viide probleemide välistamiseks koostamine ja käivitage rakendus tagamiseks on esmalt välja töötada. Peaksite nägema konsooli rakendus, avage taustal. Kõik neli päringud käivitada ilma pausi jada. Paljude numbritelt, mis käivitab terve programm 15-sekundit. Kui konsooli rakendus sisaldab teade "lõpuleviimine. Vajutage klahvi enter jätkamiseks", programmi, mis on lõpule viidud. 

Päring käivitatakse võrdlemiseks saate märgi, kopeerimine ja kleepimine päringutulemite konsooli ja kleepige need Exceli faili. 

Järgmisel joonisel on esimene kolme päringud – kõrvuti tulemusi. Kõik päringud kasutada sama otsingutermin, kõige, mis kuvatakse mitme albumi tiitlid.

   ![][10]

Esimene päring kasutab vaikimisi hinded. Kuna otsingusõna kuvatakse ainult albumi pealkirjad ja muid kriteeriume pole määratud, tagastatakse üksused, millel "parem" albumi otsinguteenus leiab neid järjestuses. 

Teise päringu kasutab hinded profiili, kuid teade, et profiili oli mingit mõju. Identne esimese päringu tulemused. See on hinded profiili suurendab välja (Žanr"), mis pole sobiv päring. Otsingusõna "parim" pole olemas "Žanr" välja mis tahes dokumendi. Kui hinded profiili on mingit mõju, tulemused on sama, mis vaikimisi hinded.  

Kolmanda päring on selle esimese profiil mõju hinded. Otsingusõna on endiselt parim nii, et sama valiku albumit töötavad, kuid kuna hinded profiil pakub täiendavaid kriteeriume, mis suurendab "reiting" ja "Viimati värskendatud", mõned üksused on liikuma kõrgem loendis.

Järgmisel joonisel on neljas ja lõplik päring "bruto" toetas. Väli "bruto"-otsitavate ja ei kuvata otsingutulemustes. Arvutustabeli aidata illustreerivad punkt, et üksused, mille kõrgemad marginaalid kuvatakse otsingutulemite loendis kõrgema käsitsi lisatud "bruto" väärtus. 

   ![][9]

Nüüd, kui teil on proovinud hinded profiilid, proovige muuta programmi kasutada erinevaid päringu süntaksit, hinded profiilid või rikkalikumat andmeid. Järgmises jaotises lingid pakuvad lisateavet.

<a id="next-steps"></a>
## <a name="next-steps"></a>Järgmised sammud

Lisateavet profiilide hinded. Üksikasjad leiate [Azure'i otsingu registri hinded profiili lisamine](http://msdn.microsoft.com/library/azure/dn798928.aspx) .

Lisateavet otsingu valemisüntaksit ja päringu parameetrid. Üksikasjad leiate [Otsingu dokumendid (Azure'i Search REST API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Tuleb sammu tagasi ja Lisateavet registri loomise kohta? [Vaadake seda videot](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) mõista põhitõdesid.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 