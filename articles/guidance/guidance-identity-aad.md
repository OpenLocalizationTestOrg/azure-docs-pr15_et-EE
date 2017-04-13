<properties
   pageTitle="Azure Active Directory rakendamine | Microsoft Azure'i"
   description="Kuidas rakendada turvaline hübriid võrgu arhitektuur Azure Active Directory abil."
   services="guidance,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Azure Active Directory rakendamine

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse head tavad kohapealse Active Directory (AD) domeenide ja metsade integreerimine Azure Active Directory pakuvad pilvepõhist autentimist.

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. See viide arhitektuur kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

Saate kataloogi ja identiteedi teenuseid, nagu need, mida AD Directory Services (AD DS) autentida identiteedid. Need identiteedid saate kuuluvad kasutajad, arvutid, rakenduste või muud ressursid, mis moodustavad osa turvalisus äärist. Saate majutada directory ja identiteedi services kohapealse, kuid elementide rakenduse asukohta Azure'i hübriidjuurutuse stsenaarium võib olla tõhusam laiendada selle funktsiooni pilve. Seda moodust saab vähendada, saates autentimine põhjustada latentsus ja kohalik luba kutsed pilvest tagasi töötavad kohapealse kataloogi ja identiteedi teenused.

Azure'i pakub kahte lahendusi pilveteenuses kataloogi ja identiteedi teenuste rakendamiseks:

1. Saate kasutada [Azure Active Directory (AAD)] [ azure-active-directory] AD domeeni luua pilves ja linkida selle asutusesisese AD domeeni. AAD abil saate konfigureerida ühekordse sisselogimise (SSO) rakenduste kaudu pilveteenuses kasutajate jaoks. AAD kasutab [Azure'i AD-ühenduse] [ azure-ad-connect] töötab kohapealse kopeerida, objektide ja identiteedid olevad kohapealse hoidla pilveteenusesse.

    AAD saate rakendada ka ilma mõne kohapealse kataloogi kasutamine. Sel juhul AAD toimib kõik identiteedi teavet, mitte kohapealse Directory kopeeritud andmeid sisaldav esmane allikas.

    Pange tähele, et kataloogi pilveteenuses on **pole** pikendada mõne kohapealse kataloogi. Pigem on koopia, mis sisaldab sama objektid ja identiteedid. Nende üksuste kohapealse tehtud muudatused kopeeritakse pilveteenusesse, kuid tehtud muudatused pilveteenuses, **ei ole** kopeeritud tagasi kohapealse domeeni.

    >[AZURE.NOTE] Azure Active Directory Premium väljaanded ei toeta allahindluse kasutajate paroole, kohapealse kasutajatele teha pilveteenuses parooli iselähtestamist lubada. See on ainult teave, mis AAD sünkroonib naasta kohapealse kataloogi. Erinevate versioonide AAD ja nende funktsioonide kohta leiate lisateavet teemast [Azure Active Directory väljaanded][aad-editions].

2. Juurutamist VM Azure, ulatub oma olemasoleva AD infrastruktuuri (sh AD DS ja AD FS) domeenikontrolleri AD DS töötab teie asutusesisese andmekeskuse kaudu. See lähenemine on rohkem levinud stsenaariumid, kus kohapealse võrgu- ja Azure virtuaalse võrgu on ühendatud VPN-ja/või ExpressRoute ühendus. See lahendus toetab samuti kahesuunalise dispersioonanalüüs, mis võimaldab teil muuta pilves ja kohapealse, on kõige kohasem.

See arhitektuur keskendub lahendus 1. Teine lahendus kohta leiate lisateavet teemast [Azure ulatub Active Directory][guidance-adds].

Tüüpilised kasutamine karpide see arhitektuur on järgmised.

- Pakkudes SSO kasutajatega suhtlemiseks SaaS ja muude rakenduste pilves, kasutades sama identimisteavet, mida nad määrata kohapealse rakendusi.

- Avaldamise kohapealse veebirakenduste pilve kaudu juurdepääsu remote kasutajatele, kes kuuluvad teie organisatsiooni.

- Rakendab iseteenindusliku võimalusi lõppkasutajatele, nt paroolide lähtestamine ja delegeerivad rühma haldamine. 

    >[AZURE.NOTE] Neid võimalusi saab kontrollida administraator rühmapoliitika kaudu.

- Olukordades, kus kohapealse võrgu- ja Azure virtuaalse võrgu majutusteenuse pilv rakendusi ei ole otse seotud VPN tunneliga või ExpressRoute ringi abil.

Pange AAD ei võimalda AD kõiki funktsioone. Näiteks AAD praegu tegeleb ainult kasutaja autentimise asemel arvuti autentimine. Teatud rakenduste ja teenuste, nt SQL Server võib olla vaja arvuti autentimise sel juhul see lahendus on sobiv. Lisaks ei pruugi AAD süsteemide, kus üle kohta – ruumide/cloud piiri migreerida komponendid sobivad, mitte ainult kopeerida.

> [AZURE.NOTE] Üksikasjalik selgitus Azure Active Directory toimimise, vaadake [Microsoft Active Directory ülevaade][aad-explained].

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm tõstab esile see arhitektuur olulised komponendid. Azure töökoormus elementide kohta lisateabe saamiseks vt [Töötab VMs jaoks N-astme arhitektuur Azure][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Lihtsa, see diagramm ainult näitab otseselt seotud AAD ühendused ja kujutatakse web brauseri taotluse ümbersuunamised või muu protokoll seotud liikluse, mis võivad ilmneda autentimis- ja identiteedi federation käigus. Näiteks (kohapealse või remote) kasutaja pääseb tavaliselt web appi kaudu kui ka ja veebirakenduse võib läbipaistev ümber suunata veebibrauseri kaudu AAD taotluse kinnitamiseks. Pärast autentimist, saate kutse edasi tagasi web appi koos teabega vajalikud isikut.

[! [0]][0]

- **Azure Active Directory rentniku**. See on loodud ettevõtte AAD eksemplari. See toimib lihtsa kataloogiteenusest pilve rakendused (tal objektide kopeeritud asutusesisese AD), ja pakub identiteedi.

- **Web taseme alamvõrgu**. Selle alamvõrgu hoiab VMs eeldab, et teie asutuse ja mis AAD võib toimida ka identiteedi ta väljatöötatud kohandatud pilvepõhist rakendus.

- **Kohapealse AD DS server**. Ettevõtte tõenäoliselt juba olemasoleva kataloogiteenusest, nt AD DS. Saate sünkroonida paljud üksused AD DS kataloogis (nt kasutaja ja rühma teabe) koos AAD lubamiseks AAD kasutada seda teavet autentida identiteedid.

- **Azure'i AD-ühenduse sünkroonimine serveriga**. See on kohapealse arvuti, mis töötab Azure'i AD-ühenduse sünkroonimise teenuse. Selle teenuse installida Azure AD-ühenduse tarkvara abil. Azure'i AD-ühenduse sünkroonimise teenuse sünkroonib teabe (kasutajad, rühmad, kontaktide, jne) olevad asutusesisese reklaami AAD pilveteenuses. Näiteks saate ettevalmistamise või rühmade ja kasutajate kohapealsesse deprovision ja need muudatused paljundatakse AAD. Azure'i AD-ühenduse sünkroonimise teenuse edastab AAD rentniku teavet.

    >[AZURE.NOTE] Turvalisuse põhjustel kasutaja parooli ei talletata otse AAD. Pigem AAD hoiab räsi iga parool. See on piisavalt kasutaja parooli kinnitamiseks. Kui kasutaja parooli lähtestamine jaoks on vaja see peab olema tehtud kohapealse ja uue räsi AAD saadetud. AAD Premiumi väljaannetes sisaldavad funktsioone, mida saate automatiseerida selle ülesande, et kasutajad saaksid oma parooli lähtestamine.

## <a name="recommendations"></a>Soovitused

Selles jaotises toodud soovitused AAD järgmiselt.

- AD-ühenduse
- Turvalisus

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure'i AD-ühendus sünkroonimise teenuse soovitused

Sünkroonimine on seotud tagada, et kasutaja identiteedi teavet pilveteenuses mis kohapealne kooskõlas. Azure'i AD-ühenduse sünkroonimise teenuse eesmärk säilitada selles. Soovitused Azure'i AD-ühenduse sünkroonimise teenuse järgmist Kokkuvõte

- Enne rakendamisel sünkroonimine Azure'i AD-ühenduse, peaksite kindlaks tegema ettevõtte sünkroonimise nõuetele (sünkroonida, millised domeenis, mida ja kui sageli. Artikli [määratlemine directory sünkroonimise nõuetele] [ aad-sync-requirements] kirjeldatakse punktid, mida peaks kaaluma.

- Azure'i AD-ühenduse sünkroonimise teenuse VM abil saate käivitada või arvuti majutatud kohapealse. Sõltuvalt volatiilsus AD kataloogi teabe, kui algse sünkroonimisega AAD on tehtud Azure'i AD-ühenduse sünkroonimise teenuse laadi tõenäoliselt kõrge. Kasutades VM võimaldab teil hõlpsam mastaapimiseks server (kuvar, nagu on kirjeldatud jaotises [jälgimis kaalutlused](#monitoring-considerations) tegevuse määratlemaks, kas see on vajalik).

- Kui teil on mitu kohapealse domeeni mets, saate talletada ja sünkroonida kogu mets ühe AAD rentniku kohta (see pole soovitatav). Filtreerida teavet identiteetide, mis ilmnevad mitu domeeni, et iga identiteedi kuvatakse ainult üks kord AAD asemel on kopeeritud, kuna see võib põhjustada selliste vastuolude tekkimist, kui andmed on sünkroonitud. Seda moodust jaoks on vaja rakendada mitme Azure'i AD-ühenduse sünkroonimine serveriga. Lisateabe saamiseks vt mitme AAD stsenaarium jaotises [topoloogia peaksite arvesse võtma](#topology-considerations) . 

- Filtri abil saate piirata talletatud AAD ainult, et andmeid, mida on vaja. Näiteks ettevõtte võib ei soovi AAD passiivsed või -isiklik kontode teabe talletamiseks. Filtreerimine võib olla rühma-põhine, domeeni põhinev, OU-põhine või atribuut põhinev ja filtrid keerukamate reeglite loomiseks saate omavahel kombineerida. Näiteks võib valige sünkroonida ainult objektide olevad domeen on kindel väärtus valitud atribuut. Üksikasjalikku teavet leiate teemast [Azure'i AD-ühenduse sünkroonimine: konfigureerimine filtreerimine][aad-filtering].

- Rakendada kõrge-saadavus sünkroonimise teenuse AD-ühendus, käivitage teisene lavastus server. Lisateavet leiate jaotisest [topoloogia peaksite arvesse võtma](#topology-considerations) .

### <a name="security-recommendations"></a>Turvalisus soovitused

Järgmiste üksuste Kokkuvõte esmane turvalisuse soovitused rakendamiseks AAD, sõltuvalt teie asutuse vajadustega

- Hallata kasutajate paroole.

    Kui kasutate AAD Premiumi väljaannet, saate lubada kohandatud installi, Azure'i AD-ühenduse parooli allahindluse AAD kaudu oma kohapealse kataloogi.

    [! [9]][9]

    See funktsioon võimaldab kasutajatel Azure portaali oma paroole lähtestada, kuid peaks olema lubatud ainult pärast teie ettevõtte paroolipoliitika. Näiteks saate piirata, millised kasutajad saavad oma parooli muuta, ja te saate kohandada parooli juhtimise kogemus. Lisateavet leiate teemast [Kohandamise parooli haldus teie asutuse vajadustele sobivaks][aad-password-management].

- Säilitada kaitse kohapealse rakenduste, millele pääseb väliselt.

    Azure'i AD Rakenduse puhverserveri abil pakkuda kontrollitud juurdepääsu kohapealse veebirakenduste mis kaudu AAD välised kasutajad. Seda moodust kaitseb rakenduste ei seatakse otse Interneti-ühendus. Ainult need kasutajad, mis on sobiv mandaate Azure kataloogis on jõudnud rakenduste. Lisateabe saamiseks lugege artiklit [Lubamine rakenduse puhverserveri Azure'i portaalis][aad-application-proxy].

- Aktiivset jälgimist AAD kahtlaste tegevuste märke.

    Kaaluge AAD Premium P2 väljaanne, mis sisaldab AAD identiteedi kaitse. Identiteedi kaitse kasutab kohandatava masina õ algoritmide ja heuristika kõrvalekaldeid avastada ja sündmusi, mis võivad viidata, et identiteedi on rikutud. Näiteks tuvastab see potentsiaalselt Anomaalne tegevuse korrapäratu sisselogimise tegevused, nt sisselogimist tundmatutest allikatest või IP-aadresside kahtlase tegevuse või seadmest, mis võivad olla nakatunud sisselogimist. Identiteedi kaitse abil andmed, loob aruanded ja teatised, mis võimaldab teil uurida järgmisi risk sündmused ja vastav parandamise või vähendamiseks toimingu tegemine. Lisateavet leiate teemast [Azure Active Directory identiteedi kaitse][aad-identity-protection].

    Saate aruannete funktsioon AAD Azure'i portaalis kahtlaste ja muude turvalisusega seotud tegevusi, mis ilmnevad teie süsteemi jälgimiseks. AAD saate luua Kokkuvõte aruanded:

    [! [17]][17]

    Aruannete kasutamise kohta leiate lisateavet teemast [Azure Active Directory aruandluse juhend][aad-reporting-guide].

## <a name="topology-considerations"></a>Topoloogia kaalutlused

Kui on mõni kohapealse kataloogi integreerimise AAD, konfigureerida Azure'i AD-ühenduse topoloogia, mis kõige paremini vastab teie asutuse vajadustega; rakendada. Topoloogiatest toetab Azure'i AD-ühenduse, mis sisaldavad järgmist:

- **Ühe mets, ühe AAD kataloogi**. See topoloogia kasutate Azure'i AD-ühenduse objektide sünkroonimiseks ja identiteedi teavet üks või mitu domeenides igaks kohapealse mets ühe AAD rentnikuga. See on vaikimisi topoloogia, mida kiire installimise Azure'i AD-ühenduse.

    [! [1]][1]

    Ärge looge mitme Azure'i AD-ühenduse sünkroonimine serveriga ühenduse erinevate domeenide sama kohapealse mets sama AAD rentniku juhul, kui te kasutate Azure'i AD-ühenduse sünkroonimine serveriga lavastus režiimi (vt allpool lavastus Server).

- **Mitme metsad, ühe AAD kataloogi**. Kasutage seda topoloogia, kui teil on rohkem kui üks asutusesiseses kogumis. Võite konsolideerida identiteedi teavet, et iga kordumatu kasutaja on esitatud ühe korra kataloogis AAD isegi juhul, kui sama kasutaja on rohkem kui üks mets olemas. Kõik metsade kasutada sama Azure'i AD-ühenduse sünkroonimine serveriga. Azure'i AD-ühenduse sünkroonimine server ei ole osalema domeene, kuid peab olema kättesaadav kõigi metsast.

    [! [2]][2]

    See topoloogia, ärge kasutage eraldi Azure'i AD-ühenduse sünkroonimine serverid iga asutusesisese mets ühenduse ühe AAD rentniku. See võib põhjustada dubleeritakse identiteedi teavet AAD, kui kasutajad on rohkem kui üks mets.

- **Mitme metsad, eraldi topoloogiatest**. Seda moodust võimaldab teil ühe AAD rentniku identiteedi teavet eraldi metsast ühendada. See strateegia on kasulik, kui olete ühendatud metsade eri ettevõtetest (pärast ühinemise või omandamise, näiteks) ja iga kasutaja identiteedi teavet hoitakse ainult üks mets.

    Kui GALs iga mets on sünkroonitud, siis kasutaja ühe mets võib esineda teise kontaktina. See võib juhtuda siis, kui näiteks teie asutuses on rakendatud GALSync Forefront Identity manager 2010 või Microsoft Identity Manager 2016. Selle stsenaariumi korral saate määrata, et kasutajad peaksid tähistatud oma atribuuti *Mail* . Samuti saate sobitada identiteedid abil *ObjectSID* ja *msExchMasterAccountSID* atribuute. See on kasulik, kui teil on ühe või mitme ressursside kogumit keelatud kontod.

- **Lavastus server**. Selle konfiguratsiooni käivitate esimese paralleelselt teise eksemplari sünkroonimine server Azure'i AD-ühenduse. See struktuur toetab stsenaariumid nagu:

    - Kõrge-saadavus.

    - Testimine ja uue konfiguratsiooni Azure'i AD-ühenduse sünkroonimine serveri juurutamine.

    - Uue serveri tutvustus ja eemaldama vana konfigureerimisel. 

    Need stsenaariumid, teist töötab *lavastus režiim*. Server kirjete imporditud objektide ja sünkroonimise andmeid oma andmebaasi, kuid edastama AAD andmed. Ainult siis, kui keelate lavastus režiim ei server kirjutage andmete AAD ja käivitamise tegelik parooli täitmine – toetab kohapealse kataloogid vajaduse korral:

    [! [4]][4]

    Lisateavet leiate teemast [Azure'i AD-ühenduse sünkroonimine: töötoimingute ja kasutuse][aad-connect-sync-operational-tasks].

- **Mitme AAD kataloogide**. On soovitatav, et loote ühe AAD directory ettevõtte jaoks, kuid võib tekkida olukordi, kus teil tuleb sektsiooni teave üle eraldi AAD kataloogide. Sel juhul veenduge, et iga objekti asutusesiseses kogumis kuvatakse ainult üks AAD directory sünkroonimise ja parooli allahindluse probleemide vältimiseks.

    Sel juhul rakendada, konfigureerida eraldi Azure'i AD-ühenduse sünkroonimine serverid iga AAD kataloog ja kasutada nii, et iga sünkroonimine Azure'i AD-ühenduse server töötab üksteist välistavad objektide kogumi filtreerimine: 

    [! [5]][5]

Nende topoloogiatest kohta leiate lisateavet teemast [topoloogiatest jaoks Azure'i AD-ühenduse][aad-topologies].

## <a name="user-sign-in-considerations"></a>Kasutaja sisselogimist peaksite arvesse võtma

Vaikimisi AAD teenuse eeldab, et kasutajad logige sisse sama parool, et nad kasutavad kohapealse ja sünkroonimine Azure'i AD-ühenduse server konfigureerib parooli sünkroonimise kohapealse domeeni ja AAD esitada. Paljud ettevõtted, see on sobiv, kuid võiksite kaaluda ettevõtte olemasoleva poliitika ja taristu. Näiteks:

- Ettevõtte turbepoliitika võib keelata sünkroonimise parooli hashes pilveteenusesse.

- Saate võib nõuda, et kasutajatele, kellel sujuvalt SSO (ilma paroolide palub) juurdepääs pilveteenuses ressursid domeeni liitunud masinad ettevõtte võrgus.

- Ettevõtte võib juba ADFS-i või mõne muu tootja federation pakkuja juurutatud. Saate konfigureerida AAD kasutada seda taristu rakendada autentimis- ja SSO asemel pilveteenuses teabele parooli abil.

Lisateabe saamiseks vt [Suvandid sisse logida Azure'i AD-ühenduse kasutaja][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure'i AD Rakenduse puhverserveri kaalutlused

Kasutage Azure AD kohapealse rakendused juurdepääsust.

- Pange oma kohapealse veebirakenduste abil rakenduse puhverserveri konnektorid haldamiseks Azure AD Rakenduse puhverserveri komponent. Rakenduse puhverserveri konnektor avatakse väljaminev võrguühenduse Azure AD rakenduse puhverserver. Kaugtöölaua kasutajate kutsed marsruuditakse veebirakenduste tagasi kaudu AAD selle ühenduse kaudu. See eemaldab vaja kohapealne tulemüür, vähendada rünnak pind esitatud ettevõtte sissetuleva portide avamine.

Lisateabe saamiseks lugege teemat [Avalda rakenduste Azure AD Rakenduse puhverserveri][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Objekti sünkroonimise kaalutlused

Azure'i AD-ühenduse konfiguratsiooni vaikimisi sünkroonib objektid kohaliku AD kataloogi alusel määratud artiklis reeglikomplekti [Azure'i AD-ühenduse sünkroonimine: mõistmine vaikimisi konfiguratsiooni][aad-connect-sync-default-rules]. Ainult objektid, mis vastavad reegleid on sünkroonitud, teised ignoreeritakse. Näiteks kasutaja objektid peab olema kordumatu *sourceAnchor* atribuut ja *accounEnabled* atribuut peab olema täidetud. Kasutaja objektid, mis ei sisalda atribuut *sAMAccountName* või, mis algavad teksti *AAD_* või *MSOL_* ei sünkroonita. Azure'i AD-ühendus kehtib mitme reegli kasutaja, kontakti, rühma, ForeignSecurityPrincipal ja arvuti objektid. Kui teil on vaja muuta vaikimisi reeglikomplekti Azure'i AD-ühenduse kaudu installitud sünkroonimise reeglite redaktori kasutamine (ka [Azure'i AD-ühenduse sünkroonimine: mõistmine vaikimisi konfiguratsiooni][aad-connect-sync-default-rules]).

Saate määratleda oma domeeni või OU sünkroonitavate objektide piiritlemiseks filtreid. Või rakendada keerukamate kohandatud filtreerimine, nagu on kirjeldatud [Azure'i AD-ühenduse sünkroonimine: konfigureerimine filtreerimine][aad-filtering].

## <a name="security-considerations"></a>Turvalisuse alused

Tingimusvormingu juurdepääsu reguleerimine abil saate keelata taotluste autentimise ootamatud allikatest:

- [Azure'i mitme teguriga autentimine (MFA)] käivitamine[ azure-multifactor-authentication] kui kasutaja proovib ühendust luua asukohast – usaldusväärsed (näiteks Internetis) asemel usaldusväärsete võrgus.

- Seadme platvormi tüüp kasutaja (iOS, Android, Windows Mobile Windows) abil saate määratleda Accessi rakenduste ja funktsioonide poliitika.

- Salvestamine lubatud või keelatud olekusse kasutajate seadmete ja selle teabe lisada Accessi poliitika kontrollid. Näiteks kui kasutaja telefon on kadunud või varastatud märgitakse nagu on keelatud, et takistada selle kasutamist pääseda juurde.

- Rühmakuuluvus põhineb kasutaja juurdepääsu reguleerida. Kasutada [AAD dünaamiline liikmelisusel] [ aad-dynamic-membership-rules] lihtsustada haldamist rühma. Põgusa ülevaate sellest, kuidas see toimib, lugege [artikleid Sissejuhatus rakendusse dünaamiline Liikmelisused rühmade][aad-dynamic-memberships].

- AAD identiteedi kaitse juurdepääsu riski poliitikate abil saate täpsemalt kaitse ebatavalised tegevuste ja sündmuste põhjal.

Lisateavet leiate teemast [Azure Active Directory juurdepääsu][aad-conditional-access].

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

Skaleeritavus käsitletakse AAD teenuse ja sünkroonimine server Azure'i AD-ühenduse konfiguratsiooni:

- Teenuse AAD on konfigureerida rakendada skaleeritavus suvandid. Teenuse AAD toetab skaleeritavus põhjal koopiad. AAD rakendab ühe peamine koopia, mis pidemete kirjutada, mitme kirjutuskaitstud teisene koopiad. AAD suunab läbipaistev proovitud kirjutab tehtud suhtes teisene koopiad peamine koopia. AAD pakub lõpliku järjepidevuse. Kõik muudatused peamine koopia paljundatakse teisene koopiad. Enamik toiminguid vastu AAD on loeb, mitte kirjutab see arhitektuur skaala ka.

    Lisateavet leiate teemast [Azure AD: meie geograafilise liigne, väga kättesaadav, jaotatud cloud kataloogi kaitstud][aad-scalability].

- Azure'i AD-ühenduse sünkroonimine serveri määratlevad mitu objektid, on tõenäoline, et sünkroonimine kohaliku Directoryst. Kui teil on vähem kui 100 000 objektid, saate kasutada vaikimisi SQL Server Express LocalDB tarkvara Azure'i AD-ühenduse abil. Kui teil on suurem arv objektide, peaksite tootmise SQL serveri versiooni installimine ja kohandatud installi soovitud Azure'i AD-ühenduse täpsustada, et see kasutaks olemasoleva SQL serveri eksemplar.

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Sarnaselt skaleeritavus probleemid, kättesaadavus hõlmavad AAD teenuse ja Azure'i AD-ühenduse konfiguratsiooni:

- AAD teenus on mõeldud kõrge kättesaadavus. On pole kasutaja konfigureeritav kättesaadavus võimalusi. On geo jaotatud ja töötab mitu andmekeskuste ümbruses kogu maailmas, kus automatiseeritud Tõrkesiirde. Kui andmekeskuse pole saadaval, AAD tagab andmete kataloogi eksemplari juurdepääsu vähemalt kaks rohkem piirkonnas hajutatud andmekeskuste saadaval.

    >[AZURE.NOTE] SLA AAD põhi- ja Premium teenuste garantiid vähemalt 99,9% saadavalolekut. On tasuta kiht, AAD puudub SLA. Lisateavet leiate teemast [SLA Azure Active Directory jaoks][sla-aad].

- Azure'i AD-ühenduse sünkroonimine serveri kättesaadavuse parandamiseks saate kasutada teise astme lavastus režiim, nagu on kirjeldatud jaotises [topoloogia peaksite arvesse võtma](#topology-considerations) . 

    Lisaks, kui te ei kasuta SQL Server Express LocalDB eksemplari, mis on kaasas Azure'i AD-ühenduse, siis kaaluge kõrge kättesaadavus SQL Server. Pange tähele, et toetatud ainult kõrge-saadavus lahendus on rühmitamise SQL-i. Lahendusi, nt peegeldus ja alati ei toeta Azure'i AD-ühenduse.

    Täiendavad asjaolud säilitamise sünkroonimine Azure'i AD-ühenduse server ja kuidas taastada pärast tõrke kohta, lugege teemat [Azure'i AD-ühenduse sünkroonimine: töötoimingute ja kaalutlused - avariitaastet][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Kaalutlused haldus

On hallata AAD kahel viisil:

1. Haldamise AAD pilveteenuses.

2. Azure'i AD-ühenduse sünkroonimine serverid säilitada.

AAD pakub haldamise domeenid ja kataloogide pilveteenuses järgmistest suvanditest:

- [Azure Active Directory PowerShelli moodul][aad-powershell]. Kui teil on vaja skript levinud Azure AD haldustoiminguid nagu kasutaja haldamine, domeenihaldus ja ühekordse sisselogimise konfigureerida, kasutage selle mooduli.

- Azure'i AD halduse blade Azure'i portaalis. See tera annab ülevaate interaktiivsed halduse kataloogi ja võimaldab teil määrata, ja enamikule AAD konfigureerimine.

    [! [10]][10]

Azure'i AD-ühendus installitakse järgmised tööriistu, mida kasutada säilitamiseks Azure'i AD-ühenduse sünkroonimise teenused oma kohapealse masinad:

- Microsoft Azure Active Directory ühendust konsooli. See tööriist võimaldab teil muuta Azure AD sünkroonimine serveri konfiguratsiooni, kohandada, kuidas sünkroonimise, lubamine või keelamine lavastus režiimi ja vahetamine kasutaja sisselogimise režiimi (saate lubada AD FS-i sisse logida kasutades oma kohapealse taristu).

- Sünkroonimise Service Manager. See tööriist vahekaardil *toimingud* abil saate hallata sünkroonimine ja tuvastada, kas ei ole mis tahes osale protsess. Võite käivitada käsitsi kasutades seda tööriista sünkroonimine. 

    [! [12]][12]

    *Konnektorid* menüü võimaldab teil määrata domeenide ühendused (kohapealse ja pilveteenuses), mis on seotud sünkroonimise mootori:

    [! [13]][13]

-  Sünkroonimise reeglid redaktor. Selle tööriista abil saate kohandada viisi, kus on objektide ümber kui kopeerimist AAD pilves ja kohapealse kataloogi vahel. See tööriist võimaldab teil määrata täiendavad atribuudid ja objektide sünkroonimiseks ja filtrid, mis määratlevad, millised eksemplarid peaks või ei saa sünkroonida.

    Lisateabe saamiseks vt sünkroonimise redaktoris dokumendi [Azure'i AD-ühenduse sünkroonimine: mõistmine vaikimisi konfiguratsiooni][aad-connect-sync-default-rules].

Lehe [Azure'i AD-ühenduse sünkroonimine: head tavad vaikimisi konfiguratsiooni muutmise] [ aad-sync-best-practices] sisaldab täiendavat teavet ja näpunäiteid Azure'i AD-ühenduse.

## <a name="monitoring-considerations"></a>Kaalutlused jälgimine

Seisundi jälgimine läbi abil installitud agentide kohapealse sarja:

- Azure'i AD-ühendus installib agent, mis sisaldab teavet sünkroonimise kohta. Azure'i portaalis Azure Active Directory ühenduse seisund tera abil jälgida seisundi ja jõudluse Azure'i AD-ühenduse. Lisateabe saamiseks vt [abil Azure'i AD-ühenduse seisund sünkroonimise][aad-health].

- AD DS domeenid ja kataloogide Azure seisundi jälgimine, installige Azure'i AD-ühenduse seisundi AD DS agent kohapealse domeeni arvutisse. Kasutage Azure Active Directory ühenduse seisund tera Azure AD DS kuvari portaalis. Lisateabe saamiseks vt [abil Azure'i AD-ühenduse seisund koos AD DS][aad-health-adds]

- Installige Azure'i AD-ühenduse seisundi AD FS-i Agent AD FS-i töötavate kohapealse seisundi jälgimine ja kasutada Azure Active Directory ühenduse seisund tera portaalis Azure AD DS jälgimiseks. Lisateabe saamiseks vt [abil Azure'i AD-ühenduse seisund AD FS-i abil][aad-health-adfs]

Installimise AD ühenduse seisund agentide ja nende nõuete kohta lisateabe saamiseks vaadake teemat [Azure AD-ühenduse seisund Agent installimine][aad-agent-installation].

## <a name="sample-solution"></a>Valimi lahendus

Selles jaotises dokumendid ehitamine valimi AAD konfiguratsiooni testimiseks kasutatavad juhised. Lahendus hõlmab järgmisi elemente:

- Rakenduse web n-astme, järgides ülesehitust kirjeldatud [Töötab VMs jaoks N-astme arhitektuur Azure][implementing-a-multi-tier-architecture-on-Azure].

- Testi klientarvutis. Soovitame kasutada mõne muu VM selle arvuti. See VM konfiguratsiooni oluline, kui teil on juurdepääs administraator ja see saab ühenduda n-astme veebirakenduse.

- Jäljendatud asutusesiseses keskkonnas, mis sisaldab oma domeeni kasutavate AD DS.

Stsenaariumid, mis illustreerivad need juhised on:

- Töötab pilveteenuses välistele kasutajatele, pakkudes Paroolautentimine AAD n-astme veebirakenduse võimaldavad juurdepääsu.

- Võimaldab juurdepääsu n-astme veebirakenduse AAD Paroolautentimine esitada teie asutuses töötavad kasutajad pilveteenuses töötab.

### <a name="prerequisites"></a>Eeltingimused

Järgige juhistes järgmine kohustuslik tarkvara:

- Teil on, kus saate luua ressursirühma Azure olemasolevale tellimusele.

- Olete installinud [Azure'i käsurea liides][azure-cli].

- Alla laadinud ja installinud viimase koostamine. Leiate [siit] [ azure-powershell-download] juhised.

- Olete installinud [makecert] koopia[ makecert] kasuliku klientarvuti testi.

- Teil on juurdepääs arengu arvutisse, kuhu on installitud Visual Studio.

### <a name="create-the-n-tier-web-application-architecture"></a>N-astme web rakenduse arhitektuur loomine

1. Looge kaust nimega `Scripts` , mis sisaldab alamkausta nimega `Parameters`.

2. Laadige alla [Deploy-ReferenceArchitecture.ps1] [ solution-script] PowerShelli skripti skriptide kausta.

3. Parameetrite kausta, luua teise alamkausta nimega Windows.

4. Parameetrite/Windowsi kausta alla laadida järgmised failid:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Skriptide kausta **Deploy-ReferenceArchitecture.ps**1 faili redigeerida ja muuta järgmine rida määramiseks ressursirühm, mida tuleks luua või kasutada hoidke VM ja ressursse, mis on loodud skripti:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Redigeerimine **parameetrite/akna**s kausta järgmised failid ja seadke soovitud `resourceGroup` väärtus on `virtualNetworkSettings` jaotis olema sama, mis neid faile **Deploy-ReferenceArchitecture.ps1** skriptifail määratud.

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Azure'i PowerShelli akna avamine skriptide kausta teisaldada ja käivitage järgmine käsk:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Asendage **<subscription id>** oma Azure tellimuse ID-ga.

    Jaoks **<location>**, määrake Azure piirkond, nt *eastus* või *westus*.

8. Kui script on lõppenud, kasutage Azure portaali saada web taseme laadi koormusetasakaalustusteenuse (*ra-aad-ntier-web-lb*) avaliku IP-aadress.

    [! [18]][18]

9. Logige sisse oma testi kliendi arvuti (või VM) ja veenduge, et pääsete veebi-astme Internet Exploreri abil sirvimiseks web taseme laadi koormusetasakaalustusteenuse avaliku IP-aadress. IIS-i vaikelehe peaks kuvatama.

### <a name="simulate-configuration-of-a-public-web-site"></a>Simuleerida avaliku veebisaidi konfigureerimine

>[AZURE.NOTE] Järgmised toimingud simuleerida, muutes hosts faili testi klientarvuti DNS-i nimi www.contoso.com web-astme seostada. Lisaks veebi-astme VMs on konfigureeritud iseallkirjastatud serdid ja majutusteenuse asutuse võltsitud. See on testimiseks eesmärgil ja **kasutada neid võtteid tootmiskeskkonnas**.

1. Testi klientarvutis **C:\Windows\System32\drivers\etc\hosts** faili redigeerimiseks Notepadi abil ja mis seostab nime www.contoso.com web taseme laadi koormusetasakaalustusteenuse avaliku IP-aadressi kirje lisamine.

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Veenduge, et saate nüüd sirvides www.contoso.com klientarvuti testi. Vaikimisi IIS-i sama lehe peaks kuvatama nagu enne.

3. Testi kliendi arvutisse, avage administraatorina käsuviiba ja kasutada makecert kasuliku c
4. Loo võltsitud juurkausta sertimine sertimiskeskuse sert:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Veenduge, et järgmised failid on loodud.

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Käivitage test juurkausta sertimiskeskus installimiseks järgmine käsk:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Sertimiskeskus testi abil saate luua serdi www.contoso.com:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. Käivitage **MMC-s** käsk ja lisage serdid kohaliku arvuti konto lisandmoodul.

7. Klõpsake soovitud */Certificates (kohaliku arvuti) / isiklik/serdi/* talletada, www.contoso.com serdiga eksportida koos selle faili nimega www.contoso.com.pfx privaatvõti:

    [! [20]][20]

8. Azure'i portaali ja ühenduse halduse taseme VM (*ra aad-ntier-mgmt-vm1*). Vaikimisi kasutajanimi on parooliga *testuser* *AweS0me@PW*:

    [! [21]][21]
    
9. Juhtimise taseme VM ühenduse iga taseme web VMs omakorda kasutajanimi *testuser* ja parooli abil *AweS0me@PW*, ja tehke järgmist. Võtke arvesse, et VMs on privaatne aadressid IP 10.0.1.4, 10.0.1.5 ja 10.0.1.6.

    >[AZURE.NOTE] Web taseme VMs on ainult privaatsed IP-aadressid. Saate ainult ühendada need halduse taseme VM abil.

    1. Kopeerige failid *www.contoso.com.pfx* ja *MyFakeRootCertificateAuthority.cer* klientarvuti testi.
    
    2. Avage administraatorina käsuviiba, teisaldamiseks kausta faile, mille kopeerisite ja käivitage järgmine käsk:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Käivitage selle `mmc` käsk, lisage serdid kohaliku arvuti konto lisandmoodul ja veenduge, et järgmised serdid on installitud:

        - \Certificates (kohaliku arvuti) \Personal\Certificates\ www.contoso.com

        - \Certificates (kohaliku arvuti) \Trusted Root sertimine Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Käivitage Internet Information Services (IIS) Manager konsooli ja liikuge *Sites\Default veebisaidi* arvutis.

    5. *Toimingud* paanil klõpsake *seosed*ja https-sidumise abil www.contoso.com SSL-serdi lisamine.

        [! [22]][22]

10. Klientarvuti testi tagasi ja veenduge, et saate nüüd https://www.contoso.com sirvida.

### <a name="create-an-azure-active-directory-tenant"></a>Azure Active Directory rentnikku loomine

1. Azure'i portaalis loomine on Azure Active Directory rentnik, nt *myaadname*. onmicrosoft.com, kus on *myaadname* kausta nime, mille olete valinud.

2. Lisage kasutaja kataloogi GlobalAdmin rolli *haldus* . Kirjutage äsja loodud parool.

3. Internet Exploreris, liikuge sirvides https://account.activedirectory.windowsazure.com/ ja logige admin@ *myaadname*. onmicrosoft.com. Saate muuta oma parooli, kui seda küsitakse.

### <a name="create-and-deploy-a-test-web-application"></a>Luua ja juurutada veebirakenduse test

1. Visual Studio abil arvutis arengu loomine ASP.net-i veebirakenduse nimega ContosoWebApp1 (.NET Frameworki 4.5.2 kasutada):

    [! [23]][23]

2. Valige mall *MVC* , muuta autentimise *töö ja kooli kontode*ja määrake oma AAD rentniku nimi. Ärge looge rakenduse teenuse pilveteenuses.

    [! [24]][24]

3. Koostamine ja käivitage rakendus testimiseks autentimine. Logige sisse lehel sisestage konto nimi admin@ *myaadname*. onmicrosoft.com, sisestage parool ja klõpsake nuppu *Logi sisse*:

    [! [25]][25]

4. Veenduge, et AAD küsib luba teil sisse logida ja lugege oma profiili ja seejärel käivitub veebirakenduse identiteet haldus.

5. Sulgege Internet Explorer ja naaske Visual Studio.

6. Avage fail, ja selle `<appSettings>` jaotises *ida: PostLogoutRedirectUri* võti väärtust muuta *https://www.contoso.com:443 /*. Salvestage fail.

7. *Lahendus Exploreri* aknas, paremklõpsake ContosoWebApp1 projekt, klõpsake nuppu *Avalda*.

8. *Veebis avaldamine* aknas, klõpsake nuppu *kohandatud*. Saate luua kohandatud profiili nimega *ContosoWebApp1*.

9. Klõpsake lehel *ühenduse* määratud *Avalda meetod* *Failisüsteemis* ja määrake kausta nimega *ContosoWebApp*, mugav asukohas arengu arvutis.

10. Seadke lehel *sätted* *väljaanne* *konfigureerimine* .

11. *Eelvaates* leheküljele, klõpsake nuppu *Avalda*.

12. Ühenduse iga veebiserverisse omakorda (kaudu halduse taseme VM) ja teha järgmist:

    1. Kopeerige *ContosoWebApp* kausta ja selle sisu arvutist arengu *C:\inetpub* kausta.

    2. Teenusekomplekti Internet Information Services (IIS) Manager konsooli, liikuge *Sites\Default veebisaidi* arvutis.

    3. Paanil *toimingud* nuppu *Alus sätted*ja muuta veebisaidi füüsilise tee *%SystemDrive%\inetpub\ContosoWebApp*.

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>Testi veebirakenduse kaudu AAD avaldamine

1. Azure portaali sisse logida ja liikuge AAD kataloogi.

2. Klõpsake menüü *rakendused* ContosoWebApp1 rakendus.

3. Veenduge, et teie taotlus on lisatud kataloogi, ja seejärel klõpsake vahekaarti *KONFIGUREERIMINE* .

4. Muutmine *Logi-ON URL-i* https://www.contoso.com:443 ja *Vasta URL-i* määramine https://www.contoso.com:443 (sama URL).

5. Salvestage konfiguratsiooni.

6. Liikuge klientarvuti testi https://www.contoso.com. Veenduge, et AAD palub teil oma kasutajanimi ja parool ja seejärel logige sisse.

>[AZURE.NOTE] See on esimene stsenaariumi lõpp-punkti: töötab pilveteenuses välistele kasutajatele, pakkudes Paroolautentimine AAD n-astme veebirakenduse juurdepääsu lubamine.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Looge jäljendatud kohapealse keskkonna Active Directoryga

Kohapealse keskkonna sisaldab kahte domeenikontrollerid on `contoso.com` domeeni ja kaks serverid hosting Azure AD ühenduse sünkroonida teenuse. Azure'i AD-ühenduse hosting serverid on domeeni-ühendatud.

1. Naaske skriptide kaust, kus N-astme veebirakenduse loomiseks kasutatud script File Exploreris.

2. Parameetrite kausta, lisada teise sub kausta nimega `Onpremise`.

3. Parameetrite/valikul Asutusesisene kausta alla laadida järgmised failid:

    - [Lisage – lisab-domeeni-controller.parameters.json][add-adds-domain-controller-parameters]

    - [Looge – lisab – mets-extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines-adc.parameters.json][virtualMachines-adc-parameters]

    - [virtualMachines-adc-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines-adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [virtualNetwork lisab dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Skriptide kausta Deploy-ReferenceArchitecture.ps1 faili redigeerida ja muuta järgmine rida määramiseks ressursirühm, mida tuleks luua või kasutada hoidke VM ja ressursse, mis on loodud skripti:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Järgmised failid kausta parameetrite/valikul Asutusesisene redigeerimine ja seadke soovitud `resourceGroup` väärtus on `virtualNetworkSettings` jaotis olema sama, mis neid faile Deploy-ReferenceArchitecture.ps1 skriptifail määratud.

    - virtualMachines-adds.parameters.json

    - virtualMachines-adc.parameters.json

    - virtualNetwork.parameters.json

    - virtualNetwork lisab dns.parameters.json

7. Azure'i PowerShelli akna avamine skriptide kausta teisaldada ja käivitage järgmine käsk:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Asendage `<subscription id>` oma Azure tellimuse ID-ga.

    Jaoks `<location>`, määrake Azure piirkond, näiteks `eastus` või `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Installimine ja Azure'i AD-ühenduse sünkroonimise teenuse konfigureerimine

Kujutatud järgmist konfiguratsiooni koosneb Azure'i AD-ühenduse sünkroonimise teenuse kaks korda. Esimene on aktiivne, kuigi teine on konfigureeritud lavastus režiimis, et anda kiire Tõrkesiirde ja kõrge-saadavus, kui esimene server ei.

1. Azure portaali kaudu, liikuge ressursirühm, hoides VMs Azure'i AD-ühenduse sünkroonimine teenuste (vaikimisi*ra aad-valikul Asutusesisene rg* ) ja ühendage *ra aad-valikul Asutusesisene-adc-vm1* VM. *Testuser* parooliga sisse logida *AweS0me@PW*.

2. Laadige alla [siin]Azure'i AD-ühenduse[aad-connect-download].

3. Käivitage installer Azure'i AD-ühenduse ja installi *kohandamine* suvandi abil.

    >[AZURE.NOTE] Suvand *Kiire* ei saa kasutada, kuna majutusteenuse Azure'i AD-ühenduse sünkroonimise teenuse VM pole domeeni-ühendatud.

4. Lehel *nõutavate komponentide installimine* tühjaks valikuline konfiguratsioonisätted vaikesuvandite kinnitamiseks.

5. Valige lehel *kasutajate sisselogimist* *Parooli sünkroonimine*.

6. Sisestage lehel *Azure AD ühenduse* admin@ *myaadname*. onmicrosoft.com, kus *myaadname* on teie AAD rentniku nimi. Sisestage administraatori konto parool.

7. Määrake lehel *ühenduse oma kataloogide* contoso.com jaoks mets (tüüp väärtus, kuna seda ei kuvata loendis rippmenüüst), sisestage CONTOSO\testuser kasutaja nimi, määrake AweS0me@PW parool ja seejärel klõpsake käsku *Lisa kataloogi*jaoks.

8. Aktsepteerige lehel *Azure AD sisselogimise konfiguratsioon* kasutaja turvasubjektinimi vaikeväärtus. Märkige ruut *mis tahes kinnitatud domeenid ilma Jätka*.

    >[AZURE.NOTE] Kataloogi contoso.com on loetletud *Kinnitamata*. Veendumaks, et domeeni nimi, peate looma oma domeeni registraatori jaoks TXT-kirje. Selles näites kohapealse domeen on registreeritud väliselt. Lisateavet leiate teemast [Azure Active Directory domeeni nime lisamine][aad-custom-directory].

9. Valige lehel *domeeni ja OU filtreerimine* *sünkroonida kõik domeenid ja organisatsiooniüksused* (vaikesäte).

10. Aktsepteerige lehel *kordumatult tuvastada kasutajate* vaikeväärtused.

11. Valige lehel *filtreerimine kasutajatele ja seadmetele* *sünkroonida kõigile kasutajatele ja seadmetele* (vaikesäte).

12. Valige lehel *valikulised funktsioonid* *parooli tagasikirjutusega*.

13. *Konfigureerida* lehel Valige *sünkroonimise alustamiseks kui konfigureerimine on lõpule jõudnud*, kuid jätta *lavastus režiimi* märkimata.

14. Veenduge, et konfigureerimine lõpule vigadeta ja seejärel sulgege installer.

15. Azure'i portaalis ühenduse *ra aad-valikul Asutusesisene-adc-vm2* VM. *Testuser* parooliga sisse logida *AweS0me@PW*.

16. Allalaadimine: Azure'i AD-ühenduse ja seejärel käivitage installer.

17. Juhis viisardiga ja vastata, nagu on kirjeldatud toiminguid 3 – 12.

18. Valige lehel *konfigureerida* *käivitage sünkroonimine kui konfigureerimine on lõpule jõudnud*, ja **märkida ruudu** *Luba lavastus režiim*. See põhjustab Azure'i AD-ühenduse sünkroonimise teenuse üksikasjade kontode ja objektide domeeni contoso.com ja salvestaks need oma andmebaasi, kuid see ei edastada oma AAD rentniku järgmisi üksikasju.

14. Veenduge, et konfigureerimine lõpule vigadeta ja seejärel sulgege installer.

### <a name="test-the-aad-configuration"></a>AAD konfiguratsiooni testimiseks

1. Azure portaali kaudu, aktiveerige AAD kataloogi, avage Azure Active Directory tera, nuppu *kasutajad ja rühmad*ja klõpsake *kõikide kasutajate* kasutajatele ja rühmadele Directoryga sünkroonitud loendi kuvamiseks. Peaksite nägema nende kontode kasutajad:

    - administraator (admin@ *myaadname*. onmicrosoft.com)

    - Kohapealse kataloogi sünkroonimise teenuse konto (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

    - Kohapealse kataloogi sünkroonimise teenuse konto (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

2. Azure'i portaalis, liikuge ressursirühm, hoides VMs AD DS domeenikontrollerid (vaikimisi*ra aad-valikul Asutusesisene rg* ) ja ühenduse *ra aad-valikul Asutusesisene-ad-vm1* VM. *Testuser* parooliga sisse logida *AweS0me@PW*.

3. *Active Directory kasutajad ja arvutid* konsooli abil lisada uue domeeni kasutaja nimega Diane Tibbot, kus sisselogimisnimi dianet@contoso.com. Saate määrata parooli oma valik. Muutke kasutaja kohalike administraatorite rühma liige (see on ainult nii, et saate logida kohalik ning selle kasutaja hiljem - ainult masinad domeen on DCs).

4. Aktiveerige *ra aad-valikul Asutusesisene-adc-vm1* VM, avage PowerShelli aken ja käivitage järgmine käsk:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Veenduge, et käsk tagastab *edu*.

    >[AZURE.NOTE] See toiming on vaja, kuna vaikimisi sünkroonimine on plaanitud käivituma 30 minuti tagant. Need käsud jõustada kaasa kohe sünkroonimise.

5. Azure portaali, aktiveerige oma AAD kataloogi avatud Azure Active Directory tera, nuppu *kasutajad ja rühmad*ja klõpsake käsku *Kõik kasutajad*. Nüüd peaks nähtaval olema Diane Tibbot (dianet@ *myaadname*. onmicrosoft.com) kuvatakse kasutajate loendit.

6. Azure Active Directory tera, klõpsake *Ettevõtte rakendused*ja seejärel klõpsake nuppu *Kõik rakendused*. Klõpsake rakenduse *ContosoWebApp1* ja seejärel klõpsake nuppu *kasutajad ja rühmad*. Klõpsake tööriistaribal nuppu *Lisa*. Klõpsake nuppu *kasutajad ja rühmad*ja valige *Diane Tibbot*. Klõpsake nuppu *Määra*.

7. Internet Exploreris, liikuge saidi https://account.activedirectory.windowsazure.com. Logige dianet@ *myaadname*. onmicrosoft.com varem määratud parool.

    >[AZURE.NOTE] Klõpsake ikooni ContosoWebApp rakenduste loendis. AAD püüab leida veebirakenduse avalikult loetletud DNS-i aadressil www.contoso.com, mis erineb oma web taseme laadi koormusetasakaalustusteenuse aadress.

8. Klõpsake vahekaardi *profiil* . Peaks kuvatama (kui teie määratud mis tahes, kui see on loodud) kasutaja üksikasjad.

    >[AZURE.NOTE] Kui klõpsate nuppu *Muuda parooli*, teil on lubatud seda toimingut peab olema lubatud administraator esmalt selle toimingu sooritamiseks. Lisateabe saamiseks vaadake teemat [Alustamine parooli haldamise][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>Kohapealse kasutajad saaksid käivitage rakendus kaudu AAD autentimist kasutades

1. Naaske *ra aad-valikul Asutusesisene-ad-vm1* domeenikontrolleri VM.

2. Avage *DNS-i halduri* konsooli ja lisage uus hosti kirje www.contoso.com. Määrake web taseme laadi koormusetasakaalustusteenuse avaliku IP-aadress.

3. Kopeerige fail *MyFakeRootCertificateAuthority.cer* testi klientarvuti (loodud selle failide protseduuri [avaliku veebisaidi Simulate konfigureerimine](#simulate-configuration-of-a-public-web-site)
    
4. Avage administraatorina käsuviiba, hoides fail, mille kopeerisite kausta ja käivitage järgmine käsk:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Internet Exploreris, liikuge https://www.contoso.com. Veenduge, et lehe AAD sisselogimise ContosoWebApp1 veebirakenduse kuvatakse.

6. Logige dianet@ *myaadname*. onmicrosoft.com. Rakendus peaks käivitamine ja sisselogimine saate õigesti.

## <a name="next-steps"></a>Järgmised sammud

- Lugege heade tavade [domeeni kohapealse lisatakse Azure] pikendamiseks[adds-extend-domain]

- Lugege heade tavade loomiseks [on lisatakse ressursi mets] [ adds-resource-forest] Azure

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Pilveteenuse identiteedi arhitektuur Azure Active Directory abil"
[1]: ./media/guidance-identity-aad/figure2.png "Üks Koputus ühe mets AAD directory topoloogia"
[2]: ./media/guidance-identity-aad/figure3.png "Mitme investeeringute korral ühe AAD directory topoloogia"
[4]: ./media/guidance-identity-aad/figure5.png "Lavastus serveri topoloogia"
[5]: ./media/guidance-identity-aad/figure6.png "Mitme AAD kataloogide topoloogia"
[6]: ./media/guidance-identity-aad/figure7.png "Azure'i AD-ühenduse sünkroonida mõne kindla SQL serveri eksemplar kohandatud installi valimine"
[7]: ./media/guidance-identity-aad/figure8.png "Milles tuuakse ära SSO kasutaja sisselogimine"
[8]: ./media/guidance-identity-aad/figure9.png "Domeeni ja OU filtreerimise suvandid"
[9]: ./media/guidance-identity-aad/figure10.png "Parooli allahindluse lubamine"
[10]: ./media/guidance-identity-aad/figure11.png "Azure AD halduse tera portaalis"
[11]: ./media/guidance-identity-aad/figure12.png "Azure'i AD-ühenduse konsooli"
[12]: ./media/guidance-identity-aad/figure13.png "Sünkroonimise Service Manager vahekaardil toimingud"
[13]: ./media/guidance-identity-aad/figure14.png "Menüü sünkroonimine Service Manager konnektorid"
[14]: ./media/guidance-identity-aad/figure15.png "Sünkroonimise reeglid redaktor"
[15]: ./media/guidance-identity-aad/figure16.png "Azure Active Directory ühenduse seisund tera Azure'i portaalis nähtaval sünkroonimise seisund"
[16]: ./media/guidance-identity-aad/figure17.png "Azure Active Directory ühenduse seisund tera Azure AD DS seisund nähtaval portaalis"
[17]: ./media/guidance-identity-aad/figure18.png "Turve, aruanded Azure'i portaalis"
[18]: ./media/guidance-identity-aad/figure19.png "Azure'i portaali esiletõstmine web taseme laadi koormusetasakaalustusteenuse avaliku IP-aadress"
[19]: ./media/guidance-identity-aad/figure20.png "Sirvige avaliku IP-aadress web taseme laadi koormusetasakaalustusteenuse Internet Exploreri abil"
[20]: ./media/guidance-identity-aad/figure21.png "Selle lisandmooduli serdid nähtaval www.contoso.com sert"
[21]: ./media/guidance-identity-aad/figure22.png "Ühenduse halduse taseme VM"
[22]: ./media/guidance-identity-aad/figure23.png "HTTPS side vaikimisi veebisaidi loomine"
[23]: ./media/guidance-identity-aad/figure24.png "ContosoWebApp1 veebirakenduse loomine"
[24]: ./media/guidance-identity-aad/figure25.png "Veebirakenduse ContosoWebApp1 autentimise atribuutide seadmine"
[25]: ./media/guidance-identity-aad/figure26.png "Azure'i AAD ContosoWebApp1 veebirakenduse kaudu sisselogimine"
[26]: ./media/guidance-identity-aad/figure27.png "Vaikimisi veebisaidi kausta muutmine"