<properties
   pageTitle="Määrab teenuse struktuuri teenuse lõpp-punktid | Microsoft Azure'i"
   description="Kuidas kirjeldada teenuse manifestis, sh kuidas luua HTTPS-i lõpp-punktid lõpp-punkti ressursid"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="specify-resources-in-a-service-manifest"></a>Määrake teenuse manifestis ressursid

## <a name="overview"></a>Ülevaade

Teenuse manifest võimaldab ressursse, mida kasutatakse teenuse olema deklareeritud/muuta kompileeritud koodi muutmata. Azure teenuse struktuuri toetab teenuse lõpp-punkti ressursid konfigureerimine. Juurdepääs teenuse manifestis määratud ressursside saab kontrollida SecurityGroup manifestis rakenduse kaudu. Ressursside deklareerimist võimaldab nende ressursside muuta juurutamise ajal, tähendab, et teenus ei vaja tutvustada uue süsteemi konfiguratsioon. Skeemi määratluse faili ServiceManifest.xml on installitud teenuse struktuuri SDK ja *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*tööriistu.

## <a name="endpoints"></a>Lõpp-punktid

Manifestis teenuse lõpp-punkt on ressurss määratlemisel määrab teenuse struktuuri pordid reserveeritud rakenduse port vahemikus, kui pordi pole määratud otseselt. Näiteks pilk lõpp-punkti *ServiceEndpoint1* määratud manifest väljavõte pärast selle lõiguga. Lisaks tellida teenuste ka teatud pordi ressursi. Erinevate kobar sõlmed töötab teenus koopiad saab määrata erinevad pordinumbrid, ajal koopiad sama sõlme töötab teenus ühiskasutus pordi. Teenuse koopiad saate need pordid vastavalt vajadusele kopeerimise ja kuulamine klientide päringutele.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Vaadake [konfigureerimine stateful usaldusväärsed teenused](service-fabric-reliable-services-configuration.md) lugeda Lisateavet viitamine lõpp-punktid config paketi sätted faili (settings.xml).

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Näide: täpsustades oma teenuse lõpp http kaudu

Järgmised teenuse manifest määratleb üks TCP lõpp-punkti ressurss ja kahe HTTP lõpp-punkti ressursside soovitud &lt;ressursid&gt; element.

HTTP lõpp-punktid on automaatselt ACL oleks teenuse struktuuri.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Näide: täpsustades oma teenuse lõpp HTTPS

HTTPS-protokolli pakub autentimist ja kasutatakse ka krüptimiseks kliendi serveri suhtlus. Teenuse struktuuri teenust HTTPS-i lubamiseks määrake protokolli soovitud *ressursid -> lõpp-punktid -> lõpp-punkti* jaotist teenuse manifesti, nagu on näidatud varem lõpp-punkti *ServiceEndpoint3*.

>[AZURE.NOTE] Teenuse Protocol (protokoll) ei saa muuta rakenduse uuendamisel pole see juhis on server muuta.


Siin on näide ApplicationManifest, mille peate määrama https. Teie serdi sõrmejälje tuleb esitada. Funktsiooni EndpointRef on viide EndpointResource sisse ServiceManifest, kus saate määrata HTTPS-protokolli. Saate lisada rohkem kui üks EndpointCertificate.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
