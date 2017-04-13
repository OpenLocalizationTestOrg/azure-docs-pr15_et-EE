<properties
   pageTitle="Luua ka Interneti-ühendusega laadi koormusetasakaalustusteenuse rakenduses ressursihaldur Azure'i portaalis | Microsoft Azure'i"
   description="Saate teada, kuidas luua ka Interneti-ühendusega laadi koormusetasakaalustusteenuse rakenduses ressursihaldur Azure'i portaalis"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Interneti-ühendusega laadi koormusetasakaalustusteenuse Azure portaali loomine

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel. Samuti saate [teada, kuidas luua Interneti-ühendusega laadi koormusetasakaalustusteenuse klassikaline juurutamise abil](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

See hõlmab teha, et luua laadi koormusetasakaalustusteenuse ja selgitatakse üksikasjalikult, mis tehakse tegemise eesmärk on üksikute tööülesannete järjestust.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Mida on vaja luua ka Interneti-ühendusega laadi koormusetasakaalustusteenuse?

Peate loomine ja konfigureerimine järgmiste objektide laadi koormusetasakaalustusteenuse juurutamiseks.

- IP-konfiguratsiooni ees - sisaldab avaliku IP-aadresside sissetulevat võrguliiklust.

- Tagaandmebaas aadress pool - sisaldab virtuaalmasinates saada võrguliiklust laadi koormusetasakaalustusteenuse võrgu liidesed (NICs).

- Reeglite koormusetasandus - sisaldab vastendamise avaliku porti laadi koormusetasakaalustusteenuse pordi tagaandmebaas aadress kogumi reeglid.

- Sissetulevad reeglid NAT - sisaldab avaliku porti koormusetasakaalustusteenuse laadimine ja teatud virtuaalse masina tagaandmebaas aadress rakenduskausta pordi vastendamise reeglid.

- Sondid – sisaldab seisund sondid kasutada saadavuse virtuaalmasinates eksemplaride tagaandmebaas aadress kogumi.

Saad Lisateavet laadi koormusetasakaalustusteenuse komponendid Azure'i ressursihaldur [Azure'i ressursihaldur kasutajatugi koormusetasakaalustusteenuse laadimine](load-balancer-arm.md).


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Laadi koormusetasakaalustusteenuse Azure'i portaalis häälestamine

> [AZURE.IMPORTANT] Selles näites eeldab, et teil on virtuaalse võrgust, mida nimetatakse **myVNet**. Vaadake seda teha [virtuaalse võrgu loomine](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) . Seda ka eeldab, on mõne alamvõrgu jooksul **myVNet** nimega **LB alamvõrgu olla** ja kahe VMs nimega **web1** ja **web2** kuuluvad vastavalt sama kättesaadavus määramine nimega **myAvailSet** **myVNet**. Vaadake [seda linki](../virtual-machines/virtual-machines-windows-hero-tutorial.md) VMs loomiseks.


1. Brauseri kaudu liikuge Azure'i portaal: [http://portal.azure.com](http://portal.azure.com) ja Azure'i kontosse sisse logima.

2. Valige ekraani vasakus servas ülemise **Uus** > **Networking** > **Laadi koormusetasakaalustusteenuse.**

3. **Loo laadimine koormusetasakaalustusteenuse** tera, tippige oma laadi koormusetasakaalustusteenuse nimi. Tehke seda nimetatakse **myLoadBalancer**.

4. Klõpsake jaotises **Tüüp**valige **avalik**.

5. Jaotises **avaliku IP-aadressi**luua uue avaliku IP nimetatakse **myPublicIP**.

6. Valige jaotises ressursirühm, **myRG**. Seejärel valige soovitud **asukohta**ja seejärel klõpsake nuppu **OK**. Laadi koormusetasakaalustusteenuse alustatakse juurutada ja võtab edukalt lõpule juurutamise paar minutit.

