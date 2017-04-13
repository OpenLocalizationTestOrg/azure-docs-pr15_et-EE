<properties
    pageTitle="Kopeerige või teisaldage andmed salvestusruumi AzCopy | Microsoft Azure'i"
    description="Andmete teisaldamine või kopeerimine või bloobimälu, tabeli ja faili sisu AzCopy kasuliku abil. Andmete kopeerimine Azure Storage kohaliku failidest või või salvestusruumi kontode vahel andmete kopeerimine. Andmete hõlpsalt Azure Storage migreerida."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>AzCopy käsurea kasuliku andmete edastamine

## <a name="overview"></a>Ülevaade

AzCopy on mõeldud andmete kopeerimise ja sealt Microsoft Azure'i bloobimälu, failide ja tabeli salvestusruumi optimaalse jõudluse tagamiseks lihtsa käskude kasutamine Windows käsurea kasuliku. Saate kopeerida andmeid ühest objektist teise kontoga salvestusruumi või salvestusruumi kontode vahel.

> [AZURE.NOTE] Sellest juhendist eeldab, et teil on juba tuttavad [Azure Storage](https://azure.microsoft.com/services/storage/). Kui ei, [Azure Storage Sissejuhatus](storage-introduction.md) dokumentatsiooni lugemine abiks olla. Kõige olulisem, peate [salvestusruumi konto](storage-create-storage-account.md#create-a-storage-account) loomiseks peab AzCopy kasutamise alustamine.

## <a name="download-and-install-azcopy"></a>Laadige alla ja installige AzCopy

### <a name="windows"></a>Windows

Laadige [AzCopy uusim versioon](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac/Linux

AzCopy ei ole saadaval Mac/Linux OSs. Azure'i CLI aga andmete kopeerimine ja sealt Azure Storage sobiv alternatiiv. Lugege lisateavet [Azure CLI Azure Storage abil](storage-azure-cli.md) .

## <a name="writing-your-first-azcopy-command"></a>Oma esimese AzCopy käsu kirjutamine

Põhilised käsud AzCopy süntaks on:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Avage käsuviiba aken ja liikuge AzCopy installikaust arvutis – kui selle `AzCopy.exe` käivitatava asub. Soovi korral saate lisada oma tee AzCopy installimise kohta. Vaikimisi on installitud AzCopy `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` või `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

Järgmised näited demonstreerivad seda erinevaid andmete kopeerimine ja Microsoft Azure plekid, failide ja tabeleid. Vaadake jaotist [AzCopy parameetrite](#azcopy-parameters) üksikasjaliku selgituse iga valimi kasutamine.

## <a name="blob-download"></a>Bloobimälu: allalaadimine

### <a name="download-single-blob"></a>Laadige alla ühe bloobimälu

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Pange tähele, et kui kausta `C:\myfolder` olemas, AzCopy loob seda ja laadige `abc.txt ` uude kausta.

### <a name="download-single-blob-from-secondary-region"></a>Ühe bloobimälu alla laadida teisene piirkond

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Pange tähele, peab teil olema lugemisõigus – geograafilise liigne salvestusruumi lubatud.

### <a name="download-all-blobs"></a>Kõik plekid allalaadimine

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Endale järgmised plekid asu määratud container:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Pärast allalaadimise toimingut kataloogi `C:\myfolder` sisalduvad järgmised failid:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Kui te ei määra suvand `/S`, pole plekid laaditakse alla.

### <a name="download-blobs-with-specified-prefix"></a>Laadige alla plekid määratud eesliitega

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Endale järgmised plekid asuvad määratud ümbrises. Kõik plekid, alustades eesliite `a` laaditakse:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Pärast kausta allalaadimine toimingut `C:\myfolder` sisalduvad järgmised failid:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Eesliite kehtib virtuaalse kataloogi, mis bloobimälu nime esimene osa. Ülaltoodud näites virtual directory ei vasta määratud eesliite nii, et see on alla. Lisaks, kui suvandi `\S` pole määratud, AzCopy ei lae mis tahes plekid.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Viimase muutmise aeg eksporditud failide olema sama mis andmeallika plekid seadmine

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

Samuti saate plekid välistamine laadida toiming oma viimase muutmise aja. Näiteks, kui soovite välistada plekid, mille viimase muutmise kellaaeg on sama või uuem kui sihtfail, lisage soovitud `/XN` suvand:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Või kui soovite välistada plekid, mille viimase muutmise kellaaeg on sama või vanemad kui sihtfail, lisage soovitud `/XO` suvand:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>Bloobimälu: üleslaadimine

### <a name="upload-single-file"></a>Ühe faili üleslaadimine

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Kui määratud sihtkohaga olemas, AzCopy luua ja laadige fail üles sinna.

### <a name="upload-single-file-to-virtual-directory"></a>Ühe faili üleslaadimine virtuaalne kataloog

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Kui määratud virtuaalse kausta olemas, AzCopy üles laadida faili, et kaasata virtual directory oma nime (*nt* `vd/abc.txt` eeltoodud näites).

### <a name="upload-all-files"></a>Kõikide failide üleslaadimine

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Võimalus määrata `/S` lisatud bloobimälu salvestusruumi rekursiivselt, mis tähendab, et kõik alamkaustad ja nende failide laaditakse ka määratud kataloogi sisu. Oletame näiteks järgmised failid asuda kaustas `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Pärast üles toimingut ümbris hõlmab järgmisi faile:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Kui te ei määra suvand `/S`, AzCopy ei saa üles laadida rekursiivselt. Pärast üles toimingut ümbris hõlmab järgmisi faile:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Laadige failid sobitamine määratud mustrile

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Endale järgmised failid asuda kaustas `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Pärast üles toimingut ümbris hõlmab järgmisi faile:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Kui te ei määra suvand `/S`, AzCopy üles laadida ainult plekid, mis ei asu virtuaalkaust:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Määrake MIME sisutüüp sihtkoha bloobimälu

Vaikimisi määrab AzCopy sihtkoha bloobimälu abil sisutüübi `application/octet-stream`. Alates versioonist 3.1.0, saate otseselt määrata sisutüübi kaudu suvandi `/SetContentType:[content-type]`. Süntaksit määrab kõik plekid sisutüübi toimingu Laadi üles.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Kui määrate `/SetContentType` ilma väärtuse, siis AzCopy loob iga bloobimälu või faili sisutüübi muutmiseks vastavalt selle faililaiend.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>Bloobimälu: kopeerimine

### <a name="copy-single-blob-within-storage-account"></a>Kopeerige ühe bloobimälu salvestusruumi konto

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Salvestusruumi konto lisamine bloobimälu kopeerimisel sooritatakse [serveripoolne koopia](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) toiming.

### <a name="copy-single-blob-across-storage-accounts"></a>Kopeerige kogu salvestusruumi kontod ühe bloobimälu

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Kui kopeerite kogu salvestusruumi kontod on bloobimälu, sooritatakse [serveripoolne koopia](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) toiming.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Kopeerige ühe bloobimälu teisene piirkond esmane alale

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Pange tähele, peab teil olema lugemisõigus – geograafilise liigne salvestusruumi lubatud.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopeerige ühe bloobimälu ja selle pilte üle salvestusruumi kontod

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Pärast selle toimingu Kopeeri, kaasatakse siht-ümbrisest selle bloobimälu ja selle pilte. Eeldades, et bloobimälu eeltoodud näites on kaks pilte, ümbris järgmised järgmised bloobimälu ja pilte.

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Kopeerige sünkroonselt plekid üle salvestusruumi kontod

Vaikimisi AzCopy kopeerib asünkroonselt andmete vahel kahe salvestusruumi lõpp-punktid. Seetõttu Kopeeri toiming käivitatakse tausta, kasutades vaba läbilaskevõime maht on puudub SLA kuidas kiire lisamine bloobimälu kopeeritakse ja AzCopy kontrollib perioodiliselt Kopeeri olek, kuni kopeerimise täita või ei ole.

Funktsiooni `/SyncCopy` suvand tagab, et kopeerimine on ühtlast kiirust. AzCopy täidab sünkroonse koopia allalaadimine plekid määratud kohaliku mälu kopeerida, ja seejärel üleslaadimiseks bloobimälu salvestusruumi sihtkohta.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`võiksite luua täiendavaid sealt maksumus võrrelduna asünkroonne koopia, on soovitatav kasutada suvand Azure VM, mis on sama piirkonna konto allikas salvestusruumi sealt kulude vältimiseks.

## <a name="file-download"></a>Faili: allalaadimine

### <a name="download-single-file"></a>Ühe faili alla laadida

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Kui määratud on mõni Azure faili ühiskasutus ja seejärel määrake faili täpset nime, kas (*nt* `abc.txt`) ühe faili alla laadida või määrake suvandi `/S` alla laadida kõik failid rekursiivselt ühiskasutus. Määrake faili mustri ja valik katsel `/S` koos tulemuseks viga.

### <a name="download-all-files"></a>Kõikide failide allalaadimine

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Pange tähele, et tühja kaustu ei laadita alla.

## <a name="file-upload"></a>Faili: üleslaadimine

### <a name="upload-single-file"></a>Ühe faili üleslaadimine

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Kõikide failide üleslaadimine

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Pange tähele, mis tahes tühja kaustu ei laadita.

### <a name="upload-files-matching-specified-pattern"></a>Laadige failid sobitamine määratud mustrile

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Faili: kopeerimine

### <a name="copy-across-file-shares"></a>Kopeerige kogu failikettad

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Faili ühiskasutus Bloobivahemälu kopeerimine

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Pange tähele, et asünkroonne kopeerimine failide talletamine lehe BLOOBI ei toetata.

### <a name="copy-from-blob-to-file-share"></a>Faili ühiskasutus bloobimälu kopeerimine

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Failide kopeerimine sünkroonselt

Saate määrata selle `/SyncCopy` suvandit, kui soovite andmete kopeerimine failide talletamine, et failide talletamine, bloobimälu failide talletamine ja bloobimälu faili mäluruumi sünkroonselt, AzCopy teeb seda lähteandmete allalaadimiseks kohaliku mälu ja laadige see uuesti sihtkohta.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Kui kopeerite bloobimälu: failide talletamine, vaikimisi bloobimälu tüüp on blokeerimine bloobimälu, kasutaja saab määrata suvand `/BlobType:page` sihtkoha bloobimälu tüüpi muuta.

Pange tähele, et `/SyncCopy` võiksite luua täiendavad sealt maksumus võrreldes asünkroonne koopia, selle suvandi kasutamiseks Azure VM, mis on sama piirkonna konto allikas salvestusruumi sealt kulude vältimiseks on soovitatav.

## <a name="table-export"></a>Tabel: eksport

### <a name="export-table"></a>Ekspordi tabel

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy kirjutab manifestfailile määratud sihtkausta. Avaldamisfail kasutatakse importimine leidmiseks vajalikud andmefailid ja teha andmete valideerimine. Vaikimisi kasutab järgmist nimetamistava Avaldamisfail:

    <account name>_<table name>_<timestamp>.manifest

Kasutaja saab määrata ka suvandi `/Manifest:<manifest file name>` seadmiseks manifest faili nime.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Tükeldamiseks mitmeks faile eksportimine

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy kasutab tükeldatud andmete failinimedes on *Helitugevuse index* eristada mitut faili. Helitugevuse registri koosneb kahest osast on *partition olulisi index* ja *tükeldamine faili indeks*. Mõlemad registrid on 0-põhine.

Sektsiooni võtme vahemiku registri on 0, kui kasutaja ei määra suvand `/PKRS`.

Oletame näiteks, et AzCopy loob kaks andmefaili pärast kasutaja suvand on määratud `/SplitSize`. Tulemiks olevad andmed failinimedes võib olla:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Pange tähele, et suvand väärtus võimalikult `/SplitSize` on 32MB. Kui määratud sihtkohta on bloobimälu, AzCopy kuvatakse tükeldamine andmefaili selle suurused ulatub bloobimälu mahupiirang (200GB), olenemata sellest, kas valik `/SplitSize` kasutaja määratud.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Tabeli eksportimine JSON- või CSV-faili vormingut

Vaikimisi AzCopy ekspordib tabelite JSON andmefailid. Saate määrata suvandi `/PayloadFormat:JSON|CSV` eksportimiseks tabelid JSON-või CSV.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Last CSV-vorminguga määramisel AzCopy loovad ka skeemifaili faililaiend `.schema.csv` iga andmefaili.

### <a name="export-table-entities-concurrently"></a>Ekspordi tabel üksuste samaaegselt

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy hakkavad samaaegseid toimingute üksuste eksportimine, kui kasutaja on määratud suvand `/PKRS`. Iga toimingu ekspordib ühe sektsiooni võtme vahemiku.

Pange tähele, et suvand kontrollib ka samaaegseid toimingute arv `/NC`. AzCopy kasutab protsessorite arvu vaikeväärtust, milleks on `/NC` tabeli üksuste kopeerimisel isegi juhul, kui `/NC` pole määratud. Kui kasutaja on määratud suvand `/PKRS`, AzCopy kasutab väiksem neist kahest väärtusest - sektsiooni võtme vahemike võrreldes otseselt või kaudselt määratud samaaegset toimingute - juurde samaaegseid toimingute alustamiseks arvu. Täpsema teabe saamiseks tippige `AzCopy /?:NC` käsurida.

### <a name="export-table-to-blob"></a>Ekspordi tabel Bloobivahemälu

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy loob JSON andmefaili bloobimälu dokumente koos jälgimise nimetamistava:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Loodud JSON andmefaili järgib minimaalsete metaandmete last vorming. Selles vormingus last kohta täpsema teabe saamiseks vt [Last vormingu tabeli toimingud](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Pange tähele, et tabelite plekid eksportimisel AzCopy kuvatakse tabeli üksuste allalaadimiseks kohaliku ajutised failid üksused, seejärel soovitud bloobimälu üles laadida. Need ajutised failid pannakse teega vaikimisi kausta Päevik faili "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", saate määrata suvand/Z: [töölehe faili kausta] muutmiseks töölehe faili kausta asukoht ja seega andmete ajutiste failide asukoha muutmine. Andmete ajutiste failide maht on otsustanud tabeli üksuste suurust ja määratud suvand /SplitSize, kuigi ajutine andmefaili kohalikule kettale kustutatakse kohe pärast seda, kui see on üles laaditud soovitud bloobimälu, veenduge, et teil on piisavalt kõvakettaruumi salvestada need ajutised failid enne kustutatakse.

## <a name="table-import"></a>Tabel: importimine

### <a name="import-table"></a>Tabeli importimine

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

Suvandi `/EntityOperation` näitab, kuidas lisada tabelisse üksused. Võimalikud väärtused on:

- `InsertOrSkip`: Ignoreerib olemasoleva üksuse või lisab uue üksuse, kui see pole tabelis.
- `InsertOrMerge`: Ühendab olemasoleva üksuse või lisab uue üksuse, kui see pole tabelis.
- `InsertOrReplace`: Asendab olemasoleva üksuse või lisab uue üksuse, kui see pole tabelis.

Teate, et te ei saa määrata suvand `/PKRS` impordi stsenaariumi. Erinevalt ekspordi stsenaarium, kus saate määrata suvand `/PKRS` samaaegseid tegevuse alustamiseks AzCopy vaikimisi hakkavad samaaegseid toimingute tabeli importimisel. Samaaegseid toimingute alustamine vaikearvu on võrdne arv protsessorite; Siiski saate määrata erinevat hulka samaaegseid suvandiga `/NC`. Täpsema teabe saamiseks tippige `AzCopy /?:NC` käsurida.

Pange tähele, et AzCopy toetab ainult, JSON, mitte CSV importida. AzCopy toetab JSON kasutaja loodud tabeli import ja näidata faile. Mõlemad failid peavad olema pärit AzCopy tabeli eksport. Tõrgete vältimiseks Palun muutke eksporditud JSON ega näidata faili.

### <a name="import-entities-to-table-using-blobs"></a>Üksuste importimine tabeli plekid abil

Endale bloobimälu container sisaldab järgmist: A JSON faili saab Azure'i tabeli ja selle kaasnevaid manifestifaili.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Käivitada üksuste importimine see bloobimälu ümbrises Avaldamisfail abil tabeli järgmine käsk:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Muud AzCopy funktsioonid

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Kopeerige ainult andmeid, mida pole sihtkoht

Funktsiooni `/XO` ja `/XN` parameetrite abil saate välistamine ei saa kopeerida, vastavalt vanema või uuemat versiooni allika ressursid. Kui soovite allika ressursid, mida pole sihtkohta kopeerida, saate määrata nii parameetrite AzCopy käsk:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Märkus: See on toetatud, kui lähte- või sihtkoha on tabel.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Vastuse faili abil saate määrata käsurea parameetrid

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Saate lisada mis tahes AzCopy käsurea parameetrid vastuse failis. AzCopy töötleb faili parameetrid, nagu siis, kui nad määratud käsureal, läbimiseks otsese asendamiseks faili sisu.

Endale vastuse fail nimega `copyoperation.txt`, mis sisaldab järgmised read. Iga parameetri AzCopy saab määrata ühele reale

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

või eraldi read.

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy nurjub, kui parameetri tükeldamist üle kahe rea, nagu on näidatud siin jaoks soovitud `/sourcekey` parameeter:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Mitme vastuse failide abil saate määrata käsurea parameetrid

Oletame, et vastuse fail nimega `source.txt` allika container, mis täpsustab:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Ja vastuse fail nimega `dest.txt` , mis täpsustab sihtkausta failisüsteemi:

    /Dest:C:\myfolder

Ja vastuse fail nimega `options.txt` , mis täpsustab AzCopy suvandid:

    /S /Y

Helistamiseks AzCopy vastuse nende failidega, mis kõik asuvad kataloogis `C:\responsefiles`, kasutage järgmist käsku:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy töötleb selle käsu, just nagu oleksite selle saate lisada kõik üksikute käsurea parameetrid:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Määrake ühiskasutusega juurdepääsu signatuuri (SAS)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

Saate määrata mõne SAS oleval URI:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Faili kausta Päevik

Iga kord, kui te AzCopy, et anda käsku See kontrollib, kas töölehe fail on olemas vaikekausta või kas see on olemas suvand kaudu teie määratud kausta. Kui töölehe fail pole kas koht, AzCopy käsitleb toimingu uue ja loob uue töölehe faili.

Kui töölehe faili olemas, AzCopy kontrollib, kas käsureal, et sisestate vastab käsurea töölehe faili. Kui need kaks joont käsk vasta, uuesti AzCopy lõpetamata toiming. Kui nad ei sobi, palutakse teil kirjutada kas töölehe faili käivitage uus toiming või praeguse toimingu tühistamine.

Kui soovite kasutada vaikeasukohta töölehe faili:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Kui argument suvand `/Z`, või määrata suvand `/Z` ilma kausta tee, nagu eespool näidatud AzCopy loob kausta Päevik faili vaikeasukohta, mis on `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Kui töölehe fail on juba olemas, seejärel uuesti AzCopy toimingu töölehe faili alusel.

Kui soovite määrata kohandatud töölehe faili asukoht.

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

Selles näites loob kausta Päevik faili, kui see pole juba olemas. Kui see on olemas, seejärel uuesti AzCopy toimingu töölehe faili alusel.

Kui soovite jätkata toimingu AzCopy.

    AzCopy /Z:C:\journalfolder\

Selles näites uuesti viimane toiming, mis ei õnnestunud lõpule viia.

### <a name="generate-a-log-file"></a>Luua logifaili

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Kui määrate suvandi `/V` faili tee Paljusõnaline Logi võimaldamata AzCopy loob siis logifaili vaikeasukohta, mis on `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Muul juhul saate luua kohandatud asukoha mõne logifaili:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Kui teie määratud suhteline tee jälgimise suvandi `/V`, näiteks `/V:test/azcopy1.log`, siis Paljusõnaline logifailid luuakse praegust töötavat kausta sees alamkausta nimega `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Määrata samaaegseid tegevuste alustamine

Suvand `/NC` määrab samaaegseid Kopeeri toimingute arv. Vaikimisi AzCopy alustab andmete edastamine läbilaskevõime suurendamise samaaegseid toimingute arv. Tabeli toimingute puhul samaaegseid toimingute arv võrdub teil protsessorite arvu. Samaaegseid toimingute arv bloobimälu ja faili toimingud on võrdne teil protsessorite arvu 8 korda. Kui kasutate võrguühendust väikese läbilaskevõimega AzCopy, saate määrata väiksema jaoks /NC vältimiseks põhjustatud ressursi konkurentsiga tõrge.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Azure'i salvestusruumi emulaator AzCopy vastuolus

Saate kasutada AzCopy [Azure salvestusruumi emulaator](storage-use-emulator.md) plekid suhtes.

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

ja tabeleid.

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>AzCopy parameetrid

Allpool on kirjeldatud AzCopy parameetrid. Samuti võite tippida ühte järgmistest käskudest käsureal abi kasutamise AzCopy:

- Üksikasjaliku käsurea abi AzCopy:`AzCopy /?`
- Üksikasjalikumat teavet mis tahes AzCopy parameetriga:`AzCopy /?:SourceKey`
- Käsurea näited:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Allikas: "andmeallika"

Saate määrata lähteandmed, millest soovite kopeerida. Allika saab faili süsteemi kataloogi, bloobimälu container, bloobimälu virtuaalkaust, salvestusruumi Failide ühiskasutamine, salvestusruumi faili kataloogi või Azure'i tabelist.

**Kehtivad:** Plekid, failid, tabelid

### <a name="destdestination"></a>/ Dest: "sihtkoht"

Saate määrata sihtkohta kopeerida. Sihtkoht võib olla faili süsteemi kataloogi, bloobimälu container, bloobimälu virtuaalkaust, salvestusruumi Failide ühiskasutamine, salvestusruumi faili kataloogi või Azure'i tabelist.

**Kehtivad:** Plekid, failid, tabelid

### <a name="patternfile-pattern"></a>/ Muster: "faili muster"

Saate määrata faili mustri, mis näitab, millised failid kopeerida. Parameetri /Pattern toimimist määratakse lähteandmete asukoha ja Rekursiivsed režiimi valik olemasolu. Rekursiivsed režiim on määratud kaudu valik /S.

Kui määratud on kataloog failisüsteemi, siis on standardne metamärkide ja esitatud mustriga sobib failide kataloogi raames vastu. Kui suvand /S pole määratud, siis AzCopy ka vastab määratud mustrile vastu kõik failid alamkaustade kataloogi all.

Kui määratud on bloobimälu container või virtuaalne kaust, seejärel metamärke ei rakendata. Kui suvand /S pole määratud, siis AzCopy tõlgendab määratud mustriga bloobimälu eesliite. Kui suvand /S pole määratud, siis AzCopy vastab täpselt bloobimälu nimede mustriga.

Kui määratud on mõni Azure failikettal, siis peate määrata täpse faili nime (nt abc.txt) kopeerimiseks ühte faili, või määrata suvand /S kopeerimiseks kõik failid rekursiivselt ühiskasutus. Määrake faili mustri ja valik katsel /S koos annab tulemuseks vea.

AzCopy kasutab tõstutundlik sobitamine, kui selle statistikast/allikas on bloobimälu container või bloobimälu virtuaalne kaust ja kasutab väiketähed sobitamine kõigil muudel juhtudel.

Vaikimisi kasutada, kui määratud pole mustriga mustriga on *.* faili asukoht või on tühi eesliide on Azure talletuskoht. Määrata mitu faili mustreid ei toetata.

**Kehtivad:** Plekid, failid

### <a name="destkeystorage-key"></a>/ DestKey: "salvestusruumi-key"

Saate määrata salvestusruumi konto võti sihtkoha ressursi.

**Kehtivad:** Plekid, failid, tabelid

### <a name="destsassas-token"></a>/ DestSAS: "sas luba"

Määrab ühiskasutusse Accessi allkirja (SAS) sihtkoht lugemine ja kirjutamine õigustega (vajadusel). Ümbritsege muude jutumärkidega, nagu see võib sisaldab käsurea erimärke.

Kui sihtkoha ressurss on bloobimälu container, faili jagada või tabeli, saate määrata, kas see suvand Luba SAS, millele järgneb või muude saate määrata sihtkoha bloobimälu ümbris, failikettal või tabeli URI, pole see suvand.

Kui lähte-kui ka on nii plekid, siis sihtkoha bloobimälu peab asuma sama nimega allika bloobimälu salvestusruumi kontol.

**Kehtivad:** Plekid, failid, tabelid

### <a name="sourcekeystorage-key"></a>/ SourceKey: "salvestusruumi-key"

Saate määrata andmeallika ressursi salvestusruumi konto võti.

**Kehtivad:** Plekid, failid, tabelid

### <a name="sourcesassas-token"></a>/ SourceSAS: "sas luba"

Määrab ühiskasutusse Accessi signatuuri allika LUGEMIS- ja loendi õigustega (vajadusel). Ümbritsege muude jutumärkidega, nagu see võib sisaldab erimärke käsurea.

Kui allika ressurss on bloobimälu container ja klahvi ega on SAS on esitatud, siis bloobimälu ümbris ei loe anonüümse juurdepääsu kaudu.

Kui Lähtevaluuta on faili jagada või tabel, tuleb klahvi või mõne SAS.

**Kehtivad:** Plekid, failid, tabelid

### <a name="s"></a>/S

Saate määrata Kopeeri toimingute Rekursiivsed režiim. Rekursiivsed režiimis, kuvatakse AzCopy kopeerige kõik plekid või faile, mis vastavad määratud mustriga, kaasa arvatud alamkaustad.

**Kehtivad:** Plekid, failid

### <a name="blobtypeblock--page--append"></a>/ BlobType: "block" | "leht" | "lisab"

Määrab, kas sihtkoht bloobimälu on blokeerimine bloobimälu, lehe bloobimälu või mõne lisa bloobimälu. See suvand on rakendatav vaid siis, kui mõnda bloobimälu üleslaadimiseks. Muul juhul genereeritakse tõrge. Kui sihtkoht on soovitud bloobimälu ja see suvand pole määratud, vaikimisi AzCopy loob Blokeeri bloobimälu.

**Kehtivad:** Plekid

### <a name="checkmd5"></a>/ CheckMD5

Arvutab allalaaditud andmete jaoks soovitud MD5 räsi ja kinnitab, et MD5 räsi talletatud soovitud bloobimälu või faili sisu-MD5 atribuudi vastab arvutatud räsi. MD5 ruut on vaikimisi välja lülitatud, määrake see suvand, et teha MD5 sisse andmete allalaadimisel.

Pange tähele, et Azure Storage ei garanteeri, salvestatud bloobimälu või faili MD5 räsi ajakohasuse. Vastutab kliendi MD5 värskendada iga kord, kui bloobimälu või fail on muudetud.

AzCopy määrab alati sisu-MD5 atribuudi Azure'i bloobimälu või faili üleslaadimine teenusesse pärast.  

**Kehtivad:** Plekid, failid

### <a name="snapshot"></a>/ Hetktõmmis

Näitab, kas hetktõmmiseid edastamiseks. See suvand kehtib ainult siis, kui allikas on soovitud bloobimälu.

Edastatud bloobimälu hetktõmmiseid ümber järgmises vormingus: .extension bloobimälu-nime (hetktõmmise aeg)

Vaikimisi on kopeeritud pilte.

**Kehtivad:** Plekid

### <a name="vverbose-log-file"></a>/ V: [Paljusõnaline-logifaili]

Väljundeid Paljusõnaline olekuteade log faili.

Vaikimisi on Paljusõnaline logifaili nimi AzCopyVerbose.log sisse `%LocalAppData%\Microsoft\Azure\AzCopy`. Kui määrate olemasoleva faili asukoha selle suvandi, lisatakse faili Paljusõnaline logifailid.  

**Kehtivad:** Plekid, failid, tabelid

### <a name="zjournal-file-folder"></a>/ Z: [töölehe faili kausta]

Saate määrata töölehe failikausta jaoks toimingu jätkata.

AzCopy toetab alati, kui toiming on katkestatud jätkata.

Kui see suvand pole määratud või see on määratud ilma selle kausta tee, siis AzCopy loob kausta Päevik faili vaikeasukohta, mis on % LocalAppData%\Microsoft\Azure\AzCopy.

Iga kord, kui te AzCopy, et anda käsku See kontrollib, kas töölehe fail on olemas vaikekausta või kas see on olemas suvand kaudu teie määratud kausta. Kui töölehe fail pole kas koht, AzCopy käsitleb toimingu uue ja loob uue töölehe faili.

Kui töölehe faili olemas, AzCopy kontrollib, kas käsureal, et sisestate vastab käsurea töölehe faili. Kui need kaks joont käsk vasta, uuesti AzCopy lõpetamata toiming. Kui nad ei sobi, palutakse teil kirjutada kas töölehe faili käivitage uus toiming või praeguse toimingu tühistamine.

Töölehe fail kustutatakse pärast toimingu edukat lõpetamist.

Pange tähele, et jätkata töölehe faili varasema versiooni AzCopy loodud toimingu ei toetata.

**Kehtivad:** Plekid, failid, tabelid

### <a name="parameter-file"></a>/@:"parameter-file"

Saate määrata faili, mis sisaldab parameetrid. AzCopy töötleb faili parameetrid, just nagu siis, kui nad määratud käsureal.

Vastuse failis, saate määrata mitu parameetrite ühele reale või määrata iga parameetri eraldi reale. Pange tähele, et on üksikud parameeter ei saa ühendada mitu rida.

Vastuse failid võivad sisaldada kommentaarid read, mis algavad # sümbol.

Saate määrata, mitu vastust faili. Pange tähele, et AzCopy ei toeta pesastatud vastuse failide.

**Kehtivad:** Plekid, failid, tabelid

### <a name="y"></a>/Y

Tõkestab kõik AzCopy kinnituse viipasid.

**Kehtivad:** Plekid, failid, tabelid

### <a name="l"></a>/ L

Saate määrata kirjet toiming ainult; andmed kopeeritakse.

AzCopy tõlgendab, kasutades seda võimalust simuleerimisena töötab see ilma käsurea valik/l ja loendamiseks, kui palju objektid kopeeritakse, saate määrata suvand Paljusõnaline Logi kopeeritakse /V samal ajal kontrollida, milliseid objekte.

See suvand toimimist määratakse ka lähteandmete asukoha ja Rekursiivsed režiimi suvand /S ja faili mustri suvandi /Pattern olemasolu.

AzCopy on vaja selle andmeallika asukoha loend ja lugege õigusi, kui selle suvandi abil.

**Kehtivad:** Plekid, failid

### <a name="mt"></a>/MT

Määrab allalaaditud faili viimase muutmise aeg olema sama allika bloobimälu või faili.

**Kehtivad:** Plekid, failid

### <a name="xn"></a>/XN

Välistab andmeallika uuema ressursi. Ressursi ei saa kopeerida, kui viimase muutmise aja allika on sama või uuem kui sihtkoht.

**Kehtivad:** Plekid, failid

### <a name="xo"></a>/XO

Välistab on vanemad allika ressurss. Ressursi ei saa kopeerida, kui viimase muutmise aja allika on sama või vanem kui sihtkoht.

**Kehtivad:** Plekid, failid

### <a name="a"></a>/A

Lisatud ainult faile, mis on seatud atribuut arhiivi.

**Kehtivad:** Plekid, failid

### <a name="iarashcnetoi"></a>/ I A: [RASHCNETOI]

Lisatud ainult faile, mis on mõni määratud atribuutide määramine.

Saadaolevad atribuudid on järgmised.

- R = kirjutuskaitstud failid
- A = failide valmis arhiivimine
- S = süsteemifailid
- H = peitfailid
- C = tihendatud failid
- N = tavaline failid
- E = krüptitud failide
- T = ajutised failid
- O = ühenduseta failid
- I = mitte indekseeritud failid

**Kehtivad:** Plekid, failid

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Välistab faile, mis sisaldavad mis tahes määratud atribuutide määramine.

Saadaolevad atribuudid on järgmised.

- R = kirjutuskaitstud failid
- A = failide valmis arhiivimine
- S = süsteemifailid
- H = peitfailid
- C = tihendatud failid
- N = tavaline failid
- E = krüptitud failide
- T = ajutised failid
- O = ühenduseta failid
- I = mitte indekseeritud failid

**Kehtivad:** Plekid, failid

### <a name="delimiterdelimiter"></a>/ Eraldaja: "eraldaja"

Näitab eraldaja sümboli, mida kasutatakse piiritlemiseks virtuaalkaustad bloobimälu nime.

Vaikimisi kasutab AzCopy / eraldaja märgina. Siiski toetab AzCopy, kasutades mõnda levinud märk (nt @, # või %) nimega eraldaja. Kui soovite kaasata mõne käsurea järgmisi erimärke, Pange nimi jutumärkidega.

See suvand kehtib ainult allalaadimiseks plekid.

**Kehtivad:** Plekid

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "arv-,-paralleelsete-toiminguid"

Saate määrata samaaegseid toimingute arv.

Vaikimisi AzCopy alustab andmete edastamine läbilaskevõime suurendamise samaaegseid toimingute arv. Pange tähele, et suure hulga samaaegseid toimingute väikese läbilaskevõimega keskkonnas, võib uputama võrguühendus takistavad täielikult täites need toimingud. Throttle samaaegseid toimingute tegeliku võrgu läbilaskevõime põhjal.

Samaaegseid toimingute on 512.

**Kehtivad:** Plekid, failid, tabelid

### <a name="sourcetypeblob--table"></a>/ Allikatüübi: "Bloobimälu" | "Tabel"

Saate määrata, mis on `source` ressurss on bloobimälu, mis on saadaval salvestusruumi emulaator töötab kohaliku arenduskeskkond.

**Kehtivad:** Plekid, tabelid

### <a name="desttypeblob--table"></a>/ DestType: "Bloobimälu" | "Tabel"

Saate määrata, mis on `destination` ressurss on bloobimälu, mis on saadaval salvestusruumi emulaator töötab kohaliku arenduskeskkond.

**Kehtivad:** Plekid, tabelid

### <a name="pkrskey1key2key3"></a>/ PKRS: "võti1 # võti2 #key3 #..."

Jagab sektsiooni võtme vahemiku lubamiseks paralleelselt, mis suurendab eksporditoimingu kiiruse tabeliandmete eksportimisel.

Kui see suvand pole määratud, siis AzCopy kasutab ühes teemas tabeli üksuste eksportimine. Näiteks, kui kasutaja on määratud /PKRS: "aa #bb", siis AzCopy algab kolm samaaegseid toimingud.

Iga toimingu ekspordi üks kolme sektsiooni võtme vahemikud, nagu allpool näidatud:

  [esimese-partition-klahv, aa)

  [aa, bb)

  [bb viimane-partition-key]

**Kehtivad:** Tabelid

### <a name="splitsizefile-size"></a>/ SplitSize: "faili suurus"

Määrab eksporditud fail tükeldamine MB minimaalse väärtuse lubatud maht on 32.

Kui see suvand pole määratud, kuvatakse AzCopy ühe faili eksportida tabeli andmeid.

Kui tabeli andmed on eksporditud on bloobimälu ja eksporditud fail suurus jõuab 200 GB limiit bloobimälu suurus, seejärel AzCopy jagab eksporditud fail, isegi siis, kui see suvand pole määratud.

**Kehtivad:** Tabelid

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Saate määrata tabeli andmete importimine käitumist.

- InsertOrSkip - ignoreerib olemasoleva üksuse või lisab uue üksuse, kui see pole tabelis.

- InsertOrMerge - ühendab olemasoleva üksuse või lisab uue üksuse, kui see pole tabelis.

- InsertOrReplace - asendab olemasoleva üksuse või lisab uue üksuse, kui see pole tabelis.

**Kehtivad:** Tabelid

### <a name="manifestmanifest-file"></a>/ Manifest: "manifesti-faili"

Määrab Avaldamisfail tabeli eksportimine ja importimine toiming.

See suvand pole kohustuslik eksporditoimingu käigus, AzCopy loob eelmääratletud nimega manifestfailile, kui see suvand pole määratud.

See suvand on nõutav jaoks andmefailide otsimine imporditoimingu ajal.

**Kehtivad:** Tabelid

### <a name="synccopy"></a>/ SyncCopy

Näitab, kas kopeerimiseks sünkroonselt plekid või failide vahel kaks Azure Storage lõpp-punktid.

Vaikimisi AzCopy kasutab serveripoolne asünkroonne koopiat. Määrake see suvand, et teha sünkroonse koopia, mis plekid või failide allalaadimist kohaliku mälu ja seejärel lisatud Azure Storage.

Saate selle suvandi failide sees bloobimälu, failide talletamine või: failide talletamine ja vastupidi bloobimälu kopeerimisel.

**Kehtivad:** Plekid, failid

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "sisutüüp"

Määrab MIME sisutüüp sihtkoha plekid või failid.

AzCopy määrab bloobimälu või faili sisutüübi teenuserakenduse/oktettide-voo vaikimisi. Saate otseselt määrata selle suvandi väärtus sisutüübi kõik plekid või failid.

Kui see suvand ilma väärtuse määramiseks klõpsake AzCopy seab iga bloobimälu või faili sisutüübi muutmiseks vastavalt selle faililaiend.

**Kehtivad:** Plekid, failid

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Saate määrata tabelis eksporditud andmefaili vorming.

Kui see suvand pole määratud, vaikimisi AzCopy ekspordi tabel andmefaili JSON-vormingus.

**Kehtivad:** Tabelid

## <a name="known-issues-and-best-practices"></a>Teadaolevate probleemide ja head tavad

### <a name="limit-concurrent-writes-while-copying-data"></a>Andmete kopeerimisel samaaegseid kirjutab piiramine

Kui kopeerite plekid või AzCopy faile, pidage meeles, et mõnda muusse rakendusse võib olla muutmine kui kopeerite selle andmed. Võimaluse korral veenduge, et kopeerite andmeid ei muudetakse käigus. Näiteks on seostatud mõne Azure virtuaalse masina VHD kopeerimisel veenduge, et ei ole muud rakendused praegu kirjutate selle VHD. Hea viis selleks on liisingu ressursi kopeerida. Vaheldumisi, saate luua selle VHD hetktõmmise esmalt ja seejärel kopeerige hetktõmmis.

Kui te ei saa takistada muude rakenduste kirjutamisel plekid või failide samal ajal, kui need on kopeeritud, siis pidage meeles, et lõpetab töö ajaks enam olla kopeeritud ressursside täielik tarkvarakomplektiga allika ressursid.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Käivitage ühe AzCopy eksemplari ühes arvutis.
AzCopy on mõeldud kasutamise kohta oma arvuti ressursside kiirendada andmeedastus maksimeerimiseks, soovitame teil käitada ainult üks AzCopy eksemplari ühes arvutis ja määrake soovitud suvandit `/NC` kui teil on vaja rohkem samaaegseid toimingud. Täpsema teabe saamiseks tippige `AzCopy /?:NC` käsurida.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>FIPS ühilduv MD5 algoritmid lubamine AzCopy kui te "kasutage FIPS ühilduv algoritmid krüptimine, räsi ja allkirjastamise".
AzCopy Vaikimisi kasutab .NET MD5 rakendamist MD5 arvutamiseks, kui objektide kopeerimine, kuid on mõned turbenõuetele, mida on vaja AzCopy lubamiseks FIPS ühilduv MD5 seade.

Saate luua rakenduse app.config fail `AzCopy.exe.config` atribuudiga `AzureStorageUseV1MD5` ja pange selle kõrvale koos AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Atribuudi "AzureStorageUseV1MD5" • True – kasutatakse vaikeväärtus AzCopy .NET MD5 rakendamist.
• False – AzCopy kasutab FIPS ühilduv MD5 algoritmi.

Märkus FIPS ühilduv algoritmid keelatakse vaikimisi teie Windowsi arvutisse, tippige secpol.msc oma Käivita aken ja märkige see lüliti turbesätte -> kohaliku poliitika -> Turve suvandid -> süsteemi krüptograafia: kasutamine FIPS ühilduv algoritmid krüptimine, räsi ja allkirjastamine.

## <a name="next-steps"></a>Järgmised sammud

Vaadake Azure Storage ja AzCopy kohta leiate lisateavet järgmistest teemadest.

### <a name="azure-storage-documentation"></a>Azure'i salvestusruumi dokumentatsioon:

- [Azure Storage Sissejuhatus](storage-introduction.md)
- [Kuidas kasutada bloobimälu .net-i kaudu](storage-dotnet-how-to-use-blobs.md)
- [Kuidas kasutada .NET: failide talletamine](storage-dotnet-how-to-use-files.md)
- [Kuidas kasutada tabelimälu .net-i kaudu](storage-dotnet-how-to-use-tables.md)
- [Kuidas luua, hallata või salvestusruumi konto kustutamine](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure'i salvestusruumi ajaveebipostituste:
- [Azure'i salvestusruumi andmete liikumine teegi eelvaate tutvustus](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Sünkroonse kopeerimine ja kohandatud sisutüübi tutvustus](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Väljakuulutamist üldine kättesaadavus AzCopy 3.0 pluss eelvaate versiooni AzCopy 4.0 tabeli ja faili tugi](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Suuremahuliste Kopeeri stsenaariumid jaoks optimeeritud](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Lugemisõigus – geograafilise liigne salvestusruumi tugi](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Andmete uuesti startable režiimi ja SAS luba](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Abil rist-konto Kopeeri bloobimälu](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Azure'i plekid failid üles/alla](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
