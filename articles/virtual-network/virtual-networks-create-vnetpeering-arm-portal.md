<properties
   pageTitle="Looge VNet silmitsemine Azure'i portaalis | Microsoft Azure'i"
   description="Saate teada, kuidas luua virtuaalse võrgu kaudu Azure portaali sisse ressursihaldur."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Virtuaalse võrgu silmitsemine Azure portaali loomine

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

VNet silmitsemine stsenaarium kohal põhjal, kasutades Azure portaali loomiseks tehke järgmist.

1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2. Luua VNET silmitsemine, peate looma kaks lingid, üks kummagi suuna kahe VNets vahel. Saate luua VNET silmitsemine linki, et VNET2 VNET1 esmalt. Portaalis, klõpsake nuppu **Sirvi** > **virtuaalse võrgu valimine**

    ![VNet silmitsemine Azure portaali loomine](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. Virtuaalne võrkude tera, valige VNET1, klõpsake nuppu Peerings ja seejärel käsku Lisa

    ![Valige silmitsemine](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. Lisada silmitsemine blade silmitsemine lingi nimi LinkToVnet2, valige soovitud tellimus ja omavahelistes virtuaalse võrgu VNET2, klõpsake nuppu OK.

    ![Link VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Kui see VNET silmitsemine link on loodud. Lingi olekut saate vaadata järgmiselt:

    ![Lingi oleku](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Järgmine luua VNET silmitsemine linki, et VNET1 VNET2. Virtuaalne võrkude tera, valige VNET2, klõpsake nuppu Peerings ja seejärel käsku Lisa

    ![Vastastikune muude VNet kaudu](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Lisada silmitsemine blade silmitsemine lingi nimi LinkToVnet1, valige see tellimus ja omavahelistes virtuaalse võrgu, klõpsake nuppu OK.

    ![Virtuaalne paanil loomine](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Kui see VNET silmitsemine link on loodud. Lingi olekut saate vaadata järgmiselt:

    ![Lõplik lingi oleku](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Riigi otsida LinkToVnet2 ja nüüd tabelduskohta ühendatud ka.  

    ![Lõplik lingi oleku 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] VNET silmitsemine ainult kindlaks, kui mõlemad lingid on ühendatud.

On paar konfigureeritav atribuudid iga link.

|Suvand|Kirjeldus|Vaikimisi|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Kas aadress omavahelistes VNet Virtual_network sildi lisada.|Jah|
|AllowForwardedTraffic|Liikluse piilus VNet pärit on kas aktsepteeritud või lähevad|Ei|
|AllowGatewayTransit|Võimaldab omavahelistes VNet VNet lüüsi kasutamiseks|Ei|
|UseRemoteGateways|Kasutage oma omavahelistes VNet lüüsi. Omavahelistes VNet peab olema konfigureeritud lüüsi ja AllowGatewayTransit oleks märgitud. Ei saa kasutada seda suvandit, kui teil on konfigureeritud lüüsi|Ei|

Iga link VNet silmitsemine on ülaltoodud atribuutide kogum. Portaalist saate linki VNet silmitsemine ja mis tahes saadaolevate suvandite muutmine, klõpsake nuppu Salvesta efekti muutmine.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2. Selles näites me kasutame kahe tellimuste A ja B ning kaks kasutajat UserA ja UserB privileegidega tellimused sisse vastavalt
3. Portaalis, klõpsake nuppu Sirvi, valige virtuaalse võrgu. Klõpsake soovitud VNET ja klõpsake nuppu Lisa.

    ![Stsenaarium 2 sirvimine](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Klõpsake nuppu Lisa juurdepääsu enne, valige roll, ja valige võrgu kaasautor, klõpsake käsku Lisa kasutajad, tippige UserB logi nime ja klõpsake nuppu OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    On see pole kohustuslik, silmitsemine saab kindlaks isegi siis, kui kasutajad eraldi tõsta silmitsemine taotlusi nende vastavate Vnets kui taotlused vastavad. Kohaliku VNet kasutajate lisamine muude VNet õigustega kasutaja on hõlpsam häälestamine portaalis.

5. Seejärel Logi Azure portaali UserB, kellel on õigus kasutaja jaoks SubscriptionB. Järgige ülaltoodud juhiseid UserA võrgu kaasautori lisada.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] Saate välja logida ja brauseris tagamaks, et loa edukalt lubatud nii kasutaja seansi sisse logida.

6. Logi sisse portaali UserA, liikuge VNET3 tera, klõpsake Peering, märkige ruut "tean, et minu ressursi ID" ruut ja tippige ressursi ID VNET5 sisse, vorming.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Ressursi ID-d](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. UserB ja järgige eespool samm silmitsemine lingi loomiseks VNET5 VNet3 portaali sisse logida.

    ![Ressursi ID 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Silmitsemine luuakse ja mis tahes virtuaalse masina VNet3 peaks oskama suhelda mis tahes virtuaalse masina VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Esimese sammuna VNET silmitsemine lingid HubVnet VNET1. Pange tähele, et lubada edasisaatmist liikluse ruut on märkimata lingi.

    ![Tavaline silmitsemine](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Järgmise sammuna silmitsemine kaudu VNET1 HubVnet saab linkida. Pange tähele, et oleks valitud suvand Luba edasisaatmist liikluse.

    ![Tavaline silmitsemine](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Pärast silmitsemine on loodud, saate vaadake käesoleva [artikli](virtual-network-create-udr-arm-ps.md) ja kasutaja määratletud Route(UDR) kaudu virtuaalse seadme oma võimaluste kasutamiseks VNet1 liikluse ümber määratleda. Kui määrate marsruutimiseks järgmise Hop aadressi, saate häälestada selle partneri VNet HubVNet virtuaalse seadme IP-aadress


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.

2. Selle stsenaariumi silmitsemine VNET loomiseks peate looma ainult üks link, Azure ressursihaldur, et see klassikaline virtuaalse võrgust. See tähendab, et **VNET2** **VNET1** . Portaalis, klõpsake nuppu **Sirvi** > **Virtuaalse võrgu** valimine

3. Valige labale virtuaalne võrkude **VNET1**. Klõpsake **Peerings**ja seejärel klõpsake nuppu **Lisa**.

4. Lisada silmitsemine blade nimi oma link. Tehke seda nimetatakse **LinkToVNet2**. Valige jaotises üksikasjad omavahelistes **klassikaline**.

5. Valige soovitud tellimus ja omavahelistes virtuaalse võrgu **VNET2**. Klõpsake nuppu OK.

    ![Vnet1 linkimine Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Kui see VNet silmitsemine link on loodud, on kahe virtuaalse võrgu piilus ja saab, leiate järgmistest:

    ![Silmitsemine ühenduse kontrollimine](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>VNet silmitsemine eemaldamine

1.  Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2.  Avage virtuaalse võrgu tera, klõpsake Peerings, klõpsake linki, mille soovite eemaldada, klõpsake nuppu Kustuta.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Kui eemaldate ühe lingi VNET silmitsemine, omavahelistes lingi oleku läheb lahti.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. Selles olekus ei saa uuesti luua lingi kuni omavahelistes link olek muutub algataja. Soovitame enne uuesti luua VNET silmitsemine eemaldada nii linke.
