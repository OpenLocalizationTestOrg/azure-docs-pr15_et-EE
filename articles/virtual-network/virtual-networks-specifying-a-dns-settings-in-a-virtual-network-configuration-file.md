<properties 
   pageTitle="DNS-i sätted, mis määrab virtuaalse võrgu konfigureerimise faili | Microsoft Azure'i"
   description="Kuidas muuta DNS-serveri sätete virtuaalse võrgu virtuaalse võrgu konfigureerimise faili klassikaline juurutamise mudeli kasutamine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Konfiguratsioonifailis virtuaalse võrgu DNS-i sätete määramine

Võrgu konfigureerimise faili on kaks elemendid, mille abil saate määrata süsteemi (DNS) sätted: **DnsServers** ja **DnsServerRef**. Saate lisada loendisse DNS-serverite täpsustades oma IP-aadressid ja **DnsServers** elemendi viide nimed. Seejärel saate määrata, millised DNS-serveri kirjed alates DnsServers element on kasutada erinevate võrgu saitide oma virtuaalse võrgustikus **DnsServerRef** element.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel.

Võrgu konfiguratsiooni fail võib sisaldada järgmisi elemente. Iga elemendi pealkirja on lingitud leht, mis annab Lisateavet elemendi väärtuse sätted.

>[AZURE.IMPORTANT] Konfiguratsioonifail võrgu konfigureerimise kohta leiate teavet teemast [konfigureerimine virtuaalse võrgu kaudu võrgu konfigureerimise faili](virtual-networks-using-network-configuration-file.md). Iga element failis võrgu konfigureerimise kohta leiate teemast [Azure virtuaalse võrgu konfiguratsioon skeemi](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[DNS-i Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] Atribuut **name** **DnsServer** elementi kasutatakse ainult viite **DnsServerRef** elemendi jaoks. See ei kajasta hostinimi DNS-i serveri. Iga **DnsServer** atribuudi väärtus peab olema kordumatu kogu Microsoft Azure'i tellimus üle

[Virtuaalse võrgu saitide Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Selleks, et määrata selle sätte virtuaalse võrgu saitide elemente, see peab olema eelnevalt määratletud DNS-i element. DnsServerRef *nimi* virtuaalse võrgu saitide elementi peavad viitama määratud DNS-i elementi DnsServer *nimi*nimi väärtus.

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i Virtual Network Configuration skeemi](http://go.microsoft.com/fwlink/?LinkId=248093)mõista.
- [Azure'i teenus konfiguratsioon skeemi](https://msdn.microsoft.com/library/windowsazure/ee758710)mõista.
- [Konfigureerimine virtuaalse võrgu Network konfigureerimine failide kasutamine](virtual-networks-using-network-configuration-file.md).
