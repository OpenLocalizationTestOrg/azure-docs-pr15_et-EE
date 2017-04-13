<properties
   pageTitle="ADFS-i rakendamine Azure | Microsoft Azure'i"
   description="Kuidas rakendada turvaline hübriid võrgu arhitektuur Active Directory Federation teenust autoriseerimine ja Azure."
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
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Azure Active Directory Federation Services (ADFS) rakendamine

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse head tavad rakendamiseks turvaline hübriid võrku, mis ulatub kohapealse võrgu Azure ja [Active Directory Federation Services (ADFS)] , mis kasutab[ active-directory-federation-services] sooritamiseks ühendatud autentimise ja luba komponentide töötab pilveteenuses. See arhitektuur laiendab kirjeldatud [Ulatub Active Directory Azure]struktuuri[implementing-active-directory].

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. See viide arhitektuur kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

ADFS-i käitamise kohapealse, kuid hübriidjuurutuse stsenaariumi, kus rakendused asuvad Azure saab tõhusamalt rakendada selle funktsiooni pilveteenuses. Tüüpilised kasutamine karpide see arhitektuur on järgmised.

- Hübriidjuurutuse rakenduste kust töökoormused Käivita osaliselt kohapealse ja osaliselt teemast Azure.

- Lahendusi, mis kasutavad ühendatud autoriseerimine esitamist veebirakenduste partneri ettevõtted.

- Veebibrauserid töötab väljaspool ettevõtte tulemüüri juurdepääsu toetavatest meilisüsteemidest.

- Süsteemid, mis võimaldavad kasutajatel juurdepääs veebirakenduste, ühendades volitatud välise seadmest, nt kaugarvutite, märkmike ja muude mobiilseadmete jaoks. 

ADFS-i tööpõhimõtete kohta leiate lisateavet teemast [Active Directory Federation Services ülevaade][active-directory-federation-services-overview]. Lisaks artiklis [ADFS-i juurutamist Azure] [ adfs-intro] sisaldab üksikasjalikku samm-sammult tutvustus Azure ADFS-i rakendamine.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm tõstab esile see arhitektuur (*suurendamiseks klõpsake*) olulised komponendid. Mis tahes osa, mis pole seotud ADFS-i kohta lisateabe saamiseks lugege [rakendamise turvaline hübriid võrgu arhitektuur Azure][implementing-a-secure-hybrid-network-architecture], [rakendamise turvaline hübriid võrgu arhitektuur Interneti-juurdepääsu Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], ja [rakendada turvaline hübriid võrgu arhitektuur Azure Active Directory identiteetidega][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] See diagramm on kujutatud kasutada järgmistel juhtudel:
>
>- Rakenduse koodi töötab partneri organisatsiooni sees olev oma Azure'i VNet majutatud veebirakenduse.
>
>- Rakenduse välise, registreeritud kasutaja (mandaadiga talletatud lisatakse) juurdepääs teie Azure'i VNet majutatud veebirakenduse.
>
>- Kasutaja ühenduse oma VNet volitatud seadmes ning käivitage oma Azure'i VNet majutatud veebirakenduse.
>
>Lisaks selles arhitektuur keskendub passiivne federation, kus liiduserveris otsustada, kuidas ja millal kasutaja autentimiseks. Kasutaja oodatakse sisselogimisteave rakenduse käivitamisel töötab. See on on kõige sagedamini kasutatavaid veebibrauserid ja hõlmab protokolli, mis suunab brauseris saidile, kuhu kasutaja saab sisestada oma volitusi. ADFS-i toetab samuti aktiivne federation, millega kohustusi ilma edasise kasutaja suhtluse mandaadi sisestamise võtab rakenduse, kuid sel juhul on see arhitektuur väljapoole.

- **LIIDAB alamvõrgu.** LIIDAB serverid sisalduvad oma alamvõrgu. NSG reeglid kaitsta lisatakse serverid ja saate sisestada tulemüüri vastu liikluse ootamatud allikatest.

- **LISAB serverites.** Need on pilves VMs töötab domeenikontrollerid. Järgmistes serverites saate sisestada domeeni kohaliku identiteetide autentimist.

- **ADFS-i alamvõrgu.** ADFS-i serverid saab oma alamvõrgu asuvad abistas tulemüüri NSG reeglid.

