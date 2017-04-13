<properties
   pageTitle="Mis on Log Analytics? | Microsoft Azure'i"
   description="Log Analytics on teenus sisse toimingud halduse komplekti (OMS), mis aitab teil loodud ressursid oma pilveteenuses funktsionaalseid andmete kogumise ja ja kohapealse keskkonna.  Selles artiklis on toodud lühiülevaade erinevate osade Log analüüsi- ja üksikasjalik sisu linke."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Mis on Log Analytics?
Log Analytics on teenus [Operations Management Suite \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) mis aitab teil koguda ja ressursid oma pilveteenuses loodud andmete analüüsimine ja asutusesiseses keskkonnas. Siit leiate reaalajas integreeritud otsing ja kohandatud armatuurlaudade abil hõlpsasti analüüsida miljonid kirjed kõigis oma töökoormus ja serverid sõltumata nende füüsilise asukoha ülevaateid.


## <a name="log-analytics-components"></a>Logige Analytics komponendid
Log Analytics keskele on OMS hoidla, mis on majutatud Azure pilveteenuses.  Andmed on kogutud hoidlasse ühendatud allikatest konfigureerimine andmeallikate ja lisamise lahendused tellimusele.  Andmeallikate ja lahenduste iga loob erinevate kirjetüüpe, mis on oma atribuutide, kuid võib siiski koos analüüsida päringute hoidlasse.  See võimaldab teil töötamine erinevaid andmeid erinevatest allikatest kogutud samade tööriistade ja meetodite abil.


![OMS hoidla](media/log-analytics-overview/overview.png)


Ühendatud allikad on arvutid ja muud ressursid, millest kogutud Log Analytics andmete loomiseks.  See kaasata agentide installitud arvutitesse [Windows](log-analytics-windows-agents.md) ja [Linux](log-analytics-linux-agents.md) otse ühendust luua või agentide [ühendatud System Center Operations Manager rühma](log-analytics-om-agents.md).  Log Analytics saate andmete kogumine ka [Azure salvestusruumi](log-analytics-azure-storage.md).

[Andmeallikad](log-analytics-data-sources.md) on kogutud andmete erinevat tüüpi iga ühendatud andmeallikast.  See hõlmab sündmused ja [tulemustega seotud andmete](log-analytics-data-sources-performance-counters.md) [Windows](log-analytics-data-sources-windows-events.md) ja Linux agentide Lisaks näiteks [IIS-i logid](log-analytics-data-sources-iis-logs.md)ja [kohandatud teksti logid](log-analytics-data-sources-custom-logs.md).  Saate konfigureerida iga andmeallika, mida soovite koguda ja konfiguratsiooni saadetakse automaatselt iga ühendatud allikas.


## <a name="analyzing-log-analytics-data"></a>Log Analytics andmete analüüsimine
Enamik teie Suhtlus logi Analytics on mis tahes töötab ja annab teile juurdepääsu konfiguratsioonisätted ja mitme tööriistad analüüsimiseks ja toimivad kogutud andmete OMS portaali kaudu.  Portaalist saate kasutada [log otsingud](log-analytics-log-searches.md) , kus te ehitada päringute analüüsimiseks kogutud andmete [armatuurlaudade](log-analytics-dashboards.md) saate kohandada vaatega graafilise kõige hinnatuma otsingute ja [lahendused](log-analytics-add-solutions.md) lisafunktsioone ja tööriistad.

![OMS portaal](media/log-analytics-overview/portal.png)


Log Analytics pakub päringu süntaks hoidla andmete konsolideerimine ja kiiresti alla laadida.  Saate luua ja salvestada [Log otsingud](log-analytics-log-searches.md) otse OMS portaali andmete analüüsiks või on log otsingud käivitamine automaatselt luua teatise, kui päringu tulemused näitavad tingimuses oluline.

![Logi otsing](media/log-analytics-overview/log-search.png)

Graafilise kiirvaate üldine keskkonna seisundi andmiseks saate lisada [armatuurlaua](log-analytics-dashboards.md)visualiseeringute salvestatud log otsingute.   

![Armatuurlaua](media/log-analytics-overview/dashboard.png)

Selleks, et analüüsida väljaspool Log Analytics andmete, saate eksportida andmed OMS hoidla tööriistad, nt [Power BI](log-analytics-powerbi.md) või Excel.  Samuti saate kasutada [Log otsingu API](log-analytics-log-search-api.md) luua kohandatud lahendusi, mida kasutada Log Analytics andmete või muudest süsteemidest integreerida.

## <a name="solutions"></a>Lahendused
Lahenduste lisada Log Analytics funktsioone.  Eelkõige pilveteenuses käivitamine ja OMS hoidlas kogutud andmete analüüsi. Samuti võib kindlaks määrata uue kirje tüüpi võib analüüsida Log otsinguid või täiendavate kasutajaliides, mis on esitatud lahendus OMS armatuurlaua koguda.  

![Jälgimise lahendus muutmine](media/log-analytics-overview/change-tracking.png)


Lahenduste jaoks on saadaval mitmesuguseid funktsioone, ja saate hõlpsalt sirvida saadaval lahenduste ja [lisada need oma OMS tööruumi](log-analytics-add-solutions.md) lahendusegaleriist.  Paljud juurutatakse automaatselt ja alustada tööd kohe, kui teised võivad nõuda mõned konfiguratsioon.

![Lahendusegalerii](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Kasutusanalüüsi arhitektuur
Juurutamise nõudeid Log Analytics on minimaalne, kuna keskse komponendid on majutatud Azure pilveteenuses.  See hõlmab lisaks teenuseid, mis võimaldavad teil oleksid ja kogutud andmete analüüsiks hoidlasse.  Portaali pääseb mis tahes brauseri kaudu, seega ei pea klienttarkvara jaoks.

Installige [Windows](log-analytics-windows-agents.md) ja [Linux](log-analytics-linux-agents.md) arvutitesse agentide, kuid on pole täiendavad agent nõutav arvutitele, mis on juba [ühendatud SCOM rühma](log-analytics-om-agents.md)liikmed.  SCOM agentide jätkab management serverid, mis edastab oma andmeid Log Analytics suhelda.  Mõned lahendused küll vajavad agentide Log Analytics suhtlemiseks.  Oma side nõuetele täpsustatakse iga lahenduse dokumentatsioonist.

Kui te [Log Analytics kasutajaks](log-analytics-get-started.md), loote mõne OMS tööruumi.  Mõelge tööruumi nimega on kordumatu OMS keskkond, mis on oma andmete talletuskoht, andmeallikate ja lahendusi. Mitme tööruume saate luua teie tellimus toetab mitut keskkonnas, näiteks ja testida.

![Kasutusanalüüsi arhitektuur](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Järgmised sammud

- [Tasuta Log Analytics konto](log-analytics-get-started.md) Testige oma keskkonnas.
- Saate vaadata erinevate [Andmeallikate](log-analytics-data-sources.md) andmekogumise hoidlasse OMS saadaval.
- [Liikuge saadaolevate lahenduste Lahendusegaleriis](log-analytics-add-solutions.md) Log Analytics funktsioone lisada.
