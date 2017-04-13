<properties 
   pageTitle="PowerShelli StorSimple seadmehalduse | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada Windows PowerShelli StorSimple StorSimple seadme haldamine."
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
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Teie seadme haldamine Windows PowerShelli StorSimple abil

## <a name="overview"></a>Ülevaade

Windows PowerShelli StorSimple pakub käsurea liides, mille abil saate hallata oma Microsoft Azure'i StorSimple seadme. Juba nime, on Windows PowerShelli põhinev käsurea liides, mis on sisse ehitatud piiratud runspace. Käsurida kasutaja seisukohast piiratud runspace kuvatakse piiratud versiooni Windows PowerShelli. Säilitades osa Windows PowerShelli põhilisi funktsioone, on see kasutajaliidese täiendavad sihtotstarbeline cmdlet-käsud, mis on suunatud Microsoft Azure'i StorSimple seadme haldamine. 

Selles artiklis kirjeldatakse Windows PowerShelli StorSimple funktsioone, sh kuidas saate luua ühenduse selle kasutajaliidese ja sisaldab linke üksikasjalikud juhised või töövood, mida saate teha seda liidest kasutades. Töövood sisaldavad kuidas seadme registreerimine, konfigureerida võrgu liidese teie seadmes, installige värskendused, mis nõuavad seadme hooldus režiimis, seadme muuta, ja mis tahes ilmneda võivate probleemide tõrkeotsing.

Pärast selle artikli lugemist on võimalik:

- Ühenduse loomine Windows PowerShelli kaudu StorSimple StorSimple seadme.

- Windows PowerShelli kaudu StorSimple StorSimple seadme haldamine.

- Abi Windows PowerShelli StorSimple.

>[AZURE.NOTE]   

