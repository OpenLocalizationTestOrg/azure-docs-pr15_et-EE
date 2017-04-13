<properties 
    pageTitle="Käivitamine ja peatamine virtuaalmasinates Azure automatiseerimine - PowerShelli töövoo | Microsoft Azure'i"
    description="Azure'i automaatika stsenaarium, mis hõlmab tegevusraamatud käivitamine ja peatamine klassikaline virtuaalmasinates graafiline versioon."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure'i automaatika stsenaarium - käivitamine ja peatamine virtuaalmasinates

Selle stsenaariumi Azure automatiseerimine sisaldab tegevusraamatud käivitamine ja peatamine klassikaline virtuaalmasinates.  Saate selle stsenaariumi jaoks ühte järgmistest:  

- Kasutage oma keskkonnas tegevusraamatud muutmata kujul. 
- Muutke tegevusraamatud kohandatud funktsionaalsust sooritamiseks.  
- Kõne on tegevusraamatud teise käitusjuhendi raames üldise lahenduse. 
- Kasutage funktsiooni tegevusraamatud õpetused teavet käitusjuhendi loome põhimõtet. 

> [AZURE.SELECTOR]
- [Graafilise](automation-solution-startstopvm-graphical.md)
- [PowerShelli töövoo](automation-solution-startstopvm-psworkflow.md)

See on selle stsenaariumi PowerShelli töövoo käitusjuhendi versiooni. See on saadaval [graafiline tegevusraamatud](automation-solution-startstopvm-graphical.md)abil.

## <a name="getting-the-scenario"></a>Saada stsenaarium

Selle stsenaariumi koosneb kahest PowerShelli töövoo tegevusraamatud, järgmiste linkide kaudu saate alla laadida.  Vt [graafilist versiooni](automation-solution-startstopvm-graphical.md) selle stsenaariumi graafiline tegevusraamatud linke.

