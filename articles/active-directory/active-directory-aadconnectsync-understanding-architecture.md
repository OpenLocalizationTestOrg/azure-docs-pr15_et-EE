<properties
   pageTitle="Azure'i AD-ühendus sünkroonimine: ülesehituse mõistmine | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse Azure'i AD-ühenduse sünkroonimine arhitektuur ja selgitatakse termineid."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure'i AD-ühendus sünkroonimine: ülesehituse mõistmine
Selles teemas käsitletakse lihtsa arhitektuur sünkroonimise Azure'i AD-ühenduse. Mitmed aspektid on sarnane selle eelkäijad MIIS 2003, ILM 2007 ja FIM 2010. Azure'i AD-ühendus sünkroonimine on nende tehnoloogiate areng. Kui olete tuttav varasemate mobiilsidevõrgu, selle teema sisu on teile tuttavad ka. Kui olete kasutanud sünkroonimise, siis see teema on teie jaoks. See ei ole siiski teada üksikasju, see teema on edukalt teha kohandused (nn sünkroonimine mootor selles teemas) sünkroonimise Azure'i AD-ühenduse nõue.

## <a name="architecture"></a>Arhitektuur
Sync engine loob objekte, mis on talletatud ühendatud andmeid mitmest allikast integreeritud ülevaate ja haldab identiteedi teavet nendele andmeallikatele. See integreeritud vaade määratakse saadud ühendatud andmeallikate ja reeglikomplekti, et määrata, kuidas seda teavet töödelda identiteedi teabele.

### <a name="connected-data-sources-and-connectors"></a>Ühendatud andmeallikate ja konnektoreid.
Sync engine töötleb identiteedi teavet erinevat tüüpi andmete hoidlate, nt Active Directory või SQL serveri andmebaasi. Iga mis korraldab oma andmete andmebaasi-vormingus ja standard andmepääsu meetodid, mis pakub andmete talletuskoht on võimalik andmete allikas testversiooni sync engine. Andmete hoidlate Sünkroonimisrakenduse abil sünkroonitud nimetatakse **ühendatud andmeallikate** või **ühendatud kataloogide** (CD-d).

Sync engine kapseloi suhtlemine ühendatud andmeallika nimega **konnektor**mooduli sees. Erinevat tüüpi ühendatud andmeallika on teatud konnektor. Konnektor vaste vajalikud toimingud, mis ühendatud andmeallika mõistab vormingusse.

Konnektorid helistada API identiteedi teabevahetuseks (lugemine ja kirjutamine) ühendatud andmeallikaga. Samuti on võimalik lisada kohandatud konnektori abil Laiendatav Ühenduvus raames. Järgmisel joonisel konnektori ühenduse loomise viisi ühendatud andmeallika sync engine.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Saate andmevoo suunda, kuid ei saa flow mõlemast suunast korraga. Teisisõnu, konnektori saab konfigureerida lubada flow kaudu ühendatud andmeallika sünkroonimine mootor või sünkroonimine mootor andmeallikaga ühendatud andmete, kuid ainult üks need toimingud võivad tekkida korraga ühe objekti ja atribuut. Suunas võivad olla erinevad eri objektid ja muu atribuute.

Konnektor konfigureerimiseks saate määrata objekti tüübid, mida soovite sünkroonida. Täpsustada, mis tüüpi objekti määratleb objekte, mis sisalduvad sünkroonimine ulatust. Järgmiseks tuleb valida atribuute sünkroonida, mis on tuntud ka atribuut loendisse. Neid sätteid saab muuta igal ajal muutustega oma äri reeglid. Azure'i AD-ühenduse installimise viisard kasutamisel need sätted on konfigureeritud teie jaoks.

Objektide eksportimiseks ühendatud Andmeallika atribuut lisamise loend peab sisaldama vähemalt looma kindla objekti tüüp ühendatud andmeallika miinimum atribuute. Näiteks peab atribuudi lisamine loendisse eksportimine kasutaja objekti Active Directory, kuna kõigil objektidel kasutaja Active Directory peab olema atribuut **sAMAccountName** , määratletud kaasatud atribuut **sAMAccountName** . Installimise viisard ei uuesti, saate selle konfiguratsiooni.

