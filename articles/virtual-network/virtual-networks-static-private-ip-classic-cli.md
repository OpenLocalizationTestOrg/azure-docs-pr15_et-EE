<properties 
   pageTitle="Kuidas seada privaatne staatiline IP klassikaline režiim ausing CLI | Microsoft Azure'i"
   description="Privaatne staatiline IP (DIPs) ja kuidas neid hallata klassikaline režiim abil CLI mõistmine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Kuidas määrata staatilise privaatne IP-aadress (klassikaline) Azure'i CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Samuti saate [hallata staatilise privaatne IP-aadress ressursihaldur juurutamise mudeli](virtual-networks-static-private-ip-arm-cli.md).

Proovi Azure'i CLI käsud allpool oodatud lihtne keskkond juba loonud. Kui soovite käivitada käske, nagu need on kuvatud selles dokumendis, esmalt koostada testi keskkond, mis on kirjeldatud [on vnet loomine](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kuidas määrata staatilise privaatne IP-aadress VM loomisel
Uue nimega *DNS01* uue pilveteenuses, nimega *TestService* ülaltoodud stsenaariumi VM loomiseks tehke järgmist.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.
1. **Azure'i teenus luua** pilveteenusesse loomiseks käsu käivitamine.

        azure service create TestService --location uscentral

    Oodatav väljund:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. **Azure'i loomine vm** VM loomiseks käsu käivitamine. Pange tähele väärtus staatilise privaatne IP-aadress. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Oodatav väljund:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (või - asukoht)**. Azure'i regioon, kus luuakse VM. Meie stsenaariumi *centralus*.
    - **-n (või--vm-nimi)**. Luua VM nimi.
    - **-w (või--virtuaalse võrgu nimi)**. VNet, kus luuakse VM nimi. 
    - **-S (või--staatiline ip)**. Staatilise privaatne IP-aadress VM.
    - **TestService**. Pilveteenuses, kus luuakse VM nimi.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage Windowsi-2012R2-x64-v14.2**. Pilt, mida saab luua VM.
    - **adminuser**. Kohalik administraator Windows VM.
    - **AdminP@ssw0rd**. Kohaliku administraatori parooli Windows VM.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Kuidas hankida staatilise privaatne IP-aadressi teave VM
Staatilise privaatne IP-aadressi teavet loodud ülaltoodud skripti VM vaatamiseks käivitage järgmine käsk Azure'i CLI ja jälgitakse *Võrgu StaticIP*väärtus:

    azure vm static-ip show DNS01

Oodatav väljund:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kuidas eemaldada VM staatilise privaatne IP-aadress
Eemaldamiseks staatilise privaatne IP-aadress lisatakse VM eeltoodud skripti Azure CLI järgmine käsk:
    
    azure vm static-ip remove DNS01

Oodatav väljund:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Kuidas lisada mõne olemasoleva VM privaatne staatiline IP
Staatiline lisamiseks privaatne IP-aadressi ülaltoodud umbes skripti abil loodud VM ta järgmine käsk:

    azure vm static-ip set DNS01 192.168.1.101

Oodatav väljund:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [reserveeritud avaliku IP](virtual-networks-reserved-public-ip.md) -aadressid.
- Lisateavet [astme avaliku IP (ILPIP)](virtual-networks-instance-level-public-ip.md) aadresside kohta.
- Konsulteerige [reserveeritud IP REST API-d](https://msdn.microsoft.com/library/azure/dn722420.aspx).
