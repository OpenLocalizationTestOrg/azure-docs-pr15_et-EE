<properties 
    pageTitle="Ettevõtte integreerimine EDIFACT | Microsoft Azure'i" 
    description="Saate teada, kuidas luua loogika rakenduste EDIFACT lepingute abil" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>Ettevõtte EDIFACT integreerimine 

> [AZURE.NOTE] Sellel lehel antakse ülevaade EDIFACT funktsioonid loogika rakendused. Klõpsake X12 saamiseks klõpsake [siin](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>EDIFACT lepingu loomine 
Saate vahetada EDIFACT sõnumeid, peate EDIFACT lepingu luua ja salvestada selle konto integreerimine. Järgmised toimingud juhendab teid EDIFACT lepingu loomise protsess.

### <a name="heres-what-you-need-before-you-get-started"></a>Siin on, mida peate enne alustamist
- Määratletud Azure tellimuse [konto](./app-service-logic-enterprise-integration-accounts.md)  
- Teie konto juba määratletud vähemalt kaks [partnerite](./app-service-logic-enterprise-integration-partners.md)  

>[AZURE.NOTE]Lepingu loomisel sõnumeid te kuvatakse/saatmine ja partneri sisu peab vastama lepingu tüübist.    


Kui olete [loonud integreerimine kontot](./app-service-logic-enterprise-integration-accounts.md) ja [lisatud partnerite](./app-service-logic-enterprise-integration-partners.md), saate luua EDIFACT lepingu järgmiste juhiste järgi:  

### <a name="from-the-azure-portal-home-page"></a>Azure'i videoportaali avalehel

Pärast sisselogimist [Azure portaali](http://portal.azure.com "Azure portaali"):  
1. Valige vasakul menüüst käsk **Sirvi** .  

>[AZURE.TIP]Kui te ei näe linki **Sirvi** , peate esmalt Menüü laiendamiseks. Selleks valige **menüü kuvamine** link, mis asub aadressil ülemises vasakus ahendatud menüü.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. Tippige otsinguväljale filter *integreerimine* , seejärel valige tulemiloendis **Integreerimine kontod** .       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. Valige **Integreerimine kontod** labale avanenud integreerimine konto, mille loote lepingu. Kui te ei näe, mis tahes integreerimine kontod loendid, [loomiseks esimese](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Valige paan **lepingud** . Kui te ei näe paani lepingud, lisage see esmalt.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. Valige nupp **Lisa** lepingute tera, mis avab.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Sisestage oma lepingu **nimi** , seejärel valige **Tüüp** EDIFACT **Host partneri**, **Host identiteedi**, **Külastajate partneri**, **Külastajate identiteedi**lepingute tera, mis avab.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Pärast lepingu atribuutide määramiseks valige **Vastuvõtmine sätete** konfigureerimiseks saabuvaid sõnumeid vastu võetud rakenduses käesolevat lepingut viisi.  
8. Järgmistes jaotistes, sh identifikaatorite, kinnitus, skeemid, juhtelemendi numbrid, valideerimine, sisemine sätted ja Pakktöötlus on jagatud vastu sätted juhtimine. Konfigureerige atribuutidest oma lepingu partneri teil on vahetamine sõnumid, mille põhjal. Siit leiate ülevaate neid juhtelemente, nende põhjal, kuidas soovite käesolevat lepingut tuvastamiseks ja käsitlemiseks sissetulevate sõnumite konfigureerimine.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Sätete salvestamiseks nuppu **OK** .  

### <a name="identifiers"></a>Identifikaatorid

|Atribuut|Kirjeldus |
|---|---|
|UNB6.1 (adressaatide viide parool)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vahemikus 1 kuni 14 märki.|
|UNB6.2 (adressaatide viide kvalifikatsioon)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vähemalt üks märk ja kuni kaks märki.|

### <a name="acknowledgments"></a>Litsents 

|Atribuut|Kirjeldus |
|----|----|
|Saamist sõnum (CONTRL)|Märkige see ruut tehnilise (CONTRL) kinnitus naasmiseks andmevahetuse saatja. Andmevahetuse saatja alusel lepingu saatmine sätete saadetakse kinnitus.|
|Kinnituse (CONTRL)|Valige see märkeruut, et tagastada otstarbekas (CONTRL) kinnitus andmevahetuse saatjale kinnitust saadetakse andmevahetuse saatja lepingu saatmine sätete põhjal.|

### <a name="schemas"></a>Skeemid

|Atribuut|Kirjeldus |
|----|----|
|UNH2.1 (TÜÜP)|Valige tehingu määramine tüüp.|
|UNH2.2 (VERSIOON)|Sisestage sõnumi versiooninumber. (Miinimum, ühe märgi, kuni kolm märki).|
|UNH2.3 (VÄLJAANNE)|Sisestage sõnum väljaanne number. (Miinimum, ühe märgi, kuni kolm märki).|
|UNH2.5 (SEOTUD MÄÄRATUD KOOD)|Sisestage määratud kood. (Kuni kuus märki. Peab olema tähtedest ja numbritest koosnev).|
|UNG2.1 (RAKENDUSE SAATJA ID)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vähemalt üks märk ja kuni 35 märki.|
|UNG2.2 (RAKENDUSE SAATJA KOOD KVALIFIKATSIOON)|Sisestage tähtedest ja numbritest koosnev väärtus, kuni neli märki.|
|SKEEMI|Valige varem üleslaaditud skeemi, mida soovite kasutada oma seotud konto.|

### <a name="control-numbers"></a>Juhtelemendi arvud

|Atribuut|Kirjeldus |
|----|----|
|Keela Interchange juhtelemendi arvu duplikaadid|Märkige see ruut Blokeeri dubleeritud tehnosiirdeid. Kui valitud, kontrollib EDIFACT dekodeerida toimingu andmevahetuse juhtelemendi number (UNB5) saadud andmevahetuse ei vasta varem töödeldud andmevahetuse juhtelemendi arvu. Kui tuvastatakse vaste, siis on andmevahetuse ei töödelda.
|Otsida dubleeritud UNB5 iga (päeva)|Kui teil on liitunud keelamiseks dubleeritud andmevahetuse juhtelemendi arve, saate määrata kus toimub andes sobiv väärtus **otsida dubleeritud UNB5 iga (päeva)** suvand kontrolli päevade arv.|
|Rühma juhtelement arv duplikaatide keelamiseks|Märkige see ruut Blokeeri tehnosiirdeid dubleeritud jaotises juhtelemendi arvudega (UNG5).|
|Tehingu määramine juhtelemendi arv duplikaatide keelamiseks|Märkige see ruut Blokeeri tehnosiirdeid määramine juhtelemendi arvudega dubleeritud tehingu (UNH1).|
|EDIFACT kinnituse juhtelemendi arv|Tehingu määramine viidete kasutatavad kinnitus määramiseks sisestage väärtus eesliite, viide arve sisaldav vahemik ja järelliide.|

### <a name="validations"></a>Valideerimised

|Atribuut|Kirjeldus |
|----|----|
|Sõnumi tüüp|Määrake sõnumi tüüp. Iga rea valideerimine lõpetatuks teise lisatakse automaatselt. Kui reegleid ei on määratud, siis rida tähisega Vaikimisi kasutatakse valideerimine.|
|EDI valideerimine|Märkige see ruut, kui sooritada EDI valideerimine EDI atribuutide skeemi, pikkus piirangud, tühja andmeid ja lõpunullid eraldajad määratletud andmetüübid.|
|Laiendatud valideerimine|Märkige see ruut, et lubada laiendatud (XSD) valideerimine tehnosiirdeid saadud andmevahetuse saatja. See hõlmab valideerimine välja pikkus, vaba, ja lisaks XSD andmevalideerimise tüüp korduste arv.|
|Luba viib/lõputühikud nullidega|Valige **Luba** , et lubada viib/lõpunullid; **NotAllowed** ei luba viib/lõputühikud null või juhtiva trimmimise **trimmimine** ja lõputühikud nullid.|
|Viib/lõputühikud nullidega trimmimine|Märkige see ruut, mis tahes või lõputühikute nullidega trimmimine|
|Eraldajaga lõpust poliitika|Valige **Pole lubatud** , kui te soovite lubada lõpunullid eraldajaid ja eraldajad teabevahetus saadud andmevahetuse saatja. Kui andmevahetuse sisaldab lõpunullid eraldajaid ja eraldajad, see kehtetuks. Valige **Valikuline** aktsepteerimiseks tehnosiirdeid koos või ilma lõpunullid eraldajaid ja eraldajad. Valige **kohustuslik** , kui vastuvõetud andmevahetuse peab sisaldama lõpunullid eraldajaid ja eraldajad.|

### <a name="internal-settings"></a>Sisemise sätted

|Atribuut|Kirjeldus |
|----|----|
|Kui lõpunullid eraldajad on lubatud, looge tühi XML-sildid|Märkige see ruut olema andmevahetuse saatja lisada tühja XML-i sildid lõpunullid eraldajad.|
|Sissetulev partiide töötlemine|Järgmised suvandid.</br></br>**Kui tehingu komplekti - tükeldatud andmevahetuse peatada tehingu komplekti tõrketeade**: sõelub iga tehingu seadmine teabevahetus eraldi XML-i dokumenti, rakendades vastav ümbriku tehingu häälestada. See suvand, kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis ainult need tehingu komplekti on peatatud. Kui tehingu komplekti - tükeldatud andmevahetuse peatada Interchange tõrge: sõelub iga määramine, rakendades vastav ümbriku teabevahetus eraldi XML-i dokumenti tehingu. See suvand, kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis kogu andmevahetuse peatatakse.</br></br>**Säilitada Interchange - peatada tehingu komplekti tõrketeade**: jätab andmevahetuse samaks, XML-dokumendi kogu batched andmevahetuse loomine. See suvand, kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis ainult need tehingu komplekti on peatatud, samas kui muud tehingu komplekti töötlemist.</br></br>**Säilitada Interchange - peatada Interchange tõrketeade**: jätab andmevahetuse samaks, XML-dokumendi kogu batched andmevahetuse loomine. See suvand, kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis kogu andmevahetuse on peatatud.|

Teie lepingu on valmis käsitlema sissetulevaid sõnumeid, mis vastavad teie valitud sätted.

Partneritele saadetud sõnumite käsitlemiseks sätete konfigureerimiseks järgmist.  
10. Valige **Saada sätteid** konfigureerida, kuidas käesolevat lepingut kaudu saadetud sõnumid on saabuvaid.  

Saada sätted juhtimine on jagatud järgmistes jaotistes, sh identifikaatorite, kinnitus, skeemid, ümbrikud, märgistikku ja eraldajad, juhtelemendi arvude ja valideerimine. 

Siit leiate ülevaate neid juhtelemente. Tehke soovitud valikud, kuidas soovite käsitleda partneritele käesolevat lepingut kaudu saadetud sõnumite põhjal:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Sätete salvestamiseks nuppu **OK** .  

### <a name="identifiers"></a>Identifikaatorid
|Atribuut|Kirjeldus |
|----|----|
|UNB1.2 (süntaks versioon)|Valige väärtus vahemikus **1** – **4**.|
|UNB2.3 (saatja tagant marsruutimise aadress)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vähemalt üks märk ja kuni 14 märki.|
|UNB3.3 (adressaatide tagant marsruutimine aadress)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vähemalt üks märk ja kuni 14 märki.|
|UNB6.1 (adressaatide viide parool)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vähemalt ühe ja kuni 14 märki.|
|UNB6.2 (adressaatide viide kvalifikatsioon)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vähemalt üks märk ja kuni kaks märki.|
|UNB7 (rakenduse viite ID)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vähemalt üks märk ja kuni 14 märki|

### <a name="acknowledgment"></a>Kinnitus
|Atribuut|Kirjeldus |
|----|----|
|Saamist sõnum (CONTRL)|Märkige see ruut, kui majutatud partneri eeldab, et saada vastu võtta tehnilised (CONTRL) kinnitus. See säte määrab, et majutatud partner, kes saadab sõnumi, taotleb Külastajate partnerilt kinnitus.|
|Kinnituse (CONTRL)|Märkige see ruut, kui majutatud partneri ootab otstarbekas (CONTRL) kinnitus. See säte määrab, et majutatud partner, kes saadab sõnumi, taotleb Külastajate partnerilt kinnitus.|
|Luua SG1/SG4 tsükkel aktsepteeritud tehingu komplekti jaoks|Kui otsustasite paluda funktsionaalne kinnituse, märkige see ruut genereerimine SG1/SG4 silmuseid kehtivate otstarbekas CONTRL litsents aktsepteeritud tehingu komplekti jaoks.|

### <a name="schemas"></a>Skeemid
|Atribuut|Kirjeldus |
|----|----|
|UNH2.1 (TÜÜP)|Valige tehingu määramine tüüp.|
|UNH2.2 (VERSIOON)|Sisestage sõnumi versiooninumber.|
|UNH2.3 (VÄLJAANNE)|Sisestage sõnum väljaanne number.|
|SKEEMI|Valige skeemi kasutada. Skeemid asuvad teie konto. Teie skeemid juurdepääsu link esmalt loogika rakenduse oma konto.|

### <a name="envelopes"></a>Ümbrikud
|Atribuut|Kirjeldus |
|----|----|
|UNB8 (töötlemine prioriteet kood)|Sisestage tähestikuline väärtus, mis pole rohkem kui ühe märgi pikkused.|
|UNB10 (side leping)|Sisestage tähtedest ja numbritest koosnev väärtus, mis on vähemalt üks märk ja kuni 40 märki.|
|UNB11 (testida tähis)|Märkige see ruut näitavad, et loodud andmevahetuse testi andmed|
|UNA lõigu (stringi poole) rakendamine|Märkige see ruut UNA lõigu saata andmevahetuse loomiseks.|
|UNG segmente (funktsioon rühmapäises) rakendamine|Märkige see ruut loomiseks rühmitamise segmente funktsionaalne rühmapäises Külastajate partneri saadetud sõnumid. UNG lõigud loomiseks järgmised väärtused:</br></br>**UNG1**, sisestage tähtedest ja numbritest koosnev väärtusega vähemalt ühe märgi ja kuni kuus märki.</br></br>**UNG2.1**, sisestage tähtedest ja numbritest koosnev väärtusega vähemalt ühe märgi ja kuni 35 märki.</br></br>**UNG2.2**, sisestage tähtedest ja numbritest koosnev väärtus, kuni neli märki.</br></br>**UNG3.1**, sisestage tähtedest ja numbritest koosnev väärtusega vähemalt ühe märgi ja kuni 35 märki.</br></br>**UNG3.2**, sisestage tähtedest ja numbritest koosnev väärtus, kuni neli märki.</br></br>**UNG6**, sisestage tähtedest ja numbritest koosnev väärtusega vähemalt üks ja kuni kolm märki.</br></br>**UNG7.1**, sisestage tähtedest ja numbritest koosnev väärtusega vähemalt ühe märgi ja kuni kolm märki.</br></br>**UNG7.2**, sisestage tähtedest ja numbritest koosnev väärtusega vähemalt ühe märgi ja kuni kolm märki.</br></br>**UNG7.3**, sisestage tähtedest ja numbritest koosnev väärtusega 1 märgi vähemalt ja kuni 6 märki.</br></br>**UNG8**, sisestage tähtedest ja numbritest koosnev väärtusega vähemalt ühe märgi ja kuni 14 märki.|

### <a name="character-sets-and-separators"></a>Märk komplekti ja eraldajad
Kui märgistikus, saate sisestada teistsuguseid eraldajat kasutatakse iga sõnumi tüüp. Kui antud sõnumi Schema märgistikku pole määratud, kasutatakse vaikimisi märgistik.

|Atribuut|Kirjeldus |
|----|----|
|UNB1.1 (süsteemi ID)|Valige rakendatavad väljamineva andmevahetuse märgistik EDIFACT.|
|Skeemi|Valige ripploendist soovitud skeem. Iga rea lõpetatuks lisatakse uus rida. Valitud skeemi, saate valida seatud kasutada eraldajad.</br></br>**Komponendi elemendi eraldaja** – Enter üksikmärk eraldi kombineeritud andmeid.</br></br>**Andmete elemendi eraldaja** – Enter üksikmärk eraldi lihtsad andmed elemente kombineeritud andmeid.</br></br></br></br>**Asendamine märgi** – märkige see ruut, kui last, andmed, mis sisaldab märke, mida kasutatakse ka andmeid, lõigu või komponent eraldajad. Seejärel saate sisestada asendamine märk. EDIFACT väljamineva sõnumi loomisel kõik eksemplarid eraldusmärgid last andmed on asendatud määratud märgi.</br></br>**Lõigu Terminator** – Enter üksikmärk tähistamiseks on EDI lõigu lõppu.</br></br>**Järelliite** – valige märk, mis on kasutatud identifikaatoriga lõigu. Kui määrate järelliite, siis lõigu terminator andmeväli võib olla tühi. Kui lõigu Terminaator on tühi, peate määrama järelliide.|

### <a name="control-numbers"></a>Juhtelemendi arvud
|Atribuut|Kirjeldus |
|----|----|
|UNB5 (Interchange juhtelemendi arv)|Sisestage eesliite, väärtustevahemik andmevahetuse juhtelemendi number ja järelliide. Luua väljamineva teabevahetus kasutatakse neid väärtusi. Ees- ja järelliide on vabatahtlik; juhtelemendi number on nõutav. Juhtelemendi arvu suurendatakse iga uue sõnumi; ees- ja järelliide jäävad samaks.|
|UNG5 (rühm juhtelemendi arv)|Sisestage eesliite, väärtustevahemik andmevahetuse juhtelemendi number ja järelliide. Rühma juhtelement arvu kasutatakse neid väärtusi. Ees- ja järelliide on vabatahtlik; juhtelemendi number on nõutav. Juhtelemendi number muudeta iga uue sõnumi kuni suurima väärtuse; ees- ja järelliide jäävad samaks.|
|UNH1 (sõnumi päise viitenumbrit)|Sisestage eesliite, väärtustevahemik andmevahetuse juhtelemendi number ja järelliide. Luua sõnumi päise viitenumbrit kasutatakse neid väärtusi. Ees- ja järelliide on vabatahtlik; viide on nõutav. Viitenumbrit suurendatakse iga uue sõnumi; ees- ja järelliide jäävad samaks.|

### <a name="validations"></a>Valideerimised
|Atribuut|Kirjeldus |
|----|----|
|Sõnumi tüüp|Selle suvandi valimisel võimaldab andmevahetuse vastuvõtja valideerimine. See valideerimine sooritab EDI valideerimine tehingu seadistatud andmete elemente, andmetüübid, pikkus piirangud ja tühja andmete valideerimine ja koolitus eraldajad.|
|EDI valideerimine|Märkige see ruut, kui sooritada EDI valideerimine EDI atribuutide skeemi, pikkus piirangud, tühja andmeid ja lõpunullid eraldajad määratletud andmetüübid.|
|Laiendatud valideerimine|Selle suvandi valimisel võimaldab laiendatud valideerimine tehnosiirdeid saadud andmevahetuse saatja. See hõlmab valideerimine välja pikkus, vaba, ja lisaks XSD andmevalideerimise tüüp korduste arv. Saate lubada laiend valideerimine ilma EDI valideerimine, mis võimaldab või vastupidi.|
|Luba viib/lõputühikud nullidega|Selle suvandi valimisel saate määrata, et EDI andmevahetuse, mis saadud pool valideerimine pole nurjub, kui andmete element on mõni EDI andmevahetuse ei vasta selle pikkus nõue või lõputühikud, kuid vasta selle nõude pikkus, kui.|
|Viib/lõputühikud nullidega trimmimine|Selle suvandi valimisel kuvatakse ees- ja lõpunullid nullidega Kärbi.|
|Eraldajaga lõpust|Selle suvandi valimisel saate määrata EDI andmevahetuse, mis saadud pool pole valideerimine nurjub, kui andmete element on mõni EDI andmevahetuse ei vasta selle pikkus nõue (või lõputühikute) nullidega või lõputühikud tõttu, kuid vasta selle nõude pikkus, kui.</br></br>Valige **Pole lubatud** , kui te soovite lubada lõpunullid eraldajaid ja eraldajad teabevahetus saadud andmevahetuse saatja. Kui andmevahetuse sisaldab lõpunullid eraldajaid ja eraldajad, see kehtetuks.</br></br>Valige **Valikuline** aktsepteerimiseks tehnosiirdeid koos või ilma lõpunullid eraldajaid ja eraldajad.</br></br>Valige **kohustuslik** , kui vastuvõetud andmevahetuse peab sisaldama lõpunullid eraldajaid ja eraldajad.|

Pärast valige avatud enne **OK** .  
12. Valige konto enne **lepingute** paan ja te näete loetletud äsja lisatud lepingu.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>Lisateave
- [Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet ettevõtte integreerimine pakett")  
