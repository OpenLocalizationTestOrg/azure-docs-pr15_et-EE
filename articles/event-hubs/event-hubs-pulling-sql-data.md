<properties
   pageTitle="Tõmbamine andmete SQL Azure'i sündmuse jaoturi sisse | Microsoft Azure'i"
   description="Ülevaade sündmuse jaoturiga importimine SQL-i näidis"
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor=""/>

<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Tõmbamine andmed: SQL Azure'i sündmuse jaoturiga

Tüüpilised arhitektuur rakenduse reaalajas andmete töötlemiseks hõlmab esmalt kinni Azure'i sündmuse jaoturiga. See võib olla asjade stsenaarium, nt jälgimine erinevate mõne on kiirtee liiklus või mängimine stsenaariumi puhul jälgimise horde meeletu võistlejad, toimingud või ettevõtte stsenaarium, nt building inimesele jälgimine. Sellisel juhul andmete tootjad on üldiselt väliste objektide aeg-sarja andmeid, et peate koguda, analüüsida, talletada ja otsustama, ja võivad kulutada palju vaeva building välja järgmiste protsesside taristu üheks. Mida teha teha, kui soovite tuua andmed midagi nagu andmebaasi, mitte streaming andmete allikas, ja seda kasutada koos oma streaming andmeid? Kaaluge võimalust juhul, kui soovite kasutada Azure voo Analytics, Remote Explorer (RDX) või mõnda muud tööriista analüüsimiseks ja toimivad aeglaselt muutuvate andmete Microsoft Dynamics AXI või kohandatud vahendid haldamise süsteem? Probleemi lahendamiseks oleme kirjutada ja avatud allikad väike pilv näide, mis saate muuta ja juurutada, mis tõmmata SQL tabeli andmed ja lükake Azure'i sündmuse jaoturiga kasutada sisendina teie järgmise etapi analytical rakenduses. Mõistan, et see on harv stsenaarium ja mida tavaliselt teha mõne sündmuse jaoturi vastand. Aga kui leiate ise juhul, kui see on vajalik teha, leiate koodi [Azure'i näidised Galerii](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/).  

Pange tähele, et see proovi kood on lihtsalt, et valimi. On tootmise rakenduse, et funktsioon **pole** mõeldud ja pole katsete on tehtud teha seda sobib kasutamiseks sellise keskkonna - stricly DIY, on arendaja värskendustest näide. Peate üle igasuguseid turvalisus, jõudlus, funktsioonid ja maksumus tegureid enne lahendamise kohta – lõpuni arhitektuur.

## <a name="application-structure"></a>Rakenduse struktuuri

Rakendus on kirjutatud C# ja valimis readme.md fail sisaldab kogu teavet, mida soovite muuta, luua ja avaldada rakendus. Järgmistes jaotistes on kõrge ülevaade sellest, mida rakendus ei.

Alustame eeldusel, et teil on juurdepääs SQL Azure'i tabeli. Peate ka seadistanud on Azure sündmuse jaoturi ja leida ühenduse string, mis on vaja juurde pääseda.

Lahendus SqlToEventHub sisselülitamisel loeb konfiguratsioonifail (App.config) saada mitmeid asju, nagu on kirjeldatud readme.md fail. Need on üsna selgituseta, näiteks nimi andmetabel jne ja ei ole vaja, et kordamine selgitused siin. 

Kui rakendus on config faili lugeda, selle läheb üheks esitatavaks, lugemise SQL tabel ja lükkamine kirjete sündmuse jaoturi, siis ootamine kasutaja määratletud une intervall enne seda kõik uuesti. Mõned asjad on märkimist.

1. Rakendus on põhinevad eeldusel, et SQL tabel värskendatakse välise meetodi abil ja soovite saada kõik ja ainult sündmuse jaoturiga värskendusi.
2. SQL-i tabeli peab olema kordumatu ja suurenevad numbri, nt kirje numer välja. See võib olla sama lihtne kui väljal nimega "Id" või midagi muud, mis muudeta nagu mis tahes värskendab andmebaasi lisab kirjeid, nt "Creation_time" või "Sequence_number". Märkmete ja talletab selle välja väärtus iga iteratsiooni. Iga järgmise läbida kursis rakendus sisuliselt küsib tabeli kõik kirjed, kus välja väärtus on suurem väärtus kuvati see Esita uuesti kuni viimase aja. Me kutsume selle viimase väärtuse "offset".
3. Rakenduse loob tabeli "TableOffsets", kui see käivitub nihked talletamiseks. Tabel luuakse "CreateOffsetTableQuery" määratletud config faili päring. 
4. On päringute offset tabeli "OffsetQuery", "UpdateOffsetQuery" ja "InsertOffsetQuery" config faili määratletud töötamiseks kasutada. Te ei peaks neid muuta.
5. Lõpuks "Andmepäringu" määratletud config faili päring on tõmmata tabeli SQL-i kirjeid töötavad päringut. See on praegu piiratud top 1000 kirjetele iga läbida tsükkel jaoks optimeerimiseks – kui näiteks on 25 000 kirjete lisatud andmebaasi viimase päringu, kuna see võib võtta aega päringu. Päringu 1000 kirjete piiramine iga kord, päringud on palju kiirem. Kõrvutiasuvad pakettidena 1000 kirjete valimine üles 1000 lihtsa sündmuse jaoturi sunnib.    

## <a name="next-steps"></a>Järgmised sammud

Lahendus juurutamiseks klooni või SqlToEventHub rakenduse allalaadimine App.config faili redigeerimiseks, ehitada ja lõpuks avaldada. Kui rakendus on avaldatud, saate vaadata see töötab Azure klassikaline portaalis jaotises pilveteenustega ja jälgida oma sündmuse jaoturi tulevad sündmused. Märkus. määrab sagedus mõlemat: värskendusi SQL tabel ja une intervalli config faili rakenduse määratud sagedusega.