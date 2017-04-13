<properties
    pageTitle="Ühenduseta sünkroonimise Azure mobiilirakendused | Microsoft Azure'i"
    description="Kontseptuaalne viide ja ühenduseta sünkroonimise funktsiooni Azure'i mobiilirakenduste ülevaade"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Ühenduseta sünkroonimise Azure mobiilirakendused

## <a name="what-is-offline-data-sync"></a>Mis on ühenduseta sünkroonimine?

Kliendi ja serveri SDK funktsioon Azure'i mobiilirakenduste, mis võimaldab arendajatel luua rakendusi, mis töötavad ilma võrguühendus on ühenduseta sünkroonimine.

Kui teie rakendus on ühenduseta režiimis, kasutajate saab endiselt luua ja muuta andmeid, mis salvestatakse kohalikku salve. Kui rakendus töötab võrguühenduse taastamisel, see kohaliku muudatused sünkroonida oma Azure'i Mobile'i rakendus taustväärtus. Funktsioon sisaldab ka tugi tuvastamiseks konfliktide sama kirje muutmisel nii klient ja selle taustväärtus. Konfliktide saab käsitleda kas serveris või kliendi.

Ühenduseta sünkroonimine on mitmeid eelised:

* Rakenduse tundlikkuse parandamiseks serveri andmed kohalikult seadme vahemällu talletamine
* Töökindlaid rakendusi, mis jäävad kasulik, kui seal on võrguprobleemide loomine
* Luba loomine ja andmete muutmiseks, isegi siis, kui seal on juurdepääsu pole, toetavad stsenaariume või pole Ühenduvus
* Andmete sünkroonimine mitmes seadmes ja avastada konfliktide, kui sama kirje on muudetud kahe seadmed
* Piirake latentsusajaga või mahupõhise võrgu network kasutamine

Järgmised õppetükid Kuva ühenduseta sünkroonimine lisamine Azure'i Mobile'i rakenduste kasutamise Mobiiliversiooni kliendid:

* [Android: Luba ühenduseta sünkroonimine]
* [iOS: ühenduseta sünkroonimise lubamine]
* [IOS-i Xamarin: ühenduseta sünkroonimise lubamine]
* [Xamarin Android: Luba ühenduseta sünkroonimine]
* [Xamarin.Forms: Luba ühenduseta sünkroonimine](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Universaalne Windowsi platvormi: Luba ühenduseta sünkroonimine]

## <a name="what-is-a-sync-table"></a>Mis on sünkroonimine tabeli?

"/ Tabelid" lõpp-punkti juurdepääsu Azure Mobile kliendi SDK-d pakuvad liidesed nagu `IMobileServiceTable` (.net-i klient SDK) või `MSTable` (iOS-i klient). Nende API-de ühenduse Azure'i mobiilirakenduse kirjutamata ja nurjub, kui kliendi seade pole võrguühendust.

Ühenduseta režiimile toetamiseks rakenduse peaksite kasutama *sünkroonimine tabeli* API-d, näiteks `IMobileServiceSyncTable` (.net-i klient SDK) või `MSSyncTable` (iOS-i klient). Kõiki samu CRUD-toiminguid (loomine, lugemine, värskendamine, kustutamine) töötavad vastu sünkroonimine tabeli API-d, kuid nüüd nad lugeda või kirjutada *kohalikku salve*. Enne mis tahes sünkroonimine tabeli toiminguid teha, tuleb lähtestada kohalikku salve.

## <a name="what-is-a-local-store"></a>Mis on kohalikku salve?

Kohalikku salve on andmete püsimine layer kliendi seadme. Azure'i mobiilirakenduste kliendi SDK-d pakuvad vaikimisi kohalikku salve rakendamine. Opsüsteemis Windows, Xamarin ja Android, see põhineb SQLite; iOS, põhineb Core andmed.

Windows Phone või Windows Store 8.1 SQLite-põhiste rakendamise kasutamiseks on vaja installida SQLite laiendamine. Lisateavet leiate teemast [ühes kohas Windowsi platvormi: ühenduseta sünkroonimise lubamine]. Androidi ja iOS-i saata versiooniga SQLite seadme operatsioonisüsteemi ise, seega ei ole vaja oma versiooni SQLite viitamine.

Arendajad saate rakendada ka oma kohalikku salve. Näiteks, kui soovite andmete talletamiseks mobile kliendi krüptitud vormingus, saate määratleda kohalikku salve, mis kasutab SQLCipher krüptimiseks.

## <a name="what-is-a-sync-context"></a>Mis on sünkroonimine kontekstis?

*Sünkroonimine kontekstis* on seostatud mobiilikliendi objekti (näiteks `IMobileServiceClient` või `MSClient`) ja rajad sünkroonimine tabelitega tehtud muudatusi. Sünkroonimine kontekstis väidab mõni *toiming järjekorda* , mis hoiab on järjestatud loendi CUD toimingud (loomine, värskendamine, kustutamine), mis saadetakse hiljem server.

