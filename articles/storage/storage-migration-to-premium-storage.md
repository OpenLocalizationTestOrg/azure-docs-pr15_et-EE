<properties
    pageTitle="Migreerimine Azure Premium mälu | Microsoft Azure'i"
    description="Teie olemasoleva virtuaalmasinates Azure Premium Storage migreerida. Premium salvestusruumi pakub suure jõudlusega, latentsusajaga ketta I/O-nõudvaid töökoormus töötab Azure'i Virtuaalmasinates tugi."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Azure Premium mälu migreerimine

## <a name="overview"></a>Ülevaade

Azure Premium mälu pakub suure jõudlusega, latentsusajaga ketta tugi virtuaalmasinates töötab I/O-nõudvaid töökoormus. Saate ära kiiruse ja jõudluse ketast, migreerimine rakenduse VM ketast Azure Premium mälu.

Selle juhendi eesmärk on uute kasutajate Azure Premium Storage paremini valmistuda tagavad sujuva ülemineku oma praeguse süsteemist Premium salvestusruumi aidata. Juhend aadressid kolme põhikomponentide selle protsessi: 

  - [Migreerimise Premium mäluruumi kavandamine](#plan-the-migration-to-premium-storage)
  - [Ettevalmistamine ja Premium salvestusruumi virtuaalse kõvaketta (VHDs) kopeerimine](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Azure virtuaalse masina Premium salvestusruumi loomine](#create-azure-virtual-machine-using-premium-storage)

Saate migreerida VMs muude platvormide jaoks loodud Azure Premium Storage või olemasoleva Azure VMs kindlad migreerimine Premium mälu. Käesolev juhend hõlmab nii kahte järgmist stsenaariumi juhised. Sõltuvalt sellest, milline olukord teie jaoks oluline jaotises määratud juhised.

>[AZURE.NOTE] Leiate funktsiooni ülevaade ja hinnakirjad Premium salvestusruumi Premium laos: [Suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](storage-premium-storage.md). Soovitame kõik virtuaalse masina ketta nõudva kõrge IOPS Azure Premium mäluruumi parima jõudluse rakenduse migreerimine. Kui teie kõvakettal ei nõua kõrge IOPS, saate piirata kulude säilitades see Standard hoidlas, mis salvestab kõvaketta draivid (HDD-d) asemel SSD virtuaalse masina ketta andmed.

Migreerimise protsessi tervikuna nõuda lisatoiminguid enne ja pärast selle juhendi toodud juhiseid. Näiteks konfigureerimise virtuaalse võrgu või lõpp-punktid või rakenduses ise, mis võivad nõuda mõned tööseisakute oma rakenduse koodi muudatuste tegemist. Neid toiminguid on kordumatu iga ja need tuleb täita koos teha täielik üleminek Premium salvestusruumi sama sujuv kui võimalik kasutada järgmist juhendit toodud juhiseid.


## <a name="plan-the-migration-to-premium-storage"></a>Migreerimise Premium mäluruumi kavandamine

Selles jaotises tagab on valmis migreerimise järgige selle artikli ja aitab teil teha parim otsus VM ja ketta tüüpi.

### <a name="prerequisites"></a>Eeltingimused
- Peate Azure tellimuse. Kui teil pole ühte, saate luua ühe kuu [tasuta prooviversiooni](https://azure.microsoft.com/pricing/free-trial/) tellimuse või külastage [Azure'i hinnad](https://azure.microsoft.com/pricing/) lisavõimaluste.
- PowerShelli cmdlet-käskude käivitamiseks peate Microsoft Azure'i PowerShelli mooduli. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimine punkti ja installimise juhised.
- Kui kavatsete kasutada Azure VMs töötab Premium salvestusruumi, peate kasutama Premium mälu toega VMs. Saate Standard- ja Premium salvestusruumi ketast Premium mälu toega VMs. Premium salvestusruumi ketast on saadaval rohkem VM andmetüüpidega tulevikus. Kõik saadaolevad Azure VM ketta tüüpi ja igas suuruses kohta leiate lisateavet teemast [virtuaalmasinates suurused](../virtual-machines/virtual-machines-windows-sizes.md) ja [suurused pilveteenustega](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Kaalutlused

Mõne Azure VM toetab manustamise mitu Premium salvestusruumi ketast, et rakenduste võib olla kuni 64 TB salvestusruumi VM. Premium salvestusruumi, mis rakenduste saavutada 80 000 IOPS (sisend toimingute sekundis) kohta VM ja 2000 MB teise ketta läbilaskevõimet kohta VM väga väike latentsused loetuks toimingute kohta. Teil on VMs ja ketast erinevaid võimalusi. Selles jaotises on aidata leida suvand, mis sobib kõige paremini teie töökoormus.

#### <a name="vm-sizes"></a>VM suurused
Azure'i VM suurus kirjeldused on loetletud [virtuaalmasinates suurused](../virtual-machines/virtual-machines-windows-sizes.md). Vaadake üle virtuaalmasinates, mis töötamine Premium salvestusruumi ja valige sobivaim VM suurus, mis sobib kõige paremini teie töökoormus parameetritele. Veenduge, et on piisavalt ribalaiust saadaval oma VM liiklust ketas.


#### <a name="disk-sizes"></a>Ketas suurused
On kolme tüüpi ketast, mida saab kasutada koos oma VM ja iga on teatud IOPs ja läbilaskevõime piirangud. Arvestama need piirangud ketta tüübi jaoks oma VM valik vastavalt vajadustele rakenduse võimsus, jõudlus ja skaleeritavus seisukohast ja laadib tippväärtus.

|Premium salvestusruumi ketta tüüp|P10|20|P30|
|:---:|:---:|:---:|:---:|
|Ketta suurus|128 GB|512 GB|1024 GB (1 TB)|
|IOPS ketta kohta.|500|2300|5000|
|Ühe ketta|100 MB sekundis|150 MB sekundis|200 MB sekundis|

Olenevalt teie töökoormus kindlaks teha, kui täiendavad andmed ketast on vaja oma VM. Mitme püsivate andmete ketast saate lisada oma VM. Vajadusel saate saate triip üle ketast, võimsuse ja jõudluse helitugevuse suurendamiseks. (Vt mis on ketas Striping [siin](storage-premium-storage-performance.md#disk-striping).) Kui te triip Premium mälu andmete ketast abil [Salvestusruum][4], peaksite konfigureerima seda iga ketta, mida kasutatakse ühe veeruga. Muul juhul üldise jõudluse musta Triibutatud maht võib olla väiksem kui oodatud tõttu ebaühtlane jaotumine liikluse üle soovitud ketast. Linux vms saate *mdadm* kasuliku samad saavutamiseks. Lugege artiklit [Konfigureerimine tarkvara RAID Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) üksikasju.

#### <a name="storage-account-scalability-targets"></a>Salvestusruumi konto skaleeritavus sihtkohad
Premium salvestusruumi kontodel on järgmised skaleeritavus eesmärgid Lisaks [Azure salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md). Kui teie rakenduse nõuded skaleeritavus sihtkohtade ühe salvestusruumi konto, koostada rakenduse kasutamiseks mitme salvestusruumi kontod ja partition nende salvestusruumi kontode vahel andmete.

|Kokku konto võimsus|Kogu läbilaskevõime kohalikult liigsete salvestusruumi konto jaoks|
|:--|:---|
|Plaadi maht: 35TB<br />Hetktõmmise võimsus: 10 TB|Enam kui 50 teostused sissetulev + väljaminev|

Premium mälu kohta lisateabe saamiseks vaadake [skaleeritavus ja jõudluse sihtkohtade Premium salvestusruumi kasutamise korral](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Ketta vahemällu poliitika
Vaikimisi ketta vahemällu poliitika on *Kirjutuskaitstud* kõiki Premium andmete ketast ja *Lugemis-ja kirjutamisõigusega* Premium operatsioonisüsteemi ketta külge VM. Selle konfiguratsiooni säte on soovitatav optimaalse jõudluse tagamiseks teie rakendus iOS-i saavutamiseks. Andmete kirjutamine-raske või kirjutage ainult ketast (nt SQL serveri logifailide), keelata ketta vahemällu, et teil oleksid rakenduse töökindluse. Olemasolevate andmete ketast vahemälu sätete saab värskendada [Azure portaali](https://portal.azure.com) või *Set-AzureDataDisk* cmdlet *- HostCaching* parameetri abil.

#### <a name="location"></a>Asukoht
Valige soovitud asukoht, kus Azure Premium mälu on saadaval. Asukoht saadaolevate asukohtade kohta saate ajakohast teavet leiate [Azure'i teenuste regiooniti](https://azure.microsoft.com/regions/#services) . VMs, mis asub samas piirkonnas salvestusruumi konto, et poed ketast VM annab palju töökindluse kui, kui need on omaette piirkondades.

#### <a name="other-azure-vm-configuration-settings"></a>Muud Azure VM konfiguratsiooni sätted
Azure'i VM-i loomisel küsitakse teilt teatud VM sätete konfigureerimine. Pidage meeles, et mõned sätted on fikseeritud VM, kestuse ajal saate muuta või lisada teistele hiljem. Vaadake neid Azure VM konfiguratsiooni sätteid ja veenduge, et need on konfigureeritud õigesti vastavad teie töökoormus.

### <a name="optimization"></a>Optimeerimine

[Azure Premium Storage: kujundus kõrge jõudluse](storage-premium-storage-performance.md) leiate juhised, suure jõudlusega rakenduste abil Azure Premium mälu. Jõudluse põhitõed kehtivad rakenduse tehnoloogiaid suuniste võite järgida.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Ettevalmistamine ja Premium salvestusruumi virtuaalse kõvaketta (VHDs) kopeerimine

Järgmisest jaotisest leiate juhised oma VM VHDs ettevalmistamine ja Azure Storage VHDs kopeerimine.

- [Stsenaarium 1: "ma olen migreerimine olemasoleva Azure VMs Azure Premium mälu."](#scenario1)
- [Stsenaarium 2: "ma olen migreerimine VMs teiste platvormide Azure Premium mälu."](#scenario2)

### <a name="prerequisites"></a>Eeltingimused

Funktsiooni VHDs migreerimiseks ettevalmistamiseks peate.

- Azure'i tellimuse, salvestusruumi konto ja selle salvestusruumi konto, millele saate kopeerida oma VHD on ümbrises. Pange tähele, et sihtkoha salvestusruumi konto saate Standard- või Premium salvestusruumi konto, sõltuvalt teie nõue.
- Tööriist üldistada on VHD, kui plaanite VM mitmes eksemplaris koostada. Näiteks sysprep Windowsi või virt-sysprep Ubuntu.
- Tööriist VHD faili üleslaadimine salvestusruumi kontole. Vt [AzCopy käsurea kasuliku andmete edastamiseks](storage-use-azcopy.md) või kasutada [Azure storage explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Sellest juhendist kirjeldatakse kopeerimise oma VHD AzCopy tööriista abil.

> [AZURE.NOTE] Kui valite sünkroonse kopeerida Kopeeri suvand optimaalse jõudluse tagamiseks AzCopy, kus teie VHD käivitades nende tööriistade Azure VM, mis on sama piirkonna sihtkoht salvestusruumi konto. Kui kopeerite on VHD on Azure VM teises regioonis, teie võib olla aeglasem.
>
> Piiratud läbilaskevõime üle kopeerimine suurt hulka andmeid, on soovitatav [Azure impordi/ekspordi teenuse bloobimälu andmete edastamiseks](storage-import-export-service.md); See võimaldab teil oma andmete edastamiseks saatmise kõvaketta draivid on Azure andmekeskusesse. Saate andmete kopeerimine kindlad konto ainult teenuse Azure impordi/ekspordi. Kui andmed on teie konto kindlad, saate selle [Koopia bloobimälu API](https://msdn.microsoft.com/library/azure/dd894037.aspx) või AzCopy kontole premium salvestusruumi andmete edastamiseks.
>
> Pange tähele, et Microsoft Azure'i toetab ainult kindlaksmääratud suurusega VHD failid. VHDX failid või dünaamiline VHDs ei toetata. Kui teil on dünaamiline VHD, saate selle teisendada kindlaksmääratud suurusega [Teisenda-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet-käsu abil.

### <a name="scenario1"></a>Stsenaarium 1: "ma olen migreerimine olemasoleva Azure VMs Azure Premium mälu."

Kui migreerite olemasoleva Azure VMs, peatada VM, ettevalmistamine VHDs VHD soovitud tüüpi kohta ning seejärel kopeerige VHD AzCopy või PowerShelli abil.

VM peab olema alla. migreerimiseks puhta oleku. Seal on soovitud tööseisakute kuni migreerimine on lõpule viidud.

#### <a name="step-1-prepare-vhds-for-migration"></a>Samm 1. VHDs migreerimiseks valmistumine

Kui migreerite olemasoleva Azure VMs Premium salvestusruumi, võib teie VHD:

- Generalized operatsioonisüsteemi pilt
- Kordumatu operatsioonisüsteemi kettal
- Andmete kettal

Allpool tutvustame need 3 stsenaariumid oma VHD ettevalmistamine.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Generalized operatsioonisüsteemi VHD abil saate luua VM mitmes eksemplaris

VHD, mitu üldise Azure VM eksemplari loomiseks kasutatud üleslaadimisel peab teil esmalt üldistada VHD abil sysprep kasuliku. See kehtib VHD, mis on kohapealse või pilveteenuses. Sysprep eemaldab selle VHD masina teabe.

>[AZURE.IMPORTANT] Hetktõmmis või varundada oma VM enne selle üldistamist. Töötava sysprep peatub ja deallocate VM eksemplari. Järgige allpool sysprep Windows OS VHD. Pange tähele, et käsu Sysprep nõuab teie arvuti virtuaalne sulgeda. Sysprep kohta leiate lisateavet teemast [Sysprep ülevaade](http://technet.microsoft.com/library/hh825209.aspx) või [Sysprepi tehniline teave](http://technet.microsoft.com/library/cc766049.aspx).

1. Avage käsuviiba aken administraatorina.
2. Sisestage järgmine käsk Sysprep avamiseks.

        %windir%\system32\sysprep\sysprep.exe

3. Süsteemi ettevalmistamise tööriista, valige süsteem sisestage Out Box Experience (OOBE), märkige ruut Generalize, valige **sulgemine**ja seejärel klõpsake nuppu **OK**, nagu on näidatud järgmisel pildil. Sysprep on operatsioonisüsteem üldistada ja süsteemi sulgeda.

    ![][1]

Mõne Ubuntu VM kasutada virt-sysprep saavutada sama. Vt lisateavet [virt-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) . Vt ka mõned avatud allika [Linux Server ettevalmistamise tarkvara](http://www.cyberciti.biz/tips/server-provisioning-software.html) opsüsteemide Linuxi jaoks.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Kordumatu operatsioonisüsteemi VHD abil saate luua VM ühekordsest

Kui teil on rakendus töötab VM, mis nõuab kohapeal konkreetsed andmed, mitte üldistada on VHD. Üldiste VHD saab luua kordumatu Azure VM eksemplari. Näiteks, kui teil on selle domeenikontrolleri oma VHD, sysprep käivitamisel teeb ebaefektiivne domeenikontrolleri nimega. Vaadake üle oma VM ja nende mõju töötab sysprep need enne selle VHD üldistamist töötavad rakendused.

##### <a name="register-data-disk-vhd"></a>Andmete kettale VHD registreerimine

Kui teil on andmete ketast Azure migreerida, peate veenduma, VMs, mida kasutada andmete ketast sulgema.

Kopeerige VHD Azure Premium Storage ja kasutajaks andmete ettevalmistatud kettal allpool kirjeldatud juhised.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Samm 2. Looge oma VHD sihtkoht

Looge konto salvestusruumi säilitada oma VHDs. Teie VHDs salvestuskoha kavandamisel võtke arvesse järgmist:

- Sihtrakenduse Premium salvestusruumi konto.
- Konto talletuskoht peab olema sama mis Premium mälu toega Azure VMs loomist viimasel etapil. Kopeerite võib uue salvestusruumi konto või lepingu kasutada sama salvestusruumi konto vastavalt oma vajadustele.
- Kopeerimine ja salvestamine järgmiseks etapiks salvestusruumi konto võti sihtkoha salvestusruumi konto.

Andmete ketast, saate valida mõne andmete ketta jätma kindlad konto (nt ketast, mis on jahedam salvestusruumi), kuid me soovitame teil teisaldada kõik andmed tootmise töökoormus kasutada premium mälu.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Samm 3. Kopeerige VHD AzCopy või PowerShelli abil

Peate leida oma container tee ja salvestusruumi konto võti töödelda kas need kaks võimalust. Container tee ja salvestusruumi konto võti leiate **Azure'i**portaalis > **salvestusruumi**. Container URL on näiteks "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Variant 1: Kopeerimine on VHD koos AzCopy (asünkroonne koopia)

AzCopy abil saate hõlpsalt laadida selle VHD Interneti kaudu. Sõltuvalt selle VHDs suurust, see võib võtta aega. Pidage meeles, kui, et konto sissepääsu/sealt salvestuslimiidi selle suvandi abil. Üksikasjad leiate [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md) .

1. Laadige alla ja installige AzCopy siit: [AzCopy uusim versioon](http://aka.ms/downloadazcopy)
2. Avage Azure'i PowerShelli ja valige kaust, kuhu on installitud AzCopy.
3. Järgmise käsu abil saate kopeerida VHD faili "Allikas" "Sihtkoht".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Näide:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Siin on kasutatud AzCopy käsk parameetrite kirjeldused.

 - * */Allikas: * &lt;allikas&gt;:*** salvestusruumi container URL, mis sisaldab soovitud VHD või kausta asukoht.
 - * */SourceKey: * &lt;allikas kontovõti&gt;:*** salvestusruumi konto võti allika salvestusruumi konto.
 - * */Dest: * &lt;sihtkoha&gt;:*** salvestusruumi container URL-i VHD soovite kopeerida.
 - * */DestKey: * &lt;dest kontovõti&gt;:*** salvestusruumi konto võti sihtkoha salvestusruumi konto.
 - * */Muster: * &lt;faili nimi&gt;:*** kopeerimiseks VHD faili nimi.

AzCopy tööriista kasutamise kohta leiate teemast [andmete AzCopy käsurea kasuliku edastamine](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Variant 2: Kopeerimine on VHD PowerShelliga (sünkroonitud eksemplar)

Samuti saate kopeerida VHD faili, kasutades PowerShelli cmdleti Start-AzureStorageBlobCopy. Azure'i PowerShelli järgmise käsu abil saate kopeerida VHD. <> Väärtuste asendamine lähte-kui ka salvestusruumi konto vastavad väärtused. Selle käsu kasutamiseks peab teil olema ümbris nimega vhds sihtkoha salvestusruumi konto. Kui ümbris pole olemas, looge see enne käsu.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Näide:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>Stsenaarium 2: "ma olen migreerimine VMs teiste platvormide Azure Premium mälu."

Kui migreerite VHD kaudu mitte - Azure'i salvestusruumi Azure, tuleb esmalt eksportida on VHD kohalikku kausta. On täielik tee kohalik kataloog VHD talletuskoht mugav ja üleslaadimine Azure Storage AzCopy abil.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Samm 1. Ekspordi VHD kohalik kataloog

##### <a name="copy-a-vhd-from-aws"></a>Kopeerige on VHD AWS

1. Kui kasutate AWS, eksportimine on VHD on Amazon S3 Ämber EC2 eksemplari. Amazon dokumentatsioonist eksportimise Amazon EC2 eksemplarid Amazon EC2 käsurea liides (CLI) tööriista installimiseks ja käivitage käsk Loo eksemplari ekspordi ülesande EC2 eksemplari VHD faili eksportida kirjeldatud juhised. Veenduge, et kasutada **VHD** ketas & #95; pilt & #95; Kui käsk **Loo-eksemplari-ekspordi-tööülesanne** muutuja vorming. VHD eksporditud fail on salvestatud, saate määrata selle käigus Amazon S3 Ämber.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. S3 kopp VHD faili alla laadida. Valige VHD fail, seejärel **toimingud** > **alla laadida**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Kopeerige on VHD muude-Azure'i pilveteenuses

Kui migreerite VHD kaudu mitte - Azure'i salvestusruumi Azure, tuleb esmalt eksportida on VHD kohalikku kausta. Kopeerige VHD talletamiseks kohalikku kataloogi täielik tee.

##### <a name="copy-a-vhd-from-on-premise"></a>Kopeerige on VHD kohapealne

Kui migreerite VHD asutusesiseses keskkonnas, peate VHD talletuskoht täielik tee. Allika tee võib olla server või failikettal.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Samm 2. Looge oma VHD sihtkoht

Looge konto salvestusruumi säilitada oma VHDs. Teie VHDs salvestuskoha kavandamisel võtke arvesse järgmist:

- Target salvestusruumi konto võib sõltuvalt teie rakenduse nõue standard- või premium salvestusruumi.
- Salvestusruumi konto piirkond peab olema sama mis Premium mälu toega Azure VMs loomist viimasel etapil. Kopeerite võib uue salvestusruumi konto või lepingu kasutada sama salvestusruumi konto vastavalt oma vajadustele.
- Kopeerimine ja salvestamine järgmiseks etapiks salvestusruumi konto võti sihtkoha salvestusruumi konto.

Soovitame teil teisaldada kõik andmed tootmise töökoormus kasutada premium mälu.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Samm 3. Azure'i salvestusruumi on VHD üleslaadimine

Nüüd, kui teil on oma VHD kohalikku kausta, saate AzCopy või AzurePowerShell Azure Storage .vhd faili üleslaadimiseks. Siin on esitatud mõlema variandi:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Variant 1: Azure'i PowerShelli lisa-AzureVhd abil .vhd faili üleslaadimine

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Näide <Uri> võib olla **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_**. Näide <FileInfo> võib olla **_"C:\path\to\upload.vhd"_**.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Variant 2: Abil AzCopy .vhd faili üles laadida

AzCopy abil saate hõlpsalt laadida selle VHD Interneti kaudu. Sõltuvalt selle VHDs suurust, see võib võtta aega. Pidage meeles, kui, et konto sissepääsu/sealt salvestuslimiidi selle suvandi abil. Üksikasjad leiate [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md) .

1. Laadige alla ja installige AzCopy siit: [AzCopy uusim versioon](http://aka.ms/downloadazcopy)
2. Avage Azure'i PowerShelli ja valige kaust, kuhu on installitud AzCopy.
3. Järgmise käsu abil saate kopeerida VHD faili "Allikas" "Sihtkoht".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Näide:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Siin on kasutatud AzCopy käsk parameetrite kirjeldused.

 - * */Allikas: * &lt;allikas&gt;:*** salvestusruumi container URL, mis sisaldab soovitud VHD või kausta asukoht.
 - * */SourceKey: * &lt;allikas kontovõti&gt;:*** salvestusruumi konto võti allika salvestusruumi konto.
 - * */Dest: * &lt;sihtkoha&gt;:*** salvestusruumi container URL-i VHD soovite kopeerida.
 - * */DestKey: * &lt;dest kontovõti&gt;:*** salvestusruumi konto võti sihtkoha salvestusruumi konto.
 - **/BlobType: leht:** Saate määrata, et sihtkoht on lehe bloobimälu.
 - * */Muster: * &lt;faili nimi&gt;:*** kopeerimiseks VHD faili nimi.

AzCopy tööriista kasutamise kohta leiate teemast [andmete AzCopy käsurea kasuliku edastamine](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Muud suvandid on VHD üleslaadimine

Saate üles laadida ka on VHD salvestusruumi kontole, kasutades ühte järgmistest viisidest:

- [Azure'i salvestusruumi Kopeeri bloobimälu API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure'i salvestusruumi Exploreris üles plekid](https://azurestorageexplorer.codeplex.com/)
- [Salvestusruumi impordi/ekspordi teenuse REST API viide](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Soovitame kasutada impordi/ekspordi teenuse, kui üleslaadimise aeg on pikem kui 7 päeva. Saate [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) prognoosimiseks suurus ja edastamine andmekogumiks aeg.
>
> Import/eksport saab kopeerida kindlad konto. Peate premium salvestusruumi kontot kasutades AzCopy kindlad kopeerida.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Azure VMs abil Premium salvestusruumi loomine

Pärast selle VHD on üles laaditud või soovitud salvestusruumi kontole kopeerida, järgige selle jaotise registreerida selle VHD OS pildi või OS ketta vastavalt stsenaariumile ja seejärel looge VM eksemplari see. Andmed kettale VHD võivad olla seotud VM pärast selle loomist. Selle jaotise lõpus on esitatud valimi migreerimise skripti. Selle lihtsa skripti ei vasta kõik stsenaariumi. Võib-olla peate värskendama skripti sobitada teie konkreetse stsenaariumi. Kui see skript kehtib stsenaariumist vaatamiseks allpool [A valimi migreerimise skripti](#a-sample-migration-script).

### <a name="checklist"></a>Kontroll-loend

1.  Oodake, kuni kõik VHD ketast kopeerimine on lõpule viidud.
2.  Veenduge, et Premium mälu on saadaval ala, kui soovite migreerimist.
3.  Otsustage, kasutate uue VM sarja. Peaks olema Premium Storage toega ja suurus peaks olema sõltuvalt kättesaadavus piirkonna ja vastavalt oma vajadustele.
4.  Otsustage, kasutage täpne VM suurus. VM maht peab olema andmete ketast teil arvu jaoks piisavalt suur. Nt Kui teil on 4 andmete ketast, VM peab olema 2 või rohkem tuuma. Pidage silmas ka töötlemise võimsus, mälu ja võrgu läbilaskevõime peab olema.
5.  Looge konto Premium salvestusruumi piirkonna sihtkoht. See on uus VM kasutate konto.
6.  On mugav, sh ketast ja vastavate VHD plekid loendit praeguse VM andmed.

Rakenduse ette valmistada. Puhasta migreerimise tegemiseks tuleb kõik töötlemise lõpetamine praeguse süsteemi. Alles siis saate seda ühtsete olekusse, mis saate migreerida platvormi. Tööseisakute kestus sõltub andmehulga ketta migreerida.

>[AZURE.NOTE] Kui loote mõnda Azure ressursihaldur VM eriotstarbelisi VHD kettal, vaadake [selle malli](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) abil olemasolevat ketast ressursihaldur VM kasutamise kohta.

### <a name="register-your-vhd"></a>Teie VHD registreerimine

OS VHD VM loomiseks või uue VM andmed kettale manustamiseks peate registreeruma neid. Järgige juhiseid all sõltuvalt teie VHD stsenaariumi.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Üldiste operatsioonisüsteemi VHD Azure VM mitu eksemplari loomine

Pärast üldise OS pildi VHD on üles laaditud salvestusruumi kontole, registreerida **Azure'i VM pilt** nii, et saate luua ühe või mitme VM eksemplari seda. Kasutage järgmist PowerShelli cmdlet-käskude registreerida oma VHD Azure'i VM OS pildina. Sisestage täieliku container URL, kuhu soovite kopeeritud VHD.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Kopeerida ja salvestada selle uue Azure VM pildi nimi. Ülaltoodud näites on *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Kordumatu operatsioonisüsteemi VHD Azure VM ühekordsest loomiseks

Pärast kordumatu OS VHD on üles laaditud salvestusruumi kontole, registreerida on **Azure OS kettale** nii, et see saab luua VM eksemplari. Need PowerShelli cmdlet-käskude abil registreerida oma VHD on Azure OS ketas. Sisestage täieliku container URL, kuhu soovite kopeeritud VHD.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Kopeerige ja salvestage see uue Azure OS ketta nime. Ülaltoodud näites on *tavalise nimetusega OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Andmete kettale VHD lisatakse uus Azure VM loendisInstance

Pärast andmete kettale VHD on üles laaditud salvestusruumi kontole, registreerida mõne Azure'i andmed ketas, et see saab lisada uue DS andmesarjas, DSv2 sarja või GS sarja Azure VM eksemplari.

Need PowerShelli cmdlet-käskude abil registreerida oma VHD on Azure andmete ketas. Sisestage täieliku container URL, kuhu soovite kopeeritud VHD.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Kopeerige ja salvestage see uue Azure'i andmed kettale nime. Ülaltoodud näites on *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Luua ühenduse võimalusega VM Premium salvestusruum

Kui OS pilt või OS kui teil on juba registreeritud, saate luua uue DS-sarja, DSv2-sarja või GS-sarja VM. Kasutate opsüsteemi pilti või operatsioonisüsteemi ketta nimi registreeritud. Valige VM tüüp Premium talletusmahu taseme. Alltoodud näites me kasutame *Standard_DS2* VM suurus.

>[AZURE.NOTE] Värskendage ketta suurus veendumaks, et see vastab teie nõuded ja ja Azure vaba suurused.

Järgige juhiseid, et luua uus VM samm-sammult PowerShelli cmdletid. Esmalt määrata levinud parameetrid.

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Esmalt looge mõnda pilveteenusesse, kus on teie hosting oma uue VMs.

    New-AzureService -ServiceName $serviceName -Location $location

Järgmiseks vastavalt stsenaariumile, luua Azure VM eksemplari OS pilt või OS, kuhu olete registreerunud.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Üldiste operatsioonisüsteemi VHD Azure VM mitu eksemplari loomine

Saate luua ühe või mitme uue DS sarja Azure'i VM juhtudel kasutades **Azure OS pilt** registreeritud. Määrake OS pildi nimi VM konfiguratsiooni, kui loote uue VM, nagu allpool näidatud.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Kordumatu operatsioonisüsteemi VHD Azure VM ühekordsest loomiseks

Saate luua uue DS sarja Azure VM eksemplari **Azure'i OS ketta** registreeritud. Määrake OS ketta nimi VM konfiguratsiooni, kui loote uue VM, nagu allpool näidatud.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Muud Azure VM teabe, nt pilveteenuses, piirkond, salvestusruumi konto, kättesaadavus määramine ja vahemällu poliitika määramine. Pange tähele, et VM eksemplari peab olema seotud operatsioonisüsteemi koostööd asub või andmete ketast, et valitud cloud teenus, piirkond ja salvestusruumi konto tuleb kõik samas kohas nende ketast aluseks VHDs.

### <a name="attach-data-disk"></a>Andmete kettale manustamine

Lõpuks, kui olete registreerunud andmed kettale VHDs, lisab selle uue Premium mälu toega Azure VM.

Abil jälgima PowerShelli cmdlet-käsu lisada uue VM andmed kettale ja vahemällu poliitika määramine. Järgmises näites vahemällu poliitika on seatud *kirjutuskaitstud*.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Võib olla lisatoimingud korral, mis on rakenduse toetamiseks ei kuulu juhendi.

### <a name="checking-and-plan-backup"></a>Kontrollimine ja varundamise kavandamine

Pärast uue VM on tööks, juurde, kasutades sama sisselogimise id ja parool on algse VM ja veenduge, et kõik töötab ootuspäraselt. Kõik sätted, sh musta Triibutatud mahud, oleks Esita uue VM.

Viimases etapis on kavandada varundus ja hooldus ajakava uue VM põhjal rakenduse vajadustele.

### <a name="a-sample-migration-script"></a>Valimi migreerimise skripti

Kui teil on mitu VMs migreerida, automatiseerimise PowerShelli skripti kaudu on hea. Järgmine on näide skripti, mis automatiseerib VM migreerimise. Märkus all script on ainult näide ja on mõned eelduste praeguse VM ketast kohta. Võib-olla peate värskendama skripti sobitada teie konkreetse stsenaariumi.

Eelduste on:

- Loote klassikaline Azure VMs.
- Oma allika OS ketast ja allika andmete ketast on sama salvestusruumi konto ja sama ümbrises. Kui teie OS ketast ja andmete ketast pole samas kohas, saate kasutada AzCopy või Azure PowerShelli kopeerida VHDs salvestusruumi kontod ja ümbriste üle. Eelmises etapis viidata: [Kopeeri VHD AzCopy või PowerShelli abil](#copy-vhd-with-azcopy-or-powershell). Selle skripti kohta, mis vastavad teie stsenaariumi redigeerimine on teine valik, kuid soovitame kasutada AzCopy või PowerShelli, kuna see on lihtsam ja kiirem.

Allpool on esitatud skripti automatiseerimine. Teksti asendamine oma andmeid ja värskendage kindla stsenaariumist sobitada skripti.

>[AZURE.NOTE] Olemasoleva skripti abil allikas VM võrgukonfiguratsioon ei säilitata. Peate re-config võrgu sätete kohta oma migreeritud VMs.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optimeerimine

Teie praegune VM konfiguratsiooni võib kohandada spetsiaalselt töötada ka standardse ketast. Näiteks jõudluse, kasutades palju ketast musta Triibutatud helitugevuse suurendamiseks. Näiteks asemel 4 ketast eraldi Premium Storage võimalik optimeerida maksumus ühe ketta lastes. Optimeerimised nagu seda vaja vaadeldav juhtumi eraldi ja nõuavad kohandatud toimingud pärast migreerimist. Samuti võtke arvesse, et seda toimingut ei pruugi hästi töötada andmebaasid ja rakendused, mis sõltuvad ketas paigutus, mis on määratletud häälestamine.

##### <a name="preparation"></a>Ettevalmistamine

1.  Täitke lihtsa migreerimise varasemates jaotises kirjeldatud. Optimeerimised tehakse uue VM pärast migreerimist.
2.  Määratlege uue ketta suurused jaoks optimeeritud konfigureerimine vajalik.
3.  Määratlege kaardistamine praeguse ketast/mahud uue ketta tehnilised andmed.

##### <a name="execution-steps"></a>Täitmise juhised

1.  Looge uus ketast õige suurusega Premium salvestusruumi VM.
2.  Logi VM ja kopeerige andmed praeguse helitugevuse kaartide selle uue kettale. Tehke seda praeguse maht, mida tuleb vastendada uue ketta.
3.  Järgmiseks rakenduse aktiveerimiseks uue ketast sätete muutmine ja vana mahud eemaldada.

Taotlus parema jõudluse häälestamiseks, vaadake [Optimeerimine rakenduste jõudlust](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Rakenduse migratsioon

Andmebaasid ja muud keerukate rakendused võivad nõuda teisiti juhiseid rakenduse pakkuja migreerimise jaoks määratletud. Vaadake vastava rakenduse dokumentatsioonist. Nt tavaliselt andmebaaside migreeritud kaudu varundamine ja taastamine.

## <a name="next-steps"></a>Järgmised sammud

Leiate järgmistest ressurssidest jaoks teatud stsenaariumide migreerimine virtuaalse masinad:

- [Migreerimine Azure'i Virtuaalmasinates salvestusruumi kontode vahel](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Luua ja üles laadida Windows Server VHD Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Luua ja üles laadida virtuaalse kõvaketta, mis sisaldab Linux operatsioonisüsteem](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Migreerimine Virtuaalmasinates Amazon AWS Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Vaadake Azure Storage ja Azure'i Virtuaalmasinates lisateavet järgmistest teemadest:

- [Azure'i salvestusruum](https://azure.microsoft.com/documentation/services/storage/)
- [Azure'i Virtuaalmasinates](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Premium mälu: Suure jõudlusega Azure virtuaalse masina töökoormus salvestusruum](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
