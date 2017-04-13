<properties
 pageTitle="Linux HPC Pack kobar suvandid pilveteenuses | Microsoft Azure'i"
 description="Lisateabe saamiseks valikute Microsoft HPC Pack luua ja hallata Linux suure jõudlusega arvutuste (HPC) kobar Azure'i pilves kohta"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Suvandite abil saate luua ja hallata HPC kobar Linux töökoormus Azure HPC Pack

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

See artikkel keskendub suvandid HPC Pack abil saate käivitada Linuxi töökoormus. On ka [Windows HPC töökoormus HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md)suvandid.

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Azure VMs käivitamiseks on HPC Pack kobar

### <a name="azure-templates"></a>Azure'i Mallid


* (Turuplats) [Linux töökoormus HPC Pack kobar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Kiirjuhend) [Loo on HPC kobar Linux arvutada sõlmed](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>PowerShelli juurutamise skripti

* [Linux HPC kobar HPC Pack IaaS juurutamise skripti loomine](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Õpetused

* [Õpetus: Alustamine Linux Arvuta sõlmed on Azure HPC Pack kobar](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Õpetus: Käivita NAMD Microsoft HPC Pack Linux arvutada Azure sõlmed](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Õpetus: Käivita OpenFOAM Microsoft HPC Pack Linux RDMA klaster Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Õpetus: Käivita täht-CCMi + koos Microsoft HPC Pack Linux RDMA klaster Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Kobar haldus

* [Edastab tööd on Azure HPC Pack kobar](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Töö haldus HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA kogumite MPI töökoormus loomine

* [Õpetus: Käivita OpenFOAM Microsoft HPC Pack Linux RDMA klaster Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Seadistamiseks Linux RDMA kobar MPI rakendused](virtual-machines-linux-classic-rdma-cluster.md)

