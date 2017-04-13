<properties
   pageTitle="Azure teenuse struktuuri avariitaastet | Microsoft Azure'i"
   description="Azure teenuse struktuuri pakub vaja käsitleda igat tüüpi katastroofid võimalusi. Selles artiklis kirjeldatakse tüüpi katastroofid, mis võivad tekkida ja kuidas need lahendada."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Azure teenuse struktuuri avariitaastet

Kriitiline osa pakkuda kõrge-saadavus pilve rakendus on tagada, et see võib jääda kõik erinevat tüüpi tõrkeid, sealhulgas need, mis on väljaspool teie kontrolli. Selles artiklis kirjeldatakse võimalike katastroofide kontekstis on Azure teenuse struktuuri kobar füüsilise paigutuse ja juhised tegelema sellise katastroofi piirata või riski tööseisakute või andmete kaotsimineku vältimiseks.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Teenuse struktuuri füüsilise paigutuse kogumite Azure

Erinevat tüüpi vigade riski mõista, on kasulik teada, kuidas kogumite füüsilise värviprinteriga Azure.

Kui loote Azure teenuse struktuuri kobar, teil palutakse valida, kui see on majutatud piirkonnas. Azure'i infrastruktuuri sätted seejärel ressursid selle kobar piirkonnas, eriti arv nõutav virtuaalmasinates (VMs). Vaatame lähemalt kuidas ja kus need on ette valmistatud.

### <a name="fault-domains"></a>Viga domeenid

Vaikimisi on klaster VM ühtlaselt kõigi loogiliste rühmadena tuntud viga domains (FDs) segmendi masinad võimalikke puudusi host riistvara põhjal. Eelkõige sellest, kui kaks VMs elavad kaks erinevat FDs, võib olla kindel, et neil pole sama power lähte- või võrgu aktiveerimine. Selle tulemusena kohalikus võrgus või elektrikatkestus mõjutamata ühe VM ei mõjuta muid, mis võimaldab teenuse struktuuri töökoormus ei reageeri masina klaster jooksul tasakaalustamiseks.

Saate visualiseerida klaster paigutust, viga domeenides [teenuse struktuuri](service-fabric-visualizing-your-cluster.md)Exploreris esitatud kobar kaardi abil:

![Levinud viga domeenides teenuse struktuuri Exploreris sõlmed][sfx-cluster-map]

>[AZURE.NOTE] Teine telg kobar kaart kuvatakse versioonitäienduse domeenid ja loogiline rühmitamine sõlmed põhjal kavandatud hooldustööd. Teenuse struktuuri kogumite Azure on alati ette viis versioonitäienduse domeenides.

### <a name="geographic-distribution"></a>Geograafilised jaotuse.

Praegu on [26 Azure piirkondade kogu maailmas][azure-regions], mitme rohkem teada. Üksikute piirkond võib sisaldada ühe või mitme füüsilise andmekeskuste sõltuvalt nõudmisel ja sobivad kohad, muu hulgas järgmised olemasolu. Pange tähele, et isegi regioonid, mis sisaldavad mitut füüsilise andmekeskuste, on mingit garantiid, et teie kobar VMs jagatakse ühtlaselt üle nende füüsilised asukohad. Tõepoolest praegu kõik VMs jaoks antud kobar on ette valmistatud ühe füüsilise saidil.

## <a name="dealing-with-failures"></a>Tõrked tegelemine

On mitut tüüpi tõrkeid, mis võivad mõjutada klaster, igal versioonil oma vähendamiseks. Käsitleme neid tekkida tõenäosuse järjestuses.

### <a name="individual-machine-failures"></a>Üksikute masina tõrkeid

Nagu mainitud, on üksikud masina tõrkeid, kas VM või seda viga domeenis majutada tarkvara või riistvara ohtu pole omaette. Teenuse struktuuri tavaliselt tuvastab selle sekunditega ja vastata vastavalt klaster oleku põhjal. Näiteks kui sõlme oli hosting esmane koopiad sektsiooni jaoks, uue esmase valitakse ka sektsiooni teisene koopiad. Kui Azure toob nurjunud seadme varundamine, see koosolekuga liituda klaster automaatselt ja uuesti tegemiseks oma osa töökoormus.

### <a name="multiple-concurrent-machine-failures"></a>Mitme samaaegseid masina tõrkeid

Kuigi viga domeenide märkimisväärselt vähendada samaaegseid masina tõrkeid, on alati võimalusi tuua mitu masinat klaster alla korraga mitme juhusliku ebaõnnestumist.

Üldiselt enamik sõlmed jäävad saadaval, klaster ka edaspidi kasutada, ehkki väiksemas mahus stateful koopiad hankida pakitakse väiksemat masinad ning vähem kodakondsuseta eksemplarid on saadaval levinud laadi.

#### <a name="quorum-loss"></a>Kvoorumi kaotsimineku

Kui enamik kujundusmuudatusi stateful teenuse sektsiooni minna, sektsiooni läheb olekusse tuntud "kvoorumi loss." Selles etapis lõpetab teenuse struktuuri lubamisel kirjutab sektsiooni tagada seisu järjepidevaid ja usaldusväärne. Tegelikult meil on valida aktsepteerimiseks perioodi saada, et tagada, et kliendid on ütles oma andmeid salvestati kui tegelikult ei olnud. Pange tähele, et kui teil on liitunud lubamisel loeb teisene koopiad stateful teenuse kaudu, saate jätkata need lugeda ajal selles olekus toimingute sooritamiseks. Sektsiooni jääb kvoorumi kaotsiminekut, kuni vastuste piisav arv naaske või kobar administraator sunnib [Parandamine – ServiceFabricPartition API]abil teisaldada[repair-partition-ps].

>[AZURE.WARNING] Andmete kaotsimineku tulemuseks on parandamise toimingu ajal esmane koopia alla.

Teenused saate ka saada kvoorumi kahju, on teatud teenuse osutamiseks mõjuga. Näiteks kvoorumi kaotsimineku nime teenus mõjutab nimelahendus, arvestades kvoorumi kaotsimineku Tõrkesiirde halduri teenus blokeerib uue teenuse loomine ja failovers. Pange tähele, et erinevalt oma teenuste katsel parandus süsteem *pole* soovitatav. Selle asemel, on parem lihtsalt oodata, kuni selle alla koopiad.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>Kvoorumi kaotsimineku ohtu minimeerimine

Saate minimeerida kvoorumi kaotsimineku ohtu target koopia määramine mahtu teie teenuse jaoks. On kasulik mõelda vastuste arv vajaliku arvu saate veebisaidil luba, kui ajal saadaolevatest kirjutab, võttes arvesse see rakendus pole saadaval sõlmed või kobar uuendamine saab teha sõlmed ajutiselt saadaval Lisaks riistvara tõrkeid.

Kaaluge eeldades, et olete konfigureerinud oma teenuste lisamine MinReplicaSetSize kolme, et vähim arv soovitatav tootmisteenused järgmised näited. Koos on kolme TargetReplicaSetSize (üks põhi- ja kaks sekundaaride) riistvara tõrke versiooniuuenduse (kaks koopiad alla) tulemuseks kvoorumi kaotsimineku ja teenust muutuvad kirjutuskaitstud. Teine võimalus, kui teil on viis koopiad, saate oleks pidama kaks ebaõnnestumist uuendamisel (kolme koopiad alla), nagu ülejäänud kaks koopiat saate endiselt vormi otsustusvõimeline määratud minimaalsed koopia jooksul.

### <a name="data-center-outages-or-destruction"></a>Andmete keskmist katkestuste või hävitamise

Harva füüsilise andmekeskuste võib muutuda ajutiselt saadaval power ega võrguühenduse kadu. Sellisel juhul teie teenuse struktuuri kogumite ja rakenduste samuti olla saadaval, kuid teie andmed jäävad alles. Kogumite Azure töötab, saate vaadata värskenduste kohta katkestuste [Azure oleku leht][azure-status-dashboard].

Väga juhul, et kogu füüsilise andmekeskuse hävitatakse, mis tahes teenuse struktuuri kogumite majutatud seal lähevad kaotsi, nende koos.

Kaitsta seda võimalust, on äärmiselt oluline perioodiliselt geograafilise liigne Store [varundamine oleku](service-fabric-reliable-services-backup-restore.md) ja veenduge, et teil on kinnitatud võimalus taastada. Kui sageli varukoopia tegemist on sõltuvad teie taastamine punkti eesmärk (RPO). Isegi juhul, kui teil on täielikult rakendanud varundus ja taaste veel, rakendate on sündmuseohjuri on `OnDataLoss` sündmuse nii, et saate sisse logida nimega ilmnemisel järgmiselt:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Tarkvara tõrkeid ja andmekao muudest allikatest

Andmete kaotsimineku põhjus, koodi defekte teenused, inimeste tegevuse tõrgete ja turvalisuse rikkumiste on rohkem levinud kui levinud andmete keskmist ebaõnnestumist. Siiski igal juhul taastamise strateegia on sama: võtta korrapäraste varukoopiate plaanimine stateful kõigi teenuste ja kasutada oma võimalus seda taastada.

## <a name="next-steps"></a>Järgmised sammud

- Saate teada, kuidas mitmesuguste tõrkeid, kasutades [testability framework](service-fabric-testability-overview.md) jäljendamiseks
- Muud ressursid-katastroofiabi ja kõrge-saadavus lugeda. Microsoft on avaldatud suur hulk juhiseid järgmisi teemasid. Ajal osa need dokumendid osutavad teatud võtteid kasutada muude toodete, neis on palju saate rakendada ka teenuse struktuuri kontekstis üldist head tavad:
 - [Kättesaadavus kontroll-loend](../best-practices-availability-checklist.md)
 - [Läbimiseks katastroofi taastamine drill](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Katastroofiabi ja Azure rakenduste kõrge kättesaadavus][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
