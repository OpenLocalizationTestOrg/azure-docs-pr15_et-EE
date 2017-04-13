<properties 
    pageTitle="Kuidas Twilio (Java) kaudu helistamine | Microsoft Azure'i" 
    description="Saate teada, kuidas helistada veebilehelt Twilio Azure Java rakenduse kasutamine." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Kuidas Java rakenduses Azure'i Twilio abil helistamine 

Järgmises näites kuvatakse kasutamist Twilio helistamiseks majutatud Azure veebilehelt. Tulemuseks taotlus küsib kasutaja telefonikõne väärtusi, nagu on näidatud järgmisel kuvatõmmisel kuvatõmmis.

![Azure'i kõne vormi Twilio ja Java abil][twilio_java]

Peate tegema järgmist kasutada koodi selles teemas.

1. Hankida Twilio konto ja autentimise sümboolne. Twilio alustada hinnata hindade [http://www.twilio.com/pricing][twilio_pricing]. Soovi korral saate veebisaidil [https://www.twilio.com/try-twilio][try_twilio]. Esitatud Twilio API kohta leiate teemast [http://www.twilio.com/api][twilio_api].
2. Hankige Twilio JAR. Veebisaidil [https://github.com/twilio/twilio-java][twilio_java_github], saate alla laadida GitHub allikad ja luua oma JAR või allalaadimine valmismallide JAR (koos või ilma sõltuvused).
Selles teemas kood on kirjutatud valmismallide TwilioJava-3.3.8-ja-sõltuvused JAR abil.
3. Saate lisada oma Java Koosta tee purki.
4. Kui kasutate selle Java rakenduse loomiseks Eclipse, kaasake Twilio JAR rakenduse juurutamine faili (sõja) Eclipse's juurutamise komplekti funktsiooni abil. Kui te ei kasuta Eclipse selle Java rakenduse loomiseks, veenduge, et Twilio JAR on sama Azure rolli Java rakenduse kuuluvad ja lisatakse klassi tee rakenduse.
5. Veenduge, et teie cacerts võtmehoidjas sisaldab Equifax Secure sertimiskeskus serdi MD5 sõrmejälje 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (järjenumber on 35:DE:F4:CF ja SHA1 sõrmejälje D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). See on serdi sertimiskeskuse (CA) sert [https://api.twilio.com] [ twilio_api_service] teenus, mida nimetatakse Twilio API-de kasutamisel. See CA serdi lisamine oma JDK cacert poe kohta leiate teavet teemast [Java CA serdi talletamiseks serdi lisamine][add_ca_cert].

Lisaks tundmine teabe [loomise Tere maailma rakenduse kasutades Azure tööriistakomplekt Eclipse][azure_java_eclipse_hello_world], või teiste meetodite abil hosting Java rakendusi Azure'is, kui te ei kasuta Eclipse, kus on soovitatav.

## <a name="create-a-web-form-for-making-a-call"></a>Helistamine jaoks veebivormi loomine

Järgmine kood näitab, kuidas kasutaja andmete allalaadimiseks helistamine veebivormi loomine. Selles näites huvides on loodud uue dünaamilise projekti nimega **TwilioCloud**, ja **callform.jsp** lisati JSP failina.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Helistamiseks koodi loomine
Järgmine kood, mida nimetatakse, kui kasutaja on lõpule jõudnud vormil kuvatud callform.jsp kõne sõnumi ja loob kõne. Selles näites selleks, JSP fail nimega **makecall.jsp** ja lisatud **TwilioCloud** projekt. (Kasutage Twilio konto ja autentimise sümboolne asemel kohatäite väärtused määratud **accountSID** ja **authToken** kood allpool.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Lisaks helistamise, kuvab makecall.jsp Twilio lõpp-punkti, API versioon ja kõne olek. Näide on järgmine pilt:

![Azure'i kõne vastuse Twilio ja Java abil][twilio_java_response]

## <a name="run-the-application"></a>Käivitage rakendus
Järgnevalt on üksikasjalik juhiseid oma rakenduse; üksikasjade jaoks neid juhiseid leiate [loomise Tere maailma rakenduse kasutades Azure tööriistakomplekt Eclipse][azure_java_eclipse_hello_world].

1. Eksportida oma TwilioCloud sõda Azure **approot** kausta. 
2. Muutke oma TwilioCloud sõda lahti **startup.cmd** .
3. Teie taotlus Arvuta emulaator kompileerida.
4. Käivitage oma juurutuse Arvuta emulaator.
5. Avage brauser ja käivitage **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Vormi väärtuste sisestamiseks, klõpsake **selle helistada**ja seejärel vaadata makecall.jsp tulemusi.

Kui olete valmis kasutama Azure'i Kompileeri juurutamiseks pilveteenusesse, juurutada Azure ja http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp töötavad brauseris (asendada oma *your_hosted_name*väärtus).

## <a name="next-steps"></a>Järgmised sammud
Järgmine kood esitatud Twilio Java kasutamine Azure põhifunktsioonide kuvamiseks. Enne juurutamist Azure valmistamisel, soovite lisada veel tõrge käsitsemise või muid funktsioone. Näiteks:

* Veebivormi asemel võite kasutada Azure storage plekid või SQL-andmebaasi salvestada telefoninumbrid ja kõne teksti. Azure'i salvestusruumi plekid Java kasutamise kohta leiate teavet teemast [bloobimälu salvestusteenus Java kaudu kasutada][howto_blob_storage_java]. SQL-andmebaasi Java kasutamise kohta leiate teemast [Abil SQL-i andmebaasi Java][howto_sql_azure_java].
* Võite kasutada **RoleEnvironment.getConfigurationSettings** too Twilio konto ID ja autentimise luba oma juurutuse konfigureerimine sätted asemel suur-kodeerimine makecall.jsp väärtused. Klassi **RoleEnvironment** kohta leiate teemast [Azure teenuse käitusaja teegi JSP] [ azure_runtime_jsp] ja Azure teenuse käitusaja paketi dokumentatsiooni [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Makecall.jsp koodi määrab antud Twilio URL [http://twimlets.com/message][twimlet_message_url], et **URL-i** muutuja. Seda URL-i pakub Twilio märgistuskeel (TwiML) vastust, mis teavitab Twilio kõne jätkamiseks tehke järgmist. Näiteks võib sisaldada TwiML, mis tagastatakse on ** &lt;öelda&gt; ** tegusõna, mis annab tulemuseks teksti esituse kõne adressaadile. URL-ide Twilio asemel võib koostada oma teenuse vastata Twilio isiku taotlus; Lisateabe saamiseks vaadake, [Kuidas kasutada Twilio hääl-ja SMS-võimalustega Java][howto_twilio_voice_sms_java]. Lisateavet TwiML kohta leiate [http://www.twilio.com/docs/api/twiml][twiml], ja rohkem teavet ** &lt;öelda&gt; ** ja muud Twilio verbide leiate [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lugege veebisaidil [https://www.twilio.com/docs/security]Twilio turvalisuse juhised[twilio_docs_security].

Twilio kohta lisateabe saamiseks lugege teemat [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Vt ka
* [Kuidas kasutada Twilio heli- ja SMS-võimalustega Java][howto_twilio_voice_sms_java]
* [Serdi lisamine Java CA serdi pood][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
