<properties 
   pageTitle="Lüüsi punkti saidi VPN-ühenduse virtuaalse võrguühendust ressursihaldur juurutamise mudeli ja Azure portaali konfigureerimine | Microsoft Azure'i"
   description="Turvaliselt ühendada Azure virtuaalse võrgu ressursihaldur ja Azure portaali abil punkti saidi VPN-lüüsi ühenduse loomise teel."
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
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Punkti saidi VNet, mis on Azure portaalis ühenduse konfigureerimine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassikaline - Azure'i portaal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

SharePointi saidil (P2S) konfiguratsioon võimaldab teil luua turvalist ühendust üksikute klientarvutist virtuaalse võrku. P2S ühendus on kasulik, kui soovite ühendada oma VNet mujalt, nagu kodus või konverentsi, või kui teil on vaid mõned kliendid, kes on vaja virtuaalse võrguga. 

Punkti saidi ühendused, pole vaja VPN seadme või avaliku IP-aadressi töötamiseks. VPN-ühendus on loodud ühendus alates klientarvuti. Punkti saidi ühenduste kohta leiate lisateavet teemast [VPN lüüsi KKK](vpn-gateway-vpn-faq.md#point-to-site-connections) ja [kavandamisel ja kujundus](vpn-gateway-plan-design.md).

Selles artiklis tutvustatakse loomise on VNet punkti saidi ühendusega ressursihaldur juurutamise mudeli Azure'i portaalis.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Juurutamise mudelite ja P2S ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Järgmises tabelis on kaks juurutamise mudelit ja saadaval juurutamise meetodid P2S konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse selle tabeli.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Tavaline töövoo 

![Punkti-abil – saidile-skeemi] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punkti – saidile")

### <a name="example"></a>Näide väärtusi

- **Nimi: VNet1**
- **Aadress: 192.168.0.0/16**<br>Selles näites kasutatakse ainult üks aadressiruumi. Saate määrata rohkem kui üks aadressiruumi oma VNet.
- **Alamvõrgu nimi: FrontEnd**
- **Alamvõrgu aadress vahemiku: 192.168.1.0/24**
- **Tellimus:** Kui teil on mitu tellimust, veenduge, et kasutate õige.
- **Ressursirühm: TestRG**
- **Asukoht: Ida-USA**
- **GatewaySubnet: 192.168.200.0/24**
- **Virtuaalne võrgu lüüsi nimi: VNet1GW**
- **Lüüsi tüüp: VPN**
- **VPN-tüüp: marsruutimiseks vastavalt**
- **Avaliku IP-aadress: VNet1GWpip**
- **Ühenduse tüüp: punkti – saidile**
- **Kliendi aadressi uudiseid: 172.16.201.0/24**<br>VPN-i kliendid, kes ühenduse VNet, kasutamine punkti saidi seoses saadud kliendi aadressi uudiseid IP-aadress.

## <a name="before-beginning"></a>Enne alustamist

- Veenduge, et teil on Azure tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>Osa 1 - virtuaalse võrgu loomine

Kui loote selle konfiguratsiooni teostamiseks, võib viidata [näidisväärtused](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2 täiendavad aadressiruumi ja alamvõrku lisamine

Saate lisada täiendavad aadressiruumi ja alamvõrku oma VNet pärast selle loomist.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. lüüsi alamvõrgu loomine

Enne virtuaalse võrgu ühenduse lüüs, peate esmalt lüüsi alamvõrgu virtuaalse võrgu, millele, millega soovite ühenduse luua. Võimaluse korral on parem luua lüüsi alamvõrku, kasutate CIDR plokk /28 või /27 tulevaste konfigureerimise nõuded mahutamiseks piisavalt IP-aadressid.

Kuvatõmmised selles jaotises antakse leheviite näide. Kindlasti kasutamiseks GatewaySubnet aadress vahemik, mis vastab teie konfiguratsiooni nõutav väärtustega.

#### <a name="to-create-a-gateway-subnet"></a>Lüüsi alamvõrgu loomiseks

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. määrata DNS-i serveris (valikuline)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Osa 2 - virtuaalse võrgu lüüsi loomine

Punkti – saidile ühendused nõuavad järgmised sätted:

- Lüüsi tüüp: VPN
- VPN-tüüp: marsruutimiseks vastavalt

### <a name="to-create-a-virtual-network-gateway"></a>Lüüsi virtuaalse võrgu loomine

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Osa 3 - luua serdid

Serte kasutatakse Azure autentida VPN kliente VPN punkti saidi jaoks. Eksportimist avaliku serdi andmed (mitte privaatvõti) nii, nagu Base – 64 X.509 CER-fail juurkausta serdi loodud lahenduse ettevõtte sert või juurkausta iseallkirjastatud sert. Seejärel importida avaliku sertifikaadi andmed juur sertifikaadi Azure. Lisaks on vaja luua kliendi sertifikaadi juur sertifikaadi klientidele. Iga kliendi, mis soovib P2S kaudu virtuaalse võrguga ühenduse loomiseks peab olema kliendi installitud juurkausta serdi põhjal loodud.

### <a name="getcer"></a>1. saamiseks serdi juurkausta CER-faili

Kui kasutate lahenduse enterprise, saate oma olemasolev sert ahelas. Kui te ei kasuta ettevõte CA lahenduse, saate luua iseallkirjastatud juurkausta cert. Iseallkirjastatud cert loomise meetod on makecert.

- Kui kasutate mõnda ettevõtte sert süsteemi, saada CER-faili jaoks juurkausta sert, mida soovite kasutada. 

- Kui te ei kasuta lahenduse ettevõtte sert, peate juurkausta iseallkirjastatud serdi loomiseks. Windows 10 juhised, võib viidata [töötamine iseallkirjastatud juursertide punkti saidi jaoks](vpn-gateway-certificates-point-to-site.md).

1. Saada sert CER-fail, avage **tekst certmgr.msc** ja leidke juurkausta sert. Paremklõpsake root iseallkirjastatud serdi, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**. **Serdi ekspordiviisardi**avaneb.

2. Viisardi, klõpsake nuppu **edasi**, valige **ei, ekspordi privaatvõti**ja seejärel klõpsake nuppu **edasi**.

3. Lehel **Ekspordi failivorming** valige **Base-64-kodeeritud X.509 (. CER).** Klõpsake nuppu **edasi**. 

4. Klõpsake selle **faili eksportida**, **liikuge** asukohta, kuhu soovite eksportida sert. Serdi faili nimi **faili nimi**. Klõpsake nuppu **edasi**.

5. Klõpsake nuppu **valmis** serdi eksportida.

### <a name="generateclientcert"></a>2 luua kliendi sert

Kas saate luua kordumatu sert iga kliendi, mis loob ühenduse või kasutada mitme kliendi serti. Võimalus tühistada ühe serdi vajaduse korral on see eelis, nendele kordumatu kliendi serdid. Muul juhul, kui kõik kasutab sama kliendi serti ja leiate, et peate ühe kliendi serdi tühistada, pead luua ja installige kõik kliendid, mida kasutada seda serti autentida uus.

- Kui kasutate lahenduse ettevõtte sert, luua kliendi serti levinud nimi väärtus vorming 'name@yourdomain.com', asemel "domeeni name\username" vorming. 

- Kui kasutate iseallkirjastatud serdi, vt [töötamine iseallkirjastatud juursertide punkti saidi jaoks](vpn-gateway-certificates-point-to-site.md) luua kliendi serti.

### <a name="exportclientcert"></a>3. ekspordi kliendi sert

Kliendi sert on vaja autentimist. Pärast genereerimine kliendi sert, eksportida. Kliendi serdi eksportimist installitakse hiljem igas klientarvutis.

1. Eksportida klient, saate *tekst certmgr.msc*. Paremklõpsake kliendi sert, mida soovite eksportida, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**.
2. Eksportida privaatvõti kliendi sert. See on *pfx-* fail. Veenduge, et salvestada või parool (võti), mida saate seada selle serdi meeles pidada.

## <a name="addresspool"></a>Osa 4 - Kliendi aadressi uudiseid lisamine

1. Kui virtuaalse võrgu lüüsi on loodud, liikuge virtuaalse võrgu lüüsi tera jaotise **sätted** . Klõpsake jaotises **sätted** **SharePointi saidil konfiguratsiooni** **konfiguratsiooni** tera avamiseks.

    ![Valige käsk saidi blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "Valige käsk saidi blade")

2. **Aadress pool** on kogumi IP-aadressid, kust saavad kliendid, et ühendust IP-aadress. Lisage rakenduskausta aadress ja klõpsake siis nuppu **Salvesta**.

    ![Kliendi aadressi uudiseid] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "Kliendi aadressi uudiseid")

## <a name="uploadfile"></a>Osa 5 – juurkausta serdi CER-faili üleslaadimine

Pärast lüüsi on loodud, saate üles laadida CER-faili jaoks usaldusväärne juur serdi Azure. Saate üles laadida kuni 20 juursertide failid. Privaatvõti juurkausta serti ei saa Azure üles laadida. Kui CER-fail on üles laaditud, Azure'i kasutab seda autentida klientides virtuaalse võrguga ühenduse luua.

1. Liikuge **SharePointi saidil konfiguratsiooni** tera. Lisate CER-failide selle tera **serdi** osas.

    ![Valige käsk saidi blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "Valige käsk saidi blade")

2. Veenduge, et nii, nagu Base – 64 X.509 (CER) faili eksportida juurkausta sert. Peate selle selles vormingus eksportida, et saate avada serdi tekstiredaktorit.
3. Avage tekstiredaktoris, nt Notepadis sert. Kopeerige ainult järgmisest jaotisest.

    ![serdi andmed] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "serdi andmed")

