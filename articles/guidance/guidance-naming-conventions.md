<properties
   pageTitle="Soovitatav nimereeglid Azure ressursid | Juhised | Microsoft Azure'i"
   description="Soovitatav nimereeglid Azure ressursid. Kuidas nime virtuaalmasinates, salvestusruumi kontod, võrkude, virtuaalne võrkude, alamvõrku ja muud Azure üksused"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Soovitatav nimereeglid Azure ressursid

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Mis tahes ressursi Microsoft Azure nimi valik on oluline, sest:

- See on raske nimi hiljem muuta.
- Nimed peavad vastama nende ressursside teatud tüüpi.

Järjepidevad nimereeglid teha ressursid leidmise lihtsamaks. Need määrata rolli lahenduse ressursi.

See artikkel kehtib nime reeglite ja piirangute Azure ressursid kokkuvõte ja soovitused nimereeglid kogumi võrdlusalus.  Saate nende soovituste alguspunktina oma põhimõtted teatud oma vajadustele.  

Edu nimereeglid ja luua ja neid jälgima teie rakendused ja ettevõtted üle. 

## <a name="naming-subscriptions"></a>Nime andmise tellimused

Azure'i tellimused panemisel Paljusõnaline nimed veenduge, et konteksti ja iga tellimuse selge eesmärk.  Palju tellimusi keskkonnas töötamisel pärast ühiskasutuses nimetamistava selgust parandada.

Soovitatav mustri nime tellimused on:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Ettevõtte tavaliselt oleks sama iga tellimuse jaoks. Mõnes ettevõttes võib siiski lapse ettevõtete ettevõtte struktuuris. Need ettevõtted haldab keskse IT rühma. Sellisel juhul võivad need liigendatud nii ema ettevõtte nimi (*Contoso*) ja lapse ettevõtte nimi (*Põhja Tuul*).

- Osakonna on teie asutuses nimi, kus töötan isikute rühma. See üksus, valikuline nimeruumi.

- Toote rida on teatud toote või funktsioon, mis tehakse sees osakonna nimi.
See on üldiselt vabatahtlik sisemise suunatud teenused ja rakendused. Siiski see on soovitatav kasutada avalikkusele suunatud teenuseid, mis nõuavad lihtne hoida eraldi ja (nt selge eraldamine arveldus kirjeid).

- Keskkond on juurutamise elutsükli rakenduste ja teenuste, nt arendaja, kV või tootekataloogi kirjeldav nimi.

| Ettevõtte | Osakonna | Toote või teenuse | Keskkonnas | Täisnimi  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Tootmise | Contoso SocialGaming AwesomeService tootmine |
| Contoso | SocialGaming | AwesomeService | Arendaja | Contoso SocialGaming AwesomeService arendaja |
| Contoso | SEE | InternalApps | Tootmise | Contoso IT InternalApps tootmise |
| Contoso | SEE | InternalApps | Arendaja | Contoso IT InternalApps arendaja |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Kasutage kinnitab tähendus

Ressursid Azure panemisel on soovitatav kasutada levinud eesliiteid või sufiksid tüüp ja ressursi kontekstis.  Ajal tüüp, metaandmete teavet konteksti, on saadaval programmiliselt, rakendades levinud kinnitab lihtsustab visuaalse ID.  Kui teie nimetamistava sisaldavad kinnitab, on oluline täpselt kindlaks määrata, kas selle kinnitada on nimi (eesliide) alguses ja lõpus (järelliide).  

Näiteks siin on kaks võimalikku nimed hostiteenuse arvutamise mootor teenuse.

- SvcCalculationEngine (eesliide)
- CalculationEngineSvc (järelliide)

Kinnitab võib viidata erinevaid aspekte, mis kirjeldavad teatud ressursid. Järgmises tabelis on mõned näited tavaliselt kasutatakse.

| Proportsioonid | Näide | Märkmete |
| ------ | ------- | ----- |
| Keskkonnas | Arendaja tootekataloogi kv | Tuvastab ressursi keskkonnas |
| Asukoht | UW (meile Lääne), ue (meile Ida) | Tuvastab piirkond, kuhu on juurutatud ressurss |
| Eksemplari | 01, 02 | Ressursid, mis on rohkem kui üks nimega eksemplari (web serverid jne). |
| Toote või teenuse | teenus | Tuvastab toote, rakenduse või teenus, mis toetab ressurss |
| Roll | SQL-i, web, sõnumside | Tuvastab seotud ressursside roll |

