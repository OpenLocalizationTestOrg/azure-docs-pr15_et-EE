<properties
    pageTitle="Azure Active Directory domeeniteenused ülevaade | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused ülevaade"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure'i AD domeeniteenused

## <a name="overview"></a>Ülevaade
Azure taristu teenused võimaldavad teil juurutada mitmesuguseid arvutuste lahenduste dünaamilised viisil. Koos Azure'i Virtuaalmasinates, saate juurutada peaaegu hetkega ja maksate ainult minutid. Windowsi, Linuxi, SQL Server, Oracle, IBM, SAP-i ja BizTalki tugi kasutamisel saate juurutada mis tahes töökoormus, mis tahes keeleks, peaaegu ühtegi opsüsteemi. Järgmisi eeliseid, mis võimaldavad teil migreerimine kohapealse juurutatud pärand rakenduste Azure salvestamiseks klõpsake funktsionaalseid kulud.

Oluline osa migreerimine kohapealsesse rakenduste Azure on töötlemise identiteedi vajadustele need rakendused. Directory-rakendusi võib toetuvad LDAP lugemis-või ettevõtte kataloogist kirjutusõiguse või toetuvad Windowsi integreeritud autentimine (NTLM- või Kerberos-autentimine) lõppkasutajad autentida. Line business (LOB) rakenduste käivitamine Windows serveris on tavaliselt juurutatud ühendatud domeeni masinad, nii, et neid saab hallata turvaliselt rühmapoliitika. Kohapealse 'lifti-ja-shift' rakendused pilveteenusesse, tuleb need sõltuvuse identiteet taristu lahendada.

Administraatorid lülitage sageli ühte järgmistest lahendustest nende rakenduste juurutatud Azure identiteedi vajaduste rahuldamiseks.

- Juurutada vahel töötab Azure'i taristu teenused ja ettevõtte kataloogi asutusesisese töökoormus-saidilt VPN-ühendus.
- Ettevõtte AD domeeni/mets taristu laiendamine häälestades koopia domeenikontrollerid Azure'i virtuaalmasinates abil.
- Juurutada eraldiseisev domeeni Azure Azure'i virtuaalmasinates juurutatud domeenikontrollerid abil.

Kõik need lähenemisel on kõrge maksumuse ja haldus pea kohal. Administraatorid peavad juurutamine domeenikontrollerid Azure'i virtuaalmasinates kasutamine. Lisaks peavad haldamine, turvaline, paik, jälgida, varundamine ja nende virtuaalmasinates tõrkeotsing. Kohapealse kataloogi VPN-ühenduste sõltuvust põhjustab juurutatud Azure olema siirdamiseks võrgu tõrgete või katkestuste töökoormus. Nende katkeb tulemuseks omakorda alumise sees ja nende rakenduste vähendatud töökindluse.

Lõime Azure AD domeeniteenused lihtsam asemel esitada.


## <a name="introducing-azure-ad-domain-services"></a>Azure'i AD domeeniteenused tutvustus
Azure'i AD domeeniteenused pakub hallatava domeeni domeeni Liitu rühma poliitika, LDAP, Kerberos/NTLM-autentimine, mis on Windows Server Active Directory ühilduda. Saab kasutada nende domeeniteenused ilma vajaduseta juurutada, hallata ja paik domeenikontrollerid pilveteenuses. Azure'i AD domeeniteenused ühendab teie olemasoleva Azure AD rentniku, mistõttu on võimalik sisse logida sisse oma ettevõtte mandaadi abil. Lisaks saate rühmi ja kasutajakontod turvamiseks ressurssidele, tagades seega on sujuvam 'lifti-ja-shift"kohapealse ressurssi Azure'i taristu teenused.

