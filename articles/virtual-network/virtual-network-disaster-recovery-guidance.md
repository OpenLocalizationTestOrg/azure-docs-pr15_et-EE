<properties
    pageTitle="Mida teha, kui ka Azure teenuse häirete mõjutavad Azure virtuaalse võrgu | Microsoft Azure'i"
    description="Siit saate teada, mida teha, kui ka Azure teenuse häirete mõjutavad Azure virtuaalse võrgu."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Virtuaalse võrgu – järjepidevuse tagamine

##<a name="overview"></a>Ülevaade

Virtuaalse võrgu (VNet) on pilves võrgu loogiline esitus. See võimaldab määratleda oma privaatne IP-aadress ruumi ja võrgu segmendi üheks alamvõrku. VNets toimib usalda piiri majutada oma Arvuta ressursse, nt Azure'i Virtuaalmasinates ja pilveteenused (web/töötaja rollid). Mõne VNet võimaldab otsest privaatne IP suhtlemine ressursse, mis on see majutatud. Virtuaalse võrgu saate lingitud ka kohapealse võrgu kaudu hübriid suvandid, nt VPN-lüüsi või ExpressRoute.
 
Mõne VNet luuakse piirkonnas piires. Saate luua VNets sama aadressiruumi kahes piirkonnas (st meile Ida ja Lääne meile, kuid ei saa neid üksteisele otse). 

##<a name="business-continuity"></a>Süsteemi järjepidevuse tagamine

Põhjusi võib olla mitu võimalust, et rakenduse võib olla katkenud. Piirkonnast võiks ära täielikult lõigata tõttu loodusõnnetuse või mõne osalise tõttu, mitme seadmete ja teenuste tõrke tõttu. Mõju VNet teenust erineb kõigis olukordades.

**Q: mis teha korral on katkestuste kogu piirkonnale? st kui piirkonnas on täielikult äralõikekuupäevast loodusõnnetuse tõttu? Mis juhtub majutatud piirkonna virtuaalse võrkude?**

V: teenuse häirete ajal jääb kättesaamatuks Virtual võrgu ja ressursside piirkonna kohta.

![Lihtne virtuaalse võrgustikudiagramm](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**Q: mis ma uuesti luua virtuaalse samasse võrku teises regioonis?**

V: virtuaalse võrgu (VNet) on üsna kerge ressurss. Saate kasutada Azure API-de loomiseks on VNet sama aadressiruumi teises regioonis. Samas keskkonnas, kus esines piirkonna uuesti loomiseks tuleb uuesti juurutada oma pilveteenustega (web/töötaja rollid) ja Virtuaalmasinates, et teil oli API helistada. Teil on ka tööle hakata VPN-lüüsi ja ühenduse loomiseks kohapealse võrgu kui oleksite kohapealse Ühenduvus (nt hübriidjuurutuse).

Leitakse mõne VNet loomise juhised [siin](./virtual-networks-create-vnet-arm-pportal.md). 

**Q: kas koopia on VNet antud piirkonnas olla uuesti loodud teises regioonis ette valmistada?**

A: Jah, saate luua kaks VNets sama privaatne IP-aadress ruumi ja ressursside kasutamine kahe eri regioonide ette valmistada. Kui kliendi oli hosting teenused on VNet vastastikuste internet, nad on väärtuseks liikluse halduri seadistamine geo-marsruutimiseks liiklus alale, mis on aktiivne. Siiski kliendi ei saa ühendada kaks VNets sama aadressiruumi oma kohapealse võrgu nagu marsruutimise probleeme põhjustab. Loodusõnnetuse ajal ja kadu on VNet ühe piirkonna, kliendi saab ühendada muude VNet saadaval piirkonna sobitamine aadressiruumi oma kohapealse võrku.

Leitakse mõne VNet loomise juhised [siin](./virtual-networks-create-vnet-arm-pportal.md).
