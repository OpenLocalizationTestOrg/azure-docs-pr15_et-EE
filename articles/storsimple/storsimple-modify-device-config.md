<properties 
   pageTitle="StorSimple seadme konfiguratsiooni muuta | Microsoft Azure'i" 
   description="Kirjeldab, kuidas konfigureerida StorSimple seade, mis on juba juurutanud StorSimple halduri teenuse abil." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>StorSimple halduri teenuse abil saate muuta oma StorSimple seadme konfigureerimine

## <a name="overview"></a>Ülevaade 

Azure'i klassikaline portaali lehe **konfigureerimine** sisaldab kõiki seadme parameetreid saate konfigureerida, StorSimple seadmes, mida haldab StorSimple halduri teenuse. Selles õpetuses selgitatakse, kuidas saate kasutada lehe **konfigureerimine** järgmised seadme taseme ülesannete täitmiseks:

- Seadme sätete muutmine 
- Kellaaja sätete muutmine 
- DNS-i sätete muutmine 
- Võrgu liidesed muutmine
- Vaheta või määrata IP-d

## <a name="modify-device-settings"></a>Seadme sätete muutmine

Seadme sätted sisaldavad sõbralik nimi seadme ja seadme kirjeldus.

StorSimple seade, mis on ühendatud StorSimple halduri teenus on määratud vaikenime. Vaikenime peegeldab tavaliselt seadme järjenumbri. Näiteks vaikimisi seadme nimi, mis on 15 märki, nt 8600 SHX0991003G44HT, näitab järgmist:

- **8600** – näitab, et seadme mudelist.
- **SHX** – näitab tootva.
- **0991003** – näitab, et mõne kindla toote.
- **G44HT**- viimased 5 numbrit on kordumatu järjenumbreid loomiseks muudetakse. See ei pruugi olla järjestikku kogum.

Azure'i klassikaline portaali abil saate muuta seadme nimi ja selle määramine kordumatu sõbralik nimi oma valik. Sõbralik nimi võib sisaldada mis tahes märgid ja võib olla kuni 64 märgi pikkused.

Saate määrata ka seadme kirjeldus. Tavaliselt aitab seadme kirjeldus, omanik ja seadme füüsilise asukoha tuvastamine. Väljale Kirjeldus peab sisaldama vähem kui 256 märki.
 
## <a name="modify-time-settings"></a>Kellaaja sätete muutmine

Teie seade peab sünkroonimiseks selleks, et pilveteenuses salvestusruumi teenusepakkuja juures autentida. Ajavööndi rippmenüü loendist ja määrake kuni kaks korda Protocol (NTP) serverites. Esmane NTP server on vaja ja on määratud, kui kasutate Windows PowerShelli StorSimple seadme konfigureerida. Saate määrata vaikimisi Windows Server **time.windows.com** NTP serveri. Esmane NTP serveri konfiguratsiooni Azure klassikaline portaali kaudu saate vaadata, kuid kasutajaliidese Windows PowerShelli abil peate seda muuta.

Teisene NTP serveri konfiguratsioon pole kohustuslik. Klassikaline portaali abil saate konfigureerida teisene NTP server. 

NTP serveri konfigureerimisel veenduge, et võrgu võimaldab NTP liiklust Interneti kaudu oma andmekeskuse edastamiseks. Avalik NTP server määramisel peate tegema veenduge, et teie tulemüürid ja muid seadmeid konfigureeritud liikuda ja sealt väljaspool võrgu NTP liikluse lubamiseks. Kui kahesuunaline NTP liiklust ei ole lubatud, peate kasutama ettevõtte NTP serverisse (Windowsi domeenikontrolleri pakub see funktsioon). Kui seadme aega ei saa sünkroonida, ei pruugi see pakkuja pilveteenuse salvestusruumi suhelda.

Avaliku NTP serverite loendi vaatamiseks avage [NTP serverid Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Mis juhtub, kui seade on juurutatud erinevate ajavööndi?

Kui seade on juurutatud erinevate ajavöönd, muutub seadme ajavöönd. Varukoopia poliitikaid kasutada seadme ajavöönd, et reguleerida varukoopia poliitikate automaatselt vastavalt uue ajavööndi. Pole kasutaja sekkumine on nõutav.

## <a name="modify-dns-settings"></a>DNS-i sätete muutmine

DNS-i server kasutatakse, kui teie seade proovib suhelda pilve salvestusruumi teenusepakkuja. Kõrge olemasolu, on vaja konfigureerida esmase ja teisese DNS-serverid seadme algset juurutamise käigus. Konfigureerida esmane DNS-i server, peate kasutajaliidese Windows PowerShelli kasutamine StorSimple seadmes.

Teisene DNS server muutmiseks saate kasutada Azure klassikaline portaali.



## <a name="modify-network-interfaces"></a>Võrgu liidesed muutmine

Teie seade on kuus seadme võrgu liidesed, millest neli on 1 New York ja kaks, mis on 10 New York. Liideste märgistatud andmete 0 – andmed 5. ANDMETE 0, andmed 1, andmete 4 ja 5 andmed on 1 New York, andmete 2 ja 3 andmed on 10 New York võrgu liidesed.

**Võrgu kasutajaliidese sätete** konfigureerimine iga kasutatava liideste. Kõrge kättesaadavuse tagamiseks soovitame teie seadmes on vähemalt kaks iSCSI liidesed ja kahe pilve lubatud liideste. Soovitame, kuid ei nõua, et kasutamata liidesed keelatakse.

Kui konfigureerite mis tahes võrgu liidesed, tuleb konfigureerida virtuaalse IP (VIP).

ANDMETE 0 on vaikimisi cloud lubatud. ANDMETE 0 konfigureerimisel peavad ka kahe fikseeritud IP-aadressid, üks iga domeenikontrolleri konfigureerimine. Need fikseeritud IP-aadressid saab kasutada seadme kontrollerid otse juurde pääseda ja on kasulik, kui värskenduste installimine seadme või kui pöördute kontrollerid eesmärgil tõrkeotsing.

StorSimple 8000 seeria Update 1 marsruutimise mõõdiku andmete 0 on seatud väikseimani; Seetõttu, kui teie seade töötab StorSimple 8000 seeria Update 1, suunatakse kõik cloud liikluse läbi andmete 0. Märkige see, kui teil on rohkem kui üks cloud lubatud võrgu liidese StorSimple seadmes.

>[AZURE.NOTE] Fikseeritud IP-aadresside selle domeenikontrolleri kasutatakse teenindamine seadmele värskendusi. Seetõttu fikseeritud IP-d tuleb marsruuditavaid ja võimalus Interneti-ühenduse.

Iga võrgu liidese, kuvatakse järgmised parameetrid:

- **Kiiruse** – pole kasutaja konfigureeritav parameeter. ANDMETE 0, andmed 1, andmete 4 ja 5 andmed on alati 1 New York, andmete 2 ja 3 andmed on 10 New York liidesed.

     >[AZURE.NOTE] Kiiruse ja dupleks on alati automaatselt üle. Jumbo raame ei toetata.
 
- **Kasutajaliidese olek** – liidest saate lubada või keelata. Kui lubatud, proovib seade kasutajaliides. Soovitame lubada ainult liidesed, mis on ühendatud võrku ja kasutada. Keela kõik liidesed, mida te ei kasuta.

- **Liidese tüüp** – see parameeter, mis võimaldab teil eristamiseks iSCSI liiklust pilveteenuse salvestusruumi liiklust. See parameeter võib olla üks järgmistest:

    - **Pilveteenuse lubatud** – kui lubatud, kasutatakse seadme selle kasutajaliidese pilveteenuses suhelda.
    - **iSCSI lubatud** – kui lubatud, seade on selle kasutajaliidese abil suhelda iSCSI host.

    Soovitame, et te eristada iSCSI liikluse pilveteenuse salvestusruumi liikluse aadressilt. Samuti võtke arvesse, kui teie host on sama alamvõrgu oma seade, pole vaja määrata lüüsi; Kui teie host on erinevate alamvõrku, kui teie seade, peate lüüsi määramine.

- **IP-aadress** – see võib olla IPv4 või IPv6 või mõlemad. Seadme võrgu liidesed nii IPv4 ja IPv6 aadress peredele on toetatud. Kui kasutate IPv4, määrata dot-koma märke 32-bitise IP-aadress (*xxx.xxx.xxx.xxx*). IPv6 kasutamisel lihtsalt sisestama 4-kohaline eesliite ja 128-bitine aadress luuakse automaatselt seadme võrgu kasutajaliidese selle eesliite põhjal.

- **Alamvõrgu** – see viitab alamvõrgu maski ja on konfigureeritud Windows PowerShelli kasutajaliidese kaudu.

- **Lüüsi** – see on vaikimisi lüüsi, mis tuleks kasutada seda kasutajaliidese, kui see proovib suhelda sõlmed, mis ei kuulu sama IP-aadress ruumi (alamvõrgu). Vaikimisi lüüsi peab olema sama aadressiruumi (alamvõrgu) liides IP-aadress määratud alamvõrgu mask.

- **Fikseeritud IP-aadress** – see väli on saadaval ainult siis, kui saate konfigureerida andmete 0 kasutajaliidese. Toiminguid nagu värskendusi või seadme tõrkeotsing, peate ühenduse seadme kontrolleril. Fikseeritud IP-aadressi saab kasutada nii aktiivse ja passiivne domeenikontrolleri seadme juurde pääseda.

Klassikaline Azure portaali kaudu saate konfigureerida kontrolleril 0 ja 1 kontrolleril.

>[AZURE.NOTE] 
>
>- Õige töö tagamiseks kontrollige kasutajaliidese kiiremaks ja dupleks ühendatud iga seadme kasutajaliidese vahetamise kohta. Vaheta liideste tuleks teha ja rääkida või olla konfigureeritud Gigabit Ethernet (1000 MB) ja täis-dupleks. Opsüsteem aeglasemalt või pool-dupleks liideste tulemuseks jõudlusprobleeme.
>
>- Häireid ja tööseisakute minimeerimiseks soovitame teil lubada portfast iga iSCSI võrgu liidese seadme ühenduse vahetamise pordid. See tagab, et Tõrkesiirde korral on võimalik kiiresti luua võrguühendus.
 
## <a name="swap-or-reassign-ips"></a>Vaheta või määrata IP-d

Praegu, kui mis tahes võrk kasutajaliidese kontrolleril on määratud VIP, mis kasutab (kas samasse seadmesse või mõnda muud seadet võrgus), siis selle domeenikontrolleri ei õnnestu üle. Seetõttu tuleb kasutage proper toimingut, kui teil on vahetamine VIP seadme, võrgu kasutajaliidese, kuna loote dubleeritud IP olukord.

Järgmiste toimingute Vaheta või ümbermääramiseks VIP mis tahes võrgu liidesed:

#### <a name="to-reassign-ips"></a>Määrata IP-d

1. Tühjendage nii liideste IP-aadress.

2. Kui IP-aadressid on kustutatud, määrata vastavate liideste uue IP-aadressid.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [konfigureerida MPIO StorSimple seadme](storsimple-configure-mpio-windows-server.md).

- Siit saate teada, [StorSimple halduri teenuse haldamiseks StorSimple seadme kasutamise](storsimple-manager-service-administration.md)kohta.
     
