<properties 
    pageTitle="Teatis jaoturi Python kasutamise kohta" 
    description="Saate teada, kuidas kasutada Azure teatis jaoturi Python tagaandmebaas." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Kuidas kasutada teatis jaoturi kaudu Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Pääsete teatis jaoturi kõik funktsioonid on Java, PHP, Python ja Ruby tagaandmebaas teatis jaoturi ülejäänud liidest kasutades, nagu on kirjeldatud teemas MSDN-i [teatise jaoturi REST API-d](http://msdn.microsoft.com/library/dn223264.aspx).

> [AZURE.NOTE] See on valimi viide rakendamist rakendamiseks teatise saadab Python ja pole toetatud ametliku teatised jaoturi Python SDK.
>
> See näide kirjutatakse Python 3.4.

Selles teemas näitame kohta:

* ÜLEJÄÄNUD kliendi teatis jaoturi funktsioonide Python koostamine.
* Saada teatisi soovite teatise jaoturi REST API-de Python liidest kasutades. 
* Dump HTTP ülejäänud taotluse või vastuse saamiseks silumine hariduse eesmärk. 

Saate jälgida [toomine alustamise õpetus](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) teie valitud mobiilne platvorm rakendamise tagaandmebaas osa Python.

> [AZURE.NOTE] Proovi ulatus on piiratud saata teatised ja seda ei mis tahes registreerimise juhtimine.

## <a name="client-interface"></a>Kliendi kasutajaliides
Peamised kliendi kasutajaliidese saate sisestada sama meetodid, mis on saadaval [.NET teatis jaoturi SDK](http://msdn.microsoft.com/library/jj933431.aspx). See võimaldab teil tõlkimiseks otse õpetused ja sellel saidil on praegu saadaval näidiseid ja ühenduse Internetis poolt.

Siit leiate kõik saadaval [Python ülejäänud ümbris proovi]kood.

Näiteks kliendi loomiseks järgmist.

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
Mõne Windowsi töölauateatise saatmiseks tehke järgmist.
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Rakendamine
Kui te pole veel, järgige meie [toomine alustamise õpetus] kuni viimase jaotis, kus teil on rakendada selle tagaandmebaas.

Kõik üksikasjad, et rakendada kogu ülejäänud ümbrisesse leiate [MSDN-i](http://msdn.microsoft.com/library/dn530746.aspx). Selles jaotises kirjeldatakse Python rakendamise nõutav juurdepääsu teatise jaoturi ülejäänud lõpp-punktid ja saada teatisi põhitoimingud

1. Ühendusstringi sõeluda
2. Luba luba luua
3. Saata teate, kasutades HTTP REST API-ga

### <a name="parse-the-connection-string"></a>Ühendusstringi sõeluda

Siin on peamised ainekursuse rakendamise klient, mille ehitaja sõelub ühendusstring.

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Turbeloa loomine
Turvalisus Turbeloa loomine üksikasjad on saadaval [siin](http://msdn.microsoft.com/library/dn495627.aspx).
Järgmistest meetoditest on lisatakse **NotificationHub** klassi luba URI praeguse taotluse ja ühendusstringi ekstraktimist mandaadi põhjal luua.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Saata teate, kasutades HTTP REST API-ga
Esimese, lase kasutamine määratleda klassi tähistav teatis.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

See tund on ümbris kohalikke teatise sisu või atribuutide malli teate, päiste kogum, mis sisaldab platvormi kohased atribuudid (nt Apple aegumise atribuudi ja WNS päised) ja vormingu (kohalikke platvormi või malli) komplekt.

Vaadake [teatis jaoturi REST API-de dokumentatsiooni](http://msdn.microsoft.com/library/dn495827.aspx) ja teatud teatis platvormide jaoks loodud vormingud jaoks saadaolevad suvandid.

Nüüd selle klassi saame kirjutada saada teatis meetodite **NotificationHub** klassi sees.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Ülaltoodud meetodid HTTP POST-taotluse saata oma teatise keskuses õige keha ja saata teate päised /messages lõpp-punktile.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Üksikasjalik logimise silumine atribuudi abil
Lubamine silumine atribuudi teate jaoturi lähtestamisel kirjutab välja logimine üksikasjalikku teavet HTTP-päringu ja vastuse dump kui ka üksikasjalikku teavitussõnumi saatmine tulemus. Hiljuti lisasime selle atribuudi nimega [teatis jaoturi TestSend atribuudi](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) tagastab üksikasjalikku teavet teatise saada tulemusi. Seda kasutada - lähtestada, kasutades järgmist:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Teatis jaoturi saada taotlus HTTP URL-i saab lisatud koos "test" päringustringi tulemusena. 

##<a name="complete-tutorial"></a>Õpetuse lõpuleviimine
Nüüd saavad teostada õpetusega alustamine, saates teate Python tagaandmebaas.

Lähtestab teie teate jaoturi klient (substitute vastavalt [toomine alustamise õpetus]ühenduse string ja keskuse nimi):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Lisage saada koodi sõltuvalt teie target mobiilne platvorm. See näide lisab ka kõrgema taseme meetodid, mis võimaldavad saatmise teatised platvormil nt send_windows_notification Windowsi; send_apple_notification (for apple) jne. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windowsi poe ja Windows Phone 8.1 (mitte Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ja 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS-i

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Teie Python koodi käivitamist tuleks aedvili seadme target kuvataks teatis.

## <a name="examples"></a>Näited:

### <a name="enabling-debug-property"></a>Atribuut silumine lubamine
Kui lubate silumine lipu lähtestamisel soovitud NotificationHub, siis kuvatakse üksikasjalikud HTTP-päringu ja vastuse dump samuti NotificationOutcome umbes järgmist kus saavad aru, milliseid HTTP päised möödas taotlus ja teate jaoturi saadi mis HTTP vastus:    ![][1]

Kuvatakse üksikasjalikud teate jaoturi tulem, nt 

- Kui sõnum on edukalt saadetud teavitusteenuse Push. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Tõuketeatised teatest leitud sihtmärgid oleksid siis tõenäoliselt kavatsete kuvada järgmine vastus (mis näitab, et oli veel ühtki registreerimist leitud esitamisel teate ilmselt, sest registreerimise oli mõned sobimatud sildid)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Windowsi töölauateatise leviedastus 

Pange tähele päised, mis saadetakse välja, kui saadate leviedastuse töölauateatise Windows klient. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Silt (või sildi avaldis) teatise saatmine

Pange tähele siltide HTTP-päis, mille saab lisada HTTP-päringu (alltoodud näites me saadavad teate ainult registreerimise koos last "Sport")

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Mitme sildi täpsustades teatise saatmine

Pange tähele, kuidas siltide HTTP päis muudab mitme siltide saatmisel. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Templated teatis

Pange tähele, saadetakse osana HTTP taotluse keha vorming HTTP päise muudatused ja last keha.

**Kliendipoolse – registreeritud Mall**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Serveripoolne - saatmise funktsiooni last**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Järgmised sammud
Selles teemas me ilmnes lihtsa Python ülejäänud kliendi teatis jaoturi loomise kohta. Siit saate teha järgmist.

* Laadige alla täielik [Python ülejäänud ümbris proovi], mis sisaldab kõiki ülal koodi.
* Rohkem õppematerjale kohta teatis jaoturi sildistamine funktsioon [katkestamine uudiste õpetus]
* Rohkem õppematerjale kohta teate jaoturi Mallid funktsiooni [tüüpie Uudised õpetus]

<!-- URLs -->
[Python ülejäänud ümbris näidis]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Hangi alustamise õpetus]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Uudised õpetuse katkestamine]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Tüüpie uudiste õpetus]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