Kui ühendatud andmeallika kasutab strukturaalset osad sektsioonid või ümbriste korraldamiseks objektid, nagu saate piirata alad ühendatud andmeallika esitatud lahenduse kasutatavate.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Sisemise struktuuri sünkroonimine engine nimeruum
Kogu sünkroonimine engine nimeruumi koosneb kahte nimeruumi, kus talletatakse identiteedi teavet. Kahe nimeruumid on:

- Konnektori ruumi (CS)
- Metaversumi (MV)

**Konnektori ruumi** on koondusalal, mis sisaldab määratud objektide kaudu ühendatud andmeallika ja atribuut lisamise loendis määratud atribuute. Sync engine kasutab konnektori ruumi määramiseks, mis on muutunud ühendatud andmeallika ja etapp sissetuleva muudatused. Sync engine kasutab ka konnektori ruumi etapp väljamineva muudatuste eksportimiseks ühendatud andmeallikas. Sync engine säilitab erinevate konnektori ruumi lavastus ala iga konnektor.

Mõne koondusalal abil sync engine jääb sõltumatu ühendatud andmeallikate ja nende kättesaadavuse ei mõjuta. Seetõttu saab töödelda abil andmete koondusalal identiteedi teavet igal ajal. Sync engine saate taotleda ainult ühendatud andmeallika sees identiteedi teavet, mis on ühendatud andmeallika ei ole veel saanud, mis vähendab võrguliiklust sync engine ja ühendatud andmeallika vahel viimase side seanss lõpetatakse või tõuketeatised välja ainult muudatused alates tehtud muudatusi.

Lisaks sünkroonimine engine talletab olek teavet kõigi objektide, et see etapid konnektori ruumis. Uute andmete saabumisel sünkroonimine engine hindab alati kas andmed on juba sünkroonitud.

**Metaversumi** on ühendatud andmeid mitmest allikast, andes ühe üldise, integreeritud silmas kombineeritud kõigi objektide liidetud identiteedi teavet sisaldav talletuskoht. Metaversumi objektid luuakse identiteedi teavet, mis on ühendatud andmeallikate ja reeglid, mis võimaldavad teil kohandada sünkroonimine kogumi alusel.

Järgmisel joonisel nimeruumi konnektori ruumi ja metaversumi nimeruumi sünkroonimine mootori.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Sünkroonimine engine identiteedi objektid
Objektide sync engine on ühendatud andmeallika kummagi objektide kujutised või nende objektid on integreeritud vaade, mille mootor sünkroonida. Iga sünkroonimine engine objekt peab olema globaalne ainuidentifikaator (GUID). GUID pakuvad andmeterviklus ja objektide kiire seoseid.

### <a name="connector-space-objects"></a>Objektide konnektor ruumi
Kui sünkroonimine engine suhtleb ühendatud andmeallika, loeb identiteedi teavet ühendatud andmeallika ja kasutab seda teavet konnektori ruumis kujutis identiteedi objekti loomiseks. Ei saa luua ega nende objektide kustutamine ükshaaval. Siiski saate käsitsi kustutada kõigi objektide konnektor ruumi.

Kõigil objektidel konnektori ruumis on kaks atribuute:

- Globaalne ainuidentifikaator (GUID)
- Eraldusnimi (DN tuntud ka kui)

Kui ühendatud andmeallika määrab objekti kordumatu atribuut, siis võib olla objektide konnektor alale ankur atribuut. Atribuut ankur kordumatult ühendatud andmeallika objekti. Sync engine kasutab ankur esinduse selle objekti ühendatud andmeallika leidmiseks. Sünkroonimine engine eeldab, et objekti ankur muutub kunagi eluea objekti.

Paljude konnektorid abil teadaolevad kordumatu kinnistamine automaatselt iga objekti importimisel. Näiteks kasutab Active Directory konnektor **atribuutiFederatedIdentityDistinguishedNameatribuuti** ankur. Ühendatud andmeallikate, mis pakuvad selgelt määratletud kordumatut tunnust, saate määrata ankur genereerimine konnektori konfiguratsiooni osana.

Selle juhul ankur ehitatud üks või mitu objekti kordumatu atribuutide tippige, ei, milliseid muudatusi ja kordumatult tuvastab objekti konnektori ruumis (nt töötaja numbri või kasutaja ID).

Konnektori ruumi objekti võib olla üks järgmistest:

- Lavastus objekt
- Kohatäite

### <a name="staging-objects"></a>Lavastus objektid
Lavastus objekti tähistab kaudu ühendatud andmeallika eksemplari määratud objekti tüübid. Lisaks GUID ja Eraldusnimi, lavastus objekt on alati väärtus, mis näitab, et objekti tüüp.

Lavastus objekte, mis on imporditud alati olema ankur atribuudi väärtus. Lavastus objekte, mis on sünkroonimine mootori äsja ette valmistatud ja parajasti ühendatud andmeallika loomist ei saa ankur atribuudi väärtus.

Lavastus objektide teha ka praegusi väärtusi, business atribuudid ja funktsionaalseid sünkroonimine sünkroonimise mootori vajalikku teavet. Funktsionaalseid teave sisaldab lipud, mis näitavad värskendusi, mis on etapiviisilise lavastus objekti tüüp. Kui lavastus objekt on saanud identiteedi teavet kaudu ühendatud andmeallika, mis pole veel töödeldud, objekti lipuga **Ootel impordi**. Kui lavastus objekt on identiteedi uut teavet, mis pole veel ühendatud andmeallika eksporditud, on see lipuga **Ootel ekspordi**.

Lavastus objekti saab objekti importimine või eksportimine objekti. Sync engine loob objekti importimine objekti saadud ühendatud andmeallika teabe abil. Kui sünkroonimine mootor saab teave uue objekti, mis vastab konnektor valitud objekti tüüp, loob see objekti importimine konnektori ruumis nimega kujutis ühendatud andmeallika objekti.

Järgmisel joonisel impordi objekti, mis tähistab objekti ühendatud andmeallikas.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

Sync engine loob objekti ekspordi metaversumis objekti teabe abil. Ekspordi objektid eksporditakse jooksul järgmise ühendatud andmeallikas. Sync engine seisukohast ekspordi objektide pole ühendatud andmeallika veel. Seetõttu ankur atribuuti objekti eksportimine pole saadaval. Pärast seda saab objekti sünkroonimise engine, loob ühendatud andmeallika ankur atribuudi objekti kordumatu väärtus.

Järgmisel joonisel, kuidas objekti eksportimine on loodud metaversumis identiteedi teabe abil.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

Sync engine kinnitab impordikorralduses objekti kaudu ühendatud andmeallika objekti ekspordi. Ekspordi objektid muutuvad objektide importimine, kui sünkroonimine mootor saab neid kaudu ühendatud andmeallika järgmise importimisel.

### <a name="placeholders"></a>Kohatäidete
Sync engine kasutab tasapinnalise nimeruum objektide talletamiseks. Siiski kasutada mõnda ühendatud andmeallikatest, nt Active Directory hierarhilise nimeruum. Teavet hierarhilise nimeruumi muutuda tasapinnalise nimeruumi, kasutab sünkroonimine engine säilitada hierarhia kohatäited.

Iga kohatäide objekti hierarhilise nimi, mis on imporditud ei sünkrooni engine, kuid on vaja ehitada hierarhilise nime osa (nt ettevõtte üksus). Need täita lünki, mis on loodud ühendatud andmeallika viiteid objekte, mis on lavastus objektide konnektor alale.

Sync engine kasutab ka kohatäidete viidatud objekte, mis pole veel imporditud talletamiseks. Näiteks kui sünkroonimine on konfigureeritud halduri atribuuti *Abbie Spencer* objekti ja saadud väärtus on objekti, mis ei ole veel, imporditud, näiteks *CN Lee Sperry CN = = kasutajad, näiteks Põhiliselt fabrikam AV = = com*, halduri teave on talletatud konnektori ruumis kohatäidetena. Kui haldur objekti hiljem imporditakse, kirjutatakse kohatäite objekti lavastus objekti, mis tähistab ülemus.

### <a name="metaverse-objects"></a>Metaversumi objektid
Metaversumi objekti sisaldab liidetud vaate selle sünkroonimine engine on lavastus objektide konnektor alale. Sünkroonimine engine loob metaversumi objektid objektide importimine teabe abil. Ühe metaversumi objekti saab linkida mitme konnektori ruumi objekti, kuid konnektori ruumi objekti ei saa olla lingitud rohkem kui üks metaversumi objekti.

