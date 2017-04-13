<properties
   pageTitle="Turvaline hübriid võrgu arhitektuur rakendamisel Azure'i | Microsoft Azure'i"
   description="Kuidas rakendada turvaline hübriid võrgu ülesehituse Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Azure'i ja teie asutusesisese andmekeskuse vahelise on DMZ rakendamine

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse head tavad turvaline hübriid võrku, mis on kohapealse võrgu laieneb Azure'i rakendamiseks. See viide arhitektuur rakendab on DMZ vahel on kohapealse võrgu ja Azure virtuaalse võrgu, mis marsruudib kasutaja määratletud (UDRs) abil. DMZ sisaldab tugevalt võrgu virtuaalse seadmed (NVAs), et rakendada turvalisuse funktsioone nagu tulemüürid ja paketi kontrollimise. Kõik väljamineva liikluse aadressilt on VNet on Jõusta tunneled Interneti kaudu kohapealse võrgu nii, et seda saab auditeerida. 

See arhitektuur nõuab teie asutusesisese andmekeskuse rakendada, kasutades kas [VPN-lüüsi]ühenduse[ra-vpn], või mõne [ExpressRoute] [ ra-expressroute] ühendus.

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. See viide arhitektuur kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

Tüüpilised kasutamine karpide see arhitektuur on järgmised.

- Hübriidjuurutuse rakenduste kust töökoormused Käivita osaliselt kohapealse ja osaliselt teemast Azure.

- Azure'i infrastruktuuri, mis marsruudib sissetulev liiklus kohapealse ja Interneti-ühendus.

- Rakenduste auditeeritavad väljamineva liikluse nõutav. See on sageli palju trükikojas süsteemide nõuetele nõue ja aitab vältida privaatse teabe avalikustamist.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm olulisemad toimingud see arhitektuur olulised komponendid.

> Visio dokumendi, mis sisaldab arhitektuur diagramm on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on "DMZ - Privaatne" lehel.

[! [0]][0]

- **Kohapealse võrgu.** See on võrgu arvutite ja ühendatud seadmete kaudu privaatse kohaliku piirkonna võrgu ettevõtte rakendada.

- **Azure virtuaalse võrgu (VNet).** Funktsiooni VNet majutab rakendus ja muud ressursid, mis töötab pilveteenuses.

- **Lüüsi.** Lüüsi pakub Ühenduvus ruuterid kohapealse võrgu ja selle VNet.

- **Võrgu virtuaalse seadme (Dessandi).** Dessandi on üldine termin, mis kirjeldab nagu ligipääs tulemüüri, optimeerimise WAN toimingute (sh võrgu tihendamise), kohandatud marsruutimist või muud võrgu funktsioonid ja ülesandeid täitvate VM. 

- **Esimese taseme, business taseme ja andmete taseme alamvõrku veebis.** Need on alamvõrku majutusteenuse VMs ja teenuseid, mis on näide 3 rakendus töötab pilveteenuses rakendada. [Opsüsteemi Windows VMs N-astme arhitektuur Azure] näha[ ra-n-tier] lisateabe saamiseks.

- **Kasutaja määratletud marsruudib (UDR).** [Kasutaja määratletud marsruudib] [ udr-overview] määratlevad IP liikluse Azure'i VNets. 

> [AZURE.NOTE] Sõltuvalt nõuetele VPN-ühendus, saate konfigureerida äärise lüüsi protokolli (BGP) marsruudib alternatiivina UDRs abil saate rakendada edasisaatmise reegleid, mis suunaks liikluse tagasi kohapealse võrgu kaudu.

- **Juhtimise alamvõrgu.** Selle alamvõrgu sisaldab VMs eeldab, et haldus ja komponentide käitamine on VNet jälgimise võimalusi. 

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme pakkunud viide arhitektuur soovituste järgneva installimiseks on Azure ressursihaldur malli. Kui soovite luua oma viite arhitektuur peaksite nende soovituste täitmisel juhul, kui teil on konkreetse nõude, mis ei sobi soovitus.

### <a name="rbac-recommendations"></a>RBAC soovitused

