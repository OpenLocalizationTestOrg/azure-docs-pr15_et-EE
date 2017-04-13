<properties
    pageTitle="Kuidas kasutada Teenusehaldus API (Python) - funktsioonide juhend"
    description="Saate teada, kuidas sooritada programmiliselt Python levinud teenuste haldamisega seotud toiminguid."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Kuidas kasutada Python: Teenusehaldus

> [AZURE.NOTE] Teenuse juhtimise API asendatakse uue ressursi halduse API, eelvaade väljaanne praegu saadaval.  Uue ressursi haldamine API Python kasutamise kohta leiate [Azure'i ressursihalduse dokumentatsiooni](http://azure-sdk-for-python.readthedocs.org/) üksikasju.

Sellest juhendist näitab, kuidas sooritada programmiliselt Python levinud teenuste haldamisega seotud toiminguid. **ServiceManagementService** klassi [Python Azure'i SDK](https://github.com/Azure/azure-sdk-for-python) toetab Programmeerimisjuurdepääs palju teenuste haldamisega seotud funktsioone, mis on [Azure klassikaline portaali] [ management-portal] (nt **luua, värskendada, ja kustutada pilveteenustega, kasutuselevõttu, andmeteenuste haldus ja virtuaalmasinates**). Seda funktsiooni saab kasulikud rakenduste, mida on vaja Programmeerimisjuurdepääs Teenusehaldus loomine.

## <a name="WhatIs"> </a>Mis on teenuste haldus
Teenuse juhtimise API pakub Programmeerimisjuurdepääs palju teenuse halduse funktsioonidele saadaval [Azure klassikaline portaali]kaudu[management-portal]. Azure'i SDK jaoks Python võimaldab teil hallata oma pilveteenustega ja salvestusruumi kontod.

Teenuse juhtimise API kasutamiseks peate [looma Azure'i konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Mõisted
Azure'i SDK jaoks Python murtakse [Azure'i teenuse juhtimise API][svc-mgmt-rest-api], mis on REST API-ga. Kõik API toimingud sooritatakse SSL ja üksteist autenditud X.509 v3 sertide kasutamine. Halduse teenuse loendile töötab Azure või otse Interneti rakendusest, mida saate HTTPS-i taotluse saata ja vastu võtta vastuse HTTPS teenuse raames.

## <a name="Installation"> </a>Installimine

Kõik selles artiklis kirjeldatud funktsioonid on saadaval on `azure-servicemanagement-legacy` paketti, mille saate installida pip abil. Täpsemat teavet (nt kui olete kasutanud Python), installimise kohta leiate käesoleva artikli: [Python installimist ja Azure SDK](../python-how-to-install.md)

## <a name="Connect"> </a>Kohta: Teenusehaldus ühendamine
Teenusehaldus lõpp-punkti loomiseks peate oma Azure tellimuse ID ja kehtiv management sert. Teie tellimuse ID [Azure klassikaline portaali]kaudu saate hankida[management-portal].

> [AZURE.NOTE] Azure'i SDK Python v0.8.0, kuna on võimalik kasutada serdid loodud OpenSSL kui töötab Windows.  See nõuab Python 2.7.4 või uuem versioon. Soovitame kasutajatele OpenSSL asemel pfx, alates tugi pfx serdid tõenäoliselt eemaldatakse tulevikus.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Windows/Mac/Linux (OpenSSL) halduse serdid
[OpenSSL](http://www.openssl.org/) abil saate luua oma serdi haldus.  Tegelikult kaks serdid, üks server loomiseks vajaliku (mõne `.cer` fail) ja ühte kliendi jaoks (on `.pem` faili). Loomiseks on `.pem` faili, käivitada:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Loomiseks on `.cer` serdi, käivitada:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Azure'i serdid kohta leiate lisateavet teemast [Serdid ülevaade Azure'i pilveteenustega jaoks](./cloud-services-certs-create.md). OpenSSL parameetrite täieliku kirjelduse leiate teemast [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html)dokumentatsioonis.

Pärast nende failide loomist peate üles laadida soovitud `.cer` Azure "Upload" toimingu "Seaded" menüü [Azure klassikaline portaali]kaudu faili[management-portal], ja märkige üles, kuhu salvestasite peate selle `.pem` faili.

Pärast seda, kui olete saanud oma tellimuse ID, loodud sert ja üles laadida soovitud `.cer` faili Azure, saate luua ühenduse Azure'i halduse lõpp-punkti, läbides tellimuse id ja tee on `.pem` **ServiceManagementService**faili:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Eelmises näites `sms` on **ServiceManagementService** objekt. Klassi **ServiceManagementService** on esmane ainekursuse abil saate hallata Azure teenused.

### <a name="management-certificates-on-windows-makecert"></a>Halduse serdid Windows (MakeCert)

Seadme abil saate luua halduse iseallkirjastatud serdi `makecert.exe`.  Avage **Käsuviip Visual Studio** **administraator** ja kasutage järgmist käsku, asendades *AzureCertificate* serdi nimi, mida soovite kasutada.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

See käsk loob selle `.cer` fail ja installib **isikliku** serdi salve. Lisateabe saamiseks vt [Serdid ülevaade Azure'i pilveteenustega jaoks](./cloud-services-certs-create.md).

Serdi loomist peate pärast üles soovitud `.cer` Azure "Upload" toimingu "Seaded" menüü [Azure klassikaline portaali]kaudu faili[management-portal].

Kui olete saanud oma tellimuse ID, loodud sert ja üles laadida soovitud `.cer` faili Azure, saate luua ühenduse Azure'i halduse lõpp-punkti saates tellimuse id ja serdi asukoha poes **isikliku** serdi **ServiceManagementService** (uuesti, Asendage *AzureCertificate* teie serdi nimi):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Eelmises näites `sms` on **ServiceManagementService** objekt. Klassi **ServiceManagementService** on esmane ainekursuse abil saate hallata Azure teenused.

## <a name="ListAvailableLocations"> </a>Kohta: loendis Saadaolevad asukohad

Loendis Saadaolevad majutusteenuste asukohad, kasutage funktsiooni **loendi\_asukohtade** meetod:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Kui loote pilveteenuses või salvestusteenus, peate sisestama õige asukoht. Funktsiooni **loendi\_asukohtade** meetod tagastab alati ajakohast praegu saadaval asukohtade loendit. Kirjutamise, on saadaval asukohad.

- Lääne Europe
- Põhja-Europe
- Kagu-Aasia
- Ida-Aasia
- Kesk-USA
- Põhja Kesk-USA
- Lõuna-, Kesk-USA
- Lääne USA.
- Ida-USA
- Jaapan Ida
- Jaapan Lääne
- Brasiilia Lõuna
- Austraalia Ida
- Austraalia kodutee

## <a name="CreateCloudService"> </a>Kohta: pilveteenus loomine

Kui rakenduse loomine ja käivitage see Azure, koodi ja konfiguratsiooni koos nimetatakse Azure [pilveteenuses][] (tuntud on *majutatud teenuse* Azure eelmiste versioonidega). Funktsiooni **loomine\_majutatud\_teenuse** meetodi abil saate luua uue majutatud teenuse majutatud teenuse nimi (mis peab olema kordumatu Azure), (automaatselt kodeeritud base64) sildi, kirjeldus ja asukoht.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Saate loetleda kõik majutatud teenuseid koos teie tellimus on **loendi\_majutatud\_teenuste** meetod:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Kui soovite konkreetse majutatud teenuse kohta teabe saamiseks, saate seda teha majutatud teenuse nime, et selle **saada\_majutatud\_teenuse\_atribuudid** meetod:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Pärast loomist mõnda pilveteenusesse, saate teenuse kood juurutada on **luua\_juurutamise** meetod.

## <a name="DeleteCloudService"> </a>Kohta: pilveteenus kustutamine

Saate kustutada pilveteenus teenuse nime, et selle **kustutamine\_majutatud\_teenuse** meetod:

    sms.delete_hosted_service('myhostedservice')

Enne teenuse kustutamiseks tuleb esmalt kustutada kõik juurutuste teenuse. (Vt [kohta: kustutamine juurutamine](#DeleteDeployment) üksikasjalikku.)

## <a name="DeleteDeployment"> </a>Kohta: kustutamine juurutamine

Kustutada juurutamine, kasutage funktsiooni **kustutamine\_juurutamise** meetod. Järgmises näites on kujutatud, kuidas kustutada nimega juurutamine `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Kohta: salvestusruumi teenuse loomine

[Salvestusteenus](../storage/storage-create-storage-account.md) annab teile juurdepääsu Azure [plekid](../storage/storage-python-how-to-use-blob-storage.md), [tabelite](../storage/storage-python-how-to-use-table-storage.md)ja [järjekorrad](../storage/storage-python-how-to-use-queue-storage.md). Salvestusruumi teenuse loomiseks peate nimi (vahel 3 ja 24 väiketähed ja kordumatute Azure'is) teenus, kirjeldus, sildi (kuni 100 märki, automaatselt kodeeritakse base64) ja asukoht. Järgmises näites on kujutatud salvestusruumi teenuse asukoht määramisega loomise kohta.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Eelmises näites teate mis olekut on **loomine\_salvestusruumi\_konto** toimingut saate tuua, läbides selle tulemiga **loomine\_salvestusruumi\_konto** abil soovitud **saada\_toiming\_olek** meetod.  

Saate loetleda salvestusruumi kontod ja nende atribuutide abil soovitud **loendi\_salvestusruumi\_kontod** meetod:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Kohta: teenuse salvestusruumi kustutamine

Saate kustutada salvestusruumi teenuse salvestusruumi teenuse nime, et selle **kustutamine\_salvestusruumi\_konto** meetod. Teenuse salvestusruumi kustutamine kustutab kõik andmed, mis on talletatud teenuse (plekid, tabelite ja järjekorrad).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Kohta: loendis Saadaolevad opsüsteemid

Loendis Saadaolevad majutusteenuste opsüsteemide, kasutage funktsiooni **loendi\_opsüsteem\_süsteemide** meetod:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Teise võimalusena saate kasutada funktsiooni **loendi\_opsüsteem\_süsteemi\_peredele** meetod, mis pakuvad opsüsteemide perekonnad:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Kohta: operatsioonisüsteemi pildi loomine

Kasutage pildi hoidlasse operatsioonisüsteemi pildi lisamiseks soovitud **lisada\_os\_pilt** meetod:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Loendi operatsioonisüsteemi pilte, mis on saadaval, kasutage funktsiooni **loendi\_os\_piltide** meetod. See sisaldab kogu platvormi pildid ja kasutajale pildid.

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Kohta: operatsioonisüsteemi pildi kustutamine

Kasutaja pildi kustutamiseks kasutage funktsiooni **kustutamine\_os\_pilt** meetod:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Kohta: luua virtuaalse masina

Virtuaalse masina loomiseks peate esmalt looma [pilveteenuses](#CreateCloudService).  Looge virtuaalse masina juurutamise abil soovitud **loomine\_virtuaalse\_masina\_juurutamise** meetod:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Kohta: virtuaalse masina kustutamine

Virtuaalse masina kustutamiseks esmalt kustutate juurutamise abil soovitud **kustutamine\_juurutamise** meetod:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Pilveteenusesse seejärel kustutada, kasutades funktsiooni **kustutamine\_majutatud\_teenuse** meetod:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Kuidas: Luua virtuaalse masina jäädvustatud virtuaalse masina pildilt

Jäädvustada VM pilt, mida esmalt kõne on **jäädvustada\_vm\_pilt** meetod:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Edasi, veenduge, et on edukalt jäädvustatud pildi, kasutage funktsiooni **loendi\_vm\_piltide** api, ja veenduge, et teie pilt kuvatakse tulemused:

    images = sms.list_vm_images()

Lõpuks luua virtuaalse masina pilti, kasutage selle **loomine\_virtuaalse\_masina\_juurutamise** meetod nagu varem, kuid seekord selle asemel funktsiooni vm_image_name edasi

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Jäädvustada Linux virtuaalse masina kohta lisateabe saamiseks lugege teemat [kuidas jäädvustada Linux virtuaalse masina.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Lisateavet selle kohta, kuidas jäädvustada Windowsi virtuaalse masina, lugege teemat [kuidas jäädvustada Windowsi virtuaalse masina.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Järgmised sammud

Nüüd, kui olete õppinud Teenusehaldus põhitõdesid, pääsete juurde [täieliku API dokumentides Azure'i Python SDK](http://azure-sdk-for-python.readthedocs.org/) ja keerukate ülesannete hõlpsalt python rakenduse juhtida.

Lisateavet leiate teemast [Python Arenduskeskus](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[pilveteenuses]:/services/cloud-services/

