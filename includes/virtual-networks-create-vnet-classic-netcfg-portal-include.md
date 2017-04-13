## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>Kuidas luua VNet, mis on Azure portaali võrgu config faili abil

Azure'i kasutab XML-faili saate määrata kõik VNets saadaval tellimuse. Saate selle faili alla laadida ja seda muuta või kustutada olemasolevate VNets redigeerida ja uusi faile luua. Seda dokumenti saate teada, kuidas faili, nimetatakse network configuration (või netcgf) faili allalaadimiseks ja redigeerige seda uue VNet. Vaadake lisateavet võrgu konfiguratsioonifail [Azure virtuaalse võrgu konfigureerimise skeemi](https://msdn.microsoft.com/library/azure/jj157100.aspx) .

VNet, mis on Azure portaali kaudu netcfg faili loomiseks järgige alltoodud juhiseid.

1. Brauserist, avage http://manage.windowsazure.com ja vajadusel logige sisse oma Azure kontoga.
2. Liikuge kerides allapoole ja klõpsake teenuste loend ja klõpsake **võrkude** nagu allpool näha.

    ![Azure'i](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. Klõpsake lehe allservas nuppu **ekspordi** , nagu allpool näidatud.

    ![Nupp ekspordi](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. Lehel **ekspordi võrgukonfiguratsioon** valige tellimus, mida soovite eksportida virtuaalse võrgu konfigureerimise kaudu ja seejärel klõpsake lehe vasakpoolses allnurgas nuppu märge.
5. Järgige oma brauseri juhiseid **NetworkConfig.xml** faili salvestada. Veenduge, et teie Märkus. Kui salvestate faili.
6. Juhis 5 kohal XML- või teksti redaktori rakenduse abil salvestatud faili avamiseks ja otsige soovitud **<VirtualNetworkSites>** element. Kui teil on juba loodud mis tahes võrgud, iga võrgu kuvatakse eraldi **<VirtualNetworkSite>** element.
7. Selle stsenaariumi kirjeldatud virtuaalse võrgu loomiseks lisage järgmine XML-i alla soovitud **<VirtualNetworkSites>** elementi:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  Võrgu konfigureerimise faili salvestada.
9.  Azure'i portaalis, klõpsake lehe vasakpoolses allnurgas nuppu **Uus**, seejärel **Võrguteenuste**, klõpsake nuppu ja seejärel klõpsake **VIRTUAALSE võrgu**ja seejärel käsku **Impordi konfiguratsioon** , nagu on näidatud alloleval joonisel.

    ![Impordi konfigureerimine](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  Lehel **võrgu konfigureerimise faili importimine** nuppu **SIRVI jaoks faili …**ja seejärel liikuge kausta, mis on salvestatud faili samm 8, valige fail ja klõpsake nuppu **Ava**. Veebilehe peaks sarnanema alloleval joonisel. Klõpsake noolt nupu järgmise juhise juurde liikumiseks klõpsake parempoolses alanurgas lehe.

    ![Võrgu konfiguratsiooni faili lehel importimine](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   Lehel **koostamise võrgu** teade kirje jaoks oma uue VNet, nagu on näidatud alloleval joonisel.

    ![Koostamise lehele võrgu](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Klõpsake lehe loomine on VNet parempoolses allnurgas nuppu märge. Mõne sekundi pärast oma VNet kuvatakse loendis Saadaolevad VNets, nagu on näidatud alloleval joonisel.

    ![Uue virtuaalse võrgu](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)