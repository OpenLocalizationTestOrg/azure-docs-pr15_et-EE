<properties
   pageTitle="Loetelu rakenduse Azure Active Directory rakenduse Galerii"
   description="Kuidas rakendus, mis toetab ühekordse sisselogimise Azure Active Directory Galerii loendis | Microsoft Azure'i"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Loetelu rakenduse Azure Active Directory rakenduse Galerii

Rakendus, mis toetab ühekordse sisselogimise Azure Active Directory [Azure AD Galerii](https://azure.microsoft.com/marketplace/active-directory/all/)lisamiseks rakenduse esmalt peab rakendada üks järgmistest režiimidest integreerimine.

* **OpenID Connect** – otsese integreerimine Azure AD OpenID Connect kasutamise autentimis- ja Azure AD nõusolekut API konfiguratsiooni. Kui teil on hakanud just mõne integreerimine ja teie rakendus ei toeta SAML, siis on soovitada režiimi.

* **SAML** -rakenduse juba on võimalus konfigureerimine muude tootjate identiteedipakkujad SAML-protokolli abil.

Iga režiimi kirjet nõuded on allpool.

##<a name="openid-connect-integration"></a>OpenID ühenduse integreerimine

Kui soovite rakenduse integreerimine Azure AD, [arendaja juhiseid](active-directory-authentication-scenarios.md). Seejärel täitke alltoodud küsimusi ja saata waadpartners@microsoft.com.

* Mandaat testi rentniku või konto jaoks oma rakendusega, mida saab kasutada testimiseks integreerimine Azure AD meeskond.  

* Kuidas sisse logida ja ühenduse abil [Azure AD nõusoleku framework](active-directory-integrating-applications.md#overview-of-the-consent-framework)rakenduse eksemplari Azure AD Azure AD meeskonnatöö pakuvad juhiseid. 

* Sisestage nõutav Azure AD meeskond testimiseks ühekordse sisselogimise ja rakenduse mis tahes täiendavaid juhiseid. 

* Sisestage info allpool.

> Ettevõtte nimi:
> 
> Ettevõtte veebisaidi:
> 
> Rakenduse nimi:
> 
> Rakenduse kirjeldus (256 märki limiit):
> 
> Rakenduse veebileht (teatised):
> 
> Rakenduse tehnilise toe veebisaidil või kontaktteave.
> 
> Kliendi ID taotluse, nagu on näidatud https://manage.windowsazure.com rakenduse üksikasju:
> 
> Sign up URL-i klientide jaoks sihtkoht ja/või osta taotlus:
> 
> Valige rakenduse esitamiseks loendis (jaoks kättesaadavaks kategooriaid leiate Azure'i Active Directory turuplatsi) kuni kolm kategooriad.
> 
> Lisage rakenduse väikest ikooni (PNG-fail, 45px 45px ühtlase tausta värvi järgi):
> 
> Kinnitage rakenduste Suured ikoonid (PNG-fail, 215px 215px ühtlase tausta värvi järgi):
> 
> Lisage rakenduse Logo (PNG fail, 150px 122px läbipaistev tausta värvi järgi):

##<a name="saml-integration"></a>SAML integreerimine

Mis tahes rakendus, mis toetab SAML 2.0 saab integreerida otse Azure AD rentniku, mis [neid juhiseid, et lisada kohandatud rakenduse](active-directory-saas-custom-apps.md)abil. Kui olete oma rakenduste integreerimise töötab Azure AD testinud, saatke järgmist teavet <waadpartners@microsoft.com>.

* Mandaat testi rentniku või konto jaoks oma rakendusega, mida saab kasutada testimiseks integreerimine Azure AD meeskond.  

* Sisestage väärtused SAML sisselogimise URL-i, väljaandja URL (üksuse ID) ja vastus URL-i (argument tarbija teenus) rakenduse, nagu kirjeldatud [allpool](active-directory-saas-custom-apps.md). Kui annate need väärtused tavaliselt SAML metaandmete faili osana, siis saatke mis samuti.

* Esitage, kuidas konfigureerida Azure AD identiteedi pakkuja rakenduse abil SAML 2.0 Lühikirjeldus. Kui teie rakendus toetab konfigureerimine Azure AD nimega identiteedi pakkuja iseteenindusliku haldus portaali kaudu, siis veenduge, sisaldab mandaat, mis on toodud võimalus selle häälestanud.

* Sisestage info allpool.

> Ettevõtte nimi:
> 
> Ettevõtte veebisaidi:
> 
> Rakenduse nimi:
> 
> Rakenduse kirjeldus (256 märki limiit):
> 
> Rakenduse veebileht (teatised):
> 
> Rakenduse tehnilise toe veebisaidil või kontaktteave.
> 
> Sign up URL-i klientide jaoks sihtkoht ja/või osta taotlus:
> 
> Valige rakenduse esitamiseks loendis (jaoks kättesaadavaks kategooriaid leiate [Azure'i Active Directory turuplatsi](https://azure.microsoft.com/marketplace/active-directory/)) kuni kolm kategooriad).
> 
> Lisage rakenduse väikest ikooni (PNG-fail, 45px 45px ühtlase tausta värvi järgi):
> 
> Kinnitage rakenduste Suured ikoonid (PNG-fail, 215px 215px ühtlase tausta värvi järgi):
> 
> Lisage rakenduse Logo (PNG-fail, 150px 122px läbipaistva tausta värvi järgi):
