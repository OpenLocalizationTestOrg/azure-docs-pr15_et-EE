<properties
   pageTitle="EAI loogika rakenduse abil VETR loogika Azure'i rakendust Service rakenduste loomine | Microsoft Azure'i"
   description="Kinnitage, kodeerida ja muuta BizTalki XML services funktsioonid"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>EAI loogika rakenduse abil VETR loomine

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Enamik Enterprise integreerimine (EAI) stsenaariumid olla vahendajaks andmete allikas ja sihtkoht vahel. Sageli on sellised stsenaariumid ühtsed nõuded.

- Veenduge, et eri andmed on õigesti vormindatud.
- "Saate" teha sissetulevate andmete otsuseid langetada.
- Andmete teisendamine ühest vormingust teise. Näiteks andmete teisendamine CRM süsteemi andmete vormingust ERP süsteemi andmete vormindamine.
- Marsruutimine soovitud rakendus või süsteemi andmed.

Selles artiklis kirjeldatakse levinud integreerimine mustri: "ühesuunalise sõnumi vahendustegevuse" või VETR (Valideeri Enrich, transformatsioon, marsruutimiseks). VETR mustri vahendab andmete allikas üksus ja sihtkoha üksuse vahel. Lähte-kui ka on tavaliselt andmeallikad.

Kaaluge veebisait, mida aktsepteerib tellimused. Kasutajad postitavad tellimused süsteemi HTTP kaudu. Varjatult süsteemi sissetulevate andmete õigsust kinnitatakse, normaliseerib selle ja ei lahene, see teenus siini järjekorras edasiseks töötlemiseks. Süsteemi võtab tellimusi välja järjekorda, oodanud seda kindlas vormingus. Seega-lõpuni vool on:

**HTTP** → **kinnitage** → **muuta** → **teenuse siini**

![Põhilised VETR kulgemist][1]

Järgmised BizTalki API rakendused abiks olla selle mustri koostamine.

* **HTTP päästik** - allikas, et käivitada sõnumi sündmuse
* **Kontrolli** – sissetulevate andmete õigsust kinnitatakse
* **Transform** - teisendusi sissetulevad vorming vorming vastavalt järgmise etapi süsteemi andmed
* **Siini konnektori** - sihtkoha üksus, kus andmed on saadetud


## <a name="constructing-the-basic-vetr-pattern"></a>Põhiline VETR muster ehitamine
### <a name="the-basics"></a>Põhitõed

Azure'i portaalis valige **+ Uus**, valige **Web + Mobile**ja valige **Loogika rakendus**. Valige nimi, asukoht, tellimus, ressursirühm ja asukohta, mis töötab. Ressursi rühmad toimivad rakenduste; ümbriste kõik ressursid oma rakenduse minge ühte ressursirühma.

Järgmiseks loome päästikute ja toimingute lisamine.


## <a name="add-http-trigger"></a>HTTP päästik lisamine
1. Valige **Loogika appi mallide** **loomine algusest peale**.
1. Valige **HTTP kuulajale** luua uue kuulajale galeriist. Kõne see **HTTP1**.
2. Määrake soovitud **automaatselt vastuse saata?** seada väärtuseks väär. Käivitab toimingu sätte _HTTP meetod_ _postitusse_ ja sätet _Suhteline URL_ -i _/OneWayPipeline_konfigureerida.  
    ![HTTP päästik][2]
3. Valige roheline märge päästiku lõpuleviimiseks.

## <a name="add-validate-action"></a>Lisage toimingu valideerimiseks

Nüüd sisestage vaatame toiminguid, mis käivitab käivitub käivitamise – see tähendab, siis, kui kõne on saanud HTTP lõpp-punkti.

1. **BizTalki XML-i süntaksi** lisamine galeriist ja pange sellele eksemplari loomine _(Validate1)_ .
2. XSD-skeemile XML-i sissetulevate sõnumite valideerimiseks konfigureerimine. Valige _triggers('httplistener').outputs _kontrolli_ toimingu valimiseks. Sisu_ _inputXml_ parameetri väärtus.

Nüüd kontrolli toiming on pärast HTTP kuulajale esimene toiming: 

![BizTalki XML-i süntaksi][3]

Samuti lisada ülejäänud toimingud. 

## <a name="add-transform-action"></a>Teisenduse toimingu lisamine
Konfigureerige teisendusteta normaliseeritav sissetulevad andmed.

1. Lisage **BizTalki muuta teenuse** galeriist.
2. Muuta sissetulevate sõnumite XML-i transformatsioon konfigureerimiseks valige toiming, mis tuleks teha, kui see API nimetatakse **muuta** toimingu. Valige ```triggers(‘httplistener’).outputs.Content``` _inputXml_väärtusena. *Kaardi* on valikuline parameeter, kuna sissetulevate andmete vastendamise koos kõigi konfigureeritud teisendusteta, ning need rakendatakse ainult need, mis vastavad skeemiga.
3. Lõpetuseks transformatsioon töötab ainult kui Valideeri õnnestus. See tingimus konfigureerimiseks valige klõpsake paremas ülanurgas hammasrattaikooni ja valige _Lisa tingimus on täidetud_. Määratud tingimusele ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![BizTalki Teisendusteta][4]


## <a name="add-service-bus-connector"></a>Siini Konnektori lisamine
Järgmiseks lisame sihtkoht – teenus siini järjekorda – andmete kirjutamiseks.

1. Lisage **Siini konnektori** galeriist. Määratud **nimi** _Servicebus1_, **Ühendusstringi** määramine teie siini eksemplari ühendusstringi, **Isiku nimi** _järjekorda_seadmine ja vahele jätta **tellimuse nime**.
2. **Sõnumi saatmine** toimingu valimiseks ja määrake soovitud toimingu **sisu** väljal _actions('transformservice').outputs. OutputXml_.
3. Määrata välja **Sisutüüp** *rakendus-xml*.  

![Teenuse siini][5]


## <a name="send-http-response"></a>HTTP vastuse saatmine
Kui müügivõimaluste töötlemine on lõpule jõudnud, saatke tagasi HTTP vastuse nii edu ja tõrge, tehke järgmist:

1. Valige **HTTP vastuse saata** toimingu lisada mõne **HTTP kuulajale** galeriist.
2. Määrake **Vastuse ID** *sõnumi*saatmiseks.
2. Seadke **Vastuse sisu** *müügivõimaluste töötlemine on lõpule viidud*.
3. **Vastuse tähis** *200* tähistamiseks HTTP 200 OK.
4. Valige rippmenüüst klõpsake paremas ülanurgas ja valige **Lisa tingimus on täidetud**.  Määrake tingimus järgmine avaldis:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Korrake neid juhiseid ka tõrke kohta HTTP vastuse saatmiseks. Muuda **tingimust** järgmine avaldis:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Valige **OK** seejärel **loomine**.



## <a name="completion"></a>Lõpuleviimine
Iga kord, kui keegi saadab sõnumi HTTP lõpp-punkti, käivitab rakenduse ja käivitab äsja loodud toimingud. Sellise loogika rakenduste haldamiseks saate luua Azure'i portaalis valige **Sirvi** ja valige **Loogika rakendused**. Lisateabe saamiseks rakenduse valimine

Mõned kasulikud Teemad:

[Hallata ja jälgida oma API rakendused ja konnektorid](app-service-logic-monitor-your-connectors.md)  <br/>
[Loogika rakenduste jälgimine](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
