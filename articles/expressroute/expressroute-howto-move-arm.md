<properties
   pageTitle="Liikumine ExpressRoute topoloogia klassikalises ressursside halduri | Microsoft Azure'i"
   description="Sellel lehel kirjeldatakse, kuidas klassikaline ringi teisaldamiseks ressursihaldur juurutamise mudel."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Klassikaline ExpressRoute topoloogia teisaldamiseks ressursihaldur juurutamise mudeli

## <a name="configuration-prerequisites"></a>Konfiguratsiooni eeltingimused

- Teil on vaja Azure PowerShelli moodulid uusim versioon (vähemalt versiooni 1.0).
- Veenduge, et teil vaadata [eeltingimused](expressroute-prerequisites.md), [marsruutimine nõuded](expressroute-routing.md)ja [töövoogude](expressroute-workflows.md) enne alustamist konfigureerimine.
- Läbi enne eelneva täpsemaks, teave, mis on esitatud jaotise [teisaldamine mõnda ExpressRoute ringi klassikalises ressursside haldur](expressroute-move.md). Veenduge, et te aru saanud piirangud ja kitsendused, mis on võimalik.
- Kui soovite mõne Azure'i ExpressRoute ringi klassikaline juurutamise mudeli liikumiseks Azure'i ressursihaldur juurutamise mudeli, peab teil olema täielikult seadistatud ja töökorras klassikaline juurutamise mudeli ringi.
- Veenduge, et teil on loodud ressursihaldur juurutamise mudeli ressursirühma.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Ressursihaldur juurutamise mudeli ExpressRoute ringi liikumine

Mõne ExpressRoute ringi peab liikuda ressursihaldur juurutamise mudeli nii, et saate seda kasutada nii klassikaline ja ressursihaldur juurutamise mudelid. Saate seda teha, käitades järgmine PowerShelli käske.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Samm 1: Klassikaline juurutamise mudeli ringi üksikasjad kogumine

Koguge teavet oma ExpressRoute ringi esmalt peate.

Azure'i klassikaline keskkonnas sisse logida ja kogumine teenuse võti. Järgmine PowerShelli koodilõigu abil saate teabe kogumine:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Kopeerige **teenuse klahv** topoloogia, mida soovite teisaldada ressursihaldur juurutamise mudel.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Samm 2: Logige sisse ressursihaldur keskkonna ja ressursside uue rühma loomine

Järgmised koodilõigu abil saate luua uue ressursirühma:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Samuti saate olemasoleva ressursi rühma, kui teil on juba üks.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Samm 3: Ressursihaldur juurutamise mudeli ExpressRoute ringi liikumine

Nüüd olete valmis ressursihaldur juurutamise mudeli klassikaline üle oma ExpressRoute ringi liikuda. Läbi vaadata enne täiendavate jaotise [teisaldamine mõnda ExpressRoute ringi: klassikaline ressursihaldur juurutamise mudeli](expressroute-move.md) teave.

Saate seda teha, kui järgmised koodilõigu töötab:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Pärast teisaldamist on lõpule jõudnud, kasutatakse uus nimi, mis on loetletud eelmise cmdlet lahendada. Ringi on põhiosas ümber nimetada.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Mõne ExpressRoute ringi nii juurutamise mudelite lubamine

Enne juurdepääsu juurutamise mudeli kontrollimine peab teie ExpressRoute ringi liikuda ressursihaldur juurutamise mudel.

Käivitage järgmine cmdlet-käsk nii juurutamise mudelite juurdepääsu lubamiseks.

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Kui see toiming on lõpule jõudnud edukalt, on võimalik vaadata ringi klassikaline juurutamise mudeli.

Käivitage ExpressRoute topoloogia üksikasjade vaatamiseks järgmist:

    get-azurededicatedcircuit

Peate olema loetletud teenuse võti näeksid. Nüüd saate hallata oma standard klassikaline juurutamise mudeli käske klassikaline VNets ja oma standard ARM käskude kasutamise ARM VNETs ExpressRoute ringi lingid. Järgmised artiklid juhendab teid haldamine ExpressRoute ringi lingid:

- [Oma ExpressRoute ringi ressursihaldur juurutamise mudeli virtuaalse võrgu linkimine](expressroute-howto-linkvnet-arm.md)
- [Oma ExpressRoute ringi klassikaline juurutamise mudeli virtuaalse võrgu linkimine](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Klassikaline juurutamise mudeli ExpressRoute ringi keelamine

Käivitage järgmine cmdlet-käsk juurdepääsu klassikaline juurutamise mudeli:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Kui see toiming on lõpule jõudnud edukalt, ei saa vaadata ringi klassikaline juurutamise mudeli.

## <a name="next-steps"></a>Järgmised sammud

Kui olete loonud oma ringi, veenduge, et teha järgmist:

- [Luua ja muuta oma ExpressRoute ringi marsruutimine](expressroute-howto-routing-arm.md)
- [Oma ExpressRoute ringi virtuaalse võrgu linkimine](expressroute-howto-linkvnet-arm.md)
