<properties
   pageTitle="Azure'i mobiilsideseadmete kaasamine kasutajaliidese - Analytics"
   description="Saate teada, kuidas analüüsida andmeid rakenduse abil Azure Mobile kaasamine"
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>Rakenduse varasemate andmete analüüsimine

Selles artiklis kirjeldatakse **Mobile kaasamine** portaali vahekaarti **ANALYTICS** . **Mobile kaasamine** portaali abil saate jälgida ja hallata oma mobiilirakendused. Pange tähele, et hakata kasutama portaali peate esmalt Looge konto **Azure Mobile allikaid** .


Jaotise Kasutusanalüüsi UI kirjeldatakse liidetud rakenduse põhjal ajaloolisi andmeid värskendatakse kord 24 tunni jooksul. Teave kuvatakse eri armatuurlaudade rida/riba/sektordiagrammide, võrkude ja kaardid. Andmeid saab alla laadida ka csv-failina. Suurema osa sellest sama teavet on reaalajas UI jaotises jälgimine ja selle pääseb juurde ka Analytics API.

>[AZURE.NOTE] Palju jaotisi **Mobile kaasamine** portaalile UI sisaldavad nupp **Kuva spikker** . Selle nupu abil kontekstipõhine lisateavet jaotise.

## <a name="standard-and-custom-analytics"></a>Standard- ja kohandatud Analytics

Azure'i Mobile kaasamine pakub põhi-, analüütiline teave, mida saate graphed kohe, kui teie rakendus integreerida SDK rakenduste komplekti. Azure'i Mobile kaasamine pakub täiendavaid kohandatud analytics kohta teavet koguda soovitud oma lõppkasutajate käitumine võimalus. Selleks saate luua kohandatud "silte (rakenduse teave)", loodud **sätteid** nii, et Azure Mobile kaasamine saate täiendavate andmete kogumise teile plaan silt.



## <a name="analytics"></a>Kasutusanalüüsi
- Armatuurlaud: Kuvatakse Üldteave uue ja actives kasutajate ja nende trende.
- Kasutajad: Tuvastatakse kasutajad saavad identifikaator: see ID on kordumatu iga seadme jaoks (ühe uue kasutaja on tegelikult ühe uue seadme). Kui ta on selle ajavahemiku jooksul tema esimese seansi käsitletakse kasutaja antud ajapõhiste uue nimega. Kasutaja loetakse säilitatakse, kui ta on vähemalt ühe seansi viimase 7 päeva jooksul. Aktiivsed kasutajad on vähemalt ühe seansi antud perioodi jooksul tehtud kasutajatelt. Saate sortida, iga kuu, iga nädal, iga päev või tunni aja jooksul. Kõik diagrammid sarnanevad, kuid võimaldavad erinevaid funktsioone, näiteks teie rakenduse versiooni Filtreeri ja seejärel Sortimisalus teatud aja jooksul. Standardse integreerimise SDK abil kogutud teave sisaldab järgmist: klientide kasutatavaid aktiivsed kasutajad, uus kasutaja, seansside arv pikkus iga seansi, tehnilist teavet riik, kohalikud, asukoht, keele carrier, seadmed, püsivara, võrku (WIFI), rakenduse ja Tarkvaraarenduskomplektist, versioonid. Reaalajas jälgimine jaotises saavad seda teavet vaadata.

> Märkus: Aja jooksul põhineb kuupäeva kasutajate seadme sätted, et kasutaja nime, kelle telefon on valesti seatud kuupäev võib vale aja jooksul kuvataks.

