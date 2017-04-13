<properties
   pageTitle="Kasutades Elasticsearch teenuse struktuuri rakenduse Jälita poe | Microsoft Azure'i"
   description="Kirjeldab, kuidas teenuse struktuuri rakenduste abil saate Elasticsearch ja Kibana talletada, index, ja otsida rakenduse jälgi (logid)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Kasutage Elasticsearch teenuse struktuuri rakenduse jälgida talletamine
## <a name="introduction"></a>Sissejuhatus
Selles artiklis kirjeldatakse, kuidas [Azure teenuse struktuuri](https://azure.microsoft.com/documentation/services/service-fabric/) rakendusi saate kasutada **Elasticsearch** ja **Kibana** rakenduse Jälita talletamine, indekseerimine ja otsing. [Elasticsearch](https://www.elastic.co/guide/index.html) on avatud lähtekoodi, jaotatud ja scalable reaalajas otsingu ja analüüsi mootor, mis sobib hästi selle ülesande jaoks. See saab installida Windows ja Linux töötab Microsoft Azure'i virtuaalmasinates. Elasticsearch efektiivselt töödelda *liigendatud* jälgi toodeti abil, nagu **Sündmuse jälgimine for Windows (ETW)**.

ETW kasutab teenuse struktuuri runtime diagnostikateave source (jälgi). See on soovitatav meetod teenuse struktuuri rakendusi hankida oma diagnostikateave, liiga. Sama kasutamist võimaldab korrelatsiooni käitusaja esitatud ja rakenduse esitatud jälgi ja muudab tõrkeotsingu hõlbustamiseks. Teenuse struktuuri projekti Mallid Visual Studio kaasata logimine API (vastavalt.NET **EventSource** klassi), mis eraldab vaikimisi ETW jälgi. Ülevaate teenuse struktuuri rakenduse jälgimine ETW abil, vt [jälgimine ja diagnoosimise kohalikus arvutis arengu häälestamise teenused](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Jälgi kuvamiseks Elasticsearch, tuleb jäädvustab teenuse struktuuri kobar sõlmed reaalajas (kui rakendus töötab) ja saadetakse Elasticsearch lõpp. On kaks põhilist võimalust Jälita hõivamine.

+ **Protsess Jälita hõivamine**  
Rakenduse või teenuse protsessi täpsemalt, vastutab saates Diagnostikaandmete Jälita pood (Elasticsearch).

+ **Protsessi-, Jälita hõivamine**  
Eraldi agent hõivamine jälgi teenuse protsessi või protsesside ja saata neile Jälita pood.

Allpool, me on kirjeldatud, kuidas luua Elasticsearch Azure, käsitletakse selle spetsialistid ja miinuste nii jäädvustada suvandid ning selgitatakse, kuidas konfigureerida teenuse struktuuri teenuse Elasticsearch andmeid saata.


## <a name="set-up-elasticsearch-on-azure"></a>Azure Elasticsearch häälestamine
Kõige lihtsam viis Elasticsearch teenuse Azure häälestamiseks on [**Azure ressursihaldur**](../azure-resource-manager/resource-group-overview.md)mallid. Täielik [Kiirjuhend Azure'i ressursihaldur malli Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) on saadaval Azure Kiirjuhend mallide hoidla. See mall kasutab eraldi salvestusruumi kontod skaala üksuste (sõlmed rühmad). Saate ka säte, eraldi kliendi ja serveri sõlmed koos eri konfiguratsioone ja andmete ketast, millele on manustatud erinevate arvud.

Siin kasutame teise malli, nimetatakse **ES-MultiNode** [Azure diagnostikatööriistu hoidla](https://github.com/Azure/azure-diagnostics-tools). See mall on hõlpsam ja loob Elasticsearch kobar, mis kaitsevad HTTP lihttekstautentimine. Enne jätkamist alla laadida hoidla GitHub teie arvutis (, kas kloonimine hoidla või alla zip-fail). Sama nimega kaust asub ES-MultiNode mall.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Masina Elasticsearch installi skriptide käitamiseks ettevalmistamine
Lihtsaim viis ES-MultiNode malli kasutamiseks on nimega esitatud Azure PowerShelli skripti kaudu `CreateElasticSearchCluster`. Selle skripti kasutamiseks on vaja installida PowerShelli moodulid ja tööriista nimega **openssl**. Viimane on vaja luua SSH võti, Elasticsearch klaster kaugühenduse teel haldamiseks kasutatavate.

`CreateElasticSearchCluster`Script on mõeldud kasutuslihtsus ES-MultiNode malli abil Windowsi seadmes. On võimalik kasutada malli, mitte Windowsi arvutisse, kuid see stsenaarium, mis on selles artiklis ei käsitleta.

1. Kui need pole juba installitud, installige [**Azure'i PowerShelli moodulid**](http://aka.ms/webpi-azps). Kui kuvatakse vastav viip, klõpsake **käivitada**, seejärel **installimine**. Nõutav on Azure PowerShelli 1.3 või uuemat versiooni.

2. **Openssl** tööriist sisaldub [**Windowsi Git**](http://www.git-scm.com/downloads)jaotuse. Kui te pole seda veel teinud, installige [Windowsi Git](http://www.git-scm.com/downloads) . (Vaikimisi installisuvandid on OK).

3. Eeldades, et Git on installitud, kuid ei sisalda süsteemi tee, avage Microsoft Azure'i PowerShelli aken ja käivitage järgmine käsk:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Asendada selle `<Git installation folder>` Git asukoht teie arvutis; Vaikimisi on **"C:\Program Files\Git"**. Pange tähele semikoolon märgi esimene tee alguses.

4. Veenduge, et olete sisse logitud Azure (kaudu [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet) ja, et olete valinud tellimus, mida tuleks kasutada klaster elastne otsingu loomine. Saate kontrollida õige tellimus on valitud, kasutades `Get-AzureRmContext` ja `Get-AzureRmSubscription` cmdlet-käsud.

5. Kui te pole seda veel teinud, muuta praeguse kataloogi ES-MultiNode kausta.

### <a name="run-the-createelasticsearchcluster-script"></a>Käivitage CreateElasticSearchCluster skript
Enne skripti käivitamiseks avage soovitud `azuredeploy-parameters.json` fail ja kontrollige või skripti parameetrite väärtused ette. Järgmiste parameetrite on esitatud.

|Parameetri nimi           |Kirjeldus|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |Nimi, mida kasutatakse loomiseks elastne otsingu kobar avalikult nähtav DNS-i nimi (Azure'i piirkond domeeni lisamine esitatud nimi). Kui selle parameetri väärtus on "myBigCluster" ja Azure piirkonnas on Lääne USA, on tulemuseks DNS-i nimi klaster myBigCluster.westus.cloudapp.azure.com. <br /><br />Sellise nimega toimib ka palju esemeid seostatud elastne otsingu kobar, nt andmete sõlm nimed root nimi.|
|adminUsername           |Administraatori konto nimi haldamise elastne otsingu kobar (vastavate SSH klahvid luuakse automaatselt).|
|dataNodeCount           |Elastne otsingu kobar sõlmed arv. Skripti praegune versioon ei erista andmed ja päringu sõlmed; kõik sõlmed esitada mõlemad rollid. 3 sõlmed vaikesätted.|
|dataDiskSize            |Andmete ketast (GB) jaotises suurus, mis eraldatakse iga andmete sõlme. Iga sõlme saab 4 andmete ketast, elastne otsinguteenuse mõeldud.|
|piirkond                  |Azure'i piirkond, kus elastne otsingu kobar peaksid asuma nimi.|
|esUserName              |Kasutajanime ja kasutaja, mis on konfigureeritud juurdepääsuks ES kobar (kehtivad HTTP elementaarautentimine). Parooli pole osa parameetrite fail ja peab andma, kui `CreateElasticSearchCluster` script on vaja järgida.|
|vmSizeDataNodes         |Azure virtuaalse masina suurus elastne otsingu kobar sõlmed. Vaikimisi Standard_D2.|

Nüüd olete valmis skripti käivitamiseks. Probleemi järgmine käsk:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

Kui 

|Skripti parameetri nimi    |Kirjeldus|
|-----------------------  |--------------------------|
|`<es-group-name>`        |Kõik elastne otsingu kobar ressursid sisaldavate Azure ressursirühma nimi.|
|`<azure-region>`         |Azure'i piirkond, kus luuakse elastne otsingu kobar nimi.|         
|`<es-password>`          |Elastne otsingu kasutaja parooli.|

>[AZURE.NOTE] Kui teile kuvatakse mõni NullReferenceException Test-AzureResourceGroup cmdlet-käsk, unustasite Azure'i sisselogimiseks (`Add-AzureRmAccount`).

Kui kuvatakse tõrketeade skripti kaudu ja olete kindlaks teinud, et tõrge põhjustas vale malli parameetri väärtuse, vea parameetri faili ja käivitage skript uuesti erinevate ressursside rühma nime. Saate uuesti kasutada sama ressursi rühma nime ja on skripti puhastada vana, lisades selle `-RemoveExistingResourceGroup` skripti kutsumise-parameeter.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Tulem CreateElasticSearchCluster skripti käivitamine
Pärast selle `CreateElasticSearchCluster` skripti, luuakse järgmised peamised esemeid. Selle näite puhul me oletame, et olete kasutanud "myBigCluster" väärtus on `dnsNameForLoadBalancerIP` parameetri ja kui olete loonud klaster piirkondade Lääne US.

|Artefakt|Nime, asukoha ja kommentaarid|
|----------------------------------|----------------------------------|
|SSH võti kaughalduse |myBigCluster.key faili (kataloogis, kust on CreateElasticSearchCluster käivitanud). <br /><br />Võtme faili saab administraator sõlm ja (administraator sõlm) kaudu ühenduse andmete sõlmed klaster.|
|Administraator sõlm                        |myBigCluster-admin.westus.cloudapp.azure.com <br /><br />Sihtotstarbeline VM Elasticsearch kobar kaughalduse--ainus, mis võimaldab SSH väliste ühenduste jaoks. See töötab kõik Elasticsearch kobar sõlmed virtuaalse samasse võrku, kuid see ei tööta Elasticsearch teenused.|
|Andmete sõlmed                        |myBigCluster1... myBigCluster*N* <br /><br />Andmete sõlmed, mis töötab Elasticsearch ja Kibana teenused. Saate luua ühenduse iga sõlme SSH, kuid ainult sõlm halduskeskuse kaudu.|
|Elasticsearch kobar             |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Esmane lõpp-punkti jaoks Elasticsearch kobar (Pange tähele /es järelliide). See on kaitstud HTTP elementaarautentimine (mandaadi olid määratud esUserName/esPassword parameetrid ES-MultiNode malli). Klaster on lihtne kobar halduse ka päisesse lisandmoodulit installida (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head).|
|Kibana teenus                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Kibana teenus on häälestatud loodud Elasticsearch kobar andmete kuvamiseks. See on kaitstud sama nimega ise klaster autentimiseks mandaadi.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>Protsessi või out protsess Jälita hõivamine
Sissejuhatus, saame mainitud kaks peamised viisid, kuidas Diagnostikaandmete kogumine: tootmisprotsessi ja out protsess. Iga on tugevuste ja puudusi.

**Protsess Jälita hõivamine** eelised on järgmised.

1. *Lihtne konfiguratsiooni ja juurutamine*

    * Diagnostikaandmete saidikogumi konfiguratsiooni on ainult osa rakenduse konfigureerimine. See on lihtne hoida alati "sünkroonis" ülejäänud rakendus.

    * Lihtne vahe konfigureerimise kohta rakenduse või teenuse kohta.

    * Protsessi-, Jälita hõivamine tavaliselt nõuab eraldi juurutamine ja diagnostika agendi, mis on lisatööd haldustoimingut ja tõrgete võimalikke allikas konfiguratsiooni. Kindla agent tehnoloogia võimaldab sageli ainult ühe eksemplari agent virtual masina (sõlme) kohta. See tähendab, et selle konfiguratsiooni saidikogumi diagnostika konfiguratsiooni jagada kõik rakendused ja teenused töötavad sõlme.

2. *Paindlikkus*

    * Rakenduse saate andmeid saata, võimaluse korral vaja minna, kui seal on kliendi teek, mis toetab suunatud andmete säilitamise süsteemi. Saate lisada uue valamud soovitud.

    * Keerukate jäädvustada, filtreerimise ja andmete liitmise reegleid saab rakendada.

    * Protsessi-, Jälita-hõivamine on sageli piiratud andmete valamud, mis toetab. Mõned on laiendatav.

3. *Juurdepääs andmetele sisemine ja kontekst*

    * Diagnostika alasüsteemi töötab rakenduse või teenuse protsessi sees saab kergesti tõsta jälgi kontekstipõhine teabega.

    * Out protsess lähenemisviisi, tuleb saata andmed agent mõned omavahel protsess side süsteem, nt Windowsi sündmuse jälitamise kaudu. See võiks kehtestada täiendavad piirangud.

**Protsessi-, Jälita hõivamine** eelised on järgmised.

1. *Rakenduse jälgimine ja kogumise krahh puistab*

    * Protsessi Jälita hõivamine võib nurjuda, kui rakendus ei käivitu või jookseb. Sõltumatu esindaja on parem võimalus, jäädvustada oluline tõrkeotsinguteavet.<br /><br />

2. *Tähtaeg, töökindluse ja jõudluse tõestatud*

    * Range testimine ja lahing-kõvenemise on agent platvormi müüja (nt Microsoft Azure'i diagnostika agent) välja töötatud.

    * Koos-protsess Jälita hõivamine, tuleb tagada, et tegevuse Diagnostikaandmete taotlemise protsess varjutaks rakenduse peamised ülesanded või tutvustamine ajastamise või jõudlusega probleeme. Sõltumatult töötava agent on väiksem suurem järgmiste probleemide ja piirata selle mõju süsteemi spetsiaalselt.

On võimalik ühendamiseks ja kasu mõlema lähenemisel. Tõepoolest, võib olla parim lahendus rakendustele.

Siin kasutame **Microsoft.Diagnostic.Listeners teek** ja -protsess jälitus saata teenuse struktuuri rakendusest mõnda Elasticsearch arvutikobaras andmete talletamiseks.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Saada Diagnostikaandmete Elasticsearch kuulajatele teegi abil
Microsoft.Diagnostic.Listeners teek on PartyCluster valimi teenuse struktuuri rakenduse osa. Selle kasutamiseks tehke järgmist.

1. GitHub [PartyCluster valimi](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) alla laadida.

2. Rakendus, mis peaks andmeid saata Elasticsearch lahenduse kausta Kopeeri PartyCluster valimi kataloogi Microsoft.Diagnostics.Listeners ja Microsoft.Diagnostics.Listeners.Fabric projekte (kogu kaustad).

3. Avage target lahendus, paremklõpsake Solution Exploreris sõlme lahendus ja valige **Lisamine projekti**. Saate lisada projekti Microsoft.Diagnostics.Listeners lahendus. Korrake sama Microsoft.Diagnostics.Listeners.Fabric projekt.

4. Lisada projekti viite oma teenuse projekti kaks lisatud projektidele. (Iga teenus, mis peaks andmeid saata Elasticsearch peaks viide Microsoft.Diagnostics.EventListeners ja Microsoft.Diagnostics.EventListeners.Fabric).

    ![Projekti viited Microsoft.Diagnostics.EventListeners ja Microsoft.Diagnostics.EventListeners.Fabric teekide][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Teenuse struktuuri üldine kättesaadavus väljaanne ja Microsoft.Diagnostics.Tracing Nugeti pakett
Teenuse struktuuri üldine kättesaadavus Release (2.0.135 välja 31 märts 2016) loodud suunata **.NET Frameworki 4.5.2**. See versioon on kõrgeim Azure'i GA väljalaske ajal toetatud .NET Frameworki versiooni. Kahjuks ei ole selles versioonis raames teatud EventListener API-d, mis tuleb Microsoft.Diagnostics.Listeners teegi. Kuna EventSource (aluseks oleva logimine API-de struktuuri rakenduste osa) ja EventListener kindlalt haagitud, iga projekti, mis kasutab Microsoft.Diagnostics.Listeners teegi peate kasutama EventSource alternatiivne rakendamist. Selle rakendamist pakub **Microsoft.Diagnostics.Tracing Nugeti pakett**Microsoft autoriks. Paketi on kaasatud nii koodi muudatusi peaks olema vajalikud peale viidatud nimeruum muudatuste raames, EventSource täielikult tagasiühilduvad.

Hakata kasutama Microsoft.Diagnostics.Tracing rakendamise EventSource ainekursust, toimige järgmiselt iga teenuse projekti, mis tuleb andmeid saata Elasticsearch

1. Teenuse project paremklõpsake ja klõpsake käsku **Halda Nuget-paketid**.

2. Aktiveerige nuget.org paketi source (kui see pole juba valitud) ja otsige "**Microsoft.Diagnostics.Tracing**".

3. Installimine on `Microsoft.Diagnostics.Tracing.EventSource` pakett (ja sõltuvustega).

4. Teenuse projektis **ServiceEventSource.cs** või **ActorEventSource.cs** faili avada ja asendada selle `using System.Diagnostics.Tracing` direktiiv peal faili selle `using Microsoft.Diagnostics.Tracing` direktiivi.

Neid juhiseid ei ole vaja pärast **.NET Frameworki 4.6** toetab Microsoft Azure'i.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch kuulajale eksemplari loomise ja konfigureerimine
Diagnostikaandmete Elasticsearch jaoks viimane toiming on luua eksemplari `ElasticSearchListener` ja konfigureerige see Elasticsearch ühenduse andmetega. Kuulajale salvestab kaamera kaudu EventSource klassid on määratletud teenuse project esitatud kõik sündmused. See peab olema elus kestel teenuse nii, et see on parim lähtestamine teenuse kood. Kuidas kodakondsuseta teenuse kood lähtestamine võib välja näha pärast vajalikud muudatused (täiendused märkida, alustades kommentaarid `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Elasticsearch ühenduse andmete peaksin eraldi jaotist teenuse konfiguratsioonifailis (**PackageRoot\Config\Settings.xml**). Jaotise nimi peab vastama edasi väärtus on `FabricConfigurationProvider` ehitaja, nt:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Väärtuste `serviceUri`, `userName` ja `password` parameetrid vastavad vastavalt Elasticsearch kobar lõpp-punkti aadress, Elasticsearch kasutajanime ja parooli. `indexNamePrefix`on eesliide Elasticsearch indeksid; teegi Microsoft.Diagnostics.Listeners loob uue registri oma andmete jaoks iga päev.

### <a name="verification"></a>Kinnitamine
See on õige! Nüüd iga kord, kui teenus on käivitada, see algab saata Elasticsearch teenuse konfiguratsioonis määratud jälgi. Saate kontrollida, avades Kibana Kasutajaliidese see seotud target Elasticsearch eksemplari. Selles näites on lehe aadressi http://myBigCluster.westus.cloudapp.azure.com/. Kontrollige, kas valitud nime eesliitega indeksid on `ElasticSearchListener` eksemplari tõepoolest loodud ja andmetega.

![Kus on kuvatud Kibana PartyCluster rakenduse sündmused][2]

## <a name="next-steps"></a>Järgmised sammud
- [Lisateavet diagnoosimise ja teenuse struktuuri teenuse jälgimine](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
