<properties
    pageTitle="Azure'i otsingus Multitenancy modelleerimine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Lisateavet levinud kujundus mustrite rentnikuga SaaS rakenduste Azure otsingu kasutamise ajal."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Kujundage mustrite rentnikuga SaaS rakenduste ja Azure otsing

Rentnikuga rakenduse on üks, mis pakub sama teenuste ja võimaluste rentnike jaoks, kes ei näe või teine rentnik andmete ühiskasutus iga rida. Selle dokumendi käsitletakse rentniku eraldamise strateegiad rentnikuga rakenduste ehitatud Azure'i otsing.

## <a name="azure-search-concepts"></a>Azure'i otsingu mõisted
Otsingu-kui-a-service lahendusena Azure'i otsing võimaldab arendajatel lisada rikkaliku otsing kogemusi rakendusi ilma mis tahes taristu haldamise või muutudes ekspert otsing. Andmete teenuse ja seejärel pilves talletatud. Lihtsa taotluste Azure'i otsingu API abil andmeid saab siis muudetud ja otsida. Teenuse ülevaate leiate [selle](http://aka.ms/whatisazsearch)artikli. Enne kujundus mustrite, on oluline mõista mõned mõisted Azure'i otsing.

### <a name="search-services-indexes-fields-and-documents"></a>Teenuste, registrid, väljade ja dokumentide otsimine
Azure'i otsingu kasutamisel ühte tellib _otsinguteenus_. Kui andmed on üles laaditud Azure'i otsimiseks, talletatakse sees otsinguteenuse _register_ . Ei saa arv ühe teenuses registrid. Andmebaaside kasutada tuttav, võib otsinguteenus võrrelda andmebaasi ajal teenuse registrite võrrelda tabeleid andmebaasi.

Iga sees otsinguteenuse register on oma skeemi, mis on määratletud kohandatavad _väljad_. Andmed lisatakse Azure'i otsinguregistri üksikute _dokumentide_. Iga dokumendi teatud indeksi tuleb üles laadida ja peab vastavalt selle indeksi skeemi. Azure'i otsingu abil andmete otsimisel otsingu päringud on antud kindla register.  Need mõisted neile andmebaasi võrdlemiseks väljad võrrelda tabeli veergude ja dokumentide võrrelda read.

### <a name="scalability"></a>Skaleeritavus.
Mis tahes Azure'i otsinguteenuse Standard [hinnad taseme](https://azure.microsoft.com/pricing/details/search/) saate mastaapimiseks kahes dimensioonis: salvestusruumi ja kättesaadavust.
* _Sektsioonid_ saab lisada otsinguteenuse talletusmahu suurendamiseks.
* Teenuse taotlusi, et otsinguteenuse hakkama läbilaskevõime suurendamiseks saate lisada _koopiad_ .

Lisada ja sektsioonid ja koopiad veebisaidil võimaldab kasvada suurt hulka andmeid ja liiklus rakenduse nõudmistele otsinguteenus võimsus. Selleks, et lugeda [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)saavutamiseks otsinguteenuse, eeldab kahe koopiad. Selleks saavutamiseks lugemis-ja kirjutamisõigusega [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)teenus, on vaja kolme koopiad.


### <a name="service-and-index-limits-in-azure-search"></a>Azure'i otsingu teenuse ja register piirangud
On mõni muu [hinnad astme](https://azure.microsoft.com/pricing/details/search/) Azure'i otsingus, kasutatakse on erinevad [piirangud ja kvootide](search-limits-quotas-capacity.md). Mõned need piirangud on teenuse tasemel, mõned on index tasemel ja mõned on partition tasemel.


|                                  | Tavaline     | Standard1   | Standard2   | Standard3   | Standard3 HD  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Suurim lubatud koopiad teenuse kohta.     | 3         | 12          | 12          | 12          | 12            |
| Suurim lubatud sektsioonid teenuse kohta.   | 1         | 12          | 12          | 12          | 1             |
| Üksuste maksimaalne otsing (koopiad * sektsioonid) teenuse kohta. | 3         | 36          | 36          | 36          | 36 (max 3 sektsioonid)            |
| Suurim lubatud dokumentide teenuse kohta.    | 1 miljonit | 180 miljonit | 720 miljonit | 1,4 miljardit | 600 miljonit   |
| Suurim lubatud talletusmaht teenuse kohta      | 2 GB      | 300 GB      | 1.2 TB      | 2.4 TB      | 600 GB        |
| Suurim lubatud dokumentide arv Partition  | 1 miljonit | 15 miljonit  | 60 miljonit  | 120 miljonit | 200 miljonit   |
| Suurim lubatud salvestusruumi Partition    | 2 GB      | 25 GB       | 100 GB      | 200 GB      | 200 GB        |
| Suurim lubatud registrite teenuse kohta.      | 5         | 50          | 200         | 200         | 3000 (max 1000 registrid/sektsioon)          |


#### <a name="s3-high-density"></a>S3 Suure tihedusega "
Azure'i otsingu S3 hinnakirjad astme, on võimalus rentnikuga stsenaariumid mõeldud kõrge tihedusfunktsiooni (HD) režiimi. Paljudel juhtudel on vajalik toetamiseks suure hulga väiksemad rentnikud jaotises ühe teenuse lihtne ja maksumus kasu saavutamiseks.

S3 HD võimaldab palju small registrid pakitakse ühe otsinguteenuse juhtimise all, börsipäev võimalus mastaapimiseks registrite abil majutada veel ühe teenuse registrite sektsioonid võimet välja.

Konkreetselt teenuse S3 võib olla 1 kuni 200 registrid majutavad koos võib kuni 1,4 miljardit dokumentide vahel. S3 HD teiselt võimaldaks üksikute registrid minna ainult kuni 1 miljon dokumente, kuid seda võib käsitleda kuni 1000 registrid sektsiooni (kuni 3000 iga teenuse) kogu dokumendis arv 200 miljoni sektsiooni kohta (kuni 600 miljonit teenuse kohta).



## <a name="considerations-for-multitenant-applications"></a>Kaalutlused rentnikuga rakenduste
Rentnikuga rakenduste peab tõhus levitamine ressursid on rentnikud säilitades mõned taseme vaheline erinevate rentnikega. On paar kaalutlused kujundamisel arhitektuur ette.

* _Rentniku eraldamise:_ Rakenduste arendajatele vaja võta asjakohaseid pole rentnikud on volitused või soovimatu muud rentnikud andmetele juurdepääsu tagamiseks. Lisaks andmete privaatsust perspektiivi, rentniku eraldamise strateegiad vaja ühiskasutusega ressursse ja maksta kaitse tõhusa haldamise.
* _Cloud Ressursi maksumus:_ Sarnaselt muude rakenduste tarkvara lahenduste peab jääma maksumus konkurentsianalüüsi rentnikuga rakenduse osana.
* _Hõlpsamaks toimingud:_ Rentnikuga arhitektuuri väljatöötamisel rakenduse toimingute ja keerukuse mõju on oluline. Azure'i otsing on mõne [99,9% SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* _Globaalne andmed väiksemates vähem ruumi:_ Rentnikuga rakenduste peate tõhusalt teenida rentnikud, mis asuvad kogu maailmas.
* _Skaleeritavus:_ Rakenduste arendajatele peaks kaaluma, kuidas need sobitada, säilitades piisavalt madal rakenduse keerukuse ja rentnikud arv ja suurus rentnikud andmed ja töökoormus mastaapimiseks rakenduse kujundamise vahel.

Azure'i otsing pakub mõne piirmäärad rentnikud andmed ja töökoormus eristamiseks kasutatavate.

## <a name="modeling-multitenancy-with-azure-search"></a>Modelleerimine multitenancy Azure otsingu abil
Rentnikuga stsenaariumi puhul rakenduse arendaja tarbib ühe või mitme otsingu teenuste ja jagage oma rentnikke teenused, registrite või mõlemad vahel. Azure'i otsing on mõned levinud mustrite kui modelleerimine rentnikuga stsenaariumi.

1. _Index rentniku kohta:_ Iga Rentnik on oma jooksul muud rentnikud ühiskasutusse antud otsinguteenuse register.
1. _Teenuse rentniku kohta:_ Iga rentniku on oma sihtotstarbeline Azure'i otsingu teenus pakub kõrgeima taseme andmed ja töökoormus eraldamine.
1. _Kombineerida:_ Suurema ja Lisateavet aktiivne rentnikud määratakse sihtotstarbeline teenuste ajal väiksemad rentnikud määratakse üksikute registrite ühisteenuste.

## <a name="1-index-per-tenant"></a>1. indekseerida rentniku kohta
![Kujutamine mudel index rentniku kohta.](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

Registri – rentniku mudelis mitme rentniku hõivata ühe Azure'i otsinguteenuse, kus iga rentniku on oma indeks.

Rentnikud saavutada andmete eraldamise, kuna kõik otsingu taotlusi ja dokumendi toimingud on välja antud tasemel index Azure'i otsing. Rakenduse kiht, on vaja teadlikkuse proper registrid erinevate rentnikega liikluse suunamiseks samal ajal ka kõik rentnikud haldamise ressursid tasemel.

Võtme atribuut index – rentniku mudeli on võimalus rakenduse arendaja abil oversubscribe otsinguteenuse rakenduse rentnikud võimsus. Kui soovitud rentnikud on töökoormus jaotamine, saate optimaalse kombinatsiooni rentnikud levitatakse kogu otsingu teenuse registrid mahutamiseks arvu aktiivne, ressursse rentnikud pikk saba vähem aktiivne rentnikega korraga kandmise ajal. Kompromiss on mudel ei suuda käsitlema olukordades, kus iga rentniku samaaegselt aktiivne.

Registri – rentniku mudeli alus mudeli muutuja maksumus, kellelt ostsite kogu Azure'i otsinguteenuse võiksite ja seejärel uuesti täis rentnikke. See võimaldab kasutamata läbilaskevõime määratud katsete arv ja tasuta kontod.

Globaalne jalajälg rakendusi, ei pruugi index – rentniku mudeli kõige tõhusam. Kui rakenduse rentnikud levitatakse kogu maailmas, eraldi teenus võib olla vajalik iga piirkonna, mis võivad dubleerimine maksumus üks neist üle.

Azure'i otsing võimaldab nii üksikute registrid ja registrite arv kasvada. Kui asjakohased hinnad taseme on valitud, sektsioonid ja koopiad saab lisada kogu otsinguteenuse kui üksikute registri teenuses paisub liiga suureks salvestusruumi või liikluse.

Kui registrid koguarvu paisub liiga suureks ühe teenuse, muu teenuse peab olema ette valmistatud uute rentnike mahutamiseks. Kui registrid teisaldada otsingu teenuste vahel on lisatud uusi teenuseid, registri andmed on käsitsi kopeerida ühe registrist teise nagu Azure'i otsing ei võimalda registri üle viia.


## <a name="2-service-per-tenant"></a>2 teenuse rentniku kohta
![Kujutamine mudeli teenuse rentniku kohta.](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Teenuse – rentniku arhitektuur, on iga rentniku oma otsinguteenuse.

See mudel rakenduse saavutab maksimaalne selle rentnike jaoks eraldi. Iga teenuse eesmärk on jätkuvalt salvestus- ja läbilaskevõime käsitlemise Otsingutaotluse kui ka eraldi API võtmed.

Rakenduste, kus iga Rentnik on suur jalajälg või töökoormus on vähe hajuvuse rentniku rentniku, teenuse-rentniku mudeli on oleva tegeliku valiku ressursid ei jagataks üle erinevate rentnikega töökoormus.

Rentniku mudeli kohta teenus pakub ka hõlpsalt prognoosida, fikseeritud kulu mudeli kasu. Enne, kui on rentniku täitmiseks, kuid maksumus-kohta – rentniku on suurem kui register – rentniku mudelit, mis on pole võiksite investeering kogu teenuse.

Teenuse – rentniku mudeli on rakenduste globaalse jalajälje tõhusa valik. Rentnikud geograafiliselt jaotatud, on lihtne iga rentniku teenus on sobiv piirkond.

Skaleerimist see muster probleeme tekkida, kui üksikute rentnikud kasvada oma teenuse. Azure'i otsing ei toeta praegu üleminekut hinnakirjad taseme otsinguteenuse, et kõik andmed on uus teenus käsitsi kopeerida.

## <a name="3-mixing-both-models"></a>3. mõlema segamine
Mõne muu mustri modelleerimine multitenancy segamist index – rentniku-ja teenuse rentniku kohta.

Segamise kahte tüüpi, rakenduse suurim rentnikud hõivata sihtotstarbeline teenuste ajal vähem aktiivne, väiksemad rentnikud pikk saba saate hõivata registrid ühiskasutusega teenuses. See mudel tagab suurima rentnikega pidevalt kõrge jõudlusprobleeme teenuse väiksemad rentnikud kaitsmine mis tahes maksta.

Siiski selle strateegia tugineb prognoos prognoosida, millised rentnikud vajavad asjakohast teenuse võrreldes registri ühiskasutusega teenuse sisse. Rakenduse keerukuse suureneb vajadust haldamine mõlemad multitenancy mudelid.

## <a name="achieving-even-finer-granularity"></a>Isegi olevaid saavutamiseks
Ülaltoodud kujundus mustrite mudel rentnikuga stsenaariumid Azure'i otsingus endale ühtse ulatus, kus iga Rentnik on kogu eksemplari rakendus. Rakenduste vahel saate siiski toime palju väiksem otsinguulatuste.

Kui teenus – rentniku ja index – rentniku mudelite pole piisavalt väike otsinguulatuste, on võimalik registri soovite saavutada isegi peenem granulaarsus mudel.

On ühe index, mis käituvad teisiti muu kliendi lõpp-punktide jaoks, saate lisada välja registri, mis tähistab iga võimalike kliendi teatud väärtus. Iga kord, kui mõnda muud klienti helistab Azure'i otsingu päringu või muuta registri, saate määrata klientrakenduse kood sobiv väärtus kasutades Azure otsingu [filter](https://msdn.microsoft.com/library/azure/dn798921.aspx) võimalus päringu ajal välja.

Seda meetodit saate kasutada funktsioone eraldi Kasutajakontod eraldi õigusetasemed, saavutamiseks ja isegi täiesti eraldi rakendused.

## <a name="next-steps"></a>Järgmised sammud
Azure'i otsing on pilkupüüdvaid valik rakendustele, [Lugege lisateavet teenuse võimaluste kohta](http://aka.ms/whatisazsearch). Erinevate kujundus mustrite rentnikuga rakenduste võrdlemisel kaaluge [erinevad hinnakirjad astme](https://azure.microsoft.com/pricing/details/search/) ja parim kohandada Azure'i otsingu rakenduse töökoormus ja igas suuruses arhitektuurides mahutamiseks vastavate [teenuste piirangud](search-limits-quotas-capacity.md) .

Azure'i otsingu ja rentnikuga stsenaariumid küsimusi saab suunata azuresearch_contact@microsoft.com.