Metaversumi objekte ei saa käsitsi loodud või kustutatud. Sync engine kustutab automaatselt metaversumi objekte, mis on mis tahes konnektori ruumi objekti link konnektori ruumis.

Objektide ühendatud andmeallika vastendamiseks vastava objekti tüüp sees metaversumi sünkroonimine engine pakub laiendatav skeemiga eelmääratletud seatud objekti tüübid ja seotud atribuute. Saate luua uue objekti tüübid ja atribuudid metaversumi objektid. Atribuute saab ühe väärtusega või mitme väärtusega ja atribuudi tüüpi võib olla stringid, viited, arvude ja kahendmuutujaga väärtused.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Lavastus objektid ja metaversumi objektide vahelisi seoseid
Sees sünkroonimine engine nimeruumi, andmevoo lubamiseks lavastus objektid ja metaversumi objektide link seos. Lavastus objekti, mis on seotud metaversumi objekti on nimetatakse **liitunud objekti** (või **Connectori objekt**). Lavastus objekti, mis ei ole seotud metaversumi objekti on nimetatakse **makseasutustest objekti** (või **disconnector objekt**). Terminite liitunud ja makseasutustest on eelistatakse ajage ei vastuta andmete importimisse ja eksportimisse ühendatud kataloogi konnektorid.

Kohatäidete lingitakse kunagi metaversumi objekti

Ühendatud objekt sisaldab lavastus objekti ja selle lingitud seoses ühe metaversumi objekti. Ühendatud objektide kasutatakse atribuutide väärtused objekti konnektori ruumi ja metaversumi objekti vahelise sünkroonimiseks.

Kui lavastus objekti muutub ühendatud objekti sünkroonimise ajal, saate atribuute flow lavastus objekti ja metaversumi objekti vahel. Atribuudi voog on kahesuunaline ja on konfigureeritud, kasutades atribuut reeglite import ja eksport atribuut reegleid.

Ühe konnektori ruumi objekti saab linkida ainult ühe metaversumi objekti. Siiski iga metaversumi objekti saab linkida mitme konnektori ruumi objektide sama või muu konnektor tühikud, nagu on näidatud järgmisel joonisel.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

Lingitud objekti lavastus ja metaversumi objekti seos on püsiv ja saab eemaldada ainult teie määratud reeglid.

Lahtiühendatud objekt on lavastus objekti, mis ei ole seotud mis tahes metaversumi objekti. Atribuut lahtiühendatud objekti väärtused ei töödelda mis tahes täiendavaid metaversumi sees. Sünkroonimise mootori atribuudiväärtused ühendatud andmeallika vastava objekti ei värskendata.

Lahtiühendatud objektide abil saate talletada identiteedi teavet sünkroonimine mootor ja seda hiljem töödelda. Säilitamise lavastus objekti konnektori ruumis lahtiühendatud objekt on mitmeid eeliseid. Kuna süsteem on juba etapiviisilise selle objekti kohta nõutav teave, ei ole vaja luua kujutis objekti järgmise importimisel kaudu ühendatud andmeallika. Sellisel viisil sünkroonimine engine on alati täieliku ülevaate ühendatud andmeallika isegi juhul, kui praegune ühendus andmeallikaga ühendatud puudub. Lahtiühendatud objektide saab ümber ühendatud objektid ja vastupidi, sõltuvalt teie määratud reegleid.

Objekti importimine luuakse lahtiühendatud objektina. Objekti ekspordi peab olema ühendatud objekti. Süsteemi loogika jõustab see reegel ja kustutab iga ekspordi objekti, mis pole ühendatud objekt.

## <a name="sync-engine-identity-management-process"></a>Sünkroonimise mootori identiteedi haldamine protsess
Identiteedi haldamine protsess juhtelemendid identiteedi teabe värskendamise vahel ühendatud erinevatest andmeallikatest. Identiteedi haldamise esineb kolm protsessid:

- Importimine
- Sünkroonimine
- Eksportimine

Importimise käigus sünkroonimine engine hindab sissetulevaid identiteedi teavet kaudu ühendatud andmeallika. Muudatuste tuvastamisel see loob uue lavastus objektide või värskendab olemasoleva lavastus objektide sünkroonimiseks konnektori ruumis.

Sünkroonimise käigus sünkroonimine engine värskendused metaversumi konnektori ruumis muutuste kajastamiseks ja värskendused konnektori ruumi metaversumis muutuste kajastamiseks.

