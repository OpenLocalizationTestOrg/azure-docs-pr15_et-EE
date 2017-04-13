<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: mõistmine vaikimisi konfiguratsiooni | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse sünkroonimine Azure'i AD-ühenduse konfiguratsiooni vaikimisi."
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure'i AD-ühendus sünkroonimine: vaikimisi konfiguratsiooni mõistmine
Selles artiklis selgitatakse out box konfiguratsiooni reeglid. Dokumendid on reeglid ja kuidas need reeglid mõjutavad konfiguratsiooni. See ka juhatab teid läbi sünkroonimine Azure'i AD-ühenduse konfiguratsiooni vaikimisi. Eesmärk on, et lugeja mõistab konfiguratsiooni mudeli, nimega deklaratiivseid ettevalmistamise aktiivsuse tegelike näites. Selles artiklis eeldatakse, et teil on juba installitud ja konfigureerida Azure'i AD-ühenduse sünkroonimine installimise viisardi abil.

Et aru saada konfiguratsiooni mudel, lugege [Mõistmine deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning.md)üksikasjad.

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Azure AD asutusesisesest out box reeglid
Järgmised avaldised leiate out box konfiguratsiooni.

### <a name="user-out-of-box-rules"></a>Kasutaja out box reeglid
Reegleid rakendada ka iNetOrgPerson objekti tüüp.

Kasutaja objekti peab vastama sünkroonitavate järgmist:

- Peab olema on sourceAnchor.
- Pärast Azure AD on loodud objekti, seejärel sourceAnchor ei saa muuta. Kui väärtus on muudetud kohapealse, lõpetab objekti sünkroonimise seniks, kuni soovitud sourceAnchor on muutunud tagasi eelmisele väärtusele.
- Peab olema accountEnabled (userAccountControl) atribuut täidetud. Kohapealse Active Directory see atribuut on alati kohal ja täidetud.

Järgmised kasutaja objektid on **ei** sünkroonita Azure AD:

- `IsPresent([isCriticalSystemObject])`. Tagada paljude out box objektide Active Directorys, näiteks sisseehitatud administraatori konto, ei sünkroonita.
- `IsPresent([sAMAccountName]) = False`. Veenduge, et kasutaja objektid pole atribuut sAMAccountName ei sünkroonita. Sel juhul ainult praktiliselt juhtub domeeni NT4 üle läinud.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Sünkroonida teenuse konto sünkroonimine Azure'i AD-ühenduse ja selle varasemates versioonides.
- Sünkroonimine ei tööta Exchange Online'i Exchange'i kontode.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Sünkroonida objekte, mis ei tööta Exchange Online'i.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
Järgmiste objektide soovite filtreerida selle Bitimask esitab (ja H21C07000):
    - Meililoaga ühiskausta
    - Süsteemi automaatkeskjaama postkasti
    - Postkasti andmebaasi postkast (süsteemi postkasti)
    - Universaalne turberühm (ei rakendada mõne kasutaja jaoks, kuid on olemas pärand põhjustel)
    - Mitte-Universal rühma (ei rakendada mõne kasutaja jaoks, kuid on olemas pärand põhjustel)
    - Postkasti kavandamine
    - Tuvastuspostkasti
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Sünkroonida kõik dispersioonanalüüs ohvriks objektid.

