<properties
    pageTitle="Azure'i AD federation ühilduvuse loend"
    description="Selle lehel on mitte-Microsofti identiteedipakkujad, mida saab kasutada ühekordse sisselogimise rakendamiseks."
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
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure'i AD federation ühilduvuse loend
Azure Active Directory annab ühekordse sisselogimise kohta ja täiustatud rakendus Accessi turbes? Office 365 ja muude Microsoft Online services hübriid-ja vaid rakendusi nõudmata mis tahes mitte-Microsofti lahendus. Office 365, nagu enamik Microsoft Online services, on integreeritud Azure Active Directory kataloogiteenuste, autentimine ja autoriseerimine. Azure Active Directory pakub ühekordse sisselogimise tuhandetele SaaS rakenduste ja kohapealse veebirakendusi. Leiate Azure'i Active Directory rakenduse Galerii SaaS rakenduste.

Ettevõtted, mis on investeerida – Microsoft federation lahendusi, see teema sisaldab juhiseid, kasutades mitte-Microsofti identiteedipakkujad "Azure Active Directory federation ühilduvus"alltoodud loendist teenusega Microsoft Online services ühekordse sisselogimise oma Windows Server Active Directory kasutajate jaoks konfigureerida. 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford arvuti rühma](http://oxfordcomputergroup.com/)kolmanda osapoole, Microsoft, nimel katsetada need ühekordse sisselogimise kogemused mitte-Microsofti identiteedipakkujad kogumi kasutamise juhtudel levinud funktsiooniga Azure Active Directory.

Kuidas saab teie kolmanda osapoole identiteedipakkuja siin loetletud kohta lisateabe saamiseks pöörduge Oxford arvuti rühma [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Oxford arvuti rühma katsetada ainult need ühekordse sisselogimise stsenaariumid federation funktsionaalsust. Oxford arvuti rühma ei täitnud sünkroonimise, kahekordne autentimine, nende ühekordse sisselogimise stsenaariumid jne komponentide testimine.

>Logi sisse, alternatiivse UPN-i ID kasutamine ei testita ka selles programmis.



- [Azure Active Directory](#azure-active-directory)
- [Optimaalse IDM virtuaalse identiteedi Server Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli ühendatud identiteedi halduri 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ Access Manager 4.0.1](#netiq-access-manager-401) 
- [Access poliitika Manager suur-IP ver. 11.3 x-11.6 x IP suur](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [VMware tööruumi portaali versioon 2.1](#vmware-workspace-portal-version-21) 
- [Logige sisse ja minge 5.3](#signampgo-53) 
- [IceWall Federation versiooni 3.0](#icewall-federation-version-30) 
- [CA turvaline pilveteenuses](#ca-secure-cloud) 
- [Dell ühe identiteedi pilvepõhise juurdepääsu halduri v7.1](#dell-one-identity-cloud-access-manager-v71) 
- [AuthAnvil ühekordse sisselogimise 4.5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Kuna tegemist on kolmanda osapoole, Microsoft ei toeta juurutus, konfiguratsiooni, probleemide tõrkeotsinguks ja lahendamiseks, head tavad, jne probleemid ja nende identiteedipakkujad küsimusi. Tugiteenuse-ja nende identiteedipakkujad küsimusi, pöörduge otse toetatud tootjatele.

>Need kolmanda osapoole identiteedipakkujad olid testitud koostalitluse abil oli-liitmise ja oli-usalda ainult Protokollid Microsofti pilveteenustega. Testimine ei sisalda SAML-protokolli abil.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory saate kasutajate autentimiseks ühendamine oma kohapealse Active Directory või ilma kohapealse liiduserveri, parooli sünkroonimise abil. 

Stsenaarium tugi maatriksi selle sisselogimise kogemus on järgmine: 


| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|
|Tänapäevane rakendustes, mis kasutavad ADAL näiteks Office 2016| Toetatud|Ükski|

Azure Active Directory AD FS kasutamise kohta leiate lisateavet teemast [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Azure Active Directory abil parooli sünkroonimise kasutamise kohta leiate lisateavet teemast [Azure AD-ühenduse](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimaalse IDM virtuaalse identiteedi Server Federation Services 
Kasutajate, klientide kohapealse Active kataloogide paiknevaid saate autentimiseks optimaalse IDM virtuaalse identiteedi Server Federation Services.

Järgnevalt on seda stsenaariumi toetavad maatriksi see ühekordse sisselogimise kogemus:


| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Integreeritud Windowsi autentimine|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Jaoks poliitikat kliendi juurdepääsu kohta lisateabe saamiseks vt [piirata juurdepääsu Office 365 teenuste vastavalt kliendi asukohale.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 kasutab levinumate oli Federation identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:


| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| Näiteks Lync, Office'i tellimus, CRM RTF klientrakendused |  Toetatud |Mitte keegi (varasemates versioonides peab versioonile 6.11|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

PingFederate juhised selle kohta, kuidas konfigureerida see STS esitada ühekordse sisselogimise teenuse Active Directory kasutajatele, PDF-faili alla laadida [siin.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7.2 
PingFederate 7.2 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:


| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

PingFederate kohta, kuidas konfigureerida selle STS ühekordse sisselogimise teenuse Active Directory kasutajatele anda, leiate artiklist [siin.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:


| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

PingFederate kohta, kuidas konfigureerida selle STS ühekordse sisselogimise teenuse Active Directory kasutajatele anda, leiate artiklist [siin.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify aitab pakuvad ühendatud ühekordse sisselogimise teenuse Office 365 ilma majutusteenuse on kohapealse liiduserveri.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:


| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Kliendi juurdepääsu reguleerimine ei toetata 

Centrify kohta leiate lisateavet teemast [siin.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli ühendatud identiteedi halduri 6.2.2 
IBM Tivoli ühendatud identiteedi halduri 6.2.2 IBM turvalisus Accessi halduriga Microsoft rakenduste 1.4 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine: 

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

Lisateabe saamiseks IBM Tivoli ühendatud identiteedi halduri, leiate teemast [IBM turvalisus Access Manager for Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise teenuse ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine: 

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

SecureAuth kohta leiate lisateavet teemast [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 kumulatiivne väljaanne 4
CA SiteMinder Federation 12.52 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine: 

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

CA SiteMinder kohta leiate lisateavet teemast [CA SiteMinder Federation.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0 
RadiantOne pilve Federation teenuse (CFS) 3.0 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine: 

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Integreeritud Windowsi autentimine|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

RadiantOne CFS kohta leiate lisateavet teemast [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine: 


| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Integreeritud Windowsi autentimist nõuab täiendava veebiserver ja Okta rakenduste häälestamine.|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Integreeritud Windowsi autentimine|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

Okta kohta leiate lisateavet teemast [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin, sest mai 2014 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine: 

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Integreeritud Windowsi autentimine|
| Näiteks Lync, Office'i tellimus, CRM RTF klientrakendused |  Toetatud |Integreeritud Windowsi autentimine|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

OneLogin kohta leiate lisateavet teemast [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>NetIQ Access Manager 4.0.1 
NetIQ Access Manager 4.0.1 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |* Kerberose lepingute toetatud|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

* NetIQ toetama Kerberos-autentimist konfiguratsiooni Kerberos lepingu kaudu.  Selle konfiguratsiooni abi saamiseks võtke ühendust NetIQ või vaadata häälestamise juhend. NetIQ Access Manager kohta leiate lisateavet teemast [NetIQ Access Manager.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>Access poliitika Manager suur-IP ver. IP suur 11.3 x-11.6 x 
SUUR IP Access poliitika Manager (APM) IP – suured ver. 11.3 x – 11,6 x kasutab levinumate SAML identiteedi standardit ühekordse sisselogimise teenuse ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine: 

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Pole toetatud |Pole toetatud|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

SUUR-IP Access poliitika Manager kohta leiate lisateavet teemast [suur-IP Access poliitika Manager.](https://f5.com/products/modules/access-policy-manager) 

SUUR-IP Access poliitika Manager juhised selle kohta, kuidas konfigureerida see STS pakkuda ühekordse sisselogimise teenuse Active Directory kasutajad alla laadida PDF-i [siin.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>VMware tööruumi portaali versioon 2.1 
VMware tööruumi portaali versioon 2.1 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga | Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

Lisateavet VMware tööruumi portaali versioon 2.1 allalaadimine PDF-i [siin.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Logige sisse ja minge 5.3 
Logige ja avage 5.3 rakendab levinumate oli Federation/oli-usalda identiteedi standard on ühekordse sisselogimise ja atribuudi Exchange'i framework.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Kerberose lepingute toetatud |
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga | Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|


Sisselogimise ja Mine 5.3 toetab Kerberos-autentimine konfiguratsiooni Kerberos lepingu kaudu.  Selle konfiguratsiooni abi saamiseks võtke ühendust Ilex või vaadata häälestamise juhend [siin.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>IceWall Federation versiooni 3.0 
IceWall Federation versiooni 3.0 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga | Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

IceWall Federation kohta leiate lisateavet teemast [siin](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) ja [siin.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>CA turvaline pilveteenuses 

CA Secure Cloud kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente Exchange'i Web Access ja SharePoint Online | Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga | Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

CA Secure pilveteenuse kohta leiate lisateavet teemast [CA Secure pilve.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell ühe identiteedi pilvepõhise juurdepääsu halduri v7.1 
Dell ühe identiteedi pilvepõhise juurdepääsu halduri kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente veebipääs Exchange ja SharePoint Online | Toetatud |Ükski|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga |  Toetatud |Ükski|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|

Lisateabe saamiseks Dell ühe identiteedi pilvepõhise juurdepääsu halduri, leiate teemast [Dell ühe identiteedi pilvepõhise juurdepääsu halduri.](http://software.dell.com/products/cloud-access-manager)

 Ühekordse sisselogimise teenuse Office 365 kasutajatele anda selle STS konfigureerimise juhised vt [konfigureerimine Office 365 kasutajatele.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil ühekordse sisselogimise 4.5 
AuthAnvil ühekordse sisselogimise kohta 4.5 kasutab levinumate oli Federation/oli-usalda identiteedi standardit ühekordse sisselogimise ja atribuudi Exchange'i raames.

Stsenaarium tugi maatriksi selle ühekordse sisselogimise kogemus on järgmine:

| Kliendi |Tugi  |Erandid|
| --------- | --------- |--------- |
| Veebipõhise kliente Exchange'i Web Access ja SharePoint Online | Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-vormingus klientrakendustes nagu Lync, Office'i tellimus, CRM-iga | Toetatud |Integreeritud Windowsi autentimist ei toetata|
| RTF-e-posti kliendid, nagu Outlook ja ActiveSync |  Toetatud |Ükski|


Lisateavet leiate teemast [AuthAnvil ühekordse sisselogimise.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
