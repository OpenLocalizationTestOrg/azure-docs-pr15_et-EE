<properties
    pageTitle="Lisateavet funktsioonide BizTalki teenuste väljaandena | Microsoft Azure'i"
    description="BizTalki teenuste väljaanded võimaluste võrdlus: vaba, arendaja, Basic, Standard, ja. MABS, WABS."
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
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalki Services: Väljaanded diagramm

Azure'i BizTalki Services pakub mitu väljaannetes. Selles artiklis abil saate määratleda, milline versioon sobib teie stsenaarium ja ettevõtte vajadustele.


## <a name="compare-the-editions"></a>Võrrelge väljaanded

**Tasuta (eelvaade)**

Saate luua ja hallata hübriid ühendused. Ühenduse hübriid on lihtne viis Azure veebisait ühenduse loomiseks kohapealse süsteemi, nt SQL serveri.

**Arendaja**

Sisaldab hübriid ühendused, EAI ja elektrooniline sõnumi töötlemine lihtne kasutada kauplemise partneri haldusportaal ja tugi levinud EDI skeeme ja rikkaliku EDI töötlemine X12 ja AS2 üle. Saate luua teenuste pilveteenuses ühendavad lugeda ja kirjutada sõnumite HTTP/S, ülejäänud, FTP, WCF-i ja SFTP protokollidega EAI tavastsenaariumid.  Kasutada ühenduvuse kohapealse LOB süsteemide valmis kasutada SAP-i, Oracle'i e, Oracle'i Andmebaasipakkuja, Siebel ja SQL serveri adapterit. Lihtne ja juurutamine Visual Studio tööriistad arendaja kesksele keskkonnas kasutada. Piiratud Arendus ja testimine eesmärgil ainult koos pole teenindustaseme leping (SLA).

**Tavaline**

Hõlmab kõige arendaja võimalusi, mille suureneb hübriid ühendused, EAI sillad EDI lepingud ja artikkel kehtib ühendused. Pakub kõrge-saadavus ja võimalus mastaapimiseks koos teenindustaseme leping (SLA).

**Standard**

Sisaldab kõiki põhilisi funktsioone, mis suurendab hübriid ühendused, EAI sillad EDI lepingud ja artikkel kehtib ühendused. Pakub kõrge-saadavus ja suvandit mastaapida koos teenindustaseme leping (SLA).

**Premium**

Kaasab kõik Standard võimalusi suureneb hübriid ühendused, EAI sillad EDI lepingud ja artikkel kehtib ühendused. Sisaldab ka arhiivimine, kõrge-saadavus ja võimalus mastaapimiseks koos teenindustaseme leping (SLA).


## <a name="editions-chart"></a>Diagrammi väljaanded
Järgmises tabelis on loetletud erinevused.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Tasuta (eelvaade)</th>
        <th>Arendaja</th>
        <th>Tavaline</th>
        <th>Standard</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Alghinna</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Azure'i BizTalki teenuste hinnad</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure'i hinnad kalkulaator</a></td>
