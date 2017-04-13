<properties
   pageTitle="StorSimple virtuaalse massiiv - sätte Hyper-v juurutamine"
   description="Teine õppeteema StorSimple virtuaalse massiiv juurutuse hõlmab ettevalmistamise virtuaalse seadme Hyper-v."
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
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Juurutamine StorSimple virtuaalse massiiv – virtuaalse massiiv Hyper-v ettevalmistamine

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Ülevaade

Õppeteema ettevalmistamise kehtib Microsoft Azure'i StorSimple virtuaalse massiivi (tuntud ka kui StorSimple kohapealse virtuaalse või StorSimple virtuaalse seadmed) käivitatud märts 2016 üldiselt kättesaadav (GA) väljaanne. Selles õppetükis kirjeldatakse ettevalmistamise StorSimple virtuaalse massiiv host operatsioonisüsteemis Hyper-V Windows Server 2012 R2, Windows Server 2012 või Windows Server 2008 R2. See artikkel kehtib juurutamise StorSimple virtuaalse massiivi Azure klassikaline portaali kui ka Microsoft Azure'i Government Cloud.

Teil on vaja administraatoriõigusi ettevalmistamine ja virtuaalne seadme konfigureerimine. Ettevalmistamise ja algne häälestus võib kuluda umbes 10 minutit lõpuleviimiseks.


## <a name="provisioning-prerequisites"></a>Ettevalmistamise eeltingimused

Siit leiate eeltingimused ettevalmistamise virtuaalse seadme host operatsioonisüsteemis Hyper-V Windows Server 2012 R2, Windows Server 2012 või Windows Server 2008 R2.

### <a name="for-the-storsimple-manager-service"></a>StorSimple halduri teenuse kohta

Enne alustamist veenduge, et:

-   Kõigi vajalike toimingute tegemiseks [ettevalmistamine portaali StorSimple virtuaalse array](storsimple-ova-deploy1-portal-prep.md)on lõpule viidud.

-   Virtuaalne seadme pilt on Azure portaalist allalaaditud Hyper-v. Lisateavet leiate teemast [Samm 3: alla laadida pildi virtuaalse seadme](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] Tarkvara töötab StorSimple virtuaalse massiiv võib kasutada ainult koos Storsimple halduri teenuse.

### <a name="for-the-storsimple-virtual-device"></a>StorSimple virtuaalse seadme

Enne juurutamist virtuaalne seadme, veenduge, et:

-   Teil on juurdepääs host operatsioonisüsteemis Hyper-V Windows Server 2008 R2 või uuem versioon, mida saab kasutada seadme säte.

-   Host süsteem on võimalus suunata ettevalmistamise virtuaalse seadme järgmistest allikatest:

    -   Vähemalt 4 valdkond.

    -   Vähemalt 8 GB RAM-i.

    -   Ühe võrgu liidese.

    -   500 GB virtuaalse ketta süsteemi andmete jaoks.

### <a name="for-the-network-in-the-datacenter"></a>Võrgu andmekeskuses

