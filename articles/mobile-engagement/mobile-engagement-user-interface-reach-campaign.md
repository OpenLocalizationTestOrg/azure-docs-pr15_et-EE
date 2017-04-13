<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine kasutajaliidese – REACHi turunduskampaania" 
   description="Laern kuidas luua ja hallata tõuketeatised teatis kampaaniate abil Azure Mobile kaasamine" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Kuidas luua ja hallata tõuketeatised teatis kampaaniat
Jaotise REACHi UI abil saate luua uue tõuketeatised turunduskampaania keerukate valemi abil esitada kõik andmed, mida vajate tõuketeatised teatise saatmine. Tõuketeatised turunduskampaania suvandi veidi erineda sõltuvalt turunduskampaania nelja tüüpi: Teadaanded, küsitluste, andmete sunnib ja paanid (ainult Windows Phone).

### <a name="option-applies-to"></a>Võimalus kehtib:
- Keeled: Kõik (teated, küsitluste, andmete sunnib, paanid)
- Turunduskampaania: Kõik (teated, küsitluste, andmete sunnib, paanid)
- Teatis: Teadaanded, küsitluste
- Sisu: Iga turunduskampaania jaoks kordumatu
- Sihtrühm: Kõik (teated, küsitluste, andmete sunnib, paanid)
- Aja jooksul: Teadaanded, küsitluste, paanid
- Test: Kõik (teated, küsitluste, andmete sunnib, paanid)
 
![REACHi-Campaign1][20]

## <a name="languages"></a>Keeled
Saate saata oma tõuketeatised mõne muu versiooni seadmetes, kus on valitud muude keelte kasutamine keelte rippmenüü. Vaikimisi saate kõigis seadmetes sama tõuketeatised sõltumata sellest, mida nad on seatud kasutada keel. Oma seadmega määratud mõnes muus keeles kasutajad saavad Push vaikekeele versioon. Paljude suvandi tõuketeatised abil saate määrata alternatiivse sisu iga valite täiendavad keeled. 
 
![REACHi-Campaign2][21]

### <a name="language-differences-apply-to"></a>Keele erinevused rakendamiseks tehke järgmist.
- Keeled: Kordumatu keelte võib valitud vaikekeele Lisaks
- Turunduskampaania: Sama kõigi keelte jaoks
- Teatis: Kordumatu Lisaks vaikekeele iga keele jaoks
- Sisu: Kordumatu Lisaks vaikekeele iga keele jaoks
- Sihtrühm: Võib filtreerida eraldi keele kriteeriumi alusel
- Aja jooksul: kõigi keelte puhul sama
- Test: Saadetakse iga keele korraga
 
### <a name="supported-languages"></a>Toetatud keeltes:
- Araabia (ar) 
- Bulgaaria (bg) 
- Katalaani (ca) 
- Hiina (zh) 
- Horvaadi (hr) 
- Czech (CS) 
- Taani keele (da) 
- Hollandi (Holland) 
- Inglise (en) 
- Soome keele (Soome) 
- Prantsuse (Prantsusmaa) 
- Saksa (Saksamaa) 
- Kreeka (el) 
- Heebrea (ta) 
- Hindi (Tere) 
- Ungari (hu) 
- Indoneesia (id) 
- Itaalia (it) 
- Jaapani keele (ja) 
- Korea keele (ko) 
- Läti (lv) 
- Leedu (lt) 
- Malai (macrolanguage) (ms) 
- Norra (Bokmål) (NB!) 
- Poola (pl) 
- Portugali (pt) 
- Rumeenia (ro) 
- Vene keele (ru) 
- Serbia (sr) 
- Slovaki (sk) 
- Sloveeni (sl) 
- Hispaania (es) 
- Rootsi (sv) 
- Tagalogi (tl) 
- Tai (th) 
- Türgi (tr) 
- Ukraina (Suurbritannia) 
- Vietnami (vi) 
 
## <a name="campaign"></a>Turunduskampaania
Turunduskampaania jaotise abil saate seada nime ja kategooria oma turunduskampaania samuti nagu siis, kui plaanite tõuketeatised turunduskampaania sihtrühma osas ignoreerimiseks ja saatmist kampaanias API saavutamiseks (ja teatud elemente koos vähese Push API) kaudu. Kategooriaid saab kasutada kohandatud teatise malli põhjal eelmääratletud sätted juhtimine – rakenduse teatised. Saate oma olemasolevad "kategooria" kaudu saavutamiseks API loendit.

