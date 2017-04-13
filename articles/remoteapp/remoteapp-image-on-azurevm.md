<properties
    pageTitle="Azure RemoteApp pilt, mis on Azure VM põhjal luua | Microsoft Azure'i"
    description="Saate teada, kuidas luua pilt Azure RemoteApp alustades on Azure virtuaalse masina järgi."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Luua vastavalt Azure virtuaalne arvutis Azure RemoteApp pilt

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Saate luua Azure virtuaalne arvutist Azure RemoteApp pildid (mis pikalt ühiskasutusse oma saidikogumi rakendused). Võite ka määrata, et lisasime Azure VM Pildigalerii, mis vastab kõigile Azure RemoteApp pildi virtuaalse masina pildi kasutamine – saate selle pildi VM alguspunktina oma VM, kui soovite. Otsige pilt "Windows Server kaugtöölaua seansi hosti" teegis.

On luua oma pildi põhjal on Azure VM – luua pilt ja seejärel laadige see teegist Azure VM Azure RemoteApp kaks toimingut.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Luua kohandatud pilt, mis on Azure VM põhjal

Järgmiste juhiste abil saate luua ka Azure VM põhjal pilt.

1. Saate luua ka Azure virtuaalse masina. Saate kasutada funktsiooni "Windows Server kaugtöölaua seansi hosti" või "Windows Server Remote Desktop seansi Host koos Microsoft Office 365 ProPlusi" Pilt galeriist Azure virtuaalse masina pilt. Sellel pildil vastab kõigile Azure RemoteApp malli pilti.

    Lisateavet leiate teemast [loomine VM, kus töötab Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Ühenduse loomine VM ja installima ja konfigureerima rakendused, mida soovite ühiskasutusse anda RemoteAppi kaudu. Veenduge, et teha mis tahes täiendavaid Windowsi konfiguratsioone nõutud rakendusi.

    Lisateavet leiate teemast [virtuaalse masina opsüsteemi Windows Server sisse logida](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. Kui kasutate ühte Windows Server kaugtöölaua seansi hosti pilte, on kaasatud valideerimine skripti, mis vastab teie VM RemoteApp eel-reqs. tagavad Skripti käivitamiseks topeltklõpsake töölaual **ValidateRemoteAppImage** . Veenduge, et kõik tõrkeid skripti on fikseeritud enne järgmise juhise juurde asumist.

4. SYSPREP üldistada ja jäädvustada pilt. Vaadake, [kuidas jäädvustada Windowsi virtuaalse masina kasutamine mallina](../virtual-machines/virtual-machines-windows-classic-capture-image.md) juhised.



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Pildi importimine pilditeegis Azure RemoteApp

Järgmiste juhiste abil saate importida Azure RemoteApp uus pilt:

1. Klõpsake menüüs **Malli pilti** :
    - Kui teil pole olemasolevaid pilte, klõpsake nuppu **üles laadida või importimine malli pilti**.
    - Kui teil on juba vähemalt üks pilt, klõpsake **+** uue pildi lisamiseks.

2. Valige **Impordi pilt oma Virtuaalmasinates** teek ja seejärel klõpsake nuppu **edasi**.

3. Järgmisel lehel Valige loendist oma kohandatud pilt ja veenduge, et olete täitnud oma pildi loomisel toodud juhised. Klõpsake nuppu **edasi**.
4. Sisestage uuele RemoteAppi pildile nimi ja valige asukoht ning klõpsake nuppu impordi käivitamiseks märke.

> [AZURE.NOTE] Saate importida pilte mis tahes toetatud Azure'i Virtuaalmasinates mis tahes Azure'i asukohta Azure RemoteApp ei toeta Azure asukohast. Sõltuvalt asukohad importimine võib kuluda kuni 25 minutit.

Nüüd olete valmis looma oma uue saidikogumi, kas [pilveteenuse](remoteapp-create-cloud-deployment.md) saidikogumi või [hübriid](remoteapp-create-hybrid-deployment.md), sõltuvalt teie vajadustele.
