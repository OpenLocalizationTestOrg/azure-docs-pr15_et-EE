<properties
   pageTitle="Installige värskendus 1.2 StorSimple seadmes | Microsoft Azure'i"
   description="Selgitatakse, kuidas installida StorSimple 8000 seeria Update 1.2 StorSimple 8000 sarja seadmes."
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
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Installige värskendus 1.2 StorSimple seadmes

## <a name="overview"></a>Ülevaade

Selle õpetuse selgitatakse, kuidas installida Update 1.2 StorSimple seadmesse, kus töötab tarkvara versioon enne Update 1. Õpetuse hõlmab ka lisatoimingud, mis on nõutav värskendus, kui lüüsi on konfigureeritud võrgu liides, v.a StorSimple seadme andmete 0.

1.2 uuendusest seadme tarkvara, LSI draiveri värskendusi ja ketta püsivara värskendused. Tarkvara ja LSI draiveri värskendused on-häiriva värskendusi ja Azure klassikaline portaali kaudu saab rakendada. Ketas püsivara värskendused on häiriva värskendusi ja saab rakendada ainult Windows PowerShelli liidese seadme kaudu.

Sõltuvalt sellest, millist versiooni teie seade töötab, saate määrata, kui Update 1.2 rakendatakse. Saate vaadata oma seadmesse tarkvara versiooni navigeerides seadme **armatuurlaua**jaotisse **kiire ülevaade** .

</br>

| Kui tarkvara versioon...   | Mis juhtub portaali?                              |
|---------------------------------|--------------------------------------------------------------|
| Väljalaske - GA                    | Kui kasutate versiooni (GA), pole selle värskenduse. Palun uuendada oma seadme [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) .|
| Värskendus 0,1                      | Portaali kehtib Update 1.2.                                |
| 0,2 värskendamine                      | Portaali kehtib Update 1.2.                                |
| Värskendage 0,3                      | Portaali kehtib Update 1.2.                                |
| Värskendage 1                        | Värskendust ei ole saadaval.                           |
| 1.1 värskendus                      | Värskendust ei ole saadaval.                           |

</br>

> [AZURE.IMPORTANT]

> -  Te ei näe Update 1.2 kohe, kuna me etappidega plaanida värskendusi. Mõne päeva jooksul uuesti Värskenduste otsimine nimega selle värskenduse muutuvad kättesaadavaks varsti.
> - Selle värskenduse sisaldab kogum, käsitsi ja automaatne enne kontrolli määratlemiseks seadme seisundi osas riigi ja võrgu riistvara Ühenduvus. Need enne kontrollitakse ainult juhul, kui rakendate Värskenduste Azure klassikaline portaalist.
> - Soovitame installida Azure klassikaline portaali kaudu tarkvara ja draiveri värskendusi. Windows PowerShelli kasutajaliidese seadme (värskenduste installimiseks) saate ainult minge nurjumisel eelse update lüüsi sisse portaali. Värskenduste võib kuluda 5 – 10 tundi installimiseks (sh Windowsi värskenduste). Hoolduse režiimi värskendusi peab olema installitud Windows PowerShelli liidese seadme kaudu. Hoolduse režiimi värskendused on häiriva värskendused, tulemuseks need nähtud aeg oma seade.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Installige värskendus 1.2 Azure klassikaline portaali kaudu

Järgmiste toimingute värskendada oma seadme 1.2 [Värskendada](storsimple-update1-release-notes.md). Selle toiminguga ainult siis, kui teil on konfigureeritud andmete 0 võrgu liides seadme lüüsi.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Veenduge, et teie seade töötab **StorSimple 8000 seeria Update 1.2 (6.3.9600.17584)**. **Viimati värskendatud kuupäeva** tuleks ka muuta. Samuti näete, et hooldustööd režiimis värskendused on saadaval (see sõnum edasi võib kuvada kuni 24 tundi pärast värskenduste installimist).

    Hoolduse režiimi värskendused on häiriva värskendused, mis seadme tööseisakute, saab rakendada ainult Windows PowerShelli liidese seadme kaudu.

    ![Lehe hooldustööd] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Lehe hooldustööd")

