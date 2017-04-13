<properties
 pageTitle="Luua mõne HPC Pack pea sõlme on Azure VM | Microsoft Azure'i"
 description="Saate teada, kuidas luua Microsoft HPC Pack pea sõlme on Azure VM Azure portaali ja ressursihaldur juurutamise mudeli abil."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Luua mõne HPC Pack kobar pea sõlme on Azure VM pildiga turuplats


[Microsoft HPC Pack virtuaalse masina pilt](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) : Azure'i turuplatsi ja Azure portaali abil saate luua ka HPC kobar pea sõlme. Sellel pildil HPC Pack VM põhineb Windows Server 2012 R2 andmekeskuse HPC Pack 2012 R2 Update 3 eelinstallitud. Kasutage seda pea sõlme tõendada mõistet juurutuse HPC Pack Azure. Seejärel saate lisada Arvuta sõlmed klaster käivitamiseks HPC töökoormus.



>[AZURE.TIP]Täieliku HPC Pack kobar Azure, mis sisaldab pea sõlme ja Arvuta sõlmed juurutada, soovitame kasutada automaatset meetodit. Suvandite hulka kuuluvad [HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) ja [HPC Pack kobar Windows töökoormus](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) ressursihaldur malli. Teemast [Azure HPC Pack kobar suvandid](virtual-machines-windows-hpcpack-cluster-options.md) täiendavaid malle. 


## <a name="planning-considerations"></a>Plaanimist

Nagu on näidatud järgmisel joonisel, juurutamist HPC Pack pea sõlme virtuaalse võrgu Azure Active Directory domeenis.

![HPC Pack pea sõlme][headnode]

* **Active Directory domeeni** - The HPC Pack pea sõlme on ühendatud Azure Active Directory domeeni enne alustamist HPC teenuste VM. Nagu on näidatud selles artiklis on tõendada mõistet juurutus, võite edendada saate luua pea sõlme domeenikontrolleri enne HPC teenuste VM. Teine võimalus on juurutada eraldi domeenikontrolleri ja mets Azure, millele ei pea sõlme VM liituda.

* **Azure virtuaalse võrgu** - kui ressursihaldur juurutamise mudeli abil saate juurutada pea sõlme, määrake või Azure virtuaalse luua. Virtuaalse võrgu kasutada siis, kui peate olemasoleva Active Directory domeeni pea sõlme ühendamiseks. Samuti vajate seda hiljem lisada MSDN VMs klaster.

    
## <a name="steps-to-create-the-head-node"></a>Juhised pea sõlme loomiseks

Järgnevalt on üksikasjalik juhiseid Azure portaali abil saate luua ka Azure VM pea sõlme HPC Pack for ressursihaldur juurutamise mudeli abil. 


1. Kui soovite luua uue Active Directory kogumis Azure eraldi domeenikontrolleri VMs, üheks võimaluseks on kasutada [ressursihaldur malli](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Mõne lihtsa tõendada mõistet juurutus, pole midagi, jätke see toiming ja domeenikontrolleri pea sõlme VM ise konfigureerida. See suvand on kirjeldatud selle artikli.
    
2. Klõpsake [HPC Pack 2012 R2 Windows Server 2012 R2 lehel](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) Azure'i turuplatsi, **virtuaalse masina loomine**. 

3. Portaalis lehel **HPC Pack 2012 R2 Windows Server 2012 R2** valige **Ressursihaldur** juurutamise mudel ja seejärel klõpsake nuppu **Loo**.

    ![HPC Pack pilt][marketplace]

4. Portaali abil saate konfigureerida ja luua VM. Kui olete kasutanud Azure, järgige õpetuse [virtuaalse masina Windows Azure portaali loomine](virtual-machines-windows-hero-tutorial.md). Mõne tõendada mõistet juurutus, saate tavaliselt nõustuge vaikevalikuga või Soovitatavad sätted.

    >[AZURE.NOTE]Kui soovite liituda pea sõlme olemasoleva Azure Active Directory domeeni, veenduge, et teie määratud domeeni virtuaalse võrgu VM loomisel.
       
4. Pärast loomist VM ja VM töötab kaugtöölaua [ühenduse VM](virtual-machines-windows-connect-logon.md) . 

5. Liituda mõne olemasoleva domeeni mets VM või looge domeeni mets ise VM.

    * Kui olete loonud VM Azure virtuaalse võrgu koos mõne olemasoleva domeeni mets, liituda VM mets standard Server Manager või Windows PowerShelli tööriistade abil. Taaskäivitage.

    * Kui olete loonud VM uue virtuaalse võrku (ilma olemasoleva domeeni mets), seejärel edendada VM domeenikontrolleri. Standardse juhiste abil saate installida ja konfigureerida Active Directory domeeniteenused roll pea sõlme. Üksikasjalikud juhised leiate teemast [uue Windows Server 2012 Active Directory mets installida](https://technet.microsoft.com/library/jj574166.aspx).

5. Kui VM töötab ja on Active Directory kogumis on ühendatud, käivitage HPC Pack teenuste järgmiselt.

    lisamine. Ühenduse pea sõlme VM domeeni kontoga, mis on kohalike administraatorite rühma liige. Näiteks kasutada administraatori konto häälestamist, kui olete loonud pea sõlme VM.

    b. Vaikimisi pea sõlme paigutus, käivitage Windows PowerShelli administraatorina ja tippige järgmine käsk:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Võib kuluda mitu minutit HPC Pack teenuste käivitamiseks.

    Täiendavad pea sõlme konfiguratsiooni suvandeid, tippige `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Järgmised sammud

* Nüüd saate töötada klaster HPC Pack pea sõlme. Näiteks HPC halduri käivitamine ja täielik [Juurutamise Ülesandeloend](https://technet.microsoft.com/library/jj884141.aspx).
* Kui soovite suurendada klaster Arvutage võimsus tellitavate, lisada [Azure lõhkemist sõlmed](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) mõnda pilveteenusesse. 

* Proovige käivitada klaster testi töökoormus. Näiteks vt HPC Pack [alustamisjuhendit](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
