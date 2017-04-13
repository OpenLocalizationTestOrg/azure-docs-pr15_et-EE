## <a name="download-and-understand-the-arm-template"></a>Laadige alla ja mõista ARM Mall

Saate alla laadida olemasoleva ARM malli loomiseks on VNet ja kahe alamvõrku github, tehke soovitud muudatused, võite soovi ja uuesti kasutada. Selleks tehke järgmist.

1. Liikuge [lehel näidis malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klõpsake **azuredeploy.json**ja seejärel käsku **RAW**.
3. Salvestage fail mõnda kohalikus kaustas teie arvutis.
4. Kui olete tuttav ARM Mallid, jätkake juhisega 7.
5. Avage salvestatud fail ja vaadake jaotises 5 **parameetrite** sisu. Kohatäite ette ARM malli parameetrite väärtused, mida saate täita käigus juurutamist.

    | Parameetri | Kirjeldus |
    |---|---|
    | **asukoht** | Azure'i piirkond, kus on VNet luuakse |
    | **vnetName** | Uue VNet nimi |
    | **addressPrefix** | Aadressiruumi jaoks VNet, CIDR vormingus |
    | **subnet1Name** | Esimese VNet nimi |
    | **subnet1Prefix** | Jaoks esimene alamvõrgu CIDR blokeerimine |
    | **subnet2Name** | Teine VNet nimi |
    | **subnet2Prefix** | Jaoks teise alamvõrgu CIDR blokeerimine |

    >[AZURE.IMPORTANT] ARM Mallid säilitada github saate aja jooksul muutuda. Veenduge, et kontrollida mall enne selle kasutamist.
    
6. Märkige jaotises **ressursse** sisu ja pange tähele järgmist:

    - **Tüüp**. Ressursi on loodud malli tüüp. Sel juhul **Microsoft.Network/virtualNetworks**, mis tähistavad on VNet.
    - **nimi**. Ressursi nimi. Pange tähele, **[parameters('vnetName')]**, mis tähendab, et nimi on esitatud sisendina kasutaja või parameetri faili juurutamisel kasutamine.
    - **Atribuudid**. Ressursi atribuutide loendi. See mall kasutab aadress ruumi ja alamvõrgu atribuutide VNet loomise ajal.

7. Liikuge tagasi [malli lehel näidis](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Klõpsake **azuredeploy-paremeters.json**ja klõpsake **RAW**.
9. Salvestage fail mõnda kohalikus kaustas teie arvutis.
10. Salvestatud fail avada ja redigeerida parameetrite väärtused. Kasutage alltoodud väärtused VNet, mis on kirjeldatud meie stsenaariumi.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Salvestage fail.
  