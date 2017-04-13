<properties
    pageTitle="Keela SSH paroolid oma Linux VM SSHD konfigureerimisega | Microsoft Azure'i"
    description="Secure oma Linux VM Azure keelamisega parooliga sisselogimise SSH jaoks."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Keela SSH paroolid oma Linux VM SSHD konfigureerimise abil

See artikkel keskendub lukustada oma Linux VM turvalisus sisselogimise kohta.  Kui SSH port 22 on avatud maailma eest Start üritab sisselogimise paroolid ära.  Mida teeme selle artikli on parooliga sisselogimise keelamine üle SSH.  Võimalus kasutada paroolide kaotamise kaitseme Linuxi seda tüüpi Jõuvõtete rünnak.  Lisatud boonus on configure Linux SSHD lubama ainult sisselogimise kaudu SSH avalike ja privaatvõ klahvid kaugelt kõige turvalisem võimalus Linuxi sisselogimiseks.  Võimalikud kombinatsioonid nõuaks privaatvõti on suur ja seetõttu mitte eest isegi häirib jõuvõtete SSH klahvid proovida mõelda.


## <a name="goals"></a>Eesmärgid

- Konfigureerige SSHD keelamiseks.
  - Parooliga sisselogimise
  - Juurkausta kasutaja sisselogimine
  - Probleem – vastuse autentimine
- Kui soovite lubada SSHD konfigureerida.
  - ainult SSH võtme logimine
- Taaskäivitage SSHD olles endiselt sisse logitud
- Uue SSHD konfiguratsiooni testimiseks

## <a name="introduction"></a>Sissejuhatus

[SSH määratletud](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD on SSH Server, mis töötab Linux VM.  SSH on klient, mis töötab shell kaudu oma töökoha MacBook või Linuxi.  SSH protokolli kasutatakse ka turvata ja krüptimine suhtlemine oma töökoha ja Linux VM.

See artikkel on väga oluline hoida ühes sisselogimiseks oma Linux VM avada kogu Jalutuskäik läbi.  Seetõttu avame kaks terminalid ja SSH Linux VM nii neid.  Kasutame esimese terminalis SSHDs konfiguratsiooni fail soovitud muudatused ja taaskäivitage SSHD.  Kasutame teine terminal testimiseks neid muudatusi, kui teenuse taaskäivitamist.  Kuna me on keelamine SSH paroolid ja tuginedes tingimata SSH klahvid, kui teie SSH klahvid pole õige ja sulgege VM ühendus, VM jäädavalt lukus ja seda ei saaks seda nõudva kustutatud ja uuesti sisse logida.

## <a name="prerequisites"></a>Eeltingimused

- [Luua SSH võtmed Mac ja Linux Linuxi vms Azure](virtual-machines-linux-mac-create-ssh-keys.md)
- Azure'i konto
  - [tasuta prooviversiooni kasutajaks registreerumist](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure'i portaal](http://portal.azure.com)
- Linux VM azure töötab
- SSH avalike ja privaatvõ paari rakenduses`~/.ssh/`
- SSH avalik võti `~/.ssh/authorized_keys` Linux VM
- VM sudo õigusi
- Pordi 22 avamine

## <a name="quick-commands"></a>Kiirülevaate käsud

_Kogenud Linux administraatoritele, kes soovivad lihtsalt TLDR versioon, alustage siit.  Kõigile, kes soovib üksikasjaliku selgituse ja kõndida läbi selle sammu vahele._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Üksikasjalik kõndida läbi

Logi Linux VM terminal 1 (T1).  Logi Linux VM terminal 2 (T2).

Klõpsake T2 me SSHD konfiguratsiooni faili redigeerimiseks.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Siin on me redigeerida ainult sätteid keelata paroolid ja SSH võtme sisselogimise lubamine.  On palju sätete teha Linux & SSH turvaline kui teil on vaja seda faili, mis teil peaks teadus- ja muutmine.

#### <a name="disable-password-logins"></a>Parooliga sisselogimise keelamine

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Avalik võti autentimine

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Keelake juurkausta sisselogimine

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Probleem – vastuse autentimise keelamine

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Taaskäivitage SSHD

Veenduge, et teil on ikka sisseloginud T1 shell.  See on oluline, et te ei saada lukustades oma VM kui teie SSH klahvid pole õige, kuna paroolid on praegu keelatud.  Kui mis tahes säte on vale oma Linux VM T1 abil saate lahendada sshd_config, kui te endiselt logitakse ja SSH hoiab ühenduse elus SSHD töö käigus uuesti.

Kaudu käivitada T2:

##### <a name="on-the-debian-family"></a>Debian pere

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Pere RedHat

```
username@macbook$ sudo service sshd restart
```

Paroolide keelatakse nüüd teie kaitsmine jõuvõtete parooliga sisselogimise katsete VM.  Ainult SSH abil lubatud, mida saab logida kiirem ja palju turvalisemaks.
