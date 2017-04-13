<properties
   pageTitle="Kuidas marginaalide andmeallikate | Microsoft Azure'i"
   description="Juhise esiletõstmise kohta marginaalide andmete varasid Azure'i andmekataloogi, sh sõbralikud nimed, siltide, kirjeldus ja eksperdid."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>


# <a name="how-to-annotate-data-sources"></a>Kuidas marginaalide andmeallikad

## <a name="introduction"></a>Sissejuhatus
**Microsoft Azure'i andmekataloogi** on täielikult hallatud pilveteenuses, mis toimib registreerimise süsteemi ja ettevõtte andmeallikaid discovery süsteem. Teisisõnu on andmekataloogi teave aitab tuvastada, mõista ja andmeallikate ja aidata ettevõtted abil saate oma olemasolevad andmed rohkem väärtust. Kui andmeallikas on registreeritud andmekataloogi, metaandmete kopeeritakse ja teenuse indekseeritud, kuid lugu ei lõpe. Andmekataloogi võimaldab kasutajatel sisestada oma kirjeldav metaandmete – nt kirjeldus ja sildid – täiendada ekstraktimist andmeallika metaandmete ja teha andmeallika paremini mõistetav rohkem inimesi.

## <a name="annotation-and-crowdsourcing"></a>Marginaalide lisamise ja crowdsourcing
Igaühel arvamust. Ja see on hea.
Andmekataloogi tuvastab, et on erinevatele kasutajatele erineva enterprise andmeallikate ja iga perspektiivi võib olla väärtuslik. Võtke arvesse järgmist stsenaariumi.

* Süsteemiadministraator teab taseme hooldusleppe serverid või teenuste majutavate andmeallika.
* Andmebaasi administraatori teab iga andmebaas ja lubatud ETL töötlemine Windowsi varukoopia ajakava.
* Süsteemi omanik teab protsessi andmeallikale juurdepääsu taotlemiseks kasutajate jaoks.
* Funktsiooni andmehaldur teab, kuidas varad ja andmeallika atribuutide vastendamine ettevõtte andmemudeli.
* Analüütik teab andmete kasutamise ta toetab äriprotsesside kontekstis.

Iga perspektiivi on väärtuslik ja andmekataloogi kasutab crowdsourcing lähenemine metaandmeid, mis võimaldab igale nupule, et olla ja kasutada täieliku ülevaate registreeritud andmeallikad. Portaali andmekataloogi kasutamisel iga kasutaja saate lisada ja muuta oma marginaale, on võimalik vaadata teiste kasutajate marginaalid.

## <a name="different-types-of-annotations"></a>Erinevat tüüpi marginaalid
Andmekataloogi toetab järgmist tüüpi marginaalid.

| Marginaalide     | Märkmete                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sõbralik nimi  | Sõbralikud nimed saab esitada tasemel andmete varade andmete vara, mis on hõlpsam mõista. Sõbralikud nimed on kõige kasulikumad, kui aluseks oleva objekti nimi on peidetud, lühendatud või muul viisil mõtestatud, kasutajatele.                                                                                                                            |
| Kirjeldus    | Kirjeldused saab esitada andmete varade ja atribuut / veeru tasemed. Kirjeldused on vabakujulise lühitekst marginaalid, mis kirjeldavad kasutaja perspektiiv andmete varade või selle kasutamist.                                                                                                                                                              |
| Siltide (kasutaja sildid)          | Silte saate esitada andmete aktiva ja atribuut / veeru tasemed. Kasutaja sildid on kasutaja määratletud Sildid, mida saab kasutada andmete varad või atribuute liigitamiseks.                                                                                                                                                                                                    |
| Siltide (sõnastik sildid)          | Silte saate esitada andmete aktiva ja atribuut / veeru tasemed. Sõnastik sildid on ühes keskses kohas määratletud sõnastiku termineid, mida saab kasutada andmete varad või atribuute, kasutades levinud business taksonoomia liigitamiseks. Lisateabe saamiseks vaadake, [Kuidas häälestada Business sõnastik reguleeritakse sildistamiseks](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Eksperdid        | Varade tasemel saab esitada eksperdid. Asjatundjate tuvastada kasutajate või rühmade expert perspektiivid andmed ja olla kontaktisikud kasutajatele, kes registreeritud andmeallikate ja on küsimusi, millele ei vastata, olemasoleva marginaalid.  |
| Juurdepääsu taotlemine | Taotleda juurdepääsu teavet saab edastada varade tasemel. See teave on kasutajatele, kes avastamine andmeallikas, mida nad ei ole veel juurde pääsemiseks õigused. Kasutajate e-posti aadressi kasutaja või rühm, kes annab juurdepääsu, URL-i või tööriist, mida kasutajad peavad pääseda juurde, saate sisestada või saate sisestada protsess ise tekstina. |
| Dokumentatsioon | Dokumente saab esitada andmete varade tasemel. Varade dokumentatsiooni on rikkaliku teksti teave, mis võib sisaldada linke ja pilte ja mida saate teavet, kirjeldus ja sildid kaudu ei toimetatud. |


## <a name="annotating-multiple-assets"></a>Marginaalid lisanud mitu varad
Andmekataloogi portaalis andmete varasid valimisel kasutajad saavad marginaale kõik valitud varad ühekordne. Kõigi valitud varad, hõlbustades valige ja sisestage ühtsete kirjeldus ja sildid ja ekspertide seotud andmete varade rakendatakse marginaalid.

> [AZURE.NOTE] Siltide ja ekspertide saab anda ka siis andmeallika registreerimiseks andmete varad Andmekataloogis andmetega registreerimise tööriista.

Kui mitme tabelid ja vaated, vaid veerge, et kõik valitud andmete varasid ühised kuvatakse andmekataloogi portaalis. See võimaldab kasutajatel anda sildid ja kirjeldused kõigi veergude kõik valitud varad sama nimi.

## <a name="annotations-and-discovery"></a>Marginaalid ja tuvastamine
Nagu andmekataloogi otsinguregistri lisatakse ekstraktimist andmeallika registreerimise käigus metaandmeid, metaandmete kasutaja esitatud ka indekseeritud. See tähendab, et mitte ainult marginaale oleks lihtsam kasutajatel mõista avastavad andmed, marginaalid lihtsustavad avastada selgitustega andmete varad otsides tingimustel, mis toimivad neile kasutajatele.

## <a name="summary"></a>Kokkuvõte
Registreerimise andmeallika andmekataloogi teeb andmete leitavaks kopeerides struktuuri ja kirjeldava metaandmete andmeallikast kataloogi teenus. Pärast andmeallika on registreeritud, kasutajad saavad sisestada hõlbustamiseks mõista andmekataloogi portaali kaudu üles leida ja marginaalid.

## <a name="see-also"></a>Vt ka
- [Alustamine Azure'i andmekataloogi](data-catalog-get-started.md) õpetus marginaalide andmeallikate üksikasjalike üksikasju.
