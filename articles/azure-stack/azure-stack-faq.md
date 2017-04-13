<properties
    pageTitle="Korduma kippuvad küsimused Azure'i virnas | Microsoft Azure'i"
    description="Azure'i virnas korduma kippuvad küsimused."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Korduma kippuvad küsimused Azure'i virnas

## <a name="deployment"></a>Juurutamine

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Vaja enne alustamist või taaskäivitada installi minu andmed ketast vormindada?

Ketast peaks olema vormingusse. Kui opsüsteemi uuesti installima, peate võib-olla kui vana talletusmahtu on endiselt alles, märkige ja kustutada, tehes järgmist:

1. Avage serveri haldur.
2. Valige salvestusruumi kaustu.
3. Vaadake, kui loendis on talletusmahtu.
4. Kui loetletud ja lubada lugemine / kirjutamine, paremklõpsake **talletusmahtu** .
5. Paremklõpsake **virtuaalse kõvaketta** (vasakus allnurgas) ja valige Kustuta.
6. Paremklõpsake **Talletusmahtu** ja klõpsake nuppu Kustuta.
7. Azure'i virnas skripti uuesti käivitada ja veenduge, et ketta kontrollimise edastab.

Soovi korral saate kasutada järgmist skripti:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Saate kasutada kõigi SSD ketta talletusmahtu POC installi?

Selle konfiguratsiooni ei toetata selles versioonis.  Lisateabe saamiseks vt lisateavet [nõuded juhend](azure-stack-deploy.md) .

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Saate kasutada Microsoft Azure'i virnas POC NVMe andmete ketast?

Salvestusruumi tühikuid otsese toetab NVMe ketast, toetab Azure virnas võimalike ketas tüübid ja kombinatsioonid alamhulga ainult salvestusruumi tühikuid Direct. 

### <a name="how-can-i-reinstall-azure-stack"></a>Kuidas Azure'i virnas uuesti installida?
Te saate järgige [positsioonide juhend](azure-stack-redeploy.md).  

## <a name="tenant"></a>Rentniku

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Saate oma pilte saab juurutada nimega rentniku jaoks?

Jah, nagu Azure, rentniku saate üles laadida pilte Azure'i virnas, lisaks piltide administraatorilt kasutamise. Ülevaate leiate teemast [lisamise VM pilt](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testimine

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Microsoft Azure'i virnas POC testimiseks saab kasutada pesastatud Virtualization?

Pesastatud virtualization on toetatud või katsetada Azure virnas Technical Preview 2.

## <a name="virtual-machines"></a>Virtuaalmasinates

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Kas Azure'i virnas toetab dünaamiline ketast virtuaalmasinates?

Azure'i virnas ei toeta dünaamiline ketast.

## <a name="next-steps"></a>Järgmised sammud

[Tõrkeotsing](azure-stack-troubleshooting.md)
