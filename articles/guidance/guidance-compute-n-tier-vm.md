<properties
   pageTitle="Opsüsteemi Windows VMs N-astme arhitektuur | Viide arhitektuur | Microsoft Azure'i"
   description="Kuidas rakendada mitmekihilise arhitektuuri Azure, pöörates erilist tähelepanu kättesaadavus, turvalisus, skaleeritavus ja hallatavus turvalisus."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>Opsüsteemi Windows VMs N-astme arhitektuur Azure

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Azure'i N-astme arhitektuur Linux VMs opsüsteemi](guidance-compute-n-tier-vm-linux.md)
- [Opsüsteemi Windows VMs N-astme arhitektuur Azure](guidance-compute-n-tier-vm.md)

See artikkel kirjeldab rakenduse, kus N-astme arhitektuuri tõestatud tavade jaoks, kus töötab Windows virtuaalmasinates (VMs). Selles artiklist [töötab mitu VMs Azure]on[multi-vm].

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. Selles artiklis kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

## <a name="architecture-diagram"></a>Arhitektuur skeem

On N-astme arhitektuurides variante. Enamasti, ei tohiks erinevused oluline soovituste eesmärgil. Selles artiklis eeldatakse tüüpiline 3-tasandi veebirakendus:

- **Web taseme.** Töötleb HTTP sissetulevad taotlused. Vastuste tagastatakse see taseme kaudu.

- **Äri taseme.** Rakendab äriprotsesside ja muude otstarbekas loogika süsteem.

- **Andmebaasi taseme.** Pakub püsivate andmed, kasutades [SQL serveri alati klõpsake kättesaadavus rühmad] [ sql-alwayson] jaoks kõrge kättesaadavus.

> Visio dokumendi, mis sisaldab arhitektuur diagramm on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on "Arvuta - mitme esimese taseme (Windows) lehe.

![[0]][0]

- **Kättesaadavus komplektid.** Luua mõne [Kättesaadavus] [ azure-availability-sets] iga taseme ja ette vähemalt kaks VMs iga astme. See lähenemine on vaja kättesaadavus [SLA] [ vm-sla] vms.

- **Alamvõrku.** Luua eraldi alamvõrgu iga taseme jaoks. Määrake aadress vahemik ja alamvõrgu maski abil [CIDR] märke. 

- **Laadi soolise.** Kasutada ka [Interneti-ühendusega laadi koormusetasakaalustusteenuse] [ load-balancer-external] levitada sissetuleva Interneti-liikluse web taseme ja ka [sisemise koormuse koormusetasakaalustusteenuse] [ load-balancer-internal] levitada võrguliikluse web teise taseme business.

- **Jumpbox**. Mõne _jumpbox_, nimetatakse ka [bastion host]on VM administraatorid kasutada ühenduse loomiseks muu VMs võrgus. Funktsiooni jumpbox on NSG, mis võimaldab remote liiklus ainult whitelisted avaliku IP-aadressid. Funktsiooni NSG lubama kaugtöölaua (RDP) liiklus.

- **Jälgimine**. Kontrolli tarkvara, nt [Nagios], [Zabbix]või [Icinga] annab teile ülevaate aega, VM sees ja teie süsteemi seisundi üldine. Kontrolli tarkvara installimine paigutatakse eraldi juhtimine alamvõrgu VM.

- **NSGs**. Kasutage [võrgu turberühmad] [ nsg] (NSGs) piirata sees on VNet võrguliiklust. Näiteks 3-tasandi arhitektuur joonisel, andmebaasi taseme ei aktsepteeri liiklus web front end business taseme- ja haldus alamvõrgu ülaservast.

- **SQL Server alati kättesaadavus rühma.** Pakub kõrge kättesaadavuse andmete taseme, võimaldades kopeerimise ja Tõrkesiirde.

- **Active Directory domeeni Services (AD DS) serverites**. Active Directory domeeniteenused (AD DS) salvestab andmed ja haldab suhtlemine kasutajate ja domeenide, sh kasutaja sisselogimise protsessid, autentimise ja directory otsingud. Active Directory domeenikontrolleri, mis on serveris, kus töötab AD DS. Enne Windows Server 2016, peab alati klõpsake kättesaadavus rühmad ühendatud domeeniga. See on kättesaadavus rühmad sõltuvad Windows Server Tõrkesiirde kobar (WSFC) tehnoloogia. Windows Server 2016 tutvustab tõrkesiirdeklastrite ilma Active Directory loomise võimalus. Lisateavet leiate teemast [mis on uut rakenduses tõrkesiirdeklastrite Windows Server 2016][wsfc-whats-new]

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme andnud mõni Azure ressursihaldur Mall viide arhitektuur soovituste järgneva installimiseks. Kui soovite luua oma viite arhitektuur nende soovituste juhul, kui teil on konkreetse nõude, mis ei sobi soovitus.

