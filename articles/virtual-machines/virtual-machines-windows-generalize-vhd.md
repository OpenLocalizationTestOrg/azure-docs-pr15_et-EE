<properties
    pageTitle="Üldistada Windowsiga VHD | Microsoft Azure'i"
    description="Siit saate teada, üldistada Windows VM kasutada mudeliga ressursihaldur juurutamise Sysprep abil."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Üldistada Windowsi virtuaalse masina Sysprepi abil

Selles jaotises kirjeldatakse, kuidas üldistada arvuti Windows virtual pildi kasutamiseks. Sysprep eemaldab kogu teie isikliku konto teave, muu hulgas ja valmistab kohapeal kasutatava pildina. Sysprep kohta leiate üksikasjalikumat teavet leiate teemast [Kuidas kasutada Sysprepi: Sissejuhatus](http://technet.microsoft.com/library/bb457073.aspx).

Veenduge, et arvutis töötab serveri rollid ei toeta Sysprep. Lisateabe saamiseks lugege teemat [Sysprep serverirollide tugi](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Kui teil on Sysprep enne oma VHD üleslaadimine Azure'i esimest korda, veenduge, et olete [valmis oma VM](virtual-machines-windows-prepare-for-upload-vhd-image.md) enne Sysprep käivitamist. 

1. Logige sisse Windowsi virtuaalse masina.

2. Avage käsuviiba aken administraatorina. **%Windir%\system32\sysprep**kataloog muuta, ja seejärel käivitage `sysprep.exe`.

3. **Süsteemi ettevalmistamise tööriista** dialoogiboksis **Sisestage süsteemi Out Box Experience (OOBE)**valige ja veenduge, et **Generalize** ruut oleks märgitud.

4. **Sulgemise suvandid**, valige **sulgumist**.

5. Klõpsake nuppu **OK**.

    ![Sysprep käivitamine](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Kui Sysprep on lõpule jõudnud, see sulgub virtuaalse masina. 

## <a name="next-steps"></a>Järgmised sammud

- Kui VM on asutusesiseselt, saate nüüd [VHD Azure üles laadida](virtual-machines-windows-upload-image.md).
- Kui VM on juba Azure'is, saate nüüd [luua generalized VM pilt](virtual-machines-windows-capture-image.md).