Azure'i AD domeeniteenused funktsioon töötab sujuvalt sõltumata sellest, kas teie Azure AD rentniku pilves lugemisõigused või oma kohapealse Active Directoryga sünkroonitud.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure'i AD domeeniteenused vaid ettevõtete jaoks
Pilveteenuse-ainult Azure AD rentniku (sageli nimetatakse "hallatavate rentnikud") ei saa mis tahes kohapealse identiteedi andmed väiksemates vähem ruumi. Teisisõnu, Kasutajakontod, nende paroolid ja rühmaliikmeid on kõik kohalikke pilveteenusesse - mis on loodud ja Azure AD hallatavate. Mõelge hetkeks, et Contoso on pilv-ainult Azure AD rentniku. Nagu on näidatud järgmisel pildil, Contoso's administraator on konfigureeritud virtuaalse võrgu Azure'i taristu teenused. Rakenduste ja serveri töökoormus on juurutatud Azure'i virtuaalmasinates virtuaalse võrku. Kuna Contoso on vaid rentniku, kõigi kasutajate identiteetide, volituste ja rühmaliikmeid luuakse ja hallatakse Azure AD.

![Azure'i AD domeeni teenuste ülevaade](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso on IT-administraator saab lubada Azure AD domeeniteenused oma Azure AD rentniku ja valige domeeniteenused virtuaalse võrku kättesaadavaks tegemiseks. Pärast seda, Azure AD domeeniteenused hallatava domeeni sätted ja teeb selle kättesaadavaks virtuaalse võrgu. Kõik kasutajakontod, rühmaliikmeid ja kasutajatunnust Contoso's Azure AD rentniku saadaval on ka selle vastloodud domeeni saadaval. See funktsioon võimaldab kasutajatel ettevõte ettevõtte mandaadi abil – näiteks kui domeeni ühendatud masinad kaudu kaugtöölaua kaugühenduse teel ühenduse domeeni sisse logida. Administraatorid saavad ettevalmistamise ressurssidele abil olemasoleva rühmaliikmeid Domeen. Kasutusele võetud virtuaalmasinates virtuaalse võrgus saate kasutada funktsioone, nagu domeeni Liitu, LDAP lugemine, LDAP siduda, NTLM ja Kerberos-autentimist ja rühmapoliitika.

Mõne hallatava domeeni, mis on Azure AD domeeniteenused ettevalmistatud olulisemaid aspekte on järgmised:

- Contoso on IT-administraator pole vaja haldamine, paik või selle domeeni või mis tahes selle hallatava domeeni domeenikontrollerid jälgimine.
- Ei ole vaja AD kopeerimine selle domeeni haldamiseks. Kasutajakontode ja rühmaliikmeid Contoso's Azure AD rentniku mandaat on automaatselt saadaval selle hallatava domeenis.
- Kuna domeeni haldab Azure AD domeeniteenused, ongi Contoso administraator pole domeeni administraatori või ettevõtte administraatori õigusi selles domeenis.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure'i AD domeeniteenused hü ettevõtete jaoks
Ettevõtted, kus hübriid IT taristu tarbimine cloud ressursse ja kohapealse ressursse. Organisatsiooni sünkroonida oma Azure AD rentniku oma kohapealse kataloogi identiteedi teavet. Nagu hübriid ettevõtted vaadata rohkem migreerimiseks kohapealse taotluste pilveteenusesse, eriti pärand directory arvestada rakendused, Azure AD domeeniteenused võib olla kasulik neid.

Litware Corporation on juurutatud [Azure'i AD-ühenduse](../active-directory/active-directory-aadconnect.md), identiteedi teavet oma Azure AD rentniku oma kohapealse kataloogi sünkroonimine. Identiteedi teavet, mida sünkroonitakse sisaldab Kasutajakontod, nende mandaadi hashes autentimine (parooli sünkroonimise) ja rühmaliikmeid.

> [AZURE.NOTE] **Parooli sünkroonimine on kohustuslik hübriid ettevõtted Azure AD domeeni teenuste kasutamiseks**. Selle nõude sellepärast, et kasutajate mandaadid on vaja hallatava domeeni Azure AD domeeni teenuste, nende kasutajate kaudu NTLM või Kerberose autentimismeetodite autentida.

![Azure'i AD domeeniteenused Litware ettevõtte jaoks](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Eelmisel joonisel ettevõtted, kus hübriid IT taristu, nt Litware Corporation, kuidas kasutada Azure AD domeeniteenused. Litware's rakenduste ja serveri töökoormus, mis nõuavad domeeniteenused on juurutatud virtuaalse võrgu Azure'i taristu teenused. Litware oma IT-administraator saab lubada Azure AD domeeniteenused oma Azure AD rentniku ja valige see virtuaalse võrgu kättesaadavaks tegemiseks hallatavate domeeni. Kuna Litware on hübriid IT taristu organisatsiooni, Kasutajakontod, rühmad ja mandaadi sünkroonitakse oma Azure AD rentniku oma kohapealse kataloogi. See funktsioon võimaldab kasutajatel ettevõtte mandaadi abil – näiteks kui kaugühenduse teel ühenduse masinad ühendatud domeeni kaudu kaugtöölaua domeeni sisse logida. Administraatorid saavad ettevalmistamise ressurssidele abil olemasoleva rühmaliikmeid Domeen. Kasutusele võetud virtuaalmasinates virtuaalse võrgus saate kasutada funktsioone, nagu domeeni Liitu, LDAP lugemine, LDAP siduda, NTLM ja Kerberos-autentimist ja rühmapoliitika.

Mõne hallatava domeeni, mis on Azure AD domeeniteenused ettevalmistatud olulisemaid aspekte on järgmised:

- Hallatavate domeen on eraldiseisev Domeen. See ei ole Litware's kohapealse domeeni laiendit.
- Litware oma IT-administraator pole vaja haldamine, paik, või selle hallatava domeeni domeenikontrollerid jälgimine.
- Ei ole vaja AD kopeerimine selle domeeni haldamiseks. Kasutajakontode ja rühmaliikmeid identimisteabe Litware's kohapealse kataloogi sünkroonitakse Azure AD Azure'i AD-ühenduse kaudu. Need Kasutajakontod, rühmaliikmeid ja mandaadi on automaatselt saadaval hallatava domeeni.
- Kuna domeeni haldab Azure AD domeeniteenused, Litware kasutaja seda administraator pole domeeni administraatori või ettevõtte administraatori õigusi selles domeenis.


## <a name="benefits"></a>Eelised
Azure'i AD domeeniteenused saate nautida järgmised eelised:

-   **Lihtne** – te saate samade identiteedi virtuaalmasinates juurutatud Azure'i taristu teenused vaid paari klõpsuga. Teil pole vaja juurutada ja hallata identiteedi taristu Azure- või häälestustõ ühenduvuse naasmiseks oma kohapealse identiteedi taristu.

-   **Integreeritud** – Azure AD domeeniteenused on integreeritud sügavalt teie Azure AD rentniku. Nüüd saate Azure AD integreeritud pilvepõhise ettevõte kausta, mis vajadustele tänapäevased rakendused ja traditsiooniline directory-rakendusi.

-   **Ühilduvad** – Azure AD domeeniteenused põhineb tõestatud enterprise hinde taristu Windows Server Active Directory. Seega saate oma rakendused sõltuvad suurem astet ühilduvuse Windows Server Active Directory funktsioone. Kõik funktsioonid saadaval Windows Server AD on praegu saadaval Azure AD domeeniteenused. Saadaolevate funktsioonide ühilduvad siiski oma kohapealse infrastruktuuri toetuvad vastava Windows Server AD funktsioone. LDAP, Kerberos, NTLM, rühmapoliitika ja domeeni liitumise võimaluste moodustavad küps pakkumine, mida kontrollitud ja rafineeritud üle erinevate Windows Server väljalasete.

-   **Kulutõhus** – Azure AD domeeniteenused, saate vältida taristu ja haldus koormust, mis on seotud identiteedi infrastruktuuri traditsiooniline directory arvestada rakenduste haldamine. Saate liikuda Azure'i taristu teenused need rakendused ja kasu suurem kokkuhoid funktsionaalseid kulud.
