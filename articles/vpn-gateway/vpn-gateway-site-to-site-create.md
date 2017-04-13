<properties
   pageTitle="Luua virtuaalse võrgu-saidilt VPN-lüüsi ühendusega Azure klassikaline portaalis | Microsoft Azure'i"
   description="On VNet asutusesiseses lüüsi S2S VPN-ühenduse loomiseks ja hübriidi konfiguratsiooni klassikaline juurutamise mudeli abil."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>On VNet Azure klassikaline portaalis saidilt ühenduse loomine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klassikaline – klassikaline portaal](vpn-gateway-site-to-site-create.md)

Selles artiklis tutvustatakse virtuaalse võrgu ja klassikaline juurutamise mudeli ja klassikaline portaali abil kohapealse võrgu-saidilt VPN-lüüsi ühenduse loomiseks. Saitide ühendused saab kasutada asukohaga ja hübriid konfiguratsioone.

![Saitide skeem] (./media/vpn-gateway-site-to-site-create/site2site.png "saidilt")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Juurutamise mudelite ja -saitide ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Järgmises tabelis on praegu saadaval juurutamise mudelid ja meetodid-saidilt konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse selle tabeli.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Täiendavad konfiguratsioone 

Kui soovite VNets omavahel ühendada, lugege teemat [VNet-VNet klassikaline juurutamise mudeli ühenduse konfigureerimine](virtual-networks-configure-vnet-to-vnet-connection.md). Kui soovite lisada saidilt ühenduse VNet, mis on juba ühenduse, lugege teemat [lisamine on VNet koos olemasoleva ühenduse lüüs VPN-ühendus S2S](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et enne konfigureerimine on järgmist.

- Ühilduvad VPN-seade ja keegi, kes saab seda konfigureerida. Lugege [VPN seadmed](vpn-gateway-about-vpn-devices.md). Kui te pole tuttav konfigureerimise seadme VPN, või ei tunne vahemike asub IP-aadress oma kohapealse võrgu konfiguratsiooni, on vaja kellegagi, kes saab anda need andmed teie eest.

- Väliselt suunatud avaliku IP-aadressi seadme VPN. IP-aadress ei leita taga mõnda NAT.

- Azure'i tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).


## <a name="CreateVNet"></a>Virtuaalse võrgu loomine

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com/)sisse logida.

2. Klõpsake ekraani vasakus allnurgas nuppu **Uus**. Klõpsake navigeerimispaanil, klõpsake **Võrguteenuste**ja klõpsake **Virtuaalse võrgu**. Klõpsake nuppu **Kohandatud loomine** konfiguratsiooniviisardi alustada.

3. Sisestage oma VNet loomiseks järgmistel lehtedel sätete konfigureerimine.

## <a name="Details"></a>Lehe virtuaalse võrgu üksikasjad

Sisestage järgmine teave:

- **Nimi**: virtuaalse võrgu nimi. Näiteks *EastUSVNet*. Saate kasutada seda virtuaalse võrgu nimi VMs ja PaaS eksemplarid juurutamisel nii, et te ei taha liiga keeruline nime muuta.
- **Asukoht**: asukoht on otseselt seotud, kuhu soovite oma ressursse (VM) asuvad füüsilise asukoha (piirkond). Näiteks, kui soovite selle virtuaalse võrguga asuma füüsilise *Ida-USA*juurutamist VMs, valige soovitud kohta. Piirkond, mis on seotud virtuaalse võrgu pärast selle loomist ei saa muuta.

## <a name="DNS"></a>DNS-serverid ja VPN ühenduvuse lehe

Sisestage järgmine teave ja seejärel nuppu Järgmine parempoolses allnurgas noolt.

- **DNS-serverid**: sisestage DNS-i serveri nimi ja IP-aadress või valige otseteemenüü käsk varem registreeritud DNS-i serveri. See säte ei loo DNS-i serveri. See võimaldab teil määrata DNS-serverid, mida soovite kasutada selle virtuaalse võrgu nimelahendus.
- **Konfigureerimine-saidilt VPN**: märkige ruut **Konfigureeri VPN saitide**jaoks.
- **Kohalikus võrgus**: kohtvõrgu tähistab kohapealsete füüsilise asukoha. Saate valida varem loodud kohtvõrgu või luua uue kohalikus võrgus. Kui valite varem loodud kohtvõrgu kasutama, minge lehele **Kohaliku võrgu** konfigureerimise ja veenduge, et VPN seadme IP-aadress (avaliku suunatud IPv4 aadress) VPN-seade on täpne.

