## <a name="service-chaining---transit-through-peered-vnet"></a>Teenuse Chaining - piilus VNet kaudu

Kuigi süsteemi marsruudib kasutamine hõlbustab liikluse automaatselt juurutamiseks, on juhtumeid, kus soovite reguleerida paketid marsruutimine virtuaalse seadme kaudu.
Selle stsenaariumi korral on kaks VNets tellimus, HubVNet ja VNet1, nagu on kirjeldatud alloleval joonisel. Juurutamist võrgu virtuaalse Appliance(NVA) VNet HubVNet sisse. Pärast VNet silmitsemine HubVNet ja VNet1 vahel, saate määrata selle järgmise sõnumihüppe kohta, pärast Dessandi abil soovitud HubVNet ja kasutaja määratletud protsesside.

![Dessandi teel](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Lihtsa, endale kõik VNets siin on sama tellimus. Kuid see töötab ka rist-tellimuse stsenaariumi.

Võtme atribuudi lubamiseks teel marsruutimine on "Luba edasisaatmist liiklus" parameeter. See võimaldab vastu võtmist ja saata selle Dessandi, et liikluse piilus VNet.  
