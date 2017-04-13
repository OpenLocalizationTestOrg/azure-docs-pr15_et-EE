<properties
    pageTitle="IPv6 jaoks Azure laadi koormusetasakaalustusteenuse ülevaade | Microsoft Azure'i"
    description="IPv6 tugi Azure koormusetasakaalustusteenuse laadimine ja koormusetasakaalustusega VMs mõistmine."
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

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>IPv6 jaoks Azure laadi koormusetasakaalustusteenuse ülevaade

Interneti-ühendusega koormus soolise saab kasutada koos IPv6-aadress. Lisaks IPv4 connectivity, võimaldab see järgmisi võimalusi.

* Kohalikule-lõpuni IPv6 ühenduvuse avaliku Interneti klientide ja Azure'i Virtuaalmasinates (VM) vahel koormusetasakaalustusteenuse laadimise kaudu.
* Keele-lõpuni IPv6 Väljamineva meili Ühenduvus VMs ja avaliku IPv6 Interneti-toega kliendid.

Järgmisel pildil on näidatud IPv6 funktsioonid Azure'i laadi koormusetasakaalustusteenuse jaoks.

![Azure'i laadi koormusetasakaalustusteenuse IPv6-ga](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Kui juurutada, kui IPv4 või IPv6 lubatud Interneti klient saaksid suhelda avaliku IPv4 või IPv6 aadressid (või hostinimed), Azure'i Interneti-ühendusega laadi koormusetasakaalustusteenuse. Laadi koormusetasakaalustusteenuse marsruudib IPv6 pakette privaatne IPv6-aadresside vms, kasutades võrgu aadress tõlget (NAT). IPv6 Interneti kliendi ei saa suhelda otse VMs IPv6-aadress.

## <a name="features"></a>Funktsioonid

Kohalikke IPv6 tugi VMs juurutatud kaudu Azure'i ressursihaldur pakub.

1. Koormusetasakaalustusega IPv6 teenuste IPv6 kliendid Internetis
2. Kohalikke IPv6- ja IPv4 lõpp-punktid, klõpsake VMs ("kahe virn")
3. Sissetulevate ja väljaminevate algatatud kohalikke IPv6 ühendused
4. Toetatud protokollid, nt TCP, UDP ja HTTP (S) lubada kõiki äriprotsessid

## <a name="benefits"></a>Eelised

See funktsioon võimaldab võtme järgmised eelised:

* Vastavad valitsuse määrus nõudva uute rakenduste pääsema ainult IPv6 klientidele
* Luba mobile ja Internet asjade arendajad aadress kasvab mobile ja asjade turgude kahe virn (IPv4 + IPv6) Azure'i Virtuaalmasinates abil

## <a name="details-and-limitations"></a>Andmed ja piirangud

Üksikasjad

* Azure'i DNS-i teenus sisaldab nii IPv4 A ja IPv6 AAAA nimi kirjeid ja vastab laadi koormusetasakaalustusteenuse mõlemad kirjed. Klient valib, milline aadress (IPv4 või IPv6) suhelda.
* Kui VM käivitab avaliku IPv6 Interneti-ühendusega seadme ühenduse, on VM allika IPv6-aadress on võrgu aadress tõlgitud (NAT) laadi koormusetasakaalustusteenuse avaliku IPv6-aadress.
* VMs töötab, Linux operatsioonisüsteemi tuleb konfigureerida IPv6 IP-aadressi DHCP vastu võtta. Paljude Linuxi pildid Azure'i galeriis on juba konfigureeritud toetama IPv6 muutmata kujul. Lisateabe saamiseks lugege teemat [Linux vms DHCPv6 konfigureerimine](load-balancer-ipv6-for-linux.md)
* Kui otsustate kasutada koos oma laadi koormusetasakaalustusteenuse seisundi juures, on IPv4 juures luua ja kasutada seda nii IPv4 ja IPv6 lõpp-punktid. Kui teie VM teenuse läheb alla, on välja pööre võtta nii IPv4 ja IPv6 lõpp-punktid.

Piirangud

* Ei saa lisada IPv6 koormust tasakaalustavad reeglid Azure'i portaalis. Reegleid saab luua ainult Mall, CLI, PowerShelli kaudu.
* Täiendate võib kasutada IPv6-aadresside olemasoleva VMs. Uue VMs peab juurutamist.
* Ühe võrgu kasutajaliideses iga VM saab määrata ühe IPv6-aadress.
* Avaliku IPv6-aadresside ei saa määrata VM. Ta saab määrata ainult laadimise koormusetasakaalustusteenuse.
* Ei saa konfigureerida vastupidise DNS-i otsingu jaoks avaliku IPv6-aadresside.
* VMs IPv6 aadressid, ei saa teenuse Azure pilveteenuse liikmetele. Ta saab ühendada Azure virtuaalse võrgu (VNet) ja omavahel suhelda üle nende IPv4 aadressid.
* Privaatne IPv6-aadresside saab juurutada üksikute VMs ressursirühma, kuid ei saa juurutatud rühma ressursside kaudu skaala komplektid.
* Azure'i VMs ei saa ühendust üle IPv6 teiste VMs muudele Azure'i teenuste või kohapealse seadmed. Ta saab ainult suhelda Azure laadi koormusetasakaalustusteenuse üle IPv6. Siiski nad suhelda abil IPv4 ressursid.
* Võrgu turberühma (NSG) kaitseks IPv4 on toetatud juurutuste kahe-virnlintdiagrammil (IPv4 + IPv6). NSGs ei rakendata IPv6 lõpp-punktid.
* IPv6 lõpp-punkti VM on avatud otse Interneti-ühendus. Laadi koormusetasakaalustusteenuse taga on. Ainult määratud laadi koormusetasakaalustusteenuse reegleid pordid puuetega inimestele juurdepääsetavate üle IPv6.
* IPv6 IdleTimeout parameetri muutmine on **praegu ei toetata**. Vaikimisi on neli minutit.

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas juurutada laadi koormusetasakaalustusteenuse IPv6-ga.

* [IPv6 regiooniti kättesaadavus](https://go.microsoft.com/fwlink/?linkid=828357)
* [Laadi koormusetasakaalustusteenuse IPv6 malli abil juurutamine](load-balancer-ipv6-internet-template.md)
* [Laadi koormusetasakaalustusteenuse IPv6 Azure PowerShelli kaudu juurutamine](load-balancer-ipv6-internet-ps.md)
* [Laadi koormusetasakaalustusteenuse IPv6 abil Azure'i CLI juurutamine](load-balancer-ipv6-internet-cli.md)
