<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine kasutajaliidese – REACHi kohta"
   description="Azure'i mobiilsideseadmete kaasamine kasutajaliidese ülevaade" 
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

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Kuidas alustada kasutamine ja haldamine sunnib jõuda oma lõppkasutajad

Kui SDK on täielikult integreeritud rakenduse, saate kasutamise alustamisel selle jaotise REACHi Tõuketeatiste kasutajatele rakenduse UI.  

## <a name="do-your-first-push-notification-campaign"></a>Tehke oma esimese tõuketeatised teatis turunduskampaania
-    Veenduge, et teie REACHi on integreeritud rakenduse SDK abil. 
-    Valige rakenduse
 
![First1][1]

-    Avage jaotis "REACHi" ja klõpsake nuppu "uus teadet"
 
![First2][2]

-    Uue turunduskampaania loomine ja pange sellele
 
 ![First3][3]

-    Valige, kuidas teate tuleks toimetada, nagu Rakendusesisese ainult
 
![First4][4]

-    Looge sõnum, mida soovite tõuketeatised
 
![First5][5]

-    Pealkiri võib kirjutada teatamise (valikuline).
-    Kirjutage tõuketeatised sõnumi sisu.
-    Pildi üleslaadimine Pange tähele, et selle faili maht ei tohi ületada 32,768 baiti.
-    Teil on olemas ka võimalus suvandite valimine, kuid selles õpetuses keskmes jaoks me näeme seda hiljem.

-    Valige sisutüüp, kui teate ainult
 
![First6][6]

-    Teie tõuketeatised turunduskampaania loomine ja selle kuvatakse loendis turunduskampaania.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testige oma tõuketeatised teatis turunduskampaania
![Test1][8]

-    Registreerige oma seade.
-    Klõpsake soovitud seade push märkeruutu.
-    Klõpsake nuppu "Test" push saatmiseks seade.
 
![Test2][9]

-    Kampaanias aktiveerimine
 
![Testi3][10]

-    Nüüd, kui olete loonud oma turunduskampaania peate aktiveerima teatamise kasutajatele lükata.
 
## <a name="send-personalized-pushes"></a>Saata isikupärastatud sunnib
-    Selles näites luuakse push, kus kohandatud tagasimakse koodi sisestatakse tõuketeatised teatis.
 
![Personalize1][11]

Isikupärastamissaidi töötab, asendades marker kaudu soovitud rakendus teave silt, et, peate veenduge, et kasutajal on õige rakendus teave esmalt määratletud. Selles näites suunatud kasutajatel on rakenduse teave sildi nimega rebate_code määratletud.
Nagu näete eespool tõuketeatised teate sisu sisaldab marker ${rebate_code}, mis näitab, et see on asendatakse rakenduse teave sildi tegeliku sisu.

> Hoiatus: Kui rakenduse teave sildi pole kasutaja määratletud, ei saa kasutaja push.

-    Tulemus
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>Saate isikupärastada tekst oma teatis
![Personalize3][13]

-    Pealkirja, teate, sh
-    Ja sõnumi sisu.
-    Teadaanne (teksti vaate või veebi) tüübi valimine
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>Teate kehasse võib-olla ka isikupärastatud:
-    Toimingu URL-i peaks soovite kohandada sihtleht
-    Pealkiri,
-    Sõnumi kehasse.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Eristada teie Push Notification (või vähendamine rakenduse)
-    Valige teatise saate push, valige rakenduse, vaadake jaotist "Jõuda", valige või loob tõuketeatised turunduskampaania ja lugege jaotist "Teade" tüüpi.
 
-    Klõpsake soovitud "kohaletoimetamise režiim".
-    Valige "Piirata tegevused" ruut, kui soovite teatise tekib teatud tegevuste (ekraani).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Välja ainult rakenduse" kohaletoimetamise režiim
![Differentiate2][16]

