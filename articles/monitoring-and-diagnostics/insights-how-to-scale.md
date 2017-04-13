<properties
    pageTitle="Mastaapimiseks arvu automaatselt või käsitsi | Microsoft Azure'i"
    description="Saate teada, kuidas teenuste Azure skaala."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="scale-instance-count-manually-or-automatically"></a>Mastaapimiseks arvu automaatselt või käsitsi

[Azure portaali](https://portal.azure.com/), saate käsitsi määrata eksemplari arvu teenust või saate seada parameetrid on see automaatselt nõudmisel põhjal. See on tavaliselt edaspidi *skaala välja* või *sisse skaala*.

Enne mastaapimist alusel arvu, kaaluge, et mastaapimist mõjutavad **hinnakirjad taseme** Lisaks eksemplari arv. Erinevatele hinnakirjad tasemetele määrata erinevaid numbreid ja -vormid mälu ja seega on neil parema jõudluse jaoks sama palju eksemplari (mis on *üles* - või *allapoole skaala*). Selles artiklis käsitletakse spetsiaalselt *skaala sisse* ja *välja*.

Muudate portaali ja Lisaks saate [REST API -ga](https://msdn.microsoft.com/library/azure/dn931953.aspx) või [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) reguleerimine mastaabi automaatselt või käsitsi.

> [AZURE.NOTE] Selles artiklis kirjeldatakse, kuidas luua mõne sätte autoscale portaali aadressil [http://portal.azure.com](http://portal.azure.com). Portaali loodud Autoscale sätted ei saa redigeerida selle klassikalise portaali ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Mastaapimist käsitsi

1. [Azure'i portaal](https://portal.azure.com/), klõpsake nuppu **Sirvi**ja seejärel liikuge soovitud mastaapida, nt **rakenduse teenusleping**ressursi.

2. Paani **skaala** **toimingutes** teile oleku skaleerimist: **välja** jaoks, kui teil on skaleerimist käsitsi **kohta,** kui teil on skaleerimist teguriga üks või mitu jõudluse mõõdikute.
    ![Skaala paan](./media/insights-how-to-scale/Insights_UsageLens.png)

3. Klõpsake paani viib teid **skaala** tera. Skaala tera ülaosas näete autoscale toimingute ajaloo teenus.  
    ![Skaala blade](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] See diagramm kuvatakse ainult toimingud, mis on teostatud autoscale. Kui kohandate käsitsi eksemplari arv, ei kajastu Muuda see diagramm.

4. Saate reguleerida käsitsi arv **eksemplarid** liugurit.
5. Klõpsake käsku **Salvesta** ja teile kuvatakse ülespoole eksemplaride arv, et peaaegu kohe.

## <a name="scaling-based-on-a-pre-set-metric"></a>Skaleerimist eelseadistatud mõõdiku põhjal

Kui soovite, et mõõdiku põhjal automaatselt reguleerida eksemplaride arv, valige rippmenüüst **skaala järgi** meetermõõdustik, mida soovite. Näiteks on **rakenduse teenusleping** saate skaalal **CPU protsendi**võrra.

1. Kui valite mõõdiku kuvatakse liugur ja/või, tekstiväljade sisestamiseks soovite mastaapimiseks vahel eksemplaride arv.

    ![Skaala abaluuga CPU protsent](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Autoscale võtab kunagi alla või kohale piirid, mis on määratud, olenemata teie laadi teenust.

2. Teiseks, valite sihtväärtust mõõdiku jaoks. Näiteks kui valisite **CPU protsent**, saate siht Keskmine CPU linnanimede kõigis teie teenust. Välja skaala juhtub, kui keskmine CPU ületab määratlete, samuti ulatusega juhtub iga kord, kui keskmise CPU langeb madalamale.

3. Klõpsake käsku **Salvesta** . Autoscale kontrollib iga mõni minut, veenduge, et olete eksemplari vahemiku- ja sihtsaitide jaoks oma meetermõõdustik. Kui teenust saab täiendavat liiklust, saate mitu eksemplari ei tee midagi.

## <a name="scale-based-on-other-metrics"></a>Muude mõõdikute põhjal skaala

Saate skaala põhjal mõõdikute peale valmissätteid, mida kuvatakse ripploendis **mastaapimiseks** ja isegi on skaala läbi kogumid ja skaala reeglid.

### <a name="adding-or-changing-a-rule"></a>Lisamise või muutmisega reegel

1. Valige rippmenüüst **skaala järgi** **plaanimine ja jõudluse reeglid** : ![jõudluse reeglid](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Kui teil on varem olnud autoscale, klõpsake kuvatakse vaate täpse reegleid, et teil oli.

3. Skaala alusel teise argumendil klõpsake real **Lisa reegel** . Võite klõpsata ka ühte olemasolevate ridade varem mõõdiku soovite mastaapimiseks alusel pidite mõõdiku muuta.
![Reegli lisamine](./media/insights-how-to-scale/Insights_AddRule.png)

4. Nüüd peate valima mis meetermõõdustik, mida soovite skaala järgi. Valimisel silmas pidada paari asja mõõdiku:
    * *Ressursi* mõõdiku pärineb. Tavaliselt on see olla sama, mis teil on skaleerimist ressurss. Kui soovite mastaapimiseks sügavus salvestusruumi järjekorda, on ressurss skaala järgi soovitud järjekorras.
    * *Argumendil nimi* .
    * *Aja liitmine* mõõdiku. See on, kuidas andmed on kombineerimine *kestus*.

5. Pärast valides oma mõõdiku valite lävi mõõdiku ja tehtemärk. Näiteks võiks öelda **suurem kui** **80%**.

6. Valige toiming, mille soovite võtta. On paar erinevat tüüpi toiminguid.
    * Suurendamine või vähendamine võrra – see on lisamine või eemaldamine **väärtus** määratlete eksemplaride arv
    * Suurendage või vähendage protsenti – see muudab eksemplari arv protsenti. Näiteks saate võiks panna 25 välja **väärtus** ja kui teil praegu 8 eksemplaris, lisatakse 2.
    * Suurendamine või vähendamine – seatakse eksemplari arv **väärtus** määratlete.

7. Lõpetuseks, saate valida lahedad alla - kui kaua see reegel peaks ootama pärast skaala eelmise toimingu uuesti skaala.

8. Pärast reegli konfigureerimist tabas **OK**.

9. Kui olete konfigureerinud soovite reegleid, kindlasti tabas käsku **Salvesta** .

### <a name="scaling-with-multiple-steps"></a>Mitme juhistega mastaapimine

Näidetes on üsna lihtne. Juhul, kui soovite olla tõhusamaks kohta skaleerimist üles (või alla), saate isegi lisada mitme skaala reeglid sama meetermõõdustik. Näiteks saate määrata kahe skaala reeglite kohta protsent CPU:

1. Mastaapimiseks 1 astme, kui CPU protsent on üle 60%
2. Mastaapimiseks, 3 eksemplaris, kui CPU protsent on üle 85%

![Mitme skaala reeglid](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Täiendavad reegliga kui oma laadi ületab 85% enne skaala toimingut, saate ühe asemel kaks täiendavad eksemplarid.

## <a name="scale-based-on-a-schedule"></a>Skaala ajakava põhjal


Vaikimisi skaala reegli loomisel selle alati rakendatakse. Näete, et profiili päise klõpsamisel:

![Profiil](./media/insights-how-to-scale/Insights_Profile.png)

Aga soovite on tõhusamaks mastaapimine päeva või nädal, kui nädalavahetuse. Teil võib isegi sulgeda teenust täielikult välja tööaeg.

1. Seda teha, on teil profiili, valige **Korduvus** asemel **alati,** ja valige korda, mida soovite rakendada profiili.

2. Näiteks on profiil, mis kehtib nädala jooksul, **päeva** rippmenüüst tühjendage **laupäevast** ja **pühapäevast**.

3. Profiili, mis kehtib päeval olevat seatud **alguskellaaeg** kellaaeg, mida soovite käivitada.
    ![Vaikimisi Korduvus.](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Klõpsake nuppu **OK**.

5. Järgmiseks peate lisamiseks profiili, mida soovite rakendada muul ajal. Klõpsake käsku **Lisa profiili** rida.
    ![Praegu ei tööta](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Teie uus, teise profiili nimi, näiteks võite nimedeks see **praegu ei tööta**.

7. Klõpsake uuesti nuppu **Korduvus** ja valige eksemplari arv vahemikus soovite selle aja jooksul.

8. Vaikeprofiili, valida **päeva** kirjutamise ajal soovite selle reegli rakendamiseks ja **alguskellaaeg** päeva jooksul.

>[AZURE.NOTE] Autoscale kasutab sellest, millist **ajavööndi** valite suve säästude reegleid. Siiski ajal kuvatakse UTC-ajavahe base ajavööndi nihke, mitte suve säästude UTC offset suve aeg.

9. Klõpsake nuppu **OK**.

10. Nüüd, peate lisada mis tahes ajal teise profiili soovite rakendada reegleid. Klõpsake nuppu **Lisa reegel**ja seejärel võib ehitada ajal vaikeprofiili teile sama reegel.
    ![Lisa reegel välja töötamine](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Veenduge, et luua nii reegli skaala läbi ja skaala või klõpsake siis profiili eksemplari arv käigus kasvab (või üksnes vähendamiseks).

12. Lõpuks nuppu **Salvesta**.

## <a name="next-steps"></a>Järgmised sammud

* [Kuvari teenuse mõõdikute](insights-how-to-customize-monitoring.md) veendumaks, et teie teenus on saadaval ja reageeri.
* [Diagnostika- ja lubada](insights-how-to-use-diagnostics.md) koguda üksikasjalikku suure sagedusega mõõdikute teenust.
* Iga kord, kui funktsionaalseid sündmuste [teatiste vastuvõtmine](insights-receive-alert-notifications.md) või mõõdikute risti läve.
* Kui soovite mõista täpselt kuidas oma koodi läbimiseks pilveteenuses [kuvari rakenduse jõudlus](../application-insights/app-insights-azure-web-apps.md) .
* [Sündmuste vaatamine ja audit logid](insights-debugging-with-events.md) teavet kõike, mis on juhtunud teenust.
* [Kuvari-saadavus ja mis tahes veebilehe tundlikkuse](../application-insights/app-insights-monitor-web-app-availability.md) rakenduse ülevaated nii, et saate teada, kui teie leht on alla.
