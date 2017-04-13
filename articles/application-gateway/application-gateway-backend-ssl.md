<properties
   pageTitle="SSL-i poliitika ja klõpsake rakenduse lüüsi lõpuni SSL-i lubamine | Microsoft Azure'i"
   description="Sellel lehel antakse ülevaade rakenduse lüüsi lõpuni SSL-i tugi."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>SSL-i poliitika ja klõpsake rakenduse lüüsi lõpuni SSL-i lubamine

## <a name="overview"></a>Ülevaade

Rakenduse lüüsi toetab SSL-i lõpetamisel Gateway, pärast mis tavaliselt liigub sujuvalt krüptimata kirjutamata serverid. See võimaldab web serverid unburdened kulukas krüptimine/dekrüptimine pea kohal. Siiski mõne krüptimata side kirjutamata serverid ei ole lubatud võimalus. Põhjuseks võib olla Turve ja nõuetele või rakendus võib vastu võtta ainult turvalist ühendust. Selliste rakenduste rakenduse lüüsi toetab nüüd lõpuni SSL-i krüptimine.

End, et SSL-i lõpp võimaldab teil edastada turvaliselt tundliku loomuga andmeid on kirjutamata krüptitud ikka ära Layer 7 laadi tasakaalustamiseks funktsioonid eelised, mis rakenduse lüüsi toetab, küpsise kuuluvuse, nt URL-i-põhine marsruutimine, marsruutimine, mis põhineb saitide või võimalus sisestab X-edasisaadetud-* päised.

Lõpuni SSL-i side režiimiga konfigureerimisel rakenduse lüüsi lõpeb kasutaja SSL-i seansside lüüsi ja dekrüptib kasutaja liikluse. See kehtib siis konfigureeritud reeglid, valige sobiv kirjutamata rakenduskausta eksemplari-liikluse marsruutimiseks. Rakenduse lüüsi seejärel käivitab uue taustväärtus serveriga ühenduse SSL-i ja uuesti krüptib andmeid, kasutades taustväärtus serveri avaliku võtme serdi enne selle kirjutamata taotluse edastamise. Lõpeta lõpetamiseks SSL-i lubamiseks määramise protokolli BackendHTTPSetting https, mis rakendatakse siis taustväärtus pool. Iga taustväärtus serveri taustväärtus kogumi lõpuni SSL-iga lubatud peab olema konfigureeritud lubama turvaline side sertifikaadiga.

![SSL-i lõpuni stsenaarium][1]

Selles näites marsruuditakse kasutades TLS1.2 taustväärtus serverites Pool1 SSL-i lõpuni.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Lõpeta lõpetamiseks SSL-i ja valge nimekirja serdid.

Rakenduse lüüsi suhtleb ainult teadaolevad kirjutamata eksemplarid, mis on whitelisted nende rakenduste lüüsi serti. Valge nimekirja sertide lubamiseks peab üleslaadimiseks rakenduste portaali (mitte juurkausta sert) avalik võti taustväärtus serveri serdid. Ainult ühenduste teadaolevad ja whitelisted taustaprogrammid seejärel lubatud. Ülejäänud taustaprogrammid tulemused lüüsi tõrge. Iseallkirjastatud serdid on katsetamiseks ainult ja pole soovitatav tootmise töökoormus. Selliste sertifikaatide peab olema whitelisted rakenduse Gateway, nagu on kirjeldatud eelmist toimingut, enne kui saate neid kasutada.

## <a name="application-gateway-ssl-policy"></a>Rakenduse lüüsi SSL-i poliitika

Lüüsi rakendus toetab kasutaja konfigureeritav SSL-i läbirääkimiste reeglid, mis võimaldavad kliendid rohkem kontrolli SSL-ühendusi rakenduse Gateway.

1. SSL-i 2.0 ja 3.0 kõigi rakenduste lüüside vaikimisi keelatud. Need pole üldse konfigureeritav.
2. SSL-i poliitika määratlus annab teile suvandit, kui soovite kasutada mõnda järgmist 3 Protokollid - TLSv1\_0, TLSv1\_1, TLSv1\_2.
3. Kui SSL-i poliitika on määratletud kõigi kolme (TLSv1\_0, TLSv1\_1, TLSv1_2) on lubatud.

## <a name="next-steps"></a>Järgmised sammud

Pärast õppida end, et SSL-i ja SSL-i poliitika, avage [rakendus Gateway lõpuni SSL-i](application-gateway-end-to-end-ssl-powershell.md) lubamiseks luua rakenduste portaali võimalus saata liiklust taustaprogrammid krüptitud kujul.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png