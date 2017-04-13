<properties
   pageTitle="Loogika rakendused installida kohapealse andmete lüüsi | Microsoft Azure'i"
   description="Teave loogika rakenduse kasutamiseks kohapealse andmete lüüsi installimise kohta."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Kohapealse andmete lüüsi loogika rakenduste installimine

## <a name="installation-and-configuration"></a>Installimine ja konfigureerimine

### <a name="prerequisites"></a>Eeltingimused

Minimaalne:

* .NET 4.5 framework
* 64-bitine Windows 7 või Windows Server 2008 R2 (või uuem versioon)

Soovitatav:

* 8 core CPU
* 8 GB mälu
* 64-bitine versioon Windows 2012 R2 (või uuem versioon)

Seotud asjaoluga:

* Te ei saa installida lüüsi domeenikontrolleri.
* Lüüsi ei tohiks arvutisse, sülearvuti, mis võib olla välja lülitatud, une, installida või mitte Internetiga ühendatud, kuna lüüsi ei saa käivitada mis tahes olukorras alusel. Lisaks võidakse lüüsi jõudluse on üle traadita võrgu.

### <a name="install-a-gateway"></a>Lüüsi installimine

Saate [Installeri kohapealse andmete lüüsi siin](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

Määrata **kohapealse andmete lüüsi** režiimi, logige sisse oma töö või kooli kontot, ja seejärel uue lüüsi konfigureerimine või migreerimine, taastamine või võtta mõne olemasoleva lüüsi üle.

* Lüüsi konfigureerida **nimi** tippige see ja **taastamise võti**ja klõpsake või koputage nuppu **Konfigureeri**.

    Määrake taastamine klahvi, mis sisaldab vähemalt kaheksat tähemärki ja hoidke seda kindlas kohas. Kui soovite migreerida, taastamine või selle lüüsi üle võtta, peate selle klahvi.

* Migreerimine, taastamine või mõne olemasoleva lüüsi üle võtta, sisestage lüüsi loomisel määratud taastamise võti.

### <a name="restart-the-gateway"></a>Taaskäivitage lüüsi

Lüüsi töötab teenus Windows ja sarnaselt muude Windows teenuse käivitamine ja peatamine see on mitu võimalust. Näiteks saate avage käsuviip laiendatud õigustega arvutisse, kus töötab lüüsi ja seejärel käivitage kas need käsud:

* Teenuse peatamine käivitage see käsk:

    `net stop PBIEgwService`

* Teenuse käivitamine, käivitage see käsk:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Tulemüüri või puhverserveri konfigureerimine

Lisateavet selle kohta, kuidas anda oma lüüsi puhverserveri teavet teemast [puhverserveri sätete konfigureerimine](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

Saate kontrollida, kas teie tulemüür või puhverserver, võib blokeerida ühendust PowerShelli käsuviibas järgmine käsk töötab. Azure'i teenus siini connectivity test. Selles ainult kontrollib võrguühendus ja pole midagi teha pilveteenuses serveri või lüüsi. See aitab määratleda, kas teie arvuti tegelikult jõuaksid Interneti-ühendus.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Tulemused peaks sarnanema selles näites. Kui **TcpTestSucceeded** pole täidetud, mida võib olla blokeeritud tulemüüriga.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Kui soovite on täielikud, asendada need loendis [konfigureerimine pordid](#configure-ports) käesoleva teema **arvutinimi** ja **pordi** väärtused.

Tulemüüri võib ka ühendused, mis teeb Azure'i teenus siini Azure andmekeskuste blokeerida. Kui see on nii, mida soovite nimekiri (vabastamine) kõik teie piirkonna jaoks need IP-aadressid. Saate loendit [siin Azure'i IP-aadressid](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Portide konfigureerimine

Lüüsi loob väljamineva ühenduse Azure'i teenus siini. See edastab Väljaminev pordid kohta: TCP 443 (vaikeväärtus) 5671, 5672, 9350 9354 kaudu. Lüüsi ei nõua sissetuleva meili pordid.

Lisateavet [hübriid lahendusi](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMEENINIMED | VÄLJAMINEV PORDID | KIRJELDUS |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | HTTPS |
| *. login.windows.net | 443 | HTTPS |
| *. servicebus.windows.net |5671-5672 | Täiustatud sõnumi, järjekorda paneku protokolli (AMQP) |
| *. servicebus.windows.net | 443, 9350-9354 | Klõpsake teenuse siini Relay (nõuab 443 juurdepääsu reguleerimine Turbeloa omandamise) TCP kuulajatele |
| *. frontend.clouddatahub.net | 443 | HTTPS |
| *. core.windows.net | 443 | HTTPS |
| login.microsoftonline.com | 443 | HTTPS |
| *. msftncsi.com | 443 | Kasutada kontrollimiseks Interneti-ühenduse lüüs on Power BI teenus kättesaamatu. |

Kui soovite valge loendi domeenide asemel IP-aadressid, saate alla laadida ja kasutada [Microsoft Azure'i andmekeskuse IP-vahemike loendi](https://www.microsoft.com/download/details.aspx?id=41653). Mõnel juhul tehakse Azure'i teenuse turvalist ühendust IP-aadress täielik domeeninimi nimede asemel.

### <a name="sign-in-account"></a>Kontoga sisselogimine

Kasutajad saavad kas sisselogimine töö- või koolikontoga. See on teie ettevõtte konto. Kui soovite pakkumine Office 365 kasutajaks registreerunud ei andnud oma tegelik töö e-posti, see võib tunduda jeff@contoso.onmicrosoft.com. Teie konto sees pilveteenus talletatakse rentniku rakenduses Azure Active Directory (AAD). Enamikul juhtudel vastavad AAD konto UPN-i e-posti aadress.

### <a name="windows-service-account"></a>Windowsi teenusekonto

Kohapealse andmete lüüsi on konfigureeritud kasutama NT SERVICE\PBIEgwService Windowsi teenuse sisselogimise identimisteabega. Vaikimisi on õigus Logi teenust. See on arvuti, kuhu installite lüüsi kontekstis.

See pole kasutatud ühenduse loomiseks kohapealse andmeallikate või töökoha või kooli kontoga, millega saate sisse logida cloud services konto.

##<a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

### <a name="general"></a>Üldine

**Küsimus**: milliseid andmeallikaid lüüsi toetab?<br/>
**Answer (vastus)**: selle kirjutada, SQL Server.

**Küsimus**: vaja lüüsi pilves, nagu SQL Azure'i andmeallikate jaoks? <br/>
**Answer (vastus)**: ei. Lüüsi loob ühenduse ainult kohapealse andmeallikad.

**Küsimus**: tegelik Windowsi teenuse nn?<br/>
**Answer (vastus)**: teenuste lüüsi nimetatakse teenus Power BI Enterprise lüüsi.

**Küsimus**: on mis tahes sissetulevaid ühendusi lüüsiga pilvest? <br/>
**Answer (vastus)**: ei. Lüüsi kasutab Väljaminevad ühendused Azure'i teenus siini.

**Küsimus**: mida teha, kui blokeerida Väljaminevad ühendused? Mida on vaja avada? <br/>
**Answer (vastus)**: vaadake teemat pordid ja hosts kasutava lüüsi.


**Küsimus**: kas lüüsi peab olema installitud samasse arvutisse andmeallikana? <br/>
**Answer (vastus)**: ei. Andmeallika ühenduse teavet, mis oli lisatud loob ühenduse lüüs. Mõelge lüüsi klientrakendusega selles mõttes. Ainult peate saama ühenduse serverinime, mis oli lisatud.


**Küsimus**: mis on latentsus töötab päringute andmeallika lüüsi kaudu? Mis on parim arhitektuur? <br/>
**Answer (vastus)**: Võrgu latentsuse vähendamiseks installida lüüsi andmeallikale võimalikult lähedal. Kui installite lüüsi tegelik andmeallikas, siis minimeerida kasutusele latentsus. Kaaluge võimalust kasutada ka andmekeskuste. Näiteks kui teie teenuse on kasutusel USA Lääne andmekeskuse ja teil on majutatud on Azure VM SQL serveri, peaksite on samuti Azure VM Lääne USA. See minimeerida latentsus ja Azure VM sealt kulude vältimiseks.


**Küsimus**: on mis tahes nõuded võrguressurssi? <br/>
**Answer (vastus)**: soovitatav on hea jõudlus võrguühenduse. Iga keskkond on erinevad ja andmehulga saadetakse mõjutab tulemusi. Kasutades ExpressRoute võib aidata tagada läbilaskevõime kohapealse ja Azure andmekeskuste vahel.

Kolmanda osapoole tööriista Azure kiiruse Test rakenduse abil saate aitavad hinnata, mis on teie läbilaskevõime.


**Küsimus**: ettekandmiseks lüüsi teenuse Windows Azure Active Directory kontot? <br/>
**Answer (vastus)**: ei. Teenuse Windows peab olema lubatud Windowsi konto. Vaikimisi see töötab teenus sid, NT SERVICE\PBIEgwService.


**Küsimus**: kuidas tulemused saadetakse tagasi pilv? <br/>
**Answer (vastus)**: seda tehakse Azure'i teenus siini teel. Lisateabe saamiseks lugege teemat Kuidas see toimib.


**Küsimus**: kus minu identimisteave talletatakse? <br/>
**Answer (vastus)**: sisestatud andmeallika identimisteabe talletatakse krüptitud lüüsi pilveteenuses. Mandaat on dekrüptitakse lüüsi asutusesisene.

### <a name="high-availabilitydisaster-recovery"></a>Kõrge-saadavus/katastroofiabi

**Küsimus**: Kas on kavas lüüsi kõrge-saadavus stsenaariumid, mis võimaldab? <br/>
**Answer (vastus)**: see on peal, kuid me pole veel ajaskaala.


**Küsimus**: millised suvandid on saadaval tõrkejärgseks? <br/>
**Answer (vastus)**: taastamise võti abil saate taastada või teisaldada lüüsi. Kui installite lüüsi, määrake taastamise võti.


**Küsimus**: mis on kasu taastamise võti? <br/>
**Answer (vastus)**: See võimaldab migreerida või lüüsi sätete taastamine pärast katastroofi.

### <a name="troubleshooting"></a>Tõrkeotsing

**Küsimus**: kus asuvad lüüsi logid? <br/>
**Answer (vastus)**: vt käesoleva teema tööriistad.


**Küsimus**: Kuidas vaadata päringud on saadetud kohapealse andmeallika? <br/>
**Answer (vastus)**: saate lubada query jälitamise, mis sisaldab päringud, mis saadetakse. Ärge unustage muuta tagasi esialgse väärtuse, kui lõpetanud tõrkeotsing. Jätke päring jälgimine lubatud põhjustab suurem logid.

Saate vaadata ka tööriistu, mis teie andmeallika on jälgimine päringuid. Näiteks saate pikendada sündmusi või SQL Profiler ja SQL Server Analysis Services.

## <a name="how-the-gateway-works"></a>Lüüsi tööpõhimõtted

Kui kasutaja suhtleb element, mis on ühendatud kohapealse andmeallika:

1. Pilveteenusesse loob koos krüptitud identimisteabe andmeallika, päringu ja saadab päringu töötlemiseks lüüsi järjekorda.
1. Teenuse analüüsib päringu ja sunnib taotluse Azure'i teenus siini.
1. Kohapealse andmete lüüsi küsitlused Azure'i teenus siini jaoks ootel kutsed.
1. Lüüsi saab päringu dekrüptib mandaadi ja loob ühenduse andmeallikad mandaadi.
1. Lüüsi saadab päringu täitmise andmeallikas.
1. Tulemused saadetakse andmeallikast uuesti lüüsi ja seejärel peale pilveteenusesse. Teenuse kasutab tulemused.

## <a name="troubleshooting"></a>Tõrkeotsing

### <a name="update-to-the-latest-version"></a>Uusima versiooni

Suure hulga probleemide saab kuvada, kui lüüsi versiooni on aegunud.  See on üldine hea tava veenduge, et teil on uusim versioon.  Kui kuus või enam, ei ole värskendatud lüüsi, võite installida lüüsi uusim versioon ja vaadake, kui kutsute probleemi uuesti esile.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Tõrge: Nurjus kasutaja lisamiseks rühma. (-2147463168 PBIEgwService jõudluse logi kasutajad)

See tõrge võib ilmneda, kui proovite installida lüüsi domeenikontrolleri, mis pole toetatud. Peate arvutisse, mis pole domeenikontrolleri lüüsi juurutamine.

## <a name="tools"></a>Tööriistad

### <a name="collecting-logs-from-the-gateway-configurator"></a>Lüüsi konfiguraatori koguda logid

Kogute mitme lüüsi logid. Alati alustada logid!

#### <a name="installer-logs"></a>Installeri logid

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Logide konfigureerimine

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Ettevõtte lüüsi teenuste logid

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Sündmuselogide

Andmehalduslüüs ja PowerBIGateway logid on olemas, klõpsake jaotises **rakenduste ja teenuste logid**.

### <a name="fiddler-trace"></a>Fiddler jälgimine

[Viiuldaja](http://www.telerik.com/fiddler) on tasuta tööriist Telerik, mis jälgib liikluse HTTP kaudu.  Saate vaadata tagasi ja edasi teenuse Power BI klientarvutis. See võib kuvada tõrgete ja muu teave.

## <a name="next-steps"></a>Järgmised sammud
- [Kohapealse ühenduse loomiseks loogika rakendused](app-service-logic-gateway-connection.md)
- [Ettevõtte Kliendiintegratsiooni funktsioonide](app-service-logic-enterprise-integration-overview.md)
- [Loogika rakenduste konnektorid](../connectors/apis-list.md)