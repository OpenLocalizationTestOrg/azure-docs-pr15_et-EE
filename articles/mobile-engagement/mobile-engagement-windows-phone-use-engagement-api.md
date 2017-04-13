<properties 
    pageTitle="Windows Phone Silverlighti kaasamine API kasutamine" 
    description="Windows Phone Silverlighti kaasamine API kasutamine"    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" /> 

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>Windows Phone Silverlighti kaasamine API kasutamine

Selles dokumendis on lisandmooduli dokumenti [Kuidas integreerida Mobile Windows Phone Silverlighti rakenduse osalemine](mobile-engagement-windows-phone-integrate-engagement.md). Pakub sügavus üksikasjade kaasamine API abil saate oma rakenduse statistika aruande.

Kui soovite kaasamine teatada rakenduse seansid, tegevused, jookseb ja tehnilist teavet, siis on kõige lihtsam teha kõik teie `PhoneApplicationPage` sub tunnid pärivad selle `EngagementPage` klassi.

Kui soovite teha rohkem, kui teil on vaja aruande rakenduse teatud sündmused, tõrgete ja töö, nt, või kui teil on teatada rakenduse tegevuste erineval viisil, kui üks rakendada soovitud `EngagementPage` klassid, siis peate kasutama rakenduse kaasamine API-ga.

Kaasamine API on esitatud selle `EngagementAgent` klassi. Nende meetodite kaudu pääsete `EngagementAgent.Instance`.

Isegi juhul, kui agent moodul on lähtestatud, iga API kõne edasi lükata, on ja käivitatakse uuesti, kui ta on saadaval.

##<a name="engagement-concepts"></a>Kaasamine mõisted

Järgmised osad piiritleda Mobile kaasamine põhimõtet Windows Phone platvormi.

### <a name="session-and-activity"></a>`Session`ja`Activity`

*Tegevuste* on tavaliselt seotud üks leht, st *tegevuse* algab lehel ei kuvata ja lõpetab lehe sulgemisel: see on juhul, kui kaasamine SDK on integreeritud, kasutades funktsiooni `EngagementPage` klassi.

Kuid *tegevuste* saab ka kaasamine API abil käsitsi kontrollida. See võimaldab tükeldamiseks antud lehele mitu sub osad, et saada täpsemat teavet selle lehe (nt teadaolevad sageduse ja kaua dialoogid kasutatakse selle lehe sees) kasutamist.

##<a name="reporting-activities"></a>Tegevuste teatamine

### <a name="user-starts-a-new-activity"></a>Kasutaja käivitab uue

#### <a name="reference"></a>Viide

            void StartActivity(string name, Dictionary<object, object> extras = null)

Helistage `StartActivity()` iga kord, kui kasutaja tegevuste muutub. Selle funktsiooni esimene käivitab uue kasutaja seansiga.

> [AZURE.IMPORTANT] Kui rakendus on suletud, helistage SDK EndActivity meetodit automaatselt. Seega on väga soovitatav StartActivity meetodit kutsuda iga kord, kui kasutaja muudatust ja mitte kunagi tegevuse kõne EndActivity meetodit, kuna selle meetodi jõustab praeguse seansi tuleb lõpetada.

#### <a name="example"></a>Näide

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Kasutaja lõpeb oma praeguse töö

#### <a name="reference"></a>Viide

            void EndActivity()

Helistage `EndActivity()` vähemalt ühe korra siis, kui kasutaja lõpetab viimati. See teatab kaasamine SDK, et kasutajal on praegu jõude ja, et kasutaja seansi vaja sulgeda üks kord seansi ajalõpp teie aegub (kui helistate `StartActivity()` enne seansi ajalõpp teie seansi jätkatakse lihtsalt).

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

On kolme tüüpi vigu:

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

Samuti saate valikuline parameeter kaasamine seansi samal ajal kui saatmine krahhi lõpetada. Selleks helistada.

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Kui te seda teha, suletakse lihtsalt pärast saatmist krahhi seanss ja -tööde haldamine.

### <a name="send-an-unhandled-exception"></a>Saada töötlemata erandi

#### <a name="reference"></a>Viide

            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Kaasamine annab ka meetodi saata töötlemata erandid. See on eriti kasulik, kui seda kasutatakse Silverlighti UnhandledException sündmuseohjuri sees.

See meetod on **alati** lõpetada pärast seda nimetatakse kaasamine seanss ja -tööde haldamine.

#### <a name="example"></a>Näide

Saate rakendada oma UnhandledException sündmuseohjuri (eriti siis, kui olete keelanud automaatse krahhi aruandluse funktsioon Engagement). Näiteks kuvatakse `Application_UnhandledException` meetod on `App.xaml.cs` faili:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code
            
              EngagementAgent.Instance.SendCrash(e);
            }

##<a name="onactivated"></a>OnActivated

### <a name="reference"></a>Viide

            void OnActivated(ActivatedEventArgs e)

Kui kasutaja viib edasi, eemale rakendus pärast tõstetakse deaktiveeritud sündmus, proovib rakendus pannakse soikus riigi operatsioonisüsteem. Seejärel klõpsake soovitud rakendus on Tombstoning. Selle protsessi rakenduse lõpetatakse, kuid mõned rakenduse ja üksikute lehtede rakendusest andmeid säilib.

Teil on vaja lisada `EngagementAgent.Instance.OnActivated(e)` sisse selle `Application_Activated` kaasamine Agent lähtestada, kui on taotlus Tombstoned failist App.xaml.cs meetod.

### <a name="example"></a>Näide

            // Inside your App.xaml.cs file
            
            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

##<a name="device-id"></a>Seadme Id

            String GetDeviceId()

Saate kaasamine seadme ID-d, helistades seda meetodit.

##<a name="extras-parameters"></a>Lisad parameetrid

Suvalise andmetega seotud sündmuse, tõrge, tegevus või töö. Need andmed saate ehitatakse üles sõnastik. Võtmed ja väärtused võib olla mis tahes tüüpi.

Lisad andmed on seeriasertide nii, et kui soovite lisada oma tüüp lisad peate lisama seda tüüpi andmete leping.

### <a name="example"></a>Näide

Loome uue ainekursuse "Kelle".

            using System.Runtime.Serialization;
            
            namespace Engagement.Agent
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

Pange tähele, et need andmed saate saata sammhaaval: ainult Viimane väärtus antud võti säilitab seadmele. Sündmuse lisad, nt kasutada sõnastik\<objekti, objekti\> informations manustamiseks.

### <a name="example"></a>Näide

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
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

            {"subscription":"2013-12-07","premium":"true"}
 
##<a name="logging"></a>Logimine
###<a name="enable-logging"></a>Logimise lubamine

SDK saab konfigureerida andes testi logid IDE konsooli.
Need logid ei aktiveerita vaikimisi. See kohandamiseks atribuudi värskendamiseks `EngagementAgent.Instance.TestLogEnabled` üks väärtus, mis on saadaval on `EngagementTestLogLevel` loendamine, näiteks:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
