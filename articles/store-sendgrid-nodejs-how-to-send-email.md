<properties 
    pageTitle="Kuidas kasutada SendGrid e-posti teenus (Node.js) | Microsoft Azure'i" 
    description="Siit saate teada, kuidas saata meilisõnumeid, SendGrid e-posti teenuse Azure. Kirjutada, kasutades Node.js API proovi kood." 
    services="" 
    documentationCenter="nodejs" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="01/05/2016" 
    ms.author="erikre"/>
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a>Kuidas saata meili kaudu Node.js SendGrid

Sellest juhendist näitab, kuidas sooritada Azure'i programmeerimise põhitoimingud SendGrid e-posti teenus. Näidiste kirjutada Node.js API-ga. Stsenaariumid, mis hõlmab kaasake **ehitamine e-posti**, **e-posti saatmine**, **manuste lisamise**, **filtrite abil**ja **atribuutide värskendamine**. SendGrid ja e-posti saatmise kohta leiate lisateavet jaotisest [Järgmised toimingud](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Mis on SendGrid e-posti teenus?

SendGrid on [pilvepõhist e-posti teenus], mis pakub usaldusväärne [selgituseks meilisõnumite kohaletoimetamise], skaleeritavus ja reaalajas analytics koos paindlik API-d, mis muudavad kohandatud integreerimine lihtne. Levinud SendGrid kasutamise stsenaariumid on järgmised.

-   Automaatne teatiste saatmise klientidele
-   Igakuine e-fliers ja pakkumisi klientidele saatmiseks haldamise jaotuse loendid
-   Reaalajas mõõdikute asjad blokeeritud e-posti ja klientide tundlikkuse kogumine
-   Trendide tuvastamiseks aruannete loomiseks
-   Ümbersuunamine klientide päringud
-   Meiliteatised rakenduse kaudu

Lisateavet leiate teemast [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>SendGrid konto loomine

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a>SendGrid Node.js mooduli viitamine.

SendGrid moodul Node.js saab installida sõlm paketi manager (npm) kaudu, kasutades järgmine käsk:

    npm install sendgrid

Pärast installimist saate nõuda mooduli oma rakenduse abil järgmine kood:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

SendGrid mooduli ekspordib **SendGrid** ja **e-posti** funktsioone.
**SendGrid** vastutab saatmine e-posti kaudu Web API, samal ajal **e-posti** kapseloi meilisõnumi.

## <a name="how-to-create-an-email"></a>Kuidas: luua e-posti

SendGrid mooduli kasutamise meilisõnumi loomine hõlmab esmalt loomine meilisõnumi, kasutades funktsiooni e-posti ja seejärel saatmist SendGrid funktsiooni abil. Järgmises näites luua uue sõnumi, kasutades e-posti funktsiooni:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

Saate määrata ka HTML-meilisõnumis kliente, mis toetavad seda, seades atribuuti HTML-vormingus. Näiteks:

    html: This is a sample <b>HTML<b> email message.

Nii teksti kui ka HTML-i atribuutide seadmine pakub klientidele, mis ei toeta HTML-sõnumite graatsiline Fall teksti sisu.

Kõik atribuudid, mis ei toeta e-posti funktsiooni kohta leiate lisateavet teemast [[sendgrid nodejs]].

## <a name="how-to-send-an-email"></a>Kuidas: e-posti saatmine

Pärast meilisõnumi, kasutades e-posti funktsiooni loomist saate selle Web API nähtud SendGrid abil saata. 

### <a name="web-api"></a>Veebi-API

    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [AZURE.NOTE] Kuigi ülaltoodud näidetes läbimise e-posti objekti ja tagasihelistamise funktsiooni, saate ka otse autonoomsest funktsiooni saada määramisega otse e-posti atribuudid. Näiteks:  
>
>`````
sendgrid.send({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});
`````

## How to: Add an Attachment

Attachments can be added to a message by specifying the file name(s) and
path(s) in the **files** property. The following example demonstrates
sending an attachment:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [AZURE.NOTE] When using the **files** property, the file must be accessible
through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). If the file you wish to attach is hosted in Azure Storage, such as in a Blob container, you must first copy the file to local storage or to an Azure drive before it can be sent as an attachment using the **files** property.

## How to: Use Filters to Enable Footers and Tracking

SendGrid provides additional email functionality through the use of
filters. These are settings that can be added to an email message to
enable specific functionality such as enabling click tracking, Google
analytics, subscription tracking, and so on. For a full list of filters,
see [Filter Settings][].

Filters can be applied to a message by using the **filters** property.
Each filter is specified by a hash containing filter-specific settings.
The following examples demonstrate the footer and click tracking filters:

### Footer

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### Click Tracking

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });
    
    sendgrid.send(email);

## How to: Update Email Properties

Some email properties can be overwritten using **set*Property*** or
appended using **add*Property***. For example, you can add additional
recipients by using

    email.addTo('jeff@contoso.com');

or set a filter by using

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

For more information, see [sendgrid-nodejs][].

## How to: Use Additional SendGrid Services

SendGrid offers web-based APIs that you can use to leverage additional
SendGrid functionality from your Azure application. For full
details, see the [SendGrid API documentation][].

## Next Steps

Now that you've learned the basics of the SendGrid Email service, follow
these links to learn more.

-   SendGrid Node.js module repository: [sendgrid-nodejs][]
-   SendGrid API documentation:
    <https://sendgrid.com/docs>
-   SendGrid special offer for Azure customers:
    [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)
  [special offer]: https://sendgrid.com/windowsazure.html
  [sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
  [Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API documentation]: https://sendgrid.com/docs
  [cloud-based email service]: https://sendgrid.com/email-solutions
  [transactional email delivery]: https://sendgrid.com/transactional-email
