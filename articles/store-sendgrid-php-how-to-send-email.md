<properties 
    pageTitle="Kuidas kasutada SendGrid e-posti teenus (PHP) | Microsoft Azure'i" 
    description="Siit saate teada, kuidas saata meilisõnumeid, SendGrid e-posti teenuse Azure. Koodinäiteid php kirjutada." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Kuidas kasutada PHP SendGrid e-posti teenuse

Sellest juhendist näitab, kuidas sooritada Azure'i programmeerimise põhitoimingud SendGrid e-posti teenus. Näidiste kirjutada PHP.
Stsenaariumid, mis hõlmab kaasata **ehitamine e-posti**, **e-posti saatmine**ja **manuste lisamine**. SendGrid ja e-posti saatmise kohta leiate lisateavet jaotisest [Järgmised toimingud](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Mis on SendGrid e-posti teenus?

SendGrid on [pilvepõhist e-posti teenus] , mis pakub usaldusväärne [selgituseks meilisõnumite kohaletoimetamise], skaleeritavus ja reaalajas analytics koos paindlik API-d, mis muudavad kohandatud integreerimine lihtne. Levinud SendGrid kasutamise stsenaariumid on järgmised.

-   Automaatne teatiste saatmise klientidele
-   Igakuine e-fliers ja pakkumisi klientidele saatmiseks haldamise jaotuse loendid
-   Reaalajas mõõdikute asjad blokeeritud e-posti ja klientide tundlikkuse kogumine
-   Trendide tuvastamiseks aruannete loomiseks
-   Ümbersuunamine klientide päringud
- Meiliteatised rakenduse kaudu

Lisateavet leiate teemast [https://sendgrid.com][].

## <a name="create-a-sendgrid-account"></a>SendGrid konto loomine

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>SendGrid oma PHP rakenduse kasutamine

Azure'i PHP rakenduse SendGrid kasutamine nõuab pole teisiti konfiguratsiooni või kood. Kuna SendGrid on teenus, see pääseb rakendusest cloud täpselt samamoodi nagu seda saab kohapealse rakendusest.

## <a name="how-to-send-an-email"></a>Kuidas: e-posti saatmine

Saate saata e-posti, kasutades SMTP või esitatud SendGrid Web API-ga.

### <a name="smtp-api"></a>SMTP API

SendGrid SMTP API abil meilisõnumeid saata, kasutage *Kiire meiliga saatmiseks*, komponent-põhiste teegi meilide saatmisel PHP rakendustest. *Kiire meiliga saatmiseks* teegist saate alla laadida [http://swiftmailer.org/download][] v5.3.0 (kasutage [helilooja] installimiseks kiire meiliga saatmiseks). Teegi meilisõnumite saatmise hõlmab, luua esinemisjuhud on <span class="auto-style2">kiire\_SmtpTransport</span>, <span class="auto-style2">kiire\_meiliga saatmiseks</span>, ja <span class="auto-style2">kiire\_sõnumi</span> tunnid, sobiv atribuutide seadmine ja kutsudes on <span class="auto-style2">kiire\_Mailer::send</span> meetod.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Veebi-API

PHP's [curl funktsiooni][] abil saate saata meili SendGrid Web API.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

SendGrid's Web API on väga sarnane REST API, kui see pole tõeliselt rahulik API-ga, kuna enamik kõned, mõlemad hankimine ja postituse verbide saab kasutada vaheldumisi.

## <a name="how-to-add-an-attachment"></a>Kuidas: manuse lisamine

### <a name="smtp-api"></a>SMTP API

Kasutades SMTP API Manuse saatmine hõlmab ühe täiendava rida koodi näide skripti kiire meiliga saatmiseks meili saatmine.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Täiendavad rida koodi on järgmine:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Seda palub manustamine meetod on <span class="auto-style2">kiire\_sõnumi</span> objekti ja kasutab staatiline meetod <span class="auto-style2">fromPath</span> on <span class="auto-style2">kiire\_manuse</span> klassi saada ja faili manustamiseks sõnumile.

### <a name="web-api"></a>Veebi-API

Kasutades Web API Manuse saatmine on väga sarnane veebi-API abil meilisõnumi saatmine. Pange tähele, et järgneva näites parameeter massiiv peab sisaldama – see element:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Näide:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Kuidas: jalused, jälgimine ja Analytics filtrite abil

SendGrid pakub e-posti funktsiooni abil "filtreid". Neid sätteid, mille saab lisada meilisõnumi lubamiseks teatud funktsioone nagu lubamine nuppu jälitus, Google Analyticsi tellimuse jälgimise ja jne.

Filtreid saab rakendada sõnumile atribuudi filtrite abil. Iga filter on määratud räsi sisaldavad filtri sätted. Järgmises näites jaluse filter võimaldab ja määrab tekstsõnumi, mis lisatakse meilisõnumi lõppu.
Selles näites me kasutame [sendgrid-php teek].
Teegi installimiseks kasutada [helilooja] :
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Näide:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid SendGrid e-posti teenus, järgige neid linke lisateabe.

-   SendGrid dokumentatsiooni: <https://sendgrid.com/docs>
-   SendGrid PHP Raamatukogu: <https://github.com/sendgrid/sendgrid-php>
-   Azure'i klientidele Eripakkumine SendGrid: <https://sendgrid.com/windowsazure.html>

Lisateabe saamiseks vt ka [PHP Arenduskeskus](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/Download]: http://swiftmailer.org/download
  [funktsioon Curl]: http://php.net/curl
  [pilvepõhine e-posti teenus]: https://sendgrid.com/email-solutions
  [Selgituseks meilisõnumite kohaletoimetamise]: https://sendgrid.com/transactional-email
  [sendgrid-php teek]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Helilooja]: https://getcomposer.org/download/
