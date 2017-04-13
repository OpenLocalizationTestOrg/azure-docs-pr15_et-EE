<properties
    pageTitle="Rakenduste Azure Active Directory haldamine | Microsoft Azure'i"
    description="See artikkel oma kohapealse, pilveteenuste ja SaaS rakenduste Azure Active Directory integreerimise eelised."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Azure Active Directory rakenduste haldamine

Lisaks tegelik töövoo või sisu, ettevõtetele on kaks täitmiseks kõik rakendused.

1. Tootlikkuse suurendamiseks rakenduste peaks olema lihtne leida ja juurdepääs

2. Turvalisus ja juhtimise lubamiseks peab organisatsiooni juhtelement ja kes saavad ja tegelikult on juurdepääs konkreetse rakenduse järelevalvet

Maailma pilv rakendusi selle kõige saavutamiseks kasutab ",*kellel on õigus seda, mida*" kontrolli identiteeti.

Arvuti terminoloogia:

- *Kes* on tuntud *identiteedi* - kasutajate ja rühmade haldamine

- *Mis* on tuntud *juurdepääsu haldamine* – kaitstud ressurssidele juurdepääsu haldamine

Mõlemad komponendid koos nimetatakse *identiteedi ja juurdepääsu juhtimine (IAM)*, mis on määratletud [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) rühma nimega "*Turvalisus distsipliin, mis võimaldab õige üksikisikute juurdepääsuks õige ressursside paremas servas korda õigust põhjustel*".

Olgu, mis on probleem? Kui IAM on *ei hallata* ühes kohas koos integreeritud lahendust.

- Identiteedi administraatoritel on eraldi loomine ja värskendamine kõigis rakendustes kasutajakontode eraldi, liigsed ja aeganõudev tegevus.

- Kasutajatel on meeles rakendustele, tuleb need töötada mitme mandaat. Selle tulemusena kasutajad tavaliselt kirjutage oma parooli või muude parooli lahendusi, mis tutvustab muude andmete ohtu turvalisusele kasutamiseks.

- Liigsete aeganõudev tegevuste vähendada aja kasutajate ja administraatorite töötavad äritegevuse, mis teie ettevõtte loosung suurendada.

Jah, mis takistab üldiselt ettevõtted IAM integreeritud lahenduste vastu võtta?

- Kõige lahenduste põhinevad tarkvara platvormid, mida on vaja juurutada ja kohandada oma rakenduste iga ettevõtte.

- Pilveteenuse rakendused on sageli kõrgem kui IT-osakond saate integreerida olemasolevate IAM lahenduste vastu.

- Turvalisus ja jälgimine instrumentaarium jaoks on vaja täiendav kohandamine ja integreerimine täielik E2E stsenaariumid saavutamiseks.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory integreeritud rakendused

Azure Active Directory on Microsofti täielik identiteedi lahendusena teenuse (IDaaS) mis:

- Lubab IAM nimega pilveteenus 

- Annab keskse juurdepääsu juhtimine, ühekordse sisselogimise (SSO) ja aruannete 

- Toetab integreeritud [rakenduste tuhandeliste](https://azure.microsoft.com/marketplace/active-directory/) rakenduse Galerii, sh Salesforce, Google Appsi, väljale, Concur ja juurdepääsu juhtimine. 


Azure Active Directory, kõik rakendused avaldada oma partneritele ja klientidele (ettevõte või tarbija) on sama identiteedi ja juurdepääs haldusvõimalusi.<br> See võimaldab teil oma tegevuse kulude märkimisväärselt vähendamiseks.

Mida teha, kui soovite rakendada rakendus, mis pole veel rakenduse Galerii loendis? Kuigi see on veidi aeganõudvamaks juhul, kui SSO rakenduste galeriist rakenduse konfigureerimise, pakub Azure AD viisardi, mis aitab teil konfiguratsioon.

Azure AD väärtus ületab "lihtsalt" pilv rakendusi. Samuti saate selle asutusesisese rakendustega turvaline Kaugpöördusteenuse esitada. Turvaline Kaugpöördusteenuse, saate eemaldada selle VPN või muude traditsiooniline Kaugpöördusteenuse haldus rakendusi.

Kõikide rakenduste keskses Accessi haldus ja ühekordse sisselogimise (SSO) ette Azure AD pakub lahendus peamine andmete turbe- ja tootlikkuse probleeme.

- Kasutajad pääsevad mitme rakenduste ühe sisselogimise annab rohkem aega tulu loomisel või ettevõtte toimingute tegevusi teha.

- Identiteedi administraatorid saavad hallata ühes kohas rakendustele juurdepääsemiseks.

Kasutaja ja oma ettevõttele kasu on selge. Vaatame lähemalt eeliste administraator identiteedi ja oma asutuse jaoks.

## <a name="integrated-application-benefits"></a>Integreeritud rakenduste eelised

SSO protsess on kaks toimingut.

- Kasutaja identiteedi valideerimine autentimine.

- Luba, luba või ploki juurdepääsu ressursile juurdepääsu poliitika koos otsus.

Kui kasutate Azure AD rakenduste haldamine ja lubamine SSO:

- Kasutaja asutusesisese (nt AD) või Azure AD konto autentimine on tehtud.

- Luba aktiveeritakse Azure AD ülesanne ja kaitsmise kohta tagada ühtsete lõppkasutaja kogemuse kohta ning võimaldab teil lisada ülesande, asukohad ja MFA tingimuste mis tahes rakenduses sõltumata selle sisemine võimaluste kohta.

Mõistmaks, et nii, nagu luba on jõustatud sihtrakenduse kohta on oluline, sõltub sellest, kuidas rakenduse integreeritud Azure AD.

- **Rakenduste integreeritud eelse teenuse pakkuja** Office 365 ja Azure, nagu need on rakenduste tugineb Azure AD ja tuginedes selle oma täielik identiteedi ja juurdepääsu halduse võimaluste kohta. Kataloogi andmed ja Turbeloa emissiooni kaudu on lubatud juurdepääs need rakendused.

- **Rakenduste eelnevalt integreeritud Microsoft ja kohandatud rakendused.** Need on sõltumatute pilv rakendusi, mis on sisemine rakenduse kataloogis toetuvad ja töötavad sõltumata Azure AD. Juurdepääs need rakendused on lubatud väljastanud rakenduse teatud mandaat konto rakenduse vastendatud. Sõltuvalt rakenduse võimaluste, mandaat võib olla federation luba või kasutajanime ja parooliga kontot, mis on varem ette valmistatud rakenduse jaoks.

- **Kohapealse rakendused** Rakenduste avaldatud peamiselt lubamine kohapealse rakendustele juurdepääsemiseks Azure AD Rakenduse puhverserveri kaudu. Need rakendused sõltuvad keskne eeldusel directory nagu Windows Server Active Directory kohta. Juurdepääs need rakendused on lubatud puhverserverist käivitavad pakkuda rakenduse sisu lõppkasutaja samas austus kohapealse sisselogimise nõue.

Näiteks kui kasutaja liitub ettevõtte, peate looma konto kasutaja esmase sisselogimise toimingute Azure AD. Kui selle kasutaja jaoks on vaja näiteks Salesforce'i hallatava rakenduse juurdepääsu, peate selle kasutaja jaoks konto loomist Salesforce'i ja linkida selle Azure'i konto SSO töö tegemiseks. Kui kasutaja lahkub teie asutuses, on soovitatav Azure AD konto kustutada ja kõik töölauafunktsioonid kontod on IAM talletab kasutajal oleks juurdepääs rakendusi.

## <a name="access-detection"></a>Accessi avastamine

Tänapäevane ettevõtetes IT-osakondade ei ole sageli teadlikud cloud kõik rakendused, mida kasutatakse. Pilveteenuse rakenduse Discovery koos, Azure AD pakub teile lahenduse tuvastamiseks need rakendused.

## <a name="account-management"></a>Konto haldamine

Traditsiooniliselt konto rakenduste haldamine on käsitsi tehtud või toetavad organisatsiooni isiklik. Azure AD täielikult automatiseeritud kontohaldus üle kõik teenuse pakkuja integreeritud rakendused ja need ette toetavad automatiseeritud kasutaja ettevalmistamise Microsoft või SAML JIT integreeritud rakendused.

## <a name="automated-user-provisioning"></a>Automatiseeritud kasutajate ettevalmistamine

Mõni rakendus võimaldab automatiseerimise liideste loomine ja eemaldamine (või desaktiveerimine) kontode jaoks. Kui pakkuja pakub sellise liidest, on see kasutada Azure AD. See vähendab oma tegevuse kulud, kuna haldustoimingute tehakse automaatselt ja parandab keskkonna turvalisust, kuna see vähendab volitamata juurdepääsu võimalus.

## <a name="access-management"></a>Juurdepääsu juhtimine

Azure AD abil saate hallata juurdepääsu rakendusi kasutades üksiku või reegli juhitud ülesanded. Saate ka volitatud esindaja juurdepääsu juhtimine, et õigetel inimestel ettevõtte tagada parima haldusrühmadest ja kasutajatugi koormuse vähendamiseks.

## <a name="on-premises-applications"></a>Kohapealse rakendused

Sisseehitatud rakenduse puhverserveri võimaldab teil avaldada oma kohapealse rakendusi kasutajatele tulemuseks nii ühtsete pääseda kogemus tänapäevane rakendus ja eeliste Azure AD jälgimine, aruandlus ja turvalisuse võimalusi.

## <a name="reporting-and-monitoring"></a>Aruandlus- ja jälgimine

Azure AD annab teile eelnevalt integreeritud aruandlus- ja jälgimise võimalusi, mille abil saate teada, kellel on juurdepääs rakendustele ja neid tegelikult neid kasutada.

## <a name="related-capabilities"></a>Seotud funktsioonid

Azure AD saate turvaline Varundustöö juurdepääsupoliitikaid ja MFA eelnevalt integreeritud rakenduste. Lisateavet Azure MFA leiate [Azure'i MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Alustamine

Alustamiseks rakenduste integreerimine Azure AD Heitke pilk [Alustamisjuhend integreerimine Azure Active Directory saada rakendustega](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Vt ka

[Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