4. Kleepige andmed sertifikaadi portaali **Avaliku sertifikaadi andmete** osas. Panna nime serdi **nimi** ruumi ja klõpsake siis nuppu **Salvesta**. Saate lisada kuni 20 usaldusväärsed juursertide.

    ![serdi üleslaadimine] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "serdi üleslaadimine")

## <a name="clientconfig"></a>Laadige alla ja installige VPN kliendi konfiguratsiooni paketi osa 6 –

Kliendid Microsoft Azure'i abil P2S peab olema kliendi serti ja VPN kliendi konfiguratsiooni pakett installitud. VPN kliendi konfiguratsiooni on saadaval Windows klientidele. 

VPN kliendi pakett sisaldab teavet VPN klienttarkvara, mis on Windowsi sisse ehitatud konfigureerimiseks. Konfiguratsiooni on VPN, mida soovite ühendada. Paketi ei saa installida täiendavat tarkvara. Vt lisateavet [VPN lüüsi KKK](vpn-gateway-vpn-faq.md#point-to-site-connections) .

1. **SharePointi saidil konfiguratsiooni** labale **allalaadimine VPN klient** **alla laadida VPN klient** tera avamiseks klõpsake.

    ![VPN klient allalaadimine] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "VPN klient allalaadimine")

2. Valige õige paketi oma kliendi ja seejärel klõpsake nuppu **Laadi alla**. 64-bitine klienti, valige **AMD64**. 32-bitine klienti, valige **x86**.

