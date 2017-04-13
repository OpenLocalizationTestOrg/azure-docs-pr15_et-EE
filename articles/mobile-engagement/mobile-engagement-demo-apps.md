<properties
    pageTitle="Azure'i Mobile kaasamine demo rakenduse | Microsoft Azure'i"
    description="Kirjeldatakse kui alla laadida, kuidas kasutada ja Azure Mobile kaasamine demo rakenduse kasutamise eelised"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure'i Mobile kaasamine demo rakenduse

Meil on avaldatud Azure Mobile kaasamine demo rakendus **iOS-i**, **Androidi**ja **Windowsi** platvormide jaoks loodud aidata leida kasulikke ressursse ja Lisateavet Mobile kaasamine.

Rakenduse abil saate:

- Kerge vaevaga kasulikud lingid Mobile kaasamine ressursid, nt videod, dokumendid, tugiteenuste foorumit ja kust tõsta soovitada funktsioone.
- Kogemus valimi teatised Mobile kaasamine ideede saamiseks oma mobiilirakenduste toetatud.
- Viide rakendamise abil saate uurida, kuidas rakendada Mobile kaasamine enda rakendusse. Saate teada, et:

    - Analytics andmete kogumise.
    - Täiustatud teatis stsenaariumid tüübid, nt *täisekraani interstitsiaalne* või *hüpikakende*rakendada.
    - Uuringute ja küsitluste rakendada.
    - Vaikne tõuketeatised andmed ja tõuketeatised stsenaariumid rakendada.   

## <a name="app-installation"></a>Rakenduse installimine
See rakendus on saadaval järgmised app salvestab:

- **Windowsi universaalne demo rakenduse**:

    - Laadige rakendus veebisaidil [Windowsi rakenduse talletada](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - Rakendus on välja töötatud Windows 10 Universal rakendusena. Lähtekoodi on saadaval [github](https://github.com/Azure/azure-mobile-engagement-app-windows).

- **iOS-i demo rakenduse**:

    - Laadige rakendus veebisaidil [Apple'i talletada](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
    - Rakendus on iOS-i kiire välja töötatud. Lähtekoodi on saadaval [github](https://github.com/Azure/azure-mobile-engagement-app-ios).

- **Androidi demo rakenduse**:

    - Laadige rakendus [Google Play talletada](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement)juures.
    - Lähtekoodi on saadaval [github](https://github.com/Azure/azure-mobile-engagement-app-android).

![Windowsi universaalne demo rakenduse][1]

![iOS-i demo rakenduse][2]
![Androidi demo rakenduse][3]


## <a name="usage"></a>Kasutus

Saate rakendust kasutada järgmisel viisil:

**Seadmes rakendus alla laadida rakenduste poest linkide (varem):**

>[AZURE.IMPORTANT] Te ei pea Azure'i konto või ühendust rakenduse tagasi end. Rakendus töötab sõltumatult.

- Kui teil on rakendus oma seadmes, siis saate avada vasakul menüüs Mobile kaasamine kasulik vahendite otsimiseks linkide kaudu.
- Oleme lisanud [teenuse RSS-kanali](https://aka.ms/azmerssfeed) see rakendus, et te olete alati värskendatud toote uusimate värskenduste kohta.
- Samuti saate avada läbi valimi teatis stsenaariumi kogemus teated, mis toetab Mobile kaasamine iga platvormi tüüp. Need teatised saab kogenud kohalik – mis on, saate klõpsata nuppude kuvamiseks on teatised komplekti on identne teabe edastamisel Mobile kaasamine platvormi ekraanidel.

![Windowsi rakenduse menüüd][4]

![IOS-i rakenduse menüüd][5]
![rakenduse menüüd Androidi jaoks][6]

**Lähtekoodi alla laadida GitHub linkide (varem):**

- Kui allalaaditud lähtekoodi, avage see vastav arenduskeskkond--XCode iOS-i, Androidi ja Windowsi jaoks mõeldud Visual Studio Androidi Studio.
- Meie [SDK integreerimine põhijuhised](mobile-engagement-windows-store-dotnet-get-started.md) peaksite järgima edasi, et olete selle rakenduse oma Mobile kaasamine tagaandmebaas eksemplar ühendust luua.
    - Teil on vaja konfigureerida ühendusstringi rakenduses.
    - Samuti peate tõuketeatised teatis platvormi oma rakenduse konfigureerimine.
- Märkate, et ise rakendus on kinnitatakse Mobile allikaid. Seetõttu, kui avate rakenduse pärast ühenduse tagasi lõpuks, saate küll vaadata vahekaardil **kuvar** kasutaja seansi, tegevused ja sündmused jne.
- Saate küll saata teatisi selle rakenduse oma Mobile lingid eksemplari, asemel kohalik teatised.
    - Siin saate lisada oma seadmesse testi seade rakenduse menüükäsku **saada seadme ID** abil. See annab teile seadme ID, mis seejärel registreerimist testi seade platvormi tagaandmebaas eksemplariga.

    ![Seadme ID Windowsis][7]

    ![IOS-seadme ID][8]
    ![Androidi seadme ID][9]

## <a name="key-features-of-the-demo-app"></a>Põhifunktsioonide demo rakenduse

- Selle rakendusega eelpool teil tootenumbri ressursside Mobile kaasamine käes. Linke saate läbida vasakpoolsest menüüst.

- Saate rakenduse-, teatiste iga platvormi kogemusi. Need teatised saab **ainult teatis**, kus klõpsates teatise lihtsalt avab kohalikke Kuva rakenduse (kasutades **sügav linkimise**)--või mõne **Web teadaanne**, kus saate Mobile kaasamine tagasi end, et teate klõpsamisel kuvada täiendavad HTML-sisu toomiseks.

    ![Rakenduse-ja teatised][29]

- IOS-i, peate sulgege rakendus rakenduse-, või süsteemi Tõuketeatiste kuvamiseks. Saate vaadata rakendamise kohta, kuidas lisada **toimingunuppudega**, nagu need, mis lisatakse see rakendus –, teatis *tagasiside* ja *ühiskasutus* (nii, et kasutaja võib kuluda toimingu otse teate enda).

    ![IOS-i rakenduse-ja teatised][11] ![IOS rakenduse-, teate kuvamine][14]

- Android, suvandid, mis on toetatud on mitmerealise teksti (**Suur tekst**) või lisamise teatise pilt (**Suur pilt**) teatis koos **Toimingunuppude** (nagu toetab iOS-i).

    ![Androidi rakenduse-ja teatised][12] ![Androidi rakenduse-, teate kuvamine][15]

- Opsüsteemis Windows 10, saate vaadata teatised arvuti ilmet. See teatis kuvatakse ka Windows 10 **Teatise Center**. Ei toetata lisades **Toimingunuppude** praegu Windows SDK.

    ![Windowsi rakenduse-, teatised][10] ![Rakenduse-, kuvamis Windowsis][13]

- Saate vaikimisi "Rakendusesisese" teatised iga platvormi kogemusi. See on kaheastmelise kogemus, kus kuvatakse **teade** aken esimesena. Kui klõpsate, avaneb täisekraanvaates **teadet**, mis on kuvatud järgmine pilt. Selle teate sisu pärineb teie Mobile kaasamine tagaandmebaas eksemplari. SDK on nii teatised ja teadaannete mallid. Saate hõlpsasti kohandada, nagu on näidatud selles demo rakenduse lisamine meie logo ja värvimine.  

    ![Klõpsake rakenduse teatised Windows][16]

    ![IOS-rakenduse teatised][17]  ![Android-rakenduse teatised][18]

    **iOS-i**, **Android**

- Saate Mobile kaasamine funktsiooni **kategooria** see vaikimisi kasutuskogemuse kohandamiseks. Demo rakenduse, olete näidanud kaks levinud viisi muutmiseks kogemus teatised. Pange tähele, et funktsiooni kategooria veel ei toetata Windows SDK.

    **Täisekraanvaate interstitsiaalne:**

    ![Klõpsake rakenduse teatis – interstitsiaalne kategooria][30]

    ![IOS interstitsiaalne kategooria][21]  ![Interstitsiaalne kategooria Androidi seadmes][22]

    **Hüpikteatise:**

    ![Klõpsake rakenduse teatis – hüpikakende kategooria][31]

    ![IOS hüpikteatise][19]   ![Hüpikteatise Androidi seadmes][20]

**iOS-i**, **Android**

- Mobile kaasamine toetab samuti eriotstarbelisi tüüpi nimetatakse **küsitluste**Rakendusesisese teatis. See võimaldab teil saata kiirülevaate vaatluste segmenditud rakenduse kasutajatele. Saate lisada küsimuste ja suvandite iga küsimus nagu järgmine pilt. See siis saada kuvatakse rakenduse kasutajale teatis – rakenduse nimega.   

    ![Küsitluse teatised][32]

    ![Küsitluse Windowsis][26]

    ![IOS-i ülevaade][27]   ![Küsitluse Androidi seadmes][28]

**iOS-i**, **Android**

- Mobile kaasamine toetab samuti vaikne **Andmete** tõuketeatised saatmine. Need teatised, saate saata andmed kataloogiteenusest (nt JSON andmed järgmises näites), mida saate teha rakenduse ja mingi toimingu. Kuidas oleme muutmine üksuse hinna valikuliselt abil andmete tõuketeatised on näiteks.

    ![Andmete tõuketeatised teatis][33]

    ![Andmete tõuketeatised teatis Windowsis][23]

    ![Andmete tõuketeatised teatis iOS][24]  ![Andmete tõuketeatised teatis Androidi seadmes][25]

**iOS-i**, **Android**

> [AZURE.NOTE] Saate vaadata üksikasjalikku üksikasjalikud juhised nende teadetest, klõpsates **siit juhised, kuidas need teatised Mobile kaasamine platvormi saatmiseks** valimi teatis mis tahes kuval.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