### <a name="vnet--subnets"></a>VNet / alamvõrku

Kui loote selle VNet, eraldada alamvõrku, peate aadressiruumi jaoks piisavalt. Määrake VNet aadress vahemik ja alamvõrgu mask abil [CIDR] märke. Kasutada aadressiruumi, mis kuulub standard [Privaatne IP-aadress plokid][private-ip-space], mis on 10.0.0.0/8, 172.16.0.0/12 ja 192.168.0.0/16.

Valige aadress vahemik, mis ei kattuks kohapealne võrgu, kui teil on vaja hiljem lüüsi vahel on VNet ja kohapealne võrgu häälestamine. Kui loote selle VNet, ei saa muuta aadress vahemikus.

Kujundage alamvõrku meeles funktsionaalsuse ja turvalisuse nõuetele. Kõik VMs sama taseme või rolli peaks minema sama alamvõrgu, mis võib olla turvalisus äärist. VNets ja alamvõrku kujundamise kohta leiate lisateavet teemast [leping ja kujunduse Azure virtuaalse võrgu][plan-network].

Määratlege iga alamvõrgu aadressiruumi jaoks alamvõrgu CIDR märke. Näiteks '10.0.0.0/24' loob mitmesuguseid 256 IP-aadressid. (VMs saate kasutada järgmisi 251; 5 on kaitstud. Lugege artiklit [virtuaalse võrgu KKK][vnet faq].) Veenduge, et aadresside vahemikud ei kattuks alamvõrku üle.

### <a name="network-security-groups"></a>Võrgu turberühmad

NSG reeglite abil saate piirata astme vahel. Näiteks 3-tasandi arhitektuur eespool näidatud, web taseme suhtlema otse andmebaasi taseme. Selle jõustamiseks tuleks andmebaasi taseme Blokeeri sissetulev liiklus web taseme alamvõrgu.  

  1. Mõne NSG loomine ja selle andmebaasi taseme alamvõrgu seostada.

  2. Lisada reegli, mis keelab kogu sissetulev liiklus on VNet kaudu. (Kasutage funktsiooni `VIRTUAL_NETWORK` reegli silti.) 

  3. Suurema tähtsusega, mis võimaldab sissetulev liiklus business taseme alamvõrgu reegli lisamine. See reegel alistab eelmise reegli ja võimaldab äri teise taseme andmebaasi rääkida.

  4. Lisage reegel, mis lubab sissetulev liiklus andmebaasi taseme alamvõrgu ise sees. See reegel võimaldab suhtlemine andmebaasi taseme kohta, mis on vajalikud andmebaasi tiražeerimine ja Tõrkesiirde VMs.

  5. Lisage reegel, mis lubab RDP liikluse aadressilt jumpbox alamvõrgu. See reegel võimaldab ühenduse loomiseks andmebaasi taseme soovitud jumpbox administraatorid.

  > [AZURE.NOTE] Mõne NSG on [vaikereeglit] [ nsg-rules] , mis lubavad Sissetuleva liikluse sees on VNet kaudu. Reegleid ei saa kustutada, kuid saate neid alistada, luues-prioriteet reegleid.

### <a name="load-balancers"></a>Koormus soolise

Välise koormuse koormusetasakaalustusteenuse jaotab Interneti-liikluse web taseme. Looge selles laadi koormusetasakaalustusteenuse avaliku IP-aadress. Vt [ka Interneti-ühendusega laadi koormusetasakaalustusteenuse loomine][lb-external-create].

Sisemise koormuse koormusetasakaalustusteenuse jaotab võrguliikluse web teise taseme business. Anda seda laadi koormusetasakaalustusteenuse privaatne IP-aadress, frontend IP konfiguratsiooni loomine ja seostada alamvõrgu business kiht. Vt [ka sisemise koormuse koormusetasakaalustusteenuse loomise alustamiseks][lb-internal-create].

### <a name="sql-server-always-on-availability-groups"></a>SQL Server alati kättesaadavus rühmad

Soovitame [Alati klõpsake kättesaadavus rühmade] [ sql-alwayson] SQL serveri kõrge saadavalolekut. Alati klõpsake kättesaadavus rühmade jaoks on vaja domeenikontrolleri. Kõik sõlmed kättesaadavus rühma peab olema sama AD domeeni.

Tasandi andmebaasiga ühenduse loomiseks on [kuulajale kättesaadavus rühma]kaudu[sql-alwayson-listeners]. Kuulajale võimaldab SQL-i kliendile ühenduse teadmata füüsilise SQL Serveri eksemplari nimi. Accessi andmebaasi ühendatud domeeni VMs. Klient (sel juhul teise taseme) kasutab DNS-i lahendamiseks on kuulajale virtuaalse võrgu nimi sisse IP-aadressid.

