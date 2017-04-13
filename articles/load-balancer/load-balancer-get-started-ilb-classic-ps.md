<properties
   pageTitle="Luua ka sisemise laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli PowerShelli kaudu | Microsoft Azure'i"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>PowerShelli kaudu on sisemine laadi koormusetasakaalustusteenuse (klassikaline) loomise alustamiseks

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Saate luua ka sisemise koormuse koormusetasakaalustusteenuse virtuaalmasinates määramine

Loomiseks on sisemine laadi koormusetasakaalustusteenuse seadmine ja saadab sellele oma liikluse serverid, peate tegema järgmist:

1. Looge eksemplari sisemise laadimine tasakaalustamiseks mis liiklust olema laadi koormusetasakaalustusega Sea serverite lõikes lõpp-punktile.

1. Lisage vastav virtuaalmasinates, mida saavad sissetuleva liikluse lõpp-punktid.

1. Konfigureerige serverid, et saata liikluse olema koormus tasakaalustatud saatma oma liikluse virtuaalse IP (VIP) aadressil sisemise laadimine tasakaalustamiseks astme.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Samm 1: Loo sisemise Koormusetasakaalustuseks eksemplar

Olemasoleva pilvepõhise teenuse või piirkondliku virtuaalse võrgu juurutatud pilveteenus, saate luua sisemise Koormusetasakaalustuseks näiteks järgmine Windows PowerShelli käskude abil:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Võtke arvesse, et see [Lisa-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShelli cmdlet-käsu kasutamine kasutab DefaultProbe parameetri määramine. Täiendav parameeter komplekti kohta leiate lisateavet teemast [Lisa-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Samm 2: Lisada sisemise Koormusetasakaalustuseks eksemplari lõpp-punktid

Siin on näide:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Samm 3: Konfigureerimine oma serverid saata oma liiklust uue sisemise Koormusetasakaalustuseks lõpp-punkti

Peate konfigureerima serverid, mille liiklus läheb koormus tasakaalustatud kasutamiseks uus IP-aadress (VIP) eksemplari sisemise laadimine tasakaalustamiseks. See on aluseks on sisemine Koormusetasakaalustuseks eksemplari listening aadress. Enamikul juhtudel peate lihtsalt lisada või muuta DNS-i kirje VIP sisemise Koormusetasakaalustuseks astme.

Kui määrasite sisemise Koormusetasakaalustuseks eksemplari loomise ajal IP-aadress, on teil juba on VIP. Muul juhul näete VIP kaudu järgmised käsud:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Need käsud kasutamiseks sisestage soovitud väärtused ja eemaldamine on < ja >. Siin on näide:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Get-AzureInternalLoadBalancer käsu Kuva, Pange tähele, IP-aadress ja tehke vajalikud muudatused serverid või DNS-i kirjeid, tagamaks, et liikluse saadetakse VIP.

>[AZURE.NOTE]Microsoft Azure'i platvormi kasutab mitmesuguseid haldus stsenaariumid staatilise, avalikult marsruuditavaid IPv4 aadress. IP-aadress on 168.63.129.16. IP-aadress peaks ei blokeeritaks kõik tulemüürid, kuna see võib põhjustada käitu.
>Suhtes Azure'i sisemine laadimine tasakaalustamiseks, IP-aadressi kasutatakse sondid alla laadimine koormusetasakaalustusteenuse jälgides määratlemiseks virtuaalmasinates koormus tasakaalustatud komplekti jaoks seisundioleku. Kui Network turberühma kasutatakse liikluse piiramiseks Azure'i virtuaalmasinates ettevõttesiseselt koormusetasakaalustusega ematermin või rakendatakse virtuaalse alamvõrku, tagama võrgu turvalisus reegli lisamise kaudu 168.63.129.16 liikluse lubamiseks.


## <a name="example-of-internal-load-balancing"></a>Sisemise koormusetasakaalustuseks näide

Teid samm-sammult end, et end loomise protsess kahe näide jaoks koormusetasakaalustusega kogum, leiate järgmistest jaotistest.

### <a name="an-internet-facing-multi-tier-application"></a>Vastastikuste, mitmekihilise rakenduse Interneti

Soovite tagada koormus tasakaalustatud andmebaasi teenus Interneti-ühendusega web serverite kogum. Ühe Azure pilveteenuses on majutatud serverite mõlemas. Web server liikluse TCP port 1433 peab jaotab kaks virtuaalmasinates andmebaasi astme. Joonis 1 näitab konfiguratsiooni.

![Andmebaasi taseme sisemise koormusetasakaalustusega määramine](./media/load-balancer-internal-getstarted/IC736321.png)


Selle konfiguratsiooni koosneb järgmistest:

- Majutusteenuse on virtuaalmasinates olemasoleva pilveteenuses nimega mytestcloud.

- Kahe olemasoleva andmebaasi serverid nimega DEG1 DB2.

- Web servers web astme ühenduse andmebaasi serverid andmebaasi astme abil privaatne IP-aadress. Teine võimalus on kasutada oma DNS-i virtuaalse võrgu ja käsitsi registreerimiseks sisemise koormuse koormusetasakaalustusteenuse määramine A-kirje.

Järgmised käsud konfigureerimine uue sisemise Koormusetasakaalustuseks eksemplari nimega **ILBset** ja lisage lõpp-punktid virtuaalmasinates, mis vastab kaks andmebaasi serverid:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Sisemise Koormusetasakaalustuseks konfigureerimisel eemaldamine

Nimega lõpp virtuaalse masina eemaldamine sisemise koormuse koormusetasakaalustusteenuse eksemplari kasutada järgmisi käske:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Need käsud kasutamiseks sisestage soovitud väärtused, eemaldades selle < ja >.

Siin on näide:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Pilveteenus sisemise koormuse koormusetasakaalustusteenuse eksemplari eemaldamiseks saate kasutada järgmisi käske:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Need käsud kasutamiseks sisestage väärtus ja eemaldamine on < ja >.

Siin on näide:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Lisateavet sisemise koormuse koormusetasakaalustusteenuse cmdlet-käsud


Sisemise Koormusetasakaalustuseks cmdlet-käskude kohta lisateabe saamiseks, käivitage Windows PowerShelli käsuviibas järgmised käsud:

- Abi saamiseks uue AzureInternalLoadBalancerConfig-täielik

- Abi saamiseks lisada AzureInternalLoadBalancer-täielik

- Leidke abi Get-AzureInternalLoadbalancer-täielik

- Abi saamiseks Eemalda-AzureInternalLoadBalancer-täielik

## <a name="next-steps"></a>Järgmised sammud

[Laadi koormusetasakaalustusteenuse jaotuse režiimi kasutamine allika IP osaleja konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)