<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine kasutajaliidese – REACHi" 
   description="Saate teada, kuidas jõuda rakenduse kasutajatele Tõuketeatiste abil Azure Mobile kaasamine" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Kuidas jõuda koos Tõuketeatiste rakenduse kasutajatele

Selles artiklis kirjeldatakse **Mobile kaasamine** portaali vahekaarti **saavutamiseks** . **Mobile kaasamine** portaali abil saate jälgida ja hallata oma mobiilirakendused. Pange tähele, et hakata kasutama portaali peate esmalt Looge konto **Azure Mobile allikaid** . Lisateavet leiate teemast [Azure Mobile kaasamine konto loomine](mobile-engagement-create.md).

UI REACHi osas on tõuketeatised turunduskampaania haldustööriista, kus saate luua/edit/aktiveerimine/valmis/jälgimine ja saada statistika tõuketeatised teatis kampaaniat ja funktsioone, mis pääseb ka API saavutamiseks (ja teatud elemente vähese Push API). Pidage meeles, et kas kasutate selle API-d või UI, peate integreerida REACHi oma rakendusse iga platvormi SDK, enne kui saate kasutada nii Azure Mobile kaasamine saavutamiseks kampaaniat.

>[AZURE.NOTE] Palju jaotisi **Mobile kaasamine** portaalile UI sisaldavad nupp **Kuva spikker** . Selle nupu abil kontekstipõhine lisateavet jaotise.

## <a name="four-types-of-push-notifications"></a>Nelja tüüpi tõuketeatised
1.    Teated - võimaldavad reklaami sõnumeid saata kasutajatele, et need suunamine teise asukohta rakenduse sees või saatmiseks veebilehe või väljaspool teie app store. 
2.    Küsitluste - võimaldavad teabe kogumine lõppkasutajad, esitades neile küsimustele.
3.    Andmete sunnib - võimaldavad kahendarvuks või base64 andmete faili saata. Andmete tõuketeatised sisalduvat teavet saadetakse teie kasutajate praeguse teenuse rakenduse muutmiseks rakenduse. Rakenduse peab olema võimalus andmeid tõuketeatised andmete töötlemiseks.

## <a name="campaign-details"></a>Turunduskampaania üksikasjad

Saate redigeerida, klooni, kustutamine või aktiveerimine kampaaniat, mis on pole veel aktiveeritud libistamisel üle nende nimed või nende avamiseks klõpsake. Te saate klooni kampaaniat, mis on juba aktiveeritud libistamisel üle nende nimed või nende avamiseks klõpsata. Kui see on aktiveeritud, ei saa siiski muuta kampaaniat.
 
![Reach1][18]

## <a name="reach-feedback"></a>Tagasiside saavutamiseks.

**Statistika** REACHi turunduskampaania üksikasjade kuvamiseks klõpsake nuppu. **Lihtsas** vaates pakub visuaalseks kujutamiseks veeru tulpdiagrammil kohta, kuhu on kadunud pärast kampaaniat on aktiveeritud kujul. **Täpsem** vaade pakub täpsema üksikasjade tõuketeatised turunduskampaania. Need andmed ei ole saadaval, kui saadate testi turunduskampaania st push saadetud testi seade. Siin on, kuidas peaks tõlgendada järgmisi üksikasju.

1. **Pushed** – seda määrab lükata seadmete teadete arv. Tõuketeatised turunduskampaania loomisel määratud sihtrühma sõltub see arv. Kui te ei määra mis tahes sihtrühma, siis see push saadetakse registreeritud seadmed. Nagu kõik muud tõuketeatised teenuseid, me ei push teatised otse seadmete, kuid hoopis murravad vastav platvormi teatud tõuketeatised teatis Services (PNS - APNs-i/GCM/WNS) nii, et nad saavad esitada teatised seadmete. 

