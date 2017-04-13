<properties
   pageTitle="Juurutamise toimingute portaali vaadata | Microsoft Azure'i"
   description="Kirjeldab, kuidas kasutada Azure portaali kaudu ressursihaldur juurutamise vigade tuvastamiseks."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Vaate juurutamise toimingute Azure'i portaal

> [AZURE.SELECTOR]
- [Portaal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShelli](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure'i CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-GA](resource-manager-troubleshoot-deployments-rest.md)

Saate vaadata tegevuse juurutamiseks, Azure portaali kaudu. Teil võib olla kõige enam huvi kuvamist toimingud, kui olete saanud tõrke juurutamisel nii, et see artikkel keskendub vaatamine toimingute kohta, mida ei ole. Portaalis on kasutajaliides, mis võimaldab leidmist vigade ja võimalike paranduste kindlaks teha.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Juurutamise toimingute abil saate otsida

Juurutamise tegevuste kuvamiseks tehke järgmist:

1. Teade kaasatud juurutamise ressursirühma viimase juurutamise olekut. Saate valida selle olekuks rohkem üksikasju.

    ![juurutamise olek](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Kuvatakse tehtud juurutamise ajalugu. Valige juurutamise, mida ei saanud.

    ![juurutamise olek](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Valige **nurjus. Üksikasjade kuvamiseks klõpsake siin** juurutamise nurjumise põhjust kirjelduse kuvamiseks. Alloleval pildil pole DNS-i kirje kordumatu.  

    ![nurjunud juurutamise kuvamine](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    See tõrketeade kuvatakse peaks olema nii palju, mida tõrkeotsingu jaoks. Kui teil on vaja rohkem üksikasju kohta, millised tööülesanded on lõpule viidud, toiminguid saate vaadata, nagu on näidatud toimige järgmiselt.

4. Saate vaadata kõiki toiminguid juurutamise **juurutamise** tera. Valige mis tahes toimingu kohta täpsema teabe kuvamiseks.

    ![vaate toimingud](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    Sel juhul näete salvestusruumi konto, virtuaalse võrgu ja kättesaadavus Sea edukalt loodi. Avaliku IP-aadressi nurjus ja muud ressursid olid proovitakse kohale toimetada.

5. Sündmuste kasutuselevõtuks saate vaadata, valides **sündmused**.

    ![Kuva sündmused](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Kõik sündmused kasutuselevõtuks lugege teemat ja valige mõni üksikasjalikumat teavet.

    ![vt sündmused](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Kasutage auditilogid tõrkeotsing

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Tõrked juurutamine vaatamiseks tehke järgmist:

1. Ressursirühma auditilogide vaatamiseks, valides **Audit logid**.

    ![Valige auditeeritavad logid](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. **Auditilogide** tera, kuvatakse kõigi rühmade ressursi tehtud toimingute kokkuvõte teie tellimus. See sisaldab graafiliselt aeg ja toimingute olekut ning toimingute loendit.

    ![Kuva toimingud](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Saate filtreerida oma vaate auditilogide keskenduda teatud tingimused. Valige **Filter** **auditilogid** tera ülaosas.

    ![filtri logid](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Valige keelest **filtri** tingimuste piirata teie vaate auditilogide ainult need toimingud, mida soovite näha. Näiteks saate filtreerida toimingute vigade ressursirühma ainult kuvamiseks.

    ![filtreerimissuvandite seadmine](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Seades ajavahemiku saate filtreerida täiendavaid toiminguid. Järgmisel pildil filtreerib kindla 20-minutilise kuuline ajavahemik vaatesse.

    ![Määra kellaaeg](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. Kõik toimingud, saate valida loendist. Valige toiming, mis sisaldab viga, mida soovite uurimine.

    ![Valige toiming](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Kuvatakse kõik sündmused selle toimingu jaoks. Pange tähele **Korrelatsiooni ID-d** kokkuvõte. Selle ID-d kasutatakse seotud sündmustel silma peal hoidmiseks. See võib olla kasulik tehnilise toe poole ja tõrkeotsingu töötamisel. Sündmuse üksikasjade kuvamiseks sündmuse saate valida.

    ![Valige sündmus](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Kuvatakse sündmuse üksikasjad. Eelkõige tähelepanu **atribuutide** teavet tõrke kohta.

    ![Auditilogi log üksikasjade kuvamine](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Auditilogi rakendatud filter alles järgmine kord, kui vaatate seda nii, et peate oma vaate toimingute laiendada neid väärtusi muuta.

## <a name="next-steps"></a>Järgmised sammud

- Kindla juurutamise tõrgete kõrvaldamise, lugege teemat [ressursid Azure'i Azure ressursihaldur juurutamisel levinud vigade lahendamine](resource-manager-common-deployment-errors.md).
- Muud tüüpi toimingute jälgimiseks auditilogide kasutamise kohta leiate teemast [auditi toimingud koos ressursihaldur](resource-group-audit.md).
- Kinnitage oma juurutuse enne selle käivitamisel, lugege teemat [Deploy ressursirühma Azure'i ressursihaldur malli abil](resource-group-template-deploy.md).
