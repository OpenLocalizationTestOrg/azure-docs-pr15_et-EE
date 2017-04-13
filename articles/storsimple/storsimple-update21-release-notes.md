<properties 
   pageTitle="StorSimple 8000 seeria Update 2.2 väljalaskemärkmed | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple 8000 seeria Update 2.2 uusi funktsioone, probleemid ja lahendused."
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
   ms.date="07/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 seeria Update 2.2 väljalaskemärkmed  

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed kirjeldada uusi funktsioone ja kriitiliste avatud probleemid StorSimple 8000 seeria Update 2.2. Sisaldavad ka StorSimple tarkvaravärskendusi see väljaanne sisaldab loendit. 

Värskenduse 2.2 saab rakendada mis tahes StorSimple seade Release (GA) või Update 0,1 värskendus 2.1 kaudu. Värskenduse 2.2 seostatud seadme versioon on 6.3.9600.17708.

Palun vaadake üle enne juurutamist värskenduse oma StorSimple lahendus on väljalaskemärkmed sisalduvat teavet.

>[AZURE.IMPORTANT]
> 
> - Värskenduse 2.2 on ainult tarkvaravärskendusi. Kulub ligikaudu 1,5-2 tundi värskendust. 

> - Kui teil on värskendus 2.1, soovitame rakendada värskenduse 2.2 nii kiiresti kui võimalik.

> - Uue keskkonda, te ei näe värskendused kohe, kuna me etappidega plaanida värskendusi. Oodake mõni päev ja seejärel otsige värskendusi uuesti, kui need muutuvad kättesaadavaks varsti.


## <a name="whats-new-in-update-22"></a>Mis on uut rakenduses Update 2.2

Järgmisi olulisi täiustusi tehtud värskendus 2.2.

 
- **Automaatse ruumi taastamise optimeerimine** – kui andmed on kustutatud õhukesed ettevalmistatud draividel, plokid kasutamata salvestusruumi vaja taastatud. See väljaanne sisaldab täiustatud ruumi taastamise protsessi pilvest, mille tulemusena muutuvad kättesaadavaks kiiremini võrreldes varasemate versioonidega kasutamata ruumi.


- **Hetktõmmis jõudlustäiustused** – Update 2.2 paranenud aega, et töödelda hetktõmmise teatud olukordades, kus suur maht on kasutusel ja suhteliselt vähesele pole andmeid piimapütt pilve. Stsenaarium, mis selle suurendamise kasu oleks arhiivi maht.


- **Tugiteenuste kõvendid paketi kogumine** – on paranenud, nagu paketi tugi on kogutud ja üles laadida selles versioonis. 


- **Värskendage töökindluse täiustamine** – see väljaanne on värskendus usaldusväärsus põhjustavad veaparandused.

  
 

## <a name="issues-fixed-in-update-22"></a>Fikseeritud Update 2.2 probleemid

Järgmistes tabelites toodud probleeme, mis olid värskenduste 2.2 ja 2.1 Kokkuvõte.    

| Ei | Funktsioon                                    | Probleem                                                                                                                                                                                                                                                                                        | Kehtib seadme | Kehtib virtuaalse seade |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Host jõudlus                      | Varasema versiooniga, host-side jõudlusprobleemide ilmnenud kohalikult kinnitatud helitugevuse loomise ajal ja kohalikult kinnitatud helitugevuse astmeline mahu teisendamise ajal. Järgmiste probleemide korral on fikseeritud selles versioonis, seega tulemuseks on paremad host täitmisel ajal helitugevuse loomise ja muutmise juhised.                                                                        | Jah                        | Ei                        |
| 2  | Kohalik kinnitatud mahud                     | Harvaesinevate juhtudel süsteemi krahhi kohalikult kinnitatud helitugevuse loomisel. See viga on fikseeritud selles versioonis.                                                                                                                                                               | Jah                        | Ei                        |
| 3  | Tiering                                    | Kui metaandmete StorSimple Cloud seadmete (8010 ja 8020) mitmetasandilise pilveteenusesse oli juhuslik krahh. See probleem on lahendatud selles versioonis.                                                                                                                              | Ei                         | Jah                       |
| 4  | Hetktõmmise loomine                          | Oli probleemid seotud stsenaariumid suuri andmemahtusid suureneva hetktõmmiseid luua ja minimaalsete pole andmeid piimapütt. Järgmiste probleemide korral on fikseeritud selles versioonis.                                                                                                                 | Jah                        | Jah                       |
| 5  | OpenStack autentimine                   | Pilveteenuse pakkuja nimega Openstack kasutamisel kasutaja oleks sattunud mõne funktsiooni autentimisega seotud harv viga kui JSON-parseri põhjustasid krahhi. See veaparandused on selles versioonis.                                                                                                                              | Jah                        | Ei                        |
| 6  | Host-side kopeerimine                             | Varasemates versioonides tarkvara, on seotud ODX ajastuse harv viga oli näha andmete kopeerimisel ühe teise helitugevust. Selle domeenikontrolleri Tõrkesiirde tooks ja süsteemi võib potentsiaalselt minema taaste režiimis. See veaparandused on selles versioonis. | Jah                        | Ei       |
| 7  | Windows Management Instrumentation (WMI) | Tarkvara varasemates versioonides olid mitu eksemplari web puhverserveri tõrge, välja arvatud "<ManagementException> pakkuja laadimine tõrge". See viga oli omistatud WMI põhjus ja on praegu määratud.                                                               | Jah                        | Ei                        |
| 8  | Värskendamine                                     | Teatud harvaesinevate juhtudel varasemates versioonides tarkvara, kasutaja saanud "CisPowershellHcsscripterror" kui proovite skannida või värskenduste installimine. See probleem on lahendatud selles versioonis.                                                                                        | Jah                        | Jah                       |
| 9  | Tugiteenuste pakett                            | Selles väljaandes on paranenud nii, nagu paketi tugi on kogutud ja üles laadida.                                                                                                                                                                                                      | Jah                        | Jah                                    |


## <a name="known-issues-in-update-22"></a>Teadaolevad probleemid Update 2.2

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

## <a name="controller-and-firmware-updates-in-update-22"></a>Selle domeenikontrolleri ja püsivara värskendus 2.2 värskendused

See väljaanne sisaldab tarkvara ainult värskendused. Juhul, kui värskendate versioonilt enne Update 2, peate draiver, Storport, Spaceport, ja (mõnel juhul) kettale püsivara värskendused oma seadmes.
 
Draiveri, Storport ja Spaceport ketta püsivara värskenduste installimise kohta leiate lisateavet teemast [installige värskendus 2.2](storsimple-install-update-21.md) StorSimple seadmes.

 
## <a name="virtual-device-updates-in-update-22"></a>Värskenduse 2.2 virtuaalse seadme värskendused

Värskendust ei saa rakendada virtuaalse seade. Uue virtuaalse seadmed on vaja luua. 

## <a name="next-step"></a>Järgmise juhise juurde

Siit saate teada, kuidas [installida värskenduse 2.2](storsimple-install-update-21.md) StorSimple seadme.
