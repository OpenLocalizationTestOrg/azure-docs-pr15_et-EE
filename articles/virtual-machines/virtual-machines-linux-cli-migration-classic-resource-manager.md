<properties
    pageTitle="Migreerimine IaaS ressursid klassikaline Azure'i ressursihaldur Azure'i CLI abil | Microsoft Azure'i"
    description="Selles artiklis tutvustatakse platvormi toetatud migreerimise ressursside kaudu klassikaline Azure'i ressursihaldur Azure'i CLI abil"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migreerimine IaaS ressursid klassikaline Azure'i ressursihaldur Azure'i CLI abil

Need juhised näitavad, kuidas kasutada Azure käsurea liides (CLI) käsud migreerida taristu teenus (IaaS) ressurssidena klassikaline juurutamise mudeli Azure'i ressursihaldur juurutamise mudel. See artikkel nõuab [Azure'i CLI](../xplat-cli-install.md).

>[AZURE.NOTE] Kõik toimingud, mida on kirjeldatud allpool, idempotent. Kui teil on probleeme peale toetuseta funktsiooni või konfiguratsiooni tõrge, soovitame teil proovige uuesti ettevalmistamine, katkestada või Kinnita toiming. Platvormi proovige siis uuesti.

## <a name="step-1-prepare-for-migration"></a>Samm 1: Migreerimiseks valmistumine

Siin on paar põhitõed, mida me soovitame kui hindate migreerimine IaaS ressursid klassikalises ressursside haldur.

- Lugege läbi [toetamata konfiguratsioone või funktsioonide loendi](virtual-machines-windows-migration-classic-resource-manager.md). Kui teil on toetamata konfiguratsioone või funktsioone kasutavate virtuaalmasinates, soovitame teil oodata funktsioonide/konfigureerimise tugiteenuste tuleb teada. Teise võimalusena saate eemaldada selle funktsiooni või kolima selle konfiguratsiooni lubamiseks migreerimise, kui teie vajadustele.
-   Kui teil on automatiseeritud skriptide, kus juurutada ja oma rakenduste täna, proovige luua sarnane test setup migreerimise nende skriptide abil. Teise võimalusena saate häälestada valimi keskkonnas Azure portaali kaudu.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Samm 2: Tellimuse seadmine ja registreerida pakkuja

Migreerimise stsenaariumid, peate nii klassikaline keskkonna häälestamine ja ressursihaldur. [Installige Azure'i CLI](../xplat-cli-install.md) ja [Valige oma tellimuse](../xplat-cli-connect.md).

Logige sisse oma kontole.
    
    azure login

Valige Azure tellimuse järgmise käsu abil.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Registreerimise on üks samm, kuid seda tuleb teha üks kord enne, kui proovite migreerimise. Registreerimata kuvatakse järgmine tõrketeade 

>   *BadRequest: Tellimus pole registreeritud migreerimise jaoks.* 

Järgmise käsu abil registreerida migreerimise ressursi pakkuja. Pange tähele, et mõnel juhul korda see käsk saada. Registreerimise on siiski eduka.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Oodake viis minutit registreerimise lõpuleviimiseks. Järgmise käsu abil saate kinnituse olek. Veenduge, et RegistrationState on `Registered` enne jätkamist.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Nüüd vahetada CLI on `asm` režiim.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Samm 3: Veenduge, et teil on piisavalt Azure'i ressursihaldur virtuaalse masina tuuma Azure piirkonna praeguse juurutamise või VNET

Selle toimingu jaoks peate aktiveerimiseks `arm` režiim. Tehke seda järgmise käsu abil.

```
azure config mode arm
```

Saate kontrollida summa teil on Azure ressursihaldur valdkond CLI järgmine käsk. Core kvootide kohta leiate lisateavet artiklist [piirangud ja Azure ressursihaldur](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Kui olete lõpetanud kinnitatava see toiming, mida saab vahetada tagasi `asm` režiim.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Samm 4: Variant 1 - migreerimine virtuaalmasinates mõnes pilveteenuses 

Saada pilveteenustega loendi järgmise käsu abil ja seejärel valige pilveteenuses, mida soovite migreerida. Pange tähele, et kui VMs pilveteenusesse, virtuaalse võrgus või kui need on web/töötaja rollid, kuvatakse tõrketeade.

    azure service list

Käivitage järgmine käsk saada juurutamise nimi pilveteenusesse Paljusõnaline väljund. Enamikul juhtudel juurutamise nimi on sama mis pilvepõhise teenuse nimi.

    azure service show <serviceName> -vv

Ettevalmistused virtuaalmasinates pilveteenuses migreerimise jaoks. Teil on kaks võimalust valida.

Kui soovite migreerida VMs platvormi loodud virtuaalse võrku, käsu.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Kui soovite olemasoleva virtuaalse võrgu ressursihaldur juurutamise mudeli migreerida, käsu.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Pärast ettevalmistamine toiming on edukalt, saate läbi vaadata Paljusõnaline väljund saada VMs migreerimise olek ja veenduge, et need on selle `Prepared` olekus.

    azure vm show <vmName> -vv

Kontrollige konfiguratsiooni valmis ressursid CLI või Azure portaali abil. Kui te ei ole valmis migreerimise ja soovite naasta ja vana, käsu.

    azure service deployment abort-migration <serviceName> <deploymentName>

Kui valmis konfiguratsiooni sobib, saate liikuda edasi ja kinnita ressursside järgmise käsu abil.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Samm 4: Variant 2 – virtuaalmasinates virtuaalse võrgu migreerimine

Valige virtuaalse võrgu, mida soovite migreerida. Pange tähele, et kui virtuaalse võrgu sisaldab toetuseta konfiguratsioone web/töötaja rollid ja VMs, saate valideerimise tõrketeade kuvatakse.

Saada virtuaalne võrkude tellimuse järgmise käsu abil.

    azure network vnet list
    
Väljund näeb välja umbes selline:

![Kuvatõmmis käsurea esile tõstetud kogu virtuaalse võrgu nimi.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Ülaltoodud näites on **virtualNetworkName** kogu nimi **"Rühma classicubuntu16 classicubuntu16"**.

Teie valitud virtuaalse võrgu migreerimiseks valmistumine järgmise käsu abil.

    azure network vnet prepare-migration <virtualNetworkName>

Kontrollige valmis virtuaalmasinates konfigureerimine CLI või Azure portaali abil. Kui te ei ole valmis migreerimise ja soovite naasta ja vana, käsu.

    azure network vnet abort-migration <virtualNetworkName>

Kui valmis konfiguratsiooni sobib, saate liikuda edasi ja kinnita ressursside järgmise käsu abil.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Juhis 5: Migreerimine salvestusruumi konto

Kui olete lõpetanud migreerimine on virtuaalmasinates, soovitame migreerite salvestusruumi konto.

Järgmise käsu abil migreerimise salvestusruumi konto ettevalmistamine

    azure storage account prepare-migration <storageAccountName>

Kontrollige konfiguratsiooni valmis salvestusruumi konto CLI või Azure portaali abil. Kui te ei ole valmis migreerimise ja soovite naasta ja vana, käsu.

    azure storage account abort-migration <storageAccountName>

Kui valmis konfiguratsiooni sobib, saate liikuda edasi ja kinnita ressursside järgmise käsu abil.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Järgmised sammud

- [Platvormi toetatud migreerimise IaaS ressursside klassikalises ressursside haldur](virtual-machines-windows-migration-classic-resource-manager.md)
- [Tehnika deep dive platvormi toetatud migreerimise kohta kaudu klassikalises ressursside halduri](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
