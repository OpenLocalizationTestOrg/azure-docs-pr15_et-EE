<properties
    pageTitle="Mitme rentniku Web Application muster | Microsoft Azure'i"
    description="Otsige arhitektuuri ülevaated ja kujundus mustreid, mis on kirjeldatud, kuidas rakendada mitme rentniku veebirakenduse Azure."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Rentnikuga rakenduste Azure

Rentnikuga rakendus on ühiskasutuses ressurss, mis võimaldab eraldi kasutajate või "rentnikukontodele" nii, nagu see oli oma rakenduse kuvamiseks. Tüüpilised stsenaarium, mis ärisündmuste rentnikuga rakendus on üks, kus kõik kasutajad rakenduse soovivad kasutusvõimaluse kohandada, kuid muidu on sama lihtne business nõuetele. Suure rentnikuga rakendused on Office 365, Outlook.com-i ja visualstudio.com.

Rakenduse pakkuja seisukohalt multitenancy eelised peamiselt seotud töö- ja tõhusust. Rakenduse versiooni saate palju rentnikud või klienti, lubamisel konsolideerimise süsteemi haldustoimingute nagu jälgimine, jõudluse parandamine, tarkvara hooldus ja andmete varundamise vajadustele.

Järgmine loetletakse kõige olulisemad eesmärgid ja mõni pakkuja seisukohalt.

- **Provisioning**: peate saama ettevalmistamise uute rentnike rakenduse.  Rentnikuga rakenduste abil suure hulga rentnikud, on tavaliselt vaja, võimaldades iseteenindusliku ettevalmistamise protsessi automatiseerimiseks.
- **Hooldatavust**: peate saama rakenduse täiendamine ja teha muude hooldustoiminguid mitme rentniku on selle kasutamise ajal.
- **Jälgimine**: teil tuleb alati rakenduse probleemide tuvastamine ja nende tõrkeotsinguga jälgimiseks. See hõlmab jälgida, kuidas iga rentniku rakenduse abil.

Õigesti rakendatud rentnikuga rakenduse pakub kasutajad on järgmised eelised.

- **Eraldamise**: üksikute rentnikud tegevuste ei mõjuta muid rentnike rakenduse kasutamine. Rentnike jaoks ei saa kasutada üksteise andmeid. Tundub rentniku nii, nagu need on rakendus kasutab.
- **Kättesaadavus**: üksikute rentnikud soovi olla pidevalt saadaval, võib-olla koos määratletud SLA garantiid rakendus. Muud rentnikud tegevuste ei tohiks uuesti mõjutada rakenduse kättesaadavus.
- **Skaleeritavus**: rakenduse skaala rahuldamiseks üksikute rentnikega. Kohaloleku- ja muud rentnikud toiminguid ei tohiks mõjutada rakenduse jõudlus.
- **Maksumus**: kulud on väiksemad kui töötab pühendunud, ühe – rentniku rakenduse, sest mitme rendiõigus võimaldab ressursside.
- **Customizability**. Võimalus kohandada üksikute rentnikku mitmel viisil, nt lisamise või eemaldamise funktsioone, muuta värve ja logod või isegi lisamine oma koodi või skripti taotlus.

Lühidalt, kui palju teil tuleb arvesse võtta väga paindlik pakkumiseks asjaolu, on ka eesmärgid ja nõuetele, mis on palju rentnikuga rakenduste arv. Mõned ei pruugi olla teatud stsenaariumide asjakohaste ja prioriteedi üksikute eesmärgid ja nõuded erinevad iga stsenaariumi. Nagu rentnikuga rakenduse pakkuja, mida on ka eesmärkide saavutamise ja nagu, koosoleku on rentnike jaoks eesmärkide saavutamise ja nõuetele, tasuvust, arveldus, mitme teenuse tasemed, ettevalmistamise, hooldatavust jälgimine ja automatiseerimine.

Täiendavad kujundamine rentnikuga rakenduse kohta leiate lisateavet teemast [mitme rentniku rakenduse Azure Hosting][]. Ühiseid andmeid arhitektuur mudeleid mitme rentniku tarkvara-kui-a-service (SaaS) andmebaasi rakenduste kohta leiate teavet teemast [Kujundus mustrid mitme rentniku SaaS rakenduste Azure SQL-andmebaas](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure'i pakub mitmeid funktsioone, mis võimaldavad teil kui rentnikuga kavandamise võtme probleemide lahendamiseks.

**Eraldamise**

- Lõigu veebisaidi rentnikud Host päistega koos või ilma SSL-side
- Päringu parameetrite lõigu veebisaidi rentnikega
- Töötaja rolliga veebiteenused
    - Töötaja rollid. mis on tavaliselt protsessi kirjutamata rakenduse andmeid.
    - Web rollid, mis tavaliselt toimivad frontend rakendusi.

**Salvestusruumi**

Andmete haldamine nagu Azure'i SQL-andmebaasi või Azure Storage teenused, nt tabeli teenus, mis osutab Storage suurte struktureerimata andmed ja bloobimälu teenus, mis osutab suure hulga teksti struktureerimata talletamiseks või binaarandmete nagu video, heli ja pilte.

- Vastav SQL-andmebaasis Multitenant andmete turvamise kohta – rentniku SQL serveri sisselogimise.
- Kasutades Azure tabelite rakenduse ressursid, määrates container juurdepääsutase poliitika, saate võimalus kohandada õigusi ilma probleemi uus URL ressursid, mis on kaitstud ühiskasutusega juurdepääsu allkirjad.
- Azure'i järjekorrad rakenduse ressursid Azure'i järjekorrad kasutatakse ketas töötlemine rentnikud nimel, kuid võib kasutada ka levitada töö ettevalmistamise või haldamiseks nõutav.
- Teenuse siini järjekorrad rakenduse ressursse, mis sunnib töö ühiskontakti teenus, saate ühe järjekorda kus iga rentniku saatja on ainult õigused (nagu saadud taotluste välja ACS) lükake see järjekorda, samal ajal ainult vastuvõtjad teenuse õigust tõmmata kuhjuda mitme rentniku pärit andmeid.


**Ühenduse loomise ja teenused**

- Azure'i teenus siini sõnumside taristu, mis asub rakenduste neil Exchange sõnumite lõdvalt seotud viis täiustatud skaala paindlikkust.

**Võrgunduse teenused**

Azure'i pakub mitu võrguteenuste, mis toetavad autentimine, hallatavust majutatud rakenduste parandada. Need teenused on järgmised:

- Azure virtuaalse võrgu võimaldab teil ettevalmistamise virtuaalse privaatse võrgu (VPN) Azure haldamine ning turvaliselt link neid kohapealse IT taristu.
- Virtuaalse võrgu liikluse haldur võimaldab teil laadida saldo liiklust mitmes majutatud Azure'i teenuses, kas nad töötab sama andmekeskuse ühest või mitmest eri andmekeskuste kogu maailmas.
- Azure Active Directory (Azure AD) on tänapäevane, ülejäänud teenus, mis pakub identiteedi haldus ja juurdepääsu juhtimine pilveteenuste rakendusi. Rakenduse ressursid Azure AD, et abil on lihtne autentimist ja lubada kasutajatel juurde pääseda oma web rakenduste ja teenuste lubades autentimise ja luba välja oma kood, tuleb kaasata funktsioonide kasutamine Azure AD.
- Azure'i teenus siini pakub Turvaline sõnumside ja andmete meilivoo võimalus jaoks jaotatud ja hübriid rakendused, näiteks suhtlemine Azure'i majutatud rakendusi ja kohapealse rakenduste ja teenuste keerukate tulemüür ja turvalisuse infrastruktuuri nõudmata. Rakenduse ressursid teenuse siini Relay abil teenuseid, mis on esitatud lõpp-punktid võib kuuluvad rentniku (nt majutatavale süsteemi, nt kohapealne) või nad võivad olla teenuste ette valmistatud spetsiaalselt rentniku (kuna tundliku loomuga, rentniku kohased andmete läbib kogu neid).



**Ettevalmistamise ressursid**

Azure'i pakub mitmel viisil sätte uute rentnike rakendus. Rentnikuga rakenduste abil suure hulga rentnikud, on tavaliselt vaja, võimaldades iseteenindusliku ettevalmistamise protsessi automatiseerimiseks.

- Töötaja rollid võimaldavad teil ja de rentniku kohta ressursid (nt kui uue rentniku märgid üles- või tühistab), koguda mõõdikute mõõtmine kasutamise ja haldamise skaala pärast teatud ajakava või vastuseks tootluse võtmenäitajad piirmäärade ületamise. Selle sama rolli võib kasutada ka Push välja värskendused ja uuendamine, et lahendus.
- Azure'i plekid saab kasutada ettevalmistamise Arvuta või eelnevalt lähtestatud salvestusruumi ressursid uue rentnikud container juurdepääsutase poliitikate pakkudes selle Arvuta kaitsmiseks teenuse pakettide VHD pilte ja muud ressursid.
- Järgmised suvandid ettevalmistamise SQL-andmebaasi ressursid rentniku jaoks.

    -   DDL skripte või manustatud ressurssidena assemblereid sees
    -   SQL Server 2008 R2 DAC paketid juurutatud programmiliselt.
    -   Juhtslaidi viide andmebaasi kopeerimine
    -   Ettevalmistamise uute andmebaaside failist andmebaasi importimise ja eksportimise abil.



<!--links-->

[Mitme rentniku rakenduse Azure majutusteenuse]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
