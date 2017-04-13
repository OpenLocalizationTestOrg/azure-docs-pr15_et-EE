<properties
    pageTitle="KKK - Azure Active Directory domeeniteenused | Microsoft Azure'i"
    description="Korduma kippuvad küsimused Azure Active Directory domeeniteenused kohta"
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
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory domeeniteenused: Korduma kippuvad küsimused (KKK)

Sellelt lehelt leiate vastused korduma kippuvatele küsimustele Azure Active Directory domeeniteenused. Kontrollimiseks tagasi värskendused.

### <a name="troubleshooting-guide"></a>Tõrkeotsing juhend
Vaadake meie [tõrkeotsingu juhend](active-directory-ds-troubleshooting.md) kui konfigureerimise või manustamist Azure AD domeeniteenused levinud probleemidele lahendusi.


### <a name="configuration"></a>Konfigureerimine

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Kas saab luua mitu domeeni ühe Azure AD directory?
Ei. Saate luua ainult ühe domeeni teenindatud Azure AD domeeni teenused ühe Azure AD kataloogi.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Lubada Azure AD domeeniteenused Azure'i ressursihaldur virtuaalse võrgus?
Ei. Azure'i AD domeeniteenused saab lubada ainult klassikaline Azure virtuaalse võrku. Saate luua ühenduse klassikaline virtuaalse võrgu ressursihaldur virtuaalse võrgu virtuaalse võrgu silmitsemine ressursihaldur virtuaalse võrku hallatava domeeni kasutamiseks.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Esitada Azure AD domeeniteenused saadaval mitme virtuaalse võrgu sees oma tellimust?
Teenuse ise otse ei toeta seda stsenaariumi. Azure'i AD domeeniteenused on saadaval ainult üks virtuaalse võrk korraga. Siiski võib konfigureerida Ühenduvus mitme virtuaalse võrgu esitamist Azure AD domeeniteenused teiste virtuaalse võrkudega. Selles artiklis kirjeldatakse, kuidas saate [Azure virtuaalne võrkude ühendamine](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Azure'i AD domeeniteenused PowerShelli kaudu saate lubada?
Azure'i AD domeeni teenuste kasutamise PowerShelli/automatiseeritud pole praegu saadaval.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Azure'i AD domeeniteenused on uue Azure'i portaalis?
Ei. Azure'i AD domeeniteenused saab seadistada ainult [Azure klassikaline portaalis](https://manage.windowsazure.com). Me oodata tugi [Azure portaali](https://portal.azure.com) tulevikus laiendada.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Saate lisada domeenikontrollerid Azure AD domeeniteenused hallatava domeeni?
Ei. Azure'i AD domeeni teenuste domeen on hallatava domeeni. Teil pole vaja ettevalmistamise, konfigureerimine või muul viisil haldamine selle domeeni - domeenikontrollerid järgmiste tegevuste haldus on antud teenuse Microsoft. Seetõttu ei saa lisada täiendavad domeenikontrollerid (lugemis-ja kirjutamisõigusega või kirjutuskaitstud) hallatava domeeni.

### <a name="administration-and-operations"></a>Haldamine ja toimingud

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Saate ühendada domeenikontrolleri minu kaugtöölaua kasutamise hallatava domeeni?
Ei. Teil pole domeenikontrollerid hallatava domeeni kaudu kaugtöölaua ühenduse õigused. 'AAD näiteks Põhiliselt administraatorite' rühma liikmed saate hallata hallatava domeeni AD haldustööriistade, nt Active Directory haldus Center (ADAC) või AD PowerShelli kasutamine. Nende tööriistade installitakse Windows Server ühendatud hallatava domeeni 'Remote Haldustööriistad' funktsiooni abil.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Ma olen lubatud Azure AD domeeniteenused. Kasutajakonto, mis on kasutada domeeni Liitu masinad selle domeeni?
Olete lisanud haldusrühma (nt "AAD näiteks Põhiliselt administraatorid") kasutajad saate Domeeniga liitumine masinad. Lisaks kasutajatele selles jaotises antakse masinad, mis on liitunud domeeni töölauaversiooni kaugjuurdepääs.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Ma valitsema domeeni administraatoriõigused Azure AD domeeni teenuste domeeni?
Ei. Te ei anta hallatava domeeni administraatoriõigused. Domeeni administraatori nii ettevõtte administraatori õigused ei ole saadaval domeeni kasutamiseks. Olemasoleva domeeni administraatori või ettevõtte administraator rühma Azure AD kataloogi ka ei anta domeeni ja ettevõtete administraatoriõigusi domeenis.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>LDAP või muude AD Haldustööriistad Azure AD domeeni teenuste domeenide kasutamine rühmaliikmeid saab muuta?
Ei. Rühmaliikmeid ei saa muuta domeenid teenindatud Azure AD domeeni teenused. Sama kehtib kasutaja atribuudid. Võite siiski muutmine rühmaliikmeid või kasutaja atribuutide Azure AD või kohapealse domeeni. Muudatused sünkroonitakse automaatselt Azure AD domeeniteenused.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Saate pikendada skeemiga Azure AD domeeni teenuste domeeni?
Ei. Skeemi haldab Microsoft hallatava domeeni. Azure'i AD domeeniteenused ei toeta skeemi laiendid.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Saate muuta või lisada oma hallatava domeeni DNS-i kirjeid?
Jah. Kasutajatele, mis 'AAD AV administraatorid' privaatsusrühma antakse DNS-i administraatori õigused, hallatava domeeni DNS-i kirjete muutmiseks. Need kasutajad saavad kasutada DNS-i halduri konsooli arvutis, kus töötab Windows Server liidetud hallatava domeeni DNS-i haldamine. DNS-i halduri konsooli kasutamiseks installida "DNS-i serveri tööriistad", mis on osa 'Remote Haldustööriistad' valikuline funktsioon serveris. Lisateavet [Utiliidid haldamise, jälgimine ja DNS-i tõrkeotsing](https://technet.microsoft.com/library/cc753579.aspx) on saadaval TechNetis.


### <a name="billing-and-availability"></a>Arveldus- ja kättesaadavus

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>On Azure AD domeeniteenused tasuline?
Jah. Lisateavet leiate teemast [hinnad lehe](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service"></a>Kas on olemas tasuta prooviversioon teenuse kohta?
See teenus on kaasatud tasuta prooviversioon Azure. Saate registreeruda [tasuta prooviversiooni ühe kuu Azure](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Kas saan Azure AD domeeniteenused osana ettevõtte mobiilsus komplekti (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Vaja Azure AD Premium Azure AD domeeni teenuste kasutamist?
Ei. Azure'i AD domeeniteenused on grupikindlustusleping Azure'i teenus on ja pole osa EMS. Azure'i AD domeeniteenused on saadaval kõigi Azure AD (tasuta, Basic ja, lisatasu) ja on arve tunnis sõltuvalt kasutus.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Mis Azure piirkonnad on teenus kättesaadav?
Vaadake [Azure'i teenuste regiooniti](https://azure.microsoft.com/regions/#services/) lehe leiate Azure'i regioonid, kus on saadaval Azure AD domeeniteenused loendit.
