<properties 
    pageTitle="Väljaandja nimi ja väljaandja võti BizTalki teenused | Microsoft Azure'i" 
    description="Saate teada, kuidas tuua väljaandja nimi ja väljaandja võti teenuse siini või Access Control (ACS BizTalki teenused). MABS WABS" 
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




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalki Services: Väljaandja nimi ja väljaandja võti

Azure'i BizTalki Services kasutab teenuse siini väljaandja nimi ja väljaandja võti, ja juurdepääsu juhtimine väljaandja nimi ja väljaandja võti. Täpsemalt:

Ülesanne | Millised väljaandja nimi ja väljaandja võti
--- | ---
Rakenduse Visual Studio juurutamine | Juurdepääsu juhtimine väljaandja nimi ja väljaandja võti
Azure'i BizTalki Services portaal konfigureerimine | Juurdepääsu juhtimine väljaandja nimi ja väljaandja võti
Visual Studio teenustega BizTalki adapterit LOB releed loomine | Teenuse siini väljaandja nimi ja väljaandja võti

Selles teemas on loetletud toimingud tuua väljaandja nimi ja väljaandja võti. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Juurdepääsu juhtimine väljaandja nimi ja väljaandja võti
Juurdepääsu juhtimine väljaandja nimi ja väljaandja võti kasutatakse järgmist:

- Azure'i BizTalki teenuse rakenduse Visual Studios loodud: Azure'i edukalt juurutada BizTalki teenuserakenduse Visual Studios, sisestate juurdepääsu juhtimine väljaandja nimi ja väljaandja võti. 
- Azure'i BizTalki Services portaal: Kui loote BizTalki teenuse ja avage BizTalki Services portaal, oma Accessi juhtelemendi väljaandja nimi ja väljaandja võtme automaatselt registreeritakse teie juurutuste juurdepääsu reguleerimine sama väärtusega.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Kopeerimine ja kleepimine juurdepääsu juhtimine väljaandja nimi ja väljaandja võti

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Valige vasakpoolsel navigeerimispaanil **BizTalki teenuseid**.
3. Valige oma BizTalki teenus. 
4. Valige **Ühenduseteavet** tegumiriba kaudu. Juurdepääsu juhtimine Namespace, vaikimisi väljaandja (väljaandja nimi) ja vaikimisi võti (väljaandja Key) on loetletud ja saate kopeerida ja kleepida.  

Kokkuvõte  
Väljaandja nimi = vaikimisi väljaandja  
Väljaandja klahv = vaikimisi võti


Samuti võite märkida ruudu **Avatud ACS haldusportaali** , et saada juurdepääsu reguleerimine väärtused:

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Valige vasakpoolsel navigeerimispaanil **BizTalki teenuseid**.
3. Valige oma BizTalki teenus.
4. Valige **Avatud ACS haldusportaali**ühenduse teave nuppu.
5. Valige jaotises **Teenusesätted**portaalis **Teenuse identiteedid**. Kuvatakse teie teenuse identiteedi, mis on teie juurdepääsu juhtimine väljaandja nimi väärtus. Valige oma veebiteenuse linki, et näha parooli, mis on teie väljaandja võti väärtus. Nende väärtuste saab kopeerida.<br/><br/>
Näiteks **Teenuse identiteedid**, näete "omanik". "Omanik" on teie juurdepääsu juhtimine väljaandja nimi. "Omanik" lingi klõpsamisel kuvatakse **parool**. "Parool" lingi klõpsamisel kuvatakse väärtus. Parooli väärtus teie juurdepääsu juhtimine väljaandja võti.  

Kokkuvõte  
Väljaandja nimi = teenuse isiku nimi  
Väljaandja klahv = parooli väärtus

Klõpsake vasakpoolsel navigeerimispaanil, võite valida ka **Active Directory** juurdepääsu reguleerimine väärtuste toomiseks. 

> [AZURE.IMPORTANT]**Kui ka Accessi juhtelemendi Namespace on loodud **Active Directory**, teenuse identiteedi abil luuakse automaatselt.** Kui olete ette BizTalki teenuse Access juhtelemendi Namespace, veebiteenuse nimega "omanik" (väljaandja nimi), parooli (väljaandja Key), ja sümmeetriline võti luuakse automaatselt.<br /> 
[Kohta: kasutamine ACS halduse teenuse konfigureerimine teenuse identiteedid](http://go.microsoft.com/fwlink/p/?LinkID=303942) leiate teavet selle kohta juurdepääsu juhtimine teenuse identiteedid.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Teenuse siini väljaandja nimi ja väljaandja võti
Teenuse siini väljaandja nimi ja väljaandja klahvi kasutatakse BizTalki adapterit teenused. Visual Studio BizTalki teenuste projektis kasutate teenust BizTalki adapterit ühenduse loomiseks kohapealse joon-, äri (LOB) süsteemiga. Ühenduse, luua LOB edastamine ja sisestage oma LOB süsteemi andmed. Seda tehes sisestate ka teenuse siini väljaandja nimi ja väljaandja võti.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Tuua teenuse siini väljaandja nimi ja väljaandja võti

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Valige vasakpoolsel navigeerimispaanil **Teenuse siini**.
3. Valige oma nimeruum. Valige tööülesande **Ühenduseteavet**. Kuvatakse **Vaikimisi väljaandja** (väljaandja nimi) ja **Vaikimisi võti** (väljaandja võti). Nende väärtuste saab kopeerida.  

Kokkuvõte  
Väljaandja nimi = vaikimisi väljaandja  
Väljaandja klahv = vaikimisi võti

## <a name="next"></a>Järgmise
Täiendavad Azure BizTalki teenuste Teemad:

-  [Azure'i BizTalki teenuste SDK installimine](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Õpetus: Azure'i BizTalki teenused](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Kuidas alustada abil Azure'i BizTalki teenuste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure'i BizTalki teenused](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Vt ka
-  [Kuidas: ACS halduse teenuse abil saate konfigureerida teenuse identiteedid](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalki Services: Arendaja, Basic, Standard- ja Premiumi väljaannetes diagrammi](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalki Services: Ettevalmistamise abil Azure'i klassikaline portaal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalki Services: Ettevalmistamise olek diagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalki Services: Varundus ja taaste](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalki teenuste: ahendamine](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
