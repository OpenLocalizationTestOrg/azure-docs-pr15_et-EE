<properties
   pageTitle="Kuidas registreeruda andmeallikate | Microsoft Azure'i"
   description="Juhise esiletõstmine registreerimise andmeallikate Azure'i andmekataloogi, sh metaandmete väljad ekstraktimist registreerimise käigus."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-register-data-sources"></a>Kuidas registreeruda andmeallikad

## <a name="introduction"></a>Sissejuhatus
**Microsoft Azure'i andmekataloogi** on täielikult hallatud pilveteenuses, mis toimib registreerimise süsteemi ja ettevõtte andmeallikaid discovery süsteem. Teisisõnu on **Azure andmekataloogi** teave aitab tuvastada, mõista ja andmeallikate ja aidata ettevõtted abil saate oma olemasolevad andmed rohkem väärtust. Ja muuta andmeallika leitavaks **Azure'i andmekataloogi** kaudu esimene samm on selle andmeallika registreerimiseks.
## <a name="registering-data-sources"></a>Andmeallikate registreerimine
Registreerimine on metaandmete andmeallikast ja **Azure andmekataloogi** teenuse andmeid kopeerida. Andmete jääb, kus see praegu asub, ja jaotises juhtelemendi administraatorid ja -poliitikad praeguse süsteemi jääb.

Andmeallika registreerimiseks lihtsalt käivitage **Azure'i andmekataloogi** andmete allikas registreerimise tööriista **Azure'i andmekataloogi** portaalist. Logige sisse oma töö või kooli kontoga (sama Azure Active Directory mandaadi abil saate portaali sisse logida) ja seejärel valige andmeallika registreerimiseks.
[Azure'i andmekataloogi kasutamise alustamine](data-catalog-get-started.md) õpetuse juhised üksikasjalikumat teavet teemast.

Pärast andmeallika on registreeritud, kataloogi jälgib selle asukohta ja selle metaandmed indeksid nii, et kasutajad saaksid otsida, sirvida, ja avastamine andmeallika ja seejärel selle asukohta ühenduse loomiseks kasutada rakenduse või enda valitud tööriista abil.

## <a name="sources-supported"></a>Toetatud andmeallikate
Vaadake [Andmete kataloogi DSR](data-catalog-dsr.md) praegu toetatud andmeallikate loendi.
<br/>


## <a name="structural-metadata"></a>Strukturaalset metaandmed
Andmeallika registreerimisel tööriista registreerimise väljavõte teavet valite objektide struktuuri – see on nimetatud nii strukturaalset metaandmete.

Kõigi objektide, sisaldab see strukturaalset metaandmete objekti asukohta, nii, et kasutajad, kes andmeid leida saate kasutada seda teavet ühenduse kasutamiseks klienditööriistades enda valitud objekti. Muude strukturaalset metaandmete sisaldab objekti nimi ja tüüp ja tippige atribuut/veeru nimi ja andmed.

## <a name="descriptive-metadata"></a>Kirjeldav metaandmed
Lisaks core strukturaalset metaandmete ekstraktimist andmeallika, ka väljavõte andmete allikas registreerimise tööriista kirjeldav metaandmete ka. SQL Server Analysis Services ja SQL serveri aruandlusteenuste, see on võetud esitatud nende teenuste atribuutide kirjeldus. SQL Server, kasutades ms_description, laiendatud atribuudi väärtuste ekstraktitakse. Oracle'i andmebaasi, andmete allikas registreerimise tööriista väljavõte kommentaarid veeru ALL_TAB_COMMENTS vaade.

Lisaks ekstraktimist andmeallika kirjeldav metaandmeid, kasutajad saavad sisestada kirjeldav metaandmete andmete allikas registreerimise tööriista abil. Kasutajate sildistada ja saate tuvastada asjatundjate registreeritud objektide jaoks. Kõik selles kirjeldav metaandmete kopeeritakse **Azure andmekataloogi** koos strukturaalset metaandmete teenus.

## <a name="including-previews"></a>Sh eelvaated

Vaikimisi ainult metaandmed on ekstraktimist allikate ja **Azure andmekataloogi** teenusega kopeerida, kuid mõistmine andmeallika on tihti lihtsam näha andmete valimi sisaldab.

**Azure'i andmekataloogi** andmete allikas registreerimise tööriista abil saavad kasutajad hetktõmmise eelvaade andmete kaasamiseks iga tabeli ja vaadata, mis on registreeritud. Kui kasutaja valib kaasata eelvaated registreerimise käigus, kaasatakse registreerimise tööriista kuni 20 kirjed iga tabelist ja vaade. Selle hetktõmmise on seejärel kopeerida kataloogi struktuuri ja kirjeldava metaandmete koos.


> [AZURE.NOTE]  Suure hulga veergu lai tabelid võib-olla vähem kui 20 kirjed, mis sisaldab nende eelvaade.


## <a name="including-data-profiles"></a>Sh andmete profiilid

Nii nagu, sh eelvaated esitada väärtuslik kontekstis kasutajatega otsimine **Andmekataloogi Azure'i**andmeallikad, sh andmete profiili saate ka oleks lihtsam avastatud andmeallikate mõistmiseks.

**Azure'i andmekataloogi** andmete allikas registreerimise tööriista abil saavad kasutajad iga tabeli ja vaade, mis on registreeritud profiili andmete kaasamiseks. Kui kasutaja valib profiili andmete kaasamiseks registreerimise käigus, registreerimise tööriist hõlmab liitväärtuse statistikat andmed iga tabeli ja kuvage, sh:

* Arv ridu ja objekti andmete maht
* Andmete ja objekti skeemi viimase värskenduse kuupäeva
* Nullväärtusega kirjed ja veergude erinevate väärtuste arv
* Veergude väärtused miinimum, maksimum, Keskmine ja standardhälve

Seejärel kopeeritakse kataloogi koos metaandmete struktuuri ja kirjeldav statistika.

> [AZURE.NOTE]  Tekst ja kuupäev veergude ei sisalda keskmise või standardhälve statistika oma profiili andmeid.

## <a name="updating-registrations"></a>Registreerimise värskendamine

Andmeallika registreerimisel teeb leitavaks **Azure'i andmekataloogi** abil metaandmete ja valikuline eelvaade ekstraktimist registreerimise käigus. Kui andmeallika vaja värskendada kataloogi (näiteks siis, kui objekti skeemiga on muutunud, tabelid, mis on algselt välja jätta tuleks lisada või kasutaja soovib andmete värskendamiseks on kaasatud eelvaated) tööriista andmete allikas registreerimist saate uuesti käivitada.

Uuesti registreerimisel juba registreeritud andmeallika sooritab Ühenda "upsert" toiming: olemasolevate objektide värskendatakse samal ajal, kui luuakse uus objektid. Säilitatakse kõik metaandmete kasutajate **Azure'i andmekataloogi** portaali kaudu.

## <a name="summary"></a>Kokkuvõte
Andmeallika registreerimise andmeallika **Azure'i andmekataloogi** on lihtsam üles leida ja mõista, kopeerides struktuuri ja kirjeldava metaandmeid andmeallikast kataloogi teenuse. Pärast andmeallika on registreeritud, seda saab siis märge, hallatavate ja avastatud **Azure'i andmekataloogi** portaalis.

## <a name="see-also"></a>Vt ka
- [Alustamine Azure'i andmekataloogi](data-catalog-get-started.md) õpetuse üksikasjalike üksikasju selle kohta, kuidas registreerida andmeallikad.
