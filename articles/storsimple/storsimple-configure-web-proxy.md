<properties 
   pageTitle="Veebiteenuse puhverserveri StorSimple seadme häälestamine | Microsoft Azure'i"
   description="Siit saate teada, kuidas kasutada Windows PowerShelli StorSimple seadme StorSimple web puhverserveri sätete konfigureerimine."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Seadme StorSimple veebiteenuse puhverserveri konfigureerimine

## <a name="overview"></a>Ülevaade

Selles õppetükis kirjeldatakse, kuidas kasutada Windows PowerShelli StorSimple seadme StorSimple web puhverserveri sätete vaatamiseks ja konfigureerimiseks. Web puhverserveri sätete kasutatakse StorSimple seadme suhtlemisel pilve. Web puhverserverit saab lisada veel üks Turve, filtri sisu hõlpsamaks läbilaskevõime nõuete või isegi abi analytics vahemälu.

Veebiteenuse puhverserveri on valikuline konfiguratsioon StorSimple seadme. Saate konfigureerida veebiteenuse puhverserveri ainult Windows PowerShelli kaudu StorSimple. Konfiguratsiooni on kaheosaline toiming järgmiselt:

1. Konfigureerige esmalt web puhvrisätete häälestusviisardi või Windows PowerShelli kaudu StorSimple cmdlet-käsud.

2. Seejärel lubate konfigureeritud web puhverserveri sätete Windows PowerShelli kaudu StorSimple cmdlet-käsud.

Kui web puhverserveri konfigureerimine on lõpule jõudnud, saate vaadata konfigureeritud web puhverserveri sätete teenus Microsoft Azure'i StorSimple halduri nii Windows PowerShelli StorSimple. 

Pärast selle õpetuse lugemist, on võimalik:

- Häälestusviisardi ja cmdlet-käskude abil veebiteenuse puhverserveri konfigureerimine
- Cmdlet-käskude abil veebiteenuse puhverserveri lubamine
- Azure'i klassikaline portaali web puhverserveri sätete kuvamine
- Web puhverserveri konfigureerimise käigus tõrkeotsing


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Windows PowerShelli kaudu veebiteenuse puhverserveri konfigureerimine StorSimple

Kasutate web puhverserveri sätete konfigureerimiseks ükskõik kumba järgmistest:

- Häälestusviisard juhendab teid konfigureerimise juhised.

- StorSimple Windows PowerShelli cmdlet-käsud.

Järgmistes lõikudes käsitletakse iga järgmised võimalused.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Häälestusviisardi kaudu veebiteenuse puhverserveri konfigureerimine

Häälestusviisardi abil saate juhatavad teid web puhverserveri konfigureerimise juhised. Järgmiste toimingute veebiteenuse puhverserveri konfigureerimine oma seadmes.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Häälestusviisardi kaudu veebiteenuse puhverserveri konfigureerimine

1. Järjestikune konsooli menüü, valige suvand 1, **täielik juurdepääs sisse logida** ja sisestage **seadme administraatori parooli**. Tippige järgmine käsk häälestamise viisardi seansi käivitamine.

    `Invoke-HcsSetupWizard`

2. Kui see on esimene kord, et te olete kasutanud häälestusviisardi seadme registreerimine, peate kõik nõutud võrgu sätteid konfigureerida, kuni jõuate web puhverserveri konfigureerimine. Kui teie seade on juba registreeritud, saate aktsepteerida konfigureeritud võrgu sätteid seni, kuni jõuate web puhverserveri konfigureerimine. Häälestusviisardi, web puhverserveri sätete konfigureerimiseks küsimise korral tippige **Jah**.

3. Määratlege **Web puhverserveri URL-i**IP-aadress või täielik domeeninimi (FQDN) web puhverserverisse ja TCP pordi number, mida soovite kasutada pilveteenuses suhtlemisel seadme. Kasutage järgmises vormingus:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    Vaikimisi on määratud TCP pordi number 8080.

