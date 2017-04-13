Identiteedi haldamine on sama oluline avaliku pilves, et see on kohapealne. Abiks olla, Azure toetab mitut eri pilveteenuse identiteedi tehnoloogiaid. Nendeks on need:

- Käivitage Windows Server Active Directory (üldiselt tuntud lihtsalt AD) abil loodud Azure virtuaalse masinad virtuaalmasinates pilveteenuses. Seda moodust mõttekas, kui kasutate Azure laiendada oma asutusesisese andmekeskuse pilve.


- Saate anda oma kasutajate ühekordse sisselogimise [tarkvara kui teenus (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) rakenduste Azure Active Directory. Microsoft Office 365 kasutab seda tehnoloogiat, näiteks, ja töötavad Azure või muude platvormide jaoks loodud pilve rakendused saate kasutada ka see.


- Helistamine ja kohapealse ja pilveteenuse saate anda, et kasutajad Logi sisse, kasutades Facebooki, Google, Microsoft ja muude identiteedipakkujad identiteedid Azure Active Directory juurdepääsu reguleerimine.


Selles artiklis kirjeldatakse kõigi kolme järgmistest suvanditest.

## <a name="table-of-contents"></a>Sisukord

- [Opsüsteemi Windows Server Active Directory virtuaalmasinates](#adinvm)

- [Azure Active Directory abil](#ad)

- [Kasutades Azure Active Directory juurdepääsu reguleerimine](#ac)


## <a name="adinvm"></a>Opsüsteemi Windows Server Active Directory virtuaalmasinates

Opsüsteemi Windows Server AD Azure'i virtuaalmasinates on palju, nagu see kohapealse töötab. [Joonis 1](#fig1) näitab tüüpiline näide kuidas see välja näeb.

![Azure Active Directory Virtual kohapeal](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Joonis 1: Azure'i virtuaalmasinates ühendatud ettevõtte asutusesisese andmekeskuse Azure virtuaalse võrgu kaudu saab käitada Windows Server Active Directory.

Siin näidatud näites töötab Windows Server AD VMs Azure'i Virtuaalmasinates, platvormi IaaS tehnoloogia abil loodud. Need VMs ja mõned teised on rühmitatud virtuaalse võrku ühendatud asutusesisese andmekeskuse, mis Azure virtuaalse võrgu kaudu. Virtuaalse võrgu carves välja cloud virtuaalmasinates suhelda kohapealse võrgu virtuaalse privaatvõrgu (VPN) ühenduse kaudu. See võimaldab teha neid Azure'i virtuaalmasinates näevad ainult teise alamvõrgu kohapealse andmekeskusesse. Nagu joonisel on näidatud, töötavad need kaks Windows Server AD domeenikontrollerid. Muud virtuaalmasinates virtuaalse võrgu võib töötama rakendused, näiteks SharePoint või kasutatakse mõnel muul viisil, näiteks katsetamine. Kohapealse andmekeskuse töötab kaks Windows Server AD domeenikontrollerid.

On mitu võimalust ühendamise domeenikontrollerid pilveteenuses töötavate kohapealne.

- Veenduge, et kõik neist ühe Active Directory domeeni osa.

- Luua eraldi AD domeenide kohapealse ja pilves, mis on osa sama mets.

- Kohapealse ja pilveteenuse eraldi AD metsade loomine ja seejärel metsad rist – mets usalduste või Windows Server Active Directory Federation Services (AD FS), mille saab käivitada ka virtuaalmasinates Azure, abil.

Administraator peaks veenduge, avage taotluste autentimise kohapealse kasutajad pilveteenuses domeenikontrolleritega ainult siis, kui see on vajalik, kuna pilveteenusesse link on tõenäoliselt aeglasem kui kohapealse võrgu, mis tahes valik tehakse. Teine tegur kaaluda ühendusjooni pilveteenuste ja kohapealse domeenikontrollerid on dispersioonanalüüs võrguliiklusest. Domeenikontrollerid pilveteenuses on tavaliselt oma AD sait, mis võimaldab kavandada sageduse kopeerimine on lõpule jõudnud. Azure'i kulude liikluse on Azure andmekeskuse välja saadetud, kuigi pole rakenduses, mis võivad mõjutada administraatori dispersioonanalüüs valikuid saadetud baiti. Samuti on vaja rõhutada, et ajal Azure'i oma domeeninime süsteemi (DNS) toetama, see teenus on puudu funktsioonid nõutud Active Directory (nt tugi dünaamiline DNS-i ja SRV-kirjed). Seetõttu, kus töötab Windows Server AD Azure nõuab häälestamiseks oma DNS-serverid pilveteenuses.

Töötab Windows Server AD Azure VMs saate teha mitu erinevates olukordades. Siin on mõned näited:

- Kui kasutate oma andmekeskuse pikendamine Azure'i Virtuaalmasinates, võib käivitate rakenduste kohaliku domeenikontrollerid käsitlema näiteks Windowsi integreeritud autentimist taotlusi või LDAP päringute allkirjastamiseks pilveteenuses. SharePointi, näiteks suhtleb sageli Active Directory ning nii ajal on võimalik SharePointi pargis käivitamiseks on kohapealse kataloogi kasutamine Azure, häälestamise domeenikontrollerid pilves märkimisväärselt parandab jõudlust. (See on oluline, et mõistan, et see ei pruugi olla vajalik, aga; palju rakendusi saab käitada edukalt ainult kohapealse domeenikontrollerid abil pilves).

- Oletame, et kauge kontoris puuduvad ressursid oma domeenikontrollerid käivitada. Täna, selle kasutajad peavad autentimiseks domeenikontrolleritega teisel pool maailma - logimine on aeglane. Täpsem Microsofti andmekeskuses Azure Active Directory töötavate saate kiirendada see rohkem haru Office'i serverite nõudmata.

- Azure'i tõrkejärgseks kasutava ettevõtte võib säilitada võrdlemisi väike hulk pilves, sh domeenikontrolleri active VMs. Seejärel olema valmis laiendamiseks selle saidi vastavalt vajadusele tõrkeid mujal võtta.

Olemas on ka muid võimalusi. Näiteks te ei vaja Windows Server AD pilveteenuses ühenduse mõne asutusesisese andmekeskuse. Kui soovite käivitada SharePointi pargis, serveeritud teatud kasutajate kogumi, näiteks kõik oleks sisse logida ainult identiteetidega pilvepõhist, võite luua autonoomse mets Azure. See tehnoloogia kasutamise sõltub sellest, millised eesmärgid. (Jaoks üksikasjalikud juhised, Windows Server AD funktsiooniga Azure, [leiate siin](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Azure Active Directory abil

SaaS rakenduste muutuvad rohkem levinud, nad tõsta on selge küsimus: milliseid kataloogiteenusest peaks need pilvepõhist rakendused kasutada? Microsofti vastus on Azure Active Directory.

On kaks peamist võimalust selle kataloogiteenuse pilveteenuses kasutamise kohta:

- Üksikisikute ja kasutada ainult SaaS rakendusi saate toetuvad Azure Active Directory nende ainus kataloogiteenusest nimega.

- Ettevõtted, kus käivitatakse Windows Server Active Directory oma kohapealse kataloogi ühenduse Azure Active Directory ja seejärel selle abil anda oma kasutajate ühekordse sisselogimise SaaS rakendused.


[Joonis 2](#fig2) illustreerib esimene need kaks võimalust, kus Azure Active Directory on kõik, mida on vaja.

![Azure Active Directory Virtual kohapeal](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Joonis 2: Azure Active Directory annab asutuse kasutajate ühekordse sisselogimise SaaS taotlusi, sh Office 365.

Nagu joonisel on näidatud, on Azure AD rentniku mitme teenus. See tähendab, et korraga toetab paljude erinevate ettevõtted, talletamine directory teavet iga kasutaja kohta. Selles näites asutuses A kasutaja proovib SaaS rakenduse avamiseks. See rakendus võib olla osa Office 365 SharePoint Online'i, nt, või see võib olla midagi muud - mitte-Microsofti rakendusi saate kasutada ka selle tehnoloogia. Kuna Azure AD toetab SAML 2.0 protokolli, kõik, mida on vaja rakendusest on võimalus suhelda, kasutades selle valdkonna standard. (Tegelikult rakendusi, mis kasutavad Azure AD käitamise mis tahes andmekeskuses, mitte ainult mõne Azure andmekeskuse.)

Protsess algab, kui kasutaja pääseb juurde SaaS rakenduse (samm 1). Selle rakenduse kasutamiseks peab kasutaja Esita märgiks Azure AD välja.

Luba see sisaldab andmeid, mis kasutaja ja see on digitaalselt allkirjastatud Azure AD. Luba saamiseks kasutaja autendib ise Azure AD esitada kasutajanime ja parooli (samm 2). Azure AD seejärel tagastab luba ta peab (samm 3).

Seejärel saadetakse see luba SaaS rakendus (samm 4), mis on luba allkirja kinnitatakse ja selle sisu (juhis 5) kasutab. Tavaliselt kasutatakse rakenduse identiteedi teave luba sisaldab otsustada, millist tüüpi teavet kasutajal on lubatud juurdepääs ja võib-olla muul viisil.

Kui rakendus peab kasutaja kohta lisateavet, kui mis sisaldub luba, seda taotleda see otse Azure AD abil Azure AD Graph API (samm 6). Azure AD esialgse versioonis directory skeemi on üsna lihtne: see sisaldab vaid kasutajad ja rühmad ja nende vahel seoseid. Rakendusi saate kasutada seda teavet kohta kasutajate vahelisi seoseid. Oletame näiteks, et rakendus on vaja teada, kes selle kasutaja manager on otsustada, kas ta on lubatud mõni andmekogumi juurdepääsu. Seda saate teada, päringute Azure AD Graph API kaudu.

Graph API kasutab tavaliste rahulik protokolli, mis muudab lihtne kasutada enamiku kliendi, sh mobiilsideseadmete kaudu. API toetab samuti laiendid, mis on määratletud OData lisamise asju nagu päringukeele lasta kliendid Accessi andmed kasulik viisil. (OData kohta leiate lisateavet teemast [Tutvustus OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Kuna Graph API abil saab kasutajate vaheliste seoste kohta see võimaldab rakenduste mõista social graafik, mis on manustatud Azure AD skeemi organisatsiooni (mistõttu seda nimetatakse Graph API). Ja autentida ise Azure AD Graph API taotluste, rakendus kasutab OAuth 2.0.

Kui ettevõtte ei kasuta Windows Server Active Directory - ei ole kohapealsed serverid või domeenide -, sõltub üksnes pilveteenuses rakendusi, mis kasutavad Azure AD, vaid selle pilve kataloogi kasutamine annaks ettevõtte kasutajate ühekordse sisselogimise neile kõigile. Veel ajal iga päev saab rohkem levinud stsenaariumi, enamiku organisatsioonide kasutada asutusesisese loodud Windows Server Active Directory domeenid. Azure AD on kasulik roll esitamiseks siin samuti, nagu [Joonisel](#fig3) 3.

![Azure Active Directory Virtual kohapeal](./media/identity/identity_03_AD.png)
<a id="fig3"></a>joonis 3: ettevõtte saate liita Windows Server Active Directory Azure Active Directory anda oma kasutajate ühekordse sisselogimise SaaS rakendused.

Selle stsenaariumi korral asutuses B kasutaja soovib SaaS rakenduse avamiseks. Enne, kui ta on see, peab ettevõtte nimistust administraatorid loovad federation seose Azure AD AD FS-i abil, nagu joonisel näha. Nende administraatorid tuleb konfigureerida ka ettevõtte kohapealse Windows Server AD- ja Azure AD vaheline andmete sünkroonimine. See kopeerib automaatselt kasutaja ja rühma teabe kohapealse kataloogi Azure AD. Pange tähele, milline see võimaldab:, organisatsiooni ulatub oma kohapealse kataloogi pilve. Windows Server AD ja Azure AD nii ühendades annab organisatsiooni kataloogiteenusest, mida saab hallata ühtse tervikuna ajal lahenenud on jalajälg asutusesiseselt ja pilves.

Azure AD kasutamiseks kasutaja esmalt sisse logib oma kohapealse Active Directory domeeni nagu tavaliselt (samm 1). Kui ta proovib juurde pääseda rakenduse SaaS (samm 2), protsessi federation tulemuste Azure AD emissiooni teda märgiks selle rakenduse (samm 3). (Federation toimimise kohta leiate lisateavet teemast [taotlusepõhise identiteedi Windowsi jaoks: tehnoloogiad ja stsenaariumid](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Enne selle märgiks sisaldab teavet, mille abil tuvastatakse kasutaja ning see on digitaalselt allkirjastatud Azure AD. Seejärel saadetakse see luba SaaS rakendus (samm 4), mis on luba allkirja kinnitatakse ning kasutab selle sisu (juhis 5). Eelmises näites, SaaS, rakenduse abil saate Graph API lugege lisateavet selle kasutaja, kui on vaja (samm 6).

Täna, Azure AD pole täielik asendamine kohapealse Windows Server AD. Nagu juba mainitud, on lihtsam skeemi cloud kataloogis ja seda pole asju, näiteks rühmapoliitika, võimalus talletada teavet masinad ja kasutajatugi LDAP. (Tegelikult Windowsi arvuti ei saa konfigureerida nii, et lasta kasutajatel midagi, kuid Azure AD abil sisse logida – see pole toetatud stsenaariumi.) Selle asemel algse eesmärkide Azure AD kaasata lastes ettevõtte kasutajate Accessi rakenduste pilves, säilitades kasutajanime ja hülgamine kohapealse kataloogi administraatorid: iga SaaS rakendust oma asutuses on kasutusel oma kohapealse kataloogi sünkroonimine käsitsi. Aja jooksul, aga eeldavad selle kataloogi pilveteenuses suurema hulga stsenaariumid käsitlema.

## <a name="ac"></a>Kasutades Azure Active Directory juurdepääsu reguleerimine

Pilvepõhist tehnoloogiad saab kasutada mitmesuguseid probleeme lahendada. Azure Active Directory saate anda ettevõtte kasutajate ühekordse sisselogimise mitme SaaS rakenduste, näiteks. Kuid identiteedi tehnoloogiad pilveteenuses saab kasutada ka muid võimalusi.

Oletagem näiteks, et rakendus soovib selle kasutajatel sõned mitme *identiteedipakkujad (IdPs)*väljastatud sisse logima. Paljude erinevate identiteedipakkujad olemas täna, Facebooki, Google, Microsoft ja teised, sh ja rakenduste sageli kasutajad logige sisse, kasutades ühte need identiteedid. Miks peaks rakenduse vaeva säilitada oma kasutajate ja paroolide loendit, kui selle asemel saate toetuvad identiteedid, mis on juba olemas? Olemasoleva identiteedid vastu võtmist teeb elu lihtsamaks nii kasutajad, kellel on üks vähem kasutajanimi ja parool meeles pidada, kui inimesed, kes luua rakenduse, kes pole enam vaja säilitada oma loendite kasutajanimed ja paroolid.

Kuid ajal iga identiteedipakkuja probleemid mingi luba, nende sõned ei standard - iga IdP on oma vormingusse. Lisaks nende sõned teave pole ka standardse. Rakendus, mis soovib aktsepteerida sõned väljastatud öelda, Facebooki, Google ja Microsoft on tegeleda kirjutamise koodiga käsitlema iga neid vorminguid.

Kuid miks? Miks ei saa luua hoopis ühe Turbeloa vormingu levinud kujutis identiteedi teavet andvate andmiseks? Seda moodust oleks elu lihtsam arendajad luua rakendusi, kuna nüüd peavad käsitlema ainult ühte tüüpi Turbeloa. Azure Active Directory juurdepääsu reguleerimine see täpselt, esitada andmiseks pilveteenuses mitmesuguse sõned töötamiseks. [Joonisel 4](#fig4) on näidatud, kuidas see toimib

![Azure Active Directory Virtual kohapeal](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>joonis 4: Azure Active Directory juurdepääsu reguleerimine hõlbustab rakenduste identiteedi sõned välja erinevate identiteedipakkujad kinnitamiseks.

Protsess algab, kui kasutaja proovib juurde pääseda rakenduse brauseri kaudu. Rakendus suunab teda mõne IdP oma valikut (ja rakenduse ka loodab). Ta kontrollib ise selle IdP, näiteks sisestades kasutajanime ja parooli (samm 1) ja ka IdP tagastab märgiks, mis sisaldab teavet tema (samm 2).

Nagu joonisel on näidatud, juurdepääsu reguleerimine toetab mitmesuguseid eri pilvepõhist ümberasustatud isikule, sh Google, Yahoo, Facebooki, Microsoft (varem Windows Live ID) ja mis tahes OpenID teenusepakkuja loodud kontod. Samuti toetab Azure Active Directory abil loodud identiteedid ja AD FS Windows Server Active Directory federation kaudu. Eesmärk on katta kõige sagedamini kasutatav identiteedid täna, kas need on antud sisepagulaste reintegreerimine pilveteenuses või kohapealse.

Kui kasutaja brauseris on mõne IdP luba tema valitud IdP kaudu, saadetakse selle Turbeloa juurdepääsu reguleerimine (samm 3). Juurdepääsu reguleerimine kinnitatakse luba, alustades seda kindlasti, et see tõesti väljastanud selle IdP ja seejärel loob uue loa selle rakenduse jaoks määratletud reeglite kohaselt. Azure Active Directory, nt juurdepääsu reguleerimine on mitme rentniku teenus, kuid on rentnike jaoks on rakendused, mitte kliendi ettevõtted. Konkreetse rakenduse saate avada oma nimeruum, nagu joonisel näha ja saate määratleda erinevate reeglite kohta autoriseerimine ja palju muud.

Reegleid lasta rakenduse administraatori määrata, kuidas peaks sõnet erinevate sisepagulaste reintegreerimine ümber on luba juurdepääsu reguleerimine. Näiteks kui erinevate sisepagulaste reintegreerimine kasutada erinevaid tähistav kasutajanimed, juurdepääsu reguleerimine reeglid saate muuta kõik need üheks levinum kasutajanimi. Juurdepääsu reguleerimine saadab seejärel selle uue loa tagasi brauseri (samm 4), mis esitab taotluse (juhis 5). Kui see on luba juurdepääsu reguleerimine, rakenduse kinnitab, et see luba tõesti on välja andnud juurdepääsu reguleerimine, siis kasutab seda teavet see sisaldab (samm 6).

Kuigi see protsess võib tunduda veidi keeruline, see tegelikult muudab elu märkimisväärselt lihtsam rakenduse looja. Mitte toime mitmesuguse sõned, mis sisaldavad erinevat teavet, nõus rakenduse identiteedid välja mitme identiteedipakkujad ajal ei saa ikka veel ühe luba tuttavad teabega. Ka, selle asemel, et nõuda konkreetse rakenduse tuleb konfigureerida erinevate sisepagulaste reintegreerimine usaldada, hoopis säilitatakse nende seoste usalda juurdepääsu reguleerimine – rakenduse tuleb ainult usalda seda.

See on vaja rõhutada, et midagi juurdepääsu reguleerimine on seotud Windows – seda võib kasutada ka lihtsalt Linuxi rakendus, mis aktsepteeritud ainult Google ja Facebook identiteedid. Ja isegi juhul, kui juurdepääsu reguleerimine on osa pere Azure Active Directory, mõelge selle täiesti eraldi teenust, mis eelmises jaotises kirjeldatud. Kuigi mõlemad tehnoloogiad töötada identiteedi, nad probleemide erinevad (kuigi Microsoft ütles, et see eeldab, et integreerida kahe mingil hetkel).

Identiteedi töötamine on oluline peaaegu iga rakenduses. Juurdepääsu reguleerimine eesmärk on lihtsam arendajatel luua rakendusi, mis aktsepteerida mitmesuguse identiteedipakkujad identiteeti. Pannes selle teenuse pilves, Microsoft on seda teha kättesaadavaks mis tahes rakendus, mis tahes platvormi.

##<a name="about-the-author"></a>Autorist

David Chappell on põhisumma Chappell ja äripartnerite [www.davidchappell.com](http://www.davidchappell.com) Californias San Francisco.
