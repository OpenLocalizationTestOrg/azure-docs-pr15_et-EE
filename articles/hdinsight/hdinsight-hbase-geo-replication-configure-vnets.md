<properties 
   pageTitle="Konfigureerimine VPN-ühenduse, virtuaalse ühest võrgust | Microsoft Azure'i" 
   description="Saate teada, kuidas konfigureerida VPN-ühendused ja domeeninimede lahendamine kahe Azure virtuaalse võrgu vahel ja kuidas konfigureerida HBase geo-dispersioonanalüüs." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Kahe Azure virtuaalse võrgu vahel virtuaalse privaatvõrgu konfigureerimine  

> [AZURE.SELECTOR]
- [VPN ühenduvuse konfigureerimine](hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS-i konfigureerimine](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigureerimine HBase dispersioonanalüüs](hdinsight-hbase-geo-replication.md) 

Azure virtuaalse-saidilt võrguühendus kasutab secure tunneliga, kasutades Ipsec/IKE VPN-lüüsi. Funktsiooni VNets võib olla erinevad tellimused ja eri piirkondadest. Isegi saate ühendada VNet VNet suhtlemine mitme saidi konfiguratsioone. On mitmeid põhjuseid VNet VNet Ühenduvus.

- Piirkonna geo-koondamise ja geo-kohalolek 
- Piirkondliku mitmekihilise rakenduste tugev eraldamise äärist 
- Tellimuse ettevõtte vaheline side Azure Cross

