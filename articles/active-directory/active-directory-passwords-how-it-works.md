<properties
    pageTitle="Kuidas see toimib: Azure'i AD parooli haldus | Microsoft Azure'i"
    description="Lisateavet erinevate osade Azure AD parooli haldus, kus kasutajate registreerimine, lähtestamine ja muuta oma parooli, sh ja kui administraatorid konfigureerida, aruandluseks ja lubada kohapealse Active Directory paroolide haldamine."
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

# <a name="how-password-management-works"></a>Kuidas töötab parooli haldus

> [AZURE.IMPORTANT] **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).

Parooli haldus Azure Active Directory hulka kuuluvad mitme loogilise komponendid, mida on kirjeldatud allpool.  Klõpsake iga komponendi kohta lisateabe saamiseks linki.

- [**Parooli haldusportaali konfiguratsiooni**](#password-management-configuration-portal) – administraatorid saate määrata, kuidas paroolide hallatakse nende rentnikud navigeerides oma kataloogi konfigureerimine menüü [Azure'i haldusportaal](https://manage.windowsazure.com)eri aspekte.
- [**Kasutaja registreerimise portaali**](#user-registration-portal) – kasutajad saate ise registreerida parooli lähtestamise selle veebiportaali kaudu.
- [**Kasutaja parooli lähtestamine portaali**](#user-password-reset-portal) – kasutajad saavad lähtestada oma paroole, kasutades mitmeid erinevaid probleeme vastavalt kontrollitud administraatori parooli lähtestamine poliitika
- [**Kasutaja parooli muutmine portaali**](#user-password-change-portal) – kasutajad saate igal ajal oma parooli muuta sisestada oma vana parool ja valige uus parool, kasutades portaal
- [**Parooli haldamine aruannete**](#password-management-reports) – administraatorid saate vaadata ja analüüsida parooli lähtestamine ja registreerimise tegevuse oma rentniku navigeerides oma kataloogi "Aruanded" menüü [Azure'i haldusportaal](https://manage.windowsazure.com) "Tegevuse aruanded" jaotis
- [**Parooli tagasikirjutusega osa Azure'i AD-ühenduse**](#password-writeback-component-of-azure-ad-connect) – administraatorid soovi korral saate lubada parooli tagasikirjutusega funktsiooni installimise Azure'i AD-ühenduse lubamiseks haldamine ühendatud või parooli sünkroonitud kasutaja paroolid pilvest.

## <a name="password-management-configuration-portal"></a>Parooli haldusportaali konfigureerimine
Parooli teabehalduspoliitikate teatud kataloog directory's **konfigureerimine** vahekaardil **Kasutaja parooli lähtestamine poliitika** jaotisse liikumine [Azure haldusportaali](https://manage.windowsazure.com) abil saate konfigureerida.  Konfiguratsiooni lehelt saate määrata palju aspekte paroolid teie asutuse haldamise viisi sh:

- Lubamine ja keelamine parooli lähtestamise kõigi kasutajate kataloog
- Säte toimetulekuks arv (ühe või kahe) kasutaja tuleb läbida oma parooli lähtestamiseks
- Kindlat tüüpi soovite lubada kasutajate alltoodud loendist oma asutuse toimetulekuks seadmine
 - Mobiiltelefon (kinnituskood teksti või telefonikõne kaudu)
 - Töötelefoninumber (telefonikõne)
 - Alternatiivne meiliaadress (kinnituskood e-posti)
 - Turvaküsimused (teadmisi autentimise)
- Säte küsimuste arvust kasutaja peab registreerima, et kasutada turvalisuse küsimused autentimismeetodi määramine (ainult kuvatakse, kui turvaküsimused on lubatud)
- Küsimused arvu seadmine kasutaja esitama lähtestamisel kasutada turvalisuse küsimused autentimismeetodi määramine (ainult kuvatakse, kui turvaküsimused on lubatud)
- Kasutamise eel-konservid, lokaliseeritud, turvaküsimused, et kasutaja võib valida kasutada registreerimisel parooli lähtestamine (nähtav ainult kui turvaküsimused on lubatud)
- Kohandatud turvalisus määratlemise küsimused, et kasutaja võib valida kasutada registreerimisel parooli lähtestamine (nähtav ainult kui turvaküsimused on lubatud)
- Kasutaja parooli lähtestamise, kui nad lähevad rakenduse Access paani [http://myapps.microsoft.com](http://myapps.microsoft.com)registreerida nõudmine.
- Kinnitamiseks uuesti oma varem registreeritud andmete konfigureeritav arvu päevade järel nõuab kasutajate arvu möödumist (nähtav ainult kui jõustatud registreerimine on lubatud)
- Kohandatud Kasutajatugi e-posti või URL-i, mis kuvatakse kasutajatele juhuks, kui neil ilmnevad probleemid paroolide lähtestamine
- Lubamine või keelamine parooli tagasikirjutusega võimalus (kui parooli tagasikirjutusega juurutatud AAD ühenduse abil)
- Parooli tagasikirjutusega agent seisundi vaatamine (kui parooli tagasikirjutusega juurutatud AAD ühenduse abil)
- Kui oma parool on lähtestatud (leitud ** [Azure'i haldusportaal](https://manage.windowsazure.com)teatised** ), mis võimaldab kasutajatele meiliteatised
- Lähtestamisel teiste administraatorite paroolide (leitud [Azure'i haldusportaal](https://manage.windowsazure.com) jaotises **teatised** , mis võimaldab meiliteatised administraatoritele
- Portaali sümboolika kasutaja parooli lähtestamine ja parooli lähtestamise meilisõnumid teie ettevõtte logo ja nime rentniku sümboolika (leitud **Directory atribuudid** jaotises [Azure'i haldusportaal](https://manage.windowsazure.com) kohandamise funktsiooni abil

Ettevõttes parooli halduse konfigureerimise kohta leiate lisateavet teemast [Alustamine: Azure'i AD parooli halduse](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>Kasutaja registreerimise portaal
Enne, kui kasutajad saavad kasutada parooli lähtestamist, tuleb värskendada oma pilveteenuses kasutajakontode õige autentimine andmetega, et tagada, et saate läbida parooli lähtestamine toimetulekuks oma administraatori määratud sobiv arv.  Administraatorid ka saate määratleda autentimine seda teavet oma kasutaja nimel, kasutades Azure või Office web portaalide, DirSync / Azure'i AD-ühenduse või Windows PowerShelli.

Juhul, kui te eelistaksite registreerida oma andmete kasutajate, pakume ka veebilehele, kus kasutajad saavad minna, et selle teabe.  Selle lehe võimaldab kasutajal määrata autentimise teavet vastavalt parooli lähtestamiseks poliitikad, milles on lubatud oma asutuses.  Kui andmed on kinnitatud, salvestatakse see pilve kasutajakontot kasutatakse hiljem konto taastamine. Siin, mis registreerimise portaali välja näeb.

  ![][001]

Lisateabe saamiseks lugege teemat [Alustamine: Azure'i AD parooli haldus](active-directory-passwords-getting-started.md) ja [head tavad: Azure'i AD parooli haldus](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Kasutaja parooli lähtestamine portaal
Kui teil on lubatud Iseteeninduslik parooli lähtestada, häälestamine teie asutuse Iseteeninduslik parooli lähtestamine poliitika ja tagada, et teie kasutajatel on vastav kontaktandmed kataloogis, teie asutuse kasutajatele saab automaatselt kaudu mis tahes veebileht, mis kasutab töö või kooli kontoga sisselogimine (nt [portal.microsoftonline.com](https://portal.microsoftonline.com)) oma paroole lähtestada. Kasutajad näevad lehtedel, nagu need on **ei pääse oma kontole juurde?** link.

  ![][002]

Selle lingi klõpsamisel käivitatakse Iseteeninduslik parooli lähtestamine portaali.

  ![][003]

Lisateavet selle kohta, kuidas kasutajad saavad oma paroole lähtestada, lugege teemat [Alustamine: Azure'i AD parooli halduse](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Kasutaja parooli muutmine portaal
Kui kasutajad soovivad oma parooli muuta, saate need seda teha igal ajal parooli muutmine portaali kaudu.  Kasutajad pääsevad parooli muutmine portaali kaudu juurdepääsu paani profiililehel või Office 365 rakenduste kaudu "parooli muutmine" lingi klõpsamisel.  Juhul kui nende paroolide aegumise, kasutajate ka palutakse muuta neid sisselogimisel automaatselt.

  ![][004]

Mõlemal juhul, kui parool tagasikirjutusega on lubatud ja kasutajale on kas ühendatud või parooli sünkroonimise oleks need muudetud paroolid kirjutada tagasi oma kohapealse Active Directory. Mida parooli muutmine portaali näeb välja nagu siin.

  ![][005]

Kuidas kasutajad saavad muuta oma kohapealse Active Directory paroolide kohta leiate lisateavet teemast [Alustamine: Azure'i AD parooli halduse](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Parooli haldus aruanded
Liikumine **vahekaardile** ja jaotises **Logid** , kuvatakse kaks parooli haldus aruanded: **parooli lähtestamise tegevuste** ja **parooli lähtestamise registreerimise tegevuse**.  Nende kahe aruande saate avada vaate kasutajate registreerimine ja parooli lähtestamise ettevõtte abil. Siin on neid aruandeid näha [Azure'i haldusportaal](https://manage.windowsazure.com).

  ![][006]

Lisateabe saamiseks lugege teemat [saada ülevaateid: Azure'i AD parooli halduse aruannete](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Azure'i AD-ühenduse parooli tagasikirjutusega komponent
Kui teie asutuse kasutajate paroole pärit kohapealse keskkonna (kas liitmist või parooli sünkroonimise kaudu), saate installida Azure'i AD-ühenduse lubamiseks värskendamine nende paroolid otse pilvest uusim versioon.  See tähendab, et kui kasutajate unustate või soovite muuta oma parooli AD, nad saavad teha nii otse veebist.  Kust leida parooli tagasikirjutusega installiviisardis Azure'i AD-ühenduse siin.

  ![][007]

Azure'i AD-ühenduse kohta leiate lisateavet teemast [Alustamine: Azure'i AD-ühenduse](active-directory-aadconnect.md). Parooli tagasikirjutusega kohta leiate lisateavet teemast [Alustamine: Azure'i AD parooli halduse](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Linkide parooli lähtestamiseks dokumentatsioon
Allpool on lingid kõik lehed Azure AD parooli lähtestamist dokumentatsiooni:

* **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).
* [**Alustamine**](active-directory-passwords-getting-started.md) – saate teada, kuidas võimaldavad kasutajatel lähtestamine ja pilveteenuse või kohapealse parooli muutmine
* [**Kohanda**](active-directory-passwords-customize.md) – siit saate teada, kuidas kohandada ilme ja olemus ja käitumine teenuse teie asutuse vajadustest
* [**Head tavad**](active-directory-passwords-best-practices.md) – saate teada, kuidas kiiresti juurutada ja paroolide ettevõtte tõhus haldamine
* [**Saada ülevaadet**](active-directory-passwords-get-insights.md) - Lisateavet meie integreeritud aruandlusvõimalused
* [**FAQ**](active-directory-passwords-faq.md) – siin on vastused korduma kippuvatele küsimustele
* [**Tõrkeotsing**](active-directory-passwords-troubleshoot.md) – saate teada, kuidas kiiresti teenuse probleemide tõrkeotsing
* [**Lisateavet leiate teemast**](active-directory-passwords-learn-more.md) -valige suure tehnilise üksikasjadesse teenuse tööpõhimõtted



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
