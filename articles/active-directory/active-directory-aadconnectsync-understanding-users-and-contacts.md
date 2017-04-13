<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: mõistmine kasutajad ja kontaktid | Microsoft Azure'i"
    description="Selgitab kasutajatele ja kontaktide sünkroonimine Azure'i AD-ühenduse."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure'i AD-ühendus sünkroonimine: mõistmine kasutajad ja kontaktid

On mitu eri põhjust, miks on teil mitu Active Directory kogumit ja on mitme eri juurutamise topoloogiatest. Levinud mudelid sisaldavad teenusekomplekti konto ressursi juurutamist ja GAL sync'ed metsad pärast ühinemise ja omandamise. Kuid isegi juhul, kui on puhas mudeleid, hü mudelid on samuti levinud. Vaikimisi sünkroonimine Azure'i AD-ühenduse konfiguratsiooni ei vastuta mis tahes mudeli, kuid sõltuvalt sellest, kuidas kasutaja sobitamine valitud juhendist installimist, tuleb järgida erineva käitumise.

Selles teemas on läbida vaikimisi konfiguratsiooni käitumise teatud topoloogiatest. Meil on läbida konfiguratsiooni ja sünkroonimise reeglite redaktori saab vaadata konfiguratsiooni.

On paar üldised reeglid konfiguratsiooni eeldab.

- Sõltumata me importimine allika aktiivne kataloogide järjestuse seetõttu tuleb alati sama.
- Aktiivne konto aitab alati sisselogimise teave, sh **userPrincipalName** ja **sourceAnchor**.
- Keelatud kontot aitab userPrincipalName ja sourceAnchor, kui see on lingitud postkasti, kui seal on ei leidu aktiivse konto.
- Lingitud postkasti konto userPrincipalName ja sourceAnchor kunagi kasutada. Eeldatakse, et aktiivne konto hiljem leitakse.
- Kontakti objekti võib ette valmistatud Azure AD kontaktina või kasutajana. Te ei tea tõesti seni, kuni kõik andmeallika Active Directory kogumit olema töödeldud.

## <a name="contacts"></a>Kontaktid

Kontaktide tähistav erinevate mets kasutaja on levinud pärast ühinemise ja omandamise kui GALSync lahendus on ajutine kahe või enama Exchange'i metsad. Kontakti objekti alati liitumise konnektori ruumi metaversumisse abil atribuuti mail. Kui seal on juba soovitud kontakt või kasutaja objekti sama e-posti aadress, objektid on omavahel ühendatud. See on konfigureeritud reegli **sisse: AD-kontakti liitumine**. Olemas on ka reegli nimega **sisse: AD-kontakti levinud** koos mõne atribuudi voog metaversumi atribuut **sourceObjectType** koos konstandi **kontakti**. See reegel on liiga nõrk järjestuse seda, kui mõni objekt on ühendatud sama metaversumi objekti ja seejärel reegli **sisse: AD-kasutaja levinud** aitab selle atribuudi väärtuseks kasutaja. Selle reegliga see atribuut on väärtus kontakti kui ükski kasutaja on ühinenud ja kasutaja väärtuse leidmisel vähemalt üks kasutaja.

Azure AD objekti ettevalmistamine **välja AAD – pöörduge liituda** Väljamineva reegli loomine kontakti objekti, kui **Kontakt**on seatud metaversumi atribuut **sourceObjectType** . Kui see atribuut on seatud väärtusele **kasutaja**, reegel **välja AAD – kasutaja liitumine** loob kasutaja objekti asemel.
On võimalik, et objekti on esiletõstetud kontakti kasutajale kui mitme andmeallika aktiivne kataloogide imporditakse ja sünkroonida.

Näiteks GALSync topoloogia me otsib kontakti objektid kõigi jaoks teine mets kui me importida esimese mets. See on esiplaanil uue kontakti objektide konnektor AAD. Kui hiljem importimine ja sünkroonimine teise mets, me real kasutajad otsimine ja olemasoleva metaversumi objektide liituma. Seejärel AAD kontakti objekti kustutamine ja luua uus kasutaja objekti asemel.

Kui teil on mõne topoloogia kus kasutajate ja esindatud kontaktide hulka, veenduge, et valite kasutajate e-posti atribuudi installi juhendist vastavaks. Kui valite mõne muu suvandi, siis on järjestus sõltuvate konfigureerimisel. Kontakti objektid on alati e-posti atribuudi liituda, kuid kasutaja objektid kuvatakse ainult liitumine atribuuti mail kui on valitud suvand installi juhend. Teil võib siis lõpuks kahte erinevat objekti atribuudiga sama e-posti metaversumis kui enne kasutajaobjektis imporditud kontaktide objekti. Ekspordi Azure AD ajal visatakse tõrge. Selline käitumine on teadlikult ja näitab vigaste andmete või et topoloogia ei tuvastatud õigesti installimise ajal.

## <a name="disabled-accounts"></a>Keelatud kontod

Keelatud kontod sünkroonitakse ka Azure AD. Keelatud kontod on levinud tähistada Exchange, näiteks konverentsiruumi ressursid. Erandiks on need kasutajad, kellel on lingitud postkast. nagu eelnevalt mainitud, need kunagi säte konto Azure AD.

Eeldatakse, et kui blokeeritud kasutajakonto ei leita, siis me ei leia active mõne muu kontoga hiljem ja objekt on ette valmistatud Azure AD userPrincipalName ja sourceAnchor leitud. Juhuks, kui metaversumi samale objektile liituda mõne muu aktiivse konto, siis selle userPrincipalName ja sourceAnchor kasutatakse.

## <a name="changing-sourceanchor"></a>SourceAnchor muutmine

Kui objekti eksporditud Azure AD, siis ei ole lubatud selle sourceAnchor enam muuta. Eksportimisel objekti metaversumi atribuut **cloudSourceAnchor** on seadistatud Azure AD **sourceAnchor** väärtus. Kui **sourceAnchor** on muutunud ja ei vasta **cloudSourceAnchor**, kuvatakse **välja AAD – kasutaja liitumine** reegli põhjustada tõrke **sourceAnchor atribuut on muutunud**. Sel juhul andmed ja konfiguratsiooni parandada nii, et sama sourceAnchor esineb metaversumi uuesti enne objekti saab uuesti sünkroonida.

## <a name="additional-resources"></a>Lisaressursid

* [Azure'i AD ühenduse sünkroonimine: Kohandamise sünkroonimise suvandid](active-directory-aadconnectsync-whatis.md)
* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
