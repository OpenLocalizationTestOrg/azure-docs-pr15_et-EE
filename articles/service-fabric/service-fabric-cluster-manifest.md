<properties
   pageTitle="Autonoomse klaster konfigureerimine | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas konfigureerida oma eraldi või privaatne teenuse struktuuri kobar."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Autonoomse Windows kobar sätete konfigureerimine

Selles artiklis kirjeldatakse, kuidas konfigureerida autonoomse teenuse struktuuri kobar _**ClusterConfig.JSON**_ -faili abil. Selle faili abil saate määrata teenuse struktuuri sõlmed ja nende IP-aadressid, erinevat tüüpi sõlmed näiteks järgmist teavet klaster, Turve konfiguratsioone kui ka võrgutopoloogia osas viga/uuendada domeenide jaoks eraldi klaster.

Kui te [laadige autonoomse teenuse struktuuri](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), mõned näited ClusterConfig.JSON faili laaditakse teie arvutis töötavad. Näidised, millel *DevCluster* nende nimed abil saate luua klaster koos kõigi kolme sõlmed samasse arvutisse, nt loogika sõlmed. Nendest peab olema märgitud vähemalt üks sõlm esmane sõlm nimega. See on abiks arendus või katse lahendada ja ei toetata tootmise kobar. Näidised, millel *MultiMachine* nende nimed abil saate luua tootmise kvaliteedi kobar, koos eraldi arvutisse iga sõlme. Arvu need kobar esmane sõlmed põhineb [töökindluse tase](#reliability).

1. *ClusterConfig.Unsecure.DevCluster.JSON* ja *ClusterConfig.Unsecure.MultiMachine.JSON* näitab, kuidas luua turvamata testi või tootmise kobar vastavalt. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* ja *ClusterConfig.Windows.MultiMachine.JSON* näitab, kuidas luua testi või tootmise kobar, turvatud [Windowsi turvalisuse](service-fabric-windows-cluster-windows-security.md)abil.

3. *ClusterConfig.X509.DevCluster.JSON* ja *ClusterConfig.X509.MultiMachine.JSON* näitab, kuidas luua testi või tootmise kobar, kasutades turvatud [X509 serdi põhineva turvalisuse](service-fabric-windows-cluster-x509-security.md). 


Nüüd uurime erinevate osade _**ClusterConfig.JSON**_ fail nimega all.

## <a name="general-cluster-configurations"></a>Üldine kobar konfiguratsioone
See hõlmab laia kobar teatud konfiguratsioone, nagu on näidatud allpool JSON väljavõte.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

Saate anda mis tahes sõbralik nimi klaster teenuse struktuuri määrates muutuja **nimi** . **ClusterConfigurationVersion** on versiooninumber klaster; tuleks suurendada seda iga kord, kui täiendate klaster teenuse struktuuri. Olete aga jätke **apiVersion** vaikeväärtus.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Sõlmed klaster
Saate konfigureerida sõlmed klaster teenuse struktuuri kohta, kasutades järgmist koodilõigu kuvatakse jaotises **sõlmed** .

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Teenuse struktuuri kobar peab sisaldama vähemalt 3 sõlmed. Selles jaotises saate lisada rohkem sõlmi, häälestust ühe. Järgmises tabelis selgitatakse iga sõlme sätete konfigureerimine.

|**Sõlm konfigureerimine**|**Kirjeldus**|
|-----------------------|--------------------------|
|nodeName|Saate anda sõlme mis tahes sõbralik nimi.|
|Sordib|Avage käsuviiba aken ja tippige oma sõlm IP-aadress teada `ipconfig`. Pange tähele IPV4 aadress ja selle määramine **sordib** muutujana.|
|nodeTypeRef|Iga sõlme saab määrata eri sõlm tüüp. [Tüüpi](#nodetypes) on määratletud allpool olevat jaotist.|
|faultDomain|Viga domeenide lubamine kobar administraatorid määratleda füüsilise sõlmed, mis võib nurjuda tõttu ühiskasutusega füüsilise sõltuvused samal ajal.|
|upgradeDomain|Täiendamine domeenide kirjeldada komplekti sõlmed teenuse struktuuri uuendamine veebisaidil kohta samal ajal sulgema. Millised sõlmed mis täiendamine domeenide määramiseks saate valida, nagu need on piiratud, mis tahes füüsilise nõuded.| 


## <a name="cluster-properties"></a>Kobar atribuudid

Jaotist **Atribuudid** ja klõpsake soovitud ClusterConfig.JSON kasutatakse konfigureerimiseks klaster järgmiselt.

<a id="reliability"></a>
### <a name="reliability"></a>Töökindluse 
Jaotise **reliabilityLevel** määratleb süsteemi teenused, mida saab käitada esmane sõlmed klaster ning eksemplaride arv. Suurendab nende teenuste töökindluse ja seega klaster. Saate seada muutuja *pronks*, *hõbedane*, *kuldne* või *Platinum* 3, 5, 7 või 9 eksemplari nende teenuste jaoks vastavalt. Vaadake alltoodud näites.

    "reliabilityLevel": "Bronze",
    
Pange tähele, et alates esmane sõlm töötab ühe eksemplari süsteemi teenuseid, oleks vaja vähemalt 3 esmane sõlmed jaoks *pronks*, *hõbedane*, *kuldne* 7 ja 9 *Platinum* töökindluse tasemete 5.


### <a name="diagnostics"></a>Diagnostika
**DiagnosticsStore** jaotis võimaldab teil konfigureerida parameetreid diagnostika- ja tõrkeotsingu sõlm või kobar tõrkeid, nagu on näidatud järgmises koodilõigu. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**Metaandmete** on teie kobar diagnostika kirjelduse ja saate määrata ühe häälestust. Nende muutujate aidake allalaadimine ETW Jälita logid, krahh puistab ning jõudlust hinnale. ETW Jälita logid kohta lisateabe saamiseks lugege [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) ja [ETW jälgimine](https://msdn.microsoft.com/library/ms751538.aspx) . Kõik logid, sh [ootamatult sulguda puistab](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) ja [jõudluse hinnale](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) saate suunata **connectionString** kausta teie arvutis. Samuti saate *AzureStorage* diagnostika talletamiseks. Allpool on valimi koodilõigu.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Turvalisus 
**Turbejaotist** on vajalik turvaline autonoomse teenuse struktuuri kobar. Järgmised koodilõigu näitab käesoleva osa.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**Metaandmete** on turvaline klaster kirjelduse ja saate määrata ühe häälestust. **ClusterCredentialType** ja **ServerCredentialType** kindlaks väärtpaberi aastatulususe klaster ja sõlmed rakendab tüüpi. Need saate määrata, kas *X509* serdi põhineva turvalisuse või *Windows* Azure Active Directory-põhise turvalisuse eesmärgil. Ülejäänud **turbejaotist** põhineb tüüpi turvalisus. [Autonoomse kobar turvalisuse serdid-põhine](service-fabric-windows-cluster-x509-security.md) või [Windowsi turvalisuse autonoomse kobar](service-fabric-windows-cluster-windows-security.md) lugeda teavet jaotisest **Turvalisus** täitke kohta.


<a id="nodetypes"></a>
### <a name="node-types"></a>Sõlm tüübid
**NodeTypes** jaotises kirjeldatakse sõlmed, mis on klaster tüüp. Vähemalt üks sõlm tüüp peab olema määratud klaster, nagu on näidatud allpool koodilõigu. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

**Nimi** on sõbralik nimi selle konkreetse sõlme tüübi puhul. Seda tüüpi sõlm sõlm luua, määrata selle sõbralik nimi, mida **nodeTypeRef** muutuja jaoks sõlme, nagu [eespool](#clusternodes)nimetatud. Määratlege iga sõlme puhul kasutatakse ühenduse lõpp-punktid. Soovi korral saate need ühenduse lõpp-punktid, mis tahes pordinumber seni, kuni ta ei ole vastuolus teiste näitajate see. Mitme sõlme klaster, ilmneb üks või mitu esmane sõlmed (st **isPrimary** on seatud *tõene*), olenevalt [**reliabilityLevel**](#reliability). [Teenuse struktuuri kobar võimsus plaanimist](service-fabric-cluster-capacity.md) siit leiate teavet **nodeTypes** ja **reliabilityLevel** väärtuste ja teada, mis on esmane ja peamise sõlm tüübid. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Lõpp-punktid saab konfigureerida sõlm tüübid

- *clientConnectionEndpointPort* on ühenduse kobar, kasutades klient API kliendi poolt kasutatav port. 
- *clusterConnectionEndpointPort* on portide, kus sõlmed omavahel suhelda.
- *leaseDriverEndpointPort* on välja selgitada, kui sõlmed on endiselt aktiivsed kobar üürilepingu juht kasutatav port. 
- *serviceConnectionEndpointPort* on pordi kasutatavaid rakenduste ja teenuste juurutatud sõlme, teenuse struktuuri kliendi kindla sõlme suhelda.
- *httpGatewayEndpointPort* on teenuse struktuuri Exploreri klaster ühenduse loomiseks kasutatav port.
- *ephemeralPorts* on [dünaamiline pordid, mida OS](https://support.microsoft.com/kb/929851). Teenuse struktuuri kasutab neid pordid rakenduse osa ja ülejäänud saab OS. See on ka kaart selles vahemikus olemasoleva vahemiku kohal OS, kõik eesmärgil abil saate näidisfailide JSON esitatud vahemikud. Peate veenduge, et algus ja lõpp pordid erinevus on vähemalt 255 märki. 
- *applicationPorts* on pordid, mis kasutavad rakendused teenuse struktuuri. Need peaks olema selle *ephemeralPorts*, piisavalt kataks lõpp-punkti nõue rakenduste alamhulga. Teenuse struktuuri kasutada neid iga kord, kui uus pordid on nõutavad, samuti avada tulemüüri portide nende eest. 
- *reverseProxyEndpointPort* on valikuline tagant puhverserveri lõpp. Üksikasjalikumat teavet teemast [Teenuse struktuuri tühistada puhverserveri](service-fabric-reverseproxy.md) . 


### <a name="other-settings"></a>Muud sätted
Jaotise **fabricSettings** võimaldab teil määrata juurkausta kataloogide teenuse struktuuri andmete ja logid. Saate kohandada need ainult algse kobar loomise ajal. Allpool on valimi koodilõigu käesoleva.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Soovitame kasutades-OS ketas FabricDataRoot ja FabricLogRoot nagu see pakub rohkem töökindluse suhtes OS jookseb. Pange tähele, et kui soovite kohandada ainult juursait andmeid, siis log juurkausta paigutatakse ühe taseme võrra allapoole andmete juurkausta.


## <a name="next-steps"></a>Järgmised sammud

Kui teil on seadistatud autonoomse kobar häälestust ühe täieliku ClusterConfig.JSON faili, saate juurutada klaster järgides artikli [loomine on Azure teenuse struktuuri kobar kohapealse või pilves](service-fabric-cluster-creation-for-windows-server.md) ja siis jätkake [visualiseerimine klaster teenuse struktuuri Exploreriga](service-fabric-visualizing-your-cluster.md).


