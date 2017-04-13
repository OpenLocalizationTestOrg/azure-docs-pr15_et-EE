
<properties
   pageTitle="Vastastikuste laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli Azure klassikaline portaalis Interneti loomise alustamiseks | Microsoft Azure'i"
   description="Saate teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli Azure klassikaline portaalis"
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
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Vastastikuste laadi koormusetasakaalustusteenuse (klassikaline) Azure klassikaline portaalis Interneti loomise alustamiseks

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Samuti saate [teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse Azure'i ressursihaldur abil](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Häälestamine on Interneti-ühendusega laadi koormusetasakaalustusteenuse virtuaalmasinates jaoks

Laadimiseks saldo võrguliiklust Interneti kaudu üle virtuaalmasinates, pilveteenus, peate looma koormusetasakaalustusega kogum. See toiming eeldab, et olete juba loonud selle virtuaalmasinates ja, et need on kõik sama pilveteenuses.

**Virtuaalmasinates koormusetasakaalustusega kogumi konfigureerimine**

1. Klassikaline portaalis Azure'i **Virtuaalmasinates**nuppu, ja klõpsake koormusetasakaalustusega määramine virtuaalse masina nimi.

2. Klõpsake nuppu **lõpp-punktid**ja seejärel klõpsake nuppu **Lisa**.

3. Klõpsake lehel **Lisa virtuaalse masina lõpp-punkti** paremnoolt.

4. **Määrake lõpp-punkti üksikasjade** lehel järgmist.

    * **Nime**, tippige lõpp-punkti nimi või valige loendist eelmääratletud lõpp-punktide jaoks levinud Protokollid nimi.
    * **Protocol (protokoll)**, valige kas TCP või UDP, lõpp-punkti tüüp nõutud protokoll vastavalt vajadusele.
    * Sisestage **avaliku Port ja privaatne Port**pordinumbrid, mida soovite kasutada, virtuaalse masina vastavalt vajadusele. Saate privaatne pordi ja tulemüüri reeglid virtual arvutisse ümber suunata liikluse viisil, mis sobib teie rakendus. Privaatne pordi võib olla sama, mis avaliku pordi. Näiteks lõpp-punkti võrguliikluse (HTTP), võite määrata pordi 80 avalike ja privaatvõ port.

5. Valige **Loo koormusetasakaalustusega kogum**, ja seejärel klõpsake paremnoolt.

6. Lehe **konfigureerimine koormusetasakaalustusega määramine** , tippige koormusetasakaalustusega jaoks soovitud nimi ja seejärel määrake juures toimimist Azure'i laadimine koormusetasakaalustusteenuse väärtused. Laadi koormusetasakaalustusteenuse kasutatakse sondid virtuaalmasinates koormusetasakaalustusega määramine saadaolevaid saada liiklust.

7. Klõpsake märkeruutu koormusetasakaalustusega lõpp-punkti loomiseks. Kuvatakse **Jah** **lõpp-punktid** leht virtuaalse masina veerus **koormusetasakaalustusega hulga nimi** .

8. Portaalis klõpsake **Virtuaalmasinates**, klõpsake selle nime, mis täiendavad virtuaalse masina koormusetasakaalustusega määramine, klõpsake nuppu **lõpp-punktid**ja seejärel klõpsake nuppu **Lisa**.

9. Lehel **Lisa lõpp virtuaalse masina** nuppu **Lisa lõpp-punkti olemasoleva koormusetasakaalustusega häälestada**, valige koormusetasakaalustusega komplekti nimi ja seejärel klõpsake paremnoolt.

10. Lehe **Määrake lõpp-punkti üksikasjad** tippige lõpp-punkti nimi ja seejärel klõpsake märkeruutu.

Jaoks täiendavate virtuaalmasinates koormusetasakaalustusega määramine, korrake juhiseid 8-10.



## <a name="next-steps"></a>Järgmised sammud

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-ps.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)

