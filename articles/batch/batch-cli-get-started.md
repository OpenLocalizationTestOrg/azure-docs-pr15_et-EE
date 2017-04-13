<properties
   pageTitle="Azure'i paketi CLI alustamine | Microsoft Azure'i"
   description="Saada paketi käsud tutvustuse Azure'i CLI haldamise Azure'i paketi teenuste ressursid"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Azure'i paketi CLI kasutamise alustamine

Mitu platvormi Azure'i käsurea liides (Azure'i CLI) võimaldab teil paketi kontod ja kaustu, töö ja Linux, Maci ja Windowsi käsk kestad ülesannete haldamiseks. Azure'i CLI, saate teha ja skript paljude paketi API-d, Azure portaali ja paketi PowerShelli cmdlet-käskude teostada samu toiminguid.

Selles artiklis on Azure CLI versioon 0.10.5 alusel.

## <a name="prerequisites"></a>Eeltingimused

* [Installige Azure'i CLI](../xplat-cli-install.md)

* [Azure'i CLI ühenduse Azure tellimuse](../xplat-cli-connect.md)

* Aktiveerige **ressursihaldur režiim**.`azure config mode arm`

>[AZURE.TIP] Soovitame, et te Azure'i CLI installi sageli, et teenuse värskendused ja täiustatud ära Värskenda.

## <a name="command-help"></a>Käsu spikker

Saate kuvada iga käsu spikriteksti Azure'i CLI mõnel `-h` ainult suvand pärast käsu. Näiteks:

* Spikri funktsiooni `azure` käsk, sisestage:`azure -h`
* CLI paketi kõik käsud loendi saamiseks kasutage:`azure batch -h`
* Sisestage paketi konto loomise kohta abi saamiseks.`azure batch account create -h`

Kahtluse korral kasutada funktsiooni `-h` käsurea suvandit Azure'i CLI käsk kohta abi saamiseks.

## <a name="create-a-batch-account"></a>Paketi konto loomine

Kasutamine:

    azure batch account create [options] <name>

Näide:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Loob uue paketi konto määratud parameetrid. Määrake vähemalt asukoht, ressursirühm ja konto nimi. Kui teil pole veel ressursirühma, luua käivitades `azure group create`, ja määrake Azure piirkondadesse (nt "Lääne meie") jaoks soovitud `--location` suvand. Näiteks:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Konto nimi peab olema kordumatu Azure piirkonnas konto loomist. See võib sisaldada ainult tähtedest ja numbritest koosnev väiketähed ja peab olema 3-24 märki. Te ei saa kasutada erimärke, nt `-` või `_` paketi konto nimed.

### <a name="linked-storage-account-autostorage"></a>Lingitud salvestusruumi konto (autostorage)

(Valikuline) **Üldine otstarve** salvestusruumi konto kontole paketi link, kui loote selle. Paketi [rakenduse paketid](batch-application-packages.md) funktsioon kasutab bloobimälu lingitud üldine otstarve salvestusruumi konto, ei [Paketi faili põhimõtted .NET](batch-task-output.md) teek. Need valikulised funktsioonid aitavad juurutamine tööülesannete paketi käivitamine rakendusi ja püsib toodavad andmed.

Lingi loomisel selle uue paketi konto Azure Storage konto, saate määrata selle `--autostorage-account-id` suvand. See suvand nõuab salvestusruumi konto täielik ressursi ID-d.

Esmalt Kuva salvestusruumi konto üksikasjad:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Seejärel kasutage **URL-i** väärtus on `--autostorage-account-id` suvand. URL-i väärtus algab selgitava lausega "/ tellimuste /" ja sisaldab teie tellimuse ID- ja ressursihalduse tee salvestusruumi konto.

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Paketi konto kustutamine

Kasutamine:

    azure batch account delete [options] <name>

Näide:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Kustutab määratud paketi konto. Küsimise korral veenduge, et eemaldada konto (konto eemaldamise võib võtta aega lõpuleviimiseks).

## <a name="manage-account-access-keys"></a>Kiirklahvide konto haldamine

Peate teie paketi konto võti [luua](#create-and-modify-batch-resources) ja muuta ressursid.

### <a name="list-access-keys"></a>Kiirklahvide loendi

Kasutamine:

    azure batch account keys list [options] <name>

Näide:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Loetleb konto võtmed antud paketi konto.

### <a name="generate-a-new-access-key"></a>Luua uusi Accessi võti

Kasutamine:

    azure batch account keys renew [options] --<primary|secondary> <name>

Näide:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Taastab antud paketi konto jaoks määratud kontovõti.

## <a name="create-and-modify-batch-resources"></a>Luua ja muuta paketi ressursid

Saate luua, lugeda, värskendada, kustutada (CRUD) paketi ressursid nagu kaustu, arvutada sõlmed, töö ja ülesannete Azure'i CLI. Neid toiminguid CRUD vajavad teie paketi konto nime, võti ja lõpp-punkti. Saate need määrata funktsiooni `-a`, `-k`, ja `-u` suvandid või määramine [keskkonna muutujate](#credential-environment-variables) CLI kasutab automaatselt (kui täidetud).

### <a name="credential-environment-variables"></a>Mandaadi keskkonna muutujad

Saate määrata `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, ja `AZURE_BATCH_ENDPOINT` keskkonna muutujate määratlemise asemel `-a`, `-k`, ja `-u` iga käsu täidate käsurea suvandid. Paketi CLI kasutab neid muutujaid (kui seadmine) nii, et saate argument on `-a`, `-k`, ja `-u` suvandid. Selles artiklis ülejäänud eeldab nende keskkonna muutujate kasutamine.

>[AZURE.TIP] Loend oma võtmed `azure batch account keys list`, ja kuvada konto lõpp-punkti `azure batch account show`.

### <a name="json-files"></a>JSON-failid

Paketi ressursid nagu kaustu ja -tööde haldamine loomisel saate määrata JSON faili sisaldava uue ressursi konfiguratsiooni mitte saata oma parameetrid käsurea suvandid. Näiteks:

`azure batch pool create my_batch_pool.json`

Kui palju ressursside loomine toiminguid ainult käsurea suvandite abil saate teha, mõned funktsioonid nõuavad JSON-vormingus faili, mis sisaldab ressursi üksikasjad. Näiteks peate kasutama JSON faili, kui soovite määrata start tööülesande ressursi failid.

JSON, looma ressursi leidmiseks viidata [paketi REST API viide] [ rest_api] dokumentatsiooni MSDN-is. Iga "Lisa *Ressursi tüüp"*teema sisaldab näide JSON ressurss, mille abil saate mallidena JSON failide loomise kohta. Näiteks JSON kausta loomiseks leiate [Lisa konto lisamine pool][rest_add_pool].

>[AZURE.NOTE] Kui määrate JSON faili loomisel ressursi, kõik muud saate määrata selle ressursi käsurea parameetrid ignoreeritakse.

## <a name="create-a-pool"></a>Koostada andmebaas

Kasutamine:

    azure batch pool create [options] [json-file]

Näide (virtuaalne arvuti konfiguratsiooni):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Näide (Cloud Services konfiguratsiooni):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Paketi teenus loob kogumi Arvuta sõlmed.

Nagu [paketi funktsioonide ülevaate](batch-api-basics.md#pool), kui valite operatsioonisüsteemi sõlmed olevate on kaks võimalust: **Virtuaalse masina konfiguratsiooni** ja **Cloud Services konfigureerimine**. Kasutage funktsiooni `--image-*` suvandid, et luua virtuaalse masina konfiguratsiooni, ja `--os-family` loomiseks Cloud Services konfigureerimine. Te ei saa määrata nii `--os-family` ja `--image-*` suvandid.

Saate määrata pool [rakenduse paketid](batch-application-packages.md) ja käsurea [tööülesande käivitamine](batch-api-basics.md#start-task). Ressurss faile start tööülesande määramiseks, peate selle asemel kasutada [JSON-fail](#json-files).

On kustutamiseks tehke järgmist.

    azure batch pool delete [pool-id]

>[AZURE.TIP] Märkige ruut [virtuaalse masina pilte loendi](batch-linux-nodes.md#list-of-virtual-machine-images) jaoks sobiv väärtused on `--image-*` suvandid.

## <a name="create-a-job"></a>Töö loomine

Kasutamine:

    azure batch job create [options] [json-file]

Näide:

    azure batch job create --id "job001" --pool-id "pool001"

Lisab töö paketi kontole ja määrab kausta, kuhu oma ülesandeid täita.

Töökoha kustutamiseks tehke järgmist.

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Loendi kaustu, töö, tööülesannete ja muud ressursid

Iga paketi ressursi tüüp toetab on `list` käsk päringut konto paketi ja ressursside seda tüüpi loendeid. Näiteks saate oma konto ja projekti tööülesanded loendis soovitud kaustu:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Ressursside tõhus loetelu

Kiirem päringute, saate määrata, **Valige**, **filtreerimine**ja **laiendamine** klausel suvandite `list` toimingud. Nende suvandite abil saate piirata paketi teenuse tagastatud andmeid. Kuna kõik filtreerimine toimub serveripoolse, ainult soovitud andmed, mida olete huvitatud ületab kaabel. Kasutage need sätted salvestada läbilaskevõime (ja seega kellaaeg) Kui loendi toiminguid teha.

Näiteks tagastab see ainult kaustu, kelle ID on alustada "renderTask":

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Paketi CLI toetab paketi teenus ei toeta kõik kolm klauslit:

* `--select-clause [select-clause]`Iga üksuse atribuutide alamhulga tagasi
* `--filter-clause [filter-clause]`Tagastab ainult üksused, mis vastavad määratud OData avaldis
* `--expand-clause [expand-clause]`Ühe aluseks ülejäänud kõne juriidilise teabe saamiseks. Laienda klausel toetab ainult selle `stats` atribuudi sel ajal.

Kolm klauslit ja tegelik loendi päringute nendega leiate teemast [Azure paketi teenuse tõhus päringu](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Paketi Rakendusehaldus

Rakenduse paketid võimalda lihtsustatud juurutada rakendused Arvuta sõlmi oma kaustadesse. Azure'i CLI, saate üles laadida rakenduse paketid, paketi versioonide haldamine ja kustutada paketid.

Loo uus rakendus ja lisada paketi versioon:

Rakenduse **loomine** :

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Lisa** rakendusepaketi:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktiveeri** paketi:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Rakenduse **vaikeversiooniks** seadmiseks tehke järgmist.

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Rakendusepaketi juurutamine

Kui loote uue kausta, saate määrata ühe või mitme rakenduse paketid juurutamiseks. Kui määrate paketi kausta loomise ajal, see juurutatud iga sõlme sõlm ühenduste pool. Paketid on ka juurutanud kui sõlme on rebooted või reimaged.

Määrake soovitud `--app-package-ref` valik pool, nagu nad liituda pool selle kausta sõlmed rakendusepaketi juurutamiseks loomisel. Funktsiooni `--app-package-ref` suvand aktsepteerib semikooloniga eraldatud loend juurutada Arvuta sõlmed rakenduste ID-sid.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Kui loote on käsureasuvandeid abil, ei saa määrata praegu *kasutatava* rakenduse paketi versiooni võtta kasutusele Arvuta sõlmed, näiteks "1.10-beta3". Seetõttu tuleb määrata vaikimisi versioon taotluse `azure batch application set [options] --default-version <version-id>` enne kausta loomist (vt eelmises jaotises). Siiski määramiseks paketi versioon mõeldud pool siis, kui kasutate [JSON faili](#json-files) asemel käsureasuvandeid pool loomisel.

Lisateavet rakenduse paketid leiate [Azure'i paketi rakenduse paketid rakenduse juurutamine](batch-application-packages.md).

>[AZURE.IMPORTANT] Peate kasutama rakenduse paketid kontole paketi [link Azure Storage konto](#linked-storage-account-autostorage) .

### <a name="update-a-pools-application-packages"></a>Mõne kausta rakenduse paketid värskendamine

Mõne olemasoleva kausta määratud rakenduste värskendamiseks probleemi selle `azure batch pool set` käsk koos selle `--app-package-ref` suvand:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Uue rakendusepaketi arvutada sõlmed juba mõne olemasoleva kausta juurutamiseks peate taaskäivitage või reimage need sõlmed:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] Saate hankida sõlmed pargis koos oma sõlm ID-d, loendit koos `azure batch node list`.

Pidage meeles, peab juba konfigureerinud rakenduse enne juurutamist vaikimisi versiooniga (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Tõrkeotsingu näpunäited

See jaotis on mõeldud teile ressursid kasutada, kui Azure'i CLI probleemide tõrkeotsing. See ei pruugi olla kõik probleeme lahendada, kuid aitab teil piiritlemiseks põhjuse ja valige käsk teid aidata ressursse.

* Kasutage `-h` CLI käsk saada **teksti spikker**

* Kasutage `-v` ja `-vv` kuvamiseks **Paljusõnaline** käsu väljund; `-vv` on "lisatasu" Paljusõnaline ja kuvab tegeliku ülejäänud taotlusi ja koosolekutaotluste. Need parameetrid on mugav tõrke täielik väljundi kuvamiseks.

* Saate vaadata **JSON käsu väljund** on `--json` suvand. Näiteks `azure batch pool show "pool001" --json` pool001's atribuudid kuvatakse JSON-vormingus. Seejärel saate kopeerida ja muuta selle väljundi kasutamine on `--json-file` (vt käesoleva artikli [JSON-failid](#json-files) ).

* [MSDN-i paketi Foorum] [ batch_forum] on suureks abiks ressurss ja paketi meeskonnaliikmete hoolikalt jälgida. Ärge unustage postitada oma küsimusi seal, kui teil tekib probleeme või soovite abi teatud toimingu.

* Mitte iga paketi ressursi toiming on praegu toetatud Azure'i k. Näiteks ei saa praegu määrata ka rakenduse paketi *versioon* mõeldud pool, ainult paketi ID Sel juhul peate sisestama mõne `--json-file` oma käsu asemel käsurea suvandite abil. Kindlasti kursis CLI uusim versioon, mis kättesaamine tulevaste täiustused.

## <a name="next-steps"></a>Järgmised sammud

*  Leiate [Azure'i paketi rakenduse paketid rakenduse juurutamine](batch-application-packages.md) , et teada saada, kuidas seda funktsiooni abil saate hallata ja täidate paketi Arvuta sõlmed rakenduste juurutamine.

* Lugege [päringu paketi teenuse tõhus](batch-efficient-list-queries.md) rohkem üksuste arv ja tüüp, mis tagastatakse päringuid paketi vähendamine.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx