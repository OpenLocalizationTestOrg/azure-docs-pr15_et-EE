<properties
    pageTitle="Vahemälu ASP.net-i seansi oleku pakkuja | Microsoft Azure'i"
    description="Siit saate teada, kuidas säilitada ASP.net-i seansi olek Azure'i Redis vahemälu abil"
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
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>ASP.net-i seansi oleku pakkuja Azure Redis vahemälu

Azure'i Redis vahemälu pakub seansi oleku pakkuja, mille abil saate salvestada oma seansi olek vahemälu asemel mälu- või SQL serveri andmebaasi. Vahemällu seansi oleku pakkuja kasutamiseks esmalt konfigureerida vahemälu ja konfigureerimist ASP.net-i rakenduse jaoks vahemälu Redis vahemälu seansi olek Nugeti pakett.

Pole sageli mõttekas reaalseid pilve rakenduses vältimiseks talletamise olekus mingi kasutaja seansi jaoks, kuid mõned võimalustest mõjutada jõudlus ja skaleeritavus rohkem kui teised. Kui teil on laoseisu, on parim lahendus hoida olekus väikest ja see küpsiseid salvestada. Kui see ei ole võimalik järgmise parim lahendus on kasutada ASP.net-i seansi olek pakkujaga jaotatud, -mälu vahemälu. Jõudlus ja skaleeritavus vaatevinklist kehvem lahendus on kasutada andmebaasi varundada seansi oleku pakkuja. Selles teemas antakse juhiseid ASP.net-i seansi oleku pakkuja Azure'i Redis vahemälu kasutamise kohta. Muude seansi olek suvandite kohta leiate teavet teemast [ASP.net-i seansi olek suvandid](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>ASP.net-i seansi laoseisu vahemälus

Visual Studio abil Redis vahemälu seansi olek Nugeti pakett konfigureerimiseks kliendi rakendus, paremklõpsake **Solution** Exploreris projekti ja valige **Haldamine NuGet-paketid**.

