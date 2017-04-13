<properties 
   pageTitle="Kuidas migreerida osaleja rühmade piirkondliku virtuaalse võrku (VNet)"
   description="Saate teada, kuidas migreerida osaleja rühmade piirkondliku vnets"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Kuidas migreerida osaleja rühmade piirkondliku virtuaalse võrku (VNet)

Saate ka osaleja rühma ressursse, mis on loodud osaleja rühma füüsilise majutatakse serverid, mis on koos, lubada järgmiste ressursside kaudu suhtlemiseks kiiremini tagamiseks. Varem olid osaleja rühmade loomise virtuaalne võrkude (VNets) nõue. Sel ajal, võib võrguteenuse halduri hallatava VNets ainult töötama füüsilise serveri või skaala ühik kogum. Võrgu juhtimise alale ulatus kasvanud arhitektuuri täiustused.

Selle tulemusena arhitektuuri parandusi, enam on osaleja rühmade Soovitatavad või virtuaalse võrgu jaoks nõutavad. Osaleja rühmade kasutamine VNets asendatakse regioonid. Mis on seotud piirkondade VNets nimetatakse piirkondliku VNets.

Lisaks soovitame, et te ei kasuta üldiselt osaleja rühmad. Peale VNet nõue, osaleja rühmad olid ka olulised abil tagada ressursid Arvuta ja salvestusruumi, nt suunati üksteise lähedal. Siiski praeguse Azure võrgu arhitektuur, nende paigutuse nõuetele on enam ei vaja. Vaadake teemat [osaleja rühmad ja VMs](#Affinity-groups-and-VMs) mõned ülejäänud teatud juhul, kui võiksite kasutada osaleja rühma.

## <a name="creating-and-migrating-to-regional-vnets"></a>Loomine ja piirkondliku VNets migreerimine

Edasi, kui loote uue VNets, kasutage *piirkond*. Näete seda valikut haldusportaal. Pange tähele, et konfiguratsioonifailis võrgu see *asukoht*.

>[AZURE.IMPORTANT] Kuigi see on endiselt tehniliselt võimalik virtuaalse võrgu, mis on seostatud mõne osaleja rühma loomiseks, on selleks mõjuv põhjus. Palju uusi funktsioone, näiteks võrgu turberühmad, on saadaval ainult piirkondliku VNet kasutamisel ja teiega ei saa virtuaalse võrke, mis on seotud osaleja rühmad.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>VNets praegu seostatud osaleja rühmade kohta

VNets, mis on praegu seotud osaleja rühmad on lubatud migration, et piirkondliku VNets. Migreerimine piirkondliku VNet, toimige järgmiselt.

1. Võrgu konfigureerimise faili eksportida. Saate kasutada PowerShelli või portaalis haldus. Haldusportaali abil juhised leiate teemast [konfigureerimine oma VNet võrgu konfigureerimise faili abil](virtual-networks-using-network-configuration-file.md).

1. Redigeerige oma võrgu konfiguratsioonifail vana väärtuste asendamine uued väärtused. 

    > [AZURE.NOTE] **Asukoht** on määratud osaleja rühma, mis on seotud teie VNet piirkond. Näiteks kui teie VNet on seostatud osaleja rühma, mis asub Lääne USA, kui migreerite, asukoha peavad osutama Lääne US. 
    
    Järgmised read võrgu konfigureerimise faili, asendades väärtused ise redigeerimiseks tehke järgmist. 

    **Vana väärtus:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 

    **Uus väärtus:** \<VirtualNetworkSitename = "VNetUSWest" asukoht = "Lääne USA"\>

1. Salvestage muudatused ja [importida](virtual-networks-using-network-configuration-file.md) võrgukonfiguratsioon Azure.

>[AZURE.NOTE] See migreerimise ei põhjusta mis tahes tööseisakute juurde oma teenustele.

## <a name="affinity-groups-and-vms"></a>Osaleja rühmad ja VMs

Nagu eelnevalt mainitud, on soovitatav VMs enam üldiselt osaleja rühmad. Kasutage ka osaleja rühma ainult siis, kui kogumi VMs peab olema absoluutne alumise Võrgu latentsuse VMs vahel. Osaleja rühma VMs pannes VMs kõik paigutatakse sama Arvuta kobar või mastaabi üksuse.

See on oluline tähele, et ka osaleja rühma abil saate on kaks, võib-olla negatiivsed tagajärjed:

- VM suuruses määramine piirdub VM suuruses pakutud Arvuta skaala ühik määramine.

- On suurem tõenäosus, ei saa määrata uue VM. See juhtub siis, kui osaleja rühma teatud skaala üksus on välja võimsus.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Mida teha, kui teil on VM osaleja nimel

VMs, mis on praegu osaleja nimel pole vaja osaleja rühmast eemaldada.

Kui VM on juurutatud, juurutamist ühe skaala ühik. Osaleja rühmad saavad piirata saadaval VM suuruses uue VM juurutamiseks määramine, kuid olemasolevate VM, mis on juurutatud on juba piiratud kogumisse VM läiked skaala ühik, milles on juurutatud VM. Seetõttu osaleja rühma VM eemaldamine ei mõjuta.
 
