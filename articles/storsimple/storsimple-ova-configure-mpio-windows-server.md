<properties 
   pageTitle="Konfigureerimine MPIO StorSimple virtuaalse massiiv hostis | Microsoft Azure'i"
   description="Kirjeldab, kuidas konfigureerida oma StorSimple virtuaalse massiiv ühendatud Host töötab Windows Server 2012 R2 silma i/o (MPIO)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Windows Server hosti StorSimple virtuaalse massiivi silma i/o konfigureerimine

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas installida oma Windows Server hosti silma i/o funktsioon (MPIO), rakendada teatud konfiguratsioonisätted StorSimple ainult mahud ja seejärel kontrollige MPIO StorSimple maht. Soovitud toiming eeldab, et oma StorSimple 1200 virtuaalse massiivis kahe võrgu liidesed on ühendatud Windows Server host koos kahe võrgu liidesed. Selles artiklis sisalduv teave kehtib ainult virtuaalse massiiv. StorSimple 8000 seeria seadmed kohta lisateabe saamiseks minge [StorSimple Host MPIO konfigureerimine](storsimple-configure-mpio-windows-server.md). 

Funktsioon MPIO Windows Server aitab koostamine väga kättesaadav, tõrketaluvusega salvestusruumi konfiguratsioone. MPIO kasutab liigsete füüsilise tee komponendid-adapterit, kaabel ja parameetrid – server ja mäluseade loogilise teed loomiseks. Kui komponendi tõrke, põhjustades loogiline tee nurjumise, kasutab multipathing loogika mõne muu tee I/O nii, et rakenduste pääsete endiselt juurde oma andmeid. Lisaks teie konfiguratsioonist MPIO saate ka jõudluse parandamiseks ümberkorraldamine laadi üle kõik need teed. Lisateavet leiate teemast [MPIO ülevaade](https://technet.microsoft.com/library/cc725907.aspx "MPIO ülevaade ja funktsioonid").  

Kõrge-saadavus StorSimple lahenduse, konfigureerida MPIO Windows Server hosts ühendatud teie StorSimple 1200 virtuaalse massiiv (nimetatakse ka kohapealse virtuaalse seade). Seejärel saate hosti serverid luba, link, võrgu või kasutajaliidese tõrge. 

Peate MPIO konfigureerimiseks järgmist: 

- Konfiguratsiooni eeltingimused

- Samm 1: Installi MPIO Windows Server host

- Samm 2: StorSimple mahud MPIO konfigureerimine

- Samm 3: Mount StorSimple mahud hostis

Järgmistes lõikudes käsitletakse iga ülaltoodud juhiseid.


## <a name="prerequisites"></a>Eeltingimused

Selles jaotises details konfiguratsiooni eeltingimused Windows Server hosti ja oma virtuaalse massiiv.

### <a name="on-windows-server-host"></a>Windows Server host

-  Veenduge, et oma Windows Server hosti on 2 võrgu liidesed lubatud.


### <a name="on-storsimple-virtual-array"></a>StorSimple virtuaalse massiivi

- Virtuaalne massiiv peaks olema konfigureeritud iSCSI serveriks. Lisateabe saamiseks lugege teemat [häälestamine virtuaalse massiivi iSCSI serveriks nimega](storsimple-ova-deploy3-iscsi-setup.md). Ühe või mitme võrgu liidesed peaks olema lubatud massiiv.   

- Võrgu liidesed oma virtuaalse massiivi peaks olema kättesaadav Windows Server host.

- Üks või mitu draivi tuleks luua oma StorSimple virtuaalse massiiv. Lisateabe saamiseks vt [draivi lisamine](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) oma StorSimple 1200 virtuaalse massiiv. Selle toimingu lõime virtuaalse massiivi 3 maht (2 kohalikult kinnitatud ja 1 astmeline maht nagu on näidatud allpool).
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Riistvara konfiguratsioon StorSimple virtuaalse massiiv

Joonisel on kõrge-saadavus riistvara konfiguratsiooni ja koormust tasakaalustavad multipathing oma Windows Server hosti ja StorSimple virtuaalse massiivi kasutanud selle protseduuri käigus pakutakse.  

![MPIO riistvara konfiguratsioon](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Nagu on näidatud joonisel:

- Teie StorSimple virtuaalse ette valmistatud Hyper-v massiiv ühe sõlme aktiivne iSCSI server on konfigureeritud.

- Kahe virtuaalse võrgu liidesed on lubatud teie massiiv. Kohalik Web Kasutajaliidese oma 1200 virtuaalse massiivi, veenduge, et kahe võrgu liidesed on lubatud navigeerides **Võrgu sätteid** nagu allpool näidatud:

    ![Võrgu liidesed 1200 sisse lülitatud](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Pange tähele IPv4 aadressid lubatud võrgu liideste (Ethernet Ethernet 2 vaikimisi) ja Host edaspidiseks kasutamiseks salvestada.

- Windows Server Server on lubatud kaks võrgu liidesed. Kui olete ühendatud liidesed host ja massiivi sama alamvõrgu, siis tekib 4 teed saadaval. See oli see selle protseduuri käigus pakutakse. Kui massiivi ja host kasutajaliidese iga võrgu liides on erinevate IP alamvõrgu (ja mitte marsruuditavaid), siis ainult 2 teed on siiski saadaval.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Samm 1: Installi MPIO Windows Server host

MPIO on valikuline funktsioon Windows Server ja vaikimisi pole installitud. See peaks olema installitud funktsioonina Server Manageri kaudu. Selle funktsiooni installimiseks oma Windows Server hosti tehke järgmist.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Samm 2: StorSimple mahud MPIO konfigureerimine

MPIO peab olema konfigureeritud StorSimple maht kindlaks teha. MPIO StorSimple mahud äratuntavaks konfigureerimiseks tehke järgmist.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Samm 3: Mount StorSimple mahud hostis

Pärast Windows Server on konfigureeritud MPIO, draivid StorSimple massiivile saab kinnitada ja saate siis ära MPIO koondamise jaoks. Maht ühendada, tehke järgmist.

#### <a name="to-mount-volumes-on-the-host"></a>Ühendada mahud hostis

1. Avage Windows Server hosti **iSCSI algataja atribuutide** aken. Klõpsake **Server Manager > Armatuurlaud > Tööriistad > iSCSI algataja**.
2. Dialoogiboksis **iSCSI algataja atribuudid** klõpsake vahekaarti Discovery, ja klõpsake **Avastamine Target portaalis**.
3. Dialoogiboksis **Avastamine Target portaalis** , tehke järgmist.
    
    - Sisestage oma StorSimple virtuaalse massiivi esimese lubatud võrgu liidese IP-aadress. Vaikimisi oleks **Ethernet**. 
    - Klõpsake nuppu **OK** , et naasta dialoogiboksi **iSCSI algataja atribuudid** .

    >[AZURE.IMPORTANT] **Kui kasutate privaatvõrk iSCSI ühendusi, sisestage andmed portide, mis on ühendatud privaatvõrgu IP-aadress.**

4. Korrake juhiseid 2 – 3 teise võrgu kasutajaliidese (nt Ethernet 2) oma massiivi. 

5. Valige vahekaart **sihtkohtade** **iSCSI algataja atribuutide** dialoogiboksis. Oma virtuaalse array, peaksite iga helitugevuse pind siht **Avastanud sihtkohtade**jaotises nimega. Sel juhul tuleks avastanud kolm sihtkohtade (vastavalt kolm).

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Klõpsake nuppu **Loo ühendus** luua mõne iSCSI seansi oma StorSimple massiiv. Kuvatakse dialoogiboks **ühenduse sihtkoht** . Märkige ruut **Luba mitme tee** . Klõpsake nuppu **Täpsemalt**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. Dialoogiboksis **Täpsemad sätted** tehke järgmist.                                       
    -    Valige ripploendis **Kohaliku adapterit** **Microsoft iSCSI algataja**.
    -    Valige ripploendist **Algataja IP** hosti IP-aadress.
    -    Valige ripploendist **Portaali Target** IP IP massiiv liidese.
    -    Klõpsake nuppu **OK** , et naasta dialoogiboksi **iSCSI algataja atribuudid** .

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Klõpsake nuppu **Atribuudid**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. Klõpsake dialoogiboksis **Atribuudid** **Seansi lisada**.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. Dialoogiboksis **ühenduse Target** märkige ruut **Luba mitme tee** . Klõpsake nuppu **Täpsemalt**.
11. Dialoogiboksis **Täpsemad sätted** järgmist.                                        
    -  Valige ripploendist **kohaliku adapterit** Microsoft iSCSI algataja.
    -  Valige ripploendist **Algataja IP** vastav Host IP-aadress. Sel juhul on kaks võrgu liidesed massiiv ühendamine ühe võrgu kasutajaliidese hosti. Seetõttu on selle kasutajaliidese sama kui esimese seansi jaoks.
    -  Valige ripploendist **Target portaali IP** teise andmeid kasutajaliidese massiivi lubatud IP-aadress.
    -  Klõpsake nuppu **OK** , et naasta dialoogiboksi iSCSI algataja atribuudid. Olete lisanud teise seansi sihtkohas.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Pärast lisamist **iSCSI algataja atribuutide** dialoogiboksis soovitud seansid (teed), valige siht ja klõpsake käsku **Atribuudid**. Dialoogiboksi **Atribuudid** vahekaardil seansid Märkus nelja seansi identifikaatoritest, mis vastavad tee võimalike permutatsioonide. Seansi lõpetamine tühistamiseks valige seansi identifikaator kõrval olev ruut ja seejärel klõpsake nuppu **Katkesta ühendus**.
 
    - Esitatud jooksul seansid seadmete vaatamiseks valige vahekaarti **seadmed** . Klõpsake valitud seadme MPIO poliitika konfigureerimiseks **MPIO**. Funktsiooni **
    -  Üksikasjad** kuvatakse dialoogiboks. Klõpsake soovitud **MPIO** menüü, saate valida sobiva **laadi saldo poliitika** sätted. Saate vaadata ka selle **aktiivne** või **puhkerežiimis olev ** tee tüüp.

10. Korrake juhiseid 8-11 lisamiseks sihtkohas täiendavad seansid (teed). Kahe liidesed host ja kahe massiivi virtuaalse, saate lisada kokku neli seanssi iga sihtrühma. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. Peate korrake neid juhiseid iga maht (pind siht nimega).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. Avage **Arvuti haldamine** navigeerimise **Server Manager > Armatuurlaud > arvuti haldamine**. Klõpsake vasakpoolsel paanil nuppu **salvestusruum > ketta halduse**. StorSimple virtuaalse massiivile draivid, mis on nähtavad selle hosti kuvatakse jaotises **Ketta haldamine** uue ketta.

13. Lähtesta ketas ja looge uus draiv. Valige vorming käigus määramata ühiku maht (AUS) 64 KB. Korrake toimingut saadaval mahud.

    ![Ketas haldus](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. **Ketas haldus**, paremklõpsake **kettale** ja valige **Atribuudid**.

15. **Mitme tee ketas seadme atribuutide** dialoogiboksis vahekaarti **MPIO** .

    ![Ketas atribuudid](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. **DSM nimi** jaotises nuppu **üksikasjad** ja veenduge, et väärtused, mis on parameetrid. Väärtused, mis on:

    - Tee kinnitamine perioodi = 30
    - Uute katsete arv = 3
    - KPN eemaldamine perioodi = 20
    - Proovige intervall = 1
    - Tee kinnitamine lubatud = märkimata.

    >[AZURE.NOTE] **Muutke vaikimisi parameetrid.**


## <a name="next-steps"></a>Järgmised sammud

Lisateavet [StorSimple halduri teenuse abil saate hallata oma StorSimple virtuaalse massiiv](storsimple-ova-manager-service-administration.md).
 
