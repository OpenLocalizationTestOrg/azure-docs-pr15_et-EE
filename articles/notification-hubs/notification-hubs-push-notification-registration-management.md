<properties
    pageTitle="Registreerimise haldamine"
    description="See teema selgitab, kuidas seadmed registreerida teatis jaoturi tõuketeatised teatiste saamiseks."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Registreerimise haldamine

##<a name="overview"></a>Ülevaade

See teema selgitab, kuidas seadmed registreerida teatis jaoturi tõuketeatised teatiste saamiseks. Teema kirjeldab registreerimise kõrge, siis tutvustab kahe põhilise mustrite registreerusite seadmed: otse, et teate jaoturi seadme registreerimine ja registreerimine on kirjutamata rakenduse kaudu. 


##<a name="what-is-device-registration"></a>Mis on seadme registreerimine

Seadme registreerimine teate jaoturi abil saab teha **registreerimise** või **Installi**abil.

#### <a name="registrations"></a>Registreerimine
Registreerimise seostab platvormi teatise teenuse (PNS) pidet seadme jaoks siltide ja võib-olla malli. PNS pidet võib olla ChannelURI, seadme luba või GCM registreerimise id. Siltide kasutatakse õige komplekti seadme pidemete teatised marsruutimiseks. Lisateabe saamiseks vt [marsruutimine ja sildi avaldised](notification-hubs-tags-segment-push-message.md). Mallide kasutatakse rakendamiseks registreerimise kohta teisendus. Lisateavet leiate teemast [Mallid](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Installid
Installi on täiustatud registreerimine, mis sisaldab tõuketeatised koti seotud atribuudid. See on uusim ja parim lähenemine oma seadmed registreerida. Aga seda ei toeta kliendipoolse .NET SDK ([Teatis jaoturi SDK taustväärtus toimingute](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) veel.  See tähendab, et kui kliendi seade ise, oleksite toetab installimist [Teatis jaoturi REST API](https://msdn.microsoft.com/library/mt621153.aspx) lähenemisviisi abil. Taustväärtus teenuste kasutamisel peaks saama kasutada [Teatis jaoturi SDK taustväärtus-analüüsitoiminguid](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Võtme seadmete kasutamise eelised on järgmised:

* Loomine ja värskendamine installi on täielikult idempotent. Nii saate selle uuesti ilma dubleeritud registreerimise muret.
* Installi mudeli abil on lihtne teha üksikute sunnib - suunamise konkreetse seadme. Süsteemi sildi **"$InstallationId: [installationId]"** lisatakse automaatselt iga installi vastavalt registreerimine. Nii saate helistada on see silti, et suunata kindla seadme ilma teha täiendavaid kodeerimine saada.
* Seadmete kasutamise abil saate teha osalise registreerimine värskendused. Osalise värskenduse installi, on nõutav [JSON-paik standard](https://tools.ietf.org/html/rfc6902)paik meetod. See on eriti kasulik, kui soovite värskendada registreerimise sildid. Teil pole kogu registreerimise rippmenüü ja seejärel uuesti eelmise sildid uuesti.

Installi võib sisaldada soovitud järgmised atribuudid. Installimise täieliku loendi atribuudid teemast, [loomine või kirjuta installi REST API-ga](https://msdn.microsoft.com/library/azure/mt621153.aspx) või [Installi atribuudid](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) on.

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

See on oluline Pange tähele, et registreerimise ja installide vaikimisi enam aegumise.

Registreerimise ja installide peab sisaldama iga seadme/kanali kehtiv PNS pidet. Kuna PNS pidemete vaid saab kliendi rakenduse seadmes, on ühe mustri registreerida otse kliendi rakenduse seadmes. Teisalt, turvakaalutlused ja siltide seotud äriloogika teilt rakenduse tagaandmebaas seadme registreerimise haldamine. 

#### <a name="templates"></a>Mallid

Kui soovite kasutada [malle](notification-hubs-templates-cross-platform-push-messages.md), seadme installimise pidada ka kõik Mallid, mis on seotud seadmes on JSON vormindada (vt ülal valimi). Malli nimed aitavad target erinevaid malle samasse seadmesse.

Pange tähele, et iga malli nimi kaardid malli sisu ja siltide komplekt. Lisaks saate iga platvormi on täiendavad vormimalli atribuudid. Windowsi poe (abil WNS) ja Windows Phone 8 (kasutades MPNS), saab täiendava kogumi päised osa mall. Puhul APNs-i, saate mõne aegumise atribuudi kummagi konstandi või malli avaldis. Installimise täieliku loendi atribuudid teemat, [loomine või kirjuta ülejäänud installi](https://msdn.microsoft.com/library/azure/mt621153.aspx) .

#### <a name="secondary-tiles-for-windows-store-apps"></a>Windowsi poe rakenduste teisene paanid

Windowsi poe klientrakendustes, teatiste saatmisel teisene paanid on sama, mis see peamine saata. See on toetatud ka installid. Pange tähele, et teisene paanid erinevate ChannelUri, mis teie kliendi rakenduse SDK tegeleb läbipaistev.

SecondaryTiles sõnastiku kasutab sama TileId, mis saab luua SecondaryTiles objekti oma Windowsi poe rakenduses.
Esmane ChannelUri, sarnaselt ChannelUris teisene paanid, saate muuta igal ajal. Selleks, et hoida värskendatud teate jaoturi seadmed, seadme peate värskendama need koos praeguse ChannelUris teisene paanidest.


##<a name="registration-management-from-the-device"></a>Seadme registreerimise juhtimine

Kui seadme registreerimine haldamine kliendi rakenduste kaudu, kuvatakse taustväärtus vastutab ainult teatised. Kliendi rakendused PNS pidemete ajakohastamine ja siltide registreerida. Järgmisel pildil on näidatud selles mustri.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Seadme esmalt toob PNS PNS pidet, siis registreerib teate jaoturi otse. Pärast registreerimise õnnestumise, rakenduse taustväärtus suunamise registrist teatise saata. Teatiste saatmise kohta leiate lisateavet teemast [marsruutimine ja sildi avaldised](notification-hubs-tags-segment-push-message.md).
Pange tähele, et sellisel juhul kasutate ainult kuulata juurdepääsuõigus teie teate jaoturi seadmest. Lisateavet leiate teemast [Turvalisus](notification-hubs-push-notification-security.md).

Seadme registreerimine on lihtsaim viis, kuid see on mõned puudused.
Esimene puudus on, et kliendi rakendus ainult saate värskendada oma siltide, kui rakendus pole aktiivne. Näiteks kui kasutaja on kahe seadme registreerimine seotud sport meeskonnad, kui esimene seade registreerib on täiendavad sildi (nt Seahawks) sildid, teine seade ei saa Seahawks teavitusi teises seadmes rakendus käivitatakse teist korda, kuni. Kui sildid mõjutab mitmes seadmes, siltide haldamine kaudu soovitud kirjutamata üldisemalt on soovitav valik.
Registreerimise juhtimine kliendi rakendusest teine puudus on see, et kuna rakendusi saate hakkeroitu, turvaliseks registreerimise kindlate siltidega nõuab täiendavat abi, nagu on selgitatud jaotises "sildi taseme turvalisus."



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Näide koodi teate jaoturi abil installi seadmest registreerima 

Sel ajal, see on toetatud ainult [Teatis jaoturi REST API](https://msdn.microsoft.com/library/mt621153.aspx)abil.

Samuti saate paik meetodit kasutades [JSON-paik standard](https://tools.ietf.org/html/rfc6902) installi värskendamine.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Näide koodi registreerima teate jaoturi seadmest, kasutades registreerimine


Nende meetodite loomine või värskendamine neid nimetatakse seadme registreerimine. See tähendab, et värskendada pidet või sildid, et peate kirjutada kogu registreerimise. Pidage meeles, et registreerimise siirdamiseks, et alati peaks olema usaldusväärne pood teatud seade peab praeguse sildid.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Registreerimise haldamine on taustväärtus kaudu

Registreerimise haldamine kaudu soovitud taustväärtus jaoks on vaja täiendavat koodi kirjutamist. Seadme rakendus peab võimaldama värskendatud PNS toime abil soovitud taustväärtus iga kord, kui rakendus käivitub (koos sildid ja mallid) ja selle taustväärtus peate värskendama selle lahendada, teate jaoturi. Järgmisel pildil on näidatud selles kujundus.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Registreerimise haldamine kaudu soovitud kirjutamata eeliseid hulka võimalus muuta silte, et registreerimise isegi siis, kui seadme vastav rakendus pole aktiivne ja autentida kliendi rakendus enne nende registreerimiseks sildi lisamine.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Näide koodi registreerima teate jaoturi kaudu on taustväärtus installi abil

Kliendi seadme endiselt saab selle PNS täitepide ja oluline installi atribuudid enne ja kohandatud API kutsub kirjutamata, mida saab teha registreerimise ja lubada sildid jne. Funktsiooni taustväärtus saate kasutada [Teatis jaoturi SDK taustväärtus-analüüsitoiminguid](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Samuti saate paik meetodit kasutades [JSON-paik standard](https://tools.ietf.org/html/rfc6902) installi värskendamine.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Näide koodi registreerima teate jaoturi seadmest, kasutades registreerimise ID-d

Kaudu oma rakenduse taustväärtus, mida saate toiminguid lihtsa CRUDS registreerimise. Näiteks:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


Selle kirjutamata tuleb toime kokkulangevus vahel registreerimise värskendused. Teenuse siini pakub registreerimise juhtimine loodetav kokkulangevus juhtelement. HTTP tasemele, rakendatakse see ETag kasutamisega haldamise toimingud. Seda funktsiooni kasutatakse läbipaistev Microsoft SDKs, mis põhjustada erandi, kui värskendus on tagasi lükatud kokkulangevus põhjustel. Rakenduse taustväärtus vastutab töötlemise järgmised erandid ja uuesti proovimine värskendus, kui nõutav.