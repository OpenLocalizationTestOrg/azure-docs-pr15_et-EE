<properties
    pageTitle="Autoscaling ja rakenduse teenuse keskkonna | Microsoft Azure'i"
    description="Autoscaling ning rakenduse teenuse keskkonnas"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>Autoscaling ja rakenduse teenuse keskkonnas

Azure'i rakendust Service keskkonnad *autoscaling*. Saate autoscale töötaja kaustu mõõdikute või ajakava järgi.

![Töötaja rakenduskausta Autoscale suvandid.][intro]

Autoscaling optimeerib automaatselt kasvab ja kahanemine rakenduse teenuse keskkond, mis sobib teie eelarve ja/või profiili laadida oma ressursi kasutamine.

## <a name="configure-worker-pool-autoscale"></a>Töötaja rakenduskausta autoscale konfigureerimine

Pääsete autoscale funktsioonid töötaja pool vahekaarti **sätted** .

![Töötaja pool vahekaarti sätted.][settings-scale]

Sealt liides peaks olema üsna tuttav, sest see on sama kogemus, et näete, kui muudate mõne rakenduse teenusleping. 

![Käsitsi skaala sätteid.][scale-manual]

Samuti saate konfigureerida eraldi autoscale profiil.

![Autoscale sätted.][scale-profile]

Autoscale profiili on vaja määrata oma skaala. Sellisel viisil, peate ühtsete jõudluse, kogemusi, seades alampiir skaala väärtus (1) ja prognoositavad kulutada ots seadmisega ülemist (2).

![Profiili skaala sätteid.][scale-profile2]

Pärast ülesande määratlemist profiili, saate lisada autoscale reeglid mastaapimiseks üles või alla arvul töötaja kogumi profiili määratud piiridesse. Autoscale reeglid on mõõdikute põhjal.

![Skaala reegel.][scale-rule]

 Igal pool töötaja või ees mõõdikute saab määratleda autoscale reeglid. Need mõõdikud on sama mõõdikute saate jälgida ressursi blade graafikud või teatiste seadmine jaoks.

## <a name="autoscale-example"></a>Autoscale näide

Rakenduse teenuse keskkonna Autoscale võib kõige paremini illustreerida jalutuskäigul stsenaarium.

Selles artiklis selgitatakse kõik vajalikud kaalutlused autoscale häälestamisel. See artikkel juhendab teid kasutusviisid, mis tulevad mängu, kui te tegur autoscaling rakenduse teenuse keskkonnas, mis on majutatud rakenduse teenuse keskkonnas.

### <a name="scenario-introduction"></a>Stsenaarium tutvustus

Kalle on süsteemiadministraator, ettevõttele, kes on migreeritud töökoormus, mida ta haldab osa rakenduse teenuse keskkonnas.

Rakenduse teenuse keskkonna konfiguratsioon käsitsi skaala järgmiselt:

* **Ees lõpetatakse:** 3
* **Töötaja rakenduskausta 1**: 10
* **Töötaja pool 2**: 5
* **Töötaja pool 3**: 5

Töötaja rakenduskausta 1 kasutatakse tootmise töökoormus, töötaja pool 2 ja töötaja pool 3 kasutatakse kvaliteedi tagamine (KV) ja arengu töökoormus.

KV rakendust Service kavad arendaja on konfigureeritud käsitsi skaala. Tootmise rakenduse teenusleping on seatud autoscale tegelema laadimine ja liikluse muutused.

Kalle on väga tuttav rakendusega. Ta teab, on tippväärtus tunni jaoks laadi 9:00 AM ja 6:00 PL kuna see on line business (LOB) rakendus, mis töötajad kasutada, kui nad Office. Kasutus langeb pärast seda, kui kasutaja on valmis sel päeval. Väljaspool, on veel mõned laadi kuna kasutajad saavad kaugjuurdepääs rakenduse abil saavad mobiilsideseadmed või koduarvutis. Tootmise rakenduse teenusleping on juba konfigureeritud autoscale CPU hõivatus koos järgmiste reeglite alusel:

![LOB rakenduse teatud sätted.][asp-scale]

|   **Autoscale profiil – nädalapäevade – rakenduse teenusleping**     |   **Autoscale profiil – nädalavahetustel – rakenduse teenusleping**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Nimi:** WEEKDAY profiil                               |   **Nimi:** Nädalavahetuse profiil                               |
|   **Skaala järgi:** Plaanimine ja jõudluse reeglid            |   **Skaala järgi:** Plaanimine ja jõudluse reeglid            |
|   **Profiili:** Nädalapäevade                                   |   **Profiili:** Nädalavahetuse                                    |
|   **Tüüp:** Korduvus                                    |   **Tüüp:** Korduvus                                    |
|   **Sihtväärtust:** 5 kuni 20 eksemplari                     |   **Sihtväärtust:** 3-10 eksemplari                     |
|   **Päeva:** Esmaspäev, Teisipäev, Kolmapäev, Neljapäev, reede  |   **Päeva:** Laupäev, pühapäev                              |
|   **Alguskellaaeg:** 9:00 AM                                 |   **Alguskellaaeg:** 9:00 AM                                 |
|   **Ajavöönd:** UTC-08                                   |   **Ajavöönd:** UTC-08                                   |
|                                                           |                                                           |
|   **Autoscale reegli (skaala üles)**                           |   **Autoscale reegli (skaala üles)**                           |
|   **Ressurss:** Tootmise (rakenduse teenuse keskkond)      |   **Ressurss:** Tootmise (rakenduse teenuse keskkond)      |
|   **Mõõdiku:** CPU %                                       |   **Mõõdiku:** CPU %                                       |
|   **Toiming:** Rohkem kui 60%                         |   **Toiming:** Suurem kui 80%.                         |
|   **Kestus:** 5 minutit.                                 |   **Kestus:** 10 minuti                                |
|   **Ajastamine koondamine:** Keskmine                           |   **Ajastamine koondamine:** Keskmine                           |
|   **Toiming:** Suurendada arvu 2                         |   **Toiming:** Suurendada arvu 1                         |
|   **Lahedad allapoole (minutites):** 15                             |   **Lahedad allapoole (minutites):** 20                             |
|                                                           |                                                           |
  	|   **Autoscale reegli (skaala alla)**                     |   **Autoscale reegli (skaala alla)**                         |
|   **Ressurss:** Tootmise (rakenduse teenuse keskkond)      |   **Ressurss:** Tootmise (rakenduse teenuse keskkond)      |
|   **Mõõdiku:** CPU %                                       |   **Mõõdiku:** CPU %                                       |
|   **Toiming:** Vähem kui 30%                            |   **Toiming:** Vähem kui 20%                            |
|   **Kestus:** 10 minuti                                |   **Kestus:** 15 minutit                                |
|   **Ajastamine koondamine:** Keskmine                           |   **Ajastamine koondamine:** Keskmine                           |
|   **Toiming:** Count vähendamine 1                         |   **Toiming:** Count vähendamine 1                         |
|   **Lahedad allapoole (minutites):** 20                             |   **Lahedad allapoole (minutites):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>Rakenduse teenuse leping inflatsioon

Rakenduse Service plaanib konfigureeritud autoscale teete seda piirmäär tunnis. See saab arvutada vastavalt esitatud autoscale reegli väärtusi.

Mõistmine ja *rakenduse teenuse leping inflatsioon* arvutamisel on oluline rakenduse teenuse keskkonna autoscale Kuna töötaja pool skaala muudatused ei ole kohene.

Rakenduse teenuse leping inflatsioon arvutatakse järgmiselt:

![Rakenduse teenuse leping inflatsioon arvutamine.][ASP-Inflation]

Autoscale – skaala üles reegli Weekday profiili toote rakenduse teenusleping põhjal:

![Rakenduse teenuse leping inflatsioon nädalapäevade Autoscale – skaala üles reegli alusel.][Equation1]

Puhul Autoscale – nädalavahetuse profiili toote rakenduse teenusleping skaala üles reegli valem oleks lahendada, et:

![Rakenduse teenuse leping inflatsioonimäär nädalavahetustel Autoscale – skaala üles reegli alusel.][Equation2]

See väärtus arvutatakse ka skaala-alla-analüüsitoiminguid.

Põhjal Autoscale – skaala alla reegli Weekday profiili toote rakenduse teenusleping, näeks see järgmiselt:

![Rakenduse teenuse leping inflatsioon nädalapäevade Autoscale – skaala alla reegli alusel.][Equation3]

Puhul Autoscale – nädalavahetuse profiili toote rakenduse teenusleping skaala alla reegli valem oleks lahendada, et:  

![Rakenduse teenuse leping inflatsioonimäär nädalavahetustel Autoscale – skaala alla reegli alusel.][Equation4]

Rakenduse teenusleping tootmise võivad kasvada piirmäär kaheksa eksemplari tunnis nädala jooksul ning neli eksemplari tunnis ajal. See võib vabastada eksemplari kuni nelja eksemplari tunnis nädala jooksul määra ja kuus eksemplari tunnis nädalavahetustel.

Mitme rakenduse teenuse lepingute majutatakse töötaja pool, peate arvutada *kokku inflatsioon* rakenduse teenuse kõik lepingud, millele on inflatsioonimäär summana majutusteenuse selle töötaja kogumi.

