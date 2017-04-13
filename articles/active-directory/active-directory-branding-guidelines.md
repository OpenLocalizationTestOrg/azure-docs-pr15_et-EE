<properties
   pageTitle="Juhised rakenduste sümboolika | Microsoft Azure'i"
   description="Mahuka Azure Active Directory arendaja rakendusse ressursid"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Juhised rakenduste sümboolika


Selles teemas käsitletakse tootemargi juhised, kasutage rakenduste Azure Active Directory (Azure AD). Nende juhiste abil otse oma klientidele, kui kasutajad soovivad oma töökoha või kooli kontoga hallatavate Azure AD, või oma isikliku konto registreerumise ja rakenduse sisselogimine.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Isiklikud kontod vs töö või kooli meilikonto Microsofti

Microsoft haldab kahte tüüpi Kasutajakontod:

- **Isiklikud kontod** (varem Windows Live ID). Nende kontodega tähistada *üksikkasutajate* ja Microsoft seos ja kasutatakse Microsofti tarbija seadmed ja teenuste juurde. Need kontod on mõeldud isiklikuks kasutamiseks.

- **Töö või kooli kontot.** Nende kontodega haldab Microsoft Azure Active Directory kasutavate nimel. Nende kontodega kasutatakse Microsoft Office 365 ja muude teenustega business sisse logida.

Microsofti töö-või koolikonto on tavaliselt määratud lõppkasutajad (töötajad, õpilaste, federal töötajad) abil saavad ettevõtted (ettevõte, kooli, government asutuse). Nende kontodega on kas otse pilves, Azure AD platvormi, õppinud või sünkroonitud Azure AD kohapealse kataloogi, näiteks Windows Server Active Directory. Microsoft on *halduri* töö- või koolikonto kontod, kuid omanik ja kontrollib asutuse aruandeid.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Viidates rakenduse Azure AD kontod

Microsoft ei jätke lõppkasutajad on Azure või Active Directory kaubamärgi nimed ja te ei peaks.

- Kui kasutaja on sisse logitud, kasutage ettevõtte nimi ja logo abil võimalikult palju. See on parem kui üldise tingimustel nagu "ettevõtte".

- Kui kasutaja pole sisse logitud, tutvuge nende kontod nimega "töö või kooli meilikonto" ja kasutada Microsofti logo edasi, et nende kontodega haldab Microsoft. Ärge kasutage termineid nagu "enterprise konto", "business konto" või "ettevõtte konto", mis tekitada segadust kasutaja.

## <a name="user-account-pictogram"></a>Kasutaja konto piktogramm
Varasemates versioonides neid juhiseid, soovitame piktogramm "sinine märgi" abil. Kasutaja ja arendaja tagasiside põhjal, soovitame nüüd Microsoft logo kasutamise asemel. See aitab kasutajatel mõista, et nad need Teenusekomplektis Office 365 või muu Microsoft business services abil oma rakendusse sisse logida konto uuesti kasutada.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Registreerumine ja Azure AD sisse logida

Rakenduse võib põhjustada eraldi teed sisse logida ja logige sisse ja järgmistest jaotistest leiate mõlemal juhul visuaalsed juhised.

**Kui teie rakendus toetab lõppkasutaja märk (nt tasuta prooviversiooni või freemium mudel) üles**: **sisselogimise** nupu, mis võimaldab kasutajatel teie rakenduse oma töö või oma isikliku konto saate kuvada. Azure AD kuvatakse viip nõusolekut juurdepääs rakenduse esimest korda.

**Kui teie rakenduse jaoks on vaja õigused, mis ainult administraatorid saavad nõustu, või oma rakenduse jaoks on vaja ettevõtte litsentsid**: eraldate administraator omandamise kaudu kasutaja Logi sisse. **Nupp "saada see rakendus"** suunab administraatorid sisse logida, siis paluge tal anda oma asutuse kasutajate nimel nõusolekut. See on lisatud kasu allasurumine lõppkasutajad nõusolekut viipasid rakenduse.

## <a name="visual-guidance-for-app-acquisition"></a>Visuaalsed juhised rakenduse saamine

Link "rakenduse hankimine" peate häälestama ümbersuunamise kasutaja Azure AD juurdepääsu andmine (luba) lehe ettevõtte administraator lubada juurdepääsu oma ettevõtte andmeid, mis on majutatud Microsoft rakenduse lubamiseks. [Azure Active Directory rakenduste integreerimise](active-directory-integrating-applications.md) artiklis käsitletakse juurdepääsu taotlemise kohta.

Pärast administraatorid nõustu rakenduse, saate need lisada nende kasutajate teenusekomplekti Office 365 rakenduse käiviti kasutamine (hõlbustusfunktsioonidega rakendusekäiviti ja [https://portal.office.com/myapps](https://portal.office.com/myapps)). Kui soovite reklaamida seda võimalust, saate kasutada termineid, näiteks "Lisa see rakendus ettevõtte" ja kuvada nupu umbes järgmine:

![Rakenduse tüübid ja stsenaariumid](./media/active-directory-branding-guidelines/add-to-my-org.png)

Soovitame siiski kirjutada, selle asemel nupud selgitav tekst. Näiteks:
> *Kui kasutate juba Office 365 või muu Microsofti teenuse business, saate lihtsalt < your_app_name > ettevõtte andmetele juurdepääsu anda. See võimaldab juurde pääseda oma töö olemasoleva kontoga < your_app_name > Kasutajad.*


## <a name="visual-guidance-for-sign-in"></a>Visuaalsed juhised sisselogimine
Rakenduse peaks olema kuvatud nupp, mis suunab kasutaja sisselogimise lõpp-protokolli, saate integreerimine Azure AD vastav märk. Järgmine jaotis sisaldab üksikasju mis see nupp peaks välja nägema.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogramm ja "Logite sisse Microsofti"
See on Microsofti logo seost ja Azure AD muude identiteedipakkujad seas kordumatult tähistava "Logige sisse Microsoft" tingimustel võib teie rakendus toetab. Kui teil pole piisavalt ruumi "Logige sisse Microsoft", on see lühendamise "Sisse logida" ok.

![Rakenduse tüübid ja stsenaariumid](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Rakenduse tüübid ja stsenaariumid](./media/active-directory-branding-guidelines/sign-in-light.png)

Samuti saate nuppude tume värviskeem.

![Rakenduse tüübid ja stsenaariumid](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Rakenduse tüübid ja stsenaariumid](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Sümboolika käsud ja keelud

**Kasutage "töökoha või kooli kontoga" koos "Logige sisse Microsoft" nuppu täiendavad selgitada abil lõppkasutajad tuvastada, kas ta saab seda kasutada.** **Ärge** kasutage muid tingimusi, näiteks "enterprise konto", "business konto" või "ettevõtte konto."

**Ärge** kasutage "Office 365 ID" või "Azure ID". Office 365 on ka Microsoftilt, mis ei kasuta Azure AD autentimise pakub tarbija nimi.

Microsofti logo **ei** Muuda.

**Ärge** jätke Azure'i või Active Directory kaubamärkide lõppkasutajatele. See on ok siiski kasutada järgmiste terminite tähendust arendajad, IT-spetsialistid ja administraatoritele.

## <a name="navigation-dos-and-donts"></a>Navigeerimise käsud ja keelud

**Ei** võimalda kasutajatel logige välja ja aktiveerige teine kasutajakonto. Kuigi enamik inimesi on ühe isikliku konto Microsoft, Facebooki, Google ja Twitter, inimesed on sageli seotud mitu organisatsiooni. Mitme sisselogitud kasutaja tugi on tulekul.
