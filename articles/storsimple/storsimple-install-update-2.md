<properties
   pageTitle="Installida 2 StorSimple seadmes | Microsoft Azure'i"
   description="Selgitab, kuidas installida StorSimple 8000 sarja 2 StorSimple 8000 sarja seadmes."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="install-update-2-on-your-storsimple-device"></a>Installida 2 StorSimple seadmes

## <a name="overview"></a>Ülevaade

Selle õpetuse selgitab, kuidas installida seadmesse tarkvara varasem versioon töötab Azure klassikaline portaali kaudu StorSimple 2. Õpetusega ka käsitletakse toiminguid, mis on nõutav värskendus, kui lüüsi on konfigureeritud võrgu liides, v.a StorSimple seadme andmete 0 ja proovite värskendada eelse Update 1 tarkvara versiooni.

2 uuendusest seadme tarkvaravärskendused, LSI draiveri värskendusi ja ketta püsivara värskendused. Seadme tarkvara ja LSI värskenduste on-häiriva värskendusi ja Azure klassikaline portaali kaudu saab rakendada. Ketas püsivara värskendused on häiriva värskendusi ja saab rakendada ainult Windows PowerShelli liidese seadme kaudu.

> [AZURE.IMPORTANT]

> -  Te ei näe Update 2 kohe, kuna me etappidega plaanida värskendusi. Mõne päeva jooksul uuesti Värskenduste otsimine nimega selle värskenduse muutuvad kättesaadavaks varsti.
> - Kogum, käsitsi ja automaatne enne kontrolli toimub enne installimist määratlemiseks seadme seisundi osas riigi ja võrgu riistvara Ühenduvus. Need enne kontrollitakse ainult juhul, kui rakendate Värskenduste Azure klassikaline portaalist.
> - Soovitame installida Azure klassikaline portaali kaudu tarkvara ja draiveri värskendusi. Windows PowerShelli kasutajaliidese seadme (värskenduste installimiseks) saate ainult minge nurjumisel eelse update lüüsi sisse portaali. Värskenduste võib kuluda 4-7 tundi installimiseks (sh Windowsi värskenduste). Hoolduse režiimi värskendusi peab olema installitud Windows PowerShelli liidese seadme kaudu. Hoolduse režiimi värskendused on häiriva värskendused, tulemuseks need nähtud aeg oma seade.
> - Kui valikuline StorSimple hetktõmmise ülemus, veenduge, et versiooniks teie haldur hetktõmmise versioon Update 2 enne uuendamist seade.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Azure'i klassikaline portaali kaudu installida 2

Järgmiste toimingute uuendada oma seadme [Update](storsimple-update2-release-notes.md)2.


> [AZURE.NOTE]
Update 2 võimaldab Microsoftil täiendavad diagnostikateave seadme tõmmata. Selle tulemusena kui meie meeskond toimingute tuvastab seadmed, mida teil on probleeme, oleme paremini valmis seadme teabe kogumine ja diagnoosimine probleeme. Aktsepteerige Update 2, lubate meile selle aktiivne toetada.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Veenduge, et teie seade töötab **StorSimple 8000 seeria Update 2 (6.3.9600.17673)**. **Viimati värskendatud kuupäeva** tuleks ka muuta. Samuti näete, et hooldustööd režiimis värskendused on saadaval (see sõnum edasi võib kuvada kuni 24 tundi pärast värskenduste installimist).

    Hoolduse režiimi värskendused on häiriva värskendused, mis seadme tööseisakute, saab rakendada ainult Windows PowerShelli liidese seadme kaudu. Mõnel juhul kui teil on Update 1.2, oma ketta püsivara võib juba olla ajakohane, mille puhul te ei pea hoolduse režiimi värskenduste installimiseks.

13. Laadige [alla laadida käigultparandused](#to-download-hotfixes) toodud juhised abil otsimine ja allalaadimine KB3121899, milliseid installe ketta püsivara värskendusi (muud värskendused peaks olema juba installitud nüüd) hooldustööde režiimi värskendused.

13. Loendis [installimine ja hooldus režiimi käigultparandused kinnitamine](#to-install-and-verify-maintenance-mode-hotfixes) hoolduse režiimi värskenduste installimiseks järgige.


## <a name="install-update-2-as-a-hotfix"></a>Paik installida 2

Kasutage seda toimingut, kui teil ei õnnestu lüüsi sisse, kui proovite installida Azure klassikaline portaali kaudu. Kontrollimine nurjub, kui teil on määratud-andmete 0 võrgu liides lüüsi ja teie seade töötab tarkvara versioon enne Update 1.

Tarkvara versioonid, mida saab täiendada kiirparandus meetodil on värskendus 0,1, värskendus 0,2, ja Värskenda 0,3, Update 1, 1.1 värskenduse ja Update 1.2. Kiirparandus meetod sisaldab kolme järgmist:

- Kiirparandusi alla laadida Microsoft Update'i kataloogi.
- Installige ja veenduge, et kiirparanduste tavalises režiimis.
- Installige ja veenduge, et kiirparandus hooldustööd režiim.

Paik installida 2, tuleb alla laadida ja installida järgmisi Kiirparandusi:

| Tellimuse  | KB        | Kirjeldus                    | Värskenduse tüüp  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Tarkvara värskendus         |  Tavaline     |
| 2      | KB3121900 | LSI draiver              |  Tavaline     |
| 3      | KB3080728 | Storport parandamine </br> Windows Server 2012 R2 |  Tavaline     |
| 4      | KB3090322 | Spaceport parandamine </br> Windows Server 2012 R2 |  Tavaline     |
| 5      | KB3121899 | Ketta püsivara           | Hooldustööd  |


> [AZURE.IMPORTANT]
>
> - Kui teie seade töötab versioon Release (GA), võtke ühendust [Microsofti tugiteenuste](storsimple-contact-microsoft-support.md) teid aidata värskendus.
> - Seda toimingut tuleb teha vaid üks kord rakendada Update 2. Azure'i klassikaline portaali abil saate edaspidised värskendused.
> - Iga kiirparandus installi saavad võtta umbes 20 minutit lõpuleviimiseks. Installi kogu aeg on 2 tundi.
> - Enne selle toimingu abil saate värskenduse, veenduge, et mõlema seadme kontrollerid on võrgus ja kõik riistvara on terve.

Selle värskenduse paik järgmiste toimingute.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [Update 2 väljaanne](storsimple-update2-release-notes.md).
