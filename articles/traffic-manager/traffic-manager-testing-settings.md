<properties
    pageTitle="Testimine liikluse haldur sätted | Microsoft Azure'i"
    description="See artikkel aitab teil testida liikluse haldur sätted"
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

# <a name="test-your-traffic-manager-settings"></a>Testige oma liikluse haldur

Liikluse haldur sätete testimiseks peate mitme kliendi, on erinevates kohtades, kust teie käivitada. Seejärel tuua liikluse haldur profiili alla ükshaaval lõpp-punktid.

* Määrata DNS-i TTL-i väärtust madal, et muudatused kajastuma kiiresti (nt 30 sekundit).
* Leida oma Azure pilveteenustega ja teil on testimine profiili veebisaidid IP-aadressid.
* Kasutada tööriistu, mis võimaldavad teil IP-aadressi lahendamiseks DNS-i nimi ja selle aadressi kuvamine.

On kontrollida, kas DNS-i nimed lahendamiseks IP-aadressid profiili lõpp-punktid. Nimede lahenevad liikluse marsruutimise meetod liikluse haldur profiilis määratletud viisil. Nagu **nslookup** või **tõrkeid** tööriistade abil saate lahendada DNS-i nimed.

Järgmised näited aitavad teil testida profiili liikluse haldur.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Märkige ruut liikluse haldur profiili nslookup ja ipconfig Windowsi abil

1. Avage käsu või Windows PowerShelli käsuviibas administraatorina.
2. Tippige `ipconfig /flushdns` tühjendada DNS-i Resolveri vahemälu.
3. Tippige `nslookup <your Traffic Manager domain name>`. Näiteks järgmine käsk kontrollib eesliite *myapp.contoso* domeeninimi

        nslookup myapp.contoso.trafficmanager.net

    Tüüpilised tulem kuvatakse järgmine teave:

    * DNS-i nimi ja IP-aadressi lahendamiseks selles liikluse haldur domeeninime juurdepääsu DNS-i serveri.
    * Domeeninimi halduri liikluse sisestatud käsu "nslookup" ja IP-aadress, millele liikluse haldur domeeni lahendab pärast. Teine IP-aadress on oluline kontrollida. Selle virtuaalse avaliku IP (VIP) aadressi jaoks ühte pilveteenustega või teil on testimine liikluse haldur profiili veebisaidid peaksid olema samad.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Kuidas testida Tõrkesiirde liikluse marsruutimise meetod

1. Jätke üles kõik lõpp-punktid.
2. Ühe kliendi kasutamisel taotleda DNS-i resolvimise nslookup või sarnase kasuliku abil oma ettevõtte domeeni nimi.
3. Veenduge, et lahendatud IP-aadress vastab esmane lõpp-punkti.
4. Teie peamine lõpp-punkti alla või eemaldada jälgimisega seotud faili, et selle liikluse haldur arvab, et rakendus on alla.
5. Oodake, kuni soovitud DNS-i aeg-to-Live (TTL) liikluse haldur profiili pluss veel kaks minutit. Näiteks kui teie DNS-i TTL on 300 sekundi (5 minutit), peate ootama seitse minutit.
6. Joondada oma DNS-i kliendi vahemälu ja taotluse DNS-i resolvimise Nslookupi abil. Klõpsake Windowsi saate joondada DNS-i vahemälu käsk ipconfig/flushdns.
7. Veenduge, et lahendatud IP-aadress vastab teie teisene lõpp-punkti.
8. Korrake toimingut, iga lõpp-punkti omakorda langetamine. Veenduge, et DNS-i tagastab loendi järgmise lõpp-punkti IP-aadress. Kui kõik lõpp-punktid allapoole, peaksite hankima IP-aadress esmane lõpp-punkti uuesti.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Kuidas testida kaalutud liikluse marsruutimise meetod

1. Jätke üles kõik lõpp-punktid.
2. Ühe kliendi kasutamisel taotleda DNS-i resolvimise nslookup või sarnase kasuliku abil oma ettevõtte domeeni nimi.
3. Veenduge, et lahendatud IP-aadress vastab mõnele oma lõpp-punktid.
4. DNS-i kliendi vahemälu ja korrake juhiseid 2 – 3 iga lõpp-punkti. Peaksite nägema tagastatud iga lõpp-punktide jaoks eri IP-aadressid.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Kuidas testida jõudluse liikluse marsruutimise meetod

Tõhus testimiseks jõudluse liikluse marsruutimise meetod, peab olema kliendid asuvad maailma eri osades. Saate luua eri Azure piirkondades, mida saab kasutada testimiseks teenuste kliendid. Kui teil on globaalne võrgus, saate kaugühenduse teel klientidele kogu maailmas mujal sisse logida ja käivitage teie sealt.

Teine võimalus on veebipõhine DNS-i otsingu tasuta ja pakutavaid tõrkeid. Mõned nende tööriistade teile kontrollida DNS-i nimelahendus kogu maailmas erinevatest asukohtadest. Kas otsingu näiteid "DNS-i otsing". Muu tootja teenused, nt Gomez või ettekande saab kinnitada, et teie profiilid on levitamise liikluse ootuspäraselt.

## <a name="next-steps"></a>Järgmised sammud

* [Liikluse haldur liikluse marsruutimise meetodite kohta](traffic-manager-routing-methods.md)
* [Liikluse haldur jõudluse huvides](traffic-manager-performance-considerations.md)
* [Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)




