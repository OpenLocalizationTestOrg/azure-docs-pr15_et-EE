<properties
   pageTitle="Luua eraldi kobar opsüsteemi Windows Azure VMs | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua ja hallata ka Azure teenuse struktuuri kobar kohta, kus töötab Windows Server Azure'i virtuaalmasinates."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Luua kolm sõlm autonoomse teenuse struktuuri kobar Azure'i virtuaalmasinates, kus töötab Windows Server

Selles artiklis kirjeldatud, kuidas luua kobar sisse Windowsi-põhiste Azure'i virtuaalmasinates (VM), kasutades autonoomse teenuse struktuuri installer Windows Server. See on on [loomine ja käivitamine Windows serveris klaster haldamine](service-fabric-cluster-creation-for-windows-server.md) VMs paiknemise [Azure VMs, kus töötab Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md), kuid loomisel [on Azure pilvepõhise teenuse struktuuri kobar](service-fabric-cluster-creation-via-portal.md). Erinevus on järgmiste juhiste järgi loodud eraldi teenuse struktuuri kobar täielikult haldab teie, samal ajal soovitud Azure'i pilvepõhise teenuse struktuuri kogumite hallatavate ja teenuse struktuuri ressursi pakkuja versioonile.


## <a name="steps-to-create-the-standalone-cluster"></a>Autonoomse kobar loomine

1. Azure portaali sisse logida ja luua uue Windows Server 2012 R2 andmekeskuse VM ressursirühma. Lugege lisateavet [loomine Windows Azure'i portaalis VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) artikkel.
2. Sama ressursirühm paari veel Windows Server 2012 R2 andmekeskuse VMs lisada. Veenduge, et iga VMs on sama administraatori kasutajanime ja parooli loomisel. Juba loodud, kuvatakse kõigi kolme VMs virtuaalse samasse võrku.
3. Ühenduse iga VMs ja kasutades [Server Manager, kohaliku serveri armatuurlaua](https://technet.microsoft.com/library/jj134147.aspx)Windowsi tulemüüri väljalülitamine. See võimaldab, võrguliiklust suhelda masinad vahel. Ajal ühendatud igas arvutis, avage käsuviip ja tippige IP-aadressi saada `ipconfig`. Teise võimalusena saate vaadata iga seadme IP-aadress, valides ressursirühma virtuaalse viitab Azure'i portaalis.
4. Ühenduse loomine mõne VMs ja testida kaks VMs edukalt ping.
5. Ühenduse üks VMs ja [autonoomse teenuse struktuuri Laadige Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) uude kausta arvutis ja ekstraktida paketi.
6. Notepad *ClusterConfig.Unsecure.MultiMachine.json* faili avada ja redigeerida iga sõlm koos masinad kolme IP-aadressid. Muutke kobar nime kohal ja salvestage fail.  Osalise näide kobar manifesti on näidatud allpool.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. [PowerShell ISE akna](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise)avamine Liikuge kausta, kus ekstraktimist allalaaditud autonoomse Installeri pakett ja kobar Avaldamisfail salvestatud. Käivitage PowerShelli käsk.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Peaksite nägema PowerShelli käivitada, ühenduse igas arvutis, ning luua klaster. Pärast umbes minuti, saate kontrollida, kui klaster on ühendatud teenuse struktuuri Exploreri ühele masina IP-aadressi nt abil funktsionaalseid `http://10.7.0.5:19080/Explorer/index.html`. Kuna see on autonoomse kobar, kasutades Azure VMs, teha seda turvaline, siis on juurutada [serdid Azure vms](service-fabric-windows-cluster-x509-security.md) või seadistada üks masinad [Windows Server Active Directory (AD) selle domeenikontrolleri Windowsi autentimine](service-fabric-windows-cluster-windows-security.md), nagu teeksite kohapealne.


## <a name="next-steps"></a>Järgmised sammud
- [Opsüsteemis Windows Server ja Linux autonoomse teenuse struktuuri kogumite loomine](service-fabric-deploy-anywhere.md)
- [Lisage või eemaldage sõlmed autonoomse teenuse struktuuri kobar](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Autonoomse Windows kobar sätete konfigureerimine](service-fabric-cluster-manifest.md)
- [Turvaline autonoomse kobar Windows Windows turvalisuse abil](service-fabric-windows-cluster-windows-security.md)
- [Autonoomse kobar abil X509 Windows Secure serdid](service-fabric-windows-cluster-x509-security.md)
