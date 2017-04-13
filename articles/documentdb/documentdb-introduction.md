<properties 
    pageTitle="Sissejuhatus DocumentDB, JSON andmebaasi | Microsoft Azure'i" 
    description="Lisateavet Azure DocumentDB NoSQL JSON andmebaasi. Selle dokumendi andmebaasi loomiseks big data, elastne skaleeritavus ja kõrge-saadavus jaoks." 
    keywords="JSON andmebaasi, dokumendi andmebaasi"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Sissejuhatus DocumentDB: NoSQL JSON andmebaasi

##<a name="what-is-documentdb"></a>Mis on DocumentDB?

DocumentDB on täielikult hallatud NoSQL andmebaasi teenus ehitatud kiiresti ja hõlpsalt prognoosida jõudluse, kõrge-saadavus, elastne mastaapimist, levitamist ja hõlpsamaks arenduse. Skeemi tasuta NoSQL andmebaasi DocumentDB pakub rikkaliku ja tuttavad SQL-i päring võimaluste ühtsete madal latentsused JSON andmete – 99% oma serveeritakse alla 10 millisekundit – 99% teie kirjutab tagamine on saadaval jaotises 15 millisekundit. Nende kordumatu eeliste tegemine DocumentDB suur sobivad web, mobiil, mängimine, ja asjade ja palju muud rakendused, mis on vaja sujuvalt ulatuse ja globaalse dispersioonanalüüs.

## <a name="how-can-i-learn-about-documentdb"></a>Kuidas saada DocumentDB kohta? 

Lisateavet DocumentDB ja vaadake seda töötamas kiiresti on kolme järgmist: 

