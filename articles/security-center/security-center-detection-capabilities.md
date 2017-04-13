<properties
   pageTitle="Azure'i turbekeskus tuvastamise funktsioonidele | Microsoft Azure'i"
   description="Selle dokumendi aitab teil mõista, kuidas töötavad Azure turbekeskus tuvastamise võimalusi."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Azure'i turbekeskus tuvastamise võimalused
Selle dokumendi käsitletakse Azure'i turbekeskus täpsemalt tuvastamise võimalusi, mis aitab tuvastada suunamise oma ressursse Microsoft Azure active ohtude ja annab teile ülevaateid, mis on kiiresti vaja.

> [AZURE.NOTE] Täiustatud avastused on saadaval Azure Security Center Standard taseme. 90-päevane tasuta prooviversioon on saadaval. Saate üle minna [Turbepoliitika](security-center-policies.md)hinnad taseme valikut. Külastage [turbekeskus lehe](https://azure.microsoft.com/pricing/details/security-center/) kohta rohkem teavet saada. 


## <a name="responding-to-todays-threats"></a>Tänase ohtude reageerimine
On muudetud märgatavat ohtu maastik viimase 20 aasta jooksul. Varem ettevõtetes tavaliselt oli ainult muretsema veebisaidi moonutamist üksikute turvaohte, kes on peamiselt huvitatud näha "mida nad võivad teha". Tänase ründajad on palju keerukamaid ja korraldada. Need on tihti finants- ja strategic eesmärke. Lisaks on veel võimalusi, nagu nad võivad tuleb katta riikide või tuluallikatega.

See lähenemine on põhjustanud ennenägematute professionaalsed ründaja ridadesse. Enam on need huvitatud veebi moonutamist. Nüüd on huvitatud varastamine teabe, kontode ja isikuandmed – mis kõik saate luua raha Ava turu või võimendada kindla business, poliitika või väljaande military asukoht. Rohkem kui osas nende ründajad rahandus eesmärk on ründajad, kes rikub võrkude kahjustada taristu ja inimesed.

Vastuseks ettevõtted sageli juurutada erinevate punkti lahendusi, mis keskenduda kaitsta otsides teadaolevad rünnak allkirjade enterprise perimeetri või lõpp-punktid. Väike kvaliteedikadu teatised, mis nõuavad turvalisus analüütik kaudu alusel järjestada ja uurida, suure hulga loomiseks tavaliselt järgmisi lahendusi. Enamikule asutustest puudub aeg ja teadmiste vaja vastata need teatised – nii palju Mine adressaadita.  Samal ajal ründajad on muutunud nende meetodite subvert palju allkirja põhineb kaitset ja [kohandada cloud keskkonnas](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Uue lähenemisel on vaja kiiremini uute ohtude tuvastamine ja kiirendada tuvastamise ja vastus. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Kuidas Azure'i turbekeskus tuvastab ja ohtude vastab

Microsofti turvalisus teadlased on pidevalt ohtude vaade. Neil on juurdepääs suur kogum, telemeetria saadud Microsofti esindused kohapealse ja pilveteenuse. See suurt ja mitmesuguse kogumine andmekomplektide võimaldab Microsoftil leida uus rünnak mustrite ja trendide kohapealse tarbija ja ettevõtte tooted kui ka selle veebiteenuste. Selle tulemusena turbekeskus kiiresti värskendada oma tuvastamise algoritmid nagu ründajad vabastage uusi ja keerukamaid kasutab. Seda moodust abil saate kiiresti jooksva ohtu keskkonnale sammu. 

Turbekeskus ohtude tuvastamise toimib automaatselt turvalisus teabe kogumine oma Azure ressursse, võrgu ja ühendatud partnerilahenduste kaudu. See analüüsib seda teavet, sageli tõestusmeetodid teavet ohtude tuvastamiseks mitmest allikast. Turbeteatiste on prioriteetsed Security Center koos soovitustega selle kohta, kuidas ohtu migreerimisel ning probleemide lahendamisel.

![Turbekeskus andmete kogumine ja esitlus](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Turbekeskus töötab täpsemate turbesätete Kasutusanalüüsi, mis ületavad palju allkirja lähenemise. Läbimurdeid big data ja [seadme Õppekeskuse](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) tehnoloogiad on kasutada sündmuste üle kogu cloud struktuuri – tuvastamiseks ohtude, mida oleks võimalik tuvastada, kasutades käsitsi lähenemine koostada prognoose uuendus ning hinnata. Nende turvalisus analytics on järgmised. 

- **Integreeritud ohtude ärianalüüsi**: otsitakse teada halb osalejate tehtavate üldist ohtu ärianalüüsi Microsofti toodete ja teenuste, Microsoft digitaalse kuritegude üksus (DCU), Microsoft turvalisus vastuse Center (MSRC) ja välise kanalite kaudu.
- **Käitumist analytics**: kehtib teadaolevate mustrite avastada pahatahtlik käitumine. 
- **Normaalne tuvastamise**: kasutab statistika profiilide koostamiseks ajalooliste võrdlusalus. See hoiatab kõrvalekaldeid loodud võrdlusandmeid, mis vastavad võimalikud rünnak vektori kohta.


### <a name="threat-intelligence"></a>Ohtu ärianalüüsi
Microsoft on suur üldist ohtu ärianalüüsi. Telemeetria juhtimine mitmest allikast, näiteks Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AXI, outlook.com, MSN.com, Microsofti digitaalse kuritegude üksus (DCU) ja Microsoft turvalisus vastuse Center (MSRC). Teadlased saavad ohtu teabe põhi pilve jagada ja muude tootjate ohtu ärianalüüsi kanalite tellib. Azure'i turbekeskus saate kasutada seda teavet märku teadaolevad halb osalejate ohtudega. Mõned näited:

- **Väljamineva side pahatahtlik IP-aadress**: väljaminev liiklus teadaolevad bot või darknet tõenäoliselt näitab, et teie ressursi on rikutud ja selle katse käivitada käske ründaja süsteemi või exfiltrate andmeid. Azure'i turbekeskus võrdleb võrguliikluse Microsofti üldist ohtu andmebaas ja teid, kui tuvastab, side pahatahtlik IP-aadress.

## <a name="behavioral-analytics"></a>Käitumist analytics

Käitumist analytics on meetod, mis analüüsib ja võrreldakse teadaolevate kogumi andmed. Neid mustreid ei ole lihtsa allkirjade. Need määratakse Õppekeskuse algoritmide kohta, mida rakendatakse suuri andmekomplektide keerukate seadme kaudu. Need on ka määratud ettevaatlik analüüsi pahatahtlik klahvikäitumise expert ärianalüütikud. Azure'i turbekeskus saate käitumist analytics abil saate tuvastada rikutud ressursse, mis põhinevad virtuaalse masina logid, virtuaalse võrgu seadme logid, struktuuri logid, krahh puistab ja muudest allikatest. 

Lisaks on vastavus muude signaale täiendavad dokumendid on levinud turunduskampaania otsida. Selle korrelatsioonikordaja aitab tuvastada sündmused, mis on loodud näidikud kompromiss kooskõlas. Mõned näited:

- **Kahtlane töötlemine täitmise**: ründajad kasutavad mitmesuguseid võtteid ründetarkvara ilma käivitada. Ründaja võib näiteks anda ründevara sama nimega korralikud süsteemifailid, kuid paigutada need failid alternatiivse asukoha, kasutage nime, mis on väga sarnane hea faili või mask true failinimelaiend. Turbekeskus mudelid protsesside käitumist ja kuvarite protsessi täitmised võõrväärtuste, nagu need tuvastamiseks.  
- **Peidetud ründevara ja kasutamise avaldab**: keerukaid ründevara on võimalik vältida traditsiooniline ründevaratõrje toodete kunagi kettale kirjutamine või krüptimise tarkvara komponendid kettale salvestatud.  Siiski sellise ründevara saab avastada kasutades mälu analüüsi, peab ründevara selleks, et funktsioon mälu lahkuvad jälgi. Kui tarkvara jookseb, sisaldab krahh dump mälu osa krahhi ajal.  Analüüsib mälu dump krahh, Azure'i turbekeskus on tuvastanud võtteid kasutada nõrkade kohtade tarkvara, kasutada konfidentsiaalseid andmeid ja salaja säilivad koos – rikutud seadme ilma oma seadme jõudlust mõjutavad.
- **Liikumine ja sisemise uurimistööd loova**: püsima rikutud võrk ja otsige üles harvest väärtuslik andmeid, ründajad sageli katse teisaldada külg rikutud arvutist teistele sama võrgustikus. Turbekeskus jälgib protsess ja Logi sisse, et leida katsete ründaja tugipunkt remote käsu täitmine võrgu katsetamine, nt ja konto loendi laiendamiseks.
- **Pahatahtlik PowerShelli skripti**: PowerShelli kasutab ründajad pahatahtlik kood target virtuaalmasinates jaoks mitmel eesmärgil käivitada. Turbekeskus uurib PowerShelli tegevuse kahtlaste tegevuste suhtes. 
- **Väljaminevat eest**: ründajad sageli suunata cloud ressursid eesmärgiga ühendada täiendavad eest nende ressursside abil. Rikutud virtuaalmasinates, näiteks võib kasutada jõuvõtete vastu muude virtuaalmasinates suunatud käivitada, saata rämpsposti või avatud pordid ja muudes seadmetes, mis Internetis skannimine. Rakendades masina õ võrguliiklust, saate turbekeskus tuvastada, millal väljaminev võrgu side ületada norm. RÄMPSPOSTI, puhul turbekeskus korreleerub ka ebatavalised e-posti liiklus ärianalüüsi Office 365 kas meili on tõenäoliselt õelate või õiged e-posti turunduskampaania tulemus.  

### <a name="anomaly-detection"></a>Normaalne automaattuvastus

Azure'i turbekeskus kasutab normaalne tuvastamise ka ohtude tuvastamiseks. Käitumist analytics (mis sõltub teadaolevate mustrite tuletatud suurte andmekogumite) Seevastu normaalne tuvastamise on veel "isikupärastatud" ja keskendub võrdlusandmeid, mis on seotud teie juurutuste. Seadme õ rakendatakse tavaline tegevuste jaoks saate määratleda ja seejärel määratleda võõrväärtuste tingimuste, turvalisus sündmuse tähistavate luuakse reeglid. Siin on näide:

- **Sissetulev RDP/SSH brute jõustada eest**: saate olla hõivatud virtuaalmasinates sisselogimise palju iga päev ja virtuaalmasinates, mis on väga vähe või mis tahes sisselogimise. Azure'i turbekeskus saate määratleda võrdlusalus login tegevuste jaoks neid virtuaalmasinates ja kasutada seadme Õppekeskuse määratleda, mis on väljaspool tavaline sisse logida tegevus. Kui arvu sisselogimise või selle sisselogimise või asukohta, kust on nõutav on sisselogimise kellaaeg või muid sisselogimise seotud omadusi on oluliselt erinev alusjoone, siis teatis võib loodud. Klõpsake uuesti masina õ määratleb, mis on oluline.

## <a name="continuous-threat-intelligence-monitoring"></a>Pidev ohtu ärianalüüsi jälgimine

Azure'i turbekeskus toimib turvalisus uurimistöö ja andmete teadus meeskonnad, mida pidevalt ohtu horisontaalpaigutus muudatuste jälgimine. See hõlmab järgmisi algatused:

- **Ohtu ärianalüüsi jälgimine**: ohtu ärianalüüsi sisaldab menetlustele, näidikud, mõju ja vaidlustatavate nõuandeid olemasoleva või uute ohtude kohta. Seda teavet jagatakse ühenduse turvalisus ja Microsoft jälgib pidevalt ohtu ärianalüüsi kanalite sise- ja allikatest.
- **Signaal ühiskasutus**: turvalisus meeskondadel üle Microsoft's lai portfellivalikuotsuse asutusesisese ja pilvepõhise teenustel, serverite ja kliendi lõpp-punkti seadmete teadmisi jagada ja analüüsida. 
- **Microsofti turvalisuse spetsialistid**: poolelioleva suhete üle Microsoft meeskonnad, mida web rünnak tuvastamise ja eriotstarbelisi turvalisus väljadel nagu kohtumeditsiini töötamine.
- **Tuvastamise häälestamine**: algoritmide kohta on vastuolus tegelik klient andmekogumi ja turvalisus teadlased töötada klientide tulemuste kinnitamiseks. True ja false positiivsed kasutatakse piiritlemiseks masina õ algoritmide kohta.

Nende kombineeritud püüete kulmineerub uued ja täiustatud avastused, mida saate kasutada kohe – teil võtta toimingust.

## <a name="see-also"></a>Vt ka
Selles dokumendis te teada, kuidas Azure'i turbekeskus tuvastamise funktsioonid töötavad. Lisateavet turbekeskus, leiate järgmistest:

- [Azure'i turbekeskus plaanimine ja toimingute juhend](security-center-planning-and-operations-guide.md)
- [Haldamine ja turbeteatised Azure'i turbekeskus reageerimine](security-center-managing-and-responding-alerts.md)
- [Turbeteatiste Azure'i turbekeskus tüübi järgi](security-center-alerts-type.md)
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Partnerite lahenduste ja Azure turbekeskus jälgimine](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i väärtpaberi ajaveeb](http://blogs.msdn.com/b/azuresecurity/) – Leia ajaveebi Azure Turve ja nõuetele vastavuse kohta.
