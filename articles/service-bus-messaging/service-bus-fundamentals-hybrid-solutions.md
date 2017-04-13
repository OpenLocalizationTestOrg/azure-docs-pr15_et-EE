<properties 
    pageTitle="Azure'i teenus siini | Microsoft Azure'i" 
    description="Ühenduse loomine muu tarkvara Azure rakendusi teenuse siini abil tutvustus." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Azure'i teenus siini

Kas rakenduse või teenuse käivitamist pilveteenuses või kohapealne tuleb sageli suhelda muud rakendused või teenused. Anda selleks laiemalt kasulikul viisil, pakub Microsoft Azure'i teenuse siini. Selles artiklis tutvustatakse selle tehnoloogia, mis kirjeldab, mis see on ja miks võiksite kasutada seda vaadata.

## <a name="service-bus-fundamentals"></a>Teenuse siini põhialused

Erinevates olukordades helistada erinevate laadide teatise. Mõnikord on parim lahendus lastes rakenduste kaudu lihtne järjekorda sõnumite saatmiseks ja vastuvõtmiseks. Muudel juhtudel on tavapärase järjekorda ei piisa; järjekorda avalda ja tellida abil on parem. Mõnel juhul kõik, mida on vaja tingimata on omavahel rakenduste; järjekorrad pole kohustuslikud. Teenuse siini pakub kõigi kolme suvandid oma rakenduste suhelda mitmel viisil.

Teenuse siin on mitme rentniku pilveteenuses, mis tähendab, et teenuse mitme kasutaja ühiselt. Iga kasutaja, nt rakendus arendaja, loob *nimeruumi*, siis määratleb side menetlustele, peab ta selle nimeruumi sees. Joonisel 1 on näidatud, kuidas see välja näeb.

![][1]
 
**Joonis 1: Teenuse siini pakub mitme rentniku teenust rakenduste ühendamiseks pilveteenuse kaudu.**

Nimeruumi, saate ühe või mitme eksemplari nelja erinevate vahendeid, mis ühendab rakenduste erineval viisil. Valikud on:

- *Järjekorrad*, mis võimaldavad ühe-suunamata suhtlus. Iga järjekorra toimib andmiseks (mõnikord nimetatakse *ta*) talletava saadetud sõnumid kuni saabumisel. Ühe adressaadi on iga sõnumi vastu.
- *Teemad*, mis pakuvad ühe-suunamata side abil *tellimuste*- ainult ühe teema võib olla mitu tellimust. Järjekorda, nagu on ta toimib teema, kuid iga tellimuse soovi korral saate filtri kasutamine ainult teatud kriteeriumidele vastavaid sõnumeid saada.
- *Releed*, mis pakuvad kahesuunaline ühendus. Erinevalt järjekorrad ja teemad, on relay ei talletada parda sõnumid; ei saanud ta. Selle asemel seda lihtsalt annab selle edasi sihtrakendust.

Kui loote järjekorda, teema või edastamine, saate sellele nime panna. See nimi koos mis tahes, mida nimetatakse oma nimeruumi, loob objekti ainuidentifikaator. Rakenduste sisestage selle nimi teenuse siini ja seejärel selle järjekorda, teema või relay suhtlevad abil. 

Kasutada kõiki neid objekte relay stsenaariumi, Windowsi rakendusi saate kasutada Windows Communication Foundation (WCF). Järjekorrad ja teemad, saate Windowsi rakenduste teenuse siini määratletud sõnumside API-d. Nende objektide – Windows rakenduste kasutamise lihtsustamiseks pakub Microsoft SDK-d Java, Node.js ja muude keelte jaoks. Võite kasutada ka järjekorrad ja teemade üle httpd REST API-de abil. 

