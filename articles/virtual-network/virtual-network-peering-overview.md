
<properties
   pageTitle="Azure virtuaalse võrgu silmitsemine | Microsoft Azure'i"
   description="Lisateavet VNet silmitsemine Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>VNet silmitsemine

VNet silmitsemine on süsteem, mis ühendab kaks virtuaalse võrku (VNets) piirkonna Azure nurgakivi võrgu kaudu. Kui piilus, kahe virtuaalse võrgu kuvatakse kõik ühenduvuse eristata. Hallatakse endiselt eraldi ressurssidena, kuid virtuaalmasinates nende virtuaalse võrkudes saate omavahel otse privaatse IP-aadresside abil suhelda.

Omavahel virtuaalmasinates piilus virtuaalse võrkudes marsruuditakse sarnaselt sama virtuaalse võrgu VMs vahel on marsruutida liikluse Azure taristu kaudu. Mõned VNet silmitsemine kasutamise eelised on järgmised:

- Ressursside erinevate virtuaalse võrkude latentsusajaga, latentsusajaga seost.
- Võimalus kasutada võrgu seadmed ja VPN lüüside piilus VNet punktide teel.
- Võimalus virtuaalse võrk, mis kasutab Azure ressursihaldur mudeli virtuaalse võrguga, mis kasutab klassikaline juurutamise mudelit ja lubada täielik Ühenduvus ressursside virtuaalse võrgustikuga ühendust luua.

Nõuded ja VNet silmitsemine olulisi aspekte.

- Kahe virtuaalse võrgu, mis on piilus peaks olema Azure piirkonna.
- Virtuaalne võrkude, mis on piilus peaks olema kattuvad IP address tühikuid.
- VNet silmitsemine on kaks virtuaalse vahel ja ei ole tuletatud sihiliste seost. Näiteks kui virtuaalse võrgul A on piilus virtuaalse võrguga B ja kui virtuaalse võrgu B on piilus virtuaalse võrguga C, see tõlkimine juurde virtuaalse võrgu lisamine on piilus virtuaalse võrguga C.
- Silmitsemine saate kindlaks virtuaalse võrkude kaks erinevat tellimuste vahel, kui kaua õigustega kasutaja nii tellimuste lubab selle silmitsemine ja tellimused on seotud sama Active Directory rentnikuga. 
- Ressursi halduri mudelis virtuaalse võrgu ja klassikaline juurutamise mudeli vahel silmitsemine nõuab, et selle VNets peaks olema sama tellimuse.
- Virtuaalne võrk, mis kasutab ressursihaldur juurutamise mudeli saate piilus teise virtuaalse võrguga, mis kasutab seda mudelit või virtuaalse võrguga, mis kasutab klassikaline juurutamise mudel. Virtuaalne võrkudes, milles klassikaline juurutamise mudeli kasutamine, ei saa siiski piilus üksteisega.
- Kuigi virtuaalmasinates piilus virtuaalse võrkudes suhtlemine ei ole läbilaskevõime täiendavad piirangud, kehtib endiselt läbilaskevõime ots VM suurusest.


![Tavaline VNet silmitsemine](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Ühenduvus
Pärast kahe virtuaalse võrgu on piilus, virtuaalse masina (web/töötaja roll) virtuaalse võrgu saate otse ühendada muude virtuaalmasinates piilus virtuaalse võrgu. Nende kahe võrgu on täielik IP-tasandi Ühenduvus.

Võrgu latentsuse round reisi vahel kaks virtuaalmasinates piilus virtuaalse võrkudes on sama mis pendellevi, virtuaalse kohtvõrgu sees. Võrgu läbilaskevõime põhineb läbilaskevõime, virtuaalse masina proportsionaalne suuruse lubatud. Täiendavate piirangute ribalaius pole.

Omavahel virtuaalmasinates piilus virtuaalse võrkudes marsruuditakse otse Azure tagaandmebaas taristu ja pole lüüsi kaudu.

Sisemise koormusetasakaalustusega (ILB) lõpp-punktid piilus virtuaalse võrgu pääsevad virtuaalmasinates virtuaalse võrku. Võrgu turberühmad (NSGs) saate rakendada kas blokeerida juurdepääsu muude virtuaalse võrgu või alamvõrku, kui soovitud virtuaalse võrku.

Kui kasutajad konfigureerida silmitsemine, nad avamine või sulgemine NSG reegleid virtuaalse võrgu vahel. Kui kasutaja valib avamiseks täielik ühenduvus vahel piilus virtuaalse (see on vaikimisi valitud), siis saate NSGs teatud alamvõrku või virtuaalmasinates blokeerimine või teatud juurdepääsu keelamiseks.

Azure'i antud sisemise DNS-i nimi resolutsioon virtuaalmasinates ei tööta piilus virtuaalse võrkude. Virtuaalmasinates on sisemise DNS-i nimed, mis on ainult kohaliku virtuaalse võrgustikus lahutatavat. Kasutajad saavad siiski konfigureerida virtuaalmasinates, mis töötavad piilus virtuaalse võrgu virtuaalse võrgu jaoks DNS-i serverid.

## <a name="service-chaining"></a>Teenuse Aheldamise
Kasutajad saavad konfigureerida kasutaja määratletud marsruutimiseks tabelid, mis IP-aadress, osutage käsule virtuaalmasinates piilus virtuaalse võrkudes nimega "järgmise hop", nagu on näidatud joonisel selle artikli. See võimaldab kasutajatel saavutada teenuse Aheldamise, kaudu, mille nad saate suunata liikluse virtuaalse seadme, kus töötab piilus virtuaalse võrgu kaudu kasutaja määratletud marsruutimiseks tabelite virtuaalse võrgust.

Kasutajad saavad luua ka jaoturi rääkis tüüp keskkonnas, kus jaoturi majutada taristu komponendid, nt võrgu virtuaalse seadme. Kõikide rääkis virtuaalse võrkude vaadata läbi siis seda, kui ka seadmed, mis töötavad jaoturi virtuaalse võrgu-liikluse alamhulga. Lühidalt öeldes VNet silmitsemine võimaldab järgmise sõnumihüppe kohta, pärast IP-aadressi kasutaja määratletud marsruutimiseks tabeli olema virtuaalse masina piilus virtuaalse võrgu IP-aadress.

## <a name="gateways-and-on-premises-connectivity"></a>Lüüside ja kohapealsete Ühenduvus
Iga virtuaalse võrgu sõltumata sellest, kas see on piilus teise virtuaalse võrguga, saate siiski on eraldi lüüsi ja kasutada seda ühenduse loomiseks kohapealse. Kasutajad saavad ka konfigureerida [VNet-VNet ühendused](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) abil lüüsid, kuigi virtuaalne võrkude on piilus.

Mõlema variandi virtuaalse võrgu ühendamiseks konfigureerimisel liikluse virtuaalse võrgu vahel liigub sujuvalt silmitsemine konfigureerimise kaudu (st Azure nurgakivi kaudu).

Kui virtuaalne võrkude on piilus, kasutajad saavad ka konfigureerida lüüsi piilus virtuaalse võrgu kohapealse punktina teel. Sel juhul ei saa virtuaalse võrgu, mis kasutab remote lüüsi on eraldi lüüsi. Ühe virtuaalse võrgu võib olla ainult üks lüüsi. See võib olla kohaliku lüüsi või vastuseks Kaug lüüsi (piilus virtuaalse võrgu), nagu on näidatud järgmisel pildil.

Lüüsi teel ei toetata silmitsemine seos virtuaalse võrgu ressursihaldur mudeli ja neile, kes kasutavad klassikaline juurutamise mudeli abil. Nii virtuaalse võrgu silmitsemine seosega vaja kasutamine ressursihaldur juurutamise mudeli lüüsi teel.

Kui virtuaalse võrke, mis ühiskasutavate ühe Azure'i ExpressRoute ühendus on piilus, liikluse nende vahel läheb läbi silmitsemine seose (ehk teisisõnu öeldes on Azure võrgu kaudu). Kasutajad saavad endiselt abil kohaliku lüüside iga virtuaalse võrku ühenduse loomiseks kohapealse ringi. Teise võimalusena saate nad kasutavad ühiskasutusega lüüsi ja kohta kohapealse ühenduvuse konfigureerimine.

![VNet silmitsemine teel](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Ettevalmistamise
VNet silmitsemine on õigustega toiming. See on eraldi funktsioon jaotises VirtualNetworks nimeruumi. Kasutaja saab lubada silmitsemine teatud õigusi anda. Kasutaja, kes on lugemis-kirjutuspääs virtuaalse võrku pärib automaatselt need õigused.

Kasutaja, kes on kas administraator või silmitsemine võimalus õigustega kasutaja saab algatada mõne muu VNet silmitsemine toimingut. Kui on kattuvad taotluse silmitsemine teisele küljele ja muud tingimused on täidetud, luuakse selle silmitsemine.

Vaadake lisateavet selle kohta, kuidas luua VNet silmitsemine kahe virtuaalse võrgu vahel jaotist "Järgmised sammud" artikleid.

## <a name="limits"></a>Piirangud
On ühe virtuaalse võrgu jaoks lubatud peerings maksimumarvuga. Vaadake lisateavet [Azure võrgu piirangud](../azure-subscription-service-limits.md#networking-limits) .

## <a name="pricing"></a>Hinnad
VNet silmitsemine on tasuta jooksul. Pärast seda, on nominaalne tasuta sissepääsu ja sealt liikluse, mis kasutab funktsiooni silmitsemine. Lisateabe saamiseks vaadake [hinnad lehe](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="next-steps"></a>Järgmised sammud
- [Häälestamine silmitsemine virtuaalse võrgu vahel](virtual-networks-create-vnetpeering-arm-portal.md).
- Lisateavet [NSGs](virtual-networks-nsg.md).
- Teave [kasutaja määratletud marsruudib ja IP ümbersuunamine](virtual-networks-udr-overview.md).
