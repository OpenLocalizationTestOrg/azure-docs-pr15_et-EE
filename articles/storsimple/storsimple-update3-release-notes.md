<properties 
   pageTitle="StorSimple 8000 seeria Update 3 väljalaskemärkmed | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple 8000 seeria Update 3 uusi funktsioone, probleemid ja lahendused."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>StorSimple 8000 seeria Update 3 väljalaskemärkmed  

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed kirjeldada uusi funktsioone ja kriitilised avatud probleemid StorSimple 8000 seeria Update 3. Sisaldavad ka StorSimple tarkvaravärskendusi see väljaanne sisaldab loendit. 

Mis tahes StorSimple seade Release (GA) või Update 0,1 värskendus 2.2 kaudu saab rakendada Update 3. Seotud Update 3 seadme versioon on 6.3.9600.17759.

Palun vaadake üle enne juurutamist värskenduse oma StorSimple lahendus on väljalaskemärkmed sisalduvat teavet.

>[AZURE.IMPORTANT]
> 
> - Värskenduse 3 on seadme tarkvara, LSI draiveri ja püsivara ja Storport ja Spaceport värskendamist. Kulub ligikaudu 1,5-2 tundi värskendust. 

> - Uue keskkonda, te ei näe värskendused kohe, kuna me etappidega plaanida värskendusi. Oodake mõni päev ja seejärel otsige värskendusi uuesti, kui need muutuvad kättesaadavaks varsti.


## <a name="whats-new-in-update-3"></a>Mis on uut rakenduses Update 3

