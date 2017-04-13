<properties
    pageTitle="Luua Java veebirakenduse teenuses Azure rakendus | Microsoft Azure'i"
    description="Selle õpetuse näidatakse, kuidas juurutada Java web appi Azure'i rakendust Service."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Java web appi teenuses Azure rakenduse loomine

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Selle õpetuse näitab, kuidas luua Java [veebirakenduse Azure'i rakenduse teenuses] [Azure portaali]kaudu. Azure'i portaal on web liides, mille abil saate hallata oma Azure ressursse.

> [AZURE.NOTE] Selle õpetuse tegemiseks on vaja Microsoft Azure'i kontosse. Kui teil pole kontot, saate [aktiveerida oma Visual Studio abonendi kasu] või [tasuta prooviversiooni kasutajaks].
>
> Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus]. Seal saate kohe luua lühiajaline starter web appi rakenduse teenuses; krediitkaarti on nõutav, ja kohustusi.

## <a name="java-application-options"></a>Java rakenduse suvandid

On mitu võimalust, saate häälestada Java rakenduse App teenuse web Appis. 

1. Rakenduse loomine ja seejärel **rakenduse sätete**konfigureerimine.

    Rakenduse teenus pakub mitmeid Tomcat ja Jetty versioone, vaikimisi konfiguratsiooni. Kui rakendus, mis on hosting töötab koos sisseehitatud versioon, luua web container seda meetodit on kõige lihtsam ja sobib, kui kõik soovitud toiming on sõda faili üleslaadimine veebi ümbrises. Selle meetodi Azure'i portaalis rakenduse loomine ja seejärel avage **rakenduse sätted** tera oma rakenduse versiooni Java koos soovitud Java web container valimiseks. Kui seda meetodit kasutada Java nii teie web container on käivitada Programmifailid. Muid viise panna veebi-ümbrisest ja potentsiaalselt soovitud töötab teie kettaruumi. Kui kasutate seda mudelit, pole teil juurdepääs failisüsteemi osa failide redigeerimiseks. See tähendab, et te ei saa teha, näiteks konfigureerida *server.xml* faili või kausta *lib* teegi failid paigutada. Lisateabe saamiseks vt selle [loomine ja konfigureerimine Java web appi](#appsettings) jaotist selles õpetuses.
    
2. Malli kasutamine Azure'i turuplatsilt.

    Azure'i turuplatsi sisaldab malle, mida automaatselt loomine ja konfigureerimine Java veebirakenduste Tomcat või Jetty web ümbriste. Web ümbriste loodud mallid on konfigureeritav. Lisateabe saamiseks selle õpetuse jaotisest [kasutamine Java malli Azure'i turuplatsilt](#marketplace) .
  
3. Rakenduse loomine ja seejärel kopeerige käsitsi konfigureerimine failide redigeerimiseks 

    Võite majutada kohandatud Java rakendus, mis pole mis tahes web ümbriste, rakenduse teenuse juurutamine. Näiteks:
    
    * Java rakenduse jaoks on vaja versiooni Tomcat või Jetty, mis pole otse rakenduse teenus ei toeta või esitatud galeriis.
    * Java rakenduse võtab HTTP päringuid ja juurutamine nimega vähe olemasoleva web container sisse.
    * Soovite konfigureerida web container algusest peale ise. 
    * Mida soovite kasutada versiooni Java, mida ei toetata rakenduse teenuses ja soovite selle ise üles.

    Juhtudel, nagu need, saate luua rakenduse Azure'i portaalis ja sisestage asjakohane runtime failide käsitsi. Sel juhul loendatakse failid teie oma rakenduse teenusleping ruumi salvestusmahu suhtes. Lisateabe saamiseks lugege teemat [kohandatud Java veebirakenduse Azure'i üleslaadimine].

## <a name="portal"></a>Loomine ja konfigureerimine Java web app

Selles jaotises kirjeldatakse, kuidas luua veebirakenduse ja konfigureerimine Java **rakenduse sätted** tera portaali kaudu.

