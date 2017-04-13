<properties 
    pageTitle="Python veebirakendusi pudelile Azure" 
    description="Õppeteema, mis tutvustab töötab Python web appi Azure'i rakenduse teenuse veebirakendustes." 
    services="app-service\web" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# <a name="creating-web-apps-with-bottle-in-azure"></a>Azure pudelile veebirakenduste loomine

Selles õppetükis kirjeldatakse, kuidas alustada töötab Python Azure'i rakenduse teenuse veebirakendustes. Web Apps pakub piiratud tasuta hosting ja kiire ja saate kasutada Python! Rakenduse kasvab, mida saab vahetada makstud hosting ja saate ka kõik Azure'i teenuste integreerimine.

Loote web appi, kasutades pudel web raamistikku (vt alternatiivse versiooni selles õpetuses [Django](web-sites-python-create-deploy-django-app.md) ja [lisatakse](web-sites-python-create-deploy-flask-app.md)). Kuvatakse veebirakenduse loomine Azure'i turuplatsilt, Git juurutamise häälestamine ja klooni hoidla kohalikult. Seejärel kuvatakse käivitada web app kohalikult, tehke muudatused, Kinnita ja murravad [Azure'i rakenduse teenuse veebirakenduste](http://go.microsoft.com/fwlink/?LinkId=529714). Õpetuse näitab, kuidas seda teha Windowsi või Mac-arvutisse ja Linux.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="prerequisites"></a>Eeltingimused

- Windowsi, Maci või Linuxi
- Python 2.7 või 3.4
- setuptools, pip, virtualenv (ainult Python 2.7)
- Git
- [Python tööriistad 2.2 Visual Studio][] (PTVS) – Märkus: see pole kohustuslik

**Märkus**: TFS avaldamise pole praegu toetatud Python projektide jaoks.

### <a name="windows"></a>Windows

Kui teil pole veel Python 2.7 või 3.4 installitud (32-bitine), Soovitame installida [Azure SDK Python 2,7] või [Azure SDK Python 3.4] Web platvormi Installeri abil. See installib 32-bitise versiooni Python setuptools, pip, virtualenv, jne (32-bitine Python on, mis on installitud Azure hosti masinad). Teise võimalusena saate avada Python [python.org]kaudu.

Git, soovitame [Git for Windows] või [GitHub for Windows]. Kui kasutate Visual Studio, saate kasutada integreeritud Git tugi.

Soovitame [Python tööriistad 2.2 Visual Studio]installimisel. See pole kohustuslik, kuid kui teil on [Visual Studios], sh tasuta Visual Studio ühenduse 2013 või Visual Studio Express 2013 Web, siis see annab teile hästi Python-IDE.

### <a name="maclinux"></a>Mac/Linux

Peaks olema Python ja Git juba installitud, kuid veenduge, et teil on kas Python 2.7 või 3.4.


## <a name="web-app-creation-on-the-azure-portal"></a>Azure'i portaalis Web Appi loomine

Kõigepealt rakenduse loomisel on [Azure portaali](https://portal.azure.com)kaudu veebirakenduse loomine.  

1. Azure portaali sisse logida ja alumises vasakus nurgas nuppu **Uus** . 
3. Tippige otsinguväljale "püüton".
4. Otsingutulemustes, valige **pudel**ja seejärel klõpsake nuppu **Loo**.
5. Uus pudel rakendus, näiteks luua uue rakenduse teenusleping ja uue ressursirühma seda konfigureerida. Klõpsake nuppu **Loo**.
6. Git avaldamise vastloodud veebirakenduse jaoks konfigureerida veebisaidil [Kohaliku Git juurutamine Azure'i rakendust Service](app-service-deploy-local-git.md)juhiste järgi.
 
## <a name="application-overview"></a>Rakenduste ülevaade

### <a name="git-repository-contents"></a>Git hoidla sisu

Siit leiate ülevaate leiate algse Git hoidla, mis kuvatakse me klooni järgmises jaotises failid.

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

Rakenduse allikast. Koosneb 3 juhtslaidi paigutus lehed (indeks kontakti kohta).  Staatiliseks sisuks ja skriptide sisaldavad alglaaduri, jquery, modernizr ja vasta.

    \app.py

Kohaliku arengu serveri tugi. Kasutage seda kohalikult rakenduse käivitamiseks.

    \BottleWebProject.pyproj
    \BottleWebProject.sln

Projekti failide [Python Tools for Visual Studio]jaoks.

    \ptvs_virtualenv_proxy.py

IIS-i puhverserveri virtuaalkeskkonnas ja PTVS Kaug silumine tugi.

    \requirements.txt

Välise pakettide selle rakenduse jaoks vajalik. Juurutamise script on pip installige loendis selle faili paketid.
 
    \web.2.7.config
    \web.3.4.config

IIS-i failid. Juurutamise skripti kasutab vastav web.x.y.config ja kopeeritakse web.config.

### <a name="optional-files---customizing-deployment"></a>Valikuline failide - kohandamise juurutamine

[AZURE.INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Valikuline failide - Python käitusaja

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Täiendavad failid serveris

Mõned failid serveris olemas, kuid ei lisata git hoidla. Need on loodud juurutamise skript.

    \web.config

IIS-i konfigureerimise faili. Loodud web.x.y.config iga juurutuse.

    \env\

Python virtuaalse keskkonnas. Kui ühilduvad virtuaalse keskkonna pole veel olemas web app loodud käigus juurutamist.  Loendis requirements.txt on pip installitud, kuid pip vahele jätta installi, kui paketid on juba installitud.

Järgmiseks 3 jaotistes kirjeldatakse jätkata web app areng 3 viibite alusel:

- Windowsi Python Tools for Visual Studio abil
- Windowsi käsurea
- Mac/Linux käsurea


## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Web App arengu - Windows - Python Visual Studio tööriistad

### <a name="clone-the-repository"></a>Klooni hoidla

Esmalt klooni hoidla Azure'i portaalis esitatud URL-i. Lisateabe saamiseks vt [Kohaliku Git juurutamise Azure'i rakenduse teenusega](app-service-deploy-local-git.md).

Avage lahenduse fail (.sln), mis sisaldab root hoidla.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a>Virtuaalse keskkonna loomine

Nüüd saame luua virtuaalse keskkonna arengu. Paremklõpsake **Python keskkonnas** valige **Lisada virtuaalse keskkonna …**.

- Veenduge, et keskkonna nimi on `env`.

- Valige base Tõlgi. Veenduge, et kasutada sama versiooni Python, mis on valitud oma web app (runtime.txt või Azure'i portaalis oma veebirakenduse tera **Rakenduse sätted** ).

- Veenduge, et alla laadima ja installima paketid on valitud.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

Klõpsake nuppu **Loo**. See loomine virtuaalse keskkonna ja installige loendis requirements.txt sõltuvused.

### <a name="run-using-development-server"></a>Käivitage arengu serveri kasutamine

Silumine käivitamiseks vajutage klahvi F5 ja veebibrauseris avatakse automaatselt töötab kohalikult lehele.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

Saate määrata katkestuspunktid allikad, kasutage vaadata windows jne. [Python Tools for Visual Studio dokumentatsiooni kohta] lisateabe saamiseks vt teemat erinevaid funktsioone.

### <a name="make-changes"></a>Muudatuste tegemine

Nüüd saate proovida tehes muudatusi rakenduse allikatest ja/või mallid.

Kui olete muudatuste testinud, kinnita need Git hoidla:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a>Lisateavet pakettide installimine

Rakenduse võib-olla sõltuvused Python ja pudel.

Saate installida pakette pip abil. Paketi installimiseks paremklõpsake virtuaalse keskkonna ja valige **Installida Python pakett**.

Näiteks installida Azure SDK Python, mis annab teile juurdepääsu Azure storage, teenuse siini ja muude Azure'i teenuste, sisestage `azure`:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

Paremklõpsake virtuaalse keskkonna ja valige **Genereeri requirements.txt** requirements.txt värskendada.

Seejärel kinnitage muudatused requirements.txt Git hoidla.

### <a name="deploy-to-azure"></a>Azure'i juurutamine

Juurutamine käivitamiseks nuppu **Sünkrooni** või **Push**. Sünkroonimine ei nii push ja tõmmake.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

Esimese juurutamise võtab aega, nagu see loob virtuaalse keskkonnas, installi-pakettide jne.

Visual Studio ei kuvata juurutamise edenemist. Kui soovite selle väljundi üle vaatama kohta leiate jaotisest [tõrkeotsing – juurutamise](#troubleshooting-deployment)kohta.

Liikuge sirvides Azure'i URL-i muudatuste vaatamiseks.


## <a name="web-app-development---windows---command-line"></a>Web app areng - Windows - käsu joone

### <a name="clone-the-repository"></a>Klooni hoidla

Esmalt klooni Azure'i portaalis esitatud URL-i hoidla ning lisage Azure hoidla Interneti. Lisateabe saamiseks vt [Kohaliku Git juurutamise Azure'i rakenduse teenusega](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Virtuaalse keskkonna loomine

Loome uue virtuaalse keskkonna arengu eesmärgil (ei saa lisada see hoidla). Virtuaalkeskkonnas Python ei ole liigutatavat, nii, et iga arendaja, töötate rakenduse saab luua oma kohalikult.

Veenduge, et kasutada sama versiooni Python, mis on valitud oma web app (runtime.txt või rakenduse sätted tera Azure'i portaalis veebirakenduse jaoks)

Jaoks Python 2.7:

    c:\python27\python.exe -m virtualenv env

Jaoks Python 3.4:

    c:\python34\python.exe -m venv env

Mis tahes välise pakettide nõutud rakenduse installida. Hoidla juurtasemel requirements.txt faili abil saate installida paketid virtuaalse keskkond:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Käivitage arengu serveri kasutamine

Taotlemiseks arengut server järgmise käsu abil saate käivitada:

    env\scripts\python app.py

Konsooli kuvatakse URL-i ja pordi server on kuulata.

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

Avage see URL oma veebibrauseri.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a>Muudatuste tegemine

Nüüd saate proovida tehes muudatusi rakenduse allikatest ja/või mallid.

Kui olete muudatuste testinud, kinnita need Git hoidla:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Lisateavet pakettide installimine

Rakenduse võib-olla sõltuvused Python ja pudel.

Saate installida pakette pip abil. Näiteks tippige Python, mis annab teile juurdepääsu Azure storage, teenuse siini ja muude Azure'i teenuste, installige Azure'i SDK:

    env\scripts\pip install azure

Veenduge, et värskendada requirements.txt.

    env\scripts\pip freeze > requirements.txt

Muudatuste kinnitamine:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Azure'i juurutamine

Juurutamine käivitamiseks vajutage muudatuste Azure:

    git push azure master

Kuvatakse väljund juurutamise skripti, sealhulgas virtuaalse keskkonna loomine installimise paketid, web.config loomine.

Liikuge sirvides Azure'i URL-i muudatuste vaatamiseks.


## <a name="web-app-development---maclinux---command-line"></a>Web app arengu - Mac/Linux - käsurea

### <a name="clone-the-repository"></a>Klooni hoidla

Esmalt klooni Azure'i portaalis esitatud URL-i hoidla ning lisage Azure hoidla Interneti. Lisateabe saamiseks vt [Kohaliku Git juurutamise Azure'i rakenduse teenusega](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Virtuaalse keskkonna loomine

Loome uue virtuaalse keskkonna arengu eesmärgil (ei saa lisada see hoidla). Virtuaalkeskkonnas Python ei ole liigutatavat, nii, et iga arendaja, töötate rakenduse saab luua oma kohalikult.

Veenduge, et kasutada sama versiooni Python, mis on valitud oma web app (runtime.txt või Azure'i portaalis oma veebirakenduse tera rakenduse sätted).

Jaoks Python 2.7:

    python -m virtualenv env

Jaoks Python 3.4:

    python -m venv env
või pyvenv keskkonna

Mis tahes välise pakettide nõutud rakenduse installida. Hoidla juurtasemel requirements.txt faili abil saate installida paketid virtuaalse keskkond:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Käivitage arengu serveri kasutamine

Taotlemiseks arengut server järgmise käsu abil saate käivitada:

    env/bin/python app.py

Konsooli kuvatakse URL-i ja pordi server on kuulata.

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

Avage see URL oma veebibrauseri.

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a>Muudatuste tegemine

Nüüd saate proovida tehes muudatusi rakenduse allikatest ja/või mallid.

Kui olete muudatuste testinud, kinnita need Git hoidla:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Lisateavet pakettide installimine

Rakenduse võib-olla sõltuvused Python ja pudel.

Saate installida pakette pip abil. Näiteks tippige Python, mis annab teile juurdepääsu Azure storage, teenuse siini ja muude Azure'i teenuste, installige Azure'i SDK:

    env/bin/pip install azure

Veenduge, et värskendada requirements.txt.

    env/bin/pip freeze > requirements.txt

Muudatuste kinnitamine:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Azure'i juurutamine

Juurutamine käivitamiseks vajutage muudatuste Azure:

    git push azure master

Kuvatakse väljund juurutamise skripti, sealhulgas virtuaalse keskkonna loomine installimise paketid, web.config loomine.

Liikuge sirvides Azure'i URL-i muudatuste vaatamiseks.


## <a name="troubleshooting---package-installation"></a>Tõrkeotsing – paketi installimine

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Tõrkeotsing – virtuaalse keskkonnas

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## <a name="next-steps"></a>Järgmised sammud

Järgmiste linkide kaudu Lisateavet pudel ja Python tööriistad Visual Studio: 
 
- [Pudel dokumentatsioon]
- [Python Tools for Visual Studio dokumentatsioon]

Azure'i Tabelimälu ja MongoDB kasutamise kohta teavet:

- [Pudel ja MongoDB Azure Python Tools for Visual Studio abil]
- [Pudel ja Azure Python Tools for Visual Studio abil Azure'i Tabelimälu]

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Pudel ja MongoDB Azure Python Tools for Visual Studio abil]: web-sites-python-ptvs-bottle-table-storage.md
[Pudel ja Azure Python Tools for Visual Studio abil Azure'i Tabelimälu]: web-sites-python-ptvs-bottle-table-storage.md

<!--External Link references-->
[Azure'i SDK Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure'i SDK Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[Python.org]: http://www.python.org/
[Git for Windows]: http://msysgit.github.io/
[GitHub for Windows]: https://windows.github.com/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python tööriistade 2.2 Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python Tools for Visual Studio dokumentatsioon]: http://aka.ms/ptvsdocs 
[Pudel dokumentatsioon]: http://bottlepy.org/docs/dev/index.html
 
