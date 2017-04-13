<properties
   pageTitle="Marsruutimise nõuded ExpressRoute | Microsoft Azure'i"
   description="Sellelt lehelt leiate üksikasjalikke nõudeid, konfigureerimise ja haldamise marsruudi ExpressRoute topoloogia."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>ExpressRoute marsruutimise nõuded  

Kasutades ExpressRoute Microsofti pilveteenustega ühendust luua, peate häälestamine ja haldamine marsruutimist. Mõned ühenduvuse pakuvad häälestamise ja haldamise hallatav teenus marsruutimist. Kontrollige, et näha, kui nad pakuvad selle teenuse pakkuja Ühenduvus. Kui pole, peavad vastama järgmistele nõuetele. 

Lugege artiklit [ahelatega ja marsruutimise domeenide](expressroute-circuit-peerings.md) marsruutimise seansid, mis luuakse ühenduvuse hõlbustamiseks on vaja kirjeldust.

>[AZURE.NOTE] Microsoft ei toeta kõrge-saadavus konfiguratsioone marsruuteri koondamise protokollidega (nt HSRP, VRRP). Me toetuvad liigsete paari BGP seansside kohta silmitsemine jaoks kõrge kättesaadavus.

## <a name="ip-addresses-used-for-peerings"></a>Kasutatakse peerings IP-aadressid

Peate mõne kvartali IP-aadresside konfigureerida võrgu ja Microsofti Enterprise serva (MSEEs) ruuterid marsruutimine reserveerida. Selles jaotises loetletakse nõuded ja kirjeldatakse reeglite kohta, kuidas need IP-aadressid tuleb omandatud ja kasutada.

### <a name="ip-addresses-used-for-azure-private-peering"></a>Kasutatakse Azure privaatne silmitsemine IP-aadressid