Enne alustamist vaadake üle võrgu nõuded StorSimple virtuaalse seadme juurutada ja konfigureerida andmekeskuse võrgu õigesti. Lisateabe saamiseks vt [StorSimple virtuaalse massiiv võrgu nõuetele](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Samm-sammult ettevalmistamine

Ette ja virtuaalse seadmega ühendada, peate tegema järgmised toimingud:

1.  Tagada, et host süsteem on piisavalt ressursse, mis vastavad minimaalsed virtuaalse seadme.

2.  Ettevalmistamise virtuaalse seadme oma hypervisor.

3.  Käivitage virtuaalse seade ja saada IP-aadress.

Järgmistes jaotistes on selgitatud iga järgmist.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Samm 1: Veenduge, et host süsteem vastab minimaalse virtuaalse seade

Virtuaalne seadme loomiseks peate:

-   Windows Server 2012 R2, Windows Server 2012 või Windows Server 2008 R2 SP1 arvutisse installitud Hyper-V roll.

-   Microsoft Hyper-V halduri Microsoft Windowsi kliendi ühendatud Host.

Teil peab veenduge, et aluseks oleva riistvara (hosti süsteem), mis loodava virtuaalse seadme on võimalus suunata virtuaalse seadme järgmistest allikatest:

- Vähemalt 4 valdkond.
- Vähemalt 8 GB RAM-i.
- Ühe võrgu liidese.
- 500 GB virtuaalse ketta süsteemi andmete jaoks.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Samm 2: Ettevalmistamine hypervisor virtuaalse seadme

Järgmiste toimingute ettevalmistamise oma hypervisor seadme.

#### <a name="to-provision-a-virtual-device"></a>Virtuaalne seadme ettevalmistamine

1.  Kopeerige hostis Windows Server kohalikule kettale virtuaalse seadme pilt. See on Azure portaali kaudu allalaaditud pilt (VHD või VHDX). Märkige üles asukoht, kuhu kopeerisite pilt, kui kasutate seda hiljem sisse juhiseid.

2.  Avage **Serveri haldur**. Parempoolses ülanurgas, klõpsake nuppu **Tööriistad** ja valige **Hyper-V haldur**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Kui kasutate opsüsteemi Windows Server 2008 R2, avage Hyper-V haldur. Server Manager klõpsake **rollid > Hyper-V > Hyper-V halduri**.

1.  Teie süsteemi sõlm kontekstimenüü avamiseks paremklõpsake **Hyper-V halduri**ulatus paanil ja seejärel nuppu **Uus** > **virtuaalse masina**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  **Enne alustamist** lehel uue virtuaalse masina viisard, klõpsake nuppu **edasi**.

1.  Sisestage lehel **Määrake nimi ja asukoht** virtuaalse seadme **nimi** . Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  Lehel **Määrake genereerimine** valige seadme pildi tüüp ja klõpsake siis nuppu **edasi**. Sellel lehel ei kuvata, kui kasutate Windows Server 2008 R2.

    * Kui laadisite .vhdx pilt Windows Server 2012 või uuem versioon, valige **genereerimine 2** .
    * Kui laadisite .vhd pilt Windows Server 2008 R2 või uuem versioon, valige **loomine 1** .

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  Lehel **määrata mälu** määrata **käivitus mälu** vähemalt **8192 MB**, ei luba dünaamilise mälu ja klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  Lehel **konfigureerimine võrgunduse** Määrake virtuaalse parameetrit, mis on Interneti-ühendus ja klõpsake siis nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  Lehel **Loo ühendus virtuaalse kõvaketta** suvand **Kasuta olemasolevaid virtuaalse kõvaketta**, virtuaalse seadme pilt (.vhdx või .vhd) asukoha määramine ja seejärel klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  **Kokkuvõte** ja seejärel nuppu **valmis** , virtuaalse masina loomiseks. Kuid ei edasiliikumiseks veel – peate siiski lisada mõningaid CPU poole ja teine ketas. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Minimaalne nõuete täitmiseks peate 4 tuuma. Lisada koos arvutisüsteemi host valitud **Hyper-V halduri** akna Parempoolse paani jaotises loendi **Virtuaalmasinates**, virtuaalse protsessorite, otsige üles äsja loodud virtuaalse masina. Valige ja paremklõpsake masina nimi ja klõpsake nuppu **sätted**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Klõpsake lehel **sätted** vasakul paanil **protsessor**. Parempoolse paani, määrake **virtuaalse protsessorite arvu** 4 (või rohkem). Klõpsake nuppu **Rakenda**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Minimaalne nõuete täitmiseks peate kettal 500 GB virtuaalse andmete lisamiseks. Klõpsake lehel **sätted** :

    1.  Valige vasakul paanil **SCSI kontrolleril**.
    2.  **Kõvaketas,** valige parempoolsel paanil ja klõpsake nuppu **Lisa**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  Lehel **kõvakettale** **Virtual kõvaketas** suvand ja klõpsake nuppu **Uus**. See käivitab **Uue virtuaalse kõvaketta viisard**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  **Enne alustamist** lehe uue virtuaalse kõvaketta viisardi, klõpsake nuppu **edasi**.

1.  **Valige ketta vormindamine lehe**Aktsepteeri **VHDX** vorming vaikesuvand. Klõpsake nuppu **edasi**. Kuva ei kuvata, kui kasutate operatsioonisüsteemi Windows Server 2012 R2 või Windows Server 2008 R2.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  **Valige ketas Tippige lehe**seadmine virtuaalse kõvaketta tüüp **dünaamiliselt laiendamine** (soovitatav). Kui valite **kindla suurusega** vaba, töötab ka, kuid võib-olla peate ootama kaua aega. Soovitame, et kasutate **Differencing** suvandit. Klõpsake nuppu **edasi**. Pange tähele, et **dünaamiliselt laiendamine** on vaikimisi Windows Server 2012 R2 ja Windows Server 2012. Vaikimisi on Windows Server 2008 R2, **kindlaksmääratud suurusega**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Lehel **Määrake nime ja asukoha** ette **nimi** samuti **asukoht** (saate sirvida ühele) andmed kettale. Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  Lehel **Konfigureerimine kettale** suvand **Loo uus tühi virtuaalse kõvaketta** ja määrake soovitud suurusega **500 GB** (või rohkem). 500 GB on miinimumnõuetele, saate suurema kettal alati ette valmistada. Pange tähele, et te ei saa laiendamine või kahandamine ühe korra ette valmistatud ketas. Sätte kettale suuruse kohta lisateabe saamiseks vaadake suurus jaotises dokumendi [head tavad](storsimple-ova-best-practices.md#configuration-best-practices) . Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Lehel **Kokkuvõte** kõvakettal virtuaalse andmete põhjal üksikasju ja kui täidetud, klõpsake nuppu **valmis** , luua ketas. Viisard suletakse ja virtuaalse kõvaketta lisatakse teie arvutis.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  Naasete lehele **sätted** . Klõpsake nuppu **OK** ja sulgege **sätetelehe** Hyper-V halduri akna naasmiseks.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Samm 3: Alustada virtuaalse seade ja saada IP

Järgmiste toimingute käivitamiseks virtuaalse seadme ja ühenduse loomiseks.

#### <a name="to-start-the-virtual-device"></a>Virtuaalne seadme käivitamine

1.  Käivitage virtuaalse seade.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Pärast seadet töötab, valige seade, paremklõpsake ja valige **Ühenda**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Peate olema valmis seadme 5 – 10 minuti ootama. Sõnumi olek kuvatakse konsooli edenemise näitamiseks. Pärast seadet on valmis, liikuge **toiming**. Vajutage `Ctrl + Alt + Delete` virtuaalse seadmesse sisse logida. Kasutaja vaikimisi *StorSimpleAdmin* ja vaikimisi parool on *parool1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Turvalisuse huvides seadme administraatori parooli aegub esimese Logi sisse. Teil palutakse parooli muuta.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Sisestage parool, mis sisaldab vähemalt 8 märki. Parooli, peavad vastama vähemalt 3 kokku 4 järgmised nõuded: suurtähti, väiketähti, arvulised ja teisiti. Sisestage parool selle kinnitamiseks uuesti. Kuvab teate, et parool on muutunud.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Pärast seda, kui parool on muudetud, virtuaalse seadme võib-olla taaskäivitama. Oodake, kuni seadme.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    Windows PowerShelli konsooli seadme kuvatakse koos edenemise riba.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  Samme 6 – 8 rakenduvad ainult siis, kui käivitamist kuni mitte DHCP keskkonnas. Kui olete DHCP keskkonnas, jätke need sammud vahele ja jätkake 9. Kui teie seadme mitte DHCP keskkonnas käivitatud, kuvatakse järgmisel kuvatõmmisel.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Nüüd peate võrgu konfigureerimine.

1.  Kasutage funktsiooni `Get-HcsIpAddress` käsk võrgu liidesed virtuaalse seadmes sisse lülitatud. Kui teie seade on lubatud ühe võrgu kasutajaliides, määratud selle liidese vaikenimi on `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Kasutage funktsiooni `Set-HcsIpAddress` cmdlet-käsu võrgu konfigureerimine. Näide on allpool näidatud:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Pärast algne häälestus on lõpetatud ja seade on käivitatud, kuvatakse seade banner tekst. Kirjutage IP-aadress ja URL-i banner tekst kuvatakse seadme haldamine. Kasutate veebi UI virtuaalse seadme ühenduse loomiseks ja selle kohaliku häälestamine ja IP-aadress.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Valikuline) Seda juhist täitke ainult siis, kui juurutate seadme Government pilveteenuses. Nüüd lubate Ameerika Ühendriikide Federal teabe töötlemise Standard (FIPS) režiimi seadmes. FIPS 140 standard määratleb cryptographic algoritmide kohta meile Federal government arvutisüsteemide tundliku teabe kaitse kinnitama kasutamiseks.
    1. Luba FIPS režiim, käivitage järgmine cmdlet-käsk:

        `Enter-HcsFIPSMode`

    2. Taaskäivitage seade, kui teil on lubatud FIPS-režiimis, et cryptographic valideerimised muudatuste jõustumiseks.

        > [AZURE.NOTE] Saate lubada või keelata FIPS-režiimis seadmes. Vahelduvad FIPS ja FIPS režiimi vahel pole toetatud.

Kui teie seade ei vasta nõuetele minimaalne konfiguratsioon, kuvatakse tõrge banner tekst (vt allpool). Peate seadme konfiguratsiooni muutmiseks nii, et see on piisavalt ressursse, mis vastavad miinimumnõuetele. Seejärel taaskäivitage ja ühendage seade. Minimaalne konfiguratsioon nõuetele viidata [Samm 1: tagada host süsteem vastab minimaalse virtuaalse seadme](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Kui teil näo algsel konfigureerimisel kohalik web Kasutajaliidese abil muu viga, vaadake järgmist töövoogude [haldamine oma StorSimple virtuaalse massiiv](storsimple-ova-web-ui-admin.md)kasutamise kohaliku web UI.

-   Käivitage diagnostika kontrollib [web UI häälestamise](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)tõrkeotsing.

-   [Genereeri log paketi ja logifailid vaade](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![video ikoon](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **Video saadaval**

Videost saate teada, kuidas ette valmistada StorSimple virtuaalse massiiv Hyper-v.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Järgmised sammud

-   [Häälestada oma StorSimple virtuaalse massiiv faili Server](storsimple-ova-deploy3-fs-setup.md)

-   [Seadistada oma StorSimple virtuaalse massiivi iSCSI serveriks](storsimple-ova-deploy3-iscsi-setup.md)
