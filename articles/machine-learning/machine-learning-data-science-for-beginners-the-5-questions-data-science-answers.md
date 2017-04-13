<properties
   pageTitle="5 andmed teadus küsimused - andmete teadus algajatele | Microsoft Azure'i"
   description="Saada andmed teadus tutvustuse andmete teadus algajatele viis lühivideoid, mis algavad 5 küsimused andmete teadus vastused."
   keywords="andmete teadus, andmete teadus algajatele, andmete teadus tegemiseks algajatele, küsimused, andmete teadus küsimused, andmete teadus video tüübid"
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

# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Andmete teadus algajatele video 1: The 5 küsimused, andmete teadus vastused

Sissejuhatus andmete teadus *andmete* teadus algajatele viis lühikeses videos toomine ülemise andmete teadlane. Neid videoid, on lihtne, kuid kasulik, kas olete huvitatud teed andmete teadus või andmete Aserbaidžaan töö käigus.

See esimene video on teave selle kohta, milliseid andmeid teadus saate vastata küsimustele. Kõigi võimaluste sarja, vaadake neid kõiki. [Avage loend videod](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-the-5-questions-data-science-answers]

## <a name="other-videos-in-this-series"></a>Selle sarja muud videod

*Andmete teadus algajatele* on tutvustuse andmete teadus võttes 25 minutit kokku. Vaadake neli Videod:

  * Video 1: 5 küsimused, andmete teadus vastused
  * Video 2: [kas teie andmed on valmis andmete teadus?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 SEK)*
  * Video 3: [Esitage küsimus saate vastata andmetega](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 SEK)*
  * Video 4: [Lihtsa mudeli vastust prognoosida](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 SEK)*
  * Video 5: [Kopeerida teiste inimeste tööd teha andmete teadus](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 SEK)*

## <a name="transcript-the-5-questions-data-science-answers"></a>Logifaili: 5 küsimused, andmete teadus vastused

Hei! Tere tulemast videosari *Andmete teadus algajatele*.

Andmete teadus võib hirmutada, nii, et tutvustada põhitõed ilma võrrandite või programmeerimine žargoon.

Selle esimese video me rääkida "5 küsimused andmete teadus vastused."

Andmete teadus kasutab numbrite ja nimedega (nimetatakse ka siltide või kategooriad) prognoosida vastused küsimustele.

See võib üllatus teile, kuid *on ainus viis küsimused selle andmete teadus vastused*:

  * See A ja B on?
  * See on imelik?
  * Kui suur- või -kui palju?
  * Kuidas see korraldada?
  * Mida teha järgmisena?

  Igat neile küsimustele on vastatud eraldi perekonnad masina õ võimalused, nimetatakse algoritmide kohta.


On kasulik mõelda algoritmi nimega retsept ja andmete koostisosad. Algoritmi kirjeldatakse, kuidas ühendamiseks ja segada andmed vastuse saamiseks. Arvutid sarnanevad on seguri. Teie eest kõige algoritmi tööd teha ja nad kiiresti.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Küsimus 1: Kas see A ja B? kasutab liigitamine algoritmide kohta

Alustame küsimus: see A ja B on?

![Liigitamine algoritmide kohta: see A ja B on?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-classification-algorithms.png)

See pere algoritmide nimetatakse kaks klassi liigitamine.

See on kasulik küsimusi, millel on kaks võimalikku vastust.

Näiteks:

  * Selle rehvi nurjub järgmise 1000 miili: Jah või ei?
  * Mis toob rohkem kliendid: $5 kupong või 25% allahindlust?

Samuti saate rephrased käesoleva teema küsimusele, lisada rohkem kui kaks võimalust: on see A ja B ja C või D, jne.?  Seda nimetatakse multiclass liigitamine ja see on kasulik, kui teil on mitu – või mitu tuhat – võimalikku vastust. Multiclass liigitamine valib tõenäoliselt üks.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Küsimus 2: Kas see imelik? kasutab normaalne tuvastamise algoritmide kohta

Saate vastata andmete teadus järgmine küsimus on: on see imelik? Selle küsimusele on vastatud nii perekonnad nimetatakse normaalne tuvastamise algoritmid.

