<properties
   pageTitle="Opsüsteemi Windows VMs mitme piirkondade jaoks kõrge-saadavus | Viide arhitektuur | Microsoft Azure'i"
   description="Kuidas juurutada VMs mitme Azure'i kõrge-saadavus ja paindlikkust piirkondades."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-in-multiple-regions-for-high-availability"></a>Opsüsteemi Windows VMs mitme piirkondade jaoks kõrge-saadavus

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Mitme piirkonna jaoks kõrge-saadavus Linux VMs töötab?](guidance-compute-multiple-datacenters-linux.md)
- [Opsüsteemi Windows VMs mitme piirkondade jaoks kõrge-saadavus](guidance-compute-multiple-datacenters.md)

Selles artiklis on soovitatav tavade käivitamiseks mitme Azure'i regioonid, kättesaadavus ja robustne katastroofi taastamine taristu Windows virtuaalmasinates (VMs).

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource groups] ja klassikaline. Selles artiklis kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

Mitme piirkonna arhitektuur võib anda suurema kättesaadavuse ühele piirkonnale juurutamine. Kui piirkondliku katkestuste mõjutab esmane piirkond, saate kasutada [Liikluse haldur] [ traffic-manager] kasutada teisene alale. See arhitektuur aitab ka siis, kui ka üksikute alasüsteemi rakenduse nurjub. 

On mitu üldist lähenemisviisi saavutamiseks kõrge kättesaadavus kogu andmekeskuste:

- Aktiivne/passiivne kuum puhkerežiimis olev. Liikluse läheb ühe piirkonna, samal ajal ootab puhkerežiimis. Teisene piirkonna VMs on eraldatud ja käivitatud igal ajal.

- Aktiivne/passiivne külma puhkerežiimis olev. Sama, aga VMs teisene piirkonnas on eraldatud kuni Tõrkesiirde vaja. Seda moodust maksab vähem käivitamiseks, kuid üldiselt on enam tõrke ajal alla.

- Aktiivne/aktiivne. Mõlemad piirkonnad on aktiivne ja taotlused on laadi nende vahel. Kui üks andmekeskuse pole saadaval, on selle välja pöörde võtta. 

See arhitektuur keskendub aktiivne/passiivne kuum puhkerežiimis olev, liikluse halduris Tõrkesiirde abil. Pange tähele, et teil võib juurutada väheste VMs jaoks saadaolevad puhkerežiimis olev mastaapimiseks klõpsake välja, vastavalt vajadusele.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm põhineb näidatud [lisamine usaldusväärsuse Azure N-astme arhitektuur](guidance-compute-n-tier-vm.md)arhitektuur.

> Visio dokumendi, mis sisaldab arhitektuur diagramm on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on "Arvuta - mitme piirkond (Windows) lehe.

[! [0]][0]

- **Põhi- ja regioonid**. See arhitektuur kasutab mõlema piirkonna kõrgemale saavutamiseks. See peamine piirkond. Esmane piirkond suunatakse ajal toiminguid, võrguliiklust. Kuid kui see pole saadaval, liiklus marsruuditakse teisene alale.

- ** [Azure liikluse haldur] [ traffic-manager] ** marsruudib sissetulevad taotlused esmane alale. Kui selle piirkonna pole saadaval, liikluse haldur ei üle teisene piirkond. Lisateavet leiate jaotisest [Konfigureerimise liikluse haldur](#configuring-traffic-manager).

- **Ressursi rühmad**. Luua eraldi [Ressursi rühmade] [ resource groups] piirkonna esmane, teisene piirkond, ja liikluse haldur. See kaudu saate hallata iga piirkonna ressursid ühe kogumina. Näiteks võib ümberkorraldamine ühe piirkonna, võtmata alla teise. [Ressursi rühmad link][resource-group-links], nii et saate käivitada päringu loendis kõik ressursid rakenduse.

- **VNets**. Luua eraldi VNet iga piirkonna jaoks. Veenduge, et aadress tühikuid ei kattuks. 

- **SQL Server alati kättesaadavus rühma**. Kui kasutate SQL Serverit, soovitame [SQL alati kohta Availabilty rühmad] [ sql-always-on] jaoks kõrge kättesaadavus. Saate luua ühe kättesaadavus rühma, mis sisaldab mõlema piirkonna SQL Serveri eksemplari. Lisateavet leiate jaotisest [konfigureerimine SQL serveri alati klõpsake kättesaadavus rühma](#configuring-the-sql-server-alwayson-availability-group ).

> [AZURE.NOTE] Samuti võtke arvesse [Azure'i SQL-andmebaasi][azure-sql-db], mis annab relatsiooniandmebaasist nimega mõnda pilveteenusesse. SQL-andmebaasi, te ei pea konfigureerimine kättesaadavus rühma või haldamiseks Tõrkesiirde.  

- **VPN lüüside**: [VPN-lüüsi] loomine[ vpn-gateway] iga VNet sisse ja konfigureerida [ühenduse VNet-VNet][vnet-to-vnet], et lubada võrguliiklust kahe VNets vahel. See on nõutav SQL alati kohta kättesaadavus rühma.

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme andnud mõni Azure ressursihaldur Mall viide arhitektuur soovituste järgneva installimiseks. Kui soovite luua oma viite arhitektuur nende soovituste juhul, kui teil on konkreetse nõude, mis ei sobi soovitus.

### <a name="regional-pairing"></a>Piirkondliku sidumine

Iga Azure'i piirkonna on ühendatud muu piirkonna sama geograafia. Üldiselt valida piirkondade sama piirkondliku paar (nt Ida-USA 2 ja meile keskse). Tehes eelised on järgmised.

- Lai katkestuste korral prioriteetne vähemalt ühe piirkonna välja iga paari taastamine.

- Kavandatud Azure süsteemi värskendusi saadetakse välja andmepunktipaaride piirkondadele järjest, võimalike tööseisakute minimeerimiseks.

- Paari riigis elavad sama geograafia, andmete tingimust täita.

Aga veenduge, et mõlemad piirkonnad toetavad kõik nõutavad rakenduse Azure teenused (vt [teenuste regiooniti][services-by-region]). Piirkondliku paari kohta leiate lisateavet teemast [äri järjepidevus ja Avariijärgne taaste (BCDR): Azure'i paaris piirkondade][regional-pairs].

### <a name="traffic-manager-configuration"></a>Liikluse haldur konfigureerimine

Võtke arvesse järgmist liikluse konfigureerimisel milline olukord teie haldur.

- **Marsruutimist.** Liikluse haldur toetab mitut [marsruutimise algoritmide][tm-routing]. Selles artiklis kirjeldatud stsenaariumi kasutada _prioriteet_ marsruutimist (varasema nimega _Tõrkesiirde_ marsruutimine). Selle sätte liikluse haldur saadab kõik taotlused esmane piirkond, kui esmane piirkond kättesaamatu. Sel hetkel seda automaatselt ei üle teisene piirkond. Vt [Tõrkesiirde konfigureerimine marsruutimise meetod][tm-configure-failover].

- **Tervise juures.** Liikluse haldur kasutab HTTP (või HTTPS) [juures] [ tm-monitoring] iga piirkonna olemasolu kontrollida. Funktsiooni juures kontrollib vastuse HTTP 200 jaoks määratud URL-tee. Hea tava, lõpp-punkti, mis rakenduse üldist tervist aruannete loomine ja kasutamine selle lõpp-punkti seisundi juures. Muul juhul võib selle juures aruande "terve" lõpp-punkti, kui rakenduse kriitiliste osade tegelikult ei suuda. Lisateabe saamiseks lugege teemat [Seisund lõpp-punkti jälgimise mustri][health-endpoint-monitoring-pattern].   

Kui liikluse haldur ei üle, pole teatud aja jooksul, kui kliendid ei jõua rakendus, mis võib olla mitu minutit. Kahe mõjutavad kogupikkus:

- Seisund juures peab avastada, et esmane andmekeskuse on muutunud kättesaamatu.

- DNS-serverid peate värskendama vahemällu talletatud DNS-i kirjeid IP-aadressi, mis sõltub DNS-i aeg live (TTL). Vaikimisi TTL on 300 sekundi (5 minutit), kuid saate konfigureerida selle väärtuse liikluse haldur profiili loomisel.

Lisateavet leiate teemast [Kohta liikluse haldur jälgimise][tm-monitoring]. 

Kui liikluse haldur ei ole üle, soovitame läbimiseks käsitsi failback, mitte ei automaatselt uuesti. Veenduge, et kõik rakenduse allsüsteemide on terve esmalt. Muul juhul saate luua olukorda, kus rakenduse ümberpööramisel liigub kaasa edasi ja tagasi andmekeskuste vahel.

Vaikimisi liikluse haldur automaatselt nurjub tagasi. Selle vältimiseks käsitsi madalam prioriteet Esmane piirkonna pärast Tõrkesiirde sündmus. Oletame näiteks, et esmane regioon on prioriteet 1 ja teisese on prioriteet 2. Pärast Tõrkesiirde, määramine prioriteet 3, vältimaks automaatse failback esmane piirkond. Kui olete valmis uuesti aktiveerimiseks, värskendage prioriteet 1.

Azure'i CLI järgmine käsk värskendab prioriteet:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Teine võimalus vältida flip-flop on lõpp-punkti ajutise keelamise kohta:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```

Sõltuvalt Tõrkesiirde põhjus, peate redploy piirkonnas ressursse. Enne ei ole uuesti teha on seadistada test. Test tuleks kontrollida, näiteks:

- VMs on õigesti konfigureeritud. (Kõik vajalik tarkvara on installitud, IIS-i on töötab jne).

- Rakenduse allsüsteemide on terve. 

- Funktsionaalne testimine. (Nt andmebaasi taseme on eesandmebaasis juurdepääsetavad web taseme.)

### <a name="sql-server-always-on-configuration"></a>SQL serveri alati klõpsake konfigureerimine

SQL serveri alati klõpsake kättesaadavus rühmade jaoks on vaja domeenikontrolleri. Kõik sõlmed kättesaadavus rühma peab olema sama AD domeeni. Järgmised annavad juhised selle kohta, kuidas konfigureerida SQL serveri alati klõpsake kättesaadavus rühma:

- Vähemalt kaks domeenikontrollerid paigutamiseks iga piirkonna. 

- Pange iga domeenikontrolleri staatiline IP-aadress.

- Luba suhtlust soovitud VNets VNet-VNet ühenduse loomine.

- Lisage iga VNet IP-aadresside hulka kuuluvad soovitud Domeen (mõlemad piirkonnad) kaudu loendisse DNS-i serveri. Saate kasutada CLI järgmine käsk. Lisateabe saamiseks lugege teemat [Halda DNS-i serverid kasutada virtuaalse võrgu (VNet)][vnet-dns].

```bat
azure network vnet set --resource-group dc01-rg --name dc01-vnet --dns-servers "10.0.0.4,10.0.0.6,172.16.0.4,172.16.0.6"
```

- [Windows Serveri tõrkesiirdeklastrite] loomine[ wsfc] kobar (WSFC), mis sisaldab mõlema piirkonna SQL Serveri eksemplari. 

- SQL serveri alati klõpsake kättesaadavus rühm, mis sisaldab SQL Serveri eksemplari esmaseid ja teiseseid piirkondades luua. Vaadake juhiseid [Ulatub alati klõpsake kättesaadavus rühma Remote Azure'i andmekeskusesse (PowerShelli)](https://blogs.msdn.microsoft.com/sqlcat/2014/09/22/extending-alwayson-availability-group-to-remote-azure-datacenter-powershell/) . 

- Pange esmane piirkonna peamine koopia.

- Pange esmane piirkonna ühe või mitme teisene koopiad. Need sünkroonse Kinnita kasutamine automaatselt Tõrkesiirde konfigureerimine.

- Pange teisene piirkonna ühe või mitme teisene koopiad. Neid kasutama *asünkroonse* kinnitamine, jõudluse konfigureerimine. (Vastasel korral kõik SQL-i tehingud ootama round reisi võrgus, et teisene piirkond.) 

> [AZURE.NOTE] Asünkroonne Kinnita koopiate automaatselt Tõrkesiirde ei toeta. 

Lisateavet leiate teemast [Opsüsteemi Windows VMs N-astme arhitektuur Azure](guidance-compute-n-tier-vm.md#SQL-AlwaysOn-Availability-Group).

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Keerukate N-astme rakendusega, võib pole vaja kogu rakenduse teisene piirkonna korrata. Selle asemel võite lihtsalt paljundada kriitilised alasüsteemi, mis on vaja toetada järjepidevuse.

Liikluse haldur on võimalik tõrge punkti süsteem. Kui teenus ei, kliendid ei pääse teie taotlus on tööseisakute ajal. Vaadake üle [Liikluse haldur SLA][tm-sla], ja määrata, kas liikluse halduriga eraldi vastab teie ettevõtte kõrge-saadavus. Kui ei, siis võiksite lisada mõne muu liikluse lahendus on failback nimega. Azure'i liikluse haldur teenuse nurjumisel muuta oma CNAME-kirjeid DNS-i muude liikluse halduse teenuse osutamiseks. (Selle juhise käsitsi täita ja rakenduse ei ole saadaval, kuni DNS-i muudatused paljundatakse.) 

SQL serveri klaster, on kaks Tõrkesiirde stsenaariumid, millega tuleks arvestada.

1. Kõik SQL-i kujundusmuudatusi esmane piirkonna nurjub. Näiteks see võib juhtuda piirkondliku katkestuste ajal. Sel juhul peate käsitsi jätate üle SQL-i, ehkki liikluse haldur nurjub automaatselt üle esiosa. Järgige [teha sunnitud käsitsi Tõrkesiirde SQL serveri kättesaadavus rühma](https://msdn.microsoft.com/library/ff877957.aspx), kus kirjeldatakse, kuidas jõustatud Tõrkesiirde SQL Server 2016 SQL Server Management Studio, Transact-SQL-i või PowerShelli abil teha. 

    > [AZURE.WARNING] Jõustatud Tõrkesiirde, kus on andmete kaotsimineku ohtu. Kui esmane regioon on võrguühenduse taastamisel, andmebaasi hetktõmmis ja kasutada [tablediff] leida erinevused.

2. Liikluse haldur ei üle teisene piirkond, kuid esmane SQL-i koopia on endiselt saadaval. Näiteks võib nurjuda ees taseme SQL-i VMs mõjutamata. Sel juhul Interneti-liikluse marsruuditakse teisene piirkond ja piirkonnas saate luua ühenduse SQL-i peamine koopia. Siiski on suurem latentsus, sest SQL ühendused piirkondade lõikes. Sellisel juhul tuleks teha käsitsi Tõrkesiirde järgmiselt: 

    - Ajutiselt aktiveerige SQL-i koopia teisene piirkonna *sünkroonse* kinnitamine. See tagab, et selle Tõrkesiirde ajal ei saa andmete kaotsimineku.

    - Ei õnnestu üle, et SQL-i koopia. 
    
    - Kui teil ei õnnestu tagasi esmane alale, taastamine asünkroonne Kinnita säte. 

## <a name="manageability-considerations"></a>Kaalutlused hallatavus

Juurutamise värskendamisel värskendada ühe piirkonna korraga vähendamiseks globaalne esinevate vale konfigureerimisel või rakenduse tõrge.

Testige paindlikkust süsteemi tõrkeid. Siin on mõned levinud tõrge stsenaariume testimiseks.

- Sulgege VM juhtudel.

- Rõhk ressursid nagu CPU ja mälu.

- Lahutage viivitus võrku.

- Krahhi protsessid.

- Aegumise serdid.

- Simuleerida riistvara vead.

- Sulgeda domeenikontrollerite DNS-i teenus.

Mõõtke taastamise korda ja veenduge, et need vastaksid teie ettevõtte vajadustele. Testige tõrgete laad, samuti kombinatsioonid.

## <a name="next-steps"></a>Järgmised sammud

See sari sisaldab värskendustest puhas cloud juurutuste. Ettevõtte stsenaariumid sageli vaja hübriid võrku, on kohapealse võrgu Azure virtuaalse võrguga ühenduse. Saate teada, kuidas koostada hübriid võrku, lugege teemat [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[azure-sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[sql-always-on]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc.png "Azure'i N taseme rakendused tugevalt võrgu arhitektuur"