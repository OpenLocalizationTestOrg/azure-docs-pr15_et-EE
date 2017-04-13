<properties
   pageTitle="Autoscaling juhised | Microsoft Azure'i"
   description="Juhised autoscale eraldada dünaamiliselt vajab ressursse."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="autoscaling-guidance"></a>Autoscaling juhised

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Ülevaade
Autoscaling on jagatakse dünaamiliselt vastavad jõudlus ja teenusetaseme lepinguid (SLAs) vastavad vajab ajal minimeerimine käitusaja kulud. Töö kasvab, rakendus võib olla vaja uusi ressursse, mis õigeaegne ülesannete täitmiseks. Nagu nõudmisel köis lõdveneb, võib olla tühistage eraldatud kulude minimeerimiseks endiselt piisav jõudluse ja koosoleku SLAs ressursid.
Autoscaling ära pilve majutatud keskkonnas elastsust kuigi halduse pea kohal. Ühendav vähendamine tehtemärgi pidevalt süsteemi jõudluse jälgimist ja otsuste lisamise või eemaldamise ressursside kohta.
>[AZURE.NOTE] Autoscaling kehtib kõigile ressursse, mis kasutavad rakenduse, mitte ainult Arvuta ressursid. Näiteks kui teie süsteemi kasutab teatejärjekordi teabe saatmiseks ja vastuvõtmiseks, see võib luua täiendavaid järjekorrad, nagu see skaala.

## <a name="types-of-scaling"></a>Skaleerimist tüübid
Tavaliselt skaleerimist suunab ühte kahest järgmisest:

- **Vertikaalne** (sageli nimetatakse _skaleerimist üles ja alla_). Selle vormi nõuab, et muuta riistvara (laiendamine või vähendada ja selle tulemuslikkuse) või ümberkorraldamine abil alternatiivse riistvara, mis on asjakohane võimalused ja jõudluse lahendus. Pilveteenuse keskkonnas, riistvara platform on tavaliselt virtualiseeritud keskkonnas. Kui algse riistvara on oluliselt overprovisioned, mis järgnevad esmast kapitali arvel, vertikaalselt ülespoole selles keskkonnas hõlmab ettevalmistamise võimsam ressursid ja seejärel teisaldada süsteemi peale uue ressursid. Vertikaalne skaleerimist on sageli häiriva protsess, mille jaoks on vaja teha süsteemi ajutiselt saadaval on kogurahastus. See võib olla võimalik hoida algse süsteemi käitamise ajal uue riistvara on ette valmistatud ja toonud võrgus, kuid tõenäoliselt mõne katkemise ajal uue vana keskkonnast üleminekud töötlemine. On aeg-ajalt autoscaling vertikaalne skaleerimise strateegia abil.
- **Horisontaalne** (sageli nimetatakse _ja skaleerimist_). Selle vormi jaoks on vaja täiendavaid lahendusest või vähem ressursse, mis on tavaliselt kaup ressursid, mitte võimsusega süsteemide juurutamine. Lahenduse saate jätkata katkestusteta töötab samal ajal, kui need ressursid on ette valmistatud. Kui ebausaldusväärsete on lõpule jõudnud, saate koopiate elemente, mis moodustavad lahendus juurutatud need täiendavad ressursid ja kättesaadavaks teha. Kui nõudmisel langeb, saate täiendavaid materjale taastatud pärast abil need elemendid on sulgeda puhtalt. Paljude pilvepõhist süsteemide, sealhulgas Microsoft Azure'i, toetavad ning selle vormi automatiseerimine.

## <a name="implement-an-autoscaling-strategy"></a>Rakendada autoscaling strateegia
Tavaliselt on autoscaling strateegia rakendamine hõlmab järgmisi komponente ja protsesside:

- Näidikuid ja jälgimise süsteemid rakenduse-, teenuse- ja taristu tasanditel. Need süsteemid jäädvustada olulisemad mõõdikud, nt vastuse korda, järjekorra pikkusega, Protsessori kasutuse ja mälukasutust.
- Otsuste loogika, mida saab hinnata jälgida skaleerimise tegureid eelmääratletud süsteemi lävede või ajakavasid ja otsuste kohta, kas mastaapimiseks või mitte.
- Mis on seostatud skaleerimist süsteemi, nt ettevalmistamise või tühistage ettevalmistamise ressursid ülesannete komponendid.
- Katsetada, jälgimine ja häälestamine autoscaling strateegia tagamaks, et see toimib ootuspäraselt.

