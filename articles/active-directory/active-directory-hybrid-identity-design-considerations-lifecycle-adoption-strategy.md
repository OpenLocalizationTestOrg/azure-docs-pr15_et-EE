
<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratleda hübriid identiteedi elutsükli kasutuselevõtustrateegia arendamine | Microsoft Azure'i"
    description="Aitab määratleda hübriid identiteedi haldamise toiminguid iga etapi elutsükli saadaolevad suvandid sõltuvad."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Hübriid identiteedi elutsükli kasutuselevõtu strateegia määratlemine
Selles ülesandes tuleb määratleda identiteedi haldamise strateegia [määratlemine hübriid identiteedi haldustoimingute](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)määratud business nõuete täitmiseks oma hübriid identiteedi lahenduse.


Hübriid identiteedi haldustoimingute vastavalt eespool toodud selle etapi lõpuni identiteedi elutsükli määratlemiseks peate võimalusi elutsükli etappide silmas pidada.

## <a name="access-management-and-provisioning"></a>Juurdepääs haldus ja ettevalmistamine
Hea konto juurdepääsu halduse lahenduse, teie asutuses saate jälgida täpselt kellel on juurdepääs millist teavet terves asutuses.

Juurdepääsu reguleerimine on kriitiline funktsioon tsentraliseeritud, ühe punkti ettevalmistamise süsteem. Lisaks kaitsta tundlikku teavet, jätke Accessi juhtelemendid olemasolevaid kontosid, millel on kinnitamata lube või on pole enam vajalikud. Määrata aegunud kontod, ettevalmistamise süsteemi lingid koos kontoteabe autoriteetsete kasutajad, kes oma kontode kohta lisateabe saamiseks. Autoriteetsete kasutaja identiteedi teave on tavaliselt alles andmebaasid ja kataloogid Personalihaldus.

Keerukate IT ettevõtetes sisaldavad sadu parameetrid, mis määratlevad asutused ja neid andmeid saab kontrollida ettevalmistussüsteemi. Uued kasutajad saavad tuvastatud andmetega varustavate autoriteetsete lähtekoha. Accessi taotluse kinnitamine võimalus käivitab protsessid, mis neid ettevalmistamise ressursside kinnitamine (või tagasilükkamine).