Eksportimise käigus sünkroonimine engine sunnib välja muudatused, mis on etapiviisilise esitada objektid ja mis on lipuga märgitud nagu ootel ekspordi.

Järgmisel pildil on näha, kus iga protsesside esineb identiteedi teabevoogude ühe ühendatud andmeallika teise.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Importimise protsess
Importimise käigus hindab sünkroonimine engine identiteedi teavet värskendused. Sünkroonimine engine võrdleb saadud lavastus objekti teabega identiteedi ühendatud andmeallika teabe identiteedi ja määrab, kas lavastus objekti nõuab värskendusi. Kui on vaja värskendada lavastus objekti uute andmetega, lavastus objekt on märgitud, nagu ootel impordi.

Objektide konnektor alale enne sünkroonimise peatuspaikadena, saab sünkroonimine mootor protsessi ainult identiteedi teavet, mis on muutunud. Selle protsessi pakub järgmised eelised:

- **Tõhusa sünkroonimine**. Sünkroonimisel töödeldud andmete summa on minimeeritud.
- **Tõhusa resynchronization**. Saate muuta, kuidas sünkroonimine mootor töötleb identiteedi teavet ilma taastamine sync engine andmeallikale.
- **Võimalus eelvaate sünkroonimine**. Saate vaadata ka sünkroonimise kinnitamaks, et oma identiteedi haldamine protsessi oletused on õiged.

Iga objekti määratud konnektor, sync engine esmalt üritab leida kujutis objekti konnektori ruumi konnektor. Sünkroonimine engine lavastus kõigil objektidel konnektori ruumi ja vastavate lavastus objekti, mis sisaldab vastavaid ankur atribuut leidmiseks. Kui pole olemasoleva lavastus objekti on kattuvad ankur atribuut, proovib sünkroonimine engine leida vastavate lavastus objekti eristatav nimi.

Kui sünkroonimine engine leiab lavastus objekti, mis vastab Eraldusnimi, kuid mitte ankur, järgmised teisiti käitumine ilmneb:

- Kui konnektori ruumis asub objektil pole ankur, siis sünkroonimine engine eemaldab selle objekti konnektori ruumi ja märgib metaversumi objekti, mis see on lingitud nimega **proovige ettevalmistamise kohta järgmise sünkroonimise käivitamine**. See loob siis uus impordi objekt.
- Kui objekt asub konnektori ruumis on ankur, sync engine eeldab, et see objekt on ümber nimetatud või kustutatud ühendatud kataloogis. Seda määrab ajutine, uue Eraldusnimi konnektori ruumi objekti nii, et saate selle etapi sissetulevate objekti. Vana objekti saab siis **siirdamiseks**, ümbernimetamine või kustutamine olukorda lahendada importimiseks konnektori ootamine.

Kui sünkroonimine engine otsib lavastus objekti, mis vastab määratud konnektor, seda määrab milliseid muudatusi rakendada. Näiteks sünkroonimine engine võib Nimeta ümber või Kustuta ühendatud andmeallika objekti või seda värskendada ainult objekti atribuutide väärtused.

Värskendatud andmete lavastus objektid on tähistatud nagu ootel impordi. Erinevat tüüpi ootel impordi on saadaval. Sõltuvalt protsessi põhjustanud lavastus objekti konnektori ruumis on üks järgmistest ootel impordi tüübid.

- **Mitte keegi**. Muudatustest mis tahes lavastus objekti atribuudid on saadaval. Sünkroonimine mootor ei märgi seda tüüpi nagu ootel impordi.
- Saate **lisada**. Uue impordi objekti konnektori ruumis on lavastus objekt. Sünkroonimine engine lipud seda tüüpi nagu ootel impordi metaversumis töötlemiseks.
- **Värskenda**. Sünkroonimine mootor otsib vastavate lavastus objekti konnektori ruumi ja seda tüüpi ootel impordi lipud, et atribuutide värskenduste saab töödelda metaversumis. Värskendused hõlmavad objekti ümbernimetamine.
- Saate **kustutada**. Sünkroonimine mootor otsib vastavate lavastus objekti konnektori ruumi ja seda tüüpi ootel impordi lipud, et ühendatud objekti saab kustutada.
- **Kustuta/lisada**. Sünkroonimine engine leiab vastavate lavastus objekti konnektori ruumis, kuid objekti tüübid ei vasta. Sel juhul on Kustuta lisamine muudatus on etapiviisilise. A Kustuta lisamine muutmine näitab sync engine, et täieliku resynchronization selle objekti peab jääma aastasse erinevat rakenduvad sellele objektile objekti tippimisel muudatused.

