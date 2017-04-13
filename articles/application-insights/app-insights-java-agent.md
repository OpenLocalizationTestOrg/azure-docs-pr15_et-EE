<properties 
    pageTitle="Sõltuvused, erandid ja täitmise ajal Java veebirakenduste jälgimine" 
    description="Laiendatud Java veebisaidi Rakenduse ülevaated jälgimine" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Sõltuvused, erandid ja täitmise ajal Java veebirakenduste jälgimine

*Rakenduse ülevaated on eelvaade.*

Kui teil on [varustatud mõõteriistadega tegemiseks oma Java veebirakenduse rakenduse ülevaated][java], Java Agent abil saate saada täpsemat ülevaadet, muutmata kood:


* **Sõltuvused:** Andmete kõnedest, mis teeb rakenduse muud komponendid, sh:
 * **Ülejäänud kõned** HttpClient, OkHttp ja RestTemplate (kevad) kaudu.
 * **Redis** kõned Jedis kliendi kaudu. Kui kõne kestab kauem kui 10s, agent tõmbab ka kõne argumendid.
 * **[JDBC kõned](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** – MySQL-i, SQL Server, PostgreSQL-i, SQLite, Oracle'i Andmebaasipakkuja või Apache Derby DB. "executeBatch" kõned on toetatud. MySQL-i ja PostgreSQL-i, kui kõne kestab kauem kui 10s, agent aruanded päringu plaan. 
* **Püütud erandid:** Andmeid erandid, mida käsitletakse oma koodi.
* **Meetod täitmisaeg:** Aega kulub käivitada teatud andmeid.

Java agent kasutamiseks installimist serverisse. [Rakenduse ülevaateid Java SDK]peab teie veebirakenduste kinnitatakse[java].

## <a name="install-the-application-insights-agent-for-java"></a>Installige rakenduse ülevaated agent Java

1. Arvutis töötab teie Java server [agent alla laadida](https://aka.ms/aijavasdk).
2. Rakenduse server startup skripti redigeerida ja lisada järgmised töötab:

    `javaagent:`*agent JAR faili täielik tee*

    Näiteks klõpsake Tomcat Linux arvutisse:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Taaskäivitage rakendus serverisse.

## <a name="configure-the-agent"></a>Agent konfigureerimine

Looge fail nimega `AI-Agent.xml` ja paigutamine agent JAR faili samasse kausta.

Määrake XML-faili sisu. Järgmises näites kaasamiseks või argument funktsioonid soovite redigeerida. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Teil on meetod ajastuse üksikute meetodite ja aruannete erandi lubamiseks.

Vaikimisi `reportExecutionTime` on täidetud ja `reportCaughtExceptions` on väär.

## <a name="view-the-data"></a>Andmete kuvamine

Rakenduse ülevaated ressurss, kuvatakse liidetud remote sõltuvus ja meetod täitmise korda [jõudluse paani jaotises][metrics]. 

Üksikud eksemplarid, sõltuvus, erand ja viis aruannete otsimiseks avage [Otsingu][diagnostic]. 

[Diagnoosimine sõltuvus probleemid – lugege lisateavet](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Teil on küsimusi? Probleeme?

* Pole andmeid? [Automaatkorrektuuri erandite määramine tulemüüri](app-insights-ip-addresses.md)
* [Java tõrkeotsing](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
