<properties
    pageTitle="Vahemälu ASP.net-i väljundi vahemälu pakkuja"
    description="Siit saate teada, kuidas ASP.net-i abil Azure'i Redis vahemälu Leheväljundi vahemälu"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>ASP.net-i väljundi vahemälu pakkuja Azure Redis vahemälu

Redis väljundi vahemälu pakkuja on väljundi vahemälu andmete out protsess salvestusruumi. Andmed on mõeldud täielik HTTP vastuste (lehel väljundi vahemällu talletamise). Pakkuja ühendatakse uue väljundi vahemälu pakkuja laiendatavuse punkti mis võeti kasutusele ASP.net-i 4.

Redis väljundi vahemälu pakkuja kasutamiseks esmalt konfigureerida vahemälu ja seejärel konfigureerimine ASP.net-i rakenduse abil Redis väljundi vahemälu pakkuja Nugeti pakett. Selles teemas antakse juhiseid rakenduse kasutamiseks Redis väljund vahemälu pakkuja konfigureerimise kohta. Loomise ja Azure Redis vahemälu eksemplari konfigureerimise kohta leiate lisateavet teemast [loomine vahemälu](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.net-i Leheväljundi vahemälu talletamine

Visual Studio abil Redis väljundi vahemälu pakkuja Nugeti pakett konfigureerimiseks kliendi rakendus, paremklõpsake **Solution** Exploreris projekti ja valige **Haldamine NuGet-paketid**.

![Azure'i Redis vahemälu haldamine NuGet-paketid](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

Tippige väljale Otsing tekst **RedisOutputCacheProvider** , valida tulemuste ja klõpsake nuppu **Installi**.

![Azure'i Redis vahemälu väljundi vahemälu pakkuja](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Redis väljundi vahemälu pakkuja Nugeti pakett on sõltuvus StackExchange.Redis.StrongName paketti. Kui StackExchange.Redis.StrongName paketi pole projektis olema installitud. Pange tähele, et lisaks keeruka nimega StackExchange.Redis.StrongName paketi ka StackExchange.Redis mitte-tugev-nimega versioon. Kui projekti kasutab mitte-tugev-nimega StackExchange.Redis versiooni desinstallida, kas enne või pärast installimist Redis väljundi vahemälu pakkuja Nugeti pakett, muidu teil on saada nime panemise konfliktide projektis. Need paketid kohta leiate lisateavet teemast [konfigureerimine .net-i vahemälu kliendid](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Nugeti pakett allalaaditavate failide ja lisab nõutud komplekti viited ja lisab teie fail sisaldab ASP.net-i rakenduse kasutamiseks Redis väljundi vahemälu pakkuja nõutav konfiguratsioon üheks järgmisest jaotisest.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

Kommenteeritud jaotises antakse näide atribuudid ja iga atribuudi jaoks.

Teie vahemälu tera Microsoft Azure'i portaalis olevad väärtused ja atribuutide konfigureerimine ja konfigureerige muud väärtused vastavalt soovile. Teie vahemälu atribuutide juhised leiate teemast [vahemälusätete konfigureerimine Redis](cache-configure.md#configure-redis-cache-settings).

-   **host** – saate määrata oma vahemälu lõpp-punkti.
-   **port** – kasutage oma pordi SSL- või oma SSL port sõltuvalt SSL-i sätted.
-   **accessKey** – põhi- või klahvi kasutamiseks vahemälu.
-   **ssl** -true, kui soovite turvalist ssl; vahemälu/kliendi suhtlemine muidu väärtus false. Kindlasti õige port määrata.
    -   Pordi SSL on vaikimisi uue vahemälu jaoks keelatud. Määrake selle sätte Kasuta SSL-i porti true. Pordi SSL lubamise kohta leiate lisateavet jaotisest [Juurdepääs pordid](cache-configure.md#access-ports) [konfigureerimine vahemälu](cache-configure.md) teema.
-   **databaseId** – määratud millist andmebaasi andmete vahemälu väljundi jaoks kasutada. Kui pole määratud, kasutatakse vaikeväärtust, milleks on 0.
-   **applicationName** – kiirklahvid on talletatud redis nimega <AppName>_<SessionId>_Data. See võimaldab mitme rakendusi sama klahvi ühiskasutusse anda. See parameeter on valikuline ja kui see ei paku vaikeväärtuse kasutatakse.
-   **connectionTimeoutInMilliseconds** – see säte võimaldab connectTimeout sätte StackExchange.Redis klientrakenduses alistada. Kui pole määratud, kasutatakse 5000 connectTimeout vaikesäte. Lisateabe saamiseks vt [StackExchange.Redis konfiguratsiooni mudel](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – see säte võimaldab syncTimeout sätte StackExchange.Redis klientrakenduses alistada. Kui pole määratud, kasutatakse 1000 syncTimeout vaikesäte. Lisateabe saamiseks vt [StackExchange.Redis konfiguratsiooni mudel](http://go.microsoft.com/fwlink/?LinkId=398705).

Lisada mõne OutputCache direktiiv igale lehele, mille jaoks soovite väljundi vahemällu.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

Selles näites jäävad vahemälu andmete vahemällu talletatud lehe 60 sekundi järel ja muu versioon, klõpsake lehe vahemällu iga parameetri kombinatsiooni. OutputCache direktiivi kohta leiate lisateavet teemast [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Kui tehakse neid juhiseid, rakenduse on konfigureeritud kasutama Redis väljundi vahemälu pakkuja.

## <a name="next-steps"></a>Järgmised sammud

Leiate [Azure'i Redis vahemälu ASP.net-i seansi olek pakkuja](cache-aspnet-session-state-provider.md).
