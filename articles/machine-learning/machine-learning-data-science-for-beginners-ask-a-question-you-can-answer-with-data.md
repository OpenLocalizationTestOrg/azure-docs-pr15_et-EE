<properties
   pageTitle="Postitage oma küsimus, saate vastata andmetega - koostada küsimused | Microsoft Azure'i"
   description="Saate teada, kuidas kujundada andmeid teadus küsimus andmete teadus algajatele video 3. Sisaldab võrdlus liigitamine ja regressiooni küsimused."
   keywords="andmete teadus küsimusi, koostada küsimusi, regressiooni küsimusi, liigitamine küsimusi, terav küsimus"
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

# <a name="ask-a-question-you-can-answer-with-data"></a>Saate vastata andmetega küsimuse esitamine

## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Andmete teadus algajatele sarja

Saate teada, kuidas kujundada andmeid teadus küsimus andmete teadus algajatele video 3. See video sisaldab võrdlus küsimused liigitamine ja regressiooni algoritmide kohta.

Kõigi võimaluste sarja, vaadake neid kõiki. [Avage loend videod](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>Selle sarja muud videod

*Andmete teadus algajatele* on viis lühikesi videoid vaadates andmete teadus tutvustuse.

  * Video 1: [5 küsimused andmete teadus vastused](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 SEK)*
  * Video 2: [kas teie andmed on valmis andmete teadus?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 SEK)*
  * Video 3: Küsida, saate vastata andmetega
  * Video 4: [Prognoosida lihtsa mudeli vastust](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 SEK)*
  * Video 5: [Kopeerida teiste inimeste tööd teha andmete teadus](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 SEK)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Logifaili: Küsida, saate vastata andmetega

Tere tulemast kolmanda video sarja "Andmete teadus algajatele."  

Selline, kuvatakse mõned näpunäited kujundamiseks saate vastata andmetega küsimus.

Lisateavet välja selle video, võite saada, kui vaatate esmalt kaks varem videod selle sarja: "5 küsimused andmete teadus saab vastata" ja "On teie andmed on valmis andmete teadus?"

## <a name="ask-a-sharp-question"></a>Terav küsimuse esitamine

Oleme rääkinud kuidas andmed teadus on protsessi kasutades (nimetatakse ka kategooriate ja siltide) nimed ja numbrid prognoosida küsimuse vastuse. Kuid seda ei saa lihtsalt küsimusi; See peab olema mõne *terav küsimus.*

Ebamäärane küsimus pole vastata nimi või arvu. Peate terav küsimus.

Oletage, et olete leidnud maagiline lamp koos džinn, kes ausalt vastata saate küsida küsimusi. Kuid see on vallatu džinn ja ta proovida muuta oma vastus ebamäärane ja selgem, kui ta ei pääse. Soovite kinnitada teda nii õhutihedad, et ta ei saa aidata, kuid kindlaks teha, kui soovite teada küsimus.

Kui te ei küsi ebamäärane, nagu "Mis saab minu börsi juhtub?", "hind muudab" genie võivad vastata. Mis on tõesed vastust, kuid see on väga kasulik.

Kuid kui teil on küsida terav küsimus, näiteks "Mida minu stock müügihind saab järgmise nädala?", genie ei saa aidata, kuid teile kindla vastata ja prognoosida müügihind.

## <a name="examples-of-your-answer-target-data"></a>Näiteid vastust oma küsimusele: sihtrakendus

Kui saate koostada oma küsimus, siis kontrollige, kas teil on vastus näited andmete.

Kui meie küsimus on "mis on minu stock müügihind järgmisel nädalal?" seejärel meil veenduge, et meie andmed sisaldavad aktsia hind ajalugu.

Kui meie küsimus on "mis auto minu laevastiku saab nurjumise esmalt?" seejärel meil veenduge, et andmed sisaldab teavet eelmise ebaõnnestumist.

![Target (sihtkoht) andmed – vastust oma küsimusele näited. Tutvumist andmete teadus küsimus.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

Nendes näidetes vastused nimetatakse siht. Siht on, mida me proovimise prognoosida kohta tulevaste andmepunkte, kas see on kategooria või arvu.

Kui teil pole target andmeid, peate saaksin. Te ei saa ilma selleta oma küsimusele vastata.

## <a name="reformulate-your-question"></a>Sõnastada oma küsimus

Mõnikord võib otsustasingi saada rohkem kasu vastust oma küsimusele.

"Kas see andmete punkti A ja B?" küsimus prognoosib kategooria (või nimi või sildi) midagi. Vastata, kasutame *liigitamine algoritmi*.

Küsimus "Palju?" või "Mitu?" prognoosib summa. Sellele vastata kasutame *regressiooni algoritmi*.

Et näha, kuidas me neid muuta, Heitkem pilk küsimus, "mis uudisloo on kõige huvitavamaks see lugeja?" Ta küsib prognoosida ja ühe valiku palju võimalusi – Teisisõnu "On see A ja B ja C või D?" - ja kasutaks liigitamine algoritmi.

Kuid see võib lihtsam vastata, kui te seda otsustasingi "Kuidas huvitav on selles loendis selle Reader iga lugu?" Nüüd saate anda iga artiklis numbrilise ja seejärel on lihtne tuvastamine artiklis kõrgeim hinded. See on lisamine rakendusse regressiooni küsimus küsimus liigitamine põhilahenduste või palju?

![Sõnastada oma küsimus. Liigitamine küsimus vs regressiooni küsimus.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

Kuidas palute küsimus on aimu, milline algoritmile saab teile vastata.

Leida teatud peredele algoritmide - nagu need on meie uudiste lugu näites - lähedalt seotud. Saate sõnastada kasutada algoritmi, mis annab teile kõige enam kasu vastust oma küsimusele.

Kuid kõige olulisem, paluge see terav küsimus - küsimus, et saate vastata andmetega. Ja veenduge, et teil on õiged andmed sellele vastata.

Oleme rääkinud mõned põhitõed küsib küsimuse saate vastata andmetega.

Kindlasti "Andmete teadus algajatele" Microsoft Azure'i masina õppe muude videotest.


## <a name="next-steps"></a>Järgmised sammud

  * [Proovige esimese andmete teadus katse masina õ Studio](machine-learning-create-experiment.md)
  * [Microsoft Azure'i hankida arvuti õ tutvustus](machine-learning-what-is-machine-learning.md)
