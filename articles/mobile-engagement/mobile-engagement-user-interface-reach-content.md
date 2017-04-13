<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine kasutajaliidese – REACHi sisu" 
   description="Saate teada, kuidas eri tüüpi tõuketeatised teatis kampaaniat rakenduses Azure Mobile kaasamine kordumatu sisu haldamine" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Eri tüüpi tõuketeatised teatis kampaaniat kordumatu sisu haldamine
 
Uue REACHi turunduskampaania sisu osas abil saate muuta oma teadaandeid, küsitluste, andmete sunnib ja paanid (ainult Windows Phone) sisu. Tõuketeatised kampaaniat sisu kehtestamine on teatud tüüpi turunduskampaania. 
 
### <a name="content-types"></a>Sisutüübid:
- Teadaanded
- Küsitluste
- Andmete sunnib
- Paanide (ainult Windows Phone)
 
## <a name="content-of-announcements"></a>Teadaannete sisu
 ![REACHi-Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Valige oma teadaanne tüüp:
-    Ainult teade: see on lihtne standardteatis. Tähendab, et kui kasutaja klõpsab ta, kuvatakse lisateavet ei kuvata, kuid ainult sellega seotud toimingu juhtub.
-    Teksti teadaanne: on teade, et kasutajal on pilk teksti vaate tegeleb.
-    Web teadaanne: on teade, et kasutajal on pilk veebivaate tegeleb.

### <a name="see-also"></a>Vt ka
- [Saavutamiseks – kuidas Tos - teated][Link 3] 

### <a name="about-web-view-announcements"></a>Veebi Kuva teated: kohta
Mustri "{deviceid}" HTML-koodi või JavaScripti koodi siin esinemiskordade automaatselt asendatakse identifikaator seadme teate kuvamise. See on lihtne viis tuua Azure Mobile kaasamine identifikaatorid välise veebiteenus, mis on majutatud tagasi Office'i rakenduses.
Kui soovite luua täisekraan veebivaade (ilma vaikimisi toimingu välju nuppe ja pakume) saate kasutada järgmisi funktsioone teie web vaade teadaanne JavaScripti koodi kaudu: 

-    toimingu teadaanne: ReachContent.actionContent()
-    väljakuulutamist väljumine: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Valige oma toiming:

### <a name="about-action-urls"></a>Toimingu URL-ID: kohta
Toimingu URL-i saate kasutada seda võidakse tõlgendada suunatud seadme operatsioonisüsteemi URL-i.
Mis tahes spetsiaalne URL, mis võivad rakenduse tugi (nt kasutajate liikuda kindlale ekraanile) saate kasutada ka toimingu URL-i.
Selle toimingu tegemiseks seadme identifikaator asendatakse automaatselt iga esinemiskorra {deviceid} mustri. See saab hõlpsalt toomiseks Azure Mobile kaasamine identifikaatorid kaudu välise veebiteenus, mis on majutatud oma tagasi office.

- **Androidi + iOS-i toimingud**
    - Avage veebileht
    - http://\[web-saidi domeen\] 
    - Näide: http://www.azure.com
    - Meilisõnumi saatmine
    - mailto:\[e-posti adressaat\]?subject =\[teema\]& sisu =\[sõnum\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - SMS saatmine
    - SMS:\[-telefoninumber\] 
    - Näide: sms:2125551212
    - Telefoninumbri valimiseks
    - Tel:\[-telefoninumber\] 
    - Näide: tel:2125551212
- **Androidi ainult toimingud**
    - Play poes rakenduse allalaadimine
    - Market://details?id=\[rakendusepaketi\] 
    - Näide: market://details?id=com.microsoft.office.word
    - Otsingu geo lokaliseeritud
    - Geo:0, 0?q =\[otsinguväljale päring.\] 
    - Näide: geo:0, 0? k = starbucks, Pariis
- **iOS-i ainult toimingud**
    - Laadige rakendus App Store'ist
    - http://iTunes.Apple.com/ [riik] [rakenduse nimi] /app/ /id [rakenduse id]? mt = 8 
    - Näide: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Windowsi toimingud
    - Avage veebileht
    - http://\[web-saidi domeen\] 
    - Näide: http://www.azure.com
    - Meilisõnumi saatmine
    - mailto:\[e-posti adressaat\]?subject =\[teema\]& sisu =\[sõnum\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Saata SMS (nõutav Skype'i poe rakendus)
    - SMS:\[-telefoninumber\] 
    - Näide: sms:2125551212
    - Valige telefoni number (nõutav Skype'i poe rakendus)
    - Tel:\[-telefoninumber\] 
    - Näide: tel:2125551212
    - Play poes rakenduse allalaadimine
    - MS-windows-store: PDP?PFN =\[rakenduse paketi ID\] 
    - Näide: ms-windows-store: PDP? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1
    - Bingmaps Otsingu alustamine
    - bingmaps:?q =\[otsinguväljale päring.\] 
    - Näide: bingmaps:? k = starbucks, Pariis
    - Kohandatud värviskeemi kasutamine
    - \[kohandatud värviskeemi\]://\[kohandatud värviskeemi http Authentication\] 
    - Näide: myCustomProtocol://myCustomParams
    - Kasutage paketi andmed (poe rakendus pikendamise lugeda nõutav)
    - \[kausta\]\[andmete\]. \[laiend\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Koostage jälgimise URL:
-    Vaadake jaotist "Settings" on <UI Documentation> jaoks juhiseid koostamise jälituse URL, mis võimaldab kasutajatel mõne muu rakenduse alla laadida.
 
### <a name="define-the-texts-of-your-announcement"></a>Teie teadaanne teksti määratlemine
Sisestage tiitel, sisu ja nupp tekstid oma teadaanne. Saate suunata tulevaste kampaaniat REACHi tagasiside, kuidas kasutajad vastanud kampaanias sihtrühma. Sihtrühmasid, saate vastavalt kas kampaanias oli lihtsalt lükata, vastatavate, actioned või lahkus tagasiside.

### <a name="see-also"></a>Vt ka
- [Kasutajaliidese dokumentatsiooni – REACHi - uus tõuketeatised kriteerium][Link 28]

## <a name="content-of-polls"></a>Küsitluste sisu
![REACHi-Content2][31] täitke pealkiri, kirjeldus ja nupp tekstid oma teadaanne. Seejärel lisage küsimused ja vastused küsimustele võimalusi.
Saate suunata tulevaste kampaaniat REACHi tagasiside, kuidas kasutajad vastanud kampaanias publik. Sihtrühma määramine põhinema kas kampaanias oli lihtsalt lükata, vastatavate, actioned või lahkus. Samuti saate sihtrühmasid, põhineb küsitluse vastuse tagasiside, kus küsimus ja vastus valik kasutatakse kriteeriumidena.

### <a name="see-also"></a>Vt ka
- [Kasutajaliidese dokumentatsiooni – REACHi - uus tõuketeatised kriteerium][Link 28]
 
## <a name="content-of-data-pushes"></a>Sunnib andmete sisu
![REACHi-Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Valige oma andmete tüüpi.
- Teksti
- Binaarandmeid
- Base64 andmed

### <a name="define-the-content-of-your-data"></a>Oma andmete sisu määratlemine
- Kui olete valinud teksti andmete edastamiseks, kopeerige ja kleepige soovitud tekst väljale "sisu".
- Kui valisite kahendarvuks või base64 andmete edastamiseks, "oma faili üles laadida" nupu abil oma faili üles laadida.
- Saate suunata tulevaste kampaaniat REACHi tagasiside, kuidas kasutajad vastanud kampaanias publik. Sihtrühma määramine põhinema kas kampaanias oli lihtsalt lükata, vastatavate, actioned või lahkus.

### <a name="see-also"></a>Vt ka
- [Kasutajaliidese dokumentatsiooni – REACHi - uus tõuketeatised kriteerium][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Sisu paanid (ainult Windows Phone)
![REACHi-Content4][33]

### <a name="define-the-content-of-your-tile"></a>Paan teie sisu määratlemine
Paani last on tekst kuvatakse teie rakendus opsüsteemis Windows Phone seadmed paani.
Paani tõuketeatised on kohalikke tõuketeatised for Windows Phone versioon Microsoft tõuketeatised teatise teenuse (MPNS). Paani tõuketeatised tüüp on ainult tõuketeatised tüüp, mis ei ole vastuse ja seega tulevaste kampaaniat publik ei saa tugineb paani tõuketeatised turunduskampaania tulemusi. 

### <a name="see-also"></a>Vt ka
- [API dokumentatsiooni – REACHi API - kohalikke tõuketeatised][Link 4]

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
 