## <a name="Connectivity"></a>Saitide ühenduvuse lehe

Kui loote uue kohalikus võrgus, kuvatakse teile **Saidilt ühenduvuse** lehe. Kui soovite kasutada kohtvõrgu varem loodud, kuvatakse selle lehe viisardi ja saate teisaldada järgmise jaotise juurde.

Sisestage järgmine teave ja seejärel klõpsake noolt järgmise.

-   **Nimi**: võrgu nimi, kellele soovite helistada kohalikult (kohapealse) saidil.
-   **VPN seadme IP-aadress**: kohapealse VPN seadme Azure'i ühenduse loomiseks kasutatav avaliku suunatud IPv4-aadressi. VPN-seade ei leita taga mõnda NAT.
-   **Aadressiruumi**: kaasata, alustades IP ja CIDR (aadressi arv). Saate määrata range(s), mida te soovite oma kohaliku kohapealse asukohta lüüsi virtuaalse võrgu kaudu saadetakse aadressi. Kui sihtkoha IP-aadress jääb vahemikku, mida siin, see marsruuditakse virtuaalse võrgu lüüsi kaudu.
-   **Lisa aadressiruumi**: kui teil on mitu aadresside vahemikud, mida soovite saata virtuaalse võrgu lüüsi kaudu, Määrake iga täiendava aadressi vahemik. Saate lisada või eemaldada vahemike hiljem lehel **Kohalikus võrgus** .

## <a name="Address"></a>Lehel virtuaalse võrgu aadressi tühikud

Määrake aadress vahemiku, mida soovite kasutada virtuaalse võrgu jaoks. Neid dünaamiliseks IP-aadresside (DIPS) VMs ja muud rolli eksemplarid, mis selle virtuaalse võrguga juurutamist määratud.

See on eriti oluline valige vahemik, mis ei kattuks mis tahes vahemikud, mida kasutatakse kohapealse võrgu jaoks. On vaja oma võrguadministraatori poole. Leida soovitud vahemiku IP-aadresside kaudu oma kohapealse võrgu aadressiruumi virtuaalse võrgu jaoks kasutamiseks vajalikud võrguadministraatori poole.

Sisestage järgmine teave ja klõpsake märke konfigureerimine võrgu paremal.

- **Aadressiruumi**: kaasata, alustades IP ja aadress arvu. Veenduge, et teie määratud aadressi tühikuid ei kattu ühte aadressi tühikuid, et teil on kohapealse võrgu.
- **Lisa alamvõrgu**: kaasata alates IP ja aadress arvu. Täiendavad alamvõrku pole kohustuslikud, kuid soovite luua eraldi alamvõrgu vms, mis on staatiline DIPS. Või võite olla teie VMs alamvõrku, eraldi oma rolli aknad.
- **Lisa lüüsi alamvõrgu**: klõpsake lüüsi alamvõrgu lisamiseks. Lüüsi alamvõrgu kasutatakse ainult virtuaalse võrgu lüüsi ja selle konfiguratsiooni jaoks on vaja.

Klõpsake lehe allservas märke ja virtuaalse võrgu alustab loomiseks. Kui see on lõpule jõudnud, kuvatakse **loodud** loetletud Azure'i klassikaline portaalis lehel **võrgu** **olek** . Pärast soovitud VNet on loodud, saate konfigureerida virtuaalse võrgu lüüsi.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>Virtuaalse võrgu lüüsi konfigureerimine

Konfigureerida virtuaalse võrgu lüüsi-saidilt turvalist ühendust luua. Teemast [Azure klassikaline portaalis virtuaalse võrgu lüüsi konfigureerimine](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Järgmised sammud

Kui teie ühendus on loodud, saate lisada virtuaalse võrguga virtuaalmasinates. Lisateavet dokumentatsioonist [Virtuaalmasinates](https://azure.microsoft.com/documentation/services/virtual-machines/) .
