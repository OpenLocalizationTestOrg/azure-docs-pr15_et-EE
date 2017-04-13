<properties
   pageTitle="Azure virtuaalse võrgu (VNet) leping ja kujundus juhend | Microsoft Azure'i"
   description="Saate teada, kuidas plaanimine ja kujundamine virtuaalne võrkude Azure oma eraldamise, Ühenduvus ja asukoha vajaduste järgi."
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
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Plaanimine ja kujundamine Azure virtuaalse võrgu

VNet, eksperimenteerida loomine on lihtne, kuid tõenäoliselt juurutamist on mitu VNets aja jooksul teie ettevõtte vajaduste toetamiseks. Mõned kavandamise ja kujundus, siis saab VNets juurutada ja ressursse, peate tõhusamalt ühenduse. Kui olete tuttav VNets, on soovitatav saate [Lisateavet VNets](virtual-networks-overview.md) ja [juurutamise](virtual-networks-create-vnet-arm-pportal.md) ühte enne jätkamist. 

## <a name="plan"></a>Kavandamine

Azure'i tellimused, piirkondade ja võrgu ressursse põhjalik tundmine on oluline edu. Kaalutlused all loendit saate kasutada lähtepunktina. Kui teil mõista nende peaksite arvesse võtma, saate määratleda oma võrgu kavandamise nõuetele.

### <a name="considerations"></a>Kaalutlused

Enne vastamise kavandamise küsimused all, arvestage järgmist.

- Kõik loote Azure koosneb ühe või mitme ressursid. Virtuaalse masina (VM) on ressurss, võrgu adapterit kasutajaliides (NIC) kasutavad VM on ressursi, kasutavad Võrguadapter avaliku IP-aadress on ressurss, VNet, mis on ühendatud NIC on ressurss.
- [Azure'i piirkonna](https://azure.microsoft.com/regions/#services) - ja tellimisprobleemidega ressursside loomine Ja ressursid saate ühendada ainult VNet, mis on olemas sama piirkonna ja need on tellimus. 
- Saate luua ühenduse VNets üksteisest on Azure [VPN-lüüsi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)abil. Samuti saate ühendada VNets piirkondade-ja tellimuste sellisel viisil.
- Saate luua ühenduse VNets kohapealse võrgu, kasutades ühte järgmistest [ühenduvuse suvandid](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) saadaval Azure. 
- Erinevate ressursid saate rühmitada [Ressursi rühmad](../azure-resource-manager/resource-group-overview.md#resource-groups), hõlpsam hallata ressursside üksusena. Ressursirühma võib sisaldada mitut piirkondadest ressursid kui ressursside kuuluvad ühe tellimuse.

### <a name="define-requirements"></a>Nõuded määratlemine

Azure'i võrgu kujunduse jaoks kasutada lähtepunktina järgmistele küsimustele.  

1. Azure'i asukohtade te kasutate Host VNets?
2. Kas vajate anda need Azure asukohad suhtlemine?
3. Kas vajate esitada oma Azure'i VNet(s) ja teie asutusesisese datacenter(s) suhtlemine?
4. Mitu infrastruktuuri teenus (IaaS) VMs cloud services rollid ja web apps kas teil on vaja oma lahenduse?
5. Kas vajate eristamiseks liikluse põhinevad VMs (st front end web serverid ja tagasi end andmebaasi serverid)?
6. Kas vajate liiklust virtuaalse seadmete abil kontrollida?
7. Kasutajad vajavad erinevaid erinevate Azure ressursse õigused?

### <a name="understand-vnet-and-subnet-properties"></a>Mõista VNet ja alamvõrgu atribuudid

VNet ja alamvõrku ressursid määratlemine turvalisus piiri töötab Azure'i töökoormus. Mõne VNet iseloomustab aadress tühikud, määratletud CIDR plokid kogum. 

>[AZURE.NOTE] Võrgu administraatoreid tuttavaid CIDR märke. Kui olete tuttav CIDR, [Lugege lisateavet selle kohta](http://whatismyipaddress.com/cidr).

VNets sisaldavad järgmised atribuudid.

|Atribuut|Kirjeldus|Piiranguid|
|---|---|---|
|**Nimi**|VNet nimi|Kuni 80 märgistringi. Võivad sisaldada tähtede, numbrite, allkriips, perioodid või sidekriipse. Peate käivitama tähe või numbri. Lõpus peab olema kirjas, arvu või allkriips. Saate sisaldab upper ja väiketähti.|  
|**asukoht**|Azure'i asukoht (nimetatakse ka piirkond).|Peab olema lubatud Azure kohast.|
|**addressSpace**|Kogu aadress eesliidete tähised, mis moodustavad VNet CIDR märke.|Peab olema lubatud CIDR aadress blocks, sh avaliku IP-aadresside vahemikud massiivi.|
|**alamvõrku**|Saidikogumi alamvõrku, mis moodustavad selle VNet.|vt alltoodud tabelit alamvõrgu atribuudid.||
|**dhcpOptions**|Objekti, mis sisaldab ühe atribuudi nõutav väärtuseks nimega **dnsServers**.||
|**dnsServers**|DNS-i serverid, mida on VNet massiivi. Kui serverit pole määratud, kasutatakse Azure sisemine nimelahendus.|Peab olema massiivi kuni 10 DNS-serverid IP-aadress.| 

Mõne alamvõrgu on lapse ressurss on VNet ja aitab määratleda segmente aadress ruumide CIDR plokk, kasutades IP address eesliiteid sees. NICs saate lisatud alamvõrku ja ühendatud VMs, pakkudes ühenduvuse eri töökoormus.

Alamvõrku sisaldavad järgmised atribuudid. 

|Atribuut|Kirjeldus|Piiranguid|
|---|---|---|
|**Nimi**|Alamvõrgu nimi|Kuni 80 märgistringi. Tähtede, numbrite, allkriips, perioodid või sidekriipsu võib sisaldada. Peate käivitama tähe või numbri. Lõpus peab olema kirjas, arvu või allkriips. Saate sisaldab upper ja väiketähti.|
|**asukoht**|Azure'i asukoht (nimetatakse ka piirkond).|Peab olema lubatud Azure kohast.|
|**addressPrefix**|Ühe aadress eesliide, mis moodustavad alamvõrgu CIDR märke|Peab olema ühe CIDR plokk, mis on osa üks soovitud VNet aadress tühikuid.|
|**networkSecurityGroup**|Rakendatud alamvõrgu NSG|vt [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Rakendatud alamvõrgu marsruutimiseks tabel|vt [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|Saidikogumi IP alamvõrgu ühendatud NICs konfiguratsiooni objektid|vt [IP konfigureerimine](../resource-groups-networking.md#IP-configurations)|

### <a name="name-resolution"></a>Nimelahendus

Vaikimisi kasutab teie VNet [antud Azure'i nimelahendus.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) lahendamiseks sees on VNet ja avaliku Interneti-ühendusega arvutid nimed. Juhul, kui loote ühenduse oma VNets oma kohapealse andmekeskuste, peate [oma DNS-i server](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) nimede vahel.  

### <a name="limits"></a>Piirangud

Vaadake üle võrgu piirangud tagamaks, et teie kujundus ei vastuolus kõiki [Azure'i piirab](../azure-subscription-service-limits.md#networking-limits) artikli. Mõned piirangud tõsta, avades tugi Piletite.

### <a name="role-based-access-control-rbac"></a>Rollipõhine juurdepääsu reguleerimine (RBAC)

[Azure'i RBAC](../active-directory/role-based-access-built-in-roles.md) abil saate määrata pääsutaseme erinevatele kasutajatele võib-olla erinevad ressursid Azure. Nii võite eraldada tööd, mida teie meeskond vastavalt oma vajadustele. 

Virtuaalne võrkude puhul on kasutajate **Võrku kaasautori** roll Azure'i ressursihaldur virtuaalse võrgu ressursse üle täielik kontroll. Samuti **Klassikaline võrgu kaasautori** roll kasutajatel klassikaline virtuaalse võrgu ressursse üle täielik kontroll.

>[AZURE.NOTE] Samuti saate [luua rolle](../active-directory/role-based-access-control-configure.md) eraldi oma haldus vajadustele.

## <a name="design"></a>Kujundus

Kui olete tuttav [kavandamine](#Plan) jaotise küsimustele vastused, läbi vaadata enne oma VNets määratlemine.

### <a name="number-of-subscriptions-and-vnets"></a>Tellimuste ja VNets arv

Soovitatav on luua mitme VNets järgmistel juhtudel:

- **Mis peavad olema paigutatud eri asukohtades olevate inimestega Azure VMs**. VNets Azure on piirkondlik. Ta ei saa jaotada asukohad. Seega peate olema vähemalt üks VNet iga Azure'i kohta, mida soovite hosti VMs.
- **Mis peavad olema üksteisest täielikult eraldatud töökoormus**. Saate luua eraldi VNets, mida isegi kasutada sama IP address tühikud, eristada üksteisest erinevad töökoormus. 

Pidage meeles, et näete eespool on piirkonna tellimuse kohta. Mida tähendab, et saate mitu tellimust suurendamiseks saate säilitada Azure ressursside limiit. Saate luua ühenduse VNets muu tellimuste VPN saidilt või mõne ExpressRoute ringi.

### <a name="subscription-and-vnet-design-patterns"></a>Tellimuse ja VNet kujundus mustrid

Järgmises tabelis on mõned levinud kujundus mustrid tellimused ja VNets kasutamise.

|Stsenaarium|Skeem|Spetsialistidele|Miinuste|
|---|---|---|---|
|Ühe tellimuse kahe VNets rakenduse kohta|![Ühe tellimuse](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Ainult ühe tellimuse haldamiseks.|Suurim lubatud arv VNets Azure piirkonna kohta. Pärast, et peate mitu tellimust. Vaadake lisateavet [Azure piirab](../azure-subscription-service-limits.md#networking-limits) artiklit.|
|Ühe tellimuse kohta rakenduses kaks VNets rakenduse kohta|![Ühe tellimuse](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Kasutatakse ainult kahe VNets tellimuse kohta.|Raskem, kui seal on liiga palju rakenduste haldamiseks.|
|Ühe tellimuse kahe VNets kohta rakenduse business ühiku.|![Ühe tellimuse](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Saldo tellimuste arv ja VNets vahel.|Suurim lubatud arv VNets business ühiku (tellimuse). Vaadake lisateavet [Azure piirab](../azure-subscription-service-limits.md#networking-limits) artiklit.|
|Ühe tellimuse kahe VNets rühma rakenduste kohta business ühiku.|![Ühe tellimuse](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Saldo tellimuste arv ja VNets vahel.|Rakenduste peate eraldi alamvõrku ja NSGs abil.|


### <a name="number-of-subnets"></a>Arvu alamvõrku.

Tuleks arvesse võtta mitut alamvõrku on VNet järgmistel juhtudel.

- **Pole piisavalt privaatne IP-aadresside jaoks kõik NICs on alamvõrgu sisse**. Kui teie alamvõrku aadressiruumi ei sisalda piisavalt IP-aadresside arvu NICs alamvõrku, peate looma mitu alamvõrku. Pidage meeles, et Azure'i 5 privaatne IP-aadresside kaudu iga alamvõrku, mida ei saa kasutada: esimese ja viimase aadressid aadressiruumi (jaoks alamvõrgu aadress ja multisaate) ja 3-aadressid kasutamiseks ettevõttesiseselt (DHCP ja DNS-i). 
- **Turvalisus**. Saate kasutada alamvõrku eraldi rühmad vms üksteisest töökoormus, mis on mitmekihilise struktuuri ja rakendada eri nende alamvõrku [võrgu turberühmad (NSGs)](virtual-networks-nsg.md#subnets) .
- **Hübriidjuurutuse Ühenduvus**. Saate kasutada VPN lüüside ja ExpressRoute skeeme [ühenduse](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) oma VNets üksteisele ja teie asutusesisese andmete center(s). VPN lüüside ja ExpressRoute topoloogia jaoks on vaja luua oma alamvõrku.
- **Virtuaalne seadmed**. Saate kasutada ka Azure VNet virtuaalse seadme, nt tulemüüri, WAN accelerator või VPN-lüüsi. Kui te seda teete, tuleb [liikluse marsruutimiseks](virtual-networks-udr-overview.md) seadmete ja eristada neid oma alamvõrgu.

### <a name="subnet-and-nsg-design-patterns"></a>Alamvõrgu ja NSG kujundus mustrite

Järgmises tabelis on mõned levinud kujundus mustrid alamvõrku kasutamise kohta.

|Stsenaarium|Skeem|Spetsialistidele|Miinuste|
|---|---|---|---|
|Ühe alamvõrgu NSGs rakenduse kihis rakenduse kohta|![Ühe alamvõrgu](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Ainult ühe alamvõrgu haldamiseks.|Mitme NSGs eristamiseks rakendusele vajalikud.|
|Ühe alamvõrgu rakenduse NSGs kihis rakenduse kohta|![Alamvõrgu rakenduse kohta](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Vähem NSGs haldamiseks.|Mitu alamvõrku haldamiseks.|
|Ühe alamvõrgu NSGs rakenduse kohta rakenduse kihis.|![Alamvõrgu kihis](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Jääk arvu alamvõrku ja NSGs vahel.|NSGs suurim arv tellimuse kohta. Vaadake lisateavet [Azure piirab](../azure-subscription-service-limits.md#networking-limits) artiklit.|
|Ühe alamvõrgu rakenduse kihis kohta rakenduses NSGs alamvõrgu kohta|![Alamvõrgu kihis rakenduse kohta](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Võimalik, et väiksema arvu NSGs.|Mitu alamvõrku haldamiseks.|

## <a name="sample-design"></a>Valimi kujundus

Selle artikli teave rakenduse illustreerida, proovige järgmist varianti.

Mõni ettevõte, mis on 2 andmekeskuste Põhja-Ameerikas ja kaks andmekeskuste Europe töötate. Saate tuvastatud 6 erinevad kliendi suunatud rakenduste haldab 2 eri business üksused, mida soovite migreerida Azure pilootprojekti nimega. Rakenduste lihtsa arhitektuur on järgmised:

- APP1, App2, App3 ja App4 on veebirakenduste töötab Ubuntu Linux serverites majutatud. Iga rakendus loob eraldi rakenduse server majutatakse rahulik teenuste Linux serverites. Rahulik teenuste tagasi end MySQL-andmebaasiga ühenduse loomine.
- App5 ja App6 on veebirakenduste, kus töötab Windows Server 2012 R2 Windowsi serverites majutatud. Iga rakendus ühendab tagasi end SQL serveri andmebaasi.
- Kõik rakendused praegu majutatakse ühes ettevõtte andmekeskuste Põhja-Ameerikas.
- Kohapealse andmekeskuste kasutada 10.0.0.0/8 aadressiruumi.

Peate koostama virtuaalse võrgu lahenduse, mis vastab järgmistele tingimustele:

- Iga üksuse business ei tohiks mõjutada ressursi kasutamine muudes business üksustes.
- Mida peaks minimaalseks VNets ja alamvõrku haldamise hõlbustamiseks.
- Iga business üksus peaks olema ühe testi/arengu VNet kasutatakse kõik rakendused.
- Konkreetse rakenduse majutatakse 2 eri Azure andmekeskuste continent (Põhja-Ameerika ja Euroopa) kohta.
- Iga rakendus on täielikult eraldatud.
- Konkreetse rakenduse pääseb kliendid HTTP kaudu Interneti kaudu.
- Konkreetse rakenduse pääseb kasutajad ühendatud kohapealse andmekeskuste mõne krüptitud tunneliga abil.
- Kohapealse andmekeskuste ühenduse tuleks kasutada olemasoleva VPN seadmed.
- Ettevõtte võrkude rühm peaks olema VNet konfiguratsiooni üle täielik kontroll.
- Igas üksuses business arendajate peaks oskama ainult juurutada VMs olemasoleva alamvõrku.
- Kõik rakendused migreeritakse, kui need on Azure (lifti-ja-shift).
- Iga asukoha andmebaasid tuleks korrata Azure mujal kord päevas.
- Konkreetse rakenduse tuleks kasutada 5 front end web serverid, 2 rakenduste serverid (vajadusel) ja 2 andmebaasi serverid.

### <a name="plan"></a>Kavandamine

Te peaksite oma kujundus kavandamise vastates [Määratle nõuded](#Define-requirements) jaotises küsimus, nagu allpool näidatud.

1. Azure'i asukohtade te kasutate Host VNets?

    Põhja-Ameerikas 2 asukohad ja Euroopa 2 asukohad. Valige need, võttes aluseks olemasolevad kohapealse andmekeskuste füüsilise asukoha. Nii teie ühendus oma füüsilise asukohtadest Azure'i oleks parem latentsus.

2. Kas vajate anda need Azure asukohad suhtlemine?

    Jah. Kuna andmebaasid peab olema kopeeritud kõik asukohad.

3. Kas vajate esitada oma Azure'i VNet(s) ja teie asutusesisese andmete center(s)?

    Jah. Kuna kasutajad ühendatud peab olema asutusesisese andmekeskuste rakenduse avamiseks on krüptitud tunneliga kaudu.
 
4. Kui palju IaaS VMs peate oma lahenduse?

    200 IaaS VMs. APP1, App2 ja App3 jaoks on vaja serverid 5 web, serverid 2 rakenduste ja 2 andmebaasi serverid iga. Mis on kokku 9 IaaS VMs kohta ja 36 IaaS VMs. App5 ja App6 jaoks on vaja 5 web serverid ja 2 andmebaasi serverid. Mis on kokku 7 IaaS VMs taotluse kohta või 14 IaaS VMs. Seega peate 50 IaaS VMs Azure'i piirkonna kõik rakendused. Kuna tuleb kasutada 4 regioonid, saab 200 IaaS VMs.

    Samuti peate DNS-serverid iga VNet või oma kohapealse andmekeskuste lahendamiseks oma Azure IaaS VMs ja kohapealse võrgu nime anda. 

5. Kas vajate eristamiseks liikluse põhinevad VMs (st front end web serverid ja tagasi end andmebaasi serverid)?

    Jah. Iga rakendus peab olema täielikult eraldatud ja iga kihid peaks olema eraldatud. 

6. Kas vajate liiklust virtuaalse seadmete abil kontrollida?

    Ei. Virtuaalne seadmed saab esitada liiklust, sh üksikasjalikud andmed lennuk logimine üle rohkem kontrolli. 

7. Kasutajad vajavad erinevaid erinevate Azure ressursse õigused?

    Jah. Võrgu meeskond peab Täielik kasutusõigus virtuaalse võrgu sätted, ajal arendajate peaks oskama ainult juurutada järel olemasoleva alamvõrku. 

### <a name="design"></a>Kujundus

Peate järgima tellimused, VNets, alamvõrku ja NSGs kujundus. Me NSGs siin arutada, kuid te peaksite Lisateavet [NSGs](virtual-networks-nsg.md) enne lõpetamist oma kujundus.

**Tellimuste ja VNets arv**

Järgmised nõuded on seotud tellimused ja VNets:

- Iga üksuse business ei tohiks mõjutada ressursi kasutamine muudes business üksustes.
- Mida peaks minimaalseks VNets ja alamvõrku.
- Iga business üksus peaks olema ühe testi/arengu VNet kasutatakse kõik rakendused.
- Konkreetse rakenduse majutatakse 2 eri Azure andmekeskuste continent (Põhja-Ameerika ja Euroopa) kohta.

Nende vajaduste järgi, peate tellimuse business iga üksuse jaoks. Nii tarbimine ressursse üksus business ei arvestata piirangud muude üksuste business. Ja kuna soovite vähendada VNets arvu, võiksite kasutada **ühe tellimuse kahe VNets rühma rakenduste kohta business ühiku** mustri, nagu allpool näha.

![Ühe tellimuse](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Samuti peate määrama iga VNet aadress ruumi. Kuna peate – kohapealsete andmete Ühenduvus andmekeskuste ja Azure regioonid, ei saa kasutada Azure VNets aadress ruumi vastuolus kohapealse võrgu ja aadressiruumi, mida iga VNet ei peaks vastuolus muude olemasoleva VNets. Saate kasutada aadressi tühikuid Allolev tabel nendele nõuetele.  

|**Tellimuse**|**VNet**|**Azure'i piirkond**|**Aadressiruumi jaoks**|
|---|---|---|---|
|BU1|ProdBU1US1|Lääne USA.|172.16.0.0/16|
|BU1|ProdBU1US2|Ida-USA|172.17.0.0/16|
|BU1|ProdBU1EU1|Põhja-Euroopa|172.18.0.0/16|
|BU1|ProdBU1EU2|Lääne Euroopa|172.19.0.0/16|
|BU1|TestDevBU1|Lääne USA.|172.20.0.0/16|
|BU2|TestDevBU2|Lääne USA.|172.21.0.0/16|
|BU2|ProdBU2US1|Lääne USA.|172.22.0.0/16|
|BU2|ProdBU2US2|Ida-USA|172.23.0.0/16|
|BU2|ProdBU2EU1|Põhja-Euroopa|172.24.0.0/16|
|BU2|ProdBU2EU2|Lääne Euroopa|172.25.0.0/16|

**Alamvõrku ja NSGs arv**

Järgmised nõuded on seotud alamvõrku ja NSGs:

- Mida peaks minimaalseks VNets ja alamvõrku.
- Iga rakendus on täielikult eraldatud.
- Konkreetse rakenduse pääseb kliendid HTTP kaudu Interneti kaudu.
- Konkreetse rakenduse pääseb kasutajad ühendatud kohapealse andmekeskuste mõne krüptitud tunneliga abil.
- Kohapealse andmekeskuste ühenduse tuleks kasutada olemasoleva VPN seadmed.
- Iga asukoha andmebaasid tuleks korrata Azure mujal kord päevas.

Nende vajaduste järgi, võib kasutada ühe alamvõrgu rakenduse kihis ja NSGs abil filter liikluse taotluse kohta. Nii teil on ainult 3 alamvõrku iga VNet (ees, kihid ja andmete layer) ja ühe NSG iga alamvõrgu kohta. Sel juhul võiksite kasutada **ühe alamvõrgu NSGs rakenduse kohta rakenduse kihis** kujundus muster. Alloleval joonisel kujundus mustri, mis tähistab **ProdBU1US1** VNet kasutamine.

![Ühe alamvõrgu kihis ühe NSG taotluse kihis kohta](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Siiski peate ka luua mõne eest alamvõrgu VPN Ühenduvus on VNets ja teie asutusesisese andmekeskuste vahel. Ja määrake aadressiruumi jaoks iga alamvõrgu. Joonis näitab **ProdBU1US1** VNet valimi lahendus. Teil oleks selle stsenaariumi puhul iga VNet korrata. Iga värv tähistab mõne muu rakenduse.

![Valimi VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Juurdepääsu reguleerimine**

Järgmised nõuded on seotud juurdepääsu juhtimine:

- Ettevõtte võrkude rühm peaks olema VNet konfiguratsiooni üle täielik kontroll.
- Igas üksuses business arendajate peaks oskama ainult juurutada VMs olemasoleva alamvõrku.

Nende vajaduste järgi, saate lisada kasutajaid võrgu meeskond sisseehitatud **Võrgu kaasautori** roll iga tellimuse; ja looge kohandatud rolli rakendus arendajad iga tellimus, andes neile õigusi lisada VMs olemasoleva alamvõrku.

## <a name="next-steps"></a>Järgmised sammud

- [Deploy virtuaalse võrgu](virtual-networks-create-vnet-arm-template-click.md) stsenaariumi.
- Mõista [laadimine saldo](../load-balancer/load-balancer-overview.md) IaaS VMs ja [haldamine, üle mitme Azure'i piirkonnad marsruutimist](../traffic-manager/traffic-manager-overview.md).
- Lugege lisateavet [NSGs ja kuidas plaanimine ja kujundamine](virtual-networks-nsg.md) NSG lahenduse.
- Lugege lisateavet oma [asutusesiseses ja VNet ühenduvuse suvandid](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
