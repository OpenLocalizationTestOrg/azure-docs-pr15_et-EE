<properties 
    pageTitle="Windowsi universaalne kaasamine API kasutamine" 
    description="Windowsi universaalne kaasamine API kasutamine"            
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-universal"></a>Windowsi universaalne kaasamine API kasutamine

Selles dokumendis on lisandmooduli [kohta Windows universaalne kaasamine integreerida](mobile-engagement-windows-store-integrate-engagement.md)dokumenti: sügavus kaasamine API abil saate oma rakenduse statistika aruande üksikasjade pakub.

Pidage meeles, et kui soovite ainult kaasamine teatada rakenduse seansid, tegevused, jookseb ja tehnilist teavet, siis lihtsaim viis on teha kõik teie `Page` pärivad sub klassid on `EngagementPage` klassi.

Kui soovite teha rohkem, kui teil on vaja aruande rakenduse teatud sündmused, tõrgete ja töö, nt, või kui teil on teatada rakenduse tegevuste erineval viisil, kui üks rakendada soovitud `EngagementPage` klassid, siis peate kasutama rakenduse kaasamine API-ga.

Kaasamine API on esitatud selle `EngagementAgent` klassi. Nende meetodite kaudu pääsete `EngagementAgent.Instance`.

Isegi juhul, kui agent moodul on lähtestatud, iga API kõne edasi lükata, on ja käivitatakse uuesti, kui ta on saadaval.

##<a name="engagement-concepts"></a>Kaasamine mõisted

Järgmised osad piiritleda levinud [Mobile kaasamine põhimõtet](mobile-engagement-concepts.md) ühes kohas Windowsi platvormi.

### <a name="session-and-activity"></a>`Session`ja`Activity`

*Tegevuste* on tavaliselt seotud üks leht, st *tegevuse* algab lehel ei kuvata ja lõpetab lehe sulgemisel: see on juhul, kui kaasamine SDK on integreeritud, kasutades funktsiooni `EngagementPage` klassi.

Kuid *tegevuste* saab ka kaasamine API abil käsitsi kontrollida. See võimaldab teil tükeldada mitut sub osad, et saada täpsemat teavet (nt teadma, kui sageli ja kaua dialoogid kasutatakse selle lehe sees) selle lehe kasutamist antud lehele.

##<a name="reporting-activities"></a>Tegevuste teatamine

### <a name="user-starts-a-new-activity"></a>Kasutaja käivitab uue

#### <a name="reference"></a>Viide

            void StartActivity(string name, Dictionary<object, object> extras = null)

Helistage `StartActivity()` iga kord, kui kasutaja tegevuste muutub. Selle funktsiooni esimene käivitab uue kasutaja seansiga.

> [AZURE.IMPORTANT] SDK kõned EndActivity meetodit automaatselt, kui rakendus on suletud. Seega on väga soovitatav StartActivity meetodit kutsuda iga kord, kui kasutaja muudatuste ja mitte kunagi tegevuse kõne EndActivity meetodit, kuna selle meetodi jõustab praeguse seansi tuleb lõpetada.

#### <a name="example"></a>Näide

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Kasutaja lõpeb oma praeguse töö

#### <a name="reference"></a>Viide

            void EndActivity()

See lõpeb aktiivsust ja seansi. Mida peaksite helistama seda meetodit, kui te ei tea tõesti, mida te teete.

#### <a name="example"></a>Näide

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Aruandlusteenuste tööde haldamine

### <a name="start-a-job"></a>Töö alustamine

#### <a name="reference"></a>Viide

            void StartJob(string name, Dictionary<object, object> extras = null)

Töö abil saate jälgida tööülesannete certains aja jooksul.

#### <a name="example"></a>Näide

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Töö lõpetamine

#### <a name="reference"></a>Viide

            void EndJob(string name)

Kui tööülesande jälitatud tööd on lõpetatud, peaksite helistama EndJob meetodit selle töö esitades töö nimi.

#### <a name="example"></a>Näide

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Aruannete sündmused

On kolme tüüpi sündmused:

-   Autonoomse sündmused
-   Seansi sündmused
-   Töö sündmused

### <a name="standalone-events"></a>Autonoomse sündmused

#### <a name="reference"></a>Viide

            void SendEvent(string name, Dictionary<object, object> extras = null)

Autonoomse sündmused saate väljaspool seansi kontekstis.

#### <a name="example"></a>Näide

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Seansi sündmused

#### <a name="reference"></a>Viide

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Seansi sündmused on tavaliselt kasutatakse kasutaja toimingud oma seansi jooksul teatada.

#### <a name="example"></a>Näide

**Ilma andmeteta:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Andmetega:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Töö sündmused

#### <a name="reference"></a>Viide

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Töö sündmused on tavaliselt kasutatakse teatada kasutaja toimingud töö käigus.

#### <a name="example"></a>Näide

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Aruannete tõrked

On kolme tüüpi tõrkeid.

-   Autonoomse tõrked
-   Seansi tõrked
-   Töö tõrked

### <a name="standalone-errors"></a>Autonoomse tõrked

#### <a name="reference"></a>Viide

            void SendError(string name, Dictionary<object, object> extras = null)

Vastuolus seansi tõrgete, autonoomne tõrked võivad ilmneda väljaspool seansi kontekstis.

#### <a name="example"></a>Näide

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Seansi tõrked

#### <a name="reference"></a>Viide

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Seansi tõrked on tavaliselt kasutatakse tõrgete mõjutavad kasutaja oma seansi jooksul teatada.

#### <a name="example"></a>Näide

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Töö tõrked

#### <a name="reference"></a>Viide

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Tõrgete võib olla seotud esitatava töö asemel on seotud praeguse kasutaja seansi.

#### <a name="example"></a>Näide

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Aruandlusteenuste jookseb

Agent pakub kahte meetodit jookseb tegelema.

### <a name="send-an-exception"></a>Erandi saatmine

#### <a name="reference"></a>Viide

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Näide

Saate saata erandi igal ajal helistades:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Samuti saate valikuline parameeter samal ajal kui saatmine krahhi kaasamine seansi lõpetamiseks. Selleks helistada.

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Kui te seda teha, suletakse lihtsalt pärast saatmist krahhi seanss ja -tööde haldamine.

### <a name="send-an-unhandled-exception"></a>Saada töötlemata erandi

#### <a name="reference"></a>Viide

            void SendCrash(Exception e)

Kaasamine annab ka meetodi töötlemata erandid saata, kui teil on **keelatud** kaasamine automaatne **krahh** teatamine. See on eriti kasulik, kui kasutada rakenduse UnhandledException sündmuseohjuri sees.

See meetod on **alati** lõpetada pärast seda nimetatakse kaasamine seanss ja -tööde haldamine.

#### <a name="example"></a>Näide

Saate rakendada oma UnhandledExceptionEventArgs sündmuseohjuri. Näiteks lisada selle `Current_UnhandledException` meetod on `App.xaml.cs` faili:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

Lisage App.xaml.cs sisse "Avaliku App() {}".

            Application.Current.UnhandledException += Current_UnhandledException;

##<a name="device-id"></a>Seadme Id

            String EngagementAgent.Instance.GetDeviceId()

Saate kaasamine seadme ID-d, helistades seda meetodit.

##<a name="extras-parameters"></a>Lisad parameetrid

Suvalise andmetega seotud sündmuse, tõrge, tegevus või töö. Need andmed saate ehitatakse üles sõnastik. Võtmed ja väärtused võib olla mis tahes tüüpi.

Lisad andmed on seeriasertide nii, et kui soovite lisada oma tüüp lisad peate lisama seda tüüpi andmete leping.

### <a name="example"></a>Näide

Loome uue ainekursuse "Kelle".

            using System.Runtime.Serialization;
            
            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Seejärel lisame mõne `Person` eksemplar, et eest.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Kui muud tüüpi objektid, veenduge, et nende ToString() meetodit rakendatakse inimeste loetav stringi tagastamiseks.

### <a name="limits"></a>Piirangud

#### <a name="keys"></a>Kiirklahvid

Iga objekti sisestage peab vastama tavalise järgmine avaldis:

`^[a-zA-Z][a-zA-Z_0-9]*$`

See tähendab, et klahvid peab algama vähemalt ühe tähe, millele järgneb tähtede, numbrite või allakriipsutatud märkideks (\_).

#### <a name="size"></a>Suurus

Lisad on piiratud **1024** märki kõne kohta.

##<a name="reporting-application-information"></a>Teenuserakenduse teabe teatamine

### <a name="reference"></a>Viide

            void SendAppInfo(Dictionary<object, object> appInfos)

Saate käsitsi teatamise funktsioon jälituse abil soovitud SendAppInfo() teabe (või muude rakenduste kindla teabe).

Pange tähele, et neid andmeid saab saata sammhaaval: ainult Viimane väärtus antud võti säilitab seadmele. Sündmuse lisad, nt kasutada sõnastik\<objekti, objekti\> andmeid lisada.

### <a name="example"></a>Näide

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Piirangud

#### <a name="keys"></a>Kiirklahvid

Iga objekti sisestage peab vastama tavalise järgmine avaldis:

`^[a-zA-Z][a-zA-Z_0-9]*$`

See tähendab, et klahvid peab algama vähemalt ühe tähe, millele järgneb tähtede, numbrite või allakriipsutatud märkideks (\_).

#### <a name="size"></a>Suurus

Teenuserakenduse teabe on piiratud **1024** märki kõne kohta.

Eelmises näites, JSON, mis on saadetud server on 44 tähemärki.

            {"birthdate":"1983-12-07","gender":"female"}

##<a name="logging"></a>Logimine
###<a name="enable-logging"></a>Logimise lubamine

SDK saab konfigureerida andes testi logid IDE konsooli.
Need logid ei aktiveerita vaikimisi. See kohandamiseks atribuudi värskendamiseks `EngagementAgent.Instance.TestLogEnabled` üks väärtus, mis on saadaval on `EngagementTestLogLevel` loendamine, näiteks:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
 
