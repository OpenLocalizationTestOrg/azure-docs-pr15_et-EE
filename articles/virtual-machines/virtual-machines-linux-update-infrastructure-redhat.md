<properties
   pageTitle="Punane rolli Update infrastruktuur (RHUI) | Microsoft Azure'i"
   description="Siin on nõudmisel punane rolli Enterprise Linux eksemplarid Microsoft Azure kohta punane rolli Update infrastruktuur (RHUI)"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Punane rolli Update taristu nõudmisel punane rolli Enterprise Linux vms Azure (RHUI)

Nõudmisel Red rolli Enterprise Linux (RHEL) pilte saadaval Azure'i turuplatsi loodud virtuaalmasinates registreeritakse juurdepääsuks on punane rolli Update infrastruktuur (RHUI) juurutatud Azure.  Nõudmisel RHEL eksemplarid on juurdepääs piirkondliku yum hoidla ja võimalus suureneva värskendusi.

Yum hoidla loend, mida haldab RHUI ajal ettevalmistamise oma RHEL eksemplari konfigureeritud. Te ei pea tegema mis tahes täiendavaid konfiguratsiooni - käivitada `yum update` pärast oma RHEL eksemplari on valmis, et saada uusimad värskendused.

> [AZURE.NOTE] Azure'i RHUI taristu on viimati värskendatud (mai 2016) ja nõuab muudatusi teie olemasoleva RHEL eksemplaride konfiguratsiooni Azure'i RHUI pideva juurdepääsu. Vaadake jaotist RHUI Azure infrastruktuuri värskendus üksikasju.


