<properties
    pageTitle="Azure'i Accessi ülesandeid vaadata | Microsoft Azure'i"
    description="Saate vaadata ja hallata Rollipõhine juurdepääsu reguleerimine ülesanded mis tahes kasutaja või rühma Azure'i portaalis"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>Vaate Accessi ülesannete kasutajate ja rühmade Azure'i portaalis – avaliku eelvaade

> [AZURE.SELECTOR]
- [Kasutaja või rühma juurdepääsu haldamine](role-based-access-control-manage-assignments.md)
- [Ressurss juurdepääsu haldamine](role-based-access-control-configure.md)

Koos Rollipõhine juurdepääsu juhtimine (RBAC) Azure Active Directory eelvaade, saate hallata oma Azure ressursid access. [Mis on eelvaates?](active-directory-preview-explainer.md)

Juurdepääs määratud RBAC on kohandatud, sest on kaks võimalust, saate piirata õigusi:

- **Ulatus:** RBAC rollimääranguid on rakendatud teatud tellimus, ressursirühm või ressurss. Juurdepääs ressurss kasutaja ei pääse muud ressursid, samas tellimus.
- **Rolli:** Ülesande piires, on juurdepääs vähenenud veelgi, rolli määramine. Rollide võib olla üksikasjalik, nt omanik või teatud, nt virtuaalse masina lugeja.

Rollide saab määrata ainult kaudu tellimus, ressursirühm või ressurss, mille ulatus on ülesande jaoks. Kuid kõik Accessi määratud antud kasutaja või rühma saate vaadata ühes kohas.

Lisateave selle kohta, kuidas [kasutada rollimääranguid juurdepääsu oma ressursse Azure tellimuse haldamiseks](role-based-access-control-configure.md).

##  <a name="view-access-assignments"></a>Vaate Accessi ülesanded

Accessi ülesannete ühe kasutaja või rühma otsimiseks käivitada Azure Active Directory [Azure portaali](http://portal.azure.com).

1. Valige **Azure Active Directory**. Kui see suvand pole nähtav loendisse navigeerimine, valige **Rohkem teenuseid** ja seejärel kerige allapoole **Azure Active Directory**.
2. Valige **kasutajad ja rühmad**ja seejärel kas **kõigi kasutajate** või **rühmade kõik**. Selle näite puhul me keskenduda üksikud kasutajad.
    ![Kasutajate ja rühmade Azure Active Directory - kuvatõmmis haldamine](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Otsige soovitud kasutaja nimi või kasutajanimi.
4. Valige kasutaja enne **Azure ressursse** . Accessi ülesannete kasutaja jaoks kuvada.

### <a name="read-permissions-to-view-assignments"></a>Lugemisõigused ülesannete vaatamine

Sellel lehel kuvatakse ainult Accessi ülesanded, mida teil on õigus lugeda. Näiteks on lugemisõigus tellimuse A ja minge Azure'i ressursid tera kontrollida kasutaja ülesanded. Te saate vaadata oma Accessi ülesannete tellimuse A, kuid ei kuvata, et ta on ka Accessi ülesannete tellimuse B.

## <a name="delete-access-assignments"></a>Accessi ülesannete kustutamine

Selle tera kaudu saate kustutada kasutaja või rühma määratud Accessi ülesanded. Kui access ülesande päris ema rühma, peate avage ressursi või tellimuse ja hallata ülesande seal.

1. Accessi ülesannete kasutaja või rühma loendist, valige see, mida soovite kustutada.
2. Valige **Eemalda** ja seejärel **Jah** kinnitamiseks.
    ![Accessi ülesande - kuvatõmmis eemaldamine](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Seotud teemad

- Alustamine [juurdepääsu oma ressursse Azure tellimuse haldamiseks kasutada rollimääranguid](role-based-access-control-configure.md) Rollipõhine juurdepääsu reguleerimine
- Vt [RBAC sisseehitatud rollid](role-based-access-built-in-roles.md)
