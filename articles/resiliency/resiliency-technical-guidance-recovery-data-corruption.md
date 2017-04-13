<properties
   pageTitle="Paindlikkust tehnilised juhendid: andmed on rikutud või juhuslikku kustutamist taastamisel | Microsoft Azure'i"
   description="Artikkel andmete rikkumist andmete või ekslike andmete kustutamine, et ja olles, mis on väga saadaval, vea salliv rakenduste kujundamine kui ka kavandamise avariitaastet taastamine mõistmine"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Azure'i paindlikkust tehnilised juhendid: andmed on rikutud või juhuslikku kustutamist taastamine

Robustne järjepidevuse äriplaani osa, millel lepingu kui andmete saab rikutud või kogemata kustutanud. Järgnevalt on teavet taastamine pärast seda, kui andmed on rikutud või kogemata kustutanud, rakenduse tõrked või tehtemärk tõrke tõttu.

##<a name="virtual-machines"></a>Virtuaalmasinates

Rakenduse tõrked või juhuslikku kustutamist kaitsta oma Azure'i Virtuaalmasinates (mõnikord nimetatakse taristu-kui-a-service VMs), kasutage [Azure varukoopia](https://azure.microsoft.com/services/backup/). Azure'i varukoopiad võimaldab luua varukoopiaid, mis vastavad mitme VM ketast üle. Lisaks saab paljundada varundamise Vault piirkondade esitada taastamise piirkond kadu.

##<a name="storage"></a>Salvestusruumi

Pange tähele, et Azure Storage pakub andmete paindlikkust kaudu automatiseeritud koopiad, ei takista see oma rakenduse koodi (või arendajate/kasutajad) kaudu andmete rikkumist soovimatute või ekslike kustutamise, värskendamine ja jne. Säilitades andmete töökindluse ees rakendus või kasutaja tõrge nõuab rohkem täiustatud viise, nt teisene talletuskoht on auditilogi abil andmete kopeerimine. Arendajad saate ära bloobimälu [hetktõmmise võimalus](https://msdn.microsoft.com/library/azure/ee691971.aspx), mis on kirjutuskaitstud punkti /-kellaaja hetktõmmiseid bloobimälu sisu saate luua. Seda saab kasutada andmete-töökindluse lahenduse alusena Azure Storage plekid.

###<a name="blob-and-table-storage-backup"></a>Bloobimälu ja tabeli salvestusruumi varundamine

Kuigi plekid ja tabelid on väga püsiv, moodustavad need alati hetkeseisu andmed. Soovimatu muuta või kustutada andmete taastamine võib olla vaja andmete taastamine varasemast versioonist. See saavutamiseks esitatud Azure talletada ja säilitada punkti /-kellaaja eksemplaride võimaluste ära.

Azure'i plekid, saate teha punkti /-kellaaja varukoopiate [bloobimälu pildi funktsioon](https://msdn.microsoft.com/library/ee691971.aspx)abil. Iga hetktõmmise jaoks on ainult eest talletamist nõutav talletamiseks sees on bloobimälu erinevused, kuna viimane hetktõmmise olek. Tõmmised sõltuvad olemasolu algse bloobimälu, need põhinevad, et koopia toiming muus bloobimälu või isegi muu salvestusruumi konto on soovitatav. See tagab, et varundatud andmed on õigesti kaitstud juhuslikku kustutamist. Azure'i tabelid, saate punkti õigel ajal koopiate tegemise või mõne muu tabeli Azure plekid. Lisateavet üksikasjalikud juhised ja näited läbimiseks Rakendusetaseme varukoopiate tabelite ja plekid leiate siit:

  * [Tabelite suhtes rakenduse tõrked kaitsmine](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Kaitsta oma plekid vastu rakenduse tõrked](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Andmebaasi

See on saadaval Azure'i SQL-andmebaasi [järjepidevuse](../sql-database/sql-database-business-continuity.md) (varundada, taastada) mitu võimalust. Andmebaasid saate kopeerida [Andmebaasi kopeerimine](../sql-database/sql-database-copy.md) funktsiooni abil või [eksportimine](../sql-database/sql-database-export.md) ja [importimine](https://msdn.microsoft.com/library/hh710052.aspx) SQL serveri bacpac faili. Andmebaasi kopeerimine pakub ülekandega lapsed on bacpac (impordi/ekspordi service) kaudu ei saa. Mõlema suvandi Käivita järjekorda-põhiste teenuste andmekeskuse ja nad ei paku aeg-lõpetamise SLA praegu.

>[AZURE.NOTE]Andmebaasi käske Kopeeri ja impordi/ekspordi suvandid asetage laadi olulisel määral allika andmebaasi. Ta saab käivitamine ressursi väide või pidurdamise sündmused.

###<a name="sql-database-backup"></a>SQL-i andmebaasi varundamine

Punkti /-kellaaja varukoopiate Microsoft Azure'i SQL-i andmebaasi saavutamine [SQL Azure'i andmebaasi kopeerimise](../sql-database/sql-database-copy.md)abil. Selle käsu abil saate luua andmebaasi ülekandega ühtsete koopia sama loogilise andmebaasiserveri või muu server. Mõlemal juhul andmebaasi koopia on täielikult toimiv ja täiesti sõltumatu lähteandmebaasi. Iga koopia loomist tähistab punkti /-kellaaja taastamise suvand. Saate taastada andmebaasi riigi täielikult allika andmebaasi nimi uue andmebaasi ümber nimetada. Teise võimalusena saate taastada teatud andmete alamkogumit, uue andmebaasi Transact-SQL-i päringute abil. SQL-andmebaasi kohta saate lisateavet teemast [Ülevaade ettevõtte järjepidevus Azure'i SQL-andmebaasi](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>SQL serveri Virtuaalmasinates varundamise kohta

SQL Server Azure'i infrastruktuuri teenuse virtuaalmasinates, (sageli nimetatakse IaaS ja IaaS VMs) kasutada, on kaks võimalust: traditsiooniline varukoopiate ja Logi saatmine. Traditsiooniline varukoopiate abil võimaldab taastamiseks teatud punktis ajas, kuid taastamise protsess on aeglane. Traditsiooniline taastamise jaoks on vaja alustades on esialgse täieliku varukoopia ja seejärel rakendada mis tahes varukoopiate, pärast mida. Teine võimalus on konfigureerida Logi saatmine seansi varundamine, taastamine (nt kahe tunni) võrra edasi. See pakub akna taastamine esmase vead.

##<a name="other-azure-platform-services"></a>Azure'i platvormi muude teenustega

Teatud Azure'i platvormi teenuste saate talletada teavet kasutaja kontrollitud salvestusruumi konto või Azure'i SQL-andmebaasi. Kui konto või salvestusruumi ressursi kustutada või rikutud, see raske tõrgete teenuse. Sellisel juhul on oluline hoida varukoopiate, mis võimaldab teil nende ressursside uuesti luua, kui need kustutada või rikutud.

Azure veebisaitide ja Azure Mobile teenused, peate varundamine ja seotud andmebaaside haldamine. Azure Media teenusele ja Virtuaalmasinates, peate hoidma Azure Storage seotud konto ja kõiki selle konto. Näiteks jaoks Virtuaalmasinates, peate varundamine ja hallata VM ketast Azure'i bloobimälu sisse.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Andmed on rikutud või juhuslikku kustutamist kontroll-loendid

##<a name="virtual-machines-checklist"></a>Virtuaalmasinates kontroll-loend

  1. Vaadake üle Virtuaalmasinates jaotises selle dokumendi.
  2. Varundamine ja hallata VM ketast Azure varukoopia (või oma varukoopia süsteemi abil Azure'i bloobimälu ja VHD pilte).

##<a name="storage-checklist"></a>Salvestusruumi kontroll-loend

  1. Vaadake üle selle dokumendi jaotise salvestusruumi.
  2. Regulaarselt varundada kriitilised salvestusruumi ressursid.
  3. Kaaluge pildi funktsioon plekid.

##<a name="database-checklist"></a>Andmebaasi kontroll-loend

  1. Vaadake üle selle dokumendi jaotist andmebaasi.
  2. Punkti õigel ajal varundamiseks, kasutades käsku andmebaas Kopeeri.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server Virtuaalmasinates varundamise kontroll-loend

  1. Vaadake üle SQL Server selle dokumendi jaotist Virtuaalmasinates varukoopia.
  2. Kasutage traditsiooniline varundamine ja taastamine meetodite abil.
  3. Hilinenud Logi saatmine seansi loomine.

##<a name="web-apps-checklist"></a>Web Appsi kontroll-loend

  1. Varundamine ja hallata andmebaasi, kui mõni.

##<a name="media-services-checklist"></a>Media Servicesi kontroll-loend

  1. Varundada, ja salvestusruumi seotud ressursid säilitada.

##<a name="more-information"></a>Lisateave

Azure'i varundus ja taaste funktsioonide kohta leiate lisateavet teemast [salvestusruumi, varundamine ja taastamine stsenaariumid](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Järgmised sammud

See artikkel on osa sarjast värskendustest [Azure paindlikkust tehnilised juhendid](./resiliency-technical-guidance.md). Kui otsite rohkem paindlikkust, katastroofiabi ja kõrge-saadavus ressursse, leiate Azure'i paindlikkust tehnilised juhendid [Täiendavad ressursid](./resiliency-technical-guidance.md#additional-resources).