<properties
   pageTitle="Rakenduste portaali loomine tulemüüriga web rakenduse portaalis | Microsoft Azure'i"
   description="Saate teada, kuidas luua rakenduste portaali web rakenduse tulemüüri portaali abil"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Luua rakenduste portaali web rakenduse tulemüüri portaali abil

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-web-application-firewall-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-web-application-firewall-powershell.md)

Web rakenduse tulemüüri (WAF) Azure'i rakenduse lüüsi kaitseb veebirakenduste levinud veebipõhine eest nagu SQL süst, saitidevaheline skriptimine eest ja seansi hijacks. Veebirakenduse kaitseb paljud selle OWASP ülemine 10 levinud web kohtade.

Azure'i rakenduse lüüs on kiht-7 laadi koormusetasakaalustusteenuse. Kas nad on kohapealse ja pilveteenuse pakub Tõrkesiirde jõudlus marsruutimine HTTP päringuid serveritest, vahel.
Rakenduse pakub paljusid rakenduse kohaletoimetamise kontrolleril (ADC) funktsioonid, sh HTTP koormusetasakaalustuseks, seansi küpsise vastavalt osaleja, turvasoklite kiht (SSL) offload, kohandatud seisund sondid, mitme saidi tugi ja paljud teised.
Toetatud funktsioonide täieliku loendi saamiseks külastage [Rakenduse lüüsi ülevaade](application-gateway-introduction.md)

## <a name="scenarios"></a>Stsenaariumid

Selles artiklis on kahte järgmist stsenaariumi:

