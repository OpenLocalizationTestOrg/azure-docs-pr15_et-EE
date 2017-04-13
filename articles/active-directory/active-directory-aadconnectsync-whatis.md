<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine | Microsoft Azure'i"
    description="Selgitatakse, kuidas Azure'i AD-ühenduse sünkroonimine töötab ja kuidas kohandada."
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
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine
Azure Active Directory ühenduse sünkroonimisteenused (Azure'i AD-ühenduse sync) on peamine osa Azure'i AD-ühenduse. See hoolitseb kõik toimingud, mis on seotud identiteedi oma kohapealse keskkonna ja Azure AD vaheline andmete sünkroonimine. Azure'i AD-ühendus sünkroonimine on DirSync ja Azure AD sünkroonimine Forefront Identity Manager ja Azure Active Directory konnektor konfigureeritud.

Selles teemas on kodus **Azure'i AD-ühenduse sünkroonimine** (nimetatakse ka **sünkroonimine engine**) ja loetleb kõik muud sellega seotud teemade lingid. Leiate Azure'i AD-ühenduse lingid, [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).

Sünkroonimise teenuse koosneb kahest osast, kohapealse **Azure'i AD-ühenduse sünkroonimine** komponent ja teenusepakkuja Azure AD **Azure'i AD-ühenduse sünkroonimise teenuse**nimega. Teenus on levinud DirSync, Azure AD sünkroonimine ja Azure'i AD-ühenduse.

## <a name="azure-ad-connect-sync-topics"></a>Azure'i AD-ühendus sünkroonimine Teemad

Teema: | Mida see hõlmab ja millal lugemine
----- | -----
**Azure'i AD-ühendus sünkroonimine põhialused** |
[Ülesehituse mõistmine](active-directory-aadconnectsync-understanding-architecture.md) | Neile, kes on kasutanud sync engine ja soovite teavet arhitektuur ja termineid.
[Tehnilised terminid](active-directory-aadconnectsync-technical-concepts.md) | Lühendatud arhitektuur teema ja lühidalt selgitatakse termineid.
[Topoloogiatest Azure'i AD-ühenduse loomine](active-directory-aadconnect-topologies.md) | Kirjeldab erinevaid topoloogiatest ja stsenaariumid sync engine toetab.
**Kohandatud konfiguratsioon** |
[Installi viisardi uuesti käivitada](active-directory-aadconnectsync-installation-wizard.md) | Selgitatakse, millised suvandid on saadaval, kui Azure'i AD-ühenduse installimise viisard uuesti käivitada.
[Deklaratiivseid ettevalmistamise mõistmine](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| Kirjeldatakse nimetatakse deklaratiivseid ettevalmistamise konfiguratsiooni mudel.
[Deklaratiivseid ettevalmistamise avaldiste mõistmine](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | Kirjeldatakse süntaks avaldis deklaratiivseid ettevalmistamise kasutatavat keelt.
[Vaikimisi konfiguratsiooni mõistmine](active-directory-aadconnectsync-understanding-default-configuration.md)| Kirjeldatakse out box reeglitest ja vaikimisi konfiguratsiooni. Samuti kirjeldatakse, kuidas reegleid out box stsenaariumid töötada koos töötada.
[Mõistmine kasutajad ja kontaktid](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Eelmise teema jätkub ja kirjeldatakse, kuidas konfigureerimine kasutajatele ja kontaktide töötab koos, eriti mitme mets keskkonnas.
[Kuidas muuta vaikimisi konfigureerimine](active-directory-aadconnectsync-change-the-configuration.md) | Kuidas teha levinud konfiguratsiooni muuta atribuudi puhul tutvustatakse.
[Head tavad vaikimisi konfiguratsiooni muutmine](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Tugiteenuste piirangud ja out box konfiguratsiooni muudatuste tegemiseks.
[Filtreerimise konfigureerimine](active-directory-aadconnectsync-configure-filtering.md) | Kirjeldab erinevaid võimalusi kohta, kuidas piirata, milliseid objekte on sünkroonitud Azure AD ja samm-sammult kuidas konfigureerida järgmisi suvandeid.
**Funktsioonid ja stsenaariumid** |
[Vältida juhuslikku kustutab](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Kirjeldatakse funktsiooni *vältida juhuslikku kustutab* ja kuidas seda konfigureerida.
[Ajasti](active-directory-aadconnectsync-feature-scheduler.md) | Sisseehitatud ajasti, mis on, sünkroonimise, andmete importimisse ja eksportimisse kirjeldatakse.
[Rakendada parooli sünkroonimine](active-directory-aadconnectsync-implement-password-synchronization.md) | Kirjeldatakse parooli sünkroonimise tööpõhimõtted, kuidas rakendada ja kuidas toimida ja tõrkeotsing.
[Seadme tagasikirjutusega](active-directory-aadconnect-feature-device-writeback.md) | Kirjeldab, kuidas töötab seadme tagasikirjutusega Azure'i AD-ühenduse.
[Directory laiendid](active-directory-aadconnectsync-feature-directory-extensions.md) | Kirjeldab, kuidas laiendada Azure AD skeemi koos oma kohandatud atribuute.
**Sünkroonida teenuse** |
[Azure'i AD-ühendus sünkroonimise teenuse funktsioonid](active-directory-aadconnectsyncservice-features.md) | Kirjeldatakse sünkroonimise teenuse küljel ja Azure AD sünkroonimine sätete muutmiseks.
[Atribuut paindlikkust dubleerimine](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Kirjeldab, kuidas lubada ja kasutada paindlikkust **userPrincipalName** ja **proxyAddresses** atribuut dubleeritud väärtused.
**Toimingute ja kasutajaliides** |
[Sünkroonimise Service Manager](active-directory-aadconnectsync-service-manager-ui.md) | Kirjeldatakse sünkroonimise Service Manager UI, sh [toimingud](active-directory-aadconnectsync-service-manager-ui-operations.md), [konnektorid](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaversumi Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)ja [Metaversumi otsingu](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) vahekaardid.
[Töötoimingute ja kaalutlused](active-directory-aadconnectsync-operations.md) | Kirjeldatakse suunad, nt katastroofiabi.
**Kuidas...** |
[Azure AD konto lähtestamine](active-directory-aadconnectsync-howto-azureadaccount.md) | Kuidas lähtestada kasutatud Azure AD sünkroonimine Azure'i AD-ühenduse kaudu ühenduse teenuse konto identimisteave.
**Lisateavet ja viited** |
[Pordid](active-directory-aadconnect-ports.md) | Loetleb portide, peate avama sync engine ja teie asutusesisese kataloogid ja Azure AD vahel.
[Sünkroonitud Azure Active Directory atribuudid](active-directory-aadconnectsync-attributes-synchronized.md) | Loetleb kõik atribuudid on sünkroonitud kohapealse AD ja Azure AD.
[Funktsioonide juhend](active-directory-aadconnectsync-functions-reference.md) | Ära kõik funktsioonid saadaval deklaratiivseid ettevalmistamise.

## <a name="additional-resources"></a>Lisaressursid

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
