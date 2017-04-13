<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine kasutajaliidese – REACHi kriteeriumi" 
   description="Saate teada, kuidas kasutada valimise kriteeriumid tõuketeatised kampaaniat saatmiseks valige alamhulga kasutajaid, kasutades Azure Mobile kaasamine" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Tõuketeatised kampaaniat saatmiseks valige alamhulga kasutajate suunatud kriteeriumide kasutamise kohta

Suunamise publikule nuppu "Uus kriteerium" kindlate kriteeriumide järgi on üks kõige tõhusamaid mõisted Azure Mobile kaasamine aitab teil saada oluline tõuketeatised asemel Rämpspost kõik kliendid vastata. Saate piirata publikule tavalise kriteeriumi põhjal ja määrata mitu inimest saavad teatise sunnib simuleerida.

**Vt ka:**

- [Kasutajaliidese dokumentatsiooni – REACHi - uue tõuketeatised turunduskampaania][Link 27]

## <a name="audience-criteria-can-include"></a>Sihtrühma kriteeriumid võib sisaldada.
- **Technicals:** Saate suunata sama tehnilist teavet, saate vaadata analüüsi- ja kuvari jaotiste põhjal. **Vt ka:** [Kasutajaliidese dokumentatsiooni - Analytics] [ Link 15], [UI dokumentatsiooni - jälgimine][Link 16]
- **Asukoht:** Rakendustes, mis kasutavad "reaalajas asukoha aruandlus" Geo hoides abil saate kasutada geograafilise asukoha kriteeriumi soovitud sihtrühm GPS asukohast. "Rongile ala asukoht aruandlus" kõne kasutada ka mobiiltelefoni asukohast soovitud sihtrühm ("reaalajas asukoht" ja "Rongile ala asukoht aruanne" tuleb aktiveerida SDK). **Vt ka:** [SDK dokumentatsiooni - iOS - integreerimine] [ Link 5], [SDK dokumentatsiooni - Android - integreerimine][Link 5]
- **Saavutamiseks tagasiside:** Saate suunata enda ja publiku eelmise REACHi teatised – REACHi tagasiside teadaandeid, küsitluste ja andmete sunnib oma tagasiside põhjal. See võimaldab teil paremini teie sihtrühm pärast kahte või kolme saavutamiseks kampaaniat, kui te esimest korda. See saab kasutajatele, kes juba teade sarnase sisuga, seades kampaaniat, et kasutajad, kes juba saanud teatud eelmise turunduskampaania ei saadeta välja filtreerida. Saate isegi välistada kasutajate kes kuuluvad teatud turunduskampaania, mis on endiselt aktiivne uue sunnib saajate hulgast. **Vt ka:** [Kasutajaliidese dokumentatsiooni - saavutamiseks – Push sisu][Link 29]
- **Installida jälgimine:** Saate jälitada teavet lähtuvalt sellest, kuhu kasutajad installitud rakenduse. **Vt ka:** [Kasutajaliidese dokumentatsiooni - sätted][Link 20]
- **Kasutajaprofiili:** Saate suunata põhjal standardhälbe kasutajateave ja te saate target põhjal kohandatud rakenduse teave, mille olete loonud. See hõlmab vastanud need seadmine rakenduses asemel ainult kuidas need on vastanud eelmise kampaaniat teilt teatud küsimustele kasutajate ja kasutajad, kes on sisse loginud. Kõik rakenduse teave teie rakendus kuvatakse selle loendi jaoks määratletud.
- Segmente: Saate ka teatud kasutaja käitumine, mis sisaldab mitme kriteeriumi põhjal target põhjal segmente, mille olete loonud. Kõik salvest määratletud oma rakenduse Kuva selles loendis. **Vt ka:** [Kasutajaliidese dokumentatsiooni - segmente][Link 18]
- **Rakenduse teave:** "Settings" jälgimiseks kasutaja saab luua kohandatud rakenduse teave sildid. **Vt ka:** [Kasutajaliidese dokumentatsiooni - sätted][Link 20]