Järgmisi olulisi parandusi ja veaparandusi on tehtud värskenduste 3.

 
- Valige kontrolleril tulemuseks kiirem täitmise süsteemi käivitada **automatiseeritud ruumi taastamise muudatused** – alates Update 3, ruumi taastamise algoritmide. Pordid, mida on vaja töötada ruumi taastamise kohta lisateabe saamiseks vaadake [StorSimple võrgu nõuetele](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Jõudlustäiustused** – Update 3 on täiustatud lugemis-ja kirjutamisõigusega jõudluse pilveteenusesse.

- **Migreerimise seotud täiustused** – see väljaanne, mitu veaparandused ja täiustused olid teha migreerimise funktsiooni 5000/7000 sarja seadmetest 8000 sarja seadmed. Migreerimise funktsiooni kasutamise kohta lisateabe saamiseks minge [migreerimise 5000/7000 sarja seadmest 8000 sarja seadmesse](https://www.microsoft.com/en-us/download/details.aspx?id=47322). 

- **Seotud paranduste jälgimine** – see väljaanne vead seotud diagrammid, teenuse armatuurlaua ja seadme armatuurlaua kindlaks.


## <a name="issues-fixed-in-update-3"></a>Parandatud 3 probleemid

Järgmistes tabelites toodud probleemid, mis olid parandatud 3 Kokkuvõte.    

| Ei | Funktsioon                                    | Probleem                                                                                                                                                                                                                                                                                        | Kehtib seadme | Kehtib virtuaalse seade |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Host andmed migreerimine                      | Varasem versioon on StorSimple Cloud seadme ühenduseta host andmed migreerimise ajal. See probleem on lahendatud selles versioonis.                                                | Ei                        | Jah                        |
| 2  | Kohalik kinnitatud mahud                     | Eelmist versiooni ilmnenud tõrkeid, helitugevuse teisendamine tõrkeid, ja Andmeteed tõrkeid kohalikult kinnitatud maht seotud probleemid. Järgmiste probleemide korral olid root põhjustada ja fikseeritud selles versioonis.                | Jah                        | Ei                       |
| 3  | Jälgimine                                    | Esines mitme seotud üksuste aruandlus- ja järelevalve samuti seadme armatuurlaua diagrammid, kus vale teavet kuvati kohalikult kinnitatud maht. Järgmiste probleemide korral on fikseeritud selles versioonis. | Jah                         | Ei                       |
| 4  | Raske kirjutab I/O                          | Töökoormus, mis on seotud raske kirjutab StorSimple kasutamisel kasutaja oleks sattunud mõni harv viga kus töökomplekt oli on mitmetasandilise pilve. See veaparandused on selles versioonis. | Jah                        | Jah                       |
| 5  | Varundus                                     | Teatud harvaesinevate juhtudel varasemates versioonides tarkvara, kui kasutaja võttis remote klooni varukoopia, neil läheks cloud tõrkeid ja toiming oleks tõrge välja. Selles väljaandes probleem on lahendatud ja toiming on lõpule jõudnud edukalt.             | Jah                        | Jah                       |
| 6  | Varukoopia poliitika                                     | Teatud juhtudel harvaesinevate varasemates versioonides tarkvara, oli seotud varukoopia poliitika kustutamise viga. See probleem on lahendatud selles versioonis.       | Jah                        | Jah                       |



## <a name="known-issues-in-update-3"></a>Teadaolevad probleemid Update 3

Järgmises tabelis antakse ülevaade selle väljaande teadaolevate probleemide.

| Ei. | Funktsioon | Probleem | Kommentaaride / lahendus | Kehtib seadme | Kehtib virtuaalse seade |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Ketas kvoorumi | Harvaesinevate juhul, kui ketta jaotise EBOD ruumi 8600 seadme enamik ei saa tulemuseks kvoorumit kettale, siis selle talletusmahtu läheb ühenduseta. See jääb ühenduseta isegi juhul, kui selle ketast ühendatakse. | Peate taaskäivitage seade. Kui probleem ei lahene, pöörduge Microsoft Support järgmised toimingud. | Jah | Ei |
| 2 | Vale kontrolleril ID | Kui selle domeenikontrolleri asendamine toimub, domeenikontrolleri 0 võib kuvada selle domeenikontrolleri 1. Ajal kontrolleril väljavahetamine, pildi laadimisel omavahelistes sõlme kontrolleril ID kuvada algselt omavahelistes kontrolleril ID. Harvaesinevate juhtudel selline käitumine võib näha ka pärast taaskäivitada. | Kasutajatoimingut ei vaja. Olukord lahendab ise, kui selle domeenikontrolleri asendamine on lõpule jõudnud. | Jah | Ei |
| 3 | Salvestusruumi kontod | Salvestusruumi teenuse abil konto salvestusruumi kustutada on mõni mittetoetatud stsenaariumi. See viib olukorda, kus kasutaja andmeid ei saa tuua.|  | Jah | Jah |
| 4 | Seadme Tõrkesiirde | Mitme failovers helitugevuse container sama allika seadmest ja muu target seadmed ei toetata. Helitugevuse ümbriste esimesel üle seadme nurjus ilma andmete omaniku teeb Tõrkesiirde ühe surnud seadmest, mitu seadet. Pärast Tõrkesiirde, nende helitugevuse ümbriste kuvada või toimivad teisiti Azure klassikaline portaali kuvatava. | | Jah | Ei |
| 5 | Installimine | Ajal StorSimple adapterit SharePointi installi, tuleb sisestada seadme IP selleks installi lõpule.    | | Jah | Ei |
| 6 | Veebiteenuse puhverserveri | Kui teie web puhverserveri konfigureerimine on määratud protokoll HTTPS, mõjutab oma seadme-teenus side ja seadme katkestada. Tugiteenuste pakettide luuakse ka protsessi, tarbimine märgatavalt ressursid oma seadmes. | Veenduge, et web puhverserveri URL on määratud protokoll HTTP. Lisateabe saamiseks minge [konfigureerimine veebiteenuse puhverserveri oma seade](storsimple-configure-web-proxy.md). | Jah | Ei |
| 7 | Veebiteenuse puhverserveri | Kui konfigureerimine ja lubada veebiteenuse puhverserveri registreeritud seadmes, siis pead taaskäivitage aktiivne kontrolleril seadmes. | | Jah | Ei |
| 8 | Kõrge cloud latentsus ja suur I/O töökoormus | Väga kõrge cloud latentsused (järjestuse sekundites) ja suur I/O töökoormus kombinatsiooni käivitamise StorSimple seadme seadme mahud minema halvenenud olekus ja selle väljundtoimingute võib nurjuda tõrketeatega "seade ei ole valmis" tõrge. | Peate käsitsi taaskäivitage seade kontrollerid või seadme Tõrkesiirde taastamine juhul teha. | Jah | Ei |
| 9 | Azure'i PowerShelli | Kui kasutate StorSimple cmdlet-käsk Get-AzureStorSimpleStorageAccountCredential **& #124; Valige – Object – esmalt 1 - ootamine** saate luua uue **VolumeContainer** objekti esimest objekti valimiseks soovitud cmdlet kuvab kõik objektid. | Murra cmdlet sulgudes järgmiselt: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Valige – Object – esmalt 1 - ootamine** | Jah | Jah |
| 10| Migreerimise | Kui mitme helitugevuse ümbriste edastatakse migreerimise jaoks, ETA uusima varundamiseks on ainult esimene helitugevuse container jaoks täpne. Lisaks hakkavad paralleelselt migreerimise pärast migreerimist esimese 4 varukoopiaid esimese helitugevuse ümbrises. | Soovitame migreerite ühe helitugevuse container korraga. | Jah | Ei |
| 11| Migreerimise | Pärast taastada, ei lisata mahud varukoopia poliitika või virtuaalse ketta rühma. | Peate need mahud lisamiseks varukoopia poliitika varukoopiate loomiseks. | Jah | Jah |
| 12| Migreerimise | Pärast migreerimise lõpuleviimist peate 5000/7000 sarja seadme juurde ei pääse migreeritud andmete ümbriste. | Soovitame, et kustutate migreeritud andmete ümbriste kui migreerimine on lõpule viidud ja kinnitatud. | Jah | Ei |
| 13| Klooni ja DR | StorSimple seadet, kus töötab Update 1 ei saa klooni või teha avariitaastet eelse update 1 tarkvara seade. | Peate värskendama target seadme värskendamine 1 järgmiste toimingute lubamiseks | Jah | Jah |
| 14 | Migreerimise | Kui seal on seotud mahud helitugevuse rühmade ei pruugi konfiguratsiooni varundus migreerimise 5000-7000 sarja seadmes. | Tühja helitugevuse rühmad koos seotud mahud kustutamine ja proovige uuesti konfiguratsiooni varukoopia.| Jah | Ei |
| 15 | Azure'i PowerShelli cmdlet-käskude ja kohalikult kinnitatud mahud | Ei saa luua kohalikult kinnitatud helitugevuse Azure PowerShelli cmdlet-käskude kaudu. (Mis tahes Helitugevuse Azure PowerShelli kaudu loomist kuvatakse sõltuvad.) |Alati konfigureerimine kohalikult kinnitatud mahud StorSimple halduri teenuse abil.| Jah | Ei |
| 16 |Kohalik kinnitatud maht vaba ruumi | Kui kustutate kohalikult kinnitatud helitugevust, ei värskendata uue mahud ruumi kohe. StorSimple halduri teenuse värskendab kohaliku ruumi ligikaudu iga tund.| Oodake enne, kui proovite luua uus draiv tund. | Jah | Ei |
| 17 | Kohalik kinnitatud mahud | Tööpäevad taastamine seab ajutine hetktõmmise varukoopia varukoopia kataloogi, kuid ainult taastamine töö kestel. Lisaks seab selle virtuaalse ketta rühma eesliite **tmpCollection** **Varundamise poliitikate** lehel, kuid ainult taastamine töö kestel. | See käitumine võib ilmneda juhul, kui tööpäevad taastamine on kinnitatud ainult kohalikult mahud või kombinatsiooni kohalikult kinnitatud ja astmeline mahu. Kui taastamine töö sisaldab ainult astmeline mahtu, siis seda käitumist ei toimu. Pole kasutaja sekkumine on nõutav. | Jah | Ei |
| 18 | Kohalik kinnitatud mahud | Kui tühistate tellimuse taastamine töö ja selle domeenikontrolleri Tõrkesiirde ilmneb kohe, kui hiljem taastamine töö kuvatakse **nurjunud** asemel **tühistatud**. Kui taastamine töö nurjub ja selle domeenikontrolleri Tõrkesiirde kohe, kui hiljem taastamine töö kuvatakse **tühistatud** asemel **nurjus**. | See käitumine võib ilmneda juhul, kui tööpäevad taastamine on kinnitatud ainult kohalikult mahud või kombinatsiooni kohalikult kinnitatud ja astmeline mahu. Kui taastamine töö sisaldab ainult astmeline mahtu, siis seda käitumist ei toimu. Pole kasutaja sekkumine on nõutav. | Jah | Ei |
| 19 |Kohalik kinnitatud mahud | Kui tühistate taastamine töö või taastamise nurjub, ja seejärel selle domeenikontrolleri Tõrkesiirde, kuvatakse ka täiendavad taastamine töö lehel **töö** . | See käitumine võib ilmneda juhul, kui tööpäevad taastamine on kinnitatud ainult kohalikult mahud või kombinatsiooni kohalikult kinnitatud ja astmeline mahu. Kui taastamine töö sisaldab ainult astmeline mahtu, siis seda käitumist ei toimu. Pole kasutaja sekkumine on nõutav. | Jah | Ei |
| 20 |Kohalik kinnitatud mahud | Kui proovite teisendamise astmeline (loodud ja kloonitud Update 1,2 või varasemates) kohalikult kinnitatud helitugevust ja seadme hakkab otsa saama või on pilv katkestuste, siis korral kloon(ID) saab rikutud.| See probleem ilmneb ainult nende mahtu, mis on loodud ja kloonitud koos eelse Update 2.1 tarkvara. See peaks olema ka harv stsenaariumi.|
| 21 | Helitugevuse teisendamine | Värskendage ACRs, manustatud maht helitugevuse teisendamise ajal (mitmetasandilise soovite kohalikult kinnitatud või vastupidi). Värskendamine on ACRs võib põhjustada andmete rikkumist. | Vajadusel värskendage ACRs enne helitugevus teisendamise ja teha täiendavaid ACR värskendused, teisendamise ajal. |
| 22 | Värskendused | Kasutades Update 3, **hooldusvaates Azure klassikaline portaalis** kuvatakse järgmine teade, mis on seotud Update 2 - "StorSimple 8000 seeria Update 2 sisaldab võimaluse Microsoftile aktiivselt kogu Logiteave seadme kaudu, kui me ei tuvasta võimalikke probleeme". See on eksitavat, nagu see näitab, et seade värskendatakse Update 2. Kui seade on värskendatud Update 3 succeesfully, see teade kaob. | Selline käitumine kõrvaldatakse tulevikus. | Jah | Ei |


## <a name="controller-and-firmware-updates-in-update-3"></a>Selle domeenikontrolleri ja püsivara värskendused Update 3

See väljaanne sisaldab LSI draiveri ja püsivara. LSI draiveri ja püsivara värskenduste installimise kohta leiate lisateavet teemast [installida 3](storsimple-install-update-3.md) StorSimple seadmes.

 
## <a name="virtual-device-updates-in-update-3"></a>Virtuaalne seadme värskenduste Update 3

Värskendust ei saa rakendada StorSimple Cloud seadme (nimetatakse ka virtuaalse seadme). Uue virtuaalse seadmed on vaja luua. 


## <a name="next-step"></a>Järgmise juhise juurde

Siit saate teada, kuidas [installida 3](storsimple-install-update-3.md) StorSimple seadme.
