## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Kuidas luua mõne VNet Azure'i portaalis

Mõne VNet põhjal stsenaarium kohale, Azure eelvaade portaalis loomiseks järgige alltoodud juhiseid.

1. Brauserist, avage http://portal.azure.com ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake nuppu **Uus** > **Networking** > **Virtual võrgu**, valige loendist **Valige juurutamise mudeli** **Ressursihaldur** ja seejärel klõpsake nuppu **Loo**, nagu on näidatud alloleval joonisel näha.

    ![Azure'i portaalis VNet loomine](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. **Loo virtuaalse võrgu** enne, VNet sätteid konfigureerida nagu on näidatud alloleval joonisel.

    ![Virtuaalse võrgu blade loomine](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Klõpsake **ressursirühm** ja valida ressursirühma VNet, et lisada või klõpsake nuppu **Loo uus** on VNet lisada uue ressursirühma. Joonisel on ressurss **TestRG**ehk uue ressursirühma rühma sätted. Ressursi rühmade kohta lisateabe saamiseks külastage [Azure'i ressursihaldur ülevaade](../articles/resource-group-overview.md#resource-groups).

    ![Ressursirühm](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. Vajaduse korral oma VNet **tellimust** ja **asukoha** sätete muutmine. 

6. Kui te ei soovi selle VNet kuvamiseks klõpsake **Startboard**paani, keelake **Startboard Kinnita**. 

7. Klõpsake nuppu **Loo** ja pange tähele nimega **loomise virtuaalse võrgu** , nagu on näidatud alloleval joonisel.

    ![Virtuaalne paanil loomine](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Oodake, kuni VNet luua ja seejärel **virtuaalne võrgu** tera, valige **Kõik sätted** > **alamvõrku** > **Lisa** nagu allpool näha.

    ![Azure'i portaalis alamvõrgu lisamine](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Määrake alamvõrgu sätete *Taustväärtus* alamvõrku, nagu allpool näidatud ja seejärel klõpsake nuppu **OK**. 

    ![Alamvõrgu sätted](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. Pange tähele alamvõrku, loendi, nagu on näidatud alloleval joonisel.

    ![Alamvõrku VNet loend](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
