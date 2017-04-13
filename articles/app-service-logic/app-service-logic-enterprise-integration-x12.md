<properties 
    pageTitle="Ülevaade X12 ja ettevõtte integreerimine pakett | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
    description="Siit saate teada, kuidas kasutada X12 lepingute loogika rakenduste loomine" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>Ettevõtte X12 integreerimine 

>[AZURE.NOTE]Selle lehel hõlmab soovitud X12 funktsioonid loogika rakenduste. Klõpsake EDIFACT saamiseks klõpsake [siin](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Saate luua ka X12 leping 
Enne kui saate vahetada X12 sõnumeid, peate looma ka X12 leping ja säilitage oma konto. Järgmist juhendab teid loomisel on X12 leping.

### <a name="heres-what-you-need-before-you-get-started"></a>Siin on, mida peate enne alustamist
- Määratletud Azure tellimuse [konto](./app-service-logic-enterprise-integration-accounts.md)  
- Teie konto juba määratletud vähemalt kaks [partnerite](./app-service-logic-enterprise-integration-partners.md)  

>[AZURE.NOTE]Kui loote leping, lepingu faili sisu peab vastama lepingu tüübist.    


Kui olete [loonud integreerimine kontot](./app-service-logic-enterprise-integration-accounts.md) ja [lisatud partnerite](./app-service-logic-enterprise-integration-partners.md), saate luua ka X12 lepingu järgmiste juhiste järgi:  

### <a name="from-the-azure-portal-home-page"></a>Azure'i videoportaali avalehel

Pärast sisselogimist [Azure portaali](http://portal.azure.com "Azure portaali"):  
1. Valige vasakul menüüst käsk **Sirvi** .  

>[AZURE.TIP]Kui te ei näe linki **Sirvi** , peate esmalt Menüü laiendamiseks. Selleks valige **menüü kuvamine** link, mis asub aadressil ülemises vasakus ahendatud menüü.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Tippige otsinguväljale filter *integreerimine* , seejärel valige tulemiloendis **Integreerimine kontod** .       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. Valige **Integreerimine kontod** labale avanenud integreerimine konto, mille loote lepingu. Kui te ei näe, mis tahes integreerimine kontod loendid, [loomiseks esimese](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Valige paan **lepingud** . Kui te ei näe paani lepingud, lisage see esmalt.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. Valige nupp **Lisa** lepingute tera, mis avab.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Sisestage oma lepingu **nimi** , seejärel valige lepingud tera, mis avab **Tüüp**, **Host Partner**, **Host identiteedi**, **Külastajate partneri**, **Külastajate identiteedi**.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Kui seate selle vastuvõtu sätted atribuudid, valige nupp **OK**  
Vaatame jätkata:  
8. Valige **Vastuvõtmine sätete** konfigureerimiseks saabuvaid sõnumeid vastu võetud rakenduses käesolevat lepingut viisi.  
9. Järgmistes jaotistes, sh identifikaatorite, kinnitus, skeemid, ümbrikud, juhtelemendi numbrid, valideerimised ja sisemise sätted on jagatud vastu sätted juhtimine. Konfigureerige atribuutidest oma lepingu partneri teil on vahetamine sõnumid, mille põhjal. Siit leiate ülevaate neid juhtelemente, nende põhjal, kuidas soovite käesolevat lepingut tuvastamiseks ja käsitlemiseks sissetulevate sõnumite konfigureerimine.  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Sätete salvestamiseks nuppu **OK** .  

### <a name="identifiers"></a>Identifikaatorid

|Atribuut|Kirjeldus |
|---|---|
|ISA1 (luba kvalifikatsioon)|Valige rippmenüüst loendis luba kvalifikatsioon väärtus.|
|ISA2|Valikuline. Sisestage autoriseerimine teabe väärtus. Kui sisestasite ISA1 väärtus pole 00, sisestage vähemalt üks täht või number ja kuni 10.|
|ISA3 (Turvalisus kvalifikatsioon)|Valige loendist drop-down turvalisus kvalifikatsioon väärtus.|
|ISA4|Valikuline. Sisestage väärtus, teabe turvalisus. Kui sisestasite ISA3 väärtus pole 00, sisestage vähemalt üks täht või number ja kuni 10.|

### <a name="acknowledgments"></a>Litsents 

|Atribuut|Kirjeldus |
|----|----|
|Oodatud TA1|Märkige see ruut tehnilise (TA1) kinnitus naasmiseks andmevahetuse saatja. Andmevahetuse saatja alusel lepingu saatmine sätete saadetakse need litsents.|
|Oodatud FA|Märkige see ruut otstarbekas (FA) kinnitus naasmiseks andmevahetuse saatja. Seejärel valige, kas soovite 997 või 999 kinnitused, mis põhineb skeemi versioonid, millega töötate. Andmevahetuse saatja alusel lepingu saatmine sätete saadetakse need litsents.|
|Kaasa AK2/IK2 tsükkel|Märkige see ruut Luba genereerimine AK2 silmuseid otstarbekas litsents aktsepteeritud tehingu komplekti jaoks sisse. Märkus: See ruut on lubatud ainult juhul, kui valisite PV oodatud ruut.|

### <a name="schemas"></a>Skeemid

Valige skeem iga kande tüüp (ST1) ja saatja rakenduse (GS2). Müügivõimaluste vastuvõtu disassembles sissetuleva sõnumi sobitamine väärtused ST1 ja GS2 sissetulevate sõnumis siin seatud väärtused ja skeemiga sissetuleva sõnumi koos skeemi seate siin.

|Atribuut|Kirjeldus |
|----|----|
|Versioon|Valige soovitud X12 versioon|
|Kande tüüp (ST01)|Tehingu tüüp|
|Saatja rakenduse (GS02)|Valige saatja rakendus|
|Skeemi|Valige skeemifaili, mida soovite meile. Skeemifailid asuvad teie konto.|

### <a name="envelopes"></a>Ümbrikud

|Atribuut|Kirjeldus |
|----|----|
|ISA11 kasutamine|Selle välja abil saate määrata eraldaja tehingu komplekti:</br></br>Valige Standard identifikaator kasutamine koma märke, "." Sissetuleva dokumendi EDI koma märke asemel saavad kohaletoimetamisel.</br></br>Valige Korduse eraldaja määramiseks eraldaja korduvate esinemiskordade lihtsad andmed element või korduvate andmestruktuur jaoks. Näiteks tavaliselt kasutatakse korduse eraldaja (^). Jaoks HIPAA skeemid, saate kasutada ainult (^).|

### <a name="control-numbers"></a>Juhtelemendi arvud

|Atribuut|Kirjeldus |
|----|----|
|Keela Interchange juhtelemendi arvu duplikaadid|Märkige see suvand, et blokeerida dubleeritud tehnosiirdeid. Kui valitud, kontrollib BizTalki Services portaal andmevahetuse juhtelemendi number (ISA13) vastuvõetud andmevahetuse ei vasta andmevahetuse juhtelemendi number. Vaste tuvastamisel vastuvõtu müügivõimaluste protsessi andmevahetuse.<br/>Kui teil on liitunud keelamiseks dubleeritud andmevahetuse juhtelemendi arve, saate määrata kus kontrollitakse andes sobiv väärtus Otsi dubleeritud ISA13 iga x päeva päevade arv.|
|Rühma juhtelement arv duplikaatide keelamiseks|Märkige see suvand, et blokeerida tehnosiirdeid dubleeritud jaotises juhtelemendi arvudega.|
|Tehingu määramine juhtelemendi arv duplikaatide keelamiseks|Märkige see suvand, et blokeerida tehnosiirdeid dubleeritud tehingu määramine juhtelemendi arvudega.|

### <a name="validations"></a>Valideerimised

|Atribuut|Kirjeldus |
|----|----|
|Sõnumi tüüp|EDI sõnumi tüüp, nt 850 ostutellimuse või 999-rakendamine kinnitus.|
|EDI valideerimine|Sooritab EDI valideerimine EDI atribuutide skeemi, pikkus piirangud, tühja andmeid ja lõpunullid eraldajad määratletud andmetüübid.|
|Laiendatud valideerimine|Kui andmetüüp pole EDI, valideerimine on andmete elemendi nõue ja lubatud korduse, iseloomustavate ja andmevalideerimise elemendi pikkus (min ja max).|
|Luba viib/lõputühikud nullidega|Säilitatakse lisaruumi ja null märgid, mis on viib või lõputühikud. Need on eemaldatud.|
|Eraldajaga lõpust poliitika|Loob lõpunullid eraldajad saanud andmevahetuse kohta. Suvandite hulka kuuluvad NotAllowed, valikuline ja kohustuslik.|

### <a name="internal-settings"></a>Sisemise sätted

|Atribuut|Kirjeldus |
|----|----|
|Teisendada ka mitte kaudset kümnendsüsteemis Nn alusena 10 arvulise väärtuse.|Teisendab EDI numbri, määratud alusega 10 arvulise väärtuse vahe XML-BizTalki teenuste portaali sisse Nn vorming.|
|Kui lõpunullid eraldajad on lubatud, looge tühi XML-sildid|Märkige see ruut olema andmevahetuse saatja lisada tühja XML-i sildid lõpunullid eraldajad.|
|Sissetulev partiide töötlemine|Kui tehingu komplekti - tükeldatud andmevahetuse peatada tehingu komplekti tõrge: sõelub iga tehingu seadmine teabevahetus eraldi XML-i dokumenti, rakendades vastav ümbriku tehingu häälestada. See suvand, kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis BizTalki teenuste peatab ainult need tehingu komplekti. </br></br>Kui tehingu komplekti - tükeldatud andmevahetuse peatada interchange tõrge: sõelub iga määramine, rakendades vastav ümbriku teabevahetus eraldi XML-i dokumenti tehingu. See suvand, kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis BizTalki teenuste peatab kogu andmevahetuse.</br></br>Säilitada Interchange - peatada tehingu komplekti tõrge: jätab andmevahetuse samaks, XML-dokumendi kogu batched andmevahetuse loomine. See suvand, kui onAe või rohkem tehingut seab andmevahetuse valideerimine nurjub, siis BizTalki teenuste peatab ainult need tehingu komplekti, jätkates töödelda muude tehingu komplekti.</br></br>Säilitada Interchange - peatada interchange tõrge: jätab andmevahetuse samaks, XML-dokumendi kogu batched andmevahetuse loomine. See suvand, kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis BizTalki teenuste peatab kogu andmevahetuse.</br></br>|

Teie lepingu on valmis hakkama sissetulevad sõnumid, mis vastavad teie valitud skeemiga.

Partneritele saadetud sõnumite käsitlemiseks sätete konfigureerimiseks järgmist.  
11. Valige **Saada sätete** konfigureerimiseks sõnumite kustutamine käesolevat lepingut käsitleda viisi.  

Saada sätted juhtimine on jagatud järgmistes jaotistes, sh identifikaatorite, kinnitus, skeemid, ümbrikud, juhtelemendi numbrid, märgistikku ja tuhandeliste eraldaja ja valideerimine. 

Siit leiate ülevaate neid juhtelemente. Tehke soovitud valikud, kuidas soovite käsitleda partneritele käesolevat lepingut kaudu saadetud sõnumite põhjal:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Sätete salvestamiseks nuppu **OK** .  

### <a name="identifiers"></a>Identifikaatorid
|Atribuut|Kirjeldus |
|----|----|
|Luba kvalifikatsioon (ISA1)|Valige rippmenüüst loendis luba kvalifikatsioon väärtus.|
|ISA2|Sisestage autoriseerimine teabe väärtus. Kui see väärtus pole 00, seejärel sisestage vähemalt üks täht või number ja kuni 10.|
|Turvalisus kvalifikatsioon (ISA3)|Valige loendist drop-down turvalisus kvalifikatsioon väärtus.|
|ISA4|Sisestage väärtus, teabe turvalisus. Kui selle väärtus on peale 00 tekstivälja väärtus (ISA4), seejärel sisestage vähemalt üks tähtedest ja numbritest koosnev väärtus ja kuni 10.|

### <a name="acknowledgment"></a>Kinnitus
|Atribuut|Kirjeldus |
|----|----|
|Oodatud TA1|Märkige see ruut tehnilise (TA1) kinnitus naasmiseks andmevahetuse saatja. See säte määrab, et host partner, kes saadab sõnumi taotleb kinnituse Külastajate partnerilt lepingus. Nende litsents oodatakse host partneri põhjal lepingu vastu sätted.|
|Oodatud FA|Märkige see ruut otstarbekas (FA) kinnitus naasmiseks andmevahetuse saatja, ja seejärel valige, kas soovite 997 või 999 kinnitused, mis põhineb skeemi versioonid, millega töötate. Nende litsents oodatakse host partneri põhjal lepingu vastu sätted.|
|PV versioon|Valige PV versioon|

### <a name="schemas"></a>Skeemid
|Atribuut|Kirjeldus |
|----|----|
|Versioon|Valige soovitud X12 versioon|
|Kande tüüp (ST01)|Tehingu tüüp|
|SKEEMI|Valige skeemi kasutada. Skeemid asuvad teie konto. Teie skeemid juurdepääsu link esmalt loogika rakenduse oma konto.|

### <a name="envelopes"></a>Ümbrikud
|Atribuut|Kirjeldus |
|----|----|
|ISA11 kasutamine|Selle välja abil saate määrata eraldaja tehingu komplekti:</br></br>Valige Standard identifikaator kasutamine koma märke, "." Sissetuleva dokumendi EDI koma märke asemel saavad kohaletoimetamisel.</br></br>Valige Korduse eraldaja määramiseks eraldaja korduvate esinemiskordade lihtsad andmed element või korduvate andmestruktuur jaoks. Näiteks tavaliselt kasutatakse korduse eraldaja (^). Jaoks HIPAA skeemid, saate kasutada ainult (^).</br>|
|Korduse eraldaja|Sisestage korduse eraldaja|
|Juhtelemendi versiooninumbrit (ISA12)|Valige kasutatavat BizTalki teenuste portaali loomisel on väljamineva andmevahetuse X12 standard versiooni.|
|Kasutus näidik (ISA15)|Sisestage teave (I), andmeid (P), kas teabevahetus kontekstis või testida andmeid (T). EDI vastuvõtmine müügivõimaluste tõstab esile selle atribuudi konteksti.|
|Skeemi|Saate sisestada, kuidas BizTalki Services portaal genereeritud X12 kodeeritud andmevahetuse, mis saadetakse saata müügivõimaluste GS ja ST lõigud.</br></br>Saate seostada GS1, GS2, GS3, GS4, GS5, GS7 ja GS8 andmete elementide väärtuste kande tüüp ja versioon/väljaanne andmete elementide väärtusi. Kui BizTalki Services portaal mis määratleb meilisõnumi XML-i on kande tüüp ja versioon/väljaanne elementide rea ruudustiku väärtusi, siis see kuvab GS1, GS2, GS3, GS4, GS5, GS7 ja GS8 andmete elementide ümbrik väljamineva asenduse ruudustiku samal real olevad väärtused. Tüüp ja versioon/väljaanne elementide väärtused peavad olema kordumatud.</br></br>Valikuline. GS1, valige rippmenüü loendist väärtus otstarbekas kood.</br></br>Nõutav. GS2, sisestage tähtedest ja numbritest koosnev väärtuse rakenduse saatja on vähemalt kaks märki ja kuni 15 märki.</br></br>Nõutav. GS3, sisestage tähtedest ja numbritest koosnev väärtuse taotluse vastuvõtja on vähemalt kaks märki ja kuni 15 märki.</br></br>Valikuline. Valige GS4, CCYYMMDD või AAKKPP.</br></br>Valikuline. Valige GS5, HHMM, HHMMSS või HHMMSSdd.</br></br>Valikuline. GS7, valige väärtus vastutav asutus rippmenüü loendist.</br></br>Valikuline. GS8, sisestage oma vähemalt ühe märgi ning kuni 12 märki dokumendi tähtedest ja numbritest koosnev väärtuse.</br></br>**Märkus**: need väärtused, mis BizTalki Services portaal sisestab see hoone kui tehingu tippige asenduse väljadel GS ja sama rea versioon/väljaanne elemendid on seotud andmevahetuse vaste.|

### <a name="control-numbers"></a>Juhtelemendi arvud
|Atribuut|Kirjeldus |
|----|----|
|Interchange juhtelemendi Number (ISA13)|Nõutav. Sisestada väärtuste vahemiku andmevahetuse juhtelemendi number BizTalki Services portaal väljamineva teabevahetus loomisel kasutada. Sisestage vähemalt 1 ja maksimaalselt 999999999 arvulise väärtuse.|
|Rühma juhtelement Number (GS06)|Nõutav. Sisestage arvud, et BizTalki teenuste portaali kasutama jaotises juhtelemendi arv vahemikus. Sisestage kuni üheksa märki ja vähemalt ühe märgi arvulise väärtuse.|
|Tehingu määramine juhtelemendi Number (ST02)|Tehingu määrata juhtelemendi number (ST02), sisestage arvväärtuste keskmisel välju ja tähtedest ja numbritest koosnev väärtuste vahemiku valikuline ees- ja järelliide. Kõik neli välja maksimumpikkus on üheksa märki.|
|Prefix (eesliide)|Tehingu määramine juhtelemendi numbrid, mida kasutatakse kinnitus vahemiku määramiseks sisestage väärtused väljadele ACK juhtelemendi number (ST02). Sisestage arvulise väärtuse keskele kaks väljade ja tähtedest ja numbritest koosnev väärtuse (soovi korral) ees- ja järelliide väljade jaoks. Teise eesnime väljad on nõutavad ja sisaldavad miinimum- ja väärtusi juhtelemendi arv; ees- ja järelliide on valikulised. Kõigi kolme väljaga maksimumpikkus on üheksa märki.|
|Järelliide|Tehingu määramine juhtelemendi numbrid, mida kasutatakse kinnitus vahemiku määramiseks sisestage väärtused väljadele ACK juhtelemendi number (ST02). Sisestage arvulise väärtuse keskele kaks väljade ja tähtedest ja numbritest koosnev väärtuse (soovi korral) ees- ja järelliide väljade jaoks. Teise eesnime väljad on nõutavad ja sisaldavad miinimum- ja väärtusi juhtelemendi arv; ees- ja järelliide on valikulised. Kõigi kolme väljaga maksimumpikkus on üheksa märki.|

### <a name="character-sets-and-separators"></a>Märk komplekti ja eraldajad
Kui märgistikus, saate sisestada teistsuguseid eraldajat kasutatakse iga sõnumi tüüp. Kui antud sõnumi Schema märgistikku pole määratud, kasutatakse vaikimisi märgistik.

|Atribuut|Kirjeldus |
|----|----|
|Kasutatav märgistik|Valige soovitud X12 märgistik atribuudid, mida saate lepingu kinnitamiseks.</br></br>**Märkus**: BizTalki Portal kasutab seda sätet ainult valideerimiseks seotud lepingu atribuutide väärtused. Vastuvõtu kohaletoimetamisel või saada müügivõimaluste ignoreerib-märgistik atribuut täites käitusaja töötlemine.|
|Skeemi|Valige (+) sümbol ja valige loendist ripploendis skeem. Valitud skeemi, saate valida seatud kasutada eraldajad.</br></br>Komponendi elemendi eraldaja – Enter üksikmärk eraldi kombineeritud andmeid.</br></br>Andmete elemendi eraldaja – Enter üksikmärk eraldi lihtsad andmed elemente kombineeritud andmeid.</br></br></br></br>Asendamine märk – märkige see ruut, kui last, andmed, mis sisaldab märke, mida kasutatakse ka andmeid, lõigu või komponent eraldajad. Seejärel saate sisestada asendamine märk. Loomisel on väljamineva sõnumi X12 kõik eksemplarid eraldaja märkide last, andmed on asendatud määratud märgi.</br></br>Segmendi Terminator – sisestage üksikmärki tähistamiseks on EDI lõigu lõppu.</br></br>Järelliide – valige märk, mis on kasutatud identifikaatoriga lõigu. Kui määrate järelliite, siis lõigu terminator andmeväli võib olla tühi. Kui lõigu Terminaator on tühi, peate määrama järelliite.|

### <a name="validation"></a>Valideerimine
|Atribuut|Kirjeldus |
|----|----|
|Sõnumi tüüp|Selle suvandi valimisel võimaldab andmevahetuse vastuvõtja valideerimine. See valideerimine sooritab EDI valideerimine tehingu seadistatud andmete elemente, andmetüübid, pikkus piirangud ja tühja andmete valideerimine ja lõputühikud eraldajad.|
|EDI valideerimine||
|Laiendatud valideerimine|Selle suvandi valimisel võimaldab laiendatud valideerimine tehnosiirdeid saadud andmevahetuse saatja. See hõlmab valideerimine välja pikkus, vaba, ja lisaks XSD andmevalideerimise tüüp korduste arv. Saate lubada laiend valideerimine ilma EDI valideerimine, mis võimaldab või vastupidi.|
|Luba ees / lõpunullid nullidega|Selle suvandi valimisel saate määrata, et EDI andmevahetuse, mis saadud pool valideerimine pole nurjub, kui andmete element on mõni EDI andmevahetuse ei vasta selle pikkus nõue või lõputühikud, kuid vasta selle nõude pikkus, kui.|
|Lõpunullid eraldaja|Selle suvandi valimisel saate määrata EDI andmevahetuse, mis saadud pool pole valideerimine nurjub, kui andmete element on mõni EDI andmevahetuse ei vasta selle pikkus nõue (või lõputühikute) nullidega või lõputühikud tõttu, kuid vasta selle nõude pikkus, kui.</br></br>Valige pole lubatud, kui te soovite lubada lõpunullid eraldajaid ja eraldajad teabevahetus saadud andmevahetuse saatja. Kui andmevahetuse sisaldab lõpunullid eraldajaid ja eraldajad, see kehtetuks.</br></br>Valige valikuline aktsepteerimiseks tehnosiirdeid koos või ilma lõpunullid eraldajaid ja eraldajad.</br></br>Valige kohustuslik, kui vastuvõetud andmevahetuse peab sisaldama lõpunullid eraldajaid ja eraldajad.|

Pärast valite **OK** avatud labad:  
13. Valige konto enne **lepingute** paan ja te näete loetletud äsja lisatud lepingu.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>Lisateave
- [Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet ettevõtte integreerimine pakett")  