## <a name="rhui-azure-infrastructure-update"></a>RHUI Azure'i infrastruktuuri värskendus
Mai 2016 seisuga Azure on uus komplekt punane rolli infrastruktuur (RHUI) serverites. Need serverid on juurutatud [Azure'i liikluse]( https://azure.microsoft.com/services/traffic-manager/) Manager nii, et ühe lõpp-punkti (rhui 1.micrsoft.com) saab kasutada mis tahes VM sõltumata piirkond. Nad kasutavad ka SSL cert, mis on ühendatud, et tuntud sertimiskeskus (Baltimore root). Selle värskenduse automaatse tegemist oleks ohtlik mõned klientidele, kes on ACL-ID või kohandatud marsruutimise tabelite RHUI update servereid, nii, et see värskendus on "osalemine." Käsitsi juhised aitavad need uue serverid on saadaval selle lehe ja täielik skript kasutuselevõtt automatiseeritud viisil (kontrollimisel üksikute juhiseid). Uue RHEL PAYG pilte Azure'i turuplatsi (mai 2016 kupongil või uuemad versioonid) automaatselt uue Azure'i RHUI serverid punktist ja nõuavad mis tahes lisatoiminguid.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Azure'i RHUI taristu kasutuselevõtt ajaskaala

| Kuupäev | Märkus |
| --- | --- |
|22 mai 2016 | RHUI serverid ja installimise juhised kasutamiseks saadaval. VMs juurutatud uue (mai 2016 kuupäevaga) kasutab RHEL PAYG turuplatsi piltide automaatselt uue RHUI serverid, kuid olemasoleva VMs on "osalemine"
|1 november 2016 | Pärand RHEL PAYG VM pildid (mis kasutada vana Azure'i RHUI serverid) eemaldatakse Azure'i turuplatsi Galerii
|16 Jaanuar 2017 | Kasutuselt kõrvaldatakse vana Azure'i RHUI serverid. Värskendada kõik teie probleemse PAYG RHEL VMs seekord Azure'i RHUI juurdepääsu haldamine

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>IP-d uue RHUI sisu kohaletoimetamise serverid on

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Käsitsi värskenduse toimingu kasutama uut Azure'i RHUI serverid

Avalik võti allkirja (kaudu curl) allalaadimine

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Veenduge, et allalaaditud võti

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Kontrollige väljund, kontrollige `keyid` ja `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Avalik võti installimine

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Laadige alla, kinnitamine ja installida kliendi pööret MINUTIS

Allalaadimine: Jaoks RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

RHEL 7 jaoks

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Kontrollida.

```
rpm -Kv azureclient.rpm
```

Märkige ruut väljund paketi allkiri on OK

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Installige soovitud pööret MINUTIS

```
sudo rpm -U azureclient.rpm
```

Lõpetamisel, veenduge, et pääsete Azure'i RHUI vormi VM

### <a name="all-in-one-script-for-automating-the-above-task"></a>Kõik ühes skripti automatiseerimiseks ülaltoodud ülesanne
Kasutage järgmist skripti vastavalt vajadusele või värskendamise probleemse VMs uue Azure'i RHUI servereid ülesannete automatiseerimiseks.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>RHUI ülevaade
[Punane rolli Update taristu](https://access.redhat.com/products/red-hat-update-infrastructure) pakub väga paindlik lahendus yum hoidla jaoks punane rolli Enterprise Linux cloud eksemplarid, mis on majutatud punane rolli sertifitseeritud pilveteenuse pakkujate sisu haldamiseks. Põhjal varustava paberimassi projekt, RHUI võimaldab pilveteenuse pakkujate kohalikult punane rolli majutatud hoidla sisu kajastavad, luua kohandatud hoidlate nende endi sisuga ja nende hoidlate suurele rühmale lõppkasutajate koormusetasakaalustusega sisu kohaletoimetamise süsteemi kaudu kättesaadavaks teha.

## <a name="regions-where-rhui-is-available"></a>Regioonid, kus RHUI on saadaval
RHUI on saadaval kõikides piirkondades, kus RHEL nõudmisel pildid on saadaval. See sisaldab praegu Azure'i USA valitsuse piirkondade ja [Azure olek](https://azure.microsoft.com/status/) armatuurlaualehe loetletud kõik avaliku piirkondades. RHUI juurdepääsu vms ette valmistatud piltidest RHEL nõudmisel on nende hinna sisse. Täiendavad riigi/piirkonna cloud-saadavus värskendatakse, kui laiendame RHEL nõudmisel kättesaadavus tulevikus.

> [AZURE.NOTE] Azure'i majutatud RHUI juurdepääs on piiratud VMs sees [Microsoft Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Mõne muu värskenduse hoidla Värskenduste toomine

Kui teil on vaja Värskenduste toomine erinevad värskenduse hoidla (Azure'i majutatud RHUI) asemel pead eksemplaride kaudu RHUI tühistada ja registreerige need uuesti soovitud värskenduse infrastruktuuri (nt punane rolli kaabel või punane rolli kliendi portaali CDN-ID). [Punane rolli pilvepõhise juurdepääsu Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure)te peate nende teenuste ja registreerimise vastav punane rolli tellimused.

Unregister RHUI ja uuesti registreerida update taristu jätkata selle all juhiseid.

1.  /Etc/yum.repos.d/rh-cloud.repo redigeerida ja muuta kõik `enabled=1` et `enabled=0`. Näiteks:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  /Etc/yum/pluginconf.d/rhnplugin.conf redigeerida ja muuta `enabled=0` et `enabled=1`.
3.  Klõpsake soovitud infrastruktuuri, näiteks punane rolli kliendi portaalis registreerida. Järgige punane rolli lahendus juhend kohta, [Kuidas registreerida ja tellimine punane rolli kliendi portaali süsteem](https://access.redhat.com/solutions/253273).

> [AZURE.NOTE] Azure'i majutatud RHUI juurdepääsu sisaldub RHEL grupikindlustusleping (PAYG) pilt hind. Registreerimise tühistamine PAYG RHEL VM: Azure'i majutatud RHUI teisendada virtuaalse masina Too-oma-enda-litsents (BYOL) liiki VM ja seega saate võib-olla tekiks kahekordsete kulude kui sama VM registreerida värskenduste muust allikast. 
> 
> Kui teil on vaja süsteemne kasutamine update infrastruktuur kui Azure majutatud RHUI kaaluge loomine ja juurutamine (BYOL tüüp) oma piltide [loomine ja üles laadida punane rolli-põhise arvutiga Azure](virtual-machines-linux-redhat-create-upload-vhd.md) artiklis kirjeldatud.

## <a name="next-steps"></a>Järgmised sammud
Azure'i turuplatsi grupikindlustusleping pilt ja võimendada punane rolli Enterprise Linux VM loomiseks avage Azure'i majutatud RHUI [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/partners/redhat/). Saab kasutada `yum update` oma RHEL eksemplari ilma mis tahes täiendavaid häälestus.