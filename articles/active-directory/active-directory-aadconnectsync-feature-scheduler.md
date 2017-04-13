<properties
   pageTitle="Azure'i AD-ühendus sünkroonimine: Scheduler | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse sisseehitatud ajasti Azure'i AD-ühenduse sünkroonimise funktsiooni."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure'i AD-ühendus sünkroonimine: ajasti
Selles teemas kirjeldatakse sisseehitatud ajasti Azure'i AD-ühenduse sünkroonimise (alias sünkroonimine engine).

See funktsioon võeti kasutusele koostamine 1.1.105.0 (avaldatud veebruar 2016).

## <a name="overview"></a>Ülevaade
Azure'i AD-ühendus sünkroonimine sünkroonib kohapealse kataloogi abil ajastatud toimuvad muutused. Objekti/atribuudi sünkroonimine ja hooldustoiminguid on kaks ajasti protsessid, üks parooli sünkroonimise ja teine. See teema hõlmab viimase.

Varasemates versioonides objektide ja atribuutide ajasti oli välise sync engine ja Windowsi Toiminguajasti või eraldi Windowsi teenus kasutati käivitamiseks sünkroonimine. 1.1 väljalasete valmis, et sünkroonimine on ajasti mootori ja luba mõned kohandamine. Uue vaikimisi sünkroonimise sagedus on 30 minutit.

Kahe toimingu vastutab ajasti:

- **Sünkroonimise tsükli**. Importida, sünkroonimine ja eksportimine muudatuste protsess.
- **Hooldustoiminguid**. Pikendada, võtmed ja paroolide lähtestamine ja seadme registreerimise teenus (DRS) serdid. Likvideerite vana kirjed toimingute Logi sisse.

Ajasti ise alati töötab, kuid seda saab konfigureerida käitada ainult ühte või pole neid toiminguid. Näiteks kui peate oma sünkroonimine tsükli, saate keelata selle ülesande ajasti kuid siiski käivitada hooldustööd tööülesande.

## <a name="scheduler-configuration"></a>Ajasti konfigureerimine
Oma praeguste sätete konfigureerimine nägemiseks valige PowerShelli ja Käivita `Get-ADSyncScheduler`. See näitab teile umbes selline:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Kui näete **sünkroonimine käsu või cmdlet-käsk on pole saadaval** , kui käivitate selle cmdlet, siis PowerShelli moodul ei laadita. See võib juhtuda siis, kui käivitate Azure'i AD-ühenduse domeenikontrolleri või serveris, kus PowerShelli piirang kõrgem kui vaikesätteid. Kui näete selle vea, käivitage `Import-Module ADSync` cmdlet kättesaadavaks.

