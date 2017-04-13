<properties
   pageTitle="Teenuse struktuuri mudel | Microsoft Azure'i"
   description="Kuidas modelleerimise ja rakendused ja teenused teenuse struktuuri."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Mudel rakenduse teenuse struktuuri

Selles artiklis antakse ülevaade Azure teenuse struktuuri rakenduse mudel. Samuti kirjeldatakse määratlemine rakendus ja manifestifailidele kaudu ja saada taotlus pakkimisel ja juurutamiseks valmis.

## <a name="understand-the-application-model"></a>Rakenduse mudel mõistmine

Rakendus on komponendi teenuseid, mis sooritavad teatud funktsiooni või funktsioonide kogum. Teenuse sooritab lõpule viidud ja autonoomse funktsioon (selle saab käivitada ja muude teenuste sõltumatult) ja koosneb kood, konfigureerimine ja andmed. Iga teenuse jaoks kood koosneb käivitatava kahendfaile, konfiguratsiooni koosneb Teenusesätted, mida saab laadida käitusajal ja andmete koosneb suvalise staatiliste andmete teenuse kaupa. Iga komponendi hierarhilise rakenduse mudelis saab versiooniga ja täiendatud sõltumatult.

![Teenuse struktuuri rakenduse mudel][appmodel-diagram]


Rakenduse tüübi on kategoriseerimine rakenduse ja koosneb kogumi teenuse tüüpi. Teenuse tüüp on kategoriseerimine teenuse. Kategoriseerimine võib olla eri sätteid ja konfiguratsioone, kuid põhifunktsioone jääb samaks. Teenuse esinemisjuhud on muu teenuse konfigureerimine variatsioonid teenuse sama tüüpi.  

Tunnid (või "tüüp") rakenduste ja teenuste kaudu XML-failid (manifestid rakenduste ja teenuste manifestid), mis on mallid, mille saate käivitatud rakendused, poest soovitud klaster pilt on kirjeldatud. Skeemi määratluse faili ServiceManifest.xml ja ApplicationManifest.xml on installitud teenuse struktuuri SDK ja *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*tööriistu.

Erinevate teenuserakenduse eksemplaride kood käivitatakse eraldi protsessid isegi siis, kui korraldaja sama teenuse struktuuri sõlm nimega. Lisaks saate iga rakenduse eksemplari elutsükli (st hallatavate täiendatud) sõltumatult. Järgmine diagramm näitab, kuidas rakenduse tüübid on koostatud tüüpi teenus, mis omakorda koosnevad kood, konfigureerimine ja pakettide. Skeemi lihtsustamiseks ainult kood/config/andmete pakettide `ServiceType4` on näidatud, kuigi kaasaks iga teenuse tüüp, mõned või kõik need paketti tüübid.

![Teenuse struktuuri rakenduse tüüpide ja teenuse][cluster-imagestore-apptypes]

Kaks erinevat manifestifailidele kasutatud kirjeldatakse rakenduste ja teenuste: teenuse manifesti ja Rakendusmanifest. Need asuvad üksikasjalikult järgnev jaotised.

Ei saa olla üks või mitu eksemplari teenuse tüüp klaster aktiivne. Näiteks stateful eksemplare või koopiad, suurendada usaldusväärsus imitatsiooniga olekus vahel asub erinevaid sõlmi klaster koopiad. Kopeerimist sisuliselt koondamise teenuse, et isegi juhul, kui ühe sõlme klaster nurjub. [Sektsioonitud teenuste](service-fabric-concepts-partitioning.md) täpsemaks jagab selle riigi (ja selle Accessi mustrite) klaster sõlmed üle.

Järgmisel joonisel on esitatud rakenduste ja teenuste eksemplari, sektsioonid ja koopiad seos.

![Sektsioonid ja koopiad teenuse raames][cluster-application-instances]


>[AZURE.TIP] Saate vaadata paigutuse rakenduste abil teenuse struktuuri Exploreri tööriista, mis on saadaval veebisaidil http:// klaster&lt;yourclusteraddress&gt;: 19080/Explorer. Täpsema teabe saamiseks vt [visualiseerimine klaster teenuse struktuuri Exploreriga](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>Teenus on kirjeldatud

