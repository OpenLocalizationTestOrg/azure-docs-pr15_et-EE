<properties 
   pageTitle="Azure'i klassikaline portaalis VPN-lüüsi konfigureerimine | Microsoft Azure'i"
   description="See artikkel suunab teid läbi virtuaalse võrgu VPN-lüüsi konfigureerimise ja muutmise lüüsi VPN marsruutimise tüüp."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>Klassikaline juurutamise mudeli VPN-lüüsi konfigureerimine


Kui soovite luua turvalist asutusesiseses seos Azure ja teie asutusesisese asukohta, peate virtuaalse privaatvõrgu lüüsi konfigureerimine. Klassikaline juurutamise mudeli, lüüsi võib olla VPN marsruutimise kahes: staatiline või dünaamiline. Valige tüüp sõltub teie võrgu kujundus leping ja kohapealse VPN seade, mida soovite kasutada. 

Näiteks nõuda ka ühenduvuse võimalusi, ühenduse punkti saidi, nt dünaamiline marsruutimine lüüsi. Kui soovite konfigureerida oma nii SharePointi saidil (P2S) ühendused ja saidi saidi (S2S) ühenduse lüüs, tuleb konfigureerida dünaamilise marsruutimise lüüsi isegi juhul, kui-saidilt saab konfigureerida kas lüüsi VPN marsruutimise tüüp. 

Lisaks peate veenduge, et seade, mida soovite kasutada oma ühenduse toetab seda VPN marsruutimise tüüpi, mida soovite luua. Lugege [VPN seadmed](vpn-gateway-about-vpn-devices.md).


**Selle artikli kohta** 