Esimese stsenaariumi puhul saate teada, lisada [web rakenduse tulemüüri olemasoleva rakenduste portaali](#add-web-application-firewall-to-an-existing-application-gateway).

Teise võimalusena saate teada, [web rakenduse tulemüüriga rakenduste portaali](#create-an-application-gateway-with-web-application-firewall) loomine

![Stsenaarium näide][scenario]

>[AZURE.NOTE] Rakenduse lüüsi, sh kohandatud seisund konfigureerimise sondid, kirjutamata rakenduskausta aadressid ja täiendavad reeglid on konfigureeritud pärast lüüsi rakendus on konfigureeritud ja mitte algse juurutamisel.

## <a name="before-you-begin"></a>Enne alustamist

Azure'i rakenduse lüüsi nõuab eraldi alamvõrgu. Virtuaalse võrgu loomisel veenduge, et jätta ruumi aadress on mitu alamvõrku. Kui juurutate rakenduste portaali on alamvõrku, ainult täiendav taotlus lüüsid saavad alamvõrgu lisada.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Olemasoleva rakenduste portaali tulemüüri web rakenduse lisamine

Selle stsenaariumi värskendab olemasoleva rakenduste portaali tulemüüri web rakenduse toetamiseks vältimise režiim.

### <a name="step-1"></a>Samm 1

Liikuge Azure'i portaal, valige olemasolev rakenduste portaali.

![Rakenduste portaali loomine][1]

### <a name="step-2"></a>Samm 2

Klõpsake **konfiguratsiooni** ja rakenduse lüüsi sätete värskendamine. Kui olete lõpetanud, klõpsake **salvestamine**

On värskendada olemasoleva rakenduste portaali toetamiseks web rakenduse tulemüüri sätted.

- **Esimese taseme** - taseme, mis on valitud peab olema **WAF** toetamiseks web rakenduse tulemüüri
- **SKU suurus** – see säte on rakenduse lüüsi web rakenduse tulemüüriga suurus, saadaolevate suvandite (**Keskmine** ja **suur**).
- **Tulemüüri olek** – see säte keelab või lubab web rakenduse tulemüüri.
- **Tulemüüri režiim** – see säte on kuidas web rakenduse tulemüüri tegeleb pahatahtlik liikluse. **Tuvastamise** režiim ainult logib sündmusi, kus **vältimise** režiimiga logib sündmused ja lõpetab pahatahtlik liikluse.

![kus on kuvatud Blade põhisätted][2]

>[AZURE.NOTE] Web tulemüüri logid kuvamiseks peab olema lubatud diagnostika- ja ApplicationGatewayFirewallLog valitud. Eksemplari arv 1 saab valida katsetamiseks. See on oluline teada, et iga eksemplari arv jaotises kahel ei kuulu SLA ja on seega pole soovitatav. Väike lüüsid pole saadaval, kasutades web rakenduse tulemüüri.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Rakenduse web tulemüüriga rakenduste portaali loomine

Selle stsenaariumi järgmist:

- Saate luua kaks korda Keskmine web rakenduse tulemüüri rakenduste portaali.
- Luua nimega AdatumAppGatewayVNET koos reserveeritud CIDR plokk 10.0.0.0/16 virtuaalse võrgu.
- Looge alamvõrku, mis on nimega Appgatewaysubnet, mis kasutab 10.0.0.0/28 oma CIDR plokk.
- Jaoks offload SSL-i serdi konfigureerimine.

### <a name="step-1"></a>Samm 1

Liikuge Azure'i portaal, klõpsake nuppu **Uus** > **Networking** > **Rakenduse lüüsi**

![Rakenduste portaali loomine][1-1]

### <a name="step-2"></a>Samm 2

Järgmine täitke lüüsi rakenduse põhiteavet. Ärge unustage valida **WAF** mis. Kui olete lõpetanud, klõpsake **OK**

Põhilised seaded jaoks vajalik teave on:

- **Nimi** – rakenduse lüüsi nimi.
- **Esimese taseme** - taseme rakenduse lüüsi, Valikud on (**Standard** - ja **WAF**). Web rakenduse tulemüüri kättesaadav ainult WAF astme.
- **SKU suurus** – see säte on rakenduse lüüsi suurus, saadaolevate suvandite (**Keskmine** ja **suur**).
- **Arvu** - eksemplaride arv, see väärtus peaks olema arvu **2** kuni **10**.
- **Ressursirühm** - ressursirühm hoida rakenduse lüüsi, võib olla ressursi olemasolevasse rühma või luua uus.
- **Asukoht** – rakenduse lüüsi, piirkond on ressursirühma samas kohas. *Asukoht on oluline kui virtuaalse võrgu ja avaliku IP peab olema lüüsi samasse asukohta*.

![kus on kuvatud Blade põhisätted][2-2]

>[AZURE.NOTE] Eksemplari arv 1 saab valida katsetamiseks. See on oluline teada, et iga eksemplari arv jaotises kahel ei kuulu SLA ja on seega pole soovitatav. Väike lüüside web rakenduse tulemüüri stsenaariumid ei toetata.

### <a name="step-3"></a>Samm 3

Kui põhisätted on määratletud, järgmise sammuna tuleb määratleda virtuaalse võrgu kasutada. Virtuaalse võrgu maja rakendus, mis rakenduse lüüsi laadimine jaoks.

Klõpsake käsku **Vali virtuaalse võrgu** konfigureerida virtuaalse võrgu.

![Blade rakenduse lüüsi sätete kuvamine][3]

### <a name="step-4"></a>Samm 4

**Valige virtuaalse võrgu** tera, klõpsake nuppu **Loo uus**

Ajal ei selgitatud selle stsenaariumi, võib sel hetkel valitud olemasolevat virtuaalse võrku.  Kui kasutatakse olemasolevat virtuaalse võrku, on oluline teada, et virtuaalse võrgu on vaja mõnda tühja alamvõrgu või alamvõrgu ainult rakenduse lüüsi ressurssi kasutada.

![Valige virtuaalse võrgu blade][4]

### <a name="step-5"></a>Juhis 5

Täitke võrgu teave **Loomine virtuaalse võrgu** tera kirjeldatud eelmise [stsenaariumi](#scenario) kirjeldust.

![Luua virtuaalse võrgu blade sisestatud teave][5]

### <a name="step-6"></a>Juhist 6

Kui virtuaalse võrgu on loodud, on järgmiseks määratleda ees IP rakenduse lüüsi. Selles etapis valik on avalik või privaatne IP-aadressi jaoks soovitud ees vahel. Valik sõltub sellest, kas rakendus on Interneti suunatud või sisemise ainult. See stsenaarium eeldab, kasutades avaliku IP-aadressi. Privaatne IP-aadress valimiseks saate klõpsata nuppu **era** . Valitakse automaatselt määratud IP-aadress või klõpsake ühte käsitsi sisestamiseks **Valige teatud privaatne IP-aadress** ruut.

Klõpsake käsku **Vali avaliku IP-aadressi**. Kui olemasolev avalik IP-aadress on saadaval saab valida sel hetkel, selle stsenaariumi luua uue avaliku IP-aadressi. Klõpsake nuppu **Loo uus**

![Valige avaliku IP-aadressi blade][6]

### <a name="step-7"></a>Juhis 7

Edasi anda avaliku IP-aadressi sõbralik nimi ja klõpsake nuppu **OK**

![Avaliku IP-aadressi blade loomine][7]

### <a name="step-8"></a>Samm 8

Järgmisena saate seadistada kuulajale konfigureerimine.  Kui kasutatakse **http** , midagi ei jää konfigureerimine ja saate klõpsata **OK** . Kasutage **https** täpsemaks konfigureerimine on nõutav.

**HTTPS-i**kasutamiseks on vaja sert. Serdi privaatvõti on vaja mõnda pfx eksport esitatava serdi vajadustele ja parooli faili.

Klõpsake **HTTPS**, klõpsake **tekstivälja **serdi üleslaadimine PFX** kõrval asuvat kaustaikooni** .
Liikuge pfx serdifail oma failisüsteemis. Kui see on valitud, Pange serdi sõbralik nimi ja tippige parool pfx-fail.

Kui täielik nuppu **OK** , et rakenduste lüüsi sätete ülevaatamine.

![Kuulajale konfigureerimine jaotises sätted blade][8]

### <a name="step-9"></a>Samm 9

**WAF** teatud sätete konfigureerimine.

- **Tulemüüri olek** – see säte lülitab WAF sisse või välja.
- **Tulemüüri režiim** – see säte määrab toimingud WAF võtab pahatahtlik liikluse. Kui valitud on **avastamine** , logitakse ainult liikluse.  Kui valitud on **vältimise** , liikluse on sisse logitud ja 403 volitamata on peatatud.

![veebirakenduse tulemüüri sätted][9]

### <a name="step-10"></a>Samm 10

Vaadake läbi kokkuvõtteleht, ja klõpsake nuppu **OK**.  Nüüd on rakenduse lüüsi järjekorda ja loodud.

### <a name="step-12"></a>Samm 12

Kui rakenduse lüüsi on loodud, liikuge selle portaali jätkata rakenduse lüüsi konfigureerimine.

![Rakenduse lüüsi Ressursivaade][10]

Need juhised luua taotluse lüüsi vaikesätete kuulajale, taustväärtus pool, taustväärtus http sätted ja reegleid. Saate muuta neid sätteid vastavalt pärast selle ettevalmistamise on eduka juurutamise

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas konfigureerida diagnostikalogimine logige sündmusi, mis on avastamisel või Web rakenduse tulemüüriga külastades [Rakenduse lüüsi diagnostika](application-gateway-diagnostics.md)

Saate teada, kuidas luua kohandatud seisund sondid külastades, [luua kohandatud seisundi juures](application-gateway-create-probe-portal.md)

Siit saate teada, kuidas konfigureerida SSL-i mahalaadimine ja võtta kulukas SSL dekrüptimine välja veebi serveri külastades [Offload SSL-i konfigureerimine](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png