<properties
   pageTitle="Töötab mitu VMs | Viide arhitektuur | Microsoft Azure'i"
   description="Kuidas panna mitu VM eksemplari Azure'i skaleeritavus, paindlikkust, hallatavust ja turvalisus."
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
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Töötab mitu VMs Azure skaleeritavus ja kättesaadavuseks. 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

See artikkel kirjeldab tõestatud tavade töötab mitu virtuaalmasinates (VM) eksemplari parandamiseks skaleeritavus, kättesaadavus, hallatavust ja turvalisus.   

See arhitektuur levitatakse mitmes eksemplaris VM töökoormus. On ühe avaliku IP-aadress ja Interneti-liikluse levitatakse vms laadi koormusetasakaalustusteenuse abil. See arhitektuur saab ühe taseme rakenduse, nt kodakondsuseta web appi või salvestusruumi kobar. Samuti on koosteüksuse jaoks N taseme rakendused. 

Selles artiklis koostab töötab [Ühe VM Azure][single vm]. Selles artiklis soovitused kehtivad ka see arhitektuur.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Azure'i VMs jaoks on vaja võrgunduse ja toetavad.

> Visio dokumendi, mis sisaldab arhitektuur diagramm on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on funktsiooni "Arvuta - menüü mitme VM." 

![[0]][0]

Arhitektuur on järgmised osad: 

