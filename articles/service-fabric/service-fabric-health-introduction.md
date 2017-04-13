<properties
   pageTitle="Seisundi jälgimine teenuse struktuuri | Microsoft Azure'i"
   description="Jälgimise mudel, mis pakub jälgimise klaster ja selle rakenduste ja teenuste seisundi Azure teenuse struktuuri tutvustus."
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

# <a name="introduction-to-service-fabric-health-monitoring"></a>Sissejuhatus teenuse struktuuri seisundi jälgimine
Azure teenuse struktuuri tutvustab seisund mudelit, mis pakub rikkaliku, paindlik ja laiendatav hindamine ja teatamine. Kui seate mudeli võimaldab lähedal reaalajas hinnata klaster ja teenuseid, ei tööta. Saate teavet seisundi ja parandada võimalikud probleemid enne nende kaskaadi ja põhjustada suuri katkestuste. Tüüpilised mudeli teenuste saata aruannete põhjal saavad kohaliku vaadete ja, et anda üldist teavet liidetakse kobar taseme vaade.

Teenuse struktuuri komponendid selle rikkaliku seisund mudeli kasutamine oma praeguse oleku teatada. Saate sama süsteemi seisund aruande rakenduste kaudu. Kui te investeerida kvaliteetne seisund aruandlus, mis salvestab teie kohandatud tingimused, saate tuvastada ja palju kergemini töötava rakenduse seotud probleemide lahendamine.

> [AZURE.NOTE] Algas seisund allsüsteemi vajadust jälgida uuendamine. Teenuse struktuuri pakub rakendus ja kobar täienduste, mis tagavad täieliku kättesaadavuse, pole tööseisakute jälgida ja minimaalsete pole kasutaja sekkumist. Nende eesmärkide versioonitäienduse kontrollib seisund vastavalt konfigureeritud versioonitäienduse poliitikate ja võimaldab täiendamist jätkata ainult siis, kui seisund peab kinni soovitud lävede. Muul juhul uuele versioonile on automaatselt tagasi pöörata või peatatud, et anda administraatorid võimalus probleemide lahendamiseks. Lisateavet rakenduse uuendused, lugege [artiklit](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Tervise pood
Tervise säilitamiseks hoiab kobar otsimist ja hindamise üksuste seotud teavet. Seda rakendatakse siis, kui teenuse struktuuri püsis stateful teenus kättesaadavuse ja skaleeritavus. Tervise säilitamiseks on osa selle **struktuuri: / süsteemi** rakendus ja see on saadaval, kui klaster on loodud ja töötab.

## <a name="health-entities-and-hierarchy"></a>Üksuste seisundi ja hierarhia
Üksuste seisund on korraldatud loogilise hierarhia, mis salvestab kasutusviisid ja sõltuvused erinevate üksuste vahel. Tervise säilitamiseks põhjal aruandeid, mis on saadud teenuse struktuuri komponendid automaatselt loodud üksused ja hierarhia.

Üksuste seisundi kajastavad teenuse struktuuri üksused. (Nt **seisund rakenduse üksus** vastab rakenduse eksemplari juurutatud kobar, samal ajal **seisund sõlm üksus** vastab teenuse struktuuri kobar sõlm.) Seisund hierarhia jäädvustab süsteemi üksuste kasutusviisid ja Täpsem seisundi hindamise aluseks on. Saate teada, [teenuse struktuuri tehniline ülevaade](service-fabric-technical-overview.md)põhilistest teenuse struktuuri kontseptsioonide kohta. Rakenduse kohta leiate lisateavet teemast [teenuse struktuuri mudel](service-fabric-application-model.md).

Seisund üksused ja hierarhia luba kobar ja tõhusalt esitada, silumisel ja jälgida rakendusi. Seisund mudel annab täpne, *Varundustöö* kujutis palju jooksva nuppe klaster seisundi.

![Seisund üksused.][1]
Üksuste seisundi korraldatud hierarhia põhjal ema-tütre seose.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Seisund üksused on:

- **Kobar**. Tähistab teenuse struktuuri kobar seisundi. Kobar Seisundiaruannete kirjeldada tingimused, mis mõjutavad kogu kobar ja ei saa vähendada ühe või mitme vigane alla. Näiteks aju klaster tükeldamise eraldamine või side võrguprobleemide tõttu.

- **Sõlm**. Tähistab teenuse struktuuri sõlm seisundi. Sõlm Seisundiaruannete kirjeldada tingimusi, mis mõjutavad sõlme funktsioonid. Need mõjutavad tavaliselt kõik juurutatud üksuste see töötab. Näiteks kui sõlm on välja vaba ruumi (või mõne muu hõlmav atribuudi, nt mälu ühendused) ja kui sõlm on alla. Sõlm üksus on tähistatud sõlme nimi (stringi).

- **Rakenduse**. Tähistab seisundi klaster töötavad rakenduse eksemplari. Rakenduse Seisundiaruannete kirjeldada tingimused, mis mõjutavad rakenduse üldist tervist. Ta ei saa vähendada üksikute alla (teenused või juurutatud rakendused). Näiteks rakenduse eri teenuste vahelise suhtluse lõpuni. Rakenduse üksus on tähistatud rakenduse nimi (URI).

- **Teenus**. Tähistab klaster töötab teenuse seisund. Teenuse Seisundiaruannete kirjeldada tingimused, mis mõjutavad üldised teenuse seisundi ja nad ei saa vähendada sektsiooni või koopia. Näiteks teenuse konfigureerimine (nt pordi või välise failikettal) põhjustav kõik partitsioonid probleemid. Teenuse üksus on tähistatud teenuse nimi (URI).

- **Sektsiooni**. Tähistab sektsiooni teenuse seisund. Sektsiooni Seisundiaruannete kirjeldada tingimusi, mis mõjutavad kogu koopia määramine. Näiteks kui vastuste arv on alla target arv ja kui sektsiooni on kvoorumi kaotsiminekut. Sektsiooni üksus on tähistatud sektsiooni ID (GUID).

- **Koopia**. Tähistab seisundi stateful teenuse koopia või kodakondsuseta teenuse eksemplari. Väikseim üksus, mille valvekoerte ja komponentide saate aruande rakenduse. Stateful teenuste, näiteks peamine koopia, aruandlus, kui seda ei saa korrata toiminguid sekundaaride ja kui kopeerimine on takistada oodatud tempos. Kodakondsuseta eksemplari saate teatada ka, kui see hakkab otsa ressursid või on probleeme. Üksuse koopia on tähistatud sektsiooni ID (GUID) ja koopia või eksemplari ID (pikk).

- **DeployedApplication**. Mõni *rakendus töötab sõlme*seisundi tähistab. Juurutatud rakendusega Seisundiaruannete kirjeldatakse sõlm, mida ei saa vähendada teenuse pakette juurutatud sama sõlme rakenduse seotud tingimusi. Näiteks kui rakenduse pakett ei saa alla sõlme ja probleemi häälestamise rakenduse turvalisus kasutajad sõlme korral. Juurutatud rakendusega on tähistatud rakenduse nimi (URI) ja sõlme nimi (stringi).

- **DeployedServicePackage**. Tähistab seisundi töötavate sõlme klaster pakett. See kirjeldab teatud pakett tingimused, mis ei mõjuta muid pakettide jaoks sama rakenduse sama sõlme. Näiteks koodi paketi pakett, mida ei saa käivitada ja konfiguratsiooni pakett, mida ei saa. Juurutatud pakett on tähistatud rakenduse nimi (URI), sõlme nimi (stringi) ja teenuse manifest nimi (stringi).

Seisund mudeli granulaarsus lihtne avastada ja parandada probleemid. Näiteks kui teenus ei vasta, on võimalik, et rakenduse eksemplari on vigane teatada. Aru, et tase ei ole optimaalne, kuid kuna probleem võib olla mõjutamata kõiki teenuseid sellest rakendusest. Aruande rakendatakse Vigane teenuse või teatud lapse sektsiooni, kui Lisateavet osutab sektsiooni. Andmete automaatne pind hierarhia ja vigane sektsiooni kaudu tehakse nähtav teenuseid ja rakendusi tasanditel. See koondamine aitab Pinpointi ja probleemi põhjuseks kiiremini lahendada.

Hierarhia seisund on ema-tütre seose. Klaster koosneb sõlmed ja rakendused. Rakendused on teenused ja juurutatud rakendused. Juurutatud rakenduste juurutanud paketid. Teenused on sektsioonid ja iga sektsiooni on üks või mitu koopiad. On teisiti seos sõlmed ja juurutatud üksuste vahel. Kui sõlm on vigane, nagu oma asutuse osa (Tõrkesiirde halduri teenus), see mõjutab juurutatud rakendused, pakettide ja seda juurutatud koopiad.

Seisund hierarhia tähistab vastavalt põhineb seisund uusimaid aruandeid, mis on peaaegu reaalajas teavet süsteem.
Siseste ja väliste valvekoerte saate aruande samad üksused, mis on rakenduse kohased loogika või kohandatud jälgida tingimuste alusel. Kasutaja aruanded koos aruannetega süsteem.

Kuidas teatada ja neile vastamine seisund teenuse hõlbustamiseks silumine, jälgimine ja kasutada suurte pilveteenuses kujundamisel investeerida kavandamine.

## <a name="health-states"></a>Seisund Ühendriigid
Teenuse struktuuri kasutatakse kolme seisundi olekus kirjeldamiseks, kas üksus on terve või mitte: OK, hoiatus ja viga. Aruande, mis saadetakse seisund poe peate määrama ühe sellise. Seisundi hindamise tulemus on ühe sellise.

On võimalik [seisundi olekus](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) .

- **OK**. Üksus on terve. On pole teadaolevad probleemid teatatud või tema tütardele (kui see on rakendatav).

- **Hoiatus**. Üksuse kogemusi mõned probleemid, kuid see pole veel vigane (nt on viivitusi, kuid ei põhjusta otstarbekas probleemidest veel). Mõnel juhul hoiatus tingimus võib parandada ise teisiti sekkumiseta ja see on kasulik nähtavus, mis toimub. Muudel juhtudel võib hoiatus tingimus halvendada kasutaja sekkumiseta raske probleem.

- **Tõrge**. Üksus on vigane. Toimingu tuleb parandada riigi üksuse, kuna see ei tööta õigesti.

- **Pole teada**. Üksuse pole seisund pood. Järgmise tulemi aadressil jaotatud päringute ühendamine tulemusi mitme komponendid. Näiteks saate läheb **FailoverManager** ja **HealthManager**, too rakenduste loendi päring läheb **ClusterManager** ja **HealthManager**sõlm päring. Need päringud ühendada mitu osad tulemusi. Kui teine osa tagastab ettevõte, mis on jõudnud või puhastada üles poest seisund, ühendatud tulem on tundmatu seisundioleku.

## <a name="health-policies"></a>Rakendamist
Tervise säilitamiseks kehtib rakendamist kas üksus on terve vastavalt oma aruandeid ja tema tütardele.

> [AZURE.NOTE] Rakendamist saab määrata kobar manifest (kobar ja sõlm hindamine) või Rakendusmanifest (rakenduse hindamiseks ja selle laste) jaoks. Seisundi hindamise taotlusi edasi anda kohandatud seisundi hindamise poliitikat, mida kasutatakse ainult selle hindamise.

Vaikimisi kehtib teenuse struktuuri hierarhilise ema-tütre seose range reeglid (kõik peab olema terve). Kui üks lapsed on üks vigane sündmus, käsitletakse ema vigane.

### <a name="cluster-health-policy"></a>Kobar seisund poliitika
[Kobar seisund poliitika](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) kasutatakse väärtustada kobar seisundioleku ja sõlm seisund olekus. Saate määratleda poliitika kobar manifestis. Kui seda pole olemas, kasutatakse vaikepoliitika (lubatud tõrkeid null).
Kobar seisund poliitika sisaldab:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Saate määrata, kas kasutatakse hoiatus seisundi aruanded nimega tõrgete ajal hindamine. Vaikimisi: false.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Määrab lubatud rakendusi, mis võib olla vigane enne klaster käsitletakse tõrge.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Määrab suurim lubatud protsent sõlmed, mis võib olla vigane enne klaster käsitletakse tõrge. Suurtes rühmades, mõned sõlmed on alati alla või välja parandamine, et konfigureeritud luba, et see protsent.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). Rakenduse tüüp seisundi poliitika kaardi saab kobar seisundi hindamisel teisiti rakenduse tüüpi kirjeldamiseks. Vaikimisi on kõik rakendused on pannakse ja hinnatud koos MaxPercentUnhealthyApplications. Kui rakenduse Mõned sõnumitüübid, tuleks käsitleda erinevalt, saate neid võtta kogumisse välja. Selle asemel nende hindamise seostatud tema rakenduse tüüp nimi kaardi protsendid suhtes. Näiteks klaster on tuhandete eri tüüpi rakendusi ja mõni juhtelement rakenduse eksemplari teisiti rakenduse tüüp. Juhtelemendi rakenduste ei tohi kunagi olla tõrge. Saate määrata globaalne MaxPercentUnhealthyApplications 20% luba teatud tõrkeid, kuid rakenduse tüüp "ControlApplicationType" on MaxPercentUnhealthyApplications väärtuseks 0. Sellisel viisil, kui palju rakendusi on vigane, kuid all globaalne vigane protsenti, tuleks väärtustada klaster hoiatus. Hoiatus tervise riik ei mõjuta kobar täiendamine või muude jälgimise käivitab tõrge seisundioleku. Kuid isegi ühe juhtelemendi rakenduse tõrge teeks kobar vigane, mis käivitab tagasi või peatab kobar uuele versioonile, versioonitäienduse konfiguratsioonist.
Rakenduste puhul, määratletud kaardi, on kõik teenuserakenduse eksemplaride kogumisse rakenduste välja võtta. Nende rakenduste tüübi, kasutades teatud MaxPercentUnhealthyApplications kaart koguarvu põhjal hindamise. Kõik ülejäänud rakenduste jäävad kogumisse ja analüüsitakse koos MaxPercentUnhealthyApplications.

