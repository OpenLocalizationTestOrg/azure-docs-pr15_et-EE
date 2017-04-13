### <a name="app-service-plan"></a>Rakenduse teenusleping

Loob teenusleping hosting web appi. Esitate lepingu kaudu **hostingPlanName** parameetri nimi. Leping asukoht on kasutatud ressursirühma samasse asukohta. Hinnakirjad taseme- ja töötaja maht on määratud **sku** ja **workerSize** parameetrid

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

