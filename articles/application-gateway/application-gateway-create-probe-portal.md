<properties
   pageTitle="Luua kohandatud juures jaoks rakenduste portaali kaudu portaali | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua kohandatud juures rakenduse lüüsi portaali abil"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Luua kohandatud juures rakenduse lüüsi portaali abil

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-probe-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-probe-ps.md)
- [Azure'i klassikaline PowerShelli](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Stsenaarium

Järgmistel läheb läbi kohandatud seisundi juures olemasoleva rakenduste portaali loomine.
Stsenaarium eeldab, et olete juba järgisite juhiseid [Rakenduste portaali](application-gateway-create-gateway-portal.md)loomine.

## <a name="createprobe"></a>Funktsiooni juures loomine

Sondid on konfigureeritud kaheosaline toiming portaali kaudu. Kõigepealt tuleb luua selle juures, edasi lisada selle juures rakenduse lüüsi sätete taustväärtus http.

### <a name="step-1"></a>Samm 1

Liikuge http://portal.azure.com ja valige olemasolev rakenduste portaali.

![Rakenduse lüüsi ülevaade][1]

### <a name="step-2"></a>Samm 2

Klõpsake nuppu **Lisa** uus juures lisamiseks klõpsake **sondid** .

![Täidetud teabega juures blade lisamine][2]

### <a name="step-3"></a>Samm 3

Täitke selle juures nõutavad andmed ja kui olete lõpetanud, klõpsake **OK**.

- **Nimi** – see on sõbralik nimi, mis juures, millele pääseb portaalis.
- **Host** – see on hosti nimi, mida kasutatakse selle juures. Kohaldatavad ainult siis, kui mitme saidi on konfigureeritud rakenduse lüüsi, muul juhul kasutatakse "127.0.0.1". See erineb VM hosti nimi.
- **Tee** - ülejäänud täielik URL-i kohandatud juures. Sobiv tee algab selgitava lausega "/"
- **Intervall (SEC)** – kui sageli käivitatakse selle juures seisundi kontrollimiseks. Ei ole soovitatav määrata on väiksem kui 30 sekundit.
- **Ajalõpp (SEC)** – selle juures, mille möödumisel ajastuse läbi aja. Ajalõpp intervall peab olema piisavalt suur, et http-kõne teha tagada kirjutamata seisundi lehel on saadaval.
- **Vigane lävi** - käsitletaks vigane nurjunud katsete arv. Lävi 0 tähendab, et kui seisundi kontrollimine nurjub funktsiooni tagaandmebaas määratakse vigane kohe.

> [AZURE.IMPORTANT] hosti nimi pole serveri nime. See on rakenduse serveris, virtuaalse serveri nimi. Funktsiooni juures saadetakse http://(host name):(port from httpsetting)/urlPath

![juures konfiguratsioonisätted][3]

## <a name="add-probe-to-the-gateway"></a>Lüüsi juures lisamine

Nüüd, kui selle juures on loodud, on aeg lüüsi lisamiseks. Juures sätted on seatud rakenduse lüüsi kirjutamata http sätted.

### <a name="step-1"></a>Samm 1

Klõpsake rakenduse lüüsi **HTTP sätted** ja klõpsake praeguse kirjutamata http sätete aknas avab konfiguratsiooni tera.

![HTTPS-i sätete aknas][4]

### <a name="step-2"></a>Samm 2

Enne **appGatewayBackEndHttp** sätted, klõpsake **kasutada kohandatud juures** ja valige juures, [Loo selle juures](#createprobe) tööetapis loodud.
Kui olete lõpetanud, klõpsake nuppu **OK** ja sätted on rakendatud.

![blade appgatewaybackend sätted][5]

Vaikimisi juures kontrollib veebirakenduse vaikimisi juurdepääs. Nüüd, kui kohandatud juures on loodud, rakenduse lüüs kasutab kohandatud tee määratletud, jälgida tervis taustväärtus, mis on valitud. Mis on määratletud kriteeriumide alusel, rakenduse lüüsi kontrollib juures määratud fail. Kui kõne pordiga host / tee ei tagasta vastuse olek Http 200 OK, server viiakse pöördeefekti pärast vigane lävi. Katsetamine jätkub vigane eksemplari kindlaks teha, kui see on terve uuesti. Kui eksemplari lisatakse tagasi terve serveri ühendada liikluse algab minevaid see uuesti ja katsetamine eksemplari jätkub kasutaja määratud intervalliga nagu tavaliselt.


## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, kuidas konfigureerida SSL-i mahalaadimine Azure'i rakenduse Gateway vt [Offload SSL-i konfigureerimine](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png