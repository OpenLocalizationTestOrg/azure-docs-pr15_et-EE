<properties
    pageTitle="Analüüsimine kliendi piimapütt abil arvuti õ | Microsoft Azure'i"
    description="Juhtumianalüüsi arendada mudelit, mis on integreeritud analüüsida ja klientide piimapütt hinded"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Kliendi piimapütt analüüsimine Azure seadme õ abil

##<a name="overview"></a>Ülevaade
Selles teemas on esitatud viide rakendamine kliendi piimapütt analüüsi projekti, mis on loodud Azure seadme õ Studio abil. Arutatakse seotud üldise mudelite terviklikult probleemide tööstusliku kliendi piimapütt. Ka mõõta mudeleid, mis on loodud, kasutades arvuti õ täpsuse ja me hinnata kohta käivad juhised edasi.  

### <a name="acknowledgements"></a>Kinnitused

See katse on välja töötatud ja testida Serge Berger, põhisumma Data teadlane Microsofti ja Roger Barga varem toote halduri Microsoft Azure'i masina õ. Azure'i dokumendid meeskonna tänulikult tunnustab oma teadmisi ning tänu nende jagamiseks see lühiülevaade.

>[AZURE.NOTE] Andmed, mida kasutatakse selle katse pole avalikult kättesaadavaks. Näiteks koostamiseks masina õ mudeli piimapütt analüüsi jaoks, lugege teemat: [Telco piimapütt mudeli malli](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) [Cortana ärianalüüsi](http://gallery.cortanaintelligence.com/) galeriis


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Kliendi piimapütt probleem
Ettevõtted, turuosa ja kõik ettevõtte valdkondades tegelema piimapütt. Mõnikord piimapütt on liiga suur ja mõjutab poliitika otsuseid. Traditsiooniline lahendus on kõrge kalduvus churners prognoosida ja oma vajadustele kaudu portjeeteenus, turunduskampaaniate, või rakendades teisiti erandid. Et valdkonna ja isegi kindla tarbija kobar teise ühe valdkonna (nt side) saate olla erinevad järgmistest viisidest.

Levinud kujuneb, et ettevõtted ei pea neid teisiti klientide säilitamise või minimeerimine. Seega oleks loomulikus meetodid Keskmine piimapütt tõenäosuse iga klient ja ülemised N neist aadress. Tähtsamad kliendid võivad olla tulusaimad neist; näiteks keerukamaid stsenaariumid, kasum funktsioon töötab leviloendite puhul teisiti soodusluba valiku ajal. Need kaalutlused on siiski ainult osa tervikliku strateegia piimapütt tegelemiseks. Ettevõtted on ka arvestada konto risk (ja sellega seotud riske hälbe) sekkumise ja tõenäoline kliendi osadeks ja tase.  

##<a name="industry-outlook-and-approaches"></a>Valdkonna Outlooki ja võimalused
Keerukate piimapütt on küps valdkonna märk. Klassikaline näide on ning kus Tellijad võivad teadaolevalt sageli ühe pakkuja üle minna teisele. See vabatahtlik piimapütt on töökindlus on. Lisaks pakkujate kogunud olulisi teadmisi *piimapütt draiverid*, mis on tegurid ketas tarbijatele üle minna.

Näiteks mobiiltelefoni või seadme valik on tuntud piimapütt ettevõtte mobiiltelefon. Selle tulemusena populaarsed poliitika on toetada telefonitoru hind uue tellijad ja laadimise täielik hind olemasolevate klientide uuendada. Selle poliitika on varem viinud klientide hopping ühe pakkuja teise uue allahindlust, mis omakorda on küsitakse pakkujate piiritlemiseks oma strateegiad.

Mõnede telefonitoru pakkumisi on tegur, mis muudab väga kiiresti kehtetuks piimapütt mudelid, mis põhinevad praeguses telefonitoru mudelite. Lisaks mobiiltelefonid ei ole telekommunikatsiooni seadmed; need on ka mood laused (kaaluge iPhone) ja neid social aitavad prognoosida ei kuulu tavaline side andmekogumi.

Modelleerimiseks ongi tulemus on, et te ei saa töötada heli poliitika, lihtsalt kõrvaldamise teadaolevad piimapütt miks. Pidev modelleerimine strateegia, sh klassikaline mudeleid arvuliselt väljendatud kategooriline muutujate (nt otsus puude) on tegelikult **kohustuslik**.

Suurte andmekogumite kasutades oma klientidele, toimivad ettevõtted suur andmeanalüüsi (eriti piimapütt tuvastamise suur andmete põhjal) nimega tõhus moodus probleemi. Leiate suur andmete lähenemine piimapütt ETL jaotises soovitused probleemi lähemalt.  

##<a name="methodology-to-model-customer-churn"></a>Mudeli kliendi piimapütt meetod
Levinud probleemide lahendamine protsessi lahendada kliendi piimapütt on kujutatud arvud 1 – 3:  

1.  Risk mudeli võimaldab teil silmas pidada? kuidas toimingud mõjutavad tõenäosuse ja riski.
2.  Sekkumise mudelit, mis võimaldab teil silmas pidada? kuidas sekkumise tasandi võib mõjutada tõenäosuse piimapütt ja summa kliendi eluea väärtuse (CLV).
3.  Analüüsi ärisündmuste kvalitatiivse, mis on aktiivne turunduskampaania, mis on kujundatud esitamisel optimaalse pakkumine suunatud koosolekut laiendanud.  

![][1]

See tulevikku suunatud lähenemine on parim viis kasutatakse piimapütt, kuid see on kaasas keerukuse: välja mitme mudeli arhetüüp ja Jälita sõltuvused mudelite vahel. Suhtluse mudelite hulgas saab kokku, nagu on näidatud järgmisel joonisel:  

![][2]

*Joonis 4: Ühendatud mitme mudeli arhetüüp*  

Mudelid omavaheliste on oluline, kui me esitamisel tervikliku lähenemisviisi klientide hoidmine. Iga mudeli tingimata laguneb aja jooksul; Seetõttu on arhitektuur on peidetud tsükkel (sarnane arhetüüp määratud CRISP-DM andmete kaevandamine standard [***3***]).  

Üldine risk-otsust-turunduse osadeks/lagunemise on endiselt generalized struktuur, mis on palju äri probleeme suhtes. Piimapütt analüüsi on lihtsalt tugev esindaja selle rühma probleeme, kuna nendest kõik jooned ei võimalda lihtsustatud sõnastikupõhise lahenduse keerukate probleem ilmneb. Tänapäevane lähenemisviisi piimapütt külgedega ei tõsteta eriti lähenemisviisi, kuid sotsiaalseid aspekte on kokku modelleerimine arhetüüp, kui nad oleks mis tahes mudel.  

Huvitav Lisaks on suur andmeanalüüsi. Tänase telekommunikatsiooni ja jaemüügi jaoks koguda täielik andmeid oma klientidele ja saame hõlpsasti näha, et vajate mitme mudeli Ühenduvus muutub levinud trend, nt asjade Interneti ja asukohast sõltumatud seadmetes, mis võimaldavad business tööle nutikas lahendusi mitmest uute trendide antud.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Seadme õ Studios modelleerimine arhetüüp rakendamine
Antud kirjeldatud probleem, mis on parim viis on integreeritud modelleerimine ja hinded lähenemisviisi rakendamiseks? Selles jaotises näitab, kuidas me täidetakse see Azure seadme õ Studio abil.  

Mitme mudeli lähenemine on peab kujundamisel globaalne arhetüüp piimapütt jaoks. Isegi hinded (ennustav) osa lähenemisviisi peaks olema mitme mudeli.  

Järgmisel joonisel on esitatud prototüüp lõime, mis töötab neli hinded algoritmid masina õ Studios prognoosida piimapütt. Põhjus kasutamiseks mitme mudeli lähenemine pole ainult loomiseks ansambel klassifitseerijale, suurendamiseks täpsuse, ka üle abiseadme kaitsta ja parandada asemel funktsioonide valik.  

![][3]

*Joonis 5: Prototüüp on piimapütt modelleerimine lähenemine*  

Järgmistes jaotistes on lähemalt prototüüp hinded mudelit, mis oleme rakendanud masina õ Studio abil.  

###<a name="data-selection-and-preparation"></a>Andmete valik ja ettevalmistamine
Mudelid koostamiseks kasutatud andmed ja Keskmine kliendid saadi CRM vertikaalne lahendus kliendi privaatsuse kaitsmiseks obfuscated andmetega. Andmed sisaldavad teavet 8 000 tellimuste USA-s ja ühendab kolm allikad: ettevalmistamise andmete (tellimuse metaandmed), tegevuse andmed (kasutamise kohta) ja Kliendiandmete tugi. Andmed ei hõlma äri seotud teavet klientide; Näiteks ei sisalda lojaalsus metaandmete või krediitkaardiga hinded.  

Lihtsa, ETL ja puhastamise protsesside andmed on väljapoole Kuna me Oletagem, et andmete ettevalmistamise on juba tehtud mujal.   

Funktsioonide valik modelleerimiseks põhineb esialgse Ümardusalus hinded aitavad prognoosida, kaasatud protsessi, mis kasutab juhusliku mets mooduli kogum. Seadme õ Studios rakendamiseks me arvutatud keskmine, median ja vahemikud tüüpilised funktsioonide jaoks. Näiteks lisasime agregaadid kvaliteedi andmeid, nagu miinimum- ja väärtusi kasutaja tegevuste jaoks.    

Me ka jäädvustata ajaline teabe viimase kuus kuud. Saame analüüsida andmeid ühe aasta ja me kindlaks, et isegi kui statistiliselt trende, piimapütt mõju on oluliselt kahanenud järel.  

Kõige olulisemad seisneb selles, et arvuti õ Studio, kasutades Microsoft Azure'i andmeallikad on rakendatud kogu protsessi, sh ETL funktsioonide valik, ja modelleerimine.   

Järgmised skemaatilised diagrammid illustreerivad andmed, mida kasutati.  

![][4]

*Joonis 6: Väljavõte andmeallika (obfuscated)*  

![][5]


*Joonis 7: Funktsioonid ekstraktimist andmeallikas*
 
> Pange tähele, et andmed on privaatne ja seetõttu mudel ja andmeid ei saa ühiskasutusse anda.
> Siiski sarnase mudeli avalikult kättesaadavaks andmete kasutamise, leiate selle valimi katsetamiseks [Cortana ärianalüüsi Galerii](http://gallery.cortanaintelligence.com/): [Telco kliendi piimapütt](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Lisateavet selle kohta, kuidas rakendada piimapütt analüüsi mudeli kasutamine Cortana ärianalüüsi komplekti, soovitame [selle video](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) programmi juht Wee Hyong Tok. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Prototüüp algoritmid

Kasutasime nelja seadme õ algoritmide koostamiseks prototüüp (mitte kohandamine):  

1.  Logistilise regressiooni (LR)
2.  Võimendatud otsustuspuu (BT)
3.  Keskmine perceptron (AP)
4.  Tugiteenuste vektorkuju masina (SVM)  


Järgmine diagramm näitab kujunduspinna katse, mis näitab, kus on loodud mudelid järjekord osa.  

![][6]  


*Joonis 8: Loomine mudelite masina õ Studio*  

###<a name="scoring-methods"></a>Hinded meetodid
Me sai neli mudelit siltidega koolitus andmekomplekti abil.  

Me esitada ka võrdlev mudel loodud abil SAS Enterprise kaevandaja 12 töölaua väljaannet andmekomplekti hinded. Me mõõta SAS mudeli ja kõik neli masina õ Studio mudelid täpsuse.  

##<a name="results"></a>Tulemused
Selles jaotises tutvustame meie tulemused täpsuse mudelid, mis põhineb hinded andmekomplekti kohta.  

###<a name="accuracy-and-precision-of-scoring"></a>Täpsusega hinded
Üldiselt Azure seadme õppe rakendamist on taha SAS täpsuse 10 – 15% (ala jaotises kõvera või AUC).  

Kõige olulisemad meetermõõdustik rakenduses piimapütt aga väärklassifikatsiooni määr: St, ülemised N churners nimega klassifitseerijale abil, millised neist tegelikult ei **piimapütt,** ja veel said teisiti? Järgmine diagramm võrreldakse seda klassifikatsioonivead määra kõigi mudelite:  

![][7]


*Joonis 9: Passau prototüüp ala kõvera*

###<a name="using-auc-to-compare-results"></a>Võrrelge tulemusi AUC abil
Ala jaotises kõvera (AUC) on mõõdiku tähistava globaalne mõõtmine *eraldatavus* hinded positiivsete ja negatiivsete populatsiooni jaotamine vahel. See on sarnane traditsiooniline graafik vastuvõtja tehtemärk omadus (ROC), kuid üks oluline erinevus on AUC meetermõõdustik ei nõua teil valida läve. Selle asemel teeb kokkuvõtte tulemuste **kõigi** võimalike valikute üle. Seevastu traditsiooniline ROC graafik esitab positiivne määr vertikaalteljel ja tulemuste määra horisontaalset telge ja liigitamine lävi muutub.   

AUC tavaliselt kasutatakse mõõtmine väärt erinevate algoritmide kohta (või muu süsteemide) Kuna see lubab mudelite abil oma AUC väärtused võrrelda. See on populaarne lähenemine on nagu Meteoroloogia ja biosciences. Seega AUC tähistab populaarse tööriista klassifitseerijale tulemuslikkuse hindamise.  

###<a name="comparing-misclassification-rates"></a>Võrdlemise klassifikatsioonivead
Me võrreldes väärklassifikatsiooni määr kõnealuse andmekomplekti kohta, kasutades CRM andmed ligikaudu 8 000 tellimused.  

-   SAS väärklassifikatsiooni määr on 10 – 15%.
-   Seadme õ Studio klassifikatsioonivead oli top 200-300 churners 15-20%.  

Side valdkonna, on oluline käsitleda ainult klientidele, kes on suurim risk piimapütt pakkudes portjeeteenust või muu teisiti töötlemine. Sellega masina õ Studio rakendamist saavutab tulemusi võrdselt SAS mudel.  

Samamoodi, täpsus on olulisem arvutustäpsuse, kuna me enamasti huvitatud õigesti liigitamine võimalikud churners.  

Wikipedia järgmisel joonisel on kujutatud seose pildi elav, lihtne:  

![][8]

*Joonis 10: Miinuseks täpsusega vahel*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Täpsusega tulemuste võimendatud otsust puu mudel  

Järgmises tabelis kuvatakse töötlemata hinded võimendatud otsust puu mudelit, mis juhtub olema kõige täpsem vahel neli mudelit masina õ prototüüp kasutamise tulemusi.  

![][9]

*Joonis 11: Võimendada otsust puu mudeli omadused*

##<a name="performance-comparison"></a>Jõudluse võrdlus
Me ei võrreldes kiirust, kus loodud andmete masina õ Studio mudelite ja töölaua väljaannet SAS Enterprise kaevandaja 12.1 abil loodud sarnastes mudeli abil.  

Järgmises tabelis on toodud algoritmide jõudlus:  

*Tabel 1. Üldise jõudluse algoritmide (Täpsemad)*

| LR|BT|AP|SVM|
|---|---|---|---|
|Keskmine mudel|Parim mudel|Vähem|Keskmine mudel|

Mudelid majutatud arvuti õ Studios suurel määral nominaalväärtus on laenuportfelli SAS kiirust täitmise, kuid täpsuse 15-25%-ga.  

##<a name="discussion-and-recommendations"></a>Arutelu ja soovitused
Side valdkonna, mitu tavad on tekkinud analüüsimiseks piimapütt, sh:  

-   Tuletada mõõdikute nelja olulise kategooria:
    -   **Üksuse (nt tellimust)**. Sätte põhiteavet tellimus ja/või kliendi, mis on piimapütt teema.
    -   **Tegevuste**. Hankige kõigi võimalike kasutusteavet, mis on seotud üksus, näiteks sisselogimise arvu.
    -   **Klienditugi**. Kogutakse teavet kliendi tugi logid näitamaks, kas tellimuse olid probleemid või koos klienditoe poole.
    -   **Konkurentsianalüüsi ja äriandmete**. Hankida teavet võimalike kliendi kohta (nt võib olla saadaval või on raske jälgida).
-   Kasutage ketas funktsioonide valik tähtsus. See tähendab, et võimendatud otsust puu mudel on alati.  

Need neli kategooriate kasutamine loob illusiooni, et kliendid risk piimapütt tuvastamiseks piisab lihtsa *tarkadeks* moodust, registrid loodud mõistlikum tegurite kohta kategooria alusel. Kahjuks, kuigi see mõiste tundub tõenäoline, see on vale mõistmine. Põhjus on piimapütt on ajalise mõju ja piimapütt teguritest on tavaliselt siirdamiseks Ühendriikides. Mis viib klient, jättes täna silmas pidada võivad olla erinevad homme, ning see kindlasti teistsugused kuus kuud nüüd. Seetõttu on *tõenäosusel* mudeli on vajalik.  

See oluline jälgimine sageli tähelepanuta üldiselt eelistab business intelligence rakendusse lähenemine analytics Business, enamasti, sest see on lihtsam müüa ja võimaldab lihtne automatiseerimine.  

Siiski on Iseteeninduslik analytics masina Õppekeskuse Studio abil lubadus, muutuvad nelja liiki teavet, osakonna või osakonna, sorteeritud kohapeal tutvudes piimapütt väärtuslik allikas.  

Mõne muu köitva võimalus tulevad Azure seadme õ on võimalus lisada kohandatud moodul hoidla eelmääratletud moodulid, mis on juba olemas. See funktsioon loob sisuliselt võimalus valige teekide ja vertikaalne turgude jaoks mallide loomine. See on oluline diferentsiaator Azure seadme õppe, turu kohas.  

Loodame jätkata selle teema tulevikus, eriti seotud suur andmeanalüüsi.
  
##<a name="conclusion"></a>Kokkuvõte
Selles dokumendis kirjeldatakse mõistlikum probleemiga levinud kliendi piimapütt üldise raamistiku abil. Me lugeda prototüüp mudelite saades ja rakendada selle Azure seadme õ abil. Lõpuks me hinnata täpsuse ja jõudluse prototüüp lahenduse osas võrreldavad algoritmide kohta rakenduses SAS.  

**Lisateave:**  

Kas see raamat teid aidata? Palun andke tagasisidet. Meile skaalal (halb) 1-5 (suurepärane), kuidas te hindate seda paberi ja miks teil andnud See reiting? Näiteks:  

-   On teil selle hindamine kõrge tõttu näidetele, suurepäraseid ekraanipilt, tühjendage ruut kirjutamine, või muul põhjusel?
-   On teil selle hindamine kehva näited, udune kuvatõmmiseid või segane kirjutamist madal?  

See tagasiside aitab parandada anname lühiülevaated.   

[Tagasiside saatmine](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Viited
Ennustav [1] : juuni 2011, lk 18 – 20 Lisaks the prognoose, W. McKnight, teabe haldamine.  

[2] Wikipedia artiklit: [täpsusega](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [CRISP-DM 1.0: üksikasjalike andmete kaevandamine juhend](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [big Data turundus: osaleda klientide tõhusamalt ja juhtida väärtuse](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco piimapütt mudeli malli](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) [Cortana ärianalüüsi](http://gallery.cortanaintelligence.com/) galeriis 
 
##<a name="appendix"></a>Lisa

![][10]

*Joonis 12: Hetktõmmist piimapütt prototüüp esitluse*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
