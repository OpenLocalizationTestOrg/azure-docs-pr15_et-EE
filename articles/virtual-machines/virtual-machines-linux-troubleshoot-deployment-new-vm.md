<properties
   pageTitle="Linux VM juurutus-RM tõrkeotsing | Microsoft Azure'i"
   description="Ressursihaldur juurutamise probleemide tõrkeotsing, kui loote uue Linuxi virtuaalse masina Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Ressursihaldur seotud luua uue Linuxi virtuaalse masina Azure juurutamise probleemide tõrkeotsing

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Koguda auditilogid

Tõrkeotsingu alustamiseks koguda auditilogide viga, mis on seotud probleemi tuvastamiseks. Järgmised lingid sisaldavad üksikasjalikku teavet protsessi jälgimiseks.

[Ressursi rühma juurutuste Azure'i portaalis tõrkeotsing](../resource-manager-troubleshoot-deployments-portal.md)

[Auditi toimingud koos ressursihaldur](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** OS on Linux üldiste, kui see on üles laaditud ja/või jäädvustatud generalized säte, siis ei vigu. Samamoodi kui OS on Linux spetsialiseerunud, ja see on üles laaditud ja/või eriotstarbeline säte jäädvustatud, siis ei vigu.

**Tõrgete üleslaadimiseks tehke järgmist.**

**N<sup>1</sup>:** Kui OS on Linux üldiste ja see on üles laaditud, nagu spetsialiseeritud, saate ettevalmistamise ajalõpp tõrge Kuna VM jääb ettevalmistamise etapis.

**N<sup>2</sup>:** Kui OS on Linux spetsialiseerunud ja üles laadida, kuna üldiste, saad ettevalmistamise nurjumise tõrge, kuna uue VM töötab koos algse arvuti nimi, kasutajanimi ja parool.

**Lahendus:**

Nii nende vigade lahendamise kohta üles laadida originaal VHD, saadaval kohapeal, kui sama sätte OS (üldiste/spetsialiseerunud). Üleslaadimiseks üldiste, pidage meeles, et käivitada - deprovision esmalt.

**Jäädvustada vigu:**

**N<sup>3</sup>:** Kui OS on Linux üldiste ja see on kaetud nagu spetsialiseeritud, saate ettevalmistamise ajalõpp tõrge Kuna algse VM ei ole kasutatav, kuna see on märgitud üldiste.

**N<sup>4</sup>:** Kui OS on Linux spetsialiseerunud ja jäädvustata, kui üldiste, saad ettevalmistamise nurjumise tõrge, kuna uue VM töötab koos algse arvuti nimi, kasutajanimi ja parool. Lisaks algse VM ei saa kasutada kuna see on märgitud nii eriotstarbelisi.

**Lahendus:**

Nii nende vigade lahendamise kustutada praeguse pilti portaali ja [selle praeguse VHDs tabamine](virtual-machines-linux-capture-image.md) sama sättega kui OS (üldiste/spetsialiseerunud).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probleem: Kohandatud / Galerii / turuplatsi pilt; eraldatud nurjumise
See tõrge tekib olukordades, kui klaster, mis ei toeta nõutud VM suuruse või ei ole ruumi mahutamiseks taotluse uue VM taotlus on kinnitatud.

**Põhjustada 1:** Klaster ei toeta nõutud VM suurus.

**Lahendus 1:**

- Kasutades VM väiksem kutse uuesti.
- Kui soovitud VM suurust ei saa muuta:
  - Peatage kõik VMs kättesaadavus määramine.
  Klõpsake nuppu **ressurss rühmad** > *Ressursirühma* > **ressursid** > *Kättesaadavusoleku määramine* > **Virtuaalmasinates** > *virtual arvuti* > **lõpetada**.
  - Pärast kõigi VMs peatamiseks luua uue VM soovitud suuruse.
  - Alustage uut VM esmalt ning seejärel iga peatatud VMs ja klõpsake nuppu **Käivita**.

**Põhjus 2:** Klaster ei saa tasuta ressursid.

**Lahendus 2:**

- Proovige hiljem uuesti taotluse.
- Kui uus VM võivad olla erinevad-saadavus osa
  - Luua uue VM erinevate kättesaadavus, määramine (piirkonna).
  - Lisage uus VM virtuaalse samasse võrku.

## <a name="next-steps"></a>Järgmised sammud
Kui peatatud Linux VM käivitamisel või suurust muuta olemasolevaid Linux VM Azure ilmneb probleeme, lugege teemat [tõrkeotsing ressursihaldur juurutamise probleemid taaskäivitada või suurust muuta olemasolevaid Linuxi virtuaalse masina Azure](virtual-machines-linux-restart-resize-error-troubleshooting.md).
