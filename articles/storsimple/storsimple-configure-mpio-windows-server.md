<properties 
   pageTitle="Seadme StorSimple MPIO konfigureerimine | Microsoft Azure'i"
   description="Kirjeldab, kuidas konfigureerida StorSimple seadme ühendatud Host töötab Windows Server 2012 R2 silma i/o (MPIO)."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>Seadme StorSimple silma i/o konfigureerimine

Microsoft ehitatud silma i/o (MPIO) funktsiooni Windows Serveri spikker koostamine väga kättesaadav, tõrketaluvusega SAN konfiguratsioone tugi. MPIO kasutab liigsete füüsilise tee komponendid – adapterit, kaabel ja parameetrid – luua server ja mäluseade loogilise teed. Kui komponent tõrke, põhjustab loogiline tee nurjumise, kasutab multipathing loogika mõne muu tee I/O nii, et rakenduste pääsete endiselt juurde oma andmeid. Lisaks teie konfiguratsioonist MPIO saate ka jõudluse parandamiseks ümberkorraldamine laadi üle kõik need teed. Lisateavet leiate teemast [MPIO ülevaade](https://technet.microsoft.com/library/cc725907.aspx "MPIO ülevaade ja funktsioonid").  

Kõrge-saadavus StorSimple lahenduse, tuleks MPIO konfigureeritud StorSimple seadmes. Kui MPIO installitakse teie hosti serverites, kus töötab Windows Server 2012 R2, siis saate serverid luba link, võrgu või kasutajaliidese tõrge. 

MPIO on valikuline funktsioon Windows Server ja vaikimisi pole installitud. See peaks olema installitud funktsioonina Server Manageri kaudu. See teema kirjeldab toiminguid, saate installida ja kasutada funktsiooni MPIO hostis, kus töötab Windows Server 2012 R2 eemale ja StorSimple füüsilise seadmega ühendatud.

>[AZURE.NOTE] **See toiming on mõeldud ainult StorSimple 8000 sarja. Praegu ei toetata MPIO StorSimple virtuaalse seade.**

Peate seadme StorSimple MPIO konfigureerimiseks tehke:

- Samm 1: Installi MPIO Windows Server host

- Samm 2: StorSimple mahud MPIO konfigureerimine

- Samm 3: Mount StorSimple mahud hostis

- Samm 4: Kõrge-saadavus MPIO konfigureerimine ja tasakaalustamiseks laadimine

Järgmistes lõikudes käsitletakse iga ülaltoodud juhiseid.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Samm 1: Installi MPIO Windows Server host

Selle funktsiooni installimiseks oma Windows Server hosti tehke järgmist.

#### <a name="to-install-mpio-on-the-host"></a>Host MPIO installimiseks

1. Avage oma Windows Server hosti Server Manager. Vaikimisi Server Manager algab siis, kui arvutis, kus töötab Windows Server 2012 R2 või Windows Server 2012 siseneb administraatorite rühma liige. Kui Server haldur pole juba avatud, klõpsake **Alustamine > Server Manager**.
![Server Manager](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Klõpsake **Server Manager > Armatuurlaud > Rollid ja funktsioonide lisamine**. Käivitub viisard **lisada rollid ja funktsioonid** .
![Rollide ja funktsioonide viisard 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. **Rollide ja funktsioonide lisamine** viisardis tehke järgmist.

    - **Enne alustamist** lehel klõpsake nuppu **edasi**.
    - Klõpsake lehel **Valige installi tüübi** nõustuda vaikimisi **rolli või funktsiooni** installi. Klõpsake nuppu **edasi**. ![Rollid ja funktsioonid viisardi 2 lisamine](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - Valige lehel **valige sihtkoht server** **Valige server server kaustast**. Kui teie hosti server automaatselt avastatakse. Klõpsake nuppu **edasi**.
    - Klõpsake lehel **Valige serverirollide** nuppu **Järgmine**.
    - **Valige** lehel Valige **Silma i/o**ja klõpsake nuppu **edasi**. ![Rollid ja funktsioonid viisardi 5 lisamine](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - Lehel **kinnitamine installimise valikud** valiku kinnituseks ja seejärel valige **uuesti sihtkoha server vajadusel automaatselt**, nagu allpool näidatud. Klõpsake nuppu **Installi**. ![Rollid ja funktsioonid viisardi 8 lisamine](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - Kui installimine on lõpule jõudnud, teavitatakse teid sellest. Klõpsake viisardi sulgemiseks **sulgeda** . ![Rollid ja funktsioonid viisardi 9 lisamine](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Samm 2: StorSimple mahud MPIO konfigureerimine

MPIO peab olema konfigureeritud StorSimple maht kindlaks teha. MPIO StorSimple mahud äratuntavaks konfigureerimiseks tehke järgmist.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>Konfigureerida MPIO StorSimple mahud

1. Avage **MPIO konfigureerimine**. Klõpsake **Server Manager > Armatuurlaud > Tööriistad > MPIO**.

2. **MPIO atribuutide** dialoogiboksis valige vahekaart **Avastamine mitme teed** .

3. Valige **Lisa tugiteenuste iSCSI seadmete jaoks**ja seejärel klõpsake nuppu **Lisa**.  
![MPIO atribuudid avastamine mitme teed](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Taaskäivitage server, kui seda küsitakse.
5. Klõpsake dialoogiboksis **MPIO atribuudid** vahekaarti **MPIO seadmed** . Klõpsake nuppu **Lisa**.
    </br>![MPIO atribuudid MPIO seadmed](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. Dialoogiboksis **Lisa MPIO tugiteenuste** jaotises **seadme riistvara ID-d**, sisestage oma seadme järjenumbri. Saate seadme järjenumbri juurdepääs teie haldur StorSimple teenuse ja liikumine **seadmed > armatuurlaua**. Õige seadme armatuurlaua paanil **Kiirülevaate lühidalt** kuvatakse seadme järjenumbri.
    </br>![Lisage MPIO tugi](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Taaskäivitage server, kui seda küsitakse.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Samm 3: Mount StorSimple mahud hostis

Pärast Windows Server on konfigureeritud MPIO, draivid loodud StorSimple seadme saab kinnitada ja saate siis ära MPIO koondamise jaoks. Maht ühendada, tehke järgmist.

#### <a name="to-mount-volumes-on-the-host"></a>Ühendada mahud hostis

1. Avage Windows Server hosti **iSCSI algataja atribuutide** aken. Klõpsake **Server Manager > Armatuurlaud > Tööriistad > iSCSI algataja**.
2. Dialoogiboksis **iSCSI algataja atribuudid** klõpsake vahekaarti Discovery, ja klõpsake **Avastamine Target portaalis**.
3. Dialoogiboksis **Avastamine Target portaalis** , tehke järgmist.
    
    - Sisestage andmed pordi StorSimple seadme IP-aadress (nt sisestage andmed 0).
    - Klõpsake nuppu **OK** , et naasta dialoogiboksi **iSCSI algataja atribuudid** .

    >[AZURE.IMPORTANT] **Kui kasutate privaatvõrk iSCSI ühendusi, sisestage andmed portide, mis on ühendatud privaatvõrgu IP-aadress.**

4. Korrake juhiseid 2 – 3 teise võrgu kasutajaliidese (nt andmete 1) seadmes. Pidage meeles, et liideste peaks olema lubatud iSCSI. Lisateavet leiate teemast [Muuda võrgu liidesed](storsimple-modify-device-config.md#modify-network-interfaces).
5. Valige vahekaart **sihtkohtade** **iSCSI algataja atribuutide** dialoogiboksis. Peaksite nägema StorSimple seadme target IQN **Avastanud sihtkohtade**alusel.
 ![iSCSI algataja sihtkohtade vahekaardil atribuudid](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Klõpsake nuppu **Loo ühendus** luua mõne iSCSI seansi StorSimple seadmega. Kuvatakse dialoogiboks **ühenduse sihtkoht** .

7. Dialoogiboksis **ühenduse Target** märkige ruut **Luba mitme tee** . Klõpsake nuppu **Täpsemalt**.

8. Dialoogiboksis **Täpsemad sätted** tehke järgmist.                                       
    -    Valige ripploendist **Kohaliku adapterit** **Microsoft iSCSI algataja**.
    -    Valige ripploendist **Algataja IP** hosti IP-aadress.
    -    Valige ripploendist **Portaali Target** IP IP seadme liidest.
    -    Klõpsake nuppu **OK** , et naasta dialoogiboksi **iSCSI algataja atribuudid** .

9. Klõpsake nuppu **Atribuudid**. Klõpsake dialoogiboksis **Atribuudid** **Seansi lisada**.
10. Dialoogiboksis **ühenduse Target** märkige ruut **Luba mitme tee** . Klõpsake nuppu **Täpsemalt**.
11. Dialoogiboksis **Täpsemad sätted** järgmist.                                        
    -  Valige ripploendist **kohaliku adapterit** Microsoft iSCSI algataja.
    -  Valige ripploendist **Algataja IP** vastav Host IP-aadress. Sel juhul on kaks võrgu liidesed seadme ühendamine ühe võrgu kasutajaliidese hosti. Seetõttu on selle kasutajaliidese sama kui esimese seansi jaoks.
    -  Valige ripploendist **Target portaali IP** teise andmeid kasutajaliidese seadmes lubatud IP-aadress.
    -  Klõpsake nuppu **OK** , et naasta dialoogiboksi iSCSI algataja atribuudid. Olete lisanud teise seansi sihtkohas.

12. Avage **Arvuti haldamine** navigeerimise **Server Manager > Armatuurlaud > arvuti haldamine**. Klõpsake vasakpoolsel paanil nuppu **salvestusruum > ketta halduse**. StorSimple seadmes on loodud draivid, mis on nähtavad selle hosti kuvatakse jaotises **Ketta haldamine** uue ketta.

13. Lähtesta ketas ja looge uus draiv. Käigus, valige Blokeeri suurus 64 KB.
![Ketas haldus](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. **Ketas haldus**, paremklõpsake **kettale** ja valige **Atribuudid**.
15. StorSimple mudeli ### **Mitme tee ketas seadme atribuutide** dialoogiboksi väljale, klõpsake vahekaarti **MPIO** .
![StorSimple 8100 mitme tee ketta DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. **DSM nimi** jaotises nuppu **üksikasjad** ja veenduge, et väärtused, mis on parameetrid. Väärtused, mis on:

    - Tee kinnitamine perioodi = 30
    - Uute katsete arv = 3
    - KPN eemaldamine perioodi = 20
    - Proovige intervall = 1
    - Tee kinnitamine lubatud = märkimata.


>[AZURE.NOTE] **Muutke vaikimisi parameetrid.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Samm 4: Kõrge-saadavus MPIO konfigureerimine ja tasakaalustamiseks laadimine

Mitme tee põhineva kättesaadavuse ja koormusetasakaalustuseks, mitut seanssi tuleb käsitsi lisada deklareerida saadaval eri teed. Näiteks kui selle hosti on kaks ühendatud SAN ja seade on ühendatud SAN kaks liideste, siis peate neli seanssi konfigureeritud õige tee permutatsioonide (ainult kaks on vaja kui iga andmete kasutajaliidese ja host kasutajaliidese on erinevate IP alamvõrgu ja ei saa marsruutida).

>[AZURE.IMPORTANT] **Soovitame, et mitte segada 1 New York ja 10 New York võrgu liidesed. Kui kasutate kahe võrgu liidesed, nii liideste peaks olema sama tüüpi.**

Järgnevalt kirjeldatakse, kuidas lisada seansid, kui kaks võrgu liidesed StorSimple seade on ühendatud koos kahe võrgu liidesed Host.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Kõrge-saadavus MPIO konfigureerimine ja tasakaalustamiseks laadimine

1. Teha on discovery eesmärgi: klõpsake dialoogiboksis **iSCSI algataja atribuudid** vahekaart **tuvastamine** , **Vaadake portaalis**.
2. Sisestage dialoogiboksis **ühenduse Target** IP-aadress ühe seadme võrgu liidesed.
3. Klõpsake nuppu **OK** , et naasta dialoogiboksi **iSCSI algataja atribuudid** .

4. **ISCSI algataja atribuutide** dialoogiboksis valige vahekaart **sihtkohtade** , avastatud target esile tõsta ja seejärel klõpsake nuppu **Ühenda**. Kuvatakse dialoogiboks **ühenduse sihtkoht** .

5. Dialoogiboksis **ühenduse Target (sihtkoht)** :
    
    - Jätke vaikimisi valitud sihtrühma säte **Lisa see ühendus** lemmik sihtkohtade loendisse. Sel juhul seade automaatselt katsel taaskäivitage ühendus iga kord, kui see arvuti taaskäivitamist.
    - Märkige ruut **Luba mitme tee** .
    - Klõpsake nuppu **Täpsemalt**.

6. Dialoogiboksis **Täpsemad sätted** järgmist.
    - Valige ripploendist **Kohaliku adapterit** **Microsoft iSCSI algataja**.
    - Valige ripploendist **Algataja IP** hosti IP-aadress.
    - Valige ripploendist **Target portaali IP** andmete kasutajaliideses seadmes lubatud IP-aadress.
    - Klõpsake nuppu **OK** , et naasta dialoogiboksi iSCSI algataja atribuudid.

7. Klõpsake nuppu **Atribuudid**ja klõpsake dialoogiboksis **Atribuudid** **Seansi lisada**.

8. Dialoogiboksis **ühenduse Target** märkige ruut **Luba mitme tee** ja seejärel klõpsake nuppu **Täpsemalt**.

9. Dialoogiboksis **Täpsemad sätted** järgmist.
    1. Valige ripploendist **kohaliku adapterit** **Microsoft iSCSI algataja**.
    2. Valige ripploendist **Algataja IP** vastav teise kasutajaliidese hosti IP-aadress.
    3. Valige ripploendist **Target portaali IP** teise andmeid kasutajaliidese seadmes lubatud IP-aadress.
    4. Klõpsake nuppu **OK** , et naasta dialoogiboksi **iSCSI algataja atribuudid** . Nüüd olete lisanud teise seansi sihtkohas.

10. Korrake juhiseid 8-10 lisamiseks sihtkohas täiendavad seansid (teed). Kahe liidesed host ja kaks seadmes, saate lisada kokku neli seanssi.

11. Pärast lisamist **iSCSI algataja atribuutide** dialoogiboksis soovitud seansid (teed), valige siht ja klõpsake käsku **Atribuudid**. Dialoogiboksi **Atribuudid** vahekaardil seansid Märkus nelja seansi identifikaatoritest, mis vastavad tee võimalike permutatsioonide. Seansi lõpetamine tühistamiseks valige seansi identifikaator kõrval olev ruut ja seejärel klõpsake nuppu **Katkesta ühendus**.

12. Esitatud jooksul seansid seadmete vaatamiseks valige vahekaarti **seadmed** . Klõpsake valitud seadme MPIO poliitika konfigureerimiseks **MPIO**. Kuvatakse dialoogiboks **Mobiilsideseadme üksikasjad** . Menüü **MPIO** saate valida sobiv **Laadi saldo rühmapoliitika** sätted. Saate vaadata ka **aktiivne** või **puhkerežiimis olev** tee tüüp.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet [StorSimple halduri teenuse abil saate muuta oma StorSimple seadme konfigureerimine](storsimple-modify-device-config.md).
 
