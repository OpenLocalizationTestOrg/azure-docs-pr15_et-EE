## <a name="scenario"></a>Stsenaarium

Parem selgitavad, kuidas luua UDRs, kasutatakse selle dokumendi allpool stsenaariumi.

![PILDI KIRJELDUS](./media/virtual-network-create-udr-scenario-include/figure1.png)

Selle stsenaariumi loote ühe UDR *ette end alamvõrgu* ja muu UDR jaoks *tagasi end alamvõrku* , nagu allpool kirjeldatud: 

- **UDR-FrontEnd**. Esiosa UDR rakendatakse *FrontEnd* alamvõrgu ja sisaldavad ühe marsruutimiseks.  
    - **RouteToBackend**. Selle protsessi saadab kogu liikluse tagasi lõpuks alamvõrgu **FW1** virtual arvutis.
- **UDR-Taustväärtus**. Tagasi lõpuks UDR rakendatakse *Taustväärtus* alamvõrgu ja sisaldavad ühe marsruutimiseks. 
    - **RouteToFrontend**. Selle protsessi saadab kogu liikluse esiosa alamvõrgu **FW1** virtual arvutis.

Lennuliinide kombinatsioon tagab, et kõik määratud üks alamvõrgu teise liikluse suunatakse **FW1** virtuaalse masina, mida kasutatakse virtuaalse seadme. Samuti peate IP ümbersuunamine selle VM, tagamaks, et ta saaks sinna teiste VMs liikluse jaoks sisse lülitada.
