<properties 
   pageTitle="Seadmete haldamine StorSimple hetktõmmise halduriga | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse ühenduse loomiseks ja haldamiseks StorSimple seadmete StorSimple hetktõmmise halduri MMC lisandmooduli abil."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>Ühenduse loomine ja haldamine StorSimple seadmete StorSimple hetktõmmise halduri abil

## <a name="overview"></a>Ülevaade

Saate sõlmed StorSimple hetktõmmise halduri **ulatus** paanil kinnitada imporditud StorSimple seadme andmete ja värskendamine ühendatud salvestusseadmed. Lisaks **seadmete** sõlm klõpsamisel näete loendit ühendatud seadmete ja vastava oleku teabe **tulemuste** paanil.

![Ühendatud seadmete](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Joonis 1: StorSimple hetktõmmise halduri ühendatud seadet** 

Sõltuvalt **Vaade** , **tulemuste** paanil kuvatakse iga seadme kohta järgmine teave. (Vaate konfigureerimise kohta lisateabe saamiseks avage [menüü Vaade](storsimple-use-snapshot-manager.md#view-menu).


| Tulemite veerg  |Kirjeldus          |
|:----------------|:--------------------| 
| Nimi            | Seadme konfigureeritud Azure klassikaline portaalis nimi|
| Mudel           | Seadme mudeli number|
| Versioon         | Seadmesse installitud tarkvara versioon |
| Olek          | Kas seade on saadaval |
| Viimati sünkroonitud     | Kuupäev ja kellaaeg, millal viimati sünkrooniti seade |
| Järjestikune ei.      | Seadme järjenumbri |
 
Kui paremklõpsate **ulatus** paanil sõlme **seadmed** , saate valida järgmised toimingud:

- Lisamiseks või selle asendamiseks mõne seadme 
- Ühendage seade ja kontrollige import 
- Ühendatud seadmete värskendamine 

Kui klõpsate **seadmete** sõlm ja seejärel paremklõpsake seadme nime **tulemuste** paanil, saate valida järgmised toimingud:

- Seadme autentida 
- Seadme üksikasjade kuvamine 
- Seadme värskendamine 
- Seadme konfiguratsiooni kustutamine 
- Seadme parooli muutmine

>[AZURE.NOTE] Kõik need toimingud on saadaval paanil **toimingud** .
 
Selle õpetuse selgitatakse, kuidas kasutada StorSimple hetktõmmise halduri ühenduse loomiseks ja haldamiseks seadmed ja teha järgmisi toiminguid:

- Lisamiseks või selle asendamiseks mõne seadme 
- Ühendage seade ja kontrollige import 
- Ühendatud seadmete värskendamine 
- Seadme autentida 
- Seadme üksikasjade kuvamine 
- Üksikute seadmes värskendamine 
- Seadme konfiguratsiooni kustutamine 
- Aegunud seadme parooli muuta
- Nurjunud seadme asendamine

>[AZURE.NOTE] Üldteavet StorSimple hetktõmmise halduri liidest kasutades Avage [StorSimple hetktõmmise halduri kasutajaliides](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Lisamiseks või selle asendamiseks mõne seadme

Järgmiste toimingute abil saate lisada või asendamine StorSimple seadme.

#### <a name="to-add-or-replace-a-device"></a>Lisamiseks või seadme asendamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil Paremklõpsake sõlme **seadmed** ja seejärel klõpsake nuppu **seadme konfigureerimine**. Kuvatakse dialoogiboks **seadme konfigureerimine** .

    ![StorSimple seadme konfigureerimine](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. Valige ripploendis **seade** seadme või virtuaalse seadme IP-aadress. 

4. Tippige väljale **parool** seadme Azure klassikaline portaali loodud StorSimple hetktõmmise halduri parool. Klõpsake nuppu **OK**. Seadme, mille te otsib StorSimple hetktõmmise haldur. 

    - Kui seade on saadaval, lisab StorSimple hetktõmmise halduri ühenduse. 

    - Kui seade on mingil põhjusel saadaval, StorSimple hetktõmmise halduri annab tõrketeate. Sulgemiseks klõpsake nuppu **OK** kuvatakse tõrketeade ja seejärel käsku **Tühista** **seadme konfigureerimine** dialoogiboksi sulgemiseks.

## <a name="connect-a-device-and-verify-imports"></a>Ühendage seade ja kontrollige import

Ühendage seade StorSimple ja kontrollige, et kõik olemasolevad helitugevuse rühmad, mis on seotud varukoopiate imporditakse järgmiste toimingute abil.

#### <a name="to-connect-a-device-and-verify-imports"></a>Ühendage seade ja kontrollige import

1. Ühendage seade StorSimple hetktõmmise halduri, järgige juhiseid teemas lisamine või asendamine seade. Kui see loob seade, vastab StorSimple hetktõmmise halduri järgmiselt:

    - Kui seade on mingil põhjusel saadaval, StorSimple hetktõmmise halduri annab tõrketeate. 

   - Kui seade on saadaval, lisab StorSimple hetktõmmise halduri ühenduse. Kui olete seadme, kuvatakse see **otsingutulemite** paan ja välja olek näitab, et seade on **saadaval**. StorSimple hetktõmmise halduri impordib seade, mis tahes Helitugevuse rühmade eeldusel, et helitugevuse rühmad on seotud varukoopiad. Varukoopia poliitikate ei impordita. Helitugevuse rühmad, mis on seotud varukoopiate ei impordita.

2. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

3. Paremklõpsake ülemises **ulatus** paani ja seejärel klõpsake nuppu **Lülita impordi kuvamine**.

    ![Valige Lülita impordi kuvamine](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. Kuvatakse dialoogiboks **Lülita impordi kuvamine** , mis näitab imporditud helitugevuse rühmad ja varukoopiate oleku. Klõpsake nuppu **OK**. 

Pärast helitugevuse rühmade ja varukoopiate on edukalt imporditud, saate hallata neid täpselt samuti nagu sooviksite helitugevuse rühmad ja varukoopiate, mida olete loonud ja konfigureerinud StorSimple hetktõmmise halduriga StorSimple hetktõmmise haldur. 

## <a name="refresh-connected-devices"></a>Ühendatud seadmete värskendamine

StorSimple hetktõmmise halduriga ühendatud seadmete StorSimple sünkroonimiseks tehke järgmist.

####<a name="to-refresh-connected-devices"></a>Ühendatud seadmete värskendamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil Paremklõpsake **seadmed**ja seejärel klõpsake nuppu **Värskenda seadmed**. See sünkroonib ühendatud seadmete StorSimple hetktõmmise halduriga nii, et saate vaadata helitugevuse rühmad ja varukoopiate, sh tehtud täiendusi. 

    ![StorSimple seadmete värskendamine](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
**Värskenda seadmete** toimingu toob kõik uue helitugevuse rühmad ja mis tahes seotud varukoopiaid ühendatud seadmete. Erinevalt **uuesti mahud** toimingu **mahud** sõlm jaoks saadaval, ei **Värskenda seadmete** varukoopia registri taastada.

## <a name="authenticate-a-device"></a>Seadme autentida

Järgmise toiminguga autentida StorSimple seadme StorSimple hetktõmmise halduriga.

#### <a name="to-authenticate-a-device"></a>Seadme autentida

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. Klõpsake paanil **ulatus** **seadmed**.

3. **Tulemuste** paanil Paremklõpsake seadme nimi ja klõpsake **autentimise**.

4. Kuvatakse dialoogiboks **autentimise** . Tippige seadme parool ja seejärel klõpsake nuppu **OK**.

    ![Dialoogiboksi autentida](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Seadme üksikasjade kuvamine

Järgmiste toimingute abil StorSimple seadme üksikasjade kuvamine ja vajadusel sünkroonige uuesti seadme StorSimple hetktõmmise halduriga.

#### <a name="to-view-and-resynchronize-device-details"></a>Üksikasjade kuvamiseks ja sünkroonige seade uuesti.

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. Klõpsake paanil **ulatus** **seadmed**.

3. **Otsingutulemite** paan, paremklõpsake seadme nime ja seejärel klõpsake käsku **üksikasjad**. 

4. kuvatakse dialoogiboks **Mobiilsideseadme üksikasjad** . Kuvatakse see väljal nimi, mudel, versioon, järjenumbri, olek, target iSCSI kvalifitseeritud nimi (IQN) ja viimase sünkroonimise kuupäev ja kellaaeg. 

   - Klõpsake **Resync** seadme sünkroonima.

   - Klõpsake nuppu **OK** või **tühistamiseks** sulgege dialoogiboks.

    ![Mobiilsideseadme üksikasjad](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Üksikute seadmes värskendamine

Järgmise toiminguga StorSimple üksikud StorSimple hetktõmmise halduriga seade uuesti.

#### <a name="to-refresh-a-device"></a>Seadme värskendamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. Klõpsake paanil **ulatus** **seadmed**. 

3. **Tulemuste** paanil Paremklõpsake seadme nime ja seejärel klõpsake nuppu **Värskenda seade**. See sünkroonib seade StorSimple hetktõmmise halduriga. 

## <a name="delete-a-device-configuration"></a>Seadme konfiguratsiooni kustutamine

Järgmiste toimingute abil saate kustutada üksikuid StorSimple seadme konfigureerimisel StorSimple hetktõmmise haldur.

#### <a name="to-delete-a-device-configuration"></a>Seadme konfiguratsiooni kustutamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. Klõpsake paanil **ulatus** **seadmed**. 

3. **Otsingutulemite** paan, paremklõpsake seadme nime ja seejärel klõpsake käsku **Kustuta**. 

4. Kuvatakse järgmine teade. Klõpsake nuppu **Jah** kustutada konfiguratsiooni või **ei** kustutamise tühistamiseks klõpsake nuppu.

    ![Seadme konfiguratsiooni kustutamine](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Aegunud seadme parooli muuta

Sisestage parooli autentida StorSimple seadme StorSimple hetktõmmise halduriga. Konfigureerige see parool seadme häälestamine Windows PowerShelli kasutajaliidese kasutamisel. Siiski saate parooli aegumise. Sel juhul saate Azure klassikaline portaali parooli muutmine. Seejärel, kuna seade on konfigureeritud StorSimple hetktõmmise halduris enne, kui parool on aegunud, tuleb uuesti autentida StorSimple hetktõmmise halduris seade. 

#### <a name="to-change-the-expired-password"></a>Aegunud parooli muutmine

1. Klassikaline portaalis Azure StorSimple halduri teenuse käivitamine.

2. Klõpsake nuppu **seadmed** > seadme**konfigureerimine** .

3. Liikuge kerides jaotiseni StorSimple hetktõmmise haldur. Sisestage parool, mis on 14-15 märki. Veenduge, et parool sisaldab suurtähti, väiketähti, arvulised ja erimärkide segu.

4. Sisestage parool, kinnitage see uuesti.

5. Klõpsake lehe allosas **salvestada** .

#### <a name="to-re-authenticate-the-device"></a>Kinnitamiseks uuesti seade

1. Käivitage StorSimple hetktõmmise haldur.

2. Klõpsake paanil **ulatus** **seadmed**. Konfigureeritud seadmete loend kuvatakse **tulemuste** paanil. 

3. Valige soovitud seade, paremklõpsake seda ja seejärel klõpsake käsku **autentimise**.

4. Klõpsake aknas **autentimise** sisestage uus parool. 

5. Valige soovitud seade, paremklõpsake seda ja valige käsk **Värskenda seade**. See sünkroonib seade StorSimple hetktõmmise halduriga. 

## <a name="replace-a-failed-device"></a>Nurjunud seadme asendamine

Kui StorSimple seadme nurjub ja on asendatud puhkerežiimis olev (Tõrkesiirde) seadet, tehke järgmist ühenduse uuele seadmele ja vaadata seotud varukoopiad.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Pärast Tõrkesiirde uue seadme ühendamiseks

1. Taaskonfigureerimine iSCSI ühendus uuele seadmele. Juhised leiate "juhis 7: kinnitus, Lähtesta ning vorming maht" [Deploy kohapealse StorSimple seadme](storsimple-deployment-walkthrough-u2.md)sisse. 

>[AZURE.NOTE] Kui uus StorSimple seade on vana nimega sama IP-aadress, võib olla võimalik vana konfiguratsiooni ühendada. 

2. Microsoft StorSimple halduse teenuse lõpetamiseks tehke järgmist.

    1. Käivitage Server Manager.

    2. Valige serveri halduri armatuurlaual, klõpsake menüü **Tööriistad** **teenuseid**. 

    3. Valige **Microsoft StorSimple halduse teenuse**aken **teenused** . 

    4. Klõpsake parempoolsel paanil **Microsoft StorSimple halduse teenuse**, klõpsake jaotises **teenuse peatamine**. 

3. Vana seadme seotud konfiguratsiooniteavet eemaldamiseks tehke järgmist. 

    1. Liikuge File Exploreris C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. Kustutage failid BACatalog kausta. 

4. Taaskäivitage Microsoft StorSimple halduse teenust. 

    1. Valige serveri halduri armatuurlaual, klõpsake menüü **Tööriistad** **teenuseid**. 

    2. Valige **Microsoft StorSimple halduse teenuse**aken **teenused** . 

    3. Parempoolsel paanil jaotises **Microsoft StorSimple halduse teenuse**nuppu **teenust uuesti**. 

5. Käivitage StorSimple hetktõmmise haldur. 

6. Konfigureerimine uue StorSimple seadet, täitke juhised samm 2: Ühendage seade StorSimple [juurutamine StorSimple hetktõmmise](storsimple-snapshot-manager-deployment.md)Manager. 

7. Paremklõpsake sõlme ülataseme **ulatus** paanil (näites StorSimple hetktõmmise Manager) ja seejärel klõpsake nuppu **Lülita impordi kuvamine**. 

8. Kui imporditud helitugevuse rühmade ja varukoopiaid on nähtavad StorSimple hetktõmmise halduris kuvatakse teade. Klõpsake nuppu **OK**. 

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [StorSimple hetktõmmise halduri hallata oma StorSimple lahenduse kasutamise](storsimple-snapshot-manager-admin.md)kohta.
- Siit saate teada, [StorSimple hetktõmmise halduri vaadata ja hallata mahud kasutamise](storsimple-snapshot-manager-manage-volumes.md)kohta.