On oluline mõista, et isegi juhul, kui teenus siini ise käivitatakse pilves (st Microsoft Azure'i andmekeskuste), rakendusi, mis kasutavad seda saab käivitada igal pool. Saate ühendada töötavad Azure, näiteks rakendused või rakenduste töötab sees oma andmekeskuse teenuse siini. Saate seda kasutada ka Azure või teise töötavad rakenduse ühendamiseks pilveteenuse platvormi kohapealse rakendusega või telefonides ja tahvelarvutites. On võimalik isegi ühenduse keskne rakenduse või mõnda muud kodumasinad, andurid ja muudes seadmetes. Teenus on pilves, mis on kättesaadav päris palju kõikjal side süsteem. Kuidas kasutada sõltub rakenduste tuleb teha.

## <a name="queues"></a>Järjekorrad

Oletame, et te otsustate ühendada kaks rakendustes, mis kasutavad teenuse siini järjekorda. Joonis 2 illustreerib olukord.

![][2]
 
**Joonis 2: Teenuse siini järjekorrad pakuvad ühesuunalise asünkroonne queuing.**

Protsess on lihtne: teenuse siini järjekorda saadab sõnumi saatja ja vastuvõtja jätkab see sõnum mõned hiljem. Järjekorda võib olla üks vastuvõtja, nagu joonisel 2 näitab. Või mitu rakendust saate lugeda samas järjekorras. Viimasel iga sõnumi loeb ainult ühe vastuvõtja. Mitme valatud teenuse, peaksite kasutama teema asemel.

Iga sõnumi on kaks osa: komplekt, atribuutide, iga paari /-väärtuse ja kahendarvu sõnumi kehasse. Nende kasutamine sõltub sellest, milline rakendus proovib teha. Näiteks võivad rakenduse kohta viimaste müük sõnumi saatmist atribuutide *müüja = "Ava"* ja *summa = 10000*. Sõnumi kehasse võib sisaldavad skannitud pilt allkirjastatud müügileping, või kui ühte arvutisse pole lihtsalt tühjaks.

Vastuvõtja saate lugeda sõnumi teenuse siini järjekorda kahel viisil. Esimene suvand, *ReceiveAndDelete*, eemaldatakse see kuhjuda ja kohe kustutab selle. See on lihtne, kuid kui vastuvõtja jookseb enne selle lõppemist sõnumi töötluse, sõnumi, lähevad kaotsi. Kuna see on eemaldatud järjekorda, ei ole muud vastuvõtja sellele juurde pääsevad. 

Teine võimalus, *PeekLock*, on mõeldud aitama selle probleemi lahendamiseks. Nagu **ReceiveAndDelete**, eemaldab **PeekLock** lugemine sõnumi järjekorda. Ei Kustuta sõnum, kuid. Selle asemel lukud sõnumi, mistõttu nähtamatu muud vastuvõtjad seejärel ootab üks kolmest sündmustest.

- Kui vastuvõtja töötleb sõnumi edukalt, seda nõuab **lõpuleviimine**ja järjekorda kustutab sõnumi. 
- Kui vastuvõtja otsustab, et seda ei saa sõnumi protsessi edukalt, see nõuab **hülgama**. Järjekorda siis lukustus eemaldab sõnumi ja teeb selle kättesaadavaks muud vastuvõtjad.
- Kui vastuvõtja helistab ei konfigureeritav aja jooksul (vaikimisi 60 sekundi järel), järjekorda eeldab, et vastuvõtja on nurjunud. Sel juhul käitub nagu siis, kui vastuvõtja oli nimetatakse **loobuda**sõnumi muud vastuvõtjad kättesaadavaks tegemine.

Pange tähele, mis võib juhtuda siin: sama sõnumi võib toimetada kaks korda, võib-olla kaks erinevat vastuvõtjad. See peab olema valmis teenuse siini järjekorrad kasutavad rakendused. Hõlbustamiseks duplikaatide tuvastamine, iga sõnumi on kordumatu **MessageID** atribuut, mis vaikimisi jääb samaks, olenemata sellest, kuidas mitu korda on sõnumi lugeda järjekorda. 

Järjekorrad on kasulik üsna vähe olukordades. Need võimaldavad suhelda, isegi kui mõlemad pole töötab samal ajal, midagi, mis on eriti mugav paketi ja mobiilirakenduste rakendused. Mitme vastuvõtjad järjekorda pakub automaatse koormusetasakaalustuseks, kuna saadetud sõnumid on laiali nende vastuvõtjad.

## <a name="topics"></a>Teemade

Kasulik, kui need on, järjekorrad pole alati õige lahendus. Mõnikord on parem teenuse siini teemade. Joonis 3 kujutab idee.

![][3]
 
**Joonis 3: Põhjal tellides rakenduse määrab filtri, saate, mõned või kõik sõnumid saadetakse teenuse siini teema.**

*Teema* on sarnane järjekorda mitmel viisil. Teema samal viisil esitada sõnumitele järjekorda, ja nende saatjate Edasta sõnumeid vaadata sama nagu järjekorrad. Suur erinevus on, et teemade võimaldavad iga vastuvõtmise rakenduse loomiseks oma *tellimuse* määratledes *filter*. Tellija siis kuvatakse ainult need sõnumid, mis vastavad filtri. Näiteks joonis 3 näitab saatja ja teema kolme tellijad, iga eraldi filtriga:

- Abonendi 1 saab ainult sõnumeid, mis sisaldavad atribuudi *müüja = "Ava"*.
- Abonendi 2 saab sõnumeid, mis sisaldavad atribuudi *müüja = "Ruby"* ja/või sisaldavad mõne *summa* atribuut, mille väärtus on suurem kui 100 000. Võib-olla Ruby on müügijuht, nii, et ta soovib, et nii oma müügi ja kogu suur müük sõltumata sellest, kes muudab need.
- Abonendi 3 on seatud *tõene*, mis tähendab, et see saab kõigi sõnumite selle filter. Näiteks see rakendus võib olla ülesanne säilitada auditi ning seetõttu tuleb näha kõiki sõnumeid.

Sarnaselt järjekordade tellijad teemale saate lugeda **ReceiveAndDelete** või **PeekLock**sõnumeid. Erinevalt järjekorrad, aga ühe sõnumi teema saab vastu mitu tellimust. Seda moodust, üldiselt tuntud *avaldamine ja tellimine* (või *pub/sub*), on kasulik iga kord, kui mitu rakenduste huvitab sama sõnumeid. Määratledes õige filter, saate iga abonendi koputage lihtsalt osa sõnumi voo, mida on vaja näha.

## <a name="relays"></a>Releed

Nii järjekorrad ja teemade pakuvad ühesuunalise asünkroonne side kaudu on ta. Liikluse juhtimine ainult ühes suunas ja ühendust pole otsest saatja ja vastuvõtja vahel. Kuid mida teha, kui te ei soovi seda? Oletame, et rakenduste vaja nii sõnumite saatmiseks ja vastuvõtmiseks, või võib-olla soovite need otsene seos ning te ei pea ta, sõnumite talletamiseks. Aadress stsenaariumid, et teenuse siini pakub *edastab*, nagu joonisel 4 on näidatud.

![][4]
 
**Joonis 4: Teenuse siini relay pakub sünkroonse, kahesuunaline side rakenduste vahel.**

Selge küsimus küsida releed on järgmine: Miks sooviksite kasutada ühte? Isegi juhul, kui ma ei pea järjekorrad, miks teha otse nendega suhelda pilveteenus kaudu, mitte ainult rakendusi? Vastus on, et kõik rääkimise otse võib olla raskem kui arvate, võib.

Oletame, et soovite ühendada kaks kohapealse rakendused, töötab nii ettevõtte andmekeskuste sees. Iga rakendus asub tulemüüri taga ja iga andmekeskuse kasutab tõenäoliselt võrku aadress tõlge (NAT). Tulemüür blokeerib sissetulevad andmete kõik vaid mõne pordid ja NAT tähendab, et iga rakendus töötab seade ei jõuate otse väljaspool andmekeskuse fikseeritud IP-aadress. Ilma täiendavat abi, ühenduse avaliku Interneti kaudu need rakendused on probleeme.

Aitab teenuse siini relay. Suhelda kahesuunaliselt on relay, konkreetse rakenduse loob mõne teenuse siini väljamineva ühenduse TCP ja seejärel hoiab avatud. Kogu suhtlus kaks taotlust liigub läbi need ühendused. Kuna iga ühendus on loodud kaudu andmekeskuse sees, võimaldab tulemüüri liiklust iga uue pordid avamata. Seda moodust saab NAT probleemi, kuna iga taotlus on pilves teatises ühtse lõpp-punkti. Andmevahetus kaudu soovitud relay rakendusi saate vältida probleeme, mis muidu teeks side keeruline. 

Teenuse siini kasutama edastab rakendusi kasutada Windows Communication Foundation (WCF). Teenuse siini pakub WCF-i sidumiste, mida oleks lihtne Windowsi rakenduste releed kaudu suhtlemiseks. Rakendustes, mis juba kasutavad WCF saate tavaliselt lihtsalt määrata ühe nende sidumiste, ja seejärel üksteisele kaudu soovitud relay. Erinevalt järjekorrad ja teemad, aga abil releed kaudu – Windows rakendused, kui võimalik, nõuab mõned programmeerimise peegeldav; pole standard teekides on olemas.

Erinevalt järjekorrad ja teemad, ei rakenduste loomine releed. Selle asemel, kui rakendus, mis soovib sõnumeid, mis loob teenuse siini TCP-ühenduse, on relay luuakse automaatselt. Kui ühendus katkeb, kustutatakse selle relay. Rakenduse leidmiseks relay, mis on loodud teatud kuulajale lubamiseks teenuse siini pakub registri, mis võimaldab rakenduste leidmiseks teatud edastamine nime järgi.

Releed on õige, kui peate otsese suhtlemine rakenduste. Näiteks kaaluge lennufirma broneerimiseks süsteemi töötab asutusesisese andmekeskuses, millele pääseb juurde sisse kioskis, mobiilsideseadmete ja teiste arvutite kaudu. Kõik need süsteemid töötavad rakendused võivad toetuvad teenuse siini releed pilveteenuses suhelda, kuhu nad võivad töötama.

## <a name="summary"></a>Kokkuvõte

Ühenduse rakendused on alati olnud osa täieliku lahenduste, ja stsenaariumid, mis nõuavad rakenduste ja teenuste omavahel suhelda vahemik on seatud rohkem applications tõstmiseks ja Internetiga ühendatud. Pilvepõhise tehnoloogiad pakkudes selle saavutamiseks järjekorrad, teemade ja releed kaudu teenuse siini eesmärk on oluline funktsioon lihtsam rakendada ja laiemalt kättesaadavaks tegemine.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhikomponentide Azure'i teenus siini, järgige neid linke lisateabe.

- Kuidas kasutada [teenuse siini järjekorrad](service-bus-dotnet-get-started-with-queues.md)
- Kuidas kasutada [teenuse siini Teemad](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- Kuidas kasutada [teenuse siini edastamine](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Teenuse siini näidised](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
