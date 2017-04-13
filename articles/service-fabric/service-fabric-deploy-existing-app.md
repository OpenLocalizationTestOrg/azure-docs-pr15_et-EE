<properties
   pageTitle="Olemasoleva täitmisfaili juurutada Azure teenuse struktuuri | Microsoft Azure'i"
   description="Kuidas paketti taotluse külalisena käivitatava, et seda saab kasutada teenuse struktuuri arvutikobaras kiirtutvustus"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>Executable külaline juurutama teenuse struktuuri

Saate kasutada mis tahes tüüpi rakendus, näiteks node.js, Java või algsete rakenduste Azure teenuse struktuuri. Teenuse struktuuri viitab järgmist tüüpi rakendusi Külastajate executables.
Külalisena täitmisfailid käsitletakse kodakondsuseta teenused, teenuse struktuuri. Selle tulemusena paigutatakse need sõlmed klaster, kättesaadavus ja muude mõõdikute põhjal. Selles artiklis kirjeldatakse, kuidas paketti ja juurutada külalisena käivitatava teenuse struktuuri kobar, kasutades Visual Studio või mõne käsurea kasuliku.

Selles artiklis käsitleme külalisena käivitatava paketti ja selle juurutama teenuse struktuuri juhiseid.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Külalisena käivitatava teenuse struktuuri töötamise eelised

On kaasas töötab külalisena käivitatava teenuse struktuuri klaster annab mitmeid eeliseid.

- Kõrge-saadavus. Rakendusi, mis läbiviimisel teenuse struktuuri on väga kättesaadavaks. Teenuse struktuuri tagab, et töötab rakenduse eksemplari.
- Seisundi jälgimine. Teenuse struktuuri seisundi jälgimine tuvastab, kui rakendus töötab ja diagnostika teave, kui ilmneb tõrge.   
- Rakenduse elutsükli haldus. Lisaks pakkuda pole tööseisakute täienduste, teenuse struktuuri pakub automaatse eelmise versiooni tagasipööramine kui seal on versiooniuuenduse teatatud halb seisund sündmus.    
- Tihedusfunktsiooni. Mitme rakendusi käivitada klaster, mis välistab konkreetse rakenduse käivitamiseks oma riistvara.


## <a name="overview-of-application-and-service-manifest-files"></a>Rakenduste ja teenuste manifestifailidele ülevaade

Osana juurutamine külalisena käivitatava on kasulik mõista teenuse struktuuri pakendit ja juurutamise mudeli kirjeldatud [mudel](service-fabric-application-model.md). Teenuse struktuuri pakendit mudel tugineb kaks XML-failid: rakenduste ja teenuste manifestid. Skeemi definitsiooni ApplicationManifest.xml ja ServiceManifest.xml failid on installitud teenuse struktuuri SDK *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*sisse.

* **Rakenduse näidata** Rakenduse manifesti kasutatakse rakenduse kirjeldamiseks. See on loetletud teenused, mida üleminekul selle ja muude parameetrite abil saate määratleda, kuidas ühe või mitme teenuseid tuleb kasutada, nagu näiteks eksemplaride arv.

  Rakendus on teenuse struktuuri üksus juurutus- ja täiendamine. Rakenduse versiooni täiendamist kus võimalike vigade ja võimalike tagasipööramine hallatakse ühe üksusena. Teenuse struktuuri tagab, et versiooniuuendus ei õnnestunud, või, kui uuele versioonile nurjub, ei jäta rakendus pole teada/ebastabiilses olekus.

* **Teenuse näidata** Teenuse manifest kirjeldatakse teenuse komponendid. See sisaldab andmeid, nt nime ja tüüpi teenus, ja selle koodi, konfigureerimine ja andmed. Teenuse manifest sisaldab ka mõned täiendavad parameetrid, mida saab konfigureerida teenuse, kui see on juurutatud.


## <a name="application-package-file-structure"></a>Rakenduse paketi faili struktuur
Teenuse struktuuri rakenduse juurutamiseks peab taotluse eelmääratletud kataloogi struktuuri. Järgmine näide selle struktuuri.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

