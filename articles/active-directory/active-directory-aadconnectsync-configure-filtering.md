<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: konfigureerimine filtreerimine | Microsoft Azure'i"
    description="Selgitab, kuidas konfigureerida filtreerimine Azure'i AD-ühenduse sünkroonimine."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure'i AD-ühendus sünkroonimine: konfigureerimine filtreerimine
Filtreerimine, saate määrata, millised objektid peaks kuvatama Azure AD oma kohapealse kataloogi. Vaikimisi konfiguratsiooni võtab kõigi objektide kõik domeenide konfigureeritud metsades. Üldiselt, see on soovitatav konfiguratsioon. Office 365 teenustest, nt Exchange Online ja Skype'i ärirakenduse abil lõppkasutajad kasu täieliku globaalsest aadressiloendist nii, et nad saaksid meilisõnumeid saata ja helistada kõigile. Vaikimisi konfiguratsiooni, nad saavad nad kohapealse rakendamine Exchange'i või Lynci sama kogemus.

Mõnel juhul on nõutav vaikimisi konfiguratsiooni muudatusi teha. Siin on mõned näited:

- Mida plaanite kasutada [mitme Azure'i AD-directory topoloogia](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory). Siis, kui teil on vaja määrata, millisele objektile sünkroonitakse konkreetsete filtri rakendamine Azure AD kataloogi.
- Käivitate pilootprojekti Azure'i või Office 365 ja soovite kasutajate alamhulga ainult Azure AD. Väike katseprojekti, ei ole oluline, et näidata funktsioonid täieliku globaalsest aadressiloendist.
- Teil on palju teenus kontod ja te ei soovi Azure AD-isiklik kontodelt.
- Nõuetele vastavuse tagamiseks mis tahes kasutaja kontod kohapealse ei kustutata. Kui keelate ainult need. Kuid Azure AD soovite ainult aktiivse kontod kohal olla.

Selles artiklis antakse ülevaade konfigureerimine erinevad filtreerimise viisid.

> [AZURE.IMPORTANT]Microsoft ei toeta muutmine või väljaspool neid toiminguid ametlikult dokumenteerida Azure'i AD-ühenduse sünkroonimine toimimist. Nende toimingute võib kaasa tuua vastuolulised või toetamata olekus Azure'i AD-ühenduse sünkroonimine ning seetõttu Microsoft ei paku tehnilist tuge sellise juurutuste.

## <a name="basics-and-important-notes"></a>Põhitõed ja olulised märkmed
Azure'i AD-ühenduse sünkroonimise, saate lubada filtreerimine igal ajal. Kui alustate vaikimisi konfiguratsiooni kataloogi sünkroonimise ja konfigureerimist filtreerimine, sünkroonitakse Azure AD enam objekte, mis on välja filtreeritud. Selle tulemusena muutus kustutatakse kõik Azure AD objektid, mis on varem sünkroonitud, kuid seejärel on filtreeritud Azure AD.

Enne alustamist, et tegemist muudab filtreerimise, veenduge, et saate [keelata Ajastatud ülesanne](#disable-scheduled-task) nii, et te eksikombel eksportimine muudatused, mida pole veel kinnitanud õige.

Kuna filtreerimine saate eemaldada korraga palju objekte, soovite veenduge, et oma uue filtrid on õige enne alustamist, et muudatused eksportimine Azure AD. Kui olete konfigureerimine juhiseid lõpule viinud, on tungivalt soovitatav, [kontrollimise juhiseid](#apply-and-verify-changes) järgida enne eksportida ja Azure AD muudatusi teha.

Teie kaitsmiseks juhusliku arvu objektide kustutamine, [vältida juhuslikku kustutab](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) funktsioon on vaikimisi sisse. Kui kustutate palju objekte tõttu filtreerimine (vaikimisi 500), peate järgige selle artikli lubamiseks kustutab läbida Azure AD.

Kui kasutate ehitada enne November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) filtri konfiguratsiooni muudatuste tegemine ja kasutate parooli sünkroonimise, siis peate kõik paroolid täieliku sünkroonimise käivitamiseks, kui olete lõpetanud konfiguratsiooni. Juhised selle kohta, kuidas parooli täieliku sünkroonimise käivitamiseks vt [käivitamine täieliku sünkroonimise kõik paroolid](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Kui olete 1.0.9125 või uuem versioon, siis tavaline **täieliku sünkroonimise** toimingu ka arvutab kui sünkroonitakse paroolid ja selle lisatööd pole enam vaja.

Kui **kasutaja** objektid olid kogemata kustutanud Azure AD filtreerimise tõrke tõttu, saate uuesti luua Azure AD kasutaja objektid, eemaldades oma filtreerimise konfiguratsioone ja seejärel uuesti sünkroonida oma kataloogide. See toiming taastab prügikastist kasutajate Azure AD. Siiski ei saa taastada teiste objekti tüübid. Näiteks kui kustutate kogemata turberühma ja seda kasutati ACL ressursi, rühma ja selle ACL ei saa taastada.

Azure'i AD-ühendus kustutab ainult kiirklõpsulipuna on üks kord hõlmab objektid. Kui Azure AD objekte, mis on loodud mõne muu sünkroonimine engine ja need objektid pole ulatus, lisades filtreerimine eemaldage need. Näiteks kui alustate koos Dirsynci server ja selle loonud täielik koopia kogu kataloogi Azure AD ja uue Azure'i AD-ühenduse sünkroonimine serveri installimist paralleelselt filtreerimine lubatud algusest, eemaldage eest objektide loodud DirSync.

Filtreerimise konfiguratsiooni alles installimisel või uuemat versiooni Azure'i AD-ühenduse versioonile. See on alati parim kinnitamaks, et konfiguratsiooni pole kogemata muudetud pärast täiendamist uuemale versioonile enne käivitamist esimese sünkroonimise tsükli.

Kui teil on rohkem kui üks mets, siis filtreerimine kuvatakse selles teemas kirjeldatud peab rakenduma iga mets (kui soovite sama konfiguratsiooni nende kõigi jaoks).

### <a name="disable-scheduled-task"></a>Keela Ajastatud ülesanne
Iga 30 minuti järel, mis käivitab domeenikirjeid sisseehitatud ajasti toimige järgmiselt.

1. Avage soovitud PowerShelli Küsi.
2. Käivitage `Set-ADSyncScheduler -SyncCycleEnabled $False` ajasti abil.
3. Tehke soovitud muudatused, nagu selles teemas dokumenteerida.
4. Käivitage `Set-ADSyncScheduler -SyncCycleEnabled $True` ajasti uuesti lubada.

**Kui kasutate Azure'i AD-ühenduse koostamine enne 1.1.105.0**  
Ajastatud ülesanne, mis käivitab domeenikirjeid iga 3 tunni keelamiseks tehke järgmist.

1. Käivitage **Toiminguajasti** menüü start kaudu.
2. Otse leiate jaotisest **Toiminguajasti teek**, nimega **Azure AD sünkroonimine ajasti**, paremklõpsake seda ja valige käsk **Keela**tööülesanne.  
![Toiminguajasti](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Nüüd saate muuta konfiguratsiooni ja sync engine **sünkroonimise teenuse halduri** konsooli käsitsi käivitamine.

Kui filtreerimise muudatused on lõpule jõudnud, ärge unustage tulevad tagasi ja **lubada** tööülesande uuesti.

## <a name="filtering-options"></a>Filtreerimise suvandid
Filtreerimise konfigureerimine järgmist tüüpi saab rakendada Directory sünkroonimistööriist:

- [**Põhjal**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): ühe rühma alusel filtreerimise saab ainult konfigureerida algne install installimise viisardi abil. See edasi ei kuulu selles teemas.

- [**Domeeni põhinev**](#domain-based-filtering): see valik võimaldab teil valida, millised domeene, mida sünkroonida Azure AD. Lisaks võimaldab lisada ja eemaldada domeenide sünkroonimine engine konfiguratsiooni kui muudate oma kohapealse taristu pärast installimist Azure'i AD-ühenduse sünkroonimine.

- [**Organisatsioonisisene ühiku põhise**](#organizational-unitbased-filtering): selle filtreerimise suvandi abil saate valida, mis organisatsiooniüksused sünkroonida Azure AD. See suvand on valitud organisatsiooniüksused objekti kõigi jaoks.

- [**Atribuudi põhise**](#attribute-based-filtering): see suvand, mis võimaldab filtreerida objektid, objektid atribuudi väärtuste põhjal. Saate määrata ka erinevate filtrite muu objekti tüübid.

Saate korraga mitme filtreerimise suvandid. Näiteks saate OU-põhine filtreerimine ainult kaasatavate objektide ühe OU ja samal ajal atribuut põhinev filtreerimist objektide täpsemaks filtreerimiseks. Kui kasutate mitut filtreerimise meetodit, kasutage filtrid loogilise ja filtrid vahel.

## <a name="domain-based-filtering"></a>Domeeni põhinev filtreerimine
Selles jaotises pakub filtrisse domeeni konfigureerimine juhiseid. Kui teil on lisatud või eemaldatud domeenide oma mets, kui olete installinud Azure'i AD-ühenduse, on teil ka värskendada filtreerimise konfigureerimine.

Eelistatud võimalus muuta domeeni põhinev filtreerimine on installimise viisard ja muutke [domeeni ja filtreerimise organisatsiooniüksused](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Installimise viisard on ülesandeid, mis on selles teemas dokumenteerida automatiseerimine.

Kui mingil põhjusel ei saa installi viisardi käivitamiseks, tuleks ainult tehke järgmist.

Domeeni põhinev filtreerimise konfiguratsiooni koosneb järgmistest toimingutest:

- [Valige domeenid,](#select-domains-to-be-synchronized) mis tuleks lisada sünkroonimine.
- Iga lisatud ja eemaldatud domeeni, saate reguleerida [käivitada profiilid](#update-run-profiles).
- [Rakenda ja muudatuste kontrollimiseks](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Valige domeenid sünkroonimist
**Domeeni filtri seadmiseks tehke järgmist.**

1. Logige sisse serveris, kus töötab Azure'i AD-ühenduse sünkroonimine kontoga, mis on **ADSyncAdmins** turvalisuse rühma liige.
2. Menüü start kaudu **Sünkroonimise teenuse** käivitamine.
3. Valige **konnektorid** ja **konnektorid** lisanud, valige konnektor **Active Directory domeeniteenused**tüüp. **Toimingud**, valige **Atribuudid**.  
![Konnektor atribuudid](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klõpsake nuppu **Konfigureeri sektsiooni kausta**.
5. Valige loendis **Valige sektsiooni kausta** ja valimise domeenide vastavalt vajadusele. Veenduge, et ainult sektsioonid, mida soovite sünkroonida on valitud.  
![Sektsioonid](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Kui olete muutnud oma kohapealse AD taristu ja lisatud või eemaldatud domeenides mets, seejärel klõpsake nuppu **Värskenda** värskendatud loendi saamiseks. Kui proovite värskendada, küsitakse mandaati. Pakkuda oma kohapealse Active Directory mis tahes identimisteabe lugemisõigus. See ei pea olema kasutaja, mis on juba eelnevalt täidetud dialoogiboksis.  
![Vajalik värskendamine](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Kui olete lõpetanud, sulgege dialoogiboks **Atribuudid** nuppu **OK**. Kui olete eemaldanud domeenide mets, sõnumi hüpikakna räägivad domeen on eemaldatud ja selle konfiguratsiooni on puhastada.
7. Jätkake reguleerimiseks [käivitage profiilid](#update-run-profiles).

### <a name="update-run-profiles"></a>Käivita profiili värskendamine
Kui olete oma domeeni filter värskendanud, peate ka Käivita profiili värskendamine.

1. **Konnektorid** lisanud, veenduge, et olete muutnud eelmises etapis konnektor on valitud. Valige **toimingud**, **Käivitage profiilid konfigureerimine**.  
![Konnektor käivitada profiilid](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

Soovite reguleerida järgmisi profiile:

- Täielik importimine
- Täieliku sünkroonimise
- Delta importimine
- Delta sünkroonimine
- Eksportimine

Iga viis profiilid, järgige järgmisi juhiseid iga domeeni **lisanud** :

1. Käivita profiil ja klõpsake nuppu **Uus toiming**.
2. Lehel **Toiming konfigureerimine** **Tüüp** ripploendis valige toimingu tüüp sama nimega konfigureerite profiil. Klõpsake nuppu **edasi**.  
![Konnektor käivitada profiilid](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Valige lehel **Konnektori konfiguratsiooni** **sektsiooni** rippmenüüst see domeen, mille olete lisanud oma domeeni filtri nimi.  
![Konnektor käivitada profiilid](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. **Käivitage profiili konfigureerimine** dialoogiboksi sulgemiseks klõpsake nuppu **valmis**.

Iga viis profiilid, järgige järgmisi juhiseid iga domeeni **eemaldada** :

1. Valige Käivita profiil.
2. Kui **sektsiooni** atribuudi **väärtus** on GUID, valige Käivita ja klõpsake **Kustutamine toimingut**.  
![Konnektor käivitada profiilid](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Tulemus peaks olema iga domeeni, mida soovite sünkroonida peaks olema lisatud sammuna iga run profiili.

Sulgege dialoogiboks **Konfigureerimine käivitage profiilid** , klõpsake nuppu **OK**.

- Seadistamiseks, [Rakenda ja muudatuste kontrollimiseks](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Organisatsioonisisene ühiku-põhine filtreerimine
Eelistatud võimalus muuta OU-põhine filtreerimine on installimise viisard ja muutke [domeeni ja filtreerimise organisatsiooniüksused](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Installimise viisard on ülesandeid, mis on selles teemas dokumenteerida automatiseerimine.

Kui mingil põhjusel ei saa installi viisardi käivitamiseks, tuleks ainult tehke järgmist.

**Ettevõtte-ühiku-põhine filtreerimine konfigureerimiseks tehke järgmist.**

1. Logige sisse serveris, kus töötab Azure'i AD-ühenduse sünkroonimine kontoga, mis on **ADSyncAdmins** turvalisuse rühma liige.
2. Menüü start kaudu **Sünkroonimise teenuse** käivitamine.
3. Valige **konnektorid** ja **konnektorid** lisanud, valige konnektor **Active Directory domeeniteenused**tüüp. **Toimingud**, valige **Atribuudid**.  
![Konnektor atribuudid](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klõpsake nuppu **Konfigureeri sektsiooni kausta**, valige domeen, mida soovite konfigureerida ja klõpsake **ümbriste**.
5. Küsimisel sisestage mis tahes identimisteabe lugemis-juurdepääsu oma kohapealse Active Directory. See ei pea olema kasutaja, mis on juba eelnevalt täidetud dialoogiboksis.
6. Tühjendage dialoogiboksis **Valige ümbriste** organisatsiooniüksused, mida te ei soovi cloud kataloogi sünkroonimine ja seejärel klõpsake nuppu **OK**.  
![OU](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - **Arvutid** ümbris peaks olema valitud Azure AD edukalt sünkroonimist arvutite Windows 10 jaoks. Kui teie domeeni ühendatud teiste organisatsiooniüksused asuvad arvutites, veenduge, et need poleks valitud.
  - Kui teil on mitu kogumit koos usalduste peaks olema valitud **ForeignSecurityPrincipals** ümbrises. See ümbris võimaldab rist – mets turvalisus rühmakuuluvus lahendada.
  - Kui olete lubanud funktsiooni seadme tagasikirjutusega peaks olema valitud **RegisteredDevices** OU. Kui kasutate funktsioonist tagasikirjutusega rühma tagasikirjutusega, nt veenduge, et järgmistesse kohtadesse on valitud.
  - Valige kasutajad, iNetOrgPersons, rühmad, kontaktide ja arvutite asukohaks teiste OU. Järgmisel pildil kõik need asuvad ManagedObjects OU.
7. Kui olete lõpetanud, sulgege dialoogiboks **Atribuudid** nuppu **OK**.
8. Seadistamiseks, [Rakenda ja muudatuste kontrollimiseks](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Atribuut põhinev filtreerimine
Veenduge, et te olete November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) või hiljem luua neid juhiseid.

Atribuut vastavalt filtreerimine on paindlikult filter objektidele. Saate määrata peaaegu iga osa, kui objekti sünkroonitakse Azure AD power [deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning.md) .

Filtreerimine saate rakendada nii [sissetulevate](#inbound-filtering) Active Directoryst metaversumisse ja [Väljamineva meili](#outbound-filtering) metaversumi Azure AD. Soovitatav on rakendada, filtreerides sissetuleva, kuna see on kõige lihtsam säilitada. Väljamineva filtreerimine tuleks kasutada ainult vajadusel liituda rohkem kui üks mets objektid enne väärtustamise.

### <a name="inbound-filtering"></a>Sissetulev filtreerimine
Sissetuleva vastavalt filtreerimine on kasutusel vaikimisi konfiguratsiooni kus objektide kavatse Azure AD peab olema seatud väärtus sünkroonitavate metaversumi atribuut cloudFiltered. Kui selle atribuudi väärtuseks on seatud **tõene**, siis objekti ei sünkroonita. See pole seadma **väär (FALSE)** , kujundus. Veendumaks, et muude reeglitega on võimalus täiendada väärtuse, ainult peaks see atribuut on **tõene** või **NULLVÄÄRTUSEGA** (puuduvad) väärtused.

Sissetuleva filtreerimine, saate **ulatus** power määratleda, millist objektide peaks või ei saa sünkroonida. See on, kui teete kohandada vastavalt oma ettevõtte vajadustele. Mooduli ulatus on **rühmitamine** ja **klausel** kui sünkroonimine reegel peaks olema ulatuse määramiseks. **Jaotis** sisaldab ühe või mitme **punkti**. Seal on loogiline ja mitme osalaused ja loogiline või mitme rühmade vahel.

Vaadakem näide:  
![Ulatus](./media/active-directory-aadconnectsync-configure-filtering/scope.png) selle asemel lugeda **(osakonna = seda) või (osakonna = müügi-ja c = US)**.

Näidiseid ja alltoodud juhiseid, saate kasutada kasutaja objekti näide, kuid saate seda kõik objekti tüübid.

Allpool esitatud proovi alustada järjestuse väärtus 500. See väärtus tagab reegleid hinnatakse pärast out box reegleid (lower järjestuse, suurema arvulise väärtuse).

#### <a name="negative-filtering-do-not-sync-these"></a>Negatiivne filtreerimine, "sünkroonida need"
Järgmises näites, saate filtreerida (mitte sünkroonitakse) kõik kasutajad, kus **extensionAttribute15** on väärtus **NoSync**.

1. Logige sisse serveris, kus töötab Azure'i AD-ühenduse sünkroonimine kontoga, mis on **ADSyncAdmins** turvalisuse rühma liige.
2. Käivitage **Sünkroonimine reeglite redaktori** menüü start kaudu.
3. Veenduge, et **Sissetulev** on valitud ja klõpsake nuppu **Lisa uus reegel**.
4. Andke reeglile kirjeldav nimi, näiteks "*sisse: AD-kasutaja DoNotSyncFilter*". Valige õige mets, **kasutaja** **CS objekti tüüp**ja **isiku** **MV objekti tüüp**. **Lingi tüüp**, valige soovitud **liitumine** järjestuse tippige väärtus praegu ei kasutata teise sünkroonimise reegli (nt 500) ja seejärel klõpsake nuppu **edasi**.  
![Sissetulev 1 kirjeldus](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. **KMH ulatuse määramise filter**, nuppu **Lisa rühm**, klõpsake nuppu **Lisa klausel**ja valige atribuudi **ExtensionAttribute15**. Veenduge, et kasutaja on määratud **võrdne** ja tippige väljale väärtus väärtus **NoSync** . Klõpsake nuppu **edasi**.  
![Sissetulev 2 ulatus](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. **Liitumine** reegleid tühjaks jätta, ja seejärel klõpsake nuppu **edasi**.
7. **Transformatsiooni lisamiseks**klõpsake nuppu, valige **FlowType** , et **konstandi**, valige Target atribuut **cloudFiltered** ja tippige väljale Allikas **True**. Klõpsake nuppu **Lisa** reegel salvestada.  
![Sissetulev 3 teisenduse.](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Seadistamiseks, [Rakenda ja muudatuste kontrollimiseks](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Positiivse filtreerimist "ainult need sünkroonimine"
Väljendada positiivne filtreerimine võib olla keerukam, kuna tuleb arvesse võtta ka objekte, mis pole selge sünkroonitud, näiteks konverentsiruumi.

Positiivne filtreerimise suvandi jaoks on vaja kaks sünkroonimine reeglid. Ühe (või mitme) õige ulatus objektide sünkroonimiseks ja teine kõikehõlmav sünkroonimine reegel seda filtrit välja kõik objektid, mis ei ole veel tuvastatud sünkroonitakse objektina.

Järgmises näites saate sünkroonida ainult kasutaja objektid, mille atribuudi osakond on väärtus **Müük**.

1. Logige sisse serveris, kus töötab Azure'i AD-ühenduse sünkroonimine kontoga, mis on **ADSyncAdmins** turvalisuse rühma liige.
2. Käivitage **Sünkroonimine reeglite redaktori** menüü start kaudu.
3. Veenduge, et **Sissetulev** on valitud ja klõpsake nuppu **Lisa uus reegel**.
4. Andke reeglile kirjeldav nimi, näiteks "*sisse: AD-sünkroonida kasutaja müük*". Valige õige mets, **kasutaja** **CS objekti tüüp**ja **isiku** **MV objekti tüüp**. **Lingi tüüp**, valige soovitud **liitumine** järjestuse tippige väärtus praegu ei kasutata teise sünkroonimise reegli (nt 501) ja seejärel klõpsake nuppu **edasi**.  
![Sissetulev 4 kirjeldus](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. **KMH ulatuse määramise filter**, nuppu **Lisa rühm**, klõpsake nuppu **Lisa klausel**ja valige atribuut **osakonna**. Veenduge, et kasutaja on määratud **võrdne** ja tippige väljale väärtus väärtus **Müük** . Klõpsake nuppu **edasi**.  
![Sissetulev 5 ulatus](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. **Liitumine** reegleid tühjaks jätta, ja seejärel klõpsake nuppu **edasi**.
7. **Transformatsiooni lisamiseks**klõpsake nuppu, valige **FlowType** , et **konstandi**, valige Target atribuut **cloudFiltered** ja tippige väljale Allikas **False**. Klõpsake nuppu **Lisa** reegel salvestada.  
![Sissetulev 6 teisendus](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
See on eriline juhtum, kus saate määrata cloudFiltered konkreetselt väär (FALSE).

    Meil on nüüd kõikehõlmav sünkroonimise reegli loomiseks.

8. Andke reeglile kirjeldav nimi, näiteks "*sisse: AD-kasutaja kõikehõlmav filter*". Valige õige mets, **kasutaja** **CS objekti tüüp**ja **isiku** **MV objekti tüüp**. **Lingi tüüp**, valige **liitumine** ja tippige järjestuse praegu ei kasutata teise sünkroonimise reegli (nt 600) väärtus. Teil on valitud on järjestuse väärtus kõrgema (lower järjestuse) kui eelmine sünkroonimine reegel, kuid ruumi ka vasakule, et saaksime lisada filtreerimise sünkroonimine lisareeglite hiljem kui soovite täiendavaid osakondade sünkroonimise käivitamiseks. Klõpsake nuppu **edasi**.  
![Sissetulev 7 kirjeldus](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. **KMH ulatuse määramise filtri** tühjaks jätta, ja klõpsake nuppu **edasi**. Määrab Tühjenda filter näitab kõigi objektide rakendatakse reegel.
10. **Liitumine** reegleid tühjaks jätta, ja seejärel klõpsake nuppu **edasi**.
11. **Transformatsiooni lisamiseks**klõpsake nuppu, valige **FlowType** , et **konstandi**, valige Target atribuut **cloudFiltered** ja tippige väljale Allikas **True**. Klõpsake nuppu **Lisa** reegel salvestada.  
![Sissetulev 3 teisenduse.](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Seadistamiseks, [Rakenda ja muudatuste kontrollimiseks](#apply-and-verify-changes).

Kui soovite, saate luua lisareeglite, kui lisate rohkem objekte meie sünkroonimise esimese tüüpi.

### <a name="outbound-filtering"></a>Väljamineva filtreerimine
Mõnel juhul on vaja teha ainult siis, kui objektid on liitunud metaversumis filtreerimine. Näiteks oleks vaja vaadata atribuuti mail ressursi mets ja atribuut userPrincipalName, kui sünkroonitakse objekti määratlemiseks pärineb konto. Sellisel juhul saate luua Väljamineva reegli filtreerimist.

Selles näites muudate filtreerimise nii ainult kasutajad, kus meili- ja userPrincipalName lõpus @contoso.com on sünkroonitud:

1. Logige sisse serveris, kus töötab Azure'i AD-ühenduse sünkroonimine kontoga, mis on **ADSyncAdmins** turvalisuse rühma liige.
2. Käivitage **Sünkroonimine reeglite redaktori** menüü start kaudu.
3. Valige jaotises **Tüüp reeglite** **Väljaminev**.
4. Otsige reeglit nimega **AAD – kasutaja liitumine SOAInAD välja**. Klõpsake nuppu **Redigeeri**.
5. Klõpsake hüpikaknas vastus **Jah** koopia reegli loomiseks.
6. Muuta lehe **Kirjeldus** kasutamata väärtus, mis on näiteks 50 järjestuse.
7. Klõpsake vasakpoolsel navigeerimispaanil **KMH ulatuse määramise filter** . Klõpsake käsku **Lisa tingimus**, atribuut valige **e-posti**, valige tehtemärk **ENDSWITH**ja väärtuse tüüp **@contoso.com**. Klõpsake käsku **Lisa tingimus**, valige atribuut **userPrincipalName**, valige tehtemärk **ENDSWITH**ja väärtuse tüüp **@contoso.com**.
8. Klõpsake nuppu **Salvesta**.
9. Seadistamiseks, [Rakenda ja muudatuste kontrollimiseks](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Rakendage ja muudatuste kinnitamine
Pärast konfiguratsiooni muudatuste tegemist, rakendatud juba süsteemis objektidele. On võimalik, et objektide pole praegu sync engine töödeldakse ja sync engine peab lugema allika süsteem selle sisu kinnitamiseks uuesti.

Kui olete muutnud **domeeni** kaudu või **Organisatsiooniüksus** filtreerimise konfiguratsiooni, siis peate **täielik import** , millele järgneb **Delta sünkroonimise**teha.

Kui olete muutnud konfigureerimise abil **atribuut** filtreerimine, siis peate teha **täieliku sünkroonimise**.

Tehke järgmised toimingud.

1. Menüü start kaudu **Sünkroonimise teenuse** käivitamine.
2. Valige **konnektorid** ja valige **konnektorid** lisanud, kui tegite konfiguratsioonis muudatuse varem konnektor. **Toimingud**, valige **Käivita**.  
![Käivitage konnektor](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. **Käivitage profiilid**valige eelmises jaotises kirjeldatud toiming. Kui peate käivitama kahe toimingud, käivitage teise, kui esimene on lõpule jõudnud ( **osariik** veerg on valitud konnektor **jõudeoleku** ).

Pärast sünkroonimise, on kõik muudatused etapiviisilise eksporditakse. Enne Azure AD tegelikult tehtud muudatusi, mida soovite veenduge, et kõik need muudatused on õiged.

1. Käivitage käsu viip ja minge`%Program Files%\Microsoft Azure AD Sync\bin`
2. Käivitage:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Sünkroonimise teenuse leiate saatja nimi. See on nimi, mis on sarnane "contoso.com – AAD" Azure AD jaoks.
3. Käivitage:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Nüüd on teil % temp % nimega export.csv, et tutvuda Microsoft Exceli faili. See fail sisaldab kõik muudatused, mis on eksportida.
5. Tehke vajalikud muudatused andmete või konfigureerimine ja käivitage järgmist uuesti (impordi, sünkroonimine ja kinnitamine) kuni muudatusi, mis on eksportida.

Kui olete tulemusega rahul, saate eksportida Azure AD soovitud muudatused.

1. Valige **konnektorid** ja valige loendis **konnektorid** Azure AD konnektor. **Toimingud**, valige **Käivita**.
2. **Käivitage profiilid**valige **ekspordi**.
3. Kui palju objektide kustutamine muudatuste konfiguratsiooni, siis näete tõrke ekspordi kui arv on ületab läve konfigureeritud (vaikimisi 500). Kui näete selle vea, siis peate ajutise keelamise funktsioon [vältida juhuslikku kustutab](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md).

Nüüd on aeg ajasti uuesti lubada.

1. Käivitage **Toiminguajasti** menüü start kaudu.
2. Otse leiate jaotisest **Toiminguajasti teek**, tööülesande nimega **Azure AD sünkroonimine ajasti**, paremklõpsake seda ja valige käsk **Luba**.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
