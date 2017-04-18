<properties 
   pageTitle="Lüüsi punkti saidi VPN-ühenduse ressursihaldur juurutamise näidise virtuaalse võrgu konfigureerimine | Microsoft Azure'i"
   description="Turvaliselt ühendada Azure virtuaalse võrgu lüüsi punkti saidi VPN-ühenduse loomise teel."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Punkti saidi on VNet PowerShelli kaudu ühenduse konfigureerimine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassikaline - Azure'i portaal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

SharePointi saidil (P2S) konfiguratsioon võimaldab teil luua turvalist ühendust üksikute klientarvutist virtuaalse võrku. P2S ühendus on kasulik, kui soovite ühendada oma VNet mujalt, nagu kodus või konverentsi, või kui teil on vaid mõned kliendid, kes on vaja virtuaalse võrguga. 

Punkti saidi ühendused, pole vaja VPN seadme või avaliku IP-aadressi töötamiseks. VPN-ühendus on loodud ühendus alates klientarvuti. Punkti – saidile ühenduste kohta leiate lisateavet teemast [VPN lüüsi KKK](vpn-gateway-vpn-faq.md#point-to-site-connections) ja [planeerimine ja kujundus](vpn-gateway-plan-design.md). 

Selles artiklis tutvustatakse loomise on VNet punkti saidi ühendusega ressursihaldur juurutamise mudeli PowerShelli kaudu.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Juurutamise mudelite ja P2S ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Järgmises tabelis on kaks juurutamise mudelit ja saadaval juurutamise meetodid P2S konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse sellest tabelist.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Tavaline töövoo 

![Punkti-abil – saidile-skeemi] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punkti – saidile")

Selle stsenaariumi korral loote virtuaalse võrgu punkti saidi ühendusega. Juhised aitab teil luua sertifikaatide kohta, mis on vajalikud selle konfiguratsiooni. P2S ühenduse koosneb järgmistest põhjustest: A VNet VPN-lüüsi, root serdi CER-fail (avalik võti), kliendi serti ja kliendi konfiguratsiooni VPN. 

Selle konfiguratsiooni kasutame järgmised väärtused. Me määrata muutujate artikli jaotist [1](#declare) . Te saate kasutada juhiseid soovitud walk-through ja kasutada muutmata neid väärtusi või nende kajastamiseks keskkonna muutmiseks. 

### <a name="example"></a>Näide väärtused

- **Nimi: VNet1**
- **Aadress: 192.168.0.0/16** ja **10.254.0.0/16**<br>Selles näites kasutame rohkem kui üks aadressiruumi illustreerimiseks, et selle konfiguratsiooni töötab mitu aadressi tühikutega. Mitme aadressi tühikud pole vaja selle konfiguratsiooni.
- **Alamvõrgu nimi: FrontEnd**
    - **Alamvõrgu aadress vahemiku: 192.168.1.0/24**
- **Alamvõrgu nimi: kirjutamata**
    - **Alamvõrgu aadress vahemiku: 10.254.1.0/24**
- **Alamvõrgu nimi: GatewaySubnet**<br>Alamvõrgu nime *GatewaySubnet* on kohustuslik VPN-lüüsi töötamiseks.
    - **Alamvõrgu aadress vahemiku: 192.168.200.0/24** 
- **VPN-i kliendi aadressi uudiseid: 172.16.201.0/24**<br>VPN-i kliendid, kes ühenduse VNet, kasutamine punkti saidi seoses saadud VPN kliendi aadressi uudiseid IP-aadress.
- **Tellimus:** Kui teil on mitu tellimust, veenduge, et kasutate õige.
- **Ressursirühm: TestRG**
- **Asukoht: Ida-USA**
- **DNS-i Server: IP-aadress** DNS server, mida soovite kasutada nimelahendus.
- **GW nimi: Vnet1GW**
- **Avaliku IP nimi: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Enne alustamist

- Veenduge, et teil on Azure tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Installige uusim versioon Azure ressursihaldur PowerShelli cmdlet-käsud. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet. Kui töötate PowerShelli selle konfiguratsiooni puhul, veenduge, et kasutate administraatorina. 

## <a name="declare"></a>Osa 1 - Logi sisse ja muutujate määramine

Selles jaotises logige sisse ja selle konfiguratsiooni kasutatud väärtuste deklareerida. Funktsiooni näidisskriptid kasutatakse esitatud väärtused. Muutke vastavalt oma keskkonna väärtusi. Või saate kasutada esitatud väärtused ja läbida kasutamise juhiseid.

1. PowerShelli konsooli, logige sisse oma Azure'i kontosse. Selle cmdlet-käsu küsib teilt sisselogimise Azure'i kontosse. Pärast sisselogimist, allalaadimist oma konto sätted, et need on saadaval Azure PowerShelli.

        Login-AzureRmAccount 

2. Azure'i tellimuste loendi hankimine.

        Get-AzureRmSubscription

3. Määrake tellimus, mida soovite kasutada. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Deklareerida muutujad, mida soovite kasutada. Järgmises näites, asendades selle väärtused ise vajaduse korral kasutada.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Osa 2 - on VNet konfigureerimine 

1. Ressursi rühma loomine.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Saate luua virtuaalse võrgu, *FrontEnd*, *Taustväärtus*ja *GatewaySubnet*neile nime panemise alamvõrgu konfiguratsioone. Nende eesliiteid peab kuuluma VNet aadressiruumi, mida te deklareeritud.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Saate luua virtuaalse võrgu. DNS-i server määratud peaks olema DNS-i server, mis võib lahendada loote ressursside nimed. Selles näites me kasutada avaliku IP-aadressi. Ärge unustage kasutada oma väärtused.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Määrake loodud virtuaalse võrgu muutujaid.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Koosolekukutse dünaamiliselt avaliku IP-aadressi. IP-aadress on vaja lüüsi töötaks õigesti. Hiljem vahel ühenduse lüüs lüüsi IP konfigureerimine.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Osa 3 - serdid

Serte kasutatakse Azure autentida VPN kliente VPN punkti saidi jaoks. Eksportimist avaliku serdi andmed (mitte privaatvõti) nii, nagu Base – 64 X.509 CER-fail juurkausta serdi loodud lahenduse ettevõtte sert või juurkausta iseallkirjastatud sert. Seejärel importida serdi avalike andmete juurkausta serdi Azure. Lisaks on vaja luua kliendi serti juurkausta serdi klientidele. Iga kliendi, mis soovib P2S kaudu virtuaalse võrguga ühenduse loomiseks peab olema kliendi installitud juurkausta serdi põhjal loodud.

### <a name="cer"></a>1. saamiseks serdi juurkausta CER-faili

Peate saamiseks serdi avalike andmete juurkausta sert, mida soovite kasutada.

- Kui kasutate mõnda ettevõtte sert süsteemi, saada CER-faili jaoks juurkausta sert, mida soovite kasutada. 

- Kui te ei kasuta lahenduse ettevõtte sert, peate juurkausta iseallkirjastatud serdi loomiseks. Windows 10 juhised, võib viidata [töötamine iseallkirjastatud juursertide punkti saidi jaoks](vpn-gateway-certificates-point-to-site.md).


1. Saada sert CER-fail, avage **tekst certmgr.msc** ja leidke juurkausta sert. Paremklõpsake root iseallkirjastatud serdi, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**. **Serdi ekspordiviisardi**avaneb.

2. Viisardi, klõpsake nuppu **edasi**, valige **ei, ekspordi privaatvõti**ja seejärel klõpsake nuppu **edasi**.

3. Lehel **Ekspordi failivorming** valige **Base-64-kodeeritud X.509 (. CER).** Klõpsake nuppu **edasi**. 

4. Klõpsake selle **faili eksportida**, **liikuge** asukohta, kuhu soovite eksportida sert. Serdi faili nimi **faili nimi**. Klõpsake nuppu **edasi**.

5. Klõpsake nuppu **valmis** eksportida sert.

### <a name="generate"></a>2 luua kliendi sert

Järgmiseks luua kliendi serdid. Kas saate luua kordumatu sert iga kliendi, mis loob ühenduse või kasutada mitme kliendi serti. Saab kasutada kordumatu kliendi sertide loomisel on võimalus vajaduse korral ühe sertifikaadi tühistada. Muul juhul, kui kõik kasutab sama kliendi serti ja leiate, et peate ühe kliendi serdi tühistada, pead luua ja installige kõik autentida serdi kasutavate klientide uus. Kliendi serdid on hiljem selles töös iga kliendi arvutisse installitud.

- Ettevõtte sert lahenduse kasutamisel Loo kliendi sertifikaadi levinud nimi väärtus vorming 'name@yourdomain.com', asemel NetBIOS 'Domeen\kasutajanimi' vormindamine. 

- Kui kasutate iseallkirjastatud serdi, vt [töötamine iseallkirjastatud juursertide punkti saidi jaoks](vpn-gateway-certificates-point-to-site.md) luua kliendi serti.

### <a name="exportclientcert"></a>3. ekspordi kliendi sert

Kliendi sert on vaja autentimist. Pärast genereerimine kliendi sert, eksportida. Kliendi serdi eksportimist installitakse hiljem igas klientarvutis.

1. Kliendi sertifikaadi eksportida saate *tekst certmgr.msc*. Paremklõpsake kliendi sert, mida soovite eksportida, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**.
2. Eksportida privaatvõti kliendi sert. See on *pfx-* fail. Veenduge, et salvestada või parool (võti), mida saate seada selle serdi meeles pidada.

### <a name="upload"></a>4. Laadi juurkausta serdi CER-fail

Teie serdi nimi, asendades väärtus oma muutuja deklareerida:

        $P2SRootCertName = "Mycertificatename.cer"

Azure'i juurkausta serdi avaliku serdi andmeid lisada. Saate üles laadida kuni 20 juursertide failid. Privaatvõti juurkausta serti ei saa Azure üles laadida. Kui CER-fail on üles laaditud, Azure'i kasutab seda autentida klientides virtuaalse võrguga ühenduse luua. 

Asendage faili tee ise ja seejärel käivitage cmdlet-käsud.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Osa 4 - VPN-lüüsi loomine

Konfigureerige ja teie VNet virtuaalse võrgu lüüsi loomine. *-GatewayType* peab olema **Vpn** ja *-VpnType* peab olema **RouteBased**. See võib võtta kuni 45 minutit.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Osa 5 - VPN kliendi konfiguratsiooni Laadige

Kliendid Microsoft Azure'i abil P2S peab olema nii kliendi serti ja VPN kliendi konfiguratsiooni pakett installitud. VPN kliendi konfiguratsiooni on saadaval Windows klientidele. VPN kliendi pakett sisaldab teavet konfigureerida VPN klienttarkvara, mis on Windowsi sisse ehitatud ja on seotud VPN, mida soovite ühendada. Paketi ei saa installida täiendavat tarkvara. Vt lisateavet [VPN lüüsi KKK](vpn-gateway-vpn-faq.md#point-to-site-connections) .

1. Pärast lüüsi on loodud, saate kliendi konfiguratsiooni paketi alla laadida. Selles näites allalaadimist pakett 64-bitine klientidele. Kui soovite alla laadida 32-bitine klient, asendage "Amd64" "x86". Saate alla laadida ka VPN klient Azure portaali abil.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. PowerShelli cmdlet-käsk tagastab lingi URL-i. See on näide sellest, millised on tagastatud URL näeb välja umbes:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Kopeerige ja kleepige link, mis tagastatakse laadige oma brauseri. Seejärel installige pakett klientarvutis. Kui teil tekib SmartScreen popup, klõpsake **Lisateabe saamiseks**, seejärel selleks, et installida paketi **käivitamine ikkagi** .

4. Klientarvuti, liikuge **Võrgu sätteid** ja klõpsake **VPN**. Kuvatakse loendis ühendus. See näitab nimi ja virtuaalse võrk, mis loob ühenduse näeb välja umbes selles näites: 

    ![VPN klient] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN klient")


## <a name="clientcertificate"></a>Osa 6 - kliendi serdi installimine

Iga klientarvuti peab olema kliendi serti autentimiseks. Kliendi sert installimisel peate kliendi serdi eksportimisel loodud parool.

1. Kopeerige klientarvuti pfx-fail.
2. Topeltklõpsake selle installimiseks pfx-faili. Muutke installimise kohta.

## <a name="connect"></a>Osa 7 - ühenduse Azure'i

1. Ühenduse loomiseks oma VNet klientarvuti, liikuge VPN-ühendused ja otsige üles loodud VPN-ühendus. See on nime virtuaalse võrgu sama nimi. Klõpsake nuppu **Loo ühendus**. Hüpikaknas kuvatakse võidakse kuvada, mis viitab serdi abil. Kui see juhtub, klõpsake nuppu **Jätka** kasutada administraatoriõigusi. 

2. **Ühenduse** oleku lehel käsku **Ühenda** ühenduse alustamiseks. Kui kuvatakse **Serdi valimine** Kuva, veenduge, et kliendi sert, kus on üks, mida soovite kasutada ühenduse. Kui see pole õige serdi valimiseks kasutage ripploendi noolt ja seejärel klõpsake nuppu **OK**.

    ![Kliendi VPN-ühendus] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Kliendi VPN-ühendus")

3. Nüüd peaks teie ühendust luua.

    ![Ühendust luua] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Ühendust luua")

## <a name="verify"></a>Osa 8 - ühenduse kontrollimine

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

## <a name="addremovecert"></a>Lisamine või eemaldamine usaldusväärsete root sert

Serte kasutatakse autentida VPN kliente VPN punkti saidi jaoks. Järgmised toimingud teile lisamine ja eemaldamine juursertide kaudu. Azure'i Base64-kodeeringuga X.509 (CER) faili lisamisel te ei räägi Azure'i juurkausta sert, mida tähistab faili usaldada. 

Saate lisada või eemaldada usaldusväärsed juursertide PowerShelli abil või Azure'i portaalis. Kui soovite teha, kasutades Azure portaali, minge oma **virtuaalse võrgu lüüsi > Sätted > saidi punkti konfiguratsiooni > Root certificates**. Järgmised toimingud sõelub järgmised toimingud PowerShelli kaudu. 

### <a name="add-a-trusted-root-certificate"></a>Lülitu serdi lisamine

Saate lisada kuni 20 lülitu serdi CER failide Azure'i. Järgige alltoodud juurkausta sert.

1. Loomine ja uue üksuse juurkausta sert, mida tuleb lisada Azure ettevalmistamine. Avalik võti eksportimine nii, nagu Base – 64 X.509 (. CER) ja avage see tekstiredaktoris. Kopeerige ainult jaotise allpool näidatud. 
 
    Kopeerige väärtused, nagu on näidatud järgmises näites:

    ![sert] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "sert")
    
2. Määrata serdi nimi ja olulise teabe muutujana. Asendage teave oma, nagu on näidatud järgmine näide:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Uue üksuse juurkausta serdi lisamine. Korraga saab lisada ainult üks sert.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Saate kontrollida, kas uut serti lisati õigesti abil järgmine cmdlet-käsk.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Usaldusväärne juur sertifikaadi eemaldamine

Lülitu serdi eemaldamiseks Azure. Kui eemaldate usaldusväärne sert, mis pärinevad juur serdi enam saab ühenduse Azure'i punkti saidi kaudu. Kui soovite luua, tuleb installida uue kliendi sert, mis on loodud sert, mis on usaldusväärsed Azure.

1. Lülitu serdi eemaldamiseks muuta järgmises näites:

    Muutujaid deklareerida.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Serdi eemaldada.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Järgmine cmdlet-käsk abil saate kontrollida sert on edukalt eemaldatud. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Tühistatud kliendi sertide loendi haldamine

Saate tühistada kliendi serdid. Sertide tühistusloendid võimaldab valikuliselt keelamiseks punkti saidi ühenduvuse üksikute kliendi sertide põhjal. Azure'i juurkausta serdi CER eemaldamisel selles muudatusi kõik kliendi sertide loodud/allkirjastatud sertifikaadiga tühistatud root access. Kui soovite tühistada konkreetse kliendi sert, mitte root, saate seda teha. Kehtivad nii serdid, root serdi loodud ikkagi.

Levinud tava on root serdi abil saate hallata Accessi meeskonnatöö või ettevõtte tasemel, kasutades tühistatud kliendi sertide kohandatud Accessi juhtelemendi üksikkasutajate jaoks.

### <a name="revoke-a-client-certificate"></a>Kliendi serti tühistamine

1. Saada sõrmejälje kliendi sert tühistada.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Lisage soovitud sõrmejälje tühistatud sõrmejälje loendit.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Veenduge, et selle sõrmejälje lisati sertide tühistusloendid. Peate lisama ühe sõrmejälje korraga.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Kliendi serti taastada

Saate taastada, eemaldades selle sõrmejälje tühistatud kliendi sertide loendist kliendi serti.

1.  Tühistatud kliendi serdi sõrmejälje loendist eemaldada soovitud sõrmejälje.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Märkige ruut, kui selle sõrmejälje on tühistatud loendist eemaldada.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Järgmised sammud

Saate lisada virtuaalse võrgu virtuaalse masina. Üksikasjalikud juhised leiate teemast [loomine virtuaalse masina](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .