<properties 
pageTitle="SAP-i INTERAKTIIVSET EHP7 SP3 juurutamine Microsoft Azure SAP ERP 6.0 jaoks | Microsoft Azure'i" 
description="SAP-i INTERAKTIIVSET EHP7 SP3 jaoks SAP ERP 6.0 Microsoft Azure juurutamine" 
services="virtual-machines-windows" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
keywords=""/> 
<tags 
ms.service="virtual-machines-windows" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-windows" 
ms.workload="infrastructure-services" 
ms.date="09/16/2016" 
ms.author="hermannd"/> 


# <a name="deploying-sap-ides-ehp7-sp3-for-sap-erp-60-on-microsoft-azure"></a>SAP-i INTERAKTIIVSET EHP7 SP3 jaoks SAP ERP 6.0 Microsoft Azure juurutamine 

Selles artiklis kirjeldatakse, kuidas SAP-i INTERAKTIIVSET töötab Windows OS ja SQL Server Microsoft Azure'i kaudu SAP-i Cloud seadme teegi 3.0 juurutamiseks. Ekraanipildid Kuva protsessi samm-sammult. Juurutamine muid lahendusi loendis töötab samal viisil seisukohast protsess. Lihtsalt on üks muu lahendus.

Alustamiseks avage SAP-i Cloud seadme teek (SAP-i CAL) [siin](https://cal.sap.com/). On ajaveebi kaudu SAP-i uue [SAP-i Cloud seadme teegi 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience)kohta. 


Järgmised kuvatõmmised näidata samm-sammult juurutamise SAP-i INTERAKTIIVSET Microsoft Azure. Protsess töötab samal viisil muid lahendusi.


![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

Esimese pildil kõik lahendusi, mis on saadaval Microsoft Azure. Esile on tõstetud SAP-i INTERAKTIIVSET Windowsi-põhise lahenduse, mis on saadaval Azure ainult valitud läbida protsess.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic2.jpg)

Esmalt on SAP-i CAL uue konto loomiseks. Praegu on kaks võimalust Azure - standard Azure ja Azure Hiina mandri, mis on, mida käitab 21Vianet partneri.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic3.jpg)

Seejärel üks on sisestage leiate Azure'i portaalis - vt ka täpsemaks alla hankimine Azure tellimuse ID-d. Hiljem sertifikaadi Azure halduse tuleb alla laadida.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic6.jpg)

Uue Azure portaali ühe leiab üksuse "Tellimused" vasakus servas. Klõpsake nuppu Kuva kõik aktiivse tellimused oma kasutaja jaoks.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic7.jpg)

Valige üks tellimused ja valides "Halduse serdid" selgitab, et seal on uus mõiste abil "teenuse põhisumma" uus Azure'i ressursihaldur mudel.
SAP-i CAL pole veel kohandatud selle uue mudeli ja nõuab veel "klassikaline" ja endise Azure portaali töötamiseks halduse serdid.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic4.jpg)

Siin näete ühte endise Azure portaali. Serdi haldus üles annab SAP-i CAL virtuaalmasinates jooksul kliendi tellimuse loomise õigus. Jaotises "Tellimused" leiate menüü ühte Tellimuse ID, mis tuleb kanda SAP-i CAL portaalis.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic5.jpg)

Teise kaardi, siis on võimalik üles laadida mõnelt SAP-i CAL enne allalaaditud serdi haldus.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic8.jpg)

Väike dialoogiboks kuvatakse allalaaditud serdi faili valimiseks.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic9.jpg)

Serdi üleslaaditud SAP-i CAL ja kliendi Azure tellimuse vahelise ühenduse saab katsetada SAP-i CAl sees. Väike sõnum peaks ilmub mis ütleb, et ühendus on lubatud.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic10.jpg)

Pärast konto häälestamist tuleb valida lahenduse, mis tuleks juurutada ja eksemplari loomine.
"Tavaline" režiimiga, on väga triviaalne. Sisestage eksemplari nimi, valige Azure piirkond ja lahendus juhtslaidi parooli määratlemine.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic11.jpg)

Olenevalt suurusest ja keerukusest (hinnang on antud SAP-i CAL) lahendus mõne aja pärast kuvatakse "Aktiivsed" ja kasutamiseks valmis. See on väga lihtne.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic12.jpg)

Vaadates ühte lahenduse mõned üksikasjad saate vaadata, millist tüüpi VMs olid juurutatud. Sel juhul on üks ühele Azure VM suurusega D12, mis on loodud SAP-i CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic13.jpg)

Azure'i portaalis, virtuaalse masina leiate alustades sama eksemplari nimi, mis anti SAP-i Cal.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic14.jpg)

Nüüd on võimalik lahendus nupp Ühenda SAP-i CAL portaali kaudu ühenduse loomine. Väike dialoogiboks sisaldab vaikimisi identimisteabe töötamiseks lahendus kirjeldav kasutusjuhend link.
[Siin](https://caldocs.hana.ondemand.com/caldocs/help/Getting_Started_Guide_IDES607MSSQL.pdf) on link juhend INTERAKTIIVSET lahendus.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Teine võimalus on Windows VM sisse logida ja alustada näiteks eelkonfigureeritud SAP-i GUI.