![Normaalne tuvastamise algoritmide kohta: on see imelik?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-anomaly-detection-algorithms.png)


Kui teil on oma krediitkaardi andmed, olete juba kasu normaalne tuvastamise. Ettevõtte krediitkaardi analüüsib oma ostu mustrite nii, et nad saavad hoiatada teid võimalike pettuse. Mis on "imelik" kulude võib olla ostmise poodi, kus pole tavaliselt poest või osta ebatavaliselt kallis üksuse.

Käesoleva teema küsimusele võib olla kasulik palju võimalusi. Näiteks:

  * Kui teil on auto rõhu, võite leida: selle lugemine tavaline manomeeter on?
  * Kui te ei jälgi Interneti-ühendus, mida soovite leida: on tüüpilised teade Interneti kaudu?

Normaalne tuvastamise lipud ootamatute või ebatavalised sündmused ja käitumist. See annab vihjeid, kust otsida probleeme.



## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Küsimus 3: Palju? või kuidas palju? kasutab regressiooni algoritmide kohta

Seadme õ saate ka prognoosida palju kuidas vastata? või kuidas palju? Pere algoritmi, mis vastab sellele küsimusele nimetatakse regressiooni.

![Regressioonisirge algoritmide kohta: palju? või kuidas palju?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-regression-algorithms.png)


Regressioonisirge algoritmide teha arvväärtuste prognoose, näiteks:

  * Milline temperatuur on järgmine Teisipäev?  
  * Mis saab minu neljanda kvartali müük?

Need võimaldavad igale küsimusele küsib arvu.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Küsimus 4: See korraldamine? kasutab rühmitamise algoritmide kohta

Nüüd on kaks viimast küsimused natuke veel täpsemalt.

Vahel soovite mõista andmehulgas - struktuuri see korraldamine? Teil pole käesoleva teema küsimusele näiteid, mis juba teada tulemused.

On palju võimalusi Kammata välja andmete struktuuri. Üks lähenemine on rühmitamise. See eraldab andmete loomulikus "tükke," lihtsam tõlke. Rühmitamise, mis on üks õige vastus.

![Rühmitamise algoritmide kohta: kuidas see korraldada?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-clustering-algorithms.png)

Klastrite küsimused levinud näited:

  * Millised vaatajate nagu sama tüüpi Filmid?
  * Millist printerit mudelite nurjuda samamoodi?

Autor mõista, kuidas andmed on korraldatud, saate paremini mõista - ja prognoosida - käitumise ja sündmused.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Küsimus 5: Mida tuleks teha? kasutab tugevdamine Õppekeskuse algoritmide kohta

Viimane küsimus – mida teha nüüd? – kasutab algoritmide kohta nimega tugevdamine õ pere.

Kuidas aju rottidel ja inimestel vastata omandiga ja karistuse inspireeris tugevdamine õ. Nende algoritmide tulemuste õppida ja otsustada, missugused järgmine toiming.

Tavaliselt tugevdamine õ on hea sobib automatiseeritud, mida on vaja teha ohtralt small otsuseid ilma inimeste jaoks.

![Õppekeskuse algoritmide tugevdamine: mida teha järgmisena?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-reinforcement-learning-algorithms.png)

Küsimustele vastuste see on alati kohta, mida tuleks – tavaliselt masina või robot. Näited:

  * Kui ma olen temperatuuri kontrolli süsteem maja: temperatuur või jätke see, kui see on?  
  * Kui ma ise juhtimise auto: kollane hele, piduri või kiirendada?  
  * Robot vaakum: jätke tolmuimejaga või minge tagasi laadimise asub?

Tugevdamine õ algoritmide koguda andmeid, nagu need siirdumiseks õppimine prooviversiooni ja tõrge.

Nii, et see on õige - The 5 küsimused andmete teadus vastata.



## <a name="next-steps"></a>Järgmised sammud

  * [Proovige esimese andmete teadus katse masina õ Studio](machine-learning-create-experiment.md)
  * [Microsoft Azure'i hankida arvuti õ tutvustus](machine-learning-what-is-machine-learning.md)