Lisateavet leiate teemast [konfigureerimine on VNet VNet ühendus](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

See video:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Selles õpetuses on [sarja] [ hdinsight-hbase-replication] HBase geo-dispersioonanalüüs loomise kohta. 

- Konfigureerimine virtuaalse Privaatvõrgu ühendus kahe virtuaalse võrgu (selles õpetuses) vahel
- [Virtuaalne võrkude DNS-i konfigureerimine][hdinsight-hbase-geo-replication-dns]
- [Konfigureerimine HBase geo dispersioonanalüüs][hdinsight-hbase-geo-replication]

Järgmine diagramm näitab kahe virtuaalse võrgu, mis on selles õpetuses loote.

![Hdinsightiga HBase dispersioonanalüüs virtuaalse võrgustikudiagramm][img-vnet-diagram]
 

##<a name="prerequisites"></a>Eeltingimused
Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Töökoha Azure PowerShelli abil**.

    Enne käivitamist PowerShelli skriptide, veenduge, et olete loonud ühenduse Azure tellimuse abil järgmine cmdlet-käsk:

        Add-AzureAccount

    Kui teil on mitu Azure tellimust, abil järgmine cmdlet-käsk praeguse tellimuse:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Azure'i teenuste nimed ja virtuaalse masina nimed peavad olema kordumatud. Selles õpetuses kasutatav nimi on Contoso-[Azure teenuse/VM nimi]-[EU / U.S.]. Näiteks Contoso-VNet-EL on Azure virtuaalse võrgu Põhja-Euroopa andmekeskuse; Contoso-DNS-i-USA on Ida-USA andmekeskuse serveri DNS-i VM. Peab tulla oma nimed.
 

##<a name="create-two-azure-vnets"></a>Looge kaks Azure'i VNets



**Luua nimega Contoso-VNet-EL Põhja-Euroopa virtuaalse võrgu**

1.  [Azure'i klassikaline portaali]sisselogimine[azure-portal].
2.  Klõpsake nuppu **Uus**, **võrguteenuste**, **VIRTUAALSE võrgu**, **luua kohandatud**.
3.  Sisestage:

    - **Nimi**: Contoso-VNet-EL
    - **Asukoht**: Põhja Euroopa

        Selle õpetuse kasutab Põhja-Euroopa ja Ida-USA andmekeskuste. Soovi korral saate oma andmekeskuste.
4.  Sisestage:

    - **DNS-i SERVER**: (Jäta tühjaks) 
    
        Peate oma DNS-i server nimelahendus virtuaalse võrgu sees. Lisateavet kohta, millal kasutada antud Azure'i nimelahendus ja millal kasutada oma DNS-i server, vt [Nimi eraldusvõime (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Konfigureerida nimelahendus vahel VNets juhiste saamiseks vaadake teemat [Konfigureerimine DNS-i kahe Azure virtuaalse võrgu vahel][hdinsight-hbase-dns].
  
    - **Konfigureerimine punkti saidi VPN**: (tühi)

        Sel juhul ei kehti punkti – saidile.

    - **VPN saitide konfigureerimine**: (tühi)
    
        Ida-USA andmekeskuses tuleb konfigureerida Azure virtuaalse võrgu-saidilt VPN-ühendus.
5.  Sisestage:

    -   **AADRESSIRUUMI alates IP**: 10.1.0.0
    -   **Aadress ruumi CIDR**: / 16
    -   **Alamvõrgu-1 alates IP**: 10.1.0.0
    -   **Alamvõrgu-1 CIDR**: / 24

    Aadress ruumi saate USA virtuaalse võrguga ei kattuks.  

**Luua nimega Contoso-VNet-EL Lääne-Euroopa virtuaalse võrgu**

- Korrake seda toimingut viimase järgmine:

    - **Nimi**: Contoso-VNet-USA
    - **Asukoht**: Ida-USA
     
    - **DNS-i SERVER**: (Jäta tühjaks)
    - **Konfigureerimine punkti saidi VPN**: (tühi)
    - **VPN saitide konfigureerimine**: (tühi)
     
    - **AADRESSIRUUMI alates IP**: 10.2.0.0
    - **Aadress ruumi CIDR**: / 16
    - **Alamvõrgu-1 alates IP**: 10.2.0.0
    - **Alamvõrgu-1 CIDR**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Kahe VNets vahel virtuaalse privaatvõrgu konfigureerimine

###<a name="create-local-networks"></a>Kohaliku võrgu loomine

Kui loote mõnda VNet VNet konfiguratsiooni, peate iga VNet üksteise tuvastamiseks kohtvõrgu saitide konfigureerimine. Selles jaotises saate konfigureerida iga VNet kohalikus võrgus. Kohaliku võrgu ühiskasutusse vastavate VNet sama IP-aadressi tühikuid.

![Azure'i VPN-saitide konfigureerimine - Azure'i kohaliku võrgu konfigureerimine][img-vnet-lnet-diagram]


**Luua kohtvõrgu nimega Contoso-LNet-EL sobitamine Contoso-VNet-EL võrgu aadressiruumi**

1. Azure'i klassikaline portaalis nuppu **Uus**, **Võrguteenuste**, **VIRTUAALSE võrgu** **Lisamine kohalikus võrgus**.
3. Sisestage:

    - **Nimi**: Contoso-LNet-EL
    - **VPN seadme IP-aadress**: 192.168.0.1 (see aadress värskendatakse hiljem)

        Tavaliselt oleks tegelik välise IP-aadressi kasutatakse VPN seade. Jaoks VNet VNet konfiguratsioone abil, kasutage VPN gateway IP-aadress. Et teil pole loodud VPN lüüside kahe VNets jaoks, sisestage arbitary IP-aadress ja ta tagasi tuleb seda parandada.
4.  Sisestage:

    - **Aadress ruumi alates IP:** 10.1.0.0
    - **Aadress ruumi CIDR:** /16
    
    See peab vastama täpselt teie määratud varem Contoso-VNet-EL vahemiku.

**Luua kohtvõrgu nimega Contoso-LNet-USA sobitamine VNet-USA-Contoso network aadressiruumi**

- Korrake seda toimingut viimase järgmiste parameetrite abil:

    - **Nimi**: Contoso-LNet-USA
    - **VPN seadme IP-aadress**: 192.168.0.1 (see aadress värskendatakse hiljem)
     
    - **AADRESSIRUUMI alates IP**: 10.2.0.0
    - **Aadress ruumi CIDR**: / 16


###<a name="create-vpn-gateways"></a>VPN lüüside loomine

Selle konfiguratsiooni on kaks osa. Esmalt saate konfigureerida VNet saidilt ühenduse kohtvõrgu ja seejärel luua dünaamilisi marsruutimise VPN. Et VNet VNet nõuab Azure'i VPN lüüside koos dünaamilise marsruutimise VPN. Azure'i staatiline marsruutimine VPN ei toetata.

**Contoso-LNet-USA Contoso-VNet-EL-saidilt ühenduse konfigureerimine**

1.  Azure klassikaline portaali, klõpsake vasakul paanil **võrkude**
2.  Klõpsake **Contoso-VNet-EL**.
3.  Klõpsake vahekaarti **CONFIGUE** .
4.  Märkige ruut **Loo ühendus kohaliku võrgu**.
5.  **Kohalikus võrgus**, valige **Contoso-LNet-US**.
6.  Klõpsake jaotises virtuaalse võrgu aadress tühikute **lisamine lüüsi alamvõrgu** .
7.  Klõpsake nuppu **Salvesta**.
8.  Kinnitamiseks klõpsake nuppu **OK** .


**Contoso-VNet-EL VPN-lüüsi loomiseks**

1.  Azure'i klassikaline portaalis, klõpsake vahekaarti **ARMATUURLAUA** .
4.  Klõpsake lehe allservas **LÜÜSI loomine** ja klõpsake **Dünaamiline marsruutimist**.
5.  Klõpsake nuppu **Jah** kinnitamiseks. Pange tähele, et lüüsi pilti lehel muutub kollane ja öeldakse, et lüüsi loomine. Tavaliselt võtab aega umbes 15 minutit lüüsi loomiseks.

    Kui lüüsi olek muutub ühenduse loomine, iga Gateway IP-aadress on nähtav armatuurlaual. Kirjutage IP-aadress, mis vastab iga VNet, jälgides, et mitte segada nende abil. Need on kohatäite IP-aadresside redigeerimisel VPN seadme kohaliku võrkudes kasutatud IP-aadressid.

6.  Tehke koopia **GATEWAY IP-aadress**. Selle abil kuvatakse konfigureerimine Contoso-VNet-EL järgmises jaotises VPN gateway IP-aadress.

**Contoso-VNet-EL VPN-lüüsi loomiseks**

- Korrake seda konfigureerida Contoso-VNet-USA-saidilt ühenduvuse Contoso-LNet-EL ja menüüloendis VPN-lüüsi Contoso-Vnet-USA kaks viimast toimingut. Kui olete lõpetanud, on teil VPN gateway IP-aadressi jaoks Contoso-VNet-US.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>VPN seade kohaliku võrgu IP-aadressid
Viimase jaotises luua VPN-lüüsi iga soovitud VNets. Olete saanud VPN lüüside IP-aadressid. Nüüd saate minna tagasi konfigureerimine kohtvõrgu VPN seadme IP-aadressid.

**Konfigureerimiseks VPN seadme IP-aadressi jaoks Contoso-LNet-EL** 

1.  Azure'i klassikaline portaalis, klõpsake vasakul paanil **võrkude** .
2.  **Kohaliku võrgu** nuppu algusest.
3.  Klõpsake **Contoso-LNet-EL**ja seejärel nuppu **Redigeeri** all.
4.  Värskendage **VPN seadme IP-aadress**.  See on saate ARMATUURLAUA vahekaardil Contoso-VNET-EL-aadress.
5.  Klõpsake paremal.
6.  Klõpsake nuppu kontrolli.

**Contoso-LNet-USA VPN seadme IP-aadressi konfigureerimine** 

- Korrake seda toimingut viimase konfigureerimiseks Contoso-LNet-USA VPN seadme IP-aadress.

###<a name="set-vnet-gateway-keys"></a>Määrake VNet lüüsi võtmed

Lüüside Vnet ühiskasutusega klahvi abil autentida virtuaalne võrkude vahelisi seoseid. Võti ei saa konfigureerida Azure klassikaline portaalist. Kasutage PowerShelli või .NET SDK.

**Kui soovite seada võtmed**

1. Avage oma töökoha **Windows PowerShell ISE** või Windows PowerShelli konsooli.
2. Värskendage Jälgi seda skripti parameetrid ja käivitage see.

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Märkige ruut VPN-ühendus 

Ilma ühtegi VMs juurutatud soovitud VNets, saate virtuaalse võrgu visuaalset skeemi VNet armatuurlaualehe Azure'i klassikaline portaalis ühenduse oleku:

![Hdinsightiga HBase dispersioonanalüüs virtuaalse võrgu VPN-ühenduse olekut][img-vpn-status]
  



##<a name="next-steps"></a>Järgmised sammud

Selles õpetuses olete õppinud, kuidas konfigureerida VPN-ühendus kahe Azure virtuaalse võrgu vahel. Muud kaks artikleid sarja kuuluvad:

- [DNS-i konfigureerimine kahe Azure virtuaalse võrgu vahel][hdinsight-hbase-geo-replication-dns]
- [Konfigureerimine HBase geo dispersioonanalüüs][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