</tr>
<tr>
<td><strong>Vaikimisi minimaalne konfiguratsioon</strong></td>
<td>Tasuta 1 ühik</td>
<td>1 arendaja üksus</td>
<td>1 põhiühik</td>
<td>1 standard üksus</td>
<td>1 premium üksus</td>
</tr>
<tr>
<td><strong>Skaala</strong></td>
<td>Pole skaala</td>
<td>Pole skaala</td>
<td>Jah, 1 põhiühik kerimine</td>
<td>Jah, 1 Standard ühiku kerimine</td>
<td>Jah, 1 Premium ühiku kerimine</td>
</tr>
<tr>
<td><strong>Maksimaalselt lubatud välja skaala</strong></td>
<td>Pole skaala</td>
<td>Pole skaala</td>
<td>8 ühikud</td>
<td>8 ühikud</td>
<td>8 ühikud</td>
</tr>
<tr>
<td><strong>EAI sillad ühiku kohta</strong></td>
<td>Ei sisalda</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>ELEKTROONILINE AS2</strong>
<br/><br/>
Sisaldab TPM lepingud</td>
<td>Ei sisalda</td>
<td>Lisada. 10 lepingute ühiku kohta.</td>
<td>Lisada. 50 lepingute ühiku kohta.</td>
<td>Lisada. 250 lepingute ühiku kohta.</td>
<td>Lisada. 1000 lepingute ühiku kohta.</td>
</tr>
<tr>
<td><strong>Hübriidjuurutuse ühendused ühiku kohta</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hübriidjuurutuse ühenduse andmeedastus (GB) ühiku kohta</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>Kohapealse LOB süsteemide BizTalki adapterit teenuse ühendused</strong></td>
<td>Ei sisalda</td>
<td>1 ühendus</td>
<td>2 ühendused</td>
<td>5 ühendused</td>
<td>25 ühendused</td>
</tr>
<tr>
<td align="left"><strong>Toetatud protokollid/süsteemid:</strong>
<ul>
<li>HTTP KAUDU</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF-I</li>
<li>Teenuse siini (SB)</li>
<li>Azure'i bloobimälu</li>
<li>REST API-d</li>
</ul>
</td>
<td>Ei sisalda</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
</tr>
<tr>
<td><strong>Kõrge-saadavus</strong>
<br/><br/>
Jaoks teenindustaseme leping (SLA), leiate <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Azure'i BizTalki teenuste hinnad</a>.
</td>
<td>Ei sisalda</td>
<td>Ei sisalda</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
</tr>
<tr>
<td><strong>Varundus ja taaste</strong></td>
<td>Ei sisalda</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
</tr>
<tr>
<td><strong>Jälgimine</strong></td>
<td>Ei sisalda</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
</tr>
<tr>
<td><strong>Arhiivimine</strong><br/><br/>
Sisaldab mitte lepingu rikkumine jälitatud meilisõnumeid alla laadima ja teatis (NRR)</td>
<td>Ei sisalda</td>
<td>Sisalduv</td>
<td>Ei sisalda</td>
<td>Ei sisalda</td>
<td>Sisalduv</td>
</tr>
<tr>
<td><strong>Kohandatud koodi kasutamine</strong></td>
<td>Ei sisalda</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
</tr>
<tr>
<td><strong>Teisendusteta, sh kohandatud XSLT kasutamine</strong></td>
<td>Ei sisalda</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
<td>Sisalduv</td>
</tr>
</table>

> [AZURE.NOTE] Paindlikkust vastu riistvara tõrkeid, kõrge-saadavus tähendab on mitu VMs BizTalki ühe üksuse.


## <a name="faqs"></a>Korduma kippuvad küsimused

#### <a name="what-is-a-biztalk-unit"></a>Mis on BizTalki ühiku?
"Üksus" on aatomi tase on Azure BizTalki teenuste juurutamine. Iga väljaanne sisaldab üksust, mis on erinevate Arvuta võimsus ja mälu. Näiteks lihtsa üksus on rohkem kui arendaja Arvuta, Standard on rohkem kui tavaline Arvuta jne. Kui muudate teenuse BizTalki, muudate ühikute.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Mis vahe on BizTalki teenuste ja Azure BizTalki VM?
BizTalki teenuste pakub õige platvormi-kui-a-Service (PaaS) arhitektuur integreerimine lahenduste pilveteenuses. Mudeliga PaaS keskenduda täielikult rakenduse loogika ja jätke kõik taristu haldamise Microsoft, sh:

- Hallata või paik virtuaalmasinates ei ole vaja.
- Microsoft tagab kättesaadavus.
- Saate reguleerida skaala nõudmisel lihtsalt paluda rohkem või vähem võimsuse Azure portaali kaudu.

BizTalki Server Azure'i Virtuaalmasinates pakub arhitektuur taristu-kui-a-Service (IaaS). Virtuaalmasinates loomist ja konfigureerimist need täpselt nagu kohapealse keskkonna, olemasolevaid rakendusi käivitada ilma koodi muudatustega pilveteenuses lihtsamini. IaaS, olete endiselt eest konfigureerimine on virtuaalmasinates, hallata virtuaalmasinates (nt tarkvara installimine ja OS plaastrid) ning architecting kõrge-saadavus taotlus.

