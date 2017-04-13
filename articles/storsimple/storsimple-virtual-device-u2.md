<properties 
   pageTitle="StorSimple virtuaalse seadme Update 2 | Microsoft Azure'i"
   description="Saate teada, kuidas luua, juurutada ja hallata StorSimple virtuaalne seadme Microsoft Azure virtuaalse võrku. (Kehtib StorSimple Update 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Juurutada ja hallata Azure StorSimple virtuaalne seadme


##<a name="overview"></a>Ülevaade
StorSimple 8000 sarja virtuaalse seade on täiendavad võimalus, kaasneva oma Microsoft Azure'i StorSimple lahendus. StorSimple virtuaalse seade töötab Microsoft Azure virtuaalse võrgu virtuaalse masina ja saate varundada ja andmed teie hosts klooni. Selles õpetuses kirjeldatakse, kuidas juurutada ja hallata virtuaalse seadme Azure ja tarkvara versiooniga Update 2 virtuaalse seadmed ja lower.


#### <a name="virtual-device-model-comparison"></a>Virtuaalne seadme andmemudeli võrdlus

StorSimple virtuaalse seade on saadaval kaks mudelit, standard 8010, (varem on 1100) ja lisatasu 8020 (kasutusele Update 2). Allpool on esitatud tabelina kahe mudeli võrdlus.


