<properties
    pageTitle="Azure'i rakenduse teenuses rakenduste jälgimine"
    description="Saate teada, kuidas jälgida rakenduste Azure rakenduse teenuses Azure portaali kaudu."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Kuidas: Azure'i rakenduse teenuses rakenduste jälgimine

[Rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=529714) pakub ehitatud järelevalve funktsiooni [Azure portaali](https://portal.azure.com).
See sisaldab üle vaadata, **kui ka **mõõdikute** rakendus kui ka rakenduse teenusleping, **teatiste** ja isegi **skaleerimist** nende mõõdikute põhjal automaatselt häälestada** .

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Mõistmine kvootide ja mõõdikud

### <a name="quotas"></a>Kvoote

Rakendused rakendus teenuses kehtivad teatud *piirangud* ressursid, nad saavad kasutada. Piiranguid on määratletud rakendusega seotud **rakenduse teenusleping** .

Kui rakendus on majutatud **tasuta** või **ühiskasutuses** leping, on rakenduse abil saate ressursside piirangud **kvootide**määratletud.

Kui rakendus on majutatud on **lihtne**, **Standard** või **Premium** leping, siis piiranguid nad saavad kasutada ressursside on määratud **arvu** (1, 2, 3,...) **rakenduse teenusleping**ja **suurus** (väike, Keskmine ja suur).

**Kvootide** **tasuta** või **ühiskasutuses** rakenduste jaoks on:

* **CPU(Short)**
   * 3-minutilise intervalli selle rakenduse jaoks lubatud CPU summa. See kvoot uuesti määrab iga 3 minutit.
* **CPU(Day)**
   * Kogusumma CPU päevas selle rakenduse jaoks lubatud. See kvoot uuesti määrab keskööst UTC iga 24 tundi.
* **Mälu**
   * Selle rakenduse jaoks lubatud mälu kogusumma.
* **Läbilaskevõime**
   * Selle rakenduse päevas lubatud väljamineva läbilaskevõime kogusumma.
   See kvoot uuesti määrab keskööst UTC iga 24 tundi.
* **Failisüsteemi**
   * Kogusumma ruumi.

Ainult kvoodi rakenduste majutatud **tavaline**, **Standard** ja **Premium** lepingute korral on **failisüsteemi**.

Lisateavet teatud piirmäära, piirangud ja funktsioonid on erinevate rakenduse teenuse SKU-de jaoks saadaval, leiate siit: [Azure'i tellimuse teenuse piirangud](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kvoodi täitmine

Kui rakenduse kasutust ületab **CPU (lühendatud)**, **CPU (päev)**või **läbilaskevõime** kvoodi siis rakenduse seniks, kuni kvoot uuesti määrab. Selle aja jooksul kõik sissetulevad taotlused tulemuseks on **HTTP 403**.
![][http403]

Kui rakenduse **mälu** kvoodi ületamise korral siis rakenduse saab alustada.

**Failisüsteemi** kvoodi ületamise korral, siis mis tahes kirjutada toiming nurjub, see hõlmab kirjutamist soovite logid.

Kvootide saab või eemaldada rakenduse täiendamine oma rakenduse teenusleping.

### <a name="metrics"></a>Mõõdikud

**Mõõdikute** teavet rakenduse või teenuse rakenduse leping käitumine.

**Rakenduse**, on saadaval mõõdikute.

* **Vastuse Keskmine kellaaeg**
   * Keskmiselt rakenduse teenida taotlusi ms kuluv aeg.
* **Keskmine Mälu töökomplekt**
   * Mälu MiB-ID, mis kasutavad rakendust keskmine summa.
* **CPU aega**
   * Summa CPU sekundites tarbitud rakenduse abil. Lisateavet selle argumendil vt: [CPU aja vs CPU protsent](#cpu-time-vs-cpu-percentage)
* **Andmete**
   * Sissetuleva läbilaskevõime tarbitud MiB-ID rakendus summa.
* **Andmete välja**
   * Väljamineva läbilaskevõime tarbitud MiB-ID rakendus summa.
* **Http 2xx**
   * Http olekukoodi tulemuseks taotluste arv > = 200, kuid < 300.
* **Http 3xx**
   * Http olekukoodi tulemuseks taotluste arv > = 300, kuid < 400.
* **Http 401**
   * Taotlusi HTTP 401 olekukoodi tulemuseks arvu.
* **Http 403**
   * Taotlusi HTTP 403 olekukoodi tulemuseks arvu.
* **Http 404**
   * Taotlusi HTTP 404 olekukoodi tulemuseks arvu.
* **Http 406**
   * HTTP 406 olekukoodi tulemuseks taotluste arv.
* **Http 4xx**
   * Http olekukoodi tulemuseks taotluste arv > = 400, kuid < 500.
* **Http Serveri tõrked**
   * Http olekukoodi tulemuseks taotluste arv > = 500, kuid < 600.
* **Mälu töökomplekt**
   * Praegune summa MiB-ID rakendus kasutatud mälu.
* **Taotlused**
   * Sõltumata nende tulemuseks HTTP olekukoodi taotluste koguarv.

Mõne **rakenduse teenusleping**on saadaval mõõdikute:

>[AZURE.NOTE] Rakenduse teenuse leping mõõdikute saadaval ainult lepingud, **tavaline**, **Standard** ja **Premium** SKU-ga.

* **CPU protsent**
   * Keskmine CPU kasutatud üle kõik eksemplarid leping.
* **Mälu protsent**
   * Keskmine mälu üle kõik eksemplarid leping.
* **Andmete**
   * Keskmine sissetulevate läbilaskevõime kasutatud üle kõik eksemplarid leping.
* **Andmete välja**
   * Väljaminevat läbilaskevõime üle kõik eksemplarid leping kasutatakse keskmise.
* **Ketas järjekorra pikkus**
   * Keskmine arv lugeda ja kirjutada taotlusi, mis olid ootele säilitamise kohta. Suure ketta järjekorra pikkus on rakendus, mis võib aeglustavad tõttu liigse ketta I/O märge.
* **Http järjekorra pikkus**
   * HTTP-päringud, mis oli enne täidetud kuhjuda istuda keskmine arv. Kõrge või suurenevad HTTP järjekorra pikkus on suur koormus lepingu sümptom.

### <a name="cpu-time-vs-cpu-percentage"></a>CPU aja vs CPU protsent
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

On 2 mõõdikud kajastuvad CPU hõivatus. **CPU protsent** ja **kellaaega**

**CPU aega** on kasulik rakendused majutatud **tasuta** või **ühiskasutuses** lepingutes, kuna üks nende kvootide on määratletud CPU minutit kasutavad rakendust.

**CPU protsent** teiselt on kasulik majutatud **põhi**-, **standard** - ja **Premiumi** lepingutes, kuna nad saavad mastaabitud välja ning selle mõõdiku on hea märk üldine kasutus mitmes eksemplaris kõik rakendused.

##<a name="metrics-granularity-and-retention-policy"></a>Mõõdikute granulaarsus ja säilituspoliitika

Rakendus ja rakenduse teenusleping mõõdikud on sisse logitud ja koondatud järgmised granulaarsused ja säilituspoliitikate teenuse.

 * **Minutid** granulaarsus mõõdikute säilitatakse **48** tundi
 * **Tund** granulaarsus mõõdikute säilitatakse **30** päeva
 * **Päeva** granulaarsus mõõdikute säilitatakse **90** päeva

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Jälgida kvoote ja mõõdikute Azure'i portaalis.

Erinevate **kvootide** ning **mõõdikute** mõjutamata rakenduse [Azure portaali](https://portal.azure.com)olekut saate vaadata.

![][quotas]
**Kvootide** leiate jaotises Sätted >**piirmäärasid**. Funktsiooni Kasutuskogemuse võimaldab teil läbi vaadata: (1) kvootide nimi, (2) oma lähtestamine intervall, (3) oma praegune piirang ja (4) praegust väärtust.

![][metrics]
**Mõõdikute** saab Accessi ressursi keelest. Saate kohandada diagrammi: (1) **nuppu** ja (2) valige käsk **Redigeeri diagrammi**.
Siit saate muuta **ajavahemiku**(3), (4) **diagrammitüüpi**ja (5) **mõõdikute** kuvamiseks.  

Saate lisateavet mõõdikute siit: [kuvari teenuse mõõdikute](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Teatiste ja Autoscale
Mõõdikute jaoks mõne rakenduse või teenuse rakenduse lepinguga saate ühendatud kuni teatised, lugege lisateavet selle kohta leiate teemast [teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Rakenduse teenuse rakendused majutatud basic, standard või premium rakenduse teenuse lepingud toetavad **autoscale**. See võimaldab teil konfigureerida reeglid, mis rakenduse teenuse leping mõõdikute jälgimine ja saate suurendada või vähendada lisaressursse vastavalt vajadusele eksemplari arvu või salvestamine raha kui rakendus on üle säte. Saate lisateavet automaatse skaala siin: [Kuidas skaala](../monitoring-and-diagnostics/insights-how-to-scale.md) ja siin [Azure'i kuvari autoscaling head tavad](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
