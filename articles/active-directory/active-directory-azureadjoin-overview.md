<properties
    pageTitle="Ulatub pilve võimalusi Windows 10 seadmete kaudu Azure Active Directory liitumine | Microsoft Azure'i"
    description="Leiate üksikasjaliku ülevaate, kuidas Windows 10 seadmetes saab kasutada Azure AD ühendamine tuleb saada registreeritud Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine

## <a name="what-is-azure-active-directory-join"></a>Mis on Azure Active Directory liituda?
Azure Active Directory liitumine (Azure'i AD liituda) on funktsioone, mida registreerib ettevõttel omanik seadme Azure Active Directory tsentraliseeritud seadme lubamiseks. See võimaldab kasutajatele, nt töötajate ja õppurite ühendada pilveteenustega enterprise Azure Active Directory kaudu. See võimaldab lihtsustatud Windows kasutuselevõttu ja juurdepääsu ettevõtte rakendused ja ressursside Windowsi mis tahes seadme kaudu, nii ettevõtte omanik ja isiklikult kuuluv (BYOD).

Azure'i AD-ühendus on mõeldud ettevõtete on pilveteenuses – esimene/vaid--tavaliselt väikestele ja keskmise suurusega ettevõtted, kes on kohapealse Windows Server Active Directory infrastruktuur. Mida ütles: Azure'i AD-ühendus saab ja kasutab ka suurettevõtete seadmetes, mis ei suuda seda traditsiooniline domeeni Liitu (mobiilsideseadmete jaoks, näiteks) või kasutajatele, kellel on vaja eelkõige juurdepääs Office 365 või muud Azure AD SaaS rakendused.

Kuigi traditsiooniline domeeni Liitu pakub endiselt parim asutusesisese kogemus seadmetes, mis võivad domeeni liitumine, Azure'i AD-ühendus sobib seadmed, mida ei saa domeeni Liitu. Azure'i AD-ühendus sobib haldamise pilveteenuste kasutajad. Seda tehes abil mobiilsideseadme halduse võimaluste asemel traditsiooniline domeeni kirjehalduse tööriistadega nagu rühmapoliitika ja System Center Configuration Manager (SCCM) abil.

![Ettevõtte seadmed ja isiklike seadmete kohapealse Active Directory ja Azure AD ülevaade](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Miks peaks suurettevõtete vastu Azure AD liituda?

* **Ettevõtted, mis on pilves suures osas**: kui teil on teisaldatud või liiguvad mudeli, kus on vähendada oma kohapealse andmed väiksemates vähem ruumi ja soovite kasutada rohkem pilves, Azure AD liitumine võib kasu teile. Võib-olla olete loonud Azure AD kontod käsitsi või kaudu oma kohapealse Active Directory sünkroonimine. Mõlemal juhul olete konto Azure AD ja abil saate sisse logida Windows 10. Kasutajate liituda Azure AD kummagi out box experience (OOBE) või menüü Sätted kaudu oma arvutisse. Pärast liitumist, saavad kasutajad ühekordse sisselogimise (SSO) juurdepääsu cloud ressursse nagu Office 365, brauseri või Office'i rakenduste.

* **Haridusasutustes**: üks stsenaariumid, Soovime kuulda sageli on, et haridusasutustes on kahte tüüpi toiminguid kasutaja: õppejõududele ja õppuritele. Õppejõud arvestatakse organisatsiooni, pikaajalise liikmed, seega kohapealsete kontode loomine nende jaoks on soovitatav. Kuid õpilaste lühemaajalist organisatsiooni liikmed ja seega saate hallata Azure AD. See tähendab, et pilves talletatud kohapealse asemel saate lükata directory skaala. Ka see tähendab, et õpilased saate Windows Azure AD oma kontoga sisse logida ja juurdepääs Office 365 ressursse, brauseri või Office'i rakenduste.

* **Jaemüügi ettevõtetele**: me olen kuulnud klientide teine stsenaarium on soovi hooajaliste töötajate hõlpsam hallata.  Kontode pikaajalise, täiskohaga töötajate jaoks tavaliselt luuakse uuesti, nagu kohapealse arvutites domeeni ühendatud kontod. Kuid hooajaliste töötajate kuuluvad lühemaajalist organisatsiooniskeemide, seega on soovitatav hallata neid kus Kasutajalitsentsid saab kergemini liigutada. Nende kasutajakontode loomine pilveteenuses teenusekomplektiga Office 365 litsentside võimaldab kasutajatel saada kasu Windowsi ja Office'i rakenduste Azure AD kontoga sisselogimine. Samal ajal saate säilitada rohkem paindlikkust koos oma litsentside pärast lahkumist.
* **Muude ettevõtete**: isegi juhul, kui haldate oma kohapealse Active Directory kasutajaid, saate võivad saada võttes Azure'i AD-liitunud kasutajatele. On põhjus Azure AD pakub lihtsustatud liitumist kogemus tõhusa mobiilsideseadmete halduse automaatse mobiilsideseadme halduse registreerimine ja ühekordse sisselogimise võimalus Azure AD ja kohapealse ressursid.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Millised funktsioonid ei paku Azure AD liituda?
Azure'i AD liituda, saate järgmist:

* **Omas ettevalmistamise ettevõtte omanik seadmed**: Windows 10, kasutajad saavad konfigureerida täiesti uus, termokahaneva seadme out box kogemus, ilma IT kaasamine.


* **Tänapäevane vormi tegureid tugi**: Azure'i AD-ühendus töötab seadmetes, mis pole traditsiooniline domeeni liitumise võimalusi.  


* **Olemasoleva ettevõtte kontod tugi**: kasutajad pole enam vaja luua ja hallata on parima kasutuskogemuse saamiseks seadmetes ettevõtte välja nagu Windows 8 isikliku Microsofti kontoga. Saab kasutada oma töö olemasolevaid kontosid Azure AD asemel. Palju ettevõtted, tähendab see põhimõtteliselt, et kasutajad saavad häälestada ja Windowsi sama identimisteavet, mida nad kasutavad juurdepääs Office 365 sisse logida.


* **Automaatse mobiilsideseadme halduse registreerumise**: seadmete saab automaatselt teenuses Azure AD ühendatuna mobiilsideseadmete haldus. See protsess töötab Microsoft Intune'i ja partneri mobiilsideseadme halduse lahendusi. Kui mobiilsideseadmete halduse on lõpetanud Intune, IT-administraatorid kuvari/haldamiseks Azure'i AD-ühendatud seadmete domeeni ühendatud seadmete SCCM halduskonsoolis kõrval.


* **Ühekordse sisselogimise ettevõtte ressursse**: kasutajad nautida ühekordse sisselogimise Windowsi töölaua kaudu rakendused ja ressursside pilves, nt Office 365 ja tuhandeliste ärirakenduste, mis sõltuvad Azure AD autentimise [Azure'i AD-ühenduse](active-directory-azureadjoin-deployment-aadjoindirect.md)kaudu. Kui seade on ettevõtte võrgus ja igalt poolt, kui need ressursid on esitatud [Azure AD Rakenduse puhverserveri](https://msdn.microsoft.com/library/azure/Dn768219.aspx)kaudu nautida Corporate seadmetes Azure AD liidetud SSO ka asutusesisese ressursse.


* **OS olekus rändlusteenuse**: hõlbustussätteid, veebisaidid, Wi-Fi paroole ja muud sätted on sünkroonitud seadmetes ettevõtte omanik nõudmata isikliku Microsofti kontoga.


* **Ettevõttele kasutusvalmis Windowsi poe**: poe toetab rakenduse omandamise ja Azure AD kontode litsentsid. Ettevõtted saavad hulgilitsentsi rakenduste ja oma ettevõtte kasutajatele kättesaadavaks teha.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Kuidas töötavad koos Azure AD liitumine erinevatest seadmetest?

| Ettevõtte seadme (kohapealse domeeni liidetud)                                                                                                                                                                                         | Ettevõtte seadme (liidetud pilveteenusesse)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Isiklik seade                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Kasutajad saavad Windowsi sisse logida töö mandaadiga (nagu nad täna).                                                                                                                                                                        | Kasutajad saavad sisse logida Windows Azure AD hallatud töö mandaadiga. See on oluline kolmes ettevõtte seadmete jaoks: 1) pole organisatsiooni Active Directory kohapealne (nt small business). 2) organisatsiooni ei kõigi kasutajakontode loomine Active Directory (nt õppurid, konsultandid või hooajaliste töötajate kontod ei looda Active Directory). 3) ettevõttel on ettevõtte seadmed, mida ei saa liita (kohapealse) domeeni, näiteks mobiiltelefoni või tahvelarvuti töötab Mobile SKU (näiteks suunatakse factory/jaemüügi floor teisene seade). Azure'i AD-ühendus toetab ettevõtte seadmete hallatavate- ja ühendatud ettevõtete liitumise. | Windowsi sisselogimiseks kasutajate koos oma isikliku Microsofti konto identimisteave (muutmata).                                                |
| Kasutajatel on juurdepääs rändluse sätted ja ettevõtte Windowsi poe. Nende teenuste töö kontod töötamine ja ei nõua isikliku Microsofti kontoga. Selleks on vaja oma kohapealse Active Directory ühenduse Azure AD ettevõtted.                                        | Kasutaja saab teha iseteenindusliku häälestus. Võivad nad läbida esmakasutuse (FRX) oma töö konto kaudu alternatiivina on see säte seadmete, kuigi mõlema meetodi on toetatud.                                                                                                                                                                                                                                                                                                                                                                             | Kasutajad saavad hõlpsalt lisada töö konto, mille hallatakse Active Directory või Azure AD.                                                      |
| Kasutajatel on võimalus SSO töölaualt töötada rakenduste, veebisaitide ja ressursside--koos ressursid kohapealse ja pilveteenuse rakendusi, mis kasutavad autentimiseks Azure AD.                                                                                                            | Seadmed on automaatselt registreeritud ettevõtte nimistust (Azure AD) ja automaatselt registreerunud mobiilsideseadmete haldus. (Azure AD Premium funktsioon).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Kasutajatel on võimalus SSO rakendustes ja veebisaidid/ressursid töö selle kontoga.                                              |
| Kasutajad saavad lisada oma isikliku Microsofti kontode juurde pääseda oma isiklikke pilte ja faile ilma ettevõtte andmeid mõjutavad. (Rändlusteenuse sätted jätkavad tööd oma töö kontode.) Microsofti konto võimaldab SSO-d ja draivid enam rändlusteenuse sätted.  | Kasutaja saab teha Iseteeninduslik parooli lähtestamine (SSPR) winlogon, klõpsake tähendab, et nad saavad unustatud parooli lähtestamine. (Azure AD Premium funktsioon).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Kasutajate juurdepääs ettevõtte Windowsi poe nii, et nad saaksid hankida ja ärivaldkonna rakendusi kasutada oma isikliku seadmes. |                                                               |


## <a name="additional-information"></a>Lisateave
* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Parooli Microsoft Passporti kaudu ilma autentimine](active-directory-azureadjoin-passport.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
