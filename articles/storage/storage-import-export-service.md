<properties
    pageTitle="Import/eksport abil andmete edastamiseks bloobimälu | Microsoft Azure'i"
    description="Saate teada, kuidas luua importimine ja eksportimine töö Azure'i klassikaline portaalis Bloobivahemälu salvestusruumi andmete edastamiseks."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Microsoft Azure'i impordi/ekspordi teenust kasutada andmete edastamiseks bloobimälu

## <a name="overview"></a>Ülevaade

Azure'i impordi/ekspordi teenus võimaldab teil turvaliselt edastamiseks suurte andmehulkade Azure'i bloobimälu kõvaketta draivid on Azure andmekeskuse saatmine. Saate selle teenuse Azure'i bloobimälu andmete edastamiseks kõvaketta draivid ja saata oma kohapealse saidi. See teenus on sobiv olukordades soovite edastada mitu TBs andmed või Azure, kuid üles või alla võrgu kaudu ei ole võimalik piiratud läbilaskevõime või kõrge võrgu kulude tõttu.

Teenus nõuab kõvaketta draivid peaks bit locker krüptitud andmete turvalisuse. Teenuse toetab klassikaline salvestusruumi kontod Esita avaliku Azure'i kõikides piirkondades. Saadate peab kõvaketta draivid ühele toetatud selle artikli määratud kohast.

Selles artiklis on teada Lisateavet Azure impordi/ekspordi teenuse ja kuidas saata draivid andmete kopeerimine ja sealt Azure'i bloobimälu.

