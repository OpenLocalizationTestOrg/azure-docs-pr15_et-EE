<properties
   pageTitle="Teenuse struktuuri arhitektuur | Microsoft Azure'i"
   description="Teenus on hajutatud süsteemide platvormi luua pilv scalable, usaldusväärseid ja hõlpsasti hallatava rakendusi kasutada. Selles artiklis kirjeldatakse teenuse struktuuri arhitektuur."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Teenuse struktuuri arhitektuur

Teenuse struktuuri on loodud kihiti olevad alasüsteemidega. Alasüsteeme võimaldavad kirjutada rakendusi, mis on:

* Väga saadaval
* Skaleeritav
* Mõistliku
* Testitavad

Järgmisel joonisel on suur alamsüsteemide teenuse struktuuri.

![Teenuse struktuuri arhitektuur skeem](media/service-fabric-architecture/service-fabric-architecture.png)

Jaotatud süsteemis võimalus turvaliselt suhelda sõlme klaster vahel on oluline. Virnas allosas on transport alasüsteemi, mis pakub turvaline side sõlme vahel. Transport kohal asub alasüsteemi federation alasüsteemi, mis kogumite erinevaid sõlmi ühe üksuse (nimega kogumite), et teenuse struktuuri saate tuvastada tõrkeid, teha juhataja valimine ja anda ühtse marsruutimist. Kihiti peal federation alasüsteemi töökindluse alasüsteemi vastutab teenuse struktuuri teenuse kaudu näiteks dispersioonanalüüs, ressursside haldamine ja Tõrkesiirde menetlustele usaldusväärsuse. Federation alasüsteemi aluseks ka majutamist ja aktiveerimise alasüsteemi, mis haldab elutsükli rakenduse ühe sõlme. Juhtimise alasüsteemi haldab rakenduste ja teenuste elutsükli. Testability alasüsteemi aitab rakenduste arendajatele Testige oma teenuseid abil jäljendatud vead enne ja pärast tootmise keskkondades rakenduste ja teenuste juurutamine. Teenuse struktuuri pakub võimalust lahendamiseks kaudu oma side alasüsteemi teenuse asukohad. Rakenduse programmeerimise mudelite esitatud arendajatele on kihiti peal alasüsteeme koos rakenduse lubamiseks instrumentaarium mudel.

## <a name="transport-subsystem"></a>Transport alasüsteemi
Transport alasüsteemi rakendab kakspunkt datagrammi side kanali. Selle kanali kasutatakse jooksul teenus struktuuri kogumite ja edastamist teenuse struktuuri kobar ja klientide vahel. See toetab ühesuunalise ja taotlus-vasta side mustrid, mille alusel rakendamiseks leviedastuse ja multisaate Federation kiht. Transport alasüsteemi tagab side abil X509 serdid või Windowsi turberühma. Allasüsteem kasutatakse ettevõttesiseselt teenuse struktuuri ja pole otse kättesaadav arendajate programmeerimine jaoks.

## <a name="federation-subsystem"></a>Federation alasüsteemi
Jaotatud süsteemis põhjus sõlmed kogumi kohta, peate olema süsteemi vaates. Federation allsüsteemi kasutab side primitiivid esitatud transport alasüsteemi ja pisteid erinevad sõlmed sisse ühe ühendatud klaster, mis on selle põhjus, kohta. Pakub hajutatud süsteemide primitiivid nõutud allsüsteemide - tõrge avastamine, juhataja valimine ja ühtsete marsruutimist. Alasüsteemi Domeeniliit on ehitatud jaotatud räsi tabelite 128-bitise Turbeloa tühikuga. Alasüsteemi loob ring topoloogia üle sõlmed, koos iga sõlme eraldatud Turbeloa ruumi alamhulga omandiõiguse Helista. Tõrge avastamine, kasutab kiht südamelööke ja vahekohtu liisimine süsteem. Federation alasüsteemi tagatakse ka keerulisi Liitu ja lahkumise protokoll ainult ühe omaniku luba olemas igal ajal kaudu. See anda juhataja valimine ja ühtsete marsruutimise garantiid.

## <a name="reliability-subsystem"></a>Töökindluse alasüsteemi
Töökindluse alasüsteemi pakub süsteem kättesaadavaks teenuse struktuuri teenust väga _paljundaja_, _Tõrkesiirde haldur_ja _Ressursside koormusetasakaalustusteenuse_abil.

