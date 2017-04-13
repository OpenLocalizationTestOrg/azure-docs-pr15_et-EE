<properties
    pageTitle="Konfigureerimise DHCPv6 Linux vms | Microsoft Azure'i"
    description="Kuidas seadistada DHCPv6 Linux vms."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6 azure laadimine koormusetasakaalustusteenuse, kahekordne virnas, avaliku ip, kohalikke ipv6, mobile, asjade"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Linux vms DHCPv6 konfigureerimine

Azure'i turuplatsil Linux virtuaalse masina pilte ei ole konfigureeritud vaikimisi DHCPv6. IPv6 toetamiseks peavad DHCPv6 konfigureerida Linux OS jaotuse, mida kasutate sees. Erinevate Linuxi on konfigureerimise DHCPv6, kuna neid kasutada erinevaid pakette erineval viisil.

>[AZURE.NOTE] Azure'i turuplatsi tehtud SUSE Linux ja CoreOS pildid on eelkonfigureeritud DHCPv6 abil. Täiendavaid muudatusi on vaja teenuse neid pilte.

Selles dokumendis kirjeldatakse, kuidas lubada DHCPv6 nii, et arvuti Linux virtual hangib IPv6-aadress.

>[AZURE.WARNING] Võrgu konfigureerimine failide redigeerimine valesti, võivad põhjustada te kaotate võrgule juurdepääsu oma VM. Soovitame mitte tekitamiseks operatsioonisüsteemides oma konfiguratsioonimuudatuste testida. Selles artiklis antud juhiseid on testitud Linux pilte Azure'i turuplatsi uusimad versioonid. Tutvuge oma kindla versiooni Linux üksikasjalikke juhiseid.

## <a name="ubuntu"></a>Ubuntu

1. Faili redigeerimiseks `/etc/dhcp/dhclient6.conf` ja lisada järgmine rida:

        timeout 10;

2. Pärast konfiguratsioon eth0 kasutajaliidese võrgukonfiguratsioon redigeerimiseks tehke järgmist.

    * **Ubuntu 12.04 ja 14.04**, klõpsake faili redigeerimine`/etc/network/interfaces.d/eth0.cfg`
    * Faili redigeerimiseks **Ubuntu 16.04**,`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. IPv6-aadress pikendamiseks tehke järgmist.

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Faili redigeerimiseks `/etc/dhcp/dhclient6.conf` ja lisada järgmine rida:

        timeout 10;

2. Faili redigeerimiseks `/etc/network/interfaces` ja lisage järgmine konfiguratsioon:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6-aadress pikendamiseks tehke järgmist.

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle'i Linux

1. Faili redigeerimiseks `/etc/sysconfig/network` ja lisage järgmine parameeter:

        NETWORKING_IPV6=yes

2. Faili redigeerimiseks `/etc/sysconfig/network-scripts/ifcfg-eth0` ja lisage järgmised kaks parameetrid:

        IPV6INIT=yes
        DHCPV6C=yes

3. IPv6-aadress pikendamiseks tehke järgmist.

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Azure'i tehtud SLES ja openSUSE pildid on eelkonfigureeritud DHCPv6 abil. Täiendavaid muudatusi on vaja teenuse neid pilte. Kui teil on vanema või kohandatud SUSE pildi põhjal VM, siis tehke järgmist:

1. Installimine on `dhcp-client` paketti vajaduse korral:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Faili redigeerimiseks `/etc/sysconfig/network/ifcfg-eth0` ja lisage järgmine parameeter:

        DHCLIENT6_MODE='managed'

3. IPv6-aadress pikendamiseks tehke järgmist.

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 ja openSUSE hüpe

Azure'i tehtud SLES ja openSUSE pildid on eelkonfigureeritud DHCPv6 abil. Täiendavaid muudatusi on vaja teenuse neid pilte. Kui teil on vanema või kohandatud SUSE pildi põhjal VM, siis tehke järgmist:

1. Faili redigeerimiseks `/etc/sysconfig/network/ifcfg-eth0` ja asendada see parameeter

        #BOOTPROTO='dhcp4'

    järgmine väärtus:

        BOOTPROTO='dhcp'

2. Lisage järgmised parameeter `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. IPv6-aadress pikendamiseks tehke järgmist.

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Azure'i tehtud CoreOS pildid on eelkonfigureeritud koos DHCPv6. Täiendavaid muudatusi on vaja teenuse neid pilte. Kui teil on vanema või kohandatud CoreOS pildi põhjal VM, siis tehke järgmist:

1. Faili redigeerimine`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. IPv6-aadress pikendamiseks tehke järgmist.

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
