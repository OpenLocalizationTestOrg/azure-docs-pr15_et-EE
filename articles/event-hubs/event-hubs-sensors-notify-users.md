<properties 
   pageTitle="Teavitage kasutajaid andurid või muudest süsteemidest saadud andmeid | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse kasutajate teavitamis andurite andmeid sündmuse jaoturi abil."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Teavitage kasutajaid andurid või muudest süsteemidest saadud andmeid

Oletame, et teil on rakendus, mis jälgib andmete reaalajas või toodab aruannete ajakava. Kui saate vaadata nende reaalajas diagrammide või aruanded kuvatakse veebisaidil, võite näha midagi, mis nõuab. Mida teha, kui peate olema teatatud olukordades, mitte tuginedes uuendatakse märkige veebisaidi kohta? Oletage, et teil on kasvab valguses kasvuhoone ja peate teadma kohe, kui kerge läheb. Üks viis selleks, et oleks koos mõne kasutage kasvuhoone, korraldamine saata meilisõnumi, kui kerge on välja lülitatud.

![][1]

Teise stsenaariumi oletage, et käivitate lemmikloomade mineku poole ja teil peab madal laoseisu pakkumise soovite saada teatisi. Näiteks võib korraldada saata tekstsõnumeid ERP-süsteemist, kui teie laoseisu jälgimine hüpata toidu on kukkunud kriitiline tase. 

![][2]