"Välja ainult rakenduse" kohaletoimetamise režiim pakub tõuketeatised teatis, kui rakendus on suletud. See on standardne tõuketeatised teatis.
Kui valite "välja ainult rakenduse", te peate olema lisanud serdid platvormi, mis rakenduse tuginedes (APNs-i või GCM).

### <a name="see-also"></a>Vt ka
-  [Apple Push teavitusteenuse – serdid](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9)Google Cloud sõnumside serdi] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"Rakendusesisesest ainult" kohaletoimetamise režiim
![Differentiate3][17]

"Rakendusesisesest ainult" kohaletoimetamise režiim pakub tõuketeatised teatis, kui rakendus töötab.
See teatis jaoks pole vaja minna APNs-i ja GCM süsteemis.
Saate rakenduse süsteemis oma lõppkasutajad saavutamiseks.
Saate täielikult kohandada teate ja otsustada, kus kuvatakse tegevuse (Kuva) teate.

### <a name="anytime-delivery-mode"></a>"Igal ajal" kohaletoimetamise režiim
Saate valida "Igal ajal" kohaletoimetamise režiim, tagab jõuate oma lõppkasutaja kas rakendus töötab või mitte.
Kui valite "Igal ajal", peate olema lisanud serdid platvormi, mis rakenduse tuginedes (APNs-i või GCM). 
 
## <a name="schedule-a-push-campaign"></a>Tõuketeatised turunduskampaania plaanimine
### <a name="plan-to-start-a-campaign"></a>Lepingu käivitamine lühiülevaade
![Shedule1][18]

See on 21 märts ja kas teate, et ja hööveldatud jaoks soovitud 22nd, märts keskööst juures. Pole teil vaja teha push kasutajaliidese ees! Saate planeerida eelnevalt nurga minutid teatised saadetakse.
-    Tühjendage ruudud "Pole" ruut ja valige Alusta aeg 
-    Valige soovitud kuupäev ja kellaaeg soovite alustada tõuketeatised turunduskampaania.

### <a name="plan-to-end-a-campaign"></a>Lõpetamiseks kampaaniat kavandamine
![Shedule2][19]

Soovite oma turunduskampaania peatamiseks 25 märts juures 3:00 pm, kuid te ei tea, ei saa seal seda teha.
Teil pole ees push kasutajaliidese! Saate planeerida aegsasti nurga minutid kampaaniat katkestab.
-    Klõpsake "Pole" ruut või valige end aeg
-    Valige soovitud kuupäev ja kellaaeg lõpetatava tõuketeatised turunduskampaania.

### <a name="end-a-campaign-manually"></a>Kampaaniat käsitsi lõpetamine
![Shedule3][20]

Vaikimisi on "puudub"-ruut on märgitud.
See tähendab, et kampaaniat algab kohe, kui saate aktiveerida REACHi jaotises ja lõpeb, kui jaotises REACHi katkestab.
 
> Märkus: Ilma lõppkuupäev loodud kampaaniat salvestada push seade ja Kuva see järgmine kord, kui rakendus on avatud, isegi siis, kui kampaaniat käsitsi on lõppenud.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Tõuketeatised teatise teksti vaade täiustamine
### <a name="what-is-a-text-view"></a>Mis on teksti vaade?
![TextView1][21]

Teksti kuvamine on hüpikakna tekst sisuga. Hüpikaken pärast lõppkasutaja on klõpsanud tõuketeatised teatis.
Teksti vaadete abil saate esitada oma lõppkasutaja rohkem sisu. See on ka võimalus esitada nagu hüpped rakenduse ümbersuunamist salvestada, avada veebilehe saatmine e-posti, alustades geo lokaliseeritud otsingu lehele kõne jne...

### <a name="example-text-view"></a>Näide: Teksti kuvamine
-    Jaotises "REACHi" oma tõuketeatised teatis turunduskampaania loomine ja pange oma turunduskampaania nimi
 
![TextView2][22]

