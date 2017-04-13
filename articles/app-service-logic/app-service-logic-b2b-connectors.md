<properties 
    pageTitle="Business ettevõtte konnektorid ja API rakendused loogika rakendustes | Microsoft Azure'i" 
    description="Saate teada, kuidas luua ja konfigureerida EDI, EDIFACT, AS2 ja TPM konnektorid; microservices arhitektuur" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Business ettevõtte konnektorid ja API rakendused

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Loogika rakenduste sisaldab palju BizTalki API rakendusi, mis on olulised integreerimine keskkonnas. Nende rakenduste API põhinevad põhimõtet ja tööriistu, mida kasutatakse BizTalki serveri sees, kuid on nüüd saadaval loogika rakenduste osana. 

Üks kategooria API need rakendused on Business Business (B2B) API rakendused. Nende B2B API rakenduste kasutamisel saate hõlpsasti lisada partnerid, luua lepingud ja teha kõike, mida soovite kohapealse EDI, AS2 ja EDIFACT abil.  

Need B2B API rakendused pakuvad "Päästik" ja "Toimingu" võimalusi. Kindla sündmusega, nagu on X12 saabumise põhjal uue seansi käivitamine käivitamiseks partneri sõnum. Toimingu on tulemus, nagu on X12 pärast sõnumi, siis saatke sõnum AS2 abil.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Mis on äri-Business Connectori või API rakendusi
Funktsiooni Business Business (B2B) sisaldab olemasoleva, valmismallide API rakendused, mis võimaldavad ettevõtteid, osakondades, rakendused, jne kaudu suhtlemiseks AS2, EDI ja EDIFACT. 

B2B API rakendused on järgmised. 

Konnektor või API rakendused | Kirjeldus
--- | ---
BizTalki börsipäev partnerite haldus | API rakendus, mis loob business business (B2B) seoste partnerid ja lepingud. Nende seoste kasutada funktsiooni AS2, EDIFACT ja X12 Protocol (protokoll).<br/><br/>TPM API rakendus on base AS2 konnektor, ja selle X12 või EDIFACT API rakendused. 
AS2 konnektor | Konnektor, mis võtab vastu ja saadab sõnumeid AS2 transport abil. Konnektor edastus andmeid turvaliselt ja usaldusväärselt Interneti kaudu.
BizTalki EDIFACT | API rakendus, mis võtab vastu ja saadab sõnumeid, kasutades EDIFACT. EDIFACT on nimetatud ka tavaliselt UN/EDIFACT (Ameerika riigid/elektroonilise andmete Interchange haldus, äri ja Transport) ja tööstusharudes levinud.
BizTalki X12 | API rakendus, mis võtab vastu ja saadab sõnumeid, kasutades funktsiooni X12 Protocol (protokoll). X12 on nimetatud ka tavaliselt ASC X 12 (akrediteeritud IASC X12) ja tööstusharudes levinud. 


Nende rakenduste API saavad teostada erinevate EDI sõnumside tööülesanded. Näiteks AS2 kasutamisega, saate turvaliseks vastu võtta ja saata erinevat tüüpi sõnumid (elektrooniline XML, kindla-faili, jne) kliendile vastuseks jagatise ettevõtte nagu Personalihaldus või kõik, mis kasutab AS2. 

Saate luua nii palju API rakendused, kui soovite ja neid hõlpsasti luua. Saate kasutada ka ühe API rakenduse mitme stsenaariumid või töövood.

Selleks saate ilma koodi kirjutamata. Alustagem. 


## <a name="requirements-to-get-started"></a>Nõuded alustamiseks
Kui loote B2B API rakendused, on mõned ressursid. Nendeks peab olema teie loodud enne, kui neid saab kasutada muude API rakendustes. Nende nõuete hulka kuuluvad: 

