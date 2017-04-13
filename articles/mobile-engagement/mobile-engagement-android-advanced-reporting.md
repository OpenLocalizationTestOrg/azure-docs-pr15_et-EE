<properties
    pageTitle="Aruandlusteenuste Lisavalikud Azure Mobile Androidi SDK kaasamine"
    description="Kirjeldab, kuidas teha täpsemalt aruandlus jäädvustada analytics Azure Mobile Androidi SDK kaasamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Täpsemad aruandluse kaasamine Androidi seadmes

> [AZURE.SELECTOR]
- [Universaalne Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlighti](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS-i](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Selles teemas kirjeldatakse täiendavad aruannete stsenaariumid oma Androidi rakenduse. Saate rakendada loodud õpetusega [Alustamine](mobile-engagement-android-get-started.md) rakenduse suvanditest.

## <a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Õpetuse olete lõpule jõudnud oli tahtlikult vahetu ja lihtne, kuid olemas on Täpsemad suvandid, saate valida.

## <a name="modifying-your-activity-classes"></a>Muutes oma `Activity` tunnid

[Alustamine õpetuse](mobile-engagement-android-get-started.md)kõik teil oli oli teha oma `*Activity` alaliikide pärivad vastava `Engagement*Activity` tunnid. Näiteks kui teie pärand tegevuse laiendatud `ListActivity`, mida oleks laiendamine `EngagementListActivity`.

> [AZURE.IMPORTANT] Kui kasutate `EngagementListActivity` või `EngagementExpandableListActivity`, veenduge, et mõni kõne `requestWindowFeature(...);` enne kõne tehakse `super.onCreate(...);`, muidu krahhi.

Saate otsida neid tunde on `src` kausta ja kopeerige need oma projekti. Klassid on ka **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatiivne meetod: kõne `startActivity()` ja `endActivity()` käsitsi

Kui te ei saa või ei soovi ülekoormuse oma `Activity` tunnid, saate selle asemel käivitamine ja lõppkuupäeva oma tegevuste helistades on `EngagementAgent`'s meetodite otse.

> [AZURE.IMPORTANT] Android SDK kunagi helistab selle `endActivity()` meetod, isegi siis, kui rakendus on suletud (Android, mitte kunagi suletakse). Seega on *väga* soovitatav helistamiseks on `startActivity()` meetod on `onResume` tagasihelistamise *Kõik* oma tegevuste ja `endActivity()` meetod on `onPause()` tagasihelistamise *Kõik* oma tegevuste. See on ainus võimalus kindlasti seansid ei lekkinud. Kui seansi on lekkinud, kaasamine teenuse kunagi katkestatakse kaasamine kirjutamata (kuna teenuse jääb ühendatud kui seanss on ootel).

Siin on näide:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

Selles näites on sarnane selle `EngagementActivity` klassi ja selle variandid, mille lähtekood on esitatud selle `src` kausta.

## <a name="using-applicationoncreate"></a>Application.onCreate() abil

Mis tahes koodi asetate `Application.onCreate()` ja muus rakenduses kontekstiatribuuti on teie taotlus protsessid, sh kaasamine teenuse käivitamine. See võib olla tagajärgi, nt mittevajaliku mälu jaotus ja teemad on kaasamine protsessi või dubleeritud leviedastuse vastuvõtjad või teenuste.

Kui te alistada `Application.onCreate()`, soovitame lisada järgmised koodilõigu alguses oma `Application.onCreate()` funktsioon:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Sama saate teha `Application.onTerminate()`, `Application.onLowMemory()`, ja `Application.onConfigurationChanged(...)`.

Saate laiendada `EngagementApplication` laiendamise asemel `Application`: selle tagasihelistamise `Application.onCreate()` ei kontrolli protsess ja kõnede `Application.onApplicationProcessCreate()` ainult juhul, kui praeguse pole see majutusteenuses kaasamine, samu reegleid rakendamine muude kontekstiatribuuti.

## <a name="tags-in-the-androidmanifestxml-file"></a>Siltide AndroidManifest.xml faili

Teenuse silt failis AndroidManifest.xml on `android:label` atribuut võimaldab valida kaasamine teenuse nimi lõppkasutajatele "Töötab teenuste" oma telefoni ekraanil kuvatakse. Soovitame selle atribuudi säte `"<Your application name>Service"` (nt `"AcmeFunGameService"`).

Määrab selle `android:process` atribuut tagab, et kaasamine teenuse töötab eraldi protsessi (töötab kaasamine samasugune toiming nagu rakenduse muudab oma põhi-/ UI lõime vähem potentsiaalselt tundlik).

## <a name="building-with-proguard"></a>Hoone ProGuard

Kui koostate oma rakendusepaketi koos ProGuard, peate säilitada mõned tunnid. Saate teha järgmist konfiguratsiooni koodilõigu.

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
