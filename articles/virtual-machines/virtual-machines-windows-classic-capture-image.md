<properties
    pageTitle="Pildi mõnda Windows Azure'i VM | Microsoft Azure'i"
    description="Windows Azure'i virtuaalse masina loodud mudeliga klassikaline juurutamise kuvatõmmist."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Windows Azure'i virtuaalse masina loodud mudeliga klassikaline juurutamise kuvatõmmist.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressursihaldur mudeli leiate teemast [Loo koopia Windows VM töötab Azure](virtual-machines-windows-vhd-copy.md).


Selles artiklis kirjeldatakse, kuidas jäädvustada abil saate selle pildina muude virtuaalmasinates loomiseks, kus töötab Windows Azure'i virtuaalarvuti. Sellel pildil sisaldab operatsioonisüsteemi ketta ja mis tahes andmete ketast, mis on lisatud virtuaalse masina. See ei sisalda võrgu konfiguratsioone, seega peate need konfigureerimine muude virtuaalmasinates, mis kasutavad pildi loomisel.

Azure'i talletab jaotises **Minu pildid**pilt. See on samas kohas, kus talletatakse üleslaadimist pilte. Lugege lisateavet piltide [virtuaalmasinates pildid](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Enne alustamist##

Nende juhiste korral eeldatakse, et olete juba loonud mõne Azure virtuaalse masina ja konfigureeritud operatsioonisüsteemi, sh manustamise mis tahes andmete ketast. Kui te pole seda veel teinud, vaadake neid juhiseid:

- [Luua virtuaalse masina pilt](virtual-machines-windows-classic-createportal.md)
- [Kuidas lisada andmed kettale virtuaalse masina](virtual-machines-windows-classic-attach-disk.md)
- Veenduge, et serveri rollid on toetatud Sysprep. Lisateabe saamiseks vt [Sysprep serverirollide tugi](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.WARNING] Selle protsessi kustutab algse virtuaalse masina pärast selle tegemist. 

Enne caputuring pilt on Azure virtuaalse masina, on soovitatav target virtuaalse masina varundada. Azure'i virtuaalmasinates saab varundada Azure'i varundamise. Lisateavet leiate teemast [Azure'i virtuaalmasinates varundada](../backup/backup-azure-vms.md). Sertifitseeritud partnerite saadaolevaid muid lahendusi. Kui soovite teada saada, mis on praegu saadaval, otsida Azure'i turuplatsi.


##<a name="capture-the-virtual-machine"></a>Jäädvustada virtuaalse masina

1. [Azure'i klassikaline portaali](http://manage.windowsazure.com)virtuaalse masina ja **ühendamine** . Juhiste saamiseks vaadake, [Kuidas sisse logida Windows Server töötab] [].

2.  Avage käsuviiba aken administraatorina.

3.  Muuta kataloogi `%windir%\system32\sysprep`, ja seejärel käivitage sysprep.exe.

4.  Kuvatakse dialoogiboks **Süsteemi ettevalmistamise tööriista** . Tehke järgmist.

    - **Süsteemi Cleanup toimingu**, valige **süsteem sisestage Out Box Experience (OOBE)** ja veenduge, et märgitud on **Generalize** . Sysprep kasutamise kohta leiate lisateavet teemast [Kuidas kasutada Sysprepi: Sissejuhatus][].

    - **Sulgemise suvandid**, valige **sulgumist**.

    - Klõpsake nuppu **OK**.

    ![Sysprep käivitamine](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep sulgub virtuaalse masina, mis muutub virtuaalse masina Azure klassikaline portaalis olek **on peatatud**.

8.  Azure'i klassikaline portaalis, klõpsake **Virtuaalmasinates** ja valige virtuaalse masina, mida soovite hõivata.

9.  Klõpsake käsuriba, **jäädvustada**.

    ![Jäädvustada virtuaalse masina](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    Kuvatakse dialoogiboks **jäädvustada virtuaalse masina** .

10. **Pildi nime**, tippige uus pilt.

11. Enne Windows Server pildi lisada oma kohandatud pilte kogum, peab olema üldiste käivitades Sysprep eespool kirjeldatud juhistele. Valige **mul on käivitada Sysprep virtual arvutisse** näitamaks, mille tegite.

12. Klõpsake märkeruutu jäädvustada pilt. Uus pilt on nüüd saadaval jaotises **pilte**.

    ![Pildi jäädvustada eduka](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Järgmised sammud

Pilt on valmis virtuaalmasinates loomiseks kasutada. Selleks saate luua virtuaalse masina abil **Galeriist** menüükäsk ja valige äsja loodud pilti. Juhised leiate teemast [loomine virtuaalse masina pilt](virtual-machines-windows-classic-createportal.md).



[Kuidas töötab Windows Server sisse logida]: virtual-machines-windows-classic-connect-logon.md
[Kuidas kasutada Sysprepi: Sissejuhatus]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
