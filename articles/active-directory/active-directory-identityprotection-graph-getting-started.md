<properties
    pageTitle="Azure Active Directory identiteedi kaitse ja Microsoft Graphi kasutamise alustamine | Microsoft Azure'i"
    description="Azure Active Directory pakub loendi risk sündmused ja seotud teabe päringu Microsoft Graphi tutvustus."
    services="active-directory"
    keywords="Azure'i active directory identiteedi kaitse, risk sündmus, haavatavuse turbepoliitika, Microsoft Graphi"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Azure Active Directory identiteedi kaitse ja Microsoft Graphi kasutamise alustamine

Microsoft Graphi on Microsofti ühendatud API lõpp-punkti ja [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md) API-de Avaleht. Meie esimese API **identityRiskEvents**, võimaldab päringu Microsoft Graphi [risk sündmused](active-directory-identityprotection-risk-events-types.md) ja seotud teabe loendit. Selles artiklis tutvustab teile selle API päringud. Põhjalikuks Sissejuhatus, kõik dokumendid ja Accessi Graphi Explorer, leiate teemast [Microsoft Graphi saidi](https://graph.microsoft.io/).

Kolmest etapist juurdepääsul identiteedi kaitse andmete Microsoft Graphi kaudu:

1. Lisage kliendi salajane rakendus. 

2. Autentida Microsoft Graphi, kus saate ka autentimise luba see salajane ja mõned muude teabe abil. 

3. Selle märgiks abil esitada API lõpp-punkti ja naasta identiteedi kaitse andmeid.

Enne alustamist peate.

- Rakenduse loomiseks Azure AD
- Teie rentniku domeeninime (nt contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Kliendi salajane taotluse lisamine


1. [Logige sisse](https://manage.windowsazure.com) oma Azure klassikaline portaali administraatorina. 

1. Klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Klõpsake menüü peal, **rakendused**.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. Klõpsake dialoogiboksi, **mida soovite teha** **rakenduse ettevõttesiseselt areneb lisamine**.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. Klõpsake dialoogiboksis **meile rakenduse kohta** tehke järgmist.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    lisamine. Tippige **nimi** väljale rakenduse nimi (nt: AADIP Risk sündmuse API rakendus).

    b. **Tüüp**, valige **veebi-API rakenduse ja/või Web**.

    c. Klõpsake nuppu **edasi**.


5. Klõpsake **rakenduse atribuutide** dialoogiboksis tehke järgmist.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    lisamine. Tippige väljale **Sisselogimise URL-i** `http://localhost`.

    b. Tippige tekstiväljale **Rakenduse ID URI** `http://localhost`.

    c. Klõpsake nuppu **valmis**.


Teie saate nüüd konfigureerida rakenduse.

![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Anda oma rakenduse õiguse API kasutamine


1. Klõpsake rakenduse lehe ülaosas menüü **konfigureerimine**. 

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. Klõpsake jaotises **õigused muude rakenduste** **Lisa rakendus**.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. Klõpsake dialoogiboksis **õiguste muudes rakendustes** tehke järgmist.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    lisamine. Valige **Microsoft Graphi**.

    b. Klõpsake nuppu **valmis**.


1. Klõpsake **teenuserakenduse õiguste: 0**, ja seejärel valige **Kõik identiteedi risk sündmuse teavet lugeda**.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Klõpsake lehe allosas **salvestada** .

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Saada võti

1. Rakenduse lehel **klahvid** , valige jaotises 1 aasta as kestus.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Klõpsake lehe allosas **salvestada** .

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. klahvid jaotises kopeerida oma äsja loodud võtme väärtus ja seejärel kleepige see turvaline asukoht.

    ![Rakenduse loomine](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Kui te kaotate selle klahvi, on teil selle jaotise naasmiseks ja luua uue tootenumbri. Hoidke see võti on: kõigile, kellel on see teie andmetele juurde pääseda.


1. Jaotises **Atribuudid** kopeerida **Kliendi ID**ja seejärel kleepige see turvaline asukoht. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Microsoft Graphi autentimiseks ja päringu identiteedi Risk sündmuste API-ga

Selles etapis peaks olema:

- Ülal kopeeritud kliendi ID

- Ülal kopeeritud võtit

- Teie rentniku domeeni nimi


Autentida, saata postituse taotluse `https://login.microsoft.com` kehas järgmiste parameetrite abil:

- grant_type: "**client_credentials**"

- ressurss: "**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] Peate sisestama väärtused **client_id** ja **client_secret** parameeter.

Kui õnnestus, see tagastab soovitud autentimise luba.  
API helistamiseks luua päise järgmine parameeter.

    `Authorization`=”<token_type> <access_token>"


Kui autentimine, leiate loa tüüp ja juurdepääsu luba tagastatud luba.

Saatke see päis päringuga järgmine API URL:`https://graph.microsoft.com/beta/identityRiskEvents`

Vastuse, edu korral on kogumi identiteedi risk sündmused ja seotud andmed, OData JSON-vormingus, mida saab sõeluda ja käsitleda kui sobib.

Siin on proovi kood autentimist ja kutsudes API PowerShelli kaudu.  
Lisage lihtsalt oma kliendi ID-d, sisestage ja rentniku domeeni.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Järgmised sammud

Palju õnne, just tehtud esimese kõne Microsoft Graphi!  
Nüüd saate päringu identiteedi risk sündmused ja kasutada andmeid, kuid näed.

Märkige ruut välja [dokumentatsiooni](https://graph.microsoft.io/docs) ja palju muud [Microsoft Graphi saidi](https://graph.microsoft.io/)Microsoft Graphi ja kuidas luua rakendusi Graph API kasutamise kohta lisateabe saamiseks. Lisaks veenduge, et järjehoidja [Azure AD identiteedi kaitse API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) leht, mis sisaldab loendit kõigist identiteedi kaitse API Graphi saadaval. Nagu lisame uued võimalused töötamiseks identiteedi kaitse API kaudu, kuvatakse need sellel lehel.


## <a name="additional-resources"></a>Lisaressursid

- [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md)

- [Risk üritused avastada Azure Active Directory identiteedi kaitse](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graphi](https://graph.microsoft.io/)

- [Microsoft Graphi ülevaade](https://graph.microsoft.io/docs)

- [Azure'i AD identiteedi kaitse teenuse Root](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
