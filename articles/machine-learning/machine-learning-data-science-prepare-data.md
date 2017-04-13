<properties
    pageTitle="Täiustatud andmete valmistuda tööülesannete masina õ | Microsoft Azure'i"
    description="Eelnevalt protsess ja selge andmed seadme õ valmistuda."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Täiustatud andmete valmistuda tööülesannete masina õpetused

Eeltöötlus ja andmete puhastamine on tähtsad tööülesanded, mis tavaliselt tuleb teha enne andmekomplekti saab kasutada tõhusalt masina õ. Lähteandmed on sageli lärmakas ja usaldusväärsed ja võib olla puudu väärtused. Selliste andmete modelleerimiseks abil saate eksitavat tulemusi. Järgmised toimingud on osa meeskonnatöö andmete teadus protsess (TDSP) ja järgige tavaliselt mille esialgne uurimine kasutada ja kavandamine eelse töötlemise nõutav Andmekomplekt. TDSP protsessi üksikasjalikud juhised leiate teemast [Meeskonnatöö andmete teadus protsessi](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)kirjeldatud juhised.

Eelse töötlemine ja puhastamine ülesanded, nagu andmete uurimine ülesanne, saab läbi mitmesuguseid keskkonnas, nagu SQL-i või taru või Azure seadme õ Studio ja erinevate tööriistad ja tekst, nt R või Python, sõltuvalt teie andmete talletuskoht ja selle vormingu. TDSP on iteratiivne laadi, saate need toimingud toimuda erinevaid toiminguid töövoo käigus.

Selles artiklis tutvustatakse erinevaid andmete töötlemise põhimõtet ja tööülesandeid, mida saate teha enne või pärast söömisega andmed üheks Azure seadme õ.

Näide andmete uurimine ja eelse töötlemise teha Azure seadme õ studio sees, vt [eeltöötlus andmete Azure seadme õ Studios](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) video.


## <a name="why-pre-process-and-clean-data"></a>Miks eelnevalt töötlemine ja puhastada andmeid?

Tõeline kogutud erinevatest allikatest ja protsesside ja see võib sisaldada eiramise või kompromisse kvaliteedi andmekomplekti andmed. Tüüpilised andmete kvaliteedi probleeme tekkida on:

* **Mittetäielik**: andmete puudub atribuudid või mis sisaldab puuduvate väärtuste.
* **Noisy**: andmed sisaldavad vigaste kirjete või võõrväärtuste.
* **Vastuolus**: andmed sisaldavad vastuoluliste kirjete või erinevused.

Andmete kvaliteedi on nõutav kvaliteet prognoosmudelite. Vältige "prügi, prügi välja" ja andmete kvaliteedi parandamiseks ja seetõttu modelleerimise jõudluse, tuleb olemasolevates andmete seisund Kuva kohapeal andmete probleemid alguses ja otsustama vastavate andmete töötlemise ja puhastamise juhiseid.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Mis on mõned tüüpilised andmete seisund ekraanid, mis töötavad?

Me saate kontrollida üldine andmete kvaliteedi kontrollimiseks.

* **Kirjete**arvu.
* Arv **(või **funktsioonide**)** .
* Atribuut **andmetüübid** (nominal, järgarvude või pidev).
* **Puuduvate väärtuste**arv.
* **Hästi-formedness** andmed.
    * Kui andmed on TSV või CSV-, kontrollige, et selle veeru eraldajad rea eraldajad alati õigesti eraldi veerud ja read.
    * Kui andmed on HTML-i või XML-vormingus, kontrollige kas andmed on regulaarne vastavalt nende vastavate standarditele.
    * Sõelumine võib osutuda vajalikuks struktuurset teavet ekstraktida poolstruktuur või struktureerimata andmed.
* **Tabelites sisalduvas andmekirjeid**. Märkige ruut lubatud väärtuste vahemikku. Näiteks kui andmed sisaldavad kool GPA, kontrollige, kas GPA on määratud vahemiku öelda 0 ~ 4.

Kui olete leidnud probleemid andmetega, **protsessi etapid** on vaja sageli toob puhastus puuduvate väärtuste, andmete normaliseerimine, discretization, eemaldage ja/või asendamine manustatud märgid, mis võivad mõjutada andmete joondust töötlemine teksti, kombineeritud andmetüüpide ühist väljad ja teistele.

**Azure'i masina õ tarbib regulaarne tabelina esitatud andmed**.  Kui andmed on juba tabelina, saate enne töötlemise teha otse Azure seadme õ masina õ Studios.  Kui andmed ei ole tabelina, XML, on öelda sõelumine võib olla vaja teisendada andmed tabelina.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Millised on mõned peamised tööülesanded eelse töötlemise?

* **Andmete puhastamiseks**: sisestage või puuduvate väärtuste, tuvastada ja eemaldada lärmakas andmed ja võõrväärtuste.
* **Andmete teisendus**: mõõtmed ja müra vähendamiseks andmete normaliseerimine.
* **Andmete vähendamise**: Kuulake andmekirjeid või lihtsam andmete käitlemine atribuute.
* **Andmete discretization**: pidev atribuute teisendamine kategooriline atribuute kasutamise hõlbustamiseks teatud seadme õ meetoditega.
* **Teksti puhastamine**: eemaldamine manustatud märgid, mis võivad põhjustada andmete vastuolud, jaoks nt manustatud vahekaartide tabeleraldusega andmefaili manustatud uued read, mis võivad murda andmed jne.

