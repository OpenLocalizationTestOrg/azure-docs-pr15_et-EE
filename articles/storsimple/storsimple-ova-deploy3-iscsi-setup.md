<properties 
   pageTitle="StorSimple virtuaalse massiivi iSCSI serveri install | Microsoft Azure'i"
   description="Kirjeldab, kuidas teha Alghäälestus, registreerida oma StorSimple iSCSI serveri ja seadme häälestamise lõpule viia."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Juurutamine StorSimple Virtual massiiv – kui serveriks iSCSI virtuaalse seadme häälestamine

![iSCSI häälestamise protsessi kulgemist](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Ülevaade

Selle õpetuse juurutamise kehtib Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme või StorSimple virtuaalse seadme) töötava märts 2016 üldiselt kättesaadav (GA) väljaanne. Selles õppetükis kirjeldatakse, kuidas teha Alghäälestus, registreerida oma StorSimple iSCSI serveri, seadme häälestamine ja seejärel luua, mount, lähtestamine ja vormindada mahud StorSimple virtuaalse seadme iSCSI serverisse. Selle artikli teave StorSimple häälestamise kehtib ainult StorSimple virtuaalse massiivi. 

Siin tee kirjeldatud toiminguid 1 tund lõpuleviimiseks 30 minutit. Selles artiklis avaldatud teave kehtib ainult StorSimple virtuaalse massiivi.

## <a name="setup-prerequisites"></a>Eeltingimuste seadistamine

Enne konfigureerimine ja StorSimple virtuaalse seadme häälestamine, veenduge, et:

- Teil on ette valmistatud virtuaalne seadme ja ühendatud, nagu on kirjeldatud [Juurutamine StorSimple virtuaalse massiivi - sätte virtuaalse massiiv Hyper - v](storsimple-ova-deploy2-provision-hyperv.md) või [Juurutada StorSimple virtuaalse massiiv - sätte virtuaalse massiiv VMware](storsimple-ova-deploy2-provision-vmware.md).

- Teil on teenuse registreerimise võti StorSimple halduri teenuse loodud StorSimple virtuaalse seadmete haldamine. Lisateavet leiate teemast **etapp 2: saada teenuse registreerimise võti** [juurutamine StorSimple virtuaalse](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key)massiivi - ettevalmistamine portaali.

