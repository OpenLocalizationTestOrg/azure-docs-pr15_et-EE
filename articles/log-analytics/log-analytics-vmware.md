<properties
    pageTitle="Log Analytics VMware jälgimise lahendus | Microsoft Azure'i"
    description="Siit saate teada, kuidas VMware jälgimise lahendus aitab hallata logid ja jälgida ESXi hosts kohta."
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
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Log Analytics lahenduse VMware jälgimine (eelvaade)

Log Analytics VMware jälgimise lahendus on lahenduse, mis aitab teil luua tsentraliseeritud logimine ja jälgimisega seotud lähenemine suurte VMware logide. Selles artiklis kirjeldatakse, kuidas saate tõrkeotsing, jäädvustada ja hallata ühes kohas kasutamise lahendus ESXi hosts. Lahendus, näete üksikasjalikud andmed kõigi oma ESXi hosts ühes kohas. Saate vaadata ülemise sündmuse loendab, oleku ja trendide VM ja ESXi hosts ESXi host logid kaudu. Saate otsida, vaatamine ja tsentraliseeritud ESXi host logid otsimine. Ja te saate luua Logi otsingupäringuid põhinevad teatised.

Lahendus kasutab kohalikke Logi funktsionaalsus ESXi hosti suunata VM, mis on OMS Agent tõuketeatised andmetega. Lahendus ei kirjuta failid sees target VM Logi sisse. OMS agent avab pordi 1514 ja see on kuulata. Kui selle saab andmeid, sunnib OMS agent andmed OMS.

## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus

Kasutage järgmist teavet, installida ja konfigureerida lahendus.

- Lisada oma OMS tööruumi abil [lisada Log Analytics](log-analytics-add-solutions.md)Solutions lahendusegaleriist kirjeldatud VMware jälgimise lahendus.

#### <a name="supported-vmware-esxi-hosts"></a>Toetatud VMware ESXi hosts
vSphere ESXi Host 5,5 ja 6.0

