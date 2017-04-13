<properties
   pageTitle="3 installimiseks StorSimple seadmes | Microsoft Azure'i"
   description="Selgitab, kuidas installida StorSimple 8000 sarja 3 StorSimple 8000 sarja seadmes."
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
   ms.date="10/05/2016"
   ms.author="alkohli" />

# <a name="install-update-3-on-your-storsimple-device"></a>3 installimiseks StorSimple seadmes

## <a name="overview"></a>Ülevaade

Selle õpetuse selgitab, kuidas installida 3 StorSimple seadmesse tarkvara varasema versiooni Azure klassikaline portaali kaudu ja kiirparandus meetodit kasutades. Lüüsi on konfigureeritud võrgu liides, v.a StorSimple seadme andmete 0 kui proovite värskendada eelse Update 1 tarkvara versiooni kasutatakse kiirparandus meetodit.

3 uuendusest seadme tarkvara, LSI draiveri ja püsivara Storport ja Spaceport värskendamist. Kui värskendamine Update 2 või varasemas versioonis, ka olla vajalik iSCSI WMI, rakendamine ja teatud juhtudel kettale püsivara värskendused. Seadme tarkvara, WMI, iSCSI, LSI draiveri, Spaceport ja Storport parandused on-häiriva värskendused ja Azure klassikaline portaali kaudu saab rakendada. Ketas püsivara värskendused on häiriva värskendusi ja saab rakendada ainult Windows PowerShelli liidese seadme kaudu. 

> [AZURE.IMPORTANT]

> - Kogum, käsitsi ja automaatne enne kontrolli toimub enne installimist määratlemiseks seadme seisundi osas riigi ja võrgu riistvara Ühenduvus. Need enne kontrollitakse ainult juhul, kui rakendate Värskenduste Azure klassikaline portaalist.
> - Soovitame installida Azure klassikaline portaali kaudu tarkvara ja draiveri värskendusi. Windows PowerShelli kasutajaliidese seadme (värskenduste installimiseks) saate ainult minge portaali sisse eelse update lüüsi nurjumisel. Sõltuvalt värskendate kaudu versioon, värskendusi võib kuluda 1,5-2,5 tundi installimiseks. Hoolduse režiimi värskendusi peab olema installitud Windows PowerShelli liidese seadme kaudu. Hoolduse režiimi värskendused on häiriva värskendused, tulemuseks need oma seadmesse alla aeg.
> - Kui valikuline StorSimple hetktõmmise ülemus, veenduge, et versiooniks teie haldur hetktõmmise versioon Update 2 enne uuendamist seade.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Azure'i klassikaline portaali kaudu installida 3

Järgmiste toimingute värskendada oma seadme [3 värskendada](storsimple-update3-release-notes.md).


> [AZURE.NOTE]
Kui seote Update 2 või uuemat versiooni (sh Update 2.1), on võimalik Microsoft tõmmata täiendavad diagnostikateave seadmest. Selle tulemusena kui meie meeskond toimingute tuvastab seadmed, mida teil on probleeme, oleme paremini valmis seadme teabe kogumine ja diagnoosimine probleeme. Nõustumine värskenduse 2 või uuemad versioonid, lubate meile selle aktiivne toetada.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Veenduge, et teie seade töötab **StorSimple 8000 seeria Update 3 (6.3.9600.17759)**. **Viimati värskendatud kuupäeva** tuleks ka muuta. 

    Kui värskendate versioonilt enne Update 2, näete ka, et hooldustööd režiimi värskendused on saadaval (see sõnum edasi võib kuvada kuni 24 tundi pärast värskenduste installimist).

    Hoolduse režiimi värskendused on häiriva värskendused, mis seadme tööseisakute, saab rakendada ainult Windows PowerShelli liidese seadme kaudu. Mõnel juhul kui teil on Update 1.2, oma ketta püsivara võib juba olla ajakohane, mille puhul te ei pea hoolduse režiimis värskenduste installimiseks.

    Kui olete värskendamine Update 2 või uuem versioon, seadme peaks nüüd olema ajakohane. Saate ülejäänud juhised vahele jätta.

