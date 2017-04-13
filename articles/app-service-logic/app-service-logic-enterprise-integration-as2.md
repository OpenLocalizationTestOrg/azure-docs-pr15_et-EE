<properties 
    pageTitle="Saate teada, kuidas luua AS2 lepingu Enterprise integreerimine pakett" 
    description="Saate teada, kuidas luua AS2 lepingu Enterprise integreerimine Pack | Microsoft Azure'i rakendust Service" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>Ettevõtte AS2 integreerimine

## <a name="create-an-as2-agreement"></a>Lepingu AS2 loomine
Loogika rakendustes enterprise funktsioonide kasutamiseks peate esmalt looma lepingud. 

### <a name="heres-what-you-need-before-you-get-started"></a>Siin on, mida peate enne alustamist
- Määratletud Azure tellimuse [konto](./app-service-logic-enterprise-integration-accounts.md)  
- Teie konto juba määratletud vähemalt kaks [partnerite](./app-service-logic-enterprise-integration-partners.md)  

>[AZURE.NOTE]Kui loote leping, lepingu faili sisu peab vastama lepingu tüübist.    


Kui olete [loonud integreerimine kontot](./app-service-logic-enterprise-integration-accounts.md) ja [lisatud partnerite](./app-service-logic-enterprise-integration-partners.md), saate luua lepingu järgmiste juhiste järgi:  

### <a name="from-the-azure-portal-home-page"></a>Azure'i videoportaali avalehel

Pärast sisselogimist [Azure portaali](http://portal.azure.com "Azure portaali"):  
1. Valige vasakul menüüst käsk **Sirvi** .  

>[AZURE.TIP]Kui te ei näe linki **Sirvi** , peate esmalt Menüü laiendamiseks. Selleks valige **menüü kuvamine** link, mis asub aadressil ülemises vasakus ahendatud menüü.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Tippige otsinguväljale filter *integreerimine* , seejärel valige tulemiloendis **Integreerimine kontod** .       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Valige **Integreerimine kontod** labale avanenud integreerimine konto, mille loote lepingu. Kui te ei näe, mis tahes integreerimine kontod loendid, [loomiseks esimese](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Valige paan **lepingud** . Kui te ei näe paani lepingud, lisage see esmalt.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. Valige nupp **Lisa** lepingute tera, mis avab.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Sisestage oma lepingu **nimi** , seejärel valige lepingud tera, mis avab **Host partneri**, **Host identiteedi**, **Külastajate Partner**, **Külastajate identiteedi**.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Siin on mõned üksikasjad kasulik on teie lepingu sätete konfigureerimisel. 
  
|Atribuut|Kirjeldus|
|----|----|
|Host partneri|Lepingu peab nii host ja Külastajate partner. Host partneri tähistab ettevõte, mis konfigureerib lepingu.|
|Host identiteet|Identifikaatori host partner. |
|Külalisena partneri|Lepingu peab nii host ja Külastajate partner. Külalisena partneri tähistab ettevõte, mis on host partneri ärisuhetes.|
|Külalisena identiteet|Identifikaatori Külastajate partner.|
|Sätete vastuvõtmine|Neid atribuute rakendada kõikidele sõnumitele, mis on saadud leping|
|Saada sätted|Neid atribuute rakendada kõikidele sõnumitele, mis on saadetud leping|  
Vaatame jätkata:  
7. Valige **Vastuvõtmine sätete** konfigureerimiseks saabuvaid sõnumeid vastu võetud rakenduses käesolevat lepingut viisi.  
 
 - Soovi korral saate alistada sissetuleva sõnumi atribuudid. Selle tegemiseks, märkige ruut **Alista sõnumi atribuudid** .
  - Kui soovite nõuda kõigi sissetulevate sõnumite allkirjastamist, märkige ruut **sõnumi lepingut** . Kui valite selle suvandi, peate samuti valige **sert** , mida kasutatakse sõnumite allkirja valideerimiseks.
  - Soovi korral saate nõuda sõnumeid krüptida ka. Selle tegemiseks, märkige ruut **peaksid olema krüptitud sõnumit** . Seejärel peate valige **sert** , mida kasutatakse dekodeerida sissetulevad sõnumid.
  - Saate nõuda, sõnumid, kui soovite tihendada. Selle tegemiseks, märkige ruut **sõnumi peaks tihendatud** .  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Vt alltoodud tabelit, kui soovite lisateavet selle kohta, mis on vastuvõtu sätted lubamine.  

|Atribuut|Kirjeldus|
|----|----|
|Sõnumi atribuudid alistamine|Valige see näitab, et saadud sõnumite atribuudid saate alistada |
|Sõnumi lepingut|Luba see olema digitaalselt allkirjastatud sõnumite nõudma|
|Peaksid olema krüptitud sõnumi|Luba see nõudma sõnumeid krüptida. Mitte-krüptitud sõnumite on tagasi lükatud.|
|Sõnumi peaks tihendatud|Luba see nõudma sõnumid tihendada. Mitte tihendatud sõnumid on tagasi lükatud.|
|LAHTI tekst|See on vaikimisi lahti, mis saadetakse sõnumi saatja|
|Saada lahti|Luba see, kui soovite lubada hallatavate andmevõrguteenuste saata.|
|Saada allkirjastatud lahti|Luba see hallatavate andmevõrguteenuste allkirjastamist nõudmine.|
|Mikrofoni algoritmi||
|Asünkroonne lahti saatmine|Luba see asünkroonselt saadetavate sõnumite nõudma.|
|URL-I|See on sõnumid saadetakse URL-i.|
Nüüd vaatame jätkata:  
8. Valige **Saada sätete** konfigureerimiseks sõnumite kustutamine käesolevat lepingut käsitleda viisi.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Vt alltoodud tabelit, kui soovite lisateavet selle kohta, milliseid saada sätete lubamine.  

|Atribuut|Kirjeldus|
|----|----|
|Sõnumi sisselogimise lubamine|Märkige see ruut Luba kõik lepingu allkirjastamist saadetud sõnumid.|
|Mikrofoni algoritmi|Valige algoritmi sõnumi sisselogimise kasutamine|
|Sert|Valige sert sõnumi sisselogimise kasutamine|
|Sõnumi krüptimine|Märkige see ruut Krüpti kõigi sõnumite saadetud käesolevat lepingut.|
|Krüptimisalgoritmi|Valige krüptimisalgoritmi sõnumikrüptimine kasutamine|
|Paljastama HTTP päised|Märkige see ruut paljastama ühe rea päise HTTP-sisutüüp.|
|Taotluse lahti|See ruut Taotle jaoks kõik selle lepingu saadetud sõnumid on lahti lubamine|
|Kirjutab lahti|Luba, et kõik hallatavate andmevõrguteenuste saadetud käesolevat lepingut logitud taotlemine|
|Asünkroonne lahti taotlemine|Luba asünkroonne lahti, mis saadetakse käesolevat lepingut taotleda|
|URL-I|URL-i, millele saadetakse hallatavate andmevõrguteenuste|
|NRR lubamine|Märkige see ruut Luba mitte lepingu rikkumine saamise|
Me peaaegu valmis!  
9. Valige konto enne **lepingute** paan ja te näete loetletud äsja lisatud lepingu.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