![Mitme rakenduse teenuse lepingute töötaja pargis majutatavate kokku inflatsioon arvutamine.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Rakenduse teenuse leping inflatsioon abil saate määratleda töötaja rakenduskausta autoscale reeglid

Töötaja ühendab majutavate rakenduse Service plaanib konfigureeritud autoscale vaja eraldatud puhvri maht. Puhvri võimaldab autoscale toimingute kasvada ja vähendage rakenduse teenusleping vastavalt vajadusele. Minimaalne puhvri oleks arvutatud kokku rakenduse teenuse leping inflatsioonimäär.

Kuna rakendus keskkonnas skaala toimingud võtta aega rakendamiseks, mis tahes muutmine peaks arvestama nõudmisel edasisteks muutusteks, mis võib juhtuda siis, kui skaala toiming on pooleli. See latentsus mahutamiseks soovitame kasutada iga toimingu autoscale väikseima arvu eksemplarid, mis on lisatud arvutatud kokku rakenduse teenuse leping inflatsioonimäär.

Selle teabe, kalle saate määrata järgmised autoscale profiili ja reegleid:

![Autoscale profiili reeglid LOB näide.][Worker-Pool-Scale]

|   **Autoscale profiil – nädalapäevade**                        |   **Autoscale profiil – nädalavahetustel**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Nimi:** WEEKDAY profiil                               |   **Nimi:** Nädalavahetuse profiil                       |
|   **Skaala järgi:** Plaanimine ja jõudluse reeglid            |   **Skaala järgi:** Plaanimine ja jõudluse reeglid    |
|   **Profiili:** Nädalapäevade                                   |   **Profiili:** Nädalavahetuse                            |
|   **Tüüp:** Korduvus                                    |   **Tüüp:** Korduvus                            |
|   **Sihtväärtust:** 13 kuni 25 eksemplari                    |   **Sihtväärtust:** 6 kuni 15 eksemplari             |
|   **Päeva:** Esmaspäev, Teisipäev, Kolmapäev, Neljapäev, reede  |   **Päeva:** Laupäev, pühapäev                      |
|   **Alguskellaaeg:** 7:00 AM                                 |   **Alguskellaaeg:** 9:00 AM                         |
|   **Ajavöönd:** UTC-08                                   |   **Ajavöönd:** UTC-08                           |
|                                                           |                                                   |
|   **Autoscale reegli (skaala üles)**                           |   **Autoscale reegli (skaala üles)**                   |
|   **Ressurss:** Töötaja rakenduskausta 1                             |   **Ressurss:** Töötaja rakenduskausta 1                     |
|   **Mõõdiku:** WorkersAvailable                            |   **Mõõdiku:** WorkersAvailable                    |
|   **Toiming:** Väiksem kui 8                              |   **Toiming:** Vähem kui 3                      |
|   **Kestus:** 20 minutit                                |   **Kestus:** 30 minutit.                        |
|   **Ajastamine koondamine:** Keskmine                           |   **Ajastamine koondamine:** Keskmine                   |
|   **Toiming:** Suurendada arvu 8                         |   **Toiming:** Suurendada arvu 3                 |
|   **Lahedad allapoole (minutites):** 180                            |   **Lahedad allapoole (minutites):** 180                    |
|                                                           |                                                   |
|   **Autoscale reegli (skaala alla)**                         |   **Autoscale reegli (skaala alla)**                 |
|   **Ressurss:** Töötaja rakenduskausta 1                             |   **Ressurss:** Töötaja rakenduskausta 1                     |
|   **Mõõdiku:** WorkersAvailable                            |   **Mõõdiku:** WorkersAvailable                    |
|   **Toiming:** Suuremad kui 8                           |   **Toiming:** Suurem kui 3                   |
|   **Kestus:** 20 minutit                                |   **Kestus:** 15 minutit                        |
|   **Ajastamine koondamine:** Keskmine                           |   **Ajastamine koondamine:** Keskmine                   |
|   **Toiming:** Arvu 2 vähendamine                         |   **Toiming:** 3 arv väheneb                 |
|   **Lahedad allapoole (minutites):** 120                            |   **Lahedad allapoole (minutites):** 120                    |

Target vahemiku määratletud profiili arvutatakse minimaalne eksemplarid, mis on määratletud rakenduse teenusleping + puhvri profiili.

Maksimum vahemiku oleks kõik lubatud vahemikud töötaja kogumi majutatud rakenduse teenuse lepingute summa.

Suurendamine count skaala reeglid tuleks luua vähemalt 1 X skaala inflatsioonimäär rakenduse teenuse leping.

Vähenda count saab reguleerida midagi 1/2 X või 1 X rakenduse teenuse leping inflatsioonimäär skaala vahel alla.

### <a name="autoscale-for-front-end-pool"></a>Autoscale jaoks ees pool

Reeglid ees autoscale on lihtsam kui töötaja kaustu. Eelkõige, tuleks  
Veenduge, et selle kestus mõõtmise ja cooldown taimerid võtke arvesse, et mõne rakenduse teenusleping skaala toiminguid ei ole kohene.

Selle stsenaariumi korral, kalle teab, et vigade suureneb pärast esi lõpetatakse saavutamiseks 80% CPU kasutuse ja määrab autoscale reegli suurendamiseks eksemplari järgmiselt:

![Autoscale sätete ees pool.][Front-End-Scale]

|   **Autoscale profiil – ette lõpeb**              |
|   --------------------------------------------    |
|   **Nimi:** Autoscale – ette lõpeb                |
|   **Skaala järgi:** Plaanimine ja jõudluse reeglid    |
|   **Profiili:** Iga päev                           |
|   **Tüüp:** Korduvus                            |
|   **Sihtväärtust:** 3-10 eksemplari             |
|   **Päeva:** Iga päev                              |
|   **Alguskellaaeg:** 9:00 AM                         |
|   **Ajavöönd:** UTC-08                           |
|                                                   |
|   **Autoscale reegli (skaala üles)**                   |
|   **Ressurss:** Ees pool                    |
|   **Mõõdiku:** CPU %                               |
|   **Toiming:** Rohkem kui 60%                 |
|   **Kestus:** 20 minutit                        |
|   **Ajastamine koondamine:** Keskmine                   |
|   **Toiming:** Suurendada arvu 3                 |
|   **Lahedad allapoole (minutites):** 120                    |
|                                                   |
|   **Autoscale reegli (skaala alla)**                 |
|   **Ressurss:** Töötaja rakenduskausta 1                     |
|   **Mõõdiku:** CPU %                               |
|   **Toiming:** Vähem kui 30%                    |
|   **Kestus:** 20 minutit                        |
|   **Ajastamine koondamine:** Keskmine                   |
|   **Toiming:** 3 arv väheneb                 |
|   **Lahedad allapoole (minutites):** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
