<properties 
    pageTitle="Häälestada ja kasutada seadme õ soovitusi API | Microsoft Azure'i" 
    description="Microsofti soovitused API ehitatud Azure seadme õ KKK" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Häälestamise ja kasutamise masina õ soovitused API KKK


**Mis on soovitused?**

>[AZURE.NOTE]Soovitused API kognitiivse teenuse peaks kasutuselevõtt, selle asemel, et see versioon. Soovitused kognitiivse teenus on asendades selle teenuse ja seal arendatakse uusi funktsioone. See on uue võimalused nagu partiide tugi, parem API Explorer, cleaner API Surface'i, ühtsema registreerimine/arveldamine kasutuskogemuse jne.
> Lisateavet [uue kognitiivse teenuse migreerimine](http://aka.ms/recomigrate)

Ettevõtted ja asutused toetuvad soovitusi rist-sell ja üles-sell toodete ja teenuste oma klientidele, soovitusi Azure seadme õ pakub iseteenindusliku soovitusi engine. See on koostööpõhise filtreerimine rakendamist, mis kasutab maatriksi Faktoriseerimine oma core algoritmi. Rakenduste arendajatele pääsevad soovituste abil REST API-d. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Mida saab teha soovitustega?**

SOOVITUSED võtab sisendit üksuse või üksuste kogumi ja tagastab loendi vastavaid soovitusi. Näide: mõne online edasimüüjalt Klient klõpsab toote. Online edasimüüjalt saadab selle toote sisendina soovitusi, saab loendi toodetest, tagasi ja otsustab, mis nende toodete kuvatakse kliendile. Soovite kasutada soovitusi optimeerida oma poe või isegi teie sees teavitada müügi osakonnas või kõne keskele.

**Kas on mingeid piiranguid kasutus?**

Soovitused on järgmised piirangud kasutus:
* Suurim lubatud arv tellimuse kohta mudeleid: 10
* Üksused, mille kataloogiga mahub arvu ülempiir: 100 000
* Kasutus punkte, mis on maksimaalne arv on ~ 5 000 000. Vanimast kustutatakse uusi faile üles laadida või teatatud.
* Andmed, mida saab saata e-posti (nt kataloogi andmete importimine, andmete importimine kasutus) maksimaalne maht on 200 MB
* Soovitused mudeli koostamine, mis pole aktiivne sekundis (TPS) tehingute arv on ~ 2 TPS. Soovitused mudeli koostamine, mis on aktiivne, võib olla kuni 20 TPS.

##<a name="purchase-and-billing"></a>Ostu ja arveldamine 


**Kui palju maksab soovitused hind ajal käivitada?**

Soovitused on tellimispõhine teenus. Laadimise põhineb tehingute kuus. Saate kontrollida pakkumisega lehe (https://datamarket.azure.com/dataset/amla/recommendations) rakenduses Microsoft Azure'i turuplatsi hindade teavet.

**Kas soovitusi, millel on mis tahes kulusid jälgida ja salvestada kasutaja tegevuste minu jaoks?**

Pole praegu.

**Kas soovitusi on tasuta prooviversioon?**

On tasuta rada, mis on piiratud 10 000 tehingud kuus.

**Kui ma toimub soovitusi?**

Tasulisele tellimusele on mis tahes tellimus, mille puhul on kuutasu. Kui ostate tasulisele tellimusele, teilt tasu kohe esimese kuu kasutamiseks. Teil tuleb maksta summa, mis on seotud pakkumine klõpsake lehel tellimuse (lisanduvad maksud). Selle kuu eest tehakse iga kuu algse ostu kalendri kuupäeva kuni tellimuse tühistamine. 

**Kuidas täiendada versiooniks kõrgema taseme teenuse?**

Saate osta või tellimuse pakkumise lehelt värskendada Microsoft Azure'i turuplatsi lehele (https://datamarket.azure.com/dataset/amla/recommendations).

Kui täiendate tellimus:

* Uue tellimuse ei lisata tehingud, mis on ülejäänud oma vana tellimust. 
* Maksate täielik hind uude tellimusse, isegi kui teil on kasutamata tehingute vana tellimust.

Protsessi tellimust uuendada:

* Nevigate pakkumine lehele (https://datamarket.azure.com/dataset/amla/recommendations).
* Kui te pole veel sisse logitud, logige sisse kuvatakse Marketplace'ist.
* Parempoolsel paanil on loetletud kõik saadaolevad lepingud. Klõpsake raadionuppu jaoks leping, millele soovite uuendada.
* Kui soovite täiendada, klõpsake nuppu **OK**. Kui te ei soovi uuendada, klõpsake nuppu **Loobu**.

**Oluliste** Lugege dialoogiboksi hoolikalt enne versioonitäiendust, sest seal on arveldamine ja kasutamine.

**Millal minu tellimus soovitused lõpeb?**

Tellimuse lõpetamine juhul, kui saate tühistada. Kui soovite oma tellimust tühistama, vaadake järgmisi juhiseid.

**Kuidas teha soovitused tellimust tühistada?**

Tellimuse tühistamiseks tehke järgmist. Kui teie praegune tellimus on tasulisele tellimusele, tellimuse jätkub tegelikult praeguse arveldusperioodi lõpuni. Kui teil on vaja tühistamise oleks tõhus kohe, võtke meiega ühendust [Microsofti](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn)toega.

**Märkus** Ei tagastata kui tühistate enne lõppu arveldusperioodi eest või kasutamata tehingute puhul arveldusperioodi eest.

* Pakkumine lehele liikumiseks (https://datamarket.azure.com/dataset/amla/recommendations).
* Kui te pole veel sisse logitud, logige sisse kuvatakse Marketplace'ist.
* Klõpsake paremas servas andmekomplekti nime ja oleku **tühistada** . Saate selle tellimuse praeguse arveldusperioodi lõpuni või oma tehingu piirangu (kumb on esimene).

Kui soovite kohe, nii et saate osta uus tellimus oma tellimuse tühistama, failide lisamine Piletite [Microsofti](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn)toega.

##<a name="getting-started-with-recommendations"></a>Soovitused töötamise alustamine

**Soovitused on minu jaoks?** 

Soovitused masina õppe on ettevõtted ja asutused toetuvad soovitused rist-müüa ja üles-sell toodete või teenuste oma klientidele. Kui teil on kliendi-ühendusega veebisaidi, müügi jõusta, sees müügi või kõnekeskuse, ja kui üle mõned kümneid toodete või teenuste kataloogiga, teie Loosung võib olla kasu soovituste abil. 

Soovitused katsetamiset eesmärk on üsna lihtne. Praegune API-põhine versioon nõuab lihtsa programmeerimise oskusi. Kui vajate abi, võtke ühendust tarnija, kes töötasid veebisaidi. Kui teil on sisemine IT-osakond või mõne ettevõttesiseseid arendaja, peaks neid võimalik saada soovitusi, et teile abiks olla. 

**Mis on eeltingimused häälestamise soovitused?**

Soovitusi eeldab, et kasutaja Valikud samamoodi nagu see on seotud teie kataloogi. Kui te ei t on sellist Logi ja teil on klientide suunatud veebisait, soovitusi saate koguda kasutaja tegevuste jaoks. 

Soovitused nõuab ka oma toodete või teenuste kataloogiga. Kui te ei t on kataloogi, soovitusi saate kasutada kasutusandmete tegelik klient ja ajama kataloogiga. On ka mitte kaudset kataloogi ei sisalda üksused, mis ei tuvastatud kasutaja tehinguid osana.

**Kuidas häälestada soovitused esimest korda?**

Pärast [tellimise] (https://datamarket.azure.com/dataset/amla/recommendations) soovitusi, kasutage dokumentatsiooni [Azure seadme õ soovitusi Lühijuhend](machine-learning-recommendation-api-quick-start-guide.md) teenuse häälestamiseks.

**Kust leian API dokumentatsiooni?** 

Dokumentatsiooni on [Azure seadme õ soovitused Lühijuhend](machine-learning-recommendation-api-quick-start-guide.md).

**Millised võimalused on on kataloogi ja kasutamine andmete üleslaadimiseks soovitusi?**

On teil kaks võimalust üleslaadimise andmete kataloogi ja kasutus: saate eksportida andmed CRM-süsteemi või muude logid ja üleslaadimine soovitused või silte saate lisada oma veebisaidile, mis võimaldab jälgida kasutaja tegevuste. Kui kasutate viimase meetod, talletatud andmed Azure.

##<a name="maintenance-and-support"></a>Hooldus ja tugiteenused

**Kui suur võib olla minu andmehulga?**

Iga andmehulga võivad sisaldada kuni 100 000 kataloogiüksuste ja üles kasutusandmete 2048 MB.
Lisaks tellimus võib sisaldada kuni 10 andmekogumi (mudelid).

**Kui saan soovitused tehnilist tuge?**

Tehniline tugi on saadaval saidil [Microsoft Azure'i tugi](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) .

**Kust leida kasutustingimused?**

[Microsoft Azure'i masina Õppekeskuse soovitused API-teenuse kasutustingimused](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
