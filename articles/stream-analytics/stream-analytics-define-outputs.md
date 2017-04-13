<properties
    pageTitle="Voona Analytics väljundid: suvandid Storage analüüsi | Microsoft Azure'i"
    description="Saate teavet suunamise voo Analytics andmete väljundeid võimalusi, sealhulgas Power BI analüüsi tulemuste kohta."
    keywords="andmete teisendus, analüüsi tulemused, andmete talletamise võimalused"
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
    documentationCenter="" 
    authors="jeffstokes72"
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Voona Analytics väljundid: Storage analüüsi suvandid

Kui loome voo Analytics töö, võtke arvesse, kuidas saadud andmeid on tarbitud. Kuidas saate vaadata voo Analytics töö tulemustele ja kuhu salvestate seda?

Mitmesuguseid rakenduse lubamiseks Azure voo Analytics on väljundi talletamine ja analüüsi tulemuste vaatamiseks erinevaid võimalusi. See on lihtne töö väljundi kuvamiseks ja pakub tarbimine ja töö väljundi salvestusruumi paindlikkust andmete hoidmise ja muul eesmärgil. Mis tahes väljundi konfigureeritud töö kuuluma töö käivitatakse ja sündmuste hakkaksid. Näiteks kui kasutate bloobimälu väljund, töö pole loob salvestusruumi konto automaatselt. See peab kasutaja loodud enne ASA töö käivitamist.

## <a name="azure-data-lake-store"></a>Azure'i andmesalve Lake

Voo Analytics toetab [Azure andmesalve Lake](https://azure.microsoft.com/services/data-lake-store/). See salvestusruumi võimaldab teil mis tahes suurus, tüüp ja manustamisest kiirust funktsionaalseid ja uurimusliku analytics andmete talletamiseks. Sel ajal, loomine ja konfigureerimine Lake andmesalve väljundeid toetatakse ainult Azure'i klassikaline portaal. Lisaks peab voo Analytics Lake andmesalve juurdepääs. [Andmete Lake väljundi artiklis](stream-analytics-data-lake-output.md)käsitletakse andmed autoriseerimine ja kuidas registreeruda andmete Lake poe eelvaate (vajadusel).

### <a name="authorize-an-azure-data-lake-store"></a>Azure'i Lake andmesalve autoriseerida.

Kui andmesalv Lake Azure'i haldusportaal väljund on valitud, palutakse teil lubada olemasoleva andmesalve Lake ühenduse.  

![Lubada andmesalve Lake](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Seejärel täitke Lake andmesalve väljundi atribuutide, nagu allpool näha:

![Lubada andmesalve Lake](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Järgmises tabelis on loetletud atribuutide nimesid ja nende kirjeldus, mis on vaja luua Lake andmesalve väljund.

<table>
<tbody>
<tr>
<td><B>ATRIBUUDI NIMI</B></td>
<td><B>KIRJELDUS</B></td>
</tr>
<tr>
<td>Väljundi alias (pseudonüüm)</td>
<td>See on kasutatud päringuid suunamiseks selle andmete Lake Store päringuväljund sõbralik nimi.</td>
</tr>
<tr>
<td>Konto nimi</td>
<td>Kui saadate oma väljundi Lake andmesalv konto nimi. Saate esitatakse ripploendi Lake andmesalve kontod, millele portaali sisse logitud kasutajale on juurdepääs.</td>
</tr>
<tr>
<td>Tee eesliite muster [<I>valikulised</I>]</td>
<td>Faili tee kirjutada oma faile määratud Lake Poe kontoga kasutada. <BR>{date} {aeg}<BR>Näide 1: folder1/logid / {date} / {time}<BR>Näide 2: folder1/logid / {date}</td>
</tr>
<tr>
<td>Kuupäevavormingu [<I>valikulised</I>]</td>
<td>Kui eesliide Path kasutatakse kuupäeva luba, saate valida, kus on korraldatud failide kuupäevavormingu. Näide: PP/AAAA/MM</td>
</tr>
<tr>
<td>Kellaajavorming [<I>valikulised</I>]</td>
<td>Kui aega luba kasutatakse eesliite tee, määrata failide korraldust kellaajavormingu. Praegu on ainus toetatud väärtus HH.</td>
</tr>
<tr>
<td>Sündmuse serialiseerimisvormingus</td>
<td>Andmete serialiseerimisvormingus. JSON-, CSV- ja Avro on toetatud.</td>
</tr>
<tr>
<td>Kodeerimine</td>
<td>Kui CSV- või JSON vormindamiseks on kodeerimine peab olema määratud. UTF-8 on ainus toetatud kodeerimise vorming sel ajal.</td>
</tr>
<tr>
<td>Eraldaja</td>
<td>Rakendatav vaid CSV sariväljaanne. Voo Analytics toetab levinud eraldajaid mitmesuguseid jaotamine CSV-faili andmed. Toetatud väärtused on koma, semikoolon, ruumi, menüüd ja vertikaaljoon.</td>
</tr>
<tr>
<td>Vormindamine</td>
<td>Rakendatav vaid JSON sariväljaanne. Eraldatud kriipsjoon väljund vormindatud lastes iga JSON objekti, eraldades need uue rea. Massiivi määrab vormindatakse väljund massiivi JSON objektid.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Pikendamise Lake andmesalve autoriseerimine

Peate uuesti autentida Lake andmesalve konto, kui selle parool on muutunud, kuna teie töö loodud või viimase autenditud.

![Lubada andmesalve Lake](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>SQL-andmebaas

[Azure'i SQL-andmebaasi](https://azure.microsoft.com/services/sql-database/) saab ka väljundina andmeid, mis on relatsiooniline laadi või rakendusi, mis sõltuvad sisu, mis on majutatud relatsiooniandmebaasist. Voo Analytics töö kirjutada olemasolevasse tabelisse Azure SQL-andmebaasis.  Pange tähele, et tabeli skeemi peab täpselt vastama väljad ja nende tüüpi väljund oma töö. Ka [SQL Azure'i andmebaas](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) saab määrata ka väljundina, mis on SQL-andmebaasi väljundi suvand ka (see on eelvaate funktsiooni) kaudu. Järgmises tabelis on loetletud atribuutide nimesid ja nende loomine SQL-andmebaasi väljundi kirjeldus.

| Atribuudi nimi | Kirjeldus |
|---------------|-------------|
| Väljundi alias (pseudonüüm) | See on kasutatud päringuid päringu väljundi seda andmebaasi sõbralik nimi. |
| Andmebaasi | Kui saadate oma väljundi andmebaasi nimi |
| Serveri nimi | SQL-andmebaasi serveri nimi |
| Kasutajanimi | Mis on Accessi andmebaasi kirjutamiseks kasutajanimi |
| Parooli | Parool andmebaasiga ühenduse loomiseks |
| Tabel | Tabeli nimi, kus väljund on kirjutatud. Tabeli nimi on tõstutundlik ja selle tabeli skeemi peaksid olema samad täpselt väljade ja nende tüüpi oma töö väljundi loodud arvuks. |

> [AZURE.NOTE] Hetkel on toetatud Azure'i SQL-andmebaasi pakkumise töö väljundi voo Analytics. Siiski Azure virtuaalse masina töötab SQL Serveri andmebaas, millele on manustatud ei toetata. See on versioonidega tulevikus muutuda.

## <a name="blob-storage"></a>Bloobimälu

Bloobimälu pakub tulusate ja scalable lahendust salvestamine pilves suurte andmehulkade struktureerimata andmed.  Sissejuhatus Azure'i bloobimälu ja selle kasutamine dokumentatsioonist veebisaidil [plekid kasutamise kohta](../storage/storage-dotnet-how-to-use-blobs.md).

Järgmises tabelis on loetletud atribuutide nimesid ja nende kirjeldused bloobimälu väljundi loomise kohta.

<table>
<tbody>
<tr>
<td>ATRIBUUDI NIMI</td>
<td>KIRJELDUS</td>
</tr>
<tr>
<td>Väljundi alias (pseudonüüm)</td>
<td>See on kasutatud päringuid päringu väljundi selle bloobimälu sõbralik nimi.</td>
</tr>
<tr>
<td>Salvestusruumi konto</td>
<td>Kui saadate oma väljundi salvestusruumi konto nimi.</td>
</tr>
<tr>
<td>Salvestusruumi konto võti</td>
<td>Salajane võti salvestusruumi kontoga seotud.</td>
</tr>
<tr>
<td>Salvestusruumi Container</td>
<td>Ümbriste pakuvad loogiline rühmitamine plekid salvestatud Microsoft Azure'i bloobimälu teenus. Kui laadite üles soovitud bloobimälu bloobimälu teenusega, määrake see bloobimälu ümbris.</td>
</tr>
<tr>
<td>Tee eesliite muster [valikulised]</td>
<td>Faili tee kirjutada oma plekid määratud s.o ümbris, mille jooksul kasutanud.<BR>Path, võite valida üks või mitu eksemplari järgmiste 2 muutujate abil määrata, sagedus, mis on kirjutatud plekid:<BR>{date} {aeg}<BR>Näide 1: cluster1/logid / {date} / {time}<BR>Näide 2: cluster1/logid / {date}</td>
</tr>
<tr>
<td>Kuupäevavormingu [valikulised]</td>
<td>Kui eesliide Path kasutatakse kuupäeva luba, saate valida, kus on korraldatud failide kuupäevavormingu. Näide: PP/AAAA/MM</td>
</tr>
<tr>
<td>Kellaajavorming [valikulised]</td>
<td>Kui aega luba kasutatakse eesliite tee, määrata failide korraldust kellaajavormingu. Praegu on ainus toetatud väärtus HH.</td>
</tr>
<tr>
<td>Sündmuse serialiseerimisvormingus</td>
<td>Andmete serialiseerimisvormingus.  JSON-, CSV- ja Avro on toetatud.</td>
</tr>
<tr>
<td>Kodeerimine</td>
<td>Kui CSV- või JSON vormindamiseks on kodeerimine peab olema määratud. UTF-8 on ainus toetatud kodeerimise vorming sel ajal.</td>
</tr>
<tr>
<td>Eraldaja</td>
<td>Rakendatav vaid CSV sariväljaanne. Voo Analytics toetab levinud eraldajaid mitmesuguseid jaotamine CSV-faili andmed. Toetatud väärtused on koma, semikoolon, ruumi, menüüd ja vertikaaljoon.</td>
</tr>
<tr>
<td>Vormindamine</td>
<td>Rakendatav vaid JSON sariväljaanne. Eraldatud kriipsjoon väljund vormindatud lastes iga JSON objekti, eraldades need uue rea. Massiivi määrab vormindatakse väljund massiivi JSON objektid.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Sündmuse jaoturi

[Sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) on väga paindlik avaldada-Telli sündmuse ingestor. Sündmuste sekundis miljoneid saab koguda.  Ühe mõni sündmus jaoturi väljundina kasutamine kui voo Analytics töö väljund on teine voogesituse töö sisestamine.

On paar parameetrite, mida on vaja konfigureerida sündmuse jaoturi andmevoogu väljund.

| Atribuudi nimi | Kirjeldus |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Väljundi alias (pseudonüüm) | See on kasutatud päringuid päringu väljundi selle sündmuse jaoturi sõbralik nimi. |
| Teenuse siini Namespace | Teenuse siini nimeruum on ümbris kogum, sõnumside üksuste jaoks. Kui olete loonud uue sündmuse jaoturi, olete loonud teenuse siini nimeruum |
| Sündmuse jaoturi | Oma sündmuse jaoturi väljundi nimi |
| Sündmuse jaoturi poliitika nimi | Ühiskasutusega juurdepääsu poliitika, mis saab luua menüü sündmus jaoturi konfigureerimine. Iga ühiskasutusega juurdepääsu poliitika on nimi, määratavad, õigused ja kiirklahvide |
| Sündmuse jaoturi poliitika võti | Ühiskasutusse antud kiirklahv juurdepääsu teenuse siini nimeruumi autentimiseks kasutada |
| Sektsiooni võtme veerg [valikulised] | See veerg sisaldaks sektsiooni võti sündmuse jaoturi väljundi jaoks. |
| Sündmuse serialiseerimisvormingus | Andmete serialiseerimisvormingus.  JSON-, CSV- ja Avro on toetatud. |
| Kodeerimine | CSV-ja JSON UTF-8 on ainus toetatud kodeerimise vorming sel ajal |
| Eraldaja | Rakendatav vaid CSV sariväljaanne. Voo Analytics toetab paljusid levinud eraldajaid jaotamine andmete CSV-vormingus. Toetatud väärtused on koma, semikoolon, ruumi, menüüd ja vertikaaljoon. |
| Vormindamine | Rakendatav vaid JSON tüüp. Eraldatud kriipsjoon väljund vormindatud lastes iga JSON objekti, eraldades need uue rea. Massiivi määrab vormindatakse väljund massiivi JSON objektid. |

## <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com/) saab väljundina, mis on voo Analytics töö rikkaliku visualiseeringu kogemuse analüüsi tulemuste esitada. Seda funktsiooni saab funktsionaalseid armatuurlaudade, aruande genereerimine ja mõõdiku juhitud teatamine.

### <a name="authorize-a-power-bi-account"></a>Lubada Power BI konto

1.  Power BI valimisel väljundina, mis on Azure haldusportaali teil palutakse lubada olemasoleva kasutaja Power BI ja Power BI uue konto loomiseks.  

    ![Lubada Power BI kasutaja](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Uue konto loomine, kui ei, ent olema üks ja seejärel klõpsake nuppu Luba kohe.  Kuva, nagu on esitatud järgmine.  

    ![Azure'i konto Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  Selles etapis tuleb ette töökoha või kooli kontoga lubada Power BI väljundi. Kui teil on pole juba registreerunud Power BI, valige Logi kohe. Töökoha või kooli kontoga, mida kasutada Power BI võib olla erinev kontolt Azure'i tellimus, mis on praegu sisse logides.

### <a name="configure-the-power-bi-output-properties"></a>Power BI väljundi atribuutide konfigureerimine

Kui teil on autenditud Power BI konto, saate konfigureerida oma Power BI väljundi atribuudid. Järgmises tabelis on atribuut nimede loendi ja nende kirjeldused oma Power BI väljundi konfigureerimiseks.

| Atribuudi nimi | Kirjeldus |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Väljundi alias (pseudonüüm) | See on kasutatud päringuid suunamiseks selle PowerBI väljund päringuväljund sõbralik nimi. |
| Rühma tööruumi | Lubada Power BI teiste kasutajatega ühiskasutuse andmete saate valige rühmad Power BI teie kontol või valige "Minu tööruumi", kui te ei soovi rühma kirjutada.  Olemasoleva rühma värskendamine jaoks on vaja uuendada Power BI autentimist. | 
| Andmekomplekti nimi | Sisestage andmekomplekti nimi, mis see on vajalik, et kasutada Power BI väljund |
| Tabeli nimi | Sisestage jaotises Power BI väljundi andmekomplekti tabeli nimi. Praegu Power BI väljundi voo Analytics töökohta võib olla ainult ühest tabelist andmekomplekt |

Walk-through, konfigureerimise Power BI väljundi ja armatuurlaud, lugege artiklit [Azure'i voo analüüsimine ja Power BI](stream-analytics-power-bi-dashboard.md) .

> [AZURE.NOTE] Pole otseselt luua andmekomplekti ja tabel Power BI armatuurlaua. Andmekomplekti ja tabel on automaatselt luua, kui käivitatakse töö ja töö käivitatakse pumpamiseks väljundi Power BI. Pange tähele, et kui töö päringu ei luua mis tahes tulemused, andmekomplekti ja tabeli ei looda. Arvestage, et kui Power BI oli juba andmekomplekti ja tabel, kus üks selle voo Analytics töö sama nimi, olemasolevad andmed kirjutatakse.

### <a name="renew-power-bi-authorization"></a>Power BI autoriseerimine pikendamine

Peate Power BI konto uuesti autentida, kui selle parool on muutunud, kuna teie töö loodud või viimase autenditud. Kui teie peate ka Power BI autoriseerimine järel pikendamiseks Azure Active Directory (AAD) rentniku kohta on konfigureeritud mitme teguriga autentimine (MFA). Selle probleemi sümptom on töö väljund ja "autentimise kasutaja tõrge" logisid toiming:

  ![Power BI Värskenda Turbeloa tõrge](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Selle probleemi lahendamiseks töötava tööpäevad lõpetada ja liikuge oma Power BI väljundi.  Klõpsake linki "Pikenda autoriseerimine" ja taaskäivitage oma töö lõpetanud viimast andmete kaotsimineku vältimiseks.

  ![Power BI pikendada luba](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Tabelimälu

[Azure'i tabelimälu](../storage/storage-introduction.md) pakub väga kättesaadav, oluliselt scalable salvestusruumi, nii, et rakendus saab automaatselt mastaapida kasutaja rahuldamiseks. Tabelimälu on Microsofti NoSQL klahv/atribuut salvestada millise neist saate kasutada Liigendatud andmete koos skeemi vähem piirangud. Azure'i tabelimälu saab püsimine ja tõhusa otsinguga andmete talletamiseks.

Järgmises tabelis on loetletud atribuutide nimesid ja nende loomine tabeli väljundi kirjeldus.

| Atribuudi nimi | Kirjeldus |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Väljundi alias (pseudonüüm) | See on kasutatud päringuid suunamiseks selle tabeli mäluruumi päringuväljund sõbralik nimi. |
| Salvestusruumi konto | Kui saadate oma väljundi salvestusruumi konto nimi. |
| Salvestusruumi konto võti | Kiirklahv, salvestusruumi kontoga seotud. |
| Tabeli nimi | Tabeli nimi. Tabel luuakse saada, kui see pole olemas. |
| Sektsiooni võti | Väljundi veerg, mis sisaldab sektsiooni võti nimi. Sektsiooni klahv () asub sektsiooni konkreetse tabeli, mis moodustab esimene osa ettevõtte primaarvõtme jaoks kordumatut tunnust. See on string väärtus, mis võib olla kuni 1 KB suurust. |
| Rea võti | Väljundi veerg, mis sisaldab rea klahvi nimi. Rea oluline on antud partition üksuse jaoks kordumatut tunnust. Ettevõtte primaarvõtme teine osa moodustab see. Rea oluline on string väärtus, mis võib olla kuni 1 KB suurust. |
| Paketi suurus | Paketi toimingu jaoks kirjete arvu. Tavaliselt vaikimisi piisab enamiku tööde haldamine, vaadake [tabeli paketi käivitamine spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) Lisateavet selle sätte muutmise kohta. |

## <a name="service-bus-queues"></a>Teenus siini järjekorrad

[Teenuse siini järjekorrad](https://msdn.microsoft.com/library/azure/hh367516.aspx) pakkumine on esimene, esimese välja FIFO kohaletoimetamise ühe või mitme konkureerivate tarbijatele. Tavaliselt oodatakse sõnumeid vastu ja töödelda vastuvõtjad ajaline järjestuses, kus need olid varem lisatud kuhjuda ja iga sõnumi on saanud ja vaid ühe sõnumi tarbija töödelda.

Järgmises tabelis on loetletud atribuutide nimesid ja nende kirjeldused järjekorda väljundi loomise kohta.

| Atribuudi nimi | Kirjeldus |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Väljundi alias (pseudonüüm) | See on kasutatud päringuid päringu väljundi selle teenuse siini järjekorda sõbralik nimi. |
| Teenuse siini Namespace | Teenuse siini nimeruum on ümbris kogum, sõnumside üksuste jaoks. |
| Järjekorra nimi | Teenuse siini järjekorra nimi. |
| Järjekorra poliitika nimi | Järjekorda loomisel saate luua ka ühiskasutuses juurdepääsupoliitikaid menüü järjekorra konfigureerimine. Iga ühiskasutusega juurdepääsu poliitika on nimi, määratavad, õigused ja kiirklahvide. |
| Järjekorda poliitika võti | Ühiskasutusse antud kiirklahv juurdepääsu teenuse siini nimeruumi autentimiseks kasutada |
| Sündmuse serialiseerimisvormingus | Andmete serialiseerimisvormingus.  JSON-, CSV- ja Avro on toetatud. |
| Kodeerimine | CSV-ja JSON UTF-8 on ainus toetatud kodeerimise vorming sel ajal |
| Eraldaja | Rakendatav vaid CSV sariväljaanne. Voo Analytics toetab paljusid levinud eraldajaid jaotamine andmete CSV-vormingus. Toetatud väärtused on koma, semikoolon, ruumi, menüüd ja vertikaaljoon. |
| Vormindamine | Rakendatav vaid JSON tüüp. Eraldatud kriipsjoon väljund vormindatud lastes iga JSON objekti, eraldades need uue rea. Massiivi määrab vormindatakse väljund massiivi JSON objektid. |

## <a name="service-bus-topics"></a>Teenuse siini Teemad

Kuigi teenuse siini järjekorrad pakkuda üks-ühele suhtlus meetodi saatja vastuvõtja, [teenuse siini](https://msdn.microsoft.com/library/azure/hh367516.aspx) teemadest üks-mitmele vormi teatise.

Järgmises tabelis on loetletud atribuutide nimesid ja nende loomine tabeli väljundi kirjeldus.

| Atribuudi nimi | Kirjeldus |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Väljundi alias (pseudonüüm) | See on kasutatud päringuid päringu väljundi siini teema sõbralik nimi. |
| Teenuse siini Namespace | Teenuse siini nimeruum on ümbris kogum, sõnumside üksuste jaoks. Kui olete loonud uue sündmuse jaoturi, olete loonud teenuse siini nimeruumi |
| Teema nimi | Teemade on sarnane sündmuse jaoturi ja järjekorrad üksuste sõnumside. Nad on mõeldud kogumise sündmuse voogu paljudest erinevatest seadmetest ja teenused. Kui teema on loodud, see on toodud teatud nimi. Teema saadetud sõnumid ei ole saadaval, kui tellimus on loodud, nii et tagada on üks või mitu tellimust teema alla |
| Teema: poliitika nimi | Teema loomisel saate luua ka ühiskasutuses juurdepääsupoliitikaid menüü teema konfigureerimine. Iga ühiskasutusega juurdepääsu poliitika on nimi, määratavad, õigused ja kiirklahvide |
| Teema: poliitika võti | Ühiskasutusse antud kiirklahv juurdepääsu teenuse siini nimeruumi autentimiseks kasutada |
| Sündmuse serialiseerimisvormingus | Andmete serialiseerimisvormingus.  JSON-, CSV- ja Avro on toetatud. |
| Kodeerimine | Kui CSV- või JSON vormindamiseks on kodeerimine peab olema määratud. UTF-8 on ainus toetatud kodeerimise vorming sel ajal |
| Eraldaja | Rakendatav vaid CSV sariväljaanne. Voo Analytics toetab paljusid levinud eraldajaid jaotamine andmete CSV-vormingus. Toetatud väärtused on koma, semikoolon, ruumi, menüüd ja vertikaaljoon. |

## <a name="documentdb"></a>DocumentDB

[Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) on täielikult hallatav NoSQL dokumendi andmebaasi teenus, mis pakub päringu ja tehingud üle andmete skeemi vaba, hõlpsalt prognoosida ja usaldusväärne jõudlus ja kiire areng.

Järgmises tabelis on loetletud atribuutide nimesid ja nende kirjeldused DocumentDB väljundi loomise kohta.

<table>
<tbody>
<tr>
<td>ATRIBUUDI NIMI</td>
<td>KIRJELDUS</td>
</tr>
<tr>
<td>Konto nimi</td>
<td>DocumentDB konto nimi.  See võib olla ka konto lõpp-punkti.</td>
</tr>
<tr>
<td>Konto võti</td>
<td>Ühiskasutusega kiirklahv DocumentDB konto.</td>
</tr>
<tr>
<td>Andmebaasi</td>
<td>DocumentDB andmebaasi nimi.</td>
</tr>
<tr>
<td>Saidikogumi nimi muster</td>
<td>Saidikogumi nimi mustri kasutamiseks saidikogumid. Saidikogumi nimevormingut saate ehitatud abil valikuline {partition} luba, kus sektsioonid alustama 0.<BR>Nt Funktsiooni järgmiste on kehtiv sisendina:<BR>MyCollection {partition}<BR>MyCollection<BR>Pange tähele, et saidikogumid kuuluma voo Analytics töö käivitatakse ja ei looda automaatselt.</td>
</tr>
<tr>
<td>Sektsiooni võti</td>
<td>Väljundi sündmuste abil saate määrata klahvi jagamine väljundi üle saidikogumid välja nimi.</td>
</tr>
<tr>
<td>Dokumendi ID</td>
<td>Põhinevad väljundi sündmusi, määrata Primaarvõtme lisamine või värskendamine toimingute välja nimi.</td>
</tr>
</tbody>
</table>


## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud
Te olete võetud voo Analytics streaming analytics andmete põhjal asjade Interneti hallatav teenus. Selle teenuse kohta leiate lisateavet järgmistest teemadest.

- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
