<properties
    pageTitle="Mobile kaasamine põhimõtet | Microsoft Azure'i"
    description="Azure'i Mobile kaasamine mõisted"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure'i Mobile kaasamine mõisted

Mobile kaasamine määratleb kõigi toetatud platvormid paar põhimõtet levinud. See artikkel kirjeldab lühidalt neid mõisteid.

Selles artiklis on hea algus, kui teil on uus Mobile allikaid. Samuti veenduge, et lugeda platvormi te kasutate, teatud dokumendid, nagu see kuvatakse täpsustamise mõisted rohkem üksikasju ja näited kui ka võimalikke piiranguid selles artiklis kirjeldatud.

## <a name="devices-and-users"></a>Seadmed ja kasutajad
Mobile kaasamine tuvastab kasutajate luues iga seadme jaoks kordumatut tunnust. See identifikaator nimetatakse seadme identifikaator (või `deviceid`). See on loodud nii, et kõik rakendused töötab sama seadme jagada sama seadme identifikaator.

Peidetult, see tähendab, et Mobile kaasamine leiab ühe seade kuulub täpselt ühe kasutaja ja seega kasutajatele ja seadmetele on samaväärne põhimõtet.

## <a name="sessions-and-activities"></a>Seansid ja tegevusi
Seansi ühe kasutaja poolt rakenduse kasutamine, alates hetkest kasutaja alustab abil, kuni kasutaja peatub.

Tegevus on üks kasutamine ühe kasutaja poolt rakenduse antud sub osa (see on tavaliselt ekraanil, kuid see võib olla ükskõik sobiv rakendus).

Kasutaja saab teha vaid üks tegevus korraga.

Tegevuse (kuni 64 märgi) nime järgi tuvastatakse ja saate soovi korral manustada mõned lisaandmed (1024 baiti limiit).

Seansid on automaatselt arvutada jada kasutajate tegevus kaudu. Seansi lõpetamine käivitub, kui kasutaja alustab oma esimese tegevusega ja peatub, kui ta lõpetab viimati. See tähendab, et seanss ei pea selgesõnaliselt käivitada või peatada. Selle asemel on tegevuste selgesõnaliselt alustada või lõpetada. Kui teateid, teatatakse pole seanss.

## <a name="events"></a>Sündmused
Sündmuste kasutatakse aruande kiirsõnumite toiminguid (nt klõpsatud nupuga või kasutajad, lugege artikleid).

Seansi töötava tööd, võib olla seotud sündmuse või see võib olla autonoomse sündmus.

Sündmuse nimi (kuni 64 märgi) tuvastatakse ja saate soovi korral manustada mõned lisaandmed (1024 baiti limiit).

## <a name="error"></a>Tõrge
Tõrgete kasutatakse teatada probleemidest õigesti tuvastatud rakendus (nt vale kasutajatoimingute või API kõne ebaõnnestumist).

Tõrke võib olla seotud praeguse seansi, töötava töö, või see võib olla autonoomse tõrge.

Tõrge (kuni 64 märgi) nime järgi tuvastatakse ja saate soovi korral manustada mõned lisaandmed (1024 baiti limiit).

## <a name="job"></a>Töö
Töö kasutatakse aruande kestvad toimingud (nagu API kõnede kestust, Kuva reklaamid kellaaja, kestuse tausttoimingud või kasutajatoimingute kestus).

Töö on seotud seansi, kuna tööülesande toimub taustal, ilma mis tahes kasutaja suhtluse.

Töö nimi (kuni 64 märgi) tuvastatakse ja saate soovi korral manustada mõned lisaandmed (1024 baiti limiit).

## <a name="crash"></a>Ootamatult sulguda
Jookseb välja automaatselt Mobile kaasamine SDK rakenduse tõrked kui probleemid ei tuvasta rakendus oleks crash teatada.

## <a name="application-information"></a>Rakenduse teave
Teenuserakenduse teabe (või rakenduse teave) kasutatakse sildistamiseks kasutajad, mis on mõned andmed (see on sarnane web küpsised, välja arvatud, et rakendus teave on talletatud serveripoolse Azure Mobile kaasamine platvormi) rakenduse kasutajatele siduda.

Rakenduse saab registreerida Mobile kaasamine SDK API abil või Mobile kaasamine platvormi API seadme abil.

Rakenduse teave on seotud seadme paari /-väärtuse. Oluline on nimi, rakenduse teave (ainult 64 ASCII tähti [a-zA-Z], [0 – 9] arvude ja allakriipsutatud märkideks [_]). (Kuni 1024 tähemärki) väärtus võib olla mis tahes stringi, täisarv, kuupäev (yyyy-MM-dd) või kahendmuutuja (true või false).

Suvalist arvu rakenduse teave võib olla seotud seadmega Mobile kaasamine hinnakirjad terminite määratletud piires. Ühe antud Key, Mobile kaasamine ainult jälgib, viimase väärtuse rida (ajalugu). Seadmine või muutmine on rakenduse teave väärtus jõustab Mobile kaasamine uuesti hinnata sihtrühma kriteeriumid on selle rakenduse teave (vajaduse korral) tähendab, et seda teavet rakenduse saab kasutada reaalajas sunnib käivitamiseks.

## <a name="extra-data"></a>Lisaandmed
Täiendavate andmete (või lisad) on mõned suvalise andmed, mis võivad olla seotud sündmused, tõrkeid, tegevused ja -tööde haldamine.

Lisad on struktureeritud sarnaselt JSON objektidele: tegemise puu /-väärtuse paarideks. Klahvid on piiratud 64 ASCII tähti [a-zA-Z], [0 – 9] arvude ja allakriipsutatud märkideks [_]) ja suuruse lisad piirdub 1024 märki (üks kord kodeeritud JSON SDK Mobile Engagement).

Kogu puu /-väärtuse paarideks talletatakse JSON objektina. Ainult esimese taseme klahvid/väärtused on siiski lagunenud pääsema otse nagu segmente teatud täpsemate funktsioonide (nt saate hõlpsalt määratleda nimega "SciFi ventilaatoreid" saatnud vähemalt 10 korda sündmus nimega "content_viewed" kõigi kasutajate tehtud segmendi koos lisatasu klahv "content_type" määratud väärtuse "scifi" Eelmine kuu). See on seega soovitatav saatmine ainult lisad tehtud /-väärtuse paarideks skalaarse väärtuste (nt stringide, kuupäevad, täisarvude või kahendmuutuja) abil lihtsa loetelu.

## <a name="next-steps"></a>Järgmised sammud

- [Windows SDK ühes kohas ülevaade Azure Mobile kaasamine](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlighti SDK ülevaade Azure Mobile kaasamine](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS-i SDK Azure Mobile kaasamine](mobile-engagement-ios-sdk-overview.md)
- [Android SDK Azure mobiilsideseadmete kaasamine](mobile-engagement-android-sdk-overview.md)