1. Vaadake kahe minuti [mis on DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) video, mis tutvustab DocumentDB kasutamise eelised.
2. Vaadake kolme minuti [Loomine DocumentDB Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) video, mis toob esile alustamine DocumentDB Azure portaali abil tehke järgmist.
3. Külastage [Päringu mänguväljak](http://www.documentdb.com/sql/demo), kus saate tutvustavad erinevate tegevuste saadaval DocumentDB rikkalikke päringu funktsioonide kohta. Seejärel vahekaarti Liivakasti suunduge ja käivitage oma kohandatud SQL-päringud ja katsetada DocumentDB.

Tagastab funktsioon selle artikli kus meil kuvatakse tõrkeid süvitsi.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Millised võimalused ja põhifunktsioonide ei DocumentDB pakuvad?  

Azure'i DocumentDB pakub järgmisi olulisi funktsioone ja eelised:

-   **Ja elastically scalable:** Kergesti mastaapimiseks või rakenduse vastavalt vajadustele DocumentDB JSON andmebaasi mahtu. Teie andmed on salvestatud jaoks vähese prognoositavad latentsused ühtlase olekus kettale (SSD). DocumentDB toetab ümbriste nimega saidikogumid, mida saab skaala praktiliselt piiramatu salvestusruum suurused ja ettevalmistatud läbilaskevõime JSON andmete talletamiseks. Te saate elastically skaala DocumentDB prognoositavad jõudluse sujuvalt rakenduse kasvab. 

-   **Mitme piirkonna kopeerimine:** DocumentDB läbipaistev tiražeerib andmete te olete DocumentDB kontoga seostatud, mistõttu saate välja töötada ka selliseid rakendusi, mis nõuavad globaalne juurdepääs andmetele, tagades kompromissidega järjepidevuse, kättesaadavuse ja jõudluse tagamiseks kõik koos vastavate garantiid kõigis piirkondades. DocumentDB pakub läbipaistev piirkondliku Tõrkesiirde koos mitme sihitamise API-d ja võimalus elastically skaala ja kogu maailmas. Siit saate teada, mitu tekstivormingus [andmeid globaalselt DocumentDB levitamine](documentdb-distribute-data-globally.md).

-   **Kohapealseid päringuid ja tuttav SQL-süntaks:** DocumentDB heterogeensete JSON dokumentide talletamine ja päringu tuttav SQL-süntaks – need dokumendid. DocumentDB kasutab väga samaaegseid, lock, mis on tasuta, log liigendatud indekseerimise tehnoloogia kogu dokumendi sisu automaatselt registrisse. See võimaldab rikkaliku reaalajas päringute vajamata skeemi näpunäiteid, teisene registrid või vaadete määramiseks. Lisateavet [Päringu DocumentDB](documentdb-sql-query.md). 

-   **JavaScripti täitmise jooksul andmebaas:** Kiire rakenduse loogika salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid (UDF) standard JavaScripti kasutamine. See võimaldab oma rakenduse loogika ilma erinevuse rakendus ja andmebaasi skeemi muretsema andmete kasutamiseks. DocumentDB pakub täielikku selgituseks täitmise JavaScripti rakenduse loogika otse andmebaasimootor sees. JavaScripti sügav integreerimine võimaldab lisa, Asenda, KUSTUTUS- ja JavaScripti programmi kaudu valige toimingute täitmist isoleeritakse toimingu nimega. Lisateavet [DocumentDB serveripoolne kavandamine](documentdb-programming.md).

-   **Timmitavad järjepidevuse tasemed:** Valige määratletud järjepidevuse neli taset optimaalse kompromissidest järjepidevuse ja jõudluse saavutamiseks. Päringute ja toimingud, DocumentDB pakub erinevate järjepidevuse neli taset: tugev, mis on piiratud staleness, seansiga ja lõpuks. Varundustöö, täpselt määratletud järjepidevuse tasemed võimaldavad kompromisse heli järjepidevuse kättesaadavus ning latentsuse vahel. Lisateavet [järjepidevus kasutamisel kättesaadavus ja jõudluse DocumentDB](documentdb-consistency-levels.md).

-   **Täielikult hallata:** Enam vaja andmebaas ja seadme ressursside haldamine. Täielikult hallatavad Microsoft Azure'i teenus pole peab teil virtuaalmasinates haldamine, juurutada ja konfigureerida tarkvara, hallata skaleerimist, või tegeleda keerukate andmete taseme uuendamine. Automaatselt iga andmebaasi varundada ja kaitstud piirkondliku ebaõnnestumist. Saate lisada DocumentDB konto ja ettevalmistamise võimsus, nagu on vaja, mis võimaldab teil keskenduda rakenduse kasutamise ja haldamise andmebaasi asemel. 

-   **Avatud kujundus:** Töö Kiire alustamine, kasutades olemasolevaid oskusi ja tööriistu. Programmeerimise suhtes DocumentDB on lihtne, vastutulelik ja ei nõua saate vastu võtta uusi tööriistu või kohandatud laiendid JSON-või JavaScripti järgida. Pääsete andmebaasi funktsioone, sh CRUD, päringu ja JavaScripti töötlemise üle lihtne rahulik HTTP kasutajaliides. DocumentDB hõlmab olemasolevad vormid, keelte ja standardid pakkudes kõrge väärtus andmebaasi võimalusi nende peal.

-   **Automaatse indekseerimise:** DocumentDB [automaatselt indeksite](documentdb-indexing.md) vaikimisi andmebaasis kõik dokumendid ja oodata või nõua skeemi või sekundaarne indeksite loomiseks. Ei soovi indekseerida kõik? Ärge muretsege, saate [loobuda teed failides JSON](documentdb-indexing-policies.md) ka.

##<a name="data-management"></a>Kuidas DocumentDB andmete haldamine?

Azure'i DocumentDB haldab JSON andmeid täpselt määratletud andmebaasi ressurssidest. Need ressursid on kopeeritud jaoks kõrge-saadavus ja kordumatult st abil oma loogilise URI. DocumentDB pakub lihtsa HTTP põhineb rahulik programmeerimise mudeli kõik ressursid. 

DocumentDB andmebaasi konto on kordumatu nimeruumi, mis annab teile juurdepääsu Azure DocumentDB. Enne kui saate luua andmebaasi konto, peab teil olema Azure'i tellimus, mille kaudu pääsete juurde erinevaid Azure teenuseid. 

Kõigi vahendite DocumentDB on eeskujul ja talletatud JSON dokumentidena. Ressursse nimega üksused, mis on JSON dokumentide sisaldavate metaandmete ja nagu kanalid, millised on nende eelvaadet. Selliste üksuste komplektides, sisalduvad nende vastavate kanalite.

Alloleval pildil on näha DocumentDB ressursside vahelisi seoseid:

![Ressursside DocumentDB NoSQL JSON andmebaasi hierarhilise seos][1] 

Andmebaasi konto koosneb andmebaase, kumbki sisaldab mitut saidikogumid, mis võib sisaldada salvestatud toimingute, päästikute, UDF-ID, dokumente ja seotud manused. Andmebaasi ka seotud kasutajad, igal versioonil kogumi õigused juurdepääsu erinevad muude saidikogumite, Salvestatud toimingute, päästikute, UDF-ID, dokumendid ja manused. Kuigi andmebaase, kasutajaid, õiguste ja saidikogumite on tuntud skeemid ressursse süsteemi määratletud - dokumendid, Salvestatud toimingute, päästikute, UDF-ID ja manuseid sisaldavad suvalise, kasutaja määratletud JSON sisu.  

##<a name="develop"></a>Kuidas rakendused DocumentDB abil saab luua?

Azure'i DocumentDB seab REST API-ga, saab helistada võimeline tegema HTTP-või HTTPS taotlusi keelega abil. Lisaks DocumentDB pakub programmeerimise teekide jaoks mitme levinud keeled. Need teegid lihtsustada paljude aspektide töötamine Azure'i DocumentDB teostavaid üksikasjad nagu aadress vahemällu, erand haldus, automaatne korduskatsed jne. Teekide on praegu saadaval järgmised keeled ja platvormid.  

Laadi alla | Dokumentatsioon
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [.Net-i teeki](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Node.js teek](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java teek](http://azure.github.io/azure-documentdb-java/)
[JavaScripti SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScripti teek](http://azure.github.io/azure-documentdb-js/)
n/a | [Serveripoolne JavaScripti SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python teek](http://azure.github.io/azure-documentdb-python/)

Põhilised edaspidiseks loomine, lugemine, värskendada ja kustutada toimingud, DocumentDB pakub rikkaliku SQL-päringu kasutajaliidese toomine JSON dokumentide ja serveripoolne tugi selgituseks täitmise JavaScripti rakenduse loogika. Päringu ja skripti täitmise liidesed on saadaval kõigi platvormi teekide kui ka REST API-de kaudu. 

### <a name="sql-query"></a>SQL-päringu
Azure'i DocumentDB toetab päringute abil SQL keelt, mida JavaScripti tüüpi süsteem ja avaldised, mis toetab relatsiooniliste, hierarhiliste ja ruumilise päringute aluseks on dokumente. DocumentDB päringukeele on päringu JSON dokumentide lihtsat, ent võimsat kasutajaliides. Keele toetab alamhulga ANSI SQL-i grammatika ja lisab JavaScripti objekti, massiive, objekti ehituse ja funktsioon kutsumise sügav integreerimine. DocumentDB pakub oma päringu mudelit, mis tahes konkreetsete skeemi või indekseerimise näpunäiteid ilma arendaja.

Kasutaja määratletud funktsioonid (UDF) saate DocumentDB registreeritud ja seetõttu ulatub grammatika loogika kohandatud rakenduse toetamiseks osana SQL-päringu viidatud. Nende UDF-ID on kirjutatud JavaScript programmid ja andmebaasi jooksul. 

.NET arendajad, pakub DocumentDB ka LINQ päringu pakkuja [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx)osana. 

### <a name="transactions-and-javascript-execution"></a>Tehingud ja JavaScripti täitmine
DocumentDB võimaldab kirjutada rakenduse loogika kirjutatud täiesti JavaScript nimega programmid. Järgmiste programmide kogumi jaoks registreeritud ja saate probleemi andmebaasi toimingud dokumentidega antud saidikogumi sees. JavaScripti saab registreerida päästik, salvestatud toimingu või kasutaja määratletud funktsioon täitmiseks. Päästikute ja salvestatud toimingute saate loomine, lugemine, värskendamine ja kustutamine dokumentide tuleks kasutaja määratletud funktsioonid käivitada ilma kirjutamisõigused kogumi loogika päringu täitmise osana.

JavaScripti täitmine DocumentDB sees on kujundatud pärast mõisted relatsiooniandmebaasist süsteemide, JavaScript tänapäevane asendusena Transact-SQL ei toeta. Kõik JavaScripti loogika käivitatakse kaardiga hetktõmmise eraldamise toimingu ümbritseva HAPPE sees. Täitmise käigus, kui JavaScripti põhjustab erandi, siis kogu tehing on katkestatud.

## <a name="next-steps"></a>Järgmised sammud
Juba Azure'i konto? Seejärel saate saate alustamine DocumentDB [Azure portaali](https://portal.azure.com/#gallery/Microsoft.DocumentDB) [DocumentDB andmebaasi konto loomise](documentdb-create-account.md)abil.

Pole Azure kontot? Sa saad:

- Mõne [Azure tasuta prooviversioon](https://azure.microsoft.com/free/), mis annab teile 30 päeva ja $200 proovida Azure'i teenuste kasutajaks registreerumist. 
- Kui teil on MSDN-i tellimus, saate on õigus saada [150 $ tasuta Azure autorid kuus](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) kasutada iga Azure'i teenus. 

Siis, kui olete valmis rohkem teada saada, külastage meie [õppeteema](https://azure.microsoft.com/documentation/learning-paths/documentdb/) liikuda kõigi saadaolevate õppematerjalid. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
