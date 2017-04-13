<properties
   pageTitle="Windows Serveri Active Directory Azure'i Virtuaalmasinates juurutamise juhised | Microsoft Azure'i"
   description="Kui teate, kuidas kasutada AD domeeniteenused ja AD Federation Services kohapealse, selgeks nende kasutamise kohta Azure'i virtuaalmasinates."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Juhised Windows Server Active Directory juurutuse kohta Azure'i virtuaalmasinates

Selles artiklis selgitatakse, rakendades Windows Server Active Directory Domain Services (AD DS) ja Active Directory Federation Services (AD FS) kohapealse võrreldes, rakendades neile sisse Microsoft Azure'i virtuaalmasinates erinevusi.

## <a name="scope-and-audience"></a>Ulatuse ja sihtrühma

See artikkel on mõeldud neile juba kogenud kohapealse Active Directory juurutamine. See hõlmab Microsoft Azure'i virtuaalmasinates/Azure virtuaalse võrgu ja traditsioonilise kohapealse Active Directory juurutuse Active Directory juurutuse erinevused. Azure'i virtuaalmasinates ja Azure'i on osa taristu-kui-a-teenuse (IaaS) pakub ettevõtete jaoks võimendada ressursside pilveteenuses.

Neile, kes ei tunne AD juurutus, lugege teemat [AD DS juurutusjuhend](https://technet.microsoft.com/library/cc753963) või [juurutamise AD FS-i](https://technet.microsoft.com/library/dn151324.aspx) vastavalt vajadusele.

Selles artiklis eeldatakse, et lugeja tuttavat järgmist põhimõtet:

- Windows Server AD DS juurutus- ja haldus
- Juurutamise ja Windows Server AD DS taristu toetamiseks DNS-i konfigureerimine
- Windows Server AD FS-i juurutamist ja haldus
- Juurutamine, konfigureerimise ja haldamise tuginedes tootjate rakendused (veebilehed ja teenused), mida saab kasutada Windows Server AD FS märgid
- Üldine virtuaalse masina mõisted, näiteks virtuaalse masina, virtuaalse ja virtuaalse võrgu konfigureerimine

Selles artiklis on hübriidjuurutuse stsenaarium Windows Server AD DS või AD FS-i on osaliselt juurutatud asutusesisese ja osaliselt juurutatud Azure'i virtuaalmasinates nõuded tõstab esile. Dokumendi esmalt hõlmab kriitiline erinevused töötab Windows AD DS- ja AD FS Server Azure'i virtuaalmasinates kohapealse ja olulisi otsuseid, mis mõjutavad kujundus ja juurutamise. Ülejäänud dokumendis selgitatakse juhised otsust punktide täpsemalt ja erinevate juurutamise stsenaariumide suuniste rakendamise kohta.

Selles artiklis käsitletakse konfiguratsiooni [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), mis on ülejäänud põhinev teenus, mis pakub identiteetide haldus ja juurdepääsu juhtimine võimalusi pilve rakendused. Azure Active Directory (Azure AD) ja Windows Server AD DS on, aga mis koos toimides lahenduse identiteedi ja juurdepääsu juhtimine ette tänase hübriid IT keskkonnas ning tänapäevane. Selleks, et mõista erinevusi ja Windows Server AD DS ja Azure AD vahelisi seoseid, arvestage järgmist.

1. Võib käivitate Windows Server AD DS pilveteenuses Azure'i virtuaalmasinates kasutamisel Azure'i laiendada oma kohapealse andmekeskuse pilve.
2. Võite kasutada Azure AD anda oma kasutajate ühekordse sisselogimise tarkvara-kui-a-Service (SaaS) rakenduste. Microsoft Office 365 kasutab seda tehnoloogiat näiteks ja Azure või muude platvormide jaoks loodud cloud töötavad rakendused saate kasutada ka see.
3. Identiteedid Facebooki, Google, Microsoft ja muude identiteedipakkujad rakendustele, mis on majutatud kohapealse ja pilveteenuse kasutamisel võite kasutada lasta kasutajatel logida Azure AD (selle juurdepääsu juhtimine teenus).

Erinevuste kohta leiate lisateavet teemast [Azure identiteedi](fundamentals-identity.md).

## <a name="related-resources"></a>Seotud ressursid

Võite alla laadida ja käivitada [Azure virtuaalse masina valmisoleku hindamine](https://www.microsoft.com/download/details.aspx?id=40898). Hindamise automaatselt kontrollida oma kohapealse keskkonna ja luua kohandatud aruande põhjal on leitud selles teemas aitavad migreerimine keskkonna Azure juhised.

Soovitame teil esmalt ka üle vaadata õpetused, juhikutele ja videoid, mis hõlmab järgmisi teemasid:

- [Azure'i portaalis vaid virtuaalse võrgu konfigureerimine](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [Azure'i portaalis VPN saitide konfigureerimine](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Uue Active Directory kogumis installida Azure virtuaalse võrgus](active-directory-new-forest-virtual-machine.md)
- [Azure Active Directory koopia domeenikontrolleri installimine](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure'i IT-spetsialistidele IaaS: (01) virtuaalse masina põhialused](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure'i IT-spetsialistidele IaaS: (05) loomise virtuaalne võrkude ja asukohaga Ühenduvus](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Sissejuhatus

Peamised nõuete kohta Azure'i virtuaalmasinates Windows Server Active Directory juurutuse erinevad väga vähe juurutamise see kohapealse virtuaalmasinates (ja teatud määral füüsilise masinad). Näiteks puhul Windows Server AD DS, kui juurutamist Azure'i virtuaalmasinates domeenikontrollerite (DCs) on olemasoleva kohapealse domeeni mets, ettevõtte seejärel Azure juurutamise võib käsitleda suuresti samamoodi nagu te võib käsitleda muude Windows Serveri Active Directory täiendava saidi. Mis on määratletavad alamvõrku Windows Server AD DS, saidi loonud, alamvõrku lingitud selle saidi ja muude saitide vastav sait-linkide kaudu ühendatud. Siiski on mõned erinevused ühised kõik Azure juurutuste ja mõned, mis sõltuvad konkreetse juurutamise stsenaariumi. Allpool on kirjeldatud kaks Peamised erinevused:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure'i virtuaalmasinates võib vaja ühenduvust kohapealse ettevõttevõrguga.

Azure'i virtuaalmasinates tagasi ühenduse mõne kohapealse ettevõtte võrgus nõuab Azure virtuaalse võrk, mis sisaldab-saidilt või saidi punkti virtuaalse privaatse võrgu (VPN) komponent sujuvalt luua ühenduse Azure'i virtuaalmasinates ja kohapealse masinad. Selle VPN komponendi võib ka kohapealse domeeni liige arvutites Windows Server Active Directory domeeni, mille domeenikontrollerid on majutatud ainult Azure'i virtuaalmasinates juurdepääsu lubada. See on oluline tähele küll, et kui VPN-i nurjub, autentimis- ja muid toiminguid, mis sõltuvad Windows Server Active Directory ka nurjub. Kasutajad on võimalik, et Logi sisse, kasutades olemasolevaid vahemälus talletatud identimisteabe kõik--Võrdõigusvõrk või kliendi-serveri autentimine katsete, mille piletid on veel välja või on muutunud aegunud nurjub.

Lugege [Virtuaalse võrgu](http://azure.microsoft.com/documentation/services/virtual-network/) tutvustava video ja üksikasjalikud õpetused, sh [konfigureerimine Azure'i portaalis VPN saitide](../vpn-gateway/vpn-gateway-site-to-site-create.md)loend.

> [AZURE.NOTE] Windows Serveri Active Directory saate juurutada Azure virtuaalse võrgus, mis ei ole mõne kohapealse võrgu Ühenduvus. Juhised selle teema endale siiski on Azure virtuaalse võrgu kasutatakse Kuna IP-aadresside funktsioonid, mis on olulised Windows Server pakub.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Azure'i PowerShelliga peab olema konfigureeritud staatiline IP-aadressid.

Dünaamiliste aadressid on eraldatud vaikimisi, kuid cmdlet Set-AzureStaticVNetIP abil saate määrata selle asemel staatiline IP-aadress. Mis määrab staatiline IP-aadress, mis kestab kuni teenuse paranemise ja VM sulgemine ja uuesti. Lisateabe saamiseks vt [staatiline IP sisemise virtuaalmasinates](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Tingimused ja määratlused

Järgmises avatud loetelu terminite jaoks eri Azure tehnoloogiaid, mis on selles artiklis viidatud.

- **Azure'i virtuaalmasinates**: The IaaS pakkumise Azure, mis võimaldab klientide juurutamiseks VMs töötab peaaegu iga traditsiooniliselt kohapealse serveri töökoormus.

- **Azure virtuaalse võrgu**: võrgustik teenust Azure, mis võimaldab klientidele, luua ja hallata virtuaalne võrkude Azure ja need linkida turvaliselt oma kohapealse võrgu infrastruktuur virtuaalse privaatvõrgu (VPN) abil.

- **Virtuaalne IP-aadress**: Interneti-ühendusega IP-aadress, mis pole seotud kindla arvutis või võrgus kasutajaliidese kaart. Pilveteenustega määratakse saamise võrguliiklust, mis on ümber on Azure VM virtuaalse IP-aadressi. Virtuaalne IP-aadress on atribuut mõnda pilveteenusesse, mis võib sisaldada ühe või mitme Azure'i virtuaalmasinates. Samuti võtke arvesse, et mõne Azure virtuaalse võrgu võib sisaldada ühe või mitme pilveteenustega. Virtuaalne IP-aadresside pakuvad kohalikke koormust tasakaalustavad võimalusi.

- **Dünaamiline IP-aadress**: see on ainult on sisemine IP-aadress. See peaks olema konfigureeritud nimega staatiline IP-aadress (kasutades cmdlet Set-AzureStaticVNetIP) vms, näiteks Põhiliselt/DNS-i serveri rollid majutavad.

- **Teenuse tervendav**: protsess, kus Azure'i naaseb automaatselt teenuse töötab uuesti pärast seda, kui tuvastab, et teenus ei ole. Teenuse tervendav on üks Azure'i, mis toetab kättesaadavus ja paindlikkust aspekte. Ajal tõenäoline, jälgimise healing juhtum on näiteks Põhiliselt VM töötab teenus tulem sarnaneb mõne planeerimata taaskäivitage arvuti, kuid on mõned pool-efektid.

 - Virtuaalne võrguadapteri VM muudab
 - Muudab virtuaalse võrguadapteri MAC-aadress
 - Muudab VM protsessor/CPU ID-d
 - Kui VM on lisatud virtuaalse võrgu ja selle VM IP-aadress on staatiline IP konfiguratsiooni virtuaalse võrguadapteri ei muutu.

 Ükski neist käitumise mõjutavad Windows Server Active Directory, sest see on MAC-aadress või protsessor/CPU ID pole sõltuvus ja kõik Windows Server Active Directory juurutuse Azure on soovitatav käivitada Azure virtuaalse võrgus, nagu eespool kirjeldatud.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>See on ohutu Virtualisoinnilla Windows Server Active Directory domeenikontrollerid?

Klõpsake Azure'i virtuaalmasinates Windows Server Active Directory domeenikontrollereid juurutamine on samu juhiseid installitud kohapealse DCs virtuaalse masina. Töötab virtualiseeritud DCs on ohutu tava, kui ülaltoodud juhised varundus ja taaste DCs. Piiranguid ja juhised virtualiseeritud domeenikontrollereid käitamise kohta leiate lisateavet teemast [Hyper-V töötab domeenikontrollerid](https://technet.microsoft.com/library/dd363553).

Hypervisors andmine või tehnoloogiale, mis võivad põhjustada probleeme palju hajutatud süsteemide, sh Windows Server Active Directory mitteoluliseks. Näiteks füüsilise serveris saate klooni kettal või toetamata meetodite abil tagasi pöörata server, sh kasutamine SANs jne, olek, kuid seda füüsilise serveris on palju raskem kui taastamine rakenduses on hypervisor hetktõmmise virtuaalse masina. Azure'i pakub funktsioone, mis võivad põhjustada sama soovimatute tingimus. Näiteks saate tuleks kopeerida VHD failid DCs asemel korrapäraste varukoopiate plaanimine Kuna taastamise võib põhjustada olukorras hetktõmmise taastamine funktsioonide abil.

Sellise tagasipööramine Tutvustage USN mullid, mis võib kaasa tuua jäädavalt erinevad olekus DCs vahel. Mis võib põhjustada probleeme nagu:

- Toimingutest objektid
- Tabelites sisalduvas paroolid
- Tabelites sisalduvas atribuudiväärtused
- Skeemi vastuolude kui skeemi juhtslaidi on tagasi pöörata.

Kuidas mõjutab DCs kohta leiate lisateavet teemast [USN ja USN tagasipööramine](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Alustades Windows Server 2012, [täiendavad kaitse on sisse ehitatud AD DS](https://technet.microsoft.com/library/hh831734.aspx). Kaitse neid kaitsta virtualiseeritud domeenikontrollerite vastu eelnimetatud probleeme, kui aluseks hypervisor platvorm toetab VM-GenerationID. Azure'i toetab VM GenerationID, mis tähendab, et domeenikontrollerid, kus käivitatakse Windows Server 2012 või uuem versioon Azure'i virtuaalmasinates täiendavad kaitse.

> [AZURE.NOTE] Peaks sulgege ja taaskäivitage VM, mis töötab selle domeenikontrolleri roll Azure'i Külastajate operatsioonisüsteemi asemel käsk **Sule** Azure klassikaline portaalis. Täna, klassikaline portaali kaudu sulgeda VM põhjustab VM, et olla deallocated. Deallocated VM on see eelis, et ei tekiks kulud, kuid see lähtestab ka VM GenerationID, mis on on näiteks Põhiliselt soovimatu. VM-GenerationID lähtestamisel lähtestatakse ka AD DS andmebaasi invocationID, RID rakenduskausta ära ning SYSVOL on märgitud mitteautoriteetsete. Lisateavet leiate teemadest [Active Directory domeeniteenused (AD DS) Virtualization tutvustus](https://technet.microsoft.com/library/hh831734.aspx) ja [Turvaline majutustarkvara DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Miks kasutada Windows Server AD DS Azure'i Virtuaalmasinates?

Windows Server AD DS juurutamise tööiga sobivad hästi juurutamiseks Azure VMs nimega. Oletame näiteks, et teil on mõni ettevõte, mis tuleb remote kohas Aasia kasutajate autentimiseks Euroopa. Ettevõtte pole varem kasutusele Windows Server Active Directory domeenikontrollereid Aasia maksumus juurutamiseks neile ja piiratud teadmiste haldamiseks serverid pärast juurutamise tõttu. Selle tulemusena autentimist taotlusi Aasia on juurde DCs Euroopa optimaalsetesse tulemustega. Sel juhul saate juurutada on näiteks Põhiliselt VM, tuleb käitada sees Azure andmekeskuse Aasia määratud. Manustamise selle AV Azure virtuaalse võrgu, mis on ühendatud otse remote asukoht kuvatakse autentimise jõudluse parandamiseks.

Azure'i on ka sobivad hästi muidu kulukas katastroofi taastamine (DR) saitide asendada. Suhteliselt madala hinnaga hosting väheste domeenikontrollerid ja ühe virtuaalse võrgu Azure tähistab atraktiivseks asemel.

Lõpuks soovite juurutada võrgu rakendus Azure, nt SharePointi, mis nõuab Windows Server Active Directory, kuid pole sõltuvus kohapealse võrgu või ettevõtte Windows Server Active Directory on. Sel juhul juurutamine on eraldatud mets Azure täita SharePointi serveri nõuded on optimaalse. Klõpsake uuesti võrku rakendusi, mille jaoks on vaja ühenduvust kohapealse võrgu ja ettevõtte Active Directory juurutuse toetatakse ka.

> [AZURE.NOTE] Kuna see pakub layer 3 ühendus, saate lubada VPN komponent, mis pakub Ühenduvus on Azure virtuaalse võrgu ja on kohapealse võrgu liige serverid, mis töötavad kohapealse võimendada DCs, et käivitada Azure'i virtuaalmasinates Azure virtuaalse võrgus. Aga kui VPN-i pole saadaval, suhtlemine kohapealse arvutites ja Azure'i domeenikontrollerid ei toimi, tulemuseks autentimis- ja muude tõrkeid.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Vastuolus vahel Windows Server Active Directory domeenikontrollerid Azure'i Virtuaalmasinates võrreldes kohapealse juurutamine

- Mis tahes Windows Server Active Directory juurutuse stsenaarium, mis sisaldab rohkem kui ühe VM, tuleb kasutada ka Azure virtuaalse võrgu IP address järjepidevuse. Pange tähele, et see juhend eeldab, et DCs töötavad Azure virtuaalse võrgus.

- Sarnaselt kohapealse DCs, on soovitatav staatiline IP-aadressid. Azure'i PowerShelli abil saate konfigureerida ainult staatiline IP-aadress. Üksikasjalikumat teavet teemast [staatilise sisemise IP-aadressi vms](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) . Kui teil on jälgimise süsteemide või muid lahendusi, mis staatiline IP-aadressi konfiguratsiooni Külastajate operatsioonisüsteemi otsida, saate määrata atribuudid adapterit VM sama staatiline IP-aadress. Aga olema teadlik võrguadapteri hüljatakse kui VM läbib teenuse tervendav või klassikaline portaalis sulgub ja on oma aadressi deallocated. Sel juhul tuleb lähtestada sees külaline staatiline IP-aadress.

- Juurutamine VMs virtuaalse võrgus ei tähenda (või nõua) connectivity tagasi kohapealse võrguga; virtuaalse võrgu üksnes lubab seda võimalust. Peate looma virtuaalse võrgu privaatne suhtlemine Azure ja kohapealse võrgu jaoks. Peate juurutamine kohapealse võrgu VPN-i lõpp-punkti. Azure'i avatakse kohapealse võrgu VPN-i. Lisateabe saamiseks vt [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md) ja [Azure portaali VPN saitide konfigureerimine](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Võimalus [luua punkti saidi VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) on Windowsi-põhiste arvutite otse Azure virtuaalse võrguga ühenduse luua.

- Sõltumata sellest, kas virtuaalne võrgu või mitte, Azure kulude sealt liikluse, kuid mitte sissepääsu. Eri Windows Server Active Directory kujundus valikuid mõjutavad palju sealt liikluse on loodud juurutamine. Näiteks juurutamine kirjutuskaitstud domeenikontrolleri (RODC) piirab sealt liikluse kuna seda ise Väljamineva meili. Vaid mõne RODC juurutamiseks otsust tuleb kaaluda näiteks Põhiliselt ja [ühilduvus](https://technet.microsoft.com/library/cc755190) rakenduste ja teenuste saidil on RODCs kirjutamine toimingute tegemiseks vajate. Liikluse kulude kohta leiate lisateavet teemast [Azure hinnad veebisaidil kiire](http://azure.microsoft.com/pricing/).

- Kui olete valmis juhtimine üle, mida server ressursid kasutamiseks VMs asutusesiseselt, RAM, nt kettale suurus ja nii edasi, Azure valige loendist eelkonfigureeritud serveri suuruses. Jaoks on näiteks Põhiliselt, andmed kettale on vaja operatsioonisüsteemi ketas Lisaks Windows Server Active Directory andmebaasi talletada.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Saate juurutamist Windows Server AD FS-i kohta Azure'i virtuaalmasinates

Jah, klõpsake Azure'i virtuaalmasinates Windows Server AD FS-i juurutamist ja [head tavad AD FS-i juurutamiseks](https://technet.microsoft.com/library/dn151324.aspx) asutusesisese kehtivad Azure AD FS-i juurutamist. Kuid mõned soovitused, näiteks laadimine tasakaalustamiseks ja kõrge-saadavus vaja tehnoloogiad Lisaks AD FS-i pakub ise. Need tuleb aluseks infrastruktuuri. Loome vaadata mõned parimate tavade ja vaadake, kuidas need saavutada, kasutades Azure VMs ja muu Azure virtuaalse kaudu.

1. **Ärge kunagi jätke turvalisus Turbeloa teenus (STS) serverid otse Internetiga.**

    See on oluline, kuna STS probleemid märkide turvalisus. Selle tulemusena manustada STS serverid nagu AD FS-i serverid kaitse domeenikontrolleri samal tasemel. Kui mõni STS on rikutud, pahatahtlik kasutajatel on võimalus anda juurdepääsu sõned potentsiaalselt sisaldavate taotluste nende valitud tuginedes rakenduste ja muude STS serverites usaldusväärsed ettevõtted.

2. **Juurutada Active Directory domeenikontrollerid kõik kasutaja domeenide samasse võrku AD FS-i serverid.**

    AD FS-i serverid kasutada Active Directory domeeniteenused kasutajate autentimiseks. Soovitatav on juurutada domeenikontrollerid samas võrgus AD FS-i serverid. See pakub järjepidevuse juhuks, kui seost Azure võrgu ja kohapealse võrgu katkeb ja võimaldab alumise latentsus ja parema jõudluse sisselogimise jaoks.

3. **Mitme AD FS-i sõlmed kõrge-saadavus ja tasakaalustamiseks laadi juurutamine.**

    Enamikul juhtudel rakendus, mis võimaldab AD FS-i tõrge pole aktsepteeritav, kuna rakendusi, mis nõuavad turvalisus sõned on sageli esinduse kriitiline. Seetõttu, kuna AD FS-i nüüd asub kriitiline rada kriitiline ülesanne rakenduste juurdepääsu, AD FS-i teenuse tuleb väga mitme AD FS puhverserverid ja AD FS-i serverite kaudu. Jaotuse taotlusi saavutamiseks koormus soolise tavaliselt juurutatud AD FS-i puhverserverite nii AD FS-i serverid ette.

4. **Juurutage üks või mitu Web rakenduse puhverserveri sõlmed Interneti-ühendus.**

    Kui kasutajatel on vaja rakenduste kaitstud AD FS-i teenuse AD FS-i teenus peab olema Interneti kaudu saadaval. See on saavutada Web rakenduse puhverserveri teenuse juurutamine. See on tungivalt soovitatav juurutada rohkem kui üks sõlm kõrge-saadavus eesmärgil ja laadimine tasakaalustamiseks.

5. **Web rakenduse puhverserveri sõlmed ressursid sisevõrgule juurdepääsu piirata.**

    Luba välistele kasutajatele juurdepääs AD FS-i Interneti kaudu, peate juurutamine Web rakenduse puhverserveri sõlmed (või varasemates versioonides Windows Server AD FS puhverserver). Veebirakenduse puhverserveri sõlmed otsesele Interneti-ühendus. Ta ei pea olema domeeni ühendatud ja nad on vaja ainult juurdepääsu AD FS-i serverid TCP portide 443 ja 80. On tungivalt soovitatav, et kõik teised arvutid (eriti domeenikontrollerid) side on blokeeritud.

    See on tavaliselt saavutada kohapealse on DMZ abil. Tulemüürid abil nimekiri turvarežiimi piirata liikluse DMZ kohapealse võrgu (st ainult liiklus määratud IP-aadresside ja portide määratud on lubatud, ja kõik muud liiklus on blokeeritud).

Järgmine diagramm näitab on traditsiooniline kohapealse juurutuse AD FS-i.

![Traditsiooniline kohapealse Active Directory Federation Services juurutuse skeem](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Siiski Kuna Azure'i ei paku kohalikke, kõiki võimalusi pakkuva tulemüüri võimalus, muud suvandid on vaja liikluse piiramiseks kasutada. Järgmises tabelis on iga suvand ja oma eelised ja puudused.

| Suvand | Ära | Kahjuks |
| ------ | --------- | ------------ |
| [Azure'i võrgu ACL-ID](virtual-networks-acl.md) | Vähem kulukalt ja lihtsam esialgne konfiguratsioon | Täiendavad võrgukonfiguratsioon ACL nõutav, kui lisatakse uus VMs mis tahes juurutamise |
| [Barracuda NG tulemüüri](https://www.barracuda.com/products/ngfirewall) | Valge tööviisi ja see nõuab pole võrgu ACL konfigureerimine | Suurema maksumuse ja keerukuse jaoks Alghäälestus |

Üksikasjalik juhiseid AD FS-i juurutamiseks sel juhul on järgmised:

1. Saate luua virtuaalse võrgu asutusesiseses Ühenduvus, kasutades VPN- või [ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. Juurutada domeenikontrollerid virtuaalse võrgus. See toiming on valikuline, kuid soovitatav.

3. Domeeni ühendatud AD FS server töötab ka virtuaalse juurutamine.

4. Saate luua ka [sisemise koormusetasakaalustusega määramine](http://azure.microsoft.com/blog/internal-load-balancing/) , mis sisaldab AD FS server ja kasutab uus privaatne IP-aadress virtuaalne võrgustikus (dünaamiline IP-aadress).

  1. Värskendage DNS-i osutamiseks sisemise koormus tasakaalustatud määramine privaatseks (dünaamiline) IP-aadress FQDN loomiseks.

5. Luua Web rakenduse puhverserveri sõlmed mõnda pilveteenusesse (või eraldi virtuaalse võrgu).

6. Web rakenduse puhverserveri sõlmed pilveteenuses või virtuaalse võrgu juurutamine

  1. Saate luua välise koormus tasakaalustatud kogum, mis sisaldab Web rakenduse puhverserveri sõlmed.

  2. Värskendage välise DNS-i nimi (FQDN), osutage käsule pilvepõhise teenuse avaliku IP-aadress (virtuaalne IP-aadress).

  3. Konfigureerimine AD FS-i kasutamine FQDN, mis vastab sisemise koormus tasakaalustatud määramine AD FS server puhvrid.

  4. Värskendage taotlusepõhise veebisaitide kasutamiseks välise FQDN oma nõuded pakkuja.

7. Piirata juurdepääsu Web rakenduse puhverserveri ükskõik masina AD FS-i virtuaalse võrgu vahel.

Liikluse piiramiseks koormusetasakaalustusega määramine Azure sisemine laadi koormusetasakaalustusteenuse jaoks peab olema konfigureeritud ainult liiklust TCP-pordid 80 ja 443 ja kõik muud liiklus koormus tasakaalustatud määramine sisemine dünaamiline IP-aadress katkeb.

![ADFS-i võrgu ACL TCP 443 + 80 skeem lubatud.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

AD FS-i serverid-liikluse oleksid lubatud ainult järgmistest allikatest:

- Azure'i sisemine laadi koormusetasakaalustusteenuse.
- Kohapealse võrgu administraator IP-aadress.

> [AZURE.WARNING] Kujunduse peab Web rakenduse puhverserveri sõlmed takistada mõni muu VMs Azure virtuaalse võrgu või mis tahes asukohta kohapealse võrgu. Mida saab teha konfigureerimisega tulemüüri reeglid kohapealse seadme teekonna ühenduste või seadme VPN-saidilt VPN ühendused.

Kahjuks see suvand on vaja konfigureerida võrgu ACL mitmes seadmes, sh sisemise koormuse koormusetasakaalustusteenuse, AD FS server ja muudes serverites, saada lisatud virtuaalse võrgu jaoks. Kui mis tahes seadme lisatakse juurutamise konfigureerimata liikluse piiramiseks võrgu ACL-ID, saab kogu juurutamise vastutusel. Kui muudate IP-aadresside Web rakenduse puhverserveri sõlmed võrgu ACL peab lähtestada (st kasutamine peaks olema konfigureeritud kasutama [staatilise dünaamilise IP-aadressid](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![ADFS-i Azure võrguga ACL-ID.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Teine võimalus on [Barracuda NG tulemüüri](https://www.barracuda.com/products/ngfirewall) seadme abil saate reguleerida AD FS puhverserverid ja AD FS-i serverite vahel. See suvand vastab head tavad turbe- ja kõrge-saadavus ja nõuab vähem Administreerimine algse installimise järel, kuna Barracuda NG tulemüüri seadme pakub nimekiri režiimi tulemüüri halduse ja seda saab installida Azure virtuaalse võrgus otse. Mis välistab iga kord, kui lisatakse uus server juurutamise võrgu ACL konfigureerimiseks. Kuid see suvand lisab algse juurutamise keerukus ja.

Sel juhul on kahe virtuaalse võrgu juurutatud ühe asemel. Me helistan neid VNet1 ja VNet2. VNet1 sisaldab kasutamine ja VNet2 on STSs ja võrgu ühendus ettevõtte võrgus. Seetõttu on VNet1 füüsilise (kuigi peaaegu) eraldatud VNet2 ja omakorda, mis ettevõtte võrgus. Seejärel on ühendatud VNet1 VNet2 teisiti tunnelite tehnoloogia tuntud nimega Transport sõltumatu võrgu arhitektuur (TINA) abil. TINA tunneliga on lisatud iga virtuaalse võrgu kaudu Barracuda NG tulemüüri – üks Barracuda iga virtuaalse võrgu kohta.  Kõrge-saadavus, on soovitatav juurutada kahe Barracudas iga virtuaalse võrgus ühe aktiivse muude passiivne. Nad pakuvad väga rikas firewalling funktsioonid, mis võimaldavad jäljendada traditsiooniline kohapealse DMZ Azure toimimist.

![ADFS-i Azure tulemüüriga.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Lisateavet leiate teemast [AD FS-i: laiendamine taotluste arvestada kohapealse ees rakenduse Interneti](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Kui eesmärk on Office 365 SSO eraldi AD FS-i juurutamise alternatiiv

Mõne muu muud juurutamine AD FS üldse, kui teie eesmärgiks on ainult lubamiseks logige sisse Office 365 jaoks. Sel juhul saate lihtsalt juurutamine DirSync parooli sünkroonimise asutusesiseste ja saavutada sama seetõttu minimaalsete juurutamise keerukuse tõttu, kuna seda moodust ei nõua AD FS-i või Azure.

Järgmises tabelis võrreldakse sisselogimise protsesside ja ilma juurutamine AD FS-i tööpõhimõtted.

| Office 365 ühekordse sisselogimise AD FS-i ja DirSync abil | Office 365 sama sisselogimise DirSync + parooli sünkroonimise abil |
| ------------- | ------------- |
| 1 kasutaja ettevõtte võrku sisse logib ja autenditakse Windows Server Active Directory. | 1 kasutaja ettevõtte võrku sisse logib ja autenditakse Windows Server Active Directory. |
| 2 kasutaja proovib juurde Office 365 (ma olen @contoso.com). | 2 kasutaja proovib juurde Office 365 (ma olen @contoso.com). |
| 3. office 365 suunab kasutaja Azure AD. | 3. office 365 suunab kasutaja Azure AD. |
| 4. Kuna Azure AD ei saa autentida ja mõistab seal on usaldus asutusesiseste AD FS-i, suunab selle kasutaja AD FS-i. | 4. azure AD ei saa vastu võtta Kerberose piletid otse ja pole usaldusseos on olemas, et seda palub kasutajal sisestada mandaat. |
| 5. kasutaja saadab Kerberose Piletite AD FS-i STS. | 5. Kui kasutaja sisestab kohapealse sama parool ja Azure AD kinnitab, et need kasutajanimi ja parool, mida on sünkroonitud DirSync suhtes. |
| 6. AD FS-i muudab Kerberose Piletite nõutav Turbeloa vorming/nõuete ja suunab kasutaja Azure AD. | 6. azure AD suunab kasutaja Office 365. |
| 7. kasutaja autendib Azure AD (ilmneb teise teisendus). |  7. kasutaja saab sisse logida Office 365 ja Azure AD luba abil OWA. |
| 8. azure AD suunab kasutaja Office 365. |  |
| 9. kasutaja on logitud vaikselt teenusekomplekti Office 365. |  |

DirSync parooli sünkroonimine stsenaarium (AD FS) koos teenusekomplektiga Office 365 ühekordse sisselogimise asendatakse "sama sisselogimise" kui "sama" lihtsalt tähendab, et kasutajad uuesti sisestama sama kohapealse volituste Office 365 kasutamisel. Pange tähele, et neid andmeid saab kasutaja brauseri vähendamiseks edaspidised viipasid meelde.

### <a name="additional-food-for-thought"></a>Täiendavad toit mõtlema

- Kui juurutate mõne AD FS puhverserver Azure virtuaalne arvutis, on vaja ühenduvust serveriga AD FS-i. Kui need on kohapealse, soovitatakse teil ära kasutada-saidilt virtuaalse Privaatvõrgu ühendus esitatud virtuaalse võrgus, et Web rakenduse puhverserveri sõlmed suhelda oma AD FS-i serverid.

- Kui juurutate AD FS server Azure'i virtual arvutis, ühenduvuse Windows Server Active Directory domeenikontrollerid, atribuut salvestab ja konfiguratsiooni andmebaasid on vaja ja nõuda mõne ExpressRoute või -saidilt VPN-ühenduse Azure virtuaalse võrgu ja kohapealse võrgu vahel.

- Kulude rakendatakse kogu liiklus: Azure'i virtuaalmasinates (sealt liiklus). Kui hind on olulise teguriga, on soovitatav juurutada Web rakenduse puhverserveri sõlmed Azure, jättes AD FS-i serverid kohapealse. Kui ka Azure'i virtuaalmasinates AD FS-i serverid on juurutatud, täiendavad põhjuseks kohapealse kasutajate autentimiseks. Sealt liikluse andmesidekasutusele kulu sõltumata sellest, kas see on liiklevad soovitud ExpressRoute või -saidilt VPN-ühendus.

- Kui otsustate kasutada Azure kohalikke serveri koormus tasakaalustamiseks võimaluste kättesaadavuse AD FS-i serverite jaoks, Pange tähele, et koormusetasakaalustuseks pakub sondid, mis määratletakse virtuaalmasinates pilvepõhise teenuse seisundi. Azure'i virtuaalmasinates (erinevalt veebis või töötaja rollid) puhul tuleb kasutada kohandatud juures Kuna agent, mis vastab vaikimisi sondid pole Azure'i virtuaalmasinates. Lihtsa, võite kasutada kohandatud TCP juures – see nõuab ainult, et TCP-ühenduse (TCP SYN lõigu saadetud ja vastanud koos TCP SYN-ACK lõigu) on edukalt loodud virtuaalse masina seisundi määratlemiseks. Saate konfigureerida kohandatud juures kasutada mis tahes TCP port, millele teie virtuaalmasinates aktiivselt kuulata.

> [AZURE.NOTE] Masinad, mis tuleb seada samu pordid otse Internetiga (nt pordi 80 ja 443) ei saa kasutada sama pilveteenuses. Seega soovitatav on luua sihtotstarbeline pilvepõhise teenuse oma Windows Server AD FS-i serverite vältimiseks võimalikud kattub pordi rakenduse nõuded ja Windows Server AD FS-i vahel.

## <a name="deployment-scenarios"></a>Juurutamise stsenaariumid

Järgmises jaotises kirjeldatakse tavalised juurutamise stsenaariumide tähelepanu juhtimiseks olulist asjaolu tuleb arvesse. Iga stsenaarium on lingid otsuseid ja tegureid silmas pidada kohta rohkem üksikasju.

1. [AD DS: Juurutamine AD DS-arvestada taotluse ja ettevõtte võrguühendus nõue](#BKMK_CloudOnly)

    Näiteks teenuse Interneti-ühendusega SharePointi juurutatakse Azure virtuaalne arvutis. Taotlus on ettevõtte võrgus ressursid sõltuvused. Rakenduse Windows Server AD DS ei vaja, kuid ei nõua ettevõtte Windows Server AD DS.

2. [AD FS-i: Laiendamine taotluste arvestada kohapealse ees rakenduse Interneti-ühendus](#BKMK_CloudOnlyFed)

    Näiteks nõuded meeles rakendus, mis on edukalt juurutatud asutusesisese ja kasutavad ettevõtte kasutajatele peab muutuvad kättesaadavaks Interneti kaudu. Rakendus peab äripartneritega abil oma ettevõtte identiteedid ja ettevõtte olemasolevatele kasutajatele otse Interneti kaudu juurde.

3. [AD DS: Juurutamine Windows Server AD DS-arvestada rakendus, mille jaoks on vaja ühenduvust ettevõtte võrguga](#BKMK_HybridExt)

    Näiteks LDAP arvestada rakendus, mis toetab Windowsi integreeritud autentimist ja kasutab Windows Server AD DS hoidla konfigureerimine ja kasutaja profiili andmed on juurutatud Azure virtuaalne arvutis. On soovitatav kasutada olemasoleva ettevõtte Windows Server AD DS ja esitage ühekordse sisselogimise rakendamiseks. Rakendus pole taotluste arvestada.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Juurutamine AD DS-arvestada taotluse ja ettevõtte võrguühendus nõue

![Vaid AD DS juurutamise](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**joonisel 1**

#### <a name="description"></a>Kirjeldus

Azure'i virtual arvutis juurutatakse SharePoint ja taotlus on sõltuvused ettevõtte võrgus ressursid. Rakenduse Windows Server AD DS, kuid ei nõua *ei* nõua ettevõtte Windows Server AD DS. Pole Kerberose või ühendatud usalduste on vaja, kuna kasutajad on omas ettevalmistatud rakendusse Windows Server AD DS domeeni majutab ka Azure'i virtuaalmasinates pilve rakenduse kaudu.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Stsenaarium kasutuse ja kuidas seda stsenaariumi rakendamine tehnoloogia valdkondades

- [Võrgu topoloogia](#BKMK_NetworkTopology): luua Azure virtuaalse ilma asutusesiseses Ühenduvus (tuntud ka kui saidilt ühenduvuse).

- [Näiteks Põhiliselt juurutuse konfigureerimise](#BKMK_DeploymentConfig): juurutada uue domeenikontrolleri sisse uus, ühe domeeni, Windows Server Active Directory mets. See tuleb kasutada koos Windows DNS-i serveri.

- [Windows Server Active Directory saidi topoloogia](#BKMK_ADSiteTopology): kasutada Windows Server Active Directory vaikesaidi (kõik arvutid on vaikimisi esimese saidi nimi).

- [IP tegelemine ja DNS-i](#BKMK_IPAddressDNS).

 - Staatiline IP-aadressi määramiseks näiteks Põhiliselt Set-AzureStaticVNetIP Azure PowerShelli cmdleti abil.
 - Installige ja konfigureerige Windows Serveri DNS-i domeeni e Azure.
 - Nime ja IP-aadress VM, mis hostib AV ja DNS-i serveri rollid virtuaalse võrgu atribuutide konfigureerimine.

- [Globaalse kataloogi](#BKMK_GC): esimese näiteks Põhiliselt mets peab olema globaalse kataloogi serveris. Täiendavad DCs peaks konfigureeritud nimega GCs, kuna ühe domeeni mets, ei nõua globaalse kataloogi kaudu näiteks Põhiliselt lisatööd.

- [Windows Serveri AD DS andmebaasi ja SYSVOL paigutuse](#BKMK_PlaceDB): lisada andmed kettale DCs töötab Azure VMs nimega, et salvestada Windows Server Active Directory andmebaasi, logid ja SYSVOL.

- [Varundus ja taaste](#BKMK_BUR): kindlaks teha, kui soovite talletada süsteemi oleku varukoopiaid. Vajaduse korral lisada näiteks Põhiliselt VM varukoopiate salvestamiseks andmete teisele kettale.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS-i: laiendamine taotluste arvestada kohapealse ees rakenduse Interneti-ühendus

![Asutusesiseses ühenduvuse teenusekomplektiga](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**joonis 2**

#### <a name="description"></a>Kirjeldus

Nõuded meeles rakendus, mis on edukalt juurutatud asutusesisese ja kasutavad ettevõtte kasutajatele peab muutuma otse Interneti-ühendus. Rakendus toimib web frontend SQL-andmebaasiga, kus see talletab andmeid. SQL-i serverid rakenduse kasutatavad asuvad ka ettevõtte võrgus. Kaks Windows Server AD FS-i STSs ja laadi-koormusetasakaalustusteenuse on juurutatud asutusesisese ettevõtte kasutajatele juurdepääsu anda. Nüüd rakendus peab lisaks juurde otse Interneti kaudu oma ettevõtte identiteedid äripartneritega ja ettevõtte olemasolevatele kasutajatele.

Püüdes lihtsustamiseks ja selle uue nõude juurutamine ja konfiguratsiooni vajadustele on otsustanud, et kaks täiendavad veebi frontends ja kahe Azure'i virtuaalmasinates installida Windows Server AD FS puhverserverid. Kõik neli VMs puutuvad otse Internetiga ja antakse Ühenduvus Azure virtuaalse võrgu-saidilt VPN võimalus kohapealse võrguühendust.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Stsenaarium kasutuse ja kuidas seda stsenaariumi rakendamine tehnoloogia valdkondades

- [Võrgutopoloogia](#BKMK_NetworkTopology): Azure'i virtuaalse võrgu ja [asukohaga ühenduvuse konfigureerimine](../vpn-gateway/vpn-gateway-site-to-site-create.md)loomine.

 > [AZURE.NOTE] Iga Windows Server AD FS-i serdid, tagada pääseb URL-i serdi Mall ja tulemuseks serdid määratletud töötavate Azure'i Windows Server AD FS-i eksemplari. See võib nõuda asutusesiseses ühenduvust teie PKI infrastruktuuri osade. Jaoks näide kui selle CRL lõpp-punkti on LDAP põhinev ja majutatud üksnes asutusesiseselt, siis asukohaga on vaja ühenduvust. Kui see pole soovitatav, võib olla vaja kasutada välja nime, kelle CRL on juurdepääsetav Interneti sertimiskeskuse serdid.

- [Cloud services konfiguratsiooni](#BKMK_CloudSvcConfig): Veenduge, et teil on kaks pilveteenustega järjestuses pakuvad kaks koormusetasakaalustusega virtuaalse IP-aadressid. Kaks Windows Server AD FS puhverserver VMs pordid 80 ja 443 liiklus esimese pilveteenuses virtuaalse IP-aadress. Windows Server AD FS puhverserver VMs konfigureeritakse osutamiseks kohapealse laadi-koormusetasakaalustusteenuse IP-aadress see valdkondades Windows Server AD FS-i STSs. Kahe VMs, mis töötab web frontend uuesti pordid 80 ja 443 liiklus teise pilveteenuses virtuaalse IP-aadress. Saate konfigureerida kohandatud juures laadi-koormusetasakaalustusteenuse ainult suunab toimiva Windows Server AD FS puhverserver ja web frontend VMs-liikluse tagamiseks.

- [Federation server configuration](#BKMK_FedSrvConfig): Windows Server konfigureerimine AD FS liiduserveri (STS) luua turvalisus märkide jaoks Windows Server Active Directory mets loodud pilveteenuses nimega. Määrake federation taotluste pakkuja usalda seoste aktsepteerida identiteeti soovite erinevad partnerid ja konfigureerimine tuginedes pool usalda seosed erinevate rakendustega, mida soovite luua märkide.

    Enamikul juhtudel Windows Server AD FS puhverserverid on juurutatud on Interneti-ühendusega võimsus turvalisuse huvides oma Windows Server AD FS liiduserveri kolleegidega jäävad eraldatud otsene Interneti-ühendus. Sõltumata teie juurutamise stsenaariumi pordile tuleb konfigureerida oma pilveteenuses virtuaalse IP-aadress, mis annab vastuseks avalikult exposed IP-aadress ja port, mida on võimalik laadi – Topeltdegressiivse üle oma Windows Server AD FS-i STS kahel või puhverserveri eksemplarid.

- [Windows Server AD FS-i kõrge-saadavus konfigureerimine](#BKMK_ADFSHighAvail): on soovitatav juurutada Windows Server AD FS-i serveripargi vähemalt kahe serverid Tõrkesiirde ja laadimine tasakaalustamiseks. Võib kasutada Windows Server AD FS-i konfigureerimine andmete jaoks Windowsi sisemine andmebaas (WID) ja sisemise laadi tasakaalustavad võimalus Azure abil jaotada sissetulevad taotlused kogu serveripargi serverites.

Lisateavet leiate teemast [AD DS juurutusjuhend](https://technet.microsoft.com/library/cc753963).


### <a name="BKMK_HybridExt"></a>3. AD DS: Juurutamine Windows Server AD DS-arvestada rakendus, mille jaoks on vaja ühenduvust ettevõtte võrguga

![Asutusesiseses AD DS juurutamise](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**joonis 3**

#### <a name="description"></a>Kirjeldus

LDAP arvestada rakendus on juurutatud Azure virtuaalne arvutis. See toetab Windowsi integreeritud autentimist ja kasutab Windows Server AD DS hoidla konfiguratsiooni ja kasutajale profiili andmete jaoks. Eesmärk on kasutada olemasoleva ettevõtte Windows Server AD DS ja esitage ühekordse sisselogimise rakendamiseks. Rakendus pole taotluste arvestada. Kasutajad peavad ka otse Internetist rakenduse avamiseks. Optimeerida jõudluse ja maksumus, on otsustanud kaks täiendav domeenikontrollerid, mis on osa ettevõtte Domeen juurutada Azure rakenduse kõrval.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Stsenaarium kasutuse ja kuidas seda stsenaariumi rakendamine tehnoloogia valdkondades

- [Võrgu topoloogia](#BKMK_NetworkTopology): luua Azure virtuaalse [asukohaga Ühenduvus](../vpn-gateway/vpn-gateway-site-to-site-create.md).

- [Installimeetod](#BKMK_InstallMethod): juurutamine koopia DCs ettevõtte Windows Server Active Directory domeeni. Näiteks Põhiliselt koopia jaoks saate installida Windows Server AD DS VM ja soovi korral installige kaudu Media (IFM) funktsiooni abil vähendada paljundada uue näiteks Põhiliselt installimise ajal andmeid. Juhendi, leiate [installida Azure Active Directory koopia domeenikontrolleri](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Isegi juhul, kui kasutate IFM, võib olla tõhusam luua virtuaalse näiteks Põhiliselt asutusesisese ja teisaldada pilve asemel imitatsiooniga Windows Server AD DS installimise ajal kogu virtuaalse kõvaketta (VHD). Turvalisuse huvides on soovitatav kustutada selle VHD kohapealse võrgu kaudu kui see on kopeeritud Azure.

- [Windows Server Active Directory saidi topoloogia](#BKMK_ADSiteTopology): Azure'i uue saidi loomine Active Directory saidid ja teenused. Alamvõrgu Windows Server Active Directory objekti tähistavad Azure virtuaalse võrgu ja alamvõrgu lisada saidi loomine. Saate luua uue saidi lingi, mis sisaldab Azure uus sait ja saidile, kus asub selleks, et kontrollida ja optimeerida Windows Server Active Directory liikluse ja sealt Azure'i Azure virtuaalse võrgu VPN-i lõpp-punkti.

- [IP tegelemine ja DNS-i](#BKMK_IPAddressDNS).

 - Staatiline IP-aadressi määramiseks näiteks Põhiliselt Set-AzureStaticVNetIP Azure PowerShelli cmdleti abil.
 - Installige ja konfigureerige Windows Serveri DNS-i domeeni e Azure.
 - Nime ja IP-aadress VM, mis hostib AV ja DNS-i serveri rollid virtuaalse võrgu atribuutide konfigureerimine.

- [Geo jaotatud DCs](#BKMK_DistributedDCs): konfigureerida täiendavaid virtuaalse võrgu vastavalt vajadusele. Kui teie Active Directory saidi topoloogia nõuab klasterhostides kaugemad, mis vastavad Azure regioonid, kui soovite luua Active Directory saidid vastavalt sellele.

- [Kirjutuskaitstud DCs](#BKMK_RODC): juurutamist võib mõni RODC Azure'i saidi, sõltuvalt teie nõuete täitmiseks kirjutada näiteks Põhiliselt vastu ja ühilduvus rakenduste ja teenuste RODCs saidile. Rakenduste ühilduvuse kohta leiate lisateavet teemast [kirjutuskaitstud kontrollerid rakenduse ühilduvus juhend](https://technet.microsoft.com/library/cc755190).

- [Globaalse kataloogi](#BKMK_GC): GCs on vaja multi-domain metsades sisselogimise päringutele. Kui juurutate GC Azure saidil, olete tekivad sealt tasuline taotlusi autentimine põhjustada päringute GCs muude saitide. Et liiklus minimeerimiseks saate lubada universaalse rühma liikmeks vahemällu salvestamine saidi Azure Active Directory saidid ja teenused.

    Kui juurutate GC, konfigureerida saidi linke ja saidi linke maksab nii, et GC Azure saidil pole eelistatud allikana AV muude GCs, mis on vaja korrata sama osaline domeeni sektsioonid.

- [Windows Serveri AD DS andmebaasi ja SYSVOL paigutuse](#BKMK_PlaceDB): lisada andmed kettale DCs töötavate Azure VMs, et salvestada Windows Server Active Directory andmebaasi, logid ja SYSVOL.

- [Varundus ja taaste](#BKMK_BUR): kindlaks teha, kui soovite talletada süsteemi oleku varukoopiaid. Vajaduse korral lisada näiteks Põhiliselt VM varukoopiate salvestamiseks andmete teisele kettale.

## <a name="deployment-decisions-and-factors"></a>Juurutamise otsuseid ja mõjutavad tegurid

Selles tabelis on kokkuvõte Windows Server Active Directory tehnoloogia valdkondades, mis mõjutavad stsenaariumide ja vastavaid otsuseid pidada koos linkidega täpsemalt allpool. Tehnoloogia mõnes valdkonnas ei pruugi kehtivad kõigi juurutamise stsenaariumide ja tehnoloogia mõnes valdkonnas võib olla olulisem juurutamise stsenaariumile, kui muud tehnoloogia alad.

Näiteks kui juurutate koopia näiteks Põhiliselt virtuaalse võrgus ja oma mets on ainult ühe domeeni, valides seejärel globaalse kataloogi serveri juurutamine sel juhul ei kriitilised juurutamise stsenaariumile kuna see loob täiendavad dispersioonanalüüs nõuetele. Teisalt, kui mets on mitu domeeni, siis otsust globaalse kataloogi virtuaalse võrgus juurutamiseks võib mõjutada läbilaskevõime, seadme, autentimine, kataloogi otsingud, jne.

| Windows Serveri Active Directory tehnoloogia ala | Otsuste | Mõjutavad tegurid |
| ---- | ---- | ---- |
| [Võrgu topoloogia](#BKMK_NetworkTopology) | Luua virtuaalse võrgu? | <li>Nõuded juurdepääsuks Corp ressursid</li> <li>Autentimine</li> <li>Konto haldamine</li> |
| [Näiteks Põhiliselt juurutuse konfigureerimine](#BKMK_DeploymentConfig) | <li>Mis tahes usalduste ilma eraldi mets juurutada?</li> <li>Juurutage uus mets koos Domeeniliit?</li> <li>Juurutage uus mets Windows Server Active Directory mets usaldusväärse või Kerberose?</li> <li>Laiendada, näiteks Põhiliselt koopia juurutamine Corp mets?</li> <li>Laiendada Corp mets juurutamine domeeni puuvaate või uus lapse domeeni?</li> | <li>Turvalisus</li> <li>Nõuetele vastavus</li> <li>Maksumus</li> <li>Paindlikkust ja tõrketaluvust</li> <li>Rakenduse ühilduvus</li> |
| [Windows Server Active Directory saidi topoloogia](#BKMK_ADSiteTopology) | Kuidas seadistada alamvõrku, saitide ja saidi linke Azure virtuaalse võrguga optimeerida liikluse ja minimeerida maksumus? | <li>Alamvõrgu ja saidi määratlused</li> <li>Saidi lingi atribuutide ja muutmine teatis</li> <li>Dispersioonanalüüs tihendamine</li> |
| [IP tegelemine ja DNS-i](#BKMK_IPAddressDNS) | Kuidas seadistada IP-aadressid ja nimelahendus? | <li>Kasutage Set-AzureStaticVNetIP cmdlet-käsu abil saate määrata staatiline IP-aadress</li> <li>Installige Windows Server DNS-serveri ja nime ja IP-aadress VM, mis hostib AV ja DNS-i serveri rollid virtuaalse võrgu atribuutide konfigureerimine</li> |
| [Geo jaotatud DCs](#BKMK_DistributedDCs) | Kuidas paljundada DCs eraldi virtuaalse võrkudes? | Kui teie Active Directory saidi topoloogia nõuab klasterhostides kaugemad, mis vastab Azure regioonid, kui soovite luua vastavalt Active Directory saidid. [Konfigureerimine virtuaalse võrgu virtuaalse võrgu Ühenduvus](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) domeeni domeenikontrollerid eraldi virtuaalse võrkudes vahel. |
| [Kirjutuskaitstud DCs](#BKMK_RODC) | Kirjutuskaitstud või kirjutatav DCs kasutada? | <li>HBI/PII atribuute filtreerimine</li> <li>Filtri saladusi</li> <li>Piiratud väljaminev liiklus</li> |
| [Globaalse kataloogi](#BKMK_GC) | Globaalse kataloogi installida? | <li>Ühe domeenide mets, teha kõik DCs GCs</li> <li>Multi-domain mets, jaoks vajalike GCs autentimine</li> |
| [Installimeetod](#BKMK_InstallMethod) | Kuidas installida Azure näiteks Põhiliselt? | Kas: <li>Installige Windows PowerShelli või Dcpromo AD DS</li> <li>Liikumine VHD kohapealse näiteks virtuaalse Põhiliselt.</li> |
| [Windows Server AD DS andmebaas ja SYSVOL paigutamine](#BKMK_PlaceDB) | Kui Windows Server AD DS andmebaasi, logid ja SYSVOL talletada? | Saate muuta Dcpromo.exe vaikeväärtused. Need kriitilised Active Directory failide *peab* asuma Azure'i andmed kettale asemel operatsioonisüsteemi ketast, et kirjutada vahemällu. |
| [Varundus ja taaste](#BKMK_BUR) | Kuidas kaitsta ja andmete taastamine? | Süsteemi oleku varundamiseks |
| [Federation serveri konfigureerimine](#BKMK_FedSrvConfig) | <li>Juurutage uus mets koos federation pilveteenuses?</li> <li>Kohapealse AD FS-i juurutada ja jätke proxy pilveteenuses?</li> | <li>Turvalisus</li> <li>Nõuetele vastavus</li> <li>Maksumus</li> <li>Juurdepääsu taotluste äripartneritega</li> |
| [Cloud services konfigureerimine](#BKMK_CloudSvcConfig) | Pilveteenus juurutatakse peidetult esimest korda, saate luua virtuaalse masina. Kas vajate täiendavaid pilveteenustega juurutada? | <li>Kas VM ja VMs nõutav otsese käes Interneti-ühendus?</li> <li> Kas teenus nõuab koormust tasakaalustavad?</li> |
| [Federation serveri nõuded avalike ja privaatvõ IP-aadresside (dünaamilise IP vs virtuaalse IP)](#BKMK_FedReqVIPDIP) | <li>Windows Server AD FS-i eksemplari vajab saavutada otse Interneti kaudu?</li> <li>Kas rakenduse kasutusele pilveteenuses nõutav oma Interneti-ühendusega IP-aadress ja pordi?</li> | Ühe pilveteenuses iga virtuaalse IP-aadress on nõutud juurutamise loomine |
| [Windows Server AD FS-i kõrge-saadavus konfigureerimine](#BKMK_ADFSHighAvail) | <li>Minu Windows Server AD FS server serveripargis mitu sõlmed?</li> <li>Mis on minu Windows Server AD FS puhverserver serveripargi mitu sõlmed?</li> | Paindlikkust ja viga |

### <a name="BKMK_NetworkTopology"></a>Võrgu topoloogia

IP address järjepidevuse ja Windows Server AD DS-i DNS-i nõuete täitmiseks on vaja esmalt [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) loomine ja oma virtuaalmasinates manustamiseks. Selle loomise ajal peate otsustama, kas soovi laiendada ühenduvuse kohapealse ettevõtte võrguga, mis ühendab läbipaistev Azure'i virtuaalmasinates kohapealse masinad – see saavutamiseni traditsiooniline VPN-ja hõlbustusvahendite kasutamine ja nõuab, et soovitud VPN-i lõpp-punkti kokku puutuda aknaäärses nurgas ettevõtte võrgus. Mis on VPN on algatada Azure'i ettevõtte võrguga, mitte vastupidi.

Pange tähele, et lisatasu laiendamisel virtuaalse võrgus kohapealse võrgu lisaks standard kulud, mida iga VM rakendada. Täpsemalt on kulude Azure virtuaalse võrgu lüüsi CPU aega ja sealt võrguliiklusest iga VM, mis suhtleb kohapealse masinad üle VPN-i. Võrgu liikluse kulude kohta leiate lisateavet teemast [Azure hinnad veebisaidil kiire](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>Näiteks Põhiliselt juurutuse konfigureerimine

Saate konfigureerida näiteks Põhiliselt viis sõltub nõuete teenuse Azure käivitada. Näiteks võib Juurutage uus mets, eraldatud oma ettevõtte mets testimiseks on tõendada mõiste, uue rakenduse või mõne muu lühiajaline projekt, mis eeldab kataloogiteenuste, kuid ei ole juurdepääsu sisemise ettevõtte ressursid.

Kasu, on näiteks Põhiliselt kopeerida isoleeritakse mets kohapealse DCs, tulemuseks vähem väljamineva võrguliikluse süsteemi, enda loodud otse vähendada. Võrgu liikluse kulude kohta leiate lisateavet teemast [Azure hinnad veebisaidil kiire](http://azure.microsoft.com/pricing/).

Teine näide Oletame Privaatsus nõuded teenuse olemas, kuid teenus sõltub teie sisemise Windows Server Active Directory juurdepääsu. Kui teil on lubatud host andmete teenuse pilves, võib juurutamist lapse uus domeen oma sisemise mets Azure jaoks. Sel juhul saate juurutada on näiteks Põhiliselt lapse uus domeen (ilma globaalse kataloogi selleks, et aidata lahendada privaatsuse probleeme). Selle stsenaariumi korral koos koopia näiteks Põhiliselt juurutuse nõuab virtuaalse võrgu Ühenduvus oma kohapealse DCs.

Kui loote uus mets, valida, kas kasutada [Active Directory loodab](https://technet.microsoft.com/library/cc771397) või [Federation loodab](https://technet.microsoft.com/library/dd807036). Saldo ühilduvust, turvalisus, nõuetele vastavus, maksumuse ja paindlikkust tulenevad nõuded. Näiteks ära [Valikuline autentimise](https://technet.microsoft.com/library/cc755844) võite juurutada uus mets Azure ja loomine Windows Server Active Directory usalda mets kohapealse ja pilveteenuse mets vahel. Kui rakendus on taotluste teada, aga võib juurutamine federation usalduste Active Directory mets usalduste asemel. Teine tegur saab paljundada rohkem andmeid oma kohapealse Windows Server Active Directory pilveteenusesse pikendades või rohkem väljamineva liikluse tulemusena autentimis- ja päringu laadimise.

Kättesaadavus ja viga nõuded mõjutada ka oma valik. Näiteks kui link on katkenud, rakenduste kasutamine Kerberos usalda või mõne federation usaldada on kõik tõenäoliselt täiesti ei tööta juhul, kui olete juurutanud piisav infrastruktuur Azure. Näiteks koopia DCs alternatiivsest konfiguratsioone (kirjutatav või RODCs) ei saaks seda luba link katkestuste tõenäosust.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory saidi topoloogia

Peate saidid ja saidi linke optimeerimine liikluse ja minimeerida maksumus õigesti määratlemiseks. Saitide, saidi linke ja alamvõrku mõjutavad dispersioonanalüüs topoloogia DCs ja voo autentimise liikluse vahel. Võtke arvesse järgmist liikluse kulude juurutada ja konfigureerida vastavalt stsenaariumile juurutamise DCs.

- On tunnis ise lüüsi nominal tasu:

 - Saate alustada ja vastavalt vajadusele on peatatud

 - Kui peatatud, Azure VMs on eraldatud ettevõtte võrgus

- Sissetulev liiklus on tasuta

- Väljaminev liiklus on vastavalt [Azure'i hinnad veebisaidil kiire](http://azure.microsoft.com/pricing/)lisandu. Saate optimeerida saidi lingi atribuudid kohapealse saitidele ja saitide pilveteenuse vahel järgmiselt:

 - Kui kasutate mitut virtuaalse võrku, konfigureerida linkide – saidile ja tema maksab õigesti Windows Server AD DS takistada seadmise Azure'i saidi üle ühe varustavate teenust tasuta samal tasemel. Samuti võiksite keelamine silla kõik saidi link (BASL) valik (see on vaikimisi). See tagab, et ainult otse ühendatud saite kopeerida mõnda teise. DCs transitively ühendatud saitidel pole enam võimalik kopeerida otse üksteise, kuid peate ise levinud saidi või saitide kaudu. Kui vahendaja saitide mingi põhjusel saadaval, ei toimu dispersioonanalüüs DCs transitively ühendatud saitidel vahel ka siis, kui saitide vahel ühendus on olemas. Lõpetuseks, kui sihiliste dispersioonanalüüs käitumine jaotised jäävad soovitav, saidi loomine link sillad, mis sisaldavad vastav sait-linke ja saidid, nt asutusesiseselt, ettevõtte võrgus saidid.

 - [Konfigureerimine saidi lingi kulud](https://technet.microsoft.com/library/cc794882) õigesti, et vältida faili tahtmatut. Näiteks kui **Proovige järgmise lähima saidi** säte on lubatud, veenduge, et virtuaalse võrgu saidid ei ole järgmise kõige lähemal suurendades seotud link sait objekti, mis ühendab Azure'i saidi tagasi ettevõtte võrgus.

 - Saidi lingi [intervallide](https://technet.microsoft.com/library/cc794878) ja [ajakava](https://technet.microsoft.com/library/cc816906) vastavalt ühtsuse nõuded ja määr objekti muudatuste konfigureerimine. Latentsus hälbega joondada dispersioonanalüüs ajakava. DCs korrata ainult Viimane olek väärtuse, et väheneb dispersioonanalüüs intervall saate salvestada kulud, kui seal on piisavalt objekti muuta.

- Kui minimeerimine kulud on prioriteet, veenduge, et dispersioonanalüüs on ajastatud ja Muuda teatise pole lubatud. See on vaikimisi konfiguratsiooni, kui imitatsiooniga saitide vahel. See pole oluline. Kui juurutate on RODC virtuaalse võrgus, kuna RODC kuvatakse pole ise muudatused Väljamineva meili. Kuid kui juurutate kirjutatav näiteks Põhiliselt, veenduge, et saidi lingi kopeerida värskendused mittevajalike sagedus pole konfigureeritud. Kui juurutate globaalse kataloogi serveris g (c), veenduge, et iga saidi, mis sisaldab GC tiražeerib domeeni sektsioonid mõnest muust allikast, näiteks Põhiliselt sait, mis on seotud link sait või saidi linke, mis on väiksem kulu kui GC Azure'i saidi.


- On võimalik endiselt vähendada vahele, muutes dispersioonanalüüs tihendamise algoritmi kopeerimine genereeritud võrguliiklust. Tihendamise algoritmi kontrollib REG_DWORD registri kirje HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator tihendamise algoritmi. Vaikeväärtus on 3, mis vastab loendis efektidele Xpress Tihenda algoritmile. Saate muuta väärtuse 2, mis muutub algoritmi MSZip. Enamikul juhtudel tihendus suurendada, kuid see nii arvelt Protsessori kasutuse. Lisateabe saamiseks lugege teemat [Kuidas Active Directory kopeerimise topoloogia töötab](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP tegelemine ja DNS-i

Azure'i virtuaalmasinates jagatakse "DHCP võetud aadressid" vaikimisi. Kuna Azure virtuaalse võrgu dünaamiline aadresside püsida virtuaalse masina eluiga virtuaalse masina, Windows Server AD DS nõuded on täidetud.

Selle tulemusena dünaamilise aadressi kasutamisel Azure olete efekti abil staatiline IP-aadress, sest see on marsruuditavaid perioodi on asjaolude ja selle asjaolude perioodi on võrdne eluiga pilveteenusesse.

Siiski dünaamilise aadress on deallocated kui VM on sulgumist. IP-aadress on deallocated vältimiseks saate [kasutada Set-AzureStaticVNetIP määrata staatiline IP-aadress](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Nime eraldusvõimet, juurutada oma (või kasutada oma praegust) DNS-i serveri taristu; Azure'i antud DNS-i ei vasta täpsemalt nimi eraldusvõime vajadustele Windows Server AD DS. Näiteks see ei toeta dünaamiline SRV-kirjed jne. Nimelahendus on kriitiline konfiguratsiooni kirje DCs ja klientide domeeni ühendatud. DCs peavad saama registreerimisel resource records ja lahendamine teiste AV resource records.
Viga hälbe ja jõudluse huvides on optimaalse Windows Serveri DNS-i teenuse installida Azure töötavate DCS. Seejärel konfigureerida Azure virtuaalse atribuudid nimi ja DNS-i serveri IP-aadress. Muud VMs virtuaalse võrgu käivitamisel konfigureeritakse oma DNS-i kliendi lahendaja sätted DNS-i server on dünaamiline IP address eraldatud osana.

> [AZURE.NOTE] Kohapealse arvutites ei saa liita Windows Server AD DS Active Directory domeeni, mis on majutatud Azure'i otse Interneti kaudu. Pordi nõuete Active Directory ja domeeni liitumine renderdamiseks otstarbekas otse nähtavaks tegemine vajalikud pordid ja mõju, on kogu näiteks Põhiliselt Interneti-ühendus.

VMs registreerida oma DNS-i nime automaatselt käivitamisel või nime muutmise korral.

Selles näites ja teine näide, mis näitab, kuidas ette esimese VM ja AD DS installimine kohta leiate lisateavet teemast [installida Microsoft Azure Active Directory uus mets](active-directory-new-forest-virtual-machine.md). Windows PowerShelli kasutamise kohta leiate lisateavet teemast [Azure PowerShelli installimine](../powershell-install-configure.md) ja [Azure halduse cmdletid](https://msdn.microsoft.com/library/azure/jj152841).

### <a name="BKMK_DistributedDCs"></a>Geo jaotatud DCs

Azure'i pakub eeliseid kui hosting mitme DCs erinevates virtuaalse võrkudes:

- Mitme saidi tõrketaluvust

- Füüsiline lähedus harukontorite (madalam latentsus)

Otsest suhtlemine virtuaalse võrgu konfigureerimise kohta leiate teemast [konfigureerimine virtuaalse võrgu virtuaalse võrguühendus](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Kirjutuskaitstud DCs

Peate, kas soovite kirjutuskaitstud või kirjutatav DCs juurutamine. Teil võib valmis RODCs juurutada, kuna on teil füüsiline kontroll nende üle, kuid RODCs on koostatud asukohtades, kus nende füüsiline turvalisus on risk, nt harukontorite kasutusele võtta.

Azure'i ei esita füüsilise turvalisuse riski kontori, kuid RODCs endiselt osutuda kõige kuna need funktsioonid on sobivad hästi nendes keskkondades ehkki erinevatel põhjustel. Näiteks RODCs on pole väljaminev kopeerimise ja saavad asustamiseks valikuliselt saladusi (paroolid). Negatiivsed, neid saladusi puudumine võidakse nõuda nõudmisel väljaminev liiklus valideerimiseks need kasutaja või arvuti kontrollib. Kuid saladusi valikuliselt eelnevalt asustatud ja vahemällu.

RODCs pakuvad täiendavaid eeliseid sisse ja ümber HBI ja PII probleemid, kuna atribuudid, mis sisaldavad tundlikku andmed RODC filtreeritud atribuudi (FAS) saate lisada. FAS on kohandatavad atribuute, mis on kopeeritud RODCs. Saate kasutada FAS juhuks, kui te ei ole lubatud või ei soovi säilitada PII või HBI Azure kaitsta. Lisateavet leiate teemast [RODC filtreeritud seatud atribuut [(https://technet.microsoft.com/library/cc753459)].

Veenduge, et rakenduste peab olema kooskõlas RODCs, mida plaanite kasutada. Windows Serveri Active Directory lubatud rakendustele töötada ka RODCs, kuid mõned rakendused saate teha ebaefektiivselt või nurjuda, kui neil pole juurdepääsu kirjutatav näiteks Põhiliselt. Lisateabe saamiseks vaadake [kirjutuskaitstud DCs rakenduse ühilduvus juhend](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Globaalse kataloogi

Peate, kas soovite installida globaalse kataloogi (g c). Ühe domeeni mets, peaksite konfigureerima kõik DCs globaalse kataloogi serverid. See suurenevad ei kulud, sest seal pole täiendavad kopeerimise liikluse.

Multi-domain mets, GCs on vaja laiendamiseks universaalne rühmaliikmeid autentimise käigus. Kui juurutate GC töökoormus virtuaalse võrgus, mis on näiteks Põhiliselt Azure autentimiseks loob kaudselt väljaminevat autentimist liikluse päringu kohapealse GCs iga autentimise katse ajal.

Seotud GCs kulud on vähem hõlpsalt prognoosida, sest need hosti iga domeeni (sisse osa). Kui töökoormus majutab teenuse Interneti-ühendusega ja autendib kasutajad Windows Server AD DS vastu, võivad kulud täielikult ettearvamatuid. Aitab vähendada GC päringute cloud saidilt autentimisel, saate [lubada universaalse rühma liikmeks vahemällu salvestamine](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Installimeetod

Peab teil valida, kuidas installida DCs virtuaalse võrgu.

- Saab reklaamida uue DCs. Lisateabe saamiseks lugege teemat [uue Active Directory kogumis Azure virtuaalse võrgus installida](active-directory-new-forest-virtual-machine.md).

- Kohapealse näiteks virtuaalse Põhiliselt, VHD teisaldada pilve. Sel juhul peate veenduma, et kohapealse virtuaalse näiteks Põhiliselt "teisaldatakse," ei "kopeeritud" või "kloonitud."

Kasutage ainult Azure'i virtuaalmasinates DCs (erinevalt Azure "web" või "töötaja" roll VMs). Need on püsival ja kestvus on näiteks Põhiliselt jaoks on vaja. Azure'i virtuaalmasinates on mõeldud näiteks DCs töökoormus.

Ärge kasutage SYSPREP kasutuselevõtuks või klooni DCs. Võimalus klooni DCs on ainult saadaval alates Windows Server 2012. Funktsiooni kloonimise vajab tugi VMGenerationID aluseks hypervisor. Mõlemad toeta Hyper-V Windows Server 2012 ja Azure virtuaalse võrkudes VMGenerationID, nagu muude tootjate virtualiseerimise tarkvara müüjad.

### <a name="BKMK_PlaceDB"></a>Windows Server AD DS andmebaas ja SYSVOL paigutamine

Valige, kuhu Windows Server AD DS andmebaasi, logid ja SYSVOL leidmiseks. Ta saab juurutada Azure'i andmed kettale.

> [AZURE.NOTE] Azure'i andmed ketast on piiratud vahemikuga 1 TB.

Andmete kettadraivide ei ole vahemälu kirjutab vaikimisi. Andmete kettadraivide, mis on lisatud VM kasutada vahemällu kirjutamine kaudu. Kirjutage-kaudu vahemällu kontrollib, kas selle kirjutamine on kinnitatud püsival Azure storage enne tehingu on lõpule jõudnud, kuvatakse VM operatsioonisüsteemi seisukohast. Pakub kestvus veidi aeglasem kirjutab arvelt.

See on oluline Windows Server AD DS, kuna maha kirjutada ketta vahemällu oletused näiteks Põhiliselt muudab kehtetuks. Windows Server AD DS proovib keelamine kirjutada vahemällu, kuid on IO süsteemi au see ketas. Tõrge keelamiseks kirjutada vahemällu võivad teatud tingimustel kehtestada USN tagasipööramine tulemuseks ikka objektid ja muude probleemide korral.

Hea tava virtuaalse DCs, tehke järgmist.

- Määrake säte Host vahemälu eelistused Azure'i andmed kettal jaoks pole. See takistab probleeme kirjutada vahemällu AD DS-analüüsitoiminguid.

- Salvestama andmebaasi, logid ja SYSVOL kas sama andmete kettale või andmeid eraldi ketast. Tavaliselt on eraldi ketas kasutatud operatsioonisüsteem ise kettalt. Võti buffee on see, et Windows Server AD DS andmebaas ja SYSVOL ei tohi hoida on Azure operatsioonisüsteemi ketta tüüp. Vaikimisi installib AD DS installitoimingut % systemroot % kausta, mis ei ole soovitatav Azure järgmised komponendid.

### <a name="BKMK_BUR"></a>Varundus ja taaste

Arvestage, mis on ei toetata varundus ja taaste, on näiteks Põhiliselt üldiselt ja täpsemalt need töötavad VM. [Varundus ja taaste kaalutluste kohta virtualiseeritud domeenikontrollereid](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers)näha.

Süsteemi oleku varukoopiaid abil luua ainult varundustarkvara, mis on spetsiaalselt teadlikud varukoopia nõuded Windows Server AD DS, näiteks Windows Serveri varukoopia.

Kopeerige või klooni VHD failid DCs asemel korrapäraste varukoopiate plaanimine. Peaks taastamise kunagi olla nõutav, tehes abil kloonitud või kopeeritud VHDs ilma Windows Server 2012 ja toetatud hypervisor tutvustab USN mullid.

### <a name="BKMK_FedSrvConfig"></a>Federation serveri konfigureerimine

Konfiguratsiooni Windows Server AD FS-i liiduserverite (STSs), sõltub osaliselt kas rakendusi, mida soovite juurutada Azure tuleb kohapealse võrgu ressurssidele juurdepääsu.

Kui rakenduste vastate järgmistele kriteeriumitele, võib juurutamist rakenduste eraldi kohapealse võrgu kaudu.

- Aktsepteerimist SAML turvalisus sõned

- Need on exposable Interneti-ühendus

- Juurdepääs kohapealse ressursid

Sel juhul konfigureerimine Windows Server AD FS-i STSs järgmiselt:

1. Saate konfigureerida Azure on eraldatud ühekordse domeenide mets.

2. Sisestage välise juurdepääsu mets Windows Server AD FS liiduserveri serveripargi konfigureerimisega.

3. Kohapealse mets konfigureerimine Windows Server AD FS-i (federation serveripargi ja liiduserveri puhverserveri serveripargi).

4. Luua federation usaldusseos kohapealse ja Azure eksemplarid Windows Server AD FS-i vahel.

Teisalt, kui rakenduste kasutamiseks on vaja juurdepääsu kohapealse ressursse, võib konfigureerite Windows Server AD FS-i rakenduse Azure järgmiselt:

1. Konfigureerida kohapealse võrgu ja Azure Ühenduvus.

2. Windows Server AD FS liiduserveri serveripargi konfigureerida kohapealse mets.

3. Azure'i Windows Server AD FS liiduserveri serveripargi puhverserveri konfigureerimine.

Selle konfiguratsiooni on vähendada kohapealse ressursse, mis on sarnane Windows Server AD FS-i konfigureerimine rakendustega perimeetri võrku käes.

Pange tähele, kas stsenaarium saate luua usalduste seosed veel identiteedipakkujad, kus business ettevõtte koostöö vajadusel.

### <a name="BKMK_CloudSvcConfig"></a>Cloud services konfigureerimine

Pilveteenustega on vaja, kui soovite seada VM Internetiga otse või Interneti-ühendusega koormusetasakaalustusega rakenduse kuvamiseks. See on võimalik, sest iga pilveteenuses pakub konfigureeritav virtuaalse IP-aadressi.

### <a name="BKMK_FedReqVIPDIP"></a>Federation serveri nõuded avalike ja privaatvõ IP-aadresside (dünaamilise IP vs virtuaalse IP)

Iga Azure virtuaalse masina saab dünaamilise IP-aadressi. Dünaamiline IP-aadress on juurdepääsetav ainult Azure'is privaatne aadress. Enamikul juhtudel siiski tuleb konfigureerida oma Windows Server AD FS-i juurutuste virtuaalse IP-aadress. Virtuaalne IP on vaja Windows Server AD FS-i lõpp-punktid Interneti nähtavaks tegemine ja kasutab ühendatud partnerid ja kliendid autentimis- ja jooksvate haldustoimingute kohta. Virtuaalne IP-aadress on atribuut pilveteenus, mis sisaldab ühe või mitme Azure'i virtuaalmasinates. Kui nõuded meeles rakendus, mis on Azure ja Windows Server AD FS-i juurutatud on Interneti-ühendusega nii ühiskasutus levinud pordid, iga nõuab oma virtuaalse IP-aadress ja see on vaja luua ühe pilveteenuses rakenduse ja teine Windows Server AD FS-i.

Mõisted virtuaalse IP-aadress ja dünaamilise IP-aadressi leiate [tingimused ja määratlused](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS-i kõrge-saadavus konfigureerimine

See on võimalik juurutada autonoomse Windows Server AD FS-i federation services, on soovitatav juurutada serveriparki vähemalt kaks sõlme AD FS-i STS-ja puhverserverite tootmise keskkonna jaoks.

Teemast [AD FS 2.0 juurutamise topoloogia kaalutlused](https://technet.microsoft.com/library/gg982489) on [AD FS 2.0 kujundus juhend](https://technet.microsoft.com/library/dd807036) otsustada, millist konfiguratsiooni Juurutussuvandid parim oma kindla vajadustele.

> [AZURE.NOTE] Laadi jaoks Windows Server AD FS-i lõpp-punktid Azure'i saamiseks sama pilveteenuses kõikidele liikmetele serveripargi Windows Server AD FS-i konfigureerimine ja kasutage koormust tasakaalustavad võimalus Azure http (vaikimisi 80) ja HTTPS pordid (vaikimisi 443). Lisateabe saamiseks lugege teemat [Azure laadi-koormusetasakaalustusteenuse juures](https://msdn.microsoft.com/library/azure/jj151530).
Azure'i Windows Server Network laadi tasakaalustamiseks (NLB) ei toetata.
