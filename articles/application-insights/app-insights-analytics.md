<properties 
    pageTitle="Analüüsi -, rakenduse ülevaated tööriista võimas otsing | Microsoft Azure'i" 
    description="Kasutusanalüüsi tööriista võimas diagnostika otsing, rakenduse ülevaated ülevaade. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Klõpsake rakenduse ülevaated Analytics


[Analytics](app-insights-analytics.md) on [Rakenduse ülevaated](app-insights-overview.md)võimsaid otsingufunktsiooni. Nende lehtede kirjeldada Analytics päringu lanquage. 

* **[Vaata tutvustavat videot](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* Kui teie rakendus pole andmete saatmine veel rakenduse ülevaated **[test drive Analytics meie jäljendatud andmeid](https://analytics.applicationinsights.io/demo)** .

## <a name="queries-in-analytics"></a>Päringute Kasutusanalüüsi
 
Tüüpilised päring on *järgneb *tehtemärgid* eraldatud sarja lähtetabeliga* `|`. 

Näiteks Oletame teada, mis kellaajal Hyderabad kodanike proovige meie web app. Ja kui me seal, vaatame, mis tulemus koodid tagastatakse HTTP-päringud. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Me loendamine erinevate kliendi IP-aadressid, rühmitamise tund päeva viimase 7 päeva jooksul. 

Vaatame lintdiagrammi esitlus, virnastada tulemused erinevad vastuse koodide valimise tulemite kuvamiseks:

![Valige lintdiagramm, x ja y-telge, siis osadeks](./media/app-insights-analytics/020.png)

Paistab, et rakendus on kõige populaarsemate lõunaaeg ja Double-ajal Hyderabad. (Ja uurime nende 500 koodide.)


On ka võimas statistika toimingud:

![](./media/app-insights-analytics/025.png)


Soovitud keel on palju atraktiivseks funktsioone.

* [Filtri](app-insights-analytics-reference.md#where-operator) oma töötlemata rakenduse telemeetria kõik väljad, sh oma kohandatud atribuudid ja mõõdikute järgi.
* [Liitumine](app-insights-analytics-reference.md#join-operator) mitme tabeli – seostada taotlusi lehe vaateid, sõltuvus kõnesid, erandid ja log jälgi.
* Võimas statistika [liitmised](app-insights-analytics-reference.md#aggregations).
* SQL-i sama võimas, kuid märksa lihtsam keerukate päringute: asemel pesastamise laused, mida toru järgmise ühe põhikooli toiminguga andmed.
* Vahetu ja võimas visualiseeringuid.







## <a name="connect-to-your-application-insights-data"></a>Oma rakenduse ülevaated andmetega ühenduse loomine


Klõpsake rakenduse ülevaated oma rakenduse [Ülevaade tera](app-insights-dashboards.md) kaudu avatud Analytics: 

![Avage portal.azure.com, avage oma rakenduse ülevaated ressurss ja seejärel klõpsake käsku analüütika.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Piirangud

Praegu päringutulemite on piiratud andmete viimase nädala jooksul.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Järgmised sammud


* Soovitame alustada [keele tutvustus](app-insights-analytics-tour.md).