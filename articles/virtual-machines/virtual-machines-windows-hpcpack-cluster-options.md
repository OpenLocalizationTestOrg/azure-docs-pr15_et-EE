<properties
 pageTitle="Windows HPC Pack kobar suvandid pilveteenuses | Microsoft Azure'i"
 description="Lisateabe saamiseks valikute Microsoft HPC Pack loomiseks ja haldamiseks Windows suure jõudlusega arvutuste (HPC) kobar Azure'i pilves kohta"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Suvandite abil saate luua ja hallata Windows HPC kobar Azure HPC Pack

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

See artikkel keskendub HPC Pack kogumite käivitamiseks Windows töökoormus loomiseks. On ka võimalusi loomise kogumite [Linux HPC töökoormus HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md)käivitamiseks.


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Azure VMs käivitamiseks on HPC Pack kobar

### <a name="azure-templates"></a>Azure'i Mallid

* (Turuplats) [Windowsi töökoormus HPC Pack kobar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Turuplats) [Exceli töökoormus HPC Pack kobar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Kiirjuhend) [Loo on HPC kobar](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Kiirjuhend) [Loo kohandatud Arvuta sõlm pildiga on HPC kobar](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure'i VM pildid

* [HPC Pack Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [HPC Pack MSDN Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack arvutada sõlm Windows Server 2012 R2 Exceliga](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>PowerShelli juurutamise skripti

* [Mõne HPC kobar HPC Pack IaaS juurutamise skripti loomine](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Õpetused

* [Õpetus: Alustamine on Excelis ja SOA töökoormus käivitamiseks Azure HPC Pack kobar](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Käsitsi juurutus, nii et Azure'i portaal

* [Mõne HPC Pack kobar rakenduses on Azure VM pea sõlme häälestamine](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Kobar haldus

* [Arvuta sõlmed on HPC Pack klaster Azure haldamine](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Kasvata ja vähendage Azure Arvuta ressursside mõne HPC Pack kobar](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Edastab tööd on Azure HPC Pack kobar](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Töö haldus HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Töötaja rolli sõlmed lisada mõne HPC Pack kobar


* [Azure'i töötaja eksemplarides HPC Pack lõhkeda](https://technet.microsoft.com/library/gg481749.aspx)

* [Õpetus: Häälestamine hübriid kobar HPC Pack Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [HPC Pack pea sõlme Azure'i Azure "plahvatuse" sõlmed lisamine](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Azure'i paketi integreerimine 

* [Azure'i paketti HPC Pack lõhkeda](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA kogumite MPI töökoormus loomine

* [MPI rakenduste käivitamiseks Windows RDMA kobar HPC Pack häälestamine](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
