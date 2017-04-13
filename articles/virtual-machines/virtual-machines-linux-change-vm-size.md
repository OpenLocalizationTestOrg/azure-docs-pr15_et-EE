<properties
   pageTitle="Suuruse muutmise Linux VM | Microsoft Azure'i"
   description="Kuidas skaalal või mastaapimiseks Linux virtuaalse masina, alla, muutes VM suurus."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Suuruse muutmise Linux VM

## <a name="overview"></a>Ülevaade 

Kui olete ette virtuaalse masina (VM), saate muudate VM üles või alla, muutes [VM suurus][vm-sizes]. Mõnel juhul peab deallocate VM esmalt. See võib juhtuda siis, kui uue suuruse pole saadaval, mis majutab VM riistvara klaster.

Selles artiklis kirjeldatakse suuruse muutmise abil [Azure'i CLI]Linux VM[azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel.


## <a name="resize-a-linux-vm"></a>Linux VM suuruse muutmine 

VM suuruse muutmiseks tehke järgmist.

1. Käivitage järgmine käsk CLI. See käsk on loetletud VM suurused, mis on saadaval, kus VM majutatakse riistvara klaster.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Kui loendis on soovitud suuruse, käivitage järgmine käsk suuruse VM.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    Selle toimingu käigus uuesti VM. Pärast uuesti, kas teie olemasoleva OS ja andmete ketast aadressirida. Midagi ajutine kettal lähevad kaotsi.

    Kasutage funktsiooni `--enable-boot-diagnostics` valik võimaldab [buutimine diagnostika][boot-diagnostics], logige käivitus seotud vigu.

3. Muul juhul, kui soovitud suurus pole loendis, käivitage järgmised käsud Deallocate VM, selle suurust muuta, ja seejärel taaskäivitage VM.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Deallocating VM ka välja mis tahes dünaamiline IP-aadresside määratud VM. Ketast OS- ja see ei mõjuta.
   
## <a name="next-steps"></a>Järgmised sammud

Täiendavad skaleeritavus, käivitage VM mitmes eksemplaris ja mastaapimiseks välja. Lisateabe saamiseks lugege teemat [automaatselt mastaapida Linux masinad virtuaalse masina skaala seadmine][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md