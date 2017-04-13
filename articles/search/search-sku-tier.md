<properties
    pageTitle="Valige soovitud SKU-ga või hinnad taseme Azure'i otsingu jaoks | Microsoft Azure'i"
    description="Azure'i otsing saate ette valmistatud veebisaidil nende SKU-de jaoks: tasuta, Basic ja Standard, kus Standard on saadaval erinevad ressursi konfiguratsioone ja võimsuse tasemete."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Valige soovitud SKU-ga või hinnad taseme Azure'i otsingu jaoks

Azure'i otsing, on [teenus on ette valmistatud](search-create-service-portal.md) teatud hinnakirjad taseme või SKU-ga. Suvandite hulka kuuluvad **tasuta**, **lihtne**või **Standard**, kus **Standard** on saadaval mitut konfiguratsioone ja võimalusi. 

Selles artiklis eesmärk aitab teil valida mõne teise astme. Kui mõne teise astme võimsus osutub liiga nõrk, peate on uus teenus kõrgema taseme veebisaidil ette ja siis uuesti oma registrid. On pole versiooniuuenduse teise sama teenuse kaudu ühe SKU-ga. 

> [AZURE.NOTE] Pärast seda, kui valite taseme- ja [sätte otsinguteenuse](search-create-service-portal.md), saate suurendada koopia ja partition loendab teenuses. Juhised leiate teemast [Ressursi tasemeid päringu ja indekseerimise töökoormus](search-capacity-planning.md).

## <a name="how-to-approach-a-pricing-tier-decision"></a>Kuidas moodust hinnakirjad taseme otsus

Azure'i otsinguväljale soovitud taseme määratleb maht, mitte funktsioonide saadavalolek. Üldiselt funktsioonid on saadaval iga taseme kohta, sh eelvaade funktsioone. Ühe erandi indexers S3 HD ei toetata.

> [AZURE.TIP] Soovitatav on alati ette valmistada (üks tellimus pole aegumiskuupäeva kohta) **Free** teenuse nii, et selle käepärast kerge projektide jaoks. Kasutage **tasuta** teenus testimine ja hindamine; Loo ** **põhi** - või** taseme koostamiseks või suurem testi töökoormus teine arveldatavate teenus.

Ametinimetus ja teenuse kohta avage käsitsi sisse käsitsi. Selle artikli teave aitab teil otsustada, milline SKU pakub õige saldo, kuid mis tahes, et see on kasulik, peate olema vähemalt hinnang järgmistest:

- Arvu ja registrite kavatsete luua
- Arvu ja dokumente üles laadida
- Päringu maht, osas päringute kohta teise Transpordieelseks aimu

Hulk ja suurus on olulised, kuna ülempiirid raske piirangut registrid või mõnes muus teenuses dokumentide arvu kaudu teile helistada või teenuse kasutatav ressursid (salvestusruumi või koopiad). Oma teenuse tegelik piirmäär on, kumb saab kasutada esimesena: ressursid või objektidele.

Hinnangud käes, tuleks lihtsustada järgmist:

- **Samm 1** Vaadake üle SKU kirjeldused saadaolevate suvandite kohta lisateavet allpool.
- **Samm 2** Vastab jõuda esialgne otsus järgmistele küsimustele.
- **Samm 3** Teie otsus viimistlemine läbivaatamise raske piirangud salvestusruumi ja hinnakirjad.

## <a name="sku-descriptions"></a>SKU kirjeldused

Järgmises tabelis on ära toodud kirjeldust iga taseme. 

Esimese taseme|Esmane stsenaariumid
----|-----------------
**Tasuta**|Ühiskasutusega teenuse tasuta, kasutatakse hindamise, uurimine või väike töökoormus. Kuna seda ühiskasutusse teiste tellijad, päringu läbilaskevõime ja indekseerimine erineb sõltuvalt, kes veel on teenuse kasutamise kohta. On väike (50 MB või 3 registrite kuni 10 000 dokumentide iga).
**Tavaline**|Väike töökoormus sihtotstarbeline riistvara. Väga saadaval. Lubatud kuni 3 koopiad ja 1 sektsiooni (2 GB).
**S1**|Standard 1 toetab sektsioonid (12) ja koopiad (12), kasutatakse keskmise tootmise töökoormus sihtotstarbeline riistvara paindlik kombinatsioonid. Saate määrata sektsioonid ja koopiad kombinatsioonid ei toeta 36 arveldatavate otsingu üksuste arvu ülempiir. Samal tasemel, sektsioonid on 25 GB ja Transpordieelseks on umbes 15 päringute sekundis.
**S2**|Standard 2 käivitatakse suuremat tootmise töökoormus kasutades sama 36 otsingu üksused S1, kuid suurema suurusega sektsioonid ja koopiad. Samal tasemel, sektsioonid on 100 GB ja Transpordieelseks on umbes 60 sekundi kohta päringuid.
**S3**|Standardse 3 töötab proportsionaalselt suurema tootmise töökoormus operatsioonisüsteemides kõrgema Lõpeta, konfiguratsioone kuni 12 sektsioonid või 12 koopiad alla 36 otsingu üksused. Samal tasemel, sektsioonid on 200 GB ja Transpordieelseks on rohkem kui 60 sekundi kohta päringutes. 
**S3 HD**|Standardse 3 suure tihedusega on mõeldud suure hulga väiksem registrid. Teil võib olla kuni 3 sektsioonid, 200 GB. Transpordieelseks on rohkem kui 60 sekundi kohta päringutes. 

> [AZURE.NOTE] Koopia ja partition üsnagi on välja arve üksustena otsing (36 üksuse iga teenuse maksimaalne), mis kui mis kaasneb maksimaalne nominaalväärtuse efektiivse alampiir. Näiteks võib kasutada kuni 12 koopiate, on kõige 3 sektsioonid (12 * 3 = 36 ühikud). Samuti saate kasutada suurima sektsioonid, vähendada koopiad 3. Vaadake [ressursside tasemeid päringu ja indekseerimise töökoormus Azure'i otsingus](search-capacity-planning.md) lubatud kombinatsioonide diagrammi.

## <a name="review-limits-per-tier"></a>Vaadake üle teise kohta piirangud

Järgmises tabelis on alamhulk piirangud [Teenuste piirangud Azure'i otsingu](search-limits-quotas-capacity.md)kaudu. See on loetletud tõenäoliselt SKU otsust mõjutavad tegurid. See diagramm võib viidata vaadates järgmistele küsimustele.

Ressurss|Tasuta|Tavaline|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Teenuseleping (SLA)|Nr <sup>1</sup> |Jah |Jah  |Jah |Jah  |Jah 
Registri piirangud|3|5|50|200|200|1000 <sup>2</sup>
Dokumendi piirangud|10 000 kokku|1 miljonit teenuse kohta.|15 miljonit partition |60 miljonit partition|120 miljonit partition |1 miljon index
Suurim lubatud sektsioonid|N/A |1 |12  |12 |12|3 <sup>2</sup>
Sektsiooni suurus|50 MB kokku|2 GB teenuse kohta|25 GB ühe partition |100 GB kohta partition (kuni 1,2 TB iga teenuse)|200 GB kohta partition (kuni 2,4 TB iga teenuse)|200 GB (kuni 600 GB teenuse kohta)
Suurim lubatud koopiad|N/A |3 |12 |12 |12|12
Päringute sekundis|N/A|~ 3 iga koopia|~ 15 koopia kohta.|~ 60 koopia|> 60 koopia|> 60 koopia

<sup>1</sup> tasuta ja eelvaade SKU-de jaoks ei toeta SLAs. SLAs jõustamine saamisel, on SKU üldiselt.

<sup>2</sup> S3 ja S3 HD on tagatud identse suuremahulised taristu, kuid iga üks jõuab selle ülempiir erineval viisil. S3 eesmärgid väiksema arvu väga suurte registrid. Sellisena on selle ülempiir ressurssi seotud (2.4 TB iga teenuse jaoks). S3 HD suunatud suure hulga väga väike registrid. Veebisaidil 1000 registrid, S3 HD jõuab oma piirid index piiranguid kujul. Kui olete S3 HD klient, kes nõuab rohkem kui 1000 registrid, pöörduge Microsoft Support teavet, mida edasi teha.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Kõrvaldada SKU-d, mis ei vasta nõuetele 

Järgmised küsimused aitavad teil jõudma SKU õigeid otsuseid jaoks oma töökoormus.

1. Kas teil on nõuded **Teenindustaseme leping (SLA)** ? Põhi- või eelvaade Standard SKU otsus piiritlemiseks.
2. **Mitu registrid** teil vaja? Üks suurim muutujate üheks SKU otsust faktooring on iga SKU ei toeta registrite arv. Registri tugi on hinnakirjad määras märgatavalt erinevatel tasanditel. Nõuded registrite arv võib olla esmane determinandi SKU otsust.
3. **Kui palju dokumente** laaditakse iga indeksi sisse? Dokumentide arvu ja määrab registri lõpliku suurust. Eeldades, et saate hinnata registri plaanitud suurust, saate võrrelda numbrit vastu megabaitide SKU-ga, nõutav, et suurus registri talletamiseks sektsioonid arvu võrra pikendatud kohta. 
4. **Mis on oodatud päringu laadimise**? Kui nõuded on aru saanud, kaaluge päringu töökoormus. S2 ja nii S3 SKU-d pakuvad lähedal võrduva läbilaskevõime, kuid SLA nõuded reegel välja mis tahes eelvaade SKU-de jaoks. 
5. Kui kaalute S2 või S3 taseme, kindlaks teha, kas teil on vaja [indexers](search-indexer-overview.md). Indexers ei ole veel S3 HD taseme jaoks saadaval. Alternatiivne lähenemine on kasutada tõuketeatised mudeli index värskendusi, kuhu kirjutada rakenduse koodi PTT andmehulga registri.

Enamik kliente saate reegel teatud SKU, sisse- või väljapoole vastavalt nende vastused ülaltoodud küsimustele. Kui te veel pole kindel, millist SKU minna, saate postitada küsimusi MSDN-i või StackOverflow Foorumid või Azure tugiteenuste täiendavad juhised.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Otsus valideerimine: kas kasutatava SKU pakub piisavalt salvestusruumi ja Transpordieelseks?

Viimase sammuna vaadata [lehe hinnad](https://azure.microsoft.com/pricing/details/search/) ja [teenuste piirangud iga teenuse ja kohta index jaotiste](search-limits-quotas-capacity.md) proovieksemplar prognoosi suhtes tellimus ja teenuste piirangud. 

Kui hind või salvestusruumi nõuded on vahemikust väljas, mida soovite refaktoorime töökoormus mitme väiksema teenuste vahel (näiteks). Täpsema tasemel, võib ümberkujundamiseks registrid väiksem või päringute tõhusamaks filtrite abil.

> [AZURE.NOTE] Mäluruumi saab üle täidetav, kui kõrvalised andmeid sisaldavad dokumendid. Ideaalvariandis mitte dokumendid sisaldavad ainult otsitavate andmete või metaandmete. Binaarandmeid on otsitav ja hoida eraldi (ehk Azure'i tabeli või bloobimälu salvestusruumi) koos index viite URL-i väliste andmete mahutamiseks välja. Dokumendi maksimaalne maht on 16 MB (või vähem kui olete ühe taotluse mitme dokumendi üleslaadimine hulgi). Lisateavet leiate [Azure'i otsingu teenuse piirangud](search-limits-quotas-capacity.md) .

## <a name="next-step"></a>Järgmise juhise juurde

Kui teate, mis SKU-ga on õige sobivad, jätkata järgmiselt:

- [Otsinguteenuse portaali loomine](search-create-service-portal.md)
- [Sektsioonid ja koopiad teenust skaala muutmine](search-capacity-planning.md)

