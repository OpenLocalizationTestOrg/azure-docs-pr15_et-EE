<properties
    pageTitle="Ressursside Azure'i virnas (teenuse administraator ja rentniku) kasutaja kohta õiguste haldamiseks | Microsoft Azure'i"
    description="Kui administraator või rentniku, saate teada, kuidas ressursid iga kasutaja õiguste haldamiseks."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Kasutajaõiguste haldamine

Azure'i virnas kasutaja saab lugeja, omanik või kaasautor iga eksemplari tellimus, ressursirühm või teenuse. Näiteks võib kasutaja A on lugemise õigused tellimuse 1, kuid on virtuaalse masina 7 omaniku õigused.

-   Lugeja: Kasutaja saab vaadata kõike, kuid mitte muuta.

-   Kaasautor: Kasutaja saab hallata kõik, välja arvatud ressurssidele.

-   Omanik: Kasutaja saab hallata kõik, sh ressurssidele.


## <a name="set-access-permissions-for-a-user"></a>Accessi kasutaja jaoks õiguste määramine

1.  Logige sisse kontoga, millel on ressurss, mida soovite hallata omaniku õigused.

2.  Ressursi tera, klõpsake ikooni **Accessi** ![](media/azure-stack-manage-permissions/image1.png).

3.  Klõpsake labale **kasutajate** **rollid**.

4.  **Rollide** tera, nuppu **Lisa** kasutaja õiguste lisamiseks.

## <a name="next-steps"></a>Järgmised sammud

[Azure'i virnas rentnikku lisamine](azure-stack-add-new-user-aad.md)
