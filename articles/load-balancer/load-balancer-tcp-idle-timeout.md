<properties
   pageTitle="Laadi koormusetasakaalustusteenuse TCP jõudeaja ajalõpu konfigureerimine | Microsoft Azure'i"
   description="Laadi koormusetasakaalustusteenuse TCP jõudeaja ajalõpu konfigureerimine"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>TCP jõudeaja ajalõpu sätete Azure'i laadi koormusetasakaalustusteenuse konfigureerimine

Oma vaikimisi konfiguratsiooni Azure'i laadi koormusetasakaalustusteenuse on jõudeaja ajalõpu 4 minutit. Kui tegevusetuse perioodi on pikem kui ajalõpu väärtust, ei ole kindel, et kliendi ja teie pilveteenuses vahel TCP või HTTP seansi ei säilitata.

Kui ühendus on suletud, klientrakenduse võidakse kuvada järgmine tõrketeade: "aluseks oleva ühenduse suleti: ellu eeldatud ühenduse suleti server."

Levinud on kasutada TCP ülalhoidmise. See tava hoiab ühenduse aktiivse pikemaks ajaks. Lisateabe saamiseks vt järgmisi [.net-i näited](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Ülalhoidmise lubatud, paketid saadetakse ühenduse tegevusetuse perioodi. Need ülalhoidmise paketid tagavad jõude ajalõpu väärtust pole kunagi jõudnud ja ühenduse säilib kaua aega.

See säte töötab ainult sissetulevaid ühendusi. Ühenduse kaotamise vältimiseks peate konfigureerimine TCP ülalhoidmise intervalliga vähem kui jõudeaja ajalõpu sätte või jõudeaja ajalõpu väärtust. Sellised stsenaariumid toetamiseks oleme lisanud konfigureeritav jõudeaja ajalõpu tugi. Nüüd saate häälestada selle kestuse 4 kuni 30 minutit.

TCP ülalhoidmise sobib hästi stsenaariumid, kus aku on piirang. Soovitatav on mitte mobiilirakenduste. TCP ülalhoidmise mobiilsideseadmete rakenduse abil saate seadmes asuvat kiiremini tühjendada.

![TCP ajalõpp](./media/load-balancer-tcp-idle-timeout/image1.png)

Järgmistes jaotistes kirjeldatakse, kuidas virtuaalmasinates jõudeaja ajalõpu sätteid muuta ja pilveteenused.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>TCP ajalõpp konfigureerimine eksemplari taseme avaliku IP 15 minutit

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`pole kohustuslik. Kui see on seatud, on vaikimisi ajalõpp 4 minutit. Aktsepteeritav ajalõpp vahemik on 4 kuni 30 minutit.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Määrake jõudeaja ajalõpu lõpp Azure virtuaalse masina loomisel

Lõpp-punkti ajalõpp sätet saate muuta järgmist:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Teie jõudeaja ajalõpu konfiguratsiooni toomiseks kasutada järgmine käsk:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>TCP ajalõpp pannud koormusetasakaalustusega lõpp-punkti määramine

Kui lõpp-punktid on osa koormusetasakaalustusega lõpp-punkti, TCP ajalõpp peavad olema määratud koormusetasakaalustusega lõpp-punkti määramine. Näiteks:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Pilveteenustega ajalõpp sätete muutmine

Azure'i SDK abil saate värskendada oma pilveteenuses. .Csdef failis tehtud pilveteenustega lõpp-punkti sätted. TCP ajalõpp kasutuselevõtuks pilveteenus värskendamine nõuab juurutamise versioonitäiendus. Erandiks on, kui TCP ajalõpp määratud ainult avaliku IP. Avaliku IP-sätted on failis .cscfg ja saate värskendada neid juurutamise update ja täiendamine kaudu.

Lõpp-punkti sätete .csdef muudatused on:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Ajalõpp sätte kohta avaliku IP .cscfg muudatused on:

    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
        <InstanceAddress roleName="VMRolePersisted">
        <PublicIPs>
          <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
        </PublicIPs>
        </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="rest-api-example"></a>REST API näide

Saate konfigureerida TCP jõudeaja ajalõpu Teenusehaldus API abil. Veenduge, et selle `x-ms-version` päis on seatud versiooni `2014-06-01` või uuem versioon. Värskendage konfiguratsiooni määratud koormusetasakaalustusega Sisestuskeel lõpp-punktid kõik virtuaalmasinates juurutamine kohta.

### <a name="request"></a>Taotlemine

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Vastus

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Järgmised sammud

[Sisemise koormuse koormusetasakaalustusteenuse ülevaade](load-balancer-internal-overview.md)

[Alustamine on Interneti-ühendusega laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-internet-arm-ps.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)
