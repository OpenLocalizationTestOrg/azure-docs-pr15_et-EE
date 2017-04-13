## <a name="scenario"></a>Stsenaarium

Parem selgitavad, kuidas luua NSGs, kasutatakse selle dokumendi allpool stsenaariumi.

![VNet stsenaarium](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Selle stsenaariumi loote iga alamvõrgu **TestVNet** virtuaalse võrgu jaoks ka NSG, nagu allpool kirjeldatud: 

- **NSG-FrontEnd**. Esiosa NSG rakendatakse *FrontEnd* alamvõrgu ja kahe reeglid.  
    - **rdp-reegel**. See reegel võimaldab RDP-liikluse *FrontEnd* alamvõrgu.
    - **veebi-reegel**. See reegel võimaldab HTTP-liikluse *FrontEnd* alamvõrgu.
- **NSG-Taustväärtus**. Tagasi lõpuks NSG rakendatakse *Taustväärtus* alamvõrgu ja kahe reeglid. 
    - **sql-reegel**. See reegel võimaldab SQL-i liikluse *FrontEnd* alamvõrgu ülaservast.
    - **veebi-reegel**. Selle reegliga saab kõik Interneti seotud liikluse *Taustväärtus* alamvõrgu kaudu.

Saate luua reegleid kombinatsiooni DMZ-nagu stsenaarium, kus tagasi lõpuks alamvõrgu saab ainult saadud liiklust SQL esiosa alamvõrgu ja ei pääseks Interneti-ühendus, ajal esiosa alamvõrgu saate Internetiga ja vastu võtta sissetuleva HTTP päringuid.
 
