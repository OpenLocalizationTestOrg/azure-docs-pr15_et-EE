<properties 
    pageTitle="Azure'i rakendust Service veebirakendustega Python konfigureerimine" 
    description="Selles õppetükis kirjeldatakse suvandite loome ja Azure rakenduse teenuse veebirakenduste lihtsa veebiserverisse lüüsi kasutajaliidese (WSGI) nõuetele vastavuse Python rakenduse konfigureerimine." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Azure'i rakendust Service veebirakendustega Python konfigureerimine

Selles õppetükis kirjeldatakse suvandite loome ja [Azure rakenduse teenuse veebirakenduste](http://go.microsoft.com/fwlink/?LinkId=529714)basic Web serveri lüüsi kasutajaliidese (WSGI) nõuetele vastavuse Python rakenduse konfigureerimine.

See kirjeldab lisavõimalusi Git juurutus, nt virtuaalse keskkonna ja paketi installimine requirements.txt abil.


## <a name="bottle-django-or-flask"></a>Pudel, Django või lisatakse?

Azure'i turuplatsi sisaldab malle pudel, Django ja lisatakse raamistiku. Kui arendate oma esimese veebirakenduse Azure'i rakendust Service või te ei ole Git tuttav, soovitame, et järgite ühte olümpiaandmed, mis sisaldavad üksikasjalike juhiste koostamise töötamine rakenduse abil Git juurutamise Windowsi või Mac-arvutisse galeriist:

- [Pudelile veebirakenduste loomine](web-sites-python-create-deploy-bottle-app.md)
- [Django veebirakenduste loomine](web-sites-python-create-deploy-django-app.md)
- [Lisatakse veebirakenduste loomine](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Azure'i portaalis Web Appi loomine

Selle õpetuse eeldab Azure olemasolevasse tellimusse ja juurdepääsu Azure'i portaal.

Kui teil pole olemasoleva web app, saate selle luua [Azure portaali](https://portal.azure.com).  Klõpsake vasakus ülanurgas nuppu Uus ja seejärel klõpsake nuppu **Web + Mobile** > **Web app**.

## <a name="git-publishing"></a>Git avaldamine

Git avaldamise vastloodud veebirakenduse jaoks konfigureerida veebisaidil [Kohaliku Git juurutamine Azure'i rakendust Service](app-service-deploy-local-git.md)juhiste järgi. Selle õpetuse kasutab Git luua, hallata ja avaldada meie Python veebirakenduse Azure'i rakendust Service.

Kui Git avaldamine on häälestatud, Git hoidla loodud ja seotud web app. Hoidla URL-i kuvatakse ja seda saab kasutada edaspidi push andmete põhjal kohaliku arenduskeskkond pilveteenusesse. Rakenduste kaudu Git avaldada, veenduge, et installitud on ka Git kliendi ja kasutada push web appi sisu Azure'i rakenduse teenusega juhiseid.


## <a name="application-overview"></a>Rakenduste ülevaade

Järgmistes jaotistes on loodud järgmised failid. Nad peavad saama juurkaustas Git hoidla.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI sündmuseohjuri

WSGI on kirjeldatud [SÄRTSU 3333](http://www.python.org/dev/peps/pep-3333/) määratlemine veebiserverisse ja Python Python standard. Erinevate veebirakenduste ja raamistiku abil Python pakub standardne kasutajaliidese. Populaarsed Python web raamistik kasutada täna WSGI. Azure'i rakenduse teenuse veebirakenduste annab teie kasutajatugi sellise raamistiku; Lisaks kogenud kasutajad saavad isegi Autor oma seni, kuni kohandatud sündmuseohjuri järgib WSGI määratlus juhised.

Siin on näide on `app.py` mis määratleb kohandatud sündmuseohjuri:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Selle rakenduse kohalik abil saate käivitada `python app.py`, liikuge sirvides `http://localhost:5555` veebibrauseris.


## <a name="virtual-environment"></a>Virtuaalne keskkonnas

Kuigi eeltoodud näites rakendus pole vaja mis tahes välise pakettide, on tõenäoline, et rakenduse nõuab teatud.

Azure'i Git juurutamise toetab-i väliste pakett sõltuvuste haldamiseks luua virtuaalkeskkonnas.

Kui Azure tuvastab mõne requirements.txt hoidla juurkaustas, loob automaatselt virtuaalne keskkond nimega `env`. See juhtub ainult esimese juurutuse või mis tahes ajal pärast valitud Python käitusaja on muutunud.

Tõenäoliselt soovite luua virtuaalse keskkonna kohalikult arengu, kuid ei kaasata oma Git hoidla.


## <a name="package-management"></a>Paketi haldus

Paketid on loetletud requirements.txt installitakse automaatselt pip kasutamine virtuaalkeskkonnas. See juhtub iga juurutus, kuid pip vahele jätta installi, kui pakett on juba installitud.

Näide `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python versioon

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Näide `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Peate, et määrata, kuidas server peaks hakkama taotlusi web.config faili loomiseks.

Pange tähele, et kui teil on oma andmebaasi, kus x.y vastab valitud Python käitusaja, siis Azure kopeerib automaatselt vastav fail web.config web.x.y.config faili.

Järgmised näited web.config toetuvad virtuaalse keskkonna puhverserveri skript, mis on kirjeldatud järgmises jaotises.  Need töötavad näites kasutatakse funktsiooni WSGI sündmuseohjuri `app.py` kohal.

Näide `web.config` Python 2.7 jaoks:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Näide `web.config` Python 3.4 jaoks:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Staatilise failide tegeleb veebiserver otse ilma Python tähist paranenud jõudlus.

Eespool toodud asukoha staatiline faile kõvakettal peaksid olema samad asukoha URL. See tähendab, et taotluse `http://pythonapp.azurewebsites.net/static/site.css` aitab fail kettal veebisaidil `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`on, kus saate määrata WSGI sündmuseohjuri. Eespool toodud, on `app.wsgi_app` kuna selle sündmuseohjuri on funktsiooni nimega `wsgi_app` sisse `app.py` juurkaustas.

`PYTHONPATH`saab kohandada, kuid kui installite kõik teie sõltuvused virtuaalkeskkonnas, määrates need requirements.txt, peate ei peaks seda muuta.


## <a name="virtual-environment-proxy"></a>Virtuaalse keskkonna puhverserveri

Järgmine skript kasutatakse hankida WSGI sündmuseohjuri, virtuaalse keskkonna ja log tõrgete aktiveerimine. See on mõeldud üldise ja kasutatud ilma muudatusi.

Sisu `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Git juurutamise kohandamine

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Tõrkeotsing – paketi installimine

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Tõrkeotsing – virtuaalse keskkonnas

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Python Arenduskeskus](/develop/python/).

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)





 
