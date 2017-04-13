<properties
   pageTitle="StorSimple virtuaalse massiiv - sätte VMware juurutamine"
   description="Teine õppeteema StorSimple virtuaalse massiiv juurutamise sarja hõlmab ettevalmistamise VMware virtuaalse seadme."
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
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Juurutamine StorSimple virtuaalse massiiv – virtuaalse massiiv VMware ettevalmistamine

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Ülevaade 
Õppeteema ettevalmistamise kehtib StorSimple virtuaalse massiivi (tuntud ka kui StorSimple kohapealse virtuaalse või StorSimple virtuaalse seadmed) töötava märts 2016 üldiselt kättesaadav (GA) väljaanne. Selle õpetuse kirjeldatakse, kuidas ette valmistada ja luua ühenduse StorSimple virtuaalse massiiv host operatsioonisüsteemis VMware ESXi 5,5 ja üle. See artikkel kehtib juurutamise StorSimple virtuaalse massiivi Azure klassikaline portaali kui ka Microsoft Azure'i Government Cloud.

Teil on vaja administraatoriõigusi ettevalmistamine ja virtuaalse seadmega ühendada. Ettevalmistamise ja algne häälestus võib kuluda umbes 10 minutit lõpuleviimiseks.


## <a name="provisioning-prerequisites"></a>Ettevalmistamise eeltingimused

Siit leiate eeltingimused ettevalmistamise host operatsioonisüsteemis VMware ESXi 5,5 ja üle virtuaalse seadme.

### <a name="for-the-storsimple-manager-service"></a>StorSimple halduri teenuse kohta

Enne alustamist veenduge, et:

-   Kõigi vajalike toimingute tegemiseks [ettevalmistamine portaali StorSimple virtuaalse array](storsimple-ova-deploy1-portal-prep.md)on lõpule viidud.

-   Virtuaalne seadme pilt on Azure portaalist allalaaditud jaoks VMware. Lisateavet leiate teemast [Samm 3: alla laadida pildi virtuaalse seadme](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>StorSimple virtuaalse seadme 

Enne juurutamist virtuaalne seadme, veenduge, et:

-   Teil on juurdepääs host operatsioonisüsteemis Hyper-V (2008 R2 või uuem versioon) mis võib olla seadme säte kasutada.

-   Host süsteem on võimalus suunata ettevalmistamise virtuaalse seadme järgmistest allikatest:

    -   Vähemalt 4 valdkond.

    -   Vähemalt 8 GB RAM-i.

    -   Ühe võrgu liidese.

    -   500 GB virtuaalse ketta süsteemi andmete jaoks.

### <a name="for-the-network-in-datacenter"></a>Võrgu andmekeskuses 

Enne alustamist veenduge, et:

-   Olete läbi võrgu nõuetele StorSimple virtuaalse seadme juurutamiseks ja konfigureerinud andmekeskuse võrgu vastavalt nõuetele. Lisateavet leiate teemast [StorSimple virtuaalse massiiv süsteeminõuded](storsimple-ova-system-requirements.md).

## <a name="step-by-step-provisioning"></a>Samm-sammult ettevalmistamine 

Ette ja virtuaalse seadmega ühendada, peate tegema järgmised toimingud:

1.  Tagada, et host süsteem on piisavalt ressursse, mis vastavad minimaalsed virtuaalse seadme.

2.  Ettevalmistamise virtuaalse seadme oma hypervisor.

3.  Käivitage virtuaalse seade ja saada IP-aadress.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Samm 1: Veenduge, et host süsteem vastab minimaalse virtuaalse seade

Virtuaalne seadme loomiseks peate:

-   Juurdepääs host operatsioonisüsteemis VMware ESXi serveri 5,5 ja kohal.

-   VMware vSphere kliendi arvutisse ESXi host haldamiseks.

    -   Vähemalt 4 valdkond.

    -   Vähemalt 8 GB RAM-i.

    -   Ühe võrgu liidese võimeline marsruutimise Interneti-liikluse võrku ühendatud. Minimaalne Interneti-läbilaskevõime peaks olema 5 Mbps seadme optimaalse kasutamise võimaldamiseks.

    -   500 GB virtuaalse ketta andmete jaoks.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Samm 2: Ettevalmistamine hypervisor virtuaalse seadme

Järgmiste toimingute ettevalmistamise virtuaalse seadme oma hypervisor.

1.  Virtuaalne seadme pilt arvutisse kopeerida. See on Azure klassikaline portaali kaudu allalaaditud pilt. 
    1.  Veenduge, et see on alla laaditud uusima pildifaili. Kui laadisite pildi varasemas versioonis, laadige see uuesti, et tagada, et teil on uusim pilt. Viimane pilt on kaks faili (üks) asemel.
    2.  Märkige üles asukoht, kuhu kopeerisite pilt, kui kasutate seda hiljem sisse juhiseid.

2.  Logige sisse vSphere kliendi ESXi server. Peate olema administraatoriõigused virtuaalse masina loomiseks.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  Valige vasakul paanil jaotises varude vSphere klient ESXi Server.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  Esmalt üles laadida soovitud VMDK ESXi server. Liikuge parempoolsel paanil vahekaarti **konfigureerimine** . Klõpsake jaotises **riistvara**, valige **salvestusruumi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  Valige parempoolsel paanil jaotises **Datastores**, andmesalve, kus soovite üles laadida soovitud VMDK. Andmesalve peab olema ketast OS- ja vaba ruumi.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Paremklõpsake ja valige **Sirvi andmesalve**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  Kuvatakse **Andmesalve brauseriaknas** kuvada.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Klõpsake tööriistaribal, ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) ikooni Loo uus kaust. Määrake kausta nimi ja märkige see. Kasutage seda kausta nimi hiljem virtuaalse masina (soovitatav parimaks) loomisel. Klõpsake nuppu **OK**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  Uue kausta kuvatakse vasakul paanil **Andmesalve brauseris**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Klõpsake ikooni üles ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) ja valige **Faili üles laadida**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  Nüüd peaks sirvida ja osutage VMDK alla laaditud failid. Ilmneb kaks faili. Valige faili üles laadida.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Klõpsake nuppu **Ava**. See käivitab nüüd määratud andmesalve VMDK fail üles. Võib kuluda mitu minutit faili üles laadida.


1.  Kui üleslaadimine on lõpule jõudnud, kuvatakse andmesalve loodud kausta fail. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Peate nüüd sama andmesalve teise VMDK faili üles laadida.

1.  Naaske vSphere kliendi akna. ESXi serveriga on valitud, paremklõpsake ja valige **Uus virtuaalse masina**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  **Saate luua uue virtuaalse masina** aken. Lehel **Konfiguratsioon** suvand **kohandatud** . Klõpsake nuppu **edasi**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Määrake lehel **nime ja asukoha** virtual arvuti nimi. Sellise nimega peaksid olema samad eelnevalt kirjeldatud juhise 8 määratud kausta nimi (soovitatav parim).

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Valige lehel **salvestusruumi** lisamine andmesalve, mida soovite kasutada oma VM ette valmistada.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Valige lehel **Virtuaalse masina versioon** **virtuaalse masina versioon: 8**. Pange tähele, et versioonid 8 – 11 on toetatud.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  Valige lehel **külaline operatsioonisüsteemi** **Windows** **külaline operatsioonisüsteem** . **Versiooni**, valige ripploendist, **Microsoft Windows Server 2012 (64-bitine)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  Lehel **protsessorid** reguleerida **virtuaalse sockets arv** ja **arvu valdkond virtuaalse turvasoklite kohta** nii, et **koguarv valdkond** 4 (või rohkem). Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Määrake lehel **mälu** 8 GB või rohkem RAM-i. Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Määrake lehel **Network** võrgu liidesed arv. Miinimumnõuetele on ühe võrgu liidese.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  Lehel **SCSI kontrolleril** , nõustuge vaikevalikuga **LSI loogika SAS kontrolleril**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Valige lehel **Valige ketas** **Kasuta olemasolevat virtuaalse ketast**. Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  Klõpsake lehel **Valige olemasoleva vaba** **Kettaruumi faili tee**, klõpsake nuppu **Sirvi**. **Sirvige Datastores** dialoogiboks avatakse. Liikuge asukohta, kus te selle VMDK üles laadida. Nüüd kuvatakse ainult üks fail andmesalve nagu kaks Algselt üleslaaditud failid on ühendatud. Valige fail ja klõpsake nuppu **OK**. Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Lehel **Täpsemad suvandid** , nõustuge vaikevalikuga ja klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  **Valmis lõpuleviimine** lehel vaadata kõik sätted, mis on seotud uue virtuaalse masina. Märkige ruut **virtuaalse masina sätteid enne lõpetamist redigeerida**. Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Leidke lehel **Virtuaalmasinates atribuudid** vahekaarti **riistvara** seadme riistvara. Valige **uue kõvaketta**. Klõpsake nuppu **Lisa**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Avab akna **Lisa riistvara** . Valige **kõvaketta** lehe **Seadme tüüp** , valige jaotises **Vali tüüpi seade, mida soovite lisada**, ja klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Klõpsake lehel **Valige ketas** , klõpsake käsku **Loo uus virtuaalse ketta**. Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  Klõpsake lehe **loomine kettal** , **Ketta suurus** muutmine (500 GB või rohkem). Valige jaotises **Ketta ettevalmistamise**, **Õhuke säte**. Klõpsake nuppu **edasi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Lehel **Täpsemad suvandid** , nõustuge vaikevalikuga.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  **Valmis lõpuleviimine** lehel läbivaatus ketta suvandid. Klõpsake nuppu **valmis**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Kohe tagasi lehe virtuaalse masina atribuudid. Uue kõvaketta lisatakse virtual arvuti. Klõpsake nuppu **valmis**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  Liikuge koos parempoolsel paanil valitud virtual arvuti vahekaarti **Kokkuvõte** . Vaadake üle virtuaalse arvuti sätteid.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

Nüüd on ette valmistatud virtual arvuti. Järgmiseks on selles arvutis power ja saada IP-aadress.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Samm 3: Alustada virtuaalse seade ja saada IP

Järgmiste toimingute virtuaalse seadme käivitamiseks ja ühenduse loomiseks.

#### <a name="to-start-the-virtual-device"></a>Virtuaalne seadme käivitamine

1.  Käivitage virtuaalse seade. Valige oma seade ja paremklõpsake kontekstimenüü avab vSphere Configuration Manager, vasakul paanil. Valige **Power** ja seejärel valige **Power kohta**. See peaks power virtual teie arvutis. Saate vaadata oleku vSphere kliendi paanil all **Tehtud toimingud** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  Funktsiooni häälestustoimingute võtab paar minutit. Kui seade töötab, liikuge vahekaardile **konsooli** . Saada seadme logige klahvikombinatsiooni Ctrl + Alt + Delete. Teise võimalusena saate kursoriga aknas konsooli ja vajutage klahvikombinatsiooni Ctrl + Alt + Insert. Kasutaja vaikimisi *StorSimpleAdmin* ja vaikimisi parool on *parool1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Turvalisuse huvides seadme administraatori parooli aegub esimese Logi sisse. Teil palutakse parooli muuta.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Sisestage parool, mis sisaldab vähemalt 8 märki. Parool peab sisaldama 3 4 nende nõuete: suurtähti, väiketähti, arvulised ja teisiti. Sisestage parool selle kinnitamiseks uuesti. Kuvab teate, et parool on muutunud.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Pärast seda, kui parool on muudetud, virtuaalse seade võib-olla taaskäivitama. Oodake, kuni soovitud lõpuleviimiseks taaskäivitage arvuti. Windows PowerShelli konsooli seadme võidakse kuvada koos edenemise riba.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  Samme 6 – 8 rakenduvad ainult siis, kui käivitamist kuni mitte DHCP keskkonnas. Kui olete DHCP keskkonnas, jätke need sammud vahele ja jätkake 9. Kui teie seadme mitte DHCP keskkonnas käivitatud, kuvatakse järgmisel kuvatõmmisel. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Nüüd peate võrgu konfigureerimine.

1.  Kasutage funktsiooni `Get-HcsIpAddress` käsk võrgu liidesed virtuaalse seadmes sisse lülitatud. Kui teie seade on lubatud ühe võrgu kasutajaliides, määratud selle liidese vaikenimi on `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Kasutage funktsiooni `Set-HcsIpAddress` cmdlet-käsu võrgu konfigureerimine. Näide on allpool näidatud:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Pärast algne häälestus on lõpetatud ja seade on käivitatud, kuvatakse seade banner tekst. Kirjutage IP-aadress ja URL-i banner tekst kuvatakse seadme haldamine. Kasutate veebi UI virtuaalse seadme ühenduse loomiseks ja selle kohaliku häälestamine ja IP-aadress.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Valikuline) Seda juhist täitke ainult siis, kui juurutate seadme Government pilveteenuses. Nüüd lubate Ameerika Ühendriikide Federal teabe töötlemise Standard (FIPS) režiimi seadmes. FIPS 140 standard määratleb cryptographic algoritmide kohta meile Federal government arvutisüsteemide tundliku teabe kaitse kinnitama kasutamiseks.
    1. Luba FIPS režiim, käivitage järgmine cmdlet-käsk:
        
        `Enter-HcsFIPSMode`

    2. Taaskäivitage seade, kui teil on lubatud FIPS-režiimis, et cryptographic valideerimised muudatuste jõustumiseks.

        > [AZURE.NOTE] Saate lubada või keelata FIPS režiimi oma seadmes. Vahelduvad FIPS ja FIPS režiimi vahel pole toetatud.


Kui teie seade ei vasta nõuetele minimaalne konfiguratsioon, kuvatakse tõrge banner tekst (vt allpool). Peate seadme konfiguratsiooni muutmiseks nii, et see on piisavalt ressursse, mis vastavad miinimumnõuetele. Seejärel taaskäivitage ja ühendage seade. Minimaalne konfiguratsioon nõuetele viidata [Samm 1: tagada host süsteem vastab minimaalse virtuaalse seadme](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Kui teil näo algsel konfigureerimisel kohalik web Kasutajaliidese abil muu viga, vaadake järgmist töövoogude [haldamine oma StorSimple virtuaalse massiiv](storsimple-ova-web-ui-admin.md)kasutamise kohaliku web UI.

-   Käivitage diagnostika kontrollib [web UI häälestamise](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)tõrkeotsing.

-   [Genereeri log paketi ja logifailid vaade](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Järgmised sammud

-   [Häälestada oma StorSimple virtuaalse massiiv faili Server](storsimple-ova-deploy3-fs-setup.md)

-   [Seadistada oma StorSimple virtuaalse massiivi iSCSI serveriks](storsimple-ova-deploy3-iscsi-setup.md)

