<properties
   pageTitle="Luua ja üles laadida pildi FreeBSD VM | Microsoft Azure'i"
   description="Saate teada, kuidas luua ja üles laadida virtuaalse arvuti kõvakettale (VHD), mis sisaldab FreeBSD operatsioonisüsteem on Azure virtuaalse masina loomiseks"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Luua ja üles laadida FreeBSD VHD Azure

Selles artiklis kirjeldatakse, kuidas luua ja üles laadida virtuaalse arvuti kõvakettale (VHD), mis sisaldab FreeBSD operatsioonisüsteem. Pärast üleslaadimist, saate selle pildina oma Azure virtuaalse masina (VM) loomiseks.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et teil on järgmised üksused:

- **An Azure tellimuse**--kui teil pole kontot, saate luua ühe vaid paar minutit. Kui teil on MSDN-i tellimus, lugege teemat [kuu Azure'i krediiti Visual Studio tellijad.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Muul juhul saate teada, kuidas [luua tasuta prooviversiooni konto](https://azure.microsoft.com/pricing/free-trial/).  

- **Azure'i PowerShelli tööriistad**--The Azure PowerShelli mooduli olema installitud ja konfigureeritud kasutama Azure'i tellimuse. Mooduli allalaadimiseks leiate [Azure'i allalaadimist](https://azure.microsoft.com/downloads/). Õpetuse, mis kirjeldab, kuidas installida ja konfigureerida moodulit on saadaval. [Azure'i allalaadimist](https://azure.microsoft.com/downloads/) cmdlet-i abil üles soovitud VHD.

- **FreeBSD operatsioonisüsteem .vhd faili**--toetatud FreeBSD operatsioonisüsteem on peab olema installitud virtuaalse kõvakettale. Mitme tööriistad olemas .vhd faile luua. Näiteks saate näiteks Hyper-V virtualization lahenduse .vhd faili loomine ja installige operatsioonisüsteem. Juhised selle kohta, kuidas installida ja kasutada Hyper-V, leiate artiklist [installida Hyper-V ja luua virtuaalse masina](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Azure'i ei toeta VHDX uude vormingusse. Saate teisendada ketas VHD vorming, kasutades Hyper-V halduri või cmdlet-käsk [Teisenda-vhd](https://technet.microsoft.com/library/hh848454.aspx). Lisaks on [õpetus MSDN-i FreeBSD Hyper - v kasutamise kohta](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

See hõlmab viis järgmist.

## <a name="step-1-prepare-the-image-for-upload"></a>Samm 1: Pildi ettevalmistamine üleslaadimine

Virtuaalne arvutisse, kuhu installisite FreeBSD operatsioonisüsteem, tehke järgmist:

1. Lubage DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. Luba SSH.

    SSH on vaikimisi pärast installi kettalt. Kui see pole lubatud mingil põhjusel või kui kasutate FreeBSD VHD otse, tippige järgmine tekst.

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Saate häälestada järjestikune konsooli.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Installige sudo.

    Azure'i keelatakse root konto. See tähendab, et peate kasutama sudo eesõiguseta käivitamiseks käsud administraatoriõigustega kasutaja.

        # pkg install sudo
;
5. Azure'i Agent eeltingimuste.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Installige Azure'i Agent.

    [Github](https://github.com/Azure/WALinuxAgent/releases)alati leiate Azure'i Agent uusimat versiooni. Versioon 2.0.10 + ametliku toetab FreeBSD 10 & 10,1 ja versioon 2.1.4 ametliku toetab FreeBSD 10.2 ja uuemad versioonid väljalasete.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    2.0, kasutame 2.0.16 näide:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    2,1, vaatame kasutavad näidisena 2.1.4:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Pärast installimist Azure Agent, on soovitav kontrollida, kas see töötab:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Deprovision süsteem.

    Deprovision puhastamiseks ja veenduge, et see sobib uuesti ettevalmistamise süsteem. Järgmine käsk kustutatakse ka viimase ettevalmistatud kasutaja konto ja seotud andmeid:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Nüüd saate oma VM sulgeda.

## <a name="step-2-create-a-storage-account-in-azure"></a>Samm 2: Azure salvestusruumi konto loomine ##

Teil on vaja salvestusruumi konto Azure nii, et see saab luua virtuaalse masina .vhd faili üleslaadimiseks. Azure'i klassikaline portaali abil saate luua salvestusruumi konto.

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida.

2. Valige käsk **Uus**.

3. Valige **Data Services** > **salvestusruumi** > **kiire loomine**.

    ![Kiire salvestusruumi konto loomine](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Täitke väljad järgmiselt:

    - Tippige väljale **URL-i** alamdomeeni kasutamine salvestusruumi konto URL-i nimi. Kirje võib sisaldada 3-24 arvude ja väiketähti. See nimi muutub hostinimi sees URL, mida kasutatakse tellimuse aadress Azure'i bloobimälu, Azure'i järjekorda salvestusruumi või Azure'i tabeli salvestusruumi ressursid.

    - Valige rippmenüüst **Asukoht/osaleja rühma** selle **asukoha või osaleja rühma** salvestusruumi konto. Mõne osaleja rühma võimaldab teil panna oma pilveteenustega ja salvestusruumi sama andmekeskuse.

    - Väljale **Dispersioonanalüüs** otsustada, kas kasutada **Geograafilise liigne** dispersioonanalüüs salvestusruumi konto. Geo-dispersioonanalüüs on vaikimisi sisse lülitatud. See suvand tiražeerib andmete teisese asukohta, tasuta teile nii, et teie salvestusruumi ei üle sinna suurema tõrke ilmnemisel esmane asukohas. Teisene asukoht määratakse automaatselt ja ei saa muuta. Kui vajate rohkem kontrolli asukoha pilvepõhist salvestusruumi juriidilistele nõuetele või ettevõtte poliitika tõttu, saate geo-dispersioonanalüüs välja lülitada. Siiski arvestage, et kui lülitate hiljem geo-dispersioonanalüüs, saate tuleb tasuda ühekordse andmete edastamine tasu ise oma olemasolevad andmed teisel asukohta. Salvestusruumi teenuste ilma geo-dispersioonanalüüs pakutakse allahindlust. Rohkem üksikasju geo-dispersioonanalüüs salvestusruumi kontod haldamise kohta leiate siit: [loomine, haldamine, või salvestusruumi konto kustutada](../storage-create-storage-account/#replication-options).

    ![Sisestage salvestusruumi konto üksikasjad](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Valige **salvestusruumi konto loomine**. Konto kuvatakse nüüd jaotises **salvestusruum**.

    ![Salvestusruumi konto on loodud](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Järgmisena looge ümbris .vhd üles laaditud failidega. Valige salvestusruumikonto nimi ja seejärel valige **ümbriste**.

    ![Salvestusruumi konto üksikasjad](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Valige **ümbris loomine**.

    ![Salvestusruumi konto üksikasjad](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. Tippige väljale **nimi** nimi oma ümbrises. Seejärel valige **Accessi** rippmenüü, mis tüüpi Accessi poliitika soovite.

    ![Container nimi](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Vaikimisi ümbris on privaatne ja pääseb juurde ainult konto omanik. Luba avaliku lugemisõigus plekid ümbrises, kuid mitte container atribuudid ja metaandmed, kasutage **Avaliku bloobimälu** suvandit. Kasutage täielikku avaliku lugemisõigus container ja plekid lubamiseks **Avaliku Container** suvandit.

## <a name="step-3-prepare-the-connection-to-azure"></a>Samm 3: Ettevalmistamine Azure'i ühendus

Enne .vhd faili saate üles laadida, peate oma arvuti ja Azure tellimuse turvalist ühendust luua. Selleks saate kasutada Azure Active Directory (Azure AD) meetodite või sert.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Azure AD meetodi abil .vhd faili üleslaadimine

1. Avage Azure'i PowerShelli konsooli.

2. Tippige järgmine käsk:  
    `Add-AzureAccount`

    See käsk avab kus saate sisse logida oma töökoha või kooli kontoga sisselogimine aken.

    ![PowerShelli aken](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure'i autendib ja salvestab mandaadi teavet. Klõpsake akna sulgeda.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Serdi meetodi abil .vhd faili üleslaadimine

1. Avage Azure'i PowerShelli konsooli.

2. Tüüp:  `Get-AzurePublishSettingsFile`.

3. Brauseriaknas avatakse ja palub teil .publishsettings faili alla laadida. See fail sisaldab teavet ja Azure tellimuse sert.

    ![Brauseri allalaadimise lehelt](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Salvestage fail .publishsettings.

4. Tüüp:  `Import-AzurePublishSettingsFile <PathToFile>`, kus `<PathToFile>` on .publishsettings faili täielik tee.

   Lisateabe saamiseks lugege teemat [Azure cmdlettide kasutamise alustamine](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Installimise ja PowerShelli konfigureerimise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Samm 4: .vhd faili üles laadida

Kui .vhd faili üleslaadimiseks võite paigutada selle suvalist bloobimälus sees. Järgnevalt on teatud tingimustel peate kasutama faili üleslaadimisel.
-  **BlobStorageURL** on URL-i salvestusruumi konto, mille olete loonud samm 2.
-  **YourImagesFolder** on pakendi sees bloobimälu, kuhu soovite salvestada oma pilte.
- **VHDName** on silt, mis kuvatakse Azure klassikaline portaalis tuvastamiseks kettaruumi.
- **PathToVHDFile** on täielik tee ja .vhd faili nimi.


Kasutasite eelmises etapis Azure PowerShelli aknas tippige:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Juhis 5: Luua VM üleslaaditud .vhd faili abil
Pärast üleslaadimist .vhd faili, saate lisada selle pildina kohandatud pilte, mis on seotud teie tellimus ja luua virtuaalse masina selle kohandatud pildi loendit.

1. Kasutasite eelmises etapis Azure PowerShelli aknas tippige:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Kasutage Linux OS tüüp. Praegune versioon Azure PowerShelli aktsepteerib ainult "Linux" või "Windows" parameetrina.

2. Pärast lõpetamist eelmisi toiminguid, kuvatakse uus pilt kui valite Azure klassikaline portaalis menüüd **pildid** .  

    ![Valige pilt](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Luua virtuaalse masina galeriist. See uus pilt on nüüd saadaval jaotises **Minu pildid**.
4. Valige uus pilt. Järgmiseks läbida viipasid häälestamine hosti nimi parool SSH võti ja jne.

    ![Kohandatud pilt](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Kui olete selle ettevalmistamise, näete oma FreeBSD VM Azure töötab.

    ![Azure FreeBSD pilt](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