Probleem on olulise teabe hankimine, kui teatud tingimused on täidetud, ei, kui saate navigeerida väljamöllimiseks staatiline aruanne. Kui kasutate [Azure sündmuse jaoturi][] - või [Azure asjade jaoturi][] seadmed või ettevõtte rakendusi nagu [Dynamics AXI][]andmete saamiseks, on teil mitu võimalust, kuidas neid töödelda. Saate vaadata nende veebisaidil, saate need analüüsida, saate need talletada ja saate neid kasutada käivitamiseks käsud midagi teha. Selleks saate kasutada võimsaid tööriistu, nagu [Azure veebisaitide][], [SQL Azure'i][], [Hdinsightiga][], [Cortana ärianalüüsi komplekti][], [Asjade komplekti][], [Loogika rakendused][]või [Azure teatis jaoturi][]. Kuid mõnikord kõik, mida soovite teha andmeid saata kellelegi minimaalselt pea kohal. Näitab teile, kuidas seda teha vaid natuke koodi, oleme lisanud uue valimi [AppToNotifyUsers][]. Suvandeid on e-posti (SMTP), SMS-i ja telefoni.

## <a name="application-structure"></a>Rakenduse struktuuri

Rakendus on kirjutatud C# ja readme-faili valimis sisaldab kogu teave, mida soovite muuta, luua ja avaldada rakendus. Järgmistes jaotistes on üksikasjalik ülevaade sellest, mida rakendus ei.

Alustame eeldusel, et teil on kriitilised sündmused, et lükata Azure'i sündmuse jaoturi või asjade jaoturi. Mis tahes jaoturi tuleb teha, kui teil on juurdepääs ja leida ühendusstring.

Kui teil on juba mõni sündmus jaoturi või asjade jaoturi, saate hõlpsasti häälestada testi Double on Arduino kilp ja Vaarika Pi, Projectis [Ühenduse The punkti](https://github.com/Azure/connectthedots) juhistele. Selle kohta, et Arduino kaitse kasutage saadab pimendus kaudu funktsiooni Pi [Azure'i sündmuse jaoturi][] (**ehdevices**) ja on [Azure voo Analytics](https://azure.microsoft.com/services/stream-analytics/) töökohtade sunnib teatiste teise sündmuse jaoturi (**ehalerts**), kui pimendus saanud alla teatud taseme.

**AppToNotify** käivitamisel loeb konfiguratsioonifail (App.config) URL-i ja mandaadi saamiseks sündmuse jaoturi, teatiste vastuvõtmine. Seejärel koeb protsessi pidevalt jälgida, et sündmuse jaoturi kõik sõnumid, mis tuleb läbi – kui teil on juurdepääs sündmuse jaoturi või asjade jaoturi ja sobiv mandaate URL-i, selle sündmuse jaoturi lugeja koodi pidevalt ei loe, mis on peagi saadaval. Käivitamisel rakendus tuvastab URL-i ja sõnumiteenuse (e-posti, SMS, telefon), mida soovite kasutada, ja nimi ja aadress saatja ja adressaatide loendi mandaat.

Kui sündmus jaoturi kuvari tuvastab sõnumi, see käivitab protsessi, mis saadab sõnumi konfiguratsioonifailis määratud meetodil. Pange tähele, et see saadab iga sõnumi tuvastatakse. Kui määratud punkti kuvari sündmuse jaoturiga, mis võtab kümme sõnumeid sekundis, saatja saadab kümme sõnumeid sekundis – kümne meilisõnumite sekundis, 10 SMS-sõnumite sekundis, kümme telefonikõnede sekundis. Seetõttu veenduge, et teil jälgida sündmuse jaoturi, mida saab ainult teatised, mille soovite saata, mitte sündmuse jaoturi, mis saab kõik toorandmetega andurid või rakenduste.

## <a name="applicability"></a>Kohaldamine

Seda proovi kood kuvatakse ainult kuidas jälgida sündmuse jaoturi ja kuidas helistada välisteenused sõnumside juhuks, kui soovite lisada rakenduse seda funktsiooni. Pange tähele, et see lahendus on DIY, arendaja värskendustest näide ainult. Ettevõtte nõuded ei käsitleta nagu koondamise ei lõppenud, taaskäivitage korral tõrge jne. Veel põhjalik ja tootmise lahendusi, vaadake järgmist.

- Konnektorid või Tõuketeatiste [Azure'i loogika rakenduste](../app-service-logic/app-service-logic-connectors-list.md) teenuse kasutamise abil.
- Kasutades [Azure teatis jaoturi](https://msdn.microsoft.com/library/azure/jj927170.aspx), nagu on kirjeldatud ajaveebi [leviedastuse Tõuketeatiste miljoneid mobiilsideseadmete Azure'i teatis jaoturi abil](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs). 

## <a name="next-steps"></a>Järgmised sammud

See on väga lihtne luua lihtsa teavitusteenuse, mis saadab e-kirju või tekstsõnumeid adressaatide või helistab neid saanud sündmuse jaoturi või asjade jaoturi relay andmetega. Kasutajate neid jaoturi saadud teabe lahenduse juurutamine, külastage [AppToNotifyUsers][].

Nende jaoturi kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Azure'i sündmuse jaoturi]
- [Azure'i asjade jaoturi]
- Kuvatakse [sündmuse jaoturi õpetuse]alustamine.
- Täieliku [valimi rakendus, mis kasutab sündmuse jaoturi].

Kasutajate neid jaoturi saadud andmete põhjal lahenduse juurutamine, külastage:

- [AppToNotifyUsers][]

[Sündmuse jaoturi õpetus]: event-hubs-csharp-ephcs-getstarted.md
[Azure'i asjade jaoturi]: https://azure.microsoft.com/services/iot-hub/
[Azure'i sündmuse jaoturi]: https://azure.microsoft.com/services/event-hubs/
[Azure'i sündmuse jaoturi]: https://azure.microsoft.com/services/event-hubs/
[valimi rakendus, mis kasutab sündmuse jaoturi]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AXILE]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure'i veebisaidid]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure'i]: https://azure.microsoft.com/services/sql-database/
[Hdinsightiga]: https://azure.microsoft.com/services/hdinsight/
[Cortana ärianalüüsi komplekti]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[Asjade komplekti]: https://azure.microsoft.com/solutions/iot-suite/
[Loogika rakendused]: https://azure.microsoft.com/services/app-service/logic/
[Azure'i teatis jaoturi]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png