<properties 
    pageTitle="Python web app Django Linux | Microsoft Azure'i" 
    description="Saate teada, kuidas majutada Django vastavalt veebirakenduse Azure Linux virtuaalse masina abil." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Django Tere, maailm veebirakenduse Linux VM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac/Linux](virtual-machines-linux-python-django-web-app.md)

<br>

Selles õppetükis kirjeldatakse, kuidas on veebisait Django-põhise Microsoft Azure'i Linux virtuaalse masina abil. Selle õpetuse eeldab, et te ei ole eelnevalt kogemusi, kasutades Azure. Selle juhendi vormistamisel on teil Django-põhiste rakenduste häälestamine ja töötab pilveteenuses.

Saate teada, kuidas:

* Install on Azure virtuaalse masina Host Django. Kui selle õpetuse selgitab, kuidas täita seda **Linux**, ka saaks sama teha Windows Server VM Azure majutada. 
* Saate luua uue Django rakenduse Linux.

Selle õpetuse järgides koostate lihtsa Tere, maailm veebirakenduse. Rakendus on majutatud on Azure virtuaalne kohapeal.

Allpool on täidetud taotluse kuvatõmmis.

![Azure'i lehel Tere tulemast maailma kuvamine brauseriaknas](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Loomine ja konfigureerimine on Azure virtuaalse masina Host Django

1. Järgige antud [siin](virtual-machines-linux-quick-create-portal.md) on Azure virtuaalse masina *Ubuntu Server 14.04 LTS* jaotuse loomiseks.  Soovi korral saate valida Paroolautentimine asemel SSH avalik võti.

1. Pordi 80 juhiste abil sissetuleva http-liikluse lubamiseks võrgu turberühma redigeerimine [siin](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Vaikimisi pole virtual seadme täielik domeeninimi (FQDN).  Järgmiste juhiste abil saate luua [siin](virtual-machines-linux-portal-create-fqdn.md).  See toiming on valikuline selles õpetuses lõpuleviimiseks.

## <a id="setup"> </a>Arengu keskkonna häälestamine

**Märkus:** Kui on vaja installida Python või kliendi teegid kasutada, leiate [Python installi juhend](../python-how-to-install.md).

Ubuntu Linux VM on juba eelinstallitud Python 2.7, kuid see pole Apache või django installitud.  Järgmiste juhiste abil ühenduse oma VM ja alla ja installige Apache django.

1.  Käivitage **Terminal** uues aknas.
    
1.  Sisestage järgmine käsk ühenduse Azure VM.  Kui te ei saanud FQDN loonud, saate ühendada abil kuvatakse Kokkuvõte Azure klassikaline portaalis virtuaalse masina avaliku IP-aadressi.

        $ ssh yourusername@yourVmUrl

1.  Sisestage django installimiseks järgmised käsud:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Sisestage järgmine käsk installida Apache mod-wsgi.

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Uue Django rakenduse loomine

1.  Avage **Terminal** aken kasutasite eelmises jaotises ssh sisse oma VM.
    
1.  Sisestage Django uue projekti loomiseks järgmised käsud:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    **Django-admin.py** skript loob Django-põhise veebisaitide põhistruktuuri:
    -   **helloworld/Manage.py** aitab teil majutusteenuse käivitamine ja peatamine Django-põhise veebisaidi majutuse
    -   **helloworld/helloworld/Settings.py** sisaldab rakenduse Django sätted.
    -   **helloworld/helloworld/urls.py** sisaldab vastenduse iga URL-i ja selle vaate vahel.

1.  Looge uus fail nimega **views.py** **/var/www/helloworld/helloworld** kataloogis. See sisaldab renderdab "Tere, maailm" lehel soovitud vaadet. Käivitage oma redaktor ja sisestage järgmine:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Nüüd Asendage **urls.py** faili sisu järgmist:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Kuidas häälestada Apache

1.  Saate luua ka Apache virtuaalne host konfiguratsiooni faili **/etc/apache2/sites-available/helloworld.conf**. Määratud sisu järgmine ja Asendage *yourVmName* tegelikku nime masina kasutate (nt *pyubuntu*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Luba saidi järgmine käsk:

        $ sudo a2ensite helloworld

1.  Taaskäivitage Apache järgmine käsk:

        $ sudo service apache2 reload

1.  Lõpuks laadimine veebilehe brauseris:

    ![Azure'i lehel Tere tulemast maailma kuvamine brauseriaknas](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Azure'i virtual arvuti sulgemine

Kui olete lõpetanud selles õpetuses, sulgemine ja/või Eemalda oma vastloodud Azure virtuaalse masina ressursid muud õpetused ja proovima Azure kasutamise eest.
