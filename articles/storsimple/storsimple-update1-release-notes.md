<properties 
   pageTitle="StorSimple 8000 seeria Update 1.2 väljalaskemärkmed | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple 8000 seeria Update 1.2 uusi funktsioone, probleemid ja lahendused."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>StorSimple 8000 seeria Update 1.2 väljalaskemärkmed  

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed kirjeldada uusi funktsioone ja kriitilised avatud probleemid StorSimple 8000 seeria Update 1.2. Sisaldavad ka loendi StorSimple tarkvara, draiver ja ketas püsivara uuendused selles versioonis. 

Update 1.2 saab rakendada mis tahes StorSimple seade Release (GA), värskendus 0,1, värskendus 0,2 või Värskenda 0,3 tarkvara. Update 1.2 pole saadaval, kui teie seade töötab 1 Värskenda või Värskenda 1.1. Kui teie seade töötab väljaanne (GA), palun aidata teil selle värskenduse installimist [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) .

Järgmises tabelis on loetletud värskendused 1, 1.1 ja 1.2 vastav seade tarkvara versioonid.

| Kui operatsioonisüsteemi värskendamine... | See on teie seadmesse tarkvara versioon. |
|---------------------|---------------------------------------|
| Värskendage 1.2          | 6.3.9600.17584                        |
| 1.1 värskendus          | 6.3.9600.17521                        |
| Värskendage 1.0          | 6.3.9600.17491                        |

Palun vaadake üle enne juurutamist värskenduse oma StorSimple lahendus on väljalaskemärkmed sisalduvat teavet. Lisateavet leiate teemast [Update 1.2 StorSimple seadme installimise](storsimple-install-update-1.md)kohta. 

>[AZURE.IMPORTANT]
> 
- Kulub ligikaudu 5 – 10 tundi selle värskenduse (sh Windowsi värskenduste). 
- Update 1.2 on tarkvara, LSI draiver ja ketta püsivara värskendused. Installimiseks järgige [Update 1.2 StorSimple seadmesse installida](storsimple-install-update-1.md).
- Uue keskkonda, te ei näe värskendused kohe, kuna me etappidega plaanida värskendusi. Mõne päeva jooksul uuesti Värskenduste otsimine kui need muutuvad kättesaadavaks varsti.


## <a name="whats-new-in-update-12"></a>Mis on uut rakenduses Update 1.2

Nende funktsioonide esmalt lubati Update 1, mis on piiratud hulk kasutajatele kättesaadavaks. Väljaandesse Update 1.2 enamik StorSimple kasutajad näeksid järgmised uued funktsioonid ja täiustused:

- **Migreerimise 5000-7000 seeria ja 8000 sarja seadmed** – see väljaanne tutvustab uue migreerimise funktsioon, mis võimaldab soovitud StorSimple 5000-7000 seeria seadme kasutajate StorSimple 8000 seeria füüsilise seadme või virtuaalse seadme oma andmete migreerimiseks. Migreerimise funktsiooni on kaks väärtust ettepanekud:                                                                  

    - **Süsteemi järjepidevuse**, võimaldab migreerimise 5000-7000 seeria seadmete 8000 seeria seadmetele olemasolevaid andmeid.
    - **Täiustatud funktsioon pakkumised 8000 seeria seadmed**, nt tõhus tsentraliseeritud mitme seadmete StorSimple halduri teenuse kaudu, on parem klassi riistvara ja värskendatud püsivara, virtuaalse seadmed, andmete mobility ja tulevaste teejuht funktsioonid.

    Täpsemat teavet migreerida StorSimple 5000-7000 sarja 8000 sarja seadmesse [migreerimisjuhend](http://www.microsoft.com/download/details.aspx?id=47322) viidata. 

- **Azure'i Government portaalis kättesaadavus** – StorSimple on nüüd saadaval Azure Governmenti portaalis. Vt juurutamise [StorSimple seadme Azure Governmenti portaalis](storsimple-deployment-walkthrough-gov.md).

- **Pilve väliselt tugi** – selle Pilve väliselt toetatud on Amazon S3, Amazon S3 RRS, HP ja OpenStack (beetaversioon).

- **Värskendamine, et uusim salvestusruumi API** -selles versioonis, StorSimple on värskendatud uusim Azure Storage teenusega API-d. Enne Update 1 Tarkvara versioonid töötavad StorSimple 8000 seeria seadmed (väljaanne, 0,1, 0,2 ja 0,3) kasutavad versioonid Azure salvestusruumi teenuse API vanemad kui 17 juuli 2009. 1 August 2016 värskendatud [teadaanne salvestusruumi teenuse versioonide eemaldamise kohta](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), nagu need API-d on aegunud. On oluline, et rakendate StorSimple 8000 seeria Update 1 enne 1 August 2016. Kui te ei tee, peatub StorSimple seadmete toimimist.

- **Tugi jaoks Zone liigsete mälu (ZRS)** – koos uuele versioonile üle uusimale versioonile salvestusruumi API-de StorSimple 8000 sarja toetab Zone liigsete mälu (ZRS) Lisaks kohalikult liigsete salvestusruumi (LRS) ja geograafilise liigne salvestusruumi (GRS). Vaadake käesoleva [artikli Azure'i talletussuvandite koondamise](../storage/storage-redundancy.md) ZRS üksikasjad.

- **Täiustatud juurutamine ja värskenduse kogemust** – see väljaanne, installimine ja Värskenda protsessid on täiustatud. Viisardi installimine on täiustatud tagasiside kasutajale kui võrgu konfigureerimise ja tulemüüri sätted on vale. Täiendavad diagnostika cmdlet-käsud on antud aitavad tõrkeotsingu võrgunduse seade. Vaadake [tõrkeotsingu juurutamise artiklis](storsimple-troubleshoot-deployment.md) uue diagnostika cmdlettide, kasutatakse tõrkeotsingu kohta lisateavet.

## <a name="issues-fixed-in-update-12"></a>Küsimused fikseeritud Update 1.2

Järgmises tabelis antakse ülevaade probleeme, mis olid värskenduste 1.2, 1.1 ja 1.    


| Ei. | Funktsioon | Probleem | Värskendus | Kehtib seadme | Kehtib virtuaalse seade |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Windows PowerShelli StorSimple | Kui kasutaja juurde kaugühenduse teel StorSimple seadme StorSimple Windows PowerShelli abil ja seejärel alustada häälestusviisardi, krahhi ilmnenud niipea, kui IP oli sisestada andmeid 0. Nüüd on see viga fikseeritud 1. | 1 värskendamine | Jah | Jah |
| 2 | Factory lähtestamine | Mõnel juhul, kui tegite factory lähtestamist StorSimple seadme sai ummikus ja kuvatakse teade: **Reset to factory on pooleli (etapp 8)**. See on juhtunud kui ajal cmdlet pooleli vajutatud klahvikombinatsiooni CTRL + C. See on nüüd veaparandused.| 1 värskendamine | Jah | Ei |
| 3 | Factory lähtestamine | Pärast nurjunud kahekordne kontrolleril factory lähtestamiseks lubati seadme registreerimine jätkata. Selle tulemusena on toetamata süsteemi konfiguratsioon. Update 1, kuvatakse tõrketeade ja registreerimist on blokeeritud seadmes, et on nurjunud factory lähtestada. | 1 värskendamine | Jah | Ei |
| 4 | Factory lähtestamine | Mõnel juhul false positiivne lahknevuse teatiste esitati. Vale lahknevuse teatiste luuakse enam seadmetes, kus töötab Update 1. | 1 värskendamine | Jah | Ei |
| 5 | Factory lähtestamine | Kui factory reset katkestati enne lõpuleviimist, seadme sisestatud taastamise režiimi ja ei luba teil juurdepääs Windows PowerShelli StorSimple. See on nüüd veaparandused. | 1 värskendamine | Jah | Ei |
| 6 | Katastroofiabi | Katastroofi taastamine (DR) viga on kinnitatud, kus DR nurjub ajal leitavust varukoopiate target seadmes. | 1 värskendamine | Jah | Jah |
| 7 | LED jälgimine | Teatud juhtudel jälgimise taga seadme LED ei märkinud õige olek. Sinine LED oli välja lülitatud. ANDMETE 0 ja 1 andmete LED olid vilkuvad, isegi kui liideste pole konfigureeritud. Probleem on lahendatud ja jälgimisega seotud LED näitab kohe õige olek.  | 1 värskendamine | Jah | Ei |
| 8 | LED jälgimine | Teatud juhtudel pärast värskenduse 1, helesinine aktiivne kontrolleril välja lülitatud muutes raske aktiivse kontrolleril tuvastamiseks. See probleem on lahendatud see paik väljaanne.| Värskendage 1.2 | Jah | Ei |
| 9 | Võrgu liidesed | Varasemates versioonides, võib StorSimple seadme konfigureeritud-marsruuditavaid lüüsi katkestada. Selles väljaandes marsruutimise meetermõõdustik andmete 0 on tehtud madalaim; Seetõttu isegi juhul, kui muud võrgu liidesed on pilv lubatud, suunatakse cloud liikluse seadme kaudu andmete 0. | 1 värskendamine | Jah | Jah | 
| 10 | Varukoopiate | Viga Update 1, mis põhjustas varukoopiaid 24 päeva pärast on fikseeritud paik versiooni 1.1 värskenduse. | 1.1 värskendus | Jah | Jah |
| 11 | Varukoopiate | Varasemate versioonide vea tulemuseks kehva jõudluse cloud hetktõmmiseid madal muutmine määr. See viga on fikseeritud see paik väljaanne.| Värskendage 1.2 | Jah | Jah |
| 12 | Värskendused | See paik väljaanne on fikseeritud Update 1, et teatatud nurjus täiendamine ja põhjustada kontrollerid taastamine režiimis, et viga.| Värskendage 1.2 | Jah | Jah |


## <a name="known-issues-in-update-12"></a>Teadaolevad probleemid Update 1.2

Järgmises tabelis antakse ülevaade selle väljaande teadaolevate probleemide.

| Ei. | Funktsioon | Probleem | Kommentaaride/lahendus | Kehtib seadme | Kehtib virtuaalse seade |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Ketas kvoorumi | Harvaesinevate eksemplari, kui ketta jaotise EBOD ruumi 8600 seadme enamik ei saa tulemuseks kvoorumit kettale, siis selle talletusmahtu saab ühenduseta. See jääb ühenduseta isegi juhul, kui selle ketast ühendatakse. | Peate taaskäivitage seade. Kui probleem ei lahene, pöörduge Microsoft Support järgmised toimingud. | Jah | Ei |
| 2 | Vale kontrolleril ID | Kui selle domeenikontrolleri asendamine toimub, domeenikontrolleri 0 võib kuvada selle domeenikontrolleri 1. Ajal kontrolleril väljavahetamine, kui pilt on laaditud omavahelistes sõlme kontrolleril ID kuvada algselt omavahelistes kontrolleril ID Harvaesinevate juhtudel selline käitumine võib näha ka pärast taaskäivitada. | Kasutajatoimingut ei vaja. Olukord lahendab ise, kui selle domeenikontrolleri asendamine on lõpule jõudnud. | Jah | Ei |
| 3 | Salvestusruumi kontod | Salvestusruumi teenuse abil konto salvestusruumi kustutada on mõni mittetoetatud stsenaariumi. See viib olukorda, kus kasutaja andmeid ei saa tuua. | Jah | Jah |
| 4 | Seadme Tõrkesiirde | Mitme failovers helitugevuse paaniümbrist allika samasse seadmesse target erinevate seadmete ei toetata. Seadme Tõrkesiirde ühe surnud seadmest, mitu seadet teeb helitugevuse ümbriste nurjus üle seadme esimesel kaotada andmete omandiõiguse. Pärast Tõrkesiirde, nende helitugevuse ümbriste kuvada või toimivad teisiti Azure klassikaline portaali kuvatava. | | Jah | Ei |
| 5 | Installimine | Ajal StorSimple adapterit SharePointi installi, tuleb sisestada seadme IP selleks installi lõpule.    | | Jah | Ei |
| 6 | Veebiteenuse puhverserveri | Kui teie web puhverserveri konfigureerimine on määratud protokoll HTTPS, mõjutab oma seadme-teenus side ja seadme katkestada. Tugiteenuste pakettide luuakse ka protsessi, tarbimine märgatavat ressursid oma seadmes. | Veenduge, et web puhverserveri URL on määratud protokoll HTTP. Lisateabe saamiseks minge [konfigureerimine veebiteenuse puhverserveri oma seade](storsimple-configure-web-proxy.md). | Jah | Ei |
| 7 | Veebiteenuse puhverserveri | Kui konfigureerimine ja lubada veebiteenuse puhverserveri registreeritud seadmes, siis pead taaskäivitage aktiivne kontrolleril seadmes. | | Jah | Ei |
| 8 | Kõrge cloud latentsus ja suur I/O töökoormus | Väga kõrge cloud latentsused (järjestuse sekundites) ja suur I/O töökoormus kombinatsiooni käivitamise StorSimple seadme seadme mahud minema halvenenud olekus ja selle väljundtoimingute võib nurjuda tõrketeatega "seade ei ole valmis" tõrge. | Peate käsitsi taaskäivitage seade kontrollerid või seadme Tõrkesiirde taastamine juhul teha. | Jah | Ei |
| 9 | Azure'i PowerShelli | Kui kasutate StorSimple cmdlet-käsk Get-AzureStorSimpleStorageAccountCredential **& #124; Valige – Object – esmalt 1 - ootamine** saate luua uue **VolumeContainer** objekti esimest objekti valimiseks soovitud cmdlet kuvab kõik objektid. | Murra cmdlet sulgudes järgmiselt: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Valige – Object – esmalt 1 - ootamine** | Jah | Jah |
| 10| Migreerimise | Kui mitme helitugevuse ümbriste edastatakse migreerimise jaoks, ETA uusima varundamiseks on ainult esimene helitugevuse container jaoks täpne. Lisaks hakkavad paralleelselt migreerimise pärast migreerimist esimese 4 varukoopiate esimese helitugevuse ümbrises. | Soovitame migreerite ühe helitugevuse container korraga. | Jah | Ei |
| 11| Migreerimise | Pärast taastada, ei lisata mahud varukoopia poliitika või virtuaalse ketta rühma. | Peate lisatakse need mahud varukoopia poliitika, et luua varukoopiaid. | Jah | Jah |
| 12| Migreerimise | Pärast migreerimise lõpuleviimist peate 5000/7000 sarja seadme juurde ei pääse migreeritud andmete ümbriste. | Soovitame, et kustutate migreeritud andmete ümbriste kui migreerimine on lõpule viidud ja kinnitatud. | Jah | Ei |
| 13| Klooni ja DR | StorSimple seadet, kus töötab Update 1 ei saa klooni või teha avariitaastet eelse update 1 tarkvara seade. | Peate värskendama target seadme värskendamine 1 järgmiste toimingute lubamiseks | Jah | Jah |
| 14 | Migreerimise | Kui seal on seotud mahud helitugevuse rühmade ei pruugi konfiguratsiooni varundus migreerimise 5000-7000 sarja seadmes. | Tühja helitugevuse rühmad koos seotud mahud kustutamine ja proovige uuesti konfiguratsiooni varukoopia.| Jah | Ei |

## <a name="physical-device-updates-in-update-12"></a>Update 1.2 füüsilise seadme värskendused

Kui värskendada 1.2 rakendatakse füüsilise seadmega (töötab versioonides värskendada 1), tarkvara versiooni muutub 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Selle domeenikontrolleri ja püsivara värskendused Update 1.2

See väljaanne värskendab juht- ja ketta püsivara seadmes.
 
- SAS kontrolleril värskenduste kohta leiate lisateavet teemast [LSI SAS kontrollerid Microsoft Azure StorSimple seadme värskendamine 1](https://support.microsoft.com/kb/3043005). 

- Lisateavet ketta püsivara update, vt [ketta püsivara värskendamine 1 Microsoft Azure StorSimple seadme](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Virtuaalne seadme värskenduste Update 1.2

Värskendust ei saa rakendada virtuaalse seade. Uue virtuaalse seadmed on vaja luua. 

## <a name="next-steps"></a>Järgmised sammud

- [Installige värskendus 1.2 seadmes](storsimple-install-update-1.md).
 
