<properties
   pageTitle="VM taaskäivitamine või suuruse muutmise probleemid | Microsoft Azure'i"
   description="Ressursihaldur juurutamise seotud taaskäivitada või suurust muuta olemasolevaid Linuxi virtuaalse masina Azure probleemide tõrkeotsing"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Ressursihaldur juurutamise seotud taaskäivitada või suurust muuta olemasolevaid Linuxi virtuaalse masina Azure probleemide tõrkeotsing

Käivitage peatatud Azure virtuaalse masina (VM) või suurust muuta mõne olemasoleva Azure VM katsel on levinud viga ilmneda tõrge eraldatud. Selle tõrke tulemusi, kui kobar või regioonis ei ole ressursse, mis on saadaval või ei toeta nõutud VM suurus.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Koguda auditilogid

Tõrkeotsingu alustamiseks koguda auditilogide viga, mis on seotud probleemi tuvastamiseks. Järgmised lingid sisaldab üksikasjalikku teavet protsessi:

[Ressursi rühma juurutuste Azure'i portaalis tõrkeotsing](../resource-manager-troubleshoot-deployments-portal.md)

[Auditi toimingud koos ressursihaldur](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Viga peatatud VM käivitamisel

Proovite peatatud VM alustada, kuid saate tõrke eraldatud.

### <a name="cause"></a>Põhjus

Peatatud VM alustamiseks taotluse peab olema proovitakse veebisaidil algse kobar, mis hostib pilveteenusesse. Klaster ei ole taotluse vaba ruumi.

### <a name="resolution"></a>Eraldusvõime

*   Kõik VMs kättesaadavus määramine peatada ja seejärel taaskäivitage iga VM.

  1. Klõpsake nuppu **ressurss rühmad** > _Ressursirühma_ > **ressursid** > _Kättesaadavusoleku määramine_ > **Virtuaalmasinates** > _virtual arvuti_ > **lõpetada**.

  2. Pärast kõigi VMs lõpetada, valige iga peatatud VMs ja klõpsake nuppu Start.

*   Proovige hiljem uuesti taotluse.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Tõrge, kui mõne olemasoleva VM suuruse muutmine

Proovite mõne olemasoleva VM suurust muuta, kuid saate tõrke eraldatud.

### <a name="cause"></a>Põhjus

Taotluse VM suuruse muutmiseks peab olema proovitakse veebisaidil algse kobar, mis hostib pilveteenusesse. Siiski ei toeta klaster VM soovitud suuruse.

### <a name="resolution"></a>Eraldusvõime

* Kasutades VM väiksem kutse uuesti.

* Kui soovitud VM suurust ei saa muuta:

  1. Peatage kõik VMs kättesaadavus määramine.

    * Klõpsake nuppu **ressurss rühmad** > _Ressursirühma_ > **ressursid** > _Kättesaadavusoleku määramine_ > **Virtuaalmasinates** > _virtual arvuti_ > **lõpetada**.

  2. Pärast kõigi VMs peatamiseks suuruse muutmine suuremaks soovitud VM.
  3. Valige muudetud suurusega VM ja klõpsake nuppu **Start**ja alustage iga peatatud VMs.

## <a name="next-steps"></a>Järgmised sammud

Kui loote uue Linux VM Azure tekib probleeme, lugege teemat [tõrkeotsing juurutamise abil luua uue Linuxi virtuaalse masina Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
