<properties
   pageTitle="3-sõlm juurutamine Deis kobar | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas luua 3 – sõlme Deis kobar Azure on Azure ressursihaldur malli abil"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>3-sõlm juurutamine Deis kobar

Selles artiklis tutvustatakse ettevalmistamise on [Deis](http://deis.io/) kobar Azure. See hõlmab kõik vajalikud serdid juurutamine ja mastaapimine valimi **avage** rakendus äsja ettevalmistatud kobar loomise juhiseid.

Järgmisel joonisel on juurutatud süsteemi arhitektuur. Süsteemiadministraator haldab kobar abil Deis nagu **deis** ja **deisctl**. Ühendused on loodud Azure laadi koormusetasakaalustusteenuse, andev ühendused ühele liige sõlmed klaster kaudu. Kliendi juurdepääsu juurutatud rakenduste kaudu kasutada ka laadi koormusetasakaalustusteenuse. Sel juhul laadi koormusetasakaalustusteenuse edastab liiklust on Deis täpsemaks routs vastava keskmise suurusega ümbriste majutatud klaster-liikluse ruuteri silma.

  ![Arhitektuur skeem juurutatud Desis kobar](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Järgmiste toimingute abil käitamiseks peate.

 * Azure'i aktiivne tellimus. Kui teil ei ole üks, saad tasuta rada [azure.com](https://azure.microsoft.com/).
 * Töö- või koolikonto id, et kasutada Azure ressursi rühmad. Kui teil on konto ja logige sisse Microsofti id, peate [looma töö id oma isikliku üks](virtual-machines-windows-create-aad-work-id.md).
 * Kas--olenevalt kliendi operatsioonisüsteem-- [Azure PowerShelli](../powershell-install-configure.md) või [Mac-arvutisse, Linux, ja Windows Azure'i CLI](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL kasutatakse loomiseks vajalikud serdid.
 * Git klient, nt [Git Bash](https://git-scm.com/).
 * Test valimi taotluse, peate samuti DNS-i serveri. Saate kasutada mis tahes DNS-serverid või teenuseid, mis toetavad metamärkide A kirjed.
 * Arvuti käivitamiseks Deis kliendi tööriistad. Saate kas kohalikus arvutis või virtuaalse masina. Neid tööriistu saate kasutada mistahes Linux jaotuse, kuid järgmised juhised kasutada Ubuntu.

## <a name="provision-the-cluster"></a>Sätte klaster

Selles jaotises saate kasutada mõni [Azure ressursihaldur](../azure-resource-manager/resource-group-overview.md) Mall avatud allika hoidla [azure-Kiirjuhend-Mallid](https://github.com/Azure/azure-quickstart-templates). Esmalt saate kopeerida Mall alla. Seejärel saate luua uue SSH võti paari autentimiseks. Ja seejärel tuleb konfigureerida uue tunnuse saate kobar. Ja lõpuks, saate kasutada Shell script või PowerShelli skripti ettevalmistamise klaster.

1. Hoidla klooni: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Avage Mall kausta.

        cd azure-quickstart-templates\deis-cluster-coreos

3. Looge uus SSH võti paari kasutades ssh-keygen.

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Luua serdi abil ülaltoodud privaatvõti:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Minge [https://discovery.etcd.io/new](https://discovery.etcd.io/new) luua uue kobar loa, mis näeb välja midagi.

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Iga CoreOS kobar peab olema kordumatu luba selle tasuta teenuse kaudu. Üksikasjalikumat teavet leiate [CoreOS dokumentatsiooni](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) .

6. Asendage olemasolevad **discovery** luba uue loa **cloud-config.yaml** faili muutmiseks tehke järgmist.

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Muutke **azuredeploy-parameters.json** faili: Avage tekstiredaktoris juhises 4 loodud sert. Kopeerige kogu teksti vahel `----BEGIN CERTIFICATE-----` ja `-----END CERTIFICATE-----` **sshKeyData** parameetri (peate eemaldada kõik reavahetusmärgid) sisse.

8. Muutke **newStorageAccountName** parameeter. See on salvestusruumi konto jaoks VM OS ketast. Selle konto nimi peab olema kordumatu globaalselt.

9. Muutke **publicDomainName** parameeter. See muutub osa laadi koormusetasakaalustusteenuse avaliku IP-ga seostatud DNS-i nimi. Lõplik FQDN on _[parameetri väärtuse]_vormingut. _[piirkond]_. cloudapp.azure.com. Näiteks kui määrate deishbai32 nime ja Lääne USA piirkonnas on juurutatud ressursirühma, siis lõplik FQDN-i abil oma laadi koormusetasakaalustusteenuse saab deishbai32.westus.cloudapp.azure.com.

10. Salvestage fail parameeter. Ja seejärel saate ettevalmistamise klaster Azure PowerShelli kaudu.

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  Azure'i CLI või tehke järgmist.

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Kui ressursirühma on ette valmistatud, näete kõigi ressursside jaotises Azure klassikaline portaalis. Nagu on näidatud järgmises kuvatõmmis, ressursirühma sisaldab kolme VMs, mis on ühendatud kättesaadavus samu virtuaalse võrgu. Rühm sisaldab ka laadi koormusetasakaalustusteenuse, mis on seotud avaliku IP.

  ![Azure'i klassikaline portaalis ettevalmistatud ressursirühm](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Kliendi installimine

Teil on vaja **deisctl** juhtelemendile oma Deis kobar. Kuigi deisctl installitakse automaatselt kõik kobar sõlmed, on mõistlik kasutada deisctl eraldi haldus arvutisse. Lisaks kuna kõik sõlmed on konfigureeritud ainult privaatne IP-aadressid, peate kasutama SSH tunneling laadi koormusetasakaalustusteenuse, mis on avalik IP, sõlm masinad ühenduse kaudu. Järgmisena kirjeldame deisctl füüsilise eraldi Ubuntu või virtuaalse masina häälestamise juhiseid.

1. Installi deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Lisage oma isikliku võtme ssh agent.

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Konfigureerige deisctl.

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Mall määratleb NAT sissetulevad reeglid, mis vastendamine 2223, kui soovite näiteks 1, 2 eksemplari 2224 ja 2225 eksemplari 3. See võimaldab koondamise deisctl tööriista abil. Saate uurida Azure klassikaline portaalis reegleid:

![Laadi koormusetasakaalustusteenuse NAT reeglid](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] Malli toetab praegu ainult 3 sõlme kogumite. See on piirang Azure'i ressursihaldur malli NAT reegli määratlus, mis ei toeta tsükkel süntaks.

## <a name="install-and-start-the-deis-platform"></a>Installimine ja käivitamine on Deis platvorm

Nüüd saate deisctl installimiseks ja selle Deis platvorm:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Alates platvormi võtab aega (nii palju kui 10 minuti). Eriti, teenuse Koosturi käivitamine võib võtta kaua aega. Ja mõnikord kulub mitu korda õnnestub: kui toiming lahendab hangub, proovige tippida `ctrl+c` katkestamine käsu täitmine ja proovige uuesti.

Saate kasutada `deisctl list` kinnitamiseks, kui kõik teenused töötavad:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Palju õnne! Nüüd olete saanud jooksva Deis clsuter Azure! Järgmiseks loome valimi kobar tegelikkuses kuvamiseks avage rakenduse juurutamine.

## <a name="deploy-and-scale-a-hello-world-application"></a>Juurutada ja mastaapimiseks Tere, maailm rakendus

Järgmiste juhiste kuvamine juurutamise on "Tere, maailm" Avage rakendus klaster. Juhised põhinevad [Deis dokumentatsiooni](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Marsruutimise silma töötaks õigesti, peate olema metamärkide A kirje oma domeeni osutamise laadi koormusetasakaalustusteenuse avaliku IP. Järgmine pilt kuvatakse GoDaddy domeeni registreerimine valimi A-kirje:

    ![GoDaddy A-kirje](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Installi deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Loo uus SSH võti ja seejärel lisage github avalik võti (muidugi, saate ka uuesti kasutada oma olemasoleva klahvid). Uue SSH võti paari loomiseks kasutada:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Lisada GitHub id_rsa.pub või avalik võti oma valik. Saate teha selle abil lisada SSH võtme nupp ekraani SSH klahvid konfigureerimine.

  ![Github võti](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Uue kasutaja registreerimine

        deis register http://deis.[your domain]
<p />
6. SSH võti lisamiseks tehke järgmist.

        deis keys:add [path to your SSH public key]
  <p />      
7. Rakenduse loomine.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.Tõuketeatised git käivitab keskmise suurusega pilte ehitatud ja juurutada, mis võtab paar minutit. Minu kogemus, võib toimingu 10 (väsimus pilt privaatne hoidlasse) aeg-ajalt, hanguda, et. Sellisel juhul saate peatada protsessi, rakenduse kaudu eemaldada ' rakendused deis: hävitada – <application name> ` to remove the application and try again. You can use `deis apps:list' teada oma rakenduse nimi. Kui kõik toimib, peaksite nägema umbes järgmine käsk väljundeid lõpus:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Kontrollige, kas rakendus töötab:

        curl -S http://[your application name].[your domain]
  Näha peaks olema:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Skaala rakenduse eksemplari 3:

        deis scale cmd=3
<p />
11. Soovi korral saate kasutada deis teave rakenduse üksikasjade tutvuda. Minu rakenduse juurutamine on järgmised:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis kõndis saate ettevalmistamise uus juhiseid järgides Deis kobar Azure on Azure ressursihaldur malli abil. Malli toetab koondamise instrumentaarium ühenduste kui ka laadi juurutatud rakenduste jaoks. Malli ka vältida avaliku IP liikme sõlmed, mis salvestab väärtuslik avaliku IP ressursid ja pakub rohkem turvalise host rakenduste abil. Lisateabe saamiseks lugege järgmisi artikleid:

[Azure'i ressursihaldur ülevaade] [resource-group-overview]  
[Kuidas kasutada Azure CLI] [azure-command-line-tools]  
[Azure'i ressursihaldur Azure PowerShelli abil] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
