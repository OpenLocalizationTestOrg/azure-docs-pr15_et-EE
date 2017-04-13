<properties
    pageTitle="Skript toimingu arendamiseni Linuxi-põhiste Hdinsightiga | Microsoft Azure'i"
    description="Kuidas kohandada Linux-põhine Hdinsightiga kogumite skripti toiminguga. Skripti toimingud on võimalus kohandada Windows Azure Hdinsightiga kogumite täpsustades kobar konfiguratsioonisätted või klaster lisateenuse, tööriistad või muu tarkvara installimine. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Skripti toimingu arendamiseni Hdinsightiga

Skripti toimingud on võimalus kohandada Windows Azure Hdinsightiga kogumite täpsustades kobar konfiguratsioonisätted või klaster lisateenuse, tööriistad või muu tarkvara installimine. Saate skripti toimingud või töötava kobar kobar loomise ajal.

> [AZURE.NOTE] Teave selles dokumendis on Linux-põhine Hdinsightiga kogumite. Kasutades Windowsi-põhiste kogumite skripti toimingud leiate teemast [skripti toimingu arendamiseni Hdinsightiga (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Mis on skripti tegevused?

Skripti toimingud on Bash skripte, mis käivitatakse Azure'i kobar sõlmed konfiguratsiooni muudatuste tegemine või tarkvara installimine. Skripti toiming käivitatakse root ja pakub täielikud juurdepääsuõigused kobar sõlmi.

Skripti toiminguid saab rakendada kaudu järgmistest viisidest:

| Kasutage seda rakendada skripti... | Ajal klaster loomine... | Töötava klaster... |
| ----- |:-----:|:-----:|
| Azure'i portaal | ✓ | ✓ |
| Azure'i PowerShelli | ✓ | ✓ |
| Azure'i CLI | &nbsp; | ✓ |
| Hdinsightiga .NET SDK | ✓ | ✓ |
| Azure'i ressursihaldur Mall | ✓ | &nbsp; |

Nende meetodite abil skripti toimingute kohta leiate lisateavet teemast [kohandamine Hdinsightiga kogumite skripti toimingute kasutamine](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Skripti arengu head tavad

Kui teil tekib mõne Hdinsightiga kobar kohandatud skript, on mitu head tavad meeles pidada.

- [Sihtrakenduse Hadoopi versioon](#bPS1)
- [Sihtrakenduse Opsüsteemi versioon](#bps10)
- [Skripti ressursid ühed linke](#bPS2)
- [Kasutage eelnevalt kompileeritud ressursid](#bPS4)
- [Veenduge, et kobar kohandamine script on idempotent](#bPS3)
- [Tagada kõrge kobar arhitektuur](#bPS5)
- [Azure'i bloobimälu kasutada kohandatud komponentide konfigureerimine](#bPS6)
- [Kirjutage teabe STDOUT ja STDERR](#bPS7)
- [Failide salvestamine ASCII LF rea lõpud](#bps8)
- [Proovi uuesti loogika abil saate taastada siirdamiseks vigu](#bps9)

> [AZURE.IMPORTANT] Skripti toimingud peate täitma 60 minutit või need kuvatakse ajalõpp. Sõlm ettevalmistamise, script on alguspäeva samaaegselt muude häälestamise ja konfigureerimise protsessid. Ressursid, nt CPU kellaaja või võrgu läbilaskevõime konkurentsiga võib põhjustada skripti võtta enam lõpetada, kui teie arenduskeskkond.

### <a name="bPS1"></a>Sihtrakenduse Hadoopi versioon

Erinevate versioonide Hdinsightiga on erinevaid versioone Hadoopi teenuste ja komponentide installitud. Kui teie script eeldab, teenuse või osa konkreetse versiooni, saab kasutada ainult skripti Hdinsightiga, mis sisaldab nõutavad komponendid versiooniga. Leiate komponent versioonid kaasas Hdinsightiga [Hdinsightiga komponendi Versioonimine](hdinsight-component-versioning.md) dokumendi abil.

###<a name="bps10"></a>Suunata Opsüsteemi versioon

Linux-põhine Hdinsightiga põhineb Ubuntu Linux jaotuse. Erinevate versioonide Hdinsightiga toetuvad Ubuntu, mis võivad esineda oma skripti käitumise erinevaid versioone. Näiteks põhinevad Ubuntu versioonid, mis kasutavad Upstart Hdinsightiga 3.4 ja varasemad versioonid. Versioon 3.5 põhineb Ubuntu 16.04, mis kasutab Systemd. Systemd ja Upstart toetuvad erinevad käsud, seega tuleb kirjutada oma skripti töötada nii.

Teine oluline erinevus Hdinsightiga 3.4 ja 3.5 on see, et `JAVA_HOME` nüüd osutab Java 8.

Opsüsteemi versioon saate kontrollida, kasutades `lsb_release`. Järgmised Koodilõigud tooni installi skripti näitab, kuidas kindlaks teha, kui skript töötab Ubuntu 14 või 16:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

Täielik skripti, mis sisaldab järgmisi Koodilõigud veebisaidil https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh otsimine

Ubuntu, mis kasutavad Hdinsightiga versiooni jaoks [Hdinsightiga komponent version](hdinsight-component-versioning.md) dokumenti vaadata.

Systemd ja Upstart erinevused, vt [Systemd Upstart kasutajate jaoks](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Skripti ressursid ühed linke

Veenduge, skripte ja ressursse, mis kasutavad skripti jäävad klaster eluea ja, et need failid versioone ei muuda kestel. Need ressursid on nõutav, kui uusi sõlmi lisatakse klaster ajal skaleerimist toimingud.

Parim on alla laadida ja arhiivida kõik teie tellimuse Azure Storage konto.

> [AZURE.IMPORTANT] Salvestusruumi konto, mida kasutatakse peab olema vaikimisi salvestusruumi konto klaster või avalik, kirjutuskaitstud container storage konto.

Näiteks Microsofti pakutavate on talletatud [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) salvestusruumi konto, mis on avalik, kirjutuskaitstud ümbris säilitada Hdinsightiga meeskonna.

### <a name="bPS4"></a>Kasutage eelnevalt kompileeritud ressursid

Vähendada aega kulub skripti, et vältida toimingute kohta, mida koostada ressursside kaudu lähtekoodi. Selle asemel eelnevalt kompileerida ressursside ja Azure'i bloobimälu kahendarvu versiooni salvestada, et seda saab kiiresti alla klaster oma käsikirjaga.

### <a name="bPS3"></a>Veenduge, et kobar kohandamine script on idempotent

Skriptide peab mõeldud idempotent, et kui script on parandusfunktsiooni mitu korda, see peaks tagama, et iga kord, kui see on oli tagastatakse klaster muutmata.

Näiteks kui kohandatud skript installib rakenduse juures /usr/local/bin esimese käivitada, siis iga järgmise jooksma skripti tuleks kontrollida, kas rakendus on juba olemas /usr/local/bin asukoht enne jätkamist muud etappide skripti.

### <a name="bPS5"></a>Tagada kõrge kobar arhitektuur

Linux-põhine Hdinsightiga kogumite pakub kahte pea sõlme, mis on aktiivne klaster ja toimingud on skripti parandusfunktsiooni nii sõlmed. Kui installite komponendid ainult üks pea sõlme, peate skripti, mis kuvatakse ainult installida komponent üks kahest pea sõlmed klaster kujundama.

> [AZURE.IMPORTANT] Vaikimisi installitakse Hdinsightiga osana on loodud nurjumise üle kahe pea sõlme vahel vastavalt vajadusele, aga seda funktsiooni ei laiendada kohandatud skript määral kaudu installitud komponendid. Kui teil on vaja skripti toimingu tugevalt saadaval kaudu installitud komponendid, peate oma Tõrkesiirde süsteem, mis kasutab kahte saadaval pea sõlme rakendama.

### <a name="bPS6"></a>Azure'i bloobimälu kasutada kohandatud komponentide konfigureerimine

Komponendid, mille saate installida klaster võib olla vaikimisi konfiguratsiooni, mis kasutab Hadoopi jaotatud faili süsteemi (HDFS) salvestusruumi. Hdinsightiga kasutab vaikimisi salvestusruumi Azure'i bloobimälu salvestusruumi (WASB). See pakub HDFS ühilduvad süsteemi, mida püsib andmete isegi juhul, kui klaster kustutatakse. Peaksite konfigureerima installimist kasutada WASB asemel HDFS komponendid.

Näiteks järgmine kopeerib giraph-examples.jar faili kohaliku faili süsteemist WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Kirjutage teabe STDOUT ja STDERR

Skripti käitamise ajal STDOUT ja STDERR kirjutatud teave logitakse ja saab vaadata Ambari web UI.

> [AZURE.NOTE] Ambari kuvatakse üksnes juhul, kui klaster on loodud. Kui kasutate skripti toimingu kobar loomise ajal ja loomine nurjub, vt tõrkeotsingu [kohandamine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) muid võimalusi, juurdepääsu andmeid.

Enamik Utiliidid ja install pakette juba kirjutab teavet STDOUT ja STDERR, aga soovite lisada täiendavad logimine. Teksti saatmiseks STDOUT kasutamine `echo`. Näiteks:

        echo "Getting ready to install Foo"

Vaikimisi `echo` stringi saata STDOUT. Suunata STDERR, lisage `>&2` enne `echo`. Näiteks:

        >&2 echo "An error occurred installing Foo"

See teave saadetakse STDOUT (1, mis on vaikimisi, seega pole siin loetletud,) suunab STDERR (2). IO ümbersuunamise kohta leiate lisateavet teemast [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Sisse logitud skripti toimingud teabe vaatamise kohta leiate lisateavet teemast [kohandamine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>Failide salvestamine ASCII LF rea lõpud

ASCII-vormingus, hoida bash skripte joonega LF lõpetada. Kui failid UTF-8, mis võivad hõlmata bait tellimuse märgi faili või rea lõpud on CRLF, mis on levinud Windows redaktorid, alguses siis skripti nurjub vigadega järgmine:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Proovi uuesti loogika abil saate taastada siirdamiseks vigu

Kui allalaadimise, installimise paketid kasutades apt-get või muid toiminguid, mis andmete edastamiseks Interneti-failid ei pruugi toiming siirdamiseks võrgu tõrgete tõttu. Näiteks võib protsessi probleemse varukoopia sõlme suhtlete remote ressurss.

Tegemiseks oma skripti olles siirdamiseks tõrkeid, saate rakendada uuesti loogika. Järgmises näites on funktsioon, mis kestab käsk selle edasi ja proovige (kui käsk nurjub,) kuni kolm korda. Siis ootama proovi uuesti vahele kaks sekundit.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Järgnevalt on selle funktsiooni kasutamise näited.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Kohandatud skriptid Helper meetodid

skripti toimingu helper meetodid on Utiliidid, mida saate kasutada kohandatud skriptide kirjutamise ajal. Need on määratletud [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)ja saab lisada oma skriptide abil järgmist:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

See teeb järgmist abilised kasutada oma skripti:

| Helper kasutus | Kirjeldus |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | Laadib faili andmeallika URL-i määratud failitee. Vaikimisi pole kirjutatakse olemasolev fail. |
| `untar_file TARFILE DESTDIR` | Ekstraktib tar faili (kasutades `-xf`,) soovitud sihtkausta. |
| `test_is_headnode` | Kui parandusfunktsiooni kobar pea sõlme tagastab 1; muidu 0. |
| `test_is_datanode` | Kui praegune sõlm on sõlm andmete (töötaja), tagastab väärtuse 1; muidu 0. |
| `test_is_first_datanode` | Kui praegusest on esimene andmete (töötaja) sõlm (nimega workernode0), tagastab väärtuse 1; muidu 0. |
| `get_headnodes` | Tagastab täielik domeeninimi on headnodes klaster. Nimed on komaga eraldatud. Tühja stringi, tagastatakse tõrge. |
| `get_primary_headnode` | Saab esmane headnode täielik domeeninimi. Tühja stringi, tagastatakse tõrge. |
| `get_secondary_headnode` | Saab teisene headnode täielik domeeninimi. Tühja stringi, tagastatakse tõrge. |
| `get_primary_headnode_number` | Arvuline järelliite, esmane headnode saab. Tühja stringi, tagastatakse tõrge. |
| `get_secondary_headnode_number` | Arvuline järelliite, teisene headnode saab. Tühja stringi, tagastatakse tõrge. |

## <a name="commonusage"></a>Levinud mustreid

Selles jaotises antakse juhiseid mõned levinud kasutus mustreid, mis võivad ilmneda kirjutamiseks oma kohandatud skript rakendamise kohta.

### <a name="passing-parameters-to-a-script"></a>Skripti läbimise parameetrid

Mõnel juhul võib skripti vaja parameetrid. Näiteks, peate selleks, et otsida teavet Ambari REST API klaster administraatori parooli.

Parameetrite skripti nimetatakse _positsiooniga seotud parameetrite_ja määratakse `$1` esimese parameetri `$2` teise, ja nii kohta. `$0`sisaldab nime script ise.

Skripti parameetrid väärtus olema ümbritsetud ülakomadega ('), nii, et edastatud väärtus käsitletakse mõnda literaalmärgid ning eraldi käsitlemist pole antud kaasatud märgid, näiteks "!".

### <a name="setting-environment-variables"></a>Keskkonna muutujate seadmine

Säte on muutuja toimub järgmiselt:

    VARIABLENAME=value

Kus asub VARIABLENAME muutuja nimi. Juurdepääs muutuja pärast seda, kasutage `$VARIABLENAME`. Näiteks väärtuse, mis on antud positsiooniga seotud parameeter on muutuja nimega parooli määramiseks kasutaksite järgmist:

    PASSWORD=$1

Järgmise teabe juurdepääsu kasutada ka `$PASSWORD`.

Keskkonna muutujate skripti raames määratud ainult olemas skripti ulatust. Mõnel juhul peate lisama süsteemi lai keskkonna muutujad, mis kestab pärast script on lõpule jõudnud. Tavaliselt on see, et kobar SSH kaudu saate kasutada oma skripti installitud komponendid. Saate selle saavutamiseks, lisades keskkonna muutuja `/etc/environment`. Näiteks järgmine lisab __HADOOPI\_CONF\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Accessi asukohta, kuhu on talletatud kohandatud skriptid

Skriptide abil klaster peab kas olema salvestusruumi vaikekonto kobar või, kui teise salvestusruumi konto avaliku kirjutuskaitstud ümbrises. Kui teie script pääseb ressursse, mis asub mujal need ka peavad olema avalikult juurdepääsetava (vähemalt avaliku kirjutuskaitstud). Näiteks võite soovida faili allalaadimiseks kobar abil `download_file`.

Talletamine faili Azure storage konto juurdepääsetav kobar (nt salvestusruumi vaikekonto) järgi, annavad kiiresti juurde pääseda, mis selle salvestusruumi on Azure võrgustikus.

### <a name="checking-the-operating-system-version"></a>Operatsioonisüsteemi versiooni kontrollimine

Erinevate versioonide Hdinsightiga toetuvad teatud versioonide Ubuntu. Võib olla mis tuleb kontrollida oma skripti OS vahelised erinevused. Näiteks võib-olla peate installima kahendarvuks, mis on seotud Ubuntu versiooni.

Opsüsteemi versioon, kasutage `lsb_release`. Näiteks järgmine näitab, kuidas viide tõrva erinevad faili sõltuvalt Opsüsteemi versioon:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Skripti toimingu juurutamine kontroll

Siin on toimingud, me võttis juurutamise nende skriptide ettevalmistamisel.

- Pange failid, mis sisaldavad kohandatud skriptide kohas, kus on juurdepääsetav kobar sõlmed käigus juurutamist. See võib olla mis tahes vaike- või täiendava salvestusruumi kontod määratud kobar juurutamine või muude avalikult juurdepääsetava salvestusruumi container ajal.

- Veenduge, et nad käivitada impotently, et skripti saab teostada mitu korda sama sõlme skriptide kontrolli lisada.

- Ajutiste failide kausta /tmp abil saate hoida kasutatavaid skriptide allalaaditud failid ja seejärel puhastada need pärast skriptide on täidetud.

- Muudetud OS taseme sätted või Hadoopi teenuse konfiguratsiooni faile, kui soovite taaskäivitage Hdinsightiga teenuseid, et nad saavad kättesaamine mis tahes OS taseme sätteid, näiteks keskkonna muutujate skriptide määramine.

## <a name="runScriptAction"></a>Kuidas käivitada skripti toiming

Skripti toimingute abil saate kohandada Hdinsightiga kogumite Azure'i portaalis Azure PowerShelli Azure'i ressursi Manager (ARM) malle või Hdinsightiga .NET SDK abil. Juhised leiate teemast [skripti toiminguga](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Kohandatud skript näidised

Microsoft osutab näidisskriptid komponentide installimine on Hdinsightiga kobar. Näidisskriptid ja juhised, kuidas neid kasutada on saadaval veebisaidil saamiseks allpool olevaid linke.

- [Installimine ja kasutamine tooni Hdinsightiga kogumite](hdinsight-hadoop-hue-linux.md)
- [Installimine ja kasutamine R Hdinsightiga Hadoopi kogumite](hdinsight-hadoop-r-scripts-linux.md)
- [Installimine ja kasutamine Solri Hdinsightiga kogumite](hdinsight-hadoop-solr-install-linux.md)
- [Installimine ja kasutamine Giraph Hdinsightiga kogumite](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] Ülaltoodud seotud dokumendid on Linux-põhine Hdinsightiga kogumite. Skriptide, mis töötavad Windowsi-põhiste Hdinsightiga, teemast [skripti toimingu arendamiseni Hdinsightiga (Windows)](hdinsight-hadoop-script-actions.md) või kasutada linke, mis on saadaval iga artiklis ülaosas.

##<a name="troubleshooting"></a>Tõrkeotsing

Järgnevalt on tõrked võivad ilmneda, kui olete loonud skriptide abil.

__Error__: `$'\r': command not found`. Mõnikord järgneb `syntax error: unexpected end of file`.

_Põhjus_: selle tõrke põhjustavad kui read on skripti lõpus CRLF. UNIX süsteemide oodata ainult LF nagu rea lõpus.

See probleem ilmneb kõige sagedamini kui script on autoriks kohta Windows keskkonnas, nagu CRLF on levinud rida, mille nimi lõpeb jaoks palju teksti redaktorid opsüsteemis Windows.

_Eraldusvõime_: kui see on võimalus oma tekstiredaktoris, valige Unix vorming või LF rea lõpus. Muuta mõne LF CRLF võib kasutada ka Unix süsteemi järgmised käsud:

> [AZURE.NOTE] Järgmised käsud on umbes samaväärne, et nad peaksid muuta CRLF rea lõpud LF. Valige üks teie süsteemis saadaval Utiliidid põhjal.

| Käsk | Märkmete |
| ------- | ----- |
| `unix2dos -b INFILE` | Varundatakse koos algse faili lisamine. BAK laiend |
| `tr -d '\r' < INFILE > OUTFILE` | OUTFILE sisaldab ainult LF lõpud versioon |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | See muudab faili otse ilma uue faili loomine. |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | OUTFILE sisaldab ainult LF lõpud versioon.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_Põhjus_: See tõrge ilmneb juhul, kui skripti salvestatud UTF-8 koos bait tellimuse märgi (komplekti).

_Eraldusvõime_: ASCII või UTF-8 ilma komplekti faili salvestada. Ilma komplekti uue faili loomiseks võib kasutada ka Linuxi või Unix süsteemi järgmine käsk:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Ülaltoodud käsu, Asendage __INFILE__ fail, mis sisaldab komplekti. Uue faili nimi, mis sisaldab skripti ilma komplekti tuleks __OUTFILE__ .

## <a name="seeAlso"></a>Järgmised sammud

* Siit saate teada, kuidas [kohandada Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster-linux.md)

* .NET rakenduste haldamine Hdinsightiga loomise kohta lisateavet [Hdinsightiga .NET SDK viite](https://msdn.microsoft.com/library/mt271028.aspx) abil

* Saate teada, kuidas kasutada ülejäänud haldamise toiminguid Hdinsightiga kogumite [Hdinsightiga REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) abil.
