<properties
    pageTitle="KKK: Azure'i AD parooli haldus | Microsoft Azure'i"
    description="Korduma kippuvad küsimused (KKK) parooli haldustoimingute kohta Azure AD, sh parooli lähtestada, registreerimine, aruanded ja tagasikirjutusega kohapealse Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="password-management-frequently-asked-questions"></a>Korduma kippuvad küsimused parooli haldus

> [AZURE.IMPORTANT] **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).

Järgmisena kirjeldame mõningaid korduma kippuvaid küsimusi parooli haldamisega seotud kõik asjad.

Kui te ei leia ise küsimus, mida te ei tea, vastata või otsite abi mõne kindla probleemi lahendamiseks, mis teil on ees, saate lugeda allpool näha, kui oleme käsitlenud seda juba.  Kui meil pole seda veel teinud, ärge muretsege! Võite esitada küsimusi teil on, mis ei kuulu siin [Azure AD Foorumid](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) ja võtame teile niipea, kui võimalik.

Korduma Kippuvate on jaotatud järgmisteks alljaotisteks järgmistest jaotistest.

- [**Küsimused parooli lähtestamine registreerimine**](#password-reset-registration)
- [**Küsimused parooli lähtestamine**](#password-reset)
- [**Küsimused parooli haldus aruanded**](#password-management-reports)
- [**Parooli tagasikirjutusega seotud küsimused**](#password-writeback)

## <a name="password-reset-registration"></a>Parooli lähtestamise registreerimine
 - **Q: Kas minu kasutajad registreerida oma parooli lähtestamine andmete?**

 > **A:** Jah, kui on lubatud parooli lähtestamine ja need on litsents, nad minna parooli lähtestamiseks registreerimise portaali aadressil http://aka.ms/ssprsetup registreerida oma autentimise teavet koos parooli lähtestamine. Kasutajate registreerida ka Accessi paneeli http://myapps.microsoft.com läheb, klõpsates vahekaardi profiil ja klõpsake suvandi parooli lähtestamise Register. Lugege lisateavet selle kohta, kuidas kasutajate parooli lähtestada kasutaja parooli lähtestamine konfigureeritud hankimine lugedes konfigureeritud.

 - **Q: saate määrata parooli lähtestamine andmete minu kasutajate nimel?**

 > **A:** Jah, saate seda teha DirSync või PowerShelli või [Azure'i haldusportaal](https://manage.windowsazure.com) või Office'i administraator portaali kaudu. Lisateavet selle funktsiooni sisse täiustatud Privaatsus Azure AD MFA ja parooli lähtestamiseks telefoninumbrid ja lugedes saate teada, kuidas andmeid kasutatakse parooliga ajaveebipostitust lähtestada.

 - **Q: Kas ma sünkroonida turvaküsimused kaudu kohapealne andmeid?**

 > **A:** Ei, see ei ole võimalik täna, kuid me kaalute.

 - **Q: Kas minu kasutajad registreerida andmed nii, et teised kasutajad ei näe neid andmeid?**

 > **A:** Jah, parooli lähtestamiseks registreerimise portaalis andmete registreerimisel saab salvestatakse see privaatne autentimise väljadele, mis on ainult nähtavad globaalse administraatorid ja kasutaja ise. Lisateavet selle funktsiooni sisse täiustatud Privaatsus Azure AD MFA ja parooli lähtestamiseks telefoninumbrid ja lugedes saate teada, kuidas andmeid kasutatakse parooliga ajaveebipostituse lähtestada.

 - **Q: Kas minu kasutajad on enne, kui nad saavad kasutada parooli lähtestamist registreerida?**

 > **A:** Ei, kui määrate piisavalt autentimise teavet nende nimel, kasutajad ei pea registreerida. Parooli lähtestamine töötavad hästi, kui teil on õigesti vormindatud väljadele kataloogis talletatud andmed. Lugege lisateavet lugedes saate teada, kuidas andmeid kasutab parooli lähtestamine.

 - **Q: kas sünkroonida või määrata autentimise telefon, autentimine e-posti või alternatiivse autentimise telefoni välja minu kasutajate nimel?**

 > **A:** Pole praegu, kuid me kaalute seda võimalust lubada.

 - **Q: kuidas registreerimise portaali teada, millised suvandid kuvamiseks minu kasutajad?**

 > **A:** Parooli lähtestamine registreerimise portaalis kuvatakse ainult soovitud suvandid, et olete lubanud oma kataloogi konfigureerimine menüü jaotises kasutaja parooli lähtestamine poliitika kasutajate jaoks. See tähendab, et kui te ei luba, näiteks turvaküsimused, siis kasutajad ei saa seda suvandit registreeruda.

 - **Q:, kui kasutaja loetakse registreeritud?**

 > **A:** Kasutaja loetakse registreeritud, kui ta on vähemalt N tekstilõigule autentimise teave määratletud, kus N on soovitud arv autentimist meetodite nõutav [Azure haldusportaali](https://manage.windowsazure.com)määratud. Lisateavet leiate teemast kohandamine kasutaja parooli lähtestamine poliitika.


## <a name="password-reset"></a>Parooli lähtestamine

 - **K: kui kaua võtab saada e-posti, SMS või telefonikõne ajal parooli lähtestamine**

 > **A:** E-posti, SMS-sõnumite ja telefonikõnede peaks saabuvad jaotises 1 minutiga, tavaline sellisel juhul 5 – 20 minutit. Kui kuvatakse teade selle aja jooksul, kontrollige oma rämpspostikausta, mis number / e-posti ühenduse saamist on üks teie oodatav ja, et kataloogi autentimise andmed on õigesti vormindatud. Lisateavet vormingu telefoninumbrid ja e-posti aadressid jaoks parooli lähtestamist vt saate teada, kuidas andmeid kasutab parooli lähtestamine.

 - **Q: milliseid keeli toetab parooli lähtestamist?**

 > **A:** Parooli lähtestamise UI, SMS-sõnumite ja kõnede on lokaliseeritud sama 40 keeles, mis on toetatud Teenusekomplektis Office 365. Need on: Araabia, Bulgaaria, Hiina (lihtsustatud), Hiina (traditsiooniline), horvaadi, Tšehhi, Taani, hollandi, inglise, Eesti, Soome, prantsuse, saksa, Kreeka, heebrea, Hindi, Ungari, Indoneesia, Itaalia, Jaapani, kasahhi, Korea, Läti, Leedu, malai (Malaisia), Norra (Bokmål), Poola, Portugali (Brasiilia), Portugali (Portugal), Rumeenia, vene, Serbia (ladina tähestik), slovaki, sloveeni, Hispaania, Rootsi, Tai, Türgi, Ukraina, ja Vietnami.

 - **Q: milliseid osi parooli lähtestamine kasutuskogemuse saamiseks kandvate määramisel ettevõtte sümboolika minu kataloog kasutaja konfigureerimine menüü?**

 > **A:** Parooli lähtestamine portaalis kuvatakse teie ettevõtte logo ja võimaldab teil konfigureerida kontakti ka kohandatud e-posti või URL-i osutamiseks teie administraator link. E-posti, mida saab saata parooli lähtestamise hõlmab ettevõtte logo, värvid (sel juhul punane), nime, e-posti ja kohandatud nimi. Vaadake alltoodud firmastiilis elementidega näide. Lisateabe saamiseks lugege määramine parooli lähtestamine ilme ja olemus.

  ![][001]

 - **Q: Kuidas ma harige kohta, kust minu kasutajad saavad paroole lähtestada?**

 > **A:** Saate saata oma kasutajate https://passwordreset.microsoftonline.com otse või juhendada neid klõpsake soovitud ei pääse juurde teie konto lingile leida mis tahes kooli või töö ID sisselogimiskuva. Saate vabalt avaldada neid linke (või URL-i ümbersuunamised neile loomiseks) mis tahes kohas, mis on kergesti juurdepääsetavad kasutajatele.

 - **Q: kas selle lehe mobiilsideseadme kaudu kasutada?**

 > **A:** Jah, selle lehe töötab mobiilsideseadmetes.

 - **Q: Kas te toetada avamise kohaliku active directory kontod, kui kasutajad saavad paroole lähtestada?**

 > **A:** Jah, kui kasutaja lähtestab oma parool ja parooli tagasikirjutusega juurutatud kõik versioonid Azure'i AD-ühenduse või Azure AD sünkroonimine 1.0.0485.0222 versiooni või uuemat versiooni, siis kasutajakonto saab automaatselt lukustamata kui kasutaja lähtestab oma parooli.

 - **Q: kuidas integreerida parooli lähtestamise otse oma kasutaja töölaua sisselogimine?**

 > **A:** See ei ole võimalik täna. Juhul, kui täiesti vaja seda võimalust ja kas olete Azure AD Premiumi klient, saate installida Microsofti Identity Manager täiendava tasuta ja juurutamine kohapealse parooli lähtestamine lahenduse leida selles, et selle nõude lahendada.

 - **Q: kas määrata eri turvaküsimused eri asukohtades?**

 > **A:** Ei, see ei ole võimalik täna, kuid me kaalute.

 - **Q: kuidas palju küsimusi me saate konfigureerida turvaküsimused autentimise suvandit?**

 > **A:** Saate konfigureerida kuni 20 kohandatud turvaküsimused [Azure'i haldusportaal](https://manage.windowsazure.com).

 - **Q: kuidas pikk võib olla turvaküsimused?**

 > **A:** Turvaküsimused võib 3 – 200 märki.

 - **Q: kuidas pikk võib olla turvalisuse küsimustele vastused?**

 > **A:** Vastused võib olla 3 kuni 40 tähemärki.

 - **Q: on tagasi lükatud turvaküsimused dubleeritud vastused?**

 > **A:** Jah, me Hülga dubleeritud turvalisuse küsimustele vastused.

 - **Q: mai kasutaja registreerida mitu sama turvalisus küsimus?**

 > **A:** Ei, kui kasutaja registreerib kindlale küsimusele, ta võib Registreeri küsimuse teist korda.

 - **Q: Kas on võimalik on piirväärtuse turvaküsimused registreerimise ja lähtestada?**

 > **A:** Jah, saate määrata ühe limiit registreerimist ja teine lähtestamine. 3-5 turvaküsimused võidakse nõuda registreerimist ja 3 – 5 võib olla vaja lähtestamine.

 - **Q: kui kasutaja on registreeritud rohkem kui maksimumarv lähtestamiseks nõutav küsimusi, kuidas turvaküsimused valitakse lähtestamisel?**

 > **A:** N turvaküsimused on valitud juhusliku küsimused kasutaja on registreeritud, kus N on nõutav parooli lähtestamiseks küsimused väikseima arvu koguarvust. Näiteks kui kasutajal on 5 turvaküsimused registreeritud, kuid ainult 3 on nõutav lähtestada, 3 need 5 valitud juhusliku ID-ga ja kasutajale lähtestamine ajal esitada. Kui kasutaja saab vastused küsimustele valesti, valimise käigus ilmneb uuesti küsimus haamriga vältimiseks.

 - **Q: kas saate takistada kasutajate proovite lühikese aja jooksul mitu korda lähtestada parooli?**

 > **A:** Jah, on mitu parooli lähtestamist sisseehitatud funktsioonid. Kasutajate ainult proovida 5 parooli lähtestamine proovib ühe tunni jooksul enne lukustatavad 24 tundi. Kasutajate ainult proovida valideerimiseks telefoninumbri 5 korda ühe tunni jooksul enne lukustatavad 24 tundi. Kasutajate proovida ainult ühe autentimise meetodit 5 korda ühe tunni jooksul enne lukustatavad 24 tundi.

 - **K: kaua kehtivad e-posti ja SMS ühekordse parooli?**

 > **A:** Seansi eluiga parooli lähtestamiseks on 105 minutit. See tähendab, et algusest parooli lähtestamiseks toiming, kasutajal on 105 minutit oma parooli lähtestada. E-posti ja SMS ühekordse parooli ei sobi pärast aegumist selle aja jooksul.


## <a name="password-management-reports"></a>Parooli haldus aruanded

 - **Q: kuidas palju aega kulub andmete kuvamiseks parooli halduse aruannete?**

 > **A:** Andmete peaks kuvatama parooli halduse aruannete 5 – 10 minuti jooksul. Seda mõnel juhul võib kuluda kuni tund kuvada.

 - **Q: Kuidas saab ka parooliga halduse aruannete filtreerimiseks?**

 > **A:** Saate filtreerida parooli haldus aruanded, klõpsates väike suurendusklaasi äärmiselt paremal veerusiltide aruande ülaossa (vt pilti). Kui soovite teha rikkalikumat filtreerimine, saate alla laadida excel ja luua PivotTable-liigendtabeli aruanne.

  ![][002]

 - **Q: mis on maksimaalne arv sündmused on talletatud aruannetes parooli haldus?**

 > **A:** Kuni 1000 parooli lähtestamine või parooli lähtestamine registreerimise sündmused on talletatud parooli haldus aruanded.  Töötame selle number veel sündmusi laiendamiseks.

 - **Q: kaugele tagasi parooli haldamine aruannete minna?**

 > **A:** Parooli haldamine aruannete kuvamine toimingute viimase 30 päeva jooksul. Praegu uurime kuidas teha pikema perioodi. Nüüd, kui soovite arhiivida andmed, saate aruannete perioodiliselt alla laadida ja salvestada eraldi asukohta.

 - **Q: kas saate parooli halduse aruannetes kuvatavate ridade arvu ülempiir?**

 > **A:** Jah, kuni 1000 ridade võidakse kuvada kas parooli haldus aruanded, kas nad kuvatakse UI või laaditakse alla. Praegu uurime selle piirmäära suurendamiseks tehke järgmist.

 - **Q: kas API parooli lähtestamine või registreerimise andmeid juurde pääseda?**

 > **A:** Jah, lugege saate teada, kuidas pääsete parooli lähtestamist, aruandlus andmevoos järgmised dokumendid.  [Saate teada, kuidas pääseda juurde parooli lähtestamiseks aruandlusteenuste sündmuste programmiliselt](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Parooli tagasikirjutusega
 - **Q: kuidas parooli tagasikirjutusega töötab taustal?**

 > **A:** Vaadake, [Kuidas parooli tagasikirjutusega töötab](active-directory-passwords-learn-more.md#how-password-writeback-works) üksikasjalik kirjeldus, mis juhtub, kui lubate parooli tagasikirjutusega, samuti kuidas andmevahetus süsteemi kaudu uuesti sisse oma kohapealse keskkonna jaoks. Vaadake, kuidas parooli tagasikirjutusega [parooli tagasikirjutusega turvalisus mudeli](active-directory-passwords-learn-more.md#password-writeback-security-model) töötab teavet tagamine parooli tagasikirjutusega on väga turvaline teenus.

 - **Q: kuidas kaua parooli tagasikirjutusega aega töötada?  Kas sünkroonimise viivituse nagu parooli räsi sync?**

 > **A:** Parooli tagasikirjutusega on kiire. See on sünkroonse kohaletoimetamisel, mis töötab põhjalikult teisiti kui parooli räsi sünkroonimist. Parooli tagasikirjutusega võimaldab kasutajatel reaalajas tagasisidet õnnestumise kohta oma parooli lähtestamine või muutmine toiming. Eduka tagasikirjutusega parooli jaoks keskmiselt on 500-ms.

 - **Q: millised kontotüüpide parooli tagasikirjutusega jaoks toimib?**

 > **A:** Parooli tagasikirjutusega toimib Mikroneesia ja parooli räsi sünkroonimise 'd kasutajad.

 - **Q: kas parooli tagasikirjutusega oma domeeni parooli poliitikaid rakendada?**

 > **A:** Jah, jõustab tagasikirjutusega parool parool vanus, ajalugu, keerukus, filtrite ja muud piirangud, mida te võib-olla panete paroolide kohas kohaliku domeeni.

 - **Q: kas parooli tagasikirjutusega turvaline?  Kuidas kindlasti ei saada häkkinud?**

 > **A:** Jah, parooli tagasikirjutusega on väga turvaline. Soovite rohkem teavet 4 kihid väärtpaberi rakendada parooli tagasikirjutusega teenus, lugege teemat Kuidas parooli tagasikirjutusega töötab [parooli tagasikirjutusega turvalisus mudeli](active-directory-passwords-learn-more.md#password-writeback-security-model) .




## <a name="links-to-password-reset-documentation"></a>Linkide parooli lähtestamiseks dokumentatsioon
Allpool on lingid kõik lehed Azure AD parooli lähtestamist dokumentatsiooni:

* **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).
* [**Kuidas see toimib**](active-directory-passwords-how-it-works.md) - teave kuus erinevate osade teenuste, mida iga ei
* [**Alustamine**](active-directory-passwords-getting-started.md) – saate teada, kuidas võimaldavad kasutajatel lähtestamine ja pilveteenuse või kohapealse parooli muutmine
* [**Kohanda**](active-directory-passwords-customize.md) – siit saate teada, kuidas kohandada ilme ja olemus ja käitumine teenuse teie asutuse vajadustest
* [**Head tavad**](active-directory-passwords-best-practices.md) – saate teada, kuidas kiiresti juurutada ja paroolide ettevõtte tõhus haldamine
* [**Saada ülevaadet**](active-directory-passwords-get-insights.md) - Lisateavet meie integreeritud aruandlusvõimalused
* [**Tõrkeotsing**](active-directory-passwords-troubleshoot.md) – saate teada, kuidas kiiresti teenuse probleemide tõrkeotsing
* [**Lisateavet leiate teemast**](active-directory-passwords-learn-more.md) -valige suure tehnilise üksikasjadesse teenuse tööpõhimõtted


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