Mitme RBAC rollide haldamine oma rakenduse ressursside loomine. Kaaluge DevOps [kohandatud] [ rbac-custom-roles] õigustega rakenduse infrastruktuuri haldamine. Kaaluge tsentraliseeritud IT administraatori [rolli kohandatud] [ rbac-custom-roles] võrgu ressursse ja eraldi turvalisus IT-administraator [kohandatud] haldamise[ rbac-custom-roles] hallata secure võrgu ressursse, nagu on NVAs. 

DevOps roll peaks sisaldama rakenduse osad, samuti kuvari juurutada ja taaskäivitage VMs õigused. Tsentraliseeritud IT-administraatori rolli peaks sisaldama õiguste jälgida võrgu ressursse. Järgmistes valimata rollid peaks olema juurdepääs Dessandi ressursid, nagu see tuleks piirata turvalisus IT-administraatori roll.

### <a name="resource-group-recommendations"></a>Allika rühma soovitused

Azure'i ressursid, nt VMs, VNets ja koormus soolise saab hõlpsasti hallata rühmitamine nende ressursside rühmadesse. Seejärel saate määrata iga ressursirühma juurdepääsu piiramise kohal rollid.

Soovitame loomine järgmist:

- Ressursirühm, mis sisaldab alamvõrku, (v.a VMs), NSGs ja kohapealse võrgu ühenduse lüüs ressursside. Saate määrata selle ressursirühma tsentraliseeritud IT-administraatori roll.

- Sisaldavad VMs NVAs, (sh laadi koormusetasakaalustusteenuse), välja hüpata ja muid halduse VMs ja UDR jaoks lüüsi alamvõrku, mis liiklust kaudu soovitud NVAs ressursirühma. Saate määrata selle ressursirühma turvalisus IT-administraatori roll.

- Ressursi rühmade rakenduse iga taseme laadi koormusetasakaalustusteenuse ja VMs sisaldavate eraldi. Pidage meeles, et selle ressursirühma ei peaks iga taseme alamvõrku. Saate määrata selle ressursirühma DevOps roll.

### <a name="virtual-network-gateway-recommendations"></a>Virtuaalse võrgu lüüsi soovitused

