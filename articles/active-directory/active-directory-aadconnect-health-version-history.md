<properties
    pageTitle="Azure'i AD-ühenduse seisund versiooniajalugu"
    description="Selles dokumendis kirjeldatakse selle väljalasete Azure'i AD-ühenduse seisundi ja mis on lisatud nende versioonidega."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure'i AD-ühenduse seisund: Väljaanne versiooniajalugu

Azure Active Directory meeskonnatöö värskendab regulaarselt Azure'i AD-ühenduse seisund uute funktsioonide kohta. Selles artiklis on loetletud versioone ja funktsioone, mis on välja.

## <a name="october-2016"></a>Oktoober 2016
**Agent Update:**
- Azure'i AD-ühenduse seisund agent AD FS-i \(2.6.408.0 versioon\)
    1. Kliendi IP-aadresside avastada taotluste autentimise täiustused
    2. Veaparandused seotud teatised
- Azure'i AD-ühenduse seisund agent AD DS (versioonid 2.6.408.0)
    1. Veaparandused seotud teatised.
- Azure'i AD-ühendus seisund agent sünkroonimine (versioon 2.6.353.0) järgmises versioonis Azure'i AD-ühenduse 1.1.281.0
    1. Vajalikud andmed ette sünkroonimise tõrketeated
    2. Teatiste seotud veaparandusi

**Uue eelvaade funktsioonid.**
- Sünkroonimise tõrge aruanded Azure AD ühenduse loomine

**Uued funktsioonid:**
- Azure'i AD-ühendus seisund AD FS - IP-aadressi välja on saadaval aruandes kohta ülemise 50 kasutajat halb kasutajanime ja parooli abil.

## <a name="july-2016"></a>Juuli 2016

**Uue eelvaade funktsioonid.**

- [Azure'i AD-ühenduse AD DS seisund](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>2016 jaanuar


**Agent Update:**

- Azure'i AD-ühenduse seisund agent AD FS-i (versioon 2.6.91.1512)


**Uued funktsioonid:**

- [Testi ühenduvuse tööriist Azure AD ühenduse seisund agentide](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>November 2015


**Uued funktsioonid:**

- [Rollipõhise juurdepääsu reguleerimine](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) tugi


**Uue eelvaade funktsioonid.**

- [Azure'i AD-ühenduse seisund sünkroonimist](active-directory-aadconnect-health-sync.md).

**Fikseeritud küsimustes:**

- Veaparandused tõrkeid näinud agent registreerimise käigus.

## <a name="september-2015"></a>Mai 2015

**Uued funktsioonid:**

- Vale kasutajanimi parool aruande AD FS-i
- Tugiteenuste konfigureerida autentimata HTTP-puhverserver
- Konfigureerimiseks agent Server core tugi
- Teatiste AD FS-i täiustused
- Azure'i AD-ühenduse seisund Agent Ühenduvus ja andmete AD FS-i täiustused üles laadida.


**Fikseeritud küsimustes:**

- Veaparandusi kasutus ülevaateid AD FS-i tõrke tüüpi.


## <a name="june-2015"></a>Juuni 2015

**Azure'i AD-ühenduse seisund algne väljaanne AD FS-i ja AD FS puhverserver.**

**Uued funktsioonid:**

- Teatiste AD FS-i ja AD FS puhverserver serverite meiliteatised jälgimiseks.
- Lihtne juurdepääs AD FS-i topoloogia ja mustrid AD FS-i jõudlus hinnale.
- Trend õnnestunud Turbeloa taotluste AD FS-i serverid Rühmitusalus rakendused, autentimismeetodeid, taotleda võrgu asukoht jne.
- Trende nurjunud taotluste AD FS-i serverid Rühmitusalus rakenduste tõrge tüüpi jne.
- Lihtsam Agent juurutus Azure AD üldadministraator mandaadi abil.  




## <a name="next-steps"></a>Järgmised sammud
Lisateavet [kuvari oma kohapealse identiteedi taristu ja sünkroonimise teenuste pilveteenuses](active-directory-aadconnect-health.md).
