<properties title="" pageTitle="Azure'i dokumentatsiooni - kirjutamine, laadi ja kõneposti petavad leht" description="Laadi ja kõneposti teabe abil saate luua tehnilise sisu Azure dokumentatsiooni keskele." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="12/16/2014" ms.author="glenga" />

#<a name="writing-azure-documentation---style-and-voice-cheat-sheet"></a>Azure'i dokumentatsiooni - kirjutamine, laadi ja kõneposti petavad leht

Siin on täieliku leht, mis sisaldab näpunäiteid selle kohta, kuidas kirjutada tehnilise artikleid Azure teenused ja tehnoloogiad. Need juhised kehtivad uute dokumentide loomisel või olemasoleva dokumentatsiooni värskendamine.

Tühi vähemalt, palun:

- Õigekirja ja grammatika kontrollimine oma teemade isegi juhul, kui teil on lõikamine ja seda teha Wordi kleepida.
- Kasutage juhusliku ja sõbralikke kõneposti--nagu te ei räägi teisele isikule üks-ühele.
- Kasutage lihtsaid lauseid. Need on kergem mõista ja nad on hõlpsam tõlgitud inim- ja arvuti tõlkijad.

Järgmised jaotised sisaldavad rohkem üksikasju:

+ [Sõbralik kõneposti kasutamine]
+ [Kaaluge lokaliseerimine ja masintõlkega]
+ [Vaadata teiste laad ja kõneposti probleemid]


##<a name="use-a-customer-friendly-voice"></a>Sõbralik kõneposti kasutamine

Soovime järgige neid põhimõtteid, kui me kirjutada tehnilise sisu Azure. Me ei pruugi alati olemas, kuid läheb vaja proovima!

- **Kasutage igapäevaseid sõnu**: proovige kasutada loomulikus keeles sõnad klientide kasutamine; olla väiksem formaalne, kuid mitte vähem tehniliste; näiteid, mis selgitavad uue põhimõtet.

- **Kirjutage lühidalt**: ei raiska sõnade ja ei üle selgitada. Olema positiivsed ja ei kasuta täiendavaid sõnu või palju täpsustus. Säilita laused lühikesed ja lakoonilised. Hoidke oma värskendustest teema. Kui tööülesande kvalifikatsioon, pange see lause või lõigu algusesse. Ka hoidma võimalikult palju märkmeid. Kui saate selle salvestada sõnu, kasutage kuvatõmmis.

- **Lihtne skannida**: kõige olulisemad asjad esmalt panna. Jaotiste abil saate patakas pikk toimingute paremini hallataval rühmadena (toimingute üle 12 juhistega on tõenäoliselt liiga pikk) järgmist. Kasutage seda lisab selgust kuvatõmmis.

- **Kuva empaatia**: kasutage toetav kostab artiklist ja hoidke lahtiütlemisi; minimaalse. Helistage ausalt välja alad, mis on üsna tülikas klientidele. Veenduge, et see artikkel keskendub kliendile oluline, ei anna tehnilise loengul.

##<a name="consider-localization-and-machine-translation"></a>Kaaluge lokaliseerimine ja masintõlkega
Meie tehnilised artiklid tõlgitakse paljude muude keelte ja mõned on muudetud teatud turgude jaoks. Inimesed võib ka kasutada masintõlke veebis tehnilise artiklid. Jah, kirjutada meeles järgmisi juhiseid:

- **Veenduge, et see artikkel sisaldab grammatika, õigekirja, või Kirjavahemärgivigade**: see on, mida me peaks üldiselt. Allahindlusest Valimisklahvistik 2.0 on lihtne õigekirjakontrolli, kuid ei peaks ka kleepida (sulatatud HTML) sisu artiklis sõna, mis on tugevam õigekirja- ja grammatikakontrolli sisse.

- **Veenduge, et teie lauset võimalikult vähe**: ühendi lausete või kettide klauslitest raskendada tõlge. Kui te seda ilma liiga liigsete või kõlav imelik laused jagatakse. Me ei taha artikleid, kas ebaloomulik keeles kirjutatud.

- **Kasutage lihtne ja terviklik lause ehituse**: järjepidevuse on parem tõlge. Vältida parentheticals ja asides ja teemaga nimega võimalikult lause alguses. Tutvuge paar avaldatud teemat – kui teema on sõbralik, lihtne laadi read, kasutage seda mudel.

- **Kasutage järjepideva sõnastuse ja suurtähestuse**: uuesti järjepidevus on võti. Azure'i kasutab lause kest pealkirju, nii kunagi Suurtähesta sõna, kui see pole lause või proper nimisõna alguses.

- **Kaasa "small sõnad"**: sõnad, mida me kaaluda väike- ja ebaolulised inglise keeles, kuna need on aru saanud kontekstis (nagu "a", "mida", "mida" ja "on") on väga oluline masintõlkeks – veenduge, et saate need kaasata!

##<a name="other-style-and-voice-issues-to-watch-for"></a>Vaadata teiste laad ja kõneposti probleemid

- Ära katkesta üles kommentaar või asides juhiseid.

- Juhised, mis sisaldavad Koodilõigud, pannakse Lisateave etappi koodi kommentaaridena. See vähendab teksti peavad inimesed läbi lugenud ja olulist teavet saab kopeerida Koodiprojekti mis kood teeb, kui ta vaadata seda hiljem meelde tuletada.

- Ametlik toote nimi on "Microsoft Azure", kuid võib peaaegu alati lihtsalt öelda "Azure" nagu "Azure Mobile Services".

- Ärge looge lühendeid, mis algavad "MA" või "A." Kasutada lihtsalt "Azure" esimese viide enne teenuse või funktsiooni nime ja seejärel pukseerige see (nt "Azure Mobile Services" muutub "Mobile Services" pärast esmakordsel kasutamisel). Proovige vältida lühendeid üldiselt – need lihtsalt ajage inimesed.

- Azure'i kasutab lause kest kõik pealkirjad.

- Kasutage "Sisselogimistõrgete" ei "Sisselogimine ja."

- Kaasake iga lause, mis eelneb loendi või koodi koodilõigu sõnu "jälgitavad" või "järgmine".

- "SQL-andmebaasi" Azure funktsioon. "SQL-andmebaasi" on andmebaasi eksemplari, mis töötab SQL-andmebaasi.

- Azure'i salvestusruumi sisaldab mitme "andmete halduse teenused" tabeli teenuse, bloobimälu teenuse ja teenuse järjekorda. (See on nn "Azure'i tabeli salvestusteenus".)




###<a name="contributors-guide-links"></a>Osaliste juhend lingid

- [Artikli ülevaade](./../README.md)
- [Juhised artiklite register](./contributor-guide-index.md)



<!--Anchors-->
[Sõbralik kõneposti kasutamine]: #use-a-customer-friendly-voice
[Kaaluge lokaliseerimine ja masintõlkega]: #consider-localization-and-machine-translation
[vaadata teiste laad ja kõneposti probleemid]: #other-style-and-voice-issues-to-watch-for
