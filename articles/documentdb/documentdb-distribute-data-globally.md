<properties
   pageTitle="Andmete globaalselt DocumentDB levitamine | Microsoft Azure'i"
   description="Teavet planeedi ulatusega geo-dispersioonanalüüs, Tõrkesiirde ja andmete taastamine globaalne andmebaaside: Azure'i DocumentDB täielikult hallatud NoSQL andmebaasi teenuse abil."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Andmete globaalselt DocumentDB levitamine

> [AZURE.NOTE] Globaalse jaotuse andmebaase DocumentDB on üldiselt kättesaadav ja mis tahes vastloodud DocumentDB kontode jaoks automaatselt lubatud. Töötame kõigi olemasolevate kontode globaalne jagamise lubamiseks, kuid vahepeal kui soovite, et teie kontol lubatud levitamist palun [pöörduge klienditoe poole](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ja me tuleb lubada teile kohe.

Azure'i DocumentDB on mõeldud asjade rakenduste koosneb miljonite globaalselt jaotatud seadmed ja skaala rakendusi, mis pakuvad väga tundlik funktsioone kasutajatele kogu maailmas vajadustele. Andmebaasi tarvilikud esinema ülesanne saavutada madal latentsus Accessi rakenduse andmetega mitme geograafiliste piirkondade täpselt määratletud andmete järjepidevus ja kättesaadavus garantiid. Globaalselt hajutatud andmebaasi süsteemil DocumentDB lihtsustab globaalne andmete jaotumine pakkudes täielikult hallatud, mitme piirkond andmebaasi kontod, mis pakuvad Eemalda kompromissidega järjepidevuse, kättesaadavuse ja jõudluse tagamiseks kõik koos vastavate garantiid. DocumentDB andmebaasi kontod pakutakse kõrge-saadavus, üks number ms latentsused, mitme [täpselt määratletud järjepidevuse tasemed] [consistency], läbipaistev piirkondliku Tõrkesiirde koos mitme sihitamise API-d ja võimalus elastically skaala ja kogu maailmas. 

  
## <a name="configuring-multi-region-accounts"></a>Mitme piirkonna kontode konfigureerimine

Vähem kui minutiga [Azure portaali](documentdb-portal-global-replication.md)kaudu saab teha DocumentDB konto skaala kogu maailmas konfigureerimiseks. Kõik, mida peate tegema on valige õige järjepidevuse taseme vahel toetatud täpselt määratletud järjepidevuse mitu taset ja mõni muu arv Azure piirkondade seostada konto andmebaasi. DocumentDB järjepidevuse tasemed pakuvad Eemalda kompromissidega teatud järjepidevuse tagatise ja jõudluse vahel. 

![DocumentDB pakub mitme, määratletud (lõdvestunud) järjepidevuse mudelid][1]

DocumentDB pakub mitme, määratletud (lõdvestunud) järjepidevuse mudelid.

Valides taseme õige järjepidevuse sõltub andmete järjepidevuse tagamiseks oma rakenduse vajadustele. DocumentDB automaatselt tiražeerib andmete kõigi määratud piirkondade ja tagab järjepidevuse valitud andmebaasi konto jaoks. 


## <a name="using-multi-region-failover"></a>Mitme piirkonna Tõrkesiirde abil 

Azure'i DocumentDB suudab läbipaistev Tõrkesiirde andmebaasi kontod mitme Azure'i piirkondade – uue [mitme sihitamise API-de] [ developingwithmultipleregions] garantiid, et teie rakendus edasi kasutada loogiline lõpp-punkti ja on katkestusteta, on tõrkesiire. Tõrkesiirde kontrollib teie paindlikkuse rehome andmebaasi kontole juhuks, kui tekib mõni vahemiku nurjumise võimalikud tingimused, sh rakenduse, et taristu, teenuse või piirkondliku tõrked (real või jäljendatud). DocumentDB piirkondliku nurjumise korral teenuse nurjub läbipaistev üle andmebaasi konto ja rakenduse jätkub kättesaadavus kaotamata pääsevad andmetele juurde. DocumentDB pakub [99,99%-saadavus SLAs][sla], saate testida rakenduse end, et end-saadavus atribuudid jäljendamine piirkondliku tõrge mõlemad, [programmiliselt] [ arm] Azure portaali kaudu.


## <a name="scaling-across-the-planet"></a>Skaleerimist kogu maailmas
DocumentDB võimaldab teil sõltumatult ettevalmistamise läbilaskevõime ja tarbimine iga DocumentDB saidikogumi igal alal salvestusruum globaalselt kõigis teie andmebaasi kontoga seotud piirkondades. DocumentDB saidikogumi automaatselt levitatakse globaalselt ja hallatavate kõigis piirkondades andmebaasi kontoga seotud. Saidikogumite kontol andmebaasi saab levitada kõigis Azure regioonid, mille [DocumentDB on saadaval][serviceregions]. 

On ostetud ja tarbitud DocumentDB iga saidikogumi jaoks on automaatselt ette valmistatud kõigis piirkondades võrdselt. See võimaldab rakenduse mastaapimiseks sujuvalt üle maakera [maksma ainult läbilaskevõime ja salvestusruumi te kasutate iga tunni jooksul][pricing]. Näiteks kui 2 miljonit RUs DocumentDB saidikogumi jaoks on ette valmistatud, siis iga piirkonna andmebaasi kontoga seotud saab 2 miljonit RUs selle saidikogumi jaoks. See on kujutatud allpool.

![Skaleerimise läbilaskevõime DocumentDB kogumi üle neli regiooni][2]

DocumentDB tagab < 10 ms lugemine ja kirjutamine < 15 ms latentsused P99 veebisaidil. Lugege taotlusi kunagi üle andmekeskuse piiri selleks, et tagada [ühtsuse nõuded valitud][consistency]. Funktsiooni kirjutab on alati kvoorumi kinnitatud kohalikult enne neid peetakse klientidele. Iga andmebaasi konto on konfigureeritud kirjutamine piirkond prioriteet. Piirkond, mis on tähistatud kõrgeim eelistus toimib kirjutamine praeguse ala konto. Kõik SDK-d on läbipaistev suunata andmebaasi konto kirjutab kirjutamine praeguse ala. 

Lõpetuseks, kuna DocumentDB on täielikult [skeemi diagnostika] [ vldb] – te ei pea muretsema skeemid haldamine/värskendamine või sekundaarne registrid üle mitme andmekeskuste. [SQL-päringud] [ sqlqueries] ajal teie taotlus ja andmemudelid edaspidi muutuda tööd jätkata. 


## <a name="enabling-global-distribution"></a>Globaalse jaotuse lubamine 

Oma andmete kohalikult või globaalselt jaotatud kas seostamise kaudu saate otsustada ühe või mitme Azure'i regioonid, kus on DocumentDB andmebaasi konto. Saate lisada või piirkondade andmebaasi kontole igal ajal eemaldada. Globaalne jagamise lubamiseks portaali kaudu, vaadake, [Kuidas teha DocumentDB globaalne andmebaasi tiražeerimine Azure'i portaalis](documentdb-portal-global-replication.md). Globaalne jagamise lubamiseks programatically, lugege teemat [mitme piirkonna DocumentDB kontode arendamine](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet jaotavad andmete globaalselt DocumentDB järgmistest artiklitest:

* [Ettevalmistamise ja kogumi jaoks] [throughputandstorage]
* [Mitme sihitamise API ülejäänud kaudu. .NET, Java, Python ja sõlm SDK-d] [developingwithmultipleregions]
* [DocumentDB tasemete järjepidevus] [consistency]
* [Kättesaadavus SLAs] [sla]
* [Andmebaasi konto haldamine] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

