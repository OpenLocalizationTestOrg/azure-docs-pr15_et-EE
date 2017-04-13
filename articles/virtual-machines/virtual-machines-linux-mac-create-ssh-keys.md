<properties
    pageTitle="Luua SSH klahvid Linux ja Mac-arvutis | Microsoft Azure'i"
    description="Luua ja neid kasutada SSH klahvid ressursihaldur ja klassikaline juurutamise mudelite Azure Mac ja Linux."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Luua SSH võtmed Mac ja Linux Linuxi vms Azure

Mõne SSH keypair saate luua Virtuaalmasinates Azure, mis vaikimisi SSH klahvide abil autentimise kõrvaldada paroolid sisse logida.  Paroole saab arvasid ja avage oma VMs kuni järeleandmatu jõuvõtete katsete paroole. Azure'i mallide abil loodud VMs või `azure-cli` saate lisada oma SSH avalik võti juurutamise, eemaldamise postitus juurutuse konfigureerimise osana.  Kui loote Linux VM Windows, lugege teemat [selles dokumendis.](virtual-machines-linux-ssh-from-windows.md)

See artikkel nõuab tehke järgmist.

- Azure'i konto ([saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure'i CLI](../xplat-cli-install.md) sisse logitud`azure login`

- Azure'i CLI _peab olema_ Azure'i ressursihaldur režiim`azure config mode arm`

## <a name="quick-commands"></a>Kiirülevaate käsud

Järgmised käsud, asendage näiteid oma valikud.

SSH võtmed on vaikimisi hoida funktsiooni `.ssh` kataloogi.  

```bash
cd ~/.ssh/
```

Kui teil on mõne `~/.ssh` directory soovitud `ssh-keygen` käsk luua selle ning vajalikke õigusi.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Sisestage nimi, mis on salvestatud fail soovitud `~/.ssh/` kataloog:

```bash
id_rsa
```

Sisestage parool id_rsa:

```bash
correct horse battery staple
```

Nüüd on `id_rsa` ja `id_rsa.pub` SSH võtme paari soovitud `~/.ssh` kataloogi.

```bash
ls -al ~/.ssh
```

Lisage vastloodud võti `ssh-agent` Linux ja Mac-arvutisse (lisada ka OSX võtmekimbu):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Kopeerige oma Linuxi serverisse SSH avalik võti.

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Klahvide abil parooli asemel login testimiseks tehke järgmist.

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Üksikasjaliku selgituse

