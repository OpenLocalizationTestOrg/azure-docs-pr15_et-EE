<properties 
   pageTitle="StorSimple juurutamise tõrkeotsing | Microsoft Azure'i"
   description="Kirjeldab, kuidas diagnoosimine ja parandamine, mis tehakse, kui juurutate esmalt StorSimple."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>StorSimple seadme juurutamise probleemide tõrkeotsing

## <a name="overview"></a>Ülevaade

Sellest artiklist leiate abi tõrkeotsingu juhiseid oma Microsoft Azure'i StorSimple juurutamiseks. See kirjeldatakse levinumaid probleeme, võimalikud põhjused ja soovituslikku toimingut abil saate lahendada probleeme, mis võivad ilmneda StorSimple konfigureerimisel. See teave kehtib StorSimple kohapealse seadme nii StorSimple virtuaalse seade.

> [AZURE.NOTE] Seadme konfiguratsiooni seotud probleemid, mis võib tekkida võib tekkida siis, kui juurutate seadme esimest korda, või need võivad tekkida hiljem, kui seade on töökorras. See artikkel keskendub esimest korda juurutamise probleemide tõrkeotsing. Tõrkeotsing funktsionaalseid seadmes, minge [funktsionaalseid seadme probleemide tõrkeotsing](storsimple-troubleshoot-operational-device.md).

Selles artiklis kirjeldatakse StorSimple juurutuste tõrkeotsingu tööriistade ka ja samm-sammult tõrkeotsingu näide.

## <a name="first-time-deployment-issues"></a>Esimest korda juurutamise probleemid

Kui tekib probleeme seadme juurutamisel esimest korda, arvestage järgmist.

