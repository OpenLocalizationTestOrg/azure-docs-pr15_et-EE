<properties
   pageTitle="Azure'i arhitektuur viide - IaaS: Rakendamise Azure ja Interneti-ühendus on DMZ | Microsoft Azure'i"
   description="Kuidas rakendada Azure'i turvaline hü võrgu arhitektuur Interneti-ühendusega."
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

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>Azure'i ja Interneti-ühendus on DMZ rakendamine

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse head tavad turvaline hübriid võrku, mis ulatub kohapealse võrgu, mida aktsepteerib Azure'i võrgust Interneti-liikluse rakendamiseks. See viide arhitektuur laiendab kirjeldatud artiklis [rakendada mõne DMZ Azure ja teie asutusesisese andmekeskuse vahelise]arhitektuur[implementing-a-secure-hybrid-network-architecture]. Soovitatav on seda dokumenti lugeda ja aru, et viide arhitektuur enne selle dokumendi lugemist.

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. See viide arhitektuur kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste. 

Tüüpilised kasutamine karpide see arhitektuur on järgmised.

- Hübriidjuurutuse rakenduste kust töökoormused Käivita osaliselt kohapealse ja osaliselt teemast Azure.

- Azure'i infrastruktuuri, mis marsruudib sissetulev liiklus kohapealse ja Interneti-ühendus.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm olulisemad toimingud see arhitektuur olulised komponendid.

> Visio dokumendi, mis sisaldab selle arhitektuur diagrammi on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on "DMZ - avalik" lehel.

[! [0]][0]

- **Avaliku IP-aadress (PIP).** See on lõpp-punkti avaliku IP-aadress. Väliste kasutajate Interneti-ühendus, pääsete juurde süsteemi läbi selle aadressi.

- **Võrgu virtuaalse seadme (Dessandi).**  Dessandi on üldine termin, mis kirjeldab nagu ligipääs tulemüüri, optimeerimise WAN toimingute (sh võrgu tihendamise), kohandatud marsruutimist või muud võrgu funktsioonid ja ülesandeid täitvate VM.

- **Azure'i laadi koormusetasakaalustusteenuse.** Kõik sissetulevad taotlused Interneti kaudu seda laadi koormusetasakaalustusteenuse läbi ja jaotatud NVAs sisse avaliku DMZ sissetuleva alamvõrgu.

- **Avaliku DMZ sissetulev alamvõrgu.** Selle alamvõrgu aktsepteerib Azure laadi koormusetasakaalustusteenuse päringutele. Sissetulevad taotlused edastatakse NVAs DMZ sisse.

- **Avaliku DMZ väljaminev alamvõrgu.** Taotlusi, mis kinnitab selle Dessandi läbida selle alamvõrgu sisemise koormuse koormusetasakaalustusteenuse web kiht.

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme andnud mõni Azure ressursihaldur Mall viide arhitektuur soovituste järgneva installimiseks. Kui soovite luua oma viite arhitektuur peaksite nende soovituste täitmisel juhul, kui teil on konkreetse nõude, mis ei sobi soovitus.

### <a name="nva-recommendations"></a>Dessandi soovitused

Rakendada liikluse pärit internetis ja teise liikluse pärit kohapealse NVAs kogumit. See on kasutada ainult ühe komplekti NVAs mõlemad, kuna see kujundus pakub kahte liiki võrguliiklust vahel pole turvalisus perimeetri ohtu turvalisusele. See on kasulik kasutada seda kujundust, kuna see vähendab keerukus kontrolli reeglid turvalisus ja selgitab, millised reeglid vastavad iga sissetuleva võrgus taotluse. Näiteks NVAs kogumit rakendatakse ainult ajal teise reeglikomplekti NVAs rakendada kohapealse liikluse ainult Interneti-liikluse reeglid.

Kaasa kiht 7 Dessandi rakenduse ühendused tasemel Dessandi lõpetada ja hallata taustväärtus astme koos osaleja. See tagab sümmeetriline ühenduvuse vastuse liikluse aadressilt taustväärtus astme tagastab selle Dessandi kaudu.  

### <a name="public-load-balancer-recommendations"></a>Avaliku koormus koormusetasakaalustusteenuse soovitused ###

Skaleeritavus ja -saadavus säilitamiseks juurutamine avaliku DMZ sissetulev NVAs [kättesaadavus] on[ availability-set] ja kasutada [Interneti vastastikuste laadi koormusetasakaalustusteenuse] [ load-balancer] levitada Interneti-päringud üle NVAs kättesaadavus määramine.  

Taotlusi, ainult vajalikud Interneti-liikluse pordid laadi koormusetasakaalustusteenuse konfigureerimine. Näiteks piirata sissetuleva HTTP päringuid pordi 80 ja sissetuleva HTTPS päringuid pordi 443.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

Kujundada oma taristu Interneti vastastikuste laadi koormusetasakaalustusteenuse ette sissetuleva avaliku DMZ alamvõrgu algusest. Isegi juhul, kui teie arhitektuur initally nõuab ühe Dessandi, oleks lihtsam mastaapimiseks mitme NVAs tulevikus kui vastastikuste laadi koormusetasakaalustusteenuse Interneti-ühendus on juba juurutanud.

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Vastastikuste laadi koormusetasakaalustusteenuse Interneti nõuab iga Dessandi sisse avaliku DMZ sissetuleva alamvõrgu rakendada [seisundi juures][lb-probe]. Tervise juures, mis ei vasta selle näitaja loetakse saadaval ja laadi koormusetasakaalustusteenuse suunab taotluste muude NVAs sama kättesaadavus määramine. Pange tähele, et kui kõik NVAs ei vasta, rakenduse nurjub, seetõttu on oluline on jälgimine konfigureerida teatiste DevOps terve Dessandi eksemplaride arv langeb alla.

