### <a name="to-verify-your-connection-by-using-powershell"></a>Kinnitamiseks ühendust PowerShelli abil

Saate kontrollida, et teie ühendus õnnestus, kasutades funktsiooni `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet koos või ilma `-Debug`. 

1. Kasutage järgmine cmdlet-käsk näide konfigureerimine vastavaks oma väärtused. Kui kuvatakse vastav viip, valige "A" käivitamiseks "Kõik". Näites `-Name` viitab nimi ühendus, mida olete loonud ja soovite testida.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Kui cmdlet on lõpule jõudnud, saate vaadata väärtused. Alltoodud näites ühenduse olekuna kuvatakse "Ühendatud" ja te näete sissepääsu ja sealt baiti.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Kinnitamiseks ühendust Azure portaali kaudu

Azure'i portaalis saate ühenduse oleku vaatamiseks ühenduse liikumine. On selleks mitu võimalust. Järgmised toimingud kuvada üks viis liikuge ühendust ja veenduge, et.

1. [Azure'i portaal](http://portal.azure.com), klõpsake **ressurssidele** ja liikuge oma virtuaalse võrgu lüüsi.
2. Enne virtuaalse võrgu lüüsi jaoks, klõpsake nuppu **ühendused**. Saate vaadata iga ühenduse olekut.
3. Klõpsake nime ühendus, mida soovite kontrollida **Essentialsi**avamiseks. Essentials, saate vaadata ühenduse kohta lisateabe saamiseks. **Olek** on "Õnnestus" ja "Ühendatud", kui olete teinud edukat ühendust.

    ![Kontrollige ühendust](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)