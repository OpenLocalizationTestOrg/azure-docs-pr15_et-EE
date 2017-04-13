<properties
    pageTitle="Azure'i faili salvestusruumi tõrkeotsing | Microsoft Azure'i"
    description="Azure'i faili salvestusruumi probleemide tõrkeotsing"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Azure'i faili salvestusruumi seotud probleemide tõrkeotsing

Selles artiklis on loetletud levinumad probleemid, mis on seotud Microsoft Azure'i failide talletamine, kui loote ühenduse Windows ja Linuxi. See sisaldab ka võimalikud põhjused ja lahendused neid probleeme.

**Üldised probleemid (ilmneda nii Windows ja Linux kliendid)**

- [Kui proovite avada faili kvoodi tõrge](#quotaerror)

- [Aeglase, kui pöördute Linux või Windows Azure'i failide talletamine](#slowboth)

**Windows kliendi probleemid**

- [Aeglase juurdepääsuks Windows 8.1 või Windows Server 2012 R2 Azure'i failide talletamine](#windowsslow)

- [Tõrge 53 proovite ühendada mõne Azure'i faili ühiskasutusse andmine](#error53)

- [NET kasutamine õnnestus, kuid ei näe Azure faili jagada ühendatud Windows Exploreris](#netuse)

- [Minu konto salvestusruumi sisaldab "/" ja net kasutamine käsk nurjub?](#slashfails)

- [Minu rakenduste/teenus ei pääse ühendatud Azure'i failide draivile.](#accessfiledrive)

- [Lisasoovitused optimeerida jõudlust](#additional)

**Linux kliendi probleemid**

- [Tõrge "Kopeerite faili sihtkohta, mis ei toeta krüptimist" kui failide Azure'i failide üleslaadimise/kopeerimine](#encryption)

- [Olemasoleva faili tõrge "Host on alla" jagab või shell hangub tehes Käsuloend ühenduspunkti kohta](#errorhold)

- [Mount mount Azure'i failide Linuxi katsel tõrketeade 115](#error15)

- [Linux VM probleeme juhusliku viivitust käsud nagu "ls"](#delayproblem)

## <a name="general-problems"></a>Üldised probleemid
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Kui proovite avada faili kvoodi tõrge

Klõpsake Windowsi kuvatakse tõrketeated, mis näeb välja järgmine:

**1816 ERROR_NOT_ENOUGH_QUOTA <> - 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Pole piisavalt piirmäär on selle käsu töötlemiseks**

**Sobimatu pide väärtus GetLastError: 53**

Linux, kuvatakse tõrketeade, mis näeb välja järgmine:

**<filename>[juurdepääs keelatud]**

**Ketta kvoodi ületamise**

#### <a name="cause"></a>Põhjus

Probleem ilmneb, kuna olete jõudnud samaaegseid avatud pidemetega faili jaoks lubatud ülempiiri.

#### <a name="solution"></a>Lahendus

Mõned pidemete sulgedes samaaegseid avatud pidemete arvu vähendamiseks ja proovige uuesti. Lisateavet leiate teemast [Microsoft Azure'i salvestusruumi jõudlus ja skaleeritavus kontroll-loend](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Aeglane, kui juurdepääsu Windowsi või Linuxi failide talletamine

- Kui teil pole teatud minimaalne I/O suurus nõue, soovitame kasutada optimaalse jõudluse tagamiseks I/O suurus 1 MB.

- Kui teate, et fail, mis on ulatub, mis kirjutab lõpliku suuruse ja tarkvara pole ühilduvusega seotud probleemid, kui see pole veel kirjutatud saba nullide sisaldava faili, määrake eelnevalt asemel iga kirjutamine failimaht on mõni laiendamine kirjutamine.

## <a name="windows-client-problems"></a>Windows kliendi probleemid
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Failide talletamine kasutamisel Windows 8.1 või Windows Server 2012 R2 aeglane jõudlus

Klientidele, kes on opsüsteemi Windows 8.1 või Windows Server 2012 R2, veenduge, et kiirparandus [KB3114025](https://support.microsoft.com/kb/3114025) on installitud. See paik parandab Loo ja sulgege pidet jõudlust.

Saate kasutada järgmist skripti kontrollida, kas kiirparandus on installitud.

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Kui kiirparandus on installitud, kuvatakse järgmine väljund:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1**

> [AZURE.NOTE]  Windows Server 2012 R2 Azure'i turuplatsi pildid on kiirparandus KB3114025 alates detsember 2015 vaikimisi installitud.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Lisasoovitused optimeerida jõudlust

Kunagi luua või avada faili vahemällu talletatud i/o, mis taotleb kirjutamisõigused, kuid mitte lugemisõigus. See tähendab, kui saate helistada **CreateFile()**, kunagi määrata ainult **GENERIC_WRITE**, kuid alati määrata **GENERIC_READ | GENERIC_WRITE**. Kirjutage ainult pidet ei saa vahemälu small kirjutab kohalikult, isegi siis, kui see on ainult avatud faili pidet. See väike kirjutab paneb raske jõudluse karistus. Pange tähele, et CRT **fopen()** "a" režiimi avaneb ainult kirjutamiseks pidet.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>"53 tõrge" kui proovite ühendamine või lahutada on Azure faili ühiskasutusse andmine

See probleem võib olla põhjustatud järgmistest tingimustest:

#### <a name="cause-1"></a>Põhjus 1

"Ilmnes süsteemi tõrge 53. Juurdepääs keelatud." Turvalisuse huvides ühendused Azure'i failide ühtlasi on blokeeritud kui side kanali pole krüptitud ja ühenduse loomise katse ei ole tehtud sama andmekeskuse, kus asuvad Azure'i failikettad. Kui kasutaja kliendi OS ei toeta krüptimist SMB side kanali krüptimise ei esitata. Seda näitab on "System 53 ilmnes tõrge. Juurdepääs keelatud"tõrketeade, kui kasutaja üritab kohapealse või muu andmekeskuse failikettale ühendada. Windows 8, Windows Server 2012 ja uuemad versioonid iga negotiate taotlus, mis sisaldab SMB 3.0, mis toetab krüptimist.

#### <a name="solution-for-cause-1"></a>Lahendus 1 põhjusega

Ühendage klient, mis vastab Windows 8, Windows Server 2012 või uuemad versioonid, või mis ühenduse, virtuaalse arvutist, mis on sama nimega Azure Storage konto, mida kasutatakse andmekeskuse Azure'i faili jagada.

#### <a name="cause-2"></a>Põhjus 2

"Süsteemi tõrge 53" kui ühendate on Azure failikettal võib juhtuda, kui Port 445 väljaminev suhtlus Azure'i failide andmekeskuse on blokeeritud. Klõpsake [siin](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) näha, kes lubamiseks või keelamiseks juurdepääsu port 445 kokkuvõte.

Comcast ja mõned IT organisatsioonid blokeerida seda porti. Et aru saada, kas see on teade "Süsteemi tõrge 53" põhjuseks, saate Portqry päringu TCP:445 lõpp-punkti. Kui TCP:445 lõpp-punkti ei kuvata, nagu on filtreeritud, TCP pordi on blokeeritud. Siin on näide päring.

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Kui TCP 445 blokeeritud reegli võrgu teed, kuvatakse järgmine väljund.

**TCP-port 445 (microsoft-ds teenus): FILTREERITUD**

Portqry kasutamise kohta lisateabe saamiseks vt [Portqry.exe käsurea kasuliku kirjeldus](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Lahendus 2 põhjusega

Saate töötada ettevõtte IT avamiseks Port 445 väljaminev [Azure'i IP-vahemikke](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="cause-3"></a>Põhjus 3

"Süsteemi tõrge 53" saate saanud ka siis, kui NTLMv1 side on lubatud. Kas teil on lubatud NTLMv1 loob vähem turvaline kliendi. Seetõttu side on blokeeritud failide Azure'i. Kontrollimaks, kas see on tõrke põhjuse, kontrollige järgmist registri alamvõtit on määratud väärtus 3:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Lisateabe saamiseks lugege teemat [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) TechNeti.

#### <a name="solution-for-cause-3"></a>Lahendus põhjus 3

Selle probleemi lahendamiseks failitüübid LmCompatibilityLevel väärtuse HKLM\SYSTEM\CurrentControlSet\Control\Lsa registrivõtme 3 vaikeväärtus.

Azure'i failide toetab ainult NTLMv2 autentimist. Veenduge, et kliendid on rakendatud rühmapoliitika. See aitab vältida selle vea tekkimist. Seda peetakse turvalisus parimaid tavasid. Lisateavet leiate teemast [konfigureerimine klientidele NTLMv2 rühmapoliitikaga](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

Soovitatav poliitikasäte on **saada NTLMv2 vastuse ainult**. See vastab Registrisätte väärtuseks 3. Kliendid kasutada ainult NTLMv2 autentimist ja neid kasutada NTLMv2 kui server toetab seda. Domeenikontrollerite aktsepteerida LM NTLM ja NTLMv2 autentimist.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>NET kasutamine õnnestus, kuid ei näe Azure faili jagada ühendatud Windows Exploreris

#### <a name="cause"></a>Põhjus

Vaikimisi Windows Exploreris Käivita administraatorina. Kui käivitate **net kasutamine** administraatori Käsuviip, vastendate võrgudraivi "Administraatorina". Kuna vastendatud draivid on kasutaja-kesksele, kasutajakonto, mis on sisse logitud ei kuvata draivid kui nad on paigaldatud jaotises kontot.

#### <a name="solution"></a>Lahendus

Ühendage administraator käsureal ühiskasutus. Teise võimalusena võite järgida [selles TechNeti teemas](https://technet.microsoft.com/library/ee844140.aspx) **EnableLinkedConnections** registriväärtuse konfigureerimiseks.

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Minu konto salvestusruumi sisaldab "/" ja net kasutamine käsk nurjub?

#### <a name="cause"></a>Põhjus

Käsu **net use** käitamisel jaotises Käsuviip (cmd.exe) selle sõeluda, lisades "/" käsurea suvandit. See põhjustab ketas vastenduse nurjumise.

#### <a name="solution"></a>Lahendus

Kas järgmiste juhiste abil saate selle probleemi lahendamiseks:

• Järgmist PowerShelli käsu kasutamine

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Paketi faili seda saab teha kujul

`Echo new-smbMapping ... | powershell -command –`

Pane võti selle probleemi lahendamiseks ümber kahekordsed jutumärgid – kui "/" on esimene märk. Kui see on, kasutada interaktiivsete režiimi ja sisestage oma parool eraldi või taastada oma klahvid võtme, mis ei käivitu kaldkriips (/).

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Minu rakenduste/teenus ei pääse ühendatud Azure'i failide draivile

#### <a name="cause"></a>Põhjus

Draivid on paigaldatud kasutaja kohta. Kui teie rakenduse või teenuse töötab jaotises kasutajakontot, kasutajad ei näe ketas.

#### <a name="solution"></a>Lahendus

Ühendage ketas, mille rakendus on sama kasutajakonto kaudu. Seda saab teha tööriistadega nagu psexec.

Teise võimalusena saate luua uus kasutaja, et samade õigused võrgu opsüsteemis teenuse või konto on, ja seejärel käivitage **cmdkey** ja **net kasutamine** selle konto alla. Kasutaja nimi peab olema salvestusruumikonto nimi ja parool peaksid olema salvestusruumi konto võti. Teine võimalus **net** kasutamiseks on salvestusruumikonto nimi ja sisestage kasutaja nimi ja parool parameetrid **net kasutamine** käsk edasi.

Pärast nende juhiste täitmist võidakse kuvada järgmine tõrketeade: "ilmnes tõrge süsteemi 1312. Määratud sisselogimise seansi pole olemas. See võib on juba lõpetatud" **net kasutamine** süsteemi/võrgu teenusekonto käivitamisel. Sel juhul veenduge, et domeeni teave sisaldab kasutajanime, mis **net** kasutamine (nt: "[salvestusruumikonto nimi]. file.core.windows .net").

## <a name="linux-client-problems"></a>Linux kliendi probleemid

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Tõrge "Kopeerite faili sihtkohta, mis ei toeta krüptimist"

#### <a name="cause"></a>Põhjus

Azure'i faile saate kopeerida BitLockeri krüptitud failide. Siiski ei toeta failide talletamine NTFS EFS. Seetõttu tõenäoliselt kasutate EFS sel juhul. Kui teil on EFS krüptitud failide, kopeerimisel, et failide talletamine võib nurjuda, kui käsku Kopeeri on dekrüptimine kopeeritud faili.

#### <a name="workaround"></a>Lahendus

Faili kopeerimine failide talletamine, peate esmalt dekrüptida seda. Saate seda teha, kasutades ühte järgmistest.

• Kasutada **Kopeeri d**.

• Määrata järgmisele registrivõtmele:

- Tee = HKLM\Software\Policies\Microsoft\Windows\System
- Väärtuse tüüp = DWORD
- Nimi = CopyFileAllowDecryptedRemoteDestination
- Väärtus = 1

Pange tähele, et registrivõtme säte mõjutab kõiki Kopeeri toiminguid võrgu osa.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Olemasoleva faili tõrge "Host on alla" jagab või shell hangub, kui käivitate Käsuloend ühenduspunkti

#### <a name="cause"></a>Põhjus

See tõrge ilmneb Linuxi klient, kui kliendi on pikema perioodi jooksul jõude olnud. Selle tõrke ilmnemisel kliendi katkestab ja kliendi ühenduse ajalõpp.

#### <a name="solution"></a>Lahendus

See probleem on lahendatud nüüd sisse Linuxi tuum [muutmine kogum](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), ootel backport üheks Linux jaotuse osana.

Selle probleemi lahendamiseks töötab, säilitada ühenduse ja vältida sattumist jõudeolekus, hoida faili Azure'i Failide ühiskasutamine, mida peaksite kirjutama perioodiliselt. See peab olema kirjutamine toiming, nt ümberkirjutamine faili loodud/muutmise kuupäev. Muul juhul võite saada vahemällu salvestatud tulemuste ja toiming võib käivitab ühendus.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>"Mount 115 tõrge" kui püüate ühendada Azure'i failide Linuxi

#### <a name="cause"></a>Põhjus

Linuxi veel ei toeta krüptimisfunktsiooni SMB 3.0. Mõned viimati kasutaja võib saada "115" tõrketeade, kui püüate mount Azure'i failide SMB 3.0 tõttu kadunud funktsiooni abil.

#### <a name="solution"></a>Lahendus

Kui Linux SMB klient, mida kasutatakse ei toeta krüptimist, mount Azure'i failide SMB 2.1 Linux VM sama andmekeskuse faili salvestusruumi konto abil.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux VM probleeme juhusliku viivitust käsud nagu "ls"

#### <a name="cause"></a>Põhjus

See võib juhtuda siis, kui ühendamine käsu ei sisalda **serverino** suvand. Ilma **serverino**, käivitatakse käsk ls **stat** iga faili.

#### <a name="solution"></a>Lahendus

Kontrollige **serverino** oma "/ jne/fstab" kirje.

azureuser.File.Core.Windows.net/WMS/Comer/home/sampledir tüüp cifs kohta (rw, nodev, relatime, vers = 2.1, sec = ntlmssp, vahemälu = range, kasutajanimi = xxx, domeeni X, file_mode = = 0755 dir_mode = 0755, serverino, rsize = 65536 wsize = 65536 actimeo = 1)

Kui **serverino** suvand puudub, lahutada ja mount Azure'i faile uuesti, millel on valitud suvand **serverino** .

## <a name="learn-more"></a>Lisateave

- [Alustamine Windows Azure'i failide talletamine](storage-dotnet-how-to-use-files.md)

- [Azure'i failide talletamine Linux kasutamise alustamine](storage-how-to-use-files-linux.md)
