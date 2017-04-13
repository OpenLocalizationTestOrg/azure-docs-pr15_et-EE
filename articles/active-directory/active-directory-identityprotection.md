<properties
    pageTitle="Azure Active Directory identiteedi kaitse | Microsoft Azure'i"
    description="Siit saate teada, kuidas Azure'i AD identiteedi kaitse võimaldab piirata ründaja rikutud identiteedi või seadme ja secure identiteedi või seadme, mis on varem arvatav või teadaolevad konfliktid."
    services="active-directory"
    keywords="Azure'i active directory identiteedi kaitse, pilveteenuse rakenduse discovery, rakendused, Turve, risk, taseme, haavatavuse, turbepoliitika haldamine"
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
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Azure Active Directory identiteedi kaitse 

Azure Active Directory identiteedi kaitse funktsioon Azure AD Premium P2 väljaande, mis pakub teile kokkuvõtlik risk sündmuste ja võimalike nõrkade kohtade mõjutamata ettevõtte identiteedid sisse. Azure'i AD identiteedi kaitse, Microsoft on need sama süsteemide kättesaadavaks tegemine ettevõtte kliendid ja Microsoft on identiteedid pilvepõhist turvamine üle kümne. Identiteedi kaitse mõjutab olemasoleva Azure AD normaalne tuvastamise võimalused (saadaval Azure AD Anomaalne tegevuse aruannete kaudu) ja tutvustatakse uue risk sündmuste tüübid, mis tuvastab kõrvalekaldeid reaalajas.



##<a name="getting-started"></a>Alustamine

Enamik turvalisuse rikkumisest liikumisel teostatava toimingu ründajad pääseda keskkonnas varastada kasutaja identiteet. Ründajad on muutunud järjest tõhus kasutamine kolmanda osapoole seotud rikkumiste eest ja kasutate keerukaid andmepüügi eest. Kui ründaja pääseb isegi madal õigustega kasutaja kontole, on üsna lihtne neile juurde pääseda olulised ettevõtte ressursid külgmine liikumine. Seetõttu on oluline, et kõik identiteedid kaitse ja kui identiteedi on rikutud, takistada aktiivselt rikutud identiteedi olla ära. 

Rikutud identiteedid avastamine ei ole lihtne ülesanne. Õnneks aitab identiteedi kaitse: kaitse kasutab kohandatava masina õ algoritmide ja heuristika kõrvalekaldeid avastada ja sündmusi, mis võivad viidata, et identiteedi on rikutud.
 
Identiteedi kaitse abil andmed, loob aruandeid ja teatised, mis võimaldab teil uurida järgmisi risk sündmuste ja vastav parandamise või vähendamiseks toimingu tegemine.
 
Kuid Azure Active Directory identiteedi kaitse on rohkem kui järelevalve ja tööriista. Identiteedi kaitse põhjal risk sündmusi, arvutab kasutaja risk tase iga kasutaja, mistõttu saate konfigureerida riski põhjal kaitsmiseks poliitikaid rakendada automaatselt teie ettevõtte identiteedid.  Nende risk poliitikat, lisaks muude juurdepääsu juhtelementide Azure Active Directory ja EMS, saate automaatselt blokeerida või pakkuda kohandatava parandamise toimingud, mis sisaldavad parooli lähtestamise mitmikautentimise jõustamine.  

####<a name="explore-identity-protections-capabilities"></a>Identiteedi kaitse võimaluste uurimine 

**Risk sündmuste ja riskantne kontode tuvastamiseks:**  

- Tuvastamiseks 6 risk tüüpi arvuti õ ja heuristiline reeglite abil 

- Kasutaja tasemete arvutamine

- Kohandatud soovitusi, et parandada üldine turvalisus asendi esile nõrkade kohtade pakkudes



**Risk sündmuste uurimiseks:**

- Risk toimuvate sündmuste teatised saatmine

- Uurib risk sündmuste jaoks oluline ja kontekstipõhise teabe abil

- Esitada lihtsa töövoogude juurdluste jälgimiseks

- Lihtsasti parandamise toimingud nagu parooli lähtestamine



**Poliitika juurdepääsu riski põhjal:**

- Poliitika sisselogimist blokeerimine või nõudva mitmikautentimise väljakutsed riskantne sisselogimist leevendada.

- Poliitika blokeerimine või turvaline riskantne Kasutajakontod

- Poliitika kasutajad mitmikautentimise kasutajaks registreerumine


## <a name="detection-and-risk"></a>Automaattuvastus ja ohud

### <a name="risk-events"></a>Risk sündmused

Risk sündmused on sündmused, mis olid poolt kahtlaseks identiteedi kaitse ja näitab, et identiteedi on kahjustatud. Risk sündmuste täieliku loendi leiate teemast [risk üritused avastada Azure Active Directory identiteedi kaitse](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Taseme

Risk risk sündmuse on märge (kõrge, Keskmine või madal) risk sündmuse tõsidust. Risk tase aitab identiteedi kaitse kasutajatel tähtsuse vähendamiseks oma organisatsiooni võtma toiming. Risk sündmuse tõsidust tähistab signaal tugevus kui ennustaja identiteedi kompromiss, koos müra, et see on tavaliselt tutvustab summa. 

- **Kõrge**: kõrge confidence ja kõrge raskusaste risk sündmus. Need sündmused on keerukas näitajaid, mis on rikutud kasutaja identiteedi ja kõik kasutajakontod, mõjutab peaks tervendama kohe.

- **Keskmise**: kõrge raskusaste, kuid väiksem confidence risk sündmus, või vastupidi. Need sündmused on potentsiaalselt ohtlik ja kõik kasutajakontod, mõjutab peaks tervendama.

- **Madal**: madal turvaline ja madal raskusaste risk sündmus. Sel juhul võivad nõuda viivitamatut toimingut, kuid koos risk muid sündmusi, ei pruugi see tugev märk, et andmed on rikutud esitada. 


![Taseme] (./media/active-directory-identityprotection/01.png "Taseme")

 

Kas tuvastatud risk sündmuste **reaalajas**, või asetage töötlemise pärast risk ürituse juba (ühenduseta). Praegu enamik risk sündmuste identiteedi kaitse on arvutatud ühenduseta ja kuvataks identiteedi kaitse 2 – 4 tunni jooksul. Kuigi hinnatud reaalajas, reaalajas risk sündmuste näitan identiteedi kaitse konsooli 5 – 10 minuti jooksul.

Mitme varasemate versioonide kliendid ei toeta reaalajas risk sündmuse avastamine ja ennetamine. Seetõttu ei saa neid kliendi kaudu sisselogimist tuvastatud või reaalajas vältida.


## <a name="investigation"></a>Uurimine
Oma reisi läbi identiteedi kaitse algab tavaliselt identiteedi kaitse armatuurlaud. 

![Parandamise] (./media/active-directory-identityprotection/1000.png "Parandamise")

Armatuurlaua annab teile juurdepääsu:
 
- Aruannete **Kasutajad lipuga risk**, **Risk sündmused** ja **nõrkade kohtade**
- Sätteid nagu konfiguratsiooni **Turbepoliitikate**, **teatiste** ja **mitmikautentimise registreerimine**
 

See on tavaliselt alguspunkt uurimiseks, mis on läbivaatamist tegevuste, logid ja muu teave, mis on seotud risk sündmuse otsustada, kas parandamise või vähendamiseks samme on vaja, ning kuidas ID turvalisust on rikutud, ja aru rikutud identiteedi kasutamise.

Saate siduda oma juurdlus [teatised](active-directory-identityprotection-notifications.md) Azure Active Directory kaitse saadab e-posti teel.

Järgmistes jaotistes teile rohkem üksikasju ja toiminguid, mis on seotud juurdlust.  



## <a name="what-is-a-user-risk-level"></a>Mis on kasutaja riski tase?

Kasutaja risk tase on märge (kõrge, Keskmine või madal) tõenäosus, et kasutaja identiteet on rikutud. See on arvutatud kasutaja risk sündmused, mis on seotud kasutaja identiteet. 

Risk sündmuse olek on **aktiivne** või **suletud**. Kasutaja risk arvutamise kaasa ainult risk sündmused, mis on **aktiivne** . 

Kasutaja risk tasemel arvutatakse sisendeid järgmist:

- Aktiivse risk sündmused, mis mõjutavad kasutaja
- Taseme need sündmused 
- Kas on võetud mis tahes parandamise toimingud 

![Kasutaja riskid] (./media/active-directory-identityprotection/1001.png "Kasutaja riske")



Kasutaja risk taset abil saate luua tingimusvormingu juurdepääsu poliitika riskantne kasutajate sisselogimine blokeerida või sundige neid turvaliselt oma parooli muuta. 


## <a name="closing-risk-events-manually"></a>Risk sündmuste käsitsi sulgemine

Enamikul juhtudel võtate parandamise toiminguid, näiteks turvalise parooli lähtestamine risk sündmuste automaatselt sulgemiseks. Aga see ei pruugi alati olla võimalik.  
See on, näiteks juhul, kui:

- Kasutaja Active risk sündmused on kustutatud
- Juurdluse käigus ilmneb, et teatatud risk sündmus on antud teha õiged kasutaja

Kuna risk sündmused, mis on **aktiivne** kaasa kasutaja risk arvutus, tuleb käsitsi langetada risk taseme sulgedes risk sündmuste käsitsi.  
Juurdluse käigus saate valida olekuks risk sündmuse nende toimingute tegemiseks.

![Toimingud] (./media/active-directory-identityprotection/34.png "Toimingud")

- **Tõrke** - pärast juurdlust risk sündmus, mida tegite eelnevalt väljaspool identiteedi kaitse vastav parandamise toimingu, kui arvate, et risk sündmuse kaaluda sulgenud, märgi sündmuse lahendatud. Lahendada sündmused on määratud risk ürituse oleku suletud ja risk ürituse aitab enam kasutaja risk.

- **Märgi valepositiivsete** - mõnel juhul võib risk sündmuse uurimine ja avastamine, et see on valesti lipuga on riskantne. Sellise esinemiskordade saab vähendada, märkides valepositiivsete risk sündmust. See aitab masina Õppekeskuse algoritmide parandamiseks liigitamine sarnaste sündmuste tulevikus. Oleku valepositiivsete sündmused on **suletud** , ja need aitavad enam kasutaja risk.

- **Ignoreeri** – kui teil on võtnud mis tahes parandamise toimingu, kuid soovite risk sündmuse aktiivne loendist eemaldada, saate märkida risk sündmuse Ignoreeri ja sündmuse olek on suletud. Ignoreeritud sündmuste ei kaasa kasutaja risk. See suvand võib kasutada ainult ebatavalised tingimustel. 

- **Uuesti** - Risk sündmuste käsitsi suletud (valides **lahendada**, **vale positiivne**või **Ignoreeri**) saate uuesti aktiveerida, säte sündmuse oleku tagasi **aktiivne**. Kasutaja risk taseme arvutamise kaasa taasaktiveeritud risk sündmused. Risk sündmuste suletud parandamise kaudu (nagu turvalise parooli lähtestamiseks) ei saa uuesti aktiveerida. 




**Seotud konfiguratsiooni dialoogiboksi avamiseks**:

1. **Azure'i AD identiteedi kaitse** enne, **uurida**, klõpsake **Risk sündmused**.

    ![Käsitsi parooli lähtestamine] (./media/active-directory-identityprotection/1002.png "Käsitsi parooli lähtestamine")

2. Klõpsake loendis **Risk sündmuste** riski.

    ![Käsitsi parooli lähtestamine] (./media/active-directory-identityprotection/1003.png "Käsitsi parooli lähtestamine")

2. Enne risk, paremklõpsake kasutaja.

    ![Käsitsi parooli lähtestamine] (./media/active-directory-identityprotection/1004.png "Käsitsi parooli lähtestamine")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Sulgege kõik risk sündmused kasutaja käsitsi

Risk sündmuste kasutaja käsitsi ükshaaval sulgemise, asemel Azure Active Directory identiteedi kaitse lisaks ka meetodi sulgeda kõik sündmused kasutaja ühe hiireklõpsuga.


![Toimingud] (./media/active-directory-identityprotection/2222.png "Toimingud")

Kui klõpsate **eiramine kõik sündmused**, suletakse kõik sündmused ja mõjutatud kasutaja pole enam vastutusel.



## <a name="remediating-user-risk-events"></a>Remediating kasutaja risk sündmused

Lisamine ja kä on toiming identiteedi või seadet, mis on varem arvatav või teadaolevad konfliktid. Parandamise toimingu taastab identiteedi või seadme kaitstud olekusse ja lahendab eelmise risk sündmused, mis on seotud identiteedi või seadme.

Migreerimisel ning probleemide lahendamisel kasutaja risk sündmusi, saate teha järgmist.

- Tehke turvalise parooli lähtestamiseks migreerimisel ning probleemide lahendamisel kasutaja risk sündmuste käsitsi 

- Kasutaja risk turbepoliitika leevendada või migreerimisel ning probleemide lahendamisel kasutaja risk sündmuste automaatne konfigureerimine

- Piltide uuesti nakatunud seade  


### <a name="manual-secure-password-reset"></a>Käsitsi turvalist parooli lähtestamine

Turvalist parooli lähtestamine on mõne efektiivse parandamise palju risk sündmuste jaoks ja kui läbi, automaatselt need risk sündmused suletakse ja arvutab kasutaja risk tasemel. Saate algatada parooli lähtestamine riskantne kasutaja identiteedi kaitse armatuurlaud. 

Seotud dialoogiboksides parooli lähtestamiseks kaks viisi:

**Parooli lähtestamine** – valige **nõuab kasutaja parooli lähtestamine** võimaldab kasutajal ise taastada, kui kasutaja on registreeritud mitmikautentimise. Ajal kasutaja järgmise sisselogimist, lahendada mitmikautentimise ülesanne edukalt nõutav kasutaja ja seejärel sunnitud parooli muutmine. See suvand pole saadaval, kui kasutaja pole juba registreeritud mitmikautentimise.

**Ajutine parool** - valige **Genereeri ajutine parool** kohe seadmele olemasolev parool ja kasutajale uus ajutine parool luua. Saatke uus ajutine parool teise meiliaadressi jaoks kasutaja või kasutaja manager. Kuna ajutine parool, palutakse kasutajal pärast sisselogimist parooli muutmine.


![Poliitika] (./media/active-directory-identityprotection/1005.png "Poliitika")


**Seotud konfiguratsiooni dialoogiboksi avamiseks**:

1. **Azure'i AD identiteedi kaitse** enne, klõpsake nuppu **Kasutajad lipuga risk**.

    ![Käsitsi parooli lähtestamine] (./media/active-directory-identityprotection/1006.png "Käsitsi parooli lähtestamine")


2. Valige kasutaja, kellel on vähemalt üks risk sündmuste kasutajate loendit.

    ![Käsitsi parooli lähtestamine] (./media/active-directory-identityprotection/1007.png "Käsitsi parooli lähtestamine")


2. Enne kasutaja, klõpsake nuppu **Lähtesta parool**.

    ![Käsitsi parooli lähtestamine] (./media/active-directory-identityprotection/1008.png "Käsitsi parooli lähtestamine")





## <a name="user-risk-security-policy"></a>Kasutaja risk turbepoliitika

Kasutaja risk turbepoliitika on juurdepääsu poliitika, mis mõne kindla kasutaja riski tase hindab ja kehtib puhastamiseks ja vähendamiseks toimingud, mis põhineb eelmääratletud tingimused ja reeglid.


![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/1009.png "Kasutaja ridk poliitika")


Azure'i AD identiteedi kaitse aitab hallata vähendamiseks ja puhastamiseks lipuga märgitud risk, mis võimaldab teil kasutajad:

- Kasutajad ja rühmad kehtib poliitika määramine 

    ![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/1010.png "Kasutaja ridk poliitika")


- Kasutaja risk taseme piirmäära (madal, Keskmine või kõrge) mis käivitab poliitika määramine 

    ![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/1011.png "Kasutaja ridk poliitika")


- Juhtelementide täitmisele, kui poliitika käivitab seadmiseks tehke järgmist.

    ![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/1012.png "Kasutaja ridk poliitika")


- Aktiveerige oma poliitika olek:

    ![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/403.png "MFA registreerimine")


- Vaadake üle ja hinnata enne selle aktiveerimist muudatuste mõju.

    ![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/1013.png "Kasutaja ridk poliitika")


Mitu korda poliitika käivitatakse vähendab mõju kasutajatele ja valides **väga** vähendab.
Siiski ei hõlma **madala** ja **keskmise** lipuga risk poliitikast võivad turvaline, identiteedid või varem arvatav või teadaolevad konfliktid seadmete kasutajad.

Kui poliitika, seadmine

- Kasutajad, kellel on palju vale-positiivsed (arendajate, Turve ärianalüütikud) seotud välistamine

- Asukohtades, kus lubamisega poliitika ei ole praktiline kasutajate välistamine (nt kasutajatugi ei pääse)

- Kasuta **kõrge** piiri ajal algsete poliitika ärirakenduseks, või kui te peate minimeerida väljakutsed end kasutajad.

- Kui teie asutus vajab suurem turvalisus, kasutage **madal** lävi. Valige **madal** lävi tutvustab täiendavat sisselogimist probleeme, kuid suurem turvalisus.

Vaikimisi on soovitatav enamikule asutustest tuleb teil konfigureerida reegli **keskmise** lävi tasakaalu kasutatavuse ja turvalisuse vahel.

Seotud kasutusvõimalused ülevaate leiate järgmistest teemadest.

- [Compromised konto taastamise kulgemist](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Compromised konto blokeeritud kulgemist](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**Seotud konfiguratsiooni dialoogiboksi avamiseks**:

1. Klõpsake jaotises **konfigureerimine** **Azure AD identiteedi kaitse** enne, **kasutaja risk poliitika**.

    ![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/1009.png "Kasutaja ridk poliitika")






## <a name="mitigating-user-risk-events"></a>Kasutaja risk sündmuste leevendada.
Administraatorid saate seada kasutaja risk turbepoliitika Blokeeri kasutajate pärast sisselogimist sõltuvalt riski. 

Blokeerimine sisselogimine:
 
- Takistab uute kasutajate risk sündmuste mõjutatud kasutaja tekkimist

- Abil saavad käsitsi migreerimisel ning probleemide lahendamisel risk sündmuste mõjutamata kasutaja identiteedi ja taastada selle turvaline riik



## <a name="what-is-a-sign-in-risk-level"></a>Mis on sisselogimise risk tase?

Sisselogimise risk tase on märge (kõrge, Keskmine või madal) tõenäosus, et jaoks mõne kindla sisselogimist, kellegi teise proovib kasutaja identiteet autentimiseks. Sisselogimise risk hinnatakse sisselogimine ajal ning leiab risk sündmused ning näidikud leitud reaalajas selle kindla sisselogimine. 

## <a name="mitigating-sign-in-risk-events"></a>Sündmuste logi sisse risk leevendada. 
Mõne vähendamiseks on toiming piirata ründaja võimalus kasutada rikutud identiteedi või seadme identiteedi või seadme taastamata kaitstud olekusse. Mõne vähendamiseks ei lahenda eelmise sisselogimise risk sündmuste identiteedi või seadme seotud.

Saate juurdepääsu Azure AD identiteedi kaitse automaatselt sisselogimise risk sündmuste leevendada. Need poliitikad kasutamisel teie arvates risk taseme kasutaja või Logi sisse riskantne sisselogimist blokeerida või nõuda kasutajalt mitmikautentimise sooritamiseks. Neid toiminguid võivad takistada ründaja ära varastatud identiteedi kahjustada, ja võivad teile aega, et turvaline ID. 


## <a name="sign-in-risk-security-policy"></a>Sisselogimise risk turbepoliitika

Sisselogimise risk poliitika on juurdepääsu poliitika, mis hindab riski teatud sisselogimine ja kehtib kergendamise põhjal eelmääratletud tingimused ja reeglid.

![Sisselogimise risk poliitika] (./media/active-directory-identityprotection/1014.png "Sisselogimise risk poliitika")


Azure'i AD identiteedi kaitse aitab hallata vähendamiseks ja riskantne sisselogimist, mis võimaldab teil:

- Kasutajad ja rühmad kehtib poliitika määramine 

    ![Sisselogimise risk poliitika] (./media/active-directory-identityprotection/1015.png "Sisselogimise risk poliitika")


- Sisselogimise risk taseme lävi (madal, Keskmine või kõrge) mis käivitab poliitika määramine 

    ![Sisselogimise risk poliitika] (./media/active-directory-identityprotection/1016.png "Sisselogimise risk poliitika")


- Juhtelementide täitmisele, kui poliitika käivitab seadmiseks tehke järgmist.  

    ![Sisselogimise risk poliitika] (./media/active-directory-identityprotection/1017.png "Sisselogimise risk poliitika")
    

- Aktiveerige oma poliitika olek:

    ![MFA registreerimine] (./media/active-directory-identityprotection/403.png "MFA registreerimine")

- Vaadake üle ja hinnata enne selle aktiveerimist muudatuste mõju. 

    ![Sisselogimise risk poliitika] (./media/active-directory-identityprotection/1018.png "Sisselogimise risk poliitika")


### <a name="what-you-need-to-know"></a>Mida on vaja teada

Saate konfigureerida sisselogimise risk turbepoliitika nõudma mitmikautentimise.

![Sisselogimise risk poliitika] (./media/active-directory-identityprotection/1017.png "Sisselogimise risk poliitika")

Siiski turvalisuse põhjustel see säte töötab ainult kasutajatele, kellel on juba registreeritud mitmikautentimise. Kui nõudma mitmikautentimise tingimus on täidetud kasutaja, kes pole veel mitmikautentimise jaoks registreeritud, kasutaja on blokeeritud. 

Hea tava, kui soovite nõuda mitmikautentimise jaoks riskantne sisselogimist, peaksite:

1. Luba [mitmikautentimise registreerimist poliitika](#multi-factor-authentication-registration-policy) mõjutatud kasutajate jaoks.
2. Nõua kasutajatel login-riskantne seansi teha MFA registreerimine

Nende juhiste tagab, et mitmikautentimise jaoks on vaja riskantne sisselogimine. 


### <a name="best-practices"></a>Head tavad

 
Mitu korda poliitika käivitatakse vähendab mõju kasutajatele ja valides **väga** vähendab.  
 
Siiski ei hõlma **madala** ja **keskmise** sisselogimist riski poliitika, mis võib blokeerida ründaja kasutamise rikutud identiteedi lipuga. 

Kui poliitika, seadmine 

- Kasutajad, kes ei / ei tohi olla mitmikautentimise välistamine

- Asukohtades, kus lubamisega poliitika ei ole praktiline kasutajate välistamine (nt kasutajatugi ei pääse)

- Kasutajad, kellel on palju vale-positiivsed (arendajate, Turve ärianalüütikud) seotud välistamine

- Kasuta **kõrge** piiri ajal algsete poliitika ärirakenduseks, või kui te peate minimeerida väljakutsed end kasutajad.

- Kui teie asutus vajab suurem turvalisus, kasutage **madal** lävi. Valige **madal** lävi tutvustab täiendavat sisselogimist probleeme, kuid suurem turvalisus.

Vaikimisi on soovitatav enamikule asutustest tuleb teil konfigureerida reegli **keskmise** lävi tasakaalu kasutatavuse ja turvalisuse vahel.

 
Sisselogimise risk poliitika on:

- Rakendatud kõik brauseri liikluse ja kaasaegne autentimist kasutades sisselogimist.
- Ei rakendata rakendustes, mis kasutavad vanema turvalisus Protokollid keelamisel oli-usalda lõpp-punkti ühendatud IDP, nt ADFS-i veebisaidil.

**Risk sündmuste** lehe kaitse konsooli on loetletud kõik sündmused:

- See poliitika on rakendatud
- Saate vaadata tegevuse ja kindlaks teha, kas toimingu oli vastav või ei 

Seotud kasutusvõimalused ülevaate leiate järgmistest teemadest.

- [Riskantne taastamine](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Riskantne sisselogimist blokeeritud](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Mitmikautentimise registreerimise käigus riskantne sisselogimine](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**Seotud konfiguratsiooni dialoogiboksi avamiseks**:

1. Klõpsake jaotises **konfigureerimine** **Azure AD identiteedi kaitse** enne, **sisselogimise risk poliitika**.

    ![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/1014.png "Kasutaja ridk poliitika")





## <a name="multi-factor-authentication-registration-policy"></a>Mitmikautentimise registreerimise poliitika

Azure'i mitmikautentimise on kontrollida, kes te olete meetod, mis nõuab rohkem kui lihtsalt kasutajanime ja parooliga. Teise kasutaja sisselogimist ja tehingud turvalisus kiht pakub.  
Soovitatav on nõuda Azure mitmikautentimise jaoks kasutaja sisselogimist kuna see:

- Pakub mitmesuguseid võimalusi lihtne kinnitamine tugev autentimine

- Tähtis roll ettevalmistamine oma asutuse kaitse ja kompromisse konto taastamine

![Kasutaja ridk poliitika] (./media/active-directory-identityprotection/1019.png "Kasutaja ridk poliitika")



Lisateavet leiate teemast [mis on Azure Mitmikautentimise?](../multi-factor-authentication/multi-factor-authentication.md)


Azure'i AD identiteedi kaitse aitab hallata selle välja mitmikautentimise registreerimise konfigureerimisega poliitika, mis võimaldab teil: 



- Kasutajad ja rühmad kehtib poliitika määramine 

    ![MFA poliitika] (./media/active-directory-identityprotection/1020.png "MFA poliitika")



- Juhtelementide täitmisele, kui poliitika käivitab määramine:  

    ![MFA poliitika] (./media/active-directory-identityprotection/1021.png "MFA poliitika")


- Aktiveerige oma poliitika olek:

    ![MFA poliitika] (./media/active-directory-identityprotection/403.png "MFA poliitika")

- Registreerimise praeguse oleku vaatamiseks tehke järgmist. 

    ![MFA poliitika] (./media/active-directory-identityprotection/1022.png "MFA poliitika")


Seotud kasutusvõimalused ülevaate leiate järgmistest teemadest.

- [Mitmikautentimise registreerimine voog](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Mitmikautentimise registreerimise käigus riskantne sisselogimine](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**Seotud konfiguratsiooni dialoogiboksi avamiseks**:

1. Klõpsake jaotises **konfigureerimine** **Azure AD identiteedi kaitse** enne, **mitmikautentimise registreerimist**.

    ![MFA poliitika] (./media/active-directory-identityprotection/1019.png "MFA poliitika")





## <a name="next-steps"></a>Järgmised sammud

 - [Kanali 9: Azure'i AD ja identiteedi Kuva: identiteedi kaitse eelvaade](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Risk üritused avastada Azure Active Directory identiteedi kaitse](active-directory-identityprotection-risk-events-types.md)
 - [Nõrkade kohtade avastada Azure Active Directory identiteedi kaitse](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory identiteedi kaitse teatised](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory identiteedi kaitse playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory identiteedi kaitse sõnastik](active-directory-identityprotection-glossary.md)

 - [Sisselogimise kogemused Azure AD identiteedi kaitse](active-directory-identityprotection-flows.md)

 - [Azure Active Directory identiteedi kaitse lubamine](active-directory-identityprotection-enable.md)
 - [Azure Active Directory identiteedi kaitse – kuidas blokeeringu kasutajad](active-directory-identityprotection-unblock-howto.md)

 - [Azure Active Directory identiteedi kaitse ja Microsoft Graphi kasutamise alustamine](active-directory-identityprotection-graph-getting-started.md)


