<properties
    pageTitle="Mis on Microsoft Azure Active Directory litsentsimise? | Microsoft Azure'i"
    description="Microsoft Azure Active Directory litsentsimise kirjeldus, kuidas see toimib, kuidas alustada ja head tavad, sh Office 365, Microsoft Intune ja Azure Active Directory Premium ja põhilised väljaanded"
    services="active-directory"
      keywords="Azure'i AD litsentsimine"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>Mis on Microsoft Azure Active Directory litsentsimise?

##<a name="description"></a>Kirjeldus
Azure Active Directory (Azure AD) on Microsofti Identity teenuse (IDaaS) lahenduse ja platvorm. Azure AD pakutakse arv vahemikus Azure AD tasuta, mis pole saadaval, mis tahes Microsofti teenusega Office 365, Dynamics, Microsoft Intune'i ja Azure tehnilised versioonide (Azure AD genereerida tarbimine tasud selles režiimis), Azure AD makstud versioonid, näiteks ettevõtte mobiilsus komplekti (EMS), Azure AD Premium ja Basic, samuti Azure mitme teguriga autentimine (MFA). Nagu paljud teenuse Microsoft online services, edastatakse kõige Azure AD makstud versioonid kasutaja õiguste kaudu, kui need on Office 365, Microsoft Intune'i ja Azure AD. Sellisel juhul teenuste ostmine on esindatud üks või mitu tellimust ja iga tellimus sisaldab teie rentnik eelnevalt ostu litsentside arvu. Kasutaja õiguste on saavutada litsentsi määramine, kasutaja ja teenuse komponendid kasutaja lubamine ja tarbimine ühte ettemakstud litsentside toote vahelise seose loomine.

