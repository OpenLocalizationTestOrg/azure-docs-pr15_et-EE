<properties
 pageTitle="Arvutage võrdlusalus hinded Windows vms | Microsoft Azure'i"
 description="Azure'i vms, kus töötab Windows Server SPECint Arvuta võrdlusalus hinded võrdlus"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="cynthn"/>

# <a name="compute-benchmark-scores-for-windows-vms"></a>Arvutage võrdlusalus hinded Windows vms

Järgmised SPECInt võrdlusalus tulemused näitavad Arvuta jõudlust Azure suure jõudlusega VM loetelu, kus töötab Windows Server. Arvuta võrdlusalus hinded saadaolevaid [Linux](virtual-machines-linux-compute-benchmark-scores.md)vms.



## <a name="a-series---compute-intensive"></a>A-sarja-Arvuta mahukat


Suurus | vCPUs | NUMA sõlmed | CPU | Käivitatakse | AVG määr | StdDev
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_A8 | 8 | 1 | Inteli Xeon CPU E5-2670 0 @ 2,6 GHz | 10 | 236,1 | 1.1
Standard_A9 | 16 | 2 | Inteli Xeon CPU E5-2670 0 @ 2,6 GHz | 10 | 450,3 | 7.0
Standard_A10 | 8 | 1 | Inteli Xeon CPU E5-2670 0 @ 2,6 GHz | 5 | 235.6 | 0,9
Standard_A11 | 16 | 2 | Inteli Xeon CPU E5-2670 0 @ 2,6 GHz |7 | 454.7 | 4.8

## <a name="dv2-series"></a>Dv2-sarja


Suurus | vCPUs | NUMA sõlmed | CPU | Käivitatakse | AVG määr | StdDev
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_D1_v2 | 1 | 1 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 83 | 36,6 | 2.6
Standard_D2_v2 | 2 | 1 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 27 | 70,0 | 3,7
Standard_D3_v2 | 4 | 1 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 19 | 130,5 | 4.4
Standard_D4_v2 | 8 | 1 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 19 | 238,1 | 5.2
Standard_D5_v2 | 16 | 2 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 14 | 460.9 | 15.4
Standard_D11_v2 | 2 | 1 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 19 | 70,1 | 3,7
Standard_D12_v2 | 4 | 1 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 2 | 132.0 | 1.4
Standard_D13_v2 | 8 | 1 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 17 | 235.8 | 3.8
Standard_D14_v2 | 16 | 2 | Inteli Xeon E5-2673 v3 @ 2.4 GHz | 15 | 460.8 | 6.5


## <a name="g-series-gs-series"></a>G-sarja kuuluv GS-sarja


Suurus | vCPUs | NUMA sõlmed | CPU | Käivitatakse | AVG määr | StdDev
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_G1 Standard_GS1  | 2 | 1 | Inteli Xeon E5-2698B v3 @ 2 GHz | 31 | 71.8 | 6.5
Standard_G2 Standard_GS2 | 4 | 1 | Inteli Xeon E5-2698B v3 @ 2 GHz | 5 | 133,4 | 13,0
Standard_G3 Standard_GS3 | 8 | 1 | Inteli Xeon E5-2698B v3 @ 2 GHz | 6 | 242.3 | 6.0
Standard_G4 Standard_GS4 | 16 | 1 | Inteli Xeon E5-2698B v3 @ 2 GHz | 15 | 398.9 | 6.0
Standard_G5 Standard_GS5 | 32 | 2 | Inteli Xeon E5-2698B v3 @ 2 GHz | 22 | 762,8 | 3,7

## <a name="h-series"></a>H seeria

Suurus | vCPUs | NUMA sõlmed | CPU | Käivitatakse | Iteratsiooni sekundis | StdDev
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_H8 | 8 | 1 | Inteli Xeon E5-2667 v3 @ 3,2 GHz | 5 | 297.4 | 0,9
Standard_H16 | 16 | 2 | Inteli Xeon E5-2667 v3 @ 3,2 GHz | 5 | 575.8 | 6,8
Standard_H8m | 8 | 1 | Inteli Xeon E5-2667 v3 @ 3,2 GHz | 5 | 297.0 | 1.2
Standard_H16m | 16 | 2 | Inteli Xeon E5-2667 v3 @ 3,2 GHz | 5 | 572.2 | 3.9
Standard_H16r | 16 | 2 | Inteli Xeon E5-2667 v3 @ 3,2 GHz | 5 | 573.2 | 2.9
Standard_H16mr | 16 | 2 | Inteli Xeon E5-2667 v3 @ 3,2 GHz | 7 | 569,6 | 2,8

## <a name="about-specint"></a>SPECint kohta

Windowsi arvude olid arvutatud [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) Windows Server töötab. SPECint käivitanud suvandiga määra suurust (SPECint_rate2006), üks koopia core kohta. SPECint koosneb 12 eraldi katse, iga kolm korda käivitada, võttes iga katse keskväärtuse ja kaal need moodustavad kombineeritud keskmine. Need katsed olid käivitage üle mitme VMs esitada näidatud keskmised hinded.


## <a name="next-steps"></a>Järgmised sammud

* Salvestusruumi võimalusi, ketta üksikasjad ja täiendavad kaalutluste valimise vahel VM suurused leiate teemast [virtuaalmasinates suurused](virtual-machines-windows-sizes.md).