![Azure'i Redis vahemälu haldamine NuGet-paketid](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

Tippige väljale Otsing tekst **RedisSessionStateProvider** , valida tulemuste ja klõpsake nuppu **Installi**.

>[AZURE.IMPORTANT] Kui kasutate funktsiooni klastrite premium taseme kaudu, tuleb kasutada [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 või suurema või erandi Expression.error. See on server muutmine; Lisateabe saamiseks vt [v2.0.0 katkestamine üksikasjade muutmine](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

![Azure'i Redis vahemälu seansi oleku pakkuja](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

Redis seansi oleku pakkuja Nugeti pakett on sõltuvus StackExchange.Redis.StrongName paketti. Kui StackExchange.Redis.StrongName paketi pole projektis olema installitud. Pange tähele, et lisaks tugev nimega StackExchange.Redis.StrongName paketi ka StackExchange.Redis mitte-tugev-nimega versioon. Kui projekti kasutab mitte-tugev-nimega StackExchange.Redis versiooni desinstallida, kas enne või pärast installimist Redis seansi oleku pakkuja Nugeti pakett, muidu teil on saada nime panemise konfliktide projektis. Need paketid kohta leiate lisateavet teemast [konfigureerimine .net-i vahemälu kliendid](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Nugeti pakett allalaaditavate failide ja lisab nõutud viitab ja lisab lisab järgmisest jaotisest järgmist teie fail sisaldab ASP.net-i rakenduse kasutamiseks Redis vahemälu seansi oleku pakkuja nõutav konfiguratsioon.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

Kommenteeritud jaotises antakse näide atribuudid ja iga atribuudi jaoks.

Microsoft Azure'i portaalis teie vahemälu tera olevad väärtused ja atribuutide konfigureerimine ja konfigureerige muud väärtused vastavalt soovile. Teie vahemälu atribuutide juhised leiate teemast [vahemälusätete konfigureerimine Redis](cache-configure.md#configure-redis-cache-settings).

-   **host** – saate määrata oma vahemälu lõpp-punkti.
-   **port** – kasutage oma pordi SSL- või oma SSL port sõltuvalt SSL-i sätted.
-   **accessKey** – põhi- või klahvi kasutamiseks vahemälu.
-   **ssl** -true, kui soovite turvalist ssl; vahemälu/kliendi suhtlemine muidu väärtus false. Kindlasti õige port määrata.
    -   Pordi SSL on vaikimisi uue vahemälu jaoks keelatud. Määrake selle sätte Kasuta SSL-i porti true. Pordi SSL lubamise kohta leiate lisateavet jaotisest [Juurdepääs pordid](cache-configure.md#access-ports) [konfigureerimine vahemälu](cache-configure.md) teema.
-   **throwOnError** – väärtuse true, kui soovite erandi visati tõrge või false, kui soovite selle toimingu nurjumise vaikselt. Saate otsida tõrke, märkides ruudu staatilise Microsoft.Web.Redis.RedisSessionStateProvider.LastException atribuuti. Vaikimisi on täidetud.
-   **retryTimeoutInMilliseconds** – toimingud, mida on selles ajavahemikus määratud millisekundites uuesti proovida. Esimese proovi uuesti ilmneb pärast 20 millisekundit, aga siis korduskatsed iga teine kuni retryTimeoutInMilliseconds intervalli aegub. Kohe pärast selle intervall toimingu uuesti proovida lõplik üks kord. Kui toiming nurjub, välja arvatud Expression.Error tagasi helistaja, olenevalt throwOnError. Vaikeväärtus on 0, mis tähendab, et pole korduskatsed.
-   **databaseId** – määrab, millist andmebaasi kasutamiseks vahemällu väljund andmed. Kui pole määratud, kasutatakse vaikeväärtust, milleks on 0.
-   **applicationName** – kiirklahvid on talletatud redis nimega `{<Application Name>_<Session ID>}_Data`. See võimaldab mitme rakendusi sama klahvi ühiskasutusse anda. See parameeter on valikuline ja kui see ei paku vaikeväärtuse kasutatakse.
-   **connectionTimeoutInMilliseconds** – see säte võimaldab connectTimeout sätte StackExchange.Redis klientrakenduses alistada. Kui pole määratud, kasutatakse 5000 connectTimeout vaikesäte. Lisateabe saamiseks vt [StackExchange.Redis konfiguratsiooni mudel](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – see säte võimaldab syncTimeout sätte StackExchange.Redis klientrakenduses alistada. Kui pole määratud, kasutatakse 1000 syncTimeout vaikesäte. Lisateabe saamiseks vt [StackExchange.Redis konfiguratsiooni mudel](http://go.microsoft.com/fwlink/?LinkId=398705).

Nende atribuutide kohta leiate lisateavet teemast algse ajaveebi postituse teate [Redis teatavaks ASP.net-i seansi olek pakkuja](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)juures.

Ärge unustage välja tavalise InProc seansi oleku pakkuja jaotisele oma web.config kommentaar.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Kui tehakse neid juhiseid, rakenduse on konfigureeritud kasutama Redis vahemälu seansi oleku pakkuja. Seansi olek kasutamisel oma rakenduse, salvestatakse Azure Redis vahemälu eksemplar.

>[AZURE.NOTE] Pange tähele, et vahemälus talletatud andmed peavad olema sarjadesse jaotatav, erinevalt andmed, mida saab salvestada vaike-mälu ASP.net-i seansi maakond pakkuja. Seansi oleku pakkuja jaoks Redis kasutamisel olla kindel, et salvestatud seansi olek andmetüübid on sarjadesse jaotatav.

## <a name="aspnet-session-state-options"></a>ASP.net-i seansi olek suvandid

- Mälu seansi oleku pakkuja – see pakkuja talletab mällu seansi olek. See pakkuja kasutamise eelised on kiire ja lihtne. Kui te kasutate mälu pakkuja, kuna see on jaotatud, ei saa siiski skaala oma veebirakenduste.

- SQL serveri seansi olek pakkuja – see pakkuja talletatakse Sql serveri seansi olek. See pakkuja tuleks kasutada siis, kui soovite seansi oleku püsivate laos säilitamise. Muudate oma veebirakenduse, kuid seansi Sql serveri kasutamine on jõudluse mõju teie Web Appis.

- Jaotatud mälu seansi olek teenuse pakkuja nagu Redis vahemälu seansi oleku pakkuja – see pakkuja annab teile kõige paremini mõlematpidi. Oma veebirakenduse võib olla lihtne, Kiire ja scalable seansi oleku pakkuja. Kuid peate võtke arvesse, et see pakkuja talletab seansi olek vahemälu ja rakenduse on kõik omadused seotud kui rääkida on jaotatud rakenduses vahemälu nt siirdamiseks võrgu tõrkeid arvesse võtta. Head tavad vahemälu kasutamise kohta, vt [vahemälu juhiseid](../best-practices-caching.md) Microsoft Patterns ja headest tavadest [ja Azure pilveteenuse rakenduse kujundus](https://github.com/mspnp/azure-guidance).

Seansi olek ja muude heade tavade kohta leiate lisateavet teemast [Web arengu head tavad (Building tegelike pilve rakendused Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Järgmised sammud

Tutvuge [ASP.net-i väljundi vahemälu pakkuja Azure'i Redis vahemälu](cache-aspnet-output-cache-provider.md).