2.  **Andis** – seda määrab Mobile kaasamine SDK vastuvõetud sõnumid, mis on edukalt esitatud PNS seadmele ja tunnistada arv. 
        
    *Miks andis loendamine on väiksem kui tõugatav count:*
    
    1. Kui kasutaja on seadmest eemaldada rakenduse, kuid PNS ei tea saadame push PNS ajal klõpsake sõnumi kõrvaldatakse.
    2. Kui seade on rakenduse, kuid ise seadmete võrguühenduseta pikka aega, siis PNS ei õnnestu edastatavat sõnumit seade. 
    3. Kui sõnum saada toimetada seade, kuid rakenduses Mobile kaasamine SDK ei tuvasta sõnumi sisu, siis langeb see sõnum. See võib juhtuda siis, kui rakenduse teatise kohandus genereeritud erandi, mida me jaole juba SDK ja drop sõnum. See võib ilmneda ka siis, kui seadme rakendus kasutab versiooni Mobile kaasamine Tarkvaraarenduskomplektist, mis ei ole uuema versiooni tõuketeatised sõnumi platvormi aru saama, kuid see on ainult siis, kui rakendus täiendati pärast teenuse platvormi oli saata teate. Kui palju sõnumeid jäeti ütleb vahekaarti **Täpsemalt** . 
    4. IOS-seadmetes sõnumite vahel ei saada on esitatud kas seade on vähese funktsioone, mis suurendab või kui rakenduse tarbib suurel hulgal power töötlemise ajal remote teatised. See on iOS-seadmete piirang.   

3.  **Kuvatakse** – seda määrab arvu sõnumeid, mis kuvatakse edukalt rakenduse kasutajale seadme süsteemi tõuketeatised/välja-ja-rakenduse teatis teatis administreerimiskeskuses või -rakenduse teatise mobiilirakenduse kujul.  Vahekaart **Täpsemalt** ütleb teile, kui palju olid süsteemi teatisi ja kui palju-rakenduste teatised. 
    
    *Miks kuvatakse loendamine on väiksem kui andis loendus (ootab kuvada)*
    
    1. Kui teate turunduskampaania oli lõppkuupäev seda, siis on võimalik, et teate toimetati, aga kui aeg tuli avamiseks ja kuvamiseks rakenduse kasutaja, see juba aegunud nii, et kunagi kuvati.   
    2. Kui teate on teatis – rakenduse, siis teade kuvatakse ainult rakenduse avamisel rakendus. Juhul, kui rakenduse kasutaja pole avada rakendus, SDK aruande, et teate on kohale toimetatud, kuid pole veel kuvatud, kuni rakendus on avatud. 
    2. Kui teate on teatis – rakendus ja konfigureeritud juhul teatud tegevuse/Kuva, siis teate teatada ka, nagu on kohale toimetatud, kuid seni, kuni kasutaja on esitatud avab rakenduse teatud ekraanil. 
    
4.  **Kasutaja tegevuste** – seda määrab arvu sõnumeid, mis on juba rakenduse kasutaja ja sisaldab sõnumeid, mis on kas actioned või lahkus. 

    - *Rakenduse kasutaja saab toimingu teavitus ühel järgmisel viisil:*
            
        1. Kui teate süsteemi/välja-ja-rakenduse teatise või teatis – rakenduse saatis nii teate ainult rakenduse kasutaja klõpsab teate.
        2. Kui teate on teatis – rakenduse teksti või veebivaade või küsitluste siis rakenduse kasutaja klõpsab teatises nuppu toiming.
        3. Kui teate on teatis – rakenduse web vaade, siis rakenduse kasutaja klõpsab URL-i vaates [Androidi ainult]
    
    - *Rakenduse kasutaja saab väljumine teavitus ühel järgmisel viisil:*
    
        1. Klõpsake teates nuppu Sule otse. 
        2. Nipsake eemal või kustutamise teate. 
        3. Tavaliselt kuvatakse-rakenduse teatisi teksti/veebisisu ja küsitluste rakenduse kasutaja kaheosaline toiming. Need esmalt näha teatis ja kui nad klõpsavad, nad näevad järgneva teksti/web/küsitluse sisu. Rakenduse kasutaja saab teatise kas neid juhiseid väljumiseks ja Täpsem vaade üksikasjad sisaldab see. 

5.  **Actioned** – seda määrab arvu sõnumeid, mis olid selgesõnaliselt actioned rakenduse kasutaja. See on kõige enam huvi arv, kui see on määranud mitu rakenduse kasutajad olid huvitatud olete lükanud teatise sõnum. 
 
> [AZURE.NOTE] IOS-i ja Windows platvormid, kui kasutajal on rakenduse avamiseks ja kampaaniat oli turunduskampaania "Igal ajal" siis on võimalik, et mõlemad välja rakenduse ja -rakenduste teatised kuvatakse samal ajal. See võib põhjustada kuvatakse arv on suurem kui selle andis. Kui kasutaja suhtleb või toimingute teate, siis isegi kasutaja kasutusviisid/Actioned arv võib olla suurem kui andis. 


![Määrus2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