Rakendatakse järgmised reeglid atribuut:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. SourceAnchor atribuut on lingitud postkastist ei ole kasutatud. Eeldatakse, et lingitud postkasti leidmisel tegelik konto on liitunud hiljem.
- Exchange'i seotud atribuudid on sünkroonitud ainult siis, kui väärtus on atribuut **mailNickName** .
- Kui loendis on mitu metsade, siis atribuute tarbitud järgmises järjestuses:
    1. Sisselogimise seotud atribuute (nt userPrincipalName) on lubatud konto pärineb kaasa.
    2. Exchange'i postkasti koos pärineb on kaasa atribuute, mille leiate Exchange'i globaalse Aadressiloendi (globaalne aadressiloend).
    3. Kui leiate ühtegi postkasti, saate neid atribuute mis tahes mets pärit.
    4. Exchange'i seotud atribuute (tehniline atribuudid ei kuvata gal) on kaasa mets: kui `mailNickname ISNOTNULL`.
    5. Kui loendis on mitu metsade, mis vastavad ühe reegliga, siis loomine järjestuse (kuupäev/kellaaeg) konnektorid (forests) kasutatakse kindlaks teha, millised mets aitab atribuute.

### <a name="contact-out-of-box-rules"></a>Kontakti out box reeglid
Kontakti objekti peab vastama sünkroonitavate järgmist:

- Kontakti peab olema meililoaga. See on kontrollida järgmiselt:
    - `IsPresent([proxyAddresses]) = True)`. Atribuut proxyAddresses peavad olema täidetud järgmised.
    - Atribuut proxyAddresses või atribuuti mail leiate esmase meiliaadressi. Esinemine on @ kasutatakse kinnitamaks, et sisu on e-posti aadress. Ühe kahe reegliga tuleb hinnata väärtuseks True.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Kas kirje "SMTP:" ja kui on, saate mõne @ stringi leida?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Atribuuti mail täidetud ja kui see on, saate mõne @ stringi leida?

Järgmised kontakti objektid on **ei** sünkroonita Azure AD:

- `IsPresent([isCriticalSystemObject])`. Veenduge, et pole märgitud nii kriitiline kontakti objektid on sünkroonitud. Ei tohi olla mis tahes vaikimisi konfiguratsioon.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Neid objekte ei tööta Exchange Online'i.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Sünkroonida kõik dispersioonanalüüs ohvriks objektid.

### <a name="group-out-of-box-rules"></a>Rühma out box reeglid
Rühma objekti peab vastama sünkroonitavate järgmist:

- Peab olema väiksem kui 50 000 liiget. See arv on kohapealse rühma liikmete arv.
    - Kui see on liikmete enne sünkroonimise alustamist esimest korda, ei sünkroonita rühma.
    - Liikmete arv kasvab algselt loomisel, siis kui see jõuab 50.000 liikmeid lakkab sünkroonimise kuni liikmete arv on väiksem kui 50 000 uuesti.
    - Märkus: 50 000 liikmete arv on ka jõustatud Azure AD. Te ei saa sünkroonida rühma liikmete isegi juhul, kui te muuta või eemaldada see reegel.
