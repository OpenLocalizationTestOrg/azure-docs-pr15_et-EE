<properties 
    pageTitle="Armatuurlaua, jälgida, skaala konfigureerimiseks ja hübriid ühendused BizTalki teenused | Microsoft Azure'i" 
    description="Lisateavet juhtelementide ja BizTalki teenuste klassikaline portaali vahekaartidel jõudluse jälgimist: armatuurlaud, kuvar, skaala, konfigureerimine ja hübriid ühendused. MABS WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Vaadake üle vahekaartidel armatuurlaud, kuvar, skaala, konfigureerimine ja ühenduse hübriid

Kui loote BizTalki teenust ja juurutada rakendust, saate muuta mõningaid BizTalki Teenusesätted ja jälgida rakenduse jõudlus. 

Azure'i klassikaline portaali avamisel saate on automaatselt paigutatud menüü **Kõik üksused** . BizTalki teenust kuvamiseks valige vahekaardil **Kõik üksused** BizTalki teenust või valige vahekaart **BIZTALKI teenuste** ja seejärel valige oma BizTalki teenuse nimi.

Järgmised sakkidega avatakse uus aken. Selles teemas kirjeldatakse järgmisi vahekaarte.

## <a name="quick-start-quick-startquickstart"></a>Lühijuhend)![Lühijuhend][QuickStart])
Sõltuvalt BizTalki Services Edition, ei pruugi kõik loendis suvandid saadaval. 
<table border="1">
    <tr>
        <td><strong>Tööriistad</strong></td>
        <td>Laadige alla teenuste BizTalki SDK Visual Studio projekti Mallid kohapealse arengu arvutisse installimiseks. Need Mallid luua <strong>BizTalki Services</strong> (bridge) ja BizTalki teenust juurutatud <strong>BizTalki teenuse artefaktide</strong> (transformatsioon) Visual Studio projektid.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Kuidas alustada abil Azure'i BizTalki teenuste SDK</a> ja <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Azure BizTalki teenuste SDK installimine</a> on loetletud toimingud alustada.
        </td>
    </tr>
    <tr>
        <td><strong>Partneri lepingute loomine</strong></td>
        <td>Azure'i BizTalki Services portaal majutatud Azure, kus lisage partneritega ja luua X12 AS2, avaneb ja EDIFACT EDI lepingud.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigureerida komponendid EDI sõnumside BizTalki teenuste portaalis</a> loetletud toimingud alustada.
        </td>
    </tr>

<tr>
        <td><strong>Lisateavet BizTalki teenused</strong></td>
        <td>Avage soovitud <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">õppekeskust</a> Azure BizTalki teenuste kohta.</td>
</tr>
</table>


Allosas asuva tegumiriba, saate teha järgmist.

<table border="1">

<tr>
<td><strong>Haldamine</strong> teie rakenduse juurutamine</td>
<td>Avaneb Azure'i BizTalki Services portaal. BizTalki Services portaal on EDI konfiguratsiooni, lisades partnerid ja loomise X12 AS2, sh sissepääsu ja EDIFACT lepingud.
<br/><br/>
See on sama, mis <strong>Loo partneri lepingute</strong> vahekaardil <strong>Quick Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigureerimise komponendid EDI sõnumside BizTalki Services portaal</a> annab Lisateavet BizTalki Services portaal.</td>
</tr>

<tr>
<td><strong>Ühenduseteavet</strong> juurdepääsu juhtimine Namespace.</td>
<td>Kui valite ühenduseteavet, siis juurdepääsu juhtimine Namespace, vaikimisi väljaandja ja vaikimisi võti kuvatakse. Saate kopeerida need väärtused.
<br/><br/>
Saate avada ka juurdepääsu juhtimine portaali. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Loo pääsuõiguste Namespace</a> annab Lisateavet juurdepääsu juhtimine portaalis.</td>
</tr>

