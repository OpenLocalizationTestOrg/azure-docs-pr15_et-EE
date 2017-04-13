<properties
   pageTitle="Kuidas hallata juurdepääsu juhtimine loendid (ACL) lõpp-punktide jaoks PowerShelli abil"
   description="Saate teada, kuidas hallata ACL PowerShelli abil"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Kuidas hallata juurdepääsu juhtimine loendid (ACL) lõpp-punktide jaoks PowerShelli abil

Saate luua ja hallata võrgu juurdepääsu juhtimine loendid (ACL) jaoks lõpp-punktid Azure PowerShelli abil või haldusportaal. Sellest teemast leiate toimingute ACL tavalised toimingud, mida saate täita PowerShelli kaudu. Azure'i PowerShelli cmdlet-käskude loetelu leiate [Azure'i halduse cmdletid](http://go.microsoft.com/fwlink/?LinkId=317721). ACL-ide kohta leiate lisateavet teemast [mis on soovitud võrgu juurdepääsu juhtimine loend (ACL)?](virtual-networks-acl.md). Kui soovite hallata oma ACL haldusportaali abil, vaadake, [Kuidas lõpp üles virtuaalse masina](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Azure'i PowerShelli abil võrgu ACL haldamine

Azure'i PowerShelli cmdlet-käskude abil saate loomine, eemaldamine ja konfigureerimine (seadmine) võrgu juurdepääsu juhtimine loendid (ACL). Oleme lisanud mõned näited mõned viisid, kuidas mõni ACL PowerShelli abil saate konfigureerida.

ACL PowerShelli cmdlet-käskude täieliku loendi toomiseks saate teha ühte järgmistest.

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Võrgu ACL lubada juurdepääsu remote alamvõrgu reeglite loomine

Järgmisel näitel näha, kuidas luua uus ACL, mis sisaldab reegleid. Selle ACL rakendatakse siis virtuaalse masina lõpp-punkti. Järgmises näites ACL reeglid võimaldab juurdepääsu remote alamvõrgu. Luua uue võrgu ACL luba reeglid remote alamvõrku, avage mõni Azure PowerShell ISE. Kopeerimine ja kleepimine skripti all konfigureerimise skripti oma väärtustega ja seejärel käivitage skripti.

1. Saate luua uue võrgu ACL objekti.

        $acl1 = New-AzureAclConfig

1. Saate määrata reegli, mis lubab juurdepääsu remote alamvõrgu kaudu. Järgmises näites, määrake remote alamvõrgu *10.0.0.0/8* juurdepääsu virtuaalse masina lõpp-punkti reegli *100* (mis on üle 200 ja kõrgema reegli eelistus). Asendage väärtused ise konfiguratsiooni nõuded. Nime "SharePointi ACL config" asendatakse sõbralik nimi, mida soovite selle reegli helistada.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Korrake täiendavate reeglite cmdlet-käsk, asendades väärtused ise konfiguratsiooni nõuded. Veenduge, et reeglit muuta arv kajastamiseks järjestuses, milles soovite rakendatavad reeglid. Väiksem reegli arv ülimuslik suurem arv.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Järgmisena saate luua uue lõpp-punkti (lisa) või määrata ACL olemasoleva lõpp (seatud). Selles näites me uue virtuaalse masina lõpp-punkti nimega "web" lisada ja värskendada virtuaalse masina lõpp-punkti sätetega ACL.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Järgmiseks ühendamiseks cmdlet-käskude ja käivitage skript. Selle näite puhul ühendatud cmdlettide näeb välja järgmine:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Võrgu ACL reegli, mis lubab juurdepääsu alates remote alamvõrgu eemaldamine

Järgmisel näitel näha, kuidas eemaldada võrgu ACL reegli.  Võrgu ACL reegel on luba reeglid remote alamvõrgu eemaldamiseks avage mõni Azure PowerShell ISE. Kopeerimine ja kleepimine skripti all konfigureerimise skripti oma väärtustega ja seejärel käivitage skripti.

1. Kõigepealt tuleb teil saada võrgu ACL objekti virtuaalse masina lõpp-punkti. Seejärel saate eemaldada, ACL reegel. Sel juhul me on eemaldamine selle reegli ID. See eemaldab ainult ACL reegli ID 0. See ei eemalda ACL objekti virtuaalse masina lõpp-punkti.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Järgmiseks peate võrgu ACL objekti rakendamine virtuaalse masina lõpp-punkti ja virtuaalse masina värskendada.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Eemaldage võrgu ACL virtuaalse masina lõpp

Teatud juhtudel võib eemaldatavat võrgu ACL objekti virtuaalse masina lõpp. Selleks, et avada mõne Azure PowerShell ISE. Kopeerimine ja kleepimine skripti all konfigureerimise skripti oma väärtustega ja seejärel käivitage skripti.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Järgmised sammud

[Milleks on võrgu juurdepääsu juhtimine loend (ACL)?](virtual-networks-acl.md)
