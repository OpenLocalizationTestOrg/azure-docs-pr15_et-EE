## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>Kuidas luua mõne VNet võrgu config faili PowerShelli abil

Azure'i kasutab XML-faili saate määrata kõik VNets saadaval tellimuse. Saate selle faili alla laadida ja seda muuta või kustutada olemasolevate VNets redigeerida ja uusi faile luua. Seda dokumenti saate teada, kuidas faili, nimetatakse network configuration (või netcgf) faili allalaadimiseks ja redigeerige seda uue VNet. Vaadake lisateavet võrgu konfiguratsioonifail [Azure virtuaalse võrgu konfigureerimise skeemi](https://msdn.microsoft.com/library/azure/jj157100.aspx) .

On VNet abil PowerShelli kaudu netcfg faili loomiseks järgige alltoodud juhiseid.

1. Kui te pole kunagi varem Azure PowerShelli, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../articles/powershell-install-configure.md) ja järgige juhiseid kõik viis selleks Azure'i sisse logida ja valige oma tellimuse.
2. Azure'i PowerShelli konsooli, kasutada **Get-AzureVnetConfig** cmdlet allpool käsu võrgu konfigureerimise faili alla laadida. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Oodatav väljund:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Avage fail, mille salvestasite 2. juhise alusel kohal XML- või teksti redaktori rakenduse abil ja otsige soovitud **<VirtualNetworkSites>** element. Kui teil on juba loodud mis tahes võrgud, iga võrgu kuvatakse eraldi **<VirtualNetworkSite>** element.
4. Selle stsenaariumi kirjeldatud virtuaalse võrgu loomiseks lisage järgmine XML-i alla soovitud **<VirtualNetworkSites>** elementi:

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

9.  Võrgu konfigureerimise faili salvestada.
10. Azure'i PowerShelli konsooli, kasutage cmdlet **Set-AzureVnetConfig** allpool käsu võrgu konfigureerimise faili üleslaadimiseks. Pange tähele väljund käsu, kuvatakse jaotises **OperationStatus** **õnnestus** . Kui see on ei ole, märkige ruut XML-failis tõrkeid.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Siin on oodatud väljundi ülaltoodud käsk:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Azure'i PowerShelli konsooli, kasutada **Get-AzureVnetSite** cmdlet veenduge, et uue võrgu on lisatud käsu allpool. 

        Get-AzureVNetSite -VNetName TestVNet

    Siin on oodatud väljundi ülaltoodud käsk:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded