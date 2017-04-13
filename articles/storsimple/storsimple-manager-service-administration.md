<properties 
   pageTitle="StorSimple halduri teenuse haldus | Microsoft Azure'i"
   description="Saate teada, kuidas hallata StorSimple seadme Azure klassikaline portaalis StorSimple halduri teenuse abil."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>StorSimple halduri teenuse abil StorSimple seadme haldamine

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse StorSimple halduri teenuse kasutajaliides, sh kuidas ühendada see, erinevaid võimalusi ja lingid välja teatud töövood, mida saab teha seda Kasutajaliidese kaudu. Juhised on mõeldud StorSimple füüsilise ja virtuaalse seade.

Pärast sellest artiklist saate teada, kuidas:

- StorSimple halduri teenusega ühenduse loomiseks
- Liikuge StorSimple halduri kasutajaliides
- StorSimple halduri teenuse kaudu StorSimple seadme haldamine


## <a name="connect-to-storsimple-manager-service"></a>StorSimple halduri teenusega ühenduse loomiseks

StorSimple halduri teenuse Microsoft Azure'i töötab ja ühendab mitu StorSimple seadmete. Keskse Microsoft Azure'i klassikaline portaali töötavad brauseris abil saate hallata järgmistesse seadmetesse. StorSimple halduri teenusega ühenduse loomiseks tehke järgmist.

#### <a name="to-connect-to-the-service"></a>Teenusega ühenduse

