<properties 
pageTitle="Suhtlus pilveteenustega rollide jaoks | Microsoft Azure'i" 
description="Rolli pilveteenuste aknad võib olla lõpp-punktid (http, https, tcp, udp) nende jaoks määratletud mis suhelda välise või muud rolli eksemplari vahel." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Suhtlus rolli eksemplaride Azure lubamine

Siseste ja väliste ühenduste teel suhtlemine pilvepõhise teenuse rollid. Välised ühendused nimetatakse **Sisestuskeel lõpp-punktid** sisemised ühendused nimetatakse **sisemise lõpp-punktid**. Selles teemas kirjeldatakse, kuidas muuta [teenuse määratlus](cloud-services-model-and-package.md#csdef) loomiseks lõpp-punktid.


## <a name="input-endpoint"></a>Sisestuskeel lõpp-punkti
Sisestuskeel lõpp-punkti kasutatakse, kui soovite seada pordi väljapoole. Saate määrata protokolli tüüp ja pordi lõpp-punkti, mis rakendavad mõlema funktsiooni sise- ja portide lõpp-punkti. Kui soovite, saate teise sisemise porti lõpp-punkti [localPort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) atribuuti.

Sisestuskeel lõpp-punkti, saate kasutada järgmisi protokolle: **http, https, tcp, udp**.

Sisestuskeel lõpp-punkti loomiseks lisada **InputEndpoint** tütarelement veebi- või töötaja rolli element **lõpp-punktid** .

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Eksemplari Sisestuskeel lõpp-punkti
Lõpp-punktid, kuid võimaldab teil sisestada sarnanevad eksemplari Sisestuskeel lõpp-punktid vastendamine teatud avalikkusele suunatud pordid iga üksiku rolli eksemplari laadi koormusetasakaalustusteenuse pordi suunamise abil. Saate määrata ühe avalikkusele suunatud pordi või mitut porti.

Eksemplari Sisestuskeel lõpp-punkti saate kasutada ainult **tcp** või **udp** protokoll.

Eksemplari Sisestuskeel lõpp-punkti loomiseks lisada **InstanceInputEndpoint** tütarelement veebi- või töötaja rolli element **lõpp-punktid** .

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Sisemise lõpp-punkti
Sisemise lõpp-punktid on saadaval eksemplari-eksemplari suhtlemiseks. Pordi pole kohustuslik ja kui välja jäetud, dünaamiline port on määratud lõpp-punkti. Port vahemikus saab kasutada. On kuni viis sisemise lõpp-punktid iga rolli kohta.

Sisemise lõpp-punkti, saate kasutada järgmisi protokolle: **http, tcp, udp, mis tahes**.

Sisemise Sisestuskeel lõpp-punkti loomiseks lisada **InternalEndpoint** tütarelement veebi- või töötaja rolli element **lõpp-punktid** .

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Samuti saate port vahemikus.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Töötaja rolli vs Web rollid

Töötaja ja web rollid töötamisel on üks väike erinevus koos lõpp-punktid. Web roll peab olema vähemalt ühe Sisestuskeel endpoint, kasutades **HTTP** -protokolli.


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Lõpp-punkti juurdepääs .NET SDK abil
Azure'i hallatavate teegi pakub meetodit rolli aknad käitusajal suhelda. Koodi, mis töötab sees rolli eksemplari, saate otsida teavet teiste rolli aknad ja nende lõpp-punktid, samuti teavet rolli praeguse eksemplari.

> [AZURE.NOTE] Saate tuua ainult rolli aknad, mis töötavad teie pilveteenuses ja mis määratlevad vähemalt üks sisemine lõpp-punkti teave. Ei saa hankida rolli muus teenuses töötavate eksemplaride andmeid.

Saate tuua eksemplari rolli atribuudi [eksemplarid](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) . Esmalt [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) abil tagasi praeguse eksemplari rolli viide ja tagastab viite roll ise [rolli](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) atribuudi abil.

Kui loote ühenduse rolli eksemplari programmiliselt kaudu .NET SDK, on üsna lihtne juurdepääs lõpp-punkti teave. Pärast teatud rolli keskkonnas juba ühendatud, saate oma kindla lõpp-punkti selle tähisega pordi.

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Atribuudi **juhtudel** tagastab **RoleInstance** objektide kogum. Selle saidikogumi alati sisaldab praeguse eksemplari. Kui roll Määratle sisemise lõpp, kogu sisaldab praeguse eksemplari, kuid muud eksemplarid. Arvu rolli aknad kogumist alati on 1 juhul, kui puudub sisemine lõpp-punkti on rollile määratud. Kui roll määratleb sisemine lõpp, selle eksemplarid on leitav käitusajal ja kogumine eksemplaride arv vastab määratud roll teenuse konfiguratsioonifail eksemplaride arv.

> [AZURE.NOTE] Azure'i hallatavate teegi ei paku seisundi muud rolli aknad, kindlaks teha, tähendab see, kuid kui teenust peab see funktsioon saab seisundit hinnangud ise rakendada. [Azure'i diagnostika](cloud-services-dotnet-diagnostics.md) abil saate teavet töötavad rolli eksemplarid.