13. Laadige [alla laadida käigultparandused]( #to-download-hotfixes) toodud juhised abil otsimine ja allalaadimine KB3063416, milliseid installe ketta püsivara värskendusi (muud värskendused peaks olema juba installitud nüüd) hooldustööd režiimi värskendused.

13. Loendis [installimine ja hooldus režiimi käigultparandused kinnitamine](#to-install-and-verify-maintenance-mode-hotfixes) hoolduse režiimi värskenduste installimiseks järgige.

14. Azure'i klassikaline portaalis, liikuge lehele **hooldustööd** ja klõpsake lehe allosas, klõpsake **Värskenduste skannida** mis tahes Windowsi värskendusi ja seejärel klõpsake nuppu **Installi värskendused**. Olete lõpetanud, kui kõik värskendused on installitud.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Update 1.2 installida seadmesse, mis on konfigureeritud-andmete 0 võrgu kasutajaliidese lüüsi

Kasutage seda toimingut ainult siis, kui teil ei õnnestu lüüsi sisse, kui proovite installida Azure klassikaline portaali kaudu. Kontrollimine nurjub, kui teil on määratud-andmete 0 võrgu liides lüüsi ja teie seade töötab tarkvara versioon enne Update 1. Kui teie seade pole lüüsi-andmete 0 võrgu kasutajaliidese, saate värskendada oma seadme otse Azure klassikaline portaali. Vaadake teemat [installimine 1.2 Azure klassikaline portaali kaudu värskendada](#install-update-1.2-via-the-azure-classic-portal).

Tarkvara saab uuendada selle meetodi kasutamise on värskendus 0,1, värskendus 0,2 ja 0,3 värskendus.


> [AZURE.IMPORTANT]
>
> - Kui teie seade töötab versioon Release (GA), võtke ühendust [Microsofti tugiteenuste](storsimple-contact-microsoft-support.md) teid aidata värskendus.
> - Seda toimingut tuleb teha vaid üks kord Update 1.2 rakendamiseks. Azure'i klassikaline portaali abil saate edaspidised värskendused.

Kui teie seade töötab eelse Update 1 tarkvara ja see on lüüsi määramine võrgu liidese peale andmete 0, saate rakendada Update 1.2 toimetada kahel viisil:

- **Suvand 1**: selle värskenduse allalaadimiseks ja rakendada selle abil soovitud `Start-HcsHotfix` seadme liides Windows PowerShelli cmdlet-käsu. See on soovitatav meetod. **Kasutage seda meetodit Update 1.2 rakendada, kui teie seade töötab Update 1.0 või 1.1 värskenduse.**

- **Variant 2**: eemaldada lüüsi konfigureerimine ja installige värskendus otse Azure klassikaline portaalist.


Järgmistes jaotistes on esitatud üksikasjalikke juhiseid iga.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Variant 1: Windows PowerShelli StorSimple Update 1.2 paik rakendamiseks

Kasutage seda toimingut ainult siis, kui teil on värskendus 0,1, 0,2, 0,3 ja kui oma lüüsi sisse on nurjunud, kui proovite installida Azure klassikaline portaalist värskendused. Kui kasutate Release (GA) tarkvara, siis [Microsofti tugiteenuste](storsimple-contact-microsoft-support.md) uuendada oma seadme.

Paik installida Update 1.2, peate alla laadima ja installimiseks järgmisi Kiirparandusi:

| Tellimuse  | KB        | Kirjeldus             | Värskenduse tüüp  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Tarkvara värskendus         |  Tavaline     |
| 2      | KB3043005 | LSI SAS kontrolleril värskendamine |  Tavaline     |
| 3      | KB3063416 | Ketta püsivara           | Hooldustööd  |

Enne selle toimingu abil saate värskenduse, veenduge, et:

- Nii seadme kontrollerid on võrgus.

Järgmiste toimingute Update 1.2 rakendada. **Värskenduste võib võtta umbes 2 tundi lõpuleviimiseks (30 minutit tarkvara, 30 minuti draiveri, ketta püsivara aega kulub 45 minutit).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Variant 2: Kasutada Azure klassikaline portaali Update 1.2 rakendamiseks pärast eemaldamist lüüsi konfigureerimine

See toiming kehtib ainult StorSimple seadmete tarkvara versioon enne Update 1 ja on pannud võrgu liidese peale andmete 0 lüüsi. Peate enne värskenduse sätte lüüsi tühjendamine.

Värskenduse võib kuluda mõni tund lõpuleviimiseks. Kui teie hosts on teise alamvõrku, eemaldades lüüsi konfigureerimine iSCSI liideste võib põhjustada tööseisakute. Soovitame konfigureerida andmete iSCSI liikluse 0 on tööseisakute vähendada.

Järgmiste toimingute võrgu ühisosa lüüsi keelamine ja seejärel rakendage värskendus.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [Update 1.2 väljaanne](storsimple-update1-release-notes.md).
