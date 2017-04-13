<properties
    pageTitle="Valimi andmekogumi kasutamine arvuti õ Studio | Microsoft Azure'i"
    description="Kasutatud valimi mudelite kaasatud ML Studio andmekogumi kirjeldused. Saate neid andmekogumite oma katsete."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Azure'i masina õ Studio valimi andmekogumi kasutamine

[top]: #machine-learning-sample-datasets

Uue tööruumi loomisel Azure seadme õ on vaikimisi kaasatud andmekogumite ja katsete arv. Paljude nende andmekogumite kasutavad valimi mudelite [Azure'i Cortana ärianalüüsi Galerii](http://gallery.cortanaintelligence.com/)ja teised on kaasatud erinevat tüüpi andmetega tavaliselt kasutatakse masina õ näitena.

Mõned nende andmekogumi on saadaval Azure'i bloobimälu. Nende andmekogumite järgmises tabelis antakse otseselt. Saate kasutada järgmisi andmekogumi oma katsete abil [Andmete importimine] [ import-data] mooduli.

Nende andmekogumite ülejäänud on loetletud jaotises **Salvestatud andmekomplektide** mooduli värvipaleti vasakule katse lõuendi avamisel või luua uue katse ML Studio.
Saate kasutada mis tahes nende andmekogumi oma katse lohistada oma katse lõuend.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>Andmekomplekti nimi</th>
  <th align=left>Andmekomplekti kirjeldus</th>
</tr>

<tr>
  <td valign=top>Täiskasvanud rahvaloendus tulu kahendarvu liigitamine andmekomplekti</td>
  <td valign=top>
Kasutades üle 16 > 100 registri korrigeeritud tulu koos töötamine Double 1994 rahvaloendus andmebaasist alamhulga.<p> </p><b>Kasutamine:</b> liigitada inimestega, kes kasutavad demograafia prognoosida, kas inimene teenib üle 50K aastas.<p> </p><b>Seotud uurimistöö:</b> Kohavi R., Becker, b, (1996). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Andmekomplekti Airport koodid</td>
  <td valign=top>
USA airport koode.<p> </p>Tuvastatavate sisaldab üht rida iga USA airport, pakkudes airport ID-numbri ja koos asukoha linna ja osariigi nimi.
  </td>
</tr>

<tr>
  <td valign=top>Auto andmed (Raw)</td>
  <td valign=top>
Teavet poolt tegemine ja modelleerimise hind, sh funktsioone, näiteks balloonide ja MPG, samuti on riski suurust keskmine arv.<p> </p>Risk Keskmine on esialgu seotud automaatne hind ja seejärel kohandatud tegelik risk teadaolevalt aktuaaride nimega symboling protsessis. + 3 väärtus näitab, et automaatse on riskantne ja väärtuse – 3, et see on üsna tõenäoliselt ohutu.<p> </p><b>Kasutamine:</b> prognoosida funktsioonid, kasutades regressiooni või mitmemõõtmelised liigitamine risk Keskmine. <p> </p><b>Seotud uurimistöö:</b> Schlimmer, J.C. (1987). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Bike rent UCI andmekomplekti</td>
  <td valign=top>
UCI rattarenti andmekomplekti tegelikke andmeid muutmine Bikeshare ettevõttest, mis säilitab bike rent võrgu Washingtoni AV põhjal.<p> </p>Andmekomplekti on üks rida iga tund iga päev 2011 ja 2012 kokku 17,379 read. Tunni jalgrattalaenutust vahemik on 1-977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates RGB pilt</td>
  <td valign=top>
Avalikult kättesaadavaks pildifaili teisendatakse CSV-faili andmed.<p> </p>Pildi teisendamine kood on toodud <strong>värvi abil K-tähendab rühmitamise Kvantiseerimine</strong> mudeli üksikasjade lehel.
  </td>
</tr>

<tr>
  <td valign=top>Vererõhumõõtja annetuse andmed</td>
  <td valign=top>
Vererõhumõõtja doonori andmebaasi vereülekanne teenuse keskele, Hsin-Chu linn, Taiwan andmete alamhulga.<p> </p>Doonori andmete sisaldab selle kuu viimase annetuse), ja sagedus või annetused, alates viimase annetuse koguarvu ja vererõhumõõtja kingitust summa.<p> </p><b>Kasutamine:</b> eesmärk on prognoosida teel liigitamine, kas doonor kingitust vererõhumõõtja märtsis 2007, kus 1 näitab doonor target jooksul, ja 0 mitte-doonor. <p> </p><b>Seotud uurimistöö:</b> Yeh päramootorid, (2008). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia <p> </p>Yeh ma Cheng Yang, King-Jang ja Ting, lõpetamiseks koputage-mingi "teadmisi discovery RFM mudeli Bernoulli järjestust, kasutades" ekspert süsteemide rakendustega, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Raamat ülevaateid Amazon</td>
  <td valign=top>
