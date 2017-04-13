<properties
    pageTitle="Twilio kasutamine, kõneposti ja SMS (PHP) | Microsoft Azure'i"
    description="Saate teada, kuidas helistada ja Azure SMS-teenuse Twilio API sõnumi saatmine. Koodinäiteid php kirjutada."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Hääl-ja SMS-võimalustega php Twilio kasutamise kohta
Sellest juhendist näitab, kuidas sooritada programmeerimise põhitoimingud Twilio API teenuse Azure. Stsenaariumid, mis hõlmab sisaldavad Helistamine ja lühike tekstsõnum (SMS) sõnumi saatmist. Twilio ja kõneposti ja SMS kasutamine rakenduste kohta leiate lisateavet jaotisest [Järgmised toimingud](#NextSteps) .

## <a id="WhatIs"></a>Mis on Twilio?
Twilio on anda tulevikus äri side, mis võimaldab arendajatel manustamine kõneposti, VoIP ja sõnumside rakendustesse. Need Virtualisoinnilla kõik infrastruktuuri pilveteenuseid, globaalne keskkonnas, asetades selle Twilio suhtlus API platvormi kaudu. Rakendused on koostada lihtsaid ja scalable. Teiega maksta-kui on võimalus avage hinnad ja pilveteenuse töökindluse kasu.

**Twilio Voice** võimaldab rakenduste ja kõnesid vastu võtta. **Twilio SMS** võimaldab saata ja vastu võtta tekstsõnumeid rakenduse. **Twilio klient** , saate teha VoIP kõnede mis tahes telefoni, tahvelarvuti või veebibrauseri kaudu ja WebRTC toetab.

## <a id="Pricing"></a>Twilio hinnad ja pakutakse teisiti

Azure'i kliendid saavad [Eripakkumine] $10 Twilio krediidi täiendamisel Twilio kontole. Selle krediidi Twilio saab rakendada mis tahes Twilio kasutus ($10 krediitkaardiga nii palju 1000 lühisõnumite või vastuvõtmine kuni 1000 sissetuleva heli minutit, sõltuvalt oma telefoni number ja sõnumi või kõne sihtkoha asukoht). Selle krediidi Twilio lunastamine ja alustamine veebisaidil: [ahoy.twilio.com/azure].

Twilio on pensionitingimustega teenus. Puuduvad ülesehituse tasu ja konto sulgemist igal ajal. Leiate rohkem üksikasju [Twilio hinnad] [twilio_pricing].

## <a id="Concepts"></a>Mõisted
Twilio API on rahulik API-ga, mis pakub kõneposti ja SMS funktsioonid rakenduste. Kliendi teegid on saadaval mitmes keeles; loendi leiate teemast [Teekide Twilio API] [twilio_libraries].

Võtme aspektide Twilio API on Twilio verbide ja Twilio märgistuskeel (TwiML).

### <a id="Verbs"></a>Twilio verbide
API kasutab Twilio verbide; Näiteks kuvatakse ** &lt;öelda&gt; ** tegusõna juhendab Twilio kuuldavalt esitamisel sõnumi telefonil.

Järgmises loendis Twilio verbide. Lisateavet muude verbide ja võimaluste kaudu [Twilio märgistuskeel dokumentatsiooni] [http://www.twilio.com/docs/api/twiml].

* ** &lt;Sissehelistamise&gt;**: helistaja ühendub teises telefonis.
* ** &lt;Kogumine&gt;**: kogub telefoni numbriklahvistikul sisestatud kahekohalise arvuna.
* ** &lt;Lahutamise&gt;**: kõne lõpeb.
* ** &lt;Esitada&gt;**: esitab helifaili.
* ** &lt;Paus&gt;**: vaikselt ootab määratud sekundite arv.
* ** &lt;Kirje&gt;**: kirjete helistaja hääl ja tagastab URL-i faili, mis sisaldab salvestise.
* ** &lt;Ümber suunata&gt;**: annab juhtimise telefonikõne või SMS TwiML erinevate URL.
* ** &lt;Kõnest&gt;**: sissetuleva kõne oma Twilio arvuks hülgab ilma arveldamine saate
* ** &lt;Öelda&gt;**: teisendab teksti kõneks telefonil tehtud.
* ** &lt;Sms&gt;**: saadab sõnumi.

### <a id="TwiML"></a>TwiML
TwiML on XML-põhine juhiste põhjal Twilio verbide, mida sellest, kuidas töödelda kõne Twilio või SMS.

Näiteks soovite järgmised TwiML **Tere, maailm** teksti teisendamine kõneks.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Kui teie rakendus nõuab Twilio API, üks API parameetrid on URL, mis tagastab TwiML vastus. Arengu eesmärgil saate Twilio antud URL-ide esitada rakenduste kasutatava TwiML vastuseid. Võib ka majutate oma URL-id, andes TwiML vastuste ja teine võimalus on kasutada **TwiMLResponse** objekti.

Twilio verbide, nende atribuutide ja TwiML kohta leiate lisateavet teemast [TwiML] [twiml]. Twilio API kohta lisateabe saamiseks lugege teemat [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Twilio konto loomine
Kui olete valmis saada Twilio konto, [Proovige Twilio]veebisaidil registreeruda [try_twilio]. Saate alustada tasuta konto ja uuendada oma konto hiljem.

Kui te registreerute Twilio konto, saate konto ID-d ja mõne autentimise luba. Mõlemad on vaja Twilio API kõnede tegemiseks. Oma kontole volitamata juurdepääsu vältimiseks turvaliseks oma autentimise luba. Oma konto ID ja autentimise [Twilio kontole leht]vaadatav luba [twilio_account], väljade nimega **konto SID** ja **AUTH TURBELOA**,.

## <a id="create_app"></a>PHP rakenduse loomine
PHP rakendus, mis kasutab teenust Twilio ja töötab Azure ei erine muu PHP rakendus, mis kasutab teenust Twilio. Kuigi Twilio teenused on ülejäänud põhinev ja saab helistada PHP on mitu võimalust, see artikkel keskendub [Twilio]teegiga php kaudu GitHub Twilio teenuste kasutamiseks[twilio_php]. PHP Twilio teegi kasutamise kohta leiate lisateavet teemast [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Üksikasjalikud juhised koostamise ja juurutamine Azure Twilio/PHP rakendus on saadaval, kuidas [telefonikõne abil Twilio PHP rakendus Azure][howto_phonecall_php].

## <a id="configure_app"></a>Rakenduse kasutamiseks Twilio teekide konfigureerimine
Saate konfigureerida rakenduse Twilio teegi kasutamiseks PHP kahel viisil:

1. Laadige alla Twilio teegi PHP GitHub ([https://github.com/twilio/twilio-php][twilio_php]) ja lisage **teenuseid** kataloogi rakenduse.

    -VÕI-

2. Installige Twilio teegi php PIRNIPUUDE paketina. See saab installida järgmised käsud:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Kui olete installinud Twilio teegi php, saate lisada PHP failide viitamiseks teegi ülaosas seejärel **require_once** lause:

        require_once 'Services/Twilio.php';

Lisateavet leiate teemast [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Kuidas: kutset
Järgmine näitab, kuidas abil **Services_Twilio** klassi kutset. Järgmine kood kasutab ka Twilio antud saidi vastuse Twilio märgistuskeel (TwiML) tagastamiseks. Asendada oma väärtused **alates** ja **kuni** telefoninumbrid ja veenduge, et kontrollida telefoninumber **:** enne koodi Twilio kontole.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Nagu mainitud, on järgmine kood kasutatakse esitatud Twilio saidi TwiML vastus. Saate selle asemel kasutada oma saidi TwiML vastust; Lisateavet leiate teemast [pakuvad TwiML vastuste teie enda veebisaidilt](#howto_provide_twiml_responses).


- **Märkus**: SSL-i serdi valideerimine tõrgete tõrkeotsingu artiklist [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Kuidas: SMS saatmine
Pärast näitab **Services_Twilio** klassi SMS-sõnumi saatmine. Arv **,** pakub Twilio prooviversiooni kontode SMS-sõnumeid saata. **Et** telefoninumber olema kinnitatud enne koodi Twilio konto jaoks.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Kuidas: sisestage oma veebisaidi TwiML vastused
Kui rakenduse käivitab Twilio API kõne, saadab Twilio oma päringu URL, mis on oodata TwiML vastuse. Ülaltoodud näites Twilio antud URL-i [http://twimlets.com/message][twimlet_message_url]. (Kui TwiML on mõeldud Twilio kasutamiseks, saate vaadata it brauseris. Näiteks klõpsake [http://twimlets.com/message] [ twimlet_message_url] kuvamiseks tühja `<Response>` elemendi; Teine näide klõpsake [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] kuvamiseks on `<Response>` element, mis sisaldab soovitud `<Say>` element.)

Selle asemel Twilio antud URL-i, saate luua oma saidile, mis tagastab HTTP vastused. Saate luua saidi mis tahes keeleks, mis tagastab XML-i vastuseid; selles teemas eeldab, et kasutate PHP on TwiML loomiseks.

Järgmisel leheküljel PHP, on tulemuseks TwiML vastuse, mis ütleb **Tere, maailm** kõnes.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Nagu näete Ülaltoodud näites, TwiML vastus on lihtsalt XML-dokument. Php Twilio teek sisaldab tunnid, mis loob teie jaoks TwiML. Järgmises näites võrdväärse vastuse annab tulemiks, nagu eespool näidatud, kuid kasutab funktsiooni **teenuste\_Twilio\_Twiml** klassi Twilio teegis php:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

TwiML kohta leiate lisateavet teemast [https://www.twilio.com/docs/api/twiml][twiml_reference].

Kui olete oma PHP lehel häälestatud TwiML vastuseid anda, kasutage PHP lehe URL URL-i läks selle `Services_Twilio->account->calls->create` meetod. Näiteks kui teil on veebirakenduse nimega **MyTwiML** juurutatud on Azure majutatud teenus ja PHP lehe nimi on **mytwiml.php**, URL-i saab edasi **Services_Twilio -> konto -> kõned -> Loo** nagu on näidatud järgmises näites:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Azure php Twilio kasutamise kohta lisateabe saamiseks vaadake, [Kuidas teha telefonikõne abil Twilio PHP rakendus Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Kuidas: täiendavad Twilio teenuste kasutamine
Lisaks siin esitatud näidetes Twilio pakub veebipõhine API abil saate kasutada Twilio lisafunktsioone Azure rakenduse kaudu. Üksikasjadega tutvumiseks vaadake [Twilio API dokumentatsiooni] [twilio_api_documentation].

## <a id="NextSteps"></a>Järgmised sammud
Nüüd, kui olete õppinud põhitõdesid Twilio teenuse, järgige neid linke Lisateavet:

* [Twilio turvalisuse juhised] [twilio_security_guidelines]
* [Twilio juhendid juhikud ja näide kood] [twilio_howtos]
* [Twilio Kiirjuhend õppematerjalid][twilio_quickstarts]
* [Twilio github] [twilio_on_github]
* [Rääkige Twilio tugi] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
