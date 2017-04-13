<properties 
    pageTitle="Loomine ja taastamine varukoopia BizTalki teenused | Microsoft Azure'i" 
    description="BizTalki teenused sisaldab varundus ja taaste. Siit saate teada, kuidas luua ja taastamine varukoopia ja määrata, mida saab varundada. MABS WABS" 
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
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-backup-and-restore"></a>BizTalki Services: Varundus ja taaste

Azure'i BizTalki teenused sisaldab varundus ja taaste võimalusi. Selles teemas kirjeldatakse, kuidas varundus ja taaste BizTalki teenuste Azure klassikaline portaalis.

Samuti saate varundada BizTalki teenuseid kasutades [BizTalki teenuste REST API -ga](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [AZURE.NOTE] Hübriidjuurutuse ühendused pole varundatakse, olenemata väljaanne. Peate oma hübriid ühendused uuesti luua.

## <a name="before-you-begin"></a>Enne alustamist

- Varundus ja taaste ei pruugi olla saadaval kõigi. Vt [BizTalki teenuste: väljaanded diagrammi](biztalk-editions-feature-chart.md).

- Azure'i klassikaline portaali kasutamisel saate luua on Demand varundamine või ajastatud varukoopiad. 

- Varunduse sisu saab taastada sama BizTalki teenuse või uue BizTalki teenusesse. Sama nimega BizTalki teenuse taastamiseks tuleb kustutada olemasoleva BizTalki teenuse ja nimi peab olema saadaval. Pärast BizTalki teenuse kustutamiseks võib kuluda rohkem aega, kui otsitakse sama nime, et see oleks kättesaadav. Kui te ei saa oodata sama nime, et see oleks kättesaadav, siis taastamiseks BizTalki uus teenus.

- BizTalki teenuste saab taastada sama edition või uuem versioon edition. Alumises väljaande BizTalki teenuste taastamine, kui varundamine on võetud, ei toetata.

    Näiteks saate taastada varukoopia Basic Edition Premium Edition. Varukoopia Premium Edition abil ei saa taastada Standard Edition.

- Arvude EDI juhtelemendi varundatakse järjepidevuse juhtelemendi arvud. Kui sõnumite töötlemist pärast viimast varundamine, taastamine varukoopia põhjal sisu võib põhjustada dubleeritud arvud.

- Kui partii on aktiivne sõnumeid, töötlemine on paketi **enne** varukoopia. Luua varukoopia (vastavalt vajadusele või ajastatud), talletatakse kunagi sõnumite pakkidena. 

    **Varukoopia võtmisel partii aktiivne sõnumitega need sõnumid on varundatud ja seetõttu lähevad kaotsi.**

- Valikuline: Portaalis teenuste BizTalki peatada mis tahes toimingute.


## <a name="create-a-backup"></a>Varukoopia loomine

Varukoopia saate igal ajal võtta ja täielikult kontrollib teie. Selles jaotises on loetletud toimingud luua varukoopiaid Azure klassikaline portaalis, sh:

[Nõudmisel varundamise kohta](#backupnow)

[Varukoopia plaanimine](#backupschedule)

#### <a name="backupnow"></a>Nõudmisel varundamise kohta
1. Azure'i klassikaline portaalis valige **BizTalki teenused**ja valige BizTalki teenus, mida soovite varundada.
2. Vahekaart **armatuurlaud** , valige lehe allosas **varundada** .
3. Sisestage varukoopia nimi. Sisestage näiteks *myBizTalkService*BU*kuupäeva*.
4. Valige bloobimälu salvestusruumi konto ja varukoopia alustamiseks märke.

Kui varundamine on lõpule jõudnud, mille sisestate varukoopia nimi on loodud salvestusruumi konto. See ümbris sisaldab teie BizTalki teenuse varukoopia konfigureerimine.

#### <a name="backupschedule"></a>Varukoopia plaanimine

1. Azure'i klassikaline portaalis valige **BizTalki teenused**, valige BizTalki teenuse soovite ajastada varukoopia nimi ja valige vahekaart **konfigureerimine** .
2. Määrake **varukoopia olek** **Automaatne**. 
3. Valige **Salvestusruumi konto** varukoopia salvestada, sisestage **sagedus** varukoopiaid, luua ja kaua varukoopiaid (**Säilitamise päevade**):

    ![][AutomaticBU]

    **Märkmete**   
    - **Säilitamise päevade**säilitusperiood peab olema suurem kui varukoopia.
    - Valige **hoida alati vähemalt üks varundamise**, isegi juhul, kui see on viimase säilitusperiood.
    

4. Valige **Salvesta**.


Ajastatud Varundustöö käitamisel, loob see sisestatud salvestusruumi konto lisamine container (varundatud andmete talletamiseks). Ümbris nimi nimeks *BizTalki teenuse nimi – kuupäev-kellaaeg*. 

Kui BizTalki teenuse armatuurlaual **nurjunud** olek:

![Viimane ajastatud varukoopia olek][BackupStatus] 

Link avab halduse teenuste toiming logid tõrkeotsinguks. Vt [BizTalki teenuste: tõrkeotsinguks abil Logi](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Taastamine

Saate taastada varukoopiate Azure klassikaline portaali kaudu või [Taastada BizTalki teenuse REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582). Selles jaotises on loetletud toimingud taastada klassikaline portaalis.

#### <a name="before-restoring-a-backup"></a>Enne varukoopia taastamine

- BizTalki teenuse taastamisel saab sisestada uus jälgimise, arhiivimine ja järelevalve poed.

- Samade andmete EDI käitusaja taastatakse. EDI käitusaja varukoopia salvestab juhtelemendi arvud. Taastatud juhtelemendi arvud on järjestuses alates varukoopia. Kui sõnumite töötlemist pärast viimast varundamine, taastamine varukoopia põhjal sisu võib põhjustada dubleeritud arvud.

#### <a name="restore-a-backup"></a>Varukoopia taastamine

1. Azure'i klassikaline portaalis valige **Uus** > **Rakenduse teenuste** > **BizTalki teenuse** > **taastamine**:

    ![Varukoopia taastamine][Restore]

2. **Varukoopia URL-i**, valige kaustaikooni ja laiendamine Azure salvestusruumi konto, mis talletab BizTalki teenuse konfiguratsiooni varukoopia. Laiendage ümbris ja valige parempoolsel paanil txt-failist vastavate varundada. 
<br/><br/>
Valige käsk **Ava**.

3. **Taastamine BizTalki teenuse** lehel sisestage **BizTalki teenuse nimi** ja kontrollige **Domeeni URL-i**, **väljaande**ja **piirkond** taastatud BizTalki teenuse kohta. **Loo uus eksemplar SQL-i andmebaasi** jälgimise andmebaasi:

    ![][RestoreBizTalkService]

    Valige järgmine noolt.

4.  Kontrollige SQL-andmebaasi nime, sisestage selle serveri füüsilise kus luuakse SQL-andmebaasi server ja kasutajanime ja parooli.


    Kui soovite konfigureerida SQL-andmebaasi väljaande, suurust ja muid atribuute, valige **Täpsem andmebaasi sätete konfigureerimine**. 

    Valige järgmine noolt.

5. Mäluruumi uue konto loomine või olemasoleva salvestusruumi konto sisestada BizTalki teenuse.

7. Valige Taasta alustamiseks märke.

Kui taastamine on edukalt lõpule jõudnud, BizTalki uus teenus on loetletud peatatud olekus BizTalki teenuste lehel Azure'i klassikaline portaalis.



### <a name="postrestore"></a>Pärast varukoopia taastamine

BizTalki teenus on taastatud alati **Suspended** olekus. Olekus on saate konfiguratsiooni muudatusi teha enne uue keskkond on otstarbekas, sh:

- Kui olete loonud BizTalki teenuserakenduste Azure'i BizTalki teenuste SDK abil, peate abil värskendada juurdepääsu juhtimine (ACS) mandaadi nendes rakendustes taastatud keskkonnas töötamiseks.

- Saate taastada BizTalki teenus, mis kopeerida olemasoleva BizTalki teenuse keskkonnas. Sellisel juhul, kui on konfigureeritud algse BizTalki teenuste portaalis lepinguid, kasutavate lähtekaust FTP, võib-olla peate värskendama lepingute äsja taastatud keskkonnas kasutada erinevaid lähtekaust FTP. Muul juhul võib olla kaks eri lepingute üritab tõmmata sama sõnumit.

- Kui taastamine on mitu BizTalki teenuse keskkonnas, veenduge, et saate suunata õige keskkonna Visual Studio rakendusi, PowerShelli cmdlet-käskude või REST API-de börsipäev partnerite halduse OM API-d.

- See on hea tava konfigureerimiseks automatiseeritud varukoopiate äsja taastatud BizTalki teenuse keskkonnas.

Azure'i klassikaline portaalis BizTalki teenuse käivitamiseks valige teenus taastatud BizTalki ja valige **elulookirjelduse** tegumiriba kaudu. 



## <a name="what-gets-backed-up"></a>Mida saab varundada

Varukoopia loomisel varundatakse järgmised üksused:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Varundatud üksused</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure'i BizTalki Services portaal</strong></td>
</tr> 
<tr>
<td>Konfiguratsiooni ja käitusaja</td> 
<td>
<ul>
<li>Partneri ja profiili üksikasjad</li>
<li>Partneri lepingud</li>
<li>Kohandatud assemblereid juurutatud</li>
<li>Sillad juurutatud</li>
<li>Serdid</li>
<li>Teisendusteta juurutatud</li>
<li>Torujuhtmete</li>
<li>Mallid, mis on loodud ja salvestatud BizTalki Services portaal</li>
<li>X12 ST01 ja GS01 vastendused</li>
<li>Juhtelemendi arvud (EDI)</li>
<li>AS2 sõnumi mikrofoni väärtused</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure'i BizTalki teenus</strong></td>
</tr> 
<tr>
<td>SSL-sert</td> 
<td>
<ul>
<li>SSL-i serdi andmed</li>
<li>SSL-i serdi parooli</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalki Teenusesätted</td> 
<td>
<ul>
<li>Skaala ühiku arv</li>
<li>Väljaande</li>
<li>Toote versioon</li>
<li>Piirkonna/andmekeskusega</li>
<li>Access Control teenus (ACS) nimeruum ja võti</li>
<li>Andmebaasi ühendusstringi jälgimine</li>
<li>Salvestusruumi konto ühendusstringi arhiivimine</li>
<li>Salvestusruumi konto ühendusstringi jälgimine</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Täiendavate üksuste</strong></td>
</tr> 
<tr>
<td>Andmebaasi jälgimine</td> 
<td>Kui BizTalki teenus on loodud, jälgimise andmebaasi andmed on sisestatud, sh Azure SQL-andmebaasi Server ja jälitamise andmebaasi nimi. Jälitamise andmebaasi ei automaatselt varundada.
<br/><br/>
<strong>Oluliste</strong><br/>
Kui jälituse andmebaas on kustutatud ja taastatud andmebaasi peab eelmise varukoopia olemas. Kui varukoopia olemas, andmebaasi jälgimine ja selle andmete ei ole Taastatavad. Sellisel juhul andmebaasi nimi uue andmebaasi jälgimine loomine. Soovitatav on Geo-Dispersioonanalüüs.</td>
</tr> 
</table>

## <a name="next"></a>Järgmise

Azure'i klassikaline portaalis Azure BizTalki teenuste loomiseks avage [BizTalki teenuste: ettevalmistamise abil Azure klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=302280). Rakenduste loomise alustamiseks avage [Azure'i BizTalki teenuseid](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Vt ka
- [Varukoopia BizTalki teenus](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [BizTalki teenuse varukoopia põhjal taastamine](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalki Services: Arendaja, Basic, Standard- ja Premiumi väljaannetes diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalki Services: Ettevalmistamise abil Azure'i klassikaline portaal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalki Services: Ettevalmistamise olek diagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalki teenuste: ahendamine](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalki Services: Väljaandja nimi ja väljaandja võti](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Kuidas alustada abil Azure'i BizTalki teenuste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
