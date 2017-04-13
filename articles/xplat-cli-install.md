<properties
    pageTitle="Installige Azure'i käsurea liides | Microsoft Azure'i"
    description="Installige Azure'i käsurea liides (CLI) for Mac, Linux ja Windows Azure'i teenuste kasutamise alustamine"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Installige Azure'i CLI

> [AZURE.SELECTOR]
- [PowerShelli](powershell-install-configure.md)
- [Azure'i CLI](xplat-cli-install.md)

Kiiresti installige Azure'i käsurea liides (Azure'i CLI) kasutamist avatud lähtekoodi shell-põhiste käskude loomise ja haldamise ressursid Microsoft Azure. Teil on mitu võimalust nende tööriistade platvormidel oma arvutisse installida. 

* **npm paketi** - käivitada installige uusim Azure CLI pakett Linux jaotuse või OS npm (paketi manager JavaScript). Nõuab node.js ja npm teie arvutis.
* **Installer** - allalaadimine on lihtne installimine Mac-arvutisse või Windows Installeri.
* **Keskmise suurusega container** - uusim CLI valmis-to-run keskmise suurusega ümbrises kasutuselevõtt. Keskmise suurusega hosti arvuti nõuab.
    
Rohkem suvandeid ja tausta, lugege teemat projekti hoidla [github](https://github.com/azure/azure-xplat-cli). 

Kui Azure CLI on installitud, [ühendage see Azure'i tellimus](xplat-cli-connect.md) ja käivitage **Azure'i** käsud töötamiseks oma Azure ressursse oma käsurea liides (Bash, terminalis, Käsuviip ja jne).



## <a name="option-1-install-an-npm-package"></a>Variant 1: Installige npm alla

CLI kaudu installimiseks on npm paketi, veenduge, et alla laadinud ja installinud [uusimaid Node.js ja npm](https://nodejs.org/en/download/package-manager/). Käivitage **npm installida** azure-cli paketi installimiseks: 

    npm install -g azure-cli

Klõpsake Linuxi, peate **sudo** abil edukalt käivitada käsk __npm__ järgmiselt:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Kui teil on vaja installida või värskendada Node.js ja Linux jaotuse või OS npm, Soovitame installida Node.js LTS kõige uuem versioon (4.x). Kui kasutate vanemat versiooni, võidakse kuvada installitõrgetele. 

Kui eelistate, laadige viimane Linux [tar faili] [ linux-installer] npm paketi kohalikult. Seejärel installige allalaaditud npm paketi järgmised (võimalik, et peate kasutama **sudo**Linuxi).

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Variant 2: Kasuta

Kui kasutate Mac-arvutisse või Windowsiga arvutis, on allalaadimiseks saadaval järgmised CLI paigaldajaid.

* [Mac OS X installer][mac-installer]

* [Windowsi MSI][windows-installer] 

>[AZURE.TIP]Opsüsteemis Windows, saate ka [Web platvormi Installer](https://go.microsoft.com/?linkid=9828653) installida CLI alla laadida. See installer annab teile võimalust installida pärast installimist CLI täiendavad Azure'i SDK ja käsurea tööriistad. 


## <a name="option-3-use-a-docker-container"></a>Võimalus 3: Kasutada keskmise suurusega container

Kui teie arvuti [keskmise suurusega](https://docs.docker.com/engine/understanding-docker/) hostiks käivitada uusim Azure CLI keskmise suurusega ümbrises. (On võimalik, et peate kasutama **sudo**Linuxi) käivitage järgmine käsk:

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Azure'i CLI käsud
Pärast installimist Azure CLI käsu **Azure'i** oma käsurea kasutajaliidese (Bash, terminalis, Käsuviip ja jne). Näiteks Spikker käsk Käivita, tippige järgmist:

```
azure help
```
> [AZURE.NOTE]Mõned Linuxi, võidakse kuvada sarnane viga `/usr/bin/env: ‘node’: No such file or directory`. See tõrge on tehtud installimise Node.js /usr/bin/nodejs on arvutisse installitud. Seda parandada selle käsu abil /usr/bin/node sümboolse lingi loomiseks:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Leiate Azure'i CLI installitud versiooni, tippige järgmine:

```
azure --version
```

Nüüd olete valmis! Juurdepääsu CLI käsud oma ressursse, mis on [Azure tellimuse Azure'i CLI kaudu ühenduse](xplat-cli-connect.md)töötamiseks.

>[AZURE.NOTE] Kui kasutate Azure CLI, näete teadet, mis küsib, kas soovite lubada Microsoftil koguda kasutamise kohta. On vabatahtlik. Kui otsustate osaleda, võite peatada igal ajal käivitades `azure telemetry --disable`. Osalemine igal ajal käivitage `azure telemetry --enable`.


## <a name="update-the-cli"></a>CLI värskendamine

Microsoft välja sageli Azure'i CLI värskendatud versiooni. Uuesti, kasutades oma operatsioonisüsteemi installer CLI või käivitage uusimate keskmise suurusega ümbrises. Või kui teil on uusimaid Node.js ja npm installitud, värskendada, tippides (on võimalik, et peate kasutama **sudo**Linuxi) järgmist.

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Menüü lõpetamise lubamine

Menüü lõppu CLI käsud on toetatud Mac ja Linux.

Kui soovite lubada selle zsh, käivitage:

```
echo '. <(azure --completion)' >> .zshrc
```

Kui soovite lubada selle bash, käivitage:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Järgmised sammud 

* [Ühenduse loomine: Azure'i tellimuse CLI](xplat-cli-connect.md) luua ja hallata Azure ressursse.

* Azure'i CLI kohta lisateabe saamiseks laadige lähtekoodi, probleemidest, või aidata projekti saamiseks külastage [Azure'i CLI GitHub hoidla](https://github.com/azure/azure-xplat-cli).

* Kui teil on Azure CLI või Azure kasutamise kohta küsimusi, külastage [Azure'i foorumeid](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Kui soovite, võite ka Python-põhiste [Azure'i CLI 2.0 eelvaade](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