13. Laadige [alla laadida käigultparandused](#to-download-hotfixes) toodud juhised abil otsimine ja allalaadimine KB3121899, milliseid installe ketta püsivara värskendusi (muud värskendused peaks olema juba installitud nüüd) hooldustööde režiimi värskendused.

13. Loendis [installimine ja hooldus režiimi käigultparandused kinnitamine](#to-install-and-verify-maintenance-mode-hotfixes) hoolduse režiimi värskenduste installimiseks järgige. 

  

## <a name="install-update-3-as-a-hotfix"></a>Paik installida 3

Kasutage seda toimingut, kui teil ei õnnestu lüüsi sisse, kui proovite installida Azure klassikaline portaali kaudu. Kontrollimine nurjub, kui teil on määratud-andmete 0 võrgu liides lüüsi ja teie seade töötab tarkvara versioon enne Update 1.

Tarkvara saab üle kiirparandus meetodil on:

- Värskendage 0,1, 0,2, 0,3
- 1, 1.1, 1.2 värskendamine
- 2, 2.1, 2.2 värskendamine 

> [AZURE.IMPORTANT]
>
> - Kui teie seade töötab versioon Release (GA), võtke ühendust [Microsofti tugiteenuste](storsimple-contact-microsoft-support.md) teid aidata värskendus.

Kiirparandus meetod sisaldab kolme järgmist:

1.  Kiirparandusi alla laadida Microsoft Update'i kataloogi.

2.  Installige ja veenduge, et kiirparanduste tavalises režiimis.

3.  Installige ja veenduge hooldustööd režiimi kiirparandus (ainult siis, kui värskendamine eelse Update 2 tarkvara).


#### <a name="download-updates-for-your-device"></a>Laadige alla oma seadme värskendused

**Kui teie seade töötab Update 2.1 või 2.2**, mida tuleb alla laadida ja installida järgmised ettenähtud järjestuses:

| Tellimuse  | KB        | Kirjeldus                    | Värskenduse tüüp  | Installige kellaaja |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3186843 | Tarkvara värskendamine ja #42;  |  Tavaline <br></br>Mitte häiriva     | ~ 45 minutit |
| 2.      | KB3186859 | LSI draiveri ja püsivara             |  Tavaline <br></br>Mitte häiriva      | ~ 20 minutit |
| 3.      | KB3121261 | Storport ja Spaceport parandamine </br> Windows Server 2012 R2 |  Tavaline <br></br>Mitte häiriva      | ~ 20 minutit |

& #42;  *Märkus, tarkvara värskendus sisaldab kahte binaarsed failid: seadme tarkvara värskendus, koos sissejuhatava põhjendusega `all-hcsmdssoftwareupdate` ja tarkvara Cis ja MDS-i agent koos sissejuhatava põhjendusega `all-cismdsagentupdatebundle`. Enne tarkvara Cis ja MDS-i agent peab olema installitud seadme tarkvara uuendamine. Peate aktiivse domeenikontrolleri kaudu soovitud `Restart-HcsController` cmdlet pärast agent Cis ja MDS-i värskendamine (ja enne rakendamise ülejäänud värskendused).* 


**Kui teie seade töötab Update 0,1, 0,2, 0,3, 1.0, 1.1, 1.2, või 2.0**, saate peab alla laadida ja installida järgmist tarkvara, LSI draiveri ja püsivara värskendused (eelmises tabelis kuvatud), lisaks ettenähtud järjestuses:

| Tellimuse  | KB        | Kirjeldus                    | Värskenduse tüüp  | Installige kellaaja |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3146621 | iSCSI pakett | Tavaline <br></br>Mitte häiriva  | ~ 20 minutit |
| 5.      | KB3103616 | WMI pakett |  Tavaline <br></br>Mitte häiriva      | ~ 12 min |


<br></br>

**Kui teie seade töötab versioone 0,2, 0,3, 1.0, 1.1 ja 1.2**, peate ka ketta püsivara värskenduste peal eelnev tabelites värskendusi. Saate kontrollida, kas teil on vaja ketta püsivara värskenduste käivitades funktsiooni `Get-HcsFirmwareVersion` cmdlet-käsk. Kui teil on nende versioonide püsivara: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, siis pole peate installima järgmised värskendused.


| Tellimuse  | KB        | Kirjeldus                    | Värskenduse tüüp  | Installige kellaaja |
|--------|-----------|-------------------------|------------- |-------------|
| 6.      | KB3121899 | Ketta püsivara              |  Hooldustööd <br></br>Häiriva      | ~ 30 min. |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Seda toimingut tuleb teha vaid üks kord Update 3 rakendamiseks. Azure'i klassikaline portaali abil saate edaspidised värskendused.
> - Kui värskendamine, värskendus 2.2, aega kokku installi lähedane 1.1 tundi.
> - Enne selle toimingu abil saate värskenduse, veenduge, et nii seadme kontrollerid on võrgus ja kõik riistvara on terve.

Järgmiste toimingute abil alla laadida ja installida.

[AZURE.INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Järgmised sammud

Lisateave [värskenduste 3 väljaanne](storsimple-update3-release-notes.md).
