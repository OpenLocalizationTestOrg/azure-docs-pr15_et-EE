<properties 
    pageTitle="Ettevõtte integreerimine ülevaade | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
    description="Ettevõtte integreerimise funktsioone abil saate lubada protsess ja integreerimise äriprotsessid loogika rakenduste kasutamine" 
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
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Ettevõtte integreerimine pakett ülevaade

## <a name="what-is-the-enterprise-integration-pack"></a>Mis on Enterprise integreerimine pakett?
Ettevõtte integreerimine pakett on Microsofti pilvepõhise lahendus sujuvalt lubamine business-to-business (B2B) suhtlus. Pakett kasutab valdkonna standard protokollid, sh [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)ja [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) Exchange sõnumite äripartnerite vahel. Sõnumite saate soovi korral turvatud digitaalallkirju ja krüptimise abil. 

Ettevõtted, mis kasutavad erinevaid protokolle võimaldab ja vormingud vahetada sõnumite elektrooniliselt muutes vorminguid nii organisatsioonide süsteemide saate tõlgendamine ja tegutseda vormingusse. 

Kui olete tuttav BizTalki serveri või Microsoft Azure'i BizTalki teenuseid, leiate selle lihtsa Enterprise integreerimise funktsioonid, kuna enamik mõisted on sarnased. Üks suur erinevus on, et ettevõtte integreerimine kasutab integreerimine kontod lihtsustamiseks salvestamine ja haldamine esemeid B2B suhtlemiseks kasutada. 

Arhitektuuriliselt, põhineb Enterprise integreerimine pakett talletada esemeid, mida saab kasutada kujundamine, juurutada ja hallata oma B2B rakenduste **integreerimise kontod** . Konto integreerimine on põhimõtteliselt pilvepõhist container, kus saate talletada esemeid, nt skeemid, partnerid, serdid, kaardid ja lepingud. Neid esemeid saab kasutada siis loogika rakendustes koostamiseks B2B töövood. Enne kui saate kasutada esemeid loogika rakendus, peate oma konto linkimine loogika rakenduse. Pärast nende linkimist rakenduse loogika on integreeritud konto esemeid juurdepääs.  

## <a name="why-should-you-use-enterprise-integration"></a>Miks peaksin kasutama enterprise integreerimine?
- Ettevõtte integreerimise, teil on võimalik kõik teie esemeid hoida ühes kohas, kus on teie konto. 
- Saate kasutada mootori loogika rakendused ja selle konnektorid koostada B2B töövoogude ning integreerida 3 SaaS rakenduste, kohapealse apps kui ka kohandatud rakendused.
- Samuti saate kasutada Azure funktsioonid

## <a name="how-to-get-started-with-enterprise-integration"></a>Kuidas alustada enterprise integreerimise?
Saate luua ja ettevõtte integreerimine pakett kaudu loogika rakenduste kujundaja abil **Azure portaali**B2B rakenduste haldamine.  

[PowerShelli](https://msdn.microsoft.com/library/azure/mt652195.aspx "loogika rakenduste PowerShelli teemade") abil hallata loogika rakendusi. 

Siin on ülevaade sellest, peate tegema enne kui saate luua rakendusi Azure'i portaalis juhiseid: ![ülevaade pilt](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Mis on mõne levinud stsenaariumi?

Ettevõtte integreerimine toetab järgmisi tööstusstandarditele.   

- EDI - elektroonilise andmevahetuse  
- EAI – ettevõtte rakenduste integreerimine  

## <a name="heres-what-you-need-to-get-started"></a>Siin on vajalik alustamine
- Konto integreerimine Azure tellimuse
- Visual Studio 2015 loomiseks, kaardid ja skeemid
- [Microsoft Azure'i loogika rakenduste Enterprise integreerimine tööriistad Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Proovige järele
[Proovime nüüd](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) täielikult valimi AS2 saata ja vastu võtta loogika rakendus, mis kasutab funktsioone B2B loogika rakendused.

## <a name="learn-more-about"></a>Lisateave:
- [Lepingud] (./app-service-logic-enterprise-integration-agreements.md "Vaadake, kuidas lepingute enterprise integreerimine")
- [Business Business (B2B) stsenaariumid] (./app-service-logic-enterprise-integration-b2b.md "Saate teada, kuidas luua loogika rakenduste B2B funktsioonide abil")  
- [Serdid] (./app-service-logic-enterprise-integration-certificates.md "Vaadake, kuidas ettevõtte integreerimine serdid")
- [Kodeerimise ja dekodeerimise lamefailiga] (./app-service-logic-enterprise-integration-flatfile.md "Saate teada, kuidas kodeerimise ja dekodeerimise lamefaili sisu")  
- [Integreerimine kontod] (./app-service-logic-enterprise-integration-accounts.md "Vaadake, kuidas integreerimine kontod")
- [Kaardid] (./app-service-logic-enterprise-integration-maps.md "Vaadake, kuidas ettevõtte integreerimine kaardid")
- [Partnerite] (./app-service-logic-enterprise-integration-partners.md "Lugege lisateavet enterprise integreerimine partnerite kohta")
- [Skeemid] (./app-service-logic-enterprise-integration-schemas.md "Vaadake, kuidas ettevõtte integreerimine skeemid")
- [XML-i sõnumi kinnitamine] (./app-service-logic-enterprise-integration-xml.md "Saate teada, kuidas valideerimiseks XML-i sõnumite loogika rakendustega")
- [XML-i teisendus] (./app-service-logic-enterprise-integration-transform.md "Vaadake, kuidas ettevõtte integreerimine kaardid")
- [Ettevõtte integreerimine konnektorid] (../connectors/apis-list.md "Vaadake, kuidas ettevõtte integreerimine pack konnektorid")



