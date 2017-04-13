<properties 
   pageTitle="DNS-i sätted, mis määrab teenuse konfiguratsioonifail | Microsoft Azure'i"
   description="konfiguratsioonifail teenuse kasutamise virtuaalse võrgu kohandatud DNS-i sätete määramine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Konfiguratsioonifailis teenuse DNS-i sätete määramine

## <a name="dns-elements"></a>DNS-i elemendid

Teenuse konfiguratsiooni fail võib sisaldada DnsServers elemendi loendi IPv4 aadressid teenus kasutab süsteemi (DNS) serverite jaoks. Sätete teenus konfiguratsioonifailis ülimuslik võrgu konfigureerimise faili sätted. Lisateabe saamiseks lugege teemat [Azure teenuse konfiguratsioon skeemi (.cscfg fail)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Atribuut **name** **DnsServer** elementi kasutatakse ainult viite nimi. See ei kajasta hostinimi DNS-i serveri. Iga **DnsServer** atribuudi väärtus peab olema kordumatu kogu Microsoft Azure'i tellimuse ulatuses.

## <a name="see-also"></a>Vt ka

[Azure'i teenus konfiguratsioon skeemi (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure'i Virtual Network Configuration skeemi](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigureerimine virtuaalse võrgu Network Configuration failide kasutamine](http://go.microsoft.com/fwlink/?LinkId=248094)

[Haldusportaal virtuaalse võrgu sätete kohta](http://go.microsoft.com/fwlink/?LinkId=248092)

