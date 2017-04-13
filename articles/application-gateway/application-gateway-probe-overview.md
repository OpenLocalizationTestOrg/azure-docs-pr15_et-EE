

<properties
   pageTitle="Seisundi jälgimine ülevaade Azure'i rakenduse lüüsi | Microsoft Azure'i"
   description="Lisateavet selle jälgimise võimaluste Azure'i rakenduse lüüsi"
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

# <a name="application-gateway-health-monitoring-overview"></a>Rakenduse lüüsi seisundi jälgimine ülevaade

Azure'i rakenduse lüüsi vaikimisi jälgib kõik selle tagaandmebaas pool ressursside ja eemaldab automaatselt mis tahes ressursi lugeda vigane pool. Rakenduse lüüsi jätkub vigane eksemplari jälgimiseks ja lisab need tagasi terve tagaandmebaas pool, kui need on saadaval neid seisund sondid vastata. Rakenduse lüüsi saadab seisundi sondid sama Port, mis on määratletud tagaandmebaas HTTP sätted. See tagab, et selle juures on testimise sama port, mis klientide soovite kasutada ühenduse loomiseks soovitud taustväärtus.

![rakenduse lüüsi juures näide][1]

Lisaks saate kasutada vaikimisi seisundi jälgimine juures, kohandamiseks saate ka vastavalt oma rakenduse nõuded seisundi juures. Selles artiklis käsitletakse vaike- ja kohandatud seisund sondid.

## <a name="default-health-probe"></a>Vaikimisi seisundi juures

Rakenduste portaali konfigureerib automaatselt vaikimisi seisundi juures ei häälestamisel konfiguratsiooni kohandatud juures. Jälgimisega seotud käitumise töötab, muutes HTTP-päring tagaandmebaas rakenduskausta konfigureeritud IP-aadresse. Jaoks vaikimisi sondid kui kirjutamata http sätted on konfigureeritud lubama HTTPS-i funktsiooni juures kasutatakse https ka selle taustaprogrammid seisundi kontrollimiseks.

Näide: saate konfigureerida rakendus lüüsist tagaandmebaas serverid A, B ja C abil saate HTTP võrguliikluse pordi 80. Vaikimisi seisundi jälgimine kontrollib kolme serverid iga 30 sekundi järel terve HTTP vastus. Terve HTTP vastus on [olekukoodi](https://msdn.microsoft.com/library/aa287675.aspx) 200 kuni 399.

Vaikimisi juures sisse nurjumisel serveri A rakenduse lüüsi eemaldatakse see oma tagaandmebaas pool ja võrguliikluse voolamine selles serveris. Vaikimisi juures on endiselt otsida server iga 30 sekundit. Kui server A vastab edukalt ühe taotluse vaikimisi seisundi juures, lisatakse need tagasi nii terved tagaandmebaas rakenduskausta ja liikluse käivitab minevaid server uuesti.

### <a name="default-health-probe-settings"></a>Seisund juures vaikesätted

|Atribuudi juures | Väärtus | Kirjeldus|
|---|---|---|
| Juures URL-i| http://127.0.0.1:\<port\>/ | URL-tee |
| Intervall | 30 | Juures intervall sekundites |
| Ajalõpp  | 30 | Juures ajalõpp sekundites |
| Vigane lävi | 3 | Sondi uuesti loendus. Tagaandmebaas server on märgitud pärast järjestikust juures tõrge count jõuab vigane lävi alla. |

> [AZURE.NOTE] Pordi alati on sama port nimega tagaandmebaas HTTP sätted.

Vaikimisi juures välja ainult http://127.0.0.1:\<port\> määratlemiseks seisund olek. Kui teil on vaja konfigureerida seisundi juures minge kohandatud URL-i või muid sätteid muuta, peate kasutama kohandatud sondid, nagu on kirjeldatud järgmistes juhistes.

## <a name="custom-health-probe"></a>Kohandatud seisundi juures

Kohandatud sondid võimaldavad täpsema juhtida seisundi jälgimine. Kui kasutate kohandatud sondid, saate konfigureerida juures intervall, URL-i ja tee testimiseks ja mitu nurjunud vastuste kinnitamiseks enne märkimise tagaandmebaas rakenduskausta eksemplari nimega vigane.

### <a name="custom-health-probe-settings"></a>Kohandatud seisundi juures sätted

Järgmine tabel pakub kohandatud seisundi juures atribuudid.

|Atribuudi juures| Kirjeldus|
|---|---|
| Nimi | Nimi on juures. Selle nime on kasutatud viidata juures tagaandmebaas HTTP sätted. |
| Protocol (protokoll) | Protokoll, mida kasutatakse saata selle juures. Funktsiooni juures kasutatakse määratletud tagaandmebaas HTTP sätted protokoll |
| Host |  Hosti nimi saata selle juures. Kohaldatavad ainult siis, kui mitme saidi on konfigureeritud rakenduse lüüsi, muul juhul kasutatakse "127.0.0.1". See erineb VM hosti nimi. |
| Tee | Suhteline tee on juures. Sobiv tee algab /". |
| Intervall | Sondi intervall sekundites. See on kaks järjestikust sondid intervall.|
| Ajalõpp | Sondi ajalõpp sekundites. Funktsiooni juures on märgitud nurjus kui kehtiv vastuse ei selle ajavahemiku jooksul. |
| Vigane lävi | Sondi uuesti loendus. Tagaandmebaas server on märgitud pärast järjestikust juures tõrge count jõuab vigane lävi alla. |

> [AZURE.IMPORTANT] Kui saidil on konfigureeritud rakenduse lüüsi, vaikimisi selle hosti nimi peaks olema määratud nimega "127.0.0.1", kui muul viisil konfigureerida kohandatud juures.
Viide on kohandatud juures saadetud \<protokolli\>://\<hosti\>:\<port\>\<tee\>. Kasutatav port on sama port määratletud tagaandmebaas HTTP sätted.

## <a name="next-steps"></a>Järgmised sammud

Pärast õppida rakenduse lüüsi seisundi jälgimine, saate konfigureerida [kohandatud seisundi juures](application-gateway-create-probe-portal.md) Azure portaali või [kohandatud seisundi juures](application-gateway-create-probe-ps.md) PowerShelli ja Azure ressursihaldur juurutamise mudeli abil.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png