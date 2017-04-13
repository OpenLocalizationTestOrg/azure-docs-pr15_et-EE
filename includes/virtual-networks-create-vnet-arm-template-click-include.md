## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>ARM malli abil klõpsake juurutamine

Saate uuesti kasutada eelmääratletud ARM Mallid üles github hoidlasse, mida haldab Microsoft ja avada, et ühenduse. Neid malle saab juurutatud otse välja github, või alla laadida ja muuta vastavalt oma vajadustele. Juurutada Mall, mis on VNet loob kaks alamvõrku, järgige alltoodud juhiseid.

1. Brauserist, avage [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Liikuge kerides allapoole mallide loendi ja nuppu **101 vnet-kahe alamvõrku**. Kontrollige **README.md** faili, nagu allpool näidatud.

    ![READEME.md faili github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klõpsake **Azure juurutamine**. Vajadusel sisestage Azure sisselogimisteave. 
4. **Parameetrite** tera, sisestage soovitud väärtused, mida soovite kasutada oma uue VNet loomiseks ja seejärel klõpsake nuppu **OK**. Alloleval joonisel on meie stsenaarium väärtused.

    ![ARM malli parameetrid](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. Klõpsake **ressursirühm** ja valida ressursirühma VNet, et lisada või klõpsake nuppu **Loo uus** on VNet lisada uue ressursirühma. Joonisel on ressurss **TestRG**ehk uue ressursirühma rühma sätted.

    ![Ressursirühm](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. Vajaduse korral oma VNet **tellimust** ja **asukoha** sätete muutmine.
6. Kui te ei soovi selle VNet kuvamiseks klõpsake **Startboard**paani, keelake **Startboard Kinnita**.
5. Klõpsake käsku **terminite Leagl**, kasutustingimustega ja klõpsake nuppu **osta** nõus. 
6. Klõpsake nuppu **Loo** on VNet loomiseks.

    ![Esitamise juurutamise paani Eelvaade portaalis](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Kui juurutamise on lõpule jõudnud, klõpsake **TestVNet** > **Kõik sätted** > **alamvõrku** alamvõrgu atribuudid kuvamiseks, nagu allpool näidatud.

    ![VNet eelvaade portaali loomine](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)