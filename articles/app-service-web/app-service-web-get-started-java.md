<properties 
    pageTitle="Võtta kasutusele Azure'i esimese Java veebirakenduse viis minutit | Microsoft Azure'i" 
    description="Siit saate teada, kui lihtne on veebirakenduste Käivita rakendus teenuse valimi rakenduse juurutamine. Käivitage arengu kiiresti teha ja tulemuste vaatamiseks kohe." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Võtta kasutusele Azure'i esimese Java veebirakenduse viis minutit

Selle õpetuse abil saate juurutada [Azure'i rakendust Service](../app-service/app-service-value-prop-what-is.md)lihtne Java web app.
Rakenduse teenuse abil saate luua veebirakenduste, [mobiilirakenduse tagasi lõpetatakse](/documentation/learning-paths/appservice-mobileapps/)ja [API rakendused](../app-service-api/app-service-api-apps-why-best-platform.md).

Siis: 

- Azure'i rakendust Service web app luua.
- Valimi Java minirakenduse juurutamine.
- Vaadake oma koodi, mis töötab reaalajas valmistamisel.

## <a name="prerequisites"></a>Eeltingimused

- Saada FTP/FTPS kliendi, nt [FileZilla](https://filezilla-project.org/).
- Microsoft Azure'i konto. Kui teil pole kontot, saate [tasuta prooviversiooni kasutajaks](/pricing/free-trial/?WT.mc_id=A261C142F) või [aktiveerida oma Visual Studio abonendi eelised](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Võite [Proovida rakenduse teenuse](http://go.microsoft.com/fwlink/?LinkId=523751) ilma Azure'i konto. Starteri rakenduse loomine ja esitamine seda kuni tund--krediitkaarti nõutav, kohustusi.

<a name="create"></a>
## <a name="create-a-web-app"></a>Veebirakenduse loomine

1. [Azure'i portaalis](https://portal.azure.com) Azure kontoga sisse logida.

2. Klõpsake vasakpoolses menüüs **Uus** > **Web + Mobile** > **Web App**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. Rakenduse loomine tera, saate kasutada oma uue rakenduse järgmisi sätteid:

    - **Rakenduse nimi**: tippige kordumatu nimi.
    - **Ressursirühm**: valige **Loo uus** ja tippige selle nimi.
    - **Rakenduse leping/asukoht**: see konfigureerida, klõpsake käsku **Loo uus** nime, asukoha ja hinnakirjad astme rakenduse teenusleping. Võite kasutada **tasuta** hinnad taseme.

    Kui olete lõpetanud, teie rakenduse loomine tera peaks välja nägema selline:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Allservas nuppu **Loo** . Võite klõpsata **teavitusikooni ülaosas edenemise kuvamiseks** .

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Juurutamise on lõpule jõudnud, peaksite nägema selle teavitussõnumi. Klõpsake sõnumi avamiseks oma juurutuse tera.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. Klõpsake labale **juurutamise õnnestus** **Ressursi** link peaks avama oma uue veebirakenduse tera.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Kui soovite oma veebirakenduse Java minirakenduse juurutamine

Nüüd vaatame Java minirakenduse juurutamine Azure FTPS abil.

5. Web appi tera, kerige **rakenduse sätted** või otsige seda ja seejärel klõpsake seda. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Java versiooni**, valige **Java 8** ja klõpsake nuppu **Salvesta**.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Kui saate teate **värskendatud web app sätted**, liikuge http://*&lt;rakendusenimi >*. azurewebsites.net vaikimisi JSP servlet tegelikkuses kuvamiseks.

7. Tagasi web appi tera, liikuge kerides allapoole jaotiseni **juurutamise identimisteabe** või otsige seda ja seejärel klõpsake seda.

8. Juurutamise identimisteave ja klõpsake nuppu **Salvesta**.

7. Klõpsake uuesti sisse labale web app **Ülevaade**. **Juurutamise/FTP kasutajanime** ja **FTPS hostname**, kõrval nuppu **Kopeeri** kopeerida need väärtused.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Nüüd oletegi valmis kasutama oma Java rakenduse FTPS.

8. Teie FTP/FTPS klient, logige sisse oma Azure veebirakenduse FTP server viimases etapis kopeeritud väärtused. Kasutage juurutamise varem loodud parool.

    Järgmine pilt kuvatakse sisse, kasutades FileZilla.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Teile võidakse kuvada Azure tundmatute SSL-serdi kohta. Minna ja jätkata.

9. Klõpsake [seda linki](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) , et sõda faili oma arvutisse alla laadida.

9. Teie FTP/FTPS klient, liikuge **/site/wwwroot/webapps** remote saidi ja lohistage sõda allalaaditud faili oma kohalikus arvutis selle serveri kataloogis.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Klõpsake nuppu **OK** alistamiseks Azure fail.

    >[AZURE.NOTE] Vastavalt Tomcat on vaikekäitumine, failinimi **ROOT.war** sisse /site/wwwroot/webapps annab teile veebirakenduse juursait (http://*&lt;rakendusenimi >*. azurewebsites.net), ja faili nimi ** * &lt;anyname >*.war** annab teile nimega veebirakenduse (http://*&lt;rakendusenimi >*.azurewebsites.net/*&lt;anyname >*).

See on õige! Teie Java rakendus töötab nüüd reaalajas Azure. Liikuge brauseris http://*&lt;rakendusenimi >*. azurewebsites.net selle toimingu kuvamiseks. 

## <a name="make-updates-to-your-app"></a>Tehke oma rakenduse värskendused

Iga kord, kui teil on vaja teha update, vaid uue sõja laadige fail üles oma FTP/FTPS kliendi sama serveri kataloogi.

## <a name="next-steps"></a>Järgmised sammud

[Loo Java veebirakenduse Azure'i turuplatsil malli põhjal](web-sites-java-get-started.md#marketplace). Võite saada oma täielikult kohandatavat Tomcat container ja saada tuttavad Manager UI. 

Silumine oma Azure web app, otse [huvitav](app-service-web-debug-java-web-app-in-intellij.md) või [Eclipse](app-service-web-debug-java-web-app-in-eclipse.md).

Või oma esimese veebirakenduse lisavõimalused. Näiteks:

- [Muud võimalused oma koodi Azure juurutamine](../app-service-web/web-sites-deploy.md)proovida. 
- Azure'i rakenduse viimine uuele tasemele. Oma kasutajate autentimiseks. Selle aluseks nõudmisel skaala. Mõned jõudluse teatiste häälestamine. Kõik vaid mõne hiireklõpsuga. Lugege teemat [esimese veebirakendusse lisa funktsioonid](app-service-web-get-started-2.md).

