<properties
   pageTitle="VM taaskäivitada või suuruse muutmise probleemid | Microsoft Azure'i"
   description="Klassikaline juurutamise taaskäivitada või suurust muuta olemasolevaid Windows virtuaalse masina Azure seotud probleemide tõrkeotsing"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-windows"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Klassikaline juurutamise taaskäivitada või suurust muuta olemasolevaid Windows virtuaalse masina Azure seotud probleemide tõrkeotsing

> [AZURE.SELECTOR]
- [Klassikaline](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
- [Ressursihaldur](../../virtual-machines-windows-restart-resize-error-troubleshooting.md)

Käivitage peatatud Azure virtuaalse masina (VM) või suurust muuta mõne olemasoleva Azure VM katsel on levinud viga ilmneda tõrge eraldatud. Selle tõrke tulemusi, kui kobar või regioonis ei ole ressursse, mis on saadaval või ei toeta nõutud VM suurus.
> [AZURE.IMPORTANT] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [ressursihaldur ja klassikaline](../../../resource-manager-deployment-model.md).  Selles artiklis antakse ülevaade klassikaline juurutamise mudeli abil. Microsoft soovitab, et kõige uue juurutuste ressursihaldur mudeli kasutamine.

[AZURE.INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Koguda auditilogid

Tõrkeotsingu alustamiseks koguda auditilogide viga, mis on seotud probleemi tuvastamiseks.

Azure'i portaalis, klõpsake nuppu **Sirvi** > **virtuaalmasinates** > _arvuti Windows virtual_ > **sätted** > **auditilogid**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Viga peatatud VM käivitamisel

Proovite peatatud VM alustada, kuid saate tõrke eraldatud.

### <a name="cause"></a>Põhjus

Peatatud VM alustamiseks taotluse peab olema proovitakse veebisaidil algse kobar, mis hostib pilveteenusesse. Klaster ei ole taotluse vaba ruumi.

### <a name="resolution"></a>Eraldusvõime

* Looge uus pilveteenuses ja seostada ühte piirkonnas või regioonis asuva virtuaalse võrgus, kuid mitte osaleja rühma.

* Peatatud VM kustutada.

* Looge uus pilveteenuses VM, kasutades funktsiooni ketast.

* Käivitage uuesti loodud VM.

Kui teil tekib tõrge, kui proovite luua uue pilveteenuses, proovige hiljem uuesti või pilveteenusesse-piirkonna muutmine.

> [AZURE.IMPORTANT] Uue pilveteenuses on uus nimi ja VIP, seega tuleb muuta selle teabe sõltuvused, mis kasutavad seda teavet olemasoleva pilveteenuses.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Tõrge, kui mõne olemasoleva VM suuruse muutmine

Proovite mõne olemasoleva VM suurust muuta, kuid saate tõrke eraldatud.

### <a name="cause"></a>Põhjus

Taotluse VM suuruse muutmiseks peab olema proovitakse veebisaidil algse kobar, mis hostib pilveteenusesse. Siiski ei toeta klaster VM soovitud suuruse.

### <a name="resolution"></a>Eraldusvõime

Nõutud VM mahu vähendamiseks ja proovige suuruse muutmise taotluse.

* Klõpsake nuppu **Sirvi kõiki** > **virtuaalmasinates (klassikaline)** > _arvuti virtual_ > **sätted** > **suurus**. Üksikasjalikud juhised leiate teemast [virtuaalse masina suurust](https://msdn.microsoft.com/library/dn168976.aspx).

Kui see pole võimalik VM mahu vähendamiseks, toimige järgmiselt.

  * Saate luua uue pilveteenuses tagada selle osaleja rühma ei lingitud ja pole seotud virtuaalse võrk, mis on lingitud mõne osaleja rühma.

  * Saate luua uue, suurema VM see.

Saate konsolideerida kõik teie VMs sama pilveteenuses. Kui teie olemasoleva pilveteenuses on seostatud regioonis asuva virtuaalse võrgu, saate uute pilveteenuses ühenduse olemasoleva virtuaalse võrgu.

Kui olemasolev pilveteenuses pole seostatud regioonis asuva virtuaalse võrgu, siis on teil VMs olemasoleva pilveteenuses kustutada, ja neid uuesti kaudu nende uue pilveteenuses. Siiski on oluline meeles pidada, et uue pilveteenuses on uus nimi ja VIP, nii, et peate värskenda sõltuvuste, mida praegu kasutada seda teavet olemasoleva pilveteenuses.

## <a name="next-steps"></a>Järgmised sammud

Kui loote uue VM Windows Azure'i tekib probleeme, lugege teemat [tõrkeotsing juurutamise abil luua uue Windowsi virtuaalse masina Azure](../../virtual-machines-windows-troubleshoot-deployment-new-vm.md).