Järgmises näites on väljavõte kobar manifestis. Rakenduse tüüp MAPI kirjed määratlemiseks eesliide "ApplicationTypeMaxPercentUnhealthyApplications-", tippige rakenduse nimi, millele järgneb parameetri nimi.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Rakenduse seisund poliitika
[Rakenduse seisund poliitika](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) kirjeldatakse, kuidas hindamine lapse-olekus koondamine ja sündmused on tehtud rakenduste ja tema tütardele. Selle saate määratleda Rakendusmanifest, **ApplicationManifest.xml**, rakenduse pakett. Kui määratud on ei poliitikat, teenuse struktuuri eeldab, et üksus on vigane, kui see sisaldab seisundi aruande või lapse seisundioleku hoiatuse või tõrketeate.
Konfigureeritav poliitika on:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Saate määrata, kas kasutatakse hoiatus seisundi aruanded nimega tõrgete ajal hindamine. Vaikimisi: false.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Määrab lubatud juurutatud rakendusi, mis võib olla vigane enne taotlus loetakse tõrge. See protsent jagatakse vigane juurutatud taotlustest sõlmed, mis rakendused on praegu juurutatud klaster arvu. Arvutamisel Ümardab ühe tõrge väikese arvu sõlmed luba. Vaikimisi protsent: null.

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Saate määrata teenuse tüüp seisundi vaikepoliitika, mis asendab rakenduse kõigi teenuse seisund vaikepoliitika.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Pakub kaardi teenuse seisund poliitikate kohta teenuse tüüp. Need poliitikad asendada vaikimisi teenuse tüüp seisundi poliitikate iga määratud teenuse tüüp. Näiteks kui rakendus on kodakondsuseta lüüsi teenuse tüüp ja stateful engine teenuse tüüp, saate konfigureerida seisund poliitikate hindamiseks teisiti. Kui määrate poliitika kohta teenuse tüüp, pääsete täpsema juhtimise teenuse seisund.

