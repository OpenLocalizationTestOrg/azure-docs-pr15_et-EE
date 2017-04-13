## <a name="virtual-network-basics"></a>Virtuaalse võrgu põhialused

### <a name="what-is-an-azure-virtual-network-vnet"></a>Mis on Azure virtuaalse võrgustik (VNet)?

Te saate VNets abil säte ja hallata Azure virtuaalse privaatse võrgu (VPN) ja soovi korral võite linkida VNets muude VNets Azure või oma kohapealse IT taristu hübriid või asutusesiseses lahenduste loomiseks. Iga VNet loomist on oma CIDR plokk ja saab linkida muude VNets ja kohapealse võrgu kui CIDR blocks ei põrkuvad. Peate VNets DNS-i sätted, juhtelementide ja jaotamine on VNet alamvõrku sisse.

Kasutage VNets abil.

- Sihtotstarbeline vaid virtuaalse privaatvõrgu loomine

    Mõnikord ei nõua asutusesiseses konfigureerimine oma lahenduse. Mõne VNet loomisel oma teenustele ja oma VNet VMs saate otse ja turvaline omavahel suhelda pilveteenuses. See hoiab liikluse turvaliselt soovitud VNet, kuid siiski lubab teil konfigureerida lõpp-punkti ühendused VMs ja teenuseid, mis nõuavad Interneti-side teie lahendus osana.

- Turvaline oma andmekeskuse laiendamine

    VNets, saate koostada traditsiooniline saidi saidi (S2S) VPN turvaliselt mastaapimiseks oma andmekeskuse võimsus. S2S VPN kasutavad IPSEC turvalist ühendust oma ettevõtte VPN-lüüsi ja Azure vahel.

- Luba hübriid pilve stsenaariumid

    VNets annab teile paindlikkuse toetamiseks hübriid pilve stsenaariumid mitmesuguseid. Saate mis tahes tüüpi kohapealse nagu mainframes ja Unix süsteemide rakenduste pilvepõhist turvaliselt ühendada.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Kuidas teada saada, kui vajan virtuaalse võrgu?

Külastage otsust tabeli, mis aitavad teil otsustada, on parim võrgu kujundus valik teile vaatamiseks [Virtuaalse võrgu ülevaade](../articles/virtual-network/virtual-networks-overview.md) .

### <a name="how-do-i-get-started"></a>Kuidas alustada?

