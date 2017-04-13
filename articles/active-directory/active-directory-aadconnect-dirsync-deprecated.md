<properties
    pageTitle="Versioonitäienduse DirSync ja Azure AD sünkroonimine | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse versiooniks DirSync ja Azure AD sünkroonimine Azure'i AD-ühenduse."
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
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Üleminek Windows Azure Active Directory sünkroonimine ("DirSync") ja Azure Active Directory sünkroonimine ("Azure AD sünkroonimine")
Azure'i AD-ühendus on parim viis Azure AD kohapealse kataloogi suhtlemiseks ja Office 365. See on hea aeg versioonile üleminekuks Azure'i AD-ühenduse Windows Azure'i Active Directory sünkroonimine (DirSync) või Azure AD sünkroonimine nagu nende tööriistade on nüüd aegunud ja jõuavad tugiteenused veebisaidil 13 aprillis 2017 lõppu.

Kahe identiteedi sünkroonimise tööriistu, mis on aegunud olid pakutud ühe mets klientide (DirSync) ja mitme mets ja muude täpsemate kliendid (Azure AD sünkroonimine). Nende vanemate tööriistade on asendatud ühe lahenduse, mis on saadaval kõigi stsenaariumid: Azure'i AD-ühenduse. See pakub uusi funktsioone, funktsioonide täiustusi ja uus stsenaariumid tugi. Saama saate sünkroonida oma kohapealse identiteedi Azure AD ja Office 365, soovitame uuendada Azure'i AD-ühenduse.

Viimase versiooni DirSync ilmus juuli 2014 ja Azure AD sünkroonimine viimast versiooni ilmus mai 2015.

## <a name="what-is-azure-ad-connect"></a>Mis on Azure AD-ühenduse
Azure'i AD-ühendus on DirSync ja Azure AD sünkroonimine. Ühendab kõik stsenaariumid need kaks toetatud. Lugege lisateavet selle integreerimine [Azure Active Directory oma kohapealse identiteedi](active-directory-aadconnect.md)kohta.

## <a name="deprecation-schedule"></a>Taunimine ajakava

Kuupäev | Kommentaari
 --- | ---
13 aprill 2016 | Windows Azure Active Directory sünkroonimine ("DirSync") ja Microsoft Azure Active Directory sünkroonimine ("Azure AD sünkroonimine") on teada, nagu on aegunud.
13 aprill 2017 | Tugi lõpeb. Klientide enam saab avada tugi juhul ilma Täiendamine versiooniks Azure'i AD-ühenduse esmalt.

## <a name="how-to-transition-to-azure-ad-connect"></a>Azure'i AD-ühenduse ülemineku kohta
Kui teil on kaks võimalust, saate üle minna DirSync: kohapealse versioonitäienduse ja paralleelsete juurutamise. Kohapealse täienduse on soovitatav enamik kliendid ja kui teil on tehtud operatsioonisüsteem ja väiksem kui 50 000 objektid. Muudel juhtudel on soovitatav teha paralleelse juurutamine, kus teie DirSync konfiguratsiooni teisaldatakse uue serveriga Azure'i AD-ühenduse.

Kui kasutate Azure'i AD-täienduse on soovitatav. Kui soovite, on võimalik, et installida uus Azure'i AD-ühenduse server paralleelselt ja teha kiik migreerimise oma Azure AD sünkroonimine server Azure'i AD-ühenduse.

Lahendus | Stsenaarium
----- | -----
[DirSync täiendamine](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Kui teil on juba töötab olemasoleva DirSync serveri.</li>
[Versioonitäienduse: Azure'i AD sünkroonimine](active-directory-aadconnect-upgrade-previous-version.md)| <li>Kui teil on liikudes Azure AD sünkroonimine.</li>

Kui soovite teada, kuidas teha kohapealse täienduse DirSync: Azure'i AD-ühenduse, siis lugege selle kanali 9 video:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>KKK
**K: sain meiliteatise Azure'i meeskonnatöö ja/või Office 365 sõnumikeskus sõnumit, kuid ma kasutan ühenduse loomine.**  
Teate saadeti ka klientidele, kes kasutavad Koosta numbriga 1.0 Azure'i AD-ühenduse. \*.0 (abil eel-1.1 väljaanne). Microsoft soovitab kliendid püsida kursis väljalasete Azure'i AD-ühenduse. 1.1 koos [automaatse versioonitäienduse](active-directory-aadconnect-feature-automatic-upgrade.md) funktsioon teeb väga lihtne on alati uusimat versiooni Azure'i AD-ühenduse installitud.

**Q: kas DirSync/Azure AD sünkroonimine peatada kallal 13 aprill 2017?**  
Ei. Kui need pole enam suhelda Azure AD kuupäev avaldatakse hiljem. Mida saab selle teabe leiate selle teema, kui need on saadaval.

**Q: milliseid DirSync versioone täiendada, kaudu?**  
See on toetatud praegu kasutusel DirSync vabanemise uuendada.

**Q: Kuidas on lood Azure AD konnektor FIM/MIM jaoks?**  
Azure'i AD konnektor FIM/MIM jaoks **ei** ole teada, nagu on aegunud. See on **funktsioon Külmuta**; lisatakse pole uusi funktsioone ja ta saab pole veaparandused. Microsoft soovitab klientidele, kes kasutavad seda teisaldada selle Azure'i AD-ühenduse kavandada. See on tungivalt soovitatav ei käivitu mis tahes uue juurutuste seda kasutama. Selle konnektori avaldatakse aegunud tulevikus.

## <a name="additional-resources"></a>Lisaressursid

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
