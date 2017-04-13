<properties
   pageTitle="Juurutamine StorSimple seadme Government portaalis | Microsoft Azure'i"
   description="Kirjeldatakse juhiseid ja head tavad StorSimple Update 2 seadmest ja Azure Governmenti portaalis teenuste juurutamine."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="v-sharos" />

# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal-update-2"></a>Juurutamine seadme kohapealse StorSimple Government portaalis (Värskenda 2)

[AZURE.INCLUDE [storsimple-version-selector-deploy-gov](../../includes/storsimple-version-selector-deploy-gov.md)]

## <a name="overview"></a>Ülevaade

Tere tulemast rakendusse Microsoft Azure StorSimple seadme juurutamise. Juurutamise olümpiaandmed rakendada StorSimple 8000 seeria Update 2 tarkvara töötab Azure Governmenti portaalis. Selle sarja õppeteemad sisaldab konfiguratsiooni kontroll-loendi, konfiguratsiooni eeltingimused ja üksikasjalik konfigureerimistoimingute StorSimple seadme loendit.

Teave olümpiaandmed eeldab, et teil on läbi ettevaatusabinõusid, lahti, racked ja komplekslõng StorSimple seadme. Kui soovite teha, alustage läbivaatamise [ettevaatusabinõusid](storsimple-safety.md). Järgige seadme lahti, mount koguda ja kaabel seadme.

- [Lahti, mount koguda ja teie 8100 kaabel](storsimple-8100-hardware-installation.md)
- [Lahti, mount koguda ja teie 8600 kaabel](storsimple-8600-hardware-installation.md)

Peate administraatoriõigusi häälestamise ja konfigureerimise lõpuleviimiseks. Soovitame teil enne alustamist vaadake konfiguratsiooni kontroll-loend. Juurutamise ja konfigureerimise protsess võib võtta aega lõpuleviimiseks.

> [AZURE.NOTE] Veebisaidil Microsoft Azure'i StorSimple juurutamise teave kehtib ainult StorSimple 8000 seeria seadmed. Lisateavet 7000 seeria seadmete, minge: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 7000 sarja, vaadake [StorSimple süsteemi Lühijuhend](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Juurutamise juhised

Tehke StorSimple seadme konfigureerida ja ühendage see StorSimple halduri teenust vaja järgmist. Lisaks nõutud toimingute on valikulised toimingud ja toiminguid, mida võite vajada lõpuleviimiseks juurutamise käigus. Juurutamise üksikasjalikke juhiseid näitavad, kui tuleks teha iga valikuline järgmist.


