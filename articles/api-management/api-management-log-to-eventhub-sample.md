<properties
    pageTitle="Azure'i API haldus, sündmuse jaoturi ja Runscope oma API jälgimine"
    description="Proovi taotluse näidata ühendusjooni Azure'i API haldus, Azure'i sündmuse jaoturi ja Runscope http logimist ning jälgimist log-eventhub poliitika"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Azure'i API haldus, sündmuse jaoturi ja Runscope oma API jälgimine

[API haldamise teenust](api-management-key-concepts.md) pakub palju võimalusi täiustamiseks HTTP päringuid saata oma HTTP API töötlemine. Kuid taotlusi ja koosolekutaotluste olemasolu on ajutine. Taotluse ja juhtimine API halduse teenuse abil oma taustväärtus API kaudu. Oma API töötleb taotluse ja vastuse liigub sujuvalt piires tagasi API tarbija. API halduse teenuse hoiab mõned olulised statistikat API-de kuvamiseks Publisheri portaali armatuurlaual, kuid kaugemale, et andmed on kadunud.

[Log-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [poliitika](api-management-howto-policies.md) API halduse teenuse abil saate saata taotlus ja [Azure sündmuse jaoturi](../event-hubs/event-hubs-what-is-event-hubs.md)vastuse üksikasju. On mitmesuguseid põhjust, miks võiksite luua sündmuste HTTP sõnumid saadetakse teie API-d. Mõned näited auditilogi rada värskendused, Kasutusanalüüsi, erand ja 3 tootja integratsioon.   

Selles artiklis näitab, kuidas jäädvustada kogu HTTP koosolekukutsete ja kutsele vastamise sõnumi, saatke see sündmus jaoturiga ja seejärel relee see sõnum kolmandale osapoolele teenus, mis sisaldab HTTP logimist ning jälgimist teenused.

## <a name="why-send-from-api-management-service"></a>Miks saata API halduse teenust?
On võimalik HTTP vahevara HTTP API raamistik jäädvustada HTTP taotlusi ja koosolekutaotluste ja siseneda logimist ning jälgimist süsteemide ühendatavad saate kirjutada. Seda moodust negatiivne on HTTP vahevara olla integreerida taustväärtus API ja peab vastama platvormi API. Kui määratud on mitu API-de siis iga peab juurutada soovitud vahevara. Sageli on põhjused, miks ei saa värskendada taustväärtus API-d.

Logimise taristu integreerimine Azure API halduse teenuse abil annab keskse ja platvormi sõltumatul lahenduse. Samuti on scalable osalt [geo-dispersioonanalüüs](api-management-howto-deploy-multi-region.md) võimaluste Azure'i API haldus.

## <a name="why-send-to-an-azure-event-hub"></a>Miks saata Azure'i sündmuse jaoturiga?
See on mõistlik küsida, miks luua poliitika, mis on seotud Azure'i sündmuse jaoturi? On mitmest eri kohast, kuhu võib soovite logige minu päringud. Miks pole lihtsalt soovitud päringuid saata otse sihtkohta?  Mis on võimalus. Logimise taotlused API-halduse teenusest tegemisel on vaja uurida, kuidas mõjutab logimine sõnumite API jõudlus. Laadi järk-järgult tõusu võib käsitleda saadaval eksemplarid osad suurendades või ära geo-dispersioonanalüüs. Siiski lühike diagrammi liikluse võib põhjustada taotluste märkimisväärselt edasi lükata kui logimine taristu taotluste käivitamine koormuse aeglane.

Azure'i sündmuse jaoturiga on mõeldud sissepääsu suuri andmemahtusid, mahuga tegeleb sündmuste palju suurem arv, kui HTTP arvu taotleb enamik API-de protsess. Sündmuse jaoturi toimib omamoodi keerukaid puhvri API halduse teenust ja taristu, mis talletada ja töötlemine sõnumite vahel. See tagab, et API jõudluse tõttu logimine taristu ei too.  

Kui andmed on see on jätkunud ja ootama sündmuse jaoturi tarbijad töödelda sündmuse jaoturiga edasi. Sündmuse jaoturi hoolikalt, kuidas neid töödelda, see huvitab lihtsalt kohta, kuidas veenduda, et sõnumit ei toimetatud.     

Sündmuse jaoturi oska voo sündmusi mitme rühmi. See võimaldab hoopis süsteemide töödelda sündmusi. See võimaldab toetavad palju integreerimise stsenaariumid panemata lisamine viivitused API taotluse API haldamine teenuses töötlemine ainult ühe sündmuse peab olema loodud.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Poliitika rakenduse/http kaudu sõnumite saatmiseks
Kuvatakse sündmuse jaoturi aktsepteerib sündmuse andmete lihtne stringina. Sisu see string on täielikult teie. Saama paketti HTTP-päring üles ja saatke see sündmuse jaoturi tunnise kellaajavormingu stringi taotluse või vastuse teabega. Olukordades, kui seal on olemasoleva vormingu me saate uuesti kasutada, siis me ei pruugi olla kirjutada oma sõelumine kood. Esialgu ma kaaluda [HAR](http://www.softwareishard.com/blog/har-12-spec/) kasutamise HTTP päringuid ja vastuste saatmine. Selles vormingus on optimeeritud talletamise HTTP päringuid jada vastavalt JSON-vormingus. See sisaldab arvu kohustuslikud osad, lisatud tarbetu keerukus läbimise HTTP sõnumi üle kaabel stsenaariumi.  

Teise võimalusena on kasutada funktsiooni `application/http` meediumi tüüp, nagu on kirjeldatud HTTP määratlus [RFC 7230](http://tools.ietf.org/html/rfc7230). Seda tüüpi meediumi kasutab tegelikult saatmine HTTP üle kaabel on kasutatud sama vormingu, kuid saab kogu sõnumi kehas teise HTTP-päring sellele. Meie puhul lihtsalt me kasutamiseks keha meie sõnumi saatmiseks sündmuse jaoturi. Mugavalt, on parseri, mis on olemas [Microsoft ASP.net-i Web API 2.2 klient](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) teekide puhul, kus saate sõeluda seda vormingut ja muuta selle emakeelena `HttpRequestMessage` ja `HttpResponseMessage` objektid.

Saate luua see sõnum, mida läheb vaja ära C# vastavalt [poliitika avaldiste](https://msdn.microsoft.com/library/azure/dn910913.aspx) Azure'i API haldus. Siin on poliitika, mis saadab sõnumi HTTP taotluse Azure'i sündmuse jaoturi.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Poliitika deklaratsioon
Seal mõne kindla asja mainida see avaldis poliitika kohta. Log-eventhub poliitika on atribuut nimega puuraidur-id, mis viitab puuraidur API haldamine teenuses loodud nime. Häälestamine on sündmuse jaoturi puuraidur API halduse teenuse üksikasjad leiate dokumendi [sisselogimine sündmusi Azure'i sündmuse jaoturi Azure'i API haldamine](api-management-howto-log-event-hubs.md). Teine atribuut on valikuline parameeter, mis juhendab sündmuse jaoturi, mis partition sõnum talletamiseks. Sündmuse jaoturi abil sektsioonid lubada scalabilty ja nõuavad vähemalt kaks. Järjestatud sõnumite kohaletoimetamisega tagada ainult sektsiooni sees. Kui me paluge, millist sektsiooni paigutamiseks sõnumi sündmuse keskus, kasutatakse round-jaan algoritmi, laadi üles. Siiski, mis võivad põhjustada mõned meie sõnumid töödelda vales järjestuses.  

### <a name="partitions"></a>Sektsioonid
Meie sõnumeid edastatakse tarbijatele järjestuses ja ära laadi jaotuse võimalus sektsioonide tagamiseks valisin ühe sektsiooni ja HTTP vastuse sõnumid teise sektsiooni HTTP taotluse sõnumeid saata. See tagab on paaris jaotust ja saame tagada, et kõik kutsed on tarbitud järjestuses ja kõik vastused on tarbitud järjestuses. On võimalik, et vastuseks tuleb enne vastava taotluse tarbitud, kuid mis ei ole probleem, nagu oleme erinevate süsteem tõestusmeetodid taotleb vastuste ja me ei tea, et taotlusi alati enne vastuseid.

### <a name="http-payloads"></a>HTTP lõhkelaengu
Pärast koosteüksuse selle `requestLine` me kontrollida, kui koosolekukutse kehas olla kärbitud. Ainult 1024 kärbitakse koosolekukutse sisusse. See võib suurendada, aga üksikuid sündmuse jaoturi sõnumeid on piiratud 256KB, seega on tõenäoline, et mõned HTTP sõnumi asutused saavad ei mahu ühes sõnumis. Logimine ja analüüsi, tehes saate palju teavet tuletada lihtsalt HTTP taotluse rea- ja päised. Lisaks palju API taotlusi ainult tagasi small asutuste ja nii teabe väärtuse järgi Kärbi suurte asutuste vähenemine on üsna minimaalne võrreldes edastamise, töötlemise ja salvestusruumi maksumus säilitada keha kogu sisu vähenemine. Ühe lõplik Märkus keha töötlemine on peame läbima `true` on AS<string>() meetodi Kuna me loete keha sisu, kuid on ka soovite kirjutamata API lugeda keha. Mille õige seda meetodit me põhjustada keha puhverdatud, et seda saavad lugeda teist korda. See on oluline teada, kui teil on väga suurte failide üleslaadimine või kasutab pikk küsitlused API tekkimist. Sellisel juhul oleks parem vältida lugemine keha üldse.   

### <a name="http-headers"></a>HTTP päised
HTTP päised saab üle lihtsalt üle lihtsa /-väärtuse paari vormingus sõnumi vormingusse. Valisime ribad teatud turvalisus tundliku väljad vältimiseks asjatult lekib mandaadi teavet. See on tõenäoline, et API võtmed ja muud identimisteabe kasutatakse analytics eesmärgil. Kui soovime kasutaja ja kindla toote analüüsi tegemiseks, kes kasutavad siis saaksime, mille kaudu soovitud `context` objekti ja lisada, et sõnum.     
### <a name="message-metadata"></a>Sõnumi metaandmed
Kui hoone täielik sõnum saata sündmuse jaoturi, esimene rida pole tegelikult osa on `application/http` sõnum. Esimene rida on täiendavad metaandmed, kas sõnum on taotluse või vastuse sõnumi ja sõnumi ID-d, mis on seda kasutada taotluste vastuste koosneb. Sõnumi ID-d on loodud mõne muu poliitika, mis näeb välja umbes järgmine:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

Meil võib loodud taotluse sõnumi, mis talletatakse muutuja kuni vastus on tagastatud ja seejärel lihtsalt saadetud koosolekukutsete ja kutsele vastamise ühe sõnumina. Siiski, saates koosolekukutsete ja kutsele vastamise sõltumatult ja oleksid kahe sõnumi id abil, saame veidi rohkem paindlikkust sõnumi suurus, samal ajal meie logimine armatuurlaua sõnumi korra ja taotluse varem kuvatakse mitu sektsiooni ära võimalus. Samuti võib mõnel juhul, kui kehtiv vastus saadetakse kunagi sündmuse jaoturi, võib-olla API halduse teenus, surmaga taotluse tõrke tõttu, kuid ikka meil taotluse kirje.

Poliitika vastuse HTTP sõnumi saatmiseks otsib väga sarnane taotluse ja seega täieliku poliitika konfiguratsiooni näeb välja umbes järgmine:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

Funktsiooni `set-variable` poliitika loob väärtus, mis on kättesaadav nii on `log-to-eventhub` poliitika on `<inbound>` jaotis ja `<outbound>` jaotis.  

## <a name="receiving-events-from-event-hubs"></a>Sündmuse jaoturi vastuvõtmise sündmused
Sündmuste Azure'i sündmuse keskuse kaudu vastu [AMQP Protocol (protokoll)](http://www.amqp.org/). Microsofti teenuste siini meeskonnatöö tehtud kliendi teekide nõudvate sündmuste hõlbustamiseks saadaval. On kaks eri võimalust, mis on toetatud, üks on *Otsene tarbija* ja teine kasutab funktsiooni `EventProcessorHost` klassi. [Sündmuse jaoturi programmeerimisega juhendi](../event-hubs/event-hubs-programming-guide.md)leiate näiteid nende kahe lähenemise. Lühendatud erinevused on, `Direct Consumer` annab teile täielik kontroll ja `EventProcessorHost` ei osa Sanitaartehnilised tööd saate aga muudab teatud oletused kohta, kuidas töötlete üritustele.  

### <a name="eventprocessorhost"></a>EventProcessorHost
Selles näites me kasutame funktsiooni `EventProcessorHost` lihtsa, aga see võib pole parim valik kindla stsenaariumi. `EventProcessorHost`Kas tööd teha, et te ei pea muretsema threading probleemid teatud sündmuse protsessor tunni jooksul. Siiski meie stsenaariumi oleme lihtsalt sõnumit mõnda muusse vormingusse teisendamine ja saata see mööda muusse teenusesse asünkroonse meetodi abil. Ei ole vaja värskendamise ühiskasutuse oleku ja seetõttu ohtu Threading probleemid. Enamik stsenaariumid, `EventProcessorHost` ilmselt parim valik ja see on kindlasti lihtsam võimalus.     

### <a name="ieventprocessor"></a>IEventProcessor
Kasutamisel keskne mõiste `EventProcessorHost` on luua mõne rakendamise soovitud `IEventProcessor` kasutajaliides, mis sisaldab `ProcessEventAsync`. Selle meetodi olemus on näidatud siin.

  asünkroonse tööülesande IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) {}

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

EventData objektide loendi on läks meetodit ja me kordamise üle sellele loendile. Iga meetodi baiti on sõeluda HttpMessage objekti ja objekti edastatakse IHttpMessageProcessor eksemplari.

### <a name="httpmessage"></a>HttpMessage
Funktsiooni `HttpMessage` eksemplari sisaldab kolme andmeühikuga:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

Funktsiooni `HttpMessage` eksemplari sisaldab mõnda `MessageId` GUID, mis võimaldab teil HTTP-päringu ühenduse vastava HTTP vastus ja loogikaväärtus, mis tuvastab, kui objekt sisaldab HttpRequestMessage ja HttpResponseMessage. Abil ehitatud HTTP tunnid kaudu `System.Net.Http`, mul oli võimalus ära on `application/http` sõelumine kood, mis sisaldab `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
Funktsiooni `HttpMessage` eksemplari edastatakse rakendamine `IHttpMessageProcessor` mis on loodud lahutada vastuvõtmise ja Azure sündmuse keskuse kaudu sündmuse tõlgendamise ja tegeliku käsitlemine liidest.


## <a name="forwarding-the-http-message"></a>HTTP sõnumi edasisaatmine
See näide ma otsustada, huvitav push HTTP-päring üle [Runscope](http://www.runscope.com). Runscope on pilvepõhine teenus, mis on HTTP silumine, logimist ning jälgimist. Need on tasuta taseme, nii on lihtne teha, ja siis saame vaadata selle HTTP-taotluste reaalajas läbib meie API halduse teenuse.

Funktsiooni `IHttpMessageProcessor` rakendamist näeb välja selline,

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Sain mõne [olemasoleva kliendi teek Runscope jaoks](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) , mis hõlbustab tõuketeatised ära `HttpRequestMessage` ja `HttpResponseMessage` saavad võtta üles eksemplarid. Runscope API juurdepääsuks peate konto ja mis API võti. [Accessi Runscope API rakenduste loomise](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) screencast leiate juhised saada mõne API võti.

## <a name="complete-sample"></a>Täieliku näidis
[Lähtekoodi](https://github.com/darrelmiller/ApimEventProcessor) ja testib valimi on GitHub. Peate mõne [API halduse teenuse](api-management-get-started.md), [Ühendatud sündmuse jaoturi](api-management-howto-log-event-hubs.md)ja [Salvestusruumi konto](../storage/storage-create-storage-account.md) käivitamiseks valimi enda jaoks.   

Valim on lihtsalt lihtne konsooli rakendus, mis kuulab jaoks sündmustest jpm sündmuse keskuses teisendab need on `HttpRequestMessage` ja `HttpResponseMessage` objektide ja edastab need siis edasi Runscope API-ga.

Animeeritud järgmisel pildil, saate vaadata tehtud API arendaja portaalis konsooli rakendus, mis näitab sõnumit saanud, töödeldav ja edasi saata ja seejärel taotluse ja vastuse Runscope liikluse inspektor kuvatud taotluse.

![Taotluse saatmisest Runscope tutvustamine](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Kokkuvõte
Azure API halduse teenust pakub on optimaalne koht jäädvustada HTTP liiklus lähte- ja kaudu oma API-d. Azure'i sündmuse jaoturi on väga paindlik, madal lahendus selle liikluse talletamiseks ja edaspidise teisese töötlemise süsteemid logimine, jälgimine ja muude keerukas analytics. Ühenduse 3 tootja liikluse jälgimise süsteemid nagu Runscope on lihtne, kui mõni kümneid koodiread.

## <a name="next-steps"></a>Järgmised sammud
-   Lisateave Azure'i sündmuse jaoturi
    -   [Azure'i sündmuse jaoturi kasutamise alustamine](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Sõnumeid, mille EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Sündmuse jaoturi programmeerimisega juhend](../event-hubs/event-hubs-programming-guide.md)
-   Lisateavet API haldus ja sündmuse jaoturi integreerimine
    -   [Kuidas sündmused logida Azure'i sündmuse jaoturi Azure'i API haldamine](api-management-howto-log-event-hubs.md)
    -   [Puuraidur üksuse viide](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [log-eventhub poliitika viide](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    