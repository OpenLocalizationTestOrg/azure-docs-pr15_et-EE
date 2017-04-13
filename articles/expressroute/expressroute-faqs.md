<properties
   pageTitle="ExpressRoute KKK"
   description="KKK ExpressRoute sisaldab teavet toetatud Azure'i teenuste maksumus andmete ja ühenduste, SLA, pakkujate ja asukohad, läbilaskevõime ja täiendavad tehnilised üksikasjad."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="expressroute-faq"></a>ExpressRoute KKK


## <a name="what-is-expressroute"></a>Mis on ExpressRoute?
ExpressRoute on Azure'i teenus, mis võimaldab teil luua privaatne seoseid Microsofti andmekeskuses ja taristu, mis on oma ettevõttes või colocation poole. ExpressRoute ühendused avage avaliku Interneti kaudu ja pakkuda suurem turvalisus, töökindluse ja kiiruse ja väiksem kui tüüpilised ühendused latentsused Interneti kaudu.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections"></a>Mis on ExpressRoute ja privaatvõrgu ühenduse kasutamise eelised?
ExpressRoute ühendused avage avaliku Interneti kaudu ja pakkuda suurem turvalisus, töökindluse ja kiiruse koos alumise ja terviklik latentsused kui tüüpilised ühendused Interneti kaudu. Mõnel juhul ExpressRoute ühenduste abil andmete vahel kohapealsete seadmed ja Azure saate kulude kasulik.

### <a name="what-microsoft-cloud-services-are-supported-over-expressroute"></a>Millist Microsofti pilveteenustega toetavad üle ExpressRoute?
ExpressRoute toetab enamiku Microsoft Azure'i teenuste täna, sh Office 365.  Vaadake värskendusi üldiselt kättesaadav varsti.

