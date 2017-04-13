<properties 
    pageTitle="Sissejuhatus Azure'i Redis vahemälu Premium taseme | Microsoft Azure'i" 
    description="Saate teada, kuidas luua ja hallata Redis püsimine, Redis, rühmitamise ja VNET Premium taseme Azure'i Redis vahemälu eksemplaride tugi" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Azure'i Redis vahemälu Premium taseme tutvustus
Azure'i Redis vahemälu on jaotatud, hallatavate vahemälu, mis aitab teil luua väga scalable ja reageeri rakendusi, andes teie andmetele juurdepääsu ülikiire. 

Uue Premium taseme on ettevõtte valmis kiht, mis sisaldab kõiki Standard-astme funktsioone ja palju muud, nt parema jõudluse, suurem töökoormus, katastroofiabi, impordi/ekspordi ja täiustatud turvalisus. Lugege lisateavet Premium vahemälu taseme lisafunktsioone edasi.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Standard- või Basic taseme võrreldes parema jõudluse
**Parem jõudlust Standard või lihtsa taseme.** Vahemälu Premium astme on juurutatud riistvara, mis on kiirem protsessorit ja annab parema jõudluse võrreldes põhi-või tavalise taseme. Premium taseme vahemälu on suurem läbilaskevõime ja alumise latentsused. 

**Sama suurusega vahemälu läbilaskevõime on kiirem Premiumi Standard taseme.** Näiteks läbilaskevõime on 53 GB P4 (Premium) vahemälu on 250K taotlusi sekundis võrreldes 150 K C6 (standardne).