Külastage [virtuaalse võrgu dokumentatsiooni](https://azure.microsoft.com/documentation/services/virtual-network/) alustada. See leht sisaldab linke levinud konfigureerimistoimingute nagu teave, mis aitab teil mõista asju, mida peate arvestama virtuaalse võrgu kujundamisel.

### <a name="what-services-can-i-use-with-vnets"></a>Milliseid teenuseid saate kasutada VNets?

VNets saate kasutada mitmesuguseid eri Azure teenuseid, nt pilveteenustega (PaaS), Virtuaalmasinates ja veebirakenduste. Kuid on mõned teenused, mis ei toeta mõnda VNet. Kontrollige kindla teenuse, mida soovite kasutada, ja veenduge, et see oleks ühilduv.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Saate kasutada VNets ilma asutusesiseses ühendus?

Jah. Saate mõne VNet ilma saitide ühenduse kaudu. See on eriti kasulik, kui soovite käivitada domeenikontrollerid ja SharePointi parkides Azure.

## <a name="virtual-network-configuration"></a>Virtuaalse võrgu konfigureerimise

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Milliseid tööriistu kasutada loomiseks on VNet?

Saate luua või konfigureerida virtuaalse võrgu järgmiselt:

- Azure'i portaalis (Classic ja ressursihaldur VNets).

- Võrgu konfigureerimise faili (netcfg – klassikaline VNets ainult). Vaadake [konfigureerimine virtuaalse võrgu network configuration faili abil](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShelli (Classic ja ressursihaldur VNets).

- Azure'i CLI (Classic ja ressursihaldur VNets).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Aadresside vahemikud mis saab minu VNets kasutada?

Saate kasutada avaliku IP-aadresside vahemikud ja mis tahes IP-aadresside vahemiku määratletud [RFC](http://tools.ietf.org/html/rfc1918)1918.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Kas ma saan avaliku IP-aadresside minu VNets?

Jah. Avaliku IP-aadresside vahemikud kohta leiate lisateavet teemast [avaliku IP-aadress ruumi virtuaalse võrku (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Pidage meeles, et oma avaliku IP-d ei pääse otse Interneti kaudu.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Kas minu virtuaalse võrgu alamvõrku arv on piiratud?

Ei ole piiratud alamvõrku, saate kasutada mõnda VNet arvu. Kõik alamvõrku peavad sisaldama täielikult virtuaalse võrgu aadressiruumi ja peaks omavahel ei kattuks.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Kas piirangutest IP-aadresside sees need alamvõrku kasutamise kohta?

Azure'i jätab mõne IP-aadressid iga alamvõrgu sees. Protokoll vastavuse koos 3 rohkem aadresse, mis on Azure teenuste jaoks kasutada, on mõeldud esimese ja viimase IP-aadressid on alamvõrku.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Kuidas ja kuidas väikesed saab VNets ja alamvõrku olema?

Väikseim alamvõrgu toetame on /29 ja suurim on soovitud /8 (abil CIDR alamvõrgu määratlused).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Tuua minu VLANs Azure VNets abil?

Ei. VNets on Layer 3 ülekatete. Azure'i ei toeta mõnda kiht-2 semantika.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Saate määrata kohandatud suunamise poliitikaid minu VNets ja alamvõrku?

Jah. Saate kasutada kasutaja määratletud marsruutimist (UDR). UDR kohta lisateabe saamiseks külastage [kasutaja määratletud marsruudib ja IP edastamine](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Kas VNets toetavad multisaate või leviedastuse?

Ei. Me ei toeta multisaate või leviedastuse.

### <a name="what-protocols-can-i-use-within-vnets"></a>Millised Protokollid saate kasutada jooksul VNets?

Saate kasutada standard IP-põhise Protokollid VNets sees. Siiski multisaate, leviedastuse, IP--IP kapseldatud paketid ja üldise marsruutimine kapseldus (mängu) paketid on blokeeritud VNets sees. Standardse protokollid, mis töötavad järgmised.

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Saate oma vaikimisi ruuterid sees on VNet ping?

Ei.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Saate kasutada tracert diagnoosimine connectivity?

Ei.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Alamvõrku saate lisada soovitud VNet loomise järel?

Jah. Alamvõrku saate lisada VNets igal ajal kui alamvõrgu aadress pole osa on VNet teise alamvõrgu.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Minu alamvõrgu suurust saab muuta pärast seda luua?

Saate lisada, eemaldada, laiendamine või kahandamine on alamvõrgu kui puuduvad VMs või teenuste juurutatud kogumahu PowerShelli cmdlet-käskude või NETCFG faili abil. Samuti saate lisada, eemaldada, laiendamine või kahandamine mis tahes eesliiteid kui alamvõrku, mis sisaldavad VMs või teenuste muutmine ei mõjuta.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Saate pärast nende loodud alamvõrku muuta?

Jah. Saate lisada, eemaldada ja muuta kasutatavat on VNet CIDR plokid.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Saate ühendada Interneti kui mul on VNet töötab minu teenuseid?

Jah. Kõigi teenuste juurutatud sees on VNet saate Interneti-ühenduse. Iga juurutatud Azure pilveteenuses on määratud avalikult adresseeritavad VIP. Kas peate määratlema Sisestuskeel lõpp-punktide jaoks PaaS rollid ja virtuaalmasinates lubamiseks ühendused Internetist nõustuda nende teenuste jaoks lõpp-punktid.

### <a name="do-vnets-support-ipv6"></a>VNets ei toeta protokolli IPv6?

Ei. Te ei saa kasutada IPv6 VNets sel ajal.

### <a name="can-a-vnet-span-regions"></a>Mõne VNet võib hõlmata piirkondade?

Ei. Piiratud ühe regioon on VNet.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>On VNet saate ühendada mõne muu VNet Azure?

Jah. Saate luua VNet VNet teatise REST API-d või Windows PowerShelli abil. Samuti saate luua ühenduse VNets VNet silmitsemine kaudu. Üksikasjalikumat teavet silmitsemine [siin.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Nimelahendus (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Mis on minu DNS-i võimalused VNets?

Kasutage otsust tabeli lehel [Nimelahendus VMs ja rolli aknad](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) juhatavad teid kõik DNS-i suvandid saadaval.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Saate määrata DNS-serverid on VNet jaoks?

Jah. Saate määrata DNS-i serveri IP-aadresside VNet sätteid. See rakendatakse vaikimisi DNS-i server jaoks soovitud VNet kõik VMs nimega.

### <a name="how-many-dns-servers-can-i-specify"></a>Kui palju DNS-serverid saate määrata?

Saate määrata kuni 12 DNS-serverid.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Oma DNS-serverid saab muuta pärast seda, kui võrguühendus on loodud?

Jah. Saate muuta loendis DNS-i server oma VNet igal ajal. Kui soovite muuta oma DNS-i serveri loend, peate uuesti iga VMs neid oma VNet hooleks uue DNS-i serveri.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Mis on antud Azure'i DNS-i ja see töötab VNets?

Azure'i antud DNS-i on Microsofti pakutav mitme rentniku DNS-i teenus. Azure'i registreerib kõik teie VMs ja rolli aknad seda teenust. See teenus pakub nimelahendus hostname VMs ja rolli aknad sisalduvad sama pilveteenuses ja FQDN VMs ja sama VNet rolli aknad.

> [AZURE.NOTE] Seal on piirang seekord esimese 100 pilveteenustega virtuaalse võrgu rist – rentniku nimelahendus Azure'i antud DNS-i abil. Kui kasutate oma DNS-i server, seda piirangut rakendada.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Saate oma DNS-i sätete kohta VM alistada / teenuse alus?

Jah. Saate määrata DNS-serverid teenus cloud kohta eraldi võrgu vaikesätted. Soovitame siiski kasutada võimalikult palju võrgu kogu DNS-i.

### <a name="can-i-bring-my-own-dns-suffix"></a>Saate tuua oma DNS-i järelliite?

Ei. Te ei saa määrata kohandatud DNS-i järelliite oma VNets.

## <a name="vnets-and-vms"></a>VNets ja VMs

### <a name="can-i-deploy-vms-to-a-vnet"></a>VMs saavad võtta kasutusele mõne VNet?

Jah.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Linux VMs saate võtta kasutusele mõne VNet?

Jah. Saab kasutada mis tahes distributsiooni Azure'i ei toeta Linux.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Mis vahe on avalik VIP ja sisemise IP-aadress?

- Sisemise IP-aadress on IP-aadress, mis on määratud iga VM sees on VNet DHCP. Avalike veebilehtede ei ole. Kui olete loonud mõne VNet, sisemine IP-aadress on määratud alamvõrgu sätetest oma VNet määratud vahemiku. Kui teil on VNet määratakse endiselt sisemise IP-aadress. Sisemise IP-aadress jääb koos VM tööiga, v.a juhul, kui mis on deallocated VM.

- Avaliku VIP on määratud pilve teenuse või laadi koormusetasakaalustusteenuse avaliku IP-aadress. See pole määratud otse oma VM NIC. VIP, jääb see on määratud, kuni kõik VMs, et pilveteenuses on deallocated või kustutatud pilveteenusesse. See on sel hetkel välja.

### <a name="what-ip-address-will-my-vm-receive"></a>Milliseid IP-aadress minu VM saavad?

- **Sisemise IP-aadress-** VM juurutamisel on VNet VM saab kogumi sisemise IP-aadressid, mis teie määratud sisemine IP-aadress. VMs suhelda sees on VNets sisemise IP-aadresside abil. Kuigi Azure'i määrab dünaamilise sisemise IP-aadressi, saate taotleda staatilise aadressi jaoks oma VM. Staatilise sisemise IP-aadresside kohta leiate lisateavet külastage [sisemine staatiline seadmise kohta](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** Teie VM on seostatud VIP, kuigi VIP pole kunagi määratud VM otse. VIP on saab määrata oma pilveteenuses avaliku IP-aadress. Soovi korral saate, broneerida VIP oma pilveteenuses.

- **ILPIP-** Samuti saate konfigureerida eksemplari taseme avaliku IP-aadress (ILPIP). ILPIPs on otseselt seotud VM, mitte pilveteenusesse. ILPIPs kohta lisateabe saamiseks külastage [Eksemplari taseme avaliku IP ülevaade](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kas sisemise IP-aadressi saate reserveerida VM on loodud hiljem?

Ei. Te ei saa reserveerida sisemise IP-aadress. Kui sisemine IP-aadress on saadaval see määratakse VM või rolli eksemplari DHCP-server. Selle VM võib-olla või see võib olla seda, mida soovite sisemise IP-aadressi määramiseks. Siiski saate muuta sisemise IP-aadress on juba loodud VM mis tahes saadaval sisemise IP-aadress.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Tehke sisemine IP aadressid muutus on VNet VMs?

Jah. Sisemise IP-aadressid jäävad koos VM tööiga juhul, kui on deallocated VM. Kui VM on deallocated, sisemine IP-aadress on välja juhul, kui teie VM jaoks määratletud staatiline sisemise IP-aadress. Kui VM on lihtsalt peatada (ja pange oleku **peatamiseni (Deallocated)**), jääb määratud VM IP-aadress.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Saate käsitsi määrata IP-aadresside abil NICs vms?

Ei. Ei tohi muuta VMs mis tahes kasutajaliidese atribuudid. Muudatused võivad viia potentsiaalselt kaotada ühenduvuse VM.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Mis juhtub minu IP-aadressid kui VM välja lülitada?

Midagi. IP-aadressid (avaliku VIP ja sisemise IP-aadress) jäävad pilveteenuses või VM.

> [AZURE.NOTE] Kui soovite lihtsalt sulgeda VM, ärge kasutage selleks portaalis haldus. Praegu on sulgemise nupp deallocate virtuaalse masina.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Saate teisaldada VMs ühe alamvõrgu teise alamvõrku, klõpsake soovitud VNet ilma uuesti juurutamine?

Jah. Lisateavet leiate [siit](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Staatilise MAC aadressi saate konfigureerimine minu VM?

Ei. MAC-aadressi ei saa staatiliselt konfigureeritud.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>MAC-aadress jääb samaks minu VM pärast selle loomist?

Jah, MAC-aadressi samaks VM kuigi VM on peatatud (deallocated) ja uuendatud.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Saate ühendada Interneti kaudu a on VNet VM?

Jah. Kõigi teenuste juurutatud sees on VNet saate Interneti-ühenduse. Lisaks on iga pilveteenuses juurutatud Azure avalikult adresseeritavad VIP määratud. Peate määratlema Sisestuskeel lõpp-punktide jaoks PaaS rollid ja vms lubamiseks nende teenuste ühendused Internetist nõustuda lõpp-punktid.

## <a name="vnets-and-services"></a>VNets ja -teenused

### <a name="what-services-can-i-use-with-vnets"></a>Milliseid teenuseid saate kasutada VNets?

Saate kasutada ainult Arvuta teenuste VNets. Arvutage teenustele kehtivad ainult pilveteenustega (web ja töötaja rollid) ja VMs.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Kas saan kasutada veebirakenduste virtuaalse võrguga?

Jah. Veebirakenduste sees on VNet ASE (rakenduse teenuse keskkond) abil saate juurutada. Lisamine, mis veebirakenduste turvaliselt ühendada ja ressursside oma Azure'i VNet juurde, kui teil on punkti saidi jaoks oma VNet konfigureeritud. Lisateabe saamiseks vaadake järgmist:


- [Veebirakenduste loomise rakendus teenuse keskkonnas](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Web Appsi virtuaalse võrgu integreerimine](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [VNet integreerimine ja hübriid ühendused Web Apps abil](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Integreerimine Azure virtuaalse võrgustik web app](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Juurutada pilveteenused veebi ja töötaja rollid (PaaS) on VNet?

Jah. Saate juurutada PaaS teenuste VNets.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Kuidas on VNet PaaS rollid juurutada?

Saate selle saavutamiseks määramisega VNet nimi ja rolli /subnet vastendused oma teenuse konfigureerimine jaotises võrku konfigureerimine. Teil pole vaja värskendada oma kahendfaile ühte.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Saate teisaldada oma teenuste ja sealt VNets?

Ei. Ei saa teisaldada ja sealt VNets teenused. On teil kustutada ja teenuse teisaldamiseks teise VNet uuesti kasutuselevõtuks.

## <a name="vnets-and-security"></a>VNets ja turve

### <a name="what-is-the-security-model-for-vnets"></a>Mis on VNets mudeli turvalisus?

VNets on üksteisest täielikult eraldatud ja muude teenuste majutatud Azure infrastruktuuri. Mõne VNet on usalda piiri.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Saate määrata ACL-ID või NSGs minu VNets kohta?

Ei. Te ei saa seostada ACL-ID või NSGs VNets abil. Siiski ACL-ID saate määratleda Sisestuskeel lõpp-punktid vms, et on VNets juurutatud ja NSGs võib olla seotud alamvõrku või NICs.

### <a name="is-there-a-vnet-security-whitepaper"></a>Kas VNet turvalisus lühiülevaade?

Jah. Saate selle tasuta alla [siin](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>API-d, skeeme ja tööriistad

### <a name="can-i-manage-vnets-from-code"></a>Saate hallata VNets koodi?

Jah. REST API-de abil saate VNets ja asukohaga Ühenduvus. Lisateavet leiate [siit](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Kas instrumentaarium tugi VNets?

Jah. PowerShelli ja käsurea tööriistu saate kasutada mitmesuguseid platvormide jaoks loodud. Lisateavet leiate [siit](http://go.microsoft.com/fwlink/?LinkId=317721).
