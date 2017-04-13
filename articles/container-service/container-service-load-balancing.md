<properties
   pageTitle="Laadi saldo ümbriste mõne Azure'i teenus klaster | Microsoft Azure'i"
   description="Laadi saldo üle mitme ümbriste soovitud klaster Azure'i teenus."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Ümbriste mikro-teenuseid, näiteks Põhiliselt/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Laadi saldo ümbriste soovitud klaster Azure'i teenus

Selles artiklis uurime, kuidas luua ka sisemise koormuse koormusetasakaalustusteenuse rakenduses on näiteks Põhiliselt/OS hallatavate Azure'i teenus maraton-LB abil. See võimaldab teil mastaapimiseks rakenduste horisontaalselt. See võimaldab teil ära avalike ja privaatvõ agendi kogumite pannes oma koormus soolise avaliku kobar ja oma rakenduse ümbriste privaatne klaster.

## <a name="prerequisites"></a>Eeltingimused

[Azure'i teenus eksemplari Deploy](container-service-deployment.md) orchestrator tüüpi AV/OS ja [veenduge, et teie klient saab ühendada klaster](container-service-connect.md). 

## <a name="load-balancing"></a>Laadi tasakaalustamiseks

Loome klaster teenus on kaks koormust tasakaalustavad kihti. 

  1. Azure'i laadi koormusetasakaalustusteenuse rakendusel avaliku kirje (need, mis on pihta lõppkasutajad). See on esitatud Azure'i teenus automaatselt ja on vaikimisi konfigureeritud esitamist pordi 80, 443 ja 8080.
  2. Laadi maraton koormusetasakaalustusteenuse (maraton-lb) marsruudib sissetulevad taotlused container eksemplarid, mis neid hooldustaotlused. Kui me mastaapimiseks ümbriste, pakkudes meie veebiteenuse, kohandub dünaamiliselt maraton-lb. Selle laadi koormusetasakaalustusteenuse vaikimisi Container teenust ei esitata, kuid see on väga lihtne installida.

## <a name="marathon-load-balancer"></a>Maraton laadi koormusetasakaalustusteenuse

Maraton laadi koormusetasakaalustusteenuse dünaamiliselt ümber konfigureeritud ise olete juurutatud ümbriste põhjal. See on olles ümbris või agent - ka siis, kui see juhtub, Apache Mesos lihtsalt uuesti container mujal ja maraton-lb kohaneda.

Installimiseks saate kasutada kas AV/OS maraton laadi koormusetasakaalustusteenuse web UI või käsurea.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Installige maraton-LB AV/OS Web Kasutajaliidese abil

  1. Klõpsake nuppu "Universumis"
  2. 'Maraton-LB' otsimine
  3. Klõpsake nuppu Installi"

![Installimise maraton lb AV/OS Web Interface](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Installige maraton-LB AV/OS CLI abil

Pärast installimist AV/OS CLI ja tagada, et te saate ühendada klaster, käivitage järgmine käsk oma kliendi seadme kaudu.

```bash
dcos package install marathon-lb
```

See käsk automaatselt installib laadi koormusetasakaalustusteenuse avalike kobar.

## <a name="deploy-a-load-balanced-web-application"></a>Juurutamine koormus tasakaalustatud veebirakenduse

Nüüd kus oleme maraton-lb paketi, saame kasutada rakenduse ümbris, mille soovime laadimiseks saldo. Selle näite puhul me juurutada lihtne veebiserver järgmist konfiguratsiooni abil:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Väärtus `HAProxy_0_VHOST` FQDN laadi koormusetasakaalustusteenuse oma töötajatele. See on kujul `<acsName>agents.<region>.cloudapp.azure.com`. Näiteks kui loote teenus kobar nimega `myacs` piirkonnas `West US`, FQDN oleks `myacsagents.westus.cloudapp.azure.com`. Leiate ka see otsib laadi koormusetasakaalustusteenuse "agent" nimi, kui otsite ressursside jaotises teenus [Azure portaali](https://portal.azure.com)loodud ressurssidest.
  * Määrake soovitud servicePort porti > = 10 000. See tuvastab teenus, mis käitatakse selles ümbrises--maraton-lb kasutab seda teenuseid, et see peaks jääk üle tuvastamiseks.
  * Määrake soovitud `HAPROXY_GROUP` sildi juurde "välise".
  * Määrake `hostPort` 0. See tähendab, et maraton meelevaldselt eraldab vaba port.
  * Määrake `instances` loodava eksemplaride arv. Saate alati muudate need üles hiljem.

Tasub noing, et maraton saavad võtta kasutusele privaatne kobar vaikimisi see tähendab, et ülaltoodud juurutamise ainult kaudu oma laadi koormusetasakaalustusteenuse, mis on tavaliselt soovime käitumise.

### <a name="deploy-using-the-dcos-web-ui"></a>Juurutada, näiteks Põhiliselt/OS Web Kasutajaliidese abil

  1. Külastage http://localhost/marathon maraton lehte (pärast [SSH tunneliga](container-service-connect.md) ja klõpsake nuppu häälestamise`Create Appliction`
  2. Klõpsake soovitud `New Application` klõpsake dialoogiboksi `JSON Mode` paremas ülanurgas
  3. Kleepige ülal JSON redaktori
  4. Klõpsake nuppu`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Kasutades näiteks Põhiliselt/OS CLI juurutamine

Juurutada selle rakenduse AV/OS CLI lihtsalt kopeerige ülaltoodud JSON fail nimega `hello-web.json`, ja käivitage:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure'i laadi koormusetasakaalustusteenuse

Vaikimisi Azure'i laadi koormusetasakaalustusteenuse seab pordid 80, 8080 ja 443. Kui kasutate ühte nende kolme portide (nagu Ülaltoodud näites), siis pole midagi, mida peate tegema. Peaks oskama tabas oma agent laadi koormusetasakaalustusteenuse FQDN - ja iga kord, kui proovite värskendada, kuvatakse tulemus üks kolmest veebi serveri round-jaan mood. Siiski, kui kasutate erinevat porti, peate lisama round-jaan reegli ja mõne juures klõpsake Laadi koormusetasakaalustusteenuse pordi, mida kasutasite. Selleks [Azure'i CLI](../xplat-cli-azure-resource-manager.md), käsud `azure network lb rule create` ja `azure network lb probe create`. Te saate seda teha ka Azure'i portaalis.


## <a name="additional-scenarios"></a>Täiendavad stsenaariumid

Kui kasutate erinevate domeenide esitamist erinevad teenused stsenaarium võib teil olla. Näiteks:

mydomain1.com -> Azure'i LB:80 -> maraton-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure'i LB:80 -> maraton-lb:10002 -> mycontainer2:22321

Selleks vaadake [virtuaalse hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), mis võimalda seostamiseks domeenide teatud maraton-lb teed.

Teise võimalusena võib nähtavaks tegemine eri pordid ja Kaardistage need uuesti taha maraton-lb õige teenusesse. Näiteks:

Azure'i lb:80 -> maraton-lb:10001 -> mycontainer:233423  
Azure'i lb:8080 -> maraton-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Järgmised sammud

Lisateavet sees [maraton-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/)dokumentatsioonist näiteks Põhiliselt/OS.
