<properties
   pageTitle="Rakenduste portaali kaudu portaali loomine | Microsoft Azure'i"
   description="Saate teada, kuidas luua rakenduste portaali portaali abil"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
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

# <a name="create-an-application-gateway-by-using-the-portal"></a>Rakenduste portaali kaudu portaali loomine

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-gateway-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-gateway-arm.md)
- [Azure'i klassikaline PowerShelli](application-gateway-create-gateway.md)
- [Azure'i ressursihaldur Mall](application-gateway-create-gateway-arm-template.md)
- [Azure'i CLI](application-gateway-create-gateway-cli.md)

Azure'i rakenduse lüüs on kiht-7 laadi koormusetasakaalustusteenuse. Kas nad on kohapealse ja pilveteenuse pakub Tõrkesiirde jõudlus marsruutimise HTTP päringuid serveritest, vahel. Rakenduse lüüsi pakub paljusid rakenduse kohaletoimetamise kontrolleril (ADC) funktsioonid, sh HTTP koormusetasakaalustuseks, seansi küpsis vastavalt osaleja, turvasoklite kiht (SSL) offload, kohandatud seisund sondid, mitme saidi tugi ja paljud teised. Toetatud funktsioonide täieliku loendi saamiseks külastage [Rakenduse lüüsi ülevaade](application-gateway-introduction.md)

## <a name="scenario"></a>Stsenaarium

Selle stsenaariumi korral saate teada, kuidas luua rakenduste portaali Azure'i portaalis.

Selle stsenaariumi järgmist:

- Looge Keskmine rakenduse lüüsi kaks korda.
- Luua nimega AdatumAppGatewayVNET koos reserveeritud CIDR plokk 10.0.0.0/16 virtuaalse võrgu.
- Looge alamvõrku, mis on nimega Appgatewaysubnet, mis kasutab 10.0.0.0/28 oma CIDR plokk.
- Jaoks offload SSL-i serdi konfigureerimine.

![Stsenaarium näide][scenario]

>[AZURE.IMPORTANT] Rakenduse lüüsi, sh kohandatud seisund konfigureerimise sondid, kirjutamata rakenduskausta aadressid ja täiendavad reeglid on konfigureeritud pärast lüüsi rakendus on konfigureeritud ja mitte algse juurutamisel.

## <a name="before-you-begin"></a>Enne alustamist

Azure'i rakenduse lüüsi nõuab eraldi alamvõrgu. Virtuaalse võrgu loomisel veenduge, et jätta ruumi aadress on mitu alamvõrku. Kui juurutate rakenduste portaali on alamvõrku, ainult täiendav taotlus lüüsid saavad alamvõrgu lisada.

## <a name="create-the-application-gateway"></a>Lüüsi rakenduse loomine

### <a name="step-1"></a>Samm 1

Liikuge Azure'i portaal, klõpsake nuppu **Uus** > **Networking** > **Rakenduse lüüsi**

![Rakenduste portaali loomine][1]

### <a name="step-2"></a>Samm 2

Järgmine täitke lüüsi rakenduse põhiteavet. Kui olete lõpetanud, klõpsake **OK**

Põhilised seaded jaoks vajalik teave on:

- **Nimi** – rakenduse lüüsi nimi.
- **Esimese taseme** – see on rakenduse lüüsi astme. Kahe nimetatud astme on saadaval, **WAF** - ja **standardversiooni**. WAF lubab web rakenduse tulemüüri funktsiooni.
- **SKU suurus** – see säte on rakenduse lüüsi suurus, saadaolevate suvandite (**väike**, **Keskmine**ja **suur**). *Väike pole saadaval, kui valitakse WAF taseme*
- **Arvu** - eksemplaride arv, see väärtus peaks olema arvu 2 kuni 10.
- **Ressursirühm** - ressursirühm hoida rakenduse lüüsi, võib olla ressursi olemasolevasse rühma või luua uus.
- **Asukoht** – rakenduse lüüsi, piirkond on ressursirühma samas kohas. *Asukoht on oluline kui virtuaalse võrgu ja avaliku IP peab olema lüüsi samasse asukohta*.

![kus on kuvatud Blade põhisätted][2]

>[AZURE.NOTE] Eksemplari arv 1 saab valida katsetamiseks. See on oluline teada, et mis tahes eksemplari arv jaotises kahel ei kuulu SLA ja on seega pole soovitatav. Väike lüüsid on kasutusel arendaja test, mitte tootmiseks.

### <a name="step-3"></a>Samm 3

Kui põhisätted on määratletud, on järgmiseks määratleda virtuaalse võrgu kasutada. Virtuaalse võrgu maja rakendus, mis rakenduse lüüsi laadimine jaoks.

Klõpsake käsku **Vali virtuaalse võrgu** konfigureerida virtuaalse võrgu.

![Blade rakenduse lüüsi sätete kuvamine][3]

### <a name="step-4"></a>Samm 4

*Valige virtuaalse võrgu* tera, klõpsake nuppu **Loo uus**

Ajal ei selgitatud selle stsenaariumi, võib sel hetkel valitud olemasolevat virtuaalse võrku.  Kui kasutatakse olemasolevat virtuaalse võrku, on oluline teada, et virtuaalse võrgu vajab mõnda tühja alamvõrku või alamvõrku ainult rakenduse lüüsi ressursse kasutada.

![Valige virtuaalse võrgu blade][4]

### <a name="step-5"></a>Juhis 5

Sisestage välja **Loomine virtuaalse võrgu** tera teave, nagu on kirjeldatud eelmises [stsenaarium](#scenario) kirjeldus.

![Luua virtuaalse võrgu blade sisestatud teave][5]

### <a name="step-6"></a>Juhist 6

Kui virtuaalse võrgu on loodud, on järgmiseks määratleda ees IP rakenduse lüüsi. Selles etapis valik on avalik või privaatne IP-aadressi jaoks soovitud ees vahel. Valik sõltub sellest, kas rakendus on Interneti suunatud või sisemise ainult. See stsenaarium eeldab, kasutades avaliku IP-aadressi. Privaatne IP-aadress valimiseks saate klõpsata nuppu **era** . Valitakse automaatselt määratud IP-aadress või klõpsake ühte käsitsi sisestamiseks **Valige teatud privaatne IP-aadress** ruut.

### <a name="step-7"></a>Juhis 7

Klõpsake käsku **Vali avaliku IP-aadressi**. Kui olemasolev avalik IP-aadress on saadaval saab valida sel hetkel, selle stsenaariumi luua uue avaliku IP-aadressi. Klõpsake nuppu **Loo uus**

![Valige avaliku IP-aadressi blade][6]

### <a name="step-8"></a>Samm 8

Edasi anda avaliku IP-aadressi sõbralik nimi ja klõpsake nuppu **OK**

![Avaliku IP-aadressi blade loomine][7]

### <a name="step-9"></a>Samm 9

Viimase sätete konfigureerimiseks rakenduste portaali loomisel on kuulajale konfiguratsioon.  Kui kasutatakse **http** , midagi ei jää konfigureerimine ja saate klõpsata **OK** . Kasutage **https** täpsemaks konfigureerimine on nõutav.

**HTTPS-i**kasutamiseks on vaja sert. Serdi privaatvõti on vaja vaja pfx ekspordi serti ja parool.

### <a name="step-10"></a>Samm 10

Klõpsake **HTTPS**, klõpsake **tekstivälja **serdi üleslaadimine PFX** kõrval asuvat kaustaikooni** .
Liikuge pfx serdifail oma failisüsteemis. Kui see on valitud, Pange serdi sõbralik nimi ja tippige parool pfx-fail.

Kui täielik nuppu **OK** , et rakenduste lüüsi sätete ülevaatamine.

![Kuulajale konfigureerimine jaotises sätted blade][9]

### <a name="step-11"></a>Samm 11

Vaadake läbi kokkuvõtteleht, ja klõpsake nuppu **OK**.  Nüüd on rakenduse lüüsi järjekorda ja loodud.

### <a name="step-12"></a>Samm 12

Kui lüüsi rakendus on loodud, liikuge selle portaali jätkamiseks rakenduse lüüsi konfigureerimine.

![Rakenduse lüüsi Ressursivaade][10]

Need juhised luua taotluse lüüsi vaikesätete kuulajale, taustväärtus pool, taustväärtus http sätted ja reegleid. Saate muuta neid sätteid vastavalt pärast selle ettevalmistamise on eduka juurutamise.

## <a name="next-steps"></a>Järgmised sammud

Selle stsenaariumi loob rakenduse lüüs. Järgmised toimingud on konfigureerida rakendus lüüsi rakenduskausta liikmete lisamine, muutmine sätted ja reguleerimine reeglite lüüsi seda töötaks õigesti.

Saate teada, kuidas luua kohandatud seisund sondid külastades, [luua kohandatud seisundi juures](application-gateway-create-probe-portal.md)

Saate teada, kuidas konfigureerida SSL-i mahalaadimine ja võtta kulukas SSL dekrüptimine välja veebi serveri külastades [Offload SSL-i konfigureerimine](application-gateway-ssl-portal.md)

Saate teada, kuidas kaitsta rakenduste [Web rakenduse tulemüüri](application-gateway-webapplicationfirewall-overview.md) funktsioon rakenduse lüüsi.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
