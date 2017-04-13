<properties
   pageTitle="Installige näiteks Põhiliselt/OS CLI | Microsoft Azure'i"
   description="Installige näiteks Põhiliselt/OS CLI."
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
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] See on näiteks Põhiliselt/OS-ga ACS kogumite töötamiseks. Ei ole vaja selleks sülem vastavalt ACS kogumite.

Esmalt [ühenduse klaster ACS AV/OS-ga](../articles/container-service/container-service-connect.md). Kui te seda teinud, saate installida näiteks Põhiliselt/OS CLI oma kliendi seadme järgmised käsud:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Kui kasutate Python vanemat versiooni, võite märgata teatud "InsecurePlatformWarnings". Turvaline võite ignoreerida neid.

Selleks, et alustada ilma oma shell taaskäivitada, käivitage:

```bash
source ~/.bashrc
```

Seda toimingut ei ole vaja uue kestad käivitamisel.

Nüüd saate kinnitada, et CLI on installitud:

```bash
dcos --help
```