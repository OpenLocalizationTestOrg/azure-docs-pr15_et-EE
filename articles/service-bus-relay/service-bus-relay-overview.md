<properties
    pageTitle="Teenuse siini relay ülevaade | Microsoft Azure'i"
    description="Ülevaade teenuse siini relay."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Teenuse siini relay ülevaade

Suur osa teenuse siini on tsentraliseeritud (kuid väga koormusetasakaalustusega) *relay* teenus, mis võimaldab teil luua hübriidjuurutuse rakendusi, mis on Azure andmekeskuse nii oma kohapealse enterprise keskkonna käivitamine.  Teenuse siini relay toetab mitmesuguseid eri transport protokollid ja web services standarditele. See hõlmab SOAP oli-, *, ja isegi ülejäänud. Vahendusteenusesse hõlbustab hübriid rakenduste abil turvaliselt nähtavaks tegemine Windows Communication Foundation (WCF) teenuseid, mis asuvad ettevõtte ettevõtte võrgustikus avaliku pilveteenusesse, ilma et peaksite avama tulemüüri ühendus, või ettevõtte võrku taristu pealetükkivad muudatuste tegemise. 

![Relay mõisted](./media/service-bus-relay-overview/sb-relay-01.png)

Vahendusteenusesse toetab traditsiooniline ühesuunalise sõnumside, taotluse/vastuse sõnumside ja-Võrdõigusvõrk sõnumside. Samuti toetab sündmuse normaaljaotus väärtusel Interneti-ulatus lubamiseks avaldamise/tellimise stsenaariumid ja kahesuunalise turvasoklite suhtlus suurema kakspunkt suurendamiseks. 

Võrdõigusvõrku sõnumside struktuuris kohapealse teenuse kaudu väljaminev port vahendusteenusesse loob ja loob kahesuunalise turvasoklite suhtlemiseks seotud kindla rendezvous aadress. Kliendi siis suhelda asutusesisese teenuse suunamise rendezvous aadressi vahendusteenusesse sõnumeid saata. Vahendusteenusesse siis "edastab" asutusesisese teenuse kaudu juba kasutusel olevasse kahesuunalise sõnumeid. Klient ei vaja asutusesisese teenuse otsene ühendus, ei pea teada, kus asub teenuse ja kohapealse teenus ei pruugi kõik sissetulevad pordid tulemüüris avatud.

Annate kohapealne teenust ja kasutades WCF-i "relay" sidumiste komplekt vahendusteenusesse vahelise ühenduse. Varjatult relay sidumiste vastendada uus transport sidumine elementide loomiseks kasutatud WCF-i kanali komponendid, mis integreerida teenuse siini pilveteenuses. 

## <a name="next-steps"></a>Järgmised sammud

Lisateavet teenuse siini relay leiate järgmistest teemadest.

- [Azure'i teenus siini arhitektuuri ülevaade](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Kuidas kasutada teenuse siini vahendusteenusesse](service-bus-dotnet-how-to-use-relay.md)

 