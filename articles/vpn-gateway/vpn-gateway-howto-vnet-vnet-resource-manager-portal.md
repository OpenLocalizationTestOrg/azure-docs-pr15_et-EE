<properties
   pageTitle="Ühenduse VNets ressursihaldur juurutamise mudeli ja Azure portaali abil | Microsoft Azure'i"
   description="Luua VPN lüüsi seos VNets ressursihaldur ja Azure portaali abil."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Azure'i portaalis VNet-VNet ühenduse konfigureerimine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klassikaline – klassikaline portaal](virtual-networks-configure-vnet-to-vnet-connection.md)


Selles artiklis tutvustatakse VNets ressursihaldur juurutamise mudeli VPN-lüüsi ja Azure portaali abil vahelise ühenduse loomise juhiseid.

Virtuaalne võrkude ühendamiseks Azure portaali kasutamisel on VNets peab olema sama tellimuse. Kui virtuaalse võrguga on erinevad tellimused, saate neid endiselt ühendada [PowerShelli](vpn-gateway-vnet-vnet-rm-ps.md) juhiste abil.

![v2v skeem](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Juurutamise mudelite ja VNet-VNet ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Järgmises tabelis on praegu saadaval juurutamise mudelid ja meetodid VNet-VNet konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse selle tabeli.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet silmitsemine

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>VNet-VNet ühenduste kohta

Virtuaalse võrgu teise virtuaalse võrguga ühenduse loomisel (VNet VNet) on sarnane on VNet ühenduse oma kohapealse saidi asukoha. Mõlemat tüüpi ühenduvuse kasutavad on Azure VPN-lüüsi turvaline tunneliga, kasutades IPsec/IKE. VNets, loote võib olla eri piirkondadest või muu tellimused.

Saate ühendada isegi VNet-VNet suhtlemine mitme saidi konfiguratsioone. See võimaldab teil luua võrgu topoloogiatest, mis sisaldavad asutusesiseses Ühenduvus omavahel virtuaalse võrguühendus, nagu on näidatud järgmisel joonisel:

![Andmeühenduste kohta] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Andmeühenduste kohta")

### <a name="why-connect-virtual-networks"></a>Miks ühenduse virtuaalse võrkudes?

Kui soovite virtuaalne võrkude ühendamiseks järgmistel põhjustel.

- **Piirkonna geo-koondamise ja geo-kohalolek**
    - Saate häälestada oma geo-kopeerimine või sünkroonimise turvaline Ühenduvus ilma läheb üle Interneti-ühendusega lõpp-punktid.
    - Azure'i liikluse juhataja ja laadi koormusetasakaalustusteenuse, saate häälestada tugevalt saadaval töökoormus geo-koondamise mitme Azure'i piirkondade lõikes. Üks oluline näide on SQL alati kohta kättesaadavus levirühmadega levitada mitme Azure'i piirkondade häälestamiseks.

- **Piirkondliku mitmekihilise rakenduste eraldamise või äärist haldus**
    - Samas piirkonnas, saate häälestada mitmekihilise rakenduste abil mitme virtuaalse võrgu omavahel ühendatud eraldi või haldus nõuete tõttu.

VNet-VNet ühenduste kohta lisateabe saamiseks vt käesoleva artikli lõpus [VNet-VNet FAQ](#faq) .

### <a name="values"></a>Näide sätted

Kasutab neid juhiseid kasutamisel saate Näidisväärtuste konfigureerimine. Näide eesmärgil kasutame mitu aadressi tühikuid iga VNet. Siiski VNet-VNet konfiguratsioone ei nõua aadress tühikud.

**TestVNet1 väärtused:**

- VNet nimi: TestVNet1
- Aadress: 10.11.0.0/16
    - Alamvõrgu nimi: FrontEnd
    - Alamvõrgu aadress vahemiku: 10.11.0.0/24
- Ressursirühm: TestRG1
- Asukoht: Ida-USA
- Aadress: 10.12.0.0/16
    - Alamvõrgu nimi: Taustväärtus
    - Alamvõrgu aadress vahemiku: 10.12.0.0/24
- Lüüsi alamvõrgu nimi: GatewaySubnet (see on Automaattäite portaalis)
    - Lüüsi alamvõrgu aadress vahemiku: 10.11.255.0/27
- DNS-i Server: Kasutage oma DNS-i serveri IP-aadress
- Virtuaalne võrgu lüüsi nimi: TestVNet1GW
- Lüüsi tüüp: VPN
- VPN-tüüp: marsruutimiseks vastavalt
- Tootekood: Valige lüüsi SKU, mida soovite kasutada
- Avaliku IP-aadressi nimi: TestVNet1GWIP
- Ühenduse väärtused:
    - Nimi: TestVNet1toTestVNet4
    - Ühiskasutusega võti: saate luua ühiskasutusega võti ise. Selle näite puhul kasutame abc123. Oluline on see, et selle VNets vahelise ühenduse loomisel väärtus peab vastama.

**TestVNet4 väärtused:**

- VNet nimi: TestVNet4
- Aadress: 10.41.0.0/16
    - Alamvõrgu nimi: FrontEnd
    - Alamvõrgu aadress vahemiku: 10.41.0.0/24
- Ressursirühm: TestRG1
- Asukoht: Lääne USA.
- Aadress: 10.42.0.0/16
    - Alamvõrgu nimi: Taustväärtus
    - Alamvõrgu aadress vahemiku: 10.42.0.0/24
- GatewaySubnet nimi: GatewaySubnet (see on Automaattäite portaalis)
    - Vahemiku GatewaySubnet aadress: 10.41.255.0/27
- DNS-i Server: Kasutage oma DNS-i serveri IP-aadress
- Virtuaalne võrgu lüüsi nimi: TestVNet4GW
- Lüüsi tüüp: VPN
- VPN-tüüp: marsruutimiseks vastavalt
- Tootekood: Valige lüüsi SKU, mida soovite kasutada
- Avaliku IP-aadressi nimi: TestVNet4GWIP
- Ühenduse väärtused:
    - Nimi: TestVNet4toTestVNet1
    - Ühiskasutusega võti: saate luua ühiskasutusega võti ise. Selle näite puhul kasutame abc123. Oluline on see, et selle VNets vahelise ühenduse loomisel väärtus peab vastama.


## <a name="CreatVNet"></a>1. loomine ja konfigureerimine TestVNet1

Kui teil on juba VNet, kontrollige, kas sätted on ühildu teie VPN-lüüsi kujundus. Mis tahes alamvõrku, mis võivad kattuvad muude võrkude väliskontaktidega erilist tähelepanu. Kui teil on kattuvate alamvõrku, ühendust ei tööta õigesti. Kui teie VNet õiged sätted on konfigureeritud, saate alustada [DNS-i serveri määramine](#dns) jaotises toodud juhised.

### <a name="to-create-a-virtual-network"></a>Virtuaalse võrgu loomine

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. lisada täiendavad aadressiruumi ja alamvõrku loomine

Saate lisada täiendavad aadressiruumi ja luua alamvõrku, kui teie VNet on loodud.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. lüüsi alamvõrgu loomine

Enne virtuaalse võrgu ühenduse lüüs, peate esmalt lüüsi alamvõrgu virtuaalse võrgu, millele, millega soovite ühenduse luua. Võimaluse korral on parem luua lüüsi alamvõrku, kasutate CIDR plokk /28 või /27 tulevaste konfigureerimise nõuded mahutamiseks piisavalt IP-aadressid.

Kui loote selle konfiguratsiooni teostamiseks, vaadake neid [näide sätted](#values) oma lüüsi alamvõrgu loomisel.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Lüüsi alamvõrgu loomiseks

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. määrata DNS-i serveris (valikuline)

Kui soovite oma VNets juurutatud virtuaalmasinates jaoks nimelahendus, peaks teie määratud DNS-i serveri.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. virtuaalse võrgu lüüsi loomine

Selles etapis loote virtuaalse võrgu lüüsi oma VNet. Selles etapis tuleb võib kuluda kuni 45 minutit. Kui loote selle konfiguratsiooni teostamiseks, võib viidata [näide sätted](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Lüüsi virtuaalse võrgu loomine

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. loomine ja konfigureerimine TestVNet4

Kui olete konfigureerinud TestVNet1, luua TestVNet4 korrates eelmisi toiminguid, need TestVNet4 väärtuste asendamine. Saate ei pea ootama, kuni virtuaalse võrgu lüüsi jaoks TestVNet1 on lõpule jõudnud, enne konfigureerimist TestVNet4 loomine. Kui kasutate oma väärtusi, veenduge, et aadress tühikuid ei kattu VNets, mida soovite ühendada.


## <a name="TestVNet1Connection"></a>7. TestVNet1 ühenduse konfigureerimine

Kui virtuaalse võrgu lüüside TestVNet1 nii TestVNet4 lõpule viinud, saate luua virtuaalse võrgu lüüsi ühendused. Selles jaotises kuvatakse luua ühenduse kaudu VNet1 VNet4.

1. Liikuge **kõik ressursid**, teie VNet virtuaalse võrgu lüüsi. Näiteks **TestVNet1GW**. Klõpsake **TestVNet1GW** avamiseks virtuaalse võrgu lüüsi tera.

    ![Ühenduste blade] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Ühenduste blade")

2. Klõpsake nuppu **+ Lisa** avamiseks tera **Lisa ühendus** .

3. **Ühenduse lisamine** enne, väljale nimi Tippige oma ühenduse nimi. Näiteks **TestVNet1toTestVNet4**.

    ![Ühenduse nimi] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Ühenduse nimi")

4. **Ühenduse tüüp**. Valige rippmenüüst **VNet-VNet** .

5. **Esimese virtuaalse võrgu lüüsi** välja väärtus täidetakse automaatselt, kuna loote selle ühenduse lüüs määratud virtuaalse võrgu.

6. **Teine virtuaalse võrgu lüüsi** väli on VNet, mida soovite luua ühenduse, virtuaalse võrgu lüüsi. Klõpsake nuppu, **Valige teise virtuaalse võrgu lüüsi** avamiseks **Valige virtuaalse võrgu lüüsi** tera.

    ![Ühenduse lisamine] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Ühenduse lisamine")

7. Saate vaadata selle tera virtuaalse võrgu lüüsid, mis on loetletud. Pange tähele, et ainult virtuaalse võrgu lüüsid, mis on teie tellimus on loetletud. Kui soovite ühendada virtuaalse võrgu lüüsi, mis pole teie tellimus, kasutage [PowerShelli artikkel](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Klõpsake nuppu virtuaalse võrgu lüüsi, mida soovite ühendada.
 
9. Tippige väljale **ühiskasutuses klahvi** ühiskasutusega klahvi teie ühendus. Saate luua või see võti ise luua. Saidilt ühendus, saate kasutada võti oleks täpselt sama seadme kohapealse ja virtuaalse lüüsi võrguühendus. Mõiste on sarnane siin, välja arvatud juhul, et selle asemel ühenduse VPN seadme, loote teise virtuaalse võrgu lüüsi.

    ![Ühiskasutuses võti] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Ühiskasutuses võti")

10. Klõpsake nuppu **OK** tera allosas muudatuste salvestamiseks.

## <a name="TestVNet4Connection"></a>8. TestVNet4 ühenduse konfigureerimine

Järgmiseks luua ühenduse TestVNet4 TestVNet1. Kasutage sama meetodit, mida kasutasite ühenduse loomiseks TestVNet1 TestVNet4. Veenduge, et sama ühiskasutusega võtit kasutada.

## <a name="VerifyConnection"></a>9 ühenduse kontrollimine

Kontrollige ühendust. Iga virtuaalse võrgu lüüsi, tehke järgmist.

1. Otsige üles tera virtuaalse võrgu lüüsi. Näiteks **TestVNet4GW**. 
2. Enne virtuaalse võrgu lüüsi, nuppu **ühendused** ühendused tera virtuaalse võrgu lüüsi kuvamiseks.

Saate vaadata ühendused ja oleku kontrollimine. Kui ühendus on loodud, kuvatakse **õnnestus** ja **ühendatud** olek väärtustena.

![Õnnestus] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Õnnestus")

Topeltklõpsake iga ühendus eraldi vaadata täpsemat teavet ühendus.

![Essentialsi] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Essentialsi")

## <a name="faq"></a>VNet-VNet KKK

FAQ andmete VNet-VNet ühenduste kohta lisateabe vaatamiseks.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Järgmised sammud

Kui teie ühendus on loodud, saate lisada virtuaalse võrguga virtuaalmasinates. Üksikasjalikud juhised leiate teemast [loomine virtuaalse masina](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
