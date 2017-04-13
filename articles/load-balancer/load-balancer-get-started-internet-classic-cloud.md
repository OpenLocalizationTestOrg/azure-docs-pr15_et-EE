<properties
   pageTitle="Vastastikuste laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli kasutamisel pilveteenustega Interneti loomise alustamiseks | Microsoft Azure'i"
   description="Saate teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli puhul pilveteenuste"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Vastastikuste laadi koormusetasakaalustusteenuse puhul pilveteenuste Interneti loomise alustamiseks

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Samuti saate [teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse Azure'i ressursihaldur abil](load-balancer-get-started-internet-arm-cli.md).

Pilveteenustega on konfigureeritud automaatselt laadi koormusetasakaalustusteenuse ja teenuse mudeli kaudu saab kohandada.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Laadi koormusetasakaalustusteenuse, kasutades teenuse definitsioonifail loomine

Saate kasutada Azure SDK jaoks .NET 2,5 värskendada oma pilveteenuses. Pilveteenustega sätete lõpp-punkti tehakse [teenuse](https://msdn.microsoft.com/library/azure/gg557553.aspx)definitsioonifail .csdef.

Järgmises näites on kujutatud servicedefinition.csdef faili pilvepõhise juurutuse konfigureerimise:

Snipet .csdef faili loodud pilve juurutamise kontrollimine, saate vaadata konfigureeritud kasutama pordid HTTP Port 10000, 10001 ja 10002 väliste lõpp-punkti.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Laadi koormusetasakaalustusteenuse seisundi puhul pilveteenuste oleku vaatamine


Järgmine on näide seisundi juures.

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

Laadi koormusetasakaalustusteenuse ühendab lõpp-punkti teave ja juures luua URL-i kujul http://{DIP, VM}:80/Probe.aspx, mida saate kasutada päringu teenuse seisundi kohta.

Teenuse tuvastab perioodiliste sondid kaudu sama IP-aadress. See on pärit sõlme host, kus töötab virtuaalse masina seisundi juures kutse.
Teenus on vastamise HTTP 200 olek koodiga laadi koormusetasakaalustusteenuse endale, et teenus on terve jaoks. Mis tahes muud HTTP olekukoodi (nt 503) otse võtab virtuaalse masina välja pööre.

Juures määratluse kontrollib ka sagedus on juures. Ülaltoodud meie puhul on laadi koormusetasakaalustusteenuse katsetamine lõpp-punkti iga 5 sekundit. Kui 10 sekundit (kaks juures intervallide) positiivne vastust, eeldatakse, on selle juures allapoole ja virtuaalse masina võetakse kokku pööre. Samamoodi kui teenus on välja pööre ja positiivne vastus saabub, teenuse paigutatakse tagasi pööre kohe. Kui teenus on kõikuv vahel terve ja vigane, laadi koormusetasakaalustusteenuse saate määrata, uuesti kasutuselevõtu teenuse pööre viivitada kuni terve sondid arvu.

Märkige ruut teenuse [seisund juures](https://msdn.microsoft.com/library/azure/jj151530.aspx) Lisateavet määratlus skeemi.

## <a name="next-steps"></a>Järgmised sammud

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-ps.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)

