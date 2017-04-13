<properties
   pageTitle="Avaldamine WebApplicationWebSite (Windows PowerShelli skripti) | Microsoft Azure'i"
   description="Saate teada, kuidas Azure'i veebisait web projekti avaldada. Skript loob vajalikud ressursid Azure tellimuse, kui nad pole olemas."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Avaldamine WebApplicationWebSite (Windows PowerShelli skripti)

##<a name="syntax"></a>Süntaks

Avaldab web projekti Azure veebisait. Skripti loob vajalikud ressursid Azure tellimuse, kui nad pole olemas.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfigureerimine

Juurutamise üksikasjade JSON konfiguratsiooni faili tee.

|Parameetri|Vaikeväärtus|
|---|---|
|Pseudonüümid|ükski|
|See on nõutav?|True|
|Paigutus|nimega|
|Vaikeväärtus|ükski|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

## <a name="subscriptionname"></a>SubscriptionName

Azure'i tellimus, mida soovite luua veebilehe nimi.

|Parameetri|Vaikeväärtus|
|---|---|
|Aliases|ükski|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|ükski|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

## <a name="webdeploypackage"></a>WebDeployPackage

Veebisaidi avaldamiseks web juurutamise pakett tee. Visual Studio Web avaldamine viisardi abil saate luua selle paketi. Lisateavet leiate teemast [Azure pilveteenused ja ASP.net-i kasutamise alustamine](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Parameetri|Vaikeväärtus|
|---|---|
|Pseudonüümid|ükski|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|ükski|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Kasutajanime ja parooli Azure SQL-andmebaasi.

|Parameetri|Vaikeväärtus|
|---|---|
|Pseudonüümid|ükski|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|ükski|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Kui väärtus on true, Prindi sõnumite skripti väljundi voo.

|Parameetri|Vaikeväärtus|
|---|---|
|Pseudonüümid|ükski|
|See on nõutav?|FALSE|
|Paigutus|nimega|
|Vaikeväärtus|FALSE|
|Müügivõimaluste sisendit vastu?|FALSE|
|Aktsepteerige metamärke?|FALSE|

## <a name="remarks"></a>Märkused

Täieliku selgituse, kuidas luua skripti abil arendaja ja testige keskkonnas, leiate teemast [Windows PowerShelli skriptide abil avalda arendaja ja testi keskkonnas](vs-azure-tools-publishing-using-powershell-scripts.md).

Konfiguratsioonifail JSON määrab üksikasjad, mis on kasutusele võtta. See sisaldab teavet, mis on määratud, näiteks nimi ja kasutajanimi veebisaidi loomisel. See hõlmab ka andmebaasi, et näha, kui see on olemas. Järgmine kood kuvatakse näide JSON konfiguratsioonifail:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Saate redigeerida JSON konfiguratsioonifail muutmiseks, mis on juurutatud. Jaotises veebisait on nõutavad, kuid andmebaasi jaotis on valikuline.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Avalda-WebApplicationVM (Windows PowerShelli skripti)](vs-azure-tools-publish-webapplicationvm.md)
