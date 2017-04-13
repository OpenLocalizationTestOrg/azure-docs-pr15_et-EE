<properties 
    pageTitle="Teie kohapealse identiteetide integreerimine Azure Active Directory."
    description="See on selle Azure'i AD-ühenduse kirjeldav, mis see on ja miks te seda kasutada."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Koosteüksuse Mitmikautentimise kohandatud rakendustesse (SDK)

> [AZURE.IMPORTANT]  Kui soovite alla laadida SDK, peate loomine Azure mitmekordne Auth pakkuja isegi juhul, kui teil on Azure MFA, AAD Premium või EMS litsentsid.  Kui loote Azure mitmekordne Auth pakkuja selleks ja litsentside juba olemas, on vajalik luua pakkuja mudeliga **Lubatud kasutaja kohta** ja kausta, mis sisaldab Azure'i MFA, Azure AD Premium või EMS litsentside pakkuja link.  See tagab, et teil on arve, v.a juhul, kui teil on kordumatu kasutajaid, kui te oma litsentside arv SDK abil.

Funktsiooni Azure'i mitme teguri autentimist tarkvara Kit (SDK) võimaldab teil koostada telefonikõne ja teksti sõnumi kinnitamine otse sisselogimise või tehingu protsesside rakenduste oma Azure AD rentniku.

Mitme teguri autentimist SDK on saadaval C#, Visual Basic (.net-i), Java, Perl, PHP ja Ruby. SDK pakub õhuke ümbris mitmikautentimise ümber. See sisaldab kõike, mida on vaja kirjutage oma kood, sh kommenteeritud lähtefailid kood, näiteks failid ja üksikasjalik ReadMe-faili. Iga SDK sisaldab ka soovitud serti ja krüptimise tehinguid kordumatu pakkuja mitme teguri autentimist. Kui teil on mõni pakkuja, saate alla laadida nii palju keeled ja vormingud SDK kui vaja.

API-de mitmekordne autentimine SDK struktuuri on üsna lihtne. Tehke ühe funktsiooni API mitmekordne suvand parameetritega, nt kinnitamine režiimi ja kasutajaandmeid, nt kõne telefoninumber või PIN-koodi valideerimiseks helistada. Funktsiooni API-de tõlkida funktsioonikutse sisse web services nõuab pilvepõhist Azure'i mitmekordne autentimisteenus. Kõnede peab sisaldama privaatne sert, mida iga SDK sisaldab viide.

Kuna selle API-d ei saa kasutada kasutajatele registreeritud Azure Active Directory, peate sisestama kasutajateabe, nt telefoninumbri ja PIN-koode, faili või andmebaasi. Samuti on API-de ei paku liitumise või kasutaja halduse funktsioonide nii, et peate need protsessid luua rakenduse.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Azure'i Mitmikautentimise SDK allalaadimine

Azure'i mitmekordne SDK allalaadimine nõuab [Azure mitmekordne Auth pakkuja](multi-factor-authentication-get-started-auth-provider.md).  Selleks on vaja täielikku Azure'i tellimus, isegi siis, kui Azure'i MFA, Azure AD Premium või ettevõtte mobiilsus litsentside kuuluvad.  SDK allalaadimiseks peate mitme teguri haldusportaali liikuge hallata mitut tegurit Auth pakkuja otse või MFA sätete lehel linki **"Go to portaali"** .


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Azure'i mitmekordne autentimine SDK alla Azure'i portaal


1. Azure'i portaali administraatorina sisse logima.
2. Valige vasakul Active Directory.
3. Active Directory lehe ülaservas nuppu **Mitmekordne Auth pakkujad**
4. Kuva allosas nuppu **haldamine**
5. See avab uue lehe.  Klõpsake vasakul allosas nuppu SDK.
<center>![Laadi alla](./media/multi-factor-authentication-sdk/download.png)</center>
6. Valige soovitud keel ja klõpsake ühele seotud linke.
7. Salvestage allalaadimine.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Allalaadimiseks Azure'i mitmekordne autentimine SDK teenusesätete lehe kaudu


