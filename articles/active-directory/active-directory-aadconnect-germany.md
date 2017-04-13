<properties
    pageTitle="Azure'i AD-ühenduse Microsofti pilveteenuse Saksamaa"
    description="Azure'i AD-ühendus on oma kohapealse kataloogide integreerimine Azure Active Directory. See võimaldab teil esitada ühise identiteedi Office 365, Azure ja SaaS Azure AD integreeritud rakenduste."
    keywords="Sissejuhatus: Azure'i AD-ühendus, ülevaade Azure'i AD-ühenduse, mis on Azure AD-ühenduse, installige active directory, Saksamaa, must mets"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure'i AD-ühenduse Microsofti pilveteenuse Saksamaa – avaliku eelvaade

## <a name="introduction"></a>Sissejuhatus
Azure'i AD-ühendus pakub oma kohapealse Active Directory ja Azure Active Directory sünkroonimise.
Praegu paljude stsenaariumid [Microsofti pilveteenuse](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) Saksamaa peab tegema tehtemärk. Kui kasutate Microsoft Cloud Saksamaa, peate olema teadlik järgmist:


- Järgmised URL-id tuleb avada puhverserveri sünkroonimiseks edukalt tekkida:
    - *. microsoftonline.de
    - *. windows.net
    - + Sertide Tühistusloendid

- Kui logite Azure AD kataloogi, kasutage konto onmicrosoft.de Domeen.
- Järgmised funktsioonid pole saadaval:
    - Azure'i AD-ühenduse seisund
    - Automaatvärskenduste
    - Parooli tagasikirjutusega

## <a name="download"></a>Laadi alla
Saate alla laadida Azure'i AD-ühenduse portaali keelest Azure'i AD-ühenduse.  Azure'i AD-ühenduse tera otsimiseks kasutage alltoodud juhiseid.

### <a name="the-azure-ad-connect-blade"></a>Azure'i AD-ühenduse Blade

Kui teil on Azure portaali sisse logitud, tehke järgmist.

1. Valige Sirvi
2.  Valige Azure Active Directory
3.  Valige Azure'i AD-ühenduse

Peaksite nägema järgmine:

![Azure'i AD-ühenduse Blade](media\active-directory-aadconnect-germany\germany1.png)

 
Järgmises tabelis kirjeldatakse funktsioone, mis on näidatud tera.


Pealkiri|Kirjeldus|
----- | ----- |
SÜNKROONIMISE OLEK|Vaatame teate, kas sünkroonimine on lubatud või keelatud.|
VIIMANE SÜNKROONIMINE|Viimane aeg eduka sünkroonimise lõpule.|
ÜHENDATUD DOMEENIDES|Kuvatakse ühendatud domeenides praegu konfigureeritud arv.|


## <a name="installation"></a>Installimine
Installige Azure'i AD-ühenduse, saate kasutada dokumentatsiooni [siin](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Täiustatud funktsioonid ja Lisateave
Lisateavet ja juhiseid kohandatud sätetega või täpsemate konfiguratsioone, alustage [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).  Sellelt lehelt leiate teavet ja täiendavad juhised linke.
