<properties 
    pageTitle="SendGrid e-posti teenus (.NET) kasutamise kohta | Microsoft Azure'i" 
    description="Siit saate teada, kuidas saata meilisõnumeid, SendGrid e-posti teenuse Azure. Näidised kirjutatud C# koodi ja .net-i API kasutamine." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Kuidas saata meili SendGrid Azure


## <a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas sooritada Azure'i programmeerimise põhitoimingud SendGrid e-posti teenus. Näidiste kirjutada c\#
ja .net-i API kasutamine. Stsenaariumid, mis hõlmab kaasata **ehitamine e-posti**, **e-posti saatmine**, **manuste lisamist**ja **filtrite abil**. SendGrid ja e-posti saatmise kohta leiate lisateavet jaotisest [järgmised toimingud][] .

## <a name="what-is-the-sendgrid-email-service"></a>Mis on SendGrid e-posti teenus?

SendGrid on [pilvepõhist e-posti teenus] , mis pakub usaldusväärne [selgituseks meilisõnumite kohaletoimetamise], skaleeritavus ja reaalajas analytics koos paindlik API-d, mis muudavad kohandatud integreerimine lihtne. Levinud SendGrid kasutamise stsenaariumid on järgmised.

-   Automaatne teatiste saatmise klientidele.
-   Haldamise jaotuse loetletud saatmise kliendid igakuine e-fliers ja pakkumisi.
-   Reaalajas mõõdikute asjad blokeeritud e-posti ja klientide tundlikkuse kogumiseks.
-   Tuvastage trendide aruannete loomiseks.
-   Ümbersuunamine klientide päringud.
-   Sissetulevate meilisõnumite töötlemist.

