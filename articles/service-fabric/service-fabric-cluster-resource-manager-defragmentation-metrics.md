<properties
   pageTitle="Defragmentimist mõõdikute rakenduses Azure teenuse struktuuri | Microsoft Azure'i"
   description="Kasutades defragmentimist või pakkimine strateegia mõõdikute sisse teenuse struktuuri ülevaade"
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

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentimist mõõdikute ning koormus teenuse struktuuri
Teenuse struktuuri kobar ressursihaldur on peamiselt seotud levitamise laadi – veenduge, et kõiki sõlmi klaster võrdselt kasutatud seoses tasakaalustamiseks. See on tavaliselt osas ellujäänud tõrkeid, kuna see muudab veenduge, et välja antud töökoormus mõned suure hulga ei antud jätmine kindlam ja targem paigutus. Teenuse struktuuri kobar ressursihaldur ei toeta eri strateegia, mis on defragmentimine. Defragmentimist tavaliselt tähendab, et proovite levitamine kasutamise mõõdiku üle klaster, mitte me peaks tegelikult konsolideerida. See on õnn pööre tavaline strateegia – selle asemel, et optimeerida kobar, mis põhineb argumendil laadi mõõdiku keskmine standardhälve minimeerimine, alustame optimeerimine tõusu standardhälve. Kuid miks peaksite strateegia?

Ka siis, kui te olete ühtlaselt laadi sõlme klaster vahel siis olete söönud mõned ressursid, et sõlmed on pakkuda. Tavaliselt on see ei probleem, kuid mõnikord mõned töökoormus loomine teenuseid, mis on väga suure ja tarbimine sõlm enamik – Oletagem, et 75% 95% on sõlm ressursid lõpuks sihtotstarbeline ühe eksemplari või koopia. See ei probleem, kobar ressursihaldur tuvastab teenuse loomise ajal, mida läheb vaja selleks, et teha see suur töökoormus ruumi ja määrata kohta, kuidas teha see juhtub klaster ümber, kuid vahepeal ootama, et klaster on selle töökoormus.

Silmas, et uue töökoormus kavandamisel on tavaliselt vähemalt vähe latentsus tundliku loomuga, kui me ei tee midagi teisiti me ei saa mõnikord löök paremale nende SLAs, kui seal on palju teenuste ja state liikuda, eriti siis, kui töökoormus klaster on üldiselt suured (ja seega võtta rohkem klaster liikumine). Tõepoolest mõõdetuna loomine kellaajad simulatsioonid reaal kobar andmete põhjal oleme me kuvati mis kui teenuste jaoks piisavalt suur ja klaster oli üsna kasutatud, et nende suurte teenuste loomine oleks aeglustada ja, et saaksime võib parandada see defragmentimist mõõdikute poliitika kasutusele.

Nii, nagu faili loomise või access võib saada aeglustada kui arvuti kõvaketta oli killustunud ja võib kiirendada defragmentides ketas, et suurem sidusaid plokid on saadaval, saate konfigureerida defragmentimist mõõdikute olema aktiivselt proovida kirjavahetusi tihendada laadi teenuste vähem sõlmed sisse, et on (peaaegu) alati ruumi isegi suurte teenuste ressursihaldur kobar , mis võimaldab kiiresti luua. Enamik inimesi ei vaja, kuna teenuste tavaliselt peaks olema väike ja seega ei ole raske leida jaoks piisavalt ruumi, kuid kui teil on suur teenuste ja nende loodud kiiresti vaja (ja muud kompromissidega, nt suurema impactfulness tõrkeid ja mõned ressursid jäetakse kasutamata, kui nad oodata, et töökoormus nõus on), seejärel defragmentimist strateegia on teie jaoks.

Alloleval joonisel annab visuaali kaks erinevat kogumite, mis on defragmented ja üks, mis ei ole. Tasakaalustatud juhul kaaluge liikumise, mida oleks vaja koht ühe objekti suuruselt teenuse, kui uus loodud, võrrelduna defragmenteeritud kobar, kus see võib kohe paigutada sõlmed 4 ja 5.

![Tasakaalustatud ja Defragmented kogumite][Image1]

## <a name="defragmentation-pros-and-cons"></a>Defragmentimist spetsialistid ja miinuste
Millised on nende kontseptuaalne kompromissidega? Soovitame põhjalik oma töökoormus enne sisselülitamist defragmentimist mõõdikute mõõtmine. Siin on kiire tabeli asju mõelda:

| Defragmentimist spetsialistidele  | Defragmentimist miinuste |
|----------------------|----------------------|
|Võimaldab kiirem loomine suurte teenused | Kontsentraadid laadimine vähem sõlmed, suurendades väide peale
|Lubab langetada andmete liikumine loomise ajal    | Tõrked võivad mõjutada rohkem teenuseid ja põhjustada rohkem piimapütt
|Lubab rikkaliku kirjeldus nõuete ja taastamise ruumi | Keerukamaid üldine ressursside halduse konfigureerimine

Võite segada defragmenteeritud ja tavaline mõõdikute sama klaster ja ressursihaldur teha see parim tagada teie paigutus, mis koondab nii palju defragmentimist mõõdikute, kui saate samas, proovib ülejäänud hajutamine. Täpsed tulemused kuvatakse sõltub arvu tasakaalustamiseks mõõdikute võrreldes arvu defragmentimist mõõdikute ja nende kaalu, praegune laadimise jne.

## <a name="configuring-defragmentation-metrics"></a>Defragmentimist mõõdikute konfigureerimine
Defragmentimist mõõdikute konfigureerimine on globaalne otsust klaster ja üksikute mõõdikute saab valida defragmentimist:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Järgmised sammud
- Ressursihaldur kobar on palju võimalusi klaster kirjeldamiseks. Kui soovite teada saada lisateavet nende väljamöllimine see artikkel [kirjeldab teenuse struktuuri kobar](service-fabric-cluster-resource-manager-cluster-description.md)
- Mõõdikud on, kuidas struktuuri kobar Service ressursi sõim haldab tarbimine ja võimsus klaster. Saate teada, vaadake lisateavet nende ja kuidas konfigureerida neid [selles artiklis](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
