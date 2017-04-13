<properties
    pageTitle="Azure'i API haldamise pidurdamise täpsemalt taotlus"
    description="Siit saate teada, kuidas luua ja rakendada paindlik kvoodi ja piirata Azure'i API haldamise poliitikate määr."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Azure'i API haldamise pidurdamise täpsemalt taotlus

On võimalik throttle sissetulevad taotlused on Azure API Management peamine ülesanne. Kas taotlused või kokku taotlusi/andmete kontrollides API haldus võimaldab kaitsta oma API-de kuritarvitamine ja luua erinevatele API toote tasemetele väärtus API pakkujad.

## <a name="product-based-throttling"></a>Toodet pidurdamise
Praeguseks määr ahendamine võimaluste on piiratud on rakendatud kindla toote tellimuse (põhiosas key), API haldusportaal Publisheri määratletud. See on kasulik API pakkuja rakendamiseks klõpsake arendajad, kes on registreerunud kasutada oma API piirangud, aga seda ei aita, näiteks pidurdamise üksikute lõppkasutajad API. On võimalik, et ühe kasutaja arendaja taotluse peab olema kogu kvoodi ja seejärel takistada teistel arendaja rakenduse kasutamiseks. Samuti mitu klientidele, kes võib kaasa tuua suure hulga taotlusi piirata aeg-ajalt kasutajad.

## <a name="custom-key-based-throttling"></a>Kohandatud vastavalt ahendamine
Uus [määr-limiit-järgi-võti](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ja [kvoodi-võti](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) poliitikate lahenduseks oluliselt paindlikumad juhtelement liiklust. Uue poliitika abil saate määratleda avaldiste tuvastamiseks jälgida liikluse kasutamine kasutatud võtmed. Kuidas see toimib on lihtsam illustreeritud näide. 

## <a name="ip-address-throttling"></a>IP-aadress ahendamine
Järgmised poliitikad piirata ühe kliendi IP-aadress ainult 10 kõned iga minut kokku 1 000 000 kõnede ja läbilaskevõime kuus 10 000 KB. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Kui kõik kliendid Internetis kordumatu IP-aadress, võib see olla tõhusalt piirata kasutus kasutaja. Siiski, on tõenäoline, et mitme kasutaja on ühiskasutuse avaliku IP-aadressi neid Interneti kaudu NAT tõttu. Hoolimata sellest, luba autentimata juurdepääs API-de jaoks soovitud `IpAddress` võib olla parim valik.

## <a name="user-identity-throttling"></a>Kasutaja identiteedi ahendamine
Kui kasutajal on kinnitatud, siis ahendamise klahvi saab luua teave, mis tuvastab kordumatult, mis põhineb kasutaja.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

Selles näites me ekstraktida autoriseerimine päis, teisendage see `JWT` objekti ja luba teema abil saate tuvastada kasutaja ja kasutada seda väärtusena, piiravad võti. Kui kasutaja identiteet on talletatud soovitud `JWT` nagu üks teise seejärel väidab, et selle asemel võib kasutada väärtus.

## <a name="combined-policies"></a>Ühendatud poliitika
Kuigi ahendamise poliitika pakkuda rohkem võimalusi, kui olemasolev ahendamise poliitikad, on endiselt väärtuse, kombineerides nii võimalusi. Pidurdamise tellimuse tootevõtme ([piiratud kõne määra tellimus](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ja [määrata kvoodi märkimise teel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)), on suurepärane viis monetizing, API laadimine lähtudes kasutuse lubamine. Tabeliruudustikku tekstuuriga juhtelement saab kasutaja throttle täiendab ja takistab vähenemist kogemus teise ühe kasutaja käitumist. 

## <a name="client-driven-throttling"></a>Kliendi juhitud ahendamine
Kui ahendamise võti on määratletud on [poliitika avaldise](https://msdn.microsoft.com/library/azure/dn910913.aspx)abil, siis on API pakkuja, mis on valida, kuidas ahendamine on rakendatud. Siiski arendaja võib-olla soovite määrata, kuidas nad määr limiit oma klientidele. See võib lubada API pakkuja tutvustus kohandatud päise lubama arendaja klientrakendusega suhelda API võti.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

See võimaldab valida, kuidas nad soovivad luua määra klahv piirata arendaja klientrakendusega. Kliendi arendaja võib natuke leidlikkus loomiseks oma määr astme eraldamise klahvid komplekti kasutajatele ja pöörata võtme kasutamine.

## <a name="summary"></a>Kokkuvõte
Azure'i API haldus pakub määr ja hinnapakkumise pidurdamise nii kaitse ja lisada väärtus API teenust. Uue ahendamise poliitikate koos kohandatud ulatuse reeglid võimaldavad tabeliruudustikku tekstuuriga juhtida need poliitikad, klientide luua veel paremaks rakendusi. Selles artiklis näidetes kirjeldatakse uue poliitika kasutamine võrra tootmisprotsessi, piiravad klahvid kliendi IP-aadressid, kasutaja identiteet ja väärtused klient, mis on loodud. Siiski on paljudes teistes teates, et saaks kasutada näiteks kasutajaagendi, URL-i tee fragmendid, sõnumi suurus.

## <a name="next-steps"></a>Järgmised sammud
Palun andke tagasisidet Disqus teemas selle teema. Oleks hea kuulda muud võimalikud väärtused mis on teie stsenaariumide valikuna.

## <a name="watch-a-video-overview-of-these-policies"></a>Vaadake videot ülevaade poliitika
Artikkel [määr-limiit-järgi-võti](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ja [kvoodi-võti](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) poliitika kohta lisateabe saamiseks vaadake järgmist videot.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
