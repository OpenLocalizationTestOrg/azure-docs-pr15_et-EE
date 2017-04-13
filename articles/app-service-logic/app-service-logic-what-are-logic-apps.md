<properties 
    pageTitle="Mis on loogika rakendusi?" 
    description="Lisateavet rakenduse loogika rakendused" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Mis on loogika rakendusi?

Loogika rakenduste võimalda lihtsustamiseks ja rakendada scalable integratsioon ja töövoogude pilveteenuses. Pakub visuaalkonstruktor, modelleerimise ja oma töövoo tuntud etapisarja protsessi automatiseerida.  Pilveteenuste ja teenuste ja protokollid vahel kiiresti integreerida kohapealse üle on [palju konnektorid](../connectors/apis-list.md) .  Loogika rakenduse algab käivitamiseks (nagu "kui konto on lisatud Dynamics CRM-i") ja pärast süütamise saab alustada mitme kombinatsioonide toimingud dokumenditeisenduste ning tingimus loogika.

Loogika rakenduste kasutamise eelised on järgmised:  

- Säästa aega, kui loote keerukate protsesside lihtne mõista kujundusriistade abil
- Mustrite ja töövoogude sujuvalt, mis muidu oleks raske rakendada kood
- Mallide põhjal Kiire alustamine
- Kohandada rakenduse loogika oma kohandatud API-d, koodi ja toimingud
- Ühenduse loomiseks ja kohapealse ja pilveteenuse üle erinevate süsteemide sünkroonimine
- Klassi integreerimine toega off BizTalki serveri API haldus, Azure'i funktsioonid ja Azure teenuse siini koostamine

Loogika rakendused on täielikult hallatud iPaaS (integreerimine Platform teenus) võimaldab arendajatel ei pea muretsema koostamise majutusteenuse, skaleeritavus, kättesaadavus ja haldus.  Loogika rakendused kuvatakse skaalal automaatselt rahuldamiseks.

![Meilivoo rakenduse kujundaja](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Nagu mainitud, loogika apps, saate äriprotsesside automatiseerimine. Siin on mõned näited:  
 
* Teisaldada faile üles laadida FTP server Azure'i salvestusruum
* Protsess ja marsruutimiseks tellimused üle kohapealse ja pilveteenuse süsteemid
* Kõik tweets teatud teema kohta jälgida, analüüsida soovitud meeleolu ja teatiste ja tööülesannete vajavad followup üksuste loomine.

Stsenaariumid, nagu need saab konfigureerida kõik visuaalkonstruktorit ja üks rida koodi kirjutamata. Saada alustamine [koostamise loogika rakenduse nüüd][create].  Kui - loogika rakenduse saab [kiiresti juurutatud ja uuesti](app-service-logic-create-deploy-template.md) mitme keskkond ja piirkondade vahel.

## <a name="why-logic-apps"></a>Miks loogika rakendusi?

Loogika rakenduste toob töökiirusest ja skaleeritavus enterprise integreerimine ruumi.  Designer kasutuslihtsus erinevaid saadaval päästikute ja toimingute ja võimsaid Haldusriistad veenduge, et koondamise oma API-de lihtsamaks kui kunagi varem.  Kui ettevõtted liikuda digitaliseerimise, loogika Apps võimaldab teil pärand-ja tipptasemel omavahel ühendada.

Lisaks meie [Enterprise konto] koos[ biztalk] saate muudate vanemad integreerimise stsenaariumid power [XML-i sõnumside][xml], [börsipäev partnerite halduse][tpm], ja palju muud.

- **Lihtne kasutada kujundusriistade** - loogika rakendused võivad olla kujundatud--lõpuni brauseris või Visual Studio tööriistu. Käivitage käivitamiseks - kaudu lihtne graafik GitHub probleemi loomisel. Seejärel korraldab mis tahes arv toiminguid rikkaliku Galerii konnektorite abil.

- **Ühenduse API-de hõlpsalt** -isegi koosseis tööülesandeid, mida on lihtne kirjeldada on raske rakendada koodi. Loogika rakenduste abil on lihtne eraldiseisvat süsteemi ühendada. Oma pilveteenuses turunduse oma kohapealse lahenduse ühendatava arveldus süsteemi? Kas soovite koondada, sõnumside API-d ja süsteemid on Enterprise teenuse siini koos üle? Loogika rakendused on kõige usaldusväärne on kiireim viis neile probleemidele lahendusi.

- **Mallide põhjal kiiresti alustamine** - mis aitavad teil alustada oleme lisanud [Galerii malle] [ templates] mis võimaldavad kiiresti luua mõne levinud lahendusi. Täpsemalt: B2B lahendused lihtsa SaaS Ühenduvus ja isegi mõned, mis on "niisama" - Galerii on kõige kiiremini alustada loogika rakenduste power.

- **Laiendatavuse küpsetatud sisse** - peate Connectorit ei kuvata? Loogika rakendused on mõeldud töötamiseks oma API-d ja kood; Saate hõlpsasti luua oma API rakenduse kasutamiseks kohandatud konnektor või sissehelistamine [Azure'i funktsioon](https://functions.azure.com) pikad tellitavate koodi käivitada. 

- **Reaal integreerimine Hobujõud** - käivitage lihtne ja kasvab, kui teil on vaja. Loogika rakendusi saate kasutada hõlpsalt BizTalki Microsofti valdkonna ees integreerimine lahenduse integreerimine spetsialistid saavad luua lahendusi, nad power. Lisateave [Ettevõtte integreerimine keelepaketi](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Loogika rakenduse mõisted

Järgnevalt mõned peamised moodustavate loogika rakenduste kasutusvõimalusi. 

- **Töövoo** - loogika Apps võimaldab graafiline mudel äriprotsessid juhiseid või töövoo reana.
- **Hallatavate konnektorid** - rakenduste loogika on vaja juurdepääsu teenustele. Hallatavate konnektorid on loodud eelkõige abi teile, kui olete ühenduse ja andmetega töötamine. Loendi kuvamiseks ühendused, mis on nüüd saadaval [hallatavate konnektorid][managedapis].
- **Päästikute** - mõni hallatavate konnektorid võite toimida ka käivitamiseks. Käivitamiseks käivitab uue põhjal kindla sündmusega, näiteks e-posti või Azure Storage konto muudatuste saabumise töövoo eksemplari.
-  **Toimingud** - iga toimingu pärast töövoo käivitab nimetatakse toimingu. Iga toimingu kaardid tavaliselt toimingu hallatavate konnektor või kohandatud API rakendused.
- **Ettevõtte integreerimine Pack** - täpsemate integreerimise stsenaariumid, loogika rakenduste sisaldab funktsioone BizTalkist. BizTalki on Microsofti valdkonna ees integreerimine platvorm. Ettevõtte integreerimine Pack konnektorid võimaldab teil hõlpsasti lisada valideerimine, transformatsioon ja muud oma loogika rakenduse töövood.

## <a name="getting-started"></a>Alustamine  

- Loogika rakendustega alustamiseks järgige [loogika rakenduse loomine] [ create] õpetuse.  
- [Vaate levinud näited ja stsenaariumid](app-service-logic-examples-and-scenarios.md)
- [Te saate loogika rakenduste abil äriprotsesside automatiseerimine](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Saate teada, kuidas teie integreerida loogika Apps](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