1. [Azure'i portaali]sisse logima.

2. Klõpsake **uus > Web + Mobile > Web Appi**.

    ![Uue veebirakenduse][newwebapp]

4. Sisestage väljale **veebirakenduse** veebirakenduse nimi.

    See nimi peab olema kordumatu azurewebsites.net domeen, sest web appi URL on {nimi}. azurewebsites.net. Kui teie sisestatud nimi pole kordumatud, kuvatakse punane hüüumärk tekstiväljale.

5. Valige **Ressursirühm** või looge uus.

    Ressursi rühmade kohta leiate lisateavet teemast [Azure portaalis hallata oma Azure ressursse].

6. Valige soovitud **Rakenduse leping/asukoht** või looge uus.

    Rakenduse teenuse lepingute kohta leiate lisateavet teemast [Azure rakenduse teenuse lepingute ülevaade].

7. Klõpsake nuppu **Loo**.

    ![Veebirakenduse loomine][newwebapp2]
 
8. Veebirakenduse loomisel klõpsake **veebirakenduste > {oma veebirakenduse}**.
 
    ![Valige Web App][selectwebapp]

9. **Veebirakenduse** tera, klõpsake nuppu **sätted**.

10. Klõpsake **rakenduse sätted**.

11. Valige soovitud **Java versiooni**. 

12. Valige soovitud **Java vaheversioon**. Kui valite **uusimast**, kasutatakse rakenduse uusim vaheversioon, mis on saadaval rakenduse teenuses selle Java põhiversioon. **Uusimast** üksus on kordumatu Java rakenduste loodud **rakenduse sätted**. Kui loote Java rakenduse galeriist, siis tuleb container ja töötab muudatuste haldamine. 

12. Valige soovitud **Web ümbrises**. Kui valite container nimi, mis algab **uusimast**, rakenduse hoitakse selle web container põhi versioon, mis on saadaval rakenduse teenuse uusim versioon. 

    ![Web Container versioonid][versions]

13. Klõpsake nuppu **Salvesta**.

    Mõne hetke, muutuvad oma veebirakenduse Java ja konfigureeritud kasutama web ümbris, mille valisite.

14. Klõpsake **veebirakendusi > {oma uue veebirakenduse}**.

15. Liikuge sirvides uue saidi **URL-i** nuppu.

    Veebilehe kinnitab, et olete loonud Java-põhine web appi.

## <a name="marketplace"></a>Azure'i turuplatsilt Java malli kasutamine

Selles jaotises kirjeldatakse Azure'i turuplatsi loomiseks Java web Appi kasutamise kohta. Java-põhine mobiil- või API rakenduse loomiseks saate kasutada ka sama üldist ülesehitust. 

1. [Azure'i portaali] sisselogimine

2. Klõpsake **uus > turuplatsi**.

    ![Uue turuplats][newmarketplace]

3. Klõpsake **Web + Mobile**.

    Peate kerides vasakule kuvamiseks **turuplatsi** tera, kus saate valida **Web + Mobile**.

4. Otsitav tekst, sisestage Java rakenduste serveri **Apache Tomcat** või **Jetty**, nt nimi ja vajutage sisestusklahvi Enter.

5. Klõpsake otsingutulemite Java rakenduste serveri.

    ![Web mobiilsideseadmete Jetty][webmobilejetty]

6. Esimene **Apache Tomcat** või **Jetty** tera, klõpsake nuppu **Loo**.

    ![Sadamasild portaali Blade][jettyblade]

7. Sisestage järgmine **Apache Tomcat** või **Jetty** blade web appi **veebiversiooni** väljale nimi.

    See nimi peab olema kordumatu azurewebsites.net domeen, sest web appi URL on {nimi}. azurewebsites.net. Kui teie sisestatud nimi pole kordumatud, kuvatakse punane hüüumärk tekstiväljale.

8. Valige **Ressursirühm** või looge uus.

    Ressursi rühmade kohta leiate lisateavet teemast [Azure portaalis hallata oma Azure ressursse].

9. Valige soovitud **Rakenduse leping/asukoht** või looge uus.

    Rakenduse teenuse lepingute kohta leiate lisateavet teemast [Azure rakenduse teenuse lepingute ülevaade].

10. Klõpsake nuppu **Loo**.

    ![Sadamasild portaali loomine][jettyportalcreate2]

    Lühike kellaaeg tavaliselt vähem kui minutiga, lõpetab Azure, uue veebirakenduse loomine.

11. Klõpsake **veebirakendusi > {oma uue veebirakenduse}**.

12. Liikuge sirvides uue saidi **URL-i** nuppu.

    ![Sadamasild URL][jettyurl]

    Tomcat teenusega koos vaikimisi määratud lehtede; Kui valisite Tomcat, oleks näha leht sarnaneb järgmises näites.

    ![Web Appi abil Apache Tomcat][tomcat]

    Kui valisite Jetty, oleks näha leht sarnaneb järgmises näites. Jetty pole lehe vaikimisi, et sama JSP, mida on kasutatud tühja Java saidi uuesti kasutada siin.

    ![Web Appi abil Jetty][jetty]

Nüüd, kui olete loonud web Appi abil kuvatakse rakenduse container, leiate teavet selle kohta, kuidas üles laadida rakenduse web appi jaotisest [järgmised toimingud](#next-steps) .

## <a name="next-steps"></a>Järgmised sammud

Selles etapis teil Java rakenduse serveris, kus töötab teenuses Azure rakenduse web Appis. Juurutada kood oma veebirakenduse, lugege teemat [rakenduse lisamine või veebilehe Java veebirakendusse].

Azure'i Java rakenduste arendamise kohta leiate lisateavet teemast [Java Arenduskeskus].

<!-- URL List -->

[Rakenduse või veebilehe lisamine oma Java web app]: ./web-sites-java-add-app.md
[Azure'i rakendust Service lepingud ülevaade]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure'i portaal]: https://portal.azure.com/
[Aktiveerige oma Visual Studio abonendi eelised]: http://go.microsoft.com/fwlink/?LinkId=623901
[tasuta prooviversiooni kasutajaks registreerumine]: http://go.microsoft.com/fwlink/?LinkId=623901
[Proovige rakenduse teenus]: http://go.microsoft.com/fwlink/?LinkId=523751
[Azure'i rakendust Service web app]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java Arenduskeskus]: /develop/java/
[Azure'i portaal abil saate hallata oma Azure ressursid]: ../azure-portal/resource-group-portal.md
[Kohandatud Java veebirakenduse Azure'i üleslaadimine]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
