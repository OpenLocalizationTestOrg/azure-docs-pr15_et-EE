<properties
    pageTitle="API halduse teenuse abil luua HTTP päringuid"
    description="Saate teada, kuidas kasutada koosolekukutsete ja kutsele vastamise poliitikate API halduse kõne välisteenused oma API"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="using-external-services-from-the-azure-api-management-service"></a>Azure'i API halduse teenuse väliste teenuste kaudu

Saadaval teenuses Azure API halduse poliitikaid saate teha mitmesuguseid kasulik töö puhtalt sissetuleva taotluse, väljamineva vastus ja seadistada teabe põhjal. Siiski ei saaks suhelda välisteenused API juhtimine poliitikate avab palju rohkem võimalusi.

Oleme varem näinud, kuidas saab interaktiivselt [Azure'i sündmuse jaoturi teenuse logimine, jälgimine ja analüüsi](api-management-log-to-eventhub-sample.md). Selles artiklis demonstreerime poliitikad, mis võimaldavad teil suhelda mis tahes välise HTTP vastavalt teenus. Need poliitikad saab käivitamise remote sündmuste või toomine teavet, mida kasutatakse töödelda selle algse koosolekukutsete ja kutsele vastamise mingil viisil.

## <a name="send-one-way-request"></a>Saada üks-viis päring
Võimalik, et lihtsaim väline suhtlus on fire-ja-unusta laadi taotluse, mis võimaldab teenust teavitama mingi oluline sündmus. Kasutame control flow poliitika `choose` tuvastamiseks mis tahes tüüpi tingimus, et oleme huvitatud ja siis, kui tingimus on täidetud, saame [saada üks-viis päring](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) poliitika abil välise HTTP-päring. See võiks olla sõnumisüsteemist üle, nt Hipchat või vaikne või mail API, nt SendGrid või MailChimp taotluse või kriitiliste tugiteenuse juhtumile midagi PagerDuty. Kõik need sõnumside süsteemid on lihtne HTTP API, mida me saate hõlpsalt autonoomsest.

### <a name="alerting-with-slack"></a>Vaikne koos teavitamine
Järgmises näites näitab, kuidas vaikne jututoa sõnumeid saata, kui HTTP vastuse olek kood on suurem kui või võrdne 500. 500 vahemiku tõrge näitab meie taustväärtus API, mida meie API kliendi ei saa ise lahendada probleem. Tavaliselt on vaja mõnda tüüpi sekkumist meie osa.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Vaikne on sissetulevate web konksud mõiste. Mõne sissetulevate web konks konfigureerimisel loob vaikne teisiti URL, mis võimaldab teil teha lihtne POST taotlus ja sõnumi edastamiseks vaikne kanali. JSON sisu, mille loome on määratletud vaikne vormingu alusel.

![Vaikne Web konks](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>On fire ja unusta piisavalt hea?
Taotluse fire-ja-unusta laadi kasutamisel on teatud kompromissidega. Kui mingil põhjusel taotlus nurjub, siis ei saa selle esitada. Sellisel juhul kindla keerukus teisene tõrke aruandluse süsteem ja täiendavad jõudluse kulu vastuse ootamine on õigustatud. Stsenaariumid, kui see on oluline kontrollida vastus, siis [Saada päring](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) poliitika on parem valik.

## <a name="send-request"></a>Saada päring
Funktsiooni `send-request` poliitika lubab keerukas töötlemine ülesannete täitmiseks ja andmete haldamine API service, et tagastada välise teenuse abil saab kasutada poliitika edasiseks töötlemiseks.

### <a name="authorizing-reference-tokens"></a>Lubab viide sõned
Peamised funktsioon API juhtimise kaitseb kirjutamata ressursid. Kui autoriseerimine server kasutada oma API loob [JWT sõned](http://jwt.io/) osana oma OAuth2 kulgemist, nagu [Azure Active Directory](../active-directory/active-directory-aadconnect.md) , siis saate kasutada funktsiooni `validate-jwt` poliitika luba kehtivust. Siiski saate luua mõned autoriseerimine serverid nn ei saa kontrollida kõne tagasi autoriseerimine server tegemata [viide sõned](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) .

### <a name="standardized-introspection"></a>Standardne enesevaatluseks
Varem on pole veebiaadressile kinnitatava märgiks viide autoriseerimine serveriga. Kuid viimati pakutud standard [RFC 7662](https://tools.ietf.org/html/rfc7662) ilmus IETF, mis määratleb, kuidas ressursi serveris saate kontrollida märgiks kehtivust.

### <a name="extracting-the-token"></a>Luba ekstraktimiseks
Kõigepealt tuleb ekstraktida luba autoriseerimine päis. Päise väärtus olema vormindatud soovitud `Bearer` autoriseerimine värviskeemi, üks tühik ja seejärel ühe [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1)autoriseerimine luba. Kahjuks on juhtumeid, kus autoriseerimine kava puudub. Kui sõelumine konto nii, et päise väärtus ruumi tükeldamine ja valige viimase string stringid tagastatavale massiivile. See võimaldab minna halvasti vormindatud autoriseerimine päised.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Taotluse kinnitamine
Kui oleme autoriseerimine luba, saame taotluse kinnitamiseks luba. RFC 7662 helistab selle protsessi enesevaatluseks ja eeldab, et te `POST` HTML-vormi enesevaatluseks ressursi. HTML-vormi peab sisaldama vähemalt paari /-väärtuse võti `token`. Selle serveri autoriseerimine taotluse peavad olema kinnitatud ka tagamaks, et pahatahtlik kliendid ei saa minna traalimise lubatud märkide.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Vastuse kontrollimine
Funktsiooni `response-variable-name` atribuuti kasutatakse juurdepääsu andmiseks tagastatud vastus. Selle atribuudi määratletud nime saab kasutada klahvi sisse selle `context.Variables` sõnastiku avamiseks soovitud `IResponse` objekti.

Vastuse objektist me saate tuua keha ja RFC 7622 ütleb, et vastus peab olema JSON objekt ja peab sisaldama vähemalt atribuudi `active` mis on loogikaväärtus. Kui `active` on tõene, siis luba peetakse kehtivaks.

### <a name="reporting-failure"></a>Aruandlusteenuste tõrge
Kasutame on `<choose>` poliitika tuvastamiseks, kui luba on sobimatu ja sel juhul tagastada 401 vastuse.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Ühe [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) , kus kirjeldatakse, kuidas `bearer` sõned tuleks kasutada, me ka tagasi on `WWW-Authenticate` päise 401 vastus. WWW autentida peaks paluge klient, kuidas koostada õigesti volitatud taotluse. Võimalustest võimalik OAuth2 raames mitmesuguseid tõttu on raske suhelda vajalik teave. Õnneks on käimas, et aidata [klientidele teada, kuidas õigesti lubada taotlusi ressursi serveris](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Lõplik lahendus
Kõigi osade kokku panemine, saame poliitika järgmist:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

See on ainult üks palju näiteid, kuidas `send-request` poliitika saab kasutada kasulik välisteenused integreerida taotluste ja liigub läbi API halduse teenuse vastuseid.

## <a name="response-composition"></a>Vastuse koosseis
Funktsiooni `send-request` poliitika saab kasutada täiustamine esmane taotluse taustväärtus süsteemi, nagu me kuvati eelmises näites või seda saab kasutada täielikku Asenda jaoks taustväärtus kõne. Selle meetodi abil saame hõlpsalt luua kombineeritud ressursse, mis on koondatud mitme eri Systemsi.

### <a name="building-a-dashboard"></a>Armatuurlaua ehitamine   
Vahel soovite võimaldada teave, mis on olemas mitme taustväärtus süsteemid, näiteks seada, armatuurlaua juhtida. KPI-de pärit kõik erinevad tagasi lõpetatakse, kuid te ei soovi otsest juurdepääsu ja oleks tore, kui kõik andmed võib tuua ühe taotluse. Võib-olla kirjutamata teabest peab mõned viilutamine ja tükeldamise ja esmalt veidi desinfitseerimist! On võimalik selle kombineeritud ressursi vahemälu oleks kasulik vähendada taustväärtus laadi, nagu te teate, et kasutajatel on tavaks haamriga klahvi F5, et näha, kas nende esinev mõõdikud võib muutuda.    

### <a name="faking-the-resource"></a>Scam ressurss
Meie armatuurlaua ressursi loomisse Kõigepealt tuleb konfigureerida API haldusportaal Publisheri uue toiming. See on kohatäite toiming, saab konfigureerida koosseis poliitika koostamiseks meie dünaamiline ressursi.

![Armatuurlaua toimingu](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Taotluste tegemine
Üks kord selle `dashboard` toiming on loodud configure poliitika selle toimimiseks. 

![Armatuurlaua toiming](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Esimese sammuna tuleb ekstraktida sissetuleva taotluse, mis tahes päringu parameetrid nii, et saaksime edastab need meie taustväärtus. Selles näites on meie armatuurlaua kuvatud teabe põhjal aja on seega on `fromDate` ja `toDate` parameeter. Me kasutame funktsiooni `set-variable` poliitika eraldamiseks taotluse URL-i teave.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Kui meil on see teave saame taotlusi kirjutamata süsteemidele. Iga taotluse sisuosast teabega parameetri uus URL ja selle vastav server nõuab ja talletab vastuse kontekstis muutujana.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Taotlused käivitatakse järjest, mis ei ole optimaalne. On eelseisvad väljaanne me kasutusele uue poliitika nimega `wait` mis võimaldab kõik need taotlused samal ajal käivitada.

### <a name="responding"></a>Vasta

Ehitada kombineeritud vastuse kasutame [tagasi-vastuse](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) poliitika. Funktsiooni `set-body` elemendi abil saate avaldise koostada uut `JObject` koos komponent esinduste manustatud atribuutidena.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Täieliku poliitika näeb välja järgmine:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

Kohatäite konfiguratsiooni toiming configure armatuurlaua ressurssi vahemälurežiimi vähemalt üks tund, kuna me mõista andmete olemust tähendab, et ka siis, kui see on aegunud, see ikkagi piisavalt tõhus väärtuslik edastada kasutajatele tund.

## <a name="summary"></a>Kokkuvõte
Azure'i API halduse teenuse pakub paindlikke poliitikat, mida saab rakendada valikuliselt HTTP-liikluse ja lubab koosseis kirjutamata teenused. Kas soovite API lüüsi koos teavitamine funktsioone, kinnitamine, valideerimine võimalusi parandada või luua uue kombineeritud ressursid alusel mitu taustväärtus teenused, on `send-request` ja seotud poliitikad avada maailma võimalusi.

## <a name="watch-a-video-overview-of-these-policies"></a>Vaadake videot ülevaade poliitika
[Saada üks-viis päring](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [Saada päring](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)ja [tagasi-vastuse](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) poliitikate artikkel kohta lisateabe saamiseks vaadake järgmist videot.

> [AZURE.VIDEO send-request-and-return-response-policies]
