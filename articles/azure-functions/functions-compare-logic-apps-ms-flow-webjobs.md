<properties
    pageTitle="Valida meilivoo, loogika rakendused, funktsioonid ja WebJobs | Microsoft Azure'i"
    description="Võrdlus ja kontrasti soovitud Cloud integreerimine Microsofti teenuste ja otsustada, millised teenused, peaksite kasutama."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft meilivoo, meilivoo, loogika rakendused, azure funktsioone, funktsioonide azure webjobs, webjobs, sündmuse töötlemiseks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Valida meilivoo, loogika rakendused, funktsioonid ja WebJobs

Selles artiklis võrreldakse ja vastandatakse Microsoft pilves, mida saavad kõik integreerimine probleeme lahendada ja äriprotsesside automatiseerimine järgmistest teenustest:

- [Microsoft meilivoo](https://flow.microsoft.com/)
- [Azure'i loogika rakendused](https://azure.microsoft.com/services/logic-apps/)
- [Azure'i funktsioonid](https://azure.microsoft.com/services/functions/)
- [Azure'i rakendust Service WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Kõik need teenused on kasulik, kui "liimimise" koos eraldiseisvat süsteemi. Need kõik määratleda sisendi, toimingud, tingimuste ja väljundi. Saate kasutada iga ajakava või Käivita. Iga teenuse lisab kordumatu väärtus, ent võrreldes need pole küsimus "mis teenuse on kõige paremini?" peale ühe "mis teenuse kõige paremini vastava olukord?" Sageli kombinatsiooni need teenused on parim viis kiiresti luua esiletõstetud integreerimine scalable, täielik lahendus.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Meilivoo vs loogika rakendused

Saame arutada Microsoft Flow ja Azure loogika Apps koos kuna need on nii *konfiguratsiooni esimese* integreerimisteenuste, mis lihtsustab protsesside ja töövoogude abil koostada ning integreerida erinevaid SaaS ja ettevõtte rakendusi. 

- Voog on ehitatud loogika rakendused
- Neil on sama töövoo disaineri
- [Konnektorid](../connectors/apis-list.md) , mis töötavad ühes saate töötada ka teise

Puhul võimaldab teha lihtne integratsioon office töötaja (nt hankida SMS tähtsaid meile) ilma arendajate või seda. Teisalt, loogika rakendused saate lubada täpsema või olulise integratsioon (nt protsessid B2B) kui ettevõtte tasemel DevOps ja turve tavad on vaja. See on tüüpiline töövoos keerukuse ületunnitöö kasvada. Vastavalt sellele, saate alustada esmalt vool ja seejärel teisendage see loogika rakenduse vastavalt vajadusele.

Järgmise tabeli abil saate määrata, kas voole ega loogika rakendused on parim antud integreerimiseks.

|               | Voog                                                                             | Loogika rakendused                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Sihtrühma      | Office'i töötajad, ärikasutajad                                                   | IT-spetsialistidele arendajad                                                                                 |
| Stsenaariumid     | Iseteenindusliku                                                                     | Olulise                                                                                    |
| Kujundustööriist   | Brauseris, ainult kasutajaliides                                                              | Brauseri ja [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md), [koodi vaade](../app-service-logic/app-service-logic-author-definitions.md) saadaval |
| DevOps        | Erakorralist, arendamise valmistamisel                                                    | andmeallika juhtelemendi, testimine, tugi, ning automatiseerimine ja hallatavust [Azure'i ressursside](../app-service-logic/app-service-logic-arm-provision.md) haldamine|
| Administraator kogemus| [https://Flow.microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Turvalisus      | Standardse tavad: [andmete suveräänsuse](https://wikipedia.org/wiki/Technological_Sovereignty), [krüptimist ja ülejäänud](https://wikipedia.org/wiki/Data_at_rest#Encryption) tundlikku teavet jne. | Azure'i turvalisuse tagamine: [Azure'i turvalisus](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Turbekeskus](https://azure.microsoft.com/services/security-center/), [auditeerima logid](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)ja muud. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Funktsioonide vs WebJobs

Me arutada Azure'i funktsioonid ja Azure rakenduse teenuse WebJobs koos kuna need on nii *koodi esimese* integreerimisteenuste ja arendajatele mõeldud. Need võimaldavad teil käivitamiseks skripti või koodi osa erinevate sündmuste, nt [Uus salvestusruumi plekid](functions-bindings-storage.md) või [WebHook, mis on koosolekukutse](functions-bindings-http-webhook.md). Siin on nende sarnasusi. 

- Mõlemad on ehitatud [Azure'i rakendust Service](../app-service/app-service-value-prop-what-is.md) ja nautida funktsioone nagu [andmeallika juhtelemendi](../app-service-web/app-service-continuous-deployment.md), [autentimise](../app-service/app-service-authentication-overview.md)ja [jälgimine](../app-service-web/web-sites-monitor.md).
- Mõlemad on arendaja värskendustest teenused.
- Nii standard skriptimise ja programmeerimise keeled toega.
- Mõlemad on Nugeti ja NPM toetavad.

Funktsioonid on füüsiline areng WebJobs kulub WebJobs parimaid asju ja parandab need. Funktsiooni täiustused on järgmised. 

- Sujuv arendaja, testige ja käivitage koodi otse veebibrauseris.
- Sisseehitatud integreerimine Azure rohkem teenuseid ja 3-osaline teenuseid nagu [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Maksma-kasutada, pole vaja maksta [Rakenduse teenuse kavandamine](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
- Automaatne, [dünaamiline skaleerimist](functions-scale.md).
- Olemasoleva klientidele on rakenduse teenus, mis töötavad rakenduse teenusleping võimalik (jaotises kasutatud ressursid ära).
- Loogika rakenduste integreerimine.

Järgmises tabelis on kokkuvõte funktsioonid ja WebJobs erinevused:

|                        | Funktsioonid                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Mastaapimine                | Configurationless mastaapimist                                                                                                                                                | Rakenduse teenusleping skaala        |
| Hinnad                | Maksma-kasutada või osa sellest rakenduse teenusleping                                                                                                                                  | Rakenduse teenusleping osa           |
| Käivita-tüüpi               | vallandanud, kavandatud (timer päästik)                                                                                                                                  | käivitatud, pidev, ajastatud   |
| Päästik sündmused         | [ajastiteenuse](functions-bindings-timer.md), [Azure'i DocumentDB](functions-bindings-documentdb.md), [Azure sündmuse jaoturi](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, vaikne)](functions-bindings-http-webhook.md), [Azure'i rakenduse teenuse mobiilirakenduste](functions-bindings-mobile-apps.md), [Azure'i teatis jaoturi](functions-bindings-notification-hubs.md), [Azure'i teenus siini](functions-bindings-service-bus.md), [Azure Storage](articles/functions-bindings-storage.md) | [Azure'i salvestusruumi](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure'i teenus siini](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Klõpsake brauseri arengu | x                                                                                                                                                                        |                                    |
| Akna skriptimise       | katse                                                                                                                                                             | x                                  |
| PowerShelli             | katse                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Bash                   | katse                                                                                                                                                             | x                                  |
| PHP                    | katse                                                                                                                                                             | x                                  |
| Python                 | katse                                                                                                                                                             | x                                  |
| JavaScripti             | x                                                                                                                                                                        | x                                  |
| Java                   | katse                                                                                                                                                             | x                                  |

Kas kasutada funktsioone või WebJobs sõltub mida te juba teete rakenduse teenusega. Kui teil on rakenduse App teenuse jaoks, mille soovite käivitada Koodilõigud ja soovite hallata neid koos samas DevOps keskkonnas, kasutage WebJobs. Kui soovite käivitada Koodilõigud Azure'i teenuste või isegi 3. tootjate rakenduste korral, kui soovite hallata oma integreerimine Koodilõigud eraldi rakendust Service rakenduste või kui soovite kõne oma Koodilõigud loogika rakenduse kaudu, võtke ära kasutada kõiki parandusi funktsioone.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Voog, loogika rakenduste ja funktsioonide koos

Nagu eelnevalt mainitud, millised on kõige paremini teie sõltub olukorrast. 

- Antud business optimeerimine, siis kasutage kulgemist.
- Kui stsenaariumist integreerimine on liiga Täpsemad meilivoo jaoks või teil on vaja DevOps võimaluste ja turvalisuse nõuete, siis kasutage loogika rakendused.
- Kui juhises stsenaariumist integreerimine nõuab väga kohandatud transformatsioon või eriotstarbeline koodi, siis kirjutada funktsioon rakendus ning seejärel nimega toimingu loogika rakenduse käivitamine funktsiooni.

Loogika rakenduse saate vool sisse helistada. Funktsiooni loogika rakendus ja loogika rakenduse saate helistada ka funktsiooni. Parandab ületunnitöö jätkuvalt meilivoo, loogika rakenduste ja funktsioonide integreerimine. Saate luua midagi ühe teenuse ja muude teenustega kasutada. Seetõttu kõik need kolm tehnoloogiad tehtud investeering on kasulik.

## <a name="next-steps"></a>Järgmised sammud

Alustamine kõigi teenuste, luues oma esimese meilivoo, loogika rakendus, funktsioon rakenduse või WebJob. Klõpsake ühte järgmistest linkidest:

- [Microsoft Flow kasutamise alustamine](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Oma esimese Azure'i funktsioon loomine](../azure-functions/functions-create-first-azure-function.md)
- [Visual Studio abil WebJobs juurutamine](../app-service-web/websites-dotnet-deploy-webjobs.md)

Või lugege lisateavet nende integration services järgmised lingid.

- [Integreerimise stsenaariumid, Christopher Anderson Azure'i funktsioonid ja Azure rakenduse teenuse kasutamine](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Lihtne tehtud Charles Lamanna integratsioon](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Loogika rakenduste reaalajas veebiesitlus](http://aka.ms/logicappslive)
- [Microsoft Flow korduma kippuvad küsimused](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure'i WebJobs dokumentatsiooni ressursid](../app-service-web/websites-webjobs-resources.md)
