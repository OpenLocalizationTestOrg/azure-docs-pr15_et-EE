<properties 
    pageTitle="Azure'i otsingu arendaja juhtumianalüüsi: Kuidas WhatToPedia mõne olevad portaali tugineb Microsoft Azure'i | Microsoft Azure'i | Majutatud pilveteenuses otsing" 
    description="Saate teada, kuidas koostada teabe portaali ja metaandmete otsingu mootori otsinguga Azure, cloud majutatud otsinguteenuse arendajatele." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Juhtumianalüüsi Azure otsingu arendaja

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Kuidas koostada [WhatToPedia.com](http://whattopedia.com/) Microsoft Azure on olevad portaal

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Suur idee</font> 


Meie idee on teavet portaali, mis aitab ostukottide jaemüüjatega väga oluline, kus otsinguulatuseks on määratud otsingu kogemuse, kuidas liikuda portaalide match turiste üles hotellist, restorani ja meelelahutus kui kaardistamata piirkonnaväljad sarnase kaudu ühenduse luua. 

Me näevad ette portaali toob väga kõrge kvaliteediga otsing kogemus üle edasimüüjalt andmete antud market, aidates ostukottide leida poed asukoht ja muud mugavused selle edasimüüja pakub. Me ei seemne otsingumootori koos mõne algse andmekomplekti, kuid süvitsi väärtus ehitatakse aja jooksul edasimüüjalt tellijad, kes postitada oma ettevõtte teavet abil. Reklaamid, uute toodete, populaarsed kaubamärkide ettevõttesiseseid tellimustoodete-– kõik on andmeid, mis lisab väärtust meie saidi näited. Andmed on ise aru ja integreeritud otsing corpus, kui selle edasimüüja registreerub nagu klienti. Reklaami pluss tellimuse mudel, ette meie uut business tulu voo.

Otsingu saab põhiline kasutaja suhtluse mudeli puhas pilve platvormi. Selleks skaala ja madal – maksab, saab peaaegu kõike, mida me andmeallika juhtelemendi, portaali kogemusest veebiteenusesse kaudu. Abil otsimootor, mis pakub funktsioone, mida läheb vaja välja kasti, saame luua otsingurakenduse kiiresti, ilma et luua ja hallata otsingu mootor ise.

## <a name="what-we-built"></a>Mida me ehitatud

WhatToPedia on käivitamine olevad ettevõte. [WhatToPedia.com](http://whattopedia.com/) – ehitatud - praegu testi-Marketi Põhja-Euroopa 2 veebruar 2015 minge live kuupäevaga. Meie klientide on peamiselt tellistest segu poed, kellel on vaja taskukohane veebikeskkonna, mis on lihtne hallata ja säilitada.

Meie ülesanne on meelitada ostukottide hea veebiotsing kogemuse kaudu, suurendada põhised tulemused linna või naabruskond, kaubamärkide nimed, või salvestatavat tüübid. Meelitada ostukottide on Doominoefekt, motiveeriv jaemüüjad tellida meie portaalisaidile. Paketid on tasulised, mõistliku määra.

 ![][7] 

Pärast tellimuse kasutajaks, jaemüüjalt võtab üle oma olemasolevale profiilile (loodud algselt meile ostetud andmete põhjal), selle reklaamid, esiletõstetud kaubamärkide või teadaannete kohta täiendavate andmete värskendamist. Majasisesed võimalusi, nt keelt, valuutade vastu, maksuvabade ostmine, võib olla ise aru paremini meelitada ostjaid, kes soovivad neid mugavusi.

## <a name="who-we-are"></a>Kes oleme

Mu nimi on Thomas Segato (Microsoft Consulting) ja ma töötanud Jesper Boelling WhatToPedia kujundamise lahendus arendajate juhtimine. 

WhatToPedia on käivitamine, turundus selle uue portaali äri, Rootsi, kus enamik 60 000 jaemüüjaid on tellistest segu VKEd (väikestele ja keskmise suurusega ettevõtetele) test. Kuna me tean, et isiku ostmine Euroopa mitut keelt ja viib mitut valuutat, saame koostada lahendusi, mis majutada mitmekeelsete kandekott. Me vaja ja leidnud, otsimootor, mis toetab meie mitmekeelsete nõuded Azure'i otsing.

Azure'i otsing oli aeg-Box meie projekti. Enne Azure'i otsingu olemasolu me tühi märkimisväärne kujutatud töö kiiksud koostamise oma otsingumootori kaudu. Võttes Azure'i otsing, mida veebiteenusesse suurim tehnilise ja sammu eemaldatakse meie lahenduse, mis on mõeldud kaustadesse kiiremini ja senisest käepärasemad otsing kogemus.  

## <a name="how-we-did-it"></a>Kuidas me see

Meie nägemine oli täielik infrastruktuur pilveteenustega ainult põhjal koostada. Microsoft valiti strategic platvormi, kuna see on pakkuja, mis pakub vajalikud teenused (nii koostöö ja arengu), mastaapimiseks nõudmisel ja taskukohase.
 
### <a name="high-level-components"></a>Üksikasjalik komponendid

Meil ehitatud äri, mitte ainult saidi. Kogu vaeva toetavad kõiki tööriistade ja rakenduste nõutav. Oleme vastu Visual Studio ja Visual Studio Team Services arengu, Team Foundation teenuse (TFS) Online andmeallika juhtelemendi ja tungos haldamine, Office 365 suhtlus-ja koostööfunktsioonide ja loomulikult Microsoft Azure'i kõik saidi seotud toiminguid ja salvestusruumi. Visual Studio IDE tingimusel otsese ettevalmistamise Azure'i integreerimise TFS online esitada täiendavad tööviljakust suurendada.

Alloleval joonisel on näidatud kasutatavad WhatToPedia taristu üksikasjalik komponendid.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Microsoft Azure'i kasutamine

Vaadates eelmise skeemi roheline väljadele, näete, et need teenused on ehitatud WhatToPedia lahendus:

- [Azure'i otsing](https://azure.microsoft.com/services/search/)
- [Azure'i veebisaitide abil MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure'i WebJobs ajastatud toimingute tegemiseks.](../app-service-web/websites-webjobs-resources.md)
- [Azure'i SQL-andmebaas](https://azure.microsoft.com/services/sql-database/)
- [Azure'i bloobimälu](https://azure.microsoft.com/services/storage/)
- [SendGrid meilisõnumite kohaletoimetamise](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

Keelevälja lahendus on andmed ja otsing. Edasimüüja pakkuja klientidele andmete voogu on näidatud allpool:

  ![][9]

Peamine andmete talletamine on edasimüüja ja raamatupidamisandmetest Azure SQL-andmebaasis. See koosneb algse andmekomplekti pluss edasimüüjalt andmete aja jooksul lisatud. SQL-andmebaasi värskenduste postitamine otsingu corpus Azure'i otsingus kirjutit on Azure WebJob.

### <a name="presentation-layer"></a>Esitluse kiht

Portaal on Azure veebisaidi MVC 4 ja [Twitteri alglaaduri](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29)rakendatud. Valisime MVC, sest see pakub cleaner lähenemine HTML-i kui ASP.net-i vormipõhist areng. Mitmes seadmes rakendused luua ja hallata mitme mobiilsideseadmete kaudu vältida valitud Twitteri alglaaduri kõik seadmed ja platvormid.

### <a name="authentication-permissions-and-sensitive-data"></a>Autentimine, õiguste ja tundliku teabe

Ostukottide sirvida saidi anonüümselt. Sellisel kujul puuduvad sisselogimise nõuded ostukottide ega me ei Salvesta tarbija andmeid. 

Jaemüüjad on teine lugu. Siin me salvestada avalikkusele suunatud profiiliteavet ja arveldusteabe meediumisisu, mis soovivad saidi esitamist. Iga edasimüüjalt, kes toetab saidil saada kasutajanime, kasutatakse autentida enne värskenduste tegemist abonendi profiili.  Turvaliselt säilitada kõik abonendi andmed Azure'i SQL-andmebaasi ja Azure'i BLOOBIMÄLU salvestusruumi.
Oleme valinud autentimise mudelit, mis põhineb .NET vormipõhist autentimist. Valisime lähenemisviisi oma lihtne; Me ei pea rollid, Kasutajaliidese tugi ja muud kõrvalised funktsioonid, mis on veel mitu. 

Jaemüüjad näha ainult andmeid, mis kuulub neile tagamiseks lõime edasimüüjalt, iga edasimüüjalt, mida kasutatakse edaspidi kõigis ID lugemine ja kirjutamine toimingutega edasimüüjalt seotud andmed. Sellise lähenemise korral leidsime, et me ei vaja andmebaasi õigusi anda üksikud jaemüüjad. Jaemüüjatest suhelda süsteemi jaotises ühe andmebaasi rolli, kui meie andmete eraldamise tehnika edasimüüjalt ID-ga.

Kuna meie business on kõike järgneval efektid (juhtimise rohkem äri jaemüüjatele, loomise reklaamida ja tellida) me saate tõmmata aadressil töötlemise oste veebis. Te ei leia meie saidil, mis lihtsustab meie turbenõuetele ostukorv. 

Mõne muu lihtsustamine kasutatud on meie arveldus- ja kontode maksmisele kuuluvate toimingute tellida. Marsruutimise kliendi makse teabe otse muude tootjate poolt ([SveaWebPay](http://www.sveawebpay.se/)) me riske talletamine ja meie andmete poed tundliku loomuga andmete kaitsmine seostada. 

### <a name="search-engine"></a>Otsimootori

Meie lahendus keskmes on tugineb Azure'i otsinguteenuse otsingumootori. Esialgu me sisseehitatud kohandatud otsingumootori, kuid selle käigus aru saanud keerukus ja vaeva on väga kõrge ja mis küsitakse meile kaaluda muid alternatiive. 

Põhilised funktsioonid, mis olid kõige olulisem kaasata.

- Filtrid
- Lihvitud navigeerimine
- Tulemuste suurendamine
- AJAXI lehekülgede sirvimine

Interneti-otsingu tõi meile meid Azure'i otsingu proovida järgmisi video: [Deep Dive TechEd Euroopa](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Pärast vaatame video, me ei põhjal, mida me kuvati prototüüp valmis. Kuna andmemudeli oli juba MVC, loomise prototüüp oli lihtne, sest sisaldas andmeid otsitavate termineid ja me töötasid juba nõuded kuidas me kalendriüksusi sortimiseks, fassettidega, ja andmete filtreerimine. 

See on, kuidas me ehitatud prototüüp.

**Azure'i otsinguteenuse konfigureerimine**

1. Azure'i klassikaline portaali sisse logida ja otsinguteenus meie tellimusele lisada. Kasutasime ühiskasutusega versioon (tasuta meie tellimus).
2. Registri loomine. Prototüüp, kasutasime portaali Kasutajaliidese määratlemine otsinguväljad ja hinded külastatavate loomine. Meie hinded profiil põhineb asukohaandmete: riik | linna | aadress (vt: lisada hinded profiilid).
3. Kopeerige URL-i ja administraatori api võti meie failid. See võti on portaalis lehel teenuse ja seda kasutatakse teenusesse autentida.
    
**Välja Otsing indekseerija töö – Windows konsooli**

1. Lugege kõik edasimüüjate andmebaasist.
2. Azure'i otsingu teenuse API üles laadida edasimüüjate ükshaaval kõne (vt: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Määrata atribuut andmebaasis edasimüüja on indekseeritud suureneva indekseerimise. Me ei seda, lisades "indekseerija" välja, mis talletab indekseerimise olek iga profiili (indekseeritud või mitte). 

Vt. koodilõigu, mis põhineb indekseerija töö jaoks.

**Välja Otsing veebiportaali – MVC**

1. Kõne Azure'i otsinguteenuse saada kõik dokumendid otsing (vt: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Ekstrakti pärast otsingu teenuse vastuse (kasutades json.net http://james.newtonking.com/json)
   - Tulemused
   - Omadustega
   - Tulemite arvud
   - Välja kasutajaliidese otsingutulemite, omadustega ja loendab (meil oli juba).

See on kasutasime tulemuse saamiseks Azure'i otsingust kood:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Asukoha suurendamine**

Tõenäoliselt kõige olulisem nõue otsingusõna asukoha lisamine päringusse kaasata prototüüp kinnitamiseks. Oluline on meie portaali, et kui kasutaja sisestab linna nimi otsinguväljale päring, mis edasimüüjate antud linna soovite järjestada kõrgem edasimüüjate võttes linna märksõna kirjeldus. Selle nõude kasutasime hinded profiili välja suurem kui muude väljade linn järjestada.

**Toetab mitut keelt**

Meil on vaja õige otsingutulemite kuvamiseks õiges keeles ja võimalus erinevates keeltes sama tulemuse leidmiseks. Sellele probleemile mõlemast olid: 

- Mitme keele sõnade otsimine
- Kuva otsingutulemid õiges keeles

Meil lahendatud, lisades iga keele lokaliseeritud tekstiga ja atribuudi keeles dokumendi osa esitlus. Kui kasutaja sisestab otsingutermin, me kasutaja `$filter` avaldiste filtreerimiseks keele kasutaja on valitud.

Iga dokumendi on peidetud atribuut nimega "linnad" tugineb saidikogumi tüüp. Atribuut salvestab kõigi keelte, mis võimaldab kasutajal otsida mitmes keeles linna nimed.

###<a name="data-storage"></a>Andmete talletamine

SQL-andmebaasi talletatakse kõik andmed (profiil, tellimus ja raamatupidamine). Azure'i bloobimälu, sh pilte ja videoid, mis on esitatud selle edasimüüja salvestatakse kõik meediumifaile. Kasutades eraldi bloobimälu isoleerib efektid failide saatmisel; failid on kunagi koostööd segatud veebisaidiga, et me ei pea iga kord, kui me failide lisamine saidile uuestiloomiseks.

Meie salvestusruumi kujundusega olulist kasu on mitu arendajate saate ühiskasutusse anda ühtse arendamise salvestusruumi. Üks WhatToPedia Projecti nõuded oli võimalik luua mõne arenduskeskkond 15 minuti jooksul, sh edasimüüja andmeid, pilte ja videoid. Saada värskeimad andmed TFS Online, SQL-i skripti ja töö importimine, täielik keskkond saate olla kaitsnud ei aja üldse. See harjutus parandab ka peatuspaik protsess.

###<a name="webjobs"></a>WebJobs

Azure'i WebJobs abil registri andmete värskendamine. Otsingu indekseerija töö loomisega indekseerimise osa on väga lihtne integreerida meie lahendus. Koodi ainult me tehtud muudatus oli mahutamiseks indekseerija töö lisamiseks on `Indexed` välja indeksi kujul näitamaks meie andmemudelisse. Iga kord, kui uus profiil on lisatud või uuendatud, on `Indexed` väärtuseks on false. Sama kehtib juhul, kui selle edasimüüja muudab oma profiili andmeid portaali kaudu.  

Töö käigus otsitakse kõik read on `Indexed` väärtuseks false. Rea leidmisel sisestatakse dokumendi Azure'i otsing, ja seejärel soovitud `Indexed` väärtuseks on seatud väärtusele tõene. Me ei pea kavandamine ja andmete värskendamine, kuna Azure'i otsinguteenuse tegelikult hoolitseb selle lisamist. Kui lisate dokumenti, mis on juba olemas, teenuse värskendust automaatselt teha.

Kõik web tööd on välja töötatud konsooli rakendusi, mida saab Azure veebisaitide ZIP faile üles laadida, lahti pakitud ja seejärel ajastatud.

Töö on plaanitud ajastatud web tööülesandena iga 5 minuti käivituma. Me arvutatud, et teenuse võtab aega umbes kolm minutit üleslaadimiseks 3000 dokumendid, mis oli meie nõuetele. 

> [AZURE.NOTE] On prototüüp indekseerija funktsioon, mis võeti hiljuti kasutusele Azure'i otsing. See funktsioon oli hilinenud meile kasutamine meie esmaväljaande, kuid see kuvatakse probleemi lahendamiseks sama kasutasime indekseerija tööd, mis on andmete laadimine automatiseerimiseks.


###<a name="backup-strategy"></a>Varukoopia strateegia

Lõime mitmerajalisest varukoopia strateegia mitmesuguseid stsenaariumid katastroofiline tõrke, allapoole üksikute toimingu taastamine taastada. Vara kaitsmiseks sisaldavad kolme tüüpi andmed (veebisait, abonendi andmed ja meediumifailide). 

Esmalt veebisaidi lähtekoodi hoides TFS Online'is me teada, et kui saidi läheb alla, saame saate taastada selle uuestipublitseerimist TFS kaudu. 

Azure'i SQL-andmebaasi abonendi andmed on kõige tundliku vara. Oleme tagasi selle üles kasutades sisseehitatud funktsioonide (vt teemat [Azure SQL-i andmebaasi varundamine ja taastamine](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Varunduse ajakava on täieliku andmebaasi varukoopia kord nädalas ja erinevat andmebaasi varundamine kord päevas tehingu varukoopiate iga 5 minuti järel.  Antud andmete maht, see lahendus on rohkem kui piisavad meie vahetu ja plaanitud andmemahtudega.

Kolmas, saame talletada pildi-ja videofailid Azure'i bloobimälu. Me ikka veel hinnata ultimate varukoopia kavandamine andmed, arvestades muraka Explorer Azure on võimalik lahendus. Nüüd kasutame on WebJob pilte ja videoid kopeerimiseks mõnda muusse asukohta.

##<a name="what-we-learned"></a>Mida me õppinud

Kuna me juba andmeid, on lihtne luua tõendada mõiste. Tunni jooksul, tuli koos omadustega prototüüp hinnale, Piipar järjestatud profiilid ja otsingutulemite. Otsingutulemite olid nii täpsed, otsustasime osa end kliendile filtrid eemaldada. 

Suurim ebameeldivusi meile oli, kui kiiresti me õppida Azure'i otsing, ja kui palju meil tagasi. Sõna-sõnalt me käivitatud tõendada mõiste mõne tunni pärast (vt allpool märkust), asendades 500 koodiread 3 koodiread esiosa rakendus (pluss uue WebJob) ja parema tulemuse. 

Varem meie kood rakendada, lehtede nummerdamine, loendab ja muud käitumised, mis on standardne otsimiseks. Azure'i otsingu abil tulemused, saame tagasi sisaldavad otsingu tabamust, omadustega, Piipar andmete loendab--kõik asjad, mida me vaja ja esitama ise on. See on ka suurendada ja sisseehitatud lihvitud navigeerimine, mida me ei ole meie algne lahendus.

Suurim probleem rakendamisel oli, et see on eelvaateversiooni ja teabe leidmist ja jagada kogemusi on keeruline. Kui me ühendatud mõne punkti, leidsime Azure'i otsinguteenuse abil on üsna lihtne REST API ja JSON andmete vormingu tõttu. Me nimedeks raames otse viimati avatud allika lisandmoodulid nagu JQuery JSON.Net ja võiks kasutada tööriistu, nagu viiuldaja kiiresti katsetamine ja silumine. 

> [AZURE.NOTE] Lisaks prepped andmed, aitas, et need ehitada prototüüp juba mõista kuidas otsing tehnoloogia töötab, muutes meile veel tõhusamalt töötada, ja rohkem tänulikud sisseehitatud funktsioonid. Kui teil on vaja ramp üles, klõpsake otsingu päringu ehituse lihvitud navigeerimine, filtrid jne peaksite prototüüpimine kauem. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Esitluse lehel omadustega juhtimine

Üks meie õppetunnid ajal tõendada mõistet oli omadustega hoolikalt algusest peale kavandada. Pärast hulga andmete laadimine lahenduse, me kuvati, et õhuke omadustega oli esitada kasutajaid liiga suur. 

Me lahendada see piirates tahukas count parameeter. Count parameetri paneb raske piirarvu kasutajale tagastada aspekte. Lingi, mis sisaldab arutelu count parameetri leiate [siit](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>WebJobs plaanimiseks tööülesanded

Azure'i otsing ei olnud ainult meeldiv ebameeldivusi meile. Me avastanud WebJobs abil automatiseerida Azure'i otsimiseks meie andmete laadimine on väga parem eelmise lähenemisviisi, mis kaasnevad kasutamine opsüsteemi Windows Scheduler otsinguregistri värskendamise kavandatud ülesannetega sihtotstarbeline VM. WebJobs oli lihtsama konfigureerimine ja lihtsam silumine ja loomulikult palju odavam maksmata sihtotstarbeline VM.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure'i BLOOBIMÄLU salvestusruumi Exploreri värskendamise pildid

Leidsime, et [Azure'i BLOOBIMÄLU salvestusruumi Explorer](https://azurestorageexplorer.codeplex.com/) (saadaval codeplexis) abil väga kasulik pilt- ja värskendustega saidi haldamine. Me kasutame seda arendaja tööriist pilte ja videoid, mis kuuluvad meie põhisait käsitsi värskendamiseks. Meil tuleb paindlikumad juurutamine muudatused portaali ja kõrvaldab katse iteratsiooni iga kord, kui peame värskendada pilt. 

##<a name="a-few-final-words"></a>Lõplik paar sõna

Tänu hea inimesed on WhatToPedia võimaldab meil jagada oma lugu!  

Loodame, et olete leidnud selle juhtumianalüüsi kasulik. Kui lähete kasutada Azure otsingut, ma soovita kiirus saate mööda mõned ressursid.

- [MSDN-i Azure otsingu Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow on ka sildi](http://stackoverflow.com/questions/tagged/azure-search)
- [Dokumentatsiooni lehel Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure'i otsingu dokumentatsiooni MSDN-is](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Lisa: Otsingu indekseerija WebJob

Järgmine kood koostab indekseerija, hoone prototüüp jaotises kirjeldatud.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