#### <a name="prepare-a-linux-server"></a>Ettevalmistused Linux server
Looge Linux operatsioonisüsteemi saada kõik Logi andmed ESXi hosts VM. [OMS Linux Agent](log-analytics-linux-agents.md) on kõik ESXi host Logi andmete kogumine. Mitme ESXi hosts abil saate edastada logid ühe Linux server, nagu järgmises näites.  

   ![Logi meilivoo](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Logi saidikogumi konfigureerimine

1. Logi ümbersuunamine VSphere jaoks häälestada. Üksikasjalikku teavet Logi edasisaatmist häälestada, lugege teemat [konfigureerimise Logi ESXi 5.x ja 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Minge **ESXi Hostikonfiguratsioon** > **tarkvara** > **täpsemate sätete** > **Logi**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. Lisage väljale *Syslog.global.logHost* Linux serveri ja pordi number *1514*. Näiteks `tcp://hostname:1514` või`tcp://123.456.789.101:1514`

3. Avage ESXi host tulemüüri Logi. **ESXi Hostikonfiguratsioon** > **tarkvara** > **Turbeprofiili** > **tulemüür** ja dialoogiboksi **Atribuudid**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Märkige ruut vSphere konsooli kinnitamaks, et Logi õigesti seadistatud. Kinnitage ESXI hosti pordinumber **1514** on konfigureeritud.

5. Testida Ühenduvus Linux server ja ESXi host, kasutades funktsiooni `nc` hosti ESXi käsk. Näiteks:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Laadige alla ja installige OMS Agent Linux server Linux. Lisateabe saamiseks vaadake [OMS Agent Linuxi dokumentatsioonist](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. Pärast OMS Agent Linuxi on installitud, minge /etc/opt/microsoft/omsagent/sysconf/omsagent.d kataloogi ja kopeerige fail vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d kataloogi ja muudatus omanik/rühmitamine ja õigused faili. Näiteks:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Uuesti OMS Agent Linuxi käivitades `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. OMS portaalis, saate otsida log `Type=VMware_CL`. Kui OMS kogub Logi andmeid, säilitab Logi vorming. Portaalis on mõne kindla välja nagu *Hostname* ja *ProcessName*jäädvustatud.  

    ![tüüp](./media/log-analytics-vmware/type.png)  

    Kui teie otsingutulemuste vaatamine log sarnanevad ülalolevat pilti, on korras kasutada OMS VMware jälgimise lahendus armatuurlaua.  

## <a name="vmware-data-collection-details"></a>VMware andmete kogumise üksikasjad

VMware jälgimise lahendus kogub erinevate mõõdikute ja logige jõudlusandmeid ESXi hosts OMS agentide kasutamise Linux, mida teil on lubatud.

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas koguda andmeid.

| platvorm | OMS Agent Linuxi | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Linux|![Jah](./media/log-analytics-vmware/oms-bullet-green.png)|![Ei](./media/log-analytics-vmware/oms-bullet-red.png)|![Ei](./media/log-analytics-vmware/oms-bullet-red.png)|            ![Ei](./media/log-analytics-containers/oms-bullet-red.png)|![Ei](./media/log-analytics-vmware/oms-bullet-red.png)| iga 3 minuti järel|


Järgmises tabelis toodud näiteid VMware jälgimise lahendus kogutud andmete väljad:

| välja nimi | kirjeldus |
| --- | --- |
| Device_s| VMware salvestusseadmed |
| ESXIFailure_s | tõrge tüübid |
| EventTime_t | sündmuse toimumise aeg |
| HostName_s | ESXi hosti nimi |
| Operation_s | VM loomine või kustutamine VM |
| ProcessName_s | sündmuse nimi |
| ResourceId_s | VMware hosti nimi |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Hyper-V |
| SCSIStatus_s | VMware SCSI olek |
| SyslogMessage_s | Logi andmed |
| UserName_s | kasutaja loodud või kustutatud VM |
| VMName_s | VM nimi |
| Arvuti | arvuti |
| TimeGenerated | andmed on loodud aeg |
| DataCenter_s | VMware andmekeskusega |
| StorageLatency_s | salvestusruumi latentsus ([ms) |

## <a name="vmware-monitoring-solution-overview"></a>VMware jälgimise lahenduse ülevaade

Paani VMware kuvatakse OMS portaalis. Pakub ületaseme vaadet mis tahes ebaõnnestumist. Kui olete klõpsanud, te lähete armatuurlaua vaadet.

![paan](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Liikuge armatuurlaua kuvamine

Vaates **VMware** armatuurlaua labad on korraldatud:

- Tõrge oleku loendamine
- Loendab ülemise Host sündmus järgi
- Ülemise sündmuste arv
- Virtuaalne arvuti tegevusi.
- ESXi Host ketta sündmused


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Klõpsake mis tahes blade kuvatakse üksikasjalik teave tera jaoks teatud Log Analytics otsingupaani avamiseks.

Siit saate redigeerida otsingupäringu teatud midagi muuta. Õpetuse OMS otsingu põhitõdesid, vaadake selle [OMS log otsingu õpetuse.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Otsige ESXi host sündmused

Ühe ESXi host loob mitme logid, nende põhjal. VMware jälgimise lahendus centralizes neid ja Kokkuvõte sündmuste arv. See keskse vaade aitab teil mõista, millised ESXi host on suure hulga sündmused ja mis ilmnevad kõige sagedamini teie keskkonnas.

![sündmuse](./media/log-analytics-vmware/events.png)

Saate minna edasise, klõpsates soovitud ESXi hosti või mõne sündmuse tüüp.

Kui klõpsate mõnda ESXi hosti nimi, saate vaadata teavet majutavate ESXi. Kui soovite piirata tulemusi sündmuse tüüp, lisage `“ProcessName_s=EVENT TYPE”` otsingusse. Saate valida **ProcessName** otsingu filter. Mis ahendab teave teie jaoks.

![süvitsiminek](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Kõrge VM tegevuste otsimine

Virtuaalse masina saab luua ja mis tahes ESXi hosti kustutatud. See on abiks administraator on ESXi host loob mitu VMs tuvastamiseks. Selle sisse-omakorda aitab mõista jõudlus ja võimsus kavandamine. Hoida silma peal VM tegevuse sündmused on oluline, kui keskkonna haldamise.

![süvitsiminek](./media/log-analytics-vmware/vmactivities1.png)

Kui soovite täiendavaid ESXi hosti VM loomine andmete kuvamiseks, klõpsake mõnda ESXi hosti nimi.

![süvitsiminek](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Levinud otsingupäringuid

Lahendus sisaldab muud kasulikud päringud, mis aitavad teil hallata oma ESXi hosts, nt kõrge salvestusruumi, salvestusruumi latentsus ja tee.

![Päringud](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Päringute salvestamine

Salvestamine otsingupäringuid on standardne funktsioon OMS ja aitavad teil küsimusi, mida olete leidnud kasulik. Kui loote päringu, mis teile kasulik, salvestada, klõpsates nuppu **Lemmikud**. Salvestatud päringu abil saate hõlpsalt eri seda hiljem lehelt [Minu armatuurlaud](log-analytics-dashboards.md) , kus saate luua oma kohandatud armatuurlaudade.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Päringute teatiste loomine

Pärast seda, kui olete loonud oma päringuid, mida soovite päringute abil märku, kui teatud sündmused. Kohta leiate teavet [teatiste Log Analytics](log-analytics-alerts.md) teatiste loomise kohta. Näiteid on päringud ja muude päringu näited leiate [kuvari VMware abil OMS Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) ajaveebipostitusest.

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Mida on vaja soovitud ESXi majutada säte? Milline mõju on sellel minu praeguse keskkond?
Lahendus kasutab kohalikke ESXi Host Logi edastamise süsteem. Te ei pea täiendavat Microsofti tarkvara ESXi hosti jäädvustada logid. See peaks olema väike mõju oma olemasoleva keskkonnas. Soovite siiski seada Logi ümbersuunamine, mis on ESXI funktsionaalsus.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Vaja uuesti minu ESXi host?
Ei. Selle protsessi vaja uuesti. Mõnikord vSphere ei värskendata õigesti selle logi. Sellisel juhul ESXi hosti sisse logida ja selle logi uuesti. Uuesti, pole teil taaskäivitage host, et seda toimingut pole teie keskkonnas segab.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Saate suurendada või vähendada logiandmed OMS saadetud?
Jah saate. Saate ESXi Host Logi tase sätted vSphere. Logi saidikogumi põhineb *teave* tase. Kui soovite auditeeritavad VM loomine või kustutamine, peate hoida *teave* Hostd kohta. Lisateavet leiate teemast [VMware teabebaasi](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Miks Hostd pole pakub andmete OMS? Minu log säte on teave.
Ilmnes Logi ajatempli ka ESXi host vea. Lisateavet leiate teemast [VMware teabebaasi](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Pärast lahendus, tuleks funktsioon Hostd tavalisel viisil.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Kas mul on mitu ESXi hosts Logi andmete edastamiseks ühe VM koos omsagent?
Jah. Teil võib olla mitu ESXi hosts ühe VM koos omsagent edasi.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Miks ma ei näe andmed üheks OMS liiguvad?

Võib olla mitu põhjust:

- Hosti ESXi on pole õigesti nõudnud andmete töötab omsagent VM. Testimiseks tehke järgmist.
    1. Kontrollimaks, kasutades ssh ESXi Host sisse logida ja käivitage järgmine käsk:`nc -z ipaddressofVM 1514`

        Kui see ei õnnestu, on tõenäoline vSphere sätted täiustatud häälestust ei paranda. Leiate jaotisest teavet selle kohta, kuidas häälestada ESXi host Logi edastamiseks [konfigureerimine Logi saidikogumi](#configure-syslog-collection) .

    2. Kui Logi port Ühenduvus õnnestumise, kuid andmeid pole ikka näha, siis uuesti Logi ESXi hosti abil ssh käivitage järgmine käsk:` esxcli system syslog reload`

- OMS agent VM pole õigesti häälestatud. Testimiseks tehke järgmist.
    1. OMS jälgib pordi 1514 ja sunnib andmed üheks OMS. Veenduge, et see on avatud, käivitage järgmine käsk:`netstat -a | grep 1514`
    2. Peaksite nägema pordi `1514/tcp` avamine. Kui te ei saa, veenduge, et selle omsagent on õigesti installitud. Kui te ei näe pordi teavet, siis pole Logi pordi avatud VM.
        1. Veenduge, et OMS Agent töötab, kasutades `ps -ef | grep oms`. Kui see ei tööta, alustage protsessi käsu` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Avage soovitud `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` faili.

            Kontrollige, kas õige kasutaja ja rühma säte on lubatud, sarnaselt:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Kui faili pole olemas või kasutaja ja rühma säte on vale, võtta meetmed valmistada [Linux server](#prepare-a-linux-server).

## <a name="next-steps"></a>Järgmised sammud

- Kasutage [Log otsingud](log-analytics-log-searches.md) Log Analytics üksikasjalik VMware hosti andmete kuvamiseks.
- [Oma armatuurlaudade loomine](log-analytics-dashboards.md) VMware hosti andmete nähtaval.
- [Teatiste loomine](log-analytics-alerts.md) kui teatud VMware hosti sündmused.
