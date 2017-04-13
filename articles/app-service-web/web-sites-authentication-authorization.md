<properties 
    pageTitle="Kohapealse Active Directory Azure rakenduse autentida | Microsoft Azure'i" 
    description="Siit saate teada, kohapealse Active Directory autentimiseks ärivaldkonna rakendused teenuses Azure rakenduse erinevate võimaluste kohta." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Kohapealse Active Directory Azure rakenduse autentida #

Selles artiklis kirjeldatakse, kuidas autentimiseks kohapealse [rakenduse](../app-service/app-service-value-prop-what-is.md)teenuses Azure Active Directory (AD). Azure'i rakendus on majutatud pilves, kuid on võimalused autentida kohapealse AD kasutajad turvaliselt. 

## <a name="authenticate-through-azure-active-directory"></a>Azure Active Directory kaudu autentida
Azure Active Directory rentnikku võib olla directory sünkroonitud asutusesisese AD. Seda moodust võimaldab AD kasutajatel juurdepääs rakenduse Interneti kaudu ja autentida kohapealse mandaadi abil. Lisaks leiate Azure'i rakendust Service [võtmed lahendus seda meetodit](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Nupu vaid mõne hiireklõpsuga saate lubada autentimise rentnikuga directory sünkroonitud Azure rakenduse. Seda moodust on järgmised eelised:

-   Mis tahes autentimise koodi rakenduse ei vaja. Lubage rakenduse teenuse autentimist saate teha ja oma aega funktsioonid rakenduse kohta.
-   [Azure'i AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) võimaldab juurdepääsu directory andmete Azure rakenduste.
-   [Azure Active Directory ei toeta kõik rakendused](/marketplace/active-directory/), sealhulgas Office 365, Dynamics CRM Online'i, Microsoft Intune'i ja mitte-Microsofti pilveteenuste rakenduste tuhandeliste pakub SSO-d. 
-   Azure Active Directory toetab Rollipõhine juurdepääsu reguleerimine. Saate [Authorize(Roles="X")] mustri minimaalsete muudatustega oma koodi.

Kuidas kirjutada ärivaldkonna Azure rakendus, mis autendib Azure Active Directory, leiate artiklist [ärivaldkonna Azure rakenduse Azure Active Directory autentimise loomine](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Autentida kaudu on kohapealse STS
Kui teil on kohapealse turvaline Turbeloa teenuse (STS) nt Active Directory Federation Services (AD FS), saate mis liita oma Azure rakenduse autentimist. See lähenemine on parim, kui ettevõtte poliitika keelab salvestatakse Azure AD andmed. Siiski, võtke arvesse järgmist.

-   STS topoloogia peab olema juurutatud asutusesisese, koos maksumuse ja haldus pea kohal.
-   Ainult AD FS-i administraatorid saavad konfigureerida [tuginedes tootja usalduste ja nõude reeglid](http://technet.microsoft.com/library/dd807108.aspx), mis võivad piirata arendaja suvandid. Teisalt, on võimalik haldamine ja kohandamine [nõuete](http://technet.microsoft.com/library/ee913571.aspx) kohta rakenduse alusel.
-   Accessi kohapealse AD andmete nõuab eraldi lahenduse ettevõtte tulemüüri kaudu.

Kuidas kirjutada ärivaldkonna Azure rakendus, mis kontrollib koos mõne kohapealse STS, leiate artiklist [ärivaldkonna Azure rakenduse AD FS-i autentimise loomine](web-sites-dotnet-lob-application-adfs.md).
 
