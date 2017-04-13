<properties
    pageTitle="Azure'i lahedad salvestusruumi plekid | Microsoft Azure'i"
    description="Storage astme jaoks Azure'i bloobimälu pakkuda objekti andmete põhjal Accessi mustrite maksumus tõhusa salvestusruum. Lahedad talletusmahu taseme on optimeeritud andmeid, mis on harvem."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure'i bloobimälu: Kuum ja lahedad storage astme

## <a name="overview"></a>Ülevaade

Azure'i salvestusruumi pakub nüüd kaks storage astme jaoks bloobimälu (objekti salvestusruumi), nii, et saate salvestada oma andmeid kõige efektiivselt sõltuvalt sellest, kuidas seda kasutada. Azure'i **kuum talletusmahu taseme** on optimeeritud andmeid, mis on sageli talletamiseks. Azure'i **lahedad talletusmahu taseme** on optimeeritud andmeid, mis on harva juurde ja vanade talletamiseks. Lahedad talletusmahu taseme andmeid saab luba madalama kättesaadavus, kuid nõuab veel kõrge kestvus ja sarnase aega juurdepääsu ja läbilaskevõime omadused kuum andmetena. Lahedad andmete korral madalama kättesaadavus SLA ja suuremad Accessi kulud on aktsepteeritav kompromisse palju salvestusruumi väiksemad halduskulud.

Täna, kasvab eksponentsiaalse tempos pilves talletatud andmed. Haldamiseks laiendada salvestusruumi tarbeks kulud, on kasulik andmete põhjal atribuudid nagu Accessi ja kavandatud säilitusperiood korraldamiseks. Pilves talletatud andmed võib olla erinevad, kuidas see on loodud, töödeldav ja tööiga üle. Mõned andmed on aktiivselt avada ja muuta selle eluea. Mõned andmed on sageli väga aegsasti tööiga, pukseerimine oluliselt aegade andmed Accessi kättesaadav. Mõned andmed jääb jõude pilves ja harva, kui kunagi varem, kui juurde salvestatud.

Kõik need andmetele juurdepääsuks kasutada hübriidlahendusi kasu liigendatud taseme salvestusruumi, mis on optimeeritud kindla Accessi mustri eespool kirjeldatud. Sissejuhatus kuum ja lahedad storage astme, Azure'i bloobimälu salvestusruumi käsitletakse nüüd vajadus liigendatud storage astme koos eraldi hinnad mudelid.

## <a name="blob-storage-accounts"></a>Bloobimälu salvestusruumi kontod

**Bloobimälu salvestusruumi kontod** on eriotstarbeline salvestusruumi kontod struktureerimata andmete talletamiseks nimega Azure Storage plekid (objektid). Bloobimälu salvestusruumi kontode, Nüüd võite vahel kuum ja lahedad storage astme talletamiseks harvem juurde lahedad andmete alumise säilitamisel maksumus ja talletada sagedamini juurde kuum andmete alumise juurdepääsu, maksumus. Bloobimälu salvestusruumi kontod on sarnane olemasoleva üldotstarbeline salvestusruumi kontode ja kõik hea kestvus, kättesaadavus, skaleeritavus ja jõudluse funktsioonid, mida kasutate juba täna, sh 100% API järjepidevus Blokeeri plekid ühiskasutus ja lisa plekid.

> [AZURE.NOTE] Bloobimälu salvestusruumi kontod toetavad ainult plokk ja lisada plekid ja pole lehe plekid.

Bloobimälu salvestusruumi kontod seada **Accessi taseme** atribuut, mille abil saate määrata talletusmahu taseme **kuum** või **lahedad** sõltuvalt konto talletatud andmed. Kui kasutus mustri andmete muutmist, saate ka vaheldumisi neid storage astme igal ajal.

> [AZURE.NOTE] Talletusmahu taseme muutmine võib põhjustada lisatasud. Lugege lisateavet jaotises [hinnakirjad ja arveldamine](storage-blob-storage-tiers.md#pricing-and-billing) .

Näide kasutamise stsenaariumid jaoks saadaolevad talletusmahu taseme järgmised.

- Andmed, mis on aktiivses kasutuses või eeldatakse, et juurde (vt kaudu ja kirjutatud) sageli.
- Andmed, mis on etapiviisilise töötlemiseks ja lõpuks migratsiooni lahedad talletusmahu taseme.

Lahedad talletusmahu taseme kasutus näidisstsenaariumid on järgmised.

- Varundus arhiivimine ja katastroofi taastamine andmekomplektide.
- Vanema media sisu pole enam sageli vaadata, kuid peaks olema saadaval kohe, kui juurde.
- Suurte andmekogumite, mis peavad olema talletatud maksumus tõhus ajal rohkem andmeid on kogutud tulevaste töötlemine. (*nt*pikemaks teadusliku andmete töötlemata telemeetria tootmisprotsessi, alates)
- Algne (töötlemata) andmed, mis tuleb säilitada, isegi juhul, kui see on töödeldud lõplik kasutatavas vormis. (*nt*töötlemata meediumifailide pärast transkodeerida sisse muudesse vormingutesse)
- Nõuetele vastavus ja arhiivimine andmeid, mida tuleb säilitada pikka aega ja pääseb juurde peaaegu kunagi. (*nt*turvalisus kaamera videomaterjali, vana X-Rays/MRIs tervishoiuorganisatsioonid, helisalvestiste ja logifailid kliendi helistab ette)

Salvestusruumi kontode kohta lisateabe saamiseks vt [kohta Azure salvestusruumi kontod](storage-create-storage-account.md) .

Ainult rakenduste blokeerimine või lisa bloobimälu, soovitame kasutada bloobimälu salvestusruumi kontod, liigendatud hinnakirjad mudeli astmeline salvestusruumi eeliseid. Siiski saame aru, et see ei pruugi olla võimalik teatud tingimustel kui üldotstarbeline salvestusruumi kontodelt oleks tee, näiteks:

- Peate tabelid, järjekorrad või failide kasutamine ja soovite oma plekid sama salvestusruumi talletatakse. Pange tähele, et on pole tehnilise kasutada talletamise need sama konto Muuga kui number, millel on sama ühiskasutusse võtmed.
- Peate endiselt klassikaline juurutamise mudeli kasutamine. Bloobimälu salvestusruumi kontod on ainult Azure ressursihaldur juurutamise mudeli kaudu.
- Peate kasutama lehe plekid. Lehe plekid bloobimälu salvestusruumi kontod ei toeta. Soovitame üldiselt abil Blokeeri plekid juhul, kui teil on vaja kindla lehe plekid.
- [Salvestusruumi teenuste REST API -ga](https://msdn.microsoft.com/library/azure/dd894041.aspx) , kui 2014-02-14 või kliendi teek versiooni kasutate versiooniga, mis on väiksem kui 4.x ja ei saa värskendada oma rakenduse.

> [AZURE.NOTE] Bloobimälu salvestusruumi kontod toetavad praegu enamik Azure piirkondade rohkem jälgimiseks. Leiate saadaval piirkondade [Regiooniti Azure'i teenuste](https://azure.microsoft.com/regions/#services) lehel värskendatud loendiga.

## <a name="comparison-between-the-storage-tiers"></a>Storage astme võrdlus

Järgmises tabelis olulisemad toimingud kahe storage astme võrdlus.

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Saadaolevad talletusmahu taseme</center></strong></td>
    <td><strong><center>Lahedad talletusmahu taseme</center></strong></td
</tr>
<tr>
    <td><strong><center>Kättesaadavus</center></strong></td>
    <td><center>99,9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>Kättesaadavus<br>(RA-GRS loeb)</center></strong></td>
    <td><center>99,99%</center></td>
    <td><center>99,9%</center></td>
</tr>
<tr>
    <td><strong><center>Kulude</center></strong></td>
    <td><center>Suuremad salvestusruumi kulud<br>Väiksemad juurdepääs ja tehingu kulud</center></td>
    <td><center>Lower salvestusruumi kulud<br>Suuremad juurdepääs ja tehingu kulud</center></td>
</tr>
<tr>
    <td><strong><center>Minimaalne objekti suurus<center></strong></td>
    <td colspan="2"><center>N/A</center></td>
</tr>
<tr>
    <td><strong><center>Minimaalne salvestusruumi kestus<center></strong></td>
    <td colspan="2"><center>N/A</center></td>
</tr>
<tr>
    <td><strong><center>Latentsus<br>(Esimene bait aeg)<center></strong></td>
    <td colspan="2"><center>millisekundit</center></td>
</tr>
<tr>
    <td><strong><center>Skaleeritavus ja jõudluse sihtkohad<center></strong></td>
    <td colspan="2"><center>Sama, mis üldotstarbeline salvestusruumi kontod</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] Bloobimälu salvestusruumi kontod toetada sama jõudlus ja skaleeritavus sihtkohtade üldotstarbeline salvestusruumi kontod. Lisateavet leiate [Azure'i salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md) .

## <a name="pricing-and-billing"></a>Hinnad ja arveldamise kohta

Bloobimälu salvestusruumi kontod kasutada uut hinnakirjad mudeli bloobimälu talletusmahu taseme põhjal. Bloobimälu salvestusruumi konto kasutamisel kehtivad arvelduse järgmisega.

- **Salvestusruumi maksumus**: Lisaks talletatud andmed, andmete salvestamiseks maksumus erineb sõltuvalt talletusmahu taseme. Kohta gigabait maksumus on väiksem lahedad talletusmahu taseme kui kuum talletusmahu taseme.
- **Andmetele juurdepääsu maksumus**: lahedad talletusmahu taseme asuvate andmete korral peate tasuma kohta gigabait andmete juurdepääsu eest loeb ja kirjutab.
- **Tehingu maksumus**: on tehingu eest tasu nii astme. Lahedad talletusmahu taseme kohta tehingu hind on siiski kõrgem kui kuum talletusmahu taseme.
- **Andmesidetasude Geo-Dispersioonanalüüs andmed**: See kehtib ainult kontod geo-dispersioonanalüüs konfigureeritud, sh GRS ja RA-GRS abil. Geo-dispersioonanalüüs andmeedastus andmesidekasutusele gigabait eest tasu.
- **Andmesidetasude väljaminev liiklus andmete**: väljamineva andmeedastuse (andmed, mis on Azure piirkonnast kantakse) tekkida arveldus gigabait kohta järgides, üldotstarbeline salvestusruumi kontode läbilaskevõime kasutuse.
- **Talletusmahu taseme muutmine**: lahedad kuum talletusmahu taseme muutmisel, kehtib tasu võrdne lugemine kõik salvestusruumi konto iga ülemineku olemasolevad andmed. Teisalt, muutuvad talletusmahu taseme kuum lahedad on tasuta.

> [AZURE.NOTE] Selleks, et kasutajad saaksid proovida uue storage astme ja kinnitage funktsioonid postituse käivitada, tasuta muutmise talletamist astme alates lahedad kuum loobutakse välja kuni 30 juuni 2016. Juuli 1st 2016 alustamist tasu rakendatakse kõik üleminekud lahedad kuum kaudu. Üksikasjalikumat teavet bloobimälu hinnakirjad mudeli kontod, [Azure'i salvestusruumi hinnad](https://azure.microsoft.com/pricing/details/storage/) lehelt leiate. Üksikasjalikumat teavet väljaminev liiklus andmete edastamine kulude vaatamiseks [Andmete edastamine hinnad üksikasjade](https://azure.microsoft.com/pricing/details/data-transfers/) lehe.

## <a name="quick-start"></a>Lühijuhend

Selles jaotises demonstreerime Azure'i portaalis järgmistel juhtudel:

- Kuidas luua bloobimälu salvestusruumi konto.
- Kuidas bloobimälu salvestusruumi konto haldamine.

### <a name="using-the-azure-portal"></a>Azure'i portaalis

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Portaalis Azure'i bloobimälu salvestusruumi konto loomine

1. [Azure'i portaali](https://portal.azure.com)sisse logida.

2. Valige menüüs jaoturi **Uus** > **andmete + salvestusruumi** > **salvestusruumi konto**.

3. Sisestage oma salvestusruumi konto nimi.

    See nimi peab olema globaalselt kordumatu; seda kasutatakse URL, mida kasutatakse objektide salvestusruumi konto juurdepääsu osana.  

4. Valige **Ressursihaldur** juurutamise mudeli.

    Astmeline salvestusruumi saate kasutada ainult ressursihaldur salvestusruumi kontod; See on soovitatav juurutamise mudeli uue ressursid. Lisateabe saamiseks lugege teemat [Azure ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md).  

5. Valige ripploendis konto tüüpi **Bloobimälu**.

    See on, kui valite salvestusruumi konto tüüp. Astmeline salvestusruumi pole saadaval üldotstarbeline salvestusruumi; See on ainult bloobimälu salvestusruumi tüüp konto jaoks saadaval.    

    Pange tähele, et see valimisel jõudluse taseme on seatud Standard. Astmeline salvestusruumi pole saadaval Premium jõudluse taseme.

6. Salvestusruumi konto suvand dispersioonanalüüs: **LRS**, **GRS**või **RA-GRS**. Vaikimisi on **RA-GRS**.

    LRS = kohalikult liigsete salvestusruumi; GRS = geograafilise liigne salvestusruumi (2 piirkonnad); RA-GRS on lugemisõigus – geograafilise liigne salvestusruumi (2 regioonid, kus lugemisõigus teine).

    Azure Storage Tiražeerimissuvandid kohta lisateabe saamiseks lugege teemat [Azure Storage kopeerimine](storage-redundancy.md).

7. Valige õige talletusmahu taseme teie vajadustele: **lahedad** või **sooja** **Accessi taseme** määramine. Vaikimisi on **kuum**.

8. Valige tellimus, kus soovite luua uue konto salvestusruumi.

9. Saate määrata uue ressursirühma või valige olemasoleva ressursi rühma. Ressursi rühmade kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md).

10. Valige konto salvestusruumi piirkond.

11. Klõpsake nuppu **Loo** konto salvestusruumi loomiseks.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Talletusmahu taseme portaalis Azure'i bloobimälu salvestusruumi konto muutmine

1. [Azure'i portaali](https://portal.azure.com)sisse logida.

2. Konto salvestusruumi liikumiseks kõik ressursid valige konto salvestusruumi.

3. Klõpsake labale sätete **konfigureerimine** kuvamise ja/või muutke konto konfigureerimine.

4. Valige õige talletusmahu taseme teie vajadustele: **lahedad** või **sooja** **Accessi taseme** määramine.

5. Klõpsake nuppu Salvesta tera ülaosas.

> [AZURE.NOTE] Talletusmahu taseme muutmine võib põhjustada lisatasud. Lugege lisateavet jaotises [hinnakirjad ja arveldamine](storage-blob-storage-tiers.md#pricing-and-billing) .

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Hindamisel ja migreerimine bloobimälu salvestusruumi kontod

Selles jaotises eesmärk aitab kasutajatel teha tagavad sujuva ülemineku abil bloobimälu salvestusruumi kontod. On kaks kasutaja stsenaariumi:

- Teil on üldotstarbeline salvestusruumi konto ja soovite hinnata muudatuse bloobimälu salvestusruumi konto õige talletusmahu taseme.
- Olete otsustanud kasutada bloobimälu salvestusruumi konto või juba on üks ja soovite hinnata, kas kasutate kuum või lahedad talletusmahu taseme.

Mõlemal juhul esimese tellimuse arv business on talletamine ja juurdepääsu talletatud bloobimälu salvestusruumi konto maksumuse ja võrreldakse neid oma praeguse kulud.

### <a name="evaluating-blob-storage-account-tiers"></a>Hindamisel bloobimälu storage konto astme

Hindamiseks talletamine ja juurdepääs bloobimälu salvestusruumi konto talletatavad andmed, peate oma olemasoleva kasutamine mudelit hinnata või ühtlustada oma eeldatav kasutamine mustri. Üldiselt soovite leida.

- Teie salvestusruumi tarbimis - kuidas palju andmeid talletatakse ja kuidas see muudab iga kuu?
- Salvestusruumi Accessi muster - on palju andmeid lugeda ja kirjutada kontoga (sh uute andmete)? Mitu tehingud kasutatakse juurdepääs andmetele ja milliseid tehinguid on need?

#### <a name="monitoring-existing-storage-accounts"></a>Olemasoleva salvestusruumi kontod jälgimine

Olemasoleva salvestusruumi kontode jälgida ja selle andmete kogumine, saab kasutada Azure salvestusruumi Analytics, mis teeb logimine ja annab mõõdikute andmeid salvestusruumi konto.
Salvestusruumi Analytics saate talletada mõõdikute bloobimälu salvestusruumi teenusesse nii üldotstarbeline salvestusruumi kontod, samuti bloobimälu salvestusruumi kontod liidetud tehingu statistika ja võimsus andmeid sisaldavad.
Andmed on salvestatud tuntud tabelid sama salvestusruumi konto.

Rohkem üksikasju, kohta leiate teemast [Talletusruumi Analytics mõõdikute](https://msdn.microsoft.com/library/azure/hh343258.aspx) ja [Salvestusruumi Analytics mõõdikute tabeli skeemi](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] Bloobimälu salvestusruumi kontod seada tabeli teenuse lõpp-punkti talletamine ja juurdepääs mõõdikute andmed konto jaoks.

Salvestusruumi tarbimine bloobimälu salvestusteenus jälgimiseks peate võimsus mõõdikute lubamiseks.
Seda on lubatud, võimsuse andmed on salvestatud igapäevane salvestusruumi konto bloobimälu teenuse ja nimega tabeli kirje, mis on kirjutatud *$MetricsCapacityBlob* tabelisse sama salvestusruumi kontol salvestatud.

Andmete Accessi mustri bloobimälu salvestusteenus jälgimiseks peate kord tunnis tehingu mõõdikute tasemel API lubamiseks.
Seda on lubatud, API kohta tehingud on koondatud iga tunni ja kontaktina tabeli kirje, mis on kirjutatud *$MetricsHourPrimaryTransactionsBlob* tabelisse jooksul salvestusruumi samale kontole. *$MetricsHourSecondaryTransactionsBlob* tabeli kirjete tehingud teisene lõpp-punkti RA-GRS salvestusruumi kontode puhul.

> [AZURE.NOTE] Juhuks, kui teil on üldotstarbeline salvestusruumi konto, kus on talletatud lehe plekid ja virtuaalse masina ketast kõrval plokk ja lisada bloobimälu andmeid, selle protsessi hinnang puudub. See on, kuna teil ei ole võimalik eristada võimsus ja tehingu mõõdikute põhjal bloobimälu ainult blokeerida tüüp ja plekid, mis migreeritud bloobimälu salvestusruumi konto lisamine.

Hea ühtlustamise andmete tarbimine ja Accessi muster saamiseks soovitame valite säilitatakse mõõdikute, mis on mõeldud teie tavalise kasutamise ja ekstrapoleerida.
Üheks võimaluseks on 7 päevaks mõõdikute andmete säilitamise ja koguda andmeid iga nädal, analüüsi kuu lõpu seisuga.
Teine võimalus on viimase 30 päeva mõõdikute andmete säilitamise ja koguda ja andmete analüüsimiseks lõpus 30 päeva jooksul.

Lubamise kohta lisateabe saamiseks kogumiseks ja kuvamiseks mõõdikute andmete kohta leiate teemast, [lubada Azure Storage mõõdikute ja mõõdikute andmete vaatamiseks](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Talletamiseks, juurdepääsuks ja analytics andmete allalaadimine on ka lisandu samamoodi nagu tavalise kasutaja andmeid.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Kasutades kasutus mõõdikute prognoosimiseks kulud

##### <a name="storage-costs"></a>Salvestusruumi kulud

Võimsus mõõdikute tabeli *$MetricsCapacityBlob* rea klahv *"andmed"* Viimane kirje näitab tarbitud kasutajaandmeid mälumaht.
Võimsus mõõdikute tabeli *$MetricsCapacityBlob* rea klahv *"analytics"* Viimane kirje näitab tarbitud analytics logid mälumaht.

See koguvõimsus tarbitud nii kasutajaandmeid ja analytics logid (kui lubatud) saab kasutada siis maksumuse andmete salvestamiseks salvestusruumi konto.
Sama meetodit saate kasutada ka hindamine salvestusruumi kulud plokk ja lisada plekid üldotstarbeline salvestusruumi kontod.

##### <a name="transaction-costs"></a>Tehingu kulud

Summa *"TotalBillableRequests"*, üle kõik kirjed API tehingu mõõdikute tabelis näitab tehinguid selle kindla API koguarv. *nt* *'GetBlob'* tehingute antud perioodi koguarvu saab arvutada kokku arveldatavate taotlusi kõigi kirjete summa rea võti *' kasutaja; GetBlob'*.

Hindamiseks bloobimälu salvestusruumi kontode puhul peate murda tehingud jagada kolme rühma, kuna need on hinnaga teisiti.

- Kirjutage toiminguid nagu *'PutBlob'* *'PutBlock'* *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *"SnapshotBlob"*ja *"CopyBlob"*.
- Kustutage tehingud, näiteks *"DeleteBlob"* ja *"DeleteContainer"*.
- Kõik muud tehingud.

Prognoosimiseks üldotstarbeline salvestusruumi kontode puhul, peate kõigi tehingute sõltumata toiming/API liita.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Andmesidetasude andmed Accessi ja geo-dispersioonanalüüs

Salvestusruumi analytics ei paku andmehulga lugeda ja kirjutada salvestusruumi konto, umbes hinnangute vaadates tehingu mõõdikute tabelis.
Summa *'TotalIngress'* üle kõik kirjed API tehingu mõõdikute tabelis näitab selle kindla API kogusumma sissepääsu andmete maht baitides.
Samuti näitab summa *'TotalEgress'* sealt andmete maht baitides kogusumma.

Andmete Accessi kulud bloobimälu salvestusruumi kontod hindamiseks peate tehingud kaheks murda.

- Salvestusruumi konto andmeid prognoositud saate vaadates *'TotalEgress'* peamiselt *"GetBlob"* ja *'CopyBlob'* toimingute summa.
- Salvestusruumi konto kirjutatud andmed prognoositud saate vaadates summa *'TotalIngress'* peamiselt *'PutBlob'*, *"PutBlock"*, *"CopyBlob"* ja *"AppendBlock"* toimingute jaoks.

Geo-dispersioonanalüüs andmeedastus bloobimälu salvestusruumi kontod kulu saab arvutada ka hinnangu andmete kirjutada korral GRS või RA-GRS salvestusruumi konto abil.

> [AZURE.NOTE] Üksikasjalikumat näide arvutamise kuum või lahedad talletusmahu taseme kasutamise kulude kohta, vaadake pealkirjaga *"mis on kuum ja lahedad juurdepääsu astme ja kuidas määrama use? millist"* KKK [Azure'i salvestusruumi hinnad lehe](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Olemasolevate andmete migreerimine

Bloobimälu salvestusruumi konto on spetsialiseerunud ainult plokk talletamiseks ja lisada plekid. Olemasoleva üldotstarbeline salvestusruumi kontod, kus saate talletada tabelid, järjekorrad, failide ja ketast ning plekid, ei saa teisendada bloobimälu salvestusruumi kontod. Storage astme kasutamiseks peate luua uue bloobimälu salvestusruumi kontod ja olemasolevate andmete migreerimine vastloodud kontod.
Abil saate migreerida olemasolevad andmed bloobimälu salvestusruumi kontod kohapealne salvestusseadmed, kolmanda osapoole pilveteenuse salvestusruumi pakkujate või olemasoleva üldotstarbeline salvestusruumi kontode Azure järgmistest viisidest:

#### <a name="azcopy"></a>AzCopy

AzCopy on mõeldud suure jõudlusega kopeerimise andmete Azure Storage Windows käsurea kasuliku. AzCopy saate oma olemasolevad üldotstarbeline salvestusruumi kontodelt kontole bloobimälu salvestusruumi andmete kopeerimiseks või üles laadida andmeid oma kohapealse salvestusseadmed bloobimälu salvestusruumi kontole.

Täpsema teabe saamiseks vt [AzCopy käsurea kasuliku andmete edastamiseks](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Andmete liikumise teek

Azure'i salvestusruumi liikumine teeki, .net-i põhineb core andmete liikumine raames volitusi AzCopy. Teek on mõeldud suure jõudlusega, usaldusväärseid ja hõlpsasti andmeid edastamise toimingute sarnane AzCopy. See võimaldab teil teha täielik võimalused AzCopy oma rakenduse algupäraselt ilma töötab ja välise eksemplari AzCopy jälgimise eelised.

Leiate üksikasjalikumat teavet leiate [Azure'i salvestusruumi liikumine teegi .net-i jaoks](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST API-ga või kliendi teek

Saate luua kohandatud rakenduse andmete migreerimiseks bloobimälu salvestusruumi kontole, kasutades ühte Azure kliendi teegid või Azure storage teenuste REST API-ga. Azure'i salvestusruumi pakub rikkaliku kliendi teegid mitme keele ja platvormid nagu .NET, Java, C++, Node.JS, PHP, Ruby ja Python. Kliendi teegid pakkuda täpsemalt võimalusi, nt proovi uuesti loogikat, logimine ja paralleelsete üles. Saate arendada ka otse vastu REST API, mille saab kutsuda mis tahes keeleks, mis toodab HTTP/HTTPS päringuid.

Lisateavet leiate teemast [Alustamine Azure'i bloobimälu](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Krüptitud kliendipoolne plekid talletada koos selle bloobimälu talletatud krüptimise seotud metaandmed. On äärmiselt oluline Kopeeri kord tagama bloobimälu metaandmeid, ja eriti krüptimise seotud metaandmed, säilitatakse. Kui kopeerite selle metaandmete ilma plekid, bloobimälu sisu ei ole tagastatava uuesti. Krüptimise seotud metaandmete kohta leiate lisateavet teemast [Azure Storage kliendi küljel krüptimine](storage-client-side-encryption.md).

## <a name="faqs"></a>Korduma kippuvad küsimused

1. **On olemasoleva salvestusruumi kontod on veel saadaval?**

    Jah, olemasoleva salvestusruumi kontod on endiselt saadaval ja hinnad või funktsionaalsus ei muutu.  Need on võimalus valida soovitud talletusmahu taseme ja neil pole tiering võimaluste tulevikus.

2. **Miks ja Millal tuleks kasutusele võtta bloobimälu salvestusruumi kontod?**

    Bloobimälu salvestusruumi kontod spetsialiseeritus plekid talletamiseks ja luba tutvustame bloobimälu-kesksele uusi funktsioone. Edasi, bloobimälu salvestusruumi kontod on soovitatav viis talletamiseks plekid, nagu näiteks hierarhilise salvestusruumi tulevaste võimaluste ja tiering tutvustatakse selle konto tüüp. See aga kuni teid, kui soovite migreerida oma ettevõtte vajaduste järgi.

3. **Olemasoleva salvestusruumi konto saate teisendada bloobimälu salvestusruumi konto?**

    Ei. Bloobimälu salvestusruumi konto on mõnda muud tüüpi salvestusruumi konto ja luua uus ja andmete migreerimiseks, nagu on selgitatud peate.

4. **Objektide saate talletatakse nii storage astme sama konto?**

    *"Juurdepääs taseme"* atribuut, mis näitab talletusmahu taseme väärtuseks on määratud tasemel konto ja kehtib kõigil objektidel selles kontos. Te ei saa seada Accessi taseme atribuut objekti tasemel.

5. **Saate muuta talletusmahu taseme bloobimälu salvestusruumi konto?**

    Jah. On võimalik muuta talletusmahu taseme *"Accessi taseme"* atribuuti seades salvestusruumi konto. Kõigi objektide talletatakse rakendatakse talletusmahu taseme muutmine. Muuda salvestusruumi kiht kuum lahedad kannavad tasusid ajal lahedad kuum-ilt kaasnevad on GB maksumus lugemiseks kõik andmed konto kohta.

6. **Kui sageli saab muuta talletusmahu taseme bloobimälu salvestusruumi konto?**

    Kuigi me jõusta, kui sageli saab muuta talletusmahu taseme piirang, arvestage, mille talletusmahu taseme muutmisel lahedad kuum tekivad märgatavat kulude. Me ei soovita muutuvad sageli talletusmahu taseme.

7. **Lahedad salvestusruumi astme plekid käituvad teisiti, kui need kuum salvestusruumi astme?**

    Saadaolevad salvestusruumi astme plekid on sama nimega plekid latentsust üldotstarbeline salvestusruumi kontod. Lahedad salvestusruumi astme plekid on sarnane latentsus nimega plekid (millisekundites) üldotstarbeline salvestusruumi kontod.

    Lahedad salvestusruumi astme plekid on veidi kättesaadavus teenuse väiksem (SLA) kui talletatud kuum talletusmahu taseme plekid. Lisateavet leiate teemast [SLA Storage](https://azure.microsoft.com/support/legal/sla/storage).

8. **Saate salvestada lehe plekid ja virtuaalse masina ketast bloobimälu salvestusruumi kontodel?**

    Bloobimälu salvestusruumi kontod toetavad ainult plokk ja lisada plekid ja pole lehe plekid. Azure virtuaalse masina ketast on tagatud lehe plekid ja seetõttu ei saa kasutada bloobimälu salvestusruumi kontod virtuaalse masina ketast talletamiseks. Kuid on võimalik hoida varukoopiaid virtuaalse masina ketast Blokeeri plekid bloobimälu salvestusruumi konto.

9. **Peate muuta oma olemasoleva rakendusi kasutada bloobimälu salvestusruumi kontosid?**

    Bloobimälu salvestusruumi kontod on 100% API kooskõlas üldotstarbeline salvestusruumi kontod blokeerida ja lisa plekid. Seni, kuni teie taotlus on abil blokeerida plekid või plekid, lisamine ja kasutate 2014-02-14 versiooni [Salvestusruumi teenuste REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) - või suurem siis rakenduse peaks töötama. Kui kasutate protokolli vanemat versiooni, siis kuvatakse peate värskendama oma rakenduse uus versioon, et sujuv töötamine nii kontotüüpide salvestusruumi kasutada. Üldiselt oleme alati soovitada kasutades uusimat versiooni sõltumata sellest, millist kontot salvestusruumi tüüp, mida kasutada.

10. **Kas tuleb muudatuste kasutusvõimalused?**

    Bloobimälu salvestusruumi kontod on väga sarnased üldotstarbeline salvestusruumi kontod Blokeeri talletamiseks ja lisa plekid toetavad põhijooned Azure Storage, sh kõrge kestvus ja kättesaadavus, skaleeritavus, jõudlus ja turvalisus. Funktsioonid ja piirangud teatud bloobimälu salvestusruumi kontod ja selle storage astme, mis on kutsutud kohal veel peale jääb samaks.

## <a name="next-steps"></a>Järgmised sammud

### <a name="evaluate-blob-storage-accounts"></a>Hinnake bloobimälu salvestusruumi kontod

[Saate teavet bloobimälu salvestusruumi kontod regiooniti](https://azure.microsoft.com/regions/#services)

[Praeguse salvestusruumi kontode kasutamist, võimaldades Azure Storage mõõdikute hindamine](storage-enable-and-view-metrics.md)

[Märkige ruut bloobimälu salvestusruumi hinnad regiooniti](https://azure.microsoft.com/pricing/details/storage/)

[Märkige ruut andmeedastuse hinnad](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Bloobimälu salvestusruumi kontod kasutamise alustamine

[Azure'i bloobimälu kasutamise alustamine](storage-dotnet-how-to-use-blobs.md)

[Andmete teisaldamine ja sealt Azure Storage](storage-moving-data.md)

[AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)

[Sirvige ja uurimine salvestusruumi kontod](http://storageexplorer.com/)
