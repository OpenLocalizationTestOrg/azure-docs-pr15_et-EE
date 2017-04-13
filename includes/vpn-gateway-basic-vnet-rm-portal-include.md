Azure portaali abil luua mõne VNet ressursihaldur juurutamise mudeli, järgige alltoodud juhiseid. Pildid on toodud näited. Ärge unustage asendage väärtused ise. Virtuaalne võrkude töötamise kohta leiate lisateavet teemast [Virtuaalse võrgu ülevaade](../articles/virtual-network/virtual-networks-overview.md).

1. Brauserist, liikuge [Azure portaali](http://portal.azure.com) ja vajadusel logige sisse oma Azure kontoga.

2. Klõpsake nuppu **Uus**. Tippige väljale **Otsing kuvatakse Marketplace'ist** "Virtuaalse võrgu". Otsige üles **Virtuaalse võrgu** tagastatud loendist ja **Virtuaalse võrgu** tera avamiseks klõpsake seda.

    ![Otsige üles virtuaalse võrgu ressursi blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Leidke virtuaalse võrgu ressursi blade")

3. Virtuaalse võrgu labale loendist **Valige juurutamise mudeli** allosas valige **Ressursihaldur**ja seejärel klõpsake nuppu **Loo**.


    ![Valige ressursihaldur] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Valige ressursihaldur")

4. **Loo virtuaalse võrgu** enne, VNet sätete konfigureerimine. Kui täidate väljad, muutuvad punane hüüumärk roheline märge väljale sisestatud märke on lubatud.

    ![Välja valideerimisreegli] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Välja valideerimisreegli")

5. **Loo virtuaalse võrgu** tera näeb välja umbes järgmine näide. Võib olla väärtusi, mis on automaatselt sisestatud. Kui jah, asendage väärtused ise.

    ![Loo virtuaalse võrgu blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Loo virtuaalse võrgu blade")

6. **Nimi**: Sisestage virtuaalse võrgu nimi.

7. **Aadressiruumi**: aadress tühikut. Kui teil on mitu aadressi tühikud lisada, lisage oma esimese aadressiruumi. Saate lisada täiendavad aadress tühikute hiljem VNet selle loomise järel.
 
8. **Alamvõrgu nimi**: lisage alamvõrgu nime ja alamvõrgu aadress vahemikus. Saate lisada täiendavad alamvõrku hiljem VNet selle loomise järel.

10. **Tellimus**: Veenduge, et see tellimus, mis on loetletud oleks õige. Saate muuta tellimuste drop-down abil.

11. **Ressursirühm**: valige ressursi olemasolevasse rühma või luua uue tippides uue ressursi rühma nime. Kui loote uue rühma, nime ressursirühm vastavalt oma kavandatud väärtused. Ressursi rühmade kohta lisateabe saamiseks külastage [Azure'i ressursihaldur ülevaade](resource-group-overview.md#resource-groups).

12. **Asukoht**: valige asukoht oma VNet. Asukoht määrab, kus ressursse, mis selles VNet juurutamiseks hakkavad asuma.

13. Valige **armatuurlaud PIN-koodi** , kui soovite saama oma VNet armatuurlaual hõlpsalt leida, ja seejärel klõpsake nuppu **Loo**.
    
    ![PIN-koodi armatuurlaud] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "PIN-koodi armatuurlaud")

14. Pärast nupu **Loo**, kuvatakse paan armatuurlauale, mis kajastab oma VNet edenemist. Paan on VNet loomisel.

    ![Paani virtuaalse võrgu loomine] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Paani virtuaalse võrgu loomine")