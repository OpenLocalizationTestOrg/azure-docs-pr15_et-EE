<properties
   pageTitle="Tõrkeotsing: Windows VM juurutus-RM | Microsoft Azure'i"
   description="Ressursihaldur juurutamise probleemide tõrkeotsing, kui loote uue virtuaalse masina Windows Azure"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Ressursihaldur juurutamise seotud loomine uue virtuaalse masina Windows Azure'i probleemide tõrkeotsing

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Koguda auditilogid

Tõrkeotsingu alustamiseks koguda auditilogide viga, mis on seotud probleemi tuvastamiseks. Järgmised lingid sisaldavad üksikasjalikku teavet protsessi jälgimiseks.

[Ressursi rühma juurutuste Azure'i portaalis tõrkeotsing](../resource-manager-troubleshoot-deployments-portal.md)

[Auditi toimingud koos ressursihaldur](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** OS on üldiste Windows, kui see on üles laaditud ja/või jäädvustatud generalized säte, siis ei vigu. Samamoodi kui OS on Windows spetsialiseerunud, ja see on üles laaditud ja/või eriotstarbeline säte jäädvustatud, siis ei vigu.

**Tõrgete üleslaadimiseks tehke järgmist.**

**N<sup>1</sup>:** Kui OS on Windowsi üldiste ja see on üles laaditud, nagu spetsialiseeritud, saate ettevalmistamise ajalõpp tõrge OOBE ekraanil kinni VM abil.

**N<sup>2</sup>:** Kui OS on Windows spetsialiseerunud ja üles laadida, kuna üldiste, saad ettevalmistamise nurjumise tõrge VM kinni OOBE ekraanile, kuna uue VM töötab koos algse arvuti nimi, kasutajanimi ja parool.

**Eraldusvõime**

Nii nende vigade lahendamise kasutada [Lisa-AzureRmVhd üles laadida originaal VHD](https://msdn.microsoft.com/library/mt603554.aspx), asutusesiseselt saadaval, sama sätet kui OS (üldiste/spetsialiseerunud). Üleslaadimiseks üldiste, ärge unustage sysprep esmalt käivitada.

**Jäädvustada vigu:**

**N<sup>3</sup>:** Kui OS on Windowsi üldiste ja ei jäädvustata, kui spetsialiseerunud, saate ettevalmistamise ajalõpp tõrge Kuna algse VM ei ole kasutatav, kuna see on märgitud üldiste.

**N<sup>4</sup>:** Kui OS on Windows spetsialiseerunud ja jäädvustata, kui üldiste, saad ettevalmistamise nurjumise tõrge, kuna uue VM töötab koos algse arvuti nimi, kasutajanimi ja parool. Lisaks algse VM ei saa kasutada kuna see on märgitud nii eriotstarbelisi.

**Eraldusvõime**

Nii nende vigade lahendamise kustutada praeguse pilti portaali ja [selle praeguse VHDs tabamine](virtual-machines-windows-vhd-copy.md) sama sättega kui OS (üldiste/spetsialiseerunud).

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Probleem: Kohandatud/Galerii/turuplatsi pilt; jaotamine nurjus
See tõrge tekib olukordades, kui uue VM taotlus on kinnitatud klaster, mis ei toeta nõutud VM suuruse või ei saa majutada taotluse vaba ruumi.

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
- Kui uus VM võib olla osa eri-saadavus
  - Luua uue VM erinevate kättesaadavus, määramine (piirkonna).
  - Lisage uus VM virtuaalse samasse võrku.

## <a name="next-steps"></a>Järgmised sammud
Kui teil tekib probleeme peatatud Windows VM käivitamisel või olemasoleva Windows VM Azure suuruse, lugege teemat [tõrkeotsing ressursihaldur juurutamise probleemid taaskäivitada või olemasoleva Windowsi virtuaalse masina Azure suuruse muutmine](virtual-machines-windows-restart-resize-error-troubleshooting.md).
