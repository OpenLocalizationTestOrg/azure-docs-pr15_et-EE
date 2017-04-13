<properties
   pageTitle="Linux VM juurutamise – klassikaline tõrkeotsing | Microsoft Azure'i"
   description="Kui loote uue Linuxi virtuaalse masina Azure klassikaline juurutamise tõrkeotsing"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Klassikaline juurutamise seotud luua uue Linuxi virtuaalse masina Azure probleemide tõrkeotsing

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Koguda auditilogid

Tõrkeotsingu alustamiseks koguda auditilogide viga, mis on seotud probleemi tuvastamiseks.

Azure'i portaalis, klõpsake nuppu **Sirvi** > **virtuaalmasinates** > *arvuti Windows virtual* > **sätted** > **auditilogid**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** OS on Linux üldiste, kui see on üles laaditud ja/või jäädvustatud generalized säte, siis ei vigu. Samamoodi kui OS on Linux spetsialiseerunud, ja see on üles laaditud ja/või eriotstarbeline säte jäädvustatud, siis ei vigu.

**Tõrgete üleslaadimiseks tehke järgmist.**

**N<sup>1</sup>:** Kui OS on Linux üldiste ja see on üles laaditud, nagu spetsialiseeritud, saate ettevalmistamise ajalõpp tõrge Kuna VM jääb ettevalmistamise etapis.

**N<sup>2</sup>:** Kui OS on Linux spetsialiseerunud ja üles laadida, kuna üldiste, saad ettevalmistamise nurjumise tõrge, kuna uue VM töötab koos algse arvuti nimi, kasutajanimi ja parool.

**Lahendus:**

Nii nende vigade lahendamise kohta üles laadida originaal VHD, saadaval kohapeal, kui sama sätte OS (üldiste/spetsialiseerunud). Üleslaadimiseks üldiste, pidage meeles, et käivitada - deprovision esmalt. Lisateabe saamiseks vaadake [luua ja üles laadida virtuaalse kõvaketta, mis sisaldab Linux operatsioonisüsteemi](virtual-machines-linux-classic-create-upload-vhd.md) .

**Jäädvustada vigu:**

**N<sup>3</sup>:** Kui OS on Linux üldiste ja see on kaetud nagu spetsialiseeritud, saate ettevalmistamise ajalõpp tõrge Kuna algse VM ei ole kasutatav, kuna see on märgitud üldiste.

**N<sup>4</sup>:** Kui OS on Linux spetsialiseerunud ja jäädvustata, kui üldiste, saad ettevalmistamise nurjumise tõrge, kuna uue VM töötab koos algse arvuti nimi, kasutajanimi ja parool. Lisaks algse VM ei saa kasutada kuna see on märgitud nii eriotstarbelisi.

**Lahendus:**

Nii nende vigade lahendamise kustutada praeguse pilti portaali ja [selle praeguse VHDs tabamine](virtual-machines-linux-classic-capture-image.md) sama sättega kui OS (üldiste/spetsialiseerunud).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probleem: Kohandatud / Galerii / turuplatsi pilt; jaotamine nurjus
See tõrge tekib olukordades, kui uus VM koosolekukutse saadetakse klaster, mis ei ole ruumi mahutamiseks taotluse või ei toeta nõutud VM suuruse. Ei ole võimalik erinevast VMs sama pilveteenuses. Et kui soovite luua uue VM, kui teie pilveteenuses toetab erineva suurusega, Arvuta taotluse nurjub.

Piiranguid pilvepõhise teenuse abil saate luua uue VM, sõltuvalt võivad ilmneda kahel järgmisel juhul põhjustada tõrke.

**Põhjustada 1:** Teatud arvutikobaras on kinnitatud pilveteenusesse või see on lingitud mõne osaleja rühma ja kujundus, seega teatud arvutikobaras kinnitatud. Nii uus Arvuta ressursi taotleb, et rühma osaleja on sama kobar, kus olemasolevad ressursid on majutatud õpetuses. Siiski sama kobar võib ei toeta nõutud VM suuruse või on piisavalt vaba ruumi, tulemuseks vea eraldatud. See kehtib, kas uue ressursid on loodud uue pilveteenuses või olemasoleva pilvepõhise teenuse kaudu.

**Lahendus 1:**

- Luua uue pilveteenuses ja piirkonnas või regioonis asuva virtuaalse võrgu seostada.
- Saate luua uue VM uue pilveteenuses.
  Kui teil tekib tõrge, kui proovite luua uue pilveteenuses, proovige hiljem uuesti või pilveteenusesse-piirkonna muutmine.

> [AZURE.IMPORTANT] Kui proovisite luua uue VM olemasoleva pilveteenuses, kuid ei saanud ja oli luua uue pilveteenuses oma uue VM jaoks, saate koondada kõik teie VMs sama pilveteenuses. Selleks kustutage olemasolev pilveteenuses VMs ja tabamine nende kaudu oma ketast uue pilveteenuses. Siiski on oluline meeles pidada, et uue pilveteenuses on uus nimi ja VIP, nii, et peate värskenda sõltuvuste, mida praegu kasutada seda teavet olemasoleva pilveteenuses.

**Põhjus 2:** Pilveteenusesse on seostatud virtuaalse võrk, mis on lingitud mõne osaleja jaotises nii, et seda on kinnitatud teatud kobar kujundus. Kõigi uute Arvuta ressursi taotleb, et rühma osaleja on seega proovinud sama kobar, kus olemasolevad ressursid on majutatud. Siiski sama kobar võib ei toeta nõutud VM suuruse või on piisavalt vaba ruumi, tulemuseks vea eraldatud. See kehtib, kas uue ressursid on loodud uue pilveteenuses või olemasoleva pilvepõhise teenuse kaudu.

**Lahendus 2:**

- Saate luua uue piirkondliku virtuaalse võrgu.
- Uue VM virtuaalse uue võrgu loomine
- [Ühenduse loomine olemasoleva virtuaalse võrgu](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) uue virtuaalse võrku. Vt lisateavet [virtuaalse piirkondliku võrke](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Teise võimalusena võite [rühma osaleja põhise virtuaalse võrgu piirkondliku virtuaalse võrku migreerida](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), ja seejärel looge uus VM.

## <a name="next-steps"></a>Järgmised sammud
Kui teil tekib probleeme peatatud Linux VM käivitamisel või olemasoleva Linux VM Azure suurust, lugege teemat [tõrkeotsing klassikaline juurutamise taaskäivitada või olemasoleva Linux virtuaalse masina Azure suurust](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md).
