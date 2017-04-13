<properties
    pageTitle="Azure Active Directory domeeniteenused: Sünkroonimise hallatavate domeenides | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused hallatavate domeenis sünkroonimise mõistmine"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Sünkroonimise Azure AD domeeniteenused hallatavate domeenis
Järgmine diagramm näitab sünkroonimise tööpõhimõtted Azure AD domeeniteenused hallatavate domeenid.

![Azure'i AD domeeniteenused sünkroonimise topoloogia](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Kohapealse kataloogi sünkroonimine oma Azure AD rentniku
Azure'i AD-ühendus sünkroonimine kasutatakse sünkroonimine Kasutajakontod, rühma liikmestaatusi ja mandaadi hashes oma Azure AD rentniku. Kasutaja atribuutide nagu UPN-i kontod ja kohapealse SID (turvalisuse ID) on sünkroonitud. Azure'i AD domeeni teenuste kasutamisel sünkroonitakse teie Azure AD rentniku ka pärand mandaati hashes nõutavaid NTLM ja Kerberos-autentimist.

Kui konfigureerite allahindluse, sünkroonitakse Azure AD kataloogis muutusi tagasi oma kohapealse Active Directory. Näiteks kui muudate parooli, kasutades Azure AD Iseteeninduslik parooli muutmine funktsioone, muudetud parooli värskendatakse teie asutusesisese AD domeeni.

> [AZURE.NOTE] Alati veenduge, et teil on kõik teadaolevad vead parandustega uusima versiooni Azure'i AD-ühenduse abil.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Hallatava domeeni teie Azure AD rentniku sünkroonimine
Kasutajakontod, rühmaliikmeid ja mandaadi hashes sünkroonitakse teie Azure AD rentniku kaudu Azure AD domeeniteenused hallatava domeeni. See sünkroonimine on automaatselt. Teil pole vaja konfigureerimine, jälgida või sünkroonimise protsessi hallata. Sünkroonimine on ka üks-way/Ühesuunalised laadi. Hallatava domeeni on suuresti kirjutuskaitstud välja arvatud mis tahes kohandatud organisatsiooniüksused loomist. Seetõttu ei saa teha muudatusi kasutaja atribuutide, kasutaja paroolid või hallatava domeeni rühmaliikmeid. Seetõttu on muudatuste hallatavate domeenist tagasi oma Azure AD rentniku pole pööratud sünkroonimine.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Mitme mets kohapealse keskkonna sünkroonimine
Mitme organisatsioonidel on üsna keerukas kohapealse identiteedi taristu koosneb mitme konto metsade. Azure'i AD-ühendus toetab kasutajate, rühmade ja mandaadi hashes mitme mets keskkonnas kaudu oma Azure AD rentniku sünkroonimine.

Seevastu oma Azure AD rentniku on palju lihtsama ja kindla nimeruum. Kasutajate juurdepääsu usaldusväärselt rakenduste tagatud Azure AD lubamiseks üle erinevate metsade Kasutajakontod UPN-i konfliktide lahendamine. Teie Azure AD rentniku sarnane oma Azure AD domeeniteenused hallatava domeeni karudega sulgemine Seetõttu näete tasapinnalise OU struktuuri hallatava domeeni. 'AADDC kasutajad' container, olenemata, millest ta on sünkroonitud kohapealse domeeni või mets salvestatakse kõik kasutajad ja rühmad. Olete konfigureerinud hierarhiliste OU struktureerimine kohapealse. Hallatava domeeni veel on lihtne tasapinnalise OU struktuur.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Välistamised – mis ei sünkroonita hallatava domeeni
Järgmiste objektide või atribuute ei sünkroonita teie Azure AD rentniku või hallatava domeeni:

- **Välja atribuudid:** Võite välistada teatud atribuute sünkroonimast oma Azure AD rentniku kohapealse domeeni Azure'i AD-ühenduse kaudu. Need välja atribuudid pole saadaval hallatava domeeni.

- **Rühmitamine poliitikate:** Kohapealse domeeni konfigureeritud rühmapoliitikat ei sünkroonita hallatava domeeni.

- **SYSVOL ühiskasutus:** Samuti ei sünkroonita SYSVOL ühiskasutusse andmise kohta kohapealse domeeni sisu hallatava domeeni.

- **Arvuti objektide:** Arvuti objektide kohapealse domeeni ühendatud arvutites ei sünkroonita hallatava domeeni. Need arvutid ei ole usaldusseos hallatava domeeni ja kohapealse domeeni ainult kuuluvad. Hallatava domeeni leiate arvuti objektide ainult arvutites olete konkreetselt domeeni-liitunud hallatava domeeni jaoks.

- **SidHistory atribuute kasutajad ja rühmad:** Kasutaja, rühma esmane sid teie asutusesisese domeeni sünkroonitakse hallatava domeeni. Siiski olemasoleva SidHistory atribuute kasutajad ja rühmad ei sünkroonita teie asutusesisese domeeni hallatava domeeni.

- **Ettevõtte üksused (Browse) struktuurid:** Kohapealse domeeni määratletud organisatsiooniliste üksuste sünkroonida hallatava domeeni. Hallatava domeeni on kaks sisseehitatud organisatsiooniüksused. Vaikimisi on hallatava domeeni tasapinnalise OU struktuur. Juhul võite luua [kohandatud OU teie hallatava domeeni](./active-directory-ds-admin-guide-create-ou.md).


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Kuidas teatud atribuudid on sünkroonitud hallatava domeeni
Järgmises tabelis on loetletud mõned levinud atribuudid ja kirjeldatakse, kuidas sünkroonitakse hallatava domeeni.

| Atribuuti hallatava domeeni | Allikas | Märkmete |
|:---|:---|:---|
|UPN-I|Oma Azure AD rentniku kasutaja UPN-atribuut|Ka UPN-atribuudi kaudu oma Azure AD rentniku sünkroonitakse on hallatava domeeni. Seetõttu kasutab teie UPN-i usaldusväärselt hallatava domeeni sisse logida.|
|SAMAccountName|Kasutaja mailNickname oma Azure AD rentniku atribuuti või automaatselt loodud|Atribuut SAMAccountName on pärit oma Azure AD rentniku mailNickname atribuuti. Kui mitu kasutajakontot on sama mailNickname atribuuti, kuvatakse SAMAccountName on automaatselt loodud. Kui kasutaja mailNickname või UPN-i eesliide on pikem kui 20 märki, on selle SAMAccountName automaatselt loodud 20 lubatust SAMAccountName atribuutide vastavaks.|
|Paroolide|Kasutaja parooli oma Azure AD rentniku|Teie Azure AD rentniku on sünkroonitud mandaati hashes nõutavaid NTLM või Kerberose autentimist (nimetatakse ka täiendavatele mandaat). Kui teie Azure AD rentniku on sünkroonitud rentniku, neid mandaate on pärit kohapealse domeeni.|
|Esmane kasutaja/rühma SID|Automaatselt loodud|Esmane SID kasutaja/rühma kontod on automaatselt loodud hallatava domeeni. See atribuut ei sobi esmane kasutaja/rühma SID oma kohapealse objekti AD domeeni. Selle lahknevuse põhjuseks hallatava domeeni on erinevate SID nimeruumi, kui teie asutusesisese domeeni.|
|Kasutajate ja rühmade SID ajalugu|Kohapealse kasutaja, rühma SID|Kasutajate ja rühmade hallatava domeeni atribuut SidHistory on seatud vastavad vastavate esmane kasutaja või rühma SID teie asutusesisese domeeni. See funktsioon aitab lihtsustada lifti-shift kohapealse rakendusi hallatav domeeni, kuna teil pole vaja re-ACL ressursid.|

> [AZURE.NOTE] **UPN-vormingus hallatava domeeni sisse logida:** Atribuut SAMAccountName võib automaatselt loodud mõne hallatava domeeni Kasutajakontod. Kui mitu kasutajat on sama mailNickname atribuuti või kasutajatel on liiga pikk UPN-i eesliidete tähised, võib SAMAccountName nende kasutajate jaoks automaatselt loodud. Seetõttu SAMAccountName vormingu (nt CONTOSO100\joeuser) ei ole alati usaldusväärne viis domeeni sisse logida. Kasutajate automaatselt loodud SAMAccountName võib erineda nende UPN-i eesliide. UPN-vormingus kasutamine (nt 'joeuser@contoso100.com') usaldusväärselt hallatava domeeni sisse logida.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objekte, mis ei sünkroonita teie Azure AD rentniku hallatava domeeni kaudu
Selles artiklis eelmises jaotises kirjeldatud on pole sünkroonimine hallatava domeeni uuesti oma Azure AD rentniku. [Kohandatud ettevõtte üksus (OU)](./active-directory-ds-admin-guide-create-ou.md) loomiseks võite hallatava domeeni. Lisaks saate luua muude organisatsiooniüksused, kasutajate, rühmade või teenuse kontode need kohandatud organisatsiooniüksused sees. Ükski objekte, mis on loodud kohandatud organisatsiooniüksused sünkroonitakse tagasi oma Azure AD rentniku. Need objektid on saadaval üksnes hallatava domeeni kasutamiseks. Seetõttu neid objekte ei ole nähtav, kasutades Azure AD PowerShelli cmdletid Azure AD Graph API või Azure AD haldamise kasutajaliides.


## <a name="related-content"></a>Seotud teemad
- [Funktsioonid - Azure AD domeeniteenused](active-directory-ds-features.md)

- [Juurutamise stsenaariumide - Azure AD domeeniteenused](active-directory-ds-scenarios.md)

- [Azure'i AD domeeniteenused võrgunduse kaalutlused](active-directory-ds-networking.md)

- [Azure'i AD domeeniteenused kasutamise alustamine](active-directory-ds-getting-started.md)
