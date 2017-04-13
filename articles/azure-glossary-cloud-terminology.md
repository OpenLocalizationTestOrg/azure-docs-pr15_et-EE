<properties
    pageTitle="Azure'i sõnastik - Azure sõnastiku | Microsoft Azure'i"
    description="Pilveteenuse seotud mõisteid Azure'i platvormil Azure sõnastik abil. Selles lühikeses Azure sõnastiku pakub levinud cloud tingimustel Azure."
    keywords="Azure'i sõnastiku, cloud terminoloogia, Azure sõnastik, terminoloogia määratlused, cloud tingimustel"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure'i sõnastik: pilveteenuste terminoloogia Azure'i platvormil sõnastiku

Microsoft Azure'i sõnastik on lühike sõnastiku cloud terminoloogia Azure'i platvormi.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Teenuse määratlused ja muude cloud terminite otsimine

* Kolleegidega leiate Azure'i teenuste ja nende AWS [Microsoft Azure ja Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

* Üldine valdkonna cloud terminite leiate [Cloud arvutuste tingimustel](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Azure'i sõnastik kahe ülaltoodud viidetega pakub Azure'i ja cloud industry soovitud taksonoomia lõpuni.  

## <a name="azure-glossary-list"></a>Azure'i sõnastik loend


### <a name="account"></a>konto  
Töö või kooli või isikliku Microsofti kontoga, mida kasutatakse juurdepääsuks ja haldamiseks Azure tellimuse.  
Vt ka, [Kuidas Azure'i tellimused on seostatud Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>kättesaadavus määramine  
Hallatud koos rakenduse koondamise ja töökindluse virtuaalmasinates kogum. Mõne kättesaadavus Sea kasutamine tagab, et kas planeerimata või kavandatud hooldustööde sündmuse ajal vähemalt üks virtuaalse masina on saadaval.  
Vt ka [haldamine Windows virtuaalmasinates kättesaadavus](./virtual-machines/virtual-machines-windows-manage-availability.md) või [Linuxi kättesaadavus haldamine](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Azure'i klassikaline juurutamise mudeli  
Üks kaks [juurutamise mudelite](resource-manager-deployment-model.md) juurutamine ressursid Azure (uus mudel on Azure ressursihaldur). Mõned Azure ressursse saab kasutada mudeli ühe või teise, samal ajal teistega saab kasutada nii mudelite. Üksikute Azure ressursid üksikasjad, kus model(s) ressursi juhised saab kasutada koos.


### <a name="cli"></a>Azure'i käsurea liides (CLI)  
Mõne [käsurea liides](xplat-cli-install.md) Azure'i teenuste haldamiseks Windows, OSX ja Linux arvutites kasutatud.


### <a name="powershell"></a>Azure'i PowerShelli  
[Käsurea liides](powershell-install-configure.md) haldamiseks arvutitest Windows Azure'i teenuste mõne käsurea kaudu. Mõned teenused või teenuse funktsioone saab hallata ainult PowerShelli või CLI kaudu. Juhised iga üksiku Azure ressursi üksikasjad, mis model(s) ressursi saab kasutada koos.   
Vt ka, [Kuidas installida ja konfigureerida Azure PowerShell](powershell-install-configure.md)


### <a name="arm-model"></a>Azure'i ressursihaldur juurutamise mudeli  
Üks kaks [juurutamise mudelite](resource-manager-deployment-model.md) juurutamine ressursid Microsoft Azure (teine on klassikaline juurutamise mudeli). Mõned Azure ressursse saab kasutada mudeli ühe või teise, samal ajal teistega saab kasutada nii mudelite. Juhised üksikute Azure ressursid üksikasjad, kus model(s) ressursi saab kasutada koos.


### <a name="fault-domain"></a>viga domeeni  
Kättesaadavus komplekti, mis võib nurjuda, samal ajal virtuaalmasinates kogum. Näide on masinad on sektsioon levinud power lähte- ja võrgu vahetamise jagavad rühma. Azure'i eraldatakse automaatselt viga mitu domeenides virtuaalmasinates mõne kättesaadavus komplekti.  
Vt ka [haldamine Windows virtuaalmasinates kättesaadavus](./virtual-machines/virtual-machines-windows-manage-availability.md) või [Linuxi kättesaadavus haldamine](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>Geo  
Andmete residentuuri, mis tavaliselt sisaldab kahe või enama piirkondade määratletud äärist. Piirid võib või mujalt piirid ja maksud määrus mõjutavad. Iga geo on vähemalt üks piirkond. Aasia Vaikse ookeani piirkonna- ja Jaapani on näiteks geos. Nimetatakse ka *geograafia*.  
Vt ka [Azure piirkondade](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>Geo-dispersioonanalüüs  
Protsessi automaatselt imitatsiooniga sisu nagu plekid, tabelite ja järjekorrad piirkondliku paari sees.  
Vt ka [Aktiivse Geo-kopeerimine Azure SQL-andmebaas](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>pilt  
Faili, mis sisaldab operatsioonisüsteemi ja rakenduse konfigureerimise, mida saab kasutada mis tahes arv virtuaalmasinates loomiseks. Azure on kahte tüüpi pildid: VM pilt ja OS pilt. VM pilt sisaldab operatsioonisüsteem ja kõik ketast virtuaalse masina lisatud pildi loomisel. Pildi OS sisaldab ainult generalized operatsioonisüsteem pole andmeid kettale konfiguratsioone abil.  
Vt ka [navigeerimine ja valige virtuaalse masina pildid Windows Azure'i PowerShelli või CLI](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>piirangud  
Ressursid, millest saab luua või jõudluse võrdlusalus, mida on võimalik olla number. Piirangud on tavaliselt seotud tellimuste, teenuste ja pakkumisi.  
Vt ka [Azure tellimuse ja teenuste piirangud, kvootide ja piirangud](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>Laadi koormusetasakaalustusteenuse  
Ressursi, mis jagab liiklust võrgus arvuti vahel. Laadi koormusetasakaalustusteenuse jaotab Azure, virtuaalmasinates määratletud laadi-koormusetasakaalustusteenuse set-liikluse. [Laadimine koormusetasakaalustusteenuse](./load-balancer/load-balancer-overview.md) võib olla Interneti-ühendusega või see võib olla sisemine.  


### <a name="offer"></a>pakkumine  
Funktsiooni hinnad, krediidi ja Azure tellimusega seotud tähtajad.  
Leiate [Azure'i lehel üksikasjade pakkumine](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>portaal  
Turvaline veebiportaali juurutada ja hallata Azure teenuseid kasutada.  On kaks portaalide: [Azure'i portaal](http://portal.azure.com/) ja [klassikaline portaalis](http://manage.windowsazure.com/). Mõned teenused on saadaval nii portaalide, samas kui teised on ainult ühe või teise saadaval. Milliseid teenuseid on saadaval, millised portaalis [Azure portaali-saadavus diagrammi](https://azure.microsoft.com/features/azure-portal/availability/) loendid.  


### <a name="region"></a>piirkond  
Geo, mis ei ole rist piirid ja see sisaldab ühe või mitme andmekeskuste alal. Hinnad, piirkondlike teenuste ja pakkumise tüüpi puutuvad piirkonna tasemel. Piirkonnas on üldjuhul ühendatud muu piirkond, mis võib olla mitu saja miili ära, kuni koondumist piirkondliku paari. Piirkondliku paari saab avariitaastet ja kättesaadavuse stsenaariumid süsteem. Nimetatakse ka üldiselt *asukoht*.  
Vt ka [Azure piirkondade](best-practices-availability-paired-regions.md)


### <a name="resource"></a>ressurss  
Üksuse, mis on osa teie Azure lahendus. Iga Azure'i teenus võimaldab teil kasutada erinevat tüüpi ressursid, nt andmebaasid või virtuaalmasinates.   
Vt ka [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>ressursirühm  
Container rakenduses ressursihaldur seotud ressursid rakenduse versioonimissätteid. Ressursirühma saate kaasata kõik ressursse rakenduse või ainult ressursse, mis on loogiliselt rühmitatud. Saate otsustada, kuidas soovite eraldada ressursid ressursside rühmadesse põhjal, milline on kõige mõistlikum oma ettevõtte jaoks.  
Vt ka [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>Ressursihaldur Mall  
JSON fail, mis määratleb deklaratiivse ühe või mitme Azure'i ressursid ja selle sõltuvuste juurutatud ressursid. Malli saab juurutada ressursside pidevalt ja korduvalt.  
Vt ka [loome Azure'i ressursihaldur Mallid](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>ressursi pakkuja  
Teenus, mis varustab ressursid saate juurutada ja hallata ressursihaldur kaudu. Iga ressursi pakkuja pakub toimingute juurutatud ressursside töötamiseks. Ressursi pakkujad pääseb Azure portaali ja Azure PowerShelli mitme programmeerimise SDK-d.  
Vt ka [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>roll  
Kasutajad, rühmad ja teenuste määratud juurdepääsu juhtimine vahendid. Rollid on võimalik toiminguid, näiteks luua, hallata ja Azure ressursid lugeda.  
Vt ka [RBAC: sisseehitatud rollid](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>teenuseleping (SLA)  
Microsofti nõuetelevastavust sees ja ühenduvuse jaoks kirjeldav leping. Iga Azure'i teenus on teatud SLA.  
Vt ka [Taseme lepingud](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>salvestusruumi konto  
Salvestusruumi konto, mis annab teile juurdepääsu Azure Storage Azure'i bloobimälu, järjekorra, tabeli ja faili teenuseid. Konto salvestusruumi pakub üheste nimeruumi oma Azure Storage andmeobjektid.  
Vt ka [kohta Azure salvestusruumi kontod](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>tellimuse  
Kliendi lepingu Microsofti, mis võimaldab hankida Azure teenused. Tellimuse hinnad ja seotud terminite reguleerib valitud tellimuse pakkumine. Vt [Microsofti veebiteenuste tellimuse leping](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Vt ka, [Kuidas Azure'i tellimused on seostatud Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>sildi  
Indekseerimise termin, mis võimaldab liigitamiseks ressursid vastavalt oma vajadustele haldamine või arveldus. Teil on keerukas kogumi ressursi rühmad ja ressursse, kui soovite visualiseerida vara nii, et kõige mõistlikum, saate sildid. Näiteks võib sildistamine ressursse, mis sarnast rolli Teeni teie asutuses või sama osakonna kuuluvad.  
Vt ka [kaudu siltide korraldamiseks oma Azure ressursid](resource-group-using-tags.md)


### <a name="update-domain"></a>Värskendage domeeni  
Kogum virtuaalmasinates mõne kättesaadavus komplekti, mis on värskendatud samal ajal. Kavandatud hooldustööde ajal virtuaalmasinates sama update domeeni koos taaskäivitada. Azure'i taaskäivitamist kunagi rohkem kui üks update domain korraga. Ka edaspidi on täiendamine domeeni.  
Vt ka [haldamine Windows virtuaalmasinates kättesaadavus](./virtual-machines/virtual-machines-windows-manage-availability.md) või [Linuxi kättesaadavus haldamine](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>virtuaalse masina  
Tarkvara rakendamise füüsilise arvuti, kus töötab operatsioonisüsteem. Mitme virtuaalmasinates saate käivitada samaaegselt sama riistvara. Azure'i virtuaalmasinates on saadaval erineva suurusega.  
Vt ka [Virtuaalmasinates dokumentatsioon](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>virtuaalse masina laiend  
Ressursi, mis rakendab käitumist või funktsioone ühte muudes programmides, töö- või pakkuda võimalust suhelda töötava arvutiga. Näiteks võite laiend VM juurdepääsu lähtestamine või Kaugpöördusteenuse väärtused Azure virtuaalne arvutis muutmiseks.  
Vt ka [virtuaalse masina laiendid ja funktsioonid (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) või [virtuaalse masina laiendid ja funktsioonid (Linux) kohta](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>virtuaalse võrgu  
Võrk, mis pakub oma Azure ressursse, mis on eraldatud kõik Azure rentnikud Ühenduvus. See saab ühendatud teiste Azure virtuaalse võrkudega läbi [Azure'i VPN-lüüsi](./vpn-gateway/vpn-gateway-about-vpngateways.md) ja kohapealse võrgu kaudu [mitu võimalust](./vpn-gateway/vpn-gateway-cross-premises-options.md). IP address plokid, DNS-i sätted, turbepoliitikate ja marsruutimiseks tabelite selles võrgustikus saate täielikult kontrollida.  
Vt ka [Virtuaalse võrgu ülevaade](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Vt ka**  
- [Azure'i kasutamise alustamine](https://azure.microsoft.com/get-started/)
- [Pilveteenuse ressursikeskus](https://azure.microsoft.com/resources/)  
- [Azure'i rakenduse business jaoks](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure'i oma andmekeskuses](https://azure.microsoft.com/overview/business-apps-on-azure/) 





