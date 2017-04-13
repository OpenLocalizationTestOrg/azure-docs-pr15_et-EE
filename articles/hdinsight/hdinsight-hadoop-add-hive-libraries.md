<properties
pageTitle="Lisage taru teekide Hdinsightiga kobar loomise ajal | Azure'i"
description="Saate teada, kuidas lisada taru teekide (jar failid), on Hdinsightiga arvutikobaras kobar loomise ajal."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Lisage taru teekide Hdinsightiga kobar loomise ajal

Kui teil on teegid, mida kasutate sageli taru kohta Hdinsightiga, see dokument sisaldab teavet eelnevalt kobar loomise ajal laadida teeke skripti toimingu abil. See muudab teekide globaalselt saadaval taru (ei ole vaja kasutada neid laadimiseks [Lisamine JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) .)

##<a name="how-it-works"></a>Kuidas see toimib

Klaster loomisel saate soovi korral skripti toiming, mis käivitatakse skripti kobar sõlmed ajal nende loomisel. Selles dokumendis skripti aktsepteerib ühe parameetri, mis on WASB asukoht, mis sisaldab teekide (jar-failidena salvestatud), mis laaditakse eelnevalt.

Kobar loomise ajal skripti loetleb failid, kopeerib neid on `/usr/lib/customhivelibs/` directory pea ja töötaja sõlmed, siis liidab neid on `hive.aux.jars.path` atribuudi selle `core-site.xml` fail. Linux-põhine kogumite, klõpsake selle ka värskendamist olevat `hive-env.sh` faili asukoht failid.

> [AZURE.NOTE] Teekide kasutamine skripti toimingud selle artikli muudab saadaval järgmistel juhtudel:
>
> * __Linux-põhine Hdinsightiga__ - __taru käsurea__, __WebHCat__ (Templeton) ja __HiveServer2__kasutamise korral.
> * __Windowsi-põhiste Hdinsightiga__ - __taru käsurea__ ja __WebHCat__ (Templeton) kasutamise korral.

##<a name="the-script"></a>Skripti

__Skripti asukoht__

__Kogumite Linux-põhine__jaoks: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

__Windowsi-põhiste kogumite__jaoks: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Nõuded__

* Skriptide peab rakendatakse nii __pea sõlmed__ ka __töötajal sõlmed__.

* Soovite installida purgid peavad olema talletatud Azure'i bloobimälu __ühe ümbrises__. 

* Salvestusruumi konto, mis sisaldab seda teeki, jar failid __peab__ olema seotud Hdinsightiga kobar loomise ajal. Seda saab teha on kaks võimalust:

    * Olles ümbris klaster salvestusruumi vaikekonto kohta.
    
    * Klõpsake mõnda lingitud salvestusruumi container ümbris olles. Näiteks saate kasutada portaali __Valikuline konfigureerimine__, lisada täiendavat salvestusruumi __lingitud salvestusruumi kontod__ .

* WASB tee ümbris peab olema määratud parameetrina skript toiming. Näiteks kui purgid on talletatud nimega __kena tegevust jälle__ salvestusruumi kontol nimega __mystorage__ümbris, parameetri oleks __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] Selle dokumendi eeldab salvestusruumi konto on juba loomine, Bloobivahemälu container ja üles laaditud faile. 
    >
    > Kui te pole loonud salvestusruumi konto, saate seda teha [Azure portaali](https://portal.azure.com)kaudu. Seejärel saate luua uue container konto ja failide üleslaadimine kasuliku nagu [Azure'i salvestusruumi Explorer](http://storageexplorer.com/) .

##<a name="create-a-cluster-using-the-script"></a>Kasutades skripti klaster loomine

> [AZURE.NOTE] Järgmised toimingud luua uus Linuxi-põhiste Hdinsightiga klaster. Uus Windowsi-põhiste klaster loomiseks valige __Windowsi__ kobar OS klaster loomisel ja Windows (PowerShelli) skripti bash skripti asemel kasutada.
> 
> Saate luua skripti abil klaster ka Azure PowerShelli või Hdinsightiga .NET SDK. Nende meetodite kasutamise kohta leiate lisateavet teemast [kohandamine Hdinsightiga kogumite skript toimingute](hdinsight-hadoop-customize-cluster-linux.md).

1. Käivitage ettevalmistamise klaster [sätte Hdinsightiga](hdinsight-hadoop-provision-linux-clusters.md#portal)rühmades Linux juhiste abil, kuid täitke ettevalmistamise.

2. **Valikuline konfigureerimine** enne, valige **Skripti toimingud**ja teavet, nagu allpool näidatud:

    * __Nimi__: sisestage skripti toimingu sõbralik nimi.
    * __Skripti URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __Juht__: märkige see ruut
    * __Töötaja__: märkige see suvand.
    * __ZOOKEEPER__: See väli tühjaks jätta.
    * __Parameetrite__: Sisestage meiliaadress WASB container ja salvestusruumi konto, mis sisaldab purgid. Näiteks __wasbs://libs@mystorage.blob.core.windows.net/__.

3. **Skripti toimingud**allosas kasutada konfiguratsiooni salvestamiseks **Valige** nupp.

4. **Valikuline konfigureerimine** enne, __Lingitud salvestusruumi kontod__ ja valige link __Lisa salvestusruumi klahvi__ . Valige salvestusruumi konto, mis sisaldab purgid, ja seejärel kasutage sätted salvestada ja __Valikuline konfigureerimine__ tera naasmiseks __Valige__ nupud.

5. Valikuline konfiguratsiooniteavet salvestamiseks kasutage **Valikuline konfigureerimine** tera allosas nuppu **Valige** .

6. Jätkake ettevalmistamise klaster, nagu on kirjeldatud [sätte Hdinsightiga](hdinsight-hadoop-provision-linux-clusters.md#portal)rühmades Linux.

Kui kobar loomine lõpetab, saab kasutada taru ilma seda skripti kaudu lisatud purgid on `ADD JAR` lause.

##<a name="next-steps"></a>Järgmised sammud

Selles dokumendis te teada, kuidas lisada taru teekide sisalduvate jar failide Hdinsightiga kobar kobar loomise ajal. Taru töötamise kohta leiate lisateavet teemast [Kasutamine taru koos Hdinsightiga](hdinsight-use-hive.md)
