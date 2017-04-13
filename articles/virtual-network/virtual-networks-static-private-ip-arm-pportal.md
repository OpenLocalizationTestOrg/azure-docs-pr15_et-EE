<properties 
   pageTitle="ARM režiimis Azure'i portaalis privaatne staatiline IP seadmise kohta | Microsoft Azure'i"
   description="Privaatne IP-d (DIPs) ja kuidas neid hallata ARM režiimis Azure'i portaalis"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Kuidas määrata staatilise privaatne IP-aadress Azure'i portaalis

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel. Samuti saate [staatilise privaatne IP-aadress klassikaline juurutamise mudeli haldamine](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Valimi alltoodud juhiseid oodatud lihtne keskkond juba loonud. Kui soovite käivitada juhiseid, nagu need on kuvatud selles dokumendis, esmalt koostada testi keskkond, mis on kirjeldatud [on vnet loomine](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Kuidas luua VM testimiseks privaatne staatiline IP-aadressid

Azure portaali kaudu ei saa määrata VM ressursihaldur juurutamise režiimis loomise ajal staatilise privaatne IP-aadress. Peate esmalt looma VM, tehn määrata oma isiklike IP olla staatiline.

Nimega *DNS01* on VNet nimega *TestVNet* *FrontEnd* alamvõrgu VM loomiseks järgige alltoodud juhiseid.

1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake nuppu **Uus** > **arvutada** > **Windows Server 2012 R2 andmekeskuses**, märkate, et loendis **Valige juurutamise mudeli** juba näitab **Ressursihaldur**, ja seejärel klõpsake nuppu **Loo**, nagu on näidatud alloleval joonisel näha.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. **Põhitõed** tera, sisestage soovitud nimi (meie stsenaariumi*DNS01* ), luua VM kohaliku administraatorikonto ja parool, nagu on näidatud alloleval joonisel näha.

    ![Põhitõed blade](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Veenduge, et valitud **asukoha** on *Kesk-USA*, siis **ressursirühm**all nuppu **Vali olemasolev** seejärel **ressursirühm** uuesti nuppu ja seejärel klõpsake *TestRG*ja klõpsake nuppu **OK**.

    ![Põhitõed blade](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. Labale **Valige suurus** valige **A1 Standard**ja seejärel klõpsake nuppu **Vali**.

    ![Valige suurus blade](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. **Sätete** tera, veenduge, et järgmised atribuudid on esitatud allpool väärtustega määratakse, ja seejärel klõpsake nuppu **OK**.

    -**Salvestusruumi konto**: *vnetstorage*
    - **Võrgu**: *TestVNet*
    - **Alamvõrgu**: *FrontEnd*

    ![Valige suurus blade](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. **Kokkuvõte** tera, klõpsake nuppu **OK**. Teade kuvatakse armatuurlaua allpool paan.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Kuidas hankida staatilise privaatne IP-aadressi teave VM

Staatilise privaatne IP-aadressi teavet loodud eespool antud juhiseid VM vaatamiseks käivitada alltoodud juhiseid.

1. Azure'i Azure portaalis, klõpsake nuppu **SIRVI kõik** > **virtuaalmasinates** > **DNS01** > **Kõik sätted** > **võrgu liidesed** ja seejärel klõpsake nuppu ainult võrgu liides loetletud.

    ![Juurutamine VM paan](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. **Võrgu liidese** tera, valige **Kõik sätted** > **IP-aadressid** ja pöörake tähelepanu sellele, väärtused **ülesanne** ja **IP-aadress** .

    ![Juurutamine VM paan](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kuidas lisada mõne olemasoleva VM staatilise privaatne IP-aadress
Lisada staatilisi privaatne IP-aadress VM loodud eeltoodud juhiseid järgides, tehke järgmist:

1. Keelest **IP-aadresside** eespool kuvatud, klõpsake **staatilise** **ülesande**all.
2. Tippige *192.168.1.101* **IP**-aadress ja klõpsake siis nuppu **Salvesta**.

    ![VM Azure portaali loomine](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Kui pärast **salvestamine** märkate, et ülesanne on endiselt määratud **dünaamiline**, see tähendab, et sisestatud IP-aadress on juba kasutusel. Proovige erinevaid IP-aadress.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kuidas eemaldada VM staatilise privaatne IP-aadress
Loodud kohal VM staatilise privaatne IP-aadress eemaldada, tehke selle allpool kirjeldatud juhise.
    
1. Keelest **IP-aadresside** eespool kuvatud, klõpsake **ülesande** **dünaamiliseks** , ja klõpsake siis nuppu **Salvesta**.

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [reserveeritud avaliku IP](virtual-networks-reserved-public-ip.md) -aadressid.
- Lisateavet [astme avaliku IP (ILPIP)](virtual-networks-instance-level-public-ip.md) aadresside kohta.
- Konsulteerige [reserveeritud IP REST API-d](https://msdn.microsoft.com/library/azure/dn722420.aspx).