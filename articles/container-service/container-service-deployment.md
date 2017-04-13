<properties
   pageTitle="Azure'i teenus on kobar juurutamine | Microsoft Azure'i"
   description="Azure'i teenus on kobar juurutada Azure portaali Azure'i CLI või PowerShelli abil."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Keskmise suurusega, ümbriste, mikro-teenuste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Azure'i teenus on kobar juurutamine

Azure'i teenus pakub kiire kasutuselevõtu populaarsed avatud lähtekoodi container rühmitamise ja korraldamise lahendusi. Azure'i teenus abil saate juurutada AV/OS ja keskmise suurusega sülem kogumite Azure'i ressursihaldur malle või Azure portaali. Nimetatud juurutada Azure virtuaalse masina skaala komplektid ja rühmad ära Azure võrgunduse ja salvestusruumi pakkumisi. Azure'i teenus, peate tellimuse Azure. Kui teil pole ühte, siis saate kasutajaks [tasuta prooviversioon](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Selle dokumendi juhatab teid läbi mõne Azure'i teenus kobar juurutamine [Azure portaali](#creating-a-service-using-the-azure-portal), [Azure käsurea liides (CLI)](#creating-a-service-using-the-azure-cli)ja [Azure PowerShelli mooduli](#creating-a-service-using-powershell)abil.  

## <a name="create-a-service-by-using-the-azure-portal"></a>Teenuse abil Azure portaali loomine

Azure portaali sisse logida, valige **Uus**ja Azure'i turuplatsi otsida **Azure'i teenus**.

![Juurutamise 1 loomine](media/acs-portal1.png)  <br />

Valige **Azure'i teenus**, ja klõpsake nuppu **Loo**.

![Juurutamise 2 loomine](media/acs-portal2.png)  <br />

Sisestage järgmine teave:

- **Kasutajanimi**: see on kasutaja nimi, mida kasutatakse iga soovitud virtuaalmasinates kontot ja virtuaalse masina skaala seab kobar Azure'i teenus.
- **Tellimus**: valige Azure tellimuse.
- **Ressursirühm**: valige ressursi olemasolevasse rühma või looge uus.
- **Asukoht**: valige Azure piirkond juurutamiseks Azure'i teenus.
- **SSH avalik võti**: lisa teenus Azure'i virtuaalmasinates vastu autentimiseks kasutatud avalik võti. On väga oluline selles võtmes pole reapiirid ja see sisaldab 'ssh-rsa' eesliite ja 'username@domain' postfix. See peaks välja nägema umbes järgmine: **ssh-rsa AAAAB3Nz … <> …... UcyupgH azureuser@linuxvm **. Secure Shell (SSH) klahvid loomise juhised leiate kohta leiate artiklitest [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) ja [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) .

Kui olete valmis jätkama, klõpsake nuppu **OK** .

![Juurutamise 3 loomine](media/acs-portal3.png)  <br />

Valige korraldamise tüüp. Valikud on järgmised:

- **Näiteks Põhiliselt/OS**: näiteks Põhiliselt/OS kobar kasutajaks.
- **Sülem**: keskmise suurusega sülem kobar kasutajaks.

Kui olete valmis jätkama, klõpsake nuppu **OK** .

![Juurutamise 4 loomine](media/acs-portal4.png)  <br />

Sisestage järgmine teave:

- **Juhtslaidi loendamiseks**: meistrid klaster arv.
- **Agent count**: jaoks keskmise suurusega sülem, see olla algse arvu agentide agent skaala määramine. Näiteks Põhiliselt/OS, saab algse arvu agentide kogumi privaatne skaala. Lisaks luuakse avaliku skaala kogum, mis sisaldab agentide kindlaksmääratud arvust. Määratakse agentide selle avaliku skaala komplekti arv, kui palju meistrid on loodud kobar - ühe avaliku agent ühe juhtslaidi ja kaks avalikku agentide, kolme või viie meistrid.
- **Agent virtuaalse masina suurus**: agent virtuaalmasinates suurust.
- **DNS-i eesliide**: maailma kordumatu nimi, mis kasutab eesliide põhiosadele täielikult kvalifitseeritud domeeninimede teenuse.

Kui olete valmis jätkama, klõpsake nuppu **OK** .

![Juurutamise 5 loomine](media/acs-portal5.png)  <br />

Kui teenus valideerimine on lõpule jõudnud, klõpsake nuppu **OK** .

![Juurutamise 6 loomine](media/acs-portal6.png)  <br />

Klõpsake nuppu **Loo** juurutamise alustamiseks.

![Juurutamise 7 loomine](media/acs-portal7.png)  <br />

Kui olete otsustanud kinnitamine juurutamise Azure portaali, saate vaadata juurutamise olek.

![Juurutamise 8 loomine](media/acs-portal8.png)  <br />

Kui juurutamise on lõpule viidud, klaster Azure'i teenus on kasutamiseks valmis.

## <a name="create-a-service-by-using-the-azure-cli"></a>Azure'i CLI abil luua teenus

Käsurea abil luua eksemplari Azure'i teenus, peate tellimuse Azure. Kui teil pole ühte, siis saate kasutajaks [tasuta prooviversioon](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Samuti peate olete [installinud](../xplat-cli-install.md) ja [konfigureerinud](../xplat-cli-connect.md) Azure'i CLI.

Näiteks Põhiliselt/OS või keskmise suurusega sülem kobar juurutada, valige üks järgmised Mallid GitHub. Pange tähele, et mallidest on samad, välja arvatud orchestrator vaikeväärtuse.

* [Näiteks Põhiliselt/OS Mall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Sülem Mall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Seejärel veenduge, et Azure'i CLI on Azure tellimuse ühendus. Järgmise käsu abil saate seda teha.

```bash
azure account show
```
Kui Azure'i konto ei tagastata, kasutage järgmine käsk CLI sisse logida Azure.

```bash
azure login -u user@domain.com
```

Järgmiseks konfigureerida Azure CLI tööriistu kasutada Azure ressursihaldur.

```bash
azure config mode arm
```

Azure ressursirühm ja teenus kobar loomine järgmine käsk, kus:

- **RESOURCE_GROUP** on ressursirühm, mida soovite kasutada selle teenuse nimi.
- Azure'i piirkond, kus luuakse ressursirühm ja juurutamise Azure'i teenus on **koht** .
- **TEMPLATE_URI** on juurutamise faili asukoht. Pange tähele, et see peab olema töötlemata fail, mitte kursori GitHub UI. Seda URL-i otsimiseks valige azuredeploy.json faili GitHub, ja klõpsake nuppu **Raw** .

> [AZURE.NOTE] Kui käivitate selle käsu, shell teilt juurutamise parameetrite väärtused.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Sisestage malli parameetrid

Selle versiooni käsk nõuab teilt interaktiivseks parameetrite määratlemine. Kui soovite sisestada parameetreid, näiteks JSON-vormingus stringi, saate seda teha, kasutades funktsiooni `-p` aktiveerimine. Näiteks:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Teise võimalusena saate sisestada parameetrite JSON-vormingus faili, kasutades funktsiooni `-e` vahetamine:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Näiteks parameetrid faili nimega `azuredeploy.parameters.json`, otsige see GitHub Azure'i teenus mallid.

## <a name="create-a-service-by-using-powershell"></a>Looge teenuse PowerShelli abil

Saate juurutada mõne Azure'i teenus kobar PowerShelli abil. See dokument põhineb versiooni 1.0 [Azure PowerShelli mooduli](https://azure.microsoft.com/blog/azps-1-0/).

Näiteks Põhiliselt/OS või keskmise suurusega sülem kobar juurutada, valige üks järgmised mallid. Pange tähele, et mallidest on samad, välja arvatud orchestrator vaikeväärtuse.

* [Näiteks Põhiliselt/OS Mall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Sülem Mall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Enne luua klaster teie Azure'i tellimus, veenduge, et teie PowerShelli seanss on juba sisse loginud Azure. Saate seda teha on `Get-AzureRMSubscription` käsk:

```powershell
Get-AzureRmSubscription
```

Kui peate sisse logima Azure, kasutage funktsiooni `Login-AzureRMAccount` käsk:

```powershell
Login-AzureRmAccount
```

Kui te olete uue ressursirühma kasutusele võtavad, peate esmalt looma ressursirühma. Uue ressursirühma loomiseks kasutage funktsiooni `New-AzureRmResourceGroup` käsk ja määrake ressursi rühma nimi ja sihtkoht piirkond:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Kui olete loonud ressursirühma, saate luua klaster järgmise käsu abil. Täpsustatakse URI soovitud malli jaoks soovitud `-TemplateUri` parameeter. Kui käivitate selle käsu, PowerShelli teilt juurutamise parameetrite väärtused.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Sisestage malli parameetrid

Kui olete tuttav PowerShelli, teate, et liikuda läbi cmdlet-käsu jaoks saadaolevate parameetrite tipite miinusmärk (-) ja seejärel vajutage tabeldusklahvi (TAB). See sama funktsioon töötab ka määratlete malli parameetrid. Kui tipite malli nimi, cmdlet toob Mall, sõelub parameetrid ja lisab malli parameetrid käsu dünaamiliselt. See on väga lihtne Määrake malli parameetrite väärtused. Ja kui parameeter value, PowerShelli küsib teilt väärtus.

Allpool on täielik käsk sisalduv parameetritega. Saate sisestada oma väärtuste ressursside nimesid.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui teil on toimiva kobar, leiate need dokumendid ühenduse loomise ja haldamise üksikasjad.

- [Ühenduse loomine mõne Azure'i teenus kobar](container-service-connect.md)
- [Azure'i teenus ja AV/OS töötamine](container-service-mesos-marathon-rest.md)
- [Azure'i teenus ja keskmise suurusega sülem töötamine](container-service-docker-swarm.md)