See artikkel on kirjutatud klassikaline juurutamise mudeli [klassikaline portaali](https://manage.windowsazure.com) (mitte Azure'i portaal). 

**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Konfiguratsiooni ülevaade

Järgmised toimingud sõelub VPN-lüüsi konfigureerimise Azure klassikaline portaalis. Need juhised kehtivad lüüside virtuaalse võrkude klassikaline juurutamise mudeli abil loodud. Praegu pole kõik lüüside sätete konfigureerimine on Azure portaalis. Kui nad on, loome uus komplekt juhiseid, mis rakendatakse Azure portaali.


1. [Teie VNet VPN-lüüsi loomine](#create-a-vpn-gateway)

1. [Koguge teavet oma VPN seadme konfigureerimine](#gather-information-for-your-vpn-device-configuration)

1. [Seadme VPN-i konfigureerimine](#configure-your-vpn-device)

1. [Veenduge, et teie kohalikus võrgus vahemiku ja VPN gateway IP-aadress](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Enne alustamist

Enne kui saate konfigureerida lüüsi, peate esmalt virtuaalse võrgu loomine. Juhised virtuaalse võrgu asutusesiseses Ühenduvus loomiseks leiate teemast [konfigureerimine virtuaalse võrgu - saidilt VPN-ühendus](vpn-gateway-site-to-site-create.md)või [konfigureerimine virtuaalse võrgu punkti – saidile VPN-ühendus](vpn-gateway-point-to-site-create.md). Seejärel kasutage järgmist VPN-lüüsi konfigureerimine ja seadme VPN konfigureerimiseks vajaliku teabe kogumine. 

Kui teil on juba VPN-lüüsi ja soovite muuta VPN marsruutimise tüüp, vaadake, [Kuidas muuta oma lüüsi VPN marsruutimise tüüpi](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>VPN-lüüsi loomine

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com)lehel **võrkude** veenduge, et veerus Olek virtuaalse võrgu jaoks on **loodud**.

1. Klõpsake veerus **Name** virtuaalse võrgu nimi.

1. **Armatuurlaualehe,** Pange tähele, et see VNet pole veel konfigureeritud lüüsi. See olek kuvatakse nagu läbite lüüsi konfigureerimine.

![Lüüsi ei looda](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Seejärel klõpsake lehe allosas **Lüüsi loomine**. Saate valida, kas *Staatiline marsruutimine* või *Dünaamiline marsruutimist*. VPN marsruutimise tüüp valite sõltub tegurist. Näiteks mis VPN-seade toetab ja kas teil on vaja punkti saidi ühenduste toetamiseks. Märkige ruut [Kohta VPN seadmete virtuaalse võrguühendus](vpn-gateway-about-vpn-devices.md) kinnitamiseks VPN marsruutimise tüüp, mida vajate. Kui lüüsi on loodud, ei saa muuta lüüsi VPN marsruutimise tüüpi ilma kustutamine ja uuesti luua lüüsi vahel. Kui süsteemi palub teil kinnitada lüüsi, mis on loodud, klõpsake nuppu **Jah**.

![Lüüsi VPN marsruutimise tüüp](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Lüüsi loomisel teade lüüsi pilti lehel muutub kollane ja öeldakse, et *Lüüsi loomine*. Võib kuluda kuni 45 minutit lüüsi loomiseks. Oodake, kuni lüüsi on lõpule jõudnud, enne kui saate liikuda edasi ja muude sätete konfigureerimine.

![Lüüsi loomine](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Lüüsi muutumisel *ühenduse loomine*, mida saate VPN-seadme jaoks on vaja teabe kogumine.

![Ühenduse lüüs](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Koguge teavet oma VPN seadme konfigureerimine

Pärast lüüsi loomist, Koguge teavet oma VPN seadme konfigureerimine. See teave on asub **armatuurlaua** lehe virtuaalse võrgu jaoks:

1. **Lüüsi IP-aadress-** **Armatuurlaualehe** leiate IP-aadress. Te ei saa seda kuni näha, kui teie lüüsi on lõpule jõudnud, loomine.

1. **Ühiskasutuses võti-** Klõpsake nuppu **Halda klahv** ekraani allosas. Klõpsake oma lõikelauale kopeerimiseks ja kleepimiseks ja salvestada võti võti kõrval olevat ikooni. – See nupp toimib ainult siis, kui seal on ühe S2S VPN tunneliga. Kui teil on mitu S2S VPN tunnelite, kasutage *Saada virtuaalse võrgu jagatud Lüüsivõtit* API või PowerShelli cmdlet-käsk.

![Klahv haldamine](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Seadme VPN-i konfigureerimine

Pärast eelmisi juhiseid, teie või teie võrguadministraator on vaja konfigureerima VPN ühenduse loomiseks. Vt [Kohta VPN seadmete virtuaalse võrguühendus](vpn-gateway-about-vpn-devices.md) VPN seadmete kohta lisateavet.

Pärast VPN-seade on konfigureeritud, saate vaadata oma värskendatud ühenduseteavet armatuurlaualehe oma VNet jaoks.

Saate kasutada ühte järgmistest käskudest ühenduse testimiseks.

|                      | Cisco ASA             | Cisco ISR/ASR         | Kadakas SSG/ISG | Kadakas SRX/J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Peamine režiimis SAs kontrollimine**  | Kuva krüpto isakmp sa | Kuva krüpto isakmp sa | Too ike küpsis  | Kuva turvalisus ike turbe-seos   |
| **Kontrollige lü režiimi SAs** | Kuva krüpto ipsec sa  | Kuva krüpto ipsec sa  | saada sa          | Turvalisus ipsec-seose kuvamine |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Veenduge, et teie kohalikus võrgus vahemiku ja VPN gateway IP-aadress

### <a name="verify-your-vpn-gateway-ip-address"></a>Veenduge, et teie VPN gateway IP-aadress

Jaoks õigesti ühenduse lüüs, VPN seadme IP-aadress peab olema õigesti konfigureeritud asutusesiseses konfiguratsioonis määratud kohalikus võrgus. Tavaliselt on see konfigureeritud-saidilt konfigureerimise käigus. Kuid kui olete varem kasutanud selle kohalikus võrgus mõne muu seadme või IP-aadress on muutunud selle kohalikus võrgus, redigeerida sätete määramiseks õige Gateway IP-aadress.

1. Lüüsi IP-aadressi kinnitamiseks **võrkude** portaali vasakpoolsel paanil nuppu ja seejärel valige **Kohaliku võrgu** lehe ülaosas. Kuvatakse iga kohalikus võrgus, mis on loodud VPN-lüüsi aadress. IP-aadressi muutmiseks valige soovitud VNet ja klõpsake nuppu **Redigeeri** lehe allosas.

1. Klõpsake lehel **teie kohalikus võrgus üksikasjad** redigeerimine IP-aadress ja seejärel lehe allservas noolenuppu järgmine.

1. Klõpsake lehe **Määrake aadress ruumi** parempoolses allnurgas sätete salvestamiseks klõpsake märke.

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Veenduge, et kohaliku võrguga aadresside vahemikud

Õige liikluse vool läbi lüüsi oma kohapealse asukohta, peate veenduge, et iga IP-aadresside vahemiku on määratud. Kõigil vahemikel peab olema lisatud teie Azure **Kohaliku võrgu** konfiguratsiooni. Asukoha kohapealse võrgu konfiguratsioonist, see võib olla veidi suur tööülesande. Lüüsi VPN virtuaalse võrgu kaudu saadetakse liiklust, mis on seotud IP-aadress, mis sisaldub loetletud vahemikud. Saate loendis ei pea olema privaatne vahemikud, ehkki te soovite kontrollida, kas teie asutusesisese konfiguratsiooni saate vastu võtta sissetulev liiklus vahemikud.

Lisada või vahemike redigeerimise kohtvõrgu jaoks, siis tehke järgmist.

1. Kohtvõrgu IP-aadresside vahemikud redigeerimiseks klõpsake vasakul paanil portaali **võrkude** ja seejärel valige **Kohaliku võrgu** lehe ülaosas. Portaalis on kõige lihtsam viis vaadata vahemikud, mida te olete loendis on lehel **Redigeeri** . Teie vahemike vaatamiseks valige soovitud VNet ja klõpsake nuppu **Redigeeri** lehe allosas.

1. Klõpsake lehel **teie kohalikus võrgus üksikasjad** ei muuda. Klõpsake lehe allservas järgmise noolt.

1. Klõpsake lehel **Määrake aadress ruumi** muudatuste võrgu aadress ruumi. Klõpsake oma konfiguratsiooni salvestamiseks märke.

## <a name="how-to-view-gateway-traffic"></a>Kuidas vaadata lüüsi liikluse

Saate vaadata oma lüüsi ja lüüsi liikluse oma virtuaalse võrgu **armatuurlaua** lehe kaudu.

**Armatuurlaua** lehel saate vaadata järgmist:

- Andmed, mis on teie lüüsi nii andmete ja välja andmete läbib summa.

- DNS-serverid, virtuaalse võrgu jaoks määratud nimed.

- Lüüsi ja seadme VPN-i vahelise ühenduse.

- Ühiskasutusega võti, mille seadmesse VPN ühenduse lüüs konfigureerimiseks.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Kuidas muuta oma Gateway VPN marsruutimise tüüp

Kuna mõned konfiguratsiooni Ühenduvus saadaval ainult teatud lüüsi marsruutimine, võib juhtuda, et peate lüüsi VPN marsruutimise tüüpi olemasoleva VPN-lüüsi muuta. Näiteks võite lisada juba olemasoleva saitide ühenduse, mis on staatiline lüüsi punkti – saidile Ühenduvus. Dünaamiliste lüüsi vajavad punkti saidi ühendused. See tähendab, et konfigureerida P2S ühenduse, on teil muuta oma lüüsi VPN marsruutimise tüüp staatiline dünaamiliseks.

Kui teil on vaja muuta lüüsi VPN marsruutimise tüüp, kuvatakse olemasoleva lüüsi kustutamiseks, ja seejärel looge uus lüüs uue marsruutimise tüübiga. Teil pole vaja kustutada kogu virtuaalse võrgu lüüsi marsruutimise tüüpi muuta.

Enne muutmist lüüsi VPN marsruutimise tüüp, kindlasti veenduge, et seadme VPN toetab marsruutimise tüüp, mida soovite kasutada. Laadige uus marsruutimise konfiguratsiooni näidiseid ja märkige VPN seadme nõuded, leiate teemast [Kohta VPN seadmete virtuaalse võrguühendus](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Kui kustutate virtuaalse võrgu VPN-lüüsi, lüüsi määratud VIP vabastatakse. Kui lüüsi uuesti luua uue VIP on määratud seda.

1. **Kustutage olemasolev VPN-lüüsi.**

    **Armatuurlaua** lehel virtuaalse võrgu jaoks, liikuge lehe allserva ja klõpsake **Lüüsi kustutamine**. Oodake, kuni teate, et lüüsi on kustutatud. Kui saate teate ekraanil, et lüüsi on kustutatud, saate luua uue lüüsi.

1. **Saate luua uue VPN-lüüsi.**

    Toiminguga lehe ülaosas saate luua uue lüüsi: [VPN-lüüsi loomine](#create-a-vpn-gateway).


## <a name="next-steps"></a>Järgmised sammud

Saate lisada virtuaalse võrgu virtuaalmasinates. Vaadake, [Kuidas luua kohandatud virtuaalse masina](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Kui soovite konfigureerida punkti saidi VPN-ühendus, lugege teemat [konfigureerimine punkti saidi VPN-ühendus](vpn-gateway-point-to-site-create.md).

 
