<properties
    pageTitle="Identiteedi sünkroonimise ja Dubleeri atribuut paindlikkust | Microsoft Azure'i"
    description="Uus käitumine reageerimine objektid UPN-i või Kasutajaatribuut konfliktide kataloogi sünkroonimine Azure'i AD-ühenduse kasutamise ajal."
    services="active-directory"
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
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Identiteedi sünkroonimise ja Dubleeri atribuut paindlikkust
Dubleeritud atribuut paindlikkust on funktsioon Azure Active Directory kõrvaldab hõõrdumise põhjustatud **UserPrincipalName** ja **Kasutajaatribuut** konfliktide kui ühe Microsofti sünkroonimise tööriistad.

Need kaks atribuuti on üldiselt peavad olema kordumatud üle antud Azure Active Directory rentniku **kasutaja**, **rühma**või **kontakti** objektid.

> [AZURE.NOTE] Ainult need kasutajad, võib olla UPN-ID.

Uus käitumine, mida see funktsioon võimaldab on sünkroonimine müügivõimaluste pilveteenuse osa, seetõttu on kliendi agnostik ja asjakohased Microsoft sünkroonimise toote, sh Azure'i AD-ühenduse, DirSync ja MIM + konnektor. Üldine termin "sünkroonimiskliendi" kasutatakse tähistada nende toodete ühte selles dokumendis.

## <a name="current-behavior"></a>Praeguse käitumine
Kui katse ettevalmistamise uuele objektile UPN-i või Kasutajaatribuut väärtus, mis rikub selle unikaalsuse piirang, blokeerib Azure Active Directory objekti loomist. Samamoodi kui objekti värskendatakse-kordumatu UPN-i või Kasutajaatribuut, värskendus nurjub. Ettevalmistamise katse või värskendus uuesti proovida tsükli ekspordi sünkroonimine klient ja ikka ebaõnnestuda kuni konflikti on lahendatud. Iga katse korral luuakse tõrketeade aruande e-posti ja sünkroonimise klient logitakse tõrge.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Dubleeritud atribuut paindlikkust käitumist
Täielikult ei ettevalmistamise või värskendada objekti dubleeritud atribuut, mitte Azure Active Directory "karantiini" oleks vastuolus unikaalsuse piirang dubleeritud atribuuti. Kui see atribuut UserPrincipalName, nt ettevalmistamise teenuse määrab kohatäite väärtus. On nende ajutine väärtuste vormindamine  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Kui atribuut pole nõutav, nagu on **kasutajaatribuudis**, Azure Active Directory lihtsalt karantiini konflikti atribuut ja liigub objekti loomine või värskendamine.

Pärast karantiini atribuut, saadetakse sama tõrketeade aruande meilisõnumite kasutatud vana käitumine teavet konflikti kohta. See teave kuvatakse tõrketeade aruande ainult üks kord, kui karantiini juhtub, seda ei jätkuvalt tulevaste e-kirju sisse logima. Ka, kuna selle objekti ekspordi õnnestus, sünkroonimisrakendus ei saa sisse logida tõrge ja proovige Loo / Uuenda järgmise sünkroonimise tsükli kasutamiseks.

Et seda toimingut toetavate uus atribuut on lisatud kasutaja, rühma ja kontakti objekti klassi:  
**DirSyncProvisioningErrors**

See on mitme väärtusega atribuut salvestamiseks vastuoluliste atribuute, mis rikub unikaalsuse piirang need lisatakse tavalisel viisil. Tausta timer tööülesande on lubatud Azure Active Directory tunnis otsida dubleeritud atribuut konflikte, mis on lahendatud ja karantiini kõnealuse atribuute eemaldab automaatselt käivituva.

### <a name="enabling-duplicate-attribute-resiliency"></a>Dubleeritud atribuut paindlikkust lubamine
Dubleeritud atribuut paindlikkust on kõik Azure Active Directory rentnikud uue vaikekäitumise. See toimub vaikimisi kõik rentnikud lubatud sünkroonimise esimest korda 22nd August 2016 või uuema versiooniga. Rentnikud enne seda kuupäeva sünkroonimine lubatud, on see funktsioon lubatud pakettidena. Selle levitamise kohta alguses August 2016 ja meiliteatise saadetakse iga rentniku tehniline teatis kontakti kindlale kuupäevale, kui see funktsioon on lubatud.

Kui atribuut paindlikkust dubleerimine on välja lülitatud seda ei saa keelata.

Et kontrollida, kui see funktsioon on lubatud teie rentniku, saate seda teha allalaadimist uusim versioon Azure Active Directory PowerShelli mooduli ja käivitades:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Kui soovite lubada aktiivselt funktsioon enne, kui see on sisse lülitatud oma rentniku jaoks, saate seda teha allalaadimist uusim versioon Azure Active Directory PowerShelli mooduli ja käivitades:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Objektid, mille DirSyncProvisioningErrors tuvastamine
On praegu kaks meetodit objekte, mis on atribuudi duplikaat konfliktide, Azure Active Directory PowerShelli ja Office 365 haldusportaali tõttu nende vigade tuvastamiseks. On lepingute laiendada täiendavate portaali vastavalt aruandluse tulevikus.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShelli
PowerShelli cmdletid selles teemas, jaoks on täidetud:

- Kõik järgmised cmdlet-käsud on tõstutundlikud.
- **– ErrorCategory PropertyConflict** alati tuleb lisada. Praegu ei ole muud tüüpi **ErrorCategory**, kuid see võib tulevikus pikendada.

Esmalt alustamiseks töötab **Ühendus MsolService** ja mandaadi sisestamise jaoks rentnikuadministraator.

Seejärel kasutage järgmised cmdlet-käskude ja tehtemärke tõrgete kuvamiseks erineval viisil:

1. [Näha kõiki](#see-all)

2. [Atribuut tüübi järgi](#by-property-type)

3. [Vastuolulised väärtuse alusel](#by-conflicting-value)

4. [Stringi otsingu abil](#using-a-string-search)

5. [Sorditud](#sorted)

6. [Piiratud kogus või kõik](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Näha kõiki
Kui ühendus on loodud, üldist loendi atribuut ettevalmistamise tõrgete rentniku käivitage.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Selle tulemiks on umbes selline:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Atribuut tüübi järgi
Atribuut tüübi järgi tõrgete kuvamiseks **- PropertyName** lipu **UserPrincipalName** või **ProxyAddresses** argument on lisamiseks tehke järgmist.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Või

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Vastuolulised väärtuse alusel
Seotud tõrgete kuvamiseks teatud atribuudi lisage **- PropertyValue** lipp (**- PropertyName** tuleb kasutada ka selle lipu lisamine).

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Stringi otsingu abil
Lai stringi otsingu kasutage **- Otsitav lõik** lipp. Seda saab kasutada sõltumatult kõigi ülaltoodud lipud, välja arvatud **-ErrorCategory PropertyConflict**, mis on alati vaja:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Piiratud kogus või kõik
1. **MaxResults <Int> ** saab kasutada päring teatud väärtuste arv piirata.

2. **Kõik** saab tagada, et kõik tulemused alla laaditakse juhul, kui suure hulga tõrgete olemas.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365 haldusportaali

Directory sünkroonimise tõrked saate vaadata Office 365 halduskeskuses. Office 365 portaalis aruandes kuvatakse ainult **kasutaja** objektid, mis on need vead. See ei Näita, **rühmad** ja **kontaktide**vaheliste konfliktide kohta.


![Aktiivsed kasutajad] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Aktiivsed kasutajad")

Juhised Office 365 halduskeskuses directory sünkroonimise tõrked vaatamise kohta vt [tuvastamine directory Sünkroonimistõrgete Teenusekomplektis Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Identiteedi Kataloogisünkroonimise kohta tõrketeatise
Kui objekti dubleeritud atribuut konfliktiga käsitletakse uue käitumise teatis sisaldab standard identiteedi Kataloogisünkroonimise kohta tõrketeatise meilisõnum, mille tehniline teatis saadetakse rentniku võtke ühendust. Siiski on oluline muudatus seda käitumist. Varem oleks teavet dubleeritud atribuut konflikti kaasatud iga järgmise tõrkearuanne kuni konflikti on lahendatud. Uue käitumise, tõrge teate antud konflikti ainult kuvatakse kohe - vastuoluliste atribuut on pandud ajal.

Siin on näide sellest, mida e-posti teatis näeb Kasutajaatribuut konflikti:  
    ![Aktiivsed kasutajad](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Konfliktide ja tõrgete lahendamine
Tõrkeotsingu strateegia ja eraldusvõime taktikaid nende vigade ei tohiks erineda nii, nagu varem käsitleja dubleeritud atribuut tõrked. Ainus erinevus on, et timer tööülesande vallutab kaudu rentniku teenuse küljel automaatselt lisada kõnealuse atribuut proper objekti pärast konflikti on lahendatud.

Järgmises artiklis antakse ülevaade eri tõrkeotsing ja eraldusvõime strateegiad: [Dubleeri või vigased atribuudid takistavad kataloogisünkroonimist Teenusekomplektis Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Teadaolevad probleemid
Ükski järgmiste teadaolevate probleemide põhjustab andmete kaotsimineku või teenuse degradeerumine. Mitu neist on esteetiline, teised põhjustada standard "*eelse paindlikkust*" dubleeritud atribuut tõrgete asemel karantiini konflikti atribuut visati ja teise põhjustab teatud vigu nõuda lisatasu käsitsi lahendus üles.

**Core käitumine.**

1. Teatud atribuut konfiguratsioone objektid on jätkuvalt ekspordi tõrgete dubleeritud attribute(s), mis on pandud erinevalt.  
Näiteks:

    lisamine. Luuakse uus kasutaja UPN ja AD **Joe@contoso.com** ja kasutajaatribuudis**smtp:Joe@contoso.com**

    b. Selle objekti atribuudid vastuolus olemasolevasse rühma, kus on kasutajaatribuudis **SMTP:Joe@contoso.com**.

    c. Eksportimisel **Kasutajaatribuut konflikti** tõrge Expression.Error asemel pandud konflikti atribuute. Toiming on pärast iga järgmise sünkroonimise tsükli, uuesti proovida, nagu oleks enne paindlikkust funktsioon on lubatud.

2. Kui kaks rühmad on loodud kohapealse sama SMTP-aadress, ei mõnda sätet standardne dubleeritud **Kasutajaatribuut** tõrkega katsel. Siiski õigesti karantiini kahekordsed väärtus järgmise sünkroonimise tsükli korral.

**Office'i portaali aruande**:

1. Üksikasjalik tõrketeade kaks objektide kogumi UPN-i konflikti on sama. See näitab, et nad on mõlemad olnud nende UPN-i muutunud / karantiini, kui tegelikult on ainult üks neist oli kõik andmed, mis on muutunud.

2. UPN-i konflikti üksikasjalik tõrketeade kuvatakse valesti kuvatav kasutaja, kes on olnud nende UPN-i muudetud/karantiini. Näiteks:

    lisamine. **Kasutaja A** sünkroonib üles esimene **UPN-i = User@contoso.com **.

    b. **Kasutaja B** on proovitakse kohale toimetada sünkroonida, kuni järgmise koos **UPN-i = User@contoso.com **.

    c. **Kasutaja B** UPN-i on muutunud **User1234@contoso.onmicrosoft.com** ja **User@contoso.com** lisatakse **DirSyncProvisioningErrors**.

    d. Kuvatakse tõrketeade, kui **Kasutaja** b märkima, et **Kasutaja A** on juba **User@contoso.com** nagu ka UPN-i, kuid see kuvatakse **Kasutaja B** oma displayName.



**Identiteedi Kataloogisünkroonimise kohta tõrketeatise**:

Juhised *probleemi lahendamiseks* seos on vale.  
    ![Aktiivsed kasutajad](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

See peaks osutage [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).


## <a name="see-also"></a>Vt ka

- [Azure'i AD-ühendus sünkroonimine](active-directory-aadconnectsync-whatis.md)

- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)

- [Directory sünkroonimise tõrked Teenusekomplektis Office 365 tuvastamine](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
