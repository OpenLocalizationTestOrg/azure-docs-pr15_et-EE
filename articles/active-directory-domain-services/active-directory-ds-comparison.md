<properties
    pageTitle="Azure'i AD domeeniteenused: Võrdlus Azure AD domeeniteenused iseseisev domeenikontrolleritega | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused iseseisev domeenikontrolleritega võrdlus"
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Kuidas otsustada, kui Azure AD domeeniteenused sobib kaasuse kasutamine
Azure'i AD domeeniteenused võimaldab juurutada oma töökoormus Azure'i taristu teenused, ilma muretsema säilitada oma identiteedi avaldamistaristu. See hallatav teenus erineb tüüpiline Windows Server Active Directory juurutuse juurutada ja hallata ise. Teenus on mõeldud hõlpsamaks, juurutamine, automatiseeritud seisundi jälgimine ja parandamise ja mõne lihtsa identiteedi taristu pilve. Me pidevalt teenuse lisamiseks tavastsenaariumid juurutamise tugi.

Otsustada, kas Azure AD domeeni teenuseid kasutada või tööle hakata ja hallata oma (iseseisev) Azure AD taristu:

- Siit leiate loendi [funktsioonid pakutud Azure AD domeeniteenused](active-directory-ds-features.md).

- Tutvuge levinud [juurutamise stsenaariumide Azure AD domeeni teenuste jaoks](active-directory-ds-scenarios.md).

