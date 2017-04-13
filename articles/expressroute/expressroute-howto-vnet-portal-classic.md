<properties
   pageTitle="Klassikaline portaalis ExpressRoute virtuaalse võrgu- ja lüüsi konfigureerimine | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse ExpressRoute klassikaline juurutamise mudeli ja klassikaline portaali abil virtuaalse võrgu häälestamiseks."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Klassikaline portaalis ExpressRoute virtuaalse võrgu loomine

Selles artiklis toodud juhiseid juhendab teid konfigureerida virtuaalse võrgu ja virtuaalse võrgu lüüsi kasutamiseks koos ExpressRoute klassikaline juurutamise mudeli ja klassikaline portaali abil.

Kui otsite juhised ressursihaldur juurutamise mudeli, saate kasutada järgmisi artikleid: [PowerShelli abil virtuaalse võrgu loomine](../virtual-network/virtual-networks-create-vnet-arm-ps.md) ja [lisamine lüüsi VPN-i ressursihaldur VNet ExpressRoute jaoks](expressroute-howto-add-gateway-resource-manager.md).

**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Klassikaline VNet ja lüüsi loomine

Klassikaline VNet ja virtuaalse võrgu lüüsi loomine toimige järgmiselt. Kui teil on juba klassikaline VNet jaotisest [konfigureerimine mõne olemasoleva klassikaline VNet](#config) selle artikli.

1. [Azure'i klassikaline portaali](http://manage.windowsazure.com)sisse logida.

2. Klõpsake Kuva alumises vasakus nurgas nuppu **Uus**. Klõpsake navigeerimispaanil, klõpsake **Võrguteenuste**ja klõpsake **Virtuaalse võrgu**. Klõpsake nuppu **Kohandatud loomine** konfiguratsiooniviisardi alustada.

3. Sisestage lehel **Virtuaalse võrgu üksikasjad** järgmist:

    - **Nimi** – virtuaalse võrgu nimi. Saate kasutada seda virtuaalse võrgu nimi VMs ja PaaS eksemplarid juurutamisel nii, et te ei taha liiga keeruline nime muuta.
    - **Asukoht** – asukoht on otseselt seotud, kuhu soovite oma ressursse (VM) asuvad füüsilise asukoha (piirkond). Näiteks, kui soovite selle virtuaalse võrguga asuma füüsilise Ida-USA juurutamist VMs, valige soovitud kohta. Piirkond, mis on seotud virtuaalse võrgu pärast selle loomist ei saa muuta.

4. Lehe **DNS-serverid ja virtuaalse Privaatvõrgu ühendus** sisestage järgmine teave ja klõpsake parempoolses allnurgas noolenuppu järgmine. 

    - **DNS-serverid** - sisestage DNS-i serveri nimi ja IP-aadress või valige otseteemenüü käsk varem registreeritud DNS-i serveri. See säte ei loo DNS-i serveri. See võimaldab teil määrata DNS-serverid, mida soovite kasutada selle virtuaalse võrgu nimelahendus.
    - **Saitide ühenduvuse** – märkige ruut **Konfigureeri VPN saitide**jaoks.
    - **ExpressRoute** – märkige ruut **Kasuta ExpressRoute**. See suvand kuvatakse ainult siis, kui valisite **VPN saitide konfigureerimine**.
    - **Kohaliku võrgu** - on vaja on kohtvõrgu sait ExpressRoute. Siiski ignoreeritakse puhul on ExpressRoute ühenduse määratud kohtvõrgu saidi aadress eesliiteid. Selle asemel kasutatakse Microsofti kaudu ExpressRoute ringi reklaamida aadress eesliiteid marsruutimise eesmärgil.<BR>Kui teil on juba kohtvõrgu jaoks ExpressRoute ühenduse loonud, saate selle valige rippmenüüst. Kui ei, siis valige **määra uue kohalikus võrgus**.

5. **Saitide ühenduvuse** lehe kuvatakse siis, kui määrata uue kohtvõrgu eelmises juhises valitud. Konfigureerimiseks teie kohalikus võrgus, sisestage järgmine teave ja klõpsake nuppu järgmise noolt. 

    - **Nimi** – nimi, kellele soovite helistada kohalikult (kohapealse) võrgu saidi.
    - **Aadressiruumi** – alates ja CIDR (aadressi arv). Mis tahes aadress vahemiku saate määrata, kui see ei kattu aadress vahemiku virtuaalse võrgu jaoks. Tavaliselt on see määrata, aadresside vahemikud oma kohapealse võrgu jaoks, kuid puhul ExpressRoute, kasutatakse neid sätteid. See säte on vajalik loomine kohaliku võrgu kasutamisel klassikaline portaali.
    - **Lisa aadressiruumi** – see säte ei ole asjakohane ExpressRoute.


6. Lehel **Virtuaalse võrgu aadress tühikuid** , sisestage järgmine teave ja klõpsake märke konfigureerimine võrgu paremal. 

    - **Aadressiruumi** – sealhulgas alustada IP ja aadress arv. Veenduge, et teie määratud aadressi tühikuid ei kattu ühte aadressi tühikuid, et teil on teie kohalikus võrgus.
    - **Lisa alamvõrgu** – alates ja aadresside arvu. Täiendavad alamvõrku pole kohustuslikud.
    - **Lisa lüüsi alamvõrgu** – klõpsake lüüsi alamvõrgu lisamiseks. Lüüsi alamvõrgu kasutatakse ainult virtuaalse võrgu lüüsi ja selle konfiguratsiooni jaoks on vaja.<BR>Lüüsi alamvõrgu CIDR (aadressi arv) ExpressRoute jaoks peab olema /28 või suurem (/ 27, / 26 jne.). See võimaldab töötada konfiguratsiooni lubama piisavalt selle alamvõrgu IP-aadressid. Klassikaline portaalis, kui olete valinud ruut, et kasutada ExpressRoute, portaali määrab lüüsi alamvõrgu /28 abil.  Ei saa klassikaline portaalis CIDR aadresside arvu korrigeerida. Lüüsi alamvõrgu kuvatakse **lüüsi** klassikaline portaalis, kuigi lüüsi alamvõrku, mis on loodud tegelik nimi on tegelikult **GatewaySubnet**. Saate vaadata selle nime PowerShelli abil või Azure'i portaalis.

7. Klõpsake lehe allservas märke ja virtuaalse võrgu alustab loomiseks. Kui see on lõpule jõudnud, kuvatakse **loodud** loendis klassikaline portaalis lehel **võrgu** **olek** .

## <a name="gw"></a>Lüüsi loomine

1. Lehel **võrkude** virtuaalse võrgu äsja loodud, klõpsake käsku **armatuurlaua** lehe ülaosas.

2. **Armatuurlaua** lehe allservas nuppu **Lüüsi loomine** ja valige **Dünaamiline marsruutimine**. Klõpsake nuppu **Jah** , et kinnitada, et soovite luua lüüsi.

3. Kui lüüs hakkab loomise, kuvatakse sõnumi ettevõttevälistel, teate, et lüüsi käivitatud. Võib kuluda kuni 45 minutit lüüsi loomiseks.

4. Võrgu link soovitud ringi. Järgige juhiseid teemas [Kuidas link VNets, et ExpressRoute topoloogia](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Mõne olemasoleva klassikaline VNet ExpressRoute konfigureerimine

Kui teil on juba klassikaline VNet, saate selle ühenduse ExpressRoute klassikaline portaalis konfigureerida. Sätted on sama, mis ülal, jaotised nii tutvumisest need jaotised tutvuda meilikontot. Kui soovite luua ühenduse ExpressRoute /-saidilt kooseksisteerimisele, leiate [selle artikli](expressroute-howto-coexist-classic.md) juhiseid. Need on erinevas selles artiklis toodud juhiseid.
 
1. Peate looma kohalikus võrgus enne ülejäänud VNet sätete värskendamist. Looge uus kohalikus võrgus, mis on nõutav ExpressRoute konfigureerimisel klassikaline portaali kaudu, klõpsake nuppu **Uus** **>** **Võrguteenuste** **>** **Virtuaalse võrgu** **>** **Lisa kohalikus võrgus**. Järgige viisardi loomine kohalikus võrgus.

2. Kasutage lehe **konfigureerimine** oma VNet sätete ülejäänud värskendamiseks ja seostamiseks VNet kohaliku võrku.

3. Pärast sätete konfigureerimine minge jaotise [Loo lüüsi](#gw) käesoleva artikli lüüsi loomine.


## <a name="next-steps"></a>Järgmised sammud

- Kui soovite lisada virtuaalmasinates virtuaalse võrgu, lugege teemat [Virtuaalmasinates õppeteemad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Kui soovite lisateavet ExpressRoute, lugege teemat [ExpressRoute ülevaade](expressroute-introduction.md).


 
