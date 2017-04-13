<properties
    pageTitle="Azure AD ja rakenduste: suunavad arendajate | Microsoft Azure'i"
    description="Kirjutada IT Pro, sellest artiklist leiate juhised rakenduste Azure Active Directory integreerimine."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD ja rakenduste: ärivaldkonna rakenduste arendamise

Selles juhendis antakse ülevaade line business (LoB) rakenduste jaoks Azure Active Directory (AD). Sihtrühma on Active Directory/Office 365 globaalne administraatoritele.

## <a name="overview"></a>Ülevaade

Azure AD integreeritud rakenduste loomine annab kasutajad oma ettevõtte ühekordse sisselogimise teenusekomplektiga Office 365. Võttes rakenduse Azure AD võimaldab valida rakenduse poliitika autentimist. Lisateavet tingimuslik juurdepääsu ja kuidas kaitsta rakenduste mitmikautentimise (MFA) leiate teemast [konfigureerimine Accessi reeglid](active-directory-conditional-access-azuread-connected-apps.md).

Rakenduse kasutamiseks Azure Active Directory registreerida. Rakenduse registreerimisel tähendab, et teie arendajad saate kasutada Azure AD kasutajate autentimiseks ja kasutajale ressursside, näiteks e-posti, kalendri ja dokumentide juurdepääsu taotlemine.

Liikme kataloogi (mitte külalistele) saate registreerida rakenduse tuntud ka kui *objekti teenuserakenduse loomine*.

Rakenduse registreerimisel võimaldab igal kasutajal teha järgmist:

- Identiteedi saamiseks nende rakendus, mis tuvastab Azure AD
- Ühe või mitme saladused/tutvustatakse, mida rakenduse abil saate ise AD autentida hankimine
- Kaubamärgi rakenduse Azure'i portaalis kohandatud nimi, logo jne.
- Rakendada Azure AD autoriseerimine funktsioonid rakenduste kohta, sh:
  - Rollipõhine juurdepääsu reguleerimine (RBAC)
  - Azure Active Directory OAuthi autoriseerimine server (secure API esitatud rakendus)

- Funktsiooni rakendamiseks oodatud viisil, sh deklareerida vajalikke õigusi:-rakenduse õigusi (ainult globaalse administraatorid). Näide: rolli kuuluvus teise Azure AD Rakenduse või rolli liikmelisuse suhtes Azure'i ressurss, ressursirühm,- või tellimuse - delegeeritud õigused (kasutaja). Näide: Azure'i AD, Logi sisse ja lugege profiil


> [AZURE.NOTE]Vaikimisi saate rakenduse registreerimine mistahes liiget. Saate teada, kuidas teatud liikmetele rakenduste registreerusite õiguste piiramine, lugege teemat [Kuidas rakenduste lisatakse Azure AD](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Siit saate globaalne administraator, tuleb selleks, et arendajad esitada taotlus valmis tootmist.

- Konfigureerimine juurdepääsureeglid (Accessi poliitika/MFA)
- Nõua kasutajate määramine ja määrata kasutajatele rakenduse konfigureerimiseks
- Vaikimisi nõusolekut kasutusvõimalused tõkestamine

## <a name="configure-access-rules"></a>Juurdepääsureeglid konfigureerimine

Rakenduse kohta juurdepääsureeglid SaaS rakenduste konfigureerimine. Näiteks saate nõuda MFA või lubada kasutajate juurdepääs ainult usaldusväärsete võrkudes. See üksikasjad on saadaval dokumendis [seadistamine juurdepääsureeglid](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Nõua kasutajate määramine ja määrata kasutajatele rakenduse konfigureerimiseks

Vaikimisi kasutajad pääsevad rakendused ilma. Aga kui rakenduse seab rollid või kui soovite kuvada kasutaja juurdepääs paneeli rakendus, tuleks nõuda kasutajate määramine.

[Nõudva kasutajate määramine](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Kui olete mõne Azure AD Premium või ettevõtte mobiilsus komplekti (EMS) abonendi, soovitame rühmade kasutamine. Rühmade määramine rakendus võimaldab volitatud esindaja rühma omanikule alalise juurdepääsu haldamine. Saate luua rühma või paluge vastutab pool oma asutuse loomine funktsiooni abil oma rühma haldamine rühma.

[Rakenduse kasutajate määramine](active-directory-applications-guiding-developers-assigning-users.md)  
[Rakenduse rühmade määramine](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Kasutaja nõusolekuta tõkestamine

Iga kasutaja läheb vaikimisi nõusolekut kogemus logida kaudu. Nõusolekut versioon, mis palub kasutajatele anda õigused rakendus võib olla hämmeldust kasutajatele, kes ei tunne selliseid otsuseid.

Rakenduste, mida peate usaldusväärseks, saate lihtsustada kasutusvõimalused, nõustute rakendusse oma organisatsiooni nimel.

Kasutaja nõusolekuta ja nõusoleku teenuses Azure kohta leiate lisateavet teemast [Azure Active Directory rakenduste integreerimine](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Seotud artiklid

- [Kohapealse rakenduste Azure AD Rakenduse puhverserveri turvalist remote juurdepääsu lubamine](active-directory-application-proxy-get-started.md)
- [Azure'i juurdepääsu SaaS Apps Preview](active-directory-conditional-access-azuread-connected-apps.md)
- [Rakenduste Azure AD juurdepääsu haldamine](active-directory-managing-access-to-apps.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
