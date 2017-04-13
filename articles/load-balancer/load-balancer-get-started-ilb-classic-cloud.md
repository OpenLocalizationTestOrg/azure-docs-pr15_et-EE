<properties
   pageTitle="Luua ka sisemise koormuse koormusetasakaalustusteenuse puhul pilveteenuste klassikaline juurutamise mudeli | Microsoft Azure'i"
   description="Saate teada, kuidas luua ka sisemise laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli PowerShelli kaudu"
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
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Mõne sisemise koormuse koormusetasakaalustusteenuse puhul pilveteenuste (klassikaline) loomise alustamiseks

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Puhul pilveteenuste sisemise koormuse koormusetasakaalustusteenuse konfigureerimine

Sisemise koormuse koormusetasakaalustusteenuse on toetatud nii virtuaalmasinates ja pilveteenustega. Sisemise koormuse koormusetasakaalustusteenuse lõpp loodud pilveteenuses, mis on väljaspool piirkondliku virtuaalse võrgu pääseb üksnes pilveteenusesse.

Sisemise koormuse koormusetasakaalustusteenuse konfigureerimine peab olema ajal luua esimese juurutamise pilveteenuses, nagu on näidatud allpool valimis.

>[AZURE.IMPORTANT] Nõutav alltoodud juhiseid käivitamiseks on virtuaalse võrgu juba loonud juurutamiseks pilve. Peate virtuaalse võrgu nimi ja alamvõrgu nime sisemise Koormusetasakaalustuseks loomiseks.

### <a name="step-1"></a>Samm 1

Avage oma pilvepõhise juurutuse teenuse konfiguratsioonifail (.cscfg) Visual Studio ja lisage loomine jaotises Viimane sisemise Koormusetasakaalustuseks järgmisest jaotisest "`</Role>`" üksuse võrgu konfigureerimise kohta.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Väärtuste ilme kuvamiseks võrgu konfigureerimise faili lisada. Näites endale loodud on nimega "test_vnet" on alamvõrgu 10.0.0.0/24 nimega test_subnet ja 10.0.0.4 staatiline IP alamvõrgu. Laadi koormusetasakaalustusteenuse nimeks testLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Laadi koormusetasakaalustusteenuse skeemi kohta leiate lisateavet teemast [laadimine koormusetasakaalustusteenuse lisamine](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Samm 2


Saate muuta teenuse määratlus (.csdef) faili lisamiseks lõpp-punktid sisemise Koormusetasakaalustuseks. Hetkel rolli eksemplari on loodud, teenuse definitsioonifail lisab rolli aknad sisemise Koormusetasakaalustuseks.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Pärast ülaltoodud näites sama väärtustega lisada teenuse määratluse faili väärtused.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Laadi tasakaalustatud testLB laadi koormusetasakaalustusteenuse sissetulevad taotlused, saates töötaja rolli aknad ka porti 80 pordi 80 kasutamise abil saab võrguliiklust.


## <a name="next-steps"></a>Järgmised sammud

[Laadi koormusetasakaalustusteenuse jaotuse režiimi kasutamine allika IP osaleja konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)