<properties
   pageTitle="Azure'i otsingu API versioone | Microsoft Azure'i | Otsingu API"
   description="Azure'i Search REST API-d ja kliendi teek .NET SDK versioon poliitika."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>Azure'i otsingus API versioonid

Azure'i otsingu koondab välja funktsioon värskenduste regulaarselt. Mõnikord, kuid mitte alati, värskendused vaja avaldada uue versiooni meie API tagasiühilduvuse säilitamiseks. Uue versiooni avaldamise võimaldab teil määrata, millal ja kuidas saate integreerida otsing teenusevärskendused koodi.

Tavaliselt, proovige avaldada uue versiooni ainult siis, kui see on vajalik, kuna see võib hõlmata mõned vaeva uuendada oma koodi kasutada API uus versioon. Me ainult avaldada uus versioon, kui peame muuta nii, et piirid tagasiühilduvuse API. See võib juhtuda, et parandusi olemasolevate funktsioonide või muuta olemasolevaid API pindala uusi funktsioone.

Järgime sama reegel SDK värskendusi. Azure'i otsingu SDK järgib [semantilise versioonimise](http://semver.org/) reeglid, mis tähendab, et selle versioon on kolm osa: suur, vaheversioon ja koostada arv (näiteks 1.1.0). Me anname ainult juhul, kui muudatusi leheküljepiiri tagasiühilduvuse SDK uus põhiversioon. Server funktsioon värskendusi me ei inkrementida teisese versiooni ja veaparandused me suurendab ainult ehitada versiooni.

##<a name="snapshot-of-current-versions"></a>Praeguse versiooni hetktõmmis 

Allpool on kõik kehtivate versioonidega hetktõmmise programmeerimise liidesed Azure'i otsimiseks. Tegevuskava ja muud üksikasjad leiate selle dokumendi järgmistes lõikudes.

Liidesed|Kõige värskem põhiversioon|Olek
----------|-------------------------|------
[.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|Üldiselt saadaval, välja veebruar 2016
[.NET SDK eelvaade](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|2.0 – eelvaade|Välja August 2016 eelvaade
[Teenuse REST API-ga](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015-02-28|Üldiselt kättesaadav
[Teenuse REST API eelvaade](search-api-2015-02-28-preview.md)|2015-02-28-eelvaade|Eelvaade
[Juhtimise REST API](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015-08-19|Üldiselt kättesaadav

Ülejäänud API-d, sh selle `api-version` iga on nõutav. See on lihtne suunata konkreetse versiooni, nt API eelvaade. Järgmises näites on näidatud, kuidas `api-version` parameeter on määratud:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Kuigi iga taotlus on mõni `api-version`, soovitame kasutada sama versioon kõik API taotlused. See kehtib eriti kui uus API versioonide sisse atribuudid või toiminguid, mis neid ei tuvasta varasemad versioonid. Segamine API versioone võib olla ootamatuid tagajärgi vältida.
> 
Teenuse REST API ja haldamine REST API on üksteisest sõltumatult versiooniga. Mis tahes sarnase versioonide numbrid on kaasautorluse juhuslike.

Üldiselt kättesaadav (või GA) API-d saab kasutada valmistamisel ja kehtivad Azure'i teenus teenusetaseme lepinguid. Eelvaate versioonid on katse funktsioone, mis ei viida alati GA versiooni. **Soovitame kasutada mängufilmide eelvaade API-d.**

##<a name="sdk-version-roadmap"></a>SDK versioon teejuht

Iga versioon .NET SDK suunatud konkreetse versiooni teenuse REST API-ga. Funktsioonid on võetud esmalt REST API-ga ja seejärel rakendada SDK.

.NET SDK on nüüd üldiselt kättesaadav ja töö käib juba järgmise versiooni. Järgmises tabelis paistab, et teil on aimu, milline on sünnipäev lähenemas edasi edasi tulevaste versioonide SDK.

.NET SDK versioon|REST API versioon|Funktsioonid|ETA
----------------|----------------|--------|---
1.1|2015-02-28|Lucene päringu süntaks|Veebruar 2016
2.0 – eelvaade|2015-02-28-eelvaade|Kohandatud analüsaatorid, Azure'i bloobimälu ja tabeli indexers, välja vastendused, ETags|August 2016
2.x|Uus GA API versioon|Sama, mis 2.0 – eelvaade|Ennetähtaegse 4 2016

##<a name="about-preview-and-generally-available-versions"></a>Eelvaate ja üldiselt kättesaadav versioonide kohta

Azure'i Otsi alati eelnevalt välja katse funktsioonid REST API kaudu esmalt seejärel .NET SDK väljalaske-eelse versiooni kaudu.

Eelvaate funktsioonid ei ole tagatud migreerida GA versioonis. Funktsioonid GA versiooni peetakse ühed ja tõenäoliselt välja arvatud väikesed tagasiühilduvad parandused ja täiustatud muutmine, eelvaade funktsioonid on saadaval testimiseks ja katsetamiseks, funktsioon kujundus ja rakendamise kohta tagasiside kogumise eesmärk. 

Siiski, kuna eelvaade funktsioonid võivad muutuda, soovitame suhtes, mis võtab sõltuvus eelvaade versioonide kood kirjutamine. Kui kasutate preview varasem versioon, soovitame üldiselt kättesaadav (GA) versiooni migreerimine. 

.NET SDK: kood migreerimise juhised leiate [.NET SDK täiendamine](search-dotnet-sdk-migration.md).

Üldiselt kättesaadav tähendab, et Azure'i otsing on nüüd jaotises taseme leping (SLA). SLA leiate [Azure'i otsingu taseme lepingud](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

