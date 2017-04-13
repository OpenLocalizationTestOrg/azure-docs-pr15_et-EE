<properties
   pageTitle="Juurutamine Node.js rakendus, mis kasutab MongoDB | Microsoft Azure'i"
   description="Kiirtutvustus kohta, kuidas mitme Külastajate täitmisfailid paketti juurutada on Azure teenuse struktuuri kobar"
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
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Mitme Külastajate täitmisfailid juurutamine

Selles artiklis kirjeldatakse, kuidas paketti ja juurutada mitme Külastajate täitmisfailid Azure teenuse struktuuri. Lugege koostamise ja juurutamine ühe teenuse struktuuri paketi juurutamise [külalisena käivitatava teenuse struktuuri](service-fabric-deploy-existing-app.md).

Ajal juhendis kuvatakse juurutamise rakendus, mis kasutab MongoDB andmesalve Node.js vastendamisel, saate rakendada rakendusele, on sõltuvusi mõnda muusse rakendusse juhiseid.   

Visual Studio saate andes rakendusepaketi, mis sisaldab mitut Külastajate executables. Lugege teemat [Abil Visual Studio paketti taotluse](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Pärast esimese külalisena käivitatava lisamist, paremklõpsake rakenduse project ja valige **Lisa -> uus teenuse struktuuri teenuse** lisada teise Külastajate käivitatava projekti lahendus. Märkus: Kui valite link allika Visual Studio Projectis, koostamise lahenduse Visual Studios teeb veenduge, et teie rakendusepaketi allikas muudatustega kursis. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Käsitsi paketti mitme Külastajate käivitatava teenuserakenduse
Teise võimalusena saate käsitsi paketti käivitatava külalisena. Selles artiklis kasutab käsitsi pakendit teenuse struktuuri pakendit tööriista, mis on saadaval veebisaidil [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Pakendamine Node.js rakendus
Selles artiklis eeldatakse, et teenuse struktuuri klaster sõlmed Node.js pole installitud. Seetõttu peate Node.exe lisamiseks juurkaust rakenduse sõlm enne pakendit. Kataloogi struktuuri Node.js taotluse (kasutades Express web framework ja Jade malli mootor) peaks sarnanema ühes allpool.

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Järgmise sammuna loote rakendusepaketi Node.js rakenduse. Alljärgnev kood loob teenuse struktuuri rakendusepaketi, mis sisaldab Node.js rakendus.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Allpool on parameetrid, mida kasutatakse kirjeldus:

- rakendus, mis peaks olema pakitud kataloogi suunab **statistikast/allikas** .
- **Target** määratleb, kus luuakse paketi kataloogi. See kaust peab olema erinevate andmeallikate kataloogist.
- **/appname** määratleb olemasoleva rakenduse rakenduse nimi. See on oluline mõista, et see tõlgib manifestis teenuse nimi ja pole teenuse struktuuri rakenduse nimi.
- **/exe** määratleb käivitatava, mis peaks teenuse struktuuri käivitada sel juhul `node.exe`.
- **/ma** määratleb argument, mida kasutatakse käivitamine käivitatava. Nagu Node.js pole installitud, täites Node.js veebiserver käivitamiseks peab teenuse struktuuri `node.exe bin/www`.  `/ma:'bin/www'`ütleb pakendit tööriista kasutada `bin/ma` jaoks node.exe argumendina.
- **/AppType** määratleb teenuse struktuuri rakenduse tippige nimi.

Kui parameeter Target määratud kataloogi sirvida, näete tööriista loonud täielikult töötavaks teenuse struktuuri paketi, nagu allpool näidatud:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Nüüd on loodud ServiceManifest.xml jaotis, mis kirjeldab, kuidas tuleks Node.js veebiserver käivitada, nagu on näidatud allpool koodilõigu.

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
Selles näites Node.js veebiserver kuulata pordi 3000, seega peate värskendama faili ServiceManifest.xml nagu allpool näidatud lõpp-punkti teave.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Pakendamine MongoDB rakendus
Nüüd, kui pakitud Node.js rakendus, saate jätkata ja paketti MongoDB. Nagu enne mainitud, pole teatud Node.js ja MongoDB toiminguid, mis valib kaudu kohe. Tegelikult juhised kehtivad kõik rakendused, mis on mõeldud ühe teenuse struktuuri taotluse kokku pakitud.  

Paketi MongoDB, mida soovite veenduge, et saate paketti Mongod.exe ja Mongo.exe. Nii kahendfaile asuvad selle `bin` kataloog MongoDB installi kataloogis. Kataloogi struktuuri näeb välja umbes allpool.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Teenuse struktuuri peab algama MongoDB käsu sarnaneb allpool, seega peate kasutama funktsiooni `/ma` parameeter kui pakendamine MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Andmed ei ei säilitata sõlm tõrke korral kui MongoDB andmekataloogi sellele kohaliku kataloogi sõlme. Tuleks kasutada püsival salvestusruumi või rakendada andmete kaotsimineku vältimiseks määrake MongoDB koopia.  

PowerShelli või käsu shell, võtame pakendit tööriista järgmisi:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

MongoDB lisamiseks oma teenuse struktuuri rakendusepaketi, peate veenduge, et Target parameeter punktide samasse kausta, mis juba sisaldab rakenduse näidata koos Node.js rakendus. Samuti peate veenduge, et kasutate ApplicationType sama nimi.

Loome kataloogi sirvida ja uurida, mis on loodud tööriist.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Nagu näete, tööriist lisanud uue kausta, MongoDB, kausta, mis sisaldab MongoDB kahendfaile. Kui avate selle `ApplicationManifest.xml` fail, näete, et pakett sisaldab nüüd Node.js rakendamist ja nende MongoDB. Alljärgnev kood kuvatakse rakenduse manifesti sisu.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Rakenduse avaldamine
Viimases etapis on avaldada rakenduse kohaliku teenuse struktuuri klaster allpool PowerShelli skriptide abil.

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Kui rakendus on edukalt avaldatud kohaliku klaster, pääsete Node.js rakendus Meil on sisestatud teenuse manifestis Node.js taotluse – näiteks http://localhost:3000 Port.

Selles õpetuses on näha, kuidas hõlpsalt paketi kahe olemasoleva rakenduse ühe teenuse struktuuri taotluse. Saate ka õppinud juurutamise teenuse struktuuri, et see osa teenuse struktuuri funktsioonid, näiteks kõrge-saadavus ja seisundi süsteemi integreerimise kasu.

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [teenuse struktuuri ja ümbriste](service-fabric-containers-overview.md) ülevaade ümbriste juurutamine