-    Kirjutage sõnum, mis kuvatakse teade.
-    Valige teadaanne sisutüüp, "tekst"
 
![TextView3][23]

> Märkus: kui vajutada teksti vaate, alati tegemist teatise esmalt. 

- Teksti määramiseks (pärast valitud teksti teadaanne sisu, sub jaotis kuvatakse, mis võimaldab teil määrata teksti, mida soovite kuvada.)
 
![TextView4][24]

-    Kirjutage pealkirja, mis kuvatakse sõnumi ülaosas.
-    Kirjutage põhisisu teksti kuvamine.
-    Kirjutage sisu, mis kuvatakse nupu (toimingunupu võimaldab teha teatud toimingu, nagu taotluse suunata rakenduste pood või mis tahes allikatest saate sisestada lehe avamine rakenduses).
-    Kirjutage sisu, mis kuvatakse nupp välju (klõpsates nuppu välju, Kuva tekst kaob.)
 
-    Teie tõuketeatised teatis turunduskampaania loomine ja see ei ilmu turunduskampaania loend.
 
![TextView5][25]

-    Kampaaniat tõuketeatised teatise saata kasutajatele teksti vaate aktiveerimine
 
![TextView6][26]

-    Tulemus
 
![TextView7][27]

-    Kasutaja saab teatis ja klõpsake seda.
-    Teksti kuvamine kuvatakse hüpikaken, mis võimaldab kasutajal seda.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Tõuketeatised teatis koos veebivaate täiustamine
### <a name="what-is-a-web-view"></a>Mis on Web vaade?
![WebView1][28]

Veebivaate on web sisuga hüpikaken. Hüpikaken on tõuketeatised teatamise lõppkasutaja klõpsamisel.
Veebivaate võimaldab teil on mitu suhtlemine lõppkasutaja.
See on ka võimalus esitada kõne ümbersuunamine nagu App Store'ist, veebilehe avamine, e-posti saatmine, geo lokaliseeritud otsingu alustamiseks jne...

### <a name="example-web-view"></a>Näide: Veebivaade
-    Jaotises "REACHi" oma tõuketeatised turunduskampaania loomine ja pange oma turunduskampaania nimi.
 
![WebView2][29]

-    Kirjutage sõnum, mis kuvatakse teade.
-    Valige teadaanne sisutüüp nimega "web"
 
![WebView3][30]

### <a name="about-announcement-types"></a>Teadaanne tüübid:
- Ainult teade: see on lihtne standardteatis. Tähendab, et kui kasutaja klõpsab ta, kuvatakse lisateavet ei kuvata, kuid ainult sellega seotud toimingu juhtub.
- Teksti teadaanne: on teade, et kasutajal on pilk teksti vaate tegeleb.
- Web teadaanne: on teade, et kasutajal on pilk veebivaate tegeleb.
Valige "Web teadaanne" sisu.

> Märkus: Kui vajutada web vaate, alati tegemist teatise esmalt.

- Määratlege veebisisu (pärast valitud teadaanne veebisisu kuvatakse selle lõikes, mis võimaldab määratleda vaate veebisisu, mida soovite kuvada.)

 
![WebView4][31]

-    Kirjutage pealkirja, mis kuvatakse sõnum (valikuline) ülaosas.
-    Kirjutage HTML-koodi siin.
-    Klõpsake redigeerimine nuppu väljaande ja vaadake, kuidas see näeb välja nagu allikas.
-    Kirjutage sisu, mis kuvatakse nupu (toimingunupu võimaldab teha teatud toimingu, nagu avamine taotluse suunata poodi või mis tahes saate sisestada allikatest lehele rakendus).
-    Kirjutage sisu, mis kuvatakse nupp välju (klõpsates nuppu välju, web vaate kaob).
 
-    Tulemus
 
![WebView5][32]

-    Kasutaja vastu teatis ja klõpsake seda.
-    Teksti kuvamine kuvatakse hüpikaken, mis võimaldab kasutajal seda.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
