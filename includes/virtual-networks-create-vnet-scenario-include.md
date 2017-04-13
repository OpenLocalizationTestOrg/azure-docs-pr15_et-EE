## <a name="scenario"></a>Stsenaarium

Paremaks illustreerimiseks on VNet ja alamvõrku loomise kohta, kasutatakse selle dokumendi allpool stsenaariumi.

![VNet stsenaarium](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Selle stsenaariumi loomist on VNet nimega **TestVNet** reserveeritud CIDR plokk **192.168.0.0./16**abil. Teie VNet sisaldab järgmist alamvõrku: 

- **FrontEnd**, kasutades oma CIDR Blokeeri **192.168.1.0/24** .
- **Taustväärtus**, kasutades **192.168.2.0/24** oma CIDR plokk.

 