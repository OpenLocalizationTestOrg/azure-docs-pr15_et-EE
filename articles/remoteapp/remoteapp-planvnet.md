<properties
    pageTitle="Kuidas kavandada virtuaalse võrgu jaoks on Azure RemoteApp saidikogumi | Microsoft Azure'i"
    description="Saate teada, kuidas mõni Azure RemoteApp saidikogumi jaoks virtuaalse võrgu plaanimine."
    services="remoteapp"
    documentationCenter="" 
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Kuidas Azure RemoteApp virtuaalse võrgu kavandamine

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Selles dokumendis kirjeldatakse, kuidas luua Azure virtuaalse võrgu (VNET) ja alamvõrgu Azure RemoteApp. Kui olete varem selliseid Azure'i, see on võimalus, mis aitab teil Virtualisoinnilla võrgu taristule pilveteenusesse ja Azure ja teie asutusesisese ressursid hübriid lahenduste loomiseks. Lugege lisateavet selle kohta [siin](../virtual-network/virtual-networks-overview.md).

Kui soovite määratlemiseks turbepoliitikate liikluse (nii sissetulevad ja väljaminevad), kui juurutate Azure RemoteApp virtuaalse võrgu, soovitame loomise eraldi alamvõrgu Azure RemoteApp ülejäänud oma juurutuste Azure virtuaalse võrgu kaudu. Määratleda turbepoliitikate Azure virtuaalse alamvõrku kohta lisateabe saamiseks lugege [mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Azure RemoteApp saidikogumid koos Azure'i tüübid

Järgnevatel piltidel kuvada kaks eri saidikogumi suvandeid, kui soovite kasutada virtuaalse võrgus.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp cloud kogum VNET

 ![Azure RemoteApp – pilveteenuses saidikogumi koos mõne VNET](./media/remoteapp-planvpn/ra-cloudvpn.png)

See tähistab on Azure RemoteApp saidikogumi, kus ressursse, mis RemoteApp seansi hosts peavad juurde pääsema on juurutatud Azure. Need võivad olla sama VNET nimega RemoteApp VNET või muu VNET Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure RemoteApp hübriid kogum VNET

![Azure RemoteApp - hübriid saidikogumi on VNET abil](./media/remoteapp-planvpn/ra-hybridvpn.png)

See tähistab on Azure RemoteApp kogum, kus mõned ressursid, mis RemoteApp seansi hosts juurdepääsuks on juurutatud asutusesisese. RemoteApp VNET on seotud kohapealse võrgu like-saidilt VPN- või teekonna Azure hübriid-ja hõlbustusvahendite kasutamine.


## <a name="how-the-system-works"></a>Süsteemi tööpõhimõte

All hõlmab juurutamine Azure RemoteApp virtuaalse alamvõrku, ettevalmistamise käigus ignoreeritud Azure'i virtuaalmasinates (koos üleslaaditud pilt). Kui olete valinud hübriid kogumi, saame selle domeenikontrolleri sisestatud ettevalmistamise töövoo virtuaalse võrgu DNS-i serveri FQDN lahendamiseks proovida.  
Kui loote olemasolevat virtuaalse võrku, veenduge, et seada vajalikud pordid sisse oma võrgu turberühmad teie alamvõrku Azure RemoteApp. 

Soovitame kasutada [Azure RemoteApp jaoks piisavalt suur alamvõrgu](remoteapp-vnetsizing.md). Suurim ei toeta Azure virtuaalse võrgu on /8 (abil CIDR alamvõrgu määratlused). Teie alamvõrku peaks olema suur, et kõik Azure RemoteApp VMs ajal scale-up on kasutamisel kasutajaid rakendused. 

Järgnevalt on asju, mida on vaja lubamiseks oma virtuaalse alamvõrku: 

2.  Väljamineva liikluse aadressilt alamvõrgu peaks lubatud port vahemikus 10101-10175 sisemise Azure RemoteApp teenuse suhelda.
3.  Väljamineva liikluse tuleks lubada teie alamvõrku ühenduse Azure Storage pordi 443
4.  Kui teil on majutatud Azure Active Directory, veenduge, et kõik VM virtuaalne alamvõrku jaoks Azure RemoteApp sees on selle domeenikontrolleri ühendust luua. DNS-i virtuaalse võrgu peaks olema selle domeenikontrolleri FQDN lahendada.


## <a name="virtual-network-with-forced-tunneling"></a>Jõustatud tunneling virtuaalse võrgu

Kõigi uute Azure RemoteApp saidikogumid on nüüd [sunnitud tunneling](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) toetatud. Me praegu ei toeta olemasolevasse kollektsiooni toetamiseks jõustatud tunneling migreerimise.  Peate oma VNET, millega lingite Azure RemoteApp abil olemasoleva saidikogumite kustutamine ja looge uus saada sunnitud tunneling lubatud teie saidikogumid. 
