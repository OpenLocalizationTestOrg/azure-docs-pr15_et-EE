<properties
   pageTitle="Mõne sisemise koormuse koormusetasakaalustusteenuse rakenduses ressursihaldur Azure'i portaalis loomise alustamiseks | Microsoft Azure'i"
   description="Saate teada, kuidas luua ka sisemise koormuse koormusetasakaalustusteenuse rakenduses ressursihaldur Azure'i portaalis"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Azure'i portaalis on sisemine laadi koormusetasakaalustusteenuse loomine

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassikaline juurutamise mudel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Sisemise koormuse koormusetasakaalustusteenuse, mis Azure'i portaalis loomise alustamiseks

Järgmiste juhiste abil saate luua ka sisemise koormuse koormusetasakaalustusteenuse, Azure'i portaalis.

1. Avage brauser, liikuge [Azure portaali](http://portal.azure.com)ja Azure kontoga sisselogimine.
2. Kuva ülemises vasakus servas nuppu **Uus** > **Networking** > **Laadi koormusetasakaalustusteenuse**.
3. **Loo laadimine koormusetasakaalustusteenuse** tera, sisestage **nimi** oma laadi koormusetasakaalustusteenuse.
4. Klõpsake jaotises **värviskeemi**, **sisemine**.
5. Klõpsake **Virtual võrk**ja valige, kuhu soovite luua laadi koormusetasakaalustusteenuse virtuaalse võrgu.

    >[AZURE.NOTE] Kui te ei näe virtuaalse võrgu, mida soovite kasutada, märkige ruut Laadi koormusetasakaalustusteenuse jaoks kasutate, ja muuta selle **asukoha** .

6. Klõpsake **alamvõrgu**ja valige alamvõrku, kus soovite luua laadi koormusetasakaalustusteenuse.
7. All **IP address ülesande**klõpsake **dünaamilise** või **staatilise**, olenevalt sellest, kas soovite IP-aadress laadi koormusetasakaalustusteenuse määratud (staatiline) või mitte.

    >[AZURE.NOTE] Kui valite kasutada staatiline IP-aadress, on teil laadi koormusetasakaalustusteenuse aadressi ette.

8. Jaotises **ressursirühm** määramiseks nimi uue ressursirühma laadi koormusetasakaalustusteenuse, või **Valige olemasolev** nuppu ja ressursside olemasoleva rühma valimine.
9. Klõpsake nuppu **Loo**.

## <a name="configure-load-balancing-rules"></a>Laadi tasakaalustamiseks reeglite konfigureerimine

Pärast laadi koormusetasakaalustusteenuse loomise, liikuge laadi koormusetasakaalustusteenuse ressursi konfigureerida.
Peate esmalt tagaandmebaas aadressi pool ja konfigureerimiseks on juures enne konfigureerimist tasakaalustamiseks reegli koormus.

### <a name="step-1-configure-a-back-end-pool"></a>Samm 1: Konfigureerimine tagaandmebaas pool

1. Azure'i portaalis, klõpsake nuppu **Sirvi** > **soolise laadimine**ja seejärel klõpsake soovitud laadi koormusetasakaalustusteenuse loodud kohal.
2. Klõpsake **sätete** labale **kirjutamata kaustu**.
3. Tera **Taustväärtus aadress kaustu** , klõpsake nuppu **Lisa**.
4. **Lisa taustväärtus pool** tera, sisestage **nimi** kirjutamata rakenduskausta ja seejärel klõpsake nuppu **OK**.

### <a name="step-2-configure-a-probe"></a>Samm 2: Konfigureerimine on juures

1. Azure'i portaalis, klõpsake nuppu **Sirvi** > **soolise laadimine**ja seejärel klõpsake soovitud laadi koormusetasakaalustusteenuse loodud kohal.
2. Klõpsake **sätete** labale **sondid**.
3. **Sondid** tera, klõpsake nuppu **Lisa**.
4. **Lisa juures** tera, sisestage **nimi** on juures.
5. Valige jaotises **protokoll**, **HTTP** (jaoks veebisaitide) või **TCP** (vastavalt TCP muude rakenduste) jaoks.
6. Määrake jaotises **Port**pordi kasutada funktsiooni juures kasutamisel.
7. Määrake jaotises **tee** (ainult HTTP sondid), tee kasutamiseks on juures.
8. Määrake jaotises **intervall** kuidas sageli sond rakendus.
9. Määrake jaotises **vigane lävi**, kui palju katsete ei suuda enne kirjutamata virtuaalse masina on märgitud nii vigane.
10. Klõpsake nuppu **OK** , et luua juures.

### <a name="step-3-configure-load-balancing-rules"></a>Samm 3: Konfigureerimine tasakaalustamiseks reeglite laadimine

1. Azure'i portaalis, klõpsake nuppu **Sirvi** > **soolise laadimine**ja seejärel klõpsake soovitud laadi koormusetasakaalustusteenuse loodud kohal.
2. Klõpsake **sätete** tera, **koormusetasandus reeglid**.
3. Blade **koormusetasakaalustuseks reeglid** nuppu **Lisa**.
4. **Lisa laadimine tasakaalustavad reegli** tera, sisestage reegli **nimi** .
5. Valige jaotises **protokoll**, **HTTP** (jaoks veebisaitide) või **TCP** (vastavalt TCP muude rakenduste) jaoks.
6. Määrake jaotises **Port**laadi koormusetasakaalustusteenuse ühendada pordi kliendid.
7. Määrake jaotises **kirjutamata portide**, kasutatakse taustväärtus kogumi pordi (tavaliselt laadi koormusetasakaalustusteenuse port ja kirjutamata port on sama).
8. Valige jaotises **Taustväärtus pool**, teie loodud kohal taustväärtus kausta.
9. Valige **seanss püsimine**, kuidas soovite, et püsida.
10. Määrake jaotises **jõude ajalõpp (minutites)**, jõudeaja ajalõpu.
11. Klõpsake jaotises **Ujuv IP (saatja otsest server)**, **lubatud**või **keelatud** .
12. Klõpsake nuppu **OK**.

## <a name="next-steps"></a>Järgmised sammud

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)