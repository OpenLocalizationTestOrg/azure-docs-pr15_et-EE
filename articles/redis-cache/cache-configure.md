<properties 
    pageTitle="Kuidas konfigureerida Azure Redis vahemälu | Microsoft Azure'i"
    description="Mõista vaikimisi Redis konfiguratsiooni Azure'i Redis vahemälu ja saate teada, kuidas konfigureerida oma Azure'i Redis vahemälu eksemplari"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Azure'i Redis vahemälu konfigureerimine

Selles teemas kirjeldatakse, kuidas läbi vaadata ja värskendada oma Azure'i Redis vahemälu eksemplari konfigureerimine ja hõlmab vaikimisi Redis serveri konfiguratsiooni Azure'i Redis vahemälu eksemplarid.

>[AZURE.NOTE] Konfigureerimise ja vahemälu lisafunktsioonidele kasutamise kohta lisateabe saamiseks leiate [konfigureerimine püsimine Premium Azure'i Redis vahemälu](cache-how-to-premium-persistence.md), [rühmitamise Premium Azure'i Redis vahemälu konfigureerimine](cache-how-to-premium-clustering.md)ja [virtuaalse võrgu tugi Premium Azure'i Redis vahemälu konfigureerimine](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Redis vahemälusätete konfigureerimine

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure'i Redis vahemälu pakub **sätted** enne järgmisi sätteid.

![Redis vahemälusätted](./media/cache-configure/redis-cache-settings.png)



-   [Tugi- ja sätete tõrkeotsing](#support-amp-troubleshooting-settings)
-   [Üldsätted](#general-settings)
    -   [Atribuudid](#properties)
    -   [Kiirklahvid](#access-keys)
    -   [Täpsemad sätted](#advanced-settings)
    -   [Redis vahemälu nõustaja](#redis-cache-advisor)
-   [Skaala sätteid](#scale-settings)
    -   [Esimese taseme hinnad](#pricing-tier)
    -   [Redis kobar suurus](#cluster-size)
-   [Andmete haldamise sätted](#data-management-settings)
    -   [Redis andmete püsimine](#redis-data-persistence)
    -   [Import/eksport](#importexport)
-   [Haldamise sätted](#administration-settings)
    -   [Taaskäivitage arvuti](#reboot)
    -   [Ajakava värskendused](#schedule-updates)
-   [Diagnostika sätete](#diagnostics-settings)
-   [Võrgu sätteid](#network-settings)
-   [Ressursside halduse sätted](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Tugi- ja sätete tõrkeotsing

Sätted jaotises **tugi + tõrkeotsingu** teile võimalust vahemälu seotud probleemide lahendamine.

![Tugi + tõrkeotsing](./media/cache-configure/redis-cache-support-troubleshooting.png)

Klõpsake **diagnoosimine ja lahendamine** esitatakse levinud probleemid ja strateegiaid neid lahendada.

Klõpsake nuppu **log tegevuste** vaatamiseks vahemälu tehtavad toimingud. Samuti saate filtreerimise laiendamiseks selle vaate lisada muud ressursid. Auditilogide töötamise kohta leiate lisateavet teemast [sündmuste vaatamine ja audit logid](../monitoring-and-diagnostics/insights-debugging-with-events.md) ja [auditi toimingud koos ressursihaldur](../resource-group-audit.md). Azure'i Redis vahemälu sündmuste jälgimise kohta leiate lisateavet teemast [toimingud ja teatised](cache-how-to-monitor.md#operations-and-alerts).

**Ressursi seisundi** jälgimine oma ressursside ja ütleb teile, kas see töötab ootuspäraselt. Azure'i ressursi seisund teenuse kohta leiate lisateavet teemast [Azure ressursside seisundi ülevaade](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Ressursi seisund ei saa praegu teatada seisundi kohta Azure Redis vahemälu eksemplarid, mis on majutatud virtuaalse võrku. Lisateavet leiate teemast [tehke kõik vahemälu funktsioonid töötavad, kui hosting vahemälu rakenduses on VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Klõpsake nuppu **Uus tugiteenuste taotlus** esitada tugiteenuse taotluse jaoks vahemälu avamiseks.

## <a name="general-settings"></a>Üldsätted

Sätted jaotises **üldist** võimaldavad juurdepääsu ja vahemälu järgmisi sätteid konfigureerida.

![Üldsätted](./media/cache-configure/redis-cache-general-settings.png)

-   [Atribuudid](#properties)
-   [Kiirklahvid](#access-keys)
-   [Täpsemad sätted](#advanced-settings)
-   [Redis vahemälu Advisor](#redis-cache-advisor)

### <a name="properties"></a>Atribuudid

Klõpsake nuppu **Atribuudid** saate vaadata teavet, sh vahemälu lõpp-punkti ja pordid vahemälu.

![Redis vahemälu atribuudid](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Kiirklahvid

Klõpsake **Pääsuklahvide** kuvamine või taastada kiirklahvide jaoks vahemälu. Neid klahve kasutatakse koos hosti nimi ja pordid keelest **Atribuudid** ühenduse vahemälu kliendid.

![Redis vahemälu kiirklahvid](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Täpsemad sätted

Järgmised sätted on enne **täpsemate sätete** konfigureerimine.

-   [Accessi pordid](#access-ports)
-   [Maxmemory-poliitika ja maxmemory reserveeritud](#maxmemory-policy-and-maxmemory-reserved)
-   [Keyspace teatised (Täpsemad sätted)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Accessi pordid

Vaikimisi on-SSL juurdepääs keelatud uue vahemälu. Pordi SSL nuppu **ei** **Luba juurdepääs ainult SSL-i kaudu** **blade Täpsemad sätted** ja klõpsake siis nuppu **Salvesta**.

![Redis vahemälu Accessi pordid](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Maxmemory-poliitika ja maxmemory reserveeritud

**Täpsemad sätted** enne **Maxmemory poliitika** ja **maxmemory reserveeritud** sätteid konfigureerida mälu poliitika vahemälu. **Maxmemory** Poliitikasätte konfigureerib väljatõstmine poliitika vahemälu ja **maxmemory reserveeritud** konfigureerib mälu-vahemälu protsesside jaoks.

![Redis vahemälu Maxmemory poliitika](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory poliitika** abil saate valida järgmised väljatõstmine poliitikad.

-   lenduvad-lru – see on vaikimisi.
-   allkeys-lru.
-   lenduvad juhuslik
-   allkeys juhusliku
-   lenduvad-ttl.
-   noeviction

Maxmemory poliitika kohta leiate lisateavet teemast [väljatõstmine poliitika](http://redis.io/topics/lru-cache#eviction-policies).

Sätte **maxmemory reserveeritud** konfigureerib mälu-vahemälu toiminguid nagu dispersioonanalüüs Tõrkesiirde ajal reserveeritud MB. Seda saab kasutada, kui teil on suur killustatus suhe. Väärtuse seadmist võimaldab teil on ühtsema Redis serveri kogemus, kui teie laadi muutub. Selle väärtuseks olema seatud kõrgema töökoormus, mis on raske kirjutada. Kui mälu on mõeldud selliste toimingute on vahemällu talletatud andmete jaoks saadaval.

>[AZURE.IMPORTANT] **Maxmemory reserveeritud** säte on saadaval Standard ainult ja Premium vahemälu.

### <a name="keyspace-notifications-advanced-settings"></a>Keyspace teatised (Täpsemad sätted)

Redis keyspace teatised on konfigureeritud enne **Täpsemad sätted** . Keyspace teatised luba klientide soovite saada teatisi, kui teatud sündmused.

![Redis vahemälu Täpsemad sätted](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Keyspace teatised ja **teavitamise keyspace sündmuste** säte on saadaval Standard ainult ja Premium vahemälu.

Lisateavet leiate teemast [Redis Keyspace teatised](http://redis.io/topics/notifications). Proovi kood, vt [Tere, maailm](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) valimis [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) faili.

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis vahemälu Advisor

**Soovitused** tera kuvab soovitused vahemälu. Ajal toiminguid, kuvatakse soovitusi ei. 

![Soovitused](./media/cache-configure/redis-cache-no-recommendations.png)

Kui tingimused tekivad vahemälu, nt kõrge mälukasutust, võrgu läbilaskevõime või serveri laadi toiminguid, kuvatakse vastav teade **Redis vahemälu** enne.

![Soovitused](./media/cache-configure/redis-cache-recommendations-alert.png)

Lisateavet leiate **soovitused** enne.

![Soovitused](./media/cache-configure/redis-cache-recommendations.png)

Saate jälgida nende mõõdikute **Redis vahemälu** tera jaotistes [jälgimis diagrammide](cache-how-to-monitor.md#monitoring-charts) ja [diagrammide kasutamine](cache-how-to-monitor.md#usage-charts) .

Iga hinnakirjad taseme on erinevad kliendi ühendusi, mälu ja läbilaskevõime piirangud. Kui vahemälu lähenedes maksimaalne maht nende mõõdikute püsiv aja jooksul, luuakse soovituse. Mõõdikute ja piirangud läbi vaadanud **soovitused** tööriista kohta lisateabe saamiseks vt järgmine tabel.

| Redis vahemälu meetermõõdustik      | Lisateabe saamiseks vaadake teemat                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Võrgu läbilaskevõime kasutuse | [Vahemälu jõudlust - läbilaskevõime](cache-faq.md#cache-performance) |
| Ühendatud kliendid       | [Vaikimisi Redis server configuration - maxclients](#maxclients)            |
| Serveri laadimine             | [Diagrammide kasutus - Redis serveri laadimine](cache-how-to-monitor.md#usage-charts)  |
| Mälukasutuse            | [Vahemälu jõudlus – suurus](cache-faq.md#cache-performance)                |

Vahemälu, klõpsake nuppu **nüüd** muuta [taseme hinnad](#pricing-tier) ja mastaapimiseks vahemälu. Hinnakirjad taseme valimise kohta leiate lisateavet teemast [mis Redis vahemälu pakkumise ja suurus peaks kasutama?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Skaala sätteid

Jaotise **skaala** sätteid võimaldavad juurdepääsu ja vahemälu järgmisi sätteid konfigureerida.

![Võrgus](./media/cache-configure/redis-cache-scale.png)

-   [Esimese taseme hinnad](#pricing-tier)
-   [Redis kobar suurus](#cluster-size)

### <a name="pricing-tier"></a>Esimese taseme hinnad

Klõpsake **hinnakirjad taseme** vaatamiseks või muutmiseks hinnakirjad taseme vahemälu jaoks. Skaleerimist kohta leiate lisateavet teemast [skaala Azure'i Redis vahemälu](cache-how-to-scale.md).

![Redis hinnad taseme vahemälu](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Redis kobar suurus

**(Eelvaade) Redis kobar maht** on käivitatud kobar suuruse muutmiseks klõpsake premium vahemälu abil rühmitamise lubatud.

>[AZURE.NOTE] Märkus ajal Azure Redis vahemälu Premium taseme väljaandmisest asemel üldiselt kättesaadav, Redis kobar suurus funktsioon on praegu eelvaade.

![Redis kobar suurus](./media/cache-configure/redis-cache-redis-cluster-size.png)

Kobar suuruse muutmiseks liuguriga või tippige arv vahemikus 1 kuni 10 **Kildu count** tekstiväljale ja salvestamiseks klõpsake nuppu **OK** .

>[AZURE.IMPORTANT] Redis rühmitamise on saadaval ainult Premium vahemälu. Lisateabe saamiseks vaadake, [Kuidas konfigureerida rühmitamise Premium Azure'i Redis vahemälu](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Andmete haldamise sätted

**Andmete haldamine** jaotises sätted võimaldavad juurdepääsu ja vahemälu järgmisi sätteid konfigureerida.

![Andmehaldus](./media/cache-configure/redis-cache-data-management.png)

-   [Redis andmete püsimine](#redis-data-persistence)
-   [Import/eksport](#importexport)

### <a name="redis-data-persistence"></a>Redis andmete püsimine

Klõpsake **andmete püsimine Redis** lubamine, keelamine või andmete püsimine premium vahemälu jaoks konfigureerida.

![Redis andmete püsimine](./media/cache-configure/redis-cache-persistence-settings.png)

Redis püsimine lubamiseks klõpsake **lubatud** lubamiseks RDB (Redis andmebaasi) varukoopia. Redis püsimine keelamiseks klõpsake nuppu **keelatud**.

Valige varukoopia intervalli konfigureerimiseks rippmenüü loendist **Varukoopia sagedus** . Valikute hulka **15 minutit**, **30 minutit**, **60 minutit**, **6 tunni**, **12 tundi**ja **24 tundi**. Ajavahemik algab loendamine, kui eelmine varukoopia toiming on edukalt lõpule jõudnud, ja kui see saabumisega algatatakse uus varukoopia.

Klõpsake **Salvestusruumi konto** valimiseks salvestusruumi konto, mida soovite kasutada, ja valige kas **primaarvõti** või **teisese võtme** **Salvestusruumi klahvi** ripploendist kasutada. Peate valima salvestusruumi konto sama piirkonna vahemälu ja **Premium salvestusruumi** konto pole soovitatav, kuna premium mälu on suurema läbilaskevõimega. Iga kord uuesti on salvestusruumi võti püsimine konto, peate valima soovitud klahvi uuesti **Salvestusruumi klahvi** ripploendist.

Klõpsake püsimine konfiguratsiooni salvestamiseks nuppu **OK** .

>[AZURE.IMPORTANT] Redis andmete püsimine kättesaadav ainult Premium vahemälu. Lisateabe saamiseks vaadake, [Kuidas konfigureerida püsimine Premium Azure'i Redis vahemälu](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Import/eksport

Import/eksport on Azure Redis vahemälu andmete haldamise toimingut, mis võimaldab teil Azure'i Redis vahemälu andmete importimine või andmete eksportimine Azure'i Redis vahemälu importimise ja eksportimise hetktõmmise Redis vahemälu andmebaasi (RDB) premium vahemälu lehe bloobimälu Azure'i salvestusruumi kontol. See võimaldab teil migreerimine erinevate Azure'i Redis vahemälu eksemplari vahel või vahemälu enne kasutamist andmetega asustada.

Impordi saab tuua Redis ühilduvad RDB failid pilve või keskkonnas, sh Linux, Windows või mis tahes pilveteenuse pakkuja nagu Amazon veebiteenuste ja teised Redis ühegi Redis serveriga. Andmete importimine on lihtne viis vahemälu luua juba eelnevalt täidetud andmetega. Importimise käigus Azure'i Redis vahemälu Azure storage RDB failid laaditakse memory ja lisab seejärel klahve vahemälu.

Ekspordi saate eksportida andmed salvestatakse Azure Redis vahemälu Redis RDB ühilduvad failid. Selle funktsiooni abil saate ühel Azure'i Redis vahemälu andmete teisaldamiseks või mõne muu Redis serveriga. Ajutiste failide luuakse VM eksportimise käigus mis majutab Azure'i Redis vahemälu serveri eksemplar ja faili üleslaadimisel kontole määratud salvestusruumi. Eksporditoimingu edu olek või tõrge on lõpule jõudnud, kustutatakse ajutine fail.

>[AZURE.IMPORTANT] Lisaks on saadaval Premium taseme vahemälu ainult impordi/ekspordi. Lisateavet ja juhiseid leiate teemast [Azure Redis vahemälu andmete importimine ja eksportimine](cache-how-to-import-export-data.md).


## <a name="administration-settings"></a>Haldamise sätted

Sätete **Administreerimine** võimaldavad järgmised haldustoimingute sooritamiseks vahemälu Premiumi jaoks. 

![Haldus](./media/cache-configure/redis-cache-administration.png)

-   [Taaskäivitage arvuti](#reboot)
-   [Ajakava värskendused](#schedule-updates)

>[AZURE.IMPORTANT] Selles jaotises sätted saadaval ainult Premium taseme vahemälu.

### <a name="reboot"></a>Taaskäivitage arvuti

**Taaskäivitage** tera võimaldab teil ühe või mitme sõlmed vahemälu taaskäivitage. See võimaldab teil testida rakenduse paindlikkust tõrke korral.

![Taaskäivitage arvuti](./media/cache-configure/redis-cache-reboot.png)

Kui teil on premium vahemälu rühmitamise lubatud, saate valida milliseid shards vahemälu taaskäivitamist.

![Taaskäivitage arvuti](./media/cache-configure/redis-cache-reboot-cluster.png)

Taaskäivitage üks või mitu sõlmed vahemälu, valige soovitud sõlmed ja klõpsake nuppu **taaskäivitage**. Kui teil on premium vahemälu rühmitamise lubatud, valige shard(s) taaskäivitage ja seejärel klõpsake nuppu **taaskäivitage**. Mõne minuti pärast uuesti võrgus valitud node(s) taaskäivitage arvuti ja on paar minutit hiljem.

>[AZURE.IMPORTANT] Taaskäivitage arvuti kättesaadav ainult Premium taseme vahemälu. Lisateavet ja juhiseid leiate teemast [Azure Redis vahemälu haldus - taaskäivitage](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Ajakava värskendused

**Ajakava värskenduste** tera võimaldab teil määrata hoolduse Redis serveri värskendusi vahemälu. 

>[AZURE.IMPORTANT] Pange tähele, et hooldustööde akna kehtib ainult Redis server värskendused ja pole mis tahes Azure'i värskenduse või värskendab vahemälu majutavad VMs operatsioonisüsteemi.

![Ajakava värskendused](./media/cache-configure/redis-schedule-updates.png)

Hoolduse määramiseks märkige soovitud päeva ja määrake hooldustööd aknas start tund iga päev ja klõpsake nuppu **OK**. Pange tähele, et hooldustööde akna aeg on UTC-vormingus. 

>[AZURE.IMPORTANT] Ajakava värskendused on saadaval Premium taseme vahemälu ainult. Lisateavet ja juhiseid leiate teemast [Azure Redis vahemälu haldus - ajakava värskendused](cache-administration.md#schedule-updates).

## <a name="diagnostics-settings"></a>Diagnostika sätete

Jaotises **diagnostika** võimaldab teil vahemälu Redis diagnostika konfigureerimine.

![Diagnostika](./media/cache-configure/redis-cache-diagnostics.png)

[Salvestusruumi konto konfigureerimine](cache-how-to-monitor.md#enable-cache-diagnostics) vahemälu diagnostika salvestamiseks klõpsake nuppu **diagnostika** .

![Redis vahemälu diagnostika](./media/cache-configure/redis-cache-diagnostics-settings.png)

Klõpsake **Redis mõõdikute** [vaadata mõõdikute](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) vahemälu ja **teatiste reeglite** [teatiste reegleid](cache-how-to-monitor.md#operations-and-alerts)luua.

Azure'i Redis vahemälu diagnostika kohta leiate lisateavet teemast [Azure Redis vahemälu jälgimine](cache-how-to-monitor.md).


## <a name="network-settings"></a>Võrgu sätteid

Sätete jaotises **võrku** võimaldavad juurdepääsu ja vahemälu järgmisi sätteid konfigureerida.

![Võrgus](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Virtuaalse võrgu sätteid saadaval ainult premium vahemälu konfigureeritud VNET toega vahemälu loomise ajal. Lisateavet loomine premium vahemälu VNET toetavad ja selle sätete värskendamine teada, [Kuidas konfigureerida virtuaalse võrgu kasutajatugi Premium Azure'i Redis vahemälu](cache-how-to-premium-vnet.md).

## <a name="resource-management-settings"></a>Ressursside halduse sätted

![Ressursside haldamine](./media/cache-configure/redis-cache-resource-management.png)

Jaotise **Sildid** abil saate korraldada oma ressursse. Lisateabe saamiseks vaadake teemat [kasutamine siltide korraldada oma Azure ressursse](../resource-group-using-tags.md).

Jaotise **lukud** abil saate lukustada tellimuse, ressursirühm või ressursside takistada teisi kasutajaid kogemata kustutamise või muutmine kriitiliste ressursid ettevõttes. Lisateabe saamiseks vt [Lock ressursse Azure'i ressursihaldur](../resource-group-lock-resources.md).

Jaotises **Kasutajad** toetab Rollipõhine juurdepääsu reguleerimine (RBAC) Azure'i portaalis aitab teie organisatsioonil vastata lihtsalt ja täpselt oma Accessi nõuetele. Lisateavet leiate teemast [Rollipõhine juurdepääsu reguleerimine Azure'i portaalis](../active-directory/role-based-access-control-configure.md).

Klõpsake **ekspordi malli** koostamine ja tulevaste juurutuste juurutatud ressursside malli eksportimiseks. Mallide töötamise kohta leiate lisateavet teemast [Deploy ressursid Azure'i ressursihaldur mallide abil](../resource-group-template-deploy.md).

## <a name="default-redis-server-configuration"></a>Vaikimisi Redis serveri konfigureerimine

Azure'i Redis vahemälu uued eksemplarid on konfigureeritud järgmised Redis konfiguratsioon vaikeväärtused.

>[AZURE.NOTE] Ei saa muuta selle jaotise sätete abil soovitud `StackExchange.Redis.IServer.ConfigSet` meetod. Kui seda meetodit nimetatakse ühe selles jaotises käsud, Expression.Error järgmine erand:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Mis tahes väärtused, mis on konfigureeritav **max-mälu-poliitika**, nagu on konfigureeritav Azure portaali või käsurea Haldusriistad nagu Azure'i CLI või PowerShelli kaudu.

|Säte|Vaikeväärtus|Kirjeldus|
|---|---|---|
|andmebaasid|16|Andmebaaside vaikenumbriks 16, kuid saate konfigureerida arvu põhjal hinnakirjad taseme. <sup>1</sup> on vaikimisi andmebaasi DB 0, saate valida mõne muu kohta ühenduse alus kasutamise kohta `connection.GetDatabase(dbid)` kus dbid on arv vahemikus `0` ja `databases - 1`.|
|MaxClients|Sõltub hinnakirjad taseme<sup>2</sup>|See on lubatud samal ajal seotud klientide maksimaalne arv. Kui jõudmisel suletakse Redis kõigi uute ühenduste saatmise tõrke kliendid jõudnud maksimaalne arv.|
|maxmemory-poliitika|lenduvad-lru|Kuidas Redis valige mida eemaldada maxmemory (pakub valitud vahemälu loomisel vahemälu suurus) jõudmisel Maxmemory poliitika on säte. Azure'i Redis vahemälu vaikesäte on lenduvad lru, mis eemaldab võtmed on aegumise määramine LRU algoritmi abil. See säte saab konfigureerida Azure portaalis. Lisateavet leiate teemast [Maxmemory-poliitika ja maxmemory reserveeritud](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory-näidised|3|LRU ja minimaalsete TTL algoritmid ei ole täpne algoritmide kohta, kuid ühtlustada algoritmide kohta (et säästa mälu), et võite valida ka kontrollida valimi suurus. Näiteks vaikimisi Redis kontrollib kolme klahvi ja valige seade, mida kasutati vähem viimati.|
|lua tähtaeg|5000|Max täitmisaeg Lua script millisekundites. Kui ülempiir täitmise ajal logige Redis, mis skripti on endiselt täitmise pärast suurim lubatud aeg ja hakkavad vastamine päringute viga.|
|lua – sündmuse-limiit|500|See on maksimaalne maht skripti sündmuste järjekorra.|
|pubsub kliendi väljundi-puhvri limiit normalclient väljundi-puhvri limiit|0 0 032mb 8mb 60|Kliendi väljundi puhvri piirangud saab jõustamine katkestamise klientide, mis on lugemise andmete piisavalt kiiresti serverist, mingil põhjusel (levinud põhjus on see, et Pub/Sub kliendi ei saa kasutada nii kiiresti kui avaldaja võivad põhjustada need sõnumid). Lisateavet leiate teemast [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a>
<sup>1</sup> Limiit `databases` on erinev iga Azure'i Redis vahemälu taseme hinnad ja võimalik seadistada vahemälu loomine. Kui pole `databases` säte on määratud vahemälu loomise ajal vaikimisi on 16.

-   Lihtne ja Standard vahemälu
    -   C0 (250 MB) vahemälu - kuni 16 andmebaasid
    -   C1 (1 GB) vahemälu - kuni 16 andmebaasid
    -   C2 (2,5 GB) vahemälu - kuni 16 andmebaasid
    -   C3 (6 GB) vahemälu - kuni 16 andmebaasid
    -   C4 (13 GB) vahemälu – kuni 32 andmebaasid
    -   C5 (26 GB) vahemälu – kuni 48 andmebaasid
    -   C6 (53 GB) vahemälu – kuni 64 andmebaasid
-   Premium vahemälu
    -   P1 (6 GB – 60 GB) - kuni 16 andmebaasid
    -   P2 (13 GB – 130 GB) – kuni 32 andmebaasid
    -   P3 (26 GB – 260 GB) – kuni 48 andmebaasid
    -   P4 (53 GB – 530 GB) – kuni 64 andmebaasid
    -   Kõiki premium vahemälu koos Redis kobar - lubatud Redis kobar toetab ainult andmebaasi 0 kasutamine nii `databases` piirata mis tahes premium vahemälu Redis kobar lubatud sisuliselt 1 ja [Valige](http://redis.io/commands/select) käsk pole lubatud. Lisateavet leiate teemast [vaja muuta oma klientrakenduse kasutamiseks, rühmitamise?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] Funktsiooni `databases` säte võib olla konfigureeritud ainult vahemälu loomise ajal ja ainult PowerShelli, CLI või teiste klientide haldamine. Näitena konfigureerimise `databases` PowerShelli kaudu vahemälu loomise ajal näha [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` on erinev iga Azure'i Redis vahemälu hinnad taseme.

-   Lihtne ja Standard vahemälu
    -   C0 (250 MB) vahemälu – kuni 256 ühendused
    -   C1 (1 GB) vahemälu – kuni 1000 ühendused
    -   C2 (2,5 GB) vahemälu – kuni 2000 ühendused
    -   C3 (6 GB) vahemälu – kuni 5000 ühendused
    -   C4 (13 GB) vahemälu – kuni 10 000 ühendused
    -   C5 (26 GB) vahemälu – kuni 15000 ühendused
    -   C6 (53 GB) vahemälu – kuni 20 000 ühendused
-   Premium vahemälu
    -   P1 (6 GB – 60 GB) – kuni 7500 ühendused
    -   P2 (13 GB – 130 GB) - kuni 15000 ühendused
    -   P3 (26 GB – 260 GB) – kuni 30 000 ühendused
    -   P4 (53 GB – 530 GB) – kuni 40 000 ühendused

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis ei toeta Azure Redis vahemälu käsud

>[AZURE.IMPORTANT] Kuna konfigureerimise ja haldamise Azure'i Redis vahemälu eksemplaride haldab Microsoft järgmised käsud on keelatud. Kui proovite kasutada neid kuvatakse tõrketeade, mis sarnaneb `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  CONFIG
>-  SILUMINE
>-  MIGREERIMINE
>-  SALVESTAMINE
>-  SULGEMINE
>-  SLAVEOF
>-  KLASTER - klaster kirjutamine käsud on keelatud, kuid kirjutuskaitstud kobar käsud on lubatud.

Redis käskude kohta leiate lisateavet teemast [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis konsooli

Oma Azure'i Redis vahemälu eksemplarides **Redis konsooli**, mis on saadaval Standard abil saate väljastada turvaliselt käsud ja Premium vahemälu.

>[AZURE.IMPORTANT] Redis konsooli ei tööta VNET, rühmitamise, ja andmebaaside, välja arvatud 0. 
>
>-  Ainult selle VNET kliendid pääsevad [VNET](cache-how-to-premium-vnet.md) – kui vahemälu on osa on VNET, vahemälu. Redis konsooli kasutab majutatud VMs, mis ei kuulu teie VNET redis-cli.exe klient, seda ei saa ühendust luua, kuna vahemälu.
>-  [Rühmitamist](cache-how-to-premium-clustering.md) - The Redis konsooli kasutab redis-cli.exe klient, mis ei toeta praegu rühmitamise. Redis-cli kasuliku [ebastabiilne](http://redis.io/download) kontoris Redis hoidla juures GitHub rakendatakse lihtsa toetab kui alustamine olevat `-c` aktiveerimine. Lisateabe saamiseks vt [võrdsed koos klaster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) [http://redis.io](http://redis.io) sisse [Redis kobar õpetuse](http://redis.io/topics/cluster-tutorial).
>-  Redis konsooli muudab andmebaasi 0 iga kord, kui saadate käsk Uus ühendus. Te ei saa kasutada funktsiooni `SELECT` käsk valige mõni muu andmebaas, kuna 0 iga käsuga lähtestatakse andmebaas. Lisateavet Redis käsud, sh muutmise mõni muu andmebaas, vt [Kuidas saab käitada Redis käsud?](cache-faq.md#how-can-i-run-redis-commands)

Redis konsooli juurdepääsemiseks klõpsake **konsooli** **Redis vahemälu** keelest.

![Redis konsooli](./media/cache-configure/redis-console-menu.png)

Anda käske vastu teie vahemälu eksemplari, lihtsalt tippige soovitud käsk konsooli.

![Konsooli redis.](./media/cache-configure/redis-console.png)

Redis käsud, mis on keelatud Azure'i Redis vahemälu loendi leiate jaotisest eelmise [Redis käsud ei toeta Azure Redis vahemälu](#redis-commands-not-supported-in-azure-redis-cache) . Redis käskude kohta leiate lisateavet teemast [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Vahemälu teisaldamine uude tellimusse

Vahemälu teisaldamiseks uus tellimus, klõpsates nuppu **Teisalda**.

![Teisalda Redis vahemälu](./media/cache-configure/redis-cache-move.png)

Liikumine ressursid ühte ressursirühma teise ja ühe tellimuselt teise kohta leiate teavet teemast [teisaldamine ressursid uue ressursirühma või tellimusele](../resource-group-move-resources.md).

## <a name="next-steps"></a>Järgmised sammud
-   Redis käsud töötamise kohta leiate lisateavet teemast [Kuidas saab käitada Redis käsud?](cache-faq.md#how-can-i-run-redis-commands).
