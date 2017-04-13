<properties
   pageTitle="Turvaline klaster töötab Windows Windows turvalisuse abil | Microsoft Azure'i"
   description="Saate teada, kuidas konfigureerida sõlm sõlme ja kliendi sõlme turvalisus autonoomse klaster töötab Windows Windows turvalisuse abil."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Turvaline autonoomse kobar Windows Windows turvalisuse abil

Teenuse struktuuri kobar volitamata juurdepääsu vältimiseks tuleb turvaline, eriti siis, kui ta on tootmise töökoormus see töötab. Selles artiklis kirjeldatakse konfigureerimise abil Windowsi turbe *ClusterConfig.JSON* faili sõlm sõlme ja kliendi sõlme turvalisus ja vastab konfigureerimine turvalisus juhisega [loomine autonoomse kobar töötab Windows](service-fabric-cluster-creation-for-windows-server.md). Lisateavet selle kohta, kuidas teenuse struktuuri kasutab Windows turvalisus, lugege teemat [kobar turvalisus stsenaariumid](service-fabric-cluster-security.md).

>[AZURE.NOTE]
Kaaluge turvalisus valiku sõlm sõlme väärtpaberi hoolikalt, kuna seal on pole kobar uuendus ühe väärtpaberi valik teise. Turvalisus valiku muutmine nõuaks täielik kobar taastada.

## <a name="configure-windows-security"></a>Windowsi turbe konfigureerimine
Valimi *ClusterConfig.Windows.JSON* konfigureerimise faili alla laadida ja [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) eraldiseisev kobar pakett sisaldab malli konfigureerida Windowsi turvalisus.  Windowsi turbe konfigureeritud jaotises **Atribuudid** :

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Otsingukonfiguratsiooni säte**|**Kirjeldus**|
|-----------------------|--------------------------|
|ClusterCredentialType|Windowsi turvalisuse lubamiseks *Windowsi*parameetri **ClusterCredentialType** .|
|ServerCredentialType|Windowsi turbe klientidele on lubatud, seades **ServerCredentialType** parameetri *Windowsi*. See näitab, et kliendid ja klaster klaster, töötavad Active Directory domeenis.|
|WindowsIdentities|Sisaldab kobar ja kliendi identiteedid.|
|ClusterIdentity|Konfigureerib sõlm sõlme turvalisus. Komaga eraldatud loend jaotises hallatavad kontod või seadme nimed.|
|ClientIdentities|Konfigureerib kliendi sõlme turvalisus. Massiivi klienti kasutajakontode.|
|Identiteedi|Kliendi ID, kasutaja domeeni.|
|IsAdmin|True saate määrata, et domeeni kasutajal on administraatori kliendi juurdepääsu, kasutaja kliendi juurdepääsu false.|

Kasutades **ClusterIdentity**, milles on konfigureeritud [sõlm sõlm turvalisus](service-fabric-cluster-security.md#node-to-node-security) . Usalda seoseid sõlmed koostamiseks need tuleb arvestada üksteisest. Seda saab teha kahel viisil: Määrake jaotises hallatavad teenusekonto klaster kõik sõlmed sisaldava või määrata kõik sõlmed domeeni sõlm identiteedid klaster. Soovitame tungivalt, kasutades [Jaotises hallatavad teenusekonto (Assotsiatsioon)](https://technet.microsoft.com/library/hh831782.aspx) meetodit, eriti suuremat kogumite (pikem kui 10 sõlmed) või kogumite, mis võivad kasvata või Kahanda.
Seda moodust võimaldab sõlmed tuleb lisada või eemaldada Assotsiatsioon, nõudmata kobar manifest muudatusi. Seda moodust nõua domeeni rühma, mille kobar administraatorid on antud lisada ja eemaldada liikmeid. Lisateavet leiate teemast [Alustamine rühmakontod hallatav teenus](http://technet.microsoft.com/library/jj128431.aspx).

[Kliendi sõlm turvalisus](service-fabric-cluster-security.md#client-to-node-security) on konfigureeritud, kasutades **ClientIdentities**. Usalda klaster ja kliendi vahelise loomiseks tuleb konfigureerida kobar teada, millised kliendi identiteedid, mis see on usaldusväärsed. Seda saab teha kahel viisil: määrake domeeni kasutajad, mida saate ühendada või määrake domeeni sõlm kasutajad, kes saab ühendada. Teenuse struktuuri toetab kahte erinevat juhtelemendi tüüpi klientidele, mis on ühendatud teenuse struktuuri kobar: administraator ja kasutajale. Juurdepääsu reguleerimine pakub võimalust kobar administraatori piirata juurdepääsu teatud erinevate kasutajarühmade, klaster turvalisemaks kobar toimingute jaoks.  Administraatoritel on täielik juurdepääs halduse võimaluste (sh lugemis-ja kirjutamisõigusega võimaluste). Kasutajad, vaikimisi on ainult lugemisõigus halduse võimaluste (nt päringu võimalused) ja võimalus lahendada rakendused ja teenused.

Järgmises näites **Turvalisus** jaotises konfigureerib Windowsi turvalisus ja määrab *ServiceFabric/clusterA.contoso.com* masinad kuuluvad klaster ja et *CONTOSO\usera* administraatori kliendi juurdepääs.

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Järgmised sammud

Pärast Windowsi turvalisuse konfigureerimist *ClusterConfig.JSON* faili, elulookirjelduse kobar loomisprotsessi [loomine autonoomse kobar](service-fabric-cluster-creation-for-windows-server.md)töötab Windows.

Lisateavet selle kohta, kuidas sõlm sõlme turvalisus, kliendi sõlme turvalisus ja Rollipõhine juurdepääsu reguleerimine, vt [kobar turvalisus stsenaariumid](service-fabric-cluster-security.md).

Vt [ühenduse loomine turvaline kobar](service-fabric-connect-to-secure-cluster.md) näiteid ühenduse PowerShelli või FabricClient.
