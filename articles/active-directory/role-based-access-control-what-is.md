<properties
    pageTitle="Rollipõhine juurdepääsu reguleerimine | Microsoft Azure'i"
    description="Alustamine juurdepääsu haldamine portaalis Azure'i Azure Rollipõhine juurdepääsu reguleerimine. Rollimäärangud abil kataloogis õiguste määramine."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Alustamine Azure'i portaalis juurdepääsu juhtimine

Turvalisus ettevõtted keskenduvad töötajate andes neile vajalikule täpse õigused. Liiga palju õiguste seab ründajate konto. Liiga vähe õiguste tähendab, et töötajad ei saa oma tööd tõhusalt. Azure'i Rollipõhine juurdepääsu juhtimine (RBAC) aitab kohandatud juurdepääsu juhtimine Azure pakkudes selle probleemi lahendamiseks.

RBAC abil saate eraldada ülesandeid oma meeskonna ja anda juurdepääsu üksnes summa kasutajatele vajadusest teha oma tööd. Selle asemel, kõik piiramatu õiguste Azure tellimuse või ressursse, saate lubada ainult teatud toiminguid. Näiteks lasta ühe töötaja haldamine virtuaalmasinates on tellimus, samas teise saate hallata SQL-i andmebaasid ühe tellimuse RBAC abil.

## <a name="basics-of-access-management-in-azure"></a>Põhiteave Azure juurdepääsu juhtimine
Iga Azure'i tellimus on seotud ühe Azure Active Directory (AD) kataloogi. Kasutajad, rühmad ja selle kataloogi rakenduste haldamiseks ressursid Azure'i tellimus. Saate määrata järgmised juurdepääsuõigused Azure portaali, Azure käsurea tööriistad ja Azure halduse API-d.

Anda juurdepääsu, määrates RBAC sobiv roll kasutajate, rühmade ja teatud ulatusega rakendused. Rollimäärang ulatus võib olla tellimuse, ressursirühma või ressurss. Rollile määratud ema ulatus ka annab juurdepääsu sisalduvate lapsed. Näiteks kasutaja ressursirühma juurdepääsu saate hallata kõik ressursse, mis sisaldab, nagu veebilehed virtuaalmasinates ja alamvõrku.

![Seose Azure Active Directory elementide - diagramm](./media/role-based-access-control-what-is/rbac_aad.png)

RBAC rolli, mida saate määrata näitab, millised ressursid kasutaja, rühma või rakenduse kaudu saate hallata selle ulatus.

## <a name="built-in-roles"></a>Sisseehitatud rollid
Azure'i RBAC on kolme lihtsa rollid, mis rakenduvad kogu ressursi tüübid.

- **Omanik** on täielik juurdepääs kõigile ressurssidele, sh delegaadi juurdepääsu teistele paremale.
- **Kaasautor** , saate luua ja hallata igat tüüpi Azure ressursse, kuid ei saa anda juurdepääsu teistele.
- **Lugeja** saate vaadata olemasolevaid Azure ressursse.

Ülejäänud RBAC rollid Azure luba teatud Azure vahendite haldamine. Näiteks võimaldab virtuaalse masina kaasautori roll kasutaja loomiseks ja haldamiseks virtuaalmasinates. See ei anna nende Accessi virtuaalse võrgu või alamvõrku, mis ühendab virtuaalse masina.

[Sisseehitatud RBAC rollid](role-based-access-built-in-roles.md) on loetletud saadaval Azure rollid. Saate määrata toimingute ja iga sisseehitatud rolli määrab kasutajatele ulatus. Kui otsite määratleda rolle rohkem võimalusi, vaadake, kuidas luua [kohandatud Azure'i RBAC rollid](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Ressursi hierarhia ja juurdepääs pärimine
- Iga **tellimuse** Azure kuulub ainult ühe kausta.
- Iga **Ressursirühma** kuulub ainult üks tellimus.
- Iga **Ressursi** kuulub ainult ühte ressursirühma.

Access, mida saate määrata ema otsinguulatuste on päritud lapse otsinguulatuste juures. Näiteks:

- Lugeja rollile määrata Azure AD rühma tellimuse ulatuses. Selle rühma liikmed saate vaadata iga ressursirühma ja ressursside tellimus.
- Taotluse ressursi rühma ulatuse määramiseks kaasautori roll. See ressursid igat tüüpi selle ressursi rühma, kuid mitte muid ressursi rühmad tellimuse haldamiseks.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure'i RBAC vs klassikaline tellimuse administraatorid
Klassikaline tellimuse administraatorid ja co-administraatorid on täielik juurdepääs Azure'i tellimus. Need saate hallata Azure ressursihaldur API-d või [Azure klassikaline portaali](https://manage.windowsazure.com) ja Azure klassikaline juurutamise mudeli [Azure portaali](https://portal.azure.com) kasutamine ressursid. Klassikaline administraatorid määratakse mudelis RBAC ulatuse tellimuse omanik rolli.

Azure'i RBAC toetavad ainult Azure portaali ja uue Azure ressursihaldur API-d. Kasutajate ja rakendused, mis on määratud RBAC rollid ei saa kasutada klassikaline haldusportaal ja Azure klassikaline juurutamise mudel.

## <a name="authorization-for-management-vs-data-operations"></a>Luba halduse vs andmete toimingute jaoks
Azure'i RBAC toetab ainult toimingute Azure ressursside Azure portaali ja Azure ressursihaldur API-d. Ta ei saa lubada kõigi andmete taseme toimingute Azure ressursse. Näiteks võite lubada keegi Halda salvestusruumi kontod, kuid plekid või salvestusruumi konto tabeleid ei saa. Lisaks SQL-andmebaasi saab hallata, kuid mitte tabelite kogumahtu.

## <a name="next-steps"></a>Järgmised sammud
- Alustamine [Rollipõhine juurdepääsu reguleerimine Azure'i portaalis](role-based-access-control-configure.md).
- Vt [RBAC sisseehitatud rollid](role-based-access-built-in-roles.md)
- [Kohandatud rollid Azure'i RBAC](role-based-access-control-custom-roles.md) määratlemine
