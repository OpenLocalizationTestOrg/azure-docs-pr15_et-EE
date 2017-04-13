<properties
    pageTitle="Azure Active Directory domeeniteenused: Administreerimiskeskuse juhend | Microsoft Azure'i"
    description="Liituda virtuaalse masina Windows Azure'i PowerShelli ja klassikaline juurutamise mudeli abil hallatavad domeeniga."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Windows Server virtuaalse masina hallatava domeeni PowerShelli kaudu liitumine

> [AZURE.SELECTOR]
- [Azure'i klassikaline portaali - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShelli - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [ressursihaldur ja klassikaline](../resource-manager-deployment-model.md). Selles artiklis antakse ülevaade klassikaline juurutamise mudeli abil. Azure'i AD domeeniteenused ei toeta praegu ressursihaldur mudel.

Need juhised näitavad, kuidas kohandada kogumi Azure PowerShelli käske, luua ja preconfigure Windowsi-põhiste Azure virtuaalse masina koosteüksuse lähenemisviisi abil. Järgmised toimingud aidata teil luua Windowsi-põhiste Azure virtuaalse masina ja sellega liituda Azure AD domeeniteenused hallatava domeeni.

Neid juhiseid järgida täitke-in-the-tühjad lähenemisviisi Azure PowerShelli käsu komplekti loomise kohta. Seda moodust võib olla kasulik, kui olete PowerShelli või soovite teada, mis väärtuste määramiseks eduka konfiguratsiooni. Täiustatud PowerShelli kasutajad saavad võtta käskude ja lisada oma väärtused muutujate (read, alustades "$").

Kui te pole seda veel teinud, kasutage juhised [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installida Azure PowerShelli kohalikus arvutis. Avage Windows PowerShelli Käsuviip.

## <a name="step-1-add-your-account"></a>Samm 1: Konto lisamine

1. PowerShelli käsuviibas tippige **Lisa-AzureAccount** ja klõpsake nuppu **Sisesta**.
2. Tippige oma Azure tellimusega seostatud meiliaadress ja klõpsake nuppu **Jätka**.
3. Tippige oma konto parool.
4. Klõpsake nuppu **Logi sisse**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Samm 2: Määrake oma tellimust ja salvestusruumi konto

Saate häälestada ja Azure tellimuse salvestusruumi konto töötab Windows PowerShelli käsuviibas järgmised käsud. Asendage kõik hinnapakkumised, sh selle < ja > märkide õige nimedega.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

**Get-AzureSubscription** käsu väljund SubscriptionName vara pääsevad õige tellimuse nime. Saad õige salvestusruumi konto nime **Get-AzureStorageAccount** käsu väljund atribuut silt pärast **Valige-AzureSubscription** käsu.


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Samm 3: Üksikasjalik juhend - virtuaalse masina ettevalmistamine ja liituda hallatava domeeni
Siin on seatud luua selle virtuaalse masina, tühjad read vahel iga ploki loetavuse vastava Azure PowerShelli käsu.

Määrake teave Windows virtuaalse masina olema ette valmistatud.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Vt InstanceSize väärtused D, DS- või G-sarja virtuaalmasinates, [virtuaalse masina ja pilvepõhise teenuse suurused Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Teavet hallatavate domeeni kohta.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Nimetage pilveteenusesse.

    $svcname="Contoso100-test"

Määrake, mis on ühendatud VM virtuaalse võrgu nimi. Veenduge, et AAD-DS hallatava domeeni on saadaval virtuaalse võrku.

    $vnetname="MyPreviewVnet"

Valige VM pildi ettevalmistamine VM kasutada.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigureerige VM - määramine VM nime, mõõtmed ja kasutatavat pilti.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Kohaliku administraatori identimisteabe saamiseks VM. Valige tugev kohaliku administraatori parool.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Hankige 'AAD näiteks Põhiliselt administraatorid' rühma liituda VM hallatava domeeni kasutajakonto mandaat. Ei määra domeeninimi – näiteks siinses näites, saame määrata "bob" kasutajanimi.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Konfigureerimine VM – määrata domeeni Liitu nõue ja nõutud mandaatidega.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Määrake soovitud alamvõrgu VM.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Valikuline: Osutage domeeni IP-aadress. Kui seate Azure AD domeeniteenused hallatava domeeni DNS-i serverid virtuaalse võrgu IP-aadressid, pole vaja seda toimingut.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Nüüd säte domeeni ühendatud Windows VM.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Skripti Windows VM ettevalmistamine ja sellega liituda automaatselt AAD domeeniteenused hallatava domeeni
See PowerShelli käsu Sea loob virtuaalse masina ärivaldkonna server mis:

- Kasutab Windows Server 2012 R2 andmekeskuse pilt.
- On ka lisatasu small virtuaalse masina.
- On nimi contoso-testi.
- On automaatselt domeeni ühendatud contoso100 hallatava domeeni.
- Lisatakse hallatava domeeni virtuaalse samasse võrku.

Siin on täielik valimi skripti loomine Windowsi virtuaalse masina ja automaatselt liituda Azure AD domeeniteenused hallatava domeeni.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Seotud teemad
- [Domeeniteenused Azure'i AD - kasutusjuhend](./active-directory-ds-getting-started.md)

- [Mõni Azure AD domeeniteenused hallatava domeeni haldamine](./active-directory-ds-admin-guide-administer-domain.md)
