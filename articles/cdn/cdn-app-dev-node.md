<properties
    pageTitle="Töö alustamine Azure'i CDN SDK Node.js | Microsoft Azure'i"
    description="Saate teada, kuidas kirjutada Node.js rakenduste haldamiseks Azure'i CDN."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Azure'i CDN arengu kasutamise alustamine

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET-I](cdn-app-dev-net.md)

[Azure'i CDN SDK Node.js](https://www.npmjs.com/package/azure-arm-cdn) abil automatiseerida loomist ja haldamist CDN profiilid ja lõpp-punktid.  Selles õppeteemas tutvustatakse loomine lihtne Node.js konsooli rakendus, mis näitab, mitu toimingud.  Selle õpetuse eesmärk ei ole kirjeldada kõigis Azure'i CDN SDK Node.js üksikasjalikult.

Selle õpetuse lõpuleviimiseks teil peaks juba [Node.js](http://www.nodejs.org) **4.x.x** või suurem installinud ja konfigureerinud.  Saate kasutada mis tahes tekstiredaktorit Node.js rakenduse loomiseks.  Selle õpetuse kirjutamiseks saab kasutada [Visual Studio kood](https://code.visualstudio.com).  

> [AZURE.TIP] [Lõpetatud projekti selles õpetuses](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) on saadaval MSDN-i alla laadida.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Projekti loomine ja lisamine NPM sõltuvused

Nüüd, kui oleme loodud ressursirühma meie CDN profiilid ja antud Azure AD rakendusel haldamiseks CDN profiilid ja selles rühmas lõpp-punktid, alustame meie teenuserakenduse loomine.

Looge rakenduse talletamiseks kaust.  Konsooli Node.js tööriistu oma praeguse tee, uue kausta oma praeguse asukoha määramiseks ja projekti lähtestada, täites.
    
    npm init
    
Saate seejärel esitada küsimusi lähtestada oma projekti.  Selle õpetuse **kirje osutage**, kasutab *app.js*.  Saate vaadata oma muud Valikud järgmises näites.

![NPM käivitamise väljund](./media/cdn-app-dev-node/cdn-npm-init.png)

Meie projekti on nüüd lähtestada *packages.json* faili abil.  Meie projekti saab kasutada mõne Azure teeke sisalduvate NPM paketid.  Kasutame Azure'i kliendi käitusaja jaoks Node.js (ms-ülejäänud-azure) ja Azure CDN kliendi teek Node.js (azure-arm-cd).  Vaatame lisada need projekti sõltuvused.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Pärast paketid on valmis installimist *package.json* faili peaks sarnanema selles näites (versioon arvude võivad erineda):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Lõpetuseks, kasutades oma tekstiredaktoris, luua tühja tekstifaili ja salvestage see meie projekti juurkausta nimega *app.js*.  Nüüd olete valmis alustama, koodi kirjutamist.

## <a name="requires-constants-authentication-and-structure"></a>Nõuab, konstantide, autentimise ja struktuuri

*App.js* meie redaktoris avatud, saate vaatame põhistruktuur meie kirjutatud programm.

1. Lisage soovitud "nõuab" meie NPM pakette ülaosas järgmisega.

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Tuleb määratleda mõne konstandid meie meetodid kasutatakse.  Saate lisada järgmist.  Asendage kohatäited, sh selle ** &lt;vahele&gt;**, vastavalt vajadusele oma väärtustega.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Järgmiseks me väärtustada CDN halduse klient ja anda sellele meie mandaat.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Kui kasutate üksiku kasutaja autentimise, näeb need kaks joont veidi teistsugused.

    >[AZURE.IMPORTANT] Kui otsustasite üksikute kasutaja autentimise asemel peamine teenus on kasutada ainult seda proovi kood.  Olge ettevaatlik, et kaitsta teie üksikuid kasutajatunnust ja hoida neid salajane.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Asendage üksuste ** &lt;vahele&gt; ** õige teabega.  Jaoks `<redirect URI>`, kasutage redirect URI Azure AD Rakenduse registreerimisel sisestatud.
    

4.  Meie Node.js konsooli rakendus saab teha mõne käsurea parameetrid.  Vaatame kinnitada, et vähemalt üks parameeter on möödas.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. See toob meile meie programmi, kus oleme hargneda muud funktsioonid, mis põhineb parameetrid võeti vastu põhiosa.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  Meie programmi mitmes kohas läheb vaja veendumaks, et õige arv parameetrite võeti vastu sisse ja mõned spikri kuvamiseks, kui nad ei vaata õige.  Selleks, et funktsioonide loomine.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Lõpuks me kasutame klientarvutis CDN halduse funktsioonide on asünkroonne, nii, et nad vajavad meetodi helistada tagasi, kui nad on valmis.  Proovime ühe, mida saab kuvada väljund CDN halduse klient (vajaduse korral) ja väljuge programmist nõtkelt.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Nüüd, kui on kirjutatud meie programmi põhistruktuur, tuleks loome funktsioonide kutsunud meie parameetrite põhjal.

## <a name="list-cdn-profiles-and-endpoints"></a>Loendi CDN profiilid ja lõpp-punktid

Alustame koodi loendi meie olemasoleva profiilid ja lõpp-punktid.  Minu kood kommentaarid anda oodatud süntaks, et me ei tea, kus iga parameetri läheb.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profiilid ja lõpp-punktid loomine

Järgmiseks me kirjutada funktsioonide loomiseks profiilid ja lõpp-punktid.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Likvideerite lõpp

Eeldades, et lõpp-punkti on loodud, on üks levinud tööülesannet, mille soovime võib teha meie programmi puhastamine sisu meie lõpp-punkti.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profiilid ja lõpp-punktid kustutamine

Me lisada viimase funktsiooni kustutab lõpp-punktid ja profiilid.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Programmi

Saaksime nüüd täita meie Node.js programmi abil meie lemmik siluri või konsooli.

> [AZURE.TIP] Kui Silur on Visual Studio kood, peate läbima käsurea parameetrid keskkonna häälestamine.  Visual Studio kood see **lanuch.json** faili.  Atribuut nimega **argumendid** otsida ja lisada oma parameetrite stringi väärtuse massiivi, et see näeb välja umbes järgmine: `"args": ["list", "profiles"]`.

Alustame loetelu meie profiilid.

![Loendi profiilid](./media/cdn-app-dev-node/cdn-list-profiles.png)

Meil tagasi tühja massiivi.  Kuna me ei ole mis tahes profiilid meie ressursirühm, mis peaks.  Loome nüüd profiil.

![Profiili loomine](./media/cdn-app-dev-node/cdn-create-profile.png)

Nüüd lisame lõpp.

![Lõpp-punkti loomine](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Lõpuks loome meie profiili kustutada.

![Profiili kustutamine](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Järgmised sammud

Kui soovite vaadata lõpetatud projekti jaotistes, [Laadige alla valimi](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74)põhjal.

Teemast Azure CDN SDK viide Node.js, vaadata [viide](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Azure'i SDK Node.js täiendavaid dokumente leida, vaadata [täielik viide](http://azure.github.io/azure-sdk-for-node/).

Saate hallata oma CDN ressursse [PowerShelli abil](./cdn-manage-powershell.md).

