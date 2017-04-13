<properties
    pageTitle="Mis on Azure relay? | Microsoft Azure'i"
    description="Ülevaade: Azure'i edastamine"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Mis on Azure Relay?

Azure'i vahendusteenusesse hõlbustab hübriid rakenduste abil turvaliselt jätke teenuseid, mis asuvad ettevõtte ettevõtte võrgustikus avaliku pilveteenusesse, ilma et peaksite avama tulemüüri ühendus, või ettevõtte võrku taristu pealetükkivad muudatuste tegemise. Relay toetab mitmesuguseid eri transport protokollid ja web services standarditele.

Vahendusteenusesse toetab ühesuunalise, traditsioonilise taotluse/vastus ja Võrdõigusvõrk liikluse. Samuti toetab sündmuse normaaljaotus väärtusel Interneti-ulatus lubamiseks avaldamise/tellimise stsenaariumid ja kahesuunalise turvasoklite suhtlus suurema kakspunkt suurendamiseks. 

Struktuuris võrdõigusvõrku andmete võtnud emaslooma kohapealse teenuse kaudu väljaminev port vahendusteenusesse loob ja loob kahesuunalise turvasoklite suhtlemiseks seotud kindla rendezvous aadress. Kliendi siis suhelda asutusesisese teenuse saatmisega liikluse suunamise rendezvous aadressi vahendusteenusesse. Vahendusteenusesse siis "edastab" andmed kohapealse teenusele kahesuunalise turvasoklite, mis iga kliendi kaudu. Klient ei vaja asutusesisese teenuse otsene ühendus, ei pea teada, kus asub teenuse ja kohapealse teenus ei pruugi kõik sissetulevad pordid tulemüüris avatud.

Võtme võimalus elemente, mis on esitatud edastamine on kahesuunaline, puhverdamata side üle võrgu piiride TCP-like pidurdamise, lõpp-punkti discovery, võrguühenduse olek ja kaetuna lõpp-punkti turvalisus. Edastamine isiku võimaluste erinevad võrgu taseme integreerimine tehnoloogiate, näiteks, et see saab olema rakendatud ühes rakenduses lõpp-punkti ühte arvutisse, virtuaalse Privaatvõrgu tehnoloogia on palju suuremaid, kuna see põhineb muutes selle keskkonnast VPN.

Azure'i edastamine on kaks funktsioone.

1. [Hübriidjuurutuse ühendused](#hybrid-connections) - kasutab avatud standard Web Sockets lubada mitmeplatvormilist stsenaariumid

2. [WCF-i releed](#wcf-relays) - kasutab Windows Communication Foundation Remote üksikasjalik kõnede lubamiseks

Hübriidjuurutuse ühendused ja WCF-i releed lubada turvalist ühendust, et assests, mis on olemas ettevõtte ettevõtte võrgus. Kasutus üksteise sõltub teie vajadustele allpool:

|                                    | WCF-i edastamine | Hübriidjuurutuse ühendused |
| ---------------------------------- |:---------:|:------------------:|
| **WCF-I**                            |     x     |                    |
| **.NET core**                      |           |         x          |
| **.NET framework**                 |     x     |         x          |
| **JavaScripti/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Standardid vastavalt avatud Protocol (protokoll)**  |           |         x          |
| **Mitme RPC Programing mudelite** |           |         x          |
*<sub>Üldine kättesaadavus</sub>

## <a name="hybrid-connections"></a>Hübriidjuurutuse ühendused

Azure'i Relay hübriid ühendused võimalus on turvaline, Ava-protokolli uuendus olemasoleva Relay funktsioone, mis võib olla rakendatakse mis tahes platvormi ja mis tahes keeleks, mida on lihtne WebSocket võimalus, mis sisaldab konkreetselt WebSocket API ühist veebibrauserid. Hübriidjuurutuse ühendused põhineb HTTP ja WebSockets.

## <a name="wcf-relays"></a>WCF-i releed

WCF-i Relay töötab jaoks täielik .NET Framework (NETFX) ja WCF-i. Annate kohapealne teenust ja kasutades WCF-i "relay" sidumiste komplekt vahendusteenusesse vahelise ühenduse. Varjatult relay sidumiste vastendada uus transport sidumine elementide loomiseks kasutatud WCF-i kanali komponendid, mis integreerida teenuse siini pilveteenuses.

## <a name="service-history"></a>Teenuse ajalugu

Hübriidjuurutuse ühendused siirdamist sama nimega "BizTalki teenused" funktsioon, mis oli ehitatud Relay Azure'i teenus siini WCF-i esimesena. Uus hübriid ühendused võimalus täiendab olemasoleva WCF-i edastamine ja neid kahte teenuse võimalusi on olemas side-by-side vahendusteenusesse lähitulevikus; nad levinud lüüsi ühiskasutusse anda, kuid on muidu rakendatud erinevalt.

WCF-i edastamine on pärand Relay, pakkudes, et paljud kliendid võivad kasutada juba oma WCF-i programing mudeleid.

## <a name="next-steps"></a>Järgmised toimingud:

- [Relay KKK](relay-faq.md)
- [Nimeruumi loomine](relay-create-namespace-portal.md)
- [.Net-i kasutamise alustamine](relay-hybrid-connections-dotnet-get-started.md)
- [Alustamine sõlm](relay-hybrid-connections-node-get-started.md)