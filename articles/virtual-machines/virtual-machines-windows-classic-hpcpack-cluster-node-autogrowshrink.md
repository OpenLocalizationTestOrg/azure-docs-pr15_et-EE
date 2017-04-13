<properties
 pageTitle="Autoscale HPC Pack kobar sõlmed | Microsoft Azure'i"
 description="Automaatselt kasvada ja vähendage HPC Pack kobar Arvuta sõlmed Azure arv"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automaatselt kasvada ja vähendage HPC Pack kobar ressursside Azure vastavalt töökoormus kobar




Kui juurutate Azure "plahvatuse" sõlmed klaster HPC Pack või loote mõne HPC Pack kobar Azure VMs, võite võimalus automaatselt kasvata või Kahanda Azure Arvuta ressursside nagu sõlmed või tuuma vastavalt klaster praeguse töökoormus. See võimaldab teil kasutada oma Azure ressursse tõhusamalt ja nende juhtimine.
Selleks saate seadistada HPC Pack kobar atribuut **AutoGrowShrink**. Teise võimalusena käivitage **AzureAutoGrowShrink.ps1** HPC PowerShelli skripti, mis installitakse koos HPC Pack.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lisaks praegu saate automaatselt ainult kasvada ja vähenda HPC Pack Arvuta sõlmed, kus töötab operatsioonisüsteem Windows Server on.

## <a name="set-the-autogrowshrink-cluster-property"></a>Määrake atribuudi AutoGrowShrink kobar

### <a name="prerequisites"></a>Eeltingimused

* **HPC Pack 2012 R2 Update 2 või uuemad versioonid kobar** - kobar pea sõlme võib olla juurutada kas kohapeal või mõne Azure VM. Vaadake [hübriidjuurutuse kobar HPC Pack häälestamine](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) on kohapealse pea sõlme ja Azure "plahvatuse" sõlmed kasutamise alustamine. Lugege teemat [HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) kiiresti juurutada HPC Pack kobar Azure'i VM.


