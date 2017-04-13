<properties
   pageTitle="Azure'i teenus container halduse REST API kaudu | Microsoft Azure'i"
   description="Ümbriste juurutamiseks on Azure Container teenuse Mesos kobar maraton REST API abil."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Keskmise suurusega, ümbriste, mikro-teenuste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-rest-api"></a>Container halduse REST API kaudu

Näiteks Põhiliselt/OS pakub juurutamine ja skaleerimist rühmitatud töökoormus, samal ajal niisutussüsteemide aluseks oleva riistvara keskkonnas. AV/OS peal on framework, haldab plaanimis- ja täitmise Arvuta töökoormus.

Kuigi raamistiku on saadaval paljude populaarsete töökoormus, selle dokumendi kirjeldatakse, kuidas saate luua ja mastaapimiseks container juurutuste maraton abil. Töö kaudu nendes näidetes, peate AV/OS klaster, mis on konfigureeritud Azure'i teenus. Peate olema selle arvutikobaras remote connectivity. Nendeks kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Azure'i teenus on kobar juurutamine](container-service-deployment.md)
- [Ühenduse loomine mõne Azure'i teenus kobar](container-service-connect.md)

Kui olete loonud ühenduse Azure'i teenus kobar, pääsete AV/OS- ja seotud REST API-de http://localhost:local porti. Näited selles dokumendis Oletagem, et teil on tunneling port 80. Näiteks maraton lõpp-punkti pääseb `http://localhost/marathon/v2/`. Erinevate API-de kohta leiate lisateavet teemast mesosfäärini dokumentatsioonist [Maraton API](https://mesosphere.github.io/marathon/docs/rest-api.html) ja [Chronos API](https://mesos.github.io/chronos/docs/api.html)ja [Mesos ajasti API](http://mesos.apache.org/documentation/latest/scheduler-http-api/)Apache dokumentatsioonist.

## <a name="gather-information-from-dcos-and-marathon"></a>Näiteks Põhiliselt/OS ja maraton teabe kogumine

Enne juurutamist ümbriste AV/OS klaster, Koguge näiteks Põhiliselt/OS kobar, nimed, näiteks Põhiliselt/OS agentide praeguse oleku kohta. Päringu selleks funktsiooni `master/slaves` AV/OS REST API lõpp-punkti. Kui kõik läheb kuvatakse iga loendi ja AV/OS agentide mitu atribuudid.

```bash
curl http://localhost/mesos/master/slaves
```

Nüüd kasutada maraton `/apps` lõpp-punkti praeguse rakenduse kasutuselevõttu AV/OS klaster kontrollimiseks. Kui see on uus klaster, kuvatakse tühi massiivi rakendused.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Keskmise suurusega vormindatud container juurutamine

Keskmise suurusega vormindatud ümbriste kaudu maraton juurutamist ettenähtud juurutamise kirjeldav JSON-faili abil. Järgmises näites kuvatakse juurutada Nginx ümbris, näiteks Põhiliselt/OS agent ümbris pordiga 80 sidumine pordi 80. Samuti võtke arvesse, et 'acceptedResourceRoles' atribuudi väärtuseks on seatud "slave_public". See on juurutada ümbris agent avalikkusele suunatud agent skaala määramine.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Keskmise suurusega vormindatud container juurutada, et oma JSON-faili loomiseks või kasutada [Azure'i teenus](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)demo esitatud valimi. Talletage see puuetega inimestele juurdepääsetavate asukoht. Järgmiseks juurutada ümbris, käivitage järgmine käsk. Määrake JSON-faili nimi.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Väljund on järgmine:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Nüüd, kui saate päringu maraton rakenduste, kuvatakse uus taotlus väljund.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Teie ümbriste skaala

Saate kasutada ka maraton API mastaapimiseks välja või mastaapimiseks klõpsake rakenduse kasutuselevõttu. Eelmises näites, saate juurutada rakenduse ühe eksemplari. Vaatame skaala selle välja rakenduse kolm eksemplari. Selleks, JSON faili järgmine tekst JSON abil luua ja salvestada selle puuetega inimestele juurdepääsetavate asukoht.

```json
{ "instances": 3 }
```

Käivitage järgmine käsk välja rakenduse skaala.

>[AZURE.NOTE] URI saab http://localhost/marathon/v2/apps/ ja seejärel mastaapimiseks rakenduste ID-d. Kui kasutate Nginx proovi, mis on esitatud allpool, oleks URI http://localhost/marathon/v2/apps/nginx.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Lõpuks päringu rakenduste maraton lõpp-punkti. Näete, et praegu on kolm Nginx ümbriste.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>PowerShelli kasutamine selle ülesande jaoks: maraton REST API suhtluse PowerShelli abil

Windowsi süsteem PowerShelli käskude abil saate teha neid samu toiminguid.

Koguge kokku teave, näiteks Põhiliselt/OS kobar, nagu agent nimed ja agent olek, käivitage järgmine käsk.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Keskmise suurusega vormindatud ümbriste kaudu maraton juurutamist ettenähtud juurutamise kirjeldav JSON-faili abil. Järgmises näites kuvatakse juurutada Nginx ümbris, näiteks Põhiliselt/OS agent pordi 80 ümbrisest sidumine pordi 80.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Oma JSON-faili loomiseks või kasutada [Azure'i teenus](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)demo esitatud valimi. Talletage see puuetega inimestele juurdepääsetavate asukoht. Järgmiseks juurutada ümbris, käivitage järgmine käsk. Määrake JSON-faili nimi.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Saate kasutada ka maraton API mastaapimiseks välja või mastaapimiseks klõpsake rakenduse kasutuselevõttu. Eelmises näites, saate juurutada rakenduse ühe eksemplari. Vaatame skaala selle välja rakenduse kolm eksemplari. Selleks, JSON faili järgmine tekst JSON abil luua ja salvestada selle puuetega inimestele juurdepääsetavate asukoht.

```json
{ "instances": 3 }
```

Käivitage järgmine käsk välja rakenduse skaala.

> [AZURE.NOTE] URI saab http://localhost/marathon/v2/apps/ ja seejärel mastaapimiseks rakenduste ID-d. Kui kasutate siin esitatud Nginx valimi, oleks URI http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Järgmised sammud

- [Lugege lisateavet Mesos HTTP lõpp-punktid]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Lugege lisateavet maraton REST API -ga]( https://mesosphere.github.io/marathon/docs/rest-api.html).
