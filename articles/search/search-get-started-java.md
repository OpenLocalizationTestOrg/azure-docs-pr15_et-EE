<properties
    pageTitle="Azure'i otsingu Java alustamine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Kuidas koostada majutatud cloud otsingurakenduse Azure, kasutades oma programmeerimiskeel Java."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-java"></a>Azure'i otsingu Java kasutamise alustamine
> [AZURE.SELECTOR]
- [Portaal](search-get-started-portal.md)
- [.NET-I](search-howto-dotnet-sdk.md)

Siit saate teada, kuidas luua kohandatud Java otsingurakenduse, mis kasutab Azure otsimine oma otsingut. Selle õpetuse kasutab [Azure otsingu teenuse REST API](https://msdn.microsoft.com/library/dn798935.aspx) ehitada objektide ja selle kasutatud toimingud.

Selle valimi käivitamiseks peavad teil olema Azure'i otsinguteenuse, kus te saate registreeruda [Azure portaali](https://portal.azure.com). Üksikasjalikud juhised leiate teemast [loomine Azure otsinguteenuse portaali](search-create-service-portal.md) .

Kasutasime koostamine ja selle proovi järgmine tarkvara:

- [Eclipse IDE Java EE arendajatele](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Ärge unustage EE versiooni saate alla laadida. Mõnda järgmistest toimingutest kontrollimise jaoks on vaja funktsioon, mis on leitud ainult see väljaanne.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Andmete kohta

See näide rakendus kasutab [USA geoloogilise Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtreeritud Rhode Island oleku kohta andmehulga mahu vähendamiseks. Kasutame neid andmeid otsingurakenduse, mis tagastab natsionaalsotsialistlikule nagu haiglad ja koolid, samuti geoloogilise funktsioone, nagu voogu järvi ja tippkohtumise koostamiseks.

Selle rakenduse **SearchServlet.java** programmi koostab ja laadib registri abil [indekseerija](https://msdn.microsoft.com/library/azure/dn798918.aspx) loomiseks, mis toomine filtreeritud USGS andmekomplekti avaliku Azure SQL-i andmebaasist. Eelmääratletud identimisteabe ja online andmeallika ühenduseteave on toodud programmist koodi. Juurdepääs andmetele, ei ole veel konfiguratsioon on vajalikud.

> [AZURE.NOTE] Meil on rakendatud filter tuvastatavate alla tasuta hinnakirjad taseme piirmäära 10 000 dokumenti. Kui kasutate standard taseme, piirang ei kehti ja saate muuta selle koodi kasutada suurem andmekomplekti. Üksikasjalikku teavet iga hinnakirjad taseme võimsus kohta leiate teemast [piirangud ja piirangutega](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>Programmi failide kohta

Järgmises loendis kirjeldatakse selles valimi olevaid faile.

- Search.jsp: Pakub kasutajaliides
- SearchServlet.java: Pakub võimalust (sarnaselt on selle domeenikontrolleri MVC)
- SearchServiceClient.java: Pidemete HTTP taotlused
- SearchServiceHelper.java: Helper klassi, mis pakub staatilised meetodid
- Document.Java: Pakub andmemudel
- config.Properties: määrab Otsinguteenuse URL ja api võti
- POM.XML: Maven sõltuvus

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Teenuse nimi ja api-võti oma Azure'i otsinguteenuse otsimine

Kõik Azure'i Search REST API kõne jaoks on vaja teenuse URL-i ja mõne api-võti esitate. 

1. [Azure'i portaali](https://portal.azure.com)sisse logima.
2. Klõpsake hüpata **otsinguteenuse** loetleda Azure'i otsingu teenuste tellimuse eest ette valmistatud.
3. Valige teenus, mida soovite kasutada.
4. Teenuse armatuurlaual kuvatakse paanid olulist teavet ning juurdepääsuks haldus klahve võtme ikooni.

    ![][3]

5. Teenuse URL-i ja mõne administraator vajutamisega kopeerida. Te peate neid hiljem, kui lisate need **config.properties** faili.

## <a name="download-the-sample-files"></a>Näidisfailide allalaadimine

1. Minge [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) github.

2. Klõpsake nuppu **Laadi alla ZIP**, ZIP-faili salvestamiseks kettale ja seejärel ekstrakti kõik failid, mida see sisaldab. Kaaluge ekstraktimiseks Java tööruumi hõlbustamiseks leidmise hõlbustamiseks projekti faile.

3. Proovi failid on kirjutuskaitstud. Paremklõpsake kausta atribuudid ja tühjendage atribuut kirjutuskaitstud.

Kõigi järgnevate faili muudatused ja Käivita laused esitatakse selles kaustas olevaid faile.  

## <a name="import-project"></a>Projekti importimine

1. Eclipse, klõpsake menüü **fail** > **impordi** > **Üldine** > **Olemasoleva projektide tööruumi sisse**.

    ![][4]

2. **Valige juurkaust**, otsige üles kaust, mis sisaldab näidisfailide. Valige kaust, mis sisaldab .project kausta. Projekti **projektide** loend kuvatakse valitud üksusega.

    ![][12]

3. Klõpsake nuppu **valmis**.

4. **Project Exploreri** abil ning failide kuvamiseks ja redigeerimiseks. Kui see pole juba avatud, klõpsake **aknas** > **Vaate kuvamine** > **Project Explorer** või kasutage otseteed selle avamiseks.

## <a name="configure-the-service-url-and-api-key"></a>Teenuse URL-i ja api võti konfigureerimine

1. Valige **Project Exploreri**topeltklõpsake **config.properties** redigeerimiseks serveri nimi ja api võti sisaldavad sätete konfigureerimine.

2. Vaadake juhiseid selle artikli, kui olete leidnud teenuse URL-i ja api võti [Azure portaali](https://portal.azure.com)väärtused kuvatakse nüüd sisestate **config.properties**saada.

3. Asendage "Api võti" **config.properties**, api võti teie teenuse jaoks. Järgmiseks teenuse nimi (esimene osa URL-i http://servicename.search.windows.net) asendab "teenuse nimi" sama faili.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Projekti, koostamine ja käitusaja keskkondade konfigureerimine

1. Eclipse Project Explorer, paremklõpsake Projekt > **Atribuudid** > **Projekti aspekte**.

2. Valige **dünaamilise mooduli**, **Java**ja **JavaScripti**.

    ![][6]

3. Klõpsake nuppu **Rakenda**.

4. Valige **akna** > **eelistused** > **serveri** > **käitusaja keskkonnas** > **Lisa.**.

5. Laiendage Apache ja valige varem installitud Apache Tomcat serveri versioon. Meie süsteemi installitud 8 versioon.

    ![][7]

6. Järgmisel lehel Määrake Tomcat installikaust. Windowsiga arvutis, seda tõenäoliselt C:\Program Files\Apache tarkvara Foundation\Tomcat *versioon*.

6. Klõpsake nuppu **valmis**.

7. Valige **akna** > **eelistused** > **Java** > **installitud JREs** > **lisada**.

8. **JRE lisada**, valige **Standard VM**.

10. Klõpsake nuppu **edasi**.

11. Klõpsake JRE määratluse JRE Avaleht, **Directory**.

12. Liikuge **Program Files** > **Java** ja valige soovitud JDK varem installitud. On oluline, saate valida soovitud JDK selle JRE.

13. Installitud JREs, valige **JDK**. Teie sätted peaks välja nägema umbes järgmine pilt.

    ![][9]

14. Soovi korral valige **akna** > **Veebibrauseri** > **Internet Exploreri** rakenduse avamiseks välise brauseriaknas. Kasutades välises brauseris annab teile Web rakenduse mugavamaks.

    ![][8]

Teil on nüüd lõpule konfiguratsiooni tööülesanded. Järgmiseks tuleb koostada ja käivitada projekt.

## <a name="build-the-project"></a>Projekti koostamine

1. Project Explorer, paremklõpsake projekti nime ja valige **Käivita kasutajana** > **Maven koostada...** projekti konfigureerimiseks.

    ![][10]

8. Redigeeri konfiguratsiooni eesmärgid, tippige "puhta installi" ja seejärel klõpsake nuppu **Käivita**.

Olekuteade on väljundi konsooli aknasse. KOOSTADA edu ehitatud vigadeta projekt, mis näitab, peaksite nägema.

## <a name="run-the-app"></a>Käivitage rakendus

Selles etapis tuleb viimase käivitate rakenduse kohaliku serveri käitusaja keskkonnas.

Kui määrasite pole veel sisse Eclipse serveri käitusaja keskkonnas, peate kõigepealt teha.

1. Laiendage **WebContent**Project Explorer.

5. Paremklõpsake **Search.jsp** > **käivitada** > **Server töötab**. Valige Apache Tomcat server ja seejärel klõpsake nuppu **Käivita**.

> [AZURE.TIP] Kui kasutasite talletamiseks projekti-vaikimisi tööruumi, peate muutmiseks **Käivitage konfiguratsiooni** osutamiseks projekti asukoht vältimiseks käivitamise serveritõrge. Project Explorer, paremklõpsake **Search.jsp** > **Käivitada nimega** > **Konfiguratsioone käivitada**. Valige Apache Tomcat server. Klõpsake **argumendid**. Klõpsake **tööruumi** või **Failisüsteemi** kaust, mis sisaldab projekti seadmine.

Rakenduse käivitamisel peaksite nägema brauseriaknas kuvada, esitada otsinguvälja sisestamise tingimused.

Oodake umbes üks minut enne nupu **Otsing** teenuse aega loomine ja laadida registri. Kui teil tekib tõrge HTTP 404, vaid peate natuke enam ootama, enne kui proovite uuesti.

## <a name="search-on-usgs-data"></a>USGS andmete otsimine

USGS andmehulga sisaldab kirjeid, mis on olulised Rhode Island olekus. Kui klõpsate mõnda tühja otsinguväljale **Otsi** , saate ülemise 50 kirjed, mis on vaikimisi.

Sisestada otsingusõna annab otsingumootori midagi liikuda. Proovige piirkondliku nime sisestamine. "Roger Williams" oli esimene kuberner keeleline. Mitme parkides, hoonete ja koolides on tema nime.

![][11]

Võib proovida ka kõik need tingimused

- Pawtucket
- Pembroke
- hane + Neeme

## <a name="next-steps"></a>Järgmised sammud

See on esimene Azure'i otsingu õpetuse Java ja USGS andmekomplekti. Aja jooksul me saate laiendada selle õpetuse näitamaks, et võiksite kasutada oma kohandatud lahendused täiendavad otsinguvõimalused.

Kui teil on veel mõni taust Azure'i otsing, saate selle valimi hüppelauaks katsetamine, võib-olla täiendada [otsingu lehele](search-pagination-page-layout.md)või rakendamise [lihvitud navigeerimine](search-faceted-navigation.md). Otsingutulemite lehe saab täiustada ka lisades loendab ja partiide dokumentide nii, et kasutajad saavad Lehitsemiseks tulemused.

Uus Azure otsing? Soovitame proovimise muud õpetused lisaandmete saamiseks, mida saate luua. Külastage meie [dokumentatsiooni lehe](https://azure.microsoft.com/documentation/services/search/) leidmiseks veel ressursse. Linke saate vaadata ka [Video ja õpetus loendi](search-video-demo-tutorial-list.md) veel teabele juurde.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
