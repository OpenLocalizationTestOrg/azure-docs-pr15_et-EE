<properties
    pageTitle="Risk üritused avastada Azure Active Directory identiteedi kaitse | Microsoft Azure'i"
    description="See teema annab teile saadaval tüüpi risk sündmused üksikasjaliku ülevaate Azure Active Directory identiteedi kaitse"
    services="active-directory"
    keywords="Azure'i active directory identiteedi kaitse, pilveteenuse rakenduse discovery, rakendused, Turve, risk, taseme, haavatavuse, turbepoliitika haldamine"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="markvi"/>

#<a name="types-of-risk-events-detected-by-azure-active-directory-identity-protection"></a>Risk üritused avastada Azure Active Directory identiteedi kaitse 

Azure Active Directory identiteedi kaitse risk sündmused on sündmused mis:

- on lipuga märgitud kahtlaseks

- Saate määrata, et identiteedi on kahjustatud. 

See teema annab teile saadaval tüüpi risk sündmused üksikasjaliku ülevaate.


## <a name="leaked-credentials"></a>Lekkinud identimisteave

Lekkinud identimisteave on leitud, postitatud avalikult tume veebis Microsofti turvalisus teadlased. Neid mandaate on tavaliselt leitud lihttekstina. Need on kontrollitud Azure AD identimisteabe ja vaste korral esitatakse "Lekkinud mandaati" identiteedi kaitse.

Lekkinud identimisteabe risk sündmused on liigitatud "Suur" raskusaste risk sündmus, kuna need on kasutajanimi ja parool on saadaval ründaja selge märge.

## <a name="impossible-travel-to-atypical-locations"></a>Võimalik reisimise ebatüüpiliste asukohtadesse

Risk sündmuse tüüp tuvastab kaks sisselogimist pärinevate geograafiliselt kaugemal kohad, kus vähemalt ühe koha ka võib olla ebatüüpilised kasutaja, viimase käitumine antud. Lisaks kaks sisselogimist vahele jääv aeg on lühem on võtnud kasutaja liikuda esimese asukohast teise, mis näitab, et mõne teise kasutaja kasutab sama identimisteavet. 

Selle seadme õ algoritmi mis ignoreerib selge "*vale-positiivsed*" kaasa võimalik reisimise tingimusele, näiteks VPN ja kasutavad regulaarselt organisatsiooni teiste kasutajate asukohad.  Süsteem on algse õ tähtaeg 14 päeva jooksul õpib selle uue kasutaja sisselogimise käitumist.

Võimalik reisi on tavaliselt hea näitaja, et häkker suutis edukalt sisselogimine. Siiski vale-positiivsed võib tekkida, kui kasutaja on reisil abil uue seadme või VPN-i, mis tavaliselt kasutatakse organisatsiooni teiste kasutajate abil. Teine allikas vale-positiivsed on rakendusi, mis läbivad valesti serveri IP-d kliendi IP-d, mis võivad sisselogimist toimub, kui see rakendus on tagaandmebaas andmekeskuse ilmet majutatakse (sageli need Microsofti andmekeskuses, mis võivad ilme sisselogimist toimub Microsofti kuulub IP-aadressid). Need false positiivsed, on see risk sündmus taseme on "**Keskmine**".

##<a name="sign-ins-from-infected-devices"></a>Sisselogimist nakatunud seadmetes

Risk sündmuse tüüp tuvastab seadmete ründevara, kellel on teada, et aktiivselt suhelda robot serveri kaudu sisselogimist. Seda määrab tõestusmeetodid kasutaja seadme vastu IP-aadressid, mis olid kokku puutunud robot serveri IP-aadressid. 

Sündmuse risk tuvastab IP-aadressid pole kasutaja seadmetes. Kui mitmed seadmed on jäänud üks IP-aadress ja vaid mõned on kontrollib robot võrgus, muudest seadmetest sisselogimist minu päästik sündmuse asjatult, mis on põhjus liigitamiseks sündmuse risk nimega "**madal**".  

Soovitame teil pöörduge kasutaja ja kõik kasutaja seadmetes skannida. Samuti on võimalik, et kasutaja isiklik seade on nakatunud või nagu varem mainitud, kellegi teise kasutas kaudu sama IP-aadressi nakatunud seadmes kasutajana. Nakatunud seadmed on sageli nakatunud ründevara veel tuvastatud viirusetõrje tarkvara, mis võib viidata ka halb kasutaja tarbetu, mis võivad põhjustada seadme nakatuda nimega.

Aadress ründevara infektsioonide kohta lisateabe saamiseks vt [Ründevara kaitse keskele](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


## <a name="sign-ins-from-anonymous-ip-addresses"></a>Sisselogimist anonüümse IP-aadressid

Risk sündmuse tüüp tuvastab kasutajad, kes on edukalt sisse logitud, IP-aadress, mis on määratletud anonüümse puhverserveri IP-aadress. Inimesed, kellele soovite peita oma seadme IP-aadress ja võib kasutada pahatahtlikul eesmärgil kasutatakse neid puhvrid.

Soovitame kohe võtta ühendust kasutaja, kui nad kasutavad anonüümse IP-aadresside kinnitamiseks. Risk sündmuse tüüp soovitud taseme on "**Keskmine**" ise anonüümse IP pole rikkumise tugev märk.

## <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>IP-aadresside kahtlaste tegevuste sisselogimist

Risk sündmuse tüüp tuvastab IP-aadressid, kust suur nurjunud katsete arv on näha, üle mitme kasutajakonto, lühikese aja jooksul. See vastab ründajad kasutatavad IP-aadresside liikluse mustrite ja on tugev näitaja, et kontod on juba kas või kavatsete konfliktid. See on Õppekeskuse algoritmi, mis ignoreerib selge "*vale-positiivsed*", näiteks IP-aadressid, mida kasutavad regulaarselt organisatsiooni teiste kasutajate masina.  Süsteem on mõni algse õ 14 päeva jooksul, kus see õpib sisselogimise toimimist uus kasutaja ja uue rentniku.

Soovitame pöörduda kasutajal kinnitamine, kui nad tegelikult sisse logitud IP-aadress, mille märgiti kahtlaseks. Sündmuse tüüp soovitud taseme on "**Keskmine**", kuna mitme seadme võib olla sama IP-aadressi taha vaid mõne võib olla kahtlaste tegevuse eest vastutav. 


## <a name="sign-in-from-unfamiliar-locations"></a>Võõras asukohtadest sisselogimine

Risk sündmuse tüüp on reaalajas hindamise leiab viimase sisselogimise asukohad (IP-, laiuskraad / pikkuskraad ja ASN) määratlemiseks uute / võõras asukohad. Süsteem talletab teavet kasutada kasutaja eelmise asukohad ja leiab need "tuttavad" asukohad. Riski isegi käivitatakse ilmnemisel sisselogimise asukohast, mis ei ole juba tuttavad asukohtade loendit. Süsteem on algse õ tähtaeg 14 päeva jooksul, mil see lipuga mis tahes uutesse asukohtadesse nimega võõras asukohad. Süsteemi ignoreerib ka tuttavad seadmed ja asukohtadest geograafiliselt tuttavad asukoha lähedal asuvate sisselogimist. <br>
Võõras asukohad saate sisestada see tugev märk, et ründaja suudab proovite kasutada varastatud identiteeti. Vale-positiivsed võivad ilmneda juhul, kui kasutaja on reisil, proovida uue seadme või uue VPN kasutab. Sündmuse tüüp soovitud taseme on need false positiivsed "**Keskmine**".


## <a name="azure-ad-anomalous-activity-reports"></a>Azure'i AD Anomaalne tegevuse aruanded

Mõned need risk sündmused on saadaval Azure AD Anomaalne tegevuse aruannete Azure portaali kaudu. Järgmises tabelis on loetletud risk sündmuse erinevate ja vastavate **Azure AD Anomaalne tegevuse** aruanne. Microsoft on jätkuvalt investeerida ruumi ja lepingute pidevalt risk olemasolevate sündmuste tuvastamise täpsuse suurendamiseks ja lisada uue risk tüüpi jooksvalt. 



| Identiteedi kaitse Risk sündmuse tüüp | Vastavate Azure AD Anomaalne tegevuse aruanne |
| :-- | :-- |
| Lekkinud identimisteave    | Kasutajad, kellel lekkinud identimisteave |
| Võimalik reisimise ebatüüpiliste asukohtadesse | Korrapäratu tegevus |
| Sisselogimist nakatunud seadmetes    | Võimalik, et nakatunud seadmetest sisselogimist |
| Sisselogimist anonüümse IP-aadressid  | Sisselogimist tundmatutest allikatest |
| IP-aadresside kahtlaste tegevuste sisselogimist | IP-aadresside kahtlaste tegevuste sisselogimist |
| Võõras asukohtadest märgid    | - |
| Blokeeringu sündmused    | - |

Azure'i AD Anomaalne tegevuse järgmised aruanded hulka ei loeta risk sündmuste Azure AD identiteedi kaitse ja seega ei ole saadaval identiteedi kaitsmise kaudu. Need aruanded on veel saadaval Azure portaali, aga need on aegunud teatud ajal tulevikus nagu need on on asendatud risk sündmuste identiteedi kaitse.

- Pärast mitme vead sisselogimist
- Mitme kaugemad kaudu sisselogimist


## <a name="see-also"></a>Vt ka

- [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md)