Järgmistes jaotistes detail mõned töötlemine järgmiselt.

## <a name="how-to-deal-with-missing-values"></a>Kuidas lahendada puuduvate väärtuste?

Puuduvate väärtuste tegelemiseks, on mõistlik esmalt puuduvate väärtuste paremini hakkama põhjus probleemi tuvastamiseks. Tüüpilised puuduvad väärtus käsitsemise meetodid on:

* **Kustutamise**: kirjetega puuduvate väärtuste eemaldamine
* **Näiva asendus**: puuduvate väärtuste asendamine fiktiivne väärtus: nt, _tundmatutelt_ kategooriline või 0 jaoks arvväärtusi.
* **Tähendab asendus**: kui puuduvad andmed pole arvulised, asendage puuduvate väärtuste keskmine.
* **Sagedased asendus**: kui puuduvad andmed on kategooriline, kõige sagedamini üksusega puuduvate väärtuste asendamine
* **Regressioonisirge asendus**: puuduvate väärtuste asendamine väärtustega taandus regressiooni meetodi abil.  

## <a name="how-to-normalize-data"></a>Kuidas andmete normaliseerimine?

Andmete normaliseerimine skaala uuesti arvväärtusi määratud vahemikule. Populaarsed andmete normaliseerimine meetodid on järgmised.

* **Min Max normaliseerimine**: lineaarses muuta vahemiku andmed, teatavad, et vahemikus 0 kuni 1, kus on mastaabitud min väärtus 0 ja max väärtus 1.
* **Z-Keskmine normaliseerimine**: andmete põhjal keskväärtuse ja standardhälbega suurust: jagatakse vahe andmed ja keskväärtuse standardhälbe.
* **Decimal skaleerimist**: mastaapimiseks, teisaldades selle atribuudi väärtus komast andmed.  

## <a name="how-to-discretize-data"></a>Kuidas discretize andmeid?

Andmeid saab discretized teisendades pidev väärtused nominal atribuute või mitte. On mõned viise.

* **Võrdse laiusega Binning**: jagage atribuudi kõigi võimalike väärtuste vahemiku N rühma sama suuruse ja määrata väärtused, mis jäävad aluse numbriga prügikast.
* **Võrdsete-kõrgus Binning**: jagage atribuudi kõigi võimalike väärtuste vahemiku N rühma, igas sama eksemplaride arv ja seejärel määrata väärtused, mis jäävad aluse numbriga prügikast.  

## <a name="how-to-reduce-data"></a>Kuidas vähendada andmed?

On erinevad meetodid lihtsam andmete töötlemise andmete mahu vähendamiseks. Sõltuvalt andmete suurus ja domeeni, saate rakendada järgmistest viisidest:

* **Kirje valimite**: Kuulake andmete kirjete ja valida ainult andmed esindavad alamhulk.
* **Atribuut valimite**: valige kõige olulisem atribuutide alamhulka andmed.  
* **Koondamine**: andmeid jagada sõprade ja talletada iga rühma arvud. Näiteks saate igapäevane tulu numbrid restorani ahelas viimase 20 aasta jooksul koondada igakuine tulu andmete mahu vähendamiseks.  

## <a name="how-to-clean-text-data"></a>Kuidas puhastada tekstandmeid?

**Tekstiväljad tabelina esitatud andmed** võivad sisaldada märgid, mis mõjutavad veergude joondus ja/või kirje piirmäärad. Nt manustatud vahekaartide tabeleraldusega failiga põhjuse veeru vastuolud ja manustatud uue rea märgid kirje ridade murdmiseks. Teabe kaotsimineku tahtmatu kasutuselevõtmine Veebikanali, nt nullide ja võib-olla ka mõjutavad teksti liigendamine viib valest teksti kodeering töötlemine teksti kirjutamist/lugemise ajal. Teksti väljade jaoks nõutav joondus ja/või struktureeritud ekstrakti andmed struktureerimata või poolstruktuur tekstandmeid puhastamisel võidakse ettevaatlik sõelumine ja redigeerimine.

**Andmete uurimine** pakub andmetest ennetähtaegse vaadet. Andmete küsimusi saab selle toimingu käigus katmata ja vastavate meetodite saab rakendada nende probleemide lahendamine.  See on oluline esitada küsimusi, nt mis on probleemi allikas ja kuidas probleemi võib olla kasutusele võetud. See aitab teil otsustada, andmete töötlemise toiminguid, mis on vaja nende lahendamiseks ette võtta. Ülevaateid ühte kavas tulenevad andmete tüüpi saate kasutada ka tähtsuse töötlemise peegeldav.

## <a name="references"></a>Viited

>*Andmete hankimiseks: kontseptsioonide ja tehnoloogiate*, kolmas väljaanne, Morgan Kaufmann 2011 Jiawei Han, Micheline Kamber ja Jian Pei
