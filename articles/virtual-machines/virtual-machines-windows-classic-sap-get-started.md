<properties
   pageTitle="SAP-i kasutamine Windows virtuaalmasinates | Microsoft Azure'i"
   description="Tühjenda Windows virtuaalmasinates (VM) Microsoft Azure SAP-i kasutamise kohta"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>SAP-i kasutamine virtuaalmasinates Windows Azure

Pilvepõhine arvutuste on levinud termin, mis on rohkem tähtsus IT valdkonna, väikeettevõtete suured ja rahvusvahelised ettevõtted ülespoole kaudu. Microsoft Azure'i on Cloud Services platvormi Microsoftilt, mis pakub veel mitmesuguseid uusi võimalusi. Nüüd kliendid saavad kiiresti ette ja lahkumise rakenduste pilveteenustega, kui nii, et need pole piiratud tehnilist või eelarve piirangud. Aja ja eelarve investeerida riistvara taristu asemel ettevõtete saate keskenduda rakendus, äriprotsesside ja selle kasu klientide ja kasutajate jaoks.

Microsoft Azure'i virtuaalmasinates, pakub Microsoft täielik taristu platvormi teenus (IaaS). SAP-i põhiste NetWeaver rakendused on toetatud klõpsake Azure'i Virtuaalmasinates (IaaS). Tehnilised ülevaated, nagu on allpool kirjeldatud, kuidas kavandada ja rakendada SAP NetWeaver põhinev rakenduste virtuaalmasinates Windows Azure. Saate rakendada ka SAP NetWeaver põhinev rakenduste [Linuxi](virtual-machines-linux-classic-sap-get-started.md).

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver Azure - HA

Pealkiri: SAP NetWeaver Azure - rühmitamise SAP-i ASCS/SCS eksemplari koos SIOS DataKeeper Azure tõrkesiirdeklastrite Windows Serveri kasutamine

Kokkuvõte: "selles dokumendis kirjeldatakse, kuidas kasutada SIOS DataKeeper tugevalt saadaval SAP-i ASCS/SCS konfiguratsiooni Azure häälestamine. SAP-i kaitseb tõrge komponendid nagu SAP-i ASCS/SCS või nimekirja lisamine Dispersioonanalüüs teenuseid koos Windows Server tõrkesiirdeklastrite konfiguratsioone, mis nõuavad ühiskasutusega ketast ühe punkti. SAP-i järgmised komponendid on olulised SAP-i süsteemi funktsioone. Kõrge-saadavus funktsionaalsus peab seega veenduge, et need komponendid saate säilitada tõrke serveri või VM lõpuleviiduks Windows kobar konfiguratsioone tühjal metallist ja Hyper-V keskkonnas tuleb luua. August 2015 Azure'i endale ei saa anda ühiskasutusse antud ketast, mida oleks vaja Windowsi vastavalt tugevalt saadaval konfiguratsioone nõutav need kriitiline SAP-i komponendid. Tänu toote DataKeeper SIOS alusel, saate Windows Server tõrkesiirdeklastrite konfiguratsioone SAP-i ASCS/SCS vajadusel siiski ehitatud Azure'i IaaS platvormi. See paberi kirjeldatakse samm-sammult lähenemisviisi installimine Windows Server tõrkesiirdeklastrite konfigureerimine ühiskasutusega kettale SIOS Datakeeper Azure järgi. Paberi selgitab konfiguratsioone Azure, Windowsi ja SAP-i, mis teeb kõrge-saadavus konfiguratsiooni üksikasjad töötama optimaalse viisil. Paberi täiendab SAP-i installimise dokumentatsiooni ja SAP-i märkmeid, mis tähistavad esmane ressursside installid ja SAP-i tarkvara juurutuste kohta antud platvormide jaoks loodud.

Värskendatud: Augustis 2015

[Selle juhendi allalaadimine](http://go.microsoft.com/fwlink/?LinkId=613056)
