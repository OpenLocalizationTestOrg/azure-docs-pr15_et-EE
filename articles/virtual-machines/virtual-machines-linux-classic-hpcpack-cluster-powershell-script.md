<properties
   pageTitle="PowerShelli skripti juurutamiseks Linux HPC kobar | Microsoft Azure'i"
   description="Käivitage PowerShelli skripti juurutamiseks Linux HPC Pack kobar Azure'i virtuaalmasinates"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Linux suure jõudlusega arvutuste (HPC) kobar HPC Pack IaaS juurutamise skripti loomine

Käivitage PowerShelli skripti juurutamiseks täieliku HPC kobar Linux töökoormus Azure'i virtuaalmasinates HPC Pack IaaS juurutamise. Klaster koosneb on Active Directory liitunud pea sõlme opsüsteemi Windows Server ja Microsoft HPC Pack ja Arvuta sõlmed, mis töötavad ühte Linuxi, HPC Pack ei toeta. Kui soovite mõne HPC Pack kobar rakenduses Windows Azure'i töökoormus juurutada, lugege teemat [loomine Windows HPC kobar HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Saate mõni Azure ressursihaldur mall on HPC Pack kobar juurutamiseks. Näiteks vt [loomine mõne HPC kobar Linux arvutada sõlmed](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Näide konfiguratsioonifail

Järgmised konfiguratsioonifail loob uue domeenikontrolleri ja domeeni mets ja kasutab HPC Pack kobar, kus on 1 pea sõlme kohaliku andmebaasidega ja 10 Linuxi Arvuta sõlmed. Kõik pilveteenustega luuakse Ida-Aasia asukohas. 2 pilveteenustega ja 2 salvestusruumi kontod (st _MyLnxCN-0001_ abil _MyLnxCN-0005_ _MyLnxCNService01_ ja _mylnxstorage01_) ja _MyLnxCN-0006_ _MyLnxCN-0010_ _MyLnxCNService02_ ja _mylnxstorage02_abil luuakse Linux Arvuta sõlmed. Arvuta sõlmed on loodud OpenLogic CentOS versiooni 7.0 Linux pilt. 

Asendada oma väärtuste oma tellimuse nime ja konto ja teenus nimed.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Tõrkeotsing

* **Tõrge "VNet ei eksisteeri"** – kui käivitate HPC Pack IaaS juurutamise skripti juurutada mitme kogumite Azure samaaegselt jaotises üks tellimus, üks või mitu juurutuste võib nurjuda tõrke "VNet *VNet\_nimi* pole olemas".
Selle tõrke ilmnemisel uuesti käivitada skripti nurjunud juurutamiseks.

* **Probleem Interneti Azure virtuaalse võrgust** – kui loote mõnda HPC Pack kobar uue domeenikontrolleri juurutamise skripti abil või käsitsi saab reklaamida pea sõlme VM domeenikontrolleri, võib tekkida probleeme VMs Azure virtuaalse võrgu Interneti-ühenduse loomine. See võib juhtuda siis, kui ekspediitori DNS-server on konfigureeritud automaatselt domeenikontrolleri ja selle ekspediitori DNS-server ei avane õigesti.

    Probleemi lahendamiseks domeenikontrolleri ja kas Eemalda ekspediitor Otsingukonfiguratsiooni säte sisse logida ja konfigureerimiseks kehtiv ekspediitor DNS-i serveri. Selleks valige Server Manager **Tööriistad** >
    **DNS-i** DNS-i halduri, ja seejärel topeltklõpsake **Edasisuunamislahendusi**.
    
## <a name="next-steps"></a>Järgmised sammud

* Kohta leiate teavet [Alustamine Linux Arvuta sõlmed on HPC Pack klaster Azure](virtual-machines-linux-classic-hpcpack-cluster.md) toetatud Linuxi andmete teisaldamine ja esitamine töö, et mõne HPC Pack kobar Linux arvutada sõlmed.
* Õpetusi, mis klaster loomine ja käivitamine Linux HPC töökoormus skripti abil, vt:
    * [Käivitage Microsoft HPC Pack Linux NAMD arvutada Azure sõlmed](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [Käivitage Microsoft HPC Pack OpenFOAM Linux Arvuta sõlmed Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Käivitage täht-CCMi + Microsoft HPC Pack Linux arvutada sõlmed Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