SSH avalike ja privaatvõ klahvide abil on kõige lihtsam viis oma Linux serverid sisse logida. [Avaliku võtme krüptograafia](https://en.wikipedia.org/wiki/Public-key_cryptography) võimaldab palju turvalisemaks sisse logima oma Linux või BSD VM Azure kui paroole, mis võib olla palju kergemini brute sunnitud. Oma avalik võti saab jagada kõigiga; kuid ainult teie (või teie kohaliku infrastruktuuri) on teie isiklik võti.  SSH privaatvõti võiks olla [väga turvaline parool](https://www.xkcd.com/936/) (Allikas:[xkcd.com](https://xkcd.com)) seda kaitsta.  See parool on lihtsalt juurdepääs SSH privaatvõti ja **pole** kasutaja konto parool.  Kui lisate parooli SSH tootevõti, krüptib privaatvõti nii, et privaatvõti on kasutuks ilma selle avamiseks parooli.  Kui ründaja varastas oma privaatvõti ja selle klahvi ei ole parooli, need oleks võimalik, et kasutada seda privaatvõti sisse logida mis tahes serverid, mis on vastava avalik võti.  Kui privaatvõti on parooliga kaitstud, seda ei saa kasutada seda ründaja, pakkudes on täiendavad layer väärtpaberi oma taristu Azure.

Selles artiklis loob *ssh-rsa* vormindatud olulisi faile, mis on soovitatav juurutuste ressursihaldur kohta.  [portaali](https://portal.azure.com) klassikaline nii ressursihaldur juurutuste peavad *SSH-rsa* võtmed.

## <a name="create-the-ssh-keys"></a>Looge selle SSH võtmed

Azure'i jaoks on vaja vähemalt 2048-bitine ssh-rsa vormindamine avalike ja privaatvõ võtmed. Klahvid kasutamine loomiseks `ssh-keygen`, mis palub sarja küsimused ja siis kirjutab isikliku ja vastavaid avalik võti. Avalik võti kopeeritakse ka Azure VM loomisel `~/.ssh/authorized_keys`.  SSH võtmed `~/.ssh/authorized_keys` kasutatakse vaidlustada kliendi vastava isikliku võtme SSH login ühendus vastavaks.

## <a name="using-ssh-keygen"></a>Kasutades ssh-keygen

See käsk loob turvatud (krüptitud) SSH Keypair 2048-bitine RSA abil parooli ja see on kommenteeritud hõlpsasti kindlaks teha.  

Kataloogide, muutes alustada nii, et kõik teie ssh klahvid luuakse selle kausta.

```bash
cd ~/.ssh
```

Kui teil on mõne `~/.ssh` directory soovitud `ssh-keygen` käsk luua selle ning vajalikke õigusi.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Käsk ülevaade_

`ssh-keygen`= programm, mida kasutatakse luua võtmed

`-t rsa`= võtme loomiseks, mis on tippige soovitud [RSA vorming](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bittide võti

`-C "myusername@myserver"`= avaliku võtme faili hõlpsaks tuvastamiseks lõppu lisatakse kommentaari.  Tavaliselt kasutatakse kommentaari e-posti, kuid saate kasutada mis tahes sobib kõige paremini teie taristu.

### <a name="using-pem-keys"></a>PEM klahvide abil

Kui kasutate klassikaline juurutamine mudeli (Azure klassikaline portaali või Azure'i teenuse haldus CLI `asm`), peate kasutama PEM vormindatud SSH klahvid juurdepääsu oma Linuxi VMs.  Saate luua olemasoleva SSH avalik võti ja mõne olemasoleva x509 PEM klahvi sert.

Vormindatud on PEM loomiseks olemasoleva SSH avalik võti: võti:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Näide: ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Võtme salvestatud failid:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Selles artiklis paari nimi.  Võti paari nimega **id_rsa** on vaikimisi ja mõned tööriistad võib oodata **id_rsa** privaatse võtme faili nime, millel on üks on hea mõte. Kataloog `~/.ssh/` on SSH võtme paari ja SSH config faili vaikeasukoht.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Loetelu soovitud `~/.ssh` kataloogi. `ssh-keygen`loob soovitud `~/.ssh` kataloog, kui see pole ja ka määrab õige omaniku- ja fail režiimid.

Võtme parooli:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`parooli viitab nimega "parooli".  See on *tungivalt* soovitatav parooli lisamiseks oma võtme paari. Ilma võti paari kaitsmine parooliga igaüks, kellel on privaatse võtme faili saate seda kasutada mis tahes server, mis on vastava avalik võti sisse logida. Parooli pakub rohkem kaitset juhuks, kui keegi on pääsete oma privaatse võtme faili, andnud teile Lisamise aeg muuta võtmed teie autentimiseks kasutada.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Privaatse võtme parooli salvestada ssh-agent abil

Oma privaatse võtme faili parooli koos iga SSH login vältimiseks saate kasutada `ssh-agent` vahemälu privaatse võtme faili parool. Kui kasutate Maci, OSX võtmekimbu talletab turvaliselt privaatse võtme paroolide, kui kutsute `ssh-agent`.

Esmalt veenduge, et `ssh-agent` töötab

```bash
eval "$(ssh-agent -s)"
```

Nüüd lisage privaatne võti `ssh-agent` käsu `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Privaatne võti parool on nüüd salvestatud `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Loomine ja konfigureerimine SSH config failina

On soovitatav heade tavade loomiseks ja konfigureerimiseks on `~/.ssh/config` faili kiirendamiseks logige lisandmoodulid ja optimeerimine oma SSH kliendi käitumist.

Järgmises näites on kujutatud kindlad konfiguratsiooni.

### <a name="create-the-file"></a>Faili loomine

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Faili lisamiseks uue SSH konfiguratsiooni redigeerimiseks tehke järgmist.

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Näide `~/.ssh/config` faili:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

See SSH config annab teile iga server lubamiseks iga on eraldi sihtotstarbeline paari jaotised. Vaikesätete (`Host *`) on hosts, mis ei vasta kindla hosts kõrgemal config faili jaoks.


### <a name="config-file-explained"></a>Otsingukonfiguratsiooni faili ülevaade

`Host`= kutsutud terminalis olevaid hosti nimi.  `ssh fedora22`ütleb `SSH` väärtuste blokeerimise sätete kasutamiseks sildistatud `Host fedora22` Märkus: see võib olla mis tahes silti, mis on teie kasutamise jaoks loogiline ja tähistada tegelik hostname mis tahes serveri.

`Hostname 102.160.203.241`= IP-aadress või DNS-i juurdepääsu serveri nimi.

`User myusername`= remote kasutajakonto server sisselogimisel kasutama.

`PubKeyAuthentication yes`= ütleb SSH, mida soovite kasutada mõnda SSH võti sisse logida.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= SSH privaatvõti ja vastav avalik võti autentimise jaoks kasutada.


## <a name="ssh-into-linux-without-a-password"></a>SSH into Linux paroolita

Nüüd, kui teil on mõni SSH paari ja konfigureeritud SSH config faili, teil on võimalik logige sisse oma Linux VM kiiresti ja turvaliselt. Saate sisse logida serverisse kasutades on SSH võti käsk viipasid saate jaoks parooli, et klahv faili esimest korda.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Käsk ülevaade

Kui `ssh fedora22` käivitatakse SSH esmalt otsib ja laadib kõik sätted soovitud `Host fedora22` plokk ja laadib kõik ülejäänud sätted viimase rühmast `Host *`.

## <a name="next-steps"></a>Järgmised sammud

Järgmisena on Azure Linux VMs abil SSH uus avalik võti loomiseks.  Azure'i VMs, mis on loodud SSH avalik võti nimega login on paremini turvatud kui VMs loodud login vaikemeetod, paroolid.  Azure'i VMs SSH klahvide abil loodud on vaikimisi konfigureeritud paroolidega keelatud, brute sunnitud AIM katsete vältimiseks.

- [Luua turvalist Linux VM on Azure malli abil](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Luua turvalist Linux VM Azure portaalis](virtual-machines-linux-quick-create-portal.md)
- [Luua turvalist Linux VM Azure CLI abil](virtual-machines-linux-quick-create-cli.md)