Teenuse manifest määratleb deklaratiivse teenuse tüüp ja versioon. Seda määrab teenuse metaandmed, näiteks teenuse tüüp, seisund atribuudid, koormust tasakaalustavad mõõdikute, teenuse kahendfaile ja failid.  Teisisõnu, see kirjeldab koodi, konfigureerimine ja andmete pakette, mis toetamiseks ühe või mitme teenuse pakett koostamine. Siin on lihtne näide teenuse manifestis.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Versiooni** atribuudid on struktureerimata stringid ja sõeluda süsteem. Need on kasutada versiooni uuendamine iga komponendi.

**ServiceTypes** teatab, milliseid teenuseid toetavad **CodePackages** selle manifesti. Kui teenus on käivitatud ühte tüüpi teenuse suhtes, on selle manifesti deklareeritud kood pakettide aktiveerida töötab kirje punkte. Tulemuseks protsesside oodatakse käitusajal toetatud failitüübid registreerimiseks. Pange tähele, et teenuse tüübid on deklareeritud manifest ja pole koodi paketi tasandil. Seega kui on mitu koodi pakette, nad on kõik aktiveerida iga kord, kui ühte tüüpi deklareeritud teenuse otsib süsteem.

**SetupEntryPoint** on õigustega sisendpunkti, mis jookseb sama identimisteavet enne muude sisendpunkti nimega teenuse struktuuri (tavaliselt *paremal* konto). Määratud **EntryPoint** käivitatava on tavaliselt pikaajalisi majutusteenus. Eraldi häälestamise kirje punkti olemasolu aitab vältida vajaduseta pikema perioodi kõrge privileegidega host teenuse käivitamine. Määratud **EntryPoint** käivitatava käivitatakse pärast **SetupEntryPoint** väljub edukalt. Tulemiks oleva protsessi jälgitakse ja uuesti (algab uuesti **SetupEntryPoint**), kui see kunagi lõpeb või krahh.

**DataPackage** kinnitab nime atribuudi **nimi** suvalise staatilise andmed olema tarbitud protsessi käitusajal sisaldava kausta.

**ConfigPackage** kinnitab kausta **nime** atribuut, mis sisaldab *Settings.xml* faili nime. See fail sisaldab kasutaja määratletud, võti väärtus sidumiseks sätted, millest protsessi saab lugeda tagasi käitusajal jaotised. Versiooniuuenduse, kui ainult **ConfigPackage** **versioon** on muutunud, siis töötab protsessi ei taaskäivitamist. Selle asemel pakkumist teavitab konfiguratsioonisätted muutunud, nii et nad saavad laaditakse uuesti dünaamiliselt protsess. Siin on näide *Settings.xml* fail:

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Teenuse manifestis võib sisaldada koodi, konfigureerimine ja andmete paketid. Kõik need saab versiooniga sõltumatult.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Rakendus on kirjeldatud


Rakenduse manifesti deklaratiivse kirjeldatakse rakenduse tüüp ja versioon. Seda määrab teenuse koosseis metaandmed, näiteks ühed nimed, eraldamine värviskeemi, eksemplari count/dispersioonanalüüs tegur, Turve ja eraldamise poliitika, paigutuse piiranguid, konfiguratsiooni alistab ja komponendi teenuse tüübid. Rakenduse asetada koormust tasakaalustavad domeenid on ka kirjeldatud.

Seega on Rakendusmanifest kirjeldatakse elementide tasemel ja viitab ühe või mitme teenuse manifestid koostamiseks rakenduse tüübi. Siin on lihtne näide rakenduse manifesti.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Teenuse manifestid, nt **versioon** atribuudid on struktureerimata stringid ja sõeluda süsteem. Need on ka kasutada versiooni uuendamine iga komponendi.

**ServiceManifestImport** sisaldab teenuse manifestid, mida üleminekul kasutatakse seda tüüpi viiteid. Imporditud teenuse manifestid kindlaks teha, millised teenuse on lubatud seda tüüpi sees.

