<properties
    pageTitle="Networking taristu juhised | Microsoft Azure'i"
    description="Teavet ja rakendamist suuniseid virtuaalse võrgunduse Azure taristu teenused kasutamise kohta."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Võrgunduse taristu juhised

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

See artikkel keskendub nõutud kavandamise toimingute Azure'is virtuaalse võrgunduse ja ühenduvuse olemasoleva kohapeal keskkondades mõistmine.


## <a name="implementation-guidelines-for-virtual-networks"></a>Virtuaalne võrkude rakendamise juhised

Otsuseid:

- Virtuaalne võrgust on vaja majutada oma IT töökoormus või taristu (vaid või asutusesiseses)?
- Asutusesiseses virtuaalse võrke, kui palju ruumi aadress vajate majutada alamvõrku ja VMs nüüd ja mõistlikum laiendamiseks tulevikus?
- Kas sa looma tsentraliseeritud virtuaalse võrgu või üksikute virtuaalse võrgu iga ressursirühma?

Ülesanded:

- Määratleda aadressiruumi virtuaalse võrkude luua.
- Määratleda iga alamvõrku ja aadress ruumi määramine.
- Asutusesiseses virtuaalne võrkude, määratleda määramine kohtvõrgu aadress pidamiseks VMs virtuaalse võrgu vaja jõuda kohapealse asukohad.
- Töö asutusesiseste võrgu meeskonnatöö tagada vastav marsruutimine on konfigureeritud kui loomine asutusesiseses virtuaalse võrgu.
- Saate luua virtuaalse võrgu kaudu oma nimereeglistikku.


## <a name="virtual-networks"></a>Virtuaalne võrkude

Virtuaalne võrkude on vaja virtuaalmasinates (VM) vahelise suhtluse tugi. Saate alamvõrku, kohandatud IP-aadressi, DNS-i sätted, Turve, filtreerimine ja laadimine tasakaalustamiseks, sarnaselt füüsilise võrgustikke. [VPN-lüüsi](../vpn-gateway/vpn-gateway-about-vpngateways.md) või [teekonna ringi](../expressroute/expressroute-introduction.md)abil saate luua ühenduse Azure'i kohapealse võrguga. Lugege lisateavet [virtuaalse võrgu ja nende komponentide](../virtual-network/virtual-networks-overview.md)kohta.

Ressursi rühmade abil on teil paindlikkust kujundamise oma virtuaalse võrgu komponendid. VMs saate luua ühenduse virtuaalse võrgu väljaspool oma ressursirühma. Ühtne kujundus oleks tsentraliseeritud ressursi rühmad, mis sisaldavad core võrgu infrastruktuur, mida saab hallata levinud meeskonnatöö, ja seejärel VMs ja nende rakenduste juurutatud eraldi ressursi rühmad luua. Seda moodust võimaldab rakenduse omanikud juurdepääsu ressursi rühm, mis sisaldab järel avamata juurdepääsu konfiguratsiooni laiuse muutmine suuremaks virtuaalse võrgu ressursse.

## <a name="site-connectivity"></a>Saidi Ühenduvus

### <a name="cloud-only-virtual-networks"></a>Vaid virtuaalne võrkude
Kui kohapealse kasutajad ja arvutid, pole vaja poolelioleva ühenduvuse vms Azure virtuaalse võrgus, on teie virtuaalse võrgu kavandamise otse edasi.