### <a name="service-type-health-policy"></a>Teenuse tüüp seisundi poliitika
[Teenuse tüüp seisundi poliitika](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) täpsustab, kuidas hinnata ja liitmine teenuste ja teenuste lapsed. Poliitika sisaldab:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Enne teenus on vigane, saate määrata vigane sektsioonid suurim lubatud protsent. Vaikimisi protsent: null.

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Enne sektsiooni on vigane, saate määrata suurim lubatud protsent vigane koopiad. Vaikimisi protsent: null.

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Enne rakendus on vigane, saate määrata vigane teenuste suurim lubatud protsent. Vaikimisi protsent: null.

Järgmises näites on mõni Rakendusmanifest väljavõte:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Hindamine
Kasutajate ja automatiseeritud teenuste saate igal ajal väärtustada tervis üksus. Hinnata ettevõtte seisund, seisundi poe agregaadid kõik seisundi aruanded üksus ja hindab kõik tema tütardele (kui see on rakendatav). Seisund koondamine algoritmi kasutab seisund poliitikad, mis määravad, kuidas hinnata Seisundiaruannete ning kuidas liita lapse seisundi Ühendriigid (kui see on rakendatav).

### <a name="health-report-aggregation"></a>Seisund aruande koondamine
Ühe üksuse võib olla mitu Seisundiaruannete saadetud erinevate ajakirjanikele (osad või valvekoerte) erinevate atribuudid. Liitmise kasutab seotud seisund poliitikad, eriti rakenduse või kobar seisund poliitika ConsiderWarningAsError liige. ConsiderWarningAsError täpsustab, kuidas hinnata hoiatused.