* Kopeerija tagab, et muutuste esmane teenuse koopia automaatselt kopeeritud teisene koopiad, säilitades järjepidevuse esmaseid ja teiseseid koopiad kogumi teenuse koopia. Kopeerija on eest kvoorumi vahel koopiad koopia määramine. See suhtleb Tõrkesiirde ühiku saada toiminguid korrata loendit ja ümberkonfigureerimist agent sisestab konfiguratsiooni koopia määramine. Selle konfiguratsiooni näitab, milliseid toiminguid korrata vaja koopiad. Teenuse struktuuri pakub vaikimisi paljundaja nimetatakse struktuuri paljundaja, mida saab kasutada programmeerimise mudeli API teha teenuse olek väga saadaval ja usaldusväärne.
* Tõrkesiirde haldur tagab, et kui sõlmed on lisatud või sealt klaster, laadi on automaatselt levitada saadaval sõlmed üle. Klaster sõlme nurjumisel klaster kuvatakse automaatselt konfigureerida selle teenuse koopiad säilitada kättesaadavus.
* Ressursihaldur paigutab üle tõrge domeeni klaster teenuse koopiad ja tagab kõigi Tõrkesiirde üksused toimivad. Ressursihaldur nõuded ka teenuse ressursid üle aluseks oleva ühiskasutusega kogumi kobar sõlmed optimaalse ühtse jaotust saavutamiseks.

## <a name="management-subsystem"></a>Alasüsteemi haldus
Juhtimise alasüsteemi pakub lõpuni teenuse ja rakenduse elutsükli haldus. PowerShelli cmdlet-käskude ja haldus API-de võimaldavad teil ettevalmistamise, juurutada, paik, versioonitäiendus ja asutusest rakenduste kaota kättesaadavuse. Juhtimise alasüsteemi teostab see kaudu järgmisi teenuseid.

* **Halduri**: on esmane teenus, mis on Tõrkesiirde halduriga töökindluse kaudu rakenduste paigutamiseks põhjal teenuse paigutuse piiranguid sõlmed suhtleb. Ressursihaldur Tõrkesiirde alamsüsteemis tagab, et piiranguid pole kunagi katkenud. Funktsiooni halduri haldab rakenduste elutsükli sättest asutusest. See ühendab seisund halduri tagamaks, et rakenduste saadavus ei lähe kaduma seisukohast semantilise seisund uuendamine.
* **Tervise halduri**: see teenus võimaldab kobar üksuste rakenduste ja teenuste seisundi jälgimine. Kobar üksuste (nt sõlmed, teenuse sektsiooni ja koopiad) saate teatada seisundi andmeid, mis seejärel koondatakse tsentraliseeritud seisund pood. Seisund teave üldist punkti õigel ajal seisund hetktõmmist teenused ja sõlmed jaotatud mitmesse sõlme klaster, mis võimaldab teil mis tahes vajadusele tegevuste üle. Seisund päringu API-de võimaldavad päringu seisund sündmuste teatatud seisund alasüsteemi. Seisund päringu API-de talletatud seisund poest või liidetud, töötlemata seisundi andmeid tagastada tõlgendada seisundi andmete kindla kobar üksust.
* **Image Store**: see teenus pakub salvestusruumi ja rakenduse kahendfaile jaotuse. See teenus pakub jaotatud failide lihtsat poe, kus rakendused üles ja alla laadida.


## <a name="hosting-subsystem"></a>Majutusteenuse alasüsteemi
Kobar haldur teavitab majutusteenuse alasüsteemi teenuste tuleb hallata konkreetse sõlme (töötab iga sõlme). Seejärel haldab majutusteenuse alasüsteemi rakenduse sõlme elutsükli. See suhtleb töökindlust ja seisundi komponentide selle koopiad paigutatakse õigesti ning et terve.

## <a name="communication-subsystem"></a>Suhtlus alasüsteemi
Allasüsteem pakub usaldusväärne sõnumside sees kobar- ja teenuseüksuste discovery nimetamise teenuse kaudu. Teenuse nimetamise lahendab teenuste nimed klaster asukohta ja võimaldab kasutajatel hallata teenuste nimed ja atribuudid. Nimetamise teenuse kasutamise kliendid saavad turvaliselt suhelda sõlm kobar lahendamiseks teenuse nimi ja tuua metaandmete teenus. Lihtne nimetamise kliendi API kasutamisel teenuse struktuuri kasutajad võivad tekkida teenustele ja klientidele võimeline lahendamine vaatamata sõlm dünaamilisus praeguse võrgukoha või klaster uuesti suurus.

## <a name="testability-subsystem"></a>Testability alasüsteemi
Testability on komplekt tööriistu, mis on mõeldud teenuse struktuuri tugineb analüüsiteenused. Tööriistade võimaldavad hõlpsalt põhjustada mõtestatud vead ja käivitage test stsenaariumid kasutada ja kinnitage arvukaid Ühendriigid ja üleminekud, et teenuse kogemus kogu tööiga kõik kontrollitud ja turvalised arendaja. Testability võimaldab ka rohkem kontrollib, mida saate itereerima läbida erinevate ebaõnnestumisi kättesaadavus kaotamata. See pakub testi sisse-tekitamiseks keskkonnas.
