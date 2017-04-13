<properties
   pageTitle="Azure'i töötab ülevaade | Microsoft Azure'i"
   description="Mõista, kuidas optimeerida asünkroonne töökoormus minutiga Azure'i funktsioonide abil."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure'i funktsioone, funktsioonide, event töötlus, webhooks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Azure'i funktsioonide ülevaade

Azure'i funktsioonid on lahendus hõlpsalt töötab small tekstilõigule koodi või "funktsioone," pilveteenuses. Saate kirjutada lihtsalt koodi, peate selle probleemi kohta, ilma muretsema kogu rakenduse või infrastruktuuri käivitage see. Seda saab teha arengu tööviljakust ja abil saate oma arengu keele valik, nt C#, F #, Node.js, Python või PHP. Maksma ainult aega käivitub teie kood ja usaldusväärsus Azure skaala vastavalt vajadusele.

Selles teemas antakse üksikasjalik ülevaade Azure'i funktsioonid. Kui soovite hüpata paremale ja Azure funktsioonide kasutamise alustamine, alustage [Loo oma esimese Azure'i funktsioon](functions-create-first-azure-function.md). Kui otsite tehnilisemat teavet funktsioonide kohta, lugege teemat [tootearendusmaterjal](functions-reference.md).

## <a name="features"></a>Funktsioonid

Siin on mõned põhijooned Azure'i funktsioonid.
    