1. Liikuge [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Microsoft Azure'i klassikaline portaali (asub paani paremas ülanurgas) logige oma Microsofti konto identimisteave abil.

1. Liikuge kerides allapoole StorSimple halduri teenuse Vasakpoolsel navigeerimispaanil.


## <a name="navigate-storsimple-manager-service-ui"></a>Liikuge StorSimple halduri teenuse kasutajaliides

Navigeerimise hierarhia StorSimple halduri teenuse UI on näidatud järgmises tabelis.

- **StorSimple halduri** sihtleht suunab teid kõigis seadmetes teenuse raames lehtede UI teenusetaseme kuvamine.

- **Seadmete** lehe suunab teid kindla seadme lehtede seadmetasemel – Kasutajaliidese kuvamine.

- **Helitugevuse ümbriste** lehe suunab teid helitugevuse lehele, kus kuvatakse kõik mahud seostatud seade.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple halduri teenuse navigeerimise hierarhia

|Sihtleht|Teenuse taseme lehed|Seadme taseme lehed|Seadme taseme lehed|
|---|---|---|---|
|StorSimple halduri teenuse|Teenuste armatuurlaud|Seadme armatuurlaud||
||Seadmed →|Jälgimine|
||Varukoopia kataloogi|Helitugevuse containers→|Mahud|
||Konfigureerimine (teenus)|Varukoopia poliitika||
||Tööde haldamine|Konfigureerimine (seadme)|
||Teatised|Hooldustööd|

![Video saadaval](./media/storsimple-manager-service-administration/Video_icon.png) **Video saadaval**

Vaadake videot, mis juhendab teid StorSimple halduri teenuse kasutajaliides, klõpsake [siin](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>StorSimple seadme abil StorSimple halduri teenuse haldamine

Järgmises tabelis on kokkuvõte tavalisi ja keerukate töövooge, mis tuleks teha StorSimple halduri teenuse kasutajaliides. Järgmised toimingud on korraldatud vastavalt nende algatatakse UI lehtedel.

Iga töövoo kohta lisateabe saamiseks klõpsake tabeli protseduuri.

#### <a name="storsimple-manager-workflows"></a>StorSimple halduri töövood

|Kui soovite selle tegemiseks...|Minge selle Kasutajaliidese lehe...|Selle toiminguga.|
|---|---|---|
|Teenuse loomine</br>Teenuse kustutamine</br>Teenuse registreerimise võti hankimine</br>Taastada teenuse registreerimise võti|StorSimple halduri teenuse|[StorSimple halduri teenuse juurutamine](storsimple-manage-service.md)
|Krüptovõtme teenuse andmete muutmine</br>Toiming logid kuvamine|StorSimple halduri teenuse → armatuurlaud|[Armatuurlaua StorSimple halduri teenuse kasutamine](storsimple-service-dashboard.md)|
|Seadme desaktiveerimine</br>Seadme kustutamine|StorSimple halduri teenuse → seadmed|[Desaktiveerimine ja kustutamine seade](storsimple-deactivate-and-delete-device.md)|
|Lisateavet katastroofi taastamine ja seadme Tõrkesiirde</br>Tõrkesiirde füüsilise seadmega</br>Virtuaalne seadme Tõrkesiirde</br>Järjepidevuse avariitaastet (BCDR)|StorSimple halduri teenuse → seadmed|[Seadme StorSimple Tõrkesiirde ja Avariijärgne taaste](storsimple-device-failover-disaster-recovery.md)|
|Loendi varukoopiate maht</br>Valige varukoopia määramine</br>Varukoopia kustutamine|StorSimple halduri teenuse → varukoopia kataloogi|[Varukoopiate haldamine](storsimple-manage-backup-catalog.md)|
|Klooni maht|StorSimple halduri teenuse → varukoopia kataloogi|[Klooni maht](storsimple-clone-volume.md)|
|Taastamine varukoopia põhjal määramine|StorSimple halduri teenuse → varukoopia kataloogi|[Taastamine varukoopia põhjal määramine](storsimple-restore-from-backup-set.md)|
|Salvestusruumi kontode kohta</br>Salvestusruumi konto lisamine</br>Salvestusruumi konto redigeerimine</br>Salvestusruumi konto kustutamine</br>Klahv pöördenurka salvestusruumi kontod|StorSimple halduri teenuse → konfigureerimine|[Halda salvestusruumi kontod](storsimple-manage-storage-accounts.md)|
|Läbilaskevõime mallide kohta</br>Läbilaskevõime malli lisamine</br>Läbilaskevõime malli redigeerimine</br>Läbilaskevõime malli kustutamine</br>Läbilaskevõime vaikemalli kasutamiseks</br>Algavad määratud aja jooksul kogu päeva läbilaskevõime malli loomine|StorSimple halduri teenuse → konfigureerimine|[Läbilaskevõime levirühmade haldamine](storsimple-manage-bandwidth-templates.md)|
|Juurdepääsu juhtimine kirjete kohta</br>Accessi juhtelement kirje loomine</br>Accessi juhtelement kirje redigeerimiseks</br>Accessi juhtelement kirje kustutamine|StorSimple halduri teenuse → konfigureerimine|[Juurdepääsu juhtimine kirjete haldamine](storsimple-manage-acrs.md)|
|Projekti üksikasjade kuvamine</br>Katkestamine|StorSimple halduri teenuse → tööde haldamine|[Hallata](storsimple-manage-jobs.md)
|Teatiste vastuvõtmine</br>Teatiste haldamine</br>Vaadake üle teatised|StorSimple halduri teenuse → teatiste|[Saate vaadata ja hallata StorSimple teatised](storsimple-manage-alerts.md)
|Ühendatud algataja kuvamine</br>Seadme järjenumbri otsimine</br>Target IQN otsimine|Seadmete StorSimple halduri teenuse → → armatuurlaud|[Kasutage StorSimple seadme armatuurlaud](storsimple-device-dashboard.md)|
|Jälgimisega seotud Diagrammide loomine|Seadmete StorSimple halduri teenuse → → jälgimine|[Seadme StorSimple jälgimine](storsimple-monitor-device.md)|
|Lisage helitugevuse container</br>Helitugevuse container muutmine</br>Helitugevuse container kustutamine|StorSimple halduri teenuse → seadmed → helitugevuse ümbriste|[Helitugevuse ümbriste haldamine](storsimple-manage-volume-containers.md)|
|Lisage maht</br>Muutke maht</br>Võrguühenduseta maht</br>Kustutage maht</br>Kuvari maht|StorSimple halduri teenuse → seadmed → helitugevuse ümbriste → maht|[Mahu haldamine](storsimple-manage-volumes.md)|
|Seadme sätete muutmine</br>Kellaaja sätete muutmine</br>DNS.md sätete muutmine</br>Võrgu liidesed konfigureerimine|Seadmete StorSimple halduri teenuse → → konfigureerimine|[Seadme konfiguratsiooni StorSimple seadme muutmine](storsimple-modify-device-config.md)|
|Vaate web puhverserveri sätted|Seadmete StorSimple halduri teenuse → → konfigureerimine|[Teie seadme jaoks veebiteenuse puhverserveri konfigureerimine](storsimple-configure-web-proxy.md)|
|Seadme administraatori parooli muutmine</br>StorSimple hetktõmmise halduri parooli muutmine|Seadmete StorSimple halduri teenuse → → konfigureerimine|[StorSimple parooli muutmine](storsimple-change-passwords.md)|
|Kaughalduse konfigureerimine|Seadmete StorSimple halduri teenuse → → konfigureerimine|[Kaugühenduse teel ühenduse StorSimple seadme](storsimple-remote-connect.md)|
|Teatiste sätete konfigureerimine|Seadmete StorSimple halduri teenuse → → konfigureerimine|[Saate vaadata ja hallata StorSimple teatised](storsimple-manage-alerts.md)|
|Seadme StorSimple CHAP konfigureerimine|Seadmete StorSimple halduri teenuse → → konfigureerimine|[Seadme StorSimple CHAP konfigureerimine](storsimple-configure-chap.md)|
|Lisada varukoopia poliitika</br>Lisage või muutke ajakava</br>Varukoopia poliitika kustutamine</br>Käsitsi varukoopia tegemine</br>Kohandatud varukoopia poliitika mitme mahud ja ajakava loomine|StorSimple halduri teenuse → seadmed → varundamise poliitika|[Varukoopia poliitikate haldamine](storsimple-manage-backup-policies.md)|
|Seadme kontrollerid peatamine</br>Taaskäivitage seade kontrollerid</br>Sulgege seadme kontrollerid</br>Seadme algsätted lähtestamine</br>(Ainult kohapealse seadme kohal on)|Seadmete StorSimple halduri teenuse → → hooldustööd|[Selle domeenikontrolleri StorSimple seadme haldamine](storsimple-manage-device-controller.md)|
|Teave riistavara StorSimple</br>Kuvari riistvara olek</br>(Ainult kohapealse seadme kohal on)|Seadmete StorSimple halduri teenuse → → hooldustööd|[Kuvari riistvara komponendid](storsimple-monitor-hardware-status.md)|
|Tugiteenuste paketi loomine|Seadmete StorSimple halduri teenuse → → hooldustööd|[Saate luua ja hallata paketi tugi](storsimple-create-manage-support-package.md)|
|Tarkvara värskenduste installimine|Seadmete StorSimple halduri teenuse → → hooldustööd|[Värskendage oma seadmesse](storsimple-update-device.md)|


##<a name="next-steps"></a>Järgmised sammud
Kui teil tekib igapäevase toimimise StorSimple seadme või mis tahes selle riistavara probleemidest, viidata:

- [Tõrkeotsing funktsionaalseid seadmes](storsimple-troubleshoot-operational-device.md)
- [Kasutage StorSimple indikaator LED jälgimine](storsimple-monitoring-indicators.md)

Kui probleemid ei lahene ja peate looma teenusetaotlus, vaadake [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md).
