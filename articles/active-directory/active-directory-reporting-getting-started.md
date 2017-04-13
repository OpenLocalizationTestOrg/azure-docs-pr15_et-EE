<properties
   pageTitle="Azure Active Directory aruandlus: Alustamine | Microsoft Azure'i"
   description="Loendite mitmesuguste saadaolevate aruannete Azure Active Directory teatamine"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Alustamine: Azure Active Directory teatamine

## <a name="what-it-is"></a>Mis on

Azure Active Directory (Azure AD) sisaldab turvalisus, tegevuste ja auditilogi aruannete jaoks kataloogi. Siit leiate loendi aruandeid, mis sisaldab:

### <a name="security-reports"></a>Turve, aruanded

- Sisselogimist tundmatutest allikatest
- Pärast mitme vead sisselogimist
- Mitme kaugemad kaudu sisselogimist
- IP-aadresside kahtlaste tegevuste sisselogimist
- Korrapäratu tegevus
- Võimalik, et nakatunud seadmetest sisselogimist
- Kasutajad, kellel Anomaalne tegevus

### <a name="activity-reports"></a>Tegevuse aruanded

- Rakenduse kasutamine: Kokkuvõte
- Rakenduse kasutamine: üksikasjalikud
- Rakenduse armatuurlaud
- Konto ettevalmistamise tõrked
- Üksikkasutaja seadmed
- Üksikkasutaja tegevus
- Rühmade tegevuse aruanne
- Parooli lähtestamine registreerimise tegevuse aruanne
- Parooli lähtestamise tegevus

### <a name="audit-reports"></a>Auditilogi aruanded

- Directory auditilogi aruande

> [AZURE.TIP] Azure'i AD aruandlusteenuste veel dokumentatsioon, tutvuge [saate vaadata oma juurdepääsu- ja aruandeid](active-directory-view-access-usage-reports.md).



## <a name="how-it-works"></a>Kuidas see toimib


### <a name="reporting-pipeline"></a>Müügivõimaluste teatamine

Aruandlusteenuste müügivõimaluste koosneb kolm põhitoimingut. Iga kord, kui kasutaja või autentimise on tehtud, siis toimub järgmine:

- Esmalt kasutaja autenditakse (edukalt või edutult) ja tulemus on talletatud Azure Active Directory teenuse andmebaasid.
- Intervalliga Kõik viimatised Logi töödeldakse lisandmoodulid. Selles etapis meie turbe- ja anomaalsete tegevuse algoritmide otsivad Kõik viimatised Logi kahtlase tegevuse lisandmoodulid.
- Pärast töötlemist aruannete kirjutada, vahemällu, ja kätte Azure klassikaline portaalis.

### <a name="report-generation-times"></a>Aruande genereerimine korda

Suure mahu isikutuvastusi ja logida tõttu lisandmoodulid töödelda Azure AD platvormi, on kõige uuemad sisselogimist töödeldud on keskmiselt, üks tund vana. Harva, võib kuluda töödelda viimast sisselogimist kaheksa tundi.

Leiate selle viimase töödeldud sisselogimist uurides abi teksti iga aruande ülaosas.

![Teksti iga aruande ülaosas spikker](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Azure'i AD aruandlusteenuste veel dokumentatsioon, tutvuge [saate vaadata oma juurdepääsu- ja aruandeid](active-directory-view-access-usage-reports.md).



## <a name="getting-started"></a>Alustamine


### <a name="sign-into-the-azure-classic-portal"></a>Azure'i klassikaline portaali sisse logida

Esmalt peate [Azure klassikaline portaali](https://manage.windowsazure.com) globaalse või vastavuse administraator sisse logida. Teil peab olema ka Azure tellimuse teenuse administraator või koostöö administraator või kasutada "Juurdepääs Azure AD" Azure'i tellimus.

### <a name="navigate-to-reports"></a>Liikuge aruanneteni

Aruannete vaatamiseks liikuge kataloogi ülaosas vahekaarti aruanded.

Kui kuvate aruannete esimest korda, peate nõustuma dialoogiboksi aruannete vaatamiseks. See on veenduge, et see on aktsepteeritav, ettevõtte andmeid, mida võib mõnes riigis isiklikku teavet administraatoritele.

![Dialoogiboks](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Iga aruande uurimine

Otsige iga aruande kuvamiseks kogutud andmed ja töödeldud sisselogimist. Siit leiate [loendi kõigi aruannete siin](active-directory-reporting-guide.md).

![Kõik aruanded](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>CSV-vormingus aruannete allalaadimine

Iga aruande saab alla laadida CSV-failina (komaeraldusega). Saate need failid Excelis, PowerBI või muude tootjate analüüsi programmid veelgi andmete analüüsimine.

Aruande lisamine CSV-vormingus allalaadimiseks liikuge aruannet ja klõpsake nuppu "Laadi alla" allosas.

![Allalaadimise nupp](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Azure'i AD aruandlusteenuste veel dokumentatsioon, tutvuge [saate vaadata oma juurdepääsu- ja aruandeid](active-directory-view-access-usage-reports.md).





## <a name="next-steps"></a>Järgmised sammud

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Tegevuse Anomaalne Logi teatiste kohandamine

Liikuge "Konfigureerimine" vahekaarti kataloogi.

Liikuge kerides jaotisse "Teatised".

Lubada või keelata "E-posti Anomaalne sisselogimist teatised" jaotis.

![Teatised](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integreerimine Azure AD, aruannete API-ga

Vaadake teemat [Alustamine aruannete API-ga](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Mitmikautentimise tegeleda kasutajatele

Valige kasutaja aruande.

Ekraani allservas nuppu "Luba MFA".

![Mitmikautentimise ekraani allservas nuppu](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Azure'i AD aruandlusteenuste veel dokumentatsioon, tutvuge [saate vaadata oma juurdepääsu- ja aruandeid](active-directory-view-access-usage-reports.md).




## <a name="learn-more"></a>Lisateave


### <a name="audit-events"></a>Auditilogi sündmused

Siit saate teada, millised sündmuste kohta auditeerida Directory [Azure Active Directory aruandlus auditilogi sündmused](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>API integreerimine

Lugege teemat [Alustamine aruannete API-ga](active-directory-reporting-api-getting-started.md) ja [API dokumentides](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Suhtlemine

E-posti [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) tagasiside, abi või küsimusi, peate võib-olla.

> [AZURE.TIP] Azure'i AD aruandlusteenuste veel dokumentatsioon, tutvuge [saate vaadata oma juurdepääsu- ja aruandeid](active-directory-view-access-usage-reports.md).