Teatud nimereeglistik oma ettevõtte või projekti väljatöötamisel on oluline valida ühised kinnitab ja nende asukoht (järelliide või eesliide).

## <a name="naming-rules-and-restrictions"></a>Nime andmise reeglid ja piirangud

Iga ressursi või teenuse tüüp Azure jõustab kogumi nime andmise piirangud ja ulatus; mis tahes nimetamistava või mustri peavad vastama nõutud nime reeglid ja ulatus.  Näiteks ajal VM nime kaardid DNS-i nimi (ja seega peab olema kordumatu kõigis Azure), on VNET nimi on rakendatud sees loodav ressursirühma.

Üldiselt vältida erimärke (`-` või `_`) esimese või viimase märgina mis tahes nime. Enamik valideerimisreeglite nurjumise põhjustab järgmisi märke.

| Kategooria | Teenuse või üksus | Ulatus | Pikkus | Kest | Lubatud märgid. | Soovitatud muster | Näide |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Ressursirühm | Ressursirühm | Globaalne | 1-64 | Väiketähed | Tähtedest ja numbritest koosnev, allkriips ja sidekriips | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Ressursirühm | Kättesaadavus määramine | Ressursirühm | 1-80 | Väiketähed | Tähtedest ja numbritest koosnev, allkriips ja sidekriips | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Üldine | Sildi | Seotud üksus | 512 (nimi), 256 (väärtus) | Väiketähed | Tähtedest ja numbritest koosnev | `"key" : "value"` | `"department" : "Central IT"` |
| Arvuta | Virtuaalse masina | Ressursirühm | 1-15 | Väiketähed | Tähtedest ja numbritest koosnev, allkriips ja sidekriips | `<name>-<role>-<instance>` | `profx-sql-001` |
| Salvestusruumi | Salvestusruumikonto nimi (andmed) | Globaalne | 3-24 | Väiketähed | Tähtedest ja numbritest koosnev | `<service short name><type><number>` | `profxdata001` |
| Salvestusruumi | Salvestusruumikonto nimi (ketast) | Globaalne | 3-24 | Väiketähed | Tähtedest ja numbritest koosnev | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Salvestusruumi | Container nimi | Salvestusruumi konto | 3-63 |   Väiketähed | Tähtedest ja numbritest koosnev ja mõttekriipsu | `<context>` | `logs` |
| Salvestusruumi | Bloobimälu nimi | Container | 1-1024 | Tõstutundlik | URL-i char | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Salvestusruumi | Järjekorra nimi | Salvestusruumi konto | 3-63 | Väiketähed | Tähtedest ja numbritest koosnev ja mõttekriipsu | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Salvestusruumi | Tabeli nimi | Salvestusruumi konto | 3-63 |Väiketähed | Tähtedest ja numbritest koosnev | `<service short name>-<context>` | `awesomeservice-logs` |
| Salvestusruumi | Faili nimi | Salvestusruumi konto | 3-63 | Väiketähed | Tähtedest ja numbritest koosnev | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Võrgunduse | Virtuaalse võrgu (VNet) | Ressursirühm | 2-64 | Väiketähed | Tähtedest ja numbritest koosnev, mõttekriipsu, allkriips ja perioodi kohta. | `<service short name>-[section]-vnet` | `profx-vnet` |
| Võrgunduse | Alamvõrgu | Ema VNet | 2-80 | Väiketähed | Tähtedest ja numbritest koosnev, allkriips, mõttekriipsu ja perioodi kohta. | `<role>-subnet` | `gateway-subnet` |
| Võrgunduse | Võrgu liidese | Ressursirühm | 1-80 | Väiketähed | Tähtedest ja numbritest koosnev, mõttekriipsu, allkriips ja perioodi kohta. | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Võrgunduse | Võrgu turberühm | Ressursirühm | 1-80 | Väiketähed | Tähtedest ja numbritest koosnev, mõttekriipsu, allkriips ja perioodi kohta. | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Võrgunduse | Võrgu turberühma reegel | Ressursirühm | 1-80 | Väiketähed | Tähtedest ja numbritest koosnev, mõttekriipsu, allkriips ja perioodi kohta. | `<descriptive context>` | `sql-allow` |
| Võrgunduse | Avaliku IP-aadress | Ressursirühm | 1-80 | Väiketähed | Tähtedest ja numbritest koosnev, mõttekriipsu, allkriips ja perioodi kohta. | `<vm or service name>-pip` | `profx-sql1-pip` |
| Võrgunduse | Laadi koormusetasakaalustusteenuse | Ressursirühm | 1-80 | Väiketähed | Tähtedest ja numbritest koosnev, mõttekriipsu, allkriips ja perioodi kohta. | `<service or role>-lb` | `profx-lb` |
| Võrgunduse | Tasakaalustatud reeglid Config laadimine | Laadi koormusetasakaalustusteenuse | 1-80 | Väiketähed | Tähtedest ja numbritest koosnev, mõttekriipsu, allkriips ja perioodi kohta. | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Ressursside siltidega korraldamine

