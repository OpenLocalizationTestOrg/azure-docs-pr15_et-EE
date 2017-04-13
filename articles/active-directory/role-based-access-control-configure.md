<properties
    pageTitle="Kasutage Rollipõhine juurdepääsu reguleerimine Azure'i portaalis | Microsoft Azure'i"
    description="Alustamine juurdepääsu haldamine portaalis Azure Rollipõhine juurdepääsu reguleerimine. Kasutage rollimääranguid õiguste määramiseks teie ressursse."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Rollimäärangud abil saate hallata oma Azure tellimuse ressurssidele

> [AZURE.SELECTOR]
- [Kasutaja või rühma juurdepääsu haldamine](role-based-access-control-manage-assignments.md)
- [Ressurss juurdepääsu haldamine](role-based-access-control-configure.md)

Azure'i Rollipõhine juurdepääsu juhtimine (RBAC) võimaldab kohandatud juurdepääsu juhtimine Azure. RBAC kasutamisel saate anda ainult juurdepääsu summa, et kasutajatel on vaja teha oma tööd. See artikkel aitab teil Valmistuge tööks RBAC Azure'i portaalis. Kui soovite rohkem üksikasju, kuidas RBAC aitab hallata juurdepääsu, vaadake, [mis on Rollipõhine juurdepääsu reguleerimine](role-based-access-control-what-is.md).

## <a name="view-access"></a>Accessi vaade
Saate vaadata, kellel on juurdepääs ressurss, ressursirühm või tellimuse kaudu oma peamist blade [Azure portaali](https://portal.azure.com). Näiteks soovite vaadata, kellel on juurdepääs ühte meie ressurss:

1. Valige vasakpoolsel navigeerimisribal **Ressursi rühmad** .  
    ![Ressursi rühmad - ikoon](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Valige keelest **Ressursirühma** ressursirühma nimi.
3. Valige vasakpoolsest menüüst **juurdepääsu reguleerimine (IAM)** .  
4. Juurdepääsu juhtimine tera on loetletud kõik kasutajad, rühmad ja rakendused, mis on antud juurdepääs ressursirühma.  

    ![Kasutajate blade - päritud vs määratud Accessi kuvatõmmis](./media/role-based-access-control-configure/view-access.png)

Teate, et mõned kasutajad on **määratud** juurde pääsevad teised **Inherited** . Juurdepääs on ressursirühma omistatud või päritud ülesandele ema tellimusega.

> [AZURE.NOTE] Klassikaline tellimuse administraatorid ja co-administraatorid käsitletakse tellimuse uue RBAC mudeli omanikud.


## <a name="add-access"></a>Juurdepääsu lisamine
Kui annate juurdepääsu ressursi, ressursirühma või tellimus, mis on ulatuse rolli määramine.

1. Valige **Lisa** juurdepääsu juhtimine enne.  
2. Valige roll, mille soovite määrata keelest **Valige roll** .
3. Valige kasutaja, rühma või rakenduse kataloogis, mille soovite anda juurdepääsu. Saate otsida kuvatavad nimed, meiliaadressid ja objekti identifikaatorite kataloogi.  

    ![Lisa kasutajad blade - otsida kuvatõmmis](./media/role-based-access-control-configure/grant-access2.png)

4. Klõpsake nuppu **OK** ülesande loomiseks. **Kasutaja lisamine** hüpikaknas jälitab edenemist.  
    ![Lisades kasutaja edenemise riba - kuvatõmmis](./media/role-based-access-control-configure/addinguser_popup.png)

Pärast Rollimäärang edukalt lisamist kuvatakse see enne **Kasutajad** .

## <a name="remove-access"></a>Juurdepääsu eemaldamine

1. Valige rolli määramine Accessi kontrolli enne.
2. Valige **Eemalda** tera ülesande üksikasjad.  
3. Valige **Jah** eemaldamise kinnitamiseks.  
    ![Kasutajate blade - Eemalda rolli kuvatõmmis](./media/role-based-access-control-configure/remove-access1.png)

Päritud ülesanded ei saa eemaldada. Pange tähele alloleval pildil nupp Eemalda on hall. Selle asemel pilk **Veebisaidil määratud** üksikasjad. Minge ressursi seal loetletud eemaldamiseks rolli määramine.

![Kasutajate blade - päritud access keelab eemaldada nupu kuvatõmmis](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Muud tööriistad juurdepääsu haldamiseks
Saate määrata rollid ja Azure RBAC käske Tööriistad peale Azure portaali juurdepääsu haldamine.  Lisateavet eeltingimused ja Azure RBAC käske alustamise linkide kaudu.

- [Azure'i PowerShelli](role-based-access-control-manage-access-powershell.md)
- [Azure'i käsurea liides](role-based-access-control-manage-access-azure-cli.md)
- [REST API-GA](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Järgmised sammud
- [Muutuste ajalugu Accessi aruande loomine](role-based-access-control-access-change-history-report.md)
- Vt [RBAC sisseehitatud rollid](role-based-access-built-in-roles.md)
- [Kohandatud rollid Azure'i RBAC](role-based-access-control-custom-roles.md) määratlemine
