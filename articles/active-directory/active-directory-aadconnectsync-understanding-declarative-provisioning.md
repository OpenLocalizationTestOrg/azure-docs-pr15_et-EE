<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: mõistmine deklaratiivseid ettevalmistamise | Microsoft Azure'i"
    description="Selgitatakse deklaratiivseid ettevalmistamise konfiguratsiooni mudeli Azure'i AD-ühenduse."
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure'i AD-ühendus sünkroonimine: mõistmine deklaratiivseid ettevalmistamine
See teema selgitab Azure'i AD-ühenduse konfiguratsiooni mudeli. Mudeli nimetatakse deklaratiivseid ettevalmistamise ja see võimaldab teil teha konfiguratsiooni lihtsalt muuta. Selles teemas kirjeldatud paljud asjad on täpsemalt ja enamik kliendi stsenaariumid ei vaja.

## <a name="overview"></a>Ülevaade
Deklaratiivseid ettevalmistamise töötleb allika ühendatud kataloogi pärit objektid ja määrab, kuidas objekt ja atribuutide peaks ümber mõnest muust allikast siht. Objekti töödeldakse sünkroonimine teel ja tulemas on sama sissetulevate ja väljaminevate reeglid. Sissetulevast on-metaversumi konnektori ruumi ja metaversumi on reegli Väljamineva meili konnektori ruumi.

![Müügivõimaluste sünkroonimine](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

Tulemas on mitmeid erinevaid mooduleid. Iga vastutab ühte mõistet objekti sünkroonimist.

![Müügivõimaluste sünkroonimine](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Andmeallika lähteobjekt
- [Ulatus](#scope), leiab kõik sünkroonimine reeglid, mis on ulatus
- [Liitumine](#join), konnektori ruumi ja metaversumi määrab seos
- [Muuta](#transform), kuidas atribuudid peaksid ümber ja meilivoo arvutab
- [Järjestuse](#precedence), lahendab vastuoluliste atribuut osakaalu
- Sihtrakenduse target objekt

## <a name="scope"></a>Ulatus
Ulatus mooduli hinnatava objekti ja määrab reeglid, mis on ulatuse ja töötlemise kaasatakse. Sõltuvalt objekti atribuutide väärtused, väärtustatakse hõlmab eri sünkroonimine reeglid. Keelatud kasutaja, kellel pole Exchange'i postkastiga olla erinevad reeglid, kui on lubatud kasutaja postkasti.  
![Ulatus](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Ulatus on määratletud rühmade ja klauslitest. Rühma sees on klauslitest. Loogilise ja kasutatakse rühma kõik osalause vahel. Näiteks (osakonna = see ja riigi = Taani). Loogiline või kasutatakse ühest rühmast teise.

![Ulatus](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Järgmisel pildil ulatuse asemel lugeda (osakonna = see ja riigi = Taani) või (riik = Rootsi). Kui 1 või 2 hinnatakse true, siis reegel on ulatust.

Ulatus mooduli toetab järgmisi toiminguid.

Toiming | Kirjeldus
--- | ---
VÕRDSETE NOTEQUAL | Stringi compare, mis hindab, kui väärtus on võrdne väärtusega atribuut. Mitme väärtusega atribuute, vt ISIN ja ISNOTIN.
VÄHEM KUI, LESSTHAN_OR_EQUAL | Stringi compare, mis hindab, kui väärtus on väiksem kui atribuudi väärtus.
SISALDAB, OLESEE SISALDAB | Stringi compare, mis hindab, kui väärtus võib leida kuskil väärtus sees atribuuti.
STARTSWITH NOTSTARTSWITH | Stringi compare, mis hindab, kui väärtus on alguses atribuudi väärtust.
ENDSWITH NOTENDSWITH | Stringi compare, mis hindab, kui väärtus on atribuut väärtuse lõpus.
ÜLETAS GREATERTHAN_OR_EQUAL | Stringi compare, mis hindab, kui väärtus on suurem kui atribuudi väärtus.
ISNULL ISNOTNULL | Kui atribuut puudub hindab objekti. Kui atribuut pole esitamine ja seetõttu null, siis reegel on ulatus.
ISIN ISNOTIN | Hindab, kui väärtus on määratletud atribuudi Esita. See toiming on võrdne ja NOTEQUAL mitme väärtusega variatsioon. Atribuut peaks olema mitme väärtusega atribuut ja kui mis tahes atribuudiväärtused leiate väärtuse, siis reegel on ulatust.
ISBITSET ISNOTBITSET | Hindab kui kindla bitise on seatud. Näiteks saab hinnata bittide rakenduses userAccountControl kuvamiseks, kui kasutaja on lubatud või keelatud.
ISMEMBEROF ISNOTMEMBEROF | Väärtus peab sisaldama DN rühma konnektori ruumis. Kui objekt on määratud rühma liige, reegel on ulatust.

## <a name="join"></a>Liitumine
Liitu mooduli sünkroonimine tulemas on otsimise sihtkohas objekti allikas ja objekti seos. Sissetuleva reegli oleks selle seosega konnektori ruumi otsimise seose objekti metaversumi objekti.  
![Liitumine cs- ja mv vahel](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Eesmärk on näha, kui seal on objekti juba metaversumi, loodud teise konnektor, see peaks seotud. Näiteks on konto ressursi mets pärineb konto kasutaja peaks liitunud kasutaja pärineb ressursi.

Ühenduste kasutatakse enamasti sissetulevad reeglid konnektori ruumi objektide kokkuliitmiseks metaversumi samale objektile.

Ühendused on määratletud ühe või mitme rühmad. Sees rühma, peate klauslitest. Loogilise ja kasutatakse rühma kõik osalause vahel. Loogiline või kasutatakse ühest rühmast teise. Rühmade töödeldakse järjekorras ülevalt alla. Kui üks rühm on leitud sihtkohas täpselt ühe objekti vastet, väärtustatakse muude Liitu reeglid. Kui null või rohkem kui ühe objekti on leitud, jätkab järgmise rühma reeglite töötlemine. Seetõttu tuleks reegleid loodud järjekorras kõige konkreetsete esimene ja veel udune lõpus.  
![Liitumine määratlus](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
Ühenduste järgmisel pildil töödeldakse ülevalt alla. Esmalt näeb sünkroonimine müügivõimaluste, kui on Tabeliseose vastendus. Kui ei, teise reegli näeb kui konto nime saab kasutada objektide kokkuliitmiseks. Kui see pole vaste kas, viimase reegel on veel udune match abil kasutaja nimi.

Kui kõik Liitu reeglid on hinnatud ja ei ole just ühe vastet, kasutatakse **Lingi tüüp** lehel **Kirjeldus** . Kui see suvand on määratud **säte**, luuakse uus objekt eesmärgi.  
![Sätte või sellega liitumine](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Objekti peaks olema ainult üks ühe sünkroonimine reegel koos Liitu reeglid ulatus. Kui loendis on mitu sünkroonimine reeglid, kui ühendus on määratletud, ilmneb tõrge. Liitu konfliktide lahendamiseks ei kasutata järjestuse. Objekti peab olema Liitu reegli ulatuse atribuute flow sissetulev/väljaminev samas suunas. Kui vajate meilivoo atribuute nii sissetulevate ja väljaminevate samale objektile, peab olema ka sissetuleva ja Väljamineva meili sünkroonimise reegli abil Liitu.

Väljamineva ühenduse on teisiti käitumine, kui püüab ettevalmistamise objekti target konnektori ruumi. Esmalt proovige vastupidi-ühenduse kasutatakse DN atribuut. Kui sama DN ruumi target konnektor on juba objekti, objektid on liitunud.

Liitu mooduli hinnatakse ainult üks kord, kui ulatus on Sünkrooni uus reegel. Kui mõni objekt on liitunud, see on disjoining isegi juhul, kui liitumise kriteeriumid ei ole enam täidetud. Kui soovite objekti disjoin, sünkroonimise reeglit, mida liitunud objektide minema ulatust.

### <a name="metaverse-delete"></a>Metaversumi kustutamine
Metaversumi objekti jääb, nagu on üks sünkroonimine ulatus **Link** tüüpi toodud **sätte** või **StickyJoin**. Mõne StickyJoin kasutatakse uuele objektile metaversumisse ettevalmistamise konnektor pole lubatud, kuid kui see on liitunud, see tuleb kustutada allikas enne metaversumi objekt on kustutatud.

Metaversumi objekti kustutamisel on märgitud kustutada kõiki objekte, mis on seotud märgitud **osutamiseks** väljaminev sünkroonimise reegli.

## <a name="transformations"></a>Teisendused
Muutusi abil saate määratleda, kuidas atribuute peaks flow lähtekoha soovitud sihtkohta. Funktsiooni puhul võib olla üks järgmistest **meilivoo tüüpi**: otsesed, konstandi või avaldis. Liigub sujuvalt otsese meilivoo, kui atribuudi väärtus-on pole täiendavad muudatused. Konstantne väärtus määrab määratud väärtuse. Avaldise kasutab deklaratiivseid ettevalmistamise avaldis keele express, kuidas peaks olema muutmist. Avaldise keele üksikasjad leiate [mõistmine deklaratiivseid ettevalmistamise avaldis keel](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) teemat.

![Sätte või sellega liitumine](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

**Kui** ruut määratletud, et atribuut ainult olema seatud objekti loomisel. Näiteks saab selle konfiguratsiooni seadmiseks on uue kasutaja objekti algset parooli.

### <a name="merging-attribute-values"></a>Ühendamise atribuudiväärtused
Atribuut liikumine on säte kui ühendada mitu eri konnektorid kaudu mitme väärtusega atribuutide määramiseks. Vaikeväärtus on **värskenduse**, mis näitab, et võidavad peaksite sünkroonimine reegel koos kõrgeim järjestuse.

![Ühenda tüübid](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Olemas on ka **ühendamine** ja **MergeCaseInsensitive**. Nende suvandite abil saate eri allikatest pärit väärtusi ühendamiseks. Näiteks saate kasutada ühendada mitu eri metsast liige või proxyAddresses atribuut. Kui kasutate seda suvandit, kõik sünkroonimine reeglite ulatuse objekti tuleb kasutada sama tüüpi Ühenda. Ei saa määratleda **Update** ühe konnektor ja **ühendamine** teise. Kui soovite, saate tõrketeate.

**Ühenda** ja **MergeCaseInsensitive** vahe on kuidas protsess atribuutide dubleeritud väärtused. Sync engine tagab dubleeritud väärtused on pole lisada target atribuuti. **MergeCaseInsensitive**, kus duplikaatväärtused vahe ainult juhul, kui kavatsete viibida. Näiteks saate peaksite nägema nii "SMTP:bob@contoso.com" ja "smtp:bob@contoso.com" target atribuut. **Ühenda** ainult vaatavad täpse väärtused ja mitu väärtust kui ainult on erinev juhul võib esineda.

Suvandi **asendada** on sama, mis **Update**, kuid seda ei kasutata.

### <a name="control-the-attribute-flow-process"></a>Juhtelemendi atribuut meilivoo protsess
Kui edastamiseks sama metaversumi atribuut on konfigureeritud mitme sissetuleva sünkroonimine reeglid, siis järjestuse kasutatakse võitja selgitamiseks. Sünkroonimine reegel koos kõrgeim järjestuse (madalam arvulise väärtuse) saab aidata väärtus. Sama juhtub väljaminev reeglid. Sünkroonimine reegel koos rahulikke järgnevus ja täiendada ühendatud kataloogi kasutatava väärtuse.

Mõnel juhul selle asemel, et aidata väärtuse, sünkroonimise reegli määratlevad kuidas peaks käitumine muude reeglitega. On mõned teisiti literaalide sel juhul kasutatakse.

Sünkroonimise sissetulevad reeglid, saab näitamaks, et vool ei ole väärtust edastamiseks sõnasõnaline **NULL** . Teise reegli abil alumise järgnevus saavad osaleda väärtuse. Kui ühtegi väärtust, eemaldatakse metaversumi atribuut. Reegli Väljamineva meili jaoks kui **NULL** on lõppväärtus Sünkrooni kõik reeglid olema töödeldud, siis väärtus eemaldatakse ühendatud kataloogi.

Sõnasõnaline **AuthoritativeNull** on sarnane, kuid erinedes selle poolest, et alumise järgnevus reegleid ei saavad osaleda väärtus **null** .

Mõne atribuudi voog saate kasutada ka **IgnoreThisFlow**. See on sarnane null mõttes, et see tähistab pole midagi aidata. Erinevus on, et seda ei saa eemaldada on juba olemasolevad väärtus sihtkohas. See on nagu atribuudi voog pole kunagi seal.

Siin on näide:

*AD - kasutaja Exchange'i hübriidjuurutuse välja* võib leida voogu järgmist:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
See avaldis asemel lugeda: kui kasutaja postkast asub Azure AD, siis flow: Azure'i AD-AD atribuut. Kui ei, meilivoo, midagi tagasi Active Directory. Sel juhul tuleks see pidage olemasolevale väärtusele AD.

### <a name="importedvalue"></a>ImportedValue
Funktsiooni ImportedValue erineb kõik muud funktsioonid, kuna atribuudi nimi peavad olema ümbritsetud jutumärkidega, mitte nurksulgudega:  
`ImportedValue("proxyAddresses")`.

Tavaliselt sünkroonimise ajal kasutab atribuut oodatav väärtus, isegi siis, kui see pole veel eksportida või tõrke võttis ajal ekspordi ("üles torni"). Mõne sissetuleva sünkroonimise eeldab, et atribuudile, mis pole veel kätte ühendatud kataloogi lõpuks jõuab see. Mõnel juhul on oluline sünkroonida ainult väärtus, mis kinnitab ühendatud kataloogi (hologramm ja importida tower delta").

See funktsioon näite leiate out box sünkroonimine reegel *sisse: AD-kasutaja levinud Exchange'ist*. Exchange'i hübriidjuurutuse, Exchange Online'i lisatud väärtuse ainult sünkroonitakse kui see on kinnitatud väärtus on edukalt eksporditud:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Järjestuse
Kui mitu sünkroonimine reeglid aitavad teid sihtkohta sama atribuudi väärtus, kasutatakse järjestuse väärtus võitja selgitamiseks. Reegli kõrgeim järgnevus madalaim väärtus, mille saab aidata konflikti atribuuti.

![Ühenda tüübid](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

See tellimine saab määratleda Täpsem atribuut puhul väikese objektid. Näiteks välja-ja-ruut-reegleid veenduge, et atribuudid: lubatud konto (**Kasutaja AccountEnabled**) on järjestuse muudelt kontodelt.

Järjestuse saate määratleda konnektorid vahel. Võimaldab paremini andmete edastamiseks väärtused esmalt konnektorid.

### <a name="multiple-objects-from-the-same-connector-space"></a>Mitme objekti sama konnektori ruumi
Kui teil on mitu objekti sama konnektori ruumis metaversumi samale objektile liitunud, järjestuse kohandada. Kui sama sünkroonimise reegli ulatus on mitu objekti, siis sync engine ei ole võimalik järjestuse määramisel. See on ebaselgete, mis lähteobjekt aitama metaversumi väärtuse. Selle konfiguratsiooni on staatusega ebaselgete isegi siis, kui atribuute allikas on sama väärtusega.  
![Mitme objekti liitunud mv samale objektile](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

Selle stsenaariumi korral peate muuta sünkroonimine eeskirjad, et allika objektid on erinevate sünkroonimine reeglid ulatus. Võimaldab määratleda eri järjestuse.  
![Mitme objekti liitunud mv samale objektile](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet avaldis keele avaldistes [mõistmine deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)kohta.
- Vaadake, kuidas deklaratiivseid ettevalmistamine on kasutatud out-of-box mõista [vaikimisi konfiguratsiooni](active-directory-aadconnectsync-understanding-default-configuration.md).
- Vaadake, kuidas abil, kuidas [muuta vaikimisi konfiguratsiooni](active-directory-aadconnectsync-change-the-configuration.md)deklaratiivseid ettevalmistamise praktiline muudatuste tegemine.
- Lugege, kuidas kasutajad ja kontaktide koostöö [mõistmine kasutajate ja kontaktide](active-directory-aadconnectsync-understanding-users-and-contacts.md)jätkata.

**Teemade ülevaade**

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)

**Viide Teemad**

- [Azure'i AD-ühendus sünkroonimine: funktsioonide juhend](active-directory-aadconnectsync-functions-reference.md)