<tr>
<td><strong>Sünkroonimine võtmed</strong> salvestusruumi konto</td>
<td>Salvestusruumi konto loomisel primaarvõtme ja teisese võtme luuakse automaatselt. Need krüptimise võtmed salvestusruumi konto juurdepääsu reguleerimine. BizTalki teenust kasutab automaatselt primaarvõtme. <strong>Sünkroonimine klahvid</strong> võimaldavad kasutajatel ilma katkestades BizTalki teenuse primaarvõtme ja teisese võtme vaheldumisi aktiveerimine.
<br/><br/>
Näiteks soovite BizTalki teenust kasutada uut primaarvõtme salvestusruumi konto. Soovitud toiming
<br/><br/>
<ol>
<li>BizTalki teenust ja valige <strong>Sünkrooni võtmed</strong>. Valige teisene võti. Kui te seda teete, käivitab BizTalki teenuse teisese võtme abil.</li>
<li>Azure'i klassikaline portaalis, valige konto salvestusruumi ja taastada primaarvõti. Pidage meeles, et teie BizTalki teenus on kasutusel teisese võtme.</li>
<li>BizTalki teenust ja valige <strong>Sünkrooni võtmed</strong>. Valige nüüd primaarvõti. See on uus primaarvõtme saate uuesti.</li>
<li>Azure'i klassikaline portaalis, valige oma konto salvestusruumi ja teisese võtme taastada.</li>
</ol>
<br/>
Seda nimetatakse "edastamiseks klahve". Eesmärk on, et kasutajad saaksid ilma katkestades BizTalki teenuse primaarvõtme ja teisese võtme vaheldumisi aktiveerimine.</td>
</tr>

<tr>
<td><strong>Kustutage</strong> rakenduse</td>
<td>Kui valite kustutamiseks BizTalki teenust ja kõigi üksuste juurutatud see eemaldatakse.</td>
</tr>
</table>


## <a name="dashboard"></a>Armatuurlaua
Sõltuvalt BizTalki Services Edition, ei pruugi kõik loendis suvandid saadaval. 

Kui valite oma BizTalki teenuse nimi, kuvatakse vahekaart armatuurlaud. Armatuurlaua, saate teha järgmist.

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Kasutus ülevaade: Kuvatakse kasutatud hübriid ühenduste arv
Kuvab ka GB andmete kasutamine. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Argumendil graafik: Fikseeritud loendi jõudluse mõõdikute esitab
Need mõõdikud sisestama reaalajas väärtused BizTalki teenuse seisundi kohta. Saate valida ka ajavahemiku mõõdikute, kuvatakse diagrammil kuvatavate **intervall** ja **suhteline** või **absoluutne** väärtuste. 

