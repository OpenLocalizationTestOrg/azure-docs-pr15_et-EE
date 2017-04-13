<properties
   pageTitle="Ühenduse loomine mõne Azure'i teenus kobar | Microsoft Azure'i"
   description="Ühenduse loomine mõne Azure'i teenus kobar on SSH tunneliga abil."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Keskmise suurusega, ümbriste, mikro-teenuseid, näiteks Põhiliselt/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Ühenduse loomine mõne Azure'i teenus kobar

Näiteks Põhiliselt/OS- ja keskmise suurusega sülem kogumite Azure'i teenus juurutatud jätke ülejäänud lõpp-punktid. Nende lõpp-punktid pole avatud muu maailma. Nende lõpp-punktid haldamiseks peate looma Secure Shell (SSH) tunneliga. Pärast soovitud SSH tunneliga on loodud, saate käivitada käsud vastu kobar lõpp-punktid ja vaadata kobar Kasutajaliidese kaudu kui ka oma arvutisse. Selle dokumendi juhatab teid läbi mõne SSH tunneliga loomise Linux, OS X ja Windows.

>[AZURE.NOTE] Saate luua ka SSH seansi kobar süsteem. Aga me ei soovita seda. Töö otse süsteem seab kliendid: tahtmatu konfiguratsioonimuudatuste riski.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Luua mõne SSH tunneliga Linux või OS X

Kõigepealt, mida teha, kui loote mõnda SSH tunneliga Linux või OS X on leidmiseks koormusetasakaalustusega juhtslaidi avaliku DNS-i nimi. Selleks laiendage ressursirühma nii, et iga ressursi kuvatakse. Otsige ja valige soovitud juhtslaidi avaliku IP-aadress. See avab tera, mis sisaldab teavet avaliku IP-aadress, mis sisaldab DNS-i nimi. Salvestage see hilisemaks kasutamiseks nimi. <br />


![Avaliku DNS-i nimi](media/pubdns.png)

Nüüd avage shell ja käivitage järgmine käsk kui:

**PORT** on port lõpp-punkti, mille soovite seada. Sülem, on see 2375. AV/OS, kasutage pordi 80.  
**Kasutajanimi** on kasutajanimi, mis oli lisatud klaster juurutamisel.  
**DNSPREFIX** on esitatud klaster juurutamisel DNS-i eesliide.  
**Piirkond** on piirkond, kus asub teie ressursirühma.  
**PATH_TO_PRIVATE_KEY** [Valikuline] on privaatvõti, mis vastab avalik võti registreerimisel loodud teenus kobar tee. Selle suvandi abil -i lipuga.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> SSH ühenduse port on 2200--ei standard pordi 22.

## <a name="dcos-tunnel"></a>Näiteks Põhiliselt/OS tunneliga

Avage soovitud tunneliga, et AV/OS seotud lõpp-punktid, käivitada käsk, mis sarnaneb järgmisega:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nüüd pääsete juurde näiteks Põhiliselt/OS seotud lõpp-punktid juures.

- NÄITEKS PÕHILISELT/OS:`http://localhost/`
- Maraton:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Samuti jõuate rest API-de iga rakenduse kaudu selle tunneliga.

## <a name="swarm-tunnel"></a>Sülem tunneliga

Avage soovitud tunneliga sülem lõpp-punkti, käivitada käsk, mis näeb välja umbes järgmine:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nüüd saate oma DOCKER_HOST muutuja järgmiselt. Saate jätkata tavapäraselt kasutada keskmise suurusega käsurea liides (CLI).

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Opsüsteemis Windows on SSH tunneliga loomine

On mitu võimalust loomise SSH tunnelid Windows. Selle dokumendi kirjeldab, kuidas PuTTY abil tehke järgmist.

Teie Windowsi süsteem PuTTY alla laadida ja käivitage rakendus.

Sisestage hosti nimi, mis koosneb kobar administraatori kasutajanime ja esimese juhtslaidi klaster avaliku DNS-i nimi. **Host Name** näeb välja umbes järgmine: `adminuser@PublicDNS`. Sisestage **pordi**2200.

![Kitt konfiguratsiooni 1](media/putty1.png)

Valige **SSH** ja **autentimist**. Lisada oma privaatse võtme faili autentimiseks.

![Kitt konfiguratsiooni 2](media/putty2.png)

Valige **tunnelid** ja konfigureerida edasisaatmist pordid järgmist:
- **Lähteport:** Oma eelistust--kasutamine 80 AV/OS või 2375 sülem jaoks.
- **Sihtkoht:** Kasutada localhost:80 AV/OS või localhost:2375 sülem.

Järgmises näites on näiteks Põhiliselt/OS konfigureeritud, kuid sarnanevad keskmise suurusega sülem jaoks.

>[AZURE.NOTE] Pordi 80 peab olema kasutusel selle tunneliga loomisel.

![Kitt konfiguratsiooni 3](media/putty3.png)

Kui olete lõpetanud, salvestage ühenduse konfiguratsiooni ja ühenduse kitt seanss. Kui loote ühenduse, saate vaadata pordi konfigureerimise kitt sündmuste logi.

![Kitt sündmuste logi](media/putty4.png)

Kui olete konfigureerinud selle tunneliga AV/OS, pääsete juurde seotud lõpp-punkti juures.

- NÄITEKS PÕHILISELT/OS:`http://localhost/`
- Maraton:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Kui olete konfigureerinud selle tunneliga jaoks keskmise suurusega sülem, pääsete sülem kobar keskmise suurusega CLI kaudu. Peate esmalt konfigureerimine Windowsi keskkonnas muutuja nimega `DOCKER_HOST` väärtusega ` :2375`.

## <a name="next-steps"></a>Järgmised sammud

Juurutada ja hallata ümbriste AV/OS või sülem:

- [Azure'i teenus ja AV/OS töötamine](container-service-mesos-marathon-rest.md)
- [Azure'i teenus ja keskmise suurusega sülem töötamine](container-service-docker-swarm.md)
