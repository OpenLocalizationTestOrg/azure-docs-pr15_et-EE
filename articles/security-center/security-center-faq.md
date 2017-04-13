<properties
   pageTitle="Korduma kippuvad küsimused (KKK) Azure'i turbekeskus | Microsoft Azure'i"
   description="Sellest artiklist leiate vastused küsimustele Azure'i turbekeskus."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure'i turbekeskus korduma kippuvad küsimused (KKK)

Sellest artiklist leiate vastused küsimused Azure'i turbekeskus, teenus, mis aitab vältida, tuvastada ja neile vastamine ohtude suurendatud nähtavuse ja juhtida oma Microsoft Azure'i ressursse turvalisus.

## <a name="general-questions"></a>Üldised küsimused

### <a name="what-is-azure-security-center"></a>Mis on Azure turbekeskus?
Azure'i turbekeskus aitab vältida, avastada ja vastata ohtude suurendatud nähtavuse ja juhtida oma Azure ressursse turvalisus. Integreeritud turvalisus jälgimine ja rühmapoliitika haldus pakub oma tellimustes, aitab tuvastada ohud, mis võivad muidu märkamata ja töötab laialdane ökosüsteemi turvalisus lahendusi.

### <a name="how-do-i-get-azure-security-center"></a>Kuidas saan Azure'i turbekeskus?
Azure'i turbekeskus on lubatud Microsoft Azure'i tellimus ja [Azure portaali](https://azure.microsoft.com/features/azure-portal/)juurde. ([Portaali sisse logida](https://portal.azure.com), valige **Sirvi**ja liikuge **Security Center**).  

## <a name="billing"></a>Arveldamine

### <a name="how-does-billing-work-for-azure-security-center"></a>Kuidas Azure'i turbekeskus arveldustoe töö?
Turbekeskus on saadaval kaks astme: tasuta- ja standardversiooni.

Tasuta taseme võimaldab teil määrata turbepoliitikate ja vastu võtta Turbeteatiste, juhtumite ja soovitused, mis juhendab teid protsessi konfigureerimine vajalik juhtelement. Tasuta taseme abil saate ka jälgida oma Azure ressursid ja partnerite lahenduste Azure tellimuse integreeritud turvalisus seisund.

Standardse taseme pakub tasuta taseme funktsioone pluss täiustatud avastused: ärianalüüsi, käitumise analüüs, krahh analüüsi ja normaalne tuvastamise. Normaliseeritud tasandi 90 päeva tasuta prooviversioon on saadaval. Valige uuendamiseks [turbepoliitika](security-center-policies.md#setting-security-policies-for-subscriptions)hinnad taseme. Lisateavet vt [turbekeskus hinnad](security-center-pricing.md) .

## <a name="data-collection"></a>Andmete kogumine

Turbekeskus kogub andmeid oma virtuaalmasinates hinnake oma turvalisus oleku, soovituste turvalisus ja märku, et ohtude. Kui kasutate esmalt turbekeskus, andmete kogumine on lubatud kõigi virtuaalmasinates teie tellimus. Soovitatav on kasutada andmete kogumine, kuid te saate loobuda [keelamine andmete kogumine](#how-do-i-disable-data-collection) turbekeskus poliitika.

### <a name="how-do-i-disable-data-collection"></a>Kuidas keelata andmete kogumine?

Saate keelata **andmete kogumine** turbepoliitika tellimuse igal ajal. ([Azure portaali sisselogimine](https://portal.azure.com), valige **Sirvi**, valige **Turbekeskus**ja valige **poliitika**.)  Kui valite tellimuse, uue tera avaneb ja pakub teile võimalust **andmete kogumine** välja lülitada. Valige suvand **kustutamine agentide** ülemise lindi olemasoleva virtuaalmasinates agentide eemaldamiseks.

> [AZURE.NOTE] Turbepoliitikate saate seada ressursi töörühma tasandil ja Azure tellimuse tasandil, kuid tellimuse andmete kogumine väljalülitamiseks valige.

### <a name="how-do-i-enable-data-collection"></a>Kuidas lubada andmete kogumine?
Saate lubada andmete kogumine oma Azure'i subscription(s) turbepoliitika sisse. Andmete kogumine lubamiseks [Azure portaali sisselogimine](https://portal.azure.com)valige **Sirvi**, valige **Turbekeskus**ja valige **poliitika**. Määratud **andmete kogumine** **sisse** ja salvestusruumi kontod, kuhu soovite andmete kogumine, et konfigureerida (vt küsimus "[oma andmete talletuskoht?](#where-is-my-data-stored)"). Kui **andmete kogumine** on lubatud, see automaatselt kogub konfiguratsiooni ja sündmuse teabe kõigi toetatud virtuaalmasinates tellimus.

> [AZURE.NOTE] Turbepoliitikate saate seada ressursi töörühma tasandil ja Azure tellimuse tasandil, kuid konfiguratsiooni andmete kogumine ilmneb ainult tellimuse tasemel.

### <a name="what-happens-when-data-collection-is-enabled"></a>Mis juhtub, kui andmete kogumine on lubatud?
Andmete kogumine on lubatud Azure jälgimise Agent ja Azure väärtpaberi jälgimise laiendamise kaudu. Azure'i turvalisus jälgimise laiend otsib erinevate turvalisuse oluline konfiguratsiooni ja saadab [Windowsi sündmuse jälitamise](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) jälgi. Lisaks operatsioonisüsteemi loob sündmuselogi kirjeid.  Azure'i jälgimise Agent loeb sündmuselogi kirjeid ja ETW jälgi ja kopeerib need kontole salvestusruumi analüüsi jaoks.  See on salvestusruumi konto, saate konfigureerida turbepoliitika. Salvestusruumi konto kohta leiate lisateavet jaotisest "[oma andmete talletuskoht?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Jälgimine Agent või turvalisus jälgimise laiend ei mõju minu server jõudlus?
Agent ja -laiendiga tarbib nominal hulgal süsteemiressursside ja peaks olema mingit mõju avaldada.

### <a name="where-is-my-data-stored"></a>Kus mu andmed talletatakse?
Iga piirkonna, kus teil on virtuaalmasinates töötab, saate valida salvestusruumi konto nende virtuaalmasinates kogutud andmete talletuskoht. See muudab lihtne hoida andmed samas piirkonnas privaatsuse ja andmete suveräänsuse eesmärgil. Tellimusele salvestusruumi konto turvalisus poliitika valimine ([Azure portaali sisselogimine](https://portal.azure.com), valige **Sirvi**, valige **Turbekeskus**ja valige **poliitika**.) Tellimuse klõpsamisel avatakse uus laba. Valige piirkonnas valimiseks **Valige salvestusruumi kontod** .

> [AZURE.NOTE] Turbepoliitikate saate seada ressursi töörühma tasandil ja Azure tellimuse tasandil, kuid valides piirkonnas salvestusruumi konto jaoks esineb ainult tellimuse tasemel.

Azure'i salvestusruumi ja salvestusruumi kontode kohta leiate lisateavet teemast [Salvestusruumi dokumentatsiooni](https://azure.microsoft.com/documentation/services/storage/) ja [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Azure'i turbekeskus abil

### <a name="what-is-a-security-policy"></a>Mis on saanud turbepoliitika?
Turvalisuse poliitika määratleb juhtelementide, mis on soovitatav ressursid määratud tellimuse või ressursirühma. Azure'i turbekeskus, saate määratleda poliitikate Azure'i tellimused ja ressursside rühmad vastavalt oma ettevõtte turbenõuetele ja rakenduste tüübi või tundlikkust iga tellimuse andmed.

Näiteks ressurssidega või test võib olla erinevad turbenõuetele kasutatavad tootmise rakendused. Samuti rakenduste reguleeritud andmetega, nt PII (isiku tuvastamist võimaldav teave) võib olla vaja kõrgema taseme turvalisus. Turbepoliitikate, lubatud Azure turbekeskus, suunatakse turvalisus soovitused ning jälgimist. Turbepoliitikate kohta leiate lisateavet teemast [väärtpaberi seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md).

> [AZURE.NOTE] Korral tellimuse poliitika ja ressursside taseme rühmapoliitika konflikt resource taseme rühmapoliitika ülimuslik.

### <a name="who-can-modify-a-security-policy"></a>Kes on turbepoliitika saab muuta?
Turbepoliitikate on konfigureeritud iga tellimuse või ressursirühma. Turbepoliitika, tellimuse tasemel või ressursside töörühma tasandil muutmiseks peate olema kaasautor selle tellimuse omanik või.

Konfigureerimine turvalisuse poliitika kohta leiate teavet teemast [säte turbepoliitikate Azure'i Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Mis on turvalisus soovitus?
Azure'i turbekeskus analüüsib oma Azure turvalisus seisundi. Kui võimalik turvahaavatavusi tuvastatakse, luuakse soovitusi. Soovitused juhatavad teid protsessi konfigureerimine vajalik juhtelement. Näited:

- Ettevalmistamise ründevaratõrje tuvastamiseks ja ründetarkvara eest eemaldada.
- [Võrgu turberühmad](../virtual-network/virtual-networks-nsg.md) ja reegleid juhtelement liiklust virtuaalmasinates konfigureerimine
- Rakenduse web tulemüür aidata oma veebirakenduste suunamise eest kaitsta ettevalmistamine
- Rakendades puuduvad süsteemi värskendused
- Adressaatide valimiseks OS konfiguratsioone, mis vastavad soovitatav võrdlusandmeid?

Ainult soovitused, mis on lubatud turbepoliitikate on näidatud siin.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Kuidas näeb minu Azure ressursse hetkeseisu turvalisus?
**Ressursside seisund** paani **Turbekeskus** enne kuvatakse üldine turvalisus asendi oma keskkonna virtuaalmasinates, veebirakenduste ja muud ressursid kaupa. Iga ressursi on mõni indikaator näitab, kui mis tahes võimalikud turvalisus nõrkade selgitanud. Paani ressursid seisund oma ressursse ja iga kui ülesanne nõuab tähelepanu või esineb probleeme.

### <a name="what-triggers-a-security-alert"></a>Mis käivitab turvalisus teatise?
Azure'i turbekeskus automaatselt kogub, analüüsib ja kaitsmed log andmeid oma Azure ressursse, võrgu ja partnerilahenduste nagu ründevaratõrje ja tulemüürid. Ohtude tuvastamisel luuakse turvalisus teatis. Näited tuvastamise.

- Rikutud virtuaalmasinates suhtlemine teadaolevad pahatahtlik IP-aadressid
- Täiustatud ründevara abil Windowsi tõrketeavitus
- Jõuvõtete vastu virtuaalmasinates
- Integreeritud partnerilahenduste turvalisus nt ründevaratõrje või Web rakenduse tulemüürid turvaviipu

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Mis vahe on ohtude tuvastada ja Microsoft Security Response Center ja Azure turbekeskus, klõpsake hoiatus?
Microsofti turvalisus vastuse Center (MSRC) sooritab, valige turvalisus jälgimine ja Azure võrgu taristu ja võtab vastu ohtu ärianalüüsi ja kuritarvitamise kaebuste tootjatele. Kui MSRC saab teada, et kliendiandmete on külastatud ebaseadusliku või volitamata isiku või kliendi kasutamine Azure ei vasta tingimustega jaoks lubatud kasutada, teavitab turvalisuse langeva manager kliendi. Teatis ilmneb tavaliselt, saates e-posti turvalisus kontaktid Azure'i turbekeskus või Azure tellimuse omanik määratud kui turvalisus kontakti pole määratud.

Turbekeskus on Azure'i teenus, mis pidevalt jälgib Azure kliendikeskkond ja kehtib analytics mitmesuguseid potentsiaalselt ohtliku tegevuse automaatseks tuvastamiseks. Need avastused on uurinud Turbeteatiste turbekeskus armatuurlaua nimega.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Kuidas käsitletakse õiguste Azure'i turbekeskus?
Azure'i turbekeskus toetab Rollipõhine juurdepääsu. Rollipõhine juurdepääsu reguleerimine (RBAC) Azure kohta leiate lisateavet teemast [Azure Active Directory rolli põhise juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md).

Kui kasutaja avab turbekeskus, ainult soovitused ja teatised, mis on seotud ressursid kasutajal on juurdepääs on näha. See tähendab, et kasutajad näevad ainult üksused, mis on seotud ressursid, kus kasutajale on määratud roll omanik, kaasautor või lugeja tellimuse või ressursi kuuluva ressursirühma.

Kui teil on vaja:

- **Turvalisuse poliitika redigeerimine**, peate olema kaasautor tellimuse omanik või.
- **Rakenda soovituse**, peate olema kaasautor tellimuse omanik või.
- **Nähtavuse kõigis oma tellimuste olekusse turvalisus on**, peate olema omanik, kaasautor või iga tellimuse lugeja (IT-administraator, meeskonnatöö turvalisus).
- **On teie turvalisus seisundi nähtavus**, peate olema ressursirühm omanikule, kaasautor või lugeja (DevOps).

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Azure'i ressursside on Azure turbekeskus jälgida?
Azure'i turbekeskus jälgib Azure järgmistest allikatest:

- Virtuaalmasinates (VM) (sh [Pilveteenustega](../cloud-services/cloud-services-choose-me.md))
- Azure'i
- Azure SQL-i teenus
- Partnerite lahenduste integreeritud Azure tellimuse, nt web rakenduse tulemüüri VMs ja [Rakenduse teenuse keskkonnas](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtuaalmasinates

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Mis tüüpi virtuaalmasinates on toetatud?
Turvalisus seisundi jälgimine ja soovitused on saadaval loodud abil nii [klassikaline ja ressursihaldur juurutamise mudelite](../azure-classic-rm.md)virtuaalmasinates (VMs).

Toetatud Windows VMs:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

Toetatud Linux VMs:

- Ubuntu versioonide 12.04, 14.04, 16.04
- Debian versioonid 7, 8
- CentOS versioonid 6. \*, 7.*
- Punane rolli Enterprise Linux (RHEL) versioonide 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) versioonide 11. \*, 12.*
- Oracle'i Linuxi versioonid 6. \*, 7.*

Toetatud on ka VMs töötab mõnes pilveteenuses. Ainult cloud services valmistamisel jälgitakse slots töötab veebi ja töötaja rollid. Pilveteenuses kohta leiate lisateavet teemast [pilveteenustega ülevaade](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Miks ei tuvasta Azure'i turbekeskus ründevaratõrje lahendus minu Azure VM töötab?

Azure'i turbekeskus on ainult nähtavus ründevaratõrje installitud kaudu Azure'i laiendid. Näiteks turbekeskus ei saa tuvastada ründevaratõrje, mis oli eelinstallitud andsite pilt või kui installisite ründevaratõrje oma virtuaalmasinates abil oma protsesside (nt konfiguratsiooni halduse süsteemid).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Miks kuvatakse sõnum "Puuduvad andmed skannida" minu VM jaoks?

See võib võtta aega (tavaliselt vähem kui tund) pärast andmete kogumine on lubatud Azure turbekeskus asustamiseks skannimine andmete jaoks. Skannib ei asustada vms peatatud olekus.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Miks kuvatakse teade "VM Agent on kadunud?"

VM Agent peab olema installitud VMs selleks, et andmete kogumine. Vaikimisi installitakse VM Agent vms Azure'i turuplatsilt juurutatud. Lisateavet selle kohta, kuidas installida muid VMs VM Agent, leiate ajaveebipostitusest [VM Agent ja laiendid](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
