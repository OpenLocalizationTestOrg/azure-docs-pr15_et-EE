<properties
    pageTitle="Ressursside juurdepääsu haldamine Azure Active Directory levirühmadega | Microsoft Azure'i"
    description="Kuidas kasutada rühmade Azure Active Directory kohapealse ja pilveteenuse rakenduste ja ressursside kasutajate juurdepääsu haldamine."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Azure Active Directory levirühmadega ressursid juurdepääsu haldamine

Azure Active Directory (Azure AD) on täielik identiteedi ja juurdepääsu juhtimine lahenduse, mis annab tugeva võimalused juurdepääsu kohapealse ja pilveteenuse rakenduste ja ressursse, sh teenuse Microsoft online services nagu Office 365 ja maailma Microsoft SaaS rakenduste haldamiseks. Selles artiklis antakse ülevaade, kuid kui soovite hakata kasutama Azure AD rühmade kohe, järgige [Azure AD haldamine turberühmad](active-directory-accessmanagement-manage-groups.md). Kui soovite teada, kuidas kasutada PowerShelli Azure Active Directory rühmade haldamiseks saate lugeda rohkem teemast [Azure Active Directory rühma haldamine eelvaade cmdlet-käsud](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Azure Active Directory kasutamiseks on vaja Azure konto. Kui teil pole kontot, saate [registreeruda tasuta Azure'i konto jaoks](https://azure.microsoft.com/pricing/free-trial/).


Azure AD, üks olulisemad funktsioonid on võimalus haldamine ressurssidele. Järgmiste ressursside saab osa kataloogi, nagu õiguste haldamiseks objektide kataloogi või ressursse, mis on välise kataloogi, nt SaaS rakendused, Azure'i teenused ja SharePointi saitide või eeldusel ressursid rollide kaudu. On neli võimalust kasutaja saab määrata ressursile juurdepääsu õigusi.


1. Otsene omistamine

    Kasutaja saab määrata otse ressursi omanik selle ressursi.

2. Rühmakuuluvus

    Rühma saab määrata ressursi ressursi omanik ja seda tehes, andmise ressursile juurdepääsu selle rühma liikmed. Seejärel saate selle rühma liikmestaatusi näha haldama rühma omanik. Tõhus, ressursside omanik delegaatide kasutajate määramine nende ressursside rühma omaniku luba.

3. Reegli

    Ressursi omanik saab reegli abil kiire, millistel kasutajatel määratakse Accessi ressursi. See reegel ja nende väärtuste teatud kasutajate jaoks kasutatakse atribuutide sõltub reegli ja nii ressursi omanik delegaatide tõhus haldamine atribuute, mida kasutatakse reeglit nende ressursside autoriteetsete allikas juurdepääsu õigus. Ressursi omanik endiselt haldab reegli ise ja määratleb, millised atribuudid ja väärtused anda juurdepääsu oma ressursi.

4. Välise asutus

    Juurdepääsu ressursile on tuletatud välise andmeallikaga; näiteks rühm, mis on sünkroonitud mõne autoriteetsete allikast, näiteks mõne kohapealse kataloogi või SaaS rakenduse, nt WorkDay. Ressursi omanik määrab rühma ressursile juurdepääsu ja välise andmeallika haldab rühma liikmeid.

  ![Accessi halduse skeemi ülevaade](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>Vaadake videot, mis selgitab juurdepääsu juhtimine

Saate vaadata lühikest videot, mis selgitab rohkem selle kohta:

**Azure AD: Dünaamiliste rühmade liikmelisuse Sissejuhatus**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Kuidas pääseda Azure Active Directory töö haldus?
Azure AD keskmes Accessi lahendus on turberühma. Turberühma abil saate hallata juurdepääsu ressursid on tuntud paradigma, mis võimaldab paindlik ja arusaadav võimalus anda juurdepääsu ressursile ettenähtud kasutajate rühma jaoks. Ressursi omanik (või administraator kataloogi) saate määrata teatud juurdepääsu õigus nad oma ressursside rühma. Rühma liikmete antakse juurdepääs ja ressursside omanik saab volitatud esindaja õigus haldamine kellelegi teisele, näiteks osakond juhataja või kasutajatoe administraator rühma liikmete loend.

![Azure Active Directory access halduse skeem](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Rühma omanik saab kättesaadavaks ka selle rühma jaoks iseteenindusliku taotlused. Tehes lõppkasutaja saate otsida ja otsida üles rühm ning teha taotlus liitumiseks tõhus otsib juurdepääs hallatud ressursside kaudu rühma. Rühma omanik, saate seadistada jaotises nii, et Liitu taotlused on kinnitatud automaatselt või rühma omanik kinnitamise nõudmine. Kui kasutaja taotleb rühmaga, edastatakse liitumise taotlus rühma omanikud. Kui üks omanikke kinnitab taotluse, teatatakse taotlevale kasutajale ja kasutajale on liitunud rühma. Kui üks omanikke saab taotluse, taotlemise kasutaja on teavitada, kuid pole ühendatud rühma.


## <a name="getting-started-with-access-management"></a>Alustamine: juurdepääsu juhtimine
Kas olete valmis alustama? Peaksite proovima välja mõned põhitoimingud, saate teha Azure AD rühmad. Kasutage neid võimalusi pakkuda eriotstarbelisi juurdepääsu rühmadele eri ressursid ettevõttes. Loendi esimese põhijuhised on loetletud allpool.

* [Reegli loomiseks lihtne, dünaamiline liikmelisused rühma konfigureerida](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Rühma haldamine SaaS rakendustele juurdepääsemiseks abil](active-directory-accessmanagement-group-saasapps.md)

* [Rühma kättesaadavaks tegemine lõppkasutaja iseteenindusliku](active-directory-accessmanagement-self-service-group-management.md)

* [Sünkroonimise ka kohapealse rühma Azure Azure'i AD-ühenduse kaudu](active-directory-aadconnect.md)

* [Omanike rühma haldamine](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Järgmised sammud juurdepääsu juhtimine
Nüüd, kui on aru saanud juurdepääsu juhtimine põhitõdesid, siin on mõned täiendavad täiustatud võimalused saadaval Azure Active Directory haldamine Accessi rakenduste ja ressursse.

* [Atribuutide abil keerukamate reeglite loomiseks](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Turberühmad Azure AD haldamine](active-directory-accessmanagement-manage-groups.md)

* [Azure AD sihtotstarbeline rühmade seadistamine](active-directory-accessmanagement-dedicated-groups.md)

* [Graafik API viide rühmade jaoks](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Azure Active Directory cmdlet-käsud rühma sätete konfigureerimine](active-directory-accessmanagement-groups-settings-cmdlets.md)
