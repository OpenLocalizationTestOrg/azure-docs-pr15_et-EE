<properties 
   pageTitle="Automatiseerida DR failikettad StorSimple abil Azure saidi taastamise jaoks | Microsoft Azure'i"
   description="Kirjeldatakse juhiseid ja katastroofi taastamise lahendus majutatud StorSimple salvestusruumi failikettad loomise head tavad."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automaatse avariitaastet lahenduse majutatud StorSimple failikettad Azure saidi taastamise abil

## <a name="overview"></a>Ülevaade

Microsoft Azure'i StorSimple on hübriid pilve salvestusruumi lahendus, mis käsitleb keerulisi tavaliselt seotud failikettad struktureerimata andmed. StorSimple kasutab pilvepõhist salvestusruumi pikendamine kohapealse lahenduse ja automaatselt astme andmete kohapealse ja pilveteenuse üle. Andmekaitse, integreeritud kohalik ja cloud pilte, välistab laialivalguv salvestusruumi taristu.

[Azure'i saidi taastamine](../site-recovery/site-recovery-overview.md) on Azure'i teenus, mis sisaldab katastroofi (DR) funktsioonid, orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates taastamine. Azure'i saidi taastamine toetab arvu dispersioonanalüüs tehnoloogiad pidevalt korrata, kaitse ja sujuvalt ei õnnestu üle virtuaalmasinates ja privaatne/avalik või majutatud pilved rakendusi.

Azure saidi taastamise, virtuaalse masina kopeerimise ja StorSimple cloud hetktõmmise võimaluste abil saate kaitsta täieliku faili serveri keskkonnas. Korral häireid, saate ühe klõpsuga tuua oma failikettad võrgus Azure paar minutit.

Selles dokumendis selgitatakse üksikasjalikult, kuidas saate luua oma failikettad hostitud StorSimple salvestusruumi, katastroofi taastamise lahendus ja teha plaanitud, ootamatute ja testida failovers abil ühe klõpsuga taastamis. Sisuliselt see näitab, kuidas saate muuta taastamise kava Azure'i saidi taastamise hoidla lubamiseks StorSimple failovers ajal katastroof stsenaariumi. Peale selle kirjeldatakse toetatud konfiguratsioone ja eeltingimused. Selle dokumendi eeldab, et olete tuttav Azure saidi taastamine ja StorSimple arhitektuurides põhitõdesid.

## <a name="supported-azure-site-recovery-deployment-options"></a>Toetatud Azure saidi taastamise Juurutussuvandid

Klientide saate juurutada serverites füüsilise serveri või Hyper-V või VMware virtuaalmasinates (VMs) ja seejärel looge failikettad uuristatud StorSimple talletusruumi maht kaudu. Azure'i saidi taastamine saate kaitsta füüsilise ja virtuaalse juurutuste teisene saidile või Azure. Selle dokumendi hõlmab üksikasjade DR lahenduse nimega taastamine saidi VM majutatud Hyper-V faili server Azure'i ja failiketastel StorSimple säilitamise kohta. Samuti saab rakendada teiste stsenaariumide failiserverisse VM on Vmware'i VM või füüsilise kohapeal.

## <a name="prerequisites"></a>Eeltingimused

Rakendamine ühe klõpsuga katastroofi taastamine lahenduse, mis kasutab Azure saidi taastamise hostitud StorSimple salvestusruumi failikettad on järgmine kohustuslik tarkvara.

-   Asutusesisene VM majutatud Hyper-V või VMware või füüsilise seadme Windows Server 2012 R2 faili server

-   Azure'i StorSimple halduri registreeritud StorSimple salvestusruumi seadme kohapealse

-   StorSimple Cloud seadme loodud Azure StorSimple Manager (selle saab hoida sulgeda olekus)

-   Failikettad majutatud konfigureeritud StorSimple mäluseade mahud

-   [Azure'i saidi taastamise teenuste hoidla](../site-recovery/site-recovery-vmm-to-vmm.md) loodud Microsoft Azure'i tellimus

Lisaks, kui Azure on taastamine saidi, käivitage [Azure virtuaalse masina valmisoleku hindamise tööriist](http://azure.microsoft.com/downloads/vm-readiness-assessment/) VMs tagamaks, et need on kooskõlas Azure VMs ja Azure saidi taastamise kohta.

Latentsus on probleeme (mis võib kaasa tuua suuremad kulud), veenduge, et teie StorSimple Cloud seadme, automatiseerimise konto ja salvestusruumi loomine vältimiseks kontod piirkonna.

## <a name="enable-dr-for-storsimple-file-shares"></a>StorSimple failikettad DR lubamine  

Iga komponendi kohapealse keskkonna tuleb lubada täieliku kopeerimise ja taastamise kaitsta. Selles jaotises kirjeldatakse, kuidas:

-   Active Directory ja DNS-i kopeerimine häälestamine (valikuline)

-   Kaitse failiserverisse VM Azure saidi taastamise abil

-   Luba kaitse StorSimple mahud

-   Võrgu konfigureerimine

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Active Directory ja DNS-i kopeerimine häälestamine (valikuline)

Kui soovite kaitse töötab Active Directory ja DNS-i nii, et need on saadaval saidil DR masinad, peate selgesõnaliselt nende kaitsmiseks (nii, et pärast fail üle autentimise juurdepääsetavat faili serverid). On kaks soovitatav võimalusi kliendi kohapealse keskkonna keerukusest.

#### <a name="option-1"></a>Suvand 1

Kui kliendil on väheste rakendused, ühe domeenikontrolleri kogu kohapealse saidi ja kuvatakse jättes üle terve saidi, siis soovitame korrata domeeni domeenikontrolleri kohapeal (see on rakendatav nii-saitide ja saidi Azure) saidi Azure saidi taastamise kopeerimise abil.

#### <a name="option-2"></a>Variant 2

Kui kliendi on suur hulk rakenduste töötab ka Active Directory kogumis ja ei suuda üle mõned rakendused, korraga, siis soovitame luua ka täiendavad domeenikontrolleri saidil DR (kas saidi või Azure).

Vaadake [automatiseeritud DR lahendus Active Directory ja DNS-i abil Azure saidi taastamise](../site-recovery/site-recovery-active-directory.md) juhised kui domeenikontrolleri DR saidil kättesaadavaks tegemine. Selle dokumendi ülejäänud, saame endale domeenikontrolleri on saadaval DR saidil.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Kaitse failiserverisse VM Azure saidi taastamise abil

Selle toimingu jaoks on vaja valmistada kohapealse faili serveri keskkonnas, luua ja ettevalmistamiseks on Azure saidi taastamise hoidla, ning lubada failikaitse VM.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Kohapealse faili serveri keskkonna ettevalmistamiseks

1.  Määrake **teavituste** **kasutajakonto kontroll** . See on nõutav, et Azure automatiseerimine skriptide abil saate ühendada iSCSI eesmärgid üle Azure saidi taastamise fail pärast.

    1.  Vajutage klahvikombinatsiooni Windowsi klahv + K ja **UAC**otsimine.

    2.  Valige **Muuda kasutajakonto kontroll sätted**.

    3.  Lohistage soovitud suunas **Teavituste**all.

    4.  Klõpsake nuppu **OK** ja seejärel valige **Jah** küsimise.

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  Faili server VMs, installige VM Agent. See on nõutav, et käivitada Azure automatiseerimine skriptide nurjunud VMs üle.

    1.  [Laadige alla agent](http://aka.ms/vmagentwin) `C:\\Users\\<username>\\Downloads`.

    2.  Avage Windows PowerShelli administraatoriõigustes (Käivita administraatorina) ja seejärel sisestage järgmine käsk Laadi alla liikuge.

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] Faili nime võivad muutuda sõltuvalt versiooni.

1.  Klõpsake nuppu **edasi**.

2.  Nõustuge **Lepingu tingimustega** ja seejärel klõpsake nuppu **edasi**.

3.  Klõpsake nuppu **valmis**.


1.  Saate luua failikettad abil uuristatud StorSimple talletusruumi maht. Lisateabe saamiseks vt [kasutamine StorSimple halduri teenuse haldamiseks maht](storsimple-manage-volumes.md).

    1.  Teie kohapealse VMs, vajutage Windowsi klahv + K ja **iSCSI**otsimine.

    2.  Valige **iSCSI algataja**.

    3.  Valige vahekaart **konfigureerimine** ja kopeerige algataja nimi.

    4.  [Azure'i klassikaline portaali](https://manage.windowsazure.com/)sisse logida.

    5.  Valige vahekaart **StorSimple** ja seejärel valige StorSimple halduri teenus, mis sisaldab füüsilise seadme.

    6.  Helitugevuse konteineri(te) ja seejärel loome draivid. (Nende maht on faili alustades failiserver VMs). Kopeerige algataja nimi ja anda juurdepääsu juhtimine kirjete jaoks sobiv nimi mahud loomisel.

    7.  Valige **Konfigureeri** menüüd ja pange kirja seadme IP-aadress.

    8.  Teie kohapealse VMs, minge **iSCSI algataja** uuesti ja sisestage jaotise Kiirühendamine IP. Klõpsake nuppu **Kiirühendamine** (seade peaks nüüd ühendatud).

    9.  Avage Azure'i haldusportaal ja valige vahekaart **mahud ja seadmed** . Klõpsake nuppu **Automaatne konfigureerimine**. Äsja loodud helitugevuse peaks kuvatama.

    10. Portaalis, klõpsake vahekaarti **seadmed** ja valige **luua uue virtuaalse seadme.** (Selle virtuaalse seadme kasutatakse Tõrkesiirde juhul). Selle uue virtuaalse seadme saab hoida eest kulude vältimiseks ühenduseta olekus. Virtuaalse seadme võrguühenduseta, liikuge jaotisse **Virtuaalmasinates** portaali sisse ja välja lülitada.

    11. Naasta kohapealse VMs ja avage ketta Management (vajutage kombinatsiooni Windowsi klahv + X ja valige **Ketas haldus**).

    12. Märkate mõne eest ketta (olenevalt maht on loodud arv). Paremklõpsake esimene, valige **Vorminda ketas**ja klõpsake nuppu **OK**. Paremklõpsake jaotist **Unallocated** , valige **Lihtne uus draiv**, määrata selle draivi nime ja viisardi juhiste täitmist.

    13. Korrake toimingut l kõigi ketta. Nüüd näete kõigi ketta **See arvuti** Windows Exploreris.

    14. Faili- ja salvestusruumi teenuste roll abil saate luua failikettad nende mahtu.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Luua ja ettevalmistamiseks on Azure saidi taastamise hoidla

Vaadake [Azure saidi taastamise dokumentatsiooni](../site-recovery/site-recovery-hyper-v-site-to-azure.md) enne kaitsmine failiserverisse VM Azure saidi taastamise alustamiseks.

#### <a name="to-enable-protection"></a>Kui soovite lubada kaitse

1.  Ühenduse katkestamine kohapealse VMs, mida soovite Azure saidi taastamise abil kaitsta iSCSI Target (s):

    1.  Vajutage klahvikombinatsiooni Windowsi klahv + K ja otsige **iSCSI**.

    2.  Valige **häälestamine iSCSI algataja**.

    3.  Eemaldage StorSimple seade ühendatud varem. Teise võimalusena saate lülitada faili server paar minutit kui kaitse lubamine.

    > [AZURE.NOTE] See võib põhjustada failikettad ajutiselt

1.  [Luba virtuaalse masina kaitse](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) faili serveri VM Azure saidi taastamise portaalist.

2.  Algse sünkroonimise alustamisel saate taastada sihtkohas uuesti. Avage iSCSI algataja, valige StorSimple seade ja klõpsake nuppu **Ühenda**.

3.  Kui sünkroonimine on lõpule viidud ja VM olek on **kaitstud**, valige VM, valige menüü **konfigureerimine** ja võrgu VM vastavalt uuendada (see on nurjunud üle VM(s) olla osa võrgus). Kui võrku ei ole nähtaval, tähendab see, sünkroonimine on ikka veel.

### <a name="enable-protection-of-storsimple-volumes"></a>Luba kaitse StorSimple mahud

Kui te pole valinud StorSimple mahud suvandi **lubamiseks vaikimisi varundamise selle maht** , avage **Varukoopia poliitikate** StorSimple halduri teenuse ja kõik mahud jaoks sobiva varukoopia poliitika loomine. Soovitame varukoopiate sageduse määramine taastamine punkti eesmärk (RPO), mida soovite näha rakenduse.

### <a name="configure-the-network"></a>Võrgu konfigureerimine

Failiserverisse VM nii, et VM võrkude on lisatud õige DR võrgu pärast Tõrkesiirde konfigureerimine Azure saidi taastamise võrgu sätteid.

Saate valida VM **VMM Cloud** või **Kaitse rühma** konfigureerida võrgu sätteid, nagu on näidatud järgmisel joonisel.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Taastamis loomine

Saate luua taastamis ASR fail aktsiate Tõrkesiirde automatiseerida. Häireid juhul saate tuua faili ühtlasi mõni minut, vaid ühe hiireklõpsuga. See automatiseerimine lubamiseks peate konto Azure automatiseerimine.

#### <a name="to-create-the-account"></a>Konto loomiseks

1.  Azure'i klassikaline portaalis ja liikuge jaotisse **automatiseerimine** .

1.  Automaatika uue konto loomine. Jätke see geo/piirkonna StorSimple Cloud seadme ja salvestusruumi kontod on loodud.

2.  Klõpsake nuppu **Uus** &gt; **Rakenduse teenuste** &gt; **automatiseerimise** &gt; **Käitusjuhendi** &gt; **Galeriist** automatiseerimise kontole kõik nõutavad tegevusraamatud importida.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  Lisage järgmised tegevusraamatud galeriis paanilt **Katastroofiabi** .

    -   Ei õnnestu üle StorSimple helitugevuse ümbriste

    -   Pärast Test Tõrkesiirde (TFO) käivitamisel võimalus StorSimple mahud puhastamiseks

    -   Pärast Tõrkesiirde StorSimple seadmes Mount mahud

    -   StorSimple virtuaalse seadme käivitamine

    -   Kohandatud skript laiendamine Azure VM desinstallimine

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Avaldada, valides käitusjuhendi automatiseerimine konto ja **Autor** vahekaardil kõik skriptid. Pärast seda toimingut **tegevusraamatud** menüü kuvatakse järgmiselt:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  Automaatika konto Avage **varad** menüü, valige **Sätte lisamine** &gt; **Mandaati lisada**, ja lisada Azure kirjeldamine – varade AzureCredential nime.

    Kasutage Windows PowerShelli mandaati. See peaks olema identimisteavet, mis sisaldab ka ettevõtte ID kasutajanimi ja parool selle Azure tellimuse juurdepääsu ja mitmikautentimise keelatud. See on nõutav autentida kasutaja nimel ajal kuvatakse failovers ja avab fail server mahud DR saidil.

1.  Automatiseerimise konto, valige menüü **varad** ja klõpsake **Lisada säte** &gt; **Lisa muutuja** ja lisage järgmised muutujad. Saate valida nende varade krüptimiseks. Need muutujad taastamise leping – konkreetset. Kui teie taastamine (mille loote Järgmises toimingus) nimi on TestPlan, siis teie muutujate peaks olema TestPlan-StorSimRegKey, TestPlan-AzureSubscriptionName jne.

    -   *RecoveryPlanName* **-StorSimRegKey**: registreerimise võti StorSimple halduri teenuse kohta.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: Azure'i tellimuse nime.

    -   *RecoveryPlanName* **-ResourceName**: StorSimple ressurss, mis sisaldab StorSimple seadme nime.

    -   *RecoveryPlanName* **-DeviceName**: seadme, mille peab olema üle nurjus.

    -   *RecoveryPlanName* **-TargetDeviceName**: The StorSimple Cloud seadme selle ümbriste on üle olla nurjus.

    -   *RecoveryPlanName* **-VolumeContainers**: komaga eraldatud tekstistring helitugevuse ümbriste Esita seadmes, kes peavad olema nurjus; näiteks volcon1, volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: target seadme teenuse nime (selle leiate jaotise **virtuaalse masina** : teenuse nimi on sama mis DNS-i nimi).

    -   *RecoveryPlanName* **-StorageAccountName**: script (mis on käitamist nurjunud üle VM) salvestatakse salvestusruumi konto nimi. See võib olla mis tahes salvestusruumi kontoga, millel on ruumi skripti ajutiselt salvestada.

    -   *RecoveryPlanName* **-StorageAccountKey**: kiirklahv ülaltoodud salvestusruumi konto.

    -   *RecoveryPlanName* **-ScriptContainer**: ümbris, kus talletatakse pilves skripti nimi. Kui ümbris pole olemas, luuakse see.

    -   *RecoveryPlanName* **-VMGUIDS**: korral VM kaitsmisele Azure saidi taastamise määrab iga VM ainu-ID, mis annab nurjunud üksikasjad üle VM. Soovitud VMGUID saamiseks valige vahekaart **Taastamise teenused** ja klõpsake **Üksust kaitstud** &gt; **Rühmade** &gt; **masinad** &gt; **Atribuudid**. Kui teil on mitu VMs, seejärel lisage selle GUID komaga eraldatud tekstistring.

    -   *RecoveryPlanName* **-AutomationAccountName** – saate lisada soovitud tegevusraamatud ja varade automatiseerimise konto nimi.

    Näiteks kui taastamise lepingu nimi on fileServerpredayRP, siis teie **varad** menüü peaks kuvatama järgmiselt kui olete lisanud kõik varad.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Liikuge jaotisse **Taastamise teenused** ja valige Azure saidi taastamise hoidla, mis on varem loodud.

2.  Valige vahekaart **Taastamine lepingud** ja luua uue lepingu taastamine järgmiselt:

    lisamine.  Määrake nimi ja valige sobiv **Kaitse rühma**.

    b.  Valige kaitse rühma, mida soovite kaasata taastamise kava VMs.

    c.  Pärast taastamis leping on loodud, valige see taastamise lepingu kohandamine vaate avamiseks.

    d.  Valige **kõik rühmad sulgumist**, klõpsake nuppu **indeks**ja valige **Lisa esmane pool skripti enne kõigi rühma sulgemine**.

    e.  Valige automatiseerimise konto (kuhu lisasite selle tegevusraamatud) ja seejärel valige **nurjuda üle StorSimple-helitugevuse ümbriste** käitusjuhendi.

    f.  Klõpsake **rühma 1: alustada**, valige **Virtuaalmasinates**ja lisada VMs, mis on kaitstud taastamise kava.

    g.  Klõpsake **rühma 1: alustada**, valige **Script**ja lisada järgmine skripte järjestuses **pärast rühma 1** juhiseid.

    - Algus-StorSimple-virtuaalse-seadme käitusjuhendi
    - Üle StorSimple-helitugevuse ümbriste käitusjuhendi nurjuda
    - Mount-mahud-pärast-Tõrkesiirde käitusjuhendi
    - Desinstallige-kohandatud-skript-laiendiga käitusjuhendi

1.  Pärast ülaltoodud 4 skriptide käsitsi toimingu lisamine samal **rühma 1: järgmised juhised** jaotis. See toiming on punkt, kus saate kontrollida, et kõik töötab õigesti. See toiming on vaja lisada ainult testi Tõrkesiirde (et ainult valige **Testida Tõrkesiirde** ruut) osana.

2.  Pärast käsitsi toimingu lisada Kettapuhastus skripti abil muu tegevusraamatud kasutatud sama toimingut. Salvestage taastamise kava.

    > [AZURE.NOTE] Kui test Tõrkesiirde, kontrollige kõike käsitsi toiming Kuna StorSimple mahud, mis oli kloonitud target seadme kustutatakse cleanup osana pärast käsitsi toiming on lõpule viidud.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Tehke test Tõrkesiirde

Vaadake [Active Directory DR lahenduse](../site-recovery/site-recovery-active-directory.md) companion juhend kaalutlused kindla Active Directory test Tõrkesiirde ajal. Kohapealse häälestus on häireid üldse test Tõrkesiirde korral. Kohapealse VM manustatud StorSimple mahud on kloonitud Azure StorSimple Cloud seadme abil. VM katsetamiseks esitatakse Azure ja kloonitud maht on lisatud VM.

#### <a name="to-perform-the-test-failover"></a>Teha test Tõrkesiirde

1.  Azure'i klassikaline portaalis, valige oma saidi taastamise hoidla.

1.  Klõpsake loodud faili serveri VM taastamise leping.

2.  Klõpsake nuppu **testi Tõrkesiirde**.

3.  Valige virtuaalse võrgu testimine Tõrkesiirde alustamiseks.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Kui teisene keskkond on loodud, saate teha oma valideerimised.

2.  Pärast selle valideerimised on lõpule jõudnud, klõpsake **Täieliku valideerimised**. Tõrkesiirde testimiskeskkonnas tuleb puhastada ja TFO toiming on lõpule viidud.

## <a name="perform-an-unplanned-failover"></a>Planeerimata Tõrkesiirde teha

Planeerimata Tõrkesiirde ajal StorSimple maht on nurjus üle virtuaalse seade, VM koopia Azure üles kasvanud ja mahud on lisatud VM.

#### <a name="to-perform-an-unplanned-failover"></a>Planeerimata Tõrkesiirde sooritamiseks

1.  Azure'i klassikaline portaalis, valige oma saidi taastamise hoidla.

1.  Klõpsake loodud faili serveri VM taastamise leping.

2.  Klõpsake **Tõrkesiirde** ja seejärel valige **Planeerimata Tõrkesiirde**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Valige target võrgus ja klõpsake nuppu kontrolli ikooni ✓ Tõrkesiirde alustamiseks.

## <a name="perform-a-planned-failover"></a>Kavandatud Tõrkesiirde teha

Kavandatud Tõrkesiirde, ajal asutusesisese faili serveri VM on sulgeda nõtkelt ja mahud StorSimple seadme varukoopia hetktõmmise tegemist pilv. StorSimple maht on nurjus üle virtuaalse seade, VM koopia esitatakse Azure ja mahud on lisatud VM.

#### <a name="to-perform-a-planned-failover"></a>Kavandatud Tõrkesiirde sooritamiseks

1.  Azure'i klassikaline portaalis, valige oma saidi taastamise hoidla.

1.  Klõpsake loodud faili serveri VM taastamise leping.

2.  Klõpsake **Tõrkesiirde** ja seejärel valige **Kavandatud Tõrkesiirde**.

3.  Valige target võrgus ja klõpsake nuppu kontrolli ikooni ✓ Tõrkesiirde alustamiseks.

## <a name="perform-a-failback"></a>Tehke soovitud failback

Ajal on failback StorSimple helitugevuse ümbriste on nurjus üle tagasi seadme pärast varukoopia.

#### <a name="to-perform-a-failback"></a>Mõne failback sooritamiseks

1.  Azure'i klassikaline portaalis, valige oma saidi taastamise hoidla.

1.  Klõpsake loodud faili serveri VM taastamise leping.

2.  Klõpsake **Tõrkesiirde** ja valige **kavandatud Tõrkesiirde** või **planeerimata Tõrkesiirde**.

3.  Klõpsake nuppu **Muuda suund**.

4.  Valige sobiv andmete sünkroonimine ja VM loomise suvandid.

5.  Klõpsake nuppu kontrolli ikooni ✓ failback alustamiseks.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Head tavad

### <a name="capacity-planning-and-readiness-assessment"></a>Võimsuse plaanimine ja valmisoleku hindamine


#### <a name="hyper-v-site"></a>Hyper-V saidi

[Kasutaja võimsus Plaanur tööriista](http://www.microsoft.com/download/details.aspx?id=39057) abil saate kujundada server, salvestusruumi ja võrgu taristule Hyper-V koopia keskkonnas.

#### <a name="azure"></a>Azure'i

[Azure virtuaalse masina valmisoleku hindamise tööriist](http://azure.microsoft.com/downloads/vm-readiness-assessment/) käivitada VMs tagamaks, et need on Azure VMs ja Azure'i saidi taastamise teenused. Tööriista valmisoleku hindamise kontrollib VM konfiguratsioone ja hoiatab kui konfiguratsioone ei ühildu Azure. Näiteks see hoiatuse, kui c-ketas on suurem kui 127 GB.


Võimsuse planeerimine koosneb vähemalt kaks olulist protsessi:

-   Vastenduse kohapealse Hyper-V VMs Azure VM suurused (nt A6, A7, A8 ja A9).

-   Nõutav Interneti-läbilaskevõime määratlemine.

## <a name="limitations"></a>Piirangud

- Praegu saab ainult 1 StorSimple seadme üle nurjus (et ühe StorSimple Cloud seadme). Stsenaarium, mis ulatub üle mitme StorSimple seadme failiserverisse veel ei toetata.

- Kui teil tekib tõrge, võimaldades VM kaitse, veenduge, et on ühendus katkestatud iSCSI eesmärgid.

- Kõik rühmitatud tõttu varukoopia poliitikad, mis ulatub üle helitugevuse ümbriste helitugevuse ümbriste kuvatakse nurjus koos üle.

- Kõik maht helitugevuse ümbriste, valitud on nurjus üle.

- Lisada rohkem kui 64 TB kuni draivid ei saa üle nurjus, kuna ühe StorSimple Cloud seadme maksimaalne maht on 64 TB.

- Plaanitud/planeerimata Tõrkesiirde nurjub ja Azure VMs on loodud, siis pole puhastamiseks VMs. Selle asemel tehke mõnda failback. Kui kustutate VMs siis kohapealse VMs ei saa sisse lülitada uuesti.

- Pärast Tõrkesiirde, kui te ei ole näha mahud, avage VMs, avage ketta haldus, on ketast uuesti ja seejärel viia need võrgus.

- Mõnel juhul võib olla erinevas tähed asutusesisese tähti DR saidil. Sel juhul peate käsitsi värskendada, kui selle Tõrkesiirde on lõpule jõudnud.

- Mitmikautentimise peaks keelatud automatiseerimise konto vara sisestatud Azure identimisteabega. Kui see autentimine on keelatud, ei lubatakse skriptide automaatselt käivituma ja taastamise kava nurjub.

- Tõrkesiirde töö ajalõpp: The StorSimple script on ajalõpuni kui helitugevuse ümbriste Tõrkesiirde võtab rohkem aega kui Azure saidi taastamise limiidist script (praegu 120 minutit).

- Varundustöö ajalõpp: The StorSimple skripti ajalõpp, kui maht varukoopia võtab rohkem aega kui Azure saidi taastamise limiidist script (praegu 120 minutit).
 
    > [AZURE.IMPORTANT] Azure'i portaalis varukoopia käsitsi käivitamine ja käivitage uuesti taastamise kava.

- Töö ajalõpp klooni: The StorSimple skripti ajalõpp, kui maht kloonimine võtab rohkem aega kui Azure saidi taastamise limiidist script (praegu 120 minutit).

- Kellaaeg sünkroonimise tõrge: The StorSimple skriptid vead välja räägivad, et varukoopiaid olid kaotanud isegi juhul, kui varundus on edukalt portaalis. Võimalik põhjus see võib olla, et StorSimple seadme kellaaeg võib olla sünkroonitud praeguse kellaaja ajavööndi.
 
    > [AZURE.IMPORTANT] Sünkroonimine seadme kellaaeg ajaga Praegune ajavöönd.

- Seadme Tõrkesiirde tõrge: The StorSimple skripti võib nurjuda, kui seal on mõne seadme Tõrkesiirde taastamise kava töötamisel.
    
    > [AZURE.IMPORTANT] Pärast seadet Tõrkesiirde on lõpule viidud, käivitage uuesti taastamise kava.

## <a name="summary"></a>Kokkuvõte

Azure'i saidi taastamise abil saate luua automatiseeritud katastroof taastamise kava failiserverisse VM on majutatud StorSimple salvestusruumi failikettad. Saate algatada selle Tõrkesiirde sekunditega igal pool korral katkemise ja saada taotlus ja töötab paar minutit.
