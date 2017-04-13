<properties
    pageTitle="Azure'i Mobile kaasamine muid sarnaseid Azure'i teenuste võrdlus"
    description="Võrdlus muude teenustega sarnaseid Azure - HockeyApp, AppInsights, teate jaoturi Azure Mobile kaasamine"
    services="mobile-engagement"
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Azure'i Mobile kaasamine võrrelda teiste sarnaste Azure teenustega

Microsoft Azure'i pakutud teenuste loend laieneb kunagi ja aeg-ajalt võib küsida, kuidas on Azure Mobile kaasamine erineb see teenus äsja lugeda või kuulda. Selles artiklis proovib tühjendage segadust ja suunab teid Azure Mobile kaasamine valimiseks, kui see on kõige paremini vastab teie kasutus. 
 
Azure'i Mobile kaasamine on teenus suunatud **digitaalse markkinoijat/CMOs jaoks** , kuid võib kasutada mis tahes **mobiilirakenduse omanik või publisher** kes soovib kasutus säilitus- ja nende mobiilirakendused töötasusid suurendada. 

*See on tarkvara-kui-a-service (SaaS) kasutaja-kaasamine platvorm, mis pakub Andmepõhiste teadmisi rakenduse kasutamine, reaalajas kasutaja osadeks, ja võimaldab kontekstipõhise arvestada tõuketeatised ja sõnumside rakendus.* 

Katkestamine selle määratluse alla, on meil järgmised ka tõstab esile selle kordumatu väärtus pakkumine põhitunnused:

1.  **Kontekstipõhise arvestada tõuketeatised ja -rakenduse sõnumside**
        
    See on core liikumine toote - suunatud ja isikupärastatud Tõuketeatiste teha. Ja see juhtub, me kogu rikkaliku käitumist analytics andmete. 

2.  **Andmepõhiste teadmisi rakenduse kasutamine**

    Pakume rist platvormi SDK-d koguda käitumist analytics rakenduse kasutajate kohta. Pange tähele Termini käitumist analytics (võrreldes jõudluse analytics), kuna me keskenduda kuidas rakenduse kasutajad kasutavad rakendust. Me lihtsa tuleks analytics jõudlusandmeid koguda tõrgete, jookseb jne, kuid mis ei core liikumine toote kohta. 

3.  **Reaalajas kasutaja osadeks**

    Kui olete rakenduse kasutajate käitumist analytics andmete kogunud, saame võimaldavad lõiku publikule erinevate parameetrite põhjal ja kogutud andmete selleks, et saaksite suunatud tõuketeatised kampaaniat käivitamiseks. 

4.  **Tarkvara-kui-a-service (SaaS):**

    Meil on eraldi Azure haldusportaali, mis on optimeeritud suhelda ja rikkaliku käitumist analytics kohta rakenduse kasutajad vaadata ja käivitage tõuketeatised turunduskampaaniate portaali. Toode on suunatud teil ei ole aega saada!   
 
Selle komplekti eristamine käsitsi, siin on, kuidas me võrrelda teiste näivalt sarnaste Azure pakkumisi - Märkus See artikkel ei võrdluse funktsiooni, kuid võtab rohkem stsenaarium vastavalt lähenemine määratleda, millist toodet toimib kõige paremini:
 
1.  [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) on Microsofti veebisaidil mobiilne DevOps lahendus. Saate levitada järgud beeta testrid, krahh andmete kogumise ja kasutaja tagasiside saamine. Samuti integreerub Visual Studio meeskonnatöö teenused, mis võimaldavad hõlpsalt Koosta kasutuselevõttu ja töö üksuse integreerimine. 
    
    Siin on DevOps ja jõudluse analytics andmete kogumise mobiilirakenduste kohta. Võite lõpuks integreerida nii HockeyApps ja Mobile kaasamine oma rakenduste ja mida ei ole ebatavalised, sest isegi juhul, kui seal on mõned neist kogutud andmete, toodete core liikumine on erinevad ja need aitavad teie jaoks eri eesmärkide saavutamisel.  

2.  [Rakenduse ülevaated](../application-insights/app-insights-overview.md) Kui teie mobiilirakenduses on serveri külg, siis kasutage rakenduse ülevaated jälgida web serveripoolse rakenduse, kuid kliendi küljel mobiilirakendused kasutage HockeyApp. 

3.  [Teatis jaoturi](https://azure.microsoft.com/services/notification-hubs/) pakub kerge teenust ei pea te ka SDK integreeritud mobiilirakenduses ja olete oma mobiilirakenduse täielikud õigused ja see võimaldab saata Tõuketeatiste koos Suhtlussiltide põhilisi funktsioone. See on suurepärane mis tahes rakenduse omanikule, kes huvitab väiksem suunamise ning veel saatmise/suhtlemine teabe kiire rakenduse kasutajatele (suur populatsiooni levisaate) kohta. 

    Pange tähele liikumine saatmisel väga lihtne osadeks võimalusega kiiresti teatised. 

Vaatame mõned stsenaariumid:

1.  Tim on osa digitaalse turunduse meeskonnatöö Adventure Worksi poest, mis avaldab mobiilirakendused kasutajate jaoks. Tim eesmärk on tagada, et kasutajad, kes saavad Mobile'i rakendused alla laadida jätkata selle kasutamist ja sageli. See mitte ainult suureneb täiesti manustada rakenduse kasutajad aga suureneb Valuutana kindla väärtuse omistamine kui rakenduse kasutajad teha ostud mobiilirakenduse kaudu. Selleks tuleb Tim rakenduse kasutajatele, mida nad kasulik, et julgustada neid Avage rakendus ja naaske selle sageli suunatud teatiste saatmine. See on, kus Tim kasulikud lingid Mobile. 

2.  Joann on osa arendusmeeskonnale Adventure Worksi mobiilirakenduste ja otsib toote üksikasjade jookseb erandid logima ja jõudluse telemeetria toomine rakendus. See on, kus Joann kasulikud HockeyApp. Joann võib integreerida nii HockeyApp tema arendaja värskendustest stsenaariumid ja Azure Mobile kaasamine Tim jaoks parimaks nii sama rakenduse. 

3.  Jaan on osa arendusmeeskonnale mobiilirakenduste Contoso News network ja kõik ta soovib on katkestamine Uudised kõigile kasutajatele ilma palju jaotus, kui uudiste reegli saata. See on, kus jaan kasulikud teatis jaoturi. Selle stsenaariumi korral on võimalik siiski nagu mobiilirakenduse tagasimakse tähtaeg, on palju rikkalikumat osadeks teha ja rakenduse kasutaja käitumist üksikasjade toomine nõue. Sel ajal, Jaan on hindamine kaasamine Azure Mobile. 
 
Sulgege, Mobile kaasamine eesmärk ei ole lihtsalt koguda analytics - ei ole "veel üks Analytics toode Microsoft". On suunatud Tõuketeatiste saatmise kohta ning jaoks selle suunamise me käitumist analytics andmete kogumise, kuid liikumine jääb saatmine Tõuketeatiste, mis on kõige mõistlikum rakenduse kasutajatele, et see ei tule rämpspostiks. 

Lisateavet - Heitke pilk selles [lühivideost](mobile-engagement-overview.md) kohta Mobile kaasamine lühidalt. 

