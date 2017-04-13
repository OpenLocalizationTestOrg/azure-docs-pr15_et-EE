<properties
   pageTitle="Teenuse struktuuri tagant puhverserveri | Microsoft Azure'i"
   description="Teenuse struktuuri tagant puhverserveri kasutamiseks microservices kaudu kui ka väljaspool klaster suhtlus"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Teenuse struktuuri tagant puhverserveri

Teenuse struktuuri tagant puhverserveri on pööratud puhverserveri sisse ehitatud teenuse struktuuri, mis võimaldab adressaatide valimiseks microservices teenuse struktuuri kobar, mis edastavad HTTP lõpp-punktid.

## <a name="microservices-communication-model"></a>Microservices side mudel

Klõpsake teenuse struktuuri Microservices tavaliselt alamhulga VM klaster Käivita ja saate teisaldada ühe VM teise mitmel põhjusel. Nii saate lõpp-punktide microservices dünaamiliselt muuta. Tüüpilised muster edastama selle microservice on lahenda tsükkel allpool

1. Lahendada teenuse asukoht algselt nime andmise teenuse kaudu.
2. Teenusega ühenduse loomiseks.
3. Põhjuse ühenduse tõrkeid ja teenuse asukoht vajaduse korral uuesti lahendada.

Selle protsessi tähendab tavaliselt mähkimine kliendipoolne side teekide uuesti tsükkel, mis rakendab teenuse eraldusvõime ja proovige uuesti poliitika.
Selle teema kohta leiate lisateavet teemast [suhtlemine teenused](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>SF tagant puhverserveri kaudu suhtlemine
Teenuse struktuuri tagant puhverserveri töötab klaster sõlme. See teeb kogu teenuse eraldusvõime protsessi kliendi nimel ja edastab Kliendi taotlus. Nii töötavate klaster kliendid saavad lihtsalt kasutada mis tahes kliendipoolne HTTP side teekide rääkida target teenus SF tagant puhverserveri töötab sama sõlme kaudu.

![Sisesuhtluse][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Kontaktidel väljaspool klaster Microservices kaudu
Vaikimisi väline suhtlus mudeli microservices on **osalemine** , kus iga teenuse vaikimisi ei pääse juurde otse väliste klientide. [Azure'i laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-overview.md) on võrgu microservices ja välise kliendid, vahelist piiri, mis teeb võrgu aadress tõlge ja edastab välise taotlusi sisemise **IP:port** lõpp-punktid. Selleks on microservice lõpp-punkti otse puuetega inimestele juurdepääsetavate väliste klientidega, Azure'i laadi koormusetasakaalustusteenuse tuleb esmalt konfigureerida iga teenuse klaster kasutatav pordi-liikluse edasi. Lisaks enamik microservices (esp. stateful microservices) ei live sõlme klaster ning nad sõlme Tõrkesiirde, klõpsake vahel liikuda nii, et sellisel juhul ei saa Azure'i laadi koormusetasakaalustusteenuse tõhusalt määratleda target sõlm kujundusmuudatusi asuvad suunata liiklust.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Kontaktidel Microservices SF tagant puhverserverist kaudu väljaspool klaster

Azure'i laadi koormusetasakaalustusteenuse konfigureerimine üksikute teenuse pordid, mitte lihtsalt SF vastupidi proxy portide võib olla konfigureeritud Azure laadimine koormusetasakaalustusteenuse. See võimaldab kliente väljaspool klaster saavutamiseks teenuste sees kobar tagant puhverserveri ilma mõne täiendava konfiguratsioone kaudu.

![Väline suhtlus][0]

>[AZURE.WARNING] Laadi koormusetasakaalustusteenuse konfigureerimine tagant proxy portide, teeb mikro teenuste kobar, mis edastavad http endpoint, tuleb addressible kaudu väljaspool klaster.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>URI vorming adressaatide valimiseks tagant puhverserveri kaudu

Pööratud puhverserveri kasutab URI kindlas vormingus tuvastada, millist sissetuleva taotluse edasisaadetava teenuse sektsiooni:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **httpd:** Tagant puhverserveri võib olla konfigureeritud HTTP- või HTTPS-liikluse. HTTPS-liikluse, kui SSL-i lõpetamise aset tagant puhverserveri. Nõuab, et ümbersuunamisele tagant puhverserveri Services klaster on http kaudu.
 - **Kobar FQDN | sisemine IP:** Välistele klientidele, tagant puhverserveri saab seadistada nii, et see oleks kättesaadav kobar-domeeni (nt mycluster.eastus.cloudapp.azure.com) kaudu. Vaikimisi tagant puhverserveri käivitatakse iga sõlm nii sisemise liikluse jõuate localhost või mis tahes sisemise sõlm IP (nt 10.0.0.1).
 - **Port:** Pööratud puhverserveri jaoks määratud pordi. Näiteks: 19008.
 - **ServiceInstanceName:** See on täielikult kvalifitseeritud juurutatud teenuse eksemplari nimi soovite saavutamiseks teenuse on "struktuuri: /" värviskeemi. Näiteks saavutamiseks teenuse *struktuuri: / myapp/myservice/*, kasutate *myapp/myservice*.
 - **Järelliite tee:** See on tegelik URL-tee teenus, mida soovite ühendada. Näiteks *myapi/väärtused/lisamine/3*
 - **PartitionKey:** Sektsioonitud teenuse, see on arvutatud sektsiooni võti sektsiooni, kellega soovite suhelda. Pange tähele, et see on *pole* sektsiooni ID GUID. See parameeter ei vaja teenuste abil singleton sektsiooni kava.
 - **PartitionKind:** Teenuse sektsiooni kava. See võib olla "Int64Range" või "Nimega". See parameeter ei vaja teenuste abil singleton sektsiooni kava.
 - **Ajalõpp:**  Seda määrab jaoks loodud tagant puhverserveri taotlus kliendi nimel teenusega http-päringu ajalõpp. See vaikeväärtus on 60 sekundi järel. See on valikuline parameeter.

### <a name="example-usage"></a>Näiteks kasutamine

Näiteks vaatame teenuse **struktuuri: / MyApp/MyService** mis avab mõni järgmised URL-i HTTP kuulajale:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Koos järgmistest allikatest:

 - `/index.html`
 - `/api/users/<userId>`

Kui teenus kasutab singleton eraldamine kava, on *PartitionKey* ja *PartitionKind* päringustringi parameetrite vajalike ja teenuse juurde lüüsi nimega.

 - Väliselt:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Ettevõttesiseselt:`http://localhost:19008/MyApp/MyService`

Kui teenus kasutab ühtse Int64 eraldamine kava, kasutatakse selle *PartitionKey* ja *PartitionKind* päringustringi parameetrite saavutamiseks teenuse sektsiooni:

 - Väliselt:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Ettevõttesiseselt:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Ressursse, mis on esitatud teenuse saavutamiseks paigutada ressursi tee lihtsalt pärast URL-i teenuse nime:

 - Väliselt:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Ettevõttesiseselt:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Lüüsi edastab need taotlused seejärel teenuse URL:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Muud käsitsemise portide jagamiseks teenused

Lüüsi rakendus proovib uuesti lahendamiseks teenuse aadress ja proovige taotlus, kui teenus ei pääse. See on üks peamisi eeliseid lüüsi, kui kliendi kood ei pruugi rakendada oma teenuse eraldusvõime ja lahendada tsükkel.

Üldiselt kui teenus ei pääse see tähendab, et eksemplari või koopia on teisaldatud erineva sõlme tavaline elutsükli osana. Kui see juhtub, lüüsi võidakse kuvada tõrketeade võrgu ühendus, mis näitab, lõpp pole enam avatud algselt lahendatud aadress.

Koopiad või teenuse eksemplari Majutaja protsess saate ühiskasutusse anda ja jagada ka portide, kui korraldaja http.sys veebipõhine server, sh:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [ASP.net-i Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

Sellisel juhul on tõenäoline, et veebiserver on saadaval Majutaja protsess ja vastata, kuid lahendatud eksemplari või koopia pole enam saadaval Host. Sel juhul saate lüüsi vastuse HTTP 404 veebiserver. Seetõttu on HTTP 404 on kaks erinevat tähendust.

 1. Teenuse aadress on õige, kuid kasutaja nõutud ressurss pole olemas.
 2. Teenuse aadress on vale ja kasutaja nõutud ressursi võib-olla erinevad sõlme tegelikult olemas.

Esimesel juhul, see on tavaline HTTP 404, mida kasutaja tõrge. Teisel juhul, taotlenud kasutaja ressurss, mis on olemas, kuid lüüsi ei saa seda leida, kuna teenuse ise on teisaldatud, millisel juhul peab lüüsi uuesti aadressi lahendamiseks ja proovige uuesti.

Lüüsi seega peab nii kummalgi juhul eristada. Selleks, et eristada, on vaja vihje serverist.

 - Vaikimisi rakenduse lüüsi eeldab juhul #2 ja proovib uuesti lahendamiseks ja probleemi uuesti taotluse.
 - Rakenduse lüüsi suurtähtedeks #1 näitamaks teenuse peaks tagastama järgmised HTTP vastuse päis:

`X-ServiceFabric : ResourceNotFound`

See HTTP vastuse päis näitab tavaline HTTP 404 olukorda, kus pole selle ressursi ja lüüsi ei oska uuesti lahendamiseks teenuse aadress.

## <a name="setup-and-configuration"></a>Häälestamise ja konfigureerimise
Teenuse struktuuri tagant puhverserveri saab lubada kobar [Azure'i ressursihaldur malli](./service-fabric-cluster-creation-via-arm.md)kaudu.

Kui teil on mall kobar, mida soovite kasutada (proovi mallide põhjal või luues kohandatud ressursi halduri malli) tagant puhverserveri saab lubada malli toimige järgmiselt.

1. Määratleda pordi tagant puhverserveri [Parameetrid jaotises](../resource-group-authoring-templates.md) malli.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Määrake pordi nodetype objekte **kobar** [Ressursi jaotis](../resource-group-authoring-templates.md)

    Jaoks apiVersion enne 2016-09-01' pordi on tähistatud parameetri nimi ***httpApplicationGatewayEndpointPort***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Jaoks apiVersion's sisse- või pärast 2016-09-01' pordi on tähistatud parameetri nimi ***reverseProxyEndpointPort***

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Pööratud puhverserveri: Azure'i kobar väljaspool aadressis, install juhises 1 määratud pordi **azure laadi koormusetasakaalustusteenuse reeglid** .

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Port SSL-sertide konfigureerimine tagant puhverserveri lisage serdi **kobar** [Ressursi Tippige jaotise](../resource-group-authoring-templates.md) atribuudi httpApplicationGatewayCertificate

    Jaoks apiVersion enne 2016-09-01' sert on tähistatud parameetri nimi ***httpApplicationGatewayCertificate***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Jaoks apiVersion's sisse- või pärast 2016-09-01' sert on tähistatud parameetri nimi ***reverseProxyCertificate***
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Järgmised sammud
 - Näide HTTP suhtlemine teenuste [valimi projekti github](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount)kuvamine

 - [Kaugprotseduurikutse kõned usaldusväärsed teenused remoting abil](service-fabric-reliable-services-communication-remoting.md)

 - [Veebi-API, mis kasutab OWIN usaldusväärsed teenused](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-i suhtlus, usaldusväärseid teenuseid kasutades](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
