<properties
    pageTitle="Virtuaalse masina nimega serveriks IPython märkmiku häälestamine | Microsoft Azure'i"
    description="Täiustatud analüüsi üles mõni Azure virtuaalse masina andmete teadus keskkonnas kasutamiseks IPython serveriga määramiseks."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Seadistada Azure'i virtuaalarvuti serveriks IPython märkmiku täiustatud analüüsi jaoks

Selles teemas kirjeldatakse, kuidas ette valmistada ja konfigureerimiseks on Azure virtuaalse masina täiustatud analüüsi, mida saab kasutada andmete teadus keskkonna jaoks. Windowsi virtual arvuti on konfigureeritud toetavad vahendid, nagu näiteks IPython märkmik, Azure'i salvestusruumi Explorer, AzCopy, samuti muud Utiliidid, mis on abiks täiustatud analüüsi projektid. Azure'i salvestusruumi Exploreris ja AzCopy, näiteks pakkuda mugavat võimalust andmete üleslaadimiseks Azure'i bloobimälu oma kohalikust arvutist või laadige see oma arvutisse alla bloobimälu.

## <a name="create-vm"></a>Samm 1: Loo üldine otstarve Azure virtuaalse masina

Kui teil on juba on Azure virtuaalse masina ja soovite lihtsalt luua IPython märkmiku server seda, saate selle sammu vahele jätta ja jätkake [Samm 2: lõpp IPython märkmike jaoks lisamiseks mõne olemasoleva virtuaalse masina](#add-endpoint).

Enne alustamist Azure virtuaalse masina loomise protsess, peate arvuti, mida on vaja oma projekti andmete suuruse. Väiksemad masinad on vähem mälu ja vähem protsessorituuma kui suuremad masinad, kuid need on väiksemate. Seadme tüübid ja hinnad loendi leiate teemast <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtuaalmasinates hinnad</a> lehe

1. <a href="https://manage.windowsazure.com" target="_blank">Azure'i klassikaline portaali</a>sisse logida ja alumises vasakus nurgas nuppu **Uus** . Avaneb aken. Valige **ARVUTADA** -> **VIRTUAALSE masina** -> **galeriist**.

    ![Tööruumi loomine][24]

2. Valige üks järgmistest pildid.

    * Windows Server 2012 R2 andmekeskusega
    * Windows Serveri Essentials Experience (Windows Server 2012 R2)

    Valige nool paremale konfiguratsiooni järgmisele lehele liikumiseks parempoolses allnurgas.

    ![Tööruumi loomine][25]

3. Sisestage virtuaalse masina, mida soovite luua, valige soovitud suurus masina nimi (vaikimisi: A3) masina saab protsessi andmete suurus ja kuidas võimsaid soovite masina olla (mälu suurus ja Arvuta valdkond arv), sisestage kasutajanimi ja parool seadme jaoks. Klõpsake paremale konfiguratsiooni järgmisele lehele liikumiseks osutav nool.

    ![Tööruumi loomine][26]

4. Valige **Piirkond/osaleja rühma/VIRTUAL NETWORK** **Salvestusruumi konto** , mida te plaanite kasutada seda virtuaalse masina sisaldava ja valige konto salvestusruumi. Lisage lõpp allosas väljal **lõpp-punktid** ("IPython" siin) lõpp-punkti nimi. Saate valida **nime** lõpp-punkti ja mis tahes täisarv vahemikus 0 kuni 65536, mis on **saadaval** **Avalik PORT**suvalist märgistringi. **Privaatne PORT** peab olema **9999**. Kasutajad peaksid **vältimiseks** kasutamise avaliku pordid, mis on juba määratud Interneti-teenuseid. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Pordid Interneti teenuste</a> loetletakse pordid, mis on määratud ja vältida.

    ![Tööruumi loomine][27]

5. Klõpsake märkeruutu virtuaalse masina ettevalmistamise protsessi käivitamiseks.

    ![Tööruumi loomine][28]


15-25 minutit virtuaalse masina ettevalmistamise protsessi lõpuleviimiseks võib kuluda. Pärast virtuaalse masina loomist peaks näitavad selle arvuti oleku **töötab**.

![Tööruumi loomine][29]

## <a name="add-endpoint"></a>Samm 2: Lisada mõne olemasoleva virtuaalse masina IPython märkmike jaoks lõpp

Kui olete loonud virtuaalse masina samm 1 juhiseid järgides, lõpp-punkti jaoks IPython märkmik on juba lisatud ja saate selle etapi vahele jätta.

Virtuaalne arvutisse on juba olemas, kui peate lisama pärast installimist sammus 3 all, Logi Azure'i klassikaline portaali sisse IPython märkmiku lõpp, virtuaalse masina valige ja lisage IPython märkmiku serveri lõpp-punkti. Järgmisel joonisel on kuvatõmmis portaali pärast Windowsi virtuaalse masina on lisatud lõpp-punkti IPython märkmiku jaoks.

![Tööruumi loomine][17]

## <a name="run-commands"></a>Samm 3: Installi IPython märkmiku ja muud täiendavad tööriistad

Pärast loomist virtuaalse masina Kaugtöölaua protokolli (RDP) abil Windowsi virtuaalse masina sisse logida. Juhised leiate teemast [virtuaalse masina opsüsteemi Windows Server sisse logida](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Avage **Käsuviip** (**mitte PowerShelli käsuaknas**) **administraator** ja käivitage järgmine käsk.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Kui installimine on lõpule jõudnud, IPython märkmiku server on käivitatud automaatselt selle *C:\\kasutajate\\\<kasutajanimi\>\\dokumentide\\IPython märkmike* kataloogi.

Küsimise korral sisestage parool IPython märkmiku ja arvutisse administraatori parool. See võimaldab IPython märkmiku käivitamiseks teenuse arvutis.

## <a name="access"></a>Samm 4: Accessi IPython märkmike veebibrauseri kaudu
Serveri IPython märkmiku avamiseks avage web brauseri ja sisendi *https://&#60;virtual DNS-i nimi >: & #60; avaliku pordinumber >* tekstiväljale URL-i. Siin on *& #60; avaliku pordinumber >* peaks olema määratud kui IPython märkmiku lõpp-punkti lisati pordinumber.

Funktsiooni *& #60; virtuaalse masina DNS-i nimi >* aadressil klassikaline portaalis Azure. Pärast sisselogimist klassikaline portaali, klõpsake **VIRTUAALMASINATES**, valige seadme, mille lõite, ja seejärel valige **ARMATUURLAUD**, DNS-i nimi kuvatakse järgmiselt:

![Tööruumi loomine][19]

Ilmneda hoiatuse selle _on probleem selle veebisaidi turbesert_ (Internet Explorer) või _teie ühendus ei ole privaatne_ (Chrome), nagu on näidatud järgmises arvud. Klõpsake nuppu **Jätka see veebisait (pole soovitatav)** (Internet Explorer) või **Täpsemalt** ja seejärel * *jätkata, et & #60;* DNS-i nimi*> (ohtlikud) ** (Chrome) jätkata. Seejärel sisestage parool teie määratud varem IPython märkmikele juurdepääsuks.

**Internet Explorer:**
![tööruumi loomine][20]

**Chrome:**
![tööruumi loomine][21]

Pärast sisselogimist IPython märkmikku, kataloog *DataScienceSamples* jäänud brauser. See kaust sisaldab Microsoft aitab kasutajatel läbiviimine teadus Andmetoimingute ühiskasutusse antud valimi IPython märkmikud. Nende valimi IPython märkmikke on välja möllitud [**Github**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) hoidla on virtuaalmasinates ajal IPython märkmiku serveri häälestamine protsess. Microsoft säilitab ja värskendab selle hoidla sageli. Kasutajate külastada Github hoidla saada viimati värskendatud valimi IPython märkmikud.
![Tööruumi loomine][18]

## <a name="upload"></a>Juhis 5: Üles laadida olemasoleva IPython märkmiku kohalikust arvutist IPython märkmiku server

IPython märkmike pakkuda lihtne viis üles laadida olemasoleva märkmiku IPython oma kohaliku masinad IPython märkmiku server on virtuaalmasinates kasutajate jaoks. Pärast seda, kui kasutajad Logige veebibrauseri IPython märkmik, klõpsake **kataloogi** IPython märkmiku laaditakse. Seejärel valige IPython märkmiku .ipynb faili üles laadida kohalikust arvutist **File Explorer**ja lohistage ja kukutage see veebibrauseri IPython märkmiku kaust. Klõpsake nuppu **üles laadida** faili üles laadida .ipynb IPython märkmiku serveriga. Teiste kasutajate saate käivitage see kaudu oma veebibrauseris.

![Tööruumi loomine][22]

![Tööruumi loomine][23]


##<a name="shutdown"></a>Sulgemine ja tühistage eraldada virtuaalse masina kui ei ole kasutusel

Azure'i Virtuaalmasinates võetakse **maksma ainult, mida te kasutate**. Selleks, et teil on pole mille eest tuleb tasuda kasutamisel ei virtual arvuti, peab olema olekus **peatamiseni (Deallocated)** , kui ei ole kasutusel.

> [AZURE.NOTE] Virtuaalse masina sees VM sulgemisel (Windowsi power suvandite abil), VM peatatakse, kuid jääb eraldatud. Saate jätkuvalt toimub tagamiseks alati peatada virtuaalmasinates [Azure klassikaline portaali](http://manage.windowsazure.com/). Te saate peatada helistades **ShutdownRoleOperation** koos "PostShutdownAction" võrdub "StoppedDeallocated" VM PowerShelli kaudu.

Sulgeda ja deallocate virtuaalse masina:

1. [Azure'i klassikaline portaali](http://manage.windowsazure.com/) konto kaudu sisse logida.  

2. Valige vasakul asuvalt navigeerimisribalt **VIRTUAALMASINATES** .

3. Virtuaalmasinates loendis klõpsake virtual arvuti nimi, siis avage **ARMATUURLAUA** leht.

4. Klõpsake lehe allosas nuppu **SULGUMIST**.

![VM sulgemine][15]

Virtuaalse masina deallocated kuid neid ei kustutata. Virtuaalne arvuti uuesti käivitamist võib igal ajal Azure'i klassikaline portaalist.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Oma Azure VM on kasutamiseks valmis: mis saab edasi?

Virtuaalne arvuti on nüüd valmis kasutama oma andmete teadus harjutused. Virtuaalse masina on ka Azure seadme õppimine ja andmete meeskonna teadus protsessi uurimise ja andmete ja muude toimingute koos serveriks IPython märkmiku kasutamiseks valmis.

Järgmiste juhiste juurde andmete meeskonna teadus protsessi on vastendatud [Õppetee](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja võivad sisaldada toiminguid, mis andmete viimine HDInsight, töötlemine ja kuulake seal õ andmete Azure seadme õppe ettevalmistamiseks.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
