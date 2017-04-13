<properties
    pageTitle="Azure'i AD-ühendus: Kohandatud installi | Microsoft Azure'i"
    description="Selle dokumendi üksikasjad kohandatud installisuvandite Azure'i AD-ühenduse. Järgmiste juhiste abil saate installida Active Directory Azure'i AD-ühenduse kaudu."
    services="active-directory"
    keywords="mis on Azure AD-ühenduse, Active Directory, nõutav Azure AD komponentide installimine"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Kohandatud installi Azure'i AD-ühenduse
Azure'i AD-ühendus **kohandatud sätteid** soovite installimine rohkem suvandeid saate kasutada. Seda kasutatakse, kui teil on mitu kogumit või kui soovite konfigureerida valikulised funktsioonid, mis ei hõlma kiire installi. Seda kasutatakse igal juhul, kui [**kiire**](active-directory-aadconnect-get-started-express.md) installisuvandit ei vasta teie juurutamise või topoloogia.

Enne alustamist installimist Azure'i AD-ühenduse, veenduge, et alla [laadida Azure'i AD-ühenduse](http://go.microsoft.com/fwlink/?LinkId=615771) ja täielik eelse nõutavad etappide [Azure'i AD-ühenduse: eeltingimuste ja riistvara](../active-directory-aadconnect-prerequisites.md). Lisaks veenduge, et teil on vaja kontod, mis on saadaval, nagu on kirjeldatud [Azure'i AD-ühenduse kontod ja õigused](active-directory-aadconnect-accounts-permissions.md).

Kui kohandatud sätted ei vasta teie topoloogia, näiteks täiendamine DirSync, vt stsenaariumid teiste [seotud dokumentidele](#related-documentation) .

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Azure'i AD-ühenduse kohandatud sätteid installimine

### <a name="express-settings"></a>Kiire sätted
Sellel lehel nuppu **Kohanda** installi kohandatud sätteid.

### <a name="install-required-components"></a>Nõutavate komponentide installimine
Kui installite sünkroonimise teenuseid, saate jätta valikuline konfigureerimine jaotises märkimata ja Azure'i AD-ühenduse häälestab kõik automaatselt. See häälestab SQL Server 2012 Express LocalDB eksemplari, sobiv rühmade loomine ja õiguste määramine. Kui soovite vaikesätete muutmiseks, saate järgmise tabeli mõista valikuline konfigureerimine suvandid, mis on saadaval.

![Nõutavad komponendid](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Valikuline konfigureerimine  | Kirjeldus
------------- | -------------
Olemasoleva SQL serveri kasutamine | Saate määrata SQL serveri nimi ja eksemplari nimi. Valige see suvand, kui teil on juba andmebaasi server, mida soovite kasutada. Sisestage järgneb koma ja pordi number **Eksemplari** nimi, kui SQL serveris ei ole lubatud sirvimise eksemplari nimi.
Teenuse konto kasutamine | Azure'i AD-ühenduse loob vaikimisi kohaliku teenusekonto sünkroonimise teenuste kasutamiseks. Parool on loodud automaatselt ja tundmatud installimist Azure'i AD-ühenduse isikule. Kui kasutate serveri SQL server või puhverserverit, mis nõuab autentimist kasutada, peate teenuse konto Domeen ja parooli. Sel juhul sisestage teenuse konto, mida soovite kasutada. Veenduge, et kasutaja installimine on SQL-is on SA nii, et teenusekonto sisselogimist saab luua. Leiate [Azure'i AD-ühenduse kontod ja õigused](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Saate määrata kohandatud sünkroonimine | Azure'i AD-ühenduse loob vaikimisi nelja kohaliku serveri kui sünkroonimise teenused on installitud. Need rühmad on: administraatorite rühma, tehtemärkide rühma, Sirvi rühma ja parooli lähtestamiseks rühma. Saate määrata oma rühmade siin. Rühmade peab olema kohaliku serveris ja seda ei saa domeeni.

### <a name="user-sign-in"></a>Kasutaja sisselogimine
Pärast installimist nõutavad komponendid, teil palutakse valida oma kasutajate ühekordse sisselogimise meetod. Järgmises tabelis kirjeldatakse lühidalt saadaolevatest valikutest. Sisselogimise meetodite täieliku kirjelduse leiate teemast [kasutajate sisselogimist](../active-directory-aadconnect-user-signin.md).

![Kasutaja sisselogimine](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Ühekordse sisselogimise suvand | Kirjeldus
------------- | -------------
Parooli sünkroonimine | Kasutajad saavad sisse logida Microsofti pilveteenustega, nt Office 365, kasutades sama parool, mida nad kasutavad ka oma kohapealse võrgu. Kasutajate paroole, sünkroonitakse Azure AD nimega parooli räsi ja autentimine toimub pilveteenuses. Lisateabe saamiseks vaadake [parooli sünkroonimine](../active-directory-aadconnectsync-implement-password-synchronization.md) .
AD FS liiduserveri | Kasutajad saavad sisse logida Microsofti pilveteenustega, nt Office 365, kasutades sama parool, mida nad kasutavad ka oma kohapealse võrgu.  Kasutajad suunatakse oma kohapealse AD FS-i astme sisse logida ja autentimine toimub kohapealse.
Konfigureerimine | Ei, funktsioon on installitud ja konfigureeritud. Valige see suvand, kui teil on juba 3 tootja liiduserveri või mõne muu olemasoleva lahenduse kohas.

### <a name="connect-to-azure-ad"></a>Ühenduse loomine Azure AD
Sisestage Azure AD Kuva ühenduse üldadministraatori kontot ja parooli. Kui valisite eelmise lehekülje **AD FS Liiduserveri** , logige sisse kontoga domeeni kavatsete lubada ühinemise jaoks. Soovitus on vaikimisi domeeni **onmicrosoft.com** , mis on kaasas Azure AD kataloogi konto kasutamine.

Selle konto kasutatakse ainult Azure AD teenuse konto loomiseks ja ei kasutata, kui viisard on lõpule jõudnud.  
![Kasutaja sisselogimine](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Kui teie konto üldadministraator on lubatud MFA, siis peate sisestage parool uuesti sisselogimist hüpikaknas ja täielik MFA juures on keerukas. Juures on keerukas võib esitada kinnituskood või telefonikõne ajal.  
![MFA kasutaja sisselogimine](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Üldadministraator konto võib olla ka [Õigustega identiteetide haldus](../active-directory-privileged-identity-management-getting-started.md) lubatud.

Kui kuvatakse tõrketeade ja ühenduvuse probleeme, siis lugege teemat [ühendusprobleemide tõrkeotsing](../active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Lehtede jaotises sünkroonimine

### <a name="connect-your-directories"></a>Ühenduse loomine oma kataloogide
Ühenduse loomine Active Directory domeeni teenus, Azure'i AD-ühenduse peab piisavaid õigusi konto identimisteave. Saate sisestada domeeni osa NetBios või FQDN-vormingus, st FABRIKAM\syncuser või fabrikam.com\syncuser. Selle konto võib olla tavakasutaja konto, sest läheb vaja üksnes vaikeõigused loetuks. Siiski vastavalt stsenaariumile, peate veel õigused. Lisateavet leiate [Azure'i AD-ühenduse kontod ja õigused](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Ühenduse loomine kataloog](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure'i AD sisselogimise konfigureerimine
Sellel lehel saate vaadata kohal kohapealse UPN-i domeenis AD DS ja mis on kinnitatud Azure AD. Selle lehe võimaldab teil konfigureerida jaoks soovitud userPrincipalName atribuuti.

![Kinnitamata domeenid](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Iga domeeni, mis on tähistatud **Ei lisata** ja **Kinnitamata**üle. Veenduge, et need domeenid, mida kasutate on kinnitatud Azure AD. Kui olete kontrollinud oma domeeni, klõpsake nuppu Värskenda sümbol. Lisateavet leiate teemast [lisada ja kinnitada domeeni](../active-directory-add-domain.md)

**UserPrincipalName** - atribuut userPrincipalName on atribuut kasutajad kasutada Azure AD sisselogimisel ja Office 365. Domeenide kasutanud, nimetatakse ka UPN-järelliite, tuleks enne kasutajad on sünkroonitud Azure AD kontrollida. Microsoft soovitab säilitada vaikimisi atribuut userPrincipalName. Kui see atribuut on marsruuditavaid ja ei saa kontrollida, siis on võimalik valida mõne muu atribuut. Saate valida näiteks e-posti atribuut, kellel on ID. **Alternatiivne ID**tuntud teise atribuudi userPrincipalName kui abil. Alternatiivne ID atribuudi väärtus järgima standardit RFC822. Alternatiivsete-ID-d saab kasutada nii parooli sünkroonimise ja federation.

>[AZURE.WARNING]
Alternatiivne ID abil ei ühildu kõik Office 365 teenustest. Lisateabe saamiseks vaadake [konfigureerimise alternatiivse sisselogimise ID](https://technet.microsoft.com/library/dn659436.aspx).

### <a name="domain-and-ou-filtering"></a>Domeeni ja OU filtreerimine
Vaikimisi kõik domeenid ja organisatsiooniüksused sünkroonitakse. Kui määratud on mõni domeenide või organisatsiooniüksused, mida soovite sünkroonida Azure AD, saate valimise, need domeenid ja organisatsiooniüksused.  
![DomainOU filtreerimine](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) selle lehe viisardi konfigureerib domeeni põhinev filtreerimine. Lisateavet leiate teemast [domeeni põhinev filtreerimine](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering).

Samuti on võimalik, et mõnda domeeni ei ole tulemüüri piirangutest kättesaadav. Need domeenid on vaikimisi valimata ja on hoiatus.  
![Kättesaamatu domeenid](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Kui kuvatakse hoiatus, veenduge, et need domeenid on tõepoolest kättesaamatu ja hoiatus peaks.

### <a name="uniquely-identifying-your-users"></a>Kordumatult tuvastada teie kasutajad
Vastavaid üle metsade funktsioon võimaldab teil määrata, kuidas kasutajad oma AD DS metsast on esindatud Azure AD. Kasutaja võib teha ainult üks kord esindatud üle kõik metsade või teil on lubatud ja keelatud kontode kombinatsiooni. Kasutaja võib esindatud ka mõned metsades kontaktina.

![Kordumatud](./media/active-directory-aadconnect-get-started-custom/unique.png)

Säte | Kirjeldus
------------- | -------------
[Kasutajad on ainult esindatud üks kord üle kõik metsad](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Kõigi kasutajate luuakse Azure AD üksikuid objektina. Objektide on ühendatud metaversumi.
[E-posti atribuut](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | See suvand juhul, kui atribuuti mail on sama väärtuse erinevate metsade ühendab kasutajate ja kontaktide. Kasutage seda suvandit, kui teie kontaktid on loodud GALSync abil.
[ObjectSID ja msExchangeMasterAccountSID / msRTCSIP-OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | See suvand ühendab lubatud kasutaja on konto mets keelatud kasutaja ressursi mets. Selle konfiguratsiooni Exchange, nimetatakse lingitud postkasti. See suvand saab kasutada ka siis, kui kasutate Lynci ja Exchange'i ressursi mets pole.
sAMAccountName ja MailNickName | See suvand ühendab atribuutide, kus see eeldatakse, et kasutaja sisselogimise ID leiate.
Teatud atribuut | See suvand võimaldab valida oma atribuut. **Piirangud:** Veenduge, et valida juba leitav metaversumi atribuut. Kui valite kohandatud atribuudi (pole rakenduses metaversumi), ei saa viisard lõpule viia.

**Andmeallika ankur** - sourceAnchor atribuut on atribuut, mis on püsiv kestel ning kasutaja objekti. See on linkimise asutusesisese kasutaja kasutaja Azure AD primaarvõti. Kuna atribuuti ei saa muuta, peate kavandamine hea atribuuti kasutada. Hea testversiooni on FederatedIdentity. See atribuut on muutunud, v.a juhul, kui kasutaja liigub metsade/domeenide vahel. Mitme mets keskkonnas, kus teisaldamine kontod metsade, tuleb teise atribuuti kasutada, nt atribuudi selle Tabeliseose. Vältige atribuute, mida soovite muuta kui isik abiellub ka ülesanded. Te ei saa kasutada atribuudid on @-sign, nii, et e-posti ja userPrincipalName ei saa kasutada. Atribuut on ka tõstutundlik kui objekti teisaldamine metsad, veenduge, et säilitada ülemine/väiketähti. Binaarsed atribuudid on base64-kodeeringuga, kuid muud tüüpi atribuuti jäävad kodeerimata kujul. Federation stsenaariumid ja mõned Azure AD liideste, on ka see atribuut immutableID. Lisateavet allika ankur leiate [kujundus põhimõtet](../active-directory-aadconnect-design-concepts.md#sourceAnchor).

### <a name="sync-filtering-based-on-groups"></a>Sünkroonimine filtreerimine rühmade põhjal
Filtreerimine rühmade funktsiooni abil saate sünkroonida ainult väikese osa objektide jaoks pilootprojekti. Selle funktsiooni kasutamiseks selleks oma kohapealse Active Directory rühma loomine. Lisage kasutajad ja rühmad, mis tuleks sünkroonitud Azure AD isikuid otse. Hiljem saate lisada ja eemaldada kasutajaid selle rühma säilitamiseks objekte, mis peaks toimuma Azure AD loendit. Kõigi objektide sünkroonimiseks peab olema otsese rühma liige. Kasutajad, rühmad, kontaktide ja arvutites ja seadmetes peab olema otseste. Pesastatud rühma liikmelisusel on lahendatud. Kui lisate rühma liige ainult rühma lisamise ja pole selle liikmeid.

![Sünkroonimine filtreerimine](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
See funktsioon on mõeldud ainult katseprojekti juurutamise toetamiseks. Kasutage seda täiemahuline tootmine juurutamine.

Täiemahuline tootmine juurutamine, läheb seda raske säilitada ühte rühma ja kõigi objektidega sünkroonida. Selle asemel kasutage ühte meetoditest [konfigureerimine](../active-directory-aadconnectsync-configure-filtering.md)filtreerides.

### <a name="optional-features"></a>Valikulised funktsioonid
Kuva võimaldab valida oma teatud stsenaariumide valikulised funktsioonid.

![Valikulised funktsioonid](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Kui teil on DirSync või Azure AD sünkroonimine aktiivne, aktiveerige ühte tagasikirjutusega funktsioonide Azure'i AD-ühenduse.

Valikulised funktsioonid | Kirjeldus
------------------- | -------------
Exchange'i hübriidjuurutus | Exchange'i hübriidjuurutuse funktsioon võimaldab olemasolu Exchange'i postkaste asutusesiseselt ja Teenusekomplektis Office 365. Azure'i AD-ühendus on tagasi oma kohapealse kataloogi sünkroonimine määratud [atribuute](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) : Azure'i AD.
Azure'i AD rakendus ja atribuut filtreerimine | Sünkroonitud atribuutide määramine saab kohandada, võimaldades Azure AD rakendus ja atribuut filtreerimise. See suvand liidab kaks konfiguratsiooni lehtede viisard. Lisateabe saamiseks lugege teemat [Azure AD rakendus ja atribuut filtreerimine](#azure-ad-app-and-attribute-filtering).
Parooli sünkroonimine | Kui Domeeniliit valitud sisselogimise lahenduse, saate lubada see suvand. Parooli sünkroonimise saate kasutada siis varukoopia suvand. Lisateabe saamiseks lugege teemat [parooli sünkroonimine](../active-directory-aadconnectsync-implement-password-synchronization.md).
Parooli tagasikirjutusega | Parooli tagasikirjutusega võimaldades parooli muuta, mis on pärit Azure AD kirjutatakse tagasi kohapealse kataloogi. Lisateabe saamiseks lugege teemat [parooli halduse töötamise alustamine](../active-directory-passwords-getting-started.md).
Rühma tagasikirjutusega | Kui kasutate **Office 365 rühmade** funktsiooni, võib olla need esindatud oma kohapealse Active Directory rühma. See suvand on saadaval, kui teil on Exchange esita oma kohapealse Active Directory ainult. Lisateavet leiate teemast [rühma tagasikirjutusega](../active-directory-aadconnect-feature-preview.md#group-writeback).
Seadme tagasikirjutusega | Võimaldab teil tagasikirjutusega seadme objektide Azure AD oma kohapealse Active Directory juurdepääsu stsenaariumid. Lisateavet leiate teemast [lubamine seadme tagasikirjutusega rakenduses Azure'i AD-ühenduse](../active-directory-aadconnect-feature-device-writeback.md).
Atribuut Kataloogisünkroonimise laiend | Atribuut Kataloogisünkroonimise laiendid võimaldades sünkroonitakse määratud atribuute Azure AD. Lisateavet leiate teemast [Directory laiendid](../active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure'i AD rakendus ja atribuut filtreerimine
Kui soovite piirata millised atribuudid Azure AD sünkroonida, alustage, valides milliseid teenuseid te kasutate. Kui teete konfiguratsioonimuudatuste sellel lehel, uus teenus peab olema märgitud selgesõnaliselt taaskäivitamisel installimise viisard.

![Valikulised funktsioonid rakendused](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Sellel lehel põhjal teenuste eelmises juhises valitud, kuvatakse kõik atribuudid, mis on sünkroonitud. See loend on kõik objekti tüübid ei sünkroonita kombinatsiooni. Kui määratud on mõni kindla atribuut, mida soovite sünkroonida, võite nende atribuutide valiku tühistada.

![Valikulised funktsioonid atribuudid](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Atribuutide eemaldamine võib mõjutada funktsioone. Head tavad ja soovitused, vt [Atribuudid sünkroonitud](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### <a name="directory-extension-attribute-sync"></a>Atribuut Kataloogisünkroonimise laiend
Kohandatud atribuute, mis on teie asutuses või muude atribuutide Active Directory abil saate laiendada Azure AD skeemi. Selle funktsiooni kasutamiseks klõpsake nuppu **Directory laiend atribuut Sünkrooni** lehel **Valikulised funktsioonid** . Saate sünkroonida sellel lehel veel atribuute.

![Directory laiendid](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Lisateavet leiate teemast [Directory laiendid](../active-directory-aadconnectsync-feature-directory-extensions.md).

## <a name="configuring-federation-with-ad-fs"></a>AD FS liiduserveri konfigureerimine
Konfigureerimine AD FS-i Azure'i AD-ühenduse abil on lihtne vaid paari klõpsuga. Enne konfiguratsiooni on vajalik.

- Kaughalduse lubatud Domeeniliit serveris Windows Server 2012 R2 server
- Windows Server 2012 R2 server Web rakenduse puhverserveri lubatud kaughalduse abil
- SSL-serdi federation teenuse nimi kavatsete kasutada (nt sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>AD FS-i konfigureerimine eeltingimused
Soovite konfigureerida oma AD FS-i serveripargi Azure'i AD-ühenduse kaudu, kontrollige WinRM on lubatud serveri serverite. Lisaks läbida [tabelis](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap)3 – Azure'i AD-ühenduse ja Federation serverid/WAP pordid nõue.

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Saate luua uue AD FS-i pargi või kasutage AD FS olemasoleva pargi
Saate kasutada olemasolevaid AD FS pargi või soovi korral saate luua uue AD FS-i pargi. Kui valite uue konto loomiseks, mida peavad SSL-serdi esitama. Kui SSL-sert on kaitstud parooliga, küsitakse teilt parooli.

![AD FS-i serveripargi](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Kui otsustate kasutada AD FS olemasoleva pargi, mida on võetud otse konfigureerimine AD FS-i ja Azure AD Kuva usalda seos.

### <a name="specify-the-ad-fs-servers"></a>Määrake AD FS-i serverid
Sisestage serverid, mida soovite installida AD FS-i. Saate lisada ühe või mitme serveri vastavalt oma vajadustele kavandamise võimsus. Selle konfiguratsiooni sooritamiseks liituma Active Directory kõikides serverites. Microsoft soovitab ühe AD FS server testi ja katseprojekti juurutuste installimisel. Seejärel lisamine ja juurutamine rohkem servereid käivitades uuesti pärast esialgne konfiguratsioon Azure'i AD-ühenduse skaleerimise vajadustele.

>[AZURE.NOTE]
Veenduge, et kõik teie serverid liitunud AD domeeni enne selle konfiguratsiooni.

![AD FS server](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Web teenuserakenduse puhverserverite määramine
Sisestage serverid, mida soovite oma veebirakenduse puhverserverite. Web rakenduse puhverserveri on juurutatud oma DMZ (Suhtevõrgu ees) ja pärit on Suhtevõrgu toetab. Saate lisada ühe või mitme serveri vastavalt oma vajadustele kavandamise võimsus. Microsoft soovitab ühe Web rakenduse puhverserveri testi ja katseprojekti juurutuste installimisel. Seejärel lisamine ja juurutamine rohkem servereid käivitades uuesti pärast esialgne konfiguratsioon Azure'i AD-ühenduse skaleerimise vajadustele. Soovitatav on samaväärne arv puhverserverid autentimine sisevõrgu vastavaks.

>[AZURE.NOTE]
<li> Kui konto, ei ole kohaliku administraatori AD FS-i serverid, siis küsitakse administraatori identimisteave.</li>
<li> Veenduge, et on HTTP-või HTTPS Ühenduvus Azure'i AD-ühenduse server ja Web rakenduse puhverserveri enne selle toimingu.</li>
<li> Veenduge, et on HTTP-või HTTPS Ühenduvus Web Application Server ja AD FS server lubamiseks taotluste autentimise flow kaudu.</li>

![Web App](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Teil palutakse sisestada mandaat, et veebiserver rakenduse saab luua turvalist ühendust AD FS server. Neid mandaate peavad olema kohaliku administraatori AD FS server.

![Puhverserver](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Teenuse AD FS-i teenuse konto määramine
AD FS-i teenus nõuab autentida kasutajate ja otsinguteabe kasutaja Active Directory domeeni teenuse konto. Toetada kahte tüüpi kontod:

- **Rühma hallatavate teenusekonto** - kasutusele Windows Server 2012 Active Directory domeeniteenused. Seda tüüpi konto osutab teenuseid, näiteks AD FS-i, värskendage konto parooli regulaarselt automaatselt ilma ühe konto. Kasutage seda suvandit, kui teil on juba Windows Server 2012 domeenikontrollerid domeen, mis kuuluvad teie AD FS-i serverid.
- **Domeeni kasutajakonto** – seda tüüpi konto jaoks on vaja sisestada parool ja parooli regulaarselt värskendada, kui parool muudab või aegub. Kasutage seda suvandit ainult siis, kui teil on Windows Server 2012 domeenikontrollerid domeen, mis kuuluvad teie AD FS-i serverid.

Kui teie valitud rühma hallatavate teenusekonto ja see funktsioon pole kunagi kasutatud Active Directory, küsitakse teilt ettevõtte administraatori identimisteave. Neid mandaate algatada klahv pood ja Active Directory funktsiooni.

![AD FS-i teenusekonto](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Valige Azure AD domeen, mida soovite liita
Selle konfiguratsiooni kasutatakse häälestamine AD FS-i ja Azure AD federation seos. See konfigureerib probleemi turvalisus sõned Azure AD AD FS-i ja konfigureerib Azure AD usaldusväärsuse sõned: AD FS-i astme. Sellel lehel saate ainult ühe domeeni konfigureerimine algse installi. Saate konfigureerida täiendavaid domeene hiljem Azure'i AD-ühenduse uuesti käivitada.

![Azure'i AD domeeni](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Ühinemise jaoks valitud Azure AD domeeni kinnitamine
Kui valite domeen on ühendatud, Azure'i AD-ühenduse pakub teile mõne kinnitamata domeeni kinnitamiseks vajaliku teabe. Vt [lisamine ja kinnitada domeeni](../active-directory-add-domain.md) kohta, kuidas kasutada seda teavet.

![Azure'i AD domeeni](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
Domeeni kinnitamine etapis konfigureerimine proovib AD-ühendus. Kui jätkate konfigureerimiseks lisamata vajalikud DNS-i kirjeid, viisardi ei ole võimalik konfigureerimise lõpuleviimiseks.

## <a name="configure-and-verify-pages"></a>Veenduge, et lehed ja konfigureerimiseks
Konfiguratsiooni juhtub sellel lehel.

>[AZURE.NOTE]
Enne installimise jätkamiseks ja kui olete konfigureerinud federation, veenduge, et olete konfigureerinud [nimelahendus liiduserveris jaoks](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).

![Kas olete valmis konfigureerimine](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Lavastus režiim
On võimalik setup uue sünkroonimine serveri paralleelselt lavastus režiim. See on toetatud ainult on üks sünkroonimine server eksportimise ühe directory pilveteenuses. Kuid kui soovite teisaldada teise serverisse, näiteks ühe töötava DirSync, siis saate lubada Azure'i AD-ühenduse sisse lavastus režiim. Kui lubatud, sync engine importimine ja sünkroonimine andmeid nagu tavaliselt, kuid see eksportida midagi Azure AD või AD. Funktsioonide parooli sünkroonimise ja parooli tagasikirjutusega on keelatud ajal lavastus režiim.

![Lavastus režiim](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

Lavastus režiimis, on võimalik sync engine vajalike muudatuste tegemine ja vaadake üle, mis on eksportida. Kui konfiguratsiooni sobib, käivitage uuesti installimise viisard ja keelamine lavastus režiim. Andmed on nüüd eksporditakse Azure AD selles serveris. Veenduge, et keelata teiste server samal ajal, nii et ainult üks server on aktiivselt eksportimine.

Lisateavet leiate teemast [lavastus režiim](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Veenduge, et teie federation konfigureerimine
Azure'i AD-ühendus kinnitab teie DNS-i sätted, kui klõpsate nuppu Kinnita.

![Lõpuleviimine](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Kontrollige](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Peale järgmiste toimingute kinnitamine:

- Kinnitage, et saaksite sisse logida sisevõrgus domeeni ühendatud seadmes brauseri kaudu: ühenduse https://myapps.microsoft.com ja kontrollige sisselogimine sisse loginud kontoga. Sisseehitatud AD DS administraatori kontot ei sünkroonita ja ei saa kasutada kinnitamiseks.
- Kinnitage, et saaksite sisse logida kaudu soovitud Suhtevõrgu seadmest. Home arvutisse või mobiilsideseadmesse, ühenduse https://myapps.microsoft.com ja sisestama oma mandaadi.
- Kinnitage rikkaliku kliendi Logi sisse. Ühenduse loomine https://testconnectivity.microsoft.com, valige **Office 365** menüüd ja valige **Office 365 ühekordse sisselogimise Test**.

## <a name="next-steps"></a>Järgmised sammud
Kui installimine on lõpule jõudnud, logige välja ja uuesti sisse logida Windowsi sünkroonimise Service Manager või sünkroonimise reegli redaktori kasutamiseks.

Nüüd, kui teil on Azure'i AD-ühenduse installitud saate [kontrollida installimine ja litsentside määramine](../active-directory-aadconnect-whats-next.md).

Lisateavet need funktsioonid, mis olid lubatud installi: [vältida juhuslikku kustutab](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) ja [Azure'i AD-ühenduse seisund](../active-directory-aadconnect-health-sync.md).

Lugege lisateavet teemadest levinud: [ajasti ja kuidas sünkroonimise käivitamiseks](../active-directory-aadconnectsync-feature-scheduler.md).

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Seotud dokumendid

Teema: |  
--------- | ---------
Azure'i AD-ühendus ülevaade | [Teie kohapealse identiteetide integreerimine Azure Active Directory](../active-directory-aadconnect.md)
Installige Express sätete abil | [Kiire Azure'i AD-ühenduse installimine](active-directory-aadconnect-get-started-express.md)
DirSync täiendamine | [Uuendada Azure AD sünkroonimistööriist (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Installi kasutatud kontod | [Lisateavet leiate Azure'i AD-ühenduse kontod ja õigused](active-directory-aadconnect-accounts-permissions.md)
