<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: kuidas muuta vaikimisi konfiguratsiooni | Microsoft Azure'i"
    description="Kuidas muuta sünkroonimine Azure'i AD-ühenduse konfiguratsiooni tutvustatakse."
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure'i AD-ühendus sünkroonimine: kuidas muuta vaikimisi konfigureerimine
Selles teemas eesmärk teid juhendaks vaikimisi sünkroonimine Azure'i AD-ühenduse konfiguratsiooni muudatuste tegemiseks. See antakse juhiseid mõne levinud stsenaariumi. Teadmised, peaks oskama ise konfiguratsiooni vastavalt oma ettevõtte reeglite lihtsa muudatusi teha.

## <a name="synchronization-rules-editor"></a>Sünkroonimise reeglid redaktor
Sünkroonimise reeglite redaktori kasutatakse näha ja muuta vaikimisi konfiguratsiooni. Leiate **Azure'i AD-ühenduse** rühmas menüüs Start.  
![Menüü Start sünkroonimine reegel redaktor](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Kui avate, kuvatakse vaikimisi out box reeglitest.

![Sünkroonimine redaktoris](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Redaktoris liikumine
Redaktori ülaosas rippvalikute võimaldavad kiiresti leida kindla reegli. Näiteks kui soovite näha reegleid, kus atribuudis proxyAddresses on kaasatud, soovite muuta rippvalikute on järgmine:  
![SRE filtreerimine](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Lähtestada, filtreerimine ja värske konfiguratsiooni laadimine, vajutage klaviatuuril **klahvi F5** .

Peate paremas ülanurgas nuppu **Lisa uus reegel**. Kohandatud reegli loomiseks kasutatakse seda nuppu.

Allosas teil on nupud tegutsemiseks valitud sünkroonimine reegel. **Redigeerimine** ja **kustutamine** teha, mida oodata neid. **Ekspordi** toodab PowerShelli skripti taasloomine sünkroonimine reegel. See toiming võimaldab sünkroonimine reegel üks server teise teisaldada.

## <a name="create-your-first-custom-rule"></a>Kohandatud esimese reegli loomine
Kõige levinum muudatus on atribuut puhul muudatustest. Andmete allikas kataloogis ei pruugi nagu Azure AD. Selles jaotises näites, mida soovite veenduge, et kasutaja eesnimi on alati **algsuurtähti**.

### <a name="disable-the-scheduler"></a>Ajasti
[Ajasti](active-directory-aadconnectsync-feature-scheduler.md) käivitatakse vaikimisi iga 30 minuti järel. Kui soovite veenduda, et see on hakanud ning samal ajal muudatuste tegemist on uue reeglite tõrkeotsing. Keelake ajutiselt ajasti, käivitage PowerShelli ja käivitamine`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Ajasti](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Reegli loomine

1. Klõpsake nuppu **Lisa uus reegel**.
2. Sisestage **Kirjeldus** lehe järgmist:  
![Sissetulev reegel filtreerimine](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Nimi: Tippige reegli kirjeldav nimi.
    - Kirjeldus: Mõningaid selgitusi nii, et keegi teine saavad aru, mida reegel on.
    - Ühendatud süsteemi: objekti leiate süsteem. Selles näites me valige Active Directory konnektor.
    - Ühendatud süsteemi/metaversumi objekti tüüp: Valige **kasutaja** - ja **isik** .
    - Lingi tüüp: Sisestage väärtuseks **liituda**.
    - Järjestuse: Sisestage väärtus, mis on süsteemis kordumatu. Väiksem arvuline väärtus näitab kõrgem.
    - Sildi: Jätke tühjaks. Ainult out box reeglid Microsofti peaks olema see ruut, mille väärtus on täidetud.
3. Sisestage lehel **KMH ulatuse määramise filtri** **givenName ISNOTNULL**.  
![Sissetulev reegel filter ulatuse määramine](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
Selles jaotises on abil saate määratleda, mis objektide reeglit tuleks rakendada. Kui tühjaks, soovite reeglit rakendada kõik kasutaja objektid. Kuid mis sisaldab konverentsiruumi, kontod ja muude objektide-inimesed kasutaja.
4. Klõpsake **liitumine reeglid**jätke tühjaks.
5. Lehel **teisendused** muuta soovitud FlowType **avaldis**. Valige Target atribuut **givenName**ja sisestage väljale Allikas `PCase([givenName])`.
![Sissetulev reegel teisendused](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
Sync engine on tõstutundlikud nii funktsiooni nime atribuudi nimi. Kui tipite midagi valesti, kuvatakse hoiatus, kui lisate reegli. Redaktori võimaldab salvestada ja jätkata, nii, et teil uuesti reeglit ja parandamise reegel.
6. Klõpsake nuppu **Lisa** reegel salvestada.

Uue kohandatud reegli peaks olema süsteemi nähtav ka muid sünkroonimine reegleid.

### <a name="verify-the-change"></a>Muudatuse kinnitamine
Selle uue muudatuse, mida soovite veenduge, et see ei tööta oodatud viisil ja on korrutamine vigu. Teil on objektide arvu sõltuvalt on selles etapis tuleb teha kaks võimalust.

1. Kõigi objektide täieliku sünkroonimise käivitamine
2. Ühele objektile eelvaade ja täieliku sünkroonimise käivitada

Menüü start kaudu **Sünkroonimise teenuse** käivitamine. Kõik selles tööriistas on selles jaotises toodud juhised.

1. **Kõigi objektide täieliku sünkroonimise**  
Valige **ühendused** ülaosas. Tehtud muudatuse eelmises jaotises, praegusel juhul Active Directory domeeniteenused konnektor tuvastamine ja valige see. Valige **Käivita** toimingud ja valige **Täieliku sünkroonimise** ja **OK**.
![Täieliku sünkroonimise](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
Objektide värskendatud metaversumi. Soovite kohe vaadata metaversumi objekti.

2. **Eelvaate ja täieliku sünkroonimise ühe objekti**  
Valige **ühendused** ülaosas. Tehtud muudatuse eelmises jaotises, praegusel juhul Active Directory domeeniteenused konnektor tuvastada ja valige see. Valige **Otsi konnektori ruumi**. Ulatuse abil saate otsida objekti, mida soovite kasutada testimiseks muutmine. Valige objekt ja klõpsake nuppu **Eelvaade**. Valige kuval uue, **Kinnita eelvaade**.
![Kinnita eelvaade](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
Nüüd soovib metaversumi muutmine.

**Vaadake metaversumi objekti**  
Nüüd soovite valida mõne valimi objektide veendumaks, et väärtus on oodata ja et reegel rakendatud. Valige ülevalt **Metaversumi otsing** . Lisage mõni oluline objektide leidmiseks filter. Otsingutulemite, avage objekti. Vaatame atribuudiväärtused ja ka kontrollida veerus **Sünkroonimine reegleid** , et reegel rakendatud ootuspäraselt.  
![Metaversumi otsing](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>Ajasti lubamine
Kui kõik on oodatud viisil, saate ajasti uuesti lubada. Käivitage PowerShelli kaudu `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Muud levinud atribuut meilivoo muudatused
Eelmises jaotises kirjeldatud, kuidas muuta mõne atribuudi voog. Selles jaotises on olemas täiendavaid näited. Sünkroonimise reegli loomise juhised on lühendeid, kuid täielik juhiseid leiate eelmisest jaotisest.

### <a name="use-another-attribute-than-the-default"></a>Kui vaikimisi teise atribuudi kasutamine
Fabrikam on mets kohaliku tähestiku kasutuskoha eesnimi, perekonnanimi ja kuvatav nimi. Neid atribuute Ladina Märgi kujutis leiate laiend atribuute. Kui hoone globaalse aadressiloendi Azure AD ja Office 365 organisatsiooni soovib selle asemel kasutada järgmisi atribuute.

Vaikimisi konfiguratsiooni, kohaliku mets objekti näeb välja selline:  
![Atribuudi voog 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Muude atribuutide alusel reegli loomiseks tehke järgmist.

- Käivitage **Sünkroonimine redaktoris** menüü start kaudu.
- **Sissetulev** vasakule endiselt valitud, klõpsake nuppu **Lisa uus reegel**.
- Pange selle reegli nimi ja kirjeldus. Valige kohapealse Active Directory ja objekti tüübid.  **Lingi tüüp**, valige **Liitu**. Valige järjestuse, arv, mida kasutatakse teise reegli järgi. Out box reeglitest alustada 100, nii väärtus 50 saab kasutada järgmises näites.
![Atribuudi voog 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Ulatus tühjaks jätta (ehk teisisõnu öeldes tuleks rakendada kõigile kasutaja objektidele mets).
- Liitu reeglid tühjaks jätta (st lase out box reegli kõik ühendused toime).
- Looge teisendused, järgmised puhul.  
![Atribuudi voog 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Klõpsake nuppu **Lisa** reegel salvestada.
- Minge **Sünkroonimise Service Manager**. Valige **konnektorid**, mille lisasime selle reegli konnektor. Valige **Käivita**ja **täieliku sünkroonimise**. Täieliku sünkroonimise ümberarvutuse toimumishetke kõigil objektidel praeguse reeglite abil.

See on tulemus sama objekti koos kohandatud reegli:  
![Atribuudi voog 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Pikkus atribuudid
Stringi atribuudid on vaikimisi määratud registreerida ja maksimumpikkus on 448 märgid. Kui töötate stringi atribuute, mis võib sisaldada rohkem, siis veenduge, et atribuudi voog on järgmised:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>Funktsiooni userPrincipalSuffix muutmine
Atribuudi userPrincipalName Active Directory kasutajad alati pole teada ja ei pruugi olla sobiv sisselogimise ID Näiteks Azure AD ühenduse sünkroonimine installimise viisardi abil muu atribuut, valides meil. Kuid mõnel juhul tuleb arvutada atribuuti. Näiteks Contoso ettevõttel on kaks Azure AD kataloogide, tootmise ja testimiseks. Soovidega arvestamine kasutajate oma testi rentniku sisselogimise ID kasutamine teise järelliide  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

See avaldis teha kõike vasakule esimese @-sign (Word) ja concatenate määratud stringiga.

### <a name="convert-a-multi-value-to-a-single-value"></a>Mitme väärtusega teisendamine ühe väärtuse
Teatud atribuutide Active Directory on mitme väärtusega skeemi isegi juhul, kui nad näevad ühe väärtustatud Active Directory kasutajad ja arvutid. Näide on atribuut kirjeldus.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

See avaldis juhuks, kui atribuut on väärtus, me võtta esimese üksuse (kirje) atribuut, eemaldada ja lõputühikud (Trim) ja hoidke stringi 448 esimest märki (vasakul).

### <a name="do-not-flow-an-attribute"></a>Voog atribuut
Klõpsake jaotises Selle stsenaariumi leiate [juhtelemendi atribuut meilivoo protsess](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

On kaks võimalust liiguks atribuut. Esimene on saadaval installiviisardis ja võimaldab teil [valitud atribuudid](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)eemaldada. See suvand töötab, kui teil on sünkroonitud kunagi enne atribuuti. Siiski alustamist sünkroonimiseks atribuudi ja selle funktsiooni hiljem eemaldada, siis sünkroonimine engine astmed atribuuti ja olemasolevate väärtuste haldamine jäetakse Azure AD.

Kui soovite eemaldada atribuudi väärtus ja veenduge, et see flow tulevikus, peate selle asemel kohandatud reegli loomine.

Veebisaidil Fabrikam me aru, et mõned me sünkroonimine pilveteenusesse atribuute ei peaks olema seal. Soovime veendumaks, et neid atribuute eemaldatakse Azure AD.  
![Vigane faililaiend atribuudid](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Sissetuleva sünkroonimise uut reeglit luua ja asustamiseks kirjeldus ![kirjeldused](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Atribuut puhul tippige **avaldis** ja allikas **AuthoritativeNull**luua. Sõnasõnaline **AuthoritativeNull** näitab, et väärtus peab olema tühi MV isegi juhul, kui alumises järjestuse sünkroonimise reegli alusel proovitakse asustamiseks väärtus.
![Teisenduse laiend atribuudid.](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- Salvestage sünkroonimine reegel. **Sünkroonimise teenuse**käivitamine, leida konnektor, valige **Käivita**ja **Täieliku sünkroonimise**. Selles etapis tuleb ümberarvutuse toimumishetke kõik atribuudi puhul.
- Veenduge, et soovitud muudatused on umbes eksportimiseks konnektori ruumi otsingu kaudu.
![Kustuta etapiviisiline](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet konfiguratsiooni mudeli [Mõistmine deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning.md)kohta.
- Lugege lisateavet avaldis keele avaldistes [mõistmine deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)kohta.

**Teemade ülevaade**

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
