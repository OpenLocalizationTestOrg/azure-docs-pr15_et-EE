<properties
    pageTitle="Web Appsi ülevaade | Microsoft Azure'i"
    description="Siit saate teada, kuidas Azure'i rakendust Service aitab teil tekib ja host veebirakenduste"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Web Appsi ülevaade

*Rakenduse teenuse veebirakenduste* on täielikult hallatud Arvuta platvorm, mis on optimeeritud majutusteenuse veebilehed ja veebirakendused. Selle [platvormi-kui-a-service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) pakkumine Microsoft Azure'i võimaldab teil keskenduda oma äriloogika ajal Azure'i hoolitseb taristu mastaapimiseks rakenduste käivitamiseks ja andmete.

Järgmised 5-minutilises videos tutvustatakse rakenduse Azure'i teenus veebirakenduste.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Mis on web app rakenduse teenuses?

Rakenduse teenus, on või *veebirakenduse* Arvuta ressursse, mis Azure pakub hosting veebisait või rakenduse.  

Arvuta ressursside võib olla ühiskasutuses või spetsiaalne virtuaalmasinates (VM), olenevalt hinnakirjad taseme, mille valimisel. Rakenduse koodis töötab hallatavate VM, mis on eraldatud teiste klientide.

Teie kood võib olla mis tahes keeles või raamistiku, mis toetab [Azure'i rakendust Service](../app-service/app-service-value-prop-what-is.md), nt ASP.net-i, Node.js, Java, PHP või Python. Samuti saate käivitada skripte, mis [PowerShelli ja muude skriptimine keelte](web-sites-create-web-jobs.md#acceptablefiles) kasutamine web Appis.

Näiteid tüüpiline rakendus stsenaariumid, mida saate kasutada veebirakenduste jaoks, lugege [Web Appi stsenaariumid](https://azure.microsoft.com/documentation/scenarios/web-app/) ja **stsenaariumid ja soovitused** jaotises [Azure'i rakendust Service](choose-web-site-cloud-service-vm.md#scenarios), Virtuaalmasinates, teenuse struktuuri, ja pilveteenustega võrdlus.

## <a name="why-use-web-apps"></a>Miks kasutada veebirakenduste?

Siin on mõned põhijooned rakenduse teenuse veebirakenduste rakendada.

- **Mitme keele ja raamistiku** - rakenduse teenus on ASP.net-i, Node.js, Java, PHP ja Python esimese klassi tugi. Saate kasutada ka [PowerShelli ja muud skriptide või täitmisfailid](../app-service-web/web-sites-create-web-jobs.md) rakenduse teenuse VMs.

- **DevOps optimeerimine** - häälestada [pidev integratsioon ja juurutamine](../app-service-web/app-service-continuous-deployment.md) Visual Studio Team Services, GitHub või BitBucket. Saab reklaamida kaudu [testi ja lavastus keskkonnas](../app-service-web/web-sites-staged-publishing.md). Tehke [A ja B testimine](../app-service-web/app-service-web-test-in-production-get-start.md). Hallata oma rakendused rakendus teenuses [Azure PowerShelli](../powershell-install-configure.md) või [mitu platvormi käsurea liides (CLI)](../xplat-cli-install.md).

- **Global mastaapimiseks kõrge kättesaadavus** - [skaala [üles](../app-service-web/web-sites-scale.md) - või](../monitoring-and-diagnostics/insights-how-to-scale.md) automaatselt või käsitsi. Teie minirakendusi, mis tahes kohas Microsofti globaalne andmekeskuse taristu ja rakenduse teenuse [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) lubab kõrge kättesaadavus.

- **Ühenduste SaaS platvormide ja kohapealsed andmed** – rohkem kui 50 [konnektorid](../connectors/apis-list.md) ettevõtte süsteemid (nt SAP, Siebel ja Oracle'i), valige SaaS teenuste (nt Office 365 ja Salesforce'i) ja Interneti-teenuseid (nt Facebook ja Twitter). Accessi kohapealsed andmed [Hübriid ühendused](../biztalk-services/integration-hybrid-connection-overview.md) ja [Azure virtuaalse võrgu](../app-service-web/web-sites-integrate-with-vnet.md)abil.

- **Turve ja nõuetele vastavus** – rakenduse teenus on [ISO, SOC, ja PCI nõuetele](https://www.microsoft.com/TrustCenter/).

- **Mallid** - valida hulgaliselt Mallid [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/) , mis võimaldavad teil installida populaarsed avatud allikaga tarkvara nagu WordPress, Joomla ja Drupal viisardi abil.

- **Visual Studio integreerimine** – asjakohast tööriistad Visual Studio loomine ja juurutamine silumine töö sujuvamaks muutmine.

Lisaks web app saate ära funktsioonide pakutud [API rakendusi](../app-service-api/app-service-api-apps-why-best-platform.md) (nt CORS tugi) ja [Mobile'i rakenduste](../app-service-mobile/app-service-mobile-value-prop.md) (nt Tõuketeatiste). Rakenduse tüüpi rakenduse teenuse kohta leiate lisateavet teemast [Azure rakenduse teenuse ülevaade](../app-service/app-service-value-prop-what-is.md).

Peale veebirakendustes rakenduse teenuses Azure pakub muud teenused, mida saab kasutada hosting veebilehed ja veebirakendused. Enamikul veebirakenduste on parim valik.  Microservice arhitektuur, kaaluge [Teenuse struktuuri](https://azure.microsoft.com/documentation/services/service-fabric)ja kui teil on vaja rohkem kontrolli VMs, mis töötab teie kood, kaaluge [Azure'i Virtuaalmasinates](https://azure.microsoft.com/documentation/services/virtual-machines/). Lisateavet selle kohta, kuidas valida nende Azure'i teenuste leiate [Azure'i rakendust Service, Virtuaalmasinates, teenuse struktuuri, ja pilveteenustega võrdlus](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Alustamine

Alustuseks juurutamist proovi kood rakenduse teenuses uue veebirakenduse, järgige õpetuse [Deploy oma esimese veebirakenduse Azure 5 minutit](app-service-web-get-started.md) . Peate tasuta Azure'i konto.

Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.
