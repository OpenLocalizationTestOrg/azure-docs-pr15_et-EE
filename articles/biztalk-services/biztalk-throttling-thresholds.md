<properties 
    pageTitle="Lisateavet BizTalki teenuste pidurdamise | Microsoft Azure'i" 
    description="Teavet pidurdamise lävede ja tulemuseks käitusaja käitumist BizTalki teenuste jaoks. Pidurdamise põhineb mälukasutuse ja teadete arv. MABS WABS" 
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





# <a name="biztalk-services-throttling"></a>BizTalki teenuste: ahendamine

Azure'i BizTalki teenuste rakendab teenuse ahendamine kahe tingimuse alusel: mälukasutuse ja samaaegne sõnumite töötlemine. Selles teemas loetletud ahendamise lävede ja kirjeldatakse käitusaja käitumine ahendamise tingimuse ilmnemisel.

## <a name="throttling-thresholds"></a>Pidurdamise piirmäärad

Järgmises tabelis on loetletud ahendamise lähte-kui ka piirmäärad:

||Kirjeldus|Madal lävi|Kõrge lävi|
|---|---|---|---|
|Mälu|kogu süsteemi mälu saadaval/PageFileBytes %. <p><p>Kokku saadaval PageFileBytes on umbes 2 korda RAM süsteem.|60%|70%|
|Sõnumi töötlemine|Sõnumite töötlemine korraga|40 * valdkond arv|100 * valdkond arv|

Väga jõudmisel Azure BizTalki teenuste hakkab throttle. Pidurdamise astmed madal lävi jõudmisel. Näiteks kasutab teenust 65% süsteemi mälu. Sellisel juhul teenuse throttle. Teenust käivitatakse 70% muutmälu abil. Sellisel juhul teenuse ahendab ja jätkub kuni teenust kasutab 60% (madal lävi) muutmälu throttle.

Azure'i BizTalki teenuste lugusid ahendamise olek (tavaline riik vs rakendus osariik) ja ahendamise kestus.


## <a name="runtime-behavior"></a>Käitusaja käitumine

Azure'i BizTalki teenuste viimisel ahendamise olekus ilmneb järgmine:

- Pidurdamise on rolli eksemplari. Näiteks:<br/>
RoleInstanceA on pidurdamise. RoleInstanceB on pole pidurdamise. Sellisel juhul RoleInstanceB sõnumite töötlemist ootuspäraselt. Sõnumite RoleInstanceA hüljatakse ja nurjuda järgmise tõrkega.<br/><br/>
**Server on hõivatud. Proovige uuesti.**<br/><br/>
- Mis tahes pull allikad küsitlus või sõnumi alla laadida. Näiteks:<br/>
Müügivõimaluste tõmbab sõnumite FTP välise andmeallikaga. Roll eksemplari, tehes tõmme saab ahendamise olekusse. Sellisel juhul lõpetab tulemas täiendavad meilisõnumeid alla laadima, kuni rolli eksemplari peatub pidurdamise.
- Vastus saadetakse kliendi nii, et kliendi saab uuesti sõnumi.
- Oodake, kuni ahendamine on lahendatud. Täpsemalt, peate ootama kuni madal lävi.

## <a name="important-notes"></a>Olulised märkused
- Ahendamine ei saa keelata.
- Ahendamine lävede ei saa muuta.
- Ahendamine rakendatakse kogu süsteemi.
- Azure'i SQL-andmebaasi Server on ka sisseehitatud ahendamine.

## <a name="additional-azure-biztalk-services-topics"></a>Täiendavad Azure BizTalki teenuste Teemad

-  [Azure'i BizTalki teenuste SDK installimine](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Õpetus: Azure'i BizTalki teenused](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Kuidas alustada abil Azure'i BizTalki teenuste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure'i BizTalki teenused](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Vt ka
- [BizTalki Services: Arendaja, Basic, Standard- ja Premiumi väljaannetes diagrammi](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalki Services: Ettevalmistamise abil Azure'i klassikaline portaal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalki Services: Ettevalmistamise olek diagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalki Services: Varundus ja taaste](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalki Services: Väljaandja nimi ja väljaandja võti](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
