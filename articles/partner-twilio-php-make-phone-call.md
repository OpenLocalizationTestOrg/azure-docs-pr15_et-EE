<properties
    pageTitle="Kuidas helistada Twilio (PHP) | Microsoft Azure'i"
    description="Saate teada, kuidas helistada ja Azure SMS-teenuse Twilio API sõnumi saatmine. Näidet on PHP rakenduse."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Kuidas Twilio kasutamine PHP rakendus Azure helistamine

Järgmises näites kuvatakse kasutamist Twilio veebilehelt PHP majutatud Azure helistamiseks. Tulemuseks taotlus küsib kasutaja telefonikõne väärtusi, nagu on näidatud järgmisel kuvatõmmisel kuvatõmmis.

![Azure'i kõne vormi Twilio ja PHP abil][twilio_php]

Peate tegema järgmist kasutada koodi selles teemas.

1. Hankida Twilio konto ja autentimise sümboolne. Twilio alustada hinnata hindade [http://www.twilio.com/pricing][twilio_pricing]. Saate sisse logida aadressil [https://www.twilio.com/try-twilio]prooviversiooni konto[try_twilio]. Esitatud Twilio API kohta leiate teemast [http://www.twilio.com/api][twilio_api].
2. Saada [Twilio teegi php](https://github.com/twilio/twilio-php) või selle installida PIRNIPUUDE paketina. Lisateabe saamiseks vaadake [readme-faili](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Installige Azure'i SDK php. SDK ja juhiseid installimist ülevaate leiate [Azure'i SDK php häälestamine][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Helistamine jaoks veebivormi loomine

Järgmised HTML-koodi näitab, kuidas koostada veebileht (**callform.html**), mis toob helistamine kasutaja andmeid:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Helistamiseks koodi loomine
Järgmine kood näitab, kuidas koostada veebileht (**makecall.php**), mida nimetatakse, kui kasutaja edastab vormi **callform.html**kuvatud. Allpool näidatud koodi kõne sõnumi ja loob kõne. (Kasutage oma Twilio konto ja autentimise sümboolne asemel määratud **$sid** ja **$token** allpool kood kohatäite väärtused.)

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Lisaks helistamise, **makecall.php** kuvatakse mõned kõne metaandmete (nt pildil näidatud). Kõne metaandmete kohta leiate lisateavet teemast [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure'i kõne vastuse Twilio ja PHP abil][twilio_php_response]

## <a name="run-the-application"></a>Käivitage rakendus
Järgmiseks on juurutada Azure veebisaitide rakendust. Järgmised artiklid sisaldavad lisateavet veebisaidi loomise ja juurutamine oma koodi Git, FTP või WebMatrix (kuigi kõik iga artikli teave on oluline):

* [PHP MySQL Azure veebisaidi loomine ja juurutamine Git abil][website-git]
* [PHP MySQL Azure veebisaidi loomine ja juurutamine FTP abil][website-ftp]

## <a name="next-steps"></a>Järgmised sammud
Järgmine kood esitatud Twilio kasutamine PHP Azure põhifunktsioonide kuvamiseks. Enne juurutamist Azure valmistamisel, soovite lisada veel tõrge käsitsemise või muid funktsioone. Näiteks:

* Veebivormi asemel võite kasutada Azure storage plekid või SQL-andmebaasi salvestada telefoninumbrid ja kõne teksti. Kasutamise kohta leiate Azure'i salvestusruumi plekid php, lugege teemat [Abil Azure Storage PHP rakendusi][howto_blob_storage_php]. SQL-andmebaasi php kasutamise kohta leiate teemast [Abil SQL-andmebaasi PHP rakendustega][howto_sql_azure_php].
* **Makecall.php** kood kasutab Twilio antud URL-i ([http://twimlets.com/message][twimlet_message_url]) esitada Twilio märgistuskeel (TwiML) vastust, mis teavitab Twilio kõne jätkamiseks tehke järgmist. Näiteks võib sisaldada TwiML, mis tagastatakse on `<Say>` tegusõna, mis annab tulemuseks teksti esituse kõne adressaadile. URL-ide Twilio asemel võib koostada oma teenuse vastata Twilio isiku taotlus; Lisateabe saamiseks vaadake, [Kuidas kasutada Twilio hääl-ja SMS-võimalustega php][howto_twilio_voice_sms_php]. Lisateavet TwiML kohta leiate [http://www.twilio.com/docs/api/twiml][twiml], ja rohkem teavet `<Say>` ja muud Twilio verbide leiate [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lugege veebisaidil [https://www.twilio.com/docs/security]Twilio turvalisuse juhised[twilio_docs_security].

Twilio kohta lisateabe saamiseks lugege teemat [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Vt ka
* [Kuidas kasutada Twilio heli- ja SMS-võimalustega php](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
