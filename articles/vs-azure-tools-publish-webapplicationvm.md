<properties
   pageTitle="Avaldamine WebApplicationVM | Microsoft Azure'i"
   description="Saate teada, kuidas juurutada veebirakenduse virtuaalse masina. Skript loob vajalikud ressursid Azure tellimuse, kui nad pole olemas."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Avaldamine WebApplicationVM (Windows PowerShelli skripti)

Veebirakenduse virtuaalse masina kasutab. Skripti loob vajalikud ressursid Azure tellimuse, kui nad pole olemas.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfigureerimine

Juurutamise üksikasjade JSON konfiguratsiooni faili tee.

|Pseudonüümid|ükski|
|---|---|
|See on nõutav?|True|
|Paigutus|nimega|
|Vaikeväärtus|ükski|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

### <a name="subscriptionname"></a>SubscriptionName

Azure'i tellimus, kus soovite luua virtuaalse masina nimi.

|Pseudonüümid|ükski|
|---|---|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|Kasutab esimese tellimuse tellimuse faili|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

### <a name="webdeploypackage"></a>WebDeployPackage

Avaldamiseks virtuaalse masina web juurutamise pakett tee. Visual Studio Web avaldamine viisardi abil saate luua selle paketi. Vt [kohta: Web juurutamise paketi loomine Visual Studios](https://msdn.microsoft.com/library/dd465323.aspx).

|Pseudonüümid|ükski|
|---|---|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|ükski|
|Aktsepteerige kohaletoimetamisel sisestatud?|FALSE|
|Aktsepteerige metamärke?|FALSE|

### <a name="allowuntrusted"></a>AllowUntrusted

Kui väärtus on true, lubada pole allkirjastatud usaldusväärse juurorgan serdid.

|Pseudonüümid|ükski|
|---|---|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|FALSE|
|Aktsepteerige kohaletoimetamisel sisestatud?|FALSE|
|Aktsepteerige metamärke?|FALSE|

### <a name="vmpassword"></a>VMPassword

Virtuaalne arvutikonto korral kasutatav mandaat. Näide: - VMPassword @{Name = "admin"; Parooli = "parool"}

|Aliases|ükski|
|---|---|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|ükski|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

SQL Azure'i andmebaasi korral kasutatav mandaat. Näide: - DatabaseServerPassword @{Name = "admin"; Parooli = "parool"}

|Pseudonüümid|ükski|
|---|---|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|ükski|
|Aktsepteerige kohaletoimetamisel sisestatud?|FALSE|
|Aktsepteerige metamärke?|FALSE|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Kui väärtus on true, Prindi sõnumite skripti väljundi voo.

|Pseudonüümid|ükski|
|---|---|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|FALSE|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

## <a name="remarks"></a>Märkused

Täieliku selgituse, kuidas luua skripti abil arendaja ja testige keskkonnas, leiate teemast [Windows PowerShelli skriptide abil avalda arendaja ja testi keskkonnas](vs-azure-tools-publishing-using-powershell-scripts.md).

Konfiguratsioonifail JSON määrab üksikasjad, mis on kasutusele võtta. See sisaldab teavet, näiteks nimi, osaleja rühma ja VHD pildi suurus virtuaalse masina loomisel määratud. Lisaks sisaldab lõpp-punktid virtual arvutis andmebaaside ettevalmistamise, kui mõni ja web juurutamise parameetrid. Järgmine kood kuvatakse näide JSON konfiguratsioonifail:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Saate redigeerida JSON konfiguratsioonifail muutmiseks, mis on ette valmistatud. Virtuaalse masina ja pilveteenus on nõutavad, kuid jaotise andmebaasi pole kohustuslik.
