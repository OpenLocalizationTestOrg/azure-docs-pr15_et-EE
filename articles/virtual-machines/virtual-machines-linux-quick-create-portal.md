<properties
    pageTitle="Linux VM Azure portaali loomine | Microsoft Azure'i"
    description="Linux VM Azure portaali loomine."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Linux VM Azure portaali loomine


Selles artiklis kirjeldatakse, kuidas [Azure portaali](https://portal.azure.com/) abil saate luua virtuaalse masina Linux.

Nõuded on:

- [Azure'i konto](https://azure.microsoft.com/pricing/free-trial/)

- [SSH avalike ja privaatvõ key failid](virtual-machines-linux-mac-create-ssh-keys.md)


1. Oma identiteedi Azure'i konto Azure portaali sisse logitud, klõpsake **+ Uus** ülemises vasakus nurgas.

    ![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. **Turuplatsi** **Virtuaalmasinates** klõpsake **Ubuntu Server 14.04 LTS** **Esiletõstetud rakendused** pilte loend.  Kontrollige allosas juurutamise mudeli on `Resource Manager` ja seejärel klõpsake nuppu **Loo**.

    ![screen2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Sisestage lehel **põhialused** :
    - VM nimi
    - Administraator kasutaja kasutajanimi
    - Authentication Type määratud **SSH avalik võti**
    - SSH avalik võti stringina (kaudu oma `~/.ssh/` kataloog)
    - ressursi rühma nimi või olemasoleva rühma valimine

    ja klõpsake nuppu **OK** ja valige VM suurus; See peaks välja nägema umbes järgmine:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Valige **DS1** suurus, mis Premium SSD Ubuntu installid ja klõpsake nuppu, **Valige** sätete konfigureerimine.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. **Sätted**, jätke salvestus- ja võrgu väärtuste jaoks vaikesätteid ja klõpsake nuppu **OK** kokkuvõtte kuvamiseks.  Teate ketta tüüp on seatud Premium SSD, valides DS1, **S** notates SSD.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Kinnitage oma uus Ubuntu VM sätted ja klõpsake nuppu **OK**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Avage portaali armatuurlaud ja valige oma NIC **võrgu liidesed**

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Klõpsake jaotises sätted NIC avaliku IP aadressid menüü avamine

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH üheks avaliku IP, kasutades oma SSH avalik võti

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete loonud Linux VM kiiresti kasutama testimine või tutvustamise eesmärgil. Linux VM loomiseks kohandada oma taristu, tehke ühte järgmistest artiklitest.

- [Luua Linuxi VM Azure mallide kasutamine](virtual-machines-linux-cli-deploy-templates.md)
- [Luua mõne SSH turvatud Linuxi VM Azure mallide kasutamine](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Luua Linuxi VM Azure CLI abil](virtual-machines-linux-create-cli-complete.md)
