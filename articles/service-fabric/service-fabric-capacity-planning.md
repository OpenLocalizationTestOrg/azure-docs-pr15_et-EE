<properties
   pageTitle="Valmisoleku kavandamine teenuse struktuuri rakenduste | Microsoft Azure'i"
   description="Kirjeldab, kuidas määrata Arvuta sõlmed on vaja teenuse struktuuri rakendada arv"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Valmisoleku teenuse struktuuri rakenduste kavandamine


Selle dokumendi õpetab ressursid (protsessorid RAM, ketta salvestusruumi), peate käivitama Azure teenuse struktuuri rakenduste hinnata, tehke järgmist. On tavaline oma ressursi nõuded aja jooksul muutuda. Vajate tavaliselt mõned ressursid, kui teie teenuse arendamise või test, ja seejärel nõuab rohkem ressursse tootmise ja rakenduse kasvab populaarsus. Rakenduse kujundamisel pikaajalised nõuded läbi mõtlema ja teha valikuid, mis võimaldavad kõrge klientide rahuldamiseks mastaapimiseks teenust.

 Teenuse struktuuri kobar loomisel saate otsustada, mis tüüpi virtuaalmasinates (VMs) moodustavad klaster. Iga VM on piiratud hulgal protsessorid (tuuma ja kiiruse), võrgu läbilaskevõime, RAM-i ja kettaruumi kujul. Kui teie teenuse kasvab aja jooksul, täiendamist vms, mis pakub suurem ressursse ja/või lisada rohkem VMs klaster. Viimase tegemiseks peab teie teenuse algselt arhitekt, nii, et see saab ära saada dünaamiliselt lisada klaster uue VMs.

Teatud teenuste haldamine vähe ise VMs pole andmeid. Seetõttu valmisoleku kavandamine nende teenuste tuleks keskenduda peamiselt jõudluse, mis tähendab, et valida VMs vastav protsessorid (tuuma ja kiiruse). Lisaks peaksite võrgu läbilaskevõime, sh tihti toimuvad võrgu edastamine ja kui palju andmeid kantakse üle. Kui teenust peab kasutus suureneb ka tegutsemine, saate lisada rohkem VMs võrgu taotleb üle kõik VMs kobar ja laadi saldo.

Teenuste haldamine suure hulga andmete VMs, võimsuse planeerimine peaks keskenduda peamiselt suurus. Seega peaksite hoolikalt soovitud VM RAM-i ja kettaruumi. Virtuaalmälu juhtimise süsteemi Windows teeb näevad RAM-i rakenduse koodi kettaruumi. Lisaks teenuse struktuuri käitusaja pakub nutikas Piipar ainult kuum andmete säilitamise mälu ja külma andmete teisaldamine ketas. Rakenduste, seega saate kasutada rohkem mälu, kui on saadaval füüsilise VM. Kas teil on rohkem RAM lihtsalt suurendab jõudlust, kuna VM jätta rohkem vaba ruumi RAM. Valige VM peaks olema piisavalt suur, et andmed, mida soovite salvestada VM ketas. Samuti VM peaks olema piisavalt RAM soovite jõudlus pakkumiseks. Kui teie teenuse andmete kasvab aja jooksul, saate lisada rohkem VMs kobar ja sektsiooni andmed üle kõik VMs.

## <a name="determine-how-many-nodes-you-need"></a>Kui palju teil on vaja sõlmed määratlemine

Teenust eraldamine võimaldab mastaapimiseks teie teenuste andmeid. Eraldamine kohta leiate lisateavet teemast [Teenuse struktuuri eraldamine](service-fabric-concepts-partitioning.md). Iga sektsiooni peab mahtuma ühe VM, kuid mitme (väike) sektsioonid saab panna ühte VM. Võttes veel väikest sektsioonid annab teile suurema paindlikkuse, kui mõni suurem sektsioonid. Kompromiss on palju sektsioonide suureneb teenuse struktuuri pea kohal ja tehingu toiminguid ei saa teha sektsioonid üle. Olemas on ka mitu võimalikku võrguliikluse kui teie teenuse kood sageli vajab kindlaid andmeid, mis talletatakse eri sektsiooni. Oma teenuse kujundamisel peaksite hoolikalt nende spetsialistid ja miinuste jõuda tõhus eraldamine strateegia.

Oletame, et teie taotlus on üks stateful teenus, mis on aastas DB_Size GB kasvada eeldatavat poe suurus. Olete valmis lisada rohkem rakenduste (ja sektsioonid) teil tekib pärast selle kasv.  Dispersioonanalüüs tegur (RF), mis määratleb teie teenuse jaoks koopiate arvu mõjutab kokku DB_Size. Kokku DB_Size üle kõik koopiad on Dispersioonanalüüs tegur korrutatuna DB_Size.  Node_Size tähistab vaba ruumi/RAM sõlm, mida soovite kasutada oma teenuse kohta. Parima jõudluse tagamiseks on DB_Size peaks sobivad mälu üle klaster, ning Node_Size, mis on ümber RAM VM tuleb valida. Jagamisega Node_Size, mis on suurem kui RAM maht, olete tuginedes Piipar, mis on esitatud teenuse struktuuri runtime. Seetõttu jõudluse ei pruugi optimaalse kui kogu andmete loetakse kuum (ajast andmed on leheküljed- /). Paljude teenuste, kus ainult osa andmeid on kuum, on keerulisemaks.

Nõutav maksimaalse jõudluse sõlmed arvu arvutatakse järgmiselt:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Konto kasvu jaoks

Kui soovite arvutada sõlmed põhjal eeldatavat teenust kasvada, lisaks DB_Size, mis teil algas DB_Size arvu. Seejärel kasvata sõlmed arvu teenust kasvab nii, et teil on üle ettevalmistamise sõlmed arv. Kuid sektsioonid arvu põhinema sõlmed, mis on vajalikud, kui kasutate teenust maksimaalse kasvu arv.

See on hea on mõned eest masinad igal ajal, nii et saate hakkama ega diagrammi ootamatu tõrge, (nt juhul, kui mõni VMs minna).  Ajal eest võimsus määratakse, kasutades oma oodatud diagrammi, on alguspunkt on mitu täiendavat VMs reserveerida (5 – 10 protsenti eest).

Eelmise eeldab ühe stateful teenus. Kui teil on rohkem kui üks stateful teenus, peate lisama DB_Size, mis on seotud võrrandiks muude teenustega. Teise võimalusena saate arvutada sõlmed iga stateful teenuse jaoks eraldi arv.  Teenust võib-olla koopiad või sektsioonid, mis pole tasakaalus. Pidage meeles, et sektsioonid võib olla ka rohkem andmeid, kui teised. Eraldamine kohta leiate lisateavet teemast [eraldamine artiklis heade tavade kohta](service-fabric-concepts-partitioning.md). Eelmise võrrand on siiski partition ja koopia agnostik, kuna teenuse struktuuri tagab, et selle koopiad on levinud sõlmed optimeeritud viisil.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Kasutada arvutustabeliga kulude arvutamise

Nüüd vaatame sellele mõni reaalarvudeks valemis. Mõni [näide arvutustabeli](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) kujutatakse võimsuse rakendus, mis sisaldab kolme tüüpi andmed objektide jaoks. Iga objekti me ligikaudne selle suurust ja kui palju objektide me oodata on. Me valida ka mitu koopiad soovime iga objekti tüüp. Arvutustabeli arvutab mälu talletamise klaster kogusumma.

Me sisestage VM suurus ja kuutasuga. Põhineb VM suurus, arvutustabel ütleb sektsioonid tuleb kasutada andmete mahutamiseks füüsilise sõlmed tükeldamiseks minimaalne arv. Soovite võib-olla suurem arv sektsioonide majutada oma rakenduse teatud arvutus võrguliiklust vajadustele. Arvutustabeli kuvatakse sektsioonid, mis on haldamise kasutaja profiili objektide arv on kasvanud ühest kuus.

Nüüd kõik selle teabe põhjal, arvutustabel näitab, et te võite füüsilise kõik andmed soovitud sektsioonid ja koopiad 26 sõlme klaster. Siiski see oleks tihedalt pakkida, nii, et soovite mõned täiendavad sõlmed mahutamiseks sõlm tõrkeid ja uuendamine. Arvutustabeli näitab, et teil on rohkem kui 57 sõlmed pakub täiendavaid väärtuseta ma pean tühja sõlmed. Uuesti soovite minna üle 57 sõlmed ikkagi mahutamiseks sõlm tõrkeid ja uuendamine. Saate kohandada efektisuvandite arvutustabeli rakenduse vajadustele vastavaks.   

![Arvutustabeli kulude arvutamise][Image1]



## <a name="next-steps"></a>Järgmised sammud

Tutvuge [teenuse struktuuri eraldamine teenuste] [ 10] Lisateavet eraldamine teenust.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
