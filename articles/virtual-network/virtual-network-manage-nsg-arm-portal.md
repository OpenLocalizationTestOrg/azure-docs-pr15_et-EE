<properties 
   pageTitle="NSGs abil eelvaade portaali sisse ressursihaldur haldamine | Microsoft Azure'i"
   description="Saate teada, kuidas hallata exising NSGs abil eelvaade portaali sisse ressursihaldur"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>Eelvaate portaalis NSGs haldamine

> [AZURE.SELECTOR]
- [Portaal](virtual-network-manage-nsg-arm-portal.md)
- [PowerShelli](virtual-network-manage-nsg-arm-ps.md)
- [Azure'i CLI](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Teabe toomiseks

Saate vaadata oma olemasoleva NSGs, toomine reeglid mõne olemasoleva NSG ja teada saada, millised ressursid on NSG on seostatud.

### <a name="view-existing-nsgs"></a>Saate vaadata olemasolevaid NSGs
Kõik olemasolevad NSGs tellimuse vaatamiseks tehke järgmist.

1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake **sirvimine >** > **võrgu turberühmad**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Märkige ruut NSGs loendis **võrgu turberühmad** tera.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Ressursirühm **RG-NSG** NSGs loendi kuvamiseks tehke järgmist. 

1. Klõpsake **Ressursi rühmad >** > **RG-NSG** > **…**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Otsige ressursid loendis üksuste kuvamise NSG ikooni, nagu on näidatud allpool tera **ressursid** .

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>Kõik reeglite loendi jaoks soovitud NSG

Mõne NSG nimega **NSG-FrontEnd**reegleid vaatamiseks tehke järgmist. 

1. **Võrgu turberühmad** tera või **ressursse** tera eespool kuvatud, klõpsake nuppu **NSG-FrontEnd**.
2. Klõpsake vahekaardil **sätted** **turvalisuse sissetulevad reeglid**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. **Sissetulev turvalisuse reeglid** tera kuvatakse, nagu allpool näidatud.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Klõpsake vahekaardil **sätted** **Väljamineva meili Turve reeglite** kuvamiseks väljaminevad reegleid.

>[AZURE.NOTE] Vaikereeglit kuvamiseks ikooni **vaikereeglit** ülaosas blade, kus on kuvatud reegleid.

### <a name="view-nsgs-associations"></a>Vaate NSGs ühendused

Ressursid on **NSG-FrontEnd** NSG seostada vaatamiseks tehke järgmist.

1. **Võrgu turberühmad** tera või **ressursse** tera eespool kuvatud, klõpsake nuppu **NSG-FrontEnd**.
2. Klõpsake vahekaardil **sätted** **alamvõrku** alamvõrku, mis on seotud selle NSG kuvamiseks.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Klõpsake vahekaardil **sätted** kuvamiseks, mis on seotud selle NSG NICs **võrgu liidesed** .

## <a name="manage-rules"></a>Reeglite haldamiseks

Saate lisada mõne olemasoleva NSG reegleid, olemasolevad reeglite redigeerimiseks ja Eemalda reeglid.

### <a name="add-a-rule"></a>Reegli lisamine

Lubada **Sissetulev** liiklus pordi **443** kaudu mis tahes seadme abil **NSG-FrontEnd** NSG reegli lisamiseks järgige alltoodud juhiseid.

1. **Võrgu turberühmad** tera või **ressursse** tera eespool kuvatud, klõpsake nuppu **NSG-FrontEnd**.
2. Klõpsake vahekaardil **sätted** **turvalisuse sissetulevad reeglid**.
3. Blade **Sissetulev turvalisuse reeglid** nuppu **Lisa**. Seejärel **Lisa sissetuleva meili Turve reegel** tera, sisestage soovitud väärtused, nagu allpool näidatud ja seejärel klõpsake nuppu **OK**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Mõne sekundi, Pange tähele **turvalisuse sissetulevad reeglid** tera uus reegel.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Reegli muutmine

Sissetuleva liikluse lubamiseks **Internet** ainult kohal loodud reegli muutmiseks tehke järgmist.

1. **Võrgu turberühmad** tera või **ressursse** tera eespool kuvatud, klõpsake nuppu **NSG-FrontEnd**.
2. Klõpsake vahekaardil **sätted** kohal loodud reegel.
3. **Luba https** labale muutmine **Allika** atribuut, nagu allpool näidatud ja klõpsake siis nuppu **Salvesta**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Reegli kustutamine

Ülaltoodud loodud reegli kustutamiseks järgige alltoodud juhiseid.

1. **Võrgu turberühmad** tera või **ressursse** tera eespool kuvatud, klõpsake nuppu **NSG-FrontEnd**.
2. Klõpsake vahekaardil **sätted** kohal loodud reegel.
3. **Luba https** tera, klõpsake käsku **Kustuta**ja seejärel klõpsake nuppu **Jah**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Ühenduste haldamine

Saate seostada ka NSG, et alamvõrku ja NICs. Samuti saate lagunevad mõne NSG mis tahes on seotud ressursside kaudu.

### <a name="associate-an-nsg-to-a-nic"></a>Seostada ka NSG on NIC

Seostada **NSG-FrontEnd** NSG **TestNICWeb1** NIC abil, järgige alltoodud juhiseid.

1. **Võrgu turberühmad** tera või **ressursse** tera eespool kuvatud, klõpsake nuppu **NSG-FrontEnd**.
2. Klõpsake vahekaardil **sätted** **võrgu liidesed** > **seostada** > **TestNICWeb1**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Lagunevad mõne NSG Võrguadapter:

Vaadelda eraldi **NSG-FrontEnd** NSG **TestNICWeb1** NIC kaudu, järgige alltoodud juhiseid.

1. Azure'i portaalis, klõpsake **Ressursi rühmad >** > **RG-NSG** > **…**  >  **TestNICWeb1**.
2. **TestNICWeb1** tera, klõpsake nuppu **Muuda turvalisus...**  > **None**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] Saate selle tera NIC, et kõik olemasolevad NSG siduda.