- Kui sooritate füüsilise seadmega, veenduge, et riistvara on installitud ja [installige seadme StorSimple 8100](storsimple-8100-hardware-installation.md) või [installida seadme StorSimple 8600](storsimple-8600-hardware-installation.md)kirjeldatud konfigureeritud.
- Kontrollige eeltingimused juurutamiseks. Veenduge, et teil on kõik andmed, mis on kirjeldatud [juurutamise konfiguratsiooni kontroll-loend](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
- Vaadake üle StorSimple väljalaskemärkmed kui probleemi kirjelduse kuvamiseks. Funktsiooni väljalaskemärkmed kaasata tuntud installiprobleemide lahendused. 

Seadme juurutamisel, kõige levinum probleemid selle kasutajad näo ilmneda juhul, kui nad töötavad häälestusviisardi ja millal registreerida StorSimple seadme Windows PowerShelli kaudu. (Kasutage Windows PowerShell StorSimple registreerida ja seadme StorSimple konfigureerida. Seadme registreerimise kohta leiate lisateavet teemast [Samm 3: konfigureerimine ja registreerida seadme Windows PowerShelli kaudu StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

Järgmised jaotised aitavad teil lahendada probleeme, mis ilmneda StorSimple seadme konfigureerimisel esimest korda.

## <a name="first-time-setup-wizard-process"></a>Esimest korda häälestamise viisardi protsess

Järgmised toimingud summarize häälestamise viisardi protsess. Üksikasjaliku häälestus leiate teemast [Deploy kohapealse StorSimple seadme](storsimple-deployment-walkthrough.md).

1. Käivitage [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) cmdlet-käsk, mis juhendab teid ülejäänud juhised häälestusviisardi käivitamiseks. 
2. Võrgu konfigureerimine: häälestusviisardi abil saate oma seadmes StorSimple andmete 0 võrgu liidese võrgu sätete konfigureerimine. Need sätted on järgmised:
  - Virtuaalne IP (VIP), alamvõrgu mask ja lüüsi – cmdlet [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) käivitatakse taustal. See konfigureerib IP address, alamvõrgu maski ja andmete 0 võrgu liidese lüüsi StorSimple seadmes.
  - Esmane DNS-i server – cmdlet [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) käivitatakse taustal. See konfigureerib teie StorSimple lahendus DNS-i sätted.
  - NTP serveri – cmdlet [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) käivitatakse taustal. See konfigureerib NTP serveri sätted oma StorSimple lahenduse.
  - Valikuline veebiteenuse puhverserveri – cmdlet [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) käivitatakse taustal. Seda määrab ja lubab web puhverserveri konfigureerimine teie StorSimple lahendus.
3. Paroolide häälestamine: järgmise sammuna tuleb häälestada seadme administraator ja StorSimple hetktõmmise halduri paroolid. Kui teil on Update 1, siis te pole peavad häälestamiseks StorSimple hetktõmmise halduri parooli.
  - Seadme administraatori parooli abil oma seadmesse sisse logida. Vaikimisi seadme parool on **parool1**.
  - StorSimple hetktõmmise halduri parool on nõutav seade kasutada StorSimple hetktõmmise halduri konfigureerimisel. Peate esmalt määrama häälestusviisardi parool ja seejärel saate määrata ja muuta selle StorSimple halduri teenuse. See parool autendib seadme StorSimple hetktõmmise halduriga.
 
    > [AZURE.IMPORTANT] Paroolid on enne registreerimist kogutud, kuid rakendatakse alles pärast seadet edukalt registreerida. Kui ilmneb tõrge rakendada parooli, palutakse parool uuesti esitama, kuni kogutakse nõutavad paroolid (keerukuse nõuetele vastavad).

4. Seadme registreerimine: Viimane toiming on StorSimple halduri teenuse Microsoft Azure töötab seadme registreerimiseks. Registreerimise peab teil saada [teenuse registreerimise võti](storsimple-manage-service.md#get-the-service-registration-key) Azure klassikaline portaalis ja sisestage selle häälestusviisardi. Pärast seadet on edukalt registreeritud, teenuse andmete krüptovõtme pakutakse teile. Kindlasti säilitada selles krüptovõtme turvaliste kohas, sest see on nõutav kõigi järgnevate seadmed registreerida teenuse.

## <a name="common-errors-during-device-deployment"></a>Levinud vigade seadme juurutamisel

Järgmises tabelis on loetletud levinud vigade, et võib ilmneda järgmistel juhtudel saate:

- Nõutav võrgu sätteid konfigureerida.
- Valikuline web puhverserveri sätete konfigureerimine.
- Häälestada seadme administraator ja StorSimple hetktõmmise halduri paroolid. 
- Seadme registreerimine. 

## <a name="errors-during-the-required-network-settings"></a>Tõrgete ajal nõutav võrgu sätteid

| Ei.| Tõrketeade | Võimalikud põhjused | Soovitatav toiming |
| ---| ------------- | --------------- | ------------------ |
| 1  | Autonoomsest HcsSetupWizard: See käsk saab käitada ainult aktiivse kontrolleril. | Passiivne kontrolleril oli läbi konfigureerimine.| Käivitage see käsk aktiivne kontrolleril. Lisateabe saamiseks vt [tuvastamine on aktiivne kontrolleril seadmes](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).|
| 2 | Autonoomsest HcsSetupWizard: Seade ei ole valmis. | Esineb probleeme võrguühenduse olemasolul andmete 0.| Märkige ruut füüsilise võrguühendus andmete 0.|
| 3 | Autonoomsest HcsSetupWizard: On IP-aadressi konflikti ja mõne muu võrgu (erandiks HRESULT: 0x80070263). | Andmete 0 esitatud IP oli juba kasutab mõni muu süsteem. | Sisestage uue IP, mis ei kasuta.|
| 4 | Autonoomsest HcsSetupWizard: Kobar ressursi nurjus. (Erandiks HRESULT: 0x800713AE). | Dubleeritud VIP. Esitatud IP on juba kasutusel.| Sisestage uue IP, mis ei kasuta.|
| 5 | Autonoomsest HcsSetupWizard: Sobimatu IPv4 aadress. | IP-aadress on esitatud vale vorminguga.| Märkige ruut vorming ja IP-aadressi uuesti esitama. Lisateavet leiate teemast [Ipv4 adressaatide valimiseks][1]. |
| 6 | Autonoomsest HcsSetupWizard: Sobimatu IPv6-aadress. | IP-aadress on esitatud vale vorminguga.| Märkige ruut vorming ja IP-aadressi uuesti esitama. Lisateavet leiate teemast [Ipv6 käsitlemine][2].|
| 7 | Autonoomsest HcsSetupWizard: Puuduvad pole veel lõpp-punkte lõpp-punkti plaanuri. (Erandiks HRESULT: 0x800706D9) | Kobar-funktsioonid ei tööta. | [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) järgmised toimingud.

## <a name="errors-during-the-optional-web-proxy-settings"></a>Tõrgete ajal valikuline web puhverserveri sätted

| Ei.| Tõrketeade | Võimalikud põhjused | Soovitatav toiming |
| ---| ------------- | --------------- | ------------------ |
| 1  | Autonoomsest HcsSetupWizard: Kehtetu parameeter (erandiks HRESULT: 0x80070057) | Ühe parameetriga ette nähtud puhverserveri sätted ei sobi.| URI ei esitata õiges vormingus. Kasutage järgmises vormingus: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Autonoomsest HcsSetupWizard: RPC server pole saadaval (erandiks HRESULT: 0x800706ba) | Põhjuseks on üks järgmistest.<ol><li>Klaster pole üles.</li><li>Passiivne kontrolleril ei saa suhelda active domeenikontrolleri ja käsk käivitatakse passiivne kontrolleril.</li></ol> | Sõltuvalt põhjus:<ol><li>Veenduge, et klaster on häälestamine [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) .</li><li>Käivitage käsk aktiivne kontrolleril. Kui soovite käivitage käsk passiivne kontrolleril, peate veenduge, et passiivne kontrolleril saaksid suhelda active kontrolleril. Peate [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) kui see funktsioon on katkenud.</li></ol> |
| 3 | Autonoomsest HcsSetupWizard: Nurjus kaugprotseduurikutse (erandiks HRESULT: 0x800706be) | Klaster on alla. | Veenduge, et klaster on häälestamine [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) .|
| 4 | Autonoomsest HcsSetupWizard: Klaster ei leitud ressursi (erandiks HRESULT: 0x8007138f) | Ressursi kobar ei leita. See võib juhtuda siis, kui installimine ei ole õige. | Kui peate seadme lähtestamiseks selle algsätted. [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) kobar ressursi loomiseks.|
| 5 | Autonoomsest HcsSetupWizard: Klaster ei online ressursi (erandiks HRESULT: 0x8007138c)| Kobar ressursid pole võrguühendust. | [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) järgmised toimingud.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Seadme administraator ja StorSimple hetktõmmise halduri paroolide seotud tõrgete

Vaikimisi seadme administraatori parool on **parool1**. See parool aegub pärast esimest sisselogimist; Seega peate häälestusviisardi abil saate seda muuta. Kui te registreerute esimest korda seadme peate sisestama uue seadme administraatori parooli. 

Kui kasutate haldamiseks seade töötab Windows Server hosti StorSimple hetktõmmise Manager tarkvara, peate ka andma StorSimple hetktõmmise halduri parooli esimest korda registreerimise käigus. 

Veenduge, et paroole vastama järgmistele nõuetele.

- Teie seadme administraatori parooli peaks olema 8 ja 15 tähemärki.
- StorSimple hetktõmmise halduri parooli peaks olema 14 või 15 tähemärki.
- Paroolide peaks sisaldama 3, 4 märgi järgmist tüüpi: väiketähed, suurtähed, arvulised ja teisiti. 
- Parooli ei tohi olla sama, mis viimase 24 paroolid.

Lisaks pidage meeles, et paroolide aegumise iga aasta ja saab muuta ainult pärast seadet edukalt registreerida. Kui registreerimine nurjus mingil põhjusel, paroolid ei saa muuta. Seadme administraator ja StorSimple hetktõmmise halduri paroolide kohta lisateabe saamiseks minge [kasutamine StorSimple halduri teenuse StorSimple oma parooli muuta](storsimple-change-passwords.md).

Võivad ilmneda üks või mitu järgmistest tõrketeadetest seadme administraator ja StorSimple hetktõmmise halduri paroolide häälestamisel.

| Ei.| Tõrketeade | Soovitatav toiming |
| ---| ------------- | ------------------ | 
| 1  | Parooli ületab maksimumpikkus. |Kasutage parooli, mis vastab:<ul><li>Teie seadme administraatori parooli peab olema 8 ja 15 tähemärki.</li><li>Parooli StorSimple hetktõmmise haldur peab olema 14 või 15 tähemärki.</li></ul> | 
| 2 | Parooli ei vasta nõutav pikkus. | Kasutage parooli, mis vastab:<ul><li>Teie seadme administraatori parooli peab olema 8 ja 15 tähemärki.</li><li>Parooli StorSimple hetktõmmise haldur peab olema 14 või 15 tähemärki.</lu></ul> |
| 3 | Parooli peab sisaldama väiketähed. | Paroolide peab sisaldama 3, 4 märgi järgmist tüüpi: väiketähed, suurtähed, arvulised ja teisiti. Veenduge, et parool vastab nendele nõuetele. |
| 4 | Parool peab sisaldama numbreid. | Paroolide peab sisaldama 3, 4 märgi järgmist tüüpi: väiketähed, suurtähed, arvulised ja teisiti. Veenduge, et parool vastab nendele nõuetele. |
| 5 | Parooli peab sisaldama erimärgid. | Paroolide peab sisaldama 3, 4 märgi järgmist tüüpi: väiketähed, suurtähed, arvulised ja teisiti. Veenduge, et parool vastab nendele nõuetele. |
| 6 | Parool peab sisaldama 3, 4 märgi järgmist tüüpi: suurtähtedeks, väiketähtedeks, arvulised ja teisiti. | Parooli ei sisalda nõutud tüüpi märke. Veenduge, et parool vastab nendele nõuetele. |
| 7 | Parameeter ei vasta kinnituse. | Veenduge, et parooli kõik vastab ja, et see oleks õigesti sisestatud. |
| 8 | Parooli ei vasta vaikimisi. | Vaikimisi parool on *parool1*. Peate selle parooli muuta, kui logite sisse esimest korda. |
| 9 | Sisestatud parool ei vasta seadme parool. Tippige parool uuesti. | Märkige parool ja sisestage see uuesti. |

Paroolide kogutakse enne, kui seade on registreeritud, kuid rakendatakse alles pärast eduka registreerimise. Parooli taastamine töövoog nõuab seadme registreerimiseks. 

> [AZURE.IMPORTANT] Üldiselt, kui rakendada parooli katse nurjus, tarkvara korduvalt proovib kogumise parooli, kuni see õnnestub. Harvaesinevate juhtudel, ei saa rakendada parooli. Sellisel juhul saate seadme registreerimine ja jätkata, kuid paroolide ei muudeta. Saate selle kohta, milliseid parool on muutunud viita – seadme administraatori parooli või StorSimple hetktõmmise halduri parool. Kui tekib olukord, soovitame teil muuta nii paroolid.

Paroolide Azure klassikaline portaalis StorSimple halduri teenuse kaudu saate lähtestada. Lisateabe saamiseks külastage: 

- [Seadme administraatori parooli muutmine](storsimple-change-passwords.md#change-the-device-administrator-password).
- [StorSimple hetktõmmise halduri parooli muutmine](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Seadme registreerimise käigus tõrked

Kasutate StorSimple halduri teenus Microsoft Azure töötab seadme registreerimiseks. Seadme registreerimise käigus võib ilmneda üks või mitu järgmistest probleemidest.

| Ei.| Tõrketeade | Võimalikud põhjused | Soovitatav toiming |
| ---| ------------- | --------------- | ------------------ |
| 1  | Tõrge 350027: Seadme StorSimple halduri registreerimine nurjus. |  | Oodake mõni minut ja proovige seejärel uuesti. Kui probleem ei lahene, [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).|
| 2  | Tõrge 350013: Tõrge ilmnes registreerimisel seade. See võib olla tingitud vale teenuse registreerimise võti. | | Registreeruge seade uuesti õige teenuse registreerimise võti. Lisateavet leiate teemast [saada teenuse registreerimise võti.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | Tõrge 350063: Autentimine StorSimple halduri teenusega möödunud, kuid registreerimine nurjus. Proovige mõne aja pärast toimingut. | See tõrge näitab, et ACS autentimine on möödunud, kuid ei ole tehtud teenuse register kõne. See võib olla juhuslik võrgu glitch tulemus. | Kui probleem ei lahene, palun [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md). |
| 4 | Tõrge 350049: Teenus ei saa ühendust registreerimise käigus. | Kui kõne teenusesse, ei pruugi web erand. Mõnel juhul see võib saada fikseeritud toimingu hiljem uuesti proovimist. | Kontrollige oma IP-aadress ja DNS-i nimi ja seejärel proovige uuesti. Kui probleem ei lahene, [pöörduge Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | Tõrge 350031: Seade on juba registreeritud. | | Toimingu vajalikud. |
| 6 | Tõrge 350016: Seadme registreerimine nurjus. | |Veenduge, et registreerimise võti on õige. |
| 7 | Autonoomsest HcsSetupWizard: Ilmnes tõrge;-oma seadme registreerimine See võib olla tingitud vale IP-aadress või DNS-i nimi. Kontrollige oma võrgu sätteid ja proovige uuesti. Kui probleem ei lahene, [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md). (Tõrge 350050) | Veenduge, et teie seade saab ping väljaspool võrgu. Kui teil pole Ühenduvus väljaspool võrku, ei pruugi selle tõrkega registreerimise. See tõrge võib olla kombinatsiooni ühte või mitut järgmistest.<ul><li>Vale IP</li><li>Vale alamvõrgu</li><li>Vale lüüs</li><li>Valed DNS-i sätted</li></ul> | Vaadake juhiseid teemas [samm-sammult tõrkeotsingu näide](#step-by-step-storsimple-troubleshooting-example). |
| 8 | Autonoomsest HcsSetupWizard: Praeguse toiming nurjus tõrke sisemise teenuse [0x1FBE2] tõttu. Proovige toimingut pärast uuesti. Kui probleem ei lahene, pöörduge Microsoft Support. | See on kõik kasutaja nähtamatuid vead visatud teenuse või agent Üldine tõrge. Enamlevinud põhjuseks võib olla, et ACS autentimine on nurjunud. Selle võimalik põhjus on selles, et esineb probleeme NTP serveri konfiguratsiooni ja seadme kellaaeg on õigesti seadistatud. | Õige aeg (kui on probleeme) ja seejärel proovige uuesti registreerimise. Kui kasutate käsu Set-HcsSystem - ajavöönd ajavööndi kohandada, Suurtähesta iga sõna ajavöönd (nt "Vaikse ookeani piirkonna Standard Time").  Kui see probleem ei lahene, [kontakti Microsofti tugiteenuste](storsimple-contact-microsoft-support.md) jaoks järgmised sammud. |
| 9 | Hoiatus: Ei õnnestunud aktiveerida seade. Teie seadme administraator ja StorSimple hetktõmmise halduri paroolid pole muudetud. | Kui registreerimine nurjub, seadme administraator ja StorSimple hetktõmmise halduri paroolid ei muudeta. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Tõrkeotsingu StorSimple juurutuste tööriistad

StorSimple sisaldab mitme tööriistu, mille abil saate oma StorSimple lahenduse tõrkeotsing. Järgmised:

- Tugiteenuste pakettide ja seadme logid 
- Cmdlet-käskude spetsiaalselt tõrkeotsing 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Tugi pakettide ja seadme logid mõeldud tõrkeotsing

Tugiteenuste pakett sisaldab asjakohaste logid, mis aitab Microsoft Support meeskond seadme probleemide tõrkeotsing. Windows PowerShelli StorSimple abil saate luua krüptitud tugiteenuste pakett, mida saate siis ühiskasutusse anda tehnilise toega.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Logide või tugiteenuste pakendi sisu kuvamiseks

1. Windows PowerShelli StorSimple abil saate luua tugi paketi, nagu on kirjeldatud [loomine ja haldamine tugi paketi](storsimple-create-manage-support-package.md).

2. Laadige kohalikult klientarvutis [dekrüptimine skripti](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) .

3. Kasutage seda [üksikasjalikult](storsimple-create-manage-support-package.md#edit-a-support-package) avamine ja dekrüptimine tugiteenuste pakett.

4. Paketi logid lahtikrüptitud tugi on etw/etvx vormingus. Saate teha neid faile vaadata Windowsi Sündmusevaatur tehke järgmist:
  1. Käivitage käsk **eventvwr** teie Windowsi klient. See käivitab vaataja.
  2. **Paanil,** klõpsake nuppu **Ava salvestatud Logi** ja osutage logifailide etvx/etw vormingus (tugiteenuste pakett). Nüüd saate vaadata faili. Kui fail on avatud, võite paremklõpsata ja salvestage fail tekstina.
   
    > [AZURE.IMPORTANT] Saate cmdleti **Get-WinEvent** nende failide avamiseks Windows PowerShelli. Lisateavet leiate teemast [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) Windows PowerShelli cmdleti viide dokumentatsiooni.

5. Kui logid avada Sündmusevaatur, otsige järgmised logid, mis sisaldavad seadme konfiguratsiooni seotud probleemid.

  - hcs_pfconfig/töökorras Log
  - hcs_pfconfig/Config

6. Logifailide otsida stringide seotud kutsunud häälestusviisardi cmdlet-käsud. Vt [esimest korda häälestamise viisardi protsessi](#first-time-setup-wizard-process) cmdlet loendi. 

7. Kui te ei saa probleemi põhjuse väljaselgitamiseks, saate teha järgmised toimingud [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) . Kasutage juhiseid [esitada tugiteenuse taotluse](storsimple-contact-microsoft-support.md#create-a-support-request) loomine, kui abi saamiseks Microsofti Support poole.

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdlet-käsud saadaval tõrkeotsing

Järgmine Windows PowerShelli cmdlet-käskude kasutamine ühenduvuse vigade tuvastamiseks.

- `Get-NetAdapter`: Kasutada cmdlet-käsu võrgu liidesed seisundi tuvastamiseks. 

- `Test-Connection`: Cmdlet-käsu abil kontrollida võrguühendus ettevõtte töötajatel ja ettevõttevälistel võrgu.

- `Test-HcsmConnection`: Cmdlet-käsu abil kontrollida registreeritud seadme Ühenduvus.

Kui kasutate oma seadmes StorSimple Update 1, on ka diagnostika järgmised cmdlet-käsud saadaval.

- `Sync-HcsTime`: Cmdlet-käsu abil kuvada seadme kellaaeg ja jõustada kellaaja sünkroonimine serveriga NTP.

- `Enable-HcsPing`ja `Disable-HcsPing`: nende cmdlettide abil saate lubada hosts ping võrgu liidesed StorSimple seadmes. Vaikimisi StorSimple võrgu liidesed ei reageeri ping taotlused.

- `Trace-HcsRoute`: Kasutada cmdlet-käsu marsruutimiseks jälgimine. See saadab paketid iga ruuteri viis soovitud sihtkohta aja jooksul ja seejärel arvutab iga sõnumihüppe kohta, pärast tagastatud paketid põhised tulemused. Kuna `Trace-HcsRoute` näitab täpselt, milliseid ruuterid või lingid võib põhjustada võrguprobleemide paketi kaotus mis tahes antud ruuteri või linki. 

- `Get-HcsRoutingTable`: Kohalik IP marsruutimise tabeli kuvamiseks kasutada cmdlet-käsu.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Get-NetAdapter cmdletiga tõrkeotsing

Kui konfigureerite võrgu liidesed esimest korda seadme juurutus, pole riistvara oleku saadaval StorSimple halduri teenuse Kasutajaliidese kuna seade pole veel teenuse registreeritud. Lisaks riistvara oleku leht võib pole õigesti alati kajastavad seade, eriti siis, kui on probleeme, mis mõjutavad sünkroonimise. Sel juhul saate kasutada funktsiooni `Get-NetAdapter` cmdlet-käsu seisundi ja teie võrgu liidesed oleku määratlemine.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Kõik võrguadapteri loendi kuvamiseks teie seadmes

1. Käivitage Windows PowerShelli StorSimple ja seejärel tippige `Get-NetAdapter`. 

2. Kasutage väljund on `Get-NetAdapter` cmdlet ja mõista teie võrgu liidese oleku järgmisi juhiseid.
  - Kui terve kasutajaliideses on lubatud, **ifIndex** olek on kujutatud **üles**.
  - Kui kasutajaliideses on terve, kuid pole ühendatud füüsilise (kui võrgukaabel), **ifIndex** on kujutatud **keelatud**.
  - Kui kasutajaliideses on terve, kuid pole lubatud, on kujutatud **NotPresent** **ifIndex** olek.
  - Kui kasutajaliideses pole olemas, ei ole selles loendis. StorSimple halduri teenuse Kasutajaliidese endiselt kuvatakse selle kasutajaliidese tõrkeolekus.

Selle cmdlet-käsu kasutamise kohta lisateabe saamiseks avage [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) Windows PowerShelli cmdleti viide. 

Järgmistes jaotistes Kuva näidised väljundi soovitud `Get-NetAdapter` cmdlet-käsk. 

 Nende proovide kontrolleril 0 on passiivne domeenikontrolleri ja konfigureeritud järgmiselt:

- ANDMETE 0, andmed 1, andmete 2 ja andmete 3 võrgu liidesed olemas seadmes.
- ANDMETE 4 ja 5 andmete võrgu kasutajaliidese kaardid ei esita; Seetõttu need on loetletud väljund.
- ANDMETE 0 on lubatud.

Selle domeenikontrolleri 1 on aktiivne domeenikontrolleri ja konfigureeritud järgmiselt:

- ANDMETE 0, andmed 1, andmete 2, 3 andmete, andmete 4 ja andmete 5 võrgu liidesed olemas seadmes.
- ANDMETE 0 on lubatud.

**Proovi väljundi – kontrolleril 0**

Järgmises väljund kontrolleril 0 (passiivne kontrolleril). ANDMETE 1, 2 andmete ja andmete 3 on ühendatud. ANDMETE 4 ja 5 andmete on loetletud, sest need ei esine seadmes. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Proovi väljundi – kontrolleril 1**

Järgmises väljund kontrolleril 1 (aktiivne kontrolleril). Ainult andmed 0 võrgu liidese seade on konfigureeritud ja töötamine.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Testi ühendust cmdletiga tõrkeotsing

Saate kasutada funktsiooni `Test-Connection` cmdlet-käsu, kas StorSimple seadme saate luua ühenduse väljaspool võrgu. Kui kõik võrgu parameetrid, sh DNS-i, on õigesti konfigureeritud häälestusviisardi, saate selle `Test-Connection` cmdlet-käsu ping teadaolevad aadress väljaspool võrku, nt outlook.com. 

Lubage ping cmdlet-käsu ühenduvuse probleemide tõrkeotsingut, kui ping on keelatud.

Vt järgmist näidet väljundi soovitud `Test-Connection` cmdlet-käsk. 

> [AZURE.NOTE] Esimese valimis, seade on konfigureeritud on vale DNS-i. Teine näide, DNS-i on õige.
 
**Proovi väljundi – valed DNS-i**

Järgmises näites on väljund IPV4 ja IPV6 aadressid, mis näitab, et DNS-i ei lahene. See tähendab, et on väljaspool võrgu pole Ühenduvus ja õige DNS-i tuleb esitada. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Kuulake väljundi – õige DNS-i**

Järgmises näites tagastab DNS-i IPV4 aadress, mis näitab, et DNS-i on õigesti konfigureeritud. See kinnitab, et on väljaspool võrgu Ühenduvus. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Test-HcsmConnection cmdletiga tõrkeotsing

Kasutage funktsiooni `Test-HcsmConnection` cmdlet-käsk seade, mis on juba ühendatud ja teie haldur StorSimple teenusega registreeritud. Selle cmdlet-käsu abil saate kontrollib ühenduvust registreeritud seadme ja vastava StorSimple halduri teenuse vahel. Käivitate selle käsu Windows PowerShelli StorSimple. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Kui soovite käivitage Test-HcsmConnection cmdlet-käsk

1. Veenduge, et seade on registreeritud.

2. Kontrollige seadme olek. Kui seade on välja lülitatud, hooldustööd režiimis või võrguühenduseta režiimis, võidakse kuvada järgmised tõrked. 

   - ErrorCode.CiSDeviceDecommissioned – see näitab, et seade on välja lülitatud.
   - ErrorCode.DeviceNotReady – see näitab, et seade on hooldus režiimis.
   - ErrorCode.DeviceNotReady – see näitab, et seade pole võrgus.

3. Veenduge, et StorSimple halduri teenuse töötab (cmdletiga [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) ). Kui teenus ei tööta, võidakse kuvada järgmised tõrked.

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError – see näitab, et oli erandi Get-ClusterResource käitamisel.

4. Märkige ruut Luba juurdepääs Control teenus (ACS). Kui see põhjustab web erand, võib olla lüüsi probleem, puuduvad puhverserveri autentimine, on vale DNS-i või tõrke autentimist. Võidakse kuvada järgmised tõrked.

   - ErrorCode.CiSApplianceGateway – seda näitab HttpStatusCode.BadGateway erand: lahendaja teenuse nime ei saanud lahendada hosti nimi. 
   - ErrorCode.CiSApplianceProxy – seda näitab HttpStatusCode.ProxyAuthenticationRequired erandi (HTTP olekukoodi 407): kliendi ei saa autentida puhverserveri abil. 
   - ErrorCode.CiSApplianceDNSError – seda näitab WebExceptionStatus.NameResolutionFailure erand: Resolveri teenuse nime ei saanud lahendada hosti nimi.
   - ErrorCode.CiSApplianceACSError – seda näitab, et teenuse tagastatud autentimise tõrge, kuid pole Ühenduvus.
   
    Kui see pole põhjustada web erand, kontrollige ErrorCode.CiSApplianceFailure. See näitab, et seade ei ole.

5. Märkige ruut pilvepõhise teenuse Ühenduvus. Kui teenus põhjustab web erand, võidakse kuvada järgmised tõrked.

  - ErrorCode.CiSApplianceGateway – seda näitab HttpStatusCode.BadGateway erand: on vahe puhverserveri teise puhverserveri või algse serverist saadud vigane päring.
  - ErrorCode.CiSApplianceProxy – seda näitab HttpStatusCode.ProxyAuthenticationRequired erandi (HTTP olekukoodi 407): kliendi ei saa autentida puhverserveri abil. 
  - ErrorCode.CiSApplianceDNSError – seda näitab WebExceptionStatus.NameResolutionFailure erand: Resolveri teenuse nime ei saanud lahendada hosti nimi.
  - ErrorCode.CiSApplianceACSError – seda näitab, et teenuse tagastatud autentimise tõrge, kuid pole Ühenduvus.
  
    Kui see pole põhjustada web erand, kontrollige ErrorCode.CiSApplianceSaasServiceError. See näitab StorSimple halduri teenuse probleem.
 
6. Kontrollimiseks Azure'i teenus siini. ErrorCode.CiSApplianceServiceBusError näitab, et seade ei saa ühendust luua teenuse siini.
 
Logifailide CiSCommandletLog0Curr.errlog ja CiSAgentsvc0Curr.errlog on rohkem teavet, näiteks erandi üksikasjad. 

Cmdlet-i kasutamise kohta lisateabe saamiseks minge Windows PowerShelli viide dokumentatsiooni [Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) .

> [AZURE.IMPORTANT] Te saate käivitada aktiivse nii passiivne kontrolleril. 
 
Vt järgmist näidet väljundi soovitud `Test-HcsmConnection` cmdlet-käsk. 

**Kuulake väljundi – registreeritud seade töötab StorSimple Release (juuli 2014)**

Esimene valim on seadmest, mis on edukalt registreeritud StorSimple halduri teenuse ja on pole probleeme. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Väljundi – registreeritud seade töötab StorSimple Update 1**

Kui kasutate oma seadmes StorSimple Update 1, kuvatakse peate käivitage see koos parameetriga Paljusõnaline.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Kuulake väljundi – ühenduseta seade töötab StorSimple Release (juuli 2014)**

See näide on seadmest, mis olek on **ühenduseta** Azure klassikaline portaalis.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Kasutatav seade ei saanud ühendust praeguse web puhverserveri konfigureerimise abil. See võib olla probleeme veebi Puhverserveri konfiguratsioon või võrgu Ühenduvus probleem. Sel juhul veenduge, et teie web puhverserveri sätted on õiged ja oma web puhverserverid on võrgus ja kättesaadav. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Sünkroonimine-HcsTime cmdletiga tõrkeotsing

Selle cmdlet-käsu abil saate kuvada seadme kellaaeg. Kui seadme kellaaeg on erinevust NTP serveriga, siis saate seda cmdlet sünkroonimiseks Jõusta-kellaaja NTP serveri. Kui offset seade ja NTP serveri vahel on suurem kui 5 minuti, kuvatakse hoiatus. Kui nihe suurem kui 15 minutit, seejärel seade läheb ühenduseta. Saate siiski kasutada cmdlet-käsu jõustamine kellaaja sünkroonimine. Juhul, kui nihe suurem kui 15 tundi, siis te ei saa sünkroonimiseks Jõusta-aja ja tõrketeade kuvatakse.

**Väljundi – jõustatud kellaaja sünkroonimine sünkroonimine-HcsTime abil**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Koos HcsPing-lubamine ja keelamine-HcsPing cmdlet-käskudega tõrkeotsing

Nende cmdlettide abil tagada, et teie seadmes võrgu liidesed vastata ICMP ping. Vaikimisi StorSimple võrgu liidesed ei reageeri ping taotlused. Selle cmdlet-käsu abil on hõlpsam leida, kui teie seade on võrgus ja kättesaadav.  

**Väljundi – HcsPing-lubamine ja keelamine-HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Jälita-HcsRoute cmdletiga tõrkeotsing

Kasutage cmdlet-käsu marsruutimiseks jälgimise tööriista. See saadab paketid iga ruuteri viis soovitud sihtkohta aja jooksul ja seejärel arvutab iga sõnumihüppe kohta, pärast tagastatud paketid põhised tulemused. Kuna cmdlet näitab paketi kaotus antud ruuteri või lingi, täpselt, milliseid ruuterid või lingid võib põhjustada võrguprobleeme.

**Näitab, kuidas jälgida koos Jälita-HcsRoute pakett tee väljundi**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Get-HcsRoutingTable cmdletiga tõrkeotsing

Selle cmdlet-käsu abil saate vaadata seadme StorSimple marsruutimise tabel. Marsruutimise tabel on reeglid, mis aitab kindlaks teha, kus liiklus andmete paketid Internet Protocol (IP) võrgu reisil kogumist. 

Marsruutimise tabelis liidesed ja lüüsi, mis marsruudib andmed määratud võrkudega. Samuti annab marsruutimise meetermõõdustik, mis on otsust maker konkreetsesse sihtkohta jõudmiseks tee. Väiksem marsruutimise meetermõõdustik, seda suurem eelistused. 

Näiteks, kui teil on 2 võrgu liidesed, andmete 2 ja 3 andmete Internetiga. Kui marsruutimise mõõdikute andmete 2 ja 3 andmed on 15 ja 261, on andmete 2 alumise marsruutimise meetermõõdustik eelistatud kasutajaliides, mis Internetis saavutamiseks kasutada.

Kui kasutate oma seadmes StorSimple Update 1, on teie andmete 0 võrgu liidese cloud liikluse kõrgeim eelistus. See tähendab, et ka siis, kui leidub muid cloud lubatud liideste, cloud liikluse oleks marsruutida läbi andmete 0. 

Kui käivitate selle `Get-HcsRoutingTable` cmdlet ilma parameetrid (nagu järgmises näites), cmdlet väljund nii IPv4 ja IPv6 marsruudi tabelid. Teise võimalusena saate määrata `Get-HcsRoutingTable -IPv4` või `Get-HcsRoutingTable -IPv6` saamiseks vastavat marsruutimise tabelit.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Samm-sammult StorSimple tõrkeotsingu näide

Järgmises näites on kujutatud StorSimple juurutuse üksikasjalike tõrkeotsing. Näide stsenaariumi seadme registreerimine nurjub sellist teadet võrgu sätteid või DNS-i nimi on vale.

Tagastatud tõrketeade on:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

See tõrge võib olla põhjustatud järgmistest:

- Vale riistvara installimine
- Vigase võrgu liidese/liideste
- Vale IP-aadress, alamvõrgu mask, lüüsi, esmane DNS-i server või veebiteenuse puhverserveri
- Vale registreerimise võti
- Vale tulemüüri sätted

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Otsige üles ja seadme registreerimine probleemi lahendamiseks

1. Kontrollige oma seadme konfiguratsiooni: aktiivne kontrolleril, käivitage `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] Häälestusviisardi peate käivitama aktiivne kontrolleril. Veenduge, et olete loonud ühenduse aktiivse kontrolleril, vaadake banner esitatud järjestikune konsooli. Banner näitab, kas olete loonud ühenduse kontrolleril 0 või 1 selle domeenikontrolleri ja kas selle domeenikontrolleri on aktiivne või passiivne. Lisateabe saamiseks minge [tuvastamine on aktiivne kontrolleril seadmes](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
 
2. Veenduge, et seade on õigesti komplekslõng: märkige võrgu kaabeldus tagasi tasapinnalised seadmes. Funktsiooni kaabeldus on seadme mudelist. Lisateabe saamiseks minge [installimine StorSimple 8100 seadme](storsimple-8100-hardware-installation.md) või [seadme StorSimple 8600 installida](storsimple-8600-hardware-installation.md).

     > [AZURE.NOTE] Kui kasutate 10 New York võrgu portide, peate kasutage pakutavaid QSFP-SFP adapterit ja SFP kaabel. Lisateavet leiate [kaabel, parameetrid, ja transiiverite soovitatud OEM tarnija Mellanox portide loendit](http://www.mellanox.com/page/cables?mtag=cable_overview).
 
3. Võrgu liidese seisundi kontrollimine

   - Get-NetAdapter cmdlet-käsk abil saate tuvastada võrgu liidesed seisundi andmete 0. 
   - Kui link ei tööta, **ifindex** olek näitab, et kasutajaliideses on alla. Seejärel peate pordi seadme ja parameetrit võrguühenduse kontrollimiseks. Samuti peate välistamiseks halb kaabel. 
   - Kui kahtlustate, et andmed 0 pordi active kontrolleril on nurjunud, saate kinnitada ühendatud andmete 0 pordi kontrolleril 1. Kontrollimaks, võrgukaabel katkestamine tagasi seadme kaudu kontrolleril 0, Ühendage kaabel kontrolleril 1 ja käivitage cmdlet-käsk Get-NetAdapter uuesti. 
   Kui andmete 0 port on selle domeenikontrolleri nurjub, [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) järgmised toimingud. Võib juhtuda kontrolleril teie süsteemi asendada.
 
4. Kontrollige ühendust lüliti:
   - Veenduge, et andmete 0 võrgu liidesed kontrolleril 0 ja domeenikontrolleri 1 sisse oma peamine ruum on sama alamvõrgu. 
   - Kontrollige ruuteri ja keskuses. Tavaliselt peaks sama jaoturi või ruuteri ühendada nii kontrollerid. 
   - Veenduge, et kasutate ühenduse parameetreid on nii kontrollerid sisse sama vLAN andmete 0.
   
5. Kõrvaldada kasutaja vigu:

  - Käivitage häälestamise viisard uuesti (Käivita **Invoke-HcsSetupWizard**) ja sisestage soovitud väärtused uuesti veendumaks, et pole vigu. 
  - Registreerimise kinnitamine võti kasutada. Sama registreerimise võti saab ühendada mitu seadet StorSimple halduri teenuse. Toiminguga sisse [saada teenuse registreerimise võti](storsimple-manage-service.md#get-the-service-registration-key) tagamaks, et kasutate õiget registreerimise võti.

    > [AZURE.IMPORTANT] Kui teil on mitu teenuseid, peate registreerimise võti asjakohane kasutamise seadme registreerimiseks. Kui olete registreerunud seadme StorSimple halduri teenuse valesti, peate [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) järgmised toimingud. Kui teil teha factory reset (mis võib põhjustada andmekao) seadme abil, siis ühenduse ettenähtud teenusega.

6. Cmdletiga testi ühendust veendumaks, et teil on väljaspool võrguga ühendus. Lisateabe saamiseks minge [tõrkeotsing cmdletiga testi ühendust](#troubleshoot-with-the-test-connection-cmdlet).

7. Kontrollige tulemüüri häireid. Kui teil on kinnitatud, mis virtuaalse IP (VIP), alamvõrgu, lüüsi ja DNS-i sätted on kõik õiged, ja näete endiselt probleeme, siis on võimalik, et teie tulemüür blokeerib suhtlemine seadme ja väljaspool võrgu. Soovite tagada pordid 80 ja 443 StorSimple seadme väljaminev suhtlemiseks. Lisateavet leiate teemast [Networking StorSimple seadme nõuded](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

8. Vaadake logid. Minge [toe pakettide ja seadme logid tõrkeotsingu jaoks saadaval](#support-packages-and-device-logs-available-for-troubleshooting).

9. Kui eelmist toimingut ei lahenda, abi saamiseks [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) .

## <a name="next-steps"></a>Järgmised sammud
[Saate teada, kuidas tõrkeotsing funktsionaalseid seadmes](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
