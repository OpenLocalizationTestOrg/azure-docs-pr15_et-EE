<properties
   pageTitle="Mitme VIP Azure laadimine koormusetasakaalustusteenuse | Microsoft Azure'i"
   description="Klõpsake Azure laadi koormusetasakaalustusteenuse mitme VIP ülevaade"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Mitme VIP Azure laadimine koormusetasakaalustusteenuse

Azure'i laadimine koormusetasakaalustusteenuse võimaldab teil laadida saldo teenuste mitme pordid, mitu IP-aadressi või mõlemad. Saate sise- ja avaliku laadi koormusetasakaalustusteenuse määratlused laadimiseks saldo puhul kogu kogumi VMs.

Selles artiklis kirjeldatakse põhialuste selle võimaluse, olulist põhimõtet ja piiranguid. Kui kavatsete nähtavaks tegemine teenuste IP-aadressi, võite leida lihtsustatud juhised [avaliku](load-balancer-get-started-internet-portal.md) või [sisemise](load-balancer-get-started-ilb-arm-portal.md) laadida koormusetasakaalustusteenuse. Mitme VIP on suureneva ühe VIP konfigureerimine. Selle artikli mõisted kasutamisel saate laiendada lihtsustatud konfiguratsiooni igal ajal.

Kui määratlete mõne Azure'i laadi koormusetasakaalustusteenuse, frontend ja kirjutamata konfiguratsioon on seotud reeglid. Kuidas uute määratlemiseks kasutatakse seisundi juures, viidatud reeglit saadetakse sõlm taustväärtus kogumi. On frontend on määratletud, virtuaalse IP (VIP), mis on IP-aadress (avalik või sisemine), transport protocol (UDP või TCP) ja pordi number 3 korteeži. DIP on IP-aadressi Azure virtuaalse NIC, mis on seotud taustväärtus kogumi VM.

Järgmine tabel sisaldab mõni näide frontend konfiguratsioone:

| VIP | IP-aadress | Protocol (protokoll) | Port |
|-----|------------|----------|------|
|1|65.52.0.1|TCP|80|
|2|65.52.0.1|TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|TCP|80|

Tabelis on neli eri frontends. Frontends #1, #2 ja #3 on ühe VIP mitme reegliga. Sama IP-aadressi kasutatakse, kuid iga frontend erineb pordi või Protocol (protokoll). Frontends #1 ja #4 on näide mitme VIP, kus sama frontend protokolli ja port on mitu VIP taaskasutada.

Azure'i laadimine koormusetasakaalustusteenuse pakub paindlikkust määratlemisel laadi tasakaalustamiseks reeglid. Reegli teatab, kuidas aadress ja pordi on frontend on vastendatud Sihtaadress ja selle kirjutamata porti. Kas taustväärtus pordid on taaskasutada reeglid sõltub selle reegli tüüp. Erinevat tüüpi reegel on teatud nõuded, mis võivad mõjutada host konfigureerimine ja juures kujundus. On kahte tüüpi reegleid:

1. Vaikimisi reegel koos pole kirjutamata pordi korduvkasutamine
2. Ujuv IP reegli, kus sihttermin kirjutamata pordid

Azure'i laadimine koormusetasakaalustusteenuse võimaldab teil mix reeglit mõlemat sama laadi koormusetasakaalustusteenuse konfiguratsiooni. Laadi koormusetasakaalustusteenuse saate neid kasutada korraga antud VM või mis tahes kombinatsiooni seni, kuni te järgima reegli piiranguid. Millised valige reegli tüüp sõltub rakenduse nõuded ja keerukuse toetavad selle konfiguratsiooni. Tuleks hinnata, milliste reegli sobivad eelkõige teie stsenaariumi.

Need stsenaariumid täpsemaks uurimiseks alustades vaikekäitumise.

## <a name="rule-type-1-no-backend-port-reuse"></a>Reegel # 1: pole kirjutamata pordi korduvkasutamine