Sisemise lõpp-punkti rolli eksemplari pordinumber määratlemiseks saate atribuudi [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) sõnastiku objekti, mis sisaldab lõpp-punkti nimed ja nende vastavate IP-aadresside ja portide tagastamiseks. Atribuudi [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) tagastab IP-aadress ja pordi määratud lõpp-punkti. Atribuudi **PublicIPEndpoint** annab port on koormus tasakaalustatud lõpp-punkti. IP address osa atribuudi **PublicIPEndpoint** ei kasutata.

Siin on näide, mis itereerib rolli aknad.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Siin on näide saab lõpp-punkti esitatud teenuse määratlemise kaudu ja käivitab listening ühenduste töötaja roll.

> [AZURE.WARNING] Järgmine kood toimib ainult juurutatud teenuse. Azure'i arvutada emulaator käivitamisel ignoreeritakse teenuse konfigureerimine elemente, mis loomine (**InstanceInputEndpoint** elemendid) otsene port lõpp-punktid.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Võrgu liikluse reeglite määrata rolli suhtlus
Pärast ülesande määratlemist sisemise lõpp-punktid, saate kontrollida, kuidas rolli aknad saate omavahel suhelda võrgu liikluse reegleid (põhjal loodud lõpp-punktid) lisada. Järgmisel joonisel on esitatud kontrollimise teatise rolli mõne levinud stsenaariumi:

![Võrgu liikluse reeglite stsenaariumid] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Võrgu liikluse reeglite stsenaariumid")

Koodi järgmises näites on kujutatud rolli määratlused eelmisel joonisel rollid. Iga rolli määratluse sisaldab vähemalt ühte sisemise lõpp-punkti määratletud.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Suhtlemine rollide piiramine võib ilmneda nii fikseeritud ja automaatselt määratud pordid sisemise lõpp-punktid.

Vaikimisi pärast sisemise lõpp on määratletud, side saate flow mis tahes sisemise lõpp-punkti rolli ilma piiranguteta. Piira suhtlus, peate lisama **ServiceDefinition** elemendi teenuse määratluse faili **NetworkTrafficRules** element.

### <a name="scenario-1"></a>Stsenaarium 1
Ainult võimaldavad võrguliikluse kaudu **WebRole1** **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Stsenaarium 2
Ainult võimaldab võrguliikluse **WebRole1** **WorkerRole1** ja **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Stsenaarium 3
Ainult võimaldab võrguliikluse **WebRole1** **WorkerRole1**ja **WorkerRole1** **WorkerRole2**abil.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Stsenaarium 4
Ainult võimaldab võrguliikluse **WebRole1** **WorkerRole1**, et **WorkerRole2** **WebRole1** ja **WorkerRole1** **WorkerRole2**abil.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

XML schema lähtepunktina kohal kasutatud elemente leiate [siit](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet pilveteenuses [mudel](cloud-services-model-and-package.md).