[Proovige Azure AD premium kohe.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Azure'i AD administreerimiskeskuse portaalis on Azure klassikaline portaali osa. Ajal abil Azure AD ei nõua mis tahes Azure'i oste, juurdepääs selle portaali nõuab Azure active tellimuse või on [Azure prooviversiooni tellimuse](https://azure.microsoft.com/pricing/free-trial/).

Lai ülevaadet Azure AD teenuse võimaluste kohta teemast [mis on Azure AD](active-directory-whatis.md).
[Lisateavet Azure AD teenuse tasemed](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Azure'i maksa tellimused on erinevad: kui ka esindatud kataloogi, järgmistest tellimustest Azure ressursid loomise lubamiseks ja Vastendage need makseviisi. Selles näites on pole litsentsi loendab tellimusega seostatud. Kasutajate ühendus tellimus, kasutajate juurdepääsu haldamine tellimuse ressursse, on saavutada, andes neile õigused Töövool vastendatud tellimuse Azure ressursid.


##<a name="how-does-azure-ad-licensing-work"></a>Azure AD hulgilitsentsimise tööpõhimõtted

Litsentsi vastavalt (õiguse-põhine) Azure AD teenuste töö aktiveerides oma Azure AD/kataloogiteenusest rentniku tellimus. Kui tellimus on aktiivne teenuse võimaluste saab hallata/kataloogiteenusest administraatorid ja kasutada litsentsitud kasutajad.

Kui ostate või ettevõtte mobiilsus, Azure AD Premium või Azure AD lihtsa aktiveerimine, värskendatakse kataloogi tellimus, sh selle kehtivusperioodi ja ettemakstud litsentsid. Tellimuse teabe, sh olek, elutsükli järgmise sündmuse ja määratud ja saadaval litsentside arvu on saadaval vahekaardil litsentside jaoks teatud kataloogi Azure klassikaline portaali kaudu. See on ka parim koht hallata oma litsentsimääramised.

Iga tellimuse koosneb ühe või mitme teenuse lepingute iga vastendamise kaasatud otstarbekas tase teenuse tüüp; näiteks Azure AD Azure'i MFA, Microsoft Intune'i, Exchange Online'i või SharePoint Online. Azure'i AD litsentsihalduse ei nõua Teenusehaldus leping taseme. See erineb Office 365, mille aluseks on selles režiimis täpsemalt konfigureerimine kaasatud teenuste juurdepääsu haldamiseks. Teenuse konfiguratsiooni funktsioonide lubamine ja üksikute õiguste haldamine tugineb Azure AD.

Üldiselt Azure AD tellimuse teave haldab litsentside menüü jaoks teatud kataloogi Azure klassikaline portaali kaudu. Azure'i AD tellimused, välja arvatud Azure AD Premium, ei kuvata Office'i portaalis.

> [AZURE.IMPORTANT] Azure'i AD Premium ja Basic, samuti ettevõtte mobiilsus tellimused, piirduvad nende ettevalmistatud directory/rentniku. Tellimuste ei saa kataloogide vahel või kasutada muude kataloogide kasutajate anna. Liikumine kataloogide tellimust on võimalik, kuid nõuab tugi Piletite või tühistamine ja uuesti osta otse ostude puhul.

> Azure AD ostmisel või ettevõtte mobiilsus läbi hulgilitsentsimise tellimuse aktiveerimine toimub automaatselt, kui leping hõlmab Microsoft Online services, nt Office 365.

Makstud Azure AD funktsioonid üle kataloogi laius. Näited:
- Rühma-põhine ülesande rakendusi, et haldate taotluse lubatud.
- Täpsemad ja rühma iseteenindusliku halduse funktsioonid on saadaval jaotises kataloogi konfiguratsiooni või teatud rühmast.
- Vahekaardil aruandlus on Premium Turvalisus aruanded
- Pilveteenuse rakenduse tuvastamine kuvatakse jaotises identiteedi Azure'i portaalis.

###<a name="assigning-licenses"></a>Litsentside määramine
Ajal tellimus on kõik, mida on vaja konfigureerida makstud võimalusi, oma Azure AD makstud funktsioonide kasutamine eeldab, levitamise õige isikutele litsentse. Üldiselt igale kasutajale, kes peaks olema juurdepääs või kellel on hallatakse Azure AD makstud funktsioon peab olema määratud litsentsi. Litsentsi määramine on kaardistamine kasutaja ja ostetud teenus, nt Azure AD Premium, Basic või ettevõtte mobiilsus vahel.

On lihtne hallata, millistel kasutajatel kataloogis peaks olema litsentsi. See on võimalik saavutada määramine rühmale Azure AD haldamine portaalis ülesande reeglite loomiseks või litsentside määramise otse portaalis, PowerShelli või API-de kaudu õige isikutele. Kui litsentside määramine rühmale, määratakse kõigi rühma liikmete litsentsi. Kui kasutaja on lisatud või rühmast eemaldatud määratakse või vastav litsents eemaldada. Rühmakuuluvus saab kasutada mis tahes rühma haldamine saadaolevate ja vastab rakenduste ülesande rühma-põhine. Kasuta seda moodust, saate reeglid häälestada nii, et kõik kasutajad kataloogis määratakse automaatselt, veenduge, et igaüks, kellel vastav ametinimetus on litsentsi või isegi delegaadi otsus organisatsiooni teiste haldurid.

Rühma vastavalt litsentsi määramine, kus iga kasutaja puuduvad kasutuse kohta pärivad kausta asukoht ülesande ajal. Administraator saab igal ajal muuta selle asukoha. Juhul, kui automatiseeritud ülesande nurjus tõrke tõttu, kajastuvad kasutajateabe jaotises selle litsentsi tüübi liikmesriigis.

##<a name="getting-started-with-azure-ad-licensing"></a>Alustamine Azure AD litsentsimise

Alustamine: Azure'i AD on lihtne; Saate luua alati kataloogi on tasuta prooviversiooni Azure registreerumine osana. [Lisateavet ettevõtte sisselogimine kohta](sign-up-organization.md). Järgmistest aitab teil tagada, et kataloogi kõige paremini ja muude Microsofti teenuste teil võib tarbimine või plaanite kasutada saamiseks teenuse eesmärgid.

Siin on mõned soovitused.
- Kui kasutate juba Microsofti ettevõtte teenuste, on teil juba on Azure AD kataloogi. Sel juhul tuleks jätkata samas kaustas muude teenustega kasutada nii, et core identiteetide haldus, sh ettevalmistamise ja hübriid SSO-d, saab kasutada üle teenuste. Kasutajad on ühekordse sisselogimise kogemus ja võidavad rikkalikumat võimaluste üle teenuseid. Kui otsustate osta Azure AD makstud teenuse oma töötajate, soovitame selle tulemusena selleks kasutada sama kataloogi.
- Kui kavatsete kasutada Azure AD teistsuguseid kasutajate (partnerid, kliendid ja jne) või kui soovite hinnata Azure AD teenuste ja soovite seda teha tootmise teenust eraldi või kui otsite häälestamine Liivakasti keskkonna teenused, soovitame teil esmalt looge uus kaust Azure'i Azure klassikaline portaali kaudu. [Lisateave uue loomine Azure AD directory Azure klassikaline portaalis](active-directory-licensing-directory-independence.md). Väliskasutajana üldadministraatori õigustega luuakse uus kataloogi kontoga. Kui logite sisse Azure klassikaline portaali selle kontoga, on võimalik vaadata selle kausta ja juurdepääs kõigi directory haldustoimingute. Soovitame kohaliku konto loomine asjakohaste õigustega haldamiseks muude Microsofti teenuste (need pole kättesaadav Azure klassikaline portaali kaudu). [Lisateavet leiate teemast Azure AD kasutajakontode loomise kohta](active-directory-create-users.md).

> [AZURE.NOTE] Azure AD toetab "väliskasutajad," on kasutajakontod Azure AD, mis on loodud, kasutades Microsofti konto (MSA) või mõne muu kataloogist identiteedi Azure AD eksemplariga. Kuigi meil on hõivatud ulatub seda võimalust sisse kõigi asutuse Microsofti teenuste kohe need kontod ei toeta mõned soovitud teenuste kogemusi; näiteks Office 365 portaalis haldus ei toeta praegu need kasutajad. Seetõttu ei saa välised kasutajad Microsofti kontoga juurdepääs Office 365 portaalis haldus, samal ajal väliste kasutajate muu Azure AD kataloogid ignoreeritakse. Viimasel juhul, ainult kohaliku kasutajakonto, Azure AD või Office 365 kataloogi, kui kasutaja on algselt loodud, oleks need kogemused kaudu.

Nagu on märgitud, on Azure AD makstud erinevaid versioone. Neid versioone on nende ostu-saadavus väikseid erinevusi.


| Toote   | EA/VL     | Avatud  |   CSP |   MPN kasutamise õigused  |   Otse | Prooviversioon |
|---|---|---|---|---|---|---|
| Ettevõtte mobiilsus |   X | X | X | X |  |      X |
| Azure'i AD Premium  | X | X | X |   | X | X |
| Azure'i AD Basic    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>Valige üks või mitu litsentsi katsete arv
 Alati, saate aktiveerida, valides kataloogis menüü litsentsi soovite teatud prooviversioonile Azure AD Premium või ettevõtte mobiilsus prooviversiooni tellimuse. Kas prooviversiooni sisaldab 30 päeva tellimust väljalülitatud 100 litsentse.

![Azure Active Directory prooviversiooni litsentsi lepingutes](./media/active-directory-licensing-what-is/trial_plans.png)

![Ettevõtte mobiilsus prooviversiooni litsentsi lepingud](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktiivse prooviversiooni litsentsi lepingud](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>Litsentside määramine
Kui tellimus on aktiivne, tuleks litsentsi endale määramiseks ja värskendage brauserit te näete kõigi funktsioonide tagamiseks. Järgmiseks tuleb määrata litsentse, mis tuleb avada või lisada, mille eest tasutakse Azure AD funktsioone. Nagu me eespool "Litsentside määramine", on parim viis seda teha esindavad soovitud sihtrühm rühma tuvastamine ja määramine selle litsentsi; Sel viisil on määratud kasutajatele, kellel on lisada või eemaldada rühma üle elutsükli või litsentsi eemaldada.

Litsentsi määramine rühmale või üksikud kasutajad, valige litsentsi leping, mida soovite määrata ja klõpsake käsku **Määra** .

![Aktiivse prooviversiooni litsentsi lepingud](./media/active-directory-licensing-what-is/assign_licenses.png)

Üks kord dialoogis ülesande jaoks valitud režiimi saate valida kasutajad ja **määrata** veeru paremal lisamine. Saate Lehitsemiseks saate kasutajate loendit või otsimine abil on kas teatud isikutele klaas ülemises paremas nurgas kasutaja ruudustiku. Määrata rühmad, valige käsk **Kuva** "Rühmad" ja klõpsake paremal värskendamine ülesandeid, mis on kuvatud nupp sisse.

![Litsentside määramine rühmadesse](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Nüüd saate otsida või Lehitsemiseks rühmad ja lisada need **määrata** veeru samal viisil. Saate need määrata kasutajatele ja rühmadele ühekordne kombinatsiooni. Ülesande lõpuleviimiseks lehe alumises paremas nurgas nuppu kontrolli.

![Litsentsi ülesande edenemine sõnum](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Kui rühm on määratud, selle liikmeid pärivad litsentside 30 minuti jooksul, kuid tavaliselt 1-2 minuti jooksul.

Ülesande tõrked võivad tekkida ajal Azure AD litsentsi määramine, kuid suhteliselt harva. Ülesande võimalike tõrgete on piiratud.
- Ülesande konflikti - kui kasutaja on varem määratud litsents, mis ei ühildu praegune litsents. Sel juhul uue litsentsi määramise vajavad eemaldamine eelmine.
- Ületatud saadaolevate litsentside – kui määratud rühmade kasutajate arv ületab saadaolevad litsentsid, kasutajate ülesande oleku kajastuvad tõrke tõttu puuduvad litsentse määrata.

###<a name="view-assigned-licenses"></a>Litsentside määratud vaatamine

Koondvaate määratud litsentside, sh saadaval, määratud ja järgmise tellimuse elutsükkel sündmuse kuvatakse menüü **litsentsid** .

![Määratud litsentside arvu vaatamine](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Üksikasjalik loend määratud kasutajatele ja rühmadele, sh ülesande oleku ja tee (otsese või ühe või mitme rühmade päritud) on saadaval, kui litsentsi kava liikumine.

![Lepingu litsentsi määratud litsentside Kuva üksikasjad](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Litsentside eemaldamine on sama lihtne kui nende määramine. Kui kasutaja on otse määratud või määratud rühma, saate litsentsi tüübi valimine, valides **eemaldamine**, kasutaja või rühma lisamine eemaldamine loendist ja toiming, mis kinnitab litsentsi eemaldada. Teise võimalusena saate avada litsentsi tüübi, valige teatud kasutaja või rühma ja puudutage käsku **Eemalda** . Kasutaja pärimise rühma litsentsi lõpetamiseks lihtsalt eemaldamiseks rühmast kasutaja.

###<a name="extending-trials"></a>Ulatub katsete arv

Prooviversiooni laiendid klientidele, kes on saadaval nii iseteenindusliku Office 365 portaali kaudu. Kliendi administraator, saate liikuda [Office portaal](https://portal.office.com/#Billing) (Accessi sõltub õiguste Office portaalis) ja valige Azure AD Premium prooviversiooni. Klõpsake linki **pikenda prooviversiooni** ja järgige juhiseid. Peate sisestama oma krediitkaardi andmed, kuid see ei lisandu.

![Valige Office portaalis prooviversiooni litsentsi laiendamine](./media/active-directory-licensing-what-is/extend_license_trial.png)

Klientide saate ka taotleda prooviversiooni pikendamise esitades esitada tugiteenuse taotluse. Kliendi administraator, saate liikuda Office 365 videoportaali [leht](http://aka.ms/extendAADtrial) (Accessi sõltub õiguste lehel Office'i tugiteenuste). Sellel lehel Valige "Tellimuste ja katsete_arv" jaotises funktsioonid ja sümptom jaotises "uuringu küsimused". Sisestage teave lõpuks olukordade kohta

![Prooviversiooni litsentsi abil esitada tugiteenuse taotluse laiendamine](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Järgmised sammud

Nüüd võib olla valmis konfigureerimine ja kasutamine Azure AD Premium funktsioone.

- [Iseteeninduslik parooli lähtestamine](active-directory-manage-passwords.md)
- [Iseteeninduslik rühma haldamine](active-directory-accessmanagement-self-service-group-management.md)
- [Azure'i AD-ühendus heath](active-directory-aadconnect-health.md)
- [Rühmakuuluvus rakendustele](active-directory-manage-groups.md)
- [Azure'i Mitmikautentimise](../multi-factor-authentication/multi-factor-authentication.md)
- [Otsest Azure AD Premium litsentside ostmine](http://aka.ms/buyaadp)
