<properties
    pageTitle="Hübriid identiteedi: Kataloogi integreerimise tööriistade võrdlus | Microsoft Azure'i"
    description="See on lehel antakse täielik tabeli, mis on erinevate kataloogiintegreerimise Tööriistad kataloogi integreerimise kasutatavate võrdleb."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hübriid identiteedi kataloogi integreerimise võrdlus

Aasta jooksul on kataloogiintegreerimise tööriistad on kasvanud ja muutunud.  See dokument on pakkuda nende tööriistade kokkuvõtlik ja funktsioone, mis on saadaval iga võrdlus.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure'i AD-ühendus hõlmab komponente ja funktsioone, mis on varem välja antud Dirsync ja AAD Sync. Nende tööriistade enam vabastatakse ükshaaval ja kõiki tulevaste täiustused kaasatakse värskendused Azure'i AD-ühenduse, nii, et teate alati, kust kõige praeguse funktsionaalsuse.
>
>DirSync ja Azure AD sünkroonimine on aegunud. Lisateavet leiate [siit](active-directory-aadconnect-dirsync-deprecated.md).


Järgmised klahvi abil iga tabeli.

● = Nüüd saadaval  
FR = tulevaste väljalasete  
PP = avaliku eelvaade  

## <a name="on-premises-to-cloud-synchronization"></a>Kohapealse Cloud sünkroonimine

| Funktsioon  | Azure Active Directory ühendamine  | Azure Active Directory sünkroonimisteenused (AAD Sync) | Azure Active Directory sünkroonimistööriist (DirSync)| Forefront Identity Manager 2010 R2 (FIM) |Microsofti Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Ühenduse loomine ühe kohapealse AD mets | ● | ● | ● | ● |● |
| Ühenduse loomine mitme kohapealse AD metsade |●  | ● |  | ● |● |
| Ühenduse loomine mitme asutusesisese Exchange'i ettevõtetes | ● |  |  |  | |
| Ühe kohapealse Lightweight directory ühendamine | FR |  |  | ● |● |
| Ühenduse loomine mitme kohapealse LDAP kataloogid |FR  |  |  | ● |● |
| Ühenduse loomiseks kohapealse AD ja kohapealsete LDAP kataloogid |FR  |  |  | ● |● |
| Ühenduse loomine kohandatud süsteemide (nt SQL-i, Oracle, MySQL-i jne) | FR |  |  | ● |● |
| Sünkroonimine klientide määratletud atribuute (directory laiendid) | ● |  |  |  |  |
|Ühenduse loomiseks kohapealse HR (nt SAP, Oracle'i e PeopleSoft)| FR |  |  | ● |● |
|Kohapealse süsteemide ettevalmistamise toetab FIM sünkroonimise reeglid ja konnektorid.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Kohapealse sünkroonimise pilveteenuses

| Funktsioon  | Azure Active Directory ühendamine  | Azure Active Directory sünkroonimisteenused | Azure Active Directory sünkroonimistööriist (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsofti Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Tagasikirjutusega seadmed | ● |  | ● |  ||
| Atribuut tagasikirjutusega (Exchange'i hübriidjuurutuse) jaoks | ● | ● | ● | ● |● |
| Kasutajate ja rühmade objektide tagasikirjutusega |  ●|  | |  ||
| Tagasikirjutusega paroolid (alates Iseteeninduslik parooli lähtestamine (SSPR) ja parooli muutmine) |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Autentimise funktsioonide tugi

| Funktsioon  | Azure Active Directory ühendamine | Azure Active Directory sünkroonimisteenused | Azure Active Directory sünkroonimistööriist (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsofti Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Ühekordse parooli sünkroonimise kohapealse AD mets | ● | ● | ● |  ||
| Mitme parooli sünkroonimise asutusesisese AD metsade |  ●| ● |  |  ||
| Ühekordse sisselogimise Federation abil | ● | ● | ● | ● |● |
| Tagasikirjutusega paroolid (alates SSPR ja parooli muutmine) |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Häälestamise ja installimine

| Funktsioon  | Azure Active Directory ühendamine  | Azure Active Directory sünkroonimisteenused | Azure Active Directory sünkroonimistööriist (DirSync) | Microsofti Identity Manager 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Toetab domeenikontrolleri | ● | ● | ● |  |
| Toetab SQL Expressi installimine | ● | ● | ● |  |
| Lihtne täiendamist DirSync |● |  |  |  |
| Asukoht administraatori Kasutuskogemuse Windows Server keeled | ● | ● | ● |  |
|Lokaliseerimine lõppkasutaja Kasutuskogemuse Windows Server keeled| |  |  |● |
| Windows Server 2008 ja Windows Server 2008 R2 tugi | ● sünkroonimise, ei ühinemise jaoks| ● | ●  | ● |
| Windows Server 2012 ja Windows Server 2012 R2 tugi | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Filtreerimine ja konfigureerimine

Funktsioon  | Azure Active Directory ühendamine | Azure Active Directory sünkroonimisteenused | Azure Active Directory sünkroonimistööriist (DirSync) | Forefront Identity Manager 2010 R2 (FIM)| Microsofti Identity Manager 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filtreerida domeenid ja ettevõtte üksused | ● | ● | ● | ●  | ●
Objektide atribuudi väärtuste filtreerimine | ● | ● | ● | ●| ●
Luba minimaalset arvu atribuute ei sünkroonita (MinSync) | ● | ● |  ||
Lubage erinevate teenuse malle kohaldatavad atribuut puhul |●  | ● |  ||
Luba tulenevad AD Azure AD atribuutide eemaldamine | ● | ● |  |  |
Luba atribuut puhul täpsem kohandamine | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedi ja Azure Active Directory integreerimine](active-directory-aadconnect.md).
