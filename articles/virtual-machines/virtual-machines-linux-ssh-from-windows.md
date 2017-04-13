<properties 
    pageTitle="Kasutage SSH Windows Linux vms | Microsoft Azure'i" 
    description="Saate teada, kuidas luua ja kasutada SSH klahve Windows arvutis ühenduse loomiseks Linux virtuaalse masina Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Kuidas kasutada SSH klahve Windows Azure

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux-või Mac-](virtual-machines-linux-mac-create-ssh-keys.md)

Kui loote ühenduse Linux virtuaalmasinates (VM) Azure, tuleks [avaliku võtme krüptograafia](https://wikipedia.org/wiki/Public-key_cryptography) abil turvalisemad võimalda logige sisse oma Linux VM. See protsess hõlmab avalike ja privaatvõ võtme Exchange'i secure shell (SSH) käsuga kinnitamiseks enda asemel kasutajanime ja parooli. Paroolid on brute-Jõusta eest, eriti klõpsake Interneti-ühendusega VMs nagu web servereid. Selles artiklis antakse ülevaade SSH võtmed ja vastav võtmed Windowsi arvutisse, tehke järgmist.


## <a name="overview-of-ssh-and-keys"></a>Ülevaade SSH ja võtmed

Saate turvaliselt logida oma Linux VM avalike ja privaatvõ klahvide abil:

- **Avalik võti** paigutatakse oma Linux VM või muu teenus, mida soovite kasutada avaliku võtme krüptograafia.
- **Privaatvõti** on, mida saate esitada oma Linux VM sisselogimisel, teie identiteedi. See privaatvõti kaitsta. Seda jagada.

Avalike ja privaatvõ klahve saab kasutada mitut VMs ja teenuste kohta. Teil pole vaja iga VM või teenusele juurdepääsu paari võtmed. Üksikasjalikuma ülevaate leiate teemast [avaliku võtme krüptograafia](https://wikipedia.org/wiki/Public-key_cryptography).

SSH on krüptitud ühendust protokolli, mis võimaldab turvaline sisselogimise üle turvamata ühendused. See on vaikimisi ühenduse protokolli Linux vms majutatud Azure. Kuigi SSH ise pakub krüptitud ühendust, paroolide kasutamise SSH ühendustega jätab VM tundlik brute Jõusta eest või aim paroolid. Ühenduse abil SSH VM turvalisemaks, ja eelistatud, meetod on need avalike ja privaatvõ võtmed, nimetatakse ka SSH klahvide abil.

Kui te ei soovi kasutada SSH klahvid, mida saate ikka sisse logida oma Linux vms parooliga. Kui teie VM puudub Interneti-ühendus, paroolide kasutamise võib olla piisav. Siiski vajate haldamine paroole iga Linux VM ja säilitada terve parool poliitika ja tavad, nt parooli miinimumpikkus ja neid regulaarselt uuendada. SSH klahvid kasutamine vähendab keerukuse üle mitme VMs üksikute identimisteabe haldamine.


## <a name="windows-packages-and-ssh-clients"></a>Windows paketid ja SSH kliendid

Ühenduse loomine ja haldamine Linux VMs Azure Kasuta e **ssh** . Windowsi arvutis on tavaliselt on **ssh** kliendi installitud. Levinud Windows SSH kliendid saate installida kuuluvad järgmised:

- [Git For Windows](https://git-for-windows.github.io/)
- [Kitt](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)

> [AZURE.NOTE] Windows 10 tähtpäeva värskenduses sisaldab Bash for Windows. See funktsioon võimaldab käivitada Windowsi alamsüsteem Linux ja Accessi Utiliidid nagu SSH kliendi jaoks. Bash Windows on alles väljatöötamisel ja käsitletakse beetaversiooni. Bash for Windows kohta leiate lisateavet teemast [Bash Ubuntu Windowsis](https://msdn.microsoft.com/commandline/wsl/about).


## <a name="which-key-files-do-you-need-to-create"></a>Võtme faile, mis on vaja luua?

Azure'i jaoks on vaja vähemalt 2048-bitine **ssh-rsa** vormindamine avalike ja privaatvõ võtmed. Kui haldate klassikaline juurutamise näidise Azure ressursse, peate ka luua mõne PEM (`.pem` fail).

Siin on juurutamise stsenaariumide ja kasutate iga failitüübid.

1. **SSH-rsa** võtmed on nõutav [Azure portaali](https://portal.azure.com)juurutamise ja kasutamise [Azure'i CLI](../xplat-cli-install.md)ressursihaldur juurutuste.
    - Need võtmed on tavaliselt kõik enamik inimesi vaja.
2. `.pem`fail on vaja luua VMs [klassikaline portaalis](https://manage.windowsazure.com). Klassikaline juurutuste, mis kasutavad [Azure'i CLI](../xplat-cli-install.md)on toetatud ka need võtmed.
    - Ainult peate looma need täiendavad võtmed ja sertifikaatide kohta, kui haldate klassikaline juurutamise mudeli abil loodud ressursid.


## <a name="install-git-for-windows"></a>Git for Windowsi installimine

Eelmises jaotises toodud mitmeid pakette, mis sisaldavad soovitud `openssl` tööriista Windowsi jaoks. See tööriist on vaja luua avalike ja privaatvõ võtmed. Järgmised näited üksikasjalikult, kuidas installida ja kasutada **Windowsi Git**, kuigi sellest, millist paketi saate soovi korral saate valida. **Windowsi git** kaudu pääsete juurde mõned täiendavad avatud allikaga tarkvara ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) tööriistad ja Utiliidid, mis võib olla kasulik, kui töötate Linux VMs.

1. Laadige alla ja installige **Windowsi Git** järgmises asukohas: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. Aktsepteerige vaikesuvandite installimisel juhul, kui peate konkreetselt neid muuta.

3. **Menüü Start** **Git Bash** pidamine > **Git** > **Git Bash**. Konsooli näeb välja järgmine näide:

    ![Git Windows Bash shell](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Privaatvõti loomine

1. Aknas **Git Bash** kasutada `openssl.exe` privaatvõti loomiseks. Järgmises näites luuakse nimega klahvi `myPrivateKey` ja serdi nimega `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Väljund näeb välja järgmine näide:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Vastake viipasid riigi nimi ja asukoht, ettevõtte nimi, jne.

3. Teie uus privaatvõti ja serdi luuakse oma praegust töötavat kausta. Jaoks parimad, peate seadma nende õiguste kohta oma privaatvõti nii, et ainult sellele juurde pääsevad:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Kui teil on vaja ka klassikalises ressursside haldamine, teisendada selle `myCert.pem` et `myCert.cer` (DER-kodeeritud X509 sert). Seda juhist valikuline ainult siis, kui teil on vaja vanema klassikalises ressursside täpsemalt juhtida. 

    Teisenda serdi abil järgmine käsk:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Privaatvõti PuTTY loomine

PuTTY on levinud SSH kliendi Windowsi jaoks. Teil on tasuta, mis tahes SSH klient, mida soovite kasutada. PuTTY kasutamiseks peate looma muud tüüpi võti - PuTTY privaatne võti (PPK). Kui te ei soovi kasutada kitt, selle sammu vahele.

Järgmises näites luuakse selle täiendavad privaatvõti spetsiaalselt PuTTY kasutada:

1. **Git Bash** abil saate muuta oma privaatvõti RSA privaatvõti, mis saavad aru PuTTYgen. Järgmises näites luuakse nimega klahvi `myPrivateKey_rsa` olemasoleva Key nimega `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Jaoks parimad, peate seadma nende õiguste kohta oma privaatvõti nii, et ainult sellele juurde pääsevad:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Laadige alla ja käivitage PuTTYgen järgmisest asukohast: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Klõpsake menüüd: **faili** > **Laadi privaatvõti**

4. Otsige üles oma privaatvõti (`myPrivateKey_rsa` eelmises näites). Kui alustate **Git Bash** vaikekataloogi on `C:\Users\%username%`. Muuta faili filter kuvamiseks **kõik failid (\*.\*)**:

    ![Olemasoleva privaatvõti PuTTYgen laadimine](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Klõpsake nuppu **Ava**. Viip näitab, et võti on edukalt imporditud:

    ![Imporditud PuTTYgen võti](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Klõpsake nuppu **OK** , et sulgeda küsimuse.

7. Avalik võti kuvatakse **PuTTYgen** akna ülaservas. Kopeerida ja kleepida selle avalik võti Azure portaali või Azure ressursihaldur malli Linux VM loomisel. Võite klõpsata ka koopia salvestamiseks oma arvutisse **salvestada avalik võti** :

    ![Kitt avaliku võtme faili salvestamine](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    Järgmises näites on kujutatud, kuidas soovite Kopeeri ja kleebi see avalik võti Azure portaali Linux VM loomisel. Avalik võti tavaliselt talletatakse seejärel `~/.ssh/authorized_keys` oma uue VM.

    ![Avalik võti kasutada, kui loote VM Azure portaalis](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Olles tagasi kohas **PuTTYgen**, klõpsake nuppu **Salvesta privaatvõti**:

    ![Salvestage fail PuTTY privaatvõti](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Viip küsib, kui soovite jätkata ilma sisestate parooli tootevõtit. Parool on nagu oma privaatvõti lisatud parool. Isegi kui keegi teie privaatvõti hankida, nad endiselt oleks ei saa autentida, kasutades lihtsalt klahvi. Nad peaksid ka parool. Ilma parooli, kui keegi saab oma privaatvõti, nad saaksid sisse logida mis tahes VM või teenus, mis kasutab selle klahvi. Soovitame teil Loo parool. Aga kui unustate parool, on kuidagi seda taastada.

    Kui soovite sisestada parool, klõpsake nuppu **ei**, sisestage parool PuTTYgen põhiaknas ja klõpsake siis nuppu **Salvesta privaatvõti** uuesti. Muul viisil, klõpsake nuppu **Jah** jätkamiseks võimaldamata valikuline parool.

8. Sisestage nimi ja asukoht PPK faili salvestamiseks.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Kasutage kitt, et SSH Linux masina

Klõpsake uuesti PuTTY on levinud SSH kliendi Windowsi jaoks. Teil on tasuta, mis tahes SSH klient, mida soovite kasutada. Järgmised toimingud üksikasjalikult autentimiseks oma Azure VM SSH abil oma privaatvõti kasutamise kohta. Juhised on sarnane muude SSH kliendid seisukohast laadimiseks oma privaatvõti vajavad SSH autentida.

1. Laadi alla ja Käivita kitt järgmisest asukohast: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Täitke hosti nimi või IP-aadress oma VM Azure portaali kaudu:

    ![Avage uus kitt ühendus](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Enne kui valite **avatud**, klõpsake **ühenduse** > **SSH** > **Auth** menüü. Otsige sirvides üles ja valige oma privaatvõti.

    ![Valige oma PuTTY privaatvõti autentimine](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Klõpsake nuppu **Ava** ühenduse loomiseks arvuti virtuaalne
 

## <a name="next-steps"></a>Järgmised sammud
Saate luua ka avalike ja privaatvõ klahvide [abil OS X ja Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Bash for Windows ja kasu on OSS tööriistad käepärast Windowsi arvutisse kohta leiate lisateavet teemast [Bash Ubuntu Windowsis](https://msdn.microsoft.com/commandline/wsl/about).

Kui teil on probleeme, kasutades SSH ühenduse loomiseks oma Linux VMs, lugege teemat [tõrkeotsing SSH ühendused on Azure Linux VM](virtual-machines-linux-troubleshoot-ssh-connection.md).