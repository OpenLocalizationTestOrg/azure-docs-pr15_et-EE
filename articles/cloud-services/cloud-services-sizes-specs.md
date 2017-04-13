<properties
 pageTitle="Pilveteenustega suurused | Microsoft Azure'i"
 description="Erinevate virtuaalse masina suurused (ja ID-d) on loetletud Azure pilveteenuse teenuse web ja töötaja rollid."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Pilveteenustega suurus

Selles teemas kirjeldatakse suurused ja suvandite pilveteenuses rolli eksemplari (web rollid ja töötaja rolli). See sisaldab ka juurutamise kaalutlused teadma, kui plaanite kasutada järgmisi ressursse. Iga maht on ID, mida te oma [teenuse määratluse faili](cloud-services-model-and-package.md#csdef).

Pilveteenustega on üks mitut tüüpi pakutud Azure Arvuta ressursid. Klõpsake [siin](cloud-services-choose-me.md) pilveteenustega kohta lisateavet.

> [AZURE.NOTE]Azure'i seotud piirangud leiate [Azure'i tellimus ja teenuste piirangud, kvootide ja piiranguid](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Veebi ja töötaja rolli aknad suurus

On mitu standard suurusega Azure valida. Kaalutluste kohta osa nende suurused on järgmised:

* D-sarja VMs on mõeldud rakendusi, mis nõuavad kõrgema Arvuta power ja ajutine jõudluse parandamine. D-sarja VMs välkdraiv (SSD) kiiremini protsessorite ja suurem mälumahuga-core suhe ette ajutine ketas. Lisateavet leiate teemast Azure blogis [Uus D-sarja virtuaalse masina suurused](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)väljakuulutamist.

* Dv2-rida, on algse D-sarja jätk omadusi võimsam Protsessor. Dv2-sarja CPU on umbes 35% kiiremini D-sarja CPU. See põhineb uusima genereerimine 2,4 GHz Inteli Xeon® E5-2673 v3 protsessor (Haswell), ja Inteli Turbo Boost Technology 2.0 saate üle minna kuni 3,1 GHz. Dv2-sarja on sama mälu ja kettaruumi konfiguratsioone sarja-D.

*   G-seeria VMs pakuvad enamik mälu ja käivitage hosts Inteli Xeon E5 V3 pere protsessorid.

*   A-sarja VMs saab kasutada mitmesuguseid riistvara tüübid ja protsessorite kohta. Suuruse on rakendus, põhineb riistvara, pakkuda ühtsete protsessori jõudlust käivitatud eksemplari, olenemata riistvara kohta juurutamist. Päringu määratlemiseks füüsilise riistvara, kus see suurus on juurutatud, virtuaalse riistvara Virtual seadmes.

*   A0 maht on üle tellitud füüsilise riistvara. Selles teatud suurus ainult, teiste klientide juurutuste võib mõjutada oma töötava töökoormus jõudlus. Suhteline jõudlus on kirjeldatud allpool nagu oodatud võrdlusalus, kehtivad ka ligikaudse hajuvuse kohta 15%.


Virtuaalse masina suurus mõjutab selle hinnad. Suuruse mõjutab ka virtuaalse masina töötluse, mälu ja salvestusruumi võimsus. Salvestusruumi maksumus arvutatakse eraldi lehtedel kasutatud salvestusruumi konto. Lisateavet leiate teemast [Virtuaalmasinates hinnad üksikasjad](https://azure.microsoft.com/pricing/details/virtual-machines/) ja [Azure salvestusruumi hinnad](https://azure.microsoft.com/pricing/details/storage/). 


Järgmisega võib aidata teil otsustada, mille suurus on:


* A8-A11 ja H-sarja suurused on ka *Arvuta mahukat eksemplarid*. Riistvara, mis töötab nende suurust on loodud ja optimeeritud Arvuta mahukat ja võrgu jaoks mahukat rakendused, sealhulgas suure jõudlusega arvutite (HPC) klaster rakendused, modelleerimine ja simulatsioonid. A8-A11 sarja kasutab Inteli Xeon E5-2670 @ 2,6 GHZ ja H-sarja kasutab Inteli Xeon E5-2667 v3 @ 3,2 GHz. Üksikasjalikku teavet ja peaksite arvesse võtma järgmisi suurused kasutamise kohta leiate teemast [H-sari ja Arvuta mahukat A-sarja VMs kohta](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2-seeria, D, G-sarja, on optimaalne rakendusi, mis nõuavad kiirem protsessorid, parem kohalik kettale jõudluse või on suurem mälumahuga nõudmistele.  Nad pakuvad rakendustele äriklassi võimas kombinatsioon.

*   Mõned füüsilise hosts Azure andmekeskuste ei toeta suuremad virtuaalse masina suurused, nt A5 – A11. Seetõttu võib kui teile kuvatakse tõrketeade **nurjunud konfigureerida virtuaalse masina {masina nimi}** või **luua virtuaalse masina {masina nimi} nurjunud** suurust muuta mõne olemasoleva virtuaalse masina uus suurus; luua uue virtuaalse masina virtual võrgus enne 16 aprillis 2013 või lisada uue virtuaalse masina mõne olemasoleva pilveteenuses. Vt [tõrge: "Ei saanud konfigureerida virtuaalse masina"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) tugi Foorum lahendusi iga juurutamise stsenaariumi puhul.  

* Teie tellimus võib ka piirata valdkond juurutate teatud suurus peredele. Kvoodi suurendamiseks tugiteenuste Azure.


## <a name="performance-considerations"></a>Jõudluse huvides

Oleme loonud mõistet, on Azure arvutada üksus (ACU) võimaldab võrrelda Arvuta (CPU) jõudluse Azure'i SKU-de jaoks üle anda. See aitab teil hõlpsasti kindlaks, millised SKU tõenäoliselt jõudluse oma vajadustele vastavaks.  ACU on praegu standardiseeritud kohta väike (Standard_A1) on 100 ja kõik muud SKU-de jaoks siis VM moodustavad palju kiiremini, et SKU käitamise standard võrdlusalus. 

>[AZURE.IMPORTANT] Funktsiooni ACU on ainult üldjoontes.  Oma töökoormus tulemused erineda. 

<br>

|Pere SKU-ga |ACU/Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8-A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 – 250 *|
|[G1 – 5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 - 300 *|

Tähistatud ACUs on * kasutada Inteli® Turbo tehnoloogia suurendada Protsessori sageduse ja jõudluse suurendamiseks.  Summa suurendada sõltuda VM suurus, töökoormus ja muude töökoormus sama Host töötab.

## <a name="size-tables"></a>Tabeli suurus

Järgmistes tabelites on suurused ja nad pakuvad võimalusi.

* Mälumaht on kuvatud GiB või 1024 ^ 3 baiti. Kui võrdlus ketast mõõdetud GB (1000 ^ 3 baiti) abil mõõdetud GiB ketast (1024 ^ 3) pidage meeles, et võimsus numbrite GiB võidakse kuvada väiksem. Näiteks 1023 bleedimine = 1098.4 GB

* Ketta läbilaskevõimet on mõõdetud sisend toimingute sekundis (IOPS) ja MBps kus MBps = 10 ^ 6 baiti sekundis.

* Andmete ketast, saate juhtida vahemällu talletatud või uncached režiimis. Vahemällu talletatud andmetega ketta tööks host vahemälu olek on seatud **kirjutuskaitstud** või **ReadWrite**.  Uncached andmete kettale tööks host vahemälu režiimi sätteks **pole**.

* Suurim lubatud võrgu läbilaskevõime on maksimaalne liidetud läbilaskevõimet eraldatud ja määratud VM tüübi kohta. Maksimaalse läbilaskevõime antakse juhiseid piisav võrgu läbilaskevõime tagada õige VM tüübi valimiseks on saadaval. Kui madal, Keskmine, kõrge ja väga kõrge, suurendab vastavalt selle läbilaskevõime. Võrgu jõudluse sõltub palju tegureid, sh võrgu ja rakenduse laadimise ja rakenduse võrgu sätteid.


## <a name="a-series"></a>A-sarja

| Suurus        | Protsessorituuma | Mälu: GiB | Kohaliku HDD: GiB | Max andmete ketast | Max andmete kettale läbilaskevõime: IOPS | Max NICs / võrgu läbilaskevõime |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / madal                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / mõõdukad              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4-x 500              | 1 / mõõdukad              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / kõrge                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16-x 500             | 4 / kõrge                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4-X 500              | 1 / mõõdukad              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / kõrge                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16-x 500             | 4 / kõrge                  |

## <a name="a-series---compute-intensive-instances"></a>A-sarja-Arvuta mahukat eksemplari

Ja peaksite arvesse võtma järgmisi suurused kasutamise kohta leiate teemast [H-sari ja Arvuta mahukat A-sarja VMs kohta](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Suurus         | Protsessorituuma | Mälu: GiB | Kohaliku HDD: GiB | Max andmete ketast | Max andmete kettale läbilaskevõime: IOPS | Max NICs / võrgu läbilaskevõime |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16-x 500             | 2 / kõrge                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16-x 500             | 4 / väga kõrge             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16-x 500             | 2 / kõrge                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16-x 500             | 4 / väga kõrge             |

* RDMA toega

## <a name="d-series"></a>Sarja-D


| Suurus         | Protsessorituuma | Mälu: GiB | Kohaliku SSD: GiB | Max andmete ketast | Max andmete kettale läbilaskevõime: IOPS | Max NICs / võrgu läbilaskevõime |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / mõõdukad              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4-x 500              | 2 / kõrge                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / kõrge                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16-x 500             | 8 / kõrge                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4-x 500              | 2 / kõrge                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / kõrge                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16-x 500             | 8 / kõrge                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32-x 500             | 8 / väga kõrge             |

## <a name="dv2-series"></a>Dv2-sarja

| Suurus            | Protsessorituuma | Mälu: GiB | Kohaliku SSD: GiB | Max andmete ketast | Max andmete kettale läbilaskevõime: IOPS | Max NICs / võrgu läbilaskevõime |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / mõõdukad              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4-x 500              | 2 / kõrge                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / kõrge                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16-x 500             | 8 / kõrge                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32-x 500             | 8 / väga kõrge        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4-x 500              | 2 / kõrge                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / kõrge                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16-x 500             | 8 / kõrge                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32-x 500             | 8 / väga kõrge        |
| Standard_D15_v2 | 20        | 140          | 1000                | 40             | 40-x 500             | 8 / väga kõrge        |

## <a name="g-series"></a>G-seeria

| Suurus        | Protsessorituuma | Mälu: GiB  | Kohaliku SSD: GiB  | Max andmete ketast | Max ketta läbilaskevõimet: IOPS | Max NICs / võrgu läbilaskevõime |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4-x 500            | 1 / kõrge                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / kõrge                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16-x 500           | 4 / väga kõrge             |
| Standard_G4 | 16        | 224          | 3,072                | 32             | 32-x 500           | 8 / väga kõrge        |
| Standard_G5 | 32        | 448          | 6 144                | 64             | 64-x 500           | 8 / väga kõrge        |


## <a name="h-series"></a>H seeria

H-sarja Azure'i virtuaalmasinates on järgmine genereerimine suure jõudlusega arvutuste VMs eesmärk on kõrge end arvutuslik vajadustele, nt molekulaarne modelleerimine ja arvutuslikke vedeliku dynamics. Need 8 ja 16 core VMs tugineb Inteli Haswell E5-2667 V3 protsessor tehnoloogia DDR4 mälu ja kohalik SSD vastavalt salvestusruumi. 

Lisaks olulisi CPU power, H-sari pakub erinevaid võimalusi madal latentsus RDMA loomiseks toetama mälu intensiivse arvutuslik nõuded FDR InfiniBand ja mitu mälu versiooni abil.


| Suurus           | Protsessorituuma | Mälu: GiB | Kohaliku SSD: GiB | Max andmete ketast | Max ketta läbilaskevõimet: IOPS | Max NICs / võrgu läbilaskevõime |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16-x 500                    | 8 / kõrge                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32-x 500                    | 8 / väga kõrge                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16-x 500                    | 8 / kõrge                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32-x 500                    | 8 / väga kõrge                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32-x 500                    | 8 / väga kõrge                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32-x 500                    | 8 / väga kõrge                  |


* RDMA toega

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Märkmed: Standard A0 - A4 CLI ja PowerShelli abil 

Mudelis klassikaline juurutamise mõned VM suurus nimed on veidi teistsugused CLI ja PowerShelli.

* Standard_A0 on ExtraSmall 
* Standard_A1 on väike
* Standard_A2 on keskmise
* Standard_A3 on suur
* Standard_A4 on ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Pilveteenustega suurused konfigureerimine

Saate määrata rolli eksemplari virtuaalse masina suurust teenuse mudelit, mis on kirjeldatud [teenuste definitsioonifail](cloud-services-model-and-package.md#csdef)osana. Roll suurus määrab CPU südamikud, mälumahu ja kohaliku faili süsteemi suurus, mis on eraldatud käivitatud eksemplari. Valige roll suurus põhineb rakenduse ressursside nõue.

Siin on näide olema [Standard_D2](#general-purpose-d) Web rolli eksemplari suuruse rolli määramine:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Järgmised sammud

- Teavet [Azure'i tellimus ja teenuste piirangud, kvootide ja piiranguid](../azure-subscription-service-limits.md).
- Lugege lisateavet [H-sari ja Arvuta mahukat A-sarja VMs kohta](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) töökoormus nagu suure jõudlusega arvutuste (HPC).

