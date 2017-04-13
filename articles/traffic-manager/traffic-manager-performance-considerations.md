<properties
    pageTitle="Jõudluse huvides Azure'i liikluse haldur | Microsoft Azure'i"
    description="Jõudluse liikluse haldur ja kuidas testida jõudlust veebisaidi liikluse haldur kasutamisel mõistmine"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="performance-considerations-for-traffic-manager"></a>Jõudluse huvides liikluse haldur

Selle lehe selgitatakse jõudluse huvides halduris liiklust. Võtke arvesse järgmist stsenaariumi.

Teil on veebisaidi eksemplari WestUS ja vihaobjekt piirkondades. Üks linnanimede ei suuda seisundikontrolli jaoks liikluse halduri juures. Rakenduse liikluse suunatakse terve piirkond. See Tõrkesiirde on oodata, kuid jõudlus saab nüüd reisil kauges alale liiklus latentsus põhjal probleem.

## <a name="how-traffic-manager-works"></a>Liikluse haldur tööpõhimõtted

Ainult jõudluse mõju liikluse haldur saab veebisaidil on algse DNS otsing. DNS-i taotluse liikluse haldur profiili nimi teostab Microsoft DNS root server, mis hostib trafficmanager.net tsooni. Liikluse haldur kuvab ja regulaarselt värskendab, on Microsofti juursertimisprogrammi nimeservereid liikluse haldur poliitika ja juures tulemuste põhjal. Isegi ajal algsete DNS-i otsing, pole DNS-i päringud saadetakse liikluse haldur.

Liikluse haldur koosneb mitmest osast: DNS-i nimi serverites, teenuse API, salvestusruumi layer ja jälgimise teenuse lõpp-punkti. Kui liikluse haldur teenuste osa nurjub, on ei mõjuta teie haldur liikluse profiiliga seotud DNS-i nimi. Microsofti DNS-serverid kirjeid ei muudeta. Siiski ei juhtu lõpp-punkti jälgimine ja DNS-i värskendamine. Seetõttu liikluse haldur ei saa värskendada DNS-i osutamiseks Tõrkesiirde saidi, kui teie peamine saidi läheb alla.

DNS-i nimelahendus on kiire ja tulemuste vahemällu. Algse DNS-i funktsiooni LOOKUP kiirus sõltub kliendi kasutab nimelahendus DNS-i serverid. Tavaliselt mõnda muud klienti saavad teostada DNS otsing ~ 50 ms jooksul. Otsingu tulemuste vahemällu kestel selle DNS-i aeg-to-live (TTL). Vaikimisi TTL liikluse haldur on 300 sekundi.

Liikluse vool ei kaudu liikluse haldur. Kui DNS-i otsing on lõpule jõudnud, on näiteks oma veebisaidi IP-aadress. Kliendi ühendub see aadress ja edastama kaudu liikluse haldur. Valige liikluse haldur poliitika on DNS-i jõudlus ei mõjuta. Jõudluse marsruutimine-meetod võivad negatiivselt rakenduse kogemus. Näiteks kui teie poliitika suunab liikluse Põhja-Ameerika näiteks majutatud Aasia, võib võrgu latentsuse need seansid JÕUDLUSPROBLEEM ei.

## <a name="measuring-traffic-manager-performance"></a>Liikluse haldur jõudluse mõõtmiseks

On mitu veebisaite abil saate mõista jõudlus ja liikluse haldur profiili toimimist. Paljud on tasuta, kuid võib-olla piirangud. Mõned saidid pakuvad täiustatud jälgimise ja aruandluse eest tasu.

Nende saitide mõõt DNS-i latentsused ja Kuva tööriistade lahendatud IP-aadresse klient maailmas-asukohtade jaoks. Nende tööriistade enamik vahemälu pole DNS-i tulemused. Seetõttu tööriistade Kuva täielik DNS otsing iga kord, kui käivitatakse test. Kui test oma kliendi, ilmneda ainult täielik DNS-i otsingu jõudlust TTL ajaks üks kord.

## <a name="sample-tools-to-measure-dns-performance"></a>DNS-i jõudluse mõõtmiseks valimi tööriistad

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS pakub mitmeid jõudluse tööriistu. DNS-i võrdlus tööriista abil saate kuvada, kui kaua võtab lahendamiseks oma DNS-i nimi ja kuidas mis võrdleb DNS-i väliselt.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Üks lihtsaim tööriistad on WebSitePulse. Sisestage URL-i DNS-i kulunud aeg, esimene bait, viimasele ja muude tootlusstatistika kuvamiseks. Saate valida kolme eri asukohtadest. Selles näites näete, et esimese täitmise näitab, et DNS-i otsing võtab 0,204 sec.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Kuna tulemuste vahemällu salvestamise, teise testi sama liikluse haldur lõpp-punkti DNS otsing võtab 0,002 sec.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [CA rakenduse sünteetiline jälgimine](https://asm.ca.com/en/checkit.php)

    Watchmouse sisse veebisaidil tööriista varem selle saidi kuvada teie DNS-i eraldusvõime aja mitme geograafiliste piirkondade korraga. Sisestage URL-i DNS-i kulunud aeg, ühenduse loomise ajal ja kiiruse mitu geograafiliste asukohtade kuvamiseks. Selle testi abil saate vaadata, millised majutatud teenuse tagastatakse erinevates kohtades kogu maailmas.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    See tööriist on tootlusstatistika iga element veebilehele. Kuvatakse leht analüüsi vahekaardil DNS-i otsingu ajakulu protsent.

- [Mis on minu DNS-i?](http://www.whatsmydns.net/)

    See sait ei DNS-i 20 erinevatest asukohtadest ja kuvab tulemuste kaardil.

- [Liidese tõrkeid](http://www.digwebinterface.com)

    See sait näitab põhjalikumat DNS-i teavet, sh CNAMEs ja soovitud kirjeid. Veenduge, et kontrollida "Toonimine väljundi" ja 'Statistika' jaotises Suvandid, ja valige "Kõik" jaotises nimeservereid.

## <a name="next-steps"></a>Järgmised sammud

[Liikluse haldur liikluse marsruutimise meetodite kohta](traffic-manager-routing-methods.md)

[Testige oma liikluse haldur](traffic-manager-testing-settings.md)

[Toimingute kohta liikluse Manager (REST API teatmematerjalid)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure'i liikluse Manager cmdlet-käsud](http://go.microsoft.com/fwlink/p/?LinkId=400769)
