<properties
   pageTitle="Teenuse struktuuri kobar ressursihaldur - rakenduse rühmad | Microsoft Azure'i"
   description="Rakenduse rühma funktsioonid rakenduses teenuse struktuuri kobar ressursihaldur ülevaade"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introduction-to-application-groups"></a>Rakenduse rühmade tutvustus
Teenuse struktuuri kobar ressursihaldur haldab tavaliselt kobar ressursid jagades laadi (mida tähistab mõõdikute kaudu) ühtlaselt kogu klaster. Teenuse struktuuri haldab ka sõlmed klaster ja klaster võimsus kogu mõiste võimsus. See töötab palju eri tüüpi töökoormus, kuid mustrite, mis muudavad raske kasutada erinevaid struktuuri teenuserakenduse eksemplaride vahel tuua lisanõuded. Mõned täiendavad nõuded on tavaliselt.

- Võimalus reserveerida võimsus rakenduse eksemplari teenuste teatud arvu sõlmed
- Sõlmed, mis käivitamine on lubatud teenuste rakenduses määratud arv piirata
- Määratlemise võimalusi rakenduse eksemplari ise arvu- või kokku ressursside tarbimine sees teenuste piiramiseks

Nende nõuete täitmiseks oleme välja töötanud tugi, mida me kutsume rakenduse rühmad.

## <a name="managing-application-capacity"></a>Rakenduse võimsus haldamine
Rakenduse võimsus saab taotluse, samuti kokku argumendil laadi, et rakenduste eksemplaride üksikud sõlmed ulatuvad sõlmed arv piirata. Seda saab kasutada ka reserveerida ressursse rakenduse klaster.

Rakenduse võimsus saate määrata uue rakenduste loomisel; See saab värskendada olemasolevaid rakendusi, mis on loodud ilma rakenduse võimsus.

### <a name="limiting-the-maximum-number-of-nodes"></a>Sõlmed suurima arvu piiramine
Lihtsaim kasutada rakenduse võimsus puhul on kui ka rakenduse eksemplari loomise peab olema piiratud sõlmed teatud maksimaalne arv. Kui rakenduse võimsus pole määratud, teenuse struktuuri kobar ressursihaldur on väärtustada koopiad tavaline reeglitele (tasakaalustamiseks või defragmentimist), mis tähendab tavaliselt, et oma teenuste jagatakse üle kõik saadaolevad sõlmed klaster, või kui defragmentimist on sisse lülitatud mõne suvalise, kuid väiksema arvu sõlmed.

Järgmisel pildil on kujutatud rakenduse eksemplari võimalikud paigutuse sõlmed määratletud maksimaalne arv ilma ja seadke siis sama rakenduse sõlmed maksimaalne arv. Pange tähele, et tehtud kohta, millised koopiad või eksemplarid, mis teenused on saada asetada koos garantiid.

![Rakenduse eksemplari maksimumarv sõlme määratlemine][Image1]

Vasakul näites rakendus ei pea rakenduse võimsus seadmine ja see sisaldab kolme teenused. CRM on otsustanud loogilise laiali kõigi koopiate üle kuus saadaval sõlmed klaster parim saldo saavutamiseks. Õige näites näeme, mis piirab kolme sõlmed ja kus teenuse struktuuri CRM on parim saldo koopiate rakenduse teenuste saavutada sama rakenduse.

Parameeter, mis määrab seda käitumist nimetatakse MaximumNodes. See parameeter saate määrata rakenduse loomisel või värskendatud rakenduse eksemplari, mis on juba töötab, mille puhul on teenuse struktuuri CRM piirata rakendusteenuste määratletud maksimumarv sõlme koopiad.

PowerShelli

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Rakenduse mõõdikute, laadi ja võimsus
Rakenduse rühmad ka abil saate määratleda mõõdikute seostatud rakenduse eksemplari, kui ka rakenduse nende mõõdikute osas võimsus. Nii näiteks võib määratlemine mis, kui palju teenuseid soovitud saanud luua

Iga meetermõõdustik on 2 väärtused, mida saate määrata selle rakenduse eksemplari läbilaskevõime kirjeldamiseks.

-   Rakenduse kokku maht tähistab kindla mõõdiku taotlus koguvõimsus. Teenuse struktuuri CRM proovib piirata summa argumendil laadimise selle rakenduse teenuste määratud väärtuse; Lisaks, kui rakenduse teenused on juba tarbimine laadi kuni selle piirmäära, teenuse struktuuri kobar ressursihaldur keelamiseks loomine mis tahes uusi teenuseid või sektsioonid, mis põhjustaks kokku laadi minna üle selle piirmäära.
-   Suurim lubatud maht sõlm määrab kokku koormus koopiate rakenduste ja teenuste ühe sõlme. Kui sõlme koormust läheb üle selle mahu, proovib teenuse struktuuri CRM koopiad teisaldamiseks sõlmi, et võimsus piirang on täidetud.

## <a name="reserving-capacity"></a>Broneerimist võimsus
Teine levinud kasutada rakenduse rühmad on tagada ressursside klaster reserveeritud rakenduse eksemplari, isegi siis, kui rakenduse pole veel seda teenuste või isegi siis, kui nad pole veel ressursside tarbimine. Vaatame vaadata, kuidas see töötab.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Sõlmed ja ressursside broneerimiseks väikseima arvu määramine
Ressursside broneerimist rakenduse eksemplari nõuab paari täiendavaid parameetreid täpsustades: *MinimumNodes* ja *NodeReservationCapacity*

- MinimumNodes - nagu täpsustades target maksimumarv sõlme, mis teenused rakenduses töötab, saate ka määrata sõlmed, mis peaks töötama rakenduse väikseima arvu. See säte määratleb tõhus sõlmed, mis ressursside tuleks reserveerida vähemalt arvu tagada võimsuse kobar loomise rakenduse osana.
- NodeReservationCapacity – selle NodeReservationCapacity saab määratleda iga mõõdiku rakendusest. See määratleb argumendil rakenduse mis tahes sõlme kui mis tahes kujundusmuudatusi või teenuste kogumahu eksemplare suunatakse reserveeritud laadi summa.

Vaatame lähemalt reserveerimise näiteks:

![Teenuserakenduse eksemplaride reserveeritud võimsus määratlemine][Image2]

Vasakul näites rakendusi ei saa rakenduse tegutsemine määratletud. Teenuse struktuuri kobar ressursihaldur saldo rakenduse lapse teenuste koopiad ja eksemplari koos muude teenustega (väljaspool rakendus), et tagada klaster.

Paremal näiteks Oletame, et rakendus on loodud MinimumNodes, mis on seatud 2, 3 ja mõõdiku määratletud reserveerimise 20, max võimsus 50 ja kokku rakenduse maht on 100, sõlme rakenduse MaximumNodes teenuse struktuuri endale kaks sõlme sinine rakenduse läbilaskevõime ja muude koopiatega klaster kasutamine läbilaskevõimet ei luba. Reserveeritud rakenduse võimsus käsitletakse tarbitud ja loendatud ülejäänud kobar võimsus suhtes.

Rakenduse loomisel broneerimine kobar ressursihaldur endale võrdne MinimumNodes * NodeReservationCapacity klaster, kuid seda ei saa reserveerida võimsus teatud sõlmed seni, kuni soovitud rakendusteenuste koopiad on loodud ja. See võimaldab paindlikkust, kuna sõlmed on valitud uus koopiate ainult siis, kui need luuakse. Võimsus on teatud sõlme reserveeritud, kui vähemalt üks koopia pannakse.

## <a name="obtaining-the-application-load-information"></a>Rakenduse Laadi teabe hankimine
Iga rakenduse, et rakendus võimsus määratlenud saate hankida liitväärtuse laadi koopiad oma teenuste esitatud teavet. Teenuse struktuuri pakub PowerShelli ja hallatava API päringute selleks.

Laadi saate näiteks tuua abil järgmine PowerShelli cmdlet-käsk:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Päringu väljund sisaldab rakenduse võimsus rakenduse, nt miinimum sõlmed ja maksimaalse sõlmed määratud põhiteavet. On ka teavet sõlmed, mis kasutab rakenduse arv. Seega on iga laadi meetermõõdustik olema kohta teabe:
- Argumendil nimi: Mõõdiku nimi.
-   Broneerimiseks maht: Kobar reserveeritud võimsusega klaster selle rakenduse jaoks.
-   Rakenduse laadimine: Kokku laadi see rakendus lapse koopiad.
-   Rakenduse maht: Suurim lubatud rakenduse koormuse väärtus.

## <a name="removing-application-capacity"></a>Rakenduse võimsus eemaldamine
Kui rakendus võimsus parameetrid on määratud rakenduse, neid saab eemaldada värskenduse rakenduse API-d või PowerShelli cmdlet-käskude abil. Näiteks:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

See käsk eemaldab kõik rakenduse võimsus parameetrid rakenduse ja teenuse struktuuri kobar ressursihaldur hakkavad kohtlemine selle rakenduse nagu muid klaster, mis on määratletud järgmiste parameetrite. Käsk on kohe ja kobar ressursihaldur kustutab kõik rakenduse võimsus parameetrid selle rakenduse; need uuesti määrata nõuaks värskenduse rakenduse API-de nimetatakse vastav parameetritega.

## <a name="restrictions-on-application-capacity"></a>Rakenduse võimsus piirangud
On mitu piiratud rakenduse võimsus parameetrite abil, mis tuleb täita. Korral valideerimise tõrkeid, kuvatakse loomine või värskendamine rakenduse tagasi lükatud viga.
Täisarv kõik parameetrid peavad olema negatiivne arvud.
Lisaks üksikute parameetrite piirangud on järgmised:

-   MinimumNodes ei tohi kunagi olla suurem kui MaximumNodes.
-   Kui võimalusi mõõdiku laadimine on määratletud, siis ta peab tehke järgmisi reegleid.
  - Sõlm broneerimiseks võimsus ei tohi olla suurem kui sõlm maksimaalne maht. Näiteks ei saa piirata sõlme 2 ühikut mõõdiku "CPU" läbilaskevõime ja proovige reserveerida 3 ühikud iga sõlme.
  - Kui MaximumNodes pole määratud, siis MaximumNodes ja sõlm maksimaalne maht ei tohi olla suurem kui rakenduse koguvõimsus. Näiteks kui seate laadi meetermõõdustik "CPU" 8 ja teie seatud maksimaalne sõlmed 10 sõlm maksimaalne maht, siis koguvõimsus rakendus peab olema suurem kui 80 selle laadi mõõdiku.

Piiranguid pole jõustatud nii ajal rakenduse loomine (kliendi poolel) ja rakenduse värskendus (serveripoolne). Loomise ajal järgmine on näide nõuete selge rikkumise alates MaximumNodes < MinimumNodes ja käsk nurjub kliendi enne kutse saatmist isegi teenuse struktuuri kobar:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Kehtetu värskendus näide on järgmine. Kui võtame taotluse ja maksimaalse sõlmed kasutusele mõne väärtusega, läheb värskendus:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Pärast seda, saate proovivad värskendada minimaalne sõlmed:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Klient ei saa piisavalt konteksti rakenduse kohta edastamiseks teenuse struktuuri kobar värskenduse lubamiseks. Siiski kobar, teenuse struktuuri kinnitada olemasoleva parameetrid koos uue parameetri ja värskendamist ei õnnestu, kuna väärtus vaenlane minimaalne sõlmed on suurem kui sõlmed suurima väärtuse. Sel juhul rakenduse võimsus parameetrid ei muudeta.

Selleks jaoks kobar ressursihaldur saama pakuvad parima paigutuse koopiate rakenduste ja teenuste paigutatakse need piirangud.

## <a name="how-not-to-use-application-capacity"></a>Kuidas kasutada rakenduse võimsus

-   Abil rakenduse võimsus piirata rakenduse konkreetne alamhulk sõlmed: Kuigi teenuse struktuuri tagab järgimise maksimaalne sõlmed iga rakendus, mis on määratud rakenduse võimsuse kasutajad ei saa otsustada, millised sõlmed seda luuakse kohta. Selle saavutamiseks abil paigutuse piiranguid teenuste jaoks.
-   Kasutage rakenduse võimsus tagamaks, et kaks sama rakenduse teenused on alati kõrvuti. Selle saavutamiseks, kasutades osaleja suhete ja osaleja võib olla piiratud ainult teenused, mida tuleks paigutada tegelikult koos.

## <a name="next-steps"></a>Järgmised sammud
- Teenuste konfigureerida saadaolevate suvandite kohta lisateabe saamiseks vaadake teemat teema kohta muude kobar ressursihaldur konfiguratsioone saadaval [Services konfigureerimise kohta lugege](service-fabric-cluster-resource-manager-configure-services.md)
- Kohta, kuidas kobar ressursihaldur haldab ja saldo laadi klaster teada saada, vaadake artikkel [tasakaalustamiseks laadimine](service-fabric-cluster-resource-manager-balancing.md)
- Alustage algusest ja [saada, et teenuse struktuuri kobar ressursihaldur tutvustus](service-fabric-cluster-resource-manager-introduction.md)
- Lisateabe saamiseks selle kohta, kuidas mõõdikute töötavad üldiselt, lugeda üles [Teenuse struktuuri laadi mõõdikud](service-fabric-cluster-resource-manager-metrics.md)
- Ressursihaldur kobar on palju võimalusi klaster kirjeldamiseks. Kui soovite teada saada lisateavet nende väljamöllimine see artikkel [kirjeldab teenuse struktuuri kobar](service-fabric-cluster-resource-manager-cluster-description.md)


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
