## <a name="how-to-deploy-with-azure-cli"></a>Azure'i CLI juurutamise

1. Azure'i kontosse sisse logida.

        azure login

  Pärast oma mandaatide käsk tagastab tulemi oma kasutajanimi.

        ...
        info:    login command OK

2. Kui teil on mitu tellimust, sisestage tellimuse id, mida soovite kasutada juurutamiseks.

        azure account set <YourSubscriptionNameOrId>

3. Azure'i ressursihaldur mooduli aktiveerimine

        azure config mode arm

   Saate kinnituse uue režiimi.

        info:     New mode is arm

4. Kui teil pole olemasoleva ressursi rühma, luua uue ressursirühma. Sisestage nimi ja ressursirühm asukohta, et peate oma lahenduse.

        azure group create -n ExampleResourceGroup -l "West US"

   Uue ressursirühma kokkuvõtte tagastatakse.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Saate luua uue ressursirühma juurutamise, käivitage järgmine käsk ja sisestage vajalikud parameetrid. Parameetrid sisaldab juurutamise nimi, olete loonud malli ressursirühma, tee või URL-i nimi ja muud parameetrid, milline olukord teie jaoks vajalik.

   Teil esitada parameetrite väärtused järgmised suvandid.

   - Tekstisisene parameetrite ja kohaliku malli kasutada.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Tekstisisene parameetrite ja lingi malli kasutada.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Kasutage parameetrit faili.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Ressursirühma juurutamisel kuvatakse juurutamise kokkuvõte.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Viimane juurutamise kohta teabe saamiseks.

         azure group log show -l ExampleResourceGroup

7. Juurutamise tõrgete kohta täpsema teabe saamiseks.

         azure group log show -l -v ExampleResourceGroup