Arvustusi raamatuid Amazon, tehtud amazon.com veebisaidilt Witmeri teadlaste (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">meeleolu</a>). Uurimistöö paberi, lugege teemat "Elulood, stiil, Boom-väljade ja Blenderid: domeeni kohandamine meeleolu liigitamiseks" John Blitzer, Mark Dredze ja Fernando Pereira; Seos arvutuslik lingvistika (ACL), 2007.<p> </p>Algne andmekomplekti on 975K ülevaateid, mille tulemused 1, 2, 3, 4 ja 5. Kommentaare on kirjutatud inglise keeles ja on pärit aja jooksul 1997 – 2007. Tuvastatavate on valimisse alla 10K kommentaare.
  </td>
</tr>

<tr>
  <td valign=top>Rindade vähk andmed</td>
  <td valign=top>
Üks kolme vähiga seotud andmekomplektide on sageli kuvatakse seadme õ kirjandust onkoloogia asutuse järgi. Ühendab diagnostikateave umbes 300 koeproovide laboratoorse analüüsi funktsioonide abil.<p> </p><b>Kasutamine:</b> liigitada tüüpi vähk, vastavalt 9 atribuute, mis on lineaarse ja mõned kategooriline. <p> </p><b>Seotud uurimistöö:</b> Wohlberg, W.H., tänav, W.N. ja Mangasarian, O.L. (1995). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Rindade vähk funktsioonid <td valign=top>
Andmekomplekti sisaldab teavet 102K kahtlaste piirkondade (leviloendite) X-ray pilte, iga kirjeldatud 117 funktsioonid. Funktsioonid, mis on kaitstud ja nende tähendused on ilmnenud pole andmekomplekti loojad (Siemens Healthcare). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Rindade vähk teave</td>
  <td valign=top>
Andmekomplekti sisaldab läbivaadatava iga kahtlaste piirkonna lisateavet. Iga näide annab teavet (nt silt, patsientide ID koordinaadid paik terve pildi suhtes) vastava rea number, rindade vähk funktsioonid andmekomplekti kohta. Iga on mitmeid määratavaid näited. Patsientide, kes on mõne, mõned näited on positiivne ja mõned on negatiivne. Patsientide, kes ei on soovitud, kõik näited on negatiivne. Andmekomplekti on 102K näited. Andmekomplekti erapoolik, 0,6% punktide on positiivne, ülejäänud on negatiivne. Andmekomplekti käsutusse Siemens Healthcare.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>Ühiskasutusse antud CRM Appetency Sildid</td>
  <td valign=top>
Siltide KDD Cup 2009 kliendi seose ennustamine ülesanne (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>) kaudu.
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>CRM piimapütt ühiskasutuses Sildid</td>
  <td valign=top>
Siltide KDD Cup 2009 kliendi seose ennustamine ülesanne (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>) kaudu.
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>CRM andmekomplekti ühiskasutuses</td>
  <td valign=top>
Need andmed on pärit KDD Cup 2009 kliendi seose ennustamine ülesanne (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Andmekomplekti sisaldab 50K kliendid Prantsuse Telecom ettevõttest oranž. Iga kliendi on 230 anonüümseks funktsioone, 190, millele on arvväärtusega ja 40 on kategooriline. Funktsioonid on väga vähe.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>Ühiskasutusse antud CRM lisaväärtuste Sildid</td>
  <td valign=top>
Siltide KDD Cup 2009 kliendi seose ennustamine ülesanne (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>) kaudu.
  </td>
</tr>

<tr>
  <td valign=top>Tõhusust regressiooni andmed</td>
  <td valign=top>
Kogum jäljendatud kujutatud profiilid, mis põhineb 12 eri building kujundid. Hoonete erinevad osas 8 funktsioone, näiteks turvaklaaside ala, aknad jaotuse ja suuna järgi.<p> </p><b>Kasutamine:</b> kujutatud tõhusust hindamine üks kaks real väärtusega vastuste põhjal prognoosida regressiooni või liigitamine abil. Arvu ümardamine lähima täisarvuni vastuse muutuja on mitme klassi liigitamine. <p> </p><b>Seotud uurimistöö:</b> Xifara v & Tsanas, A. (2012). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Flight viivitused andmete</td>
  <td valign=top>
Reisija flight jõudluse kohta aja andmed pärinevad TranStats andmete kogumine USA osakonna transport (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">-Kellaaeg</a>).<p> </p>Andmekomplekti hõlmab aja jooksul aprillis – oktoobris 2013. Enne üleslaadimine Azure'i ML Studio, töödeldud andmekomplekti järgmiselt:<ul><li>Andmekomplekti on filtreeritud kataks ainult 70 busiest lennujaamade continental US</li><li>Tühistatud olid märgistatud hilinenud rohkem kui 15 minutit</li><li>Ümbersõidetud lennud on välja filtreeritud.</li><li>Valitud järgmisi veerge: aasta, kuu, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, tühistatud</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Flight õigeaegne täitmine (Raw)</td>
  <td valign=top>
Kirjete lennukikujuline lennud ja lahkuvate jooksul Ameerika Ühendriigid: oktoober 2011.<p> </p><b>Kasutamine:</b> prognoosida lendude hilinemist. <p> </p><b>Seotud uurimistöö:</b> meile osakond, transport <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>kaudu.
  </td>
</tr>

<tr>
  <td valign=top>Mets käivitub andmed</td>
  <td valign=top>
Sisaldab ilm andmed, nt temperatuuri- ja indeksite ja tuulekiirus kirdepiirkonna Portugal, koos kirjete metsatulekahjude ala.<p> </p><b>Kasutamine:</b> see on keeruline regressiooni ülesanne, kus eesmärk on prognoosida kirjutatud ala metsatulekahjude. <p> </p><b>Seotud uurimistöö:</b> Cortez, lk, & Morais, A. (2008). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia <p> </p>[Cortez ja Morais, 2007] Lk Cortez ja v Morais. Andmete Miningu lähenemine prognoosida mets tule meteoroloogiliste andmete kasutamine. Klõpsake J. Neves M. F. Santos ja J. Machado Eds., uue trende Ai ärianalüüsi menetluse 13 EPIA 2007 – Portugali konverentsiga Ai ärianalüüsi, detsember, Guimarães, Portugal, lk 512-523, 2007. APPIA, ISBN-13 978-989-95618-0 – 9. Saadaval: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Saksa krediitkaardi UCI andmekomplekti</td>
  <td valign=top>
UCI Statlog (saksa krediitkaart) andmekomplekti (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Saksa + krediitkaardiga + andmete</a>) german.data-faili abil.<p> </p>Andmekomplekti jaotab inimesed, määrata atribuute, nagu kõrge või madal riskide kirjeldatud. Iga näites tähistab isik. On 20 funktsioone, nii numbriliste ja kategooriline, ja kahendarvu sildi (krediidi risk väärtus). Kõrge krediidi risk kirjed on sildi = 2, madal krediidi risk kirjed on sildi = 1. Maksumus misclassifying madal risk näide kui suur on 1, misclassifying suure näide madal maksumus on 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB filmi pealkirjad</td>
  <td valign=top>
Andmekomplekti sisaldab teavet filmid, mis olid hinnatud Twitteri tweets: IMDB filmi ID, filmi- ja Žanr, aasta. Andmekomplekti on 17K filmid. Andmekomplekti võeti kasutusele paberi "S. Dooms, T. De Pessemier ja L. Martens. MovieTweetings: hindamine andmekomplekti filmi kogutud Twitteri. Töörühma Crowdsourcing ja inimeste arvutus soovitaja süsteemide, RecSys 2013 CrowdRec."
  </td>
</tr>

<tr>
  <td valign=top>Iris kahe tunni andmed</td>
  <td valign=top>
See on võib-olla tuntud andmebaasi mustri tuvastus kirjandust leida. Andmehulga on üsna väike, 50 näiteid kroonleht mõõtmise kolme iris sordi.<p> </p><b>Kasutamine:</b> prognoosida iris tüüp: mõõtmed.  <p> </p><b>Seotud uurimistöö:</b> Fisher asumisest (1988). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Filmi Tweets</td>
  <td valign=top>
Andmekomplekti on laiendatud versiooni filmi Tweetings andmekomplekti. Andmekomplekti on 170K hinnangute filmid, hästi struktureeritud tweets Twitter ekstraktimist. Iga eksemplari säutsu ja on korteeži: kasutaja ID, IMDB filmi ID, reiting, ajatempli, numer säutsu ja arvu retweets, see säutsu lemmikud. Andmekomplekti on kättesaadav A. Said, S. Dooms, B. Loni ja D. Tikk soovitaja süsteemide ülesanne 2014.
  </td>
</tr>

<tr>
  <td valign=top>Eri sõiduautode MPG andmed</td>
  <td valign=top>
Tuvastatavate on veidi muudetud versiooni andmekomplekti esitatud StatLib Raamatukogu Carnegie Mellon University. Andmekomplekti kasutati 1983 Ameerika statistika seos organisatsiooni.<p> </p>Andmed on loetletud kütusekulu eri sõiduautode miili gallon koos teabega sellise arvu balloonide, engine nihke, Hobujõud, kogukaal ja kiirenduse kohta.<p> </p><b>Kasutamine:</b> prognoosida kütusesäästu põhjal 3 mitmeväärtuselise eraldi atribuudid ja 5 pidev atribuudid. <p> </p><b>Seotud uurimistöö:</b> StatLib, Carnegie Mellon University, (1993). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia </td>
</tr>

<tr>
  <td valign=top>Andmekomplekti Pima indiaanlased II kahendarvu liigitamine</td>
  <td valign=top>
Liikmesriikide II ja seedetrakti ja neeruhaigused andmebaasi andmete alamhulga. Andmekomplekti on filtreeritud, et fookus naiste Pima India pärand kohta. Andmed sisaldavad meditsiiniline andmeid nagu glükoosi ja insuliini elustiili tegurid.<p> </p><b>Kasutamine:</b> prognoosida, kas teema on II (kahendarvu liigitus). <p> </p><b>Seotud uurimistöö:</b> Sigillito v (1990). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia </td>
</tr>

<tr>
  <td valign=top>Restorani kliendiandmete</td>
  <td valign=top>
Metaandmete klientide, sh demograafia ja eelistused kogum.<p> </p><b>Kasutamine:</b> selle andmehulga kasutamine koos muude kaks restorani andmekogumite, koolitada ja testimine soovitaja süsteem. <p> </p><b>Seotud uurimistöö:</b> Bache K. ja Lichman, M. (2013). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia.
  </td>
</tr>

<tr>
  <td valign=top>Restorani funktsioonide andmed</td>
  <td valign=top>
Kogum metaandmete Restoran ja nende funktsioone, näiteks toidu tüüp, La laad ja asukoha kohta.<p> </p><b>Kasutamine:</b> selle andmehulga kasutamine koos muude kaks restorani andmekogumite, koolitada ja testimine soovitaja süsteem. <p> </p><b>Seotud uurimistöö:</b> Bache K. ja Lichman, M. (2013). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia.
  </td>
</tr>

<tr>
  <td valign=top>Restorani hinnangute</td>
  <td valign=top>
Sisaldab hinnangute antud kasutajatele Restoran skaalal 0 2.<p> </p><b>Kasutamine:</b> selle andmehulga kasutamine koos muude kaks restorani andmekogumite, koolitada ja testimine soovitaja süsteem. <p> </p><b>Seotud uurimistöö:</b> Bache K. ja Lichman, M. (2013). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia.
  </td>
</tr>

<tr>
  <td valign=top>Terasest Annealing mitme klassi andmekomplekti</td>
  <td valign=top>
Tuvastatavate sisaldab mitmeid kirjeid terasest lõõmutamine katsete_arv füüsilise atribuutidega (laius, paksus, tüüp (spiraali, lehe jne) tulemuseks terasest tüübid.<p> </p><b>Kasutamine:</b> prognoosida kõiki kahe arvulise klassi atribuute; tugevus või tugevus. Samuti võib analüüsida korrelatsiooni vahel atribuute.<p> </p>Teraseliigid järgida standard, SAE ja teiste asutuste määratletud. Otsite kindla "klass" (klassi muutuja) ja soovite väärtused, mis on vaja mõista. <p> </p><b>Seotud uurimistöö:</b> Sterling, D. & Buntine W., (NA). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Californias, kooli, teavet ja infotehnoloogia <p> </p>Kasulik juhend teraseliigid leiate siit: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Teleskoop andmed</td>
  <td valign=top>
Kõrge kujutatud gamma osakeste puruneb koos taustamüra kirjeid nii modelleerida Roc protsessi abil.<p> </p>Soovidele simulatsioon oli maa-põhiste väliskeskkonna Cherenkov gamma teleskoobid, statistiliste meetodite abil eristada soovitud signaal (Cherenkov kiirguse kohati) ja taustamüra (hadronic kohati algataja kosmiline kiired ülemise keskkonnas) täpsuse suurendamiseks.<p> </p>Andmed on eelnevalt töödeldud loomiseks on pikliku kobar pikk telg on suunatud kaamera keskele. See ellips, (sageli nimetatakse Hillas parameetrid) omadused on pildi parameetrid, mida saab kasutada diskrimineerimise vahel.<p> </p><b>Kasutamine:</b> prognoosida, kas pildi shower tähistab signaal või tausta müra.<p> </p><b>Märkmed:</b> lihtsa liigitamine täpsuse ei ole selles andmete jaoks tähenduslik, sest liigitamine tausta sündmus, nagu signaal on halvemaks liigitamine signaal sündmuse tausta. Erinevate liigitused võrdlus tuleks kasutada ROC graafik. Tõenäosuse aktsepteerimist tausta sündmuse signaal peab olema alla järgmised piirmäärade: 0,01, 0,02, 0, 05, 0,1 või 0,2.<p> </p>Pange tähele, et tausta sündmuste (h, hadronic kohati) on alahinnatud, ka otse mõõtmised, h või müra klassi kujutab enamik sündmused. <p> </p><b>Seotud uurimistöö:</b> Bock, R.K. (1995). UCI masina õ hoidla <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Californias University kooli info </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Ilm andmekomplekti</td>
  <td valign=top>
Kord tunnis maa-põhiste ilm märkused NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">201304, et 201310 ühendatud andmed</a>).<p> </p>Andmed hõlmab tähelepanekuid kaugusel airport ilm, mis hõlmab aja jooksul aprillis – oktoobris 2013. Enne üleslaadimine Azure'i ML Studio, töödeldud andmekomplekti järgmiselt:<ul><li>Ilmajaam ID-d on vastendatud vastavate airport ID-d</li><li>Pole seotud 70 busiest lennujaamade Ilmajaamad on välja filtreeritud</li><li>Veeru kuupäev on jagatud eraldi aasta, kuu ja päeva veerud</li><li>Valitud järgmisi veerge: AirportID, aasta, kuu, päev, aeg, ajavöönd, SkyCondition, nähtavuse, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, tuulekiirus, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, kõrgusemõõtja</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 andmekomplekti</td>
  <td valign=top>
Andmed on tuletatud Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) artiklid iga S & P 500 ettevõtte, mis on salvestatud XML-andmete põhjal.<p> </p>Enne üleslaadimine Azure'i ML Studio, töödeldud andmekomplekti järgmiselt:<ul><li>Teksti sisu ekstraktida iga konkreetse ettevõtte jaoks</li><li>Viki vormingu eemaldamine</li><li>Mitte-tähtnumbrilisi märke eemaldamine</li><li>Kogu teksti väiketähtedeks teisendada</li><li>Tuntud ettevõtte kategooriad on lisatud</li></ul><p> </p>Pange tähele, et mõned ettevõtted artikkel ei leitud, et kirjete arv on väiksem kui 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.CSV</a></td>
  <td valign=top>
Andmekomplekti sisaldab Kliendiandmete ja teavet otse e-posti turunduskampaania oma vastuse. Iga rea tähistab kliendi. Andmekomplekti sisaldab 9 funktsioonide kohta kasutaja demograafia ja viimase käitumine ja 3 sildi veergu (külastage, teisendamine, ja veedavad).  Külastage on kahendarvu veerg, mis näitab, et kliendi külastatud pärast turunduskampaania, teisendamise näitab kliendi ostetud midagi ja veedavad on kulunud summa.  Andmekomplekti käsutusse Kevin Hillstrom jaoks MineThatData e-posti analüüsi ja andmete Miningu ülesanne.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.CSV</a></td>
  <td valign=top>
Funktsioonid RCV1-V2 Reuters uudiste andmekomplekti testi näiteid. Andmekomplekti on 781K uudiseid koos oma ID-d (esimene veerg andmekomplekti). Iga artiklis tokenized, stopworded, ja tulenes. Andmekomplekti käsutusse David. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.CSV</a></td>
  <td valign=top>
Koolitus näited RCV1-V2 Reuters uudiste andmekomplekti funktsioone. Andmekomplekti on 23K uudiseid koos oma ID-d (esimene veerg andmekomplekti). Iga artiklis tokenized, stopworded, ja tulenes. Andmekomplekti käsutusse David. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.CSV</a><br></td>
  <td valign=top>
Andmekomplekti KDD Cup 1999 teadmisi Discovery and Data Miningu tööriistad konkurentsiga (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>) kaudu.<p> </p>Andmekomplekti on alla laaditud ja talletatud Azure'i bloobimälu (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) ja sisaldab nii koolitus ja testimine andmekomplektide. Koolitus andmekomplekti on ligikaudu 126K read ja 43 veerud, sh Sildid; 3 veeru kuuluvad silditeabe ja 40 veergude arv ja string/kategooriline funktsioonid, mis on saadaval koolitus mudel. Testi andmed on ligikaudu 22,5 K testida näited sama 43 veergudega nagu koolitus andmed.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td valign=top>
Teema: ülesanded uudiste artiklite RCV1-V2 Reuters uudiste andmekogumis. Uudised artikli saate määrata mitu teemat. Iga rea vorming on "<topic name>  <document id> 1". Andmekomplekti sisaldab 2,6 M teema ülesanded. Andmekomplekti käsutusse David. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Need andmed on pärit KDD Cup 2010 kool jõudluse hindamise ülesanne (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">kool jõudluse hindamiseks</a>). Kasutatud andmed on Algebra_2008_2009 koolituse määramine (tembeldaja J., Niculescu-Mizil A. Ritter, S., Gordon, j. & Koedinger K.R. (2010). Algebra ma 2008 2009. Ülesanne KDD Cup 2010 õppe Data Miningu ülesanne kaudu andmehulka. Otsige üles <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> või <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Andmekomplekti on alla laaditud ja talletatud Azure'i bloobimälu (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) ja see sisaldab logifailid õpilane juhendamine süsteem. Esitatud funktsioonid sisaldavad probleemi ID ja selle lühikirjeldus, õpilaste ID, ajatempli ja mitu katsete õpilase enne probleemi lahendamiseks õigesti. Algne andmekomplekti on 8,9 M kirjeid, tuvastatavate alla valimisse 100K esimese ridu. Andmekomplekti on 23 tabeleraldusega veerud eri tüüpi: arv kategooriline, ja ajatempel.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