Suuruse, läbilaskevõime ja läbilaskevõime koos premium vahemälu kohta leiate lisateavet teemast [Azure Redis vahemälu KKK](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis andmete püsimine
Premium taseme võimaldab Azure Storage konto vahemälu andmed säilivad. Basic/Standard vahemälust kõik andmed on salvestatud ainult mälu. Aluseks oleva taristu korral võib probleeme seal andmekadu. Soovitame kasutada funktsiooni Redis andmete püsimine Premium astme suurendamiseks paindlikkust andmete kaotsimineku vastu. Azure'i Redis vahemälu pakub RDB ja Ofensiva # (tulekul) [Redis püsimine](http://redis.io/topics/persistence)võimalusi. 

Püsimine konfigureerimise juhised leiate teemast [püsimine Premium Azure'i Redis vahemälu jaoks konfigureerida](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Redis kobar
Kui soovite luua vahemälu, mis on suurem kui 53 GB või soovite Kildu andmete üle mitme Redis sõlmed, saate rühmitamise, mis on saadaval Premium taseme Redis. Iga sõlme koosneb primaarne/koopia vahemälu paari haldab Azure'i jaoks kõrge kättesaadavus. 

**Redis rühmitamise annab teile skaala ja läbilaskevõime.** Läbilaskevõime suurendab lineaarses shards (sõlmed) klaster arvu suurendamisel. Nt. Kui loote P4 kobar, 10 shards, on saadaval läbilaskevõime 250K * 10 = 2,5 miljonit taotlusi sekundis. Leiate [Azure'i Redis vahemälu KKK](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) kogumahtu, läbilaskevõime ja läbilaskevõime koos premium vahemälu kohta lisateabe saamiseks.

Alustada rühmitamise, vaadake, [Kuidas konfigureerida rühmitamise Premium Azure'i Redis vahemälu](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Tõhustatud turvalisus ja eraldamise

Avaliku Interneti-ühendusega arvutid juurdepääsetavat vahemälu loodud Basic või Standard astme. Juurdepääs vahemälu on piiratud juurdepääsu võti põhjal. Premium taseme abil saate tagada põhjalikumaks ainult kliendid määratud võrgustikus juurdepääs vahemälu. Saate juurutada Redis vahemälu [Azure virtuaalse võrgu (VNet)](https://azure.microsoft.com/services/virtual-network/). Saate kasutada kõiki VNet alamvõrku, juurdepääsu juhtimine poliitikad, nt funktsioone ja muud funktsioonid edasise juurdepääsu piiramiseks Redis.

Lisateabe saamiseks vaadake, [Kuidas konfigureerida virtuaalse võrgu tugi Premium Azure'i Redis vahemälu](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Import/eksport

Import/eksport on Azure Redis vahemälu andmete haldamise toimingut, mis võimaldab teil Azure'i Redis vahemälu andmete importimine või andmete eksportimine Azure'i Redis vahemälu importimise ja eksportimise hetktõmmise Redis vahemälu andmebaasi (RDB) premium vahemälu lehe bloobimälu Azure'i salvestusruumi kontol. See võimaldab teil migreerimine erinevate Azure'i Redis vahemälu eksemplari vahel või vahemälu enne kasutamist andmetega asustada.

Impordi saab tuua Redis ühilduvad RDB failid pilve või keskkonnas, sh Linux, Windows või mis tahes pilveteenuse pakkuja nagu Amazon veebiteenuste ja teised Redis ühegi Redis serveriga. Andmete importimine on lihtne viis vahemälu luua juba eelnevalt täidetud andmetega. Importimise käigus Azure'i Redis vahemälu Azure storage RDB failid laaditakse memory ja lisab seejärel klahve vahemälu.

Ekspordi saate eksportida andmed salvestatakse Azure Redis vahemälu Redis RDB ühilduvad failid. Selle funktsiooni abil saate ühel Azure'i Redis vahemälu andmete teisaldamiseks või mõne muu Redis serveriga. Eksportimise käigus ajutine fail on loodud VM Azure Redis vahemälu serveri eksemplari hostiva ja faili üleslaadimisel kontole määratud salvestusruumi. Eksporditoimingu edu olek või tõrge on lõpule jõudnud, kustutatakse ajutine fail.

Lisateabe saamiseks vaadake, [kuidas andmeid importida ja eksportida Azure'i Redis vahemälu andmeid](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Taaskäivitage arvuti

Premium taseme võimaldab teil taaskäivitage oma vahemälu nõudmisel ühe või mitme sõlmed. See võimaldab teil testida rakenduse paindlikkust tõrke korral. Saate taaskäivitage järgmised sõlmed.

-   Juhtslaidi sõlme vahemälu
-   Ori sõlme vahemälu
-   Nii põhi-kui ka ori sõlmed vahemälu
-   Koos rühmitamise premium vahemälu kasutamisel saate taaskäivitage juhteksemplari, ori või mõlemad sõlmed jaoks omaette shards vahemälus

Lisateavet leiate teemast [taaskäivitage](cache-administration.md#reboot) ja [Taaskäivitage KKK](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Ajakava värskendused

Ajastatud värskendused funktsioon võimaldab teil määrata hoolduse jaoks vahemälu. Hoolduse akna pole määratud, tehtud värskendusi Redis server ajal see aken. Hoolduse määrata, valige soovitud päeva ja määrake säilitamine akna alustamine tund iga päev. Pange tähele, et hooldustööd akna aeg on UTC-vormingus. 

Lisateavet leiate teemast [ajakava](cache-administration.md#schedule-updates) ja [ajakava värskendusi FAQ](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Ainult Redis serveri aken kavandatud hooldustööde käigus tehtud värskendused. Hoolduse akna ei kehti Azure värskenduste või VM operatsioonisüsteem värskendused.

## <a name="to-scale-to-the-premium-tier"></a>Et premium taseme skaala

Ulatuses premium taseme lihtsalt valida ühe premium astme **muutmine hinnad taseme** tera. Samuti saate skaala vahemälu premium taseme, kasutades PowerShelli ja CLI abil. Üksikasjalikud juhised leiate teemast [Kuidas skaala Azure'i Redis vahemälu](cache-how-to-scale.md) ja [Kuidas automatiseerimiseks skaleerimise toiming](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Järgmised sammud

Vahemälu luua ja uued premium taseme funktsioonid.

-   [Kuidas seadistada püsimine Premium Azure'i Redis vahemälu jaoks](cache-how-to-premium-persistence.md)
-   [Kuidas konfigureerida virtuaalse võrgu Premium Azure'i Redis vahemälu tugi](cache-how-to-premium-vnet.md)
-   [Kuidas seadistada rühmitamise Premium Azure'i Redis vahemälu jaoks](cache-how-to-premium-clustering.md)
-   [Kuidas andmete importimine ja eksportimine andmete Azure'i Redis vahemälu](cache-how-to-import-export-data.md)
-   [Kuidas hallata Azure Redis vahemälu](cache-administration.md)
  