| Käitusjuhendi | Link | Tüüp | Kirjeldus |
|:---|:---|:---|:---|
| Algus-AzureVMs | [Azure'i klassikaline VMs käivitamine](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShelli töövoo | Käivitab kõik klassikaline virtuaalmasinates on Azure subscriptionor kõik virtuaalmasinates konkreetse teenuse nimega. |
| Lõpeta – AzureVMs | [Azure'i klassikaline VMs peatamine](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShelli töövoo | Lõpetab kõik virtuaalmasinates automatiseerimise kontol või kõigi virtuaalmasinates konkreetse teenuse nimega.  |


## <a name="installing-and-configuring-the-scenario"></a>Installimine ja konfigureerimine stsenaarium

### <a name="1-install-the-runbooks"></a>1. selle tegevusraamatud installimine

Pärast soovitud tegevusraamatud allalaadimist, saate need importimine [on Käitusjuhendi](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook)protseduuri abil importida.

### <a name="2-review-the-description-and-requirements"></a>2 läbi vaadata, kirjeldus ja nõuded
Funktsiooni tegevusraamatud sisaldavad kommenteeritud abi tekst, mis sisaldab kirjeldus ja nõutav varasid.  Samuti saate sama teavet sellest artiklist. 

### <a name="3-configure-assets"></a>3. varad konfigureerimine
Funktsiooni tegevusraamatud vaja järgmised varad, mida peate looma ja asustada väärtused.

| Varade tüüp | Vara nimi | Kirjeldus |
|:---|:---|:---|:---|
| Mandaadi | AzureCredential | Sisaldab kontoga, millel on õigus käivitamine ja peatamine tellimuse Azure'i virtuaalmasinates mandaat.  Teise võimalusena saate määrata mõne muu mandaadi varade **mandaati** parameetri **Lisamine – AzureAccount** tegevuse. |
| Muutuja | AzureSubscriptionId | Sisaldab Azure tellimuse Tellimuse ID-d. |

## <a name="using-the-scenario"></a>Selle stsenaariumi abil

### <a name="parameters"></a>Parameetrid

Iga tegevusraamatud on järgmiste parameetrite abil.  Peate sisestama mõni kohustuslik parameetrite väärtused ja soovi korral pakuvad vastavalt oma vajadustele muude parameetrite väärtused.

| Parameetri | Tüüp | Kohustusliku | Kirjeldus |
|:---|:---|:---|:---|
| Teenuse nimi | string | Ei | Kui väärtus pole, on kõik virtuaalmasinates teenuse nime alustamine või on peatatud.  Kui väärtus puudub, on kõik klassikaline virtuaalmasinates Azure tellimuse alustamine või peatada. |
| AzureSubscriptionIdAssetName | string | Ei | Sisaldab [muutuv varade](#installing-and-configuring-the-scenario) sisaldab tellimuse ID-d Azure tellimuse nime.  Kui väärtus pole määratud, kasutatakse *AzureSubscriptionId* .  |
| AzureCredentialAssetName | string | Ei | Sisaldab nime [mandaati varade](#installing-and-configuring-the-scenario) sisaldava käitusjuhendi kasutamise korral kasutatav mandaat.  Kui väärtus pole määratud, kasutatakse *AzureCredential* .  |

### <a name="starting-the-runbooks"></a>Funktsiooni tegevusraamatud käivitamine

Saate meetodite käivitamiseks [on käitusjuhendi Azure'i automaatika](automation-starting-a-runbook.md) alustamiseks kas selle tegevusraamatud selle stsenaariumi.

Valimi järgmised käsud kasutab Windows PowerShelli **StartAzureVMs** kõik virtuaalmasinates alustada teenuse nimi *MyVMService*käivitamiseks.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Väljund

Funktsiooni tegevusraamatud on [väljund sõnumi](automation-runbook-output-and-messages.md) iga virtuaalse masina, mis näitab, kas alustada või peatada käsustikuga õnnestus.  Saate vaadata kindla stringi väljund tulemi jaoks iga käitusjuhendi määramiseks.  Järgmises tabelis on loetletud võimalike väljundi stringid.

| Käitusjuhendi | Tingimus | Sõnumi |
|:---|:---|:---|
| Algus-AzureVMs | Virtuaalse masina töötab juba  | MyVM töötab juba |
| Algus-AzureVMs | Taotluse esitatud virtuaalse masina jaoks | MyVM pole alustatud |
| Algus-AzureVMs | Alusta kutse virtuaalse masina nurjus.  | MyVM käivitamine nurjus |
| Lõpeta – AzureVMs | Virtuaalne arvutisse on juba peatatud  | MyVM on juba peatatud |
| Lõpeta – AzureVMs | Kutse virtuaalse masina esitatud peatamine | MyVM on peatatud |
| Lõpeta – AzureVMs | Peata kutse virtuaalse masina nurjus.  | MyVM peatamine nurjus |

Näiteks järgmised koodilõigu kaudu on käitusjuhendi proovib kõik virtuaalmasinates alustada teenuse nimi *MyServiceName*.  Kui mõni start taotlused fail, seejärel tõrge toimingud võib võtta. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Üksikasjalik jaotus

Järgmisena kirjeldame üksikasjalik tegevusraamatud selle stsenaariumi.  Saate selle teabe kohandamine soovitud tegevusraamatud või lihtsalt jaoks loome oma automatiseerimise stsenaariume õppida.

### <a name="parameters"></a>Parameetrid

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

Töövoog käivitub õigete väärtuste [sisendparameetrid](#using-the-scenario).  Kui vara nimed ei kasutatakse vaikenimed.

### <a name="output"></a>Väljund

    # Returns strings with status messages
    [OutputType([String])]

Selle rea teatab, et käitusjuhendi väljund on string.  See ei ole vajalik, kuid on hea tava, kui käitusjuhendi kasutatakse on [lapse käitusjuhendi](automation-child-runbooks.md) nii, et ema käitusjuhendi teate oodata väljundi tüüp.

### <a name="authentication"></a>Autentimine

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Saate määrata järgmised read [identimisteabe](automation-configuring.md#configuring-authentication-to-azure-resources) ja Azure'i tellimus, mida kasutatakse käitusjuhendi ülejäänud.
Esmalt kasutame **Get-AutomationPSCredential** saada juurdepääsu käivitamine ja peatamine tellimuse Azure'i virtuaalmasinates identimisteabe malliga vara. **Lisa-AzureAccount** kasutab seda vara mandaadi määramiseks.  Väljund on määratud fiktiivne muutuja nii, et see ei sisalda käitusjuhendi väljund.  

Muutuv varade Tellimuse ID-ga tuuakse seejärel **Get-AutomationVariable** ning **Valige-AzureSubscription**seadistatud tellimus.

### <a name="get-vms"></a>Saada VMs

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** kasutatakse virtuaalmasinates, käitusjuhendi töötavad koos toomiseks.  Kui väärtus on esitatud **teenuse nimi** Sisestuskeel muutuja, laaditakse ainult virtuaalmasinates teenuse nime.  Kui **teenuse nimi** on tühi, laaditakse kõik virtuaalmasinates.

### <a name="startstop-virtual-machines-and-send-output"></a>Start/Stop virtuaalmasinates ja saatke väljund

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Joonte järgmiseks virtual iga seadme kaudu.  Esmalt **toide** virtuaalse masina on märgitud, et näha, kas see töötab juba või käitusjuhendi sõltuvalt sellest, mis on peatatud.  Kui see on juba target olekus, siis sõnum saadetakse väljundi ja käitusjuhendi lõpetatakse.  Kui ei, siis **Algus-AzureVM** või **Peata-AzureVM** kasutatakse proovib alustamine või lõpetamine virtuaalse masina talletatud muutuja taotluse tulemiga.  Sõnum saadetakse siis väljund määrata, kas taotluse alustamine või lõpetamine õnnestus.


## <a name="next-steps"></a>Järgmised sammud

- Lapse tegevusraamatud töötamise kohta leiate lisateavet teemast [lapse tegevusraamatud Azure'i automaatika](automation-child-runbooks.md) 
- Lisateavet väljundi sõnumite käitusjuhendi täitmine ja tõrkeotsingu logimine, vt [Käitusjuhendi väljundi- ja sõnumite Azure automatiseerimine](automation-runbook-output-and-messages.md)
