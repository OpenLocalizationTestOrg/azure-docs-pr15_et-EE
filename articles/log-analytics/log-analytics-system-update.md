<properties
    pageTitle="Süsteemi värskendamine Assessment lahenduse Log Analytics | Microsoft Azure'i"
    description="Saate süsteemi uuendused lahenduse Log Analytics aitavad teil puuduvad värskenduste rakendamine oma taristu serverites."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>System Update Assessment lahendus Log Analytics

Saate süsteemi uuendused lahenduse Log Analytics aitavad teil puuduvad värskenduste rakendamine oma taristu serverites. Pärast installimist lahenduse, saate vaadata värskendusi, mis on puudu jälgida serverite abil paani **Update hindamise** OMS lehel **Ülevaade** .

Puuduvate värskenduste leidmisel kuvatakse üksikasjad armatuurlaual **värskendused** . **Värskenduste** armatuurlaua abil saate töötada puuduvad värskendused ja arendada plaani rakendamiseks serverid, kus see on vajalik.

## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus
Kasutage järgmist teavet, installida ja konfigureerida lahendus.

- Lisage oma OMS tööruumi abil [lisada Log Analytics](log-analytics-add-solutions.md)Solutions lahendusegaleriist kirjeldatud värskenduse hindamise lahendus.  On pole veel konfigureerimine vajalik.

## <a name="system-update-data-collection-details"></a>Süsteemi värskendamine andmete kogumise üksikasjad

Värskenduse hindamise kogub metaandmete ja state andmete agentide, mida teil on lubatud.

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas koguda andmeid Update hindamise.

| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Windows|![Jah](./media/log-analytics-system-update/oms-bullet-green.png)|![Jah](./media/log-analytics-system-update/oms-bullet-green.png)|![Ei](./media/log-analytics-system-update/oms-bullet-red.png)|            ![Ei](./media/log-analytics-system-update/oms-bullet-red.png)|![Jah](./media/log-analytics-system-update/oms-bullet-green.png)| Vähemalt 2 korda päevas ja 15 minutit pärast värskenduse installimist|

Järgmine tabel sisaldab kogutud Update hindamise andmetüüpide.

|**Andmetüüp**|**Väljad**|
|---|---|
|Metaandmete|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, sordib, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-aadress, NetbiosDomainName, LogicalProcessors, dnsnameWindows, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Olek|StateChangeEventId, StateId, NewHealthState, OldHealthState, konteksti, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Värskenduste töötamiseks

1. Klõpsake lehe **Ülevaade** **Update hindamise** paani.  
    ![Süsteemi värskenduse Assessment paan](./media/log-analytics-system-update/sys-update-tile.png)
2. Värskenduse kategooriate kuvamiseks armatuurlaual **värskendused** .  
    ![Värskenduste armatuurlaud](./media/log-analytics-system-update/sys-updates02.png)
3. Leidke lehe vaatamiseks **Kriitiline Windowsi turbevärskendusi** tera ja klõpsake jaotises **liigitamine**, **Turbevärskendusi**.  
    ![Turbevärskendusi](./media/log-analytics-system-update/sys-updates03.png)
4. Logige lehel, kuvatakse erinevaid andmeid turbevärskendusi, mis leiti puuduvad serverid oma taristu kohta. Klõpsake **loendi** värskenduste kohta täpsema teabe kuvamiseks.  
    ![Otsingu tulemused - loend](./media/log-analytics-system-update/sys-updates04.png)
5. Logige lehel, kuvatakse üksikasjalikku teavet iga värskenduse. KBID kõrval, klõpsake menüüd **Vaade** vastava artikli vaatamiseks Microsofti tugiteenuste veebisait.  
    ![Logige otsingutulemite - vaade KB](./media/log-analytics-system-update/sys-updates05.png)
6. Veebibrauseris avatakse uuel vahekaardil Microsofti tugiteenuste veebilehte Värskenda. Saate vaadata teavet värskenduse, mis on puudu.  
    ![Microsoft Support veebileht](./media/log-analytics-system-update/sys-updates06.png)
7. Kasutatakse teabe leidmist, saate luua plaani käsitsi värskenduse puudu või jätkamiseks pärast värskenduse automaatselt ülejäänud juhised.
8. Kui soovite automaatselt värskenduse puudu, minge tagasi **värskenduste** armatuurlaud ja seejärel **Värskendamine käivitatakse**, klõpsake **käsku plaanimiseks värskendust käivitada**.  
    ![Värskenduse jookseb - ajastatud menüü](./media/log-analytics-system-update/sys-updates07.png)
9. Menüü **ajastatud** **Värskendamine käivitatakse** lehel nuppu **Lisa** uus update Käivita loomiseks.  
    ![Ajastatud vahekaardil – lisamine](./media/log-analytics-system-update/sys-updates08.png)
10. **Käivitage uus värskendada** lehe värskenduse nimi Käivita, tippige lisamine arvutite või arvuti rühmad, määratleda ajakava ja klõpsake siis nuppu **Salvesta**.  
    ![Uue Update'i käivitamine](./media/log-analytics-system-update/sys-updates09.png)
11. **Ajastatud** menüü Uus värskendus käivitada, et olete ajastatud **Värskendamine käivitatakse** lehel kuvatakse.  
    ![Ajastatud menüü](./media/log-analytics-system-update/sys-updates10.png)
12. Käivitage värskenduse käivitamisel kuvatakse teavet selle kohta, menüü **töötab** .  
    ![Töötava menüü](./media/log-analytics-system-update/sys-updates11.png)
13. Pärast värskenduse käivitamine on lõpule jõudnud, kuvatakse menüü **lõppenud** olek.
14. Kui värskendused on rakendatud läbiviimisel **Kriitiline Windowsi turbevärskendusi** labale Update'ilt näete, et värskenduste arv väheneb.  
    ![Kriitiline Windowsi turbevärskendusi blade - update count vähendatud](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Järgmised sammud

- [Otsingu logid](log-analytics-log-searches.md) vaadata üksikasjalikku süsteemi andmete värskendamiseks.
