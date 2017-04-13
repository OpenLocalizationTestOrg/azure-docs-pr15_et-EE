<properties
   pageTitle="Kui Azure külaline agent pole installitud kohaliku Windowsi parooli lähtestamine | Microsoft Azure'i"
   description="Kohaliku Windowsi kasutajakonto parooli lähtestamine kui Azure külaline agent pole installitud või toimiva VM kohta"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Kuidas Azure VM kohaliku Windowsi parooli lähtestamine
Saate VM Azure kasutamisel on [Azure portaali või Azure PowerShelli](virtual-machines-windows-reset-rdp.md) esitatud Azure külaline agent installitud kohaliku Windowsi parooli lähtestada. See meetod on esmane võimalus parooli lähtestamine on Azure VM. Kui ilmneda probleemid Azure Külastajate agent ei vasta või ei ole pärast kohandatud pildi üleslaadimise installida, võite käsitsi Windowsi parooli lähtestamine. See artikkel üksikasjad, lisades allika OS virtuaalse ketta teise VM kohaliku konto parooli lähtestamine. 

> [AZURE.WARNING] Kasutage seda toimingut ainult viimasena. Üritage kasutades [Azure portaali või Azure PowerShelli](virtual-machines-windows-reset-rdp.md) esmalt parooli lähtestamine.


## <a name="overview-of-the-process"></a>Protsessi ülevaade
Core juhiseid kohaliku parooli lähtestamine Windows Azure VM, kui ei pääse Azure külaline agent läbimiseks on järgmine:

- Kustutage allika VM. Virtuaalne ketast säilitatakse.
- Andmeallika VM OS kettale lisada teise VM Azure tellimuse. See VM edaspidi tõrkeotsingu VM.
- Mõned config faili andmeallika VM OS kettal tõrkeotsingu VM kasutamisel luua.
- Funktsiooni VM OS ketta tõrkeotsingu VM eemaldada.
- Ressursihaldur malli abil saate luua VM, kasutades algse virtuaalse ketta.
- Kui uus VM saapad, värskendage config failide loomist nõutav kasutaja parooli.


## <a name="detailed-steps"></a>Üksikasjalikud juhised
Proovige alati parooli abil [Azure portaali või Azure PowerShelli](virtual-machines-windows-reset-rdp.md) enne järgmist. Veenduge, et teil on oma VM varukoopia enne alustamist. 

