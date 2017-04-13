<properties
   pageTitle="Tasakaalustamiseks klaster koos Azure teenuse struktuuri kobar ressursihaldur | Microsoft Azure'i"
   description="Sissejuhatus koos teenuse struktuuri kobar ressursihaldur klaster tasakaalustamiseks."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="balancing-your-service-fabric-cluster"></a>Tasakaalustamiseks klaster teenuse struktuuri
Teenuse struktuuri kobar ressursihaldur võimaldab aruandlus dünaamiline koormus, reageeri muudatuste klaster, piirang rikkumised parandamine ja ümberkorraldamine klaster vajadusel. Aga kui tihti seda teha järgmisi toiminguid ja mis käivitab selle? On mitu juhtelemendid, mis on seotud selle.

Juhtelementide ümber tasakaalustamiseks esimeste on taimerid kogum. Nende taimerid haldavad sageduse kobar ressursihaldur uurib asju, mida tuleb tegeleda kobar olek. On kolme eri kategooria töö, igal versioonil oma vastavate timer. Need on:

1.  Paigutuse – selle etapi tegeleb mis tahes stateful koopiad või kodakondsuseta eksemplarid, mis on puudu. See hõlmab uusi teenuseid ja töötlemise stateful koopiad või kodakondsuseta eksemplarid, mis ei ole vaja uuesti luua. Kustutamise ja pukseerimine koopiad või eksemplari käsitletakse ka siin.
2.  Piirangu kontrollib – selle etapi kontrollib ja parandab rikkumise süsteemis piiranguid erinevate paigutus (sätted). Reeglite näited, näiteks tagada sõlmed pole üle mahu ja teenuse paigutuse piiranguid (rohkem hiljem) on täidetud.
3.  Tasakaalustamiseks – selle etapi kontrollib, kas ennetavad ümberkorraldamine on vaja konfigureeritud soovitud taset saldo erinevate mõõdikute põhjal, ja sel juhul proovib korralduse kobar, mis saabuv rohkem otsimine.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Kobar ressursihaldur etapid ja taimerid konfigureerimine
Kõik need parandused kobar ressursihaldur saab teha erinevat tüüpi kontrollib erinevate timer, millega reguleeritakse selle sagedus. Nii näiteks, kui soovite tegeleda uue teenuse töökoormus paigutamine klaster iga tund (et partii neid üles), kuid soovite tavaline tasakaalustamiseks kontrollib iga paari sekundi järel, saate konfigureerida seda käitumist. Kui iga timer, käivitub, on ajastatud ülesanne. Vaikimisi ressursihaldur skannib seisu ja värskendused (partiide kõik muudatused, mis on ilmnenud pärast viimast skannimine, nt märganud sõlm on alla) iga 1/10 sekundiks komplekti paigutuse ja piirang märkige lipud iga teine ja selle tasakaalustamiseks lipu iga 5 sekundi järel.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Täna me teha ainult üks neid toiminguid korraga, järjest (Sellepärast me viidata neid konfiguratsioone nimega "minimaalne intervallide")). See on nii, et näiteks me olete juba vastanud mis tahes ootel luua uue koopiad enne me liikuda klaster tasakaalustamiseks. Nagu näete, määratud vaikimisi ajavahemike, saame skannida ja sisse midagi, mida läheb vaja teha väga sageli tähendab, et muudatused määramine teeme iga etapi lõpus on tavaliselt väiksem: me ei arvust läbi tunni muudatuste klaster ja proovite neid kõiki korraga vea, me üritavad toime asjad, mida rohkem või vähem toimuvaga, kuid mõned partiide kui juhtuda paljud asjad on samal ajal. See muudab teenuse struktuuri ressursi halduri väga tundlik asjad, mis tehakse klaster.

Kuigi enamik neist tööülesanded on lihtsad (kui on piirang rikkumised parandamine, kui teenuste loomist, mis on luua neid), kobar ressursihaldur vajab täiendavat teavet kindlaks teha, kui tasakaalustamata kobar. Mis on meil muude kahe andmeühikuga konfiguratsiooni: *Tasakaalustamiseks lävede* ja *Tegevuste lävede*.

## <a name="balancing-thresholds"></a>Tasakaalustamiseks piirmäärad
Tasakaalustamiseks lävi on peamised juhtelemendi jaoks käivitamise ennetavad ümberkorraldamine (Pidage meeles, et ajasti on lihtsalt kontrollima jaoks sageduse ressursihaldur kobar - ei tähenda, et midagi ei juhtu). Tasakaalustamiseks lävi määratleb, kuidas tasakaalustamata klaster peab olema teatud mõõdiku järjestuses jaoks kobar ressursihaldur see tasakaalustamata ja päästik tasakaalustamiseks silmas pidada.

Tasakaalustavad lävede on määratletud mõõdiku kohta eraldi kobar määratluse osana. Lisateavet mõõdikute vaadake [artiklit](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Tasakaalustamiseks mõõdiku on suhe. Kui laadi kõige laaditud sõlme jagatuna vähemalt laaditud sõlme laadi summa ületab see arv, siis klaster käsitletakse tasakaalustamata ja tasakaalustamiseks käivitatakse järgmine kord, kui kobar ressursihaldur kontrollib (vaikimisi kunagi sekundi reguleeritud MinLoadBalancingInterval, eespool näidatud).

![Tasakaalustavad lävi näide][Image1]

Selle lihtsa näite iga teenuse tarbib mõned meetermõõdustik üks üksus. Ülemise näites koormus sõlme on 5 ja väikseim 2. Oletame, et tasakaalustavad selle mõõdiku on 3. Seetõttu käsitletakse ülemise näites klaster tasakaalustatud ja pole tasakaalustamiseks käivitatakse kui kobar ressursihaldur kontrollib (kuna klaster on 5/2 = 2,5 ja mis on väiksem kui määratud tasakaalustavad piiri 3).

Näites all sõlm max koormus on 10, minimaalne on 2, tulemuseks suhte 5. See asetab klaster kujundatud tasakaalustavad lävi selle väärtuseks meetermõõdustik 3 kohale. Seetõttu saab globaalne teabePärast Käivita ajastatud järgmine kord, kui tasakaalustavad timer, käivitub. Pange tähele, et tasakaalustavad otsing on käivitati ei tähenda midagi liigub – mõnikord klaster on tasakaalustamata, kuid olukorda ei saa parandada – kuid olukorras, nagu see (vähemalt vaikimisi) mõned laadi jagatakse peaaegu kindlasti Node3. Pange tähele, et kuna me ei kasuta ahne lähenemine teatud laadi võib jaotada Node2 kuna mis tekitaks minimeerimine sõlmed üldine erinevused, kuid me oodata, et enamik laadi oleks tehingust Node3.

![Tasakaalustavad lävi näide toimingud][Image2]

Teate, et saada tasakaalustavad piirmäärast ei ole konkreetsete eesmärk – tasakaalustamiseks lävede on lihtsalt *päästik* mis ütleb kobar ressursihaldur, et see peaks välja nägema üheks kobar määratlemaks, milliseid parandusi seda saab teha, kui mõni.

## <a name="activity-thresholds"></a>Tegevuste piirmäärad
Mõnikord, kuigi sõlmed on üsna tasakaalustamata, *Laadi klaster kogusumma* on madal. See võib olla lihtsalt tõttu kellaaeg, või kuna klaster on uus ja lihtsalt saada bootstrapped. Mõlemal juhul võib-olla ei soovi tasakaalustamiseks klaster, kuna on tegelikult väga vähe saadud – saate just veedavad võrk ja Arvuta ressursse, mis ei muuda absoluutne midagi asju ümber, Teisalda aega. Kuna soovime hoiduge sellest, on mõne muu juhtelemendi sees ressursihaldur, nimetatakse tegevuse lävede, mis võimaldab teil määrata teatud absoluutne alampiir tegevuste – kui pole sõlm on vähemalt see palju laadi siis tasakaalustamiseks pole käivitatakse isegi juhul, kui tasakaalustamiseks lävi on täidetud.

Näiteks Oletame, et aruanded ettenähtud järgmised kokkuvõtetega sõlmed meil. Oletame, et soovime säilitada meie tasakaalustamiseks lävi selle mõõdiku 3, kuid nüüd ka on mõni tegevuse läve 1536 ka. Esimese juhul ajal klaster on tasakaalustamata kohta tasakaalustamiseks lävi pole sõlm vastab selle tegevuse alammäära, nii, et saaksime jätta asju eraldi. Näites all Node1 on viis üle tegevuse piirmäära, nii tasakaalustamiseks tehakse (kuna nii tasakaalustamiseks lävi ja mõõdiku tegevuse lävi on ületatud)

![Näide tegevuste lävi][Image3]

Nagu Thresholds tasakaalustamiseks tegevuse lävede on määratletud kohta – meetermõõdustik kobar määratluse kaudu.

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Pange tähele, et tasakaalustamiseks ja tegevuste mõlemad seotud mõõdiku - tasakaalustamiseks ainult käivitatakse kui tasakaalustamiseks ja tegevuste läve jaoks sama mõõt. Seega, kui me ületavad tasakaalustamiseks lävi mälu ja tegevuse piirmäär CPU, tasakaalustamiseks ei Käivita kui ülejäänud lävede (tasakaalustamiseks lävi CPU) ja mälu tegevuse piirmäära ei ole ületatud.

## <a name="balancing-services-together"></a>Tasakaalustamiseks teenuseid koos
Midagi, mis on huvitav märkida on kas klaster on tasakaalustamata või mitte on kobar kogu otsus, et nii, nagu me minna kohta, millega ta liigub üksikute teenuse koopiad ja eksemplari ümber. See on mõistlik, õige? Kui mälu on virnastatud ühe sõlme, mitu koopiad või eksemplarid võivad aidata seda, selle võib nõuavad teisaldada mis tahes stateful koopiad või kasutada probleemse, tasakaalustamata meetermõõdustik kodakondsuseta eksemplarid.

Aeg-ajalt kuigi kliendi helistavad koosolekule kontaktteabe kuni või faili Piletite räägivad, et teenus mis polnud tasakaalustamata saanud teisaldada. Kuidas see võib juhtuda, et teenus teisaldatakse ka siis, kui kõik selle teenuse mõõdikute olid tasakaalus, isegi täiesti sel ajal muid olemasolu? Vaatame!

Võtke näiteks neli teenused, Service1, elementi2, Service3 ja Service4. Service1 aruannete mõõdikute Metric1 ja Metric2 elementi2 Metric2 ja Mmetric3 Service3 Metric3 ja Metric4 vastu ja Service4 vastu mõned argumendil Metric99 suhtes. Kindlasti näete, kus meil läheb siin. Meil on ahelas! Seisukohalt kobar ressursihaldur me ei pea tingimata nelja sõltumatu teenuse, meil on hulga teenuseid, mis on seotud (Service1, elementi2 ja Service3) ning seda ise on välja lülitatud.

![Tasakaalustamiseks teenuseid koos][Image4]

Seega on võimalik, et Metric1 ebavõrdsuse, võivad põhjustada koopiad või eksemplarid, mis kuuluvad Service3 liigutamiseks. Tavaliselt selline liikumine on üsna piiratud, kuid võib sõltuvalt sellest, kuidas tasakaalustamata Metric1 täpselt suuremat sain ja mis tuli muuta klaster selleks, et parandada. Samuti võib öelda kindlalt mõõdikute 1, 2 või 3 ebavõrdsuse, põhjustab kunagi liikumise Service4 – oleks pole punkti alates teisaldada selle koopiad või eksemplarid, mis kuuluvad Service4 ümber saab tehke midagi mõjutada ülejäänud mõõdikute 1, 2 või 3.

Ressursihaldur kobar arvud automaatselt välja, millised teenused on seotud, kuna teenused on lisatud, eemaldatud, või oli nende argumendil konfiguratsiooni muuta – näiteks, kahe jookseb tasakaalustamiseks elementi2 vahel, võib olla ümber konfigureeritud eemaldamiseks Metric2. See piirid ahelas Service1 ja elementi2 vahel. Nüüd asemel kaks rühma teenuseid, on teil kolm.

![Tasakaalustamiseks teenuseid koos][Image5]

## <a name="next-steps"></a>Järgmised sammud
- Mõõdikud on, kuidas struktuuri kobar Service ressursi sõim haldab tarbimine ja võimsus klaster. Saate teada, vaadake lisateavet nende ja kuidas konfigureerida neid [selles artiklis](service-fabric-cluster-resource-manager-metrics.md)
- Liikumine hind on üks võimalus kobar ressursihaldur-signaal, et teatud teenused on liikuda, kui teised. Lisateavet liikumine maksumus, vaadake [käesoleva artikli](service-fabric-cluster-resource-manager-movement-cost.md)
- Ressursihaldur kobar on mitu pidurdab, mida saate konfigureerida piimapütt klaster aeglustada. Nad ei ole tavaliselt vajalikud, kuid kui teil on vaja need kohta leiate teavet teemast neid [siin](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
