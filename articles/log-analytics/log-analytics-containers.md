<properties
    pageTitle="Ümbriste lahenduse Log Analytics | Microsoft Azure'i"
    description="Ümbriste lahendus Log Analytics abil saate vaadata ja hallata oma keskmise suurusega container hosts ühes kohas."
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



# <a name="containers-preview-solution-log-analytics"></a>Ümbriste (eelvaade) lahenduse Log Analytics

Selles artiklis kirjeldatakse, kuidas häälestada ja kasutada ümbriste lahenduse Log Analytics, mille abil saate vaadata ja hallata oma keskmise suurusega container hosts ühes kohas. Keskmise suurusega on ümbriste, mis automatiseerida tarkvara juurutamiseks oma IT-taristu loomiseks kasutatud tarkvara virtualization süsteem.

Lahenduse saate vaadata, millised ümbriste töötavad teie container hosts ja mida pildid töötab soovitud ümbriste. Saate vaadata nähtaval käskude kasutamist ümbriste auditilogi lisateabe. Ja saate otsida ümbriste vaadates ja otsimise tsentraliseeritud logid ilma kaugühenduse teel keskmise suurusega hosts kuvada. Ümbriste, mis võib olla lärmakas ja nõudvate liigse ressursid hosti otsimine Ja saate vaadata tsentraliseeritud CPU, mälu, salvestusruumi ja võrgu ümbriste kasutus- ja jõudluse kohta.

## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus

Kasutage järgmist teavet, installida ja konfigureerida lahendus.

Ümbriste lahendus lisada oma OMS tööruumi [lisada Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md)kirjeldatud protsessi abil.

On kaks võimalust installida ja kasutada keskmise suurusega OMS.

- Linux toetatud operatsioonisüsteemide, installida ja käitada keskmise suurusega ja seejärel installinud ja konfigureerinud OMS Agent Linux
- Klõpsake CoreOS, installida ja käitada keskmise suurusega ja konfigureerimist OMSAgent käivitamiseks on ümbrises

