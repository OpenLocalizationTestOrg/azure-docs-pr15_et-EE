<properties
    pageTitle="Konfiguratsiooni Assessment lahenduse Log Analytics | Microsoft Azure'i"
    description="Konfiguratsiooni Assessment lahendus Log Analytics annab teile üksikasjalikku teavet teie süsteemi keskmist Toiminguhalduri serveri taristu hetkeseisu Toiminguhalduri agentide või mõne rühma Toiminguhalduri kasutamisel."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="configuration-assessment-solution-in-log-analytics"></a>Konfiguratsiooni Assessment lahenduse Log Analytics

Konfiguratsiooni Assessment lahendus Log Analytics aitab leida võimalikud serveri konfigureerimisprobleemide kaudu, teatiste ja teadmiste soovitusi.

See lahendus on vaja System Center Operations Manager. Konfiguratsiooni Assessment pole saadaval, kui kasutate ainult agentide otse ühendatud.

Konfiguratsiooni Assessment lahenduse osa teabest vaatamine nõuab brauseri Silverlighti lisandmoodul.

>[AZURE.NOTE] 5 juuli 2016 algab konfiguratsiooni Assessment lahenduse saab lisada enam Log Analytics tööruumide ja seda lahendust pole enam saadaval olemasolevatele kasutajatele pärast 1 August 2016. Klientidele, kes kasutavad seda lahendust SQL Server või Active Directory, soovitame selle asemel kasutada [SQL serveri Assessment](log-analytics-sql-assessment.md), [Active Directory hindamise](log-analytics-ad-assessment.md) ja [Active Directory kopeerimise olek](log-analytics-ad-replication-status.md) lahendused. Klientidele, kes kasutavad konfiguratsiooni assessment Windowsi jaoks, Hyper-V ja System Center virtuaalse masina Manager, on soovitatav kasutada saidikogumi sündmus ja muutuste jälitus võimaluste saada tervikliku ülevaate probleemidest jooksul teie keskkonnas.

![Konfiguratsiooni Assessment paan](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Otsingukonfiguratsiooni andmete kogutud jälgida serverid ja seejärel saadetakse OMS teenuse pilveteenuses töötlemiseks. Loogika on saadud andmetele rakendatud ja pilveteenusesse andmed. Töödeldud andmete serverid on märgitud järgmisi teemasid:

- **Teatiste:** Kuvatakse konfiguratsiooni seotud, aktiivne teatised, mis on tekkinud jälgida servereid. Need on toodetud reeglid, mille autoriks Microsoft Customer ja head tavad välja tugi organisatsiooni (CSS).
- **Teadmisi soovitusi:** Kuvab Microsofti teabebaasi artikleid, mis on soovitatav töökoormus, mis leiduvad oma taristu; need soovitatakse automaatselt vastavalt kasutatavale konfiguratsioonile masina õ kasutamise läbi.
- **Serverid ja analüüsida teenustest:** Näitab serverid ja töökoormus, mis jälgitakse OMS järgi.

![Armatuurlaua konfiguratsiooni hindamine](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Saate analüüsida konfiguratsiooni hinnanguga tehnoloogiad

OMS konfiguratsiooni Assessment analüüsib järgmistest teenustest:

- Windows Server 2012 ja Microsoft Hyper-V Server 2012
- Windows Server 2008 ja Windows Server 2008 R2, sh:
    - Active Directory
    - Hyper-V Server
    - Üldine operatsioonisüsteem
- SQL Server 2008 ja uuemad versioonid
    - SQL serveri andmebaasi mootor
- Microsoft SharePoint 2010
- Microsoft Exchange Server 2010 ja Microsoft Exchange Server 2013
- Microsoft Lync Server 2013 ja Lync Server 2010
- System Center 2012 SP1 – virtuaalse masina haldur

SQL Server, on toetatud järgmised 32-bitine ja 64-bitised väljaanded analüüsi jaoks:

- SQL Server 2016 - kõik versioonid
- SQL serveri 2014 - kõik versioonid
- SQL Server 2008 ja 2008 R2 – kõik versioonid

SQL serveri andmebaasi mootor on analüüsida kõigi toetatud versioonide jaoks. Lisaks SQL serveri 32-bitine versioon on toetatud, kui töötab WOW64 rakendamist.

## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus
Kasutage järgmist teavet, installida ja konfigureerida lahendus.

- Toiminguhalduri jaoks on vaja konfiguratsiooni Assessment lahendus.
- Peate igas arvutis, kuhu soovite hinnake oma konfiguratsiooni Toiminguhalduri agent.
- Konfiguratsiooni Assessment lahendus lisada oma OMS tööruumi abil [lisada Log Analytics](log-analytics-add-solutions.md)Solutions lahendusegaleriist kirjeldatud.  On pole veel konfigureerimine vajalik.

## <a name="configuration-assessment-data-collection-details"></a>Konfiguratsiooni hindamise andmete kogumise üksikasjad

Konfiguratsiooni Assessment kogub konfiguratsiooni andmete, metaandmete ja andmete kasutamise agentide, mida teil on lubatud.

Järgmises tabelis on andmete kogumise meetodid ja muud üksikasjad kohta, kuidas konfiguratsiooni hindamiseks koguda andmeid.

| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Windows|![Ei](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Jah](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ei](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Jah](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Jah](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| kaks korda päevas|

Järgmine tabel sisaldab kogutud konfiguratsiooni Assessment andmetüüpide.

|**Andmetüüp**|**Väljad**|
|---|---|
|Konfigureerimine|Tellijaid, AgentID, EntityID, ManagedTypeID, ManagedTypePropertyID, praegune väärtus, ChangeDate|
|Metaandmete|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, sordib, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-aadress, NetbiosDomainName, LogicalProcessors, dnsnameWindows, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Olek|StateChangeEventId, StateId, NewHealthState, OldHealthState, konteksti, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Teatiste konfigureerimine hindamine
Saate vaadata ja teatiste konfigureerimine hindamise lehe teatiste haldamine. Teatiste öelda tuvastatud probleem, selle põhjust ja kuidas probleemi lahendada. Samuti pakuvad Otsingukonfiguratsiooni sätetest teie keskkonnas, mis võivad põhjustada jõudlusega seotud probleemide kohta teabe.

![Teatiste vaatamine](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] Muud teatised Log Analytics erinevad konfiguratsiooni Assessment teatised. Teatiste kuvamise nõuab brauseri Silverlighti lisandmoodul.

Kui valite üksuse või üksuste lehel teatiste kategooria, kuvatakse loendi serverid või töökoormused teated, mida iga üksuse rakendada.

![teatised](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Iga teade sisaldab artikli link Microsofti teabebaasi. Nendest artiklitest pakuvad lisateavet kohta teatise.

>[AZURE.TIP] Vaikimisi on maksimaalne arv kuvatakse teatiste 2000. Lisateavet teatiste vaatamiseks klõpsake teatise riba teatiste loendi kohal.

Saate klõpsata mis tahes üksust KB artikkel, mis võivad aidata teil probleemi, mis on loodud teatise põhjuse kuvamiseks.

![KB artikkel](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Saate hallata teatiste reegleid teatud reeglite või tunnid reeglite ignoreerimine.

![hallata](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Teadmisi soovitused
Kui vaatate teadmisi soovitusi, teile esitatakse log otsingutulemite loetelu Microsoft KB artiklid soovitatav töökoormus ja arvutites, mis annavad täiendavat teavet kohta teatise.

![otsingutulemite teadmisi soovitusi](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Serverid ja analüüsida töökoormus
Kui vaatate teadmisi soovitusi, teile esitatakse log otsingutulemite loetelu serverid ja töökoormus, mis on teadaolevalt OMS Toiminguhalduri.

![Kui ka töökoormus](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Järgmised sammud

- [Log otsingud Log Analytics](log-analytics-log-searches.md) abil saate vaadata üksikasjalikku konfiguratsiooni andmeid.
