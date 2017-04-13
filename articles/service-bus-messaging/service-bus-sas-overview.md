<properties
    pageTitle="Ühiskasutusse antud juurdepääs allkirjad ülevaade | Microsoft Azure'i"
    description="Mis on jagatud juurdepääsu allkirjad, kuidas need töötavad, ja kuidas neid kasutada sõlm, PHP ja C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Ühiskasutusega juurdepääsu allkirjad

*Ühiskasutusse antud juurdepääs allkirjad* (SAS) on teenuse siini, sh sündmuse jaoturi, vahendajaks sõnumside (järjekorrad ja teemade) jaoks esmane turvalisuse süsteem ja edastada, sõnumside. Selles artiklis käsitletakse ühiskasutusse Accessi allkirjad, iseloomustus ja platvormi diagnostika viis kasutamine.

## <a name="overview-of-sas"></a>SAS ülevaade

Ühiskasutusega juurdepääsu allkirjad on autentimise süsteem, mis põhineb SHA 256 secure hashes või URI-d. SAS on väga võimas kasutatava teenuse siini kõiki teenuseid. Tegelik kasutusel on kahest osast: *Ühiskasutuses juurdepääsu poliitika* ja *Ühiskasutuses Accessi allkirja* (sageli nimetatakse ka *Turbeloa*).

Leiate täpsemat teavet ühiskasutusega juurdepääsu allkirjade teenuse siini [teenuse siini ühiskasutusse Accessi allkirja autentimine](service-bus-shared-access-signature-authentication.md).

## <a name="shared-access-policy"></a>Ühiskasutusega juurdepääsu poliitika

Oluline teave SAS mõista on see kõik algab poliitika. Iga poliitika, saate otsustada kolme liiki andmed: **nimi**, **ulatuse**ja **õigused**. **Nimi** on lihtsalt, et; ulatuse piirides kordumatu nimi. Ulatus on lihtne: on URI kõnealuse ressursi. Teenuse siini nimeruumi, ulatus on täielik domeeninimi (FQDN), nagu näiteks `https://<yournamespace>.servicebus.windows.net/`.

Saadaolevaid õigusi poliitika enesestmυistetav:

  + Saatmine
  + Kuulake
  + Haldamine

Kui olete loonud poliitika, see on määratud *Primaarvõtme* ja *Teisese võtme*. Need on krüptimise osas tugev võtmed. Pole neid kaotada või lekkida neid - alati need [Azure portaali][]. Kasutage ühte loodud võtmed ja te saate need taastada igal ajal. Siiski primaarvõtme poliitika muutmisel või taastada, kehtetuks mis tahes ühiskasutusse Accessi allkirjade põhjal loodud.

Kui loote teenuse siini nimeruumi, poliitika luuakse automaatselt kogu nimeruumi nimetatakse **RootManageSharedAccessKey**ja selle poliitika on kõik õigused. Ärge logige sisse **root**, seega ei kasuta selles poliitika, v.a juhul, kui seal on väga head põhjust. Saate luua täiendavaid poliitikate **konfigureerimine** vahekaardil nimeruumi portaalis. Oluline on teate, et ühe puu taseme teenuse siini (nimeruumi, järjekorda, sündmuse keskuses jne) võib olla kuni 12 poliitikaid, manustatud.

## <a name="shared-access-signature-token"></a>Ühiskasutusega juurdepääsu allkirja (luba)

Poliitika pole juurdepääsu luba teenuse siini jaoks. See on objekt, millest luuakse juurdepääsu luba – nii põhi- või klahvi abil. Luba on loodud hoolikalt käsitöö stringi järgmises vormingus:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Kus `signature-string` on SHA 256 räsi luba (**ulatus** eelmises jaotises kirjeldatud) ulatuse CRLF, mis on lisatud ja kuvatakse aeg (sekundites alates epohhist: `00:00:00 UTC` 1 Jaanuar 1970).

Räsi näeb välja umbes järgmine kood pseudo ja tagastab 32 baiti.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Räsitud väärtused on **SharedAccessSignature** stringi nii, et adressaat saab arvutada räsi samade parameetritega, veendumaks, et see tagastab see sama tulemi. URI määrab ulatuse ja võtme nimi tuvastab poliitika arvutada räsi kasutada. See on oluline turvalisuse vaatevinklist. Kui soovitud signatuur ei vasta, mis arvutab adressaat (teenuse siini), siis on juurdepääs keelatud. Selles etapis võib olla kindel, et saatja oli juurdepääs võti ja antakse õigused määratud poliitika.

## <a name="generating-a-signature-from-a-policy"></a>Signatuuri loomisel poliitika

Kuidas tegelikult teha seda koodi? Heitkem pilk mõned neist.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>K & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Kaudu ühiskasutusse antud juurdepääs allkirja (HTTP tasandil)
 
Nüüd, kui teate, kuidas luua ühiskasutusse Accessi allkirjade teenuse siini üksused, olete valmis teha ka HTTP POST:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Pidage meeles, et see kõik töötab. Saate luua SAS järjekorda, teema, tellimus, sündmuse jaoturi või relay. Sündmuse jaoturi kohta Publisheri identiteedi kasutamisel lihtsalt lisamine `/publishers/< publisherid>`.

