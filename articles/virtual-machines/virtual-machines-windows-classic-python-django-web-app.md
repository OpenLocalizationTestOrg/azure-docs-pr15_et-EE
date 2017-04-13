<properties
    pageTitle="Python web app Django | Microsoft Azure'i"
    description="Selle õpetuse õpetab, kuidas majutada Django põhinev veebisaiti Azure Windows Server 2012 R2 andmekeskuse virtuaalse masina näidise klassikaline juurutamise abil."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>Windows Serveri VM Django Tere, maailm veebirakenduse

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac/Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Selles õppetükis kirjeldatakse, kuidas on veebisait Django-põhise Microsoft Azure'i Windows Server virtuaalse masina abil. Selle õpetuse eeldab, et te ei ole eelnevalt kogemusi, kasutades Azure. Pärast selle õpetuse, on teil Django-põhiste rakenduste häälestamine ja töötab pilveteenuses.

Saate teada, kuidas:

* Saate häälestada ka Azure virtuaalse masina Host Django. Kuigi selles õpetuses selgitab, kuidas täita seda jaotises Windows Server, ka saaks sama teha Linux majutatud Azure VM.
* Loo uus Django rakendus Windows.

Selle õpetuse järgides koostate lihtsa Tere, maailm veebirakenduse. Rakendus on majutatud on Azure virtuaalne kohapeal.

Kuvatõmmis täidetud taotluse kuvatakse järgmine.

![Azure'i lehel Tere tulemast maailma kuvamine brauseriaknas][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Loomine ja konfigureerimine on Azure virtuaalse masina Host Django

1. Järgige antud [siin](virtual-machines-windows-classic-tutorial.md) on Azure virtuaalse masina Windows Server 2012 R2 andmekeskuse jaotuse loomiseks.

1. Paluge Azure'i pordi 80-liikluse suunamiseks veebist virtuaalse masina 80 porti:
 - Avage äsja loodud virtuaalne arvuti Azure klassikaline portaali ja klõpsake vahekaarti **lõpp-punktid** .
 - Klõpsake kuva allosas nuppu **lisada** .
    ![lõpp-punkti lisamine](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Avage **TCP** -protokolli kasutaja **avaliku PORDI 80** **Privaatne PORDI 80**nimega.
![][port80]
1. **ARMATUURLAUA** menüü käsku **Ühenda** eemalt logige sisse äsja loodud Azure virtuaalse masina **Kaugtöölaua** abil.  

**Olulise teabe märge:** Kõik järgmised juhised endale, millele logitud virtuaalse masina õigesti ja annavad käsud olemas, mitte teie kohalikus arvutis.

## <a id="setup"> </a>Installimist Python, Django, WFastCGI

**Märkus:** Selleks, et alla laadida Internet Exploreris tuleb konfigureerida IE paoklahvi (ESC) (Start/haldus tööriistad ja serveri halduri kohalik Server ja klõpsake siis nuppu **IE täiustatud turvalisus konfiguratsiooni**seatud olekusse väljas).

1. Installige uusim Python 2.7 või 3.4 [python.org][].
1. Installige abil pip wfastcgi ja django paketid.

    Python 2,7, kasutage järgmist käsku.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Python 3.4, kasutage järgmist käsku.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>FastCGI IIS-i installimine

1. Installige IIS-i toega FastCGI.  See võib kuluda mitu minutit käivitada.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Uue Django rakenduse loomine

1.  *C:\inetpub\wwwroot*, enter Django uue projekti loomiseks järgmine käsk:

    Python 2,7, kasutage järgmist käsku.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Python 3.4, kasutage järgmist käsku.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Uus-AzureService käsu tulemus](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  Käsk **django-administraator** loob Django-põhise veebisaitide põhistruktuuri:

  -   **helloworld\manage.py** aitab teil majutusteenuse käivitamine ja peatamine Django-põhise veebisaidi majutuse
  -   **helloworld\helloworld\settings.py** sisaldab rakenduse Django sätted.
  -   **helloworld\helloworld\urls.py** sisaldab kaardistamine iga URL-i ja selle vaate vahel.

1.  Looge uus fail nimega **views.py** *C:\inetpub\wwwroot\helloworld\helloworld* kataloogis. See sisaldab renderdab "Tere, maailm" lehel soovitud vaadet. Käivitage oma redaktor ja sisestage järgmine:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Asendage urls.py faili sisu järgmist.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>IIS-i konfigureerimine

1. Globaalne applicationhost.config käitleja jaotisele vabastada.  See võimaldab kasutada python sündmuseohjuri sisse oma web.config.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. Luba WFastCGI.  Selle rakenduse globaalne applicationhost.config, mis viitab teie käivitatava Python tõlgi ja wfastcgi.py skripti.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. *C:\inetpub\wwwroot\helloworld*web.config faili loomine.  Väärtus on `scriptProcessor` atribuut peaksid olema samad eelmist toimingut väljund.  Vt lisateavet sees wfastcgi sätete pypi [wfastcgi][] leht.

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. Värskendage, IIS-i vaikimisi veebisaiti osutamiseks django projekti kausta asukoht.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Lõpuks laadida veebilehe brauseris.

![Azure'i lehel Tere tulemast maailma kuvamine brauseriaknas][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Azure'i virtual arvuti sulgemine

Kui olete lõpetanud, mille selles õpetuses, sulgeda ja/või äsja loodud Azure virtuaalne arvuti ressursid muud õpetused ja proovima Azure kasutamise eest eemaldada.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