Saate konfigureerida selle peerings privaatne IP-aadresside või avaliku IP-aadressid. Marsruudib konfigureerimiseks kasutatud aadress vahemiku peab ei kattu Azure virtuaalse võrgu loomiseks kasutatud aadresside vahemikud. 

 - Teil tuleb reserveerida on /29 alamvõrgu või kahe /30 alamvõrku marsruutimise liideste.
 - Alamvõrku, kasutatakse marsruutimine võib olla privaatne IP-aadresside või avaliku IP-aadressid.
 - Funktsiooni alamvõrku ei tohi kasutada Microsofti pilveteenuse kliendi poolt reserveeritud vahemiku vastuollu.
 - Kui soovitud /29 alamvõrgu kasutatakse, jagatakse kahe /30 alamvõrku. 
     - Esimese /30 alamvõrgu kasutatakse esmane link ja teine/30 alamvõrgu kasutatakse teisese lingi.
     - Iga soovitud /30 alamvõrku, kasutage funktsiooni /30 esimene IP-aadress oma marsruuteri alamvõrgu. Microsoft kasutab teine IP-aadress on /30 alamvõrgu BGP seansi häälestamiseks.
     - Seadistage peab [kättesaadavus SLA](https://azure.microsoft.com/support/legal/sla/) kehtivad nii BGP seansid.  

#### <a name="example-for-private-peering"></a>Privaatne silmitsemine näide

Kui valite a.b.c.d/29 abil saate häälestada selle silmitsemine, jagatakse kahe /30 alamvõrku. Alltoodud näites me vaatame a.b.c.d/29 alamvõrgu kasutamist. 

a.b.c.d/29 a.b.c.d/30 ja a.b.c.d+4/30 tükeldamine ja Microsoft ettevalmistamise API-de kaudu edasi. Kasutate a.b.c.d+1 VRF IP esmane PE ja Microsoft tarbib a.b.c.d+2 nagu VRF IP esmane MSEE. Kasutate a.b.c.d+5 VRF IP teisene PE ja kasutab Microsoft a.b.c.d+6 VRF IP teisene MSEE.

Kaaluge võimalust juhul, kui valite 192.168.100.128/29 privaatne silmitsemine häälestamiseks. 192.168.100.128/29 sisaldab 192.168.100.135, mille seas kaudu 192.168.100.128-aadressid.

- 192.168.100.128/30 määratakse link1, pakkujaga 192.168.100.129 ja Microsoft abil 192.168.100.130 abil.
- 192.168.100.132/30 määratakse link2, pakkujaga 192.168.100.133 ja Microsoft abil 192.168.100.134 abil.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP-aadressid ja silmitsemine Microsoft Azure'i kohtades kasutatakse

Kasutage avaliku IP-aadressid, mille omanik te olete häälestamise BGP seansid. Microsoft peate saama omanikuõiguse IP-aadresside marsruutimine Internet registrite ja Internet marsruutimine registrite kaudu. 

- Kasutage kordumatu/29 alamvõrgu või kahe /30 alamvõrku BGP, silmitsemine iga silmitsemine kohta ExpressRoute jaoks häälestada circuit (kui teil on rohkem kui üks). 
- Kui soovitud /29 alamvõrgu kasutatakse, jagatakse kahe /30 alamvõrku. 
    - Esimese /30 alamvõrgu kasutatakse esmane link ja teine/30 alamvõrgu kasutatakse teisese lingi.
    - Iga soovitud /30 alamvõrku, kasutage funktsiooni /30 esimene IP-aadress alamvõrgu marsruuteri kohta. Microsoft kasutab teine IP-aadress on /30 alamvõrgu BGP seansi häälestamiseks.
    - Seadistage peab [kättesaadavus SLA](https://azure.microsoft.com/support/legal/sla/) kehtivad nii BGP seansid.

## <a name="public-ip-address-requirement"></a>Avaliku IP-aadressi nõue 

### <a name="private-peering"></a>Privaatne silmitsemine 

Saate kasutada avalik või privaatne IPv4 aadressid privaatne silmitsemine. Pakume lõpuni liikluse eraldamise nii kattub teiste klientide aadressid ei ole võimalik privaatne silmitsemine. Need aadressid pole on avaldatud Interneti. 

### <a name="public-peering"></a>Avaliku silmitsemine

Azure'i avaliku silmitsemine tee võimaldab ühenduse kõigi teenuste majutatud Azure üle oma avaliku IP-aadressid. Nendeks on loetletud [ExpessRoute KKK](expressroute-faqs.md) ja teenuste majutatud tarkvaratarnijad Microsoft Azure. Microsoft Azure'i teenust avaliku silmitsemine Ühenduvus alati algatatud võrgu kaudu Microsoft võrku. Kasutage jaoks mõeldud Microsoft network liiklus avaliku IP-aadressid.

### <a name="microsoft-peering"></a>Microsoft silmitsemine

Microsoft silmitsemine tee saate ühendada Microsofti pilveteenustega, mis pole toetatud Azure avaliku silmitsemine tee kaudu. Teenuste loend sisaldab Office 365 teenuseid, näiteks Exchange Online'i, SharePoint Online, Skype'i ärirakenduse ja CRM Online'i. Microsoft toetab Microsoft silmitsemine kahesuunalise Ühenduvus. Microsofti pilveteenustega sinna liikluse peate kasutama kehtiv avaliku IPv4 aadressid enne Microsoft network.

Veenduge, et IP-aadress ja kui arv on juba registreeritud teile ühes registrid, mis on loetletud allpool.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Microsoft on piisavalt pika aja ExpressRoute avaliku IP-aadressid pole tuleb reklaamida Interneti-ühendus. See võib murda muude Microsofti teenuste Ühenduvus. Siiski võib üle ExpressRoute reklaamida avaliku IP-aadressid, mida kasutavad serverid võrgu, mis suhelda jooksul Microsoft O365 lõpp-punktid. 

## <a name="dynamic-route-exchange"></a>Dünaamiline marsruutimine exchange

Marsruutimise Exchange'i saab eBGP-protokolli. Funktsiooni MSEEs ja oma ruuterid vahel on loodud EBGP seansid. Autentimise BGP seanssi ei ole nõue. Vajaduse korral on MD5 räsi saab konfigureerida. Kohta leiate teavet [konfigureerimine marsruutimist](expressroute-howto-routing-classic.md) ja [ringi ettevalmistamise ringi olekud ja töövoogude](expressroute-workflows.md) konfigureerimise BGP seansid.

## <a name="autonomous-system-numbers"></a>Autonoomse süsteemi arvud

AS 12076 kasuta Microsoft Azure avaldada, Azure'i privaatne ja Microsoft silmitsemine. Meil on reserveeritud ASNs 65515 65520 kasutamiseks. 16 ja 32 bit nagu arvud on toetatud.

Puuduvad nõuded andmete edastamine sümmeetria ümber. Edasi- ja võrguaadressid võib läbida erinevate ruuteri paari. Ühesuguste marsruudib tuleb reklaamida üle mitme ringi paari kuuluva saate mõlemal poole. Marsruutimiseks mõõdikute ei pea olema sama.

## <a name="route-aggregation-and-prefix-limits"></a>Marsruutimiseks liitmise ja eesliite piirangud

Me ei toeta kuni 4000 eesliiteid reklaamida meile Azure privaatne silmitsemine kaudu. See võib suurendada kuni 10 000 eesliidete tähised, kui ExpressRoute premium lisandmoodul on lubatud. Võtame kuni 200 eesliiteid BGP seansi Azure avaliku ja Microsoft silmitsemine. 

BGP seansi kõrvaldatakse, kui eesliiteid arv ületab. Võtame vaikimisi marsruudib privaatne silmitsemine link ainult. Pakkuja peab filtreerida vaikimisi marsruudi ja privaatne IP-aadressid (RFC 1918) avaliku Azure ja Microsoft silmitsemine teed. 

## <a name="transit-routing-and-cross-region-routing"></a>Teel marsruutimine ja rist-piirkond marsruutimine

ExpressRoute ei saa konfigureerida teel ruuterid. On teil toetuvad teel marsruutimise teenuste pakkuja Ühenduvus.

## <a name="advertising-default-routes"></a>Reklaami vaikimisi marsruudib

Vaikimisi marsruudib on lubatud ainult Azure privaatne silmitsemine seansid. Sellisel juhul me tee kogu liikluse võrgu seotud virtuaalse võrgu kaudu. Vaikimisi marsruudib reklaami üheks privaatne silmitsemine tulemuseks Interneti kaudu Azure'i blokeeritud tee. Peate kasutama peab teie ettevõtte serva abil saate marsruutida liiklust kaudu ja Azure teenuseid Interneti-ühendus. 

 Ühenduvus muude Azure ja taristu teenused lubamiseks peate veenduma, üks järgmistest põhjustest on kohas:

 - Azure'i avaliku silmitsemine on lubatud liikluse marsruutimiseks avaliku lõpp-punktid
 - Saate kasutada kasutaja määratletud marsruutimine luba Interneti-ühendus iga alamvõrku, mis nõuab Interneti-ühendus.
 
>[AZURE.NOTE] Vaikimisi marsruudib reklaami katkestamine Windowsi ja muude VM litsentsi aktiveerimine. Järgige juhiseid [siin](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) selle lahendamiseks.

## <a name="support-for-bgp-communities-preview"></a>Tugi BGP ühenduste (eelvaade)


Selles jaotises antakse ülevaade sellest, kuidas kasutada BGP ühenduste ExpressRoute abil. Microsoft on reklaamida marsruudib avaliku ja Microsoft silmitsemine teed protsesside sildistatud vastav ühenduse väärtustega. Tehes põhjuseid ja ühenduse väärtuste üksikasju on kirjeldatud allpool. Microsoft, aga ei au sildistatud lennuliinide reklaamida Microsoft ühenduse väärtused.

Kui loote Microsoft ExpressRoute mis tahes kohas, ühe silmitsemine geopoliitiliste piirkonna kaudu, on teil juurdepääs kogu Microsofti pilveteenustega kõigis piirkondades sees geopoliitiliste äärist. 

Näiteks kui ühendatud Microsoft Amsterdamis ExpressRoute kaudu, on teil juurdepääs kogu Microsofti pilveteenustega majutatud Põhja- ja Lääne Euroopa. 

Vaadake [ExpressRoute partnerid ja silmitsemine asukohad](expressroute-locations.md) lehel üksikasjalik loend geopoliitiliste regioonid, seotud Azure piirkondade ja vastavate ExpressRoute silmitsemine asukohad.

Saate osta rohkem kui ühe ExpressRoute ringi geopoliitiliste piirkonna kohta. Kas teil on mitu ühendust pakub kõrge kättesaadavuse geo-koondamise tõttu märkimisväärsed eelised. Juhul, kui teil on mitu ExpressRoute elektriskeemide, saate sama valiku eesliiteid reklaamida avaliku silmitsemine sisse Microsofti ja Microsoft silmitsemine teed. See tähendab, et teil on mitu teed sisse Microsofti võrgu kaudu. See võib põhjustada potentsiaalselt optimaalne marsruutimise oma võrgustikus teha otsuseid. Seetõttu võib ilmneda optimaalne ühenduvuse kogemusi erinevad teenused. 

Microsoft on sildistada reklaamida kaudu avaliku silmitsemine eesliiteid ja Microsoft koos BGP ühenduse väärtused, mis näitab, piirkonna silmitsemine eesliiteid majutatakse. Saate kasutada pakkuda [optimaalse marsruutimine klientidele](expressroute-optimize-routing.md)vastav marsruutimise otsuste ühenduse väärtusi.

| **Geopoliitiline piirkond** | **Microsoft Azure'i piirkond** | **BGP ühenduse väärtus** |
|---|---|---|
| **Põhja-Ameerika** |    |  |
|    | Ida-USA | 12076:51004 |
|    | Ida-USA 2 | 12076:51005 |
|    | Lääne USA. | 12076:51006 |
|    | Lääne USA 2 | 12076:51026 |
|    | Lääne Kesk-USA | 12076:51027 |
|    | Põhja Kesk-USA | 12076:51007 |
|    | Lõuna-, Kesk-USA | 12076:51008 |
|    | Kesk-USA | 12076:51009 |
|    | Kanada Central | 12076:51020 |
|    | Kanada Ida | 12076:51021 |
| **Lõuna-Ameerika** |  |  |
|    | Brasiilia Lõuna | 12076:51014 |
| **Euroopa** |    |  |
|    | Põhja-Euroopa | 12076:51003 |
|    | Lääne Euroopa | 12076:51002 |
| **Aasia Vaikse ookeani piirkonna** |    |   |
|    | Ida-Aasia | 12076:51010 |
|    | Kagu-Aasia | 12076:51011 |
| **Jaapan** |     |   |
|    | Jaapan Ida | 12076:51012 |
|    | Jaapan Lääne | 12076:51013 |
| **Austraalia** |    |   | 
|    | Austraalia Ida | 12076:51015 |
|    | Austraalia kodutee | 12076:51016 |
| **India** |    |   |
|    | India, Lõuna | 12076:51019 |
|    | India Lääne | 12076:51018 |
|    | India Central | 12076:51017 |

Kõik marsruudib Microsofti reklaamida on sildistatud vastav ühenduse väärtusega. 

>[AZURE.IMPORTANT] Globaalne eesliidete tähised on sildistatud vastav ühenduse väärtusega ja avaldatakse ainult siis, kui ExpressRoute premium lisandmoodul on lubatud.


Lisaks Microsoft ka sildistamine, eesliiteid nad kuuluvad teenuse põhjal. See kehtib ainult Microsoft silmitsemine. Järgmises tabelis toodud vastenduse teenuse BGP ühenduse väärtus.

| **Teenus** | **BGP ühenduse väärtus** |
|---|---|
| **Exchange'i** | 12076:5010 |
| **SharePointi** | 12076:5020 |
| **Skype'i ärirakenduses** | 12076:5030 |
| **CRM Online'i** | 12076:5040 |
| **Muud Office 365 teenused** | 12076:5100 |

>[AZURE.NOTE] Microsoft au määratud marsruudib, Microsoft reklaamida BGP ühenduse väärtused.

## <a name="next-steps"></a>Järgmised sammud

- Konfigureerige ExpressRoute ühendust.

    - [Loo klassikaline juurutamise mudeli mõne ExpressRoute ringi](expressroute-howto-circuit-classic.md) või [loomine ja ExpressRoute ringi, mis Azure'i ressursihaldur abil muuta](expressroute-howto-circuit-arm.md)
    - [Klassikaline juurutamise mudeli marsruutimine konfigureerimine](expressroute-howto-routing-classic.md) või [ressursihaldur juurutamise mudeli marsruutimine konfigureerimine](expressroute-howto-routing-arm.md)
    - [Klassikaline VNet, et mõne ExpressRoute ringi](expressroute-howto-linkvnet-classic.md) või [ressursihaldur VNet, et mõne ExpressRoute ringi Link](expressroute-howto-linkvnet-arm.md)