## <a name="example"></a>Näide: 
Kui soovite lükata teadet ainult kasutajaid, mis on sooritatud toimingu Rakendusesisese ostu sub kogum.

1. Minge oma rakenduse sätete leht, valige menüü "Rakenduse info" ja valige "Uus rakendus info"
2. Uue kahendmuutujaga rakenduse teave nimega "inAppPurchase" registreerimine
3. Veenduge, et määrata selle rakenduse teave "TRUE", kui kasutaja sooritab edukalt mõne Rakendusesisese ostu rakenduse (sendAppInfo ("inAppPurchase",...) abil funktsioon)
4. Kui te ei soovi seda teha rakenduse, saate seda teha oma taustväärtus kaudu, kasutades seadme API)
5. Seejärel peate lihtsalt luua oma teadaanne piirata publikule kasutajatele, millel on määratud "inAppPurchase" kriteerium "true")
 
> Märkus: Suunamise peale rakenduse teave siltide kriteeriumide alusel nõuab Azure Mobile kaasamine teabe kogumiseks teie kasutajate seadmetest enne saatmist push ja nii võivad viivituse põhjustada. Sunnib keerukate tõuketeatised konfiguratsiooni suvandid (nt värskendamine märgid) ka edasi lükata. "Ühe shot" turunduskampaania tõuketeatised API kasutamine on absoluutne kiireim lükketeatiste meetodit Azure Mobile allikaid. Ainult rakenduse teave siltide tõuketeatised kriteeriumidena kasutamise REACHi turunduskampaania (kas saavutamiseks API või UI) on järgmine kiireim viis, kuna siltide rakenduse teave on talletatud serveripoolse. Muude suunatud kriteeriumide kasutamise tõuketeatised turunduskampaania on kõige parema paindlik, kuid tõuketeatised meetod, kuna Azure Mobile kaasamine päringu seadmete kampaaniat saatmiseks.
 
![REACHi-Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Kriteeriumi suvandid rakendamiseks tehke järgmist.
- **Technicals**     
- Püsivara nimi: püsivara nimi
- Püsivara versioon: püsivara versioon
- Seadme mudelist: seadme mudelist
- Seadme tootjalt: seadme tootjalt
- Rakenduse versioon: rakenduse versioon
- Vedaja nimi: vedaja nimi "määratlemata"
- Carrier riik: Carrier riik, määratlemata
- Võrgu tüüp: Network tüüp
- Lokaadi: lokaadi
- Ekraani suurus: ekraani suurus
- **Asukoht**      
- Teadaolev ala: riik, piirkond, asukoht
- Reaalajas geo hoides: HP loend (nime, toimingud), ringikujulise HP (nime, laiuskraad, pikkuskraad, raadiusega meetrit)
- **Tagasiside saavutamiseks.**     
- Teadaanne tagasiside: teadaanne tagasiside
- Tagasiside Küsitlus: küsitlus, tagasiside
- Vastus tagasiside Küsitlus: Küsitlus vastus tagasiside, küsimus, valik
- Andmete tõuketeatised tagasiside: andmete tõuketeatised tagasiside
- **Installi jälgimine**     
- Poe: Tööfailide talletamine, määratlemata
- Allikas: Allikas määratlemata
- **Kasutajaprofiili**     
- Sugu: mees või naine, määratlemata
- Sünniaeg: tehtemärk, kuupäev, mis on määramata
- Osalemine: true või false, määratlemata
- **Rakenduse teave**      
- String: Stringi "määratlemata"
- : Tehtemärk, kuupäeva määratlemata
- Täisarv: tehtemärk, arv, määratlemata
- Kahendmuutuja: väärtuse true või false, määratlemata
- **Lõigu**    
- Nimi (ripploendist), segmente välistamist (target kasutajad, mis pole osa selle lõigu).

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
 