- Kui rühm on **Levirühm**, siis seda ka peab olema lubatud meil. Vaadake [kontakti väljale-, reeglid](#contact-out-of-box-rules) selle reegli jõustatakse.

Järgmises jaotises objektid on **ei** sünkroonita Azure AD:

- `IsPresent([isCriticalSystemObject])`. Veenduge, et palju out box objektide Active Directory, nt sisseehitatud administraatorite rühma, ei sünkroonita.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Pärand jaotis kasutatavaid DirSync.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rollirühma.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Sünkroonida kõik dispersioonanalüüs ohvriks objektid.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out box reeglid
FSPs liidetud "mis tahes" (\*) metaversumi objekti. Tegelikult juhtub see liitumine ainult kasutajad ja turberühmad. Selle konfiguratsiooni tagab rist – mets liikmelisused on lahendatud ja Azure AD esitatud õigesti.

### <a name="computer-out-of-box-rules"></a>Arvuti out box reeglid
Arvuti objekti peab vastama sünkroonitavate järgmist:

- `userCertificate ISNOTNULL`. Ainult Windows 10 arvutites asustada see atribuut. Kõigi arvuti objektide atribuudi väärtus on sünkroonitud.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Stsenaarium out box reeglite mõistmine
Selles näites me kasutame on juurutus, nii et ühe konto mets (A), ühe ressursi mets (R) ja ühe Azure AD kataloogi.

![Pilt, mille stsenaarium kirjeldus](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

Selle konfiguratsiooni puhul eeldatakse, on lubatud konto konto mets ja ressursside mets lingitud postkastiga keelatud konto.

Meie eesmärk on vaikimisi konfiguratsioon on:

- Sisselogimise seotud atribuudid on sünkroonitud mets lubatud kontoga.
- Postkasti pärineb sünkroonitakse atribuudid, mis võib leida globaalses Aadressiloendis (globaalne aadressiloend). Kui ühtegi postkasti ei leita, kasutatakse mis tahes muud mets.
- Kui lingitud postkasti, tuleb leida objekti eksportimisel Azure AD seotud lubatud konto.

### <a name="synchronization-rule-editor"></a>Sünkroonimise redaktoris
Konfiguratsiooni saab vaadata ja muuta tööriistaga sünkroonimise reeglid Editor (LST) ja selle otsetee leiate menüü start.

![Sünkroonimise reeglite redaktori ikoon](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

SRE on resource kit tööriista ja see on installitud versiooniga Azure'i AD-ühenduse sünkroonimine. Käivitage see saama, peate olema ADSyncAdmins rühma liige. Kui see käivitub, näete umbes selline:

![Sissetulev sünkroonimise reeglid](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Sellel paanil kuvatakse kõik sünkroonimise reeglid loonud konfiguratsioonist. Tabeli iga rea on üks sünkroonimise reegel. Vasakule klõpsake jaotises reegli tüübid, on loetletud kahte erinevat tüüpi: sissetulevate ja väljaminevate. Sissetulev ja väljaminev on metaversumi vaates. Fookuse ei kavatse peamiselt sissetulevad reeglid selle ülevaate kohta. Tegelik sünkroonimise reeglite loendi sõltub tuvastati skeemi AD. Ülaltoodud joonisel konto mets (fabrikamonline.com) ei saa teenuseid, nt Exchange'i ja Lynci, ja need teenused on loodud sünkroonimise reegleid ei. Siiski ressursside mets (res.fabrikamonline.com) leiate sünkroonimise reeglid nende teenuste jaoks. Reeglite sisu erineb sõltuvalt tuvastatud versiooni. Näiteks rakendusega Exchange 2013 juurutamine on veel atribuut puhul, mis on konfigureeritud kui Exchange 2010 ja 2007.

### <a name="synchronization-rule"></a>Sünkroonimine reegel
Sünkroonimise reegel on konfiguratsiooni objekti voolav, kui tingimus on täidetud atribuutide kogum. Seda kasutatakse kirjeldamiseks, kuidas objekti konnektori ruumis on seotud objekti metaversumi, nimetatakse **Liitu** või **vasta**. Sünkroonimise reeglid on järjestuse väärtuse, mis näitab, kuidas need omavahel seotud. Sünkroonimise reegli alumise arvväärtusega on suurem järjestuse ja atribuut meilivoo konfliktide kõrgem võitis konfliktide lahendamine.

Näiteks, vaadake sünkroonimine reegel **rakenduses AD-kasutaja AccountEnabled kaudu**. Märkige see rida SRE sisse ja valige **Redigeeri**.

Kuna see reegel on out box reegel, kuvatakse hoiatus, kui avate reegel. Ei tohiks teha [muudatused väljal-, reeglid](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), et küsitakse, mis on teie kavatsused. Sel juhul ainult soovite reegli vaatamiseks. Valige **ei**.

![Sünkroonimise reeglite hoiatus](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Sünkroonimine reegel on neli konfigureerimine jaotistes: kirjeldus, filtreerimine, Liitu reeglid ja teisendused ulatuse määramine.

#### <a name="description"></a>Kirjeldus
Esimene jaotis põhiteave nt nimi ja kirjeldus.

![Kirjeldus vahekaardil sünkroonis redaktoris ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Teie teabe leiate ka kohta, kus ühendatud süsteemi see reegel on seotud, kus see kehtib ühendatud süsteemi tüüp, ja metaversumi objekti tüüp. Metaversumi objekti tüüp on alati isiku sõltumata sellest, kui andmeallika objekti tüüp on kasutaja, iNetOrgPerson või kontakti. Metaversumi objekti tüüp kunagi peaks muuta nii, et see on loodud üldine tüüp. Lingi tüüp saate määrata Liitu, StickyJoin või säte. See säte töötab koos jaotises reeglid liituda, ja käsitletakse hiljem.

Näete ka, et see sünkroonimine reegel kasutatakse parooli sünkroonimise. Kui kasutaja on ulatus selle reegli sünkroonimine, sünkroonitakse parooli asutusesisesest pilveteenusesse (eeldades, et teil on lubatud parooli sünkroonimise funktsiooni).

#### <a name="scoping-filter"></a>Ulatuse määramine filter
Jaotises Filter ulatuse määramine abil saate konfigureerida, kui sünkroonimise reegli tuleks rakendada. Kuna vaatate sünkroonimise reegli nimi näitab, et see rakendub ainult lubatud kasutajate ulatus on konfigureeritud nii, et AD-atribuut **userAccountControl** ei tohi olla bitise 2 seadmine. Kui sync engine leiab kasutaja AD, kehtib Sünkrooni see reegel kui **userAccountControl** on seatud Kümnendväärtuse 512 (tavaline kasutaja lubatud). See ei kehti reegel, kui kasutajal on **userAccountControl** määramine 514 (tavaline kasutaja keelatud).

![Ulatuse määramine menüü sünkroonimine redaktoris ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Ulatuse filter on rühmad ja tingimused, mis võivad olla pesastatud. Sünkroonimise reegli rakendamiseks peavad olema täidetud kõik laused, kes kuuluvad rühma. Kui on määratletud mitu rühma, siis vähemalt üks rühm peab olema täidetud reegli rakendamiseks. Loogiline või hinnatakse vahel rühmad ja loogiline ja hinnatakse rühma sees. Selle konfiguratsiooni näite leiate väljaminev sünkroonimine reegel **välja AAD – rühma liituda**. On mitu sünkroonimise filtrit rühmade, näiteks üks turberühmad (`securityEnabled EQUAL True`) ja ühte levirühmade (`securityEnabled EQUAL False`).

![Ulatuse määramine menüü sünkroonimine redaktoris ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Selle reegliga saab Määratle rühmad tuleks ette valmistatud Azure AD. Levirühmade peab olema lubatud Azure AD sünkroonida e-posti, kuid turberühmad e-posti ei ole vaja.

#### <a name="join-rules"></a>Liitumine reeglid
Kuidas objektide konnektor alale, mis on seotud objektide metaversumis konfigureerimiseks kasutatakse kolmas osa. Reegel on vaadanud varasemas versioonis ei saa liituda reeglid, mis tahes konfiguratsioon hoopis te ei kavatse vaadata **sisse: AD-kasutaja liitumine**.

![Liitumine reeglite menüü sünkroonimine reegel redaktoris ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

Liitu reegli sisu sõltub installiviisardis kattuvad märgituks. Sissetuleva reegli väärtustamise algab objekti allika konnektori ruumi ja järjest hinnatakse iga rühma Liitu reegleid. Kui allika objekti hinnatakse vastavaks täpselt ühele objektile metaversumis, kasutades ühte Liitu reegleid, objektid on liitunud. Kui kõik reeglid on hinnatud ja ei leita, kasutatakse lingi tüüp lehel kirjeldus. Kui **sätte**väärtuseks selle konfiguratsiooni, siis uus objekt on loodud Target (sihtkoht) metaversumi. Uue objekti metaversumi ettevalmistamise on ka **projekti** metaversumi objekti.

Liitu reegleid analüüsitakse ainult üks kord. Konnektori ruumi objekti ja metaversumi objekti on ühendatud, jäävad nad ühendatud kui sünkroonimine reegel on täidetud.

Sünkroonimise reeglid võrdlemisel peab olema ainult üks sünkroonimine reegel koos Liitu reeglid, mis on määratletud ulatus. Kui ühele objektile mitme sünkroonimise reeglite Liitu reeglid, tõrge Expression.error. Seetõttu on parim on ainult üks sünkroonimine reegel Liitu määratletud kui objekti ulatus on mitu sünkroonimise reeglid. Azure'i AD-ühenduse sünkroonimine out box konfiguratsioon, saate reegleid vaadates nime leida ja leida sõna **liitumine** nime lõpus. Sünkroonimise reegli ilma mis tahes Liitu reeglid, mis on määratletud kehtib atribuut puhul kui teise sünkroonimise reegli ühendatud objektide või ette valmistatud soovitavas uuele objektile.

Kui vaatate pildil, saate vaadata reegel proovib liitumine **objectSID** **msExchMasterAccountSid** (Exchange) ja **msRTCSIP-OriginatorSid** (Lync), mis on, mida me oodata konto ressursi mets topoloogia. Sama reegli kõik metsade otsimine Eeldatakse, et iga mets võiks olla konto või ressursside mets. Selle konfiguratsiooni töötab ka siis, kui teil on ühe mets live ja ei pea olema ühendatud kontod.

#### <a name="transformations"></a>Teisendused
Jaotise teisendus määratleb kõigi atribuut puhul, mis rakenduvad target objekti kui objektid on liitunud ja ulatus filter on täidetud. Naasmine on **sisse: AD-kasutaja AccountEnabled** sünkroonimine reegel, leiate järgmistest teisendused:

![Teisendused vahekaardil sünkroonis redaktoris ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Selle konfiguratsiooni panna kontekstis konto ressursi mets juurutuse, on tõenäoline lubatud konto konto mets ja leida keelatud konto ressursi mets Exchange'i ja Lynci sätetega. Sünkroonimise reegli vaatate sisaldab sisselogimise jaoks nõutavad atribuudid ja nende atribuutide peaks tulenevad mets koht, kus on lubatud konto. Nende atribuutide alusel pannakse koos ühe sünkroonimine reegel.

Soovitud transformatsioon võib olla eri tüüpi: konstant, otse ja avaldis.

- Konstandi meilivoo liigub sujuvalt alati kõva väärtus. Eeltoodud näites seda määrab alati väärtuse **True** metaversumi atribuut nimega **accountEnabled**.
- Otsest meilivoo alati liigub sujuvalt atribuudi allika atribuut sihtfail nimega väärtust-on.
- Kolmanda meilivoo tüüp on avaldis ja see võimaldab täpsemate konfiguratsioone.

Avaldise keel on VBA (Visual Basic for Applications), nii et inimesed kasutussätteid, mis on Microsoft Office'i või VBScript ära vorming. Atribuudid on nurksulgudes, [attributeName]. Atribuutide nimed ja funktsiooninimed on tõstutundlikud, kuid sünkroonimise reeglite redaktori hindab avaldiste ja sisestage hoiatus, kui avaldis on kehtetu. Kõigi avaldiste puhul on väljendatud Pesastatud funktsioonide ühele reale. Konfiguratsiooni keele power kuvamiseks järgmiselt voogu pwdLastSet, kuid täiendavad lisatud kommentaarid

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Avaldise keele puhul atribuudi kohta lisateabe saamiseks vt [Mõistmine deklaratiivseid ettevalmistamise avaldised](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

### <a name="precedence"></a>Järjestuse
Saate nüüd uurinud üksikute sünkroonimise reeglitest, kuid reegleid konfiguratsiooni koos töötada. Mõnel juhul on atribuudi väärtust mitme sünkroonimise reeglitest panuse sama target atribuuti. Sel juhul kasutatakse atribuudi järjestus määrata, milline atribuut võitis. Näiteks, vaadake sourceAnchor atribuut. See atribuut on oluline atribuut saama Azure AD sisse logida. Leiate atribuudi voog on kaks erinevat sünkroonimise reeglid selle atribuudi **sisse: AD-kasutaja AccountEnabled** ning **sisse: AD-kasutaja levinud**. Sünkroonimise reeglite järjestuse, tõttu sourceAnchor atribuut on kaasa lubatud konto pärineb esmalt kui seal on mitu objekti, mis on ühendatud metaversumi objekti. Kui kontosid pole lubatud, siis sync engine kasutab kõikehõlmav sünkroonimine reegel **sisse: AD-kasutaja levinud**. Selle konfiguratsiooni tagab, et isegi jaoks kontod, mis on keelatud, on veel mõne sourceAnchor.

![Sissetulev sünkroonimise reeglid](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Sünkroonimise reeglite järjestuse asub rühmade installimise viisard. Kõigi rühma on sama nime, kuid need on ühendatud muu ühendatud kataloogide. Installimise viisard annab reegli **sisse: AD-kasutaja liitumine** kõrgeim järjestuse ja itereerib üle kõik ühendatud kataloogide AD. Seejärel jätkub järgmise rühmad reeglid eelmääratletud järjestuses. Sees rühma, lisatakse reegleid konnektorid on lisatud viisardi järjestuses. Kui teine konnektor on lisatud viisardiga, sünkroonimise reeglid on ümber rühmitada ja uue konnektori reeglid lisatakse iga rühma viimase.

### <a name="putting-it-all-together"></a>Pannes kõik kokku
Nüüd teavad piisavalt sünkroonimise reeglite saama mõista, kuidas konfiguratsiooni töötab koos muu sünkroonimise reeglite kohta. Kui vaatate kasutaja ja atribuute, mis on panuse metaversumi reeglite rakendamise järgmises järjestuses:

Nimi | Kommentaari
:------------- | :-------------
Klõpsake AD-ühendus kasutaja kaudu | Liitumine konnektori ruumi objektid metaversumi reegel.
Klõpsake: AD-foorumisse lubatud | Atribuute, mis on vaja Azure AD sisselogimine ja Office 365. Soovime neid atribuute lubatud konto kaudu.
Klõpsake AD-kasutaja levinud Exchange'ist kaudu | Globaalsest aadressiloendist leitud atribuute. Oleme endale andmete kvaliteedi on parim mets, kus on leitud kasutaja postkasti.
Klõpsake AD-kasutaja levinud kaudu | Globaalsest aadressiloendist leitud atribuute. Juhuks, kui me ei leidnud postkasti, aitab mõne muu ühendatud objekti atribuudi väärtus.
Klõpsake AD-kasutaja Exchange'i kaudu | Olemas vaid siis, kui Exchange on avastatud. Juhtimine kõik taristu Exchange atribuute.
Klõpsake AD-kasutaja Lynci kaudu | Olemas vaid siis, kui Lync on avastatud. Kõigi taristu Lynci atribuutide juhtimine.

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet konfiguratsiooni mudeli [Mõistmine deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning.md)kohta.
- Lugege lisateavet avaldis keele avaldistes [mõistmine deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)kohta.
- Jätkake lugemist out box konfiguratsiooni [mõistmine kasutajad](active-directory-aadconnectsync-understanding-users-and-contacts.md) ja kontaktide tööpõhimõtted
- Vaadake, kuidas abil, kuidas [muuta vaikimisi konfiguratsiooni](active-directory-aadconnectsync-change-the-configuration.md)deklaratiivseid ettevalmistamise praktiline muudatuste tegemine.

**Teemade ülevaade**

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
