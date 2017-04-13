<properties 
   pageTitle="Azure'i SQL-andmebaasi KKK" 
   description="Vastused levinud küsimustele kliendid küsida cloud andmebaasid ja Azure'i SQL-andmebaasi, Microsofti relatsiooniandmebaasist halduse süsteem (RDBMS) ja andmebaasi teenuse pilves." 
   services="sql-database" 
   documentationCenter="" 
   authors="CarlRabeler" 
   manager="jhubbard" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>SQL-andmebaasi KKK

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Kuidas see SQL-andmebaasi kasutamist kuvatakse arve? 
SQL-andmebaasi arved prognoositavad kord tunnis määra põhjal teenuse kiht + jõudlusega ühe andmebaasid või eDTUs elastne andmebaasi rakenduskausta kohta. Tegelik kasutus arvutada ja proportsionaalse tunni, nii, et arve, võidakse kuvada murdude tundi. Kui andmebaas on olemas 12 tundi kuus, kuvatakse arve 0,5 päeva kasutamist. Lisaks teenuse astme + jõudlusega ja eDTUs pool kohta on katki bill hõlbustamiseks saate kasutada iga ühe kuu andmebaasi päevade arvu kuvamiseks.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Mida teha, kui ühe andmebaasi on aktiivne vähem kui tund või vähem kui tund teenuse jõudmine kasutab?
Iga tunni andmebaasi on olemas, kasutades kõrgeim teenuse kiht + jõudlusega, mis rakendatakse see tund, olenemata kasutus või kas andmebaasi ilmnes aktiivne vähem kui tund on arve. Kui loote ühe andmebaasi ja kustutage see viis minutit hiljem kajastab arve eest andmebaasi üks tund. 

Näited
    
- Kui lihtsa andmebaasi loomine ja seejärel kohe täiendada seda Standard S1, on tasu Standard S1 määra esimene tund.

- Kui täiendate andmebaasi lihtsa Premium kell 10:00. ja versioonitäienduse lõpulejõudmist kell 1:35 järgmisel päeval teilt tasu alates 01:00:00 Premium määr 

