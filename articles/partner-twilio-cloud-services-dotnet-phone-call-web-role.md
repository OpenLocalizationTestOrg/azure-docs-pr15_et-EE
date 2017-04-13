<properties 
    pageTitle="Kuidas helistada Twilio (.NET) | Microsoft Azure'i" 
    description="Saate teada, kuidas helistada ja Azure SMS-teenuse Twilio API sõnumi saatmine. .Net-i kirjutatud koodinäiteid." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Kuidas web rolli Azure Twilio abil helistamine

Sellest juhendist näitab, kuidas kasutada Twilio helistamiseks majutatud Azure veebilehelt. Tulemiks oleva rakenduse kasutajalt telefonikõne väärtused, nagu on näidatud järgmises kuvatõmmis.

![Azure'i kõne vormi Twilio ja ASP.net-i abil][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Eeltingimused

Peate tegema järgmist kasutada koodi selles teemas:

1. Hankida Twilio konto ja autentimise sümboolne. Twilio alustada registreeruda aadressil [https://www.twilio.com/try-twilio][try_twilio]. Saate hinnata hindade [http://www.twilio.com/pricing][twilio_pricing]. Esitatud Twilio API kohta leiate teemast [http://www.twilio.com/voice/api][twilio_api].
2. Oma web rolli Twilio .net-i teeki lisada. Vt käesoleva teema "Twilio teekide lisamiseks web rolli projekti".

Teil tuleb tuttav Azure basic web rolli loomine.

## <a name="howtocreateform"></a>Kuidas: jaoks helistamine veebivormi loomine

<a id="use_nuget"></a>Web rolli projekti Twilio teekide lisamiseks tehke järgmist.

1.  Avage oma lahenduse Visual Studios.
2.  Paremklõpsake **Viited**.
3.  Klõpsake nuppu **Halda NuGet-paketid**.
4.  Klõpsake nuppu **võrgus**.
5.  Tippige väljale Otsi online *twilio*.
6.  Klõpsake Twilio paketi **installimine** .

Järgmine kood näitab, kuidas kasutaja andmete allalaadimiseks helistamine veebivormi loomine. Selles näites on loodud ASP.net-i web rolli nimega **TwilioCloud** .

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Kuidas: helistamiseks koodi loomine
Järgmine kood, mida nimetatakse, kui kasutaja on lõpule jõudnud vorm, loob kõne sõnum ja koostab kõne. Selles näites kood käivitatakse nupu onclick sündmuseohjuri vormil. (Kasutage Twilio konto ja autentimise sümboolne asemel kohatäite väärtused määratud **accountSID** ja **authToken** kood allpool.)

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

Kõne ja Twilio lõpp-punkti, API versioon ja kõne olek kuvatakse. Järgmine pilt kuvatakse väljund valimi Käivita.

![Azure'i kõne vastuse Twilio ja ASP.net-i abil][twilio_dotnet_basic_form_output]

Lisateavet TwiML kohta leiate [http://www.twilio.com/docs/api/twiml][twiml]. Lisateavet &lt;öelda&gt; ja muud Twilio verbide leiate [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Järgmised sammud
Järgmine kood esitatud Twilio kasutamine Azure web rolli ASP.net-i põhifunktsioone kuvamiseks. Enne juurutamist Azure valmistamisel, soovite lisada veel tõrge käsitsemise või muid funktsioone. Näiteks:

* Veebivormi asemel võite kasutada Azure'i bloobimälu või Azure'i SQL-andmebaasi eksemplari salvestada telefoninumbrid ja kõne teksti. Azure plekid kasutamise kohta leiate teemast [kasutamine Azure'i bloobimälu salvestusteenus .NET-is][howto_blob_storage_dotnet]. SQL-andmebaasi kasutamise kohta leiate teemast [Azure SQL-i andmebaasi .NET rakenduste kasutamise kohta][howto_sql_azure_dotnet].
* Võite kasutada RoleEnvironment.getConfigurationSettings too Twilio konto ID ja autentimise luba oma juurutuse konfigureerimine sätted asemel suur-kodeerimise teie vormi väärtusi. Klassi RoleEnvironment kohta leiate teemast [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Lugege veebisaidil [https://www.twilio.com/docs/security]Twilio turvalisuse juhised[twilio_docs_security].
* Lugege lisateavet Twilio veebisaidil [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Vt ka
* [Kuidas kasutada Twilio heli- ja SMS-võimalustega Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
