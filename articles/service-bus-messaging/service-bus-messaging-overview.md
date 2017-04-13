<properties
    pageTitle="Teenuse siini sõnumside ülevaade | Microsoft Azure'i"
    description="Teenuse siini sõnumside: paindlik andmete kohaletoimetamise pilveteenuses"
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
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Teenuse siini sõnumside: paindlik andmete kohaletoimetamise pilveteenuses

Microsoft Azure'i teenuse siin on usaldusväärne teave teenus. See teenus eesmärk side oleks lihtsam. Kui kaks või enam soovite vahetada teavet, tuleb side süsteem. Teenus on vahendatud või muu suhtlus süsteem. See on sarnane posti teenus füüsilise geograafiline asukoht. Postiteenused teha väga lihtne saata erinevat tüüpi tähed ja pakettide mitmesuguseid kohaletoimetamise garantiid, igal pool maailmas.

Sarnaselt pakkuda tähed posti teenus, teenus on paindlik teabe saatja ja adressaadi tarne. Sõnumside teenusega tagab, et teave on kohale toimetatud isegi juhul, kui pooled on kunagi nii võrgus samal ajal või kui need pole saadaval täpse samal ajal. Sel viisil, sõnumside on sarnane saatmine kirja, kuigi side vahendajaks sarnaneb telefonikõne (või kuidas telefonikõne varem – enne kõne ootel ja helistaja ID, mis on nagu vahendatud sõnumside rohkem).

Sõnumi saatja, saate ka nõuda erinevaid kohaletoimetamise omadusi, sh tehingud, duplikaatide tuvastamine, ajapõhiste aegumine ja partiide. Need on samuti postivöötkoodi analoogiaid: korrake kohaletoimetamise, nõutav allkirja, aadressi muutmine või tagasivõtmine.

Teenuse siini toetab kaks erinevat sõnumside mustrid: *edastamine* ja *vahendatud sõnumside*.

## <a name="service-bus-relay"></a>Teenuse siini edastamine

Teenuse siini [Relay](../service-bus-relay/service-bus-relay-overview.md) komponent on tsentraliseeritud (kuid väga koormusetasakaalustusega) teenus, mis toetab paljusid erinevate transport protokollid ja Web services standardid. See hõlmab SOAP oli-, *, ja isegi ülejäänud. [Vahendusteenusesse](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) pakub mitmesuguseid eri relay võimalusi ja aitavad rääkida otsese Võrdõigusvõrk ühendused, kui see on võimalik. Teenuse siini on optimeeritud .NET arendajatele, kes kasutavad Windows Communication Foundation (WCF), nii jõudlus ja kasutatavuse, ja pakub täielikku juurdepääsu oma vahendusteenusesse SOAP-i ja ülejäänud liideste kaudu. See võimaldab SOAP või REST programmeerimisega keskkonna teenuse siini integreerida.

Vahendusteenusesse toetab traditsiooniline ühesuunalise sõnumside, taotluse/vastuse sõnumside ja-Võrdõigusvõrk sõnumside. Samuti toetab sündmuse normaaljaotus väärtusel Interneti-ulatus lubamiseks avaldada-Telli stsenaariumid ja kahesuunalise turvasoklite suhtlus suurema kakspunkt suurendamiseks. Võrdõigusvõrku sõnumside struktuuris kohapealse teenuse kaudu väljaminev port vahendusteenusesse loob ja loob kahesuunalise turvasoklite suhtlemiseks seotud kindla rendezvous aadress. Kliendi siis suhelda asutusesisese teenuse suunamise rendezvous aadressi vahendusteenusesse sõnumeid saata. Vahendusteenusesse siis "edastab" asutusesisese teenuse kaudu juba kasutusel olevasse kahesuunalise sõnumeid. Klient ei pea asutusesisese teenuse otsene ühendus ega on vaja teada, kus asub teenuse ja kohapealse teenus ei pruugi kõik sissetulevad pordid tulemüüris avatud.

Annate ühendust teie asutusesisese teenuse ja vahendusteenusesse, kasutades WCF-i "relay" sidumiste komplekt. Varjatult relay sidumiste Vastendage transport sidumine elementide loomiseks kasutatud WCF-i kanali komponendid, mis integreerida teenuse siini pilveteenuses.

Teenuse siini Relay pakub mitmeid eeliseid, kuid nõuab serveri ja kliendi nii olema võrgus samal ajal, et sõnumite saatmiseks ja vastuvõtmiseks. See pole optimaalse HTTP-laadi suhtlus, kus taotlused ei pruugi tavaliselt vanade, ega klientidele, et ühendada vaid aeg-ajalt brauserites, nt mobiilirakendusi, jne. Vahendajaks sõnumside toetab sidumata side ja on oma eelised; klientide ja serverite saate omavahel ühendada, kui vaja ja toiminguid asünkroonne viisil.

## <a name="brokered-messaging"></a>Vahendajaks sõnumside

Vastupidiselt relay värviskeemi, [vahendatud sõnumside](service-bus-queues-topics-subscriptions.md) saab nii asünkroonne mõtlesin, või "ajutiselt sidumata." Tootjate (saatjad) ja tarbijate (vastuvõtjad) ei pea olema samal ajal võrgus. Kiirsõnumside taristu säilitab sõnumite "ta" usaldusväärselt (nt järjekorda), kuni nõudvate tootja on valmis neid vastu võtta. See võimaldab komponentide jaotatud rakendus katkestatakse, kas vabatahtlikult; näiteks hooldus või komponent krahhi tõttu, ilma mõjutamata kogu süsteem. Lisaks on võib olla ainult teatud ajal päeva, nt laoseisu haldamise süsteemi ainult nõutava käivitada äri päeva lõpus veebis.

Teenuse siini vahendatud sõnumside taristu core komponendid on järjekorrad, teemasid ja tellimused.  Peamine erinevus on, et teemade toetavad avaldamise/tellimise võimalusi, mida saab kasutada keerukaid sisu-põhise marsruutimine ja kohaletoimetamise loogika, sh saata mitmele adressaadile. Nende komponentide lubada uue asünkroonne sõnumside stsenaariumid, nt ajaline lahutamine, avaldamise/tellimise ja laadimine tasakaalustamiseks. Sõnumside üksustes kohta leiate lisateavet teemast [teenuse siini järjekorrad, teemad, ja tellimused](service-bus-queues-topics-subscriptions.md).

Sarnaselt Relay taristu, vahendatud kiirsõnumside võimalus on esitatud WCF-i ja .NET Framework programmeerijatele ja ka ülejäänud kaudu.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet teenuse siini sõnumside kohta leiate järgmistest teemadest.

- [Teenuse siini põhialused](service-bus-fundamentals-hybrid-solutions.md)
- [Teenuse siini järjekorrad, teemasid ja tellimused](service-bus-queues-topics-subscriptions.md)
- [Kuidas kasutada teenuse siini järjekorrad](service-bus-dotnet-get-started-with-queues.md)
- [Kuidas kasutada teenuse siini teemade ja tellimused](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 