3. Installige pakett klientarvutis. Kui teil tekib SmartScreen popup, klõpsake **Lisateabe saamiseks**, seejärel selleks, et installida paketi **käivitamine ikkagi** .

4. Klientarvuti, liikuge **Võrgu sätteid** ja klõpsake **VPN**. Kuvatakse loendis ühendus. See näitab nimi ja virtuaalse võrk, mis loob ühenduse näeb välja umbes selles näites: 

    ![VPN klient] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN klient")

## <a name="installclientcert"></a>Osa 7 - kliendi serdi installimine

Iga klientarvuti peab olema kliendi serti autentimiseks. Kliendi sert installimisel peate kliendi serdi eksportimisel loodud parool.

1. Kopeerige klientarvuti pfx-fail.
2. Topeltklõpsake selle installimiseks pfx-faili. Muutke installimise kohta.

## <a name="connect"></a>Osa 8 - ühenduse Azure'i

1. Ühenduse loomiseks oma VNet klientarvuti, liikuge VPN-ühendused ja otsige üles loodud VPN-ühendus. See on nime virtuaalse võrgu sama nimi. Klõpsake nuppu **Loo ühendus**. Hüpikaknas kuvatakse võidakse kuvada, mis viitab serdi abil. Kui see juhtub, klõpsake nuppu **Jätka** kasutada administraatoriõigusi. 

