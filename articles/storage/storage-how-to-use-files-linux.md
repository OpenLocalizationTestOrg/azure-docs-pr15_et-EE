<properties
    pageTitle="Azure'i failide kasutamine Linux | Microsoft Azure'i"
        description="Luua on Azure faili ühiskasutus pilveteenuses selle üksikasjaliku juhendi abil. Faili ühiskasutus sisu haldamine ja mount Linux või kohapealse rakendus, mis toetab SMB 3.0 faili osa: Azure virtuaalse masina (VM)."
        services="storage"
        documentationCenter="na"
        authors="mine-msft"
        manager="aungoo"
        editor="tysonn" />

<tags ms.service="storage"
      ms.workload="storage"
      ms.tgt_pltfrm="na"
      ms.devlang="na"
      ms.topic="article"
      ms.date="10/18/2016"
      ms.author="minet" />


# <a name="how-to-use-azure-file-storage-with-linux"></a>Kuidas kasutada Linux Azure'i failide talletamine

## <a name="overview"></a>Ülevaade

Azure'i failisalvestusruumi pakub failikettad standard SMB protokolli abil pilveteenuses. Azure'i faile, saate migreerida Azure serverites ettevõtte rakendused. Helistamine ja Azure saate hõlpsalt ühendage failikettad: Azure'i virtuaalmasinates töötab Linux. Ja koos failide talletamine uusimat versiooni, saate ka ühendate failikettale kohapealse rakendusest, mis toetab SMB 3.0.

Azure'i failikettad [Azure portaali](https://portal.azure.com), Azure salvestusruumi PowerShelli cmdlet-käsud, Azure Storage kliendi teegid või Azure Storage REST API abil saate luua. Lisaks failikettad on SMB ühtlasi, pääsete neile standard failisüsteemi API-de kaudu.

Failide talletamine põhineb samal tehnoloogial bloobimälu, tabeli ja järjekorda salvestusruumi, et failide talletamine pakub kättesaadavus, kestvus ja skaleeritavus geo koondamise salvestusruumi Azure'i platvormi sisse ehitatud. Faili salvestusruumi tulemuslikkuse eesmärkide ja piirangute kohta lisateavet [Azure salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md).

Failide talletamine on nüüd üldiselt kättesaadav ja toetab nii SMB 2.1 SMB 3.0. Täiendavad üksikasjad failide talletamine, vt [faili teenuse REST API -ga](https://msdn.microsoft.com/library/azure/dn167006.aspx).

>[AZURE.NOTE] Linux SMB klient veel ei toeta krüptimist, nii, et paigaldus failikettale Linux endiselt nõuab kliendi Azure'i piirkonna nimega faili ühiskasutusse andmine. Siiski krüptimise Linuxi tugi on juhised Linux arendajad vastutab SMB funktsioonid. Tulevikus krüptimise toetavate Linuxi saab ühendada mõne Azure'i failikettal igal pool ka.

## <a name="video-using-azure-file-storage-with-linux"></a>Video: Azure'i failide talletamine funktsiooniga Linux

Siin on video, mis näitab, kuidas luua ja kasutada Azure failikettad Linux.

> [AZURE.VIDEO azure-file-storage-with-linux]

## <a name="choose-a-linux-distribution-to-use"></a>Valige Linux jaotuse kasutamine ##

Azure'i Linux virtuaalse masina loomisel saate määrata Linux pilt, mis toetab Azure Pilt galeriist SMB 2.1 või uuem versioon. Allpool on soovitatav Linux piltide loend.

- Ubuntu Server 14.04 +
- RHEL 7 +
- CentOS 7 +
- Debian 8
- openSUSE 13.2 +
- SUSE Linux Enterprise Server 12
- SUSE Linux Enterprise Server 12 (Premium pilt)

## <a name="mount-the-file-share"></a>Faili ühiskasutus Mount ##

Ühendada fail share Linux töötab, võib-olla peate installima mõne SMB/CIFS kliendi kui kasutate jaotuse ei sisalda sisseehitatud klient. See on Ubuntu käsk installida ühe valik cifs-utils:

    sudo apt-get install cifs-utils

Järgmiseks peate tegema alusega käsk (mkdir mymountpoint) ja seejärel probleemi ühendamine käsu, mis on umbes järgmine:

     sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename ./mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777

Saate lisada ka oma /etc/fstab ühendada ühiskasutus sätted.

Pange tähele, et 0777 siin tähistada directory/faili õiguste kood, mis annab täitmise/lugemis-ja kirjutamisõigusega õigused kõigile kasutajatele. Saate selle asendada muu faili õiguste kood Linux faili õiguste dokumenti jälgima.

Lisaks taaskäivitage arvuti pärast paigaldatud failikettale hoidmiseks saate lisada soovitud sätet, nagu allpool oma /etc/fstab:

    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777

Näiteks kui olete loonud Azure VM Linux pilt Ubuntu Server 15.04 (mis on saadaval Azure Pilt galeriist), kasutades saate ühendate faili allpool:

    azureuser@azureconubuntu:~$ sudo apt-get install cifs-utils
    azureuser@azureconubuntu:~$ sudo mkdir /mnt/mountpoint
    azureuser@azureconubuntu:~$ sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    azureuser@azureconubuntu:~$ df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

Kui kasutate CentOS 7.1, ühendate fail järgmises vormingus:

    [azureuser@AzureconCent ~]$ sudo yum install samba-client samba-common cifs-utils
    [azureuser@AzureconCent ~]$ sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    [azureuser@AzureconCent ~]$ df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

Kui te kasutate avatud SUSE 13.2, ühendate fail järgmises vormingus:

    azureuser@AzureconSuse:~> sudo zypper install samba*  
    azureuser@AzureconSuse:~> sudo mkdir /mnt/mountpoint
    azureuser@AzureconSuse:~> sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    azureuser@AzureconSuse:~> df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

## <a name="manage-the-file-share"></a>Faili ühiskasutusse andmise haldamine ##

[Azure'i portaal](https://portal.azure.com) sisaldab kasutajaliidese haldamise Azure'i Failisalvestusruumi. Veebibrauseri kaudu saate teha järgmisi toiminguid:

- Üles- ja allalaadimine failide ja sealt oma failikettal.
- Jälgida iga failikettal tegelik kasutamist.
- Faili ühiskasutus mahupiirangut reguleerida.
- Kopeerige soovitud `net use` käsu kasutada oma failikettal ühendada Windows kliendi.

Faili ühiskasutusse andmise haldamine saate kasutada ka Linux Azure'i platvormidel käsurea liides (Azure'i CLI). Azure'i CLI annab avatud allikas platvormidel käske töötada Azure Storage, sh failisalvestusruumi. Suur osa sellest sama funktsiooni leitud Azure portaali kui ka rikkaliku andmed Accessi toimimisele pakub. Näiteid leiate [Azure'i CLI Azure Storage abil](storage-azure-cli.md).

## <a name="develop-with-file-storage"></a>Töötada koos failide talletamine ##

Arendaja, saate koostada taotluse failide talletamine ja [Azure salvestusruumi kliendi teek Java](https://github.com/azure/azure-storage-java)abil. Koodi näited, vaadake, [Kuidas kasutada Java: failide talletamine](storage-java-how-to-use-file-storage.md).

[Azure'i salvestusruumi kliendi teegi jaoks Node.js](https://github.com/Azure/azure-storage-node) abil arendamise vastu failisalvestusruumi.

## <a name="feedback-and-more-information"></a>Tagasiside ja lisateabe saamine ##

Linuxi kasutajad – Soovime kuulda teie!

Azure'i failisalvestusruumi Linuxi kasutajate rühma jaoks annab teile ühiskasutusse anda tagasisidet hinnata ja vastu võtta failide talletamine Linux. E-posti [Azure'i faili salvestusruumi Linuxi kasutajad](mailto:azurefileslinuxusers@microsoft.com) liituda kasutajate rühma.

## <a name="next-steps"></a>Järgmised sammud

Vt lisateavet Azure failide talletamine neid linke.

### <a name="conceptual-articles-and-videos"></a>Kontseptuaalne artiklid ja videod

- [Azure'i failide salvestusruumi: frictionless pilv SMB failisüsteemi Windows ja Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
- [Alustamine Windows Azure'i failide talletamine](storage-dotnet-how-to-use-files.md)

### <a name="tooling-support-for-file-storage"></a>Instrumentaarium tugi: failide talletamine

- [AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)
- [Loomine ja haldamine failikettad](storage-azure-cli.md#create-and-manage-file-shares) Azure CLI abil

### <a name="reference"></a>Viide

- [Faili teenuse REST API viide](http://msdn.microsoft.com/library/azure/dn167006.aspx)
- [Azure'i failid tõrkeotsingu artikkel](storage-troubleshoot-file-connection-problems.md)

### <a name="blog-posts"></a>Ajaveebipostituste

- [Azure'i failide talletamine on nüüd üldiselt kättesaadav](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
- [Kollased Azure failide talletamine](https://azure.microsoft.com/blog/inside-azure-file-storage/)
- [Microsoft Azure'i teenus tutvustus](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
- [Microsoft Azure'i failide püsib ühendused](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
