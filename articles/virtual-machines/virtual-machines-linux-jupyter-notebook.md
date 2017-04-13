<properties
    pageTitle="Luua Jupyter/IPython märkmiku | Microsoft Azure'i"
    description="Saate teada, kuidas Jupyter/IPython märkmiku Linux virtual arvutis loodud ressursi halduri juurutamise mudeli Azure juurutamine."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Jupyter märkmiku Azure

[Jupyter projekti](http://jupyter.org), varem [IPython projekt](http://ipython.org)sisaldab tööriistu teaduslik arvutuste võimsaid interaktiivsed kestad, mis sisaldavad koodi täitmine reaalajas arvutuslik dokumendi loomise abil. Need märkmikufaile, mis võib sisaldada suvalise teksti, matemaatilised valemid, Sisestuskeel kood, tulemused, graafika, videoid ja muud tüüpi meediumi kaasaegne veebibrauseri suudab kuvada. Kas olete täiesti uus Python ja soovite teada, lõbus, interaktiivne keskkonnas või teha mõned raske samal ajal tehniline arvutuste, Jupyter märkmikus on hea valik.

![Kuvatõmmis](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) abil SciPy ja Matplotlib paketid heli salvestamist struktuuri analüüsimiseks.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter kaks võimalust: Azure'i märkmike või kohandatud juurutamine
Azure'i pakub teenust, mille abil saate [kiiresti hakata Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Azure'i märkmiku teenuse abil pääsete hõlpsalt juurdepääsu Jupyter's veebileht liidest scalable arvutuslik ressursid kõigi võimsusega Python ja selle paljude teekide.  Kuna installimise teostab teenus, kasutajad pääsevad need ressursid ilma vajaduseta haldus ja kasutaja konfiguratsioon.

Kui märkmik teenus ei tööta teie siis jätkake lugege seda artiklit, mis kuvatakse näitab teile, kuidas juurutada Jupyter märkmiku Microsoft Azure, kasutades Linux virtuaalmasinates (VMs).

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Loomine ja konfigureerimine VM Azure

Esimene samm on luua töötavate Azure virtuaalse masina (VM).
See VM on pilves täielik operatsioonisüsteem ja kasutatakse Jupyter märkmiku käivitamiseks. Azure'i suudab käitada Windows ja Linux virtuaalmasinates ja käsitleme Jupyter mõlemat tüüpi virtuaalmasinates häälestamine.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Linux VM loomine ja Jupyter jaoks pordi avamine

Järgige juhiseid [siin] antud[ portal-vm-linux] luua virtuaalse masina *Ubuntu* jaotuse. Selle õpetuse kasutab Ubuntu Server 14.04 LTS. Oletame kasutaja nimi *azureuser*.

Pärast virtuaalse masina kasutab läheb vaja avada turvalisuse reegel klõpsake jaotises võrku turvalisus.  Azure'i portaalis, minge **Võrgu turberühmad** ja avage vahekaart vastab teie VM turberühma. Peate lisama sissetulev turvalisus reegli abil järgmisi sätteid: **TCP** protokolli, **\*** lähteport (avaliku) ja **9999** jaoks sihtport (privaatne).

![Kuvatõmmis](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

Kuigi võrgu turberühma klõpsake **Võrgu liidesed** ja Märkus **Avaliku IP-aadress** , kui see on vaja ühenduse loomiseks oma VM järgmise juhise juurde.

## <a name="install-required-software-on-the-vm"></a>VM nõutud tarkvara installimine

Meie VM Jupyter märkmiku käivitamiseks me peate esmalt installima Jupyter ja sõltuvustega. Ühenduse loomine oma linux vm abil ssh ja sidumine kasutajanime ja parooli, saate valida, kui olete loonud vm. Selles õppetükis me kasutama PuTTY ja ühenduse loomine Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Ubuntu Jupyter installimine
Installige Anaconda, populaarsed andmete teadus python jaotuse, kasutades ühte järgmistest [Järjepidevuse](https://www.continuum.io/downloads)Analytics linkide.  Selle dokumendi kirjalikult selle all lingid on kõige kuni kuupäeva versioonid.

#### <a name="anaconda-installs-for-linux"></a>Anaconda installe Linuxi
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bitise</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bitise</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bitise</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bitise</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Installimise Anaconda3 2.3.0 64-bitine Ubuntu
Näiteks see on, kuidas saate installida Anaconda Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Kuvatõmmis](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Jupyter konfigureerimine ja SSL-i abil
Pärast installimist tuleb mõne hetke seadistada Jupyter failid. Kui teil tekib probleemid konfigureerimisega Jupyter võib olla kasulik [Jupyter märkmiku serveri dokumentatsioonist](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Järgmise me `cd` profiili kataloogi SSL-serdi loomine ja redigeerimine konfiguratsioonifail profiilid.

Kasutage Linux järgmine käsk.

    cd ~/.jupyter

Järgmise käsu abil saate luua SSL-sert (Windows ja Linux).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Pange tähele, et kuna iseallkirjastatud SSL-serdi loomine, kui ühenduse märkmik brauseri teile turbehoiatus.  Tootmise kasutamine, mida soovite kasutada õigesti allkirjastatud ettevõtte seotud sert.  Kuna serdihaldus on see demo väljapoole, saame jääda iseallkirjastatud serdi nüüd.

Lisaks serdiga, peate teadma parooliga kaitsmiseks volitamata märkmikku.  Turvalisuse põhjustel kasutab Jupyter selle konfiguratsioonifail krüptitud paroole, nii, et peate esmalt oma parooli krüptimine.  IPython pakub kasuliku teha; käsuviibale järgmine käsk.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

See küsib teilt parool ja kinnitus ning seejärel prindib parool. Pange tähele selle järgmist toimingut.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Järgmiseks me on muuta profiili konfiguratsioonifail, mis on selle `jupyter_notebook_config.py` olete kataloogis fail.  Teate, et see fail on olemas--lihtsalt luua, kui see on nii.  See fail on mitmeid määratavaid väljad ja vaikimisi on kogu kommenteerinud välja.  Saate avada selle faili mis tahes tekstiredaktor, meelepäraseks ja peate tagama, et see on vähemalt järgmist. **Asendage c.NotebookApp.password config koos sha1 eelmises juhises**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Jupyter märkmiku käivitamine

Selles etapis oleme alustan Jupyter märkmik. Selle tegemiseks liigute soovite talletada märkmike sisse ja asuge Jupyter märkmiku server järgmise käsu abil.

    /anaconda3/bin/jupyter-notebook

Peaks nüüd olema näpunäidetele juurde Jupyter märkmiku aadressil `https://[PUBLIC-IP-ADDRESS]:9999`.

Kui märkmikule juurdepääsuks, küsib sisselogimislehele oma parool. Ja kui olete sisse loginud, näete "Jupyter märkmiku armatuurlaud", mis on kogu märkmiku toiminguteks ONTROLL keskele.  Sellelt lehelt, saate uute märkmike loomine ja avada olemasolevaid.

![Kuvatõmmis](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Märkmiku Jupyter abil

Kui klõpsate nuppu **Uus** , kuvatakse järgmine avalehel.

![Kuvatõmmis](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

Ala, mis on tähistatud mõne `In []:` viip on sisestusalale, ja siia saate sisestada kehtivate Python koodi ja see käivitatakse klõpsamist `Shift-Enter` või "Start" ikooni (paremale osutav kolmnurk tööriistaribal).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Võimas paradigma: live arvutuslik dokumentide rikasmeedia

Märkmiku ise on väga loomulikus kõigile, kes on kasutanud Python ja sõna protsessor, tundub, sest see on mõned viisid, kuidas kombineerida: Python kood plokid täidate, kuid võite ka hoida märkmete ja muu teksti, muutes laadi lahtrile "Kood" "Allahindlusest" rippmenüüst menüü tööriistariba abil.

Jupyter on palju rohkem kui sõna protsessor, nagu see lubab segamine arvutus ja RTF-meediumi (teksti, piltide, video ja peaaegu kõike tänapäevane veebibrauseris kuvamise). Võite segada, teksti, koodi, videoid ja rohkem!

![Kuvatõmmis](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

Ja Python on palju suurepäraseid teekide teadus- ja võimsusega arvuti, järgmine pilt lihtsa arvutuse saab teha sama lihtne nagu keerukate võrgu analüüsi, kõik ühes keskkonnas.

See paradigma kaasaegne web power segamist reaalajas arvutus pakub palju võimalusi ja sobib ideaalvariandis mitte pilveteenuses; Märkmiku saab kasutada:

* Kui arvutuslik mustand salvestada uurimusliku töö probleem.

* Tulemuste jagamiseks kolleegide, live' arvutuslik vormi või paberkandjal vormingutes (HTML, PDF-faili).

* Levitamise ja reaalajas õppematerjale kaasata nii, et õpilaste kohe proovida otse koodi arvutus, esitada, muuta ja uuesti käivitada interaktiivseks.

* "Käivitatava papers", mida esitada tulemuste nii, et saate kohe paljundada, kinnitatud või laiendatud teistele anda.

* Koostööpõhise arvutuste platvormi: mitme kasutaja saab sisse logida sama märkmiku server arvutuslik otseseanss ühiskasutusse anda.


Kui lähete IPython lähtekoodi [hoidla][], leiate terve kausta märkmiku näiteid, mille saate alla laadida ja seejärel katsetamiseks kasutada oma Azure Jupyter VM.  Lihtsalt allalaadimine on `.ipynb` failide saidilt ja üles laadida peale armatuurlaua märkmiku Azure VM (või alla laadida otse VM).

## <a name="conclusion"></a>Kokkuvõte

Märkmiku Jupyter pakub võimsaid kasutajaliidese juurdepääsuks interaktiivseks power Python ökosüsteemi Azure.  See hõlmab mitmesuguseid kasutus juhul, kui lihtne avastamine sh Õppekeskuse Python, Andmeanalüüs ja visualiseerimine, simulatsioon ja paralleelsete arvutuste. Tulemiks oleva märkmiku dokumendid sisaldavad täielik kirje arvutuste, mis tehakse ja saab Jupyter teiste kasutajatega jagada.  Märkmiku Jupyter saab kasutada kohaliku rakenduse, kuid sobib ideaalvariandis mitte cloud juurutuste Azure

Jupyter põhilised funktsioonid on saadaval sees Visual Studio [Python Tools for Visual Studio][] (PTVS) kaudu. PTVS on tasuta ja avatud allika lisandmoodul Microsoft, mis muutub Visual Studio Täpsem Python arenduskeskkond, mis sisaldab IntelliSense'i, silumine, kus Täpsem redaktor profiilide ja paralleelsete arvutuste integreerimine.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Python Arenduskeskus](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[hoidla]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
