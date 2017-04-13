<properties
   pageTitle="Reserveeritud IP | Microsoft Azure'i"
   description="Reserveeritud IP-d ja kuidas neid hallata mõistmine"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="reserved-ip-overview"></a>Reserveeritud IP ülevaade
IP-aadresside Azure kuuluvad ühte kahest järgmisest kategooriast: dünaamiline ja reserveeritud. Azure'i haldab avaliku IP-aadressid pole vaikimisi dünaamiline. Mida tähendab, et IP-aadressi kasutatakse antud pilveteenuses (VIP) või juurdepääsuks on VM või rolli eksemplari otse (ILPIP) saate muuta aeg-ajalt, kui ressursid on sulgumist või deallocated.

IP-aadresside muutmise vältimiseks saate reserveerida IP-aadress. Reserveeritud IP-d saab kasutada ainult VIP, tagades, et pilveteenusesse IP-aadress on sama, isegi kui ressursid on sulgumist või deallocated. Lisaks saate teisendada olemasoleva dünaamilise IP-d kasutatakse VIP reserveeritud IP-aadress.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saate teada, kuidas reserveerida staatilise avaliku IP-aadressi [ressursihaldur juurutamise mudeli](virtual-network-ip-addresses-overview-arm.md)abil.

Veenduge, et teil mõista, kuidas [IP-aadressid](virtual-network-ip-addresses-overview-classic.md) töötavad Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Kui vaja reserveeritud IP?
- **Soovite tagada, et teie tellimus on reserveeritud IP**. Kui soovite IP-aadress, mis ei tohi kunagi tellimusest vabastada, kasutage reserveeritud avaliku IP.  
- **Soovite oma IP jääda isegi mitmest peatatud või deallocated state (VM) oma pilveteenuses**. Kui soovite oma teenuse IP-aadress, mis ei muutu, kasutades isegi juhul, kui VMs pilveteenusesse Peata või deallocated.
- **Soovite tagada, et Azure'i väljaminev liiklus kasutab prognoositavad IP-aadress**. Teil võib olla teie kohapealne tulemüür on konfigureeritud lubama liiklus ainult teatud IP-aadressid. Broneerimist IP, mida te teate allika IP-aadress ja ei on värskendada oma tulemüüri reeglid IP muutus tõttu.

## <a name="faq"></a>KKK
1. Reserveeritud IP saab kasutada kõiki Azure'i teenuste jaoks?  
  - Reserveeritud IP-d saab kasutada ainult VMs ja pilvepõhise teenuse eksemplari rollide VIP kaudu.
1. Kui palju reserveeritud IP-d saab olla?  
  - Sel ajal, kõik Azure'i tellimused on lubatud kasutada 20 reserveeritud IP-d. Siiski saate taotleda täiendavate reserveeritud IP-d. Vaadake lisateavet lehel [tellimus ja teenuste piirangud](../azure-subscription-service-limits.md) .
1. Kas tasu reserveeritud IP-d?
  - Hinnad teavet teemast [Reserveeritud IP Address hinnad üksikasjad](http://go.microsoft.com/fwlink/?LinkID=398482) .
1. Kuidas saan reserveerida IP-aadress?
  - Saate reserveerida kindla regiooni IP-aadressi PowerShelli või [Azure haldamine REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx) . Reserveeritud IP-aadress on seostatud teie tellimus. IP-aadress reserveerida haldusportaali abil.
1. Kasutada seda osaleja rühmaga vastavalt VNets?
  - Reserveeritud IP-d on toetatud ainult piirkondliku VNets. Ei toetata VNets, mis on seotud osaleja rühmade jaoks. Seostada on VNet piirkonnas või mõne osaleja rühma kohta leiate lisateavet teemast [piirkondliku VNets ja osaleja rühmad](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Kuidas hallata reserveeritud VIP

Enne kui saate reserveeritud IP-d, peate selle tellimuse lisada. Reserveeritud IP avaliku IP-aadresside saadaval kohas *Kesk-USA* kaustast, käivitage PowerShelli järgmine käsk:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Pange tähele, et te ei saa määrata IP on kaitstud. Vaadata, millised IP-aadressid, mis on kaitstud teie tellimus, käivitage järgmine käsk PowerShelli ja pange tähele väärtused *ReservedIPName* ja *aadress*.

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Kui IP on kaitstud, jääb teie tellimusega seostatud, kuni need kustutada. Reserveeritud IP eespool näidatud kustutamiseks käivitage PowerShelli järgmine käsk:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Kuidas endale olemasoleva pilvepõhise teenuse IP-aadress

Saate reserveerida olemasoleva pilvepõhise teenuse IP-aadress, lisades parameetri *- Teenuse nimi* . Reserveerida pilveteenuses *TestService* *USA keskse* asukoha IP-aadress, käivitage PowerShelli järgmine käsk:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Kuidas siduda reserveeritud IP uue pilveteenusesse
Allpool skripti loob uue reserveeritud IP, siis seob selle uue nimega *TestService*pilveteenusesse.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Kui loote reserveeritud IP kasutada mõnda pilveteenusesse, peate viidata VM abil *VIP:&lt;pordi number >* sissetuleva suhtlemiseks. Broneerimist IP ei tähenda, saate luua ühenduse VM otse. Reserveeritud IP on määratud pilveteenusesse, mis VM kasutusele võetud. Kui soovite IP VM otse ühendust luua, tuleb konfigureerida avaliku IP astme. Eksemplari taseme avaliku IP on teatud tüüpi avaliku IP (nimetatakse ka ILPIP), mis on seotud teie VM. See ei saa reserveerida. Lisateabe saamiseks vaadake [Eksemplari taseme avaliku IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Kuidas eemaldada reserveeritud IP töötava juurutamine
Reserveeritud IP, lisatakse uus teenus, mis on loodud ülaltoodud skripti eemaldamiseks käivitage PowerShelli järgmine käsk:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Reserveeritud IP eemaldamine käivitatud juurutamise Eemalda reserveerimine tellimusest. See lihtsalt vabastab IP kasutada mõne muu ressursiga teie tellimus.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Kuidas siduda reserveeritud IP töötava juurutamine
Allpool skripti loob uue nimega *TestService2* koos uue nimega *TestVM2*VM pilveteenus ja seejärel seob olemasolevas reserveeritud IP nimega *MyReservedIP* selle pilveteenusesse.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Kuidas siduda reserveeritud IP pilveteenusesse teenuse konfiguratsiooni faili abil
Teenuse konfigureerimine (CSCFG) faili abil saate seostada ka reserveeritud IP pilveteenusesse. Valimi XML-i allpool näitab, kuidas konfigureerida pilveteenuses kasutada reserveeritud VIP nimega *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Järgmised sammud

- Mõista, kuidas [IP tegelemine](virtual-network-ip-addresses-overview-classic.md) töötab klassikaline juurutamise mudeli.

- Lisateavet [reserveeritud privaatne IP-aadressid](virtual-networks-reserved-private-ip.md).

- Lisateavet [eksemplari tase avaliku IP (ILPIP) aadresside](virtual-networks-instance-level-public-ip.md)kohta.