![MultiVIP illustratsioon](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

Selle stsenaariumi korral on frontend VIP konfigureeritud järgmiselt:

| VIP | IP-aadress | Protocol (protokoll) | Port |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

DIP on Sissetuleva meilivoo sihtkohta. Iga VM seab sisse kirjutamata kordumatu porti SUPLUST soovitud teenuse. See teenus on seostatud frontend kaudu reegli määratlus.

Kahe reeglite määratlemine

| Reegli | Kaardi frontend | Taustväärtus ühendada |
|------|--------------|-----------------|
| 1 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![taustväärtus](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![taustväärtus](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![taustväärtus](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![taustväärtus](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Täieliku vastenduse rakenduses Azure'i laadi koormusetasakaalustusteenuse on nüüd järgmiselt:

| Reegli | VIP IP-aadress | Protocol (protokoll) | Port | Sihtkoht | Port |
|------|----------------|----------|------|-----|------|
|![reegli](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|Pane IP-aadress|80|
|![reegli](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|Pane IP-aadress|81|

Iga reegli puhul esitama vool sihtkoha IP-aadress ja sihtport kordumatu kombinatsioon. Sihtport kulgemist muutes suudab mitme reeglite puhul sama DIP erinevate pordid kohta.

Seisund sondid on alati suunatud DIP VM. Te peate veenduma, et teie juures kajastab seisundi VM.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Reegel # 2: taustväärtus port taaskasutuse ujuv IP abil

Azure'i laadi koormusetasakaalustusteenuse annab paindlikkuse uuesti kasutada frontend pordi üle mitme VIP, olenemata kasutatud reegli tüüp. Lisaks mõned rakenduse stsenaariumid eelistate või nõua sama pordi kasutavad mitu rakenduse eksemplari ühes VM taustväärtus kogumi. Levinud näited pordi taaskasutuse jaoks kõrge-saadavus rühmitamise kaasata, võrgu virtuaalne seadmed ja asetades mitme uuesti krüptimata TLS-i lõpp-punktid.

Kui soovite uuesti kasutada kirjutamata pordi üle mitme reeglid, tuleb lubada ujuv IP reegli määratluses.

Ujuv IP on osa nn nimega otsese serveri tagastada (DSR). DSR koosneb kahest osast: meilivoo topoloogia ja mõne IP-aadressi vastenduse värviskeemi. Platvormi tasemel Azure'i laadi koormusetasakaalustusteenuse toimib alati DSR meilivoo topoloogia sõltumata sellest, kas ujuv IP on lubatud või mitte. See tähendab, et vool väljaminev osa on alati õigesti ümber vool otse naasmiseks päritolu.

Vaikimisi reegli tüüp, mille seab Azure'i tasakaalustamiseks IP aadressi vastenduse värviskeemi kasutamise hõlbustamiseks traditsiooniline koormus. Ujuv IP lubamine muudab IP aadressi vastenduse värviskeemi lubamiseks paindlikkuse, nagu allpool.

Järgmine diagramm näitab selle konfiguratsiooni.

![MultiVIP illustratsioon](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

Selle stsenaariumi korral on igal pool kirjutamata VM kolme võrgu liidesed:

* DIP: virtuaalse NIC seostatud VM (Azure NIC ressurss)
* VIP1: loopback liides sees Külastajate OS, mis on konfigureeritud VIP1 IP-aadress
* VIP2: loopback liides sees Külastajate OS, mis on konfigureeritud VIP2 IP-aadress

>[AZURE.IMPORTANT] Loogika liideste konfigureerimine toimub Külastajate OS sees. Selle konfiguratsiooni on liita või hallata Azure. Selle konfiguratsiooni puhul ilma reegleid ei tööta. Seisund juures määratlused kasutada VM asemel loogilise VIP DIP. Seetõttu teenust peab võimaldama juures vastuste DIP Port, mis loogilise VIP pakutav teenus olekut.

Oletame sama frontend konfiguratsiooni, nagu eelmises näites:

| VIP | IP-aadress | Protocol (protokoll) | Port |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

Kahe reeglite määratlemine

| Reegli | Kaardi frontend | Taustväärtus ühendada |
|------|--------------|-----------------|
| 1 | ![reegli](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![taustväärtus](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (ja VM1 VM2) |
| 2 | ![reegli](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![taustväärtus](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (ja VM1 VM2) |

Järgmine tabel näitab täieliku vastenduse laadi koormusetasakaalustusteenuse:

| Reegli | VIP IP-aadress | Protocol (protokoll) | Port | Sihtkoht | Port |
|------|----------------|----------|------|-------------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|sama, mis VIP (65.52.0.1)|sama, mis VIP (80)|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|sama, mis VIP (65.52.0.2)|sama, mis VIP (80)|

Sissetuleva meilivoo sihtkoht on VIP aadress tagastusliidese VM. Iga reegli puhul esitama vool sihtkoha IP-aadress ja sihtport kordumatu kombinatsioon. Erineva voogu sihtkoha IP-aadress, on võimalik sama VM pordi taaskasutuse. Laadi koormusetasakaalustusteenuse teenust on avatud, sidumine on VIP IP-aadress ja port vastav tagastusliidese.

Pange tähele, et selles näites on sihtport ei muuda. Isegi juhul, kui see on ujuv IP stsenaarium, toetab Azure laadi koormusetasakaalustusteenuse ka reegli ümberkirjutamine kirjutamata sihtport ja teha erineb frontend sihtport määratlemine.

Ujuv IP reegli tüüp on mitu laadi koormusetasakaalustusteenuse konfigureerimine mustrite. Üks näide, mis on praegu saadaval on [SQL AlwaysOn koos mitme kuulajatele](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) konfiguratsioon. Aja jooksul me ei dokumendi veel järgmisi olukordi.

## <a name="limitations"></a>Piirangud

* Mitme VIP konfiguratsioone on toetatud ainult IaaS VMs.
* Reegliga ujuv IP kasutama rakenduse DIP Väljamineva meili puhul. Kui rakenduse seob konfigureeritud tagastusliidese sisse külalisena OS VIP-aadressi, siis SNAT pole saadaval kirjutada väljamineva meilivoo ja voogu nurjub.
* Avaliku IP-aadresside mõjutavad arveldamine. Lisateabe saamiseks vt [IP-aadress hinnad](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Tellimuse piirangud kehtivad. Lisateavet leiate teemast [teenuse piirangud](../azure-subscription-service-limits.md#networking-limits) üksikasjad.