> Hoiatus: Kui kasutate suvandit "Ignoreeri publik, tõuketeatised saadetakse kasutajate kaudu API" "Turunduskampaania" osas REACHi turunduskampaania, ei saadeta kampaaniat automaatselt, peate saavutamiseks API käsitsi saata.
 
![REACHi-Campaign3][22]
 
### <a name="option-applies-to"></a>Võimalus kehtib:
- Nimi: kõik
- Kategooria: Teadaanded, küsitluste
- Ignoreeri publik, tõuketeatised saadetakse kasutajate API kaudu: kõik
 
## <a name="notification"></a>Teatis
Teatis jaotise abil saate seada põhisätted tõuketeatised kaasamise: Push, sõnumi, pildi rakenduse pealkiri või kui see on dismissible. Seadme platvormi on palju teatise sätteid. Saate valida, kas teie tõuketeatised saadetakse "app" või "vähendamine rakenduse" või mõlemad. (Pidage meeles, et kasutajad saavad "osalemine" või "loobuda" "välja rakenduse" sunnib veebisaidil operatsioonisüsteemi taseme oma seadmes ja Azure Mobile kaasamine ei saa selle sätte alistada. Ka pidage meeles, et saavutamiseks API tegeleb "rakenduses" ja "rakenduse out" sunnib. Tõuketeatised API saab käsitlema "välja rakenduse" sunnib liiga.) Sunnib saab kohandada, piltide või HTML-sisu, sh sügav väljaspool rakenduse või mõnda muusse asukohta rakenduses (Android SDK 2.1.0 või hiljem tahtlikult kategooriad vajalik). Saate muuta ikooni või iOS-i märk ja saata teksti või web sisu (HTML-sisu, URL-i lingi teise asukohta või sellest väljaspool rakenduse popup). Saate ka Androidi seadmete heliseda või push Värin. (Pidage meeles, et peate õige SDK õiguste Androidi näidata faili Helisemine või seadme Värin.) Praegu pole valdkonna standardse Androidi "suure pildi" suurused, kuna suurusega erinevad igas seadmes, kuid 400-x 100 pildid töötamine peaaegu igast ekraani suurusega.

### <a name="delivery-types"></a>Kohaletoimetamise tüüpi:
-    Ainult Appist välja: toimetatakse teate, kui kasutaja rakendust.
- Välja rakenduse ainult teatise jaoks on vaja serdi Apple või Google (APNs-i või GCM sert).
- Rakendusesisese ainult: teatis kuvatakse ainult siis, kui rakendus on installitud.
- Kasutaja kasutab teate Capptain süsteemis. Saate kohandada visuaalse paigutuse/kuva oma tõuketeatised.
- Igal ajal: Selle suvandi abil tagatakse saata teate kas rakendus töötab või mitte.

 
![REACHi-Campaign4][23]

### <a name="option-applies-to"></a>Võimalus kehtib:
- Teatis: Teadaanded, küsitluste
 
## <a name="content"></a>Sisu
Jaotise sisu abil saate muuta oma teadaandeid, küsitluste, andmete sunnib ja paanid (ainult Windows Phone) sisu. Tõuketeatised kampaaniat sisu kehtestamine on teatud tüüpi turunduskampaania. 

### <a name="see-also"></a>Vt ka
- [Kasutajaliidese dokumentatsiooni - saavutamiseks – Push sisu][Link 29]
 
![REACHi-Campaign5][24]

## <a name="audience"></a>Sihtrühma
Jaotises sihtrühm abil saate määratleda standardse loendi üksuste piirata oma turunduskampaania või kohandatud kriteeriumide alusel oma turunduskampaania piirangud. Suvandid, et piirata publikule standardsete võimaldab push uue- ja vana või ainult kohalikke tõuketeatised kasutajatele. Samuti saate kvoot kasutajad, kes saavad push arv piirata. Kuidas oma turunduskampaania on filtreeritud, et lisada ühe või mitme kriteeriumi target kasutajatele avaldise käsitsi saate redigeerida. Tippige avaldis sihtrühma käsitsi. Sellist avaldist tuleb konkreetselt määratleda kriteeriumid seoste. Kriteerium on kirjeldatud identifikaatori, mis peab algama tähega ja ei tohi sisaldada tühikuid. Seoste kriteeriumid saab kirjeldada abil "ja", "või", "ei" tehtemärgid kui ka "(",")". Näide: "Criterion1 või (Criterion1 ja mitte Criterion2)".

> Märkus: Paljude osalejatega kampaaniat kaasatud, suunamise skannimine serveripoolne võib olla aeglane, eriti siis, kui proovite alustada mitme kampaaniat, samal ajal.

- Võimaluse korral ainult alustamine ühe turunduskampaania korraga.
- Kõige ainult alustada neli kampaaniat korraga.
- Push ainult aktiivsed kasutajad (ruut "kaasata ainult need kasutajad, kes pääseb abil kohalikke tõuketeatised" ja "Kaasata ainult aktiivsed kasutajad"), et ainult teie kasutajad, kellel on installitud ja seda kasutada ikka tuleb kontrollida.
Kui osalejatel on määratletud, saate teada, mitu kasutajat saab selle tõuketeatised simulate nupp. Välja arvutada potentsiaalselt suunatud sihtrühma (see on hinnang juhuslike kasutajate valimi põhjal) teadaolevad kasutajate arv. Pange tähele, et kasutajatele, kellel on desinstallitud rakenduse ka sihtrühma osa, kuid ei pääse.

### <a name="see-also"></a>Vt ka
- [Kasutajaliidese dokumentatsiooni – REACHi - uus tõuketeatised kriteerium][Link 28]

![REACHi-Campaign6][25]

### <a name="edit-expression"></a>Avaldise redigeerimine
![REACHi-Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Piirang kehtib teie sihtrühm suvand:
- Kaasata alamhulka kasutajad: kõik (teadaannete küsitluste andmete sunnib, paanid)
- Kaasata ainult vana kasutajaid: kõik (teadaannete küsitluste andmete sunnib, paanid)
- Kaasata ainult uusi kasutajaid: kõik (teadaannete küsitluste andmete sunnib, paanid)
- Kaasata ainult jõude kasutajaid: Teadaanded, küsitluste, paanid
- Kaasata ainult aktiivsed kasutajad: kõik (teadaannete küsitluste andmete sunnib, paanid)
- Kaasata ainult need kasutajad, kes pääseb abil kohalikke Push: teated, küsitluste
 
## <a name="time-frame"></a>Aja jooksul
Jaotises ajavahemik abil saate määrata push saadetakse või võite ajavahemik tühjaks jätta kampaaniat kohe alustada. Pidage meeles, et selle lõppkasutajate ajavööndile võib alustada kampaaniat päev varem kui te ootate oma lõppkasutajatele Aasia ja väikeste pakettidena, sunnib seni, kuni kõik ajavööndid maailmas vastavad ajavahemiku määramine teie turunduskampaania saatmine. Lõpeta kasutajate ajavööndile võib põhjustada ka viivitused kampaaniat sest seda taotleda aeg enne alustamist push telefoni.

> Märkus: Kampaaniate ilma lõppkuupäev saab vahemälu sunnib kohalikult ja ikka need kuvada pärast kampaaniat käsitsi täieliku. Selle vältimiseks, konkreetset kampaaniat lõppaega.

### <a name="see-also"></a>Vt ka
- [Saavutamiseks – kuidas Tos – plaanimine][Link 3] 
 
![REACHi-Campaign8][27]

### <a name="settings-apply-to"></a>Sätete rakendamiseks tehke järgmist.
- Aja jooksul: Teadaanded, küsitluste, paanid
 
## <a name="test"></a>Test
Saate saata selle tõuketeatised testi oma seadme enne salvestamist kampaaniat jaotise Test. Kui olete konfigureerinud mis tahes kohandatud keeli kampaanias, saate iga keele push testida. Saate häälestada testi seadme "Minu konto".
> Märkus: Pole serveripoolne andmed on sisse logitud, kui kasutate nupp "test" sunnib, andmed on sisse logitud ainult reaal tõuketeatised kampaaniat jaoks.

### <a name="see-also"></a>Vt ka
- [Kasutajaliidese dokumentatsiooni - minu konto][Link 14]
 
![REACHi-Campaign9][28]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
