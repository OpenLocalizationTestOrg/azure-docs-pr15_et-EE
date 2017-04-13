<properties
    pageTitle="Lahendus võimsuse Log Analytics | Microsoft Azure'i"
    description="Saate kasutada võimsus kavandamise lahenduse Log Analytics aidata teil mõista teie Hyper-V serverite hallatavate System Center virtuaalse masina Manager võimsus"
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

# <a name="capacity-management-solution-in-log-analytics"></a>Log Analytics võimsuse lahendus


Log Analytics võimsus kavandamise lahenduse abil saate aidata teil mõista teie Hyper-V serverite hallatavate System Center virtuaalse masina Manager võimsus. See lahendus on vaja System Center Operations Manager nii System Center virtuaalse masina Manager. Võimsuse kavandamisel pole saadaval, kui kasutate ainult agentide otse ühendatud. Installida lahenduse Toiminguhalduri agent värskendada. Lahendus loeb jõudluse hinnale jälgida serveris ja saadab kasutusandmete OMS teenuse pilveteenuses töötlemine. Loogika on rakendatud andmete kasutus ja pilveteenusesse andmed. Aja jooksul, tuvastatakse mustreid ja võimsus prognooside, praegune tarbimine põhjal.

Näiteks võib mõne projektsiooni tuvastada, kui täiendavad protsessori või täiendavaid mälu on vaja omaette serveri. Selles näites võib projektsiooni näitavad, et 30 päeva server on vaja täiendavaid mälu. See aitab teil mälu versioonitäienduse kavandamine serveri järgmise hoolduse aken, mis võivad tekkida kord kahe nädala jooksul.

>[AZURE.NOTE] Võimsuse juhtimise lahendust pole saadaval, lisatakse tööruumid. Kliendid, kellel on installitud võimsus halduse lahenduse saate jätkata lahendus.  

Kavandamise lahenduse võimsus tegeleb värskendamist teatatud käsitlema järgmisi kliendi probleemid:

- Nõue kasutada virtuaalse masina juhataja ja Toiminguhalduri
- Ei ole võimalik kohandada filter põhineb rühmad
- Kord tunnis andmete koondamine sagedased piisavalt
- Pole VM taseme ülevaateid
- Andmete usaldusväärsus

Uue võimsus lahenduse eelised:

- Täiustatud töökindlus ja täpselt Varundustöö andmete kogumine tugi
- VMM nõudmata Hyper-V tugi
- Klõpsake PowerBI mõõdikute visualiseerimine
- Teadmisi, VM taseme kasutamine


## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus
Kasutage järgmist teavet, installida ja konfigureerida lahendus.

- Toiminguhalduri jaoks on vaja võimsuse juhtimise lahendus.
- Virtuaalse masina Manager on vaja võimsuse juhtimise lahendus.
- Toimingute halduri ühendus virtuaalse masina Manager (VMM) on nõutav. Süsteemide ühenduse loomise kohta lisateabe saamiseks vaadake, [Kuidas ühendada VMM koos Toiminguhalduri](http://technet.microsoft.com/library/hh882396.aspx).
- Toiminguhalduri peab olema ühendatud Log Analytics.
- Lisage oma OMS tööruumi abil [lisada Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md)kirjeldatud protsessi võimsuse juhtimise lahendus.  On pole veel konfigureerimine vajalik.


## <a name="capacity-management-data-collection-details"></a>Võimsuse halduse andmete kogumise üksikasjad

Võimsuse halduse kogub jõudlusandmeid, metaandmete ja andmete kasutamise agentide, mida teil on lubatud.

Järgmises tabelis on andmete kogumise meetodid ja muud üksikasjad, kuidas koguda andmeid võimsus haldamise kohta.

| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Windows|![Ei](./media/log-analytics-capacity/oms-bullet-red.png)|![Jah](./media/log-analytics-capacity/oms-bullet-green.png)|![Ei](./media/log-analytics-capacity/oms-bullet-red.png)|            ![Jah](./media/log-analytics-capacity/oms-bullet-green.png)|![Jah](./media/log-analytics-capacity/oms-bullet-green.png)| tunni|

Järgmine tabel sisaldab andmetüüpide kogutud võimsus haldus.

|**Andmetüüp**|**Väljad**|
|---|---|
|Metaandmete|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, sordib, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-aadress, NetbiosDomainName, LogicalProcessors, dnsnameWindows, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Jõudluse|ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|Olek|StateChangeEventId, StateId, NewHealthState, OldHealthState, konteksti, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Võimsuse haldamise lehel


 Pärast võimsuse planeerimine lahendus on installitud, saate vaadata oma jälgida serverite võimsus abil paani **Võimsuse planeerimine** OMS lehel **Ülevaade** .

![Võimsuse planeerimine paani pilt](./media/log-analytics-capacity/oms-capacity01.png)

Paani avatakse **Võimsuse juhtimise** armatuurlaud, kus saate vaadata oma serveri võimsus Kokkuvõte. Lehel kuvatakse järgmised paanid, mille klõpsamisel saate:

- *Virtuaalse masina count*: kuvatakse ülejäänud virtuaalmasinates võimsus päevade arv
- *Arvutage*: kuvatakse protsessori ja saadaoleva mäluga
- *Salvestusruumi*: kuvatakse kettaruumi ja Keskmine ketta latentsus
- *Otsing*: andmete explorer, mille abil saate andmeid otsida OMS süsteemis

![Võimsus halduse armatuurlaua pilt](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>Võimsus lehe vaatamiseks

- Klõpsake lehe **Ülevaade** käsku **Võimsus haldus**, ja klõpsake **arvutada** või **salvestusruumi**.

## <a name="compute-page"></a>Arvuta leht

Saate **arvutada** armatuurlaua Microsoft Azure'i OMS võimsus hõivatuse prognoositud päeva, võimsuse ja tõhusust, mis on seotud teie kohta teabe kuvamiseks. Ala **kasutamise** abil saate vaadata oma virtuaalse masina hosts CPU core ja mälu kasutamine. Saate tööriista projektsiooni prognoosimiseks palju võimsus peaks olema antud kuupäevavahemiku jaoks saadaval. Abil saate vaadata, kuidas tõhusa oma virtuaalse masina hosts on **tõhusust** ala. Saate lingitud üksuste üksikasjade vaatamiseks klõpsata neid.

Saate luua Exceli töövihiku jaoks järgmised Kategooriad:

- Ülemise hosts kõrgeim core kasutamine
- Ülemise hosts kõrgeim mälu kasutamine
- Ülemise hosts ebaefektiivne virtuaalmasinates
- Ülemise hosts, kasutamine
- Alumine hosts, kasutamine

![lehe võimsus halduse arvutada pilt](./media/log-analytics-capacity/oms-capacity03.png)


**Arvutage** armatuurlaual kuvatakse järgmisi teemasid:

**Kasutamine**: vaade CPU core ja mälu kasutamine oma virtuaalse masina hosts.

- *Kasutatud tuuma*: summa kõik hosts (% kasutatud CPU korrutatuna füüsilise valdkond Host).
- *Tasuta südamikud*: tuuma miinus kasutatud tuuma kokku.
- *Protsendi tuuma saadaval*: vaba jagatuna koguarv füüsilise valdkond tuuma.
- *Virtuaalne tuumad VM kohta*: kokku virtuaalse tuuma süsteemi jagatuna koguarvu virtuaalmasinates süsteem.
- *Virtuaalne tuuma füüsilise tuuma-suhte*: suhe kokku füüsilise valdkond, et tuuma, mida kasutatakse virtuaalmasinates süsteem.
- *Arv, virtuaalse tuuma saadaval*: virtuaalse core tuuma suhe korrutatuna saadaval tuuma.
- *Kasutatud mälu*: mälu, mis on kasutanud kõik hosts summa.
- *Mälu*: kogu füüsiline mälu miinus kasutatud mälu.
- *Protsendi mälu saadaval*: vaba füüsilise mälu jagatuna kogu füüsiline mälu.
- *Virtuaalmälu VM kohta*: kokku virtuaalmälu süsteemi jagatuna koguarvu virtuaalmasinates süsteem.
- *Virtuaalmälu füüsilise mälu-suhte*: kokku virtuaalmälu süsteemi jagatuna kogu füüsilise mälu süsteemi.
- *Virtuaalmälu saadaval*: virtuaalmälu füüsilise mälu-suhte korrutatuna füüsilise mälu.

**Projektsiooni tööriist**

Projektsiooni tööriista abil saate vaadata oma ressursi kasutamine ajalooliste trende. See hõlmab kasutuse trendide virtuaalmasinates, mälu, core ja salvestusruumi. Projektsiooni võimalus kasutab projektsiooni algoritmi, mis aitavad teid, kui teil on piisavalt ressursid. See aitab teil õige võimsuse kavandamise, nii et saate teada, kui peate ostma rohkem võimsus (nt mälu, südamikud või salvestusruumi) arvutamiseks.

**Tõhusust**

- *Jõude VM*: väiksem kui 10% CPU ja 10% mälu kasutamise määratud aja jooksul.
- *Overutilized VM*: rohkem kui 90% CPU ja 90% mälu kasutamise määratud aja jooksul.
- *Host jõude*: väiksem kui 10% CPU ja 10% mälu kasutamise määratud aja jooksul.
- *Overutilized Host*: rohkem kui 90% CPU ja 90% mälu kasutamise määratud aja jooksul.

### <a name="to-work-with-items-on-the-compute-page"></a>Arvuta leht üksustega töötada

1. **Arvutage** armatuurlaual alal **kasutamine** võimsus teabe vaatamine protsessorituuma ja mälu.
2. Klõpsake üksuse avamiseks seda **lehel** ja saate vaadata üksikasjalikku teavet.
3. **Projektsiooni** tööriista, nihutage liugurit kuupäeva, projektsiooni kasutatakse kuupäeva valite võimsuse kuvamiseks.
4. Tehke alal **tõhusust** virtuaalmasinates ja virtuaalse masina hosts võimsus tõhusust teabe vaatamine

## <a name="direct-attached-storage-page"></a>Otse manustatud salvestusruumi lehekülje

Saate armatuurlaua **Otsekoheseks manustatud salvestusruumi** OMS võimsus plaanitud päeva ketta maht salvestusruumi kasutamise ja jõudluse kohta teabe kuvamiseks. Ala **kasutamise** abil saate vaadata oma virtuaalse masina hosts kettaruumi kasutus. **Jõudluse** ala abil saate vaadata ketta läbilaskevõime ja latentsusaeg oma virtuaalse masina hosts. Saate tööriista projektsiooni prognoosimiseks palju võimsus peaks olema antud kuupäevavahemiku jaoks saadaval. Saate lingitud üksuste üksikasjade vaatamiseks klõpsata neid.

Exceli töövihikus saate luua järgmiste kategooriate selle võimsus teabe põhjal:

- Host, top kettaruumi kasutus
- Ülemise keskmise latentsuse host järgi

**Salvestusruumi** lehel kuvatakse järgmisi teemasid:

- *Kasutamine*: kettaruumi kasutus oma virtuaalse masina hosts kuvada.
- *Kokku kettaruumi*: kõik pakkujate summa (loogika kettaruumi)
- *Kasutatud kettaruumi*: kõik pakkujate summa (kasutatud loogilise kettaruumi)
- *Saadaval kettaruumi*: kettaruumi miinus kasutatud kettaruumi kokku
- *Kasutusprotsenti ketas*: kasutatud kettaruumi jagatuna vaba kettaruumi
- *Protsendi ketta saadaval*: vaba kettaruumi jagatuna vaba kettaruumi

![Võimsus halduse otsese manustatud salvestusruumi lehe pilt](./media/log-analytics-capacity/oms-capacity04.png)

**Jõudluse parandamine**

OMS abil saate vaadata oma kettaruumi ajalooliste kasutus trendi. Võimalus projektsiooni kasutab algoritmi projekti tulevaste kasutamine. Ruumi kasutamine, võimalus projektsiooni võimaldab teil projekti käivitamisel võidakse piisavalt kettaruumi. See aitab teil proper mäluruumi kavandamine ja teada, kui vajate rohkem salvestusruumi ostmiseks.

**Projektsiooni tööriist**

Projektsiooni tööriista abil saate vaadata oma kettaruumi kasutamise ajalooliste trende. Projektsiooni võimalus saate ka projekti, kui teil on piisavalt vaba kõvakettaruumi. See aitab teil kavandamine proper võimsus ja teada, kui vajate rohkem salvestusruumi ostmiseks.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Töötada otse manustatud salvestusruumi lehel üksustega

1. Armatuurlaual **Otsese manustatud salvestusruumi** **kasutamise** ala, saate vaadata ketta kasutamine teabe.
2. Klõpsake nuppu lingitud üksus see **lehel** avada ja vaadata üksikasjalikku teavet.
3. **Jõudluse** ala, saate vaadata ketta läbilaskevõime ja latentsusaeg teavet.
4. **Projektsiooni tööriista**liuguri kuupäeva projektsiooni kasutatakse kuupäeva valite võimsuse kuvamiseks.


## <a name="next-steps"></a>Järgmised sammud

- [Log otsingud Log Analytics](log-analytics-log-searches.md) abil saate vaadata üksikasjalikku võimsus haldamine andmed.