**DefaultServices** kinnitab teenuse eksemplarid, mis luuakse automaatselt iga kord, kui rakendus on käivitatud suhtes selle rakenduse tüüp. Vaikimisi teenused on lihtsalt mugavuse ja käitub tavaline teenuste igas suhtes pärast seda, kui need on loodud. Need on uuendatud koos muude teenustega, rakenduse eksemplari ja saab kasutada ka eemaldada.

> [AZURE.NOTE] Mõni Rakendusmanifest võib sisaldada mitme teenuse manifest impordi ja vaikimisi teenused. Iga manifest importimine võib olla versiooniga sõltumatult.

Saate teada, kuidas säilitada erinevates ja teenuse parameetrite üksikute keskkonna jaoks, lugege teemat [haldamine rakenduse parameetrid mitme keskkonna jaoks](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Paketi rakendus

### <a name="package-layout"></a>Paketi paigutus

Rakenduse manifesti, teenuse manifest(s) ja muud vajalikud paketifailidest tuleb korraldada konkreetse paigutuse juurutamiseks teenuse struktuuri kobar sisse. Näide manifestid selle artikli oleks vaja tuleks korraldada directory järgmine struktuur:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

Kaustadele nimed vastavad iga vastava elemendi **nime** atribuudid. Näiteks kui manifest teenuse kood pakettide **MyCodeA** ja **MyCodeB**nimed, siis kaks kausta sama nimega, sisaldaks vajalikud kahendfaile iga koodi paketi.

### <a name="use-setupentrypoint"></a>Kasutage SetupEntryPoint

Tüüpilised **SetupEntryPoint** kasutamise stsenaariumid on, kui peate käivitama täitmisfaili enne teenuse käivitumist või administraatoriõigustega töötamiseks on vaja. Näiteks:

- Loomine ja käivitamine keskkonna muutujate, mis tuleb teenuse käivitatava. See pole piiratud ainult täitmisfailid kirjutada teenuse struktuuri programmeerimise mudelite kaudu. Näiteks npm.exe peab mõned keskkonna muutujad konfigureeritud node.js rakenduse kasutamise kohta.

- Kuidas häälestada juurdepääsu reguleerimine installides turvalisus serdid.

### <a name="build-a-package-by-using-visual-studio"></a>Visual Studio abil paketi koostamine

Kui Visual Studio 2015 abil saate luua rakenduse, saate käsk paketi loomiseks automaatselt paketi, mis vastab eespool kirjeldatud paigutust.

Paketi loomiseks paremklõpsake Solution Exploreris projekti rakendus ja valige käsk paketi, nagu allpool näidatud:

![Pakendamine rakenduse Visual Studio abil][vs-package-command]

Kui pakendil on lõpule jõudnud, leiate paketi asukoha **väljundi** aknas. Pange tähele, et pakendit etappi toimub automaatselt, kui juurutamine või silumine rakenduse Visual Studio.

### <a name="test-the-package"></a>Paketi testimine

**Test-ServiceFabricApplicationPackage** käsu abil saate kontrollida paketi struktuuri kohalikult PowerShelli kaudu. See käsk on sõelumine probleemide määratlemisel kontrollib ja kogu viiteid. Pange tähele, et see käsk ainult õigsuse strukturaalset kataloogide ja failide pakkimine. See ei kontrollida, mis tahes koodi või andmete paketi sisu Lisaks kontrollimine, et kõik vajalikud failid on olemas.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

See tõrge kuvatakse viidatud teenuse manifestis **SetupEntryPoint** *MySetup.bat* fail puudub pakendist kood. Kui fail puudub on lisatud, edastab taotluse kinnitamine:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Kui rakendus on õigesti pakitud ja edastab kinnitamine, siis on kasutuselevõtuks valmis.

## <a name="next-steps"></a>Järgmised sammud

[Juurutada ja rakenduste eemaldamine][10]

[Rakenduse parameetrid mitme keskkonnas haldamine][11]

[RunAs: Teenuse struktuuri rakendus töötab eri turvalisuse õigustega][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
