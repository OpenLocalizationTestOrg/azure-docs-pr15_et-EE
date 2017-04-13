<properties 
   pageTitle="Kuidas luua NSGs ARM režiimis Azure'i portaalis | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua ja juurutada NSGs rühmas Azure'i portaalis"
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

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Kuidas hallata NSGs Azure'i portaalis

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel. Samuti saate [luua NSGs klassikaline juurutamise mudeli](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

PowerShelli käskude allpool oodata on lihtne keskkond, mis on juba loodud valimi põhjal ülaltoodud stsenaariumi. Kui soovite käivitada käske, nagu need on kuvatud selles dokumendis, esmalt koostada testi keskkonna juurutamine [selle malli](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd)abil, käsku **Deploy Azure**, asendada vaikimisi parameetrite väärtused vajadusel ja järgige juhiseid teemas portaali. Alltoodud juhiseid kasutada **RG-NSG** mall on juurutatud ressursirühma nime.

## <a name="create-the-nsg-frontend-nsg"></a>NSG-FrontEnd NSG loomine

Luua **NSG-FrontEnd** NSG, nagu on näidatud ülaltoodud stsenaarium, järgige alltoodud juhiseid.

1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake **sirvimine >** > **võrgu turberühmad**.

    ![Azure'i portaal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. **Võrgu turberühmad** tera, klõpsake nuppu **Lisa**.
  
    ![Azure'i portaal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. **Turberühma loomine võrgu** tera, luua mõne NSG Töölehefunktsioon *NSG-FrontEnd* *RG-NSG* ressursirühm, ja klõpsake nuppu **Loo**.

    ![Azure'i portaal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Mõne olemasoleva NSG reeglite loomine

Mõne olemasoleva NSG Azure portaalist reeglite loomiseks järgige alltoodud juhiseid.

2. Klõpsake **sirvimine >** > **võrgu turberühmad**.

3. Klõpsake loendis NSGs **NSG-FrontEnd** > **turvalisuse sissetulevad reeglid**

    ![Azure'i portaal - NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. **Sissetuleva meili Turve reeglite**loendi, klõpsake nuppu **Lisa**.

    ![Azure'i portaal - reegli lisamine](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. **Lisa sissetuleva meili Turve reegel** labale nimega *web-reegli* prioriteet *200* lubada *TCP* kaudu juurdepääsu pordi *80* mis tahes VM kõigist allikaist reegli loomine ja klõpsake nuppu **OK**. Pange tähele, et enamik need sätted on vaikeväärtused juba.

    ![Azure'i portaal - reeglisätete lähtestamine](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Mõne sekundi pärast kuvatakse soovitud NSG uus reegel.

    ![Azure'i portaal - uus reegel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Korrake samme 6 nimega *rdp-reegli* prioriteedi *250* lubada juurdepääsu *TCP* kaudu port *3389* mis tahes VM kõigist allikaist sissetuleva reegli loomiseks.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Seostada NSG FrontEnd alamvõrgu

1. Klõpsake **sirvimine >** > **Ressursirühma** > **RG-NSG**.
2. Klõpsake **RG-NSG** labale **…**  >  **TestVNet**.

    ![Azure'i portaal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Klõpsake **sätete** labale **alamvõrku** > **FrontEnd** > **võrgu turberühma** > **NSG-FrontEnd**.

    ![Azure'i portaal - alamvõrgu sätted](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. **FrontEnd** tera, klõpsake nuppu **Salvesta**.

    ![Azure'i portaal - alamvõrgu sätted](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>NSG-kirjutamata NSG loomine

Looge **NSG-kirjutamata** NSG ja seostada **kirjutamata** alamvõrku, järgige alltoodud juhiseid.

1. Korrake juhiseid [loomine NSG-FrontEnd NSG](#Create-the-NSG-FrontEnd-NSG) on NSG nimega *NSG-Taustväärtus* loomiseks
2. Korrake [mõne olemasoleva NSG loomine reeglite](#Create-rules-in-an-existing-NSG) loomiseks **sissetulevad** reeglid järgmises tabelis toodud juhised.

  	|Sissetulev reegel|Väljaminev reegel|
  	|---|---|
  	|![Azure'i portaal - Sissetulev reegel](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure'i portaal - Väljaminev reegel](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Korrake juhiseid [seostada NSG FrontEnd alamvõrgu](#Associate-the-NSG-to-the-FrontEnd-subnet) seostamiseks **NSG-kirjutamata** NSG **kirjutamata** alamvõrku.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [hallata olemasoleva NSGs](virtual-network-manage-nsg-arm-portal.md)
- [Logimise](virtual-network-nsg-manage-log.md) NSGs.