| Toiming                                                                                   | Kirjeldus                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EELTINGIMUSED**                                                                      | Need tuleb lõpule viia ettevalmistamisel on eelseisvad juurutamiseks.                                                                                        |
|[Juurutamise konfiguratsiooni kontroll-loend](#deployment-configuration-checklist)                                                     | Kasutage järgmise kontroll-loendi koguda ja salvestada teabe enne ja juurutamise käigus.                                                                       |
| [Juurutamise eeltingimused](#deployment-prerequsites)                                                               | Need kinnitada, et keskkond on valmis juurutamiseks.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **SAMM-SAMMULT JUURUTAMINE**                                                                   | Need juhised on vaja juurutada StorSimple seadme valmistamisel.                                                                                      |
| [Samm 1: Loo uus teenus](#step-1-create-a-new-service)                                                         | Haldus pilve ja salvestusruumi StorSimple seadme häälestamine. *Jäta seda toimingut, kui teil on olemasoleva teenuse StorSimple teiste seadmete jaoks*.              |
| [Samm 2: Saada teenuse registreerimise võti](#step-2-get-the-service-registration-key)                                               | Selle klahvi abil registreerida ja StorSimple videoseadme ühendamiseks halduse teenusega.                                                                         |
| [Samm 3: Konfigureerimine ja Registreeruge StorSimple seadme Windows PowerShelli abil](#step 3-configure-and-register-the-device-through-windows-powershell-for-storsimple) | Ühendage seade võrku ja registreerida Azure'i halduse teenuse abil häälestamise lõpuleviimiseks.                                            |
| [Samm 4: Minimaalne seadme häälestamine](#step-4-complete-the-minimum-device-setup) </br>Valikuline: Värskendage StorSimple seadme. | Seadme häälestamine ja lubage selle esitada salvestusruumi halduse teenuse abil.                                                                      |
| [Samm 5: Looge helitugevuse container](#step-5-create-a-volume-container)                                                      | Looge nõus sätte maht. Helitugevuse container on salvestusruumi konto, läbilaskevõime ja kõik selles sisalduvad mahud krüptimise sätteid.    |
| [Samm 6: Luua maht](#step-6-create-a-volume)                                                               | Sätte salvestusruumi draivid StorSimple seadme servereid.                                                                                        |
| [Juhis 7: Mount, lähtestamine ja vormindada maht](#step-7-mount-initialize-and-format-a-volume) </br>Valikuline: Konfigureerige MPIO.            | Teie serveritega ühenduse pakutavate iSCSI talletamist. Soovi korral saate konfigureerida MPIO tagamaks, et teie serverid saate luba link, võrgu ja kasutajaliidese tõrge.                                                                                                                                                              |
| [Samm 8: Võtta varukoopia](#step-8-take-a-backup)                                                                  | Häälestada oma varukoopia poliitika andmete kaitsmine                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **MUUDE TOIMINGUTE**                                                                   | Võimalik, et peate viitama neid toiminguid nagu teie lahenduse juurutamine.                                                                                    |
| [Uue salvestusruumi konto jaoks teenuse konfigureerimine](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [Ühenduse seadme järjestikune konsooli PuTTY abil](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [Otsida ja värskendused](#scan-for-and-apply-updates)                                                   |                                                                                                                                                               |
| [Windows Server hosti IQN hankimine](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Käsitsi varukoopia loomine](#create-a-manual-backup)                                                                 |
| [MPIO konfigureerimine](#configure-mpio)                                                                          |

## <a name="deployment-configuration-checklist"></a>Juurutamise konfiguratsiooni kontroll-loend

Enne juurutamist StorSimple seadme, peate oma seadmesse tarkvara konfigureerimiseks teabe kogumiseks. Valmistada ette valmistada teave aitab hõlbustavad juurutamine StorSimple seadme teie keskkonnas. Laadige alla ja järgmise kontroll-loendi Märkus konfiguratsiooni üksikasjad, kui juurutate oma seadme abil.  

[Laadige alla StorSimple juurutamise konfiguratsiooni kontroll-loend](http://www.microsoft.com/download/details.aspx?id=49159)  

## <a name="deployment-prerequisites"></a>Juurutamise eeltingimused

Järgmistes jaotistes kirjeldatakse StorSimple halduri teenust ja StorSimple seadme konfiguratsiooni eelduseks.

### <a name="for-the-storsimple-manager-service"></a>StorSimple halduri teenuse kohta

Enne alustamist veenduge, et:

- Teil on juurdepääs mandaati oma Microsofti kontoga.

- Teil on Microsoft Azure'i salvestusruumi konto juurdepääsu mandaati.

- Tellimuse Microsoft Azure'i StorSimple halduri teenus on lubatud. Teie tellimus peaks ostetud [Ettevõtteleping](https://azure.microsoft.com/pricing/enterprise-agreement/).

- Teil on juurdepääs terminal imiteerimist tarkvara, nt PuTTY.

### <a name="for-the-device-in-the-datacenter"></a>Seadme andmekeskuses

Enne seadme konfigureerimist veenduge, et:

- Teie seade on täielikult lahti, paigaldatud on sektsioon ja täielikult komplekslõng power, võrgu ja järjestikune juurdepääsu, nagu on kirjeldatud:

    -  [Lahti, mount koguda ja seadme 8100 kaabel](storsimple-8100-hardware-installation.md)
    -  [Lahti, mount koguda ja seadme 8600 kaabel](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Võrgu andmekeskuses

Enne alustamist veenduge, et:

- Tulemüüri andmekeskuse pordid lahti iSCSI lubamine ja cloud liikluse, nagu on kirjeldatud [Networking StorSimple seadme nõuded](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

## <a name="step-by-step-deployment"></a>Samm-sammult juurutamine

Kasutage seadme StorSimple andmekeskuses järgmised juhised.

## <a name="step-1-create-a-new-service"></a>Samm 1: Loo uus teenus

StorSimple halduri teenuse saate hallata mitut StorSimple seadmed. Järgmiste toimingute abil saate luua uue eksemplari StorSimple halduri teenuse.

[AZURE.INCLUDE [storsimple-create-new-service-gov](../../includes/storsimple-create-new-service-gov.md)]

> [AZURE.IMPORTANT] Kui lubate ei ole teie teenusega salvestusruumi konto automaatse loomise, peate vähemalt üks salvestusruumi konto loomine, kui olete loonud teenus. Helitugevuse container loomisel, kasutatakse selle salvestusruumi kontot.
>
> * Kui te ei loo salvestusruumi konto automaatselt, minge [konfigureerimine salvestusruumi uue konto jaoks teenuse](#configure-a-new-storage-account-for-the-service) üksikasjalikud juhised.
> * Kui märkisite salvestusruumi konto automaatse loomise, minge [Samm 2: saada teenuse registreerimise võti](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Samm 2: Saada teenuse registreerimise võti

Pärast StorSimple halduri teenus on tööks, peate saada teenuse registreerimise võti. Selle klahvi abil registreerida ja ühendage seade StorSimple teenus.

Järgmiste toimingute Government portaalis.

[AZURE.INCLUDE [storsimple-get-service-registration-key-gov](../../includes/storsimple-get-service-registration-key-gov.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Samm 3: Konfigureerimine ja Registreeruge StorSimple seadme Windows PowerShelli abil

Kasutage Windows PowerShelli StorSimple seadme StorSimple algse häälestamise lõpuleviimiseks, nagu on selgitatud järgmist. Peate kasutama terminal imiteerimist tarkvara seda toimingut. Lisateabe saamiseks vt [Kasutamine PuTTY ühenduse seadme järjestikune konsooli](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-gov](../../includes/storsimple-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Samm 4: Täieliku minimaalne seadme häälestamine

Minimaalne seadme konfiguratsiooni StorSimple seadme, mida on vaja:

- Häälestage teisene DNS server.
- Luba iSCSI vähemalt üks võrk liides.
- Saate määrata nii kontrolörid fikseeritud IP-aadressid.

Valitsuse portaalis minimaalne seadme häälestamise lõpuleviimiseks tehke järgmist.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Samm 5: Looge helitugevuse container

Helitugevuse container on salvestusruumi konto, läbilaskevõime ja kõik selles sisalduvad mahud krüptimise sätteid. Peate enne alustamist ettevalmistamise mahud StorSimple seadme helitugevuse container loomiseks.

Riigiasutuste portaalis helitugevuse container loomiseks tehke järgmist.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Samm 6: Luua maht

Pärast helitugevuse container loomist saate mälu mahu StorSimple seadme servereid ette valmistada. Valitsuse portaalis loomiseks tehke järgmist.

> [AZURE.IMPORTANT] Azure'i StorSimple saate luua ainult õhukesed ettevalmistatud draividel.  Te ei saa luua ettevalmistatud täielikult või osaliselt ettevalmistatud mahud Azure StorSimple abil.

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Juhis 7: Mount, lähtestamine ja vormindada maht

Nende juhiste täitmiseks oma Windows Server hosti.

> [AZURE.IMPORTANT]

> - Kõrge kasutatavuse StorSimple lahenduse, soovitame konfigureerida MPIO serveritesse host (valikuline) enne iSCSI konfigureerimine. MPIO konfiguratsiooni hosti serverites tagab, et serverid saate luba link, võrgu või kasutajaliidese tõrge.

> - MPIO ja iSCSI installi ja konfiguratsiooni juhiseid Windows Server host, avage [Seadme StorSimple MPIO konfigureerimine](storsimple-configure-mpio-windows-server.md). Need sisaldavad ka mount, lähtestamine ja vormindada StorSimple mahud juhiseid.

> - MPIO ja iSCSI installi ja konfiguratsiooni juhiseid Linux hosti, minge [Oma StorSimple Linux pakkuja MPIO konfigureerimine](storsimple-configure-mpio-on-linux.md)

Kui te ei soovi konfigureerimine MPIO, tehke järgmist mount, lähtestamine ja teie StorSimple draivid Windows Server hosti vormindamine.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Samm 8: Võtta varukoopia

Varukoopiate punkti /-kellaaja kaitsmiseks mahtu ja parandada taastatavus ajal minimeerimine taastamine korda. Saate kahte tüüpi varundamise StorSimple seadmes: kohaliku hetktõmmiseid ja pilveteenuse pilte. Iga varukoopia järgmist tüüpi võib olla **ajastatud** või **käsitsi**.

Riigiasutuste portaalis ajastatud varukoopia loomiseks tehke järgmist.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Saate käsitsivormingu varukoopia igal ajal. Toimingute, minge [loomine käsitsi varundamise](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>Uue salvestusruumi konto jaoks teenuse konfigureerimine

See on valikuline etapp, mida tuleb teha ainult siis, kui salvestusruumi konto automaatne loomine ei võimalda teie teenusega. Microsoft Azure storage konto on vaja luua StorSimple helitugevuse ümbrises.

Kui teil on vaja luua Azure storage konto teises regioonis, leiate [Azure'i salvestusruumi kontod](../storage/storage-create-storage-account.md) üksikasjalikud juhised.

Valitsuse portaalis **StorSimple halduri teenuse** lehel tehke järgmist.

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Ühenduse seadme järjestikune konsooli PuTTY abil

Ühenduse loomine Windows PowerShelli StorSimple, peate kasutama terminal imiteerimist tarkvara, nt PuTTY. Saate kitt, kui pöördute seade otse järjestikune konsooli kaudu või avada Telneti seansi kaugarvutist.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Otsida ja värskendused

Seadme värskendamine võib kuluda mitu tundi. Järgmiste toimingute otsimiseks ja värskendused oma seadmes.

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Uuendada oma seadme

1.  Klõpsake seadme **Kiirkäivituse** lehel **seadmed**. Valige seadme, valige **hooldustööd** ja klõpsake nuppu **Värskenduste skannida**.  
2.  Saadaolevate värskenduste tööd on loodud. Kui värskendused on saadaval, muutub **Skannimine värskenduste** **Installi värskendused**. Klõpsake nuppu **Installi värskendused**.
3.  Mõne värskenduse töö luuakse. Värskendamise oleku jälgimine navigeerides **projektidele**.

    > [AZURE.NOTE] Värskenduse töö käivitudes seda kohe kuvab 50 protsenti. Olek muutub 100% ainult siis, kui töö värskendamine on lõpule jõudnud. On pole reaalajas värskenduse toimingu olek.

4.  Pärast seadet värskendatud lubada andmete 2 ja 3 andmete võrgu liidesed, kui need olid keelatud.

## <a name="get-the-iqn-of-a-windows-server-host"></a>Windows Server hosti IQN hankimine

Järgmiste toimingute saada iSCSI kvalifitseeritud Windows host, kus töötab Windows Server® 2012 nimi (IQN).

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Käsitsi varukoopia loomine

Valitsuse portaalis StorSimple seadme vajadusel käsitsi varundamise ühte loomiseks tehke järgmist.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup-gov.md)]

## <a name="configure-mpio"></a>MPIO konfigureerimine

Mitmeteelise i/o (MPIO) on valikuline funktsioon ja Windows Server vaikimisi pole installitud. See peaks olema installitud funktsioonina Server Manageri kaudu. Minge [Konfigureerimine MPIO seadme StorSimple](storsimple-configure-mpio-windows-server.md)MPIO installimisjuhiseid.

MPIO installimise juhiste saamiseks ühendatud Linux host StorSimple seade, avage [Oma Linux pakkuja MPIO konfigureerimine](storsimple-configure-mpio-on-linux.md).

> [AZURE.NOTE] MPIO Azure StorSimple virtuaalse seadmes ei toetata.

## <a name="next-steps"></a>Järgmised sammud

- Konfigureerige [virtuaalne seade](storsimple-virtual-device-u2.md).

- [StorSimple halduri teenuse](https://msdn.microsoft.com/library/azure/dn772396.aspx) abil saate hallata StorSimple seadme.
