<properties 
   pageTitle="Juurutamine StorSimple hetktõmmise halduri | Microsoft Azure'i"
   description="Saate teada, kuidas alla laadida ja installida StorSimple hetktõmmise halduri MMC lisandmooduli haldamise StorSimple andmeid varundamise ja kaitse funktsioonid."
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
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>StorSimple hetktõmmise halduri MMC lisandmooduli juurutamine

## <a name="overview"></a>Ülevaade

Microsoft Management Console (MMC) lisandmoodul, mis lihtsustab andmekaitse ja varukoopia juhtimine Microsoft Azure'i StorSimple keskkonnas on StorSimple hetktõmmise ülemus. StorSimple hetktõmmise halduriga, saate hallata Microsoft Azure'i StorSimple kohapealse ja salvestusruum pilveteenuses täielikult integreeritud salvestusruumi süsteemi, nagu oleks, seega lihtsustamine varundus ja taaste protsesside ja vähendada. 

Selles õppetükis kirjeldatakse konfiguratsiooni nõuded, samuti installimist, eemaldamine ja üleminekut StorSimple hetktõmmise haldur.

>[AZURE.NOTE] 
>
>- StorSimple hetktõmmise halduri abil ei saa hallata Microsoft Azure'i StorSimple virtuaalse massiivi (tuntud ka kui StorSimple kohapealse virtuaalne seadmed).
>
>- Kui plaanite installida StorSimple 2 StorSimple seadmes, kindlasti uusim versioon StorSimple hetktõmmise halduri alla laadima ja installima **enne installimist StorSimple 2**. Uusima versiooni StorSimple hetktõmmise Manager on tagasiühilduvad ja töötab koos kõigi avaldatud versioonid Microsoft Azure'i StorSimple. Kui kasutate eelmise versiooni StorSimple hetktõmmise Manager, peate värskendage seda (teil pole vaja desinstallida enne uue versiooni installimist eelmise versiooni).

## <a name="storsimple-snapshot-manager-installation"></a>StorSimple hetktõmmise halduri installimine

Arvutites, kus töötab Windows Server 2008 R2 SP1, Windows Server 2012 või Windows Server 2012 R2 operatsioonisüsteemi installimist StorSimple hetktõmmise haldur. Serverites, kus töötab Windows 2008 R2, peate installima Windows Server 2008 SP1 ja Windows Management Framework 3.0. 

Enne installimist või täiendamine StorSimple hetktõmmise Manager lisandmooduli jaoks Microsoft Management Console (MMC), veenduge, et seade ja host server Microsoft Azure'i StorSimple õigesti konfigureeritud. 

## <a name="configure-prerequisites"></a>Eeltingimused konfigureerimine

Järgmised toimingud anda üksikasjalik ülevaade konfigureerimistoimingud, et peate teostama enne installimist StorSimple hetktõmmise ülemus. Täieliku Microsoft Azure'i StorSimple konfiguratsiooni ja häälestamine teave, sh süsteeminõuded ja üksikasjalikud juhised leiate teemast [Deploy kohapealse StorSimple seadme](storsimple-deployment-walkthrough.md).

>[AZURE.IMPORTANT]Enne alustamist vaadake üle [juurutamise konfiguratsiooni kontroll](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) ja ja [Deploy StorSimple seadme kohapealse](storsimple-deployment-walkthrough.md) [juurutuse eeltingimused](storsimple-deployment-walkthrough.md#deployment-prerequisites) .
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>Enne installimist StorSimple hetktõmmise haldur

1. Lahti, mount ja Microsoft Azure'i StorSimple Bluetooth- [seadme StorSimple 8100 installimine](storsimple-8100-hardware-installation.md) või [seadme StorSimple 8600](storsimple-8600-hardware-installation.md)kirjeldatud.

2. Veenduge, et teie arvuti kasutab ühte järgmised operatsioonisüsteemid.

    - Windows Server 2008 R2 (töötab Windows 2008 R2 serverid, peate installima Windows Server 2008 SP1 ja Windows Management Framework 3.0)
    - Windows Server 2012
    - Windows Server 2012 R2
 
    StorSimple virtuaalse seadme selle hosti peab olema Microsoft Azure virtuaalse masina. 

3. Veenduge, et vastate kõik Microsoft Azure'i StorSimple konfiguratsiooni nõuded. Lisateabe saamiseks minge [juurutamise eeltingimused](storsimple-deployment-walkthrough.md#deployment-prerequisites).

4. Ühendage seade Host ja algse konfigureerimise. Lisateabe saamiseks minge [juurutamise juhiseid](storsimple-deployment-walkthrough.md#deployment-steps).

5. Luua mahud seadmes, määrata selle hosti ja veenduge, et selle hosti saate mount ning neid kasutada. StorSimple hetktõmmise haldur toetab järgmist tüüpi maht. 

    - Põhilised mahud
    - Lihtne mahud
    - Dünaamilised draivid
    - Peegeldatud dünaamilised draivid (RAID 1)
    - Kobar jagatud mahud
 
    StorSimple seadme või StorSimple virtuaalse seadmes mahud loomise kohta teabe saamiseks külastage [samm 6: luua maht](storsimple-deployment-walkthrough.md#step-6-create-a-volume), [Deploy kohapealse StorSimple seadme](storsimple-deployment-walkthrough.md)sisse.

## <a name="install-a-new-storsimple-snapshot-manager"></a>Uue StorSimple hetktõmmise halduri installimine

Enne installimist StorSimple hetktõmmise halduri, veenduge, et mahud enda loodud StorSimple seadmes või StorSimple virtuaalse seade ühendatud, lähtestatud ja vormindatud, nagu on kirjeldatud [konfigureerimine eeltingimused](#configure-prerequisites).

>[AZURE.IMPORTANT]
>
>- StorSimple virtuaalse seadme selle hosti peab olema Microsoft Azure virtuaalse masina. 
>
>- Selle hosti peab töötama Windows 2008 R2, Windows Server 2012 või Windows Server 2012 R2. Kui teie serveris Windows Server 2008 R2, peate installima Windows Server 2008 SP1 ja Windows Management Framework 3.0.
>
>- Mõne iSCSI ühenduse Host StorSimple seadmele tuleb konfigureerida, enne kui ühendate seadme StorSimple hetktõmmise haldur.

Järgmiste juhiste lõpuleviimiseks värske installi StorSimple hetktõmmise halduri. Kui installite uuendada, minge [täiendamine või StorSimple hetktõmmise halduri uuesti](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Samm 1: StorSimple hetktõmmise halduri installimine 
- Samm 2: Seadmega ühendada StorSimple hetktõmmise haldur 
- Samm 3: Kontrollige ühendust seadmega 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Samm 1: StorSimple hetktõmmise halduri installimine

Järgmiste juhiste abil saate installida StorSimple hetktõmmise haldur.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple hetktõmmise halduri installimine

1. StorSimple hetktõmmise Manager tarkvara (Ava Microsoft Download Center [StorSimple hetktõmmise Manager](https://www.microsoft.com/download/details.aspx?id=44220) ) alla laadida ja selle hosti kohalikult salvestada.

2. File Explorer, paremklõpsake tihendatud kaust ja seejärel klõpsake nuppu **ekstrakti kõik**.

3. **Ekstrakti tihendatud (zip) kaustade** aknas **valige sihtkoht ja ekstraktida** väljale tippige või liikuge sirvides soovitud tee, kuhu soovite faili ekstraktimist. 

       >[AZURE.IMPORTANT] Peate installima StorSimple hetktõmmise halduri c-ketas.
 
4. Märkige ruut **Kuva ekstraktitud failid, kui olete lõpetanud** , ja seejärel klõpsake nuppu **ekstrakti**.

    ![Dialoogiboksi failid ekstraktida](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. Kui soovitud eraldamine on lõpule jõudnud, avatakse kaust, kuhu. Topeltklõpsake rakenduse häälestamine kuvatavat ikooni sihtkausta.

5. Kui **Häälestamise eduka** teade kuvatakse, klõpsake nuppu **Sule**. Peaksite nägema StorSimple hetktõmmise ikooni töölauale.

    ![töölaua ikoon](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Samm 2: Seadmega ühendada StorSimple hetktõmmise haldur

Järgmiste juhiste abil saate StorSimple hetktõmmise halduri StorSimple seadmega ühendada.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Seadme StorSimple hetktõmmise halduri ühendamiseks

1. Teie töölaual ikooni StorSimple hetktõmmise haldur. StorSimple hetktõmmise Manager aken. Aken sisaldab **ulatus** paan, **otsingutulemite** paan ja kuvatakse paanil **toimingud** . 

    ![StorSimple hetktõmmise halduri kasutajaliides](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - **Ulatus** paan (vasakpaanil) sisaldab loendit sõlmed korraldada puustruktuuris. Saate laiendada mõned sõlmed valida vaate või sõlme seotud andmeid. Klõpsake noolt ikooni laiendamiseks või ahendamiseks sõlm. Paremklõpsake selle üksuse saadaval toimingute loendi kuvamiseks paanil **ulatus** . 

    - **Otsingutulemite** paan (keskmisel paanil) sisaldab üksikasjaliku oleku teavet sõlm, vaate või **ulatus** paanil valitud andmed.

    - Paanil **toimingud** on loetletud toimingud, mida saate teha sõlm, vaate või **ulatus** paanil valitud andmed.

    Kasutajaliidese StorSimple hetktõmmise halduri täieliku kirjelduse leiate teemast [StorSimple hetktõmmise halduri kasutajaliides](storsimple-use-snapshot-manager.md).

2. **Ulatus** paanil Paremklõpsake sõlme **seadmed** ja seejärel klõpsake nuppu **seadme konfigureerimine**. Kuvatakse dialoogiboks **seadme konfigureerimine** .

    ![Seadme konfigureerimine](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. Valige loendiboksis **seadme** Microsoft Azure StorSimple seadme või virtuaalse seadme IP-aadress. Tippige väljale **parool** seadme Azure klassikaline portaali loodud StorSimple hetktõmmise halduri parool. Klõpsake nuppu **OK**.

4. Seadme, mille te otsib StorSimple hetktõmmise haldur. Kui seade on saadaval, lisab StorSimple hetktõmmise halduri ühenduse. Saate [kontrollida seadme](#to-verify-the-connection) kinnitada, et ühendus on edukalt lisatud.

    Kui seade on mingil põhjusel saadaval, StorSimple hetktõmmise halduri annab tõrketeate. Sulgemiseks klõpsake nuppu **OK** kuvatakse tõrketeade ja seejärel käsku **Tühista** **seadme konfigureerimine** dialoogiboksi sulgemiseks.

5. Kui see loob seade, impordib StorSimple hetktõmmise halduri iga helitugevuse rühma jaoks seade on konfigureeritud eeldusel, et jaotises maht on seotud varukoopiad. Helitugevuse rühmad, mis on seotud varukoopiate ei impordita. Lisaks varukoopia poliitikate helitugevuse rühma jaoks loodud ei impordita. Imporditud rühmade kuvamiseks paremklõpsake sõlme kõrgeim **Helitugevuse rühmade** **ulatus** paanil ja klõpsake nuppu **Lülita imporditud rühmad**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Samm 3: Kontrollige ühendust seadmega

Järgmiste juhiste abil saate kontrollida, et StorSimple seade on ühendatud StorSimple hetktõmmise halduri.

#### <a name="to-verify-the-connection"></a>Ühenduse kinnitamiseks

1. Klõpsake paanil **ulatus** sõlme **seadmed** .

    ![StorSimple hetktõmmise halduri seadme olek](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Kontrollige **tulemuste** paanil. 

   - Kui roheline näidik kuvatakse seadme ikooni ja veerus **olek** kuvatakse **saadaval** , siis seade on ühendatud. 

   - Kui kuvatakse ikoon kuvatakse punane indikaator ja veerus **olek** kuvatakse pole saadaval, siis seade on ühendatud. 

   - Kui **värskendav** kuvatakse veerus **olek** , siis StorSimple hetktõmmise halduri laadib helitugevuse rühmad ja seotud varukoopiate ühendatud seade.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Täiendamine või uuesti StorSimple hetktõmmise haldur

Peate uninstall StorSimple hetktõmmise halduri täielikult enne täiendamine või tarkvara uuesti installima. 

Enne uuesti installimist StorSimple hetktõmmise halduri, olemasoleva StorSimple hetktõmmise halduri andmebaasi varundamine hosti arvutis. See salvestab varukoopia poliitika ja konfiguratsiooni teave, nii et saate hõlpsalt taastada andmed varukoopia põhjal.

Kui teil on täiendamine või uuesti StorSimple hetktõmmise halduri, tehke järgmist.

- Samm 1: Desinstallige StorSimple hetktõmmise haldur 
- Samm 2: StorSimple hetktõmmise halduri andmebaasi varundamine 
- Samm 3: StorSimple hetktõmmise halduri uuesti ja andmebaasi taastamine 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Samm 1: Desinstallige StorSimple hetktõmmise haldur

Järgmiste juhiste abil saate desinstallida StorSimple hetktõmmise halduri.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple hetktõmmise haldur desinstallimine

1. Host arvutites, avage **Juhtpaneel**, käsku **programmid**ja seejärel klõpsake nuppu **programmid ja funktsioonid**.

2. Klõpsake vasakpoolsel paanil nuppu **desinstallimine või muutmine programmi**.

3. Paremklõpsake **StorSimple hetktõmmise haldur**ja seejärel klõpsake käsku **desinstalli**.

4. Käivitub StorSimple hetktõmmise halduri installiprogramm. Klõpsake nuppu **Muuda häälestamine**ja seejärel klõpsake käsku **desinstalli**.

    >[AZURE.NOTE] Kui määratud on mõni MMC protsesside taustal, näiteks StorSimple hetktõmmise halduri või ketas haldus, desinstallimine ei õnnestu ja saate teate sulgege kõik eksemplarid MMC, enne kui proovite programmi desinstallimine. Valige **automaatselt Sulgege rakendused ja proovige uuesti need pärast installimine on lõpule jõudnud**, ja seejärel klõpsake nuppu **OK**.
 
5. Kui desinstallimise protsess on lõpule jõudnud, kuvatakse teade **Eduka häälestus** . Klõpsake nuppu **Sule**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Samm 2: StorSimple hetktõmmise halduri andmebaasi varundamine

Järgmiste juhiste abil saate luua ja salvestada ka koopia StorSimple hetktõmmise Manageri andmebaasi.

#### <a name="to-back-up-the-database"></a>Andmebaasi varundamine

1. Microsoft StorSimple halduse teenuse lõpetamiseks tehke järgmist.

   1. Käivitage Server Manager.

   2. Valige serveri halduri armatuurlaual, klõpsake menüü **Tööriistad** **teenuseid**.

   3. Valige lehel **teenuste** **Microsoft StorSimple halduse teenuse**.

   4. Klõpsake parempoolsel paanil **Microsoft StorSimple halduse teenuse**, klõpsake jaotises **teenuse peatamine**.

        ![StorSimple halduri teenuse peatamine](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Liikuge sirvides C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData on peidetud kaust.

3. XML-i kataloogi faili üles leidnud, kopeerige fail ja turvaliste asukohas või pilves koopia salvestada.

    ![StorSimple varukoopia kataloogi fail](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Taaskäivitage Microsoft StorSimple halduse teenust. 

    1. Valige serveri halduri armatuurlaual, klõpsake menüü **Tööriistad** **teenuseid**.

    2. Valige lehel **teenuste** **Microsoft StorSimple halduse**andmemahtude.

    3. Parempoolsel paanil jaotises **Microsoft StorSimple halduse teenuse**nuppu **teenust uuesti**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Samm 3: StorSimple hetktõmmise halduri uuesti ja andmebaasi taastamine

StorSimple hetktõmmise halduri uuesti installimiseks järgige [uue StorSimple hetktõmmise halduri installimine](#install-a-new-storsimple-snapshot-manager). Seejärel StorSimple hetktõmmise Manageri andmebaasi taastamine järgmiste toimingute abil.

#### <a name="to-restore-the-database"></a>Andmebaasi taastamine

1. Microsoft StorSimple halduse teenuse lõpetamiseks tehke järgmist.

    1. Käivitage Server Manager.

    2. Valige serveri halduri armatuurlaual, klõpsake menüü **Tööriistad** **teenuseid**.

    3. Valige lehel **teenuste** **Microsoft StorSimple halduse teenuse**.

    4. Klõpsake parempoolsel paanil **Microsoft StorSimple halduse teenuse**, klõpsake jaotises **teenuse peatamine**.

2. Liikuge sirvides C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] ProgramData on peidetud kaust.

3. Kataloogi XML-faili kustutamine ja asendage see versioon, mis on salvestatud mõnes varasemas versioonis.

4. Taaskäivitage Microsoft StorSimple halduse teenust. 

    1. Valige serveri halduri armatuurlaual, klõpsake menüü **Tööriistad** **teenuseid**.

    2. Valige lehel **teenuste** **Microsoft StorSimple halduse teenuse**.

    3. Parempoolsel paanil jaotises **Microsoft StorSimple halduse teenuse**nuppu **teenust uuesti**.

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet StorSimple hetktõmmise Manager, minge [mis on StorSimple hetktõmmise haldur?](storsimple-what-is-snapshot-manager.md).

- Kasutajaliidese StorSimple hetktõmmise halduri kohta lisateabe saamiseks minge [StorSimple hetktõmmise halduri kasutajaliides](storsimple-use-snapshot-manager.md).

- StorSimple hetktõmmise halduri kasutamise kohta lisateabe saamiseks minge [Kasutamine StorSimple hetktõmmise halduri hallata oma StorSimple lahendus](storsimple-snapshot-manager-admin.md).