- **ADFS-i serverid.** ADFS-i serverid pakuvad ühendatud autoriseerimine ja autentimist. See arhitektuur, nad teha järgmisi toiminguid:

    - Nad saavad turvalisus sõned sisaldavate taotluste tehtud partnerite liiduserveri partneri kasutaja nimel. ADFS-i saate kontrollida, et nende tähiste kehtivad enne läbides veebirakenduse Azure opsüsteemi nõuded. Selle ettevõtte veebirakenduse (Azure'i) saate kasutada nende nõuete taotlusi autoriseerida. Selle stsenaariumi korral ettevõtte veebirakendus on *tuginedes tootja*ja on partneri liiduserveri väljaandmisel ülesanne väidab, mis on aru saanud ettevõtte veebirakenduse. Partneri liiduserverite nimetatakse *konto partnerite* Kuna esitamise Juurdepääsutaotluste autenditud kontod partneri asutuse nimel. ADFS-i serverid nimetatakse *Ressursi partnerid* , kuna need võimaldavad juurdepääsu ressursid (sel juhul veebirakenduste).

    - Nad saavad autentida (lisatakse ja [Active Directory seadme registreerimise teenust]kaudu[ADDRS]) ja lubada sissetulevad taotlused veebibrauseri või seade, mida on vaja juurdepääsu oma ettevõtte veebirakenduste välised kasutajad. 

    ADFS-i serverid on konfigureeritud pargis on Azure laadi koormusetasakaalustusteenuse kaudu. See struktuur aitab parandada kättesaadavus ja skaleeritavus. Lisaks tähele, et ADFS-i serverid ei satuks otse Interneti-ühendus, pigem kõik Interneti-liikluse filtreeritakse ADFS-i web teenuserakenduse puhverserverite ja soovitud DMZ kaudu.

- **ADFS-i puhverserveri alamvõrgu.** Puhverserverid ADFS saate sisalduvad oma alamvõrgu kaitse NSG reeglite abil. Selle alamvõrgu serverite puutuvad kogumi võrgu virtuaalne seadmed, mis pakuvad tulemüüri Azure virtuaalse võrgu ja Interneti kaudu.

- **ADFS-i web rakenduse puhverserver (WAP) serverites.** Need arvutid tegutseda ADFS-i serverid sissetulevad taotlused partneri ettevõtted ja seadmed. WAP serverid tegutseda filtri, varjestus ADFS-i serverid otsest juurdepääsu avaliku Interneti kaudu. ADFS-i serverid, sarnaselt juurutamine WAP koormusetasakaalustuseks serveripargi serverites annab teile suurem kättesaadavus ja skaleeritavus. Kui juurutamine eraldiseisev serverid kogum.

    >[AZURE.NOTE] Üksikasjalikku teavet WAP serverid installimise kohta, vt [installima ja konfigureerima Web rakenduse puhverserver][install_and_configure_the_web_application_proxy_server]

- **Partneri organisatsiooni.** See on näide partneri ettevõte, mis töötab veebirakendus, mis taotleb juurdepääsu Azure töötab veebirakendusse. Partneri organisatsiooni oleva liiduserveri autendib taotlused kohalikult ja turvalisus sõned sisaldav ADFS-i Azure opsüsteemi nõuded esitab. ADFS-i Azure kinnitatakse sõned turvalisus ja kui need on lubatud seda saab edastada taotluste lubada need töötavad Azure veebirakendusse.

    >[AZURE.NOTE] Samuti saate konfigureerida VPN tunneliga, kasutades Azure lüüsi pakkuda otsest juurdepääsu ADFS-i usaldusväärsete partnerite jaoks. Need partnerid saadud läbi WAP serverid.

## <a name="recommendations"></a>Soovitused

Selles jaotises toodud soovitused ADFS-i Azure, mis hõlmab:

- Soovitused VM.

- Võrgunduse soovitusi.

- Kättesaadavus soovitusi.

- Turvalisus soovitusi.

- ADFS-i installimise soovitusi.

- ADFS-i usaldada soovitusi.

### <a name="vm-recommendations"></a>VM soovitused

Looge VMs piisavalt ressursse toime liikluse oodatud maht. Kasutage olemasoleva masinad majutusteenuse ADFS kohapealne lähtepunktina suurust. Ressursi kasutamine jälgimine. Saate VMs suuruse ja mastaapimiseks, kui need on liiga suur.

[Opsüsteemi Windows Azure VM]loetletud soovituste[vm-recommendations].

### <a name="networking-recommendations"></a>Võrgunduse soovitused

Võrgu liidese konfigureerimine iga VMs majutusteenuse ADFS-i ja WAP serverite staatilise privaatne IP-aadressid.

Ärge andke ADFS-i VMs avaliku IP-aadressid. Lisateavet leiate teemast [turvakaalutlused][security-considerations].

Määrake eelistatud ja teisene DNS-serverid võrgu liidesed iga ADFS-i ja WAP VM viitamine (mis peaks kasutama DNS-i) LISAB VMs IP-aadress. See toiming on vajalik iga VM domeeniga liituda.

### <a name="availability-recommendations"></a>Kättesaadavus soovitused

Luua mõne ADFS-i serveripargi vähemalt kahe serverid ADFS-i teenuse kättesaadavuse parandamiseks.

Kasutada muu salvestusruumi kontod iga ADFS-i VM serveripargis. Seda moodust aitab tagada, et ei saa ühe salvestusruumi konto teha kogu serveripargis kättesaamatud.

Luua eraldi Azure-saadavus komplekti ADFS-i ja WAP VMs. Veenduge, et igas kogumis on vähemalt kaks VMs. Igas kogumis kättesaadavus peab olema vähemalt kaks update domeenid ja kaks viga domeeni.

Konfigureerige koormus soolise ADFS-i VMs ja WAP VMs järgmiselt:

- Kasutada mõne Azure laadi koormusetasakaalustusteenuse WAP VMs välise juurdepääsu ja on sisemine laadi koormusetasakaalustusteenuse ADFS-i serverites ADFS-i serveripargis Laadi üles.

- Ainult läheb liikluse kuvataks ADFS-i/WAP serverid pordi 443 (HTTPS).

- Pange laadi koormusetasakaalustusteenuse staatiline IP-aadress.

- Looge seisundi juures, kasutades TCP-protokolli, mitte HTTPS. Saate ping pordi 443, kinnitamaks, et ADFS-i server töötab.

    >[AZURE.NOTE] ADFS-i serverid kasutada serveri teave (SNI) protokolli, seega katsel probe abil HTTPS-i lõpp-punkti alla laadimine koormusetasakaalustusteenuse nurjub.

- ADFS-i laadi koormusetasakaalustusteenuse jaoks domeeni DNS-i *A* -kirje lisamine. 

    Määrake laadi koormusetasakaalustusteenuse IP-aadress ja pange nimi domeen (nt adfs.contoso.com). See on nimi, mille juurde klientide ja serverite WAP serveripargi ADFS-i.

### <a name="security-recommendations"></a>Turvalisus soovitused

Saate vältida otsest mõju ADFS-i serverid Interneti-ühendus. ADFS-i serverid on domeeni ühendatud arvutites, mis on täielik loa andmiseks turvalisus sõned. Kui mõni ADFS-i server on rikutud, pahatahtlik kasutaja saab välja täielik juurdepääs sõned kõigi veebirakenduste ja liiduserverite, mis on kaitstud ADFS-i. Kui teie süsteemi peab toime taotluste välised kasutajad ei pruugi olla ühenduse partneri usaldusväärsete saitide, kasutage WAP serverid käsitlema taotlused. Lisateavet leiate teemast [kuhu Liiduserveri puhverserveri][where-to-place-an-fs-proxy].

Koha ADFS-i serverid ja WAP serverid eraldi alamvõrku oma tulemüürid. NSG reeglite abil saate määratleda tulemüüri reeglid. Kui vajate põhjalikum kaitse saate rakendada mõne suurema turvalisuse tagamiseks perimeetri ümber serverid paari alamvõrku ja NVAs, kasutades dokumendi [rakendamise turvaline hübriid võrgu arhitektuur Interneti-juurdepääsu Azure]kirjeldatud[implementing-a-secure-hybrid-network-architecture-with-internet-access]. Pange tähele, et kõik tulemüürid peaks pordi 443 (HTTPS) liikluse lubamiseks.

ADFS-i ja WAP serverid login otsest juurdepääsu piirata. Ainult DevOps töötajate peaks oskama ühendust luua.

Ära Liitu WAP serverid domeeni.

### <a name="adfs-installation-recommendations"></a>ADFS-i installimise soovitused

Artikli [juurutamine Federation serveripargi] [ Deploying_a_federation_server_farm] pakub üksikasjalikke juhiseid installimine ja konfigureerimine ADFS-i jaoks. Enne esimest ADFS-i serveri konfigureerimist serveripargis teha järgmist:

1. Hankige avalikult usaldusväärne sert läbimiseks serveri autentimine. *Subjekti nimi* peab sisaldama nime, mille juurde kliendid federation teenus. See võib DNS-i nimi, laadi koormusetasakaalustusteenuse, näiteks *adfs.contoso.com* jaoks registreeritud (Vältige metamärkide nimed nagu **. contoso.com*, turvalisuse põhjustel). Kasutage sama serdi kõik ADFS-i serveri VMs. Saate osta usaldusväärne sertimiskeskus tunnistus, kuid kui teie asutuses on kasutusel Active Directory Certificate Services saate luua oma. 

    *Teema alternatiivne nimi* kasutatakse DRS seadmetest välise juurdepääsu lubamine. See peaks olema vormi *enterpriseregistration.contoso.com*.

    Lisateavet leiate teemadest [Hangi ja konfigureerimine ADFS-i jaoks SSL-serdi][adfs_certificates].

2. Selle domeenikontrolleri luua uus root võti teenuse klahv jaotuse. Praeguse kellaaja miinus 10 tundi (selle konfiguratsiooni vähendab viivitus, mis võivad esineda levitamise ja sünkroonimise kogu domeeni klahvid) olevat tõhus kellaaja. See toiming on vaja toetada ADFS-i teenuse käivitamiseks kasutatud rühma teenusekonto loomine. PowerShelli käsk esitab näide sellest, kuidas seda teha.

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Domeeni lisamiseks iga ADFS-i serverisse VM.

>[AZURE.NOTE] ADFS-i installimiseks tuleb selle domeenikontrolleri töötab PDC emulaator FSMO rolli domeeni töötama ning VMs ADFS-i kaudu juurdepääsetavad.

### <a name="adfs-trust-recommendations"></a>ADFS-i usalduskeskuse soovitused

Luua federation usalda ADFS-i installi ja mis tahes partneri ettevõtted liiduserveris vahel. Mis tahes taotluste filtreerimise ja nõutav vastenduse konfigureerimine. Selle toimingu jaoks on vaja:

- DevOps töötajate **iga partneri organisatsiooni** tuginedes poole usaldus kaudu oma ADFS-i serverid veebirakenduste lisada.

- DevOps töötajate **ettevõtte** konfigureerida nõuded pakkuja usalda, et lubada teie ADFS-i serverid taotluste, mis pakuvad partneri ettevõtted usaldada.

- DevOps töötajate **ettevõtte** konfigureerimine ADFS-i taotluste kellelegi oma ettevõtte veebirakenduste edasi.

    Lisateabe saamiseks lugege teemat [Loomise Federation usaldada][establishing-federation-trust].

Avaldada oma ettevõtte veebirakenduste avaldatakse ja välispartnereid abil, kasutades eelautentimist Servers WAP. Lisateabe saamiseks lugege teemat [rakenduste avaldamine abil ADFS-i Preauthentication][publish_applications_using_AD_FS_preauthentication]

Pange tähele, et ADFS-i toetab Turbeloa teisendamiseks ja suurendamise. Azure Active Directory ei näe seda funktsiooni. ADFS-i, kui häälestate usalda seoste abil saate:

- Konfigureerige taotluste autoriseerimine reeglid. Näiteks saate vastendada kaudu, mitte-Microsofti partneri organisatsioon midagi, mida selle lisatakse saate lubada teie asutuses kasutatakse kujutis turvalisuse tagamist.

- Teisendamine ühest vormingust teise nõuded. Näiteks saate vastendada kaudu SAML 2.0 SAML 1.1 kui teie rakendus toetab ainult SAML 1.1 nõuded. 

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

SQL serveri või Windowsi sisemine andmebaas (WID) abil saate ADFS-i konfigureerimine teabega. WID pakub lihtsa koondamise. Muudatuste kirjutada otse lisada ainult üks ADFS-i andmebaasid ADFS-i kobar, ajal muudes serverites pull kopeerimise abil oma andmebaasid ajakohastamine. SQL serveri abil saate sisestada täieliku andmebaasi koondamise ja kõrge kättesaadavus Tõrkesiirde rühmitamise või peegelduse abil.

## <a name="security-considerations"></a>Turvalisuse alused

ADFS-i kasutab HTTPS-protokolli, seega veenduge, et NSG reeglid alamvõrku, mis sisaldab veebis taseme VMs luba HTTPS päringuid. Taotlused võib pärineda kohapealse võrgu, alamvõrku, mis sisaldab web taseme, business taseme, andmete taseme, Privaatne DMZ, avaliku DMZ ja alamvõrku, mis sisaldab ADFS-i serverid.

Kaaluge võrgu virtuaalse seadmete komplekt, mis logib üksikasjalikku teavet liikluse liiklevad serva, virtuaalse võrgu kontrollimise eesmärgil.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

Järgmisega, summeeritud [ADFS-i juurutamise]dokumendist[plan-your-adfs-deployment], anda alguspunkt ADFS-i parkides sortimine:

- Kui teil on vähem kui 1000 kasutajat, luua sihtotstarbeline ADFS-i serverid, kuid selle asemel ADFS-i installimine iga lisatakse serverid pilves (Veenduge, et teil on vähemalt kaks lisatakse serverid säilitada kättesaadavus). Saate luua ühe WAP serveri.

- Kui teil on 1000 ja 15000 kasutajate vahel, luua kaks sihtotstarbeline ADFS-i serverid ja kaks sihtotstarbeline WAP serverid.

- Kui teil on 15000 ja 60000 kasutajate vahel, luua kolm kuni viis spetsiaalne ADFS-i serverite ja veel vähemalt kahe sihtotstarbeline WAP serverite vahel.

Need arvud Oletame, et kasutate kahekordne neljatuumaline VMs (Standard D4_v2, või parem) majutada Azure serverid.

Pidage meeles, et kui te kasutate Windowsi sisemine andmebaas ADFS-i konfigureerimine andmete talletamiseks, piiratud kaheksa ADFS-i serveripargi serverites. Kui eeldate, et oleks vaja rohkem, siis kasutage SQL serveri. Lisateabe saamiseks lugege teemat [Konfigureerimine ADFS-i andmebaasi osa][adfs-configuration-database].

## <a name="management-considerations"></a>Kaalutlused haldus

DevOps töötajate peaks olema valmis tegema järgmist:

- Liiduserverite (serveripargi ADFS-i haldamise, usalda poliitika liiduserveris haldamiseks ja federation services kasutavad serdid haldamine) haldamise.

- WAP serverid (WAP serveripargi WAP sertide haldamine haldamine) haldamise.

- Haldamise veebirakenduste (osalistele, autentimismeetodeid ja taotluste vastenduste konfigureerimine).

- ADFS-i komponentide varundamine.

## <a name="monitoring-considerations"></a>Kaalutlused jälgimine

[Microsoft süsteemi Center halduspaketi Active Directory Federation Services 2012 R2] [ oms-adfs-pack] pakub nii aktiivne ja reaktiivne jälgimine juurutamise ADFS-i jaoks oleva liiduserveri. See halduspaketi jälgib:

- Sündmuste, et selle ADFS-i teenuse sündmuselogide ADFS-i kirjed.

- ADFS-i jõudlus hinnale kogumise jõudlusandmeid. 

- ADFS-i süsteemis ja web applications (osalistele) üldise seisundi ja antakse teatiste kriitiliste probleemide ja hoiatused.

## <a name="solution-components"></a>Lahenduse komponendid

Proovi lahenduse skripti [Deploy-ReferenceArchitecture.ps1][solution-script], on saadaval, saate rakendada selles artiklis kirjeldatud soovitused järgneva arhitektuur. See skript kasutab Azure ressursihaldur mallid. Mallid on saadaval kogumina olulise koosteüksuste, mis teeb teatud toimingu, näiteks on VNet luua või konfigureerida on NSG. Skripti eesmärk on korraldab malli juurutamine.

Mallid on parameetriga säilitatakse eraldi JSON parameetritega. Saate muuta need failid oma nõuetele vastavuse juurutamist konfigureerimiseks parameetrid. Pole vaja muuta ise malle. Pange tähele, et teil tuleb muuta skeemid objektide parameetri failid.

Mallide redigeerimisel luua objekte, mis nimereeglid, [On soovitatav nimetamise põhimõtted Azure ressursse]kirjeldatud järgige[naming-conventions].

Lahenduse näidis loob ja konfigureerib keskkonna lisatakse alamvõrgu ja server ADFS-i alamvõrgu ja serverid, ADFS-i puhverserveri alamvõrgu ja serverites DMZ, web taseme, business taseme, ja andmed Accessi taseme komponendid, VPN-lüüsi ja juhtimise taseme pilveteenuses. Valimi lahendus ka valikuline loomise jäljendatud kohapealse keskkonna konfiguratsioon.

Järgmistes jaotistes kirjeldada asutusesiseses elementide ja Pilveteenusest konfiguratsioone.

### <a name="on-premises-components"></a>Kohapealse komponendid

>[AZURE.NOTE] Järgmised komponendid pole dokumendis kirjeldatud arhitektuur keskmes ja pakutakse lihtsalt testimiseks cloud keskkonnas turvaliselt, reaal tootmiskeskkonna kasutamise asemel võimalust. Seetõttu Kokkuvõte selles jaotises ainult võtme parameetri failid. Saate muuta sätteid, näiteks IP-aadresside või VMs suurused, kuid on soovitatav paljude parameetrid, muutmata jätta.

See keskkond, mis hõlmab ka AD mets domeeni nimega contoso.com. Domeeni sisaldab LIIDAB kaks serverite IP-aadressid 192.168.0.4 ja 192.168.0.5. Nende kahe serverid käivitada ka DNS-i teenus. Kohaliku administraatorikonto nii VMs nimetatakse `testuser` parooliga `AweS0me@PW`. Lisaks häälestab konfiguratsiooni VPN-lüüsi ühenduse VNet pilveteenuses. Saate muuta konfiguratsiooni, redigeerides järgmised JSON failid, mis asub [**parameetrite/valikul Asutusesisene**] [ on-premises-folder] kaust:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. See fail määratleb võrgu aadressiruumi kohapealse keskkonna jaoks.

- ** [virtualMachines-adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. See fail sisaldab kohapealse VMs lisatakse majutusteenuste konfigureerimine. Vaikimisi luuakse kaks *Standard-DS3-v2* VMs.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** ja ** [connection.parameters.json][on-premises-connection-parameters]**. Nende failide hoidke Azure'i VPN-lüüsi VPN-ühenduse sätete pilves, sh ühiskasutusega võti liikluse liiklevad lüüsi kaitsmiseks kasutada.

Ülejäänud failide kausta sisaldavad kohapealse domeeni, kasutades selle taristu loomiseks kasutatud konfiguratsiooniteavet. Saate need lisatakse installimine, häälestamine DNS-i, luua mets ja dispersioonanalüüs saitide jaoks mets konfigureerimine.

### <a name="cloud-components"></a>Pilveteenuse komponendid

Nende komponentide vormi tuum see arhitektuur. [**Parameetrite/Azure'i**] [ azure-folder] kaust sisaldab järgmisi parameetri faile konfigureerida järgmised komponendid:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. See fail määratleb struktuuri on VNet VMs ja muud komponendid pilveteenuses. See sisaldab sätteid, näiteks nimi, aadressiruumi, alamvõrku ja aadressid, mis tahes DNS-serverid nõutav. Pange tähele, et DNS-i aadressid selles näites viidata kohapealse DNS-serverid ja vaikimisi Azure'i DNS-i server IP-aadressid. Need aadressid viitamiseks oma DNS-i häälestamise, kui te ei kasuta valimi kohapealse keskkonna muutmiseks tehke järgmist.

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines-adds.parameters.json ] [ virtualmachines-adds-parameters] **. Selle faili konfigureerib VMs töötab lisatakse pilveteenuses. Konfiguratsiooni koosneb kahest VMs. Peaksite administraatori kasutajanime ja parooli muutma selle `virtualMachineSettings` jaotist ja saate soovi korral suurust saab muuta VM domeeni nõuetele vastavus:

    Lisateavet leiate teemast [Ulatub Active Directory Azure][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [lisamine – lisab-domeeni-controller.parameters.json][add-adds-domain-controller-parameters]**. See fail sisaldab loomise kestvad lisatakse serverid CONTOSO domeeni sätted. Kasutab kohandatud domeeni luua ja lisada sellele lisatakse serverid laiendid. Kui loote täiendavad lisatakse serverid (sel juhul tuleks lisada neid on `vms` massiiv), muuta nende nimed vaikimisi või soovite seda luua domeeni erineva nimega ei pea te selle faili muutmiseks.

- ** [loadBalancer-adfs.parameters.json] [loadbalancer-adfs-parameters] ** Fail sisaldab kahte tüüpi konfiguratsiooni parameetrid. Funktsiooni `virtualMachineSettings` jaotis määratleb ADFS-i teenuse pilveteenuses majutavad VMs. Vaikimisi skript loob kaks neist VMs sama kättesaadavus määramine:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    Funktsiooni `loadBalancerSettings` jaotises antakse kirjeldus laadi koormusetasakaalustusteenuse need VMs. Laadi koormusetasakaalustusteenuse edastab ühe või teise VMs liikluse, mis kuvatakse pordi 443 (HTTPS):

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [ADFS-i pargi-domeeni join.parameters.json ] [ adfs-farm-domain-join-parameters] **. See fail sisaldab sätteid, mis CONTOSO domeeni lisamiseks ADFS-i serverid. Soovite selle faili muutmiseks, kui olete loonud täiendavaid ADFS-i serverid (värskendamine on `vms` massiiv sel juhul), või olete muutnud domeeni nimi.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [adfs-pargi-first.parameters.json][adfs-farm-first-parameters]**, ja ** [adfs-pargi-rest.parameters.json][adfs-farm-rest-parameters]**. Skript kasutab sätete need failid ADFS-i serveripargi loomiseks. 

    *Gmsa.parameters.json* fail sisaldab ADFS-i teenuse kasutatav hallatava rühma teenuse konto sätted. Kui soovite muuta konto või domeeni nime, saate muuta seda faili.

    *Adfs-pargi-first.parameters.json* fail sisaldab serveripargi ADFS-i loomine ja lisamine esimene server vajalik teave. Kui olete muutnud domeeni või rühma hallatavate teenuse konto nimi, peate värskendama faili.

    *Adfs-pargi-rest.parameters.json* faili saab lisada ülejäänud ADFS-i serverid serveripargis. Uuesti, kui olete muutnud domeeni või rühma hallatavate teenuse konto nimi, peate värskendama faili. Värskenduse funktsiooni `vms` massiiv, kui olete loonud täiendavaid ADFS-i serverid.

- ** [loadBalancer-adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. See fail on sarnane struktuuri ja sisu *loadBalancer-adfs.parameters.json* faili. See sisaldab andmeid, ADFS-i puhverserverid ja laadi koormusetasakaalustusteenuse.

- ** [adfsproxy-pargi-first.parameters.json][adfsproxy-farm-first-parameters]**, ja ** [adfsproxy-pargi-rest.parameters.json][adfsproxy-farm-rest-parameters]**. Skript kasutab sätteid nende failide ADFS-i puhverserveri serveripargi loomiseks. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. See fail sisaldab saab luua ühenduse loomiseks kohapealse võrgu pilveteenuses Azure'i VPN-lüüsi sätted. Muutke soovitud `sharedKey` väärtus on `connectionsSettings` jaotis kohapealse VPN seadme vastavaks. Lisateabe saamiseks lugege teemat [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN][hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** ja ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Nende failide konfigureerimine sissetulevate (avaliku) ja väljaminevate (privaatne) külgedel DMZ, kaitsmine pilveteenuses serverid moodustavate VMs. Järgmisi elemente ja nende konfigureerimise kohta leiate lisateavet teemast [rakendada mõne DMZ Azure ja Interneti vahel][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer-web.parameters-json][loadBalancer-web-parameters]**, ** [loadBalancer-biz.parameters-json][loadBalancer-biz-parameters]**, ja ** [loadBalancer-data.parameters-json][loadBalancer-data-parameters]**. Järgmiste parameetrite failid sisaldavad veebi-, äri- ja juurdepääsu astme VM kirjeldused ja konfigureerige koormus soolise iga taseme jaoks. Need on VMs, mis majutada veebirakenduste ja andmebaaside ja teha business töökoormus asutuse jaoks. Saate muuta suurused ja iga taseme vms arv vastavalt oma vajadustele.

- ** [virtualMachines-mgmt.parameters.json][virtualMachines-mgmt-parameters]**. See fail sisaldab hüpata ruut/halduse VMs konfigureerimine. Ainult on võimalik saada sisse- ja toetatavatele välja hüpata VM veebi-, äri- ja andmete astme. Skripti loob ühe *Standard_DS1_v2* VM vaikimisi, kuid saate muuta selle faili loomiseks suuremaks või täiendava VMs kui halduse töökoormus on tõenäoliselt oluline.

## <a name="solution-deployment"></a>Lahenduse juurutamine

Lahendus eeldab järgmine kohustuslik tarkvara:

- Teil on, kus saate luua ressursirühma Azure olemasolevale tellimusele.

- On alla laadida ja installida Azure PowerShell viimase koostamine. Leiate [siit] [ azure-powershell-download] juhised.

Skripti, mis kasutab lahenduse käivitamiseks tehke järgmist.

1. Kohalikus arvutis mugav kausta teisaldada ja järgmised alamkaustade loomine.

    - Skriptide

    - Skriptide/parameetrid

    - Skriptide/parameetrid/valikul Asutusesisene

    - Azure'i/skriptide/parameetrid

2. Laadige alla [Deploy-ReferenceArchitecture.ps1] [ solution-script] skriptide kausta fail

3. [Parameetrite/valikul Asutusesisene] sisu alla laadida[ on-premises-folder] skriptide/parameetrid/valikul Asutusesisene kausta:

4. [Parameetrite/Azure'i] sisu alla laadida[ azure-folder] skriptide/parameetrid/Azure kausta.

5. Skriptide kausta Deploy-ReferenceArchitecture.ps1 faili redigeerida ja muuta järgmised read määramiseks ressursi rühmad, mida tuleks luua või kasutada hoidke ressursse, mis on loodud skripti:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. Redigeerige parameetri skriptide/parameetrid/valikul Asutusesisene ja skriptide/parameetrid/Azure kaustade failid. Värskendage selle ressursi rühma viited need failid vastavad määratud muutujate Deploy-ReferenceArchitecture.ps1 faili ressursside rühmade nimed. Järgmises tabelis on näidatud, parameetri faile, mis viitavad mis ressursirühma. Pange tähele, et *ra adfs-töökoormus rg*, *ra adfs-turvalisus rg*, *ra-ADFS-i lisab-rg*, *ra adfs-i rg*ja *ra adfs-puhverserveri rg* rühmade kasutatakse ainult PowerShelli skripti parameetri failid ei esine.

  	|Ressursirühm|Parameetri failid|
  	|--------------|--------------|
  	|RA adfs-valikul Asutusesisene rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|RA adfs-võrgu rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-Public.parameters.JSON<br />parameters\azure\loadBalancer-ADFS.parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.parameters.JSON<br />parameters\azure\loadBalancer-biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-Web.parameters.JSON<br />parameters\azure\virtualMachines-ADDS.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-with-OnPremise-and-Azure-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*kaks sündmuste*)

    Lisaks asutusesisese konfiguratsiooni seadmine ja cloud komponendid, nagu on kirjeldatud jaotises [lahenduse komponendid] [lahenduse komponendid].

7. Azure'i PowerShelli akna avamine skriptide kausta teisaldada ja käivitage järgmine käsk:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Asendage `<subscription id>` oma Azure tellimuse ID-ga.

    Jaoks `<location>`, määrake Azure piirkond, näiteks `eastus` või `westus`.

    Funktsiooni `<mode>` parameeter võib olla üks järgmistest väärtustest:

    - `Onpremise`, jäljendatud kohapealse keskkonna loomiseks.

    - `Infrastructure`, luua VNet taristu ja pilveteenuses väljale liikumine.

    - `CreateVpn`, luua Azure virtuaalse võrgu lüüsi ja ühendage see kohapealse võrgu.

    - `AzureADDS`, ehitada VMs abistas lisatakse serverid, juurutada Active Directory need VMs ja domeeni loomine pilveteenuses.

    - `AdfsVm`ADFS-i VMs loomiseks ja liituma domeeni pilveteenuses.

    - `ProxyVm`ADFS-i puhverserveri VMs koostamine ja liituma domeeni pilveteenuses.

    - `Prepare`, mis teeb kõik tööülesanded. **See on soovitatav suvand, kui on loomine on täiesti uue juurutamise ja teil pole kohapealse infrastruktuuri.**

    >[AZURE.NOTE] Skripti abil saate käivitada ka mõne `<mode>` parameeter `Workload` web, äri-, ja andmete taseme VMs ja võrgu loomiseks. See setup ei kaasata ka selle `Prepare` režiim.

    Kui kasutate funktsiooni `Prepare` suvand skripti võtab mitu tundi ja viimistluse sõnumiga *ettevalmistamine on lõpule viidud. Installige kõik ADFS-i ja puhverserveri VMs sert.*

8.  Lubama oma DNS-i sätete jõustumiseks taaskäivitage välja hüpata (*ra adfs-mgmt vm1* *ra adfs-turvalisus rg* jaotises).

9.  [SSL-serdi saamiseks ADFS-i] [ adfs_certificates] ja selle serdi installimine ADFS-i VMs. Pange tähele, et saate luua ühenduse ADFS-i VMs kaudu välja hüpata. IP-aadressid on *10.0.5.4* ja *10.0.5.5*. Vaikimisi kasutajanimi on parooliga *contoso\testuser* *AweSome@PW*.

    >[AZURE.NOTE] Kommentaaride Deploy-ReferenceArchitecture.ps1 skripti sel hetkel esitama üksikasjalikud juhised luua iseallkirjastatud serdi ja asutuse abil soovitud `makecert` käsk. Siiski kasutada loodud makecert tootmiskeskkonnas serdid.

10. Käivitage PowerShelli käsk konfigureerimine ADFS-i serveripargi:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. Liikuge dialoogiboksis hüpata *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* testimiseks ADFS-i installimine (võidakse kuvada serdi hoiatus, mille võite ignoreerida selle testi jaoks). Veenduge, et Contoso Corporation sisselogimislehel kuvatakse. Logige sisse *contoso\testuser* parooliga *AweS0me@PW*.

12. SSL-i serdi installimine ADFS-i puhverserveri VMs. IP-aadressid on *10.0.6.4* ja *10.0.6.5*.

13. Käivitage PowerShelli käsk esimese ADFS-i puhverserveri konfigureerimine.

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Järgige kuvatud skripti testimiseks installimise puhverserveri esimese ADFS-i.

15. Käivitage PowerShelli käsk teise esimese ADFS-i puhverserveri konfigureerimine.

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Järgige kuvatud skripti täieliku ADFS-i puhverserveri konfiguratsiooni testimiseks.

## <a name="next-steps"></a>Järgmised sammud

- [Azure Active Directory][aad].

- [Azure Active Directory B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Turvaline hübriid võrgu arhitektuur Active Directoryga"