| Seadme mudelist          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Maksimaalselt**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure'i VM**              | Standard_A3 (4 tuuma, 7 GB mälu)                                                                      | Standard_DS3 (4 tuuma, 14 GB mälu)                                                                                                                          |
| **Versiooniühilduvus** | Versioonid töötavad värskendada eelnevalt 2 või uuem versioon                                             | Värskenduste käivitamine, 2 või uuemad versioonid                                                                                                  |
| **Piirkonna-saadavus**   | Azure'i kõigi piirkondade                                                         | Azure'i regioonid, mis toetavad Premium salvestusruum<br></br>Piirkondade loendi leiate teemast [toetatud regioonide 8020](#supported-regions-for-8020) |
| **Salvestusruumi tüüp**          | Kasutab Azure Standard salvestusruumi kohaliku ketast<br></br> Siit saate teada, [kindlad konto loomise]() kohta | Kasutab Azure Premium Storage kohaliku ketast<sup>2</sup> <br></br>Siit saate teada, [Premium salvestusruumi konto loomise](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk) kohta                                                               |
| **Töökoormus juhised**     | Üksuse taseme varukoopiate failide toomine                                              | Pilvepõhine arendaja ja stsenaariumid, madal latentsus, suurema jõudluse töökoormus testimine <br></br>Teisene seadme katastroofiabi                                                                                            |
 
<sup>1</sup> *on 1100 varem*.

<sup>2</sup> *soovitud 8010 nii 8020 kasutada Azure Standard salvestusruumi pilveteenuses taseme. Erinevus on olemas ainult kohaliku taseme jooksul seade*.

#### <a name="supported-regions-for-8020"></a>Toetatud regioonide 8020 jaoks

Allpool on esitatud tabelina Premium salvestusruumi regioonid, mis on praegu toetatud 8020. Selles loendis värskendatakse pidevalt Premium salvestusruumi vabanemisest teavitamise rohkem piirkondades. 

| S. Ei.                                                  | Praegu toetatud Regioonide |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | Kesk-USA                     |
| 2                                                       |  Ida-USA                       |
| 3                                                       |  Ida-USA 2                     |
| 4                                                       | Lääne USA.                        |
| 5                                                       | Põhja-Euroopa                   |
| 6                                                       | Lääne Euroopa                    |
| 7                                                       | Kagu-Aasia                 |
| 8                                                       | Jaapan Ida                     |
| 9                                                       | Jaapan Lääne                     |
| 10                                                      | Austraalia Ida                 |
| 11                                                      | Austraalia kodutee *           |
| 12                                                      | Ida-Aasia *                     |
| 13                                                      | Lõuna keskse U.S. *              |

* Premium salvestusruumi käivitati viimati nende geos.

Selles artiklis kirjeldatakse samm-sammult StorSimple virtuaalne seadme Azure juurutamine. Pärast selle artikli lugemist, siis:

- Mõista, kuidas virtuaalse seadme erineb seadme.

- Võimalik, loomiseks ja konfigureerimiseks virtuaalse seade.

- Virtuaalse seadmega ühendada.

- Saate teada, kuidas töötada virtuaalse seade.

Selle õpetuse kehtib kõigi StorSimple virtuaalse seadmete töötab Update 2 ja uuem versioon. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Kuidas virtuaalse seadme erineb seadme

StorSimple virtuaalse seade on StorSimple, mis töötab ühe sõlme rakenduses Microsoft Azure virtuaalse masina tarkvara ainult versiooni. Virtuaalne seadme toetab Avariijärgne taaste stsenaariumid, kus teie seadme pole saadaval ja on kasutada Üksusetaseme andmetoomisteenused varukoopiate, katastroofiabi kohapealse ja pilveteenuse arendaja ja testi stsenaariumid.

#### <a name="differences-from-the-physical-device"></a>Seadme erinevused

Järgmises tabelis on mõned peamised erinevused StorSimple virtuaalse seadme ja StorSimple seadme vahel.

|                             | Seadme                                          | Virtuaalne seadme                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Asukoht**                    | Asub andmekeskuses.                               | Azure'i töötab.                                                                            |
| **Võrgu liidesed**          | On kuus võrgu liidesed: andmete 0 – andmed 5.                  | On ainult üks võrk kasutajaliides: andmete 0.                                        |
| **Registreerimine**                | Registreeritud konfiguratsiooni sammu ajal.                | Registreerimise on eraldi ülesanne.                                                          |
| **Teenuse andmete krüptovõtme** | Füüsilise seadme taastada, ja seejärel värskendage virtuaalse seadme uus võti.           | Ei saa taastada virtuaalse seadme kaudu. |


## <a name="prerequisites-for-the-virtual-device"></a>Virtuaalne seadme eeltingimused

Järgmistes jaotistes kirjeldatakse StorSimple virtuaalse seadme konfiguratsiooni eeltingimused. Enne võtavad virtuaalse seadet, lugege läbi [turvakaalutlused virtuaalse seadme abil](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Azure'i nõuded

Enne, kui olete ette virtuaalse seade, peate tegema järgmised ettevalmistused Azure keskkond:

- Virtuaalne seadme, [virtuaalse võrgu Azure konfigureerimine](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Kui Premium salvestusruumi, peate looma virtuaalse võrgu Azure piirkond, mis toetab Premium mälu. Lisateavet [regioonid, mis on praegu toetatud 8020](#supported-regions-for-8020).
- See on soovitatav kasutada vaikimisi DNS-i server Azure'i esitatud asemel täpsustades oma DNS-i serveri nimi. Kui teie DNS-i serveri nimi ei sobi või kui DNS-i server ei saa IP-aadressi lahendamiseks õigesti, virtuaalse seadme loomine nurjub.
- Punkti saidi ja saidilt on valikuline, kuid ei vaja. Soovi korral saate konfigureerida täpsemate stsenaariumid suvanditest. 
- Saate luua virtuaalse võrgu virtuaalse seadme esitatud mahud kasutavate [Azure'i Virtuaalmasinates](../virtual-machines/virtual-machines-linux-about.md) (hosti serverid). Järgmistes serverites peab vastama järgmistele nõuetele.                            
    - ISCSI algataja tarkvara on Windowsi ja Linux VMs.
    - Arvutisse olema installitud samasse võrku virtuaalse virtuaalse seade.
    - Võimalik ühenduse iSCSI target virtuaalse seadme kaudu virtuaalse seadme sisemine IP-aadress.

- Veenduge, et olete konfigureerinud tugi iSCSI ja pilveteenuse liikluse samas virtuaalse võrgus.


#### <a name="storsimple-requirements"></a>StorSimple nõuded

Enne luua virtuaalne seadme teha järgmised värskendused Azure StorSimple teenust.


- [Accessi juhtelement kirje](storsimple-manage-acrs.md) lisamiseks vms, mida saab olema hosti serverid virtuaalse seadme.

- Kasutada sama piirkonna virtuaalse seadme [salvestusruumi konto](storsimple-manage-storage-accounts.md#add-a-storage-account) . Erinevate piirkondade salvestusruumi kontod võib kaasa tuua jõudlus. Standard- või Premium salvestusruumi konto saate kasutada virtuaalse seadmega. Lisateavet on [kindlad konto]() või [Premium salvestusruumi konto](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk) loomine

- Erinevate salvestusruumi konto abil virtuaalse seadme loomine, mida kasutatakse teie andmete jaoks. Salvestusruumi kontot kasutades võib kaasa tuua jõudlus.

Veenduge enne alustamist, et teil on järgmine teave:

- Azure'i klassikaline portaali konto juurdepääsu mandaati.

- Teenuse andmete krüptovõtme füüsilise seadmest koopia.


## <a name="create-and-configure-the-virtual-device"></a>Loomine ja konfigureerimine virtuaalse seade

Nende toimingute sooritamiseks, veenduge, et, et olete jõudnud [eeltingimuste virtuaalse seade](#prerequisites-for-the-virtual-device). 

Kui olete loonud virtuaalse, konfigureeritud StorSimple halduri teenuse ja teenuse füüsilise StorSimple seadme registreerinud, saate järgmiste juhiste loomiseks ja konfigureerimiseks StorSimple virtuaalse seade. 

### <a name="step-1-create-a-virtual-device"></a>Samm 1: Luua virtuaalse seade

Järgmiste toimingute StorSimple virtuaalse seadme loomiseks.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Virtuaalne seadme loomine nurjumisel selles etapis tuleb teil pole Interneti-ühendus. Lisateabe saamiseks minge [probleemide tõrkeotsing Interneti ühenduvus](#troubleshoot-internet-connectivity-errors) kui creatig virtuaalse seade.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Samm 2: Konfigureerimine ja virtuaalse seadme registreerimine

Enne toimingu alustamist veenduge, et teil on teenuse andmete krüptovõtme koopia. Teenuse andmete krüptovõtme loodi esimese StorSimple seadme konfigureeritud ning teil ülesandeks salvestage see turvalises asukohas. Kui teil pole teenuse andmete krüptovõtme koopia, tuleb teil ühendust võtta Microsoft Support abi.

Järgmiste toimingute StorSimple virtuaalse seadme registreerimine ja konfigureerimiseks.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Samm 3: (Valikuline) Modify seadme sätete konfigureerimine

Järgmises jaotises kirjeldatakse seadme konfiguratsioonisätted vaja selle StorSimple virtuaalse seade, kui soovite kasutada Peatükk, StorSimple hetktõmmise halduri või seadme administraatori parooli muutmine.

#### <a name="configure-the-chap-initiator"></a>Algataja CHAP konfigureerimine

See parameeter sisaldab identimisteavet, mida algataja (serverid), mida proovite juurde pääseda mahud ootab virtuaalse seadme (sihtrentnik). Selle algataja annab CHAP kasutajanime ja ise oma seadmesse tuvastamiseks see autentimisel CHAP parool. Üksikasjalikud juhised leiate Avage [Seadme Peatükk konfigureerimine](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Sihtrakenduse CHAP konfigureerimine

See parameeter sisaldab identimisteavet, mida seadme virtuaalse kasutab CHAP lubatud algataja taotleb vastastikune või kahesuunaline autentimist. Virtuaalne seadme kasutada tühistada Peatükk kasutajanime ja parooli tühistada Peatükk tuvastamiseks ise algataja autentimise käigus. Pange tähele, et CHAP sihtrakenduse sätted on Globaalsätete. Kui need on rakendatud, kasutatakse kõigi ühendatud virtuaalse mäluseadmesse mahud CHAP autentimist. Üksikasjalikud juhised leiate Avage [Seadme Peatükk konfigureerimine](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>StorSimple hetktõmmise halduri parooli konfigureerimine

StorSimple hetktõmmise Manager tarkvara asub Windows Server ja saavad administraatorid varukoopiaid kohalik kujul StorSimple seadme haldamiseks ja Pilveteenusest hetktõmmiseid luua.

>[AZURE.NOTE] Virtuaalse seadme oma Windows host on mõni Azure virtuaalse masina.

Seadme konfigureerimisel StorSimple hetktõmmise halduris, palutakse teil StorSimple seadme IP-aadress ja parool oma mäluseadmesse autentida. Üksikasjalikud juhised, minge [konfigureerimine StorSimple hetktõmmise halduri parool](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Seadme administraatori parooli muutmine 

Windows PowerShelli kasutajaliidese kasutamisel virtuaalse seadme juurdepääsu tuleb sisestada seadme administraatori parooli. Andmete turvalisus, teil on vaja enne virtuaalse seadme saab kasutada seda parooli muuta. Üksikasjalikud juhised, minge [konfigureerimine seadme administraatori parooli](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Kaugühenduse teel virtuaalse seadmega ühendada
Kaugjuurdepääs virtuaalse seadme kaudu Windows PowerShelli kasutajaliideses on vaikimisi lubatud. Peate luba kaughalduse virtuaalse seadme esmalt, ja seejärel selle juurdepääsu virtuaalse seadme kasutatud klientarvutis.

Kontrollimiseks kaugühenduse teel ühenduse on allpool.

### <a name="step-1-configure-remote-management"></a>Samm 1: Konfigureerimine kaughalduse

Järgmiste toimingute kaughalduse StorSimple virtuaalse seadme konfigureerimiseks.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Samm 2: Kaugjuurdepääs virtuaalse seade

Pärast seda, kui olete lubanud kaughalduse lehel StorSimple seadme konfiguratsioon, saate Windows PowerShelli remoting ühenduse virtuaalse seadme teise virtual masina sees virtuaalse samasse võrku; Näiteks saate ühendada Host VM, mida konfigureeritud ja ühenduse iSCSI. Enamik kasutuselevõttu, kas on juba avatud avaliku lõpp-punkti juurdepääsu oma hosti VM, mida saate kasutada juurdepääsuks virtuaalse seade.

>[AZURE.WARNING] **Turvalisuse, soovitame kasutada HTTPS-i lõpp-punktid ühendamisel ja seejärel kustutage lõpp-punktid, kui olete lõpetanud oma PowerShelli Kaugseanss.**

Peate järgima [ühenduse loomine kaugühenduse teel StorSimple seadme](storsimple-remote-connect.md) häälestamiseks remoting virtuaalse seadme toodud juhised.

## <a name="connect-directly-to-the-virtual-device"></a>Virtuaalne seadme ühendamine

Saate ka luua ühenduse otse virtuaalse seade. Kui soovite ühendada otse virtuaalse seadme mõnest teisest arvutist väljaspool virtuaalse võrgu või väljaspool Microsoft Azure keskkonnas, peate looma täiendavad lõpp-punktid, nagu on kirjeldatud järgmises toimingus. 

Järgmiste toimingute virtuaalse seadme avaliku lõpp-punkti loomiseks.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Soovitame, et loote teise virtual masina sees virtuaalse samasse võrku, kuna see tava minimeeritakse avaliku lõpp-punktid virtuaalse võrgus arv. Kui seda meetodit kasutada lihtsalt ühendada virtuaalse masina kaudu kaugtöölaua ja konfigureerimist selle virtuaalse masina kasutamiseks sarnaselt muude Windowsi kliendi kohalikus võrgus. Teil pole vaja lisamine avaliku pordi number, kuna pordi juba teada.

## <a name="work-with-the-storsimple-virtual-device"></a>StorSimple virtuaalse seadme töötamine

Nüüd, kui teil on loodud ja konfigureeritud StorSimple virtuaalse seadme, olete sellega töötamise alustamiseks valmis. Saate töötada helitugevuse ümbriste ja mahud varukoopia poliitikate virtuaalse seadmes, nagu teeksite füüsilise StorSimple seadmes; ainus erinevus on, peate veenduge, et valite loendist oma seade virtuaalse seadme. Vaadake [StorSimple halduri teenuse virtuaalne seadme haldamiseks kasutada](storsimple-manager-service-administration.md) erinevaid haldamise toiminguid, virtuaalse seadme üksikasjalikud juhised.

Järgmistes lõikudes käsitletakse ilmneda virtuaalse seadme töötades erinevusi.

### <a name="maintain-a-storsimple-virtual-device"></a>StorSimple virtuaalse seadme haldamine

Kuna see on ainult tarkvara seade, virtuaalse seadme hooldus on minimaalne hooldus seadme võrdlemisel. Teil järgmised võimalused.

- **Tarkvaravärskendusi** – saate vaadata viimati värskendati tarkvara, koos värskendamise oleku sõnumeid. Lehe allosas **skannimine värskenduste** nupu abil saate teha käsitsi kontrollimiseks, kui soovite uute värskenduste. Kui värskendused on saadaval, klõpsake nuppu **Installi värskendused** installida. Kuna seal on ainult üks kasutajaliidese virtuaalse seadmes, see tähendab, et seal kerge teenuse peatamine värskendused on rakendatud. Virtuaalse seade sulgeda ja uuesti rakendada värskendusi, mis on välja (vajadusel). Üksikasjalikud, minge [oma seadet värskendada](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Tugiteenuste pakett** – saate luua ja aidata Microsoft Support tõrkeotsing virtuaalse seadmega tugi paketi üleslaadimine. Avage üksikasjalikud, [luua](storsimple-create-manage-support-package.md)ja hallata tugiteenuste pakett.

### <a name="storage-accounts-for-a-virtual-device"></a>Virtuaalne seadme salvestusruumi kontod

Salvestusruumi kontod on loodud StorSimple halduri teenuse, virtuaalse seadme ja seadme kasutamiseks. Kui loote oma salvestusruumi kontod, soovitame kasutada tagada piirkonna vastab süsteemi komponentide kogu sõbralik nimi piirkond identifikaator. Virtuaalne seadme, on oluline, et kõik komponendid piirkonna jõudlusprobleemide vältimiseks.

Üksikasjalikud, avage [salvestusruumi konto lisamine](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>StorSimple virtuaalse seadme desaktiveerimine

Virtuaalne seadme desaktiveerides kustutab VM ja ressursse, mis on loodud, kui see on ette valmistatud. Pärast virtuaalse seade on välja lülitatud, ei saa seda taastada eelmine olek. Peatamiseks või kustutada kliendid ja hakkab majutama, mis sõltuvad veenduge enne virtuaalse seadme väljalülitamiseks.

Virtuaalne seadme desaktiveerides tulemiks järgmisi toiminguid:

- Virtuaalne seade on eemaldatud.

- OS kettale ja andmete ketast loodud virtuaalse seadme eemaldatakse.

- Majutatud teenuse ja virtuaalse võrgu ettevalmistamise ajal loodud säilivad. Kui te ei kasuta neid, peaks need käsitsi kustutada.

- Pilveteenuse hetktõmmiseid loodud virtuaalse seadme säilitatakse.

Üksikasjalikud, minge [Desaktiveeri ja kustutada seadme StorSimple](storsimple-deactivate-and-delete-device.md).

Kui virtuaalse seade on kujutatud inaktiveeritud StorSimple halduri teenuse lehel, saate kustutada virtuaalse seade seadme loendist lehel **seadmed** .


### <a name="start-stop-and-restart-a-virtual-device"></a>Käivitage, lõpetamine ja virtuaalne seadme taaskäivitamine
Erinevalt StorSimple seadme, on pole power sees või lülitage nuppu push StorSimple virtuaalse seadmes. Siiski võib korduvalt kuhu soovite peatada ja virtuaalse taaskäivitada. Näiteks mõned värskendused võivad nõuda VM taaskäivitada värskenduse lõpuleviimiseks. Lihtsaim viis teile hakata, Peata ja taaskäivitage virtuaalse seade on Virtuaalmasinates halduskonsooli kasutamise kohta.

Kui vaatate Management Console, virtuaalse seadme olek sellepärast **töötab** see käivitatakse vaikimisi pärast selle loomist. Saate alustada, peatada ja taaskäivitage virtuaalse masina igal ajal.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Algsätted lähtestamine

Kui otsustate, et soovite lihtsalt uuesti alustamiseks virtuaalse seadmega, lihtsalt desaktiveerige ja kustutage see ja seejärel looge uus. Nii nagu teie seadme reset uue virtuaalse seadme ei tohi olla värskendusi installitud; Seega veenduge, et värskendusi enne selle kasutamist.


## <a name="fail-over-to-the-virtual-device"></a>Ei õnnestu üle virtuaalse seadmesse

Avariitaastet (DR) on üks loetletud tähtsaimad stsenaariumid, mis loodi StorSimple virtuaalse seadme jaoks. Selle stsenaariumi korral StorSimple seadme või kogu andmekeskuse ei pruugi olla saadaval. Õnneks saate virtuaalne seadme toimingute alternatiivse asukoha taastamiseks. Ajal DR, helitugevuse ümbriste allikas seadme omaniku muutmine ja on üle virtuaalse seadme. DR eeltingimused on, et virtuaalse seadmes on loodud ja konfigureeritud, kõik mahud helitugevuse container sees on võetud ühenduseta ja container helitugevus on sellega seotud cloud hetktõmmis.

>[AZURE.NOTE] 
>
> - Virtuaalne seadme kasutamisel teisene mobiilsideseadme jaoks DR, pidage meeles, et selle 8010 on 30 TB salvestusruumi Standard ja 8020 on 64 TB salvestusruumi Premium. Suurem võimsus 8020 virtuaalse seade võib olla paremini DR stsenaariumi.
> - Te ei saa Tõrkesiirde või klooni seadmest, töötab seade töötab eelse Update 1 tarkvara Update 2. Teil siiski ei õnnestu üle seadet, kus töötab Update 2 seade töötab Update 1 (1.1 või 1.2)

Üksikasjalikud, minge [Tõrkesiirde virtuaalse seadmesse](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Sulgeda või kustutada virtuaalse seade

Kui eelnevalt konfigureeritud ja kasutada mõnda StorSimple virtuaalse seade, kuid nüüd soovite lõpetada, Arvuta kulude selle kasutamise, saate sulgeda virtuaalse seade. Sulgemise virtuaalse seade ei kustutata oma operatsioonisüsteemi või andmete ketta jaotise salvestusruumi. Selle peatamine kulud oma tellimuse, mis pärinevad, kuid salvestusruumi eest OS- ja ketast kasutab ka edaspidi.

Kui kustutate või virtuaalse seadme välja lülitada, see kuvatakse **ühenduseta** StorSimple halduri teenuse lehel seadmed. Saate valida desaktiveerida või kustutada seadme, kui soovite kustutada varukoopiaid, mis on loodud virtuaalse seadme. Lisateavet leiate teemast [Desaktiveeri ja Kustuta StorSimple seadme](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Interneti-ühenduvuse tõrkeotsing 

Virtuaalne seadme loomise ajal kui on ühendus Internetiga, puudub loomise etappi nurjub. Tõrkeotsing, kui selle põhjuseks on Interneti-ühendus, tehke järgmist Azure klassikaline portaalis:

1. Azure'i Windows server 2012 virtuaalse masina loomine Selle virtuaalse masina tuleks kasutada sama salvestusruumi konto, VNet ja alamvõrgu nagu virtuaalse seade kasutab. Kui teil on juba mõne olemasoleva Windows Server host Azure sama salvestusruumi konto, Vnet ja alamvõrku, saate seda ka Interneti-ühenduse tõrkeotsingu.
2. Remote log eelmises juhises loodud virtuaalne kohale. 
3. Avage käsuviiba aken virtual seadmes (Win + R ja seejärel tippige `cmd`).
4. Käivitage järgmised cmd.

    `nslookup windows.net`

5. Kui `nslookup` nurjub, siis Interneti ühenduvus tõrge takistab virtuaalse seadme registreerimine StorSimple halduri teenusega. 
6. Veenduge, et virtuaalse seade on juurdepääs Azure'i saidid, näiteks "windows.net" virtuaalse võrgu vajalikud muudatused teha.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [virtuaalse seadme haldamine StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
 
- Mõista [StorSimple helitugevuse hulgast varukoopia põhjal](storsimple-restore-from-backup-set.md)taastamine. 

