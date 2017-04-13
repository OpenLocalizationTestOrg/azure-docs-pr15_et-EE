<properties
    pageTitle="DocumentDB salvestusruumi ja jõudluse | Microsoft Azure'i" 
    description="Lisateavet andmete salvestamine ja säilitamine DocumentDB ja kuidas saab skaala DocumentDB rakenduse võimsus vajadustele." 
    keywords="dokumendi salvestamiseks"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Salvestusruum ja prognoositavad jõudluse ettevalmistamine on DocumentDB teave
Azure'i DocumentDB on täielikult hallatud, scalable dokumendi rakendusse NoSQL andmebaasi teenus JSON dokumentide jaoks. DocumentDB, pole teil virtuaalmasinates rentida, tarkvara juurutamine või andmebaaside jälgimine. DocumentDB, mida käitab ja pidevalt Microsofti insenerid esitamisel maailma klassi kättesaadavus, jõudlus ja kaitse.  

Saate alustada DocumentDB [andmebaasi konto loomise](documentdb-create-account.md) teel ja [DocumentDB andmebaasi](documentdb-create-database.md) [Azure portaali](https://portal.azure.com/)kaudu. Mõõtühikute välkdraiv (SSD) tagatud salvestusruumi ja läbilaskevõime pakutakse DocumentDB andmebaasid. Salvestusruumi need üksused on ette valmistatud, [luua andmebaasi kogu](documentdb-create-collection.md) kontol andmebaasi iga saidikogumi reserveeritud läbilaskevõime, mida saate mastaabitud üles või alla igal ajal rakenduse nõudmistele. 

Kui teie taotlus ületab oma reserveeritud läbilaskevõime ühe või mitme saidikogumid, taotlused on piiratud kohta saidikogumi alusel. See tähendab, et mõned rakenduse taotlused võib õnnestub, samal ajal, kui teised võivad rakendus.

Selles artiklis antakse ülevaade materjale ja mõõdikute haldamiseks võimsuse ja kavandamine andmesalv saadaval. 

## <a name="database-account"></a>Andmebaasi konto
Mis on Azure abonendi saate ettevalmistamise ühe või mitme DocumentDB andmebaasi konto haldamine oma andmebaasi ressursse. Iga tellimuse on seotud ühe Azure'i tellimus. 

[Azure portaali](documentdb-create-account.md)kaudu või kasutades [ARM malli või Azure CLI](documentdb-automation-resource-manager-cli.md)loomist DocumentDB kontod.

## <a name="databases"></a>Andmebaasid
Ühe DocumentDB andmebaas võib sisaldada praktiliselt piiramatus koguses dokumendi salvestamiseks saidikogumid rühmitada. Saidikogumite pakuvad jõudluse eraldamise - iga saidikogumi saab ette valmistatud läbilaskevõime, mis on jagatud koos muude saidikogumite sama andmebaasi või konto. DocumentDB andmebaas on elastne suurus, alates GBs TBs SSD tagatud dokumendi salvestamiseks ja ettevalmistatud läbilaskevõime. Erinevalt traditsioonilise RDBMS andmebaasi, andmebaasi DocumentDB on pole rakendatud ühe masina ja võib ühendada mitu masinad või kogumite.  

DocumentDB, kui vajate mastaapimiseks rakenduste, saate luua veel saidikogumid või andmebaaside või mõlemad. Andmebaasi saab luua [Azure portaali](documentdb-create-database.md) kaudu või mõni [DocumentDB SDK-d](documentdb-dotnet-samples.md).   

## <a name="database-collections"></a>Andmebaasi kogu
Iga DocumentDB andmebaas võib sisaldada ühe või mitme saidikogumid. Saidikogumite tegutseda äärmiselt olemasolevate andmete sektsioonid dokumendi salvestamise ja töötlemise. Iga saidikogumi saate salvestada dokumente heterogeensete skeemi. DocumentDB's automaatse indekseerimise ja päringu võimaluste abil saate hõlpsalt filtreerimise ja tuua dokumendid. Kogumi pakub dokumendi salvestus- ja päringu täitmise ulatust. Samuti on tehingu domeeni sisalduvate dokumentide kogumi. Saidikogumite eraldatakse läbilaskevõime määramine Azure portaali kaudu soovitud SDK-d või väärtuse. 

Saidikogumite on automaatselt liigendatud sisse ühe või mitme füüsilise serveri DocumentDB. Kui loote kogumi, saate määrata ettevalmistatud läbilaskevõime taotluse ühikute sekundis ja sektsiooni võtme atribuut. Selle atribuudi väärtust kasutatakse DocumentDB jagada dokumente sektsioonid ja marsruutimiseks taotlusi, nagu päringud. Sektsiooni võtmeväärtuse toimib ka salvestatud toimingute ja käivitab tehingu äärist. Iga saidikogumi on reserveeritud läbilaskevõime teatud selle saidikogumi, mida pole koos muude saidikogumite samale kontole. Seetõttu skaala välja rakenduse nii salvestusruumi ja läbilaskevõime. 

Saidikogumite loomist [Azure portaali](documentdb-create-collection.md) kaudu või mõni [DocumentDB SDK-d](documentdb-sdk-dotnet.md).   
 
## <a name="request-units-and-database-operations"></a>Taotleda üksused ja andmebaasi tehtavad toimingud

Kui loote kogumi, saate reserveerida läbilaskevõime sekundis [taotluse üksused (RU)](documentdb-request-units.md) . Selle asemel mõelda ja haldamise ressursid, võib ühe mõõdu ressursside **taotluse ühiku** võrrelda nõutav andmebaasi erinevaid toiminguid ja rakenduse päringu. Lugege 1 KB dokumendi tarbib sama 1 RU sõltumata kogumine või samaaegseid taotlusi, kus töötab samal arvu talletatud üksuste arvu. Kõik kutsed DocumentDB, sh keerukate toimingud nagu SQL-päringud on prognoositavad RU väärtus, mis määratakse arengu ajal suhtes. Kui teate, et dokumentide suurust ja iga toimingu (loeb, kirjutab ja päringute) rakenduse toetamiseks sageduse, ettevalmistamise läbilaskevõime rakenduse vajaduste täpse ja skaala üles ja alla andmebaasi jõudluse vajadused muutuvad. 

Iga saidikogumi saate reserveerida läbilaskevõime 100 taotluse ühikut sekundis sadu kuni taotluse üksuste sekundis miljoneid plokkidena. Ettevalmistatud läbilaskevõime saab reguleerida kogu kogumi muutmisega töötlemine vajadustega ja juurdepääs rakenduse mustrite elu. Lisateavet leiate teemast [DocumentDB jõudluse tasemed](documentdb-performance-levels.md). 

>[AZURE.IMPORTANT] Saidikogumite on tasustatav üksused. Maksumus määratakse ettevalmistatud läbilaskevõime taotluse üksused koos kogu tarbitud talletusmaht gigabaiti sekundis mõõdetud kogumi. 

Kui palju taotlemine üksused kuvatakse teatud toimingu, nt lisa, Kustuta, päringu või salvestatud protseduur täitmise peab olema? Taotluse üksus on koosolekukutse töötlemine maksumus normaliseeritud mõõt. Lugege 1 KB dokumendi on 1 RU, kuid taotluse lisamine, asendamine või kustutada samas dokumendis tarbib rohkem töötlemine teenusest ning seeläbi rohkem taotluse ühikut. Iga teenuse vastuse sisaldab kohandatud päise (`x-ms-request-charge`) mis aruannete tarbitud taotluse taotlus üksused. See päis pääseb ka [SDK-d](documentdb-sdk-dotnet.md). .NET SDK, on [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) atribuudi [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) objekti. Kui soovite oma läbilaskevõime vajadustele enne ühe helistamine prognoosimiseks, saate [võimsus Plaanur](documentdb-request-units.md#estimating-throughput-needs) see hinnang abi. 

>[AZURE.NOTE] 1 taotluse ühiku 1 KB dokumendi võrdlusalus vastab lihtsa saamiseks dokumendi [Seansi](documentdb-consistency-levels.md)järjepidevus. 

On mitmeid tegureid, mis mõjutavad taotluse ühikute tarbitud toimingu suhtes DocumentDB andmebaasi konto. Teguritest on järgmised.

- Dokumendi suurus. Kui dokumendi suuruses lugeda või kirjutada andmed suureneb ka tarbitud ühikute suurendamine
- Atribuut loendus. Eeldades vaikimisi indekseerimise kõik atribuudid, kirjutamiseks dokumendi tarbitud ühikute suureneb kui atribuudi arv suureneb.
- Andmete järjepidevuse. Andmete järjepidevuse tasemed tugev või piiratud Staleness kasutamisel kuvatakse täiendavate üksuste tarbitud dokumente lugeda.
- Indekseeritud atribuudid. Registri poliitika kohta iga saidikogumi määratleb, mis on vaikimisi indekseeritud. Saate vähendada teie taotlus ühiku tarbimine indekseeritud atribuutide arv piirata. 
- Dokumendi indekseerimine. Vaikimisi iga dokumendi automaatselt registrisse saate kasutada vähem taotluse üksuste kui te ei soovi indekseerida mõned dokumendid.

Lisateabe saamiseks vt [DocumentDB taotluse üksused](documentdb-request-units.md). 

Siin on näiteks tabeli, mis näitab mitu taotluse ühikut ettevalmistamine (1KB, 4KB ja 64KB) kolme eri dokumendi suuruses ja erinevatele tasanditel (500 loeb sekundis 100 kirjutab sekundis ja 500 loeb sekundis + 500 kirjutab sekundis). Andmete järjepidevuse seansi käigus konfigureeritud ja indekseerimise poliitika on seatud pole.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Dokumendimaht</strong></p></td>
            <td valign="top"><p><strong>Loeb sekundis</strong></p></td>
            <td valign="top"><p><strong>Kirjutab sekundis</strong></p></td>
            <td valign="top"><p><strong>Taotluse ühikud</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29000 RU/s</p></td>
        </tr>
    </tbody>
</table>

Päringute, Salvestatud toimingute ja päästikute kasutamine taotluse üksuste tehtud toimingute keerukusest. Kui teil tekib teie rakendus, kontrolli taotluse tasuta päise paremini mõista, kuidas iga toimingu tarbib taotluse seadme suurus.  


## <a name="choice-of-consistency-level-and-throughput"></a>Vormindusühtsuse taseme ja läbilaskevõime valik
Vaikimisi järjepidevuse tase valikut mõjutab läbilaskevõime ja latentsusaeg. Saate määrata vaikimisi järjepidevuse taset nii programmiliselt ja Azure portaali kaudu. Vormindusühtsuse taseme kohta soovi korral saate ka alistada. Vaikimisi on järjepidevuse taseme määramine **seansi**, mis pakub monotoonne lugemine/kirjutab ja lugeda oma kirjutamine garantiid. Seansi järjepidevuse sobib hästi kasutaja-kesksele rakenduste ja pakub on optimaalne saldo järjepidevuse ja jõudluse kompromisse.    

Azure'i portaalis oma järjepidevuse taseme muutmine juhised leiate teemast [DocumentDB konto haldamine](documentdb-manage-account.md#consistency). Või järjepidevus tasemed kohta leiate lisateavet teemast [kasutamine järjepidevus tasemed](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Dokumendi salvestusruumi ja register pea kohal ette valmistatud
DocumentDB toetab nii ühe-sektsiooni ja sektsioonitud Saidikogumite loomine. Iga sektsiooni DocumentDB toetab kuni 10 GB salvestusruumi SSD varundada. 10GB mahuga sisaldab dokumentide pluss registri salvestusruum. Vaikimisi DocumentDB saidikogumi on konfigureeritud automaatselt registrisse kõik dokumendid, mis tahes sekundaarse indeksite või skeemi konkreetselt nõudmata. Rakendustes, mis kasutavad DocumentDB põhjal, tüüpilised index pea kohal on 2-20% vahel. DocumentDB indekseerimise tehnoloogia tagab, et sõltumata atribuutide väärtused, ei ületa index pea kohal mahu dokumentide vaikimisi on rohkem kui 80%. 

Vaikimisi kõik dokumendid on indekseeritud DocumentDB automaatselt. Kui soovite registri pea kohal, saate teatud dokumendid on indekseeritud ajal sisestamine või asendamine dokumendi, nagu on kirjeldatud [DocumentDB indekseerimise poliitikate](documentdb-indexing-policies.md)eemaldamine. Saate konfigureerida DocumentDB saidikogumi indekseeritud kõigi dokumentide kogumis jätta. Samuti saate konfigureerida DocumentDB saidikogumi valikuliselt registrisse ainult teatud atribuudid või teed metamärkidega oma JSON dokumentide, nagu on kirjeldatud [kogumi poliitika indekseerimise konfigureerimine](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection). Välja arvatud atribuudid või dokumentide parandab ka kirjutamine läbilaskevõime – mis tähendab, et te tarbib vähem taotluse üksused.   

## <a name="next-steps"></a>Järgmised sammud

Rohkem õppematerjale DocumentDB tööpõhimõtete kohta, vaadake [Eraldatav ja mastaapimist Azure'i DocumentDB sisse](documentdb-partition-data.md).

Juhised jälgimine jõudluse tasemed Azure'i portaalis, vt [kuvari DocumentDB konto](documentdb-monitor-accounts.md). Tulemuslikkuse tasemeid saidikogumid valimise kohta leiate lisateavet teemast [jõudluse tasemete DocumentDB](documentdb-performance-levels.md).
 