> [AZURE.IMPORTANT] Saate luua ja hallata impordi ja ekspordi töö klassikaline Storage klassikaline portaalis või [teenuse impordi/ekspordi REST API-de](http://go.microsoft.com/fwlink/?LinkID=329099)abil. Praegu ei toetata ressursihaldur salvestusruumi kontod.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Millal tuleks Azure impordi/ekspordi teenust kasutada?

Te saate kaaluge Azure impordi/ekspordi teenuse üleslaadimisel või üle võrgu andmete allalaadimine on liiga aeglane või saada täiendavad võrgu läbilaskevõime on maksumus takistuseks.

Saate selle teenuse stsenaariumide näiteks:

- Migreerimine andmete pilve: suurte andmehulkade Azure kiiresti liikuda ja tõhusalt maksumus.
- Sisu jaotuse: kiiresti andmeid saata oma kliendi saidid.
- Varundus: Võtta varukoopiaid kohapealse andmed talletatakse Azure'i bloobimälu.
- Andmete taastamine: taastada suurt hulka andmeid, mis on talletatud bloobimälu ja selle kätte oma kohapealse asukohta.

## <a name="pre-requisites"></a>Eeltingimused

Selles jaotises Meil on loetletud eeltingimused, mis on vaja seda teenust kasutada. Palun vaadake üle nende hoolega enne saatmine teie draivid.

### <a name="storage-account"></a>Salvestusruumi konto

Azure olemasolevale tellimusele ja ühe või mitme **klassikaline** salvestusruumi kontod impordi/ekspordi teenuse kasutamiseks peab teil olema. Iga töö võib kasutada edastamiseks andmed või ainult ühe klassikaline salvestusruumi konto kaudu. Teisisõnu, ei saa ühe impordi/ekspordi tööd asuvad mitme salvestusruumi kontod. Uue salvestusruumi konto loomise kohta leiate teavet teemast [salvestusruumi konto loomine](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>Bloobimälu tüübid

Saate andmete kopeerimine **Blokeeri** plekid või **lehe** plekid teenuse Azure impordi/ekspordi. Vastupidi, saate eksportida vaid **plokk** plekid, **lehe** plekid või **Lisa** plekid Azure salvestusruumist selle teenuse abil.

### <a name="job"></a>Töö

Importimine või eksportimine bloobimälu alustamiseks loomisel tööd. Töö saab või mõne eksportimine mõnda importimine töö:

- Teil on kohapealse abil plekid Azure storage konto andmete edastamiseks looge on töö.
- Luua ka ekspordi töö, kui soovite edastada nimega plekid teie salvestusruumi konto kõvaketas, mis saadetakse teile praegu talletatud andmed.

Töö loomisel saate teavitage impordi/ekspordi teenuse, et te saatmine ühe või mitme kõvaketas on Azure andmekeskuse.

- Impordi tööd, mida saatmine kõvaketas, mis sisaldab teie andmeid.
- Ekspordi tööd, mida saatmine tühja kõvaketas.
- Saate saata kuni 10 kõvaketta draivid töökoha kohta.

Saate luua impordi või ekspordi töö [klassikaline portaali](https://manage.windowsazure.com/) või [Azure Storage impordi/ekspordi REST API](http://go.microsoft.com/fwlink/?LinkID=329099)abil.

### <a name="client-tool"></a>Kliendi tööriist

Esimene samm on **importimine** töökohtade loomisel on teie ketas, mis saadetakse ettevalmistamine impordi. Ettevalmistamiseks arvuti kettale, peate ühenduse kohaliku serveri ja käitada Azure impordi/ekspordi kliendi kohalikus serveris. See tööriist kliendi hõlbustab andmete kopeerimine ketas, krüptimise draivile andmeid ning luues ketas töölehe failid.

Töölehe faile talletada põhiteavet oma töö ja ketas, nt ketas järjenumber ja salvestusruumikonto nimi. Selle töölehe fail on salvestatud ei ketas. Seda kasutatakse impordi töö loomise ajal. Samm-sammult andmed töökohtade kohta on esitatud selle artikli.

Kliendi tööriista ühildub ainult 64-bitise Windowsi operatsioonisüsteemi. Teatud OS versioonides toetatud jaotisest [operatsioonisüsteem](#operating-system) .

Laadige alla [Azure impordi/ekspordi kliendi tööriista](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409)uusima versiooni. Tööriista Azure impordi/ekspordi kasutamise kohta leiate lisateavet teemast [Azure impordi/ekspordi tööriista viide](http://go.microsoft.com/fwlink/?LinkId=329032).

### <a name="hard-disk-drives"></a>Kõvaketta draivid

Ainult 3,5 tolli SATA III ja II sisemine kõvaketas on toetatud kasutamine teenusega impordi/ekspordi. Saate kasutada kõvaketas kuni 10TB.
Impordi töö, töödeldakse ainult esimese andmete maht kettal. Andmete maht peab olema vormindatud NTFS.
Andmete kopeerimisel kõvakettale, saate selle otse SATA kasutamisega manustada või saate selle abil välise USB SATA II/III adapterit manustada. Soovitame kasutada ühte järgmistest välise USB SATA II/III kvaliteediga:

- Anker 68UPSATAA 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon oleme XT HC

Kui teil on muundur, mis pole loendis, proovige oma muunduri kasutamine ette valmistada ketas ja vaadake, kas see töötab enne ostmist toetatud muunduri tööriista Azure impordi/ekspordi.

> [AZURE.IMPORTANT] Välise kõvaketta draivid kaasas sisseehitatud USB-adapterit, see teenus pole toetatud. Samuti ei saa kasutada ketast, välise Kõvaketta kesta sees; Ärge saatke välise HDD-d.

### <a name="encryption"></a>Krüptimine

Ketas andmeid peab krüptitud BitLockeri abil. See kaitseb teie andmeid ajal teel.

Impordi tööde, on kaks võimalust teha krüptimine. Esimene viis on kasutada funktsiooni / Krüpti parameeter, kui kliendi tööriista ketas koostamise ajal. Teine võimalus on lubada BitLockeri krüptimise käsitsi kettadraivi ja määrake krüptovõtme kliendi käsureal tööriista ketas koostamise ajal.

Kui teie andmed kopeeritakse draivid, ekspordi tööde, krüptida teenuse BitLockeri abil saatmise saate tagasi enne ketas. Krüptovõtme pakutakse teile klassikaline portaali kaudu.  

### <a name="operating-system"></a>Operatsioonisüsteem

Saate ühte järgmistest 64-bitiste opsüsteemide jaoks ettevalmistamiseks kõvaketas enne saatmine ketas Azure'i Azure impordi/ekspordi tööriista abil:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Enterprise Windows 8.1, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Kõik need opsüsteemid ei toeta BitLockeri.

> [AZURE.IMPORTANT] <sup>1</sup> Kui kasutate Windows 10 arvuti kõvakettale ettevalmistamiseks, laadige uusima versiooni tööriista Azure impordi/ekspordi.

### <a name="locations"></a>Asukohad

Azure'i impordi/ekspordi teenuse toetab andmete kopeerimise ja sealt kõik avaliku Azure'i salvestusruumi kontod. Saate saata kõvaketta draivid ühega järgmistest kohtadest. Salvestusruumi konto on Azure avalikus kohas, mis on määratud siin, antakse mõne alternatiivse saatmine asukoht loomisel tööd, kasutades klassikaline portaali või impordi/ekspordi REST API-ga.

Toetatud saatmise asukohad:

- Ida-USA

- Lääne USA.

- Ida-USA 2

- Kesk-USA

- Põhja Kesk-USA

- Lõuna-, Kesk-USA

- Põhja-Euroopa

- Lääne Euroopa

- Ida-Aasia

- Kagu-Aasia

- Austraalia Ida

- Austraalia kodutee

- Jaapan Lääne

- Jaapan Ida

- Keskse India


### <a name="shipping"></a>Saatmine

**Saatmine draivid andmekeskuse:**

On importimine või eksportimine töökohtade loomisel teile antakse postiaadress ühe saata oma draivid toetatud asukohad. Saatmine aadressile sõltub teie salvestusruumi konto asukoha, kuid see ei pruugi olla sama, mis teie konto talletuskoht.

Saate saata oma draivid saatmine aadressil vedajate nagu FedEx, DHL, UPS või meile posti teenus.

**Saatmine draivid andmekeskuse kaudu:**

On importimine või eksportimine töökohtade loomisel peate sisestama saatja-aadressi Microsoft kasutada, kui saatmine draivid tagasi siis, kui teie töö on lõpule jõudnud. Veenduge, et esitate kehtiv saatja-aadressi vältimiseks töötlemine viivitust.

Peate määrama lubatud FedEx või DHL carrier kood kasutatakse Microsoft draivid uuesti saatmine. FedEx konto number on vaja saatmine tagasi asukohtadest USA ja Euroopa draivid. DHL konto number on vaja saatmine draivid tagasi Aasia ja Austraalia asukohtadest. Kui teil on üks saate luua [DHL](http://www.dhl.com/) (Aasia ja Austraalia) carrier konto või [FedEx](http://www.fedex.com/us/oadr/) (USA ja Euroopa) jaoks. Kui teil on juba carrier konto number, veenduge, et see on kehtiv.

Klõpsake saatmine oma pakette, peate järgima terminite veebisaidil [Microsoft Azure'i teenuse tingimused](https://azure.microsoft.com/support/legal/services-terms/).

> [AZURE.IMPORTANT] Pange tähele, et teil on saatmine füüsilise meediumi vajalikud üle rahvusvaheline piiri. Olete tagama, et teie füüsilise meedia ja andmed on imporditud ja/või kohaldatavate õigusaktidega ekspordi. Enne saatmine füüsilise meedia, pöörduge oma nõustaja, et teie meediumid ja andmed kas õiguslikult saadetakse tuvastatud andmekeskuse kinnitamiseks. See aitab tagada, et see jõuab Microsoft õigel ajal. Näiteks mõni pakett, mis on rist rahvusvaheline äärised peab kommertsarve lisatakse pakett (välja arvatud juhul, kui ületavate äärised Euroopa Liidus). Teil võib arve carrier veebisaidilt täidetud koopia välja printida. Näide: äriliseks arve on [DHL Kommertsarve] (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) või [FedEx Kommertsarve](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Veenduge, et Microsoft ei ole osutatud olevale nimega.

## <a name="how-does-the-azure-importexport-service-work"></a>Azure impordi/ekspordi service tööpõhimõtted

Oma kohapealse saidi ja Azure'i bloobimälu töökohtade loomine ja saatmine kõvaketta draivid on Azure andmekeskuse Azure impordi/ekspordi teenuse abil saate edastada andmed. Iga kõvakettadraivi saadate on seotud ühe töö. Iga töö on seotud ühe salvestusruumi konto. Vaadake üle [eeltingimused jaotist](#pre-requisites) hoolega õppida üksikasjad selle teenuse nagu toetatud bloobimälu tüübid, ketta tüüpi, asukohad ja saatmine.

Selles jaotises kirjeldatakse kõrge etappe importimine ja eksportimine tööde haldamine. Allpool olevat [jaotist Kiirkäivituse](#quick-start)pakume üksikasjalikke juhiseid luua impordi ja ekspordi töö.

### <a name="inside-an-import-job"></a>Sees on töö

Kõrge, on töö hõlmab järgmist:

- Andmete importimiseks ja draivid peate arvu määratlemine.
- Leidke sihtkoha plekid bloobimälu oma andmete jaoks.
- Ühe või mitme kõvaketta draivid oma andmeid kopeerida ja neid BitLockeri abil krüptida Azure impordi/ekspordi tööriista abil.  
- Luua mõne töö target klassikaline salvestusruumi kontole juurdepääsemine klassikaline portaali või impordi/ekspordi REST API-ga. Klassikaline portaali kasutamisel ketas töölehe faile üles laadida.
- Sisestage saatja-aadressi ja konto carrier arvu saatmine draivid naasmiseks saate kasutada.
- Saata kõvakettal kettadraivide aadressile saatmise töö loomise ajal.
- Värskendada kohaletoimetamisega jälgimise arv importimine töö üksikasjad ja esitage importimine töö.
- Draivid on saanud ja Azure andmekeskuses töödeldud.
- Draivid saadetakse saatja aadressi importimine töö carrier konto abil.

    ![Joonis 1:Import töö meilivoo](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>Sees on ekspordi tööd

Kõrge, on ekspordi töökohtade hõlmab järgmist:

- Andmete eksportimisel ja draivid peate arvu määratlemine.
- Andmeallika plekid või container teed bloobimälu oma andmete tuvastamine.
- Luua ka ekspordi töö allika salvestusruumi kontole juurdepääsemine klassikaline portaali või impordi/ekspordi REST API-ga.
- Määrake allikas plekid või container teed andmete eksportimine töö.
- Saatmine draivid naasmiseks saate kasutada saatja-aadressi ja carrier kontonumbri pakkumiseks.
- Saata kõvakettal kettadraivide aadressile saatmise töö loomise ajal.
- Värskendage kohaletoimetamisega jälgimise arv ekspordi töö üksikasjad ja esitada ekspordi töö.
- Draivid on saanud ja Azure andmekeskuses töödeldud.
- Draivid on krüptitud BitLockeri abil; Klassikaline portaali kaudu saadaolevad võtmed.  
- Draivid saadetakse saatja aadressi importimine töö carrier konto abil.

    ![Joonis 2:Export töö meilivoo](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>Töö oleku vaatamine

Saate jälgida olekut impordi või ekspordi töö klassikaline portaalist. Liikuge klassikaline portaali konto salvestusruumi ja klõpsake vahekaarti **Impordi/ekspordi** . Lehel kuvatakse teie tööde loendit. Saate filtreerida loendi olek, töö nimi, töö tüüp või jälgimise numbri.

Kuvatakse üks järgmistest töö olekutest sõltuvalt sellest, kus teie ketas on protsess.

| Töö oleku vaatamine   | Kirjeldus                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Loomine     | Oma töö on loodud, kuid teil pole veel esitatud teabe saatmine.                                                                                                                                                                                                                                                                                                |
| Saatmine     | Oma töö loomist ja teie sisestatud teabe saatmine. **Märkus**: kui ketas on kohale toimetatud Azure andmekeskuse, oleku võib olekuna kuvatakse "Saatmine" juba mõnda aega. Kui teenus hakkab andmete kopeerimine, olekuks "Edastamine". Kui soovite näha täpsemale olekut arvuti kettale, saate kasutada impordi/ekspordi REST API-ga. |
| Edastamine | Andmete üle arvuti kõvaketta sisu (jaoks mõne importimine töö) või kõvakettale (jaoks mõne ekspordi töö).                                                                                                                                                                                                                                                                 |
| Pakendit    | Oma andmete kandmine on lõpule viidud ja arvuti kõvaketta sisu on valmis saatmine tagasi.                                                                                                                                                                                                                                                                             |
| Lõpuleviimine     | Arvuti kõvaketta sisu saadeti teile.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Protsessi töö aeg

Kui palju aega kulub protsess on impordi/ekspordi töö erineb sõltuvalt tegurid saatmine kellaaeg, nagu näiteks töö tüüp, -tüübi ja kopeeritava andmete maht ja ketast, mis on esitatud suurust. Import/eksport teenus pole SLA. REST API abil saate töö jälgimiseks lähemalt. Loendi töö toiming, mis näitab eksemplari edenemist on protsendimäärana täieliku parameeter. Jõuda meile kui teil on vaja hinnang lõpuleviimiseks ajastitöö kriitilised impordi/ekspordi.

### <a name="pricing"></a>Hinnad

**Drive käsitlemise tasu**

On ketas töötlemine tasu iga draivi impordi osana töödeldud või töö eksport. Üksikasju vt [Azure'i hinnad impordi/ekspordi](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Kohaletoimetamise kulud**

Kui saadate draivid Azure, maksate toimetaja hind saatmine carrier. Kui Microsoft annab teile draivid, saatmine kulude tasumist töökohtade loomise ajal andnud carrier kontole.

**Tehingu kulud**

Kui andmete importimise bloobimälu on pole tehingu kulud. Standardne sealt kulude kohaldatakse andmete eksportimisel bloobimälu. Tehingu kulude kohta leiate lisateavet teemast [andmete edastamiseks hinnad.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Lühijuhend

Selles jaotises pakume üksikasjalikke juhiseid impordi ja ekspordi on töökohtade loomise kohta. Veenduge, et vastate kõik [eeltingimused](#pre-requisites) enne edasi.

## <a name="how-to-create-an-import-job"></a>Kuidas luua mõne töö?

Luua mõne töö kopeerida andmed kontole Azure storage kõvaketas alusel määratud andmekeskuse andmeid sisaldav ühe või mitme draivid. Importimine töö loob üksikasjade kõvaketta draivid, andmed kopeerida, suunata salvestusruumi konto ja saatmine teabe teenusesse Azure impordi/ekspordi. Loomine on töö on kolme järgmist toimingut. Esmalt koostada oma draivid Azure impordi/ekspordi kliendi tööriista abil. Teiseks edastamine importimine töö, mis klassikaline portaalis. Kolmas, saata draivid aadressile saatmise töö loomise ajal ja oma töö üksikasjad uuendage saatmine.   

> [AZURE.IMPORTANT] Saate esitada ainult ühe töö salvestusruumi konto kohta. Iga ketas saadate saab importida ühe salvestusruumi kontoga. Näiteks Oletame, et soovite andmete importimine kahe salvestusruumi kontod. Peate kasutama eraldi kõvaketta draivid iga salvestusruumi konto ja luua eraldi töökohta salvestusruumi konto kohta.

### <a name="prepare-your-drives"></a>Teie draivid ettevalmistamine

Kui impordite andmeid Azure impordi/ekspordi teenuse kasutamise esimene samm on valmistada oma draivid Azure impordi/ekspordi kliendi tööriista abil. Järgige oma draivid ettevalmistamiseks alltoodud juhiseid.

1.  Leidke andmete importimist. See võib olla kataloogid ja autonoomse failide kohalike serveris või võrgukettal.  

2.  Määratleda draivid peate sõltuvalt andmete kogumaht. Hankida vajaliku arvu 3.5 tolli SATA III ja II kõvaketta draivid.

3.  Sihtrakenduse salvestusruumi konto, nõu ja virtuaalkaustad plekid tuvastada.

4.  Määratlege kataloogid ja/või iga kõvakettadraivi kopeeritakse eraldi failid.

5.  Ühe või mitme kõvaketas oma andmete kopeerimisel [Azure impordi/ekspordi tööriista](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) abil.

    - Kopeeri seansid allika andmete kopeerimiseks kõvakettal kettadraivide loob Azure impordi/ekspordi tööriist. Seansi ühte eksemplari kasutada tööriista saate kopeerida ühe directory koos selle alamkatalooge või ühe faili.

    - Peate mitme eksemplari seansid kui teie lähteandmed ulatub üle mitme kataloogide.

    - Peaksite iga kõvakettadraivi vajavad vähemalt üks koopia seanss.

6.  Saate määrata selle / krüptida BitLockeri krüptimine arvuti kõvakettal parameeter. Teise võimalusena võib ka lubamine BitLockeri krüptimise käsitsi arvuti kõvakettale ja lisage vajadusel võti tööriista käitamise ajal.

7.  Azure'i impordi/ekspordi tööriist loob ketas töölehe faili iga draivi, kuna see on valmis. Ketas töölehe fail on salvestatud teie arvutis pole kettadraivi ise. Töölehe faili üles laadida, kui loote importimine töö. Ketas töölehe fail sisaldab ketas ID ja BitLockeri võti, samuti muud teavet ketas.
**Tähtis**: iga kõvakettadraivi peaksite tulemuseks töölehe faili. Klassikaline portaalis importimine töö loomisel tuleb kõik draivid, mis on osa selle töö töölehe failid üles laadida. Töölehe faile ei töödelda ilma draivid.

8.  Muuta andmete kettadraivide kõvakettal või töölehe fail pärast ketta ettevalmistamine.

Allpool on käsud ja näited ettevalmistamine kõvakettadraiv Azure impordi/ekspordi kliendi tööriista abil.

Azure'i impordi/ekspordi kliendi tööriista PrepImport käsk esimese Kopeeri seansi kataloogi kopeerimiseks:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Näide:**

Järgmises näites on kopeerimise kõik failid ja sub kataloogide kaudu H:\Video arvuti kõvakettale paigaldatud x. Salvestusruumi konto võti ja üheks salvestusruumi ümbris nimega video määratud sihtkohta salvestusruumi konto andmeid importida. Kui salvestusruumi-ümbrisest puudub, luuakse see. See käsk ka vormindada ja krüptimine target arvuti kõvakettale.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure'i impordi/ekspordi klient tööriista PrepImport käsu edaspidised Kopeeri seansid kataloogi kopeerimiseks:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Edaspidised Kopeeri seansid sama arvuti kõvakettale, määrata sama töölehe nime ja sisestage uus seanss ID; ei ole vaja salvestusruumi konto võti ja target ketas uuesti sisestada ega abil vormindamine või draivi krüptida. Selles näites me on kopeerida H:\Photo kausta ja selle sub kataloogide sama target kettale, salvestusruumi ümbris nimega foto.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure'i impordi/ekspordi kliendi tööriista PrepImport käsk Kopeeri esimese seansi faili kopeerimine:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Azure'i impordi/ekspordi kliendi tööriista PrepImport käsu edaspidised Kopeeri seansside saate faile kopeerida:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Meeles**: vaikimisi imporditakse andmed Blokeeri plekid. Saate importida andmed lehe plekid /BlobType parameeter. Näiteks kui impordite VHD faile, mida saab paigaldatud nimega ketast on Azure VM, peate importima need lehe plekid nimega. Kui te pole kindel, millist bloobimälu tüüpi kasutada, saate määrata /blobType:auto ja aitame määratleda õige. Sel juhul kõik vhd ja vhdx failid imporditakse lehe plekid ja ülejäänud imporditakse Blokeeri plekid.

Üksikasjalikumat teavet [Ettevalmistamine kõvaketta draivid importimiseks](https://msdn.microsoft.com/library/dn529089.aspx)Azure impordi/ekspordi kliendi tööriista abil.

Vaadake ka [Valimi töövoo ettevalmistamine kõvaketta draivid impordi tööd](https://msdn.microsoft.com/library/dn529097.aspx) üksikasjalikud juhised.  

### <a name="create-the-import-job"></a>Impordi töö loomine

1.  Kui olete valmis oma ketas, liikuge [klassikaline portaali](https://manage.windowsazure.com) konto salvestusruumi ja vaadata armatuurlaud. Jaotises **Kiirülevaate lühidalt**, klõpsake nuppu **Loo on töö**. Vaadake juhiseid ja märkige ruut näitavad, kas olete valmis oma kettale ja teil on saadaval ketas töölehe fail.

2.  Samm 1, sisestage see importimine töö ja kehtiv saatja-aadressi eest vastutav isik kontaktteave. Kui soovite salvestada Paljusõnaline logifailid andmete importimine töö, ruudu salvestamiseks **Paljusõnaline logifailid minu 'waimportexport' bloobimälu ümbrises**.

3.  Samm 2, laadige sammu ketas koostamise ajal saadava ketas töölehe failid. Peate iga ketas, kuhu olete valmis ühe faili üleslaadimine.

    ![Samm 3 importimine töö - loomine](./media/storage-import-export-service/import-job-03.png)

4.  Samm 3, sisestage importimine töö kirjeldav nimi. Pange tähele, et teie sisestatud nimi võib sisaldada ainult väiketähed, arve, sidekriipse ja allakriipsutatud märkideks, peavad algama tähega ning ei tohi sisaldada tühikuid. Kasutage nime, saate valida oma töö jälgimiseks, kui nad on pooleli ja kui need on lõpule viidud.

    Järgmisena valige loendist oma andmepiirkond keskele. Andmepiirkond keskmist näitab andmekeskuse ja aadress, millele tuleb saata oma paketi. Vt lisateavet allpool KKK.

5.  Samm 4, valige loendist oma saatja vedaja ja sisestage carrier konto number. Microsoft kasutab seda kontot draivid teile saata, kui tööpäevad importimine on lõpule viidud.

    Kui teil on jälgimise number, valige loendist oma kohaletoimetamise vedaja ja sisestage jälgimise number.

    Kui te ei jälgimise numbri veel, valige **I annab mu saatmise teave selle impordi töö, kui mul on minu pakett saadetud**, siis impordi protsessi lõpuleviimiseks.

6. Sisestage jälgimise number, kui teil on saadetud teie paketi saatja **Impordi/ekspordi** lehele konto salvestusruumi klassikaline portaalis, valige loendist oma töö ja valige **Saatmise teave**. Liikuge viisardiga ja sisestage jälgimise number toiminguga 2.

    Kui jälgimise numbri loomise töö 2 nädala jooksul värskendatud töö aegub.

    Kui töö on loomine, saatmine või edastamine olekus, saate värskendada ka carrier konto number toiminguga 2 viisardi. Pärast töö on pakendit olekusse, ei saa värskendada oma carrier kontonumbri selle tööle.

7. Saate jälgida töö edenemine, portaali armatuurlaual. Vaadake, mida iga tööd riigi eelmises jaotises tähendab, [töö oleku vaatamine](#viewing-your-job-status).

## <a name="how-to-create-an-export-job"></a>Kuidas luua ka ekspordi töö?

Luua ka ekspordi töö impordi/ekspordi teenuse teatada, et teil tuleb saatmine üks või mitu tühja draivid andmekeskuse, nii, et andmeid saate eksportida salvestusruumi kontolt draivid ja draivid siis saadetakse teile.

### <a name="prepare-your-drives"></a>Teie draivid ettevalmistamine

Järgmised enne kontrolli on soovitatav seda oma draivid ekspordi tööd ettevalmistamine:

1. Kontrollige ketast nõutav Azure impordi/ekspordi tööriista PreviewExport käsuga arvu. Lisateabe saamiseks vt [Eelvaate Drive kasutus eksportimine tööd](https://msdn.microsoft.com/library/azure/dn722414.aspx). See aitab teil eelvaate draivi kasutamist plekid valisite, põhineb kavatsete kasutada draivid suurust.

2. Kontrollige, et te saate lugemis-ja kirjutamisõigusega kõvakettale, mis saadetakse ekspordi töö.

### <a name="create-the-export-job"></a>Ekspordi töö loomine

1.  Luua ka ekspordi töö, liikuge [klassikaline portaali](https://manage.windowsazure.com)konto salvestusruumi ja vaadata armatuurlaud. Jaotises **Kiirülevaate lühidalt**, klõpsake nuppu **Loo on eksportimine töökohtade** ja jätkake viisardiga.

2.  Samm 2, sisestage selle ekspordi töö eest vastutav isik kontaktteave. Kui soovite salvestada Paljusõnaline logifailid andmete eksportimine töö, ruudu salvestamiseks **Paljusõnaline logifailid minu 'waimportexport' bloobimälu ümbrises**.

3.  Samm 3 määrata, millised bloobimälu soovite eksportida salvestusruumi kontolt tühi ketas või draivid. Soovi korral saate eksportida kõik bloobimälu andmed salvestusruumi konto või saate määrata, mis plekid või määrab plekid eksportida.

    Bloobimälu, eksportida määramiseks kasutage Vaateselektori **Võrdne** ja määrake suhteline tee bloobimälu, alustades container nimi. Määrake juur-ümbrisest *$root* abil.

    Kõik plekid, alustades eesliite määramiseks kasutage Vaateselektori **Algab** ja määrake eesliite, alustades kaldkriips /". Eesliite võib eesliide container nimi, container täielik nimi või eesliide bloobimälu nime, millele järgneb container täieliku nime.

    Järgmises tabelis on kehtiv bloobimälu teed näited:

    Valija|Tee bloobimälu|Kirjeldus
    ---|---|---
    Algab|/|Ekspordib kõik plekid salvestusruumi konto
    Algab|/ $root /|Ekspordib kõik plekid juurkausta ümbrises
    Algab|/Book|Ekspordib kõik plekid ümbrises, mis algab eesliite **aadressiraamatust**
    Algab|/Music/|Ekspordib kõik plekid container **muusika**
    Algab|/ muusika/armastus|Ekspordib kõik plekid container **muusikat** , mis algavad eesliite **kuulda**
    Võrdne väärtusega|$root/logo.bmp|Ekspordi Bloobivahemälu **logo.bmp** juurkausta ümbrises
    Võrdne väärtusega|Videos/Story.mp4|Ekspordi Bloobivahemälu **story.mp4** container **videod**

    Peate sisestama bloobimälu teede kehtiv vormingutes tõrgete vältimiseks töötlemine, nagu on näidatud selles kuvatõmmis.

    ![Samm 3 ekspordi töö - loomine](./media/storage-import-export-service/export-job-03.png)


4.  Samm 4, sisestage ekspordi töö kirjeldav nimi. Teie sisestatud nimi võib sisaldada ainult väiketähed, arve, sidekriipse ja allakriipsutatud märkideks, peavad algama tähega ning ei tohi sisaldada tühikuid.

    Andmepiirkond keskmist näitab andmekeskuse, millele tuleb saata oma paketi. Vt lisateavet allpool KKK.

5.  Samm 5, valige loendist oma saatja vedaja ja sisestage carrier konto number. Microsoft kasutab selle konto pärast tööpäevad ekspordi lõpuleviimist oma draivid teile saata.

    Kui teil on jälgimise number, valige loendist oma kohaletoimetamise vedaja ja sisestage jälgimise number.

    Kui te ei jälgimise numbri veel, valige **I annab mu saatmise teave selle ekspordi töö, kui mul on minu pakett saadetud**, siis täitke eksportimise käigus.

6. Sisestage jälgimise number, kui teil on saadetud teie paketi saatja **Impordi/ekspordi** lehele konto salvestusruumi klassikaline portaalis, valige loendist oma töö ja valige **Saatmise teave**. Liikuge viisardiga ja sisestage jälgimise number toiminguga 2.

    Kui jälgimise numbri loomise töö 2 nädala jooksul värskendatud töö aegub.

    Kui töö on loomine, saatmine või edastamine olekus, saate värskendada ka carrier konto number toiminguga 2 viisardi. Pärast töö on pakendit olekusse, ei saa värskendada oma carrier kontonumbri selle tööle.

    > [AZURE.NOTE] Kui eksporditava bloobimälu kasutamise ajal kopeerimine kõvakettale, Azure impordi/ekspordi teenuse soovitud bloobimälu hetktõmmis ja kopeerige hetktõmmis.

7.  Saate jälgida töö edenemise armatuurlaual klassikaline portaalis. Vt mida iga töö oleku tähendab eelmises jaotises sisse "vaatamine töö oleku".

8.  Pärast saate eksporditud andmete draivid, saate vaadata ja kopeerige loodud teenuse draivi BitLockeri võtmed. Liikuge klassikaline portaali konto salvestusruumi ja klõpsake vahekaarti impordi/ekspordi. Valige loendist oma ekspordi töö ja klõpsake nuppu Kuva võtmed. Klahvid BitLockeri abil kuvada, nagu allpool näidatud:

    ![Kuva BitLockeri võtmed ekspordi töö](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Palun läbida jaotisest KKK all, nagu see hõlmab kõige levinumad küsimused kliendid tekkida, kui selle teenuse abil.

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused ##


**Kui kaua võtab pärast minu draividel jõuab andmekeskuse minu andmeid kopeerida?**

Aeg kopeerimiseks erineb sõltuvalt eri tegurid, nagu töö tüüp, esitada tüüp ja andmete kopeerimise, ketast suurus suurust, ja olemasoleva töökoormus. See võib erineda paar päeva paar nädalat, sõltuvalt teguritest. Seetõttu on raske üldine hinnang anda. Teenuse proovib optimeerida oma töö kopeerides mitme draivid samal ajal, kui võimalik. Kui teil on aega kriitiliste impordi/ekspordi töö, jõuda meile prognoosida.

**Kui Azure impordi/ekspordi teenust kasutada?**
Üks peaks kaaluma, kasutades Azure Import ja eksport kui üles- või allalaadimise kaudu kulub umbes prognoosimiseks üle 7 päeva. Saate arvutada, kui kaua võtab aega, kasutades mis tahes online kalkulaator või [allalaadimise](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) asus meie Azure impordi eksportimine REST API valimi Azure näidised hoidlas veebisaidil [Andmete edastamiseks kiiruse kalkulaator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html). See pole täpselt arvutada, kuid ainult töötlemata märge.

**Saate kasutada Azure impordi/ekspordi teenuse ressursihaldur salvestusruumi konto?**

Ei, ei saa andmete kopeerimine või ressursihaldur salvestusruumi konto abil otse teenuse Azure impordi/ekspordi. Teenuse toetab ainult klassikaline salvestusruumi kontod. Ressursihaldur salvestusruumi konto tugi tulevad varsti. Teise võimalusena saate andmete importimine klassikaline salvestusruumi konto ja migreerimine ressursihaldur salvestusruumi konto [AzCopy](storage-use-azcopy.md) või CopyBlob abil.

**Azure'i failide Azure'i impordi/ekspordi teenuse abil saate kopeerida?**

Ei, Azure impordi/ekspordi teenuse toetab ainult Blokeeri plekid ja lehe plekid. Kõigi muude salvestusruumi tüüpide, sh Azure failide, tabelite ja järjekorrad ei toetata.

**Azure'i impordi/ekspordi teenus on CSP tellimuste puhul saadaval?**

Ei, ei toeta teenust Azure impordi/ekspordi CSP tellimused. Funktsiooni tugi lisatakse tulevikus.

**Saab vahele jätta ketas ettevalmistamine toiming impordi tööd või saaksin kettal kopeerimata?**

Mis tahes ketas, mida soovite saata andmete importimiseks peab olema valmis Azure impordi/ekspordi kliendi tööriista abil. Kasutage tööriista kliendi andmete kopeerimine ketas.

**Vaja teha mis tahes ketas ettevalmistamine loomisel on ekspordi töö?**

Ei, kuid mõned enne kontrolli on soovitatav. Kontrollige ketast nõutav Azure impordi/ekspordi tööriista PreviewExport käsuga arvu. Lisateabe saamiseks vt [Eelvaate Drive kasutus eksportimine tööd](https://msdn.microsoft.com/library/azure/dn722414.aspx). See aitab teil eelvaate draivi kasutamist plekid valisite, põhineb kavatsete kasutada draivid suurust. Lugege lisaks ka, et te saate lugeda ja kirjutada kõvakettale, mis saadetakse ekspordi töö.

**Mis juhtub, kui saadan kogemata kõvaketas, mis ei vasta nõuetele toetatud?**

Azure'i andmekeskuse tulemuseks ketas, mis ei vasta teie toetatud nõuetele. Kui ainult osa draivid pakkimine vastavusnõudeid tugi need draivid töödeldakse ja teile tagastatakse draivid, mis ei vasta nõuetele.

**Minu töö saate tühistada?**

Töö saate tühistada, kui selle olek on loomine ja saatmine.

**Kui kaua saate lõpuleviidud tööde oleku klassikaline portaalis vaadata?**

Lõpetatud projektide olekut saate vaadata kuni 90 päeva. Lõpetatud projektide kustutatakse pärast 90 päeva.

**Kui ma ei soovi importida või eksportida pikem kui 10 draivid, mida teha?**

Ühe importimine või eksportimine töö viitamiseks ainult 10 draivid ühe töö teenuse impordi/ekspordi. Kui soovite saata rohkem kui 10 draivid, saate luua mitme töö. Draivid, mis on seotud sama töö tuleb saata koos sama pakett.

**Saate kasutada USB-SATA adapterit, mis ei ole soovitatav loendis?**

Kui teil on muundur, mis pole loendis, proovige oma muunduri kasutamine ette valmistada ketas ja vaadake, kas see töötab enne ostmist toetatud muunduri tööriista Azure impordi/ekspordi.

**Kas teenus vormindamine draivid enne nende tagastamine?**

Ei. Kõik draivid on krüptitud BitLockeri abil.

**Impordi/ekspordi tööde draivid saate osta Microsofti?**

Ei. Peate saata oma draivid nii importimiseks ja eksportimiseks tööde haldamine.

**Pärast importimist töö lõpulejõudmist, milline on minu andmed välja näeb salvestusruumi konto? Minu directory hierarhia alles?**

Kõvaketas ettevalmistamisel impordi tööd sihtkoht on määratud soovitud /dstdir: parameeter. See on sihtkoht container salvestusruumi konto, mille kõvaketas andmed kopeeritakse. See sihtkoht ümbris, virtuaalkaustad on loodud kaustu kõvakettale ja plekid on loodud faile.

**Kui see on faile, mis on minu salvestusruumi konto juba olemas, teenuse üle olemasoleva plekid salvestusruumi minu konto?**

Ketas koostamisel saate määrata, kas sihtkoht failid tuleks üle kirjutada või ignoreeritud parameetriga nimetatakse /Disposition: < ümbernimetamine | ei kirjutada | kirjutada >. Vaikimisi teenus on uue faili ümber nimetada, mitte Kirjuta olemasolevad plekid.

**Ühildub tööriista Azure impordi/ekspordi kliendi 32-bitiste opsüsteemide jaoks?**
Ei. Kliendi tööriista ühildub ainult 64-bitise Windowsi operatsioonisüsteemid. Vaadake operatsioonisüsteemid jaotist [eeltingimused](#pre-requisites) täieliku loendi toetatud OS versioonid.

**Kas ma kaasata midagi muud kui kõvakettadraiv minu paketi?**

Palun saata ainult teie kõvaketas. Kaasa üksused, nt power pakkumise kaabel või USB-kaabel.

**Kas ma pean oma draivid FedEx või DHL abil saata?**

Saate saata kasutada mis tahes teadaolevad carrier nagu FedEx, DHL, UPS või meile posti teenus andmekeskuse draivid. Siiski jaoks saatmine teile draivid andmete keskuse kaudu, peate sisestama FedEx kontonumbri USA-s ja EU või DHL kontonumbri Aasia ja Austraalia piirkondades.

**Kas koos oma ketas rahvusvaheliselt saatmine piirangutest?**

Pange tähele, et teil on saatmine füüsilise meediumi vajalikud üle rahvusvaheline piiri. Olete tagama, et teie füüsilise meedia ja andmed on imporditud ja/või kohaldatavate õigusaktidega ekspordi. Enne saatmine füüsilise meedia, kontrollige oma nõuandjad, et teie meediumid ja andmed kas õiguslikult saadetakse tuvastatud andmekeskuse kinnitamiseks. See aitab tagada, et see jõuab Microsoft õigel ajal.

**Töö loomisel saatmine aadress on asukohta, mis on minu konto talletuskoht erineda. Mida teha?**

Täpsema salvestusruumi konto on vastendatud alternatiivse saatmine asukohad. Varem saatmise asukoht saadaolevate asukohtade saab ka ajutiselt vastendada alternatiivse asukohad. Kontrollige alati enne saatmine teie draivid töö loomise ajal saatmise aadressi.

**Miks minu töö oleku on klassikaline portaali öelda "saatmine" kui näitab Carrier veebileht minu pakett on kohale toimetatud?**

Klassikaline portaalis olek muutub saatmine puhul kui töötlemine käivitamise ketas. Kui teie ketas jõudnud poole, kuid pole alustatud töötlemine, kuvatakse töö oleku nimega saatmine.

**Kui saatmine minu ketas, küsib vedaja andmete center kontakti nimi ja telefoninumber. Mida peaks andma?**

Telefoninumbri pakutakse teile töö loomise ajal. Kui teil on vaja kontakti nime, võtke meiega waimportexport@microsoft.com ja anname teile vastav teave.

**Kasutada teenust Azure impordi/ekspordi PST postkastid ja SharePointi andmete kopeerimiseks O365?**

Vaadake [importimine PST-failid või SharePointi andmete teenusekomplekti Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Kasutada teenust Azure impordi/ekspordi minu varukoopiaid ühenduseta kopeerimiseks Azure varundus teenust?**

Vaadake [ühenduseta varundamise töövoo Azure varukoopia](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Vt ka:

- [Kuidas häälestada Azure impordi/ekspordi kliendi tööriist](https://msdn.microsoft.com/library/dn529112.aspx)

- [Andmete AzCopy käsurea kasuliku](storage-use-azcopy.md)

- [Azure'i Import ja eksport REST API näidis](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
