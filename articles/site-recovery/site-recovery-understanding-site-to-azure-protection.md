<properties
    pageTitle="Hyper-V kopeerimine Azure saidi taastamine | Microsoft Azure'i"
    description="Selles artiklis abil saate ülevaate tehnilise põhimõtet, mis aitavad teil edukalt installimine, konfigureerimine ja haldamine Azure saidi taastamine."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>Hyper-V kopeerimine Azure'i saidi taastamine

Selles artiklis kirjeldatakse tehnilise põhimõtet, mis aitavad teil edukalt konfigureerimine ja haldamine Hyper-V saidi või System Center virtuaalse masina Manager (VMM) saidi Azure kaitse Azure saidi taastamise abil.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Andmeallika keskkond, mis on kohapealse ja Azure kopeerimise seadistamine

Kohapealse ja Azure avariitaastet häälestamise käigus olla kindel, et alla laadida ja installida Azure taastamise pakkujalt VMM server. Saate installida Azure taastamise Services Agent iga Hyper-V host.

![VMM saidi juurutamise kopeerimise kohapealse ja Azure vahel](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Hyper-V hallatavate taristu allika keskkonna häälestamiseks on sarnane VMM hallatavate taristu allika keskkonna häälestamiseks. Ainus erinevus on pakkuja ja agent installitud Hyper-V hosti ise. Keskkonnas VMM VMM serverisse installitud pakkuja ja agent on installitud Hyper-V hosts (korral dispersioonanalüüs Azure).

## <a name="workflows"></a>Töövood

### <a name="enable-protection"></a>Luba kaitse
Pärast virtuaalse masina kaitsmine Azure portaali või kohapealse, käivitab saidi taastamise töö nimega **lubamiseks** . Saate jälgida selle all **klõpsake vahekaarti** .

![Tööde loendi tööd "Luba kaitse"](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

**Luba kaitse** töö kontrollib eeltingimused enne [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) meetodit kasutada. See meetod loob dispersioonanalüüs Azure sisendina, mis on konfigureeritud ajal kaitse abil.

**Luba kaitse** töö algab algse dispersioonanalüüs kohapealse dokumentidega [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) meetod. See meetod saadab Azure virtuaalse masina virtual ketast.

![Töö "Luba kaitse" üksikasjad](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Kaitse virtual arvutisse viimistlemine
[Hyper-V VM hetktõmmise](https://technet.microsoft.com/library/dd560637.aspx) võetakse algse dispersioonanalüüs käivitamisel. Virtuaalse kõvaketta on töödeldud ükshaaval seni, kuni kõik ketast üles Azure. See tavalisel viisil võtab aega lõpetamiseks ketta mahtu ja selle läbilaskevõime põhjal. Optimeerida oma võrgu kasutamine, vaadake, [Kuidas hallata Azure kaitse võrgu läbilaskevõime kasutuse abil kohapealse](https://support.microsoft.com/kb/3056159).

Kui algset dispersioonanalüüs lõpule jõudnud, konfigureerib **Finalize kaitse virtual arvutisse** töö võrk ja pärast dispersioonanalüüs sätteid. Kuigi algse kopeerimine on pooleli.

- Kõikide muudatuste soovitud ketast jälgitakse. 
- Vabastage kettal salvestusruumi tarbib hetktõmmise ja Hyper-V koopia Log (HRL) faile.

Algne dispersioonanalüüs lõpetamisel, kustutatakse hetktõmmise Hyper-V VM. See kustutamise tulemuseks ühendamise andmete muutmine pärast algse dispersioonanalüüs ema-kettale.

![Tööde loendi tööd "Viimistlemine kaitse virtual arvutisse"](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Delta dispersioonanalüüs
Hyper-V koopia Dispersioonanalüüs jälgimine, mis on osa Hyper-V koopia kopeerimise mootor Hyper-V koopia Log (*.hrl) faile virtuaalse kõvaketta rajad soovitud muudatused. HRL failid on seostatud ketast samas kataloogis.

Iga ketas, mis on konfigureeritud lubama dispersioonanalüüs on seostatud HRL fail. See logi saadetakse salvestusruumi kliendikonto kui algse kopeerimine on lõpule jõudnud. Kui Logi on Azure teel, jälgitakse esmast muudatused mõne muu faili samas kaustas.

Ajal algsete kopeerimine või delta dispersioonanalüüs, saate jälgida VM dispersioonanalüüs seisund vaates VM mainitud [kuvari dispersioonanalüüs seisund virtuaalse masina jaoks](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine).  

### <a name="resynchronization"></a>Resynchronization
Virtuaalse masina on märgitud resynchronization jaoks, kui mõlemad delta kopeerimine nurjub esialgse täieliku kopeerimine on kulukas võrgu läbilaskevõime või kellaaeg. Näiteks kui HRL failimaht vaiad kuni 50 protsenti kokku ketta mahtu, virtuaalse masina on märgitud resynchronization jaoks. Resynchronization minimeeritakse andmehulga võrgu kaudu saadetud arvutuste kontrollimisel lähte- ja virtuaalse masina ketast, ja saatmine ainult vahe.

Kui resynchronization lõpule jõudnud, tavaline delta dispersioonanalüüs jätkata. Võrgu katkestuste või mõne muu katkestuste juhul võite jätkata resynchronization.

Vaikimisi on konfigureeritud automaatselt ajastatud resynchronization väljaspool töötundide tehakse. Kui virtuaalse masina peab olema resynchronized käsitsi, valige virtuaalse masina portaali ja klõpsake **sünkroonige uuesti**.

![Käsitsi resynchronization](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Resynchronization kasutab-plokk chunking algoritmi, kus lähte- ja failide jagunevad fikseeritud tükkideks. Kontrollimisel jaoks igas kogumis on loodud ja võrreldakse määramiseks, mis takistab lähtekoha tuleb kasutada mustandiversioon.

### <a name="retry-logic"></a>Uuestiproovimise loogikale
On sisseehitatud uuesti loogika dispersioonanalüüs tõrkeid. See loogika saab liigitada ühte kahest järgmisest kategooriast:

| Kategooria                  | Stsenaariumid                                    |
|---------------------------|----------------------------------------------|
| Mitte-tagastatav veaväärtus     | Proovitakse kohale toimetada No uuesti. Virtuaalse masina kopeerimise olek on **kriitiline**ja administraatori sekkumiseta on nõutav. Näited: <ul><li>Katkenud VHD ahelas</li><li>Koopia virtuaalse masina olek ei sobi</li><li>Võrgu autentimise tõrge</li><li>Luba tõrge</li><li>Virtuaalse masina, mille puhul autonoomse Hyper-V server ei leitud</li></ul>|
| Taastuv tõrge         | Korduskatsed ilmneda iga dispersioonanalüüs intervall, on eksponentsiaalse tagasi-välja mis suurendab uuesti intervalli algusest esimese proovi (1, 2, 4, 8, 10 minuti) abil. Kui tõrge kordub, proovige iga 30 minuti järel. Näited: <ul><li>Võrgu tõrge</li><li>Vähese kettaruumi</li><li>Mälu tingimus</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>Hyper-V virtuaalse masina kaitse ja taastamine elu tsükkeldiagramm

![Hyper-V virtuaalse masina kaitse ja taastamine elu tsükkeldiagramm](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Muud viited

- [Jälgimine ja kaitse VMware VMM, Hyper-V ja füüsilise saitide tõrkeotsing](./site-recovery-monitoring-and-troubleshooting.md)
- [Microsoft Support jõuda](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Azure'i saidi taastamine levinud vigade ja nende lahendused](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
