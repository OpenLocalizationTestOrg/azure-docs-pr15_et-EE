<properties
    pageTitle="Autentimisel ilma parooli Microsoft Passporti kaudu | Microsoft Azure'i"
    description="Ülevaade Microsoft Passporti ja täiendava teabe juurutamine Microsoft Passporti kohta."
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

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Parooli Microsoft Passporti kaudu ilma autentimine

Kasutajate turvalisuse ei praeguse meetodi paroole eraldi autentimise kohta. Kasutajate taaskasutamine ja parooli unustanud. Paroolid on breachable, phishable, kus praod ja sõnaraamatus. Nad saavad ka raske meeles pidada ja kus eest nagu "[arvestatud räsi](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-microsoft-passport"></a>Microsoft pass kohta
Microsoft Passporti on era/avalik võti või serdi autentimise lähenemine ettevõtete ja tarbijate jaoks, mis ületab paroolid. Selle vormi autentimise tugineb paari identimisteavet, mida saate asendada paroolid ja on seotud rikkumiste eest, vargused ja andmepüügi.

 Microsoft Passporti võimaldab kasutaja autentimiseks Microsofti konto, Windows Server Active Directory kontot, Microsoft Azure Active Directory (Azure AD) konto või mitte-Microsofti teenus, mis toetab kiire identiteedi Online (FIDO) autentimist. Pärast algse kaheastmelise kontrollimise ajal Microsoft Passporti registreerimise Microsoft Passporti on kasutaja seadme häälestamine ja kasutajale määratakse žest, mis võib olla Windows Hello või PIN-koodi. Kui kasutaja annab žest tema identiteedi kinnitamiseks. Windows kasutab Microsoft Passporti kasutaja autentimiseks ja aitavad kaitstud ressursside ja teenuste juurde pääseda.

Privaatvõti on kättesaadav üksnes kaudu "kasutaja žest" nagu PIN-koodi, biomeetria või remote seade nagu kiipkaardi, mida kasutaja kasutab seadme sisse logida. See teave on seotud serdi või asümmeetriline võtme jutumärkide paar ilma. Privaatvõti on kinnitatud kui seade on usaldusväärse platvormi mooduli (TPM) kiip riistvara. Privaatvõti ei kao seade.

Avalik võti on registreeritud ja Azure Active Directory Windows Server Active Directory (kohapealse). Identiteedipakkujad (IDPs) Kinnitage kasutaja, vastendades avalik võti kasutaja privaatvõti ja sisestage sisselogimise teave ühe kellaaja parooli (OTP), PhoneFactor või muu teatise süsteem.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Miks peaks suurettevõtete vastu võtma Microsoft Passporti

Microsoft Passporti võimaldades suurettevõtete saate nende ressursside isegi turvalisemaks muuta:

* Kuidas häälestada Microsoft Passporti riistvara eelistatud suvandiga. See tähendab, et klahvid loob TPM 1.2 või TPM 2.0, kui need on saadaval. Kui TPM pole saadaval, loob tarkvara võti.

* Määratlemise keerukus ja PIN-koodi, pikkus ja kas Tere tulemast kasutamine on lubatud teie asutuses.

* Konfigureerida Microsoft Passporti toetamiseks kiipkaardi nagu stsenaariumid sertifikaadi-põhine usalduskeskuse abil.

## <a name="how-microsoft-passport-works"></a>Microsoft Passporti tööpõhimõtted
1. Klahvid loodud riistvara TPM või tarkvara. Mitmed seadmed on sisseehitatud TPM kiip, mis tagab riistvara järgi integreerida krüptitud klahvid seadmed. TPM 1.2 või TPM 2.0 loob võtmed või serdid, mis on loodud loodud võtmed.

2. TMP kinnitab need riistvara seotud võtmed.

3. Ühe Ava žest avab seade. See žest võimaldab kasutada mitut ressursid, kui seade on domeeni ühendatud või Azure AD liitunud.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Microsoft Passporti elutsükli tööpõhimõtted

![Microsoft Passporti elutsükkel](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Eelmise diagramm näitab era/avalik võti paari ja identiteedipakkuja kinnitus. Neid juhiseid iga selgitatakse üksikasjalikult siit:

1. Kasutaja osutub tema identiteedi mitme sisseehitatud õigekirjakontrolli meetodite (žeste, füüsilise kiipkaardiga kaardid, mitmikautentimise) ja edastab selle teabe abil on identiteedi pakkuja (IDP) nagu Azure Active Directory või kohapealse Active Directory.

2. Seadme seejärel loob võti, võti kinnitab, võtab avalik osa selle klahvi, manustab station laused, sisse logib ja saadetakse IDP registreerida võti.

4. Kui ka IDP registreerib avalik võti osa, on IDP väljakutsed seadme logige privaatne osa võti.

5. Ka IDP seejärel kinnitab ja probleemid autentimise luba, mis võimaldab kasutajale ja seadmele juurdepääsu kaitstud ressursid. IDPs saate kirjutada mitu platvormi rakendused või Brauseritugi (kaudu Webcrypto/JavaScripti API) abil saate luua ja kasutada Microsoft Passporti identimisteavet, nende kasutajate jaoks.

## <a name="the-deployment-requirements-for-microsoft-passport"></a>Microsoft Passporti juurutamise nõuded
### <a name="at-the-enterprise-level"></a>Ettevõtte tasemel

* Ettevõttel on Azure tellimuse.

### <a name="at-the-user-level"></a>Kasutaja tasemel

* Kasutaja arvutis töötab Windows 10 Professional või Enterprise.

Üksikasjalik juurutamise juhised leiate teemast [Lubamine Microsoft pass asutuses tööd](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>Lisateave

* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
