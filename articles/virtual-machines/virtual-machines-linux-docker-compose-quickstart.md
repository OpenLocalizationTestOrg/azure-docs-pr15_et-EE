<properties
   pageTitle="Keskmise suurusega ja koostamine virtual arvutisse | Microsoft Azure'i"
   description="Tutvustuse koostamine ja keskmise suurusega Linux virtuaalmasinates Azure töötamine"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Keskmise suurusega ja koostamine määratlemine ja käivitada mitme container Azure virtuaalne arvutis kasutamise alustamine

Määratlemine ja Linux virtuaalse masina Azure'i keerukate rakenduse käitamist keskmise suurusega ja [koostamine](http://github.com/docker/compose) kasutamise alustamine. Koostamine, kus saate määratleda rakenduse koosneb mitmest keskmise suurusega ümbriste lihtsa teksti faili. Mida siis spinnernupu üles ühe käsu, mis teeb kõik juurutamine määratletud keskkonna rakenduse. 

Näiteks selles artiklis kirjeldatakse kiiresti seadistamise WordPress ajaveebi koos mõne kirjutamata MariaDB SQL-andmebaasi mõne Ubuntu VM. Koostamine abil saate häälestada veelgi keerukamat rakendusi.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Samm 1: Linux VM keskmise suurusega hostiks häälestamine

Saate mitmesuguste Azure toimingute ja saadaval pilte või ressursihaldur Mallid Azure'i turuplatsil Linux VM loomiseks ja keskmise suurusega hostiks häälestamine. Näiteks vt [keskmise suurusega VM laiend juurutada keskkonna abil](virtual-machines-linux-dockerextension.md) kiiresti luua ka Ubuntu VM Azure keskmise suurusega VM laiendiga [Kiirjuhend malli](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)abil. 

Kui kasutate keskmise suurusega VM laiend, teie VM automaatselt häälestada keskmise suurusega hostiks ja koostamine on juba installitud. Näide selles artiklis kirjeldatakse, kuidas kasutada [Azure käsurea liides Mac, Windows ja Linux,](../xplat-cli-install.md) (Azure'i CLI) ressursihaldur režiimis VM loomiseks.

Loob lihtsa käsk eelneva dokumendi nimega ressursirühma `myResourceGroup` sisse selle `West US` asukoht ja kasutab VM Azure keskmise suurusega VM laiendiga installitud:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Samm 2: Veenduge, et koostamine on installitud

Kui juurutamise on lõpule jõudnud, nime SSH oma uue keskmise suurusega Host DNS-i abil saate juurutamise ajal. Saate kasutada `azure vm show -g myDockerResourceGroup -n myDockerVM` üksikasjade vaatamiseks oma VM, sh DNS-i nimi.

Kontrollige, et VM koostamine on installitud, käivitage järgmine käsk:

```bash
docker-compose --version
```

Näete väljundi sarnane `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Kui kasutasite luua keskmise suurusega host ja koostamine installimiseks on vaja mõnda muud meetodit, leiate [dokumentide koostamine](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Samm 3: Looge keskmise suurusega-compose.yml

Järgmine loomist on `docker-compose.yml` faili, mis on lihtsalt konfiguratsiooni tekstifail, määratleda keskmise suurusega ümbriste käitamist VM. Faili saate määrata käivitamiseks klõpsake iga pilti (või see võib olla mõne Dockerfile kaudu ehitada) keskkond, mis on vajalikud muutujate ja sõltuvused, portide ja ümbriste vahelisi seoseid. Yml faili süntaksi kohta täpsema teabe saamiseks vt [koostamine faili viide](http://docs.docker.com/compose/yml/).

Looge selle `docker-compose.yml` faili järgmiselt:

```bash
touch docker-compose.yml
```

Kasutage oma lemmik tekstiredaktorit faili lisamiseks mõned andmed. Järgmises näites kasutatakse funktsiooni `vi` editor:

```bash
vi docker-compose.yml
```

Järgmises näites kleepige oma tekstifail. Selle konfiguratsiooni kasutab kujutised [DockerHub registri](https://registry.hub.docker.com/_/wordpress/) installida WordPress (avatud allika ajaveebinduse ja sisu haldamine süsteem) ja lingitud kirjutamata MariaDB SQL-andmebaasi. Sisestage oma `MYSQL_ROOT_PASSWORD` järgmiselt:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Samm 4: Koostamine on ümbriste alustamine

Sama kataloogis oma `docker-compose.yml` faili, käivitage järgmine käsk (olenevalt teie keskkonnast võib peate käivitama `docker-compose` abil `sudo`.):

```bash
docker-compose up -d

```

See käsk käivitab keskmise suurusega ümbriste, mis on määratud `docker-compose.yml`. Kulub minut või kahe toimingu lõpuleviimiseks. Kuvatakse väljund sarnaneb järgmises näites:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Kasutage käivitamisel suvandi **-d** nii, et selle ümbriste taustal käitada pidevalt kindlasti.

Veenduge, et selle ümbriste on üles, tippige `docker-compose ps`. Peaksite nägema umbes:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Nüüd saate luua ühenduse WordPress otse VM port 80 kohta. Avage veebibrauser ja sisestage oma VM DNS-i nimi (nt `http://myresourcegroup.westus.cloudapp.azure.com`). Nüüd peaks nähtaval olema WordPress avakuva, kus saate installimise lõpuleviimiseks ja rakenduse kasutamise alustamine.

![WordPress avakuva][wordpress_start]


## <a name="next-steps"></a>Järgmised sammud

* Minge [keskmise suurusega VM laiend kasutusjuhendi](https://github.com/Azure/azure-docker-extension/blob/master/README.md) lisavõimaluste konfigureerida oma keskmise suurusega VM keskmise suurusega ja koostamine. Üheks võimaluseks on näiteks koostamine yml faili (teisendatakse JSON) paigutada otse keskmise suurusega VM laiend konfiguratsioonis.
* Tutvuge [koostamine käsurea viide](http://docs.docker.com/compose/reference/) ja [kasutusjuhendi](http://docs.docker.com/compose/) veel näiteid koostamise ja juurutamine mitme container rakendused.
* Azure'i ressursihaldur malli, kas kasutamiseks teie enda või ühe kaasa [ühenduse](https://azure.microsoft.com/documentation/templates/)juurutamiseks on Azure VM keskmise suurusega ja rakenduse koostamine häälestatud. Näiteks [Deploy WordPress ajaveebi ja keskmise suurusega](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) Mall kasutab keskmise suurusega ja koostamine kiiresti juurutada WordPress koos MySQL-i kirjutamata on Ubuntu VM.
* Proovige integreerimise [Keskmise suurusega sülem](virtual-machines-linux-docker-swarm.md) kobar keskmise suurusega koostamine. Lugege teemat [Abil koostada koos sülem](https://docs.docker.com/compose/swarm/) stsenaariumi.

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