- **Määrake kättesaadavus.** [Kättesaadavus] [ availability set] sisaldab VMs ja on vajalik toetavad [kättesaadavus SLA Azure'i vms][vm-sla].

- **VNet**. Iga VM Azure juurutatakse virtuaalse emavõrku (VNet), mis on jagatud täpsemaks **alamvõrku**.

- **Azure'i laadi koormusetasakaalustusteenuse.** [Laadi koormusetasakaalustusteenuse] jaotab sissetuleva Interneti-päringud on kättesaadavus komplekti VM juhtudel. Laadi koormusetasakaalustusteenuse sisaldab teatud seotud ressursid.

  - **Avaliku IP-aadress.** Laadi koormusetasakaalustusteenuse saada Interneti-liikluse jaoks on vaja avaliku IP-aadressi.

  - **Ees konfigureerimine.** Seostab laadi koormusetasakaalustusteenuse avaliku IP-aadressi.

  - **Rakenduskausta tagaandmebaas aadress.** Sissetuleva liikluse saadud vms sisaldab võrgu liidesed (NICs).

- **Laadi koormusetasakaalustusteenuse reeglid.** Saab jagada hulgas kõik VMs tagaandmebaas aadress kogumi võrguliiklust. 

- **NAT reeglid.** Kasutatakse teatud VM liikluse marsruutimiseks. Näiteks kaugtöölaua protokoll (RDP) vms lubamiseks luua eraldi võrgu aadress tõlge (NAT) reegli iga VM. 

- **Võrgu liidesed (NICs)**. Iga VM on NIC, et võrguga.

- **Salvestusruumi.** Salvestusruumi kontod hoidke VM pilte ja muud failidega seotud ressursid, nt VM Diagnostikaandmete jäädvustatud Azure.

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme pakkunud allpool kirjeldatud soovitusi järgneva viite arhitektuur installimiseks mõni Azure ressursihaldur mall. Kui soovite luua oma viite arhitektuur peaksite nende soovituste täitmisel juhul, kui teil on konkreetse nõude, mis ei toeta soovituse. 

### <a name="availability-set-recommendations"></a>Kättesaadavus seadmine soovitused

Peate looma vähemalt kaks VMs seatud toetama [kättesaadavus SLA Azure'i vms]kättesaadavus[vm-sla]. Pange tähele, et Azure'i laadi koormusetasakaalustusteenuse ka et koormusetasakaalustusega VMs kuuluvad samu kättesaadavus.

### <a name="network-recommendations"></a>Võrgu soovitused

Laadi koormusetasakaalustusteenuse taha VMs peaks kõik asuma samas alamvõrgu. Ärge jätke VMs otse Interneti-ühendus, kuid selle asemel anda iga VM privaatne IP-aadress. Klientrakendustega abil laadi koormusetasakaalustusteenuse avaliku IP-aadress.

### <a name="load-balancer-recommendations"></a>Laadi koormusetasakaalustusteenuse soovitused

Lisada kõik VMs määramine laadi koormusetasakaalustusteenuse tagaandmebaas aadress kogumi kättesaadavus.

Saate määrata otsese võrguliiklust vms laadi koormusetasakaalustusteenuse reeglid. HTTP liikluse lubamiseks luua reegli, mis kaartide pordi 80 ees konfiguratsiooni pordi 80 tagaandmebaas aadress pool. Kui laadi koormusetasakaalustusteenuse saab taotluse avaliku IP-aadressi porti 80, suunab taotluse pordi 80 ühele NICs tagaandmebaas aadress kogumi.

Saate määrata teatud VM liikluse marsruutimiseks NAT reeglid. Näiteks lubada RDP vms luua eraldi NAT reegel iga VM. Iga reegli puhul, tuleks erinevate pordinumber vastendamine port 3389, default port RDP. (Näiteks kasutada port 50001 "VM1", "VM2," pordi 50002 ja jms.) Määrake NAT reeglite NICs VMs kohta. 

### <a name="storage-account-recommendations"></a>Salvestusruumi soovitusi

Luua eraldi Azure salvestusruumi kontod iga VM virtuaalse kõvaketta (VHDs) pihta sisend toimingute kohta teine [(IOPS) piirangud] vältimiseks hoidke[ vm-disk-limits] salvestusruumi kontode jaoks. 

Diagnostikalogide üks salvestusruumi konto luua. Selle konto salvestusruumi ühiselt kõik VMs.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

On kaks võimalust skaleerimist välja Azure VMs. 

- Laadi koormusetasakaalustusteenuse abil jaotada võrguliiklust kogu kogumi VMs. Välja mastaapimiseks, ette täiendavad VMs ja paneb laadi koormusetasakaalustusteenuse taha. 

- Kasutage [virtuaalse masina skaala komplekti][vmss]. Ulatuse määramine sisaldab identse VMs taha laadi koormusetasakaalustusteenuse speficied arvu. VM skaala määrab tugi autoscaling jõudluse mõõdikute põhjal. Kui VMs koormus suureneb, lisatakse täiendavaid VMs automaatselt laadi koormusetasakaalustusteenuse. 

Järgmiste jaotiste võrrelda need kaks võimalust.

### <a name="load-balancer-without-vm-scale-sets"></a>Laadi koormusetasakaalustusteenuse ilma VM skaala komplektid

Laadi koormusetasakaalustusteenuse võtab võrgu sissetulevad taotlused ja jaotab need üle NICs tagaandmebaas aadress kogumi. Mastaapimiseks horisontaalselt, lisada rohkem VM juhtudel kättesaadavus määramine (või deallocate mahtu VMs). 

Oletame näiteks, et teil on veebiserverisse. Laadi koormusetasakaalustusteenuse reegli pordi 80 ja/või pordi 443 lisate (SSL) jaoks. Kui klient saadab HTTP-taotluse, laadi koormusetasakaalustusteenuse huvitavat tagaandmebaas IP address abil [räsi algoritmi] [ load balancer hashing] , mis sisaldavad allika IP-aadress. Sel viisil levitatakse üle kõik VMs klientide päringutele. 

> [AZURE.TIP] Kui lisate uue VM saadavus määramine, veenduge, et luua mõne NIC VM ja NIC lisamiseks klõpsake Laadi koormusetasakaalustusteenuse pool tagaandmebaas aadress. Muul juhul ei Interneti-liikluse suunatakse uue VM.

Iga Azure'i tellimus on vaikimisi piirid kohast, sh maksimumarv VMs müüginäitajaid piirkonna kohta. Limiit suurendamiseks saate esitada tugiteenuse taotluse esitamine. Lisateabe saamiseks lugege teemat [Azure tellimuse ja teenuste piirangud, kvootide ja piiranguid][subscription-limits].  

### <a name="vm-scale-sets"></a>VM skaala komplektid 

VM skaala komplekti abil saate juurutada ja hallata identse VMs kogumist. Kõik VMs konfigureeritud sama, VM skaala komplektid toetavad true autoscale, ilma eelnevalt ettevalmistamise VMs hõlpsam koostada suuremahuliste teenuste suunamise suur Arvuta, big data ja konteinerite töökoormus. 

VM skaala komplekti kohta leiate lisateavet teemast [Virtuaalse masina skaala määrab ülevaade][vmss].

Kaalutlused VM skaala komplekti kasutamise kohta

- Kui teil on vaja kiiresti välja VMs mastaapimiseks või autoscale tuleb arvesse võtta skaala komplektid. 

- Praegu ei toeta skaala komplektid andmete ketast. Andmete talletamine suvandid on Azure failide talletamine, OS ketas, Temp ketas või välise talletamine, nt Azure Storage. 

- Läbivalt kogu VM jooksul automaatselt kuuluvad kättesaadavus samu 5 viga domeeni ja 5 värskenduse domeenid.

- Vaikimisi skaala komplekti kasutada "overprovisioning", mis tähendab, et skaala määramine algselt VMs rohkem kui palute jaoks sätted ja seejärel kustutab eest VMs. See parandab üldist edukust ettevalmistamise VMs. 

- Soovitame enam siis kui 20 VMs kohta salvestusruumi konto overprovisioning lubatud või pole enam kui 40 VMs koos overprovisioning keelatud.  

- Ressursihaldur Mallid leiate jaoks juurutamisel skaala määrab [Azure'i Kiirjuhend Mallid][vmss-quickstart].

- On kaks peamist võimalust konfigureerimine VMs juurutatud skaala määramine: luua kohandatud pildi või laiendid abil saate konfigureerida VM, kui see on ette valmistatud.

    - Sisseehitatud kohandatud pildile skaala kogum peate looma kõik OS ketta VHDs ühe salvestusruumi kontol. 

    - Kohandatud piltidega, peate pildi ajakohastamine.

    - Laiendiga, võib kuluda rohkem spin up äsja ettevalmistatud VM.

Täiendavad asjaolud, lugege teemat [Kujundamise VM skaala komplekti jaoks skaala][vmss-design].

> [AZURE.TIP]  Mis tahes mastaabi automaatselt lahenduse kasutamisel test see tootmise tase töökoormused varakult. 

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Kättesaadavus määramine muudab oma rakenduse paindlikumaks nii plaanitud ja plaanimata hooldustööd sündmused.

- _Kavandatud hooldustööd_ juhul, kui Microsoft värskendab aluseks platvorm, mõnikord põhjustab VMs tuleb taaskäivitada. Azure'i muudab kindel VMs on kättesaadavus komplekti ei ole kõik korraga taaskäivitada, vähemalt üks säilitatakse töötab samal ajal, kui teised on taaskäivitada.

- _Planeerimata hooldustööd_ juhtub, kui riistvara tõrke. Azure'i tagab VMs on kättesaadavus kogumis on rohkem kui üks serveri sektsioon üle ette valmistatud. See aitab vähendada riistvara tõrkeid, network katkestuste power segadusi ja muudeks tegevusteks.

Lisateabe saamiseks vaadake [haldamine virtuaalmasinates kättesaadavus][availability set]. Järgmises videos on hea ülevaade kättesaadavus komplekti: [Kuidas konfigureerida kättesaadavus seadmine skaala vms][availability set ch9]. 

> [AZURE.WARNING]  Veenduge, et määrata, kui olete ette VM konfigureerimine. Praegu on kuidagi lisada ressursihaldur VM-Saadavus: määrake pärast VM on ette valmistatud.

Laadi koormusetasakaalustusteenuse kasutab [seisund sondid] nende VM juhtudel. Kui mõne juures ei jõua eksemplari ajalõpu jooksul, laadi koormusetasakaalustusteenuse lõpetab liikluse saata selle VM. Siiski laadi koormusetasakaalustusteenuse jätkavad sond ja kui VM saab uuesti, laadi koormusetasakaalustusteenuse uuesti saata selle VM liikluse.

Siin on mõned soovitused laadi koormusetasakaalustusteenuse seisund sondid.

- HTTP- või TCP sondid testida. Kui teie VMs Käivita HTTP serveri (IIS-i, nginx, Node.js rakenduse jne), luua mõne HTTP juures. Muul juhul luua TCP juures.

- Määratlege soovitud HTTP juures HTTP lõpp-punkti tee. Funktsiooni juures kontrollib vastuse HTTP 200 selle tee. See võib olla juurkausta tee ("/") või seisundi jälgimine lõpp-punkti, mis rakendab mõned kohandatud loogika rakenduse seisundi kontrollimine. Lõpp-punkti peavad lubama anonüümse HTTP päringuid.

- Funktsiooni juures saadetakse [teadaolevad] [ health-probe-ip] 168.63.129.16 IP-aadress. Veenduge, et vältida liikluse või sealt see IP tulemüüri poliitika või võrgu turvalisuse rühma (NSG) reeglid.

- Kasutage [seisundi juures logid] [ health probe log] seisundi sondid oleku vaatamiseks. Luba logimine iga laadi koormusetasakaalustusteenuse Azure'i portaalis. Azure'i bloobimälu kirjutada logid. Logides mitu VMs tagasi lõpuks ei saanud võrguliikluse tõttu nurjunud juures vastused.

## <a name="manageability-considerations"></a>Kaalutlused hallatavus

Mitme VMs muutub automatiseerimiseks protsessid, nii et need on usaldusväärne ja korratavad oluline. Saate kasutada [Azure automatiseerimine] [ azure-automation] juurutamine, OS paikamine ja muude toimingute automatiseerimiseks. Azure'i automaatika on käivitatakse Azure'i, mis põhineb Windows PowerShelli automatiseerimine teenus. Näide automatiseerimise skriptide on saadaval TechNeti [Käitusjuhendi Galerii] .

## <a name="security-considerations"></a>Turvalisuse alused

Virtuaalne võrkude on liikluse eraldamise piiri Azure. Ühe VNet VMs ei saa suhelda otse erinevate VNet VMs. Sama VMs VNet saate suhelda, v.a juhul, kui loote [võrgu turberühmad] [ nsg] (NSGs) piirata liikluse. Lisateabe saamiseks lugege teemat [Microsofti pilveteenustega ja võrgu turvalisuse][network-security].

Laadi koormusetasakaalustusteenuse reegleid määratleda sissetulevate Interneti-liikluse jõuavad mis liikluse tagasi lõpuks. Siiski laadi koormusetasakaalustusteenuse reegleid ei toeta IP valge nimekirja, nii et kui soovite valge teatud avaliku IP-aadressid, lisada mõne NSG alamvõrgu.

## <a name="solution-deployment"></a>Lahenduse juurutamine

Viide arhitektuuri, mis rakendab soovituste juurutamine on saadaval github. See viide arhitektuur sisaldab virtuaalse võrgu (VNet), võrgu turberühma (NSG), laadi koormusetasakaalustusteenuse ja kahe virtuaalmasinates (VMs).

Viide arhitektuur saab kasutada Windowsi või Linuxi VMs alltoodud juhiseid järgides: 

1. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Kui link on avatud Azure'i portaalis, peate sisestama väärtused mõned sätted: 
    - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Kasuta olemasolevaid** ja sisestage `ra-multi-vm-rg` tekstiväljale.
    - Valige piirkonnale **asukoht** rippmenüüst.
    - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
    - Valige lahter, **Windowsi** või **Linuxi**rippmenüüst **Os tüüp** . 
    - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
    - Klõpsake nuppu **osta** .

3. Oodake, kuni juurutuse lõpuleviimiseks.

4. Parameetri faile lisada raske koodiga administraatori kasutajanime ja parooli ja soovitame teil kohe muuta nii. Klõpsake VM nimega `ra-multi-vm1` Azure'i portaalis. Seejärel klõpsake **parooli lähtestamine** tera **tugi + tõrkeotsing** . Valige rippmenüüst **režiimi** **parooli lähtestamine** ja seejärel valige uus **kasutajanimi** ja **parool**. Uue kasutajanime ja parooli salvestamiseks nuppu **Värskenda** . Korrake nimega VM `ra-multi-vm2`.

Täiendavaid võimalusi juurutada see viide arhitektuur kohta leiate teavet teemast readme-faili [juhiseid-ühe-vm] [ github-folder] GitHub kausta. 

## <a name="next-steps"></a>Järgmised sammud

Mitme VMs taha laadi koormusetasakaalustusteenuse ei koosteüksuse mitmekihilise arhitektuurides loomise kohta. Lisateabe saamiseks lugege teemat [Opsüsteemi Windows VMs N-astme arhitektuur Azure] [ n-tier-windows] ja [Töötab Linux VMs Azure N-astme arhitektuur][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[seisund sondid]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[Laadi koormusetasakaalustusteenuse]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Käitusjuhendi Galerii]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Arhitektuur mitme-VM lahenduse Azure, mis sisaldab kahte VMs ja laadi koormusetasakaalustusteenuse saadavus"