## <a name="manageability-considerations"></a>Kaalutlused hallatavus

Piira jälgimise ja halduse funktsiooni sissetulevate avaliku DMZ Dessandi kasutaja halduse alamvõrgu ainult kasti hüpata päringutele vastamiseks. Nagu [rakendada mõne DMZ Azure ja teie asutusesisese andmekeskuse vahelise] [ implementing-a-secure-hybrid-network-architecture] dokument, halduse alamvõrgu juurdepääsu piiramise ühe võrgu marsruudi kohapealse võrgu kaudu välja hüpata lüüs määratlemine.

Kui lüüsi ühenduvust kohapealse võrgu Azure ei tööta, ikka jõuate välja hüpata kasutades PIP, lisades hüpata väljale ning klõpsake remoting Interneti kaudu.

## <a name="security-considerations"></a>Turvalisuse alused

See viide arhitektuur rakendab turvalisuse mitu taset:

- Vastastikuste laadi koormusetasakaalustusteenuse Interneti suunab selle NVAs taotluste sissetuleva avaliku DMZ alamvõrgu ainult ning ainult vajalikud pordid.

- NSG reeglid sissetulevate ja väljaminevate avaliku DMZ alamvõrgu takistada selle NVAs väärkasutus blokeerides taotlusi, mis ei kuulu NSG reegleid.

- NAT marsruutimine konfigureerimine on NVAs suunab sissetulevad taotlused pordi 80 ja pordi 443 web taseme laadi koormusetasakaalustusteenuse, kuid ignoreerib kõiki muid pordid taotlusi.

Pange tähele, et logite sisse kõik pordid kõik sissetulevad taotlused. Regulaarselt audit logid, taotlusi, mis ei kuulu oodatud parameetrid, kuna see võib viidata sissetungi katsete tähelepanu.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Blokeeri/pass liikluse vahel rakenduse astme NSGs abil

Iga astme veebi-, äri- ja piirata nende vahel NSGs abil. Business taseme kasutab mõnda NSG blokeeri kogu liiklus, et ei pärinevad web taseme ja andmete taseme kasutab mõnda NSG blokeeri kogu liiklus, mis ei pärinevad business astme. Kui teil on nõue NSG reegleid laiema juurdepääsu nende astme laiendamiseks, kaaluda nende nõuete suhtes sellega. Iga uue sissetuleva rada kujutab juhusliku või otstarbeka andmete lekke või rakenduse kahju võimalus.

## <a name="solution-deployment"></a>Lahenduse juurutamine

Viide arhitektuuri, mis rakendab soovituste juurutamine on saadaval github. See viide arhitektuur sisaldab virtuaalse võrgu (VNet), võrgu turberühma (NSG), laadi koormusetasakaalustusteenuse ja kahe virtuaalmasinates (VMs).

Viide arhitektuur saab kasutada Windowsi või Linuxi VMs alltoodud juhiseid järgides: 

1. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Kui link on avatud Azure'i portaalis, peate sisestama väärtused mõned sätted: 
    - **Ressursirühm** nimi on juba määratletud parameetri faili, seega valige **Loo uus** ja sisestage `ra-public-dmz-network-rg` tekstiväljale.
    - Valige piirkonnale **asukoht** rippmenüüst.
    - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
    - Valige lahter, **Windowsi** või **Linuxi**rippmenüüst **Os tüüp** .
    - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
    - Klõpsake nuppu **osta** .

3. Oodake, kuni juurutamise lõpetamiseks.

4. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Kui link on avatud Azure'i portaalis, peate sisestama väärtused mõned sätted: 
    - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Loo uus** ja sisestage `ra-public-dmz-wl-rg` tekstiväljale.
    - Valige piirkonnale **asukoht** rippmenüüst.
    - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
    - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
    - Klõpsake nuppu **osta** .

6. Oodake, kuni juurutuse lõpuleviimiseks.

7. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Kui link on avatud Azure'i portaalis, peate sisestama väärtused mõned sätted: 
    - **Ressursirühm** nimi on juba määratletud parameetri faili, seega valige **Kasuta olemasolevaid** ja sisestage `ra-public-dmz-network-rg` tekstiväljale.
    - Valige piirkonnale **asukoht** rippmenüüst.
    - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
    - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
    - Klõpsake nuppu **osta** .

9. Oodake, kuni juurutuse lõpuleviimiseks.

10. Parameetri faile lisada raske koodiga administraatori kasutajanime ja parooli kõik vms ja soovitame teil kohe muuta nii. Jaoks iga VM juurutuse, valige see Azure portaali ja seejärel klõpsake **parooli lähtestamine** tera **tugi + tõrkeotsing** . Valige rippmenüüst **režiimi** **parooli lähtestamine** ja seejärel valige uus **kasutajanimi** ja **parool**. Klõpsake nuppu **Uuenda** , et püsida.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Turvaline hü võrgu arhitektuur"
