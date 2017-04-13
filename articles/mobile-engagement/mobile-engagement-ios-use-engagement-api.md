<properties
    pageTitle="IOS-i kaasamine API kasutamine"
    description="Viimane iOS SDK - iOS-i kaasamine API kasutamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>IOS-i kaasamine API kasutamine

Selles dokumendis on lisandmooduli dokumenti integreerida kaasamine iOS-i kohta: sügavus kaasamine API abil saate oma rakenduse statistika aruande üksikasjade pakub.

Pidage meeles, et kui soovite ainult kaasamine teatada rakenduse seansid, tegevused, jookseb ja tehnilist teavet, siis lihtsaim viis on veendumaks, et kõik teie kohandatud `UIViewController` objektide pärivad vastava `EngagementViewController` klassi.

Kui soovite teha rohkem, kui teil on vaja aruande rakenduse teatud sündmused, tõrgete ja töö, nt, või kui teil on teatada rakenduse tegevuste erineval viisil, kui üks rakendada soovitud `EngagementViewController` klassid, siis peate kasutama rakenduse kaasamine API-ga.

Kaasamine API on esitatud selle `EngagementAgent` klassi. Selle klassi eksemplar saab tuua helistades on `[EngagementAgent shared]` staatiline meetod (Pange tähele, et selle `EngagementAgent` tagastatud objekt on soovitud singleton).

Enne kõnesid API funktsiooni `EngagementAgent` objekti tuleb lähtestada meetodi`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Kaasamine mõisted

Järgmised osad täpsustamise levinud [Mobile kaasamine põhimõtet](mobile-engagement-concepts.md) platvormi iOS-i jaoks.

### <a name="session-and-activity"></a>`Session`ja`Activity`

*Tegevuste* on tavaliselt seotud ühe ekraanikuva taotluse, st *tegevuse* algab ekraanil kuvatakse ja lõpetab ekraani sulgemisel: see on juhul, kui kaasamine SDK on integreeritud, kasutades funktsiooni `EngagementViewController` tunnid.

Kuid *tegevuste* saab ka kaasamine API abil käsitsi kontrollida. See võimaldab tükeldamiseks antud ekraanil mitu sub osad, et saada täpsemat teavet selle ekraani (nt teadaolevad sageduse ja kaua dialoogid kasutatakse Kuva sees) kasutamist.

##<a name="reporting-activities"></a>Tegevuste teatamine

### <a name="user-starts-a-new-activity"></a>Kasutaja käivitab uue

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Helistage `startActivity()` iga kord, kui kasutaja tegevuste muutub. Selle funktsiooni esimene käivitab uue kasutaja seansiga.

### <a name="user-ends-his-current-activity"></a>Kasutaja lõpeb oma praeguse töö

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Te peaksite helistama **mitte kunagi** ise seda funktsiooni, välja arvatud juhul, kui soovite tükeldada sisse mitu seanssi rakenduse üks kasutamine: kõne seda funktsiooni ei lõpe praeguse seansi kohe, edaspidised kõne `startActivity()` soovite alustada uut seanssi. See funktsioon automaatselt nimetatakse SDK, kui teie taotlus on suletud.

##<a name="reporting-events"></a>Aruannete sündmused

### <a name="session-events"></a>Seansi sündmused

Seansi sündmused on tavaliselt kasutatakse kasutaja toimingud oma seansi jooksul teatada.

**Näide ilma andmeteta eest:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Näide eest andmetega:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Autonoomse sündmused

Vastuolus seansi sündmusi, saab väljaspool seansi kontekstis eraldi sündmused.

**Näide:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Aruannete tõrked

### <a name="session-errors"></a>Seansi tõrked

Seansi tõrked on tavaliselt kasutatakse tõrgete mõjutavad kasutaja oma seansi jooksul teatada.

**Näide:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Autonoomse tõrked

Vastuolus seansi tõrkeid, saab autonoomse tõrgete väljaspool seansi kontekstis.

**Näide:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Aruandlusteenuste tööde haldamine

**Näide:**

Oletame, et soovite oma sisselogimise protsessi kestuse aruanne.

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Aruande tõrgete töö käigus

Tõrgete võib olla seotud esitatava töö asemel on seotud praeguse kasutaja seansi.

**Näide:**

Oletagem, et soovite oma sisselogimise käigus tõrketeate:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Sündmuste töö käigus

Sündmuste võib olla seotud esitatava töö asemel on seotud praeguse kasutaja seansi.

**Näide:**

Oletame, et meil on mõne suhtlusvõrgu ja kasutame töö, mida soovite aruande kogu aeg, kui kasutaja on serveriga ühendatud. Kasutaja saab sõnumeid saada tema sõprade, töö sündmus.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Täiendav parameetrid

Suvalise andmetega seotud sündmused, tõrkeid, tegevused ja -tööde haldamine.

Andmeid saab liigendatud, kasutab iOS-i 's NSDictionary klassi.

Pange tähele, et lisad võivad sisaldada `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` või muude `NSDictionary` eksemplari.

> [AZURE.NOTE] Täiendav parameeter on seeriasertide JSON. Kui soovite edasi kui ülal kirjeldatud eri objektid, peate oma tunni rakendama järgmisel viisil:
>
             -(NSString*)JSONRepresentation;
>
> Meetodit peaks tagastama JSON esituse objekti.

### <a name="example"></a>Näide

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Piirangud

#### <a name="keys"></a>Kiirklahvid

Iga Sisestage soovitud `NSDictionary` peab vastama tavalise järgmine avaldis:

`^[a-zA-Z][a-zA-Z_0-9]*`

See tähendab, et klahvid peab algama vähemalt ühe tähe, millele järgneb tähtede, numbrite või allakriipsutatud märkideks (\_).

#### <a name="size"></a>Suurus

Lisad on piiratud **1024** märki (üks kord kodeeritud JSON agent Engagement) kõne kohta.

Eelmises näites, JSON, mis on saadetud server on 58 tähemärki.

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Teenuserakenduse teabe teatamine

Saate käsitsi teatada jälgimise teabe (või muude rakenduste kindla teabe) abil soovitud `sendAppInfo:` funktsioon.

Pange tähele, et need andmed saate saata sammhaaval: ainult Viimane väärtus antud võti säilitab seadmele.

Sündmuse lisad, nagu on `NSDictionary` klassi kasutatakse abstraktne teenuserakenduse teabe, Pange tähele, massiivid või alamtäpi sõnastikud käsitletakse kui tasapinnalise stringe (kasutades JSON sariväljaanne).

**Näide:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Piirangud

#### <a name="keys"></a>Kiirklahvid

Iga Sisestage soovitud `NSDictionary` peab vastama tavalise järgmine avaldis:

`^[a-zA-Z][a-zA-Z_0-9]*`

See tähendab, et klahvid peab algama vähemalt ühe tähe, millele järgneb tähtede, numbrite või allakriipsutatud märkideks (\_).

#### <a name="size"></a>Suurus

Teenuserakenduse teabe on piiratud **1024** märki (üks kord kodeeritud JSON agent Engagement) kõne kohta.

Eelmises näites, JSON, mis on saadetud server on 44 tähemärki.

    {"birthdate":"1983-12-07","gender":"female"}
