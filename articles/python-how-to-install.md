<properties
    pageTitle="Python ja SDK - Azure'i installimine"
    description="Saate teada, kuidas installida Python ja SDK Azure'i kasutamine."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Python ja SDK installimine

Python on lihtne häälestamine Windows ja tuleb eelinstallitud Mac, Linux ja [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about). Sellest juhendist juhatab teid läbi installi ja saada oma arvuti Azure kasutamiseks valmis.

## <a name="whats-in-the-python-azure-sdk"></a>Mis on Python Azure SDK?

Azure'i SDK jaoks Python sisaldab komponendid, mis võimaldavad teil töötada, juurutada ja hallata Python rakenduste Azure. Täpsemalt Azure'i SDK jaoks Python sisaldab järgmist:

* **Teekide haldus**. Nende klassi teekide pakub liidest Azure ressursse, näiteks salvestusruumi kontod, virtuaalmasinates haldamine.

* **Käitusaja teegid**. Nende klassi teekide pakub liidest juurdepääsuks Azure funktsioonid, näiteks salvestusruumi ja teenuse siini.

## <a name="which-python-and-which-version-to-use"></a>Millised Python ja versiooni kasutada

On mitmeid maitsed Python tõlke - näited:

* CPython - standard- ja kõige sagedamini kasutatav Python Tõlgi
* PyPy - CPython kiire ja nõuetele vastavuse alternatiivne juurutamine
* IronPython - Python tõlgi, mis töötab .net/CLR-i
* Jython - Python tõlgi, mis töötab Java virtuaalse masina

**CPython** v2.7 või v3.3 + ja PyPy 5.4.0 testitud ja Python Azure'i SDK toetatud.

## <a name="where-to-get-python"></a>Kust Python?

On mitu võimalust saada CPython.

* Otse [www.python.org][]
* Mainekas distributsiooni, nt [www.continuum.io][], [www.enthought.com][] või [www.activestate.com][] kaudu
* Koostage allikast!

Kui teil on vaja teatud, soovitame esmalt kaks võimalust.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>SDK installimine Windowsi, Linuxi ja MacOS (ainult kliendi teegid)

Kui teil on juba installitud Python, saate pip kogumi kliendi teekide installimiseks olemasoleva Python 2.7 või Python 3.3 + keskkonnas. See laadib paketid [Python paketi Index][] (PyPI).

Võib-olla peate administraatoriõigusi:

- Linuxi ja MacOS, kasutage funktsiooni `sudo` käsu: `sudo pip install azure-mgmt-compute`.
- Windows: Avage PowerShelli/käsuviip administraatorina

Saate installida eraldi iga teegi iga Azure'i teenuse jaoks.

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Eelvaate pakettide saab installida, kasutades funktsiooni `--pre` lipp:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Võite installida Azure teekide kogumi üks joon, mis kasutamisel on `azure` metaandmete-pakett. Kuna selle metaandmete-paketi kõik paketid on avaldatud stabiilsusena veel, on `azure` metaandmete-pakett on endiselt eelvaade. Aga core paketid, koodi kvaliteedi/täiendavalt perspektiivid saate lugeda "ühed" sel ajal
- See on ametliku nimi sellisel kujul sünkroonis muude keelte nii kiiresti kui võimalik. Me ei kavatse põhjalikumaks põhimuudatusi seni.

Kuna see on preview release, peate kasutama funktsiooni `--pre` lipp:

```console
   $ pip install --pre azure
```
   
või otse

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Saada lisateavet pakettide

[Python paketi Index][] (PyPI) on Python teekide rikkaliku valiku.  Kui valisite distributsiooni installimiseks, tuleb teil juba kõige huvitavamaks bittide erinevaid veebi arengut tehniline arvutite stsenaariume.


## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio

[Visual Studio Python tööriistad][] (PTVS) on tasuta OSS lisandmoodul Microsoft, mis muutub täieõiguslik Python-IDE VS.

![Kuidas-to-installi-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Kasutades PTVS on valikuline, kuid on soovitatav, kuna see pakub Python ja Web projekti/lahenduse tugi, silumine, profiili, interaktiivne aken, malli redigeerimine ja IntelliSense'i.

PTVS ka on lihtne juurutada Microsoft Azure juurutamise [Pilveteenustega][] ja [veebisaidid][]toega.

PTVS töötab teie olemasoleva Visual Studio 2013 või 2015 installi.  Dokumendid, allalaadimise ja arutelusid, vt [Python Tools for Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python MacOS ja Linux Azure stsenaariumid

Linux või MacOS peamised Azure stsenaariumi, mis on toetatud:

1. Azure'i teenuste tarbimine Python kliendi teekide abil

2. Linux VM teie rakendus töötab?

3. Arendamise ja Azure veebisaitide abil Git avaldamine

Esimene stsenaarium võimaldab eeliseid nagu [bloobimälu][], [järjekorda salvestusruumi][], [tabelimälu][] jne Pythonic ümbriste kaudu Azure'i PaaS võimaluste Azure'i REST API-de rikkaliku veebirakenduste koostamine. Nende toimib ühtemoodi Windowsi, Maci ja Linux.  Saate neid kliendi teegid kohaliku arengu arvuti või Linux VM Azure töötab.

Stsenaariumi VM lihtsalt käivitage Linux VM oma valik (Ubuntu, CentOS, Suse) ja Käivita/haldamine mis teile meeldib.  Näiteks saate [IPython][] Kiirsõnumiga/märkmiku käivitamine Windows/Mac/Linux arvutis ja punkt brauseri Linux või Windows mitme-proc VM IPython Engine töötavate Azure. Vaadake lisateavet [IPython märkmiku Azure][] õpetuse.

Linux VM häälestamise kohta teabe saamiseks lugege õpetuse [loomine virtuaalse masina töötab Linux][] .

Git juurutamise abil saate arendada Python veebirakenduse ja avaldada Azure veebisait opsüsteemide.  Kui vajutada oma andmebaasi Azure, loob automaatselt virtuaalse keskkonna ja pip installida oma nõutav paketid.

Arendamise ja Azure veebisaitide avaldamise kohta leiate lisateavet teemast õpetused [Django veebisaite luua][], [Pudelile veebisaitide loomine][]ja [Lisatakse veebisaite luua][]. Rohkem üldist teavet mis tahes WSGI nõuetele vastavuse raamistiku kasutamise kohta leiate teemast [Azure veebisaitide Python konfigureerida][].


## <a name="additional-software-and-resources"></a>Täiendavat tarkvara ja ressursse.

* [Azure'i SDK Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure'i SDK Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Azure'i ametlik proovide Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Järjepidevuse Kasutusanalüüsi Python jaotuse.][]
* [Enthought Python jaotuse.][]
* [ActiveState Python jaotuse.][]
* [SciPy - komplekti teaduslik Python teekides][]
* [NumPy - Python numerics teek][]
* [Django Project – küps web framework/CMS][]
* [IPython - täpsemalt Kiirsõnumiga/märkmiku jaoks Python][]
* [IPython märkmiku Azure][]
* [Visual Studio github Python tööriistad][]
* [Python Arenduskeskus](/develop/python/)

[Järjepidevuse Analytics Python jaotuse.]: http://continuum.io
[Enthought Python jaotuse.]: http://www.enthought.com
[ActiveState Python jaotuse.]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.Continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - komplekti teaduslik Python teegid]: http://www.scipy.org
[NumPy - Python numerics teek]: http://www.numpy.org
[Django Project – küps web framework/CMS]: http://www.djangoproject.com
[IPython - täpsemalt Kiirsõnumiga/märkmiku jaoks Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython märkmiku Azure]: virtual-machines-linux-jupyter-notebook.md
[Pilveteenused]: cloud-services-python-ptvs.md
[Veebilehed]: web-sites-python-ptvs-django-mysql.md
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Visual Studio github Python tööriistad]: https://github.com/microsoft/ptvs
[Python paketi Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Töötab Linux loomine]: virtual-machines-linux-quick-create-cli.md
[Django veebisaitide loomine]: web-sites-python-create-deploy-django-app.md
[Pudelile veebisaitide loomine]: web-sites-python-create-deploy-bottle-app.md
[Lisatakse veebisaitide loomine]: web-sites-python-create-deploy-flask-app.md
[Azure'i veebisaitidega Python konfigureerimine]: web-sites-python-configure.md
[tabelimälu]: storage-python-how-to-use-table-storage.md
[järjekorda salvestusruum]: storage-python-how-to-use-queue-storage.md
[bloobimälu]: storage-python-how-to-use-blob-storage.md