### <a name="dissociate-an-nsg-from-a-subnet"></a>Lagunevad mõne NSG on alamvõrgu kaudu

Vaadelda eraldi **NSG-FrontEnd** NSG **FrontEnd** alamvõrgu kaudu, järgige alltoodud juhiseid.

1. Azure'i portaalis, klõpsake **Ressursi rühmad >** > **RG-NSG** > **…**  >  **TestVNet**.
2. Klõpsake **sätete** labale **alamvõrku** > **FrontEnd** > **võrgu turberühma** > **pole**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. **FrontEnd** tera, klõpsake nuppu **Salvesta**.

![Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Mõne NSG on alamvõrgu seostada.

Seostada **NSG-FrontEnd** NSG **FronEnd** alamvõrgu uuesti, järgige alltoodud juhiseid.

1. Azure'i portaalis, klõpsake **Ressursi rühmad >** > **RG-NSG** > **…**  >  **TestVNet**.
2. Klõpsake **sätete** labale **alamvõrku** > **FrontEnd** > **võrgu turberühma** > **NSG-FrontEnd**.
3. **FrontEnd** tera, klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Mõne NSG saate seostada ka mõne alamvõrgu Laroo NSG **sätted** tera kaudu.

## <a name="delete-an-nsg"></a>Mõne NSG kustutamine

Mõne NSG saate kustutada ainult siis, kui see on seotud, mis tahes ressursi. Mõne NSG kustutamiseks järgige alltoodud juhiseid.

1. Azure'i portaalis, klõpsake **Ressursi rühmad >** > **RG-NSG** > **…**  >  **NSG-FrontEnd**.
2. Klõpsake **sätete** labale **võrgu liidesed**.
3. Kui määratud on mõni NICs loetletud, klõpsake NIC ja järgige [Dissociate on NSG kaudu Võrguadapter](#Dissociate-an-NSG-from-a-NIC)samm 2.
4. Korrake juhist 3 iga NIC.
5. Klõpsake **sätete** labale **alamvõrku**.
6. Kui määratud on mõni alamvõrku loendis, klõpsake alamvõrgu ja järgige juhiseid 2 ja 3 [Dissociate on NSG on alamvõrgu kaudu](#Dissociate-an-NSG-from-a-subnet).
7. Kerib vasakule **NSG-FrontEnd** tera, seejärel klõpsake **Kustuta** > **Jah**.

[Azure'i portaal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Järgmised sammud

- [Logimise](virtual-network-nsg-manage-log.md) NSGs.