Nende jõudluse mõõdikute kirjeldus, minge [Saadaval mõõdikute](#Metrics) selles teemas.


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Kiire ülevaade: Loetletakse teie BizTalki teenuse atribuudid

<table border="1">

<tr>
<td><strong>Andmebaasi jälgimine mandaadi värskendamine</strong></td>
<td>Muudab kasutajanime ja parooli jälgimise andmebaasi logitud.</td>
</tr>
<tr>
<td><strong>SSL-serdi värskendamine</strong></td>
<td>Saate värskendada BizTalki teenust kasutada eri SSL-sert. Iseallkirjastatud SSL-serdi luuakse automaatselt, kui saate <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">BizTalki teenuse loomine</a>.</td>
</tr>
<tr>
<td><strong>Laadige alla serdi</strong></td>
<td>SSL-serdi kasutavad teie kohalikus arvutis BizTalki teenuse saate alla laadida.</td>
</tr>
<tr>
<td><strong>Olek</strong></td>
<td>Kuvatakse praegune olek BizTalki teenust. Vt <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalki teenuste: teenuse olek diagrammi</a>. </td>
</tr>
<tr>
<td><strong>Teenuse URL-i</strong></td>
<td>Teie BizTalki teenuse URL. See on sama <strong>Domeeni URL-i</strong> sisestatud BizTalki teenust loomisel.</td>
</tr>
<tr>
<td><strong>Avaliku virtuaalse (VIP)-IP-aadress</strong></td>
<td>IP-aadress BizTalki teenust. Seda kasutatakse kõigi Sisestuskeel lõpp-punktid ja on väljaminev liiklus allikas aadress. IP-aadress kuulub BizTalki teenust, kui see on loodud. Kui kustutate BizTalki teenuse, IP-aadress on määratud muu BizTalki teenus.</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>Autendib BizTalki teenuse.</td>
</tr>
<tr>
<td><strong>Väljaande</strong></td>
<td>Loendite väljaanne sisestatud BizTalki teenuse loomisel.</td>
</tr>
<tr>
<td><strong>Asukoht</strong></td>
<td>Kuvab geograafilised piirkond, mis hostib BizTalki teenust.</td>
</tr>
<tr>
<td><strong>Loodud</strong></td>
<td>Kuvatakse kuupäev ja kellaaeg, mis on loodud BizTalki teenuse.</td>
</tr>
<tr>
<td><strong>Andmebaasi jälgimine</strong></td>
<td>Azure'i SQL-andmebaasi nimi, mis talletab jälgimise tabelid BizTalki teenust kasutada. 
<br/><br/>Jälitamise andmebaasi 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Nõuete ülevaade</a> sisaldab üksikasju.</td>
</tr>
<tr>
<td><strong>Salvestusruumi jälgimine/arhiivimine</strong></td>
<td>Azure Storage konto nimi, mis talletab jälgimise väljund BizTalki teenust.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Nõuete ülevaade</a> annab salvestusruumi konto üksikasjad.</td>
</tr>
<tr>
<td><strong>Tellimuse nimi</strong></td>
<td>Loetletud tellimus, mis hostib BizTalki teenust. Tellimuse reguleerib juurdepääs Azure'i klassikaline portaali.</td>
</tr>
<tr>
<td><strong>Tellimuse ID</strong></td>
<td>Tellimuse loomisel Tellimuse ID luuakse automaatselt. Kui kasutate REST API-d, peate sisestage Tellimuse ID</td>
</tr>
</table>

[BizTalki teenuste: ettevalmistamise abil Azure klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=302280) on loetletud toimingud BizTalki teenuse.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Juhtimiseks, ühenduseteavet, sünkroonimise klahvid, ja kustutada tegumiriba kaudu.

<table border="1">

<tr>
<td><strong>Haldamine</strong> oma rakenduse juurutamine</td>
<td>Avaneb Azure BizTalki Services portaal. BizTalki Services portaal on EDI konfiguratsiooni, lisades partnerid ja loomise X12 AS2, sh sissepääsu ja EDIFACT lepingud.
<br/><br/>
See on sama, mis <strong>Loo partneri lepingute</strong> vahekaardil <strong>Quick Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigureerimise komponendid EDI sõnumside BizTalki Services portaal</a> annab Lisateavet BizTalki Services portaal.</td>
</tr>
<tr>
<td><strong>Ühenduseteavet</strong> juurdepääsu juhtimine Namespace.</td>
<td>Kuvab Access juhtelemendi Namespace, vaikimisi väljaandja ja klahvi vaikimisi väärtused; mis saab kopeerida.
<br/><br/>
Saate avada ka juurdepääsu juhtimine portaali. See juurdepääsu juhtimine portaal on sama, mis Active Directory suvandiga Vasakpoolsel navigeerimispaanil.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Hallata oma ACS Namespace</a> annab Lisateavet juurdepääsu juhtimine portaalis.</td>
</tr>
<tr>
<td><strong>Sünkroonimine võtmed</strong> salvestusruumi konto</td>
<td>Salvestusruumi konto loomisel primaarvõtme ja teisese võtme luuakse automaatselt. Need krüptimise võtmed salvestusruumi konto juurdepääsu reguleerimine. BizTalki teenust kasutab automaatselt primaarvõtme. <strong>Sünkroonimine klahvid</strong> võimaldavad kasutajatel ilma katkestades BizTalki teenuse primaarvõtme ja teisese võtme vaheldumisi aktiveerimine.
<br/><br/>
Näiteks soovite BizTalki teenust kasutada uut primaarvõtme salvestusruumi konto. Soovitud toiming
<br/><br/>
<ol>
<li>BizTalki teenust ja valige <strong>Sünkrooni võtmed</strong>. Valige teisene võti. Kui te seda teete, käivitab BizTalki teenuse teisese võtme abil.</li>
<li>Azure'i klassikaline portaalis, valige konto salvestusruumi ja taastada primaarvõti. Pidage meeles, et teie BizTalki teenus on kasutusel teisese võtme.</li>
<li>BizTalki teenust ja valige <strong>Sünkrooni võtmed</strong>. Valige nüüd primaarvõti. See on uus primaarvõtme saate uuesti.</li>
<li>Azure'i klassikaline portaalis, valige oma konto salvestusruumi ja teisese võtme taastada.</li>
</ol>
<br/>
Seda nimetatakse "edastamiseks klahve". Eesmärk on, et kasutajad saaksid ilma katkestades BizTalki teenuse primaarvõtme ja teisese võtme vaheldumisi aktiveerimine.</td>
</tr>

<tr>
<td><strong>Kustutage</strong> rakenduse</td>
<td>BizTalki teenust ja selle juurutatud kõik üksused on eemaldatud.</td>
</tr>
</table>


## <a name="monitor"></a>Jälgimine
Ei kehti tasuta Edition.

Kui valite oma BizTalki teenuse nimi, vahekaardil kuvar on saadaval ja kuvatakse järgmine:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Argumendil joonis: Kuvatakse valitud jõudluse mõõdikud
Need mõõdikud sisestama reaalajas väärtused BizTalki teenuse seisundi kohta. Saate valida, millised jõudluse mõõdikute kuvatakse. Korraga saab kuvada kuni kuus jõudluse mõõdikute. 

Saate valida ka **suhteline** või **absoluutne** väärtused ja ajavahemiku **intervall** mõõdikute, mis on kuvatud. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Eemaldamine või graafik mõõdikute kuvamiseks tehke järgmist.
1. Valige vahekaardil **kuvar** .
2. Valige **Lisa mõõdikute** tegumiriba kaudu:  
![Valige lisa mõõdikud][AddMetrics]
3. Märkige ruut jõudluse mõõdikute, mida soovite kuvada.
4. Valige vahekaardil **kuvar** naasmiseks märke.
5. Valige ringi meetermõõdustik kuvada selle väärtuseks meetermõõdustik väärtus graafik kõrval.  

    Näiteks on Hall **CPU hõivatus** mõõt; selle väljundi ei kuvata joonisel:  
![CPU hõivatus meetermõõdustik on Hall][GrayedMetric]  

    Valige soovitud halliks toonitud ringi lubamiseks **CPU hõivatus** meetermõõdustik kuvada selle väljundi graafik:  
![CPU hõivatus meetermõõdustik on lubatud][EnabledMetric]

6. Kuva graafik ja loendi mõõdiku eemaldamiseks valige **Kustutamine meetermõõdustik** tegumiriba kaudu. Argumendil tagasi loendisse lisada, valige **Lisa mõõdikute** tegumiriba kaudu, märkige ruut mõõdiku ja märke vahekaardil **kuvar** naasmiseks valige. Valige soovitud halliks toonitud ringi mõõdiku lubamiseks.

## <a name="Metrics"></a>Saadaval mõõdikud
Saadaval on järgmised jõudluse hinnale/mõõdikute.

<table border="1">

<tr>
<td><strong>RountdTrip latentsus</strong></td>
<td>Kuvab kõik sillad üle aeg sõnumi kuni sõnumi töödeldakse täielikult BizTalki teenus võttis sõnumi keskmiselt kuluv aeg millisekundites ([ms). Arvestatakse ainult sõnumite edukalt töödeldud.<br/><br/>
Kui järgmised sündmused, luuakse ajatempel:
<ul>
<li>Sõnumi sisestab lüüsi</li>
<li>Sõnum on suunatakse sihtkoht</li>
<li>Sihtkoha tagasiside on saadud</li>
<li>Sihtkoha kinnituse vastuse saata lüüsi</li>
</ul>
<br/>
Selle mõõdiku kuvatakse järgmised arvutamise tulemus:
<br/><br/>
[Sihtkoha kinnituse vastuse saata lüüsi] - [sõnumi sisestab lüüsi]</td>
</tr>
<tr>
<td><strong>Tõrked allikas</strong></td>
<td>Sõnumid, mida ei saanud koguarv kuvatakse BizTalki teenuse kui tõmbamine sõnumite allika lõpp-punktid.</td>
</tr>
<tr>
<td><strong>CPU hõivatus</strong></td>
<td>Loetleb kõik rolli aknad Keskmine % protsessori aja.</td>
</tr>
<tr>
<td><strong>Latentsus töötlemine</strong></td>
<td>Kuvab selle Keskmine kuluv aeg millisekundites ([ms) sõnumi BizTalk teenus võttis sildu, välja arvatud sihtkohad kulunud aeg üle. Arvestatakse ainult sõnumite edukalt töödeldud.<br/><br/>
Iga järgmise sündmuse korral luuakse ajatempel:

<ul>
<li>Sõnumi sisestab lüüsi</li>
<li>Sõnum on suunatakse sihtkoht</li>
<li>Sihtkoha tagasiside on saadud</li>
<li>Sihtkoha kinnituse vastuse saata lüüsi</li>
</ul>
<br/>Selle mõõdiku kuvatakse järgmised arvutamise tulemus:<br/><br/>
[Sihtkoha kinnituse vastus saadetakse lüüsi] - [sõnumi sisestab lüüsi] - [sihtkoha tagasiside on saadud] + [sõnumi marsruuditakse sihtkohta]</td>
</tr>
<tr>
<td><strong>Protsessi tõrked</strong></td>
<td>Kuvab koguarvu sõnumid, mis ajavahemiku jooksul üle kõik sillad BizTalki teenus võttis töötlemise ajal nurjus.</td>
</tr>
<tr>
<td><strong>Saadetud sõnumid</strong></td>
<td>Kuvab sõnumid saadetakse BizTalki teenuse üle kõik sillad ajavahemiku jooksul koguarv. Selle mõõdiku muudeta müügivõimaluste saadetud sõnumi saabumisel marsruutimiseks sihtkoht. Selle mõõdiku ei tähenda, et sõnum on edukalt töödeldud.<br/><br/>
Kutse – vasta stsenaariumi muudeta mõõdiku kui marsruutimiseks sihtkohta saadetakse teatis kinnituse tagasi tulemas.</td>
</tr>
<tr>
<td><strong>Vastuvõetud sõnumid</strong></td>
<td>Kuvab sõnumeid, BizTalki teenus üle kõik sillad ajavahemiku jooksul koguarv. Selle mõõdiku muudeta uue sõnumi saabumisel tulemas järgi.</td>
</tr>
<tr>
<td><strong>Sõnumite protsess</strong></td>
<td>Kuvab praegu töödeldavate BizTalki teenuse ajavahemiku jooksul sõnumite arv.</td>
</tr>
<tr>
<td><strong>Sõnumite töödeldud</strong></td>
<td>Kuvab edukalt töödelda BizTalki teenuse üle kõik sillad ajavahemiku jooksul sõnumite arv. Selle mõõdiku muudeta kui sõnum on edukalt saadud tulemas ja kõigi sihtkohta.</td>
</tr>
</table>


## <a name="scale"></a>Skaala
Klõpsake menüüs skaala saate lisada või lahutamine ühikute BizTalki teenust kasutada. Vaikimisi on üks üksus konfigureeritud. Täiendavate üksuste saab lisada mastaapida BizTalki teenust. Kui ulatuse suurendamiseks siis suureneb läbilaskevõime. Ressursside hulga suureneb ka, sh juurutatud sillad, lepingud, LOB ühendused ja töötlemine power. Näiteks saate suurendada skaala 1 üksusest 2 ühikut. Sellisel juhul saate juurutada kahekordseks sildade arv, topeltklõpsake lepingute, topeltklõpsake LOB ühendused ja topeltklõpsake töötlemise võimsus.

Mõned BizTalki väljaanded ei paku skaala suvand. Sellisel juhul üks üksus on lubatud. Saate oma väljaande ülespoole mitu ühikut määratlemiseks viidata [BizTalki teenuste: väljaanded diagrammi](biztalk-editions-feature-chart.md).

Üksuste arvu võib mõjutada hinnad. Kui ühikute suurendamiseks valimine **salvestada** kuvatakse teade, mis ütleb teile, kui arveldamine mõjutab. Seejärel võite jätkata. Kui üksused on BizTalki teenuse olek muutub aktiivne abiteemast arvu suurendada. Olekus abiteemast jätkuvalt BizTalki teenust käivitada.

[BizTalki teenuste: väljaanded diagrammi](biztalk-editions-feature-chart.md) määratleb "Üksus".


## <a name="configure"></a>Konfigureerimine
Ei kehti hübriid ühendused.

Määrab varukoopia olek pole või automaatne. Kui määratud pole, pole varukoopiate luuakse automaatselt. Kui valitud automaatne, saate konfigureerida varundusasukohta sageduse varunduse, ja kui kaua varukoopia faile. 

[BizTalki teenuste: varundus ja taaste](biztalk-backup-restore.md) sisaldab üksikasju. 


## <a name="HybridConnections"></a>Hübriidjuurutuse ühendused
Azure'i rakendust, veebirakenduste või mobiilirakenduste kohta teenuses Azure rakenduse ühenduse hübriid ühendused kohapealse ressurss, mis kasutab staatiline TCP pordi, nt SQL Server, MySQL-i, HTTP Web API-d ja enamik kohandatud veebiteenused. Hübriid ühendused hallatakse BizTalki teenuste Azure klassikaline portaalis.

Hübriidjuurutuse ühendused teenuses Azure rakenduse loomiseks vaadake teemat [Accessi kohapealse hübriid ühendused teenuses Azure rakenduse abil](../app-service-web/web-sites-hybrid-connection-get-started.md).

Saate luua või hallata hübriid ühendused Azure'i BizTalki teenused, vt [Hübriid ühendused](integration-hybrid-connection-overview.md).



## <a name="next"></a>Järgmise
Nüüd, kui olete tuttav erinevaid vahekaarte, leiate Azure'i BizTalki teenuste funktsioonide:

- [BizTalki teenused: ahendamine](biztalk-throttling-thresholds.md)  
- [BizTalki teenused: Väljaandja nimi ja väljaandja võti](biztalk-issuer-name-issuer-key.md)  
- [BizTalki Services: Varundus ja taaste](biztalk-backup-restore.md)

## <a name="see-also"></a>Vt ka
- [Hübriidjuurutuse ühendused](integration-hybrid-connection-overview.md)  
- [BizTalki Services: Arendaja, Basic, Standard- ja Premiumi väljaannetes diagramm](biztalk-editions-feature-chart.md)  
- [BizTalki Services: Ettevalmistamise abil Azure'i klassikaline portaal](biztalk-provision-services.md)  
- [BizTalki Services: Diagrammi BizTalki teenuse olek](biztalk-service-state-chart.md)  
- [Kuidas alustada abil Azure'i BizTalki teenuste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
