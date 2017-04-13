<properties
   pageTitle="Juurutamine StorSimple virtuaalse massiiv 1 - portaali ettevalmistamine"
   description="Esimese õpetuse juurutamiseks StorSimple virtuaalse massiiv hõlmab portaali ettevalmistamine"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>Juurutamine StorSimple Virtual massiiv – portaali ettevalmistamine

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Ülevaade

See artikkel kehtib Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme või StorSimple virtuaalse seadme) käivitatud märts 2016 üldiselt kättesaadav (GA) versioonis. See on vajalik täielikult juurutada oma virtuaalse massiiv failiserverisse või mõnda iSCSI serveri juurutuse õpetused sarja esimene artikkel. Selles artiklis kirjeldatakse nõutav loomiseks ja konfigureerimiseks teie StorSimple halduri teenuse ettevalmistamise virtuaalse massiiv enne ettevalmistamine. Selles artiklis ka lingid välja juurutamise konfiguratsiooni kontroll ning samuti konfiguratsiooni eeltingimused.

Peate administraatoriõigusi häälestamise ja konfigureerimise lõpuleviimiseks. Soovitame enne alustamist läbi juurutamise konfiguratsiooni kontroll-loend. Portaali ettevalmistamine võtab alla 10 minuti.

Selles artiklis avaldatud teave kehtib juurutamise StorSimple virtuaalse massiivi Azure klassikaline portaali kui ka Microsoft Azure'i Government Cloud.

### <a name="get-started"></a>Alustamine

Juurutamise töövoo koosneb ettevalmistamine portaali, ettevalmistamise virtuaalse massiiv virtualiseeritud keskkond ja häälestamise lõpuleviimist. Alustamine StorSimple virtuaalse massiiv juurutamise failiserverisse või ettevõtte iSCSI serveris, peate järgmised tabelina vahendeid (artikleid ja videod).

#### <a name="deployment-articles"></a>Juurutamise artikleid

Vaadake oma StorSimple virtuaalse massiiv juurutamiseks ettenähtud järjestuses järgmistest artiklitest.

| **#** | **Selles etapis tuleb**                          | **Saate seda teha …**                                                         | **Kasutage neid dokumente.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **Azure'i klassikaline portaali häälestamine**       | Loomine ja konfigureerimine oma StorSimple halduri teenuse ettevalmistamise StorSimple virtuaalse seadme enne.  |[Portaali ettevalmistamine](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **Sätte virtuaalse massiiv**           | Hyper-v ettevalmistamine ja host operatsioonisüsteemis Hyper-V Windows Server 2012 R2, Windows Server 2012 või Windows Server 2008 R2 StorSimple virtuaalse seadmega ühendada. <br></br> <br></br> VMware, ettevalmistamine ja ühendage StorSimple kohapealse virtuaalse seadmele host operatsioonisüsteemis VMware ESXi 5,5 ja kohal.<br></br>| [Virtuaalne massiiv Hyper-v ettevalmistamine](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Virtuaalne massiiv VMware ettevalmistamine](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **Virtuaalne massiiv häälestamine**              | Faili serverisse teha Alghäälestus, registreerida oma StorSimple failiserverisse ja seadme häälestamise lõpule viia. Seejärel saate ette valmistada SMB ühtlasi. <br></br> <br></br> ISCSI serverisse teha Alghäälestus, registreerida oma StorSimple iSCSI serveri ja seadme häälestamise lõpule viia. Seejärel saate ette valmistada iSCSI maht.| [Faili serveriga ühiseid virtuaalse massiiv häälestamine](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[ISCSI serveriga ühiseid virtuaalse massiiv häälestamine](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Juurutamise videod

