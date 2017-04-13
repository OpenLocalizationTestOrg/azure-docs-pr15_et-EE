<properties 
   pageTitle="Klassikaline režiim Azure'i portaalis privaatne staatiline IP määramine | Microsoft Azure'i"
   description="Privaatne staatiline IP-d ja kuidas neid hallata klassikaline režiim Azure'i portaalis mõistmine"
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
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Kuidas määrata staatilise privaatne IP-aadress (klassikaline) Azure'i portaalis

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Samuti saate [hallata staatilise privaatne IP-aadress ressursihaldur juurutamise mudeli](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Valimi alltoodud juhiseid oodatud lihtne keskkond juba loonud. Kui soovite käivitada juhiseid, nagu need on kuvatud selles dokumendis, esmalt koostada testi keskkond, mis on kirjeldatud [on vnet loomine](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kuidas määrata staatilise privaatne IP-aadress VM loomisel
Nimega *DNS01* on VNet nimega *TestVNet* *192.168.1.101*, Privaatne staatiline IP alamvõrgu *FrontEnd* VM loomiseks tehke järgmist:

1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake nuppu **Uus** > **arvutada** > **Windows Server 2012 R2 andmekeskuses**, märkate, et loendis **Valige juurutamise mudeli** juba näitab **klassikaline**ja klõpsake nuppu **Loo**.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. **Luua VM** tera, sisestage soovitud nimi (meie stsenaariumi*DNS01* ), luua VM kohaliku administraatorikonto ja parool.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Klõpsake nuppu **Valikuline konfiguratsiooni** > **võrgu** > **Virtuaalse võrgu**ja klõpsake seejärel nuppu **TestVNet**. Kui **TestVNet** pole saadaval, veenduge, et kasutate *USA keskse* asukoha ja olete loonud testimiskeskkonnas, käesoleva artikli alguses kirjeldatud.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. **Võrgu** tera, veenduge, et praegu valitud alamvõrgu on *FrontEnd*, seejärel klõpsake **IP-aadressid**, klõpsake jaotises **IP address ülesande** **staatilise**ja sisestage *192.168.1.101* **IP** -aadress nagu allpool näha.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. **IP-aadresside** tera, klõpsake nuppu **OK** ja seejärel klõpsake nuppu **OK** **võrgu** tera ja **Valikuline config** tera klõpsake nuppu **OK** .
7. Labale **VM loomiseks** nuppu **Loo**. Teade kuvatakse armatuurlaua allpool paan.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Kuidas hankida staatilise privaatne IP-aadressi teave VM

Staatilise privaatne IP-aadressi teavet loodud eespool antud juhiseid VM vaatamiseks käivitada alltoodud juhiseid.

1. Azure'i Azure portaalis, klõpsake nuppu **SIRVI kõik** > **virtuaalmasinates (klassikaline)** > **DNS01** > **Kõik sätted** > **IP-aadressid** ja IP-aadressi ülesanne ja IP-aadresside nagu allpool näha teatis.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kuidas eemaldada VM staatilise privaatne IP-aadress
Ülaltoodud loodud VM staatilise privaatne IP-aadressi eemaldamiseks tehke järgmist.
    
1. Keelest **IP-aadresside** eespool kuvatud, klõpsake **dünaamiline** **IP address ülesande**paremal ja seejärel klõpsake nuppu **Salvesta**ja seejärel nuppu **Jah**.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kuidas lisada mõne olemasoleva VM staatilise privaatne IP-aadress
Lisada staatilisi privaatne IP-aadress VM loodud eeltoodud juhiseid järgides, tehke järgmist:

1. Keelest **IP-aadresside** eespool kuvatud, klõpsake nuppu **staatiline** **IP-aadress ülesande**paremal.
2. Tippige *192.168.1.101* **IP-aadress**ja seejärel klõpsake nuppu **Salvesta**ja seejärel klõpsake nuppu **Jah**.

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [reserveeritud avaliku IP](virtual-networks-reserved-public-ip.md) -aadressid.
- Lisateavet [astme avaliku IP (ILPIP)](virtual-networks-instance-level-public-ip.md) aadresside kohta.
- Konsulteerige [reserveeritud IP REST API-d](https://msdn.microsoft.com/library/azure/dn722420.aspx).