![Tavaline vaid virtuaalse võrgustikudiagramm](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

See lähenemine on tavaliselt Interneti-ühendusega töökoormus, nt Interneti-põhiste veebiserver. Need VMs RDP või punkti saidi VPN-ühendused abil saate hallata.

Kuna nad ei saa ühendust kohapealse võrgu, ainult Azure virtuaalse võrgu kasutada mis tahes osa privaatne IP-aadress ruumi, isegi siis, kui on sama privaatse ruumi kasutada kohapealse.


### <a name="cross-premises-virtual-networks"></a>Asutusesiseses virtuaalne võrkude
Kui kohapealse kasutajate ja arvutite jaoks on vaja poolelioleva ühenduvust vms Azure virtuaalse võrgus, luua mõne virtuaalse võrgu.  Ühenduse kohapealse võrgu ExpressRoute või-saidilt VPN-ühendus.

![Asutusesiseses virtuaalse võrgustikudiagramm](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

Selle konfiguratsiooni puhul Azure virtuaalse võrgu on põhiosas pilvepõhist pikendamine kohapealse võrgu.

Kuna ühenduse loomisel kohapealse võrgu, asukohaga virtuaalse võrgu tuleb kasutada aadressiruumi, kasutatakse teie asutuses on kordumatu osa. Ettevõtte asukohtades määratakse teatud IP alamvõrgu samal viisil Azure'i muutub teise asukohta nagu saate laiendada võrgu.

Luba paketid kohapealse võrgu kaudu oma virtuaalse võrgu sõitma, tuleb konfigureerida oluline kohapealse aadress eesliiteid määramine kohtvõrgu definitsiooni virtuaalse võrgu osana. Sõltuvalt aadressiruumi virtuaalse võrgu ja oluline kohapealse asukohtade määramine, võib olla palju aadress eesliiteid kohalikus võrgus.

Saate teisendada vaid virtuaalse võrgu asukohaga virtuaalse võrgus, kuid seda tõenäoliselt nõuab teilt uuesti IP oma virtuaalse võrgu aadressiruumi ja Azure ressursse. Seetõttu hoolega, kui virtuaalse võrgu peab olema ühendatud kohapealse võrgu kui määrate mõne IP alamvõrgu.

## <a name="subnets"></a>Alamvõrku
Alamvõrku võimaldavad korraldada ressursse, mis on seotud, kas loogiliselt (nt ühe alamvõrgu seotud sama rakenduse vms), või füüsilise (nt ühe alamvõrgu ressursirühma kohta). Saate kasutada ka alamvõrgu eraldamise võtteid turvalisuse tagamiseks.

Asutusesiseses virtuaalse võrkude, peate koostama alamvõrku sama nendega reeglitega, mida kasutate kohapealse ressursid. **Azure'i alati kasutab aadressiruumi jaoks iga alamvõrgu kolm esimest IP-aadressid**. Aadresside alamvõrgu jaoks vajalik arvu määramiseks Alustuseks loendamiseks VMs, mida vajate kohe. Tulevikus prognoosimiseks ja seejärel kasutage järgmise tabeli suuruse alamvõrgu.

Arvu VMs vaja. | Hosti bittide vaja arv | Alamvõrgu suurus
--- | --- | ---
1 – 3 | 3 | / 29
4-11     | 4 | / 28
12-27 | 5 | / 27
28 – 59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE] Tavaline kohapealse alamvõrku, on maksimaalne arv hosti aadressid on alamvõrgu koos n hosti 2<sup>n</sup> – 2. Mõne Azure alamvõrgu hosti aadressid on alamvõrgu koos n hosti maksimumarv on 2<sup>n</sup> – 5 (2 ja 3 iga alamvõrgu kasutava Azure'i aadresse).

Kui valite alamvõrgu suurus, mis on liiga väike, peate uuesti IP ja Juurutage uuesti alamvõrgu VM.


## <a name="network-security-groups"></a>Võrgu turberühmad
Saate rakendada filtreerimise reeglid liigub sujuvalt liiklust oma virtuaalse võrgu kaudu, kasutades võrgu turberühmad. Saate koostada Varundustöö filtreerimise reeglid tagada teie virtuaalse võrgu keskkonnas juhtimine sissetuleva ja väljamineva liikluse, lähte- ja IP-aadresside vahemikud, lubatud pordid jne. Võrgu turberühmad saab rakendada alamvõrku virtuaalse võrgustikus või otse antud virtuaalse võrgu kasutajaliidese. Soovitatav on mõned võrgu turberühma filtreerimine virtuaalse võrguga liikluse tase. Lugege lisateavet [võrgu](../virtual-network/virtual-networks-nsg.md)turberühmade.


## <a name="additional-network-components"></a>Võrgu lisakomponentide
Kohapealse füüsilise võrgu infrastruktuur, sarnaselt Azure virtuaalse võrgunduse võib sisaldada rohkem kui alamvõrku ja IP tegelemine. Kujundamisel oma rakenduse taristu, võite lisada mõned nende lisakomponentide:

- [Lüüside VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) - ühenduse muude Azure virtuaalse võrkude Azure'i või ühenduse loomiseks kohapealse võrgu-saidilt VPN-ühenduse kaudu. Rakendada teekonna ühendused sihtotstarbeline, turvalist ühenduste jaoks. Võite anda kasutajatele otsest juurdepääsu punkti saidi VPN ühendused.
- [Laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-overview.md) – pakub koormusetasandus-liikluse nii sise- ja liikluse vastavalt soovile.
- [Rakenduse lüüsi](../application-gateway/application-gateway-introduction.md) - HTTP koormust tasakaalustavad rakenduse kiht, esitada veebirakenduste mõned täiendavaid eeliseid, mitte juurutamine on Azure laadimine koormusetasakaalustusteenuse.
- [Liikluse haldur](../traffic-manager/traffic-manager-overview.md) - liikluse DNS-i-põhise jaotuse suunamiseks lõppkasutajad lähima saadaval rakenduse lõpp-punkti, võimaldab majutada Azure andmekeskuses eri piirkondadest välja rakenduse.


## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 