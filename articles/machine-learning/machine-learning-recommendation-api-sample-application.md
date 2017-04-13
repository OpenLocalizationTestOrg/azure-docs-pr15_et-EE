<properties 
    pageTitle="Levinud toimingud kohapeal õ soovitused API | Microsoft Azure'i" 
    description="Azure'i ML soovitus valimi rakendus" 
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


# <a name="recommendations-api-sample-application-walkthrough"></a>Soovitused API valimi rakenduse kiirtutvustus

>[AZURE.NOTE]Soovitused API kognitiivse teenuse peaks kasutuselevõtt, selle asemel, et see versioon. Soovitused kognitiivse teenus on asendades selle teenuse ja seal arendatakse uusi funktsioone. See on uue võimalused nagu partiide tugi, parem API Explorer, cleaner API Surface'i, ühtsema registreerimine/arveldamine kogemus jne.
> Lisateavet [uue kognitiivse teenuse migreerimine](http://aka.ms/recomigrate)

##<a name="purpose"></a>Otstarve

See dokument näitab kasutamist Azure'i masina õ soovituste API on [valimi rakenduse](https://code.msdn.microsoft.com/Recommendations-144df403)kaudu.

See rakendus ei ole mõeldud kaasata täisfunktsionaalsuse ta ei kasuta kõiki API-d. See näitab mõningaid levinud toiminguid teha, kui soovite esmalt arvutisse õ soovitusteenuse mängida. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Seadme õ soovitusteenuse tutvustus

Soovitused masina õ soovitusteenuse kaudu on lubatud, kui koostate soovitus mudeli põhjal järgmised andmed:

* Hoidla üksust, mida soovite soovitame ka kataloogiga
* Ühe kasutaja või seansi (selle saab omandada aja jooksul kaudu andmeid tuua, mitte valimi rakenduse osana) kasutamist esindavad andmed

Pärast mudeli soovitus on loodud, saate selle prognoosida üksused, mida kasutaja võib olla huvitatud, vastavalt üksuste (või ühe üksuse) kasutaja valib.

Eelmise stsenaariumi lubamiseks tehke arvuti õ soovitusteenuse järgmist:

* Andmemudeli loomine: see on loogiline container, mis hoiab andmeid (kataloogi ja kasutus) ja ennustamine geoloogilises. Mudeli iga container tuvastatakse kaudu ainu-ID, mis on eraldatud loomisel. See ID nimetatakse mudeli ID ja kasutab seda funktsiooni API-de kõige. 
* Laadi üles, et kataloogi: mudeli container loomisel saate seostada selle kataloogiga.

**Märkus**: mudeli loomise ja üleslaadimise kataloogiga tavaliselt tehakse üks kord elutsükli mudel.

* Laadige kasutus: kasutusandmete lisatakse mudeli ümbrises.
* Luua soovitus mudel: kui teil on piisavalt andmeid, saate koostada soovitus mudel. See toiming kasutab ülemise masina õ algoritmide soovitus andmemudeli loomine. Iga koostamine on seostatud kordumatu ID-ga. Peate arvestuse pidamiseks ID kuna on vaja mõned API-de funktsioonide jaoks.
* Koosteüksuse jälgimiseks: soovitus mudeli koostamine on asünkroonne toiming ja võib kuluda mitu minutit mitu tundi, olenevalt andmete (kataloogi ja kasutus) ja Koosta parameetrid. Seetõttu tuleb teil jälgida koostamine. Mudeli soovitus luuakse ainult siis, kui selle seotud koostamine on lõpule jõudnud edukalt.
* (Valikuline) Valige mõni aktiivne soovitus mudeli koostamine: See toiming on vajalik, kui teil on rohkem kui üks soovitus mudel oma mudeli ümbrises ainult. Mis tahes taotluse saada soovitusi ilma aktiivse soovitus mudelit, mis näitab, on vaikimisi aktiivne koostamine süsteem automaatselt ümber. 

**Märkus**: aktiivne soovitus mudelit, mis on valmis ja see on ehitatud tootmise töökoormus. See erineb aktiivne soovitus mudel, mis jääb testi nagu keskkonnas (mõnikord nimetatakse lavastus).

* Hangi soovitusi: olete soovitus mudeli, võite käivitada soovitused ühe üksuse või loendi üksused. 

Tavaliselt kutsute saada soovitus teatud aja jooksul. Selle aja jooksul ümber suunata kasutusandmete masina õ soovitus süsteemi, mis lisab andmed määratud mudeli ümbrises. Kui teil on piisavalt kasutusandmete, saate luua uue soovitus mudelit, mis sisaldab täiendavat kasutusandmete. 

##<a name="prerequisites"></a>Eeltingimused

* Visual Studio 2013
* Interneti-ühendus 
* Tellimuse soovitused API (https://datamarket.azure.com/dataset/amla/recommendations).

##<a name="azure-machine-learning-sample-app-solution"></a>Azure'i masina õ valimi rakenduse lahendus

See lahendus sisaldab lähtekoodi, näide ja kataloogi faili suuniseid allalaadimine pakettide koostamine jaoks vajalike.

##<a name="the-apis-used"></a>API-d kasutatakse

Rakendus kasutab masina õ soovitus funktsioonid saadaval API-de alamhulk kaudu. Järgmised API-d on näidatud rakendus:

* Andmemudeli loomine: loogilise container ootelepanekuks andmed ja soovitused mudelite loomine. Tähistab mudeli nimi ja te ei saa luua rohkem kui ühe mudeli sama nimega.
* Kataloogi faili üles laadida: kaudu üles laadida kataloogi andmed.
* Kasutus faili üles laadida: kaudu üles laadida kasutusandmete.
* Koosta käivitamine: mudeli soovitus loomiseks kasutada.
* Koosta täitmise jälgimine: abil saate jälgida olekut soovitus mudeli koostamine.
* Valige soovitused ehitatud mudel: saate näidata, millist soovitus mudeli jaoks teatud mudeli container vaikimisi kasutada. See toiming on vaja ainult siis, kui teil on rohkem kui üks soovitus mudel ja soovite aktiveerida aktiivne koostamine aktiivne soovitus mudel.
* Saada soovitus: soovitatav üksusi vastavalt antud ühe üksuse või üksuste kogum toomiseks kasutada. 

Täielikku kirjeldust soovitud API-de kohta leiate teemast Microsoft Azure'i turuplatsilt seotud dokumendid. 

**Märkus**: mudeli võib olla mitu järgud aja jooksul (korraga). Sama või värskendatud kataloogi ja täiendavad kasutusandmete luuakse iga koostamine.

## <a name="common-pitfalls"></a>Levinud vigu

* Sisestage oma kasutajanimi ja oma Microsoft Azure'i turuplatsi esmane konto võti valimi rakenduse käivitamiseks peate.
* Valimi rakendus töötab järjest nurjub. Rakenduse voogu sisaldab loomise, üles, koostamise jälgida ja soovituste hankimine eelmääratletud mudeli; Seetõttu see ei õnnestu järjestikust täitmise kohta, kui muudate vahel manamine mudeli nimi.
* Soovitused võib tagastada ilma andmeteta. Valimi rakendus kasutab väga väike faili kataloogi ja kasutamist. Mõned üksused kataloogist on seega pole soovitatav üksusi.

## <a name="disclaimer"></a>Lahtiütlemine
Valimi rakendus ei ole tootmiskeskkonnas käivitamiseks mõeldud. Kataloogis olevad andmed on väga väike ja see ei paku mõtestatud soovitus mudeli. Andmed on nagu tutvustamise. 
 