- Lõpuks [võrdlus Azure AD domeeniteenused iseseisev AD suvand](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Azure'i AD domeeniteenused iseseisev AD domeeni Azure võrdlus
Järgmine tabel aitab teil otsustada Azure AD domeeni teenuste kasutamine ja haldamine oma Azure AD infrastruktuuri vahel.

|**Funktsioon**|**Azure'i AD domeeniteenused**|**Azure'i VM 'Iseseisev' AD**|
|---|:---:|:---:|
|[**Hallatav teenus**](active-directory-ds-comparison.md#managed-service)|**& #x2713;**|**& #x2715;**|
|[**Juurutamise turvalisus**](active-directory-ds-comparison.md#secure-deployments)|**& #x2713;**|Administraator peab olema turvaline juurutamise.|
|[**DNS-i server**](active-directory-ds-comparison.md#dns-server)|**& #x2713;** (hallatav teenus)|**& #x2713;**|
|[**Domeeni või ettevõtte administraatori õigusi.**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x2715;**|**& #x2713;**|
|[**Domeeni liitumine**](active-directory-ds-comparison.md#domain-join)|**& #x2713;**|**& #x2713;**|
|[**Domeeni autentimist kasutades Kerberose ja NTLM**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x2713;**|**& #x2713;**|
|[**Kohandatud OU struktuur**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x2713;**|**& #x2713;**|
|[**Skeemi laiendid**](active-directory-ds-comparison.md#schema-extensions)|**& #x2715;**|**& #x2713;**|
|[**AD domeeni/mets loodab**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x2715;**|**& #x2713;**|
|[**LDAP lugemine**](active-directory-ds-comparison.md#ldap-read)|**& #x2713;**|**& #x2713;**|
|[**Turvaline LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x2713;**|**& #x2713;**|
|[**LDAP kirjutamine**](active-directory-ds-comparison.md#ldap-write)|**& #x2715;**|**& #x2713;**|
|[**Rühmapoliitika**](active-directory-ds-comparison.md#group-policy)|Lihtne|Täielik|
|[**Geo jaotatud juurutuste**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x2715;**|**& #x2713;**|


#### <a name="managed-service"></a>Hallatav teenus
Azure'i AD domeeniteenuste domeeni haldab Microsoft. Te ei pea muretsema lappimine, värskenduste, jälgimine, varukoopiate, ja tagada domeeni. Nende haldustoimingute pakutakse teenust Microsoft Azure'i hallatava domeeni jaoks.

#### <a name="secure-deployments"></a>Juurutamise turvalisus
Ühe Microsofti turvalisus head tavad AD juurutuste turvaliselt hallatava domeeni lukus. Järgmistest headest tavadest tulenevad AD toote meeskonnatöö aastat kogemusi tehnika ja AD juurutuste toetavad. Iseseisev juurutuste korral peate vajadus teatud juurutamise lukustada down/secure juurutamise.

#### <a name="dns-server"></a>DNS-i server
Mõni Azure AD domeeniteenused hallatava domeeni sisaldab haldab DNS-i. 'AAD näiteks Põhiliselt administraatorite' rühma liikmed saavad hallata hallatava domeeni DNS-i. Selle rühma liikmetel on esitatud täielik DNS-i halduse õigusi hallatava domeeni. DNS-i haldamise saab teha 'DNS-i halduse konsooli"paketis Remote Server Administration Tools (RSAT) abil.

#### <a name="domain-or-enterprise-administrator-privileges"></a>Domeeni või ettevõtte administraatori õigusi.
Nende administraatoriõigusi ei pakuta mõne AAD-DS hallatavate domeenis. Rakendusi, mis nõuavad nende administraatoriõigusi olema installitud ja käivitamise ei saa käivitada vastu hallatavate domeenid. Administraatoriõigusi väiksem Alamhulk on saadaval nimega 'AAD näiteks Põhiliselt administraatorid' delegeeritud halduse rühma liikmeid. Nende õiguste kaasata õigusi DNS-i konfigureerimine, rühmapoliitikaga, saada administraatoriõigusi domeeni ühendatud arvutites jne.

#### <a name="domain-join"></a>Domeeni liitumine
Saate liituda virtuaalmasinates sarnaneb kui liitute arvutite AD domeeni hallatava domeeni.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Domeeni autentimist kasutades Kerberose ja NTLM
Azure'i AD domeeniteenused abil saate oma ettevõtte mandaadi hallatava domeeni kinnitamiseks. Mandaat on hoida oma Azure AD rentniku sünkroonis. Sünkroonitud rentnikud, tagab Azure'i AD-ühenduse, et Azure AD sünkroonitakse kohapealse identimisteabe tehtud muudatused. ISESEISEV domeeni häälestamise, peate domeeni usaldusseos koos mõne asutusesisese konto mets autentimiseks volituste ettevõtte kasutajate jaoks häälestada. Vaheldumisi, peate tagamaks, et kasutaja paroolid sünkroonida Azure domeeni domeenikontrolleri virtuaalmasinates AD kopeerimine häälestamine.

#### <a name="custom-ou-structure"></a>Kohandatud OU struktuur
'AAD näiteks Põhiliselt administraatorite' rühma liikmed saavad luua kohandatud organisatsiooniüksused hallatava domeeni.

#### <a name="schema-extensions"></a>Skeemi laiendid
Base skeemiga Azure AD domeeniteenused hallatava domeeni ei saa laiendada. Seetõttu rakendused laiendid AD skeemi (näiteks uus atribuudid jaotises kasutajaobjektis) ei saa tühistada ja nihutatud AAD-DS domeenid.

#### <a name="ad-domain-or-forest-trusts"></a>AD domeeni või mets usalduste
Hallatava domeeni ei saa konfigureerida usalda seoste (sissetulev/väljaminev) muude domeenide häälestamine. Seetõttu ei saa stsenaariumid, nt ressursside mets juurutuste või juhul, kui te ei soovi sünkroonida paroolide Azure AD kasutada Azure AD domeeniteenused.

#### <a name="ldap-read"></a>LDAP lugemine
Hallatava domeeni toetab LDAP lugeda töökoormus. Seega saate juurutada rakendusi, mis toiminguid LDAP loetuks hallatava domeeni suhtes.

#### <a name="secure-ldap"></a>Turvaline LDAP
Saate konfigureerida Azure AD domeeniteenused turvaline LDAP juurdepääsu hallatava domeeni, sh Interneti kaudu.

#### <a name="ldap-write"></a>LDAP kirjutamine
Hallatavate domeen on kirjutuskaitstud ainult kasutaja objektid. Seetõttu ei toimi rakendusi, mis toiminguid LDAP kirjutamine vastu atribuute kasutaja objekti hallatava domeeni. Lisaks kasutaja paroolid ei saa muuta hallatava domeeni. Teine näide oleks muutmine rühmaliikmeid või rühma atribuute, hallatavate domeenis, mis ei ole lubatud. Siiski kõik muudatused kasutaja atribuudid või paroolide tehtud Azure AD (PowerShelli/Azure portaali) kaudu või kohapealse sünkroonitakse AD AAD-DS hallatava domeeni.

#### <a name="group-policy"></a>Rühmapoliitika
Keerukate rühma poliitika importida ei toetata AAD-DS hallatavate domeenis. Näiteks ei saa luua ja juurutada eraldi GPO iga kohandatud OU Domeen või kasutada WMI filtreerimine GP suunamise. Sisseehitatud GPO iga "AADDC arvutid" ja 'AADDC kasutajad' ümbriste, mida saate kohandatud rühmapoliitikaga on.

#### <a name="geo-dispersed-deployments"></a>Geo hajutatud juurutuste
Azure'i AD domeeniteenused hallatavate domeenid on saadaval ühe virtuaalse võrgu Azure. Stsenaariumid, mis nõuavad domeenikontrollerid kogu maailmas mitme Azure'i piirkondades saadaval olla, häälestamise domeenikontrollerid Azure IaaS VMs võib olla parem alternatiiv.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>"Iseseisev" (DIY) AD Juurutussuvandid
Teil võib olla juurutamise kasutamine – juhul, kui teil on vaja mõned funktsioonid pakutud Windows Server AD installi. Sellisel juhul kaaluge üks iseseisev (DIY) järgmistest suvanditest:

- **Autonoomse cloud Domeen:** Saate häälestada autonoomse pilvepõhise domeeni Azure'i virtuaalmasinates, mis on konfigureeritud domeenikontrolleritena abil. Selle taristu ei integreerida oma kohapealse keskkonna AD. See suvand nõuab teistsuguseid 'pilve identimisteabe' login/hallata VMs pilveteenuses.

- **Ressursi mets juurutamise:** Saate häälestada domeeni selle ressursi mets topoloogia abil Azure'i virtuaalmasinates domeenikontrolleritena konfigureeritud. Järgmisena saate konfigureerida seose AD usalda oma kohapealse keskkonna AD. Saate selle ressursi mets pilveteenuses Domeeniga liitumine arvutites (Azure'i VMs). Kasutaja autentimisega juhtub kas üle kohapealse kataloogi VPN/ExpressRoute-ühendus.

- **Hõlmavad kohapealse domeeni Azure:** Saate luua ühenduse mõne Azure virtuaalse võrgu VPN-/ ExpressRoute ühendus, kasutades kohapealse võrgu nii, et Azure VMs saate liitunud oma kohapealse AD. Teine võimalus on edendada oma kohapealse domeeni nimega VM Azure koopia domeenikontrollerid. Seejärel saate selle häälestada, VPN/ExpressRoute-ühenduse kaudu oma kohapealse kataloogi kopeerida. Selle režiimi juurutamise laiendab kohapealse domeeni tõhus Azure.

> [AZURE.NOTE] Võite määrata iseseisev suvand paremini sobib teie juurutamise kasutamise juhtudel. Kaaluge [tagasiside jagamine](active-directory-ds-contact-us.md) hõlbustavate funktsioonide aitaks mõista teie valitud Azure AD domeeniteenused tulevikus. See tagasiside aitab meil arenevad teenuse paremini vastavalt oma vajadustele juurutamise ja kasutamise juhtudel.

Meil on avaldatud [juurutamine Windows Server Active Directory Azure'i Virtuaalmasinates juhised](https://msdn.microsoft.com/library/azure/jj156090.aspx) , mis aitavad iseseisev installid oleks lihtsam.


## <a name="related-content"></a>Seotud teemad
- [Funktsioonid - Azure AD domeeniteenused](active-directory-ds-features.md)

- [Juurutamise stsenaariumide - Azure AD domeeniteenused](active-directory-ds-scenarios.md)

- [Windows Serveri Active Directory Azure'i Virtuaalmasinates juurutamise juhised](https://msdn.microsoft.com/library/azure/jj156090.aspx)