1. Kustutage probleemse VM Azure portaalis. VM kustutamisel kustutatakse ainult metaandmeid, on viide VM Azure'is. Virtuaalne ketast säilitatakse VM kustutamisel:

    - Valige VM Azure portaalis, klõpsake nuppu *Kustuta*:

    ![Olemasoleva VM kustutamine](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. Kinnitage andmeallika VM OS kettale tõrkeotsingu VM. Tõrkeotsingu VM peab olema sama piirkonna allika VM OS ketas (nt `West US`):

    - Valige tõrkeotsingu VM Azure portaali. Klõpsake *ketast* | *manustamine olemasoleva*:

    ![Olemasoleva vaba manustamine](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    Valige *VHD faili* ja seejärel valige salvestusruumi konto, mis sisaldab teie andmeallika VM.

    ![Valige salvestusruumi konto](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Valige allikas ümbris. Source container on tavaliselt *vhds*.

    ![Valige salvestusruumi ümbris](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Valige OS vhd manustamiseks. Klõpsake *Valige* lõpule viia.

    ![Valige allikas virtuaalse ketta](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Ühenduse tõrkeotsingu VM kaugtöölaua kasutamise ja veenduge, et allikas VM OS ketas on näha:

    - Valige tõrkeotsingu VM Azure portaali ja klõpsake nuppu *Loo ühendus*.
    - Avage RDP-faili, mille allalaadimist. Sisestage kasutajanimi ja parool tõrkeotsingu VM.
    - Otsige File Exploreris lisatud andmed kettale. Kui Lähtevaluuta VM VHD on seotud probleemide tõrkeotsinguks ja lahendamiseks VM ainult andmed kettale, peaks olema f ketas:

    ![Vaate manustatud andmed kettale](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Luua `gpt.ini` sisse `\Windows\System32\GroupPolicy` allikas VM kettadraivi (kui on olemas gpt.ini, Nimeta gpt.ini.bak):

    > [AZURE.WARNING] Veenduge, et ei ole kogemata luua järgmised failid C:\Windows OS ketas tõrkeotsingu VM. Luua oma Source VM, mis on lisatud andmed kettale OS ketas järgmised failid.

    - Lisada järgmised read sisse on `gpt.ini` loodud fail:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Gpt.ini loomine](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Looge `scripts.ini` sisse `\Windows\System32\GroupPolicy\Machine\Scripts`. Veenduge, et peidetud kaustade kuvamise viisi. Vajadusel looge selle `Machine` või `Scripts` kaustad.

    - Lisada järgmised read on `scripts.ini` loodud fail:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Scripts.ini loomine](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Looge `FixAzureVM.cmd` sisse `\Windows\System32` järgmised sisuga, asendades `<username>` ja `<newpassword>` oma väärtustega:

    ```
    NET USER <username> <newpassword>
    ```

    ![FixAzureVM.cmd loomine](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    Peate vastama konfigureeritud parooli keerukuse nõuded oma VM määratlemisel uus parool.

7. Azure'i portaalis lahti tõrkeotsingu VM ketas:

    - Valige tõrkeotsingu VM Azure portaalis, klõpsake *ketast*.
    - Valige andmed kettale, millele on manustatud 2. juhise alusel, klõpsake käsku *Eemalda*:

    ![Ketas eemaldamine](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Enne, kui loote VM, hangivad URI allika OS kettale.

    - Valige konto, salvestusruumi Azure'i portaalis, klõpsake *plekid*.
    - Valige ümbris. Source container on tavaliselt *vhds*.

    ![Valige salvestusruumi konto bloobimälu](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Valige allikas VM OS VHD ja *URL-i* nime kõrval nuppu *Kopeeri* .

    ![Kopeeri ketta URI](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Andmeallika VM OS kettalt VM loomiseks tehke järgmist.

    - [Azure'i ressursihaldur selle malli](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) abil saate luua VM eriotstarbelisi VHD kaudu. Klõpsake soovitud `Deploy to Azure` templated üksikasjadega saate täidetud Azure portaali avamiseks nuppu.
    - Kui soovite säilitada kõigi varasemate sätete VM, valige oma olemasoleva VNet alamvõrgu võrguadapteri või avaliku IP *malli redigeerimine* .
    - Klõpsake soovitud `OSDISKVHDURI` parameetri tekstivälja, kleebi allikas VHD URI saada eelmises etapis:

    ![VM loomine malli põhjal](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. Pärast uue VM töötab, ühenduse kaugtöölaua kasutamise määratud uue parooliga VM soovitud `FixAzureVM.cmd` skripti.

11. Kaudu oma Kaugseanss uue VM eemaldamiseks järgmised failid keskkonna puhastamiseks tehke järgmist.

    - %Windir%\System32 kaudu
        - FixAzureVM.cmd eemaldamine
    - %Windir%\System32\GroupPolicy\Machine\ kaudu
        - scripts.ini eemaldamine
    - %Windir%\System32\GroupPolicy kaudu
        - Eemaldage gpt.ini (gpt.ini olemas enne, kui soovite gpt.ini.bak bak-fail tagasi gpt.ini Nimeta ümber)

## <a name="next-steps"></a>Järgmised sammud
Kui te ei saa ühendust kaugtöölaua abil, vt [RDP tõrkeotsingu juhend](virtual-machines-windows-troubleshoot-rdp-connection.md). [Üksikasjalik RDP tõrkeotsingu juhend](virtual-machines-windows-detailed-troubleshoot-rdp.md) vaatab tõrkeotsingu meetodid, mitte teatud juhiseid. Samuti saate [avada Azure'i tugiteenuse taotluse](https://azure.microsoft.com/support/options/) praktilise abi.