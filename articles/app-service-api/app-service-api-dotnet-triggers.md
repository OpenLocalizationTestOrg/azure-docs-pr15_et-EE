<properties 
    pageTitle="Rakenduse teenuse API rakenduse päästikute | Microsoft Azure'i" 
    description="Kuidas rakendada päästikute Azure'i rakendust Service rakenduse API" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure'i rakenduse teenuse API rakenduse päästikute

>[AZURE.NOTE] Selle versiooni see artikkel kehtib API rakenduste 2014-12-01-eelvaade skeemi versioon.


## <a name="overview"></a>Ülevaade

Selles artiklis selgitatakse, kuidas rakendada API rakenduse päästikute ja kasutada neid loogika rakenduse kaudu.

Kõik Koodilõigud selles teemas kopeeritakse [FileWatcher API rakenduse koodi valimi](http://go.microsoft.com/fwlink/?LinkId=534802). 

Pange tähele, et peate Nugeti järgmised koodi luua ja käivitada selles artiklis paketi: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Mis on API rakenduse päästikute?

See on levinud stsenaariumi API rakenduse tule sündmuse, et kliendid API rakenduse sobiv toiming vastuseks sündmus. Lisamispäästiku API rakenduse nimetatakse REST API põhinev süsteem, mis toetab seda stsenaariumi. 

Näiteks Oletame, et teie kliendi kood on kasutusel [Twitter konnektor API rakenduse](../app-service-logic/app-service-logic-connector-twitter.md) ja oma koodi peab põhineva uue tweets, mis sisaldavad kindlaid sõnu toimingu sooritamine. Sel juhul võib häälestada küsitlus või tõuketeatised päästik hõlbustamiseks seda vaja.

## <a name="poll-trigger-versus-push-trigger"></a>Küsitluse päästik võrreldes tõuketeatised päästik

Praegu on toetatud päästikute kahte tüüpi:

- Küsitluse päästik - kliendi küsitlused API rakenduse teatamise sündmuse võttes töötavate 
- Tõuketeatised päästik - kliendi API rakenduse antakse teada, kui sündmuse tule 

### <a name="poll-trigger"></a>Küsitluse päästik

Küsitluse päästik rakendataks tavaline REST API ja loodab oma klientidele (nt loogika rakendus) küsitlus selle selleks, et saada teatis. Kuigi klient võib säilitada, küsitluse päästik, ise on kodakondsuseta. 

Koosolekukutsete ja kutsele vastamise paketid järgmist teavet kirjeldavad olulisemad aspektid küsitluse päästik lepingu.

- Taotlemine
    - HTTP meetod: hankimine
    - Parameetrid
        - triggerState - valikuline parameeter võimaldab klientidel määrata määratud oleku põhjal nende nii, et küsitluse päästik saate õigesti otsustada, kas teate tagastamiseks või mitte.
        - API kohased parameetrid
- Vastus
    - Oleku kood **200** - taotlus kehtib ja on päästiku teatist. Teate sisu saab vastuse keha. "Proovi uuesti pärast" päise vastus näitab, et teatis täiendavate andmete peab tuua edasise päringu kõne.
    - Oleku kood **202** - taotlus on lubatud, kuid on uut teadet kaudu käivitada.
    - Oleku koodi **4xx** - taotlus ei sobi. Kliendi peaks uuesti taotluse.
    - Oleku koodi **5xx** - taotluse tulemuseks Sisemine serveritõrge ja/või ajutine probleem. Kliendi peaks kutse uuesti.

Järgmised koodilõigu on näide sellest, kuidas küsitlus päästik rakendada.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Selle küsitluse päästik testimiseks tehke järgmist.

1. Autentimise sättega **anonüümse**avaliku API rakenduse juurutamine.
2. Helistage **puudutage** faili puudutada toiming. Järgmisel pildil on kujutatud valimi taotluse postiljon kaudu.
   ![Puudutage funktsiooni postiljon kaudu](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Helistage päästik küsitlus koos parameetriga **triggerState** määratud ajatempel enne samm #2. Järgmisel pildil on kujutatud valimi taotluse postiljon kaudu.
   ![Kõne küsitluse päästik postiljon kaudu](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Tõuketeatised päästik

Tavaline REST API, mis sunnib teatised klientidele, kes on registreeritud kohaletoimetamisest teatud sündmuste tule tõuketeatised päästik on rakendada.

Koosolekukutsete ja kutsele vastamise paketid järgmist teavet illustreerimine tõuketeatised päästik lepingu olulisemad aspektid.

- Taotlemine
    - HTTP meetod: sellele
    - Parameetrid
        - triggerId: nõutav - läbipaistmatu stringi (nt GUID), mis tähistab tõuketeatised päästik registreerimise.
        - callbackUrl: nõutav - URL-i funktsiooni tagasihelistamise kasutada, kui sündmus tule. Funktsiooni kutsumise on lihtne postituse HTTP kõne.
        - API kohased parameetrid
- Vastus
    - Oleku kood **200** - taotlus kliendi eduka registreerimiseks.
    - Oleku koodi **4xx** - taotlus ei sobi. Kliendi peaks uuesti taotluse.
    - Oleku koodi **5xx** - taotluse tulemuseks Sisemine serveritõrge ja/või ajutine probleem. Kliendi peaks kutse uuesti.
- Tagasihelistamise
    - HTTP meetod: POST
    - Koosolekukutse kehasse: teate sisu.

Järgmised koodilõigu on näide sellest, kuidas rakendada tõuketeatised päästik.

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Selle küsitluse päästik testimiseks tehke järgmist.

1. Autentimise sättega **anonüümse**avaliku API rakenduse juurutamine.
2. Liikuge sirvides [http://requestb.in/](http://requestb.in/) RequestBin, mis toimib oma tagasihelistamise URL-i luua.
3. Helistage tõuketeatised päästik nimega **callbackUrl**RequestBin URL-i ja nimega **triggerId** GUID.
   ![Kõne tõuketeatised päästik postiljon kaudu](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Helistage **puudutage** faili puudutada toimingu. Järgmisel pildil on kujutatud valimi taotluse postiljon kaudu.
   ![Puudutage funktsiooni postiljon kaudu](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Märkige ruut RequestBin kinnitada, et tõuketeatised päästik tagasihelistamise kutsumisel atribuudi väljund.
   ![Kõne küsitluse päästik postiljon kaudu](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Päästikute API määratluses kirjeldada

Pärast soovitud päästikute rakendamise ja Azure API rakenduse juurutamine, liikuge **API määratlus** tera portaalis Azure eelvaade ja te näete, käivitab automaatselt tuvastatakse UI, mida API rakenduse määratluse ärplema 2.0 API.

![API määratlus Blade](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Kui klõpsate nuppu **Allalaadimine ärplema** ja avage fail, JSON, näete tulemusi järgmine:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

Laiend atribuut **x-ms-schedular-päästik** on, kuidas päästikute on kirjeldatud API määratlus ja lisatakse automaatselt API rakenduse lüüsi, kui te taotleda API määratluse lüüsi kaudu, kui kutse mõnele järgmistest kriteeriumidest. (Võite sisestada ka selle atribuudi käsitsi.)

- Küsitluse päästik
    - Kui HTTP-meetod on **saada**.
    - Kui atribuudi **operationId** sisaldab stringi **päästik**.
    - Kui atribuudi **parameetrite** sisaldab parameetri **nimi** atribuut on seatud **triggerState**.
- Tõuketeatised päästik
    - Kui HTTP-meetod on **panna**.
    - Kui atribuudi **operationId** sisaldab stringi **päästik**.
    - Kui atribuudi **parameetrite** sisaldab parameetri **nimi** atribuut on seatud **triggerId**.

## <a name="use-api-app-triggers-in-logic-apps"></a>Loogika rakendustes API rakenduse päästikute kasutamine

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Loendi ja loogika rakenduste designer API rakenduse päästikute konfigureerimine

Loogika rakenduse loomisel sama ressursirühm rakendusena API saab lisada kujundaja lõuend, klõpsates lihtsalt. Järgmistel piltidel kirjeldavad seda.

![Päästikute loogika rakenduse Designeris](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Küsitluse päästik loogika rakenduse Designer konfigureerimine](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Loogika rakenduse Designer tõuketeatised päästik konfigureerimine](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Optimeerimine API rakenduse päästikute loogika rakendused

Kui lisate rakenduse API päästikute, on mõned asjad, mida saate teha, et täiustada loogika rakenduse API rakenduse kasutamisel.

Küsitluse päästikute **triggerState** parameeter seadma näiteks järgmine avaldis loogika rakenduses. See avaldis peaks hinnata viimase kutsumise päästik loogika rakendusest, ja tagastab selle väärtuse.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Märkus: Selgitus funktsioonidest, mida kasutatakse ülal, vaadake dokumentatsiooni [Loogika rakenduse töövoo määratlus keel](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Loogika rakenduse kasutajad oleks vaja sisestada avaldise kohal **triggerState** parameetri kasutamisel käivitada. On võimalik, et see väärtus, mis on juba määranud loogika rakenduse designer kaudu laiend atribuut **x-ms ajasti soovitus**.  **X-ms-nähtavuse** laiend atribuut saate määrata väärtuseks *sise* nii, et parameetri ise designer ei kuvata.  Järgmised koodilõigu näitab, et.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Tõuketeatised päästikute, jaoks peab **triggerId** parameetri kordumatult loogika rakendus. Soovitatav on seatud atribuut selle töövoo nime, kasutades järgmist avaldist.

    @workflow().name

**X-ms ajasti soovitus** ja **x-ms-nähtavus** laiend atribuutide kasutamine oma API määratlusega, API rakenduse saate seada automaatselt see avaldis ja kasutajale loogika rakenduse Designer edastada.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>API defintion laiend atribuutide lisamine

Täiendavad metaandmeid – nt laiend atribuudid **x-ms ajasti soovitus** ja **x-ms-nähtavuse** - saab lisada API defintion on kaks võimalust: staatiline või dünaamiline.

Staatilise metaandmete, saate otse projektis */metadata/apiDefinition.swagger.json* faili redigeerimiseks ja atribuutide käsitsi lisada.

API rakenduste dünaamiline metaandmete abil saate redigeerida SwaggerConfig.cs faili lisamiseks toiming filter, mida saab lisada need laiendid.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Järgmine on näide sellest, kuidas see tund saab rakendada hõlbustamiseks dünaamiline metaandmete stsenaariumi.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
