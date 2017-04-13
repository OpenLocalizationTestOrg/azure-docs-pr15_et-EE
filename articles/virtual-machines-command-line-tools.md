<properties
    pageTitle="Azure'i CLI käsud Teenusehaldus režiimis | Microsoft Azure'i"
    description="Azure'i käsurea liides (CLI) käsud Teenusehaldus režiimis juurutuste klassikaline juurutamise mudeli haldamine"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure'i CLI käsud Azure'i Teenusehaldus (asm) režiimis

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]Võite ka [Lugege ressursihaldur mudeli kõik käsud](virtual-machines/azure-cli-arm-commands.md), ja CLI abil [migreerida ressursid](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) klassikaline ressursihaldur mudeli.

Selles artiklis antakse süntaks ja Azure CLI käsud saate sagedamini abil saate luua ja hallata Azure ressursse klassikaline juurutamise mudeli suvandid. Need käsud juurde töötavad CLI Azure'i Teenusehaldus (asm) režiimis. See pole täielik viide ja teie CLI versioon võidakse kuvada veidi teistsugused käskude ja parameetrite. 

Alustamiseks, [installige Azure'i CLI](xplat-cli-install.md) ja [Azure tellimuse ühendada](xplat-cli-connect.md).

Praeguse käsu süntaks ja suvandite käsurida, tippige `azure help` või mõne kindla käsu spikri kuvamiseks `azure help [command]`. Leiate ka CLI näited dokumentatsiooni loomise ja haldamise teatud Azure teenused.

Valikulised parameetrid kuvatakse nurksulgudes (nt `[parameter]`). Kõik muud parameetrid on nõutav.

Lisaks käsk kohased valikuliste parameetrite dokumenteerida siin, on kolm valikuliste parameetrite, nagu taotlus ja olekukoodi üksikasjalikud väljundi kuvamiseks kasutatavate. Funktsiooni `-v` parameetri pakub Paljusõnaline väljund, ja `-vv` parameetri üksikasjalikum isegi Paljusõnaline väljund. Funktsiooni `--json` suvand väljundid tulemi töötlemata json-vormingus.

## <a name="setting-asm-mode"></a>Sätte asm režiim

Järgmise käsu abil lubada Azure CLI Teenusehaldus režiimi käsud.

    azure config mode asm

>[AZURE.NOTE] Funktsiooni CLI Azure'i ressursihaldur ja Azure Teenusehaldus režiimi on üksteist välistavad. Mis on ressursse, mis on loodud ühe režiimis ei saa hallata muu režiimi.

## <a name="manage-your-account-information-and-publish-settings"></a>Kontoteabe haldamine ja avaldada sätted
Üks võimalus CLI saate kontoga ühenduse loomiseks on Azure tellimuse teabe abil. (Vt [ühenduse loomine: Azure'i CLI Azure tellimuse](xplat-cli-connect.md) muid võimalusi.) Selle teabe saab Azure klassikaline portaalist avalda sätete fail, nagu on kirjeldatud allpool. Saate importida avalda sätete fail nagu sätte CLI püsivate konfigureerimine kohaliku kasutab järgnevate toimingute jaoks. Soovite importida oma avaldamine sätted üks kord.

**konto allalaadimine [Valikud]**

See käsk käivitab brauseri .publishsettings faili allalaadimiseks Azure klassikaline portaalist.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**konto importimine [Valikud] &lt;fail >**


Selle käsu impordi publishsettings faili või serdi nii, et seda saab kasutada tööriista edaspidi.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Publishsettings fail võib sisaldada rohkem kui ühe tellimuse üksikasjad (st tellimuse nime ja ID). Publishsettings faili importimisel kasutatakse esimese tellimuse vaikimisi kirjeldus. Kui soovite kasutada mõnda muud tellimust, käivitage järgmine käsk:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**konto tühjendada]**

See käsk eemaldab salvestatud publishsettings, mis on imporditud. Kasutage seda käsku, kui olete valmis selles arvutis tööriista abil ja soovite kinnitada, et tööriist ei saa teie konto edaspidi kasutada.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**kontode loend [Valikud]**

Imporditud tellimuste loend

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**konto [määramine] &lt;tellimus&gt;**

Praeguse tellimuse seadmine

###<a name="commands-to-manage-your-affinity-groups"></a>Käskude oma osaleja rühmade haldamiseks

**osaleja-rühma loendist [Valikud]**

See käsk on loetletud teie Azure osaleja rühmad.

Kui rühma virtuaalmasinates ulatub üle mitme füüsilise masinad, saate määrata osaleja rühmad. Osaleja rühma saate määrata, et füüsilise masinad tuleks lähedale üksteisest võimalikult vähendada võrgu latentsus.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**konto osaleja-rühma loomine [Valikud] &lt;nimi&gt;**

See käsk loob ka osaleja rühma

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**konto osaleja-rühma, Kuva [Valikud] &lt;nimi&gt;**

See käsk näitab osaleja rühma üksikasjad

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**osaleja-rühma kustutamine [Valikud] konto &lt;nimi&gt;**

See käsk kustutab määratud osaleja rühma

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Käskude keskkond, mis on teie konto haldamine

**keskkonna loendist [Valikud]**

Loendi konto keskkondadega

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**konto keskkonna kuvamine [Valikud] [keskkonna]**

Konto keskkonna üksikasjade kuvamine

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**konto lisamine keskkonna [Valikud] [keskkonna]**

See käsk lisab keskkonnas kontole

**konto keskkonna määramine [] [keskkonna]**

See käsk määrab konto keskkonnas

**konto kustutamine keskkonna [Valikud] [keskkonna]**

See käsk kustutab määratud keskkonnas konto

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Hallata oma klassikaline virtuaalmasinates käsud
Järgmisel joonisel on esitatud kuidas klassikaline Azure'i virtuaalmasinates majutatakse tootmiskeskkonnas juurutamise on Azure pilveteenuses.

![Azure'i tehniline skeem](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Loo uus** draiv loob bloobimälu (st e:\ skeemi); **Manusta** manustab on juba loodud, kuid manustamata ketta virtuaalse masina.

**VM loomine [Valikud] &lt;DNS-i nimi > &lt;pilt > &lt;kasutajanimi > [parool]**

See käsk loob mõne Azure virtuaalse masina. Vaikimisi luuakse iga virtuaalse masina (vm) oma pilveteenuses. Saate määrata, et virtuaalse masina tuleks lisada olemasoleva pilveteenusesse, kuni - c võimalust, nagu siin dokumenteerida.

Vm käsk, näiteks Azure klassikaline portaali loomine, vaid loob virtuaalmasinates tootmiskeskkonnas juurutamist. Puudub võimalus luua virtuaalse masina pilveteenus lavastus juurutamise keskkonnas. Kui teie tellimus pole Azure storage konto, käsk loob ühe.

Määrake asukoht, kuni--asukoht parameeter, või saate määrata ka osaleja rühma – osaleja-rühma parameetri kaudu. Kui ei, kui teil palutakse sisestada ühte loendist sobiv asukohad.

Sisestatud parool peab olema 8-123 tähemärki ja vastavad parooli keerukuse operatsioonisüsteem, mida kasutate selle virtuaalse masina.

Kui kavatsete kasutada SSH haldamiseks juurutatud Linux virtuaalse masina (nagu tavaliselt), tuleb lubada SSH kaudu -e valik virtuaalse masina loomisel. Ei saa pärast loomist virtuaalse masina SSH lubamiseks.

Windowsi virtuaalmasinates saate lubada RDP hiljem lisades porti 3389 lõpp.

See käsk on toetatud järgmised valikulised parameetrid:

**- c,--ühenduse** luua virtuaalse masina sees on juba loodud juurutamise hosting teenust. Kui see suvand ei kasutata - vmname, luuakse automaatselt uue virtuaalse masina nimi.<br />
**-n,--vm-nime** Määrake virtuaalse masina nimi. See parameeter võtab vaikimisi majutusteenuse teenuse nimi. Kui - vmname pole määratud, kui luuakse uus virtuaalse masina nimi &lt;teenuse nimi >&lt;ID-d >, kus &lt;ID-d > on arv, olemasoleva virtuaalmasinates teenuse Lisaks 1. Näiteks kui selle käsu abil saate lisada virtuaalse masina majutusteenus MyService, mis sisaldab ühes olemasolevad virtuaalse masina, virtuaalse masina uue nimega MyService2.<br />
**-u,--bloobimälu-URL-i** Määrata target bloobimälu salvestusruumi URL, kus luua virtuaalse masina süsteemi ketas. <br />
**-y,--vm suurus** Määrata virtuaalse masina. Sobivad väärtused on: "ExtraSmall", "Väike", "Keskmine", "Suur", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". Vaikeväärtus on "Väike". <br />
**-r** Lisab Windowsi virtuaalse masina RDP Ühenduvus. <br />
**-e,--ssh** Lisab Windowsi virtuaalse masina SSH Ühenduvus. <br />
**-t,--ssh-cert** Saate määrata SSH sert. <br />
**-s** Tellimuse <br />
**-o,--ühenduse** Määratud pilt on ühenduse pilt <br />
**-w** Virtuaalne võrgu nimi <br/>
**-l,--asukoht** määrab asukoha (nt "Põhja keskse USA"). <br />
**-,--osaleja-rühma** määrab osaleja rühma.<br />
**-w,--virtuaalse võrgu nimi** Määrake virtuaalse võrgu, kuhu soovite lisada uue virtuaalse masina. Virtuaalne võrkude saate häälestada ja hallata Azure klassikaline portaali kaudu.<br />
**-b,--alamvõrgu nimed** Määrab alamvõrgu nimed, kelle soovite määrata virtuaalse masina.

Selles näites on MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB platvormi esitatud pilt. Operatsioonisüsteemi piltide kohta leiate lisateavet teemast vm pildi loend.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**VM loomine: &lt;DNS-i nimi > &lt;rolli-fail >**

See käsk loob mõne Azure virtuaalse masina JSON osa failist.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**VM loendi [Valikud]**

See käsk on loetletud Azure'i virtuaalmasinates. Json - suvand määrab tulemit töötlemata JSON-vormingus.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**VM asukohta loendis [Valikud]**

See käsk on loetletud kõik saadaolevad Azure'i konto asukohad.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**VM kuvamine [Valikud] &lt;nimi >**

See käsk on Azure virtuaalse masina detailid. Json - suvand määrab tulemit töötlemata JSON-vormingus.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**VM kustutamine [Valikud] &lt;nimi >**

See käsk kustutab on Azure virtuaalse masina. See käsk Kustuta vaikimisi Azure'i bloobimälu, mille põhjal loodud operatsioonisüsteemi ketta ja andmed kettale. Määrake soovitud bloobimälu ja selle aluseks virtuaalse masina kustutamiseks -b suvand.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**VM [Käivitamissuvandid] &lt;nimi >**

See käsk käivitab on Azure virtuaalse masina.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**taaskäivitage VM [Valikud] &lt;nimi >**

See käsk taaskäivitamist on Azure virtuaalse masina.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**VM sulgumist [Valikud] &lt;nimi >**

See käsk sulgub on Azure virtuaalse masina. -P suvandi abil võimalik määrata, et Arvuta ressursi on välja antud sulgumist.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**VM jäädvustada &lt;vm nime > &lt;siht-image name >**

See käsk salvestab pildi Azure virtuaalse masina.

Virtuaalne arvuti pildi ainult saate jäädvustata, kui virtuaalse masina olek on **peatatud**. Enne jätkamist virtual arvuti sulgeda.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**VM ekspordi [Valikud] &lt;vm nime > &lt;faili tee >**

See käsk Ekspordi faili Azure virtuaalse masina pilt

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Käskude hallata oma Azure virtuaalse masina lõpp-punktid
Järgmisel joonisel on tüüpilised kasutuselevõtu mitu eksemplari klassikaline virtuaalse masina arhitektuur. Selles näites port 3389 oleks avatud iga virtuaalse masina (RDP juurdepääsuks). Iga virtual arvutis, kus kasutatakse laadi koormusetasakaalustusteenuse, et virtuaalse masina liikluse marsruutimiseks on ka sisemise IP-aadress (nt 168.55.11.1). Selle sisemine IP-aadressi saab kasutada ka virtuaalmasinates vahel.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Välise taotlusi virtuaalmasinates läbida laadi koormusetasakaalustusteenuse. Seetõttu ei saa määrata taotlusi kindla virtuaalse masina juurutuste kohta koos mitme virtuaalmasinates suhtes. Koos mitme virtuaalmasinates juurutuste korral peab olema konfigureeritud port kaardistamine virtuaalmasinates (vm port) ja laadi koormusetasakaalustusteenuse (lb-port) vahel.

**VM lõpp-punkti loomiseks &lt;vm nime > &lt;lb-port > [vm port]**

See käsk loob virtuaalse masina endpoint. Samuti võite -u või--luba otsesed-server tagasi, et määrata, kas lubada otsest server tagasi selle näitaja, mis on vaikimisi välja lülitatud.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**vm lõpp-punkti loomine mitme [Valikud] &lt;vm nime > &lt;lb-port > [:&lt;vm-port > [:&lt;Protocol (protokoll) > [:&lt;luba otsesed-server tagasi > [:&lt;lb-set-name > [:&lt;juures-protokolli > [:&lt;juures-port > [:&lt;juures-tee > [:&lt;sisemine lb nimi >]]] {1 -*}**

Saate luua mitme vm lõpp-punktid.

**VM lõpp-punkti kustutamine [Valikud] &lt;vm nime > &lt;lõpp-punkti nimi >**

See käsk on virtuaalse masina lõpp-punkti kustutamine

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**VM lõpp-punkti loendi &lt;vm nime >**

See käsk on loetletud kõik virtuaalse masina lõpp-punktid. Json - suvand määrab tulemit töötlemata JSON-vormingus.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**VM lõpp-punkti update [Valikud] &lt;vm nime > &lt;lõpp-punkti nimi >**

See käsk värskendab vm endpoint uued väärtused järgmiste suvandite abil.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**VM lõpp-punkti kuvamine [Valikud] &lt;vm nime >**

See käsk Pilt vm lõpp-punktid üksikasjad

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Hallata oma Azure virtuaalse masina pilte käsud

Virtuaalse masina pildid on juba konfigureeritud virtuaalmasinates, mida saate kopeeritud leidmise vastavalt vajadusele.

**VM pilt loendi [Valikud]**

See käsk saab loendi virtuaalse masina pilte. On kolme tüüpi pildid: loodud Microsoft, pildid, mis on eesliide "MSFT", pilte, mis on loodud kolmandatele isikutele, mis on tähistatud nimega hankija ja pilte saate luua. Piltide loomiseks saate jäädvustada mõne olemasoleva virtuaalse masina või luua kohandatud .vhd, mis on üles laaditud bloobimälu pilt. Kohandatud .vhd kasutamise kohta leiate lisateavet teemast vm pildi loomine.
Json - suvand määrab tulemit töötlemata JSON-vormingus.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**VM pildi kuvamine [Valikud] &lt;nimi >**

See käsk näitab virtuaalse masina pilt üksikasju.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**VM pilt Kustuta [Valikud] &lt;nimi >**

See käsk kustutab virtuaalse masina pilt.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**VM pilt loomine &lt;nimi > [lähte-tee]**

See käsk loob virtuaalse masina pilt. Oma kohandatud .vhd failid üles Bloobivahemälu salvestusruumi ja seejärel virtuaalse masina pilt luuakse seal. Seejärel saate selle virtuaalse masina pildi luua virtuaalse masina. Asukoht ja OS parameetrid on nõutav.

>[AZURE.NOTE]See käsk toetab praegu ainult fikseeritud .vhd failide saatmisel. Dünaamiliste .vhd faili üleslaadimiseks kasutada [Azure VHD Utiliidid jaoks, avage](https://github.com/Microsoft/azure-vhd-utils-for-go).

Mõned süsteemid kehtestada faililimiitide kohta protsessi deskriptori. Kui see piirang on ületatud, kuvab tööriist faili deskriptori limiit tõrge. Saate käivitada uuesti, kasutades -p käsu &lt;arv > parameetri paralleelselt lisatud suurima arvu vähendamiseks. Vaikimisi paralleelselt lisatud maksimumarv on 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Hallata oma Azure virtuaalse masina andmete ketast käsud

Andmete ketast on .vhd failide bloobimälu, mida saab kasutada virtuaalse masina järgi. Lisateavet selle kohta, kuidas andmeid ketast on juurutatud Bloobivahemälu salvestusruumi leiate Azure'i tehnilise skeemi eespool näidatud.

Käsud manustamise andmete ketast (Azure'i vm ketta manustamine ja azure vm ketta manustamine uus) loogilise üksuse Number (LUN) määramiseks manustatud andmed kettale, vastavalt vajadusele SCSI protokolli järgi. Esimene andmete ketas manustatud virtuaalse masina on määratud LUN 0, järgmise on määratud LUN 1 jne.

Kui olete sellelt andmed kettale Azure'i vm abil lahti käsk, kasutage funktsiooni &lt;lun&gt; parameetri näitamaks, mille ketas lahti.

>[AZURE.NOTE] Olete tuleks alati sellelt andmete ketast vastupidises järjekorras, alustades suurima numbriga LUN, mis on määratud. Linux SCSI kiht ei toeta saidimääratlusest madalama numbriga LUN samas suurema nummerdatud LUN on lisatud. Näiteks olete peaks sellelt LUN 0, kui LUN 1 on lisatud.

**VM ketta Kuva [Valikud] &lt;nimi >**

See käsk on Azure ketta detailid.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**VM ketta loendi [Valikud] [vm nimi]**

See käsk loendite Azure ketast või ketast, mis on seotud määratud virtuaalse masina. Kui see on virtuaalse masina nimi parameetri käivitada, tagastab see kõik ketast virtual masina külge. LUN 1 on loodud virtuaalse masina ja loetletud ketast on lisatud eraldi.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Selle käsu ilma virtuaalse masina nimi parameetri tagastab kõik ketast.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**VM ketta kustutamine [Valikud] &lt;nimi >**

See käsk kustutab on Azure ketta isikliku hoidla. Ketas peab virtuaalse masina eemaldada see enne kustutamist.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**VM ketta loomine &lt;nimi > [lähte-tee]**

See käsk laadib ja registreerib on Azure ketas. --bloobimälu-URL-i,--asukoha või--osaleja-peab olema määratud. Kui kasutate selle käsu abil [lähte-tee], määratud .vhd faili üleslaadimisel ja pilt on loodud. Seejärel saate lisada selle pildi virtuaalse masina vm kettapuhastusriista abil lisada.

Mõned süsteemid kehtestada faililimiitide kohta protsessi deskriptori. Kui see piirang on ületatud, kuvab tööriist faili deskriptori limiit tõrge. Saate käivitada uuesti, kasutades -p käsu &lt;arv > parameetri paralleelselt lisatud suurima arvu vähendamiseks. Vaikimisi paralleelselt lisatud maksimumarv on 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**VM ketta üles [Valikud] &lt;Allika tee > &lt;bloobimälu-URL-i > &lt;salvestusruumi kontovõti >**

Selle käsu abil saate üles laadida vm kettal

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**VM ketta manustamine &lt;vm nime > &lt;ketta-image name >**

See käsk manustab olemasolevat ketast rakenduses bloobimälu mõne olemasoleva virtuaalse masina juurutatud mõnda pilveteenusesse.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**VM ketta manustada uus &lt;vm nime > &lt;suurus –-gb > [bloobimälu url]**

See käsk manustab on Azure virtuaalse masina andmed kettale. Selles näites on 20 uue ketta, gigabaiti lisatakse. Soovi korral saate bloobimälu URL-i viimase argumendina määrata target bloobimälu loomiseks. Kui te ei määra bloobimälu URL-i, luuakse automaatselt bloobimälu objekt.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**VM ketta lahti &lt;vm nime > &lt;lun >**

See käsk rebib Azure'i virtuaalarvuti lisatud andmed kettal. &lt;LUN > tuvastab tuleb ketas. Seotud kettal, enne kui saate selle eemaldada ketast loendi saamiseks kasutage vm ketta-loendi &lt;vm nime >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Hallata oma Azure pilveteenused käsud

Azure'i pilveteenustega on rakenduste ja teenuste majutatud web rollid ja töötaja rollid. Järgmised käsud saab hallata Azure pilveteenustega.

**teenuse loomine [Valikud] &lt;teenuse nimi >**

See käsk loob pilveteenus

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**teenuse kuvamine [Valikud] &lt;teenuse nimi >**

See käsk näitab Azure pilveteenuse teenuse üksikasjad

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**teenuste loend [Valikud]**

See käsk on loetletud Azure pilveteenustega.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**teenuse kustutamine [Valikud] &lt;nimi >**

See käsk kustutab on Azure pilveteenuses.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Kustutamise jõustamine, kasutage funktsiooni `-q` parameeter.


## <a name="commands-to-manage-your-azure-certificates"></a>Käskude Azure sertide haldamine

Azure'i teenus serdid on SSL-sertide Azure kontoga ühendatud. Azure'i serdid kohta leiate lisateavet teemast [Haldamine serdid](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**teenuste cert loend [Valikud]**

See käsk on loetletud Azure serdid.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**teenuse cert loomine &lt;dns-i eesliide > &lt;fail > [parool]**

See käsk laadib sert. Parooli küsimuse sertide, mis on kaitstud parooliga tühjaks jätta.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**teenuse cert Kustuta [Valikud] &lt;sõrmejälje >**

See käsk kustutab sert.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Käskude web rakenduste haldamine

Azure'i web app on web konfiguratsioon puuetega inimestele juurdepääsetavate URI järgi. Veebirakenduste majutatakse virtuaalmasinates, kuid teil pole vaja mõelda loomine ja juurutamine virtuaalse masina ise. Neid andmeid töödeldakse teile Azure.

**saidi loendi [Valikud]**

See käsk on loetletud teie veebirakenduste.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**saidi määramine [] [nimi]**

See käsk seab configuration oma veebirakenduse [nimi]

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**saidi deploymentscript [Valikud]**

See käsk loob kohandatud juurutamise skripti

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**saidi loomine [Valikud] [nimi]**

See käsk loob web app ja kohalikku kausta.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] Saidi nimi peab olema kordumatu. Te ei saa luua saidi olemasoleval saidil sama DNS-i nimi.

**saidi Sirvi [Valikud] [nimi]**

See käsk avab oma veebirakenduse brauseris.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**saidi kuvada [suvandeid] [nimi]**

See käsk näitab üksikasjad web app.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**saidi kustutamine [Valikud] [nimi]**

See käsk kustutab web appi.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **saidi Vaheta [Valikud] [nimi]**

See käsk vahetuslepingud pesast web app.

See käsk toetab järgmine täiendav valik.

**k - või **--vaikne **: Küsi kinnituse. Kasutage seda suvandit automatiseeritud skriptide.


**saidi Käivitamissuvandid [] [nimi]**

See käsk käivitab web appi.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**saidi Peata [Valikud] [nimi]**

See käsk lõpetab web appi.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**saidi taaskäivitage [Valikud] [nimi]**

See käsk lõpetab ja käivitab määratud web appi.

See käsk toetab järgmine täiendav valik.

**--pesa** &lt;pesa >: nime pesa taaskäivitama.


**saidiloendi asukohta [Valikud]**

See käsk on loetletud teie web appi asukohad.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Käskude web app rakenduse sätete haldamine

**saidiloendi appsetting [Valikud] [nimi]**

See käsk on loetletud rakenduse säte lisatakse web appi.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**saidi appsetting lisamine [Valikud] &lt;keyvaluepair > [nimi]**

See käsk lisab mõne sätte rakenduse oma veebirakenduse võtmeväärtuse paaris.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**saidi appsetting kustutamine [Valikud] &lt;klahv > [nimi]**

See käsk kustutab säte määratud rakenduse web Appist.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**saidi appsetting kuvamine [Valikud] &lt;klahv > [nimi]**

See käsk kuvab säte määratud rakenduse üksikasjad

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Käskude haldamiseks sertide web app

**saidiloendi cert [Valikud] [nimi]**

See käsk kuvab web appi certs loendit.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**saidi cert lisamine [Valikud] &lt;sert tee > [nimi]**

**saidi cert kustutamine [Valikud] &lt;sõrmejälje > [nimi]**

**saidi cert Kuva [Valikud] &lt;sõrmejälje > [nimi]**

See käsk näitab cert üksikasjad

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Käskude web appi ühendusstringi haldamine

**saidiloendi connectionstring [Valikud] [nimi]**

**saidi connectionstring lisamine [Valikud] &lt;otsimist > &lt;väärtus > &lt;tüüp > [nimi]**

**saidi connectionstring kustutamine [Valikud] &lt;otsimist > [nimi]**

**saidi connectionstring kuvamine [Valikud] &lt;otsimist > [nimi]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Käskude web appi vaikimisi dokumentide haldamine

**saidiloendi defaultdocument [Valikud] [nimi]**

**saidi defaultdocument lisamine [Valikud] &lt;dokument > [nimi]**

**saidi defaultdocument kustutamine [Valikud] &lt;dokument > [nimi]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Käskude oma web rakendusejuurutuste haldamine

**saidiloendi juurutamise [Valikud] [nimi]**

**saidi juurutamise kuvamine [Valikud] &lt;commitId > [nimi]**

**saidi juurutamise ümberkorraldamine [Valikud] &lt;commitId > [nimi]**

**saidi juurutamise github [Valikud] [nimi]**

**saidi juurutamise kasutaja määramine [] [kasutajanimi] [pass]**

###<a name="commands-to-manage-your-web-app-domains"></a>Käskude oma web appi domeenide haldamiseks.

**saidi domeeni loendi [Valikud] [nimi]**

**saidi domeeni lisamine [Valikud] &lt;dn > [nimi]**

**saidi domeeni kustutamine [Valikud] &lt;dn > [nimi]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Käskude oma web appi sündmuseohjuri vastenduste haldamine

**sündmuseohjuri saidiloendi [Valikud] [nimi]**

**saidi sündmuseohjuri lisamine [Valikud] &lt;laiend > &lt;protsessor > [nimi]**

**saidi sündmuseohjuri kustutamine [Valikud] &lt;laiend > [nimi]**

###<a name="commands-to-manage-your-web-jobs"></a>Kui soovite hallata oma Web käsud

**saidiloendi töö [Valikud] [nimi]**

See käsk loendis web tööd jaotises web appi.

See käsk toetab järgmised täiendavad suvandid.

+ **--töö-tüüpi** &lt;töö tüüp >: valikuline. Funktsiooni webjob tüüp. Sobiv väärtus on "käivitatud" või "pidev". Vaikimisi tagastada webjobs igat tüüpi.
+ **--pesa** &lt;pesa >: nime pesa taaskäivitama.

**saidi töö Kuva [Valikud] &lt;jobName > &lt;jobType > [nimi]**

See käsk näitab teatud web töö üksikasju.

See käsk toetab järgmised täiendavad suvandid.

+ **--töö nimi** &lt;töö nimi >: nõutav. Selle webjob nimi.
+ **--töö-tüüpi** &lt;töö tüüp >: nõutav. Funktsiooni webjob tüüp. Sobiv väärtus on "käivitatud" või "pidev".
+ **--pesa** &lt;pesa >: nime pesa taaskäivitama.

**saidi töö kustutamine [Valikud] &lt;jobName > &lt;jobType > [nimi]**

See käsk kustutab määratud web töö.

See käsk toetab järgmised täiendavad suvandid.

+ **--töö nimi** &lt;töö nimi > nõutav. Selle webjob nimi.
+ **--töö-tüüpi** &lt;töö tüüp > nõutav. Funktsiooni webjob tüüp. Sobiv väärtus on "käivitatud" või "pidev".
+ **k -** või **--vaiksesse**: Küsi kinnituse. Kasutage seda suvandit automatiseeritud skriptide.
+ **--pesa** &lt;pesa >: nime pesa taaskäivitama.

**saidi töö üles [Valikud] &lt;jobName > &lt;jobType > <jobFile> [nimi]**

See käsk kustutab määratud web töö.

See käsk toetab järgmised täiendavad suvandid.

+ **--töö nimi** &lt;töö nimi >: nõutav. Selle webjob nimi.
+ **--töö-tüüpi** &lt;töö tüüp >: nõutav. Funktsiooni webjob tüüp. Sobiv väärtus on "käivitatud" või "pidev".
+ **--töö faili** &lt;töö-fail >: nõutav. Töö faili.
+ **--pesa** &lt;pesa >: nime pesa taaskäivitama.

**saidi töö algus [Valikud] &lt;jobName > &lt;jobType > [nimi]**

See käsk käivitab määratud web töö.

See käsk toetab järgmised täiendavad suvandid.

+ **--töö nimi** &lt;töö nimi >: nõutav. Selle webjob nimi.
+ **--töö-tüüpi** &lt;töö tüüp >: nõutav. Funktsiooni webjob tüüp. Sobiv väärtus on "käivitatud" või "pidev".
+ **--pesa** &lt;pesa >: nime pesa taaskäivitama.

**saidi töö Peata [Valikud] &lt;jobName > &lt;jobType > [nimi]**

See käsk lõpetab määratud web töö. Ainult pidev töid saate peatada.

See käsk toetab järgmised täiendavad suvandid.

+ **--töö nimi** &lt;töö nimi >: nõutav. Selle webjob nimi.
+ **--pesa** &lt;pesa >: nime pesa taaskäivitama.

###<a name="commands-to-manage-your-web-jobs-history"></a>Käskude Web töö ajaloo haldamine

**töö saidi ajalooloend [Valikud] [jobName] [nimi]**

See käsk kuvab käivitatakse määratud web töö ajalugu.

See käsk toetab järgmised täiendavad suvandid.

+ **--töö nimi** &lt;töö nimi >: nõutav. Selle webjob nimi.
+ **--pesa** &lt;pesa >: nime pesa taaskäivitama.

**saidi töö ajaloo kuvamine [Valikud] [jobName] [runId] [nimi]**

See käsk saab käivitada määratud web töö töö üksikasjad.

See käsk toetab järgmised täiendavad suvandid.

+ **--töö nimi** &lt;töö nimi >: nõutav. Selle webjob nimi.
+ **--Käivita-id** &lt;Käivita-ID-d >: valikuline. Käivita ajaloo ID-d. Kui pole määratud, kuvatakse Viimane Käivita.
+ **--pesa** &lt;pesa >: nime pesa taaskäivitama.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Hallata oma web appi diagnostika käsud

**saidi Logi allalaadimisvõimalused [] [nimi]**

Laadige alla ZIP-faili, mis sisaldab web appi diagnostika.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**saidi Logi saba [Valikud] [nimi]**

See käsk loob teie terminalis log streaming teenusega.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**saidi Logi määramine [] [nimi]**

See käsk konfigureerib oma veebirakenduse diagnostika suvandid.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Hallata oma web appi hoidlate käsud

**saidi hoidla haru [Valikud] &lt;haru > [nimi]**

**saidi hoidla kustutamine [Valikud] [nimi]**

**saidi hoidla sünkroonimine [Valikud] [nimi]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Käskude web appi skaleerimist haldamine

**saidi skaala režiimi [Valikud] &lt;režiimi > [nimi]**

**saidi skaala eksemplarid [Valikud] &lt;eksemplari > [nimi]**


## <a name="commands-to-manage-azure-mobile-services"></a>Käskude haldamiseks Azure Mobile teenused

Azure'i Mobile teenuste koondab Azure teenuseid, mis võimaldavad kirjutamata võimaluste rakenduste kogum. Järgmiste kategooriate jagunevad Mobile teenuste käsud:

+ [Käskude mobiilsideteenuse eksemplaride haldamine](#Mobile_Services)
+ [Käskude mobiilsideteenuse konfiguratsiooni haldamine](#Mobile_Configuration)
+ [Käskude mobiilsideteenuse tabelite haldamine](#Mobile_Tables)
+ [Käskude mobiilsideteenuse skriptide haldamine](#Mobile_Scripts)
+ [Käskude ajastatud tööd haldamine](#Mobile_Jobs)
+ [Käskude mastaapimiseks mobiilsideteenuse ülevaade](#Mobile_Scale)

Enamik Mobile teenuste käske rakendada järgmistest suvanditest:

+ **-h** või **--aitab**: Kuva väljund kasutamise kohta.
+ **-s `<id>` ** või **--tellimuse `<id>` **: kasutada teatud tellimuste korral määratud `<id>`.
+ **-v** või **--Paljusõnaline**: kirjutamine Paljusõnaline väljund.
+ **json-**: kirjutamine JSON väljundi.

### <a name="Mobile_Services"></a>Käskude mobiilsideteenuse eksemplaride haldamine

**mobiilse asukohad [Valikud]**

See käsk on loetletud geograafiliste asukohtade Mobile Services ei toeta.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**Mobile loomine [Valikud] [teenuse nimi] [sqlAdminUsername] [sqlAdminPassword]**

See käsk loob mobiilsideteenuse ülevaade koos ja SQL-andmebaasi server.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

See käsk toetab järgmised täiendavad suvandid.

+ **- r `<sqlServer>` ** või **--sqlServer `<sqlServer>` **: kasutada olemasoleva SQL-andmebaasi serveri, määratud `<sqlServer>`.
+ **-d `<sqlDb>` ** või **--sqlDb `<sqlDb>` **: kasutada olemasoleva SQL-andmebaasi, määratud `<sqlDb>`.
+ **-l `<location>` ** või **--asukoht `<location>` **: loomine teenuse teatud asukohta, mis on määratud `<location>`. Käivitage azure mobiilsideseadmete asukohad asukoht saadaolevate asukohtade saada.
+ **--sqlLocation `<location>` **: loomine SQL serveri kindla `<location>`; vaikesätted mobiilsideteenuse ülevaade asukohta.

**Mobile kustutamine [Valikud] [teenuse nimi]**

See käsk kustutab mobiilsideteenuse ülevaade koos oma SQL-andmebaasi ja serveri.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

See käsk toetab järgmised täiendavad suvandid.

+ **-d** või **--deleteData**: kõik andmed kustutada selle mobiilsideteenuse andmebaasist.
+ **-** või **--deleteAll**: SQL-andmebaasi server ja kustutada.
+ **k -** või **--vaiksesse**: Küsi kinnituse. Kasutage seda suvandit automatiseeritud skriptide.

**mobiilsideseadmete loendi [Valikud]**

See käsk loendite mobiilse teenuste.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**mobiilsideseadmete kuvamine [Valikud] [teenuse nimi]**

See käsk kuvab üksikasjade mobiilsideteenuse ülevaade.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**Mobile taaskäivitage [Valikud] [teenuse nimi]**

See käsk taaskäivitamist mobiilsideteenuse eksemplari.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**mobiilse log [Valikud] [teenuse nimi]**

See käsk tagastab mobiilsideteenuse logid, filtreerides välja kõigi Logi, kuid `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

See käsk toetab järgmised täiendavad suvandid.

+ **- r `<query>` ** või **--päringu `<query>` **: käivitab päringu määratud logifaili.
+ **-t `<type>` ** või **--tüüp `<type>` **: tagastatud logid filtreerimisaluseks kirje `<type>`, mis võib olla `information`, `warning`, või `error`.
+ **-k `<skip>` ** või **--vahele jätta `<skip>` **: ignoreerib määratud ridade arvu `<skip>`.
+ **-p `<top>` ** või **--ülemine `<top>` **: annab vastuseks määratud arvu ridade, määratud `<top>`.

> [AZURE.NOTE] Funktsiooni **--päringu** parameeter alistab **--Tüüp**, **--vahele jätta**, ja **--ülemine**.

**mobiilse taastamine [Valikud] [unhealthyservicename] [healthyservicename]**

See käsk taastab on vigane mobiilsideteenuse teisaldades selle terve mobiilsideteenuse ülevaade teises regioonis.

See käsk toetab järgmine täiendav valik.

**k -** või **--vaiksesse**: tõkestamine taastamise Küsi.

**mobiilse klahvi taastada [Valikud] [teenuse nimi] [tüüp]**

See käsk taastab mobiilsideteenuse kiirmenüü klahvi.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Võtme tüübid on `master` ja `application`.

> [AZURE.NOTE] Kui te taastada klahvid, võib ei pääse juurde teie mobiiliteenuse klientides vana võti. Kui te taastada kiirmenüü klahvi, peate värskendama oma rakenduse uue väärtusega.

**mobiilse klahvi määramine [] [teenuse nimi] [tüüp] [väärtus]**

See käsk määrab mobiilsideteenuse võti kindla väärtuse.


### <a name="Mobile_Configuration"></a>Käskude mobiilsideteenuse konfiguratsiooni haldamine

**mobiilse otsingukonfiguratsioonide loend [Valikud] [teenuse nimi]**

See käsk on loetletud mobiiliteenuse suvandite konfigureerimine.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**mobiilse config saada [Valikud] [teenuse nimi] [võti]**

See käsk saab teatud konfigureerimist mobiil sel juhul dünaamiline skeemi.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**mobiilse config määramine [] [teenuse nimi] [võti] [väärtus]**

See käsk määrab teatud konfigureerimist mobiil sel juhul dünaamiline skeemi.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Käskude mobiilsideteenuse tabelite haldamine

**mobiilse tabelloend [Valikud] [teenuse nimi]**

See käsk on loetletud teie mobiiliteenuse kõik tabelid.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**mobiilse tabeli kuvamine [Valikud] [teenuse nimi] [tablename]**

See käsk detailid. annab vastuseks kindla tabeli.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**mobiilse tabeli loomine [Valikud] [teenuse nimi] [tablename]**

See käsk loob tabeli.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

See käsk toetab järgmine täiendav valik.

+ **-p `&lt;permissions>` ** või **--õigused `&lt;permissions>` **: komaga eraldatud loend `<operation>` = `<permission>` paarideks, kus `<operation>` on `insert`, `read`, `update`, või `delete` ja `&lt;permissions>` on `public`, `application` (vaikeväärtus) `user`, või `admin`.

**mobiilse andmete lugemine [Valikud] [teenuse nimi] [tablename] [päringu]**

See käsk loeb andmed tabelisse.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

See käsk toetab järgmised täiendavad suvandid.

+ **-k `<skip>` ** või **--vahele jätta `<skip>` **: ignoreerib määratud ridade arvu `<skip>`.
+ **-t `<top>` ** või **--ülemine `<top>` **: annab vastuseks määratud arvu ridade, määratud `<top>`.
+ **-l** või **--loendi**: tagastab andmed loendi vormingus.

**mobiilse tabeli värskendamine [Valikud] [teenuse nimi] [tablename]**

See käsk muudab tabeli kustutamise õigus ainult administraatorid.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

See käsk toetab järgmised täiendavad suvandid.

+ **-p `&lt;permissions>` ** või **--õigused `&lt;permissions>` **: komaga eraldatud loend `<operation>` = `<permission>` paari, kus `<operation>` on `insert`, `read`, `update`, või `delete` ja `&lt;permissions>` on `public`, `application` (vaikeväärtus) `user`, või `admin`.
+ **--deleteColumn `<columns>` **: komaga eraldatud loend veergude kustutamiseks nimega `<columns>`.
+ **k -** või **--vaiksesse**: kustutab veergude kinnitust küsimata.
+ **--addIndex `<columns>` **: komaga eraldatud loend veergude registrisse kaasata.
+ **--deleteIndex `<columns>` **: komaga eraldatud loend veergude välistamine indeks.

**mobiilse tabeli kustutamine [Valikud] [teenuse nimi] [tablename]**

See käsk kustutab tabeli.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Määrake – q parameetri ilma kinnituse tabeli kustutamiseks. Tehke seda blokeerimine automatiseerimine skriptide vältimiseks.

**mobiilside kärpige [Valikud] [teenuse nimi] [tablename]**

See käsk eemaldab tabelist kõik read andmeid.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Käskude skriptide haldamine

Selles jaotises käskude abil haldamine server skripte, mis kuuluvad mobiilsideteenuse ülevaade. Lisateabe saamiseks vt [serveri skripte Mobile teenuste töö](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**mobiilse skripti loendi [Valikud] [teenuse nimi]**

See käsk on loetletud registreeritud skriptide, sh tabeli nii ajasti skriptide.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**mobiilse skripti allalaadimine [Valikud] [teenuse nimi] [scriptname]**

See käsk laadib Lisa skript TodoItem tabeli fail nimega `todoitem.insert.js` klõpsake soovitud `table` alamkausta.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

See käsk toetab järgmised täiendavad suvandid.

+ **-p `<path>` ** või **--tee `<path>` **: faili salvestamiseks skripti, kus praegust töötavat kausta on vaikimisi asukoht.
+ **-f `<file>` ** või **-faili `<file>` **: faili salvestamiseks skripti nimi.
+ **-o** või **--alistada**: kirjutada olemasolev fail.
+ **-c** või **--konsooli**: kirjutada skripti faili asemel konsooli.

**mobiilse skripti üles [Valikud] [teenuse nimi] [scriptname]**

See käsk laadib skripti nimega `todoitem.insert.js` kaudu soovitud `table` alamkausta.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

Faili nimi peab koostatud tabeli ja toiming nimesid. Peavad asuma tabeli alamkausta asukohta, kus käivitatakse käsk suhtes. Saate kasutada ka funktsiooni **-f `<file>` ** või **--faili `<file>` ** parameeter määrata erinevad faili nimi ja skripti registreerida sisaldava faili tee.


**mobiilse script kustutamine [Valikud] [teenuse nimi] [scriptname]**

See käsk Lisa olemasolev skripti eemaldab tabelist TodoItem.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Käskude ajastatud tööd haldamine

Selles jaotises käsud on abil saate hallata kuuluvad mobiiliteenuse ajastatud tööd. Lisateavet leiate teemast [tööde plaanimine](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx).

**Mobiil, töö loendi [Valikud] [teenuse nimi]**

See käsk on loetletud ajastatud tööd.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**Mobiil, töö loomine [Valikud] [teenuse nimi] [jobname]**

See käsk loob nimega tööd `getUpdates` mis on ajastatud käivitamiseks tunnis.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

See käsk toetab järgmised täiendavad suvandid.

+ **-i `<number>` ** või **--intervall `<number>` **: töö intervall, täisarv. Vaikeväärtus on `15`.
+ **-u `<unit>` ** või **--intervalUnit `<unit>` **: ühiku _intervall_, mis võib olla üks järgmistest väärtustest:
    + **minutid** (vaikeväärtus)
    + **Hour**
    + **päeva**
    + **kuu**
    + **ükski** (vajadusel töökohta)
+ **-t `<time>`** **--startTime `<time>` ** Algusaja esimese skripti, ISO-vormingus. Vaikeväärtus on `now`.

> [AZURE.NOTE] Uute töökohtade on keelatud olekusse luua, kuna skripti endiselt tuleb üles laadida. **Mobiilse skripti üleslaadimine** käsu abil saate üles laadida script ja käsk **mobiilse töö värskendada** , et töö.

**Mobiil, töö update [Valikud] [teenuse nimi] [jobname]**

Järgmine käsk võimaldab puuetega `getUpdates` töö.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

See käsk toetab järgmised täiendavad suvandid.

+ **-i `<number>` ** või **--intervall `<number>` **: töö intervall, täisarv. Vaikeväärtus on `15`.
+ **-u `<unit>` ** või **--intervalUnit `<unit>` **: ühiku _intervalli_, mis võib olla üks järgmistest väärtustest:
    + **minutid** (vaikeväärtus)
    + **Hour**
    + **päeva**
    + **kuu**
    + **ükski** (vajadusel töökohta)
+ **-t `<time>`** **--startTime `<time>` ** Algusaja esimese skripti, ISO-vormingus. Vaikeväärtus on `now`.
+ **- `<status>` ** või **--oleku `<status>` **: töö olek, mis võib olla kas `enabled` või `disabled`.

**Mobiil, töö kustutamine [Valikud] [teenuse nimi] [jobname]**

See käsk eemaldab TodoList serveri getUpdates ajastatud tööd.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Töö kustutamisel kustutatakse ka üleslaaditud skripti.

### <a name="Mobile_Scale"></a>Käskude mastaapimiseks mobiilsideteenuse ülevaade

Selles jaotises käskude abil mastaapimiseks mobiilsideteenuse ülevaade. Lisateavet leiate teemast [skaleerimist mobiilsideteenuse ülevaade](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx).

**mobiilse skaala kuvamine [Valikud] [teenuse nimi]**

See käsk kuvab skaala teavet, sh praeguse Arvuta režiimi ja eksemplaride arv.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**[suvandite] [teenuse nimi] mobiilsideseadmete skaala muutmine**

See käsk muudab mobiilsideteenuse ülevaade skaala tasuta premium režiim.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

See käsk toetab järgmised täiendavad suvandid.

+ **- c `<mode>` ** või **--computeMode `<mode>` **: Arvuta režiimi peavad olema kas `Free` või `Reserved`.
+ **-i `<count>` ** või **--numberOfInstances `<count>` **: kasutada, kui reserveeritud režiimis eksemplaride arv.

> [AZURE.NOTE] Kui seate Arvuta režiimi `Reserved`, kõigi mobiilsideseadmete teenuste piirkonna käivitada premium režiimis.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Käskude teie mobiiliteenuse eelvaate funktsioonide lubamine

**Mobile Preview 's loendi [Valikud] [teenuse nimi]**

See käsk kuvab eelvaate funktsioonide saadaval määratud teenuse ja kas need on lubatud.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**Mobile Preview 's lubamine [Valikud] [teenuse nimi] [featurename]**

See käsk võimaldab mobiilsideseadmete teenuse määratud Eelvaatefunktsioon. Kui lubatud, ei saa eelvaade funktsioonid olla keelatud mobiilsideteenuse ülevaade.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Käsud mobiilsideteenuse API-de haldamine

**mobiilse api loendi [Valikud] [teenuse nimi]**

See käsk kuvab loendi mobiilsideseadmete teenuse kohandatud API-de jaoks teie mobiiliteenuse loodud.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**mobiilse api loomine [Valikud] [teenuse nimi] [apiname]**

Loob mobiilsideteenuse kohandatud API

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

See käsk toetab järgmine täiendav valik.

**-p** või **--õigused** &lt;õigused >: komaga eraldatud loendi &lt;meetod > =&lt;õigus > paari.

**mobiilse api update [Valikud] [teenuse nimi] [apiname]**

See käsk värskendab määratud mobiilsideteenuse kohandatud API.

See käsk toetab järgmine täiendav valik.

See käsk toetab järgmised täiendavad suvandid.

+ **-p** või **--õigused** &lt;õigused >: komaga eraldatud loendi &lt;meetod > =&lt;õigus > paari.
+ **-f** või **--jõustada**: alistab kõik kohandatud õigused metaandmete failis tehtud muudatusi.

**mobiilse api kustutamine [Valikud] [teenuse nimi] [apiname]**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

See käsk kustutab määratud mobiilsideteenuse kohandatud API.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Käskude mobiilirakenduse rakenduse sätete haldamine

**mobiilse appsetting loendi [Valikud] [teenuse nimi]**

See käsk kuvab määratud teenuse mobiilirakenduse rakenduse sätteid.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**mobiilse appsetting lisamine [Valikud] [teenuse nimi] [nimi] [väärtus]**

See käsk lisab kohandatud rakendus säte oma mobiil.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**mobiilse appsetting kustutamine [Valikud] [teenuse nimi] [nimi]**

See käsk eemaldab määratud rakendus oma mobiilsideteenuse sätet.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**mobiilse appsetting kuvamine [Valikud] [teenuse nimi] [nimi]**

See käsk eemaldab määratud rakendus oma mobiilsideteenuse sätet.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Tööriista kohalike sätete haldamine

Kohalike sätete on teie tellimuse ID ja vaikimisi Salvestusruumikonto nimi.

**otsingukonfiguratsioonide loend [Valikud]**

See käsk kuvab seadistusi.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**config [määramine] &lt;nimi&gt;,&lt;väärtus&gt;**

See käsk muudab seadistuse.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Käskude teenuse siini haldamine

Need käsud abil saate hallata teenuse siini kontot

**SB nimeruum sisse [Valikud] &lt;nimi >**

Kontrollige, kas teenuse siini nimeruumi on seadusega lubatud ja saadaval.

**SB nimeruumi loomine &lt;nimi > &lt;asukoht >**

Loob teenuse siini nimeruum.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**SB nimeruum kustutamine &lt;nimi >**

Nimeruumi eemaldada.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**SB nimeruum loend**

Loetle kõik nimeruumid konto jaoks.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**SB nimeruum loendis asukoht**

Kuva kõik saadaolevad nimeruum asukohtade loendit.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**SB nimeruum Kuva &lt;nimi >**

Teatud nimeruumi üksikasjade kuvamine

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**SB nimeruum kinnitamine &lt;nimi >**

Kontrollige, kas nimeruumi on saadaval.

## <a name="commands-to-manage-your-storage-objects"></a>Käskude hallata oma salvestusruumi objektid

###<a name="commands-to-manage-your-storage-accounts"></a>Käskude hallata oma salvestusruumi kontod

**salvestusruumi kontode loend [Valikud]**

See käsk kuvab teie tellimuse salvestusruumi kontod.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**salvestusruumi konto kuvamine [Valikud]<name>**

See käsk kuvab teavet määratud salvestusruumi konto, sh URI ja konto atribuudid.

**salvestusruumi konto loomine [Valikud]<name>**

See käsk loob salvestusruumi konto põhjal esitatud suvandid.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

See käsk toetab järgmised täiendavad suvandid.

+ **-e** või **--sildi** &lt;silt >: sildi salvestusruumi konto.
+ **-d** või **--Kirjeldus** &lt;kirjeldus >: kirjeldus salvestusruumi konto.
+ **-l** või **--asukoht** &lt;nimi >: geograafilised regiooni salvestusruumi konto loomiseks.
+ **-** või **--osaleja-rühma** &lt;nimi >: osaleja rühma, millega soovite seostada salvestusruumi konto. 
+ **--Tüüp**: näitab, et luua konto tüüp: kas Standard salvestusruumi koondamise suvand (LRS/ZRS/GRS/RAGRS) või Premium salvestusruumi (PLRS).

**salvestusruumi konto [määramine]<name>**

See käsk värskendab määratud salvestusruumi konto.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

See käsk toetab järgmised täiendavad suvandid.

+ **-e** või **--sildi** &lt;silt >: sildi salvestusruumi konto.
+ **-d** või **--Kirjeldus** &lt;kirjeldus >: kirjeldus salvestusruumi konto.
+ **-l** või **--asukoht** &lt;nimi >: geograafilised regiooni salvestusruumi konto loomiseks.
+ **--Tüüp**: näitab uue konto tüüp: kas Standard salvestusruumi koondamise suvand (LRS/ZRS/GRS/RAGRS) või Premium salvestusruumi (PLRS).

**salvestusruumi konto kustutamine [Valikud]<name>**

See käsk kustutab määratud salvestusruumi konto.

See käsk toetab järgmine täiendav valik.

**k -** või **--vaiksesse**: Küsi kinnituse. Kasutage seda suvandit automatiseeritud skriptide.

###<a name="commands-to-manage-your-storage-account-keys"></a>Hallata oma salvestusruumi konto klahvid käsud

**salvestusruumi konto klahvid loendi [suvandid]<name>**

See käsk on loetletud määratud salvestusruumi konto esmaseid ja teiseseid võtmed.

**salvestusruumi konto klahvid pikendada [Valikud]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Hallata oma salvestusruumi container käsud

**salvestusruumi ümbris loend [Valikud] [eesliite]**

See käsk kuvab määratud salvestusruumi konto salvestusruumi ümbris loend. Salvestusruumi konto on määratud ühendusstring või salvestusruumi konto nimi ja konto võti.

See käsk toetab järgmised täiendavad suvandid.

+ **-p** või **-eesliite** &lt;eesliite >: salvestusruumi container nime eesliide.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi.
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti.
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring.
+ **--silumine**: töötab salvestusruumi käsk silumine režiimis.

**salvestusruumi container Kuva [Valikud] [container]**
**salvestusruumi container loomine [Valikud] [container]**

See käsk loob salvestusruumi container määratud salvestusruumi konto. Salvestusruumi konto on määratud ühendusstring või salvestusruumi konto nimi ja konto võti.

See käsk toetab järgmised täiendavad suvandid.

+ **--container** &lt;container >: salvestusruumi container loomiseks nime.
+ **-p** või **-eesliite** &lt;eesliite >: salvestusruumi container nime eesliide.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring
+ **--silumine**: töötab salvestusruumi käsk silumine režiimis.

**container salvestusruumi kustutamine [Valikud] [container]**

See käsk kustutab määratud salvestusruumi ümbrises. Salvestusruumi konto on määratud ühendusstring või salvestusruumi konto nimi ja konto võti.

See käsk toetab järgmised täiendavad suvandid.

+ **--container** &lt;container >: salvestusruumi container loomiseks nime.
+ **-p** või **-eesliite** &lt;eesliite >: salvestusruumi container nime eesliide.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi.
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti.
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring.
+ **--silumine**: töötab salvestusruumi käsk silumine režiimis.

**salvestusruumi container määramine [] [container]**

See käsk seab pääsuloendi salvestusruumi ümbrises. Salvestusruumi konto on määratud ühendusstring või salvestusruumi konto nimi ja konto võti.

See käsk toetab järgmised täiendavad suvandid.

+ **--container** &lt;container >: salvestusruumi container loomiseks nime.
+ **-p** või **-eesliite** &lt;eesliite >: salvestusruumi container nime eesliide.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi.
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti.
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring.
+ **--silumine**: töötab salvestusruumi käsk silumine režiimis.

###<a name="commands-to-manage-your-storage-blob"></a>Käskude hallata oma salvestusruumi bloobimälu

**salvestusruumi bloobimälu loendi [Valikud] [container] [eesliite]**

See käsk tagastab loendi salvestusruumi plekid määratud salvestusruumi ümbrises.

See käsk toetab järgmised täiendavad suvandid.

+ **--container** &lt;container >: salvestusruumi container loomiseks nime.
+ **-p** või **-eesliite** &lt;eesliite >: salvestusruumi container nime eesliide.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi.
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti.
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring.
+ **--silumine**: töötab salvestusruumi käsk silumine režiimis.

**salvestusruumi bloobimälu kuvamine [Valikud] [container] [bloobimälu]**

See käsk kuvab määratud salvestusruumi bloobimälu üksikasju.

See käsk toetab järgmised täiendavad suvandid.

+ **--container** &lt;container >: salvestusruumi container loomiseks nime.
+ **-p** või **-eesliite** &lt;eesliite >: salvestusruumi container nime eesliide.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi.
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti.
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring.
+ **--silumine**: töötab käsk salvestusruumi silumine.

**salvestusruumi bloobimälu kustutamine [Valikud] [container] [bloobimälu]**

See käsk toetab järgmised täiendavad suvandid.

+ **--container** &lt;container >: salvestusruumi container loomiseks nime.
+ **-b** või **--bloobimälu** &lt;blobName >: salvestusruumi bloobimälu kustutamiseks nime.
+ **k -** või **--vaiksesse**: määratud salvestusruumi bloobimälu ilma kinnituse eemaldada.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi.
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti.
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring.
+ **--silumine**: töötab käsk salvestusruumi silumine.

**salvestusruumi bloobimälu üles [Valikud] [fail] [container] [bloobimälu]**

See käsk määratud faili üleslaadimiseks specified\ salvestusruumi bloobimälu.

See käsk toetab järgmised täiendavad suvandid.

+ **--container** &lt;container >: salvestusruumi container loomiseks nime.
+ **-b** või **--bloobimälu** &lt;blobName >: nime salvestusruumi bloobimälu üles laadida.
+ **-t** või **--blobtype** &lt;blobtype >: salvestusruumi bloobimälu tüüp: lehe või ploki.
+ **-p** või **--Atribuudid** &lt;atribuudid >: üleslaaditud faili atribuutide salvestusruumi bloobimälu. Atribuudid on võti väärtus paar s = ja semicolon(;) eraldatud. Saadaval atribuudid on contentType, contentEncoding, contentLanguage ja cacheControl.
+ **m** või **--metaandmete** &lt;metaandmete >: salvestusruumi bloobimälu metaandmed üleslaaditud faili. Metaandmete on põhilised = semikoolon (;) eraldatud ja väärtuse paarideks.
+ **--concurrenttaskcount** &lt;concurrenttaskcount >: üles samaaegseid taotlusi maksimaalne arv.
+ **k -** või **--vaiksesse**: määratud salvestusruumi bloobimälu ilma kinnituse kirjutada.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi.
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti.
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring.
+ **--silumine**: töötab käsk salvestusruumi silumine.

**salvestusruumi bloobimälu allalaadimine [Valikud] [container] [bloobimälu] [sihtkoht]**

See käsk laadib määratud salvestusruumi bloobimälu.

See käsk toetab järgmised täiendavad suvandid.

+ **--container** &lt;container >: salvestusruumi container loomiseks nime.
+ **-b** või **--bloobimälu** &lt;blobName >: säilitamise bloobimälu nime.
+ **-d** või **--sihtkoha** [sihtkoht]: allalaadimine sihtkoha faili või kausta tee.
+ **m** või **--checkmd5**: märkige md5sum allalaaditud faili.
+ **--concurrenttaskcount** &lt;concurrenttaskcount > üles samaaegseid taotlusi maksimumarv
+ **k -** või **--vaiksesse**: kirjutada ilma kinnituse sihtfail.
+ **-** või **--konto nime** &lt;account_name >: salvestusruumikonto nimi.
+ **-k** või **--konto võti** &lt;accountKey >: salvestusruumi konto võti.
+ **-c** või **--ühendusstringi** &lt;connectionString >: salvestusruumi ühendusstring.
+ **--silumine**: töötab käsk salvestusruumi silumine.

## <a name="commands-to-manage-sql-databases"></a>Käskude SQL-i andmebaaside haldamine

Need käsud abil saate hallata oma Azure SQL-i andmebaasid

###<a name="commands-to-manage-sql-servers"></a>Käskude SQL-i serverite haldamine.

Need käsud haldamine SQL serveri abil

**SQL serveri loomine &lt;administratorLogin > &lt;administratorPassword > &lt;asukoht >**

Loomine andmebaasiserveriga

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**SQL serveri kuva &lt;nimi >**

Serveri üksikasjade kuvamine

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**SQL serveri loend**

Siin serverite loend.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**SQL serveri Kustuta &lt;nimi >**

Kustutab server

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Käskude SQL-i andmebaaside haldamine

Need käsud abil saate hallata oma SQL andmebaase.

**SQL-i db loomine [Valikud] &lt;serveri nimi > &lt;databaseName > &lt;administratorPassword >**

Loob andmebaasi eksemplari

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**SQL-i db kuvamine [Valikud] &lt;serveri nimi > &lt;databaseName > &lt;administratorPassword >**

Andmebaasi üksikasjade kuvamine

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**SQL-i db loendi [Valikud] &lt;serveri nimi > &lt;administratorPassword >**

Andmebaaside loendit.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**SQL-i db kustutamine [Valikud] &lt;serveri nimi > &lt;databaseName > &lt;administratorPassword >**

Andmebaasi kustutamine

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>SQL serveri tulemüüri reeglite haldamiseks käsud

Need käsud abil saate hallata oma SQL serveri tulemüüri reeglid

**SQL-i firewallrule loomine [Valikud] &lt;serveri nimi > &lt;ruleName > &lt;startIPAddress > &lt;endIPAddress >**

SQL serveri tulemüüri reegli loomine.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL-i firewallrule kuvamine [Valikud] &lt;serveri nimi > &lt;ruleName >**

Tulemüüri reegli üksikasju kuvada.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**SQL-i firewallrule loendi [Valikud] &lt;serverinimi >**

Loetle tulemüüri reeglid.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL-i firewallrule kustutamine [Valikud] &lt;serveri nimi > &lt;ruleName >**

See käsk kustutab tulemüüri reegel.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Käskude virtuaalse võrguga haldamine

Need käsud abil saate hallata virtuaalse võrguga

**võrgu vnet loomine [Valikud] &lt;asukoht >**

Saate luua virtuaalse võrgus.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**võrgu vnet Kuva &lt;nimi >**

Kuva üksikasjad virtuaalse võrku.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**võrgu vnet loend**

Loetle kõik olemasolevad virtuaalse võrgu.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**võrgu vnet kustutamine &lt;nimi >**

Kustutab määratud virtuaalse võrgu.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**võrgu ekspordi [failitee]**

Täiustatud võrgu paigutus, saate eksportida võrgu konfigureerimise kohalikult. Eksporditud võrgukonfiguratsioon sisaldab DNS-i serveri sätted, virtuaalse võrgu sätteid, kohtvõrgu saidi sätted ja muud sätted.

**võrgu impordi [failitee]**

Kohaliku võrgu konfigureerimise importida.

**võrgu dnsserver registreerida [Valikud] &lt;dnsIP >**

DNS-i server, mida soovite kasutada oma võrgu konfiguratsiooni nimelahendus registreerida.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**võrgu dnsserver loend**

Loetle kõik DNS-serverid oma võrgu konfigureerimise registreeritud.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**võrgu dnsserver unregister [Valikud] &lt;dnsIP >**

Eemaldab võrgukonfiguratsioon DNS-i serveri kirje.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