Seades ootel impordi lavastus objekti, on võimalik märkimisväärselt vähendada töödelda sünkroonimise ajal, kuna seda lubab teha ainult objektid, mida on värskendatud andmete töötlemiseks süsteemi andmed.

### <a name="synchronization-process"></a>Sünkroonimine
Sünkroonimise koosneb kahest seotud protsessid.

- Sissetulev sünkroonimise, kui metaversumi sisu värskendatakse abil andmete konnektori ruumis.
- Väljamineva sünkroonimise, kui vaheline konnektor sisu värskendatakse andmed metaversumi abil.

Etapiviisilise konnektori ruumis teabe abil sissetuleva sünkroonimine loob metaversumi andmeid, mis on talletatud ühendatud andmeallikate integreeritud vaate. Kõigi lavastus objektide või ainult need ootel impordi andmed on koondatud, olenevalt sellest, kuidas reegleid on konfigureeritud.

Väljamineva sünkroonimise protsessi värskendusi eksportida objektid, kui metaversumi objektide muutmine.

Sissetuleva sünkroonimise loob identiteedi teavet, mis on saadud ühendatud andmeallikate metaversumi integreeritud vaadet. Sünkroonimine otsimootori saate uusima identiteedi teavet, mis on kaudu ühendatud andmeallika protsessi identiteedi teavet igal ajal.

**Sissetulev sünkroonimine**

Sissetuleva sünkroonimise hõlmab järgmisi toiminguid:

- **Säte** (nimetatakse ka **projektsiooni** kui see on oluline, et eristada seda toimingut väljaminev sünkroonimise ettevalmistamise). Sync engine loob uue metaversumi objekti lavastus objekti põhjal ja neid linke. Säte on taseme objekti toiming.
- **Liituda**. Sync engine lingid lavastus objekti metaversumi objekti. Ühenduse on taseme objekti toiming.
- **Impordi atribuudi voog**. Sünkroonimine engine värskendab atribuudiväärtused, nimetatakse atribuudi voog metaversumi objekti. Impordi atribuudi voog on atribuut taseme toiming, mis nõuab lavastus objekti ja metaversumi objekti.

Sätte on ainus toiming, mis loob objektide metaversumi. Säte mõjutab ainult impordi objekte, mis on lahtiühendatud objektid. Sätte ajal sünkroonimine engine loob metaversumi objekti, mis vastab impordi objekti objekti tüüp ja seostab nii objektide, luues ühendatud objekti vahel.

Protsessi ühendus luuakse ka seos objektide importimine ja metaversumi objekti. Liitumine ja sätte erinevus on, et liitumise protsessi nõuab olemasoleva metaversumi objekti, kus sätte protsess loob uue metaversumi objekti lingitud objekti importimine.

Sünkroonimise mootori püüab liituda impordi objekti metaversumi objekti abil sünkroonimine reegel konfiguratsioonis määratud kriteeriumidele.

Sätte ja sellega siis liituda protsesside ajal sünkroonimine engine lingid lahtiühendatud objekti metaversumi objekti, muutes liitunud. Pärast nende toimingute objekti taseme on lõpule viidud, saate sünkroonimise mootori värskendada seotud metaversumi objekti atribuudiväärtused. Seda nimetatakse impordi atribuudi voog.

Kõigi objektide importimine, mis viivad uued andmed ja lingitakse metaversumi objekti ilmneb impordi atribuudi voog.

**Väljamineva sünkroonimine**

Väljamineva sünkroonimise värskenduste eksportida objektid, kui metaversumi objekti muuta, kuid ei kustutata. Väljamineva sünkroonimise eesmärk on hinnata, kas metaversumi objektide muudatused nõuavad värskendused lavastus objektide konnektor tühikuid. Mõnel juhul muudatused saate nõuda, et värskendada objektide konnektor kõik tühikud. Lavastus objekte, mis on muutunud on lipuga märgitud nagu ootel ekspordi, muutes need eksportida objektid. Need objektid on hiljem lükata ühendatud andmeallika eksportimise käigus ekspordi.

