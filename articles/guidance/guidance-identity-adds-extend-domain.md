<properties
   pageTitle="Azure'i arhitektuur viide - IaaS: Ulatub Azure Active Directory | Microsoft Azure'i"
   description="Kuidas rakendada turvaline hübriid võrgu arhitektuur Active Directory autoriseerimine ja Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Active Directory kataloogiteenuste ulatub (LISAB) Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse head tavad ulatub keskkonna Active Directory (AD) Azure esitada jaotatud autentimisteenuste [Active Directory domeeniteenused (AD DS)]abil[active-directory-domain-services]. See arhitektuur laiendab [rakendamise turvaline hübriid võrgu arhitektuur Azure] artikleid kirjeldatud[ implementing-a-secure-hybrid-network-architecture] ja [rakendamiseks turvaline hübriid võrgu arhitektuur Interneti-juurdepääsu Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. See viide arhitektuur kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

Kasutate AD DS autentida identiteedid. Need identiteedid saate kuuluvad kasutajad, arvutid, rakenduste või muud ressursid, mis moodustavad osa turvalisus domeeni. Saate majutada kohapealse AD DS, kuid elementide rakenduse asukohta Azure'i hübriidjuurutuse stsenaarium võib olla tõhusam kopeerida see funktsioon ja AD hoidla pilveteenusesse. Seda moodust saab vähendada, saates autentimine põhjustada latentsus ja kohalik luba kutsed pilvest tagasi AD DS kohapealse töötab. 

On kaks võimalust majutada oma kataloogiteenuste Azure.

1. Saate kasutada [Azure Active Directory (AAD)] [ azure-active-directory] luua uue AD domeeni pilves ja linkida selle asutusesisese AD domeeni. Siis setup [Azure'i AD-ühenduse] [ azure-ad-connect] sisse – lubab ise olevad identiteedid on kohapealse hoidla pilveteenusesse. Pange tähele, et kataloogi pilveteenuses on **pole** kohapealse süsteemi laiend, pigem on koopia, mis sisaldab sama identiteedid. Nende kohapealse pilves, kuid pilveteenuses, **ei ole** tehtud muudatused kopeeritakse identiteedid tehtud muudatusi paljundada tagasi kohapealse domeeni. Näiteks parooli lähtestamise tuleb sooritada kohapealse ja kopeerida muutmine pilveteenusesse Azure'i AD-ühenduse abil. Samuti, võtke arvesse, et samas eksemplaris AAD saab linkida mitme eksemplari AD DS; AAD sisaldab iga AD hoidla, millele on lingitud identiteedid.

    AAD on kasulik olukordades, kus kohapealse võrgu- ja Azure virtuaalse võrgu majutusteenuse cloud ressursid ei ole otse seotud VPN tunneliga või ExpressRoute ringi abil. Kuigi see lahendus on lihtne, ei pruugi sobib süsteemid kus komponendid võivad migreerimine üle kohta – ruumide/cloud piiri nagu see võib nõuda ümberkonfigureerimist AAD. Lisaks AAD tegeleb ainult kasutaja autentimise asemel arvuti autentimine. Teatud rakenduste ja teenuste, nt SQL Server võib olla vaja arvuti autentimise sel juhul see lahendus on sobiv.

2. Saate juurutada domeenikontrolleri Azure AD DS töötab, AD infrastruktuuri ulatub oma kohapealse andmekeskuse VM. See lähenemine on rohkem levinud stsenaariumid, kus kohapealse võrgu- ja Azure virtuaalse võrgu on ühendatud VPN-ja/või ExpressRoute ühendus. See lahendus toetab samuti kahesuunalise dispersioonanalüüs, mis võimaldab teil muuta pilves ja kohapealse, on kõige kohasem. Olenevalt teie turbenõuetele AD installimise pilveteenuses võib olla:
    - osa selle hoitakse kohapealse sama Domeen
    - uue domeeni jooksul ühiskasutusega mets
    - eraldi mets

<!-- we might want to add info on how to choose from the three options above -->

See arhitektuur keskendub lahendus 2, kasutades Azure'i ja kohapealsete AD DS domeeni.

Tüüpilised kasutamine karpide see arhitektuur on järgmised.

- Hübriidjuurutuse rakenduste kust töökoormused Käivita osaliselt kohapealse ja osaliselt teemast Azure.

- Rakenduste ja teenuste autentida Active Directory abil.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm tõstab esile see arhitektuur (*suurendamiseks klõpsake*) olulised komponendid. Värvi välja elementide kohta lisateabe saamiseks lugege [rakendamise turvaline hübriid võrgu arhitektuur Azure] [ implementing-a-secure-hybrid-network-architecture] ja [rakendamiseks turvaline hübriid võrgu arhitektuur Interneti-juurdepääsu Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Kohapealse võrgu.** Kohapealse võrgu sisaldab kohalikud AD serverid, mida saab teha autentimise ja luba kohapealse components asuv.

- **AD serverites.** Need on domeenikontrollerid rakendamise directory services (AD DS) töötab VMs pilveteenuses. Järgmistes serverites saate sisestada komponentide käitamine Azure virtuaalse võrgu autentimist.

- **Active Directory alamvõrgu.** AD DS-i serverid on hostitud eraldi alamvõrgu. NSG reeglid AD DS-i serverid kaitse ja saate sisestada tulemüüri vastu liikluse ootamatud allikatest.

- **Azure lüüsi ja AD sünkroonimine.**. Azure'i lüüsi pakub kohapealse võrgu ja Azure VNet vahelist ühendust. See võib olla [VPN-ühendus] [ azure-vpn-gateway] või [Azure'i ExpressRoute][azure-expressroute]. Kõik sünkroonimise taotlusi AD serverid pilves ja kohapealse läbida lüüsi. Kasutaja määratletud marsruudib (UDRs) toime marsruutimine sünkroonimise liikluse, mis edastab otse AD server pilves ja ei läbi olemasoleva võrgu virtuaalse seadmed (NVAs) kasutatakse seda stsenaariumi.

    UDRs ja selle NVAs konfigureerimise kohta leiate lisateavet teemast [rakendamise turvaline hübriid võrgu arhitektuur Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Soovitused

Selles jaotises toodud soovitused AD DS Azure, mis hõlmab:

- Soovitused VM.

- Võrgunduse soovitusi.

- Turvalisus soovitusi. 

- Active Directory saidi soovitusi.

- Active Directory FSMO paigutuse soovitusi.

>[AZURE.NOTE] Dokumendi [juhised juurutamine Windows Server Active Directory Azure'i Virtuaalmasinates] [ ad-azure-guidelines] sisaldab täpsemat teavet nende punktide paljude.

### <a name="vm-recommendations"></a>VM soovitused

Looge VMs piisavalt ressursse toime liikluse oodatud maht. Kasutage majutusteenuse AD DS kohapealne lähtepunktina masinad suurust. Kuvari ressursi kasutamine; saate VMs suuruse ja mastaapimiseks, kui need on liiga suur. Suuruse muutmise AD DS domeenikontrollerid kohta leiate lisateavet teemast [Active Directory Domeeniteenustesse võimsuse kavandamise][capacity-planning-for-adds].

Luua eraldi virtuaalse andmete talletamiseks andmebaasi, logid ja SYSVOL ad kettal. Ärge hoidke neid üksusi samale kettale operatsioonisüsteemi. Arvestage, et vaikimisi andmete ketast, mis on lisatud VM vahemällu kirjutamine kaudu. Siiski saate seda tüüpi vahemällu vastuolus AD DS nõuetele. Seetõttu seadmine *pole*kettal andmete säte *Host vahemälu eelistused* . Lisateabe saamiseks lugege teemat [Windows Server AD DS andmebaasi ja SYSVOL paigutuse][adds-data-disks].

Juurutada vähemalt kaks VMs töötab AD DS domeenikontrolleritena Azure virtuaalse võrgu [kättesaadavus](#Availability-considerations) põhjustel.

### <a name="networking-recommendations"></a>Võrgunduse soovitused

Võrgu liidese konfigureerimine iga VMs majutusteenuse AD DS staatilise privaatne IP-aadressid. Selle konfiguratsiooni toetab DNS paremini iga AD VMs. Lisateabe saamiseks vaadake, [Kuidas määrata staatilise privaatne IP-aadress Azure'i portaalis][set-a-static-ip-address].

> [AZURE.NOTE] Ärge andke AD DS VMs avaliku IP-aadressid. Vt [turvakaalutlused] [ security-considerations] rohkem üksikasju.

Teil tuleb tagada, et liikluse saate liiguks vabalt AD serverid pilves ja kohapealse vahel:

- Lisage NSG reeglid AD alamvõrgu võimaldavate liiklust kohapealse. AD DS kasutada pordid kohta leiate üksikasjalikumat teavet teemast [Active Directory ja Active Directory domeeni teenuste Port nõuetele][ad-ds-ports].

- Veenduge, et UDR tabeleid ei tee AD DS liiklus läbi NVAs, kasutatakse selle stsenaariumi. Oma juurutuste, olenevalt sellest, millist NVAs kasutate, võite selle liikluse ümber suunata.

### <a name="security-recommendations"></a>Turvalisus soovitused

AD DS serverid toime autentimist ja seetõttu väga tundliku üksused. Saate vältida otsest mõju AD DS-i serverid Interneti-ühendus. Asetage eraldi alamvõrku, koos eraldi tulemüüri AD DS-i serverid. Tagada, et vaja kasutada AD DS autentimise ja luba, ja synchronzing serverid pordid avatuks, kuid sulgege kõik muud pordid. Lisateabe saamiseks vt [Active Directory ja Active Directory domeeni teenuste Port nõuetele][ad-ds-ports]. [Lahenduse komponendid] [ solution-components] selle dokumendi jaotist näitab funktsiooni NSG reeglite, et valimi lahendus kasutab avatud pordid.

Saate luua lihtsa tulemüüri NSG reeglid. Kui vajate põhjalikum kaitse saate rakendada mõne suurema turvalisuse tagamiseks perimeetri ümber serverid paari alamvõrku ja NVAs, kasutades dokumendi [rakendamise turvaline hübriid võrgu arhitektuur Interneti-juurdepääsu Azure]kirjeldatud[implementing-a-secure-hybrid-network-architecture-with-internet-access].

Krüptimise BitLockeri või Azure'i ketas ketas majutusteenuse AD DS andmebaasi krüptimiseks.

### <a name="active-directory-site-recommendations"></a>Active Directory saidi soovitused

Saate kasutada AD DS rühma koos domeenikontrollerid, mis on ühendatud kiire link saidid. Domeenikontrollerid sama AD DS saidil ise oma directory muudatused automaatselt ja vähe konfigureerimine on vaja selle dispersioonanalüüs käsitlema.

Juhtida kopeerimise liikluse Azure ja teie asutusesisese andmekeskuse vahelise, on soovitatav lisada eraldi AD DS saidi tähistada Azure'i kasutatavat ruumi aadress. Saate konfigureerida saidi lingi vahel asutusesisese AD DS saidid ja saitidevaheline dispersioonanalüüs tõhusamalt juhtida.

Samuti saate rakendada eri rühmapoliitika objektid (GPO) liidetud arvutites Azure ja kasutada rakendusi, mis on "saidi teada", nt System Center Configuration Manager saidi eraldamine.

### <a name="active-directory-fsmo-placement-recommendations"></a>Active Directory FSMO paigutuse soovitused

Paindlik ühe juhtslaidi toiming (FSMO) serverid on eriotstarbeline domeenikontrollerid reposnsible andmete järjepidevuse üle eri sätteid AD DS, mis on loetletud allpool.

- **Skeemi juhtslaidi**. See on mets kogu rolli, mis säilitab AD DS kasutatavat skeemi struktuuri.

- **Domeeni nime panemise juhtslaidi**. See on mets kogu roll, haldab teavet domeeninimede mets sees.

- **Esmase domeenikontrolleri (PDC)**. See on domeeni hõlmav rollid. PDC tegeleb parooli värskendused ja konto blokeerimise. See on nõu teiste DCs kui hooldustaotlused autentimine on sobimatud paroolid. PDC ka tegeleb rühmapoliitika värskendused ja on näiteks Põhiliselt pärand rakenduste ja mõned administraator tööriistu, mis on mõned kirjutatav toiminguid.

- **Suhteline ID (lahti) juhtslaidi**. RIDs kasutatakse kordumatult tuvastada objektide kataloog. Selle serveri vastutab genereerimine domeeni kogu kogumi RIDs kõik AD serverid domeeni kasutamiseks.

- **Taristu roll**. Ühe domeeni objekti viitamiseks lisadomeeni objekti. Selle domeeni kogu rolli vastutab värskendamine objekti SID ja määratletud nime domeenide objekti viide. Pange tähele, et rakendada selle rolli server ei saa toimida ka globaalse kataloogi g (c) server.

Lisateabe saamiseks lugege teemat [Windows Active Directory FSMO rollide][AD-FSMO-roles-in-windows].

Selle stsenaariumi korral soovitame vältida FSMO rollide domeenikontrolleritega Azure juurutamine. 

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Saadavus AD-serverite jaoks luua. Tagada vähemalt kaks serverid määramine. AD serverid pilveteenuses peaks olema sama domeeni domeenikontrollerid. See võimaldab automaatse dispersioonanalüüs meiliserverite vahel.

Pidage silmas ka tähistavad serverid nimega [Valige toimingud meistrid] [ standby-operations-masters] juhuks, kui ühenduvust serveriga abistas FSMO rolli nurjus.

## <a name="security-considerations"></a>Turvalisuse alused

Kui kõik domeeni administraatori ülesannete võivad kasutada kohapealse DCs, tuleks teha DCs pilves, kirjutuskaitstud. Kirjutuskaitstud näiteks Põhiliselt ainult säilitab alamhulga kasutajate identimisteabe (piisavalt autentida kohalikult) ja saab konfigureerida vahemälu teavet ainult kindlatele kasutajatele. Seetõttu, kui kirjutuskaitstud AV on rikutud, ainult teabe vahemällu talletatud kasutajatele mõjutab, selle asemel, et iga domeeni konto üksikasjad. Lisateavet leiate teemast [Kirjutuskaitstud domeenikontrollerid][read-only-dc].

Vähendada haavatavust kasutajakontode ja katsed sissemurdmine ärahoidmiseks, järgige parimate tavade ja AD kasutajate paroolide haldamine. Lisateabe saamiseks lugege teemat [Parooli poliitikate jõustamine head tavad][best_practices_ad_password_policy]. Samuti olge rühmad saate määrata kasutajatele. Näiteks teha tavaliste kasutajate liikmete ettevõtte administraatorid rühma, skeemi administraatorite rühma ja rühma Domain Admins.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

AD on automaatselt scalable domeenikontrollerid, mis on osa sama domeeni. Taotlusi levitatakse üle kõik andmetöötlusalased domeeni. Saate lisada mõne muu domeenikontrolleri ja sünkroonib automaatselt domeeniga. Konfigureerige eraldi laadi koormusetasakaalustusteenuse domeeni kontrollerid liikluse suunamiseks. Veenduge, et kõik domeenikontrollerid on piisavalt mälu ja salvestusruumi ressursid käsitlema domeeni andmebaasi; ideaalvariandis mitte muuta kõik selle domeenikontrolleri VMs sama suur.

## <a name="management-considerations"></a>Kaalutlused haldus

Kopeerige VHD failid hulka kuuluvad domeeni asemel korrapäraste varukoopiate plaanimine Kuna taastamise võib põhjustada ebakõlad domeenikontrollerid vahel.

Sulgege ja taaskäivitage VM, mis töötab selle domeenikontrolleri roll Azure'i Külastajate operatsioonisüsteemi asemel käsk Sule Azure'i portaalis. Azure portaali kaudu sulgeda VM põhjustab VM, et olla deallocated. See toiming lähtestab VM GenerationID, mis on soovimatute domeenikontrolleri. VM-GenerationID lähtestamisel lähtestatakse ka AD hoidla invocationID, RID rakenduskausta ära ning SYSVOL on märgitud nii mitteautoriteetsete.

## <a name="monitoring-considerations"></a>Kaalutlused jälgimine

Jälgimine ja haldamine võrguserveri AD puudumisel võib põhjustada probleeme näiteks:

- **Sisselogimise tõrke**. Sisselogimine võib ebaõnnestuda kogu domeeni või mets kui usalda seose või nimi lahendus nurjub või globaalse kataloogi serveris ei saa määrata ühes kohas rühmakuuluvus.

- **Konto blokeeringu**. Kasutaja- ja teenuse kontod saab lukustuvad, kui esmase domeenikontrolleri pole saadaval või kopeerimine nurjub mitu domeenikontrollerid vahel.

- **Selle domeenikontrolleri tõrge**. Kui ketas, mis sisaldab Ntds.dit fail töötab piisavalt kettaruumi, saate selle domeenikontrolleri peatada toimimist.

- **Rakenduse nurjumine**. Rakendusi, mis on olulised oma äri, näiteks Microsoft Exchange või muu e-posti rakendus, võib nurjuda, kui address book päringute kataloogi nurjuda.

- **Tabelites sisalduvas directory andmed**. Kui kopeerimine nurjub pikema perioodi jooksul, dubleeritud objektide saab luua kataloogis ja võib nõuab olulisel diagnoosimise ja kõrvaldada.

- **Konto loomise tõrge**. Domeenikontrolleri ei saa luua kasutaja või arvuti kontod, kui kulutab osutada suhteline ID-d ja RID on saadaval.

- **Turvalisus poliitika tõrge**. Kui SYSVOL ühiskaust ise õigesti, rühmapoliitika objektid ja turbepoliitikate pole õigesti rakendatakse klientidele.

AD serverid hoolikalt jälgida ja rakendada kiiresti valmis. Looge kontroll-loendi jälgimine sobiva intervalli ülesanded. Näiteks võib plaanida kriitilised järgmisi toiminguid iga päev:

- Uurida ja lahendada teatatud domeenikontrollerid, teatised 

- Veenduge, et kõik domeenikontrollerid saate suhelda ja selle kopeerimine ei tööta.

- Veenduge, et SYSVOL jääb ühiskasutuses.

- Parandab aja sünkroonimisprobleeme.

- Otsige kettaruumi kasutatavaid AD füüsilise kaudu.

Muud tavalised toimingud vähem võib sageli teha. Näiteks võib usalda seosed läbi vaadata ja otsida aegunud või katkenud usalduste iga nädal ja kontrollige, et kõik domeenikontrollerid käivitatud kasutades sama hoolduspakettide ja kiirparandus plaastrid iga kuu.

Lisateavet leiate teemast [Jälgimise Active Directory][monitoring_ad]. Saate installida [Microsoft süsteemide keskus] näiteks[ microsoft_systems_center] jälgimisega seotud server (vt [arhitektuur skeemi][architecture]) abil teha järgmisi toiminguid. 

## <a name="solution-components"></a>Lahenduse komponendid

Lahendus selle arhitektuur loob turvaline hübriid võrku, nagu on kirjeldatud [rakendamisel turvaline hübriid võrgu arhitektuur Azure] dokumendid[ implementing-a-secure-hybrid-network-architecture] ja [rakendamiseks turvaline hübriid võrgu arhitektuur Interneti-juurdepääsu Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], kuid järgmiste üksuste lisamine:

- Mõne Azure ressursirühma nimega *basename*- dns-rg, kus on *basename* eesliite teie määratud lahenduse juurutamisel.

- Kahe Azure VMs nimega *basename*-ad1-vm ja *basename*-ad2-vm, loodud *basename*- dns-rg ressursirühma. Need VMs on konfigureeritud AD serverid kataloogiteenustega ja DNS-i installinud ja konfigureerinud.

- Mõne NSG nimega *basename*-ad-nsg jaotises *basename*- ntwk-rg Azure ressursi. Selle ressursirühma on turvaline hübriid võrku moodustavate infrastruktuuri osa, kuid uute NSG lisamine, mis määratleb turvalisuse sissetulevad reeglid AD serverite, nagu on näidatud järgmises tabelis:


    Priority (prioriteet)|Nimi|Allikas|Sihtkoht|Teenus|Toiming|
    --------|----|------|-----------|-------|------|
    170|VNet-port53|10.0.0.0/16|Mis tahes|Custom(any/53)|Luba|
    180|VNet-port88|10.0.0.0/16|Mis tahes|Custom(any/88)|Luba|
    190|VNet-port135|10.0.0.0/16|Mis tahes|Custom(any/135)|Luba|
    200|VNet-port137 – 9|10.0.0.0/16|Mis tahes|Custom(any/137-139)|Luba|
    210|VNet-port389|10.0.0.0/16|Mis tahes|Custom(any/389)|Luba|
    220|VNet-port464|10.0.0.0/16|Mis tahes|Custom(any/464)|Luba|
    230|VNet-rpc-dünaamiline|10.0.0.0/16|Mis tahes|Custom(any/49152-65535)|Luba|
    240|OnPrem-ad-et-port53|192.168.0.0/24|Mis tahes|Custom(any/53)|Luba|
    250|OnPrem-ad-et-port88|192.168.0.0/24|Mis tahes|Custom(any/88)|Luba|
    260|OnPrem-ad-et-port135|192.168.0.0/24|Mis tahes|Custom(any/135)|Luba|
    270|OnPrem-ad-et-port389|192.168.0.0/24|Mis tahes|Custom(any/389)|Luba|
    280|OnPrem-ad-et-port464|192.168.0.0/24|Mis tahes|Custom(any/464)|Luba|
    290|Mgmt rdp lubamine|10.0.0.128/25|Mis tahes|Custom(any/3389)|Luba|
    300|lüüsi lubamine|10.0.255.224/27|Mis tahes|Custom(any/any)|Luba|
    310|omas lubamine|10.0.255.192/27|Mis tahes|Custom(any/any)|Luba|
    320|VNet-keelamiseks|Mis tahes|Mis tahes|Custom(any/any)|Luba|

    AD DS kasutab pordid 53, 89, 135, 389 ja 464 aktsepteerida, sissetulevad kopeerimise ja autentimise liikluse. Selles tabelis on kohapealse domeenikontrolleri aadress ruumi 192.168.0.0/24 (sõltub teie aadressiruumi – saate määrata selle teabe parameetrina malle, mida lahendus.

    Funktsiooni NSG määratleb ka järgmised Väljamineva meili Turve sätted, mis võimaldavad sünkroonimise ja luba liikluse vool naasta kohapealse võrgu:

    Priority (prioriteet)|Nimi|Allikas|Sihtkoht|Teenus|Toiming|
    --------|----|------|-----------|-------|------|
    100|välja-port53|Mis tahes|192.168.0.0/24|Custom(any/53)|Luba|
    110|välja-port88|Mis tahes|192.168.0.0/24|Custom(any/88)|Luba|
    120|välja-port135|Mis tahes|192.168.0.0/24|Custom(any/135)|Luba|
    130|välja-port389|Mis tahes|192.168.0.0/24|Custom(any/389)|Luba|
    140|kontorist-port 445|Mis tahes|192.168.0.0/24|Custom(any/445)|Luba|
    150|välja-port464|Mis tahes|192.168.0.0/24|Custom(any/464)|Luba|
    160|kontorist-rpc-dünaamiline|Mis tahes|192.168.0.0/24|Custom(any/49152-65535)|Luba|

Lahendus koos skripti teha ka järgmist:

- See lisab *basename*-ad1-vm ja *basename*-ad2-vm serverid domeenikontrolleritena domeeni. Selle domeenikontrolleri kohapealse *Active Directory kasutajad ja arvutid* konsooli saate vaadata järgmistes serverites:

![[1]][1]

- See loob uue alamvõrgu (10.0.0.0/16) AD-saidi nimega Azure'i VNet Ad saidil domeenile. See sait sisaldab *basename*-ad1-vm ja *basename*-ad2-vm serverites. 

- See lisatakse IP saidi transport sätted, mis konfigureerida kohapealse saidi ja domeenikontrollerid dispersioonanalüüs intervalli pilveteenuses. Näete sätete alamvõrku, saitide ja selle domeenikontrolleri kohapealse *Active Directory saidid ja serverite* konsooli transport sätted:

![[2]][2]

## <a name="deployment"></a>Juurutamine

Valimi lahendus on järgmine prerequsites.

- Olete juba konfigureerinud asutusesisese domeeni ja, et teil on konfigureeritud DNS-i ja installitud marsruutimine ja Remote Access services toetamiseks VPN ühenduse Azure'i VPN-lüüsi.


- Azure'i CLI uusim versioon on installitud. [Järgige neid juhiseid üksikasjad][cli-install].

- Kui lahendus Windows juurutamist peate installima tööriista, mis pakub bash shell, näiteks [GitHub töölaud][github-desktop].

>[AZURE.NOTE] Kui teil pole juurdepääsu olemasoleva kohapealse domeeni, saate luua abil [onpremdeploy.sh] testimiskeskkonnas[ onpremdeploy] bash skripti. Skript loob võrgu- ja VM pilves, mis jäljendab väga lihtne kohapealse häälestus. Enne töötab see skript redigeerimine ja määrake järgmised muutujad määratletud faili alguses.
>
> - **BASE_NAME**. Kasutaja määratletud eesliite ressursirühm ja loodud skripti VM. See üksus peaks olema **pikem kui 5 märki** muidu skript proovib luua sobimatu nimega VM ja nurjuda.
> 
> - **Tellimuse**. Oma Azure tellimuse ID-ga. See suscription luuakse ressursirühma.
> 
> - **Asukoht**. Azure'i asukoht, kus soovite luua ressursirühma, nt *eastus* või *westus*.
> 
> - **ADMIN_USER_NAME**. Kasutada VM administraatori konto nimi.
> 
> - **ADMIN_PASSWORD**. Administraatori konto parool.

Järgmiste toimingute luua valimi lahenduse:

1. Laadige alla ja redigeerige [azuredeploy.sh] [ azuredeploy] script ja määrake faili alguses järgmisi:

    - **BASE_NAME**. Kasutaja määratletud eesliite ressursi rühmad ja loodud skripti VMs. Nagu enne selle üksuse peaks olema **pikem kui 5 märki**.

    - **Tellimuse**. Oma Azure tellimuse ID-ga.

    - **OS_TYPE**. Operatsioonisüsteem (*Windowsi* või *Linuxi*) veebi-, äri- ja Accessi taseme VMs kasutamiseks. Pange tähele, et kõik AD serverid loodud skripti käivitada Windows Server 2012, sõltumata sellest sättest.

    - **Domeeni_nimi**. Kohapealse domeeni nimi. Pange tähele, et kui te kasutate keskkond, mis on loodud onpremdeploy.sh skripti, see peab olema *contoso.com*.

    - **Asukoht**. Azure'i asukoht, kus soovite luua ressursi rühmad.

    - **ADMIN_USER_NAME**. Erinevate VM administraatorikontode jaoks nimi.

    - **ADMIN_PASSWORD**. Administraatori konto parool.

    - **ON_PREMISES_PUBLIC_IP**. Avaliku IP-aadress kohapealse VPN kohapeal.

    - **ON_PREMISES_ADDRESS_SPACE**. Kohapealse võrgu sisemise aadressiruumi. Kui kasutate keskkond, mis on loodud onpremdeploy.sh skripti, tuleb see 192.168.0.0/16.

    - **VPN_IPSEC_SHARED_KEY**. IPSec ühiskasutusega kasutatud võti VPN-ühenduse loomiseks kohapealse võrgu ja Azure VPN-lüüsi vahel.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. Kohapealse DNS-i serveri IP-aadress. Kui kasutate keskkond, mis on loodud onpremdeploy.sh skripti, see tuleb 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Kohapealse alamvõrgu aadress eesliide. Kui kasutate keskkond, mis on loodud onpremdeploy.sh skripti, tuleb see 192.168.0.0/24.

    >[AZURE.NOTE] Ressursside ja aja säästmiseks ei loo skripti ettevõtetele või andmed Accessi astme. Kui vajate neid üksusi, saate kommenteerige välja järgmist jaotist azuredeploy.sh skripti:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Avage bash shell küsimus ja teisaldamine kausta, mis sisaldab azuredeploy.sh skripti.

3. Azure'i kontosse sisse logida. Bash Shell, sisestage järgmine käsk:

    ```
    azure login
    ```

    Järgige ühenduse Azure'i.

4. Käivitage käsk `./azuredeploy.sh`, ja oodake, kuni skripti loob võrgu taristule.

5. *Kontrollige, kas selle VNet loodud*kuvatakse vastav viip, kasutada Azure portaali kontrollida nimega *basename*- ntwk-rg ressursirühma loodud ja see sisaldab üksuste sarnanevad järgmisel pildil näidatud:

    ![[3]][3]

    >[AZURE.NOTE] Näiteid, *basename* on seatud *pilve* skripti käitamisel.

    Klõpsake *basename*- vnet VNet nuppu *alamvõrku*, ja veenduge, et alamvõrku, allpool on loodud:

    ![[4]][4]

6. Bash shell aknas küsitakse, vajutage klahvi ja oodake, kuni web taseme- ja laadi koormusetasakaalustusteenuse luuakse.

7. *Kontrollige, kas Web taseme on loodud õigesti*kuvatakse vastav viip, kasutada Azure portaali nimetatakse *basename*web-taseme-rg ressursirühma loodud ja, et see sisaldab üksusi, mis on sarnane allpool näidatud:

    ![[5]][5]

8. Bash shell aknas küsitakse, vajutage klahvi ja oodake, kuni soovitud NVAs luuakse.

9. *Kontrollige, kas selle Dessandi on loodud õigesti*kuvatakse vastav viip, kasutada Azure portaali kontrollige, et nimega *basename*- mgmt-rg ressursirühma on loodud järgmised sisuga.

    ![[6]][6]

10. Bash shell aknas küsitakse, vajutage klahvi ja oodake, kuni soovitud jumpbox on loodud.

11. *Veenduge, et jumpbox on loodud õigesti*kuvatakse vastav viip, kasutada Azure portaali kontrollimaks, et järgmised üksused on lisatud *basename*- mgmt-rg ressursirühm:

    - Mõne kättesaadavus komplekti nimetatakse *basename*- jb-nimega.

    - VM nimega *basename*- jb-vm.

    - Võrgu liidese nimega *basename*- jb-nic.

12. Bash shell aknas küsitakse, vajutage klahvi ja oodake, kuni Azure'i VPN-lüüsi ja ühendus luuakse. Arvestage, et seda toimingut saavad võtta kuni 30 minutit.

13. *Veenduge, et VPN-lüüsi on loodud õigesti*kuvatakse vastav viip, kasutada Azure portaali kontrollimaks, et järgmised üksused on lisatud *basename*- ntwk-rg ressursirühm:

    - Lüüsi kohtvõrgu nimetatakse klõpsake tööruumide lgw.
    
    - Virtuaalse võrgu lüüsi nimetatakse *basename*- vpngw.

    - Ühenduse lüüs nimega *basename*- vnet-vpnconn. Pange tähele, et selle ühenduse oleku võib olla *pole ühendatud* , kui te pole veel konfigureeritud kohapealse lõppu ühendus; See on hiljem aadress.

14. Bash shell aknas küsitakse, vajutage klahvi ja oodake, kuni luuakse VMs ja muud ressursid DMZ.

15. *Veenduge, et DMZ on loodud õigesti*kuvatakse vastav viip, kasutada Azure portaali kontrollige, et nimega *basename*-dmz-rg ressursirühma on loodud järgmised sisuga.

    ![[7]][7]

16. Bash shell aknas küsitakse, vajutage klahvi. Järgmised viipasid peaks kuvatama:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Logige sisse arvutisse kohapealse, mis loob ühenduse lüüs Azure ja konfigureeriks õigesti. Lisada staatilise marsruudib kohapealse lüüsi seadet, mis suunab välispanka 10.0.0.0/16 aadress vahemikus – selle VNet lüüs. Juhised selle tegemiseks sõltub sellest, kuidas te loote. Vt [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN] [ implementing-a-hybrid-network-architecture-with-vpn] lisateabe saamiseks.

    Pange tähele, et leiate Azure'i VPN-lüüsi avaliku IP-aadress, kasutades Azure portaali uurida *basename*- vpngw lüüsi *basename*- ntwk-rg ressursirühm:

    ![[8]][8]

    Saate määrata, kas ühendus on loodud õigesti vaadates *basename*- vnet-vpnconn ühenduse olekut. See peaks olema seatud *ühendatud*.

    Ühenduse testimiseks avatud kaugtöölaua ühendus jumpbox (10.0.0.254) kohapealse võrgu asub seadmes.

17. Bash shell aknas küsitakse, vajutage klahvi. Järgmisel Küsi *suvalist klahvi VNet sätet VNet kohapealse DNS-i osutamiseks värskendada*, vajutage klahvi ja oodake, kuni soovitud VNet DNS-i sätete värskendatakse azuredeploy.sh skripti **ON_PREMISES_DNS_SERVER_ADDRESS** parameetrina määratud väärtuse.

18. Kui küsitakse, *veenduge, et DNS-i server on VNet seadmine on värskendatud*, kasutage Azure portaali *basename*- vnet VNet *basename*- ntwk-rg ressursi jaotises *DNS-serverid* kehtestamine. See peaks olema seatud *Kohandatud DNS-i*ja *esmane DNS-i server* peaks olema asutusesisese DNS-i serveri aadress:

    ![[9]][9]

19. *Vajutage suvalist klahvi ressursirühm AD serverite loomiseks* küsitakse bash shell aknas, vajutage klahvi ja oodake, kuni ressursirühma hoidmiseks AD serverid pilveteenuses on loodud.

20. *Vajutage suvalist klahvi, et luua AD serverite VMs* küsitakse bash shell aknas, vajutage klahvi ja oodake VMs luua ja lisada ressursirühma.

21. *Vajutage suvalist klahvi liituda kohapealse domeeni VMs* kuvamisel Azure'i portaalis ja kontrollige *basename*- dns-rg rühmas loodud ja see sisaldab kaks VMS (*basename*-ad1-vm ja *basename*-ad2-vm):

    ![[10]][10]

22. Ressursirühm *basename*- ntwk-rg sisse ka NSG loodud nimega *basename*-ad-nsg. Uurige selle NSG sissetulevate ja väljaminevate turvalisuse reeglid. Need peaksid olema samad loetletud [komponente] tabelite[ solution-components] jaotis.

23. Bash shell aknas küsitakse, vajutage klahvi ja oodake, kuni VMs lisatakse kohapealse domeeni.

24. Veebisaidil on *minge asutusesisese AD serveri kinnitamaks, et arvutid on lisatud domeenide* Küsi, kohapealse arvutiga ja kontrollige, et domeen on lisatud nii VMs *Active Directory kasutajad ja arvutid* konsooli abil:

    ![[11]][11]

25. Bash shell aknas küsitakse, vajutage klahvi ja oodake, kuni AD kopeerimine saidi loomist Domeen.

26. Veebisaidil on *minge asutusesisese AD serveri kinnitamiseks dispersioonanalüüs saidi loodud* Küsi, kasutage *Active Directory saidid ja teenused* konsooli kontrollige, et dispersioonanalüüs saidi nimega *Azure'i Vnet Ad saidil* on loodud edukalt, koos IP saidi transport link nimega *AzureToOnpremLink*ja alamvõrku, on VNet viitav:

    ![[12]][12]

27. Bash shell aknas küsitakse, vajutage klahvi ja oodake, kuni skripti installib kataloogiteenustega ja DNS-i iga AD VMs.

28. Kui kuvatakse teade *Palun logige sisse iga Azure AD serveriga, et kontrollida, kas kataloogiteenuste on edukalt konfigureeritud* , avatud kaugtöölaua ühendus mõne kohapealse arvutist jumpbox (*basename*- jb vm) ja avage teise kaugtöölaua ühendus on jumpbox esimese AD server (*basename*-ad1-vm). Logige sisse **domeeni_nimi**, **ADMIN_USER_NAME**ja **ADMIN_PASSWORD** azuredeploy.sh skripti määratud. Serveri halduris, veenduge, et AD DS ja DNS-i rollid on mõlemad lisatud. Korrake seda toimingut teise AD server (*basename*-ad2-vm).

29. Bash shell aknas küsitakse, vajutage klahvi. Kui küsitakse, *vajutage suvalist klahvi Azure DNS-i osutamiseks Azure'i VNet DNS-i sätete seadistamiseks* , vajutage klahvi ja luba script on VNet DNS-i sätete värskendamine.

30. Kui kuvatakse viip *veenduge, et VNet DNS-i säte on värskendatud viide Azure'i VM DNS-i serverid* kuvatakse, kasutades Azure portaali sisse *basename*- vnet VNet *basename*- ntwk-rg ressursi jaotises *DNS-serverid* kehtestamine. Põhi- ja DNS-serverid peaks nüüd kaks AD VMs viide:

    ![[13]][13]

31. Taaskäivitage iga AD VMs enne jätkamist. See toiming on vaja tagada, et nad iga hooleks õige DNS-i sätted, Azure. Oodake enne jätkamist töötab nii VMs.

32. Bash shell aknas küsitakse, vajutage klahvi. Järgmise küsitakse, *vajutage suvalist klahvi rakendamiseks lüüsi UDR lüüsi alamvõrgu (see võib olla eemaldatud)*, vajutage klahvi ja luba skripti värskendada lüüsi UDR.

33. Veenduge, et skripti lõpulejõudmist edukalt.

## <a name="next-steps"></a>Järgmised sammud

- Lugege heade tavade loomiseks [on lisatakse ressursi mets] [ adds-resource-forest] Azure.

- Lugege heade tavade loomise [ADFS-i infrastruktuur] [ adfs] Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Turvaline hübriid võrgu arhitektuur Active Directoryga"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "Active Directory kasutajad ja arvutid konsooli kaks Azure VMs serverid loetelu"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Active Directory saidid ja teenused konsooli nähtaval pilveteenuses saidi sätete dispersioonanalüüs"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "Sisu basename-ntwk-rg ressursirühm"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "Funktsiooni alamvõrku basename-vnet VNet"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "Web esimese taseme üksuste"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "NVAs jaotises basename-mgmt-rg ressurss"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "Ressursside kohta basename-dmz-rg ressursirühm"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Azure'i VPN-lüüsi avaliku IP-aadressi leidmine"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "DNS-serveri sätete jaoks soovitud * basename *-vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "Funktsiooni * basename *-DNS-i-sisaldav AD server VMs rg ressursirühm"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "Active Directory kasutajad ja arvutid konsooli loetelu reklaami server VMs isikuid domeeni"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Active Directory saidid ja teenused konsooli dispersioonanalüüs saidi Azure AD-serverite jaoks nähtaval"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "DNS-i serveri sätete basename-vnet VNet, AD serverid pilveteenuses viitamine"