Liidetud seisundioleku käivitab *kehvem* Seisundiaruannete olemi. Kui seal on vähemalt üks tõrkearuanne, liidetud seisundioleku on viga.

![Seisund aruande liita tõrkearuanne.][2]

Seisund üksus, mis on ühe või mitme tõrge Seisundiaruannete hinnatakse tõrge. Sama kehtib aegunud seisundiaruanne sõltumata seisu seisund.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Kui pole tõrketeated ja üks või mitu hoiatust, on liidetud seisundioleku hoiatus või viga, olenevalt ConsiderWarningAsError poliitika lipp.

![Seisund aruande koondamine hoiatus aruanne ja ConsiderWarningAsError false.][3]

Seisund aruande koondamine hoiatus aruanne ja ConsiderWarningAsError väärtuseks false (vaikesäte).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Lapse seisund koondamine
Liidetud seisundioleku üksus kajastab lapse seisundi Ühendriigid (kui see on rakendatav). Algoritmi jaoks liitmise lapse seisundi olekus kasutab seisund poliitikate korral põhineb üksuse tüüp.

![Lapse üksuste seisundi koondamine.][4]

Lapse koondamine põhjal rakendamist.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Pärast poe seisund on hinnatud kõik lapsed, see liitmise nende põhjal konfigureeritud maksimaalne osakaal vigane laste seisund riigid. See protsent on võetud poliitika üksuse- ja tütarüksuste tüübist.

- Kui kõik lapsed on OK Ühendriigid, on lapse kokkuvõtliku seisundioleku OK.

- Kui lapsed on nii hoiatus olekud ja OK, lapse kokkuvõtliku seisundioleku on hoiatus.

- Kui tõrge olekus selles osas maksimaalselt lubatud protsent vigane laste laste, liidetud seisundioleku on viga.

- Kui tõrge riikide laste osas vigane laste suurim lubatud protsent, liidetud seisundioleku on hoiatus.

## <a name="health-reporting"></a>Seisund teatamine
Teenuse struktuuri üksuste suhtes saate aruande osad, süsteemi struktuuri rakendused ja sisemine/väline valvekoerte. Reporteritele teha *kohaliku* määramised tervise jälgitud isikud, nad jälgivad tingimuste alusel. Ta ei pea globaalne riik või koondandmete vaadata. Soovitud käitumist on lihtne reportereid ja pole keerukate vastu, mis on vaja vaadata paljusid asju, millist teavet soovite saada tuletamine.

Seisund poe seisundi andmeid saata, peab reporter tuvastamine probleemse üksus ja seisundi aruande loomine. Aruande saate saata, kasutades [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx)API kaudu või läbi ülejäänud PowerShelli kaudu.

### <a name="health-reports"></a>Seisundiaruannete
[Seisundiaruannete](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) iga klaster üksuste sisaldavad järgmist teavet:

- **SourceId**. String, mis tuvastab kordumatult looma seisund sündmuse.