Enamik pilvepõhises keskkonnas, nt Azure, sisestage sisseehitatud autoscaling menetlustele selle aadressi levinud stsenaariumi. Kui keskkonnas või teenust kasutada ei paku vajalikud automatiseeritud skaleerimise funktsioone, või kui teil on äärmiselt autoscaling nõuded Lisaks selle võimalusi, kohandatud rakendamiseks võib olla vajalik. Funktsionaalseid ja süsteemi mõõdikute koguda, kindlaks teha vajalikke andmeid analüüsida ja seejärel ulatuse ressursse vastavalt selle kohandatud rakendamise abil.


## <a name="configure-autoscaling-for-an-azure-solution"></a>Azure'i lahenduse autoscaling konfigureerimine
On mitu võimalust konfigureerida autoscaling oma Azure lahendusi.

- **Azure'i Autoscale** toetab kõige levinum skaleerimise stsenaariumid põhjal ajakava ja soovi korral võite vallandanud skaleerimist toiminguid (nt protsessori kasutamine, järjekorra pikkus või valmis- ja kohandatud hinnale) käitusaja mõõdikute põhjal. Saate konfigureerida lihtsa autoscaling poliitikate lahenduse kiiresti ja kerge vaevaga Azure portaali kaudu. Täpsemat kontrolli, saate teha [Azure'i teenuse haldamine REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) või [Azure ressursihaldur REST API](https://msdn.microsoft.com//library/azure/dn790568.aspx)kasutamine. [Azure'i jälgimine teenuse juhtimine teek](http://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring) ja [Microsofti muude võimalustega Raamatukogu](https://www.nuget.org/packages/Microsoft.Azure.Insights/) (eelvaade) on SDK-d, mis võimaldavad koguda mõõdikute erinevaid ressursse ja teha autoscaling kasutades REST API-d. Azure'i ressursihaldur tugi pole saadaval või kui kasutate Azure pilveteenustega, ressursside saab autoscaling jaoks teenuse haldus REST API-ga. Kõigil muudel juhtudel kasutada Azure ressursihaldur.
- **Kohandatud lahenduse**vastavalt teie instrumentation taotluse ja haldusfunktsioonid Azure, võib olla kasulik. Näiteks võite Azure diagnostika või teiste meetodite nanoskaalas oma rakenduse koos kohandatud koodi pidevalt jälgida ja eksportimine mõõdikute rakenduse. Kohandatud reeglid, mis töötamiseks nende mõõdikute ja kasutada teenuse haldust võib teil või autoscaling käivitamiseks kasutaja ressursihaldur REST API-ga. Mõõdikute käivitamise skaleerimise toimingu jaoks võib olla mis tahes sisseehitatud või kohandatud kokkuvõtete või muu instrumentation rakendate rakenduses. Siiski kohandatud lahendust pole kerge rakendada, ja tuleks kaaluda ainult kui ükski eelmise lähenemisel täita teie vajadustele. [Autoscaling rakenduse blokeerimine](http://msdn.microsoft.com/library/hh680892%28v=pandp.50%29.aspx) kasutab seda moodust.
- **Muu tootja teenused**, näiteks [Paraleap AzureWatch](http://www.paraleap.com/AzureWatch), võimaldavad teil mastaapimiseks lahenduse ajakavade, teenuse laadimine ja süsteemi mõõdikud, kohandatud reeglid ja kombinatsioonid eri tüüpi reegleid.

Valides milline autoscaling lahendus vastu võtta, võtke arvesse järgmist.

- Sisseehitatud autoscaling funktsioone, et kasutada, kui teie nõuetele. Kui ei, hoolega, kas vaja keerukamaid skaleerimise funktsioone. Mõned näited lisanõuded võivad sisaldada rohkem granulaarsus kontrolli, avastada päästik sündmusi mastaapimist, skaleerimist tellimustes ja muud tüüpi ressursid skaleerimist erineval viisil.
- Kaaluge võimalust, kui saate rakenduse Laadi prognoosida, piisava täpsusega sõltub ainult ajastatud autoscaling (lisamine ja eemaldamine eksemplarid täita eeldatava peaks vajadusel sisse). Kui see ei ole võimalik, kasutage reaktiivne autoscaling kogutud käitusajal lubada ettearvamatuid muudatuste nõudmisel käsitlema mõõdikute põhjal. Tavaliselt saab kombineerida järgmistest viisidest. Näiteks saate luua strateegia, mis lisab Arvuta, salvestusruumi ja järjekordade korda, kui teate, et rakendus on kõige hõivatud ajakava põhjal. See aitab tagada, et võimsus on saadaval, kui see on nõutav kohe, kui uus eksemplaride käivitamise. Lisaks iga ajastatud reegli määratleda mõõdikute, mis võimaldavad reaktiivne autoscaling tagamaks, et taotluse saate nõudmisel on pidev, kuid ettearvamatuid peaks hakkama selle aja jooksul.
- Sageli on raske aru saada mõõdikute ja võimsus nõuetele, eriti siis, kui rakendus on esialgu juurutatud seos. Eelistate ette vähe eest võimsus alguses, ja seejärel jälgida ja häälestada autoscaling reegleid tegeliku laadi võimsus lähemale viia.

### <a name="use-azure-autoscale"></a>Azure'i Autoscale kasutamine
Autoscale võimaldab teil konfigureerida skaala läbi ja mastaapimiseks lahenduse suvandid. Autoscale automaatselt lisamiseks või eemaldamiseks saate eksemplarid Azure'i Cloud Services veebiteenuse ja töötaja rollid, Azure Mobile teenused ja veebirakenduste funktsiooni teenuses Azure rakendus. Saate ka lubada automaatse skaleerimist teguriga käivitamine ja peatamine eksemplarid Azure'i Virtuaalmasinates. Azure'i autoscaling strateegia sisaldab kahte tüüpi tegureid.

- Saate tagada täiendavad eksemplarid ajakava vastavalt autoscaling on saadaval, mis langeb kokku on oodatud tippväärtus kasutus ja saate mastaapimiseks klõpsake pärast tippväärtus aeg on möödas. See võimaldab teil tagada, et teil on piisavalt eksemplaris juba töötab, ootamata reageerida laadi süsteem.
- Mõõdikute põhjal autoscaling, mis reageerib tegurid, näiteks keskmise CPU kasutamine üle viimase tunni või mahajäämus sõnumid, mis lahendus on töötlemise Azure salvestusruumi või Azure'i teenus siini järjekorda. See võimaldab rakenduse reageerida eraldi ajastatud autoscaling reeglite mahutamiseks nõudmisel planeerimata või ettenägematute muudatustest.

Kui kasutate Autoscale, võtke arvesse järgmist:

- Autoscaling strateegia kavandamine ühendab ajastatud ja mõõdikute põhjal skaleerimist. Saate määrata nii tüüpi teenuse reegleid.
- Peaksite konfigureerimine autoscaling reeglid ja seejärel jälgida aja jooksul rakenduse jõudlus. Tulemuste jälgimine selle abil saate kohandada viisi, kuidas süsteemi skaala, kus vajaduse korral. Siiski pidage meeles selle autoscaling ei ole mõne hetke protsess. Võtab aega reageerida mõõdiku nagu average CPU kasutamise üle (või madalam) määratud piiri.
- Autoscaling reeglid, mis kasutavad tuvastamise süsteem, mis põhineb mõõdetud päästik atribuut (nt CPU kasutus- või järjekorra pikkus) kasutage liidetud väärtuse üle aja, mitte hetke väärtused, autoscaling toimingu käivitada. Vaikimisi on liitmise väärtuste keskmine. See takistab reageeri liiga kiiresti või põhjustada kiire võnkumist süsteem. See lubab ka uued eksemplarid, on automaatne-alustatud lahendada arvesse töötab režiimis, vältimine autoscaling täiendavad toimingud tekkimist ajal uued eksemplarid on käivitamine aega. Azure'i pilveteenustega ja Azure'i Virtuaalmasinates, on vaikimisi perioodi koondada 45 minutit, seega võib kuluda kuni selle ajavahemikul jaoks meetermõõdustik autoscaling vastuseks diagrammi sisse nõudmisel käivitada. Saate muuta koondamine perioodi SDK abil, kuid võtke arvesse, et vähem kui 25 minuti võib põhjustada ettearvamatuid tulemusi (Lisateabe saamiseks vt [Automaatne skaleerimist pilveteenustega CPU protsendimäärad teegiga Azure'i jälgimise teenuste haldus](http://rickrainey.com/2013/12/15/auto-scaling-cloud-services-on-cpu-percentage-with-the-windows-azure-monitoring-services-management-library/)). Veebirakenduste, aja on palju lühem lubamisel uued eksemplarid saadaval umbes viis minutit pärast muudatuse Keskmine päästik mõõt.
- Kui konfigureerite autoscaling SDK, mitte veebiportaali, saate määrata üksikasjalikumat ajakava, mille jooksul on aktiivne. Samuti saate luua oma mõõdikute ja neid kasutada koos või ilma olemasolevate reeglite autoscaling. Näiteks võite, kui soovite kasutada kohandatud väidab, et mõõta kindlad äriprotsessid või alternatiivne hinnale, taotlusi sekundis või average mälu-saadavus, nt.
- Kui autoscaling Azure'i Virtuaalmasinates, peate juurutama virtuaalse masina, mis on võrdne maksimum eksemplaride arv lubate autoscaling alustada. Juhul peab olema sama valiku kättesaadavus osa. Virtuaalmasinates autoscaling süsteemi loomine või kustutamine eksemplarid virtuaalse masina; selle asemel saate konfigureerida autoscaling reegleid ei käivitamine ja peatamine sobiv järgmiste eksemplaride arv. Lisateavet leiate teemast [automaatselt mastaapida rakendus töötab Web rollid, töötaja rollid, või Virtuaalmasinates](./cloud-services/cloud-services-how-to-scale.md).
- Kui uued eksemplarid ei saa käivitada, võib-olla Kuna maksimum tellimus teile helistada või tõrge ilmneb käivitamisel portaali võib näitavad, et toimingu autoscaling õnnestus. Siiski kuvatakse **ChangeDeploymentConfiguration** sündmuste portaalis kuvatakse ainult teenuse käivitamisel on nõutud, ja seal on pole sündmus, mis näitab, et see on edukalt lõpule viidud.
- Saate veebiportaali UI Arvuta teenuse eksemplar SQL-andmebaasi eksemplarid ja järjekorrad linkida. See võimaldab teil hõlpsam juurde eraldi käsitsi ja automaatne skaleerimist konfiguratsiooni suvandite lingitud ressursid. Lisateavet leiate teemast [kohta: Link ressursi pilveteenusesse](cloud-services-how-to-manage.md#linkresources) ja [Kuidas mastaapimiseks rakendus](./cloud-services/cloud-services-how-to-scale.md).
- Kui konfigureerite mitme poliitikad ja reeglid, võivad need omavahel konflikti. Autoscale kasutab järgmisi reegleid konflikti lahendamise veenduge, et on alati piisav töötavate eksemplaride arv:
  - Skaala toimingutes ülimuslik alati skaala toiminguid.
  - Kui mastaapimiseks välja toimingute konflikti, alistab reegli, mis käivitab kõige rohkem eksemplaride arv.
  - Kui mastaapimiseks toimingute konfliktis, alistab reegli, mis käivitab väikseim vähenemine eksemplaride arv.

<a name="the-azure-monitoring-services-management-library"></a>

## <a name="application-design-considerations-for-implementing-autoscaling"></a>Rakenduste kujundamine autoscaling rakendamiseks
Autoscaling pole instant lahendus. Lihtsalt lisada ressursid süsteemi või töötab mitu eksemplari protsessi ei garanteeri süsteemi jõudlust parandada. Autoscaling strateegia kujundamisel võtke arvesse järgmist:

- Süsteemi peab olema horisontaalselt scalable eesmärk. Vältida oletused kohta eksemplari osaleja; Kujundage lahendusi, mis nõuavad, et teatud eksemplari protsess töötab alati koodi. Kui pilveteenuses või veebisaidi horisontaalselt skaala ei võta taotlusi sama lähtekoha sarja alati suunatakse samas eksemplaris. Samal põhjusel kujundamine teenuste kodakondsuseta vältimiseks nõudva sarja taotlusi rakendusest alati suunatakse teenuse samas eksemplaris. Teenus, mis loeb sõnumite järjekord ja töötleb neid kujundamisel tegemisest mis tahes kohta, millised eksemplari teenuse kõrval olevaid pidemeid oletused kindla sõnumi. Autoscaling võiks alustada ülejäänud esinemisjuhud teenuse järjekorda pikkus kasvab. [Kiiruisutamises tarbijad mustri](http://msdn.microsoft.com/library/dn568101.aspx) kirjeldatakse, kuidas hallata selle stsenaariumi.
- Kui lahendus kasutab pikaajalisi tööülesande, projekti selle ülesande toetamiseks skaleerimist välja nii skaleerimist sisse. Ilma hooldus, sellise ülesande võivad takistada eksemplari protsessi sulgeb puhtalt kui süsteemi skaala sisse või see võib andmed lähevad kaotsi, kui protsess on lõpetatud sunniviisiliselt. Ideaalvariandis mitte refaktoorime pikaajalisi tööülesande ja töötlemine, mida see teeb väiksem, kindlad tükkideks lahku. Soovitud [üksust Pipes ja filtrid mustri](http://msdn.microsoft.com/library/dn568100.aspx) pakub näide sellest, kuidas te võite saavutada.
- Teise võimalusena saate rakendada, et kirjed maakond teave ülesande kohta intervalliga ja salvestage see olek püsival salvestusruumi, mida saab kasutada mis tahes astme protsess töötab tööülesande kontrollpunkt süsteem. Sel viisil, kui protsess on sulgumist, mis täitis töö võib jätkata Viimane kontrollpunkt: muus eksemplaris abil.
- Käitamisel tausttoimingud eraldi Arvuta aknad, näiteks töötaja rollid on pilveteenuste majutatud rakenduses, peate mastaapimiseks erinevate osade rakenduse abil erinevad skaleerimise reeglid. Näiteks peate juurutamine täiendavad kasutaja kasutajaliidese (UI) Arvuta eksemplaride arvu taust ilma arvutada eksemplari, või vastupidi, see. Kui te ei paku erinevaid tasemeid teenuse (nt põhi- ja premium paketid), peate mastaapimiseks Arvuta allikatest premium teenuse pakette agressiivsemalt kui tavaline pakettide SLAs täitmiseks.
- Kaaluge pikkus, mille Kasutajaliidese ja tausta arvutada eksemplarid suhelda oma autoscaling strateegia kriteeriumina järjekorda. See on parim indikaator tasakaalustamatuse või praegune koormus ja töötlemise võimsus tausta tööülesande erinevus.
- Kui autoscaling strateegia kavandamine aluseks väidab, et mõõta äriprotsesse, näiteks arv tunnis või keerukas tehingu keskmine täitmisaeg tellimusi tagada järgmist tüüpi hinnale tulemusi seos tegeliku Arvuta võimsus nõuetele täielikult mõista. See võib osutuda vajalikuks mastaapimiseks rohkem kui üks osa või arvutada üksus business protsessi hinnale muutustega.  
- Süsteemi proovite mastaapimiseks välja liiga ja töötavate eksemplaride arvu tuhandeliste seotud kulude vältimiseks vältimiseks kaaluge eksemplarid, mis saab automaatselt lisada maksimaalne arv piirata. Enamik autoscaling menetlustele abil saate määrata miinimum- ja reegli eksemplaride arv. Lisaks, kaaluge nõtkelt alandav funktsioone, mida süsteem pakub kui eksemplaride arvu ülempiir juurutanud ja süsteem on endiselt ülekoormatud.
- Pidage meeles ei pruugi selle autoscaling kõige paremini vastab süsteemi käsitlema ootamatu plahvatuse rakenduses töökoormus. Võtab aega ettevalmistamine ja uued eksemplarid teenuse käivitamine või ressursse lisada süsteemi ja tippväärtus nõudmisel võib olla järgmised täiendavad ressursid on kättesaadavaks tehtud ajaks möödas. Selle stsenaariumi korral võib olla parem throttle teenus. Lisateavet leiate teemast [Pidurdamise mustri](http://msdn.microsoft.com/library/dn589798.aspx).
- Vastupidi, kui teil on vaja võimet kõik taotlused helitugevuse muutub kiiresti, kui maksumus pole põhi tegur, kaaluge agressiivne autoscaling strateegia, mis käivitab täiendavad eksemplarid kiiremini. Samuti saate ajastatud poliitika, mis käivitab piisavalt arvul enne eeldatakse, et koormuse koormus täita.
- Autoscaling süsteem autoscaling jälgimiseks, ja logige iga autoscaling sündmuse üksikasjad (mis vallandanud dokumendil, millised ressursid on lisada või eemaldada, ja millal). Kui teil luua kohandatud autoscaling, veenduge, et see sisaldab seda võimalust. Analüüsimiseks teave, mõõt autoscaling strateegia ja häälestada see vajadusel. Saate häälestada nii lühiajaliselt kasutus mustrite muutuvad selgemaks ja üle pikaajaline, nagu ettevõtte laieneb või rakenduse nõuetele muutuda. Kui rakenduse jõuab autoscaling jaoks määratletud ülempiiri, võib selle süsteemi ka Teavita tehtemärgi, kes võivad käsitsi käivitamine lisaressursid vajaduse korral. Pidage meeles, et, sellisel juhul tehtemärk ka käsitsi eemaldada järgmiste ressursside pärast töökoormus lihtsustab.

## <a name="related-patterns-and-guidance"></a>Seotud mustrite ja juhised
Järgmised mustrite ja juhiseid ka võib milline olukord teie jaoks oluline autoscaling rakendamisel:

- [Mustri ahendamine](http://msdn.microsoft.com/library/dn589798.aspx). See muster kirjeldatakse, kuidas saate jätkata rakenduse funktsioon ja vajadusel suurendamist paigutab ressursid on äärmiselt laadi SLAs koosolekud. Pidurdamise saab kasutada koos autoscaling vältimiseks süsteemi on ülekoormatud ajal süsteemi skaala välja.
- [Kiiruisutamises tarbijad mustri](http://msdn.microsoft.com/library/dn568101.aspx). See muster kirjeldatakse, kuidas rakendada teenuse eksemplarid, mis tahes rakenduse eksemplari sõnumite käsitlemiseks kogumi. Autoscaling saab kasutada käivitamine ja peatamine eksemplaride eeldatava töökoormus vastavaks. Seda moodust võimaldab süsteemi töötlemiseks mitme sõnumid samaaegselt optimeerimine läbilaskevõime ja parandada skaleeritavus ja kättesaadavust saldo töökoormus.
- [Näidikuid ja telemeetria juhised](http://msdn.microsoft.com/library/dn589775.aspx). Näidikuid ja telemeetria on olulised Koguge kokku teave, mida saate juhtida autoscaling protsessi.

## <a name="more-information"></a>Lisateave
- [Skaala rakenduse kohta](./cloud-services/cloud-services-how-to-scale.md)
- [Automaatselt mastaapida rakendus töötab Web rollid, töötaja rolli või Virtuaalmasinates](cloud-services-how-to-manage.md#linkresources)
- [Kuidas: pilveteenus ressursi linkimine](cloud-services-how-to-manage.md#linkresources)
- [Mastaapimiseks lingitud ressursid](./cloud-services/cloud-services-how-to-scale.md#scale-link-resources)
- [Azure'i teenuste teegis jälgimine](http://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring)
- [Azure'i Teenusehaldus REST API-ga](http://msdn.microsoft.com/library/azure/ee460799.aspx)
- [Azure'i ressursihaldur REST API-ga](https://msdn.microsoft.com/library/azure/dn790568.aspx)
- [Microsofti Insights teek](https://www.nuget.org/packages/Microsoft.Azure.Insights/)
- [Autoscaling toimingud](http://msdn.microsoft.com/library/azure/dn510374.aspx)
- [Microsoft.WindowsAzure.Management.Monitoring.Autoscale Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.management.monitoring.autoscale.aspx)
