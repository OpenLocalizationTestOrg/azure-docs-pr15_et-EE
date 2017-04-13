<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine Tõrkeotsingujuhendi - SDK" 
   description="SDK integreerimine Azure Mobile kaasamine probleemide tõrkeotsing" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Juhend SDK integreerimise probleemide tõrkeotsing

Võimalikud probleemid võivad ilmneda ja kuidas Azure Mobile kaasamine ühendab arvesse rakenduse on järgmised.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>Teise ala rakenduse tõrke avastatud SDK probleemid

### <a name="issue"></a>Probleem
- Kasutajaliidese andmete kogumise tõrge (Analytics, jälgimine, osadeks või armatuurlaudade).
- Tõuketeatised tõrked (sunnib ei toimi rakenduses rakenduse või mõlemad).
- Täiustatud funktsioon tõrked (jälitus, geograafiline asukoht või platvormi teatud sunnib ei tööta).
- API tõrked (API-de fail sageli vaikselt ilma tõrketeated).
- Teenuse tõrgete (ükski Azure Mobile kaasamine töötab rakenduse).

### <a name="causes"></a>Põhjused

- Enamik probleemid, mis tuleb lahendada Azure Mobile kaasamine SDK on avastanud, ei saa (nt Kasutajaliidese andmete kogumise tõrge, tõuketeatised tõrge, täpsemate funktsioonide tõrge, API tõrge, rakendus jookseb või nähtava teenuse katkestuste).  
- Kui kindla funktsioon Azure Mobile kaasamine pole kunagi rakenduse enne töötanud, peate lõpule. 
- Kui kindla funktsioon Azure Mobile kaasamine töötas ja lõpetanud, peate versioon Azure Mobile kaasamine SDK viimasele versioonile. Pidage meeles, et on Azure Mobile kaasamine SDK iga platvormi Azure Mobile kaasamine (Android, iOS-i, Windowsi ja Windows Phone) ei toeta mõnda muud versiooni.

#### <a name="sdk-integration"></a>SDK integreerimine

- Azure'i Mobile kaasamine pole õigesti integreeritud SDK (Analytics).
- Saavutamiseks pole õigesti integreeritud SDK (rakenduse sisse ja välja rakenduse sunnib).
- Serdi aegunud või vale TOOTEKATALOOGI vs arendaja (ainult iOS).
- GCM või ADM pole õigesti integreeritud SDK (Androidi ainult - teenuse teatud sunnib).
- Jälitamise pole õigesti integreeritud SDK (installi store jälgimine).
- Rongile või GPS asukoha pole õigesti integreeritud SDK (sihtimine geograafilise asukoha järgi).


**Vt ka:**

- [SDK dokumentatsiooni - integreerimine juhikud][Link 5] 
- [Tõrkeotsingujuhendi - tõuketeatised][Link 23]

#### <a name="sdk-upgrade"></a>SDK versioonitäiendus

- Vaja uuendada SDK (sageli seotud seadme OS uuemad versioonid) SDK vanemate versioonidega probleemide lahendamiseks.
- Seadme rakenduse kõigi varasemate versioonide desinstallimine ja uuesti rakenduse uuesti registreerida uusim versioon seadme ID Azure Mobile kaasamine kasutajaliidese kinnitada, et teie seade kasutab rakenduse uusim versioon.

**Vt ka:**

- [SDK dokumentatsiooni - väljalaskemärkmed](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK dokumentatsiooni - versioonitäienduse juhikud](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Muud SDK

- Tõrgete rakenduse näidata "AndroidManifest.xml", võivad põhjustada Azure Mobile kaasamine pole töötamiseks (ainult Androidi).
- Levinud probleeme SDK integratsioon ja API kasutamine on segadusse SDK võti ja API võti.

**Vt ka:**

- [Mõisted - sõnastik][Link 6]

## <a name="advanced-coding-issues"></a>Täpsemad kodeerimise probleemid

### <a name="issue"></a>Probleem
-  Platvormi teatud kood ei ole otseselt seotud Azure Mobile kaasamine võib põhjustada probleeme iOS-i, Androidi ja Windows Phone.

### <a name="causes"></a>Põhjused

- Paljud Täpsemad kodeerimise probleemid Azure Mobile kaasamine on põhjustatud valesti kirjutatud platvormi teatud kood ei ole otseselt seotud Azure Mobile allikaid. Peate lugege lisaks Azure Mobile dokumentatsiooni (Android, iOS-i, Web, Windowsi ja Windows Phone) jaoks arenevad platvormi teatud dokumentatsiooni.
- "Kategooria" pole õigesti konfigureerimist takistab linkimise teatise mõnda muusse asukohta või sellest väljaspool app (ainult Androidi). 
- Pole säte "UIKit.framework" "valikuline" oma iOS-i kood, näitab "Symbol ei leitud tõrge" ja/või jookseb vanemate iOS-seadmetes (ainult iOS).
- Serdid on aegunud või tõuketeatised pole õigesti abil cert, põhjustab arendaja või tootekataloogi versiooni probleemid (ainult iOS).
- Kehtivad teatud piirangud omast platvorm, mis on Azure Mobile kaasamine ei saa määrata (nt süsteemi keskele tööpõhimõte Appist välja sunnib Facebook).
- Azure'i Mobile kaasamine avaldab kasutatavaid Azure Mobile kaasamine for iOS ja Android viide sisemise paketid täieliku loendi. Pidage meeles, et mõned funktsioonid Azure Mobile kaasamine on teatud platvormi (Androidi, iOS-i, Web, Windowsi ja Windows Phone).

### <a name="see-also"></a>Vt ka

 - [Tõrkeotsingujuhendi - tõuketeatised][Link 23] 
 - [SDK dokumentatsiooni - väljalaskemärkmed][Link 5]
 - [SDK dokumentatsiooni - versioonitäienduse juhikud][Link 5]

## <a name="application-crashes"></a>Rakendus jookseb

### <a name="issue"></a>Probleem
- Teie rakendus jookseb end kasutajate seadmes.

### <a name="causes"></a>Põhjused

- Krahh teavet saab vaadata *Analytics UI* või *Analytics API*
- Saate otsida seadme testi seadme ID-d ja rakenduse krahhi lõppkasutaja, mis aitab tuvastada teie krahh põhjus põhjustanud sama toimingu tegemine.
- Azure'i Mobile kaasamine SDK teadaolevad probleemid, mis põhjustada krahhi rakendused on mõnikord lahendada SDK uusimale versioonile üleminekut. Veenduge, et kontrollida väljalaskemärkmed, kohta oma jookseb uurimisel.

### <a name="see-also"></a>Vt ka

- [SDK dokumentatsiooni - väljalaskemärkmed][Link 5]
- [SDK dokumentatsiooni - versioonitäienduse juhikud][Link 5]

## <a name="app-store-upload-failures"></a>App store üles tõrked

### <a name="issue"></a>Probleem
- Apple, Google või rakenduse Windowsi poe rakenduse uusim versioon üleslaadimine seotud tõrgete.

### <a name="causes"></a>Põhjused

- Rakenduse talletab mõnikord Blokeeri rakendused lubatud teatud funktsioonid (nt Apple'i poest takistab IDFV kasutamine rakendustes poes ja GooglePlay poe takistab rakenduste vahel rakenduse teabe jagamine). 
- Veenduge, et kontrollida väljalaskemärkmed teie platvormi ja SDK praeguse versiooni kohta, kui teil on raskusi üleslaadimise rakenduste poest.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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
 
