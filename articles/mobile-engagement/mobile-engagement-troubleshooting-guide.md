<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine tõrkeotsingu juhendid" 
   description="Tõrkeotsingujuhendi Azure mobiilsideseadmete kaasamine" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure'i mobiilsideseadmete kaasamine - Tõrkeotsingujuhendi

## <a name="introduction"></a>Sissejuhatus
Järgmine tõrkeotsingu juhend aitab teil mõista, root põhjustab mõne üldiselt näha probleemide ja võimaldavad teil oma tõrkeotsing. 

## <a name="general"></a>Üldine

Üldiselt, mida tuleks alati järgmist.

1. Veenduge, et olete läinud läbi kõik integreerimiseks, nagu on kirjeldatud meie [Alustamine õppetükid](mobile-engagement-windows-store-dotnet-get-started.md)
2. Kui kasutate platvormi SDK-d uusim versioon. 
3. Testige tegelik seadmes ja emulaator, kuna mõned probleemid, mis on omased emulaator ainult. 
4. Teil on pihta mis tahes piirangud/pidurdab Mobile kaasamine: mis on [siin](../azure-subscription-service-limits.md)
5. Kui te ei ole Mobile kaasamine teenuse taustväärtus ühendust luua või näha andmeid ei laadita pidevalt seejärel tagada, et puudub poolelioleva teenusejuhtumitega, märkides ruudu [siin](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>"Jälgimine" probleemid

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Ma ei näe vahekaardil kuvar kuvata seadmes
Vahekaardil kuvar kuvatakse teie Mobile kaasamine Platformi reaalajas ühendatud seadmete. Kui teil on silumine emulaator ja seadme, peaksite nägema vähemalt ühe seansi siin. Kui rakendus on jaotatud, siis kuvatakse aktiivsed g, kajastamiseks seadmetes, mis on ühendatud platvormi reaalajas. 

Kui te ei näe oma seadme vahekaardil kuvar siis on see tõenäoliselt SDK integreerimise probleemi. Mõned levinud toimingut tõrkeotsinguks on järgmised:

1. Veenduge, et kasutate õiget ühendusstringi mobiilirakenduses ja jaotise SDK võtmed ja mitte API klahvid jaotises on. Ühendusstringi loob teie mobiilirakenduse, kus näete seadme vahekaardil kuvar Mobile kaasamine rakenduse eksemplari. 
2. Windowsi platvormi – kui teie lehe alistab selle `OnNavigatedTo` meetod, veenduge, et kõne `base.OnNavigatedTo(e)`.
3. Kui Mobile kaasamine on integreerida olemasolevate Mobile'i rakendus, siis saate tagada, et teil on puudu mis tahes juhiseid vaadates täpsema integreerimise juhiseid [siin](mobile-engagement-windows-store-integrate-engagement.md)
4. Veenduge, et saadate vähemalt üks Kuva/tegevuse alistamine lehele, mille EngagementActivity sõltuvalt töötamise [Alustamine õpetused](mobile-engagement-windows-store-dotnet-get-started.md)kirjeldatud platvormi.

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Vahekaardil kuvar, kus seansi näen isegi kui mul on katkestatud või suletud rakendus / emulaator. 
Kui teil on ainult üks ühendatud platvormi sel hetkel ja kasutate emulaator, et avada rakendus, siis on see tõenäoliselt emulaator quirks. Üldiselt, peate veenduge, et te naaske avakuvale appi seansi lõpetamiseks edukalt emulaator. Lisaks Windowsi platvormi, samal ajal silumine Visual Studio abil võimalik, et peate veenduge, et Visual Studios, saate minge **Elutsükli sündmuste** menüüriba ja **Peata** seansi tõesti sulgemiseks klõpsake nuppu. Üksikasjad leiate [Windowsi õpik](mobile-engagement-windows-store-dotnet-get-started.md) . 

## <a name="analytics-issues"></a>'Analytics' probleemid

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Ma ei näe mingeid andmeid / menüü Analytics andmete värskendamist 
Analytics andmete arvutatakse ümber regulaarselt ja võib kuluda kuni 24 tundi selle värskendamise. Andmed ei ole reaalajas ja te näete seda värskendada sees see 24 tunni jooksul.
Veenduge, kuid et saadate atleast ühe ekraanikuva või tegevuse platvormi kirjutamata, kas olulise atleast ühel lehel, kus `EngagementActivity` või helistades `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Näen menüü analüüsi mõne seadme valesti jäädvustatud kuupäev/kellaaeg
Aja jooksul Analyticsi põhineb kuupäeva kasutajate seadme sätted. Seega veenduge, kas seade on õigesti seatud kuupäev. 

## <a name="segment-issues"></a>'Lõigu' probleemid

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Segmendi loodud ja see on kuvatud, on tuhm või andmeid ei kuvata
Lõigu loomine pole praegu reaalajas. See arvutatakse samal ajal kui analytics andmete liidetakse ja nii võib kuluda kuni 24 tundi. Kontrollige uuesti hiljem, kuid samal ajal veenduge ka, et teie mobiilirakendused on tõepoolest saatmise aluseks on olevatele lõigud andmetele. Nt Kui sündmus on kirjas "foo" pole saadetud mis tahes mobiilsideseadme kaudu, siis oleks lõigu andmete lõigu, mis on loodud EventName = foo kriteeriumina. Lisaks peaksite oma SDK integreerimine tagada teie mobiilirakenduse saadab andmed õigesti. 

## <a name="reach-or-push-notifications-issues"></a>"Jõuavad või tõuketeatised probleemid

### <a name="my-push-messages-are-not-being-delivered"></a>Tõuketeatised sõnumeid ei ole esitatud 

1. Proovige saata testi seadmesse esmalt, et tagada, et kõik komponendid - mobiilirakenduse, SDK ja teenus on õigesti ühendatud ja esitama Tõuketeatiste teatised. 
2. Saada alati lihtsam rakenduse-, teatis esmalt kaudu turunduskampaania, mis on ajastatud ja ega see on mis tahes määratud sihtrühma kriteeriumi. See on taas tõestamaks teatis Ühenduvus töötab õigesti. 
3. Kui teil on probleeme-rakenduste teatised saavutamisel siis ka on hea esimene samm proovige saata teatis rakenduse-, esmalt. 
4. Veenduge, et "Kohalikke Push" õigesti konfigureeritud oma mobiilsideseadmete rakenduse. Sõltuvalt platvormi see kas kaasata, võtmed (Android, Windows) või serdid (iOS). Vt [kasutajaliidese - sätted](mobile-engagement-user-interface-settings.md)
5. Teatiste võiks ka blokeeritud kasutaja kaudu mobiilne OS nii Appist välja tagada ei ole. 
6. Veenduge, et ei seate *ignoreerida publik, saadetakse kasutajate kaudu API tõuketeatised* suvand REACHi turunduskampaania **turunduskampaania** osas kuna see tagab, et Tõuketeatiste ainult tuleb saata API-de kaudu. 
7. Veenduge, et teie tõuketeatised turunduskampaania on testimist nii võrguühenduse allikana võimalike probleemide kõrvaldamiseks Wi-Fi- ja telefoni tehtemärk võrgu kaudu ühendatud seade.
8. Veenduge, et süsteemi kuupäev/kellaaeg oma seadme/emulaator on õige, kuna kõik sünkroonitud seadme takistab ka Push teatise teenuse võimalus teatiste. 

Mitu platvormi teatud tõrkeotsingu juhiseid:

1. **iOS-i** 

    - Veenduge, et serdid on lubatud ja iOS-i tõuketeatised järelejäänud. 
    - Veenduge, et õigesti konfigureerite *esitatakse toodangusertifikaat* rakenduse Mobile kaasamine. 
    - Veenduge, et teil on katsetamine on *real, füüsilise seadme.* IOS-i simulaatorit ei saa protsessi tõuketeatised sõnumeid.
    - Veenduge, et komplekt identifikaator mobiilirakenduses õigesti konfigureeritud. Lugege juhiseid [siin](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Kui katsetamine, kasutage mobiilsideseadmete ettevalmistamise profiili "Erakorralised" jaotuse. Te ei saa saadetakse teatis, kui teie rakendus on koostatud abil "Silumine"

2. **Android**

    - Veenduge, kas olete määranud õiged projektinumber mobiilirakenduse kaudu AndroidManifest.xml failis, millele järgneb \n märgi. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Veenduge, et te pole puudu või valesti konfigureeritud mis tahes õiguste Androidi näidata faili 
    - Veenduge, et lisate oma kliendi rakenduse projektinumber on sama konto kaudu, kui teil GCM serveri võti. Mis tahes lahknevuse kahe takistada oma sunnib läheb läbi. 
    - Kui te saate süsteemi teatisi, kuid ei-rakendus ja seejärel läbivaatus [Määrake ikooni teatised](mobile-engagement-android-get-started.md) tõenäoline siis määrate õige ikooni Androidi näidata faili. 
    - Kui saadate BigPicture teatis, siis veenduge, et kui teil on väline pilt serverid, siis nad peavad saama toe HTTP "GET" ja "HEAD".

3. **Windows**
    
    - Veenduge, et teil on kehtiv Windowsi poe rakenduse seotud rakendus. Visual Studio - on teil paremklõpsake projekti ja suvand "Seostada rakenduse abil Store" ja valige loodud Windowsi poe rakendus. Selle Windowsi poe rakenduse peaks olema sama, kus teil kohalikke tõuketeatised identimisteave konfigureerimiseks Mobile kaasamine portaalis.
    - Kui te saate rakenduse-, Tõuketeatiste, kuid mitte-rakenduse teatiste `EngagementOverlay` integreerimine seejärel veenduge, et juurkausta ruudustiku element on oma lehele. EngagementOverlay kasutab seda leiab xaml faili lisamiseks kaks web "Ruudustiku" esimene element vaatamist lehel. Kui soovite leida, kus web vaadete seatakse, saate määrata koordinaatvõrgu nimega "EngagementGrid", nagu see aga on teil tagada, et seal on piisavalt kõrgus ja laius kaks edasiste Web vaatamist, mis näitab teate ja järgmine teadaanne – rakenduse teatise nimega:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Tõuketeatised teatis/teadaanne loodud/kampaaniat ja isegi juhul, kui seda saata teate, on kuvatud kujul "Active". Mida see tähendab? 
**Turunduskampaania** Mobile kaasamine loodud nimetatakse nii pikaajalise tõuketeatised teatis tähendust sellepärast, et uute seadmete ühenduda teie platvorm mobiilse kaasamine need saadetakse automaatselt teate saate konfigureerida, kui need vastavad kriteeriumi kampaanias. See on pole ühe shot ühe teatise seadistamine. On teil käsitsi klõpsake lõpetada kampaaniat nii, et see ei saada edasi teatised nuppu **valmis** . 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Tõuketeatised turunduskampaania loodud ja ma olen teatiste edukalt siiski iga kord, kui ma avada rakendus, kuvatakse sama teate isegi siis, kui mul oli actioned enne? 
See on tõenäoliselt juhtub katse ajal testimine ja kui kasutate emulators või selle osa framework nagu TestFlight. Mis toimub? siit leiate veebisaidil iga rakenduse käivitamine eksemplari, see uue DeviceID hankimine ja saata meie taustväärtus Mobile kaasamine platvormi käsitlema uuele seadmele ja saata teate põhjustava. 

## <a name="getting-support"></a>Klienditugi

Kui te ei saa selle probleemi lahendamisega ise ja seejärel saate teha järgmist.

1. Olemasoleva Teemad StackOverflow Foorum ja [MSDN-i Foorum](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) probleemi otsimine ja kui mitte, siis küsimuse esitamine seal. 
2. Kui leiate funktsiooni puuduvad siis lisa/hääletada taotluse meie [UserVoice Foorum](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Kui teil on Microsofti tugiteenuste avatud tugiteenuse juhtumi pakkudes järgmisi üksikasju: 
    - Azure'i Tellimuse ID
    - Platvormi (nt iOS, Android jne)
    - Rakenduse ID
    - Turunduskampaania ID-d (tõuketeatised teatis probleemid)
    - Seadme ID
    - Mobile kaasamine SDK versioon (nt Android SDK v2.1.0)
    - Tõrke üksikasjade täpne tõrketeade ja stsenaarium
