<properties 
    pageTitle="Valides kasutajanimed Linuxi | Microsoft Azure'i" 
    description="Saate teada, kuidas valige Azure kasutajanimed virtuaalse masina Linuxi jaoks." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



#<a name="selecting-user-names-for-linux-on-azure"></a>Kasutajanimed Linuxi Azure valimine#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Kui olete ette Linux virtuaalse masina Azure Määrake-root kasutaja, mille abil saate hiljem sisse logida VM nimi. Võite valida uue kasutaja nimi või ettevalmistamise Azure klassikaline portaali kaudu saate aktsepteerida vaikimisi nime "azureuser".

Enamikul juhtudel selle kasutaja ei eksisteeri pilti ja ebausaldusväärsete käigus luuakse. Kui kasutaja on olemas VM pilti, siis Azure Linux agent lihtsalt konfigureerib parooli ja/või SSH võti sellele kasutajale määratud VM loomisel teabe põhjal.

**Aga**Linux määratleb hulga kasutajate nimed, mis ei tohi kasutada. Ebausaldusväärsete kuvatakse **nurjuda,** kui proovite ettevalmistamise Linux VM abil olemasoleva süsteemi kasutaja, mis on määratletud kasutaja UID 0 – 99. Tüüpiline näide on selle `root` kasutaja, kes on UID 0.

 - Vt ka: [Linuxi Standard Base - kasutaja ID vahemikud](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Järgmises loendis levinud sisseehitatud kasutajaid CentOS ja Ubuntu, et tuleks vältida kui ettevalmistamise Linux virtuaalse masina Azure abil. Selles loendis on lihtsalt näide, vaadake oma jaotuse tagamaks, et valite kasutajanimi vastuolus olemasoleva süsteemi kasutaja dokumentatsioonist.


## <a name="centos"></a>CentOS
- ABRT
- adm
- heli
- Prügikast
- CDROM
- cgred
- Daemon
- DBus
- dialout
- DIP
- ketas
- floppy
- FTP
- FTP
- visualiseerimine
- Gopher
- haldaemon
- peatada
- KMEM
- lukustamine
- LP
- e-posti
- mees
- mälu
- nfsnobody
- keegi ei
- NTP
- tehtemärk
- oprofile
- postdrop
- Postfix
- qpidd
- root
- RPC
- rpcuser
- saslauth
- sulgemine
- slocate
- SSHD
- stapdev
- stapusr
- sünkroonimine
- sys
- lint
- test
- tcpdump
- TTY
- kasutajatele
- utempter
- utmp
- uucp
- vcsa
- Video
- ratta


## <a name="ubuntu"></a>Ubuntu
- adm
- administraator
- heli
- varundus
- Prügikast
- CDROM
- crontab
- Daemon
- dialout
- DIP
- ketas
- faksi
- floppy
- kaitse
- visualiseerimine
- sääski
- IRC
- KMEM
- horisontaalpaigutus
- libuuid
- loend
- LP
- e-posti
- mees
- messagebus
- mlocate
- netdev
- Uudised
- keegi ei
- nogroup
- tehtemärk
- plugdev
- puhverserver
- root
- SASL
- varju
- src
- SSH
- SSHD
- töötajate
- sudo
- sünkroonimine
- sys
- Logi
- lint
- TTY
- kasutajatele
- utmp
- uucp
- Video
- Kõnepost
- whoopsie
- ilma www-andmed

 