* **Klaster koos pea sõlme Azure jaoks** – kui HPC Pack IaaS juurutamise skripti abil luua klaster, määrates suvandi AutoGrowShrink kobar konfiguratsioonifailis **AutoGrowShrink** kobar atribuudi Luba. Lisateavet leiate teemast dokumentatsiooni lisatud [skripti alla laadida](https://www.microsoft.com/download/details.aspx?id=44949). 

    Teise võimalusena lubada atribuudi **AutoGrowShrink** kobar pärast juurutamist klaster järgmises jaotises kirjeldatud HPC PowerShelli käskude abil. Ettevalmistamiseks see esmalt tehke järgmist:
    1. Azure'i halduse serdi konfigureerimine pea sõlme ja Azure tellimus. Testi juurutamiseks saate kasutada vaikimisi Microsoft Azure HPC iseallkirjastatud serdi HPC Pack installib pea sõlme ja Azure tellimuse lihtsalt selle serdi üleslaadimine. Suvandite ja juhiseid leiate teemast [TechNeti teegi juhised](https://technet.microsoft.com/library/gg481759.aspx).
    2. Käivitage **käsk regedit** pea sõlme, minge HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo ja lisada uue stringiväärtuse. Määrake väärtus nime "Sõrmejälje" ja samm 1 serdi sõrmejälje andmete väärtus.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>Määrake atribuudi AutoGrowShrink HPC PowerShelli käskude

Järgmine on Kuulake HPC PowerShelli käskude **AutoGrowShrink** ja häälestada oma käitumine täiendavad parameetritega. Vt [AutoGrowShrink parameetrite](#AutoGrowShrink-parameters) täieliku loendi leiate selle artikli sätted. 

Nende käskude käivitamine HPC PowerShelli kobar pea sõlme administraatorina.

**Atribuudi AutoGrowShrink lubamiseks**

    Set-HpcClusterProperty –EnableGrowShrink 1

**Atribuudi AutoGrowShrink keelamine**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Kasvata intervalli muutmiseks märkige minutiga**

    Set-HpcClusterProperty –GrowInterval <interval>

**Kahanda intervalli muutmiseks märkige minutiga**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**Praeguse konfiguratsiooni AutoGrowShrink kuvamiseks**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink parameetrid

Järgmisena kirjeldame käsu **Set-HpcClusterProperty** abil saate muuta AutoGrowShrink parameetrid.

* **EnableGrowShrink** - lubamine või keelamine atribuudi **AutoGrowShrink** aktiveerimine.
* **ParamSweepTasksPerCore** - parameetrite Korrasta tööülesannete kasvada ühe core arv. Vaikimisi on üks tuum ülesande kasvada. 
 
    >[AZURE.NOTE] HPC Pack QFE KB3134307 muudab **ParamSweepTasksPerCore** **TasksPerResourceUnit**. See põhineb töö ressursi tüüp ja võib olla sõlm, turvasoklite või core.
    
* **GrowThreshold** - lävi ootel tööülesannete automaatne kasvu käivitamiseks. Vaikeväärtus on 1, mis tähendab, mis kui 1 või mitu tööülesannet on ootel olekus automaatselt kasvada sõlmed.
* **GrowInterval** - intervall automaatne kasvu käivitamiseks. Vaikimisi intervalli minutit.
* **ShrinkInterval** - intervall automaatse kahanemine käivitamiseks. Vaikimisi intervall on 5 minutit. |
* **ShrinkIdleTimes** - pidev kontrolli Kahanda tähistamiseks sõlmed jõude. Vaikesäte on 3 korda. Näiteks kui **ShrinkInterval** on 5 minutit, HPC Pack kontrollib, kas sõlme on jõude iga 5 minuti järel. Kui sõlmed on jõudeolekus pärast 3 pidev kontrollib (15 minutit), siis HPC Pack ühel vaja sõlme.
* **ExtraNodesGrowRatio** - täiendavad protsent sõlmed kasvada töökohta sõnumi läbimise kasutajaliidese (MPI). Vaikeväärtus on 1, mis tähendab, et HPC Pack kasvab sõlmed 1% MPI tööde haldamine. 
* **GrowByMin** – näitab, kas autogrow poliitika põhineb töö minimaalsete ressursside aktiveerimiseks. Vaikimisi on false, mis tähendab, et HPC Pack kasvab sõlmed maksimaalne ressursid, mis on vajalikud tööde tööde jaoks.
* **SoaJobGrowThreshold** - lävi SOA sissetulevad taotlused automaatne käivitamine kasvata protsess. Vaikeväärtus on 50000.  
    
    >[AZURE.NOTE] See parameeter on toetatud, alustades HPC Pack 2012 R2 Update 3.
    
* **SoaRequestsPerCore** -arvu sissetulevad SOA taotleb üks tuum kasvada. Vaikeväärtus on 20000.  

    >[AZURE.NOTE] See parameeter on toetatud, alustades HPC Pack 2012 R2 Update 3.

### <a name="mpi-example"></a>MPI näide

Vaikimisi kasvab HPC Pack 1% eest sõlmed MPI tööde (**ExtraNodesGrowRatio** väärtuseks 1). Põhjus on selles, et MPI võib olla vaja mitme sõlmed ja töö saab käivitada ainult siis, kui kõik sõlmed on valmis. Azure'i käivitamisel sõlmed aeg-ajalt ühe sõlme võib vaja rohkem aega, kui teised põhjustada sõlmi olema jõude ooteajal sõlme saada valmis alustada. Kasvab eest sõlmed, HPC Pack vähendab seekord ressursi ootel ja potentsiaalselt salvestab kulud. Sarnane käsu käivitada eest sõlmed MPI tööde (nt kuni 10%) protsent suurendamiseks

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA näide

Vaikimisi on seatud **SoaJobGrowThreshold** 50000 ja **SoaRequestsPerCore** on seatud arv 200 000. Kui saadate ühe SOA töö 70000 taotlusi, on ootel ühe tööülesande ja sissetulevad taotlused on 70000. Sel juhul HPC Pack kasvab amortisatsioonimäär 1 core ootel tööülesandele ja sissetulevad taotlused (70000 50000) / core 20000 = 1, et kokku kasvab 2 tuuma selle SOA töö.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Käivitage AzureAutoGrowShrink.ps1 skript

### <a name="prerequisites"></a>Eeltingimused

* **HPC Pack 2012 R2 Update 1 või uuem versioon kobar** - **AzureAutoGrowShrink.ps1** script on installitud prügikasti kaust % CCP_HOME %. Kobar pea sõlme võib olla juurutada kas kohapeal või mõne Azure VM. Vaadake [hübriidjuurutuse kobar HPC Pack häälestamine](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) on kohapealse pea sõlme ja Azure "plahvatuse" sõlmed kasutamise alustamine. Vaadake [HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) kiiresti juurutada HPC Pack kobar Azure'i VM või [Azure Kiirjuhend malli](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/)kasutamiseks.

* **Azure'i PowerShelli 0.8.12** - skript praegu sõltub selle teatud versioon Azure PowerShell. Kui teil on mõne uuema versiooni pea sõlme, peate alandada Azure PowerShelli skripti käivitamiseks [versioon 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) . 

* **Jaoks klaster koos Azure lõhkemist sõlmed** - skripti käivitada kliendi arvutisse, kuhu on installitud HPC Pack või pea sõlme. Kui kliendi arvutisse, veenduge, et teie määratud muutuja $env: CCP_SCHEDULER õigesti osutamiseks pea sõlme. Azure'i "plahvatuse" sõlmed tuleb lisada juba klaster, kuid neid ei juurutatud olekus.


* **Klaster jaoks juurutada Azure VMs** - käivitage skript pea sõlme VM, kuna see sõltub **Algus-HpcIaaSNode.ps1** ja **Peata-HpcIaaSNode.ps1** skripte, mis installitakse seal. Nende skriptide lisaks vaja sertifikaadi Azure haldus või avalda sätete fail (vt [haldamine arvutada sõlmed on Azure HPC Pack klaster](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Veenduge, et kõik MSDN peate VMs on juba lisatud klaster. Need võivad olla peatatud olekus.

### <a name="syntax"></a>Süntaks

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parameetrid

 * **NodeTemplates** - nimed sõlm malli seostamise sõlmed kasvada ja vähenda. Kui määramata (vaikeväärtus on @()), ulatus on kõik sõlmed **AzureNodes** sõlm jaotises **NodeType** on AzureNodes väärtus, kui ulatus on kõik sõlmed **ComputeNodes** sõlm jaotises kui **NodeType** on ComputeNodes väärtus.

 * **JobTemplates** - nimed töö malli seostamise sõlmed kasvada.

 * **NodeType** - tüüpi sõlm kasvada ja vähenda. Toetatud väärtused on:

     * **AzureNodes** – jaoks Azure'i PaaS (purune) sõlmed kohapealses või Azure'i IaaS kobar.

     * **ComputeNodes** - Arvuta sõlme VMs ainult mõne Azure'i IaaS kobar jaoks.

* **NumOfQueuedJobsPerNodeToGrow** - arvu kasvamiseks ühe sõlme nõutav järjekorras olevad tööd.

* **NumOfQueuedJobsToGrowThreshold** - järjekorras olevad tööd alustada käigus kasvab piirarv.

* **NumOfActiveQueuedTasksPerNodeToGrow** - active ootel tööülesannete kasvada ühe sõlme nõutav arv. Kui määratud on **NumOfQueuedJobsPerNodeToGrow** , mille väärtus on suurem kui 0, ignoreeritakse see parameeter.

* **NumOfActiveQueuedTasksToGrowThreshold** - ootel aktiivseid tööülesandeid, kasvamine alustamiseks piirarv.

* **NumOfInitialNodesToGrow** - algne väikseima arvu sõlmed kasvada, kui ulatus sõlme **Ei juurutatud** või **peatatud (Deallocated)**.

* **GrowCheckIntervalMins** - intervall minutites vahel kontrollid kasvada.

* **ShrinkCheckIntervalMins** - intervall minutites vahel kontrollid kahandamiseks.

* **ShrinkCheckIdleTimes** - arvu pidev Kahanda tähistamiseks sõlmed jõude kontrollid (eraldatud **ShrinkCheckIntervalMins**).

* **UseLastConfigurations** - failis argumendi salvestatud eelmise kuvatakse.

* **ArgFile**- kasutatud salvestamine ja värskendamine konfiguratsioone skripti käivitamiseks argument-faili nimi.

* **LogFilePrefix** - eesliite logifaili nimi. Saate määrata tee. Vaikimisi Logi kirjutatakse praegust töötavat kausta.

### <a name="example-1"></a>Näide 1

Järgmises näites konfigureerib Azure lõhkemist sõlmed juurutatud kasvada ja vähendage automaatselt vaikimisi AzureNode malli abil. Kui kõik sõlmed on esialgu **Ei juurutatud** olekus, vähemalt 3 sõlmed käivitatud. Kui järjekorras olevad tööd arv ületab 8, käivitab skripti sõlmed kuni nende arv ületab suhte **NumOfQueuedJobsPerNodeToGrow**järjekorras olevad tööd. Kui sõlm avastatakse jõude sisse 3 korda järjest jõude, on peatatud.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Näide 2

Järgmises näites konfigureerib Azure MSDN VMs juurutatud kasvada ja vähendage automaatselt vaikimisi ComputeNode malli abil.
Töökoha vaikemalli konfigureeritud töid seostamise töökoormus klaster. Kui kõik sõlmed algselt peatatud vähemalt 5 sõlmed on alustamine. Kui ootel aktiivsed tööülesanded arv ületab 15, käivitab skripti sõlmed kuni nende arv ületab aktiivne ootel tööülesandeid **NumOfActiveQueuedTasksPerNodeToGrow**suhte. Kui sõlm avastatakse jõude sisse 10 korda järjest jõude, on peatatud.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
