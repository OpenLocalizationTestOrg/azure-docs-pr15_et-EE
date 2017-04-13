<properties
   pageTitle="PowerShelli skripti juurutamiseks Windows HPC kobar | Microsoft Azure'i"
   description="Käivitage PowerShelli skripti juurutamiseks Windows HPC Pack kobar Azure'i virtuaalmasinates"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Suure jõudlusega arvutuste (HPC) klaster Windows HPC Pack IaaS juurutamise skripti loomine

Käivitage PowerShelli skripti juurutamiseks täieliku HPC kobar Windows töökoormus Azure'i virtuaalmasinates HPC Pack IaaS juurutamise. Klaster koosneb on Active Directory liitunud pea sõlme opsüsteemi Windows Server ja Microsoft HPC Pack ja täiendavad Windows arvutada määratud ressursse. Kui soovite juurutada HPC Pack kobar Azure Linux töökoormus, lugege teemat [loomine Linux HPC kobar HPC Pack IaaS juurutamise skripti](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Saate mõni Azure ressursihaldur mall on HPC Pack kobar juurutamiseks. Vt näited [loomine mõne HPC kobar](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) ja [Loo kohandatud mõne HPC kobar arvutada sõlm pilt](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Näide failid

Järgmistes näidetes asendada oma tellimuse Id väärtused või nimi ja konto ja teenus nimed.

### <a name="example-1"></a>Näide 1

Järgmised konfiguratsioonifail kasutab HPC Pack kobar, mis sisaldab pea sõlme kohaliku andmebaasidega ja 5 arvutada sõlmed, kus töötab opsüsteem Windows Server 2012 R2. Kõik pilveteenustega luuakse Lääne USA asukohas. Pea sõlme toimib domeenikontrolleri domeeni mets.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Näide 2

Järgmised konfiguratsioonifail kasutab mõnda HPC Pack kobar mõne olemasoleva domeeni mets. Klaster on 1 pea sõlme kohaliku andmebaasidega ja 12 arvutada sõlmed rakendatud BGInfo VM laiendiga.
Automaatse Windowsi värskenduste installimine on keelatud kõik vms domeeni mets. Kõik pilveteenustega luuakse Ida-Aasia asukohas. Arvuta sõlmed luuakse kolme pilveteenustega ja kolm salvestusruumi kontod: _MyHPCCN-0001_ , et _MyHPCCN-0005_ _MyHPCCNService01_ ja _mycnstorage01_; _MyHPCCN-0006_ , et _MyHPCCN0010_ _MyHPCCNService02_ ja _mycnstorage02_; ja et _MyHPCCN-0012_ _MyHPCCNService03_ ja _mycnstorage03_ _MyHPCCN-0011_ ). Arvuta sõlmed on loodud saadud Arvuta sõlme privaatne olemasoleva pildi. Automaatse kasvada ja vähendage teenus on lubatud, kus vaikevalik on kasvada ja vähendage intervalle.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Näide 3

Järgmised konfiguratsioonifail kasutab mõnda HPC Pack kobar mõne olemasoleva domeeni mets. Klaster sisaldab ühe pea sõlme, üks andmebaasiserveri 500 GB andmete kettal, 2 ta sõlmed töötab operatsioonisüsteem Windows Server 2012 R2 ja viis Arvuta sõlmed töötab operatsioonisüsteem Windows Server 2012 R2. Pilveteenuses MyHPCCNService luuakse osaleja jaotises *MyIBAffinityGroup*ja muude pilveteenustega luuakse osaleja jaotises *MyAffinityGroup*. HPC töö Toiminguajasti REST API ja HPC veebiportaali on lubatud pea sõlme.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Näide 4

Järgmised konfiguratsioonifail kasutab mõnda HPC Pack kobar mõne olemasoleva domeeni mets. Klaster on kaks pea sõlme kohaliku andmebaasidega, luuakse kaks Azure sõlme malle ja kolme suurus keskmise Azure sõlmed on loodud Azure sõlme malli _AzureTemplate1_. Skripti faili töötab pea sõlme pärast pea sõlme on konfigureeritud.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Tõrkeotsing


* **Tõrge "VNet ei eksisteeri"** – kui käivitate skripti juurutada mitme kogumite Azure samaaegselt jaotises üks tellimus, üks või mitu juurutuste võib nurjuda tõrke "VNet *VNet\_nimi* pole olemas".
Selle tõrke ilmnemisel käivitage skript nurjunud juurutamiseks.

* **Probleem Interneti Azure virtuaalse võrgust** – kui loote klaster uue domeenikontrolleri juurutamise skripti abil või käsitsi saab reklaamida pea sõlme VM domeenikontrolleri, võib tekkida probleeme VMs Interneti-ühenduse loomine. See probleem võib ilmneda juhul, kui ekspediitori DNS-server on konfigureeritud automaatselt domeenikontrolleri ja selle ekspediitori DNS-server ei avane õigesti.

    Probleemi lahendamiseks domeenikontrolleri ja kas Eemalda ekspediitor Otsingukonfiguratsiooni säte sisse logida ja konfigureerimiseks kehtiv ekspediitor DNS-i serveri. Selle sätte konfigureerimiseks klõpsake Server Manager **Tööriistad** >
    **DNS-i** DNS-i halduri, ja seejärel topeltklõpsake **Edasisuunamislahendusi**.

* **Juurdepääs RDMA võrgu kaudu Arvuta mahukat VMs probleem** – kui lisate Windows Server Arvuta või maakler sõlme VMs abil soovitud RDMA-ühenduse võimalusega suurus, nt A8 või A9, võib tekkida probleeme need RDMA rakenduse võrguga ühendus luua. See probleem ilmneb üks põhjus on, kui HpcVmDrivers laiend pole installitud õigesti VMs lisamisel klaster. Näiteks võite laiendamine kinni installimise olekus.

    Selle probleemi lahendamiseks esmalt vaadata VMs laiendamine. Kui laiendamine on õigesti installitud, proovige sõlmed eemaldamine HPC kobar ja lisage sõlmed. Näiteks saate lisada MSDN VMs lisa-HpcIaaSNode.ps1 skripti käivitades pea sõlme.
    
## <a name="next-steps"></a>Järgmised sammud

* Proovige käivitada klaster testi töökoormus. Näiteks vt HPC Pack [alustamisjuhendit](https://technet.microsoft.com/library/jj884144).

* Skript kobar juurutamise ja käivitada mõne HPC töökoormus, juhendi leiate [alustamine on HPC Pack kobar Azure käivitamiseks Exceli ja SOA töökoormus](virtual-machines-windows-excel-cluster-hpcpack.md).

* Proovige HPC Pack tööriistad alustada, peatada, lisada ja eemaldada Arvuta sõlmed klaster loomist. Vt [haldamine arvutada mõne HPC Pack klaster Azure sõlmed](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Häälestamine esitada töid klaster kohalikku arvutisse, lugege teemat [esitada HPC töö kohapealse arvutist mõne HPC Pack arvutikobaras Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
