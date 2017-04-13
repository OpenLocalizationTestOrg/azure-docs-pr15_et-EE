## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Kuidas luua klassikaline VNet Azure'i portaalis

Klassikaline VNet, ülaltoodud stsenaariumi loomiseks järgige alltoodud juhiseid.

1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake nuppu **Uus** > **Networking** > **Virtual võrgu**, märkate, et loendis **Valige juurutamise mudeli** juba näitab **klassikaline**, ja seejärel klõpsake nuppu **Loo**, nagu on näidatud alloleval joonisel näha.

    ![Azure'i portaalis VNet loomine](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Enne **Virtual võrk** , tippige soovitud VNet **nimi** ja klõpsake **aadressiruumi**. Funktsiooni VNet ja selle esimese alamvõrgu oma aadressi ruumi sätete konfigureerimine ja seejärel klõpsake nuppu **OK**. Alloleval joonisel CIDR blokeerimise sätete meie stsenaariumi.

    ![Aadress ruumi blade](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Klõpsake **Ressursirühm** ja valige ressursirühma VNet, et lisada või klõpsake nuppu **Loo uus ressursirühm** on VNet lisada uue ressursirühma. Joonisel on ressurss **TestRG**ehk uue ressursirühma rühma sätted. Ressursi rühmade kohta lisateabe saamiseks külastage [Azure'i ressursihaldur ülevaade](../articles/virtual-network/resource-group-overview.md#resource-groups).

    ![Ressursi rühma blade loomine](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. Vajaduse korral oma VNet **tellimust** ja **asukoha** sätete muutmine. 

6. Kui te ei soovi selle VNet kuvamiseks klõpsake **Startboard**paani, keelake **Startboard Kinnita**. 

7. Klõpsake nuppu **Loo** ja pange tähele nimega **loomise virtuaalse võrgu** , nagu on näidatud alloleval joonisel.

    ![VNet portaali loomine](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Oodake, kuni VNet luua ja kuvamisel paani allpool, klõpsake seda, et lisada rohkem alamvõrku.

    ![VNet portaali loomine](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. Peaksite nägema **konfiguratsiooni** jaoks oma VNet, nagu allpool näidatud. 

    ![VNet portaali loomine](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Klõpsake **alamvõrku** > **lisamine**, sisestage **nimi** ja määrake **aadress vahemikus (CIDR blokeerimine)** jaoks teie alamvõrku ja seejärel klõpsake nuppu **OK**. Alloleval joonisel on meie praeguse stsenaariumi sätted.

    ![Azure'i portaalis VNet loomine](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)