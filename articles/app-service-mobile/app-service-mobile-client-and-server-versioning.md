<properties
  pageTitle="Kliendi ja serveri SDK versioonimise Mobile'i rakendused ja teenused Mobile | Azure'i rakendust Service"
  description="Loendi kliendi SDK-d ja serveri SDK versioonidega Mobile teenuste ja Azure mobiilirakenduste kohta"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Kliendi ja serveri versioonimise Mobile'i rakendused ja teenused Mobile

Uusim versioon Azure Mobile teenused funktsioon **Mobiilirakenduste** Azure'i rakenduse teenuse.

Funktsiooni mobiilirakenduste kliendi ja serveri SDK-d põhinevad algselt nende teenuste Mobile, kuid need on *pole* omavahel ühilduvad.
Seega peate kasutama *Mobiilirakenduste* kliendi SDK *Mobiilirakenduste* serveriga SDK ja samamoodi *Mobile teenuste*jaoks. Käesolev leping rakendatakse läbi teisiti päise väärtus, mida kasutatakse kliendi ja serveri SDK-d, `ZUMO-API-VERSION`.

Märkus: iga kord, kui selle dokumendi viitab *Mobile teenuste* taustväärtus, seda ei pea tingimata olema majutatud Mobile teenuste. Nüüd on võimalik migreerida mobiilsideteenuse käivitamiseks rakenduse teenuse kood muutmata, kuid teenus endiselt olema *Mobile teenuste* SDK versioonide kasutamine.

Lisateavet migreerimine rakenduse teenuse kood muutmata, leiate artiklist [migreerimine mobiilsideteenuse, Azure'i rakenduse teenusega].

## <a name="header-specification"></a>Päise määratlus

Võti `ZUMO-API-VERSION` täpsustatud HTTP päis või päringu string. Vormi **x.y.z**versioon stringi väärtus.

Näiteks:

Https://service.azurewebsites.net/tables/TodoItem hankimine

PÄISED: ZUMO-API-VERSIOON: 2.0.0

POSTITUSE https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Loobumine versiooni kontrollimine

Saate loobuda kontrollimine, seades väärtuseks **true** seadmine **MS_SkipVersionCheck**rakenduse versioon. Määrake see oma web.config või Azure portaali jaotises rakenduse sätted.

> [AZURE.NOTE] On mitmeid käitumine muudatuste Mobile teenuste ja mobiilirakenduste eelkõige ühenduseta sünkroonimine, autentimise ja tõuketeatised vahel. Teil peaks ainult loobuda versiooni kontrollimine pärast täieliku testimine tagamaks, et neid muutusi käitumises leheküljepiiri oma rakenduse funktsioone.

## <a name="summary-of-compatibility-for-all-versions"></a>Kõik versioonid, ühilduvus Kokkuvõte

Allolevas tabelis ühilduvus kõik kliendi ja serveri vahel. Mõne taustväärtus on liigitatud mobiilsideseadmete **teenuste** või Mobile'i **rakendused** , mis põhineb server Tarkvaraarenduskomplektist, mis kasutab.

|                           | **Mobiilse teenused** Node.js või .net-i | **Mobiilirakenduste** Node.js või .net-i |
| ----------                | -----------------------             |   ----------------              |
| [Mobile teenuste kliendid] | Ok                                  | Tõrge\*                         |
| [Mobiilirakenduste kliendid]     | Tõrge\*                             | Ok                              |

\*See saab juhtida, määrates **MS_SkipVersionCheck**.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobile teenuste klient ja server

Järgmises tabelis kliendi SDK-d ühilduvad **Mobile teenused**.

Märkus: SDK-d *ei ole* Mobile Services kliendi saatmine päise väärtus `ZUMO-API-VERSION`. Kui teenus saab selle päise- või päringustringi väärtus, tagastatakse tõrge, kui te olete valinud selgesõnaliselt välja nagu eespool kirjeldatud.

### <a name="MobileServicesClients"></a>*Teenuste* mobiilikliendi SDK-d

| Kliendi platvormi                   | Versioon                                                                   | Versiooni päise väärtus |
| -------------------               | ------------------------                                                  | -------------------  |
| Klientide (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n/a                  |
| iOS-i                               | [2.2.2](http://aka.ms/gc6fex)                                             | n/a                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | n/a                  |
| HTML-VORMINGUS                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n/a     |

### <a name="mobile-services-server-sdks"></a>Mobiilse *Services* serveris SDK-d

| Protsessorid  | Versioon                                                                                                        | Aktsepteeritud versioon päis |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET-I             | [WindowsAzure.MobileServices.Backend.* versioon 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Pole versiooni päis** |
| Node.js          | (tulekul)                        | **Pole versiooni päis** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Mobile teenuste taustaprogrammid toimimist.

| ZUMO-API-VERSIOON | MS_SkipVersionCheck väärtus | Vastus |
| ---------------- | ---------------------------- | -------- |
| Pole määratud    | Mis tahes                          | 200 - OK |
| Mis tahes väärtus        | True                         | 200 - OK |
| Mis tahes väärtus        | FALSE/mitte määratud          | 400 - vigane päring |

## <a name="2.0.0"></a>Azure'i mobiilirakenduste klient ja server

### <a name="MobileAppsClients"></a>*Rakenduste* mobiilikliendi SDK-d

Versiooni kontrollimine võeti kasutusele, alustades **Azure'i**mobiilirakenduste kliendi SDK järgmisi versioone:

| Kliendi platvormi                   | Versioon                   | Versiooni päise väärtus |
| -------------------               | ------------------------  | -----------------    |
| Klientide (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS-i                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobiilne *Apps* serveri SDK-d

Versiooni kontrollimine sisaldub jälgimise serveri SDK versioone:

| Protsessorid  | SDK                                                                                                        | Aktsepteeritud versioon päis |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET-I             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [Azure'i-Mobile'i rakendused)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Mobiilirakenduste taustaprogrammid toimimist.

| ZUMO-API-VERSIOON | MS_SkipVersionCheck väärtus | Vastus |
| ---------------- | ---------------------------- | -------- |
| x.y.z või tühi    | True                         | 200 - OK |
| Null (Tühimärk)             | FALSE/mitte määratud          | 400 - vigane päring |
| 1.x.y            | FALSE/mitte määratud          | 400 - vigane päring |
| 2.0.0-2.x.y      | FALSE/mitte määratud          | 200 - OK |
| 3.0.0-3.x.y      | FALSE/mitte määratud          | 400 - vigane päring |


## <a name="next-steps"></a>Järgmised sammud

- [Migreerimine Azure'i rakendust Service mobiilsideteenuse ülevaade]


[Mobile teenuste kliendid]: #MobileServicesClients
[Mobiilirakenduste kliendid]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Migreerimine Azure'i rakendust Service mobiilsideteenuse ülevaade]: app-service-mobile-migrating-from-mobile-services.md

