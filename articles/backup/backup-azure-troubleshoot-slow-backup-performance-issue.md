<properties
   pageTitle="Failide ja kaustade Azure varukoopia aeglane varukoopia tõrkeotsing | Microsoft Azure'i"
   description="Leiate juhised aitavad diagnoosida põhjustada Azure varukoopia jõudlusega seotud probleemide tõrkeotsing"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Failide ja kaustade Azure varukoopia aeglane varukoopia tõrkeotsing

Sellest artiklist leiate juhised, mis aitavad teil diagnoosida põhjustada varukoopia aeglase faile ja kaustu, kui kasutate Azure varukoopia tõrkeotsing. Azure'i varukoopia agent kasutamisel failide varundamist võib võtta oodatust rohkem aega. See viivitus võib olla põhjustatud ühte või mitut järgmistest:

-   [Arvutis, kus on varundamist on täitmise kitsaskohtadest.](#cause1)
-   [Mõne muu protsessi või viirusetõrjetarkvara segab Azure varukoopia protsess.](#cause2)
-   [Varundus agent töötab Azure virtuaalse masina (VM).](#cause3)  
-   [Varundate suure hulga (miljoneid) faile.](#cause4)

Enne alustamist tõrkeotsingu, soovitame, et te laadige alla ja installige [uusim Azure varukoopia agent](http://aka.ms/azurebackup_agent). Sagedased uuendused teha varukoopia agent erinevate seotud probleemide lahendamine, funktsioonide lisamine ja parandada jõudlust.

Samuti soovitame teil läbi vaadata veendumaks, et teil on probleeme, mis tahes levinud konfiguratsiooniprobleemide [Azure varukoopia teenuse FAQ](backup-azure-backup-faq.md) .

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Põhjus: Täitmise kitsaskohtadest arvutis

Arvutis, kus on varundamist kitsaskohad, võivad põhjustada viivitused. Näiteks arvuti võimalus lugeda või kirjutada ketta või saata andmed üle võrgu läbilaskevõime võivad põhjustada kitsaskohad.

Windows pakub sisseehitatud seda nimetatakse [Jõudluse monitori](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmoni) nende kitsaskohtade tuvastamiseks.

Siin on mõned jõudluse hinnale ja vahemikud, mis võib olla kasulik diagnoosimise kitsaskohad optimaalse varukoopiad.

| Counter  | Olek  |
|---|---|
|Loogilise ketta (füüsiline ketas)--% jõude   | • 100% jõude 50% jõude = terve</br>• 49% jõude 20% jõude = hoiatuse või jälgimine</br>• 19% jõude 0% jõude = kriitiline või välja Spec|
|  Loogilise ketta (füüsiline ketas)--% keskmise Ketas Sec lugeda või kirjutada |  • 0,001 ms-0,015 ms = terve</br>• 0,015 ms-0,025 ms = hoiatuse või jälgimine</br>• 0.026 ms või enam = kriitiline või välja Spec|
|  Loogilise ketta (füüsiline ketas) – Praegune ketas järjekorra pikkus (kõik aknad) | üle 6 minuti 80 taotlemine |
| Mälu--rakenduskausta mitte leheküljed baiti|• Alla 60% tarbitud rakenduskausta = terve<br>• 61 80% tarbitud rakenduskausta = hoiatuse või jälgimine</br>• Suurem kui 80% pool tarbitud = kriitiline või välja Spec|
| Mälu--rakenduskausta leheküljed baiti |• Alla 60% tarbitud rakenduskausta = terve</br>• 61 80% tarbitud rakenduskausta = hoiatuse või jälgimine</br>• Suurem kui 80% pool tarbitud = kriitiline või välja Spec|
| Mälu--saadaval MB| • 50% mälu saadaval või rohkem = terve</br>• 25% tasuta mälu = jälgimine</br>• 10% tasuta mälu = hoiatus</br>• Vähem kui 100 MB või 5% tasuta mälu = kriitiline või välja Spec|
|Protsessor--\%protsessori aeg (kõik aknad)|• Alla 60% tarbitud = terve</br>• 61 90% tarbitud = kuvari või ettevaatlik</br>• 91-100% tarbitud = kriitiline|


> [AZURE.NOTE] Kui olete kindlaks teinud, et infrastruktuur on süüdlane, soovitame defragmentida ketast regulaarselt paremaks täitmiseks.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Põhjus: Teise protsessi või segab Azure varukoopia viirusetõrjetarkvara

Oleme näinud mitu eksemplari, kus muude protsesside süsteemi Windows on negatiivselt mõjutada jõudlust Azure varukoopia agent protsess. Näiteks kui kasutate nii agent Azure varukoopia ja muus programmis andmete varundamine või kui viirusetõrjetarkvara töötab ja on lukustus failidega varundada, mitu lukud klõpsake failide võib põhjustada argument. Sellisel juhul võib nurjuda varundamine või töö võib võtta oodatust rohkem aega.

Parim soovitus selle stsenaariumi on muu varukoopia programmi näha, kas varukoopia kestuse Azure varukoopia agent muudab välja lülitada. Tavaliselt kindlasti, et samal ajal ei tööta mitme varukoopia töö on piisav, et takistada neid üksteisest muutmata.

Viirusetõrje programmid, soovitame välja jätta järgmised failid ja kohad:

- C:\Program Files\Microsoft Azure taastamise teenused Agent\bin\cbengine.exe protsess
- C:\Program Files\Microsoft Azure taastamise teenused Agent\ kaustad
- Nullist asukoht (kui te ei kasuta standard asukoha)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Põhjus: Varundamise agent Azure virtuaalne arvutis töötab

Kui kasutate varundamise agent VM, jõudlust on aeglasem kui käivitate selle füüsilise arvutisse. See on ootuspärane IOPS piirangud.  Siiski saate optimeerida jõudlus üleminek andmete draivid, mis on Azure Premium mäluruumi varundamist. Töötame selle probleemi lahendamine ja tulevikus on võimalik lahendus.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Põhjus: Varundada suure hulga (miljoneid) failid

Suurel hulgal andmete teisaldamine kauem teisaldamine andmete maht on väiksem kui. Mõnel juhul varukoopia aeg on seotud mitte ainult andmeid, kuid failide või kaustade arv suurust. See on eriti true, kui on varundamist miljoneid väikesed failid (mõne KB mõne baiti).

Kui olete andmete varundamine ja teisaldada see Azure, Azure on korraga kataloogi kandmine failide põhjuseks seda käitumist. Mõnel juhul harvaesinevate kataloogi toimingu võib võtta oodatust rohkem aega.

Järgmisi näitajaid aitavad teil mõista kitsaskohaks ja vastavalt töötada järgmiste juhiste juurde.

- **Kasutajaliidese on kuvatud edenemise andmete edastamiseks**. Andmete kantakse endiselt. Võrgu läbilaskevõime või andmete maht võib põhjustada viivitused.

- **Kasutajaliidese ei kuvata andmeedastus edenemist**. Avage logid C:\Microsoft Azure taastamise teenused Agent\Temp asub, ja seejärel märkige FileProvider::EndData kirje logid. Kirje tähendab, et andmete edastamiseks lõpetanud ja toimub kataloogi toiming. Varukoopia töö ära tühistada. Selle asemel, oodake veidi enam kataloogi toimingu lõpuleviimiseks. Kui probleem ei lahene, pöörduge [Azure'i tugi](https://portal.azure.com/#create/Microsoft.Support).
