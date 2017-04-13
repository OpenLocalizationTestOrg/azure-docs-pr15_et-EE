<properties
   pageTitle="Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine | Microsoft Azure'i"
   description="Azure'i laadi koormusetasakaalustusteenuse jaotuse režiimi toetamiseks allika IP osaleja konfigureerimine"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>Jaotuse režiim laadi koormusetasakaalustusteenuse konfigureerimine

## <a name="hash-based-distribution-mode"></a>Räsi-põhise jaotuse režiim

Vaikimisi jaotuse algoritmi on korteeži 5 (lähte-IP lähteport, sihtkoha IP sihtport, protokolli tüüp) räsi liikluse vastendamiseks saadaval serverites. Kleepunud pakub ainult transport seansi jooksul. Paketid sama seansi jooksul suunatakse teid sama andmekeskuse IP (DIP) eksemplari taha koormus tasakaalustatud lõpp-punkti. Kui kliendi käivitab uue seansi sama allika IP, allika pordi muutub ja põhjustab liikluse avamiseks erinevate DIP lõpp-punkti.

![vastavalt räsi laadi koormusetasakaalustusteenuse](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Joonis 1-5-kordse jaotuse.

## <a name="source-ip-affinity-mode"></a>Allika IP osaleja režiim

Meil on teise jaotuse režiimi nimetatakse allika IP osaleja (tuntud ka kui seansiga osaleja või kliendi IP osaleja). Azure'i laadi koormusetasakaalustusteenuse saab konfigureerida liikluse vastendamine saadaval serverid 2-kordse (allika IP, sihtkoha IP) või 3-kordse (allika IP sihtkoha IP, Protocol) abil. Allika IP osaleja abil algatada sama klientarvuti ühendused läheb sama DIP lõpp-punkti.

Järgmine diagramm näitab 2-kordse konfigureerimine. Pange tähele, kuni soovitud laadi koormusetasakaalustusteenuse virtual arvutisse 1 (VM1) mis seejärel varundada VM2 ja VM3 2-kordse töö.

![seansi osaleja](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Joonis 2-2-kordse jaotuse.

Allika IP osaleja lahendab vastuolusid Azure'i laadi koormusetasakaalustusteenuse ja Remote Desktop (RD) lüüsi vahel. Nüüd saate koostada ühe pilveteenuses on RD lüüsi serveripargi.

Mõne muu kasutamine stsenaarium on meediumifailide üleslaadimine kui andmete üleslaadimise UDP kaudu, kuid juhtelemendi lennuk on saavutada TCP.

- Kliendi esmalt käivitab TCP seansi koormus tasakaalustatud avaliku aadressi, saab suunata teatud SUPLUST selle kanali alles aktiivne ühenduse seisundi jälgimine
- Uue seansi UDP sama klientarvuti algatatud sama koormus tasakaalustatud avaliku lõpp-punkti, siin eeldatakse, et sellega suunatakse sama DIP lõpp-punkti nagu eelmise TCP ühendus, et meediumifailide üleslaadimine saab teostada veebisaidil kõrge läbilaskevõime ka säilitades kontrolli kanali TCP kaudu.

>[AZURE.NOTE] Kui koormusetasakaalustusega määramine muudatused (eemaldamine või virtuaalse masina lisamise), klientide päringutele jaotuse on recomputed. Te ei saa sõltuda olemasolevate klientidega samal server jõuavad uued ühendused. Lisaks kasutades allika IP osaleja jaotuse režiimi võib põhjustada mõne liikluse ebavõrdne jaotus. Klientide töötab taha puhverserverite võib käsitleda ühe kordumatu klientrakendusega.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigureerimine allika IP osaleja sätete laadimine koormusetasakaalustusteenuse

Virtuaalmasinates, saate PowerShelli ajalõpp sätete muutmiseks:

Azure'i lõpp lisada virtuaalse masina ja laadi koormusetasakaalustusteenuse jaotuse režiim

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution saate seada sourceIP 2-kordse (allika IP, sihtkoha IP) koormuse tasakaalustamiseks sourceIPProtocol 3-kordse (allika IP sihtkoha IP, protocol) koormusetasakaalustuseks või mitte keegi, kui soovite 5-kordse koormusetasandus vaikekäitumist.

Lõpp-punkti laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimisel toomiseks kasutada järgmist:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

Kui LoadBalancerDistribution element pole Azure'i laadi koormusetasakaalustusteenuse kasutab vaikimisi 5-kordse algoritmi.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Jaotuse režiimi pannud koormus tasakaalustatud lõpp-punkti määramine

Kui lõpp-punktid on osa koormus tasakaalustatud lõpp-punkti, jaotuse režiimi peavad olema määratud koormus tasakaalustatud lõpp-punkti määramine:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Pilveteenuse teenuse konfiguratsiooni muutmiseks jaotuse režiim

Azure'i SDK jaoks .NET 2,5 (antakse välja novembri) saate kasutada värskendada oma pilveteenuses. Pilveteenustega sätete lõpp-punkti tehakse soovitud .csdef. Laadi koormusetasakaalustusteenuse jaotuse režiimi pilveteenustega juurutamise värskendamiseks on vaja juurutamise versioonitäiendus.
Siin on näide .csdef muudatuste lõpp-punkti sätted:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
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

## <a name="api-example"></a>API näide

Laadi koormusetasakaalustusteenuse jaotuse Teenusehaldus API abil saate konfigureerida. Veenduge, et lisada soovitud `x-ms-version` päis on seatud versiooni `2014-09-01` või uuem versioon.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Värskendage määratud koormusetasakaalustusega määramine juurutamine konfiguratsioon

#### <a name="request-example"></a>Taotluse näide

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

LoadBalancerDistribution väärtus võib olla sourceIP 2-kordse osaleja, sourceIPProtocol 3-kordse osaleja jaoks või mitte keegi (jaoks pole osaleja. st 5-kordse)

#### <a name="response"></a>Vastus

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Järgmised sammud

[Sisemise koormuse koormusetasakaalustusteenuse ülevaade](load-balancer-internal-overview.md)

[Töö alustamine Interneti vastastikuste laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-internet-arm-ps.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)
