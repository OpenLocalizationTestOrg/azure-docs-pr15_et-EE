<properties
  pageTitle="Azure'i ja Linux VM salvestusruumi | Microsoft Azure'i"
  description="Kirjeldab Linuxi Azure'i Standard ja Premium salvestusruumi."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure'i ja Linux VM salvestusruum

Azure'i salvestusruumi on pilveteenuse salvestusruumi lahendus tänapäevased rakendused kestvus, kättesaadavus ja nende klientide vajaduste rahuldamiseks skaleeritavus.  Lisaks võimaldab arendajatel luua suuremahuliste rakendusi toetamiseks uue stsenaariumid, Azure Storage pakub salvestusruumi foundation Azure'i Virtuaalmasinates.

## <a name="azure-storage-standard-and-premium"></a>Azure'i salvestusruumi: Standard- ja Premium

Azure'i VM ehitatud kindlad ketast või premium salvestusruumi ketast.  Portaali abil saate valida oma VM lülitada rippmenüüst standard- ja premium ketast kuvamiseks ekraanil põhitõed.  Pildil tõstab esile, et Lülita menüü.  Kui SSD, ainult premium salvestusruumi lubatud VMs näidatakse, et lülitada kõik toetavad SSD draivid.  Kui lülitada HDD, kindlad lubatud varundatud valmistamata kettadraivide näidatakse, koos premium mälu VMs toetavad SSD VMs.

  ![screen1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

VM kaudu loomisel on `azure-cli` saate valida standard- ja premium VM suurus kaudu valides soovitud `-z` või `--vm-size` cli lipp.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Luua VM standard Storage VM cli kohta

Lipu cli `-z` valib Standard_A1 A1 on standardne salvestusruumi vastavalt Linux VM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Luua VM premium Storage cli kohta

Lipu cli `-z` valib Standard_DS1 DS1 premium mälu on vastavalt Linux VM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Kindlad

Azure'i Standard salvestusruumi on vaikimisi tüübi talletusruumi.  Samas on kiire on tõhus kindlad.  

## <a name="premium-storage"></a>Premium salvestusruum

Azure Premium mälu pakub suure jõudlusega, latentsusajaga ketta tugi virtuaalmasinates töötab I/O-nõudvaid töökoormus. Virtuaalse masina (VM) ketast, Premium mälu kasutavate salvestada andmed välkdraivid (SSD). Saate rakenduse VM ketast Azure Premium Storage ära kiiruse ja jõudluse ketast migreerida.

Salvestusruumi lisafunktsioonidele:

- Premium salvestusruumi ketast: Azure Premium mälu toetab VM ketast, mis võivad olla seotud DS, DSv2 või GS sarja Azure VMs.

- Premium lehe BLOOBI: Premium mälu toetab Azure lehe plekid, mida kasutatakse hoidke püsivate ketast jaoks Azure'i Virtuaalmasinates (VMs).

- Premium kohalikult liigsete säilitamine: Premium salvestusruumi konto ainult toetab kohalikult liigsete salvestusruumi (LRS) suvand kopeerimine ja hoiab kolm eksemplari andmeid ühest.

- [Premium salvestusruum](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Premium salvestusruumi toetatud VMs

Premium mälu toetab DS-sarja kuuluv DSv2-sarja kuuluv GS-sari ja Fs-sarja Azure'i Virtuaalmasinates (VMs). Saate Standard- ja Premium salvestusruumi ketast Premium salvestusruumi vms toetatud. Kuid te ei saa kasutada Premium salvestusruumi ketast VM sari, mis ei ole Premium salvestusruumi ühilduvad.

Järgnevalt on Linuxi mida me kinnitada Premium Storage.

| Jaotuse. | Versioon                 | Toetatud tuum    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| CentOS       | 6.5, 6.6, 6,7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Failide talletamine

Azure'i failisalvestusruumi pakub failikettad standard SMB protokolli abil pilveteenuses. Azure'i faile, saate migreerida Azure serverites ettevõtte rakendused. Helistamine ja Azure saate hõlpsalt ühendage failikettad: Azure'i virtuaalmasinates töötab Linux. Ja koos failide talletamine uusimat versiooni, saate ka ühendate failikettale kohapealse rakendusest, mis toetab SMB 3.0.  Kuna failikettad on SMB ühtlasi, pääsete neile standard failisüsteemi API-de kaudu.

Failide talletamine põhineb samal tehnoloogial bloobimälu, tabeli ja järjekorda salvestusruumi, et failide talletamine pakub kättesaadavus, kestvus ja skaleeritavus geo koondamise salvestusruumi Azure'i platvormi sisse ehitatud. Faili salvestusruumi tulemuslikkuse eesmärkide ja piirangute kohta lisateavet Azure salvestusruumi skaleeritavus ja jõudluse sihtkohad.

- [Kuidas kasutada Linux Azure'i failide talletamine](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Saadaolevad salvestusruum

Azure'i kuum talletusmahu taseme on optimeeritud andmeid, mis on sageli talletamiseks.  Saadaolevad salvestusruumi on vaikimisi salvestusruumi tüüp jaoks bloobimälu poed.

## <a name="cool-storage"></a>Lahedad salvestusruum

Azure'i lahedad talletusmahu taseme on optimeeritud andmeid, mis on harva juurde ja vanade talletamiseks. Näide kasutamise juhtudel lahedad Storage kaasata varukoopiate, meediumisisu, teaduslik andmed, nõuetele vastavus ja arhiivimine andmed. Üldiselt on kõik andmed, mis on harva täiuslik testversiooni lahedad Storage.

|                             | Kuum talletusmahu taseme      | Lahedad talletusmahu taseme     |
|:----------------------------|:---------------------:|:---------------------:|
| Kättesaadavus                | 99,9%                 | 99%                   |
| Kättesaadavus (RA-GRS loeb) | 99,99%                | 99,9%                 |
| Kulude               | Suuremad salvestusruumi kulud  | Lower salvestusruumi kulud   |
|                             | Lower juurdepääs          | Suurema juurdepääsu         |
|                             | ja tehingu kulud | ja tehingu kulud |


## <a name="redundancy"></a>Koondamine

Microsoft Azure salvestusruumi konto andmed on alati kopeeritud tagada kestvus ja kõrge kättesaadavus, koosoleku Azure'i salvestusruumi SLA isegi ees siirdamiseks riistvara tõrkeid.

Salvestusruumi konto loomisel valige üks järgmistest Tiražeerimissuvandid:

- Kohalik liigsete salvestusruumi (LRS)
- Tsooni liigsete mälu (ZRS)
- Geograafilise liigne salvestusruumi (GRS)
- Lugemisõigus – geograafilise liigne salvestusruumi (RA GRS)

### <a name="locally-redundant-storage"></a>Kohalik liigsete salvestusruum

Kohalik liigsete salvestusruumi (LRS) tiražeerib andmete piirkonnas, kus teie konto salvestusruumi. Kestvus maksimeerimiseks iga taotluse vastu teie salvestusruumi andmeid korratakse kolm korda. Need kolm koopiad elavad eraldi viga domeenid ja täiendamine domeenide.  Taotluse tagastab edukalt ainult siis, kui see on kirjutatud kõigi kolme koopiate.

### <a name="zone-redundant-storage"></a>Tsooni liigsete salvestusruum

Tsooni liigsete mälu (ZRS) tiražeerib andmete üle 2-3 pakkuvad, kas ühest või mitmest mõlema piirkonna, kui LRS kõrgema kestvus. Kui teie konto salvestusruumi on lubatud ZRS, siis teie andmed on püsival isegi korral ühel viisil.

### <a name="geo-redundant-storage"></a>Geograafilise liigne salvestusruum

Geograafilise liigne salvestusruumi (GRS) tiražeerib andmete teisese alale, mis on sadu miili eemale esmane piirkond. Kui konto salvestusruumi on lubatud GRS, siis teie andmed on püsival isegi korral täieliku piirkondliku katkestuste või mõne tõttu, esmane piirkond ei ole taastatav.

### <a name="read-access-geo-redundant-storage"></a>Lugemisõigus – geograafilise liigne salvestusruum

Lugemisõigus – geograafilise liigne salvestusruumi (RA GRS) maksimeerib kättesaadavus salvestusruumi konto teisene asukoht, lisaks dispersioonanalüüs üle kahe piirkonna GRS esitatud andmed kirjutuskaitstud juurdepääsu võimaldamine. Juhuks, kui andmete muutub esmane regioonis saadaval, saate rakenduse andmeid lugeda, teisene piirkonnast.

Deep Dive Azure storage koondamise vt:

- [Azure'i salvestusruumi dispersioonanalüüs](../storage/storage-redundancy.md)

## <a name="scalability"></a>Skaleeritavus.

Azure'i salvestusruumi on oluliselt scalable, nii et saate talletada ja teadus-, analüüsi ja meedia rakendused nõutud TB andmete toetamiseks suur andmete stsenaariumid protsessi sadu. Või saate talletada vähehaaval andmekogumite väikeettevõtete veebisaidi. Kuhu kuuluvad teie vajadustele, maksate ainult on talletatud andmed. Azure'i salvestusruumi praegu salvestab kümneid triljoneid kordumatu kliendi objektid ja miljonid taotlusi sekundis keskmiselt tegeleb.

Kindlad kontode puhul: standard salvestusruumi konto on kokku päringu määr 20 000 IOPS. Kokku IOPS kõigis oma virtuaalse masina ketta jaotise standard salvestusruumi konto ei tohi olla suurem kui selle piirmäära.

Premium salvestusruumi kontode puhul: premium salvestusruumi konto on maksimaalne kokku läbilaskevõime määr 50 Gbit. Kokku läbilaskevõime kõigis oma VM ketast ei tohi olla suurem kui selle piirmäära.

## <a name="availability"></a>Kättesaadavus

Me tagada, et vähemalt 99,99% (99,9% lahedad Accessi kiht) aeg, meil on edukalt taotlused andmeid lugeda lugemisõigus – Geo liigsete salvestusruumi (RA GRS) kontodelt, tingimusel, et katsete lugeda, andmeid esmane piirkond on uuesti proovida teisene piirkonna kohta.

Me tagada, et vähemalt 99,9% (99% lahedad Accessi kiht) aeg, me edukalt töötleb taotlused kohalikult liigsete salvestusruumi (LRS), Zone liigsete mälu (ZRS) ja Geo liigsete salvestusruumi (GRS) kontode andmeid lugeda.

Me tagada, et vähemalt 99,9% (99% lahedad Accessi kiht) aeg, me on edukalt taotlused kirjutada andmete kohalikult liigsete salvestusruumi (LRS), Zone liigsete mälu (ZRS), ja Geo liigsete salvestusruumi (GRS) kontod ja lugemisõigus – Geo liigsete salvestusruumi (RA GRS) kontod.

- [Azure'i SLA Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Piirkondade

Azure'i on üldiselt saadaval kogu maailmas 30 piirkondade ja on teada 4 täiendavad piirkondade lepingud. Geograafilised laiendamine on Azure prioriteet, kuna see lubab klientidele suurema jõudluse saavutamiseks ja selle tugi nende nõuete ja eelistustes andmete asukoht.  Azures uusim piirkond käivitamiseks on Saksamaa.

Microsoft Cloud Saksamaa pakub liigendatud suvand Microsoft Cloud Services juba saadaval üle Euroopa loomise võimalused uuenduste ja economic growth reguleeritud partnerid ja kliendid Saksamaa, Euroopa Liidu (EL) ja Euroopa Assotsiatsiooni (EFTA).

Kliendiandmete need uue andmekeskuste Magdeburg ja Frankfurt, hallatakse andmete haldur, T – Systems rahvusvahelise, iseseisev Saksa ettevõte ja esinduse, Prisa kontrolli all. Trükikojas Microsofti pilveteenustega nende andmekeskuste kinni Saksa andmete töötlemise määruste ja andke klientidele, kuidas ja kus andmeid töödeldakse Lisavalikud.


- [Azure'i piirkondade kaart](https://azure.microsoft.com/regions/)

## <a name="security"></a>Turvalisus

Azure'i salvestusruumi pakub turvalisus funktsioonid, mis koos võimaldab arendajatel luua turvalist rakendusi täielik kogum. Rollipõhine juurdepääsu reguleerimine ja Azure Active Directory abil saate ise salvestusruumi konto kinnitada. Andmeid saab turvatud vahelise rakendus ja Azure kliendipoolne krüptimist, HTTPS või SMB 3.0 abil. Andmeid saate määrata automaatselt krüptimist kui kirjutatud Azure Storage salvestusruumi teenuse krüptimise (SSE) abil. Saate seada OS- ja ketast, mis kasutavad virtuaalmasinates olema krüptitud Azure'i ketas. Delegeeritud juurdepääsu andmeobjektid Azure Storage saab anda ühiskasutusse Accessi allkirjade abil.

### <a name="management-plane-security"></a>Juhtimise lennuk turvalisus

Juhtimise lennuk koosneb ressursside abil saate hallata oma salvestusruumi konto. Selles jaotises me rääkida Azure'i ressursihaldur juurutamise mudeli ja Rollipõhine juurdepääsu reguleerimine (RBAC) abil juurdepääsu oma salvestusruumi kontod. Samuti räägitakse hallata oma salvestusruumi konto võtmed ja kuidas neid uuesti looma.

### <a name="data-plane-security"></a>Andmete lennuk turvalisus

Selles jaotises vaatame jäetud tegelik andmetele juurdepääsu konto salvestusruumi plekid, faile, järjekorrad ja tabelid, nt kaudu ühiskasutusse antud juurdepääs signatuurid ja talletatud Juurdepääsupoliitikaid. Me hõlmab nii teenusetaseme SAS ja konto taseme SAS. Samuti vaatame, kuidas piirata juurdepääsu teatud IP-aadress (või vahemik, IP-aadressid), kuidas piirata protokoll HTTPS-i kasutada ja ootamata seda aegumise ühiskasutusse Accessi signatuuri tühistamine.

## <a name="encryption-in-transit"></a>Krüptimine

Selles jaotises kirjeldatakse, kuidas kaitsta andmeid, kui teisaldate või sealt välja Azure Storage. Me rääkida HTTPS ja krüptimine kasutab SMB 3.0 Azure'i Failikettad soovitatav kasutamise kohta. Võtame kliendipoolne krüptimist, mis võimaldab teil enne, kui see on üle salvestusruumi lisamine klientrakenduse andmete krüptimiseks ja andmeid dekrüptida, kui see on salvestusruum kokku üle vaadata.

## <a name="encryption-at-rest"></a>Ülejäänud krüptimist

Me rääkida salvestusruumi teenuse krüptimise (SSE) ja kuidas saate salvestusruumi konto lubamine, tulemuseks Blokeeri plekid, lehe plekid, ja lisab automaatselt on krüptitud kui kirjutatud Azure Storage plekid. Ka vaatame, kuidas saate Azure'i ketta krüptimise ja uurige põhilised erinevused ja ketta krüptimise võrreldes SSE võrreldes kliendipoolne krüptimise juhtudel. Me vaatame korraks FIPS vastavuse USA valitsuse arvutites.

- [Azure'i salvestusruumi turvalisuse juhend](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Kulude kokkuhoid

- [Salvestusruumi maksumus](https://azure.microsoft.com/pricing/details/storage/)

- [Salvestusruumi kulude kalkulaator](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Salvestuslimiidid

- [Salvestuslimiitide teenus](../azure-subscription-service-limits.md#storage-limits)