* **Keele valik** - C#, F #, Node.js, Python, PHP, paketi, bash, Java või mis tahes käivitatava kirjutamine funktsioonid.
* **Maksma-kasutada hinnakirjad mudel** - ainult maksma oma koodi käivitamist kulunud aeg. Ei näe dünaamiline rakenduse teenuse leping [hinnad jaotise](#pricing) all.  
* **Tuua oma sõltuvused** - funktsioonide toetab Nugeti ja NPM, abil saate oma lemmik teekide.  
* **Integreeritud turvalisus** - OAuthi pakkujad, nt Azure Active Directory, Facebooki, Google, Twitteri ja Microsofti Account kaitse HTTP-käivitub funktsioonid.  
* **Lihtsustatud integreerimine** - hõlpsalt kasutada Azure teenuste ja tarkvara-kui-a-service (SaaS) pakkumisi. Teavet leiate [integratsioon jaotise](#integrations) all mõned näited.  
* **Paindlik arengu** - kood oma funktsioonide otse portaalist või pidev integreerimise seadistamine ja juurutamine oma kood GitHub, Visual Studio meeskonnatöö teenuste ja muude [toetatud arengu tööriistade](../app-service-web/web-sites-deploy.md#deploy-using-an-ide)kaudu.  
* **Avatud allika** - funktsioonide käitusaja on avatud lähtekoodi ja [github saadaval](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Mida saab teha funktsioonidega?

Azure'i funktsioonid on suurepärane lahendus andmete töötlemise integreerimise numbritelt, mis on Interneti-ja-asjade töötamise ja lihtsa API-d ja microservices. Kaaluge funktsioone, nt pildi või tellimuse töötlemine, faili hooldus pikaajalisi tööülesandeid, mida soovite käivitada taust teemas või kõik toimingud, mida soovite käivitada ajakava. 

Funktsioonide pakub mallide kasutamise alustamine loetletud tähtsaimad stsenaariumid, sh järgmist:

* **BlobTrigger** - protsess Azure Storage plekid, kui need on lisatud ümbriste. Pildi suuruse muutmiseks võite seda kasutada.
* **EventHubTrigger** - vastamine sündmused, mis on esitatud Azure sündmuse jaoturiga. Eriti kasulik rakendus instrumentation, kasutaja kogemus või töövoo töötlemise ja Internet, asjade stsenaariumid.
* **Üldise webhook** - protsess webhook HTTP taotleb teenus, mis toetab webhooks.
* **GitHub webhook** - vastamine oma GitHub hoidlate sündmused. Näiteks vt [loomine on webhook või API funktsiooni](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - päästik täitmise oma kood, kasutades HTTP-päring.
* **QueueTrigger** - vastamine sõnumite saabumisel ja Azure Storage. Näide, leiate teemast [Azure funktsioon, mis mis seob Azure teenuse loomine](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - ühenduse muude Azure'i teenuste oma koodi või kohapealne teenuste teatejärjekordi kuulata. 
* **ServiceBusTopicTrigger** - ühenduse muude Azure'i teenuste oma koodi või kohapealne teenuste teemade tellimise kaudu. 
* **TimerTrigger** - käivitada Kettapuhastus või muude paketi tööülesannete eelmääratletud ajakava. Näiteks vt [sündmuse töötlemiseks funktsioon loomine](functions-create-an-event-processing-function.md).

Azure'i funktsioonide toetab *päästikute*, mis on teie koodi käivitamise käivitamise võimalustega, ja *sidumiste*, mis on kodeerimine sisend- ja andmete lihtsustamine viisid. Päästikute ja Azure funktsioone sisaldava sidumiste üksikasjaliku kirjelduse leiate [Azure'i funktsioonide päästikute ja sidumiste tootearendusmaterjal](functions-triggers-bindings.md).


## <a name="integrations"></a>Integratsioon

Azure'i funktsioonide ühendab erinevaid Azure ja 3. tootja teenused. Saate need oma funktsioon käivitamine ja käivitage täitmine või kasutada sisendina ja väljund koodi. Järgmised teenuse integratsioon ei toeta Azure funktsioonid. 

* Azure'i DocumentDB
* Azure'i sündmuse jaoturi 
* Azure'i mobiilirakenduste (tabelid)
* Azure'i teatis jaoturi
* Azure'i teenus siini (järjekorrad ja teemade)
* Azure'i salvestusruumi (bloobimälu, järjekorrad ja tabelid) 
* GitHub (webhooks)
* Kohapealse (abil teenuse siini)

## <a name="pricing"></a>Kui palju maksab maksumus töötab?

Azure'i funktsioonid on kahte tüüpi hinnad lepingud, valige üks, mis sobib kõige paremini teie vajadustele. 

* **Dünaamiliste majutusteenuse leping** – kui teie funktsioon töötab, Azure pakub kõik vajalikud arvutuslik ressursid. Te ei pea muretsema ressursside haldamine ja maksate aega, mida teie kood töötab. Kõik hinnakirjad üksikasjad on [funktsioonide hinnad lehe](/pricing/details/functions). 

* **Rakenduse teenusleping** - oma funktsioone nagu teie web, mobile ja API rakendusi käivitada. Kui kasutate rakendust Service juba teie taotlused, saate kasutada sama lepingu täiendava tasuta oma funktsioonid. Täielikud üksikasjad leiate [rakenduse teenuse hinnad lehe](/pricing/details/app-service/).

Skaleerimist oma funktsioonide kohta leiate lisateavet teemast [mastaapimiseks Azure'i funktsioonid](functions-scale.md).

##<a name="next-steps"></a>Järgmised sammud

+ [Oma esimese Azure'i funktsioon loomine](functions-create-first-azure-function.md)  
Hüpata paremale ja looge oma esimese funktsioon Kiirjuhend Azure'i funktsioonide abil. 
+ [Azure'i funktsioonide tootearendusmaterjal](functions-reference.md)  
Pakub tehnilisemat teavet Azure'i funktsioonide käitusaja ja viide kodeerimine funktsioonid ja määratlemine päästikute ja seosed.
+ [Azure'i funktsioonide testimine](functions-test-a-function.md)  
Kirjeldatakse mitmesuguste tööriistad ja nippidega, mis aitavad teie funktsioonide testimine.
+ [Kuidas mastaapimiseks Azure funktsioonid](functions-scale.md)  
Käsitletakse teenuse lepingute Azure'i funktsioonidega dünaamiline teenusleping, ja kuidas valida õige leping saadaval. 
+ [Lisateavet teenuse Azure rakenduse kohta](../app-service/app-service-value-prop-what-is.md)  
Azure'i funktsioonide mõjutab Azure'i rakendust Service platvormi põhifunktsioone, nagu juurutuste, keskkonna muutujate ja diagnostika. 