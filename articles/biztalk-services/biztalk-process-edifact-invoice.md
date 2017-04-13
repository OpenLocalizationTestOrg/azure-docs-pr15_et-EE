<properties
   pageTitle="Õpetus: Töötlemine EDIFACT arvete Azure BizTalki teenuste kaudu | Microsoft Azure'i BizTalki teenused"
   description="Loomine ja konfigureerimine ruut konnektor või API rakenduse kasutamine loogika rakenduse teenuses Azure rakenduse kohta"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Õpetus: Protsessi EDIFACT arved Azure'i BizTalki teenuste kaudu
Saate konfigureerida ja juurutada X12 ja EDIFACT lepingute BizTalki Services portaal. Selles õpetuses vaatame EDIFACT lepingu vahetamise arvete vahel kaubanduspartneritega loomise kohta. Selles õpetuses on kirjutatud-lõpuni ärilahenduse seotud kaks kaubanduspartnerit vahetada EDIFACT sõnumeid põhjatuul ja Contoso ümber.  

## <a name="sample-based-on-this-tutorial"></a>Valimi põhjal õppeteema
Selles õpetuses on kirjutatud ümber valimil **Saatmine EDIFACT arvete abil BizTalki teenuseid**, mis on saadaval [MSDN-i kood galeriis](http://go.microsoft.com/fwlink/?LinkId=401005)alla laadida. Võite kasutada valimi ning selle õpetuse mõista, kuidas valimi ehitatud läbida. Või võite kasutada selles õpetuses luua oma lahenduse maa-up. Selles õpetuses on suunatud teise lähenemisviisi nii, et teil mõista, kuidas see lahendus on ehitatud. Lisaks võimalikult palju, õpetuse vastab valimi ja kasutab sama nimega artefakte (nt skeemid, teisendusteta) kasutatud valimis.  

>[AZURE.NOTE] Kuna see lahendus hõlmab on EDI silla mõne EAI sillalt sõnumi saatmist, kasutab uuesti [BizTalki teenuste Bridge Aheldamise valimi](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) valimi.  

## <a name="what-does-the-solution-do"></a>Mida teeb lahenduse?

See lahendus, Põhjatuule saab EDIFACT arvete contoso. Nende arvete pole EDI standardne vorming. Nii, et enne saatmist arve põhjatuul, see tuleb muuta EDIFACT arve (nimetatakse ka INVOIC) dokumendile. Saamisel põhjatuul peab töötlemine EDIFACT arve ja tagastab Contoso juhtelemendi sõnumi (nimetatakse ka CONTRL).

![][1]  

Selle stsenaariumi business saavutamiseks Contoso kasutab Microsoft Azure'i BizTalki teenustega saadaolevad funktsioonid.

*   Contoso kasutab EAI sillad algse arve EDIFACT INVOIC muuta.

*   EAI silla saadab sõnumi EDI saada bridge juurutatud konfigureeritud BizTalki teenuste portaalis lepingu raames.

*   EDI saada silla töötleb EDIFACT INVOIC ja marsruudib see põhjatuul.

*   Pärast arve, põhjatuul annab vastuseks CONTRL sõnumi EDI vastu võtta kasutusele võetud lepingu bridge.  

> [AZURE.NOTE] Soovi korral võite seda lahendust ka demonstreerib partiide saata arved pakkidena iga arve eraldi saatmise asemel.  

Stsenaarium lõpuleviimiseks kasutame teenuse siini järjekorrad Contoso arve saata põhjatuul või kinnituse saadud põhjatuul. Need järjekorrad saab luua klientrakendusega, millele on allalaadimiseks saadaval valimi pakett, mis on saadaval selle õpetuse osana.  

## <a name="prerequisites"></a>Eeltingimused

*   Teenuse siini nimeruum peab teil olema. Nimeruumi loomise juhised leiate teemast [Kuidas: loomine või muutmine teenuse siini teenuse Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Oletame, et teil on juba ette valmistatud, teenuse siini nimeruumi nimetatakse **edifactbts**.

*   Teil peab olema BizTalki teenuste tellimus. Juhised leiate teemast [loomine BizTalki teenuse abil Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=302280). Selles õpetuses mõeldud Oletame teil on BizTalki teenuste tellimus, nimetatakse **contosowabs**.

*   Tellimuse BizTalki teenuste BizTalki teenuste portaalis registreerida. Juhised leiate teemast [registreerimisel BizTalki teenuse BizTalki portaalis teenuste juurutamine](https://msdn.microsoft.com/library/hh689837.aspx)

*   Peab teil olema installitud Visual Studio.

*   Peab teil olema installitud BizTalki teenuste SDK. Saate alla laadida SDK [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Samm 1: Looge teenuse siini järjekorrad  
See lahendus kasutab teenuse siini järjekorrad Exchange sõnumite kaubanduspartnerite vahel. Contoso ja põhjatuul saata sõnumeid kohale, kus EAI ja/või EDI sillad kasutada neid. See lahendus, tuleb teil kolm teenuse siini järjekorrad:

*   **northwindreceive** – põhjatuul saab arve contoso selle järjekorra üle.

*   **contosoreceive** – Contoso saab kinnitamist põhjatuul selle järjekorra üle.

*   **peatatud** – kõik peatatud marsruuditakse sõnumite selles järjekorras. Sõnumite peatatakse, kui nad ei töötlemise ajal.

Valimi paketis kliendi rakenduse abil saate luua nende teenuste siini järjekorrad.  

1.  Kui laadisite valimi asukohast, avage **Õpetuse saata arved abil BizTalki teenuste EDI Bridges.sln**.

2.  Vajutage klahvi **F5** luua ja käivitada **Kliendi õpetuse** rakendust.

3.  Sisestage sisselogimiskuval teenuse siini ACS nimeruumi, väljaandja nimi ja väljaandja võti.

    ![][2]  
4.  Teateboks, mis palub kolme järjekorrad luuakse teie teenuse siini nimeruumi. Klõpsake nuppu **OK**.

5.  Jätke õpetuse klient töötab. Avatud, klõpsake **Teenuse siini** > **_oma teenuse siini nimeruum_** > **järjekorrad**, ja kontrollige kolme järjekorrad loodud.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Samm 2: Luua ja kasutusele võtta kauplemise partner leping
Looge kauplemise partneri lepingu Contoso ja põhjatuul vahel. Kauplemise partneri lepingu määratleb trade lepingu kaks äripartnerite, nt milliseid sõnumi skeemi kasutada, millist sõnumside protokolli jne vahel. Kauplemise partner leping sisaldab kaks EDI sillad, üks kaubanduspartneritega (ehk **EDI saada bridge**) sõnumeid saata ja üks sõnumeid kaubanduspartneritelt (ehk **bridge EDI vastuvõtmine**).

See lahendus kontekstis EDI saada silla vastab saada servas lepingu ja kasutatakse Contoso EDIFACT arve saatmiseks põhjatuul. Samuti EDI vastuvõtu silla vastab vastuvõtu servas lepingu ja kasutatakse vastuvõtu kinnitused põhjatuul.  

### <a name="create-the-trading-partners"></a>Kaubanduspartnerite loomine

Et alustada, luua kaubanduspartneritega Contoso ja põhjatuul.  

1.  BizTalki portaalis, menüü **partnerid** , klõpsake nuppu **Lisa**.

2.  Uue partneri lehel sisestage **Contoso** partneri nimi ja klõpsake siis nuppu **Salvesta**.

3.  Korrake toimingut luua teine partner, **põhjatuul**.  

### <a name="create-the-agreement"></a>Lepingu loomine
Kauplemise partneri lepingute luuakse äri profiilid börsipäev partnerite vahel. See lahendus kasutab vaikimisi partneri profiilid, mis luuakse automaatselt partnerite loomisel.  

1.  BizTalki Services portaal, klõpsake **lepingute** > **lisamine**.

2.  Klõpsake lehel uue lepingu **Üldsätted** kasutatavate väärtuste määramiseks, nagu on näidatud järgmisel pildil ja seejärel klõpsake nuppu **Jätka**.

    ![][3]  

    Kui klõpsate nuppu **Jätka**, kahest järgmisest, **Sätete vastu võtta** ja **Saata sätete** muutuvad kättesaadavaks.

3.  Looge Contoso saada vaheline põhjatuul. Käesolev leping reguleerib seda, kuidas Contoso saadab EDIFACT arve põhjatuul.

    1.  Klõpsake käsku **saada sätted**.

    2.  **Sissetulev URL**, **muuta**ja **Batching** vahekaartidel vaikeväärtused säilitada.

    3.  Laadige menüü **Protocol (protokoll)** jaotises **skeemid** **EFACT_D93A_INVOIC.xsd** skeemi. Selle skeemi on valimi paketi saadaval.

        ![][4]  
    4.  Määrake vahekaardil **Transport** teenuse siini järjekorrad üksikasjad. Saada-side lepingu, kasutatakse **northwindreceive** kuhjuda EDIFACT arve saatmiseks põhjatuul ja kõik sõnumid, mis ei pruugi töötlemise ajal ja on peatatud marsruutimiseks **peatatud** järjekorda. Olete loonud need järjekorrad **Samm 1: loomine teenuse siini järjekorrad** (selles teemas).

        ![][5]  

        Klõpsake jaotises **Transport sätted > Transport tüüp** ja **sõnumi peatamine sätted > Transport tüüp**, valige Azure'i teenus siini ja sisestage väärtused, nagu pildil näidatud.

4.  Looge Contoso vastuvõtu vaheline põhjatuul. Käesolev leping reguleerib seda, kuidas Contoso saab kinnitamist põhjatuul.

    1.  Klõpsake nuppu **saada sätted**.

    2.  Säilita vaikeväärtused vahekaartidel **Transport** ja **muuta** .

    3.  Laadige menüü **Protocol (protokoll)** jaotises **skeemid** **EFACT_4.1_CONTRL.xsd** skeemi. Selle skeemi on valimi paketi saadaval.

    4.  Klõpsake vahekaarti **protsess** tagamaks, et ainult kinnitused: põhjatuul marsruuditakse Contoso filtri loomine. Klõpsake jaotises **Marsruutimiseks sätted**, **Lisa** marsruutimise filtri loomiseks.

        ![][6]  
        1.  Sisestama väärtused **Reegli nimi**ja **marsruutimiseks reeglite** **marsruutimiseks sihtkoht** nagu pildil näidatud.

        2.  Klõpsake nuppu **Salvesta**.

    5.  Määrake menüüs **marsruutimiseks** uuesti, kus marsruuditakse peatatud kinnitused (kinnitused nurjuvad töötlemise ajal). Azure'i teenus siini transport tüübi määramine, sihtkoha tüüp marsruutida **järjekorda**, autentimistüüp **Ühiskasutusse Accessi allkirja** (SAS), SAS ühendusstringi ette teenuse siini nimeruumi, ja sisestage järjekorra nimi on **peatatud**.

5.  Lõpuks käsku **Deploy** lepingu juurutamiseks. Pange tähele lõpp-punktid kui saatmine ja vastuvõtmine lepinguid ei saa juurutada.

    *   Märkus vahekaardil **Saata sätted** jaotises **Sissetulev URL-i**lõpp-punkti. Contoso sõnumi saatmiseks põhjatuul abil EDI saada silla peate saatma sõnumi selle lõpp-punkti.

    *   Märkus **Vastu sätted** jaotises **Transport**, lõpp-punkti. Sõnumi saatmiseks põhjatuul Contoso EDI abil vastu bridge, peate saatma sõnumi selle lõpp-punkti.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Samm 3: Luua ja juurutada BizTalki teenuste projekt

Eelmises etapis juurutatud EDI saatmine ja vastuvõtmine lepingute EDIFACT arved ja kinnitused töötlemine. Need lepingud saab ainult protsessi sõnumid, mis vastavad standard EDIFACT sõnum skeemi. Siiski seda lahendust stsenaariumi kohta Contoso saadab arve põhjatuul majasisesed kaitstud skeemiga. Nii, et enne sõnumi saatmist EDI saada sillale see peab ümber ettevõttesiseseid skeemi standard EDIFACT arve skeemi. Projekti BizTalki teenuste EAI ei, et.

BizTalki teenuste projekti **InvoiceProcessingBridge**, mis muudab sõnum on ka allalaaditud valimi kaasatud. Projekt sisaldab järgmisi esemeid.

*   **INHOUSEINVOICE. XSD** – skeemi ettevõttesiseseid arve, mis saadetakse põhjatuul.

*   **EFACT_D93A_INVOIC. XSD** – skeemi standard EDIFACT arve.

*   **EFACT_4.1_CONTRL. XSD** – skeemi põhjatuul saadab Contoso EDIFACT kättesaamise.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – transformatsioon, mis kaartide ettevõttesiseseid arve skeemi standard EDIFACT arve skeemi.  

### <a name="create-the-biztalk-services-project"></a>BizTalki teenuste projekti loomine
1.  Laiendage InvoiceProcessingBridge projekti lahenduse Visual Studio ja seejärel **MessageFlowItinerary.bcs** faili avada.

2.  Klõpsake suvalist lõuendile ja määrake **BizTalki teenuse URL-i** atribuudiväljale määramiseks oma BizTalki teenuste tellimuse nime. Näiteks `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Lohistage tööriistakastist, lõuend on **XML-i One-Way Bridge** . **Isiku nimi** ja **Suhteline aadress** silla atribuutide määramine **ProcessInvoiceBridge**. Topeltklõpsake **ProcessInvoiceBridge** bridge konfiguratsiooni pind avamiseks.

4.  **Sõnumitüübid** kasti, klõpsake plussmärki (**+**) nuppu, et määrata skeemiga sissetuleva sõnumi. Kuna sissetuleva sõnumi EAI Bridge on alati ettevõttesiseseid arve, määrake see **INHOUSEINVOICE**.

    ![][8]  
5.  Klõpsake kujundit, **XML-i transformatsioon** ja atribuudiväljale **kaartide** atribuudile, klõpsake nuppu kolmikpunkti (…****). **Kaartide valiku** dialoogiboksis valige **INHOUSEINVOICE_to_D93AINVOIC** transformatsioon fail ja seejärel klõpsake nuppu **OK**.

    ![][9]  
6.  Minge tagasi **MessageFlowItinerary.bcs**ja tööriistakastist, lohistage soovitud **Kahesuunalise välise teenuse lõpp-punkti** **ProcessInvoiceBridge**paremal. Määrake selle **Isiku nimi** atribuut **EDIBridge**.

7.  Solution Exploreris, laiendage **MessageFlowItinerary.bcs** ja topeltklõpsake faili **EDIBridge.config** . Asendage **EDIBridge.config** sisu järgmist.

    > [AZURE.NOTE] Miks on vaja .config faili redigeerida? Välise teenuse lõpp-punkti, mille lisasime bridge kujundaja lõuend tähistab EDI sillad, mida me kasutusele varasemas versioonis. EDI sillad on kahesuunaline sildade, saata ja vastu võtta pool. Siiski EAI bridge, mille lisasime bridge designer on on ühesuunalise. Nii, et erineva meilisõnumi Exchange'i mustrite kaks sildade käsitlema kasutame kohandatud bridge käitumine, sh selle konfiguratsiooni .config faili. Lisaks kohandatud käitumine töötleb ka autentimise EDI saada bridge lõpp-punkti. Selle kohandatud käitumine on saadaval veebisaidil [BizTalki teenuste Bridge Aheldamise näidis - EAI EDI](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104)eraldi valimi. See lahendus kasutab proovi uuesti.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Värskendada EDIBridge.config faili lisada konfiguratsiooni üksikasjad

    *   Klõpsake jaotises _<behaviors>_, ACS nimeruum ja BizTalki teenuste tellimusega seostatud võti.

    *   Klõpsake jaotises _<client>_, sisestage lõpp-punkt, kus on juurutatud EDI saada lepingu.

    Salvestage muudatused ja sulgege konfiguratsiooni faili.

9.  Tööriistakastist, klõpsake **konnektor** ja liitumine **ProcessInvoiceBridge** ja **EDIBridge** komponendid. Valige konnektor ja atribuutide dialoogiboksis seatud **Filtri tingimuse** **Vaste kõik**. See tagab, et töödelda EAI silla kõigi sõnumite marsruuditakse EDI silla.

    ![][10]  
10.  Muudatuste salvestamiseks lahendus.  

### <a name="deploy-the-project"></a>Projekti juurutamine

1.  Arvutites, kus loodud BizTalki teenuste projekt, allalaadimine ja installimine SSL-serdi tellimuse BizTalki teenuseid. Jaotises BizTalki teenused, valige **armatuurlaud**, ja klõpsake **Allalaadimine SSL-sert**. Topeltklõpsake serti ja küsimuse installimise lõpuleviimiseks järgige. Veenduge, et jaotises **Trusted Root sertimiskeskuste** sert poe sert.

2.  Visual Studio lahenduste Explorer, paremklõpsake **InvoiceProcessingBridge** projekti ja seejärel käsku **Deploy**.

3.  Sisestage väärtused, nagu pildil näidatud ja seejärel käsku **Deploy**. Saate ACS mandaadi BizTalki teenuste jaoks, klõpsates **Ühendusteabe** BizTalki teenuste armatuurlaua kaudu.

    ![][11]  

    Kopeerimiseks paanilt väljund lõpp-punkt, kus EAI silla juurutatakse, näiteks `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Seda URL-i lõpp-punkti on hiljem vaja.  

## <a name="step-4-test-the-solution"></a>Samm 4: Testige lahendus


Selles teemas vaatame testimine lahendus valimi osana **Kliendi õpetuse** rakenduse abil.  

1.  Visual Studio, vajutage klahvi F5 käivitage **Kliendi õpetuse**.

2.  Kuva peab olema eelnevalt asustatud juhises väärtused, kus oleme loodud teenuse siini järjekorrad. Klõpsake nuppu **edasi**.

3.  Järgmise akna, sisestage ACS mandaat BizTalki teenuste tellimus ja lõpp-punktid kui EAI ja elektrooniline (vastuvõtmine) sillad on juurutatud.

    Kopeeritud oli EAI bridge lõpp-punkt eelmises etapis. EDI vastuvõtmine bridge lõpp-punkti, BizTalki teenuste portaali, avage lepingu > saada sätted > Transport > lõpp-punkti.

    ![][12]  
4.  Järgmise akna Contoso, klõpsake nuppu **Saada majasisesed arve** . Faili dialoogiboksi avamine, avage INHOUSEINVOICE.txt fail. Uurida faili sisu ja seejärel klõpsake nuppu **OK** , et saada arve.

    ![][13]  
5.  Mõne sekundi arve pruugi põhjatuul. Saadud põhjatuul arve vaatamiseks linki **Kuva sõnum** . Pange tähele, kuidas arve saadud põhjatuul on EDIFACT skeemi ajal saadetud Contoso üks oli ettevõttesiseseid skeemiga.

    ![][14]  
6.  Valige arve ja seejärel käsku **Saada kinnitus**. Kuvatavas dialoogiboksis Pange tähele, et andmevahetuse ID on sama saadud arve ja kinnituse, saadetakse. **Saata kinnituse** dialoogiboksis nuppu OK.

    ![][15]  
7.  Mõne sekundi, kinnitamist edukalt ei pruugi Contoso.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Juhis 5 (valikuline): saatmine EDIFACT arve pakettidena 
BizTalki teenuste EDI sillad toetab samuti partiide väljaminevate sõnumite. See funktsioon on kasulik partnerid, mis soovi saada paketi sõnumite (teatud kriteeriumile) asemel üksikutele sõnumitele.

Kõige olulisem pakettidena töötamisel on tegeliku vabastamise paketi, nimetatakse ka väljaanne kriteeriumid. Väljalaske kriteeriumid põhinema kuidas vastuvõtmise partneri soovib sõnumeid. Kui partiide on lubatud, EDI silla ei saada väljamineva sõnumi vastuvõtmise partneri kuni väljaanne kriteeriumid on täidetud. Näiteks partiide kriteeriumide alusel sõnumi suurus vedamine partii ainult siis, kui 'n' sõnumid on batched. Paketi kriteeriumid võib olla ajapõhiste, mis rahuldab partii saadetakse kindlaksmääratud ajal iga päev. See lahendus on Proovime sõnumi suurus vastavalt kriteeriumid.

1.  Klõpsake portaalis teenuste BizTalki varem loodud lepingu. Klõpsake nuppu Saada sätted > partiide > paketi lisada.

2.  Töölehe nimi, sisestage **InvoiceBatch**kirjeldama, ja seejärel klõpsake nuppu **edasi**.

3.  Määrake paketi kriteeriumid, mis määratleb, milliseid sõnumeid peab batched. See lahendus me partii kõigile sõnumitele. Jah, märkige ruut Kasuta Täpsemad määratlused suvandit ja sisestage **1 = 1**. See on tingimus, mis on alati täidetud ja seega on batched kõigile sõnumitele. Klõpsake nuppu **edasi**.

    ![][17]  
4.  Määrata paketi väljaanne kriteeriumid. Valige **MessageCountBased**drop-välja ja **Count**, määratlege **3**. See tähendab, et paketi kolme sõnumi saadetakse põhjatuul. Klõpsake nuppu **edasi**.

    ![][18]  
5.  Kokkuvõte ja klõpsake siis nuppu **Salvesta**. Klõpsake käsku **Deploy** abil, ümberkorraldamine lepingu.

6.  Avage **Kliendi õpetuse**, klõpsake nuppu **Saada ettevõttesiseseid arve**ja järgige viipasid saata arve. Te teate arve vastuvõetud põhjatuul veebisaidil, kuna partii suurust ei ole täidetud. Korrake seda toimingut kaks korda, et teil on kolm arve sõnumite saatmiseks põhjatuul. See kriteeriumidele paketi väljaanne 3 sõnumite ja nüüd peaks nähtaval olema arve veebisaidil põhjatuul.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