Azure'i ressursihaldur toetab Suhtlussiltide üksuste suvalise tekstistringi tuvastamine konteksti ja sujuvamaks muutmine automatiseerimine.  Näiteks silti `"sqlVersion: "sql2014ee"` võiks ära tunda VMs töötab SQL Server 2014 Enterprise Edition töötab ka automatiseeritud skripti nende vastu juurutamine.  Siltide tuleks kasutada tõsta ja täiustamiseks kontekstis servas nimereeglid, mis on valitud.

> [AZURE.TIP]Üks muude siltide eeliseid on see, et sildid üle ressursi rühmad, võimaldab teil link ja oleksid üksuste eri juurutuste üle.

Iga ressursi või ressursirühma võib olla kuni **15** silte. Sildi nimi on piiratud 512 märki ja sildi väärtus on piiratud maksimaalselt 256 märki.

Ressursi sildistamise kohta leiate Lisateavet vaadake [kasutamine siltide korraldamiseks oma Azure ressursse](../resource-group-using-tags.md).

Mõned levinud Suhtlussiltide kasutamine olukorda on:

- **Arveldus**; Ressursside rühmitamis- ja seostada neid arveldus- või tasuta tagasi koode.
- **Teenuse konteksti ID**; Rühmade ressursside tuvastamine ressursi rühma levinud toiminguid ja rühmitamine
- **Juurdepääsu reguleerimine ja turvalisuse kontekstis**; Administraatori rolli ID põhjal portfellivalikuotsuse, süsteemi, teenuse, rakenduse, näiteks jne.

> [AZURE.TIP]Sildistamine varem – sageli silt.  Parem on sildistamine värviskeemi kohas võrdlusalus ja üle aja asemel lisada pärast tegelikult reguleerimine.  

Näiteks mõned levinud Suhtlussiltide võimalustest:

| Sildi nimi | Klahv | Näide | Kommentaari |
| -------- | --- | ------- | ------- |
| Arve / sisemine tagasinõude ID | billTo  | `IT-Chargeback-1234` | Sisemine I/O- või arveldusadministraatorina kood |
| Tehtemärk või otse vastutav isik (DRI) | managedBy | `joe@contoso.com`  | Pseudonüümi või e-posti aadress |
| Projekti nimi | projekti nimi | `myproject`  | Projekti või toote rea nime |
| Projekti versioon | projekti-versioon | `3.4`  | Rea Projecti või toote versioon |
| Keskkonnas | keskkonnas | `<Production, Staging, QA >` | Keskkonna identifikaator | 
| Esimese taseme | esimese taseme | `Front End, Back End, Data` | Esimese taseme või rolli/konteksti ID |
| Profiili andmed | dataProfile | `Public, Confidential, Restricted, Internal` | Andmed salvestatakse ressursi tundlikkust |
 
## <a name="tips-and-tricks"></a>Näpunäited ja nipid

Teatud tüüpi ressursid võib olla vaja täiendavat hooldust nime andmise ja põhimõtted.

### <a name="virtual-machines"></a>Virtuaalmasinates

Suuremate topoloogiatest, eriti hoolikalt nimetamine virtuaalmasinates sujuvamaks tuvastamise rolli ja iga seadme otstarbe ja nõuaksid vähem lisatööd skriptimise.

> [AZURE.WARNING]Iga virtuaalse masina Azure on mõni Azure ressursinimi ja on operatsioonisüsteem hosti nimi.  
> Kui ressursinimi ja hosti nimi on erinevad, haldamise VMs võib olla keeruline ja vältida.
> Näiteks kui virtuaalse masina on juba loodud on .vhd sisaldab operatsioonisüsteem on konfigureeritud koos hostinimi.

