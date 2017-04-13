<properties
    pageTitle="Kasutage tühja serva sõlmed Hdinsightile | Microsoft Azure'i"
    description="Kuidas lisada ampty serva sõlme Hdinsightiga arvutikobaras, mida saate kasutada mõnda muud klienti ja testi/host Hdinsightiga rakenduste."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Kasutage tühja serva sõlmed Hdinsightiga

Saate teada, kuidas lisada tühja serva sõlme Linux-põhine Hdinsightiga arvutikobaras. Mõnda tühja serva sõlm on Linux virtuaalse masina kliendi tööriistu installinud ja konfigureerinud selle headnodes nagu, kuid pole Hadoopi teenused töötavad koos. Saate serva sõlm juurdepääs klaster, Testige oma klientrakendustes ja majutada oma klientrakendustes. 

Saate lisada mõne olemasoleva Hdinsightiga kobar uue arvutikobaras klaster loomisel on tühi serva sõlm. Lisamisel kuvatakse tühja serva tehakse Azure'i ressursihaldur malli abil.  Järgmises näites näitab, kuidas seda malli abil:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Nagu on näidatud valimi, saate soovi korral helistada [skripti toimingu](hdinsight-hadoop-customize-cluster-linux.md) täiendavad konfigureerimise, näiteks installida [Apache tooni](hdinsight-hadoop-hue-linux.md) serva sõlme.

Pärast seda, kui olete loonud mõne serva sõlm, ühenduse abil SSH serva sõlme ja käivitada kliendi tööriistad juurdepääsu Hadoopi kobar Hdinsightiga sisse.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Lisada mõne olemasoleva arvutikobaras serva sõlme

