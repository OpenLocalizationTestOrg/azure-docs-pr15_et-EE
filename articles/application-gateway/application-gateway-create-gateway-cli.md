<properties
   pageTitle="Rakenduste abil Azure'i CLI ressursihaldur sisse portaali loomine | Microsoft Azure'i"
   description="Saate teada, kuidas luua rakenduste portaali sisse ressursihaldur Azure'i CLI abil"
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

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Azure'i CLI abil rakenduste portaali loomine

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-gateway-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-gateway-arm.md)
- [Azure'i klassikaline PowerShelli](application-gateway-create-gateway.md)
- [Azure'i ressursihaldur Mall](application-gateway-create-gateway-arm-template.md)
- [Azure'i CLI](application-gateway-create-gateway-cli.md)

Azure'i rakenduse lüüs on kiht-7 laadi koormusetasakaalustusteenuse. Kas nad on kohapealse ja pilveteenuse pakub Tõrkesiirde jõudlus marsruutimine HTTP päringuid serveritest, vahel. Rakenduse lüüsi on rakenduse kohaletoimetamise järgmised funktsioonid: HTTP tasakaalustamiseks, seansi küpsise vastavalt osaleja ja turvasoklite kiht (SSL) offload, kohandatud seisund sondid laadimine ja toetab mitut saidi jaoks.

## <a name="prerequisite-install-the-azure-cli"></a>Nõutav: Installige Azure'i CLI

Selles artiklis toimingute tegemiseks on vaja [installida Mac-arvutisse, Linux, ja (Azure'i CLI) Windows Azure'i käsurea liides](../xplat-cli-install.md) ja [Azure](../xplat-cli-connect.md)sisselogimiseks peate. 

> [AZURE.NOTE] Kui teil pole Azure'i konto, peate ühte. Valige Logi [tasuta prooviversiooni siin](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Stsenaarium

Selle stsenaariumi korral saate teada, kuidas luua rakenduste portaali Azure'i portaalis.

Selle stsenaariumi järgmist:

- Saate luua kaks korda Keskmine rakenduste portaali.
- Luua nimega AdatumAppGatewayVNET koos reserveeritud CIDR plokk 10.0.0.0/16 virtuaalse võrgu.
- Looge alamvõrku, mis on nimega Appgatewaysubnet, mis kasutab 10.0.0.0/28 oma CIDR plokk.
- Jaoks offload SSL-i serdi konfigureerimine.

![Stsenaarium näide][scenario]

>[AZURE.NOTE] Rakenduse lüüsi, sh kohandatud seisund konfigureerimise sondid, kirjutamata rakenduskausta aadressid ja täiendavad reeglid on konfigureeritud pärast lüüsi rakendus on konfigureeritud ja mitte algse juurutamisel.

## <a name="before-you-begin"></a>Enne alustamist

Azure'i rakenduse lüüsi nõuab eraldi alamvõrgu. Virtuaalse võrgu loomisel veenduge, et jätta ruumi aadress on mitu alamvõrku. Kui juurutate rakenduste portaali on alamvõrku, ainult täiendav taotlus lüüsid saavad alamvõrgu lisada.

## <a name="log-in-to-azure"></a>Azure'i sisse logida

Avage **Microsoft Azure'i Käsuviip**ja logige sisse. 

    azure login

Kui tipite eelmises näites, on esitatud koodi. Liikuge brauseris login jätkata https://aka.ms/devicelogin.

![cmd näitab seadme sisselogimine][1]

Sisestage brauseri saadud kood. Suunatakse teid lehele sisselogimiseks.

![brauseri koodi sisestamiseks][2]

Kui kood on sisestatud olete sisse logitud, sulgege brauser jätkamiseks seda stsenaariumi.

![olete sisse logitud][3]

## <a name="switch-to-resource-manager-mode"></a>Ressursihaldur lülitumine

    azure config mode arm

## <a name="create-the-resource-group"></a>Ressursi rühma loomine

Enne rakenduste lüüsi loomist luuakse sisaldama rakenduse lüüsi ressursirühma. Järgmisel joonisel on kujutatud nupp.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Virtuaalse võrgu loomine

Kui ressursirühma on loodud, luuakse virtuaalse võrgu rakenduse lüüsi.  Järgmises näites on aadressiruumi oli 10.0.0.0/16 eelnev stsenaarium märkmete määratletud.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Mõne alamvõrgu loomine

Pärast virtuaalse võrgu on loodud, lisatakse mõne alamvõrgu rakenduse lüüsi.  Kui kavatsete kasutada rakenduse lüüsi web Appiga, majutatud rakenduse lüüsi virtuaalse samasse võrku, kindlasti jääks piisavalt ruumi teise alamvõrgu.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>Lüüsi rakenduse loomine

Kui virtuaalse võrgu ja alamvõrgu on loodud, rakenduse lüüsi eeltingimused on lõpule viidud. Samuti on vaja järgmist toimingut varem eksporditud pfx sert ja serdi parooli: kasutatakse funktsiooni taustväärtus IP-aadressid pole oma taustväärtus serveri IP-aadressid. Need väärtused võivad olla virtuaalse võrgu privaatne IP-d, avaliku IP-d või täielikult kvalifitseeritud domeeninimede taustväärtus servereid.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Loendi parameetrid, mida saab esitada loomisel, käivitage järgmine käsk: **azure võrgu rakenduste-portaali loomine – spikker**.

Selles näites loob lihtsa rakenduse lüüsi vaikesätete kuulajale, taustväärtus pool, taustväärtus http sätted ja reegleid. Konfigureerib SSL offload. Saate muuta neid sätteid vastavalt pärast selle ettevalmistamise on eduka juurutamise.
Kui olete juba oma veebirakenduse määratletud funktsiooni taustväärtus pargi eelmist toimingut, kui loodud, koormusetasandus algab.

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas luua kohandatud seisund sondid külastades, [luua kohandatud seisundi juures](application-gateway-create-probe-portal.md)

Siit saate teada, kuidas konfigureerida SSL-i mahalaadimine ja võtta kulukas SSL dekrüptimine välja veebi serveri külastades [Offload SSL-i konfigureerimine](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png