Kohalikku salve on seotud sünkroonimine konteksti lähtestamise meetodiga nagu `IMobileServicesSyncContext.InitializeAsync(localstore)` [.net-i kliendi SDK].

## <a name="how-sync-works"></a>Kuidas ühenduseta sünkroonimise töötab

Sünkroonimine tabelite kasutamisel oma kliendi kood juhtelemendid, kui sünkroonitakse kohalikud muudatused on Azure Mobile'i rakendus taustväärtus. Midagi saadetakse soovitud taustväärtus seni, kuni on kõne *tõuketeatised* kohaliku muudatused. Samuti lisatakse kohalikku salve uued andmed ainult siis, kui kõne *pull* andmeid.

* **Tõuketeatised**: tõuketeatised on toimingu sünkroonimine konteksti ja saadab kõik CUD muudatused alates viimase tõuketeatised. Pange tähele, et ei ole võimalik saata üksikute tabeli muudatusi, kuna vastasel juhul tegevuse saanud saata vales järjestuses. Tõuketeatised aktiveeritakse sarja ülejäänud kõned Azure'i Mobile'i rakendus taustväärtus, mis omakorda muudab teie serveri andmebaas.

* **Tõmmata**: Pull kohta tabeli alusel tehakse ja saab kohandada päring alamhulka serveri andmete toomiseks. Seejärel sisestage Azure Mobile kliendi SDK-d saadud andmete kohalikku salve.

* **Peidetud sunnib**: kui tõmmake täidetakse tabel, milles on kuni kohaliku värskendusi vastu, tõmme esmalt käivitatakse sünkroonimine konteksti push. See aitab minimeerida konflikte muudatused, mida on juba järjekorras ja uute andmete serverist.

* **Astmeline sünkroonimine**: tööks pull esimene parameeter on *päringu nime* , mida kasutatakse ainult kliendi. Kui kasutate mittenullväärtusega päringu nime, täidab Azure Mobile SDK on *astmeline sünkroonimine*.
  Iga kord, kui pull toiming tagastab kogumi tulemuste uusima `updatedAt` ajatempli tulemi kogumist on talletatud kohalik süsteem SDK tabelid. Edaspidised pull toimingute toob kirjeid ainult see ajatempel pärast.

  Astmeline sünkroonimine kasutamiseks peab andma oma serveri mõtestatud `updatedAt` väärtused ja toetama, selle välja alusel sortimine. Siiski, kuna SDK lisab oma sortimine updatedAt välja, ei saa kasutada pull päring, mis on oma `$orderBy$` klausel.

  Päringu nime võib olla mis tahes stringi, saate valida, kuid see peab olema kordumatu iga loogilise päringu rakenduse.
  Muul juhul erinevate pull toimingud võivad kirjutada sama suureneva sünkroonimine ajatempli ja päringute saate vale tulemeid.

  Kui päring on parameeter, on üks võimalus luua kordumatu päringu nimi parameetri väärtuse lisada.
  Näiteks, kui teil on filtreerimine kasutajanimi, oma päringu nimi võiks järgmiselt (C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Kui soovite loobuda suureneva sünkroonimine, andke `null` päringu ID Sel juhul tuuakse kõik kirjed iga helistada `PullAsync`, mis on potentsiaalselt ebatõhus.

* **Puhastamine**: kustutada sisu kohalikku salve abil `IMobileServiceSyncTable.PurgeAsync`.
  See võib olla vajalik, kui teil on kehtetud andmed kliendi andmebaasi, või kui soovite kõik Ootel muudatuste hülgamine.

  Puhastada tühjendab tabeli kohalikku salve. Kui toimingute serveri andmebaasiga sünkroonimise ootel, kuvatakse selle puhastada põhjustada erandi juhul, kui *likvideerite Jõusta* parameeter on seatud.

  Kliendi kehtetud andmed, näiteks Oletame, et näites "todo loend" Device1 tõmbab ainult üksused, mis on lõpule viidud. Seejärel soovitud todoitem "Osta piima" märgitud lõpetatud serveris mõni muu seade. Siiski Device1 veel "Osta piim" todoitem kohaliku salvestada kuna see on ainult tõmbamine üksused, mida pole lõpetatuks märgitud. Puhastada tühjendab selle aegunud kirje.

## <a name="next-steps"></a>Järgmised sammud

* [iOS: ühenduseta sünkroonimise lubamine]
* [IOS-i Xamarin: ühenduseta sünkroonimise lubamine]
* [Xamarin Android: Luba ühenduseta sünkroonimine]
* [Universaalne Windowsi platvormi: Luba ühenduseta sünkroonimine]

<!-- Links -->
[.Net-i kliendi SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Luba ühenduseta sünkroonimine]: app-service-mobile-android-get-started-offline-data.md
[iOS: ühenduseta sünkroonimise lubamine]: app-service-mobile-ios-get-started-offline-data.md
[IOS-i Xamarin: ühenduseta sünkroonimise lubamine]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Luba ühenduseta sünkroonimine]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Universaalne Windowsi platvormi: Luba ühenduseta sünkroonimine]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
