<properties 
   pageTitle="Hallata kontot StorSimple salvestusruumi | Microsoft Azure'i"
   description="Selgitatakse, kuidas saate StorSimple halduri konfigureerimine lehe lisada, redigeerida, kustutada või pöörata seostatud StorSimple virtuaalse massiiv salvestusruumi konto turvalisus võtmed."
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
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>Hallata salvestusruumi kontod StorSimple virtuaalse massiiv StorSimple halduri teenuse abil

## <a name="overview"></a>Ülevaade

Lehe **konfigureerimine** esitab StorSimple halduri teenus loodud globaalne teenuse parameetrid. Järgmiste parameetrite saab rakendada kõigis seadmetes, mis on ühendatud ja kaasata.

- Salvestusruumi kontod 
- Kirjete juurdepääsu juhtimine 

Selle õpetuse selgitab, kuidas saate lehe **konfigureerimine** lisamine, redigeerimine või kustutamine salvestusruumi kontod oma StorSimple virtuaalse massiiv. Selles õpetuses teave kehtib ainult StorSimple virtuaalse massiiv märts 2016 GA väljaanne tarkvara.

 ![Lehe konfigureerimine](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Salvestusruumi kontod sisaldavad identimisteavet, mida kasutab seade, et konto salvestusruumi pilveteenuses teenusepakkuja juures. Microsoft Azure'i salvestusruumi kontode puhul neid mandaate nt konto nimi ja esmane kiirklahv. 

Lehe **konfigureerimine** kuvatakse kõik salvestusruumi kontod arveldustoe tellimuse jaoks loodud tabelina, mis sisaldab järgmist:

- **Nimi** – määrata konto, kui see on loodud kordumatu nimi.
- **SSL-i lubatud** – kas the SSL on lubatud ja seadme pilve side üle turvalist kanalit.

Kõige levinum salvestusruumi kontod, mida saab teha lehel **konfigureerimine** seotud ülesanded on:

- Salvestusruumi konto lisamine 
- Salvestusruumi konto redigeerimine 
- Salvestusruumi konto kustutamine 


## <a name="types-of-storage-accounts"></a>Salvestusruumi kontotüüpide

On kolme tüüpi salvestusruumi kontod, mida saab kasutada StorSimple seadmega.

- **Automaatselt loodud salvestusruumi kontod** – juba nime, seda tüüpi salvestusruumi konto luuakse automaatselt teenuse esmakordsel loomisel. Lisateavet selle kohta, kuidas see salvestusruumi konto on loodud, lugege teemat [Loo uus teenus](storsimple-ova-manage-service.md#create-a-service). 
- **Teenuse tellimuse salvestusruumi kontod** – need on Azure salvestusruumi kontod, mis on seotud ühe tellimuse kui teenus. Lisateavet selle kohta, kuidas need salvestusruumi kontod on loodud, leiate [Azure'i salvestusruumi kontod](../storage/storage-create-storage-account.md). 
- **Väljaspool teenuse tellimuse salvestusruumi kontod** – need on Azure salvestusruumi kontod, mis on seotud teie teenusega ja tõenäoliselt olemas enne seda, kui teenus on loodud.

Iga StorSimple virtuaalse massiiv loob kontoga seostatud salvestusruumi (mille eesliite hcs). Selles ümbrises on cloud andmed teie seadme jaoks. Kustutada selle container järgi juurdepääs Azure'i salvestusteenus kaudu, kui see toiming tulemuseks andmekao.

## <a name="add-a-storage-account"></a>Salvestusruumi konto lisamine

Salvestusruumi konto lisamiseks teie haldur StorSimple teenuse konfigureerimine kordumatu sõbralik nimi ja Accessi identimisteave, mis on lingitud konto salvestusruumi. Teil on ka võimaldada turvasoklite kiht (SSL) režiimi võimalus luua turvalise kanali jaoks võrguühenduse seadme ja pilveteenuse vahel.

Saate luua mitme konto on antud pilveteenuse pakkuja. Ajal salvestusruumi konto salvestatakse proovib teenus cloud teenusepakkuja suhelda. Mandaadi ja juurdepääsu materjali, mida olete sisestanud on kinnitatud sel ajal. Salvestusruumi konto on loodud ainult siis, kui autentimine õnnestus. Kui autentimine nurjub, siis vastav tõrketeade kuvatakse.

Ressursihaldur salvestusruumi kontod Azure portaali loodud on toetatud ka StorSimple. Ressursihaldur salvestusruumi kontod ei kuvata valikut ainult talletamist Azure klassikaline portaali loodud kuvatakse ripploendis. Ressursihaldur salvestusruumi kontod tuleb lisada salvestusruumi konto nagu allpool kirjeldatud protseduuri abil.

Protseduur Azure klassikaline salvestusruumi konto lisamisel on allpool.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Salvestusruumi konto redigeerimine

Saate redigeerida salvestusruumi konto, seade kasutab. Kui redigeerite salvestusruumi konto, mis on praegu kasutusel, muuta saadaolevad väljad on juurdepääsu võti ja SSL režiimi salvestusruumi konto. Lisage uus salvestusruumi kiirklahv või **lubada SSL režiimi** valiku muutmine ja värskendatud sätted salvestada.

#### <a name="to-edit-a-storage-account"></a>Salvestusruumi konto redigeerimiseks

1. Klõpsake teenuse sihtleht, valige teenust, topeltklõpsake teenuse nimi ja klõpsake nuppu **Konfigureeri**.

2. Valige **Kontod salvestusruumi lisamine või redigeerimine**.

3. Dialoogiboksis **Lisa/redigeeri salvestusruumi kontod** järgmist.

  1. **Salvestusruumi**kontode ripploendis valige olemasolev konto, mida soovite muuta. 
  2. Vajaduse korral saate muuta valik **Luba SSL-i režiim** .
  3. Kui soovite, et taastada salvestusruumi konto kiirklahvide. Lisateabe saamiseks lugege teemat [taastada salvestusruumi konto võtmed](storage-create-storage-account.md#manage-your-storage-access-keys). Lisage uus salvestusruumi konto võti. Azure'i salvestusruumi konto jaoks on esmane kiirklahv. 
  4. Klõpsake ikooni Otsi ![kontrollida ikooni](./media/storsimple-ova-manage-storage-accounts/checkicon.png) sätete salvestamiseks. Sätete värskendatakse lehe **konfigureerimine** . 
  5. Klõpsake lehe allosas nuppu **Salvesta** värskendatud sätete salvestamiseks. 

     ![Salvestusruumi konto redigeerimine](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Salvestusruumi konto kustutamine

> [AZURE.IMPORTANT] Salvestusruumi konto saate kustutada ainult juhul, kui seda ei kasuta. Kui salvestusruumi konto on kasutusel, teavitatakse teid.

#### <a name="to-delete-a-storage-account"></a>Salvestusruumi konto kustutamine

1. Klõpsake StorSimple halduri teenuse sihtleht, valige teenust, topeltklõpsake teenuse nimi ja klõpsake nuppu **Konfigureeri**.

2. Tabeli loendit salvestusruumi kontod, viige kursor konto, mille soovite kustutada.

3. Salvestusruumi konto jaoks äärmiselt paremas veerus kuvatakse ikooni Kustuta (**x**). Mandaadi kustutamiseks ikooni **x** .

4. Kinnituse küsimisel nuppu **Jah** kustutamise jätkamiseks. Muudatuste värskendatakse tabeli kirjet.

5. Klõpsake lehe allosas nuppu **Salvesta** värskendatud sätete salvestamiseks.


## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [hallata oma StorSimple virtuaalse massiiv](storsimple-ova-web-ui-admin.md).
