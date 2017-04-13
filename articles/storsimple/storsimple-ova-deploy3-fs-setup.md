<properties
   pageTitle="Juurutamine StorSimple Virtual massiiv 3 - faili serveriga ühiseid virtuaalse seadme häälestamine"
   description="Kolmanda õppeteema StorSimple virtuaalse massiiv juurutuse juhendab teid faili serveriga ühiseid virtuaalse seadme häälestamine."
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
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Juurutamine StorSimple Virtual massiivi - Sea üles fail Server

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Sissejuhatus 

See artikkel kehtib Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme või StorSimple virtuaalse seadme) käivitatud märts 2016 üldiselt kättesaadav (GA) versioonis. Selles artiklis kirjeldatakse, kuidas teha Alghäälestus registreerida oma StorSimple failiserver, seadme häälestamine ja luua ja ühendust SMB aktsiate. See on viimase artiklist sarja vajalik täielikult juurutada oma virtuaalse massiiv failiserverisse või mõnda iSCSI serveri juurutuse õpetused.

Häälestamise ja konfigureerimise protsess võib võtta umbes 10 minuti lõpuleviimiseks.


## <a name="setup-prerequisites"></a>Eeltingimuste seadistamine

Enne konfigureerimine ja StorSimple virtuaalse seadme häälestamine, veenduge, et:

-   Teil on ette valmistatud virtuaalne seadme ja nagu [pakkumine StorSimple virtuaalse massiiv Hyper - v](storsimple-ova-deploy2-provision-hyperv.md) või [sätte StorSimple virtuaalse massiiv VMware](storsimple-ova-deploy2-provision-vmware.md)on ühendatud.

-   Teil on teenuse registreerimise võti StorSimple halduri teenuse loodud StorSimple virtuaalse seadmete haldamine. Lisateavet leiate teemast [etapp 2: saada teenuse registreerimise võti](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple virtuaalse array.

-   Kui see on teise või järgmise virtuaalse seade, mida teil on registreerumist olemasoleva StorSimple halduri teenuse, peaks teil olema teenuse andmete krüptovõtme. See võti Genereeriti, kui esimene seade on edukalt registreeritud see teenus. Kui teil on selle klahvi kaotanud, lugege teemat [teenuse andmete krüptovõtme](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) oma StorSimple virtuaalse array.

## <a name="step-by-step-setup"></a>Juhendatud häälestamise

Järgmised üksikasjalike juhiste abil saate häälestada ja konfigureerida StorSimple virtuaalse seadme.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Samm 1: Kohaliku web UI häälestamise lõpule viia ja seadme registreerimine 


#### <a name="to-complete-the-setup-and-register-the-device"></a>Häälestamine ja seadme registreerimine

1.  Avage brauseriaken ja ühenduse loomine kohaliku web UI. Tüüp: 

    `https://<ip-address of network interface>`

    Kasutage eelmises toimingus ühenduse URL-i. Kuvatakse tõrge, mis näitab, et on veebisaidi turbeserdiga probleem. Klõpsake nuppu **Jätka veebilehelt**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Logige sisse veebi UI virtuaalse seadme **StorSimpleAdmin**. Sisestage seadme administraatori parool, mida olete muutnud samm 3: alustada [sätte StorSimple virtuaalse massiiv Hyper - v](storsimple-ova-deploy2-provision-hyperv.md) või [sätte StorSimple virtuaalse massiiv VMware](storsimple-ova-deploy2-provision-vmware.md)virtuaalse seade.

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  Saate võetakse lehele **Avaleht** . Sellel lehel kirjeldatakse mitmesuguste sätete konfigureerimine ja virtuaalse seadme registreerimine StorSimple halduri teenuse nõutav. Pange tähele **võrgusätted**, **Web puhverserveri sätete**ja **kellaajasätete** on valikuline. Ainult nõutavad sätted on **sätted** ja **pilveteenuse sätted**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  **Võrgu sätete** lehel jaotises **võrku liideste**konfigureeritakse andmete 0 automaatselt teie eest. Iga võrgu liidese on vaikimisi automaatselt hankida IP-aadress (DHCP). Seega IP-aadress, alamvõrgu ja lüüsi automaatselt määratakse (IPv4 ja IPv6) jaoks.

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Kui lisasite rohkem kui üks võrk kasutajaliidese ajal ettevalmistamise seade, saate need siin konfigureerida. Pange tähele, et te saate konfigureerida oma võrgu liidese IPv4 ainult või kui nii IPv4 ja IPv6. Ainult konfiguratsioone ei toeta protokolli IPv6.

1.  DNS-serverid on nõutavad, kuna neid kasutavad kui seadme proovib suhelda pilveteenuse salvestusruumi teenusepakkujatele või seadme lahendamiseks failiserverisse konfigureerimisel nime järgi. **Võrgu sätete** lehel jaotises **DNS-serverid**:

    1.  Põhi- ja DNS-i serveri konfigureeritakse automaatselt. Kui valite konfigureerida staatiline IP-aadressid, saate määrata DNS-serverid. Kõrge-saadavus, soovitame konfigureerida esmane ja teisene DNS server.

    2.  Klõpsake nuppu **Rakenda**. See rakendada ja kinnitage võrgu sätteid.

2.  Lehel **sätted** :

    1.  Saate määrata oma seadme kordumatu **nimi** . See nimi võib olla märgi 1-15 ja võib sisaldada täht, numbreid ja sidekriipse.

    2.  Klõpsake ikooni **failiserverisse** ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) loote seadme **Tüüp** . Failiserverisse võimaldab teil luua Ühiskaustad.

    3.  Kui teie seade on failiserverisse, peate seadme domeeniga liita. Sisestage **domeeni nimi**.

    1.  Klõpsake nuppu **Rakenda**.

2.  Kuvatakse dialoogiboks. Sisestage oma domeeni mandaat määratud vormingus. Klõpsake ikooni Otsi. Domeeni identimisteabe kontrollida. Kui mandaat on valed, kuvatakse tõrketeade.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Klõpsake nuppu **Rakenda**. See rakendada ja kinnitage seadme sätted.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Veenduge, et teie virtuaalse massiiv on eraldi organisatsiooniüksusega (OU) Active Directory jaoks ja pole rühmapoliitika objektid (GPO) on rakendatud või päritud. Rühmapoliitika võib installida rakendused, nt viirusetõrje tarkvara StorSimple virtuaalse massiiv. Kui installite täiendavat tarkvara ei toetata ja võib põhjustada andmete rikkumist. 

1.  (Valikuline) konfigureerida web puhverserverisse. Kuigi web puhverserveri konfigureerimine on valikuline, arvestage, et kui te kasutate veebiteenuse puhverserveri, saate ainult selle konfigureerida siin.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    **Veebiteenuse puhverserveri** lehte tehke järgmist.

    1.  Esitama **Web puhverserveri URL-i** järgmises vormingus: *http://&lt;host-IP-aadress või FDQN&gt;: pordi number*. Pange tähele, et HTTPS URL ei toetata.

    2.  Saate määrata **tavaline** või **mitte ühtegi** **autentimist** .

    3.  Kui autentimist kasutades peate ka **kasutajanime** ja **parooli**.

    4.  Klõpsake nuppu **Rakenda**. See kinnitage ja konfigureeritud web puhverserveri sätete rakendada.

1.  (Valikuline) oma seadmesse, nt ajavöönd ja esmaseid ja teiseseid NTP serverid aja sätteid konfigureerida. NTP serverid on nõutavad, kuna teie seade peab sünkroonimiseks nii, et see oma pilveteenuses teenusepakkujatele autentimist.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    **Kellaaja sätete** lehel:

    1.  Valige ripploendist, **ajavööndi** seade on kasutusele geograafilise asukoha alusel. Teie seadme jaoks vaikimisi ajavöönd on PST. Seadme kasutavad ajavööndi kõik ajastatud toimingud.

    2.  Määrake seadme **esmane NTP serveri** või time.windows.com vaikeväärtuse. Veenduge, et teie võrgus võimaldab NTP edastada oma andmekeskuse Interneti-liikluse.

    3.  Soovi korral määrake **teisene NTP serveri** seadme.

    4.  Klõpsake nuppu **Rakenda**. See kinnitage ja konfigureeritud aja sätted rakendada.

