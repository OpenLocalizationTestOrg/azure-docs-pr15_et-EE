<properties 
    pageTitle="Twilio kasutamine, kõneposti ja SMS (Java) | Microsoft Azure'i" 
    description="Saate teada, kuidas helistada ja Azure SMS-teenuse Twilio API sõnumi saatmine. Koodinäiteid kirjutatud Java." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Hääl-ja SMS-võimalustega Java Twilio kasutamise kohta

Sellest juhendist näitab, kuidas sooritada programmeerimise põhitoimingud Twilio API teenuse Azure. Stsenaariumid, mis hõlmab sisaldavad Helistamine ja lühike tekstsõnum (SMS) sõnumi saatmist. Twilio ja kõneposti ja SMS kasutamine rakenduste kohta leiate lisateavet jaotisest [Järgmised toimingud](#NextSteps) .

## <a id="WhatIs"></a>Mis on Twilio?
Twilio on telefoniside veebiteenuse API, mis võimaldab koostada heli- ja SMS rakenduste olemasoleva web keeled ja oskuste abil. Twilio on kolmanda osapoole teenusele (mitte Azure funktsioon ja pole Microsofti toodete).

**Twilio Voice** võimaldab rakenduste ja kõnesid vastu võtta. **Twilio SMS** võimaldab teha ja vastu võtta SMS-sõnumite rakenduste. **Twilio klient** , mis võimaldab rakenduste lubamiseks kõneposti teatise abil olemasoleva Interneti-ühendused, sh mobiilsideseadmete ühendused.

## <a id="Pricing"></a>Twilio hinnad ja pakkumised
Teavet Twilio hinnad on saadaval veebisaidil [Twilio hinnad] [twilio_pricing]. Azure'i kliendid saavad on [Eripakkumine][special_offer]: tasuta krediitkaardiga 1000 tekstide või 1000 sissetulev minutit. Kasutajaks registreerumine selle pakkumise või lisateabe saamiseks külastage [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Mõisted
Twilio API on rahulik API, mis pakub kõneposti ja SMS funktsionaalsus rakenduste. Kliendi teegid on saadaval mitmes keeles; loendi leiate teemast [Teekide Twilio API] [twilio_libraries].

Võtme aspektide Twilio API on Twilio verbide ja Twilio märgistuskeel (TwiML).

### <a id="Verbs"></a>Twilio verbide
API kasutab Twilio verbide; Näiteks kuvatakse ** &lt;öelda&gt; ** tegusõna juhendab Twilio kuuldavalt esitamisel sõnumi telefonil. 

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

### <a id="TwiML"></a>TwiML
TwiML on XML-põhine juhiste põhjal Twilio verbide, mis teavitab Twilio, kuidas protsess kõne või SMS.

Näiteks soovite järgmist TwiML **Tere, maailm** teksti teisendamine kõneks.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Kui teie rakendus nõuab Twilio API, üks API parameetrid on URL, mis tagastab TwiML vastus. Arengu eesmärgil saate Twilio antud URL-ide esitada rakenduste kasutatava TwiML vastuseid. Võib ka majutate oma URL-id, andes TwiML vastuste ja teine võimalus on kasutada **TwiMLResponse** objekti.

Twilio verbide, nende atribuutide ja TwiML kohta leiate lisateavet teemast [TwiML] [twiml]. Twilio API kohta lisateabe saamiseks lugege teemat [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Twilio konto loomine
Kui olete valmis saada Twilio konto, [Proovige Twilio]veebisaidil registreeruda [try_twilio]. Saate alustada tasuta konto ja uuendada oma konto hiljem.

Kui te registreerute Twilio konto, saate konto ID-d ja mõne autentimise luba. Mõlemad on vaja Twilio API kõnede tegemiseks. Oma kontole volitamata juurdepääsu vältimiseks turvaliseks oma autentimise luba. Oma konto ID ja autentimise [Twilio kontole leht]vaadatav luba [twilio_account], väljade nimega **konto SID** ja **AUTH TURBELOA**,.

## <a id="create_app"></a>Java rakenduse loomine
1. Saada Twilio JAR ja lisage see oma Java Koosta tee ja oma sõda juurutamise komplekti. Veebisaidil [https://github.com/twilio/twilio-java][twilio_java], saate alla laadida GitHub allikad ja luua oma JAR või allalaadimine valmismallide JAR (koos või ilma sõltuvused).
2. Veenduge, et teie JDK **cacerts** võtmehoidjas sisaldab Equifax Secure sertimiskeskus serdi MD5 sõrmejälje 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (järjenumbri 35:DE:F4:CF ja SHA1 sõrmejälje on D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). See on serdi sertimiskeskuse (CA) sert [https://api.twilio.com] [ twilio_api_service] teenus, mida nimetatakse Twilio API-de kasutamisel. Lisateavet selle kohta, kuidas tagada teie JDK **cacerts** võtmehoidjas sisaldab õige CA sert, vt [Java CA serdi pood serdi lisamine][add_ca_cert].

Üksikasjalikud juhised abil Twilio kliendi teek Java on saadaval, kuidas [telefonikõne abil Twilio Java rakenduse Azure][howto_phonecall_java].

## <a id="configure_app"></a>Rakenduse kasutamiseks Twilio teekide konfigureerimine
Oma koodi saate lisada laused **importida** oma lähtefailid Twilio pakettide või tunnid, mida soovite kasutada oma rakenduse ülaosas. 

Java allikas failide:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Andmeallika jaoks Java Server Page (JSP) failid:

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Sõltuvalt sellest, millist Twilio pakettide või tunnid, mida soovite kasutada, võib teie **importimine** laused.

## <a id="howto_make_call"></a>Kuidas: väljamineva helistamine
Järgmine näitab, kuidas abil **CallFactory** klassi kutset. Järgmine kood kasutab ka Twilio antud saidi vastuse Twilio märgistuskeel (TwiML) tagastamiseks. Asendada oma väärtused **alates** ja **kuni** telefoninumbrid ja veenduge, et kontrollida telefoninumber **:** enne koodi Twilio konto.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

**CallFactory.create** meetodiga sisse parameetrite kohta leiate lisateavet teemast [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Nagu mainitud, on järgmine kood kasutatakse esitatud Twilio saidi TwiML vastus. Saate selle asemel kasutada oma saidi TwiML vastust; Lisateavet leiate teemast [pakuvad TwiML vastuste Azure'i Java rakenduses](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Kuidas: SMS saatmine
Pärast näitab **SmsFactory** klassi SMS-sõnumi saatmine. Funktsiooni **kaudu** arv, **4155992671**, on esitatud Twilio prooviversiooni kontode SMS-sõnumeid saata. **Et** telefoninumber olema kinnitatud enne koodi Twilio konto jaoks.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
**SmsFactory.create** meetodiga sisse parameetrite kohta leiate lisateavet teemast [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Kuidas: sisestage oma veebisaidi TwiML vastused
Kui teie rakendus käivitab Twilio API kõne, näiteks **CallFactory.create** meetodil, Twilio saadab oma päringu URL, mis on oodata TwiML vastuse. Ülaltoodud näites Twilio antud URL-i [http://twimlets.com/message][twimlet_message_url]. (Ajal TwiML on mõeldud kasutamiseks veebiteenused, saate vaadata selle TwiML brauseris. Näiteks klõpsake [http://twimlets.com/message] [ twimlet_message_url] kuvamiseks tühja ** &lt;vastuse&gt; ** elemendi; Teine näide klõpsake [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] kuvamiseks on ** &lt;vastuse&gt; ** element, mis sisaldab soovitud ** &lt;öelda&gt; ** element.)

Selle asemel Twilio antud URL-i, saate luua oma saidi URL-i, mis tagastab HTTP vastused. Saate luua saidi mis tahes keeleks, mis tagastab HTTP vastused; selles teemas eeldab, et teil tuleb majutusteenuse JSP lehe URL.

Järgmisel leheküljel JSP tulemuseks TwiML vastuse, mis ütleb **Tere, maailm** kõnes.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

Järgmisel leheküljel JSP tulemuseks TwiML vastuse, mis ütleb teksti, on mitu pausid ja ütleb teabe Twilio API versioon ja Azure rolli nimi.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

**ApiVersion** parameeter on saadaval Twilio voice taotlusi (mitte SMS taotluste). Saadaval taotluse parameetrid Twilio heli- ja SMS taotluste vaatamiseks vt <https://www.twilio.com/docs/api/twiml/twilio_request> ja <https://www.twilio.com/docs/api/twiml/sms/twilio_request>. Muutuja **RoleName** keskkond, mis on saadaval ka Azure juurutamise käigus. (Kui soovite lisada kohandatud keskkonna muutujate, et nad võivad soovite **System.getenv**, vt keskkonna muutujate [mitmesugused rolli konfiguratsiooni]sätteid[misc_role_config_settings].)

Kui olete oma JSP lehe häälestatud pakuvad TwiML vastused, kasutage JSP lehe URL URL-i **CallFactory.create** meetod on möödas. Näiteks kui teil on nimega veebirakenduse juurutatud mõne Azure'i MyTwiML majutatud teenus ja JSP lehe nimi on mytwiml.jsp, URL-i saab edasi **CallFactory.create** , nagu on näidatud järgmises:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Teine võimalus vastamise koos TwiML on **TwiMLResponse** ainekursust, mis on saadaval **com.twilio.sdk.verbs** paketi kaudu.

Azure Java Twilio kasutamise kohta lisateabe saamiseks vaadake, [Kuidas teha telefonikõne abil Twilio Java rakenduse Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Kuidas: täiendavad Twilio teenuste kasutamine
Lisaks näiteid siin, Twilio pakub veebipõhine API abil saate kasutada Twilio lisafunktsioone Azure rakenduse kaudu. Üksikasjadega tutvumiseks vaadake [Twilio API dokumentatsiooni] [twilio_api_documentation].

## <a id="NextSteps"></a>Järgmised sammud
Nüüd, kui olete õppinud põhitõdesid Twilio teenuse, järgige neid linke Lisateavet:

* [Twilio turvalisuse juhised] [twilio_security_guidelines]
* [Twilio juhendid ja näide kood] [twilio_howtos]
* [Twilio Kiirjuhend õppetükid][twilio_quickstarts] 
* [Twilio github] [twilio_on_github]
* [Rääkige Twilio tugi] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
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
