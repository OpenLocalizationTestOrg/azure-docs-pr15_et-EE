<properties
    pageTitle="Active Directory Federation Services haldamine ja kohandamine Azure'i AD-ühenduse | Microsoft Azure'i"
    description="AD FS-i halduse Azure'i AD-ühenduse ja kasutajale AD FS-i sisselogimine Azure'i AD-ühenduse ja PowerShelli kohandamine."
    keywords="AD FS-i, ADFS-i, AD FS-i haldus AAD ühendust luua, ühendus, Logi sisse, AD FS-i kohandamine, usalda, O365, federation, tuginedes tootja parandamine"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory Federation Services haldus ja Azure'i AD-ühenduse kohandamine

Selles artiklis üksikasjad tööülesannete seotud Active Directory Federation Services (AD FS), mida saab teha, kasutades Microsoft Azure Active Directory ühendust ning muude AD FS-i põhitoimingud, mis võib olla vaja on AD FS pargi konfigureerimise lõpuleviimiseks.

| Teema: | Mida see hõlmab |
|:------|:-------------|
|**AD FS-i haldus**|
|[Usalda parandamine](#repairthetrust)| Parandamine federation usalda teenusekomplektiga Office 365 |
|[Lisage AD FS server](#addadfsserver) | Täiendava AD FS server ja AD FS-i serveripargis laiendamine|
|[Lisage AD FS-i web rakenduse puhverserver](#addwapserver) | Täiendava WAP server AD FS-i serveriparki laiendamine|
|[Ühendatud domeeni lisamine](#addfeddomain)| Ühendatud domeeni lisamine|
| **AD FS-i kohandamine**|
|[Kohandatud ettevõtte logo või illustratsiooni lisamine](#customlogo)| Ka AD FS-i sisselogimislehel ettevõtte logo ja joonisel kohandamine |
|[Lisage korral kirjeldus](#addsignindescription) | Sisselogimislehel kirjelduse lisamine |
|[AD FS-i nõude reeglid muutmine](#modclaims) | AD FS-i nõuete erinevaid federation stsenaariume muutmine |

## <a name="ad-fs-management"></a>AD FS-i haldus

Azure'i AD-ühendus pakub mitmesuguseid ülesandeid, mis on seotud AD FS-i, mida saab teha minimaalne kasutaja sekkumisel Azure'i AD-ühenduse viisardi abil. Kui teil installida Azure AD-ühenduse viisardit lõpule jõudnud, käivitage viisard uuesti, et teha täiendavaid toiminguid.

### Usalda parandamine<a name=repairthetrust></a>

Azure'i AD-ühendus saate otsida AD FS-i ja Azure Active Directory usalda praeguse seisundi ja võtta vastav usalda parandamine. Järgmiste juhiste abil oma Azure AD parandamine ja AD FS-i usalda.

1. Valige **Paranda AAD ja ADFS-i usalda** täiendavate tööülesannete loend.
![Parandamine AAD ja ADFS-i usaldusväärseks](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Klõpsake lehel **ühenduse Azure AD** mandaat üldadministraator pakuvad Azure AD ja klõpsake nuppu **edasi**.
![Azure AD ühenduse loomine](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Sisestage lehel **Kaugpöördusteenuse identimisteave** domeeni administraatori identimisteabe.
![Kaugpöördusteenuse identimisteave](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Pärast seda, kui klõpsate nuppu **edasi**, Azure'i AD-ühenduse serdi seisundi kontrollimine ja Kuva probleemidest.

    ![Olekusse järgmisi tunnistusi](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    **Konfigureerida** leht kuvatakse toiminguid, mis tehakse parandada usalda loendit.

    ![Kas olete valmis konfigureerimine](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Klõpsake nuppu Usalda parandada **installida** .

>[AZURE.NOTE] Azure'i AD-ühendus saab ainult parandamine või seadus on iseallkirjastatud serdid. Azure'i AD-ühendus, ei saa parandada kolmanda osapoole serdid.

### Lisage AD FS server<a name=addadfsserver></a>

> [AZURE.NOTE] Azure'i AD-ühendus nõuab PFX serdifail AD FS server lisamiseks. Seetõttu saab seda toimingut teha ainult siis, kui olete konfigureerinud AD FS-i serveripargi Azure'i AD-ühenduse kaudu.

1. Valige **Deploy täiendava Federation Server** ja klõpsake nuppu **edasi**.
![Täiendavad liiduserveri](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Klõpsake lehel **ühenduse Azure AD** sisestage mandaat üldadministraator Azure AD ja klõpsake nuppu **edasi**.
![Azure AD ühenduse loomine](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Sisestage domeeni administraatori identimisteave.
![Domeeni administraatori identimisteave](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure'i AD-ühendus küsib PFX-faili koos Azure'i AD-ühenduse oma uue AD FS-i serveripargi konfigureerimisel sisestatud parool. Klõpsake nuppu **Sisestage parool** parool ette PFX-fail.
![Serdi parooli](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![SSL-serdi määramine](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Sisestage lehel **AD FS -i serverid** serverinime või serveripargi AD FS-i lisada IP-aadress.
![AD FS server](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Klõpsake nuppu **Järgmine** ja läbida viimasel lehel kuvatakse **konfigureerimine** . Pärast serveripargis AD FS-i serverid lisamine on lõpetanud Azure'i AD-ühenduse, antakse teile kinnitamiseks ühendust suvand.
![Kas olete valmis konfigureerimine](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Installimine on lõpule jõudnud](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Lisage AD FS-i web rakenduse puhverserver<a name=addwapserver></a>

> [AZURE.NOTE] Azure'i AD-ühendus nõuab PFX serdifail web rakenduse puhverserveri lisada. Seetõttu saab seda toimingut teha ainult siis, kui olete konfigureerinud AD FS-i serveripargi Azure'i AD-ühenduse kaudu.

1. Valige loendist saadaolevad ülesanded **Juurutamine veebiteenuse rakenduse puhverserver** .
![Web rakenduse puhverserveri juurutamine](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Azure'i üldadministraator mandaat.
![Azure AD ühenduse loomine](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Lehel **Määrake SSL-serdi** ette registreerimisel konfigureerimine AD FS-i serveripargi koos Azure'i AD-ühenduse fail PFX parool.
![Serdi parooli](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![SSL-serdi määramine](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Server, millega lisatakse web rakenduse puhverserveri lisada. Kuna web rakenduse puhverserveri võib ühendatud pole domeeni, viisardi küsima administraatori identimisteave lisamisest server.
![Serveri haldus](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Sisestage lehel **puhverserveri usalda identimisteave** administraatori identimisteave puhverserveri konfigureerimine usaldate ja AD FS-i serveripargis esmane sisestamine.
![Puhverserveri usalda identimisteave](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. **Konfigureerida** lehel kuvatakse viisardi toimingud, mis tehakse.
![Kas olete valmis konfigureerimine](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Klõpsake **installida** konfiguratsiooni valmis. Pärast konfiguratsiooni on lõpule jõudnud, viisard pakub teile võimalust kontrollib ühenduvust serveritega. Klõpsake nuppu **Kinnita** Ühenduvus kontrollida.
![Installimine on lõpule jõudnud](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Ühendatud domeeni lisamine<a name=addfeddomain></a>

See on lihtne lisada domeeni domeeniliidu Azure AD Azure'i AD-ühenduse kaudu. Azure'i AD-ühendus lisab ühinemise jaoks domeeni ja muudab nõude reeglid kajastamiseks väljaandja õigesti, kui teil on mitu domeeni, mis on ühendatud Azure AD.

1. Ühendatud domeenis lisamiseks valige tööülesanne **lisada täiendava domeeni Azure AD**.
![Täiendavad Azure AD domeeni](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Viisardi järgmisel lehel ette Azure AD üldadministraator mandaat.
![Azure AD ühenduse loomine](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Sisestage lehel **Kaugpöördusteenuse identimisteabe** domeeni administraatori identimisteave.
![Kaugpöördusteenuse identimisteave](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Järgmisel lehel annab viisardi, millega saate liita kohapealse kataloogi Azure AD domeenide loendit. Valige loendist Domeen.
![Azure'i AD domeeni](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Kui otsustate domeeni, viisardi annab teile täiendavaid toiminguid, mis viivad viisard ja nende mõju konfiguratsiooni vajalikku teavet. Mõnel juhul, kui valite domeen, mida pole veel kinnitatud Azure AD, viisardi annab teile teavet, et saaksite kontrollida domeeni. Üksikasjalikumat teavet leiate teemast [lisamine Azure Active Directory oma kohandatud domeeni nime](active-directory-add-domain.md) .

5. Klõpsake nuppu **edasi**ja **valmis konfigureerida** lehel kuvatakse toimingud, mis Azure'i AD-ühenduse kuvatakse loendis. Klõpsake **installida** konfiguratsiooni lõpuleviimiseks.
![Kas olete valmis konfigureerimine](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS-i kohandamine

Järgmistes jaotistes on mõned levinud toimingud, mida tuleb teha oma AD FS-i sisselogimislehel kohandamisel üksikasjad.

### Kohandatud ettevõtte logo või illustratsiooni lisamine<a name=customlogo></a>

Kuvatakse lehel **sisselogimine** ettevõtte logo, saate muuta järgmine Windows PowerShelli cmdlet-käsk ja süntaks.

> [AZURE.NOTE] Soovitatav mõõtmed logo on 260 x 35 @ 96 dpi, mille failimaht on väiksem kui 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] *TargetName* parameeter on nõutav. Mis on välja AD FS vaikekujunduse nimeks vaikimisi.


### Lisage korral kirjeldus<a name=addsignindescription></a>

**Sisselogimislehel**sisselogimislehel kirjelduse lisamiseks kasutage järgmine Windows PowerShelli cmdlet-käsk ja süntaks.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### AD FS-i nõude reeglid muutmine<a name=modclaims></a>

AD FS-i toetab rikkalike taotluste keel, mille abil saate luua kohandatud nõude reeglid. Lisateabe saamiseks vt [Osa taotluste reegli keel](https://technet.microsoft.com/library/dd807118.aspx).

Järgmistes jaotistes kirjeldatakse, kuidas saate kirjutada mõne stsenaariumi, mis on seotud Azure AD kohandatud reeglid ja AD FS liiduserveri.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Tingimusvormingu viibides atribuut väärtuse püsiv ID

Azure'i AD-ühendus võimaldab määrate atribuudi kasutada andmeallika ankur objektid on sünkroonitud Azure AD. Kui kohandatud atribuudi väärtus on pole tühi, mida soovite mõne püsiv ID nõude probleemi. Näiteks võib valige **ms-ds-consistencyguid** allika ankur atribuuti ja soovite probleemi **ImmutableID** nimega **ms-ds-consistencyguid** juhuks, kui atribuut on väärtus selle vastu. Kui ei ole väärtus atribuudi suhtes, probleemi **FederatedIdentity** püsiv ID-ga.  Saate luua kohandatud taotluste reeglite järgmises jaotises kirjeldatud.

**Reegel 1: Päringu atribuudid**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

See reegel on päringute **ms-ds-consistencyguid** ja kasutajale Active Directoryst **FederatedIdentity** väärtused. Kaustapaanil sobiva poe nime AD FS-i juurutamise poe nime muuta. Muuta ka taotluste tüüp **FederatedIdentity** ja **ms-ds-consistencyguid**määratletud oma ühinemise jaoks õige taotluste tüüp.

**Lisamine** ja **probleemi**abil saate Ärge lisage väljamineva probleemi olemi samuti saate kasutada väärtuste väärtused. Teie väljastate hiljem reegli nõude pärast loote, millist väärtuse püsiv ID

**Reegel 2: Märkige ruut, kui kasutaja on olemas ms-ds-consistencyguid**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

See reegel määratleb ajutine lipu nimega **idflag** , mis on seatud **useguid** pole **ms-ds-concistencyguid** täidetud kasutaja korral. See loogika on ei võimalda AD FS-i tühja taotluste asjaolu. Nii taotluste http://contoso.com/ws/2016/02/identity/claims/objectguid ja http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid reegel 1 lisamisel saate lõpus on **msdsconsistencyguid** taotlemine ainult kui väärtus on täidetud kasutaja jaoks. Kui see on täidetud, näeb AD FS-i, et see on tühi väärtus ja langeb see kohe. Kõigil objektidel on **FederatedIdentity**, et selle nõude alati olemas pärast reegli 1 on täidetud.

**Reegel 3: Probleemi ms-ds-consistencyguid püsiv ID, kui see on olemas**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

See on peidetud märkige ruut **on olemas** . Kui nõude väärtus on olemas, siis küsimuse mis püsiv ID-ga. Eelmises näites **nameidentifier** nõue. On teil muuta see vastav taotluse tüüp püsiv ID teie keskkonnas.

**Reegel 4: Probleemi FederatedIdentity püsiv ID kui ms-ds-consistencyGuid puudub**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

See reegel sisse möllida lihtsalt ajutine lipp **idflag**. Saate otsustada, kas selle väärtuse nõude probleemi.

> [AZURE.NOTE] Reegleid järjekord oluline.

#### <a name="sso-with-a-subdomain-upn"></a>SSO alamdomeeni UPN-i abil

Saate lisada mitu domeeni, et ühendada, kasutades Azure'i AD-ühenduse, nagu on kirjeldatud [uue välise domeeni lisamine](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain). Peate muutma UPN-i nõude, nii, et väljaandja ID vastab juurdomeeni ja pole alamdomeen, kuna ühendatud juurdomeeni hõlmab ka lapse.

Taotluse reegli väljaandja ID on vaikimisi nimega:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Vaikimisi väljaandja ID nõude](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Vaikimisi reegel lihtsalt võtab UPN-i järelliite ja kasutab seda väljaandja ID nõude. Näiteks John on kasutaja sub.contoso.com ja Azure AD on ühendatud contoso.com. John sisestab john@sub.contoso.com nimega ajal Azure AD, sisselogimise kasutajanimi ja vaikimisi väljaandja ID taotlemine reegli AD FS tegeleb see järgmisel viisil.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Taotlemine väärtus:** http://sub.contoso.com/adfs/services/trust/

Üksnes juurdomeeni väljaandja taotluste väärtus, et muuta taotluse reegli vastavaks järgmist.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Järgmised sammud

Lisateavet [kasutaja sisselogimise võimaluste](active-directory-aadconnect-user-signin.md)kohta.
