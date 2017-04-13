<properties
   pageTitle="Rakenduste portaali jaoks SSL-i offload konfigureerimine portaali abil | Microsoft Azure'i"
   description="Sellelt lehelt leiate juhiseid, et luua rakenduste portaali SSL offload portaalis"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Portaali kaudu rakenduste portaali jaoks SSL-i offload konfigureerimine

> [AZURE.SELECTOR]
-[Azure'i portaalis](application-gateway-ssl-portal.md)
-[Azure ressursihaldur PowerShelli](application-gateway-ssl-arm.md)
-[Azure klassikaline PowerShelli](application-gateway-ssl.md)

Azure'i rakenduse lüüsi saab konfigureerida Gateway vältimiseks kulukas SSL dekrüptimine tööülesandeid juhtuda web serveripargi turvasoklite kiht (SSL) seansi lõpetamiseks. SSL-i offload lihtsustab ka ees serveri häälestamine ja haldamine veebirakenduse.

## <a name="scenario"></a>Stsenaarium

Järgmistel läbib konfigureerida SSL offload olemasoleva rakenduste portaali sisse. Stsenaarium eeldab, et olete juba järgisite juhiseid [Rakenduste portaali](application-gateway-create-gateway-portal.md)loomine.

## <a name="before-you-begin"></a>Enne alustamist

Konfigureerimiseks SSL offload rakenduste portaali, sert on nõutav. Selle serdi on laaditud rakenduste lüüsi ja kasutada krüptimiseks ja liiklus SSL-i kaudu saadetakse dekrüptida. Serdi peab olema isiklik teabevahetus (pfx) vormingus. See failivorming võimaldab mis peab taotluse lüüsi krüptimine ja dekrüptimine liikluse privaatvõti eksporditakse.

## <a name="add-an-https-listener"></a>Lisage mõni HTTPS kuulajale

HTTPS kuulajale otsib liikluse oma konfiguratsiooni alusel ja aitab marsruutida liiklust kirjutamata kaustu.

### <a name="step-1"></a>Samm 1

Liikuge Azure'i portaal ja valige olemasolev rakenduste portaali

### <a name="step-2"></a>Samm 2

Klõpsake kuulajatele on kuulajale lisamiseks nuppu Lisa.

![rakenduse lüüsi ülevaade blade][1]

### <a name="step-3"></a>Samm 3

Sisestage vajalikud andmed üles ja kuulajale pfx sert, kui olete lõpetanud, klõpsake nuppu OK.

**Nimi** – see väärtus on kuulajale sõbralik nimi.

**Konfiguratsiooni Frontend IP** - selle väärtuseks on frontend IP-konfiguratsiooni, mida kasutatakse kuulajale.

**Frontend port (nime/Port)** - kasutada rakenduse lüüsi ja tegeliku port, mida kasutatakse esiosa pordi sõbralik nimi.

**Protokoll** - lülitit määratlemiseks, kui kasutatakse https või http esiosa.

**Sert (nime ja parooli)** – kui SSL offload kasutatakse pfx sert on vaja seda sätet ja sõbralik nimi ja parool on nõutav.

![kuulajale blade lisamine][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Reegli loomine ja seostada kuulajale

Nüüd on loodud kuulajale. On aeg kuulajale liiklus käsitlema reegli loomiseks.

### <a name="step-1"></a>Samm 1

Klõpsake rakenduse lüüsi **reeglid** ja seejärel klõpsake nuppu Lisa.

![rakenduse lüüsi reeglid blade][3]

### <a name="step-2"></a>Samm 2

Enne **Lisa tavaline reegel** , tippige reegli sõbralik nimi ja valige eelmises etapis loodud kuulajale. Valige sobiv kirjutamata rakenduskausta ja http-säte ja klõpsake nuppu **OK**

![HTTPS-i sätete aknas][4]

Sätted on nüüd salvestatud rakenduse lüüsiga. Salvestamine protsessi jaoks neid sätteid võib võtta aega enne, kui need on saadaval portaali kaudu või PowerShelli kaudu vaadata. Kui salvestada rakenduse lüüsi tegeleb krüptimise ning dekrüptimine liiklust. Kõigi rakenduste portaali ja kirjutamata web serverite vahelist liikluse käsitletakse http kaudu. Mis tahes suhtlus kui algatatud https tagastatakse krüptitud kliendile.

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas konfigureerida kohandatud seisundi juures Azure'i rakenduse lüüsi, lugege teemat [Loo kohandatud seisundi juures](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png