- Säilituspoliitika: Käsitletakse kasutaja antud ajapõhiste säilitatakse, kui ta on selle ajavahemiku jooksul tema esimese seansi. Saate muuta tundi, päeva, nädala või kuu ajavahemikud, mille jooksul loendatakse säilitata kasutajad (ja uued kasutajad). Kasutaja säilituspoliitika analytics on ehitatud kohortide peal. Kohorti on kõik antud perioodi tuvastatud uute kasutajate määramine (st nende esimese seansi selle aja jooksul läbimiseks kasutajate seatud). Kasutame kohortide 1-ks, 2-ks, 4 päeva, 7-päevase või 1 kuu. Antud kohorti, iga 1-ks, 2 päeva, 4 päeva, 7-päevase või 1 kuu, Azure mobiilsideseadmete kaasamine arvutab kogumi kõik kasutajad, kes kuuluvad kohordi ja on endiselt aktiivne (st kasutajatele, kellel on vähemalt ühe seansi jooksul tehtud seatud). Nende kasutajate nimetatakse kohordi versioon. (Azure Mobile kaasamine saate kuvada mitu kasutajad on endiselt kasutades oma rakenduse, kuid ainult platvormi teatud poe teile öelda teie kasutajate mitu desinstallitud app – näiteks GooglePlay iTunes, Windowsi poodi, jne.).
- Seansid: Üks kasutaja rakenduse kasutamine. Seansid luuakse jada kasutajate tegevus (tegevuse on tavaliselt seotud ühe ekraanikuva rakenduse kasutamist, kuid see võib erineda sõltuvalt sellest, kuidas SDK on integreeritud rakenduse). Kasutaja saab ainult ühe tegevuse teha korraga: seanss algab kohe, kui kasutaja alustab oma esimese tegevusega ja peatub, kui ta lõpetab oma Viimati aktiivne. Kui kasutaja jääb rohkem kui mõne hetke ilma tegevust, on oma tegevuste jada kaks erinevat seansid jagatud.
- Tegevuste: Iga rakenduse Kuva nimed ja pikkus, aeg kasutajad veedavad iga ekraani. Tegevused on kohandatud analüütiline suvand, mis vastavad "rakenduse info" Sildid oma rakenduse seadistatud:
- Kasutaja Path: Näitab, kuidas teie kasutajad liikuda rakenduse tegevuste (ekraani). Saate teisaldada üksikasjad taseme reguleerimiseks liugurit. Sinine sõlmed tähistada rakenduse tegevusi. Nende maht on proportsionaalne aeg kasutajate kulunud see. Valge sõlmed tähistavad seansi käivitamise ja lõpetamise. Punane sõlmed tähistavad jookseb. Linkide tähistavad üleminekud rakenduse tegevuste (või tegevuste ja jookseb vahel). Sõlme või lingi kohtspikker oma andmete kohta täpsema teabe kuvamiseks klõpsake nuppu: üleminekud arvu ja allika tegevuse üleminek sihtkoha tegevusele protsent kindla aknas kulunud aeg. (On---60%---> B tähendab, et kasutajad seda, kui aktiivsus läheb tegevuse B 60% ajast.) Graafik saab ümber, nagu soovite seda täpsustada; positsioon salvestatakse iga kord, kui muudate. Saate kuvada või peita jookseb heledamaks graafik.
- Sündmuste: Teatud võetud rakenduse kasutaja. Jaotuse sündmused on kujutatud kasutaja ühe seansi kohta sündmuste arv. Sündmuse tähistab kiirtoiming, näiteks klõpsake nupu või teatise saamist. (Sündmuste tähendust sõltub kuidas SDK on integreeritud rakenduse.) Sündmuse võib ilmneda seansi lõpetamine või töö käigus või võib olla eraldiseisva.
- Töö: Sarnane sündmusi, kuid nad keskenduda toimingu pikkus. Näiteks töö võib öelda tehnilist teavet, kui kaua võtab sisu laadi või veebiteenus kõne. Võib ka kuvada, kui kaua võtab kasutaja vormi täitmine, looge konto või ostma. Töö tähistab ülesande kestus, näiteks allalaadimine ülesande kestus aeg banner kuvatakse ekraanil. (Töö tähendust sõltub kuidas SDK on integreeritud rakenduse.) Töö on tavaliselt seotud tausttoimingud, mis on läbi väljaspool seanss (st ilma kasutaja tegevus).
- Technicals: Tehnilist teavet seadmete kasutajate rakenduse, et saate jälitada, näiteks lokaadi, Carrier, võrku, seade, püsivara, ja akna suuruse kasutajate seadmed ja rakenduse versiooni ja rakenduse SDK versioon.
- Tõrked: Teave rakenduse sees tehnilisi vigu, mis põhjustavad rakenduse krahhi. Tõrge tähistab kiirsõnumite probleeme, näiteks võrgu tõrke või vigased stringimanipuleerimisfunktsioonide. (Sündmuste tähendust sõltub kuidas SDK on integreeritud rakenduse.) Tõrge võib ilmneda seansi lõpetamine või töö käigus või võib olla eraldiseisva.
- Jookseb: Teave oma rakenduse krahhi põhjustada tõrkeid. Krahhi on ootamatute seisund, kus lõpetab oodatud jõudlus ja peatada. Krahhi on tavaliselt rakenduse vea tõttu.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Juurdepääs säilituspoliitika ülevaade
![Analytics3][12]

Säilituspoliitika ülevaade on jaotatud iga, kus on kuvatud ülevaade teatud säilitusperiood sisse mitu kaarti, keskel. Käesolevas näites on näha 2-päevase säilitusperiood. Muud kaardid Kuva 4 päeva ja 7-päevase säilitavad.

## <a name="understanding-the-retention-overview-cards"></a>Säilituspoliitika ülevaade kaardid mõistmine
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Iga kaart koosneb 3 põhiosa:
1. 1: kohordi ja perioodi
2. 2 – 4: säilituspoliitika praeguse perioodi kohta.
3. 5: minigraafika ajalugu

### <a name="here-is-detailed-information-about-each-element"></a>Siit leiate üksikasjalikku teavet iga element:
1.    Kohordi ja perioodi: seda pealkirja annab kohordi tüüp. Siin "2 päeva jooksul" tähendab, et kasutajad toimimist käsitleme üle 2 päeva, 2 päeva järgmisega Blocks ühenduse kasutajad, mis saabusid üle 2 päeva ja kas. Ülaltoodud näites leiab tegevust kasutajate 21 ja 22 November vahel.
2.    See annab säilitamise määr on 21 ja 22 November saabuvad 19 ja 20 November kasutajate jaoks. Siin oli 1 aktiivsele kasutajale 21 ja 22, 3, mis olid uute kasutajate 19 ja 20 vahel üle.
3.    See visuaalne näidik annab sama teavet ülaltoodud graafiliselt. (Kolmanda ringi on arv 33%). Värvi annab Lisateavet: roheline näitab, et see arv kasvab eelmise arvutusest. Kollane tähendab, et ühed ja punane tähendab väheneb.
4.    See näitab väärtuste arvutamiseks.
5.    See on minigraafika ajaloo säilitamine väärtused. See võimaldab teil olema lai vaade, kuidas see kujunenud varem väärtuste kuvamiseks.


## <a name="see-also"></a>Vt ka

- [Mõisted][Link 6]
- [Tõrkeotsingu juhend teenus][Link 24]

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
