## <a name="how-to-deploy-with-powershell"></a>Juurutamise PowerShelli abil

1. Azure'i kontosse sisse logida.

          Add-AzureAccount

   Pärast oma mandaatide käsk tagastab oma konto teavet.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Kui teil on mitu tellimust, sisestage tellimuse id, mida soovite kasutada juurutamiseks. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Aktiveerige Azure'i ressursihaldur mooduli.

          Switch-AzureMode AzureResourceManager

4. Kui teil pole olemasoleva ressursi rühma, luua uue ressursirühma. Sisestage nimi ja ressursirühm asukohta, et peate oma lahenduse.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Uue ressursirühma kokkuvõtte tagastatakse.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Uue ressursirühma juurutamise loomiseks käsu **New-AzureResourceGroupDeployment** ja sisestage vajalikud parameetrid. Parameetrid sisaldab juurutamise nimi, olete loonud malli ressursirühma, tee või URL-i nimi ja muud parameetrid, milline olukord teie jaoks vajalik. 
   
   Teil esitada parameetrite väärtused järgmised suvandid. 
   
   - Tekstisisene parameetrite kasutamine.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Kasutage parameetrit objekti.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Parameetri faili abil.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Ressursirühma juurutamisel kuvatakse juurutamise kokkuvõte.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Juurutamise tõrgete kohta teabe saamiseks.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Juurutamise tõrgete kohta täpsema teabe saamiseks.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
