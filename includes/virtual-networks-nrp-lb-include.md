## <a name="load-balancer"></a>Laadi koormusetasakaalustusteenuse
Laadi koormusetasakaalustusteenuse kasutatakse, kui soovite mastaapimiseks rakenduste. Tüüpilised juurutamise stsenaariumide kaasata VM mitmes eksemplaris töötavad rakendused. Laadi koormusetasakaalustusteenuse, mille abil saate levitada võrguliiklust eri eksemplarides, on esiküljega VM juhtudel. 

![NIC ühe VM](./media/resource-groups-networking/figure8.png)

| Atribuut | Kirjeldus |
|---|---|
| *frontendIPConfigurations* | Laadi koormusetasakaalustusteenuse saate lisada ühe või mitme esiosa IP-aadressid, muidu tuntud virtuaalse IP-d (VIP). IP-aadressid olla sissepääsu liikluse jaoks ja võib olla avaliku IP- või privaatne IP |
|*backendAddressPools* | need on seostatud VM NICs, mille laadimine jagatakse IP-aadressid |
|*loadBalancingRules* | reegli atribuudi kaardid antud esiosa IP ja port kombinatsiooni kogumisse tagasi end IP-aadresside ja portide kombinatsioon. Laadi koormusetasakaalustusteenuse ressursi ühe määratlemine, saate määratleda mitme laadi tasakaalustamiseks reeglid, iga reegli puhul, mis kajastab kombinatsiooni ees lõpetamine IP ja port ja Lõpeta IP ja port seostatud virtuaalmasinates tagasi. Reegel on üks port esiosa kogumi palju virtuaalmasinates tagasi end kogumi abil |  
| *Sondid* | sondid võimaldavad teil silma peal seisundi VM juhtudel. Kui soovitud seisundi juures nurjub, virtuaalse masina eksemplari võeti pööre automaatselt |
| *inboundNatRules* | NAT reeglite määratlemise Sissetuleva liikluse läbib ees lõpetada IP ja jaotatud tagasi end IP teatud virtuaalse masina eksemplari. Reegli NAT on üks port esiosa kogumi ühe virtual arvutisse tagasi end pargis | 

Laadi koormusetasakaalustusteenuse malli Json-vormingus näide:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "dnsNameforLBIP": {
          "type": "string",
          "metadata": {
            "description": "Unique DNS name"
          }
        },
        "location": {
          "type": "string",
          "allowedValues": [
            "East US",
            "West US",
            "West Europe",
            "East Asia",
            "Southeast Asia"
          ],
          "metadata": {
            "description": "Location to deploy"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address Prefix"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/24",
          "metadata": {
            "description": "Subnet Prefix"
          }
        },
        "publicIPAddressType": {
          "type": "string",
          "defaultValue": "Dynamic",
          "allowedValues": [
            "Dynamic",
            "Static"
          ],
          "metadata": {
            "description": "Public IP type"
          }
        }
      },
      "variables": {
        "virtualNetworkName": "virtualNetwork1",
        "publicIPAddressName": "publicIp1",
        "subnetName": "subnet1",
        "loadBalancerName": "loadBalancer1",
        "nicName": "networkInterface1",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "nicId": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "backEndIPConfigID": "[concat(variables('nicId'),'/ipConfigurations/ipconfig1')]"
      },
      "resources": [
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[parameters('location')]",
      "properties": {
        "publicIPAllocationMethod": "[parameters('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsNameforLBIP')]"
        }
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[parameters('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnetRef')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(variables('lbID'), '/backendAddressPools/LoadBalancerBackend')]"
                }
              ],
              "loadBalancerInboundNatRules": [
                {
                  "id": "[concat(variables('lbID'),'/inboundNatRules/RDP')]"
                }
              ]
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[variables('publicIPAddressID')]"
              }
            }
          }
        ],
        "backendAddressPools": [
          {
            "name": "loadBalancerBackEnd"
          }
        ],
        "inboundNatRules": [
          {
            "name": "RDP",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPort": 3389,
              "backendPort": 3389,
              "enableFloatingIP": false
            }
          }
        ]
      }
    }
      ]
    }

### <a name="additional-resources"></a>Lisaressursid

Lugege lisateavet [laadimine koormusetasakaalustusteenuse REST API -ga](https://msdn.microsoft.com/library/azure/mt163651.aspx) .
