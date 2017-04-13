<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: sünkroonimise Service Manager UI | Microsoft Azure'i"
    description="Vahekaardil toimingud rakenduses sünkroonimise Service Manager for Azure'i AD-ühenduse mõista."
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

![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Vahekaardil toimingud näitab Viimati tehtud toimingute tulemusi. Sellel vahekaardil on kuupäevatabelid ja nende probleemide tõrkeotsing.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Mõista teavet, mis on nähtav, klõpsake vahekaarti toimingud
Ülaosas kuvatakse kõik töötab kroonilise järjestuses. Vaikimisi toimingute Logi hoiab seitsme viimase päeva kohta teavet, kuid [Scheduleri](active-directory-aadconnectsync-feature-scheduler.md)abil saate seda sätet muuta. Mida soovite otsida mis tahes Käivita, mis näitavad õnnestumise olek. Saate muuta, klõpsates päiste sortimine.

Veerus **olek** on kõige olulisem teave ja kuvab kõige probleemi jooksma. Siit leiate lühikese kokkuvõtte kõige levinum olekutest uurida tähtsuse järjekorras (kus * mitu võimalikku tõrkestringid tähistamiseks).

Olek | Kommentaari
--- | ---
peatatud-* | Käivita ei saanud lõpule viia. Näiteks kui kaugsüsteem on alla ja ei saa ühendust.
peatatud tõrge limiit | On rohkem kui 5000 tõrkeid. Suure hulga tõrgete tõttu automaatselt peatatud Käivita.
valmis –\*-tõrked | Käivita lõpetatud, kuid on vigu (vähem kui 5000), mida tuleks uurida.
valmis –\*-hoiatused | Käivita lõpetatud, kuid mõned andmed pole oodatud olekus. Kui teil on tõrkeid, siis see sõnum on tavaliselt ainult sümptom. Seni, kuni on adresseeritud vigu, mida peaks uurima, hoiatused.
edu | Pole probleeme.

Rea valimisel värskendatakse all töötavad üksikasjade kuvamiseks. Vasakule alumise, võib olla räägivad **samm #**loendi. Selles loendis kuvatakse ainult siis, kui teil on mitu domeeni oma mets, kus iga domeeni esindab etapi. Domeeninime leiate **sektsiooni**pealkirja all. Jaotises **Sünkroonimise statistika**, leiate lisateavet töödeldud muutuste arv. Võite klõpsata linke muudetud objektide loendi toomiseks. Kui teil on vigadega objektid, nende vigade kuvata **Sünkroonimistõrgete**all.

## <a name="troubleshoot-errors-in-operations-tab"></a>Klõpsake vahekaarti toimingud tõrkeotsing
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Kui teil on tõrkeid, objekti tõrge ja tõrke olemus on lingid, mis annab Lisateavet.

Käivitamiseks klõpsake nuppu tõrge string (**Sünkrooni-reegel-tõrge – funktsioon-käivitub** pildil). Esmalt on esitatud ülevaade objekti. Tegelik viga kuvamiseks klõpsake nuppu **Virnas jälgi**. Jälita seda kirjeldatakse silumine taseme viga.

**Näpunäide:** Saate paremklõpsake väljale **kõne virnas teave** , nuppu **Vali kõik**ja **Kopeeri**. Saate kopeerida virnas ja vaadake oma lemmik redaktoris, nt Notepadis viga.

- Kui tõrke on **SyncRulesEngine**kaudu, siis kõne virnas teabe esmalt on kõik atribuutide loendi objekti. Liikuge kerides allapoole, kuni näete pealkirja **InnerException = >**.  
![Sünkroonimine Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
Pärast rea tõrketeadet. Ülaltoodud joonisel on loodud kohandatud sünkroonimine reegel Fabrikam viga.

Kui viga ise ei anna piisavalt teavet, siis on aeg vaadata andmed ise. Saate klõpsata linki objekti identifikaator ja [järgige objekti ja selle andmete süsteemi kaudu](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