Konfigureerida SQL serveri alati järgmiselt:

- Windows Serveri Tõrkesiirde rühmitamise (WSFC) kobar ja SQL serveri alati klõpsake kättesaadavus rühma loomine. Lisateabe saamiseks lugege teemat [Alustamine alati klõpsake kättesaadavus rühmad][sql-alwayson-getting-started].

- Saate luua ka sisemise koormuse koormusetasakaalustusteenuse staatilise privaatne IP-aadress.

- Luua mõne kättesaadavus rühma kuulajale ja Vastendage on kuulajale DNS-i nimi on sisemine laadi koormusetasakaalustusteenuse IP-aadress. 

- Laadi koormusetasakaalustusteenuse reegli SQL serveri kuulata pordi (TCP port 1433 vaikimisi) loomine. Laadi koormusetasakaalustusteenuse reegli tuleb lubada _ujuv IP_, nimetatakse ka otse Server tagasi. See põhjustab VM vastamiseks klient, mis võimaldab otsest ühenduse peamine koopia.

    > [AZURE.NOTE] Kui ujuv IP on lubatud, ees pordi number peab olema sama mis laadi koormusetasakaalustusteenuse reegli tagaandmebaas pordinumber.

Kui SQL-i kliendile proovib ühendust luua, laadi koormusetasakaalustusteenuse marsruutimine ühendust loomast koopia, mis on praeguse primaarne. Kui teine koopia Tõrkesiirde, suunab laadi koormusetasakaalustusteenuse edaspidised taotlused automaatselt uue peamine koopia. Lisateabe saamiseks lugege teemat [konfigureerimine koormus koormusetasakaalustusteenuse SQL alati][sql-alwayson-ilb].

Ajal Tõrkesiirde, kliendi olemasolevad ühendused on suletud. Pärast selle Tõrkesiirde on lõpule jõudnud, suunatakse uued ühendused uue peamine koopia.

Kui teie rakendus teeb oluliselt rohkem kui kirjutab loeb, saate mõne teise koopia kirjutuskaitstud päringutega offload. Vt [kasutades on kuulajale ühendamiseks kirjutuskaitstud teisese koopia (kirjutuskaitstud marsruutimine)][sql-alwayson-read-only-routing].

Testige oma juurutuse [sunnib käsitsi Tõrkesiirde][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Luba juurdepääs RDP avaliku Interneti kaudu vms, mis töötavad rakenduse töökoormus. Selle asemel kõik need VMs RDP/SSH juurdepääsu peavad olema selle jumpbox kaudu. Administraator on jumpbox logib ja seejärel logib kaudu soovitud jumpbox VM. Funktsiooni jumpbox võimaldab RDP liikluse Interneti-ühendus, vaid ainult teadaolevad, whitelisted IP-aadressid.

Paigutage soovitud jumpbox sama VNet nimega teiste VMs, kuid eraldi juhtimine alamvõrgu.

Luua [avaliku IP-aadress] on jumpbox.

Kasutage VM väike jumpbox, nt standardne A1.

Saate konfigureerida selle NSGs web taseme, business taseme ja andmebaasi taseme alamvõrku haldus (RDP) liikluse läbida halduse alamvõrgu kaudu lubamiseks.

Selle jumpbox tagamiseks on NSG loomine ja rakendamine jumpbox alamvõrgu. Lisage NSG reegel, mis võimaldab RDP ühendused ainult kogumi whitelisted avaliku IP-aadressid.

Funktsiooni NSG saate manustatud alamvõrgu või jumpbox NIC. Sel juhul soovitatav manustamist NIC, seega RDP liikluse on lubatud ainult jumpbox, isegi siis, kui lisate teiste VMs sama alamvõrgu.

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Panna iga taseme või VM roll eraldi kättesaadavus kogum. Ärge viige VMs erinevate samu kättesaadavus. 

Veebisaidil andmebaasi taseme, võttes mitme VMs pole automaatse tõlkimise väga kättesaadav andmebaasi. Relatsiooniline andmebaasi, tavaliselt peate kasutama kopeerimise ja Tõrkesiirde kõrge-saadavus saavutamiseks. SQL serveri, soovitame kasutada [Alati klõpsake kättesaadavus rühmad][sql-alwayson]. 

Kui teil on vaja suurema kättesaadavuse [Azure'i SLA vms] [ vm-sla] pakub, korrata rakenduse kaks piirkondade ja Azure liikluse Tegumihalduris Tõrkesiirde. Lisateavet leiate teemast [Opsüsteemi Windows VMs mitme piirkonna jaoks kõrge-saadavus][multi-dc].   

## <a name="security-considerations"></a>Turvalisuse alused

Krüptige ülejäänud andmed. [Azure'i klahvi Vault] kasutada[ azure-key-vault] haldamiseks andmebaasi krüptimise võtmed. Klahv Vault saate talletada krüptimise võtmed riistvara turvalisus moodulid (HSMs). Lisateabe saamiseks vaadake jaotist [Konfigureerimine Azure'i klahvi Vault ühendamine SQL Server Azure'i VMs] [ sql-keyvault] samuti on soovitatav klahvi võlvkelder rakenduse saladusi, nt andmebaasi ühendusstringi, talletamiseks.

Soovi korral lisada võrgu virtuaalse seadme (Dessandi) loomiseks on DMZ avaliku Interneti- ja Azure virtuaalse võrgu vahel. Dessandi on üldise termini jaoks virtuaalse seadme, mida saate teha võrguga seotud toiminguid nagu tulemüüri, paketi kontrolli, auditeerimine, kohandatud marsruutimist või mitmesuguseid muid toiminguid. Lisateabe saamiseks lugege teemat [rakendada mõne DMZ Azure ja Interneti vahel][dmz].

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

Koormus soolise levitamine võrguliikluse veebi ja astme. Skaala horisontaalselt, lisades VM uued eksemplarid. Pange tähele, et saate muudate veebi ja astme sõltumatult, laadi põhjal. Võimalikud tüsistused põhjustatud säilitamise kliendi osaleja vähendamiseks tuleks kodakondsuseta VMs web astme. Äriloogika majutusteenuse VMs peaks olema kodakondsuseta.

## <a name="manageability-considerations"></a>Kaalutlused hallatavus

Lihtsustada haldamist kogu keskse administreerimiskeskuse tööriistadega nagu [Azure automatiseerimine][azure-administration], [Microsoft Operations Management Suite][operations-management-suite], [Chef][chef], või [nuku][puppet]. Nende tööriistade konsolideerida saadud mitme VMs esitada ülevaate süsteemi diagnostika- ja seisundi teave.

## <a name="solution-deployment"></a>Lahenduse juurutamine

Viide arhitektuuri, mis rakendab soovituste juurutamine on saadaval [github][github-folder]. See viide arhitektuur sisaldab web taseme, business taseme, andmete tasandi ning jumpbox VM ja Active Directory domeeni domeenikontrollerid.

Viide arhitektuur loovad alltoodud juhiseid järgides: 

1. Klõpsake nuppu.  
[! ["Juurutada Azure"] [1]][2]

2. Kui link on avatud Azure'i portaalis, sisestage jälgi väärtused. 
  - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Loo uus** ja sisestage `ra-ntier-sql-network-rg` tekstiväljale.
  - Valige piirkonnale **asukoht** rippmenüüst.
  - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
  - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
  - Klõpsake nuppu **osta** .

3. Kontrollige sõnumi juurutamise Azure portaali teatis on lõpule viidud.

4. Klõpsake nuppu.  
[! ["Juurutada Azure"] [1]][3]

5. Kui link on avatud Azure'i portaalis, sisestage jälgi väärtused. 
  - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Kasuta olemasolevaid** ja sisestage `ra-ntier-sql-workload-rg` tekstiväljale.
  - Valige piirkonnale **asukoht** rippmenüüst.
  - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
  - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
  - Klõpsake nuppu **osta** .

6. Kontrollige sõnumi juurutamise Azure portaali teatis on lõpule viidud.

7. Klõpsake nuppu.  
[! ["Juurutada Azure"] [1]][4]

8. Kui link on avatud Azure'i portaalis, sisestage jälgi väärtused. 
  - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Loo uus** ja sisestage `ra-ntier-sql-network-rg` tekstiväljale.
  - Valige piirkonnale **asukoht** rippmenüüst.
  - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
  - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
  - Klõpsake nuppu **osta** .

9. Kontrollige sõnumi juurutamise Azure portaali teatis on lõpule viidud.

10. Parameetri faile lisada on raske koodiga administraatori kasutajanimed ja paroolid ja see on tungivalt soovitatav kohe muuta nii kogu VMs. Iga VM Azure portaali klõpsake käsku **Lähtesta parool** tera **tugi + tõrkeotsing** . Valige rippmenüüst **režiimi** **parooli lähtestamine** ja seejärel valige uus **kasutajanimi** ja **parool**. Nuppu **Värskenda** jää püsima uue kasutajanime ja parooli. 

Täiendavaid võimalusi juurutada see viide arhitektuur kohta leiate teavet teemast readme-faili [juhiseid-ühe-vm] [ github-folder] Github kausta. 

## <a name="next-steps"></a>Järgmised sammud

See viide arhitektuur kõrge kättesaadavus saavutamiseks soovitame [juurutamine mitme piirkonna][multi-dc].

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[Bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[avaliku IP-aadress]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "Microsoft Azure'i kasutamine N-astme arhitektuur"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json