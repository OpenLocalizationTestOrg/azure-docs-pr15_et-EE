<properties
    pageTitle="Azure'i teenuste teatiste vastuvõtmine | Microsoft Azure'i"
    description="Kohaletoimetamisest teatiste reeglid tingimused on täidetud."
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

# <a name="receive-alert-notifications"></a>Teatiste vastuvõtmine

Saate teatise, mis põhinevad jälgimisega seotud mõõdikud või sündmuste oma Azure'i teenuste kohta.

Reegli argumendil väärtuse, kui määratud mõõtühiku väärtus ületab läve määratud, reegli aktiveerub ja saate saata teate. Reegli sündmused, saate reegli saada teatis *iga* sündmuse, või ainult siis, kui sündmuste arv tehakse.

Kui loote reegli, saate valida teenuse administraator ja kaasadministraatorite või teise administraatori, kus saate määrata, et saata meiliteatise. Teatis meilisõnumi saatmise reegel aktiveerub, ja kui teatiste tingimuses on lahendatud.

Saate konfigureerida ja teatiste reeglite kohta teabe saamiseks programmiliselt [REST API -ga](https://msdn.microsoft.com/library/azure/dn931945.aspx) või [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) .

## <a name="create-an-alert-rule"></a>Reegli loomine

1. [Portaali](https://portal.azure.com/)nuppu **Sirvi**ja seejärel ressursi teid huvitava jälgimine.

2. Klõpsake paani **toimingute** Lens **teatiste reeglid** .

3. Klõpsake käsku **Lisa teatis** .

    ![Teatise lisamine](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. Saate oma reegli nimi ja valige kirjeldus, mis kuvatakse meilisõnumi teatis.

5. Kui valite **mõõdikute** saate valida tingimus ja mõõdiku piirmäära väärtus. See on aeg, jälgimine ja graafiku teatiste tegevusele kasutava Azure'i.

    ![Tingimus ja lävi](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. Saate valida **sündmuste**, ja saada teateid, kui teatud sündmuse juhtub.

    ![Sündmused](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Lisaks saate vastutav administraatoritele meiliteatise saatmise.

Pärast nupu **Salvesta**klõpsamist mõne minuti jooksul siis tuleb teavitada iga kord, kui valite mõõdiku ületab läve.

## <a name="managing-your-alert-rules"></a>Oma haldamine

Kui olete loonud reegli, saate vaadata oma teatise eelvaade lävi võrreldes meetermõõdustik eelmise päeva.

![Sündmused](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Muidugi saate redigeerida **selle reegli, ja **lubamine** ja keelamine selle kui soovite ajutiselt peatada selle kohta** .

## <a name="next-steps"></a>Järgmised sammud

* [Teie teatised konfigureerimine webhooks](insights-webhooks-alerts.md) marsruutimiseks teavitamine mitmesuguste kanalite
* [Kuvari teenuse mõõdikute](insights-how-to-customize-monitoring.md) veendumaks, et teie teenus on saadaval ja reageeri.
* [Diagnostika- ja lubada](insights-how-to-use-diagnostics.md) koguda üksikasjalikku suure sagedusega mõõdikute teenust.
* [Kuvari-saadavus ja mis tahes veebilehe tundlikkuse](../application-insights/app-insights-monitor-web-app-availability.md) rakenduse ülevaated nii, et saate teada, kui teie leht on alla.
* Kui soovite mõista täpselt kuidas oma koodi läbimiseks pilveteenuses [kuvari rakenduse jõudlus](../application-insights/app-insights-azure-web-apps.md) .
* [Sündmuste vaatamine ja audit logid](insights-debugging-with-events.md) – siit leiate kõik, mis on juhtunud teenust.
* [Jälita teenuseseisund](insights-service-health.md) uurida, millal on Azure kogenud jõudluse degradeerumine või teenuse segadusi.
