<properties
   pageTitle="Kõrge-saadavus kontroll | Microsoft Azure'i"
   description="Kiire kontroll-loendi sätted ja toiminguid, mida saate teha, et teil on oma rakenduste kättesaadavus Azure parandada."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-checklist"></a>Kõrge-saadavus kontroll-loend
Üks hea Azure'i kasutamise eeliseid on võimalus suurendada rakenduste abil pilveteenuste kättesaadavus (ja skaleeritavus). Veendumaks, et saate need suvandid kõigi võimaluste ärakasutamine, allpool kontroll on mõeldud aitama teil mõned infrastruktuuri põhitõed rakenduste olles tagamiseks. 

>[AZURE.NOTE] Enamik alltoodud on asju, mida saate rakendada oma rakenduse igal ajal ja seega on suur "kiireid lahendusi". Parim pikaajaline lahendus hõlmab sageli rakenduse kujundus, mis on pilv ehitatud.  Jaoks kontroll-loendi (veel kujundus rakendusse valdkondades, lugege meie [kättesaadavus kontroll-loend](../best-practices-availability-checklist.md).

###<a name="are-you-using-traffic-manager-in-front-of-your-resources"></a>Kas kasutate oma ressursse ette liikluse haldur?
Interneti-liikluse abil liikluse haldur aitab marsruuditakse üle Azure piirkondade või Azure'i ja kohapealsete asukohad. Saate teha seda mitmel põhjusel, sh latentsus ja kättesaadavus. Kui soovite suurendada oma paindlikkust ja laiali saata teate oma liikluse mitme piirkonna liikluse halduri kasutamise kohta rohkem teavet, lugege [Mitme andmekeskuste jaoks kõrge-saadavus Azure VMs töötab](../guidance/guidance-compute-multiple-datacenters.md).

__Mis juhtub, kui te ei kasuta liikluse haldur?__ Kui te ei kasuta liikluse haldur rakenduse ees, mida on piiratud ühe piirkonna oma ressursid. See piirab oma skaala, suureneb latentsuse kasutajatele, mis pole teie piirkonnas lähedale ja langetatakse piirkond kogu teenuse häirete oma kaitse.

###<a name="have-you-avoided-using-a-single-virtual-machine-for-any-role"></a>Saate vältida ühe virtuaalse masina kasutamise mis tahes rolli?
Hea kujundus aitab vältida mis tahes ühe punkti tõrge. See on oluline kõigi teenuse kujundus (kohapealse või pilves), kuid on eriti kasulik pilves, nagu saate suurendada skaleeritavus ja paindlikkust küll skaleerimist välja (lisades virtuaalmasinates) asemel ülespoole (võimsam virtuaalse masina abil). Kui soovite rohkem teavet scalable rakenduse kujundus, lugege [Microsoft Azure loodud kõrge kättesaadavus](resiliency-high-availability-azure-applications.md).

__Mis juhtub, kui teil on ühe virtuaalse masina rolli?__ Ühe masina on tõrge ühtne ja [Azure virtuaalse masina teenuselepingu](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/)saadaval. Parima juhtudel rakendus töötab vaid peene, kuid see pole olles kujundus, ei kuulu Azure virtuaalse masina SLA ja mis tahes ühe punkti tõrge suureneb nurjub, kui midagi tööseisakute võimalus.

###<a name="are-you-using-a-load-balancer-in-front-of-your-applications-internet-facing-vms"></a>Kas kasutate laadi koormusetasakaalustusteenuse ette rakenduse Interneti-ühendusega VMs?
Koormus soolise abil saate laiali suvalise arvu masinad sissetulev liiklus rakenduse. Te saate lisa/Eemalda masinad oma laadi koormusetasakaalustusteenuse igal ajal, mis töötab ka Virtuaalmasinates (ja ka automaatne skaleerimist koos virtuaalse masina skaala komplekti) abil saate hõlpsalt toime liikluse või VM ebaõnnestumist. Kui soovite rohkem teada koormus soolise, lugege [Azure'i laadimine koormusetasakaalustusteenuse ülevaade](../load-balancer/load-balancer-overview.md) ja [töötab mitu VMs Azure skaleeritavus ja kättesaadavuseks](../guidance/guidance-compute-multi-vm.md).

__Mis juhtub, kui te ei kasuta laadi koormusetasakaalustusteenuse ette teie Interneti-ühendusega VMs?__ Laadi koormusetasakaalustusteenuse ilma te ei saa välja mastaapimiseks (lisa rohkem virtuaalmasinates) ja ainult skaalal (suurendamine web suunatud virtual arvuti). Näed ühes kohas koos selle virtuaalse masina esinevate. Samuti peate kirjutada DNS-i koodi pöörake tähelepanu sellele, kui teil on Interneti-ühendusega arvuti kaotanud ja uuesti kaart uue arvuti, saate alustada oma kohta oma DNS-i kirjet.

###<a name="are-you-using-availability-sets-for-your-stateless-application-and-web-servers"></a>Kas te kasutate kättesaadavus määrab kodakondsuseta ja veebi serverite jaoks?
Lihtsate oma masinad sama rakenduse astme mõne kättesaadavus komplekti muudab oma VMs Azure'i VM SLA õigus. Osa ka saadavus tagab, et teie masinad pannakse erinevad värskenduse domains (muu host seadmed, mis on paigatud korda) ja vea domains (st host masinad, mis levinud power allikas ja võrgu ühiskasutus liikumine). Ilma mõne kättesaadavus komplekti, asuda teie VMs host samasse arvutisse ja seetõttu võib olla ühtne tõrge, mis ei kuvata teile. Kui soovite oma VMs abil kättesaadavus komplekti kättesaadavuse kohta rohkem teavet, lugege [haldamine virtuaalmasinates olemasolu](../virtual-machines/virtual-machines-windows-manage-availability.md).

__Mis juhtub, kui te ei kasuta saadavus seada oma kodakondsuseta rakenduste ja web servers?__ Ei kasuta saadavus seadmine tähendab, et te ei saa Azure'i VM SLA ära. Ka see tähendab, et masinad selle rakenduse kiht võivad kõik katkestada kui värskendus host masina (VM, kasutate hostiva masina) või levinud riistvara tõrke.

###<a name="are-you-using-virtual-machine-scale-sets-vmss-for-your-stateless-application-or-web-servers"></a>Kas kasutate virtuaalse masina skaala komplektid (VMSS) kodakondsuseta rakenduse või web servers?
Hea scalable ja olles kujundus kasutab VMSS veenduge, et te saate kasvata/Kahanda masinad astme rakenduse arv (nt web esimese taseme). VMSS võimaldab teil määrata, kuidas oma rakenduse taseme skaala (lisamise või eemaldamise valite kriteeriumide alusel serverid). Kui soovite suurendada oma paindlikkust, et liikluse diagrammi Azure virtuaalse masina skaala komplekti kasutamise kohta rohkem teavet, lugege [Virtuaalse masina skaala komplekti ülevaade](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

__Mis juhtub, kui te ei meile on virtuaalse masina skaala määrata veebiserver minu kodakondsuseta rakendusega?__ Ilma on VMSS saate piirata võimalust mastaapimiseks ilma piiranguteta ja optimeerida ressursside kasutamisel. Kujundus, millel puudub VMSS on skaleerimise ülempiir, peab käsitlema täiendavad koodi (või käsitsi). On VMSS puudumine ka tähendab, et rakenduse saate hõlpsasti ei lisamine ja eemaldamine (sõltumata skaala) masinad aitavad toime suurte diagrammi liikluse (nt nagu ajal edendamine või kui teie saidi ja rakenduse/toodete läheb viiruse).

###<a name="are-you-using-premium-storage-and-separate-storage-accounts-for-each-of-your-virtual-machines"></a>Kas kasutate premium salvestusruumi ja eraldi salvestusruumi kontod iga teie virtuaalmasinates?
See on parim oma tootmise virtuaalmasinates premium mälu kasutamiseks. Lisaks veenduge, et eraldi salvestusruumi konto kasutamiseks iga virtuaalse masina (see on väikesemahuliste juurutuste tõene. Saate uuesti kasutada suuremat juurutuste salvestusruumi kontod, kuid mitme masinad on tasakaalustamiseks, mida tuleb teha on tasakaalus, värskendus domeenides ja üle astme rakenduse). Kui soovite Azure Storage jõudlus ja skaleeritavus kohta rohkem teavet, lugege [Microsoft Azure'i salvestusruumi jõudlus ja skaleeritavus kontroll-loend](../storage/storage-performance-checklist.md).

__Mis juhtub, kui te ei kasuta eraldi salvestusruumi kontod iga virtuaalse masina?__ Salvestusruumi konto, nagu paljud muud ressursid on ühtne tõrge. Kuigi on palju kaitstud ja Azure salvestusruumi paindlikkust funktsioone, on tõrge ühtne kunagi hea kujundus. Näiteks kui juurdepääsuõigused rikutud soovite, et konto, on tulemus talletusmahu piirmäära või mõne [IOPS piirata](../azure-subscription-service-limits.md#virtual-machine-disk-limits) jõutakse, mõjutab kõiki virtuaalmasinates salvestusruumi konto. Lisaks, kui seal on teenuse häirete, mis mõjutab salvestusruumi templi, mis sisaldab teatud salvestusruumi konto võib teil olla mitu virtuaalmasinates mõjutab.

###<a name="are-you-using-a-load-balancer-or-a-queue-between-each-tier-of-your-application"></a>Kas kasutate laadi koormusetasakaalustusteenuse või järjekorra iga taseme rakenduse?
Koormus soolise või järjekorrad vahel iga taseme rakenduse abil võimaldab teil hõlpsasti mastaapimiseks iga taseme rakenduse on lihtne ja sõltumatult. Jaotuse (st kui ulatuslikult levitate rakenduse) vajadustele valida nende tehnoloogiate alusel oma latentsus keerukuse, vahel. Üldiselt on tavaliselt järjekorrad on suurem latentsus ja keerukuse lisada, kuid kasu on paindlikumaks ja võimaldab teil oma rakenduse levitamine üle suuremat ala (nt piirkondade). Kui soovite rohkem teavet, kuidas kasutada ettevõttesisese koormus soolise või järjekordade, lugege [Ülevaade koormusetasakaalustusteenuse sisemise laadimine](../load-balancer/load-balancer-internal-overview.md) ja [Azure järjekorrad ja teenuse järjekorrad - võrreldes ja ja](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

__Mis juhtub, kui te ei kasuta laadi koormusetasakaalustusteenuse või järjekorra iga taseme rakenduse?__ Laadi koormusetasakaalustusteenuse või järjekorda, ilma iga taseme rakenduse vahel on raske mastaapimiseks rakenduse üles või alla ja selle laadimise mitme arvutites. Ei tee seda võib kaasa tuua üle või jaotises ettevalmistamise oma ressursse ja tööseisakute või kehva kasutaja võimalused, kui teil on ootamatud muudatused liikluse või süsteemi tõrkeid.
 
###<a name="are-your-sql-databases-using-active-geo-replication"></a>SQL-andmebaase kasutate aktiivse geo-kopeerimine? 
Aktiivse Geo-kopeerimine abil saate konfigureerida kuni 4 loetavad teisene andmebaaside sama või erinevad piirkondades. Teisene andmebaaside on teenuse häirete või esmane andmebaasiga ühenduse loomiseks ei suuda puhul saadaval. Kui soovite rohkem teada SQL-andmebaasi aktiivse geo kopeerimine, lugege [Ülevaade: SQL-i andmebaasi aktiivse Geo kopeerimine](../sql-database/sql-database-geo-replication-overview.md).
 
 __Mis juhtub, kui te ei kasuta aktiivse geo-kopeerimine koos SQL-andmebaase?__ Aktiivse geo-kopeerimine, kui teie peamine andmebaasi kunagi läheb ühenduseta ilma (kavandatud hooldustööde, teenuse häirete, riistvara tõrge jne) andmebaasi rakenduse saab ühenduseta seni, kuni saate tuua oma esmase andmebaasi taastamisel terve olekus. 
 
###<a name="are-you-using-a-cache-azure-redis-cache-in-front-of-your-databases"></a>Kas kasutate vahemälu (Azure'i Redis vahemälu) oma andmebaasi ette?
Kui rakenduse on kõrge andmebaasi laadimine, kus enamik andmebaasi kõned on loeb, saate rakenduse kiiruse suurendamiseks ja vähendamiseks laadi oma andmebaasi, rakendades vahemällu kiht ette andmebaasi, et need toimingud. Saate rakenduse kiiruse suurendamiseks ja vähendamiseks oma andmebaasi laadimine (suurendades skaala, siis võite hakkama), pannes vahemällu kiht ette andmebaasi. Kui soovite lisateavet Azure Redis vahemälu, lugege [juhiseid vahemälu](../best-practices-caching.md).
 
 __Mis juhtub, kui te ei kasuta vahemälu ette andmebaasi?__ Kui teie andmebaas arvutis on piisavalt võimas liikluse laadi saate sellele see siis rakenduse vastab nagu tavaliselt, kuigi see võib tähenda, et alumise koormuse saate tasub andmebaasi arvutisse, mis on kõrgemad kui vaja. Kui teie andmebaas arvutis pole võimas piisavalt käsitlema oma laadi, siis hakkate kogemus kehva kasutaja kogemus rakendusega (latentsus, ajalõpud ja võimalik, et teenus tööseisakute).
 
###<a name="have-you-contacted-microsoft-azure-support-if-you-are-expecting-a-high-scale-event"></a>On teiega ühendust Microsoft Azure'i tugi, kui te ootate kõrge skaala sündmuse?
Azure'i tugi aitab teil suurendada oma teenuste piirangud tegelema kavandatud veebiaadresside sündmuste (nt uue tooteesitlused või teisiti pühad). Azure'i tugi samuti on võimalik, et aitavad teil suhelda asjatundjate, kes aitavad teil läbi vaadata oma kujundus konto meeskonnaga ja aitavad teil leida parim lahendus teie vajadustele kõrge skaala sündmus. Kui soovite Azure tugiteenuste kohta rohkem teavet, lugege [Azure'i toetavad korduma kippuvad küsimused](https://azure.microsoft.com/support/faq/).

__Mis juhtub, kui te pole tugiteenuste Azure'i suure ulatusega sündmuse?__ Kui te ei suhelda või kavandamine, veebiaadresside sündmus, mida riskige pihta teatud [Azure'i teenuste piirangud](../azure-subscription-service-limits.md) ja seega luua kehva kasutuskogemuse (või halb, tööseisakute) oma üritusel. Arhitektuuri kommentaare ja enne liigpingete suhtlemine aitab neid riske leevendada.

###<a name="are-you-using-a-content-delivery-network-azure-cdn-in-front-of-your-web-facing-storage-blobs-and-static-assets"></a>Kas kasutate sisu kohaletoimetamise võrk (Azure'i CDN) ees oma web suunatud salvestusruumi plekid ja staatilise varad?
Kasutades CDN aitab teil võtta laadi välja oma serverites vahemällu CDN POP/serva kohtades, mis asuvad kogu maailmas sisu. Saate seda vähendamiseks latentsus, skaleeritavus suurendada, vähenda serveri koormus, ja osana strateegia eitamine service(DOS) eest kaitsta. Kui soovite rohkem teavet, kuidas kasutada oma paindlikkust suurendamiseks ja vähendamiseks oma kliendi latentsus Azure'i CDN, lugege [Ülevaade Azure'i sisu kohaletoimetamise võrk (CDN)](../cdn/cdn-overview.md).

__Mis juhtub, kui te ei kasuta CDN-ID?__ Kui te ei kasuta CDN-ID siis kõik teie kliendi liiklus tuleb otse oma ressursse. See tähendab, et näete suuremate serveritesse, mis vähendab nende skaleeritavus. Lisaks teie kliendid võivad ilmneda kõrgema latentsused CDN-ID pakkuda kogu maailmas kohad, mis võivad lähemale oma klientidele.

##<a name="next-steps"></a>Järgmised toimingud:
Kui soovite lugeda Lisateavet selle kohta, kuidas kujundada rakenduste jaoks kõrge-saadavus, lugege [Microsoft Azure loodud kõrge kättesaadavus](resiliency-high-availability-azure-applications.md).