Nõue | Kirjeldus
--- | ---
Azure'i SQL-andmebaas | Talletab B2B üksusi, sh partnerid, skeemid, serdid ja lepingud. B2B API rakenduste jaoks on vaja oma Azure'i SQL-andmebaasi. <br/><br/>**Märkus** Kopeerige see andmebaas ühendusstring.<br/><br/>[SQL Azure'i andmebaasi loomine](../sql-database/sql-database-get-started.md)
Azure'i bloobimälu container | Salvestab sõnumi atribuudid, kui AS2 arhiivimine on lubatud. Kui te ei vaja AS2 sõnumi arhiivimine, ei ole vaja salvestusruumi ümbrises. <br/><br/>**Märkus** Kui teil on lubamine, arhiivimine, kopeerige see bloobimälu ühendusstring.<br/><br/>[Azure'i salvestusruumi kontode kohta](../storage/storage-create-storage-account.md)
Teenuse siini Namespace ja selle klahvi väärtused | Salvestab X12 ja EDIFACT partiide andmed. Kui te ei vaja partiide, teenuse siini nimeruum ei ole vaja.<br/><br/>**Märkus** Kui teil on lubamine partiide, kopeerige need väärtused.<br/><br/>[Teenuse siini Namespace loomine](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM eksemplari | BizTalki börsipäev partnerite halduse (TPM) eksemplari on vajalik mõne AS2 konnektor ja X12 või EDIFACT API rakenduse loomiseks. TPM API rakenduse loomisel loote TPM eksemplari. <br/><br/>**Märkus** Teate oma TPM API rakenduse nime. 


## <a name="create-the-api-apps"></a>API rakenduste loomine
Azure'i portaalis või REST API-de abil saab luua B2B API rakendused. 


### <a name="create-the-api-apps-using-rest-apis"></a>Looge API rakendused REST API-de kasutamine
[Kuidas kasutada REST API-de dokumentatsioonist.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>Azure'i portaalis B2B API rakenduste loomine
Azure'i portaalis, saate luua B2B API rakenduste loomisel loogika rakendused, veebirakenduste või mobiilirakenduste kohta. Või saate luua oma tera abil. Mõlemaid viise on lihtne, et see sõltub teie vajadustele või. Mõned kasutajad eelistate B2B API kõik rakendused oma teatud omadustega esmalt looma. Seejärel loogika rakendused/Web Apps/Mobile'i rakenduste loomine ja lisamine loodud B2B API rakendused.  

Järgmiste juhiste loomine B2B API rakendused tera API rakenduste abil.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>BizTalki börsipäev partnerite halduse (TPM) API rakenduste loomine

> [AZURE.NOTE] BizTalki börsipäev partnerite halduse (TPM) eksemplari on vajalik mõne AS2 konnektor ja X12 või EDIFACT API rakenduse loomiseks. TPM API rakenduse loomisel loote TPM eksemplari.

Järgmised toimingud TPM eksemplari loomiseks tehke järgmist.

1. Azure'i portaalis Startboard (Avaleht) valimine **turuplats** **API rakendused** on loetletud kõik olemasolevad API rakendused ja konnektorid. Samuti saate **Otsingu** teatud B2B API rakendused.
2. Valige **BizTalki börsipäev partnerite haldus**. Valige uus laba **loomine**. 
3. Sisestage soovitud atribuudid. 

    Atribuut | Kirjeldus
--- | ---
Nimi | Sisestage mis tahes TPM eksemplari nimi. Näiteks saate selle nime *AccountsPayableTPM*.
Paketi sätted | Sisestage ADO.net-i **Andmebaasi ühendusstringi** loodud Azure SQL-andmebaasiga. <br/><br/>Kui kopeerite ühendusstring, parool on lisatud ühendusstring. Ärge unustage sisestage parool ühendusstring.
Rakenduse teenusleping | On loetletud teie makse leping. Saate seda muuta, kui teil on vaja rohkem või vähem ressursse.
Esimese taseme hinnad | Kirjutuskaitstud atribuudi, kus on loetletud hinnakirjad klassile Azure tellimuse. 
Ressursirühm | Looge uus või saate kasutada olemasolevat rühma. Kõik API rakendused ja ühendused loogika rakendused, veebirakenduste ja mobiilirakenduste peab olema sama ressursirühma. <br/><br/>[Ressursi rühmade kasutamine](../azure-resource-manager/resource-group-overview.md) selgitatakse selle atribuudi. 
Tellimuse | Kirjutuskaitstud atribuudi, kus on loetletud praeguse tellimuse.
Asukoht | Azure'i teenust majutava geograafilise asukoha. 
Startboard lisamine | Valige see B2B API rakenduse lisamine oma pakpoordile (Avaleht).

4. Valige **Loo**. 

Pärast TPM API rakenduse (TPM eksemplari) on loodud, saate seejärel luua AS2 konnektor ja/või selle X12 või EDIFACT API rakendused. 


#### <a name="create-the-as2-connector"></a>Looge AS2 konnektor

1. Azure'i portaalis Startboard (Avaleht) valimine **turuplats** **API rakendused** on loetletud kõik olemasolevad API rakendused ja konnektorid. Samuti saate **Otsingu** teatud B2B API rakendused.
2. Valige **AS2 konnektor**. Valige uus laba **loomine**. 
3. Sisestage soovitud atribuudid. 

    Atribuut | Kirjeldus
--- | ---
Nimi | Sisestage mis tahes AS2 konnektor. Näiteks saate selle nime *AS2Connector*.
Paketi sätted | Sisestage selle API rakenduste kohta, nagu näiteks TPM nimi teatud sätted. <br/><br/>Selle teema teatud atribuutide [Lisamine AS2 paketi sätted](#AddAS2Conn) jaotisest. 
Rakenduse teenusleping | On loetletud teie makse lepingut. Saate seda muuta, kui teil on vaja rohkem või vähem ressursse.
Esimese taseme hinnad | Kirjutuskaitstud atribuudi, kus on loetletud hinnakirjad klassile Azure tellimuse. 
Ressursirühm | Looge uus või saate kasutada olemasolevat rühma. [Ressursi rühmade kasutamine](../azure-resource-manager/resource-group-overview.md) selgitatakse selle atribuudi. 
Tellimuse | Kirjutuskaitstud atribuudi, kus on loetletud praeguse tellimuse.
Asukoht | Azure'i teenust majutava geograafilise asukoha. 
Startboard lisamine | Valige see B2B API rakenduse lisamine oma pakpoordile (Avaleht).

    **<a name="AddAS2Conn"></a>AS2 konnektor paketi sätted**

    Atribuut | Kirjeldus
--- | --- 
Andmebaasi ühendusstringi | Sisestage ühendusstring ADO.net-i loodud Azure SQL-andmebaasiga. Kui kopeerite ühendusstring, parool on lisatud ühendusstring. Ärge unustage sisestage parool ühendusstring enne kleepimist.
Sissetulevate sõnumite arhiivimise lubamine | Valikuline. Luba selle atribuudi talletamiseks sissetuleva AS2 sõnumi partneri saadud sõnumi atribuudid. 
Azure'i bloobimälu salvestusruumi ühendusstring.  | Sisestage ühendusstring Azure'i bloobimälu s.o ümbris, mille olete loonud. Arhiivimise lubamisel encoded ja dekodeeritud sõnumid salvestatakse see salvestusruumi ümbrises.
TPM eksemplari nimi | Sisestage **BizTalki börsipäev partnerite halduse** varem loodud API rakenduse nimi. AS2 konnektor loomisel selle konnektori aktiveeritakse ainult AS2 kokkulepete raames TPM astme.

4. Valige **Loo**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>X12 või EDIFACT API rakenduste loomine

1. Azure'i portaalis Startboard (Avaleht) valimine **turuplats** **API rakendused** on loetletud kõik olemasolevad API rakendused ja konnektorid. Samuti saate **Otsingu** teatud B2B API rakendused.
2. Valige **BizTalki X 12** või **BizTalki EDIFACT**. Valige uus laba **loomine**. 
3. Sisestage soovitud atribuudid. 

    Atribuut | Kirjeldus
--- | ---
Nimi | Sisestage mis tahes B2B API rakenduse nimi. Näiteks saate selle nime *EDI850APIApp*.
Paketi sätted | Sisestage selle API rakenduste kohta, nagu näiteks TPM nimi teatud sätted. <br/><br/>Lugege selles teemas teatud atribuutide [X12 või EDIFACT paketi sätted](#AddX12) . 
Rakenduse teenusleping | On loetletud teie makse leping. Saate seda muuta, kui teil on vaja rohkem või vähem ressursse.
Esimese taseme hinnad | Kirjutuskaitstud atribuudi, kus on loetletud hinnakirjad klassile Azure tellimuse. 
Ressursirühm | Looge uus või saate kasutada olemasolevat rühma. [Ressursi rühmade kasutamine](../azure-resource-manager/resource-group-overview.md) selgitatakse selle atribuudi. 
Tellimuse | Kirjutuskaitstud atribuudi, kus on loetletud praeguse tellimuse.
Asukoht | Azure'i teenust majutava geograafilise asukoha. 
Startboard lisamine | Valige see B2B API rakenduse lisamine oma pakpoordile (Avaleht).

    **<a name="AddX12"></a>X12 ja EDIFACT API rakenduste paketi sätted**  

    Atribuut | Kirjeldus
--- | --- 
Andmebaasi ühendusstringi | Sisestage ühendusstring ADO.net-i loodud Azure SQL-andmebaasiga. Kui kopeerite ühendusstring, parool on lisatud ühendusstring. Ärge unustage sisestage parool ühendusstring enne kleepimist.
Teenuse siini Namespace | Sisestage loodud teenuse siini nimeruumi. Vajalik ainult kui partiide on lubatud. 
Teenuse siini Namespace ühiskasutuses kiirklahv nimi | Sisestage teenuse siini nimeruumi kiirklahv loodud. Vajalik ainult kui partiide on lubatud. 
Teenuse siini Namespace jagatud juurdepääsu võti väärtus | Sisestage teenuse siini nimeruumi juurdepääsu võti väärtus, mis on loodud. Vajalik ainult kui partiide on lubatud. 
TPM eksemplari nimi | Sisestage **BizTalki börsipäev partnerite halduse** varem loodud API rakenduse nimi. Funktsiooni X12 või EDIFACT API rakenduste loomisel API rakenduse aktiveeritakse ainult X12/EDFIACT kokkulepete raames TPM astme.

4. Valige **Loo**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Lisage partnerid, lepingud, serdid ja skeemid 
Azure'i portaalis, avage rakenduse TPM API. Lisage jaotises **komponendid** partnerid, lepingud, serdid ja skeemid. 

Saate lisada ka lepingute AS2 konnektorid, X12 API rakendused ja EDIFACT API rakendused. 


## <a name="monitor-your-api-apps"></a>Oma API rakenduste jälgimine
Azure'i portaalis, avage rakenduse TPM API. Jaotises **toimingud** saate vaadata erinevaid toiminguid. Näiteks saate teha järgmist.

- Kuva teatised ja tõrge sündmused
- Mälu kasutus- ja jutulõnga count töötaja protsessi (leiate w3wp) vaatamine
- Vaatamine ja veebi logid

Rohkem veebisaidil [loogika rakenduste jälgimine](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>Rakenduse API rakenduste lisamine 
Microsoft Azure'i rakendust Service seab erinevates tüübid, mida saate kasutada järgmisi B2B API rakendusi. Saate luua uue või olemasoleva B2B API rakenduste lisamine loogika rakendused, mobiilirakenduste või Web Apps. 

Teie rakenduses lihtsalt käsk B2B API rakendusi galeriist automaatselt lisab selle rakenduse.  

> [AZURE.IMPORTANT] Konnektorid ja API rakenduste varem loodud lisamiseks luua loogika rakendused, mobiilirakenduste või veebirakenduste ühte ressursirühma. 

Järgmised toimingud lisada B2B API rakendused loogika rakendused, mobiilirakenduste või veebirakenduste: 

1. Azure'i portaalis Startboard (Avaleht), avage **turuplats**ja otsige oma loogika, mobiiltelefon või Web Apps. 

    Kui loote uue rakenduse, otsige loogika rakendused, mobiilirakenduste või Web Apps. Valige rakendus ja valige **Loo**uus laba. [Loogika rakenduse loomine](app-service-logic-create-a-logic-app.md) on loetletud toimingud. 

2. Avage rakendus ja valige **päästikute ja toimingud**. 

3. Valige **Galerii**B2B API rakendus, mis lisab automaatselt teie rakendus. Samuti saate luua uue B2B API rakenduse.

    > [AZURE.IMPORTANT] AS2 konnektor ja X12, vaja EDIFACT API rakendused TPM eksemplari. Nii, et kui loote uue B2B API rakendusi, TPM API rakenduse esmalt loomine ja seejärel looge AS2 konnektor, X12 API rakenduse või EDIFACT API rakenduse. 

4. Valige muudatuste salvestamiseks nuppu **OK** . 

>[AZURE.NOTE] Alustada Azure'i loogika rakendused enne kasutajaks registreerumist Azure'i konto, [Proovige loogika rakendused](https://tryappservice.azure.com/?appservice=logic). Saate luua kohe lühiajaline starter loogika rakendus. Nõutav; krediitkaardid kohustusi.

## <a name="more-b2b-resources"></a>Ressursside B2B

[B2B protsessi loomine](app-service-logic-create-a-b2b-process.md)<br/>
[Kauplemise partneri lepingu loomine](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Mis on konnektorid ja BizTalki API rakendusi](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Loogika rakendused ja veebirakenduste kohta vt
[Mis on loogika rakendusi?](app-service-logic-what-are-logic-apps.md)<br/>
[Veebisaitide ja veebirakenduste Azure rakenduse teenus](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Lisateavet konnektorid

[Konnektorid ja API rakenduste loend](app-service-logic-connectors-list.md)<br/><br/>
[Mis on konnektorid ja BizTalki API rakendusi](app-service-logic-what-are-biztalk-api-apps.md) 
