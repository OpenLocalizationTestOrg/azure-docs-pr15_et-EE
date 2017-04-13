Lüüsi IP-aadressi muutmiseks kasutage funktsiooni `New-AzureRmVirtualNetworkGatewayConnection` cmdlet-käsk. Kui hoiate kohtvõrgu lüüsi nimi täpselt samamoodi nagu olemasoleva nime, kirjutab sätted. Sel ajal, "Seatud" cmdlet ei toeta muutmine gateway IP-aadress.

### <a name="gwipnoconnection"></a>Kuidas muuta gateway IP-aadress - pole ühenduse lüüs

Värskendage oma kohalikus võrgus lüüsi, mis ei ole veel ühenduse lüüs IP-aadress, kasutage järgmises näites. Saate värskendada ka aadressi eesliiteid, samal ajal. Määratud sätteid kirjutab olemasolevad sätted. Kindlasti kasutage oma kohtvõrgu lüüsi olemasoleva nime. Kui te ei tee, mille loote uue kohaliku võrgu lüüsi, pole ülekirjutamise olemasolev fail.

Järgmises näites, asendades selle väärtused ise kasutada.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Kuidas muuta gateway IP-aadress – olemasoleva ühenduse lüüs

Kui ühenduse lüüs juba olemas, peate esmalt ühenduse eemaldamiseks. Seejärel saate muuta gateway IP-aadress ja looge uus ühendus. Seetõttu mõned tööseisakute VPN-ühenduse.


>[AZURE.IMPORTANT] Ärge kustutage VPN-lüüsi. Kui te seda teete, siis on teil minge tagasi see uuesti luua, kui ka taaskonfigureerimine asutusesisese marsruuteri IP-aadressiga vastloodud lüüsiga määratud toodud juhised.
 

1. Klõpsake ühenduse eemaldamiseks. Leiate ühenduse nimi, kasutades funktsiooni `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet-käsk.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Muutke GatewayIpAddress väärtus. Saate muuta oma aadressi eesliiteid sel ajal, vajaduse korral. Pange tähele, et see kirjutab kohalikus võrgus olemasoleva lüüsi sätete. Kasutage olemasoleva teie kohalikus võrgus lüüsi nimi, et kirjutab sätete muutmine. Kui te ei tee, mille loote uue kohaliku võrgu lüüsi, mitte muuta olemasolevat.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Ühenduse loomine. Selles näites me konfigureerite soovitud IPsec ühenduse tüüpi. Kui ühendust uuesti luua, kasutada määratud ühendusetüübi konfiguratsioonist. Täiendavad ühenduse tüüpi, lugege teemat [PowerShelli cmdlet-käsk](https://msdn.microsoft.com/library/mt603611.aspx) lehele.  Saada VirtualNetworkGateway nime, saate käivitada soovitud `Get-AzureRmVirtualNetworkGateway` cmdlet-käsk.

    Muutujate seadmiseks tehke järgmist.

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Ühenduse loomiseks tehke järgmist.
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