Kohapealse liikluse läbib on VNet virtuaalse võrgu lüüsi. Soovitatav on [Azure VPN-lüüsi] [ guidance-vpn-gateway] või mõne [Azure'i ExpressRoute lüüsi][guidance-expressroute]. 

### <a name="nva-recommendations"></a>Dessandi soovitused

NVAs osutavad erinevaid juhtimiseks ja võrguliiklust. Azure'i turuplatsi pakub mitmeid kolmandate ettevõtete loodud NVAs, sh:

- [Barracuda Web rakenduse tulemüüri] [ barracuda-waf] ja [Barracuda NextGen tulemüüri][barracuda-nf]

- [Ühtne võrkude VNS3 tulemüüri/ruuteri/VPN][vns3]

- [Fortinet FortiGate-VM][fortinet]

- [SecureSphere Web rakenduse tulemüüri][securesphere]

- [DenyAll Web rakenduse tulemüüri][denyall]

- [Punkti vSEC kontrollimine][checkpoint]

Kui ükski nende kolmanda osapoole NVAs ei vasta teie vajadustele, saate luua kohandatud Dessandi, kasutades VMs. Näiteks luua kohandatud NVAs, leiate selle viide arhitektuuri, mis rakendab funktsioonid DMZ.

- Liiklus marsruuditakse abil [IP edastamine] [ ip-forwarding] Dessandi NICs kohta.

- Liikluse on lubatud ainult juhul, kui on vaja teha selle Dessandi läbida. Iga Dessandi VM viide arhitektuur on lihtne Linux ruuteri sissetulev liiklus saabuvate võrgu kasutajaliidese *eth0*ja väljaminev liiklus kattuvad reeglid määratletud kohandatud skriptide võrgu kasutajaliidese *eth1*kaudu saadetud. 

- Halduse alamvõrgu marsruutida liikluse ei läbi soovitud NVAs ja selle NVAs saab konfigureerida ainult alamvõrgu haldus. Kuni soovitud NVAs marsruutida liikluse halduse alamvõrgu vajadusel on pole protsessi halduse alamvõrgu lahendamiseks soovitud NVAs juhul, kui nad peaksid.  

- Laadi koormusetasakaalustusteenuse taha seadmine saadavus kaasatakse jaoks soovitud Dessandi VMs. UDR lüüsi alamvõrgu suunab Dessandi taotluste laadi koormusetasakaalustusteenuse.

Teine soovitus kaaluda on mitu NVAs sarja ühendavad iga Dessandi ülesannet, eriotstarbelisi turvalisus. See võimaldab iga turvalisuse funktsioon hallatakse Dessandi kohta alusel. Näiteks võib mõni Dessandi rakendamise tulemüüri paigutada mõne Dessandi identiteedi teenused töötavad koos sarja. Haldamise hõlbustamiseks Miinuseks on lisaks eest võrgu humala, mis võivad suurendada latentsus, seega veenduge, et see ei mõjuta teie taotlus jõudluse.

### <a name="nsg-recommendations"></a>NSG soovitused

VPN-lüüsi seab kohapealse võrgu ühenduse avaliku IP-aadress. Soovitame sissetuleva Dessandi alamvõrgu eeskirjadele blokeerida kogu liikluse pärit kohapealse võrgu network turberühma (NSG) loomine.

Soovitame juurutada NSGs iga alamvõrgu jaoks teise taseme kaitse sissetulev liiklus mööda on valesti konfigureeritud või keelatud Dessandi. Näiteks rakendab web taseme alamvõrgu viide arhitektuur on NSG reegli abil ignoreerida kõik kutsed muudel saadud kohapealse võrgu (192.168.0.0/16) või selle VNet ja teise reegli, mis ignoreerib kõiki taotlusi ei port 80. 

### <a name="internet-access-recommendations"></a>Internet access soovitused

[Jõusta-tunneliga] [ azure-forced-tunneling] kõigi väljaminevate Interneti-liikluse-saidilt VPN tunneliga ja marsruutimiseks Interneti kasutamise abil kohapealse võrgu kaudu võrgu aadress tranlation (NAT). See nii vältida juhusliku lekke talletatud teie andmete taseme konfidentsiaalset teavet ja ka võimaldab kontroll ja auditeerimine kõik väljamineva liikluse.

> [AZURE.NOTE] Pole täielikult blokeerida Interneti-liikluse web, äri ja rakenduse astme. Kui need astme Azure'i PaaS teenuste need toetuvad avaliku IP-aadresside VM diagnostika logimine kasutamiseks alla laadida VM laiendid ja muud funktsioonid. Azure'i diagnostika eeldab, et komponendid saate lugeda ja kirjutada Interneti-sõltuv Azure storage konto.

Me soovitame edasi, et veenduda väljaminev Interneti-liikluse on Jõusta tunneled õigesti. Kui kasutate VPN-ühendus [marsruutimist ja remote juurdepääsu teenuse] [ routing-and-remote-access-service] kohapealse serveris kasutada tööriist, näiteks [Wiresharkis] [ wireshark] või [Microsoft sõnumi Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Alamvõrgu soovituste

Juhtimise alamvõrgu sisaldab hüpata boks, mis käivitab haldus ja jälgimisega seotud funktsioonid. Rakendada välja hüpata järgmisi soovitusi:
- Looge hüpata ruut avaliku IP-aadress. 
- Saate luua ühe marsruutimiseks sissetulevate lüüsi kaudu juurdepääsu hüpata ja rakendada mõne NSG halduse alamvõrgu vastata vaid lubatud liini.
- Piirata kõigi turvaline halduse ülesannete täitmise välja hüpata.

### <a name="nva-recommendations"></a>Dessandi soovitused

Kaasa kiht 7 Dessandi rakenduse ühendused tasemel Dessandi lõpetada ja hallata taustväärtus astme koos osaleja. See tagab sümmeetriline ühenduvuse vastuse liikluse aadressilt taustväärtus astme tagastab selle Dessandi kaudu.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

Viide arhitektuur rakendab laadi koormusetasakaalustusteenuse, mis suunavad kohapealse võrguliikluse kogumi Dessandi seadmed. Nagu varem, Dessandi seadmeid VMs võrgu liikluse marsruutimise reeglite täitmise ja üheks [kättesaadavus]on juurutatud[availability-set]. See kujundus võimaldab teil jälgida funktsiooni NVAs läbilaskevõime aja jooksul ja Dessandi seadmete vastuseks tõusu laadi lisamine.

Standardse SKU-ga VPN-lüüsi toetab kuni 100 Mbps püsiv läbilaskevõime. Suure jõudlusega SKU pakub kuni 200 Mbps. Jaoks suurema andmemahud, kaaluge mõne ExpressRoute lüüsi. ExpressRoute pakub kuni 10 Gbit läbilaskevõime alumise latentsus kui VPN-ühendus.

> [AZURE.NOTE] [Rakendamise hübriid võrgu ülesehituse Azure ja kohapealse VPN] artikleid[ guidance-vpn-gateway] ja [rakendada hübriidjuurutuse võrgu ülesehituse koos Azure'i ExpressRoute] [ guidance-expressroute] kirjeldavad küsimusi Azure lüüside skaleeritavus.

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Viide arhitektuur rakendab laadi koormusetasakaalustusteenuse, levitamise taotluste kohapealsete Dessandi seadmete Azure kogumi. Dessandi seadmeid VMs võrgu liikluse marsruutimise reeglite täitmise ja üheks [kättesaadavus]on juurutatud[availability-set]. Laadi koormusetasakaalustusteenuse regulaarselt päringut seisundi juures, rakendada iga Dessandi ja mis tahes ei reageeri NVAs eemaldamine pool. 

Kui kasutate pakkuda ühenduvus vahel VNet ja kohapealse võrgu, [esitada Tõrkesiirde VPN-lüüsi konfigureerida] Azure ExpressRoute[ guidance-vpn-failover] kui ExpressRoute ühendus pole saadaval.

Säilitada kättesaadavus VPN-i ja ExpressRoute ühenduste kohta täpsema teabe saamiseks lugege artikleid [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN] [ guidance-vpn-gateway] ja [rakendada hübriidjuurutuse võrgu arhitektuur koos Azure'i ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Kaalutlused hallatavus

Kõigi rakenduste ja ressursside jälgimine peaks teostama halduse alamvõrgu kasti hüpata. Sõltuvalt teie taotlus nõuetele, võib-olla peate lisama lisaressursid jälgimisega seotud halduse alamvõrku, kuid uuesti mõni järgmised täiendavad ressursid tuleks tutvuda välja hüpata.

Kui lüüsi ühenduvust kohapealse võrgu Azure ei tööta, ikka jõuate välja hüpata kasutades PIP, lisades hüpata väljale ning klõpsake remoting Interneti kaudu.

Iga taseme alamvõrgu viide arhitektuur on kaitstud NSG reeglid ja tuleb avada porti 3389 RDP juurdepääsu Windowsi VMs või port 22 SSH juurdepääsu Linux VMs reegli loomiseks. Muude haldus ja vahendid nõuda reeglid täiendavad portide avamiseks.

Kui kasutate ExpressRoute esitada Ühenduvus teie asutusesisese andmekeskuse ja Azure, kasutada [Azure ühenduvuse tööriistakomplekti (AzureCT)] [ azurect] jälgida ja ühenduse probleemide tõrkeotsing.

> [AZURE.NOTE] Täiendavat teavet suunatud jälgimine ja haldamine VPN-i ja ExpressRoute ühendused [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN-i] artiklitest leiate[ guidance-vpn-gateway] ja [rakendada hübriidjuurutuse võrgu arhitektuur koos Azure'i ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Turvalisuse alused

See viide arhitektuur rakendab turvalisuse mitu taset: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Kõik asutusesisese kasutaja taotlused kaudu soovitud Dessandi marsruutimine

UDR lüüsi alamvõrgu blokeerib kõik kasutaja taotlused muudel saadud kohapealse. UDR läheb lubatud taotluste soovitud NVAs privaatne DMZ alamvõrgu ja taotlused on edasi rakendusse kui need on lubatud Dessandi reegleid. Muud marsruudib saab lisada soovitud UDR, kuid veenduge, et need ei tahtmatult mööduda NVAs või Blokeeri haldus liikluse mõeldud alamvõrgu haldus.

Laadi koormusetasakaalustusteenuse ees olevat NVAs toimib ka turvalisus seade, ignoreerides liiklus pordid, mis pole avatud, klõpsake Laadi tasakaalustamiseks reeglid. Ainult kuulata koormus soolise viide arhitektuur HTTP päringuid pordi 80 ja pordi 443 HTTPS päringuid. Mis tahes täiendavaid reeglid lisatakse koormus soolise dokumenteerida ja jälgida liiklus pole probleeme turvalisuse tagamiseks.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Blokeeri/pass liikluse vahel rakenduse astme NSGs abil

Iga astme veebi-, äri- ja piirata nende vahel NSGs abil. Business taseme kasutab mõnda NSG blokeeri kogu liiklus, et ei pärinevad web taseme ja andmete taseme kasutab mõnda NSG blokeeri kogu liiklus, mis ei pärinevad business astme. Kui teil on nõue NSG reegleid laiema juurdepääsu nende astme laiendamiseks, kaaluda nende nõuete suhtes sellega. Iga uue sissetuleva rada kujutab juhusliku või otstarbeka andmete lekke või rakenduse kahju võimalus.

### <a name="devops-access"></a>DevOps juurdepääs

Piira toimingud, mida iga taseme [RBAC] abil saab teha DevOps[ rbac] õiguste haldamiseks. Õiguste andmisel kasutada [vähimate õiguste põhimõte][security-principle-of-least-privilege]. Logige kõik haldustoimingud ja tavalise auditite tagamiseks on kavandatud konfiguratsiooni muudatusi teha.

> [AZURE.NOTE] Rohkem teavet, näited ja stsenaariumid võrgu turvalisuse Azure, haldamise kohta leiate teemast [Microsofti pilveteenustega ja võrgu turvalisuse][cloud-services-network-security]. Üksikasjalikku teavet ressursid pilveteenuses kaitsmise kohta, vaadake teemat [Alustamine Microsoft Azure'i turvalisusega][getting-started-with-azure-security]. Lisateavet tegelemine turvakaalutlused üle Azure lüüsi-ühendus, leiate [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN] [ guidance-vpn-gateway] ja [rakendada hübriidjuurutuse võrgu arhitektuur koos Azure'i ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Lahenduse juurutamine

Viide arhitektuuri, mis rakendab soovituste juurutamine on saadaval github. See viide arhitektuur sisaldab virtuaalse võrgu (VNet), võrgu turberühma (NSG), laadi koormusetasakaalustusteenuse ja kahe virtuaalmasinates (VMs).

Viide arhitektuur saab kasutada Windowsi või Linuxi VMs alltoodud juhiseid järgides: 

1. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Kui link on avatud Azure'i portaalis, peate sisestama väärtused mõned sätted: 
    - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Loo uus** ja sisestage `ra-private-dmz-rg` tekstiväljale.
    - Valige piirkonnale **asukoht** rippmenüüst.
    - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
    - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
    - Klõpsake nuppu **osta** .

3. Oodake, kuni juurutuse lõpuleviimiseks.

4. Parameetri faile lisada raske koodiga administraatori kasutajanime ja parooli kõik vms ja soovitame teil kohe muuta nii. Jaoks iga VM juurutuse, valige see Azure portaali ja seejärel klõpsake **parooli lähtestamine** tera **tugi + tõrkeotsing** . Valige rippmenüüst **režiimi** **parooli lähtestamine** ja seejärel valige uus **kasutajanimi** ja **parool**. Klõpsake nuppu **Uuenda** , et püsida.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas rakendada mõne [DMZ Azure ja Interneti vahel](./guidance-iaas-ra-secure-vnet-dmz.md).
- Siit saate teada, kuidas rakendada mõne [väga kättesaadav hübriid võrgu ülesehituse](./guidance-hybrid-network-expressroute-vpn-failover.md).

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Turvaline hübriid võrgu arhitektuur"
