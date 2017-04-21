##<a name="tcp-settings-for-azure-vms"></a>Azure'i vms TCP sätted

Avaliku Interneti [NAT] abil suhelda Azure VMs[ nat] (Network Address Translation). NAT seadmeid määrata avaliku IP-aadress ja port on Azure VM, lubades, et VM turvasoklite, side luua muudes seadmetes. Kui paketid peatamiseks läbib selle turvasoklite teatud aja möödumisel NAT seade tapab kaardistamine ja pesa on tasuta kasutada muid VMs.

See on levinud NAT käitumine, mis võivad põhjustada teabevahetuseks TCP vastavalt rakendused, mida oodata turvasoklite, säilitada Lisaks uuesti saata. On kaks jõudeaja ajalõpu sätteid nii, et võiksite seansid *loonud ühenduse* olek.

- **sissetuleva meili** kaudu [Azure'i laadimine koormusetasakaalustusteenuse][azure-lb-timeout]. See ajalõpu 4 minutit vaikesätete ja saab reguleerida kuni 30 minutit.
- **Väljamineva meili** kaudu [SNAT] [ snat] (allikas NAT). See ajalõpu väärtuseks 4 minutit ja ei saa kohandada.

Ühendused on pole kadunud ajalõpp limiidi tagamiseks veenduge rakenduse hoiab seansi elus või saate konfigureerida aluseks operatsioonisüsteemi seda teha. Sätete kasutamiseks erinevad Windows ja Linux süsteemide, nagu allpool näidatud.

[Linuxi][linux], muudate tuum muutujate allpool.
net.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
[Windowsi][windows], muudate registri väärtused allpool.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Ülaltoodud sätted tagada Hoia elus paketi on kaks minutit (120 minutit) jõude aja jooksul saadetud ja seejärel saadetakse iga 30 sekundi järel. Ja kui need paketid 8 ei õnnestu, seanss katkeb.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md