Funktsiooni ApplicationPackageRoot sisaldab ApplicationManifest.xml faili, mis määratleb. Alamkausta, iga teenuse esitada kasutatakse sisaldavad kõik esemeid teenus nõuab--soovitud ServiceManifest.xml ja tavaliselt järgmised kolm kataloogi:

- *Kood*. See kaust sisaldab teenuse kood.
- *Config*. See kaust sisaldab Settings.xml faili (ja muid faile vajaduse korral) käitusajal tuua teatud konfiguratsioonisätted, et teenus on juurdepääs.
- *Andmed*. See on täiendavad kataloog täiendavad kohalikud andmed, mida võib vaja teenuse talletamiseks. Märkus: Andmete tuleks kasutada ainult efemeersed andmete talletamiseks. Teenuse struktuuri ei kopeeri/juhendist muudatuste andmete kataloogi kui teenus on vaja paigutada – näiteks Tõrkesiirde ajal.

Märkus: Teil pole vaja luua selle `config` ja `data` kataloogide, kui neid ei vaja.

## <a name="packaging-an-existing-executable"></a>Olemasoleva täitmisfaili pakendamine

Kui külaline käivitatava pakendamine, saate valida, kas kasutada Visual Studio projekti malli või [luua rakenduse pakett käsitsi](#manually). Visual Studio kasutamisel rakenduse paketi struktuuri ja manifestifailidele on loodud uue projekti viisard.

>[AZURE.NOTE] Lihtsaim viis paketti käivitatava võtta mõne olemasoleva Windows on kasutada Visual Studio.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Olemasoleva täitmisfaili paketti Visual Studio abil

Visual Studio pakub teenuse struktuuri teenuse malli teenuse struktuuri kobar külalisena käivitatava juurutamiseks.

Läbida avaldamisviisardist lõpetamiseks tehke järgmist:

1. Valige Fail -> uus projekt ja struktuuri teenuserakenduse loomine.
2. Valige Külaliselingi käivitatava teenuse mallina.
3. Klõpsake nuppu Sirvi, valige oma käivitatava kaust ja täitke ülejäänud loomine teenuse parameetrid.
    - *Koodi paketi käitumine* saate määrata kopeerida kogu kausta sisu Visual Studio projekt, mis on kasulik, kui käivitatava ei muutu. Kui muudate ega soovi võimalus uute järgud dünaamiliselt kättesaamine käivitatava, saate selle asemel kausta link. Pange tähele, et saate lingitud kaustade Visual Studio projekti rakenduse loomisel. See sisaldab linke lähtekoht Projecti, võimaldades värskendamist allika sihtkohta käivitatava külaline, on need värskendused osaks rakenduse paketi koostamine.
    - *Programmi* - valige, mida tuleks teha teenuse käivitamine.
    - *Argumendid* - argumendid, mis tuleks edastada käivitatava määrata. See võib olla argumendiga parameetrite loendi.
    - *WorkingFolder* - määrab protsessi, mida saab alustada tööd kausta. Saate määrata kolm väärtused:
        - `CodeBase`Saate määrata, et töötavat kausta saab määrata kood kataloogi rakenduse pakett (`Code` faili struktuuri näidatud enne kataloogi).
        - `CodePackage`Saate määrata, et töötavat kausta saab määrata juurkausta rakendusepaketi (`GuestService1Pkg` faili struktuuri eelneva näidatud).
        - `Work`Saate määrata, et faile paigutatakse mõnda alamkausta töö
4. Tippige oma teenuse nimi ja klõpsake nuppu OK.
5. Kui teie teenuse peab lõpp suhtlemiseks, saate nüüd lisada protokolli, portide ja tippige ServiceManifest.xml faili. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Nüüd saate kasutada ja avaldada toimingu suhtes kohaliku klaster silumine lahenduse Visual Studios. Kui olete valmis saate avaldada rakenduse remote kobar või sisse andmeallika juhtelemendi lahendus.
7. Sellest artiklist teada, kuidas saate vaadata Külastajate käivitatava teenus töötab teenus struktuuri Exploreris lõppu liikumine

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Käsitsi pakendamine ja olemasoleva täitmisfaili juurutamine
Protsessi käsitsi pakendamine Külastajate käivitatava põhineb järgmist:

1. Looge paketi kataloogi struktuuri.
2. Rakenduse koodi ja konfiguratsiooni faile lisada.
3. Redigeeri teenuse Avaldamisfail.
4. Rakenduse Avaldamisfail redigeerida.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Kataloogi struktuuri paketi loomine
Kataloogi struktuuri, luues saate alustada eespool kirjeldatud.

### <a name="add-the-applications-code-and-configuration-files"></a>Rakenduse konfiguratsioon kood ja failide lisamine
Kui olete loonud kataloogi struktuuri, saate lisada rakenduse koodi ja koodi ja config kataloogide jaotises failid. Saate luua ka täiendavad kataloogide või alamkatalooge koodi või config kataloogide alusel.

Teenuse struktuuri ei mõne xcopy sisu rakenduse juurkaust, seega pole eelmääratletud struktuuri kasutada muud peale loomine kahe ülemist kataloogi, koodi ja sätted. (Võite valida erinevaid nimesid, kui soovite. Lisateave on järgmises jaotises.)

>[AZURE.NOTE] Veenduge, et kõik failid/sõltuvused, mis rakenduse tuleb lisada. Teenuse struktuuri kopeerib sisu kõik sõlmed rakendusepaketi kobar, kus on rakendusteenuste kavatsete kasutusele võtta. Paketi peaks sisaldama koodi, mida rakenduse käivitamiseks. Me ei soovita eeldades, et sõltuvused on juba installitud.

### <a name="edit-the-service-manifest-file"></a>Teenuse Avaldamisfail redigeerimine
Järgmiseks tuleb redigeerimiseks teenuse Avaldamisfail lisada järgmine teave:

- Teenuse tüüp nimi. See on ID, mis teenuse struktuuri kasutab teenuse tuvastamiseks.
- Käsk Käivita rakendus (ExeHost) abil.
- Mis tahes skripti, mis tuleb käivitada seada üles/konfigureerida rakendus (SetupEntrypoint).

Järgmine on näide on `ServiceManifest.xml` faili:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Esmalt avage fail, mille peate värskendama erinevate osade üle.

#### <a name="updating-the-servicetypes"></a>Funktsiooni ServiceTypes värskendamine

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- Saate valida mis tahes nime, mida soovite `ServiceTypeName`. Väärtus on kasutatud funktsiooni `ApplicationManifest.xml` faili teenuse tuvastamiseks.
- Peate määrama `UseImplicitHost="true"`. Atribuudi ütleb teenuse struktuuri, et teenus põhineb iseseisev rakendus, nii, et kõik teenuse struktuuri peab tegema on käivitage see protsess ja selle seisundi jälgimine.

#### <a name="updating-the-codepackage"></a>Funktsiooni CodePackage värskendamine
CodePackage elemendi määrab selle teenuse kood asukoht (ja versioon).

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

Funktsiooni `Name` element on kasutatud määramiseks nimi kataloogi rakendusepaketi, mis sisaldab teenuse kood. `CodePackage`on ka selle `version` atribuut. See saab määrata koodi--versiooni ja seda saab kasutada ka potentsiaalselt versioonile teenuse kood teenuse struktuuri rakenduse elutsükli haldus taristu abil.
#### <a name="optional-updating-the-setupentrypoint"></a>Valikuline: Selle SetupEntrypoint värskendamine

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
SetupEntrypoint elemendi abil saate määrata käivitatava või paketi faili, mis peaks toimuma enne selle teenuse kood on käivitatud. See on valikuline samm, nii, et see ei pruugi kaasata, kui seal on lähtestamine/installimine pole nõutav. Funktsiooni SetupEntryPoint käivitatakse iga kord, kui teenuse taaskäivitamist.

On ainult üks SetupEntrypoint, setup/config skriptide tuleb ühe pakkfail kui rakenduse setup/config nõuab mitme skriptide rühmitada. Funktsiooni SetupEntrypoint käivitamiseks mis tahes tüüpi fail--täitmisfaili faile, paketi failide ja PowerShelli cmdlet-käsud. Lisateavet leiate teemast [SetupEntryPoint konfigureerimine](service-fabric-application-runas-security.md).

Eelnev näide on SetupEntrypoint töötab paketi fail nimega `LaunchConfig.cmd` mis asub selle `scripts` alamkausta kood kataloogi (eeldades, et WorkingFolder element on seatud CodeBase).

#### <a name="updating-the-entrypoint"></a>Entrypoint värskendamine

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

Funktsiooni `Entrypoint` element Avaldamisfail teenuse abil saate määrata teenuse käivitamiseks tehke järgmist. Funktsiooni `ExeHost` element, mis määrab käivitatava (ja argumendid), mida tuleks kasutada teenuse käivitamine.

- `Program`määrab nime, mis peaks toimuma teenuse käivitamine käivitatava.
- `Arguments`Saate määrata argumendid, mis tuleks edastada käivitatava. See võib olla argumendiga parameetrite loendi.
- `WorkingFolder`määrab protsessi, mida saab alustada tööd kausta. Saate määrata kolm väärtused:
    - `CodeBase`Saate määrata, et töötavat kausta saab määrata kood kataloogi rakenduse pakett (`Code` kataloog eelmise faili struktuuri).
    - `CodePackage`määrab töötavat kausta saab määrata root rakendusepaketi (`GuestService1Pkg` eelmise faili struktuuri).
  - `Work`Saate määrata, et faile paigutatakse mõnda alamkausta töö

Funktsiooni WorkingFolder on kasulik määrata õige töötamise kataloogi nii, et suhtelised teed saavad kasutada rakenduse või lähtestamine skriptide.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>Lõpp-punktid värskendamine ja registreerumist nimeteenus suhtlemiseks

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Eelmises näites kuvatakse `Endpoint` element määrab lõpp-punktid, mis rakenduse saate kuulata. Selles näites Node.js rakenduse kuulab http Port 3000.

Lisaks saate esitada teenuse struktuuri avaldamiseks selle lõpp-punkti nimetamine teenusega muude teenuste leiaksid selle teenuse lõpp-punkti aadress. See võimaldab teil suhelda, mis teenused on Külastajate täitmisfailid vahel.
Avaldatud lõpp-punkti aadress on kujul `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`ja `PathSuffix` on valikulised atribuudid. `IPAddressOrFQDN`on sordib või täielik domeeninimi sõlme seda täitmisfaili saab asetada ja arvutatakse teie eest.

Järgmises näites kui teenus on juurutatud, teenuse struktuuri Exploreris kuvatakse sarnaselt lõpp `http://10.1.4.92:3000/myapp/` avaldatud eksemplari jaoks või kui see on kohalikus arvutis, kuvatakse `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Saate need aadressid [tagant puhverserveri](service-fabric-reverseproxy.md) suhelda teenuste vahel.

### <a name="edit-the-application-manifest-file"></a>Rakenduse Avaldamisfail redigeerimine

Kui olete konfigureerinud selle `Servicemanifest.xml` fail, peate tegema mõned muudatused on `ApplicationManifest.xml` faili, et tagada õige teenuse tüüp ja nimi.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

Klõpsake soovitud `ServiceManifestImport` element, saate määrata ühe või mitme teenuseid, mida soovite kaasata rakendus. Teenuste viidatud koos `ServiceManifestName`, mis määrab nime kataloogi kui selle `ServiceManifest.xml` fail asub.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Logimise häälestamine
Külalisena täitmisfailid, on kasulik näha konsooli logid, et teada saada, kui rakenduse ja konfiguratsiooni skriptide Kuva vigu.
Konsooli ümbersuunamise saab seadistada soovitud `ServiceManifest.xml` faili, kasutades funktsiooni `ConsoleRedirection` elemendi.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`saab kasutada nii, et neid saab kasutada kinnitamiseks installimise või teenuse struktuuri kobar rakenduse täitmise ajal pole vigu töötavat kausta konsooli väljundi (stdout ja stderr) ümbersuunamiseks.

    * `FileRetentionCount`määrab, mitu failid on salvestatud töötavat kausta. Väärtus 5, näiteks tähendab, et et eelmise viis täitmised logifailide on talletatud töötavat kausta.
    * `FileMaxSizeInKb`Saate määrata logifailide maksimaalne maht.

Logifailide salvestatakse teenuse töötamise kataloogide. Failide asukohaks määramiseks peate teenuse struktuuri Exploreri abil saate määratleda, mis sõlm, et teenus töötab ja töötamise kataloogis kasutatakse. Selles artiklis käsitletakse seda toimingut.

## <a name="deployment"></a>Juurutamine
Viimane toiming on rakenduse juurutamine. Järgmist PowerShelli skripti näitab, kuidas juurutada rakendust kohaliku arengu klaster ja uue teenuse struktuuri teenuse käivitamine.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
Teenuse struktuuri teenuse saab kasutada erinevaid "konfiguratsioone." Näiteks seda saab kasutada ühe või mitme eksemplari nimega või seda saab kasutada nii, et on üks teenuse iga sõlme teenuse struktuuri kobar eksemplar.

Funktsiooni `InstanceCount` parameeter on `New-ServiceFabricService` cmdlet-käsu abil saate määrata, mitu eksemplari teenuse tuleks käivitada teenuse struktuuri klaster. Saate määrata selle `InstanceCount` väärtus, mis juurutate sõltuvalt. On kaks kõige levinumad stsenaariumid:

* `InstanceCount = "1"`. Sel juhul on ainult üks näiteks teenuse juurutatud klaster. Teenuse struktuuri ajasti määratleb mis sõlm teenuse plaanitakse juurutada.

* `InstanceCount ="-1"`. Sel juhul ühe eksemplari teenus on juurutatud iga sõlme klaster teenuse struktuuri. Tulem on võttes ühe (ja ainult üks) eksemplari teenuse iga sõlme klaster.

See on kasulik konfiguratsioon ees rakenduste (nt ülejäänud näitaja), sest klientrakendustes peate ühenduse "" mis tahes sõlmed klaster kasutada lõpp-punkti. Selle konfiguratsiooni saab kasutada ka siis, kui näiteks kõik sõlmed teenuse struktuuri kobar on ühendatud nii, et kliendi liikluse saate levitatakse kogu teenus, mis töötab kõik sõlmed klaster laadi koormusetasakaalustusteenuse.

## <a name="check-your-running-application"></a>Kontrollige oma töötava rakenduse

Teenuse struktuuri Exploreris tuvastada sõlm, kus töötab teenus. Selles näites see töötab Node1:

![Sõlm, kus töötab teenus](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Kui liikuda sõlme ja otsige sirvides üles rakendus, näete olulised sõlm andmed, sealhulgas selle asukohta ketas.

![Asukoht kettal](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Kui sirvida kataloogi Server Exploreri kaudu, leiate töötavat kausta ja teenuse Logi kausta nagu järgmisel pildil näha.

![Logi asukoht](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Järgmised sammud
Selles artiklis on õppinud, kuidas külaline käivitatava paketti ja selle juurutama teenuse struktuuri. Järgmise sammuna saate vaadata selle teema lisasisu.

- [Valimi pakendit ja juurutamine külalisena käivitatava github](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), sh tööriista pakendit väljalaske-eelne link
- [Mitme Külastajate täitmisfailid juurutamine](service-fabric-deploy-multiple-apps.md)
- [Luua esimese teenuse struktuuri rakenduse Visual Studio abil](service-fabric-create-your-first-application-in-visual-studio.md)