- Kui see on teise või järgmise virtuaalse seade, mida teil on registreerumist olemasoleva StorSimple halduri teenuse, peaks teil olema teenuse andmete krüptovõtme. See võti Genereeriti, kui esimene seade on edukalt registreeritud see teenus. Kui teil on selle klahvi kaotanud, lugege teemat **saada teenuse andmete krüptovõtme** [kasutamine Web UI hallata oma StorSimple virtuaalse massiiv](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Juhendatud häälestamise 

Järgmised üksikasjalike juhiste abil saate häälestada ja konfigureerida StorSimple virtuaalse seadme:

-  [Samm 1: Kohaliku web UI häälestamise lõpule viia ja seadme registreerimine](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Samm 2: Nõutav seadme häälestamine](#step-2-complete-the-required-device-setup)
-  [Samm 3: Lisatakse](#step-3-add-a-volume)
-  [Samm 4: Mount, lähtestamine ja vormindada maht](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Samm 1: Kohaliku web UI häälestamise lõpule viia ja seadme registreerimine 

#### <a name="to-complete-the-setup-and-register-the-device"></a>Häälestamine ja seadme registreerimine

1. Avage brauseriaken ja ühenduse kasutajaliides web, tippides.

    `https://<ip-address of network interface>`

    Kasutage eelmises toimingus ühenduse URL-i. Kuvatakse tõrge andmise on probleem veebisaidi turbesert. Klõpsake nuppu **Jätka selle veebilehe**.

    ![Turvalisus serdi tõrge](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Logige sisse veebi UI virtuaalse seadme **StorSimpleAdmin**. Sisestage seadme administraatori parool, mida olete muutnud samm 3: alustada [Juurutamine StorSimple virtuaalse massiiv - sätte Hyper - v virtuaalne seadme](storsimple-ova-deploy2-provision-hyperv.md) või [Juurutada StorSimple virtuaalse massiivi - sätte virtuaalse seadme VMware](storsimple-ova-deploy2-provision-vmware.md)virtuaalse seade.

    ![Sisselogimise leht](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. Saate võetakse lehele **Avaleht** . Sellel lehel kirjeldatakse mitmesuguste sätete konfigureerimine ja virtuaalse seadme registreerimine StorSimple halduri teenuse nõutav. Pange tähele **võrgusätted**, **Web puhverserveri sätete**ja **kellaajasätete** on valikuline. Ainult nõutavad sätted on **sätted** ja **pilveteenuse sätted**.

    ![Avaleht](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. **Võrgu sätete** lehel jaotises **võrku liideste**konfigureeritakse andmete 0 automaatselt teie eest. Iga võrgu liidese on vaikimisi automaatselt hankida IP-aadress (DHCP). Seetõttu IP-aadress, alamvõrgu ja lüüsi automaatselt määratakse (IPv4 ja IPv6) jaoks.

    Kui kavatsete juurutada seadme nimega ettevõtte iSCSI serveris (kui soovite ettevalmistamise plokk salvestusruumi), soovitame teil **saada IP-aadress automaatselt** võimaluse keelamine ja konfigureerimine staatiline IP-aadressid.

    ![Lehel](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Kui lisasite rohkem kui üks võrk kasutajaliidese ajal ettevalmistamise seade, saate need siin konfigureerida. Pange tähele, et te saate konfigureerida oma võrgu liidese IPv4 ainult või kui nii IPv4 ja IPv6. Ainult konfiguratsioone ei toeta protokolli IPv6.

5. DNS-serverid on nõutavad, kuna neid kasutavad, kui teie seade proovib suhelda pilveteenuse salvestusruumi teenusepakkujatele või lahendamiseks seadme nime järgi, kui see on konfigureeritud faili server. Jaotises **DNS-serverid**lehel **sätted** järgmist.

    1. Põhi- ja DNS-i serveri konfigureeritakse automaatselt. Kui valite konfigureerida staatiline IP-aadressid, saate määrata DNS-serverid. Kõrge-saadavus, soovitame konfigureerida esmane ja teisene DNS server.

    2. Klõpsake nuppu **Rakenda**. See rakendada ja kinnitage võrgu sätteid.

6. Tehke lehel **sätted** järgmist.

    1. Saate määrata oma seadme kordumatu **nimi** . See nimi võib olla märgi 1-15 ja võib sisaldada täht, numbreid ja sidekriipse.

    2. Klõpsake ikooni **iSCSI serveri** ![iSCSI serveri ikooni](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) loote seadme **Tüüp** . Ettevõtte iSCSI serveris võimaldab teil sätte Blokeeri salvestusruumi.

    3. Määrake, kas soovite selle seadme olema domeeni ühendatud. Kui teie seade on iSCSI server, siis liitumise domeeni pole kohustuslik. Kui otsustate domeeni Liitu oma iSCSI server, klõpsake nuppu **Rakenda**, oodake sätete rakendada ja siis jätkake järgmise juhisega.

        Kui soovite liituda seade, domeeni. Sisestage **domeeni nimi**ja seejärel klõpsake nuppu **Rakenda**.

        > [AZURE.NOTE] Kui liitumine oma iSCSI serveri, domeeni, veenduge, et teie virtuaalse massiiv on eraldi organisatsiooniüksusega (Browse) Microsoft Azure Active Directory jaoks ja pole rühmapoliitika objektid (GPO) on rakendatud.

    5. Kuvatakse dialoogiboks. Sisestage oma domeeni mandaat määratud vormingus. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Domeeni identimisteabe kontrollida. Kui mandaat on valed, kuvatakse tõrketeade.

        ![identimisteave](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Klõpsake nuppu **Rakenda**. See rakendada ja kinnitage seadme sätted.
 
7. (Valikuline) konfigureerida web puhverserverisse. Kuigi web puhverserveri konfigureerimine on valikuline, arvestage, et kui te kasutate veebiteenuse puhverserveri, saate ainult selle konfigureerida siin.

    ![veebiteenuse puhverserveri konfigureerimine](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    **Veebiteenuse puhverserveri** lehel tehke järgmist.

    1. Esitama **Web puhverserveri URL-i** järgmises vormingus: *http://host-IP aadress* või *FDQN:Port arv*. Pange tähele, et HTTPS URL ei toetata.

    2. Saate määrata **tavaline** või **mitte ühtegi** **autentimist** .

    3. Kui kasutate autentimine, peate ka **kasutajanime** ja **parooli**.

    4. Klõpsake nuppu **Rakenda**. See kinnitage ja konfigureeritud web puhverserveri sätete rakendada.
 
8. (Valikuline) oma seadmesse, nt ajavöönd ja esmaseid ja teiseseid NTP serverid aja sätteid konfigureerida. NTP serverid on nõutavad, kuna teie seade peab sünkroonimiseks nii, et see oma pilveteenuses teenusepakkujatele autentimist.

    ![Kellaaja sätted](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    **Kellaaja sätete** lehel järgmist.

    1. Valige ripploendist, **ajavööndi** seade on kasutusele geograafilise asukoha alusel. Teie seadme jaoks vaikimisi ajavöönd on PST. Seadme kasutavad ajavööndi kõik ajastatud toimingud.

    2. Määrake seadme **esmane NTP serveri** või time.windows.com vaikeväärtuse. Veenduge, et teie võrgus võimaldab NTP edastada oma andmekeskuse Interneti-liikluse.

    3. Soovi korral määrake **teisene NTP serveri** seadme.

    4. Klõpsake nuppu **Rakenda**. See kinnitage ja konfigureeritud aja sätted rakendada.

9. Seadme cloud sätteid konfigureerida. Selles etapis tuleb teil kohaliku seadmega konfigureerimise lõpuleviimiseks ja seejärel registreerida seade teie haldur StorSimple teenusega.

    1. Sisestage **teenuse registreerimise võti** , mida teil on **Samm 2: saada teenuse registreerimise võti** [juurutamine StorSimple virtuaalse](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key)massiivi - ettevalmistamine portaali.

    2. Kui see pole esimene seade, et teil on selle teenuse registreerumist, peate **teenuse andmete krüptovõtme**. See võti on vaja teenuse registreerimise võti StorSimple halduri teenuse täiendavad seadmed registreerida. Lisateabe saamiseks vaadake saada [teenuse andmete krüptovõtme](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) oma kohaliku veebis UI.

    3. Klõpsake nuppu **registreeru**. See seade uuesti. Peate võib-olla ootama, enne kui seade on edukalt registreeritud 2 – 3 minutit. Pärast seadme taaskäivitamist suunatakse lehel sisselogimine.

       ![Seadme registreerimine](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Azure'i klassikaline portaali naasmiseks. Kontrollige lehel **seadmed** , et seade on ühendatud teenusega otsides olek. Seadme olek peaks olema **aktiivne**.

    ![Lehe seadmed](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Samm 2: Nõutav seadme häälestamine

Seadme konfiguratsiooni StorSimple seadme tegemiseks peate:

- Valige salvestusruumi konto seadme seostada.

- Valige andmed, mis on saadetud Cloud krüptimise sätteid.

Azure'i klassikaline portaalis vajalik seadme häälestamise lõpuleviimiseks tehke järgmist.

#### <a name="to-complete-the-minimum-device-setup"></a>Minimaalne seadme häälestamise lõpuleviimiseks

1. Valige lehel **seadmed** seadme, mille äsja loodud. Kasutatav seade näitaks üles **aktiivsed**. Klõpsake noolt seadme nimi ja klõpsake **Kiirkäivituse**.

    ![Lehe seadmed](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Klõpsake nuppu **valmis seadme häälestamise** konfigureerimine seadme viisardi käivitamiseks.

    ![Seadme viisardi konfigureerimine](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. Konfigureerimine seadme viisardi lehel **Põhisätted** tehke järgmist.

   1. Määrake salvestusruumi konto kasutatava seadmega. See tellimus, saate valida olemasoleva salvestusruumi konto rippmenüü loendist või saate määrata konto valida mõni muu tellimus **Lisa veel** .

   2. Kõigi ülejäänud, mis saadetakse pilveteenusesse andmete krüptimise sätete määratlemine. (StorSimple kasutab AES 256 krüptimist.) Andmete krüptimiseks ruut **Luba pilveteenuse salvestusruumi krüptimine** . Sisestage pilveteenuse salvestusruumi krüptimist, mis sisaldab 32 märki. Sisestage võti selle kinnitamiseks uuesti.

   3. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Põhisätted](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    Nüüd värskendatakse sätted. Täieliku seadme häälestamise nupp saadaval juhul, kui sätted on värskendatud. Naastakse seadme **Kiirkäivituse** lehele.                                                        

>[AZURE.NOTE]Kõik muud seadme sätted igal ajal muuta juurdepääs lehe **konfigureerimine** .

## <a name="step-3-add-a-volume"></a>Samm 3: Lisatakse

Azure'i klassikaline portaalis loomiseks tehke järgmist.

#### <a name="to-create-a-volume"></a>Luua maht

1. Seadme **Kiirkäivituse** lehel klõpsake nuppu **Lisa maht**. Käivitub helitugevuse viisardit lisamine.

2. Helitugevuse viisardit, klõpsake jaotises **Põhisätted**lisamine tehke järgmist.

    1. Lisage oma helitugevuse kordumatu nimi. Nimi peab olema string, mis sisaldab märke 3 – 127.

    2. Helitugevuse kirjelduse. Kirjeldus aitab tuvastada helitugevuse omanikud.

    3. Valige mahu kasutus tüüp. Kasutamine tüüp võib olla **testimiste helitugevuse** või **kohalikult kinnitatud helitugevuse.** (**Testimiste helitugevuse** on vaikimisi). Kohaliku garantiid, madal latentsused ja suurema jõudluse nõudvate töökoormus, valige **kohalik kinnitatud** **helitugevust**. Valige kõik andmed, **testimiste** **helitugevust**.

        Kohalik kinnitatud helitugevuse paksult ette valmistatud ja tagab, et peamine andmete maht jääb seadmes ja kandu pilveteenusesse. Kui loote kohalikult kinnitatud helitugevust, kontrollib seade saadaval ruumi kohaliku astme ettevalmistamise maht on soovitud suurusega. Kohalik kinnitatud helitugevuse loomine võib olla vaja kallab olemasolevate andmete seadmest pilveteenusesse ja helitugevuse loomiseks kuluv aeg võib olla pikk. Kogu aeg sõltub ettevalmistatud maht, võrgu läbilaskevõime ja andmed teie seadmes suurust.

        Astmeline helitugevuse teiselt õhukesed ette valmistatud ja saab kiiresti luua. Kui loote astmeline helitugevust, 10% ruumi on ette valmistatud kohaliku taseme kohta ja 90% ruumi on ette valmistatud pilveteenuses. Näiteks, kui teil on ette valmistatud 1 TB maht, 100 GB oleks asu kohaliku ruumi ja 900 GB kasutataks pilves kui andmete astme. See omakorda tähendab, on see, et kui otsa kohaliku ruumi seadmes, ei saa ette astmeline ühiskasutus (kuna 10% on saadaval).

    4. Määrake oma maht ettevalmistatud võimsus. Pange tähele, et määratud võimsus peab olema väiksem kui saadaval võimsus. Kui loote astmeline helitugevust, tuleks suurusega 500 GB ja 5 TB. Kohalik kinnitatud maht, määrake helitugevus suurus 50 GB ja 500 GB. Kasutage olemasoleva võimsuse juhend ettevalmistamise maht. Kui saadaval kohaliku maht on 0 GB, siis te pole lubatud ettevalmistamise kohalikult kinnitatud või astmeline helitugevust.

        ![Põhisätted](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Klõpsake noolt ikooni ![nooleikooni](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) järgmisele lehele liikumiseks.

3. **Täiendavate sätete** lehel uue Accessi juhtelement kirje (ACR) lisamiseks tehke järgmist.

    1. Lisage oma ACR **nimi** .

    2. Klõpsake jaotises **iSCSI algataja nimi**, sisestage iSCSI kvalifitseeritud oma Windows host Name (IQN). Kui teil on IQN, valige [Lisa v: Hangi Windows Server hosti IQN](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. Soovitame teil lubada vaikimisi varukoopia märkige ruut **Luba selle draivi jaoks vaikimisi varukoopia** . Varunduse loob poliitika, mis käivitab 22:30 iga päev (seadme kellaaeg) ja loob selle helitugevuse hetktõmmise pilve.

        ![Täpsemad sätted](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). See käivitab helitugevuse loomine töö. Kuvatakse järgmise sisuga poolelioleva sõnumi.

        ![poolelioleva sõnumi](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        Maht ei looda määratud sätetega. Vaikimisi on lubatud maht varundus- ja.

    5. Veenduge, et helitugevus on loodud, minge lehele **maht** . Näha peaks olema loetletud maht.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Samm 4: Mount, lähtestamine ja vormindada maht

Järgmiste toimingute mount, lähtestamine ja teie StorSimple draivid Windows Server hosti vormindamine.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Ühendada, lähtestamine ja vormindada maht

1. Käivitage Microsoft iSCSI algataja.

2. Klõpsake aknas **iSCSI algataja atribuudid** vahekaardil **Discovery** **Vaadake portaalis**.

    ![Vaadake portaalis](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. Dialoogiboksis **Avastamine Target portaali** sisestama oma võrgu iSCSI lubatud liidese IP-aadress ja seejärel klõpsake nuppu **OK**.

    ![IP-aadress](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. Leidke aknas **iSCSI algataja atribuudid** vahekaardil **sihtkohtade** **avastatud sihtkohtade**. (Iga maht on avastatud target.) Seadme olek peaks olema **passiivne**.

    ![avastatud sihtkohad](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Valige soovitud seade ja seejärel klõpsake nuppu **Ühenda**. Kui seade on ühendatud, olek peaks vahetada **ühendatud**. (Microsoft iSCSI algataja kasutamise kohta leiate lisateavet teemast [installimine ja konfigureerimine Microsoft iSCSI algataja] [1].

    ![Valige soovitud seade](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. Windows Server, vajutage Windowsi logoga klahv + X ja seejärel klõpsake nuppu **Käivita**.

7. Tippige dialoogiboksi **käivitamine** **Diskmgmt.msc**. Klõpsake nuppu **OK**ja **Kettaruumi halduse** dialoogiboks kuvatakse. Paremal paanil kuvatakse teie hosti mahud.

8. **Ketas halduse** aknas kuvatakse ühendatud mahud nagu on näidatud järgmisel joonisel. Paremklõpsake avastatud helitugevuse (klõpsake ketta nimi) ja seejärel klõpsake nuppu **Veebipildid**.

    ![ketas haldus](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Paremklõpsake ja valige käsk **Vorminda ketas**.

    ![1 ketta lähtestamine](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. Dialoogiboksis valige ketta lähtestamine ja seejärel klõpsake nuppu **OK**.

    ![2 ketta lähtestamine](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. Lihtne uus draiv viisard käivitub. Valige ketas suurus ja seejärel klõpsake nuppu **edasi**.

    ![uue draivi viisardi 1](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Helitugevuse määramiseks draivi nime ja seejärel klõpsake nuppu **edasi**.

    ![uue draivi viisardi 2](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Sisestage soovitud parameetrid draivi vormindamiseks. **Windows Server, toetatakse ainult NTFS.** Seadke soovitud AUS 64K. Sildi ette oma helitugevust. See on soovitatav põhitõde identne andsite StorSimple virtuaalse seadme helitugevuse nimi see nimi. Klõpsake nuppu **edasi**.

    ![uue draivi viisardi 3](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Kontrollige oma helitugevuse väärtused ja klõpsake siis nuppu **valmis**.

    ![uue draivi viisardi 4](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    **Ketas halduse** lehel kuvatakse mahud **Online** .

    ![mahud võrgus](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas kasutada kohaliku web UI hallata [Oma StorSimple virtuaalse massiiv](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Lisa A: hankida Windows Server hosti IQN

Järgmiste toimingute saada iSCSI kvalifitseeritud Windows host, kus töötab Windows Server 2012 nimi (IQN).

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Saada Windows hosti IQN

1. Käivitage Microsoft iSCSI algataja Windows Server.

2. Aknas **iSCSI algataja atribuudid** vahekaardil **konfigureerimine** valige ja kopeerige väli **Algataja nimi** string.

    ![iSCSI algataja atribuudid](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Salvestage see string.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



