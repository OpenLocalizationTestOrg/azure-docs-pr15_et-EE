<properties
   pageTitle="Azure'i teenus container haldus – keskmise suurusega sülem | Microsoft Azure'i"
   description="Keskmise suurusega sülem teenuses Azure Container ümbriste juurutamine"
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

# <a name="container-management-with-docker-swarm"></a>Container haldus – keskmise suurusega sülem

Keskmise suurusega sülem pakub keskkonnas üle keskmise suurusega hosts ühise kogumi konteinerite töökoormus kasutamise kohta. Keskmise suurusega sülem kasutab kohalikke keskmise suurusega API. Töövoo haldamise kohta on keskmise suurusega sülem ümbriste on identne mis on ühe container hosti. Selles dokumendis on lihtne näiteid konteinerite töökoormus eksemplariga Azure'i teenus keskmise suurusega sülem juurutamine. Leiate põhjalikumat dokumentatsiooni kohta keskmise suurusega sülem [Keskmise suurusega sülem Docker.com kohta](https://docs.docker.com/swarm/).

Selles dokumendis harjutused eeltingimused

[Azure'i teenus sülem kobar loomine](container-service-deployment.md)

[Sülem kobar teenuses Azure Container kasutajaga](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Uus ümbris juurutamine

Keskmise suurusega Disney luua uus ümbris, kasutage funktsiooni `docker run` käsk (tagada, et olete avanud mõne SSH tunneliga meistrid ühe ülaltoodud eeltingimused). Selles näites luuakse ümbris kaudu soovitud `yeasy/simple-web` pilt:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Pärast loomist ümbris, kasutage `docker ps` ümbris kohta käiva teabe tagastamiseks. Pange tähele siin, et sülem agent, mis majutab ümbris on loetletud.


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Nüüd pääsete rakendus, mis töötab selles ümbrises kaudu sülem agent laadi koormusetasakaalustusteenuse avaliku DNS-i nimi. Selle teabe leiate Azure'i portaalis:  


![Reaal külastada tulemused](media/real-visit.jpg)  

Vaikimisi on laadi koormusetasakaalustusteenuse pordid 80, 443 ja 8080 Ava. Kui soovite luua ühenduse mõne muu pordi peate avamiseks selle porti Azure'i laadi koormusetasakaalustusteenuse Agent Pool.

## <a name="deploy-multiple-containers"></a>Mitme ümbriste juurutamine

Nagu mitme ümbriste käivitatud, täites 'keskmise suurusega Käivita' mitu korda, saate kasutada funktsiooni `docker ps` käsu kuvamiseks, mis majutab soovitud ümbriste töötavad. Alltoodud näites kolme ümbriste on ühtlaselt kolme sülem agentide:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Ümbriste juurutada keskmise suurusega koostamine

Keskmise suurusega koostamine abil automatiseerida juurutus- ja mitme ümbriste konfigureerimine. Selleks, tagada turvaline Shell (SSH) tunneliga loodud ja et DOCKER_HOST muutuja on määratud (vt ülal eeltingimused).

Keskmise suurusega-compose.yml kohalikust faili loomine. Selleks, kasutage selle [valimi](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Käivitage `docker-compose up -d` container juurutuste alustamiseks:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Lõpetuseks, tagastatakse nimekirja ümbriste töötavad. See loend kajastab keskmise suurusega koostamine abil juurutatud ümbriste:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Loomulikult, mille abil saate `docker-compose ps` uurida ainult ümbriste, mis on määratletud teie `compose.yml` faili.

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet keskmise suurusega sülem](https://docs.docker.com/swarm/)
