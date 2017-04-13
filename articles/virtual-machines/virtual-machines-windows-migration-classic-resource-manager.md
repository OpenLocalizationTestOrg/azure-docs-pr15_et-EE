<properties
    pageTitle="Platvormi toetatud migreerimise IaaS ressursside klassikaline Azure'i ressursihaldur | Microsoft Azure'i"
    description="Selles artiklis tutvustatakse platvormi toetatud migreerimise ressursside kaudu klassikaline Azure'i ressursihaldur"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>IaaS ressursside klassikaline Azure'i ressursihaldur migreerimise platvormi toetatud

Selles artiklis kirjeldame, kuidas me ei lubamine migreerimise infrastruktuuri teenus (IaaS) ressurssidena klassikaline ressursihaldur juurutamise mudelid. Lisateavet leiate [Azure'i ressursihaldur funktsioonid ja eelised](../azure-resource-manager/resource-group-overview.md)kohta. Me üksikasjalikult ressursid kaks juurutamise mudelid, mis eksisteerivad tellimuse virtuaalse võrgu-saidilt lüüside abil ühenduse loomise. 

## <a name="goal-for-migration"></a>Migreerimise eesmärk

Ressursihaldur võimaldab juurutamine keerukate rakendused Mallid, konfigureerib virtuaalmasinates abil VM laiendid ja juurdepääsu juhtimine ja sildistamiseks. Azure'i ressursihaldur sisaldab scalable, paralleelselt juurutamise virtuaalmasinates üheks kättesaadavus komplektid. Uus juurutamise mudel sisaldab ka elutsükli haldus Arvuta, võrgu ja salvestusruumi sõltumatult. Lõpuks on keskenduda turvalisus vaikimisi virtuaalmasinates virtuaalse võrku jõustamise lubamise kohta.

Klassikaline juurutamise mudel peaaegu kõik funktsioonid on toetatud Arvuta, võrgu ja salvestusruumi jaotises Azure'i ressursihaldur. Uued võimalused Azure'i ressursihaldur kasu, saate migreerida olemasoleva juurutuste klassikaline juurutamise mudel.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Teie automatiseerimine ning tööriistade pärast migreerimist muudatustest

Osana oma ressursse opsüsteemilt klassikaline juurutamise mudeli ressursihaldur juurutamise mudeli tuleb värskendada oma olemasoleva automatiseerimine või instrumentaarium tagada selle töö pärast migreerimist.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Migreerimise IaaS ressursside klassikalises ressursside halduri tähendus

Enne me süvitsiminek üksikasjad, vaatame andmete-suhtes ning haldus-plane toimingute IaaS ressursid vahe.

- *Juhtimise lennuk* kirjeldatakse kõnede, mis tulevad halduse lennuk või API muutmise ressursid. Näiteks toimingud nagu loomise VM, VM taaskäivitada ja värskendamisel virtuaalse võrgu uus alamvõrgu hallata töötava ressursid. Need otse ei mõjuta ühenduse linnanimede.
- *Andmete lennuk* (rakendus) kirjeldatakse käitusaja taotluse ja hõlmab eksemplarid, mis ei lähe Azure'i API kaudu suhtlemine. Juurdepääs veebisaidi või tõmbamine töötab SQL serveri eksemplar või MongoDB serveri andmed loetakse andmete tasapinnalised või rakenduse suhtlus. Salvestusruumi konto lisamine bloobimälu kopeerimist ja juurdepääs avaliku IP RDP või SSH virtual kohale ka andmete lennuk. Neid toiminguid hoida rakendus töötab üle Arvuta, võrgunduse ja salvestusruumi.

>[AZURE.NOTE] Mõnel juhul migreerimise Azure'i platvormi lõpetab, deallocates ja oma virtuaalmasinates taaskäivitamist. See andmesidekasutusele lühike andmete tasandiline tööseisakute.

## <a name="supported-scopes-of-migration"></a>Toetatud otsinguulatuste migreerimise

On kolme migreerimise otsinguulatuste, et peamiselt Arvuta, võrgu ja salvestusruumi. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migreerimise virtuaalmasinates (pole rakenduses virtuaalse võrk).

Mudelis ressursihaldur juurutamise turvalisus on tagatud oma rakenduste vaikimisi. Kõik VMs peavad olema virtuaalse võrku ressursihaldur mudel. Azure'i platvormi taaskäivitamist (`Stop`, `Deallocate`, ja `Start`) VMs migreerimise osana. Teil on kaks võimalust virtuaalse võrkude.

- Saate taotleda platvormi virtuaalse uue võrgu loomine ja virtuaalse masina uue virtuaalse emavõrku migreerida.
- Saate migreerida virtuaalse masina olemasoleva virtuaalse võrgu ressursihaldur sisse.

>[AZURE.NOTE] Migreerimise nimetatud nii haldus-plane toimingute ja andmete tasandiline toimingud pole lubatud migreerimisel aja jooksul.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migreerimise virtuaalmasinates (jaotises võrku virtuaalse).

Enamik VM konfiguratsioone, jaoks on ainult metaandmete migreerimine klassikaline ja ressursihaldur juurutamise mudelite vahel. Aluseks oleva VMs töötavad sama riistvara samasse võrku ning sama salvestusruumi. Haldus-plane toimingud pole lubatud teatud aja jooksul migreerimise jaoks. Andmete lennuk jätkab tööd. Mis on tekkida VMs (klassikaline) töötavad teie rakendused tööseisakute üleviimise ajal.

Järgmised pole praegu toetatud. Kui tugi on lisatud tulevikus mõned VMs selle konfiguratsiooni võivad tekkida tööseisakute (valige Peata, kuni deallocate ja taaskäivitage VM toimingud).

-   Teil on rohkem kui üks kättesaadavus määrata ühe pilveteenuses.
-   Teil on ühe või mitme kättesaadavus komplekti ja VMs, mis ei ole määrata ühe pilveteenuses olemasolu.

>[AZURE.NOTE] Migreerimise nimetatud halduse lennuk pole lubatud üleviimise ajal aja jooksul. Teatud konfiguratsioone, nagu on kirjeldatud varasemas versioonis, andmete tasandiline tööseisakute esineb.

### <a name="storage-accounts-migration"></a>Migreerimise salvestusruumi kontod

Sujuvalt migreerimise lubamiseks saate juurutada ressursihaldur VMs klassikaline salvestusruumi konto. Sellise võimalusega Arvuta ja ressursid saate ja peaks viiakse sõltumatult salvestusruumi kontod. Kui migreerite Virtuaalmasinates ja virtuaalse võrgu, peate oma salvestusruumi kontod migreerimise lõpule viia üle migreerimine. 

>[AZURE.NOTE] Ressursihaldur juurutamise mudel ei sisalda klassikaline pilte ja ketast. Kui salvestusruumi konto on migreeritud, klassikaline pilte ja ketast pole nähtavad ressursihaldur virnas, kuid varundamise VHDs jäävad salvestusruumi konto. 

## <a name="unsupported-features-and-configurations"></a>Toetuseta funktsioonid ja konfiguratsioone

Me ei toeta praegu mõned funktsioonid ja konfiguratsioone. Järgmistes jaotistes kirjeldatakse meie soovitused.

### <a name="unsupported-features"></a>Toetuseta funktsioonid

Järgmised funktsioonid pole praegu toetatud. Saate soovi korral eemaldage need sätted, VMs migreerimine ja seejärel uuesti lubamiseks ressursihaldur juurutamise mudeli sätted.

Ressursi pakkuja | Funktsioon
---------- | ------------
Arvuta | Seostamata virtuaalse masina ketast.
Arvuta | Virtuaalse masina pildid.
Võrgu | Lõpp-punkti ACL-ID.
Võrgu | Virtuaalse võrgu lüüside (saidilt, Azure'i ExpressRoute rakenduse lüüsi, valige käsk saidi).
Võrgu | Virtuaalne võrkude VNet silmitsemine abil. (ARM VNet migreerimine ja seejärel vastastikune) Lisateavet [VNet silmitsemine] (… /Virtual-Network/Virtual-Network-peering-overview.MD).
Võrgu | Liikluse haldur profiilid.

### <a name="unsupported-configurations"></a>Mittetoetatavad konfiguratsioone

Järgmised pole praegu toetatud.

Teenus | Konfigureerimine | Soovitused
---------- | ------------ | ------------
Ressursihaldur | Roll vastavalt juurdepääsu juhtimine (RBAC) klassikalises ressursside | Kuna URI ressursside muudetakse pärast migreerimist, on soovitatav, et kavatsete RBAC poliitika värskendusi, mis juhtub pärast migreerimist.
Arvuta | Mitu alamvõrku seostatud VM | Värskendage alamvõrgu konfiguratsioon, ainult alamvõrku viitamine.
Arvuta | Virtuaalmasinates, mis kuuluvad virtuaalse võrgus, kuid pole on määratud konkreetsete alamvõrgu | Soovi korral saate kustutada VM.
Arvuta | Teatised, Autoscale poliitika on | Migreerimise läheb läbi ja nende sätete kõrvaldatakse. See on soovitatav hinnata keskkonna enne migreerimise. Teise võimalusena saate konfigureerida Teatiste sätete pärast migreerimise lõpuleviimist.
Arvuta | XML-i VM laiendid (BGInfo 1.*, Visual Studio siluri, Web juurutada ja Remote silumine) | See ei toetata. Soovitatav on, et need laiendid eemaldamine virtuaalse masina migreerimise jätkamiseks või need kõrvaldatakse automaatselt migreerimise käigus.
Arvuta | Buutimine diagnostika Premium Storage | Enne jätkamist migreerimise VMs buutimine diagnostika funktsiooni keelata. Kui migreerimine on lõpule jõudnud, saate buutimine diagnostika virnas ressursihaldur uuesti lubada. Lisaks plekid, mis on kasutusel kuvatõmmis ja järjestikune logid tuleks kustutada nii, et need plekid enam ostmisega.
Arvuta | Pilveteenustega, mis sisaldavad web/töötaja rollid | See on praegu ei toetata.
Võrgu | Virtuaalse võrke, mis sisaldavad virtuaalmasinates ja web/töötaja rollid |  See on praegu ei toetata.
Azure'i rakendust Service | Virtuaalne võrkude sisaldavad rakenduse teenuse keskkonnas | See on praegu ei toetata.
Azure Hdinsightiga | Virtuaalne võrkude Hdinsightiga teenused sisaldavad | See on praegu ei toetata.
Microsoft Dynamics elutsükli teenused | Virtuaalne võrkude virtuaalmasinates, mida haldab Dynamics elutsükli teenused sisaldavad | See on praegu ei toetata.
Arvuta | Azure'i turbekeskus laiendid VNET, millel on VPN-lüüsi või kohapeal DNS-i server ER lüüsi abil | Azure'i turbekeskus installib automaatselt teie Virtuaalmasinates jälgida oma turvalisust ja tõsta teatiste laiendid. Need laiendid tavaliselt installitud automaatselt Azure'i turbekeskus poliitika lubamisel tellimust. Lüüsi migreerimise pole praegu toetatud ning lüüsi peab enne jätkamist toime migreerimise kustutada, VM salvestusruumi kontole Interneti-ühendus on kadunud lüüsi kustutamisel. Migreerimise ei jätku, kui see juhtub, kui külaline agent oleku bloobimälu ei saa täidetud. Soovitatav on keelamine Azure'i turbekeskus poliitika tellimust 3 tundi enne jätkamist migreerimise.

## <a name="the-migration-experience"></a>Migreerimise kogemus

Enne migreerimise käivitamine, soovitatakse järgmist:

- Veenduge, et ressursid, mida soovite migreerida tarbetute toetamata funktsioone või konfiguratsioone. Tavaliselt platvormi tuvastab järgmiste probleemide ja ilmneb tõrge.
- Kui teil on VMs, mis ei ole virtuaalse võrgus, siis need on peatatud ja deallocated ettevalmistamine osana. Kui te ei soovi kaotada avaliku IP-aadressi, uurida broneerimist käivitamise ettevalmistamine enne IP-aadress. Aga kui VMs on virtuaalse võrk, nad on ei lõpetanud ja deallocated.
- Migreerimise ajal mitte-tööaja mahutamiseks jaoks ootamatuid tõrkeid, mis võib juhtuda migreerimisel kavandamine.
- Laadige oma VMs konfiguratsioonile, kasutades PowerShelli, käsurea liides (CLI) käsud või REST API-de oleks lihtsam valideerimisreeglite pärast ettevalmistamine etapp on lõpule viidud.
- Värskendage oma automatiseerimine/tulemustabeli kasutuselevõtmise üle ka skriptide käsitlema ressursihaldur juurutamise mudeli enne migreerimise alustamist. Soovi korral saate teha toomine toimingute kui ressursid on valmis olekus.
- Hinnata RBAC poliitikat, mis on konfigureeritud IaaS klassikalises ressursside ja kavandamine pärast migreerimise lõpuleviimist.

Migreerimise töövoog on järgmiselt.

![Kuvatõmmis, mis näitab migreerimise töövoo](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Kõik toimingud, mida on kirjeldatud järgmistes jaotistes on idempotent. Kui teil on probleeme peale toetuseta funktsiooni või konfiguratsiooni tõrge, on soovitatav, et te proovige ettevalmistamine, katkestada või Kinnita toiming. Azure'i platvormi proovitakse toimingu uuesti.

### <a name="validate"></a>Kinnitage

Valideeri toiming on esimene samm migreerimisprotsessi. Selles etapis tuleb eesmärk on ressursid jaotises migreerimise taustal andmete analüüsimiseks ja tagastada juhul, kui ressursside migreerimise õnnestumise/tõrge.

Valite virtuaalse võrgu või majutatud teenuse (kui see ei ole virtuaalse võrgu), et soovite valideerida migreerimise jaoks.

* Kui migreerimise ei ressursile, Azure'i platvormi loetleb kõik põhjused miks see on toetatud migreerimise jaoks.

### <a name="prepare"></a>Ettevalmistamine

Ettevalmistamine on migreerimisprotsessi etapp. Selles etapis tuleb eesmärk on simuleerida IaaS ressursside klassikalises ressursside ressursihaldur transformatsioon ja esitamiseks see kõrvuti jaoks visualiseerimiseks.

Valite virtuaalse võrgu või majutatud teenuse (kui see ei ole virtuaalse võrgu), et soovite migreerimiseks ettevalmistamiseks.

* Kui migreerimine ei ressursile, Azure'i platvormi lõpetab migreerimisprotsessi ja loendite põhjus, miks ettevalmistamine nurjus.
* Kui ressursi suudab migreerimise, Azure'i platvormi esimese lukud alla ressursid migreerimise jaotises haldus-plane toimingud. Näiteks saate ei saa lisada andmed kettale jaotises migreerimise VM.

Azure'i platvormi seejärel algab metaandmete migreerimise klassikalises ressursside halduri migreerimine ressursid.

Kui ettevalmistamine on lõpule jõudnud, on teil võimalus nii klassikalises ressursside visualiseerimiseks ja ressursihaldur. Klassikaline juurutamise mudeli iga pilveteenuses Azure'i platvormi loob ressursi rühma nimi, mis on muster `cloud-service-name>-migrated`.

>[AZURE.NOTE] Virtuaalmasinates, mis ei ole klassikaline virtuaalse võrk on lõpetanud deallocated selles etapis migreerimise.

### <a name="check-manual-or-scripted"></a>Märkige ruut (käsitsi või skripti abil)

Sisse etapis, saate soovi korral varem allalaaditud konfiguratsiooni valideerimiseks migreerimise õiged. Teise võimalusena saate sisselogimiseks portaali ja rollide atribuudid ja ressursse, mis kinnitage metaandmete migreerimise sobib.

Kui virtuaalse võrgu migreerimist, enamik konfigureerimine virtuaalmasinates taaskäivitamist. Klõpsake need rakendused, saate kinnitada, et rakendus on endiselt ja töötab.

Saate testida oma jälgimine automatiseerimine ja funktsionaalseid skriptide VMs töötaksid oodatud ja oma värskendatud skriptide õigesti töötada. Ainult too toimingud on toetatud, kui ressursid on valmis olekus.

Ei ole aja akna, mille peate esmalt Kinnita migreerimise. Saate teha nii palju aega, kui soovite selle olekus. Siiski halduse lennuk on lukus need ressursid seni, kuni te katkestada või Kinnita.

Kui näete probleemidest, saate alati katkestada migreerimise ja minge tagasi klassikaline juurutamise mudel. Pärast uuesti, avatakse Azure'i platvormi ressursside haldus-plane toimingud, et need VMs klassikaline juurutamise mudeli tavaline toiminguid, võite jätkata.

### <a name="abort"></a>Kas soovite katkestada

Katkestamist on valikuline etapp, mille abil saate taastada klassikaline juurutamise mudeli muudatuste ja lõpetamise migreerimise.

>[AZURE.NOTE] Seda toimingut ei saa käivitada, kui teil on käivitanud Kinnita toiming.  

### <a name="commit"></a>Kinnitamine

Kui olete lõpetanud valideerimine, saate endale migreerimise. Ressursid ei kuvata enam klassikaline ja on saadaval ainult ressursihaldur juurutamise mudel. Uue portaalis saab hallata migreeritud ressursid.

>[AZURE.NOTE] See on toimingu idempotent. Kui see ei õnnestu, on soovitatav, et te proovige uuesti. Kui see ei lahene nurjumise, luua tugi Piletite või Foorum postitus sildiga ClassicIaaSMigration meie [VM Foorum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

**Migreerimise kava mõjutab ühte oma olemasolevad teenused või rakendused, mis töötavad Azure'i virtuaalmasinates?**

Ei. (Klassikaline) VMs on täielikult toetatud teenused üldiselt kättesaadav. Saate jätkata järgmiste ressursside kaudu saate oma andmed Microsoft Azure väiksemates vähem ruumi laiendamiseks.

**Mis juhtub minu vms, kui ma ei plaani lähitulevikus migreerimine?**

Meil on deprecating, olemasoleva klassikaline API-d ja ressursside mudel. Soovime teha migreerimise lihtne, arvestades täiustatud funktsioone, mis on saadaval ressursihaldur juurutamise mudeli. Soovitame, et teil läbi vaadata, [lisatasu osa](virtual-machines-windows-compare-deployment-models.md) , mis on osa IaaS jaotises ressursihaldur.

**Mida tähendab see migreerimise kavandamine minu olemasoleva instrumentaarium?**

Värskendada oma instrumentaarium ressursihaldur juurutamise mudeli on üks kõige olulisemad muudatused, mis tuleb arvestada plaanide migreerimise.

**Kui kaua haldus-plane tööseisakute olema?**

See sõltub ressursid, millest saab migreerida arv. Väiksem juurutuste (mõni kümneid VMs), peaks kogu migreerimise väiksem kui tunnis. Suuremahuliste juurutuste (sadu VMs) migreerimise võib kuluda mõni tund.

**Saate ma tööks uuesti pärast seda, kui minu migreerimine ressursid on kinnitatud ressursi halduris?**

Migreerimise saate katkestada, kui ressursid on valmis olekus. Pärast ressursside on migreeritud kaudu kinnitamine tagasipööramine ei toetata.

**Ma tagasi pöörata minu migreerimise kui toiming kinnitamine nurjub?**

Migreerimise ei saa katkestada, kui toiming kinnitamine nurjub. Migreerimise kõik toimingud, sealhulgas kinnitamine, idempotent. Seega soovitame mõne aja pärast toimingut. Kui teil on endiselt näo tõrge, luua tugi Piletite või Foorum postitus sildiga ClassicIaaSMigration meie [VM Foorum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

**Ei osta teise kiire marsruutimiseks ringi, kui mul on kasutada IaaS jaotises ressursihaldur?**

Ei. Me viimati lubatud [teisaldamine ExpressRoute topoloogia klassikaline ressursihaldur juurutamise mudeli kaudu](../expressroute/expressroute-move.md). Teil pole osta uue ExpressRoute ringi, kui teil on juba üks.

**Mida teha, kui oli konfigureeritud Rollipõhine juurdepääsu reguleerimine poliitikate minu IaaS klassikalises ressursside?**

Migreerimise ajal ressursside muudavad klassikalises ressursside haldur. Soovitame, et kavatsete RBAC poliitika värskendusi, mis juhtub pärast migreerimist.

**Mida teha, kui ma kasutan Azure saidi taastamise või Azure varukoopia täna?**

Virtuaalne arvuti varukoopia põhjal, lugege teemat [jaoks lubatud migreerimiseks on varundatud minu klassikaline VMs varukoopiate hoidla. Nüüd soovin migreerida minu VMs klassikaline režiim ressursihaldur režiim. Kuidas ma varundada neid rakenduses taastamise teenuste hoidla?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Saate ma kinnitada, et minu tellimus või ressursse kuvamiseks, kui need on võimalik migreerimise?**

Jah. Platvormi toetatud migreerimist valikul on esimene samm ettevalmistamine migreerimise kinnitada, et ressursside oskavad migreerimise. Juhuks, kui toiming valideerimine nurjub, saate sõnumeid ei saa migreerimise lõpule põhjustel.

**Mis juhtub, kui ma tekib kuvatakse tõrketeade kvoodi migreerimise IaaS ressursside koostamise ajal?**

Soovitame, et migreerimise katkestada ja seejärel logige esitada tugiteenuse taotluse suurendamiseks kvootide ala, kui migreerite VMs. Pärast kinnitamist kvoodi taotluse, saate alustada migreerimise juhiseid uuesti käivitamist.

**Kuidas teatada probleeme?**

Meie [VM Foorum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows), mis märksõna ClassicIaaSMigration postitada oma küsimusi ja migreerimise küsimused. Soovitame kõik küsimustele postitamine see foorum. Kui teil on tugiteenuste lepingu, olete Tere tulemast tugi Piletite ka sisse logida.

**Mida teha, kui ma ei meeldi platvormi ignoreeritud migreerimisel ressursside nimed?**

Ressursse, mis teil esitada nimede klassikaline juurutamise mudeli säilitatakse migreerimisel. Mõnel juhul on loodud uue ressursid. Näide: võrgu liidese luuakse iga VM. Me ei toeta praegu võimalus määrata migreerimisel loonud uue ressursid nimed. Logige oma häälte selle funktsiooni kasutamiseks [Azure tagasiside Foorum](http://feedback.azure.com).

* *sain sõnumi *"VM on teatamine üldine agent oleku mitte valmis. Seega ei viida VM. Veenduge, et VM Agent on teatamine üldine agent olek on valmis"* või *"VM sisaldab nime, kelle olek on mitte teatatud kaudu VM laiend. Seega selle VM, ei migreerita."***

See sõnum kui VM ei ole väljamineva Interneti-ühendus. VM agent kasutab Väljamineva meili Ühenduvus saavutamiseks Azure storage konto värskendamise oleku agent iga viie minuti järel.


## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui migreerimise klassikaline IaaS ressursside abil ressursihaldur, saate alustada migreerimine ressursid.

- [Tehnika deep dive platvormi toetatud migratsiooni klassikaline: Azure'i ressursihaldur](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [PowerShelli abil saate migreerida IaaS ressursse: klassikaline Azure'i ressursihaldur](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [CLI abil saate migreerida IaaS ressursid klassikaline Azure'i ressursihaldur](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Klassikaline virtuaalse masina Azure'i ressursihaldur klooni ühenduse PowerShelli skriptide abil](virtual-machines-windows-migration-scripts.md)
