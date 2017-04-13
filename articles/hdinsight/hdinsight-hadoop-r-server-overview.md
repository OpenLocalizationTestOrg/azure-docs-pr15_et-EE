<properties
    pageTitle="Mis on R Hdinsightiga kohta? Sissejuhatus R Server (eelvaade) Hdinsightiga | Microsoft Azure'i"
    description="Mis on R serveris Hdinsightiga (eelvaade) ja R Server loomiseks big andmeanalüüsi rakenduste kasutamise kohta."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Ülevaade R Server Hdinsightiga \(eelvaade\)

Microsoft Azure Hdinsightiga Premium, Microsoft R Server on nüüd saadaval valikut, kui loote Azure Hdinsightiga kogumite. Selle uue võimaluse pakub andmeteadlaste, statistikute ja nõudmisel juurdepääsu scalable, R programmeerijad jaotatud meetodite Analytics Hdinsightiga kohta.

Kogumite saate projektide ja ülesannete käepärast ja katki, kui nad ei ole enam vaja. Kuna need on osa Windows Azure Hdinsightiga, nimetatud on ettevõtte tasemel 24/7 tugi, SLA 99,9% sees ja paindlikkust integreerimine Azure ökosüsteemis muud komponendid.

>[AZURE.NOTE] R Server on saadaval ainult Hdinsightiga Premium. Hdinsightiga Premium on praegu saadaval ainult Hadoopi ja säde kogumite jaoks. Jah, praegu saate kasutada R Server ainult koos Hadoopi ja säde kogumite Hdinsightiga kohta. Lisateavet leiate teemast [mis on erinevate teenuse tasemed ja Hadoopi komponendid saadaval Hdinsightiga?](hdinsight-component-versioning.md).

R Server Hdinsightiga pakub uusimate võimaluste R-põhiste Analyticsi suurte andmekogumite, mis laaditakse Azure'i bloobimälu. Kuna R Server on ehitatud avatud allika R, R-põhise taotluste koostate saate kasutada mis tahes avatud allika R pakettide 8000 + kui ka selle praktikaid, mastaabimuundur Microsofti suur andmete analytics paketi, mis on kaasas R Server.

Serva sõlme Premium kogumite pakub on mugav koht ühenduse klaster ning oma R skriptide käitamiseks. Soovitud serva sõlm teil võimalik töötab mastaabimuundur 's parallelized jaotatud funktsioone üle serva sõlm serveri soonte. Teil on olemas ka võimalus käivitada kogu klaster sõlmed abil mastaabimuundur 's Hadoopi kaarti vähendada või säde arvutada kontekstides.

Ennustatud väärtuste, mida tulemi analüüsi saab alla laadida ja mudelite kasutada kohapealse. Need saab ka olla operationalized mujal Azure, nagu on [Azure seadme õ Studio](http://studio.azureml.net) [veebiteenus](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>R Hdinsightiga alustamine

R Server kaasata ka Hdinsightiga kobar, peate looma Hadoopi või säde kobar suvandiga Premium klaster loomisel Azure portaali kaudu. Mõlemat tüüpi kobar kasutada sama konfiguratsiooni, mis sisaldab R Server andmete sõlmed soovitud klaster ning soovitud serva sõlm nimega pealeminekutsooni, R – serveripõhised Analyticsi. Mõne põhjalikumat walk-through klaster loomise kohta vt [R serveris Hdinsightiga töötamise alustamine](hdinsight-hadoop-r-server-get-started.md) .

## <a name="learn-about-data-storage-options"></a>Lisateavet andmete talletamise võimalused

Vaikimisi salvestusruum Hdinsightiga kogumite on vastendatud bloobimälu container HDFS failisüsteemi bloobimälu. See tagab, et mis andmeid on kobar salvestusruumi üles laadida, või kirjutatud kobar salvestusruumi analüüsi käigus tehakse püsiv. Saate kopeerida andmeid ja sealt soovitud bloobimälu [AzCopy](../storage/storage-use-azcopy.md) kasuliku.

Lisaks bloobimälu abil, on teil võimalus kasutades [Azure andmesalv Lake](https://azure.microsoft.com/services/data-lake-store/) klaster. Kui kasutate andmete Lake, siis saate bloobimälu nii andmete Lake HDFS Storage.

Saate kasutada ka [Azure failide](../storage/storage-how-to-use-files-linux.md) salvestusruumi suvand serva sõlme kasutamiseks. Azure'i failide võimaldab teil Failide ühiskasutamine, mis loodi Azure Storage Linuxi failisüsteemi ühendada. Lisateavet andmete talletussuvandite R Server Hdinsightiga kobar kohta teemast [salvestusruumi suvandite Hdinsightiga kogumite R serveris](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Juurdepääs R Server klaster

Kui olete loonud klaster R serveriga, saate luua ühenduse R konsooli serva sõlme klaster SSH/PuTTY kaudu. Samuti saate ühendada brauseri kaudu kui soovite installida valikuline RStudio serveri IDE sõlme serva. RStudio serveri installimise kohta leiate lisateavet teemast [Installimist RStudio serveri Hdinsightiga kogumite](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Arendamise ja R skriptide käitamiseks

R skriptide saate luua ja käivitada võite kasutada mõnda 8000 + Ava allikas R pakette, lisaks parallelized ja jaotatud moodulid mastaabimuundur teek. Üldiselt skripti, mida käitatakse serveris R serva sõlme töötab sees R Tõlgi sõlme. Erandiks on need toimingud, mis on mastaabimuundur kõne funktsioon Arvuta kontekstist see on seatud Hadoopi kaardi vähendamine (RxHadoopMR) või säde (RxSpark).

Sel juhul funktsioon töötab jaotatud mood üle nende andmete (ülesanne) sõlmed klaster, mis on seotud viidatud andmeid. Erinevate Arvuta kontekstis suvandite kohta leiate lisateavet teemast [arvutada kontekstis suvandite R Server Hdinsightiga Premium](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Mudeli kiireks

Kui teie andmete modelleerimine on lõpule jõudnud, te saate kiireks mudeli uute andmete nii Azure'i ja kohapealsete prognoose teha. Selle protsessi tuntakse hinded. Siin on mõned näited.

### <a name="score-in-hdinsight"></a>Hinde Hdinsightiga

Kirjutage koguda Hdinsightiga, R funktsioon, mis nõuab teha prognoose uue andmefaili, mis te olete laaditud kontole salvestusruumi mudelist. Salvestage prognoose uuesti oma konto salvestusruumi. Serva sõlme klaster või ajastatud töö abil saate käivitada soovitud tavalised nõudmisel.  

### <a name="score-in-azure-machine-learning"></a>Hinde Azure seadme õpetused

Keskmine on Azure seadme õ veebiteenuse abil, kasutage avatud allika Azure seadme õ R paketi tuntud [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) avaldada mudelisse Azure web teenust. Mugavuse huvides on see pakett eelinstallitud sõlme serva. Järgmiseks kasutada seadme õppe loomine veebiteenuse kasutajaliides ja seejärel helistage veebiteenuse hinded vajaduse.

Kui valite selle suvandi, peate teisendamiseks võrdväärse avatud lähtekoodi mudeli objektid veebiteenuse jaoks mis tahes mastaabimuundur mudeli objektid. Seda saab teha abil mastaabimuundur kooshoidmine funktsioone, näiteks `as.randomForest()` ansambel vastavalt mudelid.


### <a name="score-on-premises"></a>Kohapealse Keskmine

Keskmine kohapealse mudelisse loomise järel, saate serialiseerida mudeli R, laadige see, tühistage see serialiseerida ja seejärel kasutage hinded uute andmete jaoks. Varem kirjeldatud [Scoring sisse Hdinsightiga](#scoring-in-hdinsight) või [DeployR](https://deployr.revolutionanalytics.com/), saate uute andmete Keskmine.

## <a name="maintain-the-cluster"></a>Klaster haldamine

### <a name="install-and-maintain-r-packages"></a>Installimine ja R pakettide haldamine

Enamik R pakette, mida kasutate peavad serva sõlme Kuna enamik teie R skriptide käivitavad seal. Installige R pakette serva sõlme, võite kasutada tavaline `install.packages()` meetod r.

Enamikul juhtudel ei pea installimiseks R pakette andmete sõlmed, kui te lihtsalt kasutate moodulid mastaabimuundur teegist klaster üle. Siiski, peate võib-olla pakette toetama **rxExec** või **RxDataStep** täitmise kasutamine andmete sõlmed.

Sellisel juhul peab täiendavad pakettide määratud skripti toimingu abil pärast loomist klaster. Lisateabe saamiseks lugege teemat [loomine mõne Hdinsightiga kobar R serveriga](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Hadoopi kaardi vähendada mälu sätete muutmine

Klaster saate muuta, et muuta mälu, mis on saadaval R Server, kui see töötab kaardi vähendada töö. Klaster muutmiseks kasutage Azure portaali höövlitera klaster kaudu saadaolevad Apache Ambari UI. Juhised Ambari Kasutajaliidese jaoks klaster juurdepääsemise kohta leiate teemast [haldamine Hdinsightiga kogumite Ambari Web Kasutajaliidese abil](hdinsight-hadoop-manage-ambari.md).

Samuti on võimalik muuta mälu, mis on saadaval R Server abil Hadoopi parameetrid **RxHadoopMR** kõne järgmiselt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Mastaapimiseks klaster

Mõne olemasoleva kobar saate mastaabitud üles või alla portaali kaudu. Suuruse muutmise, pääsete täiendav läbilaskevõime, mida peate võib-olla suurem töötlemine ülesannete jaoks, või saate skaalal tagasi klaster, kui see on jõudeolekus. Juhised selle kohta, kuidas mastaapimiseks klaster kohta leiate artiklist [haldamine Hdinsightiga kogumite](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Säilitada süsteem

Hooldust tehakse aluseks Linux VM on Hdinsightiga kobar off rakendada OS parandused ja muud värskendused-tunni jooksul. Tavaliselt hooldust kell 3:30 (vastavalt kohalikku aega VM) iga esmaspäev ja neljapäev. Uuendused tehakse nii, et need ei mõju rohkem kui veerand klaster korraga.  

Kuna pea sõlmed on üleliigsed ja mõjutab kõiki andmeid sõlmed, aeglustada mis tahes tööd, mis töötavad selle aja jooksul. Nad peaksid siiski käivitada valmimiseni, kuid. Kohandatud tarkvara või kohalikud andmed, mida teil on üle need sündmused hoolduse säilitada kui katastroofiline tõrge ilmneb, mis nõuab kobar taastada.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Lisateavet IDE suvandid R server on Hdinsightiga kobar

Linux serva sõlme mõne Hdinsightiga Premium kobar on pealeminekutsooni R-põhise analüüsi jaoks. Pärast ühenduse klaster, võite käivitada, tippides käsureale Linux **R** konsooli kasutajaliidese R-serveris. Konsooli kasutajaliidese kasutamine on suurem kui käivitada mõnes muus aknas, R skripti arengu tekstiredaktoris ja lõikamisel ja kleepimisel jaotiste oma skripti R konsooli vastavalt vajadusele.

R skripti arendamiseks keerukamaid tööriista on kasutamiseks töölaual, R-põhiste IDE Microsofti viimati teada [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS). See on pere [RStudio](https://www.rstudio.com/products/rstudio-server/)server ja tööriistu. Samuti saate Walware's Eclipse-põhise [StatET](http://www.walware.de/goto/statet).

Teine võimalus on mõne IDE installida Linux serva sõlme ise.  Valik on [RStudio Server](https://www.rstudio.com/products/rstudio-server/), mis pakub Brauseripõhine IDE remote klientidele kasutamiseks. Ja täitmise R skriptide R serveriga klaster RStudio serveri installimise serva sõlme mõne Hdinsightiga Premium kobar täielik IDE kogemus pakub. See võib oluliselt veel tõhusamalt töötada kui R konsool.  Kui soovite kasutada RStudio Server, lugege teemat [Installimist RStudio serveri Hdinsightiga kogumite](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Lisateavet hinnad

Tasud, mis on seostatud mõne Hdinsightiga Premium kobar R serveriga on struktureeritud sarnaselt kohta standard Hdinsightiga rühmad. Need põhinevad suurus aluseks VMs nimi, andmed ja serva sõlmed, lisaks core-tunnise tõusu Premiumi üle. Hdinsightiga Premium kohta lisateavet hinnad, sh avaliku eelvaate ja 30-päevane tasuta prooviversioon kättesaadavus hinnad leiate [Hdinsightiga hinnad](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Järgmised sammud

Lugeda Lisateavet allolevate linkide kohta, kuidas kasutada R Server Hdinsightiga kogumite.

- [R Server Hdinsightiga töötamise alustamine](hdinsight-hadoop-r-server-get-started.md)

- [Hdinsightiga Premium RStudio serveri lisamine](hdinsight-hadoop-r-server-install-r-studio.md)

- [Arvutage kontekstis suvandid R Server Hdinsightiga (eelvaade)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure'i talletamise võimalused R Server Hdinsightiga Premium](hdinsight-hadoop-r-server-storage.md)
