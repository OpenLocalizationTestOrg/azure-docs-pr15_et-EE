<properties
   pageTitle="Lisada kohandatud teenuse struktuuri Seisundiaruannete | Microsoft Azure'i"
   description="Kirjeldab, kuidas saata kohandatud Seisundiaruannete Azure teenuse struktuuri seisund üksused. Kujundamine ja rakendamine kvaliteedi Seisundiaruannete annab soovitused."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="add-custom-service-fabric-health-reports"></a>Lisada kohandatud teenuse struktuuri seisundiaruanded
Azure teenuse struktuuri tutvustab [seisund mudeli](service-fabric-health-introduction.md) mõeldud lipu vigane kobar ja kindlate üksuste rakenduse tingimusi. Seisund mudel kasutab **seisund reporteritele** (osad ja valvekoerte). Eesmärk on kiire ja lihtne diagnoosimine ja parandamine. Teenuse poolt vaja mõelda esmast seisundi kohta. Ükskõik milline tingimus, mis võivad mõjutada seisund esitatakse, eriti siis, kui see abistab lipp probleeme lähedale juurkausta. Süsteemiseisundi teavet saate salvestada palju aega ja vaeva silumine ja uurimine üks kord teenuse on tööks skaala pilves (era- või Azure).

Teenuse struktuuri reporteritele kuvari tuvastatud huvi pakkuvate tingimused. Ta aru nendest tingimustest nende kohalik vaate põhjal. [Seisund talletada](service-fabric-health-introduction.md#health-store) liitmise saadetud kõik reporterid kas üksused on globaalselt terve seisundi andmed. Kui seate mudeli on mõeldud rikkalike, paindlik ja lihtne kasutada. Funktsiooni Seisundiaruannete kvaliteeti määratleb seisund vaate klaster täpsuse. Vale-positiivsed kuvavate valesti vigane probleemid võivad negatiivselt mõjutada uuendamine või muude teenuste seisundi andmeid kasutavate. Selliste teenuste näited on parandamise teenused ja infoturbeküsimustes menetlustele. Seetõttu on vaja mõtlema aruandeid, mis jäädvustada tingimuste parimal viisil huvi pakuvad.

Kavandada ja rakendada seisund aruandlus, valvekoerte ja osad peavad.

- Määratleda, kas huvitatud, nii, nagu see on jälgida ja mõju kobar või rakenduse funktsioone. Selle teabe põhjal otsustada seisundi aruande atribuudi ja seisundioleku.

- Määratlege aruande kehtib [üksus](service-fabric-health-introduction.md#health-entities-and-hierarchy) .

- Kui märkeruut on valmis, teenuse või mõne sise- või valvekoer.

- Määratleda allika, mis tähistavad looma.

- Valige aruandlusteenuste strateegia, kas perioodiliselt või üleminekud. Soovitatav viis on perioodiliselt, kuna see nõuab Lihtsamad kood ja on vähem vigade tõenäosust.

- Määratlege kaua vigane tingimuste jaoks aruanne peaks jääma seisund poes ja kuidas peaks tühjendatud. Selle teabe abil otsustada aruande aega live ja Eemalda-aegumise käitumine.

Nagu mainitud, aruandlus saab teha:

- Jälgida teenuse struktuuri teenuse koopia.

- Sisemise valvekoerte juurutatud teenuse struktuuri teenuste (nt teenuse struktuuri kodakondsuseta teenus, mis jälgib tingimuste ja probleemide aruanded). Funktsiooni valvekoerte võib olla ka kõik sõlmed juurutatud või saate affinitized jälgida teenusesse.

- Sisemise valvekoerte, mis töötab teenuse struktuuri sõlmed, kuid on *pole* rakendatud teenuse struktuuri teenused.

- Välise valvekoerte probe ressurss *väljaspool* teenuse struktuuri kobar (nt jälgimise teenus nagu Gomez).

> [AZURE.NOTE] Välja kasti, lisatakse klaster Seisundiaruannete saadetud süsteemi komponendid. Lisateavet [kasutamine süsteemi seisund](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)aruandeid tõrkeotsingu jaoks. Kasutaja aruanded tuleb saata [seisund üksused](service-fabric-health-introduction.md#health-entities-and-hierarchy) , mis on juba loodud süsteem.

Kui seisundi aruandlus kujundus on selge, Seisundiaruannete saab saata hõlpsalt. Saate kasutada aruande seisund [FabricClient](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.aspx) siis, kui klaster on [turvaline](service-fabric-cluster-security.md) või kui struktuuri kliendil on administraatoriõigused. Seda saab teha, kasutades [FabricClient.HealthManager.ReportHealth](https://msdn.microsoft.com/library/system.fabric.fabricclient.healthclient.reporthealth.aspx)API kaudu või läbi ülejäänud PowerShelli kaudu. Konfiguratsiooni nupud partii aruanded paranenud jõudlus.

> [AZURE.NOTE] Aruande seisund on sünkroonse ja see tähistab ainult valideerimine kliendis. Aruande aktsepteerib seisund kliendi asjaolu või `Partition` või `CodePackageActivationContext` objekte ei tähenda, et see on rakendatud poes. See on saadetud asünkroonselt ja muude aruannetega, võib-olla batched. Endiselt ei pruugi töötlemise server: järjenumber võib olla aegunud, mille aruannet ei rakendata ettevõte on kustutatud, jne.

## <a name="health-client"></a>Kliendi seisund
Funktsiooni Seisundiaruannete saadetakse seisund poe kaudu seisund klienti, mis asub kliendi struktuuri sees. Kliendi seisundi saab konfigureerida järgmist:

- **HealthReportSendInterval**: viivituse aruanne lisatakse kliendi ja saatmist seisund poest aeg. Ühe sõnumi, mitte ühe sõnumi saatmisel iga aruande paketi aruannete abil. Funktsiooni partiide parandab jõudlust. Vaikimisi: 30 sekundit.

- **HealthReportRetrySendInterval**: intervall, kus seisundi kliendi saadab akumuleeritud seisund aruannete seisund pood. Vaikimisi: 30 sekundit.

- **HealthOperationTimeout**: aja aruande seisund pood saatis sõnumi. Kui sõnumi ajalõpp, uued seisund kliendi katsed seda, kuni seisund pood kinnitab, et aruanne on töödeldud. Vaikimisi: kaks minutit.

> [AZURE.NOTE] Kui aruannete on batched, struktuuri kliendi hoitakse elus vähemalt HealthReportSendInterval tagamaks, et need saadetakse. Kui sõnum on kadunud või seisundi pood ei saa siirdamiseks tõrgete tõttu rakendada, struktuuri kliendi hoitakse elus enam uuesti võimalus anda.

Kliendi puhverdamine võtab arvesse aruannete unikaalsuse. Näiteks kui teatud halb reporter on teatamine 100 aruanded sekundis sama atribuut, et sama olemi, aruannete asendatakse viimane versioon. Ainult ühe sellise aruande olemas kõige kliendi järjekorda. Kui partiide on konfigureeritud, aruandeid, mis on saadetud seisund poe arv on vaid üks saada intervalli kohta. See aruanne on lisatud Viimane aruanne, mis kajastab hetkeseisu üksuse.
Kõigi konfiguratsiooni parameetrite võib olla määratud, millal `FabricClient` on loodud, läbides [FabricClientSettings](https://msdn.microsoft.com/library/azure/system.fabric.fabricclientsettings.aspx) seotud kirjete jaoks soovitud väärtused.

Järgmine loob struktuuri klient ja määrab, et aruannete saadetakse lisamisel. Korduskatsed ilmneda ajalõpud ja vigu, mida saate uuesti proovida, iga 40 sekundi järel.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Sama parameetrid saab määrata arvutikobaras ühenduse loomisel PowerShelli kaudu. Järgmine alguses kohaliku kobar-ühendus.

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

> [AZURE.NOTE] Tagada, et ei saa volitamata teenuste aruande seisund klaster üksuste vastu, saate serveri konfigureeritud aktsepteerige ainult turvatud kliendid. Funktsiooni `FabricClient` kasutatakse peavad saama suhelda kobar (nt koos Kerberos või serdi autentimine) lubatud turvalisus. Lugege lisateavet [kobar turvalisus](service-fabric-cluster-security.md).

## <a name="report-from-within-low-privilege-services"></a>Madal õiguste teenustesse aruanne
Kaudu teenuse struktuuri teenustesse, mis ei ole administraator juurdepääsu klaster, saate teatada seisundi üksustele: praeguses kontekstis kaudu `Partition` või `CodePackageActivationContext`.

- Kodakondsuseta teenuste, kasutada [IStatelessServicePartition.ReportInstanceHealth](https://msdn.microsoft.com/library/system.fabric.istatelessservicepartition.reportinstancehealth.aspx) aruandluseks praeguse eksemplari.

- Stateful teenuste, kasutada [IStatefulServicePartition.ReportReplicaHealth](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.reportreplicahealth.aspx) aruandluseks praeguse koopia.

- Aruande praeguse sektsiooni üksuse [IServicePartition.ReportPartitionHealth](https://msdn.microsoft.com//library/system.fabric.iservicepartition.reportpartitionhealth.aspx) abil.

- Aruande praeguse rakenduse [CodePackageActivationContext.ReportApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportapplicationhealth.aspx) abil.

- Kasutage [CodePackageActivationContext.ReportDeployedApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth.aspx) praeguse rakenduse juurutatud praegusest teatada.

- Kasutage [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth.aspx) teenuse paketi praeguse rakenduse juurutatud praegusest teatada.

> [AZURE.NOTE] Sees, on `Partition` ja `CodePackageActivationContext` pikalt seisund klient, mis on konfigureeritud vaikesätted. Samad kaalutlused ülevaade [seisund](service-fabric-report-health.md#health-client) klient ja rakendada – aruannete on batched ja nii, et objektide saab hoida elus on võimalus saata aruande lisamine timer saadetud.

## <a name="design-health-reporting"></a>Kujundage seisund teatamine
Esimene samm kvaliteetne aruannete loomiseks on tuvastamise tingimused, mis võivad mõjutada teenuse seisund. Ükskõik milline tingimus, mis aitavad lipp probleeme kasutatava teenuse või kobar käivitab – või isegi parem enne probleemi juhtub – saate potentsiaalselt salvestada miljardit dollarit. Eeliste sisaldama vähem aega, vähem öösel kulunud uurimiseks ja parandada probleemid ja klientide rahulolu.

Kui tuvastatakse tingimusi, tuleb valvekoer poolt välja selgitada parima viisi jälgida nende pea kohal ja kasulikkus. Näiteks kaaluge teenus, mis teeb keerukate arvutuste tegemine, mis kasutavad mõned ajutised failid ühiskasutatav. Valvekoer võib jälgida ühiskasutus tagamiseks on saadaval piisavalt ruumi. See võiks kuulata jaoks faili või kausta muudatuste kohta. See võib aruande hoiatus kui ka esmast lävi ja tõrketeate, kui ühiskasutus on täis. Hoiatus, võib süsteemi Paranda alustada vanemate failide ühiskasutusse andmise kohta. Tõrke, võib süsteemi parandamine viige teenuse koopia teise sõlme. Pange tähele, kuidas Ühendriigid tingimus on kirjeldatud tervise: tingimus, mida saab pidada terve riigi või vigane (hoiatus või tõrge).

Kui määratud on jälgimisega seotud üksikasjad, peab valvekoer kirjutaja aru saada, kuidas rakendada valvekoer. Kui kaudu saate määrata tingimused, teenuses, saab valvekoer jälgida teenusesse osa. Näiteks saate teenuse kood märkige ruut ühiskasutus kasutuse ja seejärel aruande iga kord, kui püüab kirjutamine faili kohaliku struktuuri kliendi abil. Seda moodust eeliseid on aruandlus on lihtne. Tuleb takistada valvekoer vead mõjutavad teenuse funktsioone, kuigi.

Aruandluse rakenduses jälgida teenus pole alati soovitud suvand. Teenuses valvekoer ei saa tuvastada tingimused. Ei pruugi olla loogika või andmed teevad. Funktsiooni pea kohal jälgimise tingimused võib olla kõrge. Tingimustele ka ei pruugi teatud teenus, kuid selle asemel mõjutavad teenuste vahel. Teine võimalus on on valvekoerte klaster eraldi protsessid. Funktsiooni valvekoerte lihtsalt jälgida tingimuste ja aruande, mõjutamata peamised teenuseid, mis tahes viisil. Näiteks võib neid valvekoerte rakendada sama rakenduse, juurutatud kõik sõlmed või sama nimega teenuse sõlmed kodakondsuseta teenused.

Mõnikord töötab klaster valvekoer ei ole soovitud suvand. Kui jälgida tingimus on kättesaadavus või teenuse kasutajad ei näe seda, on kõige parem viis soovitud valvekoerte samas kohas, kui kasutaja klientide. Seal, nad testida toimingute kasutajad helistada samal viisil. Näiteks saate määrata, mis asub väljaspool klaster probleemid taotlusi teenusesse ja seejärel kontrollib latentsus ja tulemuse õigsuse valvekoer. (Kalkulaator teenuse, näiteks kas 2 + 2 tagasi 4 on mõistlik aeg?)

Kui valvekoer üksikasjad on lõpetatud, peaks otsustama andmeallika ID, mis tuvastab kordumatult. Kui klaster elavad mitme valvekoerte sama tüüpi, kas nad peavad aruande erinevate üksuste või, kui nad teatavad, et sama olemi, nad peavad tagama allika ID-ga või atribuudi erineb. See viis aruannete saate koos. Atribuudi seisundi aruanne peaks jäädvustada jälgida tingimus. (Ülaltoodud näites atribuudi võib olla **ShareSize**.) Kui mitu aruannet rakendada sama tingimus, atribuudi peaks sisaldama mõned dünaamiline teavet, mis võimaldab aruannetes kõrvuti. Näiteks kui mitu ühtlasi on vaja jälgida, võib atribuudi nimi **ShareSize-kaustanimi**.

> [AZURE.NOTE] Tervise säilitamiseks peaks *ei* kasutada, et hoida olekuteavet. Ainult seotud teavet peaks olema staatusega seisund, nagu see teave mõjutab selle üksuse hindamine. Tervise säilitamiseks pole loodud üldotstarbeline säilitamiseks. Seisundi hindamise loogika kasutab koondatud kõik andmed seisundioleku. Teavet, mis pole seisund (nt aruandluse olek seisund olekus OK) saatmine ei mõjuta liidetud seisundioleku, kuid seda võivad jõudlust seisund poe negatiivselt mõjutada.

Järgmise otsust on mis üksus aruandele kohta. Enamikul juhtudel, on see nähtav, selle tingimuse alusel. Üksuse valida parima võimaliku granulaarsus abil. Kui tingimus mõjutab kõigi koopiate sektsiooni sisse, aruande sektsioonis, mitte teenuses. On nurgas juhtumeid, kus rohkem mõtlema on vaja, kuigi. Kui tingimus mõjutab üksus, nt koopia, kuid soov on, et tingimus, mis on lipuga märgitud rohkem kui koopia elu kestus, siis see esitatakse sektsiooni. Muul juhul koopia kustutamisel kõik sellega seotud aruanded kuvatakse puhastada poest. See tähendab, et valvekoer poolt mõtlema eluajal ja aruande. Peab olema selge, kui aruande tuleks puhastada poest (näiteks aru üksuse tõrge enam kehtib).

Vaatame näide, mis ühendab ma kirjeldatud punktid. Kaaluge teenuse struktuuri rakendus, mis koosneb juhtslaidi stateful püsivate teenuse ja teisene kodakondsuseta teenuste juurutatud kõik sõlmed (üks teisene teenus tüüp erinevat tüüpi tööülesande puhul). Juhteksemplari on töötlemine järjekorras, mis sisaldab käskude abil sekundaaride. Funktsiooni sekundaaride käivitada sissetulevad taotlused ja saada tagasi kinnituse signaale. Ühe tingimuse, mida võib jälgida on juhtslaidi töötlemise järjekorra pikkus. Kui juhtslaidi järjekorra pikkus jõuab piirmäära, teatatakse hoiatus. Hoiatus, et selle sekundaaride ei oska laadi. Kui järjekorda jõuab maksimumpikkus ja käsud kõrvaldatakse, teatatakse tõrge, kui teenus ei saa taastada. Aruannete võib atribuudi **QueueStatus**. Valvekoer elu teenuse sees ja see saadetakse perioodiliselt juhtslaidi esmane koopia. Time to live on kaks minutit ja see saadetakse perioodiliselt iga 30 sekundi järel. Kui esmane läheb alla, aruanne on puhastada automaatselt poest. Kui teenus koopia on üles, kuid see on ummikus või on muid probleeme, aruande aegub seisund poest. Sel juhul üksus on hinnatuna tõrge.

Teine tingimus, mida saate jälgida on tööülesande täitmise ajal. Juhteksemplari jaotab tööülesandeid sekundaaride, mis põhineb ülesande tüüp. Sõltuvalt kujundus, võib juhteksemplari küsitlus sekundaaride tööülesande oleku kohta. Selle võib ka oodata sekundaaride tagasi kinnituse signaale saata, kui need on valmis. Teisel juhul tuleb tuvastamiseks olukordades, kus sekundaaride die või sõnumid lähevad kaotsi. Üheks võimaluseks on juhteksemplari ping päringu saatmiseks sama teisene, mis saadab tagasi oma oleku kohta. Kui pole olek, juhteksemplari peab tõrke ja restruktureerinud tööülesanne. Selline käitumine eeldab, et ülesanded oleksid idempotent.

Jälgida tingimus saab tõlkida Hoiatus Kui toiming ei ole teha teatud aja (**t1**, näiteks 10 minuti). Kui tööülesanne on lõpule viidud aeg (**t2**, nt 20 minutit), saate jälgida tingimus tõlkida kui tõrge. Selle aruandluse saab teha on mitu võimalust:

- Juhtslaidi peamine koopia aruanded ise perioodiliselt. Saate määrata ühe atribuudi kõik ootel tööülesannete järjekorda. Kui vähemalt üks tööülesanne kulub oodatust, atribuudi **PendingTasks** aruande olek on hoiatuse või tõrketeate vastavalt vajadusele. Kui ootel ülesandeid pole või kõik tööülesanded alustamine täitmise, aruande olek on OK. Tööülesanded on püsiv. Kui esmane läheb alla, edasi äsja esiletõstetud esmase aruande õigesti.

- Mõne muu valvekoer protsessi (pilve või väline) kontrollib tööülesanded (alates väljaspool, soovitud ülesanne tulemi põhjal) kuvamiseks, kui need on lõpule viidud. Kui nad ei vasta piirmäärad, saadetakse aruande juhtslaidi teenuse. Aruande saadetakse ka iga tööülesandega, mis sisaldab tööülesande identifikaator, nt **PendingTask + taskId**. Aruannete saadetakse ainult vigane kohta. Kellaaja live paar minutit ning märkige aruanded, eemaldatakse need aeguvad Kettapuhastus tagamiseks.

- Teisese, mis on tööülesande täitmise aruannete kui kulub oodatust rohkem aega seda käivitamiseks. Selle atribuudi **PendingTasks**eksemplari aruanded. Aruande määratletud sellega, mis on probleeme eksemplari, kuid see ei jäädvustada olukorda, kus näiteks sureb. Aruannete on seejärel puhastada. See võib aruande teisene teenus. Kui teisese tööülesande lõpule jõudnud, kustutab teisene eksemplari aruande poest. Aruande ei jäädvustada olukorras, kus on kadunud kinnituse sõnumi tööülesanne on pooleli juhtslaidi isiku seisukohast.

Kuid aruandluse teinud eespool kirjeldatud juhtudel, aruannete on jäädvustatud rakenduse seisund kui seisund hinnatakse.

## <a name="report-periodically-vs-on-transition"></a>Aruande perioodiliselt vs üleminek
Seisund aruandlusteenuste mudeli abil saate valvekoerte saata aruannete perioodiliselt või üleminekud. Soovitatav viis aruannete valvekoer jaoks on perioodiliselt, kuna kood on palju lihtsam ja vigade tõenäosust. Funktsiooni valvekoerte peab püüdma olla võimalikult lihtne käivitamine vale aruannete vigade vältimiseks. Vale *vigane* aruanded mõju seisundi hindamine ja stsenaariumid, mis põhineb seisund, sh uuendamine. Vale *terve* aruannete peitmiseks probleemid, mis on soovitud klaster.

Perioodiliste aruannete jaoks saate rakendada mõne timer valvekoer. Ajastiteenuse tagasihelistamise, valvekoer saate vaadata ja saata aruande, mis põhinevad praeguses olekus. Ei ole vaja näha, milline aruanne saadeti varem või mis tahes osas sõnumside Optimeerimised. Seisund on partiide loogika aidata jõudlust. Ajal seisund kliendi hoitakse elus, korduvad katsed ettevõttesiseselt kuni aruande on kinnitanud seisund poe või valvekoer genereeritud uuem aruande sama olemi, atribuut ja allikas.

Üleminekud aruandluse jaoks on vaja ettevaatlik käsitlemine olekus. Valvekoer jälgib olekuid ja aruannete ainult siis, kui tingimustele muuta. Seda moodust tagurpidi on vaja on vähem aruandeid. Negatiivne on see, et valvekoer loogika on keeruline. Valvekoer tuleb säilitada tingimused, või aruanded, nii, et neid saab kontrollida muutuste määramiseks. Klõpsake Tõrkesiirde, tuleb kui aruande saadetakse, mis pole saadetud varem (järjekorras, kuid ei ole veel saadetud seisund pood). Järjekorranumber peab olema üha. Kui ei, aruanded on tagasi lükatud restoran. Kui andmete kaotsimineku tekib harva, võib olla vajalik sünkroonimise looma olek ja riigi seisund poe vahel.

Aruandlus üleminekud on mõistlik teenuste kaudu ise, aruandlus `Partition` või `CodePackageActivationContext`. Kui kohalik objekt (koopia või juurutatud teenuse paketi / kasutada rakenduse) on eemaldatud, kõigi oma aruannete eemaldatakse ka. See automaatne puhastus lõdvestab reporter ja seisundi poe sünkroonimiseks vaja. Kui aruanne on ema-või ema partition, tuleb sisse Tõrkesiirde vältimiseks aegunud aruannete seisund poest. Loogika tuleb lisada õige oleku säilitamiseks ja tühjendage aruande poest kui enam ei vaja.

## <a name="implement-health-reporting"></a>Rakendada seisund teatamine
Kui üksus ja aruande üksikasjad on selge, saata Seisundiaruannete saab teha API, PowerShelli või REST kaudu.

### <a name="api"></a>API-GA
Aruande API kaudu, peate aruande seisund kindla üksuse tüüp soovidega arvestamine aruande loomine. Andke aruandele seisund kliendile. Teise võimalusena loomine süsteemiseisundi teavet ja andke seda parandada aruandlusteenuste meetodite kohta `Partition` või `CodePackageActivationContext` praeguse üksustele teatada.

Järgmises näites on kujutatud perioodilised aruanded valvekoer klaster sees. Valvekoer kontrollib, kas on välised ressurss pääseb sõlm sees. Ressursi on vaja teenuse manifesti rakendusest. Kui ressurss on saadaval, muud teenused rakendusest saate siiski toimida. Seetõttu aruanne saadetakse juurutatud teenuse paketi üksuse iga 30 sekundi järel.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether the resource can be accessed from the node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as the connectivity is needed by the specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, the report is already queued on the health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until the report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShelli
Saatke Seisundiaruannete * *Saada-ServiceFabric*EntityType*HealthReport **.

Järgmises näites on kujutatud perioodiliste CPU väärtuste sõlme teatamine. Aruannete saadetakse iga 30 sekundi järel, ja neil on kaks minutit live aega. Kui need aeguvad, looma probleeme, et on sõlme on hinnatuna tõrge. Kui CPU on kõrgemal, aruandes on seisundioleku hoiatus. Kui CPU jääb rohkem kui konfigureeritud aja kõrgemal, on teatatud tõrke. Muul juhul looma saadab seisund olekus OK.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Järgmises näites aruanded siirdamiseks hoiatus koopia. Ta saab esmalt sektsiooni ID ja seejärel koopia ID teenus on huvi. See siis saadab aruande **PowershellWatcher** atribuudi **ResourceDependency**. Aruanne on huvi ainult kaks minutit ja see on eemaldatud poest automaatselt.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saatke Seisundiaruannete abil ülejäänud POST-päringud, mis valige soovitud üksus ja keha seisundi aruande kirjeldus. Näiteks, vaadake, kuidas ülejäänud [kobar Seisundiaruannete](https://msdn.microsoft.com/library/azure/dn707640.aspx) või [teenuse Seisundiaruannete](https://msdn.microsoft.com/library/azure/dn707640.aspx)saata. Kõik üksused on toetatud.

## <a name="next-steps"></a>Järgmised sammud

Seisundi andmete põhjal teenuse poolt ja kobar/administraatorid saavad mõtlema, kuidas teabe kasutamine. Näiteks need saate häälestada teatisi seisund oleku põhjal jõuda raske probleemid enne, kui nad põhjustada katkestuste. Administraatorid saate häälestada ka parandamise süsteemide automaatselt.

[Sissejuhatus teenuse struktuuri seisundi jälgimine](service-fabric-health-introduction.md)

[Teenuse struktuuri Seisundiaruannete kuvamine](service-fabric-view-entities-aggregated-health.md)

[Kuidas teatada ja teenuse seisundi kontrollimine](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Süsteemi seisundi aruannete kasutamiseks tõrkeotsing](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Saate jälgida ja teenuste kohalikult diagnoosimine](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md)
