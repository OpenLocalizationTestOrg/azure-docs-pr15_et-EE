
<properties
   pageTitle="Vastastikuste laadi koormusetasakaalustusteenuse ülevaade Internet | Microsoft Azure'i "
   description="Internet vastastikuste koormusetasakaalustusteenuse laadimine ja selle funktsioonid ülevaates. Kuidas töötab laadi koormusetasakaalustusteenuse Azure'i virtuaalmasinates ja pilveteenustega abil."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internet-facing-load-balancer-overview"></a>Internet suunatud laadi koormusetasakaalustusteenuse ülevaade

Azure'i laadi koormusetasakaalustusteenuse kaarte avaliku IP-aadress ja pordi arvu liiklust IP-aadress ja pordi eranumbrile virtuaalse masina ja vastupidi vastuse liikluse virtuaalse masina. Laadi tasakaalustavad reeglite abil saate levitada kindlat tüüpi mitme virtuaalmasinates või teenuste vahel. Näiteks saate levitada web taotlus liikluse laadi mitmest web servers või web rollid.

Mõnda pilveteenusesse, mis sisaldab web rollid või töötaja rolli jaoks saate määratleda avaliku lõpp-punkti teenuse definitsioonifail (.csdef).

_Servicedefinition.csdef_ fail sisaldab lõpp-punkti konfiguratsiooni ja kui teil on mitu rolli eksemplari veebis või töötaja rolli juurutamiseks, laadi koormusetasakaalustusteenuse saab seda häälestus. Võimalus lisada eksemplarid pilve juurutamise muutub eksemplari arv teenuse konfigureerimine faili (.csfg).

Järgneval joonisel on kujutatud koormusetasakaalustusega endpoint krüptitud web liikluse, mis on jagatud kolme virtuaalmasinates avalike ja privaatvõ TCP pordi 443 vahel. Need kolm virtuaalmasinates on koormusetasakaalustusega kogum.

![avaliku laadi koormusetasakaalustusteenuse näide](./media/load-balancer-internet-overview/IC727496.png))

Joonis 1 - koormusetasakaalustusega lõpp-punkti krüptitud web liiklus

Kui Interneti-kliendid veebilehe päringuid saata TCP-pordi 443 pilveteenusesse avaliku IP-aadress, Azure'i laadimine koormusetasakaalustusteenuse jaotab taotlusi koormusetasakaalustusega määramine kolme virtuaalmasinates vahel. Laadi koormusetasakaalustusteenuse algoritmide kohta leiate lisateavet teemast [laadimine lehe koormusetasakaalustusteenuse ülevaade](load-balancer-overview.md#load-balancer-features).

Vaikimisi jaotab Azure'i laadi koormusetasakaalustusteenuse võrguliiklust võrdselt vahel virtuaalse masina mitmes eksemplaris. Samuti saate konfigureerida seansi kuuluvuse, lisateabe saamiseks vaadake teemat [Laadi koormusetasakaalustusteenuse jaotuse režiim](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Järgmised sammud

Lisateavet [Internal laadimine koormusetasakaalustusteenuse](load-balancer-internal-overview.md) paremini mõista, millist laadi koormusetasakaalustusteenuse on paremini sobib teie pilvepõhise juurutamiseks.

Võite ka [vastastikuste laadi koormusetasakaalustusteenuse Interneti loomise alustamiseks](load-balancer-get-started-internet-arm-ps.md) ja mis tüüpi teatud laadi koormusetasakaalustusteenuse võrgu liikluse käitumine [jaotuse režiimi](load-balancer-distribution-mode.md) konfigureerimine.

Kui teie rakendus peab ühendused toitsid serverite laadi koormusetasakaalustusteenuse taga, saavad aru veel [jõudeolekus TCP ajalõpp sätete laadi koormusetasakaalustusteenuse](load-balancer-tcp-idle-timeout.md)kohta. Lisateavet jõude ühenduse käitumise kasutamisel Azure'i laadi koormusetasakaalustusteenuse aitab.
