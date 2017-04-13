<properties
    pageTitle="Androidi kaasamine API kasutamine"
    description="Uusima Android SDK - Androidi kaasamine API kasutamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Androidi kaasamine API kasutamine

See dokument on dokumendi [Android Mobile kaasamine SDK suvandid täpsemalt aruandlusteenuste](mobile-engagement-android-advanced-reporting.md)lisandmoodul. Pakub sügavus üksikasjade kaasamine API abil saate oma rakenduse statistika aruande.

Pidage meeles, et kui soovite ainult kaasamine teatada rakenduse seansid, tegevused, jookseb ja tehnilist teavet, siis lihtsaim viis on teha kõik teie `Activity` sub tunnid pärivad vastava `EngagementActivity` klassi.

Kui soovite teha rohkem, kui teil on vaja aruande rakenduse teatud sündmused, tõrgete ja töö, nt, või kui teil on teatada rakenduse tegevuste erineval viisil, kui üks rakendada soovitud `EngagementActivity` klassid, siis peate kasutama rakenduse kaasamine API-ga.

Kaasamine API on esitatud selle `EngagementAgent` klassi. Selle klassi eksemplar saab tuua helistades on `EngagementAgent.getInstance(Context)` staatiline meetod (Pange tähele, et selle `EngagementAgent` tagastatud objekt on soovitud singleton).

##<a name="engagement-concepts"></a>Kaasamine mõisted

Järgmised osad piiritleda levinud [Mobile kaasamine põhimõtet](mobile-engagement-concepts.md), Android platvormi.

### <a name="session-and-activity"></a>`Session`ja`Activity`

Kui kasutaja jääb rohkem kui mõne hetke jõude kaks *tegevuste*vahel, siis oma *tegevuste* jada jagatakse kaks erinevat *seansid*. Nende mõne sekundi nimetatakse "seansi ajalõpp teie".

*Tegevuste* on tavaliselt seotud ühe ekraanikuva taotluse, st *tegevuse* algab ekraanil kuvatakse ja lõpetab ekraani sulgemisel: see on juhul, kui kaasamine SDK on integreeritud, kasutades funktsiooni `EngagementActivity` tunnid.

Kuid *tegevuste* saab ka kaasamine API abil käsitsi kontrollida. See võimaldab tükeldamiseks antud ekraanil mitu sub osad, et saada täpsemat teavet selle ekraani (nt teadaolevad sageduse ja kaua dialoogid kasutatakse Kuva sees) kasutamist.

##<a name="reporting-activities"></a>Tegevuste teatamine

> [AZURE.IMPORTANT] Ei pea te aruande tegevusi nagu järgmises jaotises kirjeldatud, kui kasutate funktsiooni `EngagementActivity` klassi ja selle variandid, nagu on selgitatud, kuidas integreerida kaasamine Androidi dokumendiga.

### <a name="user-starts-a-new-activity"></a>Kasutaja käivitab uue

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Helistage `startActivity()` iga kord, kui kasutaja tegevuste muutub. Selle funktsiooni esimene käivitab uue kasutaja seansiga.

Iga tegevuse on parim koht, kus see funktsioon call `onResume` tagasihelistamise.

### <a name="user-ends-his-current-activity"></a>Kasutaja lõpeb oma praeguse töö

            EngagementAgent.getInstance(this).endActivity();

Helistage `endActivity()` vähemalt ühe korra siis, kui kasutaja lõpetab viimati. See teatab kaasamine SDK, et kasutajal on praegu jõude ja, et kasutaja seansi vaja sulgeda üks kord seansi ajalõpp teie aegub (kui helistate `startActivity()` enne seansi ajalõpp teie seansi jätkatakse lihtsalt).

Iga tegevuse on parim koht, kus see funktsioon call `onPause` tagasihelistamise.

##<a name="reporting-events"></a>Aruannete sündmused

### <a name="session-events"></a>Seansi sündmused

Seansi sündmused on tavaliselt kasutatakse kasutaja toimingud oma seansi jooksul teatada.

**Näide ilma andmeteta eest:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Näide eest andmetega:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Autonoomse sündmused

Vastuolus seansi sündmusi, võivad tekkida autonoomse sündmuste väljaspool seansi kontekstis.

**Näide:**

Oletagem, et soovite aruandes sündmuste leviedastuse vastuvõtja käivitamisel:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Aruannete tõrked

### <a name="session-errors"></a>Seansi tõrked

