<properties
    pageTitle="Jälgimine, diagnoosimine ja salvestusruumi tõrkeotsing | Microsoft Azure'i"
    description="Kasutada funktsioone, näiteks salvestusruumi analytics, kliendipoolne logimine ja muude tootjate tööriistad tuvastamiseks, diagnoosimine ja Azure Storage seotud probleemide tõrkeotsing."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Jälgimine, diagnoosimine ja tõrkeotsing Microsoft Azure'i Tabelimäluga

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Ülevaade

Diagnoosimise ja tõrkeotsingu cloud keskkonnas majutatud jaotatud rakendus võib olla märksa keerukam kui traditsiooniline keskkonnas. Rakenduste saab kasutada PaaS või IaaS taristu kohapealne mobiilsideseadmes või nende kombinatsioonist. Tavaliselt rakenduse võrguliiklust võib läbida avalike ja privaatvõ võrgu ja rakenduse võib kasutada mitut salvestusruumi tehnoloogiaid, näiteks Microsoft Azure'i salvestusruumi tabelid, plekid, järjekorrad või failide muudele andmetele Lisaks talletab näiteks nii relatsiooniline ja dokumendi andmebaasid.

Selliste rakenduste haldamiseks edukalt peaks jälgida neid aktiivselt ja aru saada, kuidas diagnoosimine ja kõigis neid ja nende sõltuvad tehnoloogiad tõrkeotsing. Azure Storage teenuste kasutajaks, peaksite pidevalt jälgimine salvestusruumi teenuseid teie rakendus kasutab ootamatud muudatused rakenduses (nt aeglasem kui tavaline vastuse korda) ja kasutada logimine üksikasjalikumat andmete kogumine ja probleem sügavuti analüüsida. Nii jälgida ja logida saada diagnostika teave aitab teil määrata oma rakenduses ilmnes probleem põhjuseks. Seejärel saate probleemi tõrkeotsinguks ja kindlaks teha, mida ette võtta migreerimisel ning probleemide lahendamisel see vajalikud. Azure'i salvestusruumi on tuum Azure'i teenus ja lahendusi, mis kliendid võtta kasutusele Azure'i infrastruktuuri enamiku oluline osa. Azure'i salvestusruumi sisaldab võimaluste lihtsustamiseks jälgimine, diagnoosimise ja rakenduste pilvepõhist salvestusruumi probleemide tõrkeotsing.

> [AZURE.NOTE] Salvestusruumi kontod dispersioonanalüüs tüüpi Zone liigsete mälu (ZRS) pole mõõdikute või logimise võimalus praegu lubatud. Ka ei toeta failide Azure'i teenus logimine sel ajal.

