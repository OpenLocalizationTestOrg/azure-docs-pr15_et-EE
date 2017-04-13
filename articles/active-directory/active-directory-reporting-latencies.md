<properties
   pageTitle="Azure Active Directory aruandlus latentsused | Microsoft Azure'i"
   description="Aega kulub aruandlus sündmuste kuvamiseks oma Azure Active Directory 's"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-latencies"></a>Azure Active Directory aruande latentsused

*Need dokumendid on osa [Azure Active Directory aruandluse juhend](active-directory-reporting-guide.md).*

Aruanne                                                  | Miinimumväärtus  | Keskmine    | Maksimum
------------------------------------------------------- | -------- | ---------- | ----------
**Turve, aruanded**                                    |          |            |
Korrapäratu tegevus                              | 2 tunni  | 4 tundi    | 8 tundi
Sisselogimist tundmatutest allikatest                           | 2 tunni  | 4 tundi    | 8 tundi
Pärast mitme vead sisselogimist                        | 2 tunni  | 4 tundi    | 8 tundi
Mitme kaugemad kaudu sisselogimist                      | 2 tunni  | 4 tundi    | 8 tundi
IP-aadresside kahtlaste tegevuste sisselogimist     | 2 tunni  | 4 tundi    | 8 tundi
Võimalik, et nakatunud seadmetest sisselogimist                 | 2 tunni  | 4 tundi    | 8 tundi
Kasutajad, kellel Anomaalne tegevus                   | 2 tunni  | 4 tundi    | 8 tundi
Kasutajad, kellel lekkinud identimisteave                           | 2 tunni  | 4 tundi    | 8 tundi
**Rakenduse aruanded**                                 |          |            |
Konto ettevalmistamise tegevus                           | 2 tunni  | 4 tundi    | 8 tundi
Konto ettevalmistamise tõrked                             | 2 tunni  | 4 tundi    | 8 tundi
Rakenduse kasutamine                                       | 2 tunni  | 4 tundi    | 8 tundi
Parooli edastamiseks olek                                | 2 tunni  | 4 tundi    | 8 tundi
**Auditi- ja tegevuse aruanded**                            |          |            |
Auditilogi aruande                                            | 15 minutit | 15 minutit | 30 minutiga
Parooli lähtestamise tegevuse (Azure AD)                      | 2 tunni  | 4 tundi    | 8 tundi
Parooli lähtestamise tegevuse (Identity Manager)              | 2 tunni  | 4 tundi    | 8 tundi
Parooli lähtestamise registreerimise tegevuse (Azure AD)         | 2 tunni  | 4 tundi    | 8 tundi
Parooli lähtestamise registreerimise tegevuse (Identity Manager) | 2 tunni  | 4 tundi    | 8 tundi
Ise teenuse rühmade tegevuse (Azure AD)                 | 2 tunni  | 4 tundi    | 8 tundi
Ise teenuse rühmade tegevuse (Identity Manager)         | 2 tunni  | 4 tundi    | 8 tundi
**RMS-i aruanded**                                         |          |            |
Viimati aktiivne RMS-i kasutajad                                   | 2 tunni  | 4 tundi    | 8 tundi
RMS-i kasutamine                                               | 2 tunni  | 4 tundi    | 8 tundi
RMS-i seadme kasutus                                        | 2 tunni  | 4 tundi    | 8 tundi
RMS-i lubatud rakenduse kasutamine                           | 2 tunni  | 4 tundi    | 8 tundi
**Privaatne eelvaade aruanded**                             |          |            |
Kõik kasutaja sisselogimine tegevus                               | 2 tunni  | 4 tundi    | 8 tundi
