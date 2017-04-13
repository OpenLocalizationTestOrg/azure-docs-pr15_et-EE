<properties
    pageTitle="Windowsi virtuaalmasinates piltide kohta | Microsoft Azure'i"
    description="Lisateavet piltide kasutamise koos virtuaalmasinates Windows Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Windowsi virtuaalmasinates piltide kohta

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Piltidega töötamiseks

Azure'i PowerShelli mooduli abil saate hallata pildid, mis on saadaval Azure tellimusele. Saate kasutada ka Azure klassikaline portaali mõne pildi ülesannete jaoks, kuid käsurea rohkem valikuid.


-   **Saada kõik pildid**:`Get-AzureVMImage`tagastab loendi kõigi piltide, mis on saadaval, kui teie praegune tellimus: piltide kui ka need, mida Azure'i või partnerid. See tähendab, et võite saada mahuka loendi. Järgmised näited lühemaks loendi hankimine.
-   **Saada pilt peredele**:`Get-AzureVMImage | select ImageFamily` saab loendi pilt peredele, kuvades stringide **ImageFamily** atribuut.
-   **Kõigi piltide kindla perekonnas**.`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **Otsige VM pildid**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` see toimib, filtreerides DataDiskConfiguration atribuut, mis kehtib ainult VM pildid. Selles näites filtreerib väljundi ainult silt ja pildi nimi.
-   **Üldise pildi salvestada**.`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Eriotstarbelisi pildi salvestamiseks**tehke järgmist.`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] OSState parameeter on nõutav, kui soovite luua VM, mis sisaldab andmeid ketast, samuti operatsioonisüsteemi ketas. Kui te ei kasuta parameeter, loob cmdlet OS pilti. Parameetri väärtus näitab, kas pilt on üldiste või spetsialiseerunud, vastavalt kas operatsioonisüsteem ketas on koostatud korduskasutuseks.
-   **Pildi kustutamiseks**tehke järgmist.`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Järgmised sammud

Samuti saate [luua Windowsi masina klassikaline portaalis](virtual-machines-windows-classic-tutorial.md)

