<properties 
    pageTitle="Twilio kasutamine, kõneposti ja SMS (Ruby) | Microsoft Azure'i" 
    description="Saate teada, kuidas helistada ja Azure SMS-teenuse Twilio API sõnumi saatmine. Koodinäiteid Ruby kirjutada." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Kuidas kasutada Twilio heli- ja SMS-võimalustega Ruby
Sellest juhendist näitab, kuidas sooritada programmeerimise põhitoimingud Twilio API teenuse Azure. Stsenaariumid, mis hõlmab sisaldavad Helistamine ja lühike tekstsõnum (SMS) sõnumi saatmist. Twilio ja kõneposti ja SMS kasutamine rakenduste kohta leiate lisateavet jaotisest [Järgmised toimingud](#NextSteps) .

## <a id="WhatIs"></a>Mis on Twilio?
Twilio on telefoniside veebiteenuse API, mis võimaldab koostada heli- ja SMS rakenduste olemasoleva web keeled ja oskuste abil. Twilio on kolmanda osapoole teenusele (mitte Azure funktsioon ja pole Microsofti toodete).

**Twilio Voice** võimaldab rakenduste ja kõnesid vastu võtta. **Twilio SMS** võimaldab teha ja vastu võtta SMS-sõnumite rakenduste. **Twilio klient** , mis võimaldab rakenduste lubamiseks kõneposti teatise abil olemasoleva Interneti-ühendused, sh mobiilsideseadmete ühendused.

## <a id="Pricing"></a>Twilio hinnad ja pakkumised
Teavet Twilio hinnad on saadaval veebisaidil [Twilio hinnad] [twilio_pricing]. Azure'i kliendid saavad on [Eripakkumine][special_offer]: tasuta krediitkaardiga 1000 tekstide või 1000 sissetulev minutit. Kasutajaks registreerumine selle pakkumise või lisateabe saamiseks külastage [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Mõisted
Twilio API on rahulik API, mis pakub kõneposti ja SMS funktsionaalsus rakenduste. Kliendi teegid on saadaval mitmes keeles; loendi leiate teemast [Teekide Twilio API] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML on sellest, kuidas protsess kõne Twilio või SMS XML-põhiste juhiste kogum.

Näiteks soovite järgmist TwiML **Tere, maailm** teksti teisendamine kõneks.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Kõik TwiML dokumendid `<Response>` oma root element. Sealt saate kasutada Twilio verbide määratleda rakenduse toimimist.

### <a id="Verbs"></a>TwiML verbide
Twilio verbide on XML-sildid, mis ütlevad Twilio mida **teha**. Näiteks kuvatakse ** &lt;öelda&gt; ** tegusõna juhendab Twilio kuuldavalt esitamisel sõnumi telefonil. 

Järgmises loendis Twilio verbide.

* ** &lt;Sissehelistamise&gt;**: helistaja ühendub teises telefonis.
* ** &lt;Kogumine&gt;**: kogub telefoni numbriklahvistikul sisestatud kahekohalise arvuna.
* ** &lt;Lahutamise&gt;**: kõne lõpeb.
* ** &lt;Esitada&gt;**: esitab helifaili.
* ** &lt;Paus&gt;**: vaikselt ootab määratud sekundite arv.
* ** &lt;Kirje&gt;**: kirjete funktsiooni helistaja ja tagastab URL-i faili, mis sisaldab salvestise.
* ** &lt;Ümber suunata&gt;**: annab juhtimise telefonikõne või SMS TwiML erinevate URL.
* ** &lt;Kõnest&gt;**: sissetuleva kõne oma Twilio arvuks hülgab ilma arveldus saate
* ** &lt;Öelda&gt;**: teisendab teksti kõneks, mis tehakse telefonil.
* ** &lt;Sms&gt;**: saadab sõnumi.

Twilio verbide, nende atribuutide ja TwiML kohta leiate lisateavet teemast [TwiML] [twiml]. Twilio API kohta lisateabe saamiseks lugege teemat [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Twilio konto loomine
Kui olete valmis saada Twilio konto, [Proovige Twilio]veebisaidil registreeruda [try_twilio]. Saate alustada tasuta konto ja uuendada oma konto hiljem.

Kui te registreerute Twilio konto, saate rakenduse telefoni number. Samuti saate konto SID ja mõne auth luba. Mõlemad on vaja Twilio API kõnede tegemiseks. Oma kontole volitamata juurdepääsu vältimiseks turvaliseks oma autentimise luba. [Twilio kontole leht]vaadatav oma konto SID ja auth luba[twilio_account], väljade nimega **konto SID** ja **AUTH TURBELOA**,.

### <a id="VerifyPhoneNumbers"></a>Telefoninumbrite kinnitamine
Lisaks on antud Twilio, saate kontrollida arvud te juhtelemendi (nt mobiiltelefon või Kodutelefon telefoninumbri) rakenduste kasutamiseks. 

Kohta, kuidas kontrollida telefoninumbri leiate teemast [Haldamine arvude] [verify_phone].

## <a id="create_app"></a>Foneetiline rakenduse loomine
Foneetiline rakendus, mis kasutab teenust Twilio ja Azure töötab ei erine muu foneetiline rakendus, mis kasutab teenust Twilio. Ajal Twilio teenused on RESTful ja saab helistada Ruby on mitu võimalust, see artikkel keskendub Twilio teenuste kasutamiseks [Twilio helper]teegiga mis[twilio_ruby].

Esmalt [ülesehituse uue Azure Linux VM] [ azure_vm_setup] host uus foneetiline veebirakenduses tegutseda. Ignoreeri loomiskuupäeva rööpad rakenduse lihtsalt ülesehituse VM seotud juhiseid. Veenduge, et loote lõpp on välised pordi 80 ja sisemise port 5000.

Allpool toodud näidetes, me kasutame [Sinatra][sinatra], väga lihtne web raamistiku mis. Kuid kindlasti saate Twilio helper teegi mis mis tahes muud web raamistik, sh Ruby on Rails.

SSH sisse oma uue VM ja luua uus rakendus. Sees selle kausta fail nimega Gemfile loomine ja kopeerige see järgmine kood:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Käivitage käsureal `bundle install`. See installib ülaltoodud sõltuvused. Looge järgmine fail nimega `web.rb`. Selle isikud, kus asub kood oma veebirakenduse. Kleepige sinna järgmine kood:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Selles etapis peaksite nägema mobiilibrauseris käsk Käivita `ruby web.rb -p 5000`. See on spinnernupu üles väike veebiserver Port 5000. Mida peaks oskama otsige selle rakenduse brauseris URL-i külastades saate ülesehituse oma Azure VM. Kui jõuate oma veebirakenduse brauseris, olete valmis alustama koostamise Twilio rakenduse.

## <a id="configure_app"></a>Rakenduse kasutamiseks Twilio konfigureerimine
Saate konfigureerida oma veebirakenduse kasutada Twilio teeki, värskendades oma `Gemfile` selle rea kaasata:

    gem 'twilio-ruby'

Käivitage käsk real `bundle install`. Nüüd avage `web.rb` ja selle rea ülaservas, sealhulgas:

    require 'twilio-ruby'

Teil on nüüd kõik korras kasutada Twilio helper teeki, mis teie web Appis.

## <a id="howto_make_call"></a>Kuidas: väljamineva helistamine
Järgmine näitab, kuidas kutset. Võtme hõlmavad Twilio helper teek, mis abil saate helistada REST API-ga ja muudab TwiML. Asendada oma väärtused **alates** ja **kuni** telefoninumbrid ja veenduge, et kontrollida telefoninumber **:** enne koodi Twilio konto.

Seda funktsiooni, et lisada `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Kui teie avatud up `http://yourdomain.cloudapp.net/make_call` brauseris, mis käivitab Twilio API kõne telefonikõne teha. Kaks esimest parameetrid `client.account.calls.create` on üsnagi: numbrit kõne on `from` ja number kõne on `to`. 

Kolmas parameeter (`url`) on URL, mida soovite juhiseid, mida teha pärast kõne ühendamist taotleb Twilio. Sellisel juhul me ülesseadmine on URL-i (`http://yourdomain.cloudapp.net`) mis annab lihtsa TwiML dokumendi ja kasutab funktsiooni `<Say>` tegusõna teha mõned kõnesünteesi ja öelda "Tere ahv" kõne vastuvõtja.

## <a id="howto_recieve_sms"></a>Kuidas: saada sõnumi
Eelmises näites me algatatud **telefoni kutse** . See aeg, telefoninumber, millele Twilio andis meile registreerumise käigus kasutame töödelda **sissetuleva** sõnumi.

Esiteks [Twilio armatuurlaua]sisse logida[twilio_account]. Klõpsake ülemisel navigeerimisribal "Numbrid" ja seejärel klõpsake Twilio arvu, mis on teile selleks antud. Kuvatakse kaks URL-id, mida saate konfigureerida. Kõneposti taotluse URL-i ja SMS taotleda URL-i. Need on URL-id, mis Twilio helistab iga kord, kui kõne tehakse või SMS saadetakse teie number. URL-id on tuntud ka kui "web konksud".

Soovime töötlemine sissetulevatele SMS, seega vaatame värskendada URL-i `http://yourdomain.cloudapp.net/sms_url`. Minna, ja klõpsake käsku Salvesta muudatused lehe allosas. Nüüd uuesti sisse `web.rb` vaatame meie taotluse käsitlemiseks see programm:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Pärast muudatuste tegemist, veenduge, et uuesti alustada oma veebirakenduse. Nüüd võta telefonis ja saada SMS oma Twilio number. Saate kohe SMS-vastuse, mis ütleb "kuule, ping Täname! Twilio ja Azure rock! ".

## <a id="additional_services"></a>Kuidas: täiendavad Twilio teenuste kasutamine
Lisaks näiteid siin, Twilio pakub veebipõhine API abil saate kasutada Twilio lisafunktsioone Azure rakenduse kaudu. Üksikasjadega tutvumiseks vaadake [Twilio API dokumentatsiooni] [twilio_api_documentation].

### <a id="NextSteps"></a>Järgmised sammud
Nüüd, kui olete õppinud põhitõdesid Twilio teenuse, järgige neid linke Lisateavet:

* [Twilio turvalisuse juhised] [twilio_security_guidelines]
* [Twilio HowTos ja näide kood] [twilio_howtos]
* [Twilio Kiirjuhend õppetükid][twilio_quickstarts] 
* [Twilio github] [twilio_on_github]
* [Rääkige Twilio tugi] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
