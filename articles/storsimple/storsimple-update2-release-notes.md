<properties 
   pageTitle="StorSimple 8000 seeria Update 2 väljalaskemärkmed | Microsoft Azure'i"
   description="Kirjeldatakse uusi funktsioone, probleemid ja lahendused StorSimple 8000 seeria Update 2."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000 seeria Update 2 väljalaskemärkmed  

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed kirjeldada uusi funktsioone ja kriitiliste avatud probleemid StorSimple 8000 seeria Update 2. Sisaldavad ka StorSimple tarkvara, draiver ja ketta püsivara värskenduste see väljaanne sisaldab loendit. 

Mis tahes StorSimple seade Release (GA) või Update 0,1 Update 1.2 kaudu saab rakendada Update 2. Update-2-ga seostatud seadme versioon on 6.3.9600.17673.

Palun vaadake üle enne juurutamist värskenduse oma StorSimple lahendus on väljalaskemärkmed sisalduvat teavet.

>[AZURE.IMPORTANT]
> 
- Kulub ligikaudu 4-7 tundi selle värskenduse (sh Windowsi värskenduste). 
- Värskendus 2 on tarkvara, LSI draiver ja SSD püsivara värskendused.
- Uue keskkonda, te ei näe värskendused kohe, kuna me etappidega plaanida värskendusi. Oodake mõni päev ja seejärel otsige värskendusi uuesti, kui need muutuvad kättesaadavaks varsti.


## <a name="whats-new-in-update-2"></a>Mis on uut rakenduses Update 2

2 värskendusega järgmised uued funktsioonid.

- **Kohalik kinnitatud maht** – varasemates versioonides StorSimple 8000 sarja andmete plokid olid mitmetasandilise põhjal kasutus pilveteenusesse. Puudus võimalus selleks, et tagada, et plokid jääks kohalikus. Update 2, kui loote draivi, saate määrata maht nagu kohalikult kinnitatud ja peamine andmete tõrge kuvatakse sõltuvad pole pilveteenusesse. Kohalik kinnitatud mahud hetktõmmiste endiselt kopeeritakse nii, et pilveteenuses saab kasutada andmete mobility ja katastroofi taastamise eesmärgil varundamiseks pilveteenusesse. Lisaks saate muuta helitugevuse tüüp (st Teisenda mitmetasandilise mahud kohalikult kinnitatud mahud ja Teisenda kohalikult kinnitatud mahud mitmetasandilise). 

- **StorSimple virtuaalse seadme täiustused** – varem StorSimple 8000 sarja paigutatud virtuaalse seadme katastroofi taaskasutamiseks või arengu katse lahendus. Oli ainult üks mudel virtuaalse seadme (mudel 1100). 2 värskendusega kaks virtuaalse mudelit: 

     - 8010 (varasema nimega soovitud 1100) – muutmata; maht on 30 TB ja kasutab Azure standard salvestusruumi.
     - 8020 – on maht on 64 TB ja kasutab Azure Premium mälu paranenud jõudlus.

    On ühe VHD nii virtuaalse seadme mudelite (8010/8020). Virtuaalne seadme esmakordsel käivitamisel tuvastab platvormi parameetrid ja kehtib õige mudeli versioon.

- **Networking täiustused** – Update 2 sisaldab võrgu järgmisi täiustusi.

     - Mitu NICs saab lubada pilves, et Tõrkesiirde võib ilmneda juhul, kui on NIC nurjub.
     - Marsruutimise täiustused, fikseeritud mõõdikute Cloud lubatud plokid.
     - Nurjunud ressursside enne Tõrkesiirde online uuesti.
     - Uue teenuse tõrgete teatised.

- **Värskendamise täiustused** – Update 1,2 ja varasemad versioonid, StorSimple 8000 sarja värskendati kahe kanali kaudu: Windows Update'i rühmitamise, iSCSI, jne ja Microsoft Update kahendfaile ja püsivara.
    Värskenduse 2 kasutab Microsoft Update kõik värskendada paketid. See peaks viima vähe aega lappimine või failovers teha. 

- **Püsivara värskendused** – järgmine püsivara värskendused on kaasatud:
    - LSI: lsi_sas2.sys toote versioon 2.00.72.10
    - Ainult SSD (HDD värskendused): XMGG, XGEG, KZ50, F6C2 ja VR08

- **Aktiivne tugi** – Update 2 võimaldab Microsoftil täiendavad diagnostikateave seadme tõmmata. Kui meie meeskond toimingute tuvastab seadmed, mida teil on probleeme, oleme paremini valmis seadme teabe kogumine ja diagnoosimine probleeme. **Aktsepteerimine Update 2, lubate meile selle aktiivne toetada**.    
 

## <a name="issues-fixed-in-update-2"></a>Fikseeritud 2 probleemid

Järgmistes tabelites toodud probleeme, mis olid värskenduste 2 Kokkuvõte.    

| Ei. | Funktsioon | Probleem | Kehtib seadme | Kehtib virtuaalse seade |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Võrgu liidesed | Pärast täiendamist värskendus 1, teatatud StorSimple halduri teenuse, et Data2 ja Data3 pordid nurjus ühe kontrolleril. See probleem on lahendatud. | Jah | Ei |
| 2 | Värskendused | Pärast täiendamist värskendus 1 helisignaal teatiste ilmnenud Azure klassikaline portaalis mitmes seadmes. See probleem on lahendatud. | Jah | Ei |
| 3 | OpenStack autentimine | Pilveteenuse pakkuja nimega Openstack kasutamisel võib tõrketeade, et oma pilveteenuses autentimise string on liiga pikk. See on lahendatud. | Jah | Ei |


## <a name="known-issues-in-update-2"></a>Teadaolevad probleemid Update 2

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
| 20 |Kohalik kinnitatud mahud | Kui proovite teisendamise astmeline (loodud ja kloonitud Update 1,2 või varasemates) kohalikult kinnitatud helitugevust ja seadme hakkab otsa saama või on pilv katkestuste, siis korral kloon(ID) saab rikutud.| See probleem ilmneb ainult nende mahtu, mis on loodud ja kloonitud koos eelse Update 2 tarkvara. See peaks olema ka harv stsenaariumi.|
| 21 | Helitugevuse teisendamine | Värskendage ACRs, manustatud maht helitugevuse teisendamise ajal (mitmetasandilise soovite kohalikult kinnitatud või vastupidi). Värskendamine on ACRs võib põhjustada andmete rikkumist. | Vajadusel värskendage ACRs enne helitugevus teisendamise ja teha täiendavaid ACR värskendused, teisendamise ajal. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Selle domeenikontrolleri ja püsivara värskendused Update 2

See väljaanne värskendab juht- ja ketta püsivara seadmes.
 
- Lisateavet LSI püsivara värskendada, leiate Microsofti teabebaasi artiklist 3121900. 
- Lisateavet ketta püsivara värskendada, leiate Microsofti teabebaasi artiklist 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Virtuaalne seadme värskenduste Update 2

Värskendust ei saa rakendada virtuaalse seade. Uue virtuaalse seadmed on vaja luua. 

## <a name="next-step"></a>Järgmise juhise juurde

Siit saate teada, kuidas [installida 2](storsimple-install-update-2.md) StorSimple seadme.