### <a name="where-is-the-service-available"></a>Kus asub teenust?
Asukoht ja kättesaadavuseks sellelt lehelt leiate: [ExpressRoute partnerid ja asukohad](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-dont-have-partnerships-with-one-of-the-expressroute-carrier-partners"></a>Kuidas kasutada ExpressRoute ühenduse loomiseks Microsoft, kui mul pole partnerlus ühe ExpressRoute-carrier partneri?
Saate valida piirkondliku vedaja ja maa Ethernet ühendused ühele toetatud exchange pakkuja asukohad. Saate vaadata siis läbi Microsofti pakkuja asukohas. Märkige ruut [ExpressRoute partnerid ja asukohtade](expressroute-locations.md) kui teenusepakkuja on olemas, mis tahes kohast Exchange'i osa. Seejärel saate tellida mõni ExpressRoute ringi teenusepakkuja Azure'i ühenduse kaudu.

### <a name="how-much-does-expressroute-cost"></a>Kui palju maksab ExpressRoute hind?
Märkige ruut [hinnad üksikasjad](https://azure.microsoft.com/pricing/details/expressroute/) hindade teavet.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed"></a>Kui maksate on antud läbilaskevõime ExpressRoute ringi, kas minu võrgu teenuse pakkujalt osta VPN-ühendus peab olema sama kiirust?
Ei. Saate osta VPN-ühenduse kiirusest, mis tahes teenusepakkujalt. Azure'i teie ühendus on ExpressRoute ringi läbilaskevõime, mida osta piirdub.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-required"></a>Kui maksate on antud läbilaskevõime ExpressRoute ringi, kas võimalus lõhkeda kuni kiirema vajadusel?
Jah. ExpressRoute topoloogia on konfigureeritud toetama juhul, kui saate lõhkeda kuni kaks korda ribalaius, saate täiendavaid tasuta hangitud. Märkige teenusepakkuja juures, kui nad ei toeta seda funktsiooni.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Saab kasutada sama privaatvõrgu ühendust virtuaalse võrgu ja muude Azure'i teenuste korraga?
Jah. Mõne ExpressRoute ringi üks kord häälestus võimaldab teil juurde pääseda virtuaalse võrgustikus ja muude Azure'i teenuste korraga. Loote virtuaalne võrkude üle privaatne silmitsemine tee ja muude teenuste üle avaliku silmitsemine tee.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>Kas ExpressRoute pakub teenindustaseme leping (SLA)?
Lugege lisateavet [ExpressRoute SLA lehe](https://azure.microsoft.com/support/legal/sla/) .

## <a name="supported-services"></a>Toetatud teenused
Kõige Azure teenused on toetatud ExpressRoute üle.

- Virtuaalmasinates ja pilveteenustega juurutatud, virtuaalse võrgu Ühenduvus on toetatud üle privaatne silmitsemine tee.
- Azure'i veebisaitide on toetatud üle avaliku silmitsemine tee.
- Asjade jaoturi toetatakse üle avaliku silmitsemine tee.
- Office 365 on toetatud Microsoft silmitsemine tee üle.
- Kõik muud teenused on kättesaadavad üle avaliku silmitsemine tee. Erandiks on järgmine.

    **Ei toetata järgmisi teenuseid.**

    - CDN-ID
    - Visual Studio meeskonnatöö teenuste laadi testimine
    - Mitmikautentimise
    - Liikluse haldur

## <a name="data-and-connections"></a>Andmete ja ühenduste

### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute"></a>Kas on piirangud, mida saab üle kanda ExpressRoute abil?
Me ei määra summa andmeedastus on piiratud. Vaadake [hinnad üksikasjad](https://azure.microsoft.com/pricing/details/expressroute/) läbilaskevõime kohta lisateavet.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Millist ühenduse korral toetavad ExpressRoute?
Toetatud läbilaskevõime pakkumised:

| 50 Mbps, 100 Mbps, 200 Mbps 500 Mbps 1Gbps, 2 Gbit, 5 Gbit 10Gbps |

### <a name="which-service-providers-are-available"></a>Millised teenusepakkujad on saadaval?
Vaadake teemat [ExpressRoute partnerid ja asukohtade](expressroute-locations.md) teenusepakkujatele ja asukohtade loendit.

## <a name="technical-details"></a>Tehnilised andmed

### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure"></a>Mis on minu kohapealse asukoht ühenduse Azure'i tehnilise?
Lugege [ExpressRoute eeltingimused lehe](expressroute-prerequisites.md) nõuetele.

### <a name="are-connections-to-expressroute-redundant"></a>On ühendused ExpressRoute liigsed?
Jah. Iga teekonna ringi on üleliigsed paar rist ühendused, mis on konfigureeritud pakkuda kõrge kättesaadavus.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Kaotsi ühenduvuse kui üks minu ExpressRoute lingid ei?
Te ei kaota ühenduvuse kui ühe rist ühenduse nurjub. Liigsete võrguühenduse toetamiseks võrgu laadi. Lisaks saate luua mitme elektriskeemide tõrge paindlikkuse saavutamiseks silmitsemine asukoht on muutunud.

### <a name="onep2plink"></a>Kui ma ei ole koostööd asub veebisaidil pilvepõhise Exchange'i ja minu teenusepakkuja pakub kakspunkt ühendus, pean tellimuse kahe füüsilise seoseid oma kohapealse võrgu ja Microsoft? 
Ei, tuleb ainult üks füüsiline ühendus kui teenusepakkuja saate luua kaks Ethernet virtuaalse ahelatega füüsilise ühenduse kaudu. Füüsilise ühendus (nt kiudoptilised) on lõpetatud kiht 1 (L1) seade (vt pilti allpool). Kaks Ethernet virtuaalse topoloogia on kodeeritud erinevate VLAN ID, mille üks ja üks teisese. Nende VLAN ID-d on väline 802.1Q Ethernet päis. Sisemine 802.1Q kindla [ExpressRoute marsruutimise Domeen](expressroute-circuit-peerings.md)on vastendatud Ethernet päise (ei kuvata). 

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)


### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute"></a>Saate pikendada üks minu VLANs Azure ExpressRoute abil?
Ei. Me ei toeta layer 2 Ühenduvus laiendid Azure'i sisse.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Kas ma saan mitu ExpressRoute ringi oma tellimust?
Jah. Teie tellimus võib olla mitu ExpressRoute ringi. Sihtotstarbeline topoloogia vaikimisi piirarvu on seatud 10. Võite pöörduda Microsoft Support suurendamiseks limiit vajaduse korral.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Kas ma saan ExpressRoute topoloogia mitme teenusepakkuja?
Jah. Saate määrata ExpressRoute topoloogia paljud teenuse pakkujad. Iga ExpressRoute ringi seostatakse ainult ühe teenusepakkuja.

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>Kuidas ühendada mõne ExpressRoute ringi minu virtuaalse võrkudes
Põhitoimingud on kirjeldatud allpool.

- Peate luua mõne ExpressRoute ringi ja on see teenusepakkuja.
- Teie või pakkuja tuleb konfigureerida BGP silmitsemine (s).
- Ringi ExpressRoute peab virtuaalse võrgu linkida.

Lisateabe saamiseks vaadake [ExpressRoute töövoogusid ringi ettevalmistamise ja ringi olekus](expressroute-workflows.md) .

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>Kas minu ExpressRoute ringi ühenduvuse piirid?
Jah. [ExpressRoute partnerid ja asukohtade](expressroute-locations.md) lehel antakse ülevaade piirmäärad ühenduvuse jaoks soovitud ExpressRoute ringi. Mõne ExpressRoute ringi Ühenduvus on piiratud ühe geopoliitiline piirkond. Ühenduvus saate laiendada cross geopoliitiliste piirkondade ExpressRoute premium funktsiooni.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit"></a>Saab linkida mitme virtuaalse võrgus on ExpressRoute ringi?
Jah. Kuni 10 virtuaalse võrgu saate linkida mõne ExpressRoute ringi.

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit"></a>Mul on mitu Azure tellimust, virtuaalse võrgu sisaldavate. Saate ühendada virtuaalse võrgu, mis on ühe ExpressRoute ringi eraldi tellimuste korral?
Jah. Saate autoriseerida kuni 10 muude Azure'i tellimused kasutada ühe ExpressRoute ringi. Selle piirmäära tõsta ExpressRoute premium funktsiooni.

Leiate lisateavet teemast [ühiskasutus on ExpressRoute ringi üle mitu tellimust](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other"></a>Virtuaalse võrguga ühendatud sama ringi üksteisest eraldatud?
Ei. Kõik virtuaalse võrgu seotud sama ExpressRoute ringi kuuluvad sama marsruutimise domeeni ja ei ole eraldatud marsruutimise seisukohalt. Kui teil on vaja marsruutimiseks eraldamise, peate eraldi ExpressRoute ringi loomiseks.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit"></a>Kas ma saan ühe virtuaalse võrku ühendatud rohkem kui üks ExpressRoute ringi?
Jah. Saate ühe virtuaalse võrgu kuni 4 ExpressRoute topoloogia linkida. Neid peab tellida kuni 4 eri [ExpressRoute asukohad](expressroute-locations.md).

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits"></a>Interneti-ühendus saate pääseda minu virtuaalse võrguga ühendatud ExpressRoute topoloogia?
Jah. Kui te pole on avaldatud vaikimisi marsruudib (0.0.0.0/0) või internet marsruutimiseks eesliidete BGP seansi kaudu, saab linkida mõne ExpressRoute ringi virtuaalse võrgust Interneti-ühenduse.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits"></a>Interneti-ühenduse, virtuaalse ExpressRoute topoloogia ühendatud võrke saate blokeerida?
Jah. Saab reklaamida vaikimisi marsruudib (0.0.0.0/0) blokeerida kõik Interneti-ühenduse juurutatud virtuaalse võrgustikus virtuaalmasinates ja marsruutimine kõik liiklus välja läbi ExpressRoute ringi. Pange tähele, et kui te reklaamida vaikimisi marsruudib, me sunnib liikluse üle avaliku silmitsemine (nt Azure salvestusruumi ja SQL-i DB) tagasi oma ettevõttes pakutavaid teenuseid. On teil konfigureerida oma ruuterid liikluse naasmiseks Azure kaudu avaliku silmitsemine tee või Interneti kaudu.

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other"></a>Saate lingitud sama ExpressRoute ringi virtuaalne võrkude üksteisega rääkida?
Jah. Juurutatud, virtuaalse võrguga ühendatud sama ExpressRoute ringi virtuaalmasinates saate omavahel suhelda.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>Saate kasutada saidilt ühenduvuse virtuaalne võrkude koos ExpressRoute?
Jah. ExpressRoute saate kõrvuti VPN saidilt.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-to-use-expressroute"></a>Saate teisaldada virtuaalse võrgu-saidilt / punkti saidi konfiguratsiooni ExpressRoute kasutada?
Jah. On teil ka ExpressRoute lüüsi sees virtuaalse võrgu loomine. Ilmneb väike tööseisakute, protsessiga seotud.

### <a name="what-do-i-need-to-connect-to-azure-storage-over-expressroute"></a>Mida on vaja ühenduse Azure'i salvestusruumi üle ExpressRoute?
Tuleb luua ka ExpressRoute ringi ja konfigureerida protsesside jaoks avaliku silmitsemine.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise"></a>Saab reklaamida lennuliinide arv piirangud on?
Jah. Võtame kuni 4000 marsruutimiseks eesliiteid jaoks privaatne silmitsemine ja 200 avaliku silmitsemine ja Microsoft silmitsemine. Saate suurendada seda kuni 10 000 lennuliinide privaatne silmitsemine kui ExpressRoute premium funktsiooni.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session"></a>Saab reklaamida üle BGP seansi IP-vahemike piirangud on?
Me ei aktsepteeri avaliku ja Microsoft silmitsemine BGP seanss privaatne eesliiteid (RFC1918).

### <a name="what-happens-if-i-exceed-the-bgp-limits"></a>Mis juhtub, kui mul on BGP piirab?
Kõrvaldatakse BGP seansid. Need lähtestatakse, kui eesliide count läheb alla.

### <a name="what-is-the-expressroute-bgp-hold-time-can-it-be-adjusted"></a>Mis on ExpressRoute BGP hoida aega? Saate seda kohandada?
Hoidke aeg on 180. Ülalhoidmise sõnumid saadetakse iga 60 sekundi järel. Need on fikseeritud sätted Microsoft küljel, mida ei saa muuta.

### <a name="after-i-advertise-the-default-route-00000-to-my-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-to-i-fix-this"></a>Pärast minu virtuaalse võrkudega reklaamida vaikimisi marsruudi (0.0.0.0/0), ei saa minu Azure VMs töötab Windowsi aktiveerida. Kui soovite seda parandada?
Azure'i tuvasta aktiveerimise taotluse aitab järgmist:

1. Luua oma ExpressRoute ringi silmitsemine avaliku.
2. DNS-i teha ja **kms.core.windows.net** IP-aadressi otsimine
3. Tehke kaks järgmist, et Key Management Service tuvastab, et aktiveerimise taotluse pärineb Azure ja au kutse.
    - Kohapealse võrgus marsruutida liikluse, mis on mõeldud IP-aadress (saadud etappi 2) naasmiseks Azure kaudu avaliku silmitsemine.
    - On NSP pakkuja juuksed-pin liiklust Azure'i avaliku silmitsemine kaudu.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Saate muuta ExpressRoute topoloogia läbilaskevõime?
Jah. Läbilaskevõime ExpressRoute topoloogia saate suurendada ilma, et. On teil tagada, et neid värskendada pidurdab sees oma võrgu läbilaskevõime suurendamiseks toetama ühenduvuse pakkujaga Järeltegevus. Saate siiski ei saa ExpressRoute topoloogia läbilaskevõime vähendada. Vajaduseta alumises ribalaiust tähendab paikneva allapoole ja puhkus ExpressRoute topoloogia.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Kuidas muuta ExpressRoute topoloogia läbilaskevõime?
Läbilaskevõime ExpressRoute topoloogia, värskendus spetsiaalne ringi API ja PowerShelli cmdlet-käsu abil saate värskendada.

## <a name="expressroute-premium"></a>ExpressRoute Premium

### <a name="what-is-expressroute-premium"></a>Mis on ExpressRoute premium?
ExpressRoute premium on saidikogumi funktsioonid, mis on loetletud allpool.

 - Marsruutimise tabeli piiratud 4000 protsessidest 10 000 lennuliinide privaatne silmitsemine suurendada.
 - Suurem arv VNets, mille saab ühendada ExpressRoute ringi (vaikimisi 10). Vt lisateavet allpool esitatud tabelist.
 - Globaalse ühenduvuse Microsoft core võrgu kaudu. Nüüd saab linkida mõne VNet geopoliitiliste piirkonna koos mõne muu piirkonna ExpressRoute ringi. **Näide:** VNet, mis on loodud Euroopa Lääne ExpressRoute ringi, mis loodud Edenvale abil saate linkida.
 - Office 365 teenuste ja CRM Online'i Ühenduvus.

### <a name="how-many-vnets-can-i-link-to-an-expressroute-circuit-if-i-enabled-expressroute-premium"></a>Kui palju VNets saab linkida mõne ExpressRoute ringi ExpressRoute premium lubamisel?
Järgmistes tabelites on Kuva ExpressRoute piirangud ja VNets arv ExpressRoute ringi kohta.


[AZURE.INCLUDE [expressroute-limits](../../includes/expressroute-limits.md)]


### <a name="how-do-i-enable-expressroute-premium"></a>Kuidas lubada ExpressRoute premium?
Kui see funktsioon on sisse lülitatud ja saab sulgeda, värskendades ringi olek saab lubada ExpressRoute lisafunktsioonidele. Te saate lubada ExpressRoute premium ringi loomise ajal või saate helistada update spetsiaalne ringi API / PowerShelli cmdleti ExpressRoute premium lubamiseks.

### <a name="how-do-i-disable-expressroute-premium"></a>Kuidas keelata ExpressRoute premium?
Saate keelata ExpressRoute premium spetsiaalne värskenduse helistades circuit API / PowerShelli cmdleti, veenduge, et teil on oma ühenduvuse mastaabitud peab vastama vaikimisi piirangud enne, kui keelate ExpressRoute premium. Me ei õnnestu taotluse keelata ExpressRoute premium, kui teie kasutamine skaala Lisaks vaikimisi piirangud.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set"></a>Saate saab valida kasutaja soovi premium funktsioonikomplekt funktsioone?
Ei. Te ei saa valida funktsioone, vajate. Kui lülitate ExpressRoute premium võimaldada kõik funktsioonid.

### <a name="how-much-does-expressroute-premium-cost"></a>Kui palju maksab ExpressRoute premium hind?
Vaadake hind [hinnad üksikasjad](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges"></a>Tehke ma ExpressRoute premium lisaks standard ExpressRoute kulude eest maksta?
Jah. ExpressRoute premium kõnetasud peal ExpressRoute ringi ja tasud, mis on vaja ühenduvust pakkuja.

## <a name="expressroute-and-office-365-services-and-crm-online"></a>ExpressRoute ja Office 365 teenuste CRM Online'i

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services-and-crm-online"></a>Kuidas luua ühenduse Office 365 teenuste ja CRM Online'i mõne ExpressRoute ringi?

1. Vaadake üle [ExpressRoute eeltingimused](expressroute-prerequisites.md) lehe veendumaks, et te ei vasta nõuetele.
2. Vaadake üle teenusepakkujatele ja asukohad [ExpressRoute partnerid ja asukohtade](expressroute-locations.md) tagada teie vajadustele ühenduvuse loendit.
3. Lugege läbi [Võrgu plaanimine ja jõudluse häälestamine Teenusekomplektis Office 365](http://aka.ms/tune/)leping teie nõuetele.
4. Täitke alltoodud häälestamine ühenduvuse [ExpressRoute töövoogude ringi ettevalmistamise ja ringi riikide](expressroute-workflows.md)töövoogudes toodud juhised.

>[AZURE.IMPORTANT] Veenduge, et teil on lubatud ExpressRoute premium lisandmooduli Ühenduvus Office 365 teenuste ja CRM Online'i konfigureerimisel.

### <a name="do-i-need-to-enable-azure-public-peering-to-connect-to-office-365-services-and-crm-online"></a>Vaja lubamiseks Azure avaliku silmitsemine ühenduse Office 365 teenuste ja CRM Online'i?
Ei, peate ainult Microsoft Peering lubamine. Azure'i AD-liikluse autentimise saadetakse Microsoft Peering kaudu. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services-and-crm-online"></a>Saate oma olemasolevad ExpressRoute topoloogia toetab Ühenduvus Office 365 teenuste ja CRM Online'i?
Jah. Teie olemasoleva ExpressRoute ringi saate konfigureeritud toetama Ühenduvus Office 365 teenustega. Veenduge, et teil on piisavalt võimet Office 365 teenustega ühendust ja veenduge, et teil on lubatud premium lisandmooduli. [Võrgu plaanimine ja jõudluse häälestamine Teenusekomplektis Office 365](http://aka.ms/tune/) aitab teil leping ühenduvust teie vajadustele. Vaadake [loomine ja muutke soovitud ExpressRoute ringi](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Millist Office 365 teenuste pääseb ExpressRoute-ühenduse kaudu?

Vaadake [Office 365 URL-id ja IP-aadresside vahemikud](http://aka.ms/o365endpoints) lehe värskendatud loendi toetatud üle ExpressRoute teenuste jaoks.

### <a name="how-much-does-expressroute-for-office-365-services-and-crm-online-cost"></a>Kui palju maksab ExpressRoute Office 365 teenuste ja CRM Online'i maksumus?
Office 365 teenuste ja CRM Online'i nõuab premium olema lubatud lisandmoodul. Kulude üksikasjad [hinnad üksikasjade lehe](https://azure.microsoft.com/pricing/details/expressroute/) ExpressRoute ette.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Millised piirkonnad on ExpressRoute Office 365 jaoks toetatud?
Vaadake loendi kohta lisateavet, partnerite ja kohad, kus on toetatud ExpressRoute [ExpressRoute partnerid ja asukohad](expressroute-locations.md) .

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization"></a>Pääseb juurde Office 365 Interneti kaudu isegi siis, kui ExpressRoute konfigureeritud oma asutuse jaoks?
Jah. Office 365 teenuse lõpp-punktid on kättesaadav Interneti kaudu, kuigi ExpressRoute on konfigureeritud võrgu jaoks. Kui olete kohas, kus on konfigureeritud ühenduse Office 365 teenuste ExpressRoute kaudu, siis ühendage ExpressRoute kaudu.

### <a name="can-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Dynamics AXI Online'i pääseb ExpressRoute-ühenduse kaudu?
Ei, ei toetata.
