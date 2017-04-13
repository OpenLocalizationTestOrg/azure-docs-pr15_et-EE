<properties
   pageTitle="Registreerida hostinimed dünaamiline DNS-i abil"
   description="Sellelt lehelt leiate üksikasjaliku kuidas luua dünaamiline DNS-i registreerimiseks hostinimed ise oma DNS-serverid."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Dünaamiline DNS-i abil registreerida hostinimed ise oma DNS-i serveris

[Azure'i pakub nimelahendus](virtual-networks-name-resolution-for-vms-and-role-instances.md) virtuaalmasinates (VMs) ja rolli aknad. Juhul, kui teie nimelahendus vajab kui Azure, saate sisestada oma DNS-serverid. See annab teile power oma DNS-i lahendus oma oma vajadustele kohandada. Näiteks peate kohapealse ressursside domeenikontrolleri Active Directory kaudu juurdepääsu.

Kui teie kohandatud DNS-serverid on majutatud as Azure VMs saate edastada hostname päringuid sama vnet Azure hostinimed lahendamiseks. Kui te ei soovi seda kasutada, saate oma VM hostinimed registreerida oma DNS-i serveri dünaamiline DNS-i abil.  Azure'i ei saa (nt mandaat) otse kirjete loomiseks oma DNS-i serverid, et teistsugust korda on sageli vaja. Siin on mõned levinud stsenaariumi alternatiive.

## <a name="windows-clients"></a>Windowsi kliendid

Mitte domeeni-ühendatud Windowsi kliendid üritavad turvamata dünaamiline DNS-i (DDNS) värskendused kui nad ei käivitu või kui IP-aadress muutub. DNS-i nimi on hostname pluss esmane DNS-i järelliite. Azure'i jätab esmane DNS-i järelliite tühjaks, kuid saate seda VM, [UI](https://technet.microsoft.com/library/cc794784.aspx) või [automaatika abil](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix)kaudu.

Domeeni ühendatud Windowsi klientide Registreerige oma IP-aadressid domeenikontrolleri turvaline dünaamilise DNS-i abil. Domeeni-liitumise protsessi määrab esmane DNS-i järelliite klient ja loob ja säilitab usalda seost.

## <a name="linux-clients"></a>Linux klientidele

Linux klientidele üldiselt ei registreerida ise DNS-i serveri käivitamisel, nad endale DHCP server tähendab. Azure DHCP-server pole võimalus või identimisteabe registreerimiseks kirjeid oma DNS-i serveri.  Tööriista nimega *nsupdate*, mis on siduda paketi, saate saata dünaamiline DNS-i värskendused. Kuna on standardiseeritud dünaamiline DNS-protokolli, saate kasutada *nsupdate* isegi siis, kui te ei kasuta siduda DNS server.

Saate kasutada konksud, mis on esitatud DHCP klient, saate luua ja hallata hostname kirje DNS-i serveri. DHCP tsükli, käivitab kliendi skriptid */etc/dhcp/dhclient-exit-hooks.d/*. Seda saab kasutada registreerida, kasutades *nsupdate*IP-aadress. Näiteks:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

Käsk *nsupdate* abil teha turvaline dünaamilise DNS-i värskendused. Näiteks kui kasutate siduda DNS server, era-avalik võti paari on [loodud](http://linux.yyz.us/nsupdate/).  DNS-i server on [konfigureeritud](http://linux.yyz.us/dns/ddns-server.html) avalik võti osa, et seda saab taotluse allkirja kontrollida. *-K* suvandi abil tuleb sisestada võti paari *nsupdate* järjestuses dünaamiline DNS-i värskendamine taotluse allkirjastamist.

Kui kasutate Windows DNS-i server, saate Kerberos-autentimine (pole saadaval versioonis Windows *nsupdate*) *nsupdate* *-g* parameeter. Selleks kasutage *kinit* laadimiseks identimisteabe (nt [keytab fail](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)) kaudu. Seejärel kuvatakse *nsupdate -g* kätte vahemälust identimisteabe.

Vajadusel saate lisada VMs oma DNS-i järelliite otsing. DNS-i järelliite failis */etc/resolv.conf* määratud. Enamik Linux distros automaatselt sisu haldamiseks selle faili, nii et tavaliselt ei saa redigeerida selle. Siiski saate alistada järelliide DHCP klient *asendab* käsu abil. Selleks klõpsake */etc/dhcp/dhclient.conf*lisamiseks tehke järgmist.

        supersede domain-name <required-dns-suffix>;