- [Windows Serveri vms nimereeglid](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Salvestusruumi kontod ja salvestusruumi üksused

On kaks esmane kasutamine juhtumeid salvestusruumi kontode - varundamine ketast vms ja andmete salvestamiseks plekid, järjekorrad ja tabeleid.  Salvestusruumi kontod kasutatakse VM ketast peaks järgige, kaasates neid ema VM nime nimetamistava (ja võimalike mitmelt salvestusruumi vaja klassi VM SKU-de, rakendada arv järelliide).

> [AZURE.TIP]Salvestusruumi kontod – andmete või ketast - jaoks tuleks järgige nimereeglistik, mis võimaldab salvestusruumi mitmelt kasutada (nt alati abil arvuline järelliide).

On võimalik konfigureerida kohandatud domeeninime bloobimälu andmete Azure Storage konto.
Lõpp-punkti bloobimälu teenuse jaoks vaikimisi `https://mystorage.blob.core.windows.net`.

Kuid kui vastendate konto salvestusruumi bloobimälu lõpp-punkti kohandatud domeeni (nt www.contoso.com), võite kasutada ka Bloobivahemälu teie salvestusruumi andmeid, kasutades seda domeeni. Näiteks koos kohandatud domeeninime `http://mystorage.blob.core.windows.net/mycontainer/myblob` võiks juurde nimega `http://www.contoso.com/mycontainer/myblob`.

Selle funktsiooni konfigureerimise kohta lisateabe saamiseks vaadake [konfigureerimine kohandatud domeeninime bloobimälu salvestusruumi lõpp-punkti](../storage/storage-custom-domain-name.md).

Nime andmise plekid, ümbriste ja tabelite kohta lisateavet:

- [Nime andmise ja ümbriste, plekid ja metaandmete viitamine](https://msdn.microsoft.com/library/dd135715.aspx)
- [Nime andmise järjekorrad ja metaandmed](https://msdn.microsoft.com/library/dd179349.aspx)
- [Nime andmise tabelid](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Bloobimälu nimi võib olla mis tahes märkide kombinatsioon, kuid reserveeritud URL-i märkide peab tuleb õigesti seda vältida. Vältige bloobimälu nimed, mis lõppeda punktiga (.), kaldkriips (/) või jada või nende kahe kombinatsioonist. Kaldkriips on mess, **virtuaalse** directory eraldaja. Ärge kasutage kaldkriips (\) bloobimälu nime. Kliendi API võimaldab seda, kuid seejärel ei räsi õigesti ja allkirjad ei sobi.

Ei saa muuta salvestusruumi konto või container nime pärast selle loomist.
Kui soovite kasutada uut nime, saate kustutada ja uue konto loomiseks.

> [AZURE.TIP] Soovitame enne uue teenuse või rakenduse luua nimereeglistik salvestusruumi kontod ja -tüübid.

## <a name="example---deploying-an-n-tier-service"></a>Näide - n-astme teenuse juurutamine

Selles näites me määratleda n-astme teenus konfigureerimisel ees IIS-i serverite (majutatud Windows Server VMs), SQL Server (majutatud kaks Windows Server VMs), mis koosneb ElasticSearch kobar, mis (majutatud 6 Linux VMs) ja seostatud salvestusruumi kontod, virtuaalse võrgu, ressursside rühmitada ja laadimine koormusetasakaalustusteenuse.

Me alustame kontekstipõhine põhimõtted selle rakenduse määratledes:

| Üksuse | Mess | Kirjeldus  |
| ------ | ---------- | ------------ |  
| Teenuse nimi | `profx` | Rakenduse või teenuse kasutusele lühike nimi |
| Keskkonnas | `prod` | See on tootmise juurutamiseks (erinevalt kv testi jne.) |

Selle võrreldes me saab siis visandada selle põhimõtted iga tüüpi ressurss:

| Ressursi tüüp | Mess Base | Näide |
| ------------- | --------------- | ------- |
| Tellimuse | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Ressursirühm | `servicename-rg` | `profx-rg` |
| Virtuaalse võrgu | `servicename-vnet` | `profx-vnet` |
| Alamvõrgu | `role-subnet` | `sql-vnet` |
| Laadi koormusetasakaalustusteenuse | `servicename-lb` | `profx-lb` |
| Virtuaalse masina | `servicename-role[number]` | `profx-sql0` |
| Salvestusruumi konto | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Nagu näha skeem järgmist:

![rakenduse topoloogia skeem] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Valimi Otsingurakenduse topoloogia")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Näidis - Azure CLI skript ülaltoodud valimi juurutamine

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
