<properties
    pageTitle="Linux ja avatud lähtekoodi arvutuste Azure | Microsoft Azure'i"
    description="Loetleb Linux ja avatud lähtekoodi arvutuste artikleid Azure, sh põhialused Linux mõned peamised mõisted opsüsteemi või Azure ja muu sisu kohta teatud tehnoloogiad ja Optimeerimised Linux Piltide üleslaadimise kohta."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux ja avatud lähtekoodi arvutuste Azure

Otsige üles kõik dokumendid, mida soovite luua ja hallata Linux-põhine virtuaalmasinates klassikaline juurutamise mudeli.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Alustamine
- [Sissejuhatus Azure'i Linuxi jaoks](virtual-machines-linux-intro-on-azure.md)
- [Korduma kippuvad küsimused kohta Azure'i Virtuaalmasinates loodud mudeliga klassikaline juurutamine](virtual-machines-linux-classic-faq.md)
- [Virtuaalmasinates piltide kohta](virtual-machines-linux-classic-about-images.md)
- [Oma distributsiooni pildi üleslaadimine](virtual-machines-linux-classic-create-upload-vhd.md) (ja ka juhiste abil soovitud [Azure-Endorsed jaotuse](virtual-machines-linux-endorsed-distros.md))
- [Azure'i klassikaline portaali Linux VM abil sisse logida](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Häälestamine

- [Installige Azure'i käsurea liides (Azure'i CLI)](../xplat-cli-install.md)


## <a name="tutorials"></a>Õpetused

- [Azure'i Linux virtuaalse masina LAMP virnas installimine](virtual-machines-linux-create-lamp-stack.md)
- [Ruby rööpad veebirakenduse on Azure VM](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Kuidas: installi Apache Qpid prootonpumba-C AMQP ja teenuse siini](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Andmebaasid
- [MySQL-i Azure jõudluse optimeerimine](virtual-machines-linux-classic-optimize-mysql.md)
- [MySQL-i kogumite](virtual-machines-linux-classic-mysql-cluster.md)
- [Töötab Cassandra Linux Azure ja juurdepääsemiseks Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Mitme juhtslaidi kobar, MariaDbs loomine](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Linux Arvuta sõlmed on HPC Pack klaster Azure kasutamise alustamine](virtual-machines-linux-classic-hpcpack-cluster.md)
- [Käivitage Microsoft HPC Pack Linux NAMD arvutada Azure sõlmed](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Seadistamiseks Linux RDMA kobar MPI rakendused](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Keskmise suurusega
- [Keskmise suurusega VM laiend, käsurea Azure abil kasutajaliides (Azure'i CLI)](virtual-machines-linux-classic-cli-use-docker.md)
- [Keskmise suurusega VM laiend Azure portaali kaudu](virtual-machines-linux-classic-portal-use-docker.md)
- [Azure'i keskmise suurusega-seadme kasutamine](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Kuidas: MySQL-i kogumite](virtual-machines-linux-classic-mysql-cluster.md)
- [Kuidas: Node.js ja Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Kuidas: installida ja käitada MySQL-i](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Kuidas: Azure'i CoreOS kasutamine](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Plaanimine
- [Azure'i taristu teenused rakendamise juhised](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Linux kasutajanimed valimine](virtual-machines-linux-usernames.md)
- [Kuidas seadistada saadavus virtuaalmasinates klassikaline juurutamise mudeli määramine](virtual-machines-linux-classic-configure-availability.md)
- [Kavandatud hooldustööde toimumisaegu Azure VMs ajastamine](virtual-machines-linux-planned-maintenance-schedule.md)
- [Kättesaadavus virtuaalmasinates haldamine](virtual-machines-linux-manage-availability.md)
- [Kavandatud hooldustööde Linux virtuaalmasinates Azure jaoks](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Juurutamine
- [Luua kohandatud virtuaalse masina töötab Linux](virtual-machines-linux-classic-createportal.md)
- [Põhitõed: hõivamine Linux VM malli loomiseks](virtual-machines-linux-classic-capture-image.md)
- [Teave-kinnitatud üldiste müügijaotustega tutvumine](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Haldus

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Kuidas lähtestada parooli või SSH Linuxi atribuudid](virtual-machines-linux-classic-reset-access.md)
- [Root abil](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure'i ressursid

- [Azure'i Linux Agent](virtual-machines-linux-agent-user-guide.md)
- [Azure'i VM laiendid ja -funktsioonid](virtual-machines-windows-extensions-features.md)
- [Kohandatud andmete süstimist VM pilveteenuses – käivitamise kasutamiseks](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Salvestusruumi

- [Manustamise andmed kettale Linux VM](virtual-machines-linux-classic-attach-disk.md)
- [Andmete ketas, Linux VM saidimääratlusest](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Võrgunduse
- [Kuidas häälestada klassikaline virtual arvutisse Azure lõpp-punktid](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Tõrkeotsing
- [Secure Shell (SSH) Ühenduste Linux-põhine Azure virtuaalse masina tõrkeotsing](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Klassikaline juurutamise seotud luua uue Linuxi virtuaalse masina Azure probleemide tõrkeotsing](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Klassikaline juurutamise taaskäivitada või suurust muuta olemasolevaid Linuxi virtuaalse masina Azure seotud probleemide tõrkeotsing](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Viide

- [Azure'i CLI käsud Azure'i Teenusehaldus (asm) režiimis](../virtual-machines-command-line-tools.md)
- [Azure'i Teenusehaldus REST API-ga](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Lingid
Järgmised lingid on Microsoft ajaveebide, TechNeti lehtede, ja väliste saitide, mitte nagu eespool kirjeldatud Azure.com dokumentatsiooni. Azure'i nii avatud lähtekoodi arvuti maailmas on kiiresti sihtkohtade, on peaaegu kindel, kas järgmised lingid on aegunud, *vaatamata* sellele, et me teeb pidevalt uuemaid teemasid lisada ja eemaldada aegunud neist parima. Kui oleme ühte unustanud, võtke ühendust kommentaaride, või esitada meie [GitHub repo](https://github.com/Azure/azure-content/)taotluse tõmmata.

- [Töötab ASP.net-i 5 Linux keskmise suurusega ümbriste abil](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Juurutamise VM pildil oleva CentOS OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [SUSE Update taristu](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SAP-i Cloud seadme teegi SUSE Linux Enterprise Server](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Koosteüksuse tugevalt saadaval Linuxi Azure 12 toimingut](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Azure'i Azure CLI, node.js, jhawk ettevalmistamise Linuxi automatiseerimine](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [GlusterFS Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [Azure'i FreeBSD töötab?](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Lihtne FreeBSD juurutamine](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Kohandatud FreeBSD pilt juurutamine](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV Linux faili Server](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 avatud lähtekoodi NoSql andmebaaside Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [SlideShare (MSOpenTech): CouchDb Azure kogemusi](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [Töötab CouchDB-kui-a-Service node.js, CORS ja Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Windows Azure'i Redis vahemälu teenus redis](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [ASP.net-i seansi olek pakkuja Redis eelvaateversiooniga väljakuulutamist](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Ajaveeb: RavenHQ nüüd saadaval Azure'i turuplats](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Big Data
- [Azure'i Linux VMs Hadoopi installimine](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure Hdinsightiga](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relatsiooniandmebaasist
- [MySQL-i kõrge kättesaadavus arhitektuur Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Installimine Postgres corosync pg_bouncer ILB abil](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux suure jõudlusega arvutuste (HPC)

- [Malli Kiirjuhend: tööasendisse SLURM kobar](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (ja [ajaveebipostituse](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Malli Kiirjuhend: luua mõne HPC kobar Linux Arvuta sõlmed](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>DevOps, haldus ja optimeerimine

Nimega devops maailmas, haldamine ja optimeerimine on üsna suur ja muutmisega väga kiiresti, võiksite kaaluda loendi alguses all.

- [Video: Azure'i Virtuaalmasinates: abil Chef, nuku ja keskmise suurusega haldamise Linux VMs](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Automatiseeritud Kubernetes kobar juurutus, nii et CoreOS ja jutustama täielik teejuht](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Jenkins ori Azure lisandmoodul](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub repo: Jenkins salvestusruumi Azure lisandmoodul](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Muu tootja: Hudson ori Azure lisandmoodul](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Muu tootja: Hudson salvestusruumi Azure lisandmoodul](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Video: Mis on Chef ja kuidas see toimib?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Video: Kuidas kasutada Linux VMs Azure automatiseerimine](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Ajaveeb: Kuidas PowerShelli DSC Linuxi](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Keskmise suurusega kliendi DSC](https://github.com/anweiss/DockerClientDSC)

- [Azure pakendaja lisandmoodul](https://github.com/msopentech/packer-azure)
