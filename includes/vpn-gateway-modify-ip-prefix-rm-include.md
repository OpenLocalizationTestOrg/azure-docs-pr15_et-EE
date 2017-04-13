### <a name="noconnection"></a>Eesliidete lisamine või eemaldamine – pole ühenduse lüüs

- **Lisada** täiendavad aadress eesliiteid kohalikule võrgu lüüsi, mille olete loonud, kuid mis ei veel ühenduse lüüs, kasutada järgmises näites on. Kindlasti muuta väärtused ise.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Eemaldada** mõne aadress eesliide kohtvõrgu Gateway, mis ei sisalda VPN-ühendus, kasutage järgmises näites. Jätke eesliidete tähised, mida te enam ei vaja. Selles näites me pole enam vaja eesliide 20.0.0.0/24 (alates eelmises näites), nii, et uuendame kohaliku võrgu lüüsi ja selle eesliite välistada.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Kuidas lisada või eemaldada eesliiteid - olemasoleva ühenduse lüüs

Kui olete loonud ühenduse lüüs ja soovite lisada või eemaldada sisalduvate teie kohalikus võrgus lüüsi IP address eesliiteid, peate tegema järgmist järjestuses. Seetõttu mõned tööseisakute VPN-ühenduse. Oma eesliiteid värskendamisel tuleb esmalt ühenduse eemaldamiseks, muuta eesliiteid ja seejärel looge uus ühendus. Allpool toodud näidetes, ärge unustage muuta väärtused ise.

>[AZURE.IMPORTANT] Ärge kustutage VPN-lüüsi. Kui te seda teete, siis on teil minge tagasi see uuesti luua, kui ka konfigureerida kohapealse ruuterisse uute sätete abil toodud juhised.
 
1. Klõpsake ühenduse eemaldamiseks.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Muuda aadressi eesliiteid teie kohalikus võrgus Gateway.

    Määrata muutuja jaoks soovitud LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Muutke eesliiteid.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Ühenduse loomine. Selles näites me konfigureerite soovitud IPsec ühenduse tüüpi. Kui ühendust uuesti luua, kasutada määratud ühendusetüübi konfiguratsioonist. Täiendavad ühenduse tüüpi, lugege teemat [PowerShelli cmdlet-käsk](https://msdn.microsoft.com/library/mt603611.aspx) lehele.

    Määrata muutuja jaoks soovitud VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Ühenduse loomine. Pange tähele, et see näide kasutab muutuja $local eelmises etapis määratavad.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
