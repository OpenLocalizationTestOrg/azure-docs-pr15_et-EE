<properties 
    pageTitle="Kuidas kasutada teatis jaoturi PHP" 
    description="Saate teada, kuidas kasutada Azure teatis jaoturi PHP tagaandmebaas." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Kuidas kasutada teatis jaoturi kaudu PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Pääsete teatis jaoturi kõik funktsioonid on PHP-Java-Ruby tagaandmebaas teatis jaoturi ülejäänud liidest kasutades, nagu on kirjeldatud teemas MSDN-i [teatise jaoturi REST API-d](http://msdn.microsoft.com/library/dn223264.aspx).

Selles teemas näitame kohta:

* Koostage ülejäänud kliendi teatis jaoturi funktsioonide php;
* Järgige [toomine alustamise õpetus](notification-hubs-ios-apple-push-notification-apns-get-started.md) teie valitud mobiilne platvorm rakendamise tagaandmebaas osa php.

## <a name="client-interface"></a>Kliendi kasutajaliides
Peamised kliendi kasutajaliidese saate sisestada sama meetodid, mis on saadaval [.NET teatis jaoturi SDK](http://msdn.microsoft.com/library/jj933431.aspx), see võimaldab teil tõlkimiseks otse õpetused ja sellel saidil on praegu saadaval näidiseid ja ühenduse Internetis.

Siit leiate kõik saadaval [PHP ülejäänud ümbris proovi]kood.

Näiteks kliendi loomiseks järgmist.

    $hub = new NotificationHub("connection string", "hubname"); 

IOS kohalikke teatise saatmine
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Rakendamine
Kui te pole veel, järgige meie [toomine alustamise õpetus] kuni viimase jaotis, kus teil on rakendada selle tagaandmebaas.
Ka, kui soovite, saate [PHP ülejäänud ümbris proovi] kood ja liikuge jaotisse [lõpuleviimine õpetuse](#complete-tutorial) .

Kõik üksikasjad, et rakendada kogu ülejäänud ümbrisesse leiate [MSDN-i](http://msdn.microsoft.com/library/dn530746.aspx). Selles jaotises kirjeldatakse PHP rakendamine põhitoimingud juurdepääsu teatise jaoturi ülejäänud lõpp-punktid:

1. Ühendusstringi sõeluda
2. Luba luba luua
3. HTTP-kõne tegemine

### <a name="parse-the-connection-string"></a>Ühendusstringi sõeluda

Siin on peamised ainekursuse rakendamise klient, kelle võistkond, mis sõelub ühendusstring.

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Turbeloa loomine
Turvalisus Turbeloa loomine üksikasjad on saadaval [siin](http://msdn.microsoft.com/library/dn495627.aspx).
Järgmise meetodi on lisada **NotificationHub** klassi luba URI praeguse taotluse ja ühendusstringi ekstraktimist mandaadi põhjal luua.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Teatise saatmine
Esmalt, andke meile määratleda klassi tähistav teatis.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

See tund on ümbris kohalikke teate kehasse, või juhtum malli teatise atribuutide kogum ja päiste kogum, mis sisaldab platvormi kohased atribuudid (nt Apple aegumise atribuudi ja WNS päised) ja vormingu (kohalikke platvormi või Mall).

Vaadake [teatis jaoturi REST API-de dokumentatsiooni](http://msdn.microsoft.com/library/dn495827.aspx) ja teatud teatis platvormide jaoks loodud vorminduselementide jaoks saadaolevad suvandid.

Relvastatud see tund, saame nüüd kirjutada saada teatis meetodite **NotificationHub** klassi sees.

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Ülaltoodud meetodid HTTP POST-taotluse saata oma teatise keskuses õige keha ja saata teate päised /messages lõpp-punktile.

##<a name="complete-tutorial"></a>Õpetuse lõpuleviimine
Nüüd saavad teostada õpetusega alustamine, saates teate PHP tagaandmebaas.

Lähtestab teie teate jaoturi klient (substitute vastavalt [toomine alustamise õpetus]ühenduse string ja keskuse nimi):

    $hub = new NotificationHub("connection string", "hubname"); 

Lisage saada koodi sõltuvalt teie target mobiilne platvorm.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windowsi poe ja Windows Phone 8.1 (mitte Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS-i

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ja 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Töötab teie PHP kood tuleks aedvili nüüd seadme target kuvataks teatis.


## <a name="next-steps"></a>Järgmised sammud
Selles teemas me ilmnes lihtsa Java ülejäänud kliendi teatis jaoturi loomise kohta. Siit saate teha järgmist.

* Laadige alla täielik [PHP ülejäänud ümbris proovi], mis sisaldab kõiki ülal koodi.
* Rohkem õppematerjale teatis jaoturi sildistamine funktsioon katkestamine uudiste õpetuses kohta
* Lisateavet teatiste lükkamine üksikkasutajale [Teavitage kasutajaid õpetuses]

Lisateabe saamiseks vt ka [PHP Arenduskeskus](/develop/php/).

[PHP ülejäänud ümbris näidis]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Hangi alustamise õpetus]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
