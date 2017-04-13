<properties 
   pageTitle="Juurutamine VM abil Azure portaali sisse ressursihaldur staatilise avaliku IP | Microsoft Azure'i"
   description="Saate teada, kuidas juurutada VMs abil zure portaali sisse ressursihaldur staatilise avaliku IP"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Staatilise avaliku IP Azure'i portaalis VM juurutamine

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Luua VM staatilise avaliku IP 

Staatilise avaliku IP-aadress Azure portaali VM loomiseks järgige alltoodud juhiseid.

1. Brauserist, liikuge [Azure portaali](https://portal.azure.com) ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake vasakus ülanurgas portaali, **Uus**>>**arvutada**>**Windows Server 2012 R2 andmekeskuse**.
3. **Valige juurutamise mudeli** loendis valige **Ressursihaldur** ja klõpsake nuppu **Loo**.
4. **Põhitõed** labale VM teabe sisestamine, nagu allpool näidatud ja seejärel klõpsake nuppu **OK**.

    ![Azure'i portaal - põhialused](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. **Valige suurus** tera, klõpsake käsku **Standardne A1** , nagu allpool näidatud ja **nuppu**.

    ![Azure'i portaal - suuruse valimine](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. Tera **sätted** , klõpsake **avaliku IP-aadress**ja seejärel **Loo avaliku IP-aadressi** labale **ülesande**, klõpsake jaotises **staatilise** nagu allpool näidatud. Ja seejärel klõpsake nuppu **OK**.

    ![Azure'i portaalis – avaliku IP-aadressi loomine](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. Tera **sätted** , klõpsake nuppu **OK**.
8. Vaadake **Kokkuvõte** tera, nagu allpool näidatud ja seejärel klõpsake nuppu **OK**.

    ![Azure'i portaalis – avaliku IP-aadressi loomine](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Pange tähele oma armatuurlauale uue paani.

    ![Azure'i portaalis – avaliku IP-aadressi loomine](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Kui VM on loodud, nagu allpool näidatud tera **sätted** kuvatakse

    ![Azure'i portaalis – avaliku IP-aadressi loomine](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)