![Ressursirühm, laadi koormusetasakaalustusteenuse värskendamine](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Tagaandmebaas aadress kausta loomine

1. Kui teie laadi koormusetasakaalustusteenuse on edukalt kasutusele, valige see oma ressursse sees. Valige jaotises sätted kirjutamata kaustu. Tippige oma kirjutamata kausta nimi. Seejärel klõpsake nuppu **Lisa** üles tera, mis näitab üles nihutada.

2. Klõpsake **Lisa virtuaalse masina** **Lisa kirjutamata pool** tera.  **Määrake soovitud kättesaadavus** alusel **kättesaadavus** ja valige **myAvailSet**. Järgmiseks valige tera Virtuaalmasinates jaotises **Valige soovitud virtuaalmasinates** ja klõpsake **web1** ja **web2**, koormusetasandus loodud kaks VMs. Veenduge, et mõlemad on sinine märkige ruudud vasakule, nagu on näidatud järgmisel pildil. Klõpsake nuppu **Valige** selle järgneb **Valige virtuaalse masinad** blade OK ja seejärel **OK** **lisamine kirjutamata pool** tera tera sisse.

    ![Taustväärtus aadress pool - lisamine ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Veenduge, et teie teatised ripploendis on värskendust laadi koormusetasakaalustusteenuse kirjutamata rakenduskausta Lisaks värskendamine võrgu liidese VMs **web1** ja **web2**salvestamise kohta.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Mõne juures, LB reegel ja NAT reeglite loomine

1. Looge seisundi juures.

    Valige jaotises sätted on teie laadi koormusetasakaalustusteenuse sondid. Seejärel klõpsake nuppu **Lisa** tera ülaosas asuv.

    On konfigureerida juures on kaks võimalust: HTTP või TCP. Selles näites HTTP, kuid TCP saab konfigureerida sarnasel viisil.
    Vajaliku teabe värskendamine. Nagu mainitud, laaditakse **myLoadBalancer** saldo liiklust Port 80. Valitud tee on HealthProbe.aspx, intervall on 15 minutit ja vigane on 2. Kui olete lõpetanud, klõpsake nuppu **OK** , et luua selle juures.

    "I" kursorit ikooni Lisateavet nende üksikute konfiguratsioone ja kuidas need saab muuta, et kasutada neid oma nõuetele.

    ![Mõne juures lisamine](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Laadi koormusetasakaalustusteenuse reegli loomine.

    Klõpsake Laadi tasakaalustamiseks sätted jaotises teie laadi koormusetasakaalustusteenuse reeglid. Uue tera, nuppu **Lisa**. Reegli nimi. Siin on HTTP. Valige frontend port ja port Taustväärtus. Siin on 80 valitud mõlemad. Valige oma kirjutamata rakenduskausta ja selle juures nagu varem loodud **HealthProbe** **LB-taustväärtus** . Muud konfiguratsioone saab seada vastavalt oma vajadustele. Klõpsake Laadi tasakaalustamiseks reegli salvestamiseks nuppu OK.

    ![Koormus tasakaalustamiseks reegli lisamine](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Sissetuleva NAT reeglite loomine

    Klõpsake oma laadi koormusetasakaalustusteenuse sätted jaotises NAT sissetulevad reeglid. Uus laba, klõpsake nuppu **Lisa**. Klõpsake nime sissetulevate NAT reegli. Tehke seda nimetatakse **inboundNATrule1**. Sihtkoha peaks olema varem loodud avaliku IP. Valige jaotises teenuse kohandatud ja valige Protocol (protokoll), mida soovite kasutada. Siin on valitud TCP. Sisestage pordi, 3441, ja Target port sel juhul 3389. Klõpsake selle reegli salvestamiseks nuppu OK.

    Kui esimese reegli on loodud, korrake seda toimingut teise sissetuleva NAT reegli nimega inboundNATrule2 port 3442 Target porti 3389.

    ![Sissetuleva NAT reegli lisamine](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Laadi koormusetasakaalustusteenuse eemaldamine

Laadi koormusetasakaalustusteenuse kustutamiseks valige Laadi koormusetasakaalustusteenuse, mille soovite eemaldada. Klõpsake *Laadi koormusetasakaalustusteenuse* labale tera ülaosas asuv **kustutada** . Valige **Jah** küsimise.

## <a name="next-steps"></a>Järgmised sammud

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-cli.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)
