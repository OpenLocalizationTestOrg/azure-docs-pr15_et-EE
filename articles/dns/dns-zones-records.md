<properties 
   pageTitle="DNS-i tsoonid ja kirjete | Microsoft Azure'i" 
   description="DNS-i tsoonid ja kirjete Microsoft Azure'i DNS-i majutusteenuse tugi ülevaade." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS-i tsoonid ja kirjed

Selle lehe selgitatakse domeenid, DNS-i tsoonid, ja DNS-i kirjed ja kirje komplekti ja kuidas nad ei toeta Azure DNS-i võtme.

## <a name="domain-names"></a>Domeeninimed
 
IP-aadress on domeeni hierarhia. Hierarhia algab '' juurdomeeni nime, kelle nimi on lihtsalt**.**.  Selle all tulevad üladomeenide, nt com, "neto", "organisatsiooniskeemi", "Suurbritannia" või "jp".  Need allpool on teise taseme domeenid, näiteks "org.uk" või "co.jp". Domeenide DNS-i hierarhias on globaalselt jaotatud korraldaja DNS-i nimeserverite kogu maailmas.

Domeeninimeregistraatorilt on ettevõte, mis võimaldab teil osta domeeni nimi, näiteks "contoso.com".  Domeeninime ostmine annab õiguse kontrollida DNS-i hierarhia selle nime all, näiteks võimaldab teil suunaks nimi "www.contoso.com' teie ettevõtte veebisaidilt. Funktsiooni domeeniregistraatori võib majutada oma nimeserverite teie nimel domeeni või teise võimalusena saate määrata alternatiivse nimeservereid.

Azure'i DNS-i pakub globaalselt hajutatud, kõrge-saadavus nime serveri taristu, mille abil saate majutada oma domeeni. Domeeni Azure'i DNS-i majutusteenuse, saate hallata oma DNS-kirjete samasse mandaat API-de tööriistad, arveldamine ja abi oma Azure teenused.