- Kui vahetate andmebaasi Premium Basic 11.00 veebisaidil ja see on lõpule jõudnud kell 2:15, siis andmebaas on lisandu Premium määra kuni 3:00:00, pärast mida ei lisandu lihtsa määrade.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Kuidas see elastne andmebaasi kasutus kuvatakse arve ja mis juhtub, kui muuta eDTUs pool kohta?
Elastne andmebaasi rakenduskausta kulude kuvataks teie arvele, nagu elastne DTUs (eDTUs) kerimine, kuvatakse jaotises eDTUs rakenduskausta [hinnakirjad](https://azure.microsoft.com/pricing/details/sql-database/)lehel kohta. On ühe andmebaasi tasuta jaoks elastne andmebaasi kaustu. Iga tund on olemas kõrgeim eDTU, olenemata kasutus- või kas pool oli aktiivne vähem kui tund on arve. 

Näited

- Kui loote tavalise elastne andmebaasi rakenduskausta 200 eDTUs kell 11:18, lisamise viis andmebaasid ning ostmisega 200 eDTUs terve tunni alguses 11.00. ülejäänud päeva.
- 2 päeval kell 5:05, andmebaasi 1 algab tarbimine 50 eDTUs ja kindlat päeva kaudu. Andmebaaside 2 – 5 müügitehingu vahemikus 0 kuni 80 eDTUs muutuda. Päeva jooksul, saate lisada viis muid andmebaase, mis kasutavad erineva eDTUs kogu päeva. 2 päeva on kogu päeva eest tuleb tasuda 200 eDTU juures. 
- Päeval 3, kell 5. Saate lisada mõne muu 15 andmebaasid. Andmebaasi kasutus suureneb kogu päeva punkt, kus te otsustate suurendamiseks mõeldud pool 200 400 eDTUs kell 8:05 Kulude tasemel 200 eDTU oli tegelikult 08: 00 kuni ja suurendab 400 eDTUs ülejäänud neli tundi. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Kuidas ei kasuta, aktiivse Geo-kopeerimine mõnda elastne andmebaasi pargis kuvataks arve?
Erinevalt ühe andmebaase, ei [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md) abil elastne andmebaasid on otsene arvelduse mõju.  Ainult ostmisega eDTUs, ette valmistatud iga kaustu (esmane pool ja teisel pool)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Kuidas Auditeerimispoliitika funktsiooni kasutamist mõju minu bill? 
Auditi on SQL-andmebaasi teenuse ilma täiendava sisse ehitatud maksumus ja Basic, Standard, ja andmebaasid saadaval. Auditilogide talletamiseks vastavalt Auditeerimispoliitika funktsiooni kasutab Azure Storage konto ja tabelite ning järjekorrad Azure Storage rakendada oma auditilogi suurus.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Kuidas otsida taseme- ja jõudluse õige teenusetaseme ühe andmebaasid ja elastne andmebaasi kaustu? 
Mõned tööriistad on saadaval. 

- Kohapealse andmebaaside, soovitame andmebaasid ja nõutav DTUs [DTU sizing advisor](http://dtucalculator.azurewebsites.net/) abil ja hinnata mitu andmebaaside jaoks elastne andmebaasi kaustu.
- Kui pargis on kasu ühe andmebaasi, Azure nutikad engine soovitab elastne andmebaasi kohapeal on, kui ta näeb ajalooliste kasutamine mudelit, mis kinnitab, et see. Vt [jälgimine ja haldamine on elastne andmebaasi rakenduskausta Azure'i portaalis](sql-database-elastic-pool-manage-portal.md). Arvutustehted enda kohta lisateavet [hinna ja jõudluse kaalutluste kohta kohapeal on elastne andmebaas](sql-database-elastic-pool-guidance.md)
- Kas te peate valima ühe andmebaasi üles või alla, vaadake [jõudluse juhiseid ühe andmebaaside jaoks](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Kui sageli taseme või jõudluse teenusetaseme ühe andmebaasi muuta? 
V12 andmebaasidega, saate muuta teenuse kiht (vahel Basic, Standard ja Premium) või jõudlusega teenuse kiht (nt S1 kuni S2) sees nii tihti, kui soovite. Vanemate versioonide andmebaaside, saate muuta taseme või jõudluse teenusetaseme kokku neli korda 24 tunni jooksul.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Kui sageli reguleerida eDTUs pool kohta? 
Nii tihti, kui soovite.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Kui kaua võtab muuta ühe andmebaasi teenuse taseme või jõudluse või andmebaasi teisaldamine ja sealt elastne andmebaasi kohapeal on? 
Teenuse kiht andmebaasi muutmine ja teisaldamine ja sealt on nõuab andmebaasi platvormi tausta toiminguga kopeerida. Teenuse kiht muutmine võib kuluda paar minutit mitu tundi andmebaasid suurusest. Mõlemal juhul jäävad andmebaasid võrgus ja saadaval ajal liikuda. Ühe andmebaaside vahetamise kohta leiate teavet artiklist [Muuda teenuse kiht andmebaasi](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Millal kasutada ühe andmebaasi vs elastne andmebaase? 
Üldiselt elastne andmebaasi kaustu on mõeldud tüüpilised [tarkvara-kui-a-service (SaaS) muster](sql-database-design-patterns-multi-tenancy-saas-applications.md), kus on üks andmebaasi klientide või rentniku kohta. Ostmist üksikud andmebaasid ja vastavad muutuja ja vajadusel iga andmebaasi maksimumi overprovisioning on sageli kulu tõhusa. Kaustu, saate hallata küsitlustulemuste pool jõudlus ja andmebaaside mastaapimiseks automaatselt üles ja alla. 

Azure nutikad engine soovitab rakenduskausta, andmebaaside jaoks, kui kasutamine mudelit saab seda. Lisateavet leiate teemast [SQL-andmebaasi hinnad taseme soovitusi](sql-database-service-tier-advisor.md). Üksikasjalikud juhised selle kohta, valides ühe ja elastne andmebaasi vahel, vaadake [hind ja jõudluse kaalutluste kohta elastne andmebaasi kaustu](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Mida tähendab on kuni 200% salvestusruumi maksimaalne ettevalmistatud andmebaasi varukoopia Storage? 
Varukoopia salvestusruumi on seostatud automatiseeritud andmebaasi varukoopiate salvestamiseks kasutatud jaoks [punkti-sisse-kellaaja-taastamine](sql-database-recovery-using-backups.md#-point-in-time-restore) ja [Geo-taastamine](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure'i SQL-andmebaasi pakub kuni 200% maksimaalne ettevalmistatud andmebaasi varukoopia salvestusruumi täiendava tasuta salvestusruumi. Näiteks, kui teil on standardne DB eksemplari ettevalmistatud DB suurusega 250 GB, on olemas varukoopia salvestusruumi lisatasu 500 GB. Kui teie andmebaasi ületab pakutavaid varukoopia talletamist, saate vähendada säilitusperiood Azure'i klienditoega või maksta arve veebisaidil standardmäär lugemisõigus – geograafiliselt liigsete salvestusruumi (RA GRS) lisatasu varukoopia hoidmiseks. RA-GRS arveldamine kohta leiate lisateavet teemast salvestusruumi hinnad üksikasjad.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Ma olen liikudes Web/äri uus teenus astme, mida pean teadma?
Nüüd on kasutuselt kõrvaldatud Azure SQL-i veebi ja andmebaasid. Basic, Standard, Premium ja elastne astme asendada pensionile veebi ja andmebaasid. Oleme täiendavad FAQ, mis aitavad selle ülemineku aja jooksul. [Web ja Business Edition ajalise piirangu KKK](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Mis on oodatud dispersioonanalüüs lag, kui geo imitatsiooniga andmebaasi kaks sama Azure geograafia piirkondade vahel?  
Me praegu on viis sekundit on RPO ja dispersioonanalüüs olnud alla, et kui geo-teisene majutab on Azure soovitatav andmepunktipaaride piirkond ja sama teenuse kiht.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Mis on ka oodatud dispersioonanalüüs lag geo-teisene piirkonna esmane andmebaasi loomisel?  
Empiiriliste andmete põhjal pole liiga palju vahe siseseid piirkonna- ja muu piirkonna dispersioonanalüüs lag on Azure soovitatav kasutatakse andmepunktipaaride piirkond. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Kui kaks piirkondade vahel on võrgu tõrke, proovige uuesti loogika tööpõhimõtted kui Geo-Dispersioonanalüüs on häälestatud  
Kui on lahti, saame uuesti iga 10 sekundi järel taastada ühendused.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Mida teha selleks, et tagada, et kriitilised muutmine esmane andmebaasis on kopeeritud?
Geo-teisene on mõni asünkroonse koopia ja me proovite täieliku sünkroonimise, mille esmane alles jätta. Kuid pakume meetodi sünkroonimise alustamiseks tagada dispersioonanalüüs kriitilised muutub (nt parooli värskendused). Jõustatud sünkroonimise mõjutab jõudluse, sest see blokeerib helistaja jutulõnga seni, kuni kõik sooritatud toimingud ei kopeeritud. Lisateavet leiate teemast [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Mis tööriistad on saadaval dispersioonanalüüs lag esmane andmebaas ja geo-teisene jälgida?
Me jätke reaalajas dispersioonanalüüs lag esmane andmebaas ja geo-teisene kaudu ka DMV vahel. Lisateavet leiate teemast [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).