Lõpuni tõrkeotsingu Azure Storage rakendustes praktilise juhendi leiate teemast [End-to-End tõrkeotsingu Azure'i talletusruumi mõõdikute ja logimine, AzCopy, ja sõnumi Analyzer abil](storage-e2e-troubleshooting.md).

+ [Sissejuhatus]
    + [Sellest juhendist korraldamine]
+ [Teie salvestusteenus jälgimine]
    + [Teenuse seisundi jälgimine]
    + [Võimsus jälgimine]
    + [Kättesaadavus jälgimine]
    + [Jõudluse jälgimine]
+ [Diagnoosimise salvestusruumi probleemid]
    + [Teenuse seisundi probleeme.]
    + [Jõudlusega seotud probleemide]
    + [Diagnoosimise tõrked]
    + [Salvestusruumi emulaator probleemid]
    + [Salvestusruumi logimine tööriistad]
    + [Võrgu logimine tööriista abil]
+ [Lõpuni jälgimine]
    + [Arvega logiandmed]
    + [Kliendiid taotlus]
    + [Serveri taotluse ID]
    + [Ajatemplid]
+ [Juhised tõrkeotsing]
    + [Mõõdikute AverageE2ELatency kõrge ja madala AverageServerLatency kuvamine]
    + [Mõõdikute kuvada madala AverageE2ELatency ja madala AverageServerLatency, kuid kliendi esineb pika latentsusajaga]
    + [Mõõdikute kuvamine kõrge AverageServerLatency]
    + [Teil on probleeme ootamatu viivitust sõnumi kohaletoimetamise järjekord]
    + [Mõõdikute kuvamiseks PercentThrottlingError suurendamine]
    + [Mõõdikute kuvamiseks PercentTimeoutError suurendamine]
    + [Mõõdikute kuvamiseks PercentNetworkError suurendamine]
    + [Kliendi on sõnumeid HTTP 403 (keelatud)]
    + [Kliendi on sõnumeid HTTP 404 (ei leitud)]
    + [Kliendi on sõnumeid HTTP 409 (konflikt)]
    + [Mõõdikute kuvamine madal PercentSuccess või analytics Logi kirjed on tegevuse ClientOtherErrors koos olek]
    + [Võimsus mõõdikute kuvada ootamatud suurendada mäluruumi võimsus kasutamine]
    + [Teil on probleeme, on suur hulk manustatud VHDs ootamatut taaskäivitamisega]
    + [Teie probleemi tuleneb abil salvestusruumi emulaator või test]
    + [Ilmneb probleeme Azure'i SDK .net-i jaoks]
    + [Teil on erinev küsimus salvestusruumi teenuse]
    + [Windows ja Linux Azure'i failide probleemide tõrkeotsing](storage-troubleshoot-file-connection-problems.md)
+ [Lisa]
    + [Lisa 1: Viiuldaja abil jäädvustada HTTP ja HTTPS-liikluse]
    + [Lisa 2: Abil Wiresharkis võrguliiklust jäädvustada]
    + [Lisa 3: Microsoft sõnumi Analyzer abil võrguliiklust jäädvustada]
    + [Lisa 4: Exceli kasutamise mõõdikute vaatamiseks ja logige andmed]
    + [Lisa 5: Jälgimise rakenduse ülevaated Visual Studio meeskonnatöö teenuste jaoks]

## <a name="introduction"></a>Sissejuhatus

Kliendipoolne logimine Azure'i salvestusruumi kliendi teek ja muude tootjate tööriistad tuvastada, diagnoosimine ja Azure Storage tõrkeotsing juhendi näitab, kuidas kasutada funktsioone nagu Azure'i salvestusruumi Analytics, seotud probleemid.

![][1]

*Joonis 1 jälgimine, diagnostika, ja tõrkeotsing*

Käesolev juhend on mõeldud eelkõige tarkvaraarendajad võrguteenuste, mis kasutavad Azure salvestusruumi teenuste ja selle spetsialistidele nende teenuste kaudu haldamise eest vastutav lugeda. Sellest juhendist eesmärgid on:

- Et saaksite säilitada tervise ja jõudluse Azure Storage konto.
- Teile vajalikud protsessid ja tööriistu, mis aitavad teil otsustada, kui küsimust või probleemi rakenduses seotud Azure Storage.
- Teile vaidlustatavad juhised Azure Storage seotud probleemide lahendamine.

### <a name="how-this-guide-is-organized"></a>Sellest juhendist korraldamine

Jaotises "[jälgida oma salvestusteenus]" kirjeldatakse, kuidas jälgida seisundi ja jõudluse Azure Storage teenuste kasutamine Azure talletusruumi Analytics mõõdikute (talletusruumi mõõdikute).

Jaotises "[diagnoosimine probleeme]" kirjeldatakse probleemide Azure'i salvestusruumi Analytics logimine (salvestusruumi logimine) abil. Samuti kirjeldatakse kliendipoolne logimine abil vahendid ühes teegis kliendi salvestusruumi kliendi teek .net-i või Azure SDK Java lubamise kohta.

Jaotises "[-lõpuni jälgimine]" kirjeldatakse, kuidas saate oleksid erinevates logifailid ja mõõdikute andmete sisalduvat teavet.

Jaotises "[tõrkeotsingu juhiseid]" antakse ühise salvestusruumi seotud probleemid, mis võivad ilmneda mõned juhised tõrkeotsing.

"[Liites]" sisaldavad teavet muude tööriistade abil nagu Wiresharkis ja Netmoni viiuldaja analüüsimiseks HTTP-või HTTPS sõnumite paketi võrguandmete analüüsimiseks ja Microsoft sõnumi analüsaator tõestusmeetodid logige andmed.


## <a name="monitoring-your-storage-service"></a>Teie salvestusteenus jälgimine

Kui olete tuttav Windowsi jõudluse jälgimine, te mõtlete talletusruumi mõõdikute on Azure Storage samaväärsed Windowsi jõudlus kuvari hinnale. Talletusruumi mõõdikute leiate täielik kogum, nt teenuse kättesaadavus teenuse taotluste koguarvu või teenusesse õnnestunud taotluste protsent mõõdikute (Windowsi jõudlus kuvari terminoloogia hinnale). Saadaval mõõdikute täieliku loendi leiate teemast [Salvestusruumi Analytics mõõdikute tabeli skeemi](http://msdn.microsoft.com/library/azure/hh343264.aspx). Saate määrata, kas soovite salvestusteenus, koguda ja liitmine mõõdikute tunnis või iga minut. Lubamine mõõdikute ja jälgida salvestusruumi kontode kohta leiate lisateavet teemast [talletusruumi mõõdikute lubamine ja mõõdikute andmete vaatamine](http://go.microsoft.com/fwlink/?LinkId=510865).

Saate valida, millised kord tunnis mõõdikute soovite kuvada [Azure portaali](https://portal.azure.com) ja konfigureerida reeglid teavitavad administraatorid, e-posti iga kord, kui mõne tunni meetermõõdustik ületab teatud piiri. Lisateavet leiate teemast [Teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). 

Salvestusruumi teenuse mõõdikute parim võimalik abil kogutud, kuid võib salvestada iga Salvestustoiming.

Azure'i portaalis, saate vaadata mõõdikute, nt kättesaadavus, kokku taotlused ja Keskmine latentsusnäitajaid salvestusruumi konto. Teatise reegel on ka seadistatud administraator hoiatada, kui kättesaadavus langeb alla teatud taseme. Vaatamist andmed, üks võimalik ala uurimiseks on tabeli teenuse õnnestumise protsent on väiksem kui 100% (Lisateabe saamiseks vt jaotist "[mõõdikute kuvamine madal PercentSuccess või analytics Logi kirjed on kande olek ClientOtherErrors toimingute]").

Pidevalt tuleks jälgida oma Azure rakendused, et tagada, et terve ja jõudluse, nii:

- Mõned võrdlusalus mõõdikute rakendus, mis võimaldab teil võrrelda olemasolevad andmed ja leida olulisi muudatusi Azure salvestusruumi ja rakenduse loomine. Oma võrdlusalus mõõdikute väärtuste saab paljudel juhtudel konkreetne rakendus ja neid tuleks luua, kui olete rakenduse testimine jõudluse.
- Salvestamise minute mõõdikute ja nende abil jälgida aktiivselt ootamatud vead ja kõrvalekaldeid nagu diagrammi tõrge loendab või taotleda määra.
- Kord tunnis mõõdikute salvestamise ja nende abil jälgida keskmised väärtused nagu Keskmine tõrge loendab ja taotleda määra.
- Diagnostika tööriistadega nagu hiljem jaotises "[diagnoosimine probleeme]." võimalike probleemide uurimine

Diagrammide joonisel 3 esitatud illustreerivad, kuidas keskmistamine, mis ilmneb kord tunnis mõõdikute saate peita diagrammi alusel. Kord tunnis mõõdikute kuvamiseks ühtlase määra taotlused, samal ajal mõõdikute ilmneb väga toimuvad kõikumised minutid kuvatakse.

![][3]

Ülejäänud selles jaotises kirjeldatakse, mida mõõdikud ei peaks jälgida ja miks.

### <a name="monitoring-service-health"></a>Teenuse seisundi jälgimine

[Azure portaali](https://portal.azure.com) abil saate vaadata seisund teenuse salvestusruumi (ja muud Azure teenused) kõikides piirkondades Azure kogu maailmas. See võimaldab teil näha kohe, kui väljaspool teie kontrolli probleemi mõjutab salvestusteenus kasutate rakenduse piirkonnas.

[Azure portaali](https://portal.azure.com) võivad pakkuda, mis mõjutavad Azure'i teenuste teatised.
Märkus: See teave on varem saadaval koos [Azure'i teenus armatuurlaua](http://status.azure.com)ajalooliste andmed.

Ajal [Azure portaali](https://portal.azure.com) kogub süsteemiseisundi teavet sees Azure andmekeskuste, mis on (sees out jälgimise), samuti võiksite kaaluda väljaspool lisandmooduli lähenemisviisi sünteetiline tehingute, mis perioodiliselt mitmest kohast juurde Azure'i majutatud veebirakenduse loomiseks. Visual Studio meeskonnatöö teenuste [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) ja rakenduse ülevaated pakutavaid teenuseid näited sellest väljaspool moodust. Lisateavet rakenduse ülevaated Visual Studio Team Services vt. "[Lisa 5: rakenduse ülevaated Visual Studio meeskonnatöö teenuste jälgimise](#appendix-5)."

### <a name="monitoring-capacity"></a>Võimsus jälgimine

Talletusruumi mõõdikute ainult talletab võimsus mõõdikute bloobimälu teenuse, kuna plekid tavaliselt konto talletatud andmed suurima osa (kirjutamise ajal, ei ole võimalik talletusruumi mõõdikute abil saate jälgida oma tabelite ja järjekorrad). Leiate andmed tabelis **$MetricsCapacityBlob** kui teil on lubatud teenuse bloobimälu jälgimine. Talletusruumi mõõdikute kirjete andmed üks kord päevas ja **RowKey** väärtuse abil saate määratleda, kas rida sisaldab ettevõte, mis on seotud kasutaja andmed (väärtuse **andmed**) või analytics andmete (väärtuse **analytics**). Iga talletatud üksus sisaldab teavet talletusmahtu kasutatud (**võimsus** mõõdetud baiti) ja praeguse arv ümbriste (**ContainerCount**) ja plekid (**ObjectCount**) kasutab salvestusruumi konto. Salvestatud **$MetricsCapacityBlob** tabeli võimsus mõõdikud kohta leiate lisateavet teemast [Salvestusruumi Analytics mõõdikute tabeli skeemi](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [AZURE.NOTE] Tuleks jälgida, et teil on lähenemas võimsuse piiranguid konto salvestusruumi varajase hoiatuse need väärtused. Azure'i portaalis, saate lisada teatis reeglid teavitavad teid, kui liitväärtuse mäluruumi kasutamine suurem või teie määratud lävede all.

Erinevate salvestusruumi objektide, näiteks plekid suuruse hindamisel abi saamiseks leiate ajaveebipostitusest [Mõistmine Azure'i salvestusruumi arveldus-ribalaiuse, tehingute ja võimsus](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Kättesaadavus jälgimine

Peaks teie salvestusruumi konto salvestusruumi teenuste jälgima kord tunnis või minute mõõdikute tabeli veerus **kättesaadavus** väärtus jälgides – **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. **Kättesaadavus** veerg sisaldab protsenti väärtus, mis näitab, et teenus või rida ( **RowKey** näitab kui rida sisaldab kogu teenuse või konkreetse API toimingu mõõdikute) tähistatud API tegevus olemasolu.

Mis tahes väärtus väiksem kui 100% näitab, et mõned taotluste ei suuda. Saate vaadata, miks nad ei suuda uurides muude veergude kuvamine taotlusi, nt **ServerTimeoutError**erineva tõrke andmetüüpidega arvud mõõdikute andmeid. Peaksite **kättesaadavus** madalamale ajutiselt 100% põhjustel, näiteks ajutine serveri ajalõpud ajal teenuse viib sektsioonid parem laadi – Topeltdegressiivse taotluse; Proovi uuesti loogika klientrakenduse peaks hakkama vahelduva tingimustel. Artikli [Analytics sisse logitud ja Status Messages](http://msdn.microsoft.com/library/azure/hh343260.aspx) loetleb talletusruumi mõõdikute arvutamise **kättesaadavus** sisaldava tehingu tüübid.

[Azure portaali](https://portal.azure.com), saate lisada teatis reeglite märku, kui teie määratud piirmäärast teenuse **kättesaadavus** .

Selle juhendi jaotises "[tõrkeotsingu juhiseid]" kirjeldatakse salvestusruumi teenuse levinumad seotud kättesaadavus.

### <a name="monitoring-performance"></a>Jõudluse jälgimine

Salvestusruumi osutamise jälgida, saate järgmine kord tunnis ja minute mõõdikute tabelite mõõdikud.

- Veergude **AverageE2ELatency** ja **AverageServerLatency** väärtuste keskmine aja kuvamine salvestusruumi teenuse või API toimingu tüüp võtab aega Töötle taotlusi. **AverageE2ELatency** on-lõpuni latentsus, mis sisaldab taotluse lugeda ja saata vastuse lisaks taotluse kuluva aja (seetõttu sisaldab Võrgu latentsuse taotluse ulatub salvestusruumi teenus); **AverageServerLatency** lihtsalt töötlemise ajal mõõt ja seetõttu ei hõlma mis tahes seotud suhtlemine kliendi võrgu latentsus. Leiate jaotisest "[mõõdikute kuvada AverageE2ELatency kõrge ja madala AverageServerLatency]" hiljem sisse juhendi arutelu, miks võib olla oluline erinevus neist kahest väärtusest.
- Väärtused veergudest **TotalIngress** ja **TotalEgress** Kuva andmete kogusumma baitides tulevad ja teie salvestusruumi teenusest või teatud tüüpi API toiming kaudu.
- **TotalRequests** veeru väärtused kuvada taotlusi, mis võtab vastu salvestusteenus API toimingu koguarv. **TotalRequests** on nõuab, et teenus salvestusruumi saab koguarv.

Tavaliselt jälgite ootamatuid muutusi need väärtused näitaja, et teil on küsimus, mis nõuab uurimine.

[Azure portaali](https://portal.azure.com), saate lisada teatis reeglid märku kui mis tahes jõudluse mõõdikute selle teenuse sügis all või ületab läve, mis teie määratud.

Selle juhendi jaotises "[tõrkeotsingu juhiseid]" kirjeldatakse salvestusruumi teenuse levinumad jõudlusega seotud.


## <a name="diagnosing-storage-issues"></a>Diagnoosimise salvestusruumi probleemid

Mitmesuguseid võimalusi, et probleem või probleem võib muutuda oma rakenduse, sealhulgas:

- Suurema tõrke põhjustab rakenduse krahhi või lõpetada töötamise.
- Alusjoone väärtuste mõõdikud märgatavat muudatused on jälgimise "[jälgida oma salvestusteenus]." eelmises jaotises kirjeldatud
- Aruannete kasutajad rakenduse, et mõne kindla toiming ei täitnud ootuspäraselt või mõni funktsioon ei tööta.
- Teie rakenduses loodud vigu, mis kuvatakse logifailid või mõne muu meetodi teatise kaudu.

Tavaliselt kuuluvad Azure storage teenustega seotud probleemid ühte neli põhilist ülesandekategooriat:

- Teie taotlus on esitatud kasutajate või ilmnenud muutuste jõudlust mõõdikute JÕUDLUSPROBLEEM.
- On probleem Azure Storage taristu ühes või mitmes piirkonnas.
- Rakenduse on tekib tõrge, kas kasutajate poolt, või ilmnes tõrge count mõõdikute saate jälgida ühes suurendada.
- Arendamise ja testi, võib kasutada kohalikku emulaator; mõned probleemid, mis on seotud konkreetselt salvestusruumi emulaator kasutamist võivad ilmneda.

Järgmistes jaotistes liigendamine peaksite järgima juhiseid diagnoosimine ja nende nelja kategooria probleemide tõrkeotsing. Olevat jaotist "[tõrkeotsingu juhiseid]" juhendi leiate rohkem üksikasju, mis võivad ilmneda levinumate probleemide.

### <a name="service-health-issues"></a>Teenuse seisundi probleeme.

Teenuse seisundi probleeme on tavaliselt väljaspool teie kontrolli. [Azure portaali](https://portal.azure.com) annab poolelioleva seotud probleemide kohta teabe Azure'i teenuste, sealhulgas salvestusruumi. Kui olete valinud lugemisõigus – geograafilise liigne salvestusruumi salvestusruumi konto loomisel, kui teie andmed on saadaval esmane asukoht, rakenduse võib aktiveerige ajutiselt kirjutuskaitstud eksemplari teisene asukohta. Selleks rakenduse peavad saama vaheldumisi asukohtadega esmaseid ja teiseseid salvestusruumi ja vähendatud funktsionaalsusega režiimis kirjutuskaitstud andmetega töötada. Azure'i salvestusruumi kliendi teekide abil saate määratleda uuesti poliitika, mille saate lugeda teisene salvestusruumi juhul ei loe esmane salvestusruumist. Rakenduse peab olema teadlik, et andmed teisel asukoha lõpuks ühtsete. Lisateabe saamiseks leiate [Azure'i talletussuvandite koondamise ja lugemisõigus Geo liigsete salvestusruumi](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)ajaveebipostitusest.

### <a name="performance-issues"></a>Jõudlusega seotud probleemide

Rakenduse jõudlus võib olla subjektiivne, eriti kasutaja seisukohast. Seetõttu on oluline on võrdlusalus mõõdikute saadaval, mis aitavad teil kindlaks teha, kui on olemas jõudlusprobleemi põhjus. Palju tegureid võib mõjutada jõudlust seisukohalt kliendi rakenduse teenuse Azure salvestusruumi. Teguritest toimiks salvestusruumi teenus, kliendi või võrgu taristule; Seetõttu on oluline on strateegia jõudlusprobleemi päritolu määratlemiseks.

Asukoht: mõõdikud jõudlusprobleemi põhjus tõenäoliselt olete kindlaks teinud, siis saate logifailide diagnoosimine ja probleemi täpsemaks tõrkeotsinguks üksikasjalikku teavet leida.

Jaotises "[tõrkeotsingu juhiseid]" allpool olevat sellest juhendist leiate lisateavet mõne levinud jõudlusega seotud probleemide saate võivad ilmneda.

### <a name="diagnosing-errors"></a>Diagnoosimise tõrked

Rakenduse kasutajad võivad märku tõrkeid, klientrakendust. Talletusruumi mõõdikute kirjete ka loendab erineva tõrke tüüpi salvestusruumi teenustepakkujate, nt **NetworkError**, **ClientTimeoutError**või **AuthorizationError**. Ajal talletusruumi mõõdikute kirjeid ainult loendab erineva tõrke tüüpi, saate hankida rohkem üksikasju üksikuid taotlusi kontrollides serveripoolne, kliendipoolne ja võrgu logid. Tavaliselt on tagastatud salvestusruumi teenuse HTTP olekukoodi annavad aimu taotluse nurjumise põhjust.

> [AZURE.NOTE] Pidage meeles, et peaksite kuvamiseks vahelduva vigu: näiteks tõrgete tõttu siirdamiseks võrgu läbilaskevõime või rakenduse tõrked.

Abiks mõistmine salvestusruumi seotud olek ja viga koodid on järgmised materjalid:

- [Levinud REST API kuvatavad tõrkekoodid](http://msdn.microsoft.com/library/azure/dd179357.aspx)
- [Tõrkekoodid bloobimälu teenus](http://msdn.microsoft.com/library/azure/dd179439.aspx)
- [Järjekorda teenuse kuvatavad tõrkekoodid](http://msdn.microsoft.com/library/azure/dd179446.aspx)
- [Tabeli teenuse kuvatavad tõrkekoodid](http://msdn.microsoft.com/library/azure/dd179438.aspx)
- [Faili teenuse kuvatavad tõrkekoodid](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Salvestusruumi emulaator probleemid

Azure'i SDK sisaldab salvestusruumi emulaator, arengu töökoha käivitada. See emulaator jäljendab enamik Azure storage teenuste toimimist ja on kasulik ajal Arendus ja testimine, mis võimaldab teil käivitada rakendusi, mis kasutavad Azure storage teenused ilma vajaduseta Azure tellimuse ja Azure storage konto.

Jaotises "[tõrkeotsingu juhiseid]" juhendi kirjeldatakse levinumate probleemide ilmnes salvestusruumi emulaator abil.

### <a name="storage-logging-tools"></a>Salvestusruumi logimine tööriistad

Salvestusruumi logimine pakub serveripoolne logimine salvestusruumi taotluste Azure storage konto. Lisateavet selle kohta, kuidas lubada serveripoolne logimine ja Logi andmetele juurdepääsemiseks teemast [salvestusruumi logimise lubamine ja juurdepääs logiandmed](http://go.microsoft.com/fwlink/?LinkId=510867).

.Net-i salvestusruumi kliendi teek võimaldab teil koguda kliendipoolne log andmeid, mis on seotud rakenduse salvestusruumi toimingute. Lisateabe saamiseks vt [kliendipoolne logimine teegiga .NET salvestusruumi kliendi](http://go.microsoft.com/fwlink/?LinkId=510868).

> [AZURE.NOTE] Teatud juhtudel (nt SAS autoriseerimine tõrked), võib kasutaja aruande tõrge, mille leiate taotluse andmed serveripoolne salvestusruumi logid. Saate uurida, kui probleemi põhjuseks on kliendi salvestusruumi kliendi teek logimine võimaluste kasutamiseks või uurige võrgu võrgu jälgimise tööriistade abil.

### <a name="using-network-logging-tools"></a>Võrgu logimine tööriista abil

Saate hõivata liikluse klient ja server on vahetada andmeid ja võrgu tingimuste kohta üksikasjaliku teabe kliendi ja serveri vahel. Kasulik võrgu logimine tööriistad on järgmised.

- [Viiuldaja](http://www.telerik.com/fiddler) on tasuta web silumine puhverserveri, mis võimaldab teil uurida päised ja last andmete HTTP ja HTTPS koosolekukutsete ja kutsele vastamise sõnumeid. Lisateavet leiate teemast [lisa 1: abil viiuldaja jäädvustada HTTP ja HTTPS-liikluse](#appendix-1).
- [Microsofti võrgu kuvari (Netmonis)](http://www.microsoft.com/download/details.aspx?id=4865) ja [Wiresharkis](http://www.wireshark.org/) on tasuta võrgu protokolli analüsaatorid, mis võimaldavad teil vaadata mitmesuguseid võrguprotokollide paketi üksikasjalikku teavet. Wiresharkis kohta leiate lisateavet teemast "[lisa 2: abil Wiresharkis võrguliiklust jäädvustada](#appendix-2)".
- Microsoft sõnumi Analyzer on tööriist Microsoftilt, mis alistab Netmonis ja mis lisaks võrgu paketi andmehõive, aitab teil vaatamiseks ja analüüsimiseks jäädvustata muudelt tööriistadelt Logi andmed. Lisateavet leiate teemast "[lisa 3: kasutades Microsoft sõnumi Analyzer võrguliiklust jäädvustada](#appendix-3)".
- Kui soovite kontrollida, kas kliendi arvuti saab ühenduse Azure'i salvestusteenus üle võrgu ühenduvuse katse, ei saa klient standardne **ping** tööriista abil. Siiski saate kontrollimiseks [ **tcping** tööriista](http://www.elifulkerson.com/projects/tcping.php) abil.

Paljudel juhtudel salvestusruumi logimine ja salvestusruumi kliendi teek andmed log piisab diagnoosimine probleeme, kuid mõnel juhul võib-olla peate üksikasjalikumat teavet, mida saate sisestada need võrgu logimine tööriistad. Näiteks Fiddler abil HTTP ja HTTPS sõnumite kuvamine võimaldab teil saata ja salvestusruumi teenuseid, mis võimaldab teil läbi vaadata, kuidas kliendi rakendus uued katsed salvestusruumi toimingute päise- ja last andmeid vaadata. Protokoll analüsaatorid Wiresharkis nt töötada paketi tasemel, mis võimaldab teil vaadata TCP andmeid, mis võimaldab teil kaotatud pakettide ja ühenduvusprobleemide tõrkeotsing. Sõnumi analüsaator võib toimida HTTP ja TCP kihid.

## <a name="end-to-end-tracing"></a>Lõpuni jälgimine

Kasutades erinevaid logifailid lõpuni jälgimine on kasulik meetod võimalike probleemide uurimine. Kust otsida logifailide üksikasjalikku teavet, mis aitab teil selle probleemi tõrkeotsinguks märge abil saate oma andmetest mõõdikute kuupäeva/kellaaja andmeid.

### <a name="correlating-log-data"></a>Arvega logiandmed

Logide klientrakendustes kuvamisel võrgu jälgi ja salvestusruumi serveripoolne logimine on väga oluline on võimalik, et taotleb üle erinevate logifailid. Logifailide sisaldada mitmeid eri väljaga, mis on kasulik, kui korrelatsioonikordaja identifikaatorite. KliendiID taotlus on kõige enam kasu välja oleksid eri logid kirjete abil. Kuid vahel võib olla kasulik kasutada serveri taotluse id või ajatemplid. Järgmised jaotised sisaldavad Lisateavet nende võimaluste kohta.

### <a name="client-request-id"></a>Kliendiid taotlus

Salvestusruumi kliendi teek loob automaatselt kordumatu kliendi taotluse id iga taotlus.

- Kliendipoolne Logi, mis loob salvestusruumi kliendi teek, kuvatakse taotluse KliendiID **Kliendi taotlus ID** -välja iga Logi kirje, mis on seotud taotluse.
- Näiteks üks jäädvustatud viiuldaja võrgujälituse KliendiID taotlus on nähtav taotluse sõnumite väärtusena **x-ms-kliendi-taotlus-id** HTTP päis.
- Serveripoolne logimine salvestusruumi Logi taotlus KliendiID kuvatakse veerus kliendi taotluse ID-d.

> [AZURE.NOTE]On võimalik mitme taotluste samas KliendiID taotluse ühiskasutusse anda, kuna kliendi saate määrata selle väärtuse (kuigi salvestusruumi kliendi teek määratakse automaatselt uue väärtuse). Puhul korduskatsed klient, jagada kõigi katsete samas KliendiID taotlus. Partii saadetakse kliendi, puhul on pakett ühe KliendiID taotlus.


### <a name="server-request-id"></a>Serveri taotluse ID

Salvestusruumi teenus loob automaatselt serveri taotluse ID-d.

- Serveripoolne logimine salvestusruumi Logi server taotluse id kuvatakse **taotluse ID päise** veergu.
- Näiteks üks jäädvustatud viiuldaja võrgujälituse serveri taotluse id kuvatakse vastuse sõnumite väärtusena **x-ms-taotlus-id** HTTP päis.
- Kliendipoolne Logi, mis loob salvestusruumi kliendi teek, kuvatakse server taotluse id Logi kirje näitab serveri vastus üksikasjad veerus **Toiming teksti** .

> [AZURE.NOTE] Salvestusruumi teenuse alati määrab unikaalne server taotluse id iga taotluse saab, nii, et iga proovi uuesti katse klient ja iga toimingu kaasatud partii on kordumatu serveri taotluse id.

Kui salvestusruumi kliendi teek põhjustab mõne **StorageException** klient, sisaldab atribuudi **RequestInformation** **RequestResult** objekti, mis sisaldab **ServiceRequestID** atribuuti. Lisaks pääsete **RequestResult** objekti **OperationContext** eksemplar.

Allpool proovi kood näitab, kuidas määrata kohandatud **ClientRequestId** väärtus salvestusruumi teenuse objekti **OperationContext** taotluse lisades. Samuti kujutatakse **ServerRequestId** väärtuse toomine sõnumi vastuse.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Ajatemplid

Saate ka ajatemplid leidke seotud Logi kirjed, kuid olge ettevaatlik, mis tahes kell skew vahel klient ja server, mis on olemas. Pluss või miinus 15 minutit sobitamine serveripoolne kirjed vastavalt kliendi ajatempli tuleks otsida. Pidage meeles, et plekid, mis sisaldab mõõdikute bloobimälu metaandmed näitavad ajavahemiku jaoks mõõdikute, mis on talletatud bloobimälu; See on kasulik, kui teil on palju mõõdikute plekid sama minut või tund.

## <a name="troubleshooting-guidance"></a>Juhised tõrkeotsing

Selles jaotises aitab teil diagnoosimise ja levinud probleemide tõrkeotsingut rakenduse kohata Azure storage teenuste kasutamisel. Kasutage alltoodud loendist kindla probleemi asjakohase teabe leidmiseks.

**Otsusepuu kuvamine tõrkeotsing**

----------

Kas teie probleemi seotud salvestusruumi teenuse jõudlus?

- [Mõõdikute AverageE2ELatency kõrge ja madala AverageServerLatency kuvamine]
- [Mõõdikute kuvada madala AverageE2ELatency ja madala AverageServerLatency, kuid kliendi esineb pika latentsusajaga]
- [Mõõdikute kuvamine kõrge AverageServerLatency]
- [Teil on probleeme ootamatu viivitust sõnumi kohaletoimetamise järjekord]

----------

Kas teie probleemi seotud salvestusruumi teenuste olemasolu?

- [Mõõdikute kuvamiseks PercentThrottlingError suurendamine]
- [Mõõdikute kuvamiseks PercentTimeoutError suurendamine]
- [Mõõdikute kuvamiseks PercentNetworkError suurendamine]

----------

Klientrakenduse saab HTTP (nt 404) 4XX vastuse salvestusruumi teenuse?

- [Kliendi on sõnumeid HTTP 403 (keelatud)]
- [Kliendi on sõnumeid HTTP 404 (ei leitud)]
- [Kliendi on sõnumeid HTTP 409 (konflikt)]

----------

[Mõõdikute kuvamine madal PercentSuccess või analytics Logi kirjed on tegevuse ClientOtherErrors koos olek]

----------

[Võimsus mõõdikute kuvada ootamatud suurendada mäluruumi võimsus kasutamine]

----------

[Teil on probleeme ootamatu taaskäivitamisega Virtuaalmasinates, millele on manustatud VHDs suure hulga.]

----------

[Teie probleemi tuleneb abil salvestusruumi emulaator või test]

----------

[Ilmneb probleeme Azure'i SDK .net-i jaoks]

----------

[Teil on erinev küsimus salvestusruumi teenuse]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Mõõdikute AverageE2ELatency kõrge ja madala AverageServerLatency kuvamine

Selle all [Azure portaali](https://portal.azure.com) jälgimise tööriista joonisel näiteks kui **AverageE2ELatency** on märkimisväärselt suurem kui **AverageServerLatency**.

![][4]

Pange tähele, et selle salvestusteenus ainult arvutab meetermõõdustik **AverageE2ELatency** eduka taotluste, erinevalt **AverageServerLatency**, sisaldab klient võtab andmeid saata ja vastu võtta vastuvõtuteatis mäluruumi teenuse aeg. Seetõttu **AverageE2ELatency** ja **AverageServerLatency** vahe võib olla kas kliendi rakendus, mis on aeglane vastata, või tänu tingimuste võrgus.

> [AZURE.NOTE] Saate vaadata ka **E2ELatency** ja **ServerLatency** üksikute salvestusruumi toimingute salvestusruumi logimine Logi andmeid.

#### <a name="investigating-client-performance-issues"></a>Kliendi jõudlusprobleemide uurimine

Vasta aeglaselt kliendi võimalikku põhjust kaasata, kellel on teatud saadaval ühendused või teemade või on piisavalt ressursse, nt CPU, mälu või võrgu läbilaskevõime. Teil on võimalik, et probleemi lahendada, muutes kliendi koodi tõhusamaks (näiteks kasutab asünkroonne kõned salvestusruumi teenus) või kasutades suuremat virtuaalse masina (veel ja -vormid rohkem mälu).

Tabeli ja järjekorda teenuste Nagle algoritmi põhjuseks võib olla ka kõrge **AverageE2ELatency** võrreldes **AverageServerLatency**: lisateabe saamiseks vt [Nagle kasutaja on väike kutsed ei sõbralik](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx)postitus. Saate keelata, kasutades **ServicePointManager** klassi **System.Net** nimeruumi Nagle algoritmi kood. Peaksite seda tegema enne kui saate helistada tabelisse või järjekorda teenuste oma rakenduses, kuna see ei mõjuta ühendusi, mis on juba avatud. Järgmises näites on pärit töötaja roll **Application_Start** meetod.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Kontrollige kliendipoolseid logisid kuvamiseks, kui palju taotleb klientrakenduse esitab ja üldist .net-i jaoks sisse seotud täitmise kitsaskohtadest oma kliendi nagu CPU, .NET Prügikoristus võrgu kasutamine või mälu. Alguspunktina tõrkeotsingu .NET klientrakendustes, lugege teemat [silumine, jälgimine, ja profiili](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Latentsus võrguprobleemide uurimine

Tavaliselt on põhjustatud võrgu-lõpuni latentsusajaga siirdamiseks tingimustest. Saate uurida nii siirdamiseks ja püsivate võrguprobleemide, nt kaotatud paketid tööriistad, nt Wiresharkis või Microsoft sõnumi Analyzer abil.

Wiresharkis abil võrgu probleemide tõrkeotsingu kohta leiate lisateavet teemast "[lisa 2: Wiresharkis abil saate jäädvustada võrguliikluse]."

Võrgu tõrkeotsingu Microsoft sõnumi Analyzer kasutamise kohta lisateabe saamiseks lugege teemat "[lisa 3: Microsofti Message Analyzer abil saate jäädvustada võrguliiklust]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Mõõdikute kuvada madala AverageE2ELatency ja madala AverageServerLatency, kuid kliendi esineb pika latentsusajaga

Selle stsenaariumi korral on arvatavasti kontaktidel salvestusruumi teenuse taotluste viivitus. Peaks uurima, miks taotlusi klient ei tee seda kaudu bloobimälu teenusega.

Üks võimalik põhjus viivitamine päringu saatmine kliendile, et on saadaval ühendused või Teemad piiratud arv.

Peaksite ka seda, kas klient täidab mitme korduskatsed ja välja selgitada probleemi põhjuse, kui see on nii. Määratlemine, kas kliendi täidab mitme korduskatsed, saate teha järgmist.

- Uurige salvestusruumi Analytics logid. Kui juhtub mitme korduskatsed, kuvatakse mitu toimingud sama kliendi taotluse ID-ga, kuid erinevad serveri taotluse ID-d.
- Uurige kliendi logid. Paljusõnaline logimine näitab, et ilmnes uuesti.
- Silumine oma kood ja märkige **OperationContext** objekti, mis on seotud taotluse atribuudid. Kui toiming on uuesti proovida, **RequestResults** atribuut sisaldab mitut kordumatu serveri taotluse ID-d. Samuti saate iga taotlus algus- ja lõpukellaajad. Lisateavet leiate teemast jaotises [serveri taotluse ID]proovi kood.

Kui kliendi probleemid, tuleks uurida võimalike võrguprobleemide nt paketi kaotus. Näiteks Wiresharkis või Microsoft sõnumi Analyzer abil saate uurida võrguprobleemide.

Wiresharkis abil võrgu probleemide tõrkeotsingu kohta leiate lisateavet teemast "[lisa 2: Wiresharkis abil saate jäädvustada võrguliikluse]."

Võrgu tõrkeotsingu Microsoft sõnumi Analyzer kasutamise kohta lisateabe saamiseks lugege teemat "[lisa 3: Microsofti Message Analyzer abil saate jäädvustada võrguliiklust]."

### <a name="metrics-show-high-AverageServerLatency"></a>Mõõdikute kuvamine kõrge AverageServerLatency

Kõrge **AverageServerLatency** bloobimälu allalaadimine taotluste puhul kasutage salvestusruumi logimine logid kas on korduv taotlemine sama bloobimälu (või Sea plekid). Bloobimälu laadida taotlusi, peaks uurima, millist Blokeeri suurus kliendi kasutab (nt blokeerib väiksem kui 64K suurus võib põhjustada üldkulud kui loeb on ka väiksem kui 64K tükkideks), ja kui mitme kliendi on sama bloobimälu paralleelselt plokid üleslaadimine. Peaksite jaoks diagrammi taotlusi, mis on suurem arv minutis mõõdikute on teine skaleeritavus sihtkohtade kohta: Vaata ka "[mõõdikute Kuva suurendamist PercentTimeoutError]."

Kui te ei näe kõrge **AverageServerLatency** jaoks bloobimälu taotlusi alla laadida, kui on korduv taotlusi sama bloobimälu või plekid kogum, siis kaaluge vahemällu need plekid Azure vahemälu või Azure sisu kohaletoimetamise võrk (CDN) abil. Laadi taotluste saate parandada selle läbilaskevõime Blokeeri suuremaks abil. Päringute tabelitele, on võimalik rakendada kliendipoolne vahemälu klientide sama päringu toimingute tegemiseks mis ja kus andmed ei muutu sageli.

**AverageServerLatency** väärtusi saab ka sümptom halvasti kujundatud tabelit või päringut, et skannimine tulemi või mis järgige anti lisa/prepend mustri. Lisateabe saamiseks vaadake "[mõõdikute Kuva suurendamist PercentThrottlingError]".

> [AZURE.NOTE] Saate otsida täielik kontroll jõudluse kontroll siin: [Microsoft Azure salvestusruumi jõudlus ja skaleeritavus kontroll-loend](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Teil on probleeme ootamatu viivitust sõnumi kohaletoimetamise järjekord

Kui teil on probleeme viivituse rakenduse lisab sõnumi järjekorda ja see on saadaval kuhjuda lugemise aeg, siis võtke probleemi tuvastada järgmist:

- Veenduge, et rakendus on edukalt lisamise sõnumid järjekorda. Kontrollige, et rakendus on pole proovitakse uuesti **AddMessage** meetodit mitu korda enne edukalt. Salvestusruumi kliendi teek logides on teave mis tahes korduvate korduskatsed salvestusruumi toimingute.
- Veenduge, et kella skew vahel töötaja rolli, mis lisab teate kuhjuda ja töötaja rolli, mis loeb sõnumi järjekorda, mis muudab esineda nii, nagu on viivitus töötlemine.
- Märkige ruut, kui töötaja rolli, mis loeb sõnumite kuhjuda nurjub. Kui järjekorda kliendi kutsub **GetMessage** meetod, kuid ei vasta koos kinnituse, sõnumi jäävad järjekorda nähtamatu kuni **invisibilityTimeout** lõpeb. Selles etapis sõnum muutub kättesaadavaks uuesti.
- Märkige ruut, kui järjekorra pikkus kasvab aja jooksul. See võib juhtuda siis, kui teil on piisavalt töötajate töötlemiseks kõik sõnumid, mis teistele töötajatele asetavad järjekorda. Lisaks peaksite mõõdikute kuvamiseks, kui Kustuta taotleb suuda ja dequeue count sõnumitele, mis võib viidata korduvad nurjus katsete sõnumi kustutamine.
- Uurige salvestusruumi logimine logisid mis tahes järjekorda toiminguid, mis on suurem kui oodatud **E2ELatency** ja **ServerLatency** väärtuste üle aja kauem kui tavaliselt.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Mõõdikute kuvamiseks PercentThrottlingError suurendamine

Kui ületate skaleeritavus sihtkohtade salvestusruumi teenuse tekkida ahendamise tõrkeid. Salvestusruumi teenuse see tagada ega ühe kliendi rentnik saate kasutada teenust teiste arvelt. Lisateabe saamiseks leiate [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohtade](storage-scalability-targets.md) täpsemat skaleeritavus eesmärgid salvestusruumi kontod ja jõudluse eesmärgid sektsioonid jooksul salvestusruumi kontod.

Kui **PercentThrottlingError** mõõdiku kuvamiseks suurendada protsent taotlusi, mis on ei ole ahendamise viga, peate uurimine ühte kahte järgmist stsenaariumi:

- [PercentThrottlingError puhul]
- [Püsivate tõusu PercentThrottlingError tõrge]

Suurenemine **PercentThrottlingError** sageli aset samal ajal, kui taotluste arvu suurendada või kui olete algselt laadimine testimine rakenduse. See võib-olla ka taristu "503 Server hõivatud" või "500 toiming ajalõpp" HTTP olekuteade salvestusruumi toimingute klientrakenduses.

#### <a name="transient-increase-in-PercentThrottlingError"></a>PercentThrottlingError puhul

Kui kuvatakse diagrammi **PercentThrottlingError** väärtus, mis ühtib kõrge tegevuse perioodide rakendus, tuleks rakendada mõne välja strateegia korduskatsed eksponentsiaalse (mitte lineaarse) tagasi oma kliendi: see kohe sektsiooni vähendada ja aitavad teie taotlus läbi diagrammi liikluse sujuv. Kuidas rakendada uuesti salvestusruumi kliendi teegi kasutamise kohta leiate lisateavet teemast [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [AZURE.NOTE] Võidakse teile kuvada ka diagrammi **PercentThrottlingError** väärtus, mis ei ühti kõrge tegevuse perioodide rakendus: on siin, et see on salvestusteenus, teisaldada sektsioonid koormusetasakaalustuseks parandamiseks.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Püsivate tõusu PercentThrottlingError tõrge

Kui kuvatakse pidevalt kõrge **PercentThrottlingError** järgmine väärtus püsivalt suurendada, teie mahus tehingu või täites oma algse laadi katsed oma taotlus, siis peate hindamaks, kui teie rakendus kasutab salvestusruumi sektsioonid ja kas see on lähenemas skaleeritavus sihtkohtade salvestusruumi konto. Näiteks kui kuvatakse pidurdamise vead järjekorda (mis loeb ühe partition), siis võiksite kasutada täiendavaid järjekorrad levitada tehingud üle mitme sektsioonid. Kui kuvatakse pidurdamise tabeli tõrkeid, peate kaaluge mõne muu eraldamine värviskeemi tehinguid laiali mitu sektsiooni suurema hulga partition väärtused abil. Ühe levinud probleemi põhjus on prepend/lisamine anti muster kui valite sektsiooni võti kuupäev ja seejärel päeva kõik andmed on kirjutatud ühe sektsiooni: koormuse, võib see põhjustada kirjutamine kitsaskoht. Peaksite kaaluma erinevate eraldamine kujundus või kas kasutades bloobimälu võib olla parem lahendus. Ka kontrollida, kui ahendamine on tekkinud diagrammi oma liikluse ja uurima Eksponentsilumise oma taotlusi muster.

Kui teie levitatavat tehinguid üle mitme sektsioonid, peate olema endiselt teadlikud skaleeritavus piirides salvestusruumi konto. Näiteks kui kasutasite kümme järjekorrad iga töötlemine kuni 2000 1KB sõnumeid sekundis, saab 20 000 sõnumite sekundis salvestusruumi konto üldine alampiir. Kui vajate rohkem kui 20 000 üksuste sekundis töötlemine, võiksite kasutada mitut salvestusruumi kontod. Lisaks peaksite pidama meeles, et teie taotlused ja üksuste suurust mõjutab kui salvestusruumi teenuse ahendab kliendid: kui teil on suurem taotlusi ja üksused, mida võib rakendus varem.

Ebaefektiivne päringu kujundus võib põhjustada teie tabas tabeli sektsioonid skaleeritavus limiidid. Näiteks päringu filtriga, mis valib ainult üks protsent üksuste sektsiooni, kuid mis skannib kõik soovitud üksused sektsiooni tuleb iga üksuse juurde. Iga üksuse lugemine arvestatakse suunas koguarvu tehingute sektsiooni; Seetõttu jõuate lihtsalt sihtkohtade skaleeritavus.

> [AZURE.NOTE] Jõudluse testimine peaks näitavad mis tahes ebaefektiivne päringu kujundused oma rakenduse.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Mõõdikute kuvamiseks PercentTimeoutError suurendamine

Teie mõõdikute kuvada suurendamine ühe salvestusruumi teenuste **PercentTimeoutError** . Samal ajal saab klient "500 toiming ajalõpp" HTTP olekuteade suure hulga salvestusruumi toimingud.

> [AZURE.NOTE] Teile võidakse kuvada ajalõpp vigade ajutiselt salvestusruumi teenuse laadimine saldo taotlusi, teisaldades sektsiooni uue server.

**PercentTimeoutError** mõõdiku on järgmine mõõdikute mitmest: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**ja **SASServerTimeoutError**.

Serveri ajalõpud põhjustavad tõrge serveris. Kliendi ajalõpud põhjuseks on asjaolu, et toimingu server on ületanud aeg, mille klient; näiteks mõnda muud klienti salvestusruumi kliendi teegi abil saate seada üksusega toimingu **QueueRequestOptions** klassi atribuudi **ServerTimeout** abil.

Serveri ajalõpud viidata salvestusteenus, mis nõuab täiendavat uurimine probleem. Saate mõõdikute kuvamiseks, kui teil on pihta teenuse skaleeritavus piirangud ja mis tahes diagrammi liikluse, mis võivad põhjustada probleemi tuvastamiseks. Kui probleem on vahelduva, see võib olla tingitud koormust tasakaalustavad tegevuse teenus. Kui probleem püsib ja ei ole põhjustatud pihta skaleeritavus piirangud teenuse rakenduse, tuleks tõsta tugiteenuste küsimus. Jaoks kliendi ajalõpud, saate otsustada, kui aeg, mille väärtuseks sobivat väärtust klient ja kas ajalõpu väärtust määramine kliendis muutmine või uurige, kuidas saate parandada toimingute salvestusruumi teenus, näiteks tabeli päringute optimeerimine või sõnumite mahu vähendamine.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Mõõdikute kuvamiseks PercentNetworkError suurendamine

Oma mõõdikute kuvada suurendamine ühe salvestusruumi teenuste **PercentNetworkError** . **PercentNetworkError** mõõdiku on järgmine mõõdikute mitmest: **NetworkError**, **AnonymousNetworkError**ja **SASNetworkError**. Need ilmneda salvestusruumi teenuse tuvastab võrgu tõrge, kui klient teeb salvestusruumi kutse.

Selle tõrke kõige levinum põhjus on klient lahti enne ajalõpp lõppemist salvestusruumi teenus. Kood tuleks uurida oma kliendi mõista, miks ja millal kliendi katkestatakse salvestusruumi teenus. Saate uurida kliendi võrgu ühenduvusprobleemide ka Wiresharkis, Microsoft sõnumi Analyzer või Tcping. Nende tööriistade on kirjeldatud [liites].

### <a name="the-client-is-receiving-403-messages"></a>Kliendi on sõnumeid HTTP 403 (keelatud)

Kui klientrakenduse on korrutamine tõrgete HTTP 403 (keelatud), on võib põhjuseks kliendi kasutab seda saadab salvestusruumi kutse (kuigi muud võimalikud põhjused kell skew, kehtetu võtmed ja tühja päised) on aegunud ühiskasutusse Accessi allkirja (SAS). Kui põhjuseks on aegunud SAS klahvi, kuvatakse kõik kirjed serveripoolne logimine salvestusruumi log andmeid. Järgmine tabel sisaldab valimi kliendipoolne Logi loodud salvestusruumi kliendi teek, mis näitab, et see probleem ilmneb.

Allikas|Jaarittelu|Jaarittelu|KliendiID taotlus|Toiming tekst
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Teave|3|85d077ab-...|Toimingu alustades esmane asukoht asukoht režiimi PrimaryOnly kohta.
Microsoft.WindowsAzure.Storage|Teave|3|85d077ab-...|Alates sünkroonse taotluse https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;si = mypolicy&amp;sig = OFnd4Rd7z01fIvh 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3D&amp;api-versioon = 2014-02-14.
Microsoft.WindowsAzure.Storage|Teave|3|85d077ab-...|Vastuse ootamine.
Microsoft.WindowsAzure.Storage|Hoiatus|2|85d077ab-...|Vastuse ootamine erand: serveri tagastatakse tõrge: (403) keelatud.
Microsoft.WindowsAzure.Storage|Teave|3|85d077ab-...|Vastuse saanud. Olekukoodi = 403 taotluse ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, sisu-MD5 =, ETag =.
Microsoft.WindowsAzure.Storage|Hoiatus|2|85d077ab-...|Selle toimingu käigus erand: serveri tagastatakse tõrge: (403) keelatud.
Microsoft.WindowsAzure.Storage|Teave|3 |85d077ab-...|Kontrollimine, kui toimingut tuleb uuesti proovida. Uute katsete arv = 0, HTTP olekukoodi = 403 erandi = kaugserverisse, tagastatakse tõrge: (403) keelatud.
Microsoft.WindowsAzure.Storage|Teave|3|85d077ab-...|Järgmise asukoht on seatud esmane, mis põhineb režiimi kohta.
Microsoft.WindowsAzure.Storage|Tõrge|1|85d077ab-...|Proovi uuesti poliitika ei luba uuesti. Probleemse remote serveriga tagastatakse tõrge: (403) keelatud.

Selle stsenaariumi korral saate uurida, miks SAS luba aegub enne kliendi saadab luba server:

- Tavaliselt pole Määrake algusaeg, kui loote SAS, kliendile kohe kasutada. Kui tegemist on väike kell host, sarnaselt muude abil praeguse kellaaja ja salvestusteenus, siis on võimalik saada SAS, mis pole veel lubatud teenuse salvestusruumi erinevused.
- Ei peaks lühikese aegumise aeg on SAS pannud. Klõpsake uuesti väikest kella erinevused selle hosti muude ja salvestusruumi teenuse võib põhjustada SAS, mis on aegumas ilmselt varem oodatust.
- SAS võti versiooni parameeter ei (nt **sv = 2015-04-05**) vastavad salvestusruumi kliendi teek, mida kasutate versiooni? Soovitame Kasuta alati [Salvestusruumi kliendi teek](https://www.nuget.org/packages/WindowsAzure.Storage/)uusim versioon.
- Kui te taastada teie salvestusruumi kiirklahvide, võib see kaotada kõik olemasolevad SAS märgid. See võib olla probleeme, kui teil luua SAS sõned koos klientrakendustes vahemällu pikk kehtivuse aeg.

Kui kasutate salvestusruumi kliendi teek SAS sõned loomiseks, siis on lihtne koostada kehtiv luba. Juhul, kui kasutate salvestusruumi REST API-ga ja SAS sõned käsitsi loomine lugege hoolikalt teema [Delegeerivad juurdepääs ühiskasutusse antud Accessi allkirjastatud](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>Kliendi on sõnumeid HTTP 404 (ei leitud)
Kui server ei saa klientrakenduse meilisõnumi HTTP 404 (ei leitud), see tähendab, et klient proovis kasutada (nt üksusele, tabel, bloobimälu, container või järjekorda) objekt pole teenuse salvestusruumi. On mitmeid võimalikke põhjuseid, näiteks:

- [Kliendi või muu protsess varem kustutatud objekti]
- [Ühiskasutusse antud juurdepääs allkirja (SAS) autoriseerimine probleem]
- [Kliendipoolne JavaScripti koodi ei ole objekti juurdepääsu õigus]
- [Võrgu tõrge]

#### <a name="client-previously-deleted-the-object"></a>Kliendi või muu protsess varem kustutatud objekti
Stsenaariumid, kus kliendi proovib lugeda, värskendada ja kustutada salvestusruum teenuses andmeid on hõlbus tuvastada serveripoolne logisid eelmise toimingu, mis kustutatud objekti kõnealuse salvestusruumi teenuse. Sageli Logi andmed näitab, et teise kasutaja või protsess kustutatud objekti. Serveripoolne logimine salvestusruumi Logi toiming-tüüpi ja nõutav objekti võti veerud kuvatakse, kui mõnda muud klienti kustutatud objekti.

Kui mõnda muud klienti üritades objekti lisamine stsenaariumi see ei pruugi olla kohe selge miks tulemuseks vastuse HTTP 404 (ei leitud), et kliendi loob uue objekti. Juhul, kui kliendi loob mõne bloobimälu peab olema leida bloobimälu ümbris, kui kliendi on peab olema leida järjekorda sõnumi loomisel ja kliendi on rea lisamine peab olema leida tabel.

Salvestusruumi kliendi teegist kliendipoolne logi abil saate üksikasjalikumat mõista kui klient saadab kindlate taotluste salvestusruumi teenusega.

Järgmised kliendipoolne Logi loodud salvestusruumi kliendi teek illustreerib probleemi, kui klient ei leia ümbris bloobimälu, on luua. See logi sisaldab täpsemat salvestusruumi järgmisi toiminguid:

Päringu-ID|Toiming
---|---
07b26a5d-...|**DeleteIfExists** meetod bloobimälu kustutada. Tähele, et see toiming sisaldab **pea** taotluse ümbris olemasolu kontrollida.
e2d06d78...|**CreateIfNotExists** meetod bloobimälu container loomiseks. Tähele, et see toiming sisaldab **pea** taotlus, mis kontrollib kõigepealt ümbrist olemasolu. **Juhi** tagastab 404 sõnumi, kuid ei lahene.
de8b1c3c-...|**UploadFromStream** meetod on bloobimälu loomiseks. **Sellele** taotlus nurjub 404 sõnumiga

Logi kirjed:

Päringu-ID |  Toiming tekst
---|---
07b26a5d-...|Alates sünkroonse taotluse https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 juuni 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Vastuse ootamine.
07b26a5d-... | Vastuse saanud. Olekukoodi = 200 taotluse ID = eeead849... Sisu-MD5 =, ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Vastus päised on edukalt, töödeldud, lähtudes ülejäänud toiming.
07b26a5d-... | Allalaadimine vastuse keha.
07b26a5d-... | Toiming on edukalt lõpule.
07b26a5d-... | Alates sünkroonse taotluse https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 juuni 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Vastuse ootamine.
07b26a5d-... | Vastuse saanud. Olekukoodi = 202, taotluse ID = 6ab2a4cf-..., sisu-MD5 =, ETag =.
07b26a5d-... | Vastus päised on edukalt, töödeldud, lähtudes ülejäänud toiming.
07b26a5d-... | Allalaadimine vastuse keha.
07b26a5d-... | Toiming on edukalt lõpule.
e2d06d78-... | Alates asünkroonne taotluse https://domemaildist.blob.core.windows.net/azuremmblobcontainer.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 juuni 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Vastuse ootamine.
de8b1c3c-... | Alates sünkroonse taotluse https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... |  StringToSign = pane... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-blob-Type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-Date:Tue, 03 juuni 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Taotluse andmete kirjutamiseks ettevalmistamine.
e2d06d78-... | Vastuse ootamine erand: serveri tagastatakse tõrge: (404) ei leitud.
e2d06d78-... | Vastuse saanud. Olekukoodi = 404 taotluse ID = 353ae3bc-..., sisu-MD5 =, ETag =.
e2d06d78-... | Vastus päised on edukalt, töödeldud, kelle toimingu ülejäänud.
e2d06d78-... | Allalaadimine vastuse keha.
e2d06d78-... | Toiming on edukalt lõpule.
e2d06d78-... | Alates asünkroonne taotluse https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
e2d06d78-...|StringToSign = pane... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-Date:Tue, 03 juuni 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Vastuse ootamine.
de8b1c3c-... | Kirjutamine taotluse andmed.
de8b1c3c-... | Vastuse ootamine.
e2d06d78-... | Vastuse ootamine erand: serveri tagastas tõrke: (409) konflikt.
e2d06d78-... | Vastuse saanud. Olekukoodi = 409, taotluse ID = c27da20e-..., sisu-MD5 =, ETag =.
e2d06d78-... | Allalaadimise tõrge vastuse keha.
de8b1c3c-... | Vastuse ootamine erand: serveri tagastatakse tõrge: (404) ei leitud.
de8b1c3c-... | Vastuse saanud. Olekukoodi = 404 taotluse ID = 0eaeab3e-..., sisu-MD5 =, ETag =.
de8b1c3c-...| Selle toimingu käigus erand: serveri tagastatakse tõrge: (404) ei leitud.
de8b1c3c-... | Proovi uuesti poliitika ei luba uuesti. Probleemse remote serveriga tagastatakse tõrge: (404) ei leitud.
e2d06d78-... | Proovi uuesti poliitika ei luba uuesti. Probleemse remote Server tagastas tõrke: (409) konflikt.

Selles näites logi näitab, et kliendi vahelduvuse päringutele **CreateIfNotExists** meetod (taotluse id e2d06d78...) **UploadFromStream** meetodit taotlustele (de8b1c3c-...); See juhtub, kuna klientrakendus on kasutada asünkroonselt järgmised võimalused. Muutke asünkroonne koodi tagamaks, et see loob ümbris enne, kui proovite üles laadida kõik andmed on bloobimälu selle ümbrises klientrakenduses. Ideaalvariandis mitte, peaksite looma kõik teie ümbriste ette.

#### <a name="SAS-authorization-issue"></a>Ühiskasutusse antud juurdepääs allkirja (SAS) autoriseerimine probleem

Kui kliendi rakendus proovib kasutada SAS võti, mis ei sisalda vajalikke õigusi selle toimingu jaoks, tagastab salvestusruumi teenuse kliendile meilisõnumi HTTP 404 (ei leitud). Samal ajal näete ka väärtuse null **SASAuthorizationError** jaoks soovitud mõõdikud.

Järgmine tabel sisaldab meilisõnumi näidis serveripoolne log logifaili salvestusruumi logimine.

<table>
  <tr>
    <td>Kutse algusaeg.</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Toimingu tüüp</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Taotluse olek</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>HTTP olekukoodi</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Autentimistüüp</td>
    <td>SAS</td>
  </tr>
  <tr>
    <td>Teenuse tüüp</td>
    <td>Bloobimälu</td>
  </tr>
  <tr>
    <td>URL-i taotlemine</td>
    <td>
    https://domemaildist.blob.Core.Windows.net/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp; sr = c&amp;amp; si = mypolicy&amp;amp; sig = XXXXX&amp;amp; api-versioon = 2014-02-14&amp;amp;</td>
  </tr>
  <tr>
    <td>Taotluse id päis</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Kliendiid taotlus</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

Peaksite uurima, miks klientrakenduse proovib see pole antud õiguste toimingu.

#### <a name="JavaScript-code-does-not-have-permission"></a>Kliendipoolne JavaScripti koodi ei ole objekti juurdepääsu õigus

Kui kasutate mõnda muud klienti JavaScripti ja salvestusruumi teenus on tagasi HTTP 404 sõnumeid, saate otsida järgmistest tõrketeadetest JavaScripti brauseris:

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Internet Exploreris F12 Arendaja tööriistad saate jälgida on kliendipoolne JavaScripti probleemide tõrkeotsingu brauseri ja salvestusruumi teenuse vahel vahetatud sõnumite.

Need tõrked, kuna veebibrauseri rakendab [sama origin poliitika](http://www.w3.org/Security/wiki/Same_Origin_Policy) turvalisus piirang, mis takistab veebilehe helistamiseks API domeenil lehe pärineb domeeni.

JavaScripti probleemi lahendamiseks saate konfigureerida Cross Origin ressursside ühiskasutus (CORS) salvestusruumi teenuse kliendi on juurdepääs. Lisateabe saamiseks vt [Cross-päritolu ressursside ühiskasutus (CORS) tugi Azure salvestusruumi teenused](http://msdn.microsoft.com/library/azure/dn535601.aspx).

Järgmine kood näidis kuvatakse teie bloobimälu teenusele, et lubada Contoso domeenis töötav JavaScripti lisamine bloobimälu oma bloobimälu salvestusruum teenuses konfigureerimine:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Võrgu tõrge

Teatud juhtudel võib põhjustada kaotsi võrgu paketid salvestusteenus, naasevad kliendi HTTP 404 sõnumeid. Näiteks kui klientrakenduse on tabeli teenuse olemi kustutamist kuvatakse kliendi põhjustada salvestusruumi erandi aruandlus on "HTTP 404 (ei leitud)" olekuteade teenusest tabel. Kui saate uurida tabeli tabeli salvestusteenus, näete, et teenus ei Kustuta üksus soovile.

Erandi üksikasjad klientrakenduses sisaldavad määratud tabeli teenus taotluse taotluse id (7e84f12d): saate selle teabe leidmiseks taotluse üksikasjad serveripoolne salvestusruumi logid logifaili veerus **taotlus-id-päise** otsingu kaudu. Kindlaks teha, kui tõrkeid, nagu see tekkida ja otsige logifailide aja mõõdikud salvestatud see tõrge võib kasutada ka mõõdikud. Logi kirje näitab, et Kustuta nurjus "HTTP (404) klient muude tõrge" olek sõnumiga. Sama Logi kirje sisaldab ka **taotluse KliendiID** veerus (813ea74f) kliendi poolt loodud taotluse ID-d.

Serveripoolne logi sisaldab ka mõne muu kirje sama väärtusega **KliendiID taotluse** (813ea74f) eduka Kustutustoimingu, et sama olemi ja sama kliendi jaoks. Eduka kustutamine toimingut toimus väga vähe aega enne nurjunud kustutamine taotluse.

Sel juhul võib põhjuseks kliendi saadetud üksus Kustuta taotluse tabeli teenus, mis õnnestus, kuid ei saanud kinnituse server (võib-olla tingitud ajutine võrguprobleemi). Kliendi seejärel automaatselt uuesti proovida toimingu (kasutades sama **kutse KliendiID**) ja selle uuesti nurjus, kuna üksus oli juba kustutatud.

Kui see probleem ilmneb sageli, tuleks uurida, miks klient ei suuda kinnitused saadud tabeli teenus. Kui probleem on vahelduva, peaks ülekatet tõrge "HTTP (404) ei leitud" ja sisselogimisel kliendi kuid luba kliendi jätkamiseks.

### <a name="the-client-is-receiving-409-messages"></a>Kliendi on sõnumeid HTTP 409 (konflikt)

Järgmine tabel näitab serveripoolne Logi kahe kliendi toimingute väljavõte: **DeleteIfExists** järgneb kohe **CreateIfNotExists** bloobimälu container sama nimi. Pange tähele, et iga kliendi toimingu tulemused kahele saadetud server, esmalt **GetContainerProperties** taotluse kontrollida, kas on olemas ümbris, millele järgneb **DeleteContainer** või **CreateContainer** kutse.

Ajatempli|Toiming|Tulemus|Container nimi|KliendiID taotlus
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

Klientrakenduse koodi kustutab ja seejärel kohe recreates bloobimälu container, kasutades sama nimi: **CreateIfNotExists** meetodit (päring kliendi ID bc881924-...) lõpuks nurjub tõrkega HTTP 409 (konflikt). Kui mõnda muud klienti kustutab bloobimälu ümbriste, tabelite või järjekorrad on lühike nimi enne muutub kättesaadavaks uuesti.

Klientrakenduse tuleks kasutada kordumatu container nimed iga kord, kui luuakse uus ümbriste kui Kustuta/Loo muster on levinud.

### <a name="metrics-show-low-percent-success"></a>Mõõdikute kuvamine madal PercentSuccess või analytics Logi kirjed on tegevuse ClientOtherErrors koos olek

**PercentSuccess** mõõdiku sisaldab protsenti toiminguid, mis ei õnnestunud oma HTTP olekukoodi põhjal. Tegevuse olek koodiga 2XX loendamine nii õnnestus, toimingute 3XX, 4XX ja 5XX vahemike olek koodiga õnnestu loetakse ja langetada **PercentSucess** argumendil väärtus. Serveripoolne salvestusruumi logifailide, on need toimingud salvestatud **ClientOtherErrors**kande olek.

See on oluline Pange tähele, et need toimingud on lõpule viidud ja seetõttu ei mõjuta muid mõõdikute näiteks kättesaadavust. Mõned toimingud, mis edukalt käivitada, kuid see võib põhjustada õnnestu HTTP olekukoodi näited.
- **ResourceNotFound** (Ei leitud 404), näiteks: GET-päringu abil bloobimälu, mida pole olemas.
- **ResouceAlreadyExists** (Konflikti 409), näiteks: **CreateIfNotExist** toiming, kus ressurss juba olemas.
- **ConditionNotMet** (Pole muudetud 304), näiteks: tingimusvormingu toimingut, näiteks kui klient saadab **ETag** -väärtus ja mõne HTTP **If-pole-Match** päise taotleda pilt ainult juhul, kui see on värskendatud pärast viimast toimingut.

Leiate loendi levinumate REST API tõrkekoodid salvestusruumi teenuste lehel [Levinud REST API tõrkekoodid](http://msdn.microsoft.com/library/azure/dd179357.aspx)tulemiks.

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Võimsus mõõdikute kuvada ootamatud suurendada mäluruumi võimsus kasutamine


Kui näete ootamatu, ootamatud muudatused võimsuse kasutuse teie salvestusruumi konto, saate uurida millistel põhjustel vaadates esimese oma kättesaadavuse mõõdikute; näiteks suurenemine arvu nurjunud Kustuta taotlusi võib kaasa tuua suurendamise bloobimälu kasutate nagu rakenduse teatud puhastamine võib olla lootsite olema vabastada ruumi ei tööta oodatud viisil (nt seetõttu, et SAS märgid, mis on kasutatud ruumi vabastamise on aegunud).

### <a name="you-are-experiencing-unexpected-reboots"></a>Teil on probleeme ootamatu taaskäivitamisega, Azure'i Virtuaalmasinates millele on manustatud VHDs suure hulga

Kui Azure virtuaalse masina (VM) on suur hulk manustatud VHDs, mis on sama salvestusruumi konto, võivad olla suuremad skaleeritavus sihtkohtade põhjustada VM nurjumise üksikute salvestusruumi konto jaoks. Kontrollige minute mõõdikute salvestusruumi konto (**TotalRequests**/**TotalIngress**/**TotalEgress**) diagrammi, mis ületavad skaleeritavus sihtkohtade salvestusruumi konto jaoks. Leiate abi kindlaks teha, kui pidurdamise jaotises "[mõõdikute Kuva suurendamist PercentThrottlingError]" ilmnenud on salvestusruumi kontol.

Üldiselt iga üksiku sisendi või toimingut väljund on VHD Virtual seadmes vaste **Saada lehe** või **Sellele lehele** aluseks oleva lehe bloobimälu toiminguid. Seega saate hinnanguline IOPS keskkonna häälestamiseks saate ühe salvestusruumi konto aluseks rakenduse käitumist teatud mitu VHDs. Me ei soovita seda, kas teil on rohkem kui 40 ketast ühe salvestusruumi konto. [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohtade](storage-scalability-targets.md) üksikasjad leiate praeguse skaleeritavus eesmärkide salvestusruumi kontode, eriti kokku taotluse määr ja kokku läbilaskevõime kasutate salvestusruumi konto tüübi jaoks.
Kui skaleeritavus sihtkohtade ületavad talletusmahu konto jaoks, peaks teie VHDs paigutamiseks mitme eri salvestusruumi konto aktiivsuse iga üksiku konto.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Teie probleemi tuleneb abil salvestusruumi emulaator või test

Tavaliselt salvestusruumi emulaator hinnapakkumisel arendamise ja testida vältimiseks Azure storage konto nõue. Levinud probleemid, mis võib juhtuda, kui kasutate salvestusruumi emulaator on:

- [Funktsioon "X" salvestusruumi emulaator ei tööta]
- [Tõrge "väärtus üks HTTP päised ei ole õiges vormingus" salvestusruumi emulaator kasutamisel]
- [Töötab salvestusruumi emulaator nõuab administraatoriõigusi]

#### <a name="feature-X-is-not-working"></a>Funktsioon "X" salvestusruumi emulaator ei tööta

Salvestusruumi emulaator ei toeta kõiki Azure storage teenused, nagu faili teenuse funktsioone. Lisateabe saamiseks leiate teemast [Azure salvestusruumi emulaator ja testimine](storage-use-emulator.md).

Funktsioone, mis ei toeta salvestusruumi emulaator, kasutage Azure salvestusteenus pilveteenuses.

#### <a name="error-HTTP-header-not-correct-format"></a>Tõrge "väärtus üks HTTP päised ei ole õiges vormingus" salvestusruumi emulaator kasutamisel

Teil on rakenduse salvestusruumi kliendi teek vastu kohalikku emulaator kasutavate testimine ja nõuab nagu **CreateIfNotExists** nurjuda tõrketeatega "väärtus üks HTTP päised ei ole õiges vormingus." See näitab, et kasutate salvestusruumi emulaator versioon ei toeta Office'i versiooni te kasutate salvestusruumi kliendi teek. Salvestusruumi kliendi teek lisab kõik see muudab taotlused päise **x-ms-versioon** . Kui salvestusruumi emulaator väärtus päise **x-ms-versioon** ei tuvasta, ei nõustu kutse.

Salvestusruumi teegi kliendi logid abil saate vaadata selle saadab **päise x-ms-versiooni** väärtust. **X-ms-versiooni päist** väärtus näete ka siis, kui kasutate viiuldaja jälgida taotlused klientrakenduse kaudu.

Selle stsenaariumi tavaliselt ilmneb juhul, kui installite ja kasutada uusimat versiooni salvestusruumi kliendi teek salvestusruumi emulaator värskendamata. Peaks salvestusruumi emulaator uusima versiooni installimine või kasutada salvestusruumi asemel emulaator ja testi.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Töötab salvestusruumi emulaator nõuab administraatoriõigusi

Küsitakse administraatori identimisteave salvestusruumi emulaator käivitamisel. See juhtub ainult siis, kui teil on salvestusruumi emulaator käivitamine esimest korda. Pärast on lähtestatud salvestusruumi emulaator, pole vaja administraatoriõigusi uuesti käivitada.

Lisateabe saamiseks leiate teemast [Azure salvestusruumi emulaator ja testimine](storage-use-emulator.md). Pange tähele, et saate ka lähtestada salvestusruumi emulaator Visual Studios, mis nõuavad administraatoriõigusi.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Ilmneb probleeme Azure'i SDK .net-i jaoks

Kui proovite installida SDK, ei installimisel salvestusruumi emulaator oma kohalikus arvutis. Installi logi sisaldab ühte järgmistest:

- CAQuietExec: Tõrge: juurdepääs SQL-i eksemplari
- CAQuietExec: Tõrge: ei saa luua andmebaasi

Põhjus on olemasolev LocalDB installimisega probleeme. Vaikimisi kasutab salvestusruumi emulaator LocalDB andmed ei kao, kui see jäljendab Azure storage teenused. Saate lähtestada oma LocalDB eksemplari käivitades järgmised käsud enne installimist SDK-käsuviiba aken.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

Käsk **Kustuta** eemaldab kõik vana andmebaasifailide eelmise installide salvestusruumi emulaator.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Teil on erinev küsimus salvestusruumi teenuse

Kui tõrkeotsingu eelmiste jaotiste ei sisalda probleem, mis on salvestusteenus, peaks vastu võtma järgmist lähenemisviisi diagnoosimise ja probleemi tõrkeotsing.

- Kontrollige oma mõõdikute kuvamiseks, kui muutuste kaudu oma oodatud käitumine base-joon. Kaudu mõõdikute, teil on võimalik, et määratleda, kas probleem on ajutine või püsiv ja millised salvestusruumi toimingute probleem mõjutab.
- Mõõdikute teabe abil saate otsida serveripoolne logiandmed üksikasjalikumat teavet, mis toimuvad tõrgete kohta. See teave võib aidata teil probleemi lahendamiseks ja tõrkeotsing.
- Kui serveripoolne logid teave ei piisa probleemi tõrkeotsinguks edukalt, saate salvestusruumi kliendi teek kliendipoolseid logisid uurida oma klientrakenduse ja näiteks Fiddler, Wiresharkis ja Microsoft sõnumi Analyzer uurida võrgu toimimist.

Viiuldaja kasutamise kohta leiate lisateavet teemast "[lisa 1: viiuldaja abil saate jäädvustada HTTP ja HTTPS-liikluse]."

Wiresharkis kasutamise kohta leiate lisateavet teemast "[lisa 2: Wiresharkis abil saate jäädvustada võrguliikluse]."

Microsoft sõnumi Analyzer kasutamise kohta leiate lisateavet teemast "[lisa 3: Microsofti Message Analyzer abil saate jäädvustada võrguliiklust]."

## <a name="appendices"></a>Lisa

Lisades kirjeldatud eri tööriistu, mis teile võib-olla kasulik, kui olete diagnoosimise ja tõrkeotsingu Azure Storage (ja muud teenused). Nende tööriistade ei kuulu Azure Storage ja mõned on kolmanda osapoole. Sellisel kujul need liites nimetatud tööriistade ei hõlma mis tahes tugiteenuste lepingu võib teil olla Microsoft Azure'i või Azure Storage ja seega teie hindamise käigus tuleks uurida litsentsimine ja tugiteenuste suvandid saadaval pakkujate nende tööriistade.

### <a name="appendix-1"></a>Lisa 1: Viiuldaja abil jäädvustada HTTP ja HTTPS-liikluse

[Viiuldaja](http://www.telerik.com/fiddler) on kasulik analüüsimiseks HTTP ja HTTPS liiklust klientrakenduse ja kasutate Azure salvestusteenus vahel.

> [AZURE.NOTE] Viiuldaja saab dekodeerida HTTPS-liikluse; peaksite lugema viiuldaja dokumentatsiooni hoolikalt, et aru saada, kuidas see ja mõista turvalisus.

Selles lisas esitatakse lühike samm-sammult juhendi viiuldaja jäädvustada vahel kohalikus arvutis, kuhu on installitud viiuldaja ja Azure storage teenuste konfigureerimine.

Pärast on käivitanud viiuldaja, hakkavad hõivamine HTTP ja HTTPS-liikluse oma kohalikus arvutis. Järgnevalt on toodud mõned kasulikud käsud viiuldaja kontrollimiseks.

- Lõpetamine ja hakake hõivamine liikluse. Klõpsake põhimenüü, avage **fail** ja klõpsake **Jäädvustada liiklust** lülitab sisse ja välja.
- Jäädvustatud liikluse andmed salvestada. Põhimenüü, valige **fail**, klõpsake nuppu **Salvesta**ja seejärel klõpsake nuppu **Kõik eksemplarid**: saate salvestada liiklust seansi arhiivifaili. Seansi arhiivi analüüsi jaoks hiljem uuesti või saatke see, kui see on nõutav Microsofti tugiteenuste.

Liikluse, mis salvestab viiuldaja piiritlemiseks saate kasutada filtreid, mis **filtrid** vahekaardil konfigureerida. Järgmine pilt on kuvatud filter, mis sisaldab ainult liikluse suunamiseks **contosoemaildist.table.core.windows.net** salvestusruumi lõpp-punkti.

![][5]

### <a name="appendix-2"></a>Lisa 2: Abil Wiresharkis võrguliiklust jäädvustada

[Wiresharkis](http://www.wireshark.org/) on võrgu protokolli analüsaator, mis võimaldab vaadata mitmesuguseid võrguprotokollide paketi üksikasjalikku teavet.

Järgnevalt näidatakse, kuidas jäädvustada paketi üksikasjalikku teavet liikluse kohalikust arvutist kuhu installisite Wiresharkis tabeli teenusega Azure storage konto.

1.  Käivitage oma kohalikus arvutis Wiresharkis.
2.  Valige jaotises **käivitamine** kohtvõrgu kasutajaliidese või, mis on Interneti-ühendus.
3.  Klõpsake **jäädvustada suvandid**.
4.  Lisage filtri **Capture Filter** tekstiväli. Näiteks **host contosoemaildist.table.core.windows.net** konfigureerib Wiresharkis jäädvustada ainult paketid või tabeli teenuse lõpp-punkti **contosoemaildist** salvestusruumi konto kaudu saadetud. Vaadake üle [jäädvustada filtrite täieliku loendi](http://wiki.wireshark.org/CaptureFilters).

    ![][6]

5.  Klõpsake käsku **Käivita**. Wiresharkis saab nüüd jäädvustada kõik paketid saata või tabeli teenuse lõpp-punkt: kui kasutate oma kohalikus arvutis klientrakenduse.
6.  Kui olete lõpetanud, põhimenüü klõpsake **jäädvustada** ja seejärel **Peata**.
7.  Jäädvustatud andmete Wiresharkis jäädvustada faili salvestamiseks põhimenüü klõpsake nuppu **fail** ja seejärel **salvestage**.

Wiresharkis tõstab esile **packetlist** aknas tõrkeid. Akna **Ekspert teave** (valige **analüüsi**ja seejärel **Ekspert teave**) abil saate vaadata tõrgete ja hoiatuste kokkuvõte.

![][7]

Saate ka valida TCP andmete kuvamiseks, nagu rakenduse kiht näeb seda paremklõpsates TCP andmeid ja valides **Jälgi TCP voo**. See on eriti kasulik, kui teie dump ilma capture filter on jäädvustatud. Lisateavet leiate teemast [järgmist TCP voogu] (http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [AZURE.NOTE] Wiresharkis kasutamise kohta leiate lisateavet teemast [Wiresharkis kasutajate juhend](http://www.wireshark.org/docs/wsug_html_chunked).

### <a name="appendix-3"></a>Lisa 3: Microsoft sõnumi Analyzer abil võrguliiklust jäädvustada

Saate Microsoft sõnumi Analyzer abil saate jäädvustada HTTP ja HTTPS-liikluse viiuldaja sarnaselt ja jäädvustada Wiresharkis sarnaselt võrguliiklust.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Web jälgimine seanssi Microsoft sõnumi Analyzer abil konfigureerimine

Konfigureerida web jälgimine seanssi HTTP ja HTTPS liikluse Microsoft sõnumi Analyzer abil, käivitage rakendus Microsoft sõnumi Analyzer ja klõpsake menüü **fail** **Jäädvustada/jälgi**. Valige loendis Saadaolevad Jälita stsenaariumid, **Veebiteenuse puhverserveri**. Klõpsake paanil **Jälita stsenaarium konfiguratsiooni** tekstiväljale **HostnameFilter** lisada nimed (saate vaadata nende nimesid [Azure portaali](https://portal.azure.com)) salvestusruumi lõpp-punktide. Näiteks kui teie Azure storage konto nimi on **contosodata**, lisage järgmine **HostnameFilter** tekstiväli:

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Tühikumärgi eraldab selle hostinimed.

Kui olete valmis alustama Jälita andmete kogumise, klõpsake nuppu **Käivita** .

Microsoft sõnumi Analyzer **Veebiteenuse puhverserveri** Jälita kohta leiate lisateavet teemast [Microsofti-PEF-WebProxy pakkuja](http://technet.microsoft.com/library/jj674814.aspx).

Sisseehitatud **Veebiteenuse puhverserveri** Jälita rakenduses Microsoft sõnumi Analyzer põhineb viiuldaja; See jäädvustada kliendipoolne HTTPS-liikluse ja krüptimata HTTPS sõnumite kuvamiseks. **Veebiteenuse puhverserveri** Jälita töötab kohaliku puhverserveri kõik HTTP ja HTTPS-liikluse, mis annab juurdepääsu krüptimata sõnumitele konfigureerimisega.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnoosimise võrguprobleemide Microsoft sõnumi Analyzer abil

Lisaks Microsoft sõnumi Analyzer **Veebiteenuse puhverserveri** Jälita abil saate jäädvustada üksikasjad HTTP-või HTTPs-liikluse klientrakenduse ja salvestusruumi teenuse vahel, saate kasutada ka sisseehitatud **Kohaliku Link Layer** Jälita jäädvustada võrgu paketi teavet. See võimaldab teil jäädvustada ja Wiresharkis jäädvustada ja diagnoosimine mis sarnaneb andmete võrgu probleemid nagu lähevad paketid.

Järgmine pilt kuvatakse näide **Kohaliku Link Layer** Jälita mõned **teatised** sõnumitega **DiagnosisTypes** veerus. Klõpsates ikooni **DiagnosisTypes** veerus kuvatakse sõnumi üksikasjad. Selles näites kaabellevivõrkudesse server sõnumi #305, kuna see ei saanud kinnituse kliendi:

![][9]

Microsoft sõnumi Analyzer Jälita seansi loomisel saate määrata filtrid, et vähendada müra jälitus. Kui määratlete jälitus **jäädvustada ja jälgida** lehel klõpsake linki **konfigureerimine** **Microsoft Windows – NDIS PacketCapture**kõrval. Järgmine pilt on kuvatud konfiguratsiooni, mis filtreerib TCP liiklust kolm salvestusruumi teenuste IP-aadressid.

![][10]

Microsoft sõnumi analüsaator kohaliku Link Layer Jälita kohta leiate lisateavet teemast [Microsofti PEF-NDIS PacketCapture pakkuja](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Lisa 4: Exceli kasutamise mõõdikute vaatamiseks ja logige andmed

Mitme tööriistad lubada teil laadida talletusruumi mõõdikute andmete Azure'i tabelimälu eraldatud vormingus, mis hõlbustab laadite andmed otse Excelisse vaatamiseks ja analüüsimiseks. Logimise andmed, mille kaudu Azure'i bloobimälu on juba eraldatud vormingut, mida saate laadida Excelisse. Siiski peate lisada veergude päised [Salvestusruumi Analytics Logi vorming](http://msdn.microsoft.com/library/azure/hh343259.aspx) ja [Salvestusruumi Analytics mõõdikute tabeli skeemi](http://msdn.microsoft.com/library/azure/hh343264.aspx)teabe põhjal.

Salvestusruumi logimine andmete importimiseks Exceli pärast allalaadimist bloobimälu:

- Klõpsake menüü **andmed** nuppu **Tekstist**.
- Liikuge sirvides logifaili, mida soovite vaadata, ja klõpsake nuppu **impordi**.
- Samm 1 **Tekstiimpordiviisardi**, valige suvand **eraldajatega**.

Klõpsake samm 1 **Tekstiimpordiviisardi**, valige ainult eraldajana **semikoolon** ja kahekordne-hinnapakkumise **tekstitäpsustiks**nimega. Klõpsake nuppu **valmis** ja valige, kuhu andmeid oma töövihikus.

### <a name="appendix-5"></a>Lisa 5: Jälgimise rakenduse ülevaated Visual Studio meeskonnatöö teenuste jaoks

Saate rakenduse ülevaated funktsiooni Visual Studio meeskonnatöö teenuste osana jõudlus ja kättesaadavus jälgimine. Selles tööriistas teha järgmist.

- Veenduge, et teie veebiteenus pole saadaval ja reageeri. Kas teie rakendus on veebisaidi või seadme rakendus, mis kasutab veebiteenuse, see Testige oma URL-i iga mõne minuti järel kogu maailmas asukohtadest ja teile teada, kui seal on probleem.
- Kiiresti diagnoosida erandid oma veebiteenuse ega jõudlusprobleeme. Teada saada, kui CPU või muud ressursid tõmmatud saada virnas jälgi erandid ja hõlpsalt otsida log jälgi. Kui rakenduse jõudlus langeb alla piiridesse, oleme teile saata meilisõnumi. Saate jälgida nii .NET ja Java veebiteenused.

Rohkem teavet leiate [mis on rakenduse ülevaated?](../application-insights/app-insights-overview.md).

<!--Anchors-->
[Sissejuhatus]: #introduction
[Sellest juhendist korraldamine]: #how-this-guide-is-organized

[Teie salvestusteenus jälgimine]: #monitoring-your-storage-service
[Teenuse seisundi jälgimine]: #monitoring-service-health
[Võimsus jälgimine]: #monitoring-capacity
[Kättesaadavus jälgimine]: #monitoring-availability
[Jõudluse jälgimine]: #monitoring-performance

[Diagnoosimise salvestusruumi probleemid]: #diagnosing-storage-issues
[Teenuse seisundi probleeme.]: #service-health-issues
[Jõudlusega seotud probleemide]: #performance-issues
[Diagnoosimise tõrked]: #diagnosing-errors
[Salvestusruumi emulaator probleemid]: #storage-emulator-issues
[Salvestusruumi logimine tööriistad]: #storage-logging-tools
[Võrgu logimine tööriista abil]: #using-network-logging-tools

[Lõpuni jälgimine]: #end-to-end-tracing
[Arvega logiandmed]: #correlating-log-data
[Kliendiid taotlus]: #client-request-id
[Serveri taotluse ID]: #server-request-id
[Ajatemplid]: #timestamps

[Juhised tõrkeotsing]: #troubleshooting-guidance
[Mõõdikute AverageE2ELatency kõrge ja madala AverageServerLatency kuvamine]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Mõõdikute kuvada madala AverageE2ELatency ja madala AverageServerLatency, kuid kliendi esineb pika latentsusajaga]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Mõõdikute kuvamine kõrge AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Teil on probleeme ootamatu viivitust sõnumi kohaletoimetamise järjekord]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Mõõdikute kuvamiseks PercentThrottlingError suurendamine]: #metrics-show-an-increase-in-PercentThrottlingError
[PercentThrottlingError puhul]: #transient-increase-in-PercentThrottlingError
[Püsivate tõusu PercentThrottlingError tõrge]: #permanent-increase-in-PercentThrottlingError
[Mõõdikute kuvamiseks PercentTimeoutError suurendamine]: #metrics-show-an-increase-in-PercentTimeoutError
[Mõõdikute kuvamiseks PercentNetworkError suurendamine]: #metrics-show-an-increase-in-PercentNetworkError

[Kliendi on sõnumeid HTTP 403 (keelatud)]: #the-client-is-receiving-403-messages
[Kliendi on sõnumeid HTTP 404 (ei leitud)]: #the-client-is-receiving-404-messages
[Kliendi või muu protsess varem kustutatud objekti]: #client-previously-deleted-the-object
[Ühiskasutusse antud juurdepääs allkirja (SAS) autoriseerimine probleem]: #SAS-authorization-issue
[Kliendipoolne JavaScripti koodi ei ole objekti juurdepääsu õigus]: #JavaScript-code-does-not-have-permission
[Võrgu tõrge]: #network-failure
[Kliendi on sõnumeid HTTP 409 (konflikt)]: #the-client-is-receiving-409-messages

[Mõõdikute kuvamine madal PercentSuccess või analytics Logi kirjed on tegevuse ClientOtherErrors koos olek]: #metrics-show-low-percent-success
[Võimsus mõõdikute kuvada ootamatud suurendada mäluruumi võimsus kasutamine]: #capacity-metrics-show-an-unexpected-increase
[Teil on probleeme ootamatu taaskäivitamisega Virtuaalmasinates, millele on manustatud VHDs suure hulga.]: #you-are-experiencing-unexpected-reboots
[Teie probleemi tuleneb abil salvestusruumi emulaator või test]: #your-issue-arises-from-using-the-storage-emulator
[Funktsioon "X" salvestusruumi emulaator ei tööta]: #feature-X-is-not-working
[Tõrge "väärtus üks HTTP päised ei ole õiges vormingus" salvestusruumi emulaator kasutamisel]: #error-HTTP-header-not-correct-format
[Töötab salvestusruumi emulaator nõuab administraatoriõigusi]: #storage-emulator-requires-administrative-privileges
[Ilmneb probleeme Azure'i SDK .net-i jaoks]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Teil on erinev küsimus salvestusruumi teenuse]: #you-have-a-different-issue-with-a-storage-service

[Lisa]: #appendices
[Lisa 1: Viiuldaja abil jäädvustada HTTP ja HTTPS-liikluse]: #appendix-1
[Lisa 2: Abil Wiresharkis võrguliiklust jäädvustada]: #appendix-2
[Lisa 3: Microsoft sõnumi Analyzer abil võrguliiklust jäädvustada]: #appendix-3
[Lisa 4: Exceli kasutamise mõõdikute vaatamiseks ja logige andmed]: #appendix-4
[Lisa 5: Jälgimise rakenduse ülevaated Visual Studio meeskonnatöö teenuste jaoks]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