Selles jaotises saate ressursihaldur malli lisada mõne olemasoleva Hdinsightiga arvutikobaras serva sõlme.  Ressursihaldur malli leiate [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Ressursihaldur malli kõned skript toimingu skript https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh asukohas. Skripti ei mis tahes toiminguid.  See on näidata helistaja skripti toimingu ressursihaldur malli põhjal.

**Mõnda tühja serva sõlm lisamiseks mõne olemasoleva kobar**

1. Kui teil pole seda veel mõne Hdinsightiga kobar loomine  Vt [Hadoopi õpetus: alustamine Linux-põhine Hadoopi kasutamine Hdinsightiga](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klõpsake järgmisel pildil Azure'i sisse logida ja avage Mall, Azure'i ressursihaldur Azure'i portaalis. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigureerida järgmised atribuudid.

    - CLUSTERNAME: Sisestage mõne olemasoleva Hdinsightiga kobar nimi.
    - EDGENODESIZE: Saate valida ühe VM suurused.
    - EDGENODEPREFIX: Vaikeväärtus on **Uus**.  Vaikeväärtus kasutamisel serva sõlm nimi on **uue edgenode**.  Saate kohandada eesliite portaalist. Samuti saate kohandada malli täisnimi.


4. Klõpsake muudatuste salvestamiseks nuppu **OK** .
5. Valige **ressursirühm**ressursirühma.
6. Klõpsake **Läbivaatus juriidiline termineid**ja seejärel klõpsake nuppu **osta**.
7. Valige **armatuurlaud Kinnita**ja seejärel klõpsake nuppu **Loo**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Lisada serva sõlme klaster loomisel

Selles jaotises saate ressursihaldur malli loomine Hdinsightiga kobar soovitud serva sõlm.  Ressursihaldur malli leiate [Azure'i bloobimälu salvestusruumi container](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json)avalik. Ressursihaldur malli kõned skripti toimingu skripti, mis asub https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Skripti ei mis tahes toiminguid.  See on näidata helistaja skripti toimingu ressursihaldur malli põhjal.

**Mõnda tühja serva sõlm lisamiseks mõne olemasoleva kobar**

1. Kui teil pole seda veel mõne Hdinsightiga kobar loomine  Vt [Hadoopi õpetus: alustamine Linux-põhine Hadoopi kasutamine Hdinsightiga](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klõpsake järgmisel pildil Azure'i sisse logida ja avage Mall, Azure'i ressursihaldur Azure'i portaalis. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigureerida järgmised atribuudid.
        
    - CLUSTERNAME: Sisestage nimi uue kobar loomiseks.
    - CLUSTERLOGINUSERNAME: Sisestage kasutajanimi Hadoopi HTTP.  Vaikenimi on **administraator**.
    - CLUSTERLOGINPASSWORD: Sisestage Hadoopi HTTP kasutaja parooli.
    - SSHUSERNAME: Sisestage SSH kasutajanimi. Vaikenimi on **sshuser**.
    - SSHPASSWORD: Sisestage SSH kasutaja parool.
    - ASUKOHT: Sisestage ressursirühma nimi, klaster ja salvestusruumi vaikekonto asukoht.
    - CLUSTERTYPE: Vaikeväärtus on **Hadoopi**.
    - CLUSTERWORKERNODECOUNT: Vaikeväärtus on 2.
    - EDGENODESIZE: Saate valida ühe VM suurused.
    - EDGENODEPREFIX: Vaikeväärtus on **Uus**.  Vaikeväärtus kasutamisel serva sõlm nimi on **uue edgenode**.  Saate kohandada eesliite portaalist. Samuti saate kohandada malli täisnimi.

4. Klõpsake muudatuste salvestamiseks nuppu **OK** .
5. Sisestage **Ressursirühma**uue ressursirühma nimi.
6. Klõpsake **Läbivaatus juriidiline termineid**ja seejärel klõpsake nuppu **osta**.
7. Valige **armatuurlaud Kinnita**ja seejärel klõpsake nuppu **Loo**. 


## <a name="access-an-edge-node"></a>Juurdepääs soovitud serva sõlm

Serva sõlm ssh lõpp-punkti on <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Näiteks uue-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Serva sõlm kuvatakse rakenduse Azure'i portaalis.  Portaali annab teile juurdepääsu sõlme serva SSH abil teavet.

**Serva sõlm SSH lõpp-punkti kinnitamiseks**

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Avage soovitud serva sõlm Hdinsightiga kobar.
3. Klõpsake **rakenduste** kobar keelest. Näete peab sõlme serva.  Vaikenimi on **uue edgenode**.
4. Klõpsake selle serva sõlme. Näete peab SSH lõpp-punkti.

**Serva sõlm taru kasutamine**

5. Kasutage SSH ühenduse serva sõlm.  Vt teemat [kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix, või OS X](hdinsight-hadoop-linux-use-ssh-unix.md) või [kasutada SSH Linux-põhine Hadoopi Hdinsightiga Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Pärast serva sõlm SSH abil, kasutada taru konsooli avamiseks järgmine käsk:

        hive
7. Taru tabelite kuvamiseks klaster järgmine käsk:

        show tables;

## <a name="delete-an-edge-node"></a>Kustuta soovitud serva sõlm

Saate kustutada soovitud serva sõlm Azure portaalist.

**Juurde pääseda soovitud serva sõlm**

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Avage soovitud serva sõlm Hdinsightiga kobar.
3. Klõpsake **rakenduste** kobar keelest. Näete peab serva sõlmed loendit.  
4. Paremklõpsake sõlme serva, mille soovite kustutada, ja seejärel klõpsake käsku **Kustuta**.
5. Klõpsake nuppu **Jah** kinnitamiseks.

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis on teada, kuidas lisada serva sõlme ja kuidas pääseda juurde sõlme serva. Lisateabe saamiseks lugege järgmisi artikleid:

- [Rakenduste installimine Hdinsightiga](hdinsight-apps-install-applications.md): saate teada, kuidas installida rakendus Hdinsightile oma kogumite.
- [Kohandatud Hdinsightiga rakenduste installimine](hdinsight-apps-install-custom-applications.md): saate teada, kuidas avaldamata Hdinsightiga taotluse Hdinsightiga juurutamine.
- [Avaldamine Hdinsightiga rakenduste](hdinsight-apps-publish-applications.md): saate teada, kuidas avaldada oma kohandatud Hdinsightiga rakenduste Azure'i turuplatsilt.
- [MSDN: Hdinsightiga rakenduse installimiseks](https://msdn.microsoft.com/library/mt706515.aspx): saate teada, kuidas määratleda Hdinsightiga rakendused.
- [Kohandamine Linux-põhine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster-linux.md): saate teada, kuidas skripti toimingu abil saate installida täiendavad rakendused.
- [Hadoopi loomine Linux-põhine le Hdinsightiga ressursihaldur mallide kasutamine](hdinsight-hadoop-create-linux-clusters-arm-templates.md): saate teada, kuidas kõne ressursihaldur Mallid Hdinsightiga kogumite loomiseks.

