<properties
    pageTitle="Rühmade Azure Active Directory haldamine | Microsoft Azure'i"
    description="Kuidas luua ja hallata rühmade abil Azure Active Directory Azure'i kasutajate haldamine."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Azure Active Directory rühmade haldamine

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-groups-create-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-accessmanagement-manage-groups.md)
- [PowerShelli](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Üks Azure Active Directory (Azure AD) kasutaja halduse funktsioonid on võimalus luua kasutajate rühmad. Rühma saate teha haldustoiminguid, nt määramine kasutajate arv korraga litsentse või õigused. Samuti saate rühmadele määrata juurdepääsu õigus

- Näiteks objektide kataloogis ressursid
- Ressursside kataloogi nagu SaaS rakendused, Azure'i teenused, SharePointi saitide välise või kohapealse ressursid

Peale selle ressursi omanik saate määrata ka juurdepääsu ressursile Azure AD rühma, mis kuulub kellelegi teisele. Selle ülesande annab ressursile juurdepääsu selle rühma liikmed. Klõpsake rühma omanik haldab rühma liikmed. Tõhus, ressursi omanik delegaatide rühma omaniku luba kasutajate nende ressursside määramine.

## <a name="how-do-i-create-a-group"></a>Kuidas luua rühma?

Sõltuvalt teenuseid, millele teie asutus on liitunud, saate luua rühma, kasutades ühte järgmistest:
- Azure'i klassikaline portaal
- Office 365 portaali konto
- Windows Intune konto portaal

Kirjeldame tööülesannete täidetavate Azure klassikaline portaalis. Haldamiseks kataloogi Azure'i AD-Azure'i portaalide kasutamise kohta leiate lisateavet teemast [Azure AD kataloogi haldamise](active-directory-administer.md).

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com)valige **Active Directory**ja valige kataloogi ettevõtte nime.

2. Valige vahekaart **rühmad** .

3. Valige **jaotise lisada**.

4. Määrake aknas **Lisa rühma** nimi ja kirjeldus rühma.


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Kuidas lisada või eemaldada üksikkasutajate turberühma?

**Kasutaja lisamiseks rühma**

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com)valige **Active Directory**ja valige kataloogi ettevõtte nime.

2. Valige vahekaart **rühmad** .

3. Avage rühma, kuhu soovite liikmeid lisada. Valitud rühma **liikmete** vahekaardi avamine, kui see pole veel kuvamise.

4. Valige **Lisa liikmeid**.

5. Valige lehel **Lisa liikmeid** nimi kasutaja või rühma, mille soovite lisada selle rühma liige. Veenduge, et see nimi on lisatud **valitud** paani.


**Kasutaja eemaldamiseks rühmast**

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com)valige **Active Directory**ja valige kataloogi ettevõtte nime.

2. Valige vahekaart **rühmad** .

3. Avage rühm, millest soovite eemaldada liikmeid.

4. Valige vahekaart **liikmed** , valige liige, mida soovite eemaldada rühma nimi ja seejärel klõpsake käsku **Eemalda**.

6. Kinnitage kuvatakse vastav viip, et soovite selle liikme eemaldamiseks rühmast.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Kuidas rühma liikmete dünaamiliselt hallata?

Azure AD, saate otsustada, millised kasutajad olema rühma liikmed häälestada väga lihtne lihtne reegel. Lihtne reegel on üks, mis toodab ainult ühe võrdlus. Näiteks kui rühm on määratud SaaS rakenduse, saate häälestada reegli lisamine kasutajad, kellel on ametinimetus, "Müügiesindaja". See reegel seejärel annab selle SaaS rakenduse kõigile kasutajatele, et töö pealkiri kataloogis.

Kui mis tahes kasutaja Muuda atribuute, süsteem analüüsib kõik dünaamilise rühma reeglite kuvamiseks, kui kasutaja atribuudi muutmine oleks käivitamine rühma kataloogis lisab või eemaldab. Kui kasutaja reegli rühmas, neid rühma lisada liige. Kui need pole enam vastavad need on liige rühma reeglit, eemaldatakse need on isikuid selle rühma.

> [AZURE.NOTE] Saate häälestada dünaamiline turberühmad või Office 365 rühmade liikmelisuse reegel. Pesastatud rühmaliikmeid pole praegu toetatud rakenduste ülesande rühma-põhine.
>
> Dünaamiliste liikmelisused rühmade jaoks vaja on Azure AD Premium litsents, mis määratakse
>
> - Administraator, kes haldab reegli rühmale
> - Kõigi rühma liikmete

**Dünaamiliste rühma liikmete lubamiseks**

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com)valige **Active Directory**ja valige kataloogi ettevõtte nime.

2. Valige vahekaart **rühmad** ja avage rühm, mida soovite redigeerida.

3. Valige vahekaart **konfigureerimine** ja seejärel seadke **Lubamine dünaamiline Liikmelisused** **Jah**.

4. Lihtne ühe reegli abil määrata, kuidas dünaamiline liikmelisus selles jaotises funktsioonide rühma häälestamine. Veenduge, et selle **kasutajate lisamine kuhu** suvand on valitud, ja seejärel valige kasutaja atribuut loendist (nt osakonna, ametinimetus jne),

5. Seejärel valige tingimus (mitte võrdub, võrdub, käivitab koos, käivitab koos, ei sisaldab, sisaldab, Match, Match).

6. Määrake atribuudi valitud kasutaja võrdlus väärtuse.

Dünaamilise rühma liikmeks *keerukamate* reeglite (reeglid, mis võib sisaldada mitut võrdlemine) loomise kohta leiate teemast [keerukamate reeglite loomiseks kasutamine atribuute](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Lisateave

Azure Active Directory esitada lisateavet järgmistest artiklitest.

* [Azure Active Directory levirühmadega ressursid juurdepääsu haldamine](active-directory-manage-groups.md)

* [Azure Active Directory cmdlet-käsud rühma sätete konfigureerimine](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)

* [Mis on Azure Active Directory?](active-directory-whatis.md)

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
