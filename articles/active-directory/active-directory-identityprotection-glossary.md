<properties
    pageTitle="Azure Active Directory identiteedi kaitse sõnastik | Microsoft Azure'i"
    description="Azure Active Directory identiteedi kaitse sõnastik"
    services="active-directory"
    keywords="Azure'i active directory identiteedi kaitse, pilveteenuse rakenduse discovery, haldamise rakendused, Turve, risk, taseme, haavatavuse, turbepoliitika, sõnastik"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory identiteedi kaitse sõnastik 


### <a name="at-risk-user"></a>Risk (kasutajale)  
Kasutaja, kellel on üks või mitu aktiivsed risk sündmust. 

### <a name="atypical-sign-in-location"></a>Ebatüüpiliste asukoht   
Sisselogimine geograafilised asukohast, mis pole tüüpiline teatud kasutaja, sarnaselt kasutajate või rentniku jaoks.

### <a name="azure-ad-identity-protection"></a>Azure'i AD identiteedi kaitse    
Azure Active Directory konsolideeritud vaade risk sündmused ja võimalike nõrkade kohtade mõjutamata organisatsiooni identiteedid mooduli turvalisus.

### <a name="conditional-access"></a>Tingimusvormingu juurdepääs  
Poliitika tagamiseks ressurssidele. Juurdepääsu reeglid talletatakse Azure Active Directory ja hindab Azure AD enne ressursile juurdepääsu andmine.  Reeglite näited kaasata kasutaja asukoha alusel juurdepääsu piiramine seadme seisund või kasutaja autentimise meetodit.

### <a name="credentials"></a>Identimisteave     
Teave, mis sisaldab tuvastamist ja tõendada ID, mida kasutatakse, et saada juurdepääsu kohaliku ja võrgu ressursse. Mandaat on kasutajanimed ja paroolid, kiipkaardi ja serdid.

### <a name="event"></a>Sündmuse   
Azure Active Directory tegevuse kirje.

### <a name="false-positive-risk-event"></a>Valepositiivsete (risk sündmus) 
Risk sündmuse oleku käsitsi määramine kasutaja identiteedi kaitse, mis näitab, et risk sündmuse uurimine ja on valesti lipuga risk sündmus.

### <a name="identity"></a>Identiteedi    
Isik või üksus, peab kontrollima abil autentimine, nt parooli ja serdi kriteeriumide alusel.

### <a name="identity-risk-event"></a>Identiteedi risk sündmuse     
AAD sündmus, mis on lipuga märgitud nimega Anomaalne identiteedi kaitse ja võib märkida, et identiteedi on rikutud.

### <a name="ignored-risk-event"></a>Ignoreeritud (risk sündmus)    
Risk sündmuse oleku käsitsi määramine kasutaja identiteedi kaitse, mis näitab, et risk sündmus on suletud parandamise toimingu võtmata.

### <a name="impossible-travel-from-atypical-locations"></a>Võimalik reisimise ebatüüpiliste kohtadest   
Risk sündmuse käivitanud, kui kaks sisselogimist jaoks sama kasutaja tuvastatakse, kus vähemalt üks neist on ebatüüpilised sisselogimine asukohast ja kus Logi lisandmoodulite vahele jääv aeg on lühem minimaalne see võtaks füüsilise reisi vahel järgmistesse asukohtadesse.  

###<a name="investigation"></a>Uurimine    
Läbivaatamist tegevuste, logid ja muu teave, mis on seotud risk sündmuse otsustada, kas parandamise või vähendamiseks toimingud on vaja mõista, kui ja kuidas ID turvalisust on rikutud ja rikutud identiteedi kasutamise mõistmine.

### <a name="leaked-credentials"></a>Lekkinud identimisteave  

Risk sündmuse vallandanud leidmisel praeguse kasutaja identimisteabe (kasutajanime ja parooli) postitatud avalikult tume web meie teadlased.

### <a name="mitigation"></a>Vähendamiseks  
Toimingu piirata või kõrvaldamiseks ründaja võimalus kasutada rikutud identiteedi või seadme identiteedi või seadme taastamata kaitstud olekusse. Mõne vähendamiseks ei lahenda eelmise risk sündmused, mis on seotud identiteedi või seadme.

### <a name="multi-factor-authentication"></a>Mitmikautentimise 
Autentimise meetodit, mis nõuab kahe või enama autentimismeetodite, mis võivad hõlmata midagi kasutaja on serdi; midagi kasutaja teab, nt kasutajanimed, paroolid või edastamiseks laused; füüsilise atribuute, nagu on sõrmejälje; ja isikliku atribuute, nt isikliku allkirja.

### <a name="offline-detection"></a>Ühenduseta automaattuvastus   
Tuvastamiseks kõrvalekaldeid ja sündmus, näiteks sisselogimise katse riski hindamisest sündmus, mis juhtus alles.

### <a name="policy-condition"></a>Poliitika tingimus    
Osa turbepoliitika, mis määratleb poliitika kaasatakse sihtrühma või välistatakse sellest üksuste (rühmad, kasutajad, rakendused, seadme platvormid, seadme olekus, IP-aadresside vahemikud, KLIENDITÜÜPIDE).

### <a name="policy-rule"></a>Poliitika reegel     
Turbepoliitika, mis asjaolud, mis tuleks käivitamine poliitika ja toiminguid, kui poliitika on vallandanud kirjeldab osa.

### <a name="prevention"></a>Vältimine  
Vältimaks organisatsiooni identiteedi või seadme kaudu kuritarvitamise toimingu arvatav või leida konfliktid. Vältimise toimingu turvaline, seadme või identiteedi ja lahendada eelmise risk sündmused.

### <a name="privileged-user"></a>Õigustega (kasutajale)
Kasutaja, mis risk sündmus, ajal oli üks või mitu ressursi püsivate või ajutiste administraatoriõiguste Azure Active Directory, nt üldadministraator, Arveldusadministraator, teenuse administraator, kasutaja administraator ja administraatori parooli. 

###<a name="real-time"></a>Reaalajas    
Lugege teemat reaalajas tuvastamise.

###<a name="real-time-detection"></a>Reaalajas automaattuvastus  
Kõrvalekaldeid tuvastamise ja selle riski sündmuse sisselogimise katse enne sündmust, nt on lubatud jätkata.

### <a name="remediated-risk-event"></a>Tervendama (risk sündmus)     
Risk sündmuse oleku automaatselt määratud identiteedi kaitse, näidates risk sündmus oli tervendama, seda tüüpi risk sündmuse standard parandamise toimingu abil. Näiteks, kui kasutaja parooli lähtestamiseks palju risk sündmusi, mis näitab eelmise parooli turvalisust on rikutud on automaatselt tervendama.

### <a name="remediation"></a>Parandamise     
Toimingu identiteedi või seadme, mille arvatav varem või teadaolevad konfliktid. Parandamise toimingu taastab identiteedi või seadme kaitstud olekusse ja lahendab eelmise risk sündmused, mis on seotud identiteedi või seadme.

### <a name="resolved-risk-event"></a>Lahendatud (risk sündmus)   
Risk sündmuse oleku käsitsi määramine kasutaja identiteedi kaitse, mis näitab, et kasutaja võttis vastav parandamise toimingu väljaspool identiteedi kaitse ja risk sündmuse kaaluda suletud.

###<a name="risk-event-status"></a>Risk sündmuse olek    
Risk sündmuse atribuut, mis näitab, kas sündmus on aktiivne ja sulgenud, sulgemist põhjus.

###<a name="risk-event-type"></a>Risk sündmuse tüüp  
Risk sündmuse, milles on märgitud normaalne, mis põhjustas sündmuse käsitletakse riskantne kategooria.

###<a name="risk-level-risk-event"></a>Taseme (risk sündmus)  
Viide (kõrge, Keskmine või madal) risk sündmuse aitab identiteedi kaitse kasutajatel tähtsuse toimingud tõsidust need võtta vähendab oma organisatsiooni. 

###<a name="risk-level-sign-in"></a>Taseme (Logi sisse) 
Viide (kõrge, Keskmine või madal) tõenäosuse, et teatud sisselogimine, jaoks kellegi teise proovib kasutada kasutaja identiteet.

###<a name="risk-level-user-compromise"></a>Taseme (kasutaja kompromiss) 
Viide (kõrge, Keskmine või madal) tõenäosuse, et identiteedi on rikutud.

### <a name="risk-level-vulnerability"></a>Taseme (haavatavuse)  
Viide (kõrge, Keskmine või madal) soovitud tundlikkust tähtsuse toimingud identiteedi kaitse kasutajate aitamine tõsidust need võtta vähendab oma organisatsiooni.

### <a name="secure-identity"></a>Turvaline (identiteet)   
Parandamise toiming nagu parooli muutmine või taastada potentsiaalselt rikutud identiteedi kompromissitu oleku reimaging masina võtta.

### <a name="security-policy"></a>Turbepoliitika
Poliitika reeglid ja tingimus kogum. Üksused, nt kasutajad, rühmad, rakendused, seadmed, seadme platvormid, seadme olekus, IP-aadresside vahemikud ja Auth2.0 KLIENDITÜÜPIDE saab rakendada poliitika. Kui poliitika on lubatud, hinnatakse iga kord, kui poliitika kaasatud üksus on välja antud märgiks ressursi.

### <a name="sign-in-v"></a>Logige sisse (v) 
Identiteedi Azure Active Directory autentimiseks.

### <a name="sign-in-n"></a>Sisselogimistõrgete (n) 
Protsessi või tegevuse kinnitamise identiteedi ja Azure Active Directory sündmus, mis sisaldab seda toimingut.

###<a name="sign-in-from-anonymous-ip-address"></a>Anonüümse IP-aadressi kaudu sisselogimine    
Risk sündmuse käivitanud pärast eduka sisselogimine IP-aadress, mis on määratletud anonüümse puhverserveri IP-aadress.

###<a name="sign-in-from-infected-device"></a>Nakatunud seadme kaudu sisselogimine 
Risk sündmus, kui sisselogimine pärineb IP-aadress, kus on teada, et kasutada ühe või mitme kahjustatud seadmed, mida proovite aktiivselt robot serveriga.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Logige sisse IP-aadress kahtlaste tegevuste 
Risk sündmuse käivitanud pärast eduka sisselogimine IP aadress kõrge nurjunud sisselogimise katsete arv üle mitu kasutajakontot lühikese aja jooksul.

###<a name="sign-in-from-unfamiliar-location"></a>Võõras asukohast sisselogimine 
Risk sündmus, kui kasutaja edukalt sisse logib uues asukohas (IP, laiuskraad/pikkuskraad ja ASN).

###<a name="sign-in-risk"></a>Risk sisselogimine     
Vt Risk tase (Logi sisse)

###<a name="sign-in-risk-policy"></a>Sisselogimise risk poliitika  
Juurdepääsu poliitika, mis hindab riski teatud sisselogimine ja kehtib kergendamise põhjal eelmääratletud tingimused ja reeglid.

###<a name="user-compromise-risk"></a>Kasutaja kompromiss risk     
Vt Risk tase (kasutaja kompromiss)

###<a name="user-risk"></a>Kasutaja risk    
Vt Risk tase (kasutaja kompromiss).

###<a name="user-risk-policy"></a>Kasutaja risk poliitika     
Juurdepääsu poliitika, mis leiab Logi sisse ja kehtib kergendamise põhjal eelmääratletud tingimused ja reeglid.

###<a name="users-flagged-for-risk"></a>Kasutajate risk lipuga   
Kasutajatele, kellel on risk sündmused, mis on aktiivne või tervendama

###<a name="vulnerability"></a>Haavatavuse    
Konfiguratsiooni või Azure Active Directory, mis teeb kataloogi suhtes kasutab tingimus või ohtude.


## <a name="see-also"></a>Vt ka

- [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md)
