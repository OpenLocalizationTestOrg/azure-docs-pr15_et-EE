<properties
   pageTitle="Tausta töö juhised | Microsoft Azure'i"
   description="Juhised tausttoimingud, mis töötavad sõltumata kasutajaliidese."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Tausta töö juhised

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Ülevaade

Mitut tüüpi rakenduste jaoks on vaja tausttoimingud, mis töötavad sõltumata kasutajaliidese (UI). Näiteks pakett-tööde, kapitalimahukas töötlemine tööülesannete ja pikaajalisi protsesse, nagu töövood. Tausta töö saab teostada kasutaja suhtluse--rakenduse nõudmata saate alustada tööd ja jätkake interaktiivsed taotlused kasutajate eest. See aitab minimeerimiseks laadi rakenduse kasutajaliides, mis võivad parandada kättesaadavust ja vähendada interaktiivsed vastuse korda.

Näiteks kui rakendus on vaja luua pisipildid pilte, mis on üles laaditud kasutajad, saate selle tausta tööd teha ja salvestusruumi pisipilti salvestada, kui see on lõpule jõudnud – ilma kasutaja vajavad ootama protsessi lõpule viia. Samal viisil tellimuse kasutaja saab algatada tausta töövoo, mis töötleb tellimus, samal ajal UI võimaldab kasutajal veebirakenduse sirvimise jätkamiseks. Kui tausta töö on valmis, saate salvestatud tellimuste andmeid värskendada ja saata e-posti, mis kinnitab tellimuse kasutajale.

Kui mõelda, kas kasutusele tööülesande tausta tööd on peamised kriteeriumid, kas tööülesande käitamise ilma kasutaja suhtluse ja UI ootama töö täidetakse automaatselt. Ülesanded, mis nõuavad kasutaja või UI oodake, kuni need on lõpule viidud ei pruugi taustal töökohtade korral.

## <a name="types-of-background-jobs"></a>Taustal töökohtade tüübid

Taustal töökohtade järgmised tavaliselt üks või mitu järgmist tüüpi tööde haldamine.

- Protsessori jaoks mahukat tööd, nt matemaatilisi arvutusi või strukturaalset mudeli analüüs.
- I/O-nõudvaid tööd, nt rida salvestusruumi tehinguid täidetavate või failide indekseerimine.
- Pakett-tööde, nt öösel andmete värskendamine või ajastatud töötlemine.
- Pikaajalisi töövood, nt tellimuse täitmine või teenuste ja süsteemide ettevalmistamise.
- Kui tööülesande antakse välja veel turvalises asukohas töötlemiseks tundlik teave töötlemine. Näiteks võite ei soovi web appi tundliku loomuga andmete töötlemiseks. Selle asemel võite kasutada näiteks [Gatekeeperi](http://msdn.microsoft.com/library/dn589793.aspx) mustri isoleeritakse tausta protsessi, kellel on juurdepääs kaitstud salvestusruumi andmeid edastada.

## <a name="triggers"></a>Päästikute

Tausta töö saab algatada mitmel erineval viisil. Nad kuuluvad ühte järgmisest kategooriast:

- [**Päästikute sündmuse põhinev**](#event-driven-triggers). Tööülesande alustamist vastuseks sündmuse tavaliselt kasutaja või juhises töövoo toimingu.
- [**Päästikute ajakava põhinev**](#schedule-driven-triggers). Tööülesande kutsumisel põhjal on timer ajakava. See võib olla ajakava korduva või ühekordse kutsumise, hiljem määratud.

### <a name="event-driven-triggers"></a>Päästikute sündmuse põhinev

Sündmuse põhinev kutsumise kasutab käivitamiseks tausta toiming. Sündmuse põhinev päästikute kasutamine näited.

- Kasutajaliidese või mõne muu töö paigutab sõnumi järjekorda. Sõnum sisaldab toimingut, mis on toimunud, nt tellimuse kasutaja andmeid. Tausta tööülesande kuulab selle järjekorra ja tuvastab uue sõnumi saabumisel. See loeb ja kasutab andmeid ei sisendina tausta töö.
- Kasutajaliidese või mõne muu töö salvestab või värskendab salvestusruumi väärtuse. Tausta tööülesande jälgib talletamist ja tuvastab tehtud muudatused. See loeb andmeid ja kasutab seda sisendina tausta töö.
- Kasutajaliidese või mõne muu töö esitab taotluse lõpp, nt mõni HTTPS URI või API, mis on esitatud veebiteenuse. See edastab andmed, mida on vaja ülesande tausta taotluse osana. Teenuse lõpp-punkti või web tugineb tausta tööülesande, mis kasutab sisendina oma andmed.

Tüüpilised ülesandeid, mis sobivad sündmuse põhinev kutsumise näiteks pilditöötluse, töövood, remote teenuste teabe saata, e-posti sõnumite saatmiseks ja ettevalmistamise uute kasutajate rentnikuga.

### <a name="schedule-driven-triggers"></a>Päästikute ajakava põhinev

Ajakava põhinev kutsumise kasutab mõnda timer tausta toiming. Päästikute ajakava põhinev kasutamise näited:

- Ajastiteenuse, mis töötab kohalikult rakenduses või rakenduse operatsioonisüsteemi osana käivitab regulaarselt tööülesande tausta.
- Ajastiteenuse, kus töötab mõne muu rakenduse või ajastiteenuse, nt Azure ajasti, saadab taotluse API või veebis teenuse regulaarselt. Teenuse API või web tugineb tausta tööülesanne.
- Eraldi protsessi või rakendus käivitub timer, mis põhjustab tausta tööülesande kasutusele võtta üks kord teatud aja möödumisel või teatud ajal.

Tüüpilised ülesannete ajakava põhinev kutsumise sobivad näiteks Pakktöötlus moodulid (nt värskendamine seotud toodete loendeid kasutajatele nende tehtud käitumise põhjal), tavalised andmetöötlus tööülesannete (nt registrite värskendamiseks või akumuleeritud tulemusi), Andmeanalüüs igapäevased aruanded, andmete säilitamise puhastamine ja andmete vastavuse kontrollimine.

Kui kasutate ajakava põhinev tööülesande, mida tuleb käitada nimega ühekordsest, arvestage järgmist:

- Kui neid on Arvuta eksemplari, kus töötab ajasti (nt virtuaalse masina abil Windowsi toimingud), on teil mitu eksemplari ajasti töötab. Neid ei käivitada tööülesande mitmes eksemplaris.
- Kui tööülesanded käivitada ajasti sündmuste jaoks pikem kui, alustada ajasti muus eksemplaris tööülesande käigus eelmine.

## <a name="returning-results"></a>Tagasta tulemeid

Taustal töökohtade käivitada asünkroonselt eraldi protsessi või isegi eraldi asukohas, UI või protsess, mida kasutada tausta tööülesanne. Ideaalvariandis mitte tausttoimingud on "tule ja unusta" toimingud ja nende täitmise edenemist ei mõjuta UI või helistaja protsess. See tähendab, et helistaja protsessi ootama lõpetamise tööülesanded. Seetõttu ei saa automaatselt tuvastada tööülesande lõppemisel.

Kui vajate tausta tööülesande helistaja ülesande edenemine või lõpetamise suhelda, peate selle süsteemi rakendama. Mõned näited:

- Kirjutage oleku näidik väärtus salvestusruumi, mis on juurdepääsetav UI või helistaja tööülesanne, mille saate jälgida või märkige see väärtus, kui see on nõutav. Muud taustal ülesande peab naasmiseks helistaja andmeid saab asetada sama salvestusruumi.
- Luua UI või helistaja kuulab vasta järjekorda. Tausta tööülesande saate sõnumeid saata järjekorda, mis näitavad olek ja lõpetamise. Tausta ülesande peab tagastama helistaja andmeid saab asetada sõnumid. Azure'i teenus siini kasutamisel saate rakendada selle funktsiooni **OutboundSMTPServeri parameetrid** ja **CorrelationId** atribuudid. Lisateavet leiate teemast [korrelatsiooni teenuse siini vahendajaks sõnumside](http://www.cloudcasts.net/devguide/Default.aspx?id=13029).
- Jätke API- või lõpp-punkt: tausta tööülesannet, mille Kasutajaliidese või helistaja pääsevad oleku teabe saamiseks. Tausta ülesande peab naasmiseks helistaja andmeid saab lisada vastus.
- On tausta tööülesande oleku eelmääratletud punktide või lõpetamisel UI või helistaja API kaudu tagasi helistada. See võib olla sündmuste astmes kohalikult või avalda ja tellida süsteemi kaudu. Koosolekukutse või sündmuse last saab lisada taustal ülesande peab naasmiseks helistaja andmeid.

## <a name="hosting-environment"></a>Majutuskeskkond

Mitmesuguseid eri Azure'i platvormi teenuste abil saate majutada tausttoimingud:

- [**Azure'i veebirakenduste ja WebJobs**](#azure-web-apps-and-webjobs). Saate käivitada kohandatud tööde mitmesuguseid eri tüüpi skriptide või käivitatava programmi raames web appi WebJobs.
- [**Azure'i pilveteenustega veebi ja töötaja rollid**](#azure-cloud-services-web-and-worker-roles). Saate kirjutada koodi rolli, mis käivitab tööülesandena tausta.
- [**Azure'i Virtuaalmasinates**](#azure-virtual-machines). Kui teil on Windows teenus või soovite kasutada Windows Toiminguajasti, on tavaline majutada oma ülesandeid tausta virtual seadet.
- [**Azure'i paketi**](./batch/batch-technical-overview.md). See on platform teenus, mis koosolekut kavandava Arvuta mahukat tööd käitamist virtuaalmasinates hallatavate kogumi ja saab automaatselt skaala arvutada ressursse, mis teie töö vajadustele.

Järgmistes lõikudes kirjeldatud kõigi nende valikute üksikasjalikumalt ja lisada peaksite arvesse võtma, mis aitavad teil valida vastav variant.

## <a name="azure-web-apps-and-webjobs"></a>Azure'i veebirakenduste ja WebJobs

Azure'i WebJobs abil saate käivitada kohandatud töökoha taustal töötada jooksul Azure Web App. WebJobs käivitada oma veebirakenduse raames pideva protsessi. Azure'i ajasti või välise teguritest nagu salvestusruumi plekid ja teatejärjekordi käivitava sündmuse vastuseks käivitada ka WebJobs. Töö saate alustada ja nõudmisel on peatatud ja sulgeda nõtkelt. Kui pidevalt töötava WebJob nurjub, on see automaatselt uuesti. Proovi uuesti ja viga toimingud on konfigureeritav.

Kui konfigureerite mõne WebJob:

- Kui soovite tööd vastamise sündmuse põhinev lisamispäästiku, tuleks nimega **pidevaks**konfigureeritakse. Skripti või programm on salvestatud kausta nimega saidi/wwwroot/app_data/töö/pidev.
- Kui soovite tööd vastamise ajakava põhinev päästik, peaksite konfigureerima selle nimega **käitamist ajakava**. Skripti või programm on salvestatud kausta nimega saidi/wwwroot/app_data/töö/vallandanud.
- Kui valite suvandi **nõudmisel käivitada** töö konfigureerimisel, käivitatakse see sama koodi **käitamist ajakava** suvand käivitamisel.

Azure'i WebJobs töötavad Liivakasti web app. See tähendab, et juurdepääs keskkonna muutujate ja veebirakenduse ühendusstringi, nt ühiskasutusse. Töö on juurdepääs arvutisse, kus töötab töö ainuidentifikaator. Ühendusstringi nimega **AzureWebJobsStorage** pakub juurdepääsu Azure storage järjekorrad, plekid ja tabelite jaoks rakenduse andmete ja juurdepääsu teenuse siini sõnumside ja suhtlus. Ühendusstringi nimega **AzureWebJobsDashboard** pakub juurdepääsu töö toimingu logifailid.

Azure'i WebJobs olla järgmised omadused:

- **Turvalisus**: WebJobs juurutamise identimisteabe web Appi abil kaitstud.
- **Toetatud failitüübid**: saate määratleda WebJobs käsk skriptide (cmd), paketi failid (.bat), PowerShelli skriptide (.ps1) abil, bash shell skriptide (.sh), PHP skriptide (.php), Python skriptide (.py), JavaScripti koodi (js) ja käivitatava programmi (.exe, laiendid ja muud).
- **Juurutamise**: saate juurutada skripte ja Käivitusfailid abil Azure portaali abil [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) lisandmooduli Visual Studio või [Visual Studio 2013 värskenduse 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (saate luua ja juurutada selle suvandiga), [Azure'i WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)abil või kopeerides need otse järgmisest kohast:
  - Jaoks vallandanud täitmise: saidi/wwwroot/app_data/töö/vallandanud / {töö nimi}
  - Pidev täitmiseks: saidi/wwwroot/app_data/töö/pidev / {töö nimi}
- **Logimine**: Console.Out käsitletakse teave (tähistatud). Console.Error käsitletakse tõrge. Diagnostika-ja pääsevad Azure'i portaalis. Saate alla laadida logifailid otse saidile. Need salvestatakse järgmisest kohast:
  - Jaoks vallandanud täitmise: Vfs/andmete/töö/vallandanud/jobName
  - Pidev täitmiseks: Vfs/andmete/töö/pidev/jobName
- **Konfiguratsiooni**: portaali, REST API-ga ja PowerShelli abil saate konfigureerida WebJobs. Konfiguratsiooni fail nimega settings.job juurkaustas sama nimega töö skripti abil saate töö konfiguratsiooni teavet. Näiteks:
  - {"stopping_wait_time": 60}
  - {"is_singleton": tõene}

### <a name="considerations"></a>Kaalutlused

- Vaikimisi WebJobs skaala web appi. Siiski saate konfigureerida töö, et käivitada ühe eksemplari, seades atribuuti **is_singleton** konfiguratsiooni väärtuseks **true**. Ühekordsest WebJobs on kasulik ülesannete jaoks, mida te ei soovi mastaapimiseks või käivitada üheaegselt mitu eksemplari, nt reindexing, Andmeanalüüs ja sarnaseid ülesandeid.
- Minimeerida tööde haldamine veebirakenduse toimimise kohta, et võiksite luua tühja Azure Web Appi näiteks uus rakendus teenuse leping host WebJobs, mis võib olla pikk, kus töötab või ressursside intensiivne.

### <a name="more-information"></a>Lisateave

- [Azure'i WebJobs soovitatav ressursid](./app-service-web/websites-webjobs-resources.md) loetletud palju kasulikke ressursse, allalaadimise ja näidised WebJobs.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure'i pilveteenustega veebi ja töötaja rollid

Tausttoimingud web rolli või eraldi töötaja roll käivitamiseks. Kui teil on otsustada, kas kasutada töötaja roll, kaaluge skaleeritavus ja elastsuse nõuded, ülesande kestus, vabastage sagedus, Turve, tõrketaluvust, väide, keerukus ja loogiline arhitektuur. Lisateavet leiate teemast [Arvutada ressursi konsolideerimise mustri](http://msdn.microsoft.com/library/dn589778.aspx).

On mitu võimalust rakendada taustal töötada jooksul pilveteenustega roll.

- Roll rakendamist **RoleEntryPoint** klassi loomine ja selle meetodite abil saate käivitada tausttoimingud. Tööülesannete käivitage WaIISHost.exe kontekstis. Klassi **CloudConfigurationManager** **GetSetting** meetodi abil saate need laadimine Otsingukonfiguratsiooni sätetest. Lisateavet leiate teemast [elutsükli (pilveteenustega)](#lifecycle-cloud-services).
- Käivitus tööülesannete abil saate käivitada tausttoimingud rakenduse käivitamisel. Jõustamine ülesannete jätkavad taustal, määrake atribuudi **taskType** **taust** (kui te ei tee, rakenduse käivitamisel protsess on peatada ja oodake, kuni tööülesande lõpetamiseks). Lisateavet leiate teemast [käivitada käivitus ülesanded Azure](./cloud-services/cloud-services-startup-tasks.md).
- WebJobs SDK abil saate rakendada nagu WebJobs, mille on algatanud tööülesandena käivitamise tausttoimingud. Lisateabe saamiseks lugege teemat [Azure WebJobs SDK alustamine](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Käivitus tööülesande abil saate installida Windowsi teenus, mis käivitab ühe või mitme tausttoimingud. Peate määrama **taskType** atribuudi väärtuseks **tausta** , et teenus aktiveeritakse taustal. Lisateavet leiate teemast [käivitada käivitus ülesanded Azure](./cloud-services/cloud-services-startup-tasks.md).

### <a name="running-background-tasks-in-the-web-role"></a>Töötab taustal töötada web roll

Töötab taustal töötada web roll eelis on salvestamist hosting kulud, sest puudub nõue juurutamiseks täiendavad rollid.

### <a name="running-background-tasks-in-a-worker-role"></a>Töötaja roll tausttoimingud töötab?

Töötab taustal töötada töötaja roll on mitmeid eeliseid.

- See võimaldab teil hallata skaleerimist iga rolli jaoks eraldi. Näiteks võib juhtuda, mitu eksemplari web rolli praegune koormus toetama, kuid vähem eksemplari töötaja rolli, mis käivitab tausttoimingud. Suuruse muutmise tausta tööülesande Arvuta eksemplarid eraldi kaudu UI rollid, vähendab majutusteenuse kulud, säilitades aktsepteeritav jõudlus.
- See offloads töötlemine pea kohal, tausttoimingud web rolli jaoks. Web rolli, mis pakub UI saate jätkuvalt reageerima ja see võib tähendab, et vähem eksemplarid nõutaks antud maht taotluste kasutajate eest.
- See võimaldab teil rakendada eraldamine probleemid. Iga rolli tüüp saate rakendada määratud selgelt määratletud ja seotud ülesanded. See muudab kujundamise ja säilitades koodi lihtsam, kuna seal on vähem vastastikune sõltuvus kood ja funktsionaalsuse vahel iga rolli.
- See abistab eristamiseks tundliku protsesside ja andmed. Näiteks pole vaja web rollid, UI, et juurdepääs andmetele, mis on ja kontrollitud töötaja roll. See võib olla kasulik tugevdada turvalisus, eriti siis, kui kasutate näiteks [Gatekeeperi mustri](http://msdn.microsoft.com/library/dn589793.aspx)mustri.  

### <a name="considerations"></a>Kaalutlused

Võtke arvesse järgmist valimisel, kuidas ja kus juurutada tausttoimingud pilveteenustega veebi ja töötaja rollid kasutamisel:

- Majutusteenuse tausta tööülesanded olemasolevat web rolli saate salvestada ainult nende toimingute jaoks eraldi töötaja roll kulud. See aga võib mõjutada jõudlust ja kättesaadavust taotluse kui argument töötlemine ja muud ressursid on. Kasutades eraldi töötaja roll kaitseb web rolli mõju pikaajalisi või ressursse tausttoimingud.
- Kui majutate tausttoimingud **RoleEntryPoint** klassi abil, saate selle hõlpsalt veel mõne rolli teisaldada. Näiteks kui loote klassi veebi roll ja hiljem otsustada, et peate käivitama tööülesanded töötaja roll, võite liikuda **RoleEntryPoint** klassi rakendamist töötaja roll.
- Käivitus tööülesanded on loodud käivitada programmi või skripti. Tausta töö käivitatava programmina juurutamine võib olla raskem, eriti siis, kui on vaja ka juurutamise sõltuvad komplektide. See võib olla lihtsam juurutada ja skripti abil saate määratleda tausta töö käivitus tööülesannete kasutamisel.
- Erandid, mis põhjustavad tausta tööülesande nurjumise on erinevate mõju, olenevalt sellest, on majutatud.
  - Kui kasutate **RoleEntryPoint** klassi moodust, põhjustab nurjunud tööülesande rolli taaskäivitama, et tööülesanne käivitub automaatselt uuesti. See võib mõjutada rakenduse kättesaadavus. Selle vältimiseks tagama, et te sisaldavad robustne erandi töötlemise **RoleEntryPoint** klassi ja kõik tausttoimingud. Koodi abil taaskäivitage tööülesandeid, kui see ei õnnestu ja põhjustada erandi ainult juhul, kui te ei saa tagasi oma koodi nõtkelt selle rolli taaskäivitama.
  - Kui kasutate käivitus ülesanded, olete hallata ülesande täitmine ja kontrollimine, kui see ei õnnestu.
- Juhtimiseks ja käivitus tööülesanded on keerulisem kui **RoleEntryPoint** klassi lähenemine. Siiski Azure'i WebJobs SDK sisaldab armatuurlaua lihtsam haldamine WebJobs, kui annate käivitus põhitoiminguid.

### <a name="more-information"></a>Lisateave

- [Arvutage ressursi konsolideerimise muster](http://msdn.microsoft.com/library/dn589778.aspx)
- [Azure'i WebJobs SDK kasutamise alustamine](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure'i Virtuaalmasinates

Tausttoimingud võidakse rakendada viisil, mis takistab neil on juurutatud Azure'i veebirakenduste või pilveteenustega või need suvandid ei pruugi olla mugav. Tüüpilised näited teenuste Windows ja muude tootjate Utiliidid ja käivitatava programmi. Teine näide võib olla programmide täitmise keskkond, mis on erineb majutusteenuse rakenduse jaoks kirjutatud. Selle võib olla näiteks Unix või Linux programmi, mida soovite käivitada Windows või .net-i rakendusest. Valige vahemikus mõni Azure virtuaalse masina operatsioonisüsteemide ja käivitada oma teenuse või käivitatava selle virtual arvutisse.

Aitab teil valida, millal kasutada Virtuaalmasinates, leiate [Azure'i rakenduse teenused, pilveteenustega ja Virtuaalmasinates võrdlus](./app-service-web/choose-web-site-cloud-service-vm.md). Virtuaalmasinates võimaluste kohta leiate teavet teemast [Azure virtuaalse masina ja pilveteenuses suurused](http://msdn.microsoft.com/library/azure/dn197896.aspx). Opsüsteemid ja Varemkoostatud pilte, mis on saadaval Virtuaalmasinates kohta leiate lisateavet teemast [Azure'i Virtuaalmasinates turuplatsilt](https://azure.microsoft.com/gallery/virtual-machines/).

Tausta tööülesande eraldi virtuaalse masina käivitamiseks on teil mitmesuguseid võimalusi.

- Käivitamiseks tööülesande nõudmisel otse rakenduse kaudu, saates taotluse tööülesande seab lõpp-punkt. See edastab andmetes, mis nõuab ülesanne. Selle lõpp-punkti käivitab tööülesannet.
- Toimingu käivitada ajakava ajasti või ajasti, mis on saadaval opsüsteemi valitud abil saate konfigureerida. Näiteks Windows saate Windowsi Toiminguajasti skripte ja ülesandeid täita. Või kui teil on SQL serveri virtual arvutisse installitud, SQL Server Agent abil saate käivitada skripte ja tööülesandeid.
- Saate algatada tööülesande lisamine tööülesande kuulab järjekorda sõnumi või saatmisega API, mis on tööülesande seab Azure'i Scheduler.

Vt varasemates [päästikute](#triggers) Lisateavet selle kohta, kuidas saate algatada tausttoimingud.  

### <a name="considerations"></a>Kaalutlused

Kui teil on otsustada, kas tausta tööülesanded on Azure virtuaalse masina juurutada, võtke arvesse järgmist.

- Tausta tööülesanded eraldi Azure virtuaalse masina hosting pakub paindlikkust ja võimaldab täpselt juhtida algatamine, täitmise, ajakava ja ressursi eraldus. Siiski suureneb käitusaja maksumus, kui virtuaalse masina tuleb kasutusele võtta lihtsalt käivitage tausttoimingud.
- Seal on pole Azure portaali ja nurjunud tööülesannete--pole uuesti automatiseeritud võimalus tööülesannete jälgimiseks, kuigi saate jälgida lihtsa virtuaalse masina ja seda hallata, kasutades [Azure teenuse halduse cmdletid](http://msdn.microsoft.com/library/azure/dn495240.aspx). Siiski ei ole määrata protsesside ja Teemad Arvuta sõlmed. Tavaliselt abil virtuaalse masina vajavad täiendavad peegeldav kasutusele võtta, millega abil kogutud andmete kiired tööülesannet ja virtuaalse masina operatsioonisüsteem. Ühe lahenduse, mis võiks olla asjakohane on kasutada [System Center halduspaketi Azure](http://technet.microsoft.com/library/gg276383.aspx).
- Võiksite luua jälgimisega seotud sondid, mis on avatud kaudu HTTP lõpp-punktid. Nende sondid kood võib seisundi kontrollimine, Koguge teavet töö ja statistika--või võrdleb tõrketeabe ning naaske haldamise rakendus. Lisateabe saamiseks vt [Seisundi lõpp-punkti jälgimise mustri](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Lisateave

- Azure'i [Virtuaalmasinates](https://azure.microsoft.com/services/virtual-machines/)
- [Azure'i Virtuaalmasinates KKK](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Kujundamine

On mitmeid olulise tegureid tausttoimingud kujundamisel silmas pidada. Järgmistes lõikudes käsitletakse eraldamine, konfliktide ja koordineerituna.

## <a name="partitioning"></a>Jagamine

Kui otsustate lisada tausttoimingud olemasoleva Arvuta astme (nt web appi, web roll, olemasoleva töötaja rolli või virtuaalse masina), peate arvestama, kuidas see mõjutab kvaliteedi atribuutide Arvuta eksemplar ja tausta ülesanne ise. Teguritest aitab teil otsustada, kas tööülesanded olemasoleva Arvuta eksemplariga colocate või eraldi need välja eraldi Arvuta eksemplari:

- **Kättesaadavus**: tausttoimingud võib ei pea olema samal tasemel kättesaadavus muude osadele, eriti Kasutajaliidese ja mujal, mis on seotud otse kasutaja suhtluse. Tausttoimingud võib olla rohkem latentsus retried ühenduse tõrkeid, ja muud tegurid mõjutavad kättesaadavuse, kuna need toimingud saate järjekorda. Siiski peab olema piisava mahuga, et vältida taotlusi, mis võivad blokeerida järjekorrad ja mõjutada kogu varukoopia.
- **Skaleeritavus**: tausttoimingud tõenäoliselt eri skaleeritavus nõue kui UI ja interaktiivsed osad rakendus. Skaleerimist UI võib olla täitmiseks nõudmise, kasutada ajal tasumata tausta võib sooritada toiminguid vähem hõivatud aegadel on vähem Arvuta eksemplaride arv.
- **Paindlikkust**: Arvuta eksemplari, mis hostib lihtsalt tausttoimingud tõrge võib ei surmavalt mõjutada kogu kui taotlusi järgmiste toimingute tegemiseks saate ootele või edasi lükata, kuni toiming on saadaval uuesti. Kui Arvuta eksemplar ja/või tööülesannete saab uuesti sobiva intervalli sees, võib rakenduse kasutajad ei mõjuta.
- **Turvalisus**: tausttoimingud võib olla erinevad turbenõuetele või Kasutajaliidese või rakenduse mujal kui piirangud. Eraldi Arvuta eksemplari abil saate määrata eri turvalisuse keskkonnas toimingute. Saate näiteks Gatekeeperi mustrite eristamiseks tausta Arvuta eksemplarid UI selleks, et maksimeerimine turbe- ja eraldamine.
- **Jõudluse**: saate valida Arvuta eksemplari tüübi jaoks tausta ülesandeid spetsiaalselt match tööülesanded jõudluse nõuetele. See võib tähenda, kasutades väiksemate Arvuta suvand, kui tööülesanded ei nõua sama töötlemise võimaluste UI või suurem eksemplari, kui nad nõua täiendavat võimalused ja ressursid.
- **Hallatavust**: tausttoimingud võib olla erinevad arendamise ja juurutamise rütmi rakenduse koodi või Kasutajaliidese kaudu. Rakendades neile eraldi Arvuta eksemplari saate lihtsustada värskendused ja versioonimise.
- **Maksumus**: lisamise arvutada eksemplarid käivitada taustal tööülesannete suurendab hosting kulud. Kaaluge vahelist täiendav läbilaskevõime ja nende eest maksab hoolikalt.

Lisateabe saamiseks vt [Juhataja valimise mustri](http://msdn.microsoft.com/library/dn568104.aspx) ja [Kiiruisutamises tarbijad mustri](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>Konfliktide

Kui teil on mitu eksemplari tausta töö, on võimalik, et nad saavad võistlema kättesaadavust ja teenuseid, näiteks andmebaasid ja salvestusruumi. Samaaegseid juurdepääsu võib põhjustada ressursside väide, põhjustada konfliktide kättesaadavus teenuste ja salvestusruumi andmete kehtivust. Kardetav locking lähenemisviisi abil saate lahendada ressursi argument. See takistab kiiruisutamises eksemplarid tööülesande samaaegselt teenuse või andmete rikkumist.

Teine võimalus konfliktide lahendamiseks on määratleda tausttoimingud on singleton nii, et pole kunagi ainult üks eksemplar töötab. Kuid see kaob töökindluse ja jõudluse kasu, et saate sisestada mitu astme konfigureerimine. See kehtib eriti siis, kui UI saate piisavalt töö säilitamiseks tausta rohkem kui ühe tööülesande hõivatud.

See on oluline tagada, et taust tööülesande saab automaatselt uuesti ning see on piisavalt toime tulla nõudmise. Saate seda saavutada eraldamise Arvuta eksemplari piisavalt ressursse, rakendades queueing süsteem, mida saab salvestada hilisemaks ajaks kui nõudmisel väheneb taotlused või nende tehnikate kombinatsiooni abil.

## <a name="coordination"></a>Koordineerituna

Tausttoimingud võib olla keeruline ja võib vaja mitme üksikute tööülesannete, andes tulemuseks või kõigi nõuete täitmiseks. See on levinud need stsenaariumid väiksemate varjatud juhiseid või alamülesanded, mida saab kasutada mitut tarbijate tööülesande jagada. Multistep töö võib olla paindlikumaks ja tõhusamaks, sest üksikute juhiseid võib olla mitu tööd korduvkasutatava. Samuti on lihtne lisamine, eemaldamine või muutmine juhiseid järjestuse.

Saate keeruline mitme tööülesanded ja juhiseid, kuid on kolm levinud mustrite, mille abil saate oma lahenduse rakendamise juhend:

- **Lagunevatest tööülesande üheks mitme korduvkasutatava juhiseid**. Rakenduse võidakse sooritada erinevaid ülesandeid erineva teabe, mis töötleb. Lihtne, kuid jäik lähenemisviisi rakendatakse see rakendus võib olla see töötlemine monoliit mooduli sooritamiseks. Seda moodust aga vähendada refaktooring kood, optimeerides seda või diagrammide korduvkasutamine, kui sama töötlemise osad on nõutav mujal rakendusest võimalusi. Lisateavet leiate teemadest [üksust Pipes ja filtrid mustri](http://msdn.microsoft.com/library/dn568100.aspx).
- **Juhised ülesande täitmine haldamine**. Rakendus võib toimingud, mis sisaldab mitmeid samme (mis võib remote teenuste või või juurdepääs remote ressursid). Üksikute juhiseid võib olla üksteisest sõltumatult, kuid need on juhitud rakenduse loogika, mis rakendab tööülesanne. Lisateabe saamiseks vt [Ajasti Agent halduri mustri](http://msdn.microsoft.com/library/dn589780.aspx).
- **Taastamine ülesande juhised, mis ei suuda haldamine**. Rakenduse võib peab tagasivõtmiseks töö on läbi juhiseid (mis koos määratlemine lõpuks ühtsete toimingu) kui üks või mitu juhiseid ei. Lisateavet leiate teemast [Kompensatsioonitoodete tehingu mustri](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Elutsükkel (pilveteenustega)

 Kui otsustate taustal töökohtade pilveteenustega rakenduste abil **RoleEntryPoint** klassi veebi ja töötaja rollid kasutavate kasutusele võtta, on oluline mõista elutsükli klassi kasutamiseks õigesti.

Veebi ja töötaja rollid läbida etapiks kogum, nagu nad käivitada, käivitamine ja peatamine. Klassi **RoleEntryPoint** seab sündmused, mis näitavad, kui need etapid toimuvad sarja. Kasutage järgmisi lähtestada, käivitada, ja Lõpeta oma kohandatud tausttoimingud. Kogu tsükli on:

- Azure'i laadib rolli komplekti ja otsib klassi, mis tuleneb **RoleEntryPoint**.
- Kui see leiab see tund, see nõuab **RoleEntryPoint.OnStart()**. Saate alistada, seda meetodit lähtestada oma tausttoimingud.
- Kui **OnStart** meetod on lõpule jõudnud, Azure kõned **Application_Start()** rakenduse globaalne faili, kui see on praegu (nt Global.asax web roll, kus töötab ASP.net-i).
- Azure'i **RoleEntryPoint.Run()** kutsub esiplaani uus teema, mis käivitab **OnStart()**paralleelselt. Saate alistada, seda meetodit alustada oma tausttoimingud.
- Ava_moodul lõppemisel helistab Azure'i **Application_End()** esmalt rakenduse üld-failis, kui see on olemas, ja seejärel kutsub **RoleEntryPoint.OnStop()** Saate alistada **OnStop** meetodi abil peatada tööülesannete tausta, puhastamiseks ressursid, kõrvaldada objektid ja sulgege ühendused, mida kasutada tööülesanded.
- Azure töötaja rolli Majutaja protsess on peatatud. Selles etapis roll taaskasutamine ja uuesti.

Üksikasjalikumat ja näide **RoleEntryPoint** klassi meetodite abil leiate teemast [Arvutada ressursi konsolideerimise mustri](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Kaalutlused

Kui kavatsete, kuidas saate käivitada tausttoimingud veebis või töötaja roll, võtke arvesse järgmist.

- Vaikimisi **käivitada** meetodi rakendamine **RoleEntryPoint** tunni sisaldab hoiab roll elus lõputult **Thread.Sleep(Timeout.Infinite)** kõne. Kui **käitada** meetodit (mis on tavaliselt vajalikud tausttoimingud) alistamiseks peate lubama pole teie koodi väljumine meetodit juhul, kui soovite prügikastis rolli eksemplari.
- Tüüpilised rakendamise **käivitada** meetod sisaldab koodi käivitamiseks iga tausttoimingud ja tsükkel ehitada, mis kontrollib perioodiliselt olek kõik tausttoimingud. Saate taaskäivitada, mis ei või jälgida loobumise märgid, mis näitavad, et töö lõpetanud.
- Kui tausta tööülesande põhjustab töötlemata erandi, tuleks tööülesande rolli jätkamiseks töötab taustal töötada lubades taaskasutada. Kui erandi põhjuseks on rikutud objektide väljaspool tööülesanne, näiteks jagatud salvestusruumi, erand käsitleks **RoleEntryPoint** tunni, kõik tööülesanded tuleks tühistada, ja **käivitada** meetod peaks olema õigus lõpetada. Azure'i taaskäivitage roll.
- Viige või tappa tausttoimingud ja ressursside puhastamiseks **OnStop** meetodi abil. See võib hõlmata pikaajalisi või multistep ülesannete peatamine. On oluline silmas pidada? kuidas seda saab teha, et vältida andmete selliste vastuolude tekkimist. Kui kasutaja algatatud sulgemise muul lõpetab rolli eksemplari, töötavad **OnStop** meetodit koodi enne selle lõpetamist sunniviisiliselt viie minuti jooksul lõpule viia. Tagada, et teie kood selle aja jooksul lõpule viia või saate luba valmimiseni ei tööta.  
- Azure'i laadi koormusetasakaalustusteenuse käivitatakse suunavad liikluse rolli eksemplari, kui **RoleEntryPoint.OnStart** meetod tagastab väärtuse **true**. Seetõttu kaaluma oma lähtestamine koodi **OnStart** meetodi nii, et ei saa mis tahes liikluse rolli eksemplarid, mis on edukalt lähtestada.
- Saate kasutada käivitus tööülesannete Lisaks meetodite **RoleEntryPoint** klassi. Kasutage käivitus tööülesanded sätteid, mida peate muutma Azure laadi koormusetasakaalustusteenuse, kuna need toimingud käivitada enne roll saab taotlused lähtestada. Lisateavet leiate teemast [käivitada käivitus ülesanded Azure](./cloud-services/cloud-services-startup-tasks.md).
- Kui tööülesande käivitamine on tõrge, võib see jõustada roll pidevalt uuesti. See võib takistada virtuaalse IP (VIP) aadress Vaheta varem etapiviisilist versiooni kuna nende nõuab eksklusiivset juurdepääsu roll. See ei ole võimalik saada ajal taaskäivitamine roll. Probleemi lahendamiseks:
    -  Lisage järgmine kood oma rolli **OnStart** ja **käivitage** meetoditest:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - Kuuluva määratlus **külmutamine** säte on loogikaväärtus ServiceDefinition.csdef ja ServiceConfiguration. *roll .cscfg failid ja määrake selle väärtuseks* *FALSE* *. Kui roll läheb korduvate uuesti režiimi, saate muuta selle atribuudi * *true** külmutada rolli täitmise ja Lubage see olla vahetasin mõnelt varasemalt versioonilt.

## <a name="resiliency-considerations"></a>Paindlikkust kaalutlused

Tausttoimingud peab olema olles, usaldusväärseid pakkumiseks rakenduse. Kui olete plaanimine ja kujundamine tausttoimingud, võtke arvesse järgmist.

- Reageerimine nõtkelt rolli või teenuse taaskäivitamist, ilma andmete rikkumist või vastuolu viimine rakenduse peate saama tausttoimingud. Pikaajalisi või multistep ülesanded, on soovitatav kasutada, salvestades töö oleku püsiv hoidla või sõnumeid järjekorda, kui see on asjakohane _kontrollimine osutamine_ . Näiteks saate teavet sõnumis järjekorda püsivad ja värskendada seda teavet, et tööülesanne saab töödelda kaudu viimase teadaolevad hea kontrollpunkt--asemel taaskäivitada algusest artistide ülesande edenemine. Azure'i teenus siini järjekorrad kasutamisel saate sõnumi seansid lubamiseks sama stsenaariumi. Seansid võimaldavad salvestada ja tuua rakenduse töötlemine riigi [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) ja [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) viisidel. Kujundamise usaldusväärne multistep protsesside ja töövoogude kohta leiate lisateavet teemast [Scheduler Agent halduri mustri](http://msdn.microsoft.com/library/dn589780.aspx).
- Veebis või töötaja rollid kasutamisel majutada mitme tausttoimingud kujundamine oma alistamine nurjunud või seiskunud tööülesannete jälgimine ja taaskäivitage neid **käivitada** meetod. Kui see on praktiline ja kasutate töötaja roll, jõustada töötaja roll, **käivitage** meetod väljumist uuesti.
- Kui kasutate järjekorrad suhelda tausttoimingud, järjekorrad võib toimida puhvrit tööülesannete rakenduse ajal saadetud taotluste talletamiseks on kõrgem kui tavaliselt laadi. See võimaldab jõuda UI ajal väiksema koormusega ajavahemikele tööülesanded. Ka see tähendab, et ringlussevõtu roll ei blokeeri UI. Lisateabe saamiseks vt [järjekorda vastavalt laadi tasanduskihid mustri](http://msdn.microsoft.com/library/dn589783.aspx). Kui mõned tööülesanded on rohkem kui teistel, kaaluma [Prioriteet järjekorda mustri](http://msdn.microsoft.com/library/dn589794.aspx) tagamaks, et käivitada enne vähem olulisemad järgmised toimingud.
- Tausttoimingud, mis on algatatud sõnumid või protsess peab loodud tegelema selliste vastuolude tekkimist, nt sõnumid saabuvad vales järjekorras, sõnumid, mis seni põhjustada tõrke (sageli nimetatakse _mürki sõnumite_) ja sõnumid, mis on esitatud rohkem kui üks kord. Arvestage järgmisega.
  - Sõnumid, mis tuleb töödelda kindlas järjekorras, nagu need, mis muuta andmete põhjal olemasolevate andmete väärtus (näiteks väärtuse lisamine olevat väärtust), võib ei saabu on saadetud algset järjestuses. Teise võimalusena võivad need käsitleja oli eri eksemplarid tausta tööülesande tõttu erineva laadimise iga eksemplari erinevas järjestuses. Sõnumid, mis peavad olema töödeldud kindlas järjekorras peaks sisaldama järjekorranumber, võti või muu tähis, mis tausttoimingud abil saate tagada töötlemine õiges järjestuses. Kui kasutate Azure'i teenus siini, saate sõnumi seansid järjestuse kohaletoimetamise tagamiseks. See on tavaliselt tõhusam, võimaluse korral kujundada protsessi nii, et sõnum järjestus pole oluline.
  - Tavaliselt on tausta tööülesande Piilumine sõnumite järjekorda, mis peidab neid ajutiselt teiste sõnumi tarbijate. Kustutab sõnumi pärast edukalt töödeldud. Kui tööülesande tausta nurjub, kui sõnumi töötlemine, see sõnum järjekorda uuesti pärast aegumist peek ajalõpp. Töötleb muus eksemplaris tööülesande või järgmise töötlemine tsükli eksemplar. Kui sõnumi süsteemne põhjustab tõrke tarbija, see blokeerib tööülesanne, kuhjuda ja lõpuks ise rakenduse kui järjekorda saab täis. Seetõttu on oluline tuvastada ja eemaldada mürki sõnumid järjekorda. Kui kasutate Azure teenuse siini, sõnumid, mis põhjustada tõrke saab teisaldada automaatselt või käsitsi on seotud surnud täht järjekorda.
  - Järjekorrad on tagatud _vähemalt ühe korra_ kohaletoimetamise menetlustele, kuid need võivad pakkuda sama kuvatakse rohkem kui üks kord. Lisaks tausta tööülesande nurjumisel pärast sõnumi töötlemine, kuid enne kustutamist kuhjuda sõnumi muutuvad kättesaadavaks töötlemiseks uuesti. Tausttoimingud peaks olema idempotent, mis tähendab, et sama sõnumi töötlemine rohkem kui üks kord ei põhjusta viga või vastuolu rakenduse andmeid. Loomulikult on mõned toimingud idempotent, nt talletatud väärtus teatud uue väärtuse seadmine. Siiski, nagu lisamise väärtus olemasoleva salvestatud väärtus pole märkinud ruutu, et talletatud väärtus on endiselt sama, kui sõnum algselt saadeti põhjustab vastuolusid. Azure'i teenus siini järjekorrad saab konfigureerida dubleeritakse sõnumite automaatse eemaldamise.
  - Kiirsõnumside süsteemid, nt Azure storage järjekorrad ja Azure teenuse siini järjekorrad, toetavad de-queue count atribuut, mis näitab, mitu korda on loetud sõnumi järjekorda. See võib olla kasulik töötlemise korduvate ja mürki sõnumite. Lisateavet leiate teemast [Asünkroonne sõnumside Primer](http://msdn.microsoft.com/library/dn589781.aspx) ja [Idempotency mustrid](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Suuruse muutmise ja jõudluse kaalutlused

Tausttoimingud peab pakkuma piisavalt jõudluse tagamiseks need rakenduse blokeerida või põhjustada vastuolusid hilinemise tegevuse tõttu, kui süsteem on koormuse. Tavaliselt on jõudlust parandada skaleerimist Arvuta eksemplarid, mis majutada tausttoimingud. Kui olete plaanimine ja kujundamine tausttoimingud, võtke arvesse järgmist skaleeritavus ja jõudluse ümber:

- Azure'i toetab autoscaling (skaleerimist välja ja uuesti sisse skaleerimist) põhjal praegusi ja laadi--või eelmääratletud ajakavas veebirakendustes Cloud Services veebiteenuse ja töötaja rollid ja Virtuaalmasinates majutatud juurutuste. Selle funktsiooni abil saate tagada, et kogu rakendus on piisavalt jõudluse võimaluste ajal käitusaja kulude minimeerimine.
- Kui tausttoimingud erinevatele võimalus mujalt pilveteenustega rakendus (nt Kasutajaliidese või komponendid, nt andmete Accessi layer), majutusteenuse tausta tööülesannete koos eraldi töötaja roll võimaldab Kasutajaliidese ja tausta mastaapimiseks sõltumatult hallata laadi tööülesande rollid. Kui mitu tausttoimingud on oluliselt erinev jõudluse võimaluste üksteisest, kaaluge jagatakse eraldi töötaja rollid ja mastaapimist iga rolli tüüp sõltumatult. Pange tähele, et see võib suurendada käitusaja kulude võrreldes ühendab kõik tööülesanded vähem rollid.
- Lihtsalt skaleerimist rollid ei pruugi olla piisavalt jõudluse koormuse andmekao vältimiseks. Peate ka mastaapimiseks salvestusruumi järjekorrad ja muud ressursid vältimiseks üldine ühtne alates muutumas kitsaskoht töötlemine. Pidage silmas ka muud piirangud, nt maksimaalne läbilaskevõime salvestusruumi ja muud teenused, mis sõltuvad rakendus ja tausttoimingud.
- Tausttoimingud peab mõeldud skaleerimist. Näiteks need tuleb dünaamiliselt avastada arvu salvestusruumi järjekorda, et kuulata või vastav järjekorda sõnumeid saata.
- Vaikimisi WebJobs mastaabi koos oma seotud Azure'i veebirakenduste eksemplari. Juhul, kui soovite mõnda WebJob käitada ainult ühte eksemplari, saate luua Settings.job faili, mis sisaldab andmeid, JSON **{"is_singleton": tõene}**. See sunnib Azure'i käivitamiseks ainult ühe eksemplari WebJob, isegi juhul, kui on seotud veebirakenduse mitmes eksemplaris. See võib olla kasulik tehnika jaoks ajastatud tööd, mis tuleb käitada ainult ühte eksemplari nimega.

## <a name="related-patterns"></a>Seotud mustrite

- [Asünkroonne sõnumside Primer](http://msdn.microsoft.com/library/dn589781.aspx)
- [Autoscaling juhised](http://msdn.microsoft.com/library/dn589774.aspx)
- [Pikkusega tehingu muster](http://msdn.microsoft.com/library/dn589804.aspx)
- [Konkureerivate tarbijad muster](http://msdn.microsoft.com/library/dn568101.aspx)
- [Arvutage eraldamine juhised](http://msdn.microsoft.com/library/dn589773.aspx)
- [Arvutage ressursi konsolideerimise muster](http://msdn.microsoft.com/library/dn589778.aspx)
- [Gatekeeperi muster](http://msdn.microsoft.com/library/dn589793.aspx)
- [Mustri juhataja valimine](http://msdn.microsoft.com/library/dn568104.aspx)
- [Üksust Pipes ja filtrid muster](http://msdn.microsoft.com/library/dn568100.aspx)
- [Priority (prioriteet) järjekorda muster](http://msdn.microsoft.com/library/dn589794.aspx)
- [Ühtlustamise mustri järjekorda vastavalt laadimine](http://msdn.microsoft.com/library/dn589783.aspx)
- [Ajasti Agent halduri muster](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Lisateave

- [Skaleerimise Azure rakenduste töötaja rollid](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Tausttoimingud täitmine](http://msdn.microsoft.com/library/ff803365.aspx)
- [Azure'i rolli käivitus elutsükli](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (ajaveebipostituse)
- [Azure'i Cloud Services rolli elutsükkel](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (video)
- [Azure'i WebJobs SDK kasutamise alustamine](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure'i järjekorrad ja teenuse siini järjekorrad - võrreldes ja ja](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Kuidas lubada diagnostika mõnes pilveteenuses](./cloud-services/cloud-services-dotnet-diagnostics.md)