4. Valige autentimistüüp **NTLM**, **lihtne**või **mitte keegi**. Tavaline on kõige ebaturvalisem autentimist puhverserveri server Configuration. NT LAN Manageri (NTLM) on väga turvaline ja keerukate autentimise protokoll, mis kasutab kolm viis sõnumisüsteemist üle (mõnikord neli kui on vaja teha täiendavaid terviklus) kasutaja autentimiseks. Vaikimisi autentimine on NTLM. Lisateavet leiate teemast [põhi](http://hc.apache.org/httpclient-3.x/authentication.html) - ja [NTLM-autentimist](http://hc.apache.org/httpclient-3.x/authentication.html). 

    > [AZURE.IMPORTANT] **StorSimple halduri teenus, seadme jälgimisega seotud diagrammid ei tööta, kui lihtne või puhverserveri server Configuration seadme jaoks on lubatud NTLM autentimine. Töö jälgimisega seotud diagrammid, peate tagamaks, et autentimine sätteks pole.**

5. Kui kasutate autentimine, lisage **Web puhverserveri kasutajanime** ja **Parooliga Web puhverserveri**. Samuti peate kinnitage parool.

    ![Klõpsake StorSimple Device1 veebiteenuse puhverserveri konfigureerimine](./media/storsimple-configure-web-proxy/IC751830.png)

Kui seadme esimest korda, siis jätkake registreerimise. Kui teie seade on juba varem registreeritud, suletakse viisard. Saate salvestada konfigureeritud sätted.

Veebiteenuse puhverserveri nüüd ka lubatud. Saate [lubada veebiteenuse puhverserveri](#enable-web-proxy) sammu vahele jätta ja liikuge [vaate web puhverserveri sätted Azure klassikaline portaalis](#view-web-proxy-settings-in-the-azure-classic-portal).


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Veebiteenuse puhverserveri kaudu Windows PowerShelli cmdlet-käskude StorSimple konfigureerimine

Alternatiivne võimalus web puhverserveri sätete konfigureerimine on Windows PowerShelli cmdlet-käskude StorSimple kaudu. Järgmiste toimingute abil veebiteenuse puhverserveri konfigureerimine.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Cmdlet-käskude kaudu veebiteenuse puhverserveri konfigureerimine

1. Järjestikune konsooli menüüs valik, 1, **logige sisse täielik juurdepääs**. Küsimise korral sisestage **seadme administraatori parooli**. Vaikimisi parool on `Password1`.

2. Käsuviip, tippige:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Sisestage ja kinnitage parool kui küsitakse, nagu allpool näidatud.

    ![Klõpsake StorSimple Device3 veebiteenuse puhverserveri konfigureerimine](./media/storsimple-configure-web-proxy/IC751831.png)

Veebiteenuse puhverserveri on nüüd konfigureeritud ja peab olema lubatud.

## <a name="enable-web-proxy"></a>Veebiteenuse puhverserveri lubamine

Vaikimisi on keelatud veebiteenuse puhverserveri. Pärast web sätete konfigureerimiseks StorSimple seadmes, peate kasutama Windows PowerShelli StorSimple lubamiseks web puhverserveri sätted.

> [AZURE.NOTE] **See toiming ei pea kui häälestusviisardi abil veebiteenuse puhverserveri konfigureerimine. Pärast seansi häälestamise viisardi veebiteenuse puhverserveri vaikimisi automaatselt lubatud.**

Windows PowerShelli StorSimple veebiteenuse puhverserveri seadme lubamiseks tehke järgmist:

#### <a name="to-enable-web-proxy"></a>Kui soovite lubada veebiteenuse puhverserveri

1. Järjestikune konsooli menüüs valik, 1, **logige sisse täielik juurdepääs**. Küsimise korral sisestage **seadme administraatori parooli**. Vaikimisi parool on `Password1`.

2. Käsuviip, tippige:

    `Enable-HcsWebProxy`

    Nüüd on veebi Puhverserveri konfiguratsioon StorSimple seadmes sisse lülitatud.

    ![Klõpsake StorSimple Device4 veebiteenuse puhverserveri konfigureerimine](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Azure'i klassikaline portaali web puhverserveri sätete kuvamine

Web puhverserveri sätted on konfigureeritud Windows PowerShelli kasutajaliidese kaudu ja ei saa muuta klassikaline portaali. Klassikaline portaali, saate siiski konfigureeritud sätete kuvamine. Järgmiste toimingute veebiteenuse puhverserveri kuvamiseks.

#### <a name="to-view-web-proxy-settings"></a>Web puhverserveri sätete kuvamiseks
1. Liikuge **StorSimple halduri teenuse > seadmed**. Valige ja valige seade ja **seejärel avage**.
1. Kerige lehe jaotisse **Web puhverserveri sätete** **konfigureerimine** . Saate vaadata konfigureeritud web puhverserveri sätete StorSimple seadme, nagu allpool näidatud.

    ![Vaate veebiteenuse puhverserveri haldamine portaalis](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Tõrgete web puhverserveri konfigureerimise käigus

Kui web puhverserveri sätted on valesti konfigureeritud, kuvatakse tõrketeateid kasutaja Windows PowerShelli StorSimple. Järgmises tabelis selgitatakse mõningaid tõrketeateid, nende võimalikke põhjuseid ja Soovitatavad toimingud.

|Järjestikune ei.|HRESULT tõrkekood|Võimalik põhjus|Soovitatav toiming|
|:---|:---|:---|:---|
|1.|0x80070001|Käsk käivitatakse passiivne domeenikontrolleri ja seda ei saa suhelda active kontrolleril.|Käivitage käsk aktiivne kontrolleril. Käivitage käsk passiivne kontrolleril, peate ühenduvust passiivne: aktiivne kontrolleril parandamiseks. Peate tegelema Microsoft Support, kui see funktsioon on katkenud.|
|2.|0x800710dd - identifikaator toiming ei sobi|Puhverserveri sätted ei toetata StorSimple virtuaalse seadmes.|Puhverserveri sätted ei toetata StorSimple virtuaalse seadmes. Neid saab konfigureerida ainult StorSimple füüsilise seadmes.|
|3.|0x80070057 - sobimatu parameeter|Ühe parameetriga ette nähtud puhverserveri sätted ei sobi.|URI ei esitata õiges vormingus. Kasutage järgmises vormingus:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706ba - RPC server pole saadaval|Põhjuseks on üks järgmistest.</br></br>Kobar pole üles.</br></br>Andmeteed teenus ei tööta.</br></br>Käsk käivitatakse passiivne domeenikontrolleri ja seda ei saa suhelda active kontrolleril.|Palun tegeleda Microsoft Support klaster on üles ja Andmeteed teenus töötab.</br></br>Käivitage käsk aktiivne kontrolleril. Kui soovite käivitage käsk passiivne kontrolleril, peate passiivne kontrolleril ei saa suhelda active kontrolleril tagamiseks. Peate tegelema Microsoft Support, kui see funktsioon on katkenud.|
|5.|0x800706be - kaugprotseduurikutse nurjus.|Klaster on alla.|Palun tegeleda Microsoft Support tagamaks, et klaster on üles.|
|6.|0x8007138f - kobar ressurssi ei leitud|Platvormi teenuse kobar ressursi ei leita. See võib juhtuda siis, kui selle installimine ei proper.|Võimalik, et peate mõne factory lähtestada oma seadmes teha. Võib-olla peate looma platvormi ressursi. Järgmised sammud saamiseks pöörduge Microsofti Support.|
|7.|0x8007138c - kobar ressursi pole võrgus|Platvormi või Andmeteed kobar ressursid pole võrguühendust.|Pöörduge Microsoft Support tagada, et Andmeteed ja platvorm service ressursi on võrgus.|

> [AZURE.NOTE] 
> 
> -  Tõrketeated ülaltoodud loend pole täielik. 
> - Web puhvrisätete seotud tõrgete ei kuvata teie haldur StorSimple teenuses Azure klassikaline portaalis. Kui ilmneb probleeme veebiteenuse puhverserveri pärast konfiguratsiooni on lõpule viidud, seadme olek muutub **ühenduseta** klassikaline portaalis. |

## <a name="next-steps"></a>Järgmised sammud

- Kui teil tekib probleeme juurutamine oma seadmesse või veebi puhverserveri sätete konfigureerimine, vaadake [tõrkeotsing juurutamise StorSimple seadme](storsimple-troubleshoot-deployment.md).

- Avage StorSimple halduri teenuse kasutamise kohta leiate [StorSimple halduri teenuse haldamiseks StorSimple seadme](storsimple-manager-service-administration.md)kasutamiseks.