1.  Seadme cloud sätteid konfigureerida. Selles etapis tuleb teil kohaliku seadmega konfigureerimise lõpuleviimiseks ja seejärel registreerida seade teie haldur StorSimple teenusega.

    1.  Sisestage **teenuse registreerimise võti** , mida teil on [Samm 2: saada teenuse registreerimise võti](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple virtuaalse array.

    2.  Selle sammu vahele jätta, kui see on esimene seadme registreerimiseks, mis see teenus ja minge järgmise juhise juurde. Kui see pole esimene seade, et teil on selle teenuse registreerumist, peate **teenuse andmete krüptovõtme**. See võti on vaja teenuse registreerimise võti StorSimple halduri teenuse täiendavad seadmed registreerida. Lisateabe saamiseks vaadake saada [teenuse andmete krüptovõtme](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) oma kohaliku veebis UI.

    3.  Klõpsake nuppu **registreeru**. See seade uuesti. Peate võib-olla ootama, enne kui seade on edukalt registreeritud 2 – 3 minutit. Pärast seadme taaskäivitamist suunatakse lehel sisselogimine.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Azure'i klassikaline portaali naasmiseks. Kontrollige lehel **seadmed** , et seade on ühendatud teenusega otsides olek. Seadme olek peaks olema **aktiivne**.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Samm 2: Nõutav seadme häälestamine

Seadme konfiguratsiooni StorSimple seadme tegemiseks peate:

-   Valige salvestusruumi konto seadme seostada.

-   Valige andmed, mis on saadetud Cloud krüptimise sätteid.

[Azure'i klassikaline portaali](https://manage.windowsazure.com/) vajalik seadme häälestamise lõpuleviimiseks tehke järgmist.

#### <a name="to-complete-the-minimum-device-setup"></a>Minimaalne seadme häälestamise lõpuleviimiseks

1.  **Seadmete** lehel Valige äsja loodud seade. Kasutatav seade näitaks üles **aktiivsed**. Klõpsake nuppu seadme nime juures olevat noolt ja seejärel nuppu **Kiirkäivituse**.

2.  Klõpsake nuppu **valmis seadme häälestamise** konfigureerimine seadme viisardi käivitamiseks.

3.  Konfigureerimine seadme viisardi lehel **Põhisätted** , tehke järgmist.

    1.  Määrake salvestusruumi konto kasutatava seadmega. Saate olemasoleva salvestusruumi konto valige ripploendist selle tellimuse või määrata **Lisa veel** konto valida mõni muu tellimus.

    2.  Määratleda kõik andmed – veebisaidil-ülejäänud (AES krüptimine) pilveteenusesse saadetud krüptimise sätteid. Andmete krüptimiseks ruut liitboksi **pilveteenuse salvestusruumi krüptovõtme**lubamiseks. Sisestage pilveteenuse salvestusruumi krüptimist, mis sisaldab 32 märki. Sisestage võti selle kinnitamiseks uuesti. Kasutaja määratletud krüptimise abil kasutatakse 256-bitine AES klahvi.

    3.  Klõpsake ikooni Otsi ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

Nüüd värskendatakse sätted. Pärast sätted on värskendatud, nupp täieliku seadme häälestamise tuhm. Naasete seadme **Kiirkäivituse** lehele.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> Kõik muud seadme sätted igal ajal muuta juurdepääs lehe **konfigureerimine** .

## <a name="step-3-add-a-share"></a>Samm 3: Lisada

[Azure'i klassikaline portaali](https://manage.windowsazure.com/) osa loomiseks tehke järgmist.

#### <a name="to-create-a-share"></a>Ühiskasutusega loomiseks

1.  Seadme **Kiirkäivituse** lehel klõpsake nuppu **lisa osa**. Käivitub ühiskasutus viisardit lisamine.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Klõpsake lehel **Põhisätted** tehke järgmist.

    1.  Määrake oma ühiskasutus kordumatu nimi. Nimi peab olema string, mis sisaldab märke 3 – 127.

    2.  (Valikuline) Kirjelduse ühiskasutus. Kirjeldus aitab tuvastada ühiskasutus omanikud.

    3.  Valige osa kasutus tüüp. Kasutamine tüüp saab **testimiste** või **kohalikult kinnitatud**, astmeline on vaikimisi. Töökoormus, mis nõuavad kohaliku garantiid, madal latentsused ja suurema jõudluse, valige **kohalik kinnitatud** ühiskasutus. Valige kõik andmed, **testimiste** ühiskasutus.

    Kohalik kinnitatud ühiskasutus paksult ette valmistatud ja tagab, et lähteandmeid võrgukettal jääb kohaliku seadmega ja kandu pilveteenusesse. Astmeline ühiskasutus teiselt on õhukesed ette valmistatud. Kui loote astmeline ühiskasutus, 10% ruumi on ette valmistatud kohaliku taseme kohta ja 90% ruumi on ette valmistatud pilveteenuses. Näiteks, kui teil on ette valmistatud 1 TB maht, 100 GB oleks asu kohaliku ruumi ja 900 GB kasutataks pilves kui andmete astme. See omakorda tähendab, et kui otsa kohaliku ruumi seadmes, ei saa ette astmeline ühiskasutus.

1.  Määrake oma ühiskasutus ettevalmistatud läbilaskevõime. Pange tähele, et määratud võimsus peab olema väiksem kui saadaval võimsus. Astmeline ühiskasutus kasutamisel tuleks ühiskasutus suurusega 500 GB ja 20 TB. Kohalik kinnitatud osa, määrake ühiskasutus suurus 50 GB ja 2 TB. Kasutage olemasoleva võimsuse juhendina ettevalmistamise osa. Kui saadaval kohaliku võimsus on 0 GB, siis pole lubatakse teil ettevalmistamise aktsiad kohalike või astmeline.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Nooleikooni ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) järgmisele lehele liikumiseks.

1.  **Täiendavate sätete** lehel soovitud õigusi määrata kasutaja või rühm, mida saab juurdepääsu selle ühiskasutusse andmine. Määrake kasutaja või kasutajate rühma nimi *<john@contoso.com>* vorming. Soovitame kasutada kasutaja rühm (ühe kasutaja) asemel administraatoriõigused neid osi juurdepääsu lubamine. Pärast seda, kui olete määranud õigused siin, saate neid Windows Exploreri abil muuta järgmisi õigusi.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Klõpsake ikooni Otsi ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Ühiskasutusega luuakse määratud sätetega. Vaikimisi on lubatud ühiskasutus varundus- ja.

## <a name="step-4-connect-to-the-share"></a>Samm 4: Ühenduse ühiskasutus

Nüüd peate ühenduse eelmises etapis loodud alustades. Nende juhiste täitmiseks oma Windows Server hosti.

#### <a name="to-connect-to-the-share"></a>Ühenduse ühiskasutus

1.  Vajutage ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. Käivita aknas määrata selle * \\ * tee, asendades *faili serveri nimi* oma faili serveris määratud seadme nime. Klõpsake nuppu **OK**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  See avab Explorer. Nüüd peaks olema kaustadena loodud ühtlasi vaadata. Valige ja topeltklõpsake sisu ühiskasutus (kausta).

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  Nüüd saate faile lisada need aktsiad ja võtta varukoopia.

![video ikoon](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **Video saadaval**

Vaata videot, et näha, kuidas konfigureerida ja registreeruda StorSimple virtuaalse massiiv failiserverisse.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas kasutada kohaliku web UI hallata [Oma StorSimple virtuaalse massiiv](storsimple-ova-web-ui-admin.md).