>- Windows PowerShelli cmdlet-käskude StorSimple võimaldavad teil hallata StorSimple seadme kaudu Windows PowerShelli remoting järjestikune konsooli või kaugühenduse teel. Iga üksiku cmdlet-käskude saab kasutada selle liidese kohta lisateabe saamiseks minge [cmdleti viide StorSimple Windows PowerShelli jaoks](https://technet.microsoft.com/library/dn688168.aspx).

>- StorSimple Azure PowerShelli cmdlet-käsud on erinevate kogumi cmdlet-käsud, mis võimaldab teil automatiseerimiseks StorSimple teenusetaseme ja migreerimistoimingud käsurea kaudu. Lisateavet Azure PowerShelli cmdletid StorSimple minge [Azure'i StorSimple cmdleti viide](https://msdn.microsoft.com/library/azure/dn920427.aspx).

Pääsete juurde Windows PowerShelli StorSimple, kasutades ühte järgmistest.

- [Ühenduse loomine StorSimple seadme järjestikune konsooli](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Kaugühenduse teel ühenduse loomine Windows PowerShelli kaudu StorSimple](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Ühenduse loomine Windows PowerShelli StorSimple seadme järjestikune konsooli kaudu

Saate [alla laadida PuTTY](http://www.putty.org/) või sarnane terminalis imiteerimist tarkvara abil ühenduse loomine Windows PowerShelli StorSimple. Peate PuTTY spetsiaalselt juurdepääsu Microsoft Azure StorSimple seadme konfigureerimine. Järgmistes teemades sisaldavad üksikasjalikud juhised selle kohta, kuidas konfigureerida PuTTy ja ühendage seade. Samuti selgitatakse erinevate menüü Suvandid järjestikune konsooli.

### <a name="putty-settings"></a>Kitt sätted

Veenduge, et kasutada järgmisi sätteid kitt ühenduse loomine Windows PowerShelli kasutajaliidese järjestikune konsooli.

#### <a name="to-configure-putty"></a>PuTTY konfigureerimine

1. Valige dialoogiboksis kitt **ümberkonfigureerimine** **kategooria** paanil **klaviatuuri**.

2. Veenduge, et valitud järgmistest suvanditest (need on vaikesätete uue seansi käivitamisel). 

  	|Klaviatuuri üksus|Valige|
  	|---|---|
  	|Tagasilükkeklahvi|Juhtelemendi-? (127)|
  	|Kodu- ja lõppaeg võtmed|Standard|
  	|Funktsiooniklahvid ja klahvistik|Paoklahv (ESC) [n ~|
  	|Algne olek noolenuppude abil|Tavaline|
  	|Algne numbriklahvistiku olek|Tavaline|
  	|Klaviatuuri funktsioonide lubamine|Juhtelemendi-Alt erineb AltGr|

    ![Toetatud kitt sätted](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Klõpsake nuppu **Rakenda**.

4. **Kategooria** paanil valige väärtus **tõlge**.

5. Valige loendiboksis **Remote märgistikus** **UTF-8**.

6. Valige jaotises **töötlemise joonejoonistus märke**, **Kasuta Unicode'i joonejoonistus koodi punktides**. Järgmisel joonisel on õige kitt Valikud.

    ![UTF kitt sätted](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Klõpsake nuppu **Rakenda**.


Nüüd saate PuTTY ühenduse seadme järjestikune konsooli, tehes järgmist.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>Järjestikune konsooli kohta

Kui kasutate Windows PowerShelli, mis on esitatud kasutajaliidese StorSimple seadme kaudu järjestikune konsooli, banner sõnumi, millele järgneb menüü Suvandid. 

Plakat sõnum sisaldab StorSimple seadme põhiteabe nt mudeli, nime, versiooni installitud tarkvara ja selle domeenikontrolleri avate olekut. Järgmisel pildil on kujutatud banner sõnumi.

![Järjestikune banner sõnum](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] Plakat sõnumi abil saate tuvastada, kas olete loonud ühenduse on aktiivne või passiivne.

Järgmisel pildil on kujutatud erinevad runspace võimalusi, mis on saadaval menüüs järjestikune konsooli.

![2 seadme registreerimine](./media/storsimple-windows-powershell-administration/IC740906.png)

Saate valida järgmised sätted:

1. **Täielik juurdepääs sisse logida** See suvand võimaldab teil suhelda (õige mandaat) **SSAdminConsole** runspace kohaliku kontrolleril. (Kohaliku domeenikontrolleri on kontrolleril, mille avate praegu järjestikune konsooli StorSimple seadme kaudu). Selle suvandi saate kasutada ka Microsoft Support piiramatu runspace (seansi tugi) mis tahes seadme võimalike probleemide tõrkeotsingu juurdepääsu lubamiseks. Pärast 1 suvandi abil saate sisse logida, saate lubada juurdepääsu piiramatu runspace teatud cmdlet-käsu käivitades Microsoft Support insener. Lisateabe saamiseks vaadake [tugi seansi käivitamine](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).

2. **Logige sisse omavahelistes domeenikontrolleri täielik juurdepääs abil** See suvand on sama, mis valik 1, välja arvatud, et saate luua ühenduse (koos õige mandaat) **SSAdminConsole** runspace omavahelistes kontrolleril. Kuna tegemist StorSimple on kõrge-saadavus seadmega kaks kontrollerid on aktiivne passiivne konfiguratsiooni, omavahelistes viitab muude domeenikontrolleri seadme, mille avate järjestikune konsooli kaudu).
Sarnaselt valik 1, selle suvandi saate kasutada ka Microsoft Support piiramatu runspace omavahelistes kontrolleril juurdepääsu lubamiseks.

3. **Piiratud juurdepääsuga ühenduse loomine** See suvand kasutatakse juurdepääsuks Windows PowerShelli kasutajaliidese piiratud režiimis. Küsitakse pole Accessi mandaat. See suvand loob ühenduse rohkem piiratud runspace võrreldes suvandid 1 ja 2.  Mõned tööülesanded kaudu suvand 1 on saadaval mis **ei saa* teha seda runspace on:

    - Factory sätete lähtestamine
    - Parooli muutmine
    - Lubada või keelata tugi
    - Värskenduste rakendamine
    - Käigultparanduste installimine 
                                                

    >[AZURE.NOTE] **See on eelistatud valik, kui seadme administraatori parooli unustanud ja variant 1 või 2 kaudu ei saa ühendust luua.**

4. **Keele muutmine** See suvand, mis võimaldab teil Windows PowerShelli liides kuvamiskeele muutmine. Toetatud keeled on inglise, Jaapani, vene, prantsuse, Lõuna Korea, Hispaania, Itaalia, saksa, Hiina ja Brasiilia portugali.


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Kaugühenduse teel ühenduse loomine Windows PowerShelli kaudu StorSimple StorSimple

Windows PowerShelli remoting abil saate oma StorSimple seadmega ühendada. Kui loote ühenduse nii, te ei näe menüü. (Kuvatakse menüüs ainult juhul, kui kasutate järjestikune konsooli seadme ühenduse. Eemalt ühenduse viib teid otse võrdub "option 1 – täielik juurdepääs" järjestikune konsooli.) Windows PowerShelli remoting, saate luua ühenduse teatud runspace. Saate määrata ka kuvamiskeelt. 

Kuvamiskeele ei sõltu seate järjestikune konsooli menüü **Keele muutmine** suvandi abil soovitud keel. Kaug-PowerShelli automaatselt üles lokaadi seadme loote kaudu, kui määratud pole valida.

>[AZURE.NOTE] Kui töötate Microsoft Azure virtuaalse majutajate ja StorSimple virtuaalne seadmed, saate Windows PowerShelli remoting ja virtuaalne host virtuaalse seadmega ühendada. Kui olete loonud ühiskasutus asukoht, kuhu salvestada teavet Windows PowerShelli seansi hosti, arvestage, et kõik põhisumma sisaldab ainult autenditud kasutajad. Seetõttu, kui olete häälestanud ühiskasutus Luba juurdepääs kõigile ilma identimisteabe ühendust luua, kasutatakse autentimata anonüümse põhisumma ja kuvatakse tõrge. Selle probleemi lahendamiseks, võrgukettal majutada tuleb lubada konto ja seejärel anda külaline konto täielik juurdepääs ühiskasutusse andmise või määrake sobiv mandaate koos Windows PowerShelli cmdlet-käsk.

Saate ühendada kaudu Windows PowerShelli remoting HTTP- või HTTPS. Kasutage juhiseid järgmised õpetused:

- [Ühenduse loomine kaugühenduse teel HTTP kaudu](storsimple-remote-connect.md#connect-through-http)
- [Ühenduse loomiseks kasutada eemalt HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Ühenduse turvakaalutlused

Kuidas luua ühendus Windows PowerShelli StorSimple otsustamisel arvestage järgmisega.

- Ühenduse seadme järjestikune konsooli on turvaline, kuid ühenduse järjestikune konsooli üle võrgu parameetrid pole. Olge ettevaatlik, ohtu turvalisusele järjestikune seadme ühendamisel üle võrgu parameetrid.

- Ühenduse loomine mõne seansi HTTP kaudu pakkuda rohkem kui võrgu kaudu järjestikune konsooli ühenduse turvalisus. Kuigi see pole kõige ebaturvalisem viis, on aktsepteeritav usaldusväärsete võrkudes.

- Ühenduse loomine mõne HTTPS seansi kaudu on kõige turvalisem ja soovitatav suvand.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Windows PowerShelli kaudu StorSimple StorSimple seadme haldamine
Järgmises tabelis kuvatakse kõik tavalisi ja keerukate töövood, mida saab teha kokkuvõte Windows PowerShelli kasutajaliideses StorSimple seadme. Iga töövoo kohta lisateabe saamiseks klõpsake tabeli vastav kirje.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShelli jaoks StorSimple töövood

|Kui soovite selle tegemiseks...|Selle toiminguga.|
|---|---|
|Seadme registreerimine|[Konfigureerige ja Windows PowerShelli kaudu StorSimple seadme registreerimine](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Veebiteenuse puhverserveri konfigureerimine</br>Vaate web puhverserveri sätted|[Seadme StorSimple veebiteenuse puhverserveri konfigureerimine](storsimple-configure-web-proxy.md)|
|ANDMETE 0 võrgu kasutajaliidese seadme sätete muutmine|[ANDMETE 0 võrgu liidese StorSimple seadme muutmine](storsimple-modify-data-0.md)|
|Mõne selle domeenikontrolleri peatamine </br> Taaskäivitage või mõne selle domeenikontrolleri sulgeda </br> Sulgege seadme</br>Lähtestage seade algsätted|[Seadme kontrollerid haldamine](storsimple-manage-device-controller.md)|
|Hoolduse režiimi värskenduste installimine ja käigultparandused|[Värskendage oma seadmesse](storsimple-update-device.md)|
|Hoolduse režiimis </br>Väljumine hooldus režiimis|[StorSimple seadme režiimi](storsimple-device-modes.md)|
|Tugiteenuste paketi loomine</br>Dekrüptimine ja tugiteenuste pakett redigeerimine|[Saate luua ja hallata paketi tugi](storsimple-create-manage-support-package.md)|
|Tugiteenuste seansi käivitamine</br>|[Tugiteenuste seansi käivitamine Windows PowerShelli StorSimple](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Abi Windows PowerShelli StorSimple

StorSimple Windows PowerShelli cmdlet-käsu spikker on saadaval. Võrgus, ajakohane versiooni see spikker on saadaval ka, mille abil saate värskendada teie süsteemi abil.

Abi otsimine selle liidese on sarnane Windows PowerShelli ja töötab enamik abi seotud cmdlet-käsud. Leiate TechNeti teegi spikker Windows PowerShelli jaoks võrgus: [skriptimine Windows PowerShelli abil](http://go.microsoft.com/fwlink/?LinkID=108518).

Järgmises tüüpi spikker selle Windows PowerShelli kasutajaliides, sh kuidas värskendada aidake lühikirjeldus.

#### <a name="to-get-help-for-a-cmdlet"></a>Cmdlet-käsu jaoks abi saamiseks

- Cmdlet-käsk või funktsioon abi saamiseks kasutage järgmine käsk:`Get-Help <cmdlet-name>`

- Mis tahes cmdlet võrguspikker saamiseks kasutage eelmise cmdlet koos selle `-Online` parameeter:`Get-Help <cmdlet-name> -Online`

- Täielik abi, saate kasutada funktsiooni `–Full` parameeter, ja näiteid, kasutage funktsiooni `–Examples` parameeter.

#### <a name="to-update-help"></a>Värskendada spikker

Liidese Windows PowerShelli abil saate hõlpsasti värskendada. Järgmiste toimingute abi teie süsteemi värskendama.

#### <a name="to-update-cmdlet-help"></a>Värskendada cmdlet-käsu spikker

1. Alustage Windows PowerShelli suvandi **Käivita administraatorina** .

1. Käsuviip, tippige: `Update-Help`

1. Värskendatud abi failid on installitud.

1. Pärast installimist abi failid, tippige: `Get-Help Get-Command`. See kuvab loendi cmdlettide spikker on saadaval.


>[AZURE.NOTE] Mõne runspace saadaolevad cmdletid loendi saamiseks vastav menüükäsk sisse logida ja käivitage selle `Get-Command` cmdlet-käsk.

## <a name="next-steps"></a>Järgmised sammud
Kui tekib probleeme StorSimple seadmega läbimiseks ülaltoodud töövoogude, vaadake [tõrkeotsingu StorSimple juurutuste tööriistad](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

