<properties 
   pageTitle="Hallata kontot StorSimple salvestusruumi | Microsoft Azure'i"
   description="Selgitatakse, kuidas saate lehe StorSimple halduri konfigureerimine abil saate lisada, redigeerida, kustutada või pöörata salvestusruumi konto turvalisus võtmed."
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
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>StorSimple halduri teenuse abil saate oma konto salvestusruumi haldamine

## <a name="overview"></a>Ülevaade

Lehe **konfigureerimine** esitab kõik saab luua StorSimple halduri teenuse globaalne teenuse parameetrid. Järgmiste parameetrite saab rakendada kõigis seadmetes, mis on ühendatud ja kaasata.

- Salvestusruumi kontod 
- Läbilaskevõime Mallid 
- Kirjete juurdepääsu juhtimine 

Selles õpetuses selgitatakse, kuidas saate kasutada lehe **konfigureerimine** lisada, redigeerida, või salvestusruumi kontod kustutada või pöörata salvestusruumi konto turvalisus võtmed.

 ![Lehe konfigureerimine](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Salvestusruumi kontod sisaldavad identimisteavet, mida kasutab seade, et konto salvestusruumi pilveteenuses teenusepakkuja juures. Microsoft Azure'i salvestusruumi kontode puhul neid mandaate nt konto nimi ja esmane kiirklahv. 

Lehe **konfigureerimine** kuvatakse kõik salvestusruumi kontod, arvelduse tellimuse jaoks loodud tabelina, mis sisaldab järgmist:

- **Nimi** – määrata konto, kui see on loodud kordumatu nimi.
- **SSL-i lubatud** – kas the SSL on lubatud ja seadme pilve side üle turvalise kanali.
- **Kasutab** – väljendatud salvestusruumi kontot kasutades.

Kõige levinum salvestusruumi kontod, mida saab teha lehe **konfigureerimine** seotud ülesanded on:

- Salvestusruumi konto lisamine 
- Salvestusruumi konto redigeerimine 
- Salvestusruumi konto kustutamine 
- Võtme pöördenurka salvestusruumi kontod 

## <a name="types-of-storage-accounts"></a>Salvestusruumi kontotüüpide

On kolme tüüpi salvestusruumi kontod, mida saab kasutada StorSimple seadmega.

- **Automaatselt loodud salvestusruumi kontod** – juba nime, seda tüüpi salvestusruumi konto luuakse automaatselt teenuse esmakordsel loomisel. Lisateavet selle kohta, kuidas see salvestusruumi konto on loodud, lugege teemat [Samm 1: Loo uus teenus](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) [Deploy kohapealse StorSimple seadme](storsimple-deployment-walkthrough.md)sisse. 
- **Teenuse tellimuse salvestusruumi kontod** – need on Azure salvestusruumi kontod, mis on seotud ühe tellimuse kui teenus. Lisateavet selle kohta, kuidas need salvestusruumi kontod on loodud, leiate [Azure'i salvestusruumi kontod](../storage/storage-create-storage-account.md). 
- **Väljaspool teenuse tellimuse salvestusruumi kontod** – need on Azure salvestusruumi kontod, mis on seotud teie teenusega ja tõenäoliselt olemas enne seda, kui teenus on loodud.

## <a name="add-a-storage-account"></a>Salvestusruumi konto lisamine

Saate lisada salvestusruumi konto kordumatu sõbralik nimi ja Accessi mandaat, mis on lingitud konto salvestusruumi (koos määratud pilveteenuse pakkuja). Teil on ka võimaldada turvasoklite kiht (SSL) režiimi võimalus luua turvalise kanali jaoks võrguühenduse seadme ja pilveteenuse vahel.

Saate luua mitme konto on antud pilveteenuse pakkuja. Pange tähele, siiski, et pärast salvestusruumi konto loomist ei saa muuta pilveteenuse pakkuja.

Ajal salvestusruumi konto salvestatakse teenuse proovib suhelda cloud teenusepakkujalt. Mandaadi ja Accessi materjali, mida olete sisestanud on kinnitatud sel ajal. Salvestusruumi konto on loodud ainult siis, kui autentimine õnnestus. Kui autentimine nurjub, siis vastav tõrketeade kuvatakse.

Ressursihaldur salvestusruumi kontod Azure portaali loodud on toetatud ka StorSimple. Ressursihaldur salvestusruumi kontod ei kuvata ripploendis loendist valik kui loomise helitugevuse container, ainult salvestusruumi kontod loodud Azure klassikaline portaalis kuvatakse. Ressursihaldur salvestusruumi kontod tuleb lisada salvestusruumi konto allpool kirjeldatud protseduuri abil.

> [AZURE.NOTE] Protseduur salvestusruumi konto lisamisel väärtus erineb sõltuvalt StorSimple tarkvara versiooni te kasutate. Kindlasti õige toiminguteks oma StorSimple versioon.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Salvestusruumi konto redigeerimine

Saate redigeerida salvestusruumi konto, helitugevuse container kasutatavat. Kui redigeerite salvestusruumi konto, mis on praegu kasutusel, on saadaval muuta ainult välja kiirklahv salvestusruumi konto. Saate uue salvestusruumi kiirklahv esitama ja värskendatud sätted salvestada.

#### <a name="to-edit-a-storage-account"></a>Salvestusruumi konto redigeerimiseks

1. Klõpsake teenuse sihtleht, valige teenust, topeltklõpsake teenuse nimi ja klõpsake nuppu **Konfigureeri**.

2. Valige **Kontod salvestusruumi lisamine või redigeerimine**.

3. Dialoogiboksis **Lisa/redigeeri salvestusruumi kontod** järgmist.

  1. **Salvestusruumi**kontode ripploendist valige olemasolev konto, mida soovite muuta. See võib hõlmata ka salvestusruumi kontod teenuse esmakordsel loomisel automaatselt loodud.
  2. Vajaduse korral saate muuta valik **Luba SSL-i režiim** .
  3. Kui soovite, et pöörata salvestusruumi konto kiirklahvide. Leiate lisateavet selle kohta, kuidas teha võtme pööre [klahvi pöördenurka salvestusruumi kontod](#key-rotation-of-storage-accounts) .
  4. Klõpsake ikooni Otsi ![kontrollida ikooni](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) sätete salvestamiseks. Sätete värskendatakse lehe **konfigureerimine** . Klõpsake nuppu **Salvesta** värskendatud sätete salvestamiseks.

     ![Salvestusruumi konto redigeerimine](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Salvestusruumi konto kustutamine

> [AZURE.IMPORTANT] Salvestusruumi konto saate kustutada ainult juhul, kui seda ei kasutata helitugevuse container järgi. Kui salvestusruumi konto kasutab helitugevuse container, esmalt kustutada helitugevust ja seejärel kustutage kontoga seostatud salvestusruum.

#### <a name="to-delete-a-storage-account"></a>Salvestusruumi konto kustutamine

1. Klõpsake StorSimple halduri teenuse sihtleht, valige teenust, topeltklõpsake teenuse nimi ja klõpsake nuppu **Konfigureeri**.

2. Tabeli loendit salvestusruumi kontod, viige kursor konto, mille soovite kustutada.

3. Salvestusruumi konto jaoks äärmiselt paremas veerus kuvatakse ikooni Kustuta (**x**). Mandaadi kustutamiseks ikooni **x** .

4. Kinnituse küsimisel nuppu **Jah** kustutamise jätkamiseks. Muudatuste värskendatakse tabeli kirjet.

## <a name="key-rotation-of-storage-accounts"></a>Klahv pöördenurka salvestusruumi kontod

Turvalisuse huvides võtme pööre on sageli andmekeskuste nõue. 

> [AZURE.NOTE] Võtme pööre järgmine teave ja pööre toimingu rakendamiseks ainult Microsoft Azure'i salvestusruumi kontod. Kui kasutate muu pilveteenuse pakkuja, saate hallata salvestusruumi konto võtmed selle teenusepakkuja armatuurlaua kaudu.
 
Iga Microsoft Azure'i tellimus võib olla üks või mitu seotud salvestusruumi kontod. Juurdepääs nende kontodega kontrollib iga salvestusruumi konto tellimust ja Accessi võtmed. 

Salvestusruumi konto loomisel Microsoft Azure'i loob kaks 512-bitine salvestusruumi Accessi tutvustatakse, mida kasutatakse autentimiseks konto salvestusruumi juurde. Kahe salvestusruumi kiirklahvide võttes saate taastada ilma katkestusteta salvestusruumi teenust või teenuse juurdepääsu võtmed. Varukoopia võti nimetatakse *teisese* võtme klahvi, mis on praegu kasutusel on *primaarvõti* . Nende kahe kiirklahvidele tuleb esitada kui Microsoft Azure StorSimple seadme nimetus pilve salvestusruumi teenusepakkuja.

## <a name="what-is-key-rotation"></a>Mis on pööre tootenumbri?

Tavaliselt rakendusi kasutada ainult üks pääsevad oma andmetele juurde. Mõne kindla ajavahemiku järel, saate määrata rakenduste üle minna teise võtme abil. Pärast rakenduste teisene klahv on sisse lülitatud, saate pensionile esimene võti ja seejärel luua uus võti. Kahe klahvidega sellisel viisil võimaldab ilma, et tekiks mis tahes oma rakenduste juurdepääsu andmetele.

Salvestusruumi konto klahvid alati talletatakse teenuse krüptitud kujul. Siiski saate need lähtestada StorSimple halduri teenuse kaudu. Teenuse pääsevad primaarvõtme ja teisese võtme salvestusruumi kõigi kontode puhul sama tellimuse, sh loodud kui StorSimple halduri teenuse teenuse esmakordselt loodud loodud salvestusruumi teenuse kui ka vaikimisi salvestusruumi kontod. StorSimple halduri teenus on alati klahve Azure klassikaline portaalist ja neid krüptitud viisil talletada.

## <a name="rotation-workflow"></a>Pöörde töövoo

Microsoft Azure'i administraator saab taastada või muutke põhi- või võti otse juurdepääs salvestusruumi konto (Microsoft Azure Storage service) kaudu. StorSimple halduri teenuse ei näe selle muudatuse automaatselt.

Teavitada StorSimple halduri teenuse muudatust, teil on vaja StorSimple halduri teenuse, salvestusruumi kontole juurdepääsemiseks ja seejärel sünkroonige esmane või teisene võti (sõltuvalt sellest, millise neist on muudetud). Teenuse siis saab uusimate võti, krüptib võtmed ja saadab krüptitud võti seade.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Sünkroonida klahvid sama nimega (ainult Azure) teenuse tellimuse salvestusruumi kontod

1. **Teenuste** lehel, klõpsake vahekaarti **konfigureerimine** .

2. Valige **Kontod salvestusruumi lisamine või redigeerimine**.

3. Dialoogiboksis, tehke järgmist.

  1. Valige salvestusruumi konto võti, mida soovite sünkroonida. Salvestusruumi konto võtmed on krüptitud, kui need on kuvatud.
  2. StorSimple halduri teenus, peate värskendama klahvi, mis on varem muutnud teenuses Microsoft Azure'i Tabelimäluga. Kui esmane kiirklahv on muudetud (regenereeritud), klõpsake nuppu **Sünkrooni primaarvõtme**. Teisene klahvi muutmisel klõpsake **teisese võtme sünkroonida**.

    ![sünkroonimine võtmed](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Klahvid väljaspool teenuse tellimuse salvestusruumi kontode sünkroonimiseks

1. **Teenuste** lehel, klõpsake vahekaarti **konfigureerimine** .

2. Valige **Kontod salvestusruumi lisamine või redigeerimine**.

3. Dialoogiboksis, tehke järgmist.

  1. Valige konto salvestusruumi kiirklahv, mida soovite värskendada.
  2. Kas peate värskendama salvestusruumi kiirklahv StorSimple halduri teenus. Sel juhul näete kiirklahv salvestusruumi. Sisestage väljale **Talletusmahu konto kiirklahv**y uue tootenumbri. 
  3. Salvestage muudatused. Nüüd peaks teie salvestusruumi konto kiirklahv uuendada.

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet [StorSimple turvalisus](storsimple-security.md).
- Lisateavet [hübriidjuurutuses StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
