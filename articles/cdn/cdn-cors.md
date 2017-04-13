<properties
    pageTitle="Azure'i CDN funktsiooniga CORS | Microsoft Azure'i"
    description="Siit saate teada, kuidas kasutada funktsiooni Azure'i sisu kohaletoimetamise võrk (CDN) abil koos Cross-päritolu ressursside ühiskasutus (CORS)."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Azure'i CDN CORS kasutamine     

## <a name="what-is-cors"></a>Mis on CORS?

CORS (Cross Origin ressursside ühiskasutus) on HTTP funktsiooni, mis lubab juurdepääsu ressursside lisadomeeni töötab ühe domeeni veebirakenduse. Saitidevaheline skriptimine eest võimalus vähendamiseks kõik tänapäevane veebibrauserid rakendada [sama päritolu poliitika](http://www.w3.org/Security/wiki/Same_Origin_Policy)tuntud turvalisus piirang.  See takistab veebilehe helistaja API-de domeenil.  CORS pakub turvaliselt lubamiseks helistada API-de lisadomeeni ühe domeeni (origin Domeen).
 
## <a name="how-it-works"></a>Kuidas see toimib
1.  Brauseri saadab mõne **Origin** HTTP päisega taotluse SUVANDID. See päis väärtus serveeritud vanem leht domeeni. Kui lehte https://www.contoso.com proovib juurde pääseda kasutaja andmete fabrikam.com'i domeen, saadetakse järgmised taotluse header fabrikam.com: 
    
    `Origin: https://www.contoso.com`
 
2.  Server võib vastata järgmist:
    - **Accessi-juhtelemendi lubamine-Origin** päise vastuses, mis näitab, milliseid saite origin on lubatud. Näiteks:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Kui server ei luba cross päritolu taotluse tõrkeleht
    - **Accessi-juhtelemendi lubamine-Origin** päise metamärgiga, mis lubab kõigi domeenide abil:
        
        `Access-Control-Allow-Origin: *`
 
Keerukate HTTP-päringud, on valmis esimene "eelkontrolli" taotluse kindlaks, kas see on õigus enne kogu kutse saatmist.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Metamärkide või ühe origin stsenaariumid

Klõpsake Azure CDN CORS töötab automaatselt täiendavaid konfiguratsiooni kui **Access-juhtelemendi lubamine-Origin** päis on seatud metamärgiga (*) või ühe origin.  Funktsiooni CDN vahemälu esimese vastus ja edaspidised taotlused kasutatakse sama päis.
 
Kui taotlused on juba tehtud CDN on pannud CORS enne selle oma origin on vaja likvideerite sisule **Juurdepääsu-juhtelemendi lubamine-Origin** päise sisu uuesti laadimiseks lõpp-punkti sisu.
 
## <a name="multiple-origin-scenarios"></a>Mitme origin stsenaariumid

Kui peate esmalt lubama päritolu tuleb lubada CORS konkreetsesse nimekirja, asjad, mida saate natuke keerulisem. Probleem ilmneb siis, kui on CDN vahemälu **Accessi-juhtelemendi lubamine-Origin** esimene CORS origin päis.  Kui muu CORS origin edaspidised taotleb, on CDN on serveeritud vahemällu talletatud **Accessi-juhtelemendi lubamine-Origin** päisest, mis ei vasta.  On probleemi lahendamiseks mitu võimalust.
 
### <a name="azure-cdn-premium-from-verizon"></a>Azure'i CDN Premium Verizon

Parim viis selle on **Azure CDN Premium Verizon**, mis seab teatud täpsemate funktsioonide kasutamiseks. 
 
Peate [reegli](cdn-rules-engine.md) loomiseks märkige ruut **Origin** päise kutse.  Kui see on lubatud origin, loob reegli **Accessi-juhtelemendi lubamine-Origin** päises esitatud päritolu.  Kui määratud päises **Origin** päritolu pole lubatud, tuleks reegli argument **Accessi-juhtelemendi lubamine-Origin** päis, mis põhjustavad brauseri taotluse tagasilükkamiseks. 
 
On reeglid engine selleks kaks võimalust.  Mõlemal juhul **Juurdepääsu-juhtelemendi lubamine-Origin** päise faili origin serverist ignoreeritakse täielikult, on CDN reeglid engine täielikult haldab lubatud CORS päritolu.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Ühe tavalise avaldise kõik lubatud päritolu
 
Sel juhul saate luua regulaarne avaldis, mis sisaldab kõiki päritolu, mida soovite lubada: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **Azure'i CDN Verizon** kasutab [Perl ühilduvad regulaaravaldised](http://pcre.org/) mootori regulaaravaldised jaoks.  Saate tööriista nagu [tavalise avaldiste 101](https://regex101.com/) tavaline avaldise kinnitamiseks.  Pange tähele, et märgi "/" kehtib regulaaravaldised ja ei vaja, tuleb seda vältida, kuid see märk pääseks eelkõige soovitatakse ja mõned regex Lahendusevalidaatorite peaks.

Kui Lihtavaldise vastab, reegli asendab **Access-juhtelemendi lubamine-Origin** päise (vajadusel) päritolu koos origin, mis saadetakse kutse.  Samuti saate lisada täiendavad CORS päised, näiteks **Accessi-juhtelemendi lubamine-meetodid**.

![Lihtavaldise reeglid näide](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Taotluse päise reegel iga origin.

Selle asemel, et regulaaravaldised, saate luua hoopis eraldi reegli päritolu jaoks lubate [vastavad tingimusele](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1) **Taotleda päise metamärkide** abil. Lihtavaldise meetodit, sarnaselt määrab ainult reeglite mootori CORS päised. 
  
![Lihtavaldise ilma reeglite näide](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] Ülaloleval, kasutada metamärki * ütleb vastavaks HTTP ja HTTPS-i mootori reeglid.
 
### <a name="azure-cdn-standard"></a>Azure'i CDN Standard

Azure'i CDN Standard profiilid ainult süsteemi lubamiseks mitme päritolu ilma metamärkide origin on kasutada [päringu stringi vahemällu](cdn-query-string.md).  Peate päringu stringi sätte CDN lõpp-punkti ja seejärel kasutage kordumatuid päringustringi taotluste iga lubatud domeeni jaoks. Selle tulemusena CDN-ID vahemällu eraldi objekti jaoks kordumatu päringustringi iga. Seda moodust pole siiski optimaalne, nagu on tulemuseks mitme eksemplari vahemällu on CDN sama faili.  

