<properties
   pageTitle="Teenuse struktuuri kobar ressursihaldur – osaleja | Microsoft Azure'i"
   description="Ülevaade osaleja teenuse struktuuri teenuste konfigureerimine"
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

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigureerimine ja kasutamine teenuse osaleja teenuse struktuuri

Osaleja on juhtelement, mis on esitatud peamiselt aidata hõlbustada suurema monoliit taotluste cloud ja microservices maailma. See tähendab, et seda saab kasutada ka teatud juhtudel õigustatud optimeerimine teenuste jõudluse parandamiseks kuigi see võib olla pool mõju.

Oletame, et te toome suuremat rakenduse või mis lihtsalt ei olnud loodud microservices arvesse, et teenuse struktuuri. See üleminek on tegelikult levinud ja oleme sellisel juhul olnud mitu klientide (nii sise). Alustate tõstmiseks kuni kogu rakenduse keskkonda, kuidas see pakitud ning töötab. Siis alustada katkestamine selle sisse, et kõik rääkida üksteisest erinevad väiksemate teenused.

Seejärel on "Oih...". "Oih" tavaliselt kuuluvad ühte nendest kategooriatest:

1. Mõned komponent X monoliit rakenduse oli mõne dokumentideta sõltuvus komponent Y ja me lihtsalt välja need eraldi teenused. Kuna need on nüüd töötab klaster erinevaid sõlmi, on need katkenud.
2.  Nende asjade suhelda (Kohalik nimega üksust pipes | jagatud mälu | faile kõvakettal), kuid vajan saama kiiruse asju natuke sõltumatult kasutusele. Saate eemaldada suur sõltuvus hiljem.
3.  Kõik on korras, kuid selgub, et need kaks komponendid on tegelikult väga jutukas/jõudluse tundliku sisuga. Üleviimise neid eraldi teenuste üldist rakenduse jõudluse tanked või latentsus suurendada. Selle tulemusena üldist rakenduse on pole ootustele.

Sellisel juhul me ei soovi, et meie Refaktooringutoimingu töö ja ei soovi selle monolith naasta, kuid läheb vaja mõned tunnet asukoht. See kestab kuni me saate kujundada töötada loomulikult teenuste komponentide või seni, kuni me lahendada täitmise ootustele mõnel muul viisil, kui võimalik.

Mida teha? Ka siis proovida, kui lülitate osaleja.

## <a name="how-to-configure-affinity"></a>Osaleja konfigureerimine
Osaleja seadistamiseks määratleda osaleja seos kahe erinevad teenused. Mõelge osaleja nimega "osutada" ühe teenuse teise ja räägivad "see teenus saab käivitada ainult kus töötab teenus." Mõnikord nimetatakse osaleja ema-tütre seose, (kui osutate lapse ema). Osaleja tagab sama sõlmed koopiad või teise eksemplari paigutatakse koopiad või ühe teenuseid.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Erinevate osaleja suvandid
On esitatud ühe skeeme korrelatsiooni kaudu, ja on kaks erinevat režiimi. Kõige levinum režiimi osaleja on, mida me kutsume NonAlignedAffinity. NonAlignedAffinity paigutatakse koopiad või erinevad teenused eksemplarid sama sõlmed. Muud režiim on AlignedAffinity. Joondatud osaleja on kasulik stateful teenustega. Kahe stateful teenused on koondatud osaleja konfigureerimise tagab, et nende teenuste primaries paigutatakse üksteise sõlmed. Samuti põhjustab iga paari sekundaaride nende teenuste sama sõlmed paigutada. See on võimalik (kuigi vähem levinud) konfigureerimiseks NonAlignedAffinity stateful teenuste jaoks. NonAlignedAffinity jaoks oleks paikneb kaks stateful teenuste erinevate koopiad, samas sõlmed, kuid pole katse oleks joondamiseks nende primaries või sekundaaride.

![Osaleja ja nende mõju][Image1]

### <a name="best-effort-desired-state"></a>Parima peegeldav soovitud olek
On paar osaleja ja monoliit arhitektuurides erinevusi. Paljud neist on, kuna seose osaleja on parim võimalik. Teenuste osaleja seos on erinevas üksused, mis võib nurjuda ja sõltumatult teisaldada. Miks võivad murda osaleja seose mõeldud on ka põhjused. Näiteks kui ainult teatud teenuse objektide osaleja seosega mahupiirangud mahub antud sõlm. Sellisel juhul isegi siis, kui seose osaleja on olemas, seda ei saa jõustada muude piirangute tõttu. Kui kõik muud piiranguid ja osaleja jõustamiseks hiljem osaleja piirangu rikkumise parandatakse automaatselt.  

### <a name="chains-vs-stars"></a>Ketid vs tähed
Täna me ei saa mudeli ketid osaleja seosed. See tähendab, et teenus, mis on ühe osaleja seosega ei saa teise osaleja seose emaüksus. Kui soovite seda tüüpi seose mudel, on teil mõjuv mudel ta täht, mitte ahelas. Selleks alumise lapse peaks olema parented "keskel" laps ema hoopis. Olenevalt teie teenuste, see võib olla nõutav loomine "kohatäite" teenuse olla mitu lastele ema.

![Ketid vs tärniga kontekstis osaleja seosed][Image2]

Teine asi, mida juba täna osaleja seoste kohta võtke arvesse, et nad on suunamata. See tähendab, et reeglit "osaleja" ainult jõustab laps on, kus on ema. Kui näiteks ema äkki ei üle teise sõlme siis kobar ressursihaldur ei tegelikult arvate, et pole midagi valesti, kuni see teated, et laps ei asu vanema; seose on kohe jõustatud.

### <a name="partitioning-support"></a>Eraldamine tugi
Lõplik asi teade osaleja on selle osaleja seosed ei toetata, kus on liigendatud ema. See on midagi, mis võivad lõpuks toetame, kuid täna pole lubatud.

## <a name="next-steps"></a>Järgmised sammud
- Teenuste konfigureerida saadaolevate suvandite kohta lisateabe saamiseks vaadake teemat teema kohta muude kobar ressursihaldur konfiguratsioone saadaval [Services konfigureerimise kohta lugege](service-fabric-cluster-resource-manager-configure-services.md)
- Mitmel põhjusel, kus inimesed kasutavad kuuluvuse, nt väike teenuste piiramiseks masinad seadmine ja proovite liitmine teenusekomplekt, laadi paremini toetatud rühmade rakenduse kaudu. Tutvuge [Rakenduse rühmad](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
