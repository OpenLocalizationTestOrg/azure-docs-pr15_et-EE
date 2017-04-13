<properties 
    pageTitle="Store-sendgrid-Java-How-to-Send-email-example" 
    description="Kuidas saata meili kaudu Java SendGrid on Azure juurutamine" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Kuidas saata meili kaudu Java SendGrid on Azure juurutamine

Järgmises näites kuvatakse kasutamist SendGrid majutatud Azure veebilehelt meilisõnumite saatmiseks. Tulemuseks taotlus küsib kasutaja e-posti väärtusi, nagu on näidatud järgmisel kuvatõmmisel kuvatõmmis.

![E-posti vorm][emailform]

Selle tulemusena tekkivad e-posti näeb umbes järgmine pilt.

![Meilisõnum][emailsent]

Peate tegema järgmist kasutada koodi selles teemas.

1. Saada javax.mail purgid, näiteks <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Lisage oma Java purgid koostada tee.
3. Kui kasutate selle Java rakenduse loomiseks Eclipse, saate kaasata SendGrid teekide rakenduse juurutamine faili (sõja) Eclipse's juurutamise komplekti funktsiooni abil. Kui te ei kasuta Eclipse selle Java rakenduse loomiseks, veenduge, et teekide kuuluvad sama Azure rolli Java rakenduse ja lisatakse rakenduse tunni tee.


Peate ka oma SendGrid kasutajanime ja parooli, saate meilisõnumi saata. Alustada SendGrid, vaadake, [Kuidas saata meili kaudu Java SendGrid](store-sendgrid-java-how-to-send-email.md).

Lisaks tuttava teabe [loomise Tere maailma taotluse Azure Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944)või teiste meetodite abil hosting Java rakendusi Azure'is, kui te ei kasuta Eclipse, on soovitatav.

## <a name="create-a-web-form-for-sending-email"></a>E-posti saatmise veebivormi loomine

Järgmine kood näitab, kuidas luua veebivormi saatmiseks e-posti kasutaja andmeid tuua. Selleks selle sisu, JSP faili nimi **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Koodi saata meilisõnumi loomine

Järgmine kood, mida nimetatakse kui täidate emailform.jsp vorm, loob meilisõnumi ja saadab. Selleks selle sisu, JSP faili nimi **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Lisaks meili saatmisel emailform.jsp annab tulemuseks kasutaja; näide on järgmine pilt:

![Saada e-posti tulem][emailresult]

## <a name="next-steps"></a>Järgmised sammud

Arvuta emulaator rakenduse juurutamine ja sees brauseri käivitamine emailform.jsp, vormi väärtuste sisestamiseks, klõpsake **selle meilisõnumeid saata**ja seejärel tulemeid sendemail.jsp.

Järgmine kood esitati näitab teile, kuidas kasutada SendGrid Java Azure. Enne juurutamist Azure valmistamisel, soovite lisada veel tõrge käsitsemise või muid funktsioone. Näiteks: 

* Võite kasutada Azure storage plekid või SQL-andmebaasi meiliaadressid ja meilisõnumite veebivormi kasutamise asemel salvestada. Azure'i salvestusruumi plekid Java kasutamise kohta leiate artiklist [Java bloobimälu salvestusruumi teenuse kasutamise kohta](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). SQL-andmebaasi Java kasutamise kohta leiate artiklist [Java SQL-i andmebaasi abil](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Võite kasutada `RoleEnvironment.getConfigurationSettings` toomiseks oma juurutuse Otsingukonfiguratsiooni sätetest asemel tuua neid väärtusi veebivormi SendGrid kasutajanimi ja parool. Lisateavet selle `RoleEnvironment` klassi leiate [Azure'i teenus käitusaja teegi JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) ja Azure teenuse käitusaja paketi dokumentatsiooni <http://dl.windowsazure.com/javadoc>.
* Java SendGrid kasutamise kohta leiate lisateavet teemast [abil SendGrid Java kaudu meilisõnumeid saata](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
