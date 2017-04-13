<properties
    pageTitle="Azure'i rakendust Service web, mobile ja API rakendused | Microsoft Azure'i"
    description="Siit saate teada, kuidas Azure'i rakendust Service abil saate töötada, juurutada ja hallata web ja mobiilirakendused."
    keywords="rakenduse teenus, Azure'i rakendust service, rakenduse teenus maksab, skaala scalable, rakendusejuurutuse, azure rakendusejuurutuse, paas, platvormi-kui-a-service, veebisaidi, veebisaidi, veebi Azure'i mobiilsideseadmete"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Mis on Azure rakenduse teenust?

*Rakenduse teenus* on [platvormi-kui-a-service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) pakub Microsoft Azure. Web ja mis tahes platvormi või seadme mobiilirakenduste loomine. Rakenduste integreerimine SaaS lahendusi, kohapealse rakendustega ühendamine ja oma äriprotsesside automatiseerimine. Azure'i töötab teie rakendused täielikult hallatud virtuaalmasinates (VM) oma valiku ühiskasutusega VM ressursid ja sihtotstarbeline VMs.

Rakenduse teenus sisaldab web ja mobiilne võimalusi, mida me varem esitatud eraldi Azure veebisaitide ja Azure Mobile teenused. See hõlmab ka uued võimalused äriprotsesside automatiseerimine ja majutuse cloud API-d. Integreeritud teenusel rakenduse teenus võimaldab teil koostada erinevate osade - veebisaidid, mobiilirakenduse tagasi lõpetatakse, rahulik API-d ja äriprotsesside - sisse ühe lahenduse.

Järgmised 4-minutilises videos antakse lühiülevaade, kuidas rakenduse teenus, mis on seotud varasemate Azure pakkumisi ja mis on uut rakenduses seda.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Miks kasutada rakendust Service?

Siin on mõned olulisi funktsioone ja võimalusi rakenduse teenuse.

- **Mitme keele ja raamistiku** - rakenduse teenus on ASP.net-i, Node.js, Java, PHP ja Python esimese klassi tugi. Samuti saate kasutada [Windows PowerShelli ja muud skriptide või täitmisfailid](../app-service-web/web-sites-create-web-jobs.md) rakenduse teenuse VMs.

- **DevOps optimeerimine** - häälestada [pidev integratsioon ja juurutamine](../app-service-web/app-service-continuous-deployment.md) Visual Studio Team Services, GitHub või BitBucket. Saab reklaamida kaudu [testi ja lavastus keskkonnas](../app-service-web/web-sites-staged-publishing.md). Tehke [A ja B testimine](../app-service-web/app-service-web-test-in-production-get-start.md). Hallata oma rakendused rakendus teenuses [Azure PowerShelli](../powershell-install-configure.md) või [mitu platvormi käsurea liides (CLI)](../xplat-cli-install.md).

- **Global mastaapimiseks kõrge kättesaadavus** - [skaala [üles](../app-service-web/web-sites-scale.md) - või](../monitoring-and-diagnostics/insights-how-to-scale.md) automaatselt või käsitsi. Teie minirakendusi, mis tahes kohas Microsofti globaalne andmekeskuse taristu ja rakenduse teenuse [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) lubab kõrge kättesaadavus.

- **Ühenduste SaaS platvormide ja kohapealsed andmed** – rohkem kui 50 [konnektorid](../connectors/apis-list.md) ettevõtte süsteemid (nt SAP, Siebel ja Oracle), valige SaaS teenuste (nt Office 365 ja Salesforce'i) ja Interneti-teenuseid (nt Facebook ja Twitter). Accessi kohapealsed andmed [Hübriid ühendused](../biztalk-services/integration-hybrid-connection-overview.md) ja [Azure virtuaalse võrgu](../app-service-web/web-sites-integrate-with-vnet.md)abil.

- **Turve ja nõuetele vastavus** – rakenduse teenus on [ISO, SOC, ja PCI nõuetele](https://www.microsoft.com/TrustCenter/).

- **Mallid** - valida hulgaliselt Mallid [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/) , mis võimaldavad teil installida populaarsed avatud allikaga tarkvara nagu WordPress, Joomla ja Drupal viisardi abil.

- **Visual Studio integreerimine** – asjakohast tööriistad Visual Studio loomine ja juurutamine silumine töö sujuvamaks muutmine.

## <a name="app-types-in-app-service"></a>Rakenduse tüüpi rakenduse teenus

Rakenduse teenus pakub mitme *rakenduse tüübid*, mis on mõeldud majutada mõnda kindlat liiki töökoormus:

- [**Veebirakenduste**](../app-service-web/app-service-web-overview.md) - hosting veebilehed ja veebirakendused.

- [**Mobiilirakenduste**](../app-service-mobile/app-service-mobile-value-prop.md) Tagasi hosting mobiilirakenduse lõpetatakse.

- [**Rakenduste API**](../app-service-api/app-service-api-apps-why-best-platform.md) - hosting rahulik API.

- [**Loogika rakendused**](../app-service-logic/app-service-logic-what-are-logic-apps.md) – äriprotsesside automatiseerimine ja ja andmete integreerimisse koodi kirjutamata pilved üle.

Wordi *rakenduse* siin viitab majutusteenuse ressursse, mis töötab on töökoormus. Tegemise "web app" näidisena, olete harjunud ilmselt mõelda web appi Arvuta ressursid ja rakenduse koodi, mis koos pakkuda funktsionaalsust brauseris. Kuid rakenduse teenuse või *veebirakenduse* ressursside Arvuta hosting rakenduse koodis leiate Azure'i. Kui teie taotlus on koostatud Web ees- ja rahulik API tagasi end, võib juurutamist nii web appi või teie ees kood juurutamiseks võib web app ja rakenduse API oma tagaandmebaas koodi. Rakenduse võib koosneda mitut rakendust Service rakenduste erinevaid.

## <a name="app-service-plans"></a>Rakenduse teenuse lepingud

[Rakenduse teenuse lepingud](azure-web-sites-web-hosting-plans-in-depth-overview.md) täheregister Arvuta ressursse, mis töötavad teie rakendused. Kui plaanite liht-liikluse laadimise, saate kasutada ühiskasutusega VMs (**tasuta** ja **ühiskasutuses** hinnad astme). Suurema laadimise, saate valida mitme suuruse sihtotstarbeline vms (**tavaline**, **Standard**ja **Premium** astme). Mitme rakenduse rakendused saate ühiskasutusse anda sama lepingu ja need skaala üles ja alla hinnakirjad astme koos lepingus.

Kui teil on vaja rohkem skaleeritavus ja võrgu eraldamise, käivitada teie rakendused [Rakendus teenuse keskkonnas](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Hinnad

Kui palju maksab rakenduse teenuse kohta leiate teemast [Rakenduse teenuse hinnad](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Rakenduse teenuse katsetamiseks

[Loomine valimi web appi, mobiilirakenduse, või loogika rakenduse](http://go.microsoft.com/fwlink/?LinkId=523751) ja mängida seda 1 tund, pole krediitkaardiga nõutav, kohustusi, pole hilisemaid probleeme.

[Tasuta Azure'i konto](https://azure.microsoft.com/pricing/free-trial/)avamise või proovige ühte meie töötamise alustamine õpetused:

* [Õpetus: Loo web app](../app-service-web/app-service-web-get-started.md)
* [Õpetus: Loo mobiilirakenduse kaudu](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Õpetus: API rakenduse loomine](../app-service-api/app-service-api-dotnet-get-started.md)
* [Õpetus: Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)