| Elutsükli haldus etapp.          | Kohapealne                                                                                                                                                                                                                                                       | Pilveteenuse                                                                                                                                                                                                                                                                                                                     | Hübriid                                                                                   |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Konto haldamine ja ettevalmistamine | Roll server® Active Directory domeeniteenused (AD DS) abil saate luua scalable, turvalist ja mõistliku taristu kasutaja- ja ressursihalduse, ja directory toega rakendusi nagu Microsoft® Exchange Serveri toe pakkumine. <br><br> [Te saate ettevalmistamise rühmade AD DS-is on identiteedi halduri kaudu](https://technet.microsoft.com/library/ff686261.aspx) <br>[Te saate ettevalmistamine kasutajate AD DS-is](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Administraatorid saavad hallata kasutajate juurdepääsu ühiskasutusega ressurssidele turvalisuse huvides kasutada juurdepääsu reguleerimine. Active Directory, juurdepääsu reguleerimine on manustatud objekti tasemel säte eri tasemed juurdepääsu või õigused, objektid, nt Täielik kasutusõigus, kirjutamine, lugege teemat või puudub juurdepääs. Juurdepääsu reguleerimine Active Directory määratleb erinevate kasutajate saate kasutada Active Directory objekti. Vaikimisi on seatud objektide Active Directory õiguste kõige turvalisem säte. | Peate looma konto igale kasutajale, kes pääsevad Microsofti pilveteenuses. Saate muuta kasutajakontode või need kustutada, kui nad ei ole enam vaja. Kasutajad ei saa administraatoriõigused vaikimisi, kuid soovi korral saate määrata need. Lisateavet leiate teemast [Azure AD kasutajate haldamine](active-directory-create-users.md). <br><br> Azure Active Directory, üks olulisemad funktsioonid on võimalus ressursse juurdepääsu haldamine. Järgmiste ressursside saab osa kataloogi, nagu õiguste haldamiseks objektide kataloogi või ressursse, mis on välise kataloogi, nt SaaS rakendused, Azure'i teenused ja SharePointi saitide või eeldusel ressursid rollide kaudu. <br><br> Lahendus on keskele, Azure Active Directory's Accessi turberühma. Ressursi omanik (või administraator kataloogi) saate määrata teatud juurdepääsu õigus nad oma ressursside rühma. Rühma liikmete antakse juurdepääs ja ressursside omanik saab volitatud esindaja õigus kellelegi teisele – nt osakonna juhataja või kasutajatoe administraator rühma liikmete loendi haldamine<br> <br> Rühmade haldamine Azure AD teemas antakse lisateavet rühmade kaudu juurdepääsu haldamine.| Active Directory identiteedid pilve kaudu sünkroonimise ja federation laiendamine |

## <a name="role-based-access-control"></a>Rollipõhine juurdepääsu reguleerimine
Rollipõhine juurdepääsu reguleerimine (RBAC) kasutab rollid ja ettevalmistamise poliitikate hindamaks, testige ja rakendada oma äriprotsesside ja kasutajatele juurdepääsu andmine reeglid. Võtme administraatorid ettevalmistamise poliitikate loomine ja määrata kasutajatele rollid ja mis määratlevad õiguste kogumi ressursse järgmised rollid. RBAC laiendab identiteedi haldamine lahendus kasutada tarkvara protsesside ja kasutajale käsitsi ebausaldusväärsete vähendamiseks.
Azure'i AD RBAC võimaldab ettevõtte piirata toimingute kohta, mida inimesel saab teha, kui ta on juurdepääs Azure'i haldusportaal summa. Kasutades RBAC juurdepääsu portaali, lähenedes IT-administraatorid ca Delegaadi juurdepääs, kasutades järgmist juurdepääsu juhtimine:

- **Rühma-põhiste Rollimäärang**: saate määrata juurdepääsu Azure AD rühmad, mida saate sünkroonida kohaliku Active Directoryst. See võimaldab teil ära kasutada olemasoleva investeeringud teie ettevõttes on tehtud instrumentaarium ja protsesside haldamiseks rühmad. Saate jaotises delegeeritud halduse funktsioon Azure AD Premium.
- **Võimendada sisseehitatud rollid Azure**: saate kasutada kolme rolli – omanik, kaasautor ja lugeja, veenduge, et kasutajad ja rühmad on õigus teha ainult oma tööd teha toiminguid.
- **Granular juurdepääsu ressursse**: saate määrata kasutajate ja rühmade teatud tellimuste puhul, ressursirühm või on üksikute Azure ressurss, nt veebisaidile või andmebaasi rollid. Nii saate tagada, et kasutajatel on juurdepääs kõik vajalikud ressursid ja puudub juurdepääs ressursse, mis ei pea haldamiseks.

## <a name="provisioning-and-other-customization-options"></a>Ettevalmistamise ja muude suvandite kohandamine
Teie meeskond saab kasutada ettevõttelepingud ja nõuded otsustada palju kohandamiseks identiteedi lahendus. Näiteks suurettevõte võivad nõuda etappidega välja kava töövood ja kohandatud adapterit, artistide ettevalmistamise rakendused, mida kasutatakse üle kaugemad ajakava põhjal. Leping mõne muu kohandamine võib ette kahe või enama rakenduste kogu kogu ettevõttele pärast eduka testimine olema ette valmistatud. Kasutaja rakendus suhtluse saab kohandada, ja toiminguid ettevalmistamise ressursid võib automatiseeritud ettevalmistamise mahutamiseks muuta.

Saate deprovision eemaldamiseks teenuse või komponent. Näiteks deprovisioning konto tähendab, et konto kustutatakse ressursi.

Hübriidjuurutuse mudel ettevalmistamise ressursid ühendab taotlus ja Rollipõhine lähenemisel, mida toetatakse nii Azure AD. Töötajate või hallatavate süsteemide alamhulga, võiksite äri automatiseerida ülesande Rollipõhine juurdepääsu. Ettevõtte võib toime ka kõik muud Juurdepääsutaotluste või erandid taotluse vastavalt mudeli kaudu. Mõned ettevõtted võib alustada käsitsi ülesanne ja arenevad hübriid mudel, täielikult Rollipõhine juurutamise tulevaste huvi suunas.

Teistes ettevõtetes, võib leida äri põhjustel täieliku Rollipõhine ettevalmistamise saavutamiseks otstarbekas ja suunata hübriid lähenemine soovitud eesmärk. Veel teistes ettevõtetes, võib rahul ainult taotluse vastavalt ettevalmistamise ja soovi investeerida täiendavad peegeldav määratlemine ja Rollipõhine, automatiseeritud ettevalmistamise poliitikate haldamine.

## <a name="license-management"></a>Litsentside haldamine
Rühma-põhiste litsentsihalduse Azure AD abil määrata kasutajatele turberühm administraatorid ja Azure AD automaatselt määrab litsentsid rühma liikmete. Kui kasutaja on hiljem lisada või rühmast eemaldatud, kuvatakse litsentsi automaatselt määratud või eemaldatakse vastavalt vajadusele.

Saate kasutada rühmade sünkroonite asutusesisese AD või Azure AD haldamine. Sidumine see Azure AD Premium iseteenindusliku rühma haldamine saate hõlpsalt delegeerida litsentsi määramine vastav otsuste tegijatele. Võite olla kindel, et probleeme, näiteks litsentsikonflikte ja asukohaandmed puuduvad sorditakse automaatselt.

## <a name="self-regulating-user-administration"></a>Omas reguleerimise kasutajate haldamine
Ettevõtte käivitamisel ettevalmistamise ressursid üle kõik sisemise ettevõtted saate rakendada omas reguleerimise kasutaja halduse võimalus. Saate kogu ettevõtte piirmäärad aru eelised ja kasu ettevalmistamise kasutajad. Selles keskkonnas kasutaja olek muutub automaatselt kajastub juurdepääsuõigused kogu ettevõttes piirmäärad ja kaugemad. Saate vähendada ettevalmistamise ja protsesse juurdepääsu ja kinnitamine. Rakendamist mõistab rakendamise Rollipõhine juurdepääsu reguleerimine-lõpuni juurdepääsu haldamiseks ettevõtte kõiki võimalusi. Saate vähendada halduskulud automatiseeritud reguleerivate kasutaja ettevalmistamise kaudu. Turvalisuse täiustamine turbepoliitika jõustamine, automatiseerimine ja sujuvamaks muutmine ja koondada kasutaja elutsükli haldus ja ressursside ettevalmistamise populatsiooni suur kasutaja jaoks.

>[AZURE.NOTE]
Lisateabe saamiseks lugege teemat Azure AD ise teenust access Rakendusehaldus häälestamise

Litsentsi vastavalt (õiguse-põhine) Azure AD teenuste töö aktiveerides oma Azure AD/kataloogiteenusest rentniku tellimus. Kui tellimus on aktiivne teenuse võimaluste saab hallata/kataloogiteenusest administraatorid ja kasutada litsentsitud kasutajad. Lisateabe saamiseks lugege teemat Kuidas mõjutab litsentsimise töö Azure AD?
3. teiste pakkujate integreerimine

Azure Active Directory annab ühekordse sisselogimise ja rakenduse juurdepääsu turvalisuse tuhandeliste SaaS rakenduste ja kohapealse veebirakenduste täiustatud. Azure Active Directory rakenduse Galerii SaaS rakenduste üksikasjaliku loendi leiate Azure'i Active Directory federation ühilduvuse loend: kolmanda osapoole identiteedipakkujad, mida saab kasutada ühekordse sisselogimise rakendamiseks

## <a name="define-synchronization-management"></a>Sünkroonimise juhtimise määratlemine
Teie kohapealse kataloogide integreerimine Azure AD muudab kasutajate veel tõhusamalt töötada ühise identiteedi esitada ressursse asutusesisese ja pilvepõhise juurdepääsu. See integreerimise kasutajad ja ettevõtted saate ära järgmist:

- Ettevõtted, saate sisestada levinud hübriid identiteedi kasutajad üle kohapealsesse või pilvepõhist teenuste kasutamine Windows Server Active Directory ja seejärel ühenduse Azure Active Directory.
- Administraatorid saavad tingimusvormingu juurdepääsu ressurssi, seadme ja kasutaja identiteet, võrgukoha ja mitmikautentimise.
- Kasutajate saate kasutada oma kontode kaudu levinud identiteedi Azure AD teenusekomplekti Office 365, Intune SaaS rakenduste ja muude tootjate rakendused.
- Arendajatel luua rakendusi, mis levinud identiteedi mudel, kasutada rakendusi integreerida kohapealse Active Directory või Azure pilvepõhist rakenduste

Järgmisel joonisel on kujutatud identiteedi sünkroonimine ületaseme vaadet.

![](./media/hybrid-id-design-considerations/identitysync.png)

Identiteedi sünkroonimine

Vaadake üle järgmise tabeli abil saate võrrelda sünkroonimise suvandid.

| Sünkroonimise juhtimise suvand          | Eelised                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Puudused                                                                                                                                                                                                                                                                                  |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sünkroonimine-põhise (DirSync või AADConnect) kaudu | Kasutajad ja rühmad, mis on sünkroonitud kohapealse ja pilveteenuse <br>  **Poliitika juhtelement**: konto poliitikate saate määrata kuni Active Directory, mis annab administraatori võimalus haldamine parool poliitika, töökoha, piirangud ja lukustab juhtelemendid, ja rohkem, ilma et täita pilveteenuses.  <br>  **Juurdepääsu reguleerimine**: saate piirata juurdepääsu pilveteenusesse nii, et teenuste pääseb ettevõttekeskkonnas Online'i serverid või mõlemad kaudu. <br>  Piiratud tugi kõned: kui kasutajatel on vähem paroole meeles pidada, nad on vähem tõenäoliselt unustada. <br>  Turvalisus: Kasutaja identiteedi ja teave on kaitstud, kuna kõik serverid ja teenuseid kasutada ühekordse sisselogimise, on õppinud ja kohapealse kontrollitud. <br>  Tugeva autentimise tugi: tugev autentimine (nimetatakse ka kahekordne autentimine) saate kasutada koos pilveteenusesse. Juhul, kui kasutate tugev autentimine, peate kasutama ühekordse sisselogimise. |                                                                                                                                                                                                                                                                                                |
| Federation-põhise (AD FS) kaudu           | Lubatud, Turve Turbeloa teenus (STS). Kui konfigureerite mõne STS ühekordse sisselogimise juurdepääsu anda Microsofti pilveteenuses, siis tuleb luua välise usalda oma kohapealse STS ja ühendatud domeenis, mida olete oma Azure AD rentniku vahel. <br> Võimaldab lõppkasutajal sama mandaadikomplekt abil saate tutvuda mitme ressursse <br>lõppkasutajad pole säilitamiseks mitmele kriteeriumikogumile mandaat. Veel, kasutajad on oma mandaati igale osalevate ressursse., B2B ja B2C stsenaariumid, mis on toetatud.                                                                                                                                                                                                                                                                                                                                                                                                             | Jaoks on vaja eriotstarbelisi töötajatele juurutamise ja pidamise sihtotstarbeline kohapeal AD FS-i serverid. Kui plaanite oma STS AD FS-i kasutamiseks on tugeva autentimise kasutamise piirangud. Lisateavet leiate teemast [Konfigureerimise Täpsemad suvandid AD FS 2.0](http://go.microsoft.com/fwlink/?linkid=235649). |

>[AZURE.NOTE]
Lisateavet leiate teemast [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).


## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