- **Üksuse identifikaator**. Tuvastab üksus, kus on rakendatud aruanne. See väärtus erineb sõltuvalt [üksuse tüüp](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Kobar. Mitte keegi.

  - Sõlm. Sõlme nimi (stringi).

  - Rakendus. Rakenduse nimi (URI). Tähistab juurutatud klaster rakenduse eksemplari nimi.

  - Teenus. Teenus nimi (URI). Tähistab klaster juurutatud teenuse eksemplari nimi.

  - Sektsiooni. Sektsiooni ID (GUID). Tähistab partition kordumatut tunnust.

  - Koopia. Stateful teenuse koopia ID või kodakondsuseta teenuse eksemplari ID (INT64).

  - DeployedApplication. Rakenduse nimi (URI) ja sõlme nimi (stringi).

  - DeployedServicePackage. Rakenduse nimi (URI), sõlme nimi (stringi) ja teenuse näidata nimi (stringi).

- **Atribuut**. *String* (mitte fikseeritud loendamine) mis võimaldab liigitamiseks seisund sündmuse kindla üksuse atribuudi looma. Näiteks saate reporter A aruande Node01 "storage" atribuut ja reporter B saate aruande atribuudi Node01 "Ühenduvus" seisundi. Eraldi seisund sündmuste Node01 olemi käsitletakse seisund poest neid aruandeid.

- **Kirjeldus**. String, mis võimaldab reporter seisund sündmuse kohta üksikasjaliku teabe. **SourceId**, **atribuut**ja **HealthState** kirjeldama täielikult aruanne. Kirjeldus lisab aruande inimeste loetav teavet. Teksti hõlbustab administraatoritele ja kasutajatele mõista seisundi aruanne.

- **HealthState**. [Loend](service-fabric-health-introduction.md#health-states) , mis kirjeldab seisundioleku aruanne. Aktsepteeritud väärtused OK, hoiatused ja tõrge.

- **TimeToLive**. On aeg, mille näitab kaua seisundi aruanne on lubatud. Koos **RemoveWhenExpired**, see võimaldab teada, kuidas hinnata aegunud sündmuste seisund pood. Vaikimisi väärtus on lõpmatu ja aruande kehtib terve igavik.

- **RemoveWhenExpired**. On kahendväärtus. Kui on seatud tõene, aegunud seisundiaruanne eemaldatakse automaatselt seisundi pood ja aruande ei mõjutada üksuse hindamine. Kasutada, kui aruanne on määratud perioodi jooksul ainult ja looma ei pea konkreetselt tühjendage see. Seda kasutatakse ka kustutada aruannete poest seisund (näiteks valvekoer on muutunud ja lõpetab eelmise lähte- ja atribuudi aruannete saatmine). Selle saate saata aruande koos lühikese TimeToLive koos RemoveWhenExpired puhastada kõik endise seisundi poest. Kui väärtus on FALSE, käsitletakse aegunud aruande seisundi hindamise tõrge. Väär väärtus signaale seisundi pood, et allika peaks regulaarselt aruande atribuut. Kui seda pole, siis tuleb midagi valesti valvekoer. Funktsiooni valvekoer seisund tegemist leides tõrke sündmust.

- **SequenceNumber**. Positiivne täisarv, mis peab olema üha, tähistab see aruannete järjestuse. Seda kasutatakse seisund poe aegunud aruandeid, mis on saanud hilinenud, sest võrgu viivitused või muude probleemide tuvastamiseks. Aruande on tagasi lükatud, kui järjenumber on väiksem või võrdne kõige viimati rakendatud numbri jaoks sama olemi, lähte- ja atribuut. Kui see pole määratud, luuakse automaatselt järjenumber. See on vaja panna järjekorranumber ainult siis, kui aruandlus oleku üleminekuid. Sellisel juhul peab allika meeles pidada, mis aruannete see saadetud ja taastamise kohta Tõrkesiirde teavet säilitada.

Need neli andmeühikuga teabe--SourceId, üksuse ID, atribuut ja HealthState--jaoks vajalike iga seisundiaruanne. String pole lubatud alustada SourceId eesliite "**System.**", mis on mõeldud süsteemi aruanded. Et sama olemi on ainult üks aruande sama lähte-kui ka atribuudi. Mitme aruannete jaoks sama lähte-kui ka atribuudi alistada üksteisest, seisund kliendipoolse (kui need on batched) või seisundi salvestada pool. Asendamine põhineb järjekorranumbreid; uuem aruanded (järjenumbritega kõrgema) asendada vanema aruanded.

### <a name="health-events"></a>Seisund sündmused
Sees, hoiab seisund poe [seisund sündmused](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), mis sisaldab kogu teavet, aruandeid ja täiendavad metaandmed. Metaandmete sisaldab aruande anti seisund kliendi aeg ja serveripoolse muutmise aja. Sündmuste seisund on tagastatud [seisund päringud](service-fabric-view-entities-aggregated-health.md#health-queries).

Lisatud metaandmete sisaldab:

- **SourceUtcTimestamp**. Aeg aruande anti kliendi seisund (koordineeritud maailmaaega).

- **LastModifiedUtcTimestamp**. Aruande serveripoolse (koordineeritud maailmaaega) viimase muutmise aeg.

- **IsExpired**. Lipp näitab, kas aruanne on aegunud, kui päring on täidetud seisund poest. Sündmuse saate aegunud ainult siis, kui RemoveWhenExpired on väär. Muul juhul sündmus ei tagastata päringu ja eemaldatakse poest.

- **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. Viimast for OK/hoiatus/viga üleminekud. Need väljad anda seisundi ajalugu oleku üleminekuid sündmus.

Väljad olek ülemineku saab nutikamalt teatiste või "ajalooliste" seisundi sündmuse teabe. Need võimaldavad stsenaariumid, näiteks:

- Teavita, kui atribuut on hoiatus/tõrke rohkem kui X minutit. Teatiste ajutiste tingimuste seisukord aja jooksul aitab vältida. Näiteks kui seisundioleku on rohkem kui 5 minuti jooksul hoiatus teatise saab tõlkida (HealthState == hoiatus ja nüüd - LastWarningTransitionTime > 5 minutit).

- Teatiste tingimustel, mis on muutunud viimase X minutit. Kui aruande oli juba tõrge enne määratud aja jooksul, võib eirata, kuna see oli juba varem märku.

- Kui atribuut on lülitamise hoiatus ja viga, kindlaks teha, kui kaua on vigane (ehk teisisõnu öeldes pole OK). Näiteks kui atribuudi ei ole terve rohkem kui 5 minuti teatise saab tõlkida (HealthState! = Ok ja nüüd - LastOkTransitionTime > 5 minutit).

## <a name="example-report-and-evaluate-application-health"></a>Näide: Aruande ja hindate rakenduse seisund
Järgmises näites saadab seisundiaruanne PowerShelli kaudu rakenduse **struktuuri: / WordCount** **MyWatchdog**allikast. Seisundi aruanne sisaldab teavet seisundi atribuudi "kättesaadavus" tõrkeolekus seisund, kus lõpmatu TimeToLive. Seejärel päringute rakenduse seisund, mis tagastab kokkuvõtliku seisund olekus tõrgete ja teatatud seisund sündmuste seisund sündmuste loendis.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Seisund mudeli kasutamine
Mudeli seisund võimaldab pilveteenustega ja aluseks teenuse struktuuri platvorm mastaapida, kuna jälgimine ja seisundi määramised jagatakse erinevaid kuvari klaster sees.
Muudest süsteemidest on laiendamist teenuse sõelub kõik *potentsiaalselt* kasulikku teavet teenuste kiiratava kobar tasemel. Seda moodust takistab nende skaleeritavus. See ei võimaldaks koguda väga kindlale teabele, mis aitab tuvastada probleemid ja võimalikud probleemid nimega põhjus võimalikult lähedal.

Seisund mudelit kasutatakse tugevalt diagnostika-ja, kobar-ja rakenduse hindamiseks ja jälgida uuendamine. Muude teenuste seisundi andmete abil teha automaatne parandamine, koostada kobar seisund ajalugu ja probleemi teatiste teatud tingimustel.

## <a name="next-steps"></a>Järgmised sammud
[Teenuse struktuuri Seisundiaruannete kuvamine](service-fabric-view-entities-aggregated-health.md)

[Süsteemi seisundi aruannete kasutamiseks tõrkeotsing](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Kuidas teatada ja teenuse seisundi kontrollimine](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Lisada kohandatud teenuse struktuuri seisundiaruanded](service-fabric-report-health.md)

[Saate jälgida ja teenuste kohalikult diagnoosimine](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md)