Vaadake üle oma container pakkuja [github](https://github.com/Microsoft/OMS-docker)toetatud keskmise suurusega ja Linux operatsioonisüsteemi versiooni.

>[AZURE.IMPORTANT] Keskmise suurusega töötava tuleb **enne** [OMS Agent Linuxi](log-analytics-linux-agents.md) installimine oma container hosts. Kui teil on juba installitud agent enne installimist keskmise suurusega, peate uuesti OMS Agent Linuxi jaoks. Keskmise suurusega kohta leiate lisateavet teemast [keskmise suurusega veebisaidi](https://www.docker.com).

Peate enne ümbriste saate jälgida oma container hosts konfigureerida järgmisi sätteid.

## <a name="configure-settings-for-the-linux-container-host"></a>Linux container hosti sätete konfigureerimine

Kui olete installinud keskmise suurusega, saate kasutada järgmisi sätteid oma container pakkuja konfigureerimiseks agent kasutamiseks koos keskmise suurusega. CoreOS ei toeta seda meetodit konfigureerimine.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>Container host - systemd sätete konfigureerimiseks (SUSE, openSUSE, CentOS 7.x RHEL 7.x ja Ubuntu 15.x ja uuem versioon)

1. Docker.service lisamiseks järgmist redigeerimiseks tehke järgmist.

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Lisage $DOCKER\_valib sisse &quot;ExecStart = / usr/Prügikast ja keskmise suurusega daemon&quot; docker.service failis. Järgmises näites abil.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Taaskäivitage keskmise suurusega. Näiteks:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>Container host - sätete konfigureerimiseks Upstart (Ubuntu 14.x)

1. Muuta /etc/default/docker ja lisada järgmist:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Salvestage fail ja seejärel taaskäivitage keskmise suurusega ja OMS teenuseid.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>Container host - Amazon Linux sätete konfigureerimine

1. Muuta /etc/sysconfig/docker ja lisada järgmist:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Salvestage fail ja seejärel taaskäivitage teenuse keskmise suurusega.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>CoreOS ümbriste sätete konfigureerimine

Kui olete installinud keskmise suurusega, saate kasutada järgmisi sätteid jaoks CoreOS käivitamiseks keskmise suurusega ja ümbris loomine. Saate kasutada mis tahes toetatud versiooni Linux – CoreOS, sh selle meetodi konfigureerimine. Peate oma [OMS tööruumi ID "ja" võti](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>Kõik ümbriste koos CoreOS OMS kasutamiseks

- Käivitage OMS-ümbrisest, mida soovite jälgida. Saate muuta ja kasutada järgmises näites.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Üleminek rakenduselt abil installitud agenti ühele nõus

Kui varem kasutanud agent otse installitud ja soovite selle asemel kasutada agent töötab ümbris, peate esmalt eemaldama OMSAgent. Lugege teemat [OMS Agent Linuxi installimise juhiseid](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Ümbriste andmete kogumise üksikasjad

Ümbriste lahendus kogub erinevate mõõdikute ja logige jõudlusandmeid container majutajate ja Linux, mida teil on lubatud OMS agentide kasutamise ümbriste ja OMSAgent ümbriste töötab.

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad, kuidas koguda andmeid ümbriste kohta.

| platvorm | OMS Agent Linuxi | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Linux|![Jah](./media/log-analytics-containers/oms-bullet-green.png)|![Ei](./media/log-analytics-containers/oms-bullet-red.png)|![Ei](./media/log-analytics-containers/oms-bullet-red.png)|            ![Ei](./media/log-analytics-containers/oms-bullet-red.png)|![Ei](./media/log-analytics-containers/oms-bullet-red.png)| iga 3 minuti järel|


Järgmises tabelis kuvatakse andmetüüpide kogutud ümbriste lahendus:

| Andmetüüp | Väljad |
| --- | --- |
| Jõudluse hosts ja ümbriste | Arvuti, ObjectName, CounterName ja #40; % protsessori aeg, ketas loeb MB, kettale kirjutab MB, mälu kasutus MB, võrgu vastuvõtmine baiti, võrku saata baiti protsessor kasutus sec, võrk ja #41; põhimõttelinegi, TimeGenerated, CounterPath, SourceSystem |
| Container varude | TimeGenerated, arvuti, ContainerHostname, pildi, ImageTag, ContinerState, ExitCode, EnvironmentVar, käsk, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID container nimi |
| Laoseisu Container pilt | Peatatud TimeGenerated, arvuti, Image, ImageTag, pildi mõõtmed, VirtualSize, töötab, peatatud, nurjus, SourceSystem, ImageID, TotalContainer |
| Container log | TimeGenerated, arvuti, pildi ID container nimi, LogEntrySource LogEntry, SourceSystem, ContainerID |
| Container teenuse Logi | TimeGenerated, arvuti, TimeOfCommand, pildi, käsk, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Ümbriste jälgimine

Pärast seda, kui teil on lubatud OMS portaalis lahendus, kuvatakse Kokkuvõttev teave teie container majutajate ja ümbriste, töötavad hosts nähtaval **ümbriste** paani.

![Ümbriste paan](./media/log-analytics-containers/containers-title.png)

Paanil kuvatakse ülevaade sellest, kui palju teil ümbriste keskkonna ja kas need on nurjunud, töötab või on peatatud.

### <a name="using-the-containers-dashboard"></a>Ümbriste armatuurlaua

Klõpsake paani **ümbriste** . Seal näete vaadete järgi:

- Container sündmused
- Tõrked
- Ümbriste olek
- Laoseisu Container pilt
- CPU ja mälu jõudlus

Iga armatuurlaua paanil on visuaali otsing, mida käitatakse kogutud andmete põhjal.

![Ümbriste armatuurlaud](./media/log-analytics-containers/containers-dash01.png)

![Ümbriste armatuurlaud](./media/log-analytics-containers/containers-dash02.png)

**Container olek** tera, klõpsake ülemisel alal, nagu allpool näidatud.

![Ümbriste olek](./media/log-analytics-containers/containers-status.png)

Logi otsing teave parajasti hosts ja need töötavad ümbriste kohta.

![Logige ümbriste otsimine](./media/log-analytics-containers/containers-log-search.png)

Siit saate redigeerida otsingupäringu muuta seda, et leida kindla teabe teile huvi. Log otsingute kohta leiate lisateavet teemast [Log otsingud Log Analytics](log-analytics-log-searches.md).

Näiteks saate muuta päringu nii, et see näitab kõigi peatatud ümbriste töötava ümbriste asemel muuta **töötab** **peatamiseni** otsingupäringu.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Tõrkeotsing otsimine nurjunud container järgi

Kui see on null välju koodiga väljunud märgib OMS ümbris on **nurjunud** . Saate vaadata tõrgete ja vigade **Nurjus ümbriste** tera keskkonna ülevaade.

### <a name="to-find-failed-containers"></a>Kui soovite otsimine nurjunud ümbriste

1. Klõpsake **Container sündmuste** tera.  
  ![ümbriste sündmused](./media/log-analytics-containers/containers-events.png)
2. Logi otsing parajasti oleku ümbriste sarnaneb järgmisega.  
  ![ümbriste olek](./media/log-analytics-containers/containers-container-state.png)
3. Seejärel klõpsake lisateabe saamiseks, nt pildi suurus ja arv peatatud ja nurjunud piltide nurjunud väärtus. Laiendage **Kuva rohkem** kuva pilt ID.  
  ![nurjunud ümbriste](./media/log-analytics-containers/containers-state-failed.png)
4. Järgmiseks otsimine ümbris, kus töötab see pilt. Tippige otsinguväljale päring järgmist.
  `Type=ContainerInventory <ImageID>`Kuvatakse logid. Leidke nurjunud container kuvamiseks.  
  ![nurjunud ümbriste](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Container andmete otsing logid

Kindlate tõrkeotsingul see aitab see esinemise keskkonna kuvamiseks. Järgmist tüüpi logi abil saate luua päringute tagastamiseks teavet soovite.

- **ContainerInventory** – kasutage seda tüüpi, kui soovite teavet container asukoht, millised on nende nimed ja mis pilte nad töötab.
- **ContainerImageInventory** – kasutage seda tüüpi teabe leidmiseks püüdes korraldatud pilt ja pildi teabe nt pildi-ID-d ja suurused.
- **ContainerLog** – kasutage seda tüüpi, kui soovite leida kindla tõrke Logiteave ja kirjed.
- **ContainerServiceLog** – kasutage seda tüüpi, kui proovite auditilogi rada leiate keskmise suurusega deemon, nt start, Peata, kustutamine või pull käsud.

### <a name="to-search-logs-for-container-data"></a>Logide container andmete otsimiseks

- Valige pilt, mille te teate, et ei ole viimati ja selle Tõrkelogide otsimine. Alustuseks leida container nime, mis töötab selle pildi **ContainerInventory** otsing. Näiteks otsimine`Type=ContainerInventory ubuntu Failed`  
    ![Ubuntu ümbriste otsimine](./media/log-analytics-containers/search-ubuntu.png)

  Container **nime**kõrval nimi ja otsida neid logid. Selles näites on `Type=ContainerLog adoring_meitner`.

**Jõudluse teabe vaatamine**

Kui hakkate ehitada päringud, aitab see kuvamiseks, mis on võimalik esmalt. Näiteks kõigi tulemustega seotud andmete kuvamiseks proovige laialdane päringu tippides Otsi järgmine päring.

```
Type=Perf
```

![ümbriste jõudlus](./media/log-analytics-containers/containers-perf01.png)



Näete seda veel graafilisel kujul sõna **mõõdikute** tulemuste klõpsamisel.

![ümbriste jõudlus](./media/log-analytics-containers/containers-perf02.png)



Saate ulatus jõudlusandmeid, te näete teatud container, tippides selle paremal pool oma päringu nimi.

```
Type=Perf <containerName>
```

Mis näitab jõudluse mõõdikud kogutakse ka üksikute container loendit.

![ümbriste jõudlus](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Näide log otsingupäringuid

Sageli on kasulik koostada päringute alustades näide või kahepoolse ja seejärel muuta neid vastavalt teie keskkonnas. Lähtepunktina, võite proovida **Märkimisväärset päringute** tera abil saate koostada täpsemaid päringuid.

![Ümbriste päringud](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Log otsingupäringuid salvestamine

Päringute salvestamine on standardne funktsioon Log Analytics. Nende salvestamine, peate need, mida olete leidnud kasulik mugav edaspidiseks kasutamiseks.

Kui loote päringu, mis teile kasulik, salvestada, klõpsates nuppu **Lemmikud** Log lehe ülaosas. Seejärel pääsete hõlpsasti seda hiljem lehelt **Minu armatuurlaud** .

## <a name="next-steps"></a>Järgmised sammud

- [Otsingu logid](log-analytics-log-searches.md) container üksikasjalikud andmed kirjete vaatamiseks.