Azure'i DNS-i ei toeta praegu domeeninime ostmine. Kui soovite osta domeeni nimi, peate kasutama kolmanda osapoole domeeninimeregistraatorilt. Funktsiooni domeeniregistraatori on tavaliselt väikest tasu iga-aastase. Domeenide saate majutatud seejärel Azure'i DNS-i DNS-i kirjete haldamiseks. Üksikasjad leiate [Azure'i DNS-i domeeni volitatud esindaja](dns-domain-delegation.md) .

## <a name="dns-zones"></a>DNS-i tsoonid 

DNS-i tsooni kasutatakse domeeni DNS-i kirjeid. Oma domeeni Azure'i DNS-i majutusteenuse alustamiseks peate looma DNS-i tsooni domeeninime. Iga teie domeeni DNS-i kirje on loodud siis selle DNS-i tsooni sees. 

Näiteks "contoso.com" domeeni võib sisaldada mitu DNS-i kirjeid, nt mail.contoso.com"(jaoks meiliserver) ja www.contoso.com" (web site) jaoks.

DNS-i tsooni loomisel Azure'i DNS-i tsooni nimi peab olema kordumatu ressursirühma sees. Sama tsooni nime saab korduvkasutada erinevate ressursirühm või mõnda muud Azure tellimust. Kui mitme tsoonid sama nime anda ühiskasutusse, on iga eksemplari määratud erinevate nimeserverite aadressid. Ainult ühe komplekti aadressid saab konfigureerida domeeniregistraatori juures domeeni nimi.

>[AZURE.NOTE] Teil pole oma domeeninime domeeninime Azure'i DNS-i DNS-i tsooni loomiseks. Siiski peate konfigureerima nimeserverite Azure'i DNS-i domeeniregistraatori juures domeeni nimi domeeninime jaoks õige nimeserverite domeeni omanik.

Lisateavet leiate teemast [volitatud esindaja Azure'i DNS-i domeeni](dns-domain-delegation.md).

## <a name="dns-records"></a>DNS-i kirjed

### <a name="record-types"></a>Kirjete tüübid

Iga DNS-i kirje on nimi ja tüüp. Kirjed on korraldatud erinevaid neis andmetel. Kõige levinum on "A" kirje, mis kaartide nimi IPv4-aadress. Teine levinud tüüp on 'MX-' kirje, mis kaardid meiliserveri nimi.

Azure'i DNS-i toetab kõiki levinud DNS-i kirje: A-, AAAA, CNAME, MX, NS, PTR, SOA, SRV ja TXT.

### <a name="record-names"></a>Väljade nimed

Azure'i DNS-i, kirjed on määratud suhteline nimede abil. *Täielik* domeeninimi (FQDN) sisaldab tsooni nimi, *suhteline* nimi ei. Näiteks suhteline kirje nimi "www" zone "contoso.com" annab täielikult kvalifitseeritud kirje nimi "www.contoso.com".

*Tipp* kirje on DNS-i tsooni DNS-i kirje juursait (või *tipp*). Näiteks DNS-tsooni "contoso.com", on tipp kirje on ka täielik nimi "contoso.com" (seda nimetatakse mõnikord *alasti* Domeen).  Mess suhteline nime järgi '@' tipp kirjete loomiseks.

### <a name="record-sets"></a>Kirje komplektid
Mõnikord on vaja rohkem kui üks DNS-i kirje loomiseks antud nimi ja tüüp. Oletame näiteks, et 'www.contoso.com' veebisaiti majutatakse kaks erinevat IP-aadressid. Veebisaidi jaoks on vaja kaks erinevat kirjet, üks iga IP-aadress. See on kirjekomplekti näide:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure'i DNS-i haldab DNS-kirjeid *kirje määrab*. Kirjekomplekti (tuntud ka kui *Ressursi* kirjekomplekti) on DNS-i kirjete tsoonis, mis on sama nime ja on sama tüüpi. Enamik kirje komplektid sisaldavad üheks kirjeks, kuid näiteid, nagu see, mille kirjekomplekti sisaldab rohkem kui üks kirje, ei ole aeg-ajalt.

Näiteks Oletame, et olete juba loonud A-kirje "www" tsoonis "contoso.com", osutab IP aadress "134.170.185.46" (esimese kirje ülaltoodud).  Lisate teise kirje, mis olemasoleva kirjekomplekti salvestada, selle asemel, et luua uusi kirje loomiseks.

SOA ja CNAME-i kirjete tüübid on erandid. DNS-i standardid ei luba mitu kirjet sama nimega selliste failitüüpide, seega kogumite kirje võib sisaldada ainult üks kirje.

### <a name="time-to-live"></a>Aja live

Aeg live või TTL, saate määrata, kui kaua iga kirje vahemällu kliendid enne uuesti esitama päringu. Ülaltoodud näites on TTL 3600 sekundit või 1 tund.

Azure'i DNS-i, TTL jaoks on määratud kirjekomplekti, mitte iga kirje jaoks nii, et kõik kirjed, et kirjekomplekti sees kasutatakse sama väärtuse.  Saate määrata mis tahes TTL väärtus vahemikus 1 – 2,147,483,647 sekundit.

### <a name="wildcard-records"></a>Metamärkide kirjed

Azure'i DNS-i toetab [metamärkide kirjeid](https://en.wikipedia.org/wiki/Wildcard_DNS_record). (Kui ei ole-metamärkide kirjekomplekti kaudu Täpsem vaste), tagastatakse kattuvad nimega mis tahes päringu vastuseks metamärkide kirjed. Kõigi kirjetüüpide peale NS ja SOA metamärkide kirje komplektid on toetatud.  

Metamärkide kirjekomplekti luua, kasutada kirje nime seadmiseks "\*". Teise võimalusena võite kasutada ka nimi on "\*"sildina selle vasakpoolse, näiteks"\*.foo".

### <a name="cname-records"></a>CNAME-kirje

CNAME-kirje komplekti ei saa koos töötada sama nimega hulki kirje. Näiteks ei saa luua CNAME-kirje suhteline nimega "www" seadmine ja A-kirje suhteline nimega "www" samal ajal.

Kuna zone tipp (nime = ‘@’) sisaldab alati NS ja SOA salvestada komplekti, mis on loodud tsooni loomisel, ei saa luua tsooni tipus seada CNAME-kirje.

Need piirangud tulenevad DNS-i standardid ja Azure DNS-i piiranguid pole.

### <a name="ns-records"></a>NS-kirjed

Mõne NS kirjekomplekti luuakse automaatselt iga tsooni tipus (nime = ‘@’), ja kustutatakse automaatselt, kui tsooni kustutatakse (seda ei saa kustutada eraldi).  Saate muuta selle kirjekomplekti TTL, kuid te ei saa muuta kirjed, mis on eelkonfigureeritud Azure'i DNS-i tsooni määratud nimeserverite viidata.

Saate luua ja kustutada teiste NS-kirjed tsoonis pole tsooni tipus.  See võimaldab teil konfigureerida lapse zones (vt [Delegating alamdomeenide Azure'i DNS-i](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns)).

### <a name="soa-records"></a>SOA kirjed

SOA kirjekomplekti luuakse automaatselt iga tsooni tipus (nime = ‘@’), ja kustutatakse automaatselt, kui tsooni kustutatakse.  SOA kirjeid ei saa loodud või kustutatud eraldi. 

Saate muuta, välja arvatud 'host' atribuut, mis on eelkonfigureeritud viidata esmane nimi serveri nime järgi Azure'i DNS-i kirje SOA kõik atribuudid.

### <a name="spf-records"></a>SPF-kirjed

Saatja poliitika raamistiku (SPF) kirjete kasutatakse täpsustades mis e-posti serverid on lubatud antud domeeninime nimel meilisõnumi saatmine.  Õige konfiguratsiooni SPF-kirje on oluline, et vältida teie e-posti 'rämpspostiks märkimine adressaadid.

DNS-i teadusfondi algselt kasutusele "SPF" uus kirjetüüp toetama seda stsenaariumi. Vanema nimeserverite toetamiseks lubatud ka kasutamine TXT-kirje tüüp määramiseks SPF-kirje.  See ebaselgust viinud segadust, mis on lahendada [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1).  See on märgitud, et SPF-kirjed tuleks luua ainult TXT-kirje tüüp ja aegunud SPF-kirje tüüp. 

**SPF-kirjeid toetavad Azure'i DNS-i ja tuleks luua TXT-kirje tüüp.** SPF aegunud kirje tüüp pole toetatud. Kui [faili importimine DNS-i tsooni](dns-import-export.md), mis tahes SPF-kirje, kasutades SPF-kirje tüüp teisendatakse TXT-kirje tüüp. 

### <a name="srv-records"></a>SRV-kirjed

[SRV-kirjed](https://en.wikipedia.org/wiki/SRV_record) kasutatakse erinevate osakondade Määrake serveri asukohad. Kui soovite määrata Azure'i DNS-i SRV-kirje:

- *Teenuse* ja *protokolli* peab olema määratud kirje hulga nimi, eesliide allakriipsutatud märkideks osana.  Näiteks "\_sip. \_tcp.name ".  Kirje tsooni tipus, ei ole vaja määrata '@' kirje nimi lihtsalt kasutada teenust ja protokoll, näiteks "\_sip. \_tcp ".
- *Priority (prioriteet)*, *Weight (kaal)*, *portide*ja *sihtkoht* on määratud parameetrite iga kirje kirje määramine. 

## <a name="tags-and-metadata"></a>Siltide ja metaandmed

### <a name="tags"></a>Sildid
Sildid on loendi nimi ja väärtuse paarideks ja kasutatakse Azure ressursihaldur sildi ressursid.  Azure'i ressursihaldur siltide abil filtreeritud vaadete Azure arve lubamine ja abil saate ka siltide vajalike poliitika määramine. Siltide kohta leiate lisateavet teemast [kasutamine siltide korraldamiseks oma Azure ressursse](../resource-group-using-tags.md).

Azure'i DNS-i toetab Azure ressursihaldur siltide kasutamine DNS-i tsooni ressursid.  DNS-i kirje komplekti ei toeta sildid.

### <a name="metadata"></a>Metaandmete

Alternatiivina kirje määramine sildid, toetab Azure DNS-i marginaalid lisanud kirje ' metaandmete ' komplektid.  Sarnaselt sildid, metaandmete võimaldab seostada iga kirjekomplekti nime ja väärtuse paarideks.  See võib olla kasulik, näiteks kirje iga kirjekomplekti eesmärk.  Erinevalt sildid, metaandmete ei saa kasutada filtreeritud vaate Azure arve ja Azure ressursihaldur poliitika ei saa määrata.

## <a name="limits"></a>Piirangud

Järgmised vaikimisi piirangud kehtivad Azure'i DNS-i kasutamisel.

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Järgmised sammud

- Azure'i DNS-i kasutamise alustamiseks saate teada, kuidas [luua DNS-i tsooni](dns-getstarted-create-dnszone-portal.md) ja [DNS-kirjete loomine](dns-getstarted-create-recordset-portal.md).
- Mõne olemasoleva DNS-i tsooni migreerimiseks saate teada, kuidas [impordi- ja DNS-i tsooni faili](dns-import-export.md).
