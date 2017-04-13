<properties
   pageTitle="Paindlikkust kontroll | Microsoft Azure'i"
   description="Kontroll-loend, mis annab juhised paindlikkust probleemid ajal kujundus."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Azure'i paindlikkust juhised: paindlikkust kontroll-loend

Kujundamise paindlikkust rakenduse jaoks on vaja kavandamise ja mitmesuguseid tõrgete tüübid, mis võivad tekkida leevendada. Vaadake üle oma rakenduse kujundus teha paindlikumaks vastu järgmise kontroll-loendi üksuste.

## <a name="requirements"></a>Nõuded

- **Määratleda oma kliendi kättesaadavus nõuetele.** Oma kliendi on kättesaadavus nõuded komponendid oma rakenduse ning see mõjutab rakenduse kujundus. Oma kliendi lepingu saamiseks iga tekstilõigu rakenduse kättesaadavus sihtkohtade, muidu ei vasta teie kujundus kliendi ootustele. Lisateavet leiate jaotisest [määratlemine oma paindlikkust nõuetele](guidance-resiliency-overview.md#defining-your-resiliency-requirements) [Azure olles rakenduste kujundamine](guidance-resiliency-overview.md) dokumendi.

## <a name="failure-mode-analysis"></a>Tõrge režiimi analüüs

- **Saate teha rakenduse tõrge režiimi analüüsi (FMA).** FMA on koostamise paindlikkust rakendusse aegsasti kavandamisel. Mõne FMA eesmärke on järgmised.  

    - Määratlege, millist tüüpi tõrkeid rakendus võib tekkida. 
    - Jäädvustada võimalikud efektide ja mõju igat tüüpi rakenduse tõrge.
    - Leidke taastamise strateegiad. 

    Lisateavet leiate teemast [Azure olles rakenduste kujundamine: tõrke režiimis analüüsiteenuste][fma].  

## <a name="application"></a>Rakenduse

- **Mitmes eksemplaris teenuste juurutamine.** Teenuste paratamatult ei õnnestu ja kui teie rakendus sõltub teenuse ühekordsest see paratamatult ei õnnestu ka. [Azure'i rakenduse](../app-service/app-service-value-prop-what-is.md)teenuse ettevalmistamise mitmes eksemplaris, valige soovitud [Rakenduse teenuse leping](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) pakub mitmes eksemplaris. Azure'i pilveteenustega, konfigureerida iga kasutamiseks [mitmes eksemplaris](../cloud-services/cloud-services-choose-me.md#scaling-and-management)oma rollid. [Azure'i Virtuaalmasinates (VM)](../virtual-machines/virtual-machines-windows-about.md), tagada, et teie VM arhitektuur sisaldab rohkem kui üks VM ja iga VM on kaasatud mõne [kättesaadavus][availability-sets].   

- **Laadi koormusetasakaalustusteenuse abil levitamiseks taotluste.** Laadi koormusetasakaalustusteenuse jaotab rakenduse taotluste terve eksemplaride eemaldades vigane eksemplarid pööre. Kui teenust kasutab Azure'i rakendust Service või Azure pilveteenustega, on see juba koormus tasakaalustatud teie eest. Kui teie rakendus kasutab Azure VMs, peate laadi koormusetasakaalustusteenuse ettevalmistamine. Vt lisateavet [Azure laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-overview.md) ülevaade. 

- **Konfigureerida Azure rakenduse lüüside kasutamiseks mitmes eksemplaris.** Sõltuvalt rakenduse nõuetele, on [Azure rakenduse lüüsi](../application-gateway/application-gateway-introduction.md) võib olla paremini sobib levitamiseks taotluste oma rakendusteenuste. Siiski ühe eksemplari rakenduse lüüsi teenuse on pole tagatud SLA nii, et see on võimalik, et teie rakendus võib nurjuda, kui rakenduse lüüsi eksemplari ei. Rohkem kui ühe keskmise või suurema rakenduse lüüsi eksemplari selleks, et tagada teenuse [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)tingimustel kättesaadavus säte.

- **Kasuta kättesaadavus komplekti rakenduse iga taseme jaoks**. Eksemplaride paigutamine mõnda [kättesaadavus] [ availability-sets] tagab ühenduvuse vähemalt üks VM eksemplari [SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/)tingimustele. Kui teie VMs pole kättesaadavus kogum, mis pole teil tagab kaitse ja on võimalik, et need kõik võib nurjuda või korraga värskendada. 

- **Kaaluge rakenduse juurutamine mitme piirkondade lõikes.** Kui teie taotlus on juurutatud ühe piirkonna, sel juhul, saab kogu piirkonnas pole saadaval, rakenduse on ka saadaval. See võib olla aktsepteeritav rakenduse SLA tingimustel. Kui jah, võiksite üle mitme piirkonna oma rakenduste ja teenuste juurutamine. Mitme piirkonna juurutamise saate kasutada on aktiivne aktiivne mustri (levitamise taotluste üle aktiivne mitmes eksemplaris) või on aktiivne passiivne mustri (hoidmine "soe" eksemplari reservis, kui peamise eksemplari ei). Soovitatav on mitu eksemplari oma rakendusteenuste Juurutage piirkondliku paari. Lisateavet leiate teemast [Business järjepidevus ja Avariijärgne taaste (BCDR): Azure'i paaris piirkondade](../best-practices-availability-paired-regions.md).

- **Rakendada paindlikkust mustrite Kaug-analüüsitoiminguid vajaduse korral.** Kui teie rakendus sõltub suhtlemine remote teenused, side tee paratamatult nurjub. Kui loendis on mitu tõrkeid, võib teie rakendusteenuste ülejäänud terve eksemplarid taotlusi ülekoormatud. Mitme mustrite on kasulik tegelemiseks levinud tõrkeid, sh ajalõpp mustri, [proovige mustri][retry-pattern], [lüliti] [ circuit-breaker] mustri ja teised. Lisateavet leiate teemast [Azure olles rakenduste kujundamine](guidance-resiliency-overview.md#implementing-resiliency-strategies). 

- **Kasutage autoscaling tõusu laadi vastata.** Kui teie rakendus pole konfigureeritud automaatselt laadi suurendab välja mastaapimiseks, on võimalik, et teie rakendusteenuste nurjub, kui need muutuvad küllastatud kasutaja taotlused. Lisateavet leiate järgmistest:

    - Üldine: [skaleeritavus kontroll-loend](../best-practices-scalability-checklist.md) 
    - Azure'i rakenduse teenus: [mastaapimiseks arvu automaatselt või käsitsi][app-service-autoscale]
    - Pilveteenustega: [Kuidas automaatne skaala pilveteenus][cloud-service-autoscale]
    - Virtuaalmasinates: [määrab automaatse suuruse muutmise ja virtuaalse masina skaala][vmss-autoscale]


- **Rakendab asünkroonne alati, kui võimalik.** Toimingute sünkroonse saate monopoliseerida ressursid ja blokeerida muude toimingute ajal helistaja ootab protsessi lõpuleviimiseks. Kujundage oma rakenduse Luba asünkroonne toimingute võimaluse iga osa. Kuidas rakendada asünkroonne programmeerimine C# lisateavet, lugege teemat [asünkroonse asünkroonne programmeerimine ja oodata][asynchronous-c-sharp].

- **Kasutage rakenduse liikluse marsruutimiseks eri regioonide Azure'i liikluse haldur.**  [Azure'i liikluse haldur] [ traffic-manager] sooritab tasakaalustamiseks tasemel DNS-i laadimine ja saate marsruutida liikluse põhjal [liikluse marsruutimine] eri regioonide[ traffic-manager-routing] teie määratud meetodit ja seisundi rakenduse lõpp-punktid. 

- **Konfigureerimine ja testimine seisund sondid koormus soolise ja liikluse haldurid.** Veenduge, et teie seisund loogika kontrollib süsteemi osadesse ja vastaks seisund sondid õigesti.

    - [Azure'i liikluse haldur] sondid seisundi[ traffic-manager] ja [Azure laadi koormusetasakaalustusteenuse] [ load-balancer] Teeni konkreetne funktsioon. Jaoks liikluse haldur seisundi juures määrab, kas kasutada mõne muu alale. Laadi koormusetasakaalustusteenuse, jaoks seda määrab, kas VM eemaldamiseks pööre.      

    - Liikluse haldur juures, peaks teie seisund lõpp-punkti kontrollige kriitilised sõltuvusi mis on juurutatud samas piirkonnas ja kes ei peaks käivitama Tõrkesiirde teise alale.  

    - Laadi koormusetasakaalustusteenuse, tuleks seisund lõpp-punkti aruande seisundi VM. Ärge lisage muud astme või välisteenused. Muul juhul põhjustab tõrge, mis ilmneb siis väljaspool VM laadi koormusetasakaalustusteenuse VM eemaldamiseks pööre.

    - Seisundi jälgimine oma rakenduse rakendamise kohta vaadake teemast [Seisund lõpp-punkti jälgimise mustri](https://msdn.microsoft.com/library/dn589789.aspx).

- **Muu tootja teenused jälgimine.** Kui teie taotlus on muu tootja teenused sõltuvused, kindlaks teha, kus ja kuidas need muu tootja teenused võib nurjuda ja millist mõju ebaõnnestumiste on rakenduse kohta. Kolmanda osapoole teenusele ei tohi sisaldada diagnostika- ja seetõttu on oluline logige oma manamine neid ja need oleksid teie taotlus tervise ja diagnostika logimine abil kordumatut tunnust. Diagnostika-ja heade tavade kohta leiate lisateavet teemast [jälgimis- ja diagnostika juhised] [ monitoring-and-diagnostics-guidance] dokumendi.

- **Veenduge, et kõik kolmanda osapoole teenusele, saate kasutada pakub SLA.** Kui teie rakendus sõltub kolmanda osapoole teenusele, kuid muu tootja pakub mingit garantiid kättesaadavuse SLA kujul, rakenduse kättesaadavus ka ei saa tagada. Teie SLA on ainult nii hea vähemalt saadaval rakenduse osana.


## <a name="data-management"></a>Andmehaldus

- **Kopeerimise meetodid rakenduse andmeallikate mõistmiseks.** Rakenduse andmete salvestatakse erinevatest andmeallikatest ja on erinevad-saadavus nõuded. Hindab igat tüüpi andmesalve Azure kopeerimise meetodid, sh [Azure salvestusruumi kopeerimise](../storage/storage-redundancy.md) ja [SQL-i andmebaasi aktiivse Geo kopeerimine](../sql-database/sql-database-geo-replication-overview.md) tagamaks, et teie taotlus andmete nõuded on täidetud.

- **Tagada, et pole ühe kasutaja on nii tootmist kui ka varundamise andmetele juurdepääsu.** Varukoopiate andmed on kahjustada, kui üks ühe kasutajakonto õiguse kirjutada nii tootmise ja varukoopia allikad. Pahatahtlik kasutaja tahtlikult kõik andmed, kustutada, kui regulaarne kasutaja kogemata kustutage see. Kujundage oma rakenduse iga kasutaja konto õiguste piiramiseks nii, et ainult kirjutamine juurdepääsu nõudvate kasutajatel on juurdepääs ja see on ainult kas tootmise või varundamise, kuid mitte mõlemat.

- **Andmeallikat ei õnnestu üle ja nurjuda tagasi protsessi ning katsetage seda dokumenti.** Juhul, kui teie andmeallika ei Muulaste, inimeste operaator on dokumenteeritud juhiste nurjumise üle uue andmeallika jälgimiseks. Kui dokumenteeritud juhiseid on tõrkeid, ei saa tehtemärgi ei õnnestu ressursi üle ja järgige neid. Testige regulaarselt veenduge, et neid jälgima tehtemärgi on edukalt ei õnnestu üle ja nurjuda tagasi andmeallika käsustikuga juhiseid.

- **Kinnitage oma andmete varukoopiad.** Regulaarselt veenduge, et teie varundatud andmete on, mida oodata skripti käivitades andmetervikluse, skeemi ja päringute kinnitamiseks. Ei ole võttes varukoopia, kui see on kasulik taastada oma andmeallikad. Logige sisse ja aruande mis tahes selliste vastuolude tekkimist nii, et varukoopia teenuse saab parandada.

- **Kaaluge salvestusruumi konto tüüp, mis on geograafilise liigne.** Alati kopeeritud Azure Storage konto talletatud kohalik. Siiski on mitu dispersioonanalüüs strateegiad valida salvestusruumi konto on ette valmistatud. Valige [Azure lugemisõigus – Geo liigsete salvestusruumi (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) aitab kaitsta teie rakenduse andmeid harvaesinevate juhul, kui kogu piirkonna kättesaamatu.

    > [AZURE.NOTE] Vms, mitte toetuvad RA-GRS dispersioonanalüüs taastamine VM ketast (VHD failid). Selle asemel kasutada [Azure varukoopia][azure-backup].   

## <a name="operations"></a>Toimingud

- **Rakendada jälgimine ja head tavad oma rakenduse teavitamine.** Ilma õige jälgimine, diagnostika ja teavitamine, on võimalus oma rakenduse tõrked tuvastada ja Teavita tehtemärgi nende lahendamiseks. Heade tavade kohta leiate lisateavet teemast [jälgimis- ja diagnostika juhised] [ monitoring-and-diagnostics-guidance] dokumendi.

- **Mõõtke kõne kaugjuhtimine statistika ja teavet rakenduse meeskonna jaoks kättesaadavaks teha.**  Kui teile ei jälgida ja kõnede kaugjuhtimine statistika reaalajas aruande ja pakkuda lihtne viis selle teabe läbivaatamiseks, pole toimingute meeskond mõne hetke vaate rakenduse seisundi. Teenuste probleemid ja kui te ainult mõõt Keskmine kõne ajal saate neil pole piisavalt teavet, et kuvada. Kokkuvõtte kõne kaugjuhtimine mõõdikute nt latentsus, läbilaskevõime ja tõrgete lahendamine 99 ja 95 protsentiil. Mõõdikute avastada tõrkeid, mis ilmnevad iga protsentiili kohta statistilise analüüsi tegemine.

- **Saate jälitada siirdamiseks erandid ja korduste arv üle sobiva ajavahemiku jooksul.** Kui teile ei jälgida ja jälgida siirdamiseks erandid ja uuestiproovimise katsest aja jooksul, on võimalik probleemi või tõrge võib peidetud rakenduse proovi uuesti loogikat. See tähendab, kui teie jälgida ja logida ainult kuvatakse teavitab õnnestumisest või nurjumisest toimingu, peidetud asjaolu, et toimingu pidi proovitakse mitu korda tõttu erandid. Suureneva erandid aja jooksul trendijoon näitab, et teenus on on küsimus ja võib nurjuda. Lisateabe saamiseks lugege teemat [teenuse juhised uuesti][retry-service-guidance].

- **Varajase hoiatuse süsteemi, mis teatiste tehtemärgi rakendada.** Leidke tootluse näitajad rakenduse seisund, nt siirdamiseks erandid ja remote kõne latentsus ja määrake sobiv läviväärtuse liugureid neid. Teatise saata toimingute piirmäärast jõudmisel. Kehtestada nende tuvastavat probleemid enne need muutuvad kriitilised ja taastamise vastus.

- **Dokumendi väljaanne protsessi rakenduse.** Ilma väljaanne üksikasjaliku protsessi dokumentatsiooni kohta, võib tehtemärgi juurutamine halb värskendamine või valesti rakenduse sätete konfigureerimine. Selgelt määratlemine ja dokumendi teie väljaanne protsessi ning veenduge, et see on saadaval terve toimingute meeskond. Head tavad olles juurutamise rakenduse on üksikasjalikult kirjeldatud [olles juurutamise] [ guidance-resilient-deployment] paindlikkust juhised selle dokumendi jaotist.

- **Veenduge, et rohkem kui üks inimene meeskond õpetatakse jälgida rakendus ja mis tahes toimingute käsitsi taastada.** Kui teil on ainult üks tehtemärk meeskonna saate jälgida rakendus ja võrgukoosolekuga taastamise juhiseid, saab selle isiku ühtne tõrge. Mitme inimese tuvastamise ja taastamise kohta koolitada ja veenduge, et oleks alati vähemalt üks aktiivne igal ajal.

- **Rakenduse juurutamine protsessi automatiseerida.** Kui teie toimingute töötajad on vaja käsitsi juurutada rakendust, inimeste tõrke võib põhjustada juurutamise nurjumise. Rakenduse juurutamine automatiseerimiseks heade tavade kohta leiate lisateavet teemast [olles juurutamise] [ guidance-resilient-deployment] paindlikkust juhised selle dokumendi jaotist.

- **Kujundage oma väljaanne protsess, et suurendada rakenduste saadavus.** Kui teie väljaanne protsessi minna ühenduseta režiimi juurutamisel teenuste jaoks on vaja rakenduse on saadaval seni, kuni ta tagasi veebis. [Sinise/rohelise](http://martinfowler.com/bliki/BlueGreenDeployment.html) või [Kanaari vabastamine](http://martinfowler.com/bliki/CanaryRelease.html) juurutamise tehnika abil saate juurutada rakendust tootmisele. Nii nende meetodite kaasata, juurutamine oma Väljalaske kood kood kõrval nii, et kasutajad Väljalaske-koodi saate suunata kood tõrke korral. Lisateavet leiate teemast [olles juurutamise] [ guidance-resilient-deployment] paindlikkust juhised selle dokumendi jaotist.

- **Logige sisse ja audit oma rakenduse kasutuselevõttu.** Kui kasutate etapiviisilise juurutamise võtteid, näiteks sinine/roheline või Kanaari väljalasete on rohkem kui üks versioon töötab valmistamisel rakenduse. Probleemi korral on oluline, milline rakenduse versioon põhjustab probleem. Rakendada robustne logimine strateegia jäädvustada nii palju teavet konkreetse versiooni. 

- **Veenduge, et teie rakendus ei tööta [Azure tellimuse piirab](../azure-subscription-service-limits.md)vastu.** Azure'i tellimused on piirid teatava ressursi, nt ressursside rühmade arv, arv valdkond ja salvestusruumi kontode arv.  Kui teie rakenduse nõuded Azure tellimuse piirangud, luua teise Azure tellimust ja sätte piisavalt ressursse olemas.

- **Veenduge, et teie rakendus ei tööta vastu [piirab teenuse kohta](../azure-subscription-service-limits.md).** Üksikute Azure teenused on tarbimine piirangud &mdash; näiteks piirangud salvestusruumi, läbilaskevõime, ühendused, taotlusi sekundis ja muude mõõdikute arv. Rakenduse nurjub, kui see proovib kasutada lisaks need piirangud ressursid. See põhjustab teenuse ahendamise ja võimalike tööseisakute mõjutatud kasutajate jaoks. 

    Kindla teenuse ja oma rakenduse nõuded, olenevalt saate sageli vältida need piirangud ülespoole (nt, valides mõne muu hinnakirjad taseme) või skaala läbi (lisades uued eksemplarid).  

- **Kujundage oma rakenduse salvestusruumi nõuded kuuluvad Azure storage skaleeritavus ja jõudluse sihtkohtade.** Azure'i salvestusruumi on mõeldud eelmääratletud skaleeritavus ja jõudluse eesmärkide piires funktsioon, seega kujundada rakenduse kasutada nende eesmärkide piires salvestusruumi. Kui ületate need eesmärgid rakenduse salvestusruumi pidurdamise kogemus. Selle probleemi lahendamiseks ette valmistada täiendava salvestusruumi kontod. Kui käivitate salvestusruumi konto limiit vastu, ettevalmistamise täiendavad Azure'i tellimused ja seejärel ettevalmistamise täiendava salvestusruumi kontod. Lisateabe saamiseks lugege teemat [Azure salvestusruumi skaleeritavus ja jõudluse sihtkohad](../storage/storage-scalability-targets.md).

- **Valige rakenduse VM õige suurusega.** Tegelik CPU, mälu, ketas, ja I/O oma vms valmistamisel ja veenduge, et valitud VM suurus piisava. Kui pole, rakenduse võib tekkida võimsus probleemid nagu VMs lähenemine oma piirangud. VM suurused on dokumendis [suurused Azure'i virtuaalmasinates](../virtual-machines/virtual-machines-windows-sizes.md) üksikasjalikult kirjeldatud.

- **Määrata, kas teie taotlus töökoormus on ühed või kõikuva aja jooksul.** Kui teie töökoormus muutub aja jooksul, kasutage Azure'i VM skaala määrab automaatselt skaala VM eksemplaride arv. Muul juhul peate käsitsi suurendamiseks või vähendamiseks VMs arv. Lisateavet leiate teemast [Virtuaalse masina skaala komplekti ülevaade](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

- **Valige õige teenuse kiht Azure'i SQL-andmebaasi.** Kui teie rakendus kasutab Azure'i SQL-andmebaasi, veenduge, et olete valinud vastav teenuse kiht. Olema rakendus teie andmete kasutamine taseme, mida ei saa rakenduse andmebaasi tehingu üksus (DTU) nõuded käsitlema valimisel. Õige teenusleping valimise kohta lisateabe saamiseks vt selle [SQL-andmebaasi suvandid ja jõudluse: mõista, mis on saadaval iga teenuse kiht](../sql-database/sql-database-service-tiers.md) dokumendi. 

- **On tagasipööramine leping juurutamiseks.** On võimalik, et teie rakenduse juurutamine võib nurjuda ja põhjustada rakenduse kättesaamatuks muutuda. Kujundage tagasipööramine protsessi viimase teadaolevad hea versioonile naasmiseks ja tööseisakute minimeerimiseks. Vt [olles juurutamise] [ guidance-resilient-deployment] Lisateavet paindlikkust juhised selle dokumendi jaotist. 

- **Saate luua protsessi Azure'i tugiteenuse suhtlemiseks.** Enne tugiteenuste vajadusele protsessi poole pöördumist [Azure tugiteenuste](https://azure.microsoft.com/support/plans/) pole määratud, pikendatakse tööseisakute nagu tugi protsess on seal esimest korda. Lisage ühendust toega ja suurenevad probleemid rakenduse paindlikkust algusest osana.

- **Veenduge, et teie rakendus ei kasuta rohkem kui maksimaalne arv tellimuse kohta salvestusruumi kontod.** Azure'i lubab kuni 200 salvestusruumi kontod tellimuse kohta. Kui teie rakendus nõuab rohkem salvestusruumi kontod, kui neid on praegu saadaval teie tellimus, on teil luua uus tellimus ja seal täiendav salvestusruum kontode loomine. Lisateabe saamiseks lugege teemat [Azure tellimuse ja teenuste piirangud, kvootide ja piiranguid](../azure-subscription-service-limits.md#storage-limits).

- **Veenduge, et rakenduse ei ületa skaleeritavus eesmärkide virtuaalse masina ketast.** Mõne Azure IaaS VM toetab manustamise arvu andmete ketast olenevalt mitmest tegurist VM suurust ja salvestusruumi konto tüüp. Kui teie taotlus ületab skaleeritavus eesmärkide virtuaalse masina ketast, ettevalmistamine täiendavat salvestusruumi kontod ja seal virtuaalse masina ketast loonud. Lisateabe saamiseks vt [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohtade] (… / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Test

- **Tehke Tõrkesiirde ja failback rakenduse testimine.** Kui te pole täielikult testitud Tõrkesiirde ja failback, ei saa olla teatud, et sõltuvad teenused oma rakenduse tulla tagasi sünkroonitud viisil katastroofiabi ajal. Veenduge, et teie taotlus sõltuv services Tõrkesiirde ja fail uuesti sisse õiges järjestuses.

- **Viga täiskoormusele rakenduse testimine teha.** Rakenduse võib nurjuda erinevatel põhjustel, nt serdi aegumise, lõppemise süsteemiressursside VM või salvestusruumi ebaõnnestumist. Testige rakendust keskkonnas võimalikult tekitada poolt simuleerida või käivitamise reaal ebaõnnestumist. Näiteks kustutamine serdid, kunstlikult süsteemi ressursse või Kustuta salvestusruumi allikas. Kontrollige oma rakenduse võimalus igat tüüpi vead eraldi ja koos taastada. Kontrollige, kas tõrked on paljundus või kuhjuvate oma süsteemis.

- **Käivitage kontrollib valmistamisel nii sünteetilisi ja real kasutaja andmete kasutamine.** Testi ja tootmise identsed harva, seetõttu on oluline kasutada sinine/roheline või Kanaari juurutamise ja testige rakendust valmistamisel. See võimaldab teil testimiseks rakenduse alusel reaal laadimine ja veenduge, et see ei tööta, täieliku ootuspäraselt.

## <a name="security"></a>Turvalisus

- **Rakendada Rakendusetaseme kaitse jaotatud eitamine ning (DDoS).** Azure'i teenused on kaitsta DDos eest kihis võrku. Siiski Azure'i ei saa kaitsta rakenduse kiht eest, kuna see on keeruline eristada true kasutaja päringutele pahatahtlik kasutaja taotlused. Klõpsake rakenduse kiht DDoS eest kaitsmise kohta lisateabe saamiseks jaotisest "Kaitsmine vastu DDoS" [Microsoft Azure'i võrgu turvalisus](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (PDF-faili alla laadida).

- **Rakenduse ressursid juurdepääsuks vähimate õiguste põhimõte rakendada.** Vaikimisi on rakenduse ressurssidele peaks olema piira kui võimalik. Kinnitamise alusel kõrgema taseme õiguste andmine. Vaikimisi liiga vähem range juurdepääsu oma rakenduse ressursside andmine võib põhjustada keegi tahtlikult või kogemata kustutamine ressursid. Azure'i pakub [Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-built-in-roles.md) kasutaja õiguste haldamiseks, kuid on oluline vähemalt õiguste õiguste muud ressursid, mis on oma õigused süsteemid, näiteks SQL serveri kinnitamiseks. 

## <a name="telemetry"></a>Telemeetria

- **Logige koguda andmeid, kui rakendus töötab tootmiskeskkonda.** Robustne telemeetria teabe hõivamine, samal ajal, kui rakendus töötab tootmiskeskkonda või pole piisavalt teavet, et diagnoosida probleeme põhjustada samal ajal, kui see on aktiivselt serveeritakse kasutajad. Lisateavet on saadaval logimine põhitõed on saadaval, [jälgimine ja diagnostika juhised] [ monitoring-and-diagnostics-guidance] dokumendi.

- **Rakendada logimine on asünkroonne mustri abil.** Kui logimine toimingute sünkroonse, võivad nad blokeerida rakenduse koodis. Tagada teie logimine toimingute rakendamine asünkroonsete toimingute nimega. 

- **Seostada logiandmed üle teenuse piiride.** Tüüpilised n-astme rakenduses, võib kasutaja taotluse läbida mitme teenuse piirid. Näiteks kasutaja taotluse tavaliselt pärineb web taseme ja on business taseme edasi ja lõpuks püsis andmete astme. Keerukamaid stsenaariumid, võib kasutaja taotluse jaotada paljude erinevate teenuste ja poed. Veenduge, et teie süsteemi logimine vastab loendis efektidele kõnede üle teenuse piiride nii saate jälgida taotluse kogu rakenduse.

##  <a name="azure-resources"></a>Azure'i ressursid 

- **Kasutage ressursside Azure'i ressursihaldur malle.** Ressursihaldur malle oleks lihtsam automatiseerimiseks juurutuste PowerShelli või Azure CLI, mis viib usaldusväärne juurutamise protsessi kaudu. Lisateavet leiate teemast [Azure ressursihaldur ülevaade][resource-manager].

- **Pange ressursid tähendusega nimed.** Tähendusega nime andmine ressursid on hõlpsam leida kindla ressursi ja oma rolli mõistmine. Lisateabe saamiseks lugege teemat [soovitatav Nimepaneku tavad Azure ressursid](guidance-naming-conventions.md) 

- **Kasutage Rollipõhine juurdepääsu reguleerimine (RBAC)**. Kasutage RBAC Azure ressursse, mis juurutamist juurdepääsu piiramiseks. RBAC võimaldab teil määrata liikmetele DevOps meeskonnatöö, juhuslikku kustutamist või muudatuste vältimiseks juurutatud ressursid autoriseerimine rollid. Lisateabe saamiseks lugege teemat [Alustamine Azure'i portaalis juurdepääsu juhtimine](../active-directory/role-based-access-control-what-is.md) 

- **Kasutage ressursi lukud kriitilised ressursid, nt VMs.** Ressursi lukud takistada tehtemärgi kogemata kustutamine ressursi. Lisateabe saamiseks lugege teemat [Lock ressursse Azure'i ressursihaldur](../resource-group-lock-resources.md) 

- **Piirkondliku paari.** Mõlema piirkonna juurutamisel valida piirkondade sama piirkondliku paar. Lai katkestuste, korral ühe piirkonna taastamise prioriteetne välja iga paari. Teatud teenuste, nt geograafilise liigne salvestusruumi pakuvad automaatse dispersioonanalüüs andmepunktipaaride alale. Lisateavet leiate teemast [Business järjepidevus ja Avariijärgne taaste (BCDR): Azure'i paaris piirkondade](../best-practices-availability-paired-regions.md) 

- **Korraldamine ressursside rühmad, funktsioon ja elutsükli**.  Ressursirühma peaks sisaldama sama elutsükli jagavad ressursid. See on hõlpsam hallata juurutuste, siis kustutage testi juurutuste ja vähendamise võimalus tootmise juurutamise kogemata kustutanud või muudetud kasutusõiguste määramine. Luua eraldi ressursi rühmade tekitamiseks, arengu, ja testida keskkonnas. Mitme piirkonna juurutuse, pannud ressursse iga piirkonna jaoks eraldi ressursi rühmad. See on hõlpsam ümberkorraldamine ühe piirkonna muu piirkonna mõjutamata. 

## <a name="azure-services"></a>Azure'i teenused 

Kontroll-loend järgmiste üksuste rakendada teatud teenuste Azure.

###  <a name="app-service"></a>Rakenduse teenus 

- **Kasutage Standard või Premium taseme.** Need astme toetavad lavastus slots ja automaatse varundamise funktsiooni. Lisateavet leiate [Azure'i rakendust Service lepingute põhjalik ülevaade](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Vältige skaleerimist üles või alla.** Selle asemel valige taseme- ja eksemplari suurusega vastama teie tavalise laadi ja [skaala läbi](../app-service-web/web-sites-scale.md) jaotises eksemplarid käsitlema liikluse mahu muutused. Skaleerimist üles võib põhjustada soovitud rakendus uuesti.  

- **Kui rakenduse sätete poe konfigureerimine.** Saate rakenduse pikalt rakenduse sätete konfigureerimine sätteid. Ressursihaldur mallide või abil PowerShelli, nii et saate rakendada neid osana on automatiseeritud juurutamise / värskendada protsess, mis on usaldusväärne määratleda sätted. Lisateavet leiate teemast [konfigureerimine web apps kasutamine Azure'i rakendust Service](../app-service-web/web-sites-configure.md). 

- **Luua eraldi rakenduse teenuse tekitamiseks ja testi.** Ärge kasutage slots tootmise juurutuse testimiseks.  Kõik rakendused sees sama rakenduse teenusleping jagada sama VM juhtudel. Kui lisate tootmise ja testi juurutuste sama lepingu, võib negatiivselt mõjutada tootmise juurutamine. Näiteks võib laadi testide halvendada reaalajas tootmiskohast. Testi juurutuste eraldi kava paigutades saate eristada neid tootmise versiooni.  

- **Eraldi veebirakenduste veebist API-d**. Kui teie lahendus on ees web nii veebi-API, kaaluge lagunevatest neid eraldi teenuse rakenduse rakendustesse. See kujundus on hõlpsam lagunevad lahenduse, töökoormus. Saate kasutada web app ja API eraldi rakenduse teenuse lepingud, nii, et nad saavad mastaabitud sõltumatult. Kui te ei pea selle taseme skaleeritavus veebisaidil esmalt saate juurutada rakendused sisse sama lepingu, ja teisaldage need eraldi lepingute hiljem vajadusel.

- **Vältige rakenduse teenuste varundamiseks SQL Azure'i andmebaasi varundada.** Kasutage selle asemel [SQL-andmebaasi automaatse varundamise funktsiooni][sql-backup]. Rakenduse teenuse varundamise ekspordib andmebaasi SQL-i .bacpac faili, mis maksab DTUs.  

- **Juurutada vahekausta pesa.** Looge juurutamise pesa lavastus. Rakenduse värskenduste juurutamiseks vahekausta pesa ning veenduge, et enne selle vahetamine tootmisse juurutamise. See vähendab halb update valmistamisel. Samuti tagatakse, et kõik eksemplarid soojendada enne on vahetasin tootmisse. Paljud rakendused on märgatavat plokisoojendi ja külmkäivitus aeg. Lisateabe saamiseks lugege teemat [lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine](../app-service-web/web-sites-staged-publishing.md). 

-  **Looge juurutamine pesa hoidke viimase hästi (LKG) juurutamise.** Värskendust juurutamisel tootmise viimine LKG pesa eelmise tootmise juurutamine. See on hõlpsam halb juurutamise tagasi pöörata. Kui avastate probleem hiljem, saate kiiresti pöörduda LKG versiooni. Lisateavet leiate teemast [Azure viide arhitektuur: lihtsa veebirakenduse](guidance-web-apps-basic.md). 

- **Diagnostika logimise lubamine**, sh rakenduste logimine ja logimine veebiserver. Diagnostika-ja oluline logimine. Vt [diagnostika logimine veebirakenduste teenuses Azure rakenduse lubamine](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Log Bloobivahemälu salvestusruumi**. See on hõlpsam koguda ja andmete analüüsimiseks. 

- **Luua eraldi salvestusruumi konto logide.** Ärge kasutage sama konto salvestusruumi logid ja rakenduse andmeid. See aitab vältida logimine vähendada rakenduse jõudlus. 

- **Jõudluse jälgimist.** Kasutage jälgimise teenus, nt [Uue reliikvia](http://newrelic.com/) või [Rakenduse ülevaated](../application-insights/app-insights-overview.md) kuvari rakenduse jõudlus ja käitumine koormuse jõudlust.  Jõudluse jälgimine annab reaalajas ülevaate rakendus. See võimaldab probleemide ja tõrgete analüüsi põhjus. 


###  <a name="application-gateway"></a>Rakenduse lüüsi 

- **Ettevalmistamise vähemalt kaks korda.** Lüüsi rakenduse juurutamine koos vähemalt kaks korda. Ühekordsest on ühtne tõrge. Kasutage kahe või enama eksemplari koondamise ja skaleeritavus. Selleks, et saada [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/), peate ette kahe või enama keskmise või suurema eksemplari. 

### <a name="azure-search"></a>Azure'i otsing

- **Säte rohkem kui üks koopia.** Kasutage vähemalt kaks koopiad loetuks kõrge-saadavus või kolme jaoks lugemis-ja kirjutamisõigusega kõrge-saadavus.

- **Mitme piirkonnaga juurutuste indexers konfigureerimine.** Kui teil on mitme piirkonna juurutus, kaaluge järjepidevuse indekseerimine võimalused.

    - Kui andmeallika geo kopeeritud, osutage iga indekseerija iga piirkondliku Azure'i otsinguteenuse oma kohalikud andmed allikas koopia.  

    - Kui andmeallikas pole geo kopeeritud, osutage mitme indexers sama andmeallikas, Azure'i otsingu teenuste mitme piirkondades pidevalt ja sõltumatult indeks andmeallikast. Lisateabe saamiseks lugege teemat [Azure otsingu jõudlust ja optimeerimine kaalutluste][search-optimization].

###  <a name="azure-storage"></a>Azure'i salvestusruum 

- **Rakenduse andmete korral kasutada lugemisõigus – geograafilise liigne salvestusruumi (RA GRS).** RA-GRS salvestusruumi tiražeerib andmed teisel piirkonnas ja pakub kirjutuskaitstud juurdepääsu teisene piirkonnast. Kui esmane piirkonna salvestusruumi katkestuste, rakenduse saate lugeda andmed teisel piirkond. Lisateavet leiate teemast [Azure Storage dispersioonanalüüs](../storage/storage-redundancy.md).

- **Jaoks VM ketast, kasutage Premium salvestusruum** Lisateavet leiate teemast [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](../storage/storage-premium-storage.md).

- **Järjekorda Storage, luua varukoopia järjekorda teises regioonis.** Järjekorda Storage, kirjutuskaitstud koopia on piiratud kasutamist, kuna te ei saa järjekorda või dequeue üksuste. Selle asemel luua varukoopia järjekorda salvestusruumi konto teises regioonis. Salvestusruumi katkestuste korral saate rakenduse varukoopia järjekorda, kuni esmane piirkond muutub uuesti. Nii rakenduse endiselt protsessi uutele päringutele.  

###  <a name="documentdb"></a>DocumentDB 

- **Selle andmebaasi tiražeerimine piirkondade lõikes.** Mitme piirkonna kontoga DocumentDB andmebaas sisaldab täitmine ühe piirkonna- ja mitme loetuks regioonid. Kui piirkonnas kirjutamine on tõrge, võite lugeda teine koopia. Kliendi SDK tegeleb automaatselt. Võib nurjuda, kirjutage piirkonnas teise alale. Lisateavet leiate teemast [hajuta andmeid globaalselt DocumentDB](../documentdb/documentdb-distribute-data-globally.md).


###  <a name="sql-database"></a>SQL-andmebaas 

- **Kasutage Standard või Premium taseme.** Need astme pakuvad enam punkti õigel ajal taastamine (35 päeva) jooksul. Lisateabe saamiseks vt [SQL-andmebaasi suvandid ning jõudlust](../sql-database/sql-database-service-tiers.md).

- **Luba auditi SQL-andmebaasi.** Auditi saab diagnoosida ründetarkvara eest või inimeste tõrge. Lisateabe saamiseks lugege teemat [Alustamine SQL-andmebaasi audit](../sql-database/sql-database-auditing-get-started.md). 

- **Kasutage aktiivse Geo kopeerimine** Aktiivse Geo-kopeerimine abil saate luua loetav teisese teises regioonis.  Kui teie peamine andmebaasi ei õnnestu, või lihtsalt peab võrguühenduseta, teisene andmebaasi käsitsi Tõrkesiirde teha.  Enne, kui teil ei õnnestu üle teise andmebaasi jääb kirjutuskaitstud.  Lisateavet leiate teemast [SQL-i andmebaasi aktiivse Geo kopeerimine](../sql-database/sql-database-geo-replication-overview.md). 

- **Kasutage sharding**. Kaaluge sharding partition andmebaasi horisontaalselt. Sharding saate sisestada viga eraldamise. Lisateavet leiate teemast [mastaapimine välja koos Azure'i SQL-andmebaasi](../sql-database/sql-database-elastic-scale-introduction.md). 

- **Punkti õigel ajal taastamine abil saate taastada inimeste tõrge.**  Punkti õigel ajal taastamine tagastab andmebaasi varasemasse aega. Lisateabe saamiseks leiate [Azure'i SQL-andmebaasiga, kasutades automatiseeritud andmebaasi taastamine][sql-restore].

- **Kasutage geo-taastamine teenuse katkestuste taastada.** Geo-taastamine taastab andmebaasi geograafilise liigne varukoopia põhjal.  Lisateabe saamiseks leiate [Azure'i SQL-andmebaasiga, kasutades automatiseeritud andmebaasi taastamine][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL-i serveris (VM)

- **Selle andmebaasi tiražeerimine.** SQL serveri alati klõpsake kättesaadavus rühmade abil soovitud andmebaasi tiražeerimine. Pakub kõrge-saadavus, kui üks SQL serveri eksemplar nurjub. Lisateabe saamiseks lugege [Lisateavet...](guidance-compute-n-tier-vm.md) 

- **Andmebaasi varundamine**. Kui kasutate juba [Azure varukoopia](https://azure.microsoft.com/documentation/services/backup/) varundada oma VMs, kaaluge [Azure varukoopia SQL serveri töökoormus DPM abil](../backup/backup-azure-backup-sql.md). See lähenemine on üks varuadministraator rolli organisatsiooni ja ühendatud menetluse VMs ja SQL Server. Muul juhul kasutada [SQL serveri õnnestus varukoopia Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). 


###  <a name="traffic-manager"></a>Liikluse haldur 

- **Käsitsi failback teha.** Pärast liikluse haldur Tõrkesiirde, teha käsitsi failback, mitte ei automaatselt uuesti. Enne puudumisel tagasi, veenduge, et kõik rakenduse allsüsteemide on terve.  Muul juhul saate luua olukorda, kus rakenduse ümberpööramisel liigub kaasa edasi ja tagasi andmekeskuste vahel. Lisateabe saamiseks lugege teemat [Mitme piirkonna VMs töötab](guidance-compute-multiple-datacenters.md).

- **Loo seisundi juures lõpp-punkti**. Luua kohandatud lõpp-punkti, mis rakenduse üldist tervist aruanded. See võimaldab kasutada mis tahes kriitiline rada nurjumisel mitte ainult esiosa liikluse haldur. Lõpp-punkti peaks tagastama HTTP-tõrkekood, kui mõni kriitiline sõltuvus on vigane või kättesaamatu. Tõrgete mittekriitilise teenuste, pole siiski teatada. Muul juhul võib seisundi juures Tõrkesiirde käivitamine, kui see on vajalik, loomise vale-positiivsed. Lisateavet leiate teemast [liikluse haldur lõpp-punkti jälgimine ja Tõrkesiirde](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Virtuaalmasinates 

- **Ärge käitage tootmise töökoormus ühe VM.** Ühe VM juurutamise pole olles planeerimata või kavandatud hooldustööd. Selle asemel panna mitu VMs kättesaadavus komplekti või VM skaala komplekt, laadi koormusetasakaalustusteenuse ette. SLA saamiseks tuleb vähemalt kaks Virtuaalmasinates kasutusele sama kättesaadavus määratud jooksul. Lisateavet leiate teemast [Virtuaalse masina skaala komplekti ülevaade](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 

- **Määrake on saadaval, kui olete ette VM.** Praegu on kuidagi lisada ressursihaldur VM-Saadavus: määrake pärast VM on ette valmistatud. Kui lisate uue VM asemel mõne olemasoleva kättesaadav määramine, veenduge, et luua mõne NIC VM ja NIC lisamiseks klõpsake Laadi koormusetasakaalustusteenuse tagaandmebaas aadress pool. Muul juhul ei laadi koormusetasakaalustusteenuse marsruutimine võrguliiklust selle VM. 

- **Panna iga taseme rakenduse eraldi kättesaadavus seadmine.** N-astme rakenduses ei viige VMs erinevate samu kättesaadavus. VMs on kättesaadavus komplekti paigutatakse viga domeenides (FDs) ja värskendage domeenide (UD). Siiski FDs ja UDs koondamise kasu saada, iga VM kättesaadavus määramine peate saama samade klientide päringutele. 

- **Valige jõudlus vajaduste järgi VM õige suurusega.** Mõne olemasoleva töökoormus liikudes Azure alustada VM suurus, mis vastab teie asutusesisese serveriga. Seejärel mõõta oma tegelik töökoormus CPU, mälu ja kettaruumi IOPS ja vajadusel suurust muuta. See aitab tagada, et rakendus käitub cloud keskkonnas ootuspäraselt. Lisaks, kui teil on vaja mitu NICs, olema teadlik NIC limiit iga suurus. 

- **Kasutage VHDs premium mälu.** Azure Premium mälu toetab suure jõudlusega latentsusajaga ketas. Lisateavet leiate teemast [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](../storage/storage-premium-storage.md) VM suuruse, mis toetab premium mälu valimine. 

- **Luua eraldi salvestusruumi konto jaoks iga VM.** Koht VHDs jaoks eraldi salvestusruumi kontole ühe VM. See aitab vältida pihta IOPS limiidid salvestusruumi kontod. Lisateabe saamiseks lugege teemat [Azure salvestusruumi skaleeritavus ja jõudluse sihtkohad](../storage/storage-scalability-targets.md). Kui juurutate suure hulga VMs, olema teadlik tellimuse kohta limiit salvestusruumi kontod. Lugege teemat [salvestuslimiidi](../azure-subscription-service-limits.md#storage-limits).

- **Loo eraldi salvestusruumi konto jaoks diagnostikalogid**. Kirjutamine diagnostikalogid salvestusruumi samale kontole nimega VHDs, et vältida olukorda, kus Diagnostikaandmete logisse kandmise ei mõjuta IOPS VM ketast jaoks. Kohalik liigsete salvestusruumi (LRS) tavakasutajakonto piisab diagnostikalogid. 

- **Rakenduste installimine andmed kettale, mitte OS ketas.** Muul juhul jõuate ketta suuruspiirangut. 

- **Varundage VMs Azure varukoopia abil.** Varukoopiate kaitsta juhuslike andmete kaotsimineku. Lisateabe saamiseks vt [Kaitse Azure VMs taastamise teenuste hoidla](../backup/backup-azure-vms-first-look-arm.md).

- **Luba diagnostikalogid**, sh põhilisi mõõdikute, taristu logid ja [buutimine diagnostika][boot-diagnostics]. Buutimine diagnostika abil saate diagnoosimine buutimine ei saa teie VM sattumisel-käivitatava oleku. Lisateavet leiate teemast [Ülevaade Azure'i diagnostika logid][diagnostics-logs].

- **Kasutage AzureLogCollector laiendamine**. (Ainult Windows VM.) Sellele laiendile Azure'i platvormi logid liitmise ja lisatud need Azure salvestusruumi, tehtemärk VM kaugühenduse teel sisse logida ilma. Lisateavet leiate teemast [AzureLogCollector laiend](../virtual-machines/virtual-machines-windows-log-collector-extension.md).


###  <a name="virtual-network"></a>Virtuaalse võrgu 

- **Nimekiri või ploki avaliku IP-aadressi lisada mõne NSG alamvõrgu.** Pahatahtlike kasutajate juurdepääsu blokeerimiseks või Luba juurdepääs ainult kasutajad, kellel on õigus juurde pääseda rakenduse kaudu.  

- **Saate luua kohandatud seisundi juures.** Laadi koormusetasakaalustusteenuse seisund sondid testida HTTP- või TCP. Kui VM töötab HTTP-serverisse, HTTP juures on parem näitaja, mis seisund olek kui TCP juures. Mõne HTTP juures kasutada kohandatud lõpp-punkti, mis aruannete taotluse, sh kõik kriitilised sõltuvused üldist tervist. Lisateavet leiate teemast [Azure laadi koormusetasakaalustusteenuse ülevaade](../load-balancer/load-balancer-overview.md).

- **Ära Blokeeri seisundi juures.** Laadi koormusetasakaalustusteenuse seisundi juures saadetakse teadaolevad IP-aadress, 168.63.129.16. Ära Blokeeri liiklus või sealt see IP tulemüüri poliitika või võrgu turvalisuse rühma (NSG) reeglid. Laadi koormusetasakaalustusteenuse VM eemaldamiseks pööre põhjustaks blokeerivad seisundi juures. 

- **Laadi koormusetasakaalustusteenuse logimise lubamine.** Logides mitu VMs tagasi-otsas on vastuvõtmine tõttu nurjunud juures vastuste võrguliiklust. Lisateabe saamiseks leiate [Azure'i laadi koormusetasakaalustusteenuse Log analytics](../load-balancer/load-balancer-monitor-log.md).


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