Seansi tõrked on tavaliselt kasutatakse tõrgete mõjutavad kasutaja oma seansi jooksul teatada.

**Näide:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Autonoomse tõrked

Vastuolus seansi tõrgete, autonoomne tõrked võivad ilmneda väljaspool seansi kontekstis.

**Näide:**

Järgmises näites kujutatakse tõrketeate, kui mälu saab telefoni madal teie rakendus protsessi töötamise ajal.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Aruandlusteenuste tööde haldamine

### <a name="example"></a>Näide

Oletame, et soovite oma sisselogimise protsessi kestuse aruanne.

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Aruande tõrgete töö käigus

Tõrgete võib olla seotud esitatava töö asemel on seotud praeguse kasutaja seansi.

**Näide:**

Oletagem, et soovite aruande tõrke ajal saate sisse logida protsessi:

[...] avaliku kehtetu sisselogimiskuva (konteksti kontekst,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Sündmuste aruandlusteenuste töö käigus

Sündmuste võib olla seotud esitatava töö asemel on seotud praeguse kasutaja seansi.

**Näide:**

Oletame, et meil on mõne suhtlusvõrgu ja kasutame töö, mida soovite aruande kogu aeg, kui kasutaja on serveriga ühendatud. Kasutaja saab kursis taustal, isegi kui ta kasutab mõnda muusse rakendusse või telefoni ühendatuna seega ei ole seansi.

Kasutaja saab sõnumeid saada tema sõprade, töö sündmus.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Täiendav parameetrid

Suvalise andmetega seotud sündmused, tõrkeid, tegevused ja -tööde haldamine.

Andmed saate liigendatud, kasutab Android's komplekt klassi (tegelikult on see toimib nagu eest parameetrite Androidi kavatsused). Pange tähele, et kogumile sisaldada massiivide või mõne muu kogumi eksemplarid.

> [AZURE.IMPORTANT] Kui lisate parcelable või sarjadesse jaotatav parameetrid, veenduge, et nende `toString()` meetod on rakendatud inimese loetavad stringi tagastamiseks. Sarjadesse jaotatav tunnid, mitte siirdamiseks väljad, mis pole sarjadesse jaotatav sisaldavate teeb Androidi krahh, kui kutsute`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Vähe massiivi eest parameetrite ei toetata, st see ei seeriasertide massiivina. Ei peaks teisendada need standard massiivi enne selle kasutamist eest parameetrid.

### <a name="example"></a>Näide

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Piirangud

#### <a name="keys"></a>Kiirklahvid

Iga Sisestage soovitud `Bundle` peab vastama tavalise järgmine avaldis:

`^[a-zA-Z][a-zA-Z_0-9]*`

See tähendab, et klahvid peab algama vähemalt ühe tähe, millele järgneb tähtede, numbrite või allakriipsutatud märkideks (\_).

#### <a name="size"></a>Suurus

Lisad on piiratud **1024** märki (üks kord kodeeritud JSON kaasamine teenus) kõne kohta.

Eelmises näites, JSON, mis on saadetud server on 58 tähemärki.

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Teenuserakenduse teabe teatamine

Saate käsitsi teatada jälgimise teabe (või muude rakenduste kindla teabe) abil soovitud `sendAppInfo()` funktsioon.

Pange tähele, et need andmed saate saata sammhaaval: ainult Viimane väärtus antud võti säilitab seadmele.

Sündmuse lisad, nt kasutatakse kogumi klassi abstraktne teenuserakenduse teabe, Pange tähele, massiivid või sub kimbud käsitletakse (kasutades JSON sariväljaanne) tasapinnalise stringidena.

### <a name="example"></a>Näide

Siin on saata kasutaja sugu ja sünnipäev proovi kood:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Piirangud

#### <a name="keys"></a>Kiirklahvid

Iga Sisestage soovitud `Bundle` peab vastama tavalise järgmine avaldis:

`^[a-zA-Z][a-zA-Z_0-9]*`

See tähendab, et klahvid peab algama vähemalt ühe tähe, millele järgneb tähtede, numbrite või allakriipsutatud märkideks (\_).

#### <a name="size"></a>Suurus

Teenuserakenduse teabe on piiratud **1024** märki (üks kord kodeeritud JSON kaasamine teenus) kõne kohta.

Eelmises näites, JSON, mis on saadetud server on 44 tähemärki.

            {"expiration":"2016-12-07","status":"premium"}