Väljamineva sünkroonimine on kolm protsessid.

- **Ettevalmistamise**
- **Deprovisioning**
- **Atribuudi voog eksportimine**

Ettevalmistamise ja deprovisioning on mõlemad objekti taseme toimingud. Deprovisioning sõltub ettevalmistamise, kuna ainult ettevalmistamise saate selle kontaktiga. Deprovisioning käivitatakse kui ettevalmistamise eemaldab seost metaversumi objekti ja objekti ekspordi.

Ettevalmistamise käivitatakse alati, kui muudatused rakendatakse metaversumi objektide. Kui muudatusi ei tehta metaversumi objektidele, sünkroonimise mootori saate teha mõnda järgmistest toimingutest osana ebausaldusväärsete:

- Looge ühendatud objektid, kus metaversumi objekti on lingitud vastloodud ekspordi objekti.
- Ühendatud objekti ümbernimetamine.
- Disjoin metaversumi objekti ja lavastus lahtiühendatud objekti objektide vahelisi seoseid.

Kui ettevalmistamise nõuab sünkroonimine engine luua uue konnektori objekti, lavastus objekti metaversumi objekti seotud on alati objekti ekspordi, kuna objekt pole veel olemas ühendatud andmeallika.

Kui ettevalmistamise nõuab sünkroonimine engine disjoin ühendatud objekti, lahtiühendatud objekti, et deprovisioning käivitatakse. Deprovisioning käigus kustutatakse objekti.

Ajal deprovisioning, ekspordi objekti kustutamine ei füüsilise objekti kustutamine Objekti lipuga on **Kustutatud**, mis tähendab, et Kustutustoiming on etapiviisilise objekti.

Ekspordi atribuudi voog esineb ka käigus väljaminev sünkroonimise sarnane nii, et impordi atribuudi voog ilmneb sissetuleva sünkroonimise ajal. Ekspordi atribuudi voog toimub ainult metaversumi ja ekspordi objekte, mis on ühendatud.

### <a name="export-process"></a>Eksportimise käigus
Eksportimise käigus uurib sünkroonimine engine ekspordi kõik objektid, mis on lipuga märgitud nagu ootel konnektori ruumis ekspordi ja seejärel saadab värskendused ühendatud andmeallika.

Sync engine saate määrata ekspordi edu, kuid seda ei saa piisavalt määratleda identiteedi haldamine toimingu lõpuleviimist. Objektide ühendatud andmeallika saate alati muuta muude protsesside. Kuna sünkroonimine mootor ei ole pidev ühendus andmeallikaga ühendatud, pole piisavalt ühendatud andmeallika põhjal ainult eduka ekspordi teatis tehtud oletused objekti atribuutide kohta.

Näiteks ühendatud andmeallika protsess võib muutuda objekti atribuudid naasmiseks algväärtuste (ehk teisisõnu öeldes ühendatud andmeallika võib kirjutada väärtused kohe, kui andmed on sünkroonimise mootori lükata ja ühendatud andmeallika on rakendatud).

Sünkroonimine engine poed eksportimine ja importimine olek teavet iga lavastus objekti. Kui atribuut lisamise loendis määratud atribuutide väärtused on muutunud viimatist eksportimist, talletamist importimine ja eksportimine olek lubab sünkroonimine mootor reageerida. Sünkroonimine mootor kasutab importimine atribuutide väärtused, mis on ühendatud andmeallika eksporditud kinnitamiseks. Võrdlus imporditud ja eksporditud teave, nagu on näidatud järgmisel joonisel võimaldab sünkroonimine engine määratlemiseks ekspordi õnnestumise või kui see on vaja korrata.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Näiteks kui sünkroonimine engine ekspordib atribuut C, 5, väärtus on ühendatud andmeallikas, see talletab C = 5 ekspordi olek mällu. Täiendavad ekspordi selle objekti tulemuste püüdes eksportida C = 5 ühendatud andmeallika uuesti, kuna sünkroonimine engine eeldab, et see väärtus on pole püsivalt seotud objekti (st kui muu väärtuse imporditud viimati kaudu ühendatud andmeallika). Ekspordi mälu on tühi, C = 5 saabumisel objekti imporditoimingu käigus.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