Kui vaatate uue integreerimine lahenduste, mis minimeerida oma taristu haldamise vaeva, kasutada BizTalki teenuseid. Kui otsite kiiresti migreerimine oma BizTalki olemasolevaid lahendusi või otsite ja katsetamiseks BizTalki serveri keskkonnas nõudmisel rakendused, kasutage BizTalki serveri kohta on Azure virtuaalse masina.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Mis vahe on BizTalki adapterit teenuseid ja hübriid ühendused
Azure'i BizTalki teenuse BizTalki adapterit teenuse kasutab. BizTalki adapterit teenuse kasutab artikkel kehtib ühenduse loomiseks kohapealse rea Business (LOB) süsteemi. Ühenduse hübriid annab lihtne ja mugav viis ühenduse Azure'i rakendustes, nagu näiteks veebirakenduste funktsiooni Azure'i rakendust Service ja Azure Mobile teenused, kohapealse ressursside.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Mida tähendab "Hübriid ühenduse andmeedastus (GB) ühiku"? Kas see minutid/tund/päev/nädala/kuu eest? Mis juhtub, kui jõudmisel?

Ühiku hind ühenduse hübriid sõltub BizTalki Services edition. Lihtsalt öeldes kulud sõltuvad edastate palju andmeid. Näiteks 10 GB andmeedastuse iga päev maksab vähem kui 100 GB päevas üle. [Hinnad kalkulaator](https://azure.microsoft.com/pricing/calculator/?scenario=full) BizTalki teenuste abil saate määratleda kulud. Tavaliselt piiranguid pole jõustatud iga päev. Kui ületate, tuleb ravimisse maksta määr $ 1 GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Lepingu loomisel BizTalki teenuste miks sillad läheb ühe asemel kaks?

Iga leping hõlmab kahte erinevat sildade: saada pool side silla ja vastuvõtu pool side sild.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Mis juhtub sillad või lepingute kvoodi piirarvu klõpsasin?

Te ei saa kasutada mis tahes uue sillad või looge uus lepinguid. Lisateavet juurutamiseks peate veel kestusühikud BizTalki teenuse skaalal või suurema versiooniks.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Kuidas migratsioon ühe taseme BizTalki teenuste teise?

Tasuta väljaanne ei saa viiakse või "ülespoole" teise taseme, ja ei saa varundada ja taastada teise taseme. Kui teil on vaja teise taseme, luua uue BizTalki teenuse abil uue taseme. Mis tahes esemeid tasuta väljaanne, sh hübriid ühendusi, kasutades loodud vaja uuesti luua uue BizTalki teenus. 

Ülejäänud väljaanded, kasutage varundamine ja taastamine migreerimine oma esemeid ühe taseme teise. Näiteks varundada oma esemeid Standard astme ja seejärel taastamise Premium taseme. [BizTalki teenuste: varundus ja taaste](biztalk-backup-restore.md) toetatud migreerimise teed kirjeldab ja loendeid, mis esemeid varundatakse. Pange tähele, et hübriid ühendused on varundatud. Pärast varundamise ja taastamise abil uue taseme, mida seejärel uuesti hübriid ühendused.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Teenuse kaasatud BizTalki adapterit teenus on? Kuidas tarkvara vastu võtta?

Jah, BizTalki adapterit teenuse artikkel kehtib sisalduvad Azure BizTalki teenuste SDK [alla laadida](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Järgmised sammud

Azure'i portaalis Azure BizTalki teenuste loomiseks avage [BizTalki teenuste: Azure'i portaalis ettevalmistamise](biztalk-provision-services.md). Rakenduste loomise alustamiseks avage [Azure'i BizTalki teenuseid](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Lisaressursid
- [BizTalki Services: Ettevalmistamise Azure'i portaalis](biztalk-provision-services.md)<br/>
- [BizTalki Services: Ettevalmistamise olek diagramm](biztalk-service-state-chart.md)<br/>
- [BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalki teenused: Varundus ja taaste](biztalk-backup-restore.md)<br/>
- [BizTalki teenused: ahendamine](biztalk-throttling-thresholds.md)<br/>
- [BizTalki teenused: Väljaandja nimi ja väljaandja võti](biztalk-issuer-name-issuer-key.md)<br/>
- [Kuidas alustada abil Azure'i BizTalki teenuste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