| **Selle toimingu tegemiseks...** |  **Vaadake seda videot.**|
|----------------|-------------|
| Üksikasjalike juhiste alustada StorSimple virtuaalse massiiv. | [Alustamine StorSimple virtuaalse massiiv](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Üksikasjalike juhiste ettevalmistamise StorSimple virtuaalse massiiv Hyper-v.|[StorSimple virtuaalse Massiivivalemi loomine](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Üksikasjalike juhiste konfigureerimine ja registreerimist StorSimple virtuaalse massiiv|[StorSimple virtuaalse massiiv konfigureerimine](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Üksikasjalikke juhiseid, et luua ühtlasi, ühtlasi varundamine ja StorSimple virtuaalse massiiv konfigureeritud failiserverisse andmete taastamine|[Kasutage StorSimple virtuaalse massiiv](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Üksikasjalikud juhised StorSimple virtuaalse massiivi Tõrkesiirde ja Avariijärgne taaste|[StorSimple virtuaalse massiiv katastroofiabi](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

Saate kohe hakata Azure klassikaline portaali häälestamine.

## <a name="configuration-checklist"></a>Konfiguratsiooni kontroll-loend

Konfiguratsiooni kontroll kirjeldatakse teavet, mida peate enne tarkvara konfigureerida StorSimple seadme koguda. Selle teabe ette valmistada ettevalmistamine aitab hõlbustavad juurutamine StorSimple seadme teie keskkonnas. Sõltuvalt sellest, kas StorSimple virtuaalse seadme juurutatakse failiserverisse või ettevõtte iSCSI serveris, tuleb teil ühte järgmistest kontroll-loenditest.

-   Laadige alla [StorSimple virtuaalse massiiv faili serveri konfiguratsiooni kontroll-loend](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).

-   Laadige alla [StorSimple virtuaalse massiivi iSCSI serveri konfiguratsiooni kontroll-loend](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Eeltingimused

Siit leiate konfiguratsiooni eeltingimused StorSimple halduri teenust, virtuaalse seadme StorSimple ja andmekeskuse võrku.

### <a name="for-the-storsimple-manager-service"></a>StorSimple halduri teenuse kohta

Enne alustamist veenduge, et:

-   Teil on juurdepääs mandaati oma Microsofti kontoga.

-   Teil on Microsoft Azure'i salvestusruumi konto juurdepääsu mandaati.

-   Microsoft Azure'i tellimuse peaks olema lubatud StorSimple halduri teenuse.

### <a name="for-the-storsimple-virtual-device"></a>StorSimple virtuaalse seadme

Enne juurutamist virtuaalne seadme, veenduge, et:

-   Teil on juurdepääs host operatsioonisüsteemis Hyper-V Windows Server 2008 R2 või uuem versioon või VMware (ESXi 5,5 või uuem versioon), mida saab kasutada seadme säte.

-   Host süsteem on võimalus suunata ettevalmistamise virtuaalse seadme järgmistest allikatest:

    -   Vähemalt 4 valdkond.

    -   Vähemalt 8 GB RAM-i.

    -   Ühe võrgu liidese.

    -   500 GB virtuaalse ketta süsteemi andmete jaoks.

### <a name="for-the-datacenter-network"></a>Andmekeskuse võrgu jaoks

Enne alustamist veenduge, et:

-   Võrgu oma andmekeskuses konfigureeritakse StorSimple seadme võrgu nõuete kohta. Lisateavet leiate teemast [StorSimple virtuaalse massiiv süsteeminõuded](storsimple-ova-system-requirements.md).

-   StorSimple virtuaalse seadmes on spetsiaalne 5 Mbps Interneti-läbilaskevõime (või rohkem) saadaval igal ajal. Selle läbilaskevõime ei saa ühiskasutusse anda muude rakendustega.

## <a name="step-by-step-preparation"></a>Samm-sammult ettevalmistamine

Järgmised üksikasjalike juhiste abil saate oma portaali ettevalmistamine StorSimple halduri teenuse.

## <a name="step-1-create-a-new-service"></a>Samm 1: Loo uus teenus

Ühekordsest StorSimple halduri teenuse saate hallata mitut StorSimple 1200 seadmed. Järgmiste toimingute abil saate luua uue eksemplari StorSimple halduri teenuse. Kui teil on olemasoleva StorSimple halduri teenuse haldamiseks 1200 seadmetes, jätke see juhis vahele ja minge [Step2: saada teenuse registreerimise võti](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Kui lubate ei ole teie teenusega salvestusruumi konto automaatse loomise, peate vähemalt üks salvestusruumi konto loomine, kui olete loonud teenus.
>

> - Kui te ei loo salvestusruumi konto automaatselt, minge [konfigureerimine salvestusruumi uue konto jaoks teenuse](#optional-step-configure-a-new-storage-account-for-the-service) üksikasjalikud juhised.
>

> - Kui märkisite salvestusruumi konto automaatse loomise, minge [Samm 2: saada teenuse registreerimise võti](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Samm 2: Saada teenuse registreerimise võti


Pärast StorSimple halduri teenus on tööks, peate saada teenuse registreerimise võti. Selle klahvi abil registreerida ja StorSimple videoseadme ühendamiseks teenusega.

[Azure'i klassikaline portaali](https://manage.windowsazure.com/)tehke järgmist.


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> Teenuse registreerimise võti kasutatakse StorSimple halduri kõiki seadmeid, mida on vaja registreerida teie haldur StorSimple teenusega registreerimiseks.

## <a name="step-3-download-the-virtual-device-image"></a>Samm 3: Alla laadida virtuaalne seadme pilt

Kui olete teenuse registreerimise võti, peate alla laadida soovitud virtuaalse seade pilt ettevalmistamise virtuaalse seadme host arvutisse. Virtuaalne seadme pildid on teatud operatsioonisüsteem ja saab alla laadida Kiirkäivituse Azure klassikaline portaalis leht.

> [AZURE.IMPORTANT] Tarkvara töötab StorSimple virtuaalse massiiv võib kasutada ainult koos Storsimple halduri teenuse.


[Azure'i klassikaline portaali](https://manage.windowsazure.com/)tehke järgmist.

#### <a name="to-get-the-virtual-device-image"></a>Saada virtuaalse seadme pilt

1.  Klõpsake lehel **StorSimple halduri teenuse** loodud teenust. See viib teid lehele **Kiirkäivituse** . (Võite klõpsata ikooni Lühijuhend ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) juurdepääsu **Kiirkäivituse** lehe mis tahes hetkel.)

1.  Pilt, mida soovite alla laadida Microsoft Download Center vastavat linki. Pildi failid on umbes 4,8 GB.

    -   VHDX Hyper-v Windows Server 2012 ja uuemad versioonid

    -   VHD Hyper-v Windows Server 2008 R2 ja uuemad versioonid

    -   VMDK VMWare ESXi 5,5 ja uuemad versioonid

2.  Laadige alla ja pakkige see lahti faili kohalikule kettale, tegemist, kus asub mahalaadimist fail üles.

![video ikoon](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **Video saadaval**

Vaadake videot üksikasjalikud juhised alustada StorSimple virtuaalse massiiv.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Valikuline etapp: salvestusruumi uue konto jaoks teenuse konfigureerimine

See on valikuline etapp, mis seda tuleb teha ainult siis, kui salvestusruumi konto automaatne loomine ei võimalda teie teenusega.

Kui teil on vaja luua Azure storage konto teises regioonis, vaadake, [Kuidas luua konto salvestusruumi](storage-create-storage-account.md#create-a-storage-account) üksikasjalikud juhised.

[Azure'i klassikaline portaali](https://manage.windowsazure.com/) StorSimple halduri teenus Microsoft Azure'i salvestusruumi konto lisamiseks tehke järgmist.

#### <a name="to-add-a-storage-account"></a>Salvestusruumi konto lisamine

1.  Klõpsake StorSimple halduri teenuse sihtleht, valige teenust ja topeltklõpsake seda. See viib teid lehele **Kiirkäivituse** . Valige lehe **konfigureerimine** .

2.  Klõpsake nuppu **Add/edit salvestusruumi konto**. Dialoogiboksis **Konto lisamine/redigeerimine salvestusruumi** tehke järgmist.

    1.  Klõpsake nuppu **Lisa uus**.

    1.  Sisestage oma salvestusruumi konto nimi.

    1.  Lisage esmane **Kiirklahv** Microsoft Azure'i salvestusruumi konto jaoks.

    1.  Valige **Luba SSL režiimi** luua turvalist kanalit võrguühenduse seadme ja pilveteenuse vahel kohta. Ainult juhul, kui teil on opsüsteem sees kohaselt., tühjendage ruut **Luba SSL-i režiim** .

    1.  Klõpsake ikooni Otsi ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). Pärast konto salvestusruumi on loodud, teavitatakse teid sellest.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  Äsja loodud salvestusruumi konto kuvatakse lehe jaotises **salvestusruumi kontode** **konfigureerimine** . Klõpsake nuppu **Salvesta** vastloodud salvestusruumi konto salvestamiseks. Klõpsake nuppu **OK** kinnituse küsimisel.


## <a name="next-step"></a>Järgmise juhise juurde

Järgmise sammuna ettevalmistamise virtuaalse masina StorSimple virtuaalse seadme. Sõltuvalt opsüsteemist host, vaadake üksikasjalikke juhiseid:

-   [Ettevalmistamise StorSimple virtuaalse massiiv Hyper-v](storsimple-ova-deploy2-provision-hyperv.md)

-   [StorSimple virtuaalse massiiv VMware ettevalmistamine](storsimple-ova-deploy2-provision-vmware.md)
