<properties
   pageTitle="Juurdepääs VM ID"
   description="Kirjeldatakse juurdepääsemise ja kasutamise Azure'i VM Ainuidentifikaator"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Juurdepääs ja kasutamise Azure'i VM Ainuidentifikaator

Azure'i VM kordumatu ID on 128bits identifikaator kodeeritud ja kõik Azure IaaS VM SMBIOS talletatud ja saab praegu lugeda platvormi BIOS käskude abil.

Azure'i VM kordumatu ID on kirjutuskaitstud atribuudi. Taaskäivitage arvuti sulgemisel ei muuda Azure VM kordumatu ID (kas planeeritud planeerimata), Start/Stop tühistage eraldada, teenuse tervendav või taastada kohas. Kui VM on hetktõmmise ja luua uue eksemplari kopeeritud, konfigureeritakse uut Azure VM ID-d.

> [AZURE.NOTE] Kui teil on vanema VMs loodud ja töötab, kuna see uus funktsioon puhul sai palun välja (mai 18, 2014), võetud uuesti oma VM automaatselt saada on Azure kordumatu ID


Azure'i VM kordumatu ID VM juurdepääsemiseks tehke järgmist.


## <a name="create-a-vm"></a>Luua VM
 

Lisateavet leiate teemast [loomine virtuaalse masina](virtual-machines-linux-creation-choices.md)


## <a name="connect-to-the-vm"></a>Ühenduse loomine VM
 

Lisateavet leiate teemast [SSH Linux](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Päringu VM kordumatu ID

Käsk (näide kasutab **Ubuntu**):

    sudo dmidecode | grep UUID
    
Näide oodatud tulemusi:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Big Endian natuke tellimine, kuna tegelik VM kordumatu ID sel juhul on:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Azure'i VM Ainuidentifikaator saab kasutada erinevaid stsenaariumeid lähemalt kas VM Azure töötab või kohapealse- ja aitavad teie hulgilitsentsimise, aruannete või üldine jälitus nõuded peate oma Azure'i IaaS juurutuste. Mitme sõltumatu tarkvara tarnijate rakenduste loomine ja kinnitab need Azure võib olla nõutav on Azure VM kogu elutsükli tuvastamiseks ja kindlaks teha, kui töötab VM Azure, asutusesisese või muude teenusepakkujate pilve. See platvormi ID aitab näiteks tuvastada, kui tarkvara on õigesti litsentsitud või vastavusse viia VM andmete allikale, näiteks aidata õige mõõdikute jaoks õige platvorm seadmise kohta, et jälgida ja nende mõõdikute seas teised kasutusvõimalused oleksid spikker.
