<properties   
    pageTitle="Migreerimine BizTalki serveri EDI lahenduste BizTalki teenuste tehniline juhend | Microsoft Azure'i"
    description="Migreerimine EDI MABS; Microsoft Azure'i BizTalki teenused"
    services="biztalk-services"
    documentationCenter="na"
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags 
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Migreerimine BizTalki serveri EDI lahenduste BizTalki teenuste: tehniline juhend

Autor: Tim Wieman ja Nitin Mehrotra

Läbivaatajate: Karthik Bharthy

Kirjutada, kasutades: Microsoft Azure BizTalki teenused – veebruaris 2014 versioon.

## <a name="introduction"></a>Sissejuhatus

Elektroonilise andmete Interchange(edi) on üks kõige rohkem levinud tähendab, millised ettevõtted andmevahetus elektrooniliselt, nimetatakse ka ettevõtte äri-või B2B. BizTalk Server on üle kümne, algse BizTalk Sever vabastamist tugi EDI olnud. BizTalki teenustega, Microsoft jätkab EDI lahenduste tugi Microsoft Azure'i platvormil. B2B tehingud enamasti välise organisatsiooni ja seega on lihtsam rakendada, kui see oli rakendatud pilve platvormi. Microsoft Azure'i pakub seda võimalust BizTalki teenuste kaudu.

Kuigi mõned kliendid vaadata BizTalki teenuste "rohelised" platvormi uusi EDI lahendusi, paljud kliendid on praeguse BizTalki serveri EDI lahendusi, mida nad võiksite migreerida Azure. Kuna BizTalki teenuste EDI on architected põhinevad sama võtme üksused nimega BizTalki serveri EDI arhitektuur (börsipäev partnerite üksuste lepingute), on võimalik migreerida BizTalki serveri EDI esemeid BizTalki Services.

Selle dokumendi käsitletakse BizTalki Services seotud migreerimine BizTalki serveri EDI esemeid erinevusi. Selle dokumendi eeldab tundma BizTalki serveri EDI töötlemine ja börsipäev partneri lepingud. Lisateavet BizTalki serveri elektroonilise andmevahetuse [Börsipäev partnerite halduse abil BizTalk Server](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Millist versiooni BizTalki serveri EDI esemeid migreeritud BizTalki teenuseid?

BizTalki serveri EDI mooduli märkimisväärselt täiustatud BizTalki Server 2010, kui see oli remodeled partnerid, profiilid ja lepingud. BizTalki Services kasutab mudelit kaubanduspartnerite ja business osakondades sees nende kaubanduspartneritega korraldamiseks. Migreerimine EDI esemeid BizTalk Server 2010 ja uuemad versioonid BizTalki teenuseid, seetõttu on palju otse edasi protsess. Migreerida EDI esemeid seostatud versioonides BizTalk Server 2010, peate esmalt versioonile BizTalk Server 2010 ja seejärel migreerida oma EDI esemeid BizTalki Services.

## <a name="scenariosmessage-flow"></a>Stsenaariumid/sõnumi kulgemist

Kui BizTalk serveriga, EDI töötlemine BizTalki teenuste ehitatakse üles lahenduse börsipäev partnerite halduse (TPM). TPM lahendus on järgmiste.

- Olukorrale, mis tähistavad ettevõtte B2B toimingu.
- Profiili, mida kujutab kaubanduspartner luuakse osakonnad.
- Börsipäev partneri (või lepingute), mis tähistavad business vaheline kaks partnerite/profiilid.

Järgmisel joonisel on kujutatud sarnasusi, samuti BizTalki serveri EDI lahenduse ja BizTalki teenuste EDI lahenduse erinevused:

![][EDImessageflow]

Peamised erinevused ja sarnasusi on EDI lahenduse meilivoo BizTalk Server ja BizTalki teenused on:

- Täpselt samamoodi nagu BizTalki Server kasutab mõnda EDIReceive müügivõimaluste saada meilisõnumi EDI ja mõne EDISend müügivõimaluste EDI saatmine, BizTalki teenuste kasutab mõnda EDI vastuvõtmine bridge vastuvõtmiseks ja EDI saada bridge EDI sõnumeid saata. BizTalk Server, torujuhtmete on seotud lepingu, kasutades saatmine ja vastuvõtmine pordid. BizTalki teenused, lepingu enda tähistab saatmine ja vastuvõtmine bridge.
- BizTalk Server, pärast EDIReceive müügivõimaluste töötleb EDI teate, sõnumi dumpinguhinnaga SQL Serveri andmebaasiga. Müügivõimaluste EdiSend sõnumi siis salvestab sõnumi SQL serveri andmebaasi, töötleb seda ja siis saadab äripartneri.

    BizTalki teenused, pärast EDI saavad bridge protsessid EDI teate, see marsruudib sõnumi mõni väline protsess. Välise protsess võib töötama Microsoft Azure'i või kohapealse. Välise protsess peaks marsruutimine sõnumi EDI saada sillale; saada silla tõmba potentsiaalselt sõnum. Pärast sõnumi töötluse marsruudib EDI saada silla äripartneri sõnum.

BizTalki teenuste kasutamine tagab teile lihtne konfigureerimine kiiresti luua ja juurutada B2B lepingu olukorrale, ilma konfigureerimise, mis tahes Microsoft Azure'i arvutada eksemplarid (veebis või töötaja rollid), mis tahes Microsoft SQL Azure'i andmebaasid või mis tahes Microsoft Azure'i salvestusruumi kontode vahel. Keerukamaid stsenaariumid nõuab sidumine töövoogude või muu teenuse töötlemine "ümber servade" börsipäev partneri lepingu, mis on enne või pärast börsipäev partneri lepingu EDI bridge töötlemine. Üksikasjalike ilmneda järgmised järjestuse sündmuste meilisõnumi EDI BizTalki teenuste töötlemine.

1. Meilisõnumi EDI saadud börsipäev partneri Fabrikam.  EDI sõnumeid kaubanduspartneritelt BizTalki teenuste toetab transport protokollid, nt FTP, SFTP, AS2 ja HTTP/S.

2. Kauplemise partneri lepingu vastu küljel töötlemine disassembles EDI sõnumi XML-vormingus.  Saate marsruutida siini teenuse lõpp-punktid nt teenuse siini Relay endpoint, teenuse siini teema, teenuse siini järjekorda või BizTalki teenuste silla lahti EDI sõnumi (XML-vormingus).

3. XML-i lahti sõnumid võivad saadud seejärel lõpp-punkti jaoks kohandatud töötlemine.  Nende lõpp-punktid töödelda kohapealse asetamist või Microsoft Azure'i arvutada eksemplar, et edasise protsessi sõnumi Windows töövoo (WF) või Windows Communication Foundation (WCF) teenus, näiteks.

4. "Saada-side töötlemine" äripartneri lepingu siis paneb EDI vormingus sõnumi XML-i ja saadab kaubanduspartner, Contoso.  EDI sõnumite saatmiseks kaubanduspartneritega, toetab BizTalki teenuste sama protokollid, mida kasutati EDI sõnumeid.

Selle dokumendi täpsemaks kontseptuaalne suuniseid sisse mõne muu BizTalki serveri EDI esemeid migreerimisse BizTalki teenuseid.

## <a name="sendreceive-ports-to-trading-partners"></a>Saatmise/vastuvõtu pordid olukorrale, et

BizTalki serveris saate seadistada asukohad kuvatakse ja vastuvõtu pordid kaubanduspartneritega EDI/XML sõnumeid vastu ja häälestamist saada pordid kaubanduspartner EDI/XML sõnumeid saata. Seejärel seote üles kauplemise partneri lepingu abil BizTalki serveri administreerimiskonsool järgmised pordid. BizTalki teenused, kohad, kus saate sõnumeid olukorrale, ja kui saadate sõnumeid olukorrale, on konfigureeritud kauplemise partneri lepingu enda (osana Transport sätted) BizTalki portaalis teenuste osana.  Nii saate tõesti ei ole "saada pordid" ja "kuvatakse asukohad", BizTalki teenused. Lisateabe saamiseks vt [Loomise lepingud](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Torujuhtmete (Bridges)

BizTalki serveri elektrooniline torujuhtmete on sõnumi töötlemine üksused, mis võib sisaldada ka kohandatud loogika konkreetse töötlemise võimalused, kui rakenduse. BizTalki teenuste, tuleks selle kestvus on EDI bridge. Kuid praegu BizTalki teenustes EDI sillad "suletakse".  See tähendab, ei saa lisada oma kohandatud tegevused on EDI bridge. Kohandatud töötlemine, tuleb teha väljaspool EDI silla oma rakenduse enne või pärast sõnumi sisestab silla konfigureeritud börsipäev partneri lepingu osana. EAI sillad on võimalik teha töötlemiseks kohandatud. Kui soovite kohandatud töötlemine, saate enne või pärast sõnumi töötleb EDI silla EAI sillad. Lisateabe saamiseks vaadake, [Kuidas lisada kohandatud koodi sillad](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Saate lisada avalda/tellimise meilivoo kohandatud koodi ja/või teenuse siini sõnumside järjekorrad ja teemade enne kauplemise partneri lepingu saab sõnumi või pärast lepingu töötleb sõnumi ja marsruudib see siini teenuse lõpp-punkti abil.

Selles teemas sõnumi kulgemist mustri teemast **Stsenaariumid/sõnumi kulgemist** .

## <a name="agreements"></a>Lepingud

Kui olete tuttav BizTalki Server 2010 Trading Partner lepingute kasutatakse EDI töötlemise, vaadake väga tuttav BizTalki teenused, partneri lepingud. Enamik lepingu sätted on samad ja kasutada sama terminid. Mõnel juhul lepingu sätted on lihtsam võrreldes samu sätteid BizTalki serveris. Microsoft Azure'i BizTalki Services toetab X12, EDIFACT ja AS2 transport.

Microsoft Azure'i BizTalki Services pakub **TPM andmete migreerimise** tööriista kauplemise partnerid ja lepingute migreerimiseks serveri börsipäev partneri mooduli BizTalki Services portaal. Migreerimistööriista TPM andmed on saadaval osana paketi tööriistu, mille saab alla laadida [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Pakett sisaldab ka seletusfail, mis sisaldab juhiseid, kuidas kasutada tööriista ja põhiteabe tõrkeotsingu tööriista.

## <a name="schemas"></a>Skeemid

BizTalki Services pakub EDI skeemid, mida saate kasutada BizTalki Services lahenduste.  Lisaks BizTalki serveri EDI skeeme saate kasutada ka BizTalki teenustega Kuna EDI skeemi juurkausta sõlm on sama kogu BizTalki serveri samuti BizTalki teenuseid. Seetõttu saab otse teie BizTalki serveri EDI skeemid ja kasutada neid EDI lahendusi, mis teil tekib BizTalki teenuste kaudu. Saate alla laadida ka skeemide [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Kaartide (muutub)

Kaartide BizTalki serveris nimetatakse Teisendusteta BizTalki teenused. Migreerimine BizTalki serveri BizTalki Services kaartide võib olla keerukamate tööülesannete saavutamiseks (olenevalt kaardi keerukus). BizTalki plaanuri erineb vastenduse tööriista BizTalki teenuste jaoks kasutada. Ehkki on plaanuri enamasti näeb ühesugune välja, aluseks oleva kaardi vorming erineb. Samuti on erinevad functoids (nimega **Kaardi toimingute** BizTalki teenused) kasutajatele saadaval.  Ei saa otse kasutada kaardil BizTalki BizTalki teenused. Ka kõik BizTalki serveris saadaval functoids on saadaval kaardi toimingute BizTalki teenused.

### <a name="new-transform-operations"></a>Uue Andmeteisendustoiminguid

Kuigi kaardi andmeteisendustoiminguid saadaval loendit võib tunduda erinevad BizTalki serveri plaanuri, BizTalki teenuste muudab on uued võimalused, aga mitte samu toiminguid. Näiteks BizTalki teenuste muudab on **Loendi toimingud** saadaval. See ei saa BizTalki plaanuri.  Need **Tegevused** võimaldavad teil luua ja kasutada "Loendi", kui loend on kogumi üksuste (tuntud ka kui "read") ja kui iga üksuse võib olla mitu liiget (tuntud ka kui "veeru").  Saate loendit sortida, valimiseks hoidke üksuste vastavalt tingimusele, jne.

Teine näide uued funktsioonid rakenduses BizTalki teenuste muudab on **Tsükkel toimingud**.  On raske BizTalki serveri plaanuri pesastatud silmuseid loomiseks.  Seega lisatakse tsükkel kaardi toimingute jaoks BizTalki teenuste muudab.

Teine näide on veel **If-Than-Else** avaldis kaardi toiming.  If-Than-else toimingu tehes oli võimalik BizTalki plaanuri, kuid see on vajalik mitme functoids näivalt lihtsa ülesande.

### <a name="migrating-biztalk-server-maps"></a>Migreerimine BizTalki Server kaardid

Microsoft Azure'i BizTalki Services pakub tööriista migreerida BizTalki serveri kaardid BizTalki teenuste teisendusteta. **BTMMigrationTool** on saadaval koos [BizTalki teenuste SDK allalaadimine](http://go.microsoft.com/fwlink/p/?LinkId=235057) **Tööriistad** paketi osana. Tööriista kohta leiate lisateavet teemast [BizTalki teenuste muuta kaardil BizTalki teisendada](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Saate vaadata ka Sandro Pereira BizTalki MVP, valimil üleviimiseks [BizTalki teenuste teisendusteta BizTalki serveri kaardid](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orkestreering

Kui teil on vaja migreerida BizTalki serveri korraldamise Microsoft Azure töötlemine on orkestreering oleks vaja kirjutatakse üle, kuna Microsoft Azure'i ei toeta orkestreering BizTalki serveri käivitamine.  Kas kirjutada Windows töövoo Foundation 4.0 (WF4) teenuse korraldamise funktsiooni.  Täielik ümberkirjutamine oleks nagu praegu pole migratsiooni BizTalki serveri orkestreering WF4. Siin on mõned ressursid Windows töövoo jaoks.

- [*Kuidas integreerida WCF-i töövoo teenuse teenuse siini järjekorrad ja teemadel*](https://msdn.microsoft.com/library/azure/hh709041.aspx) Paolo Salvatori järgi. 

- [ *Koosteüksuse rakendused Windows töövoo Foundationi ja Azure* seansi](http://go.microsoft.com/fwlink/p/?LinkId=237314) koostamine 2011 konverentsi.

- [*Windows töövoo Foundation Arenduskeskus*](http://go.microsoft.com/fwlink/p/?LinkId=237315) MSDN-is.

- MSDN-i [*Windows töövoo Foundation 4 (WF4) dokumentatsiooni*](https://msdn.microsoft.com/library/dd489441.aspx) .

## <a name="other-considerations"></a>Muud kaalutlused

Järgnevalt on BizTalki teenuste kasutamisel tuleb teil teha mõned asjaolu.

### <a name="fallback-agreements"></a>Taandepäringud lepingud

BizTalki serveri EDI töötlemine on mõiste "Fall lepingud".  BizTalki teenuste ei **ei** on Fall lepingu mõistet siiani.  Teemadest BizTalki dokumentatsiooni [Lepingute rolli EDI töötlemise](http://go.microsoft.com/fwlink/p/?LinkId=237317) ja [konfigureerida üld- või Fall lepingu atribuutide](https://msdn.microsoft.com/library/bb245981.aspx) teavet BizTalki serveris Fall lepingute kasutamise kohta.

### <a name="routing-to-multiple-destinations"></a>Mitme sihtkohtade marsruutimiseks

BizTalki teenuste sillad, selle praeguses olekus ei toeta marsruutimise sõnumid mitme sihtkohad on avalda-Telli mudel. Selle asemel võib marsruutimine sõnumeid BizTalki teenuste silla teenuse siini teema, siis on mitu tellimust saada sõnum veebisaidil mitu lõpp-punkti.

## <a name="conclusion"></a>Kokkuvõte

Microsoft Azure'i BizTalki teenused on uuendatud tavaline vahekokkuvõtteid lisada rohkem funktsioone ja võimalusi. Iga värskendusega vaatame edasi toetamiseks suurem funktsioonid hõlbustada, kasutades BizTalki teenuste ja Azure tehnoloogiad lõpuni lahenduste loomine.

## <a name="see-also"></a>Vt ka

[Ettevõtte rakenduste Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