Lisateavet leiate teemast [https://sendgrid.com](https://sendgrid.com) või meie [C# Raamatukogu][sendgrid-csharp]

## <a name="create-a-sendgrid-account"></a>SendGrid konto loomine

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Viide SendGrid .NET klassiteek

[SendGrid Nugeti pakett](https://www.nuget.org/packages/Sendgrid) on kõige lihtsam viis SendGrid API ja kõik sõltuvusi rakenduse konfigureerimiseks. Nugeti on kaasas Microsoft Visual Studio 2015, mis on lihtne installida ja uuendada teekide ja tööriistad Visual Studio laiend. 

> [AZURE.NOTE] Nugeti kui teil on varem Visual Studio 2015 Visual Studio versiooni installimiseks külastage [http://www.nuget.org](http://www.nuget.org), ja klõpsake nuppu **Nugeti installida** .

SendGrid Nugeti paketi oma rakenduse installimiseks tehke järgmist.

1.  Saate luua uue projekti.

    ![Uue projekti loomine][create-new-project]

2.  Valige mall.

    ![Malli valimine][select-a-template]

3.  **Solution Exploreris**Paremklõpsake **Viited**ja seejärel klõpsake nuppu **Halda NuGet-paketid**.

4.  Otsige **SendGrid** ja **SendGrid** üksuse valimine loendis.

    ![SendGrid Nugeti pakett][SendGrid-NuGet-package]

5.  **Installige** installimise lõpuleviimiseks nuppu ja seejärel sulgege see dialoogiboksi.

SendGrid's .NET klassiteek nimetatakse **SendGridMail**. See sisaldab järgmist nimeruumid:

-   **SendGridMail** loomise ja meiliüksuste töötamine.
-   **SendGridMail.Transport** **Web/ülejäänud**või **SMTP** -protokolli HTTP 1.1 protokolli abil meilisõnumite saatmiseks.

Järgmine kood nimeruum deklaratsiooni lisada mis tahes C ülaosas\# faili, mida soovite programmiliselt juurde SendGrid e-posti teenus.
.NET Framework nimeruumi, mis on kaasatud, kuna need sisaldavad sageli kasutatavat SendGrid API-de tüübid on **System.Net** ja **System.Net.Mail** .

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Kuidas: luua e-posti

**SendGridMessage** objekti abil saate luua meilisõnumi. Kui sõnumi objekt on loodud, saate seada atribuudid ja meetodid, sealhulgas e-posti saatja, adressaat e-posti, teema ja meilisõnumi kehas.

Järgmises näites näitab, kuidas luua täielikult täidetud e-posti objekti:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Kõik atribuudid ja **SendGrid** tüüp poolt toetatud kohta leiate lisateavet teemast [sendgrid-csharp][] github.

## <a name="how-to-send-an-email"></a>Kuidas: e-posti saatmine

Pärast meilisõnumi loomist saate selle Web API abil esitatud SendGrid saata. Teise võimalusena võite [kasutamine. NET sisseehitatud Raamatukogu](https://sendgrid.com/docs/Code_Examples/csharp.html).

Meilisõnumite saatmise nõuab, et teil on pakkuda oma SendGrid konto mandaadi (kasutajanime ja parooli) või SendGrid API võti. API võti on eelistatud meetod. Kui teil on vaja üksikasjad selle kohta, kuidas konfigureerida API klahvid, külastage meie [dokumentatsioon](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Neid mandaate võib salvestada oma Azure portaali kaudu, klõpsates KONFIGUREERIMINE ja lisamise /-väärtuse paarideks jaotises "rakenduse sätted".

 ![Azure'i rakenduse sätted][azure_app_settings]

 Seejärel võite kasutada neid järgmiselt: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Mandaadi abil:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

Kasutades API võti:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


Järgmised näited kujutavad veebi-API sõnumi saatmiseks tehke järgmist.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Kuidas: manuse lisamine

Manuseid saab lisada sõnumi **AddAttachment** meetodi ja nime ja manustatava faili tee.
Saate lisada mitu manust, helistades seda meetodit, kui kõik failid, mille soovite manustada. Järgmises näites näitab meilisõnumile manuse lisamine:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Saate lisada ka soovitud andmed **voo**manused. Seda saab teha helistades samasse ülaltoodud **AddAttachment**, kuid mille andmete voo ja faili nimi, milles soovite neid nagu sõnumi kuvamine. Sel juhul peate System.IO teeki lisada.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Kuidas: jalused, jälgimine ja analytics rakenduste abil

SendGrid pakub e-posti funktsiooni abil rakendused. Need on sätted, mida saate lisada meilisõnumi lubamiseks teatud funktsioone, näiteks Jälitus nuppu, Google Analyticsi, tellimuse jälgimine, jne. Rakenduste täieliku loendi leiate teemast [Rakenduse sätted][].

Rakenduste saab rakendada **SendGrid** meilisõnumite abil meetodid, **SendGrid** klassi osana rakendada.

Järgmised näited demonstreerivad seda jaluse ja klõpsake nuppu jälitus filtrid.

### <a name="footer"></a>Jalus

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Klõpsake nuppu jälitus

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Kuidas: lisateenuse SendGrid kasutamine

SendGrid pakub veebipõhine API-d ja täiendavate SendGrid funktsioonide Azure rakenduse laiemaks kasutatavad webhooks. Üksikasjadega tutvumiseks vaadake [SendGrid API dokumentatsiooni][].

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid SendGrid e-posti teenus, järgige neid linke lisateabe.

*   SendGrid C\# teegi repo: [sendgrid-csharp][]
*   SendGrid API dokumentatsiooni: <https://sendgrid.com/docs>
*   Azure'i klientidele Eripakkumine SendGrid: [https://sendgrid.com](https://sendgrid.com)

  [Järgmised sammud]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [Rakenduse sätted]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API dokumentatsioon]: https://sendgrid.com/docs
  
  [pilvepõhine e-posti teenus]: https://sendgrid.com/email-solutions
  [Selgituseks meilisõnumite kohaletoimetamise]: https://sendgrid.com/transactional-email
 