2. **Ühenduse** oleku lehel käsku **Ühenda** ühenduse alustamiseks. Kui kuvatakse **Serdi valimine** Kuva, veenduge, et kliendi sert, kus on üks, mida soovite kasutada ühenduse. Kui see pole õige serdi valimiseks kasutage ripploendi noolt ja seejärel klõpsake nuppu **OK**.

    ![VPN klient 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Kliendi VPN-ühendus")

3. Nüüd peaks teie ühendust luua.

    ![VPN klient 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN-ühendus kliendi 2")

## <a name="verify"></a>Osa 9 - ühenduse kontrollimine

1. Veenduge, et VPN-ühendus on aktiivne, avage administraatoriõigustes Käsuviip ja käivitage *ipconfig/kõik*.

2. Tulemusi vaadata. Teade, et olete saanud IP-aadress on ühe maksimaalselt punkti saidi VPN kliendi aadressi Pool konfiguratsioonis määratud aadressi. Tulemused peaks olema umbes selline:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="add"></a>Lisamiseks või eemaldamiseks usaldusväärsed juursertide

Lülitu serdi eemaldamiseks Azure. Kui eemaldate usaldusväärne sert, mis pärinevad juur serdi enam saab ühenduse Azure'i punkti saidi kaudu. Kui soovite luua, tuleb installida sertifikaadi, mis on usaldusväärsed Azure on loodud uue kliendi serti.

Saate hallata loendi tühistatud kliendi sertide **SharePointi saidil konfiguratsiooni** tera. See on tera, mida kasutasite [lülitu serdi üleslaadimine](#uploadfile).

## <a name="revokeclient"></a>Tühistatud kliendi sertide loendi haldamine

Saate tühistada kliendi serdid. Sertide tühistusloendid võimaldab valikuliselt keelamiseks punkti saidi ühenduvuse üksikud kliendi sertide põhjal. Azure'i juurkausta serdi CER eemaldamisel selles muudatusi kõik kliendi sertide loodud/allkirjastatud sertifikaadiga tühistatud root access. Kui soovite tühistada konkreetse kliendi sert, mitte root, saate seda teha. Kehtivad nii serdid, root serdi loodud ikkagi. 

Levinud tava on root serdi abil saate hallata Accessi meeskonnatöö või ettevõtte tasemel, kasutades tühistatud kliendi sertide kohandatud Accessi juhtelemendi üksikkasutajate jaoks.

Saate hallata loendi tühistatud kliendi sertide **SharePointi saidil konfiguratsiooni** tera. See on tera kasutatud [lülitu serdi üleslaadimine](#uploadfile).


## <a name="next-steps"></a>Järgmised sammud

Saate lisada virtuaalse võrgu virtuaalse masina. Üksikasjalikud juhised leiate teemast [loomine virtuaalse masina](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .