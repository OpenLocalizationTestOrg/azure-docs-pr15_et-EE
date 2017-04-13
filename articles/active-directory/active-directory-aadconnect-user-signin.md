<properties
    pageTitle="Azure'i AD-ühenduse: Kasutaja sisselogimine | Microsoft Azure'i"
    description="Azure'i AD-ühendus kasutaja sisselogimine jaoks kohandatud sätteid."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure'i AD ühenduse kasutaja sisselogimise suvandid

Azure'i AD-ühendus võimaldab kasutajatele asutusesisese ja pilvepõhise ressursse, mis on sama paroolide kasutamise logida. Selles artiklis kirjeldatakse olulisi põhimõtet iga identiteedi mudel aitab teil valida identiteeti soovite kasutada oma sisselogimine Azure AD.

Kui olete juba tuttav Azure AD identiteedi mudeli ja soovite teatud meetodi kohta rohkem teada saada, klõpsake lihtsalt all asjakohane teema.

* [Parooli sünkroonimine](#password-synchronization)
* [Ühendatud SSO (ADFS) abil](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>Kasutaja sisselogimine meetodi valimine
Jaoks enamikule asutustest, kellele soovite lihtsalt lubamine kasutaja logige teenusekomplekti Office 365, SaaS rakenduste ja muude Azure AD põhineva, parooli sünkroonimise vaikevalik on soovitatav.
Mõni ettevõte on siiski teatud põhjused kaudu ühendatud sisselogimise suvand, nt AD FS-i.  Järgmised:

- Teie ettevõttel on juba AD FS-i või mõne 3 juurutatud federation pakkuja poole.
- Teie turbepoliitika ei luba sünkroonimise parooli hashes pilveteenusesse
- Vajate, et kasutajatele, kellel sujuvalt SSO (ilma paroolide palub) kui juurdepääs pilveteenuses ressursse domeeni liitunud masinad ettevõtte võrgus
- Vajate mõne kindla võimaluste AD FS-i on
    - Kohapealse mitmikautentimise kolmanda osapoole pakkuja või kiipkaardiga kaartide (kolmanda osapoole MFA pakkujad Windows Server 2012 R2 AD FS-i teave) abil
    - Active Directory integreerimise funktsioone nagu pehmed konto blokeeringu või AD parooli ja töö tundi poliitika
    - Tingimusvormingu juurdepääsu nii kohapealse ja Pilveteenusest seadme registreerimine, Azure'i AD-ühendus või poliitikate Intune MDM-i abil

### <a name="password-synchronization"></a>Parooli sünkroonimine
Parooli sünkroonimise, hashes kasutaja paroolid on sünkroonitud kohapealse Active Directoryst Azure AD.  Parooli muutmise või kohapealne lähtestada, uued paroolid sünkroonitakse kohe Azure AD nii, et kasutajad saavad alati abil sama parool puhul pilveteenuste ressursse kohapealse sarnaselt.  Paroolid on kunagi saadetakse Azure AD ega talletatud Azure AD avatekstina.
Parooli sünkroonimise saab kasutada koos parooli allahindluse lubamiseks ise teenuse parooli lähtestamise Azure AD.

<center>![Pilveteenuse](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[Lisateavet parooli sünkroonimine](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Windows Server 2012 R2 serveripargi kasutamine uude või olemasolevasse AD FS liiduserveri
Ühendatud sisselogimise kohta, kus kasutajate saan sisse logida vastavalt Azure AD Services kohapealse paroolide ja samal ajal ettevõtte võrgus, ilma et peaksite oma parooli uuesti.  AD FS liiduserveri suvandi võimaldab kasutuselevõtuks uue või olemasoleva AD FS Windows Server 2012 R2 serveripargi määrata.  Kui soovite määrata mõne olemasoleva pargi, Azure'i AD-ühenduse konfigureerib usalda oma serveripargi ja Azure AD vahel, nii, et teie kasutajad saavad logida.

<center>![Pilveteenuse](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Windows Server 2012 R2 AD FS Liiduserveri juurutamiseks peate järgmist

Kui võtate kasutusele uue pargi:

- Windows Server 2012 R2 server oleva liiduserveri.
- Windows Server 2012 R2 server teenuserakenduse puhverserverit.
- pfx faili SSL-serdi ettenähtud federation teenuse nimi, nt fs.contoso.com.

Kui olete uus serveripargi juurutamine või mõne olemasoleva pargi abil:

- Kohaliku administraatori identimisteave federation serveritesse.
- Kohaliku administraatori identimisteave klõpsake mis tahes töörühm (mitte-domeen liitunud) serverites, kuhu kavatsete juurutada Web rakenduse puhverserveri roll.
- Viisardi käivitada seade peab olema mis tahes muud seadmed, kuhu soovite installida AD FS-i või Web rakenduse puhverserveri kaudu Windows kaughalduse ühendust luua.

[SSO konfigureerimine AD FS-i abil](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Varasema versiooni AD FS-i abil sisse logida või kolmanda osapoole lahendus

Kui on juba loodud cloud Logi sisse varasema versiooniga AD FS-i (nt AD FS 2.0) või muude tootjate federation pakkuja, saate vahele jätta kasutaja sisselogimine konfiguratsiooni Azure'i AD-ühenduse kaudu.  See võimaldab teil saada uusima sünkroonimise ja muude võimaluste kohta Azure'i AD-ühenduse endiselt kasutades oma olemasoleva lahenduse Logi sisse.

[Azure'i AD kolmanda osapoole federation ühilduvuse loend](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Kasutaja sisselogimine ja kasutaja turvasubjektinimi (UPN)

### <a name="understanding-user-principal-name"></a>Teave kasutaja turvasubjektinimi

Active Directorys on vaikimisi UPN-i järelliite, kus luuakse kasutajakonto domeeni DNS-i nimi. Enamasti on see Internetis ettevõtte domeen on registreeritud domeeninimi. Siiski saate lisada rohkem UPN-i sufiksid Active Directory domeenid ja Usaldused abil.

Selle kasutaja UPN-i vorming on username@domai. Näiteks võib olla contoso.com kasutaja John active directory jaoks UPN-i john@contoso.com. Selle kasutaja UPN-i põhineb RFC 822. Kuigi UPN-i ja e-posti sama vormingu, kasutaja UPN-i väärtus võib või ei pruugi olla võrdne kasutaja meiliaadress.

### <a name="user-principal-name-in-azure-ad"></a>Kasutaja turvasubjektinimi Azure AD

Azure'i AD-ühendus viisard kasutab atribuudi userPrincipalName või abil saate määrata kasutada asutusesisesest kasutaja turvasubjektinimi Azure AD atribuut (jaotises kohandatud installi). See on väärtus, mida kasutatakse Azure AD sisselogimine. Kui kasutaja turvasubjektinimi atribuudi väärtus ei vasta kinnitatud Domeen Azure AD, siis Azure AD asendab see vaikimisi. onmicrosoft.com-i väärtust.

Iga Azure Active Directory kataloogis kaasas sisseehitatud domeeninimi contoso.onmicrosoft.com vorm, mis võimaldab Azure'i või muude Microsofti teenuste kasutamist alustada. Saate parandada ja sisselogimine kohandatud domeenide kasutamise lihtsustamiseks. Lisateavet kohandatud domeeni Azure AD nimed ja kuidas kontrollida domeeni, lugege [Lisa oma kohandatud domeeni nime Azure Active Directory](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure'i AD sisselogimise konfigureerimine

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure'i AD sisselogimise konfiguratsiooni Azure'i AD-ühenduse
Azure'i AD sisselogimine sõltub sellest, kas Azure AD on sobitada kasutaja turvasubjektinimi järelliite sünkroonita mõne kohandatud domeeni, kinnitatud kataloogis Azure AD kasutaja. Azure'i AD-ühendus pakub Azure AD sisselogimise sätete konfigureerimisel abi, et kasutaja sisselogimine pilveteenuses sarnaneb kohapealse versiooni kohta. Azure'i AD-ühendus on loetletud UPN-i järelliited jaoks soovitud (t) määratletud proovib vastavad need kohandatud domeeniga Azure AD ja aitab teil vastav toiming, mida tuleb arvesse võtta.
Azure AD sisselogimislehel UPN-i järelliited kohapealse Active Directory jaoks määratletud välja loendid ja kuvab vastava iga järelliite suhtes. Oleku väärtused võib olla üks selle all:

| Olek | Kirjeldus | Vaja midagi teha |
|:----|:----|:----|
|Kinnitatud| Azure'i AD-ühendus leidnud sobitamine kinnitatud Domeen Azure AD. Kõik selle domeeni kasutajad saaksid sisse logida kohapealse mandaadi abil| Midagi on vaja |
|Kinnitamata| Azure'i AD-ühendus leiaksid Azure AD kattuvad kohandatud domeeni, kuid see on kinnitatud. UPN-i järelliite selle domeeni kasutajad muutub vaikesätted. onmicrosoft.com-i järelliite pärast kui domeen on kinnitatud ei sünkrooni. | Kohandatud domeeni kinnitamine Azure AD. [Lisateave](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Ei lisata| Azure'i AD-ühendus ei leia UPN-i järelliite vastav kohandatud domeeni. UPN-i järelliite kasutajate sellelt domeenilt muutub vaikesätted. onmicrosoft.com, kui domeen on lisatud ja Azure verifeid.| Lisada ja kinnitada UPN-i järelliite [Lisateave](./active-directory-add-domain.md) vastav kohandatud domeeni|

Azure'i AD sisselogimislehel on loetletud UPN-i suffix(s), Azure AD praeguse oleku kontrollimine kohapealse Active Directory ja vastav kohandatud domeeni jaoks määratletud. Kohandatud installi, saate nüüd atribuuti kasutaja turvasubjektinimi lehel **Azure AD Logi sisse** .

![Azure'i AD sisselogimisleht](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Võite klõpsata nuppu Värskenda uuesti tuua kohandatud domeenide Viimane olek Azure AD.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Kasutaja turvasubjektinimi Azure AD-atribuut valimine

UserPrincipalName - atribuut userPrincipalName on kasutaja hakkab kasutama, kui nad sisse logima Azure AD atribuut ja Office 365. Domeenide kasutanud, nimetatakse ka UPN-järelliite, tuleks enne kasutajad on sünkroonitud Azure AD kontrollida. Tungivalt soovitatakse vaikimisi atribuut userPrincipalName säilitada. Kui see atribuut on marsruuditavaid ja ei saa kontrollida, siis on võimalik valida mõne muu atribuut, näiteks e-posti, kui atribuut, hoides sisselogimise ID Seda nimetatakse alternatiivse ID. Alternatiivne ID atribuudi väärtus järgima standardit RFC822. Alternatiivsete-ID-d saab kasutada nii parooli ühekordse sisselogimise (SSO) ja federation SSO lahendusena Logi sisse.

>[AZURE.NOTE] Alternatiivne ID abil ei ühildu kõik Office 365 teenustest. Lisateabe saamiseks vaadake [konfigureerimise alternatiivse sisselogimise ID](https://technet.microsoft.com/library/dn659436.aspx).

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Erinevate kohandatud domeeni Ühendriike ja mõju Azure sisselogimine
On väga oluline mõista kohandatud domeeni riikide Azure AD kataloogi vahel seose ja UPN-i sufiksid määratletud kohapealse. Andke meile läbida erinevate võimalike Azure sisselogimine kogemusi, kui häälestate sycnhronization Azure AD Connnect abil.

Allpool leiate Oletame, et saaksime on seotud UPN-i järelliite contoso.com, mida kasutatakse kohapealse kataloogi raames UPN-i, näiteks user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Kiire sätted / parooli sünkroonimine
| Olek         | Mõju kasutuskogemusega Azure sisselogimine |
|:-------------:|:----------------------------------------|
| Ei lisata | Sel juhul pole kohandatud domeeni contoso.com on lisatud Azure AD kataloogi. Kasutajad, kellel on UPN-i kohapealse ning järelliide @contoso.com, ei saa kasutada oma kohapealse UPN-i Azure sisse logima. Selle asemel on kasutada uut UPN-i neile esitatud Azure AD, lisades Azure AD vaikekataloogi järelliide. Oletagem näiteks, et kasutajatel Azure AD directory azurecontoso.onmicrosoft.com asutusesisese kasutaja sünkroonitavate user@contoso.com UPN ja antakseuser@azurecontoso.onmicrosoft.com|
| Kinnitamata | Sel juhul meil kohandatud domeeni contoso.com, Azure AD kataloogi lisanud, kuid see pole veel kinnitatud. Kui teil minna sünkroonimise kasutajad ilma kinnitatava domeeni, siis kasutajad määratakse uus UPN-i Azure AD, nagu seda tegi ei lisata stsenaariumi järgi.|
| Kinnitatud | Sel juhul meil kohandatud domeeni contoso.com juba lisanud ja Azure AD jaoks UPN-i järelliite kinnitatud. Kasutajad saaksid kasutada oma kohapealse kasutajanimi, nt user@contoso.com, sisse logima Azure pärast seda, kui need on sünkroonitud Azure AD|

###### <a name="ad-fs-federation"></a>AD FS Liiduserveri
Ei saa luua mõne federation vaikesätetega. onmicrosoft.com domeeni Azure AD või kinnitamata domeeni, Azure AD. Kui teil on viisard Azure'i AD-ühenduse, kui valite mõne kinnitamata domeeni lisamine teenusekomplektiga loomiseks klõpsake Azure'i AD-ühenduse palub teil vajalikud kirjed tuleb luua, kui domeeni majutab DNS-i abil. Lisateabe saamiseks vaadake [siin](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Kui valisite kasutaja sisselogimise suvand nimega "AD FS Liiduserveri", peab olema kohandatud domeeni jätkamiseks loomine on Domeeniliit Azure AD. Meie arutelu see tähendab, et meil kohandatud domeeni contoso.com, lisatud Directorys Azure AD.

| Olek         | Mõju kasutuskogemuse Azure sisselogimine |
|:-------------:|:----------------------------------------|
| Ei lisata | Sel juhul Azure'i AD-ühenduse ei leia sobitamine kohandatud domeeni jaoks UPN-i järelliite contoso.com Azure AD nimistust. Tuleb teil lisada kohandatud domeeni contoso.com, kui peate sisse logima oma kohapealse UPN-i nagu AD FS-i abil kasutajate user@contoso.com.|
| Kinnitamata | Sel juhul Azure'i AD-ühenduse palub teil asjakohased üksikasjad kohta, kuidas saate hiljem domeeni kinnitamine|
| Kinnitatud | Sel juhul võite minna ilma mis tahes täiendavaid toimingu konfiguratsioon|  

## <a name="changing-user-sign-in-method"></a>Kasutaja sisselogimise viisi muutmine

Saate muuta kasutaja sisselogimine meetod Federation parooli sünkroonimise abil tööülesannete avaialble Azure'i AD-ühenduse pärast algse konfiguratsiooni Azure'i AD-ühenduse viisardi abil. Käivitage viisard Azure'i AD-ühenduse uuesti ja esitatakse loendi Tööülesanded, mida saate teha. Valige **Muuda kasutajate sisselogimist** tööülesannete loend. 

![Muuta kasutaja sisselogimine"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Järgmisel lehel palutakse Azure AD identimisteabe ette.

![Azure AD ühenduse loomine](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Valige lehel **kasutajate sisselogimist** **Parooli sünkroonimine**. See muudab kataloogi kaudu ühendatud hallatavate üks.

![Ühenduse loomine Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Kui teete ainult ajutine vahetamise parooli sünkroonimise, siis märkige **teisendada Kasutajakontod**. Valik ei kontrolli viib iga kasutaja muutmise ühendatud ja võib kuluda mitu tundi.
  
## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedi ja Azure Active Directory integreerimine](active-directory-aadconnect.md).

Lisateave [Azure'i AD-ühenduse: kujundada põhimõtet](active-directory-aadconnect-design-concepts.md)