1. Azure'i portaali administraatorina sisse logima.
2. Valige vasakul Active Directory.
3. Topeltklõpsake oma Azure AD eksemplari.
4. Klõpsake ülaosas nuppu **Konfigureeri.**
5. Valige jaotises mitmikautentimise **haldamine haldusteenuste sätete**
![allalaadimine](./media/multi-factor-authentication-sdk/download2.png)
6. Teenuste sätete lehel ekraani allservas nuppu, **minge portaali**.
![Laadi alla](./media/multi-factor-authentication-sdk/download3a.png)
7. See avab uue lehe.  Klõpsake vasakul allosas nuppu SDK.
8. Valige soovitud keel ja klõpsake ühele seotud linke.
9. Salvestage allalaadimine.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Azure'i Mitmikautentimise SDK sisu
Sees SDK leiate järgmised üksused:

- **README**. Selgitab, kuidas kasutada mitut tegurit autentimise API-de uude või olemasolevasse rakenduses.
- Mitmikautentimise jaoks **andmeallika failid**
- **Kliendi sert** , mida abil saate suhelda Mitmikautentimise teenus
- Serdi **privaatvõti**
- **Helistage tulemused.** Kõne tulemi koodide loend. Faili avamiseks kasutate rakendust vormingut, nt WordPadi tekstiga. Kõne tulemus koodid abil katsetamine ja tõrkeotsing oma rakenduse Mitmikautentimise rakendamine. Neid ei autentimise olek koode.
- **Näited.** Lihtne töö rakendamise Mitmikautentimise proovi kood.


>[AZURE.WARNING]Kliendi sert on eriti teie jaoks loodud kordumatu privaatne sert. Ühiskasutusse anda või selle faili kaotsi minna. See on teie võti suhtluse Mitmikautentimise teenuse turvalisuse tagamiseks.

## <a name="code-sample-standard-mode-phone-verification"></a>Proovi kood: Standardrežiimis kinnitamine

Seda proovi kood kuvatakse selle API-de kasutamine Azure mitmekordne autentimine SDK standardrežiimis kõneposti kõne kontrollimise lisamiseks rakenduse. Standardrežiimis on telefonikõne, mis vastab kasutaja, vajutades klahvi #.

Selles näites kasutatakse C# .NET 2.0 mitmekordne autentimine SDK ASP.net-i taotluse C# serveripoolne loogika, kuid protsess on lihtne muude keelte jaoks üsna sarnased. Kuna SDK sisaldab lähtefailid, mitte käivitatava faili, koostada failid ja viitavad need või need otse rakenduse kaasata.

>[AZURE.NOTE]Mitmikautentimise rakendamisel kasutada muud tegurid teise või kolmanda kontrollimise täiendada teie peamine autentimise meetodit. Need meetodid ei ole mõeldud esmane autentimismeetodite kasutada.

### <a name="code-sample-overview"></a>Koodi valimi ülevaade
Väga lihtne veebirakenduse demo selle proovi kood kasutab telefonikõne vastusega # võtme kasutaja autentimise lõpuleviimiseks. Mitmikautentimise standardrežiimis tuntakse lisatakse see telefonikõne tegurit.

Kliendipoolne kood ei sisalda mis tahes mitmekordne autentimine pivotchartsi konkreetseid elemente. Kuna täiendavat autentimist tegureid ei sõltu esmane autentimine, saate need lisada olemasoleva sisselogimise kasutajaliidese muutmata. API-de mitmekordne SDK võimaldavad teil kohandada, kuid võib pole vaja midagi muuta üldse.

Serveripoolne kood lisab standard-režiimi autentimist kasutava etapp 2. See loob PfAuthParams objekti standard-režiimis kinnitamiseks vajalike parameetritega: kasutajanimi, telefoninumber, ja režiimi ja kliendi sert (CertFilePath), mis on nõutav iga kõne tee. Kõigi parameetrite PfAuthParams tutvustamise, lugege teemat näidisfail SDK.

Järgmiseks kood läheb PfAuthParams objekti pf_authenticate() funktsiooni. Tagastatav väärtus näitab takistavaid autentimine. Välja parameetrid, callStatus ja errorID, sisaldada täiendavaid kõne tulemi teavet. Kõne tulemus koodid on avaldatud SDK kõne tulemuste fail.

See minimaalsete rakendamist saate kirjutada vaid mõned read. Kuid kood, tuleks sisaldavad keerukamaid vigade käsitlemise, täiendavad andmebaasi koodi ja täiustatud kasutaja kogemus.

### <a name="web-client-code"></a>Web kliendi kood

Järgmises web kliendi koodi demo lehele.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Serveripoolne kood

Serveripoolne järgmine kood, Mitmikautentimise on konfigureeritud ja käivitage samm 2. Standardrežiimis (MODE_STANDARD) on telefonikõne, millele vastab kasutaja, vajutades klahvi #.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }
