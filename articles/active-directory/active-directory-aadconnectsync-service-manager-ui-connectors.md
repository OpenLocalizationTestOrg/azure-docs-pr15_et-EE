<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: sünkroonimise Service Manager UI | Microsoft Azure'i"
    description="Mõista konnektorid menüü rakenduses sünkroonimise Service Manager for Azure'i AD-ühenduse."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure'i AD-ühendus sünkroonimine: sünkroonimise Service Manager

[Toimingud](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Konnektorid](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaversumi kujundaja](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaversumi otsing](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Menüü konnektorite abil saate hallata kõik süsteemid sync engine on ühendatud.

## <a name="connector-actions"></a>Konnektor toimingud

Toiming | Kommentaari
--- | ---
Loomine | Ärge kasutage. Ühenduse täiendavate AD metsade, kasutage installimise viisard.
Atribuudid | Kasutada domeeni ja OU filtreerimine.
[Kustutamine](#delete) | Kas konnektori ruumis andmete kustutamiseks või kustutada ühenduse mets kasutada.
[Käivita profiilid konfigureerimine](#configure-run-profiles) | Välja arvatud domeeni filtreerimine, pole vaja midagi siin konfigureerimine. Selle toimingu abil saate vaadata juba konfigureeritud Käivita profiilid.
Käivita | Ühekordne Käivita profiili alustamiseks kasutada.
Stopp | Lõpetab praegu töötavate profiili konnektor.
Ekspordi konnektor | Ärge kasutage.
Impordi konnektor | Ärge kasutage.
Värskendage konnektor | Ärge kasutage.
Skeemi värskendamine | Värskendab vahemällu talletatud skeemi. On eelistatud suvandi installiviisardis selle asemel kasutada, kuna see ka värskendusi sünkroonida reeglid.
[Otsingu konnektori ruumi](#search-connector-space) | Objektide leidmiseks ja jälgimiseks [objekti ja selle andmete süsteemi kaudu](#follow-an-object-and-its-data-through-the-system).

### <a name="delete"></a>Kustutamine
Kustutustoiming kasutatakse kaks erinevat asja.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

Suvand **Kustuta konnektori ruumi ainult** eemaldatakse kõik andmed, kuid säilitada konfiguratsiooni.

Suvand **Kustuta konnektor ja konnektori ruumi** eemaldab andmed ja konfiguratsiooni. See suvand kasutatakse, kui te ei soovi enam mets ühenduse loomiseks.

Mõlema variandi kõigi objektide sünkroonimiseks ja metaversumi objektide värskendada. See toiming on toiming.

### <a name="configure-run-profiles"></a>Käivita profiilid konfigureerimine
See suvand võimaldab näha konnektori konfigureeritud Käivita profiilid.

![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Otsingu konnektori ruumi
Otsingu konnektori ruumi toiming on kasulik objektide otsimine ja andmete tõrkeotsing.

![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Alustuseks valige **ulatust**. Saate otsida andmete põhjal (RDN DN ankur, sub puu) või objekti (kõik muud suvandid).  
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Kui te ei tee näiteks sub puu otsingu, saate ühe OU kõigi objektide.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) selle koordinaatvõrgu, saate valida soovitud objekti, valige **Atribuudid**ja [järgige selle](#follow-an-object-and-its-data-through-the-system) andmeallika konnektori ruumi – metaversumi ja target konnektori ruumi.

## <a name="follow-an-object-and-its-data-through-the-system"></a>Järgige objekti ja selle andmete süsteemi kaudu
Andmete probleem tõrkeotsingul järgige objekti allika konnektori ruumi, metaversumisse ja sihtkohas konnektori ruumi on klahv protseduuri mõista, miks andmeid ei saa oodatud väärtusega.

### <a name="connector-space-object-properties"></a>Konnektori ruumi objekti atribuudid
**Importimine**  
Kui avate cs objekti, on mitu vahekaarti ülaosas. Mis on etapiviisilise pärast impordi andmed kuvatakse **importimine** vahekaardil.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) **Vana väärtus** näitab, mida praegu on talletatud süsteem ja **Uus väärtus** , mis on saadud allika süsteem ja pole veel seotud. Sel juhul, kuna sünkroonimise tõrge, ei saa muudatuse rakendada.

**Tõrge**  
Lehe tõrge kuvatakse ainult juhul, kui on probleem objekti. Üksikasju vt toimingute lehele Lisateavet selle kohta, kuidas [Sünkroonimistõrgete tõrkeotsing](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab).

**Pärandi**  
Vahekaart pärandi näitab, kuidas konnektori ruumi objekt on seotud metaversumi objekti. Näete, kui konnektor viimase muudatuse ühendatud süsteem ja imporditud reeglid, mis rakendatakse metaversumis andmetega asustada.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) veerus **toiming** näete on üks **Sissetulev** sünkroonimine reegel toimingu **säte**. See näitab, et kui see konnektori ruumi objekt on olemas, jääb metaversumi objekti. Kui selle asemel kuvatakse sünkroonimise reeglite loendi sünkroonimine reegli suunas **Väljaminev** ja **sätte**, see näitab, et selle objekti metaversumi objekti kustutamisel kustutatakse.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) saate vaadata ka **PasswordSync** veerus sissetuleva meili konnektori ruumi aitab muudatuste parooli, kuna üks sünkroonimine reegel on **tõene**väärtus. See parool, seejärel saadetakse Azure AD Väljamineva reegli abil.

Klõpsake menüüs pärandi saate avada metaversumi, klõpsates [Metaversumi objekti atribuudid](#metaverse-object-properties).

Kõigi vahekaartide allosas on kaks nuppu: **Eelvaade** ja **logige**.

**Eelvaade**  
Üks ühele objektile sünkroonimiseks kasutatakse eelvaate lehele. See on kasulik, kui teil on mõned reeglid selle sünkroonimine klientide tõrkeotsingu ja soovite vaadata muudatuste mõju ühe objekti. Saate valida **Täieliku sünkroonimise** ja **Delta sünkroonimise**vahel. Samuti võite märkida ruudu vahel **Luua eelvaade**, mis ainult hoiab muutmine mällu, ja **Kinnita eelvaade**, millised etapid kõik muudatused target konnektor tühikuid.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) saab kontrollida, objekti- ja millist reeglit rakendada teatud atribuudi voog jaoks.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Log**  
Leht logifailide kasutatakse parooli sünkroonimise olek ja ajalugu, lisateabe saamiseks vaadake [tõrkeotsing parooli sünkroonimise](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) kuvamiseks.

### <a name="metaverse-object-properties"></a>Metaversumi objekti atribuudid
**Atribuudid**  
Vahekaardil atribuudid näete väärtused ja millised konnektor osa moodustab see.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**konnektorid**  
Ühendused vahekaardil kuvatakse kõik konnektor tühikud, mis on esindatud objekti.
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) sellel vahekaardil ka abil saate liikuda [konnektori ruumi objekti](#connector-space-object-properties).

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
