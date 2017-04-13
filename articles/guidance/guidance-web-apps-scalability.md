<properties
   pageTitle="Skaleeritav veebirakenduse | Azure'i viide arhitektuur | Microsoft Azure'i"
   description="Täiustame töötab Microsoft Azure veebirakenduse skaleeritavus."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Veebirakenduse skaleeritavus parandamine 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse soovitatav arhitektuur skaleeritavus ja töötab Microsoft Azure'i veebirakenduse jõudluse parandamiseks. Arhitektuur põhineb [Azure viide arhitektuur: lihtsa veebirakenduse][basic-web-app]. Kaalutlused artiklist ja soovituste rakendada ka see arhitektuur.

>[AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: ressursihaldur ja klassikaline. Selles artiklis kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

## <a name="architecture-diagram"></a>Arhitektuur skeem

![[0]][0]

Arhitektuur on järgmised osad:

- **Ressursirühm**. [Ressursirühm] [ resource-group] loogilise ümbris Azure ressursse. 

- ** [Web appi] [ app-service-web-app] ** ja ** [API rakenduse][app-service-api-app]**. Tüüpilised kaasaegne rakendus võivad veebisaidi nii ühe või mitme rahulik web API-d. Veebi-API võib tarbitud brauseri klientide AJAXI kaudu, omakliendi rakenduste või serveripoolne rakendused. Kaalutlused kujundamisel veebis API-d, leiate [juhised API kujundus][api-guidance].    

- **WebJob**. Kasutage [Azure'i WebJobs] [ webjobs] käivitamiseks Pikaajalisi tööülesannete taustal. WebJobs saab käitada ajakavas pidevalt, või käivitamiseks, nt pannes sõnumi järjekorda. Mõne WebJob töötab taustal protsess rakendus App teenuse kontekstis. 

- **Järjekorda**. Siin näidatud arhitektuur, rakenduse järjekorrad tausttoimingud paigutades sõnumi peale on [Azure järjekorda Storage] [ queue-storage] järjekorda. Sõnumi käivitab selle WebJob funktsiooni. 

    Teise võimalusena saate teenuse siini järjekorrad. Võrdlus, leiate [Azure'i järjekorrad ja teenuse siini järjekorrad - compared ja ja][queues-compared].

- **Vahemälu**. [Azure'i Redis vahemälu]pooleldi staatilise andmete talletamiseks[azure-redis].  

- **CDN**. Kasutage [Azure'i sisu kohaletoimetamise võrgu] [ azure-cdn] (CDN) vahemälu avalikult kättesaadavaks sisule alumise latentsus ja kiirem kohaletoimetamise sisu.

- **Andmete talletamine.** Kasutage [Azure'i SQL-andmebaasi] [ sql-db] relatsiooniliste andmete jaoks. Mitte-relatsiooniandmete, on soovitatav NoSQL talletamine, nt Azure'i Tabelimälu või [DocumentDB][documentdb].

- **Azure'i otsida**. Kasutage [Otsingut Azure'i] [ azure-search] lisada otsingufunktsioone, sh otsingusoovitused, udune otsingu ja Keelekohased otsing. Azure'i otsingu kasutatakse tavaliselt koos mõne muu andmesalve, eriti siis, kui esmane andmesalve nõuab range järjepidevuse. Selle stsenaariumi oleks muude andmesalve autoriteetsete andmed ja otsinguregistri pannakse Azure'i otsing. Azure'i otsing saate kasutada ka konsolideerimiseks ühe otsinguregistri: mitme andmete poed.  

- **E-posti/SMS**. Kui teie rakendus peab e-posti või SMS-sõnumite saatmine, kasutage kolmanda osapoole teenusele, näiteks SendGrid või Twilio, mitte otse rakenduse selle funktsiooni koostamise.

## <a name="recommendations"></a>Soovitused

### <a name="app-service-apps"></a>Rakenduse rakendused 

Soovitame luua veebirakenduse ja veebi-API eraldi rakenduse teenuse apps. Selle kujunduse abil saate käivitada neid eraldi rakenduse teenuse lepingutes, mis omakorda võimaldab skaala neid iseseisvalt. Kui te ei pea selle taseme skaleeritavus veebisaidil esmalt saate juurutada rakendused sisse sama lepingu, ja teisaldage need eraldi lepingute hiljem vajadusel. (Basic, Standard, ja lepingute korral on arve VM juhtudel paketis, mitte kaarti rakenduse. Vt [rakenduse teenuse hinnad][app-service-pricing])

Kui kavatsete kasutada *Lihtne tabelite* või *Lihtne API* funktsioonid rakenduse Mobile'i rakendused, luua selleks eraldi rakenduse teenuse rakendus.  Nende funktsioonide toetuvad konkreetse rakenduse raamistik saaksid.

### <a name="webjobs"></a>WebJobs

Kui soovitud WebJob on ressursimahukas, kaaluge juurutamine selle rakenduse teenuse tühja rakenduse sees eraldi rakenduse teenuse lepingut, on WebJob spetsiaalne eksemplaride ette. Vt [tausta töö][webjobs-guidance].  

### <a name="cache"></a>Vahemälu

[Azure'i Redis vahemälu] abil saate parandada jõudlus ja skaleeritavus[ azure-redis] vahemälu mõned andmed. Kaaluge vahemälu Redis:

- Pooleldi staatilise tehingu andmed.

- Seansi olek.

- HTML-i väljundi. See võib olla kasulik rakendusi, mis muudavad keerukate HTML väljund. 

Juhised vahemällu strateegia kujundamise, üksikasjalikuma [vahemällu juhised][caching-guidance].

### <a name="cdn"></a>CDN-ID 

Kasutage [Azure'i CDN] [ azure-cdn] vahemälu staatiliseks sisuks. Peamine kasu CDN-ID on vähendada latentsus kasutajatele, kuna sisu on *Perimeeterserveri* geograafiliselt lähedane kasutaja vahemällu. CDN saate vähendada ka taotluse, laadi, kuna selle liikluse pole, mida kasutavad rakendus.

- Kui rakenduse koosneb enamasti staatiline lehti, kaaluge CDN kogu rakenduse vahemälu. Leiate [Azure'i rakenduse teenuses Azure CDN kasutada][cdn-app-service].

- Muul juhul sellele staatiliseks sisuks, nt piltide, CSS-i ja HTML-vormingus faili, Azure Storage ja kasutage CDN nende failide vahemälu. Vt [integreerida salvestusruumi konto koos CDN][cdn-storage-account].

> [AZURE.NOTE] Azure'i CDN ei saa olla sisu, mis nõuab autentimist.

Täpsemat juhised leiate teemast [juhised selle sisu kohaletoimetamise võrk (CDN)][cdn-guidance]. 

### <a name="storage"></a>Salvestusruumi

Tänapäevane rakenduste protsessi sageli suurte andmehulkade. Selleks, et skaala pilves, on oluline valige õige salvestusruumi tüüp. Siin on mõned soovitused võrdlusalus.  Täpsemat juhised leiate teemast [Hindamisel andmete poe võimaluste Polyglot püsimine lahenduste][polyglot-storage].

Mida soovite salvestada | Näide | Soovitatav salvestusruum
--- | --- | ---
Failide | Pildid, dokumendid, PDF-ide | Azure'i bloobimälu
Võti ja väärtuse paarideks | Kasutaja profiili andmeid, mida kasutaja ID | Azure'i Tabelimälu
Lühisõnumid mõeldud edasiseks töötlemiseks käivitamine | Tellimuse taotlused | Azure'i järjekorda salvestusruumi, teenuse siini järjekorda või teenuse siini teema
Mitte-relatsiooniliste andmete, paindlik skeem, mis nõuab põhilised päringu abil | Tootekataloogi | Näiteks Azure'i DocumentDB, MongoDB või Apache CouchDB
Relatsiooniliste andmete, mis nõuavad rikkalikumat päringu tugi, range skeemi ja/või tugev järjepidevuse | Toote laoseisu | Azure'i SQL-andmebaas

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

### <a name="app-service-app"></a>Rakenduse teenuse rakendus

Kui teie lahendus sisaldab mitut rakendust Service rakendused, kaaluge, rakendades neile eraldi rakenduse teenuse lepingud. Seda moodust võimaldab skaala neid sõltumatult, kuna need töötavad eraldi aknad. Skaala läbi kohta leiate lisateavet teemast [skaleeritavus kaalutlused] [ basic-web-app-scalability] jaotis [lihtsa veebipõhiste][basic-web-app].

Arvesse võtta kasutusele mõne WebJob oma lepingut, samuti nii, et sama eksemplarid, mis toime HTTP päringuid ei tööta tausttoimingud.  

### <a name="sql-database"></a>SQL-andmebaas

Suurendada skaleeritavus SQL-i andmebaasi *sharding* andmebaasi &mdash; , eraldamine andmebaasi horisontaalselt. Sharding võimaldab mastaapimiseks välja andmebaasi horisontaalselt, kasutades [elastne Andmebaasiriistad][sql-elastic]. Võimalikud sharding eelised on järgmised.

- Parem läbilaskevõimet.

- Päringute saate kiiremini käitada üle andmete alamhulga. 

### <a name="azure-search"></a>Azure'i otsing

Azure'i otsingu eemaldatakse üldkulu keerulisi andmeid otsinguid esmane andmesalve ja seda saab skaala käsitlema laadi. [Ressursi skaala]taseme päringu ja indekseerimise töökoormus Azure'i otsingus[azure-search-scaling].

## <a name="security-considerations"></a>Turvalisuse alused

### <a name="cross-origin-resource-sharing-cors"></a>Rist-päritolu ressursside jagamise (CORS)

Kui loote veebisaidi ja veebi-API eraldi apps, veebisaidi ei saa helistada kliendipoolne AJAXI API juhul, kui lubate CORS. 

> [AZURE.NOTE] Brauseri turvalisus takistab veebilehe esitavad lisadomeeni AJAXI päringuid. See piirang nimetatakse sama päritolu poliitika ja takistab pahatahtlik saidi mõnelt muult saidilt sentitive andmete lugemine. CORS on W3C standard, mis võimaldab serveri sama origin poliitika ja luba mõned rist-origin taotlused ehkki teistele.

Rakenduse teenused on tugi CORS, mis tahes rakenduse koodi kirjutamiseks vajamata. Vt [tarbida abil CORS JavaScripti API rakendusele][cors]. Veebisaidi lisamine loendis lubatud päritolu API. 

### <a name="sql-database-encryption"></a>SQL-andmebaasi krüptimine

[Läbipaistva andmete] krüptimise[ sql-encryption] kui teil on vaja andmeid ülejäänud andmebaasi krüptimiseks. See funktsioon teeb nõudmata rakenduse muudatusi reaalajas krüptimine ja dekrüptimine kogu andmebaasi (sh varukoopiate ja tehingulogi faile). Krüptimise lisada mõne latentsus nii, et see on hea tava andmeid, mis peab olema turvaline oma andmebaasi ja ainult andmebaasi krüptimine.  

## <a name="next-steps"></a>Järgmised sammud

- Suurema kättesaadavuse kohta rohkem kui ühe piirkonna rakenduse juurutamine ja [Azure liiklust] Tegumihalduris[ tm] Tõrkesiirde. Lisateavet leiate teemast [Azure viide arhitektuur: kõrge kättesaadavus veebirakenduse][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Veebirakendust Azure täiustatud skaleeritavus."