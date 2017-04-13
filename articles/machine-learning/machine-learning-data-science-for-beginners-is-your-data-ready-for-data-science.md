<properties
   pageTitle="Kas teie andmed on valmis andmete teadus? Andmete hindamine | Microsoft Azure'i"
   description="Siit saate teada, andmed on valmis andmete teadus 4 kriteeriumid. Andmete teadus algajatele video 2 on teid aidata peamine andmete hindamise näiteid."
   keywords="asjakohaste andmete andmete hindamiseks, ettevalmistamiseks, andmete kriteeriumid, andmete ettevalmistamine"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>


# <a name="is-your-data-ready-for-data-science"></a>Kas teie andmed on valmis andmete teadus?

## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Andmete teadus algajatele sarja

Saate teada, kuidas oma andmeid ja veenduge, et see vastab põhilised kriteeriumid on valmis andmete teadus hinnata.

Kõigi võimaluste sarja, vaadake neid kõiki. [Avage loend videod](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-is-your-data-ready-for-data-science]

## <a name="other-videos-in-this-series"></a>Selle sarja muud videod

*Andmete teadus algajatele* on viis lühikesi videoid vaadates andmete teadus tutvustuse.

  * Video 1: [5 küsimused andmete teadus vastused](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 SEK)*
  * Video 2: Kas teie andmed on valmis andmete teadus?
  * Video 3: [Esitage küsimus saate vastata andmetega](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 SEK)*
  * Video 4: [Prognoosida lihtsa mudeli vastust](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 SEK)*
  * Video 5: [Kopeerida teiste inimeste tööd teha andmete teadus](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 SEK)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Logifaili: Kas teie andmed on valmis andmete teadus?

Tere tulemast "On andmete valmis andmete teadus?" Teine video sarja *Andmete teadus algajatele*.  

Enne andmete teadus saab teile soovite vastuseid, on teil oleks anda sellele mõni kvaliteetne tooraine töötamiseks. Nii nagu pizza, parem tegemist alustada, seda ülevaatlikumaid lõpliku toote koostisosad.

## <a name="criteria-for-data"></a>Andmete kriteeriumid

Seega andmete teadus, puhul on mõned koostisosad, mida läheb vaja koondada.

Läheb vaja andmeid, mis on:

  * Oluline
  * Ühendatud
  * Täpne
  * Piisavalt töötada

## <a name="is-your-data-relevant"></a>Kas teie andmed on oluline?

Nii, et esimene komponent - läheb vaja andmeid, mis on oluline.

![Asjakohaste andmete vs oluline andmete - andmete hindamiseks](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-relevant-and-irrelevant-data.png)

Vaadake tabelis vasakul. Me täidetud seitse inimest väljaspool Boston lindid, mõõta oma vererõhumõõtja alkohol tase, Red Sox löömist keskmise oma viimase mängu ja hinna piima lähima mugavuse poes.

See on kõik täiesti õiged andmed. Ainult viga on, et see pole oluline. Need numbrid vahel puudub selge seos. Kui andsin praegune hind piima ja Red Sox löömist keskmise, on kuidagi pakud võib minu vererõhumõõtja alkohol sisu.

Nüüd vaadata tabeli paremal. Seekord me mõõta iga inimese keha mass ja arvestatud joogid nad on olnud arvu.  Iga rea arvud on nüüd omavahel seotud. Kui andsin mu keha mass ja Margaritas mul oli arv, teil hinnang veebisaidil minu vererõhumõõtja alkohol sisu.

## <a name="do-you-have-connected-data"></a>Kas on ühendatud andmeid?

Järgmise komponent on ühendatud andmeid.

![Ühendatud andmete vs lahti andmete - andmed kriteeriumid, andmete ettevalmistamine](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-connected-vs-disconnected-data.png)

Siin on mõned oluline andmete kvaliteedi hamburgerid: grill temperatuur, patty jämedus ja ajakirjade kohaliku reiting. Kuid Pange tähele lünki tabeli vasakul.

Enamik andmekogumi on puudu mõned väärtused. See on levinud auke, nagu see on ja on võimalust nende lahendamiseks. Aga kui on liiga palju puuduvad, andmete algab välja nagu Šveitsi juust.

Kui vaatate tabeli vasakul, on nii palju puuduvad andmed on seda raske grill temperatuur ja patty paksus vahelise seose jätmiseks tulla. See on näide lahti andmeid.

Tabeli paremal, kuigi täis ja täitke – näiteks ühendatud andmeid.

## <a name="is-your-data-accurate"></a>Kas teie andmed on täpne?

Kuvatakse järgmine läheb vaja on täpsuse. Siin on neli sihtkohtade, mille soovime tabas nooli.

![Täpsete andmete vs andmed – andmed kriteeriumid](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-inaccurate-vs-accurate-data.png)

Vaadake target paremas ülanurgas. Meil on tihedalt rühmitamise kohe ümber täpp. Muidugi, mis on täpne. Kummaline, andmete teadus keeles meie jõudluse selle all paremal target leitakse täpne.

Kui visandada noolte keskele, oleks näha, et see on väga lähedal täpp. Nooli on laiali kogu target, nii, et nad on ebatäpne lugeda, kuid need on keskmes täpp, nii, et nad on täpne lugeda.

Nüüd vaadata ülemise vasakpoolse Target (sihtkoht). Siin meie nooli tabas väga koos, tihedalt rühmitamine. Need on täpne, kuid need on ebatäpne, kuna keskele on viis välja täpp. Ja muidugi nooled soovitavas vasakus on ebatäpne ja ebatäpne. See archer peab rohkem tava.

## <a name="do-you-have-enough-data-to-work-with"></a>Kas teil on piisavalt töötamiseks andmeid?

Lõpuks komponent #4 - meil on piisavalt andmeid.

![Kas teil on piisavalt andmete analüüsimiseks? Andmete hindamine](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-barely-enough-data.png)

Iga andmepunkti tabelis on värvimine pintsli kriips mõelda. Kui teil on vaid mõned neist, värvimine võivad olla päris udune – see on keeruline aru saada, mis see on.

Kui lisate mõne veel pintsli tinditõmmete, siis teie värvimine hakkab saada oli hea.

Kui teil on piisavalt vaevu tinditõmmete, näete lihtsalt piisavalt lai otsuste mõned. On kuskil võiksite külastada? Tundub ergas, mis näeb välja umbes puhta vee – Jah, mis on koht, kust ma puhkusele.

Kui lisate rohkem andmeid, pilt muutub selgemaks ja saate teha üksikasjalikumat otsuseid. Nüüd saate vaatame kolme hotellist vasakul panga. Teate, mulle meeldib ühe esiplaanil arhitektuuri funktsioone. Saate peatada, turvalist.

Andmed, mis on oluline, ühendatud, õiged, ja piisavalt, meil on kõik koostisosad tuleb teha mõned kõrge kvaliteediga andmete teadus.

Kindlasti muude neli videotest *Andmete teadus algajatele* : Microsoft Azure seadme õ.




## <a name="next-steps"></a>Järgmised sammud

  * [Proovige esimese andmete teadus katse masina õ Studio](machine-learning-create-experiment.md)
  * [Microsoft Azure'i hankida arvuti õ tutvustus](machine-learning-what-is-machine-learning.md)