- **AllowedSyncCycleInterval**. Kõige sagedamini võimaldab Azure AD sünkroonimine ilmneda. Kui seda ei saa sünkroonida ja ikka ei toeta.
- **CurrentlyEffectiveSyncCycleInterval**. Ajakava praegu efekt. See on sama väärtuse, mis CustomizedSyncInterval (kui seadmine) kui see pole sagedamini kui AllowedSyncInterval. Kui muudate CustomizedSyncCycleInterval, see jõustub pärast järgmise domeenikirjeid.
- **CustomizedSyncCycleInterval**. Kui soovite ajasti käivitamiseks sageduse kui vaikimisi 30 minutiga, konfigureerige see säte. Pildi kohal ajasti on määratud käivituma iga tunni asemel. Kui see väärtus väiksem kui AllowedSyncInterval, siis kasutatakse.
- **NextSyncCyclePolicyType**. Delta või algne. Määratleb kui järgmisel peaks ainult protsessi delta muudatusi, või kui järgmisel peaks tegema täielik importimine ja sünkroonimine, mille ümber ka mis tahes uusi või muudetud reeglid.
- **NextSyncCycleStartTimeInUTC**. Järgmine kord, kui ajasti hakkavad järgmise sünkroonimise tsükli.
- **PurgeRunHistoryInterval**. Logi tuleb arvesse võtta aega. Neid saab vaadata üle sünkroonimise teenuse haldur. Vaikimisi on pidage 7 päevaks.
- **SyncCycleEnabled**. Näitab, kui ajasti töötab importimine, sünkroonimine ja ekspordi protsessid selle toimingu käigus.
- **MaintenanceEnabled**. Näitab, kas hoolduse käigus on lubatud. See värskendab sertifikaat/võti ja likvideerite toimingute Logi.
- **IsStagingModeEnabled**. Kuvatakse, kui [lavastus režiim](active-directory-aadconnectsync-operations.md#staging-mode) on lubatud.

Mõned nende sätete abil saate muuta `Set-ADSyncScheduler`. Saate muuta järgmisi:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

Azure AD talletatakse scheduler konfiguratsiooni. Kui teil on lavastus server, mõjutab muudatuste esmane serveris lavastus server (välja arvatud IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Süntaks:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - päeva, HH - tundi, mm - minut, ss - sekundid

Näide:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Muudab iga 3 tunni käivitamiseks ajasti.

Näide:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Muudab ajasti käivitamiseks iga päev.

## <a name="start-the-scheduler"></a>Ajasti käivitamine
Ajasti käivituvad vaikimisi iga 30 minuti järel. Mõnel juhul võite soovida sünkroonimine teate vahepeal ajastatud tsüklit või mõnda muud tüüpi käivitamiseks peate.

**Delta sünkroonimise tsükkeldiagramm**  
Delta sünkroonimise tsükli sisaldab järgmist:

- Delta importimise kohta kõik ühendused
- Delta sünkroonimise kohta kõik ühendused
- Ekspordi kõik konnektorid kohta

On võimalik, et teil on kiiresti muuta, mis tuleb sünkroonida kohe mistõttu tsükkel käsitsi käivitamiseks peate. Kui teil on vaja käsitsi käivitada tsükli, siis käivitage PowerShelli `Start-ADSyncSyncCycle -PolicyType Delta`.

**Täieliku sünkroonimise tsükkeldiagramm**  
Kui olete teinud üks järgmistest konfiguratsiooni muudatusi, peate käivitama täieliku sünkroonimise cycle (alias Esialgne):

- Lisatud veel objektide või atribuute importida allikas kataloog
- Tehtud muudatuste sünkroonimine reeglid
- Muudetud [filtreerimine](active-directory-aadconnectsync-configure-filtering.md) nii objektide arvu tuleks lisada

Kui olete ühte need muudatused teinud, siis peate käivitada täieliku sünkroonimise tsükli, nii et sync engine on võimalus reconsolidate konnektori tühikuid. Täieliku sünkroonimise tsükli sisaldab järgmist:

- Täielik Import kõik konnektorid
- Täieliku sünkroonimise kohta kõik konnektorid
- Ekspordi kõik konnektorid kohta

Tsükkel täieliku sünkroonimise käivitamiseks käivitage `Start-ADSyncSyncCycle -PolicyType Initial` PowerShelli käsuviibas kaudu. See käivitab täieliku sünkroonimise tsükli.

## <a name="stop-the-scheduler"></a>Ajasti peatamine
Kui ajasti töötab praegu domeenikirjeid peate selle peatada. Näiteks kui alustate installimise viisard ja saate selle tõrketeate:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Kui sünkroonimise tsükli töötab, ei saa muuta konfigureerimine. Võiks oodata, kuni ajasti on protsessi lõpule jõudnud, kuid te saate peatada selle võite teha muudatused kohe. Praegune tsükkel peatamine pole kahjulike ja endiselt ei töödelda muudatused töödeldud järgmise Käivita.

1. Alustuseks ütleb ajasti peatamiseks oma praeguse tsükli PowerShelli cmdleti `Stop-ADSyncSyncCycle`.
2. Peatamine ajasti lõpetab praeguse konnektor kaudu oma praegune ülesanne. Jõustamine konnektori lõpetada, tehke järgmised toimingud: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - Menüü start kaudu käivitada **Sychronization teenuse** . Avage **konnektorid**, tõstke esile konnektor **töötab** olek ja toimingud valige **Peata** .

Ajasti on endiselt aktiivne ja saab uuesti järgmist võimalust.

## <a name="custom-scheduler"></a>Kohandatud ajasti
Selles jaotises dokumenteerida cmdlet-käsud on ainult saadaval koostamine [1.1.130.0](active-directory-aadconnect-version-history.md#111300) ja uuemad versioonid.

Kui sisseehitatud ajasti ei vasta teie vajadustele, saate ajastada PowerShelli kaudu konnektorid.

### <a name="invoke-adsyncrunprofile"></a>Autonoomsest ADSyncRunProfile
Saate alustada profiili konnektori sel viisil:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Nimed, mida kasutada [konnektori nimede](active-directory-aadconnectsync-service-manager-ui-connectors.md) ja [Käivitage profiili nimed](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) leiate [Sünkroonimise Service Manager UI](active-directory-aadconnectsync-service-manager-ui.md).

![Autonoomsest Run profiili](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

Funktsiooni `Invoke-ADSyncRunProfile` cmdlet-käsk on sünkroonse, s.t see ei tagasta juhtelemendi kuni konnektor on lõpetanud toimingu edukalt või viga.

Kui te plaanite oma konnektorid, soovitus on plaanida need järgmises järjestuses:

1. (Täielikult/Delta) Kohapealse kataloogi, nt Active Directory importimine
2. (Täielikult/Delta) Azure AD importimine
3. (Täielikult/Delta) Kohapealse kataloogi, nt Active Directory sünkroonimine
4. (Täielikult/Delta) Azure AD sünkroonimine
5. Azure AD eksportimine
6. Kohapealse kataloogi, nt Active Directory eksportimine

Kui vaatate sisseehitatud ajasti, see on konnektorid käivituvad järjestuses.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Samuti saate jälgida sync engine kuvamiseks, kui see on hõivatud või jõudeolekus. Selle cmdlet tulemuseks tühja tulemi, kui sync engine on jõude ja konnektor ei tööta. Kui Connectori, tagastab see saatja nimi.

```
Get-ADSyncConnectorRunStatus
```

![Konnektor käivitada olek](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
Ülaltoodud joonisel on esimene rida olekust, kus sync engine on jõude. Teisel real kui Azure AD konnektor töötab kaudu.

## <a name="scheduler-and-installation-wizard"></a>Ajasti ja installimise viisard
Kui alustate viisardi installimist, siis ajasti ajutiselt peatatakse. Selle põhjuseks, siis eeldatakse, et teete konfiguratsioonimuudatuste ja neid ei saa rakendada, kui sync engine aktiivselt töötab. Seetõttu ei installi viisardi avatuks jätta kuna see katkestab sync engine mis tahes sünkroonimise toiminguid teha.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedi ja Azure Active Directory integreerimine](active-directory-aadconnect.md).