Kui annate või saatja kliendi SAS luba, neil pole võti otse ja nad ei saa räsi saamiseks ümber pöörata. Sellisena, peate kontrolli selle üle, pääsete juurde, ja kuidas pikk. Pidage meeles, et oluline on, et kui muudate primaarvõtme poliitika, mis tahes ühiskasutusse Accessi allkirjade põhjal loodud kehtetuks.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Kaudu ühiskasutusse antud juurdepääs allkirja (AMQP tasandil)

Eelmises jaotises teile kuvati kasutamine SAS luba taotlusega HTTP POST teenuse siini andmete saatmiseks. Kui teate, pääsete teenuse siini abil on täiustatud sõnumi järjekord protokolli (AMQP) on eelistatud protokoll jõudluse huvides mitmel puhul kasutada. SAS Turbeloa kasutuse AMQP on kirjeldatud dokumendis on töötamise mustand alates 2013, kuid hästi toeta Azure täna [AMQP Claim-Based turvalisus versiooni 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) .

Enne alustamist teenuse siini andmeid saata, peab avaldaja saata meilisõnumi AMQP sees SAS luba täpselt määratletud AMQP sõlm nimega **$cbs** (mida näete seda teenuse hankimine ja kinnitada kõigi SAS sõned kasutatav "eriline" järjekorda). Avaldaja peate määrama **OutboundSMTPServeri parameetrid** välja sees AMQP sõnumi; See on sõlme teenuse vastamine Publisheri tulemiga Turbeloa valideerimine (lihtne taotluse/vastuse mustri publisher ja teenuse vahel). Selle vastus sõlme luuakse "pealt," räägi "dünaamiline loomine remote sõlm" kirjeldatud AMQP 1.0 määratlus. Pärast ruudu Luba SAS kehtib, avaldaja edasi liikumine ja andmete saatmiseks teenuse käivitamine.

Järgmised toimingud näitab, kuidas saata SAS luba AMQP protokoll [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) teegi abil. See on kasulik, kui te ei saa kasutada ametliku teenuse siini SDK (näiteks kohta WinRT .net Compact Framework, .net-i tagasi ning Micro Framework) arendamise c\#. Muidugi see teek on kasulik, mis aitab mõista, kuidas taotlusepõhise turvalisus töötab AMQP tasemel, nagu te nägite, kuidas see toimib tasemel HTTP (koos HTTP POST-taotluse ja SAS luba saadetud sees "Autoriseerimine" päis). Kui te ei pea neid sügav teadmisi AMQP kohta, saate teenuse ametlik siini SDK .net Frameworki rakendused, mis teeb teie eest.

### <a name="c35"></a>K & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Funktsiooni `PutCbsToken()` meetod saab *ühenduse* (AMQP ühenduse klassi eksemplari [AMQP .NET Lite teek](https://github.com/Azure/amqpnetlite)vastavalt) tähistava TCP ühendus teenuse ning *sasToken* parameeter, mis on antud SAS saata. 

> [AZURE.NOTE] On oluline, et ühendus on loodud **SASL autentimise süsteem seatud väline** (ja pole vaikimisi tavaline kasutajanimi ja parool, kui teil pole vaja saata SAS luba).

Järgmisena loob avaldaja kaks AMQP linkide SAS luba saata ja vastu võtta vasta (tulem Turbeloa valideerimine) teenuse.

AMQP sõnum sisaldab atribuudid ja lisateavet, kui lihtne sõnum. Luba SAS on (abil oma konstruktori) sõnumi kehasse. **"OutboundSMTPServeri parameetrid"** atribuudi väärtuseks on seatud sõlm nimi vastuvõtmise valideerimine vastuvõtja link (saate muuta oma nime, kui soovite ning see luuakse dünaamiliselt, millal teenus). Kolm viimast rakenduse/kohandatud atribuutide kasutavad teenuse näidata, millist toiming on käivitada. CBS mustand määratlus kirjeldatud need peavad olema selle **toimingu nimi** ("Pane luba"), on **Tippige luba** (sel juhul "servicebus.windows.net:sastoken") ja **"nimi" sihtrühma** millele luba kehtib (kogu üksus).

Pärast saatmist SAS luba saatja linki, avaldaja peate lugema vasta vastuvõtja linki. Vastus on lihtne AMQP sõnum nimega **"olekukoodi"** , mis võib sisaldada samad väärtused on HTTP olekukoodi nimega rakenduse atribuudiga. 

## <a name="next-steps"></a>Järgmised sammud

Vt lisateavet nende tähiste SAS võimalikud [teenuse siini REST API viide](https://msdn.microsoft.com/library/azure/hh780717.aspx) .

Teenuse siini autentimise kohta leiate lisateavet teemast [teenuse siini autentimise ja luba](service-bus-authentication-and-authorization.md). 

Veel näiteid Lennureisi ajal C# ja JavaScripti on [sellest ajaveebipostitusest](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Azure'i portaal]